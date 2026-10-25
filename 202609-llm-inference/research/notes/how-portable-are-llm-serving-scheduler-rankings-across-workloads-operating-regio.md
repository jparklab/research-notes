---
title: How Portable Are LLM-Serving Scheduler Rankings Across Workloads, Operating
  Regions, and Metrics? | Research Square
id: how-portable-are-llm-serving-scheduler-rankings-across-workloads-operating-regio
tags:
- onprem-llm-inference-stacks-859099
- scheduler-portability
- gpu-utilization
created: '2026-10-23T17:21:46.303831Z'
updated: '2026-10-23T17:22:55.871230Z'
source: https://doi.org/10.21203/rs.3.rs-10935389/v1
source_domain: doi.org
fetched_at: '2026-10-23T17:21:39.936117Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'Vahidi preprint (Research Square, 8 Sep 2026, not peer reviewed): LSSP benchmark,
  3 workload sources x 6 operating regions, 9,360 configs run twice. Scheduler rank
  agreement strong between two sources (Kendall tau_b up to 1.0) but as low as 0.55
  with BurstGPT; 36/990 (3.6%) pairwise reversals, all one policy pair; across metrics
  median tau_b=0.419, top-policy agreement 31.9%; one physical-hardware validation
  did not reproduce a simulator-predicted reversal. Takeaway: scheduler choices do
  not generalize; benchmark on your own workload.'
doi: 10.21203/rs.3.rs-10935389/v1
utility_score: 13.0
citation_count: 0
venue: Research Square
is_retracted: false
---

Share
[Download PDF](https://www.researchsquare.com/article/rs-10935389/v1.pdf?c=1790701087000)
Research Article
How Portable Are LLM-Serving Scheduler Rankings Across Workloads, Operating Regions, and Metrics?
Soroush Vahidi
This is a preprint; it has not been peer reviewed by a journal.
https://doi.org/10.21203/rs.3.rs-10935389/v1
This work is licensed under a CC BY 4.0 License
### Status:
Posted
### Version 1
posted 08 Sep, 2026
You are reading this latest preprint version
## Abstract
Large language model (LLM) serving systems are typically compared on one workload, operating region, and metric. We present the LLM-Serving Scheduler Portability Benchmark (LSSP), a preregistered evaluation of scheduling policies across three independently released workload sources and six calibrated operating regions (9,360 unique configurations, each executed twice). Cross-source rank agreement is strong between two sources (Kendall’s τb up to 1.0) but lower when the third, BurstGPT, is involved (as low as 0.55); 3.6% of pairwise comparisons (36/990) show statistically supported reversals, all concentrated in a single policy pair with BurstGPT on one side. A post-campaign analysis finds portability across evaluation metrics more limited: moderate median rank correlation (τb = 0.419) and low top-policy agreement (31.9%). A selected physical-hardware validation did not reproduce a simulator-predicted reversal, while a stable-control comparison retained its qualitative ordering. Scheduler comparisons should be read as conditional on the workload, region, and metric that produced them.
LLM inference serving
request scheduling
ranking portability
performance evaluation
workload characterization
benchmark reproducibility
## Full Text
## Additional Declarations
Share
[Download PDF](https://www.researchsquare.com/article/rs-10935389/v1.pdf?c=1790701087000)
### Status:
Posted
### Version 1
posted 08 Sep, 2026
You are reading this latest preprint version
### Citations
See more
### Engagement
74 views
### Comments
