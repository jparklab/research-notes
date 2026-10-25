---
title: Production-Grade LLM Inference at Scale with KServe, llm-d, and vLLM | llm-d
id: production-grade-llm-inference-at-scale-with-kserve-llm-d-and-vllm-llm-d
tags:
- llmd-kv-cache-routing-c665d5
- production-case-study
- kserve
- tesla
created: '2026-09-14T11:59:11.125927Z'
updated: '2026-09-15T04:55:24.115707Z'
source: https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm
source_domain: llm-d.ai
fetched_at: '2026-09-14T11:59:11.105774Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'llm-d blog post documenting Tesla''s production adoption of KServe + llm-d
  + vLLM at scale (with Red Hat collaboration: Sergey Bekkerman, Greg Pereira, Robert
  Shaw et al. from Red Hat; Scott Cabrinha, Sai Krishna from Tesla). Tesla''s initial
  naive vLLM-in-StatefulSet deployment hit three operational walls: NFS storage drag
  for hundreds-of-GB safetensors, node-to-pod affinity lock-in from local LVM PVs
  requiring manual PVC deletion on hardware failure, and naive round-robin load balancing
  that ''fails to utilize the critical KV-cache on the GPU.'' Switching to KServe''s
  LLMInferenceService/LLMInferenceConfig CRDs plus Envoy + Envoy AI Gateway + Gateway
  API Inference Extension enabled prefix-cache-aware routing. Reports a concrete production
  result: ''a 3x improvement in output tokens/s and a 2x reduction in time to first
  token (TTFT) after enabling prefix-cache aware routing,'' measured serving Llama
  3.1 70B on 4x AMD MI300X GPUs with tensor-parallel-size=4, gpu-memory-utilization=0.90,
  --max-model-len=65536. Also lists upstream KServe contributions from this production
  hardening (kserve#4901, #4900, #4898, #4899 feature requests; #4970 making storageInitializer
  optional for RunAI Model Streamer; #4886 adding support for the latest Gateway API
  Inference Extension).'
---

*Suggested by [[intelligent-inference-scheduling-with-llm-d-on-red-hat-ai-red-hat-developer]] — Cited as architecture context for how KServe and llm-d integrate*

