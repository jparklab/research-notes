---
title: Maximize AI Infrastructure Throughput by Consolidating Underutilized GPU Workloads
  | NVIDIA Technical Blog
id: maximize-ai-infrastructure-throughput-by-consolidating-underutilized-gpu-workloa
tags:
- onprem-llm-inference-stacks-859099
- gpu-sharing
- provenance-vendor-benchmark
- nvidia-nim
created: '2026-10-23T17:24:06.558733Z'
updated: '2026-10-23T17:25:06.054611Z'
source: https://developer.nvidia.com/blog/maximize-ai-infrastructure-throughput-by-consolidating-underutilized-gpu-workloads/
source_domain: developer.nvidia.com
fetched_at: '2026-10-23T17:24:06.537434Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'NVIDIA developer blog (25 Mar 2026): 3x A100 testbed with NIM Operator;
  voice pipeline (Parakeet ASR, Magpie TTS, Llama-3.1-Nemotron-Nano-VL-8B). Consolidating
  ASR+TTS onto one GPU: MIG (2x 3g.40gb) ~1.00 req/s per GPU vs time-slicing ~0.76
  req/s (32% lower), TTS mean latency 144.7 ms time-sliced vs 168.2 ms MIG; 100% success;
  light load 5 users, heavy 50 users. NVIDIA recommends MIG for production, time-slicing
  for dev/CI. Small-model (not LLM-serving) test, vendor-authored, vendor recommendation;
  also states MPS lacks hardware fault isolation (fatal error can reset GPU for all
  sharers).'
---

*Suggested by [[gpu-sharing-on-k8s-mig-vs-mps-vs-time-slicing-luca-berton]] — primary/vendor doc for GPU sharing claims*

