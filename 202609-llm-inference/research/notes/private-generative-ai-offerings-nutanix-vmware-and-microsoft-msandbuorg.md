---
title: Private Generative AI Offerings - Nutanix, VMware and Microsoft - msandbu.org
id: private-generative-ai-offerings-nutanix-vmware-and-microsoft-msandbuorg
tags:
- onprem-llm-inference-stacks-859099
- vendor-onprem
- independent-comparison
created: '2026-10-23T17:21:35.557843Z'
updated: '2026-10-23T17:24:33.777466Z'
source: https://msandbu.org/private-generative-ai-offerings-nutanix-vmware-and-microsoft/
source_domain: msandbu.org
fetched_at: '2026-10-23T17:21:35.525969Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'PROVENANCE: independent practitioner blog (Marius Sandbu), Nov 2024, short
  opinion piece, not a deployment account. Compares VMware Private AI (requires VMware
  infra, with NVIDIA, vector store on PostgreSQL), Nutanix Enterprise AI (any CNCF-certified
  Kubernetes) and Microsoft Edge AI on Azure Local (early private preview). Argues
  missing enterprise piece is supported end-to-end stack (inference API + vector DB
  + model catalog + RBAC + monitoring); open-source alternatives are vLLM/Ollama.
  Key critique: vendor platforms must expose extension points (LLM Guard, Presidio,
  LoRA fine-tuning) or customers depend wholly on vendor roadmap.'
utility_score: 15.0
---

[ Skip to content](https://msandbu.org/private-generative-ai-offerings-nutanix-vmware-and-microsoft/#content)
It’s almost two years since ChatGPT was released to the public, democratizing Generative AI for all end users. Now, two years later, all the cloud providers have their own Generative AI services, models, vector databases, and various tools that support the entire ecosystem. However, we still don’t have a robust enterprise offering for building Generative AI services on private clouds or on-premises infrastructure.
Last year, VMware announced something they call **VMware Private AI** in partnership with NVIDIA, providing an end-to-end solution that combines Generative AI models with embedding services. That also includes vector store using PostgreSQL. 
A couple of weeks ago, Nutanix launched their new Generative AI service offering called Nutanix AI Enterprise,
and just last week at Ignite, Microsoft announced their initial offering called **Edge AI** , which is part of **Azure Local** and with some new edge services including Video AI Indexer and Local AI Search
Now, when we look at the different offerings, VMware’s solution is currently only supported if you have a VMware-based infrastructure. Microsoft’s offering is supported only if you’re running Azure Local, and it is still in the early private preview stages. Nutanix, however, has taken a different approach. As long as you have a CNCF-supported or CNCF-verified Kubernetes distribution, you can run their offering called **Nutanix Enterprise AI**. Now, of course, you aren’t restricted to using these three vendors. You can use open-source tools or services such as **vLLM** or **Ollama** , which can run on your own machine or any type of server. 
However, having a supported service from a vendor that provides an end-to-end solution—not just the inferencing API from a model but also a vector database, model collection, role-based access control, and monitoring capabilities—is the missing piece of the puzzle. You also need the flexibility to choose different models for different use cases. Imagine having an AI platform where you can combine various language models, maintain an agent ecosystem to which you can attach different models, and add integrations using GPT functions. I believe this will be the future of any enterprise AI platform: the ability to choose from multiple models, integrate with various data sources, and scale services up and down based on use cases. This could involve running an orchestrator, whether Kubernetes or a solution directly from a cloud provider. Additionally, the platform should enable integration with any type of unstructured data source, automatically embedding it into a vector store or vector database as part of the system.
Of course, the issue many of these platforms will face is their ability to extend, provide add-ons, and add integrations. For instance, if I want to use a specific open-source tool such as **LLM Guard** or **Microsoft Presidio** —both common tools I include in any generative AI service requiring enhanced security—this can become challenging. Let’s say I’m running Microsoft’s GenAI service on Azure Local and want to add additional security components. The only way this would be possible is if the Azure Local service offers an API I can integrate with, allowing me to add my own tools and build add-ons on top of the existing ecosystem. Otherwise, I would have to rely entirely on Microsoft to provide sufficient functionality within their core platform to meet my needs. This flexibility will be a crucial aspect of any private AI platform.
Any enterprise AI offering also needs an easy way to perform fine-tuning—both traditional fine-tuning, as supported by GPT models, and the ability to use **LoRA adapters** , which allow you to add datasets on top of existing language models. Additionally, there should be a simple method to incorporate unstructured data into a vector database and make it accessible through various web services and web portals. Some of these services might provide a user-friendly web UI or an API to build upon, but this would also require them to offer custom APIs that can be easily accessed and extended for further development.
Stay tuned, we are still in the early days of Private GenAI and we are still looking at an evolving ecosystem, so my guess that we will see many more vendors emerge in this ecosystem. 
### Share this:
  * [ Share on X (Opens in new window) X ](https://msandbu.org/private-generative-ai-offerings-nutanix-vmware-and-microsoft/?share=twitter&nb=1)
  * [ Share on Facebook (Opens in new window) Facebook ](https://msandbu.org/private-generative-ai-offerings-nutanix-vmware-and-microsoft/?share=facebook&nb=1)
  * [ Share on LinkedIn (Opens in new window) LinkedIn ](https://msandbu.org/private-generative-ai-offerings-nutanix-vmware-and-microsoft/?share=linkedin&nb=1)
  * [ Share on Reddit (Opens in new window) Reddit ](https://msandbu.org/private-generative-ai-offerings-nutanix-vmware-and-microsoft/?share=reddit&nb=1)


Scroll to Top
