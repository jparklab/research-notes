---
title: 'On-premise LLM and RAG for the enterprise: what €60k really buys you | Ask
  This Guy Blog'
id: on-premise-llm-and-rag-for-the-enterprise-what-60k-really-buys-you-ask-this-guy
tags:
- onprem-llm-inference-stacks-859099
- onprem-contrarian
- tco-break-even
- sovereignty
- vendor-adjacent
- reliability-medium
created: '2026-10-23T17:21:34.139343Z'
updated: '2026-10-23T17:25:20.949032Z'
source: https://www.askthisguy.com/en/blog/llm-local-enterprise-sovereignty-illusion/
source_domain: www.askthisguy.com
fetched_at: '2026-10-23T17:21:34.106299Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'RELIABILITY: vendor-adjacent secondary analysis (Ask This Guy, French enterprise-RAG/inference
  reseller with commercial interest in steering buyers to European providers; dated
  Feb 21 2026, updated Oct 15 2026) but unusually transparent arithmetic. Contrarian
  TCO: 2-GPU server for gpt-oss-120b ~EUR70-90k excl VAT + EUR32.5k/64.9k/104.3k per
  year (low/median/high; staff+depreciation = 77-83%, electricity 4-14%; 1.8 kW, PUE
  1.42, EUR0.20/kWh, colo EUR100-150/kW/mo, 10-12% maintenance, LLMOps engineer EUR120k
  loaded). Break-even vs SAME model via Scaleway API (EUR0.15/0.60 per M tokens, ~EUR0.20
  blended for RAG): 162B/324B/522B tokens/yr = 44%/88%/141% 24x7 utilisation; office-hours-saturated
  server = ~EUR18.6k API-equivalent/yr < depreciation. Server serves ~3,250 intensive
  RAG users vs ~11,000 needed. Argues ''9-18 month'' paybacks compare against closed
  frontier APIs (~10x pricier). Hardware dating (Oct 2026): H100 NVL last distributor
  delivery 15 Jun 2026; H200 NVL from EUR26.5k; RTX PRO 6000 96GB MSRP doubled 8,565->16,000
  USD (Aug 2026, DRAM shortage); B200 ~5x tokens of H100 at equal speed (InferenceX).
  Stack table: vLLM/SGLang/TensorRT-LLM; TGI archived Mar 2026; Ollama single request
  default; 7 components (embeddings, reranker, parsing/OCR, vector DB, gateway, observability)
  plus per-document ACLs. Thesis: sovereignty does not require on-premise (EEA-controlled
  provider gives same legal protection; CLOUD Act reaches US-parent providers); on-prem
  justified for air-gapped/contractual/24x7 batch volume. Note ironic limit: argument
  is for small-budget (~EUR60k) deployments, not large enterprises.'
---

