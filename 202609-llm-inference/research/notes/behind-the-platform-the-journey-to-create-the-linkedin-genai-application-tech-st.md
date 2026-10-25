---
title: 'Behind the platform: the journey to create the LinkedIn GenAI application
  tech stack'
id: behind-the-platform-the-journey-to-create-the-linkedin-genai-application-tech-st
tags:
- inference-stack
- vllm
- langchain
created: '2026-10-09T22:59:51.885722Z'
updated: '2026-10-10T14:52:17.139092Z'
source: https://www.linkedin.com/blog/engineering/generative-ai/behind-the-platform-the-journey-to-create-the-linkedin-genai-application-tech-stack
source_domain: www.linkedin.com
fetched_at: '2026-10-09T22:59:51.866000Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Primary first-person LinkedIn engineering blog (Nov 26, 2024; authors Karthik
  R., Xiaofeng Wang, Sandeep Jha) on LinkedIn''s GenAI application tech stack, confirming
  and sourcing the ZenML database summary. Verbatim: LinkedIn''s ''GenAI application
  framework is now a thin wrapper atop LangChain, bridging it with LinkedIn infrastructure
  for logging, instrumentation, storage access and more. It is vended as a versioned
  internal library that is now mandated for use by all new GenAI applications at LinkedIn.''
  The Java-vs-Python fragmentation was deliberate and temporary: ''Rather than be
  blocked by the onerous task of rebuilding this tooling in Java or getting our online
  serving stack to support Python, we made a conscious call to start with fragmented
  online and offline stacks.'' Python infra investment was selective -- e.g., they
  skipped building a native Python client for their Espresso document store and instead
  reused an existing REST proxy, and built Python RPC support only for the emerging
  gRPC transition target, not the legacy rest.li. Prompt management runs on a Jinja-templated
  ''Prompt Source of Truth'' component (originally a Java resolution library, later
  rewritten in Python once the LangChain framework existed), converging on the OpenAI
  Chat Completions message format. Task automation uses a ''Skill Inversion'' pattern
  -- a centralized skill registry service plus build plugins that auto-register downstream
  API endpoints as LLM-callable skills (JSON schemas compatible with LangChain''s
  tool API) -- replacing an earlier approach where each GenAI product hand-wrote and
  duplicated skill wrappers. Conversational memory reuses LinkedIn''s production messaging
  stack via the LangChain Conversational Memory abstraction rather than a new vector-store
  build. Model inference: initial GenAI apps used LLMs ''provided by the [Azure OpenAI
  service]'' (word redacted/garbled in the scraped HTML but context and the ZenML
  summary confirm Azure OpenAI) behind a centralized GenAI proxy (Trust/Responsible-AI
  checks, streaming, quota management); increasingly, inference/fine-tuning depends
  on LinkedIn''s internal AI platform ''built atop open source frameworks like PyTorch,
  DeepSpeed and vLLM'' -- confirms vLLM by name as the open-source inference engine
  underneath LinkedIn''s in-house serving layer. All LLMs (internal and external)
  are exposed behind a single OpenAI Chat Completions-compatible API with configuration-driven
  routing between on-prem and external models. Migration from the legacy Java stack
  proceeded incrementally, component-by-component, with a lean team pairing senior
  Java engineers with Python engineers.'
---

*Suggested by [[linkedin-building-and-evolving-a-production-genai-application-stack-zenml-llmops]] — primary first-person LinkedIn engineering blog post that the ZenML summary paraphrases*

