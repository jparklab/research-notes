---
title: SC25 Proceedings
id: sc25-proceedings
tags:
- onprem-llm-inference-stacks-859099
- hpc-llm-serving
created: '2026-10-23T17:22:34.657421Z'
updated: '2026-10-23T17:23:14.143683Z'
source: https://sc25.supercomputing.org/proceedings/workshops/workshop_pages/ws_fgai114.html
source_domain: sc25.supercomputing.org
fetched_at: '2026-10-23T17:22:34.635817Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'SC25 workshop abstract (UIUC/NCSA, Marwaha et al): three open-source HPC
  LLM serving frameworks: batch OpenAI-format jobs, Ray Serve on-demand interactive,
  always-on dedicated GPU server platform. Landing page only; ACM PDF blocked (escalation
  #39).'
---

*Suggested by [[experience-deploying-containerized-genai-services]] — HPC LLM serving lead*

The International Conference for High Performance Computing, Networking, Storage, and Analysis
## Workshops Archive
### Frameworks for Large Language Model Serving in HPC Environments
**Workshop:** Frontiers in Generative AI for HPC Science and Engineering: Foundations, Challenges, and Opportunities
**Authors:** Rohan Marwaha (University of Illinois at Urbana-Champaign); Qinren Zhou (University of Illinois at Urbana-Champaign, National Center for Supercomputing Applications (NCSA)); Kastan Day and Asmita Dabholkar (University of Illinois at Urbana-Champaign); and Volodymyr Kindratenko (University of Illinois at Urbana-Champaign, National Center for Supercomputing Applications (NCSA))
**Abstract:** We introduce open-source frameworks for deploying and running large language models (LLMs) within high-performance computing (HPC) environments. One such framework targets high- throughput batch inference, enabling users to submit LLM requests in an OpenAI-compatible format as traditional HPC jobs. Another framework is based on Ray Serve and it provides dynamic, on-demand allocation of HPC resources for interactive LLM serving via APIs, supporting applications such as chatbots and AI agents. The third framework is a production-grade, always- on platform for real-time interaction, that relies on a dedicated GPU server for model inference. These frameworks are designed to abstract away underlying computer system complexities, allowing researchers to request and utilize GPU resources for model inference without manual environment setup. We describe these systems and report LLM-specific performance metrics. Results demonstrate that the proposed frameworks enable scalable and resource-efficient LLM serving across both batch and interactive workloads in support of diverse user needs.
[Back to Frontiers in Generative AI for HPC Science and Engineering: Foundations, Challenges, and Opportunities Archive Listing](https://sc25.supercomputing.org/proceedings/workshops/workshop_sessions.html#sess209) [Back to Full Workshop Archive Listing](https://sc25.supercomputing.org/proceedings/workshops/index.html)
