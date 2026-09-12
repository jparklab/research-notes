---
title: 'GitHub - ai-dynamo/dynamo: A Datacenter Scale Distributed Inference Serving
  Framework · GitHub'
id: github-ai-dynamodynamo-a-datacenter-scale-distributed-inference-serving-framewor
tags:
- llmd-router-nonk8s
- dynamo
- service-discovery
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:46.237470Z'
updated: '2026-09-12T03:37:37.582392Z'
source: https://github.com/ai-dynamo/dynamo
source_domain: github.com
fetched_at: '2026-09-11T19:44:46.215797Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'ai-dynamo/dynamo GitHub repo README (fetched at HEAD, v1.4.2 latest release,
  Rust 52.7%/Python 34.6%/Go 10.5%). Dynamo is NVIDIA''s orchestration layer above
  vLLM/SGLang/TensorRT-LLM providing disaggregated P/D, KV-aware routing, multi-tier
  KV offload (KVBM: GPU->CPU->SSD->remote), and SLA-driven autoscaling (Planner).
  Directly documents non-K8s worker discovery: a ''Service Discovery and Messaging''
  table states etcd and NATS are NOT required for local development (pass --discovery-backend
  file) or for Kubernetes (native CRDs+EndpointSlices handle it over TCP); etcd or
  NATS JetStream-backed modes are needed only ''for Slurm or other distributed deployments''
  that aren''t K8s-native — etcd can run directly as ./etcd, NATS needs ''-js'' for
  JetStream. Also documents two routing topologies: (1) Dynamo-native Frontend routing,
  where the integrated Dynamo Router (client -> Frontend -> Router -> workers) makes
  worker-selection decisions with no external gateway required — the K8s-independent
  path; and (2) Gateway API routing via GAIE, where a Kubernetes Gateway API Inference
  Extension gateway calls the Dynamo Endpoint Picker Plugin (EPP) before forwarding
  to a worker''s Frontend sidecar in --router-mode direct. Cites benchmark claims
  (7x higher throughput/GPU on GB200 NVL72 vs B200 without Dynamo, 2x faster TTFT
  via KV-aware routing per a Baseten blog, 750x higher throughput on GB300 via InferenceX)
  sourced from third-party/vendor blogs, not the repo''s own controlled benchmark
  harness.'
utility_score: 11.0
---

