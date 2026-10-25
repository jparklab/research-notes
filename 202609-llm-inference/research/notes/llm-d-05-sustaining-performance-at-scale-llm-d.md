---
title: 'llm-d 0.5: Sustaining Performance at Scale | llm-d'
id: llm-d-05-sustaining-performance-at-scale-llm-d
tags:
- llmd-kv-cache-routing-c665d5
- llm-d-release
- hierarchical-kv-offloading
- nixl
- locus-default-scoring-configuration
created: '2026-09-14T11:53:20.958584Z'
updated: '2026-09-15T04:55:24.026558Z'
source: https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale
source_domain: llm-d.ai
fetched_at: '2026-09-14T11:53:20.938894Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'llm-d v0.5 release blog (''Sustaining Performance at Scale''), shifting
  focus from peak latency (v0.4) to operational rigor: reproducible benchmarking,
  Hierarchical KV Offloading, LoRA/HA-aware scheduling, resilient networking, and
  scale-to-zero autoscaling. Introduces the llm-d FS backend (a storage connector
  on vLLM''s native offloading interface) establishing a 3-tier memory hierarchy (GPU/CPU/Disk):
  decouples cache capacity from local node memory, enables cross-replica reuse via
  a shared persistent KV store (new nodes ''hydrate'' from the shared tier instead
  of cold-starting), and uses async I/O with parallel workers. Benchmark on Llama-3.1-70B
  / 4x H100 with IBM Storage Scale shows GPU-only deployments collapse once HBM saturates,
  while storage-backed offload sustains ~185k tok/s at 250 concurrent users (13.9x
  improvement over collapse). Scheduling advances: active-active HA via a KV-Events
  subscription manager with dynamic pod discovery (ZMQ subscriptions tied to pod lifecycle)
  and LoRA-precise prefix caching that routes by adapter-specific cache locality to
  avoid the ''thundering herd'' problem. Networking: contributes a UCCL (Unified Collective
  Communication Library) transport backend to NIXL for P/D KV transfer — under network
  congestion, UCCL degraded only 7.1% (359ms to 384ms) vs UCX''s 17.1% (362ms to 424ms)
  on a 4GB KV-cache payload over 200Gb/s OpenShift interconnects, a 2.4x resilience
  improvement. Also reports Wide-EP throughput on B200 (~50k output tok/s at 16P/16D)
  and inference-scheduling latency gains on Qwen3-32B/H100 (up to 109% higher throughput,
  99% lower TTFT vs baseline k8s Service). Roadmap: predictive TTFT/TPOT-based routing,
  proactive KV ''pinning'' APIs, and end-to-end distributed tracing.'
utility_score: 12.0
---

