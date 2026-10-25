---
title: 'Source Analysis — RFC Multi-tier KV Offloading (vLLM Issue #38260)'
id: source-analysis-rfc-multi-tier-kv-offloading-vllm-issue-38260
tags:
- llmd-kv-cache-routing-c665d5
- source-analysis
created: '2026-09-14T12:09:45.737190Z'
updated: '2026-09-15T04:55:24.152253Z'
status: evergreen
type: source-analysis
deprecated: false
summary: 'Digest of vLLM RFC #38260 (multi-tier KV offloading via TieringManager:
  CPU primary tier + secondary tiers, canonical TP1 layout) and its comment thread,
  which is the primary record of how tiered/offloaded KV blocks get exposed via KV
  events (medium vs. locality) for external routers like llm-d, including a maintainer''s
  explicit warning that stale placement events cause routers to score/route incorrectly.
  Flags a block of self-reported, unverified ''production'' benchmark claims from
  a single downstream contributor (unmerged fork) that must not be cited as vLLM fact.'
---

# Source Analysis — [RFC]: Multi-tier KV offloading via the vLLM offloading connector · Issue #38260 · vllm-project/vllm

**Original source:** [[rfc-multi-tier-kv-offloading-via-the-vllm-offloading-connector-issue-38260-vllm]]
**Source type:** GitHub issue thread (RFC + maintainer/contributor discussion)
**Source word count:** 6940
**Your judgment:** Ground-truth design source for how vLLM's KV offloading moves beyond single-tier CPU offload into a multi-tier hierarchy, and — more directly relevant to the research_query — the primary record of the ongoing negotiation over how tiered/offloaded KV state gets exposed as KV events (`medium`, `locality`) for external routers like llm-d to consume. Contains a large block of unverified, self-reported "production" benchmark claims from a single downstream contributor that must not be cited as vetted vLLM behavior.

*Suggested by [[rfc-multi-tier-kv-offloading-via-the-vllm-offloading-connector-issue-38260-vllm]] — source analyst's digest of the full source body*

## Thesis / Central claim
vLLM's native KV offloading originally supported only GPU→CPU offload; this RFC (opened by dannyharnik, Mar 26, 2026) proposes a `TieringManager` that generalizes offload into a primary tier (CPU DRAM, sole gateway to GPU memory) plus arbitrary secondary tiers (storage, PD-disaggregation, P2P), with CPU-resident KV stored in a canonical TP-rank-1 layout so it can be shared/transferred across nodes regardless of a given node's actual tensor-parallel rank. The subsequent comment thread (through Jul 2026) is the primary evidence trail for how the shipped implementation exposes this tiering to external consumers via KV events, and is the more load-bearing part of this source for the research_query: it is where vLLM maintainers explicitly reason about what a router like llm-d needs to know about *where* a cached block lives.

## Methodology / Basis of claims
The RFC itself is a design document with an interface sketch (`SecondaryTierManager` ABC, `JobMetadata` dataclass) and two flow diagrams (cascade store, promotion load) — no benchmarks, no empirical claims. The comment thread is GitHub-native asynchronous design negotiation among named vLLM maintainers/contributors (orozery, Change72 — both identified as collaborator/contributor roles on the repo) tracking the RFC's evolution into merged PRs. This portion is credible as a live engineering record because it links directly to specific PR numbers and quotes exact code symbols (`MEDIUM_GPU`, `medium()`, `complete_store`). A separate portion of the thread (four long comments from contributor "coltonottley", Jul 18–29, 2026) presents extremely detailed self-reported "production" and "hardware qualification" benchmark numbers for an *unmerged downstream fork* implementing canonical TP1 CPU-primary storage. These claims are single-source, self-reported, explicitly AI-assisted in preparation ("AI assistance was used for implementation support, adversarial review, and benchmark analysis"), and not peer-reviewed or merged into vLLM proper — they must be treated as unverified vendor/contributor claims, not established vLLM behavior, regardless of how precise the figures look.

