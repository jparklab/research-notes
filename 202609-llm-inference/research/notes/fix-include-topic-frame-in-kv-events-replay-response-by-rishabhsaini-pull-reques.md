---
title: 'fix: include topic frame in KV events replay response by RishabhSaini · Pull
  Request #45177 · vllm-project/vllm · GitHub'
id: fix-include-topic-frame-in-kv-events-replay-response-by-rishabhsaini-pull-reques
tags:
- llmd-kv-cache-routing-c665d5
- locus-index-freshness-and-recovery
- vllm-pr
- kv-event-replay-buffer
- merged
created: '2026-09-14T12:40:58.307817Z'
updated: '2026-09-15T04:55:24.122151Z'
source: https://github.com/vllm-project/vllm/pull/45177
source_domain: github.com
fetched_at: '2026-09-14T12:40:58.281034Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'vLLM PR #45177 (''fix: include topic frame in KV events replay response'',
  author RishabhSaini) adds a topic field to replay ROUTER responses so the wire format
  matches live PUB messages ([topic, seq, payload]), and switches test replay sockets
  from REQ to DEALER for multi-message replay with strict assertions. The PR was MERGED
  by maintainer tlrmchlsmth as commit 2f3f441f84bd5b35ec8aa9fcfffb540f107da8a7 into
  vllm-project:main on July 7, 2026, after 84 CI checks passed; the source branch
  (fix/replay-buffer-include-topic) was deleted post-merge. It carries the ''ready''
  and ''documentation'' labels. Two downstream llm-d PRs reference this fix as a dependency
  for KV-event replay-buffer index recovery: llm-d/llm-d-router#1887 (''feat: replay
  buffer consumer for KV event index recovery'', merged) and llm-d/llm-d#2196 (''feat(precise):
  enable KV event replay buffer for index recovery'', merged) -- both mentioned by
  RishabhSaini in mid-2026 and both shown as Merged. A third related PR, llm-d/llm-d-kv-cache#691,
  was mentioned but shown as Closed (not merged).'
---

