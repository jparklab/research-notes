---
title: Post-training Kimi K3 with Harvey for long-horizon legal work
id: post-training-kimi-k3-with-harvey-for-long-horizon-legal-work
tags:
- internal-llm-inference-stacks-7f0c90
- locus-managed-middle-hosted-open-weight
- harvey-tenet
- fireworks-ai
- kimi-k3
created: '2026-10-10T00:02:55.921412Z'
updated: '2026-10-10T14:52:17.259913Z'
source: https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work
source_domain: fireworks.ai
fetched_at: '2026-10-10T00:02:55.901273Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: Fireworks AI's own blog post (8/26/2026) on co-developing Harvey Tenet (Kimi
  K3 post-trained via async RL on the Fireworks Training API). States Tenet scores
  19.7% all-pass on LAB vs 10.8% for base Kimi K3, at near-flat cost ($5.92 vs $5.62
  per task) due to open-weight per-token pricing. Critically, it describes the Fireworks
  platform property that 'training and serving live on the same platform, a promising
  checkpoint can be promoted and validated while training continues, then moved into
  production without leaving the platform' -- the strongest available signal that
  Tenet is served in production on Fireworks infrastructure rather than self-hosted,
  though it stops short of an explicit first-person statement that Harvey's production
  traffic for Tenet runs on Fireworks.
---

