---
title: 'Ollama vs. vLLM: A deep dive into performance benchmarking | Red Hat Developer'
id: ollama-vs-vllm-a-deep-dive-into-performance-benchmarking-red-hat-developer
tags:
- internal-llm-inference-stacks-7f0c90
- concurrency-collapse
- primary-source
created: '2026-10-09T23:08:47.165497Z'
updated: '2026-10-10T14:52:17.230808Z'
source: https://developers.redhat.com/articles/2025/08/08/ollama-vs-vllm-deep-dive-performance-benchmarking
source_domain: developers.redhat.com
fetched_at: '2026-10-09T23:08:47.144670Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'PRIMARY SOURCE: Red Hat Developer''s own controlled benchmark (Aug 8, 2025;
  last updated July 13, 2026) directly substantiating the concurrency-collapse claims
  repeated across this batch''s secondary sources. Controlled test on a single NVIDIA
  A100-PCIE-40GB GPU on OpenShift 4.17.15, using GuideLLM to drive concurrency from
  1 to 256 simultaneous users against vLLM 0.9.1 (meta-llama/Llama-3.1-8B-instruct)
  and Ollama 0.9.2 (llama3.1:8b-instruct-fp16). Headline result: vLLM peaks at 793
  tokens/sec vs Ollama''s 41 TPS, with P99 latency of 80ms vs 673ms at peak throughput.
  At Ollama''s untuned default (max 4 parallel requests), throughput plateaus immediately
  while vLLM scales with concurrency. Even after tuning Ollama to its highest stable
  parallelism for this GPU (OLLAMA_NUM_PARALLEL=32), Ollama''s throughput still plateaus
  well below vLLM''s, and its inter-token latency becomes ''extremely erratic, with
  massive spikes at higher concurrency'' -- explicitly attributed to head-of-line
  blocking where ''a single stalled request can slow down an entire batch.'' vLLM''s
  TTFT stays low and stable across all concurrency levels; Ollama''s TTFT ''rises
  sharply'' because requests queue before processing. Conclusion: Ollama is explicitly
  scoped for local development/prototyping/single-user use, not production; vLLM is
  ''unequivocally the superior choice for production deployment.'' This is the primary
  engineering source behind the ''Ollama collapses at 10+ concurrent users'' claims
  found in tianpan.co and other secondary sources in this batch -- though note the
  specific multiplier (20x/44x) cited by tianpan.co is not the exact figure in this
  benchmark (this benchmark reports ~19x TPS gap at peak: 793 vs 41), suggesting tianpan.co''s
  figures may be rounded/approximated or drawn from a related but different Red Hat
  benchmark.'
---

*Suggested by [[the-build-vs-buy-llm-infrastructure-decision-most-teams-get-wrong]] — cited as source for Ollama vs vLLM throughput/concurrency comparison figures*

