---
title: 'Roadmap: SGLang Router · Issue #10341 · sgl-project/sglang'
id: roadmap-sglang-router-issue-10341-sgl-projectsglang
tags:
- kv-cache-routing
- sglang-router
- radix-tree
- cache-aware-routing
- pd-disaggregation
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:54:14.436686Z'
updated: '2026-10-10T14:52:17.307372Z'
source: https://github.com/sgl-project/sglang/issues/10341
source_domain: github.com
fetched_at: '2026-09-14T11:54:14.416979Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub roadmap issue (opened Sep 11, 2025, child of sgl-project/sglang #7736
  ''Development Roadmap 2025 Q3'') tracking planned work on the standalone SGLang
  Router — a comparison point for alternative prefix/cache-aware routing designs.
  Planned workstreams: (1) multi-model + model-aware load balancing, where model-aware
  selection (by priority, cost, input complexity, capabilities) runs as a pre-filter
  ahead of existing worker-selection policies (cache-aware, power-of-two, round-robin,
  random), via a policy registry supporting multiple radix trees per model family;
  (2) a lightweight in-memory state layer plus a ''naive gRPC mesh'' for high availability
  across router replicas without external Redis/etcd, synchronizing rate-limit buckets,
  tenant/key radix trees, model registry, and per-model/per-mode worker states with
  eventual consistency and fast convergence (explicitly prioritizing low overhead
  over strong consistency); (3) a gRPC router core connecting directly to the SGLang
  scheduler for request admission, with the router itself running tokenizer, detokenizer,
  tool parser, and reasoning parser inline behind a unified OpenAI-compatible entrypoint,
  supporting both monolithic and PD-disaggregated (prefill/decode) serving modes;
  (4) a Responses API, MCP module, and data connectors for enterprise integration.
  Confirms SGLang''s router-side cache-awareness is implemented via radix trees (plural,
  one per model family under the policy registry) synchronized across replicas rather
  than a centralized KV-event bus.'
utility_score: 10.0
---