## Key findings / Claims (with specific numbers where present)

1. **Two-tier architecture.** A single Primary Tier (CPU DRAM, exclusive GPU access) plus one or more Secondary Tiers (storage, PD-disaggregation remote nodes, P2P), each implementing a `SecondaryTierManager` interface: `lookup(block_hashes) -> int | None`, `submit_store(job_metadata)`, `submit_load(job_metadata)`, `get_finished()`, `touch(block_hashes)`.

2. **Five stated design principles** (RFC body, verbatim intent preserved): (1) always cascade a confirmed primary-tier block to every secondary tier asynchronously; (2) primary tier is the sole gateway to GPU memory — secondary tiers never touch GPU directly; (3) staged promotion — `lookup()` returns `None` while a secondary-tier hit is being promoted to primary, and the scheduler retries; (4) all `SecondaryTierManager` methods run non-blocking in the scheduler process (only GPU↔CPU data movement happens on the worker thread); (5) each secondary tier owns its own eviction policy.

3. **Canonical TP1 layout mandate.** Regardless of a vLLM node's actual tensor-parallel rank, CPU-resident KV must be stored in canonical TP-rank-1 form, dividing memory by attention-head count into contiguous regions mappable to any TP rank. This is stated as the foundation for cross-TP-rank sharing and storage, and depends on cross-layer KV block layout work (RFC #27742 / PR #27743).

4. **What shipped vs. proposed.** Per Change72 (Jun 22, 2026): the multi-tier data plane landed via PR #40020 (framework) and PR #41968 (object-store tier). But at that point the *external KV-event contract* for secondary tiers was explicitly unresolved — "an external KV-event consumer cannot reliably discover which blocks live below the primary CPU tier." PR #43468 added self-describing `BlockStored`/`BlockRemoved` events but *explicitly rejects* `self_describing_kv_events` for `TieringOffloadingSpec` (multi-tier), because tier promotions can generate primary-tier store events with no live GPU store job to describe them from — this only works for the single-tier `CPUOffloadingSpec`.

5. **Medium naming for external routers.** PR #46544 added stable wire-level medium constants `MEDIUM_CPU` / `MEDIUM_FS` / `MEDIUM_OBJ` alongside the pre-existing `MEDIUM_GPU` in `kv_events.py`, with `CPULoadStoreSpec`/`GPULoadStoreSpec` using them; custom (non-built-in) tiers fall back to their registered `tier_type` string. Change72 states explicitly: "llm-d can mostly treat this as an opaque string, but Dynamo maps `medium` into fixed tiers like `HostPinned`, `Disk`, and `External`" — i.e., llm-d and Dynamo consume the same `medium` field with different levels of interpretive rigor.

6. **What actually merged, and a correction.** Change72's Jul 7, 2026 update: PR #46544 merged with a **narrower scope than the Jun 22–24 comments described**. It establishes tier-owned KV event handling (each tier owns its own events; `TieringOffloadingManager` only aggregates), but as merged it "does not yet emit actual secondary-tier events or add `MEDIUM_FS` / `MEDIUM_OBJ`" — directly contradicting the Jun 24 comment claiming those constants had been added. Change72 flags this himself: "my earlier comments above described an earlier revision." This is an important internal correction: readers should trust the Jul 7 comment over the Jun 22–24 comments for what is actually in vLLM main as of that date.

7. **Eviction/invalidation is a routing-correctness requirement, stated by a vLLM contributor.** orozery (Jun 23) argues that for shared/remote storage tiers, eviction should be managed *outside* vLLM by the storage/control plane, so vLLM need not emit `BlockRemoved` for those tiers. Change72 (Jun 23) agrees but adds the load-bearing correctness claim directly relevant to the research_query: **"whoever owns eviction should also provide invalidation. Otherwise, routers may score or route based on stale placement state."** This is the clearest statement in the source of *why* KV-event staleness matters for router correctness.

