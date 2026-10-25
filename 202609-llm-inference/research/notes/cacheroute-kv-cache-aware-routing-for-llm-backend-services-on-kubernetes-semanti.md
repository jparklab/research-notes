---
title: 'CacheRoute: KV-Cache-Aware Routing for LLM Backend Services on Kubernetes
  | Semantic Scholar'
id: cacheroute-kv-cache-aware-routing-for-llm-backend-services-on-kubernetes-semanti
tags:
- llmd-kv-cache-routing-c665d5
- academic-comparison
- kv-cache-routing
- gie-compatible-router
- positive-result
created: '2026-09-14T13:02:14.576717Z'
updated: '2026-10-10T14:52:17.311027Z'
source: https://www.semanticscholar.org/paper/d4397dcaf375e91a01181f98ea3b78f7bf502972
source_domain: www.semanticscholar.org
fetched_at: '2026-09-14T13:02:14.557383Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'CacheRoute (Pi, Zhang, Shawulieti — CMU/Northeastern/UC, EITCE 2026, IEEE
  DOI 10.1109/EITCE70137.2026.11634378) is an independent, non-llm-d-maintainer KV-cache-aware
  router implemented as its own Endpoint Picker compatible with the Kubernetes Gateway
  API Inference Extension — a different work from the same-named Meta arXiv:2608.19677
  paper. It jointly scores prefix similarity, real-time GPU memory utilization, and
  predicted TTFT. Evaluated on a 4-node GPU cluster against 5 baselines using LMSYS-Chat-1M
  prompts across short-QA, multi-turn-chat, and RAG-like long-context workload profiles:
  at 8 req/s mixed workload it reduces p95 TTFT by 11-43% (up to 54% at high load),
  improves KV-cache hit rate by 7-51 percentage points, and lowers peak GPU memory
  by 3-13 percentage points versus baselines, with comparable or higher throughput.
  The fetched page is a Semantic Scholar reference/citation listing (34 references,
  7 forward citations), not the full paper text — IEEE Xplore and ResearchGate both
  blocked automated fetch and no open-access copy exists per Unpaywall. The abstract
  reports uniformly positive results; it contains no mention of skewed/low-recoverable-reuse
  adversarial conditions, hotspotting, or underperformance versus plain load balancing.'
utility_score: 14.0
---

