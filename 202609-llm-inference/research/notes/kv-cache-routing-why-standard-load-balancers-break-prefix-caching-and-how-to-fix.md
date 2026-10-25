---
title: kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it
id: kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix
tags:
- llm-d
- kv-cache-routing
- hotspot-mitigation
- load-balancing
- benchmark-numbers
- endpoint-picker
- llmd-kv-cache-routing-c665d5
- derivative-of
created: '2026-09-14T11:54:25.579625Z'
updated: '2026-10-10T14:52:17.309210Z'
source: https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it
source_domain: www.truefoundry.com
fetched_at: '2026-09-14T11:54:25.560344Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'TrueFoundry vendor blog (June 2026) explains why standard round-robin/session-affinity
  load balancers destroy prefix-cache locality in multi-replica vLLM/SGLang deployments,
  and cites production llm-d benchmark numbers as evidence: on 8 pods / 16 H100 GPUs,
  llm-d''s prefix-cache-aware routing measured 57x faster time-to-first-token and
  2x throughput versus round-robin on identical hardware; a separate Llama-3.1-70B
  benchmark on 4x AMD MI300X GPUs showed 3x output tokens/sec and 2x TTFT reduction
  after switching to cache-aware routing; DigitalOcean''s inference gateway (built
  on the same primitives) measured up to 108% throughput improvement for cache-aware
  vs. random routing. The post frames three escalating levels of prefix-aware routing
  and names their structural failure modes: Level 1 (session/sticky affinity by user
  ID) breaks on scale-out because adding a replica reshuffles the hash ring, triggering
  cache misses until the fleet re-warms; Level 2 (prefix-hash routing on the stable
  prompt prefix) requires correctly choosing the prefix boundary; Level 3 (KV-event-aware
  routing) subscribes to KV block allocation/eviction events from each inference engine,
  scores replicas on cache-overlap fraction weighted against current load, and is
  explicitly identified as the architecture used by llm-d''s Endpoint Picker component.
  The piece explicitly frames cache locality and load balancing as pulling in opposite
  directions and states prefix-aware routing should be skipped when prompts are unique,
  short (<~200 tokens), or workloads lack structural repetition.'
utility_score: 10.0
---

