---
title: 1,000 homes of power in a filing cabinet - rising power density disrupts AI
  infrastructure | Goldman Sachs
id: 1000-homes-of-power-in-a-filing-cabinet-rising-power-density-disrupts-ai-infrast
tags:
- onprem-llm-inference-stacks-859099
- datacenter-facilities
- rack-density
- roadmap
created: '2026-10-23T17:22:54.749680Z'
updated: '2026-10-23T17:23:51.135022Z'
source: https://www.goldmansachs.com/insights/articles/rising-power-density-disrupts-ai-infrastructure
source_domain: www.goldmansachs.com
fetched_at: '2026-10-23T17:22:54.725133Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Goldman Sachs Insights (2025), chased primary for Inflect: 2022-era 8-GPU
  racks 20-40 kW vs CPU racks 5-15 kW; 2024 Oberon/NVL72-class 10x; 2027 Kyber 576
  GPUs/rack ~600 kW; roadmaps target 1 MW/rack; argues Kyber ends retrofitting era;
  Summit 2018 13 MW across 314 racks.'
---

*Suggested by [[inflect-colocation]] — cited as primary source*

## What We Do
Learn more about how we cultivate and harness world-class intellectual capital and expertise to solve our clients’ most complex challenges.
### GLOBAL BANKING & MARKETS
  *   *   *   * 

### ASSET & WEALTH MANAGEMENT
  *   *   *   * 

### PLATFORM SOLUTIONS
  * 

### RESEARCH & PERSPECTIVES
  *   * 

### Serving Clients
We harness every resource, insight, relationship, and competitive advantage to drive superior results for our clients.
## Insights
Analysis and perspectives on the global economy and markets, from across Goldman Sachs
### OUR SERIES
  *   *   *   * 

