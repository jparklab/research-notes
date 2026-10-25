---
title: Great Lakes HPC Cluster Service Rates | U-M Information and Technology Services
id: great-lakes-hpc-cluster-service-rates-u-m-information-and-technology-services
tags:
- onprem-llm-inference-stacks-859099
- cc-gap-fill
created: '2026-10-23T18:16:40.006856Z'
updated: '2026-10-23T18:19:18.816512Z'
source: https://its.umich.edu/advanced-research-computing/high-performance-computing/great-lakes/rates
source_domain: its.umich.edu
fetched_at: '2026-10-23T18:16:39.985749Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'U-Michigan Great Lakes cost-recovery rates: gpu partition $0.00273912/min
  ($118.33/month per GPU) with 20 cores/90GB; spgpu $78.10/month; explicitly excludes
  any unit-provided support. Older GPU generations, not H100.'
---

[ Skip to main content ](https://its.umich.edu/advanced-research-computing/high-performance-computing/great-lakes/rates#main-content)
#  Great Lakes HPC Cluster Service Rates
The rates below represent cost recovery for the Great Lakes HPC Cluster and have been approved for use by those using the system for a fee. These rates do not include any support your unit may choose to provide.
**For**[**UMRCP**](https://its.umich.edu/advanced-research-computing/research-computing-package)**users:** as a reminder, be sure to set your jobs to use your [UMRCP](https://its.umich.edu/advanced-research-computing/research-computing-package) allocation whenever possible to avoid being unnecessarily charged. The rates below reflect costs for jobs run after the allocation is exhausted.  
| Partition  | Rate Per Minute  | Rate Per Month  | CPU Cores  | Memory Unit  | GPU Unit  |  
| --- | --- | --- | --- | --- | --- |  
| standard/debug/viz  | $0.000250463  | $10.82  | 1  | 7 GB  | N/A  |  
| largemem  | $0.000770370  | $33.28  | 1  | 41.75 GB  | N/A  |  
| gpu  | $0.002739120  | $118.33  | 20  | 90 GB  | 1  |  
| spgpu  | $0.001807870  | $78.10  | 4  | 48 GB  | 1  |  
| gpu-rtx6000  | $0.003861015  | $166.80  | 16  | 192 GB  | 1  |  
| gpu_mig40  | $0.002739120  | $118.33  | 8  | 125 GB  | 1  |  
| Faculty Owned Equipment  | $68.03 / node / month  |  
The monthly rate is based on a 30-day month; per-minute rates are included for reference. Charges are based on the percentage of the machine your job requests (in terms of the amount of cores, memory, and GPUs) and its actual runtime.
## Estimating Maximum Job Costs
To calculate the cost of a job enter one of the commands below in [Command Line](https://documentation.its.umich.edu/node/5013). The estimated cost is based on CPU, memory, GPU, and partition selection. The amount returned is the most a job could cost and does not take into account any unit support or [UMRCP](https://its.umich.edu/advanced-research-computing/research-computing-package) allocations that you have available for use.
`my_job_estimate --help`
`my_job_estimate --cores 20 --memory 10gb --partition standard`
## Other Unit Cost Sharing Information
Some units on campus provide additional funding in support of researchers for their units. Below are a list of links where you can find out if your college has additional funding you can leverage in your research: 
  * [College of Engineering Funding](https://caen.engin.umich.edu/rc/account/cost-share/)
  * [Medical School Funding](https://michmed.org/rcp)
  * [College of Literature, Science, & the Arts Funding](https://teamdynamix.umich.edu/TDClient/47/LSAPortal/KB/ArticleDet?ID=12207)
  * School of Public Health Funding


