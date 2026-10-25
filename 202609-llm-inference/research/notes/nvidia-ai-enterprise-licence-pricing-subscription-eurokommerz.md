---
title: NVIDIA AI Enterprise Licence, Pricing, Subscription | Eurokommerz
id: nvidia-ai-enterprise-licence-pricing-subscription-eurokommerz
tags:
- onprem-llm-inference-stacks-859099
- vendor-onprem
- nvidia-licensing
- derivative-of
created: '2026-10-23T17:22:22.718540Z'
updated: '2026-10-23T17:31:12.954349Z'
source: https://eurokommerz.at/blog/nvidia-ai-enterprise-licensing-explained/
source_domain: eurokommerz.at
fetched_at: '2026-10-23T17:22:22.691526Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'PROVENANCE: reseller blog (Eurokommerz, Vienna; updated 18 Oct 2026) - commercial
  interest in selling licences but cites NVIDIA guides precisely. Licence per physical
  GPU (MIG slices don''t change count; CPU-only server = 1/server). H100 PCIe/NVL
  and H200 NVL include 5-yr subscription (term starts 90 days after ship to OEM);
  L40S, L4, RTX PRO 6000 SE include none; DGX Hopper bundled, DGX B200/B300 bought
  separately. Only vGPU for Compute (C-series) is technically enforced: unlicensed
  VM drops to idle compute after 20 min; licensed VM losing server gets 7 days default;
  Delegated License Service supports air-gapped node-locked licences (since vGPU 15.0),
  clusterable to 9. Bare-metal/container needs no licence server. vLLM, TensorRT-LLM
  (Apache-2.0), Triton (BSD-3) run free; without licence you lose NIM Certified Production
  Branch (9 months stable API) and support (support only on NVIDIA-Certified Systems;
  Business Standard 4 business hours). Lifecycle: feature branch monthly, production
  branch 6-monthly/9 months support, LTS 30 months/3 yrs. VMware PAIF needs VCF +
  PAIF add-on + AI Enterprise, min 3 GPU ESX hosts; OpenShift AI NIM needs NGC key
  with AI Enterprise role; support matrix ESXi/RHEL/Ubuntu KVM/AHV, not Proxmox/Hyper-V.'
utility_score: 12.0
---

