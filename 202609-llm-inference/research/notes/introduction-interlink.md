---
title: Introduction | interLink
id: introduction-interlink
tags:
- llmd-router-nonk8s-backends-54b774
- virtual-kubelet
- hpc-offload
- non-k8s-backend
- negative-finding
created: '2026-09-11T20:51:31.144843Z'
updated: '2026-09-15T04:55:23.875861Z'
source: https://interlink-project.dev/docs/intro
source_domain: interlink-project.dev
fetched_at: '2026-09-11T20:51:31.123658Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'interLink project docs (v0.6.x, fetched as substitute for a paywalled CHEP2025
  conference paper on the same project) describe a Virtual-Kubelet-based system letting
  a ''local'' k8s cluster offload container execution to remote systems (another k8s
  cluster, HPC/SLURM/HTCondor batch systems, on-demand VMs, or lambda-like services)
  via a pluggable interLink API + provider-plugin spec. Directly relevant to router-customization
  for non-pod backends: interLink explicitly states in a ''NOT a target'' section
  that it is NOT designed for long-running, continuously available services -- ''Our
  solution is not designed for services that need to run continuously for extended
  periods. It is optimized for tasks that have a defined start and end, rather than
  persistent services exposing intra-cluster communication endpoints.'' This is a
  load-bearing negative finding: interLink''s virtual-pod abstraction targets batch/job-style
  HPC offload (AI training, ML optimization jobs, GPU-heavy on-demand runs), NOT stable,
  continuously reachable inference-serving endpoints that an EPP/router would pick
  as a routing destination -- so interLink is not a fit for making a long-lived non-k8s
  inference backend directly reachable through the standard virtual-kubelet Pod abstraction;
  a custom EPP/router backend-discovery plugin talking to the remote scheduler directly
  (Slurm, VM, etc.) is a better-aligned integration point than routing traffic through
  an interLink virtual pod. Three deployment topologies are documented (edge-node,
  in-cluster, tunneled), the last supporting secure tunneled communication to remote
  interLink API/plugin servers behind NAT or on shared login nodes.'
utility_score: 10.0
---

