---
title: GitHub - llm-d-incubation/llm-d-rl · GitHub
id: github-llm-d-incubationllm-d-rl-github
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- rl-integration
- epp
- stale-path
created: '2026-09-11T19:43:19.948577Z'
updated: '2026-09-15T04:55:23.681830Z'
source: https://github.com/llm-d-incubation/llm-d-rl
source_domain: github.com
fetched_at: '2026-09-11T19:43:19.922518Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub README for llm-d-incubation/llm-d-rl (fetched Sep 11 2026; 3 stars,
  245 commits, most recent PR #47 merged Aug 31 2026). Confirms this repo integrates
  llm-d''s Endpoint Picker Plugin (EPP) into RL rollout frameworks (veRL, vime, slime)
  as a drop-in replacement for round-robin load balancing, scoring replicas on prefix-cache
  hit rate, queue depth, and KV utilization. Documents two integration modes, both
  requiring no framework source changes: (1) ''EPP as the endpoint picker'' -- the
  framework calls EPP directly over gRPC to score replicas then dispatches itself
  (fewer moving parts, lower latency, recommended starting point); (2) ''llm-d serving''
  -- the framework sends all generation to a single Envoy endpoint that consults EPP
  and forwards (closest to production llm-d deployment). Both modes support prefill/decode
  (PD) disaggregation. CRITICAL STRUCTURAL FINDING: the repo was reorganized (commit
  ''Rename experimental/ to legacy/'', Aug 19 2026) -- the original ''experimental/rl-controller''
  directory, which held a framework-agnostic Go/Python control plane with no Kubernetes
  dependency (per its initial commit: ''weight sync coordinator, engine pool manager,
  rollout controller HTTP server''), is now at legacy/rl-controller/ and is explicitly
  marked as having ''had no active development or users for several months while work
  moved to the EPP-based routing... it is not the recommended starting point for new
  work.'' This means the URL path given in the task brief (experimental/rl-controller/docs/proposals/rl-rollout-northstar.md)
  is STALE; the correct current path is legacy/rl-controller/docs/proposals/rl-rollout-northstar.md
  (fetched separately, see llm-d-rllegacyrl-controllerdocsproposalsrl-rollout-northstarmd-at-main-llm-d-inc).'
utility_score: 13.0
---

