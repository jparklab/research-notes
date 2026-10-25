---
title: 'Atlassian’s AI Gateway: Best in Class Model Garden - Inside Atlassian'
id: atlassians-ai-gateway-best-in-class-model-garden-inside-atlassian
tags:
- internal-llm-inference-stacks
- atlassian
- ai-gateway
- vendor-routing
- first-person-account
created: '2026-10-09T23:00:19.332914Z'
updated: '2026-10-10T14:52:17.148191Z'
source: https://www.atlassian.com/blog/atlassian-engineering/ai-gateway-model-garden
source_domain: www.atlassian.com
fetched_at: '2026-10-09T23:00:19.314008Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Atlassian engineering blog (Aug 22 2025), companion piece to the Inference
  Engine post, describing a separate component: the AI Gateway, a vendor-abstraction/routing
  layer (distinct from the self-hosted Inference Engine) that gives a single consistent
  API contract across third-party model providers (OpenAI, Anthropic, Google, Meta)
  plus Atlassian-hosted models. Built in-house by the AI & ML Platform team, with
  an accompanying SDK and CLI for integration. Curates a ''model garden'' of 20+ LLMs/audio/image
  models from 5+ providers, 10+ model families; used in 100+ use cases across 8+ Atlassian
  apps and 40+ teams. Key custom capabilities stated: automated fallback to alternative
  models/vendors on provider outage/incident (explicitly credited with mitigating
  multiple incidents); centralized rate limiting, quota management, and cost tracking
  across teams to avoid a ''noisy neighbor effect''; per-model status tracking (alpha/preview/prod)
  gating production readiness; centralized security/compliance/privacy enforcement
  and routing restricted to approved endpoints. States zero data retention from third-party
  hosted LLM providers. Concrete deployment example: Atlassian was an early GPT-5
  tester via the Gateway, provisioning production GPT-5 capacity for Rovo Dev on the
  model''s public launch day. Does not describe the self-hosted inference engine/serving
  stack (that is covered in the separate Inference Engine post) -- this piece is about
  multi-vendor routing/governance, not GPU serving infrastructure.'
---

*Suggested by [[atlassians-inference-engine-our-self-hosted-ai-inference-service-inside-atlassia]] — companion first-person article on Atlassian's AI Gateway / model garden, linked as related content*