### TOPICS IN FOCUS
  *   *   *   *   * [Alternative and Private Markets](https://www.goldmansachs.com/insights/alternatives-private-markets)
  *   * 

### FORMATS
  *   *   *   *   * 

### Briefings
Goldman Sachs’ weekly newsletter, offering economic and markets analysis from across the firm. Subscribe now.
## Our Firm
We aspire to be the world’s most exceptional financial institution.
### ABOUT US
  *   *   *   * [Our Focus on Sustainability](https://www.goldmansachs.com/our-firm/sustainable-finance)
  *   *   * 

## FEATURED
### Community Transformation
We invest the power of Goldman Sachs' financial and intellectual capital to advance enduring economic growth and opportunity.
## Careers
There are many chapters in a career. There is only one Goldman Sachs.
### WORKING AT GS
  *   *   *   *   * 

## Artificial Intelligence
# 1,000 homes of power in a filing cabinet - rising power density disrupts AI infrastructure
May 7, 2025
_**Frank Long** is a vice president at the Goldman Sachs Global Institute, where he focuses on AI._
Executive Summary
  * Chip proximity driving AI performance
Packing processors closer together creates significant performance and cost improvements for both training and inference workloads. However, this comes at the cost of significantly higher power requirements. 2027 AI server rack designs require 50x the power of the server racks that power the internet today.
  * Infrastructure undergoing industrial transformation
To support this unprecedented power density, specialized electrical systems and liquid cooling are transforming datacenters from IT facilities into industrial-scale operations.
  * High-stakes strategic investments
Organizations are making their largest-ever AI infrastructure commitments while balancing accelerated obsolescence risk against potentially market-defining competitive advantages.
  * 2025 marks a decisive moment
Multi-year construction and design timelines mean organizations are acting now to prepare for next-generation systems arriving in 2027. Infrastructure decisions made today could play a significant part in determining AI leadership for the rest of the decade.


**Introduction: Scaling Density of Compute is Changing the World**
Server technology has advanced so dramatically that one rack can today deliver computational power that required hundreds only six years ago. In 2018, the Summit supercomputer became the world’s most powerful compute cluster. Its scale was staggering: 314 racks sprawling across 5,600 square feet, consuming 13 megawatts, equivalent to approximately 13,000 American homes worth of power.1 But now, NVIDIA's NVL72 server rack delivers 5 times the computation power of 2018’s Summit supercomputer in a package that is 1/300th the size.2 Perhaps more mind-boggling: cloud computing operators and AI labs are currently building datacenters to support thousands of these racks in a single building.
With AI driving unprecedented demand for high-performance compute clusters, it should come as no surprise that we stand at a compute density inflection point with significant consequences for power and datacenter infrastructure. The first wave of AI infrastructure piggybacked on cloud computing's foundation, but we've reached the threshold where traditional facilities no longer suffice. The traditional datacenter, designed for general-purpose computing, is giving way to hyper-dense computational environments, with unprecedented power density and advanced cooling systems, to power the next era of AI computing. This evolution is not only a matter of scale; today’s architectures are also dramatically more powerful and energy efficient. Systems today deliver 500 times more compute per-watt of energy consumed than the Summit supercomputer.
Industry roadmaps identify another discontinuous leap in power density coming in 2027 with the release of new designs that pack hundreds of AI chips into a single server rack.3 These newly unveiled designs represent major landmarks on a path toward next-generation infrastructure that requires running enough electricity to power over 1,000 American homes into a space the size of a filing cabinet. This technological evolution could enable breakthrough innovations that shape market leadership and national security. Consequently, we are in a decisive moment where organizations must prioritize the multi-year process of building next-generation infrastructure so they are ready to support next-generation AI servers expected in 2027.
**Why Higher Density Matters**
The push toward extreme compute density is driven by a simple principle: tightly clustered processors, with the fastest connections possible, deliver maximum performance and efficiency. Think of how a team works better with everyone sitting together in one room, rather than spread across different buildings. Likewise, Modern AI workloads require multiple graphics-processing units (GPUs) working in concert. When these processors can communicate rapidly, everything accelerates. When they cannot, bottlenecks form, as compute sits idle waiting for data. These bottlenecks have come to the forefront due to the growing gap between processing speed and data delivery. Over the past 20 years, this disparity has become extreme: compute performance has increased 90,000 times while data transfer speeds have improved only 30 times.4
These data bottlenecks present a critical challenge for AI advancement. Modern AI demands not just unprecedented compute power, but also equally unprecedented real-time data transfer between processors. AI models have outgrown single chips and require multiple chips working collectively. These clusters of chips must communicate extremely quickly to maintain performance and avoid expensive GPUs sitting idle. To accelerate this critical data movement, engineers pack more and more GPUs closer and closer together. The combined densification of packing more transistors into a chip, packing more chips into a server, and packing more servers into a rack, has driven the remarkable leaps in compute power that underpin modern AI. This same densification has also skyrocketed power requirements for a single rack of servers to unprecedented levels.
**50 Times the Power in 5 Years**
When ChatGPT burst onto the scene in 2022, NVIDIA's flagship system integrated eight GPUs into a single server. Companies rushed to repurpose conventional cloud colocation space with these new AI servers to meet surging demand for AI. A rack of these servers stacked together consumed 20-40 kilowatts of power compared to standard CPU racks that used just 5-15 kilowatts.5 This was a challenging increase, but manageable.
This changed in 2024 with NVIDIA’s “Oberon” system, where the entire filing-cabinet sized rack operates as a single server with 144 GPUs working together in harmony. This unified approach delivers the substantial benefits promised by densification but requires 10 times the power for a specific rack.
In 2027, the unified approach will almost certainly be taken to new levels as NVIDIA's “Kyber” system will launch, with 576 GPUs in a single rack requiring a whopping 600 kW, equivalent to delivering enough power for 500 US homes into the space of a filing cabinet. This is 50 times more power per rack than CPU datacenters of just five years ago. It’s also far from the end of this trend: Public industry roadmaps from leading technology companies already target 1 MW per rack.6
Retrofitting existing facilities to support these massive jumps in power density is becoming complex and compromised. Systems like Oberon have already stretched conventional datacenters to their physical limits. Therefore, we will need new, purpose-built AI infrastructure to power the next generation. We believe the requirements of systems like Kyber will mark the end of the retrofitting era altogether, by pushing power densities beyond what existing power distribution systems and cooling architectures can support.
**Industrial-Scale Computing**
Supporting a 50 times power density jump has driven dramatic evolution in power delivery, from power supply components that look the ones we use for our home computers, to components from the electric vehicle supply chain. It’s becoming clear that tomorrow’s computing infrastructure will be almost unrecognizable from traditional IT equipment.
We're witnessing the transformation of datacenters from warehouse-like computer storage facilities to a new breed of industrial infrastructure with massive power requirements and industrial cooling systems akin to aluminum smelting plants. Heat management has become another defining challenge of this new era. It is commonly accepted that hundreds of densely packed GPUs generate thermal output that air cooling simply cannot handle — no matter how efficient. This physical reality has forced a complete datacenter reimagining around liquid cooling: specialized plumbing, coolant distribution units, and sophisticated leak detection systems are becoming fundamental requirements rather than optional upgrades.
This transformation parallels the history of aluminum production, which was originally more expensive than gold per pound, but has become cheap and ubiquitous through industrial-scale manufacturing. The transformations in scale that drove aluminum production efficiency also drove it away from urban centers toward regions with abundant power. Similarly AI datacenters may also relocate away from their historic hubs of Northern Virginia and Silicon Valley, to regions with massive power generation capacity. This shift has the potential to redraw the map of digital infrastructure and creating new economic opportunities for newly emerging regions with energy advantages.
**Unlocking New Frontiers in Efficiency and Capability**
The significant investment and complexity imposed by next-generation infrastructure raises a key question: Is that investment worth it? The answer could come down to two main benefits.
First, these systems provide better economics for current AI models. The integrated, purpose-built design has proven to reduce communication bottlenecks between processors, making existing models run more efficiently. This efficiency cuts operating costs for both inference and training — potentially lowering AI costs further compared to retrofitted infrastructure. For companies using AI at scale, the economic benefits may justify upfront investment. Chips and servers dominate AI infrastructure costs — imagine an extreme scenario where a datacenter houses just one generation of servers before being demolished, the chips could still cost more than double the building itself. In reality, datacenters accommodate multiple chip generations over their lifespans, making this difference even more pronounced. If the datacenter shell represents a relatively small cost compared to the chips, dramatic spending increases on infrastructure are easily justified if they drive even moderate improvements in server utilization. If AI demand continues its dramatic growth trajectory, this new infrastructure will become not just beneficial but essential.
Perhaps even more important is the possibility that these systems could unlock completely new AI capabilities. Throughout computing history, major advances have followed increases in available compute. These new compute clusters could support AI designs that are only theoretical today — models with capabilities beyond current systems because they can process and share information in ways that aren't possible with today's hardware. If these new capabilities unlock truly transformative applications across industries, the resulting demand acceleration would create a flywheel effect that further justifies infrastructure investments. The history of computing suggests we're not just building marginally better systems. We are potentially laying the groundwork for technological leaps that could redefine what's possible.
**Building New Infrastructure Requires New Conviction**
Developers are potentially now making bigger, riskier bets than ever before based on those new frontiers. But what makes these facilities exceptional for AI also renders them impractical for any other computing purpose. These hyper-specialized structures represent a focused bet on AI’s future. The safety net that once existed — "if AI demand plummets, these datacenters can be repurposed for cloud computing" — is fading as the infrastructure diverges too dramatically from general-purpose needs.
This specialization creates profound implications for financing and valuation. Investors must confront uncomfortable questions about residual value. If power demands continue their dramatic climb, even today's cutting-edge facilities could face obsolescence far sooner than traditional datacenters. Financial models with comfortable assumptions about multi-decade useful lives and multi-purpose potential may no longer apply, directly impacting construction decisions given residual value assumptions are key to securing financing.
Organizations must now make infrastructure commitments that simultaneously represent their largest-ever AI investments while carrying unprecedented risk of obsolescence. Yet for those with genuine AI ambitions, hesitation poses an equal threat, as compute scaling has consistently unlocked critical new capabilities. With specialized facilities requiring 18-24 months to build and Kyber systems marking the large jump in power density we’ll see in 2027, today's financing decisions will determine competitive positioning through the rest of the decade and beyond.
**Geopolitics and China: Export Controls Truly Manifest**
The compute density inflection point is as significant for geopolitics as it is for markets. The US government acknowledged the importance of data interconnection when it established export controls on October 7, 2022, which specifically targeted high-speed chip-to-chip communication technology. These advanced interconnects are now one of the "stars of the show" for unlocking the next generation of AI capabilities. Policymakers’ focus has been on advanced chips made by TSMC and the importance of keeping cutting edge fabrication away from China, but interconnects are just as important.
The October 2022 and subsequent export restrictions led NVIDIA to create China-specific chips with degraded interconnect capabilitie s — like the H800, which maintained computational power but had chip-to-chip transfer rates halved.7 And despite the controls, China initially kept pace in AI model development by working around limitations, revealing that restricting interconnect speeds for 8-GPU clusters was not a large enough roadblock to prevent Chinese AI labs from reaching frontier capabilities.8
However, as interconnect technologies advance rapidly, the impact of these controls is becoming more pronounced. With new server architectures, China doesn't face mere performance degradation — it will be unable to import remotely comparable systems altogether. The gap between hundreds of cohesively networked GPUs versus fewer GPUs with limited interconnects creates what is almost certainly an unbridgeable competitive advantage. This will accelerate demand for Chinese domestic chipmaking, further bifurcating the global technology landscape.
The real geopolitical AI infrastructure race is only now beginning in earnest. As much as any other arena, interconnects are the proving ground.
**History Doesn’t Repeat, but it Rhymes**
It should not be surprising that 2025 marks a critical inflection point for AI infrastructure and hardware. Chip and system design cycles take years, so we are just now beginning to see the hardware specifically designed after ChatGPT demonstrated the transformative potential of large language models.
When the iPhone launched in 2007 and kicked off the smartphone era, it initially leveraged existing supply chains, incorporating infrastructure from the existing PC industry and other consumer electronics such as MP3 players. But as smartphones surpassed PCs to become the dominant computing paradigm, they spawned an entirely new supply chain that upended market structures and geopolitical order. During this period, TSMC leapfrogged Intel in semiconductor manufacturing, ARM overtook x86, operating systems shifted, and app ecosystems transformed. These developments laid the foundation for a new market and geopolitical structure we're still grappling with today.
When ChatGPT launched in 2022 and kicked off the modern AI era, it similarly leveraged existing supply chains by incorporating infrastructure from the existing cloud computing industry and other consumer electronics such as gaming. But as AI becomes the dominant computing paradigm, it is also spawning entirely new supply chains to support purpose-built AI infrastructure that may upend market structures and geopolitics.This transition in AI infrastructure marks more than a technical milestone, it could represent a fundamental shift in how organizations build, finance, and compete in the AI era.
Market participants may be faced with making unprecedented infrastructure commitments while navigating rapidly evolving technology, uncertain financial models, and intensifying geopolitical competition. Therefore 2025 is critical. The foundations laid this year could very well determine the AI landscape for the next decade.
_1 Oak Ridge “National Laboratory, Summit Supercomputer Page”,<https://www.olcf.ornl.gov/olcf-resources/compute-systems/summit/>_
_2 NVIDIA, “NVL72 Product Page”,<https://www.nvidia.com/en-us/data-center/gb300-nvl72/>_
_**_3 NVIDIA, “GTC March 2025 Keynote with NVIDIA CEO Jensen Huang”**,**_**_<https://www.youtube.com/watch?v=_waPvOwL9Z8>__
_**_**_4 Marvell, “Getting Moore with Less: How Chiplets and Open Interconnect Accelerate Cloud-Optimized AI Silicon” talk at 2023 OCP Global Summit, _**_** __[https://www.youtube.com/watch?v=6F9r4uK_Cog&list=WL&index=176](https://www.youtube.com/watch?v=6F9r4uK_Cog&list=WL&index=176)___
_**_**_**_5 Data Center Knowledge, “Data Center Power: Fueling the Digital Revolution”,_**_**_**___<https://www.datacenterknowledge.com/energy-power-supply/data-center-power-fueling-the-digital-revolution>____
_**_**_**_6 Google, “+--400Vdc Rack Power System for ML AI Application” talk at 2024 OCP Global Summit,_**_**_**___<https://www.youtube.com/watch?v=l8ChVDv5aoo>____
_**_**_**_7 CSIS, “Where the Chips Fall: U.S. Export Controls Under the Biden Administration from 2022 to 2024”,_**_**_**___<https://www.csis.org/analysis/where-chips-fall-us-export-controls-under-biden-administration-2022-2024>____
_**_**_**_8 VentureBeat, “Open-source DeepSeek-R1 uses pure reinforcement learning to match OpenAI o1 — at 95% less cost”_**_**_** ___<https://venturebeat.com/ai/open-source-deepseek-r1-uses-pure-reinforcement-learning-to-match-openai-o1-at-95-less-cost/>____
This article has been prepared by Goldman Sachs Global Institute and is not a product of Goldman Sachs Global Investment Research. This article is being provided for educational purposes only. This article does not purport to contain a comprehensive overview of Goldman Sachs’ products and offerings. The views and opinions expressed here are those of the author and may differ from the views and opinions of other departments or divisions of Goldman Sachs and its affiliates. The information contained in this article does not constitute a recommendation from any Goldman Sachs entity to the recipient, and Goldman Sachs is not providing any financial, economic, legal, investment, accounting, or tax advice through this article or to its recipient. Goldman Sachs has no obligation to provide any updates or changes to the information herein. Neither Goldman Sachs nor any of its affiliates makes any representation or warranty, express or implied, as to the accuracy or completeness of the statements or any information contained in this article and any liability therefore (including in respect of direct, indirect, or consequential loss or damage) is expressly disclaimed.
Related Tags
### [Artificial Intelligence How Small Business Owners Are Using AI to Help Propel Growth Oct 22, 2026 ](https://www.goldmansachs.com/insights/articles/how-small-business-owners-are-using-ai-to-help-propel-growth)
### [The Markets How Higher Rates and AI Issuance Are Affecting Credit Markets Podcast|Oct 16, 2026 ](https://www.goldmansachs.com/insights/the-markets/how-higher-rates-and-ai-issuance-are-affecting-credit-markets)
### [Artificial Intelligence Competition Among China’s AI Models Is Intensifying as the Chip Sector Advances Oct 9, 2026 ](https://www.goldmansachs.com/insights/articles/competition-among-chinas-ai-models-is-intensifying-as-chip-sector-advances)
### [The Markets AI Is Becoming a Stock Picker's Market  Podcast|Oct 2, 2026 ](https://www.goldmansachs.com/insights/the-markets/ai-is-becoming-a-stock-pickers-market)
## Subscribe to Briefings
Our weekly newsletter delivers the latest insights on economic forces shaping markets—from Goldman Sachs leaders, economists, and investors around the world.
You can unsubscribe at any time. For information about how your personal data will be used, visit [Privacy Information and Resources](https://www.goldmansachs.com/privacy-and-cookies).
We and our third-party partners use cookies and similar technologies for website analytics, advertising, and other purposes as further described in our [Privacy Policy](https://www.goldmansachs.com/privacy-and-cookies/us-privacy-policy). By accessing this website, you agree to the use of these technologies and use of information in accordance with that policy. You may opt-out of the use of certain technologies by clicking the “Manage Preferences” button. 
Manage Preferences
