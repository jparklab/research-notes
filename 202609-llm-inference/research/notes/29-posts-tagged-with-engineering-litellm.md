---
title: 29 posts tagged with "engineering" | liteLLM
id: 29-posts-tagged-with-engineering-litellm
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- gateway
- operator-evidence
created: '2026-10-10T10:56:59.663848Z'
updated: '2026-10-10T14:52:05.370937Z'
source: https://docs.litellm.ai/blog/tags/engineering
source_domain: docs.litellm.ai
fetched_at: '2026-10-10T10:56:59.638218Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'LiteLLM''s own engineering blog index: carries the joint LiteLLM/Pfizer
  debugging account of a Redis configuration bug that cut Pfizer''s gateway throughput
  ~48% with zero HTTP errors, plus named production users (Netflix, Lemonade, Rocket
  Money) and operating notes for running the proxy across 100+ pods.'
utility_score: 15.0
---

[Skip to main content](https://docs.litellm.ai/blog/tags/engineering#__docusaurus_skipToContent_fallback)
[**Decisions API is here.** Call Jev, Clef or any decision model through one /v1/decisions endpoint. →](https://docs.litellm.ai/docs/decisions)
A research agent can finish a report after reading the first 8,000 characters of each page. If its web tool cuts off a source and the agent fills in the gaps, you get a completed run with unsupported claims.
Across thousands of runs, you need to find the affected sessions and what they have in common. [LiteLLM Lens](https://docs.litellm.ai/blog/litellm-lens-launch) uses agents to do that work.
Here's how it works behind the scenes: reviewers inspect executions in parallel, grouping agents connect related observations, and investigators check each candidate pattern against the original traces.
LiteLLM was counting every token in a long conversation to answer a yes-or-no routing question.
Removing that unnecessary work took median time to first byte from **553 ms to 35 ms** in our local 440k-token benchmark: **94% lower**.
Published: Oct 3, 2026
[**Tin Lo**](https://www.linkedin.com/in/tinchi-lo/) AI Product Engineer, LiteLLM
At BerriAI, we built **Moyai Devin** , an internal engineering agent that runs in the cloud. Teammates can give it a task in Slack, follow its progress in a web app, and ask it to prepare a pull request.
This week, we cut the number of times a request to the LiteLLM proxy waits on Redis from **22 to 8**.
A request from a key with a budget, in a team with a budget and TPM/RPM limits, against a model group with usage-based routing and a Redis response cache, made 22 Redis round trips: 12 before the model was called and 10 after. The same request, with the same checks and the same writes, now makes **5 before and 3 after**.
This week, we made the LiteLLM Usage page roughly 120x faster.
With 5,000 API keys, our Usage page took **over six minutes** to show 30 days of totals. We redesigned how it loads data and brought that down to **3.2 seconds** in our benchmark. That's roughly **120x faster**.
_Last Updated: September 18, 2026_
An Auto Router pays for classification before the selected model can answer. In our benchmark, TypeSafe JEV classified requests **5.43x as fast as Haiku** , comparing median classifier latency: **126.81 ms versus 688.40 ms**. Registry-priced classifier cost was **96.12% lower** , rounded to 96% in the title
JEV matched our benchmark's expected tiers on 95.00% of calls, versus 73.75% for Haiku. That result depends on the prompts, tier definitions, instructions and context used here. The expected tiers were authored with the synthetic prompts, without independent review. This comparison does not establish general classification accuracy or the quality of the final answers
Auto-Router can move a conversation between tiers as the request changes from simple work to a harder task. Responses API clients can make that switch difficult because a previous response may include encrypted reasoning content that only the deployment that created it can decrypt
LiteLLM now keeps the readable history and removes encrypted reasoning that the newly selected tier cannot verify. The request can continue to the selected model instead of failing with `invalid_encrypted_content`
This fix is included in the LiteLLM `v1.102.x` release line and landed in [PR #40280](https://github.com/BerriAI/litellm/pull/40280)
Predict cache costs, set speed by tier, let teams manage routers, and see the chosen model and savings in your CLI.
A joint debugging story with LiteLLM, and the release testing that comes next.
A LiteLLM version bump exposed a long-standing Redis configuration bug that cut Pfizer's gateway throughput by ~48%, with zero HTTP errors in the application logs. Here's how the team isolated it through configuration bisection, and the testing infrastructure both teams are building to catch this class of regression before it ships.
We've updated the Auto-Router to account for how Claude Code and Codex package requests. It now removes more harness context from classification, preserves encrypted delegated tasks, and shows exactly what the classifier received
Heuristic v1 scores seven prompt signals, including reasoning language, code, technical terms, and prompt length. You can tune those signals for the traffic your router serves.
**An experimental router that picks a model per agent phase matched fixed Claude Opus-5 quality on a SWE-bench Verified subset for 46% less money.**
**The complexity router's LLM classifier can now be compressed more aggressively than your model calls. In internal testing, that cut classification costs a further 32% beyond what shared compression was already saving, with no change in routing accuracy.**
**Heuristic v2, LiteLLM's new AutoRouter classifier, is up to 45% more efficient than Heuristic v1: more tasks solved, at lower cost, in less time.** Only the classifier changed.
**LiteLLM Auto Router Fusion solved 14 of 21 Terminal-Bench tasks; Claude Fable-5 on its own solved 9.** Fusion runs the task on several models in parallel and has one of them synthesize the candidate work into a single answer. Both arms ran the same 21 tasks.
The Auto-Router picks a model by asking how hard the request is. This release adds the questions that come right after: does the request fit the model we picked, and can that model even see it?
We have shown the Auto-Router [saving 51% in production](https://docs.litellm.ai/blog/auto-router-production-savings) and [69% stacked on prompt caching](https://docs.litellm.ai/blog/auto-router-prompt-caching-benchmark). The question we hear next is always the same: **"would it hold quality on my traffic?"**
You can expect roughly 40% cost reductions from day one with the Auto Router, and more as the tier maps are tuned. One of our production users shared their statistics to show what that looks like at scale.
They rolled out the Auto-Router to 450+ users across dev, staging, and prod instances and saved **$12,249 over 270k+ requests**.
**An auto router matched Claude Opus-5 solve rate on a 21 task subset of Terminal-Bench 2.0 at 27% lower cost.** Every arm ran the same 21 tasks, so the comparisons are like for like.
We've made it easier than ever to setup and test your Auto-Router, and with customizable tier names + classifier system prompts you can go beyond complexity routing.
**Yes, you can use prompt caching with Auto-Routing.** The two compound rather than cancel out. We measured it across five datasets, two of which report what the provider's cache actually did.
Auto routing promises a smaller bill without a worse answer. We measured both halves against a baseline that sends every request to `claude-opus-5`: 8,619 graded prompts and cost simulations over 14,000 real conversations.
_Last Updated: July 2026_
We are launching an early beta of the LiteLLM AI Gateway in Rust, and we built [AIGatewayBench](https://github.com/BerriAI/ai-gateway-bench) to measure it against Portkey, Bifrost, and the current LiteLLM Python proxy. Across all four, the LiteLLM Rust gateway has the **lowest p99 added latency and the smallest memory footprint by a wide margin** : roughly `7x` lower overhead and `9x` less memory than the next-closest gateway (Bifrost), the lowest cost footprint, and the fastest whole-session times for coding agents. It holds its own on raw sustained throughput and pulls decisively ahead on overhead, memory, and cost.
_Last Updated: June 2026_
Over the past year, we have heard the same thing from our users and our community: they want the fastest, most lightweight AI gateway they can run. We have heard you. We are addressing it by moving LiteLLM to Rust, and committing to sub-`1ms` overhead with a sub-`100MB` memory binary you can deploy. By the end of this migration, you will get a pure Rust server that can serve 100% of your AI traffic, with every hot path operation, including auth and rate limiting, running in Rust.
Want to help us build it?
We are opening an early beta and want to work directly with teams who care about a fast, lightweight gateway. If that is you, [sign up here](https://docs.google.com/forms/d/e/1FAIpQLSecWdOjkzjEson2UiZpDftOoZPs8RQbtlAM40KSvDXZqEgYaA/viewform?usp=dialog) and we will get you testing the Rust gateway in your own stack, with a direct line to our team.
The reason it matters: under real load, CPU and memory climb with concurrency, and pods get OOM-killed at the worst time. Today the LiteLLM Python proxy peaks around `359MB` of memory under load, and that cost multiplies across every pod, region, and retry you run.
We are already seeing the payoff in benchmarks. The Rust gateway serves about `15x` the throughput (`453` to `6,782` requests per second) on about `11x` less memory (`359MB` to `32MB`), and cuts per-request overhead from about `7.5ms` on the Python path to about `0.05ms`, well under the `1ms` we commit to.
## What you get[​](https://docs.litellm.ai/blog/tags/engineering#what-you-get "Direct link to What you get")
You deploy a single Rust binary. It uses about `65MB` of memory, gateway overhead stays under `1ms`, and nothing in your setup changes: same `config.yaml`, same database, same client API, same providers. You keep LiteLLM's coverage of 100+ LLM providers behind one OpenAI-compatible API, with `/chat/completions`, `/messages`, `/responses`, and every other LLM endpoint LiteLLM supports today, now as the fastest and most lightweight LLM gateway you can self-host.
This is not a v2 and not a rewrite. There is no new major version to migrate to and nothing for you to change. The runtime under the hot path gets faster and lighter while your config stays exactly where it is.
We ship this the careful way. Each route moves to Rust only after it passes our full parity and end-to-end test suite, and it runs in production before the next route starts. Stability is the priority, and we target zero regressions on every release.
info
The platform we built is open source. Check out [litellm-agent-platform](https://github.com/BerriAI/litellm-agent-platform). The swappable harness layer is [lite-harness](https://github.com/LiteLLM-Labs/lite-harness).
Building the same thing inside your company?
  * [Schedule a 30-minute call](https://calendly.com/d/cr4t-yp7-pzn/litellm-1-1-feedback-chat)
  * [Join the LAP Discord](https://discord.gg/Q2AK7HKudm)


Our goal was to 10x the productivity of our company with agents.
Three weeks ago we began building an agent that could own 30% of our engineering tickets. Here's what we've learnt so far.
_Last Updated: April 2026_
Enterprise AI Gateway deployments put Redis in the hot path for nearly every request: rate limiting, cache lookups, spend tracking. When Redis is healthy, the latency contribution is single-digit milliseconds, invisible to end users. When it degrades, a production AI Gateway needs to stay up regardless.
Running LiteLLM at scale across 100+ pods means designing for failure modes before they appear. The easy case is Redis going fully down: fail fast, fall through to the database, continue serving requests. The hard case, the one that takes down gateways, is a _slow_ Redis: still accepting connections, still responding, but timing out after 20-30 seconds per operation.
The CI/CD v2 is now live for LiteLLM.
