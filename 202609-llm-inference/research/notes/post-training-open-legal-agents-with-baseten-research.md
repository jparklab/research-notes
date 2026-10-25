---
title: Post-Training Open Legal Agents With Baseten Research
id: post-training-open-legal-agents-with-baseten-research
tags:
- internal-llm-inference-stacks-7f0c90
- locus-managed-middle-hosted-open-weight
- harvey-tenet
- baseten
- qwen
created: '2026-10-10T00:03:04.153593Z'
updated: '2026-10-10T14:52:17.261139Z'
source: https://www.harvey.ai/blog/post-training-open-legal-agents-with-baseten-research
source_domain: www.harvey.ai
fetched_at: '2026-10-10T00:03:04.131565Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Harvey''s own blog post (5/27/2026, predates the Kimi K3/Tenet announcement)
  describing a SEPARATE research collaboration with Baseten Research: post-training
  a 27B open-weight model (Qwen3.5-27B) using iterative SFT (iSFT) plus Baseten''s
  KV-cache compaction technique (''STILL'') inside a legal-agent compaction harness,
  reaching closed-source-frontier performance on Harvey''s LAB benchmark. Explicitly
  framed as early-stage research (''base camp'', ''more to come''), not a production
  deployment -- it names governance/self-hosting (''open-weight models can be hosted
  within a firm''s own secure cloud environment'') as a motivating benefit of post-training
  but never states where this Qwen3.5-27B model actually runs, and does not address
  the Tenet/Kimi K3 production-serving question at all.'
---