AI
# On-premise LLM and RAG for the enterprise: what €60k really buys you
On-premise LLM in 2026: GPU prices, memory per model, full TCO and break-even against the same model via API. Figures to decide between Make and Buy.
February 21, 2026
Updated on October 15, 2026
12 min read
An **on-premise LLM** is a language model that runs on hardware the company owns or controls, inside its own network, with no third-party API in the path of the data. In practice, it means downloading an [**open-weight model**](https://www.askthisguy.com/en/blog/open-source-vs-open-weights-ai/), serving it on your GPUs and plugging your applications into it.
The promise is [**control**](https://www.askthisguy.com/en/blog/souverainete-numerique-dependance-americaine/): your data stays in-house, and you no longer depend on a provider. For enterprise AI projects (agents, RAG, internal assistants), this is attractive, sometimes essential.
But AI infrastructure is expensive, and the usual "Make or Buy" thinking works differently here. This article gives the October 2026 figures to decide with full knowledge of the facts: what hardware costs, which models fit on it, the full annual cost and, above all, the break-even point against **the same model** served through an API. For a broader view of every option, from buying GPUs to a turnkey provider, see our guide to [AI inference in the enterprise](https://www.askthisguy.com/en/blog/enterprise-ai-inference/).
## On-premise RAG: what people often confuse
Many teams frame the need as an **on-premise RAG** (or _on-prem RAG_): an assistant that answers from _your_ documents, hosted in-house. An on-premise RAG does not necessarily mean "rebuild everything and host the LLM yourself". Three layers can be separated:
  1. **Data and retrieval** : documents, access rights, index, citations. This is the business core of RAG.
  2. **The platform** : connectors, ingestion, evaluation, monitoring. This is what costs the most to industrialise.
  3. **LLM inference** : the model that generates the answer (sovereign cloud, European provider, or on-prem GPUs).


Many "on-prem" projects fail because they tackle point 3 first (buying H100s) when the real delays and the real TCO sit in points 1 and 2. A turnkey [enterprise RAG solution](https://www.askthisguy.com/en/solutions/knowledge-rag/) can already run with European hosting, with an on-premise option depending on your constraints, without you having to size the GPU stack alone.
## A few definitions to read the rest calmly
  * **LLM** (Large Language Model): the "brain" of the system. This is what you will deploy on your server.
  * **Number of parameters** : a measure of the model's "size". The higher this number, the more capable the model is at fine reasoning (and the more compute and memory it requires).
  * **Inference** : the moment when the model answers your request. We are not talking about training (which has already happened), but simply asking it a question and getting an answer.
  * **Token** : a small unit of text (a word or part of a word). When we say "100 tokens per second", it means the model produces about 100 of these units each second: the higher this number, the faster the response.
  * **RAG** (Retrieval-Augmented Generation): a [technique](https://www.askthisguy.com/en/blog/rag-tutorial/) that gives the model access to your documents (PDFs, internal wikis, etc.) so it can answer using _your_ data rather than its memory alone. Often the trigger for an enterprise AI project.
  * **Agentic AI** : an AI that does not just answer once; it chains steps (calling a tool, searching, reasoning, retrying) to accomplish a complex task, like an [assistant that takes initiative](https://www.askthisguy.com/en/blog/how-mcp-accelerate-agentic-ai/).


Agentic AI: chaining steps and tool calls
## Not one model but a whole stack to run
An enterprise AI project, as soon as it gets a bit advanced, does not rely on **one** model but on **several** models running in parallel: a main LLM, lightweight models for simple tasks (routing, extraction, naming), an [embeddings model](https://www.askthisguy.com/en/blog/embedding-strategy/) for semantic search, a vision model if you analyse images or scanned PDFs. You will probably start with 1 or 2 models and end up with 5 or 6.
Around those models, an on-premise RAG that works "like a cloud service" needs at least seven components to operate:  
| Component  | Open-source options you can self-host  | Watch out for  |  
| --- | --- | --- |  
| Inference engine  |  [vLLM](https://github.com/vllm-project/vllm), [SGLang](https://github.com/sgl-project/sglang), TensorRT-LLM  | Hugging Face TGI was [archived in March 2026](https://github.com/huggingface/text-generation-inference); Ollama handles [one request at a time by default](https://docs.ollama.com/faq)  |  
| Embeddings  | Qwen3-Embedding, BGE-M3, multilingual-e5  | some models (jina-embeddings-v5) forbid commercial use  |  
| Reranker  | bge-reranker-v2-m3, Qwen3-Reranker  | see how [reranking](https://www.askthisguy.com/en/blog/jev-rag-reranking/) sorts chunks before the LLM  |  
| Parsing and OCR  | Docling, PaddleOCR, olmOCR  | parsing weighs heavily on answer quality ([our OCR comparison](https://www.askthisguy.com/en/blog/mistral-ocr-paddleocr-cost/))  |  
| Vector database  | Qdrant, Milvus, Weaviate, pgvector  | pgvector is often enough to start  |  
| Gateway and quotas  | SSO and audit logs are in the paid tier  |  
| Observability  | Prometheus and Grafana (native in vLLM), Langfuse  | traces of prompts contain your data  |  
To which you must add **access control per document** : each chunk carries the rights of its source document (SharePoint, document management system), and the search filters on them before anything reaches the LLM. This is what separates an Ollama demo from a production service, and it is what readers most often underestimate.
On the engine side, the gap between a desktop tool and a server is not marginal: in a [Red Hat benchmark](https://developers.redhat.com/articles/2025/08/08/ollama-vs-vllm-deep-dive-performance-benchmarking), vLLM peaks at 793 tokens/s against 41 for Ollama under load, on the same GPU and the same model.
## Which open-weight models can you run on-premise in 2026?
The model sets the hardware. Here are the main open-weight models an enterprise can deploy in October 2026, with the size of their weights on Hugging Face:  
| Model  | Parameters (total / active)  | Weights  | Licence  | Hardware indicated  |  
| --- | --- | --- | --- | --- |  
|  [gpt-oss-20b](https://huggingface.co/openai/gpt-oss-20b) (OpenAI)  | 21B / 3.6B  | 13.8 GB  | Apache 2.0  | runs within 16 GB of memory  |  
| 24B dense  | 48 GB  | Apache 2.0  | about 55 GB of GPU memory  |  
|  [Gemma 4 31B](https://huggingface.co/google/gemma-4-31B-it) (Google)  | 30.7B dense  | 62.5 GB  | Apache 2.0  | one 80 to 96 GB GPU  |  
|  [gpt-oss-120b](https://huggingface.co/openai/gpt-oss-120b) (OpenAI)  | 117B / 5.1B  | 65.2 GB  | Apache 2.0  | one 80 GB GPU, two for several users  |  
| 119B / 6.5B  | 121 GB (FP8)  | Apache 2.0  | two GPUs  |  
|  [Nemotron 3 Super](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Super-120B-A12B-NVFP4) (NVIDIA)  | 120B / 12B  | 128 GB (FP8)  | NVIDIA Open Model License  | 2× H100 in FP8  |  
| 284B / 13B  | about 160 GB  | MIT  | 8 GPUs (vLLM recipe on 8× RTX PRO 6000)  |  
| 675B / 41B  | 682 GB (FP8)  | Apache 2.0  | an 8× H200 or B200 node  |  
Two points to keep in mind:
  * **Claude, GPT-5 and the latest Gemini models cannot be downloaded.** You can only use them in their publisher's cloud or a hyperscaler's. OpenAI's open-weight models (gpt-oss), on the other hand, install freely.
  * **Open models that rival the best closed models** (300 billion to over a trillion parameters) require at least an 8-GPU server. The "€60k" tier gives access to the gpt-oss-120b class, not beyond.


### When "small" models are enough
When small models are enough
Several use cases are fine with lighter models: **simple, precise** tasks (naming a conversation, extracting a field, rephrasing a sentence), **simple retrieval** , RAG with factual queries and little logical chaining, tasks with **fine-tuning** on a narrow domain.
Models of 20 to 30 billion parameters (**gpt-oss-20b** , **Mistral Small 3.2** , **Gemma 4**) can be enough, if you accept some hallucinations and the fact that the model "won't reason" much. The upside: light infrastructure, which makes the _Make_ scenario accessible.
> **In practice** : this scenario is rare, and usually only a pilot before a broader project.
### When medium or large models are essential
As soon as complexity increases, **a model of about 100 billion parameters becomes the minimum**. _gpt-oss-120b_ is often deployed, both for its publisher's reputation and for its solid performance for its size. Larger models (Mistral Large 3, DeepSeek, GLM, etc.) are recommended for complex queries with deep reasoning, multi-step agents with tool calls, multi-document analysis and tasks that require a fine understanding of context.
> **In short** : as soon as you target advanced agents, advanced RAG or demanding business reasoning, large models become hard to replace without significantly degrading the experience.
## How much GPU memory do you need?
According to NVIDIA, the rule fits in one line: [**memory = model weights + KV cache + a margin**](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/). The weights depend on the number of parameters and their format (2 bytes in BF16, 1 in FP8, about 0.5 in 4 bits). The KV cache is the model's working memory for each conversation: it grows with the length of the context and the number of simultaneous users.
Worked example with _gpt-oss-120b_ (calculation from its [published configuration](https://huggingface.co/openai/gpt-oss-120b/raw/main/config.json)), for 32 users at the same time, each with 32,000 tokens of context (a typical RAG):  
| Item  | Memory  |  
| --- | --- |  
| Model weights  | 65.2 GB  |  
| KV cache, 32 users × 32K tokens  | 38.7 GB  |  
| **Total, before margin**  | **about 104 GB**  |  
OpenAI is right to say that the model "fits into a single 80GB GPU". But on an 80 GB H100, about 10 GB remain for the KV cache, i.e. **roughly 7 long conversations at a time**. For thirty users in RAG, you need two H100s, or one GPU with 141 GB (H200) or more.  
| Hardware tier  | GPU memory  | What fits, with room for users  |  
| --- | --- | --- |  
| 1 GPU of 24 to 32 GB  | 24 to 32 GB  | gpt-oss-20b, Gemma 4 or Mistral Small in 4 bits, few users  |  
| 1 GPU of 80 to 96 GB (H100, RTX PRO 6000)  | 80 to 96 GB  | gpt-oss-120b with few long sessions; Gemma 4 31B  |  
| 2 GPUs  | 160 to 192 GB  | gpt-oss-120b for a few dozen users; Mistral Small 4  |  
| 8 GPUs (HGX H200, B200)  | 768 GB to 1.5 TB  | DeepSeek V4 Flash, Mistral Large 3  |  
## Users are demanding: speed per user vs total throughput
Users have become impatient, used to consumer services like ChatGPT that answer almost instantly. Speed often decides whether they accept or reject the tool. Below **100 tokens generated per second** , it is likely to feel slow. If the AI is **agentic** , calls several tools and "reasons", you should aim for **500 tokens/second** or more.
> **Reference thresholds:**
>   * **~500 tokens/s** : multi-step projects, tool calls, reasoning, complex agents.
>   * **~100 tokens/s** : simple tasks without reflection (basic chat, completion).
> 

An on-premise server has **two** limits that cannot be maximised at the same time: the speed each user feels, and the total volume it produces. The published benchmarks show it:
  * On 2× RTX PRO 6000, according to [Millstone AI's measurements](https://www.millstoneai.com/inference-benchmark/gpt-oss-120b-mxfp4-2x-rtx-pro-6000-blackwell), _gpt-oss-120b_ goes from **230 tokens/s** for a single user with a short context to **14.5 tokens/s per user** with 6 users and long contexts. The wait before the first word goes from 50 ms to **18 seconds** at 128,000 tokens of context.
  * On 2× H100 tuned for maximum throughput, according to [GPUStack's performance tests](https://docs.gpustack.ai/latest/performance-lab/gpt-oss-120b/h100/), the same model reaches **16,042 tokens/s in total** (input and output, of which 7,649 in output), but each user then receives about **12 tokens/s** , after 4.6 seconds of waiting.


The "16,000 tokens/s" in vendor brochures are users served at 12 tokens/s. The "200 tokens/s" are a single user.
For comparison, [Cerebras](https://inference-docs.cerebras.ai/models/overview) advertises about **3,000 tokens/s** on the same model. At ATG we observe **1,000 to 2,000 tokens/s** : 4 to 13 times faster than the on-premise server with a single user, and far more under load.
Users are also used to **many additional services** , such as attaching a PDF to a message and having it processed immediately. Cloud services excel at this. On-premise, you can offer it by converting the document to images for a vision model, but it will be slower and often limited (number of pages, for example).
## Hardware prices in 2026: the H100 is gone, memory is soaring
The "2× H100 ≈ €60k" order of magnitude still holds for the cards alone, but the landscape has changed:
  * **The H100 is no longer sold new.** According to the reseller nelpx, the [H100 NVL's last distributor delivery](https://www.nelpx.de/nvidia-h100-kaufen) was on June 15, 2026; other resellers, such as Prostor, still list it [at about **€28,446 excluding VAT** per card](https://shop.prostor.de/de/pny-nvidia-h100-nvl-hbm3-gpu-rechenprozessor-94gb.html). Its natural replacement, the **H200 NVL** (141 GB), costs the same: from **€26,500** excluding VAT, with 50% more memory.
  * **The "affordable" card has doubled in price.** According to Tom's Hardware, NVIDIA's RTX PRO 6000 (96 GB) went from $8,565 in March 2025 [to **$16,000** in August 2026](https://www.tomshardware.com/pc-components/gpus/nvidia-doubles-rtx-pro-6000-blackwells-msrp-to-a-staggering-usd16-000-96gb-card-started-pre-orders-below-usd8-000-last-year), driven by the memory shortage: TrendForce reports that server DRAM [rose 13 to 18% in a single quarter](https://www.trendforce.com/presscenter/news/20260709-13140.html) in Q3 2026.
  * **A complete server costs more than its cards.** Chassis, CPUs, RAM, storage and network: count **€70,000 to €90,000 excluding VAT** for a 2-GPU server (our estimate). For reference, a server with 3× RTX PRO 6000 is listed at €89,562 excluding VAT at [LDLC Pro](https://www.ldlc.pro/fiche/PB00691383.html), [a Lenovo with 2× RTX PRO 6000 at $68,011](https://lenovopress.lenovo.com/lp2368-on-premise-vs-cloud-generative-ai-total-cost-of-ownership-2026-edition) according to Lenovo Press.
  * **Obsolescence is measurable.** According to SemiAnalysis's InferenceX benchmark, at equal speed per user, a B200 [produces **5 times more** tokens than an H100](https://inferencex.semianalysis.com/run/gptoss-120b-on-b200) on gpt-oss-120b. The GPUs bought today will be two generations behind before they are paid off.


Renting is the other benchmark. An H100 costs **€2.80 per hour at[OVHcloud](https://www.ovhcloud.com/fr/public-cloud/prices/)** and €2.87 at [Scaleway](https://www.scaleway.com/en/pricing/gpu/), against [$6.88 at AWS and $12.29 at Azure](https://instances.vantage.sh/aws/ec2/p5.48xlarge), according to the Vantage price comparison site. Two H100s rented 24/7 at OVHcloud come to about **€49,000 per year** : a 2-GPU server costs the equivalent of a year and a half of continuous rental, before power, hosting, maintenance and staff.
## Total cost of ownership: the purchase price is only one line
Our estimate for a 2-GPU server bought **€80,000 excluding VAT** , sized for _gpt-oss-120b_. Assumptions: 1.8 kW drawn, data centre efficiency (PUE) of [1.42 on average in France](https://www.arcep.fr/actualites/actualites-et-communiques/detail/n/environnement-eapns-210526.html) according to Arcep, electricity at €0.20 per kWh (French SMEs, 2026), colocation at €100 to €150 per kW per month, maintenance at 10 to 12% per year, LLMOps engineer at €120,000 per year fully loaded (about [€85,000 gross](https://siecledigital.fr/2026/07/08/voici-les-metiers-du-numerique-les-mieux-payes-en-2026/)). The NVIDIA AI Enterprise licence ($4,500 per GPU per year) is [included for five years](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html) with H100 NVL and H200 NVL cards.  
| € per year  | Low (hardware only)  | Median (¼ of an engineer)  | High (½ engineer, 3-year depreciation)  |  
| --- | --- | --- | --- |  
| Depreciation (4 years)  | 20,000  | 20,000  | 26,667  |  
| Electricity  | 4,478  | 4,478  | 4,478  |  
| Hosting  | 0  | 2,400  | 3,600  |  
| Maintenance  | 8,000  | 8,000  | 9,600  |  
| Staff  | 0  | 30,000  | 60,000  |  
| **Total per year**  | **≈ 32,500**  | **≈ 64,900**  | **≈ 104,300**  |  
Electricity is not the issue: 4 to 14% of the total. **Staff and depreciation account for 77 to 83% of the cost** in the median and high scenarios. And this table includes neither the rest of the RAG stack, nor a second machine for redundancy, nor the project costs (integration, security, testing).
## Break-even: on-premise vs the same model via API
The fair comparison is with **the same model** served through an API, not with a more capable closed model. _gpt-oss-120b_ costs **€0.15 per million input tokens and €0.60 per million output tokens** at [Scaleway](https://www.scaleway.com/en/pricing/model-as-a-service/), a French provider. For a RAG (about 8 input tokens per output token), that is **€0.20 per million tokens** on average.
On the capacity side, at 100 tokens/s per user, an H100 produces at best [5,863 tokens/s on this model](https://inferencex.semianalysis.com/run/gptoss-120b-on-h100) according to InferenceX (measured on highly optimised nodes, a favourable assumption for on-premise). A 2-GPU server therefore delivers at most **370 billion tokens per year running 24/7** , and 93 billion during office hours.  
| TCO scenario  | Annual cost  | Break-even (tokens per year)  | 24/7 utilisation required  |  
| --- | --- | --- | --- |  
| Low (hardware only)  | €32,500  | 162 billion  | 44%  |  
| Median (¼ engineer)  | €64,900  | 324 billion  | 88%  |  
| High (½ engineer)  | €104,300  | 522 billion  | 141% (impossible)  |  
How to read it:
  1. **At equal model, the server only beats the API if it runs day and night** , at more than 44% of its capacity without counting staff, 88% with a quarter of an engineer. Saturated from 8 am to 6 pm and idle at night, it produces the equivalent of **€18,600 of API usage per year** : less than its depreciation alone.
  2. In users, the median threshold represents **about 11,000 intensive users** of a RAG assistant (20 requests a day). The server, saturated during office hours, serves about 3,250 at best.
  3. The threshold only becomes reachable with **bulk background processing** (classifying or extracting from documents, indexing, batch agents) that fills nights and weekends.
  4. **Why other analyses find "9 to 18 months"** : they compare with a closed API. Against a GPT-5-class model, about ten times more expensive, the median threshold drops to 34 billion tokens per year. But you are then comparing two models of different quality. The [published studies](https://arxiv.org/html/2509.18101v3) either count electricity only or compare with on-demand GPU rental at a US hyperscaler.


And the threshold moves the wrong way for on-premise: the price of inference at equal performance is falling by about [10× per year](https://www.a16z.com/llmflation-llm-inference-cost), while the hardware you buy stays at its 2026 cost.
> **Bottom line** : on cost alone, at equal model, _Buy_ wins in almost every interactive use case. On-premise pays off with huge volumes processed around the clock, or when a constraint other than cost requires it.
## Five options between "all on-premise" and "public API"
The choice is not binary. From the most controlled to the most delegated:  
| Option  | Example  | Order of price  | Sovereignty  |  
| --- | --- | --- | --- |  
| 1. On-premise, in your walls or in colocation  | 2 to 8-GPU server  | €70k to €90k + €32k to €104k per year  | depends only on you  |  
| 2. "AI in a box" appliance  | HPE Private Cloud AI, Dell AI Factory, NVIDIA DGX  | several hundred thousand euros  | hardware at your site, US or Chinese manufacturer for support  |  
| 3. Dedicated GPUs at a European host  | Scaleway Managed Inference, OVHcloud  | €6.68 per hour for 2× H100 at Scaleway  | yes, if the host is controlled from the EEA  |  
| 4. API from a European provider  | Scaleway, OVHcloud AI Endpoints  | €0.15 / €0.60 per million tokens for gpt-oss-120b  | yes, under the same condition  |  
| 5. Model in a US hyperscaler's private network  | AWS Bedrock, Azure OpenAI, Gemini on Google Distributed Cloud  | pay per use  | no: the parent company is subject to the CLOUD Act  |  
Option 3 is the most honest comparison with buying: same software, same model, same isolation, but no capex and no obsolescence. Two dedicated H100s at Scaleway cost about €58,500 per year running 24/7, or about €14,700 if you only use them during office hours.
## Sovereignty: what on-premise solves, and what it does not
On-premise guarantees that only you can access the data in clear. But **sovereignty does not require on-premise** : an open-weight model served by a European provider controlled from the EEA offers the same legal protection, without capex. Our rule is strict: a provider whose parent company is outside the EEA fails the sovereignty criterion, even with a European subsidiary and data centres in Europe, because the [CLOUD Act](https://www.askthisguy.com/en/blog/souverainete-numerique-dependance-americaine/) applies wherever the data is stored.
Conversely, an open-weight model hosted at home does not "phone home": the nationality of its publisher (OpenAI for gpt-oss, Alibaba for Qwen, DeepSeek) does not expose your data. It weighs on continuity (no guaranteed updates) and on the model's biases.
On-premise remains justified for **air-gapped** systems (defence, industrial secrets), a customer's contractual constraints, or a policy that forbids any subcontracting. That is the illusion this article's title refers to: believing that only on-premise is sovereign.
## Decision checklist
On-premise is worth studying if you tick most of these boxes:
  * an obligation to run **disconnected** from the internet, or a contract that forbids any third-party host;
  * a volume that fills the machine **day and night** , thanks to bulk processing, not just office-hours chat;
  * a **team** able to operate GPUs, an inference engine and a RAG stack, with at least two people for continuity;
  * use cases that a **120-billion-parameter-class** model handles well, or a budget for an 8-GPU server;
  * a budget of **€70k to €90k** up front, plus **€30k to €70k per year** to run it;
  * use cases and volumes already **validated** with a provider.


If you tick fewer than three, start with a European provider.
## Frequently asked questions about on-premise LLMs
### What is an on-premise LLM?
It is a language model that runs on hardware the company owns or controls, in its own network, with no third-party API in the path of the data. In practice, it is an open-weight model (gpt-oss, Mistral, Gemma, Qwen, DeepSeek) served by an inference engine such as vLLM on the company's own GPUs, in its data centre or in colocation.
### How much does an on-premise LLM cost?
For a model like gpt-oss-120b, count about €60,000 for two GPUs and €70,000 to €90,000 excluding VAT for the complete server. Running it costs €32,000 to €104,000 per year depending on the staff time dedicated to it, with depreciation and staff accounting for most of the cost, far ahead of electricity.
### When is an on-premise LLM cheaper than an API?
At equal model, a 2-GPU server for gpt-oss-120b beats the API price of a European provider beyond 160 to 520 billion tokens per year, which means running at 44 to 88% of its capacity around the clock. Interactive office-hours use never gets there; bulk background processing can.
### How much GPU memory does an LLM need?
Add up the model weights (parameters × bytes per parameter), the KV cache (which grows with context length and the number of simultaneous users) and a safety margin. gpt-oss-120b weighs 65 GB, but serving 32 users with 32,000 tokens of context each requires about 104 GB, i.e. two 80 GB GPUs.
### Should you use vLLM or Ollama in production?
Ollama is designed for a workstation: it handles one request at a time by default. For a service shared by several users, an engine such as vLLM or SGLang is required: it batches requests together and delivers many times the throughput on the same GPU.
### What do you need for an on-premise RAG?
Besides the LLM: an embeddings model, a reranker, a parsing and OCR tool for documents, a vector database, a gateway with authentication and quotas, observability, and access control per document so that each user only retrieves what they are allowed to read. That makes at least seven components to operate.
### Is on-premise required for GDPR compliance or sovereignty?
No. A provider controlled from the European Economic Area, which serves open-weight models in Europe, offers the same legal protection without buying hardware. On-premise remains useful when systems must be disconnected from the internet or when a contract forbids any third-party host.
### Can you install Claude, GPT-5 or Gemini on-premise?
Not Claude or GPT-5: their weights cannot be downloaded, and they are only available in their publisher's cloud or a hyperscaler's. Gemini exists on Google Distributed Cloud, including disconnected, but the publisher remains American. OpenAI's open-weight models, gpt-oss-20b and gpt-oss-120b, can be installed freely.
### Is a local LLM as good as ChatGPT?
Not at equal hardware budget. Open models of the 120-billion-parameter class, which run on a 2-GPU server, remain behind the best closed models on complex reasoning. Open models that come close (300 billion to over a trillion parameters) require at least an 8-GPU server.
## Conclusion
**On-premise** makes sense for well-defined use cases with strict constraints (disconnected systems, contractual obligations) or with very large volumes processed around the clock. For an ambitious project with agents and complex reasoning, _Buy_ usually wins on speed, simplicity and, at equal model, on cost.
**On-premise is often a conviction-driven choice.** Nothing wrong with that. But it is easier to implement once you have learned what your project really needs.
Our recommendation at **Ask This Guy** : **start with a European provider** to gain maturity and validate your use cases and volumes, then consider on-premise only if volume and specific requirements justify it. If your priority is an operational **on-premise RAG** without rebuilding the whole stack, first look at our [enterprise RAG solution](https://www.askthisguy.com/en/solutions/knowledge-rag/) (European hosting, on-prem option).
And if your main motivation is sovereignty rather than cost, start by reading what the word actually covers: [sovereign AI, definition and criteria](https://www.askthisguy.com/en/sovereign-ai/). Hosting the model yourself is only one of the three layers at stake, and rarely the most urgent.
### Building your AI skills
Choosing between on-premise and an inference provider, sizing GPUs, controlling the cost of a model in production: these trade-offs can be learned. For professionals who want to [train in MLOps and LLMOps](https://www.jedha.co/formations/formation-mlops), Jedha offers a 150-hour programme dedicated to industrialising AI: distributed infrastructure (GPUs, Kubernetes), data governance, GDPR and AI Act compliance, and cost control. To date, it is the only training in France that covers both. It is aimed at people who already have two years of experience in data or AI.
_Want to deploy a high-performance AI assistant without managing infrastructure?[Book a 20-minute demo](https://www.askthisguy.com/en/demo/), we'll show you how._
Tags:[LLM](https://www.askthisguy.com/en/blog/tag/llm/)[Sovereignty](https://www.askthisguy.com/en/blog/tag/souverainete/)[GDPR](https://www.askthisguy.com/en/blog/tag/rgpd/)[RAG](https://www.askthisguy.com/en/blog/tag/rag/)[AI Accelerator](https://www.askthisguy.com/en/solutions/ai-custom/)
Partager :LinkedInXCopier le lien
## Related Articles
Digital Sovereignty
### Best French AI for enterprises in 2026: 14 vendors put to the sovereignty test
Fourteen French enterprise AI vendors checked against public sources: who owns the capital, who hosts your data, who runs inference.
August 23, 2026
12 min
Digital Sovereignty
### What if the United States Turned Off Your Digital Tap?
How dependence on U.S. infrastructure exposes Europe to growing strategic risks - and what to do to protect yourself.
December 10, 2025
3 min
Digital Sovereignty
### OVHcloud's AI strategy: what Octave Klaba's plan is worth
OVHcloud is announcing its own LLM and wants the whole AI stack. What already exists (GPUs, AI Endpoints), what is still unproven, what IT leaders can learn.
October 20, 2026
11 min
OVHcloud[Sovereignty](https://www.askthisguy.com/en/blog/tag/souverainete/)AI Inference
## Interested in our solutions?
Discover how Ask This Guy can help you accelerate with AI
[Book a Demo](https://www.askthisguy.com/en/demo/)
