---
title: AI Demand Surges as Billions in Compute Remain Locked
id: ai-demand-surges-as-billions-in-compute-remain-locked
tags:
- onprem-llm-inference-stacks-859099
- utilization
created: '2026-10-23T18:15:07.476295Z'
updated: '2026-10-23T18:17:21.370752Z'
source: https://www.datacenterknowledge.com/infrastructure/ai-demand-surges-as-billions-in-compute-remain-locked
source_domain: www.datacenterknowledge.com
fetched_at: '2026-10-23T18:15:07.450350Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Data Center Knowledge (May 2026) on Cast AI 2026 State of Kubernetes Optimization:
  ~23,000 cloud K8s clusters, 5% avg GPU util (vendor baseline, pre-optimization);
  Tekonyx analyst 15-25% for K8s AI clusters; skepticism from Constellation. Cloud
  clusters, not an on-prem named cluster.'
---

# AI Demand Surges as Billions in Compute Remain Locked
A Cast AI report finds low GPU, CPU, and memory utilization across enterprise Kubernetes clusters.
[Shane Snider,](https://www.datacenterknowledge.com/author/shane-snider)Senior News Writer,Data Center Knowledge
May 5, 2026
5 Min Read
Most enterprises struggle with data readiness and production architecture, leaving expensive GPU resources largely idle.Alamy
Cloud providers are racing to build more AI capacity as demand soars. New data suggests that much of that compute remains lightly used.
The push comes as Amazon, Microsoft, Alphabet, and Meta plan up to $725 billion in capital expenditures in 2026, according to Q1 2026 company guidance.
In contrast, a report from Cast AI showed surprisingly low utilization across enterprise environments.
The [2026 State of Kubernetes Optimization Report](https://cast.ai/reports/state-of-kubernetes-optimization/) from Cast AI, which sells Kubernetes optimization software, analyzed about 23,000 clusters running on Amazon Web Services, Microsoft Azure, and Google Cloud. The study found average GPU utilization of just 5% across enterprise Kubernetes clusters. CPU utilization averaged 8%, down from about 10% a year earlier, and memory utilization averaged about 20%, down from 23%.
Holger Mueller, vice president and principal analyst at Constellation Research, said the findings may reflect a specific slice of enterprise workloads rather than overall AI utilization. “It is interesting, but I think it is a misnomer,” he said. “It depends on what workloads are being measured.”
Related:[Speed to Power: How Developers Are Restructuring for AI Demand](https://www.datacenterknowledge.com/energy-power-supply/speed-to-power-how-developers-are-restructuring-for-ai-demand)
Cast AI's measurements reflect observed usage in production environments before any optimization or autoscaling – effectively a baseline snapshot of enterprise AI deployments. The dataset spans enterprise Kubernetes environments across industries and workload types, but excludes hyperscaler-operated infrastructure or internal AI training clusters.
Sid Nag, president and chief research officer at Tekonyx, said enterprise environments typically run at higher levels, but still well below capacity. “I see about 15% to 25% utilization in Kubernetes-based AI clusters,” he told Data Center Knowledge, adding that most organizations are using only a fraction of deployed GPU capacity.
Dave McCarthy, research vice president, cloud and edge infrastructure services at IDC, said the figures point to a broader efficiency issue. “Those numbers seem low to me but are emblematic of an emerging AI concern: resource efficiency,” he said.
## Enterprise AI Deployment Remains Early
The findings appear less dramatic given the current state of enterprise AI deployment.  that about 62% of organizations are experimenting with or piloting AI agents; roughly 23% are scaling them in at least one function; and fewer than 10% have scaled enterprise-wide.
Research from  shows AI is now a top investment priority – 43% of executives list it among their leading priorities for 2026 – but many are still working out how to deploy at scale and measure its value.
Related:[Data Center World 2026: AI Pushes Infrastructure to New Limits](https://www.datacenterknowledge.com/build-design/data-center-world-2026-ai-pushes-infrastructure-to-new-limits)
Nag argued that the gap between real demand and execution reflects broader readiness challenges. “The real bottleneck isn’t model capability – it’s the lack of production-grade architecture, data readiness, and operating models needed to turn AI from experimentation into enterprise-wide systems,” he said.
Supporting that view, a recent report from  found that only about 14% of organizations consider their data architecture AI-ready, with scalability and performance cited as primary barriers.
## Requested Resources Exceed Actual Usage
Cast AI also identifies a mismatch between what workloads request and what they consume. Overprovisioning of CPU, memory, and GPUs can leave nodes underutilized even when clusters appear fully allocated.
Mueller said the scope of Kubernetes-based measurements may not capture the full picture. “Workloads are shifting to AI, but the question is whether they are being managed as Kubernetes clusters and visible to platforms like Cast AI,” he said.
In many Kubernetes environments, GPUs are assigned as discrete resources, limiting how capacity can be shared across jobs. The report also points to scheduling behavior and autoscaling as factors influencing utilization, including fragmented capacity across nodes and scale-down policies that allow idle resources to persist.
Related:[GPU Repurposing Strategies: From Sunk Cost to Cash Flow](https://www.datacenterknowledge.com/servers/5-ways-to-repurpose-data-center-gpu-hardware)
McCarthy said inefficiencies often stem from upstream constraints and allocation behavior. “In some cases, GPUs are going idle because the storage systems feeding them can’t keep up,” he said. “In other cases, organizations are locking up capacity even if they aren’t fully utilizing it due to perceived scarcity. Just like the pandemic run on toilet paper, this behavior exacerbates the capacity problem.”Nag said the problem is systemic.
“This isn’t a hardware problem – it’s a systems problem,” Nag said, pointing to overprovisioning, weak scheduling, and data pipeline bottlenecks that keep GPUs idle. “Enterprise GPU utilization today is less about silicon scarcity and more about orchestration maturity.”
Mueller said enterprise deployment patterns may also skew utilization lower. “Organizations are buying servers with GPUs and not fully using them,” he said. “That does not look the same in public cloud environments, where AI workloads are more actively run.”
## Hyperscaler Earnings Show AI Demand Growth
The findings come as hyperscale cloud providers continue to report growth tied to AI workloads.
In prior Data Center Knowledge:
  * Microsoft said that  year over year and its AI business reached a $37 billion annualized revenue run rate. Commercial remaining performance obligations were $627 billion, up 99% year over year, reflecting contracted services that have not yet been delivered.
  * Amazon reported $37.6 billion in AWS revenue, up 28% year over year.
  * Alphabet Inc. reported about $20 billion in Google Cloud revenue, up 63% year over year.
  * Meta Platforms reported capital expenditures of $19.8 billion.


These figures point to sustained demand for AI infrastructure, but they say little about how efficiently capacity is used.
McCarthy said current consumption patterns can mask inefficiencies. “Hyperscalers continue to bill for these resources whether or not they are fully utilized, which is driving revenue,” he said. “Growing cloud bills will get the attention of the CFO, who will demand an explanation. That will push teams to evaluate usage patterns and implement governance controls to manage costs.”
## Utilization Diverges at Scale
Nag said utilization patterns diverge sharply at scale. “In large, optimized AI data centers, utilization is more like 60% to 70%,” he said, contrasting typical enterprise deployments with hyperscaler systems.
In fact, hyperscalers and large AI labs often report far higher figures on dedicated clusters. Meta's Research SuperCluster (RSC-1) reported 83% to 85% GPU utilization, according to an . Salesforce, running workloads on Google Cloud, reported  from 48% to nearly 100% through advanced storage and scheduling optimizations.
## The Split: Demand vs. Deployment
The disparity between surging spend and low enterprise Kubernetes utilization highlights a split between how AI infrastructure is consumed versus how demand is measured. Hyperscalers can keep adding capacity in anticipation of growth and backlog conversion, even as many enterprises underuse what they've provisioned.
Nag said organizations that treat GPUs as fixed capacity will continue to overprovision, while those that treat them as shared, schedulable resources can reduce waste and scale more efficiently.
## About the Author
Senior News Writer, Data Center Knowledge
Shane Snider is Senior News Writer at Data Center Knowledge, covering AI infrastructure, hyperscale data centers, cloud platforms, and the power and energy systems driving modern compute expansion. His reporting focuses on the operational, economic, and environmental forces reshaping digital infrastructure, including AI factories, utility constraints, liquid cooling, renewable energy procurement, and next-generation data center architectures. He has won recent Azbee awards for news series and government reporting. Based in Raleigh, North Carolina, Snider covers how hyperscalers, utilities, chipmakers, and infrastructure providers are responding to the rapid rise of AI workloads and global compute demand.
You can reach Shane at shane.snider@informatechtarget.com or on .
## Want more Data Center Knowledge stories in your Google search results?
[Add Us Now](https://google.com/preferences/source?q=https://www.datacenterknowledge.com/)
Subscribe to the Data Center Knowledge Newsletter
Get analysis and expert insight on the latest in data center business and technology delivered to your inbox daily.
### Recommendations
#### [ZTE Launches M8000 Micro-Module Data Center for AIDC Deploy Oct 22, 2026](https://www.datacenterknowledge.com/next-gen-data-centers/zte-launches-m8000-micro-module-data-center-for-aidc-deploy#recipe=sponsored-items&source_content_id=76b54744cf915bc6fa92440d4c392420)
#### [GPU Lifespan in Data Centers: Physical vs. Economic by Christopher Tozzi Sep 08, 2026](https://www.datacenterknowledge.com/data-center-chips/gpu-lifespan-in-data-centers-physical-vs-economic#recipe=trending-items&source_content_id=aacf2e5c88edf3cc52e78bfb7f74e187)
#### [Data Center Events 2026: Previewing the Biggest Data Center Conferences of the Year by Henry Chapman Feb 05, 2026](https://www.datacenterknowledge.com/data-center-construction/data-center-events-2026-previewing-the-biggest-data-center-conferences-of-the-year#recipe=trending-items&source_content_id=993f42407ed547dac6b4978a00c2a473)
#### [Data Center Cooling Methods: Costs vs. Efficiency vs. Sustainability by Christopher Tozzi Oct 31, 2025](https://www.datacenterknowledge.com/cooling/data-center-cooling-methods-costs-vs-efficiency-vs-sustainability#recipe=trending-items&source_content_id=aa59bf59ee958c5f750fb0e332c40430)
### Editor's Choice
[See all](https://www.datacenterknowledge.com/)
[PJM Strategy Targets Data Center Growth, but Policy Gaps Remain](https://www.datacenterknowledge.com/energy-power-supply/pjm-5-year-strategy-wrestles-with-data-center-boom)
[PJM Strategy Targets Data Center Growth, but Policy Gaps Remain](https://www.datacenterknowledge.com/energy-power-supply/pjm-5-year-strategy-wrestles-with-data-center-boom)
Aug 21, 2026
[Can Large-Load Flexibility Ease Data Center Energy Concerns?](https://www.datacenterknowledge.com/energy-power-supply/can-large-load-flexibility-ease-data-center-energy-concerns)
[Can Large-Load Flexibility Ease Data Center Energy Concerns?](https://www.datacenterknowledge.com/energy-power-supply/can-large-load-flexibility-ease-data-center-energy-concerns)
Aug 19, 2026
[Home-Based GPU Networks: Viable Supplements to AI Data Centers?](https://www.datacenterknowledge.com/edge-data-centers/home-based-gpu-networks-viable-supplements-to-ai-data-centers-)
[Home-Based GPU Networks: Viable Supplements to AI Data Centers?](https://www.datacenterknowledge.com/edge-data-centers/home-based-gpu-networks-viable-supplements-to-ai-data-centers-)
Aug 18, 2026
[APAC Data Center Pipeline Surges as AI Pre-Leasing Tightens Supply](https://www.datacenterknowledge.com/data-center-construction/apac-data-center-pipeline-surges-as-ai-pre-leasing-tightens-supply)
[APAC Data Center Pipeline Surges as AI Pre-Leasing Tightens Supply](https://www.datacenterknowledge.com/data-center-construction/apac-data-center-pipeline-surges-as-ai-pre-leasing-tightens-supply)
Aug 17, 2026
Want more Data Center Knowledge stories in your Google search results?
Subscribe to the Data Center Knowledge Newsletter
Get analysis and expert insight on the latest in data center business and technology delivered to your inbox daily.
### Industry Voices
[Can Evaporative Cooling in Data Centers Still Make Sense?](https://www.datacenterknowledge.com/cooling/can-evaporative-cooling-in-data-centers-still-make-sense-)
[Can Evaporative Cooling in Data Centers Still Make Sense?](https://www.datacenterknowledge.com/cooling/can-evaporative-cooling-in-data-centers-still-make-sense-)
Oct 23, 2026
[Nvidia Unveils DSX POD Amid Greenpeace Protest](https://www.datacenterknowledge.com/ai-data-centers/nvidia-unveils-dsx-pod-amid-greenpeace-protest)
[Nvidia Unveils DSX POD Amid Greenpeace Protest](https://www.datacenterknowledge.com/ai-data-centers/nvidia-unveils-dsx-pod-amid-greenpeace-protest)
Oct 22, 2026
Data Center World POWER
## Join the entire data center ecosystem September 21 – 23, 2026, at the Gaylord Texan Resort & Convention Center in Dallas, TX. 
[Learn More](https://datacenterworld.com/innovation-challenge/?_mc=hsad_dcw_em_dcw_le_x_dck_earlybird_2026&utm_source=housead&utm_medium=hsad_&utm_campaign=dck_earlybird&utm_content=le_)