[Skip to content](https://github.com/sgl-project/sglang/issues/10341#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/sgl-project/sglang/issues/10341) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/sgl-project/sglang/issues/10341) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/sgl-project/sglang/issues/10341) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/sgl-project/sglang/issues/10341).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fsgl-project%2Fsglang) You must be signed in to change notification settings
  * [ Fork 8.8k ](https://github.com/login?return_to=%2Fsgl-project%2Fsglang)
  * [ Star  35.9k ](https://github.com/login?return_to=%2Fsgl-project%2Fsglang)


#  Roadmap: SGLang Router #10341
Copy link
Copy link
Closed
1 / 11 of 1 issue completed
Parent:[Development Roadmap (2025 Q3)](https://github.com/sgl-project/sglang/issues/7736)
Closed
[Roadmap: SGLang Router](https://github.com/sgl-project/sglang/issues/10341#top)#10341
1 / 11 of 1 issue completed
Parent:[#7736](https://github.com/sgl-project/sglang/issues/7736)
Copy link
Assignees
Labels
## Description
opened [on Sep 11, 2025](https://github.com/sgl-project/sglang/issues/10341#issue-3408149075)
Last edited by slin1237
Collaborator
Issue body actions
## High Level
## Multi-Model Support + Model-Aware Load Balancing
  * **Multi-model support** : one router instance handles many model families (e.g., LLaMA, Mistral, Qwen, VLMs).
  * **Serving modes** : support both **Monolithic** (single pool) and **PD-disaggregation** (prefill + decode pools).
  * **Model-aware selection** : choose models based on **priority, cost, input complexity, and capabilities**.
  * **Complement existing load balancers** : model-aware selection runs **before** worker selection and works with cache aware, power of two, round robin, and random 
    * Worker registry that allows different lookup
embedding support
rerank support [@tao12345666333](https://github.com/tao12345666333)
Policy registry allowing multiple radix trees and different policies for different model family
Stateless router manager which manages all type of routers
PII detection plugin and native PII detection component/endpoint
Semantic Router for model awareness load balancing (assigned [@tao12345666333](https://github.com/tao12345666333)
OpenAI endpoint routers supporting chat completion and embedding
Model priority and cost selection
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


## Lightweight State Layer + gRPC Mesh (HA, no external DB)
  * **HA without Redis/etcd** : run multiple router replicas with a lightweight in-memory state layer; avoid external databases.
  * **State to manage** : rate-limit buckets, tenant/key **radix tree** , model registry, and **worker states** (per-model, per-mode).
  * **Worker management API (CRUD)** : treat each model backend as a **worker** ; expose create/update/delete/list for workers.
  * **Naive gRPC mesh for sync** : replicas exchange deltas/snapshots over gRPC to keep state converged across the fleet.
  * **Preserve LB accuracy** : mesh keeps the radix tree and worker load/health info aligned so selections remain correct under multi-replica deployments.
  * **Eventual consistency with fast convergence** : prioritize low overhead and quick catch-up; no single replica is required for forward progress. 
    * Worker management API
Deprecate legacy `add_worker` `remove_worker` and `list_worker` APIs and use unified `/workers` API
Service Discovery with new worker management APIs and worker registry
Data mesh component [@tonyluj](https://github.com/tonyluj)
Data synchronization for worker registry among router replicas
Data synchronization for radix trees and policies among router replicas
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


## gRPC Router Core + Local Processing
  * **gRPC router core** : connects directly to the SGLang scheduler for request admission and routing.
  * **OpenAI-compatible API** : router surface is fully OpenAI-compatible for easy client integration.
  * **Local processing** : router runs tokenizer, detokenizer, gRPC client, tool parser, and reasoning parser inline.
  * **Unified entrypoint** : all requests flow through the router, which handles parsing, preprocessing, and scheduling before dispatching to workers. 
    * grpc router for regular serving mode and pd mode
grpc server that connects directly to SGL scheduler
grpc protobuf and client in router
tokenizer and detokenizer, along with testing (functional and benchmarks)
chat template implementation
hugging face hub download for tokenizer
tool parser (pythonic, json streaming, json, and xml) along with functional and load test
reasoning parser along with functional and load testing
chat completion handler
completion handler
generate handler
embedding handler
image processor
multi tokenizer, reasoning parser, and tool parser registry
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


## Responses API + MCP Module + Data Connectors
  * **Enterprise-friendly Responses API** : fully compatible with OpenAI’s responses API, designed for enterprise deployments.
  * **Chat statement management** : handles chat history and structured responses with enterprise compliance in mind.
  * **Data connectors** : clean, pluggable interface for Postgres and OracleDB; easily extendable to other database providers.
  * **MCP client implementation** : router includes its own MCP client for tool discovery and execution.
  * **Privacy-first design** : user data and chat content are never retained outside of enterprise-controlled databases; nothing is persisted to external services like OpenAI. 
    * MCP client with native rust MCP SDK
Data store interface for managing chat history
In memory data store provider
Postgres data store provider [@lengrongfu](https://github.com/lengrongfu)
OracleDB data store provider
Supports background responses along with GET responses
Integrate native MCP and tools
Multi turn support
MCP streaming
conversation api support
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


## Continuous Integration & Testing — Infrastructure Overhaul
  * **< 20 min CI** for PRs (fast path), deeper suites on main/nightly.
  * **Session-based workers** (pre-start once, reuse across tests, clean at end).
  * **Clear test layers** : 
    * **Unit (Rust)** : pure logic (selection, parsers, RL math).
    * **Component (Rust)** : router with **stub scheduler/workers**.
    * **E2E (Py)** : minimal real workers via **session fixture** , OpenAI-compatible client flows.
  * **Feature coverage** : circuit breaker, retries, SD, LB (cache-aware/P2C/RR/random), session stickiness, PD handoff.
  * **Strict validation** : CLI args, config merge, startup preflight. 
    * Session-based workers
PD and regular router load testing along with GPU usage report
PD and regular router lm-eval tests
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


## Typical Load Balancer Features
  * **Circuit breaker** : per-worker and per-endpoint circuit breakers with failure thresholds, cooldown windows, and half-open probe states.
  * **Retries with jitter and exponential backoff** : automatic retry logic for transient errors (timeouts, 5xx), capped with exponential backoff and full jitter to avoid thundering herd.
  * **Rate limiter** : token bucket–based limiter with dynamic scaling; supports global, per-tenant, and per-engine buckets. Includes engine-aware separation (aware of engine status and kvcache usage)
  * **API key integration** : middleware that authenticates incoming requests via API keys (header or bearer token). Keys map to tenants with scope, priority, and rate policies. 
    * Circuit breaker middleware integrated with worker dispatch and health checks
Retry utility with exponential backoff + jitter; integrated into router request pipeline
Rate limiter with global, per-tenant, and per-engine token buckets
Token bucket dynamic auto scaling based on the worker status
Engine aware token bucket adjustment based on connection (grpc vs http)
API key middleware for basic auth support
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


👍React with 👍20Reacted by Yineng Zhang, Tony Lu, Arthur Cheng, Swipe4057, ybyang, Xinyuan Tong, Jintao Zhang, Wenxuan Tan, cheyang, Varun Shah and 10 more
## Sub-issues
###  Collapse Sub-issuesSub-issues
1 of 11 of 1 Issues completed
Sub-issue options
1 sub-issues loaded
  * [Roadmap: gRPC-router: Chat Completions Handler #10879](https://github.com/sgl-project/sglang/issues/10879)
Manage this item ` command⌘ shift⇧ uU`


To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 
## Activity
[slin1237](https://github.com/slin1237)
self-assigned this
[on Sep 11, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19647097982)
[slin1237](https://github.com/slin1237)
added [on Sep 11, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19647099045)
[zhyncs](https://github.com/zhyncs)
added a parent issue [on Sep 11, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19647498470)
  * [Development Roadmap (2025 Q3) #7736](https://github.com/sgl-project/sglang/issues/7736)


[slin1237](https://github.com/slin1237)
added [on Sep 11, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19647540366)
[slin1237](https://github.com/slin1237)
assigned [on Sep 11, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19647544125)
### tonyluj commented on Sep 17, 2025 
Contributor
More actions
Hi [@slin1237](https://github.com/slin1237) I would like to contribute by implementing the "Multi-Model Support + Model-Aware Load Balancing" & "gRPC Router Core + Local Processing", could you please assign these tasks to me? Thanks.
Since some PRs have already been submitted and merged, I’ll proceed to follow up on the remaining tasks under the two topics.
### slin1237 commented on Sep 17, 2025 
CollaboratorAuthor
More actions
grpc router related PRs
  * [Implement Standalone gRPC Server for SGLang Python Scheduler #10283](https://github.com/sgl-project/sglang/pull/10283)
  * [[router] fix release workflow to include protobuf #10055](https://github.com/sgl-project/sglang/pull/10055)
  * [[router] fix grpc connection mode detection #9999](https://github.com/sgl-project/sglang/pull/9999)
  * [[router] move tokenizer, reasoning, tool initialization to server #9996](https://github.com/sgl-project/sglang/pull/9996)
  * [[router] fix grpc client url normalzation and health check #9939](https://github.com/sgl-project/sglang/pull/9939)
  * [[router] add tokenizer download support from hf hub #9882](https://github.com/sgl-project/sglang/pull/9882)
  * [[router] global tool parser registry #9840](https://github.com/sgl-project/sglang/pull/9840)
  * [[router] add reasoning parser readme #9837](https://github.com/sgl-project/sglang/pull/9837)
  * [Tool parser.benchmark #9835](https://github.com/sgl-project/sglang/pull/9835)
  * [[router] grpc router bootstraps #9759](https://github.com/sgl-project/sglang/pull/9759)
  * [[router] add llama3.2 multi json streaming parser #9735](https://github.com/sgl-project/sglang/pull/9735)
  * [[router] additional llama32 parser unit test and multi json support #9732](https://github.com/sgl-project/sglang/pull/9732)
  * [[router] additional pythonic parser unit test #9730](https://github.com/sgl-project/sglang/pull/9730)
  * [[router] upgrade kernel version in pd ci #9720](https://github.com/sgl-project/sglang/pull/9720)
  * [[router] add gpt-oss and glm4 tool parser #9703](https://github.com/sgl-project/sglang/pull/9703)
  * [[router] add step3 tool parser #9695](https://github.com/sgl-project/sglang/pull/9695)
  * [[router] add deepseek tool parser #9694](https://github.com/sgl-project/sglang/pull/9694)
  * [[router] restructure tool parser module folder #9693](https://github.com/sgl-project/sglang/pull/9693)
  * [[router] add ut for mistral, llama, pythonic, and streaming tool parser #9632](https://github.com/sgl-project/sglang/pull/9632)
  * [[router] add llama tool parser #9629](https://github.com/sgl-project/sglang/pull/9629)
  * [[router] add pythonic parser #9628](https://github.com/sgl-project/sglang/pull/9628)
  * [[router] add qwen tool parser #9623](https://github.com/sgl-project/sglang/pull/9623)
  * [[router] add mistral tool parser #9622](https://github.com/sgl-project/sglang/pull/9622)
  * [[router] add json tool parser #9516](https://github.com/sgl-project/sglang/pull/9516)
  * [[router] tokenizer arch doc #9513](https://github.com/sgl-project/sglang/pull/9513)
  * [[router] add tool parser base structure and partial json parser #9482](https://github.com/sgl-project/sglang/pull/9482)
  * [[router] remove all tokenizer metrics for performance #9474](https://github.com/sgl-project/sglang/pull/9474)
  * [[router] add glm and step3 reasoning parser #9415](https://github.com/sgl-project/sglang/pull/9415)
  * [[router] add tokenizer integration test with real mini tokenizer #9413](https://github.com/sgl-project/sglang/pull/9413)
  * [[router] add tokenizer chat template support #9370](https://github.com/sgl-project/sglang/pull/9370)
  * [[router] Implement gRPC SGLangSchedulerClient #9364](https://github.com/sgl-project/sglang/pull/9364)
  * [[router] adds reasoning parser pooling and thread-safe #9360](https://github.com/sgl-project/sglang/pull/9360)
  * [[router] add tiktokenizer and sequence in router #9354](https://github.com/sgl-project/sglang/pull/9354)
  * [[router] add dsr1, kimi, and qwen reasoning parser #9353](https://github.com/sgl-project/sglang/pull/9353)
  * [[router] Add spec for sglang scheduler #9322](https://github.com/sgl-project/sglang/pull/9322)
  * [[router] add reasoning parser base structure #9310](https://github.com/sgl-project/sglang/pull/9310)
  * [[router] add tokenizer metrics #9307](https://github.com/sgl-project/sglang/pull/9307)
  * [[router] tokenizer factory, hf tokenizer, and stop sequence detector #9293](https://github.com/sgl-project/sglang/pull/9293)
  * [[router] introducing tokenizer trait #9287](https://github.com/sgl-project/sglang/pull/9287)


### slin1237 commented on Sep 17, 2025 
CollaboratorAuthor
More actions
multi model related PRs
  * [[router] Add OpenAI backend support - core function #10254](https://github.com/sgl-project/sglang/pull/10254)
  * [[router] allow one router to support different model families and serving mode #10244](https://github.com/sgl-project/sglang/pull/10244)
  * [[router] Add Rerank Routing Logic in Regular Router #10219](https://github.com/sgl-project/sglang/pull/10219)
  * [[router] Add Rerank API Specification #9906](https://github.com/sgl-project/sglang/pull/9906)
  * [[router] Add IGW (Inference Gateway) Feature Flag #9371](https://github.com/sgl-project/sglang/pull/9371)


### slin1237 commented on Sep 17, 2025 
CollaboratorAuthor
More actions
responses api
  * [[router] Basic OAI Response api #10346](https://github.com/sgl-project/sglang/pull/10346)
  * [[router] move to mcp sdk instead #10057](https://github.com/sgl-project/sglang/pull/10057)
  * [[router] Add MCP Tool Handler  #9615](https://github.com/sgl-project/sglang/pull/9615)
  * [[router] Implement OpenAI Responses API specification #9367](https://github.com/sgl-project/sglang/pull/9367)


### slin1237 commented on Sep 17, 2025 
CollaboratorAuthor
More actions
CI improvement
  * [[router][ci] add gpu process check and free port before start server #10338](https://github.com/sgl-project/sglang/pull/10338)
  * [[router] add benchmark for regular router and pd router #10280](https://github.com/sgl-project/sglang/pull/10280)
  * [[router][ci] Add PD router mmlu test #10256](https://github.com/sgl-project/sglang/pull/10256)
  * [[router] Improve the e2e tests #10102](https://github.com/sgl-project/sglang/pull/10102)
  * [[router] enable sccache in ci and local build #10099](https://github.com/sgl-project/sglang/pull/10099)
  * [[router] Introduce router integration tests #10086](https://github.com/sgl-project/sglang/pull/10086)
  * [[router] add rust cache #10080](https://github.com/sgl-project/sglang/pull/10080)
  * [[router] add rust cache for rust unit test #10079](https://github.com/sgl-project/sglang/pull/10079)
  * [[router] add py binding unit tests to coverage 80% #10043](https://github.com/sgl-project/sglang/pull/10043)
  * [Simplify `Router` arguments passing and build it in docker image #9964](https://github.com/sgl-project/sglang/pull/9964)
  * [[router] include rust benchamrks #9932](https://github.com/sgl-project/sglang/pull/9932)
  * [[router] fix FunctionCallResponse proto, support arguments is null #9875](https://github.com/sgl-project/sglang/pull/9875)
  * [[router] add tokenizer benchmark #9427](https://github.com/sgl-project/sglang/pull/9427)
  * [[router] clean up lint warnings with clippy execution #9201](https://github.com/sgl-project/sglang/pull/9201)
  * [[router] optimize Rust compilation and development workflow #9133](https://github.com/sgl-project/sglang/pull/9133)


### slin1237 commented on Sep 17, 2025 
CollaboratorAuthor
More actions
bug fixes
  * [[Bugfix] fix pd chat completion protocol for batching support #10016](https://github.com/sgl-project/sglang/pull/10016)
  * [[router] clean up dependency injector to use ctx #10000](https://github.com/sgl-project/sglang/pull/10000)
  * [[router] add chat_template_kwargs in ChatCompletionRequest #9958](https://github.com/sgl-project/sglang/pull/9958)
  * [[router] Fix short timeout for the prefill client #9803](https://github.com/sgl-project/sglang/pull/9803)
  * [[Doc] add LWS(LeaderWorkerSet) use case in sgl-router README #9568](https://github.com/sgl-project/sglang/pull/9568)
  * [[router] address worker load tracking consistency #9523](https://github.com/sgl-project/sglang/pull/9523)
  * [[router] Move all protocols to spec.rs file  #9519](https://github.com/sgl-project/sglang/pull/9519)
  * [[router] fix error response in pd_router #9505](https://github.com/sgl-project/sglang/pull/9505)
  * [[router] ignore client error when record failure in pd_router #9503](https://github.com/sgl-project/sglang/pull/9503)
  * [[router] add right rustls dependency in sgl-router cargo.toml #9498](https://github.com/sgl-project/sglang/pull/9498)
  * [[router] fix router load guard tracking for streaming #9491](https://github.com/sgl-project/sglang/pull/9491)
  * [[Router] Add validation module for API parameters #9335](https://github.com/sgl-project/sglang/pull/9335)
  * [[router]restructure protocol modules for better organization #9321](https://github.com/sgl-project/sglang/pull/9321)
  * [[router] introduce prefill response draining for http compliance #9281](https://github.com/sgl-project/sglang/pull/9281)
  * [[router] add cargo clippy in CI and fix-up linting errors #9242](https://github.com/sgl-project/sglang/pull/9242)
  * [[router] fix pd prefill http request complinace issue #9237](https://github.com/sgl-project/sglang/pull/9237)
  * [[router] preserve original worker response header in router #9236](https://github.com/sgl-project/sglang/pull/9236)
  * [[router] allow more health check configuration #9198](https://github.com/sgl-project/sglang/pull/9198)
  * [router: Fix user guide link README.md #9122](https://github.com/sgl-project/sglang/pull/9122)
  * [[router] update router documentation #9121](https://github.com/sgl-project/sglang/pull/9121)
  * [[router] Add Rust Binary Entrypoint for SGLang Router #9089](https://github.com/sgl-project/sglang/pull/9089)
  * [refactor(pd-router): extract common patterns to reduce code duplication #9081](https://github.com/sgl-project/sglang/pull/9081)
  * [[router] update pyo3 version to 0.25.1 #9022](https://github.com/sgl-project/sglang/pull/9022)
  * [[router] upgrade kube version to latest #9018](https://github.com/sgl-project/sglang/pull/9018)


### slin1237 commented on Sep 17, 2025 
CollaboratorAuthor
More actions
rate limiter
  * [[router] add token bucket rate limiter #9656](https://github.com/sgl-project/sglang/pull/9656)
  * [[pd-router] add retry and circuit breakfor for pd router #9051](https://github.com/sgl-project/sglang/pull/9051)
  * [[router] regular router circuit breaker #8997](https://github.com/sgl-project/sglang/pull/8997)


### llfl commented on Sep 17, 2025 
Contributor
More actions
Hi [@slin1237](https://github.com/slin1237) , with details discussed with [@tonyluj](https://github.com/tonyluj) yesterday, I think I can help implementing the "Lightweight State Layer + gRPC Mesh (HA, no external DB)" part.
[slin1237](https://github.com/slin1237)
added [on Sep 18, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19777529545)
[slin1237](https://github.com/slin1237)
assigned [on Sep 18, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19777533010)
[slin1237](https://github.com/slin1237)
changed the title ~~[-] Roadmap: sglang gRPC Router (multi-model, model-aware, pluggable, mesh-synced)[/-]~~ [+]Roadmap: SGLang Router[/+] [on Sep 18, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19778428879)
### tao12345666333 commented on Sep 18, 2025 
Contributor
More actions
Thank you! I will start with a design proposal.
[slin1237](https://github.com/slin1237)
assigned [on Sep 18, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19778754067)
### tonyluj commented on Sep 23, 2025 
Contributor
More actions
We have prepared the design proposal [#10839](https://github.com/sgl-project/sglang/issues/10839) and development is in progress.
cc [@slin1237](https://github.com/slin1237)
[CatherineSue](https://github.com/CatherineSue)
added a sub-issue [on Sep 24, 2025](https://github.com/sgl-project/sglang/issues/10341#event-19904160887)
  * [Roadmap: gRPC-router: Chat Completions Handler #10879](https://github.com/sgl-project/sglang/issues/10879)


### lengrongfu commented on Oct 25, 2025 
Contributor
More actions
[@slin1237](https://github.com/slin1237) Hi, about "Postgres data store provider" this feature whether anybody working? if no, can assign me?
### slin1237 commented on Oct 25, 2025 
CollaboratorAuthor
More actions
> [@slin1237](https://github.com/slin1237) Hi, about "Postgres data store provider" this feature whether anybody working? if no, can assign me?
certainly
[slin1237](https://github.com/slin1237)
assigned [on Oct 25, 2025](https://github.com/sgl-project/sglang/issues/10341#event-20503300691)
### lengrongfu commented on Nov 1, 2025 
Last edited by lengrongfu
Contributor
More actions
"Supports background responses along with GET responses" this feature maybe compilte.
### slin1237 commented on Nov 11, 2025 
CollaboratorAuthor
More actions
closing
[slin1237](https://github.com/slin1237)
closed this as [completed](https://github.com/sgl-project/sglang/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Nov 11, 2025](https://github.com/sgl-project/sglang/issues/10341#event-20874549368)
[Sign up for free](https://github.com/signup?return_to=https://github.com/sgl-project/sglang/issues/10341)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/sgl-project/sglang/issues/10341)
## Metadata
## Metadata
### Assignees
  * [CatherineSue](https://github.com/CatherineSue)
  * [](https://github.com/key4ng)
  * [lengrongfu](https://github.com/lengrongfu)
  * [slin1237](https://github.com/slin1237)
  * [tao12345666333](https://github.com/tao12345666333)
  * [tonyluj](https://github.com/tonyluj)


### Labels
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
  * #### Parent issue
    * [ Development Roadmap (2025 Q3)sgl-project/sglang#7736 16 / 1716 of 17 issues completed](https://github.com/sgl-project/sglang/issues/7736)


### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
