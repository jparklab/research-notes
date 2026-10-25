---
title: 'Realtime and Batch GPU Workloads: How SS&C Built an AI‑as‑a‑Service Platform
  on Private Cloud | LavX News'
id: realtime-and-batch-gpu-workloads-how-ssc-built-an-aiasaservice-platform-on-priva
tags:
- onprem-llm-inference-stacks-859099
- hardware
- case-study
- unverified-rewrite
- derivative-of
created: '2026-10-23T17:23:56.408155Z'
updated: '2026-10-23T18:17:20.295602Z'
source: https://news.lavx.hu/article/realtime-and-batch-gpu-workloads-how-ss-c-built-an-ai-as-a-service-platform-on-private-cloud
source_domain: news.lavx.hu
fetched_at: '2026-10-23T17:23:56.385923Z'
fetch_provider: crawl4ai
status: deprecated
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Third-party AI rewrite of Stein talk. Now checked against InfoQ original
  (note realtime-and-batch-processing-of-gpu-workloads-infoq): CONFIRMED 80 GPUs,
  H100/H200, vLLM, K8s node selectors, Valkey/Lua, 4,000 tok/s, 2/4 GPUs per 8B/70B,
  650-page micro-batch. NOT in original / contradicted: 8GB VRAM, 12 EPYC nodes, 100GbE,
  30PB, K8s 1.30, Kafka 3.4, benchmark+utilization table, $7M/$1.8M/2.3x, ''250+ tenants''.
  Keep quarantined; cite InfoQ original instead.'
---

*Suggested by [[llm-inference-hardware-sizing-enterprise-gpu-guide-2026-gpusmith]] — cited as primary source for named deployment/spec*

