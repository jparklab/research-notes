---
title: Disaggregated Serving | llm-d
id: disaggregated-serving-llm-d
tags:
- llm-d
- pd-disaggregation
- architecture
- epp
- nixl
- rdma
- sglang
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:29.962293Z'
updated: '2026-09-12T03:37:37.572598Z'
source: https://llm-d.ai/docs/architecture/advanced/disaggregation
source_domain: llm-d.ai
fetched_at: '2026-09-11T19:44:29.939563Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Current (v0.9, as of the fetch banner referencing the v0.9 ''hardened for
  scale'' release) llm-d architecture doc on P/D disaggregated serving. Explains two
  required components: (1) Request Flow Orchestration -- EPP selects a P/D worker
  pair via a disagg-profile-handler that runs a decode-profile first, consults a ''decider''
  on how much of the prompt is cached on D, and only invokes the prefill-profile (selecting
  a P endpoint too) if the uncached suffix is large; both P and D endpoints belong
  to the SAME InferencePool, distinguished by the llm-d.ai/role label (values: prefill,
  decode, prefill-decode) which the prefill-filter/decode-filter plugins use to filter
  candidates -- and (2) Efficient KV Transfer, typically over RDMA via NIXL. The Routing
  Proxy Sidecar runs in each decode pod and reads the x-prefiller-host-port header
  (one or more prefill pod addresses, comma-separated/multi-value) set by the proxy/EPP;
  if present it executes the disaggregated path (send prefill request, capture KV
  transfer metadata, forward enriched request to local decoder), otherwise it proxies
  decoder-only. It documents TWO KV-transfer protocol implementations the sidecar
  must speak: vLLM''s nixlv2 (default) -- a two-phase sequential protocol using kv_transfer_params
  and max_tokens=1, with fallback to decoder-only on prefiller server errors (5xx)
  but no retry on client errors -- and SGLang''s sglang connector, a concurrent protocol
  using bootstrap_host/bootstrap_port/bootstrap_room parameters where prefill fires
  asynchronously (goroutine with context.WithoutCancel) while decode runs synchronously,
  coordinating out-of-band via the bootstrap room. NIXL is the only KV-transfer engine
  llm-d currently focuses on; it needs RDMA (InfiniBand or RoCE, both supported via
  UCX and UCCL backends; TCP-X and EFA supported by UCCL; libfabric supports IB only)
  for efficient transfer, falls back to slow TCP without RDMA (dev/test only), and
  NVLink transfer works only for local development since ''it cannot cross pod boundaries''
  in Kubernetes deployments. With GPUDirect RDMA, GPU memory is registered directly
  with the NIC so transfers bypass the CPU.'
utility_score: 14.0
---

