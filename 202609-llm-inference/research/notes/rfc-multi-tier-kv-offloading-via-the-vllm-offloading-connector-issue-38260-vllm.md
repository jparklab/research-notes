---
title: '[RFC]: Multi-tier KV offloading via the vLLM offloading connector · Issue
  #38260 · vllm-project/vllm'
id: rfc-multi-tier-kv-offloading-via-the-vllm-offloading-connector-issue-38260-vllm
tags:
- kv-cache
- vllm-issue
- rfc
- tiering-manager
- kv-events
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:53:37.914147Z'
updated: '2026-09-15T04:55:24.040875Z'
source: https://github.com/vllm-project/vllm/issues/38260
source_domain: github.com
fetched_at: '2026-09-14T11:53:37.891665Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'vLLM GitHub RFC #38260 (opened Mar 26, 2026, by dannyharnik) proposing the
  multi-tier KV offloading architecture that later shipped as the TieringOffloadingSpec
  (see kv-offloading-usage-guide-vllm and tiered-kv-cache-offloading-in-vllm-vllm-blog
  for the shipped feature). Defines a single primary tier (CPU DRAM, exclusive GPU
  access) plus multiple secondary tiers (storage, remote-node P/D, P2P), orchestrated
  by a new TieringManager. Key design principles as stated in the RFC: (1) always
  cascade a confirmed block to every secondary tier asynchronously; (2) primary tier
  is the sole gateway to GPU memory; (3) staged promotion -- lookup() returns None
  while a secondary-tier hit is being promoted to primary, and the scheduler retries;
  (4) all SecondaryTierManager methods (lookup/submit_store/submit_load/get_finished/touch)
  run non-blocking in the scheduler process, with actual data movement on the worker
  thread only for GPU<->CPU; (5) each secondary tier owns its own eviction policy.
  CPU memory is mandated to be stored in canonical TP-rank-1 form regardless of the
  vLLM node''s actual TP rank, enabling cross-TP-rank sharing/storage. Comment thread
  (through Jul 2026) tracks the RFC''s evolution into shipped PRs: #40020 (multi-tier
  data plane), #41968 (object-store tier), #43468 (self-describing KV events for single-tier
  CPUOffloadingSpec, explicitly NOT yet supported for TieringOffloadingSpec because
  tier promotions can create primary-tier store events with no live GPU job to describe),
  #46544 (opt-in hash-only BlockStored events for secondary tiers, with medium naming
  MEDIUM_CPU/MEDIUM_FS/MEDIUM_OBJ added alongside existing MEDIUM_GPU), and #48281
  (tier locality: ''medium says what the storage is; locality says where it sits relative
  to the publishing instance''). A maintainer (Change72) states explicitly that stale
  KV placement state is a routing-correctness hazard: ''whoever owns eviction should
  also provide invalidation. Otherwise, routers may score or route based on stale
  placement state.'' NOTE: several later comments (user coltonottley, Jul 2026) present
  extremely detailed self-reported ''production'' benchmark numbers (exact byte counts,
  token counts, decode tok/s) for an unmerged downstream fork implementing canonical
  TP1 CPU-primary storage; these are unverified, self-reported claims by a single
  GitHub commenter, not peer-reviewed or merged into vLLM, and should not be cited
  as vetted vLLM performance characteristics.'
utility_score: 13.0
---