[Skip to content](https://www.atlassian.com/blog/atlassian-engineering/ai-gateway-model-garden#wp--skip-link--target)
# Atlassian’s AI Gateway: Best in Class Model Garden
August 22, 2025
Published In [How We Build](https://www.atlassian.com/blog/search?topic%5B0%5D=how-we-build)
LLMs represent a rapidly advancing area of technology, with various foundation model providers competing to build the leading solution. Like any technology, the differing models often have a spectrum of their own strengths and weaknesses.
Atlassian currently partners with an array of leading proprietary model providers and open-source technologies, incorporating models from OpenAI, Anthropic, Google, Meta, and others to harness the best from each.
To operate at scale, Atlassian’s AI and ML Platform team has built our own “AI Gateway” to help support our developers in having convenient abstraction access and control to the latest models, alongside the AI Gateway Software Development Kit and Command Line Interface to provide a battle-tested integration path and convenient management tools for our AI developers across the company, all while providing security and reliability for our customers.
The Atlassian AI Gateway offers Atlassian Developers a curated model garden of 20+ LLMs, audio and image models from 5+ providers, and 10+ model families. It’s been very successful for Atlassian, with 100+ use cases and experiments built across 8+ Atlassian apps, 40+ Atlassian teams, and our internal platform.
As always, all of the LLMs used in Atlassian’s apps have zero data retention. Our third-party hosted LLM providers won’t store your inputs and outputs for any purpose.
### **The Atlassian Trust Center**
Discover how the [Atlassian Trust Center](https://www.atlassian.com/trust/atlassian-intelligence) empowers you with transparency, control, and industry-leading security for your organization’s data.
# Core Functionalities of AI Gateway: Atlassian’s AI Gateway
**Unified Access and Agility**
  * **Integration flexibility:** Developers interact with multiple LLM vendors (e.g., OpenAI, Gemini) using a single, consistent API contract. This simplifies prompt management and makes it easy to switch models or vendors with minimal code changes and run A/B tests. Centralized routing has also made it easier for us to test how each feature works with our new option for AI Features to exclusively use Atlassian Hosted Models.
  * **Shared tooling:** This eliminates the need for each team to write and maintain its own integration code, saving time and reducing errors.
  * **Deep integration with our AI platform:** The AI Evaluation suite and ML Studio, our in-house training platform, use the AI Gateway for synthetic data generation and testing LLMs, and Atlassian’s Prompt Registry is integrated to allow rapid iteration for our engineers.


**Reliability and Resiliency**
  * **Automated fallback:** LLMs are a very new technology, and because of this, they can be unreliable. AI APIs, unfortunately, have frequent incidents and performance degradations. One of the cutting-edge capabilities we’ve implemented in AI Gateway is automatically resolving incidents by falling back to alternative models or vendors when one vendor is down.


**Security, Privacy, and Compliance**
  * **Centralizes enforcement:** The Gateway embeds privacy, security, and compliance controls, providing granular access controls, routing to only approved endpoints, and ensuring policies are consistently applied across features.
  * **Model Status:** AI Gateway tracks the status of each model, whether in alpha, preview, or prod. Atlassian developers know if a model is available via AI Gateway and marked as ready for production—both the infrastructure/capacity and compliance checks have been done to make it ready for use in our apps.


#### **Cost, Resource, and Operational Management**
  * **Centrally managed capacity:** The AI Platform centrally manages capacity across our model vendors and internal hosting, ensuring we have the correct amount of resources provisioned and benefiting from pooling all of our usage, minimizing cost.
  * **Quota management:** To handle all of our use cases sharing resources, AI Gateway implements robust rate limiting, quota management, and cost tracking to prevent overuse and manage spend across dozens of teams, avoiding the “noisy neighbor effect.”
  * **Best practices built in:** The AI Gateway SDK implements resilience mechanisms like retries, backoff strategies, and error handling out of the box, ensuring robust and reliable client interactions.


# **How the AI Gateway Benefits Atlassian Apps**
**AI Gateway streamlines model releases**
We use AI Gateway’s centralized controls to manage our model release. Atlassian collaborated with OpenAI as an early tester for GPT-5, first releasing ‘alpha’ access via AI Gateway to key use cases before launch. We were able to provision and prepare production GPT-5 capacity for our developers the day of the launch—Rovo Dev rolled out GPT-5 to users on launch day!
**Operational Excellence**
We’ve seen significant uptime improvements and resilience to incidents—multiple incidents successfully mitigated due to our central detectors and automated fallback. Our on-call engineers can sleep easily, and our customers can continue to use Rovo seamlessly!
**Cutting-edge testing and learning**
We often offer cutting-edge open-source models through our AI Gateway. Our Rovo team frequently evaluates agents against all model providers and finds the best model for each use case. This allows us to experiment and improve the products quickly with minimal effort and learn about new AI innovations through experimental models before productionizing them.
This has translated into Atlassian using a diverse range of models to provide the best experience:
  * Google’s Gemini models power our Editor AI for blazing-fast contextual edits in your Confluence documents.
  * Open-source text-to-speech models transcribe your Loom videos for streamlined search and summaries.
  * GPT-5 models are combined with our advanced coding orchestration to deliver a state-of-the-art coding agent in Rovo Dev Agents.
  * We use small, blazing-fast open-source models from Meta to understand Rovo’s query and review AI inputs to apply Atlassian’s Acceptable Use Policy to user requests.
  * The thinking capabilities of Anthropic’s Claude 4 models build a research plan to power Rovo’s Deep Research.


### **Our approach to AI is ever-evolving for the best possible experience**
We continuously refine and adapt our model selections, transitioning between models in accordance with the most recent performance metrics, all conducted behind the scenes.
Learn more about how our AI solutions work in our [Rovo Deep Research](https://www.atlassian.com/blog/announcements/rovo-deep-research) and [Hybrid LLM](https://www.atlassian.com/blog/atlassian-engineering/hybrid-llm) info blogs.
## At the forefront of enterprise-level AI solutions
AI Gateway provides a foundational capability for our developers to rapidly improve our apps and offer advanced capabilities to our millions of users around the world while providing our customers with the secure, thoughtful, and trusted approach they expect from Atlassian.
##### About This Article
[Gaurav Awadhwal](https://www.atlassian.com/blog/author/gaurav-awadhwal "Posts by Gaurav Awadhwal")
Head of Engineering, AI & ML Platform
  * [Share on Facebook (Opens in new window)Facebook](https://www.atlassian.com/blog/how-we-build/ai-gateway-model-garden?share=facebook&nb=1)
  * [Share on X (Opens in new window)X](https://www.atlassian.com/blog/how-we-build/ai-gateway-model-garden?share=x&nb=1)
  * [Share on LinkedIn (Opens in new window)LinkedIn](https://www.atlassian.com/blog/how-we-build/ai-gateway-model-garden?share=linkedin&nb=1)
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
### Forge LLMs API is now GA: Here’s how you can build AI-native apps on Atlassian
Today, we’re announcing the Forge LLMs API is now generally available for all developers. Large language models are now everywhere—embedded in the apps we use every day. They provide the intelligence that reduces manual effort, surfaces information, and helps us work more effectively. The Forge LLMs API lets any developer bring that intelligence directly into […]
Article in 
### Build AI-powered Forge apps with Atlassian-hosted LLMs
LLMs have become a core tool in every app developer’s stack, powering features that feel almost magical. Users can query data with natural language, summarize long issue threads, and get smart, contextual suggestions, in an instant. Just a few years ago, these features would have been cumbersome or even impossible for developers to build. AI […]
Article in 
### Atlassian’s Inference Engine, our self-hosted AI inference service
Powering Enterprise-scale AI As Atlassian’s AI capabilities continue to scale rapidly across multiple products, a pressing challenge emerged: how do we deliver world-class AI-powered solutions to millions of users without compromising on latency, flexibility, and operational control? The answer: Atlassian’s Inference Engine, our custom-built, self-hosted AI inference platform that now powers production LLMs, search models, […]
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
