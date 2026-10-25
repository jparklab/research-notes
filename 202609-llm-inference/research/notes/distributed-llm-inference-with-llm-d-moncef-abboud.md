---
title: Distributed LLM Inference with llm-d | Moncef Abboud
id: distributed-llm-inference-with-llm-d-moncef-abboud
tags:
- llmd-router-nonk8s-backends-54b774
- epp-architecture
- file-discovery
- kv-cache-events
- disaggregated-serving
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T20:48:46.547020Z'
updated: '2026-09-15T04:55:23.838119Z'
source: https://cefboud.com/posts/llm-d/
source_domain: cefboud.com
fetched_at: '2026-09-11T20:48:46.523715Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Named-practitioner (Moncef Abboud) walkthrough of llm-d architecture: EPP-as-Envoy-ext_proc,
  scoring/picking pipeline, and disaggregated P/D serving. States plainly that llm-d
  has a mode where it runs outside Kubernetes via a ''file discovery'' plugin where
  vLLM endpoints are hardcoded/listed instead of discovered via the k8s API, linking
  to https://github.com/llm-d/llm-d-router/blob/f7e8852479d6b2902d3bd89d8d252187211ff801/docs/discovery.md.
  Documents the request path in detail: Envoy''s ext_proc streams RequestHeaders/RequestBody
  to the EPP''s StreamingServer.Process, which calls director.HandleRequest -> admission
  plugins -> flow control (priority/fairness via x-gateway-inference-fairness-id,
  x-gateway-inference-objective headers) -> data-producer plugins (tokenizer, prefix
  matcher) -> scheduler (filter -> weighted scorers -> weighted-random picker) ->
  EPP returns x-gateway-destination-endpoint header with IP:port, and Envoy forwards
  there. Gives a worked numeric scoring example (prefix-cache weight 3.0, kv-cache-utilization
  weight 1.0, queue-depth weight 1.0) showing weighted-random picking avoids thundering-herd
  on the single top-scored pod. Confirms the EPP gathers node metrics by scraping
  each backend''s Prometheus /metrics endpoint (vllm:num_requests_waiting, vllm:kv_cache_usage_perc,
  etc., with an analogous sglang:* config), and that precise KV-cache tracking subscribes
  directly to vLLM''s ZMQ KV-event stream per instance via the llm-d-kv-cache library
  (github.com/llm-d/llm-d-kv-cache). Also notes RDMA/NIXL-based direct KV-cache transfer
  between prefill and decode pods over InfiniBand/RoCE for disaggregated serving.'
utility_score: 11.0
---