[Skip to content](https://github.com/vllm-project/vllm/issues/38260#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/38260) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/38260) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/38260) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/issues/38260).
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


#  [RFC]: Multi-tier KV offloading via the vLLM offloading connector #38260
Copy link
Copy link
[[RFC]: Multi-tier KV offloading via the vLLM offloading connector](https://github.com/vllm-project/vllm/issues/38260#top)#38260
Copy link
Labels
## Description
opened [on Mar 26, 2026](https://github.com/vllm-project/vllm/issues/38260#issue-4145749280)
Last edited by dannyharnik
Issue body actions
### Motivation.
To date, vLLM offers native KV offloading to CPU memory but does not support further offloading from CPU memory to other tiers such as storage. Implementations for storage offload should either work directly with storage or implement their own CPU offloading as an additional tier. This document describes a high level design to natively support multi-tier KV offloading in vLLM.
The architecture supports a single primary tier, in most cases CPU DRAM, and multiple secondary tiers.
## Goals
  * Allow simple and native integration of the current vLLM CPU offloading with secondary tiers such as storage, or connection with other vLLM nodes for PD disaggregation settings or P2P communication of KV data.
  * Utilize async gpu<->cpu transfers for primary tier and async lookup for secondary tier loads
  * Support for HMA models out of the box
  * Seamless support for cross TP offloading and transfers
  * Simple, clean and performant implementation of PD communication via the CPU tier


What we don't intend to support
  * Direct GPU access (neither GPU-storage or GPU-GPU communication)
  * Limited flexibility for variance in block size. While we allow vLLM block size to vary, CPU block size must be constant across all vLLM nodes (and a multiple of the underlying vLLM block size).


## High level Design
The design starting point is the current offloading connector design. In this design the offloading connector uses the vLLM V1 connector API and translates it to a simpler more abstract API for a backend. The multi-tier offloading has several key changes to this framework:
  * It designates a single (CPU DRAM) backend as the primary tier but also allows multiple backends to serve as a secondary tier. However, these backends differ from the current backend definition in that the source for offloading (and target for loading) is the CPU DRAM tier rather than the GPU HBM.
  * Introduce a new component -- the **TieringManager** that serves as an orchestrator for the communication with the Primary and Secondary tiers. A more detailed description of this appears in the **TieringManager** section below.
  * The GPU-CPU offloading is **managed by the scheduler side** offloading manager yet **executed by the worker side** backend. Namely, operations are scheduled and invoked by the scheduler thread, whereas the actual data migration is executed by the worker thread (as it is today in the offloading connector). In contrast, all **secondary tier operations are both managed and executed by the scheduler side only**.
  * No matter what the TP rank of a vLLM is, the KV data in CPU DRAM will be stored in **canonical form** , that of TP rank 1. This is the foundation for supporting cross TP rank KV offloading support. For example, if the secondary tier is storage then all KV values will be stored in a single TP1 canonical form, no matter what the TP rank on the vLLM.


### Proposed Change.
In order to facilitate the design changes above we are implementing the changes described below to the current offloading connector.
## The TieringManager
The native KV offloading in vLLM v1 currently supports offloading **from GPU memory** to an external location (like CPU memory). This RFC extends the design to allow offloading **from CPU memory** to additional tiers such as local storage, object storage, and remote nodes (P/D disaggregation).
The `OffloadingConnector` interface is unchanged, it holds a single `OffloadingManager`. The new `TieringManager` implements that interface and orchestrates the tier hierarchy internally.
### Two tier types
**Primary Tier** : a single tier with exclusive access to GPU KV memory. The existing CPU Manager serves as the primary tier.
**Secondary Tier(s)** : one or more tiers with read/write access to the Primary Tier's CPU memory. No direct GPU access. Each secondary tier implements the `SecondaryTierManager` interface.
### SecondaryTierManager Interface

```
class SecondaryTierManager(ABC):
    def lookup(block_hashes) -> int | None
    def submit_store(job_metadata: JobMetadata) -> None
    def submit_load(job_metadata: JobMetadata) -> None
    def get_finished() - Iterable[JobResult]
    def touch(block_hashes)

@dataclass
class JobMetadata:
    job_id: int                         # unique job identifier
    block_hashes: list[BlockHash]       # which blocks are being transferred
    spec: CPUMemoryViewLoadStoreSpec    # memory views into CPU tensors + block IDs
```

`spec` is a zero-copy memory view into the primary tier's CPU tensors. For `submit_store` it is read-only (secondary tier reads from CPU); for `submit_load` it is writable (secondary tier writes into CPU).
### CPU Manager changes
Extend the CPU Manager to expose its worker's `cpu_tensors` so the `TieringManager` can pass **zero-copy memory views** to secondary tiers for direct reads and writes.
### Key Design Principles
  1. **Always cascade to all tiers** : When a block is confirmed in the primary tier, it is asynchronously pushed to every secondary tier.
  2. **Primary tier is the gateway** : Only the primary tier accesses GPU memory. Secondary tiers read/write CPU memory via memory views.
  3. **Staged promotion** : Blocks in secondary tiers must be promoted to the primary tier before the GPU can access them. `lookup()` returns `None` while promotion is in progress (scheduler retries).
  4. **Non-blocking scheduler methods** : All `SecondaryTierManager` methods run in the Scheduler process. `submit_store()` / `submit_load()` submit async jobs; `get_finished()` polls for completion.
  5. **Secondary tiers own their evictions** : Each secondary tier manages its own eviction policy independently.


### Store Flow (Cascade)

```
GPU → Primary Tier (CPU) → [all] Secondary Tiers

```

When `TieringManager.complete_store()` is called, the KV data is confirmed in CPU memory. The `TieringManager` calls `submit_store()` on every secondary tier to cascade the data asynchronously.
### Load Flow (Promotion)

```
GPU ← Primary Tier (CPU) ← Secondary Tier

```

When `TieringManager.lookup()` is invoked:
  1. Check primary tier first.
  2. For remaining blocks, check each secondary tier in order.
  3. On a hit, call `submit_load()` to initiate async promotion to the primary tier, then return `None` (retry later).


The `TieringManager` calls `get_finished()` on all secondary tiers each scheduling cycle to finalize completed jobs.
## Canonical CPU layout
The following Diagram depicts the general idea - the CPU memory layout is in canonical form, where each block holds all the KV data relevant to this block in consecutive memory layout. Loading and offloading from and to the secondary tier is always of a single memory buffer and is handled by a scheduler thread (rather than the GPU dedicated workers).
The main changes required to facilitate this are:
  * During initialization one of the worker side connectors allocates all CPU memory for the KV cache and assigns the relevant parts of this memory to each of the other workers. This is done in shared memory between the scheduler and workers.
  * Each worker is responsible for copying its relevant slice of the KV cache to the appropriate CPU memory locations.
  * We mandate that the layout in CPU memory will be in canonical form. In a TP setting the division between workers is according to the _heads_ count -- each worker gets an equal share of the attention heads. As in the drawing above the, we ask that the heads divide the KV CPU memory into contiguous regions and thus can be mapped easily to various TP ranks. There is no constraint on the GPU memory layout, but we achieve better performance and simpler code if that is the case in the GPU as well. This is explained in RFC [[RFC]: KV cache layout combining all layers per block #27742](https://github.com/vllm-project/vllm/issues/27742) and implemented in PR [[KVConnector][Core] Support cross-layer KV blocks #27743](https://github.com/vllm-project/vllm/pull/27743). This layout change enables coalescing copies between GPU and CPU to fewer large contiguous memory copies and thus enables efficient use of the DMA for this purpose (see discussion in the following blog <https://vllm.ai/blog/kv-offloading-connector> ).


## Potential Secondary Tiers
  * Storage -- This is the obvious secondary tier. Can include:
    * File System API
    * Object Storage
    * Key-Value Store
      * Can be either shared (remote) or local storage
  * PD disaggregation -- The current PD connector ("NIXL connector") in vLLM is GPU to GPU communication. Having an alternative CPU to CPU implementation while introduces more hops and therefore more latency, has several benefits:
    * Quicker offloading on the P node, once moved to local CPU can release GPU memory.
    * Shorter time GPU memory required on the D node. Only need to allocate GPU buffers after KV data arrives on the D node CPU memory
    * Simple and clean cross TP handling. No need for complex grid of GPU to GPU chatter.
    * Potentially faster communication on the P to D leg as we will only need to communicate a single unified buffer (relevant to a TP setting).
  * P2P -- a generalization of the PD setting is a general P2P communication between nodes.


### Feedback Period.
_No response_
### CC List.
[@ronensc](https://github.com/ronensc) [@orozery](https://github.com/orozery) [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) [@tlrmchlsmth](https://github.com/tlrmchlsmth) [@WoosukKwon](https://github.com/WoosukKwon) [@njhill](https://github.com/njhill) [@NickLucche](https://github.com/NickLucche) [@omerpaz95](https://github.com/omerpaz95)
### Any Other Things.
_No response_
### Before submitting a new issue...
  * Make sure you already searched for relevant issues, and asked the chatbot living at the bottom right corner of the [documentation page](https://docs.vllm.ai/en/latest/), which can answer lots of frequently asked questions.
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


👍React with 👍8Reacted by Cameron Quilici, Jonathan Chen, Sage, Mike Mulhearn, Effi Ofer, Maroon Ayoub, Amir Zadeh and Nathan Scott
## Activity
[dannyharnik](https://github.com/dannyharnik)
added [on Mar 26, 2026](https://github.com/vllm-project/vllm/issues/38260#event-23941867514)
### ruihong123 commented on May 21, 2026 
More actions
Can the design principle 2, "Primary tier is the gateway" be relaxed? With this limitation, the GDS can not be applied to this multi-layered caching design.
### orozery commented on May 24, 2026 
Collaborator
More actions
> Can the design principle 2, "Primary tier is the gateway" be relaxed? With this limitation, the GDS can not be applied to this multi-layered caching design.
GDS would be a separate connector. You could use it together with multi-tier offloading using `MultiConnector`.
👍React with 👍2Reacted by ruihong123 and Guy Margalit
### dannyharnik commented on May 25, 2026 
Author
More actions
> > Can the design principle 2, "Primary tier is the gateway" be relaxed? With this limitation, the GDS can not be applied to this multi-layered caching design.
> GDS would be a separate connector. You could use it together with multi-tier offloading using MultiConnector.
Optimally, we'd like to consider an approach that can selectively do loads directly from storage using GDS. This will still keep the offload path via the primary tier. In the case of TP>1 this will require the GDS to read a part of the file for each TP rank (doable but adds complexity).
👍React with 👍2Reacted by ruihong123 and Guy Margalit
### IgniteGo commented on Jun 22, 2026 
More actions
Thanks for the detailed RFC! The design for native multi-tier KV offloading looks very promising.
I have a quick conceptual question: What is the primary motivation for building this natively within vLLM, as opposed to relying on external connectors like **LMCache**? Are there specific architectural advantages, performance benefits, or core differences in this native implementation that external multi-tier cache solutions cannot achieve?
I’d love to understand the core distinction I might be missing. Looking forward to your insights!
### Change72 commented on Jun 22, 2026 
Contributor
More actions
The multi-tier offloading data plane has landed ([#40020](https://github.com/vllm-project/vllm/pull/40020) framework, [#41968](https://github.com/vllm-project/vllm/pull/41968) object-store tier), but the external KV-event contract is still unclear. Today secondary-tier state is internal to `TieringOffloadingSpec`, so an external KV-event consumer cannot reliably discover which blocks live **below** the primary CPU tier.
[#43468](https://github.com/vllm-project/vllm/pull/43468) added self-describing `BlockStored` / `BlockRemoved` events for the single-tier `CPUOffloadingSpec`, but it intentionally rejects `self_describing_kv_events` under `TieringOffloadingSpec`. The reason is that **tier promotions** can create primary-tier store events that do not correspond to a live GPU store job, so the current request-time metadata tracker cannot describe them.
We would like to extend self-describing KV events to the multi-tier path, and are thinking about a phased plan:
  1. **Tier-presence events.** Emit per-tier `BlockStored` / `BlockRemoved` events using the existing event shape: constituent `block_hashes` plus a stable `medium` value naming the tier. This is enough for consumers to maintain per-tier block presence during cascade-store, promotion, and eviction, even before every tier event carries the full self-describing payload.
  2. **Accurate secondary eviction.** Emit per-block `BlockRemoved` events when a secondary tier evicts. Today secondary eviction is internal to the secondary tier, and `JobResult` only reports `{job_id, success}`, so `TieringManager` likely needs an eviction-reporting path.
  3. **Full self-describing payload across tiers.** Persist the small self-describing metadata (`token_ids`, `parent_hash`, per-block `block_size`, cache-spec metadata, etc.) alongside the offloaded block so cascade, promotion, and secondary-tier events can all re-emit full payloads even after the original request is gone. This keeps the contract producer-side and consumer-agnostic: Dynamo, llm-d, and future consumers can all index the same event stream without consumer-specific hash-join logic.


Proposed API direction:
  * Use `medium` as the stable per-tier identifier, with one event describing one tier.
  * Extend `SecondaryTierManager` with an eviction-reporting path so `TieringManager` can emit accurate `BlockRemoved` events for secondary tiers.
  * Use `complete_store` for cascade-store events, `get_finished_jobs` for promotion completion, and secondary eviction reporting for remove events.


Happy to prototype this once there is agreement on tier identity, eviction reporting, and payload shape.
cc [@dannyharnik](https://github.com/dannyharnik) [@orozery](https://github.com/orozery) — would appreciate your guidance.
### IgniteGo commented on Jun 23, 2026 
More actions
Thank you for the detailed and clear explanation! It really helps me understand the current challenges and the proposed plan.
### orozery commented on Jun 23, 2026 
Collaborator
More actions
[@Change72](https://github.com/Change72) I think that supporting simple BlockStored events (just hashes, not self-describing) can be straightforward. Regarding simple BlockRemoved, it depends how eviction is managed for a secondary tier. I think that for storage, especially for shared storage, but maybe even for local, eviction should be managed outside of vLLM. This means the we (at kv_offload managers) don't have to emit BlockRemoved for these secondary tiers.
I would try to think of ways to avoid self-describing metadata serialization in vLLM. This complicates the data-path logically and adds performance penalty. Ideally, this should be handled externally.
cc [@vMaroon](https://github.com/vMaroon)
### Change72 commented on Jun 23, 2026 
Contributor
More actions
Thanks [@orozery](https://github.com/orozery) , that makes sense.
I opened a narrow first-step PR for the straightforward part [#46544](https://github.com/vllm-project/vllm/pull/46544). It only emits secondary-tier `BlockStored` events after a successful primary→secondary cascade store. The event is hash-only plus `medium` (`tier_type`), with no self-describing payload and no secondary `BlockRemoved`.
For `BlockRemoved`, I agree that vLLM does not have to own it in the general case. For remote/shared storage, eviction belongs to the storage/control plane. Even for local secondary tiers, the owner may be a local cache/storage manager instead of vLLM itself. The key point is simple: whoever owns eviction should also provide invalidation. Otherwise, routers may score or route based on stale placement state.
I also agree that full `token_ids` / `parent_hash` serialization in the vLLM secondary-tier data path has real complexity and performance cost. I would not make it the default assumption without a stronger reason.
Two follow-up contract questions remain for me:
  1. `medium` naming. CPU events use `"CPU"`, while secondary tiering currently exposes registered tier types like `"fs"` / `"obj"`. llm-d can mostly treat this as an opaque string, but Dynamo maps `medium` into fixed tiers like `HostPinned`, `Disk`, and `External`.
Should vLLM use stable wire names here, e.g. `"FS"` / `"OBJ"` or `"DISK"` / `"EXTERNAL"`, instead of arbitrary registered tier strings?
  2. Chunk granularity.


> Limited flexibility for variance in block size. While we allow vLLM block size to vary, CPU block size must be constant across all vLLM nodes (and **a multiple of the underlying vLLM block size**).
Lower tiers will likely prefer larger/chunked I/O to avoid small-I/O overhead. If a simple event only carries `hash + medium`, a router cannot tell whether that hash represents a normal block or a chunk/offload key. This is the same ambiguity we hit in [#43468](https://github.com/vllm-project/vllm/pull/43468), and why we added the self-describing payload there.
For example, a raw chunk key (the tail block hash) does not tell routers which constituent blocks are present. At the same time, Dynamo-style prefix indexing may need `parent_block_hash` and `token_ids`. Do you have any suggestions for a contract here?
### orozery commented on Jun 24, 2026 
Collaborator
More actions
> Should vLLM use stable wire names here, e.g. `"FS"` / `"OBJ"` or `"DISK"` / `"EXTERNAL"`, instead of arbitrary registered tier strings?
Sounds right. We should add consistent medium names to `kv_events.py` (where MEDIUM_GPU is already defined).
> For example, a raw chunk key (the tail block hash) does not tell routers which constituent blocks are present. At the same time, Dynamo-style prefix indexing may need `parent_block_hash` and `token_ids`. Do you have any suggestions for a contract here?
CPU/disk block size can also vary, but we want to be able to detect that from the router perspective. For disk, we created a config file per disk root directory which contains relevant information (including block size): 
[vllm/vllm/v1/kv_offload/file_mapper.py](https://github.com/vllm-project/vllm/blob/4c5bc41ba61640a9946e457afcfddfb5e99a1fb8/vllm/v1/kv_offload/file_mapper.py#L50-L61)
Lines 50 to 61 in [4c5bc41](https://github.com/vllm-project/vllm/commit/4c5bc41ba61640a9946e457afcfddfb5e99a1fb8)  
|  self.fields: dict = {   |  
| --- |  
|  "model_name": model_name,   |  
|  "hash_block_size": hash_block_size,   |  
|  "gpu_blocks_per_file": gpu_blocks_per_file,   |  
|  "tp_size": tp_size,   |  
|  "pp_size": pp_size,   |  
|  "pcp_size": pcp_size,   |  
|  "dcp_size": dcp_size,   |  
|  "dtype": str(dtype),   |  
|  "kv_cache_groups": kv_cache_groups or [],   |  
|  "inference_engine": inference_engine,   |  
For CPU, I think a possible solution would be a runtime config endpoint, like proposed in [#38147](https://github.com/vllm-project/vllm/issues/38147)
[Change72](https://github.com/Change72)
added a commit that references this issue [on Jun 24, 2026](https://github.com/vllm-project/vllm/issues/38260#event-27169227169)
[[kv_offload] Use stable wire medium names for offload tiers](https://github.com/Change72/vllm/commit/c3ac1f418727d7509614a2d203c553ed02dbc44a)
### Change72 commented on Jun 24, 2026 
Contributor
More actions
[@orozery](https://github.com/orozery) Thanks. The medium-name part is in [#46544](https://github.com/vllm-project/vllm/pull/46544) — I added `MEDIUM_CPU` / `MEDIUM_FS` / `MEDIUM_OBJ` next to `MEDIUM_GPU`, and `CPULoadStoreSpec` / `GPULoadStoreSpec` now use those constants too. Built-in secondary tiers emit stable wire media, while custom tiers fall back to their registered `tier_type`.
For chunk/index granularity, the config/runtime endpoint direction makes sense for block size and layout discovery. I’ll keep that as a separate follow-up rather than part of this PR.
[Change72](https://github.com/Change72)
added a commit that references this issue [on Jun 25, 2026](https://github.com/vllm-project/vllm/issues/38260#event-27216149414)
[[kv_offload] Make secondary tier events opt-in via medium()](https://github.com/Change72/vllm/commit/4655f21751cfc9a2308398a7b0ede12df6b72c45)
### Change72 commented on Jul 7, 2026 
Contributor
More actions
Update: [#46544](https://github.com/vllm-project/vllm/pull/46544) merged with a narrower scope after review. It establishes tier-owned KV event handling: each tier owns its events, while `TieringOffloadingManager` only aggregates events from the primary and secondary tiers.
It does not yet emit actual secondary-tier events or add `MEDIUM_FS` / `MEDIUM_OBJ`; my earlier comments above described an earlier revision. I’ll handle those in the follow-up already discussed: opt-in, hash-only BlockStored events from the built-in FS/OBJ tiers after successful stores. Secondary-tier removal/invalidation and self-describing payloads remain out of scope.
### Change72 commented on Jul 10, 2026 
Contributor
More actions
Update: [#47923](https://github.com/vllm-project/vllm/pull/47923) has merged. I also saw [#48021](https://github.com/vllm-project/vllm/pull/48021) covering P2P lookup and transfer, so I’m planning to work on the P2P event path next, especially tier reachability and routing metadata.
### Change72 commented on Jul 14, 2026 
Contributor
More actions
Following up on the tier locality work ([#48281](https://github.com/vllm-project/vllm/pull/48281), from [the #47923 review discussion](https://github.com/vllm-project/vllm/pull/47923#discussion_r3541681707)): how it composes with P2P, and planned follow-ups.
medium says what the storage is; locality says where it sits relative to the publishing instance.

```
A publishes (CPU, LOCAL)       # A is a valid P2P source
B fetches from A; the block lands in B's CPU tier
B publishes (CPU, LOCAL)       # A and B are both valid sources now
C publishes (FS, REMOTE)       # shared FS (NFS/Lustre); peers reach it directly

```

A fetched block re-advertises on its own: promotions complete through the same path that emits CPU BlockStored events. This is also why there is no P2P medium — P2P is a transport, not a placement.
### coltonottley commented on Jul 18, 2026 
Contributor
More actions
We independently implemented and production-qualified the RFC's **canonical TP1 CPU-primary** ownership model on **two independent TP=2 hosts, each with 2x RTX PRO 6000 Blackwell GPUs** , serving DeepSeek V4 Flash with MTP2, FP8 KV, and a 1M-token request limit. This is an evidence and implementation map for people finding the RFC later—not a claim that our downstream branch is a clean current-main cherry-pick.
## What production proved
### Canonical TP payload identity
We checksummed real model-produced logical CPU payloads after D2H completion and before transfer-resource recycling. Across all five hybrid KV groups, both TP ranks produced:
  * 126 exactly comparable keys;
  * 126 byte-identical matches;
  * zero mismatches or incomparable keys.


That includes full-history MLA, sliding-window/tail, compressor-state, and indexer groups—not only a pure-MLA fixture.
### One-writer / all-loader CPU primary
Our production ownership model keeps the scheduler's existing job/completion/refcount lifecycle:
  * one scheduler manager and allocator;
  * rank 0 performs the physical GPU→CPU store;
  * non-writer ranks return normal zero-byte completion votes;
  * a key is not ready until all worker votes complete;
  * every rank loads directly from the same node-local shared CPU bytes;
  * reset/cancellation/stale-generation behavior stays on the existing scheduler path.


A final immutable 52,173-token semantic test produced two stable cold controls. After GPU-only reset, replay restored 51,968 source-attributed external tokens with zero local GPU hits, 206 recomputed tokens, 431,169,920 aggregate H2D bytes, and exact six-answer identity in 0.872 seconds.
At TP=2, canonical storage nearly halved both resident CPU bytes and D2H store traffic. Measured density projected about 16.7M unique prompt tokens per 64 GiB shared CPU region, versus about 8.35M with two private TP copies.
### Lifecycle requirement
An ungraceful EngineCore exit can leave the shared mmap behind and block restart. Wildcard `/dev/shm` cleanup is unsafe when multiple endpoint owners coexist. Our unattended-production recovery uses:
  * an owner-scoped `0600` receipt;
  * exact version/owner/fingerprint/size/path validation;
  * `flock`;
  * host-side proof that the exact named endpoint owner is stopped or absent;
  * fail-closed handling for malformed receipts;
  * creator-only receipt ownership and idempotent exact-path cleanup.


Container PID is not valid host liveness evidence.
## Upstream map
Rather than opening competing implementations, we attached evidence to the existing owners:
  * **Canonical TP replica dedup / shared CPU primary:** [[KV Offload] Deduplicate replicated MLA KV in the shared CPU region #48906](https://github.com/vllm-project/vllm/pull/48906) Production evidence: [[KV Offload] Deduplicate replicated MLA KV in the shared CPU region #48906 (comment)](https://github.com/vllm-project/vllm/pull/48906#issuecomment-5012555181)
  * **Canonical per-layer mappings / writer election:** [[KV Connector] Add per-layer canonical KV page mappings for parallelism-agnostic offload #48408](https://github.com/vllm-project/vllm/pull/48408)
  * **Hybrid MTP/SWA store reachability required for real CPU replay:** [[Bugfix][KV Offload] Preserve reachable tails for hybrid SWA groups #48911](https://github.com/vllm-project/vllm/pull/48911)
  * **Unaligned pending physical GPU-block bound:** [[Bugfix][KV Offload] Bound unaligned SWA loads by physical GPU blocks #49052](https://github.com/vllm-project/vllm/pull/49052), fixing [CPU offload fatal: valid unaligned SWA external load exceeds aligned pending-block bound #48959](https://github.com/vllm-project/vllm/issues/48959) TP=4 validation: [[Bugfix][KV Offload] Bound unaligned SWA loads by physical GPU blocks #49052 (comment)](https://github.com/vllm-project/vllm/pull/49052#issuecomment-5012676213)
  * **EngineCore fatal exiting status 0 and defeating`Restart=on-failure` :** [Unexpected EngineCore death exits serving process 0, defeating Restart=on-failure #48966](https://github.com/vllm-project/vllm/issues/48966)


The downstream product also contains a compact exact-byte representation for hybrid groups, byte-budgeted fixed-page allocation, fixed-page scatter/gather with an explicit safe per-copy native mode, latest-head retention policy, owner-scoped shared-mmap recovery, and reset transfer-job cleanup. That branch accumulated diagnostic and repair history against an older pre-[#48150](https://github.com/vllm-project/vllm/pull/48150) base, so publishing it as a cherry-pick series would be misleading. The reusable path is to land/generalize those semantic pieces through the current-main owners above.
For anyone implementing the RFC: [#48906](https://github.com/vllm-project/vllm/pull/48906) + [#48408](https://github.com/vllm-project/vllm/pull/48408) are the correct current-main starting point for canonical CPU ownership/layout. The production results above validate the architecture across a real hybrid DeepSeek V4 TP layout and identify owner-scoped mmap recovery as a necessary unattended-service requirement.
AI assistance was used for implementation support, adversarial review, and benchmark analysis. The human submitter owns the evidence and contribution decisions.
### coltonottley commented on Jul 18, 2026 
Contributor
More actions
Follow-up capacity evidence from the same downstream compact CPU-primary implementation described above.
## TP=4 capacity-overflow proof
**Hardware topology:** one host with 4x RTX PRO 6000 Blackwell GPUs running a single TP=4 DeepSeek V4 Flash instance over PCIe 4. The server retained full 1,000,000-token admission and MTP=2. GPU KV capacity was 5,110,026 tokens. The CPU tier was 120 GiB total in the current private-per-rank TP=4 layout (30 GiB per rank).
We generated six byte-distinct prompts calibrated through the live tokenizer to exactly 900,000 tokens each. Aggregate cold traffic was therefore 5,400,000 tokens, deliberately exceeding the complete GPU KV pool.
All six cold requests had:
  * exactly 900,000 locally computed prompt tokens;
  * zero local GPU-cache hits;
  * zero external/CPU-cache hits;
  * exact expected output markers.


After a 35-second store drain, aggregate CPU-store accounting was 83,542,233,600 bytes. We then cleared GPU KV only and replayed slot 1 byte-identically.
Replay result:
  * 900,000 prompt tokens;
  * zero local GPU-cache hits;
  * **899,840 external CPU-KV hit tokens** ;
  * **160 locally recomputed boundary tokens** ;
  * 13,920,416,512 bytes loaded CPU -> GPU;
  * exact output identity;
  * 15.217 s first-geometry replay versus 474.4 s cold.


The server retained the same PID, zero restarts, no assertion/fatal/OOM/XID evidence, and the final full reset succeeded. The artifact manifest verified 75/75 files.
This is direct evidence for the CPU-primary tiering primitive in this RFC: a prefix displaced from a 5.11M-token GPU pool remained recoverable after 5.4M tokens of byte-distinct cold traffic. It also makes the replica-dedup motivation concrete. The private TP=4 layout wrote and loaded four rank-local copies; a canonical TP=1 CPU representation shared by all TP ranks would preserve the same logical prefix while materially reducing CPU capacity and PCIe write amplification.
As before, this validates the architecture and lifecycle behavior of the RFC. It is not a claim that our downstream diagnostic history is an honest current-main cherry-pick; reusable code should land through the current owners of the mapping/dedup/tiering work.
### coltonottley commented on Jul 23, 2026 
Last edited by coltonottley
Contributor
More actions
[@orozery](https://github.com/orozery) — following up on [@Change72](https://github.com/Change72)'s request in [#48906](https://github.com/vllm-project/vllm/pull/48906), we now have a clean current-main reference implementation of compact fixed-page CPU-primary storage for heterogeneous KV groups.
## Scope and ownership
This is an extension of the existing owners, not a competing path:
  * [[KV Connector] Add per-layer canonical KV page mappings for parallelism-agnostic offload #48408](https://github.com/vllm-project/vllm/pull/48408) remains the authority for canonical per-layer byte mappings and writer roles.
  * [[KV Offload] Deduplicate replicated MLA KV in the shared CPU region #48906](https://github.com/vllm-project/vllm/pull/48906) remains the authority for the shared single-copy CPU primary, rank-0 store, and all-rank load lifecycle.
  * The compact extension consumes those contracts to allocate and transfer each heterogeneous group's exact canonical bytes.
  * [[RFC]: Multi-tier KV offloading via the vLLM offloading connector #38260](https://github.com/vllm-project/vllm/issues/38260) remains the owner of multi-tier orchestration. This work changes the CPU-primary representation; it does not add a second tier or another tiering manager.


When mappings are absent, incomplete, non-invariant, role-inconsistent, or disagree across ranks, compact activation fails closed and the existing CPU representation remains active.
## Representation and scheduler contract
The implementation uses a fixed 64 KiB page pool rather than contiguous variable-size extents. A compact allocation is a logical byte range backed by one or more physical page spans.
The scheduler and worker deliberately consume different views of the same certified layout:
  1. Workers derive ordered per-layer canonical mappings from [[KV Connector] Add per-layer canonical KV page mappings for parallelism-agnostic offload #48408](https://github.com/vllm-project/vllm/pull/48408), retain rank-local packed GPU offsets/strides, and report a versioned role-neutral geometry identity.
  2. The scheduler accepts compact mode only after one complete all-rank report with matching ordered group/layer/run geometry, the expected world size, exactly one valid store writer, and all-rank load compatibility.
  3. The scheduler manager owns aggregate per-group byte charges, fixed-page allocation, readiness/refcounts, ARC/LRU eviction, replay-unit closure, atomic replacement/rollback, reset, and metrics.
  4. Workers translate each logical allocation through its physical page spans and submit GPU↔CPU scatter descriptors through the existing transfer/event/completion lifecycle.


This split avoids two failure modes we found on hardware: using scheduler aggregate charges as physical copy geometry, and losing packed rank-local GPU offsets while canonicalizing CPU placement.
## Why fixed pages
Our first compact implementation used contiguous variable-size extents. Under long-history churn it retained free bytes but no sufficiently large contiguous hole. The allocator then evicted an early full-history key, collapsing useful replay coverage.
Fixed pages remove that spatial failure mode. Replacement operates on complete cross-group replay units so retained bytes remain useful as a contiguous prefix rather than leaving unrelated resident fragments.
On DeepSeek V4 Flash, exact heterogeneous-group accounting reduced CPU bytes by approximately 19× versus charging every logical token at the largest packed GPU row. That number is model/layout-specific, not a general transformer claim. Combined with [#48906](https://github.com/vllm-project/vllm/pull/48906)'s TP=2 single-copy ownership, the measured end-to-end reduction versus the original replicated packed layout is approximately 38×.
## Copy submission
Compact scatter can produce many descriptors. We observed `cuMemcpyBatchAsync` segfaults and rank-local hangs on valid large/repeated submissions under CUDA 13.0 on RTX PRO 6000 Blackwell GPUs. That failure is tracked separately in [#49276](https://github.com/vllm-project/vllm/issues/49276).
The reference implementation keeps ordinary swaps on the existing batch path but lets compact scatter select ordered per-descriptor `cudaMemcpyAsync` on the same stream/event lifecycle. Native tests cover ordinary batching and a 20,000-descriptor D2H/H2D fallback; hardware qualification covers the full semantic store/evict/load path.
## Current evidence
Hardware topology: two independent TP=2 hosts, each with 2x RTX PRO 6000 Blackwell GPUs; additional TP=4 qualification used one four-GPU host. The current-main candidate serves DeepSeek V4 Flash with MTP2, FP8 KV, and full 1,000,000-token request admission.
Completed current-main gates, with machine-readable receipts to be included with the reference branch, include:
  * exact 52K semantic CPU-hop replay after GPU-only eviction: 51,968 external tokens, zero local GPU hits, 205 boundary tokens recomputed, 431,169,920 aggregate H2D bytes across both ranks, and exact output identity;
  * a deterministic one-GiB reduced-pool A–F replacement test: six byte-distinct ~97K prompts crossed at least four replacement boundaries; F/E restored 96,768 external tokens each; D/C/B/A missed cleanly; occupied bytes were exactly 825,884,672;
  * one physical D2H store under [[KV Offload] Deduplicate replicated MLA KV in the shared CPU region #48906](https://github.com/vllm-project/vllm/pull/48906) ownership and all-rank H2D load;
  * exact allocated/free/largest-extent/fragmentation accounting and reset behavior;
  * stable process, zero restarts, and no fatal/Xid during the accepted run;
  * full source audit and independent review against production and the previous hardware-passing implementation.


The exact immutable no-overlay image passed the full isolated TP=2 gate and a fresh TP=4 gate:
  * TP=2: full 1M admission, 1,177,731 GPU-KV tokens, exact one-writer store/all-rank load, exact 52K CPU-hop identity, deterministic one-GiB A–F replacement, exact reset/fragmentation metrics, 192.07 tok/s decode, and 0.613% write-through prefill penalty;
  * TP=4: full 1M admission, 4,898,007 GPU-KV tokens, six strict semantic CPU-hop probes with 48,128 external tokens and 803,200,000 aggregate H2D bytes each, 220.31 tok/s decode, 0.537% write-through prefill penalty, and 18/18 + 18/18 cold/post-reset agentic soak;
  * production acceptance: two independent TP=2 replicas run the exact image, and an exact 990,000-token request completed with exact semantic output and bounded allocator high-water without OOM.


The reference series was replayed conflict-free onto current [#48906](https://github.com/vllm-project/vllm/pull/48906) head `47de1bd` and [#48408](https://github.com/vllm-project/vllm/pull/48408) head `7793ccf`; all 18 per-commit patch IDs and the aggregate patch ID are unchanged. Refreshed-source validation and human approval are complete.
Reference branch: <https://github.com/coltonottley/vllm/tree/feature/native-compact-cpu-kv-current-main>
Existing public evidence:
  * shared-primary TP=2 evidence: [[KV Offload] Deduplicate replicated MLA KV in the shared CPU region #48906 (comment)](https://github.com/vllm-project/vllm/pull/48906#issuecomment-5012555181)
  * compact-versus-dedup ownership split and measured result: [[KV Offload] Deduplicate replicated MLA KV in the shared CPU region #48906 (comment)](https://github.com/vllm-project/vllm/pull/48906#issuecomment-5016345290)
  * canonical CPU-primary RFC evidence: [[RFC]: Multi-tier KV offloading via the vLLM offloading connector #38260 (comment)](https://github.com/vllm-project/vllm/issues/38260#issuecomment-5012714460)
  * TP=4 capacity proof: [[RFC]: Multi-tier KV offloading via the vLLM offloading connector #38260 (comment)](https://github.com/vllm-project/vllm/issues/38260#issuecomment-5012958163)
  * batch-copy failure: [[Bug]: cuMemcpyBatchAsync segfaults or hangs on large/repeated compact scatter submissions #49276](https://github.com/vllm-project/vllm/issues/49276)


I propose using the clean branch as the reference implementation, then landing it in owner-aligned review units: fixed-page manager, mapping/scatter consumer, and scheduler admission/lifecycle. That keeps [#48408](https://github.com/vllm-project/vllm/pull/48408) and [#48906](https://github.com/vllm-project/vllm/pull/48906) authoritative while avoiding a single 10K-line review.
AI assistance was used for implementation support, test development, adversarial review, and benchmark analysis. I reviewed every changed production line and own the contribution and evidence.
### coltonottley commented on Jul 27, 2026 
Contributor
More actions
Follow-up correction and publication update for the compact CPU-primary reference implementation above.
Prolonged low-power decode stalls were not a JIT or transfer failure. When the best-effort CPU tier was full, `prepare_store()` returned no allocation and the scheduler retried the same eligible store range on every subsequent scheduler step, putting failed cache admission on the decode critical path.
The corrected behavior treats that exact failed range as terminal cache loss for the request, advances progress, and falls back to recomputation. A later newly completed range remains eligible for one bounded attempt.
On two independent TP=2 replicas, each with 2x RTX PRO 6000 Blackwell GPUs, the patched 1 GiB canary completed equal 313,513-token arms with 1,024 output tokens at 238.7 and 242.5 output tok/s. Failed admissions occurred only as newly eligible ranges appeared during chunked prefill; there were zero failed-store retries during decode or after completion. A separate exact 52,173-token store -> GPU-only reset -> CPU reload gate restored 51,968 external tokens, recomputed 205 tokens, transferred 431,169,920 aggregate CPU-to-GPU bytes, and reproduced the exact control output.
Publication is split by owner rather than hidden inside a broad rebase:
  * Updated compact reference branch, minimally fast-forwarded with the qualified scheduler correction: <https://github.com/coltonottley/vllm/tree/feature/native-compact-cpu-kv-current-main> (head `4b20184b1d0552a996606f9aa148c862312eee1a`)
  * Generic current-main scheduler fix: [[KV Offload] Stop retrying failed best-effort stores without progress #50069](https://github.com/vllm-project/vllm/pull/50069)
  * Separate generic CUDA-runtime selection / safe host-registration fallback fix found during the same qualification: [[CUDA] Keep host registration and error drain on one runtime handle #50070](https://github.com/vllm-project/vllm/pull/50070)


[#48906](https://github.com/vllm-project/vllm/pull/48906) has merged, while [#48408](https://github.com/vllm-project/vllm/pull/48408) has advanced from separate store/load runs to `CopyRun` plus per-block writer election. The existing reference still carries the earlier DeepSeek V4 compressed-MLA/SlidingWindowMLA adaptation, and merged [#48906](https://github.com/vllm-project/vllm/pull/48906) still has a worker-global rank-0 store gate. I am therefore **not** presenting a force-rebase as a refreshed implementation. A v2 reference will follow only after those writer authorities are reconciled and requalified with exact TP=2 CPU-hop, replacement, reset/accounting, and failed-store liveness gates.
### coltonottley commented on Jul 27, 2026 
Contributor
More actions
[@Change72](https://github.com/Change72) [@orozery](https://github.com/orozery) — owner update following the reference implementation amendment above:
  * the qualified compact reference branch is now updated at `4b20184b1d0552a996606f9aa148c862312eee1a`;
  * the generic failed-store retry fix is isolated in [[KV Offload] Stop retrying failed best-effort stores without progress #50069](https://github.com/vllm-project/vllm/pull/50069);
  * the separate CUDA-runtime/fallback fix found during qualification is isolated in [[CUDA] Keep host registration and error drain on one runtime handle #50070](https://github.com/vllm-project/vllm/pull/50070).


I have deliberately deferred the compact v2 rebase rather than hiding an ownership decision: merged [#48906](https://github.com/vllm-project/vllm/pull/48906) still has worker-global rank-0 store authority, while current [#48408](https://github.com/vllm-project/vllm/pull/48408) has per-block writer election through `CopyRun`. The next compact version should consume one canonical writer owner, then rerun the exact TP=2 CPU-hop, replacement, reset/accounting, and failed-store liveness gates.
### kreeuwijk commented on Jul 28, 2026 
More actions
Can a disk usage limit be aded for the `fs` secondary tier? Not all environments can use drives up to the entire capacity, the disk usage limit is there in other KV cache solutions to ensure it plays nice with other consumers of space on the same disk.
### coltonottley commented on Jul 29, 2026 
Contributor
More actions
## Compact CPU-KV V2 follow-up
[@orozery](https://github.com/orozery) — following up on the compact CPU-primary reference posted here on July 23: you were right about the scheduler-cursor update in the later branch tip. Advancing `next_stored_chunk_idx` after a failed `prepare_store()` can skip the failed contiguous prefix and admit a disjoint later tail. I closed that attempt rather than preserve the premise.
The measured saturated retry/stall was real, but the root cause was different: the compact manager retained historical request IDs as replay-unit owners and repeatedly recomputed graph closure over those completed histories. Under long-running overlapping prefixes, that representation became unbounded and could form an ownership knot even when the endpoint was quiescent.
I replaced it with two owner-aligned changes:
  1. **Resident-content replay metadata.** Canonical replay units are now sets of resident `OffloadKey` content, not request/job IDs. Equal or subset histories deduplicate, extending units replace dominated units, and incomparable units coexist. Coherent eviction remains intact, so we keep the useful-retention benefit without historical request ownership.
  2. **Job-owned completion snapshots.** The existing asynchronous `TransferJobStatus` owns the immutable replay snapshot for each store job. Request context can clear immediately, and successful completion publishes from the exact job-local snapshot. No second completion lifecycle or shadow manager was added.


This keeps contiguous-prefix semantics: a failed admission does not advance the scheduler cursor to a later disjoint tail.
## Tested owner integration
The successor is based on tested [#48408](https://github.com/vllm-project/vllm/pull/48408) owner head `661d2a148a2bcbbd80e2d23751ebbe9d9e056be3`, which already contains merged [#48906](https://github.com/vllm-project/vllm/pull/48906). Live [#48408](https://github.com/vllm-project/vllm/pull/48408) later advanced to `8cf352b6db02fb7fd2d8803c06cf250d2692b792` through non-overlapping merges from `main`; the canonical mapping/common/worker owner blobs used here remain byte-identical. The successor consumes those owners rather than recreating them:
  * [[KV Connector] Add per-layer canonical KV page mappings for parallelism-agnostic offload #48408](https://github.com/vllm-project/vllm/pull/48408) remains authoritative for canonical per-layer/per-rank mappings and writer election.
  * [[KV Offload] Deduplicate replicated MLA KV in the shared CPU region #48906](https://github.com/vllm-project/vllm/pull/48906) remains authoritative for the shared single-copy CPU primary and ordinary non-compact replicated-layout lifecycle.
  * The compact manager remains the single owner of fixed-page allocation, readiness/refcounts, ARC/LRU policy, coherent replay-unit eviction, rollback, reset, and metrics.


One correction to my July 23 wording: compact mode no longer assumes one static writer rank. The tested [#48408](https://github.com/vllm-project/vllm/pull/48408) owner verifies tiling across all rank mappings and provides per-block writer election; the compact successor packages those verified all-rank mappings into one role-neutral consensus receipt. Every compact rank receives store jobs; `mapping.is_writer(block_id)` filters store descriptors per layer/per block, so writer election may rotate by block. Every rank retains its load descriptors. Ordinary [#48906](https://github.com/vllm-project/vllm/pull/48906) non-compact rank-0-store/all-rank-load behavior is unchanged.
The historical branch remains preserved at its rejected tip for auditability. The clean successor is a new branch based on the current owner composition:
  * branch: `feature/native-compact-cpu-kv-current-main-v2`
  * head: `ee437a64d5733acc101c77d7de22448069bc331a`
  * tree: `40f03ab0dd0786e196d015e7d66a0ec8bb54d21c`
  * 26 linear DCO commits above [[KV Connector] Add per-layer canonical KV page mappings for parallelism-agnostic offload #48408](https://github.com/vllm-project/vllm/pull/48408) head `661d2a148a2bcbbd80e2d23751ebbe9d9e056be3`


Reference branch: <https://github.com/coltonottley/vllm/tree/feature/native-compact-cpu-kv-current-main-v2>
## Source and hardware evidence
Source acceptance:
  * 232 passed, 2 honest GPU-required skips, 0 failed;
  * exact ARC and LRU 400-key victim identity;
  * complete structural identity before/after a 401-page no-fit admission;
  * no persistent request/job IDs in canonical replay metadata;
  * two asynchronous jobs from one request retain distinct exact snapshots;
  * Ruff, format check, `py_compile`, and diff checks pass.


Hardware topology:
  * TP=2 qualification used one isolated two-GPU replica on 2x RTX PRO 6000 Blackwell GPUs while a separate two-GPU replica remained the production control.
  * TP=4 qualification used all 4x RTX PRO 6000 Blackwell GPUs on the same host.
  * Exact same immutable v58 image in both gates; DeepSeek V4 Flash, MTP2, FP8 KV, full 1,000,000-token request admission, no source overlays.


TP=2 evidence:
  * tested-[[KV Connector] Add per-layer canonical KV page mappings for parallelism-agnostic offload #48408](https://github.com/vllm-project/vllm/pull/48408) two-rank compact consensus;
  * exact 52K store → GPU-only reset → CPU-hop replay: zero local hits, 51,968 external tokens, 205 recomputed boundary tokens, 431,169,920 aggregate H2D bytes, exact output;
  * deterministic one-GiB A–F replacement: F/E restored 96,768 external tokens each; D/C/B/A missed cleanly; occupied bytes 825,884,672;
  * 190.77 tok/s decode;
  * byte-identical warmed prefill: 9,612.68 tok/s no-offload vs 9,555.80 write-through, 0.592% penalty;
  * C2/C4/C8 ramp and contamination-free 18/18 cold + 18/18 post-reset agentic soak;
  * same PID, zero restarts, clean reset/health/error gates.


TP=4 evidence:
  * four-rank compact consensus, text/tool smoke, full 1M admission, 4,898,007 GPU-KV tokens;
  * six strict semantic CPU-hop probes. Every accepted hop had zero local hits, 48,128 external tokens, 803,200,000 aggregate H2D bytes, and exact cold-answer identity;
  * 222.02 tok/s scored decode;
  * byte-identical 94,314-token prefill: 4,909.49 tok/s no-offload vs 4,908.05 write-through, 0.029% penalty;
  * same PID and zero restarts through correctness/performance;
  * contamination-free 18/18 cold + 18/18 post-GPU-reset agentic soak; replay restored 294,144 external tokens;
  * peak temperatures 59/70/69/52°C across GPUs 0–3 under the accepted performance/power capture;
  * exact TP=4 teardown and owner-scoped mmap cleanup;
  * exact v58 TP=2 candidate and v57 TP=2 control restored with fresh receipts/full1M/health/error acceptance; both endpoint drains released.


Performance numbers describe the whole current-main v58 candidate. I did not build a same-base control image, so I am not attributing inherited current-main changes to the compact extension.
The earlier fixed-page representation, model-specific ~19x packing reduction, combined TP=2 ~38x result, and large/repeated CUDA batch-copy failure remain as described in the original comment and [#49276](https://github.com/vllm-project/vllm/issues/49276). This follow-up changes replay ownership/completion semantics and refreshes the compact consumer onto current owners; it does not add another tiering manager or registration path.
Proposed review shape remains owner-aligned rather than one monolithic PR: fixed-page manager/replay ownership, mapping/scatter consumer, and scheduler admission/completion lifecycle.
AI assistance was used for implementation support, test development, adversarial review, and benchmark analysis. I reviewed the changed production source and own the contribution and evidence.
[Sign up for free](https://github.com/signup?return_to=https://github.com/vllm-project/vllm/issues/38260)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/vllm-project/vllm/issues/38260)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