[Skip to main content](https://llm-d.ai/docs/architecture/advanced/disaggregation#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
Version: v0.9 (latest)
On this page
## Functionality[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#functionality "Direct link to Functionality")
Disaggregated serving separates the **prefill** and **decode** stages of LLM inference onto different model server instances, enabling:
  * **Specialization of P and D** - LLM inference is composed of two distinct phases of inference - prefill (FLOPs-bound) and decode (memory bandwidth-bound). Disaggregation enables specialization, e.g. using a larger TP for the memory-bound decoding phase while a smaller TP for the computation-bound prefill phase.
  * **Avoidance of Request Interference** - For long context requests, prefills can slow down processing of existing requests in the decode phase. Separating the prefill phase of these long requests into dedicated prefill instances allows the ongoing decoding requests to be efficiently processed without being blocked by these long prefills, improving quality-of-service.
  * **Compatibility with DP/EP** - For DP/EP deployments of Mixture of Experts models, disaggregated serving is essential to avoid pipeline bubbles and leveraging the specialized "MaskedGEMM" format for decode.


An implementation of disaggregated serving requires two key components:
  * **Request Flow Orchestration** - select and route the requests to the correct prefill and decode pods
  * **Efficient KV Transfer** - transfer the KV cache from the P instance to the D instance, typically over RDMA


note
Disaggregated Serving requires high performance (RDMA) interconnects between nodes for efficient KV transfer. Without RDMA, NIXL falls back to TCP for transfer which is not efficient and should only be used for testing and development.
## Request Flow Orchestration[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#request-flow-orchestration "Direct link to Request Flow Orchestration")
llm-d's EPP supports the concept of P/D disaggregation by selecting a prefill and decode worker pair, with the following request flow:

```
%% sequence diagram
Prefill Worker
Decode Worker
Routing Sidecar
EPP
Proxy
Client
Same Pod
Request
Run EPP protocol
Selects P Worker and D Worker
Request with max_tokens=1, do_remote_decode=True
Run prefill
Response with KVTransferParams
Request with KVTransferParams and do_remote_prefill=True
Pull KV Cache (NIXL RDMA)
Release KV Cache
Run decodes
Response
```

Next we will discuss the design of the EPP and Routing Sidecar for disaggregated serving.
### EPP[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#epp "Direct link to EPP")
The llm-d EPP supports disaggregation via the `disagg-profile-handler`.
note
Rather than hardcoding a single scheduling algorithm, the EPP delegates execution to one or more `Profile Handlers`, each of which represents a complete scheduling strategy. They can be thought of as "the dispatcher", which maps each incoming inference request to the right scheduling strategy before the scorers and pickers do their work of selecting the actual endpoint. By default, llm-d uses the `single-profile-handler` for simple aggregated serving.
When configured with `disagg-profile-handler`, the EPP processes requests in the following steps:
  * `proxy` forwards request metadata to the EPP.
  * `disagg-profile-handler` runs the `decode-profile`, which executes the `filter`, `score`, `pick` scheduler profile to select D endpoint.
  * `disagg-profile-handler` consults the `decider` — given how much of the prompt is cached on D, should this request run disagg?
  * If `no`: `disagg-profile-handler` returns only the D endpoint to the `proxy`
  * If `yes` (large uncached suffix), `disagg-profile-handler` also runs the `prefill-profile`, which executes the `filter`, `score`, `pick` scheduler profile to select the P endpoint and returns both the P and D endpoints to the proxy.


The flow looks like this:

```
%% sequence diagram
EPP
Prefill-Profile
Decode-Profile
Decider
Disagg-Profile-Handler
Proxy
opt
[do-pd=FALSE]
Request
Filter, Score, Pick
D Endpoint
Num uncached tokens on D
[do-pd] BOOL
P Endpoint
D, P Endpoint
```

In this way, disaggregated serving functionality composes with the existing set of scheduling functionality, enabling use of the existing set of scorers for prefix and load aware routing in the disaggregated setting.
Note that both the prefill and decode endpoints are part of one `InferencePool`. The `decode-profile` and `prefill-profile` are responsible for selecting only D workers or P workers in the `filter` step. llm-d uses the label `llm-d.ai/role` with the following values to filter:
  * `prefill` → prefill-only pods
  * `decode` → decode-capable pods
  * `prefill-decode` → pods capable of both prefill and decode


note
It is possible to override the default labels by configuring the `EndpointPickerConfig` to use the generic by-label filter plugin instead of the `prefill-filter` / `decode-filter`. TODO: provide an example of this.
### Routing Proxy Sidecar[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#routing-proxy-sidecar "Direct link to Routing Proxy Sidecar")
The Routing Proxy is deployed as a sidecar in each decode pod, with a two-fold role:
  * Facilitate the multi-step inference request
  * Mutate the requests to follow each model server's KV transfer protocol


#### Request Flow[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#request-flow "Direct link to Request Flow")
When a request arrives, the sidecar inspects a routing header set by the proxy:  
| Header  | Purpose  |  
| --- | --- |  
| `x-prefiller-host-port`  | One or more prefill pod addresses (comma-separated or multi-value)  |  
Based on which headers are present, the sidecar selects one of two execution paths:
  1. **Prefiller → Decoder (P/D)** — The standard disaggregated path. The request is sent to a remote prefiller, KV transfer metadata is collected, and the enriched request is forwarded to the local decoder.
  2. **Decoder-only** — No routing headers present; the request is proxied directly to the local model server instance.


All non-completion routes (`GET /health`, and any other path) pass through to the decoder unchanged.
#### KV Transfer Protocol[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#kv-transfer-protocol "Direct link to KV Transfer Protocol")
vLLM and SGLang use slightly different protocols for KV Transfer between the P and D instance, inserting additional parameters in the body of the requests to facilitate the transfer:
  * **vLLM** (`nixlv2`, default) — A two-phase sequential protocol. The sidecar sends a prefill request with `kv_transfer_params` containing remote-decode metadata, and `max_tokens=1` to suppress output. It captures the KV transfer parameters from the prefiller's response and injects them into the decode request before forwarding it to the local decoder. If the prefiller returns a server error, the sidecar falls back to decoder-only mode (client errors are not retried).
  * **SGLang** (`sglang`) — Uses a concurrent prefill/decode model. Instead of waiting for prefill to complete, the sidecar injects bootstrap coordination parameters (`bootstrap_host`, `bootstrap_port`, `bootstrap_room`) into both requests, fires the prefill asynchronously in a goroutine (with `context.WithoutCancel` to prevent premature cancellation), and immediately sends the decode request synchronously. The decoder and prefiller coordinate KV transfer out-of-band via the bootstrap room.


## Efficient KV Transfer[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#efficient-kv-transfer "Direct link to Efficient KV Transfer")
In addition to the **Request Flow Orchestration** which coordinates the metadata and RPC calls to each instance, efficient **KV Transfer** (which moves the KVs from the P instance to the D instance) is critical to a high performance disaggregated deployment.
vLLM and SGLang both support multiple KV transfer engines - in llm-d we currently focus on NIXL.
### NIXL Integration[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#nixl-integration "Direct link to NIXL Integration")
[NVIDIA Inference Transfer Library (NIXL)](https://github.com/ai-dynamo/nixl) accelerates point to point communications in AI inference frameworks.
NIXL provides a standardized API for transferring memory between remote instances offering multiple backends (e.g. UCX, UCCL, libfabric) each of which support multiple underlying transports (NVLink, RoCE, IB, EFA, etc).  
| IB  | RoCE  | TCP-X  | EFA  |  
| --- | --- | --- | --- |  
| **UCX**  | ✓  | ✓  |  
| **UCCL**  | ✓  | ✓  | ✓  | ✓  |  
| **libfabric**  | ✓  |  
note
UCX also supports transfer via TCP and NVLINK. TCP is extremely slow and is targeted for local development. NVLINK transfer is used for local development and cannot be used in Kubernetes deployments as it cannot cross pod boundaries.
### Direct KV Cache Transfer[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#direct-kv-cache-transfer "Direct link to Direct KV Cache Transfer")
vLLM and SGLang both reserve RAM ahead of time for KV cache memory. NIXL directly registers this KV cache memory and transfers the data directly between the KV caches. This avoids the need for additional buffers and memory management. With GPUDirect RDMA enabled (GPU memory registered with NIC), the transfers bypass the CPU, enabling high throughput, low latency transfers.

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

## Operations[​](https://llm-d.ai/docs/architecture/advanced/disaggregation#operations "Direct link to Operations")
Disaggregated serving introduces operational considerations that are specific to each model server's KV transfer mechanism: dynamic connections, request cancellation, fault tolerance, and rollouts. See the engine-specific operations guides:
  * [Disaggregated Serving: Operations (vLLM)](https://llm-d.ai/docs/operations/disaggregation/vllm)
  * [Disaggregated Serving: Operations (SGLang)](https://llm-d.ai/docs/operations/disaggregation/sglang)


  * [Request Flow Orchestration](https://llm-d.ai/docs/architecture/advanced/disaggregation#request-flow-orchestration)
    * [Routing Proxy Sidecar](https://llm-d.ai/docs/architecture/advanced/disaggregation#routing-proxy-sidecar)
  * [Efficient KV Transfer](https://llm-d.ai/docs/architecture/advanced/disaggregation#efficient-kv-transfer)
    * [Direct KV Cache Transfer](https://llm-d.ai/docs/architecture/advanced/disaggregation#direct-kv-cache-transfer)


