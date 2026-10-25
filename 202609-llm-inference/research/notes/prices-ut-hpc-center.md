---
title: Prices | UT HPC Center
id: prices-ut-hpc-center
tags:
- onprem-llm-inference-stacks-859099
- cc-gap-fill
created: '2026-10-23T18:16:47.794490Z'
updated: '2026-10-23T18:19:19.034125Z'
source: https://hpc.ut.ee/pricing/overview
source_domain: hpc.ut.ee
fetched_at: '2026-10-23T18:16:47.774374Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'University of Tartu HPC Centre prices valid from 1 Jul 2026: A100/L40S/Tesla
  0.5 EUR/GPU/h, RTX6000 1.0, H200 and B200 2.0 EUR/GPU/h (B200/RTX6000 public sector
  only); LUMI GPU 0.35 EUR/h. Internal public-sector rates are far below commercial
  H200 rental; whether staff included is unstated.'
---

# Prices
The table below indicates the prices of HPC Centre's services for the structural units of University of Tartu and for external users. VAT will be added to all prices for user accounts outside the University of Tartu. 
The prices are set in accordance with the University of Tartu order No. 3-1/AT/108, and are valid from July 1st, 2026. The income financial source for billing with VAT is WLTATHPC. The income financial source for billing without VAT is ELTATHPC. The Manager of the HPC Center, Ivar Koppel, is the party responsible for the billing. 
Please don’t hesitate to contact us at support@hpc.ut.ee, if you have any questions about pricing.   
| HPC Compute servers (Rocket)  | 
  * **CPU** 0.012 EUR/core/h
  * **Memory** 0.012 EUR/6GB/h
  * **GPU** from 0.5 EUR/GPU/h (see next section)
  * Memory usage is calculated by 6 GB segments. 
  * When determining the order of starting compute jobs, jobs with higher priority are given preference. 

 | [Calculate your costs](https://hpc.ut.ee/pricing/calculate-costs#computing)  |  
| --- | --- | --- |  
| GPU  | 
  * **Tesla** 0.5 EUR/GPU/h
  * **A100** 0.5 EUR/GPU/h
  * **L40S** 0.5 EUR/GPU/h
  * **RTX6000*** 1.0 EUR/GPU/h
  * **B200*** 2.0 EUR/GPU/h
  * **H200** 2.0 EUR/GPU/h
  * * RTX6000 and B200 are only available for the Estonian public sector (including universities).

 | [Calculate your costs](https://hpc.ut.ee/pricing/calculate-costs#gpu-usage)  |  
| HPC Storage space  | 
  * **HPC Storage** 80 EUR/1TB/year
  * A copy stored on tape costs 30 EUR/1TB/year
  * High speed GPFS attached directly to the HPC cluster. Exportable to other systems using either NFS or Samba.

 | [Calculate your costs](https://hpc.ut.ee/pricing/calculate-costs#storage)  |  
| S3 object storage  | 
  * **Storage** 160 EUR/1TB/year
  * NB! The data retrieval and data transfer are not charged!

 | [Calculate your costs](https://hpc.ut.ee/pricing/calculate-costs#s3-storage)  |  
| LUMI (Large Unified Modern Infrastructure)  | 
  * **CPU** 0.008 EUR/core/h
  * **GPU** 0.35 EUR/h
  * **User home directory** free
  * **Project storage** (persistent and scratch storage) 0.0106 EUR/TB/h
  * **Flash storage** (flash based scratch storage) 10 x 0.0106 EUR/TB/h

 | [Calculate your costs](https://hpc.ut.ee/pricing/calculate-costs#lumi)  |  
| Virtual Machine (Cloud service)  | 
  * **CPU** 0.003 EUR/core/h
  * **GPU** from 0.5 EUR/GPU/h*
  * **Memory** 0.0013 EUR/1GB/h
  * **Storage** SSD storage 0.138 EUR/1TB/h
  * Billing is the sum of resources booked for the **virtual private cloud (VPC)** , which can contain 1 or more virtual machines (VM). * GPU prices depend on the model. See the GPU section above to view available options and pricing. 

 | [Calculate your costs](https://hpc.ut.ee/pricing/calculate-costs#vs-host)  |  
| SAPU  | 
  * **One time set-up fee** 500 EUR
  * **CPU** 0.003 EUR/core/h
  * **Memory** 0.0013 EUR/1GB/h
  * **Storage** SSD storage 0.138 EUR/1TB/h
  * **Logs** 0.15 EUR/1GB/month
  * Logs cover logs storage and access (Elastic)

 | [Calculate your costs](https://hpc.ut.ee/pricing/calculate-costs#SAPU)  |  
| Kubernetes container managed service  | 
  * **CPU** 0.015 EUR/core-hour(s)
  * **Memory** 0.003 EUR/1GB/h
  * **GPU** from 0.5 EUR/GPU/h*
  * **Storage** SSD storage 0.138 EUR/1TB/h
  * Accounting is by used or by requested resources, whichever is greater. Accounting resolution is usage per minute. * GPU prices depend on the model. Currently, only RTX 6000 and RTX 4090 GPUs are available. See the GPU section above for pricing. 

 | [Calculate your costs](https://hpc.ut.ee/pricing/calculate-costs#kubernetes)  |  
| API access to AI models/inference  | 
  * **deepseek-v4.1-flash** : input 0.30 EUR/1M tokens, output 1.20 EUR/1M tokens, cache 0.06 EUR/1M tokens
  * **qwen3.8-27b** : input 0.17 EUR/1M tokens, output 0.50 EUR/1M tokens, cache 0.005 EUR/1M tokens
  * **whisper-large-v3** : input 0.0001 EUR/second
  * **qwen3-embedding-8B** : input 0.01 EUR/1M tokens
  * Token pricing and model availability may change over time.

 |  
| Server maintenance  | 
  * **Rate** 60 EUR/server/month
  * Includes monitoring and installation of security updates

 |  
| Website hosting  | 
  * **Rate** 30 EUR/domain/year
  * Includes up to 200 GB storage space, technical support and monitoring

 |  
| Administrator’s hourly rate  | 
  * **Rate** 60 EUR/h
  * Will be applied in case the desired software requires unusually long and complicated installation process.

 |  
| Waldur hosting  | 
  * **Small** up to 100 users - 99 EUR/month
  * **Medium** up to 500 users - 199 EUR/month
  * **Large** more than 500 users - 349 EUR/month
  * Hosting includes backups, availability monitoring and upgrades. Billing is once a year at the beginning of the billing period (the minimum contract period is one year). If, during the billing period, the number of users increases, the next bill will include the additional fee from the past period (the difference between pricing tiers). There is no refund for unused seats.

 |  
UTHPC cluster Rocket, its file systems, and the cluster cooling system will undergo scheduled maintenance (will not be available) during the period of January 12-18, 2026. [Read more »](https://status.hpc.ut.ee/cmhsyrm1v0001qx30kutebh5n)
