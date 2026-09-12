---
title: Multi-cluster GKE Inference Gateway helps scale AI workloads | Google Cloud
  Blog
id: multi-cluster-gke-inference-gateway-helps-scale-ai-workloads-google-cloud-blog
tags:
- llmd-router-nonk8s-backends
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:19.084044Z'
updated: '2026-09-12T03:37:37.566404Z'
source: https://cloud.google.com/blog/products/containers-kubernetes/multi-cluster-gke-inference-gateway-helps-scale-ai-workloads/
source_domain: cloud.google.com
fetched_at: '2026-09-11T19:44:19.063739Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: Google Cloud announces (Mar 17, 2026 preview) multi-cluster GKE Inference
  Gateway, built on multi-cluster Gateways. Each 'target cluster' still runs a normal
  InferencePool grouping Pods locally; that pool is exported and surfaces in the 'config
  cluster' as a new, GCP-proprietary GCPInferencePoolImport resource, to which a GCPBackendPolicy
  attaches for cross-cluster load-balancing signals (KV-cache metric, in-flight limits).
  Standard Gateway/HTTPRoute in the config cluster route to the imports. This is vendor-specific
  plumbing on top of the OSS InferencePool concept, not a portable non-k8s-backend
  mechanism -- InferencePool itself still only ever groups Pods within a single cluster;
  cross-cluster reach is solved by a new import CRD + GCP control plane, not by relaxing
  what InferencePool can select.
utility_score: 12.0
---

Containers & Kubernetes
# 
Introducing multi-cluster GKE Inference Gateway: Scale AI workloads around the world
March 17, 2026
##### Arman Rye
Senior Product Manager
##### Andres Guedez
Senior Staff Software Engineer
##### Try Gemini Enterprise today
The front door to AI in the workplace
[Try now ](https://business.gemini.google/?utm_source=cloud.google.com/blog&utm_medium=et&utm_campaign=FY26-Q2-GLOBAL-GLO27877-physicalevent-er-next26-mc-105752)
The world of artificial intelligence is moving fast, and so is the need to serve models reliably and at scale. Today, we're thrilled to announce the preview of **multi-cluster GKE Inference Gateway** to enhance the scalability, resilience, and efficiency of your AI/ML inference workloads across multiple Google Kubernetes Engine (GKE) clusters — even those spanning different Google Cloud regions.
Built as an extension of the [GKE Gateway API](https://cloud.google.com/kubernetes-engine/docs/concepts/gateway-api), the multi-cluster Inference Gateway leverages the power of [multi-cluster Gateways](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/multi-cluster-gateways) to provide intelligent, model-aware load balancing for your most demanding AI applications.
### **Why multi-cluster for AI inference?**
As AI models grow in complexity and users become more global, single-cluster deployments can face limitations:
  * **Availability risks:** Regional outages or cluster maintenance can impact service.
  * **Scalability caps:** Hitting hardware limits (GPUs/TPUs) within a single cluster or region.
  * **Resource silos:** Underutilized accelerator capacity in one cluster can’t be used by another
  * **Latency:** Users far from your serving cluster may experience higher latency


The multi-cluster GKE Inference Gateway addresses these challenges head-on, providing a variety of features and benefits:
  * **Enhanced high reliability and fault tolerance:** Intelligently route traffic across multiple GKE clusters, including across different regions. If one cluster or region experiences issues, traffic is automatically re-routed, minimizing downtime.
  * **Improved scalability and optimized resource usage:** Pool and leverage GPU/TPU resources from various clusters. Handle demand spikes by bursting beyond the capacity of a single cluster and efficiently utilize available accelerators across your entire fleet.
  * **Globally optimized, model-aware routing:** The Inference Gateway can make smart routing decisions using advanced signals. With `GCPBackendPolicy`, you can configure load balancing based on real-time custom metrics, such as the model server's KV cache utilization metric, so that requests are sent to the best-equipped backend instance. Other modes like in-flight request limits are also supported.
  * **Simplified operations:** Manage traffic to a globally distributed AI service through a single Inference Gateway configuration in a dedicated GKE "config cluster," while your models run in multiple "target clusters."


### **How it works**
In GKE Inference Gateway there are two foundational resources, `InferencePool` and `InferenceObjective`. An `InferencePool` acts as a resource group for pods that share the same compute hardware (like GPUs or TPUs) and model configuration, helping to ensure scalable and high-availability serving. An `InferenceObjective` defines the specific model names and assigns serving priorities, allowing Inference Gateway to intelligently route traffic and multiplex latency-sensitive tasks alongside less urgent workloads.
With this release, the system uses Kubernetes Custom Resources to manage your distributed inference service. `InferencePool` resources in each "target cluster" group model-server backends. These backends are exported and become visible as `GCPInferencePoolImport` resources in the "config cluster." Standard `Gateway` and `HTTPRoute` resources in the config cluster define the entry point and routing rules, directing traffic to these imported pools. Fine-grained load-balancing behaviors, such as using `CUSTOM_METRICS` or `IN_FLIGHT` requests, are configured using the `GCPBackendPolicy` resource attached to `GCPInferencePoolImport`.
This architecture enables use cases like global low-latency serving, disaster recovery, capacity bursting, and efficient use of heterogeneous hardware.
For more information about GKE Inference Gateway core concepts check out our [guide](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#understand_key_concepts).
### **Get started today**
As you scale your AI inference serving workloads to more users in more places, we're excited for you to try multi-cluster GKE Inference Gateway. To learn more and get started, check out the documentation:
  * [About multi-cluster GKE Inference Gateway](https://cloud.google.com/kubernetes-engine/docs/concepts/about-multi-cluster-inference-gateway)
  * [Set up multi-cluster GKE Inference Gateway](https://cloud.google.com/kubernetes-engine/docs/how-to/setup-multicluster-inference-gateway)
  * [Customize backend configurations with GCPBackendPolicy](https://cloud.google.com/kubernetes-engine/docs/how-to/customize-backend-multicluster-inference-gateway)


Posted in
  * [Containers & Kubernetes](https://cloud.google.com/blog/products/containers-kubernetes)
  * [AI & Machine Learning](https://cloud.google.com/blog/products/ai-machine-learning)
  * [Developers & Practitioners](https://cloud.google.com/blog/topics/developers-practitioners)


##### Related articles
[ AI infrastructure What’s new in AI infrastructure and orchestration in August By Alex Barrett • 13-minute read ](https://cloud.google.com/blog/topics/ai-infrastructure/whats-new-in-ai-infrastructure-this-month)
[ Containers & Kubernetes Bringing gVisor sandboxes to distributed Ray clusters By Andrew Sy Kim • 3-minute read ](https://cloud.google.com/blog/products/containers-kubernetes/gvisor-sandboxes-for-ray-clusters-on-gke)
[ Networking ClusterNetworkPolicy in GKE: Balancing control and autonomy for your microservices By Srini Jasti • 4-minute read ](https://cloud.google.com/blog/products/networking/new-clusternetworkpolicy-in-gke)
[ Containers & Kubernetes Do more with less: How GKE can reduce your cost per agent by 75% By Drake Williams • 6-minute read ](https://cloud.google.com/blog/products/containers-kubernetes/reduce-your-agents-costs-with-gke-agent-sandbox)
