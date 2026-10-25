---
title: Patreon saves nearly $600k/year in ML resources with Baseten
id: patreon-saves-nearly-600kyear-in-ml-resources-with-baseten
tags:
- internal-llm-inference-stacks-7f0c90
- baseten-managed-middle
- baseten-case-study
- managed-middle-negative
created: '2026-10-10T10:58:35.359205Z'
updated: '2026-10-10T14:52:17.276214Z'
source: https://www.baseten.co/customers/patreon/
source_domain: www.baseten.co
fetched_at: '2026-10-10T10:58:35.335258Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Baseten vendor case study: Patreon deploys OpenAI''s Whisper (off-the-shelf,
  unmodified) on Baseten for audio/video transcription and closed-caption generation
  at scale, reporting 440+ engineer-hours/year, $600k/year, and 70% GPU cost savings
  versus self-managed infra or OpenAI''s hosted Whisper API. No fine-tuning, post-training,
  or distillation of any kind is mentioned anywhere in the case study -- Patreon explicitly
  ''deployed and massively scaled the Whisper transcription model'' as-is. This is
  the clean NEGATIVE control for the managed-middle two-part test: part 2 holds (Baseten
  operates serving) but part 1 fails entirely (no weight customization) -- ordinary
  managed hosting of a stock open-weight model, not the managed-middle pattern.'
---

Use your OpenAI commitments on Baseten open models. [Learn more](https://www.baseten.co/talk-to-us/openai/)
Case study
# Patreon saves nearly $600k/year in ML resources with Baseten
## 440+
hours dev time saved yearly
## $600k
annual resources saved
## 70%
savings in GPU cost
### Background 
[**Patreon**](https://www.patreon.com/) is the ultimate platform for creators to offer memberships, provide exclusive access to their work, and cultivate deep connections with their community of supporters and fans. To date, Patreon supports **over 250,000 creators** with **8 million active monthly patrons**. In 2021 alone, these creators earned over $1 billion from memberships.
To best serve their large-scale user base, **Patreon deploys sophisticated ML solutions at record speed** — thanks to Baseten’s ML infrastructure.
### Problem 
As their user base scaled, the Patreon team needed to dedicate time to improving the customer experience of the platform — not hacking away at an unending ML infrastructure checklist. 
According to [Nikhil Harithas](https://www.linkedin.com/in/hurrythas/), Senior ML Engineer at Patreon, handling ML models and gauging when to implement each one isn’t difficult. However, **it’s still a time-consuming, cumbersome, costly process**. Deciding which combination of components is correct isn’t a corner they can cut. 
From a business lens, Patreon needed to transcribe all the audio and video content uploaded by their creators – a substantial task in itself. To add to that workload, the team also needed to implement auto-generated closed captions in their native video product to improve accessibility. 
Then, OpenAI unveiled the [**Whisper**](https://openai.com/research/whisper) neural net with a critical transcription model. Patreon tried using existing products that are for batch compute to serve the Whisper model for new use cases, but they were met with repeated challenges: **prohibitively high** **costs, heavy lifting from their engineering team, or failure to meet Patreon’s data security needs.**
So, their central aim became **minimizing time spent on ML infrastructure tasks** by serving generative AI models scalably and cost-effectively. Enter: Baseten.
### Solution 
Plugging Baseten into their toolset was a game-changer for the Patreon team. They were able to deploy and massively scale the Whisper transcription model, without needing help from infra-minded engineers. 
> “The great thing about Baseten is they’re able to get something up and working for you really, really quickly. Seeing a substantial piece of work functioning within a couple of days is such an impressive, promising work rate.”
### Result 
Since teaming up, Baseten has supercharged Patreon’s ability to serve generative AI models at record speed and low ML infrastructure cost. This in turn has **enabled their team to focus on what differentiates their product versus ML infrastructure**. 
For any business, the most valued assets are people and their time. On that front, Nikhil points out that Baseten saved his team the time and cost of hiring 2 full-time ML engineers. In addition, he adds that Baseten is half the price of serving Whisper via OpenAI, while offering fine-grain control and security plus compliance guarantees. 
**To add specifics, adopting Baseten resulted in three key metrics:**
  * 440+ hours of dev time saved per year 
  * $600k of resources saved per year 
  * 70% savings in GPU cost


> "Running our models on Baseten is more cost-effective than anything else in terms of vertical-specific solutions. It’s twice as cheap as the next cheapest solution. That plus their product improvement velocity and ticking the right compliance markers for us made working with Baseten the perfect partnership."
### What's next 
Speed is critical to project iteration because it reveals the unknowns in the problem space you must work through. With Baseten, **the Patreon team was able to ship a sophisticated ML model within days** — far faster than they could have accomplished independently. 
Looking forward, Nikhil is excited for what that **optimal speed of experimentation, iteration, and deployment** will mean for his team’s ability to provide the best possible experience of Patreon.
## Chosen by the world's most **ambitious builders**
Case study
[EliseAI case study](https://www.baseten.co/resources/customers/how-eliseai-trains-specialized-models/)
Case study
[Gamma case study](https://www.baseten.co/resources/customers/how-gamma-makes-building-presentations-criminally-fun/)
Case study
[Hebbia case study](https://www.baseten.co/resources/customers/hebbia/)
Case study
[Latent case study](https://www.baseten.co/resources/customers/latent-delivers-pharmaceutical-search-with-baseten/)
Case study
[Notion case study](https://www.baseten.co/resources/customers/how-notion-acheived-2-3x-lower-latency-with-ai-meeting-notes/)
Case study
[OpenCode case study](https://www.baseten.co/resources/customers/opencode/)
Case study
[OpenEvidence case study](https://www.baseten.co/resources/customers/openevidence-delivers-instant-medical-information-with-baseten/)
Case study
[Oxen AI case study](https://www.baseten.co/resources/customers/from-datasets-to-deployed-models-how-oxen-ai-builds-on-baseten/)
Case study
[Parallel case study](https://www.baseten.co/resources/customers/how-parallel-web-systems-pioneers-programmatic-web-with-high-throughput-agentic-i/)
Case study
[Patreon case study](https://www.baseten.co/resources/customers/patreon/)
Case study
[Speechify case study](https://www.baseten.co/resources/customers/speechify-real-time-text-to-speech/)
## Explore Baseten today