[Skip to main content](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
In our previous release ([v0.4](https://llm-d.ai/blog/llm-d-v0.4-achieve-sota-inference-across-accelerators)), we focused on improving the end-to-end latency of production inference, introducing speculative decoding and extending prefill/decode disaggregation across a broader set of accelerator architectures. That work established llm-d’s ability to deliver state-of-the-art latency along the critical serving path. Sustaining low latency increasingly depended on how KV-cache pressure is handled once GPU memory is saturated, whether cached state can be reused across replicas instead of being repeatedly rebuilt, and how requests are routed when workloads mix adapters, models, and availability requirements.
With v0.5, llm-d expands its focus from peak performance to the operational rigor required to sustain performance at scale. This release prioritizes reproducibility, resilience, and cost efficiency, with concrete improvements across the following areas:
  1. **Developer Experience and reproducibility:** We have simplified the benchmarking workflow with dedicated, in-guide benchmark support, allowing users to validate each “well-lit path” with a single command.
  2. **Hierarchical KV Offloading:** A new storage architecture decouples cache capacity from GPU memory through native CPU and filesystem tiers.
  3. **Advanced Scheduling:** Cache-aware routing now supports LoRA adapters and active-active high availability.
  4. **Resilient Networking:** A new transport backend (UCCL) improves stability in congested networks.
  5. **Autoscaling Updates:** We have introduced scale-to-zero capabilities for cost-efficient intermittent workloads.


##  **Updated developer experience**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#updated-developer-experience "Direct link to updated-developer-experience")
A core challenge in distributed inference is the variance between reported benchmark numbers and realized production performance. In v0.5, we address this by adopting the "Research Paper Principle": the requirement that every performance claim be accompanied by a reproducible, version-controlled configuration.
  * **Simplified benchmarking (one guide, one benchmark):** We have moved away from generic "one-size-fits-all" scripts. v0.5 introduces in-guide benchmark support and a standalone benchmark script. Each "well-lit path" (e.g., [Inference-Scheduling](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md#benchmarking), [Wide EP](https://github.com/llm-d/llm-d/blob/main/guides/wide-ep-lws/README.md#benchmarking), [Disaggregated](https://github.com/llm-d/llm-d/blob/main/guides/pd-disaggregation/README.md#benchmarking)) now has a defined benchmark configuration that reproduces the benchmark results shown in the guides.
  * **Use cases and personalities:** The new harness supports distinct "personalities" or use cases. While the “in-guide” benchmarking is targeted at “feature developers” who quickly and efficiently wanted to check if their code changes had a positive effect, taking into account a baseline, the llm-d-benchmark has a toolset for a variety of other users. For “config tuners”, aiming to carry out parameter exploration sweeps, there is support for [experiments](https://github.com/llm-d/llm-d-benchmark/blob/main/docs/doe.md) crafted with the purpose of finding optimal configurations and reference architecture. An llm-d service owner can track performance regressions on his production stack by selecting a mix of [workloads](https://github.com/llm-d/llm-d-benchmark/blob/main/docs/run.md#workloads) to be periodically run.
  * **Parameter sweep exploration:** Capacity planning often relies on trial-and-error. The new [Configuration Explorer](https://github.com/llm-d/llm-d-benchmark/tree/main/config) allows analysts, researchers, or customers to perform their own parameter sweep exploration, simulating activation memory usage against specific hardware constraints. This allows this category of users to validate hardware feasibility and generate memory-safe configurations prior to provisioning physical resources.


##  **Performance update**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#performance-update "Direct link to performance-update")
With the overhauled benchmarking harness, we have re-validated our core architectural patterns. Below are the updated performance numbers for v0.5, representing the "well-lit paths" enabled by this release. These results are fully reproducible using the configurations provided in the corresponding guides.
###  **Throughput-oriented: Wide-EP on Nvidia B200**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#throughput-oriented-wide-ep-on-nvidia-b200 "Direct link to throughput-oriented-wide-ep-on-nvidia-b200")
For batch-intensive workloads, throughput is the primary metric. We validated a Wide-EP topology on a cluster of Nvidia B200 GPUs, specifically targeting the "Batch Processing" use case where latency constraints are relaxed in favor of massive token generation.
  * **Topology:** 16x Prefill GPUs / 16x Decode GPUs (EP=16, DP=16, TP=1)
  * **Workload:** Random dataset, 1k/1k input/output length
  * **Result:** ~50k output tokens/sec total throughput (~3.1k output tokens/sec per decode GPU).


_Figure 1: Wide-EP throughput on NVIDIA B200 for batch-oriented inference. Total output throughput scales with decode parallelism in a disaggregated 16×16 prefill/decode topology, demonstrating efficient utilization of B200 GPUs for batch-intensive workloads where latency constraints are relaxed._
###  **Latency-oriented: Inference Scheduling**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#latency-oriented-inference-scheduling "Direct link to latency-oriented-inference-scheduling")
For workloads with shared context and compute reuse opportunities, intelligent inference scheduling maximizes cache reuse and increases throughput. We validated our inference scheduling guides on a cluster deploying Qwen/Qwen3-32B, specifically targeting the "Multi-tenant SaaS" use case where shared customer contexts enable significant computational savings through prefix caching.
  * **Topology:** 8x vLLM pods / 16x NVIDIA H100 GPUs (TP=2)
  * **Workload:** Shared prefix synthetic, 150 groups × 5 prompts, 6k/1.2k/1k system/question/output length
  * **Result:** 4.5-11k output tokens/sec throughput, P50 TTFT 136–157ms, up to 109% higher throughput and 99% lower TTFT vs baseline Kubernetes service


_Figure 2: TTFT and throughput vs QPS on Qwen3-32B (8×vLLM pods, 16×NVIDIA H100). llm-d inference scheduling maintains near-zero TTFT and scales to ~120k tok/s, while baseline Kubernetes service degrades rapidly under load._
##  **Optimized offloading: Hierarchical KV-caching**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#optimized-offloading-hierarchical-kv-caching "Direct link to optimized-offloading-hierarchical-kv-caching")
In transformer-based inference, the KV-cache is a critical resource for reducing latency, yet it is strictly bounded by the finite capacity of GPU HBM and local CPU DRAM. Even in high-end nodes, local memory creates a hard ceiling on the number of concurrent contexts a system can serve before reverting to expensive prefill computations.
In v0.5, we introduce the `llm-d FS backend`, a storage connector that plugs into vLLM’s native offloading interface. This architecture effectively establishes a three-tier memory hierarchy (GPU, CPU, and Disk) to address the twin challenges of scale and sharing.
  * **Decoupling capacity from compute:** By offloading KV blocks to a shared file system, the system decouples cache capacity from local node memory. This allows the inference engine to sustain high throughput even as the active working set grows significantly beyond the aggregate RAM of the cluster.
  * **Cross-replica reuse:** Unlike local CPU caches, which are isolated to a single instance, a shared file system acts as a persistent, global store of KV states. New nodes added to the cluster can "hydrate" their cache immediately from the shared tier, bypassing the warm-up phase typically required for new replicas.
  * **Asynchronous design:** To minimize interference with the decoding loop, the backend utilizes fully asynchronous I/O and parallel worker threads. This ensures that the latency cost of fetching blocks from disk does not block the main compute path.


Our internal benchmarks illustrate that the primary value of this architecture is operational stability under load. As shown in Figure 3, standard GPU-only deployments experience a sharp performance collapse once HBM is saturated. In contrast, the storage-backed configuration creates a "performance floor," sustaining throughput as user concurrency increases well beyond local memory limits.
_Figure 3: KV-cache throughput under growing user concurrency on Llama-3.1-70B (4×NVIDIA H100 with IBM Storage Scale). 16K token requests with previously-seen prompts. GPU-only collapses once HBM is saturated; storage offload sustains ~185k tok/s as concurrency scales (13.9x improvement at 250 users)._
For high-performance computing environments, we have also validated Tiered Prefix Caching on Lustre, demonstrating how parallel file systems can be leveraged to provide persistence and fast throughput capabilities and minimize the latency overhead of this tertiary storage tier.
##  **Advancements in scheduling**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#advancements-in-scheduling "Direct link to advancements-in-scheduling")
In v0.5, the scheduler has evolved to handle more complex routing scenarios, specifically targeting multi-adapter workloads and high-availability topologies.
  * **Unified tokenization pipeline:** We have standardized the preprocessing stack by reusing the native vLLM Python module for all tokenization and rendering paths. This lays the groundwork towards disaggregated tokenization by converging on a single, comprehensive rendering pipeline built on vLLM’s evolving Renderer API.
  * **Active-active HA with dynamic discovery:** We have introduced a new subscription manager for KV-Events that moves away from static connections. Instead of relying on deployment-time configuration, the scheduler now dynamically discovers vLLM pods and manages ZeroMQ (ZMQ) subscriptions based on the pod lifecycle. This enables robust Active-Active multi-replica scheduler High-Availability, ensuring that cache state is tracked accurately even as replicas scale up or down dynamically.
  * **LoRA-precise prefix caching:** The precise scheduling path now fully supports LoRA adapters. This allows the scheduler to route requests based on the specific cache locality of LoRA adapters, preventing the "thundering herd" problem where every replica attempts to load every adapter and maximizing cache efficiency for multi-tenant workloads.


_Figure 4: LoRA-aware prefix-cache scheduling improves throughput. Prefix-cache aware scheduling mitigates LoRA overhead by minimizing effective compute and avoiding redundant LoRA kernel execution, delivering higher throughput than standard K8s round-robin load-balancing._
##  **Resilient networking: NIXL - UCCL backend**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#resilient-networking-nixl---uccl-backend "Direct link to resilient-networking-nixl---uccl-backend")
In disaggregated architectures, tail latency is governed by the efficiency of KV-cache transport between prefill and decode nodes. In v0.5, we have integrated our contribution of the UCCL (Unified Collective Communication Library) backend into the NIXL networking layer.
UCCL provides a unified abstraction over vendor-specific collective primitives (NCCL, RCCL, MCCL). Crucially, for point-to-point (P2P) operations, the dominant traffic pattern in context migration, UCCL implements a host-resident software transport stack. By managing transport logic on the CPU rather than relying solely on hardware offload, the backend enables fine-grained flow splitting and adaptive congestion control strategies. This architecture currently supports both native RDMA and GPUDirect TCP-X transport.
To quantify the impact of this architecture, we evaluated a Llama-3.1-8B deployment on an OpenShift cluster with 200 Gb/s interconnects, measuring the transfer latency of a 4 GB KV-cache payload.
_Figure 5: PD transfer latency growth under congestion. Comparing baseline vs. congested states for UCX and UCCL transport._
While baseline latency remained comparable between transports (~360ms), the architectures diverged significantly under network stress. When subjecting the cluster to heavy cross-traffic congestion:
  * **UCX backend:** Latency degraded from 362ms to 424ms (+17.1%).
  * **UCCL backend:** Latency degraded from 359ms to 384ms (+7.1%).


The UCCL backend demonstrated **2.4x greater resilience** to network contention (7.1% degradation vs. 17.1%). These results validate the use of host-driven congestion control for ensuring consistent tail latency in shared, multi-tenant production environments.
##  **Updates to autoscaling**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#updates-to-autoscaling "Direct link to updates-to-autoscaling")
In release v0.4, we introduced the Workload Variant Autoscaler (WVA) as an experimental feature. In v0.5, we have iterated on this foundation to support more aggressive cost-saving strategies for intermittent workloads.
  * **Scale-to-zero and from-zero:** We have implemented support for scaling inference pools down to zero replicas during idle periods. Unlike simple timeout-based shutdowns, this implementation uses a specialized activator component to handle the "cold start" sequence, ensuring that incoming requests trigger provisioning without being dropped. This capability is critical for cost-sensitive environments, such as development clusters or internal RAG applications that do not require 24/7 GPU allocation.
  * **Saturation-based scaling:** The control loop has been refined to better detect saturation points based on queue depth and KV-cache pressure, allowing for more responsive scaling decisions before latency SLOs are violated.


##  **Community and ecosystem**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#community-and-ecosystem "Direct link to community-and-ecosystem")
This progress wouldn't be possible without close collaboration with our broader community. llm-d exists within a rich ecosystem of open-source projects, and we are proud to contribute our findings and code back to the upstream. A few contribution highlights from this release.
  * **vLLM KV-Connector:** We worked closely with the vLLM maintainers to define and implement the [KV Offloading Connector](https://blog.vllm.ai/2026/01/08/kv-offloading-connector.html), ensuring that the storage hierarchy we built for llm-d rests on standard, upstream interfaces.
  * **NIXL Integration:** Our UCCL backend has been [merged into the NIXL 0.9 release](https://github.com/ai-dynamo/nixl/pull/895), enabling the wider community to benefit from the host-driven congestion control strategies we developed for disaggregated serving.


### Broader hardware ecosystem integration[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#broader-hardware-ecosystem-integration "Direct link to Broader hardware ecosystem integration")
We continue to validate llm-d across a broad and growing hardware ecosystem, reinforcing its role as a hardware-agnostic inference control plane. Additional results will be shared as validations complete.
##  **What Is Next?**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#what-is-next "Direct link to what-is-next")
The v0.5 release establishes a foundation for reproducible research and production stability. We invite the community to validate these findings using the new benchmarking tools available on our GitHub repository.
Looking ahead, our focus shifts to four key areas:
  * **Scheduling:** Moving toward predictive, latency-aware routing (using TTFT/TPOT targets rather than queue depth) and native batch inference support.
  * **Offloading:** Developing proactive state management APIs to "pin" critical context blocks and exploring semantic caching policies.
  * **Autoscaling:** Introducing pluggable optimizers that leverage queuing theory and ML for predictive scaling, alongside direct SLO integration.
  * **Observability:** Implementing end-to-end distributed tracing, from the gateway through the scheduler to the engine, to expose granular latency bottlenecks in disaggregated architectures.


We will publish a feature roadmap for v0.6, outlining the next set of priorities informed by ongoing community discussions and feedback.
##  **Upcoming Events**[​](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#upcoming-events "Direct link to upcoming-events")
Please stay tuned for updates on upcoming community events. You can follow these and other events where you can find the llm-d contributors on our [community events page](https://llm-d.ai/community/events).
Follow the project updates on [Twitter/X](https://twitter.com/_llm_d_), [Bluesky](https://bsky.app/profile/llm-d.ai), and [LinkedIn](https://www.linkedin.com/company/llm-d). You can also find recordings of our community calls as well as demos of current and upcoming features by being a subscriber to the [llm-d YouTube](https://www.youtube.com/@llm-d-project).
Contribute on [GitHub](https://github.com/llm-d), [join our community calls](https://red.ht/llm-d-public-calendar) (Wed 12:30pm ET), join the SIGs and come build with us!
  * [**Updated developer experience**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#updated-developer-experience)
  * [**Performance update**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#performance-update)
    * [**Throughput-oriented: Wide-EP on Nvidia B200**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#throughput-oriented-wide-ep-on-nvidia-b200)
    * [**Latency-oriented: Inference Scheduling**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#latency-oriented-inference-scheduling)
  * [**Optimized offloading: Hierarchical KV-caching**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#optimized-offloading-hierarchical-kv-caching)
  * [**Advancements in scheduling**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#advancements-in-scheduling)
  * [**Resilient networking: NIXL - UCCL backend**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#resilient-networking-nixl---uccl-backend)
  * [**Updates to autoscaling**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#updates-to-autoscaling)
  * [**Community and ecosystem**](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#community-and-ecosystem)
    * [Broader hardware ecosystem integration](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale#broader-hardware-ecosystem-integration)