BLOG · GUIDE · 14 SEPTEMBER 2026 · UPDATED 18 OCTOBER 2026
# NVIDIA AI Enterprise licence: pricing, subscription terms, what it includes and who needs it
[Eurokommerz](https://eurokommerz.at/), Vienna, since 2006: [Private AI/ML](https://eurokommerz.at/private-ai-ml/) · [IT Managed Services](https://eurokommerz.at/it-managed-services/) · [Enterprise Training](https://eurokommerz.at/certification-training/) · [AI Hardware & Software](https://eurokommerz.at/ai-infrastructure/)
IN BRIEF
  * NVIDIA AI Enterprise is licensed per physical GPU, for every GPU in the server that runs any of its software; a four-GPU board needs four licences, a CPU-only server one per server
  * Subscriptions run one to five years with Business Standard support included; a perpetual licence exists but must be bought with five years of support
  * H100 PCIe, H100 NVL and H200 NVL ship with a five-year subscription that has to be activated by serial number; L40S, L4 and RTX PRO 6000 Server Edition ship with none
  * The only technically enforced part is vGPU for Compute: C-series vGPU profiles license only through AI Enterprise, and an unlicensed VM drops to idle-level compute after 20 minutes
  * Without the licence you can still run CUDA, vLLM, TensorRT-LLM and Triton; what you lose is the NIM Certified Production Branch, stable production branches and NVIDIA support


## What the AI Enterprise licence includes
NVIDIA AI Enterprise is licensed per physical GPU and sold as a subscription of one to five years, as a perpetual licence bought with five years of support or, in cloud marketplaces, per GPU per hour; pricing follows the GPU count, the term and the support level. The H200 NVL includes a five-year subscription, and the DGX Spark has a separate product, AI Enterprise for DGX Spark. The licence covers a software platform organised in two layers. The application layer holds NIM microservices, NeMo, Triton Inference Server, TensorRT and TensorRT-LLM, RAPIDS, the PyTorch container, Riva, DeepStream, Holoscan, TAO, Omniverse and NVIDIA’s pre-trained models, released in feature, production and long-term branches. The infrastructure layer holds the data-centre driver, the vGPU manager and guest driver, Fabric Manager, the GPU, Network, DPU and NIM operators for Kubernetes, Base Command Manager, the container toolkit and Run:ai, which NVIDIA’s documentation of 10 August 2026 lists as included both self-hosted and as SaaS.
Three things are exclusive to licence holders, according to NVIDIA’s own licensing guide: select NIM containers, the production and long-term support branches, and vGPU for Compute. The fourth is enterprise support with response targets, which for many buyers is the main reason to pay for the licence. Nearly everything else in the list also exists in open-source or freely downloadable form; self-hosted Run:ai is the exception, and NVIDIA’s free trial leaves it out. Our [AI Enterprise page](https://eurokommerz.at/software/nvidia-ai-enterprise/) covers the licences we supply; this article explains how they are counted and when you can do without them.
## How AI Enterprise licences are counted
You need one licence for every physical GPU in a server or workstation that hosts any AI Enterprise software. Boards with several GPUs count each GPU, so an A16 needs four licences. A CPU-only server or instance running the CPU parts of the stack needs one licence per server, regardless of how many processors it has. We found no mention of MIG slices in the licensing guide: partitioning a card into four instances does not change the count, which stays one per physical GPU.  
| FORM  | TERM  | SUPPORT INCLUDED  | NOTE  |  
| --- | --- | --- | --- |  
| Subscription  | 1, 2, 3, 4 or 5 years  | Business Standard  | must be renewed to stay active  |  
| Perpetual  | indefinite  | five years Business Standard, then renewal (one-year terms available)  | five years of support at purchase, per NVIDIA’s pricing table  |  
| Cloud marketplace  | per GPU per hour  | per marketplace terms  | bring-your-own-licence needs one subscription per GPU  |  
| Business Critical upgrade  | add-on to either  | round-the-clock, one-hour response on severity 1  | Business Standard targets four business hours  |  
Response targets per NVIDIA’s Enterprise Support and Services guide; support requires an NVIDIA-Certified System.
Two conditions apply. Support is available only on NVIDIA-Certified Systems, so a card in an uncertified chassis has a licence but no support path. And a five-year subscription does not extend the life of the software branch you deploy; the branch keeps its own end-of-support date.
## Which GPUs and systems include AI Enterprise
Some GPUs include a subscription in the price and others include nothing, so check this table before comparing the L40S and the H200 NVL on card price alone.  
| HARDWARE  | AI ENTERPRISE ENTITLEMENT  | WHAT TO DO  |  
| --- | --- | --- |  
| H100 PCIe, H100 NVL, H200 NVL  | five-year subscription per GPU  | activate with the GPU serial number on NGC; NVIDIA starts the term 90 days after the card ships to the server maker, activated or not  |  
| A800 40 GB Active  | three-year subscription  | same activation  |  
| H100 SXM, HGX B200 and B300 boards  | none documented (H100 SXM: add-on)  | budget one licence per GPU  |  
| DGX with Hopper GPUs  | included in the DGX software bundle  | confirm the term on the quote  |  
| DGX B200 and B300  | licensing guide: purchased separately; product pages, October 2026: “Recommended” in both specification tables  | have the quote state the entitlement explicitly  |  
| L40S, L4, RTX PRO 6000 Server Edition, RTX 6000 Ada  | none  | budget per GPU if you need vGPU compute, production NIM or support  |  
| DGX Spark  | separate product: AI Enterprise for DGX Spark  | other AI Enterprise entitlements do not automatically include it or its support  |  
Per NVIDIA’s licensing guide and product pages, as of October 2026. If a quote bundles AI Enterprise with the RTX PRO 6000 Server Edition, it is a purchased NVIDIA subscription, not an included one: server makers list a per-GPU NVIDIA software kit for this card, and NVIDIA’s product terms restrict such platform subscriptions to the platform they are sold for. Have its term stated.
The H200 NVL row changes comparisons the most. If a project needs AI Enterprise anyway, for vGPU compute profiles or for supported NIM in production, each H200 NVL carries five years of it. The [RTX PRO 6000 comparison](https://eurokommerz.at/blog/rtx-pro-6000-vs-h200-nvl-llm-inference/) then has to add eight licences to an eight-card server before the totals can be compared.
## NVIDIA AI Enterprise pricing and deployment options
NVIDIA sells AI Enterprise per GPU through two channels, which its licensing guide, last updated on 2 September 2026, describes. For self-managed systems, on-premise or at the edge on NVIDIA-Certified servers, NVIDIA’s pricing page lists the licence as available through the NVIDIA Partner Network, as a subscription of one to five years or as a perpetual licence with five years of support. For cloud-hosted systems, it is offered in the marketplaces of the large public clouds, where NVIDIA says it “is priced per GPU per hour on an on-demand/pay-as-you-go basis”.  
| DEPLOYMENT  | HOW NVIDIA SELLS IT  | SUPPORT  |  
| --- | --- | --- |  
| Self-managed, on-premise  | per GPU through NVIDIA partners: subscription of 1 to 5 years, or perpetual with 5 years of support  | Business Standard included; Business Critical and a Technical Account Manager as upgrades  |  
| Cloud marketplace, production  | pay as you go, per GPU per hour, plus the cloud instance  | limited to three calls  |  
| Cloud private offer  | subscription of 1 to 3 years, custom quote  | NVIDIA AI Enterprise Support  |  
| Cloud, development  | no term, free or bring your own licence  | developer forum and Discord  |  
Per NVIDIA’s AI Enterprise licensing guide and pricing page, both last updated on 2 September 2026; the cloud provider bills the instance itself.
Some hardware comes with the licence. Each H200 NVL includes a five-year subscription, as do the H100 PCIe and H100 NVL, and NVIDIA starts the term 90 days after the card ships to the server maker, a date that cannot be changed. DGX systems with Hopper GPUs carry AI Enterprise in the DGX software bundle; for DGX systems with Blackwell GPUs, the guide states that the licences “need to be purchased separately”. The DGX Spark has a product of its own, which our guide to [NVIDIA AI Enterprise for DGX Spark](https://eurokommerz.at/blog/nvidia-ai-enterprise-for-dgx-spark/) covers.
The price of an AI Enterprise licence depends on the number of physical GPUs, on subscription or perpetual, on the term and on the support level, and a GPU that already includes a subscription needs no second one for that term. A CPU-only server counts one licence per server, however many processors it has. In the cloud, the hourly charge comes in addition to the instance, and the guide notes that not all components are available on every cloud deployment.
We quote AI Enterprise per GPU with the hardware it runs on, on one EU contract and invoice. [Send us the GPU models and count, the term and the support level](https://eurokommerz.at/blog/nvidia-ai-enterprise-licensing-explained/#contact) through the form below, and we reply within one business day with a written quote.
## vGPU for Compute: the only part the driver enforces
Most of the platform runs whether or not a licence server can be reached: NVIDIA’s own words are that the software “runs with or without a valid license server connection”, and the licence system is only required when vGPU for Compute drivers are in use. That case is enforced in the driver, which is why it matters most to virtualisation teams.
C-series profiles, the compute vGPU types such as L40S-48C or DC-8C on the RTX PRO 6000 Server Edition, are licensed only through AI Enterprise. The RTX vWS, vPC and vApps licences that virtual-desktop teams already own do not cover them. Each C-series vGPU assigned to a VM checks out a licence at boot and returns it at shutdown; one per-GPU licence covers up to 16 vGPU instances on that GPU, or one vGPU that takes the whole card. NVIDIA’s H200 NVL profile tables allow 20 or 32 small vGPUs per card, and the guide does not say how those are licensed, so ask before designing more than 16 tenants on one GPU.  
| STATE  | WHAT HAPPENS  |  
| --- | --- |  
| VM boots without a licence  | 20 minutes at full capability, then compute reduced to an idle level until a licence is acquired  |  
| Licensed VM loses the licence server  | 7 days of normal operation by default on the cloud service, configurable on an on-premises server, then degraded  |  
| Licence acquired after degradation  | full capability returns immediately  |  
| Bare-metal or container deployment  | no licence server needed at all  |  
The licence server itself comes in two forms. The Cloud License Service is hosted on NVIDIA’s licensing portal and needs outbound HTTPS. The Delegated License Service is a virtual appliance on your network, clusterable up to nine instances, with node-locked licences for air-gapped hosts available since vGPU software 15.0. Clients carry a client configuration token generated on the service instance. In daily operation, the more likely problem is a firewall change that cuts the VMs off from the licence server, which, on the cloud service’s default, becomes visible only seven days later.
## What runs without an AI Enterprise licence
The CUDA toolkit is free under its EULA. TensorRT-LLM and vLLM are licensed under Apache-2.0, Triton Inference Server under BSD-3, and NeMo is open source. A single-tenant inference server running vLLM on eight RTX PRO 6000 cards needs no AI Enterprise licence to run, and many such servers run without one.
Without the licence, NIM containers are available to developers for research and testing on up to 16 GPUs, and NVIDIA’s NIM FAQ, last modified on 6 August 2026, states: “Using NIM in production requires an NVIDIA AI Enterprise license.” NVIDIA’s NIM offerings page of 6 October 2026 lists the NIM Certified Feature Branch “at no charge under the applicable license terms” and the Production Branch as requiring a subscription, so have the quote state which offering a production server rests on. You also get the monthly feature branch instead of a production branch with nine months of stable APIs, and no NVIDIA support contract for a driver bug in production. A 90-day trial licence exists for the move to production; it needs an NVIDIA-Certified server and excludes Run:ai.
With one or two GPUs, one team, an open-source serving stack and no VMs sharing the card, the licence is optional and mainly buys support. With C-series compute vGPUs shared by several teams in virtual machines, or NIM in production, it is required, and an eight-card server needs eight licences, regardless of the card price.
We supply AI Enterprise and vGPU licences and quote them with the hardware. If you are not sure which case is yours, [describe the GPUs, the VMs and the serving stack](https://eurokommerz.at/blog/nvidia-ai-enterprise-licensing-explained/#contact) in the form below, and we tell you whether the licence is needed.
## Per user or per GPU: the two licence families
The same card can carry two different licence models depending on the profile type, and mixing them up is the most common mistake we see on quotes.  
| PRODUCT  | PROFILE SERIES  | LICENSED PER  | TYPICAL USE  |  
| --- | --- | --- | --- |  
| NVIDIA RTX vWS  | Q-series  | concurrent user  | CAD, visualisation, virtual workstations  |  
| NVIDIA vPC  | B-series  | concurrent user  | office virtual desktops  |  
| NVIDIA vApps  | A-series  | concurrent user  | published applications  |  
| NVIDIA AI Enterprise  | C-series and MIG-backed compute  | physical GPU  | compute VMs (C-series), NIM, supported stack  |  
NVIDIA’s vGPU licensing guide lists CUDA and AI development under RTX vWS for Q-series desktops; C-series compute vGPUs are licensed only through AI Enterprise, so a Q-series and a C-series profile on the same card are two different licences.
Our guide on [MIG and vGPU counts](https://eurokommerz.at/blog/mig-vgpu-how-many-vms-per-gpu/) covers how many VMs each card holds; this table shows which licence, and so which budget line, each VM needs.
## Release branches and end-of-support dates
AI Enterprise separates the infrastructure layer from the applications, and each has its own branch cadence. On the application side, feature branches ship monthly with one month of support and may change APIs, production branches ship every six months with nine months of support and monthly security patches, and long-term support branches arrive every 30 months with three years of support and quarterly patches. Infrastructure releases are supported for one year, or three when NVIDIA designates a release as long-term, which it does at least once every two years and at least once per GPU architecture.  
| BRANCH  | CURRENT  | END OF SUPPORT  |  
| --- | --- | --- |  
| Infrastructure 8 (production branch, R595 driver)  | 8.2, August 2026  | April 2027 (planned)  |  
| Infrastructure 7 (long-term, R580 driver)  | 7.8, August 2026  | July 2028 (planned)  |  
| Application production branch 6  | May to December 2026 releases  | January 2027  |  
| Application long-term branch 2  | November 2024 to August 2027 releases  | October 2027  |  
Per NVIDIA’s AI Enterprise lifecycle pages and the planned end-of-life dates in its AI Enterprise documentation, as of October 2026. NVIDIA’s driver table ends the R595 and R580 driver branches a month earlier, in March 2027 and June 2028. Infrastructure 8.0 dropped V100, RTX A4000, RTX 4000 SFF Ada and the Quadro RTX 8000, 6000 and 4000 from vGPU for Compute; they stay supported on 7.x.
A regulated environment that needs three-year API stability belongs on the long-term branch on infrastructure 7.x. The long-term application branch reaches end of support in October 2027 and infrastructure 7.x in July 2028, as planned by NVIDIA, so the upgrade should already be in the plan. The Triton container with the TensorRT-LLM backend is no longer in production branch 6; deployments move to the vLLM backend variant.
## Where VMware, Red Hat and Nutanix put the licence
Broadcom’s requirements for [VMware Private AI Foundation with NVIDIA](https://eurokommerz.at/blog/vmware-private-ai-foundation-with-nvidia/) on VCF 9.1, updated on 21 September 2026, list three licences: the VCF subscription, a Private AI Foundation add-on licence and, for vGPU, NVIDIA AI Enterprise for the ESX host driver, the guest drivers and the NGC container images, plus a licence server or portal token. Broadcom’s VCF 9.1 licensing documentation adds that since 3 November 2025 a VCF subscription may also include a Private AI Foundation licence, so have the VMware quote confirm whether a separate line applies. For DirectPath GPUs, Broadcom documents access without the AI Enterprise requirement, though AI Enterprise software running on them still needs its per-GPU licence. Broadcom’s minimum is three GPU-enabled ESX hosts in the initial cluster of the workload domain. Red Hat OpenShift AI enables NIM with an NGC key from an account that carries the AI Enterprise Viewer role, so the entitlement comes from NVIDIA, not from Red Hat. Nutanix Enterprise AI consumes NIM with your own NIM token and states that NIM is licensed through AI Enterprise. In every case the per-GPU licence is a separate line from the platform, and the [VMware core count](https://eurokommerz.at/blog/vmware-licensing-after-broadcom/) is a separate line again.
## Common licensing mistakes we come across
Cards are ordered without the licence, and VMs on vGPU compute profiles drop to idle-level compute after 20 minutes; the fix is a per-GPU purchase, not a per-VM one. VMs are counted instead of GPUs, or the vWS licences already in the environment are assumed to cover C-series compute vGPUs. A board with several GPUs is counted as one. Included subscriptions are never activated because nobody registered the serial numbers. A DGX Spark is bought on the assumption that the department’s AI Enterprise licence covers it, although NVIDIA says other entitlements do not. The hypervisor is outside the support matrix: version 8.2 lists ESXi, Red Hat and Ubuntu KVM and Nutanix AHV, and Proxmox, Hyper-V and XenServer are not in the table. And AI Enterprise includes the DOCA driver for networking on BlueField adapters, so have the quote say whether the adapters need support of their own.
## What we supply
We supply [NVIDIA AI Enterprise subscriptions and perpetual licences](https://eurokommerz.at/software/) across the EU, on one EU contract and invoice with the GPUs they belong to. If you tell us the use case and the GPUs, we recommend the edition and term and flag what you do not need. For the [H200 NVL](https://eurokommerz.at/ai-infrastructure/h200-nvl/) the quote states the five-year subscription that comes with each card; for [RTX PRO 6000](https://eurokommerz.at/gpu/rtx-pro-6000/) servers we derive the licence lines from the vGPU plan: AI Enterprise per GPU for compute profiles, vWS per user for desktops. Prices depend on the form, the GPU count and the term, so they come with the quote.
## FAQ
Is NVIDIA AI Enterprise licensed per GPU or per server?
Per physical GPU: every GPU in a server or workstation that runs any of the software needs a licence, and a board with several GPUs needs one per GPU. Only CPU-only servers are licensed per server.
Which GPUs include an AI Enterprise subscription?
The H100 PCIe, H100 NVL and H200 NVL include five years; the A800 40 GB Active three years. They must be activated with the GPU serial number on NGC. The L40S, L4 and RTX PRO 6000 Server Edition include nothing.
Do I need it to run vLLM or Triton on my own server?
No. CUDA, vLLM, TensorRT-LLM and Triton are free to run on bare metal. You need the licence for vGPU compute profiles in VMs, for the NIM Certified Production Branch and, per NVIDIA’s NIM FAQ, for NIM in production, for production branches and for NVIDIA support.
What happens to a vGPU VM without a licence?
It runs at full capability for 20 minutes, then compute drops to an idle level until a licence is acquired. A licensed compute VM that loses contact with the licence server keeps working for seven days by default on the cloud service; the period is configurable on an on-premises DLS.
How is NVIDIA AI Enterprise pricing calculated?
Per physical GPU, by the form and term (a subscription of one to five years or a perpetual licence with five years of support), by the support level and by whether the GPUs already include a subscription, as each H200 NVL does. In cloud marketplaces NVIDIA charges per GPU per hour in addition to the cloud instance. We quote the licence with the hardware, so the price comes with the written quote.
What is the NVIDIA AI Enterprise licence for DGX Spark?
NVIDIA AI Enterprise for DGX Spark is a separate entitlement that brings NVIDIA enterprise support for the software stack on the DGX Spark, and NVIDIA’s DGX Spark documentation states that other AI Enterprise entitlements do not automatically include it or its support. NVIDIA’s product terms restrict a subscription bought for DGX Spark to that platform. NVIDIA offers a 90-day evaluation licence on request, with community support only.
Send us the GPU models and count, the hypervisor and how many VMs or users will share each card. We reply within one business day with the licence count, what your GPUs already include and a written quote.
[Talk to an expert](https://eurokommerz.at/blog/nvidia-ai-enterprise-licensing-explained/#contact)
Talk to an expert
We reply within one business day
request@eurokommerz.at
Jordangasse 7, 1010 Vienna
