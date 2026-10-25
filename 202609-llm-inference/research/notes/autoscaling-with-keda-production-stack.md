---
title: Autoscaling with KEDA — production-stack
id: autoscaling-with-keda-production-stack
tags:
- internal-llm-inference-stacks-7f0c90
- autoscaling
- vllm
- keda
- primary-oss-docs
created: '2026-10-09T23:06:18.220716Z'
updated: '2026-10-10T14:52:17.212441Z'
source: https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html
source_domain: docs.vllm.ai
fetched_at: '2026-10-09T23:06:18.201001Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Official vLLM Production Stack project documentation (docs.vllm.ai, Helm
  chart v0.1.11+) -- a first-person primary source on the exact open-source composition
  teams use to autoscale vLLM on Kubernetes: KEDA + Prometheus + the vLLM Production
  Stack Helm chart, with kube-prometheus-stack bundled as an optional sub-chart dependency
  since v0.1.11 (so a separate observability stack install is no longer required).
  Walks through the full working configuration: deploy vLLM (example: meta-llama/Llama-3.1-8B-Instruct
  via lmcache/vllm-openai image) with serviceMonitor enabled so Prometheus scrapes
  the engine''s native vllm:num_requests_waiting metric; install KEDA via its official
  Helm chart; wire a KEDA ScaledObject trigger of type ''prometheus'' directly against
  that metric (no prometheus-adapter / Kubernetes Custom Metrics API needed -- KEDA''s
  Prometheus scaler queries Prometheus directly; prometheus-adapter is only needed
  if using a standard HPA with the Custom Metrics API instead of KEDA). Concrete example
  config: minReplicaCount 1, maxReplicaCount 3, pollingInterval 15s, cooldownPeriod
  360s, scale-up threshold of 5 pending requests on vllm:num_requests_waiting. Documents
  scale-to-zero explicitly as an advanced, non-default configuration: setting minReplicaCount:
  0 requires pairing the queue-depth trigger with a second ''traffic-based keepalive''
  Prometheus trigger (querying sum(rate(vllm:num_incoming_requests_total[1m])) > 0)
  specifically to prevent the deployment from scaling to zero while traffic is still
  arriving -- i.e. the project''s own docs treat zero-replica scaling as something
  that needs an extra safety trigger bolted on, not a default-safe behavior. Also
  documents custom HPA scaleDown behavior policies (stabilizationWindowSeconds, percent-based
  step-down) and a fallback block (failureThreshold, static fallback replica count)
  for when the Prometheus metric is unavailable. Verification steps use kubectl get
  scaledobjects / kubectl get hpa and a bundled Python load generator (tutorials/assets/example-10-load-generator.py)
  to trigger and observe scale-up.'
---

*Suggested by [[llm-autoscaling-and-cold-starts-in-kubernetes-architecture-keda-metrics-model-we]] — cited as the vLLM production-stack's own KEDA autoscaling docs*

