---
title: Atlassian’s Inference Engine, our self-hosted AI inference service - Inside
  Atlassian
id: atlassians-inference-engine-our-self-hosted-ai-inference-service-inside-atlassia
tags:
- internal-llm-inference-stacks
- atlassian
- triton-inference-server
- vllm
- tensorrt-llm
- kubernetes
- karpenter
- first-person-account
created: '2026-10-09T22:52:36.000321Z'
updated: '2026-10-10T14:52:16.969478Z'
source: https://www.atlassian.com/blog/atlassian-engineering/inference-engine
source_domain: www.atlassian.com
fetched_at: '2026-10-09T22:52:35.977146Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Atlassian engineering blog (Jul 22 2025) on ''Atlassian''s Inference Engine,''
  a self-hosted inference platform built after off-the-shelf (third-party API) hosting
  hit latency SLOs, vendor lock-in, and pricing-control problems. Stated results:
  up to 40% P90 latency reduction (LLM, vs 3rd-party hosting) and >60% cost reduction
  for LLM workloads; 63% P90 latency reduction and >80% cost reduction for non-LLM
  workloads; a specific example of 72% latency reduction for query-rewriting LLM workloads
  via model/instance sizing, gRPC, and client-side tokenization. Runs on KITT (Atlassian''s
  internal Kubernetes platform) with Kubernetes, Karpenter (node autoprovisioning),
  ArgoCD (GitOps), Helm, and Kustomize for deployment — explicitly OSS/off-the-shelf,
  chosen over FluxCD/plain-YAML-CI (less control/auditability) and over Cluster Autoscaler
  (Karpenter has faster provisioning for heterogeneous GPU types). Inference backend:
  NVIDIA Triton Inference Server with TensorRT-LLM and vLLM as backends — vLLM praised
  for batching performance and run-time deployability of new models; TensorRT-LLM
  customization credited for the 72% query-rewriting latency win. Routing: Istio ingress
  (chosen over Linkerd/NGINX for retry/observability/traffic-splitting control); autoscaling
  via KEDA (chosen over HPA/cron scale-ups for clean Prometheus-metric integration);
  observability via Prometheus + SignalFx. A control plane ''Optimization Service''
  compiles/tunes/benchmarks uploaded model artifacts (GPU-provisioned via Karpenter)
  — this compilation/optimization service itself is described as custom-built, as
  is the overall Inference Engine; gRPC + client-side tokenization are called out
  as in-house optimization techniques layered onto the OSS Triton/TensorRT-LLM stack.
  The piece is vendor-authored marketing-adjacent content (Atlassian''s own ''How
  We Build'' blog) so cost/latency percentages are self-reported without third-party
  verification.'
utility_score: 17.0
---

