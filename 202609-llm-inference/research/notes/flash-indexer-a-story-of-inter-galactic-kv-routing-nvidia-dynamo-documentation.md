---
title: 'Flash Indexer: A Story of Inter-Galactic KV Routing | NVIDIA Dynamo Documentation'
id: flash-indexer-a-story-of-inter-galactic-kv-routing-nvidia-dynamo-documentation
tags:
- kv-cache-routing
- kv-event-sync
- nvidia-dynamo
- radix-tree
- kv-cache-routing-design
- prefix-cache-indexer
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:54:08.594885Z'
updated: '2026-09-15T04:55:24.062571Z'
source: https://docs.nvidia.com/dynamo/v1.1.0/blog/flash-indexer
source_domain: docs.nvidia.com
fetched_at: '2026-09-14T11:54:08.573278Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'NVIDIA Dynamo engineering blog documenting the design evolution of the Flash
  Indexer, the default global KV-block index shipped in Dynamo v1.0.0, which sustains
  over 100 million operations/second and is presented as an alternative KV-event-synchronization
  architecture to compare against llm-d''s own routing/indexer design. Every cached
  block carries a local (chunk) hash (position-independent, content-only) and a sequence
  hash (rolling hash over the full prefix, position-dependent) plus a worker ID; a
  sidecar publisher per engine emits explicit Store(worker_id, local_hash, seq_hash)
  and Remove(worker_id, seq_hash) events over pub/sub because eviction policies (LRU
  sweeps, memory pressure, preemption) are opaque and cannot be inferred from request/response
  cycles alone. The design went through six iterations: (1) a Python nested dict,
  O(W*D) per query; (2) a single-threaded Rust actor eliminating interpreter overhead
  but serializing all reads behind writes; (3) an inverted index (local_hash -> seq_hash
  -> worker set) cutting query cost to O(D+W) but reintroducing rolling-hash computation
  on reads and expensive full-index scans on removes; (4) a radix tree keyed by parent-child
  local-hash relationships (requiring a new parent-hash field in KV events) that scopes
  hash-collision risk to shared-prefix siblings only; (5) a concurrent radix tree
  using Arc<RwLock> plus DashMap-sharded per-worker lookup tables and sticky per-worker-ID
  thread routing for writes, removing the actor bottleneck for reads (parking_lot::RwLock
  cited as 2-3x faster than std due to no syscall on uncontended paths); (6) the Flash
  Indexer itself, a Vec<DashMap> positional index enabling O(1) random access to any
  prefix position and a ''jump search'' that skips ahead by a tunable jump_size (default
  64) and only backtracks to find exact worker drain points on a partial-cardinality
  mismatch, giving amortized O(D/J + W) query cost. Benchmarked on a 24-core Arrow
  Lake desktop replaying the public Mooncake FAST''25 production trace (16,384 mock
  GPU blocks, 24 concurrent event-processing threads), the Flash Indexer sustains
  170M ops/s -- 42x faster than the radix tree shipped in Dynamo v0.1.0 (4M ops/s)
  and 440x faster than the naive nested-dict implementation (385K ops/s).'
utility_score: 12.0
---

For AI agents: a documentation index is available at the root level at /llms.txt. Append /llms.txt to any URL for a page-level index, or .md for the markdown version of any page.
The **Flash Indexer** is a concurrent global index of every cached KV block across every inference worker, sustaining over **100 million operations per second**. It evolved through six iterations—from a Python dictionary to a jump-optimized spatial index—to the point where network latency, tokenization, and hashing are the bottlenecks. We’re shipping it as the default indexer in Dynamo v1.0.0.
For scale intuition: at 100M+ index ops/sec, the system can support approximately N≈108/rN \approx 10^8 / r concurrent workloads, where is the workload’s sustained index ops/sec (inserts + lookups) under real traffic, including bursty prefill, well beyond current planetary-scale inference demand.
This post walks through those iterations—how each redesign drove a new order-of-magnitude improvement, and the specific data structure or concurrency breakthrough behind it.
## 1. Background
### 1.1 KV Block Identity
Every cached block carries three identifiers:
  * **Local block hash** (`u64`): Content hash of the tokens within a single block. Position-independent—two blocks with the same tokens produce the same hash. Both the frontend and publisher use the same algorithm.
  * **Sequence block hash** (`u64`): Rolling hash of the entire prefix up to this block. Position-dependent—identical tokens at different positions produce different hashes.



```



seq_hash[0] = local_hash[0]


seq_hash[i] = hash(seq_hash[i-1] || local_hash[i])



```

  * **Worker ID** : Which worker holds the block.


