---
title: 'H100 Depreciation: How Fast NVIDIA H100s Lose Value (and What It Means for
  TCO) | Mercatus Blog'
id: h100-depreciation-how-fast-nvidia-h100s-lose-value-and-what-it-means-for-tco-mer
tags:
- onprem-llm-inference-stacks-859099
- cc-gap-fill
created: '2026-10-23T18:17:26.150563Z'
updated: '2026-10-23T18:19:17.913262Z'
source: https://www.mercatus-ai.com/blog/h100-depreciation
source_domain: www.mercatus-ai.com
fetched_at: '2026-10-23T18:17:26.130800Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Mercatus 30 Mar 2026 H100 depreciation guide: 36-month retained value 50-60%
  mid-case (30% conservative, 70% optimistic); curve 95-100% (0-12mo) to 25-35% floor
  (60+ mo); self-described ''forward-looking estimates''; A100 retained 30-50% of
  list by 2026; refurb H100 $18-22k; 100-H100 TCO swings $1.7M with depreciation assumption.
  Commercial, methodology undisclosed.'
---

[Home](https://www.mercatus-ai.com/)[Blog](https://www.mercatus-ai.com/blog)H100 Depreciation: How Fast NVIDIA H100s Lose Value (and What It Means for TCO)
GPU EconomicsMar 30, 20267 min read
# H100 Depreciation: How Fast NVIDIA H100s Lose Value (and What It Means for TCO)
How fast do NVIDIA H100 GPUs lose value? This guide breaks down H100 depreciation rates, Blackwell’s impact on residual value, secondary market pricing, and how depreciation affects real AI infrastructure TCO in 2026.
Mercatus Compute
Author
Share
For institutional AI infrastructure operators, the depreciation assumption is the single largest variable in the 3-year cluster TCO model. A 100-H100 cluster’s TCO swings **$1.7M** between conservative and optimistic depreciation — on identical hardware, identical operations, identical everything else.
This guide covers actual H100 depreciation curves observed through 2026, the drivers that move them, the 2026 secondary market reality, and how to model depreciation in capital allocation decisions. For the broader cluster TCO context: [100 H100 Cluster TCO.](https://www.mercatus-ai.com/blog/100-h100-cluster-tco)
## **TL;DR**
  * **H100 SXM5 retained value at 36 months in 2026:** 50–60% (mid-case), with 30% (conservative) to 70% (optimistic) range.
  * **Blackwell ramp speed is the dominant variable.** Faster ramp = faster H100 depreciation.
  * **The H100 secondary market is functional in 2026** — refurb cards trade at $18,000–$22,000, providing real alternatives for cost-sensitive buyers.
  * **Capacity monetization through Mercatus offsets depreciation:** every dollar of inference revenue from idle capacity reduces the effective net cost of the depreciating asset.


## **How GPU depreciation actually works**
Datacenter GPUs depreciate along a non-linear curve driven by three factors:
### **1. Generational displacement**
When a new generation ships in volume, demand for the previous generation drops. This is the largest depreciation driver.
  * **A100 history:** A100 launched 2020. H100 shipped at scale 2022. By 2024, A100 had lost 50–60% of original value. By 2026, A100 trades at 30–50% of original list.
  * **H100 trajectory:** H100 launched 2022. Blackwell (B100/B200) shipping in volume through 2026. Expected H100 depreciation curve mirrors A100’s, possibly faster if Blackwell supply expands more rapidly.


### **2. Sustained demand for the previous generation**
Even after a successor launches, the old generation retains value if there’s continued demand for its specific use cases. A100s in 2026 are not worthless — they’re still used for fine-tuning and smaller inference workloads. This creates a price floor.
For H100, the floor is determined by:
  * Continued usefulness for inference workloads where Blackwell isn’t yet cost-justified
  * Use in regions where Blackwell hasn’t yet shipped at volume
  * Ongoing training of mid-size models where H100’s economics work fine
  * Replacement parts demand for existing H100 fleets


### **3. Secondary market depth**
The depth of the secondary market determines how easily depreciated cards can be sold vs how much owners take a haircut to dispose of inventory. Through 2024, the H100 secondary market was thin (cards too new). By 2026, it’s becoming a real market with predictable pricing.
## **Observed H100 depreciation curve (2022–2026)**  
| H100 age  | Approximate retained value  | Source/comparable  |  
| --- | --- | --- |  
| 0–12 months  | 95–100%  | Limited secondary market activity  |  
| 12–18 months  | 85–95%  | Early refurb appearances  |  
| 18–24 months  | 75–85%  | Maturing secondary market  |  
| 24–36 months  | 60–75%  | Mid-life depreciation  |  
| 36–48 months  | 45–60%  | Blackwell mass-market displacement  |  
| 48–60 months  | 30–45%  | Late-life inventory  |  
| 60+ months  | 25–35%  | Approximate floor (parts/legacy use)  |  
These are 2026 forward-looking estimates. Actual depreciation depends heavily on Blackwell ramp speed and demand sustainment for H100-specific workloads.
For comparison, the A100 trajectory from launch:  
| A100 age (years)  | Retained value (observed)  |  
| --- | --- |  
| 1  | ~95%  |  
| 2  | ~85%  |  
| 3  | ~70%  |  
| 4  | ~55%  |  
| 5  | ~40%  |  
| 6 (now, 2026)  | ~30–35%  |  
H100 depreciation is currently tracking faster than A100’s at equivalent age — the AI compute supply scaling, faster successor rollout (Blackwell), and more mature secondary market all contribute.
## **What drives faster or slower depreciation**
Five variables that move H100 residual value:
### **1. Blackwell ramp speed**
Most important variable. Faster Blackwell mass shipping → faster H100 displacement. As of mid-2026:
  * B100/B200 are shipping in early-cohort volumes
  * Hyperscaler demand absorbs most current Blackwell production
  * Long-tail providers and institutional buyers see Blackwell at high prices and limited availability through 2026
  * Mainstream availability with competitive pricing expected 2027+


Faster ramp scenario: H100 36-month residual at 40–50%. Slower ramp scenario: 65–75%.
### **2. AI workload growth**
Strong continued growth in AI compute demand sustains H100 prices even as Blackwell ramps. If demand growth slows or Blackwell efficiency dramatically exceeds expectations, H100 demand collapses faster.
### **3. Secondary market liquidity**
Better secondary market = lower depreciation haircut at disposal. Mercatus and other data-driven marketplaces are improving secondary market efficiency, which reduces effective depreciation cost.
### **4. Software stack continuity**
NVIDIA continues to release CUDA, software stack updates, and AI library support for H100 through at least 2028. Software-side support sustains hardware utility, reducing functional obsolescence pressure.
### **5. Power efficiency tradeoff vs Blackwell**
If Blackwell is dramatically more power-efficient than H100 (per published specs, expected 2–3× perf-per-watt advantage on some workloads), high-power-cost regions will move to Blackwell faster. Low-power-cost regions where H100 efficiency is already acceptable see slower depreciation pressure.
## **The 2026 secondary market for H100**
By 2026, the H100 secondary market has matured enough to provide a real option for cost-sensitive buyers:  
| Source  | Typical pricing  | Notes  |  
| --- | --- | --- |  
| OEM-fresh H100 SXM5  | $25,000–$30,000  | New OEM, full warranty  |  
| Authorized refurb  | $20,000–$24,000  | OEM refurb, 12-month warranty  |  
| Data center decommission  | $18,000–$22,000  | Used cards, varying warranty  |  
| Distressed/liquidation  | $14,000–$18,000  | Limited warranty, condition variable  |  
For research and dev workloads, refurb at $18K–$22K is a defensible 30–40% capex savings vs new. For production workloads, OEM-fresh hardware is usually worth the premium for warranty coverage and remaining useful life.
The secondary market also enables a new operational pattern: **pre-emptive disposal at peak residual value**. Rather than holding GPUs through full depreciation, operators sell at 18–24 month mark when residual is 75–85% and rotate to current-generation hardware. This requires capital and operational sophistication but minimizes total depreciation cost.
## **How depreciation affects cluster TCO**
For a 100-H100 cluster with $3.5M hardware capex on a 3-year horizon, depreciation scenarios:  
| Depreciation case  | 36-month retained value  | Depreciation expense  | 3-year TCO  |  
| --- | --- | --- | --- |  
| Conservative (30% retained)  | $1.05M  | $2.45M  | $5.7M  |  
| Base case (45% retained)  | $1.58M  | $1.93M  | $5.1M  |  
| Optimistic (70% retained)  | $2.45M  | $1.05M  | $4.0M  |  
The **$1.7M swing** between conservative and optimistic is entirely a depreciation assumption. For institutional finance teams, this is the largest single line item to argue about. For TCO sensitivity, see 100 H100 Cluster TCO.
The conservative case is the right planning baseline. The optimistic case is what you hope for. Operations should be sized to be profitable in the conservative scenario.
## **How Provider monetization changes the depreciation story**
The depreciation model assumes you’re consuming the GPU’s lifetime on your primary workload. If you also monetize idle capacity through Mercatus, you’re capturing additional value from the same depreciating asset.
For a 100-H100 cluster running primary workload at 70% utilization with 30% idle capacity monetized:

```
// text
Depreciation expense (3-yr, base case):    $1.93M
Idle capacity revenue (3 years):           $1.58M ($526K/yr × 3)
Net depreciation impact:                   $0.35M effective
```

Capacity monetization can offset roughly **80% of depreciation expense** for clusters with significant idle capacity. This fundamentally changes the buy-vs-rent calculation: depreciation is no longer a pure cost, it’s an asset whose lifetime value can be captured through dual revenue streams.
→ Become a Provider to monetize cluster capacity.
For broader thesis: The Open AI Compute Economy.
## **What this means for buyers**
If you’re buying H100s in 2026, three implications:
**1. Plan for the conservative depreciation case.** Hardware capex should pencil out at 30% residual after 3 years. Anything better is upside.
**2. Consider refurb for non-production workloads.** $18K–$22K refurb pricing makes A100-class workloads cheaper on H100 than on new A100 alternatives. Worth evaluating for fine-tuning and dev fleets.
**3. Build capacity monetization into the TCO model from day one.** If you’ll list idle capacity through Mercatus, the effective cost structure of owning H100s improves dramatically. Don’t model this as bonus — model it as core.
For the buy-vs-rent decision integrating depreciation: [Buy vs Rent GPUs](https://www.mercatus-ai.com/blog/buy-vs-rent-gpus).
## **Frequently Asked Questions**
**How much does an H100 lose in value per year?**
In 2026, H100 SXM5 retains approximately 90% of value at 12 months, 80% at 24 months, 60–65% at 36 months, and continues declining roughly 10% per year afterward. The depreciation curve is non-linear — slow at first, accelerating as Blackwell mass-market displacement progresses.
**Will Blackwell make H100s worthless?**
No. H100 has a real residual value floor (estimated 25–35% at 60+ months) driven by continued utility for inference, smaller-model training, and replacement parts demand. Blackwell will displace H100 for frontier training but won’t eliminate H100 utility.
**Should I buy a refurbished H100?**
For research, dev, and non-production workloads: yes. $18K–$22K for refurb vs $25K–$30K for new is a meaningful 30–40% capex saving. For production, OEM-fresh hardware with full warranty is usually worth the premium.
**How does depreciation affect H100 cluster TCO?**
Depreciation typically represents 35–45% of 3-year cluster TCO for owned H100 fleets. The single largest variable in the model: 30% vs 70% residual value at 36 months swings TCO by ~$1.7M for a 100-GPU cluster. Conservative planning assumes 30%; base case ~45%.
**Can capacity monetization offset depreciation?**
Yes — substantially. Listing idle capacity through Mercatus generates inference revenue that offsets the depreciation expense. For typical clusters, monetization can offset 60–80% of 3-year depreciation cost. → Become a Provider.
**Is H200 depreciation faster or slower than H100?**
Too early to tell definitively (H200 launched 2024). Expected to track H100 closely — same Hopper architecture, similar Blackwell displacement risk. Memory advantage may sustain residual value for long-context inference workloads better than pure-compute H100s.
**Where can I track current H100 secondary market prices?**
[Mercatus GPU Index](https://www.mercatus-ai.com/gpu-index) tracks both new and secondary market H100 pricing across providers and OEMs in real time. Useful for institutional buyers timing acquisitions or sales.
## **Methodology**
Depreciation curves derived from observed A100 historical data and 2026 H100 secondary market activity. Pricing ranges reflect Mercatus GPU Index May 2026 cross-source snapshot. Cluster TCO sensitivity scenarios reference the 100 H100 Cluster TCO base case. Last verified: 2026-05-04.
More Articles
[GeneralMeta Muse Spark Open API Pricing: Rates, Hosts, License Meta's Muse Spark Open costs $1.25 input and $4.25 output per 1M tokens at every host listing it. The 816B open weight model has no official Meta API, a limited commercial license, and a market that is one price wide so far. 5 min read](https://www.mercatus-ai.com/blog/meta-muse-spark-open-api-pricing)[GeneralMistral Large 4 API Pricing: Rates, Hosts, Open Weights Mistral Large 4 costs $1.36 input and $4.18 output per 1M tokens on Mistral's API now that the 50% launch promotion has ended. One host sells it, and open weights are due October 31. 5 min read](https://www.mercatus-ai.com/blog/mistral-large-4-api-pricing)[GeneralQwen3.8 Max Prime Pricing: Same Model, 2x the Price Qwen3.8 Max Prime costs exactly 2x Qwen3.8 Max on Alibaba's API for the same weights, in exchange for 1.5 to 2x the output speed. Official rates by region, what Prime is, and what the speed premium costs on a real workload. 5 min read](https://www.mercatus-ai.com/blog/qwen3-8-max-prime-api-pricing)
