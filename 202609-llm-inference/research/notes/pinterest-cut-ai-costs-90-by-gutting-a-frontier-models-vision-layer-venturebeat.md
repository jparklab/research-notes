---
title: Pinterest cut AI costs 90% by gutting a frontier model's vision layer | VentureBeat
id: pinterest-cut-ai-costs-90-by-gutting-a-frontier-models-vision-layer-venturebeat
tags:
- llm-inference
- model-layer
- internal-llm-inference-stacks-7f0c90
- named-org-deployment
- custom-embeddings
- licensing
created: '2026-10-09T23:04:28.160680Z'
updated: '2026-10-10T14:52:17.194805Z'
source: https://venturebeat.com/orchestration/pinterest-cut-ai-costs-90-by-gutting-a-frontier-models-vision-layer
source_domain: venturebeat.com
fetched_at: '2026-10-09T23:04:28.138031Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'VentureBeat report (May 29 2026) on Pinterest CTO Matt Madrigal''s own account
  (from the ''Beyond the Pilot'' podcast) of customizing an open-weight model for
  Pinterest''s 620-million-MAU visual discovery product. Pinterest''s conversational
  shopping assistant, ''Navigator 1,'' was built on Qwen3-VL; Madrigal''s team ''ripped
  out'' Qwen''s native vision encoder layer entirely and replaced it with Pinterest''s
  own proprietary multimodal embeddings (precomputed offline from pin/image metadata
  and regularly retrained), cutting AI costs 90% and improving accuracy 30%. Without
  precomputed embeddings, encoding each returned image at runtime individually would
  make inference latency ''20 times worse.'' Madrigal frames the rationale explicitly
  around licensing: ''Open-source models, especially with open Apache licenses where
  you can truly tweak a lot of open weights and customize for unique use cases --
  that''s where we''ve found open source to be so powerful for us.'' Notes Pinterest''s
  longer history of building on open models for visual search (Google''s BERT, OpenAI''s
  CLIP -- the latter fine-tuned in-house as ''Pin CLIP'' with proprietary visual embeddings).
  Pinterest also runs a ''taste graph'': a representational-learning structure of
  per-user embeddings capturing evolving preferences (not a social graph), continuously
  updated from activity signals, used to drive discovery-to-intent conversion. Madrigal''s
  decision heuristic: for anything ''critical for end users, driving engagement, and
  needing to scale to over 600 million MAU,'' Pinterest will either build in-house
  or take open source and ''customize the heck out of it'' rather than default to
  a frontier-model API call per request.'
---

*Suggested by [[what-early-open-weight-model-deployments-reveal-about-production-ai]] — detailed reporting on Pinterest's Qwen3-VL vision-layer replacement, cited by Fastino*

