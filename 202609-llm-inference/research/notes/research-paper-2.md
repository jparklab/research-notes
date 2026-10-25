---
title: Research Paper
id: research-paper-2
tags:
- onprem-llm-inference-stacks-859099
- derivative-of
created: '2026-10-23T18:13:39.253895Z'
updated: '2026-10-23T18:17:22.482192Z'
source: https://arxiv.org/pdf/2407.00110v2
source_domain: arxiv.org
fetched_at: '2026-10-23T18:13:39.253631Z'
fetch_provider: crawl4ai
status: deprecated
type: note
tier: institutional
content_type: paper
deprecated: false
raw_file: raw/research-paper-2.pdf
doi: arXiv:2407.00110v2
---

Research Paper
Chat AI: A Seamless Slurm-Native
Solution for HPC-Based Services
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
GWDG
Georg-August-Universität Göttingen
Institute of Computer Science
August 5, 2024
arXiv:2407.00110v2  [cs.DC]  2 Aug 2024


---

Abstract
The widespread adoption of large language models (LLMs) has created a pressing need
for an efficient, secure and private serving infrastructure, which allows researchers to run
open source or custom fine-tuned LLMs and ensures users that their data remains private
and is not stored without their consent. While high-performance computing (HPC) sys-
tems equipped with state-of-the-art GPUs are well-suited for training LLMs, their batch
scheduling paradigm is not designed to support real-time serving of AI applications. Cloud
systems, on the other hand, are well suited for web services but commonly lack access
to the computational power of HPC clusters, especially expensive and scarce high-end
GPUs, which are required for optimal inference speed. We propose an architecture with
an implementation consisting of a web service that runs on a cloud VM with secure access
to a scalable backend running a multitude of LLM models on HPC systems. By offering a
web service using our HPC infrastructure to host LLMs, we leverage the trusted environ-
ment of local universities and research centers to offer a private and secure alternative to
commercial LLM services. Our solution natively integrates with the HPC batch scheduler
Slurm, enabling seamless deployment on HPC clusters, and is able to run side by side
with regular Slurm workloads, while utilizing gaps in the schedule created by Slurm. In
order to ensure the security of the HPC system, we use the SSH ForceCommand directive
to construct a robust circuit breaker, which prevents successful attacks on the web-facing
server from affecting the cluster. We have successfully deployed our system as a produc-
tion service, and made the source code available at https://github.com/gwdg/chat-ai
i


---

Contents
List of Tables
iii
List of Figures
iii
1
Introduction
1
2
Background
2
3
Related Work
3
4
Challenges
5
5
Chat AI Architecture
7
5.1
Authentication
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
8
5.2
API Gateway . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
8
5.3
Web App
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
9
5.4
HPC Proxy
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
10
5.5
Cloud Interface Script
. . . . . . . . . . . . . . . . . . . . . . . . . . . . .
10
5.6
Scheduler
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
11
5.7
LLM Server . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
12
5.8
External Proxy . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
12
5.9
Monitoring . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
12
6
Evaluation
12
6.1
Security . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
12
6.1.1
Security of the Web Interface
. . . . . . . . . . . . . . . . . . . . .
13
6.1.2
Security of the HPC System . . . . . . . . . . . . . . . . . . . . . .
13
6.2
Data Privacy
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
13
6.3
Performance . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
14
6.3.1
Latency . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
14
6.3.2
Throughput . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
15
6.4
User Adoption . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
16
7
Discussion
17
7.1
Limitations
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
18
7.1.1
Reliability . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
19
7.1.2
Automation . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
19
7.1.3
Scale to zero . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
19
7.1.4
Encryption
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
20
7.1.5
Throughput . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
20
7.2
Future Work . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
21
8
Conclusion
21
9
Code & Acknowledgements
21
References
23
ii


---

