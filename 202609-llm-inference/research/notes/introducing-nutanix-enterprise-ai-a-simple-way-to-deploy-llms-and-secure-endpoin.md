---
title: Introducing Nutanix Enterprise AI, a Simple Way to Deploy LLMs and Secure Endpoints
  Anywhere
id: introducing-nutanix-enterprise-ai-a-simple-way-to-deploy-llms-and-secure-endpoin
tags:
- onprem-llm-inference-stacks-859099
- vendor-onprem
- nutanix
created: '2026-10-23T17:22:34.084715Z'
updated: '2026-10-23T17:24:34.262229Z'
source: https://www.nutanix.com/blog/introducing-nutanix-enterprise-ai
source_domain: www.nutanix.com
fetched_at: '2026-10-23T17:22:34.059884Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'PROVENANCE: vendor launch blog (Nutanix PMM), 12 Nov 2024, marketing. NAI
  GA as part of GPT-in-a-Box 2.0 with Nutanix Kubernetes Platform; deploys NIM/Hugging
  Face/custom LLMs behind secure endpoints; runs on EKS/AKS/GKE and on-prem; NVIDIA-certified.
  Note ambiguity: blog states air-gapped/edge cases ''will have to find other solutions''
  regarding public cloud services, while FAQ claims dark-site support.'
utility_score: 12.0
---

