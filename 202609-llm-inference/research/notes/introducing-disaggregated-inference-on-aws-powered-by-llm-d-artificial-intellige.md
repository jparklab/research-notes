---
title: Introducing Disaggregated Inference on AWS powered by llm-d | Artificial Intelligence
id: introducing-disaggregated-inference-on-aws-powered-by-llm-d-artificial-intellige
tags:
- llmd-kv-cache-routing-c665d5
- pd-disaggregation
- nixl
- aws-infrastructure
created: '2026-09-14T11:53:57.981055Z'
updated: '2026-09-15T04:55:24.060498Z'
source: https://aws.amazon.com/blogs/machine-learning/introducing-disaggregated-inference-on-aws-powered-by-llm-d/
source_domain: aws.amazon.com
fetched_at: '2026-09-14T11:53:57.955972Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'AWS ML blog (joint effort with Red Hat''s llm-d team, credits Greg Pereira
  and Robert Shaw) announcing ghcr.io/llm-d/llm-d-aws container image with AWS-specific
  libraries (EFA, libfabric) for llm-d on SageMaker HyperPod EKS / Amazon EKS. Explains
  llm-d''s four well-lit paths: (1) intelligent inference scheduling -- the scheduler
  ''maintain[s] visibility into the cache state across the serving replicas and rout[es]
  requests accordingly'' without requiring direct introspection of KV cache state,
  guessing locality instead; (2) prefill/decode disaggregation, where a sidecar next
  to decode instances instructs vLLM to perform point-to-point KV cache transfers
  over fast interconnects from prefill servers; (3) wide expert parallelism for MoE
  models (DeepSeek-R1, Qwen3.5, Minimax, Kimi K2.5); (4) tiered prefix caching, offloading
  KV cache entries from GPU memory to CPU memory or local disk to expand effective
  cache size beyond GPU memory limits. Architecture detail: NVIDIA Inference Xfer
  Library (NIXL) provides the point-to-point KV-cache transfer abstraction (vs. NCCL''s
  collective-only patterns), using RDMA over AWS Elastic Fabric Adapter (EFA) via
  a libfabric plugin/transport layer for GPU-Direct, kernel-bypass, zero-copy transfers
  between prefill and decode nodes. Deployment uses llm-d''s Gateway API Inference
  Extension (Istio or Kgateway) plus helmfile-based well-lit-path guides (guides/inference-scheduling,
  guides/pd-disaggregation) with vLLM configured via --kv-transfer-config ''{kv_connector:NixlConnector,
  kv_role:kv_both, kv_connector_extra_config:{backends:[LIBFABRIC]}}''. Benchmark:
  GPT-OSS on vLLM tensor-parallel-4 on ml.p6-b200.48xlarge vs. llm-d P/D disaggregation
  (4 prefill pods TP=1, 1 decode pod TP=4) connected via NIXL/Libfabric/EFA -- disaggregation
  increased tokens/sec by up to 70% as concurrency rises, at 1024-in/1024-out tokens
  up to concurrency 128. Recommends EFA allocation of 4 interfaces per GPU on p5.48xlarge
  (8 H100s, 32 EFA interfaces).'
utility_score: 12.0
---

