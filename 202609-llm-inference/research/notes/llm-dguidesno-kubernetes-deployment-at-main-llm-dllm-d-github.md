---
title: llm-d/guides/no-kubernetes-deployment at main · llm-d/llm-d · GitHub
id: llm-dguidesno-kubernetes-deployment-at-main-llm-dllm-d-github
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- no-kubernetes
- deployment-guide
- file-discovery
created: '2026-09-11T20:48:07.438169Z'
updated: '2026-09-12T03:37:37.668252Z'
source: https://github.com/llm-d/llm-d/tree/main/guides/no-kubernetes-deployment
source_domain: github.com
fetched_at: '2026-09-11T20:48:07.416927Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE: full README.md of llm-d/llm-d/guides/no-kubernetes-deployment
  (main branch, fetched Sep 11 2026, unpinned commit). This is the actual step-by-step,
  deploy-ready guide the llm-d.ai no-Kubernetes blog points to. Targets vLLM on NVIDIA
  GPUs specifically (model-server image/flags/--gpus invocation are NVIDIA+vLLM-specific;
  EPP and Envoy configs are stated to be accelerator-agnostic, substitute the model-server
  step with an optimized-baseline modelserver overlay for AMD/Intel XPU/TPU/CPU).
  Pinned example versions in this guide (may lag the latest llm-d release; verify
  against current release notes before treating as current): VLLM_IMAGE=vllm/vllm-openai:v0.19.1,
  ENVOY_IMAGE=docker.io/envoyproxy/envoy:distroless-v1.33.2, MODEL=Qwen/Qwen3-32B,
  EPP image ghcr.io/llm-d/llm-d-router-endpoint-picker. Three config files ship: router/epp/config.yaml
  (EPP plugin config: optimized-baseline plugin mix -- prefix-cache-scorer weight
  3, queue-scorer/kv-cache-utilization-scorer/no-hit-lru-scorer weight 2 each, composed
  by max-score-picker -- plus file-discovery), router/epp/endpoints.yaml (the watched
  endpoints file), router/envoy/envoy.yaml (ext_proc to EPP, ORIGINAL_DST to vLLM).
  Gives the exact EPP binary invocation flags for no-Kubernetes mode: --config-file,
  --pool-name=file-discovery, --pool-namespace=default, --grpc-port=9002, --grpc-health-port=9003,
  --metrics-port=9090, --secure-serving=false -- and explicitly states ''--pool-name
  and --pool-namespace are not Kubernetes references in file-discovery mode; they
  are only used as labels in the EPP''s metrics and log lines.'' Confirms the endpoints.yaml
  worked example matches the file-discovery plugin README schema (name/address/port/labels,
  IPv4-only, hostnames not resolved). Troubleshooting section confirms live-reload
  log line ''endpoints file changed, reloading'' fires on atomic-rename updates (mv
  tmpfile -> endpoints.yaml) when watchFile: true. Confirms P/D disaggregation works
  outside Kubernetes by using file-discovery in place of InferencePool discovery,
  setting llm-d.ai/role: prefill or decode on each endpoint''s labels, combined with
  the separate pd-disaggregation guide for EPP plugin config and vLLM/sidecar setup.
  Monitoring section shows a plain static Prometheus scrape_config example (no ServiceMonitor/Kubernetes
  discovery) pointed at the EPP metrics port and each vLLM worker''s /metrics.'
utility_score: 18.0
---

*Suggested by [[no-kubernetes-no-problem-llm-d-now-runs-anywhere-llm-d]] — upstream deploy-ready guide linked from the no-Kubernetes blog post*

