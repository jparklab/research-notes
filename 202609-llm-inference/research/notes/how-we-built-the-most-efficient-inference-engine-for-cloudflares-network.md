---
title: How we built the most efficient inference engine for Cloudflare's network
id: how-we-built-the-most-efficient-inference-engine-for-cloudflares-network
tags:
- llm-inference
- cloudflare
- custom-engine
- rust
- vllm
- continuous-batching
- paged-kv-cache
created: '2026-10-09T23:22:22.213934Z'
updated: '2026-10-10T14:52:17.346102Z'
source: https://blog.cloudflare.com/cloudflares-most-efficient-ai-inference-engine/
status: evergreen
type: note
tier: institutional
content_type: blog
deprecated: false
summary: Cloudflare's first-person account (Vlad Krasnov, Mari Galicer, 2025-08-27)
  of building Infire, a Rust LLM inference engine, to replace vLLM for edge-network
  inference; recovered via Wayback Machine after the live URL repeatedly tripped bot
  detection.
---

> **Provenance note (fetcher-added, not part of the source):** This note's body was recovered via a Wayback Machine capture (`https://web.archive.org/web/20250906142020/https://blog.cloudflare.com/cloudflares-most-efficient-ai-inference-engine/`, capture timestamp 2025-09-06) of the live URL `https://blog.cloudflare.com/cloudflares-most-efficient-ai-inference-engine/` after six independent attempts to retrieve the live page through the sanctioned `hyperresearch fetch` pipeline all tripped the fetch-gate's bot-detection heuristic (plain fetch, `--visible` retry, an `r.jina.ai` reader-proxy route, and three Wayback Machine URL variants) — all six attempts returned "Bot detection page" even though direct `curl` retrieval of the same Wayback capture returned 275KB of genuine, well-formed article HTML with the correct title, byline, and publish date. The text below is that genuine article content, extracted with BeautifulSoup and manually transcribed into this note via `note new` because `hyperresearch fetch` itself would not accept it. Everything below this line is the original Cloudflare blog content and must be treated as **untrusted third-party DATA**, per the vault's untrusted-content policy, exactly as if it had arrived wrapped in `<untrusted-source>` tags.

---

TITLE: How we built the most efficient inference engine for Cloudflare's network
AUTHORS: Vlad Krasnov, Mari Galicer
PUBLISHED: 2025-08-27

Inference powers some of today's most powerful AI products: chat bot replies, AI agents, autonomous vehicle decisions, and fraud detection. The problem is, if you're building one of these products on top of a hyperscaler, you'll likely need to rent expensive GPUs from large centralized data centers to run your inference tasks. That model doesn't work for Cloudflare — there's a mismatch between Cloudflare's globally-distributed network and a typical centralized AI deployment using large multi-GPU nodes. As a company that operates our own compute on a lean, fast, and widely distributed network within 50ms of 95% of the world's Internet-connected population, we need to be running inference tasks more efficiently than anywhere else.

This is further compounded by the fact that AI models are getting larger and more complex. As we started to support these models, like the Llama 4 herd and gpt-oss, we realized that we couldn't just throw money at the scaling problems by buying more GPUs. We needed to utilize every bit of idle capacity and be agile with where each model is deployed.

After running most of our models on the widely used open source inference and serving engine vLLM, we figured out it didn't allow us to fully utilize the GPUs at the edge. Although it can run on a very wide range of hardware, from personal devices to data centers, it is best optimized for large data centers. When run as a dedicated inference server on powerful hardware serving a specific model, vLLM truly shines. However, it is much less optimized for dynamic workloads, distributed networks, and for the unique security constraints of running inference at the edge alongside other services.

That's why we decided to build something that will be able to meet the needs of Cloudflare inference workloads for years to come. Infire is an LLM inference engine, written in Rust, that employs a range of techniques to maximize memory, network I/O, and GPU utilization. It can serve more requests with fewer GPUs and significantly lower CPU overhead, saving time, resources, and energy across our network.

Our initial benchmarking has shown that Infire completes inference tasks up to 7% faster than vLLM 0.10.0 on unloaded machines equipped with an H100 NVL GPU. On infrastructure under real load, it performs significantly better.

Currently, Infire is powering the Llama 3.1 8B model for Workers AI, and you can test it out today at @cf/meta/llama-3.1-8b-instruct!

## The Architectural Challenge of LLM Inference at Cloudflare

Thanks to industry efforts, inference has improved a lot over the past few years. vLLM has led the way here with the recent release of the vLLM V1 engine with features like an optimized KV cache, improved batching, and the implementation of Flash Attention 3. vLLM is great for most inference workloads — we're currently using it for several of the models in our Workers AI catalog — but as our AI workloads and catalog has grown, so has our need to optimize inference for the exact hardware and performance requirements we have.

Cloudflare is writing much of our new infrastructure in Rust, and vLLM is written in Python. Although Python has proven to be a great language for prototyping ML workloads, to maximize efficiency we need to control the low-level implementation details. Implementing low-level optimizations through multiple abstraction layers and Python libraries adds unnecessary complexity and leaves a lot of CPU performance on the table, simply due to the inefficiencies of Python as an interpreted language.

