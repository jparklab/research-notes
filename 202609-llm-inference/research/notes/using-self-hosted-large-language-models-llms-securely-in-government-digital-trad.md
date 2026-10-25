---
title: Using self-hosted Large Language Models (LLMs) securely in Government – Digital
  trade
id: using-self-hosted-large-language-models-llms-securely-in-government-digital-trad
tags:
- internal-llm-inference-stacks-7f0c90
- locus-under-documented-layers-audited-absence
- government
- self-hosted-llm
created: '2026-10-10T00:05:59.906454Z'
updated: '2026-10-10T14:52:17.263904Z'
source: https://digitaltrade.blog.gov.uk/2025/07/09/using-self-hosted-large-language-models-llms-securely-in-government/
source_domain: digitaltrade.blog.gov.uk
fetched_at: '2026-10-10T00:05:59.886942Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: policy
deprecated: false
summary: First-person account by a Data Scientist in the Department for Business and
  Trade (DBT), a UK central government department, describing DBT's deployment of
  self-hosted LLMs on an internal AWS SageMaker-based platform called Data Workspace.
  States DBT previously used Hugging Face open-source models only on CPU instances;
  moved to GPU-backed SageMaker (in a private VPC within Data Workspace, isolated
  from the public internet) to run larger models like Llama and Mistral for use cases
  including topic modelling on trade agreement text, structured extraction from long
  documents, and sentiment analysis of parliamentary debate records. An internal Python
  package was built so users could call SageMaker directly; an initial 15-user pilot
  ran with no critical issues. The post claims DBT is 'one of the first government
  departments to deploy self-hosted LLMs in this way' and frames ongoing work around
  cost-sharing infrastructure across teams and right-sizing GPU instances.
---