[Skip to content](https://github.com/llm-d-incubation/llm-d-rl#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d-incubation/llm-d-rl) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d-incubation/llm-d-rl) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d-incubation/llm-d-rl) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d-incubation/llm-d-rl).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d-incubation%2Fllm-d-rl) You must be signed in to change notification settings
  * [ Fork 4 ](https://github.com/login?return_to=%2Fllm-d-incubation%2Fllm-d-rl)
  * [ Star  3 ](https://github.com/login?return_to=%2Fllm-d-incubation%2Fllm-d-rl)


[**1** Branch](https://github.com/llm-d-incubation/llm-d-rl/branches)[](https://github.com/llm-d-incubation/llm-d-rl/tags)
Go to file
Open more actions menu
## Latest commit
[Merge pull request](https://github.com/llm-d-incubation/llm-d-rl/commit/4df1ec1dd58fe0af92f3147dc1bdc33529d02c0e) [#47](https://github.com/llm-d-incubation/llm-d-rl/pull/47) [from ezrasilvera/gv41-kermit-epp-fixes](https://github.com/llm-d-incubation/llm-d-rl/commit/4df1ec1dd58fe0af92f3147dc1bdc33529d02c0e)
Open commit details
Aug 31, 2026
[4df1ec1](https://github.com/llm-d-incubation/llm-d-rl/commit/4df1ec1dd58fe0af92f3147dc1bdc33529d02c0e) · Aug 31, 2026
## History
[245 Commits](https://github.com/llm-d-incubation/llm-d-rl/commits/main/)
Open commit details
245 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [Add vllm018 verl engine, EPP cache_salt, and reqlog reliability fix](https://github.com/llm-d-incubation/llm-d-rl/commit/8825b18020f57ad9989aee57929915979c0ee792 "Add vllm018 verl engine, EPP cache_salt, and reqlog reliability fix

Needed for GV4.1 \(Granite-Vision\) GRPO training on Kermit:

- integrations/verl/environments.env, quickstart/kuberay/frameworks.env:
  add a vllm018 engine column \(verlai/verl:vllm018.dev1\) since GV4.1's
  fork patches are validated against vLLM 0.18.0, not the newer nightly
  the default vllm engine column ships.
- epp_grpc_client.py: add an optional cache_salt kwarg to pick\(\)/begin\(\)/
  route\(\), forwarded to EPP. EPP's prefix-hash only looks at text tokens,
  so a fixed-template multimodal prompt \(identical text, different image\)
  collapses distinct GRPO groups into one burst unit and can dump them
  onto a single replica. Passing a per-group salt \(e.g. sample_index\)
  restores correct group separation.
- reqlog.py: replace the atexit/threshold-flush design with a background
  writer thread per process. Both prior designs silently lost data
  because Ray tears down actor processes via os._exit\(\), which skips
  atexit; continuous writes bound the loss window to one record's disk
  latency instead of a batch size.

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>")  | Aug 31, 2026  |  
| [legacy/rl-controller](https://github.com/llm-d-incubation/llm-d-rl/tree/main/legacy/rl-controller "This path skips through empty directories")  | [legacy/rl-controller](https://github.com/llm-d-incubation/llm-d-rl/tree/main/legacy/rl-controller "This path skips through empty directories")  | [Rename experimental/ to legacy/ and refocus the top-level README](https://github.com/llm-d-incubation/llm-d-rl/commit/54f4c8ddeb32a7b9438a827a8bfbc7dd9805d4ed "Rename experimental/ to legacy/ and refocus the top-level README

The rl-controller exploration under experimental/ has had no active
development or users for several months while work moved to the
EPP-based routing in integrations/ and quickstart/. Rename the
directory to legacy/, mark it as such in its own README, and rewrite
the top-level README to describe only the active integrations/ and
quickstart/ work instead of presenting both as co-equal components.

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>")  | Aug 19, 2026  |  
|   |   | [Add vllm018 verl engine, EPP cache_salt, and reqlog reliability fix](https://github.com/llm-d-incubation/llm-d-rl/commit/8825b18020f57ad9989aee57929915979c0ee792 "Add vllm018 verl engine, EPP cache_salt, and reqlog reliability fix

Needed for GV4.1 \(Granite-Vision\) GRPO training on Kermit:

- integrations/verl/environments.env, quickstart/kuberay/frameworks.env:
  add a vllm018 engine column \(verlai/verl:vllm018.dev1\) since GV4.1's
  fork patches are validated against vLLM 0.18.0, not the newer nightly
  the default vllm engine column ships.
- epp_grpc_client.py: add an optional cache_salt kwarg to pick\(\)/begin\(\)/
  route\(\), forwarded to EPP. EPP's prefix-hash only looks at text tokens,
  so a fixed-template multimodal prompt \(identical text, different image\)
  collapses distinct GRPO groups into one burst unit and can dump them
  onto a single replica. Passing a per-group salt \(e.g. sample_index\)
  restores correct group separation.
- reqlog.py: replace the atexit/threshold-flush design with a background
  writer thread per process. Both prior designs silently lost data
  because Ray tears down actor processes via os._exit\(\), which skips
  atexit; continuous writes bound the loss window to one record's disk
  latency instead of a batch size.

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>")  | Aug 31, 2026  |  
|   |   | [Move deploy YAMLs and the registration shim to common, and update REA…](https://github.com/llm-d-incubation/llm-d-rl/commit/a4d805d7d94bb31d472fbc736c8ad587de8aad93 "Move deploy YAMLs and the registration shim to common, and update READMEs")  | Aug 17, 2026  |  
|   |   | [Initial llm-d-rl: RL rollout infrastructure for llm-d](https://github.com/llm-d-incubation/llm-d-rl/commit/baef6d67aa7eb4533b6cf856c27f919069d28dfa "Initial llm-d-rl: RL rollout infrastructure for llm-d

Add the foundation for a Kubernetes-native, framework-agnostic RL rollout
infrastructure that positions vLLM+llm-d as the neutral inference layer
beneath all RL post-training frameworks \(veRL, Slime, SkyRL, OpenRLHF,
NeMo-RL\).

Includes:
- North star document with competitive analysis of 5 RL frameworks
- Phased roadmap \(Foundation → Production → Adapters → Advanced\)
- API types \(v1alpha1\) for generation, weight sync, engine lifecycle
- Weight sync coordinator \(pause → NCCL/NIXL transfer → resume\)
- vLLM HTTP client implementing the EngineClient interface
- Engine pool manager with health checks and auto-recovery
- Rollout controller HTTP server exposing the RolloutControl API")  | Feb 27, 2026  |  
|   |   | [README: add architecture diagrams for the two integration modes](https://github.com/llm-d-incubation/llm-d-rl/commit/b52bf817e44d066147d6f82a3b7e2f9c7095d411 "README: add architecture diagrams for the two integration modes

Add a Mermaid flowchart for each integration mode \(EPP as the endpoint
picker, and llm-d serving\) showing the generate\(\) call path from a
generic RL framework through to the vLLM replica pool.

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>")  | Aug 19, 2026  |  
| View all files |  
## Repository files navigation
# llm-d-rl
Reinforcement-learning rollout infrastructure for the [llm-d](https://github.com/llm-d) inference serving stack.
RL post-training frameworks (veRL, OpenRLHF, SkyRL, NeMo-RL, Slime) each need to route generation requests to a pool of inference engines during training. This repo integrates llm-d's **Endpoint Picker Plugin (EPP)** into that pool as a drop-in replacement for round-robin replica selection. EPP scores each candidate vLLM replica on prefix-cache hit rate, queue depth, and KV utilization, and steers each request to the replica most likely to already have a warm cache. For large-group RL workloads (GRPO, PPO with large rollout groups), where many samples share a prompt prefix, this is a meaningful throughput win over spreading requests evenly.

```
llm-d-rl/
├── integrations/                 # one directory per framework
│   ├── verl/
│   ├── vime/
│   ├── slime/
│   └── common/                   # library and configs used by the three above
└── quickstart/                   # a full worked example: cluster + provisioning + benchmarks

```

## integrations/
How to integrate llm-d into an RL post-training framework. Supported today: [verl](https://github.com/llm-d-incubation/llm-d-rl/blob/main/integrations/verl) (the most complete - EPP routing, llm-d serving, PD and P2P KV-cache sharing), [vime](https://github.com/llm-d-incubation/llm-d-rl/blob/main/integrations/vime) (llm-d routing, vLLM engines), and [slime](https://github.com/llm-d-incubation/llm-d-rl/blob/main/integrations/slime) (llm-d routing, SGLang engines). Shared code and configs live in [`common/`](https://github.com/llm-d-incubation/llm-d-rl/blob/main/integrations/common).
There are two integration modes, named by mechanism (EPP is the Endpoint _Picker_ - it scores and selects a replica, it does not proxy):
  * **EPP as the endpoint picker** - the framework calls EPP directly over gRPC to score replicas, gets back the chosen replica, and dispatches to it itself. Fewer moving parts and lower latency; the place to start.
  * **llm-d serving** - the framework sends all generation to a single Envoy endpoint; Envoy calls EPP to pick the best replica and forwards the request. The framework only ever speaks HTTP to one address, closest to a production llm-d serving deployment.


Both modes require no framework source changes - they are wired in entirely through configuration - and both support prefill/decode (PD) disaggregation.
**EPP as the endpoint picker** - the framework asks EPP which replica to use, then dispatches the rollout there itself:
Loading

```
%%{init: {'flowchart': {'curve': 'linear'}}}%%
flowchart TD
    fw["RL rollouts"]
    epp["llm-d EPP"]
    fw -->|"1. ask which replica (gRPC)"| epp
    epp -->|"2. replica id"| fw
    fw -->|"3. dispatch generate() directly"| v0["vLLM Replica 0"]
    fw --> v1["vLLM Replica 1"]
    fw --> vN["vLLM Replica N"]

    classDef caller fill:#EEF2FF,stroke:#4F46E5,color:#1E1B4B
    classDef llmd fill:#EDE9FE,stroke:#7C3AED,color:#3B0764
    classDef replica fill:#F5F3FF,stroke:#8B5CF6,color:#3B0764
    class fw caller
    class epp llmd
    class v0,v1,vN replica

```

**llm-d serving** - the framework sends the rollout to a single Envoy endpoint, which consults EPP and forwards to the chosen replica:
Loading

```
%%{init: {'flowchart': {'curve': 'linear'}}}%%
flowchart TD
    fw["RL rollouts"]
    router["llm-d serving<br/>(Envoy)"]
    epp["llm-d EPP"]
    fw -->|"generate() request"| router
    router -->|"consults"| epp
    epp -->|"chosen replica"| router
    router -->|"forward"| v0["vLLM Replica 0"]
    router --> v1["vLLM Replica 1"]
    router --> vN["vLLM Replica N"]

    classDef caller fill:#EEF2FF,stroke:#4F46E5,color:#1E1B4B
    classDef llmd fill:#EDE9FE,stroke:#7C3AED,color:#3B0764
    classDef replica fill:#F5F3FF,stroke:#8B5CF6,color:#3B0764
    class fw caller
    class router,epp llmd
    class v0,v1,vN replica

```

See [`integrations/README.md`](https://github.com/llm-d-incubation/llm-d-rl/blob/main/integrations/README.md) and [`integrations/verl/README.md`](https://github.com/llm-d-incubation/llm-d-rl/blob/main/integrations/verl/README.md) for the full setup, the config overrides for each mode, PD disaggregation, observability, and a complete end-to-end KubeRay example.
## quickstart/
A full worked example of running the integration on a real cluster: KubeRay cluster templates, provisioning scripts, and the benchmark harness the routing results above come from. Nothing here is required to adopt the integration itself - see [`quickstart/README.md`](https://github.com/llm-d-incubation/llm-d-rl/blob/main/quickstart/README.md) for how to run it and how it is organised.
## Legacy
[`legacy/rl-controller/`](https://github.com/llm-d-incubation/llm-d-rl/blob/main/legacy/rl-controller) is an earlier exploration of a different architectural direction: a framework-agnostic Go/Python control plane that owns weight sync, engine lifecycle, and generation routing over plain HTTP, with no Kubernetes dependency. It has had no active development or users for several months while work moved to the EPP-based routing above. It is kept for reference; see [its README](https://github.com/llm-d-incubation/llm-d-rl/blob/main/legacy/rl-controller/README.md) for details, but it is not the recommended starting point for new work.
## License
Apache License 2.0
## About
No description, website, or topics provided.
### Resources
### Stars
**3** stars
### Watchers
**0** watching
### Forks
[ forks](https://github.com/llm-d-incubation/llm-d-rl/forks)
## 
No releases published
## 
## Languages
  * [Python69.5%](https://github.com/llm-d-incubation/llm-d-rl/search?l=python)
  * [Go12.8%](https://github.com/llm-d-incubation/llm-d-rl/search?l=go)
  * [Shell12.4%](https://github.com/llm-d-incubation/llm-d-rl/search?l=shell)
  * [Go Template4.6%](https://github.com/llm-d-incubation/llm-d-rl/search?l=go-template)
  * [Other0.7%](https://github.com/llm-d-incubation/llm-d-rl/search?l=Other)


You can’t perform that action at this time. 