List of Tables
1
Latency measurements from the ESX machine. . . . . . . . . . . . . . . . .
14
2
Throughput results for a regular user request.
. . . . . . . . . . . . . . . .
15
List of Figures
1
Architecture of Chat AI. . . . . . . . . . . . . . . . . . . . . . . . . . . . .
8
2
Chat AI App. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
9
3
Total number of distinct users. . . . . . . . . . . . . . . . . . . . . . . . . .
16
4
Daily Chat AI users. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
17
5
Total inference requests per day. . . . . . . . . . . . . . . . . . . . . . . . .
18
iii


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
1
Introduction
Recent advances in large language models (LLMs) [Bro+20; Ope+24; Cho+22] have
sparked an adoption of AI systems in a wide range of applications1 [Ope+24]. This puts
many institutions and companies in the difficult position of having to come up with a
strategy to adopt these new technologies and deciding to what extent their data can
be handled by (potentially foreign) companies such as OpenAI and Google [Ope+24;
Cho+22] or alternatively attempt to self-host an LLM service in order to preserve the
privacy of user data [Seb23; Ksh23].
High-quality LLM models have recently become widely available due to many compa-
nies and institutions providing free access to their model weights, commonly via Hugging
Face2. Notable recent releases include Llama3.1 via Meta3, Qwen2 via Alibaba Cloud [24f]
and Mixtral via Mistral AI [Jia+23].
Using a runtime such as llama.cpp4, vLLM [Kwo+23] or Text-Generation-Inference5
one can run smaller versions of these models, e.g., 7B or 8B parameters, on high-end
consumer GPUs with 10 to 20 GB of VRAM. However, for larger models such as Llama3.1-
70B6 and Qwen2-72B [24f] that more closely rival the performance of state-of-the-art
closed source models such as OpenAI’s ChatGPT4 [Ope+24], significant investment into
GPU resources is required.
In our testing we found that a single instance of Meta’s LLama3.1-70B model fits
into the VRAM of two of NVIDIA’s H100 GPUs when using FP8 quantization [WYH23;
Mic+22] via vLLM 0.5.0 or even a single H100 GPU when significantly reducing the
context size. While this allows effective inference of LLama3.1-70B and models of similar
size, it limits access to private inference of these models to those who can afford H100
GPUs or similarly powerful hardware.
Many research centers, universities and institutions either operate their own HPC
infrastructure or have access to HPC infrastructure via a research network7. Typically,
HPC centers operate a batch scheduler such as Slurm [YJG03] and put strong emphasis
on the security of their systems.
HPC infrastructure is an attractive target for attackers to steal research data, interrupt
operability or hijack compute resources for crypto mining. To ensure system security, on
HPC multi-user systems, admins often rely on battle-tested security mechanisms such
as POSIX permissions and SSH for access management. This significantly restricts the
1Council Post. url: https://www.forbes.com/sites/forbestechcouncil/2023/04/25/the-
generative-ai-frontier-mastering-llm-adoption-for-ceos-and-ctos-to-drive-business-
success/ (visited on 06/10/2024).
2Hugging Face – The AI Community Building the Future. url: https://huggingface.co/ (visited
on 06/14/2024).
3The Llama 3 Herd of Models | Research - AI at Meta. url: https://ai.meta.com/research/
publications/the-llama-3-herd-of-models/ (visited on 08/02/2024).
4Ggerganov/Llama.Cpp: LLM Inference in C/C++. url: https://github.com/ggerganov/llama.
cpp (visited on 06/14/2024).
5Huggingface/Text-Generation-Inference. url:
https : / / github . com / huggingface / text -
generation-inference (visited on 06/14/2024).
6Meta-Llama/Meta-Llama-3.1-70B-Instruct · Hugging Face. url: https://huggingface.co/meta-
llama/Meta-Llama-3.1-70B-Instruct (visited on 08/02/2024).
7Our Members | NHR-Verein: Wir Sind Hochleistungsrechnen. url: https://www.nhr-verein.de/
en/our-members (visited on 06/14/2024); Members. url: https://prace-ri.eu/about/members/
(visited on 06/14/2024).
Section 1
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
1


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
possibility of using HPC resources to expose services without compromising these security
frameworks.
Furthermore, a batch scheduler such as Slurm requires users to submit jobs, which it
then schedules at a suitable time to one or more compute nodes depending on the expected
job duration, resource demands and availability of resources. Considering that a user of
an interactive HPC service, e.g., chatting with an LLM, would expect their request to be
processed immediately without waiting for a Slurm job to be scheduled and potentially
waiting on other jobs, the batch scheduling paradigm is not suitable for offering services.
In this paper, we present an entirely Slurm-native solution that seamlessly integrates
with existing HPC infrastructures to offer a demanding service such as real-time conver-
sations with LLMs with minimal effort. Moreover, as it is Slurm-native, it allows for
the effective utilization of HPC resources while not permanently blocking resources for
other Slurm jobs. Using this method, we released Chat AI8, a pioneering web service that
offers rapid real-time access to state-of-the-art open source LLMs hosted on our existing
HPC infrastructure. This solution addresses the need for Slurm-based services, private
inference via local or associated HPC centers and securing said services using SSH-based
access patterns that comply with many existing HPC security frameworks. We share our
findings and explore the viability of hosting a service with the proposed architecture with
regards to security, performance, and user adoption.
This paper is structured as follows: Section 2 provides the necessary background
information to understand the rest of the paper. Section 3 places our work in the context
of existing publications and explores similar approaches. The significant challenges we
faced during the design and implementation of our solution are discussed in Section 4. Our
architecture is then presented in detail in Section 5. Next, we evaluate our service in terms
of security and performance in Section 6, and discuss its adoption by users and institutions,
as well as its limitations and future prospects in Section 7. Finally, we summarize our
contributions in Section 8 and provide our source code and acknowledgements in Section 9.
2
Background
In this section we describe the existing tools and paradigms that are relevant to our
approach including
• the HPC batch scheduler Slurm,
• the scheduling paradigms for batch and service scheduling,
• the open source LLM runtime vLLM,
• the API gateway Kong and
• some notes on security aspects for HPC infrastructure.
Slurm
The Simple Linux Utility for Resource Management, in short Slurm [YJG03], is
a workload or resource manager for HPC clusters. It offers a solution for running batch
jobs on a cluster through the use of shell scripts. Slurm assigns jobs to nodes depending
on the availability of the current nodes and requirements of the job, ensuring that the
resources of the cluster are utilized efficiently.
8Chat AI. url: https://chat-ai.academiccloud.de/chat (visited on 08/01/2024).
Section 2
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
2


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
Scheduling Paradigm
We consider two major scheduling paradigms, batch scheduling,
also called gang scheduling, and service scheduling. In batch scheduling, when assigning
a job that requests a number of resources such as multiple nodes, a batch scheduler such
as Slurm will only assign and start a job when all resources are available at once for the
requested duration. While this is desirable for a compute job such as an MPI job that can
only start when all nodes are available to use batch scheduling, it is a different case for
services. For a service that is supposed to serve user requests, multiple instances might
be required to sufficiently serve all requests but if not enough resources are available, the
service could still operate with reduced performance and dynamically add instances when
more resources become available. For one-time jobs that are not time-sensitive, Slurm
is a suitable choice, however, one would have to adapt it in order to support a service
paradigm.
vLLM
vLLM [Kwo+23] is a state-of-the-art, open source LLM runtime implementa-
tion. It uses an attention mechanism inspired by memory paging techniques in operating
systems, achieving near-zero waste in key-value cache memory. Since its first release in
June 20239 vLLM has kept up with many developments and optimizations for LLM infer-
ence techniques with the latest version, as of writing, v0.5.0.post1, having been released
on Github in June 2024. vLLM supports modern LLM architectures such as Llama and
Qwen and implements an OpenAI-compatible API, such that it is a drop-in replacement
for most applications with ChatGPT integration.
Kong
Kong10 is an API gateway that supports custom plugins, complex routing, user
management and monitoring tools. It consists of an open source edition, Kong OSS11 and
an enterprise edition with additional features. Here we focus on the open source edition,
which supports creating API routes and keys, implements native user management and
provides features such as rate-limiting and restricting access for certain routes and users.
Security Aspect
Security is a major concern when thinking about hosting a service
that is exposed to the internet on an HPC cluster as this opens a much wider attack surface
for attackers compared to regular access patterns via SSH [Bul+18]. If an attacker should
be able to break into the system, they would be able to steal potentially sensitive research
data, perform Distributed Denial of Services (DDoS) attacks on other systems [Bür+12a]
or run crypto-miners on the HPC hardware [PGE23]. To protect themselves, HPC cen-
ters commonly employ strict security regulations such as disallowing users access to root
permissions on compute and login nodes, limiting the allowed access protocols, e.g., only
SSH, and restricting compute nodes to not have direct internet access with only cluster
login nodes being reachable from the internet.
3
Related Work
Providing access to LLMs is in itself a service offering, and running such services in a
production environment is commonly associated with existing frameworks such as Ku-
9Vllm-Project/Vllm. url: https://github.com/vllm-project/vllm (visited on 06/24/2024).
10Become an API-first Company | Kong. url: https://konghq.com (visited on 06/25/2024).
11Kong/Kong. url: https://github.com/Kong/kong (visited on 06/25/2024).
Section 3
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
3


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
bernetes [Liu23]. However, most HPC infrastructures rely on batch scheduling systems
such as Slurm, Torque or HTCondor [Liu23; Hil+20] as their main resource manager.
Moreover, modern LLMs require so much compute power not only for training but also
for inference [Wan+24; Wan+21] that an inference service would benefit from access to
powerful hardware such as high-end GPUs installed in HPC infrastructure.
In order to use these HPC resources, various approaches exist.
• Zhang et al. [Zha+19] proposed a unified framework for Apache Hadoop YARN12
and Slurm to create a mobile AI service platform.
• Wan et al. [Wan+23a] proposed a Slurm-based service platform to optimize startup
times and storage usage for multi-user applications on HPC infrastructure.
• Shainer et al. [Sha+22] proposed a service architecture with a focus on supporting
specialized HPC hardware such as DPUs and Infiniband.
• Zhou et al. [Zho+21] proposed an architecture that utilizes Torque to run compute
jobs submitted via an interface running on a Kubernetes cluster.
• Metje [Met24] proposed an approach and studied various existing approaches for a
running Kubernetes cluster on top of Slurm without relying on root access.
On a given HPC system using Slurm as its resource manager, trying to employ some
of these approaches would require replacing Slurm with another scheduling system such
as Kubernetes. This would disrupt existing users and workflows of such a system and
force users to migrate to the new system. By keeping the existing resource manager,
the inference service could operate side by side with user workloads and utilize gaps in
the Slurm scheduling [Prz+; Cop+24]. While this approach has the potential to greatly
improve the resource utilization of the HPC hardware, for example, by serving inference
from gaps in the scheduling besides training jobs, this can also result in a reduced user
experience for users of the inference service. Especially when serving large LLMs, long
start-up times can be expected when loading these models into memory. Where serverless
functions commonly take only seconds to load [Prz+], loading an LLM can take multiple
minutes, which would be the time a user needs to wait for a response to their inference
request if no instance of the desired model was ready.
This issue becomes especially
pressing when considering the high need for GPU resources from LLM inference and
training workloads, which could result in training jobs consuming all GPUs in a cluster
and leaving no resources for inference services or vice versa.
Another major aspect of running services on HPC systems is security [Bra+19] as
sensitive research data and valuable compute resources are concentrated in HPC centers.
A common access protocol for HPC clusters is SSH [Liu23]. SSH itself allows for creating
a remote terminal but can also be used to forward arbitrary ports or commands to allow
remote access to web servers [Bür+12b]. Using SSH to tunnel traffic to create a web service
with a secure connection to a backend running on HPC infrastructure is an approach for
creating a HPC-based inference platform, but storing an SSH key in an exposed server
could pose a security risk and incentivize bad actors to penetrate the server in order to
gain access to the HPC cluster, and may thus not be allowed in HPC centers with strict
security standards such as SSH access only from individual users on local devices.
12Apache Hadoop 3.3.6 – Apache Hadoop YARN. url: https://hadoop.apache.org/docs/stable/
hadoop-yarn/hadoop-yarn-site/YARN.html (visited on 06/24/2024).
Section 3
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
4


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
One should also consider the possibilities for the usage of LLMs in education [LC24;
JL23] as many HPC centers are closely connected to universities and could offer inference
capabilities to students as an alternative to commercial offerings.
Besides the efforts described in this paper towards a private LLM inference, other
research centers also made efforts to develop a private chat interface including FhGe-
nie [Web+24], HAWKI13 and Minverva Messenger14. Their approaches to privacy are
mainly to implement user management on their side and to use a single OpenAI API key
to send all requests to the external ChatGPT API in order to obfuscate the attribution
of a given request to a specific user, essentially acting as a wrapper or middleman for
ChatGPT. The privacy provided by this approach is, however, limited as all requests and
responses are still sent to a third party, e.g., OpenAI or Microsoft Azure. Support for
alternative endpoints via self-hosted backends that provide an OpenAI-compatible API,
which is in development for HAWKI and FhGenie, could provide a fully private solution.
4
Challenges
In this section we present the challenges and concerns of hosting web services on an HPC
infrastructure that employs Slurm as its resource manager. As challenges we consider
• paradigm differences of using a batch scheduler, such as Slurm, to provide a service,
• security implications when exposing a web service connected to HPC infrastructure,
• performance inhibitors that a service running on HPC faces,
• dynamic scalability when using a batch scheduler,
• reliability features, such as health checks and self-healing,
• the options for guaranteeing user data privacy and
• monitoring and accounting.
Each of these points is discussed in detail in the following.
Paradigm Differences
The purpose of HPC clusters is commonly to run large-scale
and demanding batch jobs with a fixed start and end point, but not continuous and
scalable web services. The batch job and service paradigms are strictly different from one
another and have different expectations and goals. Slurm is designed to handle the batch
job paradigm and will schedule incoming jobs according to it. In order to adapt it to the
service paradigm, a job that runs an instance of a service must continuously be replaced
or extended. This requires some other scheduling control flow to operate on top of Slurm
to implement this functionality, e.g., keeping a pool of one or more instances active for
a given service by ensuring that one or more jobs are active for that service at a given
time.
13HAWK-Digital-Environments/HAWKI. url: https://github.com/HAWK-Digital-Environments/
HAWKI (visited on 06/20/2024).
14Minerva Messenger. url:
https : / / minervamessenger . mpdl . mpg . de / login (visited on
06/20/2024).
Section 4
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
5


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
Security
From a security perspective, hosting a web service on an HPC cluster opens
up additional attack surfaces. An HPC cluster is a valuable target for an attacker as it
offers massive amounts of compute resources, which could be misappropriated for crypto
mining and are hosts for potentially sensitive research data.
A web service that is intended to be reachable via the public internet requires some
components of that service to be exposed from behind the firewalls of a given HPC center.
This would open an attack surface for malicious actors to gain a foothold on the HPC
infrastructure by exploiting a vulnerability in the web service. While the implementation
of a given web service should avoid vulnerabilities, a defense-in-depth approach that
prevents further exploitation, is required.
SSH is an access protocol commonly allowed on HPC infrastructure and in some
cases is the sole option to access the necessary HPC components, thereby forcing access
patterns to build on top of SSH to remain compatible with the existing security framework.
Moreover, as an SSH connection is established using cryptographic keys, which would need
to be stored on a potentially exposed machine in order to connect to the HPC backend,
extra caution must be taken to ensure the damage caused by a potential compromise of
this key is limited, such that it cannot be used as an entry point to the HPC cluster and
its compute power by an attacker.
Performance
HPC infrastructure commonly includes specialized hardware such as HPC
interconnects, e.g., Infiniband, and parallel file systems to optimize the performance
of batch jobs.
While HPC hardware and Ethernet interconnects are normally power-
ful enough to support most services, when pushing the performance, any introduced
workarounds may result in bottlenecks and add latency or hinder throughput. An in-
frastructure that is dedicated and tailored to running production services may therefore
consistently outperform the architecture proposed in this paper.
Scalability
Slurm does not inherently provide an option to scale up the number of jobs
as the number of requests increases, requiring additional efforts to implement automatic
scaling. In order to support a service where request volumes vary over time, it is necessary
to support automatic scaling, where the number of instances for a given service is adjusted
based on request volume. This requires the implementation of a functionality to measure
the current request volume as well as to calculate and enforce the desired amount of
service jobs.
Moreover, this introduces the problem of service discovery, as incoming
requests need to be load balanced across all available service job instances and therefore
the system responsible for routing incoming requests needs to be aware of all healthy job
instances.
Reliability
Slurm itself is robust against nodes failing by ignoring problematic nodes
and assigning jobs to other nodes until an administrator fixes the issue. However, Slurm
does not implement mechanisms to ensure constant availability such as health checks,
which are crucial for a service to be reliably accessible and available at all times. These
mechanisms therefore need to be implemented on top of Slurm.
Privacy
For a service that offers an interface to chat with LLMs, essentially receiving
user inputs and responding with outputs generated by LLMs that process these conversa-
tions, one can consider all user inputs and responses to be sensitive user data that must be
kept private, thereby limiting the options for storing user data. Some approaches include
Section 4
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
6


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
using cryptography and key management mechanisms to keep user data encrypted and
inaccessible while it is not used directly.
However, in the context of highly sensitive information such as medical data, privacy is
critical and should be prioritized, as leakage of such data could be catastrophic. Depending
on the type of service, stricter privacy requirements can be achieved by enforcing the use of
encryption keys for users to be able to download and decrypt their data. The other option,
which is feasible and arguably even more secure, is to never store sensitive user data, such
as past and ongoing conversations with LLMs, on the server side and instead store all
such data only on the local devices, for example in the browser. Another consideration
to data privacy is the encryption of data in transit in order to prevent a scenario where
an attacker performs man-in-the-middle attacks after compromising part of the service to
read user inputs and responses from the services.
It is possible to implement end-to-end encryption and to keep data encrypted during
computation using Trusted Execution Environments, which are currently available for
some NVIDIA GPUs15. These additional measurements can be considered for system
hardening but go beyond our basic requirements, as these serve to protect incoming
data while parts of the system have already been compromised.
Nevertheless, when
following defense-in-depth security paradigms, these additional measurements are well
worth pursuing.
Monitoring and Accounting
When submitting jobs via Slurm, it is possible to per-
form accounting based on the compute hours used by a given job and attribute them to
the user who submitted the job. However, when offering a service, it is to be expected
that multiple users will be served by the same job instance.
Furthermore, it can also be expected that many service users do not have access
privileges to the HPC systems, such that the service user management is independent
from the HPC center. Therefore, the service jobs should be submitted via a functional
account that has no correspondence to an individual user or administrator. User-based
monitoring, accounting and moderation instead has to be handled in an authentication
layer in front of the service or integrated within the service itself. Individual requests can
be attributed to logged-in user accounts, and the timestamp for a given request can be
stored in order to monitor the service usage without violating the privacy assumptions
from the previous paragraph.
5
Chat AI Architecture
The architecture of Chat AI is shown in Figure 1 with its two major components, namely
the web server labeled as ESX Server and the HPC infrastructure labeled as KISSKI
Platform. Fundamentally, the HPC platform is only expected to provide Slurm itself.
Each of the two major components is split up into further sub-components, which are
individually explained in the following. It should also be noted that all sub-components
running on the ESX Server have been containerized and operate via docker compose.
15Data Integrity for AI Models. url: https://www.nvidia.com/en-us/data-center/solutions/
confidential-computing/ (visited on 06/26/2024).
Section 5
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
7


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
Figure 1: Architecture of Chat AI. This diagram displays the main components of the
service, consisting of an ESX web server that communicates to the login/service node,
and the compute nodes of the HPC KISSKI platform.
5.1
Authentication
We employ an Apache web server as a reverse proxy for our web interface to provide
integration with our Single Sign-On (SSO) provider Academic Cloud16. This is effectively
an OAuth2 authentication mechanism using the Apache web server module OpenIDC and
could be used via any other SSO provider that implements the same protocols.
Once a user has signed into the SSO provider, they are forwarded through the API
Gateway to the Chat AI web interface. Furthermore, when forwarding the requests, the
Apache web server attaches the email address of a given user as the user id to all requests.
5.2
API Gateway
We deployed the Kong Open Source edition17 as the API gateway to route incoming
requests to the corresponding upstreams, e.g., the HPC proxy or the web app. Kong was
chosen as it is open source and provides all the features we require from an API gateway
including creation and management of API routes, rate limiting, API key management
and observability.
Continuing from the Apache web server authentication component, a user request is
16AcademicCloud. url: https://academiccloud.de/ (visited on 06/26/2024).
17Kong/Kong. url: https://github.com/Kong/kong (visited on 06/25/2024).
Section 5
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
8


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
forwarded by the Kong API Gateway to the Chat AI web interface via its respective route.
If a user then submits an inference request to an LLM via the web interface, the request
is sent through the API Gateway to the corresponding route depending on the selected
model.
Moreover, the API Gateway also exposes an access point for API users. Users with
valid API keys can bypass the Apache web server and connect to the API Gateway directly,
which performs authentication via the provided API key. This unifies the path for web
and API users past the API gateway such that the incoming route and user is abstracted
from the backend service.
5.3
Web App
Figure 2: Chat AI App. This shows the Chat AI web interface written with React and
Vite with the chat history on the left, the prompt window on the top right and a drop
down for model selection at the bottom right.
The web interface illustrated in Figure 2 serves as a user-friendly way for users to
interact with a selection of LLMs. After testing existing open source software such as Text-
Generation-Webui18 or LibreChat19 we found that we require more tight control over the
handling of user data and the routing of requests, which we gained by implementing our
own web interface. Most notably we wanted to ensure that our application exclusively runs
in the browser with no significant active server code, which prevented us from employing
a solution such as HAWKI20 that relies largely on server-sided execution via PHP.
We experimented with various solutions and built a prototype for the web interface
using gradio [Abi+19] but this resulted in significant load on our server for large user
numbers.
Afterward, we tried to rewrite the web interface using React21 and Vite22,
18Oobabooga/Text-Generation-Webui. url: https://github.com/oobabooga/text- generation-
webui (visited on 06/26/2024).
19Danny-Avila/LibreChat. url:
https : / / github . com / danny - avila / LibreChat (visited on
06/26/2024).
20HAWK-Digital-Environments/HAWKI. url: https://github.com/HAWK-Digital-Environments/
HAWKI (visited on 06/20/2024).
21React. url: https://react.dev/ (visited on 07/31/2024).
22Vite. url: https://vitejs.dev (visited on 07/31/2024).
Section 5
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
9


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
which is the design that is shown in Figure 2. With this implementation we achieved a
significant load reduction on our web server.
In Figure 1 the Chat AI web app is shown behind both the authentication and the
API gateway from the user perspective as only the API gateway is exposed externally
and the web app itself is served as a route via the API gateway.
5.4
HPC Proxy
To enable the submission of inference requests from the web server to the HPC cluster
without compromising its tight security, we wrap this communication in an SSH con-
nection. SSH is the only protocol allowed to access the login node of our HPC center,
labelled as Service Node in Figure 1, and automatic logins via SSH to user accounts are
also forbidden.
We deploy an SSH key on the web server and prevent it from potentially being used
by an attacker to gain a foothold on the HPC cluster by employing the ForceCommand
directive of SSH23. This directive is used to restrict an SSH key to a specific command such
that it cannot be used to run generic shell commands on the remote machine. Moreover,
as this is configured via the authorized_keys file in a user’s directory, it can be configured
without root access. In our case, the SSH key belongs to a functional account which is
also responsible for the submission of service jobs to Slurm.
We have developed the HPC Proxy as an application that ensures that the SSH con-
nection between itself and the HPC login node remains open and is reestablished in case
an interruption breaks the SSH connection. Interruptions are detected by attempting to
send keep alive pings once every 5 seconds. Besides that, the HPC Proxy serves as a
regular proxy that forwards requests to our Cloud Interface Script, a bash script running
on the HPC service node. The Cloud Interface Script is designed to forward authorized,
inference-related HTTP requests to a service node, which is running the desired model
and ready to respond. Additional steps were taken to ensure the proxy is fast, efficient
and generalizable with support for various types of requests and responses, including
streaming.
In Figure 1 we included two HPC Proxy instances, as this architecture decouples the
web server from the HPC platform, allowing a single web server to potentially utilize
multiple HPC platforms by starting an HPC Proxy instance per HPC platform and load
balancing via the API Gateway. On the other hand, it is also possible to have multiple
web servers utilize the same HPC platform, each with its own SSH connection. With this
configuration, we are able to provide custom web services that require HPC resources
without directly exposing the components in the cluster.
5.5
Cloud Interface Script
This is bash script is placed on the service node and receives every request send via the
HPC Proxy. The ForceCommand directive for the SSH connection is configured such that
the HPC Proxy is only able to call the Cloud Interface Script via the SSH connection.
On every keep alive ping, set to every 5 seconds for the SSH connection, it triggers our
scheduler script, which is discussed in the next section.
23Sshd_config(5) - Linux Manual Page. url: https://man7.org/linux/man-pages/man5/sshd_
config.5.html (visited on 06/26/2024).
Section 5
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
10


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
The HPC Proxy transmits HTTP requests in the form of parameters when calling the
cloud interface script via an SSH command and receives the responses via stdout from the
cloud interface script. For larger requests, the cloud interface script receives the requests
via stdin. The cloud interface script utilizes a routing table maintained by the scheduler
script to determine where to forward the request.
5.6
Scheduler
We developed a scheduler script, which is run periodically on every incoming keep alive
ping from the HPC Proxy, with the goal of ensuring the availability of the service instances.
The scheduler script can be configured with a set of services it should maintain along with
the specifics of running their respective jobs, such as the job script and settings for when
to adjust the number of active instances for a given model.
The scheduler script obtains a list of running jobs for each service from Slurm via the
squeue command, compares it against the given configuration, and takes the necessary
actions to maintain the services, such as submitting new jobs via sbatch. In order to
avoid race conditions, we ensure only one instance of the scheduler is running at a time
by means of a lock file.
The scheduler script maintains a routing table, containing an entry for each active
job, along with its associated service, node and port number.
This table is used to
route each incoming request to a specific node and port number associated with a service
instance, selected randomly out of all eligible instances, effectively performing random
load balancing.
As Slurm provides no network virtualization, if two jobs on the same node try to
occupy the same port, it will fail for the second job as the port is already occupied by the
first job. To avoid this, the scheduler script picks a random port when submitting a new
job for that job to use and checks the routing table to ensure that the port is not already
occupied.
Once a service job has been submitted, it takes an uncertain amount of time before
the instance is ready to serve requests, as Slurm needs to assign it to a node and the
model needs to be loaded into GPU memory. Therefore, the scheduler script periodically
probes the newly submitted jobs until they are ready, before marking them as ready to
serve requests in the routing table.
To scale up and down the number of service instances according to demand, calculating
the request volume for a given service is necessary. This is performed by storing the
average number of concurrent requests to each service within a predefined time window,
which is then recalculated and updated on each scheduling run. If this average is higher
than a certain threshold, the scheduler spawns multiple instances of that service to prevent
the formation of a backlog. Likewise, when the average is too low, the scheduler allows
the excess jobs to expire without resubmitting them, effectively reducing the number of
instances and freeing up resources.
In an alternative design, the request volume could be measured by the API Gateway
or HPC Proxy and forwarded to the scheduler script, but we decided against this option
in order to minimize the coupling between the web server and the HPC platform, and
instead pursued a solution to calculate the request volume directly on the HPC platform.
Section 5
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
11


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
5.7
LLM Server
As Chat AI is an LLM chat service, we also require a runtime to serve LLM inference
requests.
For that purpose, one can assume the individual services mentioned in the
previous subsection to refer to various LLMs hosted on the HPC infrastructure, but they
can also be any server runtime that benefits from HPC resources.
To run an LLM model with GPU acceleration, we employ the open source runtime
vLLM24. vLLM has the advantages of offering an OpenAI-compatible API and an active
community, which quickly implements and supports many popular open source LLMs.
From our experience, vLLM was several times more efficient than our unoptimized LLM
runtime implementation.
5.8
External Proxy
The External Proxy serves as an optional extension to the architecture to provide access to
external models such as ChatGPT4 [Ope+24] as an additional route in the API Gateway,
effectively becoming a wrapper for additional services. As ChatGPT4 requires paid access,
we placed strict rate limits via the API Gateway and restricted access to certain user
groups.
5.9
Monitoring
The monitoring components are not shown on Figure 1 as they are optional but neverthe-
less important for capturing metrics and detecting issues. We employ an external Grafana
service, which constantly captures logging and monitoring data from a Prometheus server
that is integrated into the API gateway via a plugin. This is performed via standardized
monitoring endpoints and ensures only authorized access to the monitoring system.
6
Evaluation
Our primary goals for the LLM web service were to ensure its privacy, security and
practicality. Due to the nature of our infrastructure and demands of our users, security
and privacy took the highest priority in our design.
In this section, we evaluate the
architecture for Chat AI based on the achieved standards for security and data privacy as
well as performance measurements regarding throughput and latency. Furthermore, we
assess the user adoption as a factor in determining the architecture’s real-world viability
and overall success.
6.1
Security
In order to evaluate the security of our architecture we assume different attack scenarios.
These should serve to visualize that our designs follow the principals of defense-in-depth,
minimizing permission and not storing anything that does not need to be stored.
24Vllm-Project/Vllm. url: https://github.com/vllm-project/vllm (visited on 06/24/2024).
Section 6
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
12


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
6.1.1
Security of the Web Interface
Our Chat AI web interface employs standard security mechanism via TLS and in order
to access it, a user must have a valid account and login to authenticate with the SSO
provider.
Assuming a breach in the web interface, which gives an attacker a shell in the web
interface container, this would allow the attacker to spy on users of the web interface and
set up man-in-the-middle attacks until the breach is discovered and patched. However,
as no user data is stored, no accumulated data can be stolen by an attacker.
Administrative tasks on the web server are handled via an SSH connection, which is
only possible from our internal VPN when utilizing two-factor authentication.
Due to the containerized nature of the setup, an attacker would need to escape the
container to gain a shell on the host system and then gain root access in order to compro-
mise other parts of the system. Even in this scenario, an attacker would not be able to use
the compromised web server as an entry point into other parts of the HPC infrastructure.
6.1.2
Security of the HPC System
In our view, compromising the HPC system would be the worst case scenario that needs
to be prevented at all costs. As users do not have direct access to the HPC infrastructure
in this architecture, any attacks on the HPC infrastructure would necessarily originate
from the web server, meaning that some component within the web server would have
to be already compromised. In this case an attacker may attempt to locate and read
the SSH key used to establish the connection to the HPC system. However, as the SSH
key is configured with ForceCommand to only ever call the Cloud Interface Script and
corresponds to a functional account with no administrative permissions on the cluster, its
effectiveness in penetrating the HPC platform is significantly reduced.
A potential alternative to the attack scenario above would be if an attacker discovered a
flaw in our Cloud Interface Script and attempts to perform injection attacks by submitting
requests that instead of being routed to a service, end up executing shell commands. For
this purpose we bring extra attention to the implementation of the input parsing in the
Cloud Interface Script to protect against injection attacks, restricting any request to follow
a preset of determined paths, and avoiding any potentially dangerous commands such as
eval.
Essentially, an attacker would need to breach multiple layers of security to first breach
the the web server, and then penetrate multiple security layers again to get shell access
on the HPC system. Even if these security layers fail, the previous conversations and
messages of our users physically cannot be stolen from us as they were never stored in
the first place. While it is impossible to claim that a system’s security is perfect, we
consider having multiple layers that an attacker would need to breach as a marker of our
architecture’s security.
6.2
Data Privacy
As for data privacy, we regard compliance with GDPR and the strict data privacy mea-
sures it necessitates as our main benchmark. Sensitive and personalized data must be
transmitted and stored securely to ensure that third parties cannot access it. We take pri-
vacy a step further and minimized the data collection according to GDPR article 5(1) lit. c
to the degree that no user prompts are being stored at any circumstance, i.e., prompts
Section 6
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
13


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
and responses are not stored on the server. To still provide a stateful conversation, the
conversation history is solely stored within the user’s browser, which can be deleted with
a button provided in the web interface or by resetting the browser cache.
It follows that conversational data is also not used for any purpose such as training
models. Some non-conversational usage statistics are gathered solely for monitoring and
accounting purposes. This includes the user’s account identifier, timestamps for requests,
and selected model. With this information, we can monitor the load on the individual
models and identify potential misbehaving users.
As a result of these measures, an attacker on the web interface or on the HPC system
cannot access past conversations as they are only kept locally on the users’ devices and
never stored on the server.
6.3
Performance
For our architecture to be able to support increasing numbers of users, we need to as-
sess its performance. To evaluate the effectiveness of incorporating the HPC cluster as
the backbone of our service, we conducted a series of tests. These tests measured the
throughput and latency of each component in the architecture and can be used to iden-
tify bottlenecks in the pipeline. Latency measurements were performed using a custom
shell script, and for throughput measurements we employed the load testing framework
Locust25.
6.3.1
Latency
The latency or response time is the amount of time it takes for a client to receive a
response to a request, i.e., the duration between the time a request is sent and the point
in time when the response is received. To identify potential bottlenecks, we performed
latency measurements for every individual component in the architecture on the path for
user requests and calculated the time spent in each step. For statistical validity, we took
the average over 50 measurements with identical conditions.
The setup that the tests were performed on consists of a VM web server running
Ubuntu 22.04.4 LTS with a 16-core AMD EPYC processor and 8 GB of RAM, and a
Slurm-based HPC cluster consisting of one login node and 10 GPU nodes, each with 4
Nvidia H100/80 GB GPUs and a 52-core Intel(R) Xeon(R) Platinum 8470 processor with
500 GB of RAM running Rocky Linux 9.2.
Table 1: Latency measurements from the ESX machine. Aggregated average latency for a
request from the ESX machine to first token from LLM in active service job. Probe refers
to HTTP request on health endpoint. Column 3 gives the aggregated average latency in
ms with the difference, e.g., latency incurred in the respective operation.
Component
Operation
Agg. Avg. (std.) in ms
Diff. in ms
ESX Machine
Probe local proxy
2.59 (0.56)
2.59
HPC Service Node
SSH Command
13.12 (0.59)
10.54
HPC Service Node
Probe GPU node
18.43 (1.86)
5.30
HPC GPU Node
LLM First Token
51.06 (2.03)
32.63
25Locustio/Locust. url: https://github.com/locustio/locust (visited on 06/26/2024).
Section 6
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
14


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
Under normal conditions when the system is not overloaded, a user can expect to
receive their first response token after approximately 50 ms, of which more than 27 ms
are the compute time of the underlying LLM. Therefore, the additional latency overhead
introduced by our architecture was approximately 23 ms, which is not noticeable in most
cases and we consider acceptable.
6.3.2
Throughput
Table 2: Throughput results for a regular user request. ∗The actual throughput was higher
than the capacity of Locust to spawn requests in our setup, and was deemed sufficient for
our purposes. ∗∗The prompt was “count from 1 to 10”, to which the model almost always
responded with the numbers 1 to 10 consecutively.
Component/Operation
Throughput (RPS)
Apache Web Server
3 000+∗
Kong API Gateway
3 000+∗
Chat AI Web Interface
1 300 −1 800
Chat AI Web Interface Middleware
200 −300
SSH to HPC Service node
200
SSH to HPC GPU node
200
Single word from 7B LLM
100
Sentence from Intel Neural 7B LLM∗∗
27
Sentence from Mixtral 8x7B LLM∗∗
8
Sentence from Qwen1.5 72B LLM∗∗
2
Sentence from Meta Llama3 70B LLM∗∗
2
The throughput or load experiments were conducted to assess the capacity of our
architecture, i.e., its capability to handle a large number of requests from users. This en-
ables us to assess the efficiency and scalability of our architecture and its implementation.
Similar to the latency measurements, we individually tested the throughput for each of
the individual components and steps in the architecture on the path of a user request.
The hardware setup was identical to that of the latency tests, but we used Locust
for the throughput measurements. The results are captured in Table 2, which shows the
throughput in requests per second of every individual component of our infrastructure.
The selection of models used in this test include Intel Neural 7B26, Mixstral 8x7B [Jia+23],
Qwen1.527 and Llama3 [AIM24]. Except for Mixtral 8x7B all of these models were since
replaced by other more recent LLMs. However, this does not decrease the validity of this
throughput analysis.
While a throughput of 200 for the SSH connection is not impressive, it is sufficient for
the purpose of our Chat AI service as the bottleneck is the capacity of the LLMs to respond
to prompts. If the model instances are scaled up by a factor of 10, it is possible that the
infrastructure, especially the SSH connection, would need to be optimized or expanded
to increase the capacity of the service. Extending the HPC Proxy implementation to
establish more than one SSH connection with the cluster and having it load balance
requests would be feasible to overcome this limitation.
26Intel/Neural-Chat-7b-v3-1 · Hugging Face. url: https://huggingface.co/Intel/neural-chat-
7b-v3-1 (visited on 08/02/2024).
27Introducing Qwen1.5. url: https://qwenlm.github.io/blog/qwen1.5/.
Section 6
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
15


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
6.4
User Adoption
Figure 3: Total number of distinct users from Feburary 22nd until July 30th 2024. The
total number of users has grown consistently since its initial release, with a slight jump
following a university-wide advertisement on April 8th.
Finally, we explore the user adoption and growth in popularity of Chat AI. Since its
initial release on February 22nd, 2024 to users of Academic Cloud28, the popularity of the
service has increased rapidly and consistently. As shown in Section 6.4, in the first three
months, over 6 000 users were registered in the service, and by June 2024 this number
increased to 9 000. This includes users from over 170 universities and over 150 research
institutions with about 80 of the institutions being part of the Max-Plank-Gesellschaft
(MPG)29.
On average, about 400 to 500 users actively use Chat AI on a typical work day as can
be seen in Section 6.4, of which approximately 100 are new to the service. The drops are
on the weekends and holidays, which indicates that our users mostly prefer to use Chat
AI on weekdays rather than weekends, suggesting that Chat AI is mainly utilized in a
professional setting. A slight decrease in users can also be seen at the onset of the summer
break in Germany in July. In total, Chat AI has received and responded to more than
350 000 messages as of 30.07.2024. These numbers are expected to increase in the future
as we continue to add features and improve the service.
Over time, several features and models were added to Chat AI, which can be seen in
the timeline given in Section 6.4. The ability to interact with OpenAI’s GPT-4 model was
included shortly after the initial release of the service, followed by the addition of many
state-of-the-art open source models in the subsequent weeks including Qwen and Mixtral
as shown in Section 6.4. Overall, four internal models are available through Chat AI at
the time of writing including Meta’s Llama3.1 8B and 70B, Alibaba Cloud’s Qwen2 and
Mistral’s Mixtral 8x7B.
28AcademicCloud. url: https://academiccloud.de/ (visited on 06/26/2024).
29Startseite - Max-Planck-Gesellschaft. url: https://www.mpg.de/de (visited on 06/26/2024).
Section 6
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
16


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
Figure 4: Daily Chat AI users from February 22nd until July 30th 2024. New users, are
users that used the Chat AI service for the first time on a given day, while Daily users
are returning users.
In May 2024, the redesign of the user interface was released, moving from our old
gradio-based web interface to the new React and Vite-based interface. With the redesign,
several new functions and features were added such as custom system prompts. A key
feature that was introduced in response to popular demand was the possibility to store
conversations onto local devices via an export and import functionality. This enables the
users to restore and resume previous conversations without compromising data privacy.
Additionally, we began offering OpenAI-compatible API access to our open source
models, for researchers and students to integrate custom apps and run experiments with
Chat AI. The API access, which is provided on request, proved to be massively popular
with over 100 API users and drastically increasing the number of requests and demand
for the open source models.
Despite offering free access to GPT-4, a massively popular commercial model, many
users still chose to interact with open source LLMs in Chat AI as can be seen in Section 6.4.
The major advantages, which could explain this demand is the data privacy, security,
customization, inference speed, and more recently the ability to interact via the API. This
indicates that despite the apparent superiority of commercial models, there is sufficient
demand for open source LLMs that it is possible to offer a competitive service without
depending on commercial LLMs, by leveraging these advantages. It also demonstrates
the capability of existing HPC infrastructures to host computationally demanding web
services using the proposed architecture at large scale.
7
Discussion
As the adoption of Chat AI continues to grow throughout universities and institutions in
Germany, our contribution as well as its advantages are evident in enabling the ability
Section 7
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
17


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
Figure 5: Total inference requests per day from February 22nd until July 30th 2024.
This shows the growth in popularity of Chat AI via the number of requests per day as
well as when significant models were added to the service. Moreover, each bar shows the
amount of requests, which were handled via internal models, those hosted on our own
infrastructure as opposed to external models, consisting of OpenAI’s GPT3.5 and GPT4.
to host a practical and secure web service on existing Slurm-based HPC clusters. In this
section we discuss the limitations of our approach and attempt to visualize the future
outlook of our service.
7.1
Limitations
Throughout our development efforts and the lifetime of the service, we encountered several
problems and challenges, as well as, identified limitations of our design, some of which
were resolved or could be resolved through additional development efforts but some remain
open and would require significant changes.
These challenges most significantly include
• reliability as in ensuring consistent availability of the service and recovering from
failure states,
• automation to reduce the effort of admins to add new models to the service and
• scaling to zero the number of active instance for a given model when it is not required
to release resources,
• additional security considerations regarding man-in-the-middle attacks and
• throughput limitations of our implementation.
Section 7
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
18


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
7.1.1
Reliability
Some reliability issues are rooted in the architecture due to its many points of failure, such
as the SSH connection from the web server to the HPC service node, which, if broken, can
cause an outage of the entire service. This was resolved by improving the implementation
of the HPC Proxy to quickly and automatically restore the connection.
Another issue we encountered were the numerous failures of the LLM service on the
HPC nodes, some of which were caused by bugs and incompatibilities of vLLM running on
our GPU nodes, while some were due to the incapability of Slurm to handle cases where
a node would fail, and, for example, required a reboot. While these issues are not specific
to our architecture, our scheduler script was not equipped to properly recover from these
failure states without manual intervention.
Furthermore, in some cases during high-demand and multiple node failures, it was
possible that Slurm could not find the required resources to start a new model instance,
and a given model became unavailable in the meantime. To mitigate these issues, we
introduced some countermeasures in the scheduler script, but the reliability and stability
still need further improvement.
While scalability was one of our main goals, which we attempted to solve through
the automatic scaling of the model instances during high demand, this scaling might not
occur fast enough to ensure a smooth and consistent service from the user perspective.
While the scheduler script is reasonably quick in detecting increased load, each new model
still needs to perform a cold start before being able to serve traffic. To start an instance
of a model, the model has to first load into GPU memory, which can take as long as ten
minutes for larger models on our hardware configuration. When the service encounters
many requests for a model and subsequently scales up its number of instances, the system
may become overloaded before the new instances are ready to respond. One possible
solution is to preemptively scale up the instances when a large number of requests is
expected to arrive based on observed usage patterns, e.g., based on time of day.
7.1.2
Automation
Many of the problems that a resource manager for a service paradigm has to solve and
that are already solved through a platform such as Kubernetes had to be re-implemented
in our solutions. This meant at first more manual setup and configuration tasks, which
were gradually being automated. For example, the detection and handling of common
error states, which are solved through health checks and automatic restarts had to be
implemented from scratch.
The deployment of new LLM models on the HPC platform still requires manual effort
to set up the service for the scheduler script and to create the respective routes in the
API Gateway. However, to facilitate the adoption of our solution, more automation tools
should be implemented and added to the service.
7.1.3
Scale to zero
With our current design it is not possible for a service to properly handle scale to zero.
In an implementation of scale to zero, when no requests arrive for a given service for
some time, it would reduce the number of desired active service jobs to zero. If a request
then arrived for a service that has scaled to zero, the request would need to be held in a
queue until a single instance of a service job is ready to process the accumulated requests.
Section 7
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
19


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
Depending on the service job, the cold start time for it might be multiple minutes, for
example, to start a 70B LLM, which could result in timeouts for users. Currently, with
our architecture and its implementation, it would be possible to scale a model down to
zero instances but there is no component that maintains a queue of requests while the
model is loading. While this could be implemented it would require significant changes,
as the HPC Proxy and all components in the web server are also unaware of the status of
the HPC platform and the available service instances, such that only the Cloud Interface
Script can detect that a request cannot be answered until the model is ready. Therefore,
as this is a script and not an active component, the queue would need to be held in the
web server, and it would need to query the availability status of the services by constantly
probing the Cloud Interface Script.
Another option would be to scale models to zero on a fixed schedule. An HPC cluster
that is used to serve LLM inference requests by day, could be used to process regular
Slurm jobs by night. This could be implemented via two Cron jobs. The first job would
at the end of the work day create a backup of the last used configuration for the scheduler
and provide an empty configuration instead. With this the service jobs would expire and
the resources could be used by regular Slurm jobs. Then at the start of the work day,
the second Cron job would trigger and replace the empty configuration with the backup
created by the other job such that the service could resume regular operation during the
day. In order for the service jobs to be scheduled by Slurm without waiting for a backlog
for other Slurm jobs, they would need to be submitted with higher priority.
7.1.4
Encryption
In our current implementation, it would be possible for an attacker to perform man-in-
the-middle attacks if that attacker manages to breach the Chat AI web interface as the
user messages are not encrypted internally. Protecting against this would require user
clients to perform asymmetric encryption with decryption only happening on the HPC
platform itself. This could be implemented via API client libraries and provided to users
for sensitive workloads.
7.1.5
Throughput
Our implementation delivers a throughput of about 200 requests per second but can be
scaled up, for example, by deploying multiple instances of the HPC Proxy or the Chat
AI web server and having Kong do load balancing across these multiple instances or by
further optimizing the implementation of the code. However, the component that is the
most expensive to scale up for a data center employing our solution is the number of
service jobs with LLMs that require multiple high-end GPUs.
Assuming a throughput rate of 8 requests per second for an LLM that runs on 2 high-
end GPUs, 25 instances of this LLM would be required to max out the throughput of our
HPC Proxy, which would require 50 high-end GPUs. Via deployment of multiple instances
of the HPC Proxy and Chat AI web server, a limit of 3 000 RPS could be achieved, which
would require 750 high-end GPUs.
We consider this as sufficient for most data centers, HPC centers and even national
HPC centers. For hyperscaler data centers that serve 10 000 or even 100 000 RPS, this
solution is not sufficient but also not intended for.
Section 7
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
20


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
7.2
Future Work
Chat AI and its underlying architecture have proven to be a viable solution for hosting
HPC-based web services. As the popularity of Chat AI has grown, so did the demand for
more features, models and services. There are a multitude of additional AI services that
can be offered on our infrastructure and extend the scope of Chat AI.
As users are requesting to be able to run custom LLMs, which only a small subset of
the overall user base would use, we plan to further investigate how we can implement scale
to zero such that we can support a wide range of models without each of them constantly
occupying valuable GPU resources.
Furthermore, with the release of vision language models (VLMs) [Zha+24] both in the
open source [Wan+23b; Che+24] and the commercial space via GPT4o [Ope+24] we are
also looking to expand our implementation to support VLMs as well. Moreover, as our
architecture is not specific to LLMs we are also planning to implement support for other
AI services that benefit from GPU acceleration via our HPC infrastructure such as audio
transcription and text-to-speech services.
Finally, in order for our services to scale out more and to improve automation while
relying on well-established software, we are considering switching out the underlying in-
frastructure for a Kubernetes-based platform.
8
Conclusion
In this paper we presented a solution for securely hosting high-performance web services
on Slurm-based HPC infrastructure and demonstrated its capabilities through our imple-
mentation of Chat AI, a high-performance responsive web service that provides access
to state-of-the-art open source Large Language Models (LLMs). Notably, our solution
leverages existing Slurm installations with no special hardware or software requirements,
creating a low barrier to entry for other institutions with similar infrastructure and secu-
rity concerns.
Furthermore, we demonstrated the success and popularity of Chat AI through its user
adoption throughout the academic community. We inferred that the popularity of the
service, especially the open source models, stems from the full control we provide to users
over their data, conversations and messages, as well as the powerful web interface with
features such as custom system prompts.
The OpenAI-compatible API access to the
open source models proved to be popular and drastically increased the demand for these
models.
We believe that the Chat AI service and its underlying architecture have the potential
to democratize secure and private access to state-of-the-art LLMs, including open source
models. We hope that our contribution facilitates progress in the development of high-
performance, secure web services, accelerating innovation and research in various fields.
9
Code & Acknowledgements
Source Code
We provide the web interface as a stand-alone software, including our
Chat AI API service, which can be set up and run on a local machine or a web server
at https://github.com/gwdg/chat-ai. We also provide the software setup of our web
Section 9
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
21


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
server consisting of multiple containers, including the API gateway, the proxies, and
monitoring tools at https://github.com/gwdg/saia-hub. Finally, we provide the Cloud
Interface Script that ForceCommand runs, along with the scheduler and tools to run the
LLM servers at https://github.com/gwdg/saia-hpc.
Acknowledgements
This work was supported by the Federal Ministry of Education
and Research (BMBF), Germany under the AI service center KISSKI (grant no. 01IS22093A
and 01IS22093B) as well as many colleagues at the GWDG, the joint data center of Max
Planck Society for the Advancement of Science (MPG) and University of Göttingen.
Section 9
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
22


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
References
[24f]
“Qwen2 Technical Report”. In: (2024).
[Abi+19]
Abubakar Abid et al. “Gradio: Hassle-free Sharing and Testing of ML Models
in the Wild”. In: (June 2019). doi: 10.48550/arXiv.1906.02569. url:
https://arxiv.org/abs/1906.02569 (visited on 06/26/2024).
[AIM24]
AI@Meta. “Llama 3 Model Card”. In: (2024). url: https://github.com/
meta-llama/llama3/blob/main/MODEL_CARD.md.
[Bra+19]
David Brayford et al. “Deploying AI Frameworks on Secure HPC Systems
with Containers”. In: 2019 IEEE High Performance Extreme Computing
Conference (HPEC). 2019 IEEE High Performance Extreme Computing
Conference (HPEC). Sept. 2019, pp. 1–6. doi: 10 . 1109 / HPEC . 2019 .
8916576. url: https://ieeexplore.ieee.org/abstract/document/
8916576 (visited on 05/22/2024).
[Bro+20]
Tom Brown et al. “Language Models Are Few-Shot Learners”. In: Advances
in Neural Information Processing Systems. Vol. 33. Curran Associates, Inc.,
2020, pp. 1877–1901. url: https://proceedings.neurips.cc/paper/
2020/hash/1457c0d6bfcb4967418bfb8ac142f64a- Abstract.html (vis-
ited on 06/10/2024).
[Bul+18]
Ramesh Bulusu et al. “Addressing Security Aspects for HPC Infrastructure”.
In: 2018 International Conference on Information and Computer Technolo-
gies (ICICT). 2018 International Conference on Information and Computer
Technologies (ICICT). Mar. 2018, pp. 27–30. doi: 10.1109/INFOCT.2018.
8356835. url: https://ieeexplore.ieee.org/abstract/document/
8356835 (visited on 06/25/2024).
[Bür+12a]
Claudius M. Bürger et al. “Introduction of a Web Service for Cloud Com-
puting with the Integrated Hydrologic Simulation Platform ParFlow”. In:
Computers & Geosciences 48 (Nov. 1, 2012), pp. 334–336. issn: 0098-3004.
doi: 10.1016/j.cageo.2012.01.007. url: https://www.sciencedirect.
com/science/article/pii/S009830041200009X (visited on 06/24/2024).
[Bür+12b]
Claudius M. Bürger et al. “Introduction of a web service for cloud computing
with the integrated hydrologic simulation platform ParFlow”. In: Computers
& Geosciences 48 (2012), pp. 334–336. issn: 0098-3004. doi: https://doi.
org/10.1016/j.cageo.2012.01.007. url: https://www.sciencedirect.
com/science/article/pii/S009830041200009X.
[Che+24]
Kezhen Chen et al. Dragonfly: Multi-Resolution Zoom Supercharges Large
Visual-Language Model. 2024. arXiv: 2406.00977 [cs.CV].
[Cho+22]
Aakanksha Chowdhery et al. “PaLM: Scaling Language Modeling with Path-
ways”. In: (Oct. 5, 2022). doi: 10 . 48550 / arXiv . 2204 . 02311. arXiv:
2204.02311 [cs]. url: http://arxiv.org/abs/2204.02311 (visited
on 06/10/2024). Pre-published.
[Cop+24]
Marcin Copik et al. “Software Resource Disaggregation for HPC with Server-
less Computing”. In: (May 1, 2024). doi: 10.48550/arXiv.2401.10852.
arXiv: 2401.10852 [cs]. url: http://arxiv.org/abs/2401.10852 (vis-
ited on 05/22/2024). Pre-published.
Section 9
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
23


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
[Hil+20]
Michael Hildreth et al. “Large-Scale HPC Deployment of Scalable CyberIn-
frastructure for Artificial Intelligence and Likelihood Free Inference (SCAIL-
FIN)”. In: EPJ Web of Conferences 245 (2020), p. 09011. issn: 2100-014X.
doi: 10.1051/epjconf/202024509011. url: https://www.epj-conferences.
org / articles / epjconf / abs / 2020 / 21 / epjconf _ chep2020 _ 09011 /
epjconf_chep2020_09011.html (visited on 05/22/2024).
[Jia+23]
Albert Q. Jiang et al. “Mistral 7B”. In: (Oct. 10, 2023). doi: 10.48550/
arXiv.2310.06825. arXiv: 2310.06825 [cs]. url: http://arxiv.org/
abs/2310.06825 (visited on 06/14/2024). Pre-published.
[JL23]
Jaeho Jeon and Seongyong Lee. “Large Language Models in Education:
A Focus on the Complementary Relationship between Human Teachers
and ChatGPT”. In: Education and Information Technologies 28.12 (Dec. 1,
2023), pp. 15873–15892. issn: 1573-7608. doi: 10 . 1007 / s10639 - 023 -
11834-1. url: https://doi.org/10.1007/s10639-023-11834-1 (visited
on 05/22/2024).
[Ksh23]
Nir Kshetri. “Cybercrime and Privacy Threats of Large Language Mod-
els”. In: IT Professional 25.3 (May 2023), pp. 9–13. issn: 1941-045X. doi:
10.1109/MITP.2023.3275489. url: https://ieeexplore.ieee.org/
document/10174273 (visited on 06/17/2024).
[Kwo+23]
Woosuk Kwon et al. “Efficient Memory Management for Large Language
Model Serving with PagedAttention”. In: Sept. 12, 2023. doi: 10.48550/
arXiv.2309.06180. arXiv: 2309.06180 [cs]. url: http://arxiv.org/
abs/2309.06180 (visited on 06/10/2024). preprint.
[LC24]
Yu-Ju Lan and Nian-Shing Chen. “Teachers’ Agency in the Era of LLM and
Generative AI: Designing Pedagogical AI Agents”. In: Educational Technol-
ogy & Society 27.1 (2024), pp. I–XVIII. issn: 1176-3647. JSTOR: 48754837.
url: https://www.jstor.org/stable/48754837 (visited on 05/22/2024).
[Liu23]
Peini Liu. “Convergence of High Performance Computing, Big Data, and
Machine Learning Applications on Containerized Infrastructures”. Doctoral
thesis. Universitat Politècnica de Catalunya, July 17, 2023. doi: 10.5821/
dissertation-2117-403550. url: https://upcommons.upc.edu/handle/
2117/403550 (visited on 05/22/2024).
[Met24]
Sören Metje. Running Kubernetes Workloads on Rootless HPC Systems Us-
ing Slurm. GRO.data, Jan. 9, 2024. doi: 10.25625/GDFCFP. url: https://
data.goettingen-research-online.de/dataset.xhtml?persistentId=
doi:10.25625/GDFCFP (visited on 06/24/2024).
[Mic+22]
Paulius Micikevicius et al. “FP8 Formats for Deep Learning”. In: (Sept. 29,
2022). doi: 10 . 48550 / arXiv . 2209 . 05433. arXiv: 2209 . 05433 [cs].
url: http://arxiv.org/abs/2209.05433 (visited on 07/30/2024). Pre-
published.
[Ope+24]
OpenAI et al. “GPT-4 Technical Report”. In: (Mar. 4, 2024). doi: 10.48550/
arXiv.2303.08774. arXiv: 2303.08774 [cs]. url: http://arxiv.org/
abs/2303.08774 (visited on 06/10/2024). Pre-published.
Section 9
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
24


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
[PGE23]
Claudius Pott, Berk Gulmezoglu, and Thomas Eisenbarth. “Overcoming the
Pitfalls of HPC-based Cryptojacking Detection in Presence of GPUs”. In:
Proceedings of the Thirteenth ACM Conference on Data and Application
Security and Privacy. CODASPY ’23. New York, NY, USA: Association for
Computing Machinery, Apr. 24, 2023, pp. 177–188. isbn: 9798400700675.
doi: 10.1145/3577923.3583655. url: https://doi.org/10.1145/
3577923.3583655 (visited on 06/25/2024).
[Prz+]
Bartłomiej Przybylski et al. “Using Unused: Non-Invasive Dynamic FaaS
Infrastructure with HPC-Whisk”. In: (), p. 13.
[Sha+22]
Gilad Shainer et al. “NVIDIA’s Cloud Native Supercomputing”. In: Driving
Scientific and Engineering Discoveries Through the Integration of Experi-
ment, Big Data, and Modeling and Simulation. Ed. by Jeffrey Nichols et al.
Cham: Springer International Publishing, 2022, pp. 340–357. isbn: 978-3-
030-96498-6. doi: 10.1007/978-3-030-96498-6_20.
[Tea24]
Qwen Team. Introducing Qwen1.5. Feb. 2024. url: https : / / qwenlm .
github.io/blog/qwen1.5/.
[Wan+21]
Michael Wang et al. “GPU-Accelerated Machine Learning Inference as a
Service for Computing in Neutrino Experiments”. In: Frontiers in Big Data
3 (Jan. 14, 2021). issn: 2624-909X. doi: 10.3389/fdata.2020.604083. url:
https://www.frontiersin.org/articles/10.3389/fdata.2020.604083
(visited on 05/22/2024).
[Wan+23a]
Meng Wan et al. “OpenVenus: An Open Service Interface for HPC Environ-
ment Based on SLURM”. In: Smart Computing and Communication. Ed. by
Meikang Qiu, Zhihui Lu, and Cheng Zhang. Cham: Springer Nature Switzer-
land, 2023, pp. 131–141. isbn: 978-3-031-28124-2. doi: 10.1007/978-3-
031-28124-2_13.
[Wan+23b]
Weihan Wang et al. CogVLM: Visual Expert for Pretrained Language Mod-
els. 2023. arXiv: 2311.03079 [cs.CV].
[Wan+24]
Yuxin Wang et al. “Towards Efficient and Reliable LLM Serving: A Real-
World Workload Study”. In: (Mar. 3, 2024). doi: 10.48550/arXiv.2401.
17644. arXiv: 2401.17644 [cs]. url: http://arxiv.org/abs/2401.17644
(visited on 05/22/2024). Pre-published.
[Web+24]
Ingo Weber et al. “FhGenie: A Custom, Confidentiality-preserving Chat AI
for Corporate and Scientific Use”. In: (Feb. 29, 2024). doi: 10.48550/arXiv.
2403.00039. arXiv: 2403.00039 [cs]. url: http://arxiv.org/abs/2403.
00039 (visited on 06/20/2024). Pre-published.
[WYH23]
Xiaoxia Wu, Zhewei Yao, and Yuxiong He. “ZeroQuant-FP: A Leap Forward
in LLMs Post-Training W4A8 Quantization Using Floating-Point Formats”.
In: (July 20, 2023). doi: 10.48550/arXiv.2307.09782. arXiv: 2307.09782
[cs]. url: http://arxiv.org/abs/2307.09782 (visited on 07/30/2024).
Pre-published.
Section 9
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
25


