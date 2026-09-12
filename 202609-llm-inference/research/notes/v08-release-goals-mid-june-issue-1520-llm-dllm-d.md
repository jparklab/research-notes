---
title: 'v0.8 Release Goals (Mid-June) · Issue #1520 · llm-d/llm-d'
id: v08-release-goals-mid-june-issue-1520-llm-dllm-d
tags:
- llm-d
- non-k8s
- roadmap
- discovery-plugin
- file-discovery
- slurm
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T20:47:12.492138Z'
updated: '2026-09-12T03:37:37.660804Z'
source: https://github.com/llm-d/llm-d/issues/1520
source_domain: github.com
fetched_at: '2026-09-11T20:47:12.469371Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Primary-source GitHub tracking issue #1520 (''v0.8 Release Goals (Mid-June)''),
  opened May 16 2026 by robertgshaw2-redhat, closed Jun 23 2026 as the v0.8.0 milestone
  completed (target Jun 21, code freeze Jun 14). This is the single most load-bearing
  source found for the non-Kubernetes router question: under ''sig-rl'' it lists ''Non-Kubernetes
  deployment mode'' (owner @ezrasilvera) linking the design proposal at github.com/llm-d/llm-d/blob/main/docs/proposals/non-kubernetes-mode.md,
  and enumerates the EXACT merged-PR implementation chain in llm-d-router:

  1. llm-d-router#908 - ''feat: Add pluggable DiscoveryPlugin interface''

  2. llm-d-router#1080 - ''file-discovery-1: add BackendUpsert/Delete interface methods
  and extract upsertEndpoint helper''

  3. llm-d-router#1081 - ''filediscovery-2: add DiscoveryBackendStore interface, NewDiscoveryNotifier''

  4. llm-d-router#1135 - ''file-discovery (3/4): add FileDiscovery plugin with YAML/JSON
  file loading and fsnotify watch''

  5. llm-d-router#1154 - ''rename BackendUpsert/Delete and DiscoveryBackendStore to
  us...'' (naming cleanup)

  6. llm-d-router#1218 - ''FileDiscovery plugin (4/4) - wiring into the runner''

  7. llm-d-router#1338 - ''docs(discovery): correct binary name, module path, Factory
  signature''

  8. llm-d-router#1362 - ''fix: resolve gRPC size parameter omission in runWithFileDiscovery
  runner initialization''

  9. llm-d-router#1368 - ''epp: wire FlowControl into file-discovery mode''

  10. llm-d-router#1404 - ''fix(datalayer): dispatch endpoint update notifications''

  This is a clean four-part sequence (file-discovery 1/4 through 4/4) building: a
  pluggable DiscoveryPlugin interface -> a DiscoveryBackendStore abstraction with
  BackendUpsert/Delete methods and a notifier -> a concrete FileDiscovery plugin that
  loads backend definitions from YAML/JSON and watches the file via fsnotify for live
  updates -> wiring the FileDiscovery plugin into the router''s runner (binary entrypoint),
  followed by cleanup/bugfixes including making Flow Control (the admission-control
  feature) work in file-discovery mode and fixing endpoint-update notification dispatch
  in the data layer.


  Also under sig-rl: a ''Non-Kubernetes deployment guide'' merged as llm-d/llm-d PR
  #1618 (''Add no-kubernetes-deployment guide''); a companion issue #1659 ''[Docs][RL]
  Guide: RL training loop with llm-d serving (no Kubernetes)''; and a related but
  distinct ''time-slicing platform PoC'' proposal at docs/proposals/rl-time-slicing-platform.md
  (GPU time-sharing for RL rollout workers, tentative/unvalidated as of this issue).
  A May 19 2026 comment from contributor chcost summarizes sig-rl goals as: (1) Python
  scheduler integration with EPP for RL-optimized routing, (2) ''Non-K8s mode - Enables
  llm-d in Slurm/research labs without Kubernetes (file-based discovery)'', (3) time-slicing
  GPU sharing, (4) deployment guide for an llm-d-rl controller with framework examples.


  Other v0.8 goals of note for router/disaggregation context: sig-router lists ''finalize
  gaie migration'' (#1528), a stateless Responses/Messages API (#1529), Flow Control
  graduation to production (#1530), and a multi-modal guide (#1531); sig-disaggregation
  lists NIXL TTL (#1522), NIXL request draining (#1523), a Mooncake connector (#1524),
  a MoRI connector (via #1470), and DP-aware WideEP scheduling graduation (#1521);
  sig-kv-disaggregation lists multi-tier CPU->storage offloading, HMA support, NIXL
  P/D + P2P backends, filesystem and object-store backends, and heterogeneous TP work,
  plus a P2P NIXL EC-Connector tracked at llm-d-kv-cache#602 and storage KV-cache
  indexing tracked at llm-d-kv-cache#520.'
---

*Suggested by [[llm-d-v07-from-feature-introduction-to-production-hardening-llm-d]] — v0.8 milestone issue cited as tracking the Non-Kubernetes deployment mode / Slurm / file-based service discovery roadmap item*

[Skip to content](https://github.com/llm-d/llm-d/issues/1520#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/issues/1520) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/issues/1520) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/issues/1520) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/issues/1520).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 761 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


#  v0.8 Release Goals (Mid-June)
Copy link
Copy link
Closed
Closed
[v0.8 Release Goals (Mid-June)](https://github.com/llm-d/llm-d/issues/1520#top)#1520
Copy link
Labels
[release/v0.8v0.8 release (Jun 2026)](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3A%22release%2Fv0.8%22)v0.8 release (Jun 2026)
Milestone
## Description
opened [on May 16, 2026](https://github.com/llm-d/llm-d/issues/1520#issue-4460278591)
Last edited by robertgshaw2-redhat
Contributor
Issue body actions
llm-d v0.8.0 release target: June 21st, code freeze: June 14th
Important
This is a time based release. We will cut features that are incomplete.
### Themes:
  * solidify llm-d ci coverage and project operations
  * expand accelerator coverage
  * graduate multimodal / batch / flow control to production
  * introduce initial RL support


### sig-cicd
  * clearly document release process - [[Docs] Create release process documentation #1660](https://github.com/llm-d/llm-d/issues/1660)
clearly document ci job - [[Doc]: Document CI System #1518](https://github.com/llm-d/llm-d/issues/1518)
migrate ci to use nightly images - [[CI] Migrate CI To Use Nightly Images #1516](https://github.com/llm-d/llm-d/issues/1516)
add lm-eval and perf benchmark to each guide - [[CI] Update Guide Evaluation in CI #1517](https://github.com/llm-d/llm-d/issues/1517)
add tpu nightly tests - [[CI/CD][TPU]: Add TPU nightly tests #1471](https://github.com/llm-d/llm-d/issues/1471)
add amd nightly tests - [[CI/CD][AMD]: Support Nightly Jobs on MI3xxX & Pollara NICs #1468](https://github.com/llm-d/llm-d/issues/1468)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-installation
  * migrate epp helm charts to llm-d-router - [[Install] Migrate llm-d-router Helm Charts to llm-d from GAIE #1535](https://github.com/llm-d/llm-d/issues/1535)
predicted latency build out of llm-d - [[Install] Migrate llm-d-router Helm Charts to llm-d from GAIE #1535](https://github.com/llm-d/llm-d/issues/1535) ([@kaushikmitr](https://github.com/kaushikmitr))
migrate cuda images to upstream - [[Install] Migrate WideEP CUDA Images to Upstream vLLM #1532](https://github.com/llm-d/llm-d/issues/1532) ([@wseaton](https://github.com/wseaton))
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-docs
  * consolidate prereq for gateway install - [[Docs] Consolidate Gateway Guides/Prereqs #1538](https://github.com/llm-d/llm-d/issues/1538)
streamline monitoring setup and presentation - [[Docs] Streamline Monitoring Setup #1537](https://github.com/llm-d/llm-d/issues/1537) ([@sudoalok](https://github.com/sudoalok))
document user apis - [[Docs]: Document APIs supported by llm-d #1512](https://github.com/llm-d/llm-d/issues/1512)
document operations - [[Doc]: Better Document Operational Flows #1519](https://github.com/llm-d/llm-d/issues/1519)
add guide of roll-out - [[Docs]: Guides to demonstrate various rollouts scenarios #1473](https://github.com/llm-d/llm-d/issues/1473)
add guide for multiple models - [[Docs]: A guide for deploying multiple models #1472](https://github.com/llm-d/llm-d/issues/1472)
configure draining in all guides - [[Guides] update all guides to properly configure request draining #1525](https://github.com/llm-d/llm-d/issues/1525)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-disaggregation
  * NIXL TTL - [[P/D] Improve Operational Flows with NIXL TTL #1522](https://github.com/llm-d/llm-d/issues/1522) ([@NickLucche](https://github.com/NickLucche))
NIXL Request Draining - [[P/D] Improve Operational Flows with Request Draining #1523](https://github.com/llm-d/llm-d/issues/1523) ([@markmc](https://github.com/markmc))
Improve vLLM GPU<>NIC selection - [[P/D+WideEP] Improve vLLM GPU<>NIC Selection #1539](https://github.com/llm-d/llm-d/issues/1539) ([@rajkiranjoshi](https://github.com/rajkiranjoshi))
Mooncake connector - [[P/D] Support Mooncake Connector #1524](https://github.com/llm-d/llm-d/issues/1524) ([@Gregory-Pereira](https://github.com/Gregory-Pereira))
MoRI connector - [[WideEP]: AMD MI355X MoRI EP Guide for Pollara/Thor-2 NIC #1470](https://github.com/llm-d/llm-d/issues/1470) ([@vcave](https://github.com/vcave))
graduate DP-Aware scheduling to production - [[P/D] Graduate DP-Aware WideEP Scheduling to Production #1521](https://github.com/llm-d/llm-d/issues/1521) ([@tessapham](https://github.com/tessapham))
GB200 Guide - [[Guides][GB200][WideEP] Add GB200 WideEP Guide #1527](https://github.com/llm-d/llm-d/issues/1527) ([@tlrmchlsmth](https://github.com/tlrmchlsmth))
MI355X WideEP - [[WideEP]: AMD MI355X MoRI EP Guide for Pollara/Thor-2 NIC #1470](https://github.com/llm-d/llm-d/issues/1470) ([@vcave](https://github.com/vcave))
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-router ([llm-d/llm-d-router#1162](https://github.com/llm-d/llm-d-router/issues/1162))
  * finalize gaie migration - [[Router] Finalize GAIE Migration #1528](https://github.com/llm-d/llm-d/issues/1528) ([@ahg-g](https://github.com/ahg-g))
api - stateless messages and responses api - [[Router] Responses and Messages API #1529](https://github.com/llm-d/llm-d/issues/1529)
flow control guide graduation - [[Router] Graduate Flow Control to Production #1530](https://github.com/llm-d/llm-d/issues/1530) ([@LukeAVanDrie](https://github.com/LukeAVanDrie))
multi-modal guide creation - [[Router] Create Multi-Modal Guide #1531](https://github.com/llm-d/llm-d/issues/1531) ([@hexfusion](https://github.com/hexfusion) )
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-kv-disaggregation
  *   * Multi-tier (CPU -> storage) Offloading ([@ronensc](https://github.com/ronensc) [@orozery](https://github.com/orozery) [@dannyharnik](https://github.com/dannyharnik)) 
    * updated vLLM docs ([@effi-ofer](https://github.com/effi-ofer))
    * HMA support ([@orozery](https://github.com/orozery))
    * NIXL P/D + P2P backends ([@LiranS](https://github.com/LiranS))
    * filesystem backend ([@kfirtoledo](https://github.com/kfirtoledo) [@rotemsha](https://github.com/rotemsha))
    * object-store backend ([@effi-ofer](https://github.com/effi-ofer))
    * heterogeneous TP ([@orozery](https://github.com/orozery))
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 
fs-connector HMA support ([@kfirtoledo](https://github.com/kfirtoledo))
P2P NIXL EC-Connector, vLLM - [EPD Disaggregation: CPU NIXL EC Connector llm-d-kv-cache#602](https://github.com/llm-d/llm-d-kv-cache/issues/602) ([@OmerPaz](https://github.com/OmerPaz))
  * Advanced prefix-cache aware routing 
    * Storage connector lookup scalability + storage KV-cache indexing - [Add storage KV-cache indexing support llm-d-kv-cache#520](https://github.com/llm-d/llm-d-kv-cache/issues/520) ([@albertoperdomo2](https://github.com/albertoperdomo2))
    * HMA-aware prefix-cache indexing - [Support HMA (Hybrid Memory Attention) in Prefix-Cache Aware Scheduling llm-d-kv-cache#336](https://github.com/llm-d/llm-d-kv-cache/issues/336) ([@kapiljain1989](https://github.com/kapiljain1989))
    * Embeddings-cache aware scheduling ([@guygir](https://github.com/guygir))
    * Speculative indexing - [Speculative Indexing for the KV-Cache Aware Routing llm-d-kv-cache#353](https://github.com/llm-d/llm-d-kv-cache/issues/353) ([@bongwoobak](https://github.com/bongwoobak))
    * External KV-cache invalidate/reset API - [Add support for externally invalidate/reset the KV cache llm-d-kv-cache#396](https://github.com/llm-d/llm-d-kv-cache/issues/396) ([@yash9263](https://github.com/yash9263))
    * [[sglang] Add sglang to tiered prefix cache guide  #1631](https://github.com/llm-d/llm-d/issues/1631) ([@zdtsw](https://github.com/zdtsw))
    * [[sglang] Add sglang guide to the precise prefix cache routing #1632](https://github.com/llm-d/llm-d/issues/1632) ([@zdtsw](https://github.com/zdtsw))
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-autoscaling
  * Deprecate VariantAutoscaling CRD and migrate to llm-d.ai API group - [[Autoscaling] Deprecate & Remove the VariantAutoscaling CRD llm-d-autoscaling#1130](https://github.com/llm-d/llm-d-autoscaling/issues/1130) ([@lionelvillard](https://github.com/lionelvillard))
Improve WVA observability (metrics, dashboards, alerting) - [[Autoscaling] Epic: Improve WVA Observability — Metrics, Events, Alerting, and Dashboards llm-d-autoscaling#911](https://github.com/llm-d/llm-d-autoscaling/issues/911) ([@shuynh2017](https://github.com/shuynh2017))
Throughput Analyzer v0 and benchmarks - [Initial Throughput Analyzer v0 llm-d-autoscaling#902](https://github.com/llm-d/llm-d-autoscaling/issues/902) ([@deanlorenz](https://github.com/deanlorenz))
Limiter improvements (namespace-scoped, quota-based) - [Limiter improvements: namespace-scoped inventory, quota-based limiter, and limiter chain llm-d-autoscaling#1000](https://github.com/llm-d/llm-d-autoscaling/issues/1000)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-batch-inference
  * graduate batch gateway to production - [[Batch] Graduate Batch Gateway to Production #1533](https://github.com/llm-d/llm-d/issues/1533)
graduate async processor to production - [[Batch] Graduate Async Processor to Production #1534](https://github.com/llm-d/llm-d/issues/1534)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-rl
  * Python inference scheduler integration - [Integrate the python inference scheduler llm-d-incubation/llm-d-rl#3](https://github.com/llm-d-incubation/llm-d-rl/pull/3) ([@ezrasilvera](https://github.com/ezrasilvera))
  * Non-Kubernetes deployment mode - <https://github.com/llm-d/llm-d/blob/main/docs/proposals/non-kubernetes-mode.md> ([@ezrasilvera](https://github.com/ezrasilvera)) 
    * feat: Add pluggable DiscoveryPlugin interface - [feat: Add pluggable DiscoveryPlugin interface llm-d-router#908](https://github.com/llm-d/llm-d-router/pull/908)
    * file-discovery-1: add BackendUpsert/Delete interface methods - [file-discovery-1: add BackendUpsert/Delete interface methods and extract upertEndpoint helper llm-d-router#1080](https://github.com/llm-d/llm-d-router/pull/1080)
    * filediscovery-2: add DiscoveryBackendStore interface, NewDiscoveryNotifier - [filediscovery-2: add DiscoveryBackendStore interface, NewDiscoveryNotifier … llm-d-router#1081](https://github.com/llm-d/llm-d-router/pull/1081)
    * file-discovery (3/4): add FileDiscovery plugin with YAML/JSON loading and fsnotify watch - [file-discovery (3/4): add FileDiscovery plugin with YAML/JSON file loading and fsnotify watch llm-d-router#1135](https://github.com/llm-d/llm-d-router/pull/1135)
    * rename BackendUpsert/Delete and DiscoveryBackendStore - [rename BackendUpsert/Delete and DiscoveryBackendStore to us… llm-d-router#1154](https://github.com/llm-d/llm-d-router/pull/1154)
    * FileDiscovery plugin (4/4) - wiring into the runner - [FileDiscovery plugin (4/4) - wiring into the runner llm-d-router#1218](https://github.com/llm-d/llm-d-router/pull/1218)
    * docs(discovery): correct binary name, module path, Factory signature - [docs(discovery): correct binary name, module path, Factory signature,… llm-d-router#1338](https://github.com/llm-d/llm-d-router/pull/1338)
    * fix: resolve gRPC size parameter omission in runWithFileDiscovery - [fix: resolve gRPC size parameter omission in runWithFileDiscovery runner initialization llm-d-router#1362](https://github.com/llm-d/llm-d-router/pull/1362)
    * epp: wire FlowControl into file-discovery mode - [epp: wire FlowControl into file-discovery mode llm-d-router#1368](https://github.com/llm-d/llm-d-router/pull/1368)
    * fix(datalayer): dispatch endpoint update notifications - [fix(datalayer): dispatch endpoint update notifications llm-d-router#1404](https://github.com/llm-d/llm-d-router/pull/1404)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 
[Tentative] Time-slicing platform PoC validation - <https://github.com/llm-d/llm-d/blob/main/docs/proposals/rl-time-slicing-platform.md> ([@aishukamal](https://github.com/aishukamal))
Non-Kubernetes deployment guide - [Add no-kubernetes-deployment guide #1618](https://github.com/llm-d/llm-d/pull/1618) ([@ezrasilvera](https://github.com/ezrasilvera))
Guide: RL training loop with llm-d serving (no Kubernetes) - [[Docs][RL] Guide: RL training loop with llm-d serving (no Kubernetes) #1659](https://github.com/llm-d/llm-d/issues/1659)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### sig-benchmark
  * Nightly benchmarks with regression tracking for all "relevant" guides - [[CI/CD] Add automation to nightly testing of guides #1360](https://github.com/llm-d/llm-d/issues/1360)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


❤️React with ❤️3Reacted by alberto, Maroon Ayoub and Sam Batschelet
## Activity
[robertgshaw2-redhat](https://github.com/robertgshaw2-redhat)
pinned this issue [on May 16, 2026](https://github.com/llm-d/llm-d/issues/1520#event-25610016681)
[robertgshaw2-redhat](https://github.com/robertgshaw2-redhat)
added 
[release/v0.8v0.8 release (Jun 2026)](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3A%22release%2Fv0.8%22)v0.8 release (Jun 2026)
[on May 16, 2026](https://github.com/llm-d/llm-d/issues/1520#event-25610231235)
[robertgshaw2-redhat](https://github.com/robertgshaw2-redhat)
changed the title ~~[-][Draft] v0.8 Release Goals[/-]~~ [+][WIP] v0.8 Release Goals[/+] [on May 16, 2026](https://github.com/llm-d/llm-d/issues/1520#event-25611250514)
[robertgshaw2-redhat](https://github.com/robertgshaw2-redhat)
changed the title ~~[-][WIP] v0.8 Release Goals[/-]~~ [+][WIP] v0.8 Release Goals (Mid-June)[/+] [on May 16, 2026](https://github.com/llm-d/llm-d/issues/1520#event-25611252970)
### alexagriffith commented on May 18, 2026 
Contributor
More actions
(re-commenting from [#1103](https://github.com/llm-d/llm-d/issues/1103))
Hi! I'd like to take on the following two documentation pages for the next release:
Taking on:
Deploying Multiple Models (no file currently) Rollout New Version of the Service (no file currently) Both are listed under the Resources section of the docs revamp.
Should these cover any more or less of the following?:
Deploying Multiple Models:
  * Running multiple different models on the same cluster
  * Multi-model serving on shared hardware (LoRA adapters)
  * Best practices for resource allocation when deploying multiple models Rollout New Version:
  * Blue/green deployments for model server updates
  * Rolling updates with minimal downtime
  * Version management suggestions for both vLLM/SGLang and llm-d components
  * Testing strategies before promoting to production Let me know if you have specific requirements or examples you'd like me to reference!


### alexagriffith commented on May 18, 2026 
Contributor
More actions
I also made some basic style improvements that I hope will make it clearer to new users, lmk what you think [#1384](https://github.com/llm-d/llm-d/pull/1384)
### chcost commented on May 19, 2026 
Last edited by chcost
Contributor
More actions
Added proposed sig-rl goals. Summary:
  1. Python scheduler - Integrates llm-d EPP with py-inference-scheduler for RL-optimized routing
  2. Non-K8s mode - Enables llm-d in Slurm/research labs without Kubernetes (file-based discovery)
  3. Time-slicing - GPU time-sharing
  4. Deployment guide - Documentation for llm-d-rl controller with framework examples


### chcost commented on May 19, 2026 
Last edited by chcost
Contributor
More actions
## v0.8 Release Goals Update - sig-rl and sig-autoscaling Added
Updated the v0.8 release goals to include concrete items for **sig-rl** and **sig-autoscaling** , which previously had "XXX" placeholders.
Items still missing DRIs:
  * Most sig-cicd tasks (6 out of 6)
  * Most sig-docs tasks (6 out of 7)
  * sig-autoscaling limiter improvements
  * RL rollout guide


[robertgshaw2-redhat](https://github.com/robertgshaw2-redhat)
changed the title ~~[-][WIP] v0.8 Release Goals (Mid-June)[/-]~~ [+]v0.8 Release Goals (Mid-June)[/+] [on May 20, 2026](https://github.com/llm-d/llm-d/issues/1520#event-25761728381)
[liu-cong](https://github.com/liu-cong)
added this to [on May 21, 2026](https://github.com/llm-d/llm-d/issues/1520#event-25819721916)
[github-project-automation](https://github.com/apps/github-project-automation)
moved this to Todo in [on May 21, 2026](https://github.com/llm-d/llm-d/issues/1520)
[liu-cong](https://github.com/liu-cong)
added this to the [v0.8.0 release](https://github.com/llm-d/llm-d/milestone/13) milestone [on May 21, 2026](https://github.com/llm-d/llm-d/issues/1520#event-25819973828)
[liu-cong](https://github.com/liu-cong)
moved this from Todo to In progress in [on Jun 2, 2026](https://github.com/llm-d/llm-d/issues/1520)
### nacx commented on Jun 17, 2026 
Contributor
More actions
[#1788](https://github.com/llm-d/llm-d/pull/1788) has been added to the v0.8.0 milestone. Can we add it to this tracker?
### ahg-g commented on Jun 23, 2026 
Collaborator
More actions
Closing this issue as we are cutting the release.
[ahg-g](https://github.com/ahg-g)
closed this as [completed](https://github.com/llm-d/llm-d/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Jun 23, 2026](https://github.com/llm-d/llm-d/issues/1520#event-27113055090)
[github-project-automation](https://github.com/apps/github-project-automation)
moved this from In progress to Done in [on Jun 23, 2026](https://github.com/llm-d/llm-d/issues/1520)
[chcost](https://github.com/chcost)
unpinned this issue [on Jun 25, 2026](https://github.com/llm-d/llm-d/issues/1520#event-27203708440)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d/issues/1520)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d/issues/1520)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[release/v0.8v0.8 release (Jun 2026)](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3A%22release%2Fv0.8%22)v0.8 release (Jun 2026)
No type
### Projects
  * [llm-d Closed](https://github.com/orgs/llm-d/projects/27)
StatusShow more project fields


Show 1 closed project
### Milestone
  * [ v0.8.0 releaseClosed Jul 16, 2026, 100% complete ](https://github.com/llm-d/llm-d/milestone/13)


### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
llm-d/llm-d [ llm-d/llm-d ](https://github.com/llm-d/llm-d) on  Jun 2 
#### [[Docs] Create release process documentation #1660 ](https://github.com/llm-d/llm-d/issues/1660)
Summary
Move the release process from the GitHub issue template (`.github/ISSUE_TEMPLA…`
release/v0.9 
Press escape to close this hovercard 