[Skip to main content](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#main-content)
Back to top
Search
  * [ Repository ](https://github.com/vllm-project/production-stack)
  * [ Suggest edit ](https://github.com/vllm-project/production-stack/edit/main/docs/source/use_cases/autoscaling-keda.rst)




  * System Settings
  * Light


# Autoscaling with KEDA
##  Contents 
# Autoscaling with KEDA 
This tutorial shows you how to automatically scale a vLLM deployment using [KEDA](https://keda.sh/) and Prometheus-based metrics. With the vLLM Production Stack Helm chart (v0.1.11+), KEDA autoscaling and monitoring are integrated directly into the chart, allowing you to enable everything through simple `values.yaml` configuration.
## Table of Contents 
  * [Prerequisites](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#prerequisites)
  *     * [1. Enable the Integrated Monitoring Stack](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#enable-the-integrated-monitoring-stack)
    * [2. Configure and Deploy vLLM](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#configure-and-deploy-vllm)
    * [3. Install KEDA](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#install-keda)
    * [4. Enable KEDA Autoscaling for vLLM](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#enable-keda-autoscaling-for-vllm)
    * [5. Verify KEDA ScaledObject Creation](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#verify-keda-scaledobject-creation)
    * [6. Test Autoscaling](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#test-autoscaling)
    * [7. Advanced Configuration](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#advanced-configuration)
    * [8. Cleanup](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#cleanup)
  * [Additional Resources](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html#additional-resources)


## Prerequisites 
  * Access to a Kubernetes cluster with at least 2 GPUs
  * `kubectl` and `helm` installed (v3.0+)
  * Basic understanding of Kubernetes and Prometheus metrics
  * vLLM Production Stack Helm chart v0.1.11+


## Steps 
### 1. Enable the Integrated Monitoring Stack 
Since v0.1.11, the vLLM Production Stack Helm chart includes `kube-prometheus-stack` and `prometheus-adapter` as optional sub-chart dependencies. There is no longer a need to manually install a separate observability stack — simply enable it in your `values.yaml`:

```
# Enable ServiceMonitors so Prometheus can scrape vLLM metrics
servingEngineSpec:
serviceMonitor:
enabled:true

routerSpec:
serviceMonitor:
enabled:true

# Deploy the full kube-prometheus-stack (Prometheus + Grafana) as a sub-chart
kube-prometheus-stack:
enabled:true

```
Copy to clipboard
Note
If you already have an existing Prometheus stack in your cluster (e.g., deployed via `kube-prometheus-stack` or VictoriaMetrics), you only need to enable the `serviceMonitor` resources. Set `kube-prometheus-stack.enabled: false` and your existing Prometheus Operator will automatically discover the ServiceMonitors.
Note
`prometheus-adapter` is **not** required for KEDA autoscaling. KEDA queries Prometheus directly via its built-in Prometheus scaler. You only need `prometheus-adapter` if you want to use the Kubernetes Custom Metrics API with a standard HPA.
### 2. Configure and Deploy vLLM 
Create a `values.yaml` file to deploy vLLM with monitoring enabled. Note that we’ll enable KEDA autoscaling in a later step after KEDA is installed:

```
servingEngineSpec:
enableEngine:true
serviceMonitor:
enabled:true
modelSpec:
-name:"llama3"
repository:"lmcache/vllm-openai"
tag:"latest"
modelURL:"meta-llama/Llama-3.1-8B-Instruct"
replicaCount:1
requestCPU:10
requestMemory:"64Gi"
requestGPU:1

routerSpec:
serviceMonitor:
enabled:true

kube-prometheus-stack:
enabled:true

```
Copy to clipboard
Deploy the chart:

```
helminstallvllmvllm/vllm-stack-fvalues.yaml

```
Copy to clipboard
Wait for the vLLM deployment to be ready and verify that metrics are being exposed:

```
kubectlwait--for=condition=readypod-lmodel=llama3--timeout=300s

```
Copy to clipboard
Verify Prometheus is scraping the vLLM metrics:

```
kubectlport-forwardsvc/vllm-kube-prometheus-stack-prometheus9090:9090

```
Copy to clipboard
In a separate terminal:

```
curl-G'http://localhost:9090/api/v1/query'--data-urlencode'query=vllm:num_requests_waiting'

```
Copy to clipboard
### 3. Install KEDA 
Now that vLLM is running and exposing metrics, install KEDA to enable autoscaling:

```
kubectlcreatenamespacekeda
helmrepoaddkedacorehttps://kedacore.github.io/charts
helmrepoupdate
helminstallkedakedacore/keda--namespacekeda

```
Copy to clipboard
Verify KEDA is running:

```
kubectlgetpods-nkeda

```
Copy to clipboard
### 4. Enable KEDA Autoscaling for vLLM 
Update your `values.yaml` file to enable KEDA autoscaling:

```
servingEngineSpec:
enableEngine:true
modelSpec:
-name:"llama3"
repository:"lmcache/vllm-openai"
tag:"latest"
modelURL:"meta-llama/Llama-3.1-8B-Instruct"
replicaCount:1
requestCPU:10
requestMemory:"64Gi"
requestGPU:1

# Enable KEDA autoscaling
keda:
enabled:true
minReplicaCount:1
maxReplicaCount:3
pollingInterval:15
cooldownPeriod:360
triggers:
-type:prometheus
metadata:
serverAddress:http://vllm-kube-prometheus-stack-prometheus:9090
metricName:vllm:num_requests_waiting
query:vllm:num_requests_waiting
threshold:'5'

routerSpec:
serviceMonitor:
enabled:true

kube-prometheus-stack:
enabled:true

```
Copy to clipboard
Note
The `serverAddress` in the KEDA trigger must point to your Prometheus instance. When using the integrated sub-chart, the service name follows the pattern `<release-name>-kube-prometheus-stack-prometheus` (here: `vllm-kube-prometheus-stack-prometheus`). If you use an external Prometheus, adjust the address accordingly.
Upgrade the chart to enable KEDA autoscaling:

```
helmupgradevllmvllm/vllm-stack-fvalues.yaml

```
Copy to clipboard
This configuration tells KEDA to:
  * Monitor the `vllm:num_requests_waiting` metric from Prometheus
  * Maintain between 1 and 3 replicas
  * Scale up when the queue exceeds 5 pending requests
  * Check metrics every 15 seconds
  * Wait 360 seconds before scaling down after scaling up


### 5. Verify KEDA ScaledObject Creation 
Check that the Helm chart created the ScaledObject resource:

```
kubectlgetscaledobjects

```
Copy to clipboard
You should see:

```
NAME                        SCALETARGETKIND      SCALETARGETNAME                  MIN   MAX   TRIGGERS     AUTHENTICATION   READY   ACTIVE   FALLBACK   PAUSED    AGE
vllm-llama3-scaledobject   apps/v1.Deployment   vllm-llama3-deployment-vllm      1     3     prometheus                    True    False    Unknown    Unknown   30s

```
Copy to clipboard
View the created HPA:

```
kubectlgethpa

```
Copy to clipboard
Expected output:

```
NAME                            REFERENCE                                TARGETS     MINPODS   MAXPODS   REPLICAS
keda-hpa-vllm-llama3-scaledobject   Deployment/vllm-llama3-deployment-vllm   0/5 (avg)   1         3         1

```
Copy to clipboard
### 6. Test Autoscaling 
Watch the HPA in real-time:

```
kubectlgethpa-ndefault-w

```
Copy to clipboard
Generate load to trigger autoscaling. Port-forward to the router service:

```
kubectlport-forwardsvc/vllm-router-service30080:80

```
Copy to clipboard
In a separate terminal, run a load generator:

```
python3tutorials/assets/example-10-load-generator.py--num-requests100--prompt-len3000

```
Copy to clipboard
Within a few minutes, you should see the `REPLICAS` value increase as KEDA scales up to handle the load.
### 7. Advanced Configuration 
#### Scale-to-Zero 
Enable scale-to-zero by setting `minReplicaCount: 0` and adding a traffic-based keepalive trigger:

```
keda:
enabled:true
minReplicaCount:0# Allow scaling to zero
maxReplicaCount:5
triggers:
# Queue-based scaling
-type:prometheus
metadata:
serverAddress:http://vllm-kube-prometheus-stack-prometheus:9090
metricName:vllm:num_requests_waiting
query:vllm:num_requests_waiting
threshold:'5'
# Traffic-based keepalive (prevents scale-to-zero when traffic exists)
-type:prometheus
metadata:
serverAddress:http://vllm-kube-prometheus-stack-prometheus:9090
metricName:vllm:incoming_keepalive
query:sum(rate(vllm:num_incoming_requests_total[1m]) > bool 0)
threshold:"1"

```
Copy to clipboard
#### Custom HPA Behavior 
Control scaling behavior with custom HPA policies:

```
keda:
enabled:true
minReplicaCount:1
maxReplicaCount:5
advanced:
horizontalPodAutoscalerConfig:
behavior:
scaleDown:
stabilizationWindowSeconds:300
policies:
-type:Percent
value:50
periodSeconds:60

```
Copy to clipboard
#### Fallback Configuration 
Configure fallback behavior when metrics are unavailable:

```
keda:
enabled:true
fallback:
failureThreshold:3
replicas:2

```
Copy to clipboard
For more configuration options, see the [Helm chart README](https://github.com/vllm-project/production-stack/blob/main/helm/README.md#keda-autoscaling-configuration).
### 8. Cleanup 
To disable KEDA autoscaling, update your `values.yaml` to set `keda.enabled: false` and upgrade:

```
helmupgradevllmvllm/vllm-stack-fvalues.yaml

```
Copy to clipboard
To completely remove KEDA from the cluster:

```
helmuninstallkeda-nkeda
kubectldeletenamespacekeda

```
Copy to clipboard
To disable the integrated monitoring, set `kube-prometheus-stack.enabled: false` in your `values.yaml` and upgrade:

```
helmupgradevllmvllm/vllm-stack-fvalues.yaml

```
Copy to clipboard
## Additional Resources 
  * [KEDA Documentation](https://keda.sh/docs/)
  * [KEDA ScaledObject Specification](https://keda.sh/docs/2.18/reference/scaledobject-spec/)
  * [Helm Chart KEDA Configuration](https://github.com/vllm-project/production-stack/blob/main/helm/README.md#keda-autoscaling-configuration)
  * [Monitoring Sub-Chart Integration (PR #860)](https://github.com/vllm-project/production-stack/pull/860)


Contents 
