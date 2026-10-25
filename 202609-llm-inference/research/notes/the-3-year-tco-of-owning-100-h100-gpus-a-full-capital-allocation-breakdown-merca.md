---
title: 'The 3-Year TCO of Owning 100 H100 GPUs: A Full Capital Allocation Breakdown
  | Mercatus Blog'
id: the-3-year-tco-of-owning-100-h100-gpus-a-full-capital-allocation-breakdown-merca
tags:
- onprem-llm-inference-stacks-859099
- cc-gap-fill
created: '2026-10-23T18:17:21.590289Z'
updated: '2026-10-23T18:19:17.675357Z'
source: https://www.mercatus-ai.com/blog/100-h100-cluster-tco
source_domain: www.mercatus-ai.com
fetched_at: '2026-10-23T18:17:21.570377Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Mercatus Compute (GPU capacity-marketplace vendor) 30 Mar 2026 modelled
  3-year TCO of 100 H100 (13 HGX servers, 104 GPUs): ~$5.7M, hardware depreciated
  $2.5M (44%), power $370K (6%), colo $1.0M (18%), networking+storage $600K (11%),
  operations+maintenance $1.2M (21%). ~$3.10 per useful GPU-hr at 70% util, $2.30
  at 90%, $4.50 at 50%; claims owning beats rented 3-year reserved ($1.30-1.80) only
  at >=75% utilization and >=50 GPUs; assumes H100 retains 30-70% at 3 years. This
  is a MODEL (vendor, assumptions not an operator ledger): ops = at least one, often
  two infra engineers. Staff-inclusive but not operator-disclosed.'
---

