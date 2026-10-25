---
title: AI Inference Orchestration with Kubernetes on Instinct MI300X, Part 3 — ROCm
  Blogs
id: ai-inference-orchestration-with-kubernetes-on-instinct-mi300x-part-3-rocm-blogs
tags:
- onprem-llm-inference-stacks-859099
- hardware
- vendor-tutorial
- amd-rocm
created: '2026-10-23T17:21:59.084226Z'
updated: '2026-10-23T17:24:55.645290Z'
source: https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html
source_domain: rocm.blogs.amd.com
fetched_at: '2026-10-23T17:21:59.051223Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'AMD ROCm blog tutorial (13 Mar 2025, final part of series): Prometheus+Grafana
  with AMD Device Metrics Exporter via AMD GPU Operator, MetalLB LoadBalancer, vLLM
  on MI300X in Kubernetes, Open WebUI as OpenAI-compatible frontend operating offline.
  Shows the AMD analogue of the NVIDIA GPU Operator/DCGM stack. Vendor how-to, not
  a deployment case; no scale disclosed.'
---

[Skip to main content](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#main-content)
Back to top 
[ ROCm blogs ](https://rocm.blogs.amd.com/)
# AI Inference Orchestration with Kubernetes on Instinct MI300X, Part 3
##  Contents 
# AI Inference Orchestration with Kubernetes on Instinct MI300X, Part 3[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#ai-inference-orchestration-with-kubernetes-on-instinct-mi300x-part-3 "Link to this heading")
March 13, 2025 by [Victor Robles](https://rocm.blogs.amd.com/authors/victor-robles.html).
3 min read. | 730 total words.
[Software tools & optimizations](https://rocm.blogs.amd.com/blog/category/software-tools-optimizations.html)
Welcome back to the final part of our series! So far, we’ve successfully setup up a Kubernetes cluster and installed the AMD GPU Operator to seamlessly integrate AMD hardware with Kubernetes in . We’ve deployed vLLM on AMD Instinct MI300X GPUs, exposed it using MetalLB, and scaled it efficiently in .
Building on the groundwork from Parts 1 and 2, this blog will help you implement advanced tools to monitor GPU performance, visualize metrics, and provide a user-friendly interface for interacting with your large language models (LLMs). By the end of this final blog in our series, your deployment will be equipped with end-to-end observability and accessibility, ensuring a robust and scalable production environment.
## Prerequisites[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#prerequisites "Link to this heading")
Before proceeding, ensure that:
  * The AMD GPU Operator is installed ().
  * vLLM is deployed in your cluster ().
  * MetalLB is configured for external service exposure ().


## Metrics collection and visualization[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#metrics-collection-and-visualization "Link to this heading")
Our next step is implementing comprehensive monitoring for our GPU-accelerated workloads. AMD’s GPU Operator utilizes the **[AMD Device Metrics Exporter](https://instinct.docs.amd.com/projects/device-metrics-exporter/en/latest/)**. We’ll use Prometheus to collect metrics from the Exporter and Grafana to visualize them, providing insights into GPU utilization, memory usage, and overall system performance.
### Add Prometheus and Grafana Helm repositories[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#add-prometheus-and-grafana-helm-repositories "Link to this heading")

```
helmrepoaddprometheus-communityhttps://prometheus-community.github.io/helm-charts
helmrepoaddgrafanahttps://grafana.github.io/helm-charts
helmrepoupdate

```
Copy to clipboard
Note
Since we already configured MetalLB in [Part 2](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part2/README.html#install-the-load-balancer), we can leverage it to expose both Prometheus and Grafana services externally via LoadBalancer type services.
## Install Prometheus[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#install-prometheus "Link to this heading")

```
helminstallprometheusprometheus-community/prometheus\
--namespacemonitoring\
--create-namespace\
--setserver.service.type=LoadBalancer

```
Copy to clipboard
Once installed, we need to edit the `configmap` of Prometheus to add the metrics from the `gpu-operator-metrics-exporter` as another scrape source.
First, let us get the `CLUSTER-IP` of the `gpu-operator-metrics-exporter` by running:

```
kubectlgetsvc-nkube-amd-gpu

```
Copy to clipboard
We should see something similar to the following:

```
NAMETYPECLUSTER-IPEXTERNAL-IPPORT(S)AGE
amd-gpu-operator-kmm-controller-metrics-serviceClusterIP10.152.183.145<none>8443/TCP23h
amd-gpu-operator-kmm-webhook-serviceClusterIP10.152.183.172<none>443/TCP23h
gpu-operator-metrics-exporterNodePort10.152.183.144<none>5000:32500/TCP

```
Copy to clipboard
Note
Take note of the `CLUSTER-IP` for the `gpu-operator-metrics-exporter` service - you’ll need this for the next step.
Edit the Prometheus configuration to add the metrics source:

```
kubectleditconfigmapprometheus-server-nmonitoring

```
Copy to clipboard
Navigate to the `scrape_configs` section of the configuration and add the following `job_name` entry

```
-job_name:'gpu-operator-metrics-exporter'
static_configs:
-targets:
-'<METRICS-EXPORTER-CLUSTER-IP>:5000'# Replace with your metrics exporter's CLUSTER-IP
metrics_path:'/metrics'

```
Copy to clipboard
To confirm that Prometheus is scraping the metrics, navigate to the Prometheus UI in your browser by getting the `EXTERNAL IP` of Prometheus via:

```
kubectlgetsvc-nmonitoring

```
Copy to clipboard
You should see an entry similar to the following Prometheus server:

```
NAMETYPECLUSTER-IPEXTERNAL-IPPORT(S)AGE
prometheus-serverLoadBalancer10.152.183.14310.216.71.23180:32617/TCP161m

```
Copy to clipboard
Navigating to the `EXTERNAL-IP` in your browser and you should be greeted by the Prometheus UI:
Confirm that Prometheus is scraping metrics correctly from the `gpu-operator-metrics-exporter` by going to `Status  Target Health`
There you should see the `gpu-operator-metrics-exporter` with `State` in the mode. If you are still unable to see the metrics, confirm the `scrape_configs` section of the configmap we edited is formatted correctly and try again (this is usually the main source of issues).
### Install and configure Grafana[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#install-and-configure-grafana "Link to this heading")
To install Grafana we will run:

```
helminstallgrafanagrafana/grafana\
--namespacemonitoring\
--setserver.service.type=LoadBalancer

```
Copy to clipboard
To confirm that our load balancer has assigned external addresses to Grafana we can run:

```
kubectlgetservices-nmonitoring

```
Copy to clipboard
Among other services, we should also see something similar to:

```
NAMETYPECLUSTER-IPEXTERNAL-IPPORT(S)AGE
grafanaLoadBalancer10.152.183.12610.216.71.23280:32658/TCP100m

```
Copy to clipboard
Note the `EXTERNAL-IP` of Grafana, it’s what we will be using to navigate to its UI in our web browser. In our example above we would navigate to `http://10.216.71.232/login`.
You should be greeted with the Grafana login screen below.
The default admin username is `admin` but you will need to get the admin password by running the following in your terminal:

```
kubectlgetsecret--namespacemonitoringgrafana-ojsonpath="{.data.admin-password}"|base64--decode;echo

```
Copy to clipboard
#### Configure Grafana data source[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#configure-grafana-data-source "Link to this heading")
Grafana needs to be initialized with a Prometheus data source for metric scraping. To do this, we utilize the Grafana UI.
  1. Access the Grafana UI using the external IP (retrieve using `kubectl get svc -n monitoring`)
  2. Navigate to Connections > Data Sources
  3. Click “Add data source” and select Prometheus
  4. Enter the Prometheus server URL (http://)
  5. Click “Save & Test” to verify the connection


#### Import GPU Dashboards[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#import-gpu-dashboards "Link to this heading")
The ROCm GPU Operator provides pre-configured Grafana dashboards for monitoring Instinct GPUs. These can be found in the [ROCm GPU Operator GitHub repository](https://github.com/ROCm/gpu-operator/tree/main/grafana).
To import a dashboard:
  1. Navigate to Dashboards > Import in the Grafana UI
  2. Copy the contents of any dashboard JSON file from the repository
  3. Paste into the import wizard
  4. Select your Prometheus data source
  5. Click “Import”


## Implementing Open WebUI[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#implementing-open-webui "Link to this heading")
Now that we have our vLLM inference engine deployed (and scaling) we could provide an out-of-the-box intuitive web UI for end users to interact with LLMs on AMD hardware. For that we will utilize [Open WebUI](https://github.com/open-webui/open-webui).
Open WebUI is an extensible, feature-rich, and user-friendly self-hosted WebUI designed to operate entirely offline. It supports various LLM runners, including Ollama and OpenAI-compatible APIs.
Since vLLM utilizes an OpenAI-compatible API for hosting LLMs, Open WebUI will be able to use it as our backend LLM service.
We will utilize similar deployment manifests as our vLLM deployment in [Part 2](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part2/README.html#deploying-vllm) to deploy Open WebUI. These are:
### Deploy Open WebUI[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#deploy-open-webui "Link to this heading")
We’ll create three Kubernetes manifests to deploy Open WebUI:
  1. `open-webui-pvc.yaml` - persistent storage for Open WebUI’s user management and chat histories

```
#open-webui-pvc.yaml

apiVersion:v1
kind:PersistentVolumeClaim
metadata:
name:open-webui-pvc
namespace:default
spec:
accessModes:
-ReadWriteOnce
resources:
requests:
storage:10Gi# Adjust storage size as needed

```
Copy to clipboard
  2. `open-webui-deployment.yaml` - an instance of Open WebUI configured to connect to our vLLM engine

```
apiVersion:apps/v1
kind:Deployment
metadata:
name:open-webui
namespace:default
spec:
replicas:1
selector:
matchLabels:
app:open-webui
template:
metadata:
labels:
app:open-webui
spec:
containers:
-name:open-webui
image:ghcr.io/open-webui/open-webui:0.5.18
ports:
-containerPort:8080# Container exposes port 8080
env:
-name:OPENAI_API_BASE_URL
value:"http://10.228.20.200/v1"# Use your vLLM service's external IP
-name:OPENAI_API_KEY
value:"test1234"# Necessary for Open WebUI to function, can be anything
-name:ENABLE_OLLAMA_API
value:"false"
-name:ENABLE_RAG_WEB_SEARCH
value:"false"
-name:RAG_WEB_SEARCH_ENGINE
value:"duckduckgo"
volumeMounts:
-mountPath:/app/backend/data# Mount path inside the container
name:open-webui-data
volumes:
-name:open-webui-data
persistentVolumeClaim:
claimName:open-webui-pvc

```
Copy to clipboard
  3. `open-webui-service.yaml` - a service manifest for exposing the WebUI outside our cluster

```
#open-webui-service.yaml

apiVersion:v1
kind:Service
metadata:
name:open-webui# Indentation fixed here
namespace:default
spec:
type:LoadBalancer# Exposes the service externally
ports:
-port:80# External port to expose
targetPort:8080# Port the container is listening on
protocol:TCP
selector:
app:open-webui

```
Copy to clipboard


Note
Make sure to update the `OPENAI_API_BASE_URL` in the deployment manifest to match your vLLM service’s external IP address.
We apply the manifests with

```
kubectlapply-fopen-webui-pvc.yaml
kubectlapply-fopen-webui-deployment.yaml
kubectlapply-fopen-webui-service.yaml

```
Copy to clipboard
### Verify the Deployment[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#verify-the-deployment "Link to this heading")
After a few moments of the container being pulled and pods spinning up, running `kubectl get deployments` should show output similar to this:

```
NAMEREADYUP-TO-DATEAVAILABLEAGE
llama-3-2-1b1/1114h9m
open-webui1/1112s

```
Copy to clipboard
Running `kubectl get pods` should show:

```
NAMEREADYSTATUSRESTARTSAGE
llama-3-2-1b-55d7d48f67-crdkj1/1Running097m
open-webui-54df7bb5c-457bt1/1Running047s

```
Copy to clipboard
Finally, running `kubectl get svc` will give us the `EXTERNAL-IP` of our Open WebUI service

```
NAMETYPECLUSTER-IPEXTERNAL-IPPORT(S)AGE
kubernetesClusterIP10.152.183.1<none>443/TCP2d1h
llama-3-2-1bLoadBalancer10.152.183.20210.216.71.2308000:32703/TCP4h3m
open-webuiLoadBalancer10.152.183.11110.216.71.23380:31240/TCP4h3m

```
Copy to clipboard
Access the WebUI by navigating to `http://<OPEN-WEBUI-EXTERNAL-IP>` in your browser.
Note
The first user to register on Open WebUI automatically becomes an administrator. Secure these credentials appropriately for production deployments.
In our example we will open up our browser to `http://10.216.71.233`. If you are the first user of this instance of the app, you will be greeted by the `Get Started` webpage and asked to provide a `name`, `email` and `password` once you click. By default, this first account becomes an admin account.
After logging in you should see the following.
If the deployment was successful in reaching the vLLM API, you should now see an option to choose the model hosted by vLLM for use in Open WebUI. In our instance, that is `unsloth/Llama-3.2-1B-Instruct`
Now you can talk to your model however you see fit!
## Summary[#](https://rocm.blogs.amd.com/artificial-intelligence/k8s-orchestration-part3/README.html#summary "Link to this heading")
This concludes our series on AI inference orchestration with Kubernetes on AMD Instinct MI300X. With this setup, you are now equipped to handle large-scale AI inference workloads efficiently. By following this 3-part guide, you have successfully set up a robust and scalable infrastructure, covering:
  * Kubernetes and Helm installation
  * Persistent storage setup for caching LLMs
  * AMD GPU Operator integration
  * vLLM engine deployment
  * Load Balancing via MetalLB, and
  * Monitoring with Prometheus and Grafana,
  * Open WebUI for an intuitive user interface.


As you move forward, consider the following next steps to optimize and expand your deployment:
  * Monitor: Continuously track performance metrics and ensure system reliability using tools like Grafana and Prometheus.
  * Optimize: Experiment with GPU and fine-tuning frameworks to fine-tune your models.
  * Enhance: Leverage additional Kubernetes resources such as external data stores to strengthen your infrastructure.
  * Learn: Stay updated with the latest developments by exploring vLLM documentation and community forums.
  * **Upgrade** : Take advantage of our recently released AMD GPU Operator v1.2.0, which introduces new capabilities including automated GPU health monitoring with customizable thresholds, intelligent removal of unhealthy GPUs from the scheduling pool, seamless driver upgrades with node drain/cordon management, and a test runner for diagnosing GPU issues. For complete details, check the [GitHub release](https://github.com/ROCm/gpu-operator/releases/tag/v1.2.0) or the [full release notes](https://instinct.docs.amd.com/projects/gpu-operator/en/latest/releasenotes.html).


With this guide as a foundation, you are now prepared to handle large-scale AI inference workloads efficiently. Best of luck as you continue to build and innovate with Instinct on Kubernetes!
Disclaimers Third-party content is licensed to you directly by the third party that owns the content and is not licensed to you by AMD. ALL LINKED THIRD-PARTY CONTENT IS PROVIDED “AS IS” WITHOUT A WARRANTY OF ANY KIND. USE OF SUCH THIRD-PARTY CONTENT IS DONE AT YOUR SOLE DISCRETION AND UNDER NO CIRCUMSTANCES WILL AMD BE LIABLE TO YOU FOR ANY THIRD-PARTY CONTENT. YOU ASSUME ALL RISK AND ARE SOLELY RESPONSIBLE FOR ANY DAMAGES THAT MAY ARISE FROM YOUR USE OF THIRD-PARTY CONTENT. 
Contents 
## Cookie Notice
This site uses cookies from us and our partners to make your browsing experience more efficient, relevant, convenient and personal. In some cases, they are essential to making the site work properly. By accessing this site, you direct us to use and consent to the use of cookies. You can change your settings by clicking on the Cookie Settings link. For more information, refer to AMD's [privacy notice](https://www.amd.com/en/legal/privacy.html) and [cookie policy](https://www.amd.com/en/legal/cookies.html).
Cookie Settings Accept Cookies
## Cookie Settings
When you visit any website, it may store or retrieve information on your browser, mostly in the form of cookies. This information might be about you, your preferences, or your device, and is mostly used to make the site work as you expect. The information does not usually identify you directly, but it can give you a more personalized web experience. Because we respect your right to privacy, you can choose not to allow some types of cookies. Click on the different category headings to learn more and change our default settings. Blocking some types of cookies may impact your experience of the site and the services we are able to offer. [More information](https://www.amd.com/en/legal/cookies.html)
Allow All
### Manage Consent Preferences
#### Performance Cookies
Performance Cookies
These cookies allow us to recognize and count the number of visitors and to see how visitors move around the Sites when they use them. This helps us to understand what areas of the Sites are of interest to you and to improve the way the Sites work, for example, by helping you find what you are looking for easily. We may use third party web analytics providers to help us analyze the use of the Sites, email, and newsletters. These cookies store data such as online identifiers (including IP address and device identifiers), information about your web browser and operating system, website usage activity information (including the frequency of your visits, your actions on the Sites and, if you arrived at any of the Sites from another website, i.e. the URL of that website), and content-related activity (including the email and newsletter content you view and click on). 
Cookies Details
#### Targeting Cookies
Targeting Cookies
These cookies record online identifiers (including IP address and device identifiers), information about your web browser and operating system, website usage activity information (such as information about your visit to the Sites, the pages you have visited, content you have viewed, and the links you have followed), and content-related activity (including the email and newsletter content you view and click on). The information is used to try to make the Sites, emails, and newsletters, and the advertising displayed on them and other websites more relevant to your interests. For instance, when you visit the Sites, these targeting cookies are used by third party providers for remarketing purposes to allow them to show you advertisements for our products when you visit other websites on the internet. Our third party providers may collect and combine information collected through the Sites, emails, and newsletters with other information about your visits to other websites and apps over time, if those websites and apps also use the same providers.
Cookies Details
#### Functionality Cookies
Functionality Cookies
These cookies are used to recognize you when you return to the Sites. This enables us to remember your preferences (for example, your choice of language or region) or when you register on areas of the Sites, such as our web programs or extranets. These cookies store data such as online identifiers (including IP address and device identifiers) along with the information used to provide the function.
Cookies Details
#### Strictly Necessary Cookies
Always Active
These are cookies that are technically required for the operation of the Sites. They are usually only set in response to actions made by you which amount to a request for services, such as setting your privacy preferences, logging into secure areas of the Sites or filling in forms. These cookies store data such as online identifiers (including IP address and device identifiers) along with the information used to operate the Sites. We may estimate your geographic location based on your IP address to help us display the content available in your location and adjust the operation of the Sites.
Cookies Details
Back Button
### Cookie List
Search Icon
Filter Icon
Clear
  * checkbox label label


Apply Cancel
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Confirm My Choices
