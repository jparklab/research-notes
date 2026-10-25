---
title: How to Set Up GPU Time-Slicing and MIG Partitioning
id: how-to-set-up-gpu-time-slicing-and-mig-partitioning
tags:
- onprem-llm-inference-stacks-859099
- gpu-sharing
- provenance-vendor-blog
- how-to
created: '2026-10-23T17:23:18.144795Z'
updated: '2026-10-23T17:25:05.806322Z'
source: https://oneuptime.com/blog/post/2026-02-09-gpu-time-slicing-mig-kubernetes/view
source_domain: oneuptime.com
fetched_at: '2026-10-23T17:23:18.123951Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'OneUptime (observability vendor) tutorial, 9 Feb 2026: GPU Operator time-slicing
  ConfigMap (replicas 4, migStrategy none), MIG manager config (all-1g.5gb, 28 slices
  on 4 A100s), mixed per-GPU layouts (GPU0 3x2g.10gb, GPU1 7x1g.5gb, GPU2 1x4g.20gb,
  GPU3 full). Generic how-to with no measurements; uses A100-40GB profile names.'
---

[Skip to main content](https://oneuptime.com/blog/post/2026-02-09-gpu-time-slicing-mig-kubernetes/view#main-content)
#  How to Set Up GPU Time-Slicing and MIG Partitioning 
Learn how to configure GPU time-slicing and NVIDIA Multi-Instance GPU (MIG) partitioning to maximize GPU utilization for machine learning workloads on Kubernetes. 
By @nawazdhandala
• Feb 09, 2026 • Reading time 7 min read
[ Kubernetes  ](https://oneuptime.com/blog/tag/kubernetes) [ NVIDIA  ](https://oneuptime.com/blog/tag/nvidia) [ Machine Learning  ](https://oneuptime.com/blog/tag/machine-learning) [ Resource Management  ](https://oneuptime.com/blog/tag/resource-management)
##  On this page 
GPUs are expensive resources, and running ML workloads efficiently requires making the most of every GPU. When you have small models or inference workloads that don't fully utilize a GPU, sharing that GPU across multiple pods becomes critical. NVIDIA provides two main approaches for GPU sharing: time-slicing, where pods take turns using the GPU, and MIG (Multi-Instance GPU), which creates hardware-isolated GPU partitions. This guide shows you how to configure both approaches on Kubernetes.
## Understanding GPU Sharing Options 
Before diving into configuration, understand the trade-offs:
**Time-Slicing:**
  * Works on most NVIDIA GPUs (Kepler and newer)
  * No hardware isolation between workloads
  * Best for workloads with intermittent GPU usage
  * Simple to configure
  * Potential for resource contention


**MIG (Multi-Instance GPU):**
  * Only available on select NVIDIA GPUs, including A100, A30, H100, H200, and newer MIG-capable data center or workstation GPUs
  * Hardware-isolated GPU partitions
  * Guaranteed resources per partition
  * Better for predictable performance
  * More complex to configure


Choose time-slicing for development environments and non-critical workloads. Use MIG for production workloads that need performance guarantees.
## Installing the NVIDIA GPU Operator 
Start by installing the GPU Operator, which manages GPU drivers and device plugins:

```


BashCopy

# Add NVIDIA Helm repository

helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
helm repo update

# Install GPU Operator with default settings
helm install gpu-operator nvidia/gpu-operator \
  --namespace gpu-operator \
  --create-namespace \
  --set driver.enabled=true \
  --wait

# Verify installation
kubectl get pods -n gpu-operator

# Check GPU nodes are labeled
kubectl get nodes -l nvidia.com/gpu.present=true
```

Verify GPUs are detected:

```


BashCopy

# Check GPU resources on nodes
kubectl describe node <gpu-node-name> | grep nvidia.com/gpu

# Should show something like:
# nvidia.com/gpu: 4
```

## Configuring GPU Time-Slicing 
Time-slicing lets multiple pods share a single physical GPU by scheduling their workloads sequentially.
Create a time-slicing configuration:

```


YAMLCopy

# time-slicing-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-slicing-config
  namespace: gpu-operator
data:
  # Each GPU will be shared by up to 4 pods
  any: |
    version: v1
    flags:
      migStrategy: none
    sharing:
      timeSlicing:
        resources:
        - name: nvidia.com/gpu
          replicas: 4
```

Apply the configuration:

```


BashCopy

# Create the ConfigMap
kubectl apply -f time-slicing-config.yaml

# Update GPU Operator to use time-slicing
helm upgrade gpu-operator nvidia/gpu-operator \
  --namespace gpu-operator \
  --set devicePlugin.config.name=time-slicing-config \
  --set devicePlugin.config.default=any \
  --reuse-values

# Wait for device plugin to restart
kubectl rollout status daemonset -n gpu-operator nvidia-device-plugin-daemonset

# Verify GPUs are now showing 4x capacity
kubectl describe node <gpu-node-name> | grep nvidia.com/gpu
# Should now show: nvidia.com/gpu: 16 (if you had 4 physical GPUs)
```

Deploy a test workload to verify time-slicing:

```


YAMLCopy

# time-slicing-test.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gpu-timeslice-test
  namespace: default
spec:
  replicas: 8  # More replicas than physical GPUs
  selector:
    matchLabels:
      app: gpu-timeslice-test
  template:
    metadata:
      labels:
        app: gpu-timeslice-test
    spec:
      containers:
      - name: cuda-test
        image: nvidia/cuda:11.8.0-base-ubuntu22.04
        command: ["bash", "-c"]
        args:
        - |
          while true; do
            nvidia-smi
            echo "Running GPU workload..."
            sleep 30
          done
        resources:
          limits:
            nvidia.com/gpu: 1  # Each pod requests 1 "slice"
```

Deploy and verify:

```


BashCopy

kubectl apply -f time-slicing-test.yaml

# Check that all 8 pods are running
kubectl get pods -l app=gpu-timeslice-test

# Verify multiple pods are sharing GPUs
kubectl exec -it <pod-name> -- nvidia-smi
# You'll see multiple processes on the same GPU
```

## Configuring MIG Partitioning 
MIG creates hardware-isolated GPU instances. First, check if your GPUs support MIG:

```


BashCopy

# SSH to GPU node or use a privileged pod
nvidia-smi mig -lgip
# If supported, you'll see available profiles like:
# 1g.5gb, 2g.10gb, 3g.20gb, etc.
```

Enable MIG mode on GPUs:

```


BashCopy

# Enable MIG mode (requires GPU reset)
nvidia-smi -mig 1

# Verify MIG is enabled
nvidia-smi --query-gpu=index,mig.mode.current --format=csv
# Should show MIG mode as Enabled
```

If you are configuring MIG manually outside the GPU Operator, create MIG instances:

```


BashCopy

# Create 7 equal instances (1g.5gb profile) on each A100
# This gives you 7 separate 5GB GPU instances per physical GPU

for gpu_id in 0 1 2 3; do
  nvidia-smi mig -cgi 19,19,19,19,19,19,19 -C -i $gpu_id
done

# Verify instances were created
nvidia-smi mig -lgi

# You should see something like:
# GPU 0: 7x 1g.5gb instances
# GPU 1: 7x 1g.5gb instances
# ...
```

Configure the GPU Operator to use MIG:

```


YAMLCopy

# mig-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mig-config
  namespace: gpu-operator
data:
  config.yaml: |
    version: v1
    mig-configs:
      all-disabled:
        - devices: all
          mig-enabled: false
      # Configure for 1g.5gb profile (7 instances per GPU)
      all-1g.5gb:
        - devices: [0,1,2,3]  # Apply to all GPUs
          mig-enabled: true
          mig-devices:
            "1g.5gb": 7  # Create 7 instances per GPU
```

Apply MIG configuration:

```


BashCopy

# Create the ConfigMap
kubectl apply -f mig-config.yaml

# Update GPU Operator to use MIG
helm upgrade gpu-operator nvidia/gpu-operator \
  --namespace gpu-operator \
  --set mig.strategy=mixed \
  --set migManager.config.name=mig-config \
  --reuse-values

# Tell MIG Manager which MIG profile to apply to the node
kubectl label nodes <gpu-node-name> nvidia.com/mig.config=all-1g.5gb --overwrite

# Wait for MIG Manager to finish applying the configuration
kubectl get node <gpu-node-name> -o=jsonpath='{.metadata.labels.nvidia\.com/mig\.config\.state}'

# Verify MIG resources are available
kubectl describe node <gpu-node-name> | grep "nvidia.com/mig"
# Should show: nvidia.com/mig-1g.5gb: 28 (7 per GPU x 4 GPUs)
```

## Creating Mixed MIG Profiles 
You can create different MIG profiles for different workload types:

```


BashCopy

# Example: Mix of small and large instances
# GPU 0: 3x 2g.10gb instances
nvidia-smi mig -cgi 14,14,14 -C -i 0

# GPU 1: 7x 1g.5gb instances (for inference)
nvidia-smi mig -cgi 19,19,19,19,19,19,19 -C -i 1

# GPU 2: 1x 4g.20gb instance (for training)
nvidia-smi mig -cgi 5 -C -i 2

# GPU 3: Keep as full GPU
# (don't create any MIG instances)

# Verify the configuration
nvidia-smi mig -lgi
```

Configure the operator for mixed profiles:

```


YAMLCopy

# mixed-mig-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mixed-mig-config
  namespace: gpu-operator
data:
  config.yaml: |
    version: v1
    mig-configs:
      all-disabled:
        - devices: all
          mig-enabled: false
      mixed:
        - devices: [0]
          mig-enabled: true
          mig-devices:
            "2g.10gb": 3
        - devices: [1]
          mig-enabled: true
          mig-devices:
            "1g.5gb": 7
        - devices: [2]
          mig-enabled: true
          mig-devices:
            "4g.20gb": 1
        - devices: [3]
          mig-enabled: false
```

## Deploying Workloads on MIG Instances 
Deploy inference workloads on small MIG instances:

```


YAMLCopy

# inference-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bert-inference
  namespace: ml-inference
spec:
  replicas: 7  # Can run 7 replicas on 7x1g.5gb instances
  selector:
    matchLabels:
      app: bert-inference
  template:
    metadata:
      labels:
        app: bert-inference
    spec:
      containers:
      - name: inference
        image: your-registry/bert-inference:v1
        resources:
          requests:
            nvidia.com/mig-1g.5gb: 1  # Request 1 MIG instance
            memory: "4Gi"
            cpu: "2"
          limits:
            nvidia.com/mig-1g.5gb: 1
            memory: "4Gi"
            cpu: "2"
        env:
        - name: CUDA_MIG_ENABLED
          value: "1"
```

Deploy training workloads on larger MIG instances:

```


YAMLCopy

# training-job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: model-training
  namespace: ml-training
spec:
  template:
    spec:
      restartPolicy: OnFailure
      containers:
      - name: trainer
        image: your-registry/pytorch-trainer:v1
        command: ["python", "train.py"]
        resources:
          requests:
            nvidia.com/mig-2g.10gb: 1  # Larger instance for training
            memory: "16Gi"
            cpu: "4"
          limits:
            nvidia.com/mig-2g.10gb: 1
            memory: "16Gi"
            cpu: "4"
        env:
        - name: CUDA_MIG_ENABLED
          value: "1"
```

## Monitoring GPU Utilization 
Deploy DCGM Exporter to monitor GPU metrics:

```


BashCopy

# Install DCGM Exporter
helm repo add gpu-helm-charts https://nvidia.github.io/dcgm-exporter/helm-charts
helm repo update
helm install dcgm-exporter gpu-helm-charts/dcgm-exporter \
  --namespace gpu-operator \
  --create-namespace \
  --set serviceMonitor.enabled=true

# Verify it's collecting metrics
kubectl port-forward -n gpu-operator svc/dcgm-exporter 9400:9400

# Query metrics
curl localhost:9400/metrics | grep DCGM
```

Query GPU utilization by MIG instance:

```


PromqlCopy

# GPU utilization by MIG instance
DCGM_FI_PROF_GR_ENGINE_ACTIVE{GPU_I_ID!=""}

# Memory usage by MIG instance
DCGM_FI_DEV_FB_USED{GPU_I_ID!=""}
```

Create alerts for GPU issues:

```


YAMLCopy

# gpu-alerts.yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: gpu-alerts
  namespace: gpu-operator
spec:
  groups:
  - name: gpu
    interval: 30s
    rules:
    - alert: GPUMemoryHigh
      expr: DCGM_FI_DEV_FB_USED / (DCGM_FI_DEV_FB_USED + DCGM_FI_DEV_FB_FREE)  0.9
      for: 5m
      annotations:
        summary: "GPU memory usage above 90%"

    - alert: GPUUtilizationLow
      expr: avg_over_time(DCGM_FI_DEV_GPU_UTIL[10m])  20
      for: 30m
      annotations:
        summary: "GPU utilization below 20% for 30 minutes"
```

## Switching Between Time-Slicing and MIG 
To switch from MIG back to time-slicing:

```


BashCopy

# Disable MIG on all GPUs
kubectl label nodes <gpu-node-name> nvidia.com/mig.config=all-disabled --overwrite

# Update GPU Operator back to time-slicing
helm upgrade gpu-operator nvidia/gpu-operator \
  --namespace gpu-operator \
  --set mig.strategy=none \
  --set devicePlugin.config.name=time-slicing-config \
  --set devicePlugin.config.default=any \
  --reuse-values

# Restart device plugin
kubectl delete pods -n gpu-operator -l app=nvidia-device-plugin-daemonset
```

## Best Practices 
Configure resource quotas for GPU namespaces:

```


YAMLCopy

# gpu-quota.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: gpu-quota
  namespace: ml-inference
spec:
  hard:
    nvidia.com/mig-1g.5gb: "10"  # Limit to 10 small instances
    requests.memory: "50Gi"
    limits.memory: "50Gi"
```

Use node affinity to control workload placement:

```


YAMLCopy

# Target specific GPU types
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: nvidia.com/gpu.product
          operator: In
          values: ["A100-SXM4-40GB"]
```

## Conclusion 
GPU time-slicing and MIG partitioning give you flexible options for sharing GPU resources across ML workloads on Kubernetes. Time-slicing works well for development and testing environments where simplicity matters more than isolation. MIG provides production-grade isolation with guaranteed resources, making it ideal for critical inference services and training jobs that need predictable performance. By choosing the right approach for each workload, you can maximize GPU utilization while maintaining the performance characteristics your applications need.
Share this article
### Nawaz Dhandala
Author
@nawazdhandala • Feb 09, 2026 • 7 min read
Nawaz is building OneUptime with a passion for engineering reliable systems and improving observability.
Technically validated · Jun 04, 2026 View report 
### Help improve this post
Every OneUptime blog post is open source. Found a typo, an inaccuracy, or have a clearer way to explain something? Anyone can contribute — your edits make this post better for everyone who reads it next.
[ Edit this post on GitHub ](https://github.com/oneuptime/blog/tree/master/posts/2026-02-09-gpu-time-slicing-mig-kubernetes) [ Contributing guidelines ](https://github.com/oneuptime/blog)
[ Open source ](https://github.com/oneuptime/oneuptime)
##  OneUptime is the Open-Source Observability Platform 
Your complete reliability stack unified: infrastructure monitoring, incident management, status pages, and APM. Open-source and self-hostable. 
[ Status Page Real-time status updates ](https://oneuptime.com/product/status-page) [ Incidents Detect and resolve fast ](https://oneuptime.com/product/incident-management) [ Monitoring Monitor any resource ](https://oneuptime.com/product/monitoring) [ On-Call Smart alert routing ](https://oneuptime.com/product/on-call) [ Maintenance Plan & communicate downtime ](https://oneuptime.com/product/scheduled-maintenance) [ Logs Fastest log ingest and search ](https://oneuptime.com/product/logs-management) [ Metrics Performance insights ](https://oneuptime.com/product/metrics) [ Traces End-to-end distributed tracing ](https://oneuptime.com/product/traces) [ Exceptions Catch and fix bugs early ](https://oneuptime.com/product/exceptions) [ Workflows Automate any process ](https://oneuptime.com/product/workflows) [ Dashboards Visualize all your data ](https://oneuptime.com/product/dashboards) [ Kubernetes Monitor K8s clusters ](https://oneuptime.com/product/kubernetes) [ Profiles CPU & memory profiling ](https://oneuptime.com/product/profiles)
[ Detect, diagnose, and resolve incidents with AI-powered root cause analysis and code fixes. ](https://oneuptime.com/product/ai-agent)
We use cookies the site needs to work. Analytics and advertising cookies help us understand how the site is used and measure our marketing, and you choose whether we use them.
"Accept all" allows them. "Reject all" turns them off and removes the ones already set. You can change your answer at any time from "Cookie settings" at the bottom of every page.
Read more in our [cookie policy](https://oneuptime.com/legal/cookies) and our [privacy policy](https://oneuptime.com/legal/privacy).
Accept all Reject all
## Validation report
Technically reviewed for accuracy • Jun 04, 2026
Loading validation report… 
Automated technical review  Close