[Skip to content](https://www.atlassian.com/blog/atlassian-engineering/inference-engine#wp--skip-link--target)
# Atlassian’s Inference Engine, our self-hosted AI inference service
July 22, 2025
Published In [How We Build](https://www.atlassian.com/blog/search?topic%5B0%5D=how-we-build)
## Powering Enterprise-scale AI
As Atlassian’s AI capabilities continue to scale rapidly across multiple products, a pressing challenge emerged: how do we deliver world-class AI-powered solutions to millions of users without compromising on latency, flexibility, and operational control?
The answer: **Atlassian’s Inference Engine,** our custom-built, self-hosted AI inference platform that now powers production LLMs, search models, content moderators, and more across the Atlassian Cloud.
With it, we’ve achieved:
  * Latency Improvements for LLM and non-LLM workloads: 
    * For LLM workloads (apples-to-apples comparison moving from 3rd party hosting to Atlassian’s Inference Engine) we’ve observed up to **40% P-90 latency reduction** at scale.
    * For non-LLM workloads on an apples-to-apples comparison, we have observed up to **63% P-90 latency reduction** at scale.
  * Cost Savings for LLM and non-LLM Workloads 
    * For LLM workloads, infrastructure migration from 3rd party to hosting on Atlassian’s Inference Engine, we’ve observed **> 60% cost reduction.**
    * For non-LLM workloads, infrastructure migration from 3rd party to hosting on Atlassian’s Inference Engine, we’ve observed **> 80% cost reduction.**
  * Enhance observability and debugging at every layer
  * Rapid iteration and support for models across architectures


## Why We Built It
Off-the-shelf solutions got us off the ground, but as usage scaled and product teams demanded more control, bottlenecks became unavoidable:
  * Latency that didn’t meet our SLOs
  * Rigid deployment experience
  * Vendor lock-in and hard to troubleshoot
  * Pricing that didn’t match our optimization goals


So we built a solution from first principles. The Atlassian Inference Engine isn’t a proof-of-concept, it’s a foundation in how we ship AI at Atlassian.
## Technical Challenges & Early Concerns
Building our own inference platform wasn’t a decision we took lightly, we knew we’d be taking ownership of everything from optimization and performance to uptime and cost control, and we had real concerns going in.
Some of the early challenges and questions we asked ourselves included:
  * How can we scale inference and still manage cost with performance?
  * What tools can help us experiment and iterate faster?
  * How do we make sure our performance meets the bar for latency and throughput?
  * Can we make open-source models competitive in terms of performance and quality of proprietary models delivered through third-party APIs?
  * What sort of approaches can we take in the design to allow our team to scale output?


## Architecture Overview
Atlassian’s Inference Engine is built on a foundation of open-source technologies, with internal systems layered in to support enterprise-grade AI workloads. This initiative laid the groundwork for AI workloads on KITT (Atlassian’s internal Kubernetes platform).
Under the hood, Atlassian’s Inference Engine runs on a modern, cloud-native stack:
  * [Kubernetes](https://kubernetes.io/) – the backbone for container orchestration
  * [Karpenter](https://karpenter.sh/) – for dynamic provisioning of nodes based on real-time demand
  * [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) – enabling GitOps-style deployment workflows and declarative infrastructure management
  * [Helm](https://helm.sh/) – for parameterized and version-controlled model deployments.


This stack allows us to fully automate cluster provisioning, model rollouts, and versioning, and gives us fine-grained control over GPU spend, rollout safety, and operational observability without being locked into a proprietary platform.
We designed Atlassian’s Inference Engine to scale our impact. That meant investing early in designing for automation and clean interfaces between systems. This mindset has helped us support dozens of models in parallel, optimize for cost performance, and move fast without compromising reliability.
## Deployment Model: GitOps for Reliability and Traceability
We use a Git-based deployment model for everything from system configurations to model deployments. Each environment is managed via declarative manifests stored in Git, with ArgoCD continuously reconciling the desired state.
This approach gives us:
  * Full version control over every change to infrastructure and model rollout
  * Auditable, reproducible deployments across staging and production
  * Safer rollbacks through atomic Git-based diffs
  * Clear separation of config vs runtime concerns


Our Git deployment approach also reduces human error and simplifies collaboration, proposed changes are brought up through PRs, validated with CI, and roll out safely through Git merges. This model gives our team confidence in how we scale inference across regions, models, and versions.
## Optimization Stack
For model inferencing and optimization, our go-to setup is [NVIDIA Triton Inference Server,](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/index.html) and for GenAI models, we use [TensorRT-LLM](https://docs.nvidia.com/tensorrt-llm/index.html) and [vLLM](https://docs.vllm.ai/en/latest/) as backends. These tools have allowed us to realize impactful latency reductions at scale, with open-source setups, and have the flexibility to deploy a wide array of AI solutions. Here are some key optimization benefits:
  * Triton Inference Server gives us the flexibility to deploy a variety of models, everything from search to LLM.
  * With the Triton Inference Server we run [gRPC](https://grpc.io/) communications for our setups, providing significant performance improvements.
  * Client-side tokenization is heavily encouraged for users as it separates CPU workloads from the GPU and allows our service to talk in terms of protobuf and tokens. This renders our system agnostic to client-side code libraries, allowing for seamless product team integration.
  * Our in-house optimization techniques and enablement of customization on TensoRT-LLM have given us boosts in performance for latency and throughput. 
    * For example: We’ve seen internal LLM workloads for query rewriting having up to 72% latency reductions while maintaining performance at higher load and scale, this was accomplished through a combination of model sizing, instance sizing, gRPC enablement, client-side tokenization, and leveraging the Triton/TensorRT-LLM stack on our infra.
  * The customizability of Triton and TensorRT-LLM together has allowed us to experiment and iterate on various performance settings, compilation approaches, and batching techniques to tune our configurations for the wide array of deployments we facilitate.
  * vLLM has shown great performance for batching as well as granting the ability to facilitate at run-time deployments and deploy new models as they’re released.


This high-level diagram outlines the structure of Atlassian’s Inference Engine, showing the split between the Control Plane, where models are prepared and optimized, and the Data Plane, where inference happens at scale.
## Control Plane – Preparing the Models
Once model artifacts are uploaded to our internal registry:
  * The Optimization Service compiles and tunes them
  * It’s fully automated, GPU-provisioned using Karpenter
  * Supports parallel, concurrent optimizations at scale
  * Output models are tested and benchmarked for deployment readiness


We’re also experimenting with benchmarking different compilation strategies and broader container support.
## Data Plane – Serving Inference at Scale
  * [Helm](https://helm.sh/) + [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) + [Kustomize](https://kustomize.io/) – We use Helm for parameterized, templated deployments, Kustomize for overlaying environment-specific configs, and ArgoCD for continuous GitOps-based reconciliation. With this setup we have fine-grained control and reusable templates. Alternatives like FluxCD or plain YAML + CI pipelines were considered, but did not offer the same depth, control, auditable processes, and operational fit.
  * [Karpenter](https://karpenter.sh/) – The Atlassian Inference Engine uses Karpenter for node provisioning. Compared to alternatives like Cluster Autoscaler (which requires pre-defined node groups and scales more conservatively), Karpenter offers faster provisioning, and flexible instance selection. This is especially important for LLM workloads running on heterogeneous GPU types, where minimizing idl GPU time and reacting to increasing traffic directly impacts performance and cost. Karpenter’s dynamic, pod-driven design makes it a great fit for Atlassian’s Inference Engine.
  * [Istio](https://istio.io/) ingress routing to distribute traffic across GPU nodes. While lighter-weight alternatives like Linkerd and NGINX ingress exist, Istio gave us better control over retries, observability, and some more advanced routing approaches like splitting traffic and shadow deployments.
  * [KEDA](https://keda.sh/) – enables austoscaling based on custom metrics (queue duration, latency, GPU utilization). Compared to HPA or custom cron-based scale-ips, KEDA gave us a clean integration path with Prometheus metrics.
  * [Prometheus](https://prometheus.io/) + SignalFx for fine-grained observability into latency, GPU saturation, batching health, and request failures.


This setup lets us run resilient inference, tightly optimized for cost and performance.
## Results
Today, Atlassian’s Inference Engine:
  * Serves inference for critical production workloads daily
  * Powers models across the Rovo AI suite including search, chat, and safety
  * Replaced vendor inference with custom infrastructure and full observability
  * Enables rapid iteration, self-service model deployment, and regionally optimized scaling
  * Observability and monitoring system that gives us insight into hardware and inference analytics to tune for performance.


We monitor and alert on:
  * Latency tracked per model
  * Throughput to detect saturation or drift
  * GPU utilization and memory pressure to guide autoscaling and capacity planning
  * Model-level error rates and inference failures
  * Keda autoscaling behavior and queue depth, queue time is a common scaling factor for our deployments.
  * Batching efficiency and request queue lag to prevent throughput regressions


These metrics are surfaced through Prometheus, and we alert on the autoscaling, pod disruptions, deployment interruptions, and subnet saturation.
## What’s Next
Inference at Atlassian is an evolving discipline, and we’re just getting started. Looking ahead, our focus is on improved smart scaling, deeper optimizations, and supporting an even broader range of AI use cases across products.
We’re continuing to innovate and invest in:
  * Improving runtime efficiency and cost-awareness across workloads
  * Expanding support for emerging model architectures and modalities
  * Enhancing observability and automation in our inference pipeline
  * Strengthening deployment flexibility across regions and products


##### About This Article
[Jordan Leventis](https://www.atlassian.com/blog/author/jordan-leventis "Posts by Jordan Leventis")
Machine Learning Systems Engineer, Atlassian
[Matt Turner](https://www.atlassian.com/blog/author/matt-turner "Posts by Matt Turner")
Staff Software Engineer, Atlassian
[Eduardo Cirilo](https://www.atlassian.com/blog/author/eduardo-cirilo "Posts by Eduardo Cirilo")
Senior Software/AI Engineer, Atlassian
Principal Machine Learning Engineer
[Damien Jose](https://www.atlassian.com/blog/author/damien-jose "Posts by Damien Jose")
Sr. AI/ML Manager, Atlassian
Senior Principal TPM, Atlassian
  * [Share on Facebook (Opens in new window)Facebook](https://www.atlassian.com/blog/how-we-build/inference-engine?share=facebook&nb=1)
  * [Share on X (Opens in new window)X](https://www.atlassian.com/blog/how-we-build/inference-engine?share=x&nb=1)
  * [Share on LinkedIn (Opens in new window)LinkedIn](https://www.atlassian.com/blog/how-we-build/inference-engine?share=linkedin&nb=1)
  * Share on Mail (Opens in new window)Mail


Subscribe for more
## Inside **Atlassian**
### Get insights like this in your inbox
Sign up now
Dismiss
### Subscribe to our newsletter
Research and insights on how teams can deliver results with AI.
Thank you!
Your time is valuable. We promise to send only what’s actually worth reading.
## Related content
Article in 
### Build AI-powered Forge apps with Atlassian-hosted LLMs
LLMs have become a core tool in every app developer’s stack, powering features that feel almost magical. Users can query data with natural language, summarize long issue threads, and get smart, contextual suggestions, in an instant. Just a few years ago, these features would have been cumbersome or even impossible for developers to build. AI […]
Article in 
### Atlassian Research: What Do Developers Think About Code Readability in the Age of LLMs?
As LLM-powered agents like Atlassian’s RovoDev start writing more of our code, a timely question emerges: Does code readability still matter when RovoDev Agents is doing the heavy lifting?
Article in 
### Atlassian’s AI Gateway: Best in Class Model Garden
LLMs represent a rapidly advancing area of technology, with various foundation model providers competing to build the leading solution. Like any technology, the differing models often have a spectrum of their own strengths and weaknesses. Atlassian currently partners with an array of leading proprietary model providers and open-source technologies, incorporating models from OpenAI, Anthropic, Google, […]
## Manage Preferences
When you visit any website, it may store or retrieve information on your browser, mostly in the form of cookies. This information might be about you, your preferences or your device and is mostly used to make the site work as you expect it to. The information does not usually directly identify you, but it can give you a more personalized web experience. Because we respect your right to privacy, you can choose not to allow some types of cookies. Click on the different category headings to find out more and change our default settings. However, blocking some types of cookies may impact your experience of the site and the services we are able to offer. [More information](https://www.atlassian.com/legal/cookies%20)
Accept all
#### Strictly Necessary Cookies
Always Active
These cookies are necessary for the website to function and cannot be switched off in our systems. They are usually only set in response to actions made by you which amount to a request for services, such as setting your privacy preferences, logging in or filling in forms. You can set your browser to block or alert you about these cookies, but some parts of the site will not then work. These cookies do not store any personally identifiable information.
#### Targeting Cookies
Always Active
These cookies may be set through our site by our advertising partners. They may be used by those companies to build a profile of your interests and show you relevant adverts on other sites. They are based on uniquely identifying your browser and internet device. If you do not allow these cookies, you will experience less targeted advertising.
#### Functional Cookies
Always Active
These cookies enable the website to provide enhanced functionality and personalisation. They may be set by us or by third party providers whose services we have added to our pages. If you do not allow these cookies then some or all of these services may not function properly.
#### Performance Cookies
Always Active
These cookies allow us to count visits and traffic sources so we can measure and improve the performance of our site. They help us to know which pages are the most and least popular and see how visitors move around the site. If you do not allow these cookies we will not know when you have visited our site, and will not be able to monitor its performance.
Back Button
### Cookie List
Search Icon
Filter Icon
Clear
checkbox label label
Apply Cancel
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Reject all Confirm my choices
