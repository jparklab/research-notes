---
title: What we learned from our Inference Experiment
id: what-we-learned-from-our-inference-experiment
tags:
- internal-llm-inference-stacks
- hetzner
- vllm
- llm-d
- open-webui
- kv-cache-offload
- first-person-account
created: '2026-10-09T22:53:40.467162Z'
updated: '2026-10-10T14:52:16.998060Z'
source: https://www.hetzner.com/blog/inference-experiment/
source_domain: www.hetzner.com
fetched_at: '2026-10-09T22:53:40.442881Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Hetzner engineering blog (Aug 17 2026), a DevOps engineer''s first-person
  retrospective on Hetzner''s public-facing ''Inference Experiment'' (free OpenAI-spec
  LLM API on Hetzner-owned EU infrastructure), with background on Hetzner''s internal
  inference platform history. States Hetzner built an internal inference platform
  using ''years of Kubernetes experience'' to test server configs and LLM-serving
  software, eventually settling on a stack of GEX131 servers + vLLM + Open WebUI for
  internal production-grade applications and model benchmarking -- this predates and
  underlies the public Experiment. For the public Inference Experiment, models served:
  Qwen/Qwen3.6-35B-A3B, Kimi-K2.7-Code, DeepSeek-V4-Flash-0731 (Kimi K3 was excluded
  due to license terms incompatible with free offering). In the week before launch,
  ''routing for inference requests was greatly improved by introducing llm-d and making
  changes to some routing components we had developed in-house'' -- llm-d explicitly
  named as adopted OSS, paired with custom in-house routing changes (unspecified mechanism).
  KV-cache offloading into host RAM is cited as a promised performance improvement,
  engine unspecified for this feature (likely vLLM, not stated explicitly). Operational
  findings: demand exceeded expectations within 6 hours; TTFT p99 spiked to ~10 minutes
  at peak (effectively unusable); they reduced max parallel requests per instance
  after observing that lower concurrency per instance sometimes raised net throughput;
  no prompt data is logged, only token metrics. Separately, OpenClaw (a third-party
  agent/CLI product Hetzner offers as a preconfigured instance) is described as having
  persistent code-quality problems with frequent upstream bugs -- flagged as a software-quality
  complaint about a dependency, not Hetzner''s own inference stack.'
utility_score: 17.0
---

