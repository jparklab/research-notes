---
title: 'Navigating the LLM Landscape: Uber’s Innovation with GenAI Gateway'
id: navigating-the-llm-landscape-ubers-innovation-with-genai-gateway
tags:
- internal-llm-inference-stacks
- uber
- genai-gateway
- openai-api-compatibility
- first-person-account
created: '2026-10-09T22:52:47.401077Z'
updated: '2026-10-10T14:52:16.972947Z'
source: https://www.uber.com/blog/genai-gateway/
source_domain: www.uber.com
fetched_at: '2026-10-09T22:52:47.376659Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Uber Michelangelo team blog (Jul 11 2024 — outside the 18-24mo recency window,
  but the canonical first-person GenAI Gateway account) on the GenAI Gateway, a unified
  LLM access layer built because >60 disparate team-level LLM integration strategies
  had produced inefficiency/redundancy. Architecture: a Go service mirroring the OpenAI
  HTTP/JSON API shape, wrapping clients for third-party vendors (OpenAI, Vertex AI/PaLM2)
  plus Uber-hosted/in-house-served models — chosen specifically because OpenAI''s
  API had become the de facto interface for the OSS LLM ecosystem (LangChain, LlamaIndex),
  reducing onboarding friction and avoiding a proprietary interface going stale. For
  OpenAI integration they maintain an internal fork of the community Go client (github.com/sashabaranov/go-openai);
  for Vertex AI, finding no adequate Go implementation available at the time, they
  built and open-sourced their own (github.com/uber/go-vertex-ai), continuing to maintain
  it even after Google later published an official Prediction Client. For Uber-hosted
  LLMs they state they built ''a robust serving stack built upon the STOA (state-of-the-art)
  inference libraries'' but do not name the specific engine. In-house, explicitly
  custom-built components: the Go gateway service itself, a PII redactor/un-redactor
  (anonymizes categories like names/phone numbers with sequential placeholders before
  forwarding to third-party vendors, restores on response), auth/authz, metrics emission,
  and audit logging for cost attribution. Scale stated: ~30 customer teams, 16M queries/month,
  peak 25 QPS. Compared explicitly against Databricks'' MLflow AI Gateway, which Uber
  says uses a non-OpenAI-compatible syntax (create_route/query) and lacks Uber''s
  multi-language (Java/Go, not just Python) support.'
utility_score: 16.0
---