[Skip to content](https://github.com/llm-d/llm-d/tree/main/guides/no-kubernetes-deployment#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/tree/main/guides/no-kubernetes-deployment) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/tree/main/guides/no-kubernetes-deployment) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/tree/main/guides/no-kubernetes-deployment) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/tree/main/guides/no-kubernetes-deployment).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 761 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# no-kubernetes-deployment
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[Removed direct references to llm-d epp image and consolidated referen…](https://github.com/llm-d/llm-d/commit/f06c755a960729d9d4e94477f4a052ee88727a6a)
Open commit detailssuccess
Jun 23, 2026
[f06c755](https://github.com/llm-d/llm-d/commit/f06c755a960729d9d4e94477f4a052ee88727a6a) · Jun 23, 2026
## History
[History](https://github.com/llm-d/llm-d/commits/main/guides/no-kubernetes-deployment)
Open commit details
History
/
# no-kubernetes-deployment
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
|   |   | [update: migrate precise-prefix-cache-scorer and a bunch of default co…](https://github.com/llm-d/llm-d/commit/6c543944abdf7f137178dd9ec91159dd9a6eb990 "update: migrate precise-prefix-cache-scorer and a bunch of default configs \(#1688\)

* update: migrate precise-prefix-cache-score

- use prefix-cache-scorer with  precise-prefix-cache-producer instead

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: drop max-score-picker

- default picker for all profiles, no need set it explicityly unless
  with maxNumOfEndpoint set

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: remove single-profile-handler

- default one when no handler declared

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: remove weight when set to 1 as default value

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: drop metrics-data-source with default parameters and
core-metrics-extractor

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* docs: updates

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Jun 5, 2026  |  
|   |   |  [chore(guides): add OWNERS files to all well-lit path guides (](https://github.com/llm-d/llm-d/commit/b7fefee404ad383d7a897c58b328edb5837f0be3 "chore\(guides\): add OWNERS files to all well-lit path guides \(#1663\)

Adds an OWNERS file to each guide with approvers, reviewers, and auto-assign fields enabled, keeping the reviewers list synchronized with the approvers.

Below is a summary of each guide and their designated owners:

1. Asynchronous Processing \(asynchronous-processing\)
   Summary: Process inference requests asynchronously using a queue-based architecture for batch workloads.
   Owners: @shimib

2. Batch Gateway \(batch-gateway\)
   Summary: Submit, track, and manage large-scale batch inference jobs via an OpenAI-compatible Batch API.
   Owners: @lioraron

3. Flow Control \(flow-control\)
   Summary: Intelligent request queuing for multi-tenant deployments and managing traffic spikes.
   Owners: @LukeAVanDrie

4. No-Kubernetes Deployment \(no-kubernetes-deployment\)
   Summary: Standard deployment guide for running llm-d on bare metal or standard VM-based environments.
   Owners: @ezrasilvera

5. Optimized Baseline \(optimized-baseline\)
   Summary: Deploy vLLM with prefix-cache and load-aware routing enabled by the llm-d EPP.
   Owners: @liu-cong, @vMaroon

6. Prefill/Decode Disaggregation \(pd-disaggregation\)
   Summary: Split inference into specialized prefill and decode instances to improve throughput and QoS.
   Owners: @tlrmchlsmth

7. Precise Prefix Cache Routing \(precise-prefix-cache-routing\)
   Summary: Enhance optimized baseline with precise global indexing of the vLLM KV cache state.
   Owners: @vMaroon, @liu-cong

8. Predicted Latency-Based Routing \(predicted-latency-routing\)
   Summary: Enhance optimized baseline with real-time predictions of request latency via an XGBoost model.
   Owners: @kaushikmitr

9. Recipes \(recipes\)
   Summary: Reusable components and overlays for deployment configurations \(gateway, modelserver, router\).
   Owners: @ahg-g, @robertgshaw2-redhat, @liu-cong, @Gregory-Pereira

10. Rollouts \(rollouts\)
    Summary: Perform incremental rollout operations for LoRA adapters, base models, and model server versions.
    Owners: @ahg-g

11. Tiered Prefix Cache \(tiered-prefix-cache\)
    Summary: Offload KV caches beyond accelerator memory to CPU or disk to increase working set size.
    Owners: @kfirtoledo, @liu-cong, @vMaroon

12. Wide Expert-Parallelism \(wide-ep-lws\)
    Summary: Deploy large Mixture-of-Experts \(MoE\) models over multiple nodes via DP/EP configurations.
    Owners: @robertgshaw2-redhat, @tlrmchlsmth

13. Workload Autoscaling \(workload-autoscaling\)
    Summary: Autoscale the LLM service via proactive, SLO-aware signals reflecting true state of the inference system.
    Owners: @lionelvillard

Signed-off-by: Cong Liu <conliu@google.com>")[#1663](https://github.com/llm-d/llm-d/pull/1663)[)](https://github.com/llm-d/llm-d/commit/b7fefee404ad383d7a897c58b328edb5837f0be3 "chore\(guides\): add OWNERS files to all well-lit path guides \(#1663\)

Adds an OWNERS file to each guide with approvers, reviewers, and auto-assign fields enabled, keeping the reviewers list synchronized with the approvers.

Below is a summary of each guide and their designated owners:

1. Asynchronous Processing \(asynchronous-processing\)
   Summary: Process inference requests asynchronously using a queue-based architecture for batch workloads.
   Owners: @shimib

2. Batch Gateway \(batch-gateway\)
   Summary: Submit, track, and manage large-scale batch inference jobs via an OpenAI-compatible Batch API.
   Owners: @lioraron

3. Flow Control \(flow-control\)
   Summary: Intelligent request queuing for multi-tenant deployments and managing traffic spikes.
   Owners: @LukeAVanDrie

4. No-Kubernetes Deployment \(no-kubernetes-deployment\)
   Summary: Standard deployment guide for running llm-d on bare metal or standard VM-based environments.
   Owners: @ezrasilvera

5. Optimized Baseline \(optimized-baseline\)
   Summary: Deploy vLLM with prefix-cache and load-aware routing enabled by the llm-d EPP.
   Owners: @liu-cong, @vMaroon

6. Prefill/Decode Disaggregation \(pd-disaggregation\)
   Summary: Split inference into specialized prefill and decode instances to improve throughput and QoS.
   Owners: @tlrmchlsmth

7. Precise Prefix Cache Routing \(precise-prefix-cache-routing\)
   Summary: Enhance optimized baseline with precise global indexing of the vLLM KV cache state.
   Owners: @vMaroon, @liu-cong

8. Predicted Latency-Based Routing \(predicted-latency-routing\)
   Summary: Enhance optimized baseline with real-time predictions of request latency via an XGBoost model.
   Owners: @kaushikmitr

9. Recipes \(recipes\)
   Summary: Reusable components and overlays for deployment configurations \(gateway, modelserver, router\).
   Owners: @ahg-g, @robertgshaw2-redhat, @liu-cong, @Gregory-Pereira

10. Rollouts \(rollouts\)
    Summary: Perform incremental rollout operations for LoRA adapters, base models, and model server versions.
    Owners: @ahg-g

11. Tiered Prefix Cache \(tiered-prefix-cache\)
    Summary: Offload KV caches beyond accelerator memory to CPU or disk to increase working set size.
    Owners: @kfirtoledo, @liu-cong, @vMaroon

12. Wide Expert-Parallelism \(wide-ep-lws\)
    Summary: Deploy large Mixture-of-Experts \(MoE\) models over multiple nodes via DP/EP configurations.
    Owners: @robertgshaw2-redhat, @tlrmchlsmth

13. Workload Autoscaling \(workload-autoscaling\)
    Summary: Autoscale the LLM service via proactive, SLO-aware signals reflecting true state of the inference system.
    Owners: @lionelvillard

Signed-off-by: Cong Liu <conliu@google.com>")  | Jun 3, 2026  |  
|   |   | [Removed direct references to llm-d epp image and consolidated referen…](https://github.com/llm-d/llm-d/commit/f06c755a960729d9d4e94477f4a052ee88727a6a "Removed direct references to llm-d epp image and consolidated references to router chart \(#1909\)

Signed-off-by: ahg-g <ahg@google.com>")  | Jun 23, 2026  |  
| View all files |  
## [README.md](https://github.com/llm-d/llm-d/tree/main/guides/no-kubernetes-deployment#readme)
Outline
# No-Kubernetes Deployment
Step-by-step deployment of the llm-d routing stack — the EPP (Endpoint Picker), Envoy, and one or more vLLM workers — **without a Kubernetes cluster**. The EPP gets its endpoint inventory from a YAML file on disk via the [file-discovery plugin](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go) instead of watching an `InferencePool`.
For motivation, architecture, and parity caveats vs. the Kubernetes-based optimized-baseline, see the [No-Kubernetes Deployment well-lit path](https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/no-kubernetes-deployment.md).
The configs in this directory are plain YAML — there is no Helm chart and no Kustomize overlay. Drop them on a host, point your services at them, and run.
Note
This guide targets **vLLM on NVIDIA GPUs** — the model-server image, CLI flags, and `--gpus` invocation are NVIDIA + vLLM specific. The EPP and Envoy configs are accelerator-agnostic; for AMD, Intel XPU, TPU, or CPU substitute the model-server step with the corresponding backend from the [optimized-baseline modelserver overlays](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/modelserver).
## What's in this directory  
| Path  | Purpose  |  
| --- | --- |  
| [`router/epp/config.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/no-kubernetes-deployment/router/epp/config.yaml)  | EPP plugin config (optimized-baseline plugin set + file-discovery)  |  
| [`router/epp/endpoints.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/no-kubernetes-deployment/router/epp/endpoints.yaml)  | The endpoints file the EPP watches  |  
| [`router/envoy/envoy.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/no-kubernetes-deployment/router/envoy/envoy.yaml)  | Envoy config: ext_proc to EPP, ORIGINAL_DST to vLLM  |  
## Default configuration
Same as the [optimized-baseline guide](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/README.md):  
| Parameter  | Value  |  
| --- | --- |  
| Model  |  
| Tensor parallelism  | 2  |  
| GPUs per replica  | 2  |  
Replica count is whatever you list in `router/epp/endpoints.yaml`. Resource shapes (cpu/memory/GPU) come from your launcher; the optimized-baseline references are 16 cpu / 128 GiB / 2 GPUs per replica.
The EPP scheduling profile is the optimized-baseline mix: `prefix-cache-scorer` (weight 3), `queue-scorer`, `kv-cache-utilization-scorer`, and `no-hit-lru-scorer` (each weight 2), composed by `max-score-picker`.
## Prerequisites
  * A host with the GPUs your model requires (the default config calls for two GPUs per vLLM replica).
  * Docker (or Podman) for vLLM and Envoy, plus the EPP binary or container image.
  * A Hugging Face token in `HUGGING_FACE_HUB_TOKEN` so vLLM can pull `Qwen/Qwen3-32B` on first start.


The EPP binary is built from [`cmd/epp` of the llm-d-router repo](https://github.com/llm-d/llm-d-router) or pulled from `ghcr.io/llm-d/llm-d-router-endpoint-picker`.

```
export REPO_ROOT=$(realpath $(git rev-parse --show-toplevel))
source ${REPO_ROOT}/guides/env.sh
export ENVOY_IMAGE=docker.io/envoyproxy/envoy:distroless-v1.33.2
export VLLM_IMAGE=vllm/vllm-openai:v0.19.1
export MODEL=Qwen/Qwen3-32B
```

## Installation
### 1. Stage the configs
Copy the configs to standard paths so the commands below match the defaults baked into the YAML files:

```
sudo mkdir -p /etc/epp /etc/envoy
sudo cp router/epp/config.yaml      /etc/epp/config.yaml
sudo cp router/epp/endpoints.yaml   /etc/epp/endpoints.yaml
sudo cp router/envoy/envoy.yaml     /etc/envoy/envoy.yaml
```

If you stage them elsewhere, update the `path:` field in [`router/epp/config.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/no-kubernetes-deployment/router/epp/config.yaml) (and the `--mount` paths in the commands below) to match.
### 2. Start the vLLM workers
Run one container per replica. Adjust `--gpus`, the `--tensor-parallel-size` value, and the host port for each replica.
The image, entrypoint, and argument list mirror the optimized-baseline modelserver patch ([`guides/optimized-baseline/modelserver/gpu/vllm/base/patch-vllm.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline/modelserver/gpu/vllm/base/patch-vllm.yaml)); `--shm-size=20g` reproduces the 20 GiB `/dev/shm` `emptyDir` from the recipe (required for NCCL when `--tensor-parallel-size > 1`).

```
docker run -d --name vllm-0 --gpus '"device=0,1"' \
    --shm-size=20g \
    -p 8000:8000 \
    -e "HUGGING_FACE_HUB_TOKEN=${HUGGING_FACE_HUB_TOKEN}" \
    -v "${HOME}/.cache/huggingface:/root/.cache/huggingface" \
    --entrypoint vllm \
    "${VLLM_IMAGE}" \
    serve "${MODEL}" \
    --disable-access-log-for-endpoints=/health,/metrics,/v1/models \
    --tensor-parallel-size=2
```

The optimized-baseline pod also sets resource requests/limits (8–16 cpu, 96–128 GiB memory, 2 GPUs) and Kubernetes liveness/readiness probes. Translate those to your runtime as needed (`--cpus`, `--memory`, `--health-cmd` for Docker; equivalent fields for systemd or nomad).
Wait for `/v1/models` to respond before continuing:

```
until curl -sf http://127.0.0.1:8000/v1/models /dev/null; do sleep 2; done
```

For multiple replicas, repeat with different container names, GPU sets, and host ports — and add an entry per replica in `endpoints.yaml`.
### 3. Edit the endpoints file
`endpoints.yaml` ships with a single `127.0.0.1:8000` entry. For a multi-host or multi-replica deployment, edit it now to list each worker:

```
endpoints:
  - name: vllm-0
    address: 10.0.0.10
    port: "8000"
    labels:
      model: Qwen/Qwen3-32B
  - name: vllm-1
    address: 10.0.0.11
    port: "8000"
    labels:
      model: Qwen/Qwen3-32B
```

Important
`address` must be a literal IPv4 address — the file-discovery plugin does not resolve hostnames.
The full schema is documented in [the file-discovery blog post](https://llm-d.ai/blog/running-llm-d-without-kubernetes#1-the-endpoints-file).
### 4. Start the EPP
The EPP can run as a container or as a native binary built from source. Pick one — the flags are identical.
#### Option A — container

```
docker run -d --name epp --network host \
    -v /etc/epp:/etc/epp:ro \
    "${ROUTER_EPP_IMAGE}:${ROUTER_EPP_VERSION}" \
    --config-file=/etc/epp/config.yaml \
    --pool-name=file-discovery \
    --pool-namespace=default \
    --grpc-port=9002 \
    --grpc-health-port=9003 \
    --metrics-port=9090 \
    --secure-serving=false \
    --v=2
```

#### Option B — build from source

```
git clone https://github.com/llm-d/llm-d-router.git
cd llm-d-router
make build-epp                # produces bin/epp via the project's builder container

./bin/epp \
    --config-file=/etc/epp/config.yaml \
    --pool-name=file-discovery \
    --pool-namespace=default \
    --grpc-port=9002 \
    --grpc-health-port=9003 \
    --metrics-port=9090 \
    --secure-serving=false \
    --v=2
```

Wrap in `systemd-run`, `nohup`, or your runtime's process manager for persistence; the command above runs in the foreground.
`--pool-name` and `--pool-namespace` are not Kubernetes references in file-discovery mode; they are only used as labels in the EPP's metrics and log lines.
`--network host` (Option A) lets Envoy reach the EPP on `127.0.0.1:9002` and the EPP scrape vLLM's `/metrics` on the host loopback. Drop it and use a Docker network if the workers and the EPP are on separate hosts.
### 5. Start Envoy

```
docker run -d --name envoy --network host \
    -v /etc/envoy/envoy.yaml:/etc/envoy/envoy.yaml:ro \
    "${ENVOY_IMAGE}" \
    --service-node envoy-proxy \
    --log-level warn \
    --concurrency 8 \
    --drain-strategy immediate \
    --drain-time-s 60 \
    -c /etc/envoy/envoy.yaml
```

The `--concurrency`, `--log-level`, and `--drain-*` flags match the optimized-baseline guide's Envoy sidecar arguments.
## Verification

```
# Envoy admin
curl -s http://127.0.0.1:19000/ready
curl -s http://127.0.0.1:19000/clusters | grep -E '^(ext_proc|original_destination_cluster)'


curl -s http://127.0.0.1:9090/metrics | head

# End-to-end completion through Envoy -> EPP -> vLLM
curl -s http://127.0.0.1:8081/v1/completions \
    -H 'Content-Type: application/json' \
    -d 
        "model": "Qwen/Qwen3-32B",
        "prompt": "How are you today?"

```

## Troubleshooting
### EPP not detecting endpoint changes
  * Verify `watchFile: true` in [`router/epp/config.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/no-kubernetes-deployment/router/epp/config.yaml).
  * Update the file via atomic rename, e.g. `mv endpoints.yaml.tmp endpoints.yaml`.
  * Check EPP logs (`docker logs epp`) for `endpoints file changed, reloading`.


### Envoy returns 503
  * Verify EPP is healthy: `curl http://127.0.0.1:9090/metrics`.
  * Confirm `endpoints.yaml` lists literal IPv4 addresses (hostnames are not resolved).
  * Verify the Envoy → EPP cluster: `curl http://127.0.0.1:19000/clusters | grep ext_proc`.


### vLLM worker unreachable
  * Confirm the worker is up: `curl http://<worker-ip>:8000/v1/models`.
  * Ensure firewall rules allow Envoy and EPP to reach the worker on its service port.
  * Check that the `address` in `endpoints.yaml` matches the worker's actual IP.


## Live reload
`watchFile: true` in [`router/epp/config.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/no-kubernetes-deployment/router/epp/config.yaml) means edits to the endpoints file take effect without restarting the EPP. The watcher reacts cleanly to atomic-rename writes, so update via a temp file:

```
# Add or remove workers, then:
sudo mv /etc/epp/endpoints.yaml.tmp /etc/epp/endpoints.yaml
```

The EPP logs `endpoints file changed, reloading` and reconciles the pool. Plain in-place edits also work for most editors but are less robust — see the troubleshooting section of the [blog post](https://llm-d.ai/blog/running-llm-d-without-kubernetes#troubleshooting).
## Monitoring
Point your existing Prometheus at the EPP's `--metrics-port` (`9090` in the command above) and at each vLLM worker's `/metrics` endpoint:

```
scrape_configs:
  - job_name: 'llm-d-epp'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'vllm-workers'
    static_configs:
      - targets:
        - '10.0.0.10:8000'  # vllm-0
        - '10.0.0.11:8000'  # vllm-1
    metrics_path: '/metrics'
```

For prefill/decode disaggregation outside Kubernetes, follow the [pd-disaggregation guide](https://github.com/llm-d/llm-d/blob/main/guides/pd-disaggregation/README.md) for the EPP plugin config and vLLM/sidecar setup, and use the file-discovery plugin in place of `InferencePool` discovery (set `llm-d.ai/role: prefill` or `decode` on each endpoint's `labels`).
## Cleanup

```
docker rm -f envoy epp vllm-0     # add other vllm-N containers if any
sudo rm -rf /etc/epp /etc/envoy   # if you no longer need the configs
```

You can’t perform that action at this time. 