Local hashes are deliberately _chunk hashes_ (no prefix context) so frontends can hash query blocks cheaply in parallel. The tradeoff: chunk hashes can’t distinguish position. _“Predict the next token | Learn from the error | Predict the next token.”_ produces identical hashes at blocks 0 and 2. This collision problem drives every data structure decision below.
### 1.2 Events and Requests
The indexer handles two kinds of traffic:
**KV Events** (writes): A publisher sitting alongside each engine emits `Store(worker_id, local_hash, seq_hash)` when a block is cached and `Remove(worker_id, seq_hash)` when evicted. We need explicit events because engines cache blocks beyond request lifetime and their eviction policies (LRU sweeps, memory pressure, preemption) are opaque—there’s no way to infer cache state from request-response cycles alone. The stream is bursty: prefills produce dozens of stores at once; eviction sweeps produce bursts of removes.
Figure 1 — KV Event Density
Per-worker and aggregate KV cache event density heatmap derived from 5% of the Mooncake FAST’25 trace, replayed across 16 Mocker workers with 2,048 GPU blocks each. Green cells indicate Store-dominant time bins (prefill bursts); amber cells indicate Remove-dominant bins (eviction sweeps). The diverging colorscale is clamped at ±10 events per worker and ±100 events aggregate, highlighting the bursty, temporally correlated nature of KV cache traffic that the Flash Indexer must sustain at line rate.
**Requests** (reads): On every inference request, the frontend sends a sequence of chunk hashes `[local_hash_0, ..., local_hash_D]`. The indexer returns `(worker_id, match_depth)` scores so the router can pick the worker with the deepest cached prefix.
Figure 2 — KV Event Flow
Each engine is paired with a publisher that enriches raw KV events with worker identity and block hashes, then broadcasts them via pub/sub. The router requests store lookups from the indexer, which computes prefix overlap scores used for routing decisions.
Both paths are hot. Slow events mean stale routing decisions. Slow queries to the indexer means user-facing latency. The design goal: keep both fast without mutual contention.
## 2. Nested Dictionary → Rust Actor
### 2.1 Python Dictionary
The simplest possible index is a nested dictionary. For each worker, store a mapping from local block hash to the set of external sequence hashes that share that chunk hash. Since local hashes are chunk hashes, the same tokens can appear at different positions in different sequences, and a single local hash can map to multiple sequence hashes on the same worker. To find matches, iterate every worker and walk through the query sequence, checking for hits.

```



class KvIndex:


    # worker_id -> { local_hash -> set of seq_hashes }


    index: dict[int, dict[int, set[int]]] = {}



    def store(self, worker_id: int, blocks: list[tuple[int, int]]):


 worker_id not in self.index:


            self.index[worker_id] = {}


        for local_hash, seq_hash in blocks:


 local_hash not in self.index[worker_id]:


                self.index[worker_id][local_hash] = set()


            self.index[worker_id][local_hash].add(seq_hash)



    def remove(self, worker_id: int, seq_hashes: list[int]):


 worker_id not in self.index:


            return


        for seq_hash in seq_hashes:


 local_hash, hashes in self.index[worker_id].items():


                hashes.discard(seq_hash)



    def find_matches(self, query: list[int]) -> dict[int, int]:


        scores = {}


        for worker_id, blocks in self.index.items():


            depth = 0


 local_hash in query:


 local_hash in blocks and blocks[local_hash]:


                    depth += 1






 depth  0:


                scores[worker_id] = depth


        return scores



```

There is a correctness issue with this approach. `local_hash in blocks` tells us the worker has _some_ block with those tokens, but not _which_ one—different sequences sharing the same chunk hash are conflated. This collision problem shapes every data structure decision that follows. This is `O(W × D)` per query (W workers, D query depth).
With hundreds of workers and sequences thousands of blocks long, it’s a non-starter.
### 2.2 Rust Actor
Porting to Rust (`HashMap<WorkerId, HashMap<LocalHash, HashSet<ExternalHash>>>`) eliminates interpreter overhead. A **single-threaded actor** owns the index exclusively and communicates through channels—correct and lock-free, but serializes all reads behind all writes. The single thread is the throughput ceiling.
## 3. Inverted Index
`worker -> { hash -> ... }` forces `find_matches` to iterate every worker. But the question is _“which workers have this block?”_ —keyed by block, not worker. Instead of iterating workers and checking blocks, build a forward index keyed by LocalHash that maps to the sequence hashes and their worker sets.

