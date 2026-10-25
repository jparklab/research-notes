---
title: 'Source Analysis — Mooncake: A KVCache-centric Disaggregated Architecture for
  LLM Serving'
id: source-analysis-mooncake-a-kvcache-centric-disaggregated-architecture-for-llm-se
tags:
- internal-llm-inference-stacks-7f0c90
- source-analysis
created: '2026-10-09T23:19:26.615095Z'
updated: '2026-10-10T14:52:17.283401Z'
status: review
type: source-analysis
deprecated: false
summary: 'Digest of Moonshot AI''s Mooncake tech report: production evidence for Kimi''s
  KVCache-centric disaggregated serving platform (Conductor scheduler, Messenger RDMA
  cache-transfer, distributed CPU/DRAM/SSD KVCache pool), built atop vLLM but adding
  distributed caching the OSS version lacked, later open-sourced as kvcache-ai/Mooncake.
  Captures paired production measurements (23,608-request real trace, 34-55% cache
  hit rates vs. capacity, P90 TTFT 6.26s vs 92.07s random scheduling, 75% more requests
  served vs vLLM, up to 525% simulated throughput gain) and the overload/early-rejection
  mechanics (4,183 to 3,589 rejected requests across policies, HTTP 429, and the oscillation
  pathology naive rejection introduced).'
---

# Source Analysis — Mooncake: A KVCache-centric Disaggregated Architecture for LLM Serving

**Original source:** [[mooncake-a-kvcache-centric-disaggregated-2]]
**Source type:** paper (tech report / preprint, arXiv:2407.00079, v5, Moonshot AI + Tsinghua/MadSys)
**Source word count:** 12,154
**Your judgment:** The single richest first-person production account in this corpus — an operator (Moonshot AI) describing the actual serving platform behind a commercial, high-traffic chatbot (Kimi), with a real request trace, measured cache-hit tables, and admitted overload/rejection behavior. This is evidence of what one organization actually runs, not a description of what a system could theoretically do.

*Suggested by [[mooncake-a-kvcache-centric-disaggregated-2]] — source analyst's digest of the full source body*