---

Chat AI: A Seamless Slurm-Native Solution for HPC-Based Services
[YJG03]
Andy B. Yoo, Morris A. Jette, and Mark Grondona. “SLURM: Simple Linux
Utility for Resource Management”. In: Job Scheduling Strategies for Parallel
Processing. Ed. by Dror Feitelson, Larry Rudolph, and Uwe Schwiegelshohn.
Berlin, Heidelberg: Springer, 2003, pp. 44–60. isbn: 978-3-540-39727-4. doi:
10.1007/10968987_3.
[Zha+19]
Haikuo Zhang et al. “Artificial Intelligence Platform for Mobile Service
Computing”. In: Journal of Signal Processing Systems 91.10 (Oct. 1, 2019),
pp. 1179–1189. issn: 1939-8115. doi: 10.1007/s11265-019-1438-3. url:
https://doi.org/10.1007/s11265-019-1438-3 (visited on 05/22/2024).
[Zha+24]
Jingyi Zhang et al. “Vision-Language Models for Vision Tasks: A Survey”.
In: (Feb. 16, 2024). doi: 10.48550/arXiv.2304.00685. arXiv: 2304.00685
[cs]. url: http://arxiv.org/abs/2304.00685 (visited on 06/27/2024).
Pre-published.
[Zho+21]
Naweiluo Zhou et al. “Container Orchestration on HPC Systems through
Kubernetes”. In: Journal of Cloud Computing 10.1 (Feb. 22, 2021), p. 16.
issn: 2192-113X. doi: 10.1186/s13677-021-00231-z. url: https://doi.
org/10.1186/s13677-021-00231-z (visited on 05/22/2024).
Section 9
Ali Doosthosseini, Jonathan Decker,
Hendrik Nolte, Julian M. Kunkel
26