English 中文
# Maximize AI Infrastructure Throughput by Consolidating Underutilized GPU Workloads
Mar 25, 2026 
By [Sagar Desai](https://developer.nvidia.com/blog/author/sagdesai/ "Posts by Sagar Desai") and [Adi Margolin](https://developer.nvidia.com/blog/author/adimargolin/ "Posts by Adi Margolin")
Like 


## AI-Generated Summary
  * GPU resource fragmentation in Kubernetes leaves lightweight ASR and TTS models occupying entire GPUs while using only a fraction of VRAM, reducing cluster density and increasing infrastructure costs.
  * Hardware-level partitioning with [NVIDIA Multi-Instance GPU (MIG)](https://developer.nvidia.com/mig) physically divides an A100 GPU into isolated instances with dedicated memory and compute, achieving ~1.00 req/s per GPU and strict fault isolation for production SLAs.
  * Software-based time-slicing enables multiple CUDA processes to share a GPU through context switching, reaching ~0.76 req/s per GPU but introducing scheduling overhead and lacking hardware fault isolation.
  * Benchmarks on a voice AI pipeline using [NVIDIA Parakeet 1.1B](https://build.nvidia.com/nvidia/parakeet-1_1b-rnnt-multilingual-asr) for ASR, [NVIDIA Magpie Multilingual](https://build.nvidia.com/nvidia/magpie-tts-multilingual) for TTS, and Llama-3.1-Nemotron-Nano-VL-8B-V1 for LLM show MIG provides higher throughput stability while time-slicing offers slightly lower per-request TTS latency.
  * Consolidating ASR and TTS on a single GPU frees compute for additional LLM instances, with MIG recommended for production scale and time-slicing suited for development or low-concurrency workloads.


### Next Steps
  * Try the [voice-voice-workflow repository](https://github.com/SDcodehub/voice-voice-workflow) to experiment with the pipeline.
  * Follow the [Implementation Guide](https://github.com/NVIDIA-AI-Blueprints/rag/blob/develop/docs/mig-deployment.md) to configure MIG profiles and deploy YAML manifests that eliminate resource fragmentation.
  * Deploy [NVIDIA NIM](https://docs.nvidia.com/nim/large-language-models/latest/introduction.html) pipelines to fully utilize ASR, TTS, and LLM workloads for maximum ROI.


Powered by NVIDIA Nemotron. AI-generated content may summarize information incompletely. Verify important information. [Learn more](https://www.nvidia.com/en-us/agreements/trustworthy-ai/terms/)
In production Kubernetes environments, the difference between model requirements and GPU size creates inefficiencies. Lightweight automatic speech recognition (ASR) or text-to-speech (TTS) models may require only 10 GB of VRAM, yet occupy an entire GPU in standard Kubernetes deployments. Because the scheduler maps a model to one or more GPUs and can’t easily share across GPUs across models, expensive compute resources often remain underutilized. 
Solving this isn’t just about cost reduction—it’s about optimizing cluster density to serve more concurrent users on the same world-class hardware. This guide details how to implement and benchmark GPU partitioning strategies, specifically NVIDIA Multi-Instance GPU (MIG) and time-slicing to fully use compute resources.
Using a production-grade voice AI pipeline as our testbed, we show how to combine models to maximize infrastructure ROI while maintaining >99% reliability and strict latency guarantees. 
## Addressing GPU resource fragmentation 
By default, the NVIDIA Device Plugin for Kubernetes shows GPUs as integer resources. A pod requests `nvidia.com/gpu: 1`, and the scheduler binds it to a physical device.
Large language models (LLMs) like NVIDIA Nemotron, Llama 3, or Qwen 7B/8B require dedicated compute to maintain low time to first token (TTFT) and high batch throughput. However, support models in a generative AI pipeline—embedding models, ASR, TTS, or guardrails—often use only a fraction of a card. Running these lightweight models on dedicated GPUs results in:
  * Low utilization: GPU compute utilization often hovers near 0-10%.
  * Cluster bloat: More nodes need provisioning to run the same number of pods.
  * Scaling friction: Adding a new capability requires a new physical GPU.


To solve this, we must break the 1:1 relationship between pods and GPUs.
## Architecture: Partitioning strategies 
We evaluated two primary strategies for GPU partitioning supported by the NVIDIA GPU Operator.
### Software-based partitioning: Time-slicing and MPS 
Time-slicing enables multiple NVIDIA CUDA processes to share a GPU by interleaving execution. It functions similarly to a CPU scheduler: context A runs, pauses, and context B runs.
  * Mechanism: Software-level scheduling through the CUDA driver.
  * Pros: Maximizes utilization. Enables “bursting”—if Pod A is idle, Pod B can use 100% of the GPU’s compute cores.
  * Cons: No hardware isolation. A memory overflow (OOM) in one pod may impact the shared execution context, and heavy compute in one pod can throttle neighbors (the “noisy neighbor” effect).


In addition to time-slicing, NVIDIA Multi-Process Service (MPS) offers an alternative software-based approach. MPS enables multiple processes to share GPU resources concurrently by using a server-client architecture. This provides more flexibility than MIG and is more resilient to certain issues like memory leaks compared to standard time-slicing. 
However, in production, both methods share a single execution context, limiting isolation. While modern MPS provides isolated virtual address spaces, it lacks hardware-level fault isolation. This means a fatal execution error or illegal memory access in one process will propagate across the shared context, potentially leading to a GPU reset that affects other processes sharing the card.
### MIG: The hardware approach to partitioning 
MIG physically partitions the GPU into separate instances, each with its own dedicated memory, cache, and streaming multiprocessors (SMs). To the OS and Kubernetes, these look like separate PCI devices.
  * Mechanism: Hardware-level isolation.
  * Pros: Strict quality of service (QoS). One workload can’t impact the performance or memory stability of another.
  * Cons: Rigid sizing. If a partition is idle, its compute resources can’t be “borrowed” by a neighbor.


While time-slicing offers flexibility, MIG is preferred for production environments where strict hardware-level fault isolation is required to meet enterprise SLAs. Hardware partitioning ensures that a memory error in one model cannot cause a cascading failure across the shared GPU—a critical requirement for mission-critical Voice AI.
## Experimental setup: The voice AI pipeline 
_Figure 3: Voice-to-voice AI workflow_
To validate these strategies in a production-realistic scenario, we used a multimodal voice-to-voice AI pipeline. This workload is ideal for benchmarking because it mixes three distinct traffic patterns:
  * ASR (streaming): Constant, low-compute stream processing with [NVIDIA Parakeet 1.1B](https://build.nvidia.com/nvidia/parakeet-1_1b-rnnt-multilingual-asr)
  * TTS (bursty): Idle for seconds, then spikes to 100% usage to generate audio in milliseconds with [NVIDIA Magpie Multilingual](https://build.nvidia.com/nvidia/magpie-tts-multilingual)
  * LLM (heavy): High-compute, high memory usage, [Llama-3.1-Nemotron-Nano-VL-8B-V1.](https://catalog.ngc.nvidia.com/orgs/nim/teams/nvidia/containers/llama-3.1-nemotron-nano-vl-8b-v1?version=1)


Before optimizing, it’s critical to understand our latency profile. In our voice-to-voice pipeline, the LLM is the dominant bottleneck. Under heavy loads, the LLM accounts for ~9 seconds of the total processing time. This delay can fluctuate significantly based on context length; for instance, high-input scenarios (like training users) or growing conversation histories increase processing overhead compared to short prompts. As history accumulates, the LLM must process more tokens before generating a response, extending the bottleneck that support models must be masked behind.
Consolidating support models like ASR and TTS provides a strategic path to maximize hardware utilization while maintaining end-to-end responsiveness. While consolidation may introduce a slight latency adjustment of 100-200 ms, the gains in infrastructure throughput and ROI are significant.
##  **Our hypothesis**
Consolidating ASR and TTS workloads on a single GPU preserves latency and jitter while freeing compute for additional LLM instances.
##  **Experiment**
_Figure 4. Experimental topology configurations_
We designed three distinct configurations for testing. In each round, we used three voice samples, waiting for the first response from LLM+TTS to complete. The setup used a Kubernetes cluster, models deployed using NVIDIA NIM, and managed by the NVIDIA NIM Operator. The worker node had access to three NVIDIA A100 Tensor Core GPUs.
  * Experiment 1: Baseline with three GPUs 
    * Setup: One dedicated GPU for each model (LLM, ASR, TTS).
    * Goal: Establish the “gold standard” for latency and throughput against which to measure optimization.
    * Resource: `nvidia.com/gpu: 1` per pod.
  * Experiment 2: Time-slicing with two GPUs 
    * Setup: LLM retains a dedicated GPU. ASR and TTS share GPU 0 using software-level time-slicing.
    * Goal: Test if dynamic scheduling can handle the “noisy neighbor” contention between streaming ASR and bursty TTS.
    * Resource: `nvidia.com/gpu: 1` (Shared via `replicas: 2`).
  * Experiment 3: MIG Partitioning with two GPUs 
    * Setup: LLM retains a dedicated GPU. GPU 0 is physically partitioned into two isolated instances.
    * Goal: Test if hardware isolation provides better stability than software scheduling.
    * Resource:` nvidia.com/mig-3g.40gb: 1` per pod.


Configuration Note: To achieve these topologies, we used specific configurations within the NVIDIA GPU Operator.
  * For Experiment 2, we used the `timeSlicing` configuration to advertise multiple replicas per physical GPU.
  * For Experiment 3, we applied a custom `mig-configs` ConfigMap to partition the GPU into two `3g.40gb` instances.


(For the exact `kubectl` commands and YAML manifests used to reproduce this setup, please see the Implementation Appendix at the end of this post.)
## Results 
To evaluate resource fragmentation, we tested the system with two distinct traffic patterns:
  * Light load: 5 concurrent users simulating ~135 seconds of sustained interaction.
  * Heavy load: 50 concurrent users simulating ~375 seconds of sustained interaction. 


_Figure 5. Throughput comparison_
Figure 5 compares generative AI inference throughput across traffic patterns. The data shows how partitioning affects process requests as concurrency increases, across baseline (dedicated GPUs), time-slicing (software sharing), and MIG (hardware partitioning) under light and heavy loads. All experiments have a 100% success rate, no failures. The current req/s is the reason for the LLM bottleneck in the pipeline.
### Mean latency metrics 
_Figure 6. Heavy load latenc_ y
_Figure 7: Light load latency_
The following analysis evaluates how different GPU partitioning strategies impact overall system efficiency and responsiveness.
### Throughput compared to latency 
Consolidating ASR and TTS workloads onto a single GPU results in an optimized pipeline, enabling the cluster to support more simultaneous AI streams. However, our benchmarks reveal a critical performance divergence between the two partitioning strategies:
  1. **MIG (hardware): Highest efficiency**


Experiment 3 achieved the highest per-unit productivity, reaching ~1.00 req/s per GPU. By providing dedicated hardware paths for each instance, MIG eliminates resource contention. Organizations can achieve near-full system capacity while effectively freeing up an entire GPU for other heavy LLM workloads.
  1. **Time-slicing (software): Higher density with overhead**


Experiment 2 showed that software-level sharing can also improve per-GPU density compared to the baseline, achieving ~0.76 req/s per GPU. However, the CUDA driver’s management of rapid context switches between streaming and bursty models introduces scheduling overhead. While functional, this software-based approach doesn’t reach the aggregate throughput efficiency provided by hardware partitioning.
### Latency and the bursty factor 
Time-slicing handles individual bursty tasks slightly faster, with a mean TTS latency of 144.7 ms compared to MIG’s 168.2ms. However, this 23.5 ms difference represents a small fraction of the total end-to-end pipeline response time at the present scale. Under heavy load, the LLM accounts for the vast majority of the total interaction time. Because the end-user cannot perceive a 20ms delta within a multi-second response, the throughput stability of MIG is a more valuable production metric.
## Recommendations for partitioning 
Based on the benchmark data, we recommend the following decision matrix:
  1. Default to MIG for production scale and stability 
     * Experiment 3 showed that MIG handles higher request volumes (2 req/s) with only a minor latency trade-off.
     * Strict hardware-level fault isolation prevents a memory overflow in one process from crashing the other.
     * Best for production environments where throughput and 100% reliability are the priorities.
  2. Use time-slicing for development or low-concurrency apps 
     * This involves a 32% reduction in total throughput and shared-resource dependencies.
     * Best for development, CI/CD, and PoCs to run a full pipeline on a minimal hardware footprint.


## Get started 
  1. Experiment further: Try the [repository](https://github.com/SDcodehub/voice-voice-workflow).
  2. Implement partitioning: Follow our [Implementation Guide](https://github.com/NVIDIA-AI-Blueprints/rag/blob/develop/docs/mig-deployment.md) to configure MIG profiles and use the provided YAML manifests to eliminate resource fragmentation in your cluster.
  3. Scale with NIM: Deploy [NVIDIA NIM](https://docs.nvidia.com/nim/large-language-models/latest/introduction.html) pipelines to fully utilize your ASR, TTS, and LLM workloads for maximum ROI.


Like 
## Tags
[Agentic AI / Generative AI](https://developer.nvidia.com/blog/category/generative-ai/) | [Data Center / Cloud](https://developer.nvidia.com/blog/category/data-center-cloud/) | [MLOps](https://developer.nvidia.com/blog/category/mlops/) | [General](https://developer.nvidia.com/blog/recent-posts/?industry=General) | [CUDA](https://developer.nvidia.com/blog/recent-posts/?products=CUDA) | [Advanced Technical](https://developer.nvidia.com/blog/recent-posts/?learning_levels=Advanced+Technical) | [Benchmark](https://developer.nvidia.com/blog/recent-posts/?content_types=Benchmark) | [featured](https://developer.nvidia.com/blog/tag/featured/) | [LLM Techniques](https://developer.nvidia.com/blog/tag/llm-techniques/)
##  About the Authors 
**About Sagar Desai** Sagar Desai is a generative AI solutions architect at NVIDIA, specializing in the design and deployment of large-scale machine learning systems. With deep expertise in optimizing model performance for both training and inference workloads, Sagar brings a strong foundation in MLOps and LLMOps. Sagar architects resilient infrastructure using distributed computing and orchestration principles to deliver high-impact, enterprise-grade AI solutions. 
[ View all posts by Sagar Desai ](https://developer.nvidia.com/blog/author/sagdesai/)
**About Adi Margolin** Adi Margolin is the product manager for Riva SDK and Speech NIM. With 16 years of product management experience, Adi has built high-impact speech technology solutions across enterprise software companies, developing expertise in bringing ASR and TTS innovations to market. Based in San Jose, Adi brings a unique perspective to speech AI development, having successfully navigated the transition from legacy systems to modern AI-driven platforms while addressing complex requirements of real-time media applications. 
[ View all posts by Adi Margolin ](https://developer.nvidia.com/blog/author/adimargolin/)
## Comments
###  Start the discussion at [forums.developer.nvidia.com](https://forums.developer.nvidia.com/t/maximize-ai-infrastructure-throughput-by-consolidating-underutilized-gpu-workloads/364753)
Close Previous Next
NVIDIA uses cookies to improve your experience on our web site. We and our third-party partners also use cookies and other tools to collect and record information you provide as well as information about your interactions with our websites for performance improvement, analytics, and to assist in marketing efforts. By clicking "Accept All", you consent to our use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/). You can manage your cookie settings by clicking on "Manage Settings." By continuing to use this site or by clicking one of the buttons below, you agree to our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control (GPC) signal and recorded your rejection of all optional cookies on this site for this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. To opt out of non-cookie personal information "sales" / "sharing" for targeted advertising purposes, please visit the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies. You can manage these settings in the NVIDIA [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies which overrides at least one of your previous settings. You can manage them in the [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
Manage Settings
Reject Optional Accept All
Cookie Settings
We and our third-party partners (including social media, advertising, and analytics partners) use cookies and other tracking technologies to collect, store, monitor, and process certain information about you when you visit our website. The information collected might relate to you, your preferences, or your device. We use that information to make the site work, analyze performance and traffic on our website, provide a more personalized web experience, and assist in our marketing efforts.Under certain privacy laws, you have the right to direct us not to "sell" or "share" your personal information for targeted advertising. To opt-out of the "sale" and "sharing" of personal information through cookies, you must opt-out of optional cookies using the toggles below. To opt out of the "sale" and "sharing" of data collected by other means (e.g., online forms) you must also update your data sharing preferences through the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/).Click on the different category headings below to find out more and change the settings according to your preference. You cannot opt out of Required Cookies as they are deployed to ensure the proper functioning of our website (such as prompting the cookie banner and remembering your settings, etc.). By clicking "Save and Accept" or "Decline All" at the bottom, you consent to the use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) in accordance with your settings and accept our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). For more information about our privacy practices, please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/).
Required Cookies
Always Active
These cookies enable core functionality such as security, network management, and accessibility. These cookies are required for the site to function and cannot be turned off.
Cookies Details
Performance Cookies
Performance Cookies
These cookies are used to provide quantitative measures of our website visitors, such as the number of times you visit, time on page, your mouse movements, scrolling, clicks and keystroke activity on the websites; other browsing, search, or product research behavior; and what brought you to our site. These cookies may store a unique ID so that our system will remember you when you return. Information collected with these cookies is used to measure and find ways to improve website performance.
Cookies Details
Personalization Cookies
Personalization Cookies
These cookies collect data about how you have interacted with our website to help us improve your web experience, such as which pages you have visited. These cookies may store a unique ID so that our system will remember you when you return. They may be set by us or by third party providers whose services we have added to our pages. These cookies enable us to provide enhanced website functionality and personalization as well as make the marketing messages we send to you more relevant to your interests. If you do not allow these cookies, then some or all of these services may not function properly.
Cookies Details
Advertising Cookies
Advertising Cookies
These cookies record your visit to our websites, the pages you have visited and the links you have followed to influence the advertisements that you see on other websites. These cookies and the information they collect may be managed by other companies, including our advertising partners, and may be used to build a profile of your interests and show you relevant advertising on other sites. We and our advertising partners will use this information to make our websites and the advertising displayed on it, more relevant to your interests.
Cookies Details
Back Button
Cookie List
Search Icon
Filter Icon
Clear
  * checkbox label label


Apply Cancel
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Decline All Save and Accept