# Your choice regarding cookies on this site
We use cookies to optimise site functionality and give you the best possible experience.
I Accept CookiesI Do Not Accept CookiesSettings
[Skip to main content](https://digitaltrade.blog.gov.uk/2025/07/09/using-self-hosted-large-language-models-llms-securely-in-government/#content)
https://digitaltrade.blog.gov.uk/2025/07/09/using-self-hosted-large-language-models-llms-securely-in-government/
### Emily Lambert
As a Data Scientist in the Digital, Data and Technology (DDaT) team at the Department for Business and Trade (DBT), I’ve seen firsthand how artificial intelligence - particularly Large Language Models (LLMs) - can open new possibilities in the way government processes and analyses data.
LLMs are a type of AI that can understand and generate human-like text based on patterns in vast amounts of written language. Tools like ChatGPT and Claude are based on these models. LLMs are already transforming industry, helping businesses summarise documents, extract insights from unstructured data, or even write code.
Government departments routinely handle confidential and personal data. Therefore, using public AI tools that transmit data externally can post significant security and privacy risks. For example, they may inadvertently expose sensitive trade policy negotiations or personal data from businesses. To mitigate these risks and maintain full control over data handling, we’ve taken the important step of self-hosting LLMs within a secure, internal environment.
## **Why government needs AI**
AI, when used responsibly, can help us analyse large amounts of data efficiently, supporting better decision making and improving services. The Government Digital Service (GDS) have highlighted the need for public sector innovation using AI, alongside strict controls and ethical standards. As demand for services rises, government must evolve its digital capabilities to deliver more for less and AI is a key part of that transformation.
## Meeting user needs
Through user research, we found a growing demand across DBT for using powerful LLMs for analysis whilst maintaining a high-level of security. Some of the use cases we identified include:
  * performing topic modelling on free trade agreement articles
  * extracting structured data from long-form documents
  * analysing large volumes of policy consultation responses
  * conducting sentiment analysis on parliamentary debate records


Previously, some teams used open-source models via platforms like Hugging Face, but only with Central Processing Unit (CPU) instances. More powerful models, such as Meta’s Llama or Mistral, require high memory and processing speeds to handle complex language tasks. Therefore, Graphics Processing Unit (GPU) infrastructure is essential to support this type of analysis.
To meet this user need, we’ve been working to deploy self-hosted LLMs securely on our internal data platform, Data Workspace, using AWS SageMaker. Data Workspace ensures data is contained within our network. It is in isolation from the public internet and restricts access to authorised users to maintain a high-level standard of security. SageMaker uses open-source models, but spins resource within a private VPC (Virtual Private Cloud) as a part of the Data Workspace ecosystem. Deploying LLMs in this way is essential when dealing with sensitive information. SageMaker has enabled GPU usage, unlocking the ability to run advanced models quickly.
By engaging with users across multiple projects, we were able to select the most relevant models for a pilot deployment; this has enabled us to share resources among projects, optimising our costs and return on investment.
In response to needs identified in user research, we developed an internal python package to help users interact with SageMaker directly within our AWS environment. Our initial trial of 15 users has gone successfully, with no critical issues, strong engagement and positive feedback.
We believe DBT is one of the first government departments to deploy self-hosted LLMs in this way. This marks an important step in how government can responsibly adopt cutting-edge AI technologies while maintaining strict control over data.
## **Ongoing development and responsible scaling**
One of the biggest challenges in deploying LLMs is cost. Running these models requires powerful GPU hardware, which can be expensive – especially as usage increases. However, by deploying LLMs within Data Workspace, we’re able to share infrastructure across multiple teams and projects. This centralised approach significantly reduces duplication and helps to ensure we’re using resources efficiently.
We’re taking a holistic approach to self-hosted LLMs – ensuring models we deploy are both technically appropriate and offer clear value for money. We’re also experimenting with running larger models on smaller instances where possible, assessing the performance trade-offs to maximise efficiency. Additionally, we’re actively monitoring model usage patterns to aid in optimising request flows. These combined efforts will help us scale the use of LLMs responsibly while staying mindful of costs.
We’re continuing to work with users to improve the experience, expand access, and further develop the product in line with departmental needs. This is only the beginning, but it's a significant milestone for how AI can be securely and effectively used within government.
Learn more about how DBT works with AI by reading this blog about [understanding the evaluations role in measuring the impact of AI interventions across Government](https://digitaltrade.blog.gov.uk/2025/04/14/understanding-the-evaluations-role-in-measuring-the-impact-of-ai-interventions-across-government/).
**Tags:** [ai](https://digitaltrade.blog.gov.uk/tag/ai/), [cybersecurity](https://digitaltrade.blog.gov.uk/tag/cybersecurity/), [data](https://digitaltrade.blog.gov.uk/tag/data/), [digital](https://digitaltrade.blog.gov.uk/tag/digital/), [Large Language Models](https://digitaltrade.blog.gov.uk/tag/large-language-models/), [user-centred design](https://digitaltrade.blog.gov.uk/tag/user-centred-design/)
## Sharing and comments
### Share this page


### 1 comment
  1. Comment by Richard Boddington posted on  15 July 2025
Very interesting read. Public Sector will obviously need to tap into the potential benefits of AI, however the risks around data sovereignty and security, aligned to public trust are a significant barrier. 
Through deploying private AI, and working with vendors who can support such models, these risks can be addressed alongside ensuring that the LLMs are specific to the use cases and can deliver more value.


## Related content and links
### Digital and Data at BIST
The Digital and Data team at BIST creates the tools and services that enable businesses in the UK and overseas to prosper in the global economy. 
[Find out more.](https://digitaltrade.blog.gov.uk/about-the-department-for-international-trade/)
### Categories
### Join the team
[ Check our latest job opportunities](https://www.civilservicejobs.service.gov.uk/csr/index.cgi?SID=b3duZXJ0eXBlPWZhaXImcGFnZWFjdGlvbj1zZWFyY2hjb250ZXh0Jm93bmVyPTUwNzAwMDAmY29udGV4dGlkPTE4ODcyMDY3MiZwYWdlY2xhc3M9U2VhcmNoJnJlcXNpZz0xNzc4NzUyMzYxLTYzM2NjMmFjOTgwYjZkZWQ0ZmEyOTNlN2RhNDI2ZmRhZTU0Zjk4ZTM=) in development, data, user-centred design, product and delivery.
### Follow us
### Sign up and manage updates


