---
title: NVIDIA Dynamo Planner Brings SLO-Driven Automation to Multi-Node LLM Inference
  - InfoQ
id: nvidia-dynamo-planner-brings-slo-driven-automation-to-multi-node-llm-inference-i
tags:
- internal-llm-inference-stacks-7f0c90
- nvidia-dynamo
- disaggregated-serving
- kubernetes
created: '2026-10-09T22:54:31.885439Z'
updated: '2026-10-10T14:52:17.027124Z'
source: https://www.infoq.com/news/2026/01/nvidia-dynamo-ai-kubernetes/
source_domain: www.infoq.com
fetched_at: '2026-10-09T22:54:31.863642Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'InfoQ report (Jan 31, 2026) on NVIDIA/Microsoft''s Part 2 Dynamo-on-Azure-Kubernetes-Service
  collaboration. First announcement claimed 1.2M tokens/sec raw throughput on distributed
  GPU systems; this release adds automated resource planning via two components: the
  Dynamo Planner Profiler (pre-deployment simulation tool, ~20-30s via an AI Configurator
  mode using pre-measured performance data, sweeps tensor-parallelism sizes for prefill
  vs decode to optimize ''Goodput'' -- throughput within TTFT/ITL SLOs) and the SLO-based
  Dynamo Planner (runtime orchestration engine that watches decode-pool KV-cache load
  and prefill-queue depth to scale prefill/decode workers independently as traffic
  shifts). Illustrated with a Qwen3-32B-FP8 airline-assistant case: SLA of 500ms TTFT
  / 30ms ITL; a weather-driven spike to 200 concurrent complex-rerouting users triggers
  scale-out from 1 to 2 prefill workers (decode stays at 1), with new workers online
  within minutes. Confirms Dynamo''s rate-matching problem in disaggregated prefill/decode
  serving is a first-class, actively-developed operational concern, not just a research
  curiosity.'
utility_score: 16.0
---