# Introducing Nutanix Enterprise AI, a Simple Way to Deploy LLMs and Secure Endpoints Anywhere 
_By Mike Barmonde, Product Marketing Manager, AI_
November 12, 2024 2:00 pm | 
11
min 
GenAI is everywhere, and with good reason. It is redefining business by unlocking massive productivity gains, supercharging customer experiences, and enhancing security. By not adopting GenAI, businesses risk falling behind in a market that emphasizes innovation. As organizations map their GenAI strategy, key challenges remain: Are infrastructure and personnel ready and enabled to deploy, operate, and monitor GenAI?
With Nutanix, the answer continues to be a resounding “yes!”.
## Announcing Nutanix Enterprise AI
We’re excited to announce our latest product: Nutanix Enterprise AI, now generally available.
[As revealed in the GPT-in-a-Box 2.0 announcement at .NEXT 2024,](https://www.nutanix.com/blog/gpt-in-a-box-2-is-here) Nutanix Enterprise AI provides an easy way to deploy your choice of [LLMs (large language models)](https://www.nutanix.com/info/artificial-intelligence/large-language-models) from leading LLM providers and create and manage secure APIs to connect your GenAI applications.
_Nutanix Enterprise AI is now part of the Nutanix GPT-in-a-Box 2.0 solution with Nutanix Kubernetes Platform._
The GPT-in-a-Box 2.0 solution provides a turn-key [enterprise AI](https://www.nutanix.com/info/artificial-intelligence/enterprise-ai) stack for your private cloud - at the edge or on-premises - that’s resilient and secure. [Nutanix Enterprise AI](https://www.nutanix.com/products/nutanix-enterprise-ai) supercharges Day-2 operations for your choice of LLMs for GPT-in-a-Box, or anywhere you run your Kubernetes® environment, even in public clouds.
_Nutanix GPT-in-a-Box 2.0 for enterprise AI at the edge or on-premises / private clouds.Nutanix Enterprise AI is also able to run on public cloud-managed Kubernetes.Certified to run on NVIDIA-Certified Systems and NVIDIA accelerated computing._
### Key benefits include:
  * **Transforming IT Resources into AI Resources:** With simple workflows and testing capabilities, even IT personnel learning to support GenAI applications can ramp up quickly.
  * **Simplifying LLM Deployment and Operations:** A simple user interface makes it easy to deploy, operate, and monitor LLMs and secure endpoints with point-and-click management for role-based access controls.
  * **Optimized and Validated for NVIDIA AI:** Nutanix Enterprise AI runs on NVIDIA accelerated instances on AWS, Azure, Google Cloud, and has also been certified with NVIDIA AI Enterprise software. This includes NVIDIA NIM inference microservices for popular foundation models. NVIDIA AI Blueprints, reference workflows for canonical generative AI use cases, can also be used with GPT-in-a-Box, expanding the potential supported use cases.
  * **Enabling Choice for Enterprise AI:** Nutanix Enterprise AI can be deployed on CNCF-certified Kubernetes platforms, including Nutanix Kubernetes Platform. Inside the product, integrations enable leveraging listed LLMs with NVIDIA NIM™, Hugging Face, or even providing your own custom LLM. 


_Nutanix Enterprise AI leverages your IT resources for AI, simplifies deployment, and enables AI choice._
But there’s more.
## Nutanix Enterprise AI Embraces Public Clouds
Nutanix Enterprise AI embraces the public cloud as a prime landing spot for organizations to leverage their models and data in a secure location of their choosing. Nutanix Enterprise AI is fully compatible with NVIDIA accelerated computing available on AWS, Microsoft Azure, and Google Cloud. Nutanix Enterprise AI can also be run on any public cloud where Kubernetes operates.
We’re also excited to announce the availability of Nutanix Enterprise AI on another public cloud, now with Google Cloud. At launch, Nutanix Enterprise AI will run on GKE (Google Kubernetes Engine), providing new capabilities to the Nutanix platform. These new possibilities open doors to run GenAI, your way, anywhere.
Many customers use the public cloud as a development and testing platform for GenAI activities. Nutanix Enterprise AI simplifies how an enterprise deploys and develops AI models making testing models and GenAI apps fast and efficient on their public cloud VPCs.
_Nutanix Enterprise AI can be deployed on cloud-managed Kubernetes services including Google Kubernetes Engine (GKE), Amazon Elastic Kubernetes Service (EKS), and Azure Kubernetes Service (AKS) to simplify GenAI strategies in public clouds. All are supported to run on NVIDIA accelerated computing._
Public cloud services aren’t available for GenAI workloads outside a public cloud. The edge, on-premises, or air-gapped use cases will have to find other solutions, but using Nutanix Enterprise AI can deliver uniform operations across a hybrid multicloud environment - increasing the viability of an enterprise AI strategy.
## NVIDIA NIM Takes a Front Seat
The release of Nutanix Enterprise AI includes NVIDIA NIM microservices at the front and center when deploying secure endpoints. But why does NVIDIA NIM matter for Nutanix Enterprise AI?
_NVIDIA NIM microservices make developing and deploying GenAI apps simple and easy with Nutanix Enterprise AI._
NVIDIA NIM is part of NVIDIA AI Enterprise, providing inferencing microservices for pre-trained and customized AI models across clouds, data centers, and workstations. When paired with Nutanix Enterprise AI’s simple interface and secure endpoints, the results are powerful: a complete and fast GenAI inference solution that can be deployed anywhere from the edge to public clouds. 
Choosing to include NVIDIA NIM microservices makes secure integrations a snap for AI applications, development frameworks, and workflows, including performance optimization of AI model inferencing for GPUs. NIM runs everywhere, on-premises and in the cloud, and can be deployed with Nutanix Enterprise AI on Nutanix GPT-in-a-Box, AWS, Google, and Azure. In addition to enabling portability of workflows across hybrid infrastructure, NIM provides lower operational costs with model runtimes that are continuously optimized for low latency and high throughput on super-efficient Nutanix hyperconvereged infrastructure.
## How Does Nutanix Enterprise AI Work? 
Nutanix Enterprise AI is a snap to use and can be described in four quick steps: 
_Nutanix Enterprise AI allows you to deploy, manage, and monitor an AI model (LLM) and secure endpoints in 4 easy steps_
###  **1. Choose an environment with Kubernetes and deploy Nutanix Enterprise AI.**
Nutanix Kubernetes Platform has out-of-the-box integration with Nutanix Enterprise AI, but you can manually deploy it using other Kubernetes environments like Google Cloud GKE, AWS EKS, and Microsoft Azure AKS. Kubernetes leverages GPUs that are currently required for deployment. _Take note: Kubernetes cluster health and critical statistics like GPU usage can be monitored in the Nutanix Enterprise AI dashboard within the ‘Infrastructure Summary’ as an administrator. Nutanix Enterprise AI can also run in ‘dark sites’ or air-gapped environments for secure LLMOps that require untethered operations._
_Nutanix Enterprise AI provides administrators with monitoring and auditing capabilities for crucial items like the Kubernetes infrastructure, GPU usage, top GenAI endpoints, and more_
### **2. Once deployed, create the administrative login and choose a large language model (LLM) to import.**
Non-admin users can also deploy models and create API endpoints using role-based access controls (RBAC) tied to each user. Nutanix Enterprise AI has integration with NVIDIA NIM (using the NVIDIA NGC catalog) and Hugging Face. You can also choose to upload your own, custom model. The model wizard will take you through the model storage process. _Take note: NVIDIA NIM and Hugging Face require API keys created on their respective platforms to use their models which API keys and downloaded models are then stored in Nutanix Enterprise AI._
_With Nutanix Enterprise AI you can deploy the AI models (LLMs) you choose, including Nutanix validated models from Hugging Face or NVIDIA NGC (part of NVIDIA NIM), or a custom model you import._
### **3. When a model is ‘Active’, a secure endpoint API can be created for that model/user using the endpoint wizard.**
Everything is streamlined from naming your endpoint to choosing models tied to your user. Creating the API is part of this process using a few mouse clicks. Once the API Key is available, you can use your favorite secrets manager to store/share it. _Take note: When creating an API, a single window provides the API key that must be documented at that moment and cannot be provided again for security reasons. Individual user accounts can be created to allow for discrete access to Nutanix Enterprise AI, allowing each user to deploy and manage only their models and APIs tied solely to them. An administrator can easily revoke and deactivate users' keys and models as needed._
_When creating a new user you can choose between an ‘admin’ and a regular user who will only have access to their own models and endpoints, whereas an admin will have access to all models and endpoints for all users._
### **4. Finally, test the deployed model using the new API with a query and hand off the validated API to an application owner/developer.**
Think of this as using your typical GenAI services - you ask a question, it queries the AI model and you receive a generated response. This validation ensures that the endpoint API is functional and the model works correctly. The API key can then be sent to a developer or a GenAI application owner to provide access to an AI model (LLM). _Take note: If a change is required to a model or API endpoint (i.e., a new model is requested or an API is revoked), both must be updated/removed and then recreated for an application (aka, the API key must change). This process takes only minutes and can be accomplished within a maintenance window._
_Quickly test a deployed model and endpoint within Nutanix Enterprise AI with a simple sample query, as shown here with NVIDIA NIM._
### And that’s it! 🎉🎉🎉
With Nutanix Enterprise AI, deploying a LLM and a secure API becomes a point-and-click operation without the overhead of learning a new command line or combing through complicated features. As enterprise AI rolls out, IT resources can quickly ramp up to become AI resources using Nutanix Enterprise AI. 
## Nutanix Enterprise AI Makes GenAI Simple - Start Today 
As enterprise AI continues to march into enterprise technology stacks, the ability to transition IT resources into AI resources, provide simple AI Day 2 operations (LLM operations), and be adaptable for the next big thing from GenAI isn’t just desired, it’s necessary. Nutanix GPT-in-a-Box 2.0, coupled with Nutanix Enterprise AI, becomes even more powerful as a validated stack, with streamlined operations, infrastructure, and services to get you started with deploying enterprise AI today.
So, what’s your next move?
  * Go to <https://www.nutanix.com/products/nutanix-enterprise-ai> to learn more and run through a simple click-through demo
  * Want more? Go to [nutanix.com/testdrive](http://nutanix.com/testdrive) and get hands-on with Nutanix Enterprise AI at your own pace
  * [Learn more about Getting Started with enterprise AI on Nutanix GPT-in-a-Box 2.0](https://www.nutanix.com/blog/gpt-in-a-box-2-is-here)


**About Nutanix**
Nutanix is a global leader in cloud software, offering organizations a single platform for running apps and data across clouds. With Nutanix, organizations can reduce complexity and simplify operations, freeing them to focus on their business outcomes. Building on its legacy as the pioneer of HCI, Nutanix is trusted by companies worldwide to power hybrid multicloud environments consistently, simply, and cost-effectively. Learn more at [www.nutanix.com](http://www.nutanix.com) or follow us on social media @nutanix.
© 2024 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo, and all Nutanix product and service names mentioned herein are registered trademarks or unregistered trademarks of Nutanix, Inc. (“Nutanix”) in the United States and other countries. Kubernetes® is a registered trademark of the Linux Foundation. Other brand names or marks mentioned herein are for identification purposes only and may be the trademarks of their respective holder(s). This blog is for informational purposes only and nothing herein constitutes a warranty or other binding commitment by Nutanix. This blog contains express and implied forward-looking statements, including but not limited to statements regarding our plans and expectations relating to new product features and technology that are under development, the capabilities of such product features and technology and our plans to release product features and technology in the future. Such statements are not historical facts and are instead based on our current expectations, estimates and beliefs. The accuracy of such statements involves risks and uncertainties and depends upon future events, including those that may be beyond our control, and actual results may differ materially and adversely from those anticipated or implied by such statements. Any forward-looking statements included herein speak only as of the date hereof and, except as required by law, we assume no obligation to update or otherwise revise any of such forward-looking statements to reflect subsequent events or circumstances. Any future product or product feature information is intended to outline general product directions, and is not a commitment, promise or legal obligation for Nutanix to deliver any functionality. This information should not be used when making a purchasing decision. Our decision to link to or reference an external site should not be considered an endorsement of any content on such a site. Certain information contained in this content may relate to, or be based on, studies, publications, surveys and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys and other data are reliable as of the date of this paper, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. 
## Related Articles
[ Nutanix Expands Partnership with AWS to Simplify Hybrid Cloud Adoption In a move to reshape the hybrid cloud landscape, Nutanix announced a strategic collaboration agreement with Amazon Web Services, Inc. (AWS) to accelerate cloud migration and provide options for customers managing workloads across hybrid cloud...
  * **Funnel Stage:** TOF - Awareness
  * **Key Play:** Modern Apps
  * **Use Cases:** AI ML, Cloud Native

November 7, 2024 Read Blog ](https://www.nutanix.com/blog/nutanix-expands-partnership-aws-simplify-hybrid-cloud-adoption)
[ Nutanix: The Platform for the Next 10 Years This month, as we ship the first version of NKP, our customers will have every piece in place to set themselves up for success over the next 10 years.
  * **Key Play:** Modern Apps
  * **Use Cases:** AI ML, Cloud Native

August 28, 2024 Read Blog ](https://www.nutanix.com/blog/nutanix-the-platform-for-the-next-10-years)
[ Platform engineering as a replacement for DevOps Platform Engineering Emerges as a Practical Evolution of DevOps for Modern IT Teams.
  * **Blog Category:** Thought leadership
  * **Executive Topics:** Cloud Native
  * **Funnel Stage:** TOF - Awareness
  * **Key Play:** Modern Apps
  * **Use Cases:** AI ML, Cloud Native

September 25, 2024 Read Blog ](https://www.nutanix.com/blog/platform-engineering-as-a-replacement-for-devops)
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
