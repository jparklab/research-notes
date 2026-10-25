---
title: Productionising Large Language Models in Government | by Ng Shangru | AI Practice
  and Data Engineering Practice, GovTech | Medium
id: productionising-large-language-models-in-government-by-ng-shangru-ai-practice-an
tags:
- internal-llm-inference-stacks-7f0c90
- locus-under-documented-layers-audited-absence
- government
- self-hosted-llm
- text-generation-inference
created: '2026-10-10T00:06:07.334746Z'
updated: '2026-10-10T14:52:17.265618Z'
source: https://medium.com/dsaid-govtech/productionising-large-language-models-in-government-0fbf3909311b
source_domain: medium.com
fetched_at: '2026-10-10T00:06:07.311469Z'
fetch_provider: crawl4ai
status: review
type: note
tier: commentary
content_type: blog
deprecated: false
summary: 'Detailed first-person engineering account (15-min read, Nov 2023, by Ng
  Shangru, SWE at GovTech, published under GovTech Singapore''s ''AI Practice and
  Data Engineering Practice'' Medium publication) of the Whole-of-Government (WOG)
  NLP team''s roughly year-long journey hosting production-grade LLMs on-premise/in
  Government-on-Commercial-Cloud (GCC) for the Singapore government, written to avoid
  dependence on OpenAI/ChatGPT (US-hosted, proprietary, data-residency risk). Covers
  three serving-stack generations: (1) BLOOM-176B on AWS using 5x g4dn.metal (Nvidia
  T4, 16GB each) orchestrated via Ray + Alpa, constrained because model layer count
  must divide evenly into instance count (so only 1/2/5/7/10/14/35/70 instances work)
  and costing ~0,000 USD/month to run 24/7, with only 47% single-call GPU utilization
  and throughput degrading as concurrency rose; (2) a move to GCP A100 80GB nodes
  and HuggingFace''s transformers-bloom-inference (using Accelerate + DeepSpeed),
  which quadrupled throughput versus Alpa (4.06 vs 2.23/1.17 tokens/sec) but still
  took 238s to load the model; (3) adoption of HuggingFace''s Text Generation Inference
  (TGI, released July 2023) serving Llama-2-70b-chat-hf, which fully saturates GPU
  even on single calls via internal queuing/batching and gRPC to model shards, paired
  with a Qdrant + OpenSearch vector store and a self-hosted gte-large embedding model
  (replacing OpenAI''s text-ada-embedding-002) served via Gunicorn/FastAPI with sentence-transformers,
  all fronted by a Chainlit UI with FastAPI backend. The on-premise A100 server (obtained
  via the Government Infrastructure Group) runs fully offline RAG serving Secret-classified
  workloads; a separate GCC setup handles Confidential workloads. Gives exact sizing
  math (1B params = 2GB storage/2GB GPU RAM at bf16; Llama-2-70B needs ~140GB, though
  their deployment used ~272GB GPU RAM at max context/concurrency) and names forward-looking
  exploration of vLLM, llama.cpp, exllama, ollama, quantization (GPTQ/NF4/GGUF/AWQ/EETQ),
  and NVIDIA Triton Inference Server as a possible TGI alternative.'
---

