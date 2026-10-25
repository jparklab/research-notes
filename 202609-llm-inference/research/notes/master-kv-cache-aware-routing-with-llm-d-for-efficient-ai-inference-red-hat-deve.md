---
title: Master KV cache aware routing with llm-d for efficient AI inference | Red Hat
  Developer
id: master-kv-cache-aware-routing-with-llm-d-for-efficient-ai-inference-red-hat-deve
tags:
- llmd-router-nonk8s-backends
- kv-cache-indexer
- pod-discovery
- zmq-kv-events
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:44:05.650523Z'
updated: '2026-09-15T04:55:23.710274Z'
source: https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference
source_domain: developers.redhat.com
fetched_at: '2026-09-11T19:44:05.629674Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Red Hat Developer article (Maroon Ayoub/IBM, Oct 7 2025) walks through KV-cache-indexer
  internals using the llm-d-kv-cache-manager repo (v0.4.0-era) and gives a concrete
  Kubernetes production example (87.4% cache hit rate, 88% TTFT improvement 2,850ms->340ms).
  Lists core internal components: kvcache.Indexer (orchestrator), kvevents.Pool (sharded
  ZMQ worker pool ingesting events), kvblock.Index (in-memory two-level LRU mapping
  block hashes to pods), tokenization.PrefixStore (LRU-cached tokenized prefixes),
  kvblock.TokenProcessor (chunking/hashing matching vLLM''s algorithm), kvblock.Scorer
  (longest-consecutive-prefix-match scoring). CAUTION — conflicts with the more current
  (2026) llm-d/llm-d GitHub guide on ZMQ topic format: this article states ''Events
  parsed by topic format: kv@pod-id@model'' whereas the current guide''s --kv-events-config
  sets topic to ''kv@$(POD_IP):$(POD_PORT)@<model>'' (i.e., keyed by literal pod IP:port,
  not a Kubernetes pod-id/name). Given the domain note that llm-d/GIE APIs ''changed
  quickly'' over the last 12-18 months, and this article predates the guide by roughly
  a year, the IP:port-keyed format in the current repo should be treated as authoritative
  and this article''s ''pod-id'' phrasing as either simplified/inaccurate or reflecting
  an older topic-key scheme. Describes pod discovery as label-selector based: EPP
  auto-discovers decode-service endpoints via the Kubernetes Service label llm-d.ai/inferenceServing:
  "true", and shows a config snippet with a discovery.label_selector field — reinforcing
  that endpoint discovery in the shipped implementation is Kubernetes-label/selector
  driven rather than static config in this deployment path.'
utility_score: 10.0
---

