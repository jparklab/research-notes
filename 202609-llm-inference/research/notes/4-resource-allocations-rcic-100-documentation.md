---
title: 4. Resource Allocations — RCIC 1.0.0 documentation
id: 4-resource-allocations-rcic-100-documentation
tags:
- onprem-llm-inference-stacks-859099
- cc-gap-fill
created: '2026-10-23T18:16:30.017593Z'
updated: '2026-10-23T18:19:18.599779Z'
source: https://web3.oit.uci.edu/about/allocations.html
source_domain: web3.oit.uci.edu
fetched_at: '2026-10-23T18:16:29.998115Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'UC Irvine RCIC HPC3 documentation: recharge GPU rate $0.32/GPU-hour (proposed,
  in rate review) computed to recover hardware cost over 5 years at 60% use (4xL40S
  node ~$47K); ''campus funds pay for the people, the baseline infrastructure, and
  vendor maintenance'' so staff are explicitly excluded from the recharge. Evidence
  that real university recharge rates are hardware-only and therefore not staff-inclusive
  TCO; L40S not H100.'
---

#  4. Resource Allocations
## [4.1. Introduction](https://web3.oit.uci.edu/about/allocations.html#id16)
RCIC resources are funded and operated under a shared financial model of no-cost and recharge (pi-funded) allocations. 

**Every user**
    