[Skip to main content](https://interlink-project.dev/docs/intro#__docusaurus_skipToContent_fallback)
We will be at KubeCon EU 2026! Come visit us at booth P-24AS Wednesday 10:00 - 13:30 to learn more about interLink!
Version: 0.6.x
On this page
# Introduction
warning
interLink is in early development phase, thus subject to breaking changes with no guarantee of backward compatibility.
## Overview[​](https://interlink-project.dev/docs/intro#overview "Direct link to Overview")
interLink enables whoever is running a Kubernetes cluster (named as "local" here) to offload some of the containers to be executed on other (remote) systems; e.g. another K8S cluster, an HPC cluster, any other batch systems or VMs.
Usually the best fit for such an architecture are batch-like (or "job") payloads -- with a pre-defined lifecycle, non-interactive containers (see [Targets](https://interlink-project.dev/docs/intro#target-applications)). The dispatching to the other (remote) system is done through a combination of [Virtual Kubelets](https://virtual-kubelet.io/) interface and [interLink API plugins spec](https://interlink-project.dev/docs/guides/api-reference). Plugins will define how the containers will run on the remote system (see [Target providers](https://interlink-project.dev/docs/intro#providers)).
InterLink API and the plugin can be arranged in three different ways across the local cluster and the remote part:
  * both deployed remote ()
  * both deployed local ()
  * API local, plugin remote ()



```
+---------------------------+  +----------------------------+| Virtual K8s Node          |  |         Containers Runtime ||                           |  |                            ||                           |  |                            ||                           |  |                            ||        +-----------------------------------------+        ||        | (API + plugin) interLink                |        ||        |          (API) interLink (plugin)       |        ||        |                interLink (API + plugin) |        ||        +-----------------------------------------+        ||                           |  |                            ||                           |  |                            ||                           |  |                            ||                           |  |                            ||                           |  |                            ||                           |  |                            |+---------------------------+  +----------------------------+
```

For more information visit the [architecture page](https://interlink-project.dev/docs/arch)
## Target applications[​](https://interlink-project.dev/docs/intro#target-applications "Direct link to Target applications")
  * **K8s applications with tasks to be executed on HPC systems** : This target focuses on Kubernetes applications that require high-performance computing (HPC) resources for executing tasks (AI training and inference, ML algorithm optimizations etc). These tasks might involve complex computations, simulations, or data processing that benefit from the specialized hardware and optimized performance of HPC systems.
  * **Remote "runner"-like application for heavy payload execution requiring GPUs** : This target is designed for applications that need to execute heavy computational payloads, particularly those requiring GPU resources. These applications can be run remotely, leveraging powerful GPU hardware to handle tasks such as machine learning model training, data analysis, or rendering.
  * **Lambda-like functions calling on external resources** : This target involves running containers on demand with specific computing needs. Now these resources might also be outside of the Kubernetes cluster thanks to interLink functionality.


## Providers[​](https://interlink-project.dev/docs/intro#providers "Direct link to Providers")
interLink is designed to ease the work required to include new remote providers. It already targets a wide range of providers with container execution capabilities, including but not limited to:
  * **SLURM or HTCondor batch systems with Apptainer, Enroot, or Singularity** : These batch systems are widely used in high-performance computing environments to manage and schedule jobs. By integrating with container runtimes like Apptainer, Enroot, or Singularity, our solution can efficiently execute containerized tasks on these systems.
  * **On-demand virtual machines with any container runtime** : This includes virtual machines that can be provisioned on-demand and support container runtimes such as Docker, Podman, or others. This flexibility allows for scalable and dynamic resource allocation based on workload requirements.
  * **Remote Kubernetes clusters** : Our solution can extend the capabilities of existing Kubernetes clusters, enabling them to offload workloads to another remote cluster. This is particularly useful for distributing workloads across multiple clusters for better resource utilization and fault tolerance.
  * **Lambda-like services** : These are serverless computing services that execute code in response to events and automatically manage the underlying compute resources. By targeting these services, our solution can leverage the scalability and efficiency of serverless architectures for containerized workloads. All of this, while exposing a bare Kubernetes API kind of orchestration.


## NOT a target[​](https://interlink-project.dev/docs/intro#not-a-target "Direct link to NOT a target")
  * **Long-running services** : Our solution is not designed for services that need to run continuously for extended periods. It is optimized for tasks that have a defined start and end, rather than persistent services exposing intra-cluster communication endpoints.
  * **Kubernetes Federation** : We do not aim to support Kubernetes Federation, which involves managing multiple Kubernetes clusters as a single entity. Our focus is on enabling Kubernetes pods to execute on remote resources, not on federating all kind of resources on multiple clusters.


## Deployment scenarios[​](https://interlink-project.dev/docs/intro#deployment-scenarios "Direct link to Deployment scenarios")
### Edge-node[​](https://interlink-project.dev/docs/intro#edge-node "Direct link to Edge-node")
In this scenario, the Virtual Kubelet communicates with remote services deployed on a dedicate edge node exposing authenticated interLink APIs and its associated plugin. This setup is ideal for scenarios where edge computing resources are utilized for controlled communication b/w the Kubernetes cluster and the remote resources.
note
Get started with edge-node deployment [here](https://interlink-project.dev/docs/cookbook/edge)
### In-cluster[​](https://interlink-project.dev/docs/intro#in-cluster "Direct link to In-cluster")
This scenario involves deploying a Virtual Kubelet along with the interLink API server and the plugin to interact with a remote API. This setup allows Kubernetes pods to be executed on remote resources while all other components sits inside the Kubernetes cluster.
note
Get started with in-cluster deployment [here](https://interlink-project.dev/docs/cookbook/incluster)
### Tunneled[​](https://interlink-project.dev/docs/intro#tunneled "Direct link to Tunneled")
This deployment involves the Virtual Kubelet connecting to a remote interLink API server and its plugin through a secure tunnel. This setup ensures secure communication between the Kubernetes cluster and the remote resources, making it suitable for environments with strict security requirements or to host services on a multi user host like a login node.
note
Get started with tunneled deployment [here](https://interlink-project.dev/docs/cookbook/tunneled)
  * [Deployment scenarios](https://interlink-project.dev/docs/intro#deployment-scenarios)