[Home](https://www.mercatus-ai.com/)[Blog](https://www.mercatus-ai.com/blog)The 3-Year TCO of Owning 100 H100 GPUs: A Full Capital Allocation Breakdown
GPU EconomicsMar 30, 20265 min read
# The 3-Year TCO of Owning 100 H100 GPUs: A Full Capital Allocation Breakdown
A full breakdown of the 3-year total cost of owning 100 NVIDIA H100 GPUs, including hardware, power, colocation, networking, operations, depreciation, and utilization economics.
Mercatus Compute
Author
Share
A 100-GPU H100 cluster will run you somewhere between **$5 million and $7 million** over 3 years, all-in. That’s the headline number for any institutional buyer or finance committee evaluating an AI infrastructure purchase.
The variance — that $2M spread — is not noise. It’s the answer to four real questions: how much utilization will you actually drive, what power costs do you face, how aggressively will Blackwell drive H100 depreciation, and can you monetize idle capacity. This guide walks through every cost component with specific 2026 numbers, gives you the formula to plug your own assumptions into, and identifies where the financial picture actually tilts.
This is the **own-side** view. For the rent-side equivalent across cloud providers, see the sibling pillar [Cloud GPU Pricing in 2026](https://www.mercatus-ai.com/blog/cloud-gpu-pricing). For single-GPU economics, see [H100 GPU Cost ](https://www.mercatus-ai.com/blog/h100-gpu-cost)— the math is meaningfully different at unit scale. The buy-vs-rent decision framework that connects them is in [Buy vs Rent GPUs](https://www.mercatus-ai.com/blog/buy-vs-rent-gpus).
## **TL;DR**
Owning 100 H100 GPUs over 3 years costs roughly:  
| Cost component  | 3-year total  | % of TCO  |  
| --- | --- | --- |  
| Hardware (depreciated)  | $2.5M  | 44%  |  
| Power  | $370K  | 6%  |  
| Colocation  | $1.0M  | 18%  |  
| Networking + storage  | $600K  | 11%  |  
| Operations + maintenance  | $1.2M  | 21%  |  
| Total  | ~$5.7M  | 100%  |  
Effective cost: **~$3.10 per GPU-useful-hour** at 70% utilization. The per-hour cost drops to ~$2.30 at 90% utilization and rises to ~$4.50 at 50% utilization. Utilization is the single largest controllable lever on cost efficiency.
For comparison: reserved 3-year H100 cloud capacity from a long-tail provider runs $1.30–$1.80/GPU-hour. **Owning beats cloud rental in 2026 only at high utilization (≥75%) and at scale (≥50 GPUs)** — and even then, often only narrowly unless you can monetize idle capacity through a Provider listing.
## **Why think about 100 GPUs as a unit**
Single-GPU economics work differently from cluster economics. A single H100 in a colo is mostly hardware capex plus power and colo. At 100 GPUs, you’ve crossed thresholds that activate new cost categories:
  * **Networking** becomes a real expense. Multi-node training and high-throughput inference need InfiniBand fabric or 400G Ethernet, plus parallel filesystem storage. These are negligible at 1-GPU scale and material at 100-GPU scale.
  * **Operations** becomes a dedicated function. You need at least one infrastructure engineer (often two), monitoring infrastructure, vendor support contracts, and a hardware failure replacement reserve.
  * **Hardware procurement economics** improve. OEMs offer 8–12% volume discounts on 12+ system orders.
  * **Power and colocation** scale linearly but unlock wholesale-tier pricing in some markets.


The 100-GPU cluster is roughly the smallest scale at which “owning AI infrastructure” is its own line of business inside a company, not a side project. Below ~50 GPUs, you’re a sophisticated cloud customer. Above ~50 GPUs, you’re an infrastructure operator. The break point is closer to 50 than 100, but 100 is the round number institutional buyers think in.
For finance teams: **this is a capital allocation decision, not a software vendor decision**. The $5–7M three-year TCO requires CFO sign-off and competes with other strategic investments. Treating it as a software contract leads to wrong answers.
## **Component 1: Hardware (CapEx)**
A 100-H100 cluster comprises 13 × 8-GPU HGX H100 servers (104 GPUs deployed; “100” is the round number). Per-server pricing in 2026:  
| Component  | Cost per server  |  
| --- | --- |  
| 8× H100 SXM5 GPUs  | $200,000  |  
| Server (CPUs, motherboard, chassis)  | $30,000  |  
| NVLink fabric / NVSwitch  | $15,000  |  
| Networking (NICs, cables)  | $20,000  |  
| Integration, BIOS, vendor support  | $15,000  |  
| Total per server  | ~$280,000  |  
For 13 servers: **$3.64M base hardware capex**.
Plus cluster-level hardware:
  * Top-of-rack switches and InfiniBand spine: ~$200,000
  * Storage system (parallel filesystem, NVMe tier): ~$300,000
  * Management infrastructure (PDUs, KVM, racks): ~$50,000


**Total hardware capex: ~$4.2M** for a complete 100-GPU cluster ready to run production workloads.
Per GPU all-in: **$40,000** including everything needed for production (vs. $25–30K for the bare GPU price you see quoted).
### **Why hardware capex varies $500K+ on the same fleet**
Three drivers:
  * **OEM choice.** Supermicro consistently 8–12% cheaper than Dell/HPE. Major hyperscaler-grade deployments often go through specialty integrators (Lambda, ZT Systems) at intermediate prices.
  * **Volume tier.** 13-system orders earn 5–8% volume discount; 50+ system orders unlock 12–15% discount.
  * **Region.** EU and APAC pricing 5–10% above US for the same hardware due to import duties and logistics.


For first-time institutional buyers, getting three OEM quotes is worth ~$300–500K. This is the easiest cost lever in the entire TCO breakdown.
### **Depreciation assumption matters more than capex**
Over 3 years, an H100 retains somewhere between 30% and 70% of its original value depending on:
  * Blackwell ramp speed (faster = more H100 depreciation)
  * Demand for H100 inference at end-of-life (sustained = better residual)
  * Secondary market depth (improving rapidly)


For 2026 institutional planning, two scenarios:  
| Depreciation scenario  | 3-yr depreciation expense  | Total TCO  |  
| --- | --- | --- |  
| Conservative (30% retained, $2.94M depreciation)  | $2.94M  | $5.7M  |  
| Base case (45% retained, $2.31M depreciation)  | $2.31M  | $5.1M  |  
| Optimistic (70% retained, $1.26M depreciation)  | $1.26M  | $4.0M  |  
The **$1.7M swing** between conservative and optimistic on the same fleet, same operations, is entirely a depreciation assumption. For finance teams, this is the largest single line item to argue about. Use a base case for primary planning and stress-test against the conservative end.
For deeper analysis: GPU Depreciation: [How Fast Do H100s Lose Value?](https://www.mercatus-ai.com/blog/h100-depreciation).
## **Component 2: Power**
A 100-GPU H100 cluster has a substantial power footprint. The math:  
| Power layer  | Calculation  | Value  |  
| --- | --- | --- |  
| GPU TDP only  | 100 × 700W  | 70 kW  |  
| Server-level draw (incl. CPUs, NICs, NVSwitch)  | 13 × 10.2 kW  | 133 kW  |  
| Plus networking + storage  | + ~12 kW  | 145 kW IT load  |  
| With cooling overhead (PUE 1.4)  | × 1.4  | ~203 kW total facility  |  
At 70% workload utilization, average power consumption is roughly 140 kW (GPUs draw less when not running compute). Annual power consumption:

```
// text
140 kW × 8,760 hours × 0.7 utilization factor = 858,000 kWh/year
                                              = 0.86 GWh/year
```

At U.S. industrial electricity rates ($0.08–$0.12/kWh):  
| Power cost  | Annual  | 3-year  |  
| --- | --- | --- |  
| At $0.08/kWh  | $69K  | $206K  |  
| At $0.10/kWh  | $86K  | $258K  |  
| At $0.12/kWh  | $103K  | $309K  |  
Most TCO models use $0.10/kWh as a round number for U.S. deployments. EU and APAC rates run 1.5–2.5× higher ($0.15–$0.25/kWh), turning power from a 6% line item into a 10–14% line item.
## **Methodology**
Cost estimates use 2026 typical pricing for U.S. Tier-3 colocation deployments with mid-range OEM procurement. Hardware capex assumes Supermicro HGX H100 systems with mid-volume tier discounts. Power calculations assume PUE 1.4 and $0.10/kWh industrial rates. Operations cost assumes 1.5 FTE infrastructure engineers at fully-loaded cost. Depreciation base case assumes 45% residual value at 3 years (sensitivity range provided). Cloud comparison rates derived from Mercatus GPU Index May 2026 cross-provider snapshot.
Last verified: 2026-05-04.
Methodology and data dictionary: https://docs.mercatus-ai.com/methodology
More Articles
[GeneralMeta Muse Spark Open API Pricing: Rates, Hosts, License Meta's Muse Spark Open costs $1.25 input and $4.25 output per 1M tokens at every host listing it. The 816B open weight model has no official Meta API, a limited commercial license, and a market that is one price wide so far. 5 min read](https://www.mercatus-ai.com/blog/meta-muse-spark-open-api-pricing)[GeneralMistral Large 4 API Pricing: Rates, Hosts, Open Weights Mistral Large 4 costs $1.36 input and $4.18 output per 1M tokens on Mistral's API now that the 50% launch promotion has ended. One host sells it, and open weights are due October 31. 5 min read](https://www.mercatus-ai.com/blog/mistral-large-4-api-pricing)[GeneralQwen3.8 Max Prime Pricing: Same Model, 2x the Price Qwen3.8 Max Prime costs exactly 2x Qwen3.8 Max on Alibaba's API for the same weights, in exchange for 1.5 to 2x the output speed. Official rates by region, what Prime is, and what the speed premium costs on a real workload. 5 min read](https://www.mercatus-ai.com/blog/qwen3-8-max-prime-api-pricing)
