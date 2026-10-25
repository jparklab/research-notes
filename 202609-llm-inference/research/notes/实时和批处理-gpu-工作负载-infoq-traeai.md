---
title: 实时和批处理 GPU 工作负载 | InfoQ | traeai
id: 实时和批处理-gpu-工作负载-infoq-traeai
tags:
- onprem-llm-inference-stacks-859099
- derivative-of
created: '2026-10-23T18:12:32.996827Z'
updated: '2026-10-23T18:17:20.934529Z'
source: https://www.traeai.com/articles/2c02fbec-8e7e-412c-902b-e46354b0ea37
source_domain: www.traeai.com
fetched_at: '2026-10-23T18:12:32.977538Z'
fetch_provider: crawl4ai
status: deprecated
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: Chinese-language AI-summary aggregator copy of the InfoQ transcript; contains
  full English transcript text and the original InfoQ URL (infoq.com/presentations/realtime-gpu-workloads/).
  Derivative; cite the InfoQ note instead.
---

InfoQ·2026年5月26日
# 实时和批处理 GPU 工作负载
7.5内容质量
保存为笔记
## TL;DR · AI 摘要
Joseph Stein 分享了如何在私有云数据中心构建一个企业级 AI 服务平台，通过多命名空间调度、Valkey 和 Lua 原子优先队列管理、OWASP 风险缓解和自定义 S3 到 Kafka 代理来实现实时和批处理 GPU 工作负载。
### 核心要点
  * 通过多命名空间调度最大化未充分利用的 GPU 资源。
  * 使用 Valkey 和 Lua 进行原子优先队列管理和背压管理。
  * 利用中央代理网关缓解 OWASP Top 10 LLM 风险。


