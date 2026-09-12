# Search plan — llmd-router-nonk8s-backends-54b774 (executed 2026-09-11)

| Atomic item | Search query | Type | Lens | Target |
|---|---|---|---|---|
| Sub-Q1 components | llm-d inference scheduler endpoint picker architecture github | web | breadth | factual |
| Sub-Q1 | llm-d release notes 2026 inference scheduler | web | breadth | recent |
| Sub-Q1 | llm-d v0.8 OR v0.9 release endpoint discovery router rename | web | breadth | recent |
| Sub-Q1 | llm-d inference payload processor external model providers routing | web | breadth | entity |
| Sub-Q2 discovery today | gateway api inference extension endpoint picker protocol x-gateway-destination-endpoint ext_proc | web | depth | canonical (proposal 004) |
| Sub-Q2 | gateway-api-inference-extension data layer proposal pluggable data sources endpoint discovery | web | depth | canonical (proposal 0683, datalayer pkg) |
| Sub-Q2 | gateway-api-inference-extension InferencePool EndpointSlice or external IP endpoints issue | web | depth | code/issues |
| Sub-Q3 seams | llm-d-inference-scheduler plugins filter scorer picker EndpointPickerConfig | web | breadth | factual |
| Sub-Q3 | llm-d EndpointDiscovery interface file discovery plugin custom discovery Consul | web | depth | code |
| Sub-Q3 | llm-d-router proposal endpoint discovery plugin interface github docs/proposals | web | depth | canonical |
| Sub-Q4 upstream | gateway-api-inference-extension issue endpoints outside cluster non-kubernetes backends | web | depth | issues |
| Sub-Q4 | endpoint picker EPP standalone mode without InferencePool | web | breadth | docs |
| Sub-Q4 | llm-d-router moved endpoint picker code from GIE fork migration | web | depth | repo history |
| Sub-Q4 | InferencePool multi-cluster proposal gateway api inference extension | web | breadth | recent |
| Sub-Q4 | gateway-api-inference-extension proposal endpoint discovery abstraction decouple pods datastore | web | depth | canonical |
| Sub-Q5 backend types | llm-d without kubernetes bare metal VM deployment | web | breadth | factual |
| Sub-Q5 | llm-d Slurm Ray integration router endpoint discovery RL rollout | web | breadth | entity |
| Sub-Q5 | gateway api inference extension model server protocol metrics requirements vLLM SGLang TGI | web | depth | canonical (proposal 003) |
| Sub-Q5 | vLLM KV cache events ZMQ publisher kv-events-config remote network security | web | depth | code |
| Sub-Q5 | llm-d kv-cache-manager kvevents ZMQ vLLM precise prefix cache aware routing | web | breadth | factual |
| Sub-Q5 | llm-d routing sidecar prefill decode disaggregation x-prefiller-host-port | web | breadth | factual |
| Sub-Q6 strategies (k8s shim) | Kubernetes selectorless Service manual EndpointSlice external VM endpoints | web | breadth | docs |
| Sub-Q6 (virtual pods) | virtual kubelet Slurm HPC interLink GPU pods offload | web | breadth | alt strategy |
| Sub-Q6 (proxy) | agentgateway standalone inferenceRouting destinationMode passthrough EPP | web | depth | code/docs |
| Sub-Q6 (proxy) | Envoy ext_proc ORIGINAL_DST cluster endpoint picker standalone envoy config | web | depth | docs |
| Sub-Q6 (external providers) | inference gateway route to external OpenAI provider and self-hosted InferencePool fallback | web | breadth | alt strategy |
| Sub-Q7 compare | vllm-project router Rust vs llm-d inference scheduler comparison | web | adversarial | competing router |
| Sub-Q7 | NVIDIA Dynamo KV router vs llm-d vs AIBrix vs SGLang router comparison | web | adversarial | competing frameworks |
| Sub-Q7 | llm-d limitations criticism complexity kubernetes-only | web | adversarial | criticism |
| Sub-Q7 | ext_proc endpoint picker latency overhead inference gateway performance problems | web | adversarial | failure modes |
| Sub-Q8 risks | llm-d v0.9 router high availability endpoint picker single point of failure | web | adversarial | failure modes |
| Sub-Q8 | llm-d file discovery limitations dynamic endpoints hot reload health checks | web | adversarial | limitations |
| Sub-Q2/Q7 | LLM inference request routing KV cache aware scheduling | academic (scholar) | depth | canonical (Mooncake etc.) |

Adversarial searches executed: 6 (vLLM Router vs llm-d, Dynamo/AIBrix/SGLang comparison, llm-d criticism, ext_proc overhead,
EPP HA/SPOF, file-discovery limitations) + adversarial URLs (Azure AGC "external providers out of scope", agentgateway
fail-open issue #1661).

## Gap check vs coverage matrix
- "use llm-d components" → Sub-Q1 rows ✔
- "as much as possible" → Sub-Q7 compare rows ✔ (reuse-level dimension)
- "customize router" → Sub-Q2/Q3 rows + proxy rows ✔
- "backends other than k8s pods" → Sub-Q5 + Sub-Q6 rows (bare metal, Slurm, Ray, VMs, external providers, remote clusters, virtual pods) ✔
- GIE upstream dependency → Sub-Q4 rows ✔
No gaps.

## Early signal (shapes the rest of the pipeline)
llm-d v0.8 (2026) introduced an `EndpointDiscovery` plugin interface in the EPP (Kube Discovery vs File Discovery) and
renamed components (llm-d-inference-scheduler → llm-d-router-endpoint-picker; routing sidecar → llm-d-router-disagg-sidecar).
EPP core code moved from GIE into llm-d-router; GIE now hosts only InferencePool API + EPP protocol.