We use essential cookies to make our site work. With your consent, we may also use non-essential cookies to improve user experience, personalize advertisements, and analyze web traffic. For these reasons, we may share your site usage data with our advertising and analytics partners. By clicking “Accept,” you agree to our website's cookie use as described in our Cookie Policy. You can change your cookie settings at any time by clicking “Preferences.” 
PreferencesDeclineAccept
9:24 am, PT, May 29, 2026 
CleoP made with Midjourney
At 620 million monthly users, calling a frontier model for every image recommendation isn't a strategy — it's a bill. Pinterest CTO Matt Madrigal solved it by gutting Qwen3-VL's vision layer and rebuilding it with proprietary embeddings, cutting costs 90% and boosting accuracy 30%.
Madrigal’s team has been heavily investing in customizing open-source models “foundationally in-house.” 
“If you've got really unique data that you can then fine-tune an open source model with, data quality will, frankly, outweigh or overcome model size,” Madrigal explained in a recent [VB Beyond the Pilot podcast](https://www.youtube.com/watch?v=BvFanq9fTg0). 
## How Pinterest customized Qwen for visual discovery
Pinterest, which has around 620 million monthly active users, has long applied open source models for visual search and discovery, going back to Google’s BERT and OpenAI’s CLIP. The company fine-tuned its own Pin CLIP on the latter, incorporating proprietary visual embeddings and image metadata. 
Pinterest’s conversational shopping assistant, Navigator 1, was built on Qwen3-VL and customized in “pretty significant” ways. Madrigal’s team essentially “ripped out” Qwen’s vision encoder layer and fine-tuned the model on proprietary multimodal embeddings. This has allowed them to capture metadata around pins and images that can then be precomputed offline and regularly retrained on new information to deliver personalized experiences. 
“Open-source models, especially with open Apache licenses where you can truly tweak a lot of open weights and customize for unique use cases — that's where we've found open source to be so powerful for us,” Madrigal said. 
Bringing their own embeddings allows his team to gain context around metadata, pins, and images; also, notably, the model performs better at runtime and inference. Without these embeddings, devs would have to call and encode each image returned at runtime, one at a time. That results in a latency “20 times worse” from an inference perspective, Madrigal said. 
“If it's something that's going to be critical for our end users, that's going to drive engagement, that will have to scale to over 600 million monthly active users, we're going to either probably build it or we're going to leverage open source and customize the heck out of it,” he said. 
VB Transform · July 14–15 · Menlo Park · Agentic orchestration
Intuit rebuilt its multi-agent system in 60 days. What did they change — and why?
At Transform, engineering leaders from Intuit, Target, and Instacart break down how they redesigned their orchestration architectures for reliability, scale, and real customers.
[See the full agenda →](https://venturebeat.com/vbtransform2026)
## How a taste graph captures evolving interests
To guide users from inspiration to purchase, Madrigal's team built a "taste graph": a dynamic representation of what individual users actually like, not just what they click on. “It's this representation of billions of people's evolving tastes,” he said. 
People go to Google or other search engines when they have a clear picture of what they want; Pinterest is for when they’re still in the discovery phase, Madrigal said. Pinterest’s goal is to encourage “lateral exploration” and transform discovery to intent (that is, clicking through ads or making purchases). 
Under the hood, the architecture combines a graph structure with representational learning. User embeddings capture a user’s evolving tastes. These are constantly updated based on activity and new content and signals. “It's not a social graph,” Madrigal said. “It's much more of a preference graph: What's going to inspire you? What are you trying to do next?” 
For instance, one user may be into mid-century modern designs; another may prefer a Nantucket aesthetic. Those preferences will be captured in user embeddings, and the taste graph will deliver up specific, relevant products as a result. 
“You go from the upper funnel, inspiration discovery, all the way through lower funnel intent,” Madrigal said. 
**Listen to the full podcast to hear more about:**
  * How Pinterest uses sandboxes to encourage creativity in a way that is secure and contained; 
  * Why a continuous feedback loop can prevent visual AI slop; 
  * The importance of constant benchmarking to gauge user engagement, performance, latency, and other factors. 


**You can also listen and subscribe to**[**Beyond the Pilot**](https://beyondthepilot.ubpages.com/)**on**[**Spotify**](https://open.spotify.com/show/4Zti73yb4hmiTNa7pEYls4)**,**[**Apple**](https://podcasts.apple.com/us/podcast/beyond-the-pilot-enterprise-ai-in-action/id1839285239)**or wherever you get your podcasts.**
AI agents often struggle to assess their own progress and decide what to do next. They can waste compute resources following failed approaches, or they might overlook promising results or stop before completing a task. These problems become more costly as agents take on longer and more complex workflows.
October 9, 2026 
A system that centralizes those guidelines, identifies deviations and allows employees to correct the results could make AI generation more practical for campaigns requiring large numbers of creative assets.
October 8, 2026 
Those findings suggest unresolved gaps in the systems enterprises rely on to judge agent reliability. 
October 8, 2026 
While Google supplies its own Gemini models, the agent can already use Anthropic's Claude models, with support for additional proprietary and open-weight AI models planned.
October 8, 2026 
The strategic signal is that Atlassian is deepening its OpenAI relationship — with real dollars behind it — while deliberately keeping its platform model-agnostic, with Anthropic and others very much in the mix. 
October 6, 2026 
Cohere is going after two common enterprise agent problems with North 2, the new version of its enterprise agent platform. It adds user quotas, rate limits, organization-wide caps and a redesigned agent harness. According to Cohere, North 2 uses memory to keep an agent's context across sessions and gives teams a shared library of knowledge and assets to draw on. It also runs in the cloud, on premises or fully air-gapped.
[Sean Michael Kerner](https://venturebeat.com/author/sean-michael-kerner) October 5, 2026 
Our ITSM platform never lacked data, but it hasn’t always been easy to understand what the data was telling us and what actions we should take next, despite all the time spent building catalog items, interpreting data and usage licenses, and maintaining CMDB health.
October 4, 2026 
MIT's SIFT framework uses a language model to cut coding agent evaluation costs, achieving 35.1% accuracy on Polyglot with reduced compute resources.
October 2, 2026 
In tests, WikiSkill helped a 9B Qwen outscore a 27B model by turning past runs into reusable skills.
October 1, 2026 
OCE is explicitly vendor-neutral and self-hostable, with replaceable models, harnesses and sandboxes. That likely makes it more enticing to enterprises that want persistent agents without relying on a single model provider.
September 29, 2026 
The longer-term plan is to train smaller models on each customer’s private engineering data, using feedback from repeated tasks such as incident analysis. Autoheal argues that these models could eventually reduce operating costs and improve performance.
September 29, 2026 
Jev is reviving an older approach to machine learning, using modern pretrained models to make faster, cheaper decisions without generating text.
September 28, 2026 
2unread notifications
​