[Skip to content](https://github.com/vllm-project/vllm/pull/45177#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm/pull/45177) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm/pull/45177) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm/pull/45177) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/pull/45177).
/ Public
  * Sponsor
#  Sponsor vllm-project/vllm 
##### GitHub Sponsors
[Learn more about Sponsors](https://github.com/sponsors)
[ vllm-project ](https://github.com/vllm-project)
[ vllm-project ](https://github.com/vllm-project)
[ Sponsor  ](https://github.com/sponsors/vllm-project)
##### External links
[opencollective.com/**vllm**](https://opencollective.com/vllm)
[Learn more about funding links in repositories](https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/displaying-a-sponsor-button-in-your-repository). 
[Report abuse](https://github.com/contact/report-abuse?report=vllm-project%2Fvllm+%28Repository+Funding+Links%29)
  * [ Notifications ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm) You must be signed in to change notification settings
  * [ Fork 22.2k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm)
  * [ Star  91.7k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm)


## Conversation
### 
commented [Jun 10, 2026](https://github.com/vllm-project/vllm/pull/45177#issue-4633714723) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/pull/45177).
Copy link  Copy Markdown 
Contributor
## Summary
  * Add `self._topic_bytes` to replay ROUTER responses so the wire format matches live PUB messages: `[topic, seq, payload]`
  * Switch test `MockSubscriber` replay socket from REQ to DEALER so all replayed events are received (REQ only receives one reply)
  * Add `test_replay_includes_topic` verifying replay format matches PUB
  * Tighten existing replay test assertions to exact counts and sequences


## Context
The replay buffer in `ZmqEventPublisher` stores `(seq, payload)` but the live PUB path sends `[topic, seq, payload]`. When a subscriber requests replay via the ROUTER socket, the response omitted the topic — which contains the pod identifier and model name (e.g. `kv@<podIP>@<modelName>`).
External consumers like [llm-d's GAIE indexer](https://github.com/llm-d/llm-d-kv-cache) need the topic to route replayed events to the correct index. Without it, the replay mechanism cannot be used to recover state after a disruptive event.
Related: [llm-d/llm-d-kv-cache#372](https://github.com/llm-d/llm-d-kv-cache/issues/372)
## Test plan
  * `pytest tests/distributed/test_events.py` — all 10 tests pass
  * `test_replay_includes_topic` verifies topic presence with exact event counts (6 events replayed, sequences 0-5)
  * `test_replay_mechanism` tightened to assert exactly 10 events
  * `test_buffer_limit` tightened to assert exactly buffer_size events


AI-assisted: Claude was used for code exploration and drafting.
[RishabhSaini](https://github.com/RishabhSaini) requested a review from [markmc](https://github.com/markmc) as a [code owner](https://github.com/vllm-project/vllm/blob/bfe1001ab6d59d7da1f70a3bc107cebaff91f925/.github/CODEOWNERS#L130) [June 10, 2026 17:59](https://github.com/vllm-project/vllm/pull/45177#event-26588063027)
### 
commented [Jun 10, 2026](https://github.com/vllm-project/vllm/pull/45177#issuecomment-4672969669)
Copy link  Copy Markdown 
Contributor  
|  Documentation preview: <https://vllm--45177.org.readthedocs.build/en/45177/>  |  
| --- |  
[mergify](https://github.com/apps/mergify) Bot added the [ documentation ](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3Adocumentation) Improvements or additions to documentation label [Jun 10, 2026](https://github.com/vllm-project/vllm/pull/45177#event-26588087977)
reviewed  [ Jun 11, 2026 ](https://github.com/vllm-project/vllm/pull/45177#pullrequestreview-4476192128)
[ View reviewed changes ](https://github.com/vllm-project/vllm/pull/45177/files)
Comment thread [vllm/distributed/kv_events.py](https://github.com/vllm-project/vllm/pull/45177/files#diff-790ea324bb98482abe733e79e2df158cf99bbdcaacf3304e53ae57127b5b2dc4) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/pull/45177).
[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Jun 12, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-issue-4006316344)
[ Recover kvBlock.Index from a disruptive event llm-d/llm-d-kv-cache#372 ](https://github.com/llm-d/llm-d-kv-cache/issues/372)
### 
commented [Jun 15, 2026](https://github.com/vllm-project/vllm/pull/45177#issuecomment-4713040044)
Copy link  Copy Markdown 
Contributor Author  
|  /assign [@markmc](https://github.com/markmc)  |  
| --- |  
[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Jun 24, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-issue-4736343642)
[ Detect missed KV events via sequence gap and request replay llm-d/llm-d-router#1737 ](https://github.com/llm-d/llm-d-router/issues/1737)
approved these changes  [ Jun 30, 2026 ](https://github.com/vllm-project/vllm/pull/45177#pullrequestreview-4601545732)
[ View reviewed changes ](https://github.com/vllm-project/vllm/pull/45177/files)
[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Jun 30, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-pullrequest-4778386364)
[ feat: replay buffer consumer for KV event index recovery llm-d/llm-d-kv-cache#691 ](https://github.com/llm-d/llm-d-kv-cache/pull/691)
Closed 
2 tasks
reviewed  [ Jun 30, 2026 ](https://github.com/vllm-project/vllm/pull/45177#pullrequestreview-4604609826)
[ View reviewed changes ](https://github.com/vllm-project/vllm/pull/45177/files)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks for the fix [@RishabhSaini](https://github.com/RishabhSaini). Could you add a test for this, ideally one that demonstrates how this is supposed to be used 🙏
[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/vllm-project/vllm/compare/26f6a97ef2f96a0a93be854589e816b7ef10bf01..5eff1373a21a7f5c83a65b8809fd70951046b421) the  fix/replay-buffer-include-topic branch from [`26f6a97`](https://github.com/vllm-project/vllm/commit/26f6a97ef2f96a0a93be854589e816b7ef10bf01) to [`5eff137`](https://github.com/vllm-project/vllm/commit/5eff1373a21a7f5c83a65b8809fd70951046b421) [ Compare ](https://github.com/vllm-project/vllm/compare/26f6a97ef2f96a0a93be854589e816b7ef10bf01..5eff1373a21a7f5c83a65b8809fd70951046b421) [July 2, 2026 14:43](https://github.com/vllm-project/vllm/pull/45177#event-27484839148)
[RishabhSaini](https://github.com/RishabhSaini) requested a review from [tlrmchlsmth](https://github.com/tlrmchlsmth) [July 2, 2026 14:46](https://github.com/vllm-project/vllm/pull/45177#event-27484949801)
approved these changes  [ Jul 6, 2026 ](https://github.com/vllm-project/vllm/pull/45177#pullrequestreview-4638535094)
[ View reviewed changes ](https://github.com/vllm-project/vllm/pull/45177/files)
Comment thread [vllm/distributed/kv_events.py](https://github.com/vllm-project/vllm/pull/45177/files#diff-790ea324bb98482abe733e79e2df158cf99bbdcaacf3304e53ae57127b5b2dc4) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/pull/45177).
[tlrmchlsmth](https://github.com/tlrmchlsmth) added the [ ready ](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3Aready) ONLY add when PR is ready to merge/full CI is needed label [Jul 6, 2026](https://github.com/vllm-project/vllm/pull/45177#event-27624104353)
### 
commented [Jul 6, 2026](https://github.com/vllm-project/vllm/pull/45177#issuecomment-4896175387)
Copy link  Copy Markdown 
Contributor  
|  Hi [@RishabhSaini](https://github.com/RishabhSaini), the pre-commit checks have failed. Please run: 
```
uv pip install pre-commit=4.5.1
pre-commit install
pre-commit run --all-files
```
Then, commit the changes and push to your branch. For future commits, `pre-commit` will run automatically on changed files before each commit.  |  
| --- |  
` fix: include topic frame in KV events replay response[](https://github.com/vllm-project/vllm/pull/45177/commits/764ff0c37f84f40dc2502e3c6ad8f217b9baef26 "fix: include topic frame in KV events replay response

Add topic to replay ROUTER responses so the wire format matches
live PUB messages: \[topic, seq, payload\]. Switch test replay
sockets from REQ to DEALER for multi-message replay with strict
assertions.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/pull/45177).

```
Add topic to replay ROUTER responses so the wire format matches
live PUB messages: [topic, seq, payload]. Switch test replay
sockets from REQ to DEALER for multi-message replay with strict
assertions.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/vllm-project/vllm/compare/09af77662b7710bc25c3b4cb45f67099fd64d293..764ff0c37f84f40dc2502e3c6ad8f217b9baef26) the  fix/replay-buffer-include-topic branch from [`09af776`](https://github.com/vllm-project/vllm/commit/09af77662b7710bc25c3b4cb45f67099fd64d293) to [`764ff0c`](https://github.com/vllm-project/vllm/commit/764ff0c37f84f40dc2502e3c6ad8f217b9baef26) [ Compare ](https://github.com/vllm-project/vllm/compare/09af77662b7710bc25c3b4cb45f67099fd64d293..764ff0c37f84f40dc2502e3c6ad8f217b9baef26) [July 7, 2026 16:27](https://github.com/vllm-project/vllm/pull/45177#event-27674288692)
Hide details View details
[tlrmchlsmth](https://github.com/tlrmchlsmth) merged commit [`2f3f441`](https://github.com/vllm-project/vllm/commit/2f3f441f84bd5b35ec8aa9fcfffb540f107da8a7) into  vllm-project:main [Jul 7, 2026](https://github.com/vllm-project/vllm/pull/45177#event-27679991424)
84 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/pull/45177).
[RishabhSaini](https://github.com/RishabhSaini) deleted the  fix/replay-buffer-include-topic  branch [July 7, 2026 18:48](https://github.com/vllm-project/vllm/pull/45177#event-27680004066)
[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Jul 14, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-pullrequest-4806234113)
[ feat: replay buffer consumer for KV event index recovery llm-d/llm-d-router#1887 ](https://github.com/llm-d/llm-d-router/pull/1887)
Merged 
5 tasks
[NickLucche](https://github.com/NickLucche) pushed a commit to NickLucche/vllm that referenced this pull request [ Jul 15, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-commit-d0fed7e)
` fix: include topic frame in KV events replay response ([](https://github.com/NickLucche/vllm/commit/d0fed7e5a1f3399bac486e4f0a92ac4f7092f539 "fix: include topic frame in KV events replay response \(#45177\)

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>")vllm-project#4…[](https://github.com/vllm-project/vllm/pull/45177) `

```
…5177[](https://github.com/vllm-project/vllm/pull/45177))

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[lcskrishna](https://github.com/lcskrishna) added a commit to lcskrishna/vllm that referenced this pull request [ Jul 15, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-commit-ba82f01)
Partially verified
#  Partially verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
We cannot verify signatures from co-authors, and some of the co-authors attributed to this commit require their commits to be signed. 

```
* AGENTS MD: Add suggestion on how to incorporate tests (#47784)

Signed-off-by: Simon Mo <simon.mo@hey.com>
Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Benjamin Chislett <chislett.ben@gmail.com>

* [BugFix][LoRA] Refresh punica metadata when LoRA slots are reassigned under an unchanged mapping (#47725)

Signed-off-by: AmeenP <ameenp360@gmail.com>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [Bug] Fix tmp directory for `lm_eval` (#47755)

Signed-off-by: yewentao256 <zhyanwentao@126.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [XPU] Fix topk_sigmoid arg mismatch on XPU (#47858)

Signed-off-by: zhenwei-intel <zhenwei.liu@intel.com>

* [Doc] Fix grammatically incorrect error message in gpu_worker and xpu_worker (#36715)

Signed-off-by: Hongbin10 <jdmjdm1998@163.com>

* [Perf][3/N] Expand Triton kernel warmup coverage, Qwen (#47546)

Signed-off-by: LopezCastroRoberto <rocastro@redhat.com>

* Upgrade tpu-inference to v0.24.0 (#47835)

Signed-off-by: StingLin <sting.lin@cienet.com>

* [Bugfix] Exclude kv_cache_memory_bytes from CacheConfig.compute_hash (#47356)

Signed-off-by: Nils Matteson <nils@thaw.sh>
Signed-off-by: Nils Matteson <nilsmatteson@icloud.com>
Co-authored-by: Nils Matteson <nils@thaw.sh>

* fix: include topic frame in KV events replay response (#45177)

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>

* [Bugfix] Fix mamba+dflash for MRV2 (#47698)

* [Bugfix] Allow non-contiguous query in FlashInfer FP8 query quantization (#47908)

Signed-off-by: Matthew Bonanni <mbonanni@redhat.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* [Doc] Fix manylinux tag in installation guide (#47913)

Signed-off-by: Nick Cao <ncao@redhat.com>

* [CI Bug Fix] Temp fix for v3.2 accuracy (#47902)

* [Bugfix] Pad Mamba page size instead of scaling block_size in unify_kv_cache_spec_page_size (#45207)

Signed-off-by: Sahil170595 <147995121+Sahil170595@users.noreply.github.com>
Co-authored-by: Claude Fable 5 <noreply@anthropic.com>

* [Doc] Surface the --kv-cache-memory suggestion at INFO and document fast-startup knobs (#47374)

Signed-off-by: Nils Matteson <nils@thaw.sh>
Signed-off-by: Nils Matteson <nilsmatteson@icloud.com>
Co-authored-by: Nils Matteson <nils@thaw.sh>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* Add tuned selective_state_update config for AMD Instinct MI355 (#47767)

Signed-off-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>
Co-authored-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>

* [BugFix] Fix ModelOpt quantization inference for fused siblings (#47445)

Signed-off-by: jasonlizhengjian <jasonlizhengjian@gmail.com>

* fix(security): bound completion prompt list to prevent unbounded engine fan-out (#47845)

Signed-off-by: jperezde <jperezde@redhat.com>

* [Bugfix] Patch Hopper MXFP4 OOB scales reads leading to NaN (#47910)

Signed-off-by: Yifan Zong <yzong@redhat.com>

* [CI/Build] Accept ready-run-all-tests label in pre-commit gate (#47897)

Signed-off-by: AmeenP <ameen@primeintellect.ai>
Co-authored-by: AmeenP <ameen@primeintellect.ai>
Co-authored-by: Claude <noreply@anthropic.com>

* [ROCm][CI] Minimize comment in RocmAttention q_scale check (#47731)

Signed-off-by: stefankoncarevic <stefan.koncarevic@amd.com>

* [ROCm][Bugfix] Key sparse-MLA persistent metadata on per-request context lengths (#47766)

Signed-off-by: Rohan Potdar <rohan.potdar@amd.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* [Core] Persist and reuse the memory-profiling result across boots (opt-in) (#47388)

Signed-off-by: Nils Matteson <nils@thaw.sh>
Signed-off-by: Nils Matteson <nilsmatteson@icloud.com>
Co-authored-by: Nils Matteson <nils@thaw.sh>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [CPU][Bugfix] Fix flaky ShortConv prefill test on ARM (uninitialized weights) (#47848)

Signed-off-by: Rahul Vishwakarma <Rahul.Vishwakarma2@ibm.com>

* [Frontend] Add endpoint plugins framework (#47454)

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* [Bugfix] Avoid blocking model launching when no system ffmpeg available for TorchCodec (#47888)

Signed-off-by: Isotr0py <Isotr0py@outlook.com>

* [Core] Pass request context to CPU offload cache policy touch (#47744)

Signed-off-by: jacklin78911-collab <jacklin78911@gmail.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>

* [Core][Engine] only materialize tokens when thinking budget is in req (#47053)

Signed-off-by: walterbm <walter.beller.morales@gmail.com>

* [Bugfix][Pooling] Align CrossEncoder token type ids after truncation (#47772)

Signed-off-by: Ting Sun <suntcrick@gmail.com>

* [Minimax-M3] Using tok_sparse_select from MSA instead of triton kernels (#47502)

Signed-off-by: Yongye Zhu <zyy1102000@gmail.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* [ROCm][Perf] MXFP8 dense-linear + grouped-MoE GEMM optimizations for MiniMax-M3 (#46117)

Signed-off-by: amd-ethany <amd-ethany@users.noreply.github.com>
Co-authored-by: amd-ethany <amd-ethany@users.noreply.github.com>

* [Bugfix] DSV4 TP16 garbage output (#47493)

Signed-off-by: Jeff Ma <jeffjma@umich.edu>
Co-authored-by: Lucas Wilkinson <LucasWilkinson@users.noreply.github.com>

* [Perf] Minimax M3 - Support cross-layer allreduce-norm fusion (#47631)

Signed-off-by: wzhao18 <wzhao18.sz@gmail.com>
Co-authored-by: Yongye Zhu <zyy1102000@gmail.com>

* [Bugfix][Frontend] Fix http_requests_total metric recording some 4xx errors as 5xx (#44303)

Signed-off-by: Zach Zhu <zzqshu@126.com>

* [Bugfix] Register VLLM_BUILD_* and VLLM_IMAGE_TAG provenance env vars (#45313)

Signed-off-by: Nicklas Frahm <nicklas.frahm@gmail.com>

* [Bugfix] Avoid leaking Pydantic repr in tool_choice error message (#47028)

Signed-off-by: muhammadfawaz1 <135441198+muhammadfawaz1@users.noreply.github.com>
Co-authored-by: Mahad Durrani <114791389+mahadrehmann@users.noreply.github.com>

* [ROCm] fixed aiter master flag and expert parallelism compatibility on minimax-m3-mxfp8 (#47158)

Signed-off-by: Hongxia Yang <hongxia.yang@amd.com>

* fix: hash speculative draft model config (#47911)

Signed-off-by: Ace Eldeib <aeldeib@coreweave.com>
Signed-off-by: Ace Eldeib <alexeldeib@gmail.com>

* [Model][HunyuanVL] Use native transformers processor and adapt to transformers 5.13 (#47872)

Co-authored-by: manayang <manayang@tencent.com>

* [Docs] `kv_sharing_fast_prefill` correction (#47044)

Signed-off-by: NickLucche <nicolo.lucchesi@mistral.ai>

* Remove unused _get_kv_cache_config_deepseek_v4 alias (#47969)

Signed-off-by: NickLucche <nicolo.lucchesi@mistral.ai>

* [Test] Skip DeepEP MoE layer tests without P2P access (#47946)

Signed-off-by: Tyler Michael Smith <tyler@tylermsmith.com>
Signed-off-by: Tyler Michael Smith <tlrmchlsmth@gmail.com>
Co-authored-by: Claude <noreply@anthropic.com>

* [XPU] [Fusion passes] Disable fuse_rope_kvcache_cat_mla & qk_norm_rope_ fusion on XPU (#47962)

Signed-off-by: Chaojun Zhang <chaojun.zhang@intel.com>

* [Misc][Docs]  Add human-readable integer support for more cli-args (#47608)

Signed-off-by: NickLucche <nicolo.lucchesi@mistral.ai>

* [Frontend] Support bad_words in the /v1/completions endpoint (#46793)

Signed-off-by: sungbin1015 <sbin@solbox.com>
Co-authored-by: Claude <noreply@anthropic.com>

* [ROCm] Add tuned selective_state_update float16 config for AMD Instinct MI300X (#47945)

Signed-off-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>
Co-authored-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>

* [Model] Support MOSS-Transcribe-Diarize (#47729)

Signed-off-by: gcanlin <canlinguosdu@gmail.com>

* [ROCm] Add tuned selective_state_update float32 config for AMD Instinct MI300X (#47947)

Signed-off-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>
Co-authored-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>

* Remove router weight upcast for DSv2-related models (#47970)

Signed-off-by: Thien Tran <gau.nernst@yahoo.com.sg>

* [Bugfix] Allocate HY V3 expert_bias in float32 to prevent silent downcasting (#47797)

Signed-off-by: 辰言 <oncwnuIWp30GguOyJ615Fqj8H-yc@git.weixin.qq.com>
Signed-off-by: Jee Jee Li <jeejeelee@inferact.ai>
Co-authored-by: 辰言 <oncwnuIWp30GguOyJ615Fqj8H-yc@git.weixin.qq.com>
Co-authored-by: Jee Jee Li <jeejeelee@inferact.ai>
Co-authored-by: Jee Jee Li <pandaleefree@gmail.com>

* Add tuned selective_state_update float32 config for AMD Instinct MI355 (#47943)

Signed-off-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>
Co-authored-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>

* [Rust Frontend] Handle `continue_final_message` with renderer sentinel (#47844)

Signed-off-by: Bugen Zhao <i@bugenzhao.com>

* [CI/Build][BugFix][The Rock] Fix get_ssm_device_name to return sanitized, usable filename (#47781)

Signed-off-by: Randall Smith <Randall.Smith@amd.com>

* [Bugfix][V1] Free out-of-window blocks on the processed-token basis under async scheduling (#47728)

Signed-off-by: Saddss <28726669061@qq.com>
Signed-off-by: Nick Hill <nickhill123@gmail.com>
Signed-off-by: Yifan Qiao <yifanqiao@inferact.ai>
Co-authored-by: Yifan Qiao <yifanqiao@inferact.ai>
Co-authored-by: Saddss <28726669061@qq.com>
Co-authored-by: Nick Hill <nickhill123@gmail.com>

* Improvement of Docker image build for IBM Power using prebuilt wheels from IBM published devpi index (#46017)

Signed-off-by: vivek sharma <vivsharm@redhat.com>
Signed-off-by: puneetsharma21 <puneet.sharma21@ibm.com>
Signed-off-by: Puneet Sharma <puneet.sharma21@ibm.com>
Co-authored-by: vivek sharma <vivsharm@redhat.com>
Co-authored-by: Puneet Sharma <puneet.sharma21@ibm.com>
Co-authored-by: depthfirst-app[bot] <184448029+depthfirst-app[bot]@users.noreply.github.com>

* [Bug] Fix Batched DeepGEMM (#47884)

Signed-off-by: Robert Shaw <robertgshaw2-redhat@dgx-b200-02.mgmt.accl-001.lab.rdu2.dc.redhat.com>
Co-authored-by: Robert Shaw <robertgshaw2-redhat@dgx-b200-02.mgmt.accl-001.lab.rdu2.dc.redhat.com>
Co-authored-by: Claude <noreply@anthropic.com>

* Fix NVML capability lookup for visible devices (#47892)

Signed-off-by: Tyler Michael Smith <tlrmchlsmth@gmail.com>

* [Spec Decode] Support hybrid (SWA + full attention) DFlash drafters (#47914)

Signed-off-by: mgoin <mgoin64@gmail.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* Fix FlashAttention MLA prefill V unpadding (#42642)

Signed-off-by: Martin Vit <martin@voipmonitor.org>
Signed-off-by: Matthew Bonanni <mbonanni@redhat.com>
Co-authored-by: Matthew Bonanni <mbonanni@redhat.com>

* [Bugfix][DCP] Cast LSE to fp32 in a2a combine to fix bf16 bitcast crash (#47801)

Signed-off-by: Shawn Tsai <shawnyht@gmail.com>

* [Bugfix] Re-enable benchmarking of librispeech dataset. (#47033)

Signed-off-by: Anna Mayne <anna.mayne@arm.com>

* [CI] BugFix Eval Small Models Distributed test for DiffusionGemma (#47980)

Signed-off-by: Markov Ilya <markovilya19@gmail.com>
Co-authored-by: Markov Ilya <markovilya19@gmail.com>

* updated flash_attn GIT_TAG to point to torch Stable ABI FA3 commit (#47995)

Signed-off-by: Chris Leonard <chleonar@redhat.com>

* [Core] Move MRV1 `late_interaction_runner.py` out of MRV2 subtree (#48014)

Signed-off-by: Nick Hill <nickhill123@gmail.com>

* [Perf] Integrate TRTLLM BF16 MoE Modular Kernel  (#45182)

Signed-off-by: Kaihang Jiang <kaihangj@login-lyris02.lyris.clusters.nvidia.com>

* [ROCm][Bugfix] Fix empty-tensor .max() crash in AITER FA (#47894)

Signed-off-by: Djordje Ramic <djoramic@amd.com>

* Allow FlashInfer A2A backends for TRTLLM FP8 MoE Modular (#46661)

Signed-off-by: Thien Tran <gau.nernst@yahoo.com.sg>

* [CI] GSM8K eval integration test for KV offloading (#46893)

Signed-off-by: Tyler Michael Smith <tyler@tylermsmith.com>
Signed-off-by: Tyler Michael Smith <tlrmchlsmth@gmail.com>
Signed-off-by: Itay Etelis <itay.etelis@ibm.com>
Signed-off-by: Itay Etelis <92247226+Etelis@users.noreply.github.com>
Signed-off-by: Or Ozeri <oro@il.ibm.com>
Co-authored-by: Claude Opus 4.6 (1M context) <noreply@anthropic.com>
Co-authored-by: Itay Etelis <92247226+Etelis@users.noreply.github.com>
Co-authored-by: Itay Etelis <itay.etelis@ibm.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [Feat] Add runtime monitor for post-warmup TileLang compilation (#46718)

Signed-off-by: LopezCastroRoberto <rocastro@redhat.com>
Signed-off-by: Roberto L. Castro <38211239+LopezCastroRoberto@users.noreply.github.com>

* [docs] Fix the docs build (#48008)

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>
Co-authored-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* [ROCm] Fix pooling startup workspace lock (#47912)

Signed-off-by: Andreas Karatzas <akaratza@amd.com>

* Fix embed scaling + CUDA graphs in Transformers modelling backend (#48010)

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* [Bugfix] Use int8 workspace for FlashInfer MLA decode (#48046)

Signed-off-by: Nick Hill <nickhill123@gmail.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* [KVConnector][NIXL] Support pipeline-parallel prefill in push mode (#45880)

Signed-off-by: zixi-qi <zixi@inferact.ai>
Co-authored-by: Nick Hill <nickhill123@gmail.com>

* [ROCm][CI][MoE] Fix double-transpose of fused w3 expert weights (#47874)

Signed-off-by: stefankoncarevic <stefan.koncarevic@amd.com>
Signed-off-by: Andreas Karatzas <akaratza@amd.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [Bugfix][ROCm] Change AttentionCGSuppoort in TritonMLA to UNIFORM_SINGLE_TOKEN_DECODE (#47144)

Signed-off-by: Dino Music <Dino.Music@amd.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [XPU][LoRA] Fix torch.compile DEVICE_LOST by avoiding view-mutation in LoRA shrink (#47944)

Signed-off-by: Chaojun Zhang <chaojun.zhang@intel.com>

* [KV Offloading] Add free block iterator for CPU offload scheduling (#47849)

Signed-off-by: chaunceyjiang <chaunceyjiang@gmail.com>

* [CPU] Fix Qwen-Next SSM type for AMX GDN (#48073)

Signed-off-by: jiang1.li <jiang1.li@intel.com>

* [Bugfix] Fix Qwen3-ASR transcription streaming postprocessing (#42478)

Signed-off-by: JooHo Lee <BWAAEEEK@users.noreply.github.com>
Signed-off-by: JooHo Lee <jooho414@gmail.com>
Co-authored-by: JooHo Lee <BWAAEEEK@users.noreply.github.com>

* Remove TeleChatForCausalLM  (#47989)

Signed-off-by: Xianbao QIAN <xianbao.qian@gmail.com>
Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>
Co-authored-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* Add Intel XPU Docker release pipeline (#47880)

Signed-off-by: wenjun.liu <wenjun.liu@intel.com>
Signed-off-by: jun,du <jun.du@intel.com>
Co-authored-by: jun,du <jun.du@intel.com>
Co-authored-by: Kunshang Ji <kunshang.ji@intel.com>

* [Bugfix] Fix race condition in KVBlockZeroer (#48085)

Signed-off-by: Benjamin Chislett <bchislett@nvidia.com>
Signed-off-by: Benjamin Chislett <chislett.ben@gmail.com>
Co-authored-by: Wentao Ye <44945378+yewentao256@users.noreply.github.com>

* Sanitize server file paths from validation error responses (#46415)

Signed-off-by: muhammadfawaz1 <135441198+muhammadfawaz1@users.noreply.github.com>
Co-authored-by: Mahad Durrani <114791389+mahadrehmann@users.noreply.github.com>

* [P/D][Bugfix] Fix PD async KV load lookahead handling for MTP spec decode (#46694)

Signed-off-by: chaunceyjiang <chaunceyjiang@gmail.com>
Co-authored-by: Nick Hill <nickhill123@gmail.com>

* Remove PersimmonForCausalLM and FuyuForCausalLM model architectures (#48096)

Signed-off-by: Xianbao QIAN <xianbao.qian@gmail.com>
Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>
Signed-off-by: Cyrus Leung <tlleungac@connect.ust.hk>
Co-authored-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>
Co-authored-by: Cyrus Leung <tlleungac@connect.ust.hk>

* Migrate Olmo and Olmo2 to the Transformers modeling backend (#48100)

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* [Bugfix][Model Runner V2] Order uniform decodes first so spec decodes aren't misclassified as prefills (#47381)

Signed-off-by: Woosuk Kwon <woosuk@inferact.ai>
Signed-off-by: Nick Hill <nickhill123@gmail.com>
Co-authored-by: Claude Fable 5 <noreply@anthropic.com>
Co-authored-by: Nick Hill <nickhill123@gmail.com>

* [CI] Annotate built Docker image tags on the Buildkite build page (#48101)

Signed-off-by: khluu <khluu000@gmail.com>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>

* Pin PyNvVideoCodec to tested 2.0.4 wheel (#48056)

* [Bugfix] Guard CUDA-only rms_norm_per_block_quant in FUSED_OPS for non-CUDA builds (#47296)

Signed-off-by: Tsvika Shapira <tsvika@moonmath.ai>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [ROCm][CI] Set all timeout_in_minutes to 180 (#48146)

Signed-off-by: charlifu <charlifu@amd.com>

* [Bugfix] Preserve tensor causal metadata for grouped attention (#48135)

Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>
Co-authored-by: Codex <codex@openai.com>

* [KVConnector] MultiConnector: give every sub-connector the request's real blocks in `update_state_after_alloc` (#46865)

Signed-off-by: deng451e <838677410@qq.com>

* [Bugfix][MRV2] Reset num_accepted_tokens on add_request in all modes (#48132)

Signed-off-by: Nick Hill <nickhill123@gmail.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* [ROCM][DSV32][Perf][MTP] Enable UNIFORM_BATCH CG mode in rocm_aiter_mla_sparse (#45149)

Signed-off-by: Teemu Virolainen <teemu.virolainen@amd.com>
Co-authored-by: TJian <tunjian.tan@embeddedllm.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [KV Connector][Mooncake] Apply SWA lookup mask before hashing/key build (#47317)

Signed-off-by: Zhewen Li <zhewenli@inferact.ai>
Co-authored-by: Zhewen Li <zhewenli@inferact.ai>
Co-authored-by: Codex <codex@openai.com>
Co-authored-by: Claude <noreply@anthropic.com>

* [ROCm] Synchronize sparse MLA metadata before graph replay (#47404)

Signed-off-by: zihaomu <zmu@amd.com>

* [CI] Increase extract hidden states TP2 timeout (#48161)

Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>

* [ROCm] Revert Part of `[ROCm] Fix pooling startup workspace lock` #47912 (#48154)

Signed-off-by: Micah Williamson <micah.williamson@amd.com>

* [CI] Fix cargo-deny config flag ordering (#48170)

Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>

* Correct model layer aliasing for Bert style models (#43896)

* update marlin M size for EP (#48144)

Signed-off-by: gnovack <novackgm@gmail.com>

* [ROCm][CI] Move remaining engine/samplers AMD steps to mi325_1 (#48169)

Signed-off-by: pei.zhang <pei.zhang@amd.com>
Co-authored-by: Claude <noreply@anthropic.com>

* [bugfix] bge-m3-sparse-plugin mismatch requests (#48112)

Signed-off-by: augusto.yjh <augusto.yjh@antgroup.com>

* [CI/Build][AMD] Fix ROCm OOM in eagle_correctness_heavy by reserving CUDA graph memory (#47366)

Signed-off-by: pei.zhang <pei.zhang@amd.com>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [kv_offload] Emit tier-owned BlockStored events from FS/OBJ secondary tiers (#47923)

Signed-off-by: Change72 <changg@nvidia.com>
Co-authored-by: Claude Fable 5 <noreply@anthropic.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>

* DCP supports hybrid attention (#40996)

Signed-off-by: YanXu <yancey.yx@alibaba-inc.com>
Signed-off-by: Jingyi Yang <girasoleyang@gmail.com>
Co-authored-by: Jingyi Yang <girasoleyang@gmail.com>

* [Core][KV events] Report prefix-cache-reused blocks in full report mode (#45261)

Signed-off-by: Lei Gong <gonglei25@huawei.com>
Co-authored-by: Lei Gong <gonglei25@huawei.com>
Co-authored-by: Claude <noreply@anthropic.com>

* [Feature][Parser] Support include_reasoning param for non-Harmony models (#44301)

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>
Co-authored-by: Chauncey <chaunceyjiang@gmail.com>

* [Perf] fuse more rmsnorm and all-reduce in qwen3.5 (#46998)

Signed-off-by: zjy0516 <riverclouds.zhu@qq.com>

* [XPU] Enable v1/sample tests on XPU CI (#44472)

Signed-off-by: Chaojun Zhang <chaojun.zhang@intel.com>

* [CI] Right-size test-area timeouts from nightly durations (#48186)

Signed-off-by: khluu <khluu000@gmail.com>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>

* Add XPU nightly and release image publishing to DockerHub (#48126)

Signed-off-by: wenjun.liu <wenjun.liu@intel.com>
Signed-off-by: jun,du <jun.du@intel.com>
Co-authored-by: jun,du <jun.du@intel.com>
Co-authored-by: Kunshang Ji <kunshang.ji@intel.com>

* [Rust Frontend] Integrate MM video support (#47959)

Signed-off-by: Bugen Zhao <i@bugenzhao.com>

* [Model][CI/Build] Cosmos3: enable registry tests and register Cosmos3-Super (#48211)

Signed-off-by: Mingfei Guo <1800012773@pku.edu.cn>

* [CI] Add TORCH_NIGHTLY=1 build mode (run full suite on torch nightly) (#47180)

Co-authored-by: Andrey Talman <atalman@users.noreply.github.com>
Co-authored-by: Kevin H. Luu <khluu000@gmail.com>

* Deepstream video backend (#42424)

Signed-off-by: Viranjan Pagar <vpagar@nvidia.com>
Signed-off-by: Isotr0py <Isotr0py@outlook.com>
Signed-off-by: Isotr0py <mozf@mail2.sysu.edu.cn>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>
Co-authored-by: Isotr0py <mozf@mail2.sysu.edu.cn>
Co-authored-by: Isotr0py <Isotr0py@outlook.com>
Co-authored-by: Roger Wang <hey@rogerw.io>

* [Rust Frontend] Add roundtrip fixtures for more chat parsers (#47883)

Co-authored-by: Bugen Zhao <i@bugenzhao.com>
Signed-off-by: reidliu41 <reid201711@gmail.com>
Signed-off-by: Bugen Zhao <i@bugenzhao.com>

* [Misc] Remove dead code in ViT functionality test (#48220)

Signed-off-by: Isotr0py <Isotr0py@outlook.com>

* [Bugfix][Spec Decode] Fix DFlash draft/target layer-count mismatch (#48113)

Signed-off-by: Nick Hill <nickhill123@gmail.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* [Model] Migrate MistralLarge3ForCausalLM to AutoWeightsLoader (#48153)

Signed-off-by: Yuchen Fan <functionhx@gmail.com>

* [Refactor] Remove unused rocm kernel `combine_topk_swa_indices_ragged` (#48158)

Signed-off-by: yewentao256 <zhyanwentao@126.com>

* [Bugfix] Fix turboquant FP8 cast failure for BF16 models on Ampere GPUs (#39988)

Signed-off-by: Xu Zhou <xuzhou9417@163.com>
Co-authored-by: Xu Zhou <xuzhou9417@163.com>
Co-authored-by: Hoseung Kim <ghyutjik123@gmail.com>

* fix: correct load_weights track logic and enable weight integrity for… (#41811)

Signed-off-by: Yipeng Hu <i26268@metax-tech.com>
Signed-off-by: HuYiPeng <144002351+MynameFelix@users.noreply.github.com>
Signed-off-by: Isotr0py <Isotr0py@outlook.com>
Co-authored-by: Yipeng Hu <i26268@metax-tech.com>
Co-authored-by: Isotr0py <Isotr0py@outlook.com>

* [Build/CI] Build arm64 PR and postmerge image builds for Blackwell SM10x and SM110 (#48041)

Signed-off-by: Tyler Michael Smith <tlrmchlsmth@gmail.com>

* [Model] Add LongCat-Flash-Lite (n-gram embedding) (#47857)

Signed-off-by: mgoin <mgoin64@gmail.com>

* [ROCm] Enable DeepSeek-V4 DSpark speculative decoding on AMD (MI350X / MI355X, gfx950) (#47419)

Signed-off-by: larryli2-amd <larryli2@amd.com>
Signed-off-by: larryli2-amd <Larry.Li@amd.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [Bugfix] Fix FlashMLA dense fp8 metadata crash (num_sm_parts clamp) (#48045)

Signed-off-by: Matthew Bonanni <mbonanni@redhat.com>
Co-authored-by: Claude <noreply@anthropic.com>

* handle topk_ids padding in align sum kernel (#47785)

Signed-off-by: gnovack <novackgm@gmail.com>

* [Bugfix][Test] Register Qwen/Qwen3.5-4B example model (#48276)

Signed-off-by: Nick Hill <nickhill123@gmail.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* [Bugfix] Fix thinking_token_budget not enforced after natural </think> re-entry (#45984)

Signed-off-by: Ashwin Giridharan <girida@amazon.com>

* Add VLLM_FLASHINFER_AUTOTUNE_SKIP_OPS and skip CuTeDSL fp4_gemm autotuning by default (#48268)

Signed-off-by: mgoin <mgoin64@gmail.com>
Signed-off-by: Michael Goin <mgoin64@gmail.com>
Co-authored-by: Claude Fable 5 <noreply@anthropic.com>

* [Quantization] Bound peak memory when repacking FP4 MoE weights for Marlin (#47851)

Signed-off-by: Joe Rowell <joerowell4@gmail.com>
Signed-off-by: mgoin <mgoin64@gmail.com>
Co-authored-by: mgoin <mgoin64@gmail.com>

* [BugFix] weights processing peak memory reduction for nvfp4 MoE layers (#46276)

Signed-off-by: Jimmy Lee <hirejimmylee@gmail.com>

* [BugFix] Fix packed HND KV cache reshape for FlashAttention (#47314)

Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [Misc] Use meta tensor for KV cache stride calculation (#47316)

Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [Logs] DP Supervisor Log Improvement (#48278)

Signed-off-by: Robert Shaw <robertgshaw2-redhat@ip-172-31-18-125.us-east-2.compute.internal>
Co-authored-by: Robert Shaw <robertgshaw2-redhat@ip-172-31-18-125.us-east-2.compute.internal>

* [Revert] [Build] Update vllm ...builds FA3 with torch stable API (#48269)

Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>

* Bump Transformers version to 5.13.0 (#47867)

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* [XPU]remove is_xxx from moe class and bump up kernels (#48079)

Signed-off-by: mayuyuace <qiming1.zhang@intel.com>
Co-authored-by: Kunshang Ji <kunshang.ji@intel.com>

* [CI] Point CI at Transformers release rather than release branch (#48328)

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* FP32 router GEMV optimization (#48335)

Signed-off-by: peiyuanz <peiyuanz@inferact.ai>
Signed-off-by: Jee Jee Li <jeejeelee@inferact.ai>
Co-authored-by: peiyuanz <peiyuanz@inferact.ai>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: zhouzhou <zhouzhou@zhouzhoudeMacBook-Pro.local>

* [XPU][UT]Fix InternS1ProForConditionalGeneration AssertionError (#48232)

Signed-off-by: Lai, Yejing <yejing.lai@intel.com>

* [2/N][KV-Cache Layout Refactor] Pack K/V into the content dim across attention backends (#44455)

Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>
Signed-off-by: Matthew Bonanni <mbonanni@redhat.com>
Signed-off-by: Lucas Wilkinson <LucasWilkinson@users.noreply.github.com>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: OpenAI Codex <codex@openai.com>
Co-authored-by: Matthew Bonanni <mbonanni@redhat.com>
Co-authored-by: Nicolò Lucchesi <nlucches@redhat.com>

* fix(entrypoints): stop resolve_items leaking in-flight media fetch tasks on partial failure (#48333)

Signed-off-by: ErenAta16 <erena6466@gmail.com>

* fix(processor): route MiMo-V2-Omni media fetch through MediaConnector (#43117)

Signed-off-by: Ievgen Bondarenko <ibondarenko@student.sierracollege.edu>
Signed-off-by: Ievgen (Jack) Bondarenko <ibondarenko@student.sierracollege.edu>
Signed-off-by: Isotr0py <Isotr0py@outlook.com>
Co-authored-by: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
Co-authored-by: Isotr0py <Isotr0py@outlook.com>

* [CI] split tests to reduce CI time (#48219)

Signed-off-by: zjy0516 <riverclouds.zhu@qq.com>

* [Bugfix][LoRA] Support ark_linear base layer in _get_lora_device (#47690)

Signed-off-by: AlejandroParedesLT <alejandroparedeslatorre@gmail.com>

* [CI][CPU] Add Qwen2-VL multimodal tests for CPU backend and fix incompatibilities (#48072)

Signed-off-by: Zhenhui Zhao <zhenhui.zhao@intel.com>

* [2/N][Core] support partial prefix cache hit for hybrid model (#46384)

Signed-off-by: zjy0516 <riverclouds.zhu@qq.com>
Signed-off-by: Yifan Qiao <yifanqiao@inferact.ai>
Co-authored-by: Yifan Qiao <yifanqiao@inferact.ai>

* Runtime Draft Weight Update for Speculative Decoding (#46725)

Signed-off-by: vx120 <893600387@qq.com>
Signed-off-by: vx120 <57470515+vx120@users.noreply.github.com>
Signed-off-by: aoshen02 <aoshen@inferact.ai>
Co-authored-by: crp0128 <191679376@qq.com>
Co-authored-by: aoshen02 <aoshen@inferact.ai>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>

* [Perf][Qwen] Replace MOE all-reduce with reduce-scatter (#47006)

Signed-off-by: gcanlin <canlinguosdu@gmail.com>
Signed-off-by: yewentao256 <zhyanwentao@126.com>
Co-authored-by: yewentao256 <zhyanwentao@126.com>

* [Frontend] Add /abort_requests to the RLHF dev API router (#47173)

Signed-off-by: aoshen02 <aoshen@inferact.ai>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [Bugfix] Guard mixed-dtype allreduce RMSNorm quant fusions (#48330)

Signed-off-by: hcenteno <hugo.centeno@estudiantat.upc.edu>

* Make tiering offload region DP-replica aware (#47987)

Signed-off-by: Liran Schour <lirans@il.ibm.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>

* [CI][2/N] reduce CI time (#48394)

Signed-off-by: zjy0516 <riverclouds.zhu@qq.com>

* [EC Connector] Add EC Transfer Params (#42433)

Signed-off-by: omerpaz95 <omerpaz95@gmail.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>

* [CI][Rust Frontend] Pin cargo tool versions (#48222)

Signed-off-by: Bugen Zhao <i@bugenzhao.com>

* [Bugfix][KV Offloading] Fix stale transfer_jobs after reset_cache + harden job completion (#48102)

Signed-off-by: Alex <alex.tech.lab@outlook.com>

* [Kernel] Implement CUDA kernel for ReLUSquaredActivation (relu^2) (#39058)

Signed-off-by: Tanish Malekar <tanishmalekar32@gmail.com>
Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>

* [ROCm][MiniMax-M3] Add AITER sparse paged attention (#47287)

Signed-off-by: Tan Pin Siang <tanpinsiang@gmail.com>
Signed-off-by: tjtanaa <tunjian.tan@embeddedllm.com>
Co-authored-by: vllmellm <vllm.ellm@embeddedllm.com>
Co-authored-by: Hongxia Yang <hongxia.yang@amd.com>
Co-authored-by: Jun Kang Chow <junkangchow@gmail.com>
Co-authored-by: tjtanaa <tunjian.tan@embeddedllm.com>

* [Model] Optimize Qwen3.5 on H20 (#48350)

Signed-off-by: zzt <zengzetang.zzt@antgroup.com>

* [Bugfix][ROCm] Keep TP all_gather on base-class collective (#48446)

Signed-off-by: fai <fangzhouai@gmail.com>
Co-authored-by: Cursor <cursoragent@cursor.com>

* Re-disable CUDA graph memory profiling on ROCm (#48440)

Signed-off-by: Rohan Potdar <rohan.potdar@amd.com>
Co-authored-by: Claude Opus 4.8 (1M context) <noreply@anthropic.com>

* [CPU][Spec Decode] Support DFlash speculative decoding for GDN models on CPU (#46090)

Signed-off-by: guybd <guy.boudoukh@intel.com>
Signed-off-by: Guy Boudoukh <guy.boudoukh@intel.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Li, Jiang <jiang1.li@intel.com>

* [Bugfix][UT]Fix EagleMiniCPMForCausalLM meet TypeError (#48452)

Signed-off-by: Lai, Yejing <yejing.lai@intel.com>

* [CPU] Create Proper Numa topology for s390x (#40714)

Signed-off-by: Rehan Khan <Rehan.Khan7@ibm.com>
Co-authored-by: Li, Jiang <jiang1.li@intel.com>

* [BugFix] Restore full tokens for Qwen MTP When MoE SP (#48429)

Signed-off-by: gcanlin <canlinguosdu@gmail.com>

* [ROCm][CI] Cache Rust builds by source inputs (#46527)

Signed-off-by: Andreas Karatzas <akaratza@amd.com>
Signed-off-by: Matthew Wong <Matthew.Wong2@amd.com>
Co-authored-by: Matthew Wong <Matthew.Wong2@amd.com>

* [Distributed][Perf] Enable FlashInfer MNNVL allreduce RMS quant fusion (#48064)

Signed-off-by: Mohammad Miadh Angkad <176301910+mmangkad@users.noreply.github.com>

* [Attention] Make sliding-window support an explicit backend capability (#48011)

Signed-off-by: NickLucche <nicolo.lucchesi@mistral.ai>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [Bugfix][KV Cache] Don't route uniform-page-size MLA+SWA models into DeepseekV4 packing (#48256)

Signed-off-by: NickLucche <nicolo.lucchesi@mistral.ai>
Co-authored-by: Claude <noreply@anthropic.com>

* [Core] Support fp32 lm_head for generation models via head_dtype (RFC #48305 §3.6) (#48390)

Signed-off-by: Karthik Kothuri <karthikkothuri2009@gmail.com>
Signed-off-by: wang.yuqi <yuqi.wang@daocloud.io>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: wang.yuqi <yuqi.wang@daocloud.io>

* [CI] Add SPDX license header to Rust/Protobuf sources (#48472)

Signed-off-by: Bugen Zhao <i@bugenzhao.com>

* [Mypy Fix] Split mypy work (#48490)

Signed-off-by: yewentao256 <zhyanwentao@126.com>

* lower memory required for capturing cudagraphs for large cudagraph sizes (#48483)

Signed-off-by: Omer Ullman Argov <118735753+omera-nv@users.noreply.github.com>

* remove force channels_last in Idefics3MultiModalProcessor (#48467)

Signed-off-by: Yan Ma <yan.ma@intel.com>

* [XPU][CI]Adjust timeout_in_minutes in Intel GPU CI (#48418)

Signed-off-by: zengxian <xiangdong.zeng@intel.com>
Co-authored-by: Kunshang Ji <kunshang.ji@intel.com>

* [BugFix][ModelRunner V2] Fix stale attn metadata in speculator prefill cudagraph capture (#48261)

Signed-off-by: Nick Hill <nickhill123@gmail.com>

* [Misc]  Improve Matryoshka pooling dimensions validation (#48057)

Signed-off-by: Taneem Ibrahim <taneem.ibrahim@gmail.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [EC Connector] CPU Offloading EC Connector (#47423)

Signed-off-by: omerpaz95 <omerpaz95@gmail.com>
Signed-off-by: Or Ozeri <oro@il.ibm.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>

* [ROCm][CI] Transformers: pass only one of input_ids/inputs_embeds (#48258)

Signed-off-by: Stefan Koncarevic <stefan.koncarevic@amd.com>
Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>
Co-authored-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* [Doc] Add DeepseekV32ForCausalLM to supported_models.md (#48293)

Signed-off-by: Gavin Morris <gmorriscs@gmail.com>

* [Bugfix][Frontend] Flush engine reasoning parser at engine-reasoning → tool streaming boundary (#47606)

Signed-off-by: Aakif Nawaz <aakif.nawaz@amd.com>
Co-authored-by: Ben Browning <56071+bbrowning@users.noreply.github.com>

* [ROCm][CI] Unblock `AMD: Language Models Test (Extended Pooling)` (#48513)

Signed-off-by: Micah Williamson <micah.williamson@amd.com>

* Add DCP + Eagle support for Tokenspeed MLA backends (#48180)

Signed-off-by: Pavani Majety <pmajety@nvidia.com>
Signed-off-by: Jingyi Yang <girasoleyang@gmail.com>
Co-authored-by: Jingyi Yang <girasoleyang@gmail.com>

* Added sliding window attention support for qwen-eagle3 architecture (#47568)

Signed-off-by: shanjiaz <zsjwpianpian@gmail.com>

* [Core] Preserve Marconi caching with selective hybrid cache retention (#47782)

Signed-off-by: Nick Hill <nickhill123@gmail.com>

* [BugFix] Initialize model_config for Qwen3-VL MoE (#44863)

Signed-off-by: wenpengw-nv <wenpengw@nvidia.com>
Co-authored-by: Roger Wang <hey@rogerw.io>

* [Feat] Add Support for BertForMaskedLM to vLLM (#48463)

Signed-off-by: atalhens <sneh.lata@nutanix.com>

* [Frontend] Expose logprob_token_ids on Python OpenAI endpoints (#43463)

Signed-off-by: Lang Zhao <lang.zhao@galileo.ai>
Co-authored-by: Claude <noreply@anthropic.com>

* [BugFix] Correct OTEL span start time for Dynamo compilation (#40698)

Signed-off-by: emricksini-h <emrick.birivoutin@hcompany.ai>
Co-authored-by: Simon Mo <simon.mo@hey.com>

* add pad-aware swiglu limit kernel (#48287)

Signed-off-by: gnovack <novackgm@gmail.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [Quantization][INC][ARK] Support INT2 XPU WOQ Linear (#47521)

Signed-off-by: Zhenzhong1 <zhenzhong.xu@intel.com>
Signed-off-by: Zhenzhong Xu <zhenzhong.xu@intel.com>
Co-authored-by: Kunshang Ji <kunshang.ji@intel.com>

* [1/N] Add dense MHA path for sparse MLA short sequences (#47327)

Signed-off-by: Matthew Bonanni <mbonanni@redhat.com>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>

* up FI fp8 moe topk to 32 (#44462)

* [ROCm][MiniMax-M2] Dispatch fused QK-norm + AllReduce via AITER (#44849)

Signed-off-by: Aakif Nawaz <aakif.nawaz@amd.com>
Co-authored-by: Pawel Kowalski <pawel.kowalski@amd.com>

* [ROCm][Kernel] Add HybridW4A16LinearKernel: Triton prefill + HIP skinny decode (#40977)

Signed-off-by: Matthias Gehre <matthias.gehre@amd.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [Perf][ROCm] Fix GDN KKT warmup regression on RDNA by avoiding fp32 tl.dot (#45000)

Signed-off-by: Saeid Rostami <srostami@amd.com>
Signed-off-by: nemanjaudovic <nudovic@amd.com>

* [ROCm][MiniMax-M3][Spec Decode] Support speculative decode with AITER sparse PA (#47984)

Signed-off-by: Tan Pin Siang <tanpinsiang@gmail.com>
Co-authored-by: vllmellm <vllm.ellm@embeddedllm.com>
Co-authored-by: Hongxia Yang <hongxia.yang@amd.com>
Co-authored-by: Jun Kang Chow <junkangchow@gmail.com>
Co-authored-by: TJian <tunjian.tan@embeddedllm.com>

* [Bugfix] Skip minimax_m3 tool parser tests when Rust extension is absent (#48523)

Signed-off-by: Matt Woodson <mwoodson@redhat.com>

* [Misc] Clean up "swap_space" (#48549)

Signed-off-by: wangxiyuan <wangxiyuan1007@gmail.com>

* [ROCm] Retune MI355 selective_state_update float16 config on the unified effective_batch grid (#48372)

Signed-off-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>
Co-authored-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>

* [Perf][Feat] Add generic cuteDSL LL BF16 router (GEMM) (#42562)

Signed-off-by: LopezCastroRoberto <rocastro@redhat.com>
Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>
Signed-off-by: Roberto L. Castro <38211239+LopezCastroRoberto@users.noreply.github.com>
Co-authored-by: Lucas Wilkinson <lwilkins@redhat.com>

* fix(security): guard lm-format-enforcer regex compile with timeout (#47595)

Signed-off-by: jperezde <jperezde@redhat.com>

* Add Cosmos3 Edge Reasoner model (#48291)

Signed-off-by: Bartosz Stefaniak <bstefaniak@nvidia.com>
Co-authored-by: Bartosz Stefaniak <bstefaniak@nvidia.com>

* [Bugfix] Return 400 instead of 500 when multimodal data is sent to a text-only model (#48473)

Signed-off-by: Hoang Nguyen Tien <hoang.nguyentien.2601@gmail.com>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>

* [XPU][CI] Add tests/v1/e2e/general/test_correctness_sliding_window.py in Intel GPU CI (#47231)

Signed-off-by: zengxian <xiangdong.zeng@intel.com>
Signed-off-by: xiangdong <40376367+zxd1997066@users.noreply.github.com>

* [Test] Enable KV cache events for HMA models in CPU offloading test (#47754)

Signed-off-by: Itay Etelis <92247226+Etelis@users.noreply.github.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>

* [Misc] Rename VLLM_TRITON_ATTN_USE_TD to VLLM_TRITON_USE_TD (#45781)

Signed-off-by: Artur Fierka <artur.fierka@intel.com>
Co-authored-by: Claude <noreply@anthropic.com>

* [Bugfix][Security] Fix concurrent sparse invariant race bypassing CVE remediation (#48583)

Signed-off-by: jperezde <jperezde@redhat.com>

* [Model] Enable LoRA support for tower and connector in LlavaNextVideo (#48594)

Signed-off-by: gangula-karthik <gkarthik923@gmail.com>

* [NIXL] Avoid reading expired blocks in bidirectional turn-2 read (#47021)

Signed-off-by: Tomer Gilad <tgilad@nvidia.com>
Signed-off-by: NickLucche <nicolo.lucchesi@mistral.ai>
Co-authored-by: NickLucche <nicolo.lucchesi@mistral.ai>

* [Bugfix] Include inline per-token-head scales in offloaded page transfer width (#48411)

Signed-off-by: Itay Etelis <itay.etelis@ibm.com>
Signed-off-by: Itay Etelis <Itay.etelis@gmail.com>
Signed-off-by: Itay Etelis <92247226+Etelis@users.noreply.github.com>
Co-authored-by: Itay Etelis <itay.etelis@ibm.com>
Co-authored-by: Itay Etelis <Itay.etelis@gmail.com>

* [Bugfix] Gemma4 parser: classify channel-less output consistently in streaming and non-streaming (#48262)

Signed-off-by: Adhithya Balakrishnan <adhithya.b2004@gmail.com>
Co-authored-by: Ben Browning <56071+bbrowning@users.noreply.github.com>

* [CI Bug] Fully solve accuracy issue for DSv3.2 + MTP + Sequence Parallel (#48036)

Signed-off-by: yewentao256 <zhyanwentao@126.com>

* [Bugfix] Make MLA+SWA check the layer's backend, not the model config (#48520)

Signed-off-by: mgoin <mgoin64@gmail.com>

* [Doc] Sync four function docstrings with their signatures (#45437)

Signed-off-by: Daoyuan Li <94409450+DaoyuanLi2816@users.noreply.github.com>
Co-authored-by: Wentao Ye <44945378+yewentao256@users.noreply.github.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [KV Offload] Split cpu_cache_usage_perc into write/read usage gauges (#47666)

Signed-off-by: srinivas_oo7 <sklinkedin0120@gmail.com>
Co-authored-by: srinivas_oo7 <sklinkedin0120@gmail.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>

* [ROCm] Retune MI355 selective_state_update float32 config on the unified effective_batch grid (#48373)

Signed-off-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>
Co-authored-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>

* [Reasoning] Optimize TPOT for thinking budget when used with speculative decoding (#46662)

Signed-off-by: rishitdholakia13 <rishit+github@cohere.com>
Co-authored-by: Cursor <cursoragent@cursor.com>

* [CI] Build macOS arm64 CPU wheel natively on the macmini queue (#48289)

Signed-off-by: mgoin <mgoin64@gmail.com>

* [ROCm][CI] fix flashinfer import check (#48647)

Signed-off-by: Divakar Verma <divakar.verma@amd.com>

* [CI][Bugfix] Fix FlashAttention reported MLA dimension support (#48631)

Signed-off-by: Matthew Bonanni <mbonanni@redhat.com>

* Log fully resolved pooling config at startup (#48030)

Signed-off-by: Taneem Ibrahim <taneem.ibrahim@gmail.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* [Bugfix] MoRIIO toy P/D proxy: add /health (#45222)

Signed-off-by: Chaemin Lim <chaemin.lim@mangoboost.io>
Signed-off-by: Edwin Lim <edwinlim0919@gmail.com>
Co-authored-by: Edwin Lim <edwin.lim@mangoboost.io>
Co-authored-by: Jaeyoun Kim <jaeyoun.kim@mangoboost.io>
Co-authored-by: Edwin Lim <edwinlim0919@gmail.com>

* [Bugfix][CI] Fix test_head_dtype quant_method test on ROCm (#48654)

Signed-off-by: Micah Williamson <micah.williamson@amd.com>

* fix: size FlashInfer prefill workspace to batch head footprint (#48428)

Signed-off-by: Joe Rowell <joerowell4@gmail.com>

* [Bugfix][R3] Exclude draft routers from expert capture (#48622)

Signed-off-by: aoshen02 <aoshen@inferact.ai>

* [Bugfix] Preserve unloaded non-persistent buffers during layerwise reload (#44371)

Signed-off-by: Joan Velja <joan.velja22@gmail.com>
Co-authored-by: Dakai An <77474977+andakai@users.noreply.github.com>

* [Perf] Remove redundant repeat and copy for dsv4, 1.8% E2E TPOT improvement. (#48137)

Signed-off-by: yewentao256 <zhyanwentao@126.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>

* add pad-aware reduce path (#48385)

Signed-off-by: gnovack <novackgm@gmail.com>

* [ROCm][CI] Remove mxfp4 test skips after `amd-quark` 0.12 release (#47330)

Signed-off-by: Micah Williamson <micah.williamson@amd.com>
Signed-off-by: Andreas Karatzas <akaratza@amd.com>
Signed-off-by: Andreas Karatzas <Andreas.Karatzas@amd.com>
Co-authored-by: Andreas Karatzas <Andreas.Karatzas@amd.com>
Co-authored-by: fxmarty-amd <felmarty@amd.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [Core][LoRA] Support fp32 lm_head (head_dtype) on the LoRA path (#48525)

Signed-off-by: Karthik Kothuri <karthikkothuri2009@gmail.com>
Co-authored-by: Claude <noreply@anthropic.com>

* [CI][AMD] Configure MI300 tests for native execution without DinD (#48387)

Signed-off-by: Andreas Karatzas <akaratza@amd.com>

* [Quant] Enable humming w[2-7]a[4,8] inference with compressed-tensors (#46390)

Signed-off-by: HDCharles <charlesdavidhernandez@gmail.com>

* [LoRA][1/N] Integrate flashinfer MoE LoRA for BF16 model (#48632)

Signed-off-by: Jee Jee Li <jeejeelee@inferact.ai>

* [Security] Replace diskcache to eliminate pickle deserialization (#44549)

Signed-off-by: Russell Bryant <rbryant@redhat.com>
Co-authored-by: Claude <noreply@anthropic.com>

* Build with ABI stable FlashMLA (#48174)

Signed-off-by: Jane Xu <janeyx@meta.com>
Signed-off-by: Shengqi Chen <i@harrychen.xyz>
Co-authored-by: Shengqi Chen <i@harrychen.xyz>

* [Bugfix] Set kv_quant_mode on the generic MLA KV-cache spec (#48379)

Signed-off-by: Mikhail Kostryukov <mike@triptrack.net>
Signed-off-by: Matthew Bonanni <mbonanni@redhat.com>
Co-authored-by: Matthew Bonanni <mbonanni@redhat.com>

* [Bugfix][Spec Decode] Support heterogeneous QK fusion geometry (#48671)

Signed-off-by: aoshen02 <aoshen@inferact.ai>
Co-authored-by: OpenAI Codex <codex@openai.com>

* [ROCm] Run init test engine in-process to avoid KV-cache OOM (#48527)

Signed-off-by: Djordje Ramic <djoramic@amd.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [ROCm][CI] fix test_common.py (#48676)

Signed-off-by: charlifu <charlifu@amd.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [Rust Frontend] Integrate MM audio support (#48554)

Co-authored-by: Isotr0py <Isotr0py@outlook.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>
Signed-off-by: Bugen Zhao <i@bugenzhao.com>
Signed-off-by: Isotr0py <Isotr0py@outlook.com>

* [CI/Build] Split release artifact annotations by type (#48600)

Signed-off-by: khluu <khluu000@gmail.com>
Co-authored-by: OpenAI Codex <noreply@openai.com>

* [ROCm] Add tuned selective_state_update config for AMD MI350 (#48159)

Signed-off-by: Giuseppe Grossi <ggrossi@amd.com>

* fix flaky multi example connector consistency (#48206)

Signed-off-by: aarushjain29 <aarushi.jain2@amd.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>

* [ROCm][Bugfix] Enable the fp32 head_dtype torch.mm fast path on ROCm (#48688)

Signed-off-by: Turner Jabbour <doubleujabbour@gmail.com>
Co-authored-by: Claude <noreply@anthropic.com>

---------

Signed-off-by: Simon Mo <simon.mo@hey.com>
Signed-off-by: AmeenP <ameenp360@gmail.com>
Signed-off-by: yewentao256 <zhyanwentao@126.com>
Signed-off-by: zhenwei-intel <zhenwei.liu@intel.com>
Signed-off-by: Hongbin10 <jdmjdm1998@163.com>
Signed-off-by: LopezCastroRoberto <rocastro@redhat.com>
Signed-off-by: StingLin <sting.lin@cienet.com>
Signed-off-by: Nils Matteson <nils@thaw.sh>
Signed-off-by: Nils Matteson <nilsmatteson@icloud.com>
Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
Signed-off-by: Matthew Bonanni <mbonanni@redhat.com>
Signed-off-by: Nick Cao <ncao@redhat.com>
Signed-off-by: Sahil170595 <147995121+Sahil170595@users.noreply.github.com>
Signed-off-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>
Signed-off-by: jasonlizhengjian <jasonlizhengjian@gmail.com>
Signed-off-by: jperezde <jperezde@redhat.com>
Signed-off-by: Yifan Zong <yzong@redhat.com>
Signed-off-by: AmeenP <ameen@primeintellect.ai>
Signed-off-by: stefankoncarevic <stefan.koncarevic@amd.com>
Signed-off-by: Rohan Potdar <rohan.potdar@amd.com>
Signed-off-by: Rahul Vishwakarma <Rahul.Vishwakarma2@ibm.com>
Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>
Signed-off-by: Isotr0py <Isotr0py@outlook.com>
Signed-off-by: jacklin78911-collab <jacklin78911@gmail.com>
Signed-off-by: walterbm <walter.beller.morales@gmail.com>
Signed-off-by: Ting Sun <suntcrick@gmail.com>
Signed-off-by: Yongye Zhu <zyy1102000@gmail.com>
Signed-off-by: amd-ethany <amd-ethany@users.noreply.github.com>
Signed-off-by: Jeff Ma <jeffjma@umich.edu>
Signed-off-by: wzhao18 <wzhao18.sz@gmail.com>
Signed-off-by: Zach Zhu <zzqshu@126.com>
Signed-off-by: Nicklas Frahm <nicklas.frahm@gmail.com>
Signed-off-by: muhammadfawaz1 <135441198+muhammadfawaz1@users.noreply.github.com>
Signed-off-by: Hongxia Yang <hongxia.yang@amd.com>
Signed-off-by: Ace Eldeib <aeldeib@coreweave.com>
Signed-off-by: Ace Eldeib <alexeldeib@gmail.com>
Signed-off-by: NickLucche <nicolo.lucchesi@mistral.ai>
Signed-off-by: Tyler Michael Smith <tyler@tylermsmith.com>
Signed-off-by: Tyler Michael Smith <tlrmchlsmth@gmail.com>
Signed-off-by: Chaojun Zhang <chaojun.zhang@intel.com>
Signed-off-by: sungbin1015 <sbin@solbox.com>
Signed-off-by: gcanlin <canlinguosdu@gmail.com>
Signed-off-by: Thien Tran <gau.nernst@yahoo.com.sg>
Signed-off-by: 辰言 <oncwnuIWp30GguOyJ615Fqj8H-yc@git.weixin.qq.com>
Signed-off-by: Jee Jee Li <jeejeelee@inferact.ai>
Signed-off-by: Bugen Zhao <i@bugenzhao.com>
Signed-off-by: Randall Smith <Randall.Smith@amd.com>
Signed-off-by: Saddss <28726669061@qq.com>
Signed-off-by: Nick Hill <nickhill123@gmail.com>
Signed-off-by: Yifan Qiao <yifanqiao@inferact.ai>
Signed-off-by: vivek sharma <vivsharm@redhat.com>
Signed-off-by: puneetsharma21 <puneet.sharma21@ibm.com>
Signed-off-by: Puneet Sharma <puneet.sharma21@ibm.com>
Signed-off-by: Robert Shaw <robertgshaw2-redhat@dgx-b200-02.mgmt.accl-001.lab.rdu2.dc.redhat.com>
Signed-off-by: mgoin <mgoin64@gmail.com>
Signed-off-by: Martin Vit <martin@voipmonitor.org>
Signed-off-by: Shawn Tsai <shawnyht@gmail.com>
Signed-off-by: Anna Mayne <anna.mayne@arm.com>
Signed-off-by: Markov Ilya <markovilya19@gmail.com>
Signed-off-by: Chris Leonard <chleonar@redhat.com>
Signed-off-by: Kaihang Jiang <kaihangj@login-lyris02.lyris.clusters.nvidia.com>
Signed-off-by: Djordje Ramic <djoramic@amd.com>
Signed-off-by: Itay Etelis <itay.etelis@ibm.com>
Signed-off-by: Itay Etelis <92247226+Etelis@users.noreply.github.com>
Signed-off-by: Or Ozeri <oro@il.ibm.com>
Signed-off-by: Roberto L. Castro <38211239+LopezCastroRoberto@users.noreply.github.com>
Signed-off-by: Andreas Karatzas <akaratza@amd.com>
Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>
Signed-off-by: zixi-qi <zixi@inferact.ai>
Signed-off-by: Dino Music <Dino.Music@amd.com>
Signed-off-by: chaunceyjiang <chaunceyjiang@gmail.com>
Signed-off-by: jiang1.li <jiang1.li@intel.com>
Signed-off-by: JooHo Lee <BWAAEEEK@users.noreply.github.com>
Signed-off-by: JooHo Lee <jooho414@gmail.com>
Signed-off-by: Xianbao QIAN <xianbao.qian@gmail.com>
Signed-off-by: wenjun.liu <wenjun.liu@intel.com>
Signed-off-by: jun,du <jun.du@intel.com>
Signed-off-by: Benjamin Chislett <bchislett@nvidia.com>
Signed-off-by: Benjamin Chislett <chislett.ben@gmail.com>
Signed-off-by: Cyrus Leung <tlleungac@connect.ust.hk>
Signed-off-by: Woosuk Kwon <woosuk@inferact.ai>
Signed-off-by: khluu <khluu000@gmail.com>
Signed-off-by: Tsvika Shapira <tsvika@moonmath.ai>
Signed-off-by: charlifu <charlifu@amd.com>
Signed-off-by: Lucas Wilkinson <lwilkins@redhat.com>
Signed-off-by: deng451e <838677410@qq.com>
Signed-off-by: Teemu Virolainen <teemu.virolainen@amd.com>
Signed-off-by: Zhewen Li <zhewenli@inferact.ai>
Signed-off-by: zihaomu <zmu@amd.com>
Signed-off-by: Micah Williamson <micah.williamson@amd.com>
Signed-off-by: gnovack <novackgm@gmail.com>
Signed-off-by: pei.zhang <pei.zhang@amd.com>
Signed-off-by: augusto.yjh <augusto.yjh@antgroup.com>
Signed-off-by: Change72 <changg@nvidia.com>
Signed-off-by: YanXu <yancey.yx@alibaba-inc.com>
Signed-off-by: Jingyi Yang <girasoleyang@gmail.com>
Signed-off-by: Lei Gong <gonglei25@huawei.com>
Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>
Signed-off-by: zjy0516 <riverclouds.zhu@qq.com>
Signed-off-by: Mingfei Guo <1800012773@pku.edu.cn>
Signed-off-by: Viranjan Pagar <vpagar@nvidia.com>
Signed-off-by: Isotr0py <mozf@mail2.sysu.edu.cn>
Signed-off-by: reidliu41 <reid201711@gmail.com>
Signed-off-by: Yuchen Fan <functionhx@gmail.com>
Signed-off-by: Xu Zhou <xuzhou9417@163.com>
Signed-off-by: Yipeng Hu <i26268@metax-tech.com>
Signed-off-by: HuYiPeng <144002351+MynameFelix@users.noreply.github.com>
Signed-off-by: larryli2-amd <larryli2@amd.com>
Signed-off-by: larryli2-amd <Larry.Li@amd.com>
Signed-off-by: Ashwin Giridharan <girida@amazon.com>
Signed-off-by: Michael Goin <mgoin64@gmail.com>
Signed-off-by: Joe Rowell <joerowell4@gmail.com>
Signed-off-by: Jimmy Lee <hirejimmylee@gmail.com>
Signed-off-by: Robert Shaw <robertgshaw2-redhat@ip-172-31-18-125.us-east-2.compute.internal>
Signed-off-by: mayuyuace <qiming1.zhang@intel.com>
Signed-off-by: peiyuanz <peiyuanz@inferact.ai>
Signed-off-by: Lai, Yejing <yejing.lai@intel.com>
Signed-off-by: Lucas Wilkinson <LucasWilkinson@users.noreply.github.com>
Signed-off-by: ErenAta16 <erena6466@gmail.com>
Signed-off-by: Ievgen Bondarenko <ibondarenko@student.sierracollege.edu>
Signed-off-by: Ievgen (Jack) Bondarenko <ibondarenko@student.sierracollege.edu>
Signed-off-by: AlejandroParedesLT <alejandroparedeslatorre@gmail.com>
Signed-off-by: Zhenhui Zhao <zhenhui.zhao@intel.com>
Signed-off-by: vx120 <893600387@qq.com>
Signed-off-by: vx120 <57470515+vx120@users.noreply.github.com>
Signed-off-by: aoshen02 <aoshen@inferact.ai>
Signed-off-by: hcenteno <hugo.centeno@estudiantat.upc.edu>
Signed-off-by: Liran Schour <lirans@il.ibm.com>
Signed-off-by: omerpaz95 <omerpaz95@gmail.com>
Signed-off-by: Alex <alex.tech.lab@outlook.com>
Signed-off-by: Tanish Malekar <tanishmalekar32@gmail.com>
Signed-off-by: Tan Pin Siang <tanpinsiang@gmail.com>
Signed-off-by: tjtanaa <tunjian.tan@embeddedllm.com>
Signed-off-by: zzt <zengzetang.zzt@antgroup.com>
Signed-off-by: fai <fangzhouai@gmail.com>
Signed-off-by: guybd <guy.boudoukh@intel.com>
Signed-off-by: Guy Boudoukh <guy.boudoukh@intel.com>
Signed-off-by: Rehan Khan <Rehan.Khan7@ibm.com>
Signed-off-by: Matthew Wong <Matthew.Wong2@amd.com>
Signed-off-by: Mohammad Miadh Angkad <176301910+mmangkad@users.noreply.github.com>
Signed-off-by: Karthik Kothuri <karthikkothuri2009@gmail.com>
Signed-off-by: wang.yuqi <yuqi.wang@daocloud.io>
Signed-off-by: Omer Ullman Argov <118735753+omera-nv@users.noreply.github.com>
Signed-off-by: Yan Ma <yan.ma@intel.com>
Signed-off-by: zengxian <xiangdong.zeng@intel.com>
Signed-off-by: Taneem Ibrahim <taneem.ibrahim@gmail.com>
Signed-off-by: Stefan Koncarevic <stefan.koncarevic@amd.com>
Signed-off-by: Gavin Morris <gmorriscs@gmail.com>
Signed-off-by: Aakif Nawaz <aakif.nawaz@amd.com>
Signed-off-by: Pavani Majety <pmajety@nvidia.com>
Signed-off-by: shanjiaz <zsjwpianpian@gmail.com>
Signed-off-by: wenpengw-nv <wenpengw@nvidia.com>
Signed-off-by: atalhens <sneh.lata@nutanix.com>
Signed-off-by: Lang Zhao <lang.zhao@galileo.ai>
Signed-off-by: emricksini-h <emrick.birivoutin@hcompany.ai>
Signed-off-by: Zhenzhong1 <zhenzhong.xu@intel.com>
Signed-off-by: Zhenzhong Xu <zhenzhong.xu@intel.com>
Signed-off-by: Matthias Gehre <matthias.gehre@amd.com>
Signed-off-by: Saeid Rostami <srostami@amd.com>
Signed-off-by: nemanjaudovic <nudovic@amd.com>
Signed-off-by: Matt Woodson <mwoodson@redhat.com>
Signed-off-by: wangxiyuan <wangxiyuan1007@gmail.com>
Signed-off-by: Bartosz Stefaniak <bstefaniak@nvidia.com>
Signed-off-by: Hoang Nguyen Tien <hoang.nguyentien.2601@gmail.com>
Signed-off-by: xiangdong <40376367+zxd1997066@users.noreply.github.com>
Signed-off-by: Artur Fierka <artur.fierka@intel.com>
Signed-off-by: gangula-karthik <gkarthik923@gmail.com>
Signed-off-by: Tomer Gilad <tgilad@nvidia.com>
Signed-off-by: Itay Etelis <Itay.etelis@gmail.com>
Signed-off-by: Adhithya Balakrishnan <adhithya.b2004@gmail.com>
Signed-off-by: Daoyuan Li <94409450+DaoyuanLi2816@users.noreply.github.com>
Signed-off-by: srinivas_oo7 <sklinkedin0120@gmail.com>
Signed-off-by: rishitdholakia13 <rishit+github@cohere.com>
Signed-off-by: Divakar Verma <divakar.verma@amd.com>
Signed-off-by: Chaemin Lim <chaemin.lim@mangoboost.io>
Signed-off-by: Edwin Lim <edwinlim0919@gmail.com>
Signed-off-by: Joan Velja <joan.velja22@gmail.com>
Signed-off-by: Andreas Karatzas <Andreas.Karatzas@amd.com>
Signed-off-by: HDCharles <charlesdavidhernandez@gmail.com>
Signed-off-by: Russell Bryant <rbryant@redhat.com>
Signed-off-by: Jane Xu <janeyx@meta.com>
Signed-off-by: Shengqi Chen <i@harrychen.xyz>
Signed-off-by: Mikhail Kostryukov <mike@triptrack.net>
Signed-off-by: Giuseppe Grossi <ggrossi@amd.com>
Signed-off-by: aarushjain29 <aarushi.jain2@amd.com>
Signed-off-by: Turner Jabbour <doubleujabbour@gmail.com>
Co-authored-by: Simon Mo <simon.mo@hey.com>
Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Benjamin Chislett <chislett.ben@gmail.com>
Co-authored-by: Ameen Patel <ameenp360@gmail.com>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: mergify[bot] <37929162+mergify[bot]@users.noreply.github.com>
Co-authored-by: Wentao Ye <44945378+yewentao256@users.noreply.github.com>
Co-authored-by: liuzhenwei <zhenweiliu@habana.ai>
Co-authored-by: Robin <jdmjdm1998@163.com>
Co-authored-by: Roberto L. Castro <38211239+LopezCastroRoberto@users.noreply.github.com>
Co-authored-by: Sting Lin <sting.lin@cienet.com>
Co-authored-by: Nils Matteson <nilsmatteson@icloud.com>
Co-authored-by: Nils Matteson <nils@thaw.sh>
Co-authored-by: Rishabh Saini <rishabhsaini01@gmail.com>
Co-authored-by: Benjamin Chislett <bchislett@nvidia.com>
Co-authored-by: Matthew Bonanni <mbonanni@redhat.com>
Co-authored-by: Nick Cao <ncao@redhat.com>
Co-authored-by: Sahil Kadadekar <147995121+Sahil170595@users.noreply.github.com>
Co-authored-by: vanshbhatia-amd <vansh.bhatia@amd.com>
Co-authored-by: vanshbhatia-amd <210711135+vanshbhatia-amd@users.noreply.github.com>
Co-authored-by: Jason Li <jasonlizhengjian@gmail.com>
Co-authored-by: Juan Pérez de Algaba <124347725+jperezdealgaba@users.noreply.github.com>
Co-authored-by: yzong-rh <yzong@redhat.com>
Co-authored-by: AmeenP <ameen@primeintellect.ai>
Co-authored-by: stefankoncarevic <skoncare@amd.com>
Co-authored-by: Rohan Potdar <rohan.potdar@amd.com>
Co-authored-by: Rahul Vishwakarma <168823860+rahulssv-ibm@users.noreply.github.com>
Co-authored-by: Martin Hickey <martin.hickey@ie.ibm.com>
Co-authored-by: Isotr0py <Isotr0py@outlook.com>
Co-authored-by: Posedge_Lin <jacklin78911@gmail.com>
Co-authored-by: Or Ozeri <oro@il.ibm.com>
Co-authored-by: Walter Beller-Morales <walterbm@users.noreply.github.com>
Co-authored-by: Ting SUN <suntcrick@gmail.com>
Co-authored-by: Yongye Zhu <zyy1102000@gmail.com>
Co-authored-by: Ethan Yang <ethany@amd.com>
Co-authored-by: amd-ethany <amd-ethany@users.noreply.github.com>
Co-authored-by: Jeff (Junze) Ma <93145857+majunze2001@users.noreply.github.com>
Co-authored-by: Lucas Wilkinson <LucasWilkinson@users.noreply.github.com>
Co-authored-by: Wei Zhao <51183510+wzhao18@users.noreply.github.com>
Co-authored-by: Zach Zhu <zzqshu@126.com>
Co-authored-by: Nicklas Frahm <nicklas.frahm@gmail.com>
Co-authored-by: Muhammad Fawaz <mfawaz182@gmail.com>
Co-authored-by: Mahad Durrani <114791389+mahadrehmann@users.noreply.github.com>
Co-authored-by: Hongxia Yang <62075498+hongxiayang@users.noreply.github.com>
Co-authored-by: Ace Eldeib <aeldeib@coreweave.com>
Co-authored-by: manayang <jackmanayang@gmail.com>
Co-authored-by: manayang <manayang@tencent.com>
Co-authored-by: Nicolò Lucchesi <nlucches@redhat.com>
Co-authored-by: Tyler Michael Smith <tlrmchlsmth@gmail.com>
Co-authored-by: Chaojun Zhang <chaojun.zhang@intel.com>
Co-authored-by: sungbin1015 <sbin@solbox.com>
Co-authored-by: Canlin Guo <canlinguosdu@gmail.com>
Co-authored-by: Thien Tran <gau.nernst@yahoo.com.sg>
Co-authored-by: aoright <102943475+aoright@users.noreply.github.com>
Co-authored-by: 辰言 <oncwnuIWp30GguOyJ615Fqj8H-yc@git.weixin.qq.com>
Co-authored-by: Jee Jee Li <jeejeelee@inferact.ai>
Co-authored-by: Jee Jee Li <pandaleefree@gmail.com>
Co-authored-by: Bugen Zhao <i@bugenzhao.com>
Co-authored-by: rasmith <Randall.Smith@amd.com>
Co-authored-by: Saddss <108515797+Saddss@users.noreply.github.com>
Co-authored-by: Yifan Qiao <yifanqiao@inferact.ai>
Co-authored-by: Saddss <28726669061@qq.com>
Co-authored-by: Nick Hill <nickhill123@gmail.com>
Co-authored-by: Vivek Sharma <Vivek.Sharma20@ibm.com>
Co-authored-by: vivek sharma <vivsharm@redhat.com>
Co-authored-by: Puneet Sharma <puneet.sharma21@ibm.com>
Co-authored-by: depthfirst-app[bot] <184448029+depthfirst-app[bot]@users.noreply.github.com>
Co-authored-by: Robert Shaw <114415538+robertgshaw2-redhat@users.noreply.github.com>
Co-authored-by: Robert Shaw <robertgshaw2-redhat@dgx-b200-02.mgmt.accl-001.lab.rdu2.dc.redhat.com>
Co-authored-by: Michael Goin <mgoin64@gmail.com>
Co-authored-by: Martin Vit <martin@voipmonitor.org>
Co-authored-by: shawn <shawnyht@gmail.com>
Co-authored-by: almayne <anna.mayne@arm.com>
Co-authored-by: Ilya Markov <markovilya197@gmail.com>
Co-authored-by: Markov Ilya <markovilya19@gmail.com>
Co-authored-by: Chris Leonard <chleonar@redhat.com>
Co-authored-by: Kaihang Jiang <88449510+kjiang249@users.noreply.github.com>
Co-authored-by: djramic <djoramic@amd.com>
Co-authored-by: Itay Etelis <92247226+Etelis@users.noreply.github.com>
Co-authored-by: Itay Etelis <itay.etelis@ibm.com>
Co-authored-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>
Co-authored-by: Andreas Karatzas <akaratza@amd.com>
Co-authored-by: qizixi <22851944+zixi-qi@users.noreply.github.com>
Co-authored-by: music-dino <111048524+music-dino@users.noreply.github.com>
Co-authored-by: Chauncey <chaunceyjiang@gmail.com>
Co-authored-by: Li, Jiang <jiang1.li@intel.com>
Co-authored-by: JooHo Lee <96564470+BWAAEEEK@users.noreply.github.com>
Co-authored-by: JooHo Lee <BWAAEEEK@users.noreply.github.com>
Co-authored-by: Tiezhen WANG <38108242+xianbaoqian@users.noreply.github.com>
Co-authored-by: wenjun liu <wenjun.liu@intel.com>
Co-authored-by: jun,du <jun.du@intel.com>
Co-authored-by: Kunshang Ji <kunshang.ji@intel.com>
Co-authored-by: Cyrus Leung <tlleungac@connect.ust.hk>
Co-authored-by: Woosuk Kwon <woosuk@inferact.ai>
Co-authored-by: Kevin H. Luu <khluu000@gmail.com>
Co-authored-by: Brandon Pelfrey <bpelfrey@nvidia.com>
Co-authored-by: Tsvika Shapira <tsvikas@gmail.com>
Co-authored-by: Charlie Fu <charlifu@amd.com>
Co-authored-by: Codex <codex@openai.com>
Co-authored-by: weishu <838677410@qq.com>
Co-authored-by: Teemu Virolainen <teemu.virolainen@amd.com>
Co-authored-by: TJian <tunjian.tan@embeddedllm.com>
Co-authored-by: Zhewen Li <zhewenli@meta.com>
Co-authored-by: Zhewen Li <zhewenli@inferact.ai>
Co-authored-by: ZihaoMu <zmu@amd.com>
Co-authored-by: Micah Williamson <micah.williamson@amd.com>
Co-authored-by: ap9272 <ap9272@users.noreply.github.com>
Co-authored-by: gnovack <novackgm@gmail.com>
Co-authored-by: peizhang56 <pei.zhang@amd.com>
Co-authored-by: Augusto Yao <augusto.yjh@antgroup.com>
Co-authored-by: Chang Guo <changg@nvidia.com>
Co-authored-by: Yan Xu <yancey.yx@alibaba-inc.com>
Co-authored-by: Jingyi Yang <girasoleyang@gmail.com>
Co-authored-by: GongLei-HW <1327185943@qq.com>
Co-authored-by: Lei Gong <gonglei25@huawei.com>
Co-authored-by: alberto <aperdomo@redhat.com>
Co-authored-by: Jiangyun Zhu <riverclouds.zhu@qq.com>
Co-authored-by: Mingfei Guo <52491257+guoriyue@users.noreply.github.com>
Co-authored-by: Andrey Talman <atalman@fb.com>
Co-authored-by: Andrey Talman <atalman@users.noreply.github.com>
Co-authored-by: ViranjanPagar <vpagar@nvidia.com>
Co-authored-by: Isotr0py <mozf@mail2.sysu.edu.cn>
Co-authored-by: Roger Wang <hey@rogerw.io>
Co-authored-by: Reid <61492567+reidliu41@users.noreply.github.com>
Co-authored-by: FAN YUCHEN <2994114386@qq.com>
Co-authored-by: XuZhou <17717803682@163.com>
Co-authored-by: Xu Zhou <xuzhou9417@163.com>
Co-authored-by: Hoseung Kim <ghyutjik123@gmail.com>
Co-authored-by: HuYiPeng <144002351+MynameFelix@users.noreply.github.com>
Co-authored-by: Yipeng Hu <i26268@metax-tech.com>
Co-authored-by: larryli2-amd <Larry.Li@amd.com>
Co-authored-by: Ashwin Giridharan <ashwing@users.noreply.github.com>
Co-authored-by: Joe Rowell <joerowell4@gmail.com>
Co-authored-by: Jimmy Lee <58957694+thisisjimmyfb@users.noreply.github.com>
Co-authored-by: Robert Shaw <robertgshaw2-redhat@ip-172-31-18-125.us-east-2.compute.internal>
Co-authored-by: Qiming Zhang <qiming1.zhang@intel.com>
Co-authored-by: peiyuanz <peiyuanz@inferact.ai>
Co-authored-by: zhouzhou <zhouzhou@zhouzhoudeMacBook-Pro.local>
Co-authored-by: Yejing Lai <yejing.lai@intel.com>
Co-authored-by: ErenAta16 <erena6466@gmail.com>
Co-authored-by: Ievgen Bondarenko <ibondarenko@student.sierracollege.edu>
Co-authored-by: Alejandro Paredes La Torre <99832715+AlejandroParedesLT@users.noreply.github.com>
Co-authored-by: zhao, zhenhui <zhenhui.zhao@intel.com>
Co-authored-by: vx120 <57470515+vx120@users.noreply.github.com>
Co-authored-by: crp0128 <191679376@qq.com>
Co-authored-by: aoshen02 <aoshen@inferact.ai>
Co-authored-by: yewentao256 <zhyanwentao@126.com>
Co-authored-by: Hugo Centeno <133872718+hugo-cen@users.noreply.github.com>
Co-authored-by: liranschour <liranschour@users.noreply.github.com>
Co-authored-by: omerpaz95 <73347585+omerpaz95@users.noreply.github.com>
Co-authored-by: AlexHuang <alex.tech.lab@outlook.com>
Co-authored-by: Tanish Malekar <60835372+tanish-malekar@users.noreply.github.com>
Co-authored-by: Tan Pin Siang <tanpinsiang@gmail.com>
Co-authored-by: vllmellm <vllm.ellm@embeddedllm.com>
Co-authored-by: Hongxia Yang <hongxia.yang@amd.com>
Co-authored-by: Jun Kang Chow <junkangchow@gmail.com>
Co-authored-by: zzt <zengzetang.zzt@antgroup.com>
Co-authored-by: Fangzhou Ai <31551580+Fangzhou-Ai@users.noreply.github.com>
Co-authored-by: guybd <guy.boudoukh@intel.com>
Co-authored-by: Rehan Khan <Rehan.Khan7@ibm.com>
Co-authored-by: Matthew Wong <Matthew.Wong2@amd.com>
Co-authored-by: Mohammad Miadh Angkad …
```

[philippesic](https://github.com/philippesic) pushed a commit to philippesic/vllm-semantic-cache that referenced this pull request [ Jul 19, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-commit-05117b1)
` fix: include topic frame in KV events replay response ([](https://github.com/philippesic/vllm-semantic-cache/commit/05117b17aefd9a3bc7437f5afa371468b7d514ab "fix: include topic frame in KV events replay response \(#45177\)

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>")vllm-project#4…[](https://github.com/vllm-project/vllm/pull/45177) `

```
…5177[](https://github.com/vllm-project/vllm/pull/45177))

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Aug 6, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-pullrequest-5084150307)
[ feat(precise): enable KV event replay buffer for index recovery llm-d/llm-d#2196 ](https://github.com/llm-d/llm-d/pull/2196)
Merged 
3 tasks
[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d that referenced this pull request [ Aug 6, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-commit-9c49dc3)
` feat(precise): enable KV event replay buffer for index recovery[](https://github.com/RishabhSaini/llm-d/commit/9c49dc389b5b4a6ceb63adc65c6a9333fbd29e6c "feat\(precise\): enable KV event replay buffer for index recovery

Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix \(vllm-project/vllm#45177\). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends \(CPU, AMD ROCm, XPU, TPU\) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix (vllm-project/vllm#45177[](https://github.com/vllm-project/vllm/pull/45177)). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends (CPU, AMD ROCm, XPU, TPU) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d that referenced this pull request [ Aug 6, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-commit-e3019ed)
` feat(precise): enable KV event replay buffer for index recovery[](https://github.com/RishabhSaini/llm-d/commit/e3019edbdfdc7a5f9880833684c3a24edafa1df7 "feat\(precise\): enable KV event replay buffer for index recovery

Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix \(vllm-project/vllm#45177\). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends \(CPU, AMD ROCm, XPU, TPU\) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix (vllm-project/vllm#45177[](https://github.com/vllm-project/vllm/pull/45177)). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends (CPU, AMD ROCm, XPU, TPU) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d that referenced this pull request [ Aug 6, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-commit-d8f57fa)
` feat(precise): enable KV event replay buffer for index recovery[](https://github.com/RishabhSaini/llm-d/commit/d8f57fa06b4054b460fc730dabcdd2b429398fbc "feat\(precise\): enable KV event replay buffer for index recovery

Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix \(vllm-project/vllm#45177\). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends \(CPU, AMD ROCm, XPU, TPU\) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix (vllm-project/vllm#45177[](https://github.com/vllm-project/vllm/pull/45177)). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends (CPU, AMD ROCm, XPU, TPU) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[vMaroon](https://github.com/vMaroon) pushed a commit to llm-d/llm-d that referenced this pull request [ Aug 6, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-commit-def3f15)
` feat(precise): enable KV event replay buffer for index recovery ([](https://github.com/llm-d/llm-d/commit/def3f1580da244587f07167a33af8b1f6480a614 "feat\(precise\): enable KV event replay buffer for index recovery \(#2196\)

Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix \(vllm-project/vllm#45177\). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends \(CPU, AMD ROCm, XPU, TPU\) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>")#2196[](https://github.com/llm-d/llm-d/pull/2196))[](https://github.com/llm-d/llm-d/commit/def3f1580da244587f07167a33af8b1f6480a614 "feat\(precise\): enable KV event replay buffer for index recovery \(#2196\)

Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix \(vllm-project/vllm#45177\). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends \(CPU, AMD ROCm, XPU, TPU\) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix (vllm-project/vllm#45177[](https://github.com/vllm-project/vllm/pull/45177)). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends (CPU, AMD ROCm, XPU, TPU) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[avinashsingh77](https://github.com/avinashsingh77) pushed a commit to avinashsingh77/llm-d that referenced this pull request [ Aug 25, 2026 ](https://github.com/vllm-project/vllm/pull/45177#ref-commit-bc6ba07)
` feat(precise): enable KV event replay buffer for index recovery ([](https://github.com/avinashsingh77/llm-d/commit/bc6ba07f0d7f6b982a23fd8cc276f60515c9b75a "feat\(precise\): enable KV event replay buffer for index recovery \(#2196\)

Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix \(vllm-project/vllm#45177\). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends \(CPU, AMD ROCm, XPU, TPU\) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>")llm-…[](https://github.com/llm-d/llm-d/pull/2196) `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[avinashsingh77](https://github.com/avinashsingh77) Avinash Singh 
SSH Key Fingerprint: CQkdTxn02OS4ZY5teSEZBO6z3YdFrv+fCX0XWTPSSSE
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…d#2196[](https://github.com/llm-d/llm-d/pull/2196))

Configure vLLM's ZMQ replay buffer for the GPU model server backend
so the router can rebuild its KV-block index after an EPP restart
without waiting for live traffic.

Bump the GPU vLLM image from v0.25.0 to v0.26.0 which includes
the replay topic frame fix (vllm-project/vllm#45177[](https://github.com/vllm-project/vllm/pull/45177)). Each GPU model
server now binds a ZMQ ROUTER socket on port 5557 alongside the
existing PUB socket on 5556. The router's podDiscoveryConfig connects
to both per pod: PUB for live events, ROUTER for replay requests on
first connect or after reconnection.

Non-GPU backends (CPU, AMD ROCm, XPU, TPU) remain unchanged as their
vLLM images are on v0.23.0 or earlier which lack replay support.

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fvllm-project%2Fvllm%2Fpull%2F45177)
###  Labels 
[ documentation ](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3Adocumentation) Improvements or additions to documentation [ ready ](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3Aready) ONLY add when PR is ready to merge/full CI is needed
###  3 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
