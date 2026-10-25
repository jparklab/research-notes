---
title: Shadow Mode, Forever · Nadir
id: shadow-mode-forever-nadir
tags:
- internal-llm-inference-stacks-7f0c90
- model-rollout
- canary-deployment
- rollback-triggers
- vendor-content
created: '2026-10-09T22:56:27.640394Z'
updated: '2026-10-10T14:52:17.112958Z'
source: https://getnadir.com/blog/shadow-testing-canary-rollout-llm-model-swap/
source_domain: getnadir.com
fetched_at: '2026-10-09T22:56:27.618462Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Vendor blog post (Nadir, an LLM-routing/verification startup) using the
  SRE-derived shadow-test / canary-rollout pattern for LLM model swaps as context
  before pitching its own per-request verifier product. Defines shadow mode precisely:
  production requests mirrored to both incumbent (which answers the user) and candidate
  (which does not); an automated judge scores the pairs on accuracy/tone/task-completion/format;
  full shadow mode roughly doubles inference spend for the 1-2 week test window. Canary
  deployment ramps the candidate through real traffic at 1% -> 5% -> 20% -> 50% ->
  100%, holding each stage >=24 hours, with named automated rollback triggers: guardrail
  trip rate >1.5x baseline in a 15-min window, offline rubric score regressing below
  noise floor, p99 latency >1.3x baseline, or an error cluster unique to the candidate
  -- median rollback latency once triggered is ~35 seconds (attributed to Future AGI,
  ''LLM Eval with Shadow Traffic and Canary Deployment,'' 2026). Cites a named failure
  case: a team that flipped a candidate to 100% of traffic without staged validation
  saw function-call accuracy drop 9 points and refusal rate climb 7 points, invisible
  until the flip was live (same Future AGI source). Introduces ''mirror sampling''
  -- running shadow at 10-25% of full traffic instead of 100% -- as a cost-reduced
  middle ground (10% mirror on 1M daily requests yields ~100k scored pairs/day), attributed
  to the same Future AGI source and to TianPan, ''Releasing AI Features Without Breaking
  Production'' (April 2026). Core argument/thesis: shadow and canary validate the
  swap once, over a bounded pre-promotion window, then the comparison pipeline is
  torn down at 100% promotion -- so distribution shift after rollout (new customer
  segment, longer context, new tool schema) goes uncaught; proposes per-request calibrated
  verification (cheap model drafts, a calibrated verifier scores, escalates to a strong
  model only on failure) as a permanent alternative to one-time gating. Should be
  read as vendor content: numeric claims (AUROC 0.961, pass@1 benchmarks) are sourced
  to the vendor''s own RouterBench experiments, not independently verified.'
utility_score: 15.0
---