```



// local_hash -> (seq_hash -> set of workers)


index: HashMapLocalHash, HashMapExternalHash, HashSetWorkerId>>>



```

Now `find_matches` traverses the query once. At each position, take the union of worker sets. Workers only _drop out_ as you go deeper—each is drained at most once—giving **O(D + W)** instead of O(W × D).
The inverted index is a major win for reads, but every data structure choice is a two-sided tradeoff between query performance and update cost.
On the read side, the collision issue from Section 2.1 resurfaces in a different shape. When we union worker sets across sequence hashes at a given local hash, we conflate workers that cached different sequences sharing the same chunk. The seq hash data is in the index, but `find_matches` cannot use it without computing the query’s own seq hashes—which reintroduces rolling hash computation on the read path, exactly what chunk hashes were designed to avoid.
On the write side, removes are equally expensive: without a per-worker reverse lookup, removing a block by seq hash requires scanning the entire index. We could add a reverse lookup table, but that’s more bookkeeping on every store.
The radix tree resolves both.
## 4. Radix Tree
Each node has a small children map keyed by `LocalHash`, plus a worker set. Parent-child relationships scope collision risk: two blocks with the same chunk hash collide only if they share the same parent, which means the same prefix. Different prefixes lead to different parents. This requires one new field in KV events: the **parent hash** , so the tree can link child to parent as events arrive.
Figure 3 — Prefix Tree Structure
Prefix-aware radix tree indexes cached blocks by local hash. Shared prefixes branch where sequences diverge; each node records which workers hold that block.

```



type SharedRadixBlock = RcRefCellRadixBlock>>;



struct RadixBlock {


    children: HashMapLocalHash, SharedRadixBlock>,


    workers: HashSetWorkerWithDpRank>,


    block_hash: OptionExternalHash>,




struct RadixTree {


    root: SharedRadixBlock,


    lookup: HashMapWorkerWithDpRank, HashMapExternalHash, SharedRadixBlock>>,




```

Each node also carries a sequence hash. A per-worker **lookup table** (`worker -> { seq_hash -> node }`) provides O(1) access for event processing: stores attach children via the parent’s seq hash; removes find the node directly. Two keys for two access patterns—local hash for traversal, sequence hash for events.
Both the tree and the lookup table point to the same nodes via `Rc<RefCell<T>>` (shared ownership with interior mutability, single-threaded). The children maps at each node are small—bounded by branching factor, not total block count.
This approach remains single-threaded behind the actor, with serialized reads and writes.
## 5. Concurrent Radix Tree
Reads don’t conflict with each other. We replace `Rc<RefCell<T>>` with `Arc<RwLock<T>>` (atomic reference counting + reader-writer lock). Now `find_matches` acquires only read locks and executes _inline on the caller’s thread_ —no channel, no actor, no queue.
Writes use **sticky routing** : a `ThreadPoolIndexer` deterministically assigns each `WorkerId` to one thread. Events for the same worker always land on the same thread, so there’s no write-write contention on any worker’s subtree.
Figure 4 — Concurrency Model
Write events are sticky-routed by worker ID to a thread pool, ensuring sequential ordering. A concurrent radix tree with `Arc<RwLock>` allows `find_matches()` reads in parallel, enabling concurrent traversals.

```



type SharedBlock = ArcRwLockBlock>>;



struct ConcurrentRadixTree {


    root: SharedBlock,


    lookup: DashMapWorkerWithDpRank, RwLockHashMapExternalHash, SharedBlock>>>,




```

`DashMap` shards the outer map so reads and writes to different workers don’t touch the same lock. `parking_lot::RwLock` avoids the OS syscall on uncontended paths (2–3x faster than `std::sync::RwLock`). `FxHashMap` replaces SipHash with a single multiply-xor step—safe here because keys are `u64` hashes, not user input.
`parking_lot::RwLock` is task-fair by default: it processes waiters in arrival order rather than unconditionally favoring readers or writers. Combined with sticky routing’s guarantee that each worker’s writes are serialized on a single thread, write contention is minimal and neither reads nor writes are starved.
The actor is gone for reads. Multiple `find_matches` calls proceed concurrently with writes to different workers.
## 6. Positional Indexer with Jump Search
The radix tree traverses node-by-node, following pointers from parent to child—cache-hostile and fundamentally sequential. You can’t check position 128 without visiting 0 through 127.
Replace the tree with a `Vec<DashMap<LocalHash, SeqEntry>>` indexed by position. `index[position]` is a concurrent map from local hash to sequence entry. Any position is O(1)—no traversal required.