Ask AI
Engineering
# Inference Experiment: What we learned after one week
August 17, 202610 min read
TL;DR
The experiment proved to be a great success. The design of our inference API and the hardware behind it were generally sound, and provided very good results. The demand for inference proved much greater than we expected. To meet the high demand for large models, we will need further improvements. While we make those improvements, the experiment will continue on a smaller scale, with smaller models.
#### We recently launched our Inference Experiment: an inference API that allows you to use Large Language Models (LLMs) which we host on our infrastructure. You can connect any agent, harness, or coding tool of your choice to it, and use our LLM free of charge. This way, you can try out LLMs running on powerful hardware, while we gain experience running such models.
Now that the Inference Experiment is one week old, we would like to give you some insights into how it works, some of the things we learned, and what the future for it might look like. To do this, we’d like to show you a timeline of the Hetzner Experiment platform and all experiments on it so far.
## T - 4 months: introducing Hetzner Experiments with OpenClaw
Ever since we’ve been offering GPU servers to our customers, we have tested ways of serving LLMs internally. With many years of Kubernetes experience, we were able to build an internal inference platform, which we used to test different server configurations and LLM-serving software. 
We eventually settled on a stack consisting of a few GEX131 servers, vLLM and Open WebUI. This combination allowed us to run a few smaller LLMs, which we could use for some internal production-grade applications. It also allowed us to trial and benchmark larger models internally.
At the start of 2026, [OpenClaw](https://openclaw.ai/) became a much-hyped topic in the AI space. Many enthusiasts were using Hetzner Cloud VMs to try out OpenClaw, but struggled with safely configuring the server for it. To help our customers with this challenge, we decided to offer OpenClaw as a preconfigured instance.
We quickly built a new platform, which we could use to offer not only OpenClaw, but also other services and products we might want to experiment with in the future. It would allow rapid iteration and experimentation, while coming with fewer guarantees than our more mature products. One core feature would be the pricing: we would initially offer all Experiments completely for free.
OpenClaw was also an opportunity to gain experience with self-hosted inference. Our OpenClaw came preconfigured with our own inference API. The API was not publicly available but was limited to OpenClaw, so we could control the request volume to a degree. By offering inference only via OpenClaw, we could try out some smaller open-weight models, in various configurations. And our customers would get an AI product that was completely EU-based.
To show how unique the Hetzner Experiments platform is, we built a separate interface panel for it at [experiments.hetzner.com](https://experiments.hetzner.com/).
This limited OpenClaw Experiment gave us lots of insights:
  * Small models like Qwen 3.6 proved sufficient for most OpenClaw-based use cases.


  * GEX131 with its single RTX PRO™ 6000 Blackwell GPU was sufficient for small LLMs, but couldn’t run large models in a meaningful way.


  * Lots of users praised the fact that this was entirely EU-based.


  * OpenClaw’s code quality was a constant problem: Each new OpenClaw release introduced new challenges, as the volume of upstream bugs was quite high.


## T - 1 month: getting specialized hardware
At Hetzner, our hardware teams are constantly working on new server models. One such model was a new custom server, which houses eight RTX PRO™ 6000 Blackwell GPUs in a single server, instead of the single GPU of a GEX131. This would be much better suited for running larger models.
With some of these 8-GPU servers, we could offer larger models to our customers. The OpenClaw Experiment had already shown that our customers are interested in AI topics and inference. However, the software quality of OpenClaw remained a challenge.
To solve this, we decided to offer inference directly as an OpenAI-spec API that everyone could use, without having to go through OpenClaw. All they would need is an auth token, which they could generate via [experiments.hetzner.com](https://experiments.hetzner.com/), in the separate “Inference” tab.
## T - 2 weeks: benchmarking and picking the right models
What followed was a lot of benchmarking and market research.
We came up with a list of LLMs we wanted to offer:
• [Qwen/Qwen3.6-35B-A3B](https://huggingface.co/Qwen)
• [Kimi-K2.7-Code](https://huggingface.co/moonshotai)
• [DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai)
Qwen would continue from the OpenClaw Experiment as a smaller, “baseline” model. Kimi would offer a coding-focused alternative to the two largest models, GLM and DeepSeek. Each of those models represented the newest version of the respective LLM family.
The only exception to this was Kimi: K3 had already come out, and promised great results in public benchmarks. However, K3’s very specific license requirements did not allow us to offer it as a free experiment.
## T - 1 week: final preparations
As final preparations for the Inference Experiment, some internal components were overhauled to deal with the expected high demand.
Routing for inference requests was greatly improved by introducing llm-d and making changes to some routing components we had developed in-house. Features such as kv-cache offloading into the host’s RAM also promised substantial performance improvements.
We also added some more of the custom 8-GPU servers, and dedicated each one to running either DeepSeek, GLM or Kimi. And we dedicate the GEX131 single-GPU servers to serving the smaller Qwen 3.6 model.
## T - 0 hours: launch
Demand for the Inference Experiment was hard to judge up front. To be on the safe side, we staggered the launch over multiple days:
On August 7th, we enabled the new models and the Inference Experiment on the Hetzner Experiments platform. This gave early users the weekend to discover any issues we might have missed. After the weekend, on August 10th, we started actively communicating the experiment to our customers. We expected that word of mouth would eventually increase demand as well.
## T + 6 hours: hitting capacity limits
Word was spreading fast: within a few hours, we noticed a sharp increase in requests to the inference API. The demand exceeded our most optimistic expectations, and continued to grow at a high rate.
Qwen 3.6, which was the default model for OpenClaw, proved the most popular on the first day, both in requests and generated tokens. The other models also saw high demand, but trailed Qwen by quite a bit.
Our setup was able to generate enough token throughput to keep up with the requests, but we already noticed an increase in latency:
The 99th percentile for the time to first token (TTFT) had seen short spikes for some models, but it generally still performed reasonably well, sitting at around 5 to 10 seconds for the majority of the time.
Another surprise was the huge increase in OpenClaw instances. While a bit more demand was expected, the number of instances skyrocketed on the first day. Our small Kubernetes cluster for running OpenClaw instances was rapidly reaching its capacity limit. So we disabled the OpenClaw tab on [experiments.hetzner.com](https://experiments.hetzner.com/) for every account that hadn’t signed in to Hetzner Experiments Platform yet.
## T + 1 day: coping with the demand
On the second day of the experiment, the number of requests for all models increased drastically, yet again.
Usage patterns started to become clear:
  * DeepSeek saw the most interest. Many users reported favorable benchmark results.


  * Qwen remained popular, with very many small requests.


  * GLM and Kimi were also in high demand, but much less so than the other two models. GLM saw the highest latency of all, and needed more fine tuning.


  * We do not log any prompt data, but our token metrics implied that many users were still running benchmarks at this time.


This number of requests was exceeding the capacity of our experiment infrastructure, however. Time to first token (TTFT) went through the roof, peaking at almost 10 minutes for the 99th percentile, rendering the API effectively unusable at times. Our experiment had not been designed for this scale of requests, and it showed.
The high number of small, concurrent requests was especially challenging. To better deal with this, we reduced the number of parallel requests we would accept.
We also received lots of feedback surrounding the reasoning settings of the various models, which some users found unintuitive.
## T + 2 days: more adjustments, more hardware
Demand for inference increased even more, pushing our inference infrastructure to the limit. While the number of requests to the API endpoint was dropping, the overall number of input tokens kept rising.
In an effort to provide more raw power, we were able to build a few more special 8-GPU servers, and added them to the pool serving GLM as that model was struggling the most at the time.
Usage patterns now settled into what we’d see for the rest of the week:
  * DeepSeek saw more and more complex requests, with huge amounts of input and output tokens.


  * Qwen continued to see a very high number of smaller requests.


  * GLM and Kimi saw medium-sized requests, but on a lower level than the previous two.


  * Requests were larger on average than on the first day. This implied more use for practical purposes, with large real-world datasets.


  * All models saw higher throughput thanks to our tweaks.


  * During peak times, a high number of requests had to spend minutes waiting in queue before a GPU server could even start dealing with the inference request.


We tried many improvements over the course of the week: General inference rate limits were adjusted down to give more users a chance to try the experiment. Some usage patterns seemed malicious, so we took steps to rate limit and filter those. Actual usage was also a different pattern than our benchmarks. We learned that, for some models, a lower number of concurrent requests per instance was the most optimal with regard to throughput. With this change, the overall token throughput increased for the experiment.
We also expanded the hardware pool for OpenClaw. Early users of OpenClaw kept creating even more OpenClaw instances. The number of software issues with OpenClaw also increased, however: Multiple OpenClaw instances had actually misconfigured themselves when prompted by the users, and ended up in a broken state.
## T + 7 days: taking stock and scaling down the experiment
Looking back after one week, we have learned a lot:
Demand is much higher than we expected, both in terms of users as well as requests and tokens per request. The 8-GPU servers have proven that they can run the large models, but the demand was simply too high for the few servers in our experiment. Metrics also showed us some bottlenecks in the software used for routing and serving requests.
Addressing some of those bottlenecks will require taking hardware out of the pool for the inference API in order to test some changes. We will also look into improving our documentation by adding more details and tutorials to make it easier to use for beginners.
We will also review our selection of LLMs based on the metrics we saw. We might also look at other use cases like re-ranking or embedding, as used by RAG pipelines. In addition, we will critically review the OpenClaw Experiment, and the support effort required for it.
The Inference Experiment will continue, but in a reduced scope: We’ll continue serving small models such as Qwen 3.6 and 3.8, but will take a break from serving large models.
Try it out for yourself at [experiments.hetzner.com](https://experiments.hetzner.com/). We’ll continue to launch new projects and experiment to see what else might be possible. Stay tuned to see what comes next.
Engineering
### Stefan Neubauer
DevOps Engineer, Managed Server
Share article
##### [The Hetzner Cloud network stack - history and technical overview October 8, 202610 min read ](https://www.hetzner.com/blog/the-hetzner-cloud-network-stack-history-and-technical-overview/)
##### [Node-RED: A Low-Code, Event-Driven Automation Tool July 30, 20266 min read ](https://www.hetzner.com/blog/node-red/)
##### [How we reduced API requests in the Hetzner Cloud Controller Manager July 28, 20268 min read ](https://www.hetzner.com/blog/How-we-reduced-API-requests-in-the-Hetzner-Cloud-Controller-Manager/)