[ Skip to main content ](https://www.linkedin.com/blog/engineering/generative-ai/behind-the-platform-the-journey-to-create-the-linkedin-genai-application-tech-stack#lithograph-app)
[Generative AI](https://www.linkedin.com/blog/engineering/generative-ai)
# Behind the platform: the journey to create the LinkedIn GenAI application tech stack
[Authored byKarthik R.](https://www.linkedin.com/in/karthikramgopal)
November 26, 2024 
Co-authors: [Co-authored byKarthik R.](https://www.linkedin.com/in/karthikramgopal), [Co-authored byXiaofeng Wang](https://www.linkedin.com/in/wang-xiaofeng), and [Co-authored bySandeep Jha](https://www.linkedin.com/in/sandeepkumarjha)
In early 2023, we started rolling out a completely reimagined product portfolio, including for the first time features that leveraged Generative AI (GenAI). These GenAI features have enabled our members and customers to work more efficiently, using tools such as [collaborative articles](https://www.linkedin.com/blog/engineering/generative-ai/upleveling-collaborative-articles-match-game), [AI-assisted Recruiter](https://www.linkedin.com/business/talent/blog/talent-acquisition/reimagining-hiring-and-learning-with-power-of-ai), [AI-powered insights](https://www.linkedin.com/blog/engineering/generative-ai/musings-on-building-a-generative-ai-product), and most recently, our first AI agent, [Hiring Assistant](https://www.linkedin.com/business/talent/blog/talent-acquisition/introducing-hiring-assistant).
In the short period of time since the first GenAI feature made its way onto the platform, we’ve undergone remarkable change, learning and growth in our approach to building these unique product features and experiences. We went from launching products based on **simple “prompt in, string out” solutions** to crafting **assistive agent experiences that offered multi-turn conversation capabilities supported by advanced contextual memory**. In parallel, we gradually realized our early vision of building a GenAI application tech stack to power these products, which included **continuously optimizing between time to market goals and long-term leverage.**
This blog post will cover the work that happens below the surface of our AI-powered products and focus on our journey in building the GenAI application tech stack that enables them. As we delve deeper into various decisions around GenAI frameworks, programming languages, and the intricacies of bridging offline development with online deployment, you will get a peek into what it takes to build **AI-first products at LinkedIn scale**.
## Genesis and evolution
One of the earliest challenges we faced is not an uncommon one for any new technology being adopted rapidly at scale - building a shared technology foundation to maximize leverage. The solution manifested as a framework to act as the **hub of GenAI application development** at LinkedIn, providing standard mechanisms for common tasks like prompt construction, inference, memory access and more.
Since most of the LinkedIn online serving stack was programmed in Java, our initial pragmatic approach was to build a **shared Java midtier** for all GenAI products **encapsulating common functionality as built-in reusable Java code**. As the number of use cases grew, this midtier became a development and operational bottleneck, prompting us to split things up into multiple different use-case specific Java midtier services**.**
The **AI engineers working on the offline[Large Language Model (LLM)](https://en.wikipedia.org/wiki/Large_language_model)-based workflows, prompt engineering and evaluations preferred Python** given the plethora of **open source Python solutions** in this domain. Rather than be blocked by the onerous task of rebuilding this tooling in Java or getting our online serving stack to support Python, we made a conscious call to start with fragmented online and offline stacks with basic tooling to bridge them. 
Moving forward with the fragmented approach helped keep our momentum in the short-term, but we quickly faced challenges in maintaining that direction as we scaled. It required substantial effort toprevent divergence across the various Java midtier services and keep logic mirrored across the offline Python and online Java stacks. Maintaining all of this amidst active product development and updates to underlying framework versions proved taxing. 
As the **GenAI landscape and associated open source libraries continued to evolve primarily in Python** , it became clear that staying on Java for serving was a suboptimal long-term choice. We decided to **invest in Python as a first-class language for both offline iteration and online serving** at LinkedIn.
By this time, significant momentum had built around the open source project, including our own adoption of it for our offline stack. Our collaborative and productive relationship with the LangChain developers, and a deep analysis of areas like functionality, operability, open source community involvement, evolution track record, and future extensibility convinced us to use it for online serving as well.
Since LinkedIn had historically used Java almost exclusively for online serving, a lot of our online infrastructure for RPC, storage access, request context passing, distributed tracing and more, only had Java client implementations. Considering our emphasis on Python, we kicked off an initiative to**enable Python support for critical infrastructure dependencies** with few key principles in mind.
  * **Pragmatic prioritization** : While it would have been great to have Python equivalents of everything available in Java, doing so was rather expensive. We resorted to stack ranking requirements while coming up with creative solutions to bridge the gaps. For example, we implemented only a part of the request context spec in Python, because we didn’t need some functionality like bidirectional context passing for our GenAI applications, significantly reducing scope. We did not build a native Python client for [Espresso](https://engineering.linkedin.com/espresso/introducing-espresso-linkedins-hot-new-distributed-document-store) (our online distributed document store) and instead used an existing REST proxy to talk to it.
  * **Opportunistic alignment with migrations to future tech** : Rather than building support for Python clients for existing infrastructure, we looked at the state and adoption progress of upcoming tech, and opportunistically built support only for future tech. For example, LinkedIn is [transitioning RPCs from rest.li to grpc](https://www.infoq.com/news/2023/12/linkedin-grpc-protobuf-rest-json/); so rather than building support and closing gaps for several key RPC-related infrastructure pieces (such as request context passing, distributed tracing, call logging, service to service ACLs, etc.) atop both rest.li and gRPC, we built them only for gRPC.
  * **First-class developer experience:** We wanted the Python developer experience to feel native without any shortcuts affecting debuggability or operability. This has translated to investing in Python native builds (in lieu of our legacy grade based build system), re-engineering solutions that involve [Python code calling C/C++ code](https://docs.python.org/3/extending/extending.html) in parts that developers frequently debug to be fully Python native, tooling automation to ease importing open source Python libraries for use within LinkedIn, and investing in tooling and processes to ensure that we stay on a reasonably new version of the Python language and runtime.


Our **GenAI application framework is now a thin wrapper atop LangChain,** bridging it with LinkedIn infrastructure for logging, instrumentation, storage access and more. It is **vended as a versioned internal library** **that is now** **mandated for use by all new GenAI applications at LinkedIn**.
**Top Takeaway:** Given the relative recency and rapid evolution of GenAI, there is no one-size-fits-all formula for scalable product development. So engineering organizations - from practitioners to leaders - should make calculated framework investments that balance pragmatism and time to market with long-term leverage.
## Prompt management
Prompt engineering is core to building GenAI applications with prompts providing the primary mechanism to “program” Large Language Models (LLMs). Prompt management refers to the system and processes in place to manage and curate these prompts across GenAI applications. 
Our initial approach was **manual string interpolation in code**. While sufficient for basic use cases, this quickly proved error-prone and unscalable. 
As we considered more complex approaches to prompt engineering, we made some initial observations that were critical in shaping our ultimate path forward. 
  * We noticed many use cases **benefited from partial or full prompts** for shared functionality. Since **Trust and Responsible AI are core requirements for all our products** , there was an opportunity to universally inject guardrails that supported both into all our prompts. 
  * It was also essential to **ramp** **new prompt versions gradually** to ensure that they did not break or worsen existing product experiences.


To provide more structure around modularization and versioning, we introduced a **Prompt Source of Truth** component. We standardized the use of the [Jinja](https://jinja.palletsprojects.com/en/3.1.x/) template language for authoring prompts, and built a **Java prompt resolution library to avoid common string interpolation bugs**. 
After we built our standard application framework in Python atop LangChain, we **subsumed the prompt resolution library into it, rewriting it in Python.** Since Jinja offers Python-like expressions, the developer experience became even more fluent and native post this switch.
As conversational assistants with multi-turn conversational UIs emerged, we enhanced this component to provide **more structure around human and AI roles in conversations** , eventually **converging on the[OpenAI Chat Completions API](https://platform.openai.com/docs/guides/chat-completions)** once it was released and widely adopted.
All prompt engineers at LinkedIn today author prompts using the prompt source of truth guidelines. Developers are also required to adhere to the modularization and versioning requirements imposed by this component for storing their prompts, and in exchange **receive fluent sharing across prompts as well as seamless integration into the rest of the application framework**.
**Top Takeaway:** Prompt management can start off as deceptively simple string management, but there is a ton of nuance that includes (but is not limited to) providing systems and developer guidance for managing templating, versioning and prompt structure to make it work at scale for engineering complex GenAI applications.
## Task automation via skills
LinkedIn has been a strong proponent of a skills-based approach to many aspects of work. A skill can be a particular attribute that is acquired either via learning or experience and useful for completing tasks associated with a job. We extended the same abstraction to our GenAI applications to use skill as a mechanism to enable task automation.
The skill abstraction in our framework enables LLMs to move beyond vanilla text generation and use **function calling for** **[Retrieval Augmented Generation (RAG)](https://en.wikipedia.org/wiki/Retrieval-augmented_generation)** or task automation by converting natural language instructions in the prompts into API calls. [In our product, this manifests itself as skills](https://www.linkedin.com/blog/engineering/generative-ai/musings-on-building-a-generative-ai-product) for viewing profiles, searching for posts, querying internal analytics systems and even accessing external tools like Bing for search and news.
Initially, we built this within each GenAI product as custom codethat wrapped existing LinkedIn internal and external APIs (like Bing Search) using **LLM friendly JSON schemas that could be used with the[LangChain tool](https://python.langchain.com/v0.1/docs/modules/tools/) API**. However, this approach ran into some scaling bottlenecks.
  * Teams often **re-implement the same skills** in different products. While we tried to consolidate some popular ones via internal libraries, keeping the libraries up to date and working with various minor product differences became cumbersome.
  * As the **downstream invoked by the skill evolves, the skill also needs to be updated in tandem**.
  * Application developers **need to manually specify the skill or set of skills** to use in the prompt.


To overcome these issues, we came up with **Skill Inversion**.Instead of the calling applications defining skills over the implementing downstreams, the downstreams define the skill and expose it to the calling application, thus organically eliminating the duplication and evolution problems.
We have **eased the process of skill access, development and operations** by building the following:
  * A **centralized skill registry service** that allows definitions to be added and retrieved (via skill ID or semantic search) at runtime.
  * **Build plugins** that easily enable downstream applications to annotate their endpoint implementations and **automatically register them in the skill registry service** with the necessary validations around schema structure and documentation, as part of the build.
  * A **dynamic** that retrieves skill definitions from the skill registry and invokes the actual skill with the supplied arguments, **eliminating developer specified skills in prompts and giving significantly larger agency to LLMs**.


With this infrastructure in place, we are **gradually evolving our** **tech stack by creating skill abstractions for all APIs, enabling LLMs to interact seamlessly with them to achieve richer, more impactful outcomes.**
**Top Takeaway:** There is significant product value unlocked by using GenAI for task automation vis-a-vis content generation. However, intentional full stack tooling is necessary to enable GenAI applications to perform task automation by scalably leveraging the same APIs called by human developers using imperative code.
## Contextual awareness and personalization
Contextualization and personalization are considered essential for a great GenAI product experience, but are not available out of the box since LLMs are stateless by default (i.e., each incoming query is processed independently of other interactions). 
The workaround for this limitation is to build a**Conversational Memory Infrastructure to store LLM interactions, retrieve past context and inject it into future prompts, to share “state” with the LLM** and offer a coherent product experience.
Our initial solution to this problem used or databases as storage. Application teams were **responsible for** **repetitive tasks such as setting up databases, writing requests/responses to the databases, and reading from memory before inference.**
However, we soon needed more than raw conversation storage and retrieval. Since LLM context windows are limited and increasing input tokens has cost/latency implications, it was important to retrieve only the relevant parts of the conversation rather than the entire conversation history. To enable this, we needed **semantic search (using embeddings) and summarization** capabilities.
Rather than build yet another system ground up to solve this at scale, we decided to **leverage the[LinkedIn messaging stack](https://www.linkedin.com/blog/engineering/messaging-notifications/designing-our-new-messaging-system)** for a few reasons:
  * The conversations between a human and GenAI applications were akin to human to human conversations, so much of the functionality of the messaging stack could be reused as-is.
  * The stack was proven to work in production with high availability and reliability.
  * Enhancements we needed, like semantic search and summarization, would also be useful for product use cases outside of GenAI applications. 
  * Leverage was created by the low-latency reliable delivery of messages to mobile/web clients and state synchronization across devices.


We have now integrated our **LinkedIn messaging based Conversational Memory infrastructure** **into our** **GenAI application framework** using the **[LangChain Conversational Memory](https://python.langchain.com/v0.1/docs/expression_language/how_to/message_history/)** abstraction, making integration seamless for application developers.
As our GenAI applications became more advanced, we noticed a trend of needing to **derive signals based on the experience of user-application interactions**. Examples of such signals in our product include voice and tone for authoring text, preferred notification channel (in-app vs push vs email), choice of UI templates for visualizing AI-generated content. We call this **Experiential Memory** (i.e. memory derived based on experience) and offer a solution with drop-in **GenAI application framework integration** , and **support for partial retrievals and updates**.
**Top Takeaway:** Memory is core to the ability to learn from activities or interactions, incorporate feedback and preferences, and build expected personalized and beneficial experiences in product. Depending on the use case, memory is fast becoming a critical capability that requires thoughtful integration into tech stacks.
## Model inference and fine tuning
The advent of GenAI with powerful foundational LLMs that could be prompted to perform a wide variety of tasks turned traditional AI modeling paradigms on their head. Our initial GenAI applications wholeheartedly embraced this new trend and solely used LLMs provided by the . All requests were routed via a centralized **GenAI proxy** which offered functionalities like Trust and Responsible AI checks, seamless support for new models and model versions, incremental response streaming to reduce user-perceived latency and quota management to ensure fair use of relatively expensive LLM resources across various products.
Our GenAI applications have increasingly started depending on our **AI platform** , which is built atop open source frameworks like**[PyTorch](https://pytorch.org/), [DeepSpeed](https://github.com/microsoft/DeepSpeed), [vLLM](https://github.com/vllm-project/vllm)** and provides a **robust and highly scalable fine-tuning and serving infrastructure**. In our experience, **LLMs** like , when **fine-tuned for LinkedIn specific tasks** , **often achieve** **comparable or better quality** as state of the art commercial foundational models on these tasks, but at **much lower costs and latencies.** In keeping with LinkedIn’s ‘members first’ ethos, we also built a [setting](https://www.linkedin.com/mypreferences/d/settings/data-for-ai-improvement) to enable LinkedIn members to more easily control whether their data is used for training or fine-tuning these models.
To make the experience of application developers transparent across external and internal models, we have invested in the following areas:
  * Our inference layer exposes an **OpenAI Chat Completions API for all LLMs in use**. This always allows application developers to program to this API regardless of the underlying model.
  * **Configuration hooks in the application framework allow easy switching between on-prem and external models** , without application developers having to worry about the routing details. This also allows developers to easily experiment with different underlying models for the same use case for local debugging and A/B tests in production.


**Top Takeaway:** Both proprietary and open-source LLMs are evolving at a rapid pace. There is often no one-size fits all solution, and there are various nuanced trade-offs around quality, cost, latency and more to navigate. Engineering organizations that strategically build their tech stack with sufficient abstractions to reuse the same core infrastructure across different models will see long-term benefits and leverage in efficiency and the capabilities of their products.
## Migration
As our GenAI application stack developed and evolved, it was important to rapidly migrate off legacy bespoke solutions to more standardized ones, to minimize tech debt and increase leverage. We handled these migrations using a **lean team combining engineers** with deep knowledge of our **historical Java stack** and engineers working on **the new stack**. Our migration followed the following principles:
  * **Incrementality** : Rather than do a big bang migration of all components, we decided to **migrate individual components one by one**. For example, as soon as the LinkedIn messaging based conversational memory infrastructure was ready, we migrated many of the Java based GenAI applications to it, without waiting for the Python LangChain GenAI application framework migration. For the arguably larger GenAI application framework migration,**we started with the simpler and smaller apps first before handling the more complex and larger ones**. Each migration followed a depth first approach with a small team first prototyping, identifying gaps and fixing them in a small part of the app. This would be followed by a larger team going breadth first across the app doing the actual migration and setting up the A/B tests to ramp the new stack gradually. This led to **progressive learnings and wins** , **ensured** **high levels of operational stability** , and **minimized the impact to the product roadmap** of application teams.
  * **Upskilling talent** : Many of our senior engineers were proficient Java developers but needed to gain more Python experience. We paired them up with earlier in career but more experienced Python developers, to learn on the job, and take an “accelerated Python class”.


We have successfully **migrated many of our existing applications** to our new stack, with a handful of stragglers expected to finish within the next month.
## Final thoughts
Our new GenAI application tech stack embraces AI-first development, lays a solid foundation for building GenAI apps efficiently and responsibly, and accelerates innovation in collaboration with the AI community. This technological groundwork is crucial in achieving our vision to bring economic opportunity to every member of the global workforce.
This blog only covered part of the tech stack centered around the GenAI application framework and its immediate adjacencies. To build and launch production-grade GenAI applications, we also depend on several othercritical areas like our AI platform, in-house modeling, Observability and Monitoring stack, Responsible AI/Trust services and Evaluation process/frameworks, some of which we may cover in a future blog post.
Despite reaching significant milestones, numerous challenges remain. As the cutting edge of product experiences moves from conversational assistants to AI agents, we are seeing an influx of additional functional and operational requirements for our rapidly evolving tech stack. Stay tuned for more technical details around these areas as we build and learn.
## Acknowledgement
Building our application tech stack amidst the rapid evolution of the GenAI landscape is truly a collaborative effort. We would like to thank [Donald Thompson](https://www.linkedin.com/in/thompsondonald/), [Xavier Amatriain](https://www.linkedin.com/in/xamat/) and [Ya Xu](https://www.linkedin.com/in/ya-xu/) for their vision and leadership in architecting and initiating our GenAI platform initiatives, along with the entire leadership team:, [Kapil Surlaker](https://www.linkedin.com/in/kapilsurlaker/), [Zheng Li](https://www.linkedin.com/in/biglizheng/), [Animesh Singh](https://www.linkedin.com/in/animeshsingh1/), [Swapnil Ghike](https://www.linkedin.com/in/swapnilghike/), [Praveen Bodigutla](https://www.linkedin.com/in/praveen-kumar-bodigutla-732a961/), [Grace Tang](https://www.linkedin.com/in/gwtang/), [Mary Hearne](https://www.linkedin.com/in/mary-hearne-a7428018/), [Tyler Grant](https://www.linkedin.com/in/tylercgrant/), [Chanh Nguyen](https://www.linkedin.com/in/chanhnguyen/). 
A special thanks goes to our engineering teams who developed the critical components of our tech stack: [David Tag](https://www.linkedin.com/in/david-tag/), [Yi Liu](https://www.linkedin.com/in/yl2526/), [Nicolas Nkiere](https://www.linkedin.com/in/nicolasnkiere/), [Xiaonan Ding](https://www.linkedin.com/in/xiaonan-kate-ding-790121103/), [Don Jung](https://www.linkedin.com/in/donghoonjung/), [Eugene Jin](https://www.linkedin.com/in/eugenejin/) from the GenAI Foundations team; [Suruchi Shah](https://www.linkedin.com/in/suruchishah/), [Tiffany Zhou](https://www.linkedin.com/in/tiffanyrzhou/), [Shangjin Zhang](https://www.linkedin.com/in/shangjin-zhang-a4372174/), [Jason Belmonti](https://www.linkedin.com/in/jasonbelmonti/), [Ali Naqvi](https://www.linkedin.com/in/syedalinaqvi/), [Sasha Ovsankin](https://www.linkedin.com/in/sashao/) from the AI Platform team; [Han Wang](https://www.linkedin.com/in/-han-wang/), [Tony Guan](https://www.linkedin.com/in/xiaohuaguan/) from the Python team; [Benny Soetarman](https://www.linkedin.com/in/benny-s/), [Qishen Li](https://www.linkedin.com/in/qishen-li-47851156/), [Pantelis Apostolopoulos](https://www.linkedin.com/in/pantelis-apostolopoulos-03b6a7b/) from the System Infra team; [Adam Kaplan](https://www.linkedin.com/in/adamjkaplan/), [Tim Chao](https://www.linkedin.com/in/tim-chao-1bab60102/), [Eddy Li](https://www.linkedin.com/in/cheukmanli/) from the Product Engineering team, and [Priyanka Gariba](https://www.linkedin.com/in/pgariba/), [Sunil Ayyappan](https://www.linkedin.com/in/sunilayyappan/), [Manish Khanna](https://www.linkedin.com/in/manish-khanna/), [Alex Xia](https://www.linkedin.com/in/alexxiapmp/), and [Naqeeb Abbasi](https://www.linkedin.com/in/naqeebabbasi/) from the TPM team.
Finally, we want to express our deepest appreciation to our product engineering teams who are not only dedicated to building exceptional GenAI products but also providing us with invaluable feedback. This includes [Juan Bottaro](https://www.linkedin.com/in/juan-pablo-bottaro/), [Chenglong Hao](https://www.linkedin.com/in/chenglong-hao-161679ab/), [Daniel Hewlett](https://www.linkedin.com/in/dhewlett/), [Xie Lu](https://www.linkedin.com/in/xielu/), [Haichao Wei](https://www.linkedin.com/in/haichao-wei-3340b222/), [Christopher Lloyd](https://www.linkedin.com/in/cwlloyd/), [Lukasz Karolewski](https://www.linkedin.com/in/lukaszkarolewski/), [Parvez Ahammad](https://www.linkedin.com/in/parvezahammad/), [Lijun Peng](https://www.linkedin.com/in/lijunpeng/), [Avi Romascanu](https://www.linkedin.com/in/aviromascanu/), [Pierre Monestie](https://www.linkedin.com/in/pmonestie/) and many others. Your passion and contributions are the driving force behind our success.
Topics: [ Generative AI ](https://www.linkedin.com/blog/engineering/generative-ai) [ Infrastructure ](https://www.linkedin.com/blog/engineering/infrastructure)
Related articles
  * [ Inside LinkedIn's data context layer: curating trustworthy con... ](https://www.linkedin.com/blog/engineering/infrastructure/inside-linkedins-data-context-layer-curating-trustworthy-context-for-humans-and-agents)
Ilho Ye 
Oct 9, 2026 
  * [ Infrastructure ](https://www.linkedin.com/blog/engineering/infrastructure)
[ Infrastructure efficiency: How a classic math problem helped r... ](https://www.linkedin.com/blog/engineering/infrastructure/infrastructure-efficiency-how-a-classic-math-problem-helped-reduced-our-capacity-buffers)
Rahul Yadav 
Sep 14, 2026 
  * [ Announcing Our LinkedIn-Cornell 2026 Grant Recipients ](https://www.linkedin.com/blog/engineering/culture/announcing-our-linkedin-cornell-2026-grant-recipients)
Balaji Krishnapuram 
Sep 2, 2026 