[Skip to main content](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
## The Problem with "Simple" LLM Deployments[​](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#the-problem-with-simple-llm-deployments "Direct link to The Problem with "Simple" LLM Deployments")
Everyone is racing to run Large Language Models (LLMs), in the cloud, on-prem, and even on edge devices. The real challenge, however, isn't the first deployment; it's scaling, managing, and maintaining hundreds of LLMs efficiently. We initially approached this challenge with a straightforward vLLM deployment wrapped in a Kubernetes StatefulSet.
The approach quickly introduced severe operational bottlenecks:
  * **Storage Drag:** Models like Llama 3 can easily reach hundreds of gigabytes in size. Relying on sluggish network storage (NFS) for these massive safetensors was a non-starter.
  * **Infrastructure Lock-in:** Switching to local LVM persistent volumes solved the speed problem but created a rigid node-to-pod affinity. A single hardware failure meant a manual intervention to delete the Persistent Volume Claim (PVC) and reschedule the pod, which is an unacceptable burden for day-2 operations.
  * **Naive Load Balancing:** Beyond the looming retirement of NGINX Ingress Controller, a simple round-robin load-balancing strategy is fundamentally inefficient for LLMs. It fails to utilize the critical **KV-cache** on the GPU, a core feature of vLLM that significantly boosts throughput. In a world where GPU costs are paramount, squeezing efficiency out of every core is non-negotiable.


## What We Needed from an Operator[​](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#what-we-needed-from-an-operator "Direct link to What We Needed from an Operator")
Running LLMs at scale demanded a purpose-built Kubernetes Operator designed for the intricacies of AI/ML. After evaluating the landscape, we identified a clear set of requirements:
  * **Full spec-level customization:** We needed the ability to override the runtime specification beyond what typical Custom Resources expose — tailoring vLLM flags for specialized hardware and rapid iteration.
  * **Flexible deployment patterns:** Rather than being locked into a single prefill/decode architecture, we needed an operator that could adapt to our evolving serving topologies.
  * **Standard Kubernetes API integration:** The solution had to work with the Kubernetes API surface we already knew, not introduce an entirely new abstraction layer.


## The Winning Combination: KServe + llm-d + vLLM[​](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#the-winning-combination-kserve--llm-d--vllm "Direct link to The Winning Combination: KServe + llm-d + vLLM")
Our journey led us back to the most flexible and powerful solution: [**llm-d**](https://github.com/llm-d/llm-d), powered by [**KServe**](https://github.com/kserve/kserve) and its cutting-edge **Inference Gateway Extension**.
This combination solved every scaling and operational challenge we faced by delivering:
  1. **Deep Customization:** The **LLMInferenceService** and **LLMInferenceConfig** objects expose the standard Kubernetes API, allowing us to override the spec precisely where needed. This level of granular control is crucial for tailoring vLLM to specialized hardware or quickly implementing flag changes.
  2. **Intelligent Routing and Efficiency:** By leveraging [**Envoy**](https://www.envoyproxy.io/), [**Envoy AI Gateway**](https://aigateway.envoyproxy.io/), and [**Gateway API Inference Extension**](https://github.com/kubernetes-sigs/gateway-api-inference-extension), we moved far beyond round-robin. This technology enables **prefix-cache aware routing** , ensuring requests are intelligently routed to the correct vLLM instance to maximize KV-cache utilization and drive up GPU efficiency.


On one deployment, we observed a **3x improvement in output tokens/s** and a **2x reduction in time to first token (TTFT)** after enabling prefix-cache aware routing. These numbers were measured when serving Llama 3.1 70B model on 4 MI300X AMD GPUs with the configuration: `tensor-parallel-size=4`, `gpu-memory-utilization=0.90`, and `--max-model-len=65536`. Below is the chart that shows the performance improvement after we released the routing change (at around 12:30PM).
## Community Contributions and Collaboration[​](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#community-contributions-and-collaboration "Direct link to Community Contributions and Collaboration")
Running this stack in production surfaced real issues that we fixed upstream in KServe, benefiting the broader community:
  * **New feature requests filed:** [kserve#4901](https://github.com/kserve/kserve/issues/4901), [kserve#4900](https://github.com/kserve/kserve/issues/4900), [kserve#4898](https://github.com/kserve/kserve/issues/4898), [kserve#4899](https://github.com/kserve/kserve/issues/4899)
  * **storageInitializer made optional** ([kserve#4970](https://github.com/kserve/kserve/pull/4970)) — enabling RunAI Model Streamer as an alternative to the default storage initializer
  * **Added support for latest Gateway API Inference Extension** ([kserve#4886](https://github.com/kserve/kserve/pull/4886))


These contributions came directly from hitting production edge cases. Validating KServe and llm-d at this scale helped harden the platform for everyone running LLM workloads on Kubernetes.
## Acknowledgement[​](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#acknowledgement "Direct link to Acknowledgement")
We’d like to thank everyone from the community who has contributed to the successful adoption of KServe, llm-d, and vLLM in Tesla's production environment. In particular, below is the list of people from Red Hat and Tesla teams who have helped through the process (in alphabetical order).
  * **Red Hat team** : Sergey Bekkerman, Nati Fridman, Killian Golds, Andres Llausas, Bartosz Majsak, Greg Pereira, Pierangelo Di Pilato, Ran Pollak, Vivek Karunai Kiri Ragavan, Robert Shaw, and Yuan Tang
  * **Tesla team** : Scott Cabrinha and Sai Krishna


## Get Involved with llm-d[​](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#get-involved-with-llm-d "Direct link to Get Involved with llm-d")
The work described here is just one example of what becomes possible when a community of engineers tackles hard problems together in the open. If you're running LLMs at scale and wrestling with the same challenges — storage, routing, efficiency, day-2 operations — we'd love to have you involved.
  * **Explore the code** → Browse our [GitHub organization](https://github.com/llm-d) and dig into the projects powering this stack
  * **Join our Slack** → [Get your invite](https://llm-d.ai/slack) and connect directly with maintainers and contributors from Red Hat, Tesla, and beyond
  * **Attend community calls** → All meetings are open! Add our [public calendar](https://red.ht/llm-d-public-calendar) (Wednesdays 12:30pm ET) and join the conversation
  * **Follow project updates** → Stay current on [Twitter/X](https://twitter.com/_llm_d_), [Bluesky](https://bsky.app/profile/llm-d.ai), and [LinkedIn](https://www.linkedin.com/company/llm-d)
  * **Watch demos and recordings** → Subscribe to the [llm-d YouTube channel](https://www.youtube.com/@llm-d-project) for community call recordings and feature walkthroughs
  * **Read the docs** → Visit our [community page](https://llm-d.ai/community) to find SIGs, contribution guides, and upcoming events


  * [The Problem with "Simple" LLM Deployments](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#the-problem-with-simple-llm-deployments)
  * [What We Needed from an Operator](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#what-we-needed-from-an-operator)
  * [The Winning Combination: KServe + llm-d + vLLM](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#the-winning-combination-kserve--llm-d--vllm)
  * [Community Contributions and Collaboration](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#community-contributions-and-collaboration)
  * [Get Involved with llm-d](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm#get-involved-with-llm-d)