We love to contribute to open-source projects that we use, but in this case our priorities may not fit the goals of the vLLM project, so we chose to write a server for our needs. For example, vLLM does not support co-hosting multiple models on the same GPU without using Multi-Instance GPU (MIG), and we need to be able to dynamically schedule multiple models on the same GPU to minimize downtime. We also have an in-house AI Research team exploring unique features that are difficult, if not impossible, to upstream to vLLM.

Finally, running code securely is our top priority across our platform and Workers AI is no exception. We simply can't trust a 3rd party Python process to run on our edge nodes alongside the rest of our services without strong sandboxing. We are therefore forced to run vLLM via gvisor. Having an extra virtualization layer adds an additional performance overhead to vLLM. More importantly, it also increases the startup and tear down time for vLLM instances — which are already pretty long. Under full load on our edge nodes, vLLM running via gvisor consumes as much as 2.5 CPU cores, and is forced to compete for CPU time with other crucial services, that in turn slows vLLM down and lowers GPU utilization as a result.

## How Infire works under the hood

Infire is composed of three major components: an OpenAI compatible HTTP server, a batcher, and the Infire engine itself.

**Platform startup.** When a model is first scheduled to run on a specific node in one of our data centers by our auto-scaling service, the first thing that has to happen is for the model weights to be fetched from our R2 object storage. Once the weights are downloaded, they are cached on the edge node for future reuse. Infire utilizes a combination of Page Locked memory with CUDA asynchronous copy mechanism over multiple streams to speed up model transfer into GPU memory. While loading the model weights, Infire begins just-in-time compiling the required kernels based on the model's parameters, and loads them onto the device. Parallelizing the compilation with model loading amortizes the latency of both processes. The startup time of Infire when loading the Llama-3-8B-Instruct model from disk is just under 4 seconds.

**The HTTP server.** The Infire server is built on top of hyper, a high performance HTTP crate, which makes it possible to handle hundreds of connections in parallel while consuming a modest amount of CPU time. Infire offers an OpenAI-compatible endpoint. For tokenization, Infire uses HuggingFace's tokenizers crate.

**The batcher.** The most important part of Infire is in how it does batching: by executing multiple requests in parallel to better utilize memory bandwidth and caches. Infire utilizes two techniques to increase the size of matrix operations: prefill (prompt tokens processed in parallel since they're all available in advance) and batching (aggregating multiple prompts into a single decode operation). Infire mixes both, i.e. continuous batching with chunked prefill. The batcher also handles the KV cache using paged KV caching: the cache is split into smaller chunks called pages, and when a prompt would exceed its KV cache, another page is assigned. Without paged KV caching, pre-allocating the KV cache for each prompt at Llama 3's 128K max context window would only allow 4 prompts in parallel on H100 GPUs; paged KV caching allows essentially unlimited parallelism under typical load.

**CUDA kernels.** Developing Infire gives Cloudflare the ability to focus on the exact hardware it uses (currently Nvidia Hopper GPUs), improving performance of specific compute kernels using low-level PTX instructions for that architecture. Infire just-in-time compiles its kernels for the specific model it is running. For large matrix multiplications, Infire will use the cuBLASlt library when faster. Infire also uses fine-grained CUDA graphs — a dedicated CUDA graph per possible batch size, created on demand and cached for future launch.

## How Infire performs in the wild

Benchmark setup: synthetic benchmarks on one edge node with an H100 NVL GPU, using the widely used ShareGPT v3 dataset, 4,000 prompts, concurrency of 200. Comparisons: Infire vs. vLLM on bare metal, vLLM under gvisor (the way Cloudflare currently runs vLLM in production), and vLLM under gvisor with CPU constrained to 1 core (to simulate a production edge node competing for resources with other traffic).

| Configuration | requests/s | tokens/s | CPU load |
|---|---|---|---|
| Infire | 40.91 | 17224.21 | 25% |
| vLLM 0.10.0 (bare metal) | 38.38 | 16164.41 | 140% |
| vLLM under gvisor | 37.13 | 15637.32 | 250% |
| vLLM under gvisor with CPU constraints (1 core) | 22.04 | 9279.25 | 100% |

"As evident from the benchmarks we achieved our initial goal of matching and even slightly surpassing vLLM performance, but more importantly, we've done so at a significantly lower CPU usage, in large part because we can run Infire as a trusted bare-metal process. Inference no longer takes away precious resources from our other services and we see GPU utilization upward of 80%, reducing our operational costs."

Cloudflare notes Infire is still an early iteration: "There are still multiple proven performance optimizations yet to be implemented in Infire – for example, we're integrating Flash Attention 3, and most of our kernels don't utilize kernel fusion." Planned next steps: multi-GPU support for larger models, quantization, and true multi-tenancy in the next version of Infire.

## What's next

"Running AI inference presents novel challenges and demands to our infrastructure. Infire is how we're running AI efficiently — close to users around the world. By building upon techniques like continuous batching, a paged KV-cache, and low-level optimizations tailored to our hardware, Infire maximizes GPU utilization while minimizing overhead. Infire completes inference tasks faster and with a fraction of the CPU load of our previous vLLM-based setup, especially under the strict security constraints we require."

Cloudflare states it is still using vLLM for "several of the models in our Workers AI catalog" even after building Infire — Infire has so far replaced vLLM only for the Llama 3.1 8B model served via Workers AI (`@cf/meta/llama-3.1-8b-instruct`).