All posts
## Shadow mode catches the regression. It also stops the moment you flip to 100%.
A cheaper model that matches frontier quality shows up every few weeks now: MiniMax M3 undercutting Opus 4.8, DeepSeek V4 at a fraction of Anthropic and OpenAI's rates, Meta's Muse Spark opening at a quarter of the going output price. The economics are obvious. So is the fear: swap the model under a production agent and the failures don't show up in a demo, they show up three weeks later on a customer's edge-case prompt. [Read what happened when one team tried the blanket swap.](https://getnadir.com/blog/minimax-m3-vs-opus-blanket-swap-routing)
The industry's answer, borrowed straight from SRE, is a staged rollout. Duplicate live traffic to the candidate model without showing anyone its output, score the pairs offline, then promote gradually with an automated rollback trigger watching every stage. It's the right instinct. It also has a blind spot nobody talks about: the moment the rollout finishes, so does the checking.
## What "shadow" and "canary" actually mean
Shadow mode mirrors production requests to both the incumbent model, which answers the user, and the candidate, which doesn't. An automated judge scores the two outputs against each other on accuracy, tone, task completion, and format compliance, while the team also tracks token count, cost, and latency deltas. Because every request gets answered twice, shadow mode roughly doubles inference spend for as long as it runs, usually one to two weeks. [Source: TianPan, "Releasing AI Features Without Breaking Production," April 2026](https://tianpan.co/blog/2026-04-09-llm-gradual-rollout-shadow-canary-ab-testing)
Canary deployment is the next stage: the candidate serves real users, starting at 1% of traffic, sometimes 0.1% for anything high-stakes, and ramping 1% → 5% → 20% → 50% → 100%, holding each stage at least 24 hours before advancing. [Source: TianPan, "Releasing AI Features Without Breaking Production," April 2026](https://tianpan.co/blog/2026-04-09-llm-gradual-rollout-shadow-canary-ab-testing) Mirror sampling, running shadow at 10-25% of full traffic instead of 100%, cuts the overhead while still producing enough scored pairs to catch a distribution-level regression: a 10% mirror on a million daily requests generates roughly 100,000 scored pairs every 24 hours. [Source: Future AGI, "LLM Eval with Shadow Traffic and Canary Deployment," 2026](https://futureagi.com/blog/llm-eval-shadow-traffic-canary-2026/)
Both stages lean on automated rollback triggers, because a human watching a dashboard doesn't catch a regression fast enough. The common set: guardrail trip rate above 1.5x baseline in a 15-minute window, an offline rubric score that regresses below the noise floor, p99 latency above 1.3x baseline, or an error cluster that shows up only on the candidate and wasn't present in the incumbent's trailing week. Median rollback latency once one of those trips: about 35 seconds. [Source: Future AGI, "LLM Eval with Shadow Traffic and Canary Deployment," 2026](https://futureagi.com/blog/llm-eval-shadow-traffic-canary-2026/)
Skip the discipline and the failure is predictable. One team that flipped a candidate model to 100% of traffic without staged validation saw function-call accuracy drop 9 points and refusal rate climb 7 points, both invisible until the flip was already live. [Source: Future AGI, "LLM Eval with Shadow Traffic and Canary Deployment," 2026](https://futureagi.com/blog/llm-eval-shadow-traffic-canary-2026/)
## The gate that closes after you walk through it
Here's the part the runbooks don't emphasize: shadow and canary validate the swap, not the traffic. They answer "is this candidate model good enough to trust with my production workload," once, over a sample of requests gathered during a specific one-to-two-week window. Once the candidate hits 100% and the pipeline is torn down, nothing is comparing its answers to anything anymore. It's just the model now.
That's fine if your traffic tomorrow looks like your traffic during the rollout. It rarely does. A new customer in a market your shadow sample under-represented, a longer context than anything in the test window, a tool schema that shipped after the canary closed, a prompt that happens to sit exactly on the candidate's weak spot: none of those get caught, because the thing that would have caught them stopped running weeks ago. [Routing without a verifier is dead reckoning](https://getnadir.com/blog/routing-without-verification-dead-reckoning) for exactly this reason: you're trusting that the map still matches the territory.
## Shadow, canary, and per-request verification, side by side  
| Cost overhead  | Runs  | Catches  | After promotion  |  
| --- | --- | --- | --- |  
| Shadow (full mirror)  | ~2x inference spend  | 1-2 weeks, pre-launch  | Distribution-level regressions, before any user sees them  | Nothing. The mirror stops.  |  
| Canary (staged %)  | Marginal, scoped to the slice  | 1-2 weeks, ramping 1% to 100%  | Real-user regressions the offline rubric missed  | Nothing. The rollback logic is torn down at 100%.  |  
| Mirror sampling  | 10-25% of full duplication  | Ongoing, if someone keeps it on  | Same as shadow, lower confidence  | Only if it's still running, which it usually isn't  |  
| Per-request verification  | Built into the cascade, no separate pipeline  | Every request, permanently  | Any single response that fails a calibrated bar, not just the aggregate  | Nothing changes. It's still checking.  |  
## What continuous verification looks like
The shape of it isn't complicated. Instead of gating a model swap once, gate every response:
pythonCopy

```
def serve(prompt, cheap_model, strong_model, verifier, accept_threshold):
    draft = cheap_model.generate(prompt)
    score = verifier.score(prompt, draft)  # calibrated, not a raw judge call

    if score >= accept_threshold:
        return draft, "cheap"

    return strong_model.generate(prompt), "escalated"
```

The difference from a canary gate is where the check sits. A canary asks "did the candidate hold up across last week's sample." This asks "did the candidate hold up on this exact prompt, right now," and it asks it forever, not just during a launch window. A calibrated verifier trained for the job, not a general-purpose LLM-as-judge call bolted on for the rollout, is what makes checking every request instead of a sample cheap enough to do permanently. Judging every response with a second frontier-model call would just move the cost problem instead of solving it, which is [its own tax worth measuring](https://getnadir.com/blog/eval-tax-llm-judge-cost-cascade).
## Where Nadir fits
Nadir starts with the shadow-testing mechanism described above: return a decision receipt without changing the model that serves traffic, then attach measured usage and outcomes before enforcement. Complete non-streaming proxy responses can optionally enter a reference-free verifier cascade; streaming responses bypass it. AUROC 0.961 belongs to a separate reference-assisted RouterBench research experiment and is not deployed accuracy.
The response metadata make the decision auditable per request: `model` shows which model answered, `nadir_metadata.complexity_analysis` records the routing decision, and a configured, priced benchmark adds `nadir_metadata.benchmark_comparison.savings_usd`. Verifier outcomes appear in the cascade metadata only when that optional complete-response path runs.
## What to ship this week
  1. **If you're mid-rollout on a model swap, keep the mirror running past 100%.** Even a 5% sample after full promotion catches drift that a one-time canary can't, for a fraction of the cost of full shadow mode.
  2. **Automate the four rollback triggers, don't watch a dashboard.** Guardrail trip rate, rubric regression, p99 latency, and candidate-only error clusters, wired to auto-rollback, not a Slack alert someone reads on Monday.
  3. **Ask what happens to your quality check the day after 100%.** If the honest answer is "nothing," that's the gap. [See what a verifier-gated cascade catches that a benchmark-only comparison misses.](https://getnadir.com/blog/routerbench-cascade-benchmark)
  4. **Don't confuse a good shadow-test score with a permanent guarantee.** It's a statement about last week's traffic distribution, not next month's. [Start free](https://getnadir.com/auth?mode=signup) and let the check run on every request instead of a sampled window.


_Sources:[TianPan, "Releasing AI Features Without Breaking Production: Shadow Mode, Canary Deployments, and A/B Testing for LLMs," April 2026](https://tianpan.co/blog/2026-04-09-llm-gradual-rollout-shadow-canary-ab-testing). [Future AGI, "LLM Eval with Shadow Traffic and Canary Deployment in 2026"](https://futureagi.com/blog/llm-eval-shadow-traffic-canary-2026/)._
## Frequently asked questions What is the difference between shadow testing and canary deployment for LLMs?
    Shadow testing duplicates production traffic to a candidate model whose output is scored offline but never shown to users, typically doubling inference cost for one to two weeks. Canary deployment routes a small, growing percentage of real users to the candidate, starting around 1 percent and ramping through 5, 20, 50, and 100 percent, with automated rollback if quality or latency metrics degrade. Shadow proves the candidate behaves reasonably before anyone sees it; canary proves it holds up with real users in the loop. How long should a shadow test run before promoting a cheaper model?
    Most teams run shadow mode for one to two weeks to gather enough scored pairs across the real traffic distribution, then move into a canary ramp that holds each stage for at least 24 hours before advancing. High-stakes changes often extend the full shadow-to-100-percent window to three to six weeks; low-stakes changes can compress it to one to two weeks. What metrics should trigger an automatic rollback during a canary rollout?
    Common triggers include guardrail trip rate exceeding 1.5x the baseline within a 15-minute window, an offline rubric score regressing below the noise floor, p99 latency rising above 1.3x baseline, and error clusters appearing only on the candidate model that were not present in the incumbent's trailing week. Teams that automate these checks report rollback decisions in under a minute; teams that check manually often take hours. Does shadow testing actually cost extra money?
    Yes. Full shadow mode roughly doubles inference spend for the duration of the test, since every request is answered twice, once by the incumbent model that serves the user and once by the candidate that doesn't. Mirror sampling at 10 to 25 percent of traffic reduces that overhead while still generating enough scored pairs, for example roughly 100,000 pairs a day at a 10 percent mirror rate on 1 million daily requests, to catch most distribution-level regressions. What happens to model quality checks after a rollout is promoted to 100 percent?
    In most shadow-and-canary setups, nothing. The comparison pipeline that caught regressions during rollout stops running once the candidate is fully promoted, so a prompt pattern that falls outside the tested distribution, a new tool schema, a longer context, a user in a market the shadow sample under-represented, can fail silently in production with no equivalent safety net watching it. How is Nadir's verifier different from shadow or canary testing?
    Shadow and canary testing validate a policy on a bounded traffic window. Nadir keeps the decision receipt and outcome API available after rollout so teams can continue monitoring drift. Complete non-streaming proxy responses can optionally enter the verifier cascade; streaming responses bypass it, so the outcome loop remains necessary.
## More on routing & cascades
[See all →](https://getnadir.com/blog/category/routing)
  * [July 30, 2026We Beat Claude Opus 5 With Claude Opus 5Opus 5 is the most expensive model in our routing pool and it is also part of the answer, which is the whole trick behind beating it on HumanEval and MBPP. We scored four models on three datasets: Claude Opus 5, Claude Sonnet 5, qwen3.8-max-preview and MiniMax M3, across 1,268 arena chat prompts judged one to five, plus HumanEval and MBPP graded by executing the tests. Every cheap model lost to Opus 5 on chat. Sonnet 5 lost to Opus on all three. Then we put the same models behind Nadir, which runs the cheap model first, checks the answer, and escalates only when the check fails. Opus 5 stayed in the pool as the last resort and handled 5 of 395 coding problems, about 1% of the work. The combination scored 98.8% pass@1 on HumanEval against Opus 5 alone at 94.5%, 99.6% on MBPP against 98.7%, and 99.2% across both against 97.0%. It beat the frontier model because it keeps the cheap model's answer wherever the tests pass, including 8 problems Opus 5 got wrong, and falls back only where there is evidence of failure. Full per-dataset tables, the arms that lost, and every caveat that constrains the result.](https://getnadir.com/blog/tier-routing-25-percent-ceiling-cascade-verification)
  * [July 18, 2026New Model, Same BarMeta priced Muse Spark 1.1 at $1.25 input / $4.25 output per million tokens on July 9, a quarter of what Claude Opus 4.8 and GPT-5.5 charge. It is also cheaper than DeepSeek V4 on input. The instinct is to swap your cheap tier this afternoon. The last team that swapped a tenth-the-price model in wholesale saw 27% of answers come back clearly worse. Here is the five-step qualification protocol to run before any new model touches production traffic, and where Muse Spark 1.1 actually stands against it today.](https://getnadir.com/blog/meta-muse-spark-model-qualification-protocol)
  * [July 14, 2026LLM Routing Explained: How AI Model Routing Cuts CostsSending every request to the most capable model is the most expensive way to run an LLM product, and it is what almost everyone does by default. LLM routing is the layer that reads each request and picks the cheapest model that can still answer it well. This is the complete guide to how AI model routing works, the five architectures teams actually deploy, the one mistake that quietly makes routing worse than no routing at all, and how to add it in two lines of code.](https://getnadir.com/blog/llm-routing-guide)


Done reading?
### Put it to work on your traffic.
Try a task and see the model tier Nadir recommends. No account or payment needed.
[Try a task for free](https://getnadir.com/skill?src=blog_shadow-testing-canary-rollout-llm-model-swap#try)