[Skip to search form](https://www.semanticscholar.org/paper/CacheRoute%3A-KV-Cache-Aware-Routing-for-LLM-Backend-Pi-Zhang/d4397dcaf375e91a01181f98ea3b78f7bf502972#search-form)[Skip to main content](https://www.semanticscholar.org/paper/CacheRoute%3A-KV-Cache-Aware-Routing-for-LLM-Backend-Pi-Zhang/d4397dcaf375e91a01181f98ea3b78f7bf502972#main-content)[Skip to account menu](https://www.semanticscholar.org/paper/CacheRoute%3A-KV-Cache-Aware-Routing-for-LLM-Backend-Pi-Zhang/d4397dcaf375e91a01181f98ea3b78f7bf502972#account-menu)
  * DOI:[10.1109/EITCE70137.2026.11634378](https://doi.org/10.1109/EITCE70137.2026.11634378)
  * Corpus ID: 291039560


# CacheRoute: KV-Cache-Aware Routing for LLM Backend Services on Kubernetes

```
@article{Pi2026CacheRouteKR,
  title={CacheRoute: KV-Cache-Aware Routing for LLM Backend Services on Kubernetes},
  author={Shao-Rui Pi and Shirui Zhang and Saina Shawulieti},
  journal={2026 10th International Conference on Electronic Information Technology and Computer Engineering (EITCE)},
  year={2026},
  pages={695-700},
  url={https://api.semanticscholar.org/CorpusID:291039560}
}
```

  *   * Published in 10th International Conference… 1 June 2026
  * Computer Science
  * 2026 10th International Conference on Electronic Information Technology and Computer Engineering (EITCE)


This work proposes CacheRoute, a KV-cache-aware routing framework implemented as an Endpoint Picker compatible with the Kubernetes Gateway API Inference Extension, and considers prefix similarity for KV cache reuse, real-time GPU memory utilization, and predicted time-to-first-token (TTFT) to route each request to the highest-scoring vLLM inference instance.Expand
Save to LibrarySave
Create AlertAlert
Share
7 Citations
## Figures and Tables from this paper
  * [ figure 1](https://www.semanticscholar.org/paper/CacheRoute%3A-KV-Cache-Aware-Routing-for-LLM-Backend-Pi-Zhang/d4397dcaf375e91a01181f98ea3b78f7bf502972/figure/0)
  * [ figure 2](https://www.semanticscholar.org/paper/CacheRoute%3A-KV-Cache-Aware-Routing-for-LLM-Backend-Pi-Zhang/d4397dcaf375e91a01181f98ea3b78f7bf502972/figure/1)
  * [ figure 3](https://www.semanticscholar.org/paper/CacheRoute%3A-KV-Cache-Aware-Routing-for-LLM-Backend-Pi-Zhang/d4397dcaf375e91a01181f98ea3b78f7bf502972/figure/2)
  * [ table I](https://www.semanticscholar.org/paper/CacheRoute%3A-KV-Cache-Aware-Routing-for-LLM-Backend-Pi-Zhang/d4397dcaf375e91a01181f98ea3b78f7bf502972/figure/3)


## 7 Citations
Citation Type
Has PDF
Author
More Filters
More Filters
Filters
Sort by Relevance Sort by Most Influenced Papers Sort by Citation Count Sort by Recency
### [Difficulty-Aware Hybrid Routing for Cost-Efficient Long-Context LLM Inference](https://www.semanticscholar.org/paper/Difficulty-Aware-Hybrid-Routing-for-Cost-Efficient-Gu-Xiao/b105f84398b0c0b66f2157211e3885b6566bc586)
Computer Science
2026 6th International Conference on Intelligent…
  * 2026


The Difficulty-Aware Hybrid Router is proposed, a lightweight framework that jointly leverages query-level, context-level, and draft-level features to classify each request into one of three inference tiers: direct answering by a small model, small-model answering with lightweight verification, or full processing by a large model.Expand
  * 1 Excerpt


### [Policy-Constrained LLM Repair for Kubernetes Backend Configurations](https://www.semanticscholar.org/paper/Policy-Constrained-LLM-Repair-for-Kubernetes-Liu-Jiang/efe6f662bec278596e94846a33f9c5001c832129)
Computer Science
2026 7th International Conference on Artificial…
  * 2026


KubeRepair is proposed, a policy-constrained repair framework that integrates LLM-based patch generation with a multi-stage validation pipeline that produces smaller, safer patches with fewer residual security issues.Expand
  * 1 Excerpt


### [Confidence-Conflict Routing Between Parametric Memory, RAG, and Long-Context LLMs](https://www.semanticscholar.org/paper/Confidence-Conflict-Routing-Between-Parametric-RAG%2C-Fang-Zhou/2373167b8f1ccc363eee77539a05914e992f42e4)
Computer Science
2026 11th International Conference on Electronic…
  * 2026


This work proposes Confidence-Conflict Routing (CCR), a framework that makes a three-way routing decision by jointly leveraging two signals: the model's self-assessed confidence and a passage-induced disagreement score serving as a proxy for evidence conflict.Expand
  * 1 Excerpt


### [ErrorChain: Error Cascade Detection and Early Truncation in Multi-Step Agents](https://www.semanticscholar.org/paper/ErrorChain%3A-Error-Cascade-Detection-and-Early-in-Chen-Chen/29fa9044137b4220f5d66263b1b69c335c60041b)
Computer Science
2026 11th International Conference on Electronic…
  * 2026


ErrorChain, a lightweight monitoring framework that intercepts the agent's reasoning pipeline between consecutive steps, combines a semantic coherence score measuring alignment between the current output and the task goal, and a state deviation metric quantifying trajectory drift across successive steps.Expand
  * 1 Excerpt


### [PrivCollab-Agent: Privacy-Preserving Multi-Agent Collaboration Under Partial Information](https://www.semanticscholar.org/paper/PrivCollab-Agent%3A-Privacy-Preserving-Multi-Agent-Jiang-Xiao/3da22daae5555f8f157ee8abd54dbb60498678f2)
Computer Science
2026 7th International Conference on Artificial…
  * 2026


This paper proposes PrivCollab-Agent, a privacy-preserving multi-agent collaboration framework in which agents exchange only claim-level messages rather than raw private contexts, and reveals that claim-level communication reduces inter-agent communication costs by over 60%, offering a practical pathway toward privacy-aware multi-agent deployment.Expand
  * 1 Excerpt


### [CauSAL-Rec: Causally-Consistent LLM Agent for Faithful and Controllable Explainable Recommendation](https://www.semanticscholar.org/paper/CauSAL-Rec%3A-Causally-Consistent-LLM-Agent-for-and-Ding-Chai/63e19cbbb087494357512a85af1005ba0e2b0b79)
Computer Science
2026 6th International Conference on Intelligent…
  * 2026


This paper proposes CauSAL-Rec, a novel framework that unifies recommendation and explanation through a causally-consistent approach that constructs a Structural Causal Model to capture causal pathways from user contexts to satisfaction outcomes.Expand
  * 1 Excerpt


### [AgentDropout: Dynamic Redundancy Elimination for Multi-Agent Collaboration Efficiency](https://www.semanticscholar.org/paper/AgentDropout%3A-Dynamic-Redundancy-Elimination-for-Xiao-Guo/739e8b9eb096c7948c756009f1323b2e382b4073)
Computer Science
2026 8th International Conference on Electronics…
  * 2026


AgentDropout is a dynamic strategy inspired by dropout regularization in neural networks, which selectively deactivates low-contribution agents during multi-agent collaboration, suggesting that dynamic agent deactivation may be useful for deploying multi-agent LLM systems under computational budget constraints.Expand
  * 1 Excerpt


## 34 References
Citation Type
Has PDF
Author
More Filters
More Filters
Filters
Sort by Relevance Sort by Most Influential Papers Sort by Citation Count Sort by Recency
### [Efficient Memory Management for Large Language Model Serving with PagedAttention](https://www.semanticscholar.org/paper/Efficient-Memory-Management-for-Large-Language-with-Kwon-Li/83b90f4a0ae4cc214eb3cc140ccfef9cd99fac05)
Computer Science
[Symposium on Operating Systems Principles](https://www.semanticscholar.org/venue?name=Symposium%20on%20Operating%20Systems%20Principles)
  * 2023


This work proposes PagedAttention, an attention algorithm inspired by the classical virtual memory and paging techniques in operating systems, and builds vLLM, an LLM serving system that achieves near-zero waste in KV cache memory and flexible sharing of KV Cache within and across requests to further reduce memory usage.Expand
  * 1 Excerpt


### [CacheGen: KV Cache Compression and Streaming for Fast Large Language Model Serving](https://www.semanticscholar.org/paper/CacheGen%3A-KV-Cache-Compression-and-Streaming-for-Liu-Li/40e565e070fde823097507fd6830cfa6944df95d)
+11 authors [Junchen Jiang](https://www.semanticscholar.org/author/Junchen-Jiang/2253842783)
Computer Science
[Conference on Applications, Technologies…](https://www.semanticscholar.org/venue?name=Conference%20on%20Applications%2C%20Technologies%2C%20Architectures%2C%20and%20Protocols%20for%20Computer%20Communication)
  * 2024


CacheGen is a fast context-loading module for LLM systems that reduces the KV cache size by 3.5--4.3x and the total delay in fetching and processing contexts by 3.2--3.7x with negligible impact on the LLM response quality.Expand
  * 1 Excerpt


### [CacheBlend: Fast Large Language Model Serving for RAG with Cached Knowledge Fusion](https://www.semanticscholar.org/paper/CacheBlend%3A-Fast-Large-Language-Model-Serving-for-Yao-Li/57e2b2dd7506c4d3bbcab68acc2e4f81e0731bd2)
+6 authors [Junchen Jiang](https://www.semanticscholar.org/author/Junchen-Jiang/2249947059)
Computer Science
[European Conference on Computer Systems](https://www.semanticscholar.org/venue?name=European%20Conference%20on%20Computer%20Systems)
  * 2025


CacheBlend is presented, a scheme that reuses the precomputed KV caches, regardless prefix or not, and selectively recomputes the KV values of a small subset of tokens to partially update each reused KV cache, allowing CacheBlend to store KV caches in slower devices with more storage capacity while retrieving them without increasing the inference delay.Expand


  * 1 Excerpt


### [TokenPool-Scheduler: Token-Level GPU Pooling and Resource Slicing for Multi-Model Co-Location](https://www.semanticscholar.org/paper/TokenPool-Scheduler%3A-Token-Level-GPU-Pooling-and-Zhuang-Huang/69205ad8904c34a3fff12427c778659ce7b27580)
Computer Science
2026 IEEE 8th International Conference on…
  * 2026


TokenPool-Scheduler is presented, a token-level GPU pooling and resource slicing framework that breaks LLM inference execution into fine-grained segments that achieves up to 33% higher GPU utilization and 63% improvement in goodput compared to static allocation schemes.Expand
  * 1 Excerpt


### [DistServe: Disaggregating Prefill and Decoding for Goodput-optimized Large Language Model Serving](https://www.semanticscholar.org/paper/DistServe%3A-Disaggregating-Prefill-and-Decoding-for-Zhong-Liu/72f77a393079431e4207b3afe678ee80b420e6f8)
Computer Science
[USENIX Symposium on Operating Systems Design and…](https://www.semanticscholar.org/venue?name=USENIX%20Symposium%20on%20Operating%20Systems%20Design%20and%20Implementation)
  * 2024


DistServe significantly improves LLM serving performance in terms of the maximum rate that can be served within both TTFT and TPOT constraints on each GPU, and assigns prefill and decoding computation to different GPUs, hence eliminating prefill-decoding interferences.Expand
  * 1 Excerpt


### [Splitwise: Efficient Generative LLM Inference Using Phase Splitting](https://www.semanticscholar.org/paper/Splitwise%3A-Efficient-Generative-LLM-Inference-Using-Patel-Choukse/ad9146d98ae95bbeeef460abe083ecc2c4798672)
+4 authors [Ricardo Bianchini](https://www.semanticscholar.org/author/Ricardo-Bianchini/2268756471)
Computer Science
[International Symposium on Computer Architecture](https://www.semanticscholar.org/venue?name=International%20Symposium%20on%20Computer%20Architecture)
  * 2024


This work proposes Splitwise, a model deployment and scheduling technique that splits the two phases of LLM inference requests on to separate machines and enables phase-specific resource management using hardware that is well suited for each phase.Expand
  * 1 Excerpt


### [Taming Throughput-Latency Tradeoff in LLM Inference with Sarathi-Serve](https://www.semanticscholar.org/paper/Taming-Throughput-Latency-Tradeoff-in-LLM-Inference-Agrawal-Kedia/20f090e35ad598fba2404e550c2462dc9da03a10)
Computer Science
[USENIX Symposium on Operating Systems Design and…](https://www.semanticscholar.org/venue?name=USENIX%20Symposium%20on%20Operating%20Systems%20Design%20and%20Implementation)
  * 2024


Sarathi-Serve introduces chunked-prefills which splits a prefill request into near equal sized chunks and creates stall-free schedules that adds new requests in a batch without pausing ongoing decodes, and uniform batches in Sarathi-Serve ameliorate the imbalance between iterations resulting in minimal pipeline bubbles.Expand
  * 1 Excerpt


### [Efficient LLM Scheduling by Learning to Rank](https://www.semanticscholar.org/paper/Efficient-LLM-Scheduling-by-Learning-to-Rank-Fu-Zhu/4036c337a8cba9c724df31d8041b9961b9b72c3f)
Computer Science
[Neural Information Processing Systems](https://www.semanticscholar.org/venue?name=Neural%20Information%20Processing%20Systems)
  * 2024


A novel scheduler for LLM inference and serving is developed that can approximate the shortest-job-first (SJF) schedule better than existing approaches and is integrated with the state-of-the-art LLM serving system.Expand
  * 1 Excerpt


### [Budgeted Multi-Agent Routing: Adaptive Role Assignment and Communication Compression for Efficient LLM-Agent Collaboration](https://www.semanticscholar.org/paper/Budgeted-Multi-Agent-Routing%3A-Adaptive-Role-and-for-Yang-Wu/774b50f401ceb25613f4e8ff9dbc0d1feca235a3)
Computer Science
2025 5th International Conference on Electronic…
  * 2025


The proposed BMAR framework provides a principled framework for deploying cost-effective multi-agent systems in resource-constrained enviro nments and achieves superior Pareto efficiency.Expand
  * 1 Excerpt


### [FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning](https://www.semanticscholar.org/paper/FlashAttention-2%3A-Faster-Attention-with-Better-and-Dao/823ca4778e1027f2f0b356df051d762dcecaaba0)
Computer Science
[International Conference on Learning…](https://www.semanticscholar.org/venue?name=International%20Conference%20on%20Learning%20Representations)
  * 2024


This work tweak the algorithm to reduce the number of non-matmul FLOPs, and parallelize the attention computation, even for a single head, across different thread blocks to increase occupancy, and distribute the work between warps to reduce communication through shared memory.Expand
  * 1 Excerpt


## Related Papers
Showing 1 through 3 of 0 Related Papers