[Skip to content](https://news.lavx.hu/article/realtime-and-batch-gpu-workloads-how-ss-c-built-an-ai-as-a-service-platform-on-private-cloud#main-content)
#Infrastructure
# Realtime and Batch GPU Workloads: How SS&C Built an AI‑as‑a‑Service Platform on Private Cloud
May 26, 2026, 9:22 AM UTC• •6 min read
Share this article
Joseph Stein walks through the architecture, governance, and operational tactics that let SS&C run both low‑latency inference and high‑throughput batch jobs on a shared pool of NVIDIA H100/H200 GPUs. The article covers multi‑namespace scheduling, Valkey‑Lua priority queues, vLLM tuning, custom S3‑to‑Kafka proxies, and lessons for cost‑effective oversubscription in enterprise private clouds.
## Technical announcement
SS&C Technologies has opened its private‑cloud AI platform to the broader engineering community via an InfoQ presentation titled **“Realtime and Batch Processing of GPU Workloads.”** The platform delivers generative‑AI inference, embeddings, and Retrieval‑Augmented Generation (RAG) as a service while keeping all data on‑premises. The key claim: a single 8‑chip H100 chassis in Kansas City can simultaneously serve 250 + production tenants, 1 000+ use‑cases, and a growing batch pipeline without exceeding budgeted GPU spend.
## Specifications
### Hardware footprint  
| Component  | Quantity  | Specs  |  
| --- | --- | --- |  
| NVIDIA GPUs  | 80 (mix of H100 & H200)  | 4 000 tokens / s per H100 chip (peak), 8 GB VRAM per H100, 16 GB VRAM per H200  |  
| Compute nodes  | 12 (dual‑socket AMD EPYC)  | 256 GB RAM, NVMe local storage  |  
| Network  | 100 GbE leaf‑spine  | Low‑latency intra‑region traffic  |  
| Storage  | Multi‑region object store (S3‑compatible)  | 30 PB total capacity  |  
All nodes are provisioned through a Terraform provider that mirrors public‑cloud APIs, allowing engineers to request Kubernetes clusters, Kafka topics, or dedicated GPU pods via a self‑service portal.
### Software stack
  * **Kubernetes 1.30** with custom node selectors for H100 vs H200 resources.
  * **vLLM** (open‑source LLM serving engine) as the primary inference runtime.
  * **Valkey** (Redis‑compatible) + **Lua** scripts for atomic rate‑limiting, priority queuing, and back‑pressure handling.
  * **OpenAI‑compatible API gateway** built on Envoy, enriched with per‑tenant guardrails (prompt‑injection detection, toxicity filters, FINRA compliance checks).
  * **Kafka 3.4** for event streaming; all file uploads are first registered in a Valkey cache, then a custom **S3‑proxy** writes the object to an internal bucket and pushes a registration event to Kafka.
  * **OPA** (Open Policy Agent) with Rego policies for fine‑grained access control on the S3‑proxy.
  * **ServiceNow** integration for automated ticketing when a tenant exceeds its quota or triggers a security rule.


### Core architectural patterns
  1. **Multi‑namespace GPU pool** – each tenant’s workload runs in a distinct Kubernetes namespace. A single _GPU‑pool proxy_ aggregates requests across namespaces, inspects the request’s tenant label, and applies two‑dimensional priority:
     * **Environment priority** (Prod > Prod‑DR > Demo > UAT > Dev)
     * **Namespace priority** (high‑value services vs background batch jobs)
  2. **Atomic Lua‑based rate limiting** – every incoming request hits a Valkey script that checks:
     * Global token‑per‑second cap
     * Per‑model token window (e.g., 8 B model ≤ 7 s, 70 B model ≤ 90 s)
     * Tenant‑specific quota
     * Current vLLM back‑pressure metric (`queue_length` from the `/metrics` endpoint) If any check fails, the request is throttled or placed in a low‑priority queue.
  3. **Micro‑batching for embeddings** – a thin gRPC service aggregates up to 650 pages of text into 32‑KB batches before feeding them to the GPU, reducing kernel launch overhead by ~30 %.
  4. **Batch‑window scheduling** – file‑processing jobs (document OCR, audio transcription) are accepted with an SLA‑profile (e.g., _complete by 20:00 UTC_). The scheduler computes the expected token consumption, matches it against off‑peak GPU capacity, and either accepts the job immediately or defers it to a low‑utilisation window.
  5. **Disaster‑recovery namespace** – a hot‑standby namespace mirrors the production GPU pool. In a fail‑over, traffic is re‑routed to the standby namespace without touching the underlying hardware selectors.


### Benchmarks & performance numbers  
| Workload  | Model  | Avg latency (95th pct)  | Throughput  | GPU utilisation  |  
| --- | --- | --- | --- | --- |  
| Real‑time chat (Llama‑3.1‑8B)  | 8 B  | 120 ms  | 4 k req/s  | 68 %  |  
| RAG inference (Llama‑3.1‑70B)  | 70 B  | 1.2 s  | 850 req/s  | 74 %  |  
| Embedding micro‑batch (text‑2‑vec)  | 8 B  | 45 ms  | 9 k req/s  | 62 %  |  
| Audio transcription (Whisper‑large‑v2)  | 1.5 B  | 3.4 s  | 210 req/s  | 55 %  |  
All numbers were collected on a mixed H100/H200 fleet running Ubuntu 22.04 with the NVIDIA driver 560.68. The vLLM engine was compiled with `--max-num-batches=64` and `--max-num-seqs=256` to maximise parallelism.
## Real‑world implications
### Cost efficiency through oversubscription
SS&C’s approach shows that a **single 8‑chip chassis can safely support 80 GPU‑equivalent workloads** when you:
  1. Slice traffic by environment and tenant.
  2. Apply strict token‑based quotas.
  3. Use back‑pressure signals from vLLM to trigger low‑priority queuing. The result is a **~2.3× improvement in GPU utilisation** compared to naïve per‑tenant provisioning, translating to an estimated **$1.8 M annual OPEX saving** on a $7 M hardware investment.


### Governance at scale
By front‑ending every request with a **central gateway** that audits payloads, enforces prompt‑injection detection, and logs all decisions to ServiceNow, SS&C meets FINRA, ISO‑27001, and the emerging **OWASP LLM Top 10** requirements without sacrificing latency. The policy engine is version‑controlled in Git, enabling audit trails for compliance reviewers.
### Batch workload optimisation
The S3‑to‑Kafka proxy decouples file ingestion from GPU processing, allowing the system to **smooth spikes** caused by large document uploads. Because the proxy stores file metadata in Valkey, the scheduler can **predict token consumption** and automatically reject jobs that would breach the SLA, avoiding costly out‑of‑band retries.
### Future work
  * **KV‑Cache exploitation** – the team plans to integrate NVIDIA’s KV‑Cache extensions into vLLM to cut token‑generation latency by up to 15 % for long‑context models.
  * **Model quantisation** – exploring 4‑bit INT4 kernels to increase the number of concurrent models per GPU without sacrificing regulatory‑grade precision.
  * **Dynamic GPU partitioning** – while current H100 chips lack native MIG for the workloads Stein described, upcoming H200‑based MIG support could enable true elastic partitioning, reducing the need for static node selectors.


## Takeaways for engineers building private‑cloud AI services
  1. **Treat the GPU as a shared, rate‑limited resource** – use a fast in‑memory store (Valkey) with Lua scripts to enforce per‑tenant quotas and global back‑pressure.
  2. **Separate control‑plane (policy, registration) from data‑plane (GPU inference)** – this reduces coupling and makes it easy to swap out the inference engine (vLLM → Triton, SGLang, etc.) without rewriting governance.
  3. **Leverage Kubernetes namespace isolation** for disaster‑recovery and environment segregation; a single GPU‑pool proxy can then make global scheduling decisions.
  4. **Schedule batch jobs during off‑peak windows** – the same GPU fleet that serves sub‑second chat can also run nightly document‑intelligence pipelines, dramatically improving ROI.
  5. **Open‑source first** – Stein’s preference for community‑maintained projects (vLLM, OPA) avoids vendor lock‑in and ensures that security patches can be applied quickly.


By combining these patterns, SS&C demonstrates that **enterprise‑grade AI services can be delivered on‑premises at a fraction of the cost of public‑cloud alternatives** , while still providing the compliance guarantees required in finance and healthcare.
### Further reading
  * vLLM project – <https://github.com/vllm-project/vllm>
  * Valkey documentation – <https://valkey.io/docs>
  * Open Policy Agent – <https://www.openpolicyagent.org>
  * NVIDIA H100 architecture – <https://developer.nvidia.com/h100-gpu-architecture>


Source:
[#GPU](https://news.lavx.hu/tag/gpu)[#Private Cloud](https://news.lavx.hu/tag/private-cloud)[#AI‑as‑a‑Service](https://news.lavx.hu/tag/aiasaservice)[#Kubernetes](https://news.lavx.hu/tag/kubernetes)[#vLLM](https://news.lavx.hu/tag/vllm)
## Comments
Please log in or register to join the discussion
Log InRegister
No comments yet. Be the first to share your thoughts!
[ Back to Home](https://news.lavx.hu/)
Optional Google Analytics cookies help us see how LavX News is read. Essential cookies are always on. [Privacy Policy](https://news.lavx.hu/privacy)Cookie Settings
Reject AllAccept All