[Skip to content](https://github.com/ai-dynamo/dynamo#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/ai-dynamo/dynamo) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/ai-dynamo/dynamo) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/ai-dynamo/dynamo) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/ai-dynamo/dynamo).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fai-dynamo%2Fdynamo) You must be signed in to change notification settings
  * [ Fork 1.6k ](https://github.com/login?return_to=%2Fai-dynamo%2Fdynamo)
  * [ Star  8k ](https://github.com/login?return_to=%2Fai-dynamo%2Fdynamo)


[**3475** Branches](https://github.com/ai-dynamo/dynamo/branches)[](https://github.com/ai-dynamo/dynamo/tags)
Go to file
Open more actions menu
## Latest commit
[fix(kv-router): make standalone indexer cleanup safe (](https://github.com/ai-dynamo/dynamo/commit/6c66d4b2a6345edda51f17f7c150862fee37385d)[#14652](https://github.com/ai-dynamo/dynamo/pull/14652)[)](https://github.com/ai-dynamo/dynamo/commit/6c66d4b2a6345edda51f17f7c150862fee37385d)
Open commit detailspending
Sep 11, 2026
[6c66d4b](https://github.com/ai-dynamo/dynamo/commit/6c66d4b2a6345edda51f17f7c150862fee37385d) · Sep 11, 2026
## History
[7,203 Commits](https://github.com/ai-dynamo/dynamo/commits/main/)
Open commit details
7,203 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| [.agents/skills](https://github.com/ai-dynamo/dynamo/tree/main/.agents/skills "This path skips through empty directories")  | [.agents/skills](https://github.com/ai-dynamo/dynamo/tree/main/.agents/skills "This path skips through empty directories")  | [feat(skills): report-skillpack-issue - defect telemetry from running …](https://github.com/ai-dynamo/dynamo/commit/744f581589c60f6d5f5d91fe6b17fddf192da986 "feat\(skills\): report-skillpack-issue - defect telemetry from running agents \(#14073\)

Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")  | Sep 11, 2026  |  
|  [chore: require lazy Python logging formatting in CodeRabbit reviews (](https://github.com/ai-dynamo/dynamo/commit/3632e36ae40b96dc8ccd83b3bd9955cdbcede929 "chore: require lazy Python logging formatting in CodeRabbit reviews \(#10305\)

Signed-off-by: Alec Flowers <aflowers@nvidia.com>")[#…](https://github.com/ai-dynamo/dynamo/pull/10305)  | Jun 4, 2026  |  
|   |   |  [chore: Force frame pointers to ease profiling and debugging (](https://github.com/ai-dynamo/dynamo/commit/22805f02c6954e6ab6280cca35868abea2d29c92 "chore: Force frame pointers to ease profiling and debugging \(#9489\)

Signed-off-by: Graham King <grahamk@nvidia.com>")[#9489](https://github.com/ai-dynamo/dynamo/pull/9489)[)](https://github.com/ai-dynamo/dynamo/commit/22805f02c6954e6ab6280cca35868abea2d29c92 "chore: Force frame pointers to ease profiling and debugging \(#9489\)

Signed-off-by: Graham King <grahamk@nvidia.com>")  | May 13, 2026  |  
|   |   | [chore: Make our agent skills work with all agents, not just Claude Co…](https://github.com/ai-dynamo/dynamo/commit/665b54b76cdfa3dedff6be3c2f347427256451bd "chore: Make our agent skills work with all agents, not just Claude Code \(#8840\)

Signed-off-by: Graham King <grahamk@nvidia.com>")  | Apr 29, 2026  |  
|   |   | [feat(skills): baseline authoring and recipe confirmation when the use…](https://github.com/ai-dynamo/dynamo/commit/2a6da14c83617f5412caba5777dbf0e2399e9c5f "feat\(skills\): baseline authoring and recipe confirmation when the user has no existing DGD \(#13625\)

Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")  | Aug 22, 2026  |  
|   |   |  [refactor(mm-routing): generalize MM routing terminology (](https://github.com/ai-dynamo/dynamo/commit/830e58f6f0ac89e236a76d970c87b2ce95a51482 "refactor\(mm-routing\): generalize MM routing terminology \(#10290\)

Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>")[#10290](https://github.com/ai-dynamo/dynamo/pull/10290)[)](https://github.com/ai-dynamo/dynamo/commit/830e58f6f0ac89e236a76d970c87b2ce95a51482 "refactor\(mm-routing\): generalize MM routing terminology \(#10290\)

Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>")  | Jun 4, 2026  |  
|   |   |  [fix(ci): pass framework through shared-test pytest-local calls (](https://github.com/ai-dynamo/dynamo/commit/55fae8fcfb2af83c7d0131a913b526c0e98d8685 "fix\(ci\): pass framework through shared-test pytest-local calls \(#14688\)

Signed-off-by: Rini Gupta <rinig@nvidia.com>")[#14688](https://github.com/ai-dynamo/dynamo/pull/14688)[)](https://github.com/ai-dynamo/dynamo/commit/55fae8fcfb2af83c7d0131a913b526c0e98d8685 "fix\(ci\): pass framework through shared-test pytest-local calls \(#14688\)

Signed-off-by: Rini Gupta <rinig@nvidia.com>")  | Sep 11, 2026  |  
|   |   | [feat(skills): report-skillpack-issue - defect telemetry from running …](https://github.com/ai-dynamo/dynamo/commit/744f581589c60f6d5f5d91fe6b17fddf192da986 "feat\(skills\): report-skillpack-issue - defect telemetry from running agents \(#14073\)

Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")  | Sep 11, 2026  |  
|   |   | [feat(skills): report-skillpack-issue - defect telemetry from running …](https://github.com/ai-dynamo/dynamo/commit/744f581589c60f6d5f5d91fe6b17fddf192da986 "feat\(skills\): report-skillpack-issue - defect telemetry from running agents \(#14073\)

Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")  | Sep 11, 2026  |  
|   |   |  [fix(router): forward AIPerf trace hints (](https://github.com/ai-dynamo/dynamo/commit/d8dfcd161d3ee2ce360e2a7bfca22522fe5b281e "fix\(router\): forward AIPerf trace hints \(#14165\)

Signed-off-by: cpakkamisaac-sae <cpakkamisaac@nvidia.com>")[#14165](https://github.com/ai-dynamo/dynamo/pull/14165)[)](https://github.com/ai-dynamo/dynamo/commit/d8dfcd161d3ee2ce360e2a7bfca22522fe5b281e "fix\(router\): forward AIPerf trace hints \(#14165\)

Signed-off-by: cpakkamisaac-sae <cpakkamisaac@nvidia.com>")  | Sep 11, 2026  |  
|   |   |  [fix(lora): skip base-weight downloads during card registration (](https://github.com/ai-dynamo/dynamo/commit/f1683cc7268cfea6b101091de2857202c75e89bd "fix\(lora\): skip base-weight downloads during card registration \(#11164\)

Signed-off-by: AmeenP <ameenp360@gmail.com>")[#11164](https://github.com/ai-dynamo/dynamo/pull/11164)[)](https://github.com/ai-dynamo/dynamo/commit/f1683cc7268cfea6b101091de2857202c75e89bd "fix\(lora\): skip base-weight downloads during card registration \(#11164\)

Signed-off-by: AmeenP <ameenp360@gmail.com>")  | Sep 11, 2026  |  
|   |   | [fix(container): stop the vLLM sidecar failing to start on a missing v…](https://github.com/ai-dynamo/dynamo/commit/f2a30ed9dd931ec03766162328454ad69c9a7e63 "fix\(container\): stop the vLLM sidecar failing to start on a missing vllm-rs \(#14362\)

Signed-off-by: svc-glamr@nvidia.com <svc-glamr@nvidia.com>
Co-authored-by: yunzhoul-nv <232973175+yunzhoul-nv@users.noreply.github.com>")  | Sep 10, 2026  |  
|   |   |  [fix(helm): use valid KAI staleness duration (](https://github.com/ai-dynamo/dynamo/commit/5ef49b4cdcf5da829eddf1de07ba06d5f91c2433 "fix\(helm\): use valid KAI staleness duration \(#14722\)

Signed-off-by: Julien Mancuso <jmancuso@nvidia.com>")[#14722](https://github.com/ai-dynamo/dynamo/pull/14722)[)](https://github.com/ai-dynamo/dynamo/commit/5ef49b4cdcf5da829eddf1de07ba06d5f91c2433 "fix\(helm\): use valid KAI staleness duration \(#14722\)

Signed-off-by: Julien Mancuso <jmancuso@nvidia.com>")  | Sep 11, 2026  |  
|  [fix: shorten vLLM worker k8s service names for Grove 45-char limit (](https://github.com/ai-dynamo/dynamo/commit/6862e76cc91dc5aa65476f7489c0de0b6571fc32 "fix: shorten vLLM worker k8s service names for Grove 45-char limit \(#9848\)

Signed-off-by: Jont828 <jt572@cornell.edu>
Signed-off-by: Jonathan Tong <jt572@cornell.edu>")[#…](https://github.com/ai-dynamo/dynamo/pull/9848)  | Sep 10, 2026  |  
|  [fix(kv-router): make standalone indexer cleanup safe (](https://github.com/ai-dynamo/dynamo/commit/6c66d4b2a6345edda51f17f7c150862fee37385d "fix\(kv-router\): make standalone indexer cleanup safe \(#14652\)

Signed-off-by: PeaBrane <yanrpei@gmail.com>")[#14652](https://github.com/ai-dynamo/dynamo/pull/14652)[)](https://github.com/ai-dynamo/dynamo/commit/6c66d4b2a6345edda51f17f7c150862fee37385d "fix\(kv-router\): make standalone indexer cleanup safe \(#14652\)

Signed-off-by: PeaBrane <yanrpei@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [fix: shorten vLLM worker k8s service names for Grove 45-char limit (](https://github.com/ai-dynamo/dynamo/commit/6862e76cc91dc5aa65476f7489c0de0b6571fc32 "fix: shorten vLLM worker k8s service names for Grove 45-char limit \(#9848\)

Signed-off-by: Jont828 <jt572@cornell.edu>
Signed-off-by: Jonathan Tong <jt572@cornell.edu>")[#…](https://github.com/ai-dynamo/dynamo/pull/9848)  | Sep 10, 2026  |  
|  [fix(kv-router): make standalone indexer cleanup safe (](https://github.com/ai-dynamo/dynamo/commit/6c66d4b2a6345edda51f17f7c150862fee37385d "fix\(kv-router\): make standalone indexer cleanup safe \(#14652\)

Signed-off-by: PeaBrane <yanrpei@gmail.com>")[#14652](https://github.com/ai-dynamo/dynamo/pull/14652)[)](https://github.com/ai-dynamo/dynamo/commit/6c66d4b2a6345edda51f17f7c150862fee37385d "fix\(kv-router\): make standalone indexer cleanup safe \(#14652\)

Signed-off-by: PeaBrane <yanrpei@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat: port the networking kustomization to v1beta1 (](https://github.com/ai-dynamo/dynamo/commit/ac4599e089768f7fe28635e5b5cf2bc3606e3014 "feat: port the networking kustomization to v1beta1 \(#14142\)

Signed-off-by: yunzhoul-nv <232973175+yunzhoul-nv@users.noreply.github.com>
Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")[#14142](https://github.com/ai-dynamo/dynamo/pull/14142)[)](https://github.com/ai-dynamo/dynamo/commit/ac4599e089768f7fe28635e5b5cf2bc3606e3014 "feat: port the networking kustomization to v1beta1 \(#14142\)

Signed-off-by: yunzhoul-nv <232973175+yunzhoul-nv@users.noreply.github.com>
Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")  | Sep 11, 2026  |  
|   |   |  [feat: port the networking kustomization to v1beta1 (](https://github.com/ai-dynamo/dynamo/commit/ac4599e089768f7fe28635e5b5cf2bc3606e3014 "feat: port the networking kustomization to v1beta1 \(#14142\)

Signed-off-by: yunzhoul-nv <232973175+yunzhoul-nv@users.noreply.github.com>
Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")[#14142](https://github.com/ai-dynamo/dynamo/pull/14142)[)](https://github.com/ai-dynamo/dynamo/commit/ac4599e089768f7fe28635e5b5cf2bc3606e3014 "feat: port the networking kustomization to v1beta1 \(#14142\)

Signed-off-by: yunzhoul-nv <232973175+yunzhoul-nv@users.noreply.github.com>
Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")  | Sep 11, 2026  |  
|   |   |  [feat(llm): add FP16/BF16 variants to tensor protocol (](https://github.com/ai-dynamo/dynamo/commit/db46c147667cc5f487b900d311e62e491382809d "feat\(llm\): add FP16/BF16 variants to tensor protocol \(#14150\)

Signed-off-by: Vinya567 <vinyak@nvidia.com>")[#14150](https://github.com/ai-dynamo/dynamo/pull/14150)[)](https://github.com/ai-dynamo/dynamo/commit/db46c147667cc5f487b900d311e62e491382809d "feat\(llm\): add FP16/BF16 variants to tensor protocol \(#14150\)

Signed-off-by: Vinya567 <vinyak@nvidia.com>")  | Sep 11, 2026  |  
|   |   |  [ci: Add Copyright Verification Scripts w/ Automation (](https://github.com/ai-dynamo/dynamo/commit/c9130f8f8ce264379131e9ee2973534fe4cbf713 "ci: Add Copyright Verification Scripts w/ Automation \(#110\)")[#110](https://github.com/ai-dynamo/dynamo/pull/110)[)](https://github.com/ai-dynamo/dynamo/commit/c9130f8f8ce264379131e9ee2973534fe4cbf713 "ci: Add Copyright Verification Scripts w/ Automation \(#110\)")  | Feb 5, 2025  |  
|   |   |  [chore: keep CodeRabbit rule ownership in Python guidelines (](https://github.com/ai-dynamo/dynamo/commit/7aa7bcb89449c740d067972ea404e7127f4bba4b "chore: keep CodeRabbit rule ownership in Python guidelines \(#10307\)

Signed-off-by: Alec Flowers <aflowers@nvidia.com>")[#10307](https://github.com/ai-dynamo/dynamo/pull/10307)[)](https://github.com/ai-dynamo/dynamo/commit/7aa7bcb89449c740d067972ea404e7127f4bba4b "chore: keep CodeRabbit rule ownership in Python guidelines \(#10307\)

Signed-off-by: Alec Flowers <aflowers@nvidia.com>")  | Jun 4, 2026  |  
|   |   |  [feat: enable test coverage report for Python (](https://github.com/ai-dynamo/dynamo/commit/308bf0ccf6e48306bb629d76d147b89b734cccd5 "feat: enable test coverage report for Python  \(#5460\)

Signed-off-by: Jie Hao <jihao@nvidia.com>
Co-authored-by: Dmitry Tokarev <dtokarev@nvidia.com>")[#5460](https://github.com/ai-dynamo/dynamo/pull/5460)[)](https://github.com/ai-dynamo/dynamo/commit/308bf0ccf6e48306bb629d76d147b89b734cccd5 "feat: enable test coverage report for Python  \(#5460\)

Signed-off-by: Jie Hao <jihao@nvidia.com>
Co-authored-by: Dmitry Tokarev <dtokarev@nvidia.com>")  | Apr 24, 2026  |  
|   |   |  [refactor(aisimulate): consume standalone releases (](https://github.com/ai-dynamo/dynamo/commit/825ae4233743806cf437f36de78a67b426ee5dfa "refactor\(aisimulate\): consume standalone releases \(#13478\)

Signed-off-by: hongkuanz <hongkuanz@nvidia.com>")[#13478](https://github.com/ai-dynamo/dynamo/pull/13478)[)](https://github.com/ai-dynamo/dynamo/commit/825ae4233743806cf437f36de78a67b426ee5dfa "refactor\(aisimulate\): consume standalone releases \(#13478\)

Signed-off-by: hongkuanz <hongkuanz@nvidia.com>")  | Aug 19, 2026  |  
|   |   |  [docs(blog): Dynamo Agent Optimization Skills digest post (](https://github.com/ai-dynamo/dynamo/commit/b3358bd7207d84103dcaef9cfcf72eb0a1bdc41d "docs\(blog\): Dynamo Agent Optimization Skills digest post \(#13608\)

Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")[#13608](https://github.com/ai-dynamo/dynamo/pull/13608)[)](https://github.com/ai-dynamo/dynamo/commit/b3358bd7207d84103dcaef9cfcf72eb0a1bdc41d "docs\(blog\): Dynamo Agent Optimization Skills digest post \(#13608\)

Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")  | Aug 21, 2026  |  
|   |   |  [chore: Deprecate Go EPP and default to Rust EPP (](https://github.com/ai-dynamo/dynamo/commit/7599608ac30d7d45e2f9a05d93063ad8d9be2000 "chore: Deprecate Go EPP and default to Rust EPP \(#11355\)

Signed-off-by: Anna Tchernych <atchernych@nvidia.com>
Co-authored-by: Claude Opus 4.8 \(1M context\) <noreply@anthropic.com>")[#11355](https://github.com/ai-dynamo/dynamo/pull/11355)[)](https://github.com/ai-dynamo/dynamo/commit/7599608ac30d7d45e2f9a05d93063ad8d9be2000 "chore: Deprecate Go EPP and default to Rust EPP \(#11355\)

Signed-off-by: Anna Tchernych <atchernych@nvidia.com>
Co-authored-by: Claude Opus 4.8 \(1M context\) <noreply@anthropic.com>")  | Sep 1, 2026  |  
|   |   | [feat(recipes): add DeepSeek-V4-Pro-0813 vLLM recipes and Fern docs (H…](https://github.com/ai-dynamo/dynamo/commit/cf593037e6d478830e321a73c6aefad024f01a3b "feat\(recipes\): add DeepSeek-V4-Pro-0813 vLLM recipes and Fern docs \(H200 + GB200\) \(#13783\)

Signed-off-by: Cheng Wang <chengwa@nvidia.com>
Signed-off-by: Shubham <shugandhi@nvidia.com>
Signed-off-by: shugandhi <shugandhi@nvidia.com>
Co-authored-by: shugandhi <shugandhi@nvidia.com>
Co-authored-by: Claude Opus 5 \(1M context\) <noreply@anthropic.com>")  | Sep 4, 2026  |  
| [.pre-commit-config.yaml](https://github.com/ai-dynamo/dynamo/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/ai-dynamo/dynamo/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |  [docs: Add the recipe templates for recipe creation starting points (](https://github.com/ai-dynamo/dynamo/commit/638c82fba36a1ea11a3021c7ae34d64552cdda30 "docs: Add the recipe templates for recipe creation starting points \(#13724\)

Signed-off-by: yunzhoul-nv <232973175+yunzhoul-nv@users.noreply.github.com>")[#…](https://github.com/ai-dynamo/dynamo/pull/13724)  | Sep 2, 2026  |  
|   |   | [feat(skills): report-skillpack-issue - defect telemetry from running …](https://github.com/ai-dynamo/dynamo/commit/744f581589c60f6d5f5d91fe6b17fddf192da986 "feat\(skills\): report-skillpack-issue - defect telemetry from running agents \(#14073\)

Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")  | Sep 11, 2026  |  
|   |   |  [refactor(router): remove generic policy admission framework (](https://github.com/ai-dynamo/dynamo/commit/9cbb1e1812c8621bdebe5ea007c59dba15c687be "refactor\(router\): remove generic policy admission framework \(#12098\)

Signed-off-by: Ishan Dhanani <ishandhanani@gmail.com>")[#12098](https://github.com/ai-dynamo/dynamo/pull/12098)[)](https://github.com/ai-dynamo/dynamo/commit/9cbb1e1812c8621bdebe5ea007c59dba15c687be "refactor\(router\): remove generic policy admission framework \(#12098\)

Signed-off-by: Ishan Dhanani <ishandhanani@gmail.com>")  | Jul 30, 2026  |  
|   |   |  [refactor(operator): remove legacy Kustomize deployment path (](https://github.com/ai-dynamo/dynamo/commit/a3ab180a93ec63d6a2e1fbce477512d2c34ab00f "refactor\(operator\): remove legacy Kustomize deployment path \(#14252\)

Signed-off-by: carlory <baofa.fan@daocloud.io>")[#14252](https://github.com/ai-dynamo/dynamo/pull/14252)[)](https://github.com/ai-dynamo/dynamo/commit/a3ab180a93ec63d6a2e1fbce477512d2c34ab00f "refactor\(operator\): remove legacy Kustomize deployment path \(#14252\)

Signed-off-by: carlory <baofa.fan@daocloud.io>")  | Sep 10, 2026  |  
|   |   |  [feat: Sets the code of conduct for the repository (](https://github.com/ai-dynamo/dynamo/commit/00b6266db47a78678717ec8ebac8fdaa1d75e792 "feat: Sets the code of conduct for the repository \(#454\)")[#454](https://github.com/ai-dynamo/dynamo/pull/454)[)](https://github.com/ai-dynamo/dynamo/commit/00b6266db47a78678717ec8ebac8fdaa1d75e792 "feat: Sets the code of conduct for the repository \(#454\)")  | Apr 2, 2025  |  
|   |   |  [fix(codeowners): harden policy gate and document contributor workflow (](https://github.com/ai-dynamo/dynamo/commit/57a03b055331912d925ef67722afe3d199993496 "fix\(codeowners\): harden policy gate and document contributor workflow \(#11869\)

Judge ownership coverage across the whole tree on policy changes, block
globs that match no tracked file, and enforce that shared entries stay
additive so a more specific rule cannot silently strip a co-owner.

Document the contributor workflow for editing areas.yaml and regenerating
CODEOWNERS.

Signed-off-by: Daniel Gil <dagil@nvidia.com>")[…](https://github.com/ai-dynamo/dynamo/pull/11869)  | Sep 1, 2026  |  
|   |   | [fix(codeowners): carry planner ownership through the team, not per-pe…](https://github.com/ai-dynamo/dynamo/commit/793dba28d6bd0ccd17197a4b72346732132e5102 "fix\(codeowners\): carry planner ownership through the team, not per-person rows \(#12939\)

Signed-off-by: Dan Gil <dagil@nvidia.com>")  | Aug 10, 2026  |  
|   |   | [fix(backend-common): track worker readiness on the runtime health rou…](https://github.com/ai-dynamo/dynamo/commit/f3389106784f1e51d8365aa88211f85d733b0be0 "fix\(backend-common\): track worker readiness on the runtime health route \(#14458\)

Signed-off-by: svc-glamr@nvidia.com <svc-glamr@nvidia.com>
Signed-off-by: tanmayv25 <tanmay2592@gmail.com>
Co-authored-by: tanmayv25 <tanmay2592@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat(metrics): export metrics over OTLP from a typed engine contract (](https://github.com/ai-dynamo/dynamo/commit/757b944c81053c3e18d5dccffb38749af5a86f17 "feat\(metrics\): export metrics over OTLP from a typed engine contract \(#14071\)

Signed-off-by: Neelay Shah <neelays@nvidia.com>
Signed-off-by: nnshah1 <neelays@nvidia.com>
Co-authored-by: Saravana Periyasamy <saperiyasamy@nvidia.com>")[#…](https://github.com/ai-dynamo/dynamo/pull/14071)  | Sep 10, 2026  |  
|   |   |  [ci: Create DCO.md to help contributors (](https://github.com/ai-dynamo/dynamo/commit/088295e00750c2807a576603680cf143d2acf5af "ci: Create DCO.md to help contributors  \(#3171\)

Signed-off-by: Meenakshi Sharma <163925564+nvda-mesharma@users.noreply.github.com>
Co-authored-by: Ryan McCormick <rmccormick@nvidia.com>")[#3171](https://github.com/ai-dynamo/dynamo/pull/3171)[)](https://github.com/ai-dynamo/dynamo/commit/088295e00750c2807a576603680cf143d2acf5af "ci: Create DCO.md to help contributors  \(#3171\)

Signed-off-by: Meenakshi Sharma <163925564+nvda-mesharma@users.noreply.github.com>
Co-authored-by: Ryan McCormick <rmccormick@nvidia.com>")  | Sep 24, 2025  |  
|   |   |  [feat: DeepSeek V3.2 chat template support (](https://github.com/ai-dynamo/dynamo/commit/1efc7d63ddf1acc27ec511ef386d512080fb75c4 "feat: DeepSeek V3.2 chat template support \(#4797\)

Signed-off-by: Vladislav Nosivskoy <vladnosiv@gmail.com>
Co-authored-by: Ryan McCormick <rmccormick@nvidia.com>")[#4797](https://github.com/ai-dynamo/dynamo/pull/4797)[)](https://github.com/ai-dynamo/dynamo/commit/1efc7d63ddf1acc27ec511ef386d512080fb75c4 "feat: DeepSeek V3.2 chat template support \(#4797\)

Signed-off-by: Vladislav Nosivskoy <vladnosiv@gmail.com>
Co-authored-by: Ryan McCormick <rmccormick@nvidia.com>")  | Dec 12, 2025  |  
|   |   |   | Sep 11, 2026  |  
|   |   |  [docs(simulation): align docs with unified AISimulate CLI (](https://github.com/ai-dynamo/dynamo/commit/08cca119ea33427696f17e3517018bae058b6035 "docs\(simulation\): align docs with unified AISimulate CLI \(#14130\)

Signed-off-by: hongkuanz <hongkuanz@nvidia.com>")[#14130](https://github.com/ai-dynamo/dynamo/pull/14130)[)](https://github.com/ai-dynamo/dynamo/commit/08cca119ea33427696f17e3517018bae058b6035 "docs\(simulation\): align docs with unified AISimulate CLI \(#14130\)

Signed-off-by: hongkuanz <hongkuanz@nvidia.com>")  | Sep 1, 2026  |  
|   |   |  [chore: update all copyright headers in repo to 2026 (](https://github.com/ai-dynamo/dynamo/commit/cf433e6825d83f41905da47d69ca5ee30d4eb1ba "chore: update all copyright headers in repo to 2026 \(#5130\)

Signed-off-by: Tushar Sharma <tusharma@nvidia.com>")[#5130](https://github.com/ai-dynamo/dynamo/pull/5130)[)](https://github.com/ai-dynamo/dynamo/commit/cf433e6825d83f41905da47d69ca5ee30d4eb1ba "chore: update all copyright headers in repo to 2026 \(#5130\)

Signed-off-by: Tushar Sharma <tusharma@nvidia.com>")  | Jan 2, 2026  |  
|   |   |   | Mar 9, 2025  |  
|   |   |  [ci: auto-apply pre_merge and gpu_0 markers to unmarked tests (](https://github.com/ai-dynamo/dynamo/commit/ac0595cde806cf715fe444f01e4b9da04e2d8f3b "ci: auto-apply pre_merge and gpu_0 markers to unmarked tests \(#11208\)

Signed-off-by: elizabetht <email2eliza@gmail.com>
Signed-off-by: Elizabeth Thomas <email2eliza@gmail.com>
Co-authored-by: Saravana Periyasamy <saperiyasamy@nvidia.com>")[#11208](https://github.com/ai-dynamo/dynamo/pull/11208)[)](https://github.com/ai-dynamo/dynamo/commit/ac0595cde806cf715fe444f01e4b9da04e2d8f3b "ci: auto-apply pre_merge and gpu_0 markers to unmarked tests \(#11208\)

Signed-off-by: elizabetht <email2eliza@gmail.com>
Signed-off-by: Elizabeth Thomas <email2eliza@gmail.com>
Co-authored-by: Saravana Periyasamy <saperiyasamy@nvidia.com>")  | Aug 20, 2026  |  
|   |   |  [build(deps): Change parser for performance dependency (](https://github.com/ai-dynamo/dynamo/commit/16bcae69879e8f7c0eb2c7e7a93929643ab1e6f0 "build\(deps\): Change parser for performance dependency \(#12147\)

Signed-off-by: Harrison King Saturley-Hall <hsaturleyhal@nvidia.com>")[#12147](https://github.com/ai-dynamo/dynamo/pull/12147)[)](https://github.com/ai-dynamo/dynamo/commit/16bcae69879e8f7c0eb2c7e7a93929643ab1e6f0 "build\(deps\): Change parser for performance dependency \(#12147\)

Signed-off-by: Harrison King Saturley-Hall <hsaturleyhal@nvidia.com>")  | Jul 26, 2026  |  
|   |   |   | Mar 9, 2026  |  
|   |   |  [chore: update all copyright headers in repo to 2026 (](https://github.com/ai-dynamo/dynamo/commit/cf433e6825d83f41905da47d69ca5ee30d4eb1ba "chore: update all copyright headers in repo to 2026 \(#5130\)

Signed-off-by: Tushar Sharma <tusharma@nvidia.com>")[#5130](https://github.com/ai-dynamo/dynamo/pull/5130)[)](https://github.com/ai-dynamo/dynamo/commit/cf433e6825d83f41905da47d69ca5ee30d4eb1ba "chore: update all copyright headers in repo to 2026 \(#5130\)

Signed-off-by: Tushar Sharma <tusharma@nvidia.com>")  | Jan 2, 2026  |  
|   |   |  [chore(sglang): SGL version bump to 0.5.19 (](https://github.com/ai-dynamo/dynamo/commit/4a205cb356c9a1882ed8cda8980404026b52173a "chore\(sglang\): SGL version bump to 0.5.19 \(#14151\)

Signed-off-by: jain-ria <riajain@NVIDIA.com>")[#14151](https://github.com/ai-dynamo/dynamo/pull/14151)[)](https://github.com/ai-dynamo/dynamo/commit/4a205cb356c9a1882ed8cda8980404026b52173a "chore\(sglang\): SGL version bump to 0.5.19 \(#14151\)

Signed-off-by: jain-ria <riajain@NVIDIA.com>")  | Sep 9, 2026  |  
|   |   |  [chore: Upgrade Rust to latest stable, 1.96.1 (](https://github.com/ai-dynamo/dynamo/commit/7ffed017f97fd8341973fe7d0c7e2297feff8d40 "chore: Upgrade Rust to latest stable, 1.96.1 \(#11260\)

Signed-off-by: Graham King <grahamk@nvidia.com>")[#11260](https://github.com/ai-dynamo/dynamo/pull/11260)[)](https://github.com/ai-dynamo/dynamo/commit/7ffed017f97fd8341973fe7d0c7e2297feff8d40 "chore: Upgrade Rust to latest stable, 1.96.1 \(#11260\)

Signed-off-by: Graham King <grahamk@nvidia.com>")  | Jul 6, 2026  |  
|   |   |  [chore(skills): restore all skills to .agents/skills (](https://github.com/ai-dynamo/dynamo/commit/873188292725749ca9c2684ce316ce545bcee8a3 "chore\(skills\): restore all skills to .agents/skills \(#10755\)

Signed-off-by: Dan Gil <dagil@nvidia.com>")[#10755](https://github.com/ai-dynamo/dynamo/pull/10755)[)](https://github.com/ai-dynamo/dynamo/commit/873188292725749ca9c2684ce316ce545bcee8a3 "chore\(skills\): restore all skills to .agents/skills \(#10755\)

Signed-off-by: Dan Gil <dagil@nvidia.com>")  | Jun 16, 2026  |  
| View all files |  
## Repository files navigation
| | | | | | | | **[How to Contribute](https://github.com/ai-dynamo/dynamo#community-and-contributing)** | |
# Dynamo
**The open-source, datacenter-scale inference stack.** Dynamo is the orchestration layer above inference engines — it doesn't replace SGLang, TensorRT-LLM, or vLLM, it turns them into a coordinated multi-node inference system. Disaggregated serving, intelligent routing, multi-tier KV caching, and automatic scaling work together to maximize throughput and minimize latency for LLM, reasoning, multimodal, and video generation workloads.
Built in Rust for performance, Python for extensibility.
## Community Events  
| Date  | Event  | Location  |  
| --- | --- | --- |  
| Thu, Sep 10, 2026  | ~~[Baseten x Dynamo x SGLang RL post training meetup](https://calendar.google.com/calendar/render?action=TEMPLATE&text=Baseten+x+Dynamo+x+SGLang+RL+post+training+meetup&dates=20260911T010000Z%2F20260911T040000Z&location=https%3A%2F%2Fluma.com%2FBaseDynSGL)~~  |  
| Mon, Aug 24, 2026  |   |  
> Events are updated automatically. Subscribe to our [public calendar](https://calendar.google.com/calendar/embed?src=c_c2448d2efb09eac2ddee1f34524124135bd3f4554868769059105e18e1b97e8f%40group.calendar.google.com).
## When to use Dynamo
  * You're serving LLMs across **multiple GPUs or nodes** and need to coordinate them
  * You want **KV-aware routing** to avoid redundant prefill computation
  * You need to **independently scale prefill and decode** (disaggregated serving)
  * You want **automatic scaling** that meets latency SLAs at minimum total cost of ownership (TCO)
  * You need **fast cold-starts** when spinning up new replicas


If you're running a single model on a single GPU, your inference engine alone is probably sufficient.
**Feature support at a glance:**  
 |  
|  |  
| ✅  | ✅  | ✅  |  
| ✅  | ✅  | ✅  |  
| ✅  | ✅  | ✅  |  
| 🚧  | ✅  | ✅  |  
| ✅  | ✅  | ✅  |  
| ✅  | ✅  | ✅  |  
> **[Full Feature Matrix →](https://docs.nvidia.com/dynamo/resources/feature-matrix)** — LoRA, request migration, speculative decoding, and feature interactions.
## Key Results  
| Result  | Context  |  
| --- | --- |  
|  **7x** higher throughput per GPU  | DeepSeek R1 on GB200 NVL72 w/ Dynamo vs B200 without ([InferenceX](https://inferencex.semianalysis.com/))  |  
|  **7x** faster model startup  | ModelExpress weight streaming (DeepSeek-V3 on H200)  |  
|  **2x** faster time to first token  | KV-aware routing, Qwen3-Coder 480B ([Baseten benchmark](https://www.baseten.co/blog/how-baseten-achieved-2x-faster-inference-with-nvidia-dynamo/))  |  
|  **80%** fewer SLA breaches  | Planner autoscaling at 5% lower TCO ([Alibaba APSARA 2025 @ 2:50:00](https://yunqi.aliyun.com/2025/session?agendaId=6062))  |  
|  **750x** higher throughput  | DeepSeek-R1 on GB300 NVL72 ([InferenceXv2](https://inferencex.semianalysis.com/))  |  
## What Dynamo Does
Most inference engines optimize a single GPU or a single node. Dynamo is the **orchestration layer above them** — it turns a cluster of GPUs into a coordinated inference system.
**[Architecture Deep Dive →](https://docs.nvidia.com/dynamo/design-docs/overall-architecture)**
### Core Capabilities  
| Capability  | What it does  | Why it matters  |  
| --- | --- | --- |  
| [**Disaggregated Prefill/Decode**](https://docs.nvidia.com/dynamo/dev/knowledge-base/concepts/system-architecture/disaggregated-serving)  | Separates prefill and decode into independently scalable GPU pools  | Maximizes GPU utilization; each phase runs on hardware tuned for its workload  |  
| Routes requests based on worker load and KV cache overlap  | Eliminates redundant prefill computation — 2x faster TTFT  |  
| [**KV Block Manager (KVBM)**](https://docs.nvidia.com/dynamo/components/kvbm)  | Offloads KV cache across GPU → CPU → SSD → remote storage  | Extends effective context length beyond GPU memory  |  
| Streams model weights GPU-to-GPU via NIXL/NVLink  | 7x faster cold-start for new replicas  |  
| SLA-driven autoscaler that profiles workloads and right-sizes pools  | Meets latency targets at minimum total cost of ownership (TCO)  |  
| K8s operator for topology-aware gang scheduling (NVL72)  | Places workloads optimally across racks, hosts, and NUMA nodes  |  
| Predicts serving behavior and searches deployment configurations offline  | Finds a strong serving configuration without bringing up a GPU cluster  |  
| Canary health checks + in-flight request migration  | Workers fail; user requests don't  |  
### New in 1.0
  * **Zero-config deploy ([DGDR](https://docs.nvidia.com/dynamo/kubernetes-deployment/deploy-models/dgdr-reference))** _(beta):_ Specify model, HW, and SLA in one YAML — AISimulate performance models estimate candidate configurations, Planner optimizes the topology, and Dynamo deploys
  * **Agentic inference:** Per-request hints for priority, expected output length, and speculative prefill, plus session metadata for tracing and SGLang subagent KV isolation. [LangChain](https://docs.langchain.com/oss/python/integrations/chat/nvidia_ai_endpoints#use-with-nvidia-dynamo) + [NeMo Agent Toolkit](https://github.com/NVIDIA/NeMo-Agent-Toolkit) integrations
  * **Multimodal E/P/D:** Disaggregated encode/prefill/decode with embedding cache — 30% faster TTFT on image workloads
  * **Video generation:** Native [FastVideo](https://github.com/hao-ai-lab/FastVideo) + [SGLang Diffusion](https://lmsys.org/blog/2026-02-16-sglang-diffusion-advanced-optimizations/) support — real-time 1080p on single B200
  * **K8s Inference Gateway plugin:** KV-aware routing inside the standard Kubernetes gateway
  * **Storage-tier KV offload:** S3/Azure blob support + global KV events for cluster-wide cache visibility


## Request Routing Topologies
Dynamo can expose traffic through two Kubernetes request routing topologies. Both expose an OpenAI-compatible API and support the same backends, disaggregated serving, and KV-aware routing.  
| Topology  | What it is  | When to use  |  
| --- | --- | --- |  
| **Dynamo-native Frontend routing**  | The Dynamo Frontend serves HTTP and the integrated Dynamo Router makes worker-selection decisions. No external gateway is required.  | Local development, single-cluster deployments, and environments where Dynamo should own the request entry point end to end.  |  
| **Gateway API routing with GAIE**  | A Kubernetes [Gateway API Inference Extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) gateway calls the Dynamo Endpoint Picker Plugin (EPP) before forwarding to the selected worker's Frontend sidecar in `--router-mode direct`.  | Kubernetes platforms that standardize on Gateway API, or deployments where gateway-level policy, auth, rate limiting, and observability should sit at the cluster edge.  |  
Request flow for the Dynamo-native path is `client → Frontend → Router → workers`. Request flow for the Gateway API path is `client → Gateway → EPP → Frontend sidecar (direct) → workers`.
See the [Gateway API Inference Extension (GAIE) guide](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/kubernetes/kv-aware-routing/gateway-api.mdx) for the Gateway API setup, supported features, and configuration.
## Quick Start
> This repo ships agent skills: if you work with an AI coding agent (Claude Code, Codex, Cursor), clone the repo and ask it to deploy, troubleshoot, benchmark, or optimize a Dynamo deployment. The skills activate automatically; no setup required.
### Option A: Container (fastest)

```
# Pull a prebuilt container (SGLang example)
docker run --gpus all --network host --rm -it nvcr.io/nvidia/ai-dynamo/sglang-runtime:1.4.2

# Inside the container — start frontend and worker
python3 -m dynamo.frontend --http-port 8000 --discovery-backend file  /dev/null 2>&1 
python3 -m dynamo.sglang --model-path Qwen/Qwen3-0.6B --discovery-backend file 

# Send a request
curl -s localhost:8000/v1/chat/completions -H "Content-Type: application/json" -d 
  "model": "Qwen/Qwen3-0.6B",
  "messages": [{"role": "user", "content": "Hello!"}],
  "max_tokens": 100
 | jq
```

Also available: [`tensorrtllm-runtime:1.4.2`](https://docs.nvidia.com/dynamo/resources/release-artifacts) and [`vllm-runtime:1.4.2`](https://docs.nvidia.com/dynamo/resources/release-artifacts).
### Option B: Install from PyPI
Install [uv](https://github.com/astral-sh/uv) (`curl -LsSf https://astral.sh/uv/install.sh | sh`), then:

```
uv pip install --prerelease=allow "ai-dynamo[sglang]"   # or [vllm]
```

> **Note:** TensorRT-LLM requires `pip` with `--extra-index-url https://pypi.nvidia.com`. See the [install guide](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/cli/installation/install-dynamo.mdx) for TRT-LLM-specific instructions.
Then start the frontend and a worker as shown above. See the [full installation guide](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/cli/installation/install-dynamo.mdx) for system dependencies and backend-specific notes.
### Option C: Kubernetes (recommended)
For production multi-node clusters, install the [Dynamo Platform](https://docs.nvidia.com/dynamo/kubernetes-deployment/start-here/installation-guide) and deploy with a single manifest:

```
# Zero-config deploy: specify model + SLA, Dynamo handles the rest
apiVersion: nvidia.com/v1beta1
kind: DynamoGraphDeploymentRequest
metadata:
  name: my-model
spec:
  model: Qwen/Qwen3-0.6B
  backend: vllm
  sla:
    ttft: 200.0   
    itl: 20.0     
  autoApply: true
```

Pre-built recipes for common models:  
| Model  | Framework  | Mode  | Recipe  |  
| --- | --- | --- | --- |  
| Qwen3-32B-FP8  | TensorRT-LLM  | Aggregated  |  
| DeepSeek-R1  | SGLang  | Disaggregated  |  
| Kimi-K3  | vLLM  | Aggregated  |  
See [recipes/](https://github.com/ai-dynamo/dynamo/blob/main/recipes/README.md) for the full list. Cloud-specific guides: [AWS EKS](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/kubernetes/installation/managed-kubernetes/eks/eks-setup.mdx) · [Google GKE](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/kubernetes/installation/managed-kubernetes/gcp/gke-setup.mdx) · [Azure AKS](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/kubernetes/installation/managed-kubernetes/azure/aks-setup.mdx) · [Amazon ECS](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/kubernetes/installation/managed-kubernetes/eks/ecs.mdx)
## Building from Source
For contributors who want to build and develop locally. See the [full build guide](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/developer-guide/advanced-customizations/building-from-source.md) for details.

```
# Install system deps (Ubuntu 24.04)
sudo apt install -y build-essential libhwloc-dev libudev-dev pkg-config libclang-dev protobuf-compiler python3-dev cmake

# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh  source $HOME/.cargo/env

# Create venv and build
uv venv dynamo  source dynamo/bin/activate
uv pip install pip 'maturin[patchelf]'
cd lib/bindings/python  maturin develop --uv  cd $PROJECT_ROOT
uv pip install -e lib/gpu_memory_service
uv pip install -e .
```

> VSCode/Cursor users: see the [`.devcontainer`](https://github.com/ai-dynamo/dynamo/blob/main/.devcontainer/README.md) for a pre-configured dev environment.
## Community and Contributing
Dynamo is built in the open with an OSS-first development model. We welcome contributions of all kinds.
  * — How to contribute code, docs, and recipes
  * — RFCs for major features, tracked as `dep:*` labeled GitHub issues
  * — Biweekly calls
  * ([Youtube](https://www.youtube.com/@ai-dynamo-community)) – Weekly (Wed 10:30 AM PT) development community meetings
  * — Chat with the team and community
  * **[Dynamo Day Recordings](https://nvevents.nvidia.com/dynamoday)** — Deep dives from production users


## Latest News
  * [03/15] [Dynamo 1.0 is here — production-ready with strong community adoption](https://developer.nvidia.com/blog/introducing-nvidia-dynamo-a-low-latency-distributed-inference-framework-for-scaling-reasoning-ai-models/)
  * [03/15] [NVIDIA Blackwell Ultra sets new inference records in MLPerf](https://developer.nvidia.com/blog/nvidia-blackwell-ultra-sets-new-inference-records-in-mlperf-debut/)
  * [03/15] [NVIDIA Blackwell leads on SemiAnalysis InferenceMax benchmarks](https://developer.nvidia.com/blog/nvidia-blackwell-leads-on-new-semianalysis-inferencemax-benchmarks/)
  * [12/05] [Moonshot AI's Kimi K2 achieves 10x inference speedup with Dynamo on GB200](https://quantumzeitgeist.com/kimi-k2-nvidia-ai-ai-breakthrough/)
  * [12/02] [Mistral AI runs Mistral Large 3 with 10x faster inference using Dynamo](https://www.marktechpost.com/2025/12/02/nvidia-and-mistral-ai-bring-10x-faster-inference-for-the-mistral-3-family-on-gb200-nvl72-gpu-systems/)
  * [11/20] [Dell integrates PowerScale with NIXL for 19x faster TTFT](https://www.dell.com/en-us/dt/corporate/newsroom/announcements/detailpage.press-releases~usa~2025~11~dell-technologies-and-nvidia-advance-enterprise-ai-innovation.htm)

Older news
Dynamo provides comprehensive benchmarking tools:
  * – Compare deployment topologies using AIPerf
  * – Optimize deployments to meet SLA requirements


## Frontend OpenAPI Specification
The OpenAI-compatible frontend exposes an OpenAPI 3 spec at `/openapi.json`. To generate without running the server:

```
cargo run -p dynamo-llm --bin generate-frontend-openapi
```

This writes to `docs/reference/api/openapi.json`.
## Service Discovery and Messaging
Dynamo uses TCP for inter-component communication. On Kubernetes, native resources ([CRDs + EndpointSlices](https://github.com/ai-dynamo/dynamo/blob/main/docs/fern/pages/developer-guide/knowledge-base/concepts/system-architecture/architecture.md#discovery-plane)) handle service discovery. External services are optional for most deployments:  
| Deployment  | etcd  | NATS  | Notes  |  
| --- | --- | --- | --- |  
| **Local Development**  | ❌ Not required  | ❌ Not required  | Pass `--discovery-backend file`; vLLM also needs `--kv-events-config '{"enable_kv_cache_events": false}'`  |  
| **Kubernetes**  | ❌ Not required  | ❌ Not required  | K8s-native discovery; TCP request plane  |  
> **Note:** KV-aware routing does not require NATS. Enable KV events when you need event-backed cache-state tracking, or use `--no-router-kv-events` for prediction-based routing without external event infrastructure.
For Slurm or other distributed deployments that choose etcd or NATS JetStream-backed modes:
  * [etcd](https://etcd.io/) can be run directly as `./etcd`.
  * [nats](https://nats.io/) needs JetStream enabled: `nats-server -js`.


To quickly setup both: `docker compose -f dev/docker-compose.yml up -d`
## More News
  * [11/20] [Dell integrates PowerScale with Dynamo's NIXL for 19x faster TTFT](https://www.dell.com/en-us/dt/corporate/newsroom/announcements/detailpage.press-releases~usa~2025~11~dell-technologies-and-nvidia-advance-enterprise-ai-innovation.htm)
  * [11/20] [WEKA partners with NVIDIA on KV cache storage for Dynamo](https://siliconangle.com/2025/11/20/nvidia-weka-kv-cache-solution-ai-inferencing-sc25/)
  * [11/13] [Dynamo Office Hours Playlist](https://www.youtube.com/playlist?list=PL5B692fm6--tgryKu94h2Zb7jTFM3Go4X)
  * [10/16] [How Baseten achieved 2x faster inference with NVIDIA Dynamo](https://www.baseten.co/blog/how-baseten-achieved-2x-faster-inference-with-nvidia-dynamo/)
  * [12/01] [InfoQ: NVIDIA Dynamo simplifies Kubernetes deployment for LLM inference](https://www.infoq.com/news/2025/12/nvidia-dynamo-kubernetes/)


## Reference
  * — Hardware, OS, CUDA, and backend versions
  * — Detailed backend compatibility
  * — Containers, wheels, Helm charts
  * — K8s-native vs etcd vs file-based discovery
  * — Compare deployment topologies with AIPerf


## About
A Datacenter Scale Distributed Inference Serving Framework
[docs.nvidia.com/dynamo/latest](https://docs.nvidia.com/dynamo/latest)
### Topics
[diffusion](https://github.com/topics/diffusion)[disaggregated-serving](https://github.com/topics/disaggregated-serving)[kubernetes](https://github.com/topics/kubernetes)[llm-inference](https://github.com/topics/llm-inference)[omni](https://github.com/topics/omni)[routing-engine](https://github.com/topics/routing-engine)[rust](https://github.com/topics/rust)[sglang](https://github.com/topics/sglang)[tensorrt-llm](https://github.com/topics/tensorrt-llm)[vllm](https://github.com/topics/vllm)
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**8.0k** stars
### Watchers
**76** watching
### Forks
[**1.6k** forks](https://github.com/ai-dynamo/dynamo/forks)
## 
[ Dynamo v1.4.2Latest Aug 29, 2026 ](https://github.com/ai-dynamo/dynamo/releases/tag/v1.4.2)
## 
## Languages
  * [Rust52.7%](https://github.com/ai-dynamo/dynamo/search?l=rust)
  * [Python34.6%](https://github.com/ai-dynamo/dynamo/search?l=python)
  * [Go10.5%](https://github.com/ai-dynamo/dynamo/search?l=go)
  * [Shell0.8%](https://github.com/ai-dynamo/dynamo/search?l=shell)
  * [Dockerfile0.6%](https://github.com/ai-dynamo/dynamo/search?l=dockerfile)
  * [HTML0.2%](https://github.com/ai-dynamo/dynamo/search?l=html)
  * [Other0.6%](https://github.com/ai-dynamo/dynamo/search?l=Other)


You can’t perform that action at this time. 