Opt-out Preferences
We use third-party cookies that help us analyse how you use this website, store your preferences, and provide the content and advertisements that are relevant to you. However, you can opt out of these cookies by checking "Do Not Sell or Share My Personal Information" and clicking the "Save My Preferences" button. Once you opt out, you can opt in again at any time by unchecking "Do Not Sell or Share My Personal Information" and clicking the "Save My Preferences" button.
Do Not Sell or Share My Personal Information
Cancel Save My Preferences
Your opt-out preference has been honored.
Banner closes automatically in s...
Powered by 
[Join us for our inaugural conference, Forge 2026](https://fireworks.ai/forge)
[Blog](https://fireworks.ai/blog)Post Training Kimi K3 With Harvey For Long Horizon Legal Work
# Post-training Kimi K3 with Harvey for long-horizon legal work
PUBLISHED 8/26/2026
## Table of Contents
[
  * TL;DR](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#tldr)[
  * What LAB measures, and why all-pass is the bar](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#what-lab-measures-and-why-all-pass-is-the-bar)[
  * Results](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#results)[
  * Performance](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#performance)[
  * Generalization](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#generalization)[
  * Minimal regression](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#-minimal-regression)[
  * What’s next](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#whats-next)[
  * The infrastructure behind the run](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#-the-infrastructure-behind-the-run)[
  * Numeric alignment](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#numeric-alignment)[
  * Batch invariance ](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#batch-invariance-)[
  * Build your own frontier](https://fireworks.ai/blog/post-training-kimi-k3-with-harvey-for-long-horizon-legal-work#build-your-own-frontier) 

## Table of Contents
Explore us in AI tools
[ChatGPT](https://chatgpt.com/?q=What%20is%20Fireworks%2C%20how%20is%20it%20different%20from%20OpenAI%20or%20other%20LLM%20APIs%2C%20and%20how%20do%20I%20get%20started%3F)[Claude](https://claude.ai/new?q=What%20is%20Fireworks%2C%20how%20is%20it%20different%20from%20Anthropic%20or%20other%20LLM%20APIs%2C%20and%20how%20do%20I%20get%20started%3F)[Perplexity](https://www.perplexity.ai/?q=What%20is%20Fireworks%2C%20how%20is%20it%20different%20from%20Perplexity%20or%20other%20LLM%20APIs%2C%20and%20how%20do%20I%20get%20started%3F)[CoPilot](https://copilot.microsoft.com/?q=What%20is%20Fireworks%2C%20how%20is%20it%20different%20from%20Copilot%20or%20other%20LLM%20APIs%2C%20and%20how%20do%20I%20get%20started%3F)[Gemini](https://gemini.google.com/?q=What%20is%20Fireworks%2C%20how%20is%20it%20different%20from%20Gemini%20or%20other%20LLM%20APIs%2C%20and%20how%20do%20I%20get%20started%3F)
### Talk to the Fireworks training team
[Get in touch](https://fireworks.ai/contact-training)
## TL;DR
  * •Harvey [recently announced its first model](https://www.harvey.ai/blog/post-training-update-harvey-tenet), Tenet, post-trained in collaboration with Fireworks for long-horizon legal work from a Kimi K3 base using asynchronous reinforcement learning on the [Fireworks Training API](https://docs.fireworks.ai/fine-tuning/training-api/introduction).
  * •Initial work shows promising results for both performance and cost-efficiency, with Tenet completing almost twice as many held-out Legal Agent Benchmark (LAB) tasks as base Kimi K3.
  * •Tenet performance gains transfer to agentic benchmarks the model never saw during training, with no significant regression on legal knowledge benchmarks.


## What LAB measures, and why all-pass is the bar
Legal work is complex and exacting. It requires lawyers to synthesize information, apply judgment, and produce work to the highest of standards.
The [Legal Agent Benchmark (LAB)](https://www.harvey.ai/blog/introducing-harveys-legal-agent-benchmark) is built to meet those high standards. LAB drops agents into sandboxed workspaces with real legal documents and tools and asks them to produce finished deliverables: memos, marked-up contracts, diligence summaries. An LLM judge then opens each deliverable and grades it against dozens of concrete criteria.
A deliverable only passes if it meets every criterion. The metric that matters is all-pass: the share of tasks that clear all of the criteria end to end. A model can score well on average and still miss a single criterion that undermines the output.
## Results
### Performance
Harvey Tenet scores 19.7% all-pass on LAB against 10.8% for base Kimi K3, and 11.3% on Lab Contracts against 9.3% for base Kimi K3 — increases of 9 and 2 percentage points respectively, with almost twice as many held-out LAB tasks completed. Harvey reports these improvements are grounded in broad criteria gains around answer detail and citations. Tenet achieves state-of-the-art performance on LAB Contracts and places second on LAB.
### Generalization
The gains transfer to agentic benchmarks the model never saw during training. Against base Kimi K3, Tenet improves from 58.8% to 74.0% on [Mercor’s Apex Agents](https://www.mercor.com/apex/apex-agents-leaderboard/corporate-lawyer-agent/) (Corporate Law) and from 49.3% to 55.5% on [Crosby’s Redline Bench](https://intelligence.crosby.ai/). Neither benchmark appeared in training, and Redline Bench ran in an entirely different harness than the one the model was trained in. Behavior learned in training transferred across both benchmarks and harnesses.
###  Minimal regression
Tenet holds its performance on benchmarks that test legal knowledge and parametric reasoning rather than agentic capability, including LegalBench, CUAD, MAUD and Mercor’s Apex-v1. Measured against base Kimi K3, improving agentic capability did not come at the expense of the base model’s broader understanding of law.
Additionally, the performance came without a cost penalty. Harvey Tenet runs at $5.92 per LAB task against $5.62 for base Kimi K3 — effectively flat, while completing nearly twice as many tasks. Two things make that possible: (1) open-weight per-token pricing, and (2) the reward shaping described below. Baseline scores are from the Vals LAB leaderboard.
## What’s next
This post is an introduction to the post-training efforts that Harvey and Fireworks have partnered on. A detailed technical write-up of the training methodology (the reward design, the async RL setup, and what we learned running long-horizon rollouts at this scale) will follow.
##  The infrastructure behind the run
Post-training at this scale is an infrastructure problem. Tens of thousands of long-horizon rollouts per checkpoint, each running past 50 turns and 100,000 generated tokens in a live sandbox, put pressure on the training stack in ways that shorter-horizon workloads do not.
Two properties of the Fireworks platform matter most for reinforcement learning of this kind.
### Numeric alignment
We build the training and inference stacks together, and as a result, training and serving share the same numerics. The checkpoint you post-trained is the one that runs. This is not only an operational convenience. When training and inference diverge numerically, that divergence surfaces as token clipping and, past a threshold, reward collapse mid-run. Keeping train-inference KL down is what keeps a long RL run stable.
### Batch invariance 
Requests return identical results regardless of what else is in the batch, through deterministic choices in attention reduction ordering, sparse attention token selection, expert matmul kernel selection, router tie-breaking and cross-GPU all-reduce. Reproducibility at temperature 0 means an eval result is a property of the checkpoint rather than of the batch it happened to land in.
Because training and serving live on the same platform, a promising checkpoint can be promoted and validated while training continues, then moved into production without leaving the platform.
## Build your own frontier
  * •Build a frontier legal organization - [Harvey](https://www.harvey.ai/?utm_medium=sem&utm_source=google&utm_campaign=us_eng_brand_harvey_search_cpc_test&utm_content=general&utm_term=harvey%20ai&hstk_creative=820791704544&hstk_campaign=24133858256&hstk_network=googleAds&gad_source=1&gad_campaignid=24133858256&gbraid=0AAAAA-XoO50tWlTSJrqv0RNeF3JNYqOdR&gclid=CjwKCAjwhZDUBhBGEiwAbi5bjlL04fcUvFn9vx52kTNYYK9Q1hKM9-nsE6LoqJ2S8S99X2ruesrtexoCcX0QAvD_BwE)
  * •Train your own model on Fireworks - [Training docs](https://docs.fireworks.ai)
  * •Learn more about [Frontier-lab training infrastructure as a service](https://fireworks.ai/blog/frontier-lab-training-infrastructure-as-a-service)


## Related Posts
[ Partner Announcements9/10/2026 Gen-1 Slides: Opus 5-level decks at a fraction of the cost ](https://fireworks.ai/blog/gen-1-slides-opus-5-level-decks-at-a-fraction-of-the-cost)[ Partner Announcements12/15/2025 NVIDIA Nemotron 3 Nano on Fireworks: The Engine for Next-Generation AI Agents ](https://fireworks.ai/blog/nvidia-nemotron3-nano)[ Partner Announcements11/24/2025 Fireworks Expands AWS Alliance: Strategic Collaboration Agreement + GenAI Competency ](https://fireworks.ai/blog/fireworks-expands-aws-alliance)
