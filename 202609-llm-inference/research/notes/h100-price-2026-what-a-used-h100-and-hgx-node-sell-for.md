---
title: 'H100 Price 2026: What a Used H100 and HGX Node Sell For'
id: h100-price-2026-what-a-used-h100-and-hgx-node-sell-for
tags:
- onprem-llm-inference-stacks-859099
- cc-gap-fill
created: '2026-10-23T18:13:35.113312Z'
updated: '2026-10-23T18:19:25.412990Z'
source: https://www.amcompute.com/appraisals/h100
source_domain: www.amcompute.com
fetched_at: '2026-10-23T18:13:35.093342Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'American Compute (equipment-finance appraiser) H100 appraisal, citing its
  Oct 2026 Rack Report (data through Aug 2026): used 8-GPU HGX H100 node $195,000
  ($24,400/GPU); single used H100 SXM typical realized sale $14,250 (25th pct $13,200;
  dealer ask $16,576 = 1.16x realized; liquidity grade C; limited sample). Residual
  forecast as % of original list: 2026 27-39%, 2027 11-19% (moderate 19-42%), 2028
  5-7% (moderate 7-14%). Warns Hopper residuals fall faster once liquid-cooled Blackwell
  capacity arrives. Appraiser/lender-oriented source; the only transaction-based and
  most pessimistic residual series in the vault; implies a 2026 H100 bought new and
  sold at year 3 recovers roughly 5-14% of list, versus 45-70% in listing-based series.'
---

# H100 Appraisal
What H100 collateral is worth now, and when your loan matures.
## What it's worth
A used 8-GPU HGX H100 node was worth $195,000 and a single H100 SXM $14,250 in the October 2026 Rack Report. Our conservative band puts the H100 at 11-19% of original list price in 2027 and 5-7% in 2028.
## Our data  
| Figure  | Value  | Detail  |  
| --- | --- | --- |  
| Used HGX H100 8x80GB node  | $195,000  | $24,400 per GPU; +4% vs the sum of its parts. October 2026 Rack Report, data through August 2026.  |  
| Used H100 SXM 80GB, typical realized sale  | $14,250  | 25th percentile $13,200; dealer ask $16,576 (1.16x realized); liquidity C; limited sample. October 2026 Rack Report, data through August 2026.  |  
| H100 SXM: rental-implied value vs realized  | 1.11x  | Rent of $3.19 per GPU-hour implies $215,865 per node; nodes sold for $195,000. October 2026 Rack Report, data through August 2026.  |  
| H100 residual, 2027  | 11-19%  | Conservative band, % of original list price; moderate 19-42%. Residual report, as of June 2026.  |  
| H100 residual, 2028  | 5-7%  | Conservative band, % of original list price; moderate 7-14%. Residual report, as of June 2026.  |  
Sources: American Compute.
## What moves H100 collateral value
Asks are not sales. The median dealer ask for a used H100 SXM was $16,576, 1.16x the typical realized price. A valuation built on listings overstates the collateral by that margin.
Rents and resale moved apart. In 2025 H100 rental rates fell as new compute came online, while the median H100 resale price rose, because many data centers could not yet host Blackwell. Rental indices are a weak predictor of resale; only completed sales price the hardware.
The whole node earns little over its parts. An HGX H100 node traded +4% against the sum of its parts, against +40% for an HGX A100 node. Parted-out H100s still clear, which keeps the forced liquidation value closer to the orderly one than for older systems.
The power wall is the support, and the risk. A DGX H100 draws 10.2 kW and is air-cooled, so it runs in data centers that cannot take Blackwell. Our report expects Hopper residuals to fall faster once liquid-cooled capacity catches up. The H100 band falls from 27-39% of list price in 2026 to 5-7% in 2028.
## Value it on technology age
The H100 first shipped in late 2022. An H100 server bought in 2024 is valued as a four-year-old generation in 2026, whatever its invoice date. Depreciation schedules do not change that: CoreWeave depreciates GPU servers over six years, a convention, not a resale forecast. See the [useful-life tracker](https://www.amcompute.com/gpu-depreciation-schedules).
## What a report flags  
| Severity  | Finding  |  
| --- | --- |  
| High  | Borrowing base uses dealer asking prices; realized sales for the same GPU are lower by the ask-to-sale gap in the latest Rack Report.  |  
| Medium  | Balloon at maturity in 2028 exceeds the H100 moderate band for that year; refinancing risk sits with the lender.  |  
| Low  | Serial numbers read remotely match the UCC-1 equipment schedule for all nodes inspected.  |  
Illustrative.
## Book a call
Send us the equipment list and the loan or lease you have in mind. We will come back with an indication and the questions we would need answered.
Prefer email? hello@amcompute.com