Meet TrueForge: The open-source, vendor-neutral agent harness. 50% lower cost. [Explore Now→](https://github.com/truefoundry/trueforge)
[Sign Up](https://signup.truefoundry.com/signup)[Login](https://www.truefoundry.com/login)
[Go back](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it)
Try TrueFoundry — Live, Right Now
Get instant access to a live TrueFoundry environment. Deploy models, route LLM traffic, and explore the full platform — your sandbox is ready in seconds, no credit card required.
Loved by Enterprises and Startups
# KV Cache Routing: Why Standard Load Balancers Break Prefix Caching (and How to Fix It)
[By Amrutha Potluri](https://www.truefoundry.com/blogs/authors/amrutha-potluri)
Published: June 22, 2026
### Built for Speed: ~10ms Latency, Even Under Load
Blazingly fast way to build, track and deploy your models!
  * Handles 350+ RPS on just 1 vCPU — no tuning needed
  * Production-ready with full enterprise support


[Get Started with Truefoundry Now](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it)[Talk to the Expert](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it)
Prefix caching lets vLLM and SGLang skip recomputing tokens the model has already seen — but only if the next request lands on the same GPU replica. Standard round-robin routing can significantly reduce cache locality in multi-replica deployments, limiting the benefits of prefix caching unless routing is made cache-aware. Prefix-aware sticky routing fixes it. This post explains the mechanism, the cost of ignoring it, and how to implement it in three levels of increasing sophistication.
Inference accounts for a growing share of AI compute demand. Some industry estimates project inference could represent the majority of AI compute consumption as generative AI workloads mature, and a significant fraction of that is redundant work — your model recomputing attention states for tokens it processed seconds ago. The culprit isn't the GPU or the model. It's the load balancer sitting in front of your inference cluster, routing each request without any awareness of what's already cached where.
_For a broader overview of LLM inference engines and optimization levers, see_[ _LLM Inferencing: Optimize Speed, Cost & Scale AI_](https://www.truefoundry.com/blog/llm-inferencing) _._
## Why Prefix Caching Matters
Every LLM inference request runs in two distinct phases. During **prefill** , the model processes the entire input sequence and builds a key-value (KV) cache — a matrix of attention states, one entry per token, stored in GPU VRAM. During **decode** , the model generates output tokens one at a time, attending back to that cached state without re-reading the input.
Prefill is expensive. Its compute cost scales quadratically with input length: doubling the prompt quadruples the attention computation. For a 2,000-token system prompt processed once per request at 1,000 requests per hour, that's 2 million tokens of prefill work per hour — every hour, every day.
Prefix caching eliminates the redundant portion of that work. If Request B shares a prefix with a previously processed Request A, the engine reuses the cached KV blocks for those shared tokens and computes only the suffix that's new. The savings compound across three common workload patterns:
  * **RAG pipelines** : retrieved document chunks can often overlap across queries, particularly when many users access the same knowledge base or frequently referenced documents.
  * **Multi-turn chat** : each follow-up message appends to conversation history the model has already encoded.
  * **Few-shot prompting** : a fixed set of examples prepended to every request costs prefill compute once, not N times.


vLLM implements prefix caching via hash-based block matching — each KV block is identified by a hash of the tokens that produced it. SGLang uses RadixAttention, a radix tree that finds the longest matching prefix across all active requests and reuses those blocks directly. On prefix-heavy workloads, SGLang's RadixAttention has been reported to deliver multi-fold throughput improvements over cache-blind serving, with LMSYS reporting gains of up to roughly 5× in representative benchmarks. vLLM with automatic prefix caching enabled eliminates the majority of redundant prefill on multi-turn workloads.
This works best on a single instance when requests have stable shared prefixes and enough KV cache capacity to retain reusable blocks. The moment you scale to a multi-replica fleet, the problem begins.
## How Round-Robin Routing Destroys Cache Hit Rate
Consider a deployment with eight vLLM replicas behind a standard Kubernetes service. Replica 1 processes a request with a 2,000-token system prompt, computes the KV cache, and stores it in its VRAM. The next request with the identical prompt arrives. Round-robin sends it to Replica 2 — which has no cache for that prefix, so it recomputes the full prefill from scratch. The third request goes to Replica 3. Same miss.
In workloads where a useful prefix cache exists on only one replica—for example, growing chat histories or tenant-specific contexts—round-robin routing reduces the probability of landing on the cache-owning replica as fleet size increases. This effect can significantly reduce cache locality and increase redundant prefill computation.
In prefix-heavy workloads, naive scale-out can reduce cache locality and increase redundant prefill unless routing is cache-aware.
The production numbers are stark. The llm-d project, Red Hat's Kubernetes-native distributed inference framework, benchmarked prefix-cache-aware routing against round-robin on 8 pods / 16 H100 GPUs and measured **57× faster time-to-first-token and 2× throughput on the same hardware.** A separate benchmark on Llama 3.1 70B across 4× AMD MI300X GPUs showed **3× output tokens/sec and 2× TTFT reduction** after switching routing strategies. DigitalOcean's inference gateway, built on the same primitives, measured up to **108% throughput improvement** for cache-aware versus random routing at identical hardware configurations.
The economic impact depends on request volume, model size, GPU type, and workload characteristics, but higher cache hit rates generally translate into lower infrastructure costs and better utilization of existing hardware.
The challenge is that pure cache locality and pure load balancing pull in opposite directions. The question then becomes: how do you preserve cache locality without sacrificing load balancing?
## Three Levels of Prefix-Aware Routing
These approaches are ordered from simplest to most precise.
### Level 1 — Session Affinity
The quickest fix: configure your load balancer to hash on a session or user identifier and always route requests from the same session to the same replica. This is available natively in most load balancers as a sticky-session or consistent-hash policy — no custom routing logic required.
Session affinity works well for multi-turn chat where the same user generates sequential requests with growing conversation history. It has two structural limits. Adding a replica reshuffles the hash ring, breaking existing affinity and triggering cache misses until the fleet re-warms.
Use this as a fast initial improvement for single-tenant or low-concurrency deployments, or anywhere you want a quick win before investing in a more precise solution.
### Level 2 — Prefix-Hash Routing
Rather than hashing on who sent the request, hash on what the request contains. Specifically, hash the first N tokens of the prompt — the stable prefix that's identical across users — and route all requests with the same prefix hash to the same replica.
The key difference from session affinity: this handles shared system prompts correctly. If the routing key is computed from the stable shared prefix, users with the same system prompt can be routed to the same replica. The replica computes the prefix KV cache once and serves every subsequent matching request from cache.
Choosing the right prefix boundary matters more than the hashing mechanism itself. For a RAG pipeline, the boundary should cover the longest stable portion of the prompt, which may be only the system prompt or may include commonly reused retrieved context. For few-shot prompting, it's the end of the example block. For multi-turn chat, it's the end of the previous turn. Setting the boundary too short leaves variable content inside the hashed region and generates spurious misses; setting it too long includes content that varies across requests and fragments routing across too many replicas.
### Level 3 — KV-Event-Aware Routing
The most precise approach: the router subscribes to KV block allocation and eviction events emitted by each inference engine and maintains a real-time map of which blocks — identified by their content hashes — are resident on which replica. On each incoming request, it scores every eligible replica by how much of the request's prefix is already cached there, weighted against each replica's current load, then routes to the highest-scoring option.
The scoring function balances two signals. Cache overlap score measures what fraction of the incoming request's prefix blocks are already resident on a given replica. Load score measures how saturated that replica currently is. The router balances cache locality against current replica load when making routing decisions. 
This is broadly the architecture used by llm-d's Endpoint Picker component, which uses cache-state and load information to select a target replica before forwarding the request. The routing overhead itself is negligible against the prefill savings it unlocks.
## When to Skip Prefix-Aware Routing
Prefix-aware routing earns its complexity only when workloads have genuine prefix overlap. It adds a routing decision and a block-state lookup on every request with no return in three scenarios: when all prompts are unique (document summarization, creative generation with varied inputs), when prompts are short (under roughly 200 tokens, where prefill cost is trivial), or when batch workloads process fully distinct inputs with no structural repetition.
The practical diagnostic: measure your cache hit rate directly from vLLM's cache event metrics or SGLang's RadixAttention stats.If hit rate remains low even after cache-aware routing, inspect prompt structure first.
## How TrueFoundry Implements KV Cache Sticky Routing
TrueFoundry's AI Gateway and model deployment layer implement sticky routing for KV cache optimization natively. When a model is deployed via TrueFoundry, requests with matching prefixes can be routed to the same vLLM or SGLang replica, allowing those requests to reuse existing KV cache state instead of repeatedly recomputing the same prefill tokens.
The routing layer is designed to balance two competing goals: preserving cache locality and maintaining healthy load distribution across replicas. When a replica already contains the relevant prefix cache, routing can favor that replica to maximize reuse. When a replica becomes saturated, the gateway can fall back to alternative replicas to avoid turning cache locality into a bottleneck.
Because routing is integrated with model deployment, teams do not need to build separate infrastructure for sticky sessions, prefix hashing, or custom load-balancing policies. The same deployment workflow used to scale models horizontally also preserves cache-aware routing behavior as replicas are added or removed.
For teams running long system prompts, multi-turn chat applications, RAG workloads, or few-shot prompting pipelines, this improves the likelihood that repeated prefixes remain warm on the replicas most likely to serve subsequent requests. The result is higher cache utilization, lower redundant prefill compute, and better GPU efficiency without requiring application-level routing logic.
## Best Practices
  * **Fix prompt structure before touching routing.** Place all static content — system prompt, few-shot examples, retrieved chunks — at the start of every prompt. Variable content (user message, request ID, timestamp) must follow the stable prefix. A single misplaced dynamic field before the system prompt collapses cache-ability entirely, regardless of routing strategy.
  * **Set your prefix boundary based on actual token counts, not guesses.** Tokenize your typical system prompt and example block, count the tokens, and use that boundary for prefix hashing. An arbitrary fixed length often cuts mid-sentence inside variable content or misses the full stable region.
  * **Start at Level 2, measure, then decide whether Level 3 is worth it.** Moving from round-robin to prefix-hash routing captures the majority of available gains with no custom routing infrastructure. Level 3 is worth evaluating when you run multiple replicas at sustained concurrency and prefix-cache misses are a measured bottleneck.
  * **Monitor cache hit rate as a production alert, not just a dashboard metric.** A developer quietly adding a dynamic field to the system prompt will tank hit rate immediately. The latency regression shows up before anyone checks logs. Alert on hit rate dropping below your established baseline.
  * **Tune the locality weight per workload type, not globally.** Multi-turn chat and RAG pipelines benefit from strong cache-locality preference. Batch inference over diverse inputs benefits from pure load balancing. If your gateway serves mixed traffic, run separate routing configurations per workload class rather than compromising both.
  * **Watch VRAM utilization alongside hit rate.** Prefix caching trades GPU memory for prefill savings. At high concurrency, cached blocks compete with active requests for VRAM, triggering LRU eviction and collapsing hit rates. If hit rate degrades under load despite correct routing, memory pressure is the likely cause — adjust the memory utilization ceiling on the inference engine before assuming a routing problem.


## Conclusion
For most teams, moving from round-robin to prefix-hash routing is the highest-leverage optimization. More sophisticated cache-aware routing becomes increasingly valuable as fleet size, concurrency, and cache reuse grow.
Learn how TrueFoundry's AI Gateway handles prefix-aware routing alongside model deployment in a single control plane → [truefoundry.com/ai-gateway](https://www.truefoundry.com/ai-gateway)
TrueFoundry AI Gateway delivers ~3–4 ms latency, handles 350+ RPS on 1 vCPU, scales horizontally with ease, and is production-ready, while LiteLLM suffers from high latency, struggles beyond moderate RPS, lacks built-in scaling, and is best for light or prototype workloads.
Built for Speed: ~10ms Latency, Even Under Load
[Schedule your Demo Now](https://www.truefoundry.com/book-demo)
### Try now.
One gateway for all your models, MCP servers, and agents.No credit card needed.
[Start free](https://signup.truefoundry.com/signup?ref=%2Fblog%2Fkv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it)
How Can You Prevent GenAI Costs From Spiraling at Scale?
Gartner Hype Cycle for Platform Engineering 2026
## One Layer of Control for All AI
Route and govern model and tool traffic with a centralized AI Gateway
[Book Demo ](https://www.truefoundry.com/book-demo)
Table of Contents
[Why Prefix Caching Matters](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it#why-prefix-caching-matters)
[How Round-Robin Routing Destroys Cache Hit Rate](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it#how-round-robin-routing-destroys-cache-hit-rate)
[Three Levels of Prefix-Aware Routing](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it#three-levels-of-prefix-aware-routing)
[When to Skip Prefix-Aware Routing](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it#when-to-skip-prefix-aware-routing)
[How TrueFoundry Implements KV Cache Sticky Routing](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it#how-truefoundry-implements-kv-cache-sticky-routing)
### One Gateway for Every LLM, Agent and MCP Server
Book a 30-min with our AI expert
[Book a Demo](https://www.truefoundry.com/book-demo?ref=%2Fblog%2Fkv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it)
### The fastest way to build, govern and scale your AI
[Book Demo](https://www.truefoundry.com/blog/kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix-it)
Summarize with
## _Discover More_
No items found.
September 14, 2026
|
5 min read
### Maxim AI vs Vercel AI Gateway: Which Platform Fits Enterprise AI Teams?
No items found.
September 14, 2026
|
5 min read
### Maxim AI vs Solo.io: Which Platform Fits Enterprise AI Teams Better?
No items found.
September 14, 2026
|
5 min read
### An Agent Identity Is Not an Authorization Decision: Designing Delegated Authority End to End
No items found.
September 14, 2026
|
5 min read
### Agents, Skills, and MCP Servers Are a Software Supply Chain: Build an Admission-Control Pipeline
No items found.
No items found.
## Recent Blogs
### [Maxim AI vs Vercel AI Gateway: Which Platform Fits Enterprise AI Teams? September 14, 2026 Ashish Dubey ](https://www.truefoundry.com/blog/maxim-ai-vs-vercel-ai-gateway)
### [An Agent Identity Is Not an Authorization Decision: Designing Delegated Authority End to End September 14, 2026 Boyu Wang ](https://www.truefoundry.com/blog/agent-identity-delegated-authority-chain)
### [Agents, Skills, and MCP Servers Are a Software Supply Chain: Build an Admission-Control Pipeline September 14, 2026 Boyu Wang ](https://www.truefoundry.com/blog/agent-component-supply-chain-admission-control)
### [Maxim AI vs Solo.io: Which Platform Fits Enterprise AI Teams Better? September 14, 2026 Ashish Dubey ](https://www.truefoundry.com/blog/maxim-ai-vs-solo-io)
### [LangGraph Alternatives: 5 Options Compared for 2026 September 14, 2026 Sahajmeet Kaur ](https://www.truefoundry.com/blog/langgraph-alternatives)
### [Agno vs LangChain: Which Agent Framework Should You Pick? September 14, 2026 Sahajmeet Kaur ](https://www.truefoundry.com/blog/agno-vs-langchain)
### [Agent Harness vs Agent Framework: What's Actually Different? September 13, 2026 Sahajmeet Kaur ](https://www.truefoundry.com/blog/agent-harness-vs-agent-framework-whats-actually-different)
### [Claude Agent SDK vs LangGraph: Which Should You Build On? September 14, 2026 Sahajmeet Kaur ](https://www.truefoundry.com/blog/claude-agent-sdk-vs-langgraph)
### [Deferred Tool Loading, Explained: Treat Tool Schemas as a Context Budget September 14, 2026 Boyu Wang ](https://www.truefoundry.com/blog/trueforge-deferred-tool-loading)
### [Best Open Source Agent Harness: Top 5 Projects Compared for 2026 September 14, 2026 Sahajmeet Kaur ](https://www.truefoundry.com/blog/best-open-source-agent-harness)
### [LangChain Deep Agents Alternatives: 5 Options Compared for 2026 September 14, 2026 Sahajmeet Kaur ](https://www.truefoundry.com/blog/langchain-deep-agents-alternatives)
### [Bifrost Pricing: OSS, Enterprise Costs, and What Teams Should Know September 12, 2026 Ashish Dubey ](https://www.truefoundry.com/blog/bifrost-pricing)
### [Requesty AI Pricing: Cost, Features, and Enterprise Fit Explained September 11, 2026 Ashish Dubey ](https://www.truefoundry.com/blog/requesty-ai-pricing)
### [Top 5 Solo.io Competitors and Alternatives for 2026 September 11, 2026 Ashish Dubey ](https://www.truefoundry.com/blog/solo-io-competitors)
### [LiteLLM vs Vercel AI Gateway: Which Platform Fits AI Engineering Teams Better? September 10, 2026 Ashish Dubey ](https://www.truefoundry.com/blog/litellm-vs-vercel-ai-gateway)
Take a quick product tour
[Start Product Tour](https://www.truefoundry.com/product-tour)
Product Tour
Product


Company
  * [Terms of Service](https://www.truefoundry.com/terms)


#### Resources
  * [Events & Conferences](https://www.truefoundry.com/events-conferences)


#### Why TrueFoundry?


  * © 2022 ENSEMBLE Technologies


Ensemble Labs Inc, 355 Bryant Street, Suite 403, San Francisco, CA 94107
#### Subscribe to our newsletter
The latest news, articles, and resources sent to your inbox
© 2026 All rights reserved.
[Hey AI, learn about us](https://www.truefoundry.com/llm-info)
### Chat with TrueFoundry
We typically reply within minutes
✕
Please enter a valid work email.
Start chatting →