```



enum SeqEntry {


    Single(ExternalHash, HashSetWorkerWithDpRank>),


    Multi(HashMapExternalHash, HashSetWorkerWithDpRank>>),




struct PositionalIndexer {


    // index[position] -> { local_hash -> SeqEntry }


    index: VecDashMapLocalHash, SeqEntry>>,


    worker_blocks: DashMapWorkerWithDpRank, LevelIndex>,


    jump_size: usize,




```

The `SeqEntry` enum handles collisions: in the common case a `(position, local_hash)` slot has exactly one sequence hash, stored inline without a `HashMap` allocation. Only when multiple prefixes produce the same chunk hash at the same position does it upgrade to `Multi`.
The `Single`/`Multi` split also enables lazy hash computation: when a lookup finds a `Single` entry, the match is unambiguous without computing the query’s sequence hash. The expensive rolling hash is only needed on the rare `Multi` entries where chunk hash collisions require disambiguation.
But the positional indexer’s biggest advantage isn’t the data layout – it’s what **random access makes possible.**
Random access enables **jump search** :
  1. Initialize the active worker set from position 0.
  2. Jump ahead by `jump_size` positions (e.g., 64) to the next checkpoint.
  3. At the checkpoint, count how many active workers still match (cardinality check—no set cloning needed).
  4. If all match: the entire skipped range is confirmed. Continue jumping.
  5. If fewer match: some workers drained in the skipped range. Scan forward through positions `[previous_checkpoint + 1 .. current_checkpoint]` to find each lost worker’s exact drain point.
  6. Resume jumping from the current checkpoint.

