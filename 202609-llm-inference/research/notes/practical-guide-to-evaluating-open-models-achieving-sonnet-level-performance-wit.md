---
title: 'Practical Guide to Evaluating Open Models: Achieving Sonnet-level performance
  with Kimi'
id: practical-guide-to-evaluating-open-models-achieving-sonnet-level-performance-wit
tags:
- llm-inference
- model-layer
- internal-llm-inference-stacks-7f0c90
- named-org-deployment
- primary-technical-report
- eval-suite
created: '2026-10-09T23:03:02.887348Z'
updated: '2026-10-10T14:52:17.179016Z'
source: https://amplitude.com/blog/open-weights-experimentation
source_domain: amplitude.com
fetched_at: '2026-10-09T23:03:02.864133Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Amplitude''s own first-person engineering blog (Jacob Newman, Principal
  PM), describing exactly how Amplitude deployed open-weight models in its ''Global
  Agent'' product. Context: Microsoft''s ''Open Weights and American AI Leadership''
  open letter (signed by Nvidia, Microsoft, Meta, Dell, IBM, Palantir, Mistral, Hugging
  Face initially, growing past 200 signatories within a week including OpenAI, Google,
  Amazon, Uber, Databricks, GitHub, Fireworks AI, and Amplitude itself); Amplitude''s
  CEO Spenser Skates confirmed in his signing request that ''Kimi and GLM are already
  core to how we operate, both internally and in customer-facing AI features.'' Cites
  Stanford HAI AI Index: the human-preference gap between open and closed models on
  Chatbot Arena fell from 8% to 1.7% in one year. Prior production stack: all Global
  Agent subagents (Global Chat, Chart Agent, Data Assistant, Session Replay SQL, Preference
  Extractor) ran on Claude Sonnet 4.6. Hosting choice: selected Fireworks'' managed
  open-weight catalog (Kimi, GLM, gpt-oss billed per token) over Modal''s raw GPU
  containers, because dedicated Fireworks GPU instances matched serverless exactly
  on Amplitude''s quality suite with more consistent latency and fixed hourly cost.
  Model selection mapped three open-weight options against Claude''s tiers: gpt-oss-120B
  ~ Haiku tier (11-25x cheaper, weak instruction-following, narrow subagent use only);
  Kimi K2.6/K2.7 ~ Sonnet tier (trained for 200-300-step tool chains, #1 on tau2-bench
  Telecom at 93%, ~2x thinking-token cost); GLM 5.2 ~ near-Opus (within ~4 points
  of Opus on SWE-bench/Terminal-Bench at ~6x lower cost but weaker on long-horizon
  tasks and blind preference). Chose Kimi K2.7 for the head agent. On an internal
  186-case eval suite mirroring production traffic, Kimi K2.7 scored 64 unmodified
  vs Sonnet 4.6''s reference; after system-level remediation (caching/parallelizing
  permission checks, single-agent compiler routing for chart work -15% latency, typed/validated
  chart-definition schemas, a loop-breaker for degenerate tool loops, a dedicated
  compute/date tool to remove hand-done arithmetic, grounding rules against capability
  hallucination, a reasoning-leakage sanitizer, and a context clamp tied to the model''s
  real context window) it scored 73.7, edging past Sonnet''s 72.7, at roughly one-third
  Sonnet''s price (with the real gap possibly wider since Anthropic bills cache writes
  and Fireworks doesn''t). ~80% of failed eval cases traced to the head agent. Global
  Chat alone is ~44% of all Global Agent spend. Amplitude built this evaluation using
  its own ''Agent Analytics'' product (usage data, cost-per-session, failure taxonomy)
  and is now running a staged, customer-segment rollout rather than a single cutover,
  explicitly noting Kimi''s launch benchmarks are vendor-reported and independent
  coverage is still thin.'
---

*Suggested by [[what-early-open-weight-model-deployments-reveal-about-production-ai]] — named first-person case study of Kimi K2.7 deployment cited as primary source*

