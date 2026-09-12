---
title: llm-d/guides/precise-prefix-cache-routing/README.md at main · llm-d/llm-d ·
  GitHub
id: llm-dguidesprecise-prefix-cache-routingreadmemd-at-main-llm-dllm-d-github
tags:
- llmd-router-nonk8s-backends
- kv-cache-indexer
- zmq-kv-events
- pod-discovery
- epp-router
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:43:25.127660Z'
updated: '2026-09-12T03:37:37.531899Z'
source: https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md
source_domain: github.com
fetched_at: '2026-09-11T19:43:25.104835Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d GitHub guide (guides/precise-prefix-cache-routing/README.md, commit
  cab4b7d, Aug 2026) is the authoritative, current-HEAD spec for precise KV-cache-aware
  routing and answers the endpoint-identity/discovery questions directly. Each vLLM/SGLang
  pod is launched with ''--kv-events-config {...,"publisher":"zmq","endpoint":"$(KV_EVENTS_ENDPOINT)","topic":"kv@$(POD_IP):$(POD_PORT)@<model>"}''
  and KV_EVENTS_ENDPOINT=tcp://*:5556 — i.e. the pod BINDS its own ZMQ PUB socket
  and the ZMQ topic is keyed by literal Pod IP:port plus model name, not by Kubernetes
  pod name/UID. The GPU vLLM backend (v0.26.0+) additionally binds a ZMQ ROUTER replay
  socket on port 5559 and retains the last 10,000 batches in an in-memory replay buffer
  for index recovery after a router restart. Router-side: ''pod-discovery (kvEventsConfig.discoverPods:
  true) registers the precise-prefix-cache-producer as an extractor on the data-layer
  endpoint-notification-source, so each router replica installs a ZMQ subscriber per
  model server pod independently'' — i.e. discovery is push-registration-then-pull-connect:
  the router discovers pod IPs (implicitly via Kubernetes endpoint/pod watch given
  ''discoverPods'') and then each EPP replica opens its own outbound ZMQ SUB connection
  to every discovered pod''s bound PUB endpoint. All replicas converge to the same
  index independently, which the doc calls ''HA-safe... each replica converges independently
  via pod-discovery.'' Tokenization for exact matching goes through a separate render
  Service that fronts the model-server pods'' vLLM ''/v1/*/render'' endpoint (not
  through the ZMQ path) — render pods deliberately omit the llm-d.ai/guide label so
  the EPP does not try to subscribe to a nonexistent KV-event socket on them, confirming
  the EPP treats ''routable model server'' status (and hence ZMQ-subscription eligibility)
  as driven by that Kubernetes label/selector rather than by an independent config
  list. States router runs single-replica by default because in-flight token load
  accounting is local to each EPP process (non-HA state), while the KV index itself
  is HA-safe across multiple router replicas.'
utility_score: 14.0
---

[Skip to content](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 761 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# README.md
Copy path
More file actions
More file actions
## Latest commit
[Set tags on](https://github.com/llm-d/llm-d/commit/cab4b7ded6094b3a25a8e067752eb7f0649aa640) [branch back to "main" (](https://github.com/llm-d/llm-d/commit/cab4b7ded6094b3a25a8e067752eb7f0649aa640)[#2316](https://github.com/llm-d/llm-d/pull/2316)[)](https://github.com/llm-d/llm-d/commit/cab4b7ded6094b3a25a8e067752eb7f0649aa640)
Open commit detailssuccess
Aug 18, 2026
[cab4b7d](https://github.com/llm-d/llm-d/commit/cab4b7ded6094b3a25a8e067752eb7f0649aa640) · Aug 18, 2026
## History
[History](https://github.com/llm-d/llm-d/commits/main/guides/precise-prefix-cache-routing/README.md)
Open commit details
History
362 lines (262 loc) · 21.7 KB
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

362 lines (262 loc) · 21.7 KB
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Precise Prefix Cache Routing
## Overview
This guide routes requests on precise per-pod KV-cache state rather than request-traffic heuristics. Each vLLM pod publishes [KV-cache events](https://github.com/vllm-project/vllm/issues/16669) over ZMQ; the router subscribes, builds an index keyed by block hash, filters candidates to the pods where an incoming request's prefix is already resident, and picks the least token-loaded pod within that set.
The routing decision combines precise cache knowledge with token-based load balancing:
  * **Precise prefix-cache aware** — the [precise-prefix-cache-producer](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache) indexes real KV-block events from vLLM and publishes the exact resident-block fraction. The `prefix-cache-affinity-filter` reads it via `prefixMatchInfoProducerName` to keep each prefix group on its cache-warm endpoints, gated by a calibrated `peakPrefillThroughput` so saturated endpoints are bypassed. Indexer internals (event ingestion, block hashing, dual-key design) are documented in [llm-d-kv-cache architecture](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/architecture.md).
  * **Token-load aware** — the `token-load-scorer` (fed by the `inflight-load-producer`) picks the least token-loaded endpoint within the filtered set, balancing by queued prefill work rather than request counts.


## Default Configuration  
| Parameter  | Value  |  
| --- | --- |  
| Model  |  
| Replicas  | 8 (reduce for smaller fleets — see notes below)  |  
| Tensor Parallelism  | 2  |  
| GPUs per replica  | 2  |  
| Total GPUs  | 16  |  
| vLLM `--block-size`  | 64 (must match the `precise-prefix-cache-producer`'s `tokenProcessorConfig.blockSizeTokens`)  |  
### Supported Hardware Backends  
| Backend  | Directory  | Default model  | Notes  |  
| --- | --- | --- | --- |  
| NVIDIA GPU  | `modelserver/gpu/vllm/`  | Qwen/Qwen3-32B  | Default configuration  |  
| NVIDIA GPU (SGLang)  | `modelserver/gpu/sglang/`  | Qwen/Qwen3-32B  | SGLang; `--page-size=64` matches the producer's `blockSizeTokens`; requires `render/standalone/`  |  
| AMD GPU  | `modelserver/amd/vllm/`  | Qwen/Qwen3-32B  | AMD GPU  |  
| Intel XPU  | `modelserver/xpu/vllm/`  | Qwen/Qwen3-0.6B  | CI-sized; update router `modelName` for real use  |  
| Google TPU v6e  | `modelserver/tpu/v6/vllm/`  | Qwen/Qwen3-32B  | GKE TPU  |  
| Google TPU v7  | `modelserver/tpu/v7/vllm/`  | Qwen3-Coder-480B-FP8  | GKE TPU  |  
| CPU  | `modelserver/cpu/vllm/`  | Llama-3.2-3B-Instruct  | CI-sized  |  
Note
Some hardware variants use reduced configurations (fewer replicas, smaller models) to enable CI testing for compatibility and regression checks.
[!NOTE] The `token-producer` `modelName` in [`router/precise-prefix-cache-routing.values.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/router/precise-prefix-cache-routing.values.yaml) must match the model the overlay deploys. With the default render overlay the render call lands on the model servers themselves, so a mismatch is rejected outright rather than silently scoring against the wrong tokenizer.
[!NOTE] The `gpu/vllm/` overlay defaults to 8 replicas to match the canonical 16×H100 benchmark. For smaller fleets (or quick smoke tests), reduce `replicas` in the deployment patch (`modelserver/gpu/vllm/patch-vllm.yaml`) before applying.
[!NOTE] The router runs as a **single replica** by default: the `token-load-scorer`'s in-flight token accounting is local to each EPP process, so two active-active replicas would each see only half the per-endpoint load and mis-gate the affinity filter. The precise KV index itself is HA-safe (each replica converges independently via pod-discovery), so active-active HA (`--set router.epp.replicas=2`) can return if shared in-flight state lands upstream.
For wide-EP LWS deployments (multi-port DP model servers), use the [`wide-ep-lws` precise routing variant](https://github.com/llm-d/llm-d/blob/main/guides/wide-ep-lws/README.precise-prefix-cache-routing.md) instead of the manifests here.
## Prerequisites
  * Have the [proper client tools installed on your local system](https://github.com/llm-d/llm-d/blob/main/helpers/client-setup/README.md) to use this guide.
  * Checkout llm-d repo:



```
  export branch="main" # branch, tag, or commit hash
  git clone https://github.com/llm-d/llm-d.git  cd llm-d  git checkout ${branch}
```

  * Set the following environment variables:



```
export REPO_ROOT=$(realpath $(git rev-parse --show-toplevel))
source ${REPO_ROOT}/guides/env.sh
export GUIDE_NAME="precise-prefix-cache-routing"
export NAMESPACE="llm-d-${GUIDE_NAME}"
```

  * Install the Gateway API Inference Extension CRDs:



```
# GAIE_URL is automatically calculated from GAIE_VERSION at ${REPO_ROOT}/guides/env.sh
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/${GAIE_URL}/v1-manifests.yaml
```

  * Create a target namespace for the installation



```
kubectl create namespace ${NAMESPACE} --dry-run=client -o yaml | kubectl apply -f -
```

## Installation Instructions
### 1. Prepare HF Token
Create the `llm-d-hf-token` secret in the namespace. The router reads `HF_TOKEN` to reach gated tokenizers — Qwen/Qwen3-32B is public but the secret makes swapping in a gated model a no-op. See [helpers/hf-token.md](https://github.com/llm-d/llm-d/blob/main/helpers/hf-token.md) for the full helper.

```
export HF_TOKEN=your HuggingFace token
kubectl create secret generic llm-d-hf-token \
  --from-literal="HF_TOKEN=${HF_TOKEN}" \
  --namespace "${NAMESPACE}" \
  --dry-run=client -o yaml | kubectl apply -f -
```

### 2. Deploy the llm-d Router
#### Standalone Mode
This deploys the llm-d Router in the simple [Standalone Mode](https://github.com/llm-d/llm-d/blob/main/docs/architecture/core/router/proxy.md). The release name `${GUIDE_NAME}` is mandatory — the inference pool selector matches a guide label that pairs with this release.
Tokenization is served by a separate render Service, not a chart-injected EPP sidecar — the chart's `router.tokenizer` sidecar is off by default, and the `token-producer` plugin points at that Service.

```
helm install ${GUIDE_NAME} \
  ${ROUTER_STANDALONE_CHART} \
  -f ${REPO_ROOT}/guides/recipes/router/base.values.yaml \
  -f ${REPO_ROOT}/guides/${GUIDE_NAME}/router/${GUIDE_NAME}.values.yaml \
  -n ${NAMESPACE} --version ${ROUTER_CHART_VERSION}
```

The release name `${GUIDE_NAME}` is mandatory for standard deployments — the inference pool selector matches a guide label that pairs with this release.
The render (tokenizer) Service the `token-producer` plugin calls is deployed separately, in [step 4](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md#4-deploy-the-render-tokenizer-service).
**Gateway Mode**
To use a Kubernetes Gateway managed proxy instead of the standalone Envoy sidecar, do **not** apply the standalone chart above. Instead:
  1. **Deploy a Kubernetes Gateway**. See [the gateway guides](https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/gateway) for step-by-step deployment of a Gateway named `llm-d-inference-gateway`.
  2. **Deploy the llm-d Router and HTTPRoute** via the `llm-d-router-gateway` chart with `httpRoute.create=true`:



```
export PROVIDER_NAME=istio   # options: none, gke, agentgateway, istio

helm install ${GUIDE_NAME} \
  ${ROUTER_GATEWAY_CHART} \
  -f ${REPO_ROOT}/guides/recipes/router/base.values.yaml \
  -f ${REPO_ROOT}/guides/recipes/router/features/httproute-flags.yaml \
  -f ${REPO_ROOT}/guides/${GUIDE_NAME}/router/${GUIDE_NAME}.values.yaml \
  --set provider.name=${PROVIDER_NAME} \
  -n ${NAMESPACE} --version ${ROUTER_CHART_VERSION}
```

### 3. Deploy the Model Server
Apply the Kustomize overlay for your backend (defaulting to NVIDIA GPU / vLLM):

```
export MODEL_SERVER=vllm # vllm | sglang
export INFRA_PROVIDER=base # base | gke
kubectl apply -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/modelserver/gpu/${MODEL_SERVER}/${INFRA_PROVIDER}/
```

### 4. Deploy the Render (Tokenizer) Service
The EPP `token-producer` plugin tokenizes prompts by calling vLLM's `/v1/*/render` endpoints. This guide serves that endpoint from a Service rather than a per-EPP-pod sidecar, so a single render pool is shared across EPP replicas and render capacity is decoupled from the EPP replica count.
`vllm serve` already exposes `/v1/*/render`, so the default overlay is a **Service with no pods of its own** : it selects the model server pods you just deployed and tokenizes on them.

```
kubectl apply -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/render/
```

Why this over a separate renderer pool: a dedicated pool is scheduled independently of the model servers, so as QPS climbs it saturates before they do and render latency — which sits inside TTFT, since every request is tokenized before it is routed — starts to dominate. Fronting the model servers instead makes render capacity scale with the fleet, which keeps latency more contained at higher QPS.
The trade-offs to weigh:
  * Tokenization CPU competes with serving on the same pods. The GPU overlay requests 8 CPU / limits 16 per replica; raise that if render latency climbs under load.
  * The render call is a synchronous hop to a GPU serving pod on the request path.
  * Only `Ready` endpoints receive traffic, so render calls are never sent to a pod still loading weights. Until the first model server is `Ready` the Service has no endpoints and `token-producer` calls fail — apply this after [step 3](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md#3-deploy-the-model-server), as ordered here.

**Dedicated renderer pool (required for SGLang)**
SGLang does not implement vLLM's render endpoints, so the SGLang overlay must instead run a dedicated, GPU-less `vllm launch render` pool (3 replicas). The same applies to any deployment where you would rather not spend model server CPU on tokenization. Apply this **instead of** the default overlay above — both publish the same Service name, so the router's `vllm.url` is unchanged either way:

```
kubectl apply -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/render/standalone/
```

Because this pool is GPU-less and engine-agnostic, it tokenizes with vLLM's tokenizer no matter which engine serves inference. It is configured for `Qwen/Qwen3-32B`; for another model, change the model argument in [`render/standalone/deployment.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/render/standalone/deployment.yaml) and the router `token-producer` `modelName` together. Scale it with `kubectl scale -n ${NAMESPACE} deploy/${GUIDE_NAME}-render --replicas=<N>`.
> [!NOTE] These render pods deliberately do **not** carry the `llm-d.ai/guide` label — that label is the InferencePool / model-server selector, and the EPP would otherwise treat render pods as routable model servers (and try to subscribe to their nonexistent KV-event socket).
### 5. (Optional) Enable Monitoring
  * Install the [Monitoring stack](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/setup.md).
  * To enable Prometheus monitoring on the llm-d router, add `-f ${REPO_ROOT}/guides/recipes/router/features/monitoring.values.yaml` during the [router installation step](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md#2-deploy-the-llm-d-router).
  * Deploy the monitoring resources for model servers:

```
kubectl apply -n ${NAMESPACE} -k ${REPO_ROOT}/guides/recipes/modelserver/components/monitoring
```



## Verification
### 1. Get the IP of the Proxy
#### Standalone Mode

```
export IP=$(kubectl get service ${GUIDE_NAME}-epp -n ${NAMESPACE} -o jsonpath='{.spec.clusterIP}')
```

**Gateway Mode**

```
export IP=$(kubectl get gateway llm-d-inference-gateway -n ${NAMESPACE} -o jsonpath='{.status.addresses[0].value}')
```

### 2. Send Test Requests
**Open a temporary interactive shell inside the cluster:**

```
kubectl run curl-debug --rm -it \
    --image=cfmanteiga/alpine-bash-curl-jq \
    --namespace="$NAMESPACE" \
    --env="IP=$IP" \
    --env="NAMESPACE=$NAMESPACE" \
    -- /bin/bash
```

**Send a completion request:**

```
curl -X POST http://${IP}/v1/completions \
    -H 'Content-Type: application/json' \
    -d 
        "model": "Qwen/Qwen3-32B",
        "prompt": "How are you today?"
 | jq
```

## Benchmarking
This guide uses [`llmdbenchmark`](https://github.com/llm-d/llm-d-benchmark) — the supported standard CLI for llm-d performance benchmarking.
In this example we will demonstrate how to run [`inference-perf`](https://github.com/kubernetes-sigs/inference-perf) with a shared-prefix synthetic workload against the stack you just deployed above (standalone or gateway mode). When orchestrating benchmarks via `llmdbenchmark`, the CLI automatically and transparently deploys a harness pod (`llmdbench-harness-launcher`) into your namespace. This pod is central to driving the workload, collecting the results, and tearing itself down when it's finished.
Important
**For more in-depth explanation and features for benchmarking llm-d guides, see[`helpers/benchmark.md`](https://github.com/llm-d/llm-d/blob/main/helpers/benchmark.md).**
The Benchmarking section below contains only the **precise-prefix-cache-routing-specific commands** needed to drive the stack you just deployed — for everything else (and especially when something goes wrong), start at [`helpers/benchmark.md`](https://github.com/llm-d/llm-d/blob/main/helpers/benchmark.md).
For even more details about benchmarking, see the actual repository: [`llm-d-benchmark` on GitHub](https://github.com/llm-d/llm-d-benchmark).
Tip
The command below runs this guide's **dedicated** benchmark profile, which is intentionally shaped to stress the prefix-cache routing decision under contention — and accordingly takes longer to complete. To run a simpler workload with fewer execution cycles first (useful for validating the path, image pulls, PVC binding, etc. before committing to a real run), pick a generic sample profile such as `shared_prefix_synthetic.yaml` from the catalog in [`helpers/benchmark.md` → Available workload profiles](https://github.com/llm-d/llm-d/blob/main/helpers/benchmark.md#available-workload-profiles) and substitute it for the `--workload` flag in the command below.
### 1. Install the `llmdbenchmark` CLI
Automatically clone the benchmark repository into `./llm-d-benchmark/` and create a virtualenv at `./llm-d-benchmark/.venv/` containing dependencies and its installation:

```
curl -sSL https://raw.githubusercontent.com/llm-d/llm-d-benchmark/main/install.sh | bash
```

Activate the `venv` and enter the repository directory - both are required: the `venv` puts `llmdbenchmark` on your PATH, and the repository directory contains the `workload/profiles/` and `config/specification/` files that orchestrate the benchmark:

```
cd llm-d-benchmark
source .venv/bin/activate
llmdbenchmark --version
```

Note
Subsequent `llmdbenchmark` commands in this section assume you are inside the `llm-d-benchmark` repo directory with the `venv` activated. If you open a new shell, re-run the two commands above.
### 2. Resolve the endpoint of the stack you just deployed
Set two variables so the rest of the section is topology-agnostic: the endpoint URL and the gateway class. The gateway class tells the CLI which deployment topology the cluster is actually running, without this, the CLI re-renders against the benchmark scenario's default values.
**Standalone Mode** (the default in this guide — no Kubernetes Gateway, EPP pod with an Envoy sidecar):

```
export ENDPOINT_URL="http://$(kubectl get service ${GUIDE_NAME}-epp -n ${NAMESPACE} -o jsonpath='{.spec.clusterIP}')"
export GATEWAY_CLASS=epponly # standalone mode
```

**Gateway Mode**

```
export ENDPOINT_URL="http://$(kubectl get gateway llm-d-inference-gateway -n ${NAMESPACE} -o jsonpath='{.status.addresses[0].value}')"

# Match whichever provider you used when deploying the gateway (e.g. istio, agentgateway, gke).
export GATEWAY_CLASS=istio
```

### 3. Run the benchmark profile for Precise Prefix Cache Routing
`guide_precise-prefix-cache-routing_1.yaml` is a **dedicated workload profile** shipped with `llm-d-benchmark` specifically for this guide — it reproduces the load ladder used to generate the [graphs at the bottom of this guide](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/README.md#benchmarking-reports) (rates 3 to 60 across 150 distinct prefix groups) and is shaped to highlight the strengths of precise prefix-cache routing by stressing the routing decision under contention.
Benchmark results are copied to the `workspace` directory that is specified by _you_ (or that is automatically generated when omitted from the cli) on the machine running the CLI. The workspace location is optional — by default the CLI auto-generates a timestamped workspace and prints its full path in the logs during the run. If you'd rather choose where results land, pass `--workspace <YOUR_DIR_HERE>` as a top-level argument of `llmdbenchmark` (before the `run` subcommand):

```
llmdbenchmark \
    --spec           guides/precise-prefix-cache-routing \
    run \
    --endpoint-url   "${ENDPOINT_URL}" \
    --gateway-class  "${GATEWAY_CLASS}" \
    --model          "Qwen/Qwen3-32B" \
    --namespace      "${NAMESPACE}" \
    --harness        inference-perf \
    --workload       guide_precise-prefix-cache-routing_1.yaml \
    --analyze
```

Note
Depending on your `cluster` you may need to extend the default `timeout` values to longer duration, as `bind`, `access` and `wait-timeout` times of `pvcs` and `pods` can be arbitrarily slower on other systems, please utilize `llmdbenchmark run --help` to view the knobs needed to increase those values. [!IMPORTANT] When benchmarking TPU v6e or configurations with strict context length limits (e.g., `--max-model-len=4096` as in the default `patch-vllm.yaml` for TPU v6e), you **must** update the workload parameters inside `guide.yaml` before running. Specifically, decrease `system_prompt_len` (e.g. to `2000`), `question_len` (e.g. to `500`), and `output_len` (e.g. to `500`) so that the total request context size (`3000` tokens) stays well below the model's `4096` max token length limit. Leaving the default `6000`/`1200` values will cause the vLLM engine to reject all benchmark requests with `400 Bad Request`.
## Cleanup

```
helm uninstall ${GUIDE_NAME} -n ${NAMESPACE}
# Render (tokenizer) Service. Set this to the overlay you applied:
export RENDER_OVERLAY=render # render | render/standalone
kubectl delete -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/${RENDER_OVERLAY}/
# For vLLM (default):
kubectl delete -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/modelserver/gpu/vllm/${INFRA_PROVIDER}/
# For SGLang:
kubectl delete -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/modelserver/gpu/sglang/
```

## How It Works
  1. **Model server pods publish KV-cache events** — each pod (vLLM or SGLang) runs with `--kv-events-config '{...,"publisher":"zmq","endpoint":"$(KV_EVENTS_ENDPOINT)","topic":"kv@$(POD_IP):$(POD_PORT)@<model>"}'` and `KV_EVENTS_ENDPOINT=tcp://*:5556`, binding its own ZMQ socket. On every KV block allocation/eviction, the server emits a ZMQ message. The GPU vLLM backend (v0.26.0+) additionally binds a ZMQ ROUTER socket on port 5559 and retains the last 10,000 batches in an in-memory replay buffer for index recovery.
  2. **Router subscribes per pod** — pod-discovery (`kvEventsConfig.discoverPods: true`) registers the `precise-prefix-cache-producer` as an extractor on the data-layer `endpoint-notification-source`, so each router replica installs a ZMQ subscriber per model server pod independently. All replicas converge to the same index. When a replay endpoint is available, each subscriber requests buffered events on first connect (or after an EPP restart) to rebuild its KV-block index without waiting for live traffic.
  3. **Router tokenizes the prompt** — before it can look the prefix up in that index, the `token-producer` plugin POSTs the prompt to the render Service to get exact token IDs. By default that Service fronts the model server pods themselves (`vllm serve` exposes `/v1/*/render`), so no separate renderer pool sits on the request path.
  4. **Filter + score** — the `prefix-cache-affinity-filter` narrows candidates to the pods where the request's prefix blocks are resident (falling back to the least-loaded pods when the cache-warm set is saturated past `peakPrefillThroughput`), and the `token-load-scorer` picks the endpoint with the least in-flight token load among them.


## Benchmarking Reports
Empirical benchmark reports comparing event-driven precise prefix-cache routing performance against a standard Kubernetes Service under identical hardware configurations:
  * **[Qwen/Qwen3-32B on vLLM (16×H100 Precise Routing)](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/benchmark-results/vllm-qwen3-32b-h100.md)** : Compares precise prefix routing against round-robin Kubernetes Service load balancing on vLLM.
  * **[Qwen/Qwen3-32B on SGLang (16×H100 Precise Routing)](https://github.com/llm-d/llm-d/blob/main/guides/precise-prefix-cache-routing/benchmark-results/sglang-qwen3-32b-h100.md)** : Compares precise prefix routing against round-robin Kubernetes Service load balancing on SGLang.


You can’t perform that action at this time. 