[Skip to main content](https://www.uber.com/us/en/blog/genai-gateway/#main)
July 11, 2024
# Navigating the LLM Landscape: Uber’s Innovation with GenAI Gateway
Tse-Chi Wang
Roopansh Bansal
Share this article
FacebookLinkedinX socialLink
# **Introduction**
Large Language Models (LLMs) have emerged as pivotal instruments in the tech industry, unlocking new avenues for innovation and progress across various sectors. At Uber, the impact of LLMs is particularly noticeable, with over 60 distinct use cases being identified in diverse domains, ranging from process automation to customer support and content generation. As teams at Uber embark on the journey of integrating LLMs into their products, several challenges have surfaced. Notably, the disparate integration strategies adopted by different teams have led to inefficiencies and redundant efforts.
To address these challenges and harness the growing demand for LLMs, Uber’s Michelangelo team has innovated a solution: the GenAI Gateway. The GenAI Gateway serves as a unified platform for all LLM use cases within Uber, offering seamless access to models from various vendors like OpenAI and Vertex AI, as well as Uber-hosted models, through a consistent and efficient interface. The GenAI Gateway is designed to simplify the integration process for teams looking to leverage LLMs in their projects. Its easy onboarding process reduces the effort required by teams, providing a clear and straightforward path to harness the power of LLMs. In addition, a standardized review process, managed by the Engineering Security team, reviews use cases against Uber’s data handling standard before use cases are granted access to the gateway. If testing is successful these projects go through our standard, cross-functional software development process. The centralized nature of the gateway also streamlines the management of usage and budgeting across various teams, promoting greater control and operational efficiency in the integration of LLMs across the company.
## **Design & Architecture**
A pivotal design decision was to mirror the HTTP/JSON interface of the OpenAI API. This strategic choice is rooted in OpenAI’s widespread adoption and thriving open-source ecosystem highlighted by libraries like LangChain and LlamaIndex. This alignment fosters seamless interoperability, ensuring GenAI Gateway’s compatibility with existing open-source tools and libraries while minimizing the need for adjustments. Given the rapid evolution of the open-source community, a proprietary interface would risk becoming quickly outdated. By aligning with OpenAI’s interface, GenAI Gateway stays in step with cutting-edge advancements. This approach not only streamlines the onboarding process for developers but also extends GenAI Gateway’s reach, allowing users to access LLMs from various vendors, like Vertex AI, through a familiar OpenAI API framework. See Figure 1 below for the high-level architecture diagram.
The following code snippets demonstrate how to use GenAI Gateway to access LLMs from different vendors with unified interface:
> **Answer from gpt-4:** The capital of the USA is Washington D.C.
> **Answer from chat-bison:** Washington, D.C. is the capital of the USA. 
> **Answer from llama-2-70b-chat-hf-0:** The capital of the United States of America is Washington, D.C.
As can be seen from above, developers write code as if they’re using native OpenAI client, while being able to access LLMs from different vendors.
Architecture-wise, GenAI Gateway is a Go service that acts as an encompassing layer around the clients for third-party vendors, complemented by the in-house serving stacks tailored for Uber’s LLMs. 
Our approach to integrating with OpenAI involved developing an internal fork of the Go client implementation sourced from [the GitHub repository](https://github.com/sashabaranov/go-openai). When it came to integrating Vertex AI, specifically for accessing PaLM2, we faced a challenge: the absence of a suitable Go implementation at that time. We took the lead in developing our version and subsequently [open-sourced](https://github.com/uber/go-vertex-ai) it, contributing to the broader community. We encourage community engagement, inviting contributions ranging from bug reports to feature requests. This library mainly focused on features like text generation, chat generation, and embeddings. At the time of writing this blog, Google has also published their [Vertex AI Prediction Client](https://pkg.go.dev/cloud.google.com/go/aiplatform@v1.58.0/apiv1#PredictionClient) but we will continue to support our library because of its ease of use.
For Uber-hostedLLMs, we’ve engineered a robust serving stack built upon the STOA inference libraries, to optimize performance and efficiency. This blend of external integration and internal innovation reflects our commitment to staying at the forefront of LLM technology and contributing to its evolving ecosystem.
Beyond its serving component, an integral facet of GenAI Gateway is the incorporation of a Personal Identifiable Information (PII) redactor. Numerous studies have underscored the susceptibility of LLMs to potential data breaches, presenting significant security concerns for Uber. In response, GenAI Gateway incorporates a PII redactor that anonymizes sensitive information within requests before forwarding them to third-party vendors. Upon receiving responses from these external LLMs, the redacted entities are restored through an un-redaction process. The goal of this redaction/un-redaction process is to minimize the risk of exposing sensitive data.
Complementing its core functionalities, GenAI Gateway incorporates additional components designed for authentication and authorization, metrics emission to facilitate reporting and alerting, and the generation of audit logs for comprehensive cost attribution, security audit purposes, quality evaluation, and so on. All these components are seamlessly integrated into Uber’s in-house ecosystem, ensuring a cohesive and synergistic integration that aligns with the organization’s broader technological framework. This strategic alignment underscores GenAI Gateway’s commitment to not only meeting immediate needs, but also seamlessly integrating with Uber’s established infrastructure for enhanced efficiency and compatibility.
Today, GenAI Gateway is used by close to 30 customer teams and serves 16 million queries per month, with a peak QPS of 25.
### **Comparison to similar offering**
Although Databricks recently introduced the [MLflow AI Gateway](https://www.databricks.com/blog/announcing-mlflow-ai-gateway) that shares several features with our GenAI Gateway, GenAI Gateway stands apart in key ways from the MLflow AI Gateway. Our GenAI Gateway closely mirrors OpenAI’s interface, offering benefits not found in the MLflow AI Gateway, which has adopted a unique syntax for LLM access (create_route and query). In addition to aligning with OpenAI’s interface, GenAI Gateway enables a consistent approach to data security and privacy across all use cases. Furthermore, our platform extends beyond Python, providing support for Java and Go, which are the primary programming languages used in Uber’s backend infrastructure. This multi-language support, combined with our focus on security and alignment with OpenAI’s familiar interface, underscores GenAI Gateway’s unique position in the realm of LLM platforms.
## **Challenges**
The aim is for this platform to emulate the performance and quality of the native OpenAI API so closely that the transition is imperceptible. In this section of our blog, we will delve into the challenges encountered in achieving this seamless integration, particularly through the lens of handling PII.
### **PII Redactor**
The PII redactor, while essential for privacy and security, introduces challenges to both latency and result quality. To understand how PII redactor introduces these challenges, we first need to understand how PII redaction works.
The PII redactor scans input data, identifying and replacing all instances of PII with anonymized placeholders. Its sophisticated algorithm adeptly recognizes a wide range of PII categories. Each type of PII is substituted with a unique placeholder–for example, names are converted to ANONYMIZED_NAME_, while phone numbers are changed to ANONYMIZED_PHONE_NUMBER_. To maintain distinctiveness, these placeholders are assigned sequential numbers, creating unique identifiers for each occurrence: the first name in a dataset is labeled as ANONYMIZED_NAME_0, followed by ANONYMIZED_NAME_1 for the second, and so forth. The following example illustrates this process in action:
**Original Text**
George Washington is the first president of the United States. Abraham Lincoln is known for his leadership during the Civil War and the Emancipation Proclamation.
**Anonymized Text**
ANONYMIZED_NAME_0 is the first president of the United States. ANONYMIZED_NAME_1 is known for his leadership during the Civil War and the Emancipation Proclamation.
The mapping of PII data to anonymized placeholders is used in the un-redaction process that restores PII data from anonymized placeholders back to its original form, before returning the result to users.
Depending on the location the PII data is in the input, the same word can be redacted to different anonymized placeholders as it will be appended with different sequential numbers:
**Original Text**
Abraham Lincoln is known for his leadership during the Civil War and the Emancipation Proclamation. George Washington is the first president of the United States.
**Anonymized Text**
ANONYMIZED_NAME_0 is known for his leadership during the Civil War and the Emancipation Proclamation. ANONYMIZED_NAME_1 is the first president of the United States.
Category
Written by
Tse-Chi Wang
Tse-Chi Wang is a Senior Software Engineer on Uber's AI Serving team based in Seattle. He’s one of the main contributors to GenAI Gateway.
Roopansh Bansal
Roopansh Bansal is a Senior Software Engineer in Uber’s Customer Obsession Organization, based out of San Francisco. He collaborated with Uber AI to evolve Gen AI Gateway into what it is today. He is the primary author of the Vertex AI Go Library.
Related Articles
6 articles
AI / ML
Engineering
[Scaling AI in Legal: Building Uber’s Redlining Agent](https://www.uber.com/us/en/blog/building-ubers-redlining-agent/)
October 8, 2026
AI / ML
Backend
Engineering
[Designing MCP Gateway Uber's MCP Management Platform](https://www.uber.com/us/en/blog/designing-mcp-gateway/)
October 1, 2026
AI / ML
Engineering
[Taming the ML Firehose: Scaling Feature Consistency](https://www.uber.com/us/en/blog/taming-ml-firehose/)
September 22, 2026
Backend
Engineering
[How Uber Protects Against Retry Storms](https://www.uber.com/us/en/blog/protecting-against-retry-storms/)
September 17, 2026
Backend
Engineering
[Large-Scale Automated Dependency Analysis Across Uber's Service Mesh](https://www.uber.com/us/en/blog/automated-dependency-analysis/)
September 15, 2026
Backend
Engineering
[Halving the Time: How Uber Eats Rebuilt Its Search Pipeline](https://www.uber.com/us/en/blog/uber-eats-search-pipeline/)
September 10, 2026
## Select your preferred language
[English, English](https://www.uber.com/us/en/blog/genai-gateway/)
  * ### Products
    * [Advertising Learn more about advertising on Uber. Reach consumers as they go anywhere and get anything.](https://www.uber.com/us/en/blog/advertising/)
    * [Earn Resources for driving and delivering with Uber](https://www.uber.com/us/en/blog/earn/)
    * [Ride Experiences and information for people on the move](https://www.uber.com/us/en/blog/ride/)
    * [Eat Ordering meals for delivery is just the beginning with Uber Eats](https://www.uber.com/us/en/blog/eat/)
    * [Merchants Putting stores within reach of a world of customers](https://www.uber.com/us/en/blog/merchants/)
    * [Business Transforming the way companies move and feed their people](https://www.uber.com/us/en/blog/business/)
    * [Freight Taking shipping logistics in a new direction](https://www.uber.com/us/en/blog/freight/)
    * [Health Moving care forward together with medical providers](https://www.uber.com/us/en/blog/health/)
    * [Higher Education Enhancing campus transportation](https://www.uber.com/us/en/blog/higher-education/)
    * [Transit Expanding the reach of public transportation](https://www.uber.com/us/en/blog/transit/)
  * ### Company
    * [Engineering The technology behind Uber Engineering](https://www.uber.com/us/en/blog/engineering/)
    * [Community support Doing the right thing for cities and communities globally](https://www.uber.com/us/en/blog/community-support/)
    * [Newsroom Uber news and updates in your country](https://www.uber.com/us/en/newsroom/)
    * [Uber.com Product, how-to, and policy content—and more](https://www.uber.com/)


## Select your preferred language
[English, English](https://www.uber.com/us/en/blog/genai-gateway/)
## [Drive & deliver](https://drivers.uber.com/)
## [Uber Eats](https://ubereats.com/login-redirect/)
## [Business](https://business.uber.com/)
## [Drive & deliver](https://drivers.uber.com/)
## [Uber Eats](https://ubereats.com/login-redirect/)
## [Uber for Business](https://business.uber.com/)
## [Manage account](https://account.uber.com/)
This website uses third party cookies to serve you relevant ads and provide personalization by remembering your location. You may opt out from these cookies by selecting the "Opt out" button below. If you have an Uber account, you may opt out of the "sale" or "sharing" of your data [here](https://privacy.uber.com/us).
Opt outGot it