## Select your cookie preferences
We use essential cookies and similar tools that are necessary to provide our site and services. We use performance cookies to collect anonymous statistics, so we can understand how customers use our site and make improvements. Essential cookies cannot be deactivated, but you can choose “Customize” or “Decline” to decline performance cookies. If you agree, AWS and approved third parties will also use cookies to provide useful site features, remember your preferences, and display relevant content, including relevant advertising. To accept or decline all non-essential cookies, choose “Accept” or “Decline.” To make more detailed choices, choose “Customize.”
AcceptDeclineCustomize
## Customize cookie preferences
We use cookies and similar tools (collectively, "cookies") for the following purposes.
### Essential
Essential cookies are necessary to provide our site and services and cannot be deactivated. They are usually set in response to your actions on the site, such as setting your privacy preferences, signing in, or filling in forms. 
### Performance
Performance cookies provide anonymous statistics about how customers navigate our site so we can improve site experience and performance. Approved third parties may perform analytics on our behalf, but they cannot use the data for their own purposes.
Allowed
### Functional
Functional cookies help us provide useful site features, remember your preferences, and display relevant content. Approved third parties may set these cookies to provide certain site features. If you do not allow these cookies, then some or all of these services may not function properly.
Allowed
### Advertising
Advertising cookies may be set through our site by us or our advertising partners and help us deliver relevant marketing content. If you do not allow these cookies, you will experience less relevant advertising.
Allowed
Blocking some types of cookies may impact your experience of our sites. You may review and change your choices at any time by selecting Cookie preferences in the footer of this site. We and selected third-parties use cookies or similar technologies as specified in the [AWS Cookie Notice](https://aws.amazon.com/legal/cookies/).
CancelSave preferences
## Your privacy choices
We and our advertising partners (“we”) may use information we collect from or about you to show you ads on other websites and online services. Under certain laws, this activity is referred to as “cross-context behavioral advertising” or “targeted advertising.”
To opt out of our use of cookies or similar technologies to engage in these activities, select “Opt out of cross-context behavioral ads” and “Save preferences” below. If you clear your browser cookies or visit this site from a different device or browser, you will need to make your selection again. For more information about cookies and how we use them, read our [Cookie Notice](https://aws.amazon.com/legal/cookies/).
Allow cross-context behavioral adsOpt out of cross-context behavioral ads
To opt out of the use of other identifiers, such as contact information, for these activities, fill out the form [here](https://pulse.aws/application/ZRPLWLL6?p=0).
For more information about how AWS handles your information, read the [AWS Privacy Notice](https://aws.amazon.com/privacy/).
CancelSave preferences
## Unable to save cookie preferences
We will only store essential cookies at this time, because we were unable to save your cookie preferences.If you want to change your cookie preferences, try again later using the link in the AWS console footer, or contact support if the problem persists.
Dismiss
[ Skip to Main Content](https://aws.amazon.com/blogs/machine-learning/introducing-disaggregated-inference-on-aws-powered-by-llm-d/#aws-page-content-main)
## [Artificial Intelligence](https://aws.amazon.com/blogs/machine-learning/)
# Introducing Disaggregated Inference on AWS powered by llm-d
_We thank Greg Pereira and Robert Shaw from Red Hat for their support in bringing llm-d to AWS._
In the agentic and reasoning era, large language models (LLMs) generate 10x more tokens and compute through complex reasoning chains compared to single-shot replies. Agentic AI workflows also create highly variable demands and another exponential increase in processing, bogging down the inference process and degrading the user experience. As the world transitions from prototyping AI solutions to deploying AI at scale, efficient inference is becoming the gating factor.
LLM inference consists of two distinct phases: **prefill** and **decode**. The **prefill** phase is compute bound. It processes the entire input prompt in parallel to generate the initial set of key-value (KV) cache entries. The **decode** phase is memory bound. It autoregressively generates one token at a time while requiring substantial memory bandwidth to access model weights and the ever-growing KV cache. Adding to this complexity, inference requests vary widely in computational requirements based on input and output length, making efficient resource utilization particularly challenging.
Traditional approaches often involve deploying models on predetermined infrastructure and topology or using basic distributed strategies that don’t account for these unique phases of LLM inference. This leads to suboptimal resource utilization, with GPUs either underutilized or overloaded during different inference phases. While [vLLM](https://github.com/vllm-project/vllm) has emerged as a popular open source inference engine that improves efficiency through nearly continuous batching and PagedAttention, organizations deploying at scale still face challenges in orchestrating deployments and optimizing routing decisions across multiple nodes.
We are announcing a joint effort with the [llm-d](https://llm-d.ai/docs/architecture) team to bring powerful disaggregated inference capabilities to AWS so that customers can boost performance, maximize GPU utilization, and improve costs for serving large-scale inference workloads. This launch is the result of several months of close collaboration with the llm-d community to deliver a new container `ghcr.io/llm-d/llm-d-aws` that includes libraries that are specific to AWS, such as Elastic Fabric Adapter (EFA) and libfabric, along with integration of llm-d with the NIXL library to support critical features such as multi-node disaggregated inference and expert parallelism. We have also conducted extensive benchmarking through multiple iterations to arrive at a stable release that allows customers to access these powerful capabilities out of the box on AWS Kubernetes systems such as Amazon SageMaker HyperPod and Amazon Elastic Kubernetes Service (Amazon EKS).
Throughout this blog post, we introduce the concepts behind next-generation inference capabilities, including disaggregated serving, intelligent request scheduling, and expert parallelism. We discuss their benefits and walk through how you can implement them on Amazon SageMaker HyperPod EKS to achieve significant improvements in inference performance, resource utilization, and operational efficiency.
## What is llm-d?
[**llm-d** ](https://llm-d.ai/docs/architecture)is an open source, Kubernetes-native framework for distributed large language model (LLM) serving. Built on top of vLLM, llm-d extends the core inference engine with production-grade orchestration, advanced scheduling, and high-performance interconnect support to enable scalable, multi-node model serving.
Rather than treating inference as a single-node execution problem, llm-d introduces architectural patterns for disaggregated serving—separating and improving stages such as prefill, decode, and KV-cache management across distributed GPU resources. This allows operators to efficiently use high-speed fabrics such as AWS Elastic Fabric Adapter (EFA), while maintaining compatibility with Kubernetes-native deployment workflows.
To make these capabilities accessible, llm-d provides a set of well-lit paths—reference serving architectures that package proven optimization strategies for different performance, scalability, and workload goals:
## Intelligent inference scheduling
While the intelligent scheduling example makes routing decisions based on other factors, such as queue depth, its unique approach to routing is that it attempts to guess the locality of requests in the KVcache, without requiring it to have visibility into the state of the KVCache. In a single-instance environment, engines like vLLM use Automatic Prefix Caching to reduce redundant computation by reusing prior KV cache entries, driving faster and more efficient performance. However, the moment you scale to a distributed, multi-replica environment, assumptions about which kvblocks exist on which GPUs can’t hold. Without awareness of the locality of requests in their intermediary states, requests might be routed to instances that lack relevant cached context, negating the benefits of prefix caching entirely.
The llm-d scheduler addresses this by maintaining visibility into the cache state across the serving replicas and routing requests accordingly. For workloads with high prefix reuse, such as multi-turn conversations or agentic workflows, this cache-aware routing can lead to significant improvements in throughput and latency by making sure that requests are directed to servers that already hold relevant KV cache entries.
## Prefill and Decode disaggregation
As described earlier, the prefill and decode phases of LLM inference have fundamentally different resource profiles, with prefill being compute-intensive and decode being memory-bandwidth-intensive. In a traditional deployment, both phases share the same hardware, meaning neither can be independently optimized. Separating these two phases unlocks several optimization opportunities. For example, if your output context length is higher than your input length, you can assign more GPUs to decode than prefill. You can also place these two phases on different types of hardware, each tuned for its respective workload characteristics.
In llm-d, prefill servers are optimized for processing input prompts efficiently, while decode servers are focused on generating output tokens with low latency. The intelligent scheduler decides which instances should receive a given request, and the transfer is coordinated using a sidecar running alongside decode instances. The sidecar instructs vLLM to perform point-to-point KV cache transfers over fast interconnects to make sure that the decode server receives the necessary cached context from the prefill server with minimal overhead. This disaggregation significantly improves both time to first token (TTFT) and overall throughput, particularly for workloads with long prompts or when processing large models.
## Wide expert parallelism
For Mixture-of-Experts (MoE) models such as DeepSeek-R1, Qwen3.5, Minimax, and Kimi K2.5, llm-d provides optimized deployment patterns that use data parallelism and expert parallelism. This approach enables efficient deployment of large MoE models by distributing experts horizontally across multiple nodes while maintaining performance. By spreading model experts across accelerators and using improved communication patterns, llm-d can significantly reduce end-to-end latency and increase throughput for these complex architectures. However, scaling MoE models introduces more complex parallelism, communication, and scheduling requirements that must be carefully tuned for each deployment scenario.
## Tiered prefix caching
Prefix caching avoids performing repetitive and expensive KV cache computations, improving metrics such as TTFT and overall throughput. While inference engines like vLLM have native prefix caching built in, they’re constrained by the amount of GPU memory available on a given instance. To expand the effective size of the KV cache beyond GPU memory limits, llm-d offers a tiered caching path that offloads KV cache entries from GPU memory to other storage tiers such as CPU memory or local disk.
These well-lit paths are offered as starting points for configuration and deployment of model servers. They’re designed as composable building blocks for vLLM deployments and inference scheduler configuration, meaning features across multiple paths can be combined and configured together to suit specific workload requirements.
## Running llm-d on AWS
### Amazon SageMaker HyperPod EKS
Amazon SageMaker HyperPod offers resilient, high-performance Kubernetes infrastructure optimized for large-scale model training and inference. It provides persistent, high-performance clusters that address many of the infrastructure challenges organizations face when deploying large models. Health monitoring is built into the system, with proactive detection and remediation of hardware failures to maintain high availability for production workloads. Native Kubernetes support simplifies container orchestration, making it an ideal foundation for llm-d’s Kubernetes-native architecture.
### Reference Architecture
To understand how llm-d operates efficiently on AWS infrastructure, it is important to understand the communication layers that enable high-performance distributed inference. For GPU to GPU communication on a single node, NVLink and NVSwitch are used for high-bandwidth transfers between prefill and decode workers. The following sections describe the key components and how they work together.
**NIXL for Point-to-Point Inference Transfers**
NCCL, which is widely used in LLM training excels at collective communication patterns, disaggregated inference architectures require efficient point-to-point data transfers, for example, moving KV cache data from a prefill node to a decode node. NVIDIA Inference Xfer Library (NIXL) is purpose-built for this scenario. NIXL provides a memory abstraction layer that spans CPU memory, GPU memory, and storage backends including file, block, and object stores such as Amazon S3. It functions as an abstraction layer over different transfer methods, including libfabric for EFA interfaces, UCCL, and GPUDirect Storage.
Through NIXL, instances transfer KV cache data between prefill and decode servers using RDMA. RDMA allows GPUs to bypass the operating system and read peer device memory directly, which is critical for inference workloads where TTFT is a key performance metric. In the llm-d architecture, vLLM servers are deployed in InferencePools for routing, and prefill/decode disaggregation is configured using NIXL as the connector for KV cache sharing. NIXL leverages the EFA interfaces connected to instances for high-bandwidth communication, making sure that the overhead of transferring cached context between disaggregated phases remains minimal.
**NIXL Libfabric plugin and the Transport Layer**
NIXL libfabric plugin provides a high-performance RDMA backend for NIXL using the OpenFabrics Interfaces (OFI) Libfabric library. Libfabric supports RDMA operations that enable zero-copy, GPU-Direct, kernel-bypass networking, which is critical for minimizing latency and maximizing bandwidth in distributed workloads. Importantly, NIXL has native support for AWS Elastic Fabric Adapter (EFA) through the libfabric plugin, providing the high- performance plumbing that NIXL relies on when GPUs need to communicate across nodes.
**Elastic Fabric Adapter (EFA)**
EFA provides high performance networking interface on AWS, which is essential for scaling distributed inference across multiple nodes. EFA uses libfabric as its userspace interface, and NIXL Libfabric plugin includes a libfabric transport layer that can leverage EFA directly. This integration means that when llm-d deploys vLLM across multiple nodes, the underlying communication stack can take full advantage of EFA’s low-latency, high-bandwidth networking without requiring changes at the application level.
We can configure the AWS Load Balancer Controller to provision load balancers for connecting to the Inference Gateway. The Inference Gateway (IGW) sits in front of vLLM instances, providing intelligent request scheduling and routing based on various factors including cache locality and server load. The KV Cache Manager enables cache-aware routing and distributed cache management, tracking which KV cache blocks reside on which nodes. These components work together to create a flexible, extensible system for LLM inference that addresses the unique challenges of serving large models at scale.
With [SageMaker HyperPod’s observability dashboards](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-hyperpod-eks-cluster-observability.html), you can monitor key metrics during inference time such as GPU utilization, EFA metrics and error counts for proactively monitoring and optimizing your inference workloads.
## Best Practices
Disaggregated inference allows you to scale your prefill nodes separately to your decode nodes, allowing you to tune your performance for your workloads. For example, larger input sequence lengths with short output sequence lengths are a prefill-heavy workload. Disaggregated inference allows you to scale your prefill pods to handle more requests efficiently without an increase in cost. It is not for all workloads however. You can try it with larger models, longer input sequences, and sparse MoE architectures.
llm-d also provides paths for intelligently routing traffic to specific pods based on metrics such as request queues and KV cache events via the inference gateway. This works to improve performance and KV cache hits for LLM inference workloads for improving throughput. The project is still developing and adding more paths and improvements for hosting LLM workloads.
## Deployment Overview
### **Prerequisites**
Before we proceed with deploying either pattern, you need the following components set up locally on your device:
  * [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) (AWS CLI)
  * [HuggingFace](https://huggingface.co/) account for creating an access token
  * Access to a SageMaker HyperPod or EKS cluster


### llm-d Setup
llm-d uses the [Gateway Inference API Extension](https://gateway-api-inference-extension.sigs.k8s.io/), which requires the installation of the CRDs and an implementation such as Istio. Clone the `llm-d` repository and navigate to the installation helper:

```
git clone https://github.com/llm-d/llm-d.git
cd guides/prereq/gateway-provider
```

Code
Install the provider and implementation

```
./install-gateway-provider-dependencies.sh
helmfile apply -f istio.helmfile.yaml #or kgateway if using Kgateway
```

Code
Once they are installed, you can start deploying the guides.
### **Model Deployment**
The llm-d repository provides a number of well-lit paths for inference on Kubernetes located on their GitHub. Each guide is configured using a `helmfile` and is split under two folders. One for the Gateway AI Extension, which configures the Kubernetes Gateway and one for the model service, which configures the model hosting configuration.
Docker image with AWS libraries :`ghcr.io/llm-d/llm-d-aws:v0.5.1`
To expose a Gateway with an AWS Load Balancer, you can configure the required Type and Annotations under `./guides/prereq/gateway-provider/common-configurations`.
For example, we configured `./guides/prereq/gateway-provider/common-configurations/istio.yaml` as

```
# Infra values
gateway:
  gatewayClassName: istio
  gatewayParameters:
    accessLogging: false
    logLevel: error
    resources:
      limits:
        cpu: "16"
        memory: 16Gi
      requests:
        cpu: "4"
        memory: 4Gi
  service:
    type: LoadBalancer
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: ip
    service.beta.kubernetes.io/aws-load-balancer-scheme: internal
    service.beta.kubernetes.io/aws-load-balancer-type: external

# GAIE values
inferenceExtension:
  flags:
    v: 1
provider:
  name: istio
  istio:
    destinationRule:
      trafficPolicy:
        connectionPool:
          http:
            http1MaxPendingRequests: 256000
            maxRequestsPerConnection: 256000
            http2MaxRequests: 256000
            idleTimeout: "900s"
          tcp:
            maxConnections: 256000
            maxConnectionDuration: "1800s"
            connectTimeout: "900s"

# MS values
routing:
  proxy:
    zapLogLevel: error
```

Code
When the Istio Gateway is created, it will provision a network load balancer in your VPC for use. From here, you can configure the example as per the instructions in the README file to deploy the stack. To get started running the `inference-scheduling` example, from the `llm-d` directory run:

```
cd guides/inference-scheduling
```

Code
Here you will see the structure appearing like:

```
❯ tree
.
├── gaie-inference-scheduling
│   └── values.yaml
├── helmfile.yaml.gotmpl
├── httproute.gke.yaml
├── httproute.yaml
├── ms-inference-scheduling
│   ├── digitalocean-values.yaml
│   ├── values_amd.yaml
│   ├── values_cpu.yaml
│   ├── values_tpu.yaml
│   ├── values_xpu.yaml
│   ├── values-hpu.yaml
│   └── values.yaml
└── README.md
```

Code
The `ms-inference-scheduling` folder contains the configuration values for running vLLM replicas on your nodes. `gaie-inference-scheduling` will configure the inference gateway using your selected provider from previously.
Once you are ready to deploy, run `helmfile apply` to deploy the guide on your cluster.
### **Deploying with Prefill-Decode Disaggregation**
The guide to deploy with prefill/decode disaggregation is located in `guides/pd-disaggregation`. For running within an environment such as a SageMaker HyperPod cluster, you must configure the replicas to run using an EFA-enabled image and make sure to allocate EFA interfaces to the pods.
Within the `ms-pd/values.yaml,` you configure it similar to:

```
containers:
    - name: "vllm"
      image: ghcr.io/llm-d/llm-d-aws
      modelCommand: vllmServe
      args:
        - "--block-size"
        - "128"
        - "--kv-transfer-config"
        - '{"kv_connector":"NixlConnector", "kv_role":"kv_both","kv_connector_extra_config": {"backends": ["LIBFABRIC"]}}'
        - "--disable-uvicorn-access-log"
        - "--max-model-len"
        - "32000"
      env:
        - name: VLLM_NIXL_SIDE_CHANNEL_HOST
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
        - name: HF_HOME
          value: "/model-cache"
      ports:
        - containerPort: 8000
          name: vllm
          protocol: TCP
        - containerPort: 5600
          name: nixl
          protocol: TCP
      resources:
        limits:
          memory: 64Gi
          cpu: "8"
          # note: GPU resources get controlled by parallelism + accelerators above
          vpc.amazonaws.com/efa:4
        requests:
          memory: 64Gi
          cpu: "8"
          # note: GPU resources get controlled by parallelism + accelerators above
          vpc.amazonaws.com/efa: 4
```

Code
The image need to use llm-d’s AWS-compatible container. vLLM is configured where NIXL will use the libfabric backend to maximise network bandwidth. For configuring the number of EFA interfaces, you should allocate based on the number of GPUs each Pod is running with and the number of EFA interfaces available on the instance. For example, a p5.48xlarge instance has 8 H100 GPUs with 32 Elastic Fabric Adapter interfaces, so you should configure each replica to have 4 EFA interfaces per GPU.
Optionally, you can also configure `"enable_cross_layers_blocks": "True"` for the `kv_connector_extra_config` for reducing the amount of data that vLLM will transfer.
### **Running Inference**
Once deployed, EKS will have created an AWS Network Load Balancer for deployment. To get the Load Balancer DNS name, run `kubectl get gateways`. You can then invoke this with curl:

```
export OPENAI_API_BASE=generated ELB endpoint

curl $OPENAI_API_BASE/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "RedHatAI/Llama-3.3-70B-Instruct-FP8-dynamic",
    "messages": [
      {
        "role": "user",
        "content": "Hello! Who are you?"
      }
    ],
    "max_tokens": 256
  }' | jq
```

Bash
## Disaggregated Inference
### Benchmarking
We deployed OpenAI’s GPT-OSS on vLLM with a tensor parallel degree of 4 on an ml.p6-b200.48xlarge. We compared it against llm-d’s path for prefill/decode disaggregation with 4 prefill pods each with a tensor parallel degree of 1 and 1 decode pods with a tensor parallel degree of 4. The pods were connected using NIXL with Libfabric as the underlying transport backend for using Elastic Fabric Adapter networking on the instances.
In our testing, we found that using llm-d’s prefill/decode disaggregation path increases tokens per second by up to 70% as concurrency increases compared to using a standard vLLM deployment when load testing with an input sequence of 1024 input tokens and receiving 1024 output tokens up to a concurrency of 128. This performance profile varies based on your vLLM configuration and workload. Tuning your prefill/decode ratio and other parameters available from vLLM server can potentially bring more performance.
## **Conclusion**
llm-d provides paths for deployment methods such as prefill/decode disaggregation, precise KV aware routing and tiered KVcaching. These provide further methods to improve performance for hosting at scale. You can tune the vLLM settings as required to improve metrics such as TTFT, ITL, or cache hits. You can also use frameworks such as LMCache for KV offloading. Checkout llm-d at <https://llm-d.ai/docs/architecture>
## About the authors
### Resources


### Blog Topics


### Follow