## InfoQ Software Architects' Newsletter
A monthly overview of things you need to know as an architect or aspiring architect.
[View an example](https://www.infoq.com/software-architects-newsletter#placeholderPastIssues)
[We protect your privacy.](https://www.infoq.com/privacy-notice/)
Close
QCon AI Boston (July 8-9, 2027): Agents. Context. Evals. Security. See how other teams make decisions. [ Register Now ](https://www.infoq.com/url/pb/8aca1c95-cf31-4c14-9682-e6adff746aa4/)
Close 
NVIDIA Dynamo Planner Brings SLO-Driven Automation to Multi-Node LLM Inference
[QCon AI New York (Dec 15–16): Production AI lessons from senior engineers. ](https://newyork.qcon.ai/?utm_source=infoq&utm_medium=referral&utm_campaign=infoqyellowbox_qainy26%20)
#  NVIDIA Dynamo Planner Brings SLO-Driven Automation to Multi-Node LLM Inference 
Jan 31, 2026 3 min read 
by
  * FollowSenior DevOps Engineer


#### Follow us on 
[ Youtube232K Followers ](https://bit.ly/4bg6QM8) [ Linkedin26K Followers ](https://bit.ly/44IzAtf) [ InstagramNew ](https://bit.ly/4eYXrtM) [ RSS19K Readers ](https://bit.ly/3RaJalC) [ X57.1k Followers ](https://bit.ly/4pfxivv) [ Facebook21K Likes ](https://bit.ly/3QrGMH2) [ BlueskyNew ](https://bit.ly/4eS8FjG)
Log in to listen to this article
Loading audio
Your browser does not support the audio element. 
0:00 0:00
Normal1.25x1.5x
Like


Microsoft and NVIDIA have [released Part 2](http://www.infoq.com/news/2026/01/nvidia-dynamo-ai-kubernetes/) of their collaboration on running NVIDIA Dynamo for large language model inference on [Azure Kubernetes Service](https://learn.microsoft.com/en-us/azure/aks/what-is-aks) (AKS). The first announcement aimed for a raw throughput of 1.2 million tokens per second on distributed [GPU](https://en.wikipedia.org/wiki/Graphics_processing_unit) systems. Now, this latest release focuses on helping developers work faster and improving operational efficiency. It does this through automated resource planning and dynamic scaling features.
The new capabilities center on two integrated components: the [Dynamo Planner Profiler](https://github.com/ai-dynamo/dynamo/tree/main/benchmarks/profiler) and the SLO-based [Dynamo Planner](https://github.com/ai-dynamo/dynamo/blob/main/docs/planner/sla_planner.md). These tools work together to solve the "rate matching" challenge in disaggregated serving. The teams use this term when they split inference workloads. They separate prefill operations, which process the input context, from decode operations that generate output tokens. These tasks run on different GPU pools. Without the right tools, teams spend a lot of time determining the optimal GPU allocation for these phases.
The Dynamo Planner Profiler is a pre-deployment simulation tool. It automates the search for the best configurations. Developers can skip manually testing various parallelization strategies and GPU counts, saving hours of GPU utilization. Instead, they define their needs in a _DynamoGraphDeploymentRequest (DGDR)_ manifest. The profiler runs an automated [sweep](https://github.com/ai-dynamo/dynamo/blob/main/docs/benchmarks/sla_driven_profiling.md#profiling-method) of the configuration space. It tests different tensor parallelism sizes for both prefill and decode stages. This helps find settings that boost throughput while staying within latency limits.
The profiler includes an AI Configurator mode that can simulate performance in approximately 20 to 30 seconds based on pre-measured performance data. This capability allows teams to rapidly iterate on configurations before allocating physical GPU resources. The output gives a tuned setup to boost what teams call "[Goodput](https://arxiv.org/abs/2401.09670)." This is the highest possible throughput while staying within set limits for Time to First Token and Inter-Token Latency.
Once a system enters production, the SLO-based Dynamo Planner takes over as a runtime orchestration engine. This component is "LLM-aware", which means that, unlike traditional load balancers, it keeps an eye on the cluster state. It tracks things like key-value cache load in the decode pool and the depth of the prefill queue. The Planner uses the profiler's performance bounds to scale prefill and decode workers. This helps meet service level goals as traffic patterns change.
The announcement illustrates these capabilities through a detailed airline assistant scenario. In this case, a [Qwen3-32B-FP8](https://huggingface.co/Qwen/Qwen3-32B-FP8) model supports an airline mobile app. It follows strict service level agreements: 500 milliseconds for Time to First Token and 30 milliseconds for Inter-Token Latency. During normal operations with short passenger queries, the system runs with one prefill worker and one decode worker. When a weather disruption leads to 200 users sending complex rerouting requests, the Planner notices the spike. It then scales up to two prefill workers but keeps one decode worker. The teams report that the new worker comes online within minutes, allowing the system to maintain latency targets during the traffic spike.
This release builds on the framework introduced in the original Dynamo announcement, which InfoQ [covered](https://www.infoq.com/news/2025/12/nvidia-dynamo-kubernetes/) in December 2024. In the last article, Azure and NVIDIA explained how Dynamo's design splits compute-heavy and memory-bound tasks across various GPUs. This allows teams to optimize each phase independently, matching resources to workload needs. For example, an e-commerce app's prefill task may process thousands of tokens, while its decode task only generates short descriptions.
The move from manual setup to automated, SLO-driven resource management shows how teams can better handle large language model deployment on Kubernetes. The Planner components provide tools that turn latency needs into GPU allocation and scaling choices. This aims to lower the operational burden of running disaggregated inference architectures. Automation tools can help organizations with reasoning-heavy or long-context LLMs. They make it easier to manage the complex multi-node GPU setups. They also support meeting service level goals during changing traffic patterns.
## About the Author
#### **Claudio Masolo**
Claudio is a Senior DevOps Engineer at Nearform. In his spare time, he likes running, reading, and playing old video games. 
Show moreShow less
####  This content is in the [DevOps](https://www.infoq.com/Devops/) topic 
Follow Topic
##### Related Topics:
  * [DevOps](https://www.infoq.com/Devops/)
### [DevOps](https://www.infoq.com/Devops)
Followers: 5151
Follow Topic
  * [Kubernetes](https://www.infoq.com/Kubernetes/)
### [Kubernetes](https://www.infoq.com/Kubernetes)
Followers: 494
Follow Topic
  * [AIOps](https://www.infoq.com/AIOps/)
Followers: 17
Follow Topic


  * #### Popular in DevOps
    * ##### [Atlassian Rebuilds Metrics Pipeline Around OpenTelemetry at Massive Scale](https://www.infoq.com/news/2026/09/atlassian-opentelemetry/)
    * ##### [OpenTelemetry Makes Kubernetes Attributes Processor Stable as Observability Schema Matures](https://www.infoq.com/news/2026/10/opentelemetry-kubernetes-observ/)
    * ##### [Context is the New Code](https://www.infoq.com/presentations/context-as-code-devops-agents/)
    * ##### [Cloudflare Details its Migration to "WordPress Successor" EmDash](https://www.infoq.com/news/2026/09/cloudflare-emdash-migration/)
    * ##### [GitLab Vulnerability under Active Exploitation Enables Unauthenticated Data Exfiltration](https://www.infoq.com/news/2026/10/gitlab-critical-vulnerabilities/)
  * #### Related Sponsors
    * ##### [8 Deployment Pattern Structures to Transform your CI/CD](https://www.infoq.com/vendorcontent/show.action?vcr=db7d052e-96e8-480e-9e43-cc0043fc5640&primaryTopicId=1893&vcrPlace=BOTTOM&pageType=NEWS_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Fnews%2F2026%2F01%2Fnvidia-dynamo-ai-kubernetes%2F)
    * ##### [AI-Native Software Delivery - Download the eBook (By O'Reilly)](https://www.infoq.com/vendorcontent/show.action?vcr=9831f722-2945-4987-98d0-c1c46b32e5ef&primaryTopicId=1893&vcrPlace=BOTTOM&pageType=NEWS_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Fnews%2F2026%2F01%2Fnvidia-dynamo-ai-kubernetes%2F)
    * ##### [The State of AI in Software Engineering](https://www.infoq.com/vendorcontent/show.action?vcr=a86a4b77-7050-44e5-a2f8-cb60a0fe0232&primaryTopicId=1893&vcrPlace=BOTTOM&pageType=NEWS_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Fnews%2F2026%2F01%2Fnvidia-dynamo-ai-kubernetes%2F)
    * ##### [AI's Physical Constraints: How AI Rewired the Data Center](https://www.infoq.com/vendorcontent/show.action?vcr=7bf8495f-cd88-40d6-92aa-1afa5ad24523&primaryTopicId=1893&vcrPlace=BOTTOM&pageType=NEWS_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Fnews%2F2026%2F01%2Fnvidia-dynamo-ai-kubernetes%2F)
    * #####  [ The Practical Agentic Coding Playbook ](https://www.infoq.com/url/f/a5beeb9d-4526-4e5d-ac54-9d284445fe9f/)
  * #### Related Sponsor
**Automated code transformation at scale. Fix vulnerabilities, migrate frameworks, and modernize legacy code across every repo.[Try Moderne Free](https://www.infoq.com/url/f/f2d610c9-1037-4ae9-b96b-ab2b63b2e3fa/).**


### Related Content
  * ##### [WSO2 Releases Agent Manager as Enterprises Look to Control Growing AI Agent Sprawl](https://www.infoq.com/news/2026/09/ws02-agent-manager/)
Sep 18, 2026
  * Icon
##### [Ontology‐Driven Observability: Building the E2E Knowledge Graph at Netflix Scale](https://www.infoq.com/presentations/netflix-observability-aiops-ontology-scale/)
Oct 09, 2026
  * Icon
##### [Beyond Observability: Evolving Production Operations in the Age of AI](https://www.infoq.com/presentations/ai-production-operations/)
Oct 01, 2026
  * ##### [CircleCI Makes Machine Runner Orchestrator Generally Available](https://www.infoq.com/news/2026/10/circleci-machine-runner/)
Oct 09, 2026
  * ##### [OpenTelemetry Makes Kubernetes Attributes Processor Stable as Observability Schema Matures](https://www.infoq.com/news/2026/10/opentelemetry-kubernetes-observ/)
Oct 06, 2026
  * ##### [Istio 1.31 Adds Agentgateway Waypoints and Moves Release Artifacts off Google Cloud](https://www.infoq.com/news/2026/10/istio-1-31-agentgateway/)
Oct 03, 2026
  * ##### [Kubernetes Teams Get a Safer Way to See Their Own GPU Metrics](https://www.infoq.com/news/2026/09/kubernetes-gpu-metrics/)
Sep 29, 2026
  * ##### [Uber Separates Scaling Intent from Execution on Kubernetes Platform](https://www.infoq.com/news/2026/09/uber-kubernetes-scaling/)
Sep 28, 2026
  * ##### [GKE Pod Snapshots Cut Model Load Times, and Move the Work to Snapshot Lifecycle Management](https://www.infoq.com/news/2026/09/gke-pod-snapshots-benchmarks/)
Sep 27, 2026


### Related Sponsors
  * #### [Gartner® Magic Quadrant™ for AI-Augmented Code Modernization Tools](https://www.infoq.com/vendorcontent/show.action?vcr=22755dd8-0de0-4bfd-ba36-37811c315d1c&pageType=NEWS_PAGE&vcrPlace=TS_SPONSORED_CONTENT_TOP)
The software lifecycle is being rebuilt around AI agents, but today's processes were designed for humans. Discover the first Gartner® Magic Quadrant™ for AI-augmented code modernization and the criteria shaping this emerging market.
  * #### [How Morgan Stanley Is Scaling Software Modernization with Automated Refactoring](https://www.infoq.com/vendorcontent/show.action?vcr=2c235f81-3999-49e9-b6d7-107b14946621&pageType=NEWS_PAGE&vcrPlace=TS_SPONSORED_CONTENT_TOP)
Morgan Stanley has used Moderne for nearly a year to automate large-scale refactoring and reduce technical debt. Explore their modernization journey, key successes and challenges, and how they plan to continue transforming software delivery at enterprise scale.
  * Sponsored by


### Related Content
  * ##### [Building a Collaborative Platform Culture in a Bank](https://www.infoq.com/news/2026/09/collaborative-platform-culture/)
Sep 24, 2026
  * ##### [Beyond Kubernetes at Modal: How to Scale 1 Million Concurrent Sandboxes in Seconds](https://www.infoq.com/news/2026/09/modal-scaling-sandboxes/)
Sep 23, 2026
  * ##### [Elastic Beanstalk Adds Cluster Mode, Running Applications on Shared EKS Clusters](https://www.infoq.com/news/2026/09/elastic-beanstalk-cluster-eks/)
Sep 23, 2026
  * ##### [Kubernetes 1.37 Released: Stable Metrics API and Rootless Kubelet in Beta](https://www.infoq.com/news/2026/09/kubernetes-1-37/)
Sep 20, 2026
  * ##### [Kubernetes Multi-Cluster Project Karmada Reaches CNCF Graduation](https://www.infoq.com/news/2026/09/karmada-kubernetes-cncf/)
Sep 17, 2026
  * ##### [Microsoft Open-Sources TauGrid to Simplify AI Workload Management on Kubernetes](https://www.infoq.com/news/2026/09/microsoft-taugrid-open-source/)
Sep 16, 2026


###  **The InfoQ** Newsletter
A round-up of last week’s content on InfoQ sent out every Tuesday. Join a community of over 250,000 senior developers. [View an example](https://assets.infoq.com/newsletter/regular/en/newsletter_sample/newsletter_sample.html)
[We protect your privacy.](https://www.infoq.com/privacy-notice/)
Close