## Thesis / Central claim
Mooncake is the production serving platform for Kimi (Moonshot AI's commercial LLM service). Its central architectural bet is that KVCache — not GPU compute — should be the organizing resource for scheduling: the system disaggregates prefill from decoding (as prior work had proposed) but goes further by disaggregating the KVCache itself into a separate pool that spans the CPU/DRAM/SSD of the GPU cluster, scheduled by a central component (Conductor) that treats cache location and reuse as first-class scheduling inputs alongside instance load and latency SLOs. The paper's second, less common thesis: because Moonshot runs chronically overloaded (demand growth outpaces GPU supply), request admission/rejection policy is as load-bearing as the serving architecture itself, and naive load-based rejection causes its own oscillation pathology that had to be fixed with prediction.

## Methodology / Basis of claims
- **System description** of the production architecture (Conductor scheduler, Messenger RDMA transfer component, paged KVCache pools), presented as what is actually deployed, not a proposal.
- **A real 1-hour production request trace** (23,608 entries) sampled from online Kimi traffic, open-sourced with fields `timestamp`, `input_length`, `output_length`, `hash_ids` (prefix-cache block identifiers) — explicitly stripped of text content for privacy, used both for an offline cache-policy analysis (Table 1) and for trace-replay experiments.
- **Trace-replay experiments** on a real cluster: "we built a Mooncake cluster consisting of 8 prefill instances and 8 decoding instances, using idle machines overnight, and replayed 23,000 real-world requests for the experiment" (§6.2); a separate overload experiment replays the same real trace at 2x speed on the same 8P+8D cluster (§8.2); and a larger real-workload comparison uses Mooncake-[10P+10D] vs vLLM-[20M] (§8.1.3).
- **Simulated/public-dataset benchmarking** against vLLM as baseline, using ArXiv Summarization and L-Eval datasets plus synthetic long-context data (16k/32k/64k/128k prompts), all run on "a dummy model that follows the same architecture as LLaMA2-70B" — the paper is explicit that proprietary model details are substituted to protect confidentiality: "all the experimental results reported in this paper are based on replayed traces of real workloads, but using a dummy model that follows the same architecture as LLaMA2-70B."
- Load-bearing assumption: the dummy-model substitution means absolute latency numbers are only representative of relative comparisons (Mooncake vs vLLM), not of Kimi's actual production latency with its real model.

## Key findings / Claims (with specific numbers where present)

1. **Headline production result:** "Compared to the baseline method, Mooncake can achieve up to a 525% increase in throughput in certain simulated scenarios while adhering to SLOs. Under real workloads, Mooncake's innovative architecture enables Kimi to handle 75% more requests" (abstract). The 525% figure comes from the simulated long-context experiment (§8.1.2, Figure 12: "enhancements ranging from 50% to 525%"); the 75% figure comes from the real-workload comparison (§8.1.3): Mooncake-[10P+10D] vs vLLM-[20M], where TTFT distributions were nearly identical ("almost 100% of requests meeting the TTFT SLO" for both) but TBT diverged sharply — "approximately 100% of the requests for Mooncake-[10P+10D] satisfy the TBT SLO" vs "only 57% of the requests for vLLM-[20M]."
2. **Request trace statistics (production conditions):** sampled from a 1-hour period of real Kimi traffic, 23,608 entries; average input length 8,590 tokens, average output length 182 tokens, average input/output ratio ≈ 720 — the paper frames this explicitly as reflecting "Kimi's renowned capability for superior long-context processing," not as a generic LLM workload.
3. **Cache hit-rate vs capacity (conditions-paired):** under a single global cache pool, LRU/LFU/LengthAwareCache were compared across capacities from 1,000 to unbounded blocks (Table 1). "Increasing the cache capacity from 1,000 to 50,000 blocks boosts the cache hit ratio from 34% to 55%. Further capacity increases show minimal improvement" — but the paper flags this as a property of the sampled 1-hour trace, not a ceiling: "this should not be interpreted as an indication that larger caches are unnecessary, as the sample trace represents only a subset of real-world workloads." LRU performed best of the three policies tested.
4. **Cache popularity skew:** "over 50% of cache blocks remaining unused while certain blocks are accessed tens of thousands of times" — the stated justification for the hot-spot replication scheme; without replication, "transfer congestion" on popular blocks becomes a bottleneck.
5. **Theoretical vs. realized cache reuse (stated as a correction to the field):** "the real reusability in our online traces is much smaller than the results reproduced by open-source benchmarks. Theoretically, up to only 55% of the KVCache can be reused in our current workloads, even if we assume both the capacity of storage and the TTFT SLO are infinite. However, this reusability highly depends on the application scenario and can be as large as 90% for certain scenarios, such as our chat-to-paper service https://papers.cool/." — this is a direct, named counterpoint to vendor/benchmark claims about prefix-cache hit rates elsewhere in this corpus.
6. **Prefill scheduling experiment (production-scale cluster, not simulation):** on an 8 prefill + 8 decoding instance cluster replaying 23,000 real requests, P90 TTFT was 6.26s for the full KVCache-centric scheduler vs 14.36s for cache-aware-only, 60.41s for load-balancing-only, and 92.07s for random scheduling (§6.2, Figure 8) — a roughly 15x spread between the best and worst scheduling policy on identical hardware and identical trace, isolating the scheduler's contribution.
7. **Overload/rejection numbers (2x-replay stress test on the 8P+8D cluster, 23,000-request trace):** baseline load-based rejection (reject before either stage starts) rejected 4,183 requests; "Early Rejection" (decoding load checked before prefill begins) rejected 3,771; "Early Rejection based on Prediction" rejected 3,589 (Table 3). The paper's claim is that these reductions represent recovered effective throughput, not softer admission: "by rejecting requests early, Mooncake can avoid unnecessary prefill computations, thereby improving the effective utilization of system resources."
8. **The oscillation pathology of naive early rejection:** deploying simple Early Rejection on a real 20-machine cluster produced "significant anti-phase fluctuations between prefill and decoding machines" over a measured 20-minute window (§7.3, Figure 9), worse "in clusters with fewer prefill machines and in scenarios where the prefill stage takes longer" — root-caused to the time lag between when decoding load is measured and when it is actually realized. This is described as a second-order problem that only appears once rejection logic exists, and required a dedicated fix (prediction-based rejection, §7.4).
9. **Rejection mechanics:** Conductor evaluates admission "based on the greater load between the prefill and decoding pools" at request arrival (Early Rejection); if neither TTFT nor TBT SLO is achievable, "Conductor directly returns the HTTP 429 Too Many Requests response status code to the upper layers" — a concrete, citable mechanism (standard HTTP status code) for how overload is surfaced to callers.
10. **Definition of "goodput" used for capacity accounting:** "only requests that fully complete their execution are counted in the measure of goodput. Otherwise, all previously consumed/generated tokens are not counted, and the corresponding resources are wasted" — this is the stated rationale for why early/predictive rejection matters: a request accepted into prefill and then rejected at decoding wastes 100% of its prefill compute.
11. **Architecture is explicit prefix-cache-exact-match, not approximate:** KVCache blocks are identified by chained prefix hashes (block size 512 tokens): "F=Hash(E+f)" etc., so identical hash IDs guarantee token-identical prefixes, enabling exact dedup/reuse rather than fuzzy matching.
12. **Why they kept disaggregation despite chunked-prefill being a simpler alternative:** §5 states the field debate directly and gives Moonshot's answer: inlining chunked prefill into the decoding batch is simpler to schedule and improves MFU, "however, after careful consideration, we decided to maintain Mooncake's disaggregated architecture" for two reasons — prefill nodes need different cross-node parallelism for long context (§5.1), and disaggregation "presents a unique opportunity to save VRAM" via layer-wise async KVCache streaming (§5.2).
13. **Chunked Pipeline Parallelism (CPP) as a bespoke alternative to sequence parallelism:** rather than adopt elastic sequence parallelism (which the paper says other research [14, LoongServe] proposed), Mooncake built CPP — "to our knowledge, this is the first application in the inference stage" of pipeline-style parallelism for prefill — specifically to avoid the operational complexity of dynamically resizing SP groups under Conductor's scheduling.
14. **Layer-wise async KVCache store/load measurement:** Figure 7 shows storing-latency for layer-wise vs serialized KVCache transfer across sequence lengths 8k–128k; the paper's conclusion is only qualitative in the extracted text ("layer-wise prefill can effectively reduce the latency for long-context requests") — exact curve values are in the figure, not the prose, so treat as directionally confirmed but not independently quotable as a number.

## Load-bearing citations / sources this source depends on
- **Splitwise** (Patel et al., arXiv:2311.18677) — cited as the earliest work motivating prefill/decode separation; Mooncake explicitly credits it: "The arXiv publication of Splitwise is at the early stage of the development of Mooncake, which further motivated our progress."
- **DistServe** (Zhong et al., arXiv:2401.09670) and **TetriInfer** (Hu et al., arXiv:2401.11181) — named as concurrent prefill/decode disaggregation work; Mooncake positions itself as going further by also disaggregating the cache itself, not just the compute stages.
- **vLLM** (Kwon et al., SOSP 2023, PagedAttention) — used as the direct experimental baseline throughout §8, and credited as an intellectual debt: "Our design of Mooncake builds on these developments, particularly drawing from the open-source community of vLLM, for which we are deeply appreciative." The paper also makes a precise technical claim about vLLM's limits: "Similar reuse logic is already implemented in vLLM, but the open-source version of vLLM only supports local KVCache caching" (i.e., no distributed/disaggregated cache pool) — this is a specific, checkable claim about an OSS project's capability ceiling at the time of writing.
- **AttentionStore** (Gao et al., arXiv:2403.19708) — named as the closest concurrent/competing architecture: "The architecture of Mooncake shares many design choices with AttentionStore," distinguished by Mooncake's claim to added global KVCache-centric scheduling on top of the cache-storage mechanism.
- **SGLang / RadixAttention** (Zheng et al., arXiv:2312.07104) and **Prompt Cache** (Gim et al., arXiv:2311.04934) — cited as alternative prefix-caching approaches (radix-tree LRU vs Mooncake's exact chained-hash matching).
- **Preble** (Srivatsa et al.) — cited as corroborating work on KVCache-centric/prompt scheduling, against which Mooncake explicitly contrasts its own measured cache-reuse ceiling (55% theoretical vs higher published numbers).
- **LoongServe** (Wu et al., arXiv:2403.02310 is Sarathi — note LoongServe is [14] in refs, elastic sequence parallelism) — named as the source of the elastic-SP idea that Mooncake considered and rejected as too complex for Conductor.
- **The open-sourced request trace and code**: the paper states "The trace is open-sourced at https://github.com/kvcache-ai/Mooncake" — this is the origin point of the `kvcache-ai/Mooncake` GitHub repository. A sibling vault note's summary (on this same source) reports the repo has since grown to ~6.6k GitHub stars and that its `MooncakeStoreConnector` component has since been integrated into the llm-d project and is referenced by vLLM/SGLang KV-cache tooling — this analyst did not independently verify current star count or the llm-d integration claim from primary sources and flags it for the orchestrator to confirm against a dedicated GitHub/llm-d source if load-bearing for the draft.

## Caveats, limitations, contradictions
- **Deliberate obfuscation of the production model and absolute latencies.** The authors state outright: "To protect proprietary information and facilitate reproducibility, all the experimental results reported in this paper are based on replayed traces of real workloads, but using a dummy model that follows the same architecture as LLaMA2-70B." This means every absolute latency/throughput number in §8 is a dummy-model proxy, not Kimi's real production figure — only relative comparisons (Mooncake vs vLLM under matched conditions) are safe to cite as real. The trace timing, input/output lengths, and hash relationships are real; the "content" and the model computing on them are not.
- **The scheduling threshold is manually tuned, not learned:** explicitly flagged by the authors: "This threshold is currently adjusted manually, but can be adaptively adjusted by an algorithm in the future" (§6.2, footnote) — a concrete admission that a core load-balancing parameter (the cache-vs-transfer tradeoff threshold) is a hand-set knob in production, not an auto-tuned one at the time of writing.
- **Static prefill/decode instance ratio:** the paper admits the P/D instance split is preset and only "minor temporary imbalances" are tolerated, deferring "more flexible deployment and conversion methods" to future work — i.e., in production as described, capacity is not dynamically rebalanced between the two pools in real time.
- **The cache-hit-rate ceiling is explicitly caveated as not generalizable:** "this should not be interpreted as an indication that larger caches are unnecessary, as the sample trace represents only a subset of real-world workloads" — the 55% number is a property of a 1-hour, 23,608-request slice, not a claimed universal constant, and the paper separately notes a different internal service reaches up to 90% reuse.
- **Prediction-based rejection is only "system-level," not request-level:** the authors explicitly defer the harder problem: "The exploration of request-level prediction is left for future work," because per-request output-length prediction is "challenging due to high costs... or low accuracy, especially under overload conditions" — so even the improved rejection policy described is an approximation (uniform decoding-time assumption td), not a precise model.
- **Self-reported engineering success, no independent audit.** All production claims (75% more requests handled, cache-hit figures, rejection counts) are Moonshot's own account; there is no third-party verification in this document. The paper is candid about its own tradeoffs (manual thresholds, static P/D ratios, approximation in prediction) which somewhat offsets this, but the register is still "vendor/operator self-report," not independent benchmark.
- Internal contradiction risk for downstream drafting: the abstract's "75% more requests" and "525% throughput increase" describe two different experiments under two different conditions (real trace vs. simulated long-context data) — they must not be conflated into a single number when cited.

## Relevance to research_query
This source is near-maximally load-bearing for the research query's three asks simultaneously: (1) **tech stack as an operator's decision** — Mooncake shows a company running its own commercial LLM product (Kimi) choosing a custom KVCache-centric disaggregated architecture (Conductor scheduler + Messenger RDMA transfer + distributed CPU/DRAM/SSD cache pool) specifically because the available open-source serving stack (vLLM, at the time) "only supports local KVCache caching" and couples prefill/decode in a way that disrupts long-context decoding; (2) **open-source leverage and what gets custom-built** — the paper is explicit that vLLM's PagedAttention and continuous batching are the foundation Mooncake builds on and credits ("drawing from the open-source community of vLLM"), while the distributed cache pool, Conductor's cache-aware global scheduling, CPP multi-node prefill, layer-wise async KVCache streaming, and the overload/early-rejection prediction logic are all Moonshot-built additions layered on top — and critically, Mooncake itself became an open-source release after the fact (the trace and subsequently the serving stack under `kvcache-ai/Mooncake`), which is itself a direct data point for the query's interest in "where open source solutions come from": here, OSS is the internal system released outward, not a community tool pulled inward; (3) **overload/rejection behavior at high utilization** — §7 and §8.2 are the most detailed, numerically grounded account in this corpus of what a saturated internal inference platform actually does: it defines load via SLO-satisfaction rather than raw occupancy, implements early rejection to avoid wasting completed prefill compute, discovers and fixes an oscillation pathology that naive rejection introduces, and reports the quantified effect (4,183 → 3,771 → 3,589 rejected requests across three policies on an identical real-trace replay) — directly informing any argument in the draft about running internal inference capacity near its ceiling rather than over-provisioning.

## Extracted quotes

> "Mooncake is the serving platform for Kimi, a leading LLM service provided by Moonshot AI. It features a KVCache-centric disaggregated architecture that separates the prefill and decoding clusters."

Opening framing of the paper — establishes this is a deployed production system, not a proposal.

> "Unlike traditional studies that assume all requests will be processed, Mooncake faces challenges due to highly overloaded scenarios. To mitigate these, we developed a prediction-based early rejection policy."

States directly why existing systems/research were insufficient for Moonshot's actual operating conditions.

> "Similar reuse logic is already implemented in vLLM, but the open-source version of vLLM only supports local KVCache caching."

Precise, checkable claim about the specific gap in the OSS baseline that motivated building a distributed cache pool.

> "In real deployments, we set fixed SLOs of TTFT and TBT. If monitoring detects unmet SLOs, we either add inference resources or reject some incoming requests. However, due to the current contingent supply of GPUs, elastically scaling out the inference cluster is typically unfeasible."

Direct statement that capacity constraints, not just architecture preference, force the rejection-policy work.

> "Conductor directly returns the HTTP 429 Too Many Requests response status code to the upper layers."

Concrete, citable mechanism for how overload surfaces to API callers.

> "the real reusability in our online traces is much smaller than the results reproduced by open-source benchmarks. Theoretically, up to only 55% of the KVCache can be reused in our current workloads, even if we assume both the capacity of storage and the TTFT SLO are infinite."

A named correction to benchmark-derived expectations about prefix-cache hit rates — useful as a counterweight to vendor claims elsewhere in the corpus.

> "To protect proprietary information and facilitate reproducibility, all the experimental results reported in this paper are based on replayed traces of real workloads, but using a dummy model that follows the same architecture as LLaMA2-70B."

Essential caveat for any downstream citation of this paper's absolute performance numbers.

> "Our design of Mooncake builds on these developments, particularly drawing from the open-source community of vLLM, for which we are deeply appreciative."

Direct acknowledgment of OSS dependency/reuse.

> "This threshold is currently adjusted manually, but can be adaptively adjusted by an algorithm in the future."

Honest admission of a hand-tuned production parameter — relevant to "custom vs. automated" framing.

> "by rejecting requests early, Mooncake can avoid unnecessary prefill computations, thereby improving the effective utilization of system resources. Furthermore, by predicting the load of decoding instances, Mooncake can mitigate load fluctuations, increasing the request handling capacity."

Summary statement tying the rejection policy directly to capacity/utilization outcomes.