[No-cost](https://web3.oit.uci.edu/about/allocations.html#no-cost-allocations) one time 1000 core-hour allocation. This allocation provides baseline level of computing cycles and is required to allow users to access the Slurm free queues. It enables users who are not affiliated with any research program to have meaningful access to HPC3. Most users should use lab accounts instead of this one-time allocation. 

**Faculty**
    
Faculty allocations are _PI-centered_ but the owners can grant access
to their allocations to students, postdocs, and other faculty members.
Allocations can not be combined into larger group or center accounts.
Faculty UCI researchers can have two types of allocations:
> [No-cost](https://web3.oit.uci.edu/about/allocations.html#no-cost-allocations) allocations
>> Can be requested to baseline levels of:
>> * **Computing cycles** : are reallocated every 6 months. See [Reallocation](https://web3.oit.uci.edu/hpc3/reallocations.html#reallocation) for details.
>> * **Campus Research Storage** ([CRSP](https://web3.oit.uci.edu/references/glossary.html#term-CRSP)): 1TB per researcher.
> [Recharge](https://web3.oit.uci.edu/about/allocations.html#recharge-allocations) allocations
>> Are used when the baseline is insufficient for a particular lab.
>> Researchers can purchase additional capacity using grant or other funds.
>> The recharge (PI-funded) allocations are available for:
## [4.2. No-cost](https://web3.oit.uci.edu/about/allocations.html#id17)
In general, the cost of administration of RCIC resources is covered in RCIC’s annual budget. RCIC is funded through central campus to provide a baseline allocations of centralized, research computing and data services.  
Table 4.1 **Summary of no-cost baseline allocations for campus researchers (faculty)**  
| Capability  | No-cost allocation  | How often  | How to expand  | Expansion costs  |  
| --- | --- | --- | --- | --- |  
|  **HPC3** CPU hours  | 100000 core-hrs  | Every 6 months  |  Cycle Purchase Hardware Purchase  |  $.01/core-hr ~$15K/node  |  
|  **HPC3** GPU hours  | 1000 GPU-hrs  | Every 6 months  |  Cycle Purchase Hardware Purchase  |  $.32/core-hr ~$47K/node  |  
| **CRSP storage**  | 1 TB  | Never Expires  | TB/year  | $50/TB/Year  |  
| **DFS storage**  | N/A  | N/A  | TB/ 5 years  | $100/TB/5 Years  |  
Table 4.2 **Summary of no-cost allocations for every user**  
| Capability  | No-cost allocation  | How often  | How to expand  | Expansion costs  |  
| --- | --- | --- | --- | --- |  
|  **HPC3** CPU hours  | 1000 core-hrs  |  One time allocation  | N/A  | N/A  |  
| **HOME storage**  | 50 GB  | Never Expires   | N/A  | N/A  |  
| **DFS storage**  | 1 TB  | Never Expires   | N/A  | N/A  |  
## [4.3. Recharge](https://web3.oit.uci.edu/about/allocations.html#id18)
When research program needs exceed this baseline, additional capacity can be purchased through recharge. It should be noted, that rates apply only to UCI researchers (faculty, research faculty, research staff).
The recharge tables below provides more details on costs and commitment. Proposed rates for CRSP and CPU/GPU hours are in the process of being approved by the UCI recharge rate review process.  
Table 4.3 **Storage Related Recharges**  
| Item  | Rate  | Commitment  | Notes  |  
| --- | --- | --- | --- |  
| CRSP  | $50/TB/Year  | Year  | Two-copy storage, available on campus network. Daily Backups available on campus network/VPN  |  
| DFS  | $100/TB/5 Years  | 5 Years  | Single copy, high-performance storage. Daily backups are accessible only from HPC3  |  
| Selective backup  | $100/TB/5 Years  | 5 Years  | Second copy of user-selected file systems on HPC3.  |  
Table 4.4 **HPC3 Computing Related Recharges**  
| Item  | Rate  | Commitment  | Notes  |  
| --- | --- | --- | --- |  
| Server installation  | $1000  | One-time  | Researcher-owned server can be added to HPC3  |  
| Server purchase  | ~$15K/CPU or ~$47K/CPU+GPU node  | Equipment warranty plus 1 year  | Hardware purchases add capacity for the owner. Prices are estimates. Exact pricing depends on node confdiguration and time of purchase.  |  
| CPU hours  | $0.01/core-hour  | Minimum: 10000 core-hours  | Prepaid core-hours valid for 1 year after purchase.  |  
| GPU hours  | $0.32/GPU-Hour  | Minimum: 250 GPU-hours  | Prepaid GPU-hours valid for 1 year after purchase.  |  
## [4.4. How to buy allocations](https://web3.oit.uci.edu/about/allocations.html#id19)
The recharge is billed annually via campus recharge and is computed on the basis of recovering the cost of additional space (for CRSP and DFS storage). These campus funds pay for the people, the baseline infrastructure, and vendor maintenance required to provide the robust infrastructure.
All allocation purchase requests need to be emailed by a PI to hpc-support@uci.edu telling us what allocations are desired. Please see sections below for details.
### [4.4.1. Computing](https://web3.oit.uci.edu/about/allocations.html#id20)
Computing allocation for _accounted jobs_ can be expanded through:
1. [Purchase core-hours](https://web3.oit.uci.edu/about/allocations.html#buy-core-hours) from RCIC.
2. [Purchase hardware](https://web3.oit.uci.edu/about/allocations.html#buy-hardware) that is placed into the cluster and converted into core-hours.
#### [4.4.1.1. Purchase core-hours](https://web3.oit.uci.edu/about/allocations.html#id21)
Please send a request to hpc-support@uci.edu telling us:
1. Type of core-hours (CPU or GPU)
2. How many core-hours you need
Core-hour purchases a done through an MOU on a prepaid card basis. RCIC does not post-bill for core hours and it is not possible to be surprised with a large bill at the end of a month.
Prepaid core hours are intended to be used within one calendar year. Unused prepaid hours are forfeit after 18 months. The current [HPC3 Computing Related Recharges](https://web3.oit.uci.edu/about/allocations.html#recharge-rates) are computed to recover the cost of hardware over a 5 year period at 60% use.
#### [4.4.1.2. Purchase hardware](https://web3.oit.uci.edu/about/allocations.html#id22) 

Hardware purchase process
    
**Hardware is purchased through RCIC and can be requested at any time**. When a large enough number of nodes have been requested (at least 4 CPU nodes and/or 1 GPU node), RCIC will obtain quote(s) from vendors for acceptable hardware. Your source of funds (grant or other) is used to fund your share of the purchase. You can only purchase whole nodes, but you may use multiple sources of funds. In this model, we easily support two different faculty splitting the cost of a single node.
Hardware is commodity-based and subject to market variability, prices as of 1Q 2025:
CPU nodes (48 cores) are approximately $15K.
GPU-nodes (4 x NVIDIA L40S) are approximately $47K.
Warning
You may not purchase hardware, send to the machine room, and then expect RCIC to integrate this hardware into HPC3. Any hardware purchased outside of the above process will not be integrated into HPC3 nor will it be managed by RCIC. 

Outline of Purchase Process
    
  1. Send a request to hpc-support@uci.edu indicating your interest in purchasing nodes (CPU or GPU and how many) and time frame.
  2. RCIC obtains quotes once enough requests have been aggregated.
  3. Upon your approval purchase is made and your funds are used at purchase time.
  4. $1000/node one-time integration fee is re-charged AFTER the hardware has arrived.



Hardware that you purchase is converted to core-hours allocation
     

For example, a 48-core node can deliver:
    
8760hoursyear∗48cores=420,480corehoursyear
At 95% this would become a 399,456 core-hour (SU) credit to a CPU Slurm Account 

For example, a 32-core, 4GPU node can deliver:
    
8760hoursyear∗32cores=280,320corehoursyear
8760hoursyear∗4GPUs∗32SUsGPU=1,121,280GPUhoursyear
280,320SU+1,121,280SU=1,401,600SU
At 95% this would become a 1,331,520 SU credit to a GPU Slurm Account
Note
SUs (Service Units) are not convertible between GPU and CPU accounts.
An hour of a GPU requires at least 2 CPU cores.
Hence, the minimum charge for a single GPU is 32+2=34SUshour. 

**The conversion rate is 95% of the theoretical core hours your hardware could deliver in a year** :
    
  * The 95% factor accounts for usual annual downtime through scheduled and unscheduled maintenance.
  * The 50% of this credit is applied during the 6 month reallocation each year the node is in the cluster (warranty period + year)



Effective costs over 6 years for purchased hardware
    
Using the above estimated costs of $15K for CPU node and $47K for a GPU node _and_ consuming all SUs every 6 month reallocation cycle the following cost estimates of core-hour cost and GPU-hour costs would be as follows:  
Table 4.5 **HPC3 Effective Costs for CPU and GPU hours**  
| Equivalence  | Calculation  | Effective rate  |  
| --- | --- | --- |  
| Core Hours  | 15000(6Yr∗399456SUyr)1SUhour  | $0.0062/Core-hour  |  
| GPU Hours (2 cores)  | 47000(6Yr∗1331520SUyr)34SUhour  | $0.20/GPU-hour  |  
| GPU Hours (8 cores)  | 47000(6Yr∗1331520SUyr)40SUhour  | $0.25/GPU-hour  | 

Purchasing of nodes in HPC3 does NOT give you a “private” queue
      
This means that some of your jobs may have to wait for resources, but it also gives you non-preempt access a larger number of cores than purchased. In the first 1.5 years of HPC3 operation, owners rarely wait for long periods of time.
Please see [Hardware FAQ](https://web3.oit.uci.edu/references/hardware-faq.html#hardware-faq) for additional info about hardware purchases.
### [4.4.2. Purchase CRSP storage](https://web3.oit.uci.edu/about/allocations.html#id23)
Please send a request to hpc-support@uci.edu telling us
1. How much CRSP space you need (TB)
2. For how long (years)
We will create an MOU and sent to the PI for signing and will ask for a KFS account number for recharge. Once a PI signs an MOU for the desired amount of storage, the allocation is created on the CRSP system.
### [4.4.3. Purchase DFS storage](https://web3.oit.uci.edu/about/allocations.html#id24)
Please send a request to hpc-support@uci.edu telling us
1. How much DFS space you need (TB)
2. If this is your first DFS purchase, then the initial term is 5 years.
3. If you have existing DFS quota, then then additional quota is priced at $20/TB/Year. The PI has the choice of a) extending existing quota 5 years or b) Setting the expiration data of new quota to co-terminate with the existing quota end-date
We will create an MOU and sent to the PI for signing and will ask for a recharge account. Once a PI signs an MOU for the desired amount of storage, the allocation is created on one of the DFS systems.