## 结构提纲
按章节快速跳转。
  1. [Joseph Stein](https://www.traeai.com/entities/joseph-stein) 介绍了自己的背景和演讲的主题。
  2. 通过多命名空间调度最大化未充分利用的 GPU 资源。
  3. 使用 Valkey 和 Lua 进行原子优先队列管理和背压管理。
  4. 利用中央代理网关缓解 OWASP Top 10 LLM 风险。
  5. 使用自定义 S3 到 Kafka 代理扩展批处理管道规模。


## 思维导图
用一张图看清主题之间的关系。
最大化未充分利用的 GPU 资源
多命名空间调度
使用 Valkey 和 Lua
原子优先队列管理
中央代理网关
自定义 S3 到 Kafka 代理
批处理扩展
实时和批处理 GPU 工作负载
查看大纲文本（无障碍 / 无 JS 友好）
  * 实时和批处理 GPU 工作负载
    * 多命名空间调度
      * 最大化未充分利用的 GPU 资源
    * 原子优先队列管理
      * 使用 Valkey 和 Lua
    * 风险缓解
      * 中央代理网关
    * 批处理扩展
      * 自定义 S3 到 Kafka 代理


## 金句 / Highlights
值得收藏与分享的关键句。
  * > 我们通过多命名空间调度最大化了未充分利用的 GPU 资源。
— 第 2 段
[⬇︎ 下载 PNG](https://www.traeai.com/articles/2c02fbec-8e7e-412c-902b-e46354b0ea37/quote/0)[𝕏 分享到 X](https://twitter.com/intent/tweet?text=%22%E6%88%91%E4%BB%AC%E9%80%9A%E8%BF%87%E5%A4%9A%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4%E8%B0%83%E5%BA%A6%E6%9C%80%E5%A4%A7%E5%8C%96%E4%BA%86%E6%9C%AA%E5%85%85%E5%88%86%E5%88%A9%E7%94%A8%E7%9A%84+GPU+%E8%B5%84%E6%BA%90%E3%80%82%22%0A%E2%80%94+%E5%AE%9E%E6%97%B6%E5%92%8C%E6%89%B9%E5%A4%84%E7%90%86+GPU+%E5%B7%A5%E4%BD%9C%E8%B4%9F%E8%BD%BD%0A%0Avia+traeai&url=https%3A%2F%2Fwww.traeai.com%2Farticles%2F2c02fbec-8e7e-412c-902b-e46354b0ea37)🔗 复制图片链接
  * > 使用 Valkey 和 Lua 进行原子优先队列管理和背压管理。
— 第 3 段
[⬇︎ 下载 PNG](https://www.traeai.com/articles/2c02fbec-8e7e-412c-902b-e46354b0ea37/quote/1)[𝕏 分享到 X](https://twitter.com/intent/tweet?text=%22%E4%BD%BF%E7%94%A8+Valkey+%E5%92%8C+Lua+%E8%BF%9B%E8%A1%8C%E5%8E%9F%E5%AD%90%E4%BC%98%E5%85%88%E9%98%9F%E5%88%97%E7%AE%A1%E7%90%86%E5%92%8C%E8%83%8C%E5%8E%8B%E7%AE%A1%E7%90%86%E3%80%82%22%0A%E2%80%94+%E5%AE%9E%E6%97%B6%E5%92%8C%E6%89%B9%E5%A4%84%E7%90%86+GPU+%E5%B7%A5%E4%BD%9C%E8%B4%9F%E8%BD%BD%0A%0Avia+traeai&url=https%3A%2F%2Fwww.traeai.com%2Farticles%2F2c02fbec-8e7e-412c-902b-e46354b0ea37)🔗 复制图片链接
  * > 通过中央代理网关缓解了 OWASP Top 10 LLM 风险。
— 第 4 段
[⬇︎ 下载 PNG](https://www.traeai.com/articles/2c02fbec-8e7e-412c-902b-e46354b0ea37/quote/2)[𝕏 分享到 X](https://twitter.com/intent/tweet?text=%22%E9%80%9A%E8%BF%87%E4%B8%AD%E5%A4%AE%E4%BB%A3%E7%90%86%E7%BD%91%E5%85%B3%E7%BC%93%E8%A7%A3%E4%BA%86+OWASP+Top+10+LLM+%E9%A3%8E%E9%99%A9%E3%80%82%22%0A%E2%80%94+%E5%AE%9E%E6%97%B6%E5%92%8C%E6%89%B9%E5%A4%84%E7%90%86+GPU+%E5%B7%A5%E4%BD%9C%E8%B4%9F%E8%BD%BD%0A%0Avia+traeai&url=https%3A%2F%2Fwww.traeai.com%2Farticles%2F2c02fbec-8e7e-412c-902b-e46354b0ea37)🔗 复制图片链接
  * > 自定义 S3 到 Kafka 代理成功扩展了批处理管道规模。
— 第 5 段
[⬇︎ 下载 PNG](https://www.traeai.com/articles/2c02fbec-8e7e-412c-902b-e46354b0ea37/quote/3)[𝕏 分享到 X](https://twitter.com/intent/tweet?text=%22%E8%87%AA%E5%AE%9A%E4%B9%89+S3+%E5%88%B0+Kafka+%E4%BB%A3%E7%90%86%E6%88%90%E5%8A%9F%E6%89%A9%E5%B1%95%E4%BA%86%E6%89%B9%E5%A4%84%E7%90%86%E7%AE%A1%E9%81%93%E8%A7%84%E6%A8%A1%E3%80%82%22%0A%E2%80%94+%E5%AE%9E%E6%97%B6%E5%92%8C%E6%89%B9%E5%A4%84%E7%90%86+GPU+%E5%B7%A5%E4%BD%9C%E8%B4%9F%E8%BD%BD%0A%0Avia+traeai&url=https%3A%2F%2Fwww.traeai.com%2Farticles%2F2c02fbec-8e7e-412c-902b-e46354b0ea37)🔗 复制图片链接


#GPU#AI#云计算#实时处理#批处理
[打开原文](https://www.infoq.com/presentations/realtime-gpu-workloads/?utm_campaign=infoq_content&utm_source=infoq&utm_medium=feed&utm_term=global)
[InfoQ Homepage](https://www.infoq.com/ "InfoQ Homepage")[Presentations](https://www.infoq.com/presentations "Presentations")Realtime and Batch Processing of GPU Workloads
View Presentation
Speed:
Download
38:24
/presentations/realtime-gpu-workloads/en/slides/Jo-1779784717175.jpg)
### Summary
Joseph Stein discusses engineering an enterprise AI-as-a-Service platform within a private cloud data center. He explains how to maximize underutilized GPU pools via multi-namespace scheduling, leverage Valkey and Lua for atomic priority queuing and backpressure management, mitigate OWASP Top 10 LLM risks via central proxy gateways, and scale batch pipelines using a custom S3-to-Kafka proxy.
### Bio
Joe Stein is an Architect, Developer and Security Professional with over 25 years of experience. He has worked on production environments at [Bloomberg](https://www.traeai.com/entities/bloomberg), [Verizon](https://www.traeai.com/entities/verizon), [EMC](https://www.traeai.com/entities/emc), [CrowdStrike](https://www.traeai.com/entities/crowdstrike), [Cisco](https://www.traeai.com/entities/cisco), [Bridgewater Associates](https://www.traeai.com/entities/bridgewater-associates), [MUFG Union Bank](https://www.traeai.com/entities/mufg-union-bank) and US Bank. He was also an Apache Kafka Committer and PMC member from Jan 2012- Aug 2016. Currently he is the Principal Architect of RD at SS&C Technologies.
### About the conference
Software is changing the world. QCon San Francisco empowers software development by facilitating the spread of knowledge and innovation in the developer community. A practitioner-driven conference, QCon is designed for technical team leads, architects, engineering directors, and project managers who influence innovation in their teams.
#### INFOQ EVENTS
  * /filters:no_upscale()/sponsorship/eventsnotice/1302f11a-f90f-4a79-96d1-3dd20d032144/resources/1HarnessWebinarMay28-Transcripts-1776246863928.png)May 28th, 2026, 1 PM EDT


#### [Shipping Faster, Breaking More: Rethinking Delivery Systems in the Age of AI](https://www.infoq.com/url/t/220a7c0e-bb35-4591-8ef0-76c0fcdc9c42/?label=Harness-Transcripts)
[Presented by: Eric Minick - Sr. Director of DevOps Solutions at Harness, and Aaron Newcomb - Senior Product Marketing Manager at Harness](https://www.infoq.com/url/t/220a7c0e-bb35-4591-8ef0-76c0fcdc9c42/?label=Harness-Transcripts)
  * /filters:no_upscale()/sponsorship/eventsnotice/34b23b6d-548d-473c-9ac7-ec2f8ca684b1/resources/1GuardsquareWebinarJune11-Transcripts-1777545596301.png)June 11th, 2026, 10 AM EDT


#### [Rethinking AppSec: Why Compiler‑Level Security Changes the Architecture Conversation](https://www.infoq.com/url/t/ee944bf8-22fa-415c-a609-559534f07cd0/?label=Guardsquare-Transcripts)
[Presented by: Anton Baranenko - Product Manager at Guardsquare](https://www.infoq.com/url/t/ee944bf8-22fa-415c-a609-559534f07cd0/?label=Guardsquare-Transcripts)
  * /filters:no_upscale()/sponsorship/eventsnotice/7dd71c7c-4b0e-4760-b97d-232ac1816637/resources/1NeuBirdWebinarJune25-Transcripts-1777458459989.png)June 25th, 2026, 1 PM EDT


#### [Architecting for Autonomous Reliability: Embedding AI into Your Observability Stack](https://www.infoq.com/url/t/1799cc66-1076-4f38-ba1e-fe340c13a7b2/?label=NeuBirdAI-Transcripts)
[Presented by: Justin Griffin - Head of Product at NeuBird AI](https://www.infoq.com/url/t/1799cc66-1076-4f38-ba1e-fe340c13a7b2/?label=NeuBirdAI-Transcripts)
  * /filters:no_upscale()/sponsorship/eventsnotice/0b46c1f1-7263-457d-82d9-12be6fa07fbd/resources/1DatadogWebinarJuly9-Transcripts-1779204853394.png)July 9th, 2026, 12 PM EDT


#### [Rethinking Logs in the Age of AI Analysis](https://www.infoq.com/url/t/71ed3a08-6275-4ce8-adb0-1ceaa4e4161a/?label=Datadog-Transcripts)
[Presented by: Nicolas Jung - Product Manager, Logs at Datadog](https://www.infoq.com/url/t/71ed3a08-6275-4ce8-adb0-1ceaa4e4161a/?label=Datadog-Transcripts)
### Transcript
**Joseph Stein:** I'm going to talk about our journey for how we built an AI cloud as a service platform for real-time and batch ingestion for our GPU inside of our data center. A lot of folks know me from the Kafka days when Kafka was first open sourced. It was written in Scala. I jumped in day one and ran it in production for my logs. I then worked with the LinkedIn team to make it durable.
I was the release manager, did lots of contributions and commits. I then went into the marketplace and started my own big data open-source consulting company, writing everything from surveillance systems to the whiteboarding behind conferencing systems, doing lots of large-scale petabyte-size real-time and batch processing systems at a bunch of different companies. I then landed myself at Bridgewater Associates. There, I worked on three different projects.
The first was for the former CTO of NSA, which was a security system. The second was reimagining the research engine, moving from the HPC systems in the data center to a secure Kafka streaming platform that we built in AWS. That project was for the current Microsoft CISO. Then my third project was for the original principal architect of Watson, basically reimagining Ray Dalio's principal operating system as the solution for not just people at Bridgewater, but people outside of Bridgewater.
After that, I went to Mitsubishi Financial Group, where I ran a team for mainframe modernization around core banking technology. Then, one day, we got a consent order where the UCC basically said, we're going to take away your FDIC insurance unless you do these 18 things across 20,000 pages. I got handed three teams, and I was in charge of three of the MRAs. The next day, we got acquired by a U.S. Bank. Most of the assets got acquired.
I got put in charge as a co-lead for moving all of the data and all the money from the old bank to Mitsubishi Financial for what wasn't purchased, and all the data and money from the old bank to the new bank for what was purchased. After that, I landed myself at SS&C Technologies. We're a hybrid cloud-first company. Yes, we're in the financial sector, but we're also in health and insurance and have products that are horizontal from an industry perspective around automation.
In our hybrid cloud offering, we have our own private cloud. We have data centers all around the world, not in every country we do business in, but at least regionally so that we can cover where we need to do business with our own data centers. Our private cloud is just as you would expect the cloud to be. There's a Terraform provider and an API and a user interface, and you can get Kubernetes clusters. If you need data from the mainframe, that'll CDC to a Kafka topic. If you want a Kafka topic just for yourself, you just call an API and you get that.
### AI as a Service - From the Beginning
When I started two years ago, there were two groups that were running advanced RAG systems, and they really knocked my socks off on what they were doing. I thought to myself, how do we do this, but for everyone at SS&C? I started thinking about what would an AI as a Service look like from a cloud perspective to allow all of our engineers and all of our products within our private cloud to consume GPU, not break the bank and still cover all the conformance to policy and compliance to regulation that needed to happen.
I started off technically first. First I said, is this even going to work? I had not written code in a while at the bank, so I half wanted to make sure that I could still code this thing and half making sure that this whole vLLM, NVIDIA driver business was legitimate. For anyone who's ever worked on sound cards with Linux device drivers before, it's kind of like the same thing, but once you get the hang of it, it's all fine. I got my prototype working in AWS. That was all great.
I then looked from a security and governance perspective and I said, now that we want to have generative AI, what does that actually mean? What does that mean from a compliance perspective? What are the regulations out there? Can our AI give financial advice? No, there's a FINRA regulation that says we can't do that. What are the ISO certifications that we can get that'll line up with the other ISO certifications we have in our data center that'll allow us to have the right conformance to our policies and compliance for regulation for not only what's out there but for what was growing?
I did this 18 months ago. The EU Act had not passed. A lot of industries weren't out there. The OWASP Top 10 had not even been created. Really trying to scour and understand where the landscape comes from. The OWASP Top 10 eventually did come out, and for anyone who does anything with generative AI, it should be the same way with anyone who does anything with the user interface.
You are susceptible to cross-site scripting, and that's in the OWASP Top 10. For AI, you could be model denial of service attack. You could be susceptible to that. You should be looking at not just the high-level bullets of the Top 10 for LLM from OWASP but getting really into the nitty-gritty detail. I started off with what was just the OWASP governance document which is like a 32-page document for my initial requirements, and this eventually did come out, which was great. I had finally got to the point where I had a vision of what I wanted to do. I had a working piece of software, and I knew what I needed from the business to take the next step.
Usually, I just ask for forgiveness. That's just how I am. It was time to go ask for approval. I needed to buy hardware and GPUs are not inexpensive. Besides hardware, I needed some resourcing. I needed a dedicated resource that would work with me as a partner day by day. I needed resources from the private cloud team to introduce the GPU, so when it came to the data center, it would get racked and stacked.
It would get an operating system on it. It would get into our SALT and forming systems. It would get to the Kubernetes cluster. The operator would get there. When I went to those teams to work with them, I needed their times from their sprints in order to do this. We also needed to institute an AI and governance policy.
For all of our customers, for all of what we do, we can't just willy-nilly do whatever we want when especially we have in our contracts already obligations around what we can and cannot do around AI. Our existing contracts were already establishing that. AI is not new. We've been doing machine learning for years. We have a whole algo division where we do trades around like quants. AI is not a new thing.
To align the generative AI pieces to the traditional AI pieces of what is happening in the organization was something that really had to be called out and really taken attentive to. I said to the CTO, if we do all of these things, we could have a platform. Got approval. The vision was simple. Without a gateway, you have nothing but risk. You just have transactions coming into your models.
You don't have any way to manage them. You don't have any way to put any security controls on them. The proxy then eventually turned into a set of microservices so that when transactions come into the gateway, the first thing is everything is going to get audited. Then on a request, there are different types of request guardrails that may or may not get applied to a use case. For a chatbot, you might want to have prompt injection attacks.
For an agent, you may not, unless it's with email, so you're not susceptible to the Morris II worm. For an agent, you may not want to take the latency hit of a prompt injection detection. If it's exposing to a source where someone may be able to do that prompt injection, you want to trigger that. We have the ability to trigger those request guardrails based on use cases. When the LLM model call is done, there's other guardrails you want to apply.
What if the model went a little off the rails and said something toxic? You don't want that coming back in a chatbot. You want to apply in chatbox, toxicity guardrails, to make sure that the content coming out of the LLM has efficacy. Or if you're doing something where you're proving that financial advice hasn't been given, and you're in a RAG system, you have to check that the inputs to the LLM match the outputs to the LLM, and the log probabilities are such that the LLM isn't doing anything to adjust that, and you're checking those two things, that the inputs and the outputs are the same.
To do that all within one central system, and none of my users ever have to worry about that. All they care about is just, I need a key, I need v1 chat completions. I have to do a little configuration from governance, and then after that, everything else is under the hood.
We went live last August. We started growing really quickly. By March, we had 250 users in production. Those weren't just human users, those were dozens of production software systems that were in CMDB that were live customer-facing systems, all just on one 8-chip chassis sitting in Kansas City all by itself. It was scary times. We learned a lot. We offered inference, embeddings, and RAG as a Service.
What we really learned more than anything was that our SDLC for what we needed as a team for our bits for dev, test, UAT, prod, demo, security checks, branches, all of those bits require GPU in some way, shape, or form. Everything we were doing, we really needed the model we were trying to do it with, essentially. We were trying to call reasoning, and you can't just do that on CPU. We have to pass in the flag for reasoning and see how long it takes for tests. We needed that inside of our SDLC.
Our users, they had their own SDLC too. They were changing prompts, and they were releasing their own software, and they needed not only a stable place that they can pin themselves to, but they wanted a check on new features that we were releasing as well, and also see our new bits. The question was, how do we take this one piece of hardware, which, this is just the beginning because we're about to go out and buy a whole bunch of other hardware, and we don't want to go and spend $100 million on it.
How do we do this in an effective way so that we can maximize the over-subscription of our total utilization of all of our GPU spend. That's very key here. Now we're a team of 40 across a whole bunch of different workstreams, AI R&D teams, agent and MCP teams, core gateway teams. We have a governance and a chat team. We have 80 GPUs running in two different regions in four different zones, and we're going to get into that in a little bit.
3,000 employees are using our chat system so that nothing ever leaves the data center, and that's all integrated with RAG and web search, so you get the ChatGPT effect. We have 650 users now, mostly across application services. There are developers, too, and engineers that use it, and over 1,000 use cases in production. We've also aligned ourselves with a business unit, so we get their 24/7 support for Tier-1 through Tier-3.
### vLLM on the GPU - Optimizing for Cost and Uptime
Let's get into vLLM on GPU. Even though, in general, everything I say you can probably do without vLLM. For the most part, this is really just about vLLM. This is your hardware. This is what you get, 4,000 tokens per second per chip. It's not a lot. You're spending lots of money for this, and you're not getting anything at all. It's a bottleneck.
It's the worst bottleneck you've ever faced because you're spending so much money, and the expectations are so high because generative AI is the most amazing thing in the world, and you just dropped eight figures, and nothing is going on, and nothing is happening. When you run these models, they're really VRAM-hungry. They're going to take up all your VRAM, and you're basically going to be statically pinned to the model.
On our H100s, we have our own custom embedding service that we wrote that does micro-batching to improve efficiency. When you send 650 pages of strings, we basically micro-batch that down into the GPU to make it faster than just pushing it all in, and all that's over gRPC. Then for the Llama models, the older Llama models, we just run two chips for vLLM for 8B and four chips for 70B.
We want to run this, again, across two dimensions of environments, the top dimension, which is our customers, Dev, Test, UAT, and Prod, and our dimension for our bits, which is Prod DR, Demo, UAT, QA, Dev. How do we apply that over this piece of hardware? We were really successful, so we had to go buy more hardware. We needed more models for more use cases and had more transactions and more utilization.
Now we have H200s and H100s. From a Kubernetes perspective, those are just different node selectors. That's trivial, that's no big deal. Just the H100s, the models go to those, the other models go to the other node selector, and you're good to go, really simple to do. You're still stuck with this need for oversubscribing across the two different dimensions.
In our governance system, after you create the use case and set up your guardrails, and this is integrated with ServiceNow, so for your business unit and your CMDB application and your AI models, you're basically specifying your tenant, and you're saying, is this tenant Dev, test, UAT, or prod? Our first slice into the software system is basically saying to the user, now when you connect to this environment, whatever that environment might be, you're going to be sliced and diced based on one of these four environments.
Your user is going to be for one of those environments, because the prod user isn't going to be Joe Stein, the prod user is going to be some service account. That prod user service account, we recognize is in that tenant, and then now we've sliced and diced on that, and we could apply functionality, that we'll talk about in a little bit, according to that dimension.
Besides all the hardware that we had to buy for production, we also had to duplicate all of that purchase for standby in case production ever went down. While that's all fun and good, that's really just a whole lot of money that sits there and does absolutely nothing, just waiting for disaster to strike, which, knock on wood, doesn't happen. What do we do? What we did was we separated all of our bits by namespace, and all of our LLM proxies by namespace.
We run disaster recovery as a namespace. We run UAT as a namespace. We run QA as a namespace. We run all of our other namespaces sitting in the hot standby environment, waiting for traffic to come in. Now we're able to have another dimension across another set of underutilized systems for traffic to come in, and basically figure out what we should be doing with that traffic.
There's a little bit of a flaw here, and this is where we are today, and I'll talk about what we're going to do for the rest of the year, because each of those systems here that are sliced by namespace, each of these LLM proxies, they don't know about each other. What you need to do is create a single GPU pool proxy that all the LLM proxies talk to. When the traffic to the LLM proxy comes in to each of the different environments, and it comes to the GPU proxy, the GPU pool proxy sees both dimensions.
It sees the dimension of, you're prod, you're prod DR, you're prod DR prod. This is priority one, this must be disaster mode, and I need to have this go through. Maybe the next priority is demo prod, because we have our own conferences and sales demos, so now we could have those demos running in our hot standby environment, and when traffic fails over, we'll still have priority for the non-production traffic to run without it getting in the way of the actual production traffic.
Just by adding priority queuing on now two separate dimensions, one for the environment coming in, and two for the environment of where our bits are in namespaces. One is tenant, and the other is Kubernetes namespace.
This is what we look at when it comes to the features and functionality of what we apply on these two different dimensions. Now that we've finally figured out how to slice and dice our underlying hardware into two virtual paths, we look at it and we say, we want to do all sorts of different rate limiting. We want to rate limit on the total system. We want to rate limit just on the request to a model over a window period of time. We want to look at the tokens to a model over a period of time.
We want to break this up by tenant, by model. We won't even have timeouts because some models, like the 8B model, if it takes more than 7 seconds, something is probably wrong. Like the Qwen 235B thinking model, yes, it can go on for 90 seconds just thinking. You've got to be reasonable about the reasoning and what you're doing in your system, because if you do nothing, vLLM is just going to sit there for 25 minutes just holding onto your request and you're stuck, you're going to have to reboot your model and you've got to put timeouts in.
It's a very finicky system. At the last piece was, we want to apply priority queuing. On all these different rate limits and all of what we're doing, we want to apply all this priority queuing to all this oversubscribing virtualization stuff that I've been talking about. Let's talk about how that works. vLLM out of the box is a pretty straightforward system. You've got requests coming in, it goes into a queue, and vLLM decides whether it wants to process it or not.
If you don't put in 2-minute requests, it'll go ahead and process it. If you put in too many requests, it's going to fall over on you and die. The architecture is also very straightforward. We use the OpenAI compatible API server that goes into the async LLM engine. The LLM engine holds the queue that goes down to the model execution engine and magic happens. vLLM is really nice to you though, because when it falls over, it's going to tell you.
You can easily call vLLM and say, are you going to fall over? vLLM has an internal queue and you can call that queue. If you call that queue and that number is ever above 3, backpressure time, you need your own queue. There's an NVIDIA article about this. Trust me, you can test it yourself. It's a really simple endpoint. It's on the same URL that you're going to now for your inference. It's just on the metrics endpoint.
It's in the JSON. You look at it, you see the number is 3 or higher, and you implement backpressure. The way we did this is all with Lua scripts inside of Valkey. Everything is running atomically in memory, both from our rate limiting perspective and also from all of our queuing perspective, everything outside of establishing the queue size. Everything else but that runs in memory in Lua scripts. Extremely fast, atomically.
It does all of the different checks that we want to do for the rate limiting, and also handles the priority queuing as well as the waiting for the backpressure for vLLM to not be as busy as it needs to be. This is what allows us to basically handle any workload from any environment for any type of use case, either in Kansas City, or St. Louis, or London, or Wales, based on our capacity, essentially, by having this type of virtualization. We set up the rate limits and the priority queuing in a way so that production traffic and production use cases could get what they need versus dev traffic and dev use cases can get what they need.
### Batch GPU Workloads - Document Intelligence and Voice Transcription
Let's talk a little bit about batch GPU workloads. When we got started, we built a synchronous RAG system. Our thought was you wanted to upload your document. We had a multi-platform upload feature that streamed in files. After you uploaded your file, you would go ahead and chat with that. The processing and steps that are involved in our document intelligence system are actually multiple LLM calls as well as embeddings calls.
You're making two, three transactions to the GPU during a document intelligence lifecycle. There's traffic there that we were generating ourselves, on ourselves, during production hours, and we found out that that was for no good reason. Some people, when they uploaded their RAG document, they didn't want it to go live until next Tuesday at 8 p.m., because they were switching out their documentation and releasing the next version of their documentation. They didn't want it to go live right now.
Next Tuesday at 8 p.m. is a great time for us to go ahead and get tokens from the GPU because it's being underutilized at that time. Between 4 a.m. and 6 p.m. is when we're most utilized. 8 p.m. on a Tuesday is a great time to do document processing. The dips that exist in our utilization were something that we started to look at and say, how can we take advantage of where we're not utilizing the GPU when we can be utilizing the GPU?
We created a new control plane all around file management. We said, ok, you want to do something with a file with us. It may be RAG. It may be audio transcription. It may be something else. When you register this file with us, provide us a structure for your SLA. What is your expectation? Do you want this audio transcription file to be done in five minutes? Because we could calculate that now.
We have all of that in memory sitting in Valkey to figure out the size of the tokens and the queue size. All of that is there for us to just go and make a calculation and in real time say whether or not we can meet that SLA. When we have scheduling in Windows, those are trivial. Those are just other queues and other windows of time, like ad campaign spreading. Those are really no big deal either. We can tell them right away whether or not we can fulfill their upload.
They go ahead and they do their registration of their file. After they've done the registration of their file, now it comes down to, how is the file going to get to us? We started with the multi-form part upload and very quickly realized that wasn't going to work. It wasn't well supported from a tools perspective or from a developer perspective. Some people didn't even know what a multi-form part upload was. We decided instead that we were going to go S3 from a protocol perspective.
That S3 was going to be the way that our users were going to see us presenting our system to them to send us files. That wasn't good enough because we had a lot of agents that were being built. Those agents were integrating with MCP Confluence servers, and they were trying to get Confluence spaces into our RAG system as well. Those weren't files. We started having to think outside of just S3 files but also how we're integrating some of these MCP agents to transfer us over the data and chunk over that data as well.
What we ended up doing is we wrote our own S3 proxy server. As I said, we're multi-cloud. I can't get stuck in any one cloud's particular environment especially on-prem where one company is using pure storage and another company isn't, and one company uses Azure and another company uses AWS. For us, we just built our own S3 proxy. All that S3 proxy needs is a bucket. Everything else after that is completely under our control.
The S3 proxy integrates with our authentication and authorization service, and integrates with Key Cloak so that users can SSO and just click and download and view a file. If something got transcribed and you want to hear the original file, an auditor can just come in and just download the file. It's just available through the web. Everything is wrapped around the Open Policy Agent using Rego for controls. We have a webhook that's integrated so that when the webhook executes, all of that just goes to Kafka.
Once we have all of the files having to go to Kafka, we know where they were all going and what all virtual buckets they were meant to do. They can come up in a consumer and we can check from the consumer their registration and figure out what to do next. Because we registered the file first. Since we registered the file first, as long as that occurred, then the consumer should see that registration. Once we see the consumer registration, everything is really just simple plain old Kafka stuff. Send it down the data pipelines. Everyone knows how to do that.
### Summary
In summary, know your threat model and risk appetite, especially when venturing into this AI stuff. Just in speaking with all the folks that I speak with on a daily basis, there's the Wild West approach and there's the lock it all down approach. I'm hoping we all get to something in the middle where our risk appetite is something where we can provide effective controls around AI. Also figure out your oversubscription model.
When you buy GPUs, really think about the tokens per second and who's going to get them. How are you going to slice and dice it? What deployment zones they're going to go into and what regions they're going to go into. How much money you're really going to spend on them versus how much you need to spend on them. Also, have a gateway for your traffic. When I started this, there was no concept of a gateway.
If you're on AWS, there's an AWS gateway. Envoy proxy has a gateway. There's all sorts of gateways. You can go out there, you can build your own gateway. Take a look at the guardrails, build the gateway, run an efficient system.
### Questions and Answers
**Participant 1:** I was wondering whether you have ever experienced GPU partitioning or sharding, and eventually what's the order. Because personally, when I experienced with distributed GPU computing, one of the main issues that I'm dealing with is GPU partitioning. Personally, I never had the experience of interacting with enterprise-level H100, which I know they are capable of native partitioning, but whether you are using that or not.
**Joseph Stein:** There's the, is it capable versus should you do it. I have four chips. I need all four chips to do everything it can. I wish I had six chips, but I don't. I only have four. For me, partitioning is just too much. I think of it as pure static partitioning. Those two chips are all I'm going to get for those models, and that's static. Everything else after that is elastic.
That's where my partitioning comes in. I don't try to say, I'm going to quantize this model and fit it into this part of the VRAM, and then quantize this model and fit it into this part of the VRAM. Maybe in Q1 we start to do that so we could have more denser population of models. At the end of the day, at least the way I go is like, you don't need a PhD to do accounts receivable.
If I'm doing invoice processing, the Llama-3.1-70B model is perfectly fine. I don't need to partition it. Maybe next year more models come out and I need more space on my GPU. For right now I have nine models. They do everything my company needs. If some new model comes out that they need, maybe.
**Participant 2:** I have two questions. The first question is, do you know whether the token cost is competitive, including the 40 FTEs and 80 GPUs versus if you're using a Bedrock service? Does it end up being token competitive? Then my second question is about model depreciation. Presumably, people want to try new models as they come out all of the time. You've got a responsibility to support those legacy models, but you've only got a fixed number of GPUs. How do you think about the model depreciation cycle and model upgrade cycle?
**Joseph Stein:** Two things, one, I had a hard requirement where data couldn't leave the data center. It just wasn't an option to leave the data center. We do run on AWS now. There is a gateway on AWS. It's not just Bedrock that's the cost there. At that point, it actually becomes our cloud versus AWS cloud.
Even though the GPUs, in and of themselves, are not really going to make anyone really excited that one is less cost than the other. Us having our own private cloud and our own data centers is such a dramatic cost than what it would run to run that system in AWS that it ends up becoming a better option for us. I'm hoping that the cost of inference overall will go down.
When we went to buy the NVIDIA H100s to begin with, I looked at this company called etched.com, the transformer is in the hardware. vLLM is a chip. I was just like, I want that. It wasn't ready yet. We weren't going to go and spend seven figures for something that wasn't even fabricated yet. We had to go with the NVIDIA chips.
I think over time, we're going to see like, "I don't need graphic processing. I just need inference. I just need that only component of that architecture for that chip". What I'm hoping is that over time, I can just get inference, because I don't do training. I don't train models. I don't need that. I don't need to pay that money. I don't want to pay that money. I want to just scale inference for that.
When we run on Bedrock, we do separate what we run on instances versus what we run on Bedrock. Because there's a price comparison on AWS. If you're at a certain like 10,000 requests per second, it's going to cost you less money to run vLLM on instances. Depending on our usage scenarios and our environments, like a dev environment in AWS, we're not going to call Bedrock because it's going to cost more money. Then once we're scaling in production environments and we want serverless and the reliability, we'll go and push the buttons and pay the money for Bedrock. It depends on where and how we're going into it, and where and how you need to slice and dice it.
**Participant 3:** Since you spoke so much about vLLM, I'm just curious, did you evaluate Triton or some of the other inference engines? I think NVIDIA also open-sourced Dynamo recently. Did you also evaluate some industry-wide practices of optimization, like KVCache? If you tried something like that, did it help you increase your inference throughput and overall GPU utilization?
**Joseph Stein:** When I started this, I started with llama.cpp. Then I found vLLM and it was much faster. Then one of the business units that was doing the advanced RAG, they used vLLM. My initial using of vLLM was, if I get hit by a bus, like Raj could go ahead and handle it from interlinks and take over. I started with vLLM for that reason.
Then over the last 18 months or two years, I've looked at SGLang. SGLang is a fine inference engine. Very good. I've looked at Triton. I worry about getting into vendor technology stacks. I really worry about that as an open-source person. To me, open source is only open source if it's behind a foundation: Apache, CNCF, Linux. Otherwise, who knows? Tomorrow, it's not open source anymore. Copy left.
I'm always very hesitant about that stuff. vLLM being a community, I like the community. They've taken patches before that we've had, and they've been great. It's a community thing.
On the KVCache, we didn't really look into that much. We have looked at a couple of systems, but we have not spent any time yet doing anything there. That is definitely an area for us to go ahead and add more effort to do optimization.
**See more [presentations with transcripts](https://www.infoq.com/transcripts/presentations/)**
Recorded at:
Image 6
May 26, 2026
by
  * [![Image 7: Author photo](https://cdn.infoq.com/statics_s1_20260519082444/images/profiles/73c479b6cdfe4e6a46308569d20f89fb.jpg)](https://www.infoq.com/profile/Joseph-Stein/)[Joseph Stein](https://www.infoq.com/profile/Joseph-Stein/)


Principal Architect of R&D @SS&C Technologies
AI 伴读
实时和批处理 GPU 工作负载
InfoQ
Joseph Stein 分享了如何在私有云数据中心构建一个企业级 AI 服务平台，通过多命名空间调度、Valkey 和 Lua 原子优先队列管理、OWASP 风险缓解和自定义 S3 到 Kafka 代理来实现实时和批处理 GPU 工作负载。
1. 通过多命名空间调度最大化未充分利用的 GPU 资源。 2. 使用 Valkey 和 Lua 进行原子优先队列管理和背压管理。 3. 利用中央代理网关缓解 OWASP Top 10 LLM 风险。
GPU, AI, 云计算, 实时处理, 批处理
