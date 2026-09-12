---
title: 'GitHub - virtual-kubelet/virtual-kubelet: Virtual Kubelet is an open source
  Kubernetes kubelet implementation. · GitHub'
id: github-virtual-kubeletvirtual-kubelet-virtual-kubelet-is-an-open-source-kubernet
tags:
- llmd-router-nonk8s-backends
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:46:04.226152Z'
updated: '2026-09-12T03:37:37.625027Z'
source: https://github.com/virtual-kubelet/virtual-kubelet
source_domain: github.com
fetched_at: '2026-09-11T19:46:04.203372Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'virtual-kubelet (CNCF, v1.14.0 as of fetch, 4.6k stars) is the library underlying
  interLink, Liqo, Azure ACI, AWS Fargate, Admiralty and other virtual-node providers.
  Core contract: a provider implements PodLifecycleHandler (CreatePod/UpdatePod/DeletePod/GetPod/GetPodStatus/GetPods,
  all operating on real corev1.Pod/PodStatus objects) plus NodeProvider (Ping, NotifyNodeStatus);
  virtual-kubelet itself imposes no constraint on what PodIP the provider reports
  in PodStatus -- that value, and whether it is actually reachable, is entirely up
  to the provider implementation, not the framework. README explicitly states VK "is
  explicitly not intended to be an alternative to Kubernetes federation," ruling it
  out as a multi-cluster pattern. Documents a load-balancer integration quirk under
  "Known quirks": because virtual-kubelet nodes are not normal kubelets, Kubernetes
  control-plane versions before the ServiceNodeExclusion feature-gate (K8s 1.9+) may
  try to add virtual-kubelet nodes to cloud Load Balancer pools incorrectly; the documented
  workaround is enabling --feature-gates=ServiceNodeExclusion=true on the controller-manager.
  Liqo provider is noted as the one implementation that explicitly extends real pod-to-pod
  networking across peered clusters ("enabling Pod-to-Pod traffic and multi-cluster
  east-west services, supporting endpoints on both clusters"), unlike most other providers.'
utility_score: 11.0
---

[Skip to content](https://github.com/virtual-kubelet/virtual-kubelet#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/virtual-kubelet/virtual-kubelet) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/virtual-kubelet/virtual-kubelet) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/virtual-kubelet/virtual-kubelet) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/virtual-kubelet/virtual-kubelet).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fvirtual-kubelet%2Fvirtual-kubelet) You must be signed in to change notification settings
  * [ Fork 663 ](https://github.com/login?return_to=%2Fvirtual-kubelet%2Fvirtual-kubelet)
  * [ Star  4.6k ](https://github.com/login?return_to=%2Fvirtual-kubelet%2Fvirtual-kubelet)


[**15** Branches](https://github.com/virtual-kubelet/virtual-kubelet/branches)[](https://github.com/virtual-kubelet/virtual-kubelet/tags)
Go to file
Open more actions menu
## Latest commit
and
[build(deps): bump google.golang.org/grpc from 1.83.1 to 1.83.2](https://github.com/virtual-kubelet/virtual-kubelet/commit/24d833a96269b31793154072084d953fa7b6e3a9)
Open commit details
Sep 9, 2026
[24d833a](https://github.com/virtual-kubelet/virtual-kubelet/commit/24d833a96269b31793154072084d953fa7b6e3a9) · Sep 9, 2026
## History
[1,151 Commits](https://github.com/virtual-kubelet/virtual-kubelet/commits/main/)
Open commit details
1,151 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [deps: bump Kubernetes to v1.36 and Go to 1.26](https://github.com/virtual-kubelet/virtual-kubelet/commit/c432e10aa7c2db9c81c1dbe4f0a9da37267ee19b "deps: bump Kubernetes to v1.36 and Go to 1.26")  | Sep 7, 2026  |  
| [cmd/virtual-kubelet](https://github.com/virtual-kubelet/virtual-kubelet/tree/main/cmd/virtual-kubelet "This path skips through empty directories")  | [cmd/virtual-kubelet](https://github.com/virtual-kubelet/virtual-kubelet/tree/main/cmd/virtual-kubelet "This path skips through empty directories")  | [deps: bump Kubernetes to v1.36 and Go to 1.26](https://github.com/virtual-kubelet/virtual-kubelet/commit/c432e10aa7c2db9c81c1dbe4f0a9da37267ee19b "deps: bump Kubernetes to v1.36 and Go to 1.26")  | Sep 7, 2026  |  
|  [Prometheus metrics are encoded as text, not JSON (](https://github.com/virtual-kubelet/virtual-kubelet/commit/2c155accb7d647545d4e1e91607b918c1b034041 "Prometheus metrics are encoded as text, not JSON \(#1101\)

Co-authored-by: Sanchit Mehta <sanchit.mehta602@gmail.com>")[#1101](https://github.com/virtual-kubelet/virtual-kubelet/pull/1101)[)](https://github.com/virtual-kubelet/virtual-kubelet/commit/2c155accb7d647545d4e1e91607b918c1b034041 "Prometheus metrics are encoded as text, not JSON \(#1101\)

Co-authored-by: Sanchit Mehta <sanchit.mehta602@gmail.com>")  | Apr 6, 2023  |  
|   |   |   | Jan 27, 2026  |  
|   |   |  [Update ACI liveness/readiness probe handling to work with named ports (](https://github.com/virtual-kubelet/virtual-kubelet/commit/45d2ef06b204ec6be1b0400684b07dbac4e26e38 "Update ACI liveness/readiness probe handling to work with named ports \(#333\)

* Update ACI liveness/readiness probe handling to work with named ports")[…](https://github.com/virtual-kubelet/virtual-kubelet/pull/333)  | Apr 23, 2019  |  
| [hack/skaffold/virtual-kubelet](https://github.com/virtual-kubelet/virtual-kubelet/tree/main/hack/skaffold/virtual-kubelet "This path skips through empty directories")  | [hack/skaffold/virtual-kubelet](https://github.com/virtual-kubelet/virtual-kubelet/tree/main/hack/skaffold/virtual-kubelet "This path skips through empty directories")  | [deps: bump Kubernetes to v1.36 and Go to 1.26](https://github.com/virtual-kubelet/virtual-kubelet/commit/c432e10aa7c2db9c81c1dbe4f0a9da37267ee19b "deps: bump Kubernetes to v1.36 and Go to 1.26")  | Sep 7, 2026  |  
|   |   | [deps: bump Kubernetes to v1.36 and Go to 1.26](https://github.com/virtual-kubelet/virtual-kubelet/commit/c432e10aa7c2db9c81c1dbe4f0a9da37267ee19b "deps: bump Kubernetes to v1.36 and Go to 1.26")  | Sep 7, 2026  |  
|   | Jan 27, 2026  |  
| [deps: bump Kubernetes to v1.36 and Go to 1.26](https://github.com/virtual-kubelet/virtual-kubelet/commit/c432e10aa7c2db9c81c1dbe4f0a9da37267ee19b "deps: bump Kubernetes to v1.36 and Go to 1.26")  | Sep 7, 2026  |  
|   |   |   | Nov 7, 2020  |  
| [test/e2e](https://github.com/virtual-kubelet/virtual-kubelet/tree/main/test/e2e "This path skips through empty directories")  | [test/e2e](https://github.com/virtual-kubelet/virtual-kubelet/tree/main/test/e2e "This path skips through empty directories")  | [deps: fix Prometheus bump proposed by dependabot](https://github.com/virtual-kubelet/virtual-kubelet/commit/c485b296763088267e997845b4b6242058548d2d "deps: fix Prometheus bump proposed by dependabot")  | Sep 9, 2025  |  
|   |   |   | Jan 27, 2026  |  
|   |   |   | Mar 12, 2026  |  
|   |   | [Fix linting issues and update `make lint` target.](https://github.com/virtual-kubelet/virtual-kubelet/commit/2c4442b17f612c99cc7a8fd5b7d1558f8fbff548 "Fix linting issues and update `make lint` target.

Signed-off-by: Brian Goff <cpuguy83@gmail.com>")  | Oct 7, 2022  |  
|   |   |   | Dec 5, 2017  |  
|   |   |   | Jun 11, 2025  |  
|   |   |   | May 29, 2025  |  
|   |   |   | Jan 27, 2026  |  
|   |   |   | Dec 5, 2017  |  
|   |   |   | Feb 5, 2026  |  
|   |   |   | Feb 5, 2026  |  
|   |   |   | Jul 27, 2020  |  
|   |   | [Fix filename typo s/cencus/census/](https://github.com/virtual-kubelet/virtual-kubelet/commit/ae49bbfd1146dfd98861d3417ad88cf8fe4cd94c "Fix filename typo s/cencus/census/")  | Oct 4, 2018  |  
|   |   | [deps: bump Kubernetes to v1.36 and Go to 1.26](https://github.com/virtual-kubelet/virtual-kubelet/commit/c432e10aa7c2db9c81c1dbe4f0a9da37267ee19b "deps: bump Kubernetes to v1.36 and Go to 1.26")  | Sep 7, 2026  |  
|   |   |  [providers: remove hypersh provider (](https://github.com/virtual-kubelet/virtual-kubelet/commit/a5d7400232d67643fa7e1695538ea31b21e55375 "providers: remove hypersh provider \(#524\)

Hyper.sh was shutdown on Jan 16, 2019.
https://blog.hyper.sh/close_hyper_sh.html

Signed-off-by: Paulo Pires <pjpires@gmail.com>")[#524](https://github.com/virtual-kubelet/virtual-kubelet/pull/524)[)](https://github.com/virtual-kubelet/virtual-kubelet/commit/a5d7400232d67643fa7e1695538ea31b21e55375 "providers: remove hypersh provider \(#524\)

Hyper.sh was shutdown on Jan 16, 2019.
https://blog.hyper.sh/close_hyper_sh.html

Signed-off-by: Paulo Pires <pjpires@gmail.com>")  | Feb 28, 2019  |  
|   |   |  [update license to apache 2.0 (](https://github.com/virtual-kubelet/virtual-kubelet/commit/98a111e8b7bc80c8554a3c44e20a1ebc082c51ac "update license to apache 2.0 \(#214\)")[#214](https://github.com/virtual-kubelet/virtual-kubelet/pull/214)[)](https://github.com/virtual-kubelet/virtual-kubelet/commit/98a111e8b7bc80c8554a3c44e20a1ebc082c51ac "update license to apache 2.0 \(#214\)")  | May 30, 2018  |  
|   |   | [deps: bump Kubernetes to v1.36 and Go to 1.26](https://github.com/virtual-kubelet/virtual-kubelet/commit/c432e10aa7c2db9c81c1dbe4f0a9da37267ee19b "deps: bump Kubernetes to v1.36 and Go to 1.26")  | Sep 7, 2026  |  
|   |   | [deps: bump Kubernetes to v1.36 and Go to 1.26](https://github.com/virtual-kubelet/virtual-kubelet/commit/c432e10aa7c2db9c81c1dbe4f0a9da37267ee19b "deps: bump Kubernetes to v1.36 and Go to 1.26")  | Sep 7, 2026  |  
|   |   |   | Mar 12, 2026  |  
|   |   |   | Jul 17, 2019  |  
|   |   | [build(deps): bump google.golang.org/grpc from 1.83.1 to 1.83.2](https://github.com/virtual-kubelet/virtual-kubelet/commit/24d833a96269b31793154072084d953fa7b6e3a9 "build\(deps\): bump google.golang.org/grpc from 1.83.1 to 1.83.2

Bumps \[google.golang.org/grpc\]\(https://github.com/grpc/grpc-go\) from 1.83.1 to 1.83.2.
- \[Release notes\]\(https://github.com/grpc/grpc-go/releases\)
- \[Commits\]\(https://github.com/grpc/grpc-go/compare/v1.83.1...v1.83.2\)

---
updated-dependencies:
- dependency-name: google.golang.org/grpc
  dependency-version: 1.83.2
  dependency-type: indirect
...

Signed-off-by: dependabot\[bot\] <support@github.com>")  | Sep 9, 2026  |  
|   |   | [build(deps): bump google.golang.org/grpc from 1.83.1 to 1.83.2](https://github.com/virtual-kubelet/virtual-kubelet/commit/24d833a96269b31793154072084d953fa7b6e3a9 "build\(deps\): bump google.golang.org/grpc from 1.83.1 to 1.83.2

Bumps \[google.golang.org/grpc\]\(https://github.com/grpc/grpc-go\) from 1.83.1 to 1.83.2.
- \[Release notes\]\(https://github.com/grpc/grpc-go/releases\)
- \[Commits\]\(https://github.com/grpc/grpc-go/compare/v1.83.1...v1.83.2\)

---
updated-dependencies:
- dependency-name: google.golang.org/grpc
  dependency-version: 1.83.2
  dependency-type: indirect
...

Signed-off-by: dependabot\[bot\] <support@github.com>")  | Sep 9, 2026  |  
|   |   |   | Apr 27, 2019  |  
| View all files |  
## Repository files navigation
# Virtual Kubelet
Virtual Kubelet is an open source [Kubernetes kubelet](https://kubernetes.io/docs/reference/generated/kubelet/) implementation that masquerades as a kubelet for the purposes of connecting Kubernetes to other APIs. This allows the nodes to be backed by other services like ACI, AWS Fargate, [IoT Edge](https://github.com/Azure/iot-edge-virtual-kubelet-provider), [Tensile Kube](https://github.com/virtual-kubelet/tensile-kube) etc. The primary scenario for VK is enabling the extension of the Kubernetes API into serverless container platforms like ACI and Fargate, though we are open to others. However, it should be noted that VK is explicitly not intended to be an alternative to Kubernetes federation.
Virtual Kubelet features a pluggable architecture and direct use of Kubernetes primitives, making it much easier to build on.
We invite the Kubernetes ecosystem to join us in empowering developers to build upon our base. Join our slack channel named, virtual-kubelet, within the [Kubernetes slack group](https://kubernetes.slack.com/).
The best description is "Kubernetes API on top, programmable back."
#### Table of Contents
  * [Providers](https://github.com/virtual-kubelet/virtual-kubelet#providers)
    * [Admiralty Multi-Cluster Scheduler](https://github.com/virtual-kubelet/virtual-kubelet#admiralty-multi-cluster-scheduler)
    * [Alibaba Cloud ECI Provider](https://github.com/virtual-kubelet/virtual-kubelet#alibaba-cloud-eci-provider)
    * [Azure Container Instances Provider](https://github.com/virtual-kubelet/virtual-kubelet#azure-container-instances-provider)
    * [Azure Batch GPU Provider](https://github.com/virtual-kubelet/azure-batch/blob/master/README.md)
    * [Cisco Virtual Kubelet](https://github.com/virtual-kubelet/virtual-kubelet#cisco-virtual-kubelet)
    * [AWS Fargate Provider](https://github.com/virtual-kubelet/virtual-kubelet#aws-fargate-provider)
    * [HashiCorp Nomad](https://github.com/virtual-kubelet/virtual-kubelet#hashicorp-nomad-provider)
    * [Tensile Kube Provider](https://github.com/virtual-kubelet/virtual-kubelet#tensile-kube-provider)
    * [Adding a New Provider via the Provider Interface](https://github.com/virtual-kubelet/virtual-kubelet#adding-a-new-provider-via-the-provider-interface)
  * [Testing](https://github.com/virtual-kubelet/virtual-kubelet#testing)
    * [End-to-end tests](https://github.com/virtual-kubelet/virtual-kubelet#end-to-end-tests)
  * [Known quirks and workarounds](https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds)


## How It Works
The diagram below illustrates how Virtual-Kubelet works.
## Usage
Virtual Kubelet is focused on providing a library that you can consume in your project to build a custom Kubernetes node agent.
See godoc for up to date instructions on consuming this project: <https://godoc.org/github.com/virtual-kubelet/virtual-kubelet>
There are implementations available for [several providers](https://github.com/virtual-kubelet/virtual-kubelet#providers), see those repos for details on how to deploy.
## Current Features
  * create, delete and update pods
  * container logs, exec, and metrics
  * get pod, pods and pod status
  * capacity
  * node addresses, node capacity, node daemon endpoints
  * operating system
  * bring your own virtual network


## Providers
This project features a pluggable provider interface developers can implement that defines the actions of a typical kubelet.
This enables on-demand and nearly instantaneous container compute, orchestrated by Kubernetes, without having VM infrastructure to manage and while still leveraging the portable Kubernetes API.
Each provider may have its own configuration file, and required environmental variables.
Providers must provide the following functionality to be considered a supported integration with Virtual Kubelet.
  1. Provides the back-end plumbing necessary to support the lifecycle management of pods, containers and supporting resources in the context of Kubernetes.
  2. Conforms to the current API provided by Virtual Kubelet.
  3. Does not have access to the Kubernetes API Server and has a well-defined callback mechanism for getting data like secrets or configmaps.


### Admiralty Multi-Cluster Scheduler
Admiralty Multi-Cluster Scheduler mutates annotated pods into "proxy pods" scheduled on a virtual-kubelet node and creates corresponding "delegate pods" in remote clusters (actually running the containers). A feedback loop updates the statuses and annotations of the proxy pods to reflect the statuses and annotations of the delegate pods. You can find more details in the [Admiralty Multi-Cluster Scheduler documentation](https://github.com/admiraltyio/multicluster-scheduler).
### Alibaba Cloud ECI Provider
Alibaba Cloud ECI(Elastic Container Instance) is a service that allow you run containers without having to manage servers or clusters.
You can find more details in the [Alibaba Cloud ECI provider documentation](https://github.com/virtual-kubelet/alibabacloud-eci/blob/master/README.md).
#### Configuration File
The alibaba ECI provider will read configuration file specified by the `--provider-config` flag.
The example configure file is in the [ECI provider repository](https://github.com/virtual-kubelet/alibabacloud-eci/blob/master/eci.toml).
### Azure Container Instances Provider
The Azure Container Instances Provider allows you to utilize both typical pods on VMs and Azure Container instances simultaneously in the same Kubernetes cluster.
You can find detailed instructions on how to set it up and how to test it in the [Azure Container Instances Provider documentation](https://github.com/virtual-kubelet/azure-aci/blob/master/README.md).
#### Configuration File
The Azure connector can use a configuration file specified by the `--provider-config` flag. The config file is in TOML format, and an example lives in `providers/azure/example.toml`.
### Cisco Virtual Kubelet
[Cisco Virtual Kubelet](https://github.com/cisco-open/cisco-virtual-kubelet) is a Virtual Kubelet provider that enables Kubernetes clusters to run workloads to Cisco infrastructure devices.
The use of this provider interacts with the Application Hosting functionality supported on Cisco devices, to allow for the provisioning and lifecycle management of containers. The Virtual Kubelet provider, uses RESTCONF to perform provisioning on the device. The use of the provider introduces a simplified entrypoint for application owners to deploy containers at the Edge.
For detailed instructions, follow the guide [here](https://github.com/cisco-open/cisco-virtual-kubelet/blob/main/README.md).
### AWS Fargate Provider
[AWS Fargate](https://aws.amazon.com/fargate/) is a technology that allows you to run containers without having to manage servers or clusters.
The AWS Fargate provider allows you to deploy pods to [AWS Fargate](https://aws.amazon.com/fargate/). Your pods on AWS Fargate have access to VPC networking with dedicated ENIs in your subnets, public IP addresses to connect to the internet, private IP addresses to connect to your Kubernetes cluster, security groups, IAM roles, CloudWatch Logs and many other AWS services. Pods on Fargate can co-exist with pods on regular worker nodes in the same Kubernetes cluster.
Easy instructions and a sample configuration file is available in the [AWS Fargate provider documentation](https://github.com/virtual-kubelet/aws-fargate). Please note that this provider is not currently supported.
### Elotl Kip
[Kip](https://github.com/elotl/kip) is a provider that runs pods in cloud instances, allowing a Kubernetes cluster to transparently scale workloads into a cloud. When a pod is scheduled onto the virtual node, Kip starts a right-sized cloud instance for the pod's workload and dispatches the pod onto the instance. When the pod is finished running, the cloud instance is terminated.
When workloads run on Kip, your cluster size naturally scales with the cluster workload, pods are strongly isolated from each other and the user is freed from managing worker nodes and strategically packing pods onto nodes.
### HashiCorp Nomad Provider
HashiCorp [Nomad](https://nomadproject.io) provider for Virtual Kubelet connects your Kubernetes cluster with Nomad cluster by exposing the Nomad cluster as a node in Kubernetes. By using the provider, pods that are scheduled on the virtual Nomad node registered on Kubernetes will run as jobs on Nomad clients as they would on a Kubernetes node.
For detailed instructions, follow the guide [here](https://github.com/virtual-kubelet/nomad/blob/master/README.md).
### Interlink Provider
[interLink](https://intertwin-eu.github.io/interLink/) provides an abstraction for the execution of a Kubernetes pod on any remote resource that has the capability to manage a container's execution lifecycle. The use cases that drove the initial development of the tool are the Slurm-powered HPC centers, regardless the plugin based design is enabling several additional use cases to provide Kubernetes-API based access to infrastructure that cannot host a Kubelet processes. InterLink is a Virtual Kubelet provider that can manage container lifecycle through a well defined API specification, allowing for any resource provider to be integrated with a simple http server and a handful of methods. In other words, this is an attempt to streamline the process of creating custom Virtual Kubelet providers, avoiding the need for any resource provider to implement its own version of a Kubelet workflow, which would require having some domain expertise in the Kubernetes internals.
For detailed instruction, follow the guide [here](https://intertwin-eu.github.io/interLink/docs/category/guides).
### Liqo Provider
[Liqo](https://liqo.io) implements a provider for Virtual Kubelet designed to transparently offload pods and services to "peered" Kubernetes remote cluster. Liqo is capable of discovering neighbor clusters (using DNS, mDNS) and "peer" with them, or in other words, establish a relationship to share part of the cluster resources. When a cluster has established a peering, a new instance of the Liqo Virtual Kubelet is spawned to seamlessly extend the capacity of the cluster, by providing an abstraction of the resources of the remote cluster. The provider combined with the Liqo network fabric extends the cluster networking by enabling Pod-to-Pod traffic and multi-cluster east-west services, supporting endpoints on both clusters.
For detailed instruction, follow the guide [here](https://github.com/liqotech/liqo/blob/master/README.md)
### OpenStack Zun Provider
OpenStack [Zun](https://docs.openstack.org/zun/latest/) provider for Virtual Kubelet connects your Kubernetes cluster with OpenStack in order to run Kubernetes pods on OpenStack Cloud. Your pods on OpenStack have access to OpenStack tenant networks because they have Neutron ports in your subnets. Each pod will have private IP addresses to connect to other OpenStack resources (i.e. VMs) within your tenant, optionally have floating IP addresses to connect to the internet, and bind-mount Cinder volumes into a path inside a pod's container.

```
./bin/virtual-kubelet --provider="openstack"
```

For detailed instructions, follow the guide [here](https://github.com/virtual-kubelet/openstack-zun/blob/master/README.md).
### Tensile Kube Provider
[Tensile kube](https://github.com/virtual-kubelet/tensile-kube/blob/master/README.md) is contributed by [tencent games](https://game.qq.com), which is provider for Virtual Kubelet connects your Kubernetes cluster with other Kubernetes clusters. This provider enables us extending Kubernetes to an unlimited one. By using the provider, pods that are scheduled on the virtual node registered on Kubernetes will run as jobs on other Kubernetes clusters' nodes.
### Adding a New Provider via the Provider Interface
Providers consume this project as a library which implements the core logic of a Kubernetes node agent (Kubelet), and wire up their implementation for performing the necessary actions.
There are 3 main interfaces:
#### PodLifecycleHandler
When pods are created, updated, or deleted from Kubernetes, these methods are called to handle those actions.
[godoc#PodLifecycleHandler](https://godoc.org/github.com/virtual-kubelet/virtual-kubelet/node#PodLifecycleHandler)

```
type PodLifecycleHandler interface {
    // CreatePod takes a Kubernetes Pod and deploys it within the provider.
    CreatePod(ctx context.Context, pod *corev1.Pod) error

    // UpdatePod takes a Kubernetes Pod and updates it within the provider.
    UpdatePod(ctx context.Context, pod *corev1.Pod) error

    // DeletePod takes a Kubernetes Pod and deletes it from the provider.
    DeletePod(ctx context.Context, pod *corev1.Pod) error

    // GetPod retrieves a pod by name from the provider (can be cached).
    GetPod(ctx context.Context, namespace, name string) (*corev1.Pod, error)

    // GetPodStatus retrieves the status of a pod by name from the provider.
    GetPodStatus(ctx context.Context, namespace, name string) (*corev1.PodStatus, error)

    // GetPods retrieves a list of all pods running on the provider (can be cached).
    GetPods(context.Context) ([]*corev1.Pod, error)
}
```

There is also an optional interface `PodNotifier` which enables the provider to asynchronously notify the virtual-kubelet about pod status changes. If this interface is not implemented, virtual-kubelet will periodically check the status of all pods.
It is highly recommended to implement `PodNotifier`, especially if you plan to run a large number of pods.
[godoc#PodNotifier](https://godoc.org/github.com/virtual-kubelet/virtual-kubelet/node#PodNotifier)

```
type PodNotifier interface {
    // NotifyPods instructs the notifier to call the passed in function when
    // the pod status changes.
    //
    // NotifyPods should not block callers.
    NotifyPods(context.Context, func(*corev1.Pod))
}
```

`PodLifecycleHandler` is consumed by the `PodController` which is the core logic for managing pods assigned to the node.

```
	pc, _ := node.NewPodController(podControllerConfig) // <-- instatiates the pod controller
	pc.Run(ctx) // <-- starts watching for pods to be scheduled on the node
```

#### NodeProvider
NodeProvider is responsible for notifying the virtual-kubelet about node status updates. Virtual-Kubelet will periodically check the status of the node and update Kubernetes accordingly.
[godoc#NodeProvider](https://godoc.org/github.com/virtual-kubelet/virtual-kubelet/node#NodeProvider)

```
type NodeProvider interface {
    // Ping checks if the node is still active.
    // This is intended to be lightweight as it will be called periodically as a
    // heartbeat to keep the node marked as ready in Kubernetes.
    Ping(context.Context) error

    // NotifyNodeStatus is used to asynchronously monitor the node.
    // The passed in callback should be called any time there is a change to the
    // node's status.
    // This will generally trigger a call to the Kubernetes API server to update
    // the status.
    //
    // NotifyNodeStatus should not block callers.
    NotifyNodeStatus(ctx context.Context, cb func(*corev1.Node))
}
```

Virtual Kubelet provides a `NaiveNodeProvider` that you can use if you do not plan to have custom node behavior.
[godoc#NaiveNodeProvider](https://godoc.org/github.com/virtual-kubelet/virtual-kubelet/node#NaiveNodeProvider)
`NodeProvider` gets consumed by the `NodeController`, which is core logic for managing the node object in Kubernetes.

```
	nc, _ := node.NewNodeController(nodeProvider, nodeSpec) // <-- instantiate a node controller from a node provider and a kubernetes node spec
	nc.Run(ctx) // <-- creates the node in kubernetes and starts up the controller
```

#### API endpoints
One of the roles of a Kubelet is to accept requests from the API server for things like `kubectl logs` and `kubectl exec`. Helpers for setting this up are provided [here](https://godoc.org/github.com/virtual-kubelet/virtual-kubelet/node/api)
#### Scrape Pod metrics
If you want to use HPA(Horizontal Pod Autoscaler) in your cluster, the provider should implement the `GetStatsSummary` function. Then metrics-server will be able to get the metrics of the pods on virtual-kubelet. Otherwise, you may see `No metrics for pod ` on metrics-server, which means the metrics of the pods on virtual-kubelet are not collected.
## Testing
### Unit tests
Running the unit tests locally is as simple as `make test`.
### End-to-end tests
Check out [`test/e2e`](https://github.com/virtual-kubelet/virtual-kubelet/blob/main/test/e2e) for more details.
## Known quirks and workarounds
### Missing Load Balancer IP addresses for services
#### Providers that do not support service discovery
Kubernetes 1.9 introduces a new flag, `ServiceNodeExclusion`, for the control plane's Controller Manager. Enabling this flag in the Controller Manager's manifest allows Kubernetes to exclude Virtual Kubelet nodes from being added to Load Balancer pools, allowing you to create public facing services with external IPs without issue.
#### Workaround
Cluster requirements: Kubernetes 1.9 or above
Enable the ServiceNodeExclusion flag, by modifying the Controller Manager manifest and adding `--feature-gates=ServiceNodeExclusion=true` to the command line arguments.
## Contributing
Virtual Kubelet follows the [CNCF Code of Conduct](https://github.com/cncf/foundation/blob/master/code-of-conduct.md). Sign the [CNCF CLA](https://github.com/kubernetes/community/blob/master/CLA.md) to be able to make Pull Requests to this repo.
Monthly Virtual Kubelet Office Hours are held at 10am PST on the second Thursday of every month in this [zoom meeting room](https://zoom.us/j/94701509915). Check out the calendar [here](https://calendar.google.com/calendar/embed?src=b119ced62134053de07d6c261b50d21ebde0da54f4163f5771b60ecf906e8b90%40group.calendar.google.com&ctz=America%2FLos_Angeles).
Our google drive with design specifications and meeting notes are [here](https://drive.google.com/drive/folders/19Ndu11WBCCBDowo9CrrGUHoIfd2L8Ueg?usp=sharing).
We also have a community slack channel named virtual-kubelet in the Kubernetes slack. You can also connect with the Virtual Kubelet community via the [mailing list](https://lists.cncf.io/g/virtualkubelet-dev).
## About
Virtual Kubelet is an open source Kubernetes kubelet implementation.
### Topics
### Resources
### Contributing
### Stars
**4.6k** stars
### Watchers
**96** watching
### Forks
[ forks](https://github.com/virtual-kubelet/virtual-kubelet/forks)
## 
[ v1.14.0Latest Sep 7, 2026 ](https://github.com/virtual-kubelet/virtual-kubelet/releases/tag/v1.14.0)
## 
[+ 164](https://github.com/virtual-kubelet/virtual-kubelet/network/dependents)
## 
## Languages
  * [Go93.9%](https://github.com/virtual-kubelet/virtual-kubelet/search?l=go)
  * [JavaScript2.4%](https://github.com/virtual-kubelet/virtual-kubelet/search?l=javascript)
  * [HTML1.9%](https://github.com/virtual-kubelet/virtual-kubelet/search?l=html)
  * [Makefile0.8%](https://github.com/virtual-kubelet/virtual-kubelet/search?l=makefile)
  * [Sass0.6%](https://github.com/virtual-kubelet/virtual-kubelet/search?l=sass)
  * [Dockerfile0.3%](https://github.com/virtual-kubelet/virtual-kubelet/search?l=dockerfile)
  * [Other0.1%](https://github.com/virtual-kubelet/virtual-kubelet/search?l=Other)


You can’t perform that action at this time. 