Figure 5 — Positional Jump Search
With position as a first-class key, the indexer jumps ahead by a fixed stride. On a partial match, a lookback from the previous checkpoint identifies exact drain points, then resumes jumping from the current checkpoint.
Best case: `D / J` lookups instead of `D`.
Worst case (workers drop at every jump): degrades to a linear scan with jump overhead.
The positional indexer wins on long sequences with high prefix sharing; the radix tree wins on short or highly-divergent sequences.
The `Vec` layout also improves cache locality: early positions (shared system prompts, common preambles) are the hot path, cluster at the front of the array, and stay warm in cache.
With jump size _J_ (= `jump_size`, defaulting to 64), amortized cost drops to **O(D/J + W)**. Since _J_ is a tunable constant, the complexity remains linear in _D_ ; the practical benefit is skipping the vast majority of positions when prefix sharing is high.
## 7. Benchmarks
All benchmarks run on a 24-core Arrow Lake (285K) desktop, replaying publicly-available [Mooncake production traces](https://github.com/kvcache-ai/Mooncake/tree/main/FAST25-release/arxiv-trace) through a mock engine with 16,384 GPU blocks and prefix caching enabled. The harness tests all five backends with 24 concurrent event-processing threads.
**Ops throughput** is the combined rate of KV events and `find_matches` requests per second. We sweep offered load by compressing the same trace into shorter durations and compare achieved vs. offered throughput. The **threshold throughput** is where achieved throughput stops tracking offered—the indexer’s saturation point.
Figure 6 — Indexer Performance
Achieved vs. offered block throughput across five indexer backends, measured with `mooncake_bench` on real trace data. The Flash Indexer sustains 170M ops/s — 42x faster than the Radix Tree shipped in Dynamo v0.1.0 (4M ops/s) and 440x faster than the naive implementations (385K ops/s).
## 8. Future Directions
With the Flash Indexer shipping in Dynamo v1.0.0, the next round of optimizations targets the remaining constant factors:
  * **Binary search within jumps.** Replace the linear scan-back after a failed jump with binary search: `O(log J)` instead of `O(J)` per failed jump.
  * **Hierarchical routing.** A sparse top-level indexer for coarse-grained prefix coverage across deployment groups, with full indexers at the leaves.
  * **Inline bitsets for worker sets.** Replace `HashSet` with fixed-width bitsets stored inline in each node, turning membership tests into single bit operations and eliminating pointer chases.


## 9. Conclusion
The journey from a Python dictionary to the Flash Indexer spans six iterations, each motivated by a concrete bottleneck in the previous design:
  1. **Naive Nested Dict** — simple but O(W × D) per query.
  2. **Rust + Actor Pattern** — fast language, correct concurrency, but single-threaded bottleneck.
  3. **Inverted Index** — O(D + W) per query by flipping the key structure; secondary `seq_hash` layer for chunk-hash collision safety.
  4. **Radix Tree** — tree structure replaces giant flat map; per-node children maps stay small; dual-key design (local hash for traversal, seq hash for event processing); `Rc<RefCell<>>` for single-threaded shared ownership.
  5. **Concurrent Radix Tree** — `Arc<parking_lot::RwLock<>>` replaces `Rc<RefCell<>>`; `DashMap` with per-worker inner `RwLock` for the lookup table (shard-level locking for rare mutations, cheap shared reads on the hot path); reads bypass the actor entirely; sticky routing serializes writes per worker with zero contention.
  6. **Concurrent Positional Indexer via Jump Search (Flash Indexer)** — an alternative to the radix tree for long-sequence workloads; `Vec<DashMap<>>` indexed by position replaces pointer chasing with O(1) random access, enabling jump search that skips most of the depth; `DashMap` with per-worker inner `RwLock` for the reverse lookup; hot prefix positions cluster at the front of the `Vec` and stay warm in cache.


The result: a sustained ops throughput of **170 million operations per second** —events and requests combined—with achieved throughput tracking offered throughput all the way to the limit.
Ask AI
Assistant
Responses are generated using AI and may contain mistakes.
Hi, I'm an AI assistant with access to documentation and other content.
Tip: You can toggle this pane with
+
NVIDIA uses cookies to improve your experience on our web site. We and our third-party partners also use cookies and other tools to collect and record information you provide as well as information about your interactions with our websites for performance improvement, analytics, and to assist in marketing efforts. By clicking "Accept All", you consent to our use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/). You can manage your cookie settings by clicking on "Manage Settings." By continuing to use this site or by clicking one of the buttons below, you agree to our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control (GPC) signal and recorded your rejection of all optional cookies on this site for this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. To opt out of non-cookie personal information "sales" / "sharing" for targeted advertising purposes, please visit the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies. You can manage these settings in the NVIDIA [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies which overrides at least one of your previous settings. You can manage them in the [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
Manage Settings
Reject Optional Accept All
Cookie Settings
We and our third-party partners (including social media, advertising, and analytics partners) use cookies and other tracking technologies to collect, store, monitor, and process certain information about you when you visit our website. The information collected might relate to you, your preferences, or your device. We use that information to make the site work, analyze performance and traffic on our website, provide a more personalized web experience, and assist in our marketing efforts.Under certain privacy laws, you have the right to direct us not to "sell" or "share" your personal information for targeted advertising. To opt-out of the "sale" and "sharing" of personal information through cookies, you must opt-out of optional cookies using the toggles below. To opt out of the "sale" and "sharing" of data collected by other means (e.g., online forms) you must also update your data sharing preferences through the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/).Click on the different category headings below to find out more and change the settings according to your preference. You cannot opt out of Required Cookies as they are deployed to ensure the proper functioning of our website (such as prompting the cookie banner and remembering your settings, etc.). By clicking "Save and Accept" or "Decline All" at the bottom, you consent to the use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) in accordance with your settings and accept our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). For more information about our privacy practices, please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/).
Required Cookies
Always Active
These cookies enable core functionality such as security, network management, and accessibility. These cookies are required for the site to function and cannot be turned off.
Cookies Details
Performance Cookies
Performance Cookies
These cookies are used to provide quantitative measures of our website visitors, such as the number of times you visit, time on page, your mouse movements, scrolling, clicks and keystroke activity on the websites; other browsing, search, or product research behavior; and what brought you to our site. These cookies may store a unique ID so that our system will remember you when you return. Information collected with these cookies is used to measure and find ways to improve website performance.
Cookies Details
Personalization Cookies
Personalization Cookies
These cookies collect data about how you have interacted with our website to help us improve your web experience, such as which pages you have visited. These cookies may store a unique ID so that our system will remember you when you return. They may be set by us or by third party providers whose services we have added to our pages. These cookies enable us to provide enhanced website functionality and personalization as well as make the marketing messages we send to you more relevant to your interests. If you do not allow these cookies, then some or all of these services may not function properly.
Cookies Details
Advertising Cookies
Advertising Cookies
These cookies record your visit to our websites, the pages you have visited and the links you have followed to influence the advertisements that you see on other websites. These cookies and the information they collect may be managed by other companies, including our advertising partners, and may be used to build a profile of your interests and show you relevant advertising on other sites. We and our advertising partners will use this information to make our websites and the advertising displayed on it, more relevant to your interests.
Cookies Details
Back Button
Cookie List
Search Icon
Filter Icon
Clear
  * checkbox label label


Apply Cancel
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Decline All Save and Accept