[Sitemap](https://medium.com/sitemap/sitemap.xml)
[Open in app](https://play.google.com/store/apps/details?id=com.medium.reader&referrer=utm_source%3DmobileNavBar&source=---top_nav_layout_nav-------------------------------------------)
Sign up
Get app
Sign up
## [AI Practice and Data Engineering Practice, GovTech](https://medium.com/dsaid-govtech?source=post_page---publication_nav-4b4dbe66b49-0fbf3909311b-----------------------------------------)
Follow publication
AI Practice and Data Engineering Practice, Government Technology Office, GovTech
Follow publication
# Productionising Large Language Models in Government
## A look at the journey taken towards hosting our own on-premise LLM
Follow
15 min read Nov 10, 2023
Share
## Introduction
When ChatGPT was released by OpenAI back in November 2022, it ignited a seldom seen global fervour in Large Language Models (LLMs) and their applications beyond research. LLMs were not new, and neither was the core Transformer concept powering them under the hood.
What made ChatGPT a ubiquitous term for AI assistants almost instantly (like how we say “Google” it) was its ease of use and abstraction away from the nitty gritty hosting and serving of AI models behind a web server. It was, on the surface, a large step up from “chatbots” (do we even use the term anymore?) and almost everyone could feel the difference.
There were plenty of possibilities where LLMs could be utilised in our daily work to improve productivity and effectiveness of our public officers, and as the NLP team at GovTech, we were tasked with evaluating and assessing how LLMs could be deployed to the wider Whole-of-Government (WOG).
However, there were a few downsides for us to use OpenAI / ChatGPT as-is:
  1. OpenAI was cloud based and only in the US. This means any data sent to ChatGPT must go through their servers and to the US.
  2. GPT3 / GPT4, the model powering ChatGPT, is proprietary. This means that we don’t have access to the source code, nor can we replicate it in our own environment.


Therefore, there was a significant need to build up our own capabilities in deploying capable LLMs without being entirely reliant on proprietary LLM services like ChatGPT.
It has almost been a year since, and the landscape has significantly changed. We will be sharing our journey on hosting production grade LLMs in the WOG environment. Optimising LLM hosting covers a very large domain that cannot be sufficiently described in detail in this article, but we will try our best to cover the aspects that we have considered.
## Ancient History
Before we could learn how to fly, we had to learn to crawl. Crawling in this case would mean understanding the infrastructure requirements in hosting LLMs, the tech stack involved, and actually serving an LLM.
We also had to first define our constraints for LLM hosting in government:
  1. Usage of our Government-on-Commercial-Cloud (GCC) environment.
  2. Must use an open-source PyTorch-based model of equivalent size to GPT3.
  3. Infrastructure sizing was only for inference, not for fine-tuning.


## Using GCC CSPs
We chose to focus on AWS first as most of our production workload ran on AWS.
It was also around this time that Azure OpenAI became available, and we had a partnership with Microsoft for the [limited access usage of Azure OpenAI](https://learn.microsoft.com/en-us/legal/cognitive-services/openai/limited-access) with enhanced security requirements, such as enhanced content filtering and abuse monitoring. This meant that while data was still sent to Microsoft US, it is part of GCC and properly monitored. Given the availability of Azure OpenAI, we opted not to do our setup in Azure.
## Using an equivalent open-source model
At that time there were only two open-source models that were near the size of GPT3 — BLOOM and OPT-175B. We used BLOOM for our setup as OPT-175B (while open-source) still required a lengthy approval process to obtain the model weights.
## Sizing infrastructure for inference
The infrastructure setup was purely for inference usage a la ChatGPT. It was not meant to support fine-tuning a pre-trained LLM which would require significantly more resources, roughly at least three times the inference infrastructure on average.
## Infrastructure Design
After understanding the constraints, we came up a simple infrastructure design which looked like this:
Press enter or click to view image in full size
LLM serving infrastructure design. Image by the author
A model would be stored somewhere accessible like an S3 bucket, which would then be loaded into memory on the service nodes, and in turn managed by a service orchestrator. An optional frontend is then used to allow user access to the model.
## Why not AWS SageMaker / Google Vertex AI?
PaaS LLM offerings were still fairly limited at that stage, as they were also playing catch up with the LLM development. You would have only been able to host LMs, sans the Large. For example, AWS SageMaker only offered hosting up to BLOOM 1B7 — that’s BLOOM with 1.7B parameters compared to the full-sized BLOOM which has 176B parameters.
## Understanding LLM sizes
LLM sizes are generally measured by their parameter size which reflects the amount of data that they have been trained on. For example, OPT-175B indicates that the model has 175 billion parameters. Typically, larger models tend to have better performance, so it is in our interest to be able to support as large a model as possible.
Furthermore, LLMs at their core are nothing more than “next word predictors” — when given a certain context, it calculates the next most probable word using fancy mathematics. This means that the model data (called “weights”) are stored as numbers, and there are many ways to store numerical data in computers.
Press enter or click to view image in full size
bfloat16 structure. Source: <https://cloud.google.com/tpu/docs/bfloat16>
BLOOM uses bfloat16 (also known as half precision), which has the equivalent range of a float32 (full precision) while using only the same size as a float16. This means the model size can effectively be halved while maintaining the same level of precision.
BLOOM model parameters. Source: <https://huggingface.co/bigscience/bloom>
For bfloat16, a general rule of thumb is that every 1B parameter will require 2GB of storage and 2GB of GPU RAM to run. Therefore, a 176B model will need approximately 352GB of storage as well as 352GB of GPU RAM to run.
With an understanding of the model sizes, we’re now able to determine the kind of compute infrastructure required.
## AWS Infrastructure Challenges
To be able to host the entire model in a single node, the only available configuration was to use a Nvidia A100 8x80GB server. Sadly, this was only in the AWS p4d series and was not available in the AWS Singapore region. We had to figure out how to host a model over a distributed network. This added significantly to the technical challenge as there was now an additional orchestration layer that had to be handled, which was only sparsely explored and documented by others.
We eventually settled on using [Ray](https://www.ray.io/) as the orchestration layer and [Alpa](https://alpa.ai/) as the model serving layer. This was the setup we implemented, using 5x g4dn.metal instance which was the instance with the largest GPU RAM available in the Singapore region. The AWS G4 series uses Nvidia T4 GPUs which have 16GB GPU RAM each.
Press enter or click to view image in full size
AWS G4 series specifications. Source: <https://aws.amazon.com/ec2/instance-types/g4/>
Press enter or click to view image in full size
Ray cluster setup. Image by the author.
## Why 5x g4dn.metal?
The astute ones among you would’ve noticed that the g4dn.metal offers 128GB of GPU RAM. A simple calculation would show that 3 instances would’ve been sufficient since 3 x 128GB = 384GB > 352GB.
However, a not so apparent issue with using 3 instances was that the number of instances must be a factor of the number of layers of the model. This means that 70 / 5 = 14 is ok, whereas 70 / 3 = 23.333… will not run. Therefore, the available instance counts for running BLOOM had to be one of either 1, 2, 5, 7, 10, 14, 35, or 70.
## Benchmarking
Using this setup, we ran a simple concurrency test to evaluate the performance.

```
'{"prompt":"Once upon a time", "max_tokens":"64", "temperature":"0.7", "top_p":"0.7", "model":"default"}'
```

From the results we can observe that
  1. The serving library does not fully utilise the available infrastructure (only 47% GPU usage for single call).
  2. As the number of concurrent calls increases, the throughput decreases.


Not the fastest results by any means, but it is an LLM running in GCC! 🥳 Probably also the first!
## Problems with Ray + Alpa
While it was a working setup, we foresaw that this approach is likely not sustainable in the long run as there were a few caveats and challenges that we encountered.
### 1. Model Loading
Each time the setup is updated, the model must be reloaded into memory, and this takes a non-trivial amount of time ranging from a few minutes to half an hour.
### 2. Model Serving
Optimising the model serving libraries involve extensive modification and development in writing kernel level code which is beyond our expertise and probably not sustainable to deep dive into (versus leveraging open-source libraries)
### 3. Model Parallelism
Related to the point above, optimising the serving code involves understanding and using the best combination of model parallelism to achieve the best performance.
### 4. Model Format
Alpa requires a conversion of the standard PyTorch weights into an Alpa-PyTorch format, which is dependent on the frameworks’ development roadmap as to whether future models are supported.
### 5. Non-deterministic autoscaling
As mentioned earlier in the model layer divisibility portion, the behaviour of autoscaling nodes is unknown — whether it was necessary to scale in sets of 5 or whichever number was set at the start, or one by one which would potentially crash the model. Documentation was fairly scarce on this front.
### 6. Cost of setup
It is not cheap to run 5x g4dn.metal instances 24/7 (approximately $30,000USD / month).
BLOOM’s out of the box performance was also not close to what GPT3 offered. Hence, we also explored several techniques to improve it, such as:
  * Switching to an instruction-tuned variant of BLOOM, named bloomz-mt0, and subsequently bloomCHAT.
  * Using Prompt Engineering techniques specific to the model.
  * Using reward models in tandem with prompt engineering to refine inference results.


## Enter GCP, transformers-bloom-inference and RAG
## GCP A100
Not long afterwards, Google made their A100 40GBs and 80GBs available in the Singapore region. This meant that we could now host an entire model in a single node without fiddling around with orchestration and could focus more on optimising the serving libraries.
## Get Ng Shangru’s stories in your inbox
Join Medium for free to get updates from this writer.
Subscribe
Subscribe
Remember me for faster sign in
We replicated the AWS setup into a single A100 8x80GB GCE node and this was the result:
The results were slightly improved over the AWS setup, which was partially attributable to the better hardware. However, it became more apparent that the Alpa library is not able to fully utilise the GPU as it only peaked around 50% usage even with multiple concurrent calls.
## transformers-bloom-inference
[transformers-bloom-inference](https://github.com/huggingface/transformers-bloom-inference) is a collection of packages designed specifically for fast inference on the BLOOM model family. It allowed usage of Accelerate by HuggingFace and DeepSpeed by Microsoft to improve model throughput. We ran a comparison benchmark versus the Alpa setup we had and obtained the following:
As you can see, the choice of serving library significantly affects the throughput (4.06 tokens/sec versus Alpa’s 2.23 / 1.17). This framework also outperformed purely using Accelerate or purely using DeepSpeed for model serving, and thus we used this framework going forward.
Also note that the model loading time (238s) is still significantly long enough to make an ad-hoc lambda style serving mode infeasible.
## Retrieval Augmented Generation (RAG)
Together with improvements in the model hosting space, there were also developments in the model performance space, with RAG becoming increasingly common.
As is its namesake, RAG relies on supplementing LLMs with additional facts (provided as context) for the LLM to generate a reply.
This was attributable to the fact that the majority of the non-generative LLM use cases revolved around factual Q&A based on internal knowledge bases — like a Google Search for your own company data.
RAG required a slightly different set of infrastructure setup, and we had to alter our infrastructure design. The main additional components included an embedding model and a vector store. Essentially, you’d convert your prompt into a bunch of numbers and match that with the other bunches of numbers you already stored previously. The closest matching vectors were then returned as supporting context for the LLM to synthesise the output.
Press enter or click to view image in full size
Current LLM app architecture. Source: <https://github.blog/2023-10-30-the-architecture-of-todays-llm-applications/>
## Vector Store
We explored a variety of vector stores available, including FAISS, Pinecone, Weaviate, Chroma, Qdrant, and Opensearch. Eventually, we settled on a combination of Qdrant and Opensearch due to their ease of setup and open-source availability.
We do have to caveat here that we are not running massive datasets to be able to significantly observe the performance impact between the different products.
## Embedding Model
Initially, we used OpenAI’s text-ada-embedding-002 which provided improvements over the text-davinci-003. However, both models were not open-sourced and thus had the same problem of having data going through Azure OpenAI.
We moved towards using [thenlper’s gte-large](https://huggingface.co/thenlper/gte-large) which outperformed text-ada-embedding-002. This model was also open-source and allowed for on-premise deployments for higher classification workloads.
The embedding model was hosted with Gunicorn running FastAPI workers that uses the [sentence-transformers](https://github.com/UKPLab/sentence-transformers) library.
Being able to host the embedding ourselves marked a significant milestone in being fully capable of supporting all LLM / RAG applications within GCC, and even on-premise if necessary.
## No internet? No problem — TGI saves the day
As GCC can only support up to Confidential (Cloud Eligible) data, anything classified as Confidential and above was out of luck. This meant that some of the more impactful use cases would not be able to leverage LLMs since it was all on the cloud. We had to be able to host our own WOG LLM on-premise.
Fortunately, with the help of our friends from the Government Infrastructure Group (GIG), we managed to obtain an A100 server that was subsequently connected to our intranet on-premise.
Since we did not rely on any PaaS services like SageMaker or Vertex, and did not use proprietary CSP services, we were able to quickly replicate our cloud LLM hosting setup to this server and run a fully offline RAG application.
However, the performance was still largely dependent on transformers-bloom-inference and bloomchat-176B.
## Text Generation Inference (TGI) and Llama 2
In July 2023, HuggingFace released their [TGI](https://github.com/huggingface/text-generation-inference) server for model serving, and Meta released their free-for-commercial-use Llama 2 model family. Both represented a significant improvement over the previous setup, and we adopted these in our setups.
Press enter or click to view image in full size
Text Generation Inference architecture. Source: <https://github.com/huggingface/text-generation-inference>
TGI provided a large improvement in terms of speed and concurrent processing due to its internal queue and batching mechanism and its usage of gRPC for communication to the model shards. HuggingFace uses TGI for their own production environments.
For a quick setup, we could simply deploy a TGI docker container to run the desired model. More importantly, it does not have the restrictions that both Alpa and transformers-bloom-inference had which was limited to a specific model family. TGI can support (almost) all the models available on HuggingFace as it uses the transformers library under the hood.
Benchmarks show that TGI is capable of fully using the GPU capacity even during a single call, which gives it a significant speed advantage over the BLOOM setup.
## Current State
Our setup currently runs a customised TGI image hosting a Llama-2–70b-chat-hf together with a local Qdrant server and embedding service using gte-large, all served behind a [Chainlit](https://docs.chainlit.io/get-started/overview) frontend with FastAPI backend.
This setup is capable of handling up to Secret workloads and is fully isolated from the internet. We have another similar setup for handling Confidential workloads in the WOG intranet.
At several points along the journey, we also tried to create a stable production environment with the existing tech stack at the time. One of the biggest challenges was reducing costs and the ability to support multiple concurrent users / autoscaling.
Being able to run an equivalent on-premise service reduces a large part of the costs involved in hosting our own LLM, and running the serving libraries in containers allow for flexible autoscaling.
The journey has been a rather short but intense one involving many frameworks, models and techniques along the way. The dust is far from settled but the LLM landscape is at least moving towards a general architecture for LLM applications which is good for everyone involved.
We have also learnt much from the various challenges we encountered, some expected, mostly unforeseen, and are now capable of hosting our own production grade LLM environment regardless of whether it is cloud based. The setup is both cloud agnostic and model agnostic, which was the best outcome for us.
## What’s Next
The knowledge that we’ve gained so far is applicable to the WOG context and we’ll be able to help agencies scale their LLM applications or build central LLM services in collaboration with GIG that can serve all agencies. The WOG context here is especially important since it deals with building systems to serve Confidential and Secret workloads where we cannot rely on existing GCC or public cloud offerings.
Press enter or click to view image in full size
Credit: [Stable Diffusion XL](https://stablediffusionweb.com/#ai-image-generator)
The journey is far from over, as there are many aspects of the LLM hosting environment that we can improve on, not just on the literal hosting side of things. Some areas are listed below.
## Even more local hosting
With newer frameworks such as ollama, vLLM, exllama, and llama.cpp, we are exploring hosting LLMs not just locally, but with even lower resources. Being able to run LLMs without beefy GPU servers opens up opportunities for more “copilot” style LLMs that could benefit public officers requiring support in a hyperlocal context.
We were able to run a quantised Llama 2 33B Q4_K_M GGUF model entirely on CPU using llama.cpp on a laptop — albeit at a slower (but still respectable!) throughput of ~1 token / sec. This is a large improvement and shows how optimised C++ kernels make a big difference vs generic inference code. For reference, CPU based inference using the older libraries could take up to a few hours to generate a few hundred tokens.
In the near future, it could be entirely possible to run a quantised Llama 2 model within your GSIB laptop, which brings us to our next point.
## Quantisation
GPTQ, NF4, GGUF, AWQ, EETQ. Random sounding acronyms for various quantisation techniques which are essentially ways of compressing your LLM into a smaller LLM.
Quantisation is not lossless; a larger original model will always outperform its quantised counterpart. The goal is to strike a balance between size and “good enough”, or until such a point a lossless quantisation technique is developed.
We will continue to benchmark performance of quantised variants of the latest models (e.g. Llama 2, Mistral) to keep track of which models would fit our use cases best. And speaking of benchmarking…
## Benchmarking
To provide a better picture of model performance, we need to have an evaluation framework against which to benchmark the models.
For the large part of the LLM hosting journey, our core focus was model speed, i.e. throughput. However, with quantisation, throughput is no longer the main consideration, since a smaller model will always have a higher throughput than a larger one.
Therefore, we’ll also need to start looking at certain metrics such as perplexity, and perhaps also emerging toolchains like [Ragas](https://github.com/explodinggradients/ragas) for RAG evaluation using metrics such as faithfulness and precision recall.
## Even better throughput
TGI offers simplicity in LLM hosting. However, it may not be the most optimised in certain cases. Nvidia offers their own [Triton Inference Server](https://github.com/triton-inference-server/server) that, while not as user friendly, is supposedly capable of even better performance than TGI.
We will explore cases where Triton can be applied and perhaps switching out from TGI where applicable. Fun fact: OpenAI is powered by Triton.
## GovGPT and demand aggregation
With improvements in fine-tuning techniques (e.g. LoRA, PEFT) and concepts (e.g. foundation models with adapter layers), we could look into building a WOG-personified LLM to serve as a refined foundation LLM for our agencies’ use cases, as opposed to using only a generic pre-trained LLM.
We would also need to do a WOG-wide demand poll to better understand the needs of our agencies and to better design a training and serving pipeline or central service.
## TL;DR
Model Infrastructure Sizing — For standard half precision (bf16)
  * Storage required: Parameter size (in B) x 2
  * GPU RAM required: Parameter size (in B) x 2


e.g. Llama-2–70B will require ~140GB storage and GPU RAM
Pro-tip: This estimate does not include system overhead nor the context window requirements. For reference our Llama-2–70B-chat-hf uses ~272GB of GPU RAM with MAXED OUT context window and concurrency calls.
Inference Model Serving — [Text Generation Inference](https://github.com/huggingface/text-generation-inference)
## **Thank you for reading!**
Follow
## [Written by Ng Shangru](https://medium.com/@ng.shangru?source=post_page---post_author_info--0fbf3909311b-----------------------------------------)
SWE at GovTech
Follow
Follow
## [Published in AI Practice and Data Engineering Practice, GovTech](https://medium.com/dsaid-govtech?source=post_page---post_publication_info--0fbf3909311b-----------------------------------------)
[Last published Oct 1, 2026](https://medium.com/dsaid-govtech/strengthening-chatbot-safeguards-with-red-teaming-agents-806e0ef6354a?source=post_page---post_publication_info--0fbf3909311b-----------------------------------------)
AI Practice and Data Engineering Practice, Government Technology Office, GovTech
Follow