[ Skip to main content ](https://developers.redhat.com/articles/2025/08/08/ollama-vs-vllm-deep-dive-performance-benchmarking#main-content)
All Red Hat 
Back to menu
## Quick links: redhat.com, Customer Portal, Red Hat's developer site, Red Hat's partner site.
  * [ You are here Red Hat Learn about our open source products, services, and company. ](https://www.redhat.com/en)
  * [ You are here Red Hat Customer Portal Get product support and knowledge from the open source experts. ](https://access.redhat.com)
  * [ You are here Red Hat Developer Read developer tutorials and download Red Hat software for cloud application development. ](https://developers.redhat.com)
  * [ You are here Red Hat Partner Connect Get training, subscriptions, certifications, and more for partners to build, sell, and support customer solutions.  ](https://connect.redhat.com)


## Products & tools
  * ### [Red Hat Documentation ](https://docs.redhat.com) Find answers, get guidance, and learn how to use Red Hat products. 
  * ### [Red Hat Ecosystem Catalog ](https://catalog.redhat.com) Find hardware, software, and cloud providers―and download container images―certified to perform with Red Hat technologies. 


## Try, buy, & sell
  * ### [Red Hat Hybrid Cloud Console ](https://www.redhat.com/hybrid-cloud-console) Access technical how-tos, tutorials, and learning paths focused on Red Hat’s hybrid cloud managed services. 
  * ### [Red Hat Store ](https://www.redhat.com/store) Buy select Red Hat products and services online. 


## Events
  * ### [Red Hat Summit ](https://www.redhat.com/summit) Register for and learn about our annual open source IT industry event. 


Ask AI
#  Ollama vs. vLLM: A deep dive into performance benchmarking
August 8, 2025 

Related topics:
     [Artificial intelligence](https://developers.redhat.com/topics/ai-ml)[Open source](https://developers.redhat.com/topics/open-source-communities) 

Related products:

Table of contents: 
  * [The benchmarking setup](https://developers.redhat.com/articles/2025/08/08/ollama-vs-vllm-deep-dive-performance-benchmarking#the_benchmarking_setup)
  * [Comparison 1: Default settings showdown](https://developers.redhat.com/articles/2025/08/08/ollama-vs-vllm-deep-dive-performance-benchmarking#comparison_1__default_settings_showdown)
  * [Comparison 2: Tuned Ollama versus vLLM](https://developers.redhat.com/articles/2025/08/08/ollama-vs-vllm-deep-dive-performance-benchmarking#comparison_2__tuned_ollama_versus_vllm)
  * [The right tool for the job](https://developers.redhat.com/articles/2025/08/08/ollama-vs-vllm-deep-dive-performance-benchmarking#the_right_tool_for_the_job)


Key takeaways
  * Ollama and vLLM serve different purposes, and that's a good thing for the AI community: Ollama is ideal for local development and prototyping, while vLLM is built for high-performance production deployments.
  * vLLM outperforms Ollama at scale: vLLM delivers significantly higher throughput (achieving a peak of **793 TPS** compared to Ollama's **41 TPS**) and lower P99 latency (**80 ms** vs. **673 ms** at peak throughput). vLLM delivers higher throughput and lower latency across all concurrency levels(1-256 concurrent users), even when Ollama is tuned for parallelism.
  * Ollama prioritizes simplicity, vLLM prioritizes scalability: Ollama keeps things lightweight for single users, while vLLM dynamically scales to handle large, concurrent workloads efficiently.


In [Ollama or vLLM? How to choose the right LLM serving tool for your use case](https://developers.redhat.com/articles/2025/07/08/ollama-or-vllm-how-choose-right-llm-serving-tool-your-use-case), we introduced Ollama as a good tool for local development and [vLLM](https://www.redhat.com/en/topics/ai/what-is-vllm) as the go-to solution for high-performance production serving. We argued that while Ollama prioritizes ease of use for individual developers, vLLM is engineered for the scalability and throughput that enterprise applications demand.
Now, it's time to move from theory to practice. In this follow-up article, we'll put these two inference engines to the test in a head-to-head performance benchmark. Using concrete data, we’ll demonstrate how each inference server behaves under pressure and provide clear evidence to help you choose the right tool for your deployment needs.
The benchmarking setup
## The benchmarking setup
To ensure a true "apples-to-apples" comparison, we created a controlled testing environment on OpenShift using default arguments and original model weights without compression. The goal was to measure how each server performed while handling an increasing number of simultaneous users.
  * **Hardware and software:**
    * **GPU:** Single NVIDIA A100-PCIE-40GB GPU
    * **NVIDIA driver version:** 550.144.03
    * **CUDA version:** 12.4
    * **Platform:** OpenShift version 4.17.15
    * **Models:** meta-llama/Llama-3.1-8B-instruct for vLLM and llama3.1:8b-instruct-fp16 for Ollama
    * **vLLM version:** 0.9.1
    * **Ollama version:** 0.9.2
    * **Python version:** 3.13.5
  * **Benchmarking tool:**
We used [GuideLLM](https://github.com/vllm-project/guidellm) (version 0.2.1) to conduct our performance tests. GuideLLM is a benchmarking tool specifically designed to measure the performance of LLM inference servers. Refer to the article [GuideLLM: Evaluate LLM deployments for real-world inference](https://developers.redhat.com/articles/2025/06/20/guidellm-evaluate-llm-deployments-real-world-inference) or [this video](https://www.youtube.com/watch?v=crwoAti-mOI) to learn more about GuideLLM. We ran it as a container from within the same OpenShift cluster to ensure tests were conducted on the same network as our vLLM and Ollama services. We use its concurrency feature to simulate multiple simultaneous users by sending requests concurrently at various rates.
  * **Methodology:**
    * We used a **fixed dataset** of prompt-response pairs to ensure that every request was identical for both servers, eliminating variables from synthetic data generation. Each entry in this dataset specified the exact prompt to be sent to the model and the pre-calculated prompt and expected output token counts.
    * We simulated multiple simultaneous users by running **concurrent requests** , with concurrency levels tested from 1 up to 256. The concurrency level represents a fixed number of "virtual users" that continuously send requests. For example, the "64 concurrency" rate maintains a constant 64 active requests on the server. Each test runs for 300 seconds.
  * **Key performance metrics:**
    * **Requests Per Second (RPS):** The average number of requests the system can successfully complete each second. Higher is better.
    * **Output Tokens Per Second (TPS):** The total number of tokens generated per second, measuring the server's total generative capacity. Higher is better.
    * **Time to First Token (TTFT):** How long it takes from sending a request to receiving the first piece of the response (token). This measures initial responsiveness. Lower is better.
    * **Inter-token Latency (ITL):** The average time between each subsequent token in a response, measuring the text generation speed. Lower is better.


For TTFT and ITL, we used P99 (99th percentile) as the measure. P99 means that 99% of requests had a TTFT/ITL at or below this value, making it a good measure of "worst-case" responsiveness.
Comparison 1: Default settings showdown
## Comparison 1: Default settings showdown
First, we compared vLLM and Ollama using their standard, out-of-the-box configurations. By default, Ollama is configured to handle a maximum of four requests in parallel, as it's primarily designed for single-user scenarios.
### Throughput (RPS and TPS)
The difference in throughput was immediate and stark.
  * **vLLM** 's throughput (both RPS and TPS) scaled impressively as concurrency increased, handling a much heavier user load.
  * **Ollama** 's performance remained flat, quickly hitting its maximum capacity due to the default cap on parallel requests.


As seen in the graphs (Figures 1 and 2), vLLM's peak performance is several times higher than Ollama's default configuration, demonstrating its superior ability to manage many concurrent user requests.
Figure 1: vLLM’s throughput(TPS) scales with concurrency, while Ollama's throughput remains flat. Figure 2: vLLM’s throughput(RPS) scales with concurrency, while Ollama's throughput remains flat.
_vLLM's throughput scales with user load, while Ollama's performance remains flat._
### **Responsiveness (TTFT and ITL)**
Here, we see an interesting trade-off between how the two engines handle load.
  * **Time to First Token:** vLLM consistently delivered a much lower TTFT, meaning users get a faster initial response, even under heavy load. Ollama's TTFT rose dramatically with more users because incoming requests had to wait in a queue before being processed (Figure 3).
_"Worst-case" (P99) Time To First Token shows vLLM is significantly more responsive under load._

Figure 3: vLLM has consistently lower TTFT, while Ollama's TTFT rises sharply.
  * **Inter-token Latency:** At very high concurrency (above 16), vLLM's ITL began to rise, while Ollama's remained stable and low. This is because Ollama throttles requests, keeping its active workload small and predictable at the expense of making many users wait (high TTFT). In contrast, vLLM processes a much larger batch of requests at once to maximize overall throughput, which can slightly increase the time to generate each individual token within that large batch (Figure 4).

Figure 4: At higher concurrencies, vLLM's ITL begins to rise, while Ollama's remains stable and low.
_Even when tuned for parallelism, Ollama's throughput can't keep up with vLLM._
Comparison 2: Tuned Ollama versus vLLM
## Comparison 2: Tuned Ollama versus vLLM
Recognizing that Ollama's default settings aren't meant for high-concurrency, we tuned it for maximum performance. **We set its parallel request limit to 32**(`OLLAMA_NUM_PARALLEL=32`), which was the highest stable value for our NVIDIA A100 GPU. vLLM isn't limited by a fixed number of parallel requests like `OLLAMA_NUM_PARALLEL`. Instead, its scaling is dynamic.
We show only results up to load test concurrency of 64, as anything above that is oversaturated.
### Throughput (RPS and TPS)
Even after tuning, vLLM remained the clear leader, as shown in Figures 5 and 6.
  * **vLLM** 's throughput continued to scale almost linearly, showcasing its dynamic and efficient scheduling.
  * **Ollama** , despite the tuning, saw its performance plateau and was unable to match vLLM's capacity at any concurrency level.

Figure 5: vLLM's throughput(TPS) scales with concurrency, while Ollama's throughput plateaus at a much lower level. Figure 6: vLLM's throughput(RPS) scales with concurrency, while Ollama's throughput plateaus at a much lower level.
### Responsiveness (TTFT and ITL)
Tuning Ollama for higher parallelism revealed significant stability challenges under load.
  * **Time to First Token:** vLLM's TTFT remained extremely low and stable. In contrast, Ollama's TTFT still increased sharply, as juggling more requests meant each new one had to wait longer to begin processing (Figure 7).
  * **Inter-token Latency:** This is where the difference becomes most apparent. vLLM's token generation speed stayed fast and fluid across all loads. Ollama's ITL, however, became extremely erratic, with massive spikes at higher concurrency. This indicates significant performance degradation and potential "head-of-line blocking," where a single stalled request can slow down an entire batch (Figure 8).

Figure 7: vLLM consistently maintains a lower TTFT across all concurrency levels, while Ollama's TTFT increases significantly with higher concurrency. Figure 8: vLLM's ITL remains stable at all concurrencies, while Ollama's ITL rises sharply and becomes erratic at higher concurrency.
_vLLM maintains stable generation speed, while tuned Ollama becomes erratic under load._
The right tool for the job
## The right tool for the job
This benchmark comparison data provides definitive proof for our initial guidance:
  * **Ollama** excels in its intended role: a simple, accessible tool for local development, prototyping, and single-user applications. Its strength lies in its ease of use, not its ability to handle high-concurrency production traffic, where it struggles even when tuned.
  * **vLLM** is unequivocally the superior choice for production deployment. It is built for performance, delivering significantly higher throughput and lower latency under heavy load. Its dynamic batching and efficient resource management make it the ideal engine for scalable, enterprise-grade AI applications.


Ultimately, the choice depends on where you are in your development journey. For developers experimenting locally, Ollama is a fantastic starting point. But for teams moving toward production, this performance data confirms that **vLLM** is the powerful, scalable, and efficient foundation needed to serve LLMs reliably at scale.
Discover how [Red Hat AI Inference Server](https://www.redhat.com/en/products/ai/inference-server), powered by [vLLM](https://www.redhat.com/en/topics/ai/what-is-vllm), enables fast, cost-effective AI inference.
_Last updated: July 13, 2026_
### Platforms
  * [Red Hat Enterprise Linux](https://developers.redhat.com/products/rhel/overview)
  * [Red Hat Ansible Automation Platform](https://developers.redhat.com/products/ansible/overview)
  * [See all products](https://developers.redhat.com/products)


### Build
  * [Developer tools](https://developers.redhat.com/topics/developer-tools)
  * [Interactive tutorials](https://developers.redhat.com/learn#assembly-id-70181)


### Quicklinks
  * [Learning resources](https://developers.redhat.com/learn)


### Communicate
  * [Report a website issue](https://developers.redhat.com/report_a_website_issue)
  * [Site status dashboard](https://status.redhat.com/)
  * [Report a security problem](https://access.redhat.com/security/team/contact/)


### RED HAT DEVELOPER
Build here. Go anywhere.
We serve the builders. The problem solvers who create careers with code.
Join us if you’re a developer, software engineer, web designer, front-end designer, UX designer, computer scientist, architect, tester, product manager, project manager or team lead.
### Red Hat legal and privacy links
  * [Cool Stuff Store](https://coolstuff.redhat.com/)

© 2026 Red Hat
### Red Hat legal and privacy links
  * [All policies and guidelines](https://redhat.com/en/about/all-policies-guidelines)
  * [Digital accessibility](https://redhat.com/en/about/digital-accessibility)
  * Cookie Preferences and Opt-Out Rights


