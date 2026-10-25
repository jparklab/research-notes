---
title: Deploy an LLM inference service on OpenShift AI | Red Hat Developer
id: deploy-an-llm-inference-service-on-openshift-ai-red-hat-developer
tags:
- onprem-llm-inference-stacks-859099
- vendor-onprem
- openshift-ai
created: '2026-10-23T17:22:05.137643Z'
updated: '2026-10-23T17:24:35.152611Z'
source: https://developers.redhat.com/articles/2025/11/03/deploy-llm-inference-service-openshift-ai
source_domain: developers.redhat.com
fetched_at: '2026-10-23T17:22:05.109549Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'PROVENANCE: Red Hat developer tutorial (3 Nov 2025), vendor-authored but
  an actual internal use case: on-premise inference for Ansible Lightspeed intelligent
  assistant (RAG). Stack: OpenShift AI KServe inference service, vLLM serving runtime,
  model weights in S3-compatible store (MinIO on-cluster; also PVC/OCI), NVIDIA GPU
  Operator (AMD alternative). SLOs set: p99 TTFT < 1,500 ms, p99 inter-token latency
  < 200 ms. Shows OpenShift AI replaces the orchestration/serving-lifecycle layer
  around open-source vLLM.'
utility_score: 13.0
---

[ Skip to main content ](https://developers.redhat.com/articles/2025/11/03/deploy-llm-inference-service-openshift-ai#main-content)
All Red Hat 
Back to menu
## Quick links: redhat.com, Customer Portal, Red Hat's developer site, Red Hat's partner site.
  * [ You are here Red Hat Learn about our open source products, services, and company. ](https://www.redhat.com/en)
  * [ You are here Red Hat Customer Portal Get product support and knowledge from the open source experts. ](https://access.redhat.com)
  * [ You are here Red Hat Developer Read developer tutorials and download Red Hat software for cloud application development. ](https://developers.redhat.com)
  * [ You are here Red Hat Partner Connect Get training, subscriptions, certifications, and more for partners to build, sell, and support customer solutions.  ](https://connect.redhat.com)


## Products & tools
  * ### [Red Hat Documentation ](https://docs.redhat.com) Find answers, get guidance, and learn how to use Red Hat products. 
  * ### [Red Hat Ecosystem Catalog ](https://catalog.redhat.com) Find hardware, software, and cloud providers―and download container images―certified to perform with Red Hat technologies. 


## Try, buy, & sell
  * ### [Red Hat Hybrid Cloud Console ](https://www.redhat.com/hybrid-cloud-console) Access technical how-tos, tutorials, and learning paths focused on Red Hat’s hybrid cloud managed services. 
  * ### [Red Hat Store ](https://www.redhat.com/store) Buy select Red Hat products and services online. 


## Events
  * ### [Red Hat Summit ](https://www.redhat.com/summit) Register for and learn about our annual open source IT industry event. 


Ask AI
#  Deploy an LLM inference service on OpenShift AI
### Powering the Ansible Lightspeed intelligent assistant with Red Hat OpenShift AI, part 2
November 3, 2025 

Related topics:
     [Artificial intelligence](https://developers.redhat.com/topics/ai-ml)[Automation](https://developers.redhat.com/topics/automation/all) 

Related products:
     [Red Hat AI](https://developers.redhat.com/taxonomy/term/37288)[Red Hat OpenShift AI](https://developers.redhat.com/products/red-hat-openshift-ai/overview)[Red Hat Ansible Automation Platform](https://developers.redhat.com/products/ansible/overview)[Red Hat Ansible Lightspeed with IBM watsonx Code Assistant](https://developers.redhat.com/products/ansible/lightspeed)[Red Hat OpenShift](https://developers.redhat.com/products/openshift/overview)
Table of contents: 
  * [Powering the Ansible Lightspeed intelligent assistant with your own inference service](https://developers.redhat.com/articles/2025/11/03/deploy-llm-inference-service-openshift-ai#powering_the_ansible_lightspeed_intelligent_assistant_with_your_own_inference_service)
  * [Evaluating our inference server's performance](https://developers.redhat.com/articles/2025/11/03/deploy-llm-inference-service-openshift-ai#evaluating_our_inference_server_s_performance)


Deploying large language models (LLMs) on [Red Hat OpenShift AI](https://developers.redhat.com/products/red-hat-openshift-ai/overview) enables on-premise inference for [Red Hat Ansible Lightspeed intelligent assistant](https://www.redhat.com/en/technologies/management/ansible/ansible-lightspeed). With OpenShift AI, you can containerize, scale, and integrate LLM workloads directly into enterprise environments. This ensures better control over data, compliance with organizational policies, and the ability to optimize resource utilization. The deployment also creates a flexible platform for evaluating model performance, customizing workflows, and extending Ansible Lightspeed with domain-specific intelligence.
**Note:** [Installing OpenShift AI](https://developers.redhat.com/articles/2025/09/16/how-enable-ansible-lightspeed-intelligent-assistant) is a prerequisite for this model deployment.
Powering the Ansible Lightspeed intelligent assistant with your own inference service
## Powering the Ansible Lightspeed intelligent assistant with your own inference service
Ansible Lightspeed intelligent assistant lets you bring your own AI service to power the inference that helps generate answers. These answers are based on the enhanced context from the retrieval-augmented generation (RAG) requests that the intelligent assistant uses.
The core of these AI services is typically an LLM. An LLM is a type of AI model that uses natural language processing (NLP) to understand and generate human-like text. Trained on massive amounts of text data using transformer architectures, these models learn the deep patterns and nuances of language.
OpenShift AI provides the necessary interface for this is through an _inference service_. An inference service is a standardized API endpoint that provides access to a deployed LLM, allowing applications like the intelligent assistant to simply send its RAG-enhanced prompt and receive the generated response.
### Key components of an inference server deployment
Deploying an inference service involves several critical components:
  * **Model storage:** KServe supports Amazon S3, PVC, or OCI-based model storage. We have chosen an S3-compatible location where the large LLM files are stored. The service pulls the model from here on startup.
  * **A serving runtime:** A specialized container image that contains the necessary libraries and server software (like vLLM) to load the model from storage and serve requests.
  * **Accelerated compute infrastructure:** An underlying worker node equipped with a compatible GPU. We have chosen instances with NVIDIA GPUs, so we need to install the NVIDIA GPU Operator in OpenShift to make this hardware's processing power available to our serving runtime. This is essential for performant inference. Alternatively, OpenShift AI supports AMD GPUs, but we will not cover this in our example.


### Evaluating inference server performance
How do we know if our inference service is good enough for a real-time, interactive tool like the Ansible Lightspeed intelligent assistant? A slow, lagging assistant provides a poor user experience. To quantify performance, we focus on two key performance indicators (KPIs):
  * **Time to First Token (TTFT):** This measures the latency from when a user sends a prompt to when the very first piece (token) of the response is generated. A low TTFT is crucial for making the assistant feel responsive and not "stuck."
  * **Inter-Token Latency (ITL):** This measures the average time delay between each subsequent token in the response. A low ITL ensures the response is streamed smoothly and quickly, rather than appearing in slow, jerky bursts.


To ensure a quality user experience, we must define **service-level objectives (SLOs)** for these KPIs. For the purpose of this guide, we'll establish the following SLOs:
  * **99th percentile TTFT:** Fewer than 1,500 milliseconds.
  * **99th percentile ITL:** Fewer than 200 milliseconds.


### Deployment overview
This blog post guides you through the following high-level steps:
  1. **Configure storage:** We will deploy MinIO to create an on-cluster, S3-compatible bucket for our model files.
  2. **Prepare the serving runtime:** We will build a custom vLLM container image compatible with our GPU infrastructure.
  3. **Deploy the model:** Using the OpenShift AI interface, we will create the InferenceService, connecting our storage and runtime to launch a live API endpoint.
  4. **Benchmark and validate:** We will use the guidellm tool to test our service against the SLOs we defined, ensuring it's ready for integration.


Successfully completing these steps will set the stage perfectly for the next blog post in this series, where we will connect this proven, performant inference service to the Ansible Lightspeed intelligent assistant and measure the end-to-end user experience.
Before you begin
We recommend having an OpenShift user with cluster-admin privileges. Refer to this [article](https://ai-on-openshift.io/odh-rhoai/openshift-group-management/) for more details.
Serving a model
## Serving a model
This section walks through storage setup, serving runtime preparation, and deployment.
### Set up storage for the model with MinIO
Setting up the storage for handling the LLM model files is required, as OpenShift doesn't support large-file storage. One option is to connect an external S3-compatible service such as MinIO or a public cloud S3 provider. I went with MinIO because it offers a practical way to set up, manage, and upload the model within the cluster.
  1. Deploy the MinIO instance in the given namespace using the following file: [minio-setup.yaml](https://github.com/IsaiahStapleton/rhoai-model-deployment-guide/blob/main/minio-setup.yaml)
Either clone the repo or copy the file to your local system.
  2. Check the current project in OpenShift using the following CLI command:

```
oc project
```

  3. If needed, switch to the desired namespace using the following command:

```
oc project <namespace-name>
```

  4. Run the following command to deploy a pod with the given configurations in the file:

```
oc apply -f minio-setup.yaml
```

  5. Note the `minio_root_user` and `minio_root_password` and store them securely. The username and password are required to access the MinIO user dashboard.
  6. After setting up MinIO, you can discover routes to access it in the OpenShift dashboard.
    1. From the OpenShift homepage, navigate to **Networking** → **Routes** , which has routes to **minio-api** and **minio-ui**.
    2. Use the **minio-ui** route to access the Minio UI (Figure 1) and the `minio_root_user` and `minio_root_password` from step 5 to log in.

Figure 1: The MinIO dashboard.
With that, our storage for models is ready for use.
#### Disabling SSL verification
A possible obstacle is that if the MinIO Route is deployed with the default self-signed certificate, KServe (the underlying model serving platform) will fail to pull the model because it cannot verify the SSL certificate. The KServe `InferenceService` does not expose a simple `verify_ssl: false` option.
As a workaround, you must manually modify the data connection secret to disable SSL verification. Follow these steps:
  1. Create the initial connection for MinIO in the OpenShift AI dashboard.
  2. Create the `InferenceService` that will use this connection. This triggers an operator to create a secret named storage-config in your project namespace.
  3. Follow the steps outlined in the Red Hat Knowledgebase article, [Disable SSL verification for a data connection](https://access.redhat.com/solutions/7047512).This process involves adding an annotation to the secret to prevent the operator from overwriting your changes and then updating the secret's data to include the SSL verification flag.


Note
If you have multiple data connections, you might need to manually base64 decode the secret's contents, edit the specific connection, and re-encode it, as the `sed` command in the article might not work correctly for multiple entries.
### Upload the model to MinIO
Before moving ahead with uploading the model, you will need to download it from the [Hugging Face library](https://huggingface.co/models). LLM model sizes are generally large, so you can't download them directly. To download the model, you can either enable [Git LFS (Large File Storage)](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage) or use the [Hugging Face CLI](https://huggingface.co/docs/huggingface_hub/en/guides/cli), which aids in downloading the large files. In this setup, I used Git with Git LFS enabled to download the Llama model.
Once Git LFS is installed, downloading the model is just like cloning any repository:

```
git clone https://huggingface.co/<organization>/<repository_name>
Example: git clone https://huggingface.co/meta-llama/Llama-3.1-8B
```

Upload the downloaded model to MinIO using the MinIO UI Upload button in the dashboard, shown in Figure 2.
Figure 2: MinIO upload option.
### Ensure you have GPU nodes on your cluster
For the inference service, we need to make sure we have GPU nodes available on our cluster. Follow [this guide to create a GPU-enabled node with OpenShift 4.2 in Amazon EC2](https://www.redhat.com/en/blog/creating-a-gpu-enabled-node-with-openshift-4-2-in-amazon-ec2).
### Install and configure the Node Feature Discovery operator
Just adding a worker node with a GPU is not enough to use the GPU-enabled nodes for model serving. The Node Feature Discovery (NFD) operator in OpenShift automates the process of detecting hardware features and system configurations within an OpenShift cluster. Its primary function is to label nodes with hardware-specific information. It helps us label our GPU node and manage resources for workloads.
To install the Node Feature Discovery operator, follow the [Red Hat documentation guidance](https://docs.openshift.com/container-platform/latest/hardware_enablement/psap-node-feature-discovery-operator.html).
To verify and configure NFD, follow this [reference documentation](https://docs.nvidia.com/datacenter/cloud-native/openshift/23.9.1/install-nfd.html#install-nfd).
### Install and configure the NVIDIA GPU operator
The Node Feature Discovery operator helps properly discover and label the capabilities of nodes, but nodes still require additional configuration to be able to run GPU-accelerated workloads. This is where we use the NVIDIA GPU operator for OpenShift to automatically deploy, configure, and manage the NVIDIA software stack needed to run GPU-accelerated workloads.
  1. From the OpenShift homepage, go to **Operators** → **OperatorHub**. You can select the namespace where you want to deploy the GPU operator; we suggest `nvidia-gpu-operator`.
  2. Search for the **NVIDIA GPU Operator**. Select the operator andclick **Install**.
  3. To verify and configure the NVIDIA GPU operator, follow the instructions in the [reference documentation](https://docs.nvidia.com/datacenter/cloud-native/openshift/23.9.1/install-gpu-ocp.html#install-nvidiagpu).


### Deploy and debug the InferenceService
Once you've configured the storage and the Node Feature Discovery and NVIDIA GPU operators are in place, you can now deploy the model in a `ServingRuntime` on the GPU node we provisioned.
  1. Log in to the OpenShift AI dashboard as the new clusteradmin user (Figure 3).
Figure 3: OpenShift AI dashboard.
  2. From the left-hand menu, select **Models →** **Model deployments**(Figure 4).
Figure 4: Navigate to the Model deployments overview in OpenShift AI.
  3. Select the project in which you want to deploy the model.
  4. Click **Deploy model** (Figure 5).
Figure 5: Deploy model option available.
  5. Fill in the required details (Figure 6):
    1. Add the serving runtime as **vLLM NVIDIA GPU ServingRuntime for KServe**.
    2. In the **Connection** drop-down menu (Figure 7), locate the MinIO setup and select that.
Figure 6: Configure the model deployment properties: name, serving runtime, model framework, and more. Figure 7: Select the minio connection when configuring the source model location.
  6. Click **Deploy** to create the `InferenceService`, pointing to the model artifacts in the MinIO storage.


### Create an external route
To interact with the service, you need to create an external route.
  1. From the OpenShift, select **Networking** → **Routes** → **Create Route**.
  2. Select the **Form view** option and fill the details accordingly (Figure 8).
    1. **Name:** llama
    2. **Service:** llamas-predictor
    3. **Target port:** 80 → 8080 (TCP)
    4. **Secure Route:** Enabled
    5. **TLS termination:** Edge
    6. **Insecure traffic:** Redirect

Figure 8: Add the External Route details. Evaluating our inference server's performance
## Evaluating our inference server's performance
[GuideLLM](https://pypi.org/project/guidellm/) is a tool for analyzing and evaluating LLM deployments. By simulating real-world inference workloads, GuideLLM enables users to assess the performance, resource requirements, and cost implications of deploying LLMs on various hardware configurations. It is Python-based and can be easily installed with `pip`:

```
pip install guidellm
```

GuideLLM offers a rich set of command-line options, enabling users to customize prompt and output lengths in terms of tokens for precise control over benchmarking scenarios. One particularly useful feature is its ability to begin with synchronous requests and gradually scale up to maximum concurrency. This helps you evaluate the inference server's performance under varying, stepwise throughput loads.
Here is the command I used to kick off the benchmarking in my setup:

```
guidellm benchmark run \
--target "http://localhost:8000" \
--backend-args '{"verify": false}' \
--processor ./local_tokenizer \
--data='{"prompt_tokens": 128, "output_tokens": 2000}' \
--rate-type synchronous \
--max-seconds 200
```
Results
## Results
We ran a few tests and found that the p99 (99th percentile) Time to First Token (TTFT) was fewer than 1,000 milliseconds and that the p99 of the Inter-Token Latency (ITL) was fewer than 52 milliseconds, which met our SLOs for these KPIs (fewer than 1,500 and 200 milliseconds, respectively). Here are the detailed results:

```
"metadata_benchmark": {
	"synchronous1": {
		"reques_stats": {
			"per_second": 0.13,
"concurrency": 1,
},
"output_tokpersec": {
	"mean": 17.2,
},
"tot_tokpersec": {
	"mean": 34.6,
},
"req_latency": {
	"mean": 7.41,
	"median": 7.41,
	"p99": 7.42,
},
"ttft_in_ms": {
	"mean": 860,
	"median": 866,
	"p99": 909,
},
"itl_in_ms": {
	"mean": 51.6,
	"median": 51.8,
	"p99": 51.8,
},
"tpot_in_ms": {
	"mean": 51.2,
	"median": 51.2,
	"p99": 51.4,
},
	},
	"synchronous2": {
		"reques_stats": {
			"per_second": 0.14,
"concurrency": 1,
},
"output_tokpersec": {
	"mean": 17.3,
},
"tot_tokpersec": {
	"mean": 34.7,
},
"req_latency": {
	"mean": 7.39,
	"median": 7.4,
	"p99": 7.4,
},
"ttft_in_ms": {
	"mean": 837.7,
	"median": 838.4,
	"p99": 854.6,
},
"itl_in_ms": {
	"mean": 51.6,
	"median": 51.6,
	"p99": 52,
},
"tpot_in_ms": {
	"mean": 51.2,
	"median": 51.2,
	"p99": 51.6,
},
	},
	"synchronous3": {
		"reques_stats": {
			"per_second": 0.13,
"concurrency": 1,
},
"output_tokpersec": {
	"mean": 17.2,
},
"tot_tokpersec": {
	"mean": 34.6,
},
"req_latency": {
	"mean": 7.41,
	"median": 7.38,
	"p99": 7.56,
},
"ttft_in_ms": {
	"mean": 861.2,
	"median": 842.2,
	"p99": 999,
},
"itl_in_ms": {
	"mean": 51.6,
	"median": 51.5,
	"p99": 51.9,
},
"tpot_in_ms": {
	"mean": 51.2,
	"median": 51.1,
	"p99": 51.5,
},
	},
}
```
Next steps
## Next steps
In this article, we demonstrated how to set up storage for large language models. We then used it to deploy an LLM with OpenShift AI to provide an inference service. Finally, we evaluated the inference service's performance in terms of our key performance indicators.
Further resources:
  * [How to run vLLM on CPUs with OpenShift for GPU-free inference](https://developers.redhat.com/articles/2025/06/17/how-run-vllm-cpus-openshift-gpu-free-inference)
  * Red Hat course: [Developing and Deploying AI/ML Applications on Red Hat OpenShift AI](https://role.rhu.redhat.com/rol-rhu/app/courses/ai267-2.13/pages/pr01)
  * [How to enable Ansible Lightspeed intelligent assistant](https://developers.redhat.com/articles/2025/09/16/how-enable-ansible-lightspeed-intelligent-assistant)
  * [NVIDIA GPU Operator documentation](https://docs.nvidia.com/datacenter/cloud-native/openshift/23.9.1/introduction.html)
  * [How to deploy an LLM on RedHat OpenShift on GitHub](https://github.com/eartvit/llm-on-ocp?tab=readme-ov-file#how-to-deploy-an-llm-on-redhat-openshift)


### Platforms
  * [Red Hat Enterprise Linux](https://developers.redhat.com/products/rhel/overview)
  * [Red Hat Ansible Automation Platform](https://developers.redhat.com/products/ansible/overview)
  * [See all products](https://developers.redhat.com/products)


### Build
  * [Developer tools](https://developers.redhat.com/topics/developer-tools)
  * [Interactive tutorials](https://developers.redhat.com/learn#assembly-id-70181)


### Quicklinks
  * [Learning resources](https://developers.redhat.com/learn)


### Communicate
  * [Report a website issue](https://developers.redhat.com/report_a_website_issue)
  * [Site status dashboard](https://status.redhat.com/)
  * [Report a security problem](https://access.redhat.com/security/team/contact/)


### RED HAT DEVELOPER
Build here. Go anywhere.
We serve the builders. The problem solvers who create careers with code.
Join us if you’re a developer, software engineer, web designer, front-end designer, UX designer, computer scientist, architect, tester, product manager, project manager or team lead.
### Red Hat legal and privacy links
  * [Cool Stuff Store](https://coolstuff.redhat.com/)

© 2026 Red Hat
### Red Hat legal and privacy links
  * [All policies and guidelines](https://redhat.com/en/about/all-policies-guidelines)
  * [Digital accessibility](https://redhat.com/en/about/digital-accessibility)
  * Cookie Preferences and Opt-Out Rights