[___](https://amplitude.com/blog/open-weights-experimentation)
[ Jacob Newman Principal Product Manager, Amplitude ](https://amplitude.com/blog/author/jacob-newman)
On July 24, Microsoft published an open letter, "[Open Weights and American AI Leadership](https://www.microsoft.com/en-us/corporate-responsibility/topics/open-weight/)," making a case that open-weight models are critical infrastructure for competition, security, and the diffusion of AI into everyday business. The original 25 signatories were a specific kind of company: Nvidia, Microsoft, Meta, Dell, IBM, Palantir, Mistral, Hugging Face. They obviously have a stake in open models as a category. In the first week, that list grew past 200 signatories, adding OpenAI, Google, Amazon, Uber, Databricks, GitHub, and Fireworks AI (our own infrastructure vendor), among many others.
Amplitude is on that list too. Our CEO Spenser Skates [posted his email to Microsoft](https://x.com/spenserskates/status/2082214313406926873) requesting to join. In the note, he mentioned our open-source SDKs across every major stack and described Amplitude as “heavy users of open-weight AI.” He made a note that Kimi and GLM are already core to how we operate, both internally and in customer-facing AI features.
There’s an industry shift happening, and we’re happy to be a part of it. Companies that build AI products are willing to put their name on open weights as infrastructure worth defending. We want to go one step further. In this post, I’ll outline exactly how Amplitude is using open-weight models to improve our [Agents](https://amplitude.com/global-agent). If your team is considering open-weight models, use our example as inspiration to run your own tests and make your products even better.
### Why now is the right time to test open-weight models for agentic products
Open-weight models have been improving rapidly over the last year. As of today, they’ve significantly closed the performance gap with frontier models. The Stanford HAI AI Index reports the human-preference gap between open and closed models on Chatbot Arena falling from 8% to 1.7% in just the last year. The latest proof point is Kimi K3, the 2.8-trillion-parameter open model Moonshot released in late July, which the company says performs competitively with Anthropic's Fable 5.
These open-weight models aren't an automatic improvement on OpenAI or Anthropic’s models. Instead, they're an opportunity for teams to make specific tradeoffs. [The New Stack](https://thenewstack.io/kimi-k3-open-weights/) spells out the exact compromise teams make when they choose Kimi K3 over the closed frontier models: "same results, one-third the cost, four times slower." Cost has been the headline when it comes to discussing open-weight models, but there’s a more important reason that will ultimately matter more: autonomy.
An open-weight model can be downloaded, installed on company infrastructure, and fine-tuned against a specific product surface before it ever serves a real request. The data stays contained inside that infrastructure instead of leaving for a third party on every call. In short, it’s customizable and ownable. As teams strategize to manage increasing token prices, the control offered by open-weight models is becoming an attractive choice.
### Setting the stage: Our original proprietary setup
Before open-weight models were a possibility, the AI system behind Global Agent, Amplitude's in-product agent experience, was running on Sonnet 4.6 across its full set of subagents: Global Chat, Chart Agent, Data Assistant, Session Replay SQL, Preference Extractor, and a handful of smaller ones. Like all frontier models, Sonnet is built to be generally good at everything rather than at specifically what we need Global Chat or Chart Agent to do. Since Sonnet's weights aren't open, we could prompt around failure patterns, but couldn't fine-tune the model itself against them. It works well, but there’s still room for improvement.
The goal of our test was to find out whether an open-weight model could maintain Global Agent's existing customer experience (using Amplitude's own evaluation criteria) at a meaningfully lower cost. We wanted a real deployment decision rather than just a hypothetical research readout, so we did all we could to detail specifics: which model, at what cost, for which customers, etc.
### How we put our open-weight model tests together
**Where to run it.** We considered two primary options here:
  * [Fireworks](https://fireworks.ai/) offers a managed catalog of open-weight models (Kimi, GLM, gpt-oss) billed per token, with no infrastructure to operate.
  * [Modal](https://modal.com/) offers raw GPU containers where you bring your own serving stack and own every part of quantization and batching yourself.


Since our model is already in a hosted catalog sitting behind a production agent, Fireworks won. Inside Fireworks, dedicated GPU instances matched serverless exactly on Amplitude's own quality suite, delivered the most consistent latency the team measured, and came with a fixed hourly cost that scales with volume instead of token count. That combination made dedicated the safe default.
**Which model to run.** The team started by mapping three open-weight options against the tiers of Claude's lineup to make comparisons easier.
  * gpt-oss-120B comes in close to Haiku: strong tool-calling on τ-bench, 11 to 25x cheaper, but weak enough on instruction-following that it fits narrow subagent work rather than orchestration.
  * Kimi K2.6 and K2.7 land close to Sonnet: trained specifically for long agentic tool chains (200 to 300 steps), ranked #1 on the independent τ²-bench Telecom benchmark at 93%, at the cost of roughly double the thinking-token burn.
  * GLM 5.2 lands close to Opus, though not quite there: within about four points of Opus on SWE-bench and Terminal-Bench at roughly 6x lower cost, but it loses blind preference reviews and struggles on long-horizon tasks.


Amplitude picked Kimi K2.7 for Global Agent's head agent, not because it scored highest on paper, but because it was the closest fit.
Picking the model class was step one. Making Kimi K2.7 actually behave like Sonnet in production was the longer project. That work split into two separate problems, latency and reliability, which I’ll discuss in detail below.
**Test structure**. Our model tests ran on an 186-case internal evaluation suite built to mirror the real shape of Amplitude traffic: conversational capability questions, tool routing and dispatch, chart generation, and the date/arithmetic handling. A standalone testing environment, isolated from the production path and graded on outcomes rather than intermediate steps, let the team validate the new architecture before any of it touched live routing.
**Latency remediation.** Our data showed subagents rarely trigger but dominate wall-clock time when they do. Our Data Science (299 seconds) and Chart Deep Research (217 seconds) subagent calls took considerably longer than the agents handling the bulk of traffic.
_Average AI response latency by agent, last 30 days of production traffic._
Routing chart work through a single-agent compiler flow instead of a full subagent dispatch matched production correctness at roughly 15% lower mean latency. Two shipped changes did most of the rest:
  1. Permission checks that used to re-run expensive, blocking lookups on every tool call were cached and parallel. That turned seconds of overhead into sub-second tasks and eliminated false chart-permission denials as a side effect.
  2. Chart definitions that used to be loose JSON that only failed once they hit the backend were a typed, validated contract. This made sure no invalid chart definition was persisted in the first place.


**Reliability remediation.** The team traced every failed case on our internal eval suite back to a specific, recurring pattern. Almost none of these issues showed up when Sonnet ran the same harness:
  * Capability hallucination: answering "can Amplitude do X" from training data instead of checking, inventing UI paths or plan limits that don't exist
  * Degenerate tool loops: retrying the same wrong tool until the turn died with no answer at all
  * Hand-done math and dates: miscalculated tables, epoch conversions a year off, weekday labels shifted by one
  * Chart semantic near-misses: definitions that were syntactically valid but semantically wrong, like the wrong event or dropped attribution, relayed back to the user as a success
  * Output leakage: raw model reasoning leaking into the final answer, or a final message that came back empty


About 80% of failed cases traced back to the head agent rather than the subagents, which is part of why the fixes went there first. None of the fixes were a better-worded instruction. Instead, we built remediations to specifically address the common failures.
We used a loop breaker to cut off tool access after repeated bad calls and forced an honest answer instead of a dead turn. We added a dedicated compute tool and date table so numbers and dates never pass through the model's own arithmetic. Clearer grounding rules now require a docs or tool check before any capability claim. A sanitizer strips leaked reasoning tags and guarantees a real final message even when generation fails partway through. And a context clamp ties history compression to the model's actual context window, which turned out to be the root cause behind nearly every "prompt too long" failure.
### Test results and takeaways
Without any adjustments, Kimi K2.7 initially scored 64 on our 186-case suite, a real gap behind Sonnet 4.6. After our remediation work, Kimi scored 73.7, edging out Sonnet's 72.7 on the identical test cases.
_Kimi K2.7's score on Amplitude's internal eval, before and after the remediation work, against Sonnet 4.6 as the production reference_
The fixes closed the performance gap without impacting cost. Running the same workload on Kimi K2.7 still comes out to roughly a third of Sonnet's price. The real gap may even be wider, since Anthropic bills cache writes and Fireworks doesn't.
_Per-1M-token pricing on Fireworks vs. Anthropic._
Global Chat, the head agent, is where that cost multiplier matters most. It accounts for roughly 44% of all Global Agent spend on its own, which is why the model swap and the tuning work concentrated there first.
_Share of total Global Agent spend by subagent, most recent measurement period._
Kimi K2.7 wasn’t superior out of the box. It was a Sonnet-class open-weight model that needed Amplitude-specific tuning and correcting to account for specific failures. After our adjustments, Kimi K2.7 produces results comparable to Sonnet on the metrics that matter for Amplitude, at a third of the price.
The model conversation isn’t over. Kimi's own launch benchmarks are vendor-reported, and independent coverage is thin. Some third-party trackers still give Sonnet the edge on tool-routing reliability. There’s a lot of subjectivity in deciding what's right for an individual product or even specific user groups.
Our plan isn't to move every Amplitude customer onto an open-weight model at once. Amplitude is testing across customer segments to see whether one model should serve everyone, or whether some customers end up on an open-weight model while others stay on Sonnet. Perhaps live production traffic will help us settle the questions our eval suite can't answer.
### How Agent Analytics drove our testing
Amplitude could only run this evaluation because we already collect product usage data and build evals to determine whether a model swap holds up for customers. Agent Analytics is designed to answer exactly this kind of question for any team running AI agents of their own: how the agent performs, what it costs per session, and where the real tradeoffs are between the two.
The latency chart, the spend-by-subagent breakdown, the failure taxonomy, and the before-and-after eval scores are all Agent Analytics views of our own Global Agent. Our instrumentation is what turned a model swap into a deployment decision. Benchmarks told us Kimi K2.7 was Sonnet-class on paper. Global Chat alone is 44% of Global Agent spend, so the head agent is where a 3x cost gap pays off first. It showed Data Science calls running 299 seconds, so the latency work had a target. And it traced the 9-point eval gap to five recurring failure patterns, so every remediation shipped against a named failure instead of a hunch, and the re-run scored 73.7 against Sonnet's 72.7.
If you're weighing an open-weight swap for your own agent, the sequence is the same one we ran: baseline cost per session and failure patterns on production traffic, test the candidate model against your own evals, fix the specific gaps, then let a slice of live traffic settle what the eval suite can't. 
We built Agent Analytics for ourselves last year when we found a measurement gap between traditional product analytics and observability tools for our own agent use cases. **_Learn more[here.](https://amplitude.com/agent-analytics)_**
About the author
Jacob Newman
Principal Product Manager, Amplitude
[More from Jacob](https://amplitude.com/blog/author/jacob-newman)
Jacob is a product manager at Amplitude, focused on the core analytics product. He began his career at startups in the ed-tech and recruiting space, where he learned to build products informed by data. Outside of work, you’ll find him listening to podcasts or getting lost in a sci-fi or fantasy novel.
[More from Jacob](https://amplitude.com/blog/author/jacob-newman)
Topics
[Amplitude Agent Analytics](https://amplitude.com/blog/tag/amplitude-agent-analytics)[Open-Weight AI Models](https://amplitude.com/blog/tag/open-weight-ai-models)
#### Recommended Reading
[ Insights Lessons from consolidating our MCP server Oct 6, 2026 10 min read ](https://amplitude.com/blog/lessons-from-consolidating-our-mcp-server)[ Insights What AI engineers do in the age of agentic code Oct 1, 2026 14 min read ](https://amplitude.com/blog/what-ai-engineers-do)[ Insights An analysis of Jev: Faster and cheaper, but watch the accuracy Sep 28, 2026 13 min read ](https://amplitude.com/blog/jev-analysis)[ Product Your agent isn’t broken. It’s guessing. Sep 23, 2026 6 min read ](https://amplitude.com/blog/ai-context-configuration)