Platform
    * [Overview A unified view of how Harvey's products work together to support your entire practice.](https://www.harvey.ai/platform)
    * [Agents Purpose built agents execute complex legal work end to end.](https://www.harvey.ai/platform/agents)
    * [Vault Securely store, organize, and bulk-analyze legal documents.](https://www.harvey.ai/platform/vault)
    * [Knowledge Research complex legal, regulatory, and tax questions across domains.](https://www.harvey.ai/platform/knowledge)
    * [Spaces Work with legal teams across organizations in secure, shared spaces.](https://www.harvey.ai/platform/spaces)
    * [Command Center Analytics, benchmarking, and agentic insights to lead their organization’s AI transformation](https://www.harvey.ai/platform/command-center)
    * [Contract Intelligence Surface insights, strengthen negotiations, and accelerate reviews.](https://www.harvey.ai/platform/contract-intelligence)
    * [Horizon Scanning Track, assess, and take action on regulatory changes.](https://www.harvey.ai/platform/horizon-scanning)
    * [Harvey Mobile Get up to speed, capture new information, and keep work moving from anywhere.](https://www.harvey.ai/platform/harvey-mobile)
    * [Ecosystem Access Harvey where you already work and ground every answer in sources you trust.](https://www.harvey.ai/platform/ecosystem)
    * ### [Introducing MemoryNew Your preferences carried across Harvey so every response comes back consistent with how you work.](https://www.harvey.ai/platform/memory)
Solutions
    * ## Overview
    * ## Industries
      * [Media, Entertainment & Telecom](https://www.harvey.ai/solutions/media-entertainment-telecom)
    * ## Practice Area & Teams
    * ### [A New Era of Collaboration for Legal and Professional Services Law firms and professional service networks have been using Harvey to build new service models and add value collaboratively.](https://www.harvey.ai/blog/a-new-era-of-collaboration-for-legal-and-professional-services)
Resources
    * [Blog Product updates, insights, and behind-the-scenes from the Harvey team.](https://www.harvey.ai/blog)
    * [Resources Hub The latest videos, webinars, guides, and reports from Harvey.](https://www.harvey.ai/resources)
    * [Press Kit Resources for maintaining a uniform and professional presentation of the Harvey brand.](https://www.harvey.ai/press)
    * [Research Models, benchmarks, and field notes from Harvey's research on the frontier of legal AI.](https://www.harvey.ai/research)
    * [ROI Calculator Law Firm See Harvey's Impact on Your Firm.](https://www.harvey.ai/roi-calculator/law-firm)
    * [ROI Calculator In House See Harvey's Impact on Your Business.](https://www.harvey.ai/roi-calculator/in-house)
    * ### [Harvey Academy Introducing Harvey Academy: on-demand training, expert workflows, and step-by-step guidance to help legal teams get the most out of Harvey.](https://academy.harvey.ai/)
Company
    * [About Who we are and what we're building.](https://www.harvey.ai/company)
    * [Careers Join our team and help Harvey shape the future of professional services.](https://www.harvey.ai/careers)
    * [Newsroom Press releases and partnership announcements.](https://www.harvey.ai/newsroom)
    * ### [2025 Year in Review In 2025, we celebrated major customer wins, introduced product breakthroughs, and expanded our global presence. Most importantly, we continued to deepen our commitment to building the best AI solutions for our customers.](https://www.harvey.ai/year-in-review/2025)


[Request a Demo](https://www.harvey.ai/contact-sales)
Login


Insights
# Post-Training Open Legal Agents With Baseten Research
Initial results and research on post-training open weight agents on LAB with Baseten Research.
by •May 27, 2026
[Legal Agent Benchmark (LAB)](https://www.harvey.ai/blog/introducing-harveys-legal-agent-benchmark) was designed with two purposes in mind: (1) to provide a public, shared evaluation for how AI agents perform on real legal work, and (2) to serve as a foundation for research that pushes long-horizon legal-agent capabilities forward.
Of the core areas of agent research, post-training is one of the most promising directions for specialized legal agents, as it simultaneously addresses three key challenges faced by vertical applications:
  * **Domain expertise** : Long-horizon legal work requires a breadth of domain-specific skills, including research (retrieval and search across the closed-universe matter), analysis (reasoning and structured outputs), and drafting (well-formatted, reviewable documents). [Our recent results](https://www.harvey.ai/blog/legal-agent-benchmark-initial-results#:~:text=Legal%20Agent%20Performance) for closed-source foundation models suggests that agents still have a meaningful capability gap on this work, with even the strongest frontier models completing less than 10% of tasks end-to-end.
  * **Cost** : Frontier intelligence is expensive to run. On LAB, reaching the top of the closed-source leaderboard runs to roughly [$50 per task and over 20 minutes of latency](https://www.harvey.ai/blog/legal-agent-benchmark-initial-results#:~:text=the%20start.%E2%80%9D-,Cost%20and%20Latency,-For%20production%20agents). For production legal agents, quality only matters if it fits inside the cost and latency budgets customers can tolerate.
  * **Governance** : Post-training creates a path toward more secure and interpretable agent systems, as open-weight models can be hosted within a firm's own secure cloud environment and enable deeper auditability through full visibility into the model's reasoning traces, tool calls, and intermediate decisions.


Despite success in domains like coding, however, post-training in legal settings has been challenging, in large part due to the private nature of legal data — legal is one of the highest-stakes, sensitive forms of knowledge work. With LAB, and its comprehensive taxonomy of over 1,200 tasks spanning 24 legal practice areas, there is now an opportunity to meaningfully push on open weight model intelligence for legal tasks.
In this post, we outline our initial research and experiments towards open-weight legal agents. In collaboration with Baseten Research, we built a pipeline that takes LAB signal, pairs it with a legal-agent harness designed for long-horizon legal work, and post-trains an open-weight model with that harness in the loop. Using that pipeline, we post-trained a 27B open-weight model, bringing it into the closed-source frontier band on LAB.
The results point to a broader conclusion that post-training and harness optimization need to be developed together. They also open directions worth pursuing next, including richer, less lossy compaction strategies and using compaction artifacts as anchors for partial-credit assignment in long-horizon RL.
## Preparing LAB as a Training Environment
LAB tasks are constructed to mirror real legal work. Each task starts from a partner-style instruction, places the agent inside a closed-universe client matter of documents, and requires it to produce a reviewable legal work product. Every task is paired with an expert-written rubric and performance is measured following our standard all-pass grading.
For our experiments, we leveraged the public LAB tasks and created a hold out test set that is distributionally similar to the training set. As a starting point, we baselined both open weight and closed foundation models on the hold-out. On these baselines, Sonnet 4.6, Opus 4.7, GPT 5.5 lead, with GLM 5.1 and DeepSeek v4 mixed in among them. Below them are the rest of the popular open-weight models.
Figure 1: The mean criterion pass rate per LAB task across popular closed source and open source models. Rubrics were graded by GPT-5.4. Error bars show 95% bootstrap Cls.
## Where Frontier Performance Comes From
Before any training, we started by looking at how the strongest models on LAB research, including retrieval and search.
[As our initial public results showed](https://www.harvey.ai/blog/legal-agent-benchmark-initial-results#:~:text=a%20draft.%E2%80%9D-,Behavioral%20Analysis,-The%20way%20an), Opus, Sonnet, and GPT-5.5 were comprehensive in their research. They opened most documents in the data room and read them in full, often approaching 90% document coverage. The weaker open-weight models did the opposite, they leaned on the **grep** tool to surface specific passages and rarely read documents end-to-end.
We wanted to see whether the same strategy would emerge when a model was trained end-to-end against the LAB rubrics. We ran a light RL pass on Qwen3.5-9B using GRPO. Reward came exclusively from the LAB rubric, with no retrieval-specific signal.
After 40 steps (Figure 2), the criterion pass rate rose from 42.5% to 63.0% on the hold out test data. Importantly, the model's behavior shifted as the score rose. **grep** calls dropped by 67%, the read tool became dominant, and characters read per rollout rose by over 20%. Trained against the LAB criteria, the small model converged on the same strategy as Opus, Sonnet, and GPT-5.5.
Figure 2: Mean reward per step over a 40-step GRPO pass on Qwen3.5-9B, with reward supplied by the LAB rubric and judged by Claude Haiku 4.5. The light line shows per-step values; the bold line is the smoothed trend.
One caveat: When documents follow a fixed form, targeted retrieval can match or exceed read-everything because the model can use the structure to separate signal from a noise for a given client matter. This matters for firm-specific post-training, where the document mix inside a client matter might be narrower and more predictable. We are researching alternative approaches for when this pattern holds.
## Compaction Keeps Agent Context Manageable at Scale
If reading everything is an effective strategy, the question is how to scale that strategy to arbitrarily large client matters. Real client matters often contain hundreds or thousands of contracts and supporting materials, much of it peripheral to any given task. A common answer to this is compaction — periodically compressing the trajectory into a summary so the agent can keep reading.
In order to validate this hypothesis, we implemented a naive natural-language compaction strategy and benchmarked. The agent reads one document per turn in full. Every few reads, it writes a natural language memo to its working directory. The memo is a structured note capturing the facts it has surfaced, the open questions, and any provisional judgements it has reached. The memo replaces the prior chat history, and the agent continues reading.
Figure 3: Message-list state across a single compaction event. (1) Pre-compaction, with raw read history; (2) after rewrite_fn, where prior reads are replaced by a BRIDGE block containing the memo and the list of files already read; (3) one turn later, as the agent resumes reading from the lighter base.
The harness lifted average criterion pass rate by 5–7 points. The all pass rate — the share of matters where every criterion passes — rose more steeply, 2.6x for Sonnet and 3.7x for GPT-5.5. The all-pass rate is the one that maps to real legal review. A deliverable that misses a single issue can be materially incomplete, and partial correctness counts as a failure.
Figure 4: Effect of the natural-language compaction harness on Claude Sonnet 4.6 and GPT-5.5, evaluated on 120 LAB tasks and rubric-graded by GPT-5.4. Left: mean criterion pass rate. Right: all-pass rate, the share of matters where every rubric criterion passes. Error bars show 95% CIs
The same compaction harness gave Qwen3.5-27B minimal lift. Our hypothesis after looking at the data was the compaction mechanics were not the bottleneck. Qwen3.5-27B struggled with writing a memo that captured the reasoning and facts that matter for the task. Frontier closed models can write a useful memo on their own with the harness just unlocking it. Qwen3.5-27B needed the combination of the harness that captures the right strategy, and post-training that teaches the model to execute it.
## Training Qwen3.5-27B Model to use a Simple Compaction Harness
We post-trained Qwen3.5-27B using [Iterative SFT](https://www.baseten.co/research/iterative-sft/) (iSFT) to be better in this natural-language compaction harness.
Figure 5: Qwen3.5-27B before and after iterative SFT in the compaction harness, alongside Claude Sonnet 4.6 and GPT-5.5 running the same harness. Evaluated on 120 LAB tasks, rubric-graded by GPT-5.4. Top: criterion pass rate. Bottom: all-pass rate. Error bars show 95% CIs.
Training data for iSFT came from teachers running each task end-to-end inside the harness, with privileged access to the rubric, until they produced a fully-passing deliverable.
Because the training data was filtered to rubric-passing rollouts, post-training lifts the model on both capabilities the intro called out: (i) retrieval, where the model learns to use the compaction harness fluently, and (ii) analysis with reasoning, where it learns to produce passing deliverables from the compacted context.
The main challenge was what to do with imperfect first-pass rollouts. Discarding them wastes signal, but two of the obvious recovery paths also have issues. Feeding the teacher more privileged information, like the expected answer or additional hints, pushes the data off-policy. Each unit of extra information the teacher relies on is a unit the student will never have at inference. Surfacing the judge's feedback to the teacher fails for the same kind of reason from the other end. The model learns to respond to a grader that won't be there. We resolved both with what we call a private-mode submit. When a rollout falls short, the teacher gets a chance to notice the gap itself, in its own voice, with no trace of where the correction came from.
The other choice was matching the training shape to inference. A trajectory might span hundreds of thousands of tokens, but at inference the model only ever sees one compacted window at a time. We trained per-window, with the loss shaped to teach the model to act in the harness, not to predict it.
Figure 6: Conversion of a single rollout with M memento.md writes into M+1 per-window training examples. Each window becomes one example: window 0 retains the raw read history, while subsequent windows replace prior content with a BRIDGE block carrying the latest memento and the list of files already read. Loss (★) is applied only to assistant tokens within the current window.
## Beyond Textual Compaction: Compaction in KV Cache Space
The above memo-based compaction harness has a structural ceiling. The natural-language summary is relatively lossy. This worked well enough at the scale of the matters we tested, but it caps what compaction can preserve, and on the hardest long-horizon matters the residual losses compound.
The number of bits stored in a textual summary is very small relative to the rich internal representations in a model’s KV cache. In order to get around this information bottleneck, Baseten has developed a [novel technique](https://www.baseten.co/research/towards-infinite-context-windows-neural-kv-cache-compaction/), STILL, for directly compressing the KV cache. Given a frozen base model with layers, KV-heads per layer, and head dimension , the per-layer cache for a -token prefix has the form K(ℓ,h),V(ℓ,h)∈RK^{(\ell, h)}, V^{(\ell, h)} \in \mathbb{R}^{T \times d},∈. Baseten trains a per-layer compactor
gϕ:(K(ℓ,h),V(ℓ,h))H⟼(Ck(ℓ,h),Cv(ℓ,h))Hg^{(\ell)}\phi : (K^{(\ell,h)}, V^{(\ell,h)}){h=1}^{H} \;\longmapsto\; (C_k^{(\ell,h)}, C_v^{(\ell,h)}){h=1}^{H}(,)⟼(C,C)
with compact keys and values Ck(ℓ,h),Cv(ℓ,h)∈RC_k^{(\ell,h)}, C_v^{(\ell,h)} \in \mathbb{R}^{t \times d}C,C∈ and t \ll T. Internally, each gϕg^{(\ell)}_\phi uses cross attention to a bank of learned latent queries and projects out to a set of compact keys and values via two linear heads. This compactor is trained without requiring any alterations to the base model itself; we can apply this compaction strategy to any pre-trained open source model.
Figure 7: The STILL per-layer compactor. A bank of learned latent queries cross-attends the full per-layer KV cache (with RoPE stripped from keys), refines through self-attention and feed-forward sub-layers, and projects to compact keys  and values  in a single forward pass.
Two properties make this the right shape for long-horizon legal work. Text-based summaries work by **discarding irrelevant information**. Therefore, when a task genuinely requires knowledge of a very large amount of factual details, it is simply impossible to achieve high compression ratios with text based summaries. This kind of task is very common in legal domains where numerous clauses and specific facts may play a pivotal role in the successful completion of the task. By making use of superposition in the latent space of a compressed KV cache, we are able to store exponentially more information. Secondly, by training the KV compression module in this specific domain, we are able to learn to **distinguish important information from irrelevant details**. Both of these factors can combine to unlock a fundamental discontinuity in capabilities that would not be possible with purely text based summaries.
## The Climb From Here
A 27B open-weight model landing in the closed-source frontier on LAB is base camp. Post-training research and harness optimizations without the high quality LAB data would not have yielded the same performance. The climb ahead includes research directions around less lossy compaction in KV cache space, using compaction artifacts as anchors for partial-credit assignment in long-horizon RL, and closing the reasoning capability gap that retrieval alone doesn't address. It is a much bigger lift than this one post, more to come.
## Next Up
### [Vendor Contract Management Starts With Legal Setting the Standard](https://www.harvey.ai/blog/vendor-contract-management)### [Contract Obligation Management Has to Follow the Contract as it Changes](https://www.harvey.ai/blog/contract-obligation-management)### [AI Fluency Is Legal Education's New Differentiator](https://www.harvey.ai/blog/ai-fluency-is-legal-educations-new-differentiator)