[ Skip to main content ](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#main-content)
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
#  Master KV cache aware routing with llm-d for efficient AI inference
October 7, 2025
Maroon Ayoub (IBM)  

Related topics:
    [Artificial intelligence](https://developers.redhat.com/topics/ai-ml) 

Related products:

Table of contents: 
  * [Stateless inference fails to reuse cache](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#stateless_inference_fails_to_reuse_cache)
  * [KV cache aware routing](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#kv_cache_aware_routing)
  * [Example deployment](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#example_deployment)
  * [Official community Helm chart approach](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#official_community_helm_chart_approach)
  * [InferencePool and InferenceModel (auto-created by Helm chart)](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#inferencepool_and_inferencemodel__auto_created_by_helm_chart_)
  * [KV cache indexer : Deep architecture dive](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#kv_cache_indexer___deep_architecture_dive)
  * [Core architecture components](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#core_architecture_components)
  * [The read path: Intelligent pod scoring](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#the_read_path__intelligent_pod_scoring)
  * [The write path: Real-time cache tracking](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#the_write_path__real_time_cache_tracking)
  * [Component configuration details](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#component_configuration_details)
  * [vLLM prefix caching configuration](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#vllm_prefix_caching_configuration)
  * [EnvoyFilter for EPP integration](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#envoyfilter_for_epp_integration)
  * [Test case and results](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#test_case_and_results)
  * [Grafana dashboard monitoring](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#grafana_dashboard_monitoring)
  * [TTFT impact: Measurable performance gains](https://developers.redhat.com/articles/2025/10/07/master-kv-cache-aware-routing-llm-d-efficient-ai-inference#ttft_impact__measurable_performance_gains)


In the era of large-scale AI inference, ensuring efficiency across distributed environments is essential. As workloads grow, so does the need for more intelligent scheduling and memory reuse strategies. Enter **llm-d** , a Kubernetes-native framework for scalable, intelligent LLM inference. One of its most powerful capabilities is **KV cache aware** **routing** , which reduces latency and improves throughput by directing requests to pods that already hold relevant context in GPU memory.
In this blog post, we'll cover:
  * What KV cache aware routing is and why it matters
  * How llm-d implements this feature with External Processing Pod (EPPs), Gateway API Inference Extension, and intelligent routing
  * The key Kubernetes YAML assets that make it work
  * A test case that shows our latest 87.4% cache hit rate
  * Where to go to learn more and get started

What is llm-d?
## What is llm-d?
**llm-d** is an open source project that uses cloud-native patterns to manage large-scale LLM inference. It is a collaborative effort by IBM, Google, Red Hat, and the broader AI infrastructure community. The project introduces:
  * Disaggregated **prefill and decode** workloads
  * **Multi-model** and multi-tenant isolation
  * **Intelligent routing** via an External Processing Pod
  * And, crucially, **KV cache aware routing** for memory-efficient, low-latency inference

Stateless inference fails to reuse cache
## Stateless inference fails to reuse cache
In traditional deployments, even if KV caches are enabled inside the model server (like vLLM), the _gateway is unaware of the cache state_. This leads to:
  * Round-robin routing or explicit sticky sessions
  * Frequent cache misses
  * Repeated computation for common prefixes
  * Unnecessary GPU memory use


This breaks down under high concurrency or in workloads with large shared context (like retrieval-augmented generation, agentic, and templated inputs).
KV cache aware routing
## KV cache aware routing
llm-d enables **state-aware request scheduling** by introducing the **Gateway API Inference Extension (GAIE)** with an EPP. This high-performance system, shown in Figure 1, makes intelligent routing decisions based on KV cache awareness. The key components include:
  * An **External Processing Pod (EPP) for the GAIE** that orchestrates intelligent pod scoring for optimal cache utilization
  * An **in-memory caching system** that tracks cache state across vLLM pods without external dependencies
  * A **pod discovery and labeling system** that automatically identifies and monitors decode service endpoints
  * A **session-aware routing algorithm** that maintains request consistency for optimal cache reuse
  * A **prefix-aware scoring system** that intelligently routes requests based on prompt similarity and cache warmth


The result is an advanced scheduler that routes requests to pods most likely to have relevant cached content. This dramatically reduces inference times and GPU load.
Figure 1: Complete KV cache aware routing architecture, showing the flow from client requests through EPP intelligent routing to decode pods with Gateway API Inference Extension coordination. Example deployment
## Example deployment
This section provides a practical guide to deploying and configuring KV cache aware routing with llm-d.
Prerequisites
## Prerequisites
To follow this guide, you should have:
  * [Red Hat OpenShift](https://developers.redhat.com/products/openshift/overview) or Kubernetes with GPU-enabled nodes and NVIDIA GPU Operator
  * Istio 1.27.0+ or KGateway installed (required for Envoy features)
  * Gateway API CRDs installed (standard + inference extension)
  * [llm-d infrastructure installed using the official community Helm chart](https://llm-d.ai/docs/guide/Installation/prerequisites)
  * A Hugging Face token (for downloading models)

Official community Helm chart approach
## Official community Helm chart approach
The implementation in this example uses the official llm-d community Helm chart, which automatically provisions:
  * **Infrastructure Gateway** : Gateway with proper configuration
  * **EPP** : External Processing Pod
  * **Service discovery** : Automatic discovery of decode services via label selectors

InferencePool and InferenceModel (auto-created by Helm chart)
## InferencePool and InferenceModel (auto-created by Helm chart)
The EPP automatically discovers and manages inference pools based on service labels. For automatic discovery, the decode service must have the following label:

```
llm-d.ai/inferenceServing: "true"
```
KV cache indexer : Deep architecture dive
## KV cache indexer : Deep architecture dive
The **KV cache indexer** is the core of llm-d's intelligent routing system. It maintains a global, near-real-time view of KV cache block locality across your vLLM-based decode and prefill pods.
Core architecture components
## Core architecture components  
| Component  | Purpose  | Implementation  |  
| --- | --- | --- |  
| `kvcache.Indexer`  | Main orchestrator handling scoring requests  | Coordinates all internal modules  |  
| `kvevents.Pool`  | Ingests KV cache events from vLLM pods  | Sharded ZMQ worker pool for real-time event processing  |  
| `kvblock.Index`  | Core data store mapping block hashes to pods  | In-memory two-level LRU cache for sub-millisecond lookups  |  
| `tokenization.PrefixStore`  | Caches tokenized prompt prefixes  | LRU cache avoiding expensive re-tokenization  |  
| `kvblock.TokenProcessor`  | Converts tokens into KV block keys  | Chunking and hashing algorithm matching vLLM exactly  |  
| `kvblock.Scorer`  | Scores pods based on cache hit sequences  | Longest consecutive prefix matching strategy  |  
The read path: Intelligent pod scoring
## The read path: Intelligent pod scoring
When a router needs to select the best pod for a new prompt, the **read path** finds the pod with the most extended sequence of relevant cached KV blocks:
  1. **Token retrieval** : Check the `PrefixStore` for the most extended cached token sequence for the prompt prefix.
  2. **Key generation** : Convert tokens into deterministic KV block keys that match vLLM's internal logic.
  3. **Index lookup** : Query the `kvblock.Index` to find which pods have the consecutive blocks.
  4. **Scoring** : Rank each pod based on consecutive matching blocks from the start of the prompt.
  5. **Response** : Return scored pod rankings to the router.


**Key insight** : First-time prompts might return empty results while background tokenization occurs, but common prompts achieve sub-millisecond scoring.
The write path: Real-time cache tracking
## The write path: Real-time cache tracking
The **write path** keeps the index synchronized with actual vLLM pod cache states:
  1. **Event publication** : vLLM pods publish cache events (`BlockStored, BlockRemoved`) via ZMQ.
  2. **Message reception** : Events parsed by topic format: `kv@pod-id@model`
  3. **Sharded processing** : Pod ID is hashed to ensure ordered processing per pod.
  4. **Event decoding** : The worker decodes msgpack payloads containing event batches.
  5. **Index updates** : Apply cache changes to the in-memory `kvblock.Index`.

Component configuration details
## Component configuration details
EPP configuration (deployed via official Helm chart):

```
# plugins.yaml configuration
apiVersion: v1
kind: ConfigMap
metadata:
  name: llm-d-gaie-epp-config
  namespace: llm-d
data:
  plugins-v2.yaml: |
    plugins:
      - name: "cache-aware-router"
        type: "external_processor"
        config:
          discovery:
            label_selector: "llm-d.ai/inferenceServing=true"
          cache:
            type: "in-memory-lru"
            max_size: 10000
          routing:
            algorithm: "prefix-aware"
            session_affinity: true

```
vLLM prefix caching configuration
## vLLM prefix caching configuration
Each vLLM pod is configured for optimal prefix caching performance:

```
args:
  - "--enable-prefix-caching"             # Enable KV-cache prefix reuse
  - "--block-size=16"                     # Optimal block size for cache efficiency
  - "--gpu-memory-utilization=0.7"        # Reserve memory for cache storage
  - "--max-model-len=4096"                # Match expected prompt lengths
  - "--kv-cache-dtype=auto"               # Automatic cache data type optimization
env:
  - name: CUDA_VISIBLE_DEVICES            # GPU assignment for cache isolation
    value: "0"

```
EnvoyFilter for EPP integration
## EnvoyFilter for EPP integration
Enables the EPP to intercept and route requests based on intelligent pod scoring:

```
name: envoy.filters.http.ext_proc
typed_config:
  grpc_service:
    envoy_grpc:
      cluster_name: epp-ext-proc-cluster  # Cluster pointing to EPP service
  processing_mode:
    request_header_mode: SEND     # Send request headers for routing analysis
    response_header_mode: SEND    # Send response headers for session tracking
    request_body_mode: STREAMED   # Stream request bodies for prompt analysis
  failure_mode_allow: true        # Continue routing if EPP unavailable
  message_timeout: 30s            # Allow time for intelligent scoring
```
Test case and results
## Test case and results
To validate that KV cache aware routing was functioning correctly, we designed a Tekton pipeline that simulated a typical usage pattern: multiple requests with shared prefixes, such as repeated user prompts or template-based documents.
Monitored signals:
  * EPP logs for intelligent routing decisionsEPP and vLLM Metrics for prefix cache hits
  * Grafana dashboard for comprehensive visibility


Outstanding performance metrics:
  * **Total queries:** 4,776
  * **Total cache hits:** 4,176
  * **Cache hit rate:** 87.4% (improved from previous 86%)


Traffic distribution analysis:
  * **Primary pod** : 4,772 queries (99.92% of traffic), 87.5% cache hit rate
  * **Secondary pods** : Only 4 queries total (0.08% failover)


These results demonstrate a KV cache aware routing system with Gateway API Inference Extension, in-memory caching, and intelligent EPP routing for maximum cache utilization.
Grafana dashboard monitoring
## Grafana dashboard monitoring
To provide comprehensive observability into the KV cache aware routing performance, we used Grafana dashboards that visualize key metrics in real time (Figure 2).
Figure 2: Grafana dashboard showing cache hit rates, request distribution, and system performance metrics during our latest 87.4% cache hit rate test.
Key dashboard metrics displayed:
  * **Cache hit rate timeline** : Real-time visualization of cache effectiveness across all decode pods
  * **Request distribution** : Traffic routing patterns showing session affinity in action
  * **Pod-level performance** : Individual decode pod cache statistics and GPU utilization
  * **Latency metrics** : Response time improvements from cache hits versus cache misses
  * **System health** : Overall cluster performance and resource utilization


The dashboard confirms our latest production results:
  * Session affinity concentrated 99.92% of requests to the primary warm pod (exceptional stickiness).
  * Cache hit rates achieved 87.4% overall, with 87.5% on the primary pod.
  * GPU memory utilization stayed optimal at 90%.
  * Response latencies showed significant improvement for cache-hit requests with sub-400 ms times.


This visual monitoring validates that the KV cache aware routing system performs as designed and delivers measurable benefits in both efficiency and performance.
TTFT impact: Measurable performance gains
## TTFT impact: Measurable performance gains
One of the most immediate and noticeable benefits of KV cache aware routing is the dramatic improvement in **Time To First Token (TTFT)**. Here's how cache hits directly translate to faster inference.
The following table compares baseline versus cache-aware performance.  
| Scenario  | Without cache routing  | With KV cache routing  | Improvement  |  
| --- | --- | --- | --- |  
| Cold inference  | 2,850 ms TTFT  | 2,850 ms TTFT  | Baseline  |  
| Warm cache hit  | 2,850 ms TTFT (worst case)  | 340 ms TTFT  | 88% faster  |  
Why this matters
## Why this matters
The 87.4% cache hit rate translates into tangible business value, impacting several key areas.
### Cost savings
A 70% reduction in compute time for repeated prompts means 70% fewer GPU-hours billed. For a cluster running 10 GPUs at $2 per hour, that's $336 saved per day on redundant computation.
Additionally, cache hits use 90% less energy than full inference, significantly reducing cloud costs.
### User experience
Users see sub-second response times for cached prompts versus 3–5 seconds for cold inference.
This higher throughput also means you can support 3 times more concurrent users with the same hardware.
### Key use cases
Enterprise use cases where this shines:
  * **RAG pipelines** : Document chunks get cached, making follow-up questions instant.
  * **Customer support** : Common queries hit the cache, and agents get faster responses.
  * **Code generation** : Template-based prompts reuse cached context.
  * **Multi-tenant SaaS** : Shared prompt patterns benefit all users.

Conclusion
## Conclusion
KV cache aware routing with llm-d represents a significant leap forward in optimizing large language model inference. By intelligently directing requests to pods with existing KV cache content, llm-d reduces latency, improves throughput, and lowers operational costs. The demonstrated 87% cache hit rate and 88% faster TTFT for warm cache hits underscore the real-world impact of this technology.
For enterprises using LLMs in demanding scenarios like RAG, customer support, and code generation, llm-d's KV cache aware routing provides a robust, scalable, and highly efficient solution for maximizing the value of their AI infrastructure.
Learn more:
  * [Project code and performance test on GitHub](https://github.com/cnuland/hello-chris-llm-d)
  * [llm-d KV Cache Manager Architecture](https://github.com/llm-d/llm-d-kv-cache-manager/blob/main/docs/architecture.md)
  * [llm-d operator quick start](https://llm-d.ai/docs/guide/Installation/prerequisites)
  * [vLLM documentation](https://docs.vllm.ai)


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