8. **Chunk-granularity ambiguity flagged, unresolved contract.** Change72 raises that a hash-only event (`hash + medium`) cannot tell a router whether a hash represents a normal block or a larger chunk/offload key from a lower tier with different block-size granularity, and contrasts vLLM's approach (per-disk-root config file carrying `hash_block_size`, `gpu_blocks_per_file`, `tp_size`, `pp_size`, `pcp_size`, `dcp_size`, `dtype`, `kv_cache_groups`, `inference_engine` — file: `vllm/v1/kv_offload/file_mapper.py`) with Dynamo-style prefix indexing needing `parent_block_hash` and `token_ids`. orozery proposes a possible runtime config endpoint for CPU tier block-size discovery (referencing separate issue #38147). This exchange shows the router-facing metadata contract as still unsettled as of the thread's latest activity (Jul 2026).

9. **Tier locality distinguished from medium (PR #48281, Jul 14, 2026).** Change72: **"medium says what the storage is; locality says where it sits relative to the publishing instance."** Worked example given verbatim: `A publishes (CPU, LOCAL)` [valid P2P source]; `B fetches from A; block lands in B's CPU tier`; `B publishes (CPU, LOCAL)` [now both A and B are valid sources]; `C publishes (FS, REMOTE)` [shared FS reachable directly by peers]. There is deliberately no "P2P" medium value — "P2P is a transport, not a placement." This is the most directly relevant single finding for the research_query's routing-based-on-cache-location angle: it shows the vLLM-side event model separating *what kind of storage* from *whose storage, reachable how*, which is exactly the distinction an external cache-aware router needs.

10. **PD/P2P connector relationship.** The RFC frames a CPU-CPU offload path as an alternative to the existing GPU-to-GPU "NIXL connector" for PD disaggregation, citing benefits: faster GPU-memory release on the P node, shorter GPU-memory hold time on the D node, simpler cross-TP handling (single canonical buffer vs. GPU-to-GPU grid), and potentially faster P→D transfer. PR #48021 (referenced, not detailed in-thread) covers P2P lookup and transfer; Change72 states plans to work on "the P2P event path, especially tier reachability and routing metadata" next.

11. **[UNVERIFIED — single self-reported commenter, unmerged fork, do not cite as vLLM fact]** coltonottley (Jul 18–29, 2026) reports a downstream, unmerged fork implementing canonical TP1 CPU-primary storage with claims including: 126/126 byte-identical TP-rank payload checksums; a 52,173-token replay test restoring 51,968 external tokens with "206 recomputed tokens" in one comment and "205 boundary tokens recomputed" in a later comment describing the same nominal test (an internal inconsistency in the commenter's own numbers); 431,169,920 aggregate H2D bytes; TP=2 vs TP=4 capacity/decode-throughput figures (e.g., "192.07 tok/s decode," "220.31 tok/s decode," prefill penalties of 0.613%/0.537%/0.592%/0.029%); a claimed "~19x" CPU-byte packing reduction described by the commenter as "model/layout-specific, not a general transformer claim," and a combined "~38x" reduction. These numbers are self-reported by one GitHub contributor for an unmerged branch (`coltonottley/vllm:feature/native-compact-cpu-kv-current-main[-v2]`), explicitly AI-assisted in preparation, and not independently verified, peer-reviewed, or merged. Report them, if at all, only as "a contributor claims X" — never as vLLM's measured behavior.

12. **Unresolved thread item.** kreeuwijk (Jul 28) asks whether a disk usage limit can be added for the `fs` secondary tier (comparing to other KV cache solutions that cap disk consumption); this question is unanswered within the visible thread — a known open gap in the FS secondary tier design as of the source's last recorded activity.

## Load-bearing citations / sources this source depends on
- PR #27743 / RFC #27742 ("KV cache layout combining all layers per block") — underlies the canonical CPU layout / coalesced DMA copy rationale.
- vllm.ai blog post "kv-offloading-connector" (referenced, not independently fetched here) — background on the existing single-tier offloading connector this RFC extends.
- PR #40020 (multi-tier data plane framework), PR #41968 (object-store secondary tier) — the shipped code this RFC's design turned into.
- PR #43468 (self-describing KV events, single-tier `CPUOffloadingSpec` only) — establishes the precedent contract that PR #46544 partially extends to multi-tier.
- PR #46544 (opt-in hash-only secondary-tier `BlockStored` events, tier-owned event aggregation) — the actual shipped KV-event mechanism for secondary tiers as of Jul 2026; note the scope walked back from earlier comments in the same thread.
- PR #47923 and its review discussion (source of the tier-locality follow-up), PR #48021 (P2P lookup/transfer), PR #48281 (tier locality: medium vs. locality) — the concrete router-facing metadata this source's Key Findings #9 draws from.
- Issue #38147 (runtime config endpoint proposal) — referenced as a possible mechanism for CPU/disk block-size discovery by routers.
- `vllm/v1/kv_offload/file_mapper.py` (specific file/lines cited) — the existing per-disk-root config mechanism for FS-tier block-size/layout metadata.
- Downstream/unmerged, UNVERIFIED: PR #48906, PR #48408, PR #48911, PR #49052 (fixing issue #48959), issue #48966, PR #50069 (this one framed as accepted as a generic current-main fix, separate from the unmerged compact fork), PR #50070, issue #49276 — all tied to coltonottley's self-reported fork; flag if the orchestrator wants to chase any of these, since PR #50069/#50070 in particular are described as merged generic fixes (extracted from the unverified fork) and may be worth separately verifying via `note show`/fetch rather than trusting this thread's framing.

## Caveats, limitations, contradictions
- The source is a live GitHub issue thread, not a finished spec — several contract questions (chunk granularity vs. block-size discovery, `BlockRemoved` for secondary tiers, full self-describing payload across tiers) are explicitly described by the participants as unresolved as of the last comment (Jul 29, 2026). Anything drawn from this source about "the" KV-event contract for tiered caches should be dated and treated as a snapshot in an ongoing negotiation, not a finalized spec.
- **Direct self-contradiction within the thread itself**: Change72's Jun 24 comment states `MEDIUM_FS`/`MEDIUM_OBJ` were added in PR #46544; the Jul 7 comment from the same author states the merged PR "does not yet emit actual secondary-tier events or add `MEDIUM_FS` / `MEDIUM_OBJ`," explicitly noting the earlier comment described "an earlier revision." Any downstream reporting must prefer the later, corrected claim.
- **Discrepancy in the unverified benchmark comments**: the same "52,173-token" test is reported with "206 recomputed tokens" in one comment and "205 boundary tokens recomputed" in a subsequent comment purporting to describe the same test — a small but real inconsistency that undercuts confidence in the self-reported figures generally, beyond the more fundamental problem that they are unverified and unmerged.
- The RFC's own "What we don't intend to support" section rules out direct GPU access from secondary tiers (no GPU-storage or GPU-GPU secondary-tier communication) — a scoping limitation later contested in the thread (ruihong123 asks about relaxing it for GPUDirect Storage / GDS; orozery and dannyharnik respond that GDS would be a separate connector composed via `MultiConnector`, not a relaxation of the primary-tier-as-gateway principle).
- The coltonottley benchmark comments, while extraordinarily detailed, are single-source, self-reported, describe an unmerged fork with acknowledged "diagnostic and repair history against an older pre-#48150 base" that the commenter says "publishing it as a cherry-pick series would be misleading" — i.e., even the commenter does not claim the fork is a clean, mergeable, or independently reproducible artifact.

## Relevance to research_query
This source is directly relevant to "how llm-d manages KV cache and routes based on the cache" but from the vLLM (data-plane / producer) side rather than the llm-d (consumer / router) side: it is the primary record of what information vLLM's KV-event stream can and cannot express about tiered/offloaded cache, which is exactly the input llm-d's cache-aware routing would need to consume. The single most load-bearing finding for the query is the `medium`/`locality` split (Key Finding #9): a router needs to know both what kind of storage a block sits in (CPU/FS/OBJ) and where that storage is relative to the publishing instance (LOCAL/REMOTE) to make a cache-affinity routing decision — and this source shows that distinction was only formalized in PR #48281 (Jul 14, 2026), i.e., recently and incompletely. Finding #7 (eviction owner must provide invalidation, or "routers may score or route based on stale placement state") is a directly quotable, maintainer-stated correctness hazard for any router (including llm-d) that scores placement using these events. Finding #5/#6 establishes that llm-d specifically is named in this thread as a KV-event consumer that treats `medium` as an opaque string (contrasted with Dynamo's stricter fixed-tier mapping) — useful evidence that llm-d's current cache-aware routing logic is architecturally loosely coupled to vLLM's medium taxonomy, though this source does not itself describe llm-d's internal FS-backend implementation; that detail belongs to llm-d-side sources (e.g., llm-d-kv-cache-manager notes) and should be cross-checked there rather than inferred from this thread. The unverified benchmark comments (Finding #11) are NOT relevant as performance evidence for the research_query and should be excluded from any quantitative claims in the draft; at most they could be mentioned as "a downstream contributor claims large CPU-tier capacity/throughput gains from canonical TP1 storage, unverified and unmerged" if the orchestrator wants color on community interest in the design.

## Extracted quotes

> "whoever owns eviction should also provide invalidation. Otherwise, routers may score or route based on stale placement state."

Change72 (vLLM contributor), Jun 23, 2026 — the clearest single statement in the source of why KV-event staleness is a routing-correctness hazard.

> "medium says what the storage is; locality says where it sits relative to the publishing instance."

Change72, Jul 14, 2026, introducing PR #48281's tier-locality model — the core router-facing metadata distinction this source contributes.

> "llm-d can mostly treat this as an opaque string, but Dynamo maps `medium` into fixed tiers like `HostPinned`, `Disk`, and `External`."

Change72, Jun 23, 2026 — direct comparison of how llm-d vs. Dynamo consume the `medium` field.

> "[#43468] intentionally rejects `self_describing_kv_events` under `TieringOffloadingSpec`. The reason is that tier promotions can create primary-tier store events that do not correspond to a live GPU store job, so the current request-time metadata tracker cannot describe them."

Change72, Jun 22, 2026 — explains why the richer, self-describing KV-event path is not (yet) available for tiered/multi-level caches, only for single-tier CPU offload.

> "It does not yet emit actual secondary-tier events or add `MEDIUM_FS` / `MEDIUM_OBJ`; my earlier comments above described an earlier revision."

Change72, Jul 7, 2026 — self-correction narrowing the scope of what actually shipped in PR #46544; supersedes the Jun 24 comment describing those constants as added.

> "A publishes (CPU, LOCAL) # A is a valid P2P source / B fetches from A; the block lands in B's CPU tier / B publishes (CPU, LOCAL) # A and B are both valid sources now / C publishes (FS, REMOTE) # shared FS (NFS/Lustre); peers reach it directly ... This is also why there is no P2P medium — P2P is a transport, not a placement."

Change72, Jul 14, 2026 — worked example of the medium/locality event model for cross-node cache placement.

> "AI assistance was used for implementation support, adversarial review, and benchmark analysis. The human submitter owns the evidence and contribution decisions."

coltonottley, Jul 18, 2026 — disclosure attached to the self-reported benchmark comments; quoted here as evidence for why these numbers must be labeled unverified rather than as an endorsement of their content.
