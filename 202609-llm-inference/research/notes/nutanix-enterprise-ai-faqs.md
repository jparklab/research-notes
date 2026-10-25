---
title: Nutanix Enterprise AI FAQs
id: nutanix-enterprise-ai-faqs
tags:
- onprem-llm-inference-stacks-859099
- vendor-onprem
- nutanix
created: '2026-10-23T17:21:46.359056Z'
updated: '2026-10-23T17:24:34.017997Z'
source: https://www.nutanix.com/products/nutanix-enterprise-ai/faq
source_domain: www.nutanix.com
fetched_at: '2026-10-23T17:21:46.323713Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'PROVENANCE: vendor FAQ (Nutanix), recommendation not deployment evidence.
  NAI = set of containers on any CNCF-certified Kubernetes (NKP, Rancher, bare metal,
  EKS/AKS/GKE, OpenShift, VCF); GUI to deploy LLMs from Hugging Face or NVIDIA NIM
  (NGC) or uploaded custom models; endpoint API tokens, RBAC, GPU/k8s monitoring,
  audit, claims dark-site/air-gapped operation. NVIDIA GPUs only; text GenAI models
  only (validated list as of 12 Nov 2024); NIM requires NVIDIA AI Enterprise license
  (90-day eval); no trial license; hardware not required by NAI itself. Licensing
  details deferred to Nutanix software options page (no public price). Replaces: model
  deployment UI/endpoint management layer, not the serving engine (NIM/vLLM-class
  runtimes inside).'
utility_score: 14.0
---