Distributed LLM Inference with llm-d
Contents
Distributed LLM Inference with llm-d
  * [If It Ain’t Broke](https://cefboud.com/posts/llm-d/#if-it-aint-broke)
    * [KV-cache Utilization](https://cefboud.com/posts/llm-d/#kv-cache-utilization)
  * [Envoy and the External Processing Filter](https://cefboud.com/posts/llm-d/#envoy-and-the-external-processing-filter)
  * [Disaggregated Serving](https://cefboud.com/posts/llm-d/#disaggregated-serving)


##  Intro
What does production-grade LLM inference look like?
If you wake up in the middle of the night thinking about that question, this blog post might be for you.
Inference engines like vLLM and SGLang sit on top of PyTorch (when I say vLLM going forward, that also includes SGLang and other supported inference engines). They optimize inference at the node level, most notably by managing KV cache and improving throughput through techniques such as paged attention and continuous batching.
The elevator pitch for llm-d is that it’s an LLM-aware load balancer.
If we have multiple vLLM instances, each has its own state: available GPU memory, KV cache prefix matches, number of requests queuing to be processed, etc. We can’t simply use round robin. Selecting the best inference engine instance based on these signals is what llm-d is all about.
It also provides features such as flow control to support different classes of requests based on priority (e.g., premium real-time traffic vs batch workloads). In addition, it enables smooth disaggregated P/D, where prefix and decode run on different nodes because they benefit from different configs. Prefix is compute-bound, while decode is memory-bandwidth-bound, so they have different GPU requirements: low TP for prefix to maximize compute, and high TP for decode to maximize memory bandwidth.
It also provides features such as flow control to support different classes of requests based on priority (e.g., premium real-time traffic vs batch workloads). In addition, it enables smooth disaggregated P/D, where prefix and decode run on different nodes because they benefit from different configs. Prefix is compute-bound, while decode is memory-bandwidth-bound, so they have different GPU requirements: low TP for prefix to maximize compute, and high TP for decode to maximize memory bandwidth.
##  If It Ain’t Broke
The cool thing about llm-d is that it doesn’t reinvent the wheel. It builds on top of existing, established projects. LLM inference still happens on vLLM and SGLang, and we simply communicate with them via HTTP. The proxy layer and discovery are all built on top of Kubernetes(k8s) and Envoy. Even the integration point for deciding which vLLM instance to choose relies on an existing extension point, namely Envoy’s [`ext_proc`](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) extension. The data layer and metric collection are essentially built on top of Prometheus. So, no reinventing the wheel, just intelligently combining solid existing solutions. The best part is that for each layer and piece (metrics, scoring, flow control, etc.), llm-d is easily extensible with clear interfaces that new plugins can implement and use right away.
So if vLLM adds a new metric, or even if a new inference engine comes along, it can be easily added. If we want to implement a new way to pick or score, we just implement a new picker or scorer. If Prometheus falls out of favor, or if we want to use a custom monitoring solution, we can implement a plugin for that instead.
There’s a standardization effort to consolidate Generative AI inference on top of k8s, taking the form of the [Gateway API Inference Extension, or GAIE](https://gateway-api-inference-extension.sigs.k8s.io/). GAIE defines the API resources (like `InferencePool`) and the Endpoint Picker role. llm-d’s router is an implementation of that EPP role, paired with an Envoy proxy that does the actual request forwarding.
That’s a very strategic choice. Rather than having llm-d be an isolated initiative with a bespoke API, it’s built as an implementation of a broader k8s standard.
It’s also worth mentioning that llm-d has a mode where it runs outside of k8s via a [file discovery](https://github.com/llm-d/llm-d-router/blob/f7e8852479d6b2902d3bd89d8d252187211ff801/docs/discovery.md) plugin, where the vLLM endpoints are hardcoded instead of discovered via the k8s API.
So what are these factors that need to be taken into account for LLM inference routing?
###  Prefix Cache
Say we have N1 and N2. If a user has already gotten a response from N1, this means the user’s request KV cache has already been calculated there, so a subsequent request can skip that step. If, however, we send the user’s request to N2, we need to re-run prefill and we won’t be taking advantage of the calculation already done on N1.
In other words, it’s efficient to route requests to nodes that already have the KV cache of the prompt (this consideration changes if we have KV cache offloading, in which the cache can be stored in shared network storage, for instance).
###  KV-cache Utilization
Another factor we care about is how much free VRAM N1 and N2 have. If N1’s VRAM is almost full, even though it has the user’s KV cache, it might not be the best choice, because we might need to wait for other requests to finish or evict them in order to run the request. If at the same time N2’s VRAM is free and ready to go, it might be best to rerun the prefill and make use of the free memory.
###  Queue Depth
Both nodes might be running with little free VRAM, but N1 has only 2 requests in its queue waiting to be processed while N2 has 5. It’s best to route to the node with the smaller number of waiting requests.
There are many factors to consider when deciding which node to pick, and llm-d’s job is to pick. It gathers data from the various vLLM instances via the `/metrics` Prometheus endpoint, and keeps the state of each node in memory. When a user request comes in, it’s sent to llm-d’s router, called the Endpoint Picker or EPP, and the EPP will run a score of scorers (yes, the silly pun is intended):`prefix-cache-scorer`, `kv-cache-utilization-scorer`, `queue-scorer`, etc. for each possible candidate (vLLM instance), combining those scores and then picking one of the candidates. Before scoring and picking, there’s a filtering phase in which we can filter the candidate endpoints.
##  Envoy and the External Processing Filter
llm-d’s router doesn’t replace Envoy, it rides alongside it as an Envoy proxy filter target. When a request comes in, the Envoy proxy consults the llm-d EPP. The EPP does its thing and in the HTTP response it provides a header:
`  
  | 
```
x-gateway-destination-endpoint: IP_PORT_OF_A_VLLM_POD

```
  |  
| --- |  
`
Then Envoy will route the request to that vLLM pod. That’s it, really.
Envoy Proxy has an extension (HTTP filter) called `ext_proc`, or [External Processing](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter). When a request comes in, it’s forwarded to an external process via gRPC, and that process can do a lot of things: mutate the HTTP body, add or remove headers, reject the request, etc. In our case, the llm-d router EPP is the external process, and it simply adds a header, the one with the address of the chosen vLLM instance.
How we make that choice is the magic that llm-d brings. And as we said above, it gathers data from the [inference engines’](https://github.com/llm-d/llm-d-router/blob/f7e8852479d6b2902d3bd89d8d252187211ff801/pkg/epp/framework/plugins/datalayer/extractor/metrics/factories.go#L89) Prometheus endpoints:
`  
 | 
```
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21

```
  | 
```
var defaultEngineConfigs = []engineConfigParams{
	{
		Name:                "vllm",
		QueuedRequestsSpec:  "vllm:num_requests_waiting",
		RunningRequestsSpec: "vllm:num_requests_running",
		KVUsageSpec:         "vllm:kv_cache_usage_perc",
		LoRASpec:            "vllm:lora_requests_info",
		CacheInfoSpec:       "vllm:cache_config_info",
	},
	{
		Name:                    "sglang",
		QueuedRequestsSpec:      "sglang:num_queue_reqs",
		RunningRequestsSpec:     "sglang:num_running_reqs",
		KVUsageSpec:             "sglang:token_usage",
		LoRASpec:                "",
		CacheInfoSpec:           "sglang:cache_config_info",
		CacheBlockSizeLabelName: "page_size",
		CacheNumBlocksLabelName: "num_pages",
	},
    //...
    }

```
  |  
| --- | --- |  
`
And based on this data, it calculates a score for each candidate endpoint and picks one. Usually that’s the one with the highest score, but the picker can also sample according to score instead of just taking the max, which is a nice detail: always routing to the single best pod can create a thundering herd, where everyone piles onto the same “good” replica until it stops being good. Spreading the load based on score avoids that while still favoring the better candidates, this has a nice parallel to sampling a token after the softmax layer in an LLM.
Envoy establishes a gRPC connection with the EPP and streams it the request headers and body. Essentially, it’s:
`  
 | 
```
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34

```
  | 
```
func (s *StreamingServer) Process(
    srv extProcPb.ExternalProcessor_ProcessServer,
) error {

    // Shared state for the lifetime of the request
    reqCtx := NewRequestContext()

    // Stream reader
    recvCh := startReceiver(srv)

    for {
        select {

        case msg := <-recvCh:

            switch v := msg.Request.(type) {

            case *RequestHeaders:
                handleRequestHeaders(reqCtx, v)

            case *RequestBody:
                handleRequestBody(reqCtx, v) // Actually: s.director.HandleRequest(ctx, reqCtx, parseResult.Body)

            case *ResponseHeaders:
                handleResponseHeaders(reqCtx, v)

            case *ResponseBody:
                handleResponseBody(reqCtx, v)

            }
        }

    }
}

```
  |  
| --- | --- |  
`
When we finish receiving the request body, we run through `s.director.HandleRequest`. The director holds the datastore that has the endpoints and their metrics.
We run the request through some admission plugins (so it can be rejected if needed, this is extensible like most of the pipeline, and custom rules can be added at different points to handle that).
We then go through flow control, where requests are treated based on their priority/flow id, specified via headers (`x-gateway-inference-fairness-id`, `x-gateway-inference-objective`). Flows can have different priorities, and within the same priority band we can follow fairness and ordering policies (round robin, first come first served, earliest deadline first, etc).
Once the request makes it through flow control, the endpoint candidates are grabbed and we run the “data producer” plugins for that request. These plugins augment the request with new data: tokenize the string by adding token ids (there’s a tokenizer that relies on a [vLLM API](https://github.com/llm-d/llm-d-router/blob/20e61b8b461f7fa75911a14b3ec83ce63b4b469c/pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/README.md)), and another key producer is the prefix matcher: how many of this request’s tokens already exist in the KV cache of each candidate pod, etc.
Then comes the essential part: the scheduler. We have a scheduling profile that has filters, scorers, and a picker. We filter the candidates, then score the request according to each scorer (of which there are many: prefix cache scorer, KV cache utilization, queue depth, etc.), then pick based on a combined score (each scorer can have its own weight).
Scheduling:
`  
 | 
```
1
2
3
4
5
6
7
8
9
10

```
  | 
```
Score:
Prefix cache scorer (weight=3.0): Pod 1 = 0.75, Pod 2 = 0.4, others = 0.0
KV-cache utilization (weight=1.0): Pod 1 = 0.6, Pod 2 = 0.8, others vary
Queue depth (weight=1.0): Pod 1 = 0.7, Pod 2 = 0.5, others vary
Final scores:
Pod 1: (0.75 × 3) + (0.6 × 1) + (0.7 × 1) = 3.55
Pod 2: (0.4 × 3) + (0.8 × 1) + (0.5 × 1) = 2.5
Others: < 2.0
Picker (weighted random): Pod 1 is strongly favored but not deterministic
Result: Pod 1 selected (IP: 10.0.1.42, port: 8000).

```
  |  
| --- | --- |  
`
The EPP sends a response to Envoy with `x-gateway-destination-endpoint` set to the IP and port of the selected pod, and Envoy forwards the request there. As the pod generates tokens, Envoy streams the response back to the client. The ext_proc stream also receives response headers and body chunks for metric recording and data updates (new KV cache being calculated, etc).
And that’s about it.
##  KV Events
If the model server (vLLM) already has part of the prompt’s KV cache, routing the request there avoids recomputing the prefix. We need to know about the content of each node’s KV cache.
There is an approximate prefix cache producer estimates each node’s KV cache from past routing decisions: if a prompt was previously sent to vLLM 1, it’s likely that its KV cache is still there.
The downside is that it’s only an approximation and doesn’t account for cache evictions.
There is a precise KV cache producer takes a much more interesting approach. vLLM exposes [KV-cache allocation and eviction events over ZMQ ](https://docs.vllm.ai/en/stable/api/vllm/config/kv_events/)(a brokerless high-performance messaging library), and the llm-d router subscribes to these streams from every vLLM instance. This gives the router a real-time, exact view of each server’s KV cache, allowing it to make precise routing decisions at the cost of a bit of additional overhead.
This relies on a library hosted in its own repo: [llm-d-kv-cache](https://github.com/llm-d/llm-d-kv-cache/tree/main).
##  Disaggregated Serving
LLM inference has two phases:
  * **Prefill** : runs first, computing the KV cache for all prompt tokens. it’s compute-bound and performs best with low Tensor Parallelism (TP).
  * **Decode** : runs after prefill, autoregressively predicting the next token from the existing KV cache. Computation is minimal (only the last token), and we’re constantly moving memory from HBM to compute units. It’s memory-bandwidth bound and works best with larger TP.


Beyond their different TP needs, a large prefill phase can hog compute resources from smaller decode requests. Separating the two stages onto different nodes avoids this and improves performance overall.
In practice: a request first hits prefill pods for initial KV cache computation, then that KV cache is transferred to decode pods, where autoregressive prediction takes place.
RDMA (remote direct memory access) moves that KV cache from prefill to decode nodes very fast, bypassing the CPU entirely. The underlying networking is InfiniBand or RoCE. llm-d uses NVIDIA’s Inference Transfer Library (NIXL) for this [transfer](https://llm-d.ai/docs/architecture/advanced/disaggregation#direct-kv-cache-transfer). Fascinating stuff.
`  
 | 
```
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20

```
  | 
```
┌─────────────────────────────┐              ┌─────────────────────────────┐
│          NODE 1             │              │          NODE 2             │
│                             │              │                             │
│  ┌───────────────────────┐  │              │  ┌───────────────────────┐  │
│  │     Prefill Pod       │  │              │  │     Decode Pod        │  │
│  │                       │  │              │  │                       │  │
│  │  ┌─────────────────┐  │  │              │  │  ┌─────────────────┐  │  │
│  │  │ KV Cache (VRAM) │  │  │              │  │  │ KV Cache (VRAM) │  │  │
│  │  └────────┬────────┘  │  │              │  │  └────────┬────────┘  │  │
│  │           │           │  │              │  │           │           │  │
│  └───────────┼───────────┘  │              │  └───────────┼───────────┘  │
│              │              │              │              │              │
│              ▼              │              │              ▼              │
│  ┌───────────────────────┐  │   Network    │  ┌───────────────────────┐  │
│  │         NIC           │  │              │  │         NIC           │  │
│  │  InfiniBand / RoCE    │──┼──────────────┼──│  InfiniBand / RoCE    │  │
│  └───────────────────────┘  │              │  └───────────────────────┘  │
│                             │              │                             │
└─────────────────────────────┘              └─────────────────────────────┘

```
  |  
| --- | --- |  
`
Newsletter
[Technical Writing](https://cefboud.com/categories/technical-writing/), [Open Source](https://cefboud.com/categories/open-source/)
This post is licensed under [ CC BY 4.0 ](https://creativecommons.org/licenses/by/4.0/) by the author.
Share
## Trending Tags
[Open Source](https://cefboud.com/tags/open-source/) [AI](https://cefboud.com/tags/ai/) [LLM](https://cefboud.com/tags/llm/) [Apache Kafka](https://cefboud.com/tags/apache-kafka/) [MCP](https://cefboud.com/tags/mcp/) [Linux](https://cefboud.com/tags/linux/) [Opencode](https://cefboud.com/tags/opencode/) [vLLM](https://cefboud.com/tags/vllm/) [AI Assistant](https://cefboud.com/tags/ai-assistant/) [C](https://cefboud.com/tags/c/)
