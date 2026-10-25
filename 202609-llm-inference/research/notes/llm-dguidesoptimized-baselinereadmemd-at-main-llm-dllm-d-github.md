---
title: llm-d/guides/optimized-baseline/README.md at main · llm-d/llm-d · GitHub
id: llm-dguidesoptimized-baselinereadmemd-at-main-llm-dllm-d-github
tags:
- llmd-kv-cache-routing-c665d5
- locus-affinity-helps-or-hurts
- scheduler-defaults
- optimized-baseline
- prefix-cache-affinity-filter
created: '2026-09-14T12:41:55.512778Z'
updated: '2026-09-15T04:55:24.126239Z'
source: https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md
source_domain: github.com
fetched_at: '2026-09-14T12:41:55.492413Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Primary GitHub README for guides/optimized-baseline in llm-d/llm-d (commit
  965a508, Sep 12 2026) — the ''recommended out of the box configuration'' for most
  vLLM/SGLang/TensorRT-LLM deployments. States explicitly: ''The optimized-baseline
  defaults to two main routing criteria: Prefix-cache aware using the prefix cache
  affinity filter... Load-aware using the token load scorer... Both plugins are used
  with their built-in defaults — no per-deployment tuning is required for this guide''s
  reference setup (Qwen3-32B on H100 80GB, TP=2).'' Confirms peakPrefillThroughput
  default of 15928 is ''the value measured for this guide''s setup'' and is hardware/model-specific
  — deploying different hardware requires recalibration via guides/recipes/router/calibration.
  Reference config lives at guides/optimized-baseline/router/optimized-baseline.values.yaml,
  deployed via the llm-d-router Helm chart (ROUTER_CHART_VERSION=v0 floating channel).
  Also documents an optional flow-control addition (concurrency-detector, maxConcurrency)
  layered on top of the two default plugins, and lists three published benchmark reports
  (Qwen3-32B on H100 with SGLang/vLLM, gpt-oss-120b on H100 with vLLM) comparing this
  routing setup against a standard Kubernetes Service.'
---