# Nutanix Enterprise AI FAQs
## Nutanix Enterprise AI Overview Questions and Answers 
###  What is Enterprise AI?
There are many “types of AI” today. These include AI types such as generative AI (GenAI) with visual, text, analytics, etc., computer vision, natural language processing (NLP), etc. Regardless of the type of AI, businesses need AI to align with their outcomes and strategy. **[Enterprise AI](https://www.nutanix.com/info/artificial-intelligence/enterprise-ai) is the focus of all types of AI aligned to business outcomes for resiliency, Day 2 operations, and security surrounding AI. **
The Nutanix GPT-in-a-Box solution is tailor-made for enterprise AI. Driven by a service-led approach, GPT-in-a-Box has evolved to include new products and features making it a leading answer for enabling enterprise AI with turnkey infrastructure and composable solutions on-premises or in the public cloud.
A key feature of GPT-in-a-Box 2.0 is the Nutanix Enterprise AI product that makes deploying, operating, and monitoring generative AI models (large language models, LLMs) and secure APIs for enterprise AI applications simple. Nutanix Enterprise AI is the first component of GPT-in-a-Box enterprise customers' leverage to see tangible results for their enterprise AI initiatives, with an ease of use that may substantially reduce or even eliminate the need to hire new talent, allowing customers to scale current IT resources into AI resources. 
###  What is Nutanix Enterprise AI?
Nutanix Enterprise AI (NAI) offers endpoint APIs for leading LLM providers, including NVIDIA NIM and HuggingFace, making it easy for organizations to deploy a wide range of Gen AI models on-premises or in the public cloud. NAI includes a simple UI, role-based access controls (RBAC), and the capability for untethered operations (dark site or air-gapped deployments) to simplify the operation, monitoring, and adaptation of AI models (LLMs) with enterprise resiliency, Day 2 operations, and compliance. 
###  What’s the difference between enterprise AI and Nutanix Enterprise AI?
Enterprise AI is a generic term for focusing AI-related strategies on business outcomes and values. Nutanix Enterprise AI is a product that helps simplify and accelerate enterprise AI deployment of key generative AI components for AI applications. 
###  What is the difference between inference, tuning, and training when it comes to AI?
Here are the technical details of the differences:
  * **AI Model Training:** AI model training is the initial and most comprehensive process of building a model from scratch
  * **AI Model Fine-Tuning:** Fine-tuning is the process of adapting a pre-trained model for a specific task or domain
  * **AI Model Inference:** Inference is the application of a trained or fine-tuned model to make predictions or draw conclusions from new data 


To make this easier to explain, [GenAI explains these using a cake-baking metaphor](https://www.perplexity.ai/search/what-is-the-difference-between-J4u6EfL9RLeGuVGvlk3.yw#1):
  * AI model training is baking the cake.
  * AI Model fine-tuning is decorating it.
  * AI Model inference is eating it!


###  What is the difference between generative AI and ‘regular’ AI?
These days, these terms may be used interchangeably, but their outcomes are different. [GenAI describes it](https://www.perplexity.ai/search/what-is-the-difference-between-5P6EKqmBSO6DSlgz_bu0aw#1) as the following:
Generative AI and traditional (predictive) AI differ fundamentally in capabilities, applications, and technologies. 
  * **Traditional AI focuses on performing specific tasks** like data analysis, predictions, and pattern recognition, utilizing rule-based systems and narrow machine learning algorithms. 
  * **Generative AI (GenAI) creates original content** —such as text, images, and music—using advanced deep learning techniques and large language models. While traditional AI is designed for narrow tasks and requires specialized knowledge, generative AI is more versatile and accessible, allowing for natural language interactions and the generation of novel outputs across various modalities. 


As the field evolves, these two approaches increasingly integrate to enhance overall AI functionality.
###  Does Nutanix Enterprise AI provide generative AI (GenAI) apps, LLMs, and endpoint APIs?
Nutanix Enterprise AI provides Nutanix-validated third-party LLMs and secure APIs to create endpoints for generative AI applications you provide.
Nutanix does not provide GenAI applications but has multiple AI partners that can help you. You can learn more about GenAI here: <https://www.nutanix.com/partners/technology-alliances?tag=nutanix:filters/solution-categories/ai>
###  How is Nutanix Enterprise AI different?
Nutanix Enterprise AI provides three key differentiators for inferencing:
  * **Makes IT Resources AI Resources**
    * Enterprise AI can be complex. From new technical requirements to vastly different concepts, it may feel like a whole new world. Nutanix Enterprise AI helps ease this transition by providing simple workflows to find, deploy, manage, and test generative AI models (LLMs) in minutes. Secure APIs are included to help make your enterprise AI apps work. Key metrics and monitoring help you understand gaps or problems as you ramp up operations.
  * **Simplify LLM (large language model) Deployment and Operations**
    * Many enterprise AI solutions have dedicated common line instructions that present a steep learning curve for implementing and operating core enterprise AI components. Nutanix Enterprise AI provides a simple, yet elegant, user interface that allows you to quickly ramp on the LLMs and secure APIs you create and manage. Paired with secure role-based access controls, creating a secure enterprise AI environment is a snap. Nutanix Enterprise AI integrates with Hugging Face and NVIDIA NIM out-of-the-box to deploy the Nutanix co-validated LLM you need. Not available? Simply upload your own AI model (LLM) for total control.
  * **Enables Choice for Enterprise AI**
    * What works for generative AI today may need to be updated quickly as new, better models are made available. The requirement to be able to adapt and shift demands the ability to choose, but without changing architectures or infrastructures. Nutanix Enterprise AI provides a choice of Kubernetes® environments, including the Nutanix Kubernetes Platform (NKP) solution, products from software partners (e.g. Rancher and Docker), bare metal, or hyperscaler container instances (e.g., Google Cloud, AWS, and Azure). When a new model is available, you can decide to wait for a co-validation of the new model from Nutanix, or upload it yourself and with ease provide a new LLM and API to your generative AI applications.
  * For a deeper dive, check out the diagram below for more considerations:


###  Who should consider Nutanix Enterprise AI?
Nutanix customers and prospective customers should consider the improvements that Nutanix Enterprise AI can bring to their AI use cases. 
For Nutanix Customers:
  * **Leverage your Nutanix infrastructure and investments right now** making GenAI with Nutanix Enterprise AI seamless to secure, manage, and deploy AI key components like your choice of AI models and APIs for GenAI apps.
  * For further simplicity, u**se the Nutanix Kubernetes Platform and deploy Nutanix Enterprise AI through its validated application catalog**. Nutanix Kubernetes Platform makes managing and scaling Kubernetes effortless from the edge to public clouds.


For prospective Nutanix Customers:
  * **Use Nutanix GPT-in-a-Box** - a turn-key solution with your choice of Nutanix-validated hardware platforms and a Nutanix-validated stack of software products ranging from infrastructure management to Kubernetes and Nutanix Enterprise AI, bundled with professional services.
  * **Bring-your-own-infrastructure** - Run Nutanix Enterprise AI on VMware by Broadcom* VCF, three-tier datacenter infrastructure with the Kubernetes you choose, or even on bare-metal Kubernetes servers.
  * **Leverage previous investments or a mix** - Already have GenAI investments you’re using? No problem. Nutanix products can be configured and leveraged in different ways to take advantage of your AI stack by giving you choices like:
    * **Using Red Hat OpenShift?** Deploy Nutanix Enterprise AI.
    * **Using public cloud Kubernetes for Google Cloud GKE, AWS EKS, or Azure AKS?** Deploy Nutanix Enterprise AI.
    * **Using NVIDIA AI Enterprise?** Deploy Nutanix Cloud Infrastructure and Nutanix Unified Storage and use Nutanix Enterprise AI to deploy NVIDIA NIM, part of the NVIDIA AI Enterprise software platform.
    * **Using open-source tools?** Deploy Nutanix Kubernetes Platform for easy Kubernetes management and Nutanix Enterprise AI.


* Nutanix, Inc. is not affiliated with VMware by Broadcom or Broadcom.
###  What is the difference between Nutanix Enterprise AI and GPT-in-a-Box?
Nutanix Enterprise AI is a product licensed by Nutanix. Nutanix GPT-in-a-Box (v1.0 and 2.0) is a validated Nutanix solution/offer for enterprise AI, including the Nutanix Enterprise AI product among other Nutanix products for infrastructure, data, security, virtualization, Kubernetes, and management. This Nutanix-validated stack allows you to build secure enterprise AI on private clouds, and public clouds, or to leverage both creating a hybrid multicloud.
## Nutanix Enterprise AI Product Questions and Answers 
###  How does Nutanix Enterprise AI work?
  1. Deploy and run Nutanix Enterprise AI on any CNCF-certified Kubernetes environment.
  2. Login to the simple interface and pick and deploy the LLM (large language model) of your choice.
  3. Create a secure API endpoint to access your model. Test it with a preflight query.
  4. Once verified, send the API credentials to your AI developers or application owners.


###  What are the key features of Nutanix Enterprise AI?
The following list outlines the most prominent components:
  * Elegant, Enterprise-Grade User Interface
  * Choice of AI Models (LLMs) from Hugging Face or NVIDIA NIM 
  * Upload Your Own AI Models (LLMs)
  * API Token Creation and Management
  * Partner API Token Management for Hugging Face and NVIDIA NIM
  * API Code Samples
  * Role-Based Access Controls (RBAC)
  * AI Model Preflight Testing
  * AI Model and API Monitoring
  * Kubernetes Resource Monitoring
  * GPU Usage Monitoring
  * Event Auditing
  * Integrated Nutanix Pulse Reporting 


###  What is an IEP?
This stands for ‘Inference Endpoint’ and is a common term for what Nutanix Enterprise AI does.
###  Which AI models (LLMs) are available with Nutanix Enterprise AI?
Currently, only text-based Generative AI models from Hugging Face and NVIDIA NIM via NVIDIA NGC are validated for Nutanix Enterprise AI. The validated models supported are listed in the product when choosing either option. There is also an option to upload your own, private model using whatever model you choose.
Below is the launch-day list of models in Nutanix Enterprise AI (as of Nov. 12, 2024)
###  Does Nutanix Enterprise AI have integration with Hugging Face?
Yes, includes Nutanix validated LLMs from Hugging Face using your own Hugging Face API token and your accepted model repositories. Learn more at <https://www.nutanix.com/products/nutanix-enterprise-ai>
###  How do I get models from Hugging Face?
A Hugging Face API token is required inside of Nutanix Enterprise AI to make API calls to Hugging Face. This can be created using the[ steps from Hugging Face](https://huggingface.co/docs/hub/en/security-tokens).
Depending on the model available on the Nutanix Enterprise AI list, it is likely steps must be taken to get access to models through a Hugging Face Gated Repository. A Gated Repository is configured by a model owner (e.g., Meta, Google, etc.) and must be followed in order to correctly download and gain access to a model. [The steps to do this are available from Hugging Face.](https://huggingface.co/docs/hub/models-gated)
###  Why doesn’t the list of models for Hugging Face reflect everything on their website?
Here is a list of which AI models (LLMs) are available with Nutanix Enterprise AI, but models can also be uploaded manually.
###  Does Nutanix Enterprise AI have integration with NVIDIA NIM?
Yes, using validated LLMs from the NVIDIA NGC catalog for NIM using your own [NVIDIA NIM API token](https://build.nvidia.com/explore/discover). Learn more at <https://www.nutanix.com/products/nutanix-enterprise-ai>
###  Does NVIDIA NIM require a license when used with Nutanix Enterprise AI?
[Developers can get access to NVIDIA NIM](https://build.nvidia.com/explore/discover) with credits, or NVIDIA NIM [can be licensed through NVIDIA as part of NVIDIA AI Enterprise,](https://resources.nvidia.com/en-us-nvaie/nvidia-ai-enterprise-licensing-pg) with an available [90-day license](https://enterpriseproductregistration.nvidia.com/?LicType=EVAL&ProductFamily=NVAIEnterprise&ncid=no-ncid).
###  What is NVIDIA NGC?
Nutanix Enterprise AI leverages models from NVIDIA NIM [through the NVIDIA NGC catalog](https://catalog.ngc.nvidia.com/?filters=&orderBy=weightPopularDESC&query=&page=&pageSize=). NGC is a catalog of GPU Accelerated AI models and SDKs that help you infuse AI into your applications.
###  Do NVIDIA GPUs require a license when used with Nutanix in general?
NVIDIA [provides NVIDIA AI Enterprise as a licensing mechanism](https://resources.nvidia.com/en-us-nvaie/nvidia-ai-enterprise-licensing-pg) for certain GPUs used for datacenter and AI operations, with some GPUs including NVIDIA AI Enterprise licensing for a period of time. 
###  How is Nutanix Enterprise AI licensed?
Details can be found on the [Nutanix software options web page](https://www.nutanix.com/products/cloud-platform/software-options). 
###  Is there a trial license for Nutanix Enterprise AI?
Not at this time, but Nutanix offers a free hosted proof-of-concept option using [nutanix.com/testdrive](http://nutanix.com/testdrive).
###  Is Nutanix Enterprise AI included with other Nutanix product licenses?
Yes, Nutanix Enterprise AI is bundled with other Nutanix products using the Nutanix Enterprise AI for GPT-in-a-Box 2.0 and Nutanix Enterprise AI for Bare Metal types. 
Details can be found on the [Nutanix software options web page](https://www.nutanix.com/products/cloud-platform/software-options)
###  If I have GPT-in-a-Box 1.0 today can I upgrade to GPT-in-a-Box 2.0 and receive access to Nutanix Enterprise AI?
No, there’s no direct upgrade, but you can license Nutanix Enterprise AI Stand-Alone.
GPT-in-a-Box 1.0 is a services-led offering and opinionated stack. GPT-in-a-Box 2.0 is a validated stack that now includes Nutanix Kubernetes Platform and Nutanix Enterprise AI.
###  Is hardware required to run Nutanix Enterprise AI?
No, but [you can choose compatible hardware using Nutanix Cloud Infrastructure ](https://portal.nutanix.com/page/documents/list?type=compatibilityList)(HCI) 
Nutanix Enterprise AI is a set of containers that can run on the Nutanix Kubernetes Platform, [any CNCF-certified (Cloud Native Computing Foundation) Kubernetes runtime](https://kubernetes.io/partners/), or Kubernetes in supported public clouds. (Amazon EKS or Azure AKS, as examples)
###  Which AI Accelerators (i.e., GPUs, CPUs, etc.) does Nutanix Enterprise AI support?
Nutanix currently supports NVIDIA GPUs.
###  What GPUs are supported for Nutanix Enterprise AI?
NVIDIA GPUs. 
Each hardware/server system supports different GPUs. Also, Kubernetes must be configured to leverage GPUs. Please check compatibility for GPUs via these locations: 
  * Enabling Kubernetes to run with GPUs: 
    * <https://www.run.ai/guides/multi-gpu/kubernetes-gpu>
  * Nutanix hardware/server GPUs:
    * <https://portal.nutanix.com/page/documents/compatibility-interoperability-matrix/nvidia-drivers>


###  What about CPUs? Is Intel AMX supported for Nutanix Enterprise AI?
Yes, Intel AMX is supported.
###  What other Nutanix products are typically used with Nutanix Enterprise AI?
Nutanix Enterprise AI for GPT-in-a-Box 2.0 provides a validated platform using other Nutanix products.
  * Nutanix Cloud Infrastructure 
  * Nutanix Kubernetes Platform
  * Nutanix Unified Storage


###  Does Nutanix have AI partners they recommend?
Yes. 
From infrastructure partners to LLMOps, and security, you can learn about these partners at:
<https://www.nutanix.com/partners/technology-alliances?tag=nutanix:filters/solution-categories/ai>
###  Are there services included to help deploy and operate Nutanix Enterprise AI?
No, but services can be added if desired.
Services for Nutanix Enterprise AI can be added separately if desired but are not mandatory. 
[More information can be found here:](https://www.nutanix.com/support-services/professional-services/infrastructure-modernization)
###  Where can Nutanix Enterprise AI be deployed?
  * On any environment with a [CNCF-certified (Cloud Native Computing Foundation) Kubernetes runtime](https://kubernetes.io/partners/)
  * We also have support for the following: 
    * Via Nutanix hyperconverged infrastructure (HCI) on any supported system that includes GPUs for AI acceleration through the AHV hypervisor
    * Via Nutanix Kubernetes Platform (NKP)
    * Via any supported hyperscaler using public cloud-based infrastructure-as-a-service. Currently, we support:
      * AWS EKS
      * Azure AKS
      * Google Cloud GKE
    * Via bare metal
    * Any dark site or air-gapped environment (non-internet connected) with CNCF-certified Kubernetes running


###  Does Nutanix Enterprise AI require Nutanix hyperconverged infrastructure (HCI)?
No.
Nutanix Enterprise AI is a set of containers that can be run on any [CNCF-certified (Cloud Native Computing Foundation) Kubernetes runtime](https://kubernetes.io/partners/), including public clouds.
###  Can Nutanix Enterprise AI be deployed in public clouds
Yes.
Nutanix Enterprise AI has been tested to support the following:
  * Google Cloud GKE
  * AWS EKS
  * Azure AKS


###  Why should I deploy Nutanix Enterprise AI in a public cloud?
Nutanix Enterprise AI helps streamline the development and deployment of LLMs (large language models) and APIs of generative AI applications.
Nutanix Enterprise AI can be run on any CNCF-certified Kubernetes environment making it simple to deploy on public clouds. Once deployed, other public cloud services can be used, like storage, applications, etc. This helps public-cloud-focused organizations to leverage their models and data in a secure location of their choice, on their VPCs and networks they control.
The value of deploying Nutanix Enterprise AI isn’t to compete with similar public cloud offerings, but to make testing and development of LLMs easier and faster. Once your LLMs have been validated by you, Nutanix Enterprise AI can be used in production, or a validated LLM can be handed off to a discrete LLM production workflow (LLMOps). 
For customers wanting private enterprise AI, Nutanix Enterprise AI can be used within public cloud VPCs or on-premises to create a private cloud using Nutanix hyperconverged infrastructure (HCI).
###  Can Nutanix Enterprise AI be run in a dark-site/air-gapped environment?
Yes. 
The dark site bundle is located on the Nutanix support portal requiring a login and license entitlement. 
###  Where can I find the technical documentation for Nutanix Enterprise AI?
Setup documentation can be found on the Nutanix Support Portal (note this link will be live when Nutanix Enterprise AI is released):
[https://portal.nutanix.com/page/documents/list?type=software&filterKey=software&filterVal=Nutanix%20Enterprise%20AI](https://portal.nutanix.com/page/documents/list?type=software&filterKey=software&filterVal=Nutanix%20Enterprise%20AI)
###  Does Nutanix Enterprise AI have a validated design or reference architecture?
Yes, a Nutanix Validated Design (NVD) is available based on the Nutanix GPT-in-a-Box solution:
  * <https://portal.nutanix.com/page/documents/nutanix-validated-design/list>


Cisco also provides a Cisco Validated Design (CVD) for this: 
  * <https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/cisco_nutanix_gpt.html>


###  Who are the 3rd-party vendors and partners that work with Nutanix Enterprise AI?
Major integrations include Hugging Face and NVIDIA NIM.
[Other 3rd party AI partners that can leverage Nutanix Enterprise AI can be found here.](https://www.nutanix.com/partners/technology-alliances?tag=nutanix:filters/solution-categories/ai)
©2024 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned herein are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. Nutanix, Inc. is not affiliated with VMware by Broadcom or Broadcom. VMware and the various VMware product names recited herein are registered or unregistered trademarks of Broadcom in the United States and/or other countries. Kubernetes is a registered trademark of The Linux Foundation in the United States and other countries. All other brand names mentioned herein are for identification purposes only and may be the trademarks of their respective holder(s).
Our decision to link to or reference an external site should not be considered an endorsement of any content on such a site. Certain information contained in this [post/presentation] may relate to, or be based on, studies, publications, surveys and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys and other data are reliable as of the date of this paper, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party.
This content may contain express and implied forward-looking statements, including, but not limited to, statements regarding our plans and expectations relating to new product features and technology that are under development, the capabilities of such product features and technology and our plans to release product features and technology in the future. Such statements are not historical facts and are instead based on our current expectations, estimates and beliefs. The accuracy of such statements involves risks and uncertainties and depends upon future events, including those that may be beyond our control, and actual results may differ materially and adversely from those anticipated or implied by such statements. Any forward-looking statements included herein speak only as of the date hereof and, except as required by law, we assume no obligation to update or otherwise revise any of such forward-looking statements to reflect subsequent events or circumstances. Any future product or product feature information is intended to outline general product directions, and is not a commitment, promise or legal obligation for Nutanix to deliver any functionality. This information should not be used when making a purchasing decision.
By clicking “Accept All Cookies”, you agree to the storing of cookies on your device to enhance site navigation, analyze site usage, and assist in our marketing efforts. [Nutanix Cookie Notice](https://www.nutanix.com/legal/cookie-notice)
Cookies Settings
Reject All Accept All Cookies
## Privacy Preference Center
## Privacy Preference Center
  * ### Your Privacy
  * ### Functional Cookies
  * ### Performance Cookies
  * ### Targeting Cookies
  * ### Strictly Necessary Cookies


#### Your Privacy
When you visit any website, it may store or retrieve information on your browser, mostly in the form of cookies. This information might be about you, your preferences, or your device, and is mostly used to make the site work as you expect. The information does not usually identify you directly, but it can give you a more personalized web experience. Because we respect your right to privacy, you can choose not to allow some types of cookies. Click on the different category headings to learn more and change our default settings. Blocking some types of cookies may impact your experience of the site and the services we are able to offer. [More information](https://cookiepedia.co.uk/giving-consent-to-cookies)
#### Functional Cookies
Functional Cookies
These cookies enable the website to provide enhanced functionality and personalisation. They may be set by us or by third party providers whose services we have added to our pages. If you do not allow these cookies then some or all of these services may not function properly.
#### Performance Cookies
Performance Cookies
These cookies allow us to count visits and traffic sources so we can measure and improve the performance of our site. They help us to know which pages are the most and least popular and see how visitors move around the site. All information these cookies collect is aggregated and therefore anonymous. If you do not allow these cookies we will not know when you have visited our site, and will not be able to monitor its performance.
#### Targeting Cookies
Targeting Cookies
These cookies may be set through our site by our advertising partners. They may be used by those companies to build a profile of your interests and show you relevant adverts on other sites. They do not store directly personal information, but are based on uniquely identifying your browser and internet device. If you do not allow these cookies, you will experience less targeted advertising.
#### Strictly Necessary Cookies
Always Active
These cookies are necessary for the website to function and cannot be switched off in our systems. They are usually only set in response to actions made by you which amount to a request for services, such as setting your privacy preferences, logging in or filling in forms. You can set your browser to block or alert you about these cookies, but some parts of the site will not then work. These cookies do not store any personally identifiable information.
Back Button
### Cookie List
Filter Button
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Clear
  * checkbox label label


Apply Cancel
Confirm My Choices
Reject All Allow All