[Skip to content](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 765 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
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
[Hardware support: Rebellions NPU (optimized-baseline) (](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7)[#2476](https://github.com/llm-d/llm-d/pull/2476)[)](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7)
Open commit detailssuccess
Sep 12, 2026
[965a508](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7) · Sep 12, 2026
## History
[History](https://github.com/llm-d/llm-d/commits/main/guides/optimized-baseline/README.md)
Open commit details
History
528 lines (395 loc) · 29.6 KB
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

528 lines (395 loc) · 29.6 KB
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Optimized Baseline
## Overview
This guide deploys the recommended out of the box [configuration](https://github.com/llm-d/llm-d-router/blob/main/docs/architecture.md) for most vLLM, SGLang, and TensorRT-LLM deployments, reducing tail latency and increasing throughput through load-aware and prefix-cache aware balancing.
The optimized-baseline defaults to two main routing criteria:
  * **Prefix-cache aware** using the [prefix cache affinity filter](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/README.md), which narrows candidates to "sticky" endpoints with high estimated prompt prefix cache reuse, with a saturation-aware override that spreads load when endpoints get hot.
  * **Load-aware** using the [token load scorer](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/scheduling/scorer/tokenload/README.md), which scores endpoints based on the total prefill token load handled by each model server.


Both plugins are used with their built-in defaults — no per-deployment tuning is required for this guide's reference setup (Qwen3-32B on H100 80 GB, TP=2). If you deploy a **different model or accelerator** , the saturation-aware override gate keys off the `peakPrefillThroughput` of the filter, which is hardware- and model-specific; measure your own with the shared [calibration recipe](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/README.md) and set it on the filter. See [Adapting to other hardware](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md#adapting-to-other-hardware) below.
## Configuration  
| Parameter  | Default  | Example  |  
| --- | --- | --- |  
| Model  | [openai/gpt-oss-120b](https://huggingface.co/openai/gpt-oss-120b)  |  
| Replicas  | 8  | 16  |  
| Tensor Parallelism  | 2  | 1  |  
| GPUs per replica  | 2  | 1  |  
| Total GPUs  | 16  | 16  |  
### Supported Hardware Backends
This guide includes configurations for the following accelerators:  
| Backend  | Directory  | Notes  |  
| --- | --- | --- |  
| NVIDIA GPU  | `gpu`  | Default configuration (`INFRA_PROVIDER` options: `base`, `gke`)  |  
| AMD GPU  | `amd`  | AMD GPU  |  
| Intel XPU  | `xpu`  | Intel Data Center GPU Max 1550+  |  
| Google TPU v6e  | `tpu/v6`  | GKE TPU  |  
| Google TPU v7  | `tpu/v7`  | GKE TPU  |  
| Rebellions NPU  | `npu`  | Rebellions NPU via DRA  |  
| CPU  | `cpu`  | x86 with bf16 acceleration  |  
Note
"x86 with bf16 acceleration": AMX or AVX512-BF16 (Intel Sapphire Rapids+ / GCP C3, AMD Zen 4+); 64 cores + 64GB RAM per replica. Older CPUs without AMX/AVX512-BF16 (e.g. Cascade/Ice Lake) crash on the bf16 model unless run with `--dtype=float32`
Some hardware variants use reduced configurations (fewer replicas, smaller models) to enable CI testing for compatibility and regression checks. These configurations are maintained by their respective hardware vendors and are not guaranteed as production-ready examples. Users deploying on non-default hardware should review and adjust the configurations for their environment.
## Prerequisites
  * Have the [proper client tools installed on your local system](https://github.com/llm-d/llm-d/blob/main/helpers/client-setup/README.md) to use this guide.
  * Ensure your cluster has enough accelerators to satisfy the [Configuration](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md#configuration) table above (default: 16 GPUs). If your cluster has fewer resources, adjust `replicas` and `--tensor-parallel-size` in the [model server patch](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/modelserver/gpu/vllm/base/patch-vllm.yaml) for your environment.
  * Set the branch and clone the llm-d repo:



```
export BRANCH=main
git clone https://github.com/llm-d/llm-d.git  cd llm-d  git checkout ${BRANCH}
```

  * Set the guide specific environment variables:



```
export BRANCH=main
export REPO_ROOT=$(realpath $(git rev-parse --show-toplevel))
export GUIDE_NAME=optimized-baseline
export NAMESPACE=llm-d-optimized-baseline
```


```
export HF_TOKEN=HF_TOKEN_PLACEHOLDER
```


```
export MONITORING_VALUES=
export PROVIDER_NAME=none # options: none, gke, agentgateway, istio
export ACCELERATOR_TYPE=gpu # options: gpu, amd, xpu, hpu, tpu/v6, tpu/v7, npu, cpu
export MODEL_SERVER=vllm # options: vllm, sglang, trtllm
export INFRA_PROVIDER=base # options: base, gke
export MODEL=Qwen/Qwen3-32B
export CURL_TEST_IMAGE=cfmanteiga/alpine-bash-curl-jq:latest
export BENCHMARK_REF=main
export HARNESS=inference-perf
export WORKLOAD=guide_optimized-baseline_1.yaml
export GATEWAY_CLASS=epponly # options: epponly, gke, agentgateway, istio
```

Note
`HF_TOKEN` must be a [valid HuggingFace token](https://github.com/llm-d/llm-d/blob/main/helpers/hf-token.md); replace `HF_TOKEN_PLACEHOLDER` with your real token:
  * Source the common guide environment variables:



```
source ${REPO_ROOT}/guides/env.sh
```

Note
This file defines shared variables required by subsequent steps, including `GAIE_VERSION`, `ROUTER_CHART_VERSION`, and the router chart reference for the selected deployment mode. `env.sh` always sets `ROUTER_CHART_VERSION=v0` (the floating release channel); to pin a specific llm-d-router chart release, re-export the variable after sourcing.
  * Install the Gateway API Inference Extension CRDs:



```
# GAIE_URL is automatically calculated from GAIE_VERSION at ${REPO_ROOT}/guides/env.sh
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/${GAIE_URL}/v1-manifests.yaml
```

  * Create a target namespace for the installation:



```
kubectl create namespace ${NAMESPACE} --dry-run=client -o yaml | kubectl apply -f -
```

  * [Create the `llm-d-hf-token` secret in your target namespace with the key `HF_TOKEN` matching a valid HuggingFace token](https://github.com/llm-d/llm-d/blob/main/helpers/hf-token.md) to pull models:



```
kubectl create secret generic llm-d-hf-token \
  --from-literal="HF_TOKEN=${HF_TOKEN}" \
  --namespace "${NAMESPACE}" \
  --dry-run=client -o yaml | kubectl apply -f -
```

## Installation Instructions
### 1. Deploy the llm-d Router
  * Prepare the paths to the `helm` values files for `llm-d` router (used in the deployment commands below):



```
# Paths to values files
export ROUTER_BASE_VALUES="${REPO_ROOT}/guides/recipes/router/base.values.yaml"

# only when MODEL_SERVER=vllm or sglang:
export ROUTER_VALUES="${REPO_ROOT}/guides/${GUIDE_NAME}/router/${GUIDE_NAME}.values.yaml"

# only when MODEL_SERVER=trtllm:

# Comment out the above `ROUTER_VALUES` and uncomment the below for TensorRT-LLM (trtllm-serve)

# export ROUTER_VALUES="${REPO_ROOT}/guides/${GUIDE_NAME}/router/${GUIDE_NAME}-trtllm.values.yaml"
```

Note
As denoted above, **vllm, sglang** share a values file, while **TensorRT-LLM** (`trtllm-serve`) has it's own values file.
  * Optionally, to enable `Prometheus Monitoring` on the `llm-d` router define the `helm` values file:



```

# Uncomment the below to enable Prometheus monitoring on the llm-d router

# Unlike the ROUTER_*_VALUES paths above, this variable carries its own
# -f flag: it is empty by default, so the helm commands expand it as-is.

# export MONITORING_VALUES="-f ${REPO_ROOT}/guides/recipes/router/features/monitoring.values.yaml"
```

Note
When following the guide from top to bottom, we already have `export MONITORING_VALUES=""` by default. This means that `monitoring` is disabled by default.
Warning
Enabling monitoring here requires the monitoring stack to be installed first. The `monitoring.values.yaml` file creates a `ServiceMonitor`, which needs the Prometheus Operator CRDs. Deploying the router with this file before the monitoring stack is ready (see [Step 3: Enable monitoring](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md#3-optional-enable-monitoring)) will fail with a Helm validation error. If you want monitoring enabled from the start, install the monitoring stack before the router, or leave `MONITORING_VALUES` empty and `helm upgrade` with the monitoring values after Step 3.
#### Standalone Mode
This deploys the llm-d Router in [Standalone Mode](https://github.com/llm-d/llm-d/blob/main/docs/architecture/core/router/proxy.md) with an Envoy sidecar (default):
Important
Before running the command below, execute the path setup commands from the previous section: the `export ROUTER_BASE_VALUES=...` and `export ROUTER_VALUES=...` commands above.

```
# Assuming base-directory is the root of the llm-d repo
helm install ${GUIDE_NAME} \
  ${ROUTER_STANDALONE_CHART} \
  -f ${ROUTER_BASE_VALUES} \
  ${MONITORING_VALUES} \
  -f ${ROUTER_VALUES} \
  -n ${NAMESPACE} --version ${ROUTER_CHART_VERSION}
```

To use **agentgateway** as the sidecar proxy instead of Envoy, see [router recipes](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/README.md).
#### Gateway Mode
To use a Kubernetes Gateway managed proxy rather than the standalone version, follow these steps instead of applying the previous Helm chart:
  1. _Deploy a Kubernetes Gateway_ named by following one of [the gateway guides](https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/gateway).
  2. _Deploy the llm-d router and an HTTPRoute_ that connects it to the Gateway as follows:


> [!IMPORTANT] Before running the command below, execute the path setup commands from the previous section: the `export ROUTER_BASE_VALUES=...` and `export ROUTER_VALUES=...` commands above.
> Also set `PROVIDER_NAME` to the gateway provider you deployed in step 1 (e.g. `gke`, `istio`). The default, `none`, renders no provider-specific resources — on GKE that means no `HealthCheckPolicy`, so the Gateway marks the backends unhealthy and requests fail with 503s.

```
helm install ${GUIDE_NAME} \
  ${ROUTER_GATEWAY_CHART} \
  -f ${ROUTER_BASE_VALUES} \
  ${MONITORING_VALUES} \
  -f ${ROUTER_VALUES} \
  --set provider.name=${PROVIDER_NAME} \
  --set httpRoute.create=true \
  --set httpRoute.inferenceGatewayName=llm-d-inference-gateway \
  -n ${NAMESPACE} --version ${ROUTER_CHART_VERSION}
```

### 2. Deploy the Model Server
Apply the Kustomize overlays for your specific backend:

```
# only when ACCELERATOR_TYPE=gpu:
kubectl apply -n ${NAMESPACE} \
  -k ${REPO_ROOT}/guides/${GUIDE_NAME}/modelserver/${ACCELERATOR_TYPE}/${MODEL_SERVER}/${INFRA_PROVIDER}/

# only when ACCELERATOR_TYPE=amd or xpu or hpu or tpu/v6 or tpu/v7 or npu or cpu:

# Comment out the above `kubectl apply` and uncomment the below to run on `NON GPU` accelerators

# kubectl apply -n ${NAMESPACE} \
#  -k ${REPO_ROOT}/guides/${GUIDE_NAME}/modelserver/${ACCELERATOR_TYPE}/${MODEL_SERVER}/

```

Note
Ensure `INFRA_PROVIDER`, `ACCELERATOR_TYPE` and `MODEL_SERVER` are set appropriately, see above environment section for options.
#### Other Models
For example to deploy other models:

```
# NVIDIA GPU / vLLM — openai/gpt-oss-120b
kubectl apply -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/modelserver/gpu/vllm/gpt-oss/
```

### 3. (Optional) Enable monitoring
  * Install the [Monitoring stack](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/setup.md).
  * This requires `Prometheus Monitoring` to be enabled on `llm-d` router, see `Step 1 Deploy the llm-d Router` above.
  * Deploy the monitoring resources for model servers:



```
kubectl apply -n ${NAMESPACE} -k ${REPO_ROOT}/guides/recipes/modelserver/components/monitoring
```

### 4. Observability & Troubleshooting
Once monitoring is enabled, use the signals below to operate the optimized baseline. This section covers the metrics that matter **for this path** and how to read them; full metric definitions live in the [metric reference](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/metrics.md) and ready-to-run queries in the [PromQL reference](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/promql.md).
This path is defined by its two routing objectives: **prefix-cache affinity** (route to endpoints that already hold the prompt prefix) and **load-aware** balancing (spread work by token load), with a saturation override that trades cache locality for spread once endpoints get hot. Most issues show up as those two objectives pulling against each other, so watch **load balance** and **cache hit rate** together rather than either one alone.
#### Key metrics for this path  
| Signal  | Why it matters for the optimized baseline  | Where to look  |  
| --- | --- | --- |  
| Per-pod load (`llm_d_epp_request_total`, `vllm:num_requests_running`)  | The load-aware scorer should keep QPS and active requests roughly even across pods. A persistently hot pod next to idle ones means balancing is not taking effect  | [PromQL → Routing & Load Balancing](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/promql.md#routing--load-balancing)  |  
| Prefix cache hit rate (`vllm:prefix_cache_hits_total` / `vllm:prefix_cache_queries_total`)  | The prefix-affinity filter is only helping if hit rate stays high. A falling ratio means requests are not landing on sticky endpoints  | [PromQL → Prefix Caching](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/promql.md#prefix-caching)  |  
| Per-pod KV cache utilization and queue depth (`vllm:kv_cache_usage_perc`, `vllm:num_requests_waiting`)  | These drive the saturation-aware override. If one pod sits near saturation while others are cold, the override is either not firing or mis-tuned  | [PromQL → Basic Model Serving](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/promql.md#basic-model-serving)  |  
| Routing decision latency (`llm_d_epp_plugin_duration_seconds`)  | Rising scheduler latency with healthy model servers localizes the problem to the routing layer, not the pods  | [PromQL → Routing & Load Balancing](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/promql.md#routing--load-balancing)  |  
| TTFT and ITL (`vllm:time_to_first_token_seconds`, `vllm:inter_token_latency_seconds`)  | The user-facing SLO signals this path is tuned to protect. Regressions here are the trigger to inspect the balance/cache split above  |  
> SGLang deployments expose the equivalent signals under `sglang_*` (`sglang_num_running_reqs`, `sglang_token_usage`, `sglang_cache_hit_rate`); the PromQL reference lists both.
#### Common failure modes
  * **Uneven load across pods** (some hot, some idle) — the load-aware scorer or the saturation override is not spreading work. On **non-default hardware** this usually means `peakPrefillThroughput` is miscalibrated, so the override never gates in; measure it with the [calibration recipe](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/README.md) and set it on the filter (see [Adapting to other hardware](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md#adapting-to-other-hardware)).
  * **Low prefix cache hit rate** — either the prompt mix is not prefix-sticky, or the saturation override is spreading so aggressively that it defeats affinity. Compare hit rate against per-pod saturation; if pods are cold but hit rate is still low, the issue is the prompt pattern, not the override.
  * **TTFT/ITL regression with balanced load and healthy cache** — look at routing decision latency and model-server queue depth before touching the routing config; the bottleneck is likely the model servers, not the scheduler.


For alert rules covering these signals, see [Alerting](https://github.com/llm-d/llm-d/blob/main/docs/operations/observability/alerting.md).
## Adapting to other hardware
The routing plugins ship with defaults tuned for this guide's reference setup (Qwen3-32B on H100 80 GB, TP=2), so **no calibration is needed to run the guide as written**.
The saturation-aware override gate in the `prefix-cache-affinity-filter` keys off the filter's `peakPrefillThroughput` parameter, which is **hardware- and model-specific** (the plugin default, `15928`, is the value measured for this guide's setup). If you deploy a different model or accelerator, measure your own value with the shared calibration recipe and set it on the filter:

```
# guides/optimized-baseline/router/optimized-baseline.values.yaml
- type: prefix-cache-affinity-filter
  parameters:
    peakPrefillThroughput: <measured value>
```

The recipe (`calibrate.sh`) runs a short Kubernetes Job that measures true prefill throughput against your live deployment and prints the value — it does not modify any config. See the recipe's README for full usage:
  * [`guides/recipes/router/calibration/README.md`](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/README.md)


For reference values across the (model, accelerator) combinations shipped under `guides/` — and which ones still need a calibration run — see the [**configuration matrix**](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md).
## Optional: Enable flow control
Flow control lets the router hold excess requests in the EPP instead of immediately dispatching them to already busy model servers. Pairing it with the `concurrency-detector` caps the number of in-flight requests sent to each endpoint. That cap limits simultaneous demand on the endpoint's KV cache, which can reduce cache pressure, request preemption, and KV-cache thrashing.
[`concurrencyMode`](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/flowcontrol/saturationdetector/concurrency#configuration) determines the metric used to throttle workload per backend: in-flight requests, estimated in-flight tokens, or both.
To enable it, add the feature gate, detector plugin, and `flowControl` section to the `EndpointPickerConfig` embedded in [`router/optimized-baseline.values.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/router/optimized-baseline.values.yaml):

```
featureGates:
- flowControl

plugins:
# Keep the existing optimized-baseline plugins here.
- type: concurrency-detector
  parameters:
    maxConcurrency: 8 # Example only; tune this for each endpoint's performance and latency constraints.
    concurrencyMode: requests
    headroom: 0.0

flowControl:
  maxBytes: "10Gi"
  maxRequests: 
  defaultRequestTTL: 
  saturationDetector:
    pluginRef: concurrency-detector
```

`maxConcurrency` applies to one model-server endpoint, not the whole pool. A lower value keeps more work in the EPP and reduces pressure on the model server, but setting it too low can leave the accelerator idle. A higher value keeps the engine fed, but setting it too high can increase local queueing and KV-cache pressure. Size `maxBytes` and `maxRequests` for the EPP's available memory and expected request bodies. See [Production Tuning: Deriving `maxConcurrency`](https://github.com/llm-d/llm-d/blob/main/guides/flow-control/tuning.md) for the measurement procedure and detector trade-offs.
## Verification
### 1. Get the IP of the Proxy
**Standalone Mode**

```
export IP=$(kubectl get service ${GUIDE_NAME}-epp -n ${NAMESPACE} -o jsonpath='{.spec.clusterIP}')
```

**Gateway Mode**

```
export IP=$(kubectl get gateway llm-d-inference-gateway -n ${NAMESPACE} -o jsonpath='{.status.addresses[0].value}')
```

### 2. Send Test Requests
**Send a completion request from a temporary pod inside the cluster (model-aware; set`MODEL` to the name you want to query, e.g. `Qwen/Qwen3-32B` or `openai/gpt-oss-120b`):**

```
kubectl run curl-test --rm -i --restart=Never \
  --image=${CURL_TEST_IMAGE} \
  --namespace="${NAMESPACE}" \
  --env="IP=${IP}" \
  --env="MODEL=${MODEL}" \
  -- /bin/sh -c 'curl -sS -X POST "http://${IP}/v1/completions" -H "Content-Type: application/json" -d "{\"model\": \"${MODEL}\", \"prompt\": \"How are you today?\"}"'
```

## Benchmarking
This guide uses [`llmdbenchmark`](https://github.com/llm-d/llm-d-benchmark) — the supported standard CLI for llm-d performance benchmarking.
In this example we will demonstrate how to run [`inference-perf`](https://github.com/kubernetes-sigs/inference-perf) with a shared-prefix synthetic workload against the stack you just deployed exactly as written above (standalone or gateway mode). When orchestrating benchmarks via `llmdbenchmark`, the CLI automatically and transparently deploys a harness pod (`llmdbench-harness-launcher`) into your namespace. This pod is central to driving the workload, collecting the results, and tearing itself down when it's finished.
Important
**For more indepth explanation and features for benchmarking llm-d guides directly can be found at[`helpers/benchmark.md`](https://github.com/llm-d/llm-d/blob/main/helpers/benchmark.md).**
The Benchmarking section below contains only the **optimized-baseline-specific commands** needed to drive the stack you just deployed — for everything else (and especially when something goes wrong), start at [`helpers/benchmark.md`](https://github.com/llm-d/llm-d/blob/main/helpers/benchmark.md).
For even more details about benchmarking, see the actual repository: [`llm-d-benchmark` on GitHub](https://github.com/llm-d/llm-d-benchmark).
Tip
The command below runs this guide's **dedicated** benchmark profile, which is intentionally shaped to exercise the optimized-baseline routing under realistic load — and accordingly takes longer to complete. To run a simpler workload with fewer execution cycles first (useful for validating the path, image pulls, PVC binding, etc. before committing to a real run), pick a generic sample profile such as `shared_prefix_synthetic.yaml` from the catalog in [`helpers/benchmark.md` → Available workload profiles](https://github.com/llm-d/llm-d/blob/main/helpers/benchmark.md#available-workload-profiles) and substitute it for the `--workload` flag in the command below.
### 1. Install the `llmdbenchmark` CLI
Automatically clone the benchmark repository into `./llm-d-benchmark/` and create a virtualenv at `./llm-d-benchmark/.venv/` containing dependencies and it's installation:

```
curl -sSL https://raw.githubusercontent.com/llm-d/llm-d-benchmark/${BENCHMARK_REF}/install.sh | bash

cd llm-d-benchmark

source .venv/bin/activate

llmdbenchmark --version
```

Note
Subsequent `llmdbenchmark` commands in this section assume you are inside the `llm-d-benchmark` repo directory with the `venv` activated. If you open a new shell, re-run the two commands above.
### 2. Resolve the endpoint of the stack you just deployed
Set two variables so the rest of the section is topology-agnostic: the endpoint URL and the gateway class. The gateway class tells the CLI which deployment topology the cluster is actually running, without this, the CLI re-renders against the benchmark scenario's default values.
**Standalone Mode** (the default in this guide — no Kubernetes Gateway, EPP pod with an Envoy sidecar). `GATEWAY_CLASS=epponly` is the default:

```
export ENDPOINT_URL="http://$(kubectl get service ${GUIDE_NAME}-epp -n ${NAMESPACE} -o jsonpath='{.spec.clusterIP}')"
```

**Gateway Mode**
Set `GATEWAY_CLASS` to match whichever provider you used when deploying the gateway (e.g. `istio`, `agentgateway`, `gke`) — see the `GATEWAY_CLASS` options in the [environment section](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md#prerequisites) above.

```
export ENDPOINT_URL="http://$(kubectl get gateway llm-d-inference-gateway -n ${NAMESPACE} -o jsonpath='{.status.addresses[0].value}')"
```

### 3. Run the benchmark profile for Optimized Baseline
`guide_optimized-baseline_1.yaml` is a **dedicated workload profile** shipped with `llm-d-benchmark` specifically for this guide — it reproduces the load ladder used to generate the [graphs at the bottom of this guide](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md#benchmarking-report) (rates 3 to 60) and is shaped to highlight the strengths of the optimized-baseline routing under realistic saturation.
Benchmark results are copied to the `workspace` directory that is specified by _you_ (or that is automatically generated when omitted from the cli) on the machine running the CLI. The workspace location is optional — by default the CLI auto-generates a timestamped workspace and prints its full path in the logs during the run. If you'd rather choose where results land, pass `--workspace <YOUR_DIR_HERE>` as a top-level argument of `llmdbenchmark` (before the `run` subcommand):

```
llmdbenchmark \
  --spec           guides/${GUIDE_NAME} \
  run \
  --endpoint-url   "${ENDPOINT_URL}" \
  --gateway-class  "${GATEWAY_CLASS}" \
  --model          "${MODEL}" \
  --namespace      "${NAMESPACE}" \
  --harness        "${HARNESS}" \
  --workload       "${WORKLOAD}" \
  --analyze
```

Note
Depending on your `cluster` you may need to extend the default `timeout` values to longer duration, as `bind`, `access` and `wait-timeout` times of `pvcs` and `pods` can be arbitrarily slower on other systems, please utilize `llmdbenchmark run --help` to view the knobs needed to increase those values. Model-aware; set `model` to the name you want to query, e.g. `Qwen/Qwen3-32B` or `openai/gpt-oss-120b`
## Cleanup
To remove the deployed components:

```
helm uninstall ${GUIDE_NAME} -n ${NAMESPACE}

# only when ACCELERATOR_TYPE=gpu:
kubectl delete -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/modelserver/${ACCELERATOR_TYPE}/${MODEL_SERVER}/${INFRA_PROVIDER}

# only when ACCELERATOR_TYPE=amd or xpu or hpu or tpu/v6 or tpu/v7 or npu or cpu:

# Comment out the above `kubectl delete` and uncomment the below to run on `NON GPU` accelerators

# kubectl delete -n ${NAMESPACE} -k ${REPO_ROOT}/guides/${GUIDE_NAME}/modelserver/${ACCELERATOR_TYPE}/${MODEL_SERVER}

kubectl delete -n ${NAMESPACE} -k ${REPO_ROOT}/guides/recipes/modelserver/components/monitoring --ignore-not-found=true
```


```
kubectl delete namespace ${NAMESPACE}
```

## Benchmarking Reports
Empirical benchmark reports comparing llm-d routing performance against a standard Kubernetes Service under identical hardware configurations:
  * [Qwen/Qwen3-32B on H100 and SGLang](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/benchmark-results/sglang-qwen3-32b-h100/README.md)
  * [Qwen/Qwen3-32B on H100 and vLLM](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/benchmark-results/vllm-qwen3-32b-h100/README.md)
  * [openai/gpt-oss-120b on H100 and vLLM](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/benchmark-results/vllm-gpt-oss-120b-h100/README.md)


You can’t perform that action at this time. 
