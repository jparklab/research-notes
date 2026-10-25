---
title: '[TRTLLM-15331][feat] Improve KV event publish performance via StreamingEvent
  by tanmayv25 · Pull Request #17023 · NVIDIA/TensorRT-LLM · GitHub'
id: trtllm-15331feat-improve-kv-event-publish-performance-via-streamingevent-by-tanm
tags:
- llmd-kv-cache-routing-c665d5
- locus-approximate-vs-precise-index
- tensorrt-llm
- zmq-kv-events
created: '2026-09-14T12:44:45.673171Z'
updated: '2026-09-14T12:46:21.953118Z'
source: https://github.com/NVIDIA/TensorRT-LLM/pull/17023
source_domain: github.com
fetched_at: '2026-09-14T12:44:45.651316Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE: NVIDIA/TensorRT-LLM PR #17023 (TRTLLM-15331, author tanmayv25,
  merged by allisonlim-nv into NVIDIA:main at commit 05293ce on Sep 10, 2026) -- direct,
  dated evidence that TensorRT-LLM implements a ZMQ-based KV-cache event publisher
  compatible in spirit with vLLM/llm-d''s event schema, as production (if prototype-flagged)
  code, not aspirational roadmap.\n\nMotivation quoted verbatim: ''External KV-cache-aware
  routers (e.g. Dynamo) subscribe to a stream of block stored/removed events to route
  requests to the engine that already holds a prefix.'' Notably credits NVIDIA Dynamo
  as the named external consumer -- llm-d is not mentioned anywhere in the PR.\n\nPre-existing
  state: TRT-LLM already had a buffered/pull-based KV event path (LLM.get_kv_cache_events(),
  built on Python KVCacheEvent objects, all-gathered onto rank 0 under attention DP)
  before this PR -- so KV-cache eventing already existed; this PR adds a new opt-in
  STREAMING push path.\n\nNew components added: StreamingKVCacheEventManager (new
  tensorrt_llm/_torch/pyexecutor/kv_cache_events.py) builds wire-format msgspec structs
  named BlockStored / BlockRemoved / AllBlocksCleared -- the same three event-type
  names used by vLLM and SGLang -- reusing ''the low 64 bits of the radix block key
  as the wire hash (no re-hash)'' and coalescing consecutive blocks into one event.
  ZmqEventPublisher msgpack-encodes each per-iteration batch and publishes over a
  ZeroMQ PUB socket with 3 frames (topic, seq, payload); each attention-DP rank binds
  base_port + rank; an optional ROUTER replay socket is supported. New config: KVEventsConfig
  nested as kv_cache_config.kv_events_config, explicitly marked prototype. Scope guards
  limit it to KV cache manager V2 (PyTorch backend) only, excluded for draft models
  and KV-cache-size estimation, and raises under pipeline or context parallelism.
  Off by default; the older buffered/pull path is unchanged and remains the default.\n\nImplication
  for llm-d: TensorRT-LLM''s event schema (BlockStored/BlockRemoved/AllBlocksCleared
  over ZMQ, msgpack-encoded) is structurally compatible with what llm-d''s kvevents.EngineAdapter
  interface expects, and this is real merged code as of Sep 10 2026 -- so the llm-d.ai
  marketing claim that TensorRT-LLM ''publishes every cache change as an event'' is
  technically accurate about TRT-LLM''s own capability. However this PR gives zero
  evidence of an llm-d-side TRTLLMAdapter or wire-format parser; combined with the
  llm-d-kv-cache architecture.md doc (which lists only VLLMAdapter and SGLangAdapter)
  and the deployment guide (which ships only vllm/sglang MODEL_SERVER directories),
  the gap is on llm-d''s consumption side, not TensorRT-LLM''s production side.'
---

[Skip to content](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/NVIDIA/TensorRT-LLM/pull/17023) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/NVIDIA/TensorRT-LLM/pull/17023) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/NVIDIA/TensorRT-LLM/pull/17023) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2FNVIDIA%2FTensorRT-LLM) You must be signed in to change notification settings
  * [ Fork 2.7k ](https://github.com/login?return_to=%2FNVIDIA%2FTensorRT-LLM)
  * [ Star  14.6k ](https://github.com/login?return_to=%2FNVIDIA%2FTensorRT-LLM)


## Conversation
### 
commented [Jul 29, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issue-5013362120) •
edited by coderabbitai Bot
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Copy link  Copy Markdown 
Collaborator
## Motivation
External KV-cache-aware routers (e.g. Dynamo) subscribe to a stream of block **stored / removed** events to route requests to the engine that already holds a prefix. The existing path builds Python `KVCacheEvent` objects, buffers them, all-gathers them onto rank 0 under attention DP, and exposes them through a per-iteration pull API (`LLM.get_kv_cache_events()`).
This PR adds an opt-in path where **each rank publishes its own events directly over ZeroMQ** , reusing the V2 radix block hashes it already computed instead of re-deriving events and gathering them.
## What this changes
Adds an opt-in **streaming (push-based)** KV-event path for **KV cache manager V2** (PyTorch backend). Off by default; the buffered gather/poll path is unchanged.
  * **`StreamingKVCacheEventManager`**(new`tensorrt_llm/_torch/pyexecutor/kv_cache_events.py`) implements the V2 event-sink hooks by duck typing. Per stored/removed block it builds wire-format `msgspec` structs (`BlockStored` / `BlockRemoved` / `AllBlocksCleared`), reusing the low 64 bits of the radix block key as the wire hash (no re-hash) and coalescing consecutive blocks into one event.
  * **`ZmqEventPublisher`**msgpack-encodes each per-iteration batch and sends it from a background thread over a ZeroMQ`PUB` socket (3 frames: `topic`, `seq`, `payload`), with an optional `ROUTER` replay socket. Each attention-DP rank binds `base_port + rank`.
  * **Config** : new `KVEventsConfig`, nested as `kv_cache_config.kv_events_config` (marked `prototype`). Scope guards: V2 only (warns on a non-V2 manager); excluded for draft models and KV-cache-size estimation; raises under pipeline or context parallelism.
  * In streaming mode the pull API returns `[]`, so `LLM.get_kv_cache_events()` degrades cleanly instead of raising.


## Before / After
**Before** — buffered build → buffer → gather → per-iteration pull (still the default; unchanged by this PR):
Loading

```
flowchart LR
  H["V2 radix tree<br/>store / remove hooks"] --> EM["KVCacheEventManager<br/>builds KVCacheEvent objects"]
  EM --> B["per-rank buffer<br/>(event_buffer_max_size)"]
  B -->|"attention DP:<br/>all-gather onto rank 0"| G["rank-0 buffer"]
  G -->|"scheduler polls<br/>every iteration"| P["LLM-API pull path (IPC)"]
  P --> C["consumer<br/>LLM.get_kv_cache_events()"]

```

**After** — streaming per-rank publish (opt-in via `kv_cache_config.kv_events_config`):
Loading

```
flowchart LR
  subgraph S["scheduler / KV-manager thread"]
    H["V2 radix tree<br/>store / remove hooks"] --> NM["StreamingKVCacheEventManager<br/>build wire structs<br/>reuse radix hash · coalesce"]
    NM -->|"flush per iteration<br/>non-blocking enqueue"| Q["bounded queue"]
  end
  subgraph BG["background publisher thread"]
    Q --> ENC["msgpack encode"]
    ENC --> PUB["ZeroMQ PUB<br/>binds base_port + rank"]
  end
  PUB --> SUB["external subscriber<br/>(e.g. Dynamo)"]

```

Each rank builds events on the scheduler thread (cheap, non-blocking enqueue) and a background thread publishes them — no gather onto rank 0 and no per-iteration pull path.
## Validation
End-to-end benchmarking on a multi-node GB300 disaggregated deployment (DeepSeek-V4-Pro) is in progress; numbers will be added once available. (The streaming path requires `TLLM_KV_CACHE_MANAGER_V2_BACKEND=python`, since the per-block event hooks run in the V2 Python backend.)
## Context
Supersedes [#16869](https://github.com/NVIDIA/TensorRT-LLM/pull/16869) and [#16876](https://github.com/NVIDIA/TensorRT-LLM/pull/16876) by [@alec-flowers](https://github.com/alec-flowers) (original authorship preserved). Relates to RFC [#17013](https://github.com/NVIDIA/TensorRT-LLM/issues/17013).
## Dev Engineer Review
  * Adds opt-in ZeroMQ streaming KV-cache events for KV cache manager V2.
  * Adds `KVEventsConfig` and public exports.
  * Publishes stored, removed, and cleared events with `msgspec`.
  * Adds replay support, batching, queue limits, sequence-gap detection, and clean shutdown.
  * Validates backend, parallelism, endpoints, ports, and overlapping publish/replay ranges.
  * Preserves buffered event behavior for hybrid models and non-streaming configurations.
  * Suppresses partial and multimodal blocks.
  * Adds configuration and delivery-semantics documentation.
  * Adds the normalized runtime `BACKEND` export.
  * No unintended changes to legacy polling behavior are indicated.
  * The implementation includes cleanup guards and continues after individual encode or send failures.


## QA Engineer Review
  * Modified test file: `tests/unittest/kv_cache_manager_v2_tests/test_streaming_kv_events.py`.
  * Added coverage for: 
    * `test_dropped_batches_leave_a_sequence_gap`
    * `test_validate_streaming_support_rejects_unsupported_setups`
    * `test_validate_endpoint_ranges`
  * Expanded coverage for signed hashes, endpoint rebinding, publisher defaults, endpoint offsets, streaming setup, and block removal.
  * No `tests/integration/test_lists/` coverage entry was identified.
  * Verdict: needs follow-up because CI or manual QA test-list coverage is not shown.


[alec-flowers](https://github.com/alec-flowers) and others added 4 commits [July 29, 2026 13:08](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#commits-pushed-92b8fd3)
` feat: publish native v2 kv cache events[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/92b8fd38a4fb162d490c124c9402af1fc6d7e540 "feat: publish native v2 kv cache events

Signed-off-by: Alec Flowers <aflowers@nvidia.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Alec Flowers <aflowers@nvidia.com>
```

` perf: optimize native v2 kv event production[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/195fd1d7c8bf000b5300f9e76efc579223ef50a7 "perf: optimize native v2 kv event production

Signed-off-by: Alec Flowers <aflowers@nvidia.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Alec Flowers <aflowers@nvidia.com>
```

` refactor: streamline native KV event manager[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/7c58c0d0fe1a2232387f5a967e73ac2cbdcf5343 "refactor: streamline native KV event manager

- pull API \(get_latest_events\) returns \[\] instead of raising, so
  LLM.get_kv_cache_events\(\)/RPC fetch degrade cleanly in native mode
  instead of erroring and spamming tracebacks every poll
- drop the dead generic conversion path \(publish_local_events /
  _convert_event\) superseded by the scheduler-local fast path
- stop subclassing KVCacheEventManager; implement the event-sink hook
  interface by duck typing to avoid partially-initialised base state
- remove the hardcoded kv_event_allgathers=0 log metric

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- pull API (get_latest_events) returns [] instead of raising, so
  LLM.get_kv_cache_events()/RPC fetch degrade cleanly in native mode
  instead of erroring and spamming tracebacks every poll
- drop the dead generic conversion path (publish_local_events /
  _convert_event) superseded by the scheduler-local fast path
- stop subclassing KVCacheEventManager; implement the event-sink hook
  interface by duck typing to avoid partially-initialised base state
- remove the hardcoded kv_event_allgathers=0 log metric

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>
```

` refactor: nest KV events config under KvCacheConfig[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/5a66ee1a8c46b8a253387438c1f4ca6fb2b42881 "refactor: nest KV events config under KvCacheConfig

Unify the KV-event configuration surface: move kv_events_config from a
top-level TorchLlmArgs field into KvCacheConfig, alongside the existing
event_buffer_max_size / attention_dp_events_gather_period_ms knobs, so
there is a single place to configure KV-cache events. Mark the field
prototype and warn when native events are requested on a non-V2 KV cache
manager \(where they are silently unsupported\).

Users now set kv_cache_config.kv_events_config instead of a top-level
kv_events_config.

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).

```
Unify the KV-event configuration surface: move kv_events_config from a
top-level TorchLlmArgs field into KvCacheConfig, alongside the existing
event_buffer_max_size / attention_dp_events_gather_period_ms knobs, so
there is a single place to configure KV-cache events. Mark the field
prototype and warn when native events are requested on a non-V2 KV cache
manager (where they are silently unsupported).

Users now set kv_cache_config.kv_events_config instead of a top-level
kv_events_config.

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>
```

[github-actions](https://github.com/apps/github-actions) Bot assigned [tanmayv25](https://github.com/tanmayv25) [Jul 29, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#event-28684731860)
[tanmayv25](https://github.com/tanmayv25) added 3 commits [July 29, 2026 15:09](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#commits-pushed-92f92de)
` fix: address independent review of native KV events[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/92f92dee292cbc0ef35399547dde62d8fcd3069d "fix: address independent review of native KV events

- get_latest_events: remove the raise in KVCacheManagerV2's wrapper so
  native mode returns \[\] on the pull path \(the earlier fix only touched
  the inner manager, which the wrapper shadowed\)
- never drop block-removal events under the per-iteration entry cap; a
  dropped removal permanently desyncs the consumer \(block reported stored
  but never removed\). Add a socket-free regression test.
- inline the single-use _to_wire_hash helper and drop its unreachable
  branches

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).

```
- get_latest_events: remove the raise in KVCacheManagerV2's wrapper so
  native mode returns [] on the pull path (the earlier fix only touched
  the inner manager, which the wrapper shadowed)
- never drop block-removal events under the per-iteration entry cap; a
  dropped removal permanently desyncs the consumer (block reported stored
  but never removed). Add a socket-free regression test.
- inline the single-use _to_wire_hash helper and drop its unreachable
  branches

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>
```

` refactor: collapse KVEventAdapter into NativeKVCacheEventManager[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/902c2ee41741d7b348914d1300cbe3d93e995573 "refactor: collapse KVEventAdapter into NativeKVCacheEventManager

The adapter was a thin envelope: it wrapped wire events into a batch and
owned the publisher lifecycle, duplicating the publisher's enqueued/dropped
counters. Fold it into the manager, which now creates and owns the publisher
directly and builds the batch in flush_iteration_events. Replace the
kv_event_adapter presence flag with a native_kv_events_enabled property on
KVCacheManagerV2.

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).

```
The adapter was a thin envelope: it wrapped wire events into a batch and
owned the publisher lifecycle, duplicating the publisher's enqueued/dropped
counters. Fold it into the manager, which now creates and owns the publisher
directly and builds the batch in flush_iteration_events. Replace the
kv_event_adapter presence flag with a native_kv_events_enabled property on
KVCacheManagerV2.

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>
```

` fix: address xhigh code-review findings for native KV events[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/bee841395584bdcba62f882e2be9479f3420f8ad "fix: address xhigh code-review findings for native KV events

Config validation:
- require hwm/max_queue_size/buffer_steps > 0 \(0 inverts ZMQ/Queue
  semantics into 'unlimited', defeating backpressure\)
- reject empty endpoint; document that co-located engines need distinct ports

Endpoint handling:
- PUB socket always binds \(tcp/ipc/inproc\) instead of connect\(\)ing explicit
  hosts like tcp://0.0.0.0 \(which silently dropped all events\)
- offset_endpoint_port handles ipc:// for DP rank>0

Correctness / teardown:
- exclude non-attention \(SSM\) life cycles from native event target selection
  so hybrid Mamba models do not emit a corrupt/empty attention-reuse stream
- warn when both legacy event_buffer_max_size and native events are enabled
- removals no longer consume the store entry budget \(was starving BlockStored\)
- guard removed-event hooks on _closed; split dropped_batches into two
  single-writer counters \(lock-free\); shut the event manager down last in
  teardown and stop nulling it \(avoids a get/flush None race\); tear the
  publisher down if manager construction fails after it bound

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).

```
Config validation:
- require hwm/max_queue_size/buffer_steps > 0 (0 inverts ZMQ/Queue
  semantics into 'unlimited', defeating backpressure)
- reject empty endpoint; document that co-located engines need distinct ports

Endpoint handling:
- PUB socket always binds (tcp/ipc/inproc) instead of connect()ing explicit
  hosts like tcp://0.0.0.0 (which silently dropped all events)
- offset_endpoint_port handles ipc:// for DP rank>0

Correctness / teardown:
- exclude non-attention (SSM) life cycles from native event target selection
  so hybrid Mamba models do not emit a corrupt/empty attention-reuse stream
- warn when both legacy event_buffer_max_size and native events are enabled
- removals no longer consume the store entry budget (was starving BlockStored)
- guard removed-event hooks on _closed; split dropped_batches into two
  single-writer counters (lock-free); shut the event manager down last in
  teardown and stop nulling it (avoids a get/flush None race); tear the
  publisher down if manager construction fails after it bound

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>
```

reviewed  [ Jul 31, 2026 ](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#pullrequestreview-4823321789)
[ View reviewed changes ](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/bee841395584bdcba62f882e2be9479f3420f8ad)
Comment thread [tensorrt_llm/_torch/pyexecutor/kv_cache_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/bee841395584bdcba62f882e2be9479f3420f8ad#diff-868581853480e4093f8014cd5918f31fca188aa624b94a8771f624d73a0ea236) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
reviewed  [ Jul 31, 2026 ](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#pullrequestreview-4824917626)
[ View reviewed changes ](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/bee841395584bdcba62f882e2be9479f3420f8ad)
Comment thread [tensorrt_llm/_torch/pyexecutor/kv_cache_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/bee841395584bdcba62f882e2be9479f3420f8ad#diff-868581853480e4093f8014cd5918f31fca188aa624b94a8771f624d73a0ea236) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
### 
commented [Aug 1, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5148609185)
Copy link  Copy Markdown 
Collaborator Author  
|  /bot run  |  
| --- |  
` fix: address native KV events review comments[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf "fix: address native KV events review comments

- Reuse truncate_sha256_hash_to_int64 for the vLLM wire hash instead of a
  second, divergent SHA-256->int64 truncation, keeping native and legacy
  event hashes consistent for the same block.
- Replace logger.exception \(absent on tensorrt_llm's logger; would raise
  AttributeError\) with logger.error + traceback.format_exc\(\) at all four
  call sites.

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).

```
- Reuse truncate_sha256_hash_to_int64 for the vLLM wire hash instead of a
  second, divergent SHA-256->int64 truncation, keeping native and legacy
  event hashes consistent for the same block.
- Replace logger.exception (absent on tensorrt_llm's logger; would raise
  AttributeError) with logger.error + traceback.format_exc() at all four
  call sites.

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>
```

[tanmayv25](https://github.com/tanmayv25) marked this pull request as ready for review [August 5, 2026 21:31](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#event-29035079561)
[tanmayv25](https://github.com/tanmayv25) requested review from a team as [code owners](https://github.com/NVIDIA/TensorRT-LLM/blob/a23e8decb577601a8d3b7b7932d7c70069a1c4c4/.github/CODEOWNERS#L490) [August 5, 2026 21:31](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#event-29035080314)
[tanmayv25](https://github.com/tanmayv25) requested review from [QiJune](https://github.com/QiJune), [arysef](https://github.com/arysef), [liji-nv](https://github.com/liji-nv), [nv-xtf](https://github.com/nv-xtf), [reasonsolo](https://github.com/reasonsolo), [tongyuantongyu](https://github.com/tongyuantongyu) and [yizhang-nv](https://github.com/yizhang-nv) [August 5, 2026 21:31](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#event-29035080985)
[tanmayv25](https://github.com/tanmayv25) marked this pull request as draft [August 5, 2026 21:32](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#event-29035119940)
### 
commented [Aug 5, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5197723532) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Copy link  Copy Markdown 
Contributor  
|  Note
## Reviews paused
It looks like this branch is under active development. To avoid overwhelming you with review comments due to an influx of new commits, CodeRabbit has automatically paused this review. You can configure this behavior by changing the `reviews.auto_review.auto_pause_after_reviewed_commits` setting. Use the following commands to manage reviews:
  * `@coderabbitai resume` to resume automatic reviews.
  * `@coderabbitai review` to trigger a single review.

Use the checkboxes below for quick actions:
  * ▶️ Resume reviews
  * 🔍 Trigger review


## Walkthrough
This PR adds configurable native KV-cache event publishing. It defines event wire types, asynchronous ZeroMQ publishing, replay support, and streaming event batching. It integrates the feature with `KVCacheManagerV2`, executor setup, public configuration exports, usage metadata, documentation, and tests.
### Changes
**Native KV-cache events**  
 | Layer / File(s)  | Summary  |  
| --- | --- |  
|  **Event contracts and asynchronous publishing** `tensorrt_llm/llmapi/llm_args.py`, `tensorrt_llm/_torch/pyexecutor/kv_cache_events.py`, `tensorrt_llm/llmapi/__init__.py`, `tensorrt_llm/llmapi/llm_utils.py`, `tensorrt_llm/runtime/kv_cache_manager_v2/__init__.py`, `tensorrt_llm/usage/llm_args_golden_manifest.json`  | Adds `KVEventsConfig`, wire structures, null and ZeroMQ publishers, replay handling, endpoint rank offsets, queue limits, shutdown behavior, and the public backend constant.  |  
|  **Native event generation and batching** `tensorrt_llm/_torch/pyexecutor/kv_cache_events.py`  | Adds lifecycle filtering, signed hash conversion, stored and removed event batching, capacity enforcement, flushing, counters, and shutdown.  |  
|  **KVCacheManagerV2 integration** `tensorrt_llm/_torch/pyexecutor/kv_cache_manager_v2.py`  | Selects streaming or buffered event management, validates backend and parallelism settings, handles initialization failures, filters event windows, exposes streaming status, and shuts down streaming resources.  |  
|  **Executor wiring and validation** `tensorrt_llm/_torch/pyexecutor/_util.py`, `tensorrt_llm/_torch/pyexecutor/py_executor.py`, `tests/unittest/kv_cache_manager_v2_tests/test_streaming_kv_events.py`  | Passes configuration to managers, enables streaming events, warns for non-V2 managers, and tests publishing, filtering, removal delivery, defaults, endpoint offsets, sequence gaps, and validation.  |  
|  **KV-cache event documentation** `docs/source/features/kvcache.md`  | Documents buffered and streaming event paths, configuration, endpoint allocation, wire format, delivery gaps, and replay behavior.  |  
**Estimated code review effort:** 4 (Complex) | ~45 minutes
**Merge Risk:** _🔵 Low_ · up to `4ebfa`
The opt-in streaming KV-cache event path is mergeable, but the documentation currently lists event types that do not match the published contract, which could mislead external router implementers; align the documentation before or immediately after merge.
### Sequence Diagram(s)
Loading

```
sequenceDiagram
  participant PyExecutor
  participant KVCacheManagerV2
  participant StreamingKVCacheEventManager
  participant ZmqEventPublisher
  PyExecutor->>KVCacheManagerV2: pass KVEventsConfig
  KVCacheManagerV2->>StreamingKVCacheEventManager: initialize streaming manager
  KVCacheManagerV2->>StreamingKVCacheEventManager: forward cache lifecycle hooks
  StreamingKVCacheEventManager->>ZmqEventPublisher: publish stored or removed batch

```

**Suggested reviewers:** `qijune`, `liji-nv`
🚥 Pre-merge checks | ✅ 4 | ❌ 1
### ❌ Failed checks (1 warning)  
| Check name  | Status  | Explanation  | Resolution  |  
| --- | --- | --- | --- |  
| Docstring Coverage  |  ⚠️ Warning  | Docstring coverage is 21.67% which is insufficient. The required threshold is 80.00%. Docstring coverage is scoped to functions touched by this diff. Analyzed 60 functions across 5 files. (1 skipped: …  | Write docstrings for the functions missing them to satisfy the coverage threshold.  |  
✅ Passed checks (4 passed)  
| Check name  | Status  | Explanation  |  
| --- | --- | --- |  
| Linked Issues check  | ✅ Passed  | Check skipped because no linked issues were found for this pull request.  |  
| Out of Scope Changes check  | ✅ Passed  | Check skipped because no linked issues were found for this pull request.  |  
| Title check  | ✅ Passed  | The title clearly identifies the feature: improved KV event publishing through streaming events. It follows the required ticket and type format and matches the main changes.  |  
| Description check  | ✅ Passed  | The description clearly explains the motivation, implementation, configuration, constraints, behavior changes, validation status, and related context. It does not include explicit Test Coverage or PR …  |  
Full details: Docstring Coverage
**Explanation**
Docstring coverage is 21.67% which is insufficient. The required threshold is 80.00%. Docstring coverage is scoped to functions touched by this diff. Analyzed 60 functions across 5 files. (1 skipped: 1 unsupported.)
✨ Finishing Touches 💡 1 ⚔️ Resolve merge conflicts 💡
  * Resolve merge conflict in branch `feat/native-kv-events-clean`

🧪 Generate unit tests (beta)
  * Create PR with unit tests


Comment `@coderabbitai help` to get the list of available commands.
 |  
reviewed  [ Aug 5, 2026 ](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#pullrequestreview-4869040172)
[ View reviewed changes ](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
**Actionable comments posted: 10**
🧹 Nitpick comments (4)
> tensorrt_llm/llmapi/llm_args.py (1)
>> `3662-3664`: _📐 Maintainability & Code Quality_ | _🔵 Trivial_ | _💤 Low value_
>> **Annotate the`model_post_init` context parameter.**
>> The coding guidelines require an annotation on every function parameter. Pydantic v2 declares the hook as `model_post_init(self, context: Any, /) -> None`, so rename and annotate the parameter.
>> ♻️ Proposed refactor
>> 
```
-    def model_post_init(self, __context) -> None:
+    def model_post_init(self, context: Any) -> None:
         if self.publisher is None:
             self.publisher = "zmq" if self.enable_kv_cache_events else "null"
```

>> As per coding guidelines: "Annotate every function, use `None` for procedures" and "avoid unnecessary double underscores".
>> 🤖 Prompt for AI Agents
>> 
```
Verify each finding against current code. Fix only still-valid issues, skip the
rest with a brief reason, keep changes minimal, and validate.

In `@tensorrt_llm/llmapi/llm_args.py` around lines 3662 - 3664, Update
model_post_init so its context parameter is named context and annotated with
Any, while preserving the existing publisher initialization behavior.

```

>> _Source: Coding guidelines_
> tensorrt_llm/_torch/pyexecutor/_util.py (1)
>> `1145-1147`: _📐 Maintainability & Code Quality_ | _🔵 Trivial_ | _💤 Low value_
>> **Document why this argument bypasses the resolved`kv_cache_config`.**
>> Every other argument in this call uses the `kv_cache_config` local resolved at lines 1111-1112, which honors `kv_cache_config_override`. This expression instead reads `self._llm_args.kv_cache_config.kv_events_config`. The values agree today, because each override is produced by `model_copy()` and shares the nested `KVEventsConfig` object. A short comment prevents a future maintainer from adding a per-manager override of `kv_events_config` and finding it ignored.
>> ♻️ Proposed refactor
>> 
```
+            # Native events are a single top-level setting, deliberately not
+            # taken from kv_cache_config_override: only one manager per rank
+            # may bind the endpoint. Estimation managers are transient and
+            # draft managers have no prefix reuse to report, so both get None.
             kv_events_config=None
             if estimating_kv_cache or model_engine.is_draft_model else
             self._llm_args.kv_cache_config.kv_events_config,
```

>> 🤖 Prompt for AI Agents
>> 
```
Verify each finding against current code. Fix only still-valid issues, skip the
rest with a brief reason, keep changes minimal, and validate.

In `@tensorrt_llm/_torch/pyexecutor/_util.py` around lines 1145 - 1147, Add a
concise comment next to the kv_events_config expression explaining that it
intentionally reads self._llm_args.kv_cache_config.kv_events_config rather than
the resolved kv_cache_config, because overrides share the nested KVEventsConfig
and this argument must retain the existing behavior.

```

> tensorrt_llm/_torch/pyexecutor/kv_cache_events.py (2)
>> `510-522`: _📐 Maintainability & Code Quality_ | _🔵 Trivial_ | _⚡ Quick win_
>> **Count removal keys that are skipped as non-bytes.**
>> Line 517 skips any entry that is not `bytes` with no counter and no log. Every other suppression path in this class increments a counter, such as `partial_blocks_suppressed` or `non_target_life_cycles_ignored`. The current V2 call sites pass byte keys, so this branch is unreachable today. A missed removal is the one failure mode that makes a consumer treat a block as resident forever, so make a future contract change visible instead of silent.
>> ♻️ Proposed refactor
>> 
```
         self.non_target_life_cycles_ignored = 0
+        self.unsupported_removal_keys = 0
         self.dropped_events = 0
```

>> 
```
         for block_key in block_hashes:
             if not isinstance(block_key, bytes):
+                self.unsupported_removal_keys += 1
                 continue
             state = self._stored_blocks.pop(block_key, None)
```

>> Add the counter to the `shutdown` summary alongside the existing counters.
>> 🤖 Prompt for AI Agents
>> 
```
Verify each finding against current code. Fix only still-valid issues, skip the
rest with a brief reason, keep changes minimal, and validate.

In `@tensorrt_llm/_torch/pyexecutor/kv_cache_events.py` around lines 510 - 522,
Update add_removed_event to count entries skipped because block_key is not
bytes, using a dedicated counter consistent with the class’s existing
suppression counters. Increment it before continuing, and include the counter in
the shutdown summary alongside partial_blocks_suppressed and
non_target_life_cycles_ignored.

```

>> `246-246`: _📐 Maintainability & Code Quality_ | _🔵 Trivial_ | _💤 Low value_
>> **Consider narrowing the caught exception types.**
>> The coding guidelines require the narrowest exception possible. The publisher thread must survive transport failures, so a broad catch is defensible here, but naming the expected types documents the contract and lets a genuine programming error surface. The concrete failures are `zmq.ZMQError` from `send_multipart` and `recv_multipart`, and `msgspec.EncodeError` from `encoder.encode`.
>> If you keep the broad catch, add a short comment stating that the thread must never terminate. Ruff `BLE001` is reported by static analysis, but the repository's enabled Ruff rule set does not include `BLE`, so this is not a lint failure.
>> As per coding guidelines: "Catch the narrowest exception possible" and "Catch specific exceptions instead of using broad or bare `except:` handlers."
>> Also applies to: 270-270
>> 🤖 Prompt for AI Agents
>> 
```
Verify each finding against current code. Fix only still-valid issues, skip the
rest with a brief reason, keep changes minimal, and validate.

In `@tensorrt_llm/_torch/pyexecutor/kv_cache_events.py` at line 246, Update the
exception handlers in the publisher thread around send_multipart,
recv_multipart, and encoder.encode to catch the specific expected zmq.ZMQError
and msgspec.EncodeError types instead of Exception, while preserving thread
survival on transport or encoding failures. Apply the same narrowing to both
affected handlers.

```

>> _Sources: Coding guidelines, Linters/SAST tools_
🤖 Prompt for all review comments with AI agents

```
Verify each finding against current code. Fix only still-valid issues, skip the
rest with a brief reason, keep changes minimal, and validate.

Inline comments:
In `@tensorrt_llm/_torch/pyexecutor/kv_cache_events.py`:
- Around line 159-165: Update the initialization flow around _socket_setup to
close the already-created PUB socket whenever setup raises before the object is
fully constructed, then re-raise the original exception. Prefer moving endpoint
validation before socket creation within _socket_setup to avoid allocating
sockets for invalid or unsupported endpoints, while preserving existing bind
behavior for valid endpoints.
- Around line 305-323: Update offset_endpoint_port to detect transports using
scheme prefixes consistent with _socket_setup, so hostnames containing “ipc” or
“inproc” remain valid TCP endpoints. For TCP endpoints, validate that a port is
present and numeric before converting and offsetting it, while preserving the
existing range check and rank-zero behavior.
- Around line 343-351: Update the expected hash calculation in
test_native_kv_events.py to use the first 8 bytes of the block hash, matching
truncate_sha256_hash_to_int64() and _vllm_wire_hash_from_radix_key(). Replace
the current last-8-byte slicing while preserving the existing signed 64-bit
conversion expectations.
- Around line 491-498: Update the token conversion flow around _token_ids to
recognize blocks containing the bytes digest produced by
gen_multimodal_cache_key_tokens before native event conversion. Exclude those
multimodal blocks without raising ValueError or incrementing dropped_events
through the traceback path; otherwise, define and consistently emit a valid
vLLM-compatible token_ids representation for them.
- Around line 259-278: Update the event publishing flow around the sequence
allocation, enqueue, and exception handling so every dropped batch remains
observable to consumers. Ensure queue-full drops and encode/send failures either
reserve a sequence number and emit an explicit loss marker on the wire, or
otherwise add a corresponding marker to the replay buffer before advancing to
the next sequence; preserve ordering and ensure END_SEQ cannot make an
incomplete stream appear complete.

In `@tensorrt_llm/llmapi/llm_args.py`:
- Around line 3639-3642: Update the replay_endpoint Field declaration to enforce
a minimum length of 1, matching the validation applied to endpoint. Preserve
None as the allowed unset value while rejecting empty strings during
configuration validation before socket setup.

In `@tests/unittest/kv_cache_manager_v2_tests/test_native_kv_events.py`:
- Around line 146-181: Update
test_native_removals_are_never_dropped_by_the_entry_cap to flush the manager via
flush_iteration_events() after queuing the removals, using a recording publisher
or ZeroMQ subscriber to capture emitted batches. Assert the flushed MessagePack
payload contains both removed block hashes, rather than only inspecting
manager._pending_events.
- Around line 33-35: Add a None return annotation to both test functions:
tests/unittest/kv_cache_manager_v2_tests/test_native_kv_events.py lines 33-35,
test_native_fast_path_publishes_only_full_max_window_blocks, and lines 146-147,
test_native_removals_are_never_dropped_by_the_entry_cap.
- Around line 33-143: Wrap the manager and subscriber lifecycle in
test_native_fast_path_publishes_only_full_max_window_blocks with try/finally so
manager.shutdown(), subscriber.close(), and endpoint cleanup run even when
assertions fail. Also wrap the manager lifecycle in the test spanning
tests/unittest/kv_cache_manager_v2_tests/test_native_kv_events.py lines 146-183
with try/finally, ensuring its shutdown executes on every failure path.
- Around line 27-30: Replace the _unused_tcp_port approach and fixed time.sleep
synchronization in the NativeKVCacheEventManager ZeroMQ setup with a retry
fixture. Have the fixture retry the publish-and-receive setup when binding fails
due to an address-in-use zmq.ZMQError, catching only that expected error and
allowing other failures to propagate. Ensure the test proceeds only after the
subscriber successfully receives the published event.

---

Nitpick comments:
In `@tensorrt_llm/_torch/pyexecutor/_util.py`:
- Around line 1145-1147: Add a concise comment next to the kv_events_config
expression explaining that it intentionally reads
self._llm_args.kv_cache_config.kv_events_config rather than the resolved
kv_cache_config, because overrides share the nested KVEventsConfig and this
argument must retain the existing behavior.

In `@tensorrt_llm/_torch/pyexecutor/kv_cache_events.py`:
- Around line 510-522: Update add_removed_event to count entries skipped because
block_key is not bytes, using a dedicated counter consistent with the class’s
existing suppression counters. Increment it before continuing, and include the
counter in the shutdown summary alongside partial_blocks_suppressed and
non_target_life_cycles_ignored.
- Line 246: Update the exception handlers in the publisher thread around
send_multipart, recv_multipart, and encoder.encode to catch the specific
expected zmq.ZMQError and msgspec.EncodeError types instead of Exception, while
preserving thread survival on transport or encoding failures. Apply the same
narrowing to both affected handlers.

In `@tensorrt_llm/llmapi/llm_args.py`:
- Around line 3662-3664: Update model_post_init so its context parameter is
named context and annotated with Any, while preserving the existing publisher
initialization behavior.

```

🪄 Autofix
Fix all unresolved CodeRabbit comments on this PR:
  * Push a commit to this branch (recommended)
  * Create a new PR with the fixes

ℹ️ Review info ⚙️ Run configuration
**Configuration used** : Path: .coderabbit.yaml
**Review profile** : CHILL
**Plan** : Enterprise
**Run ID** : `20bf962b-8636-4228-8564-cb26da63e134`
📥 Commits
Reviewing files that changed from the base of the PR and between [`c45ad83`](https://github.com/NVIDIA/TensorRT-LLM/commit/c45ad837c5f9a705f1db6149e7281544f4cd8835) and [`8f3474b`](https://github.com/NVIDIA/TensorRT-LLM/commit/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf).
📒 Files selected for processing (9)
  * `tensorrt_llm/_torch/pyexecutor/_util.py`
  * `tensorrt_llm/_torch/pyexecutor/kv_cache_events.py`
  * `tensorrt_llm/_torch/pyexecutor/kv_cache_manager_v2.py`
  * `tensorrt_llm/_torch/pyexecutor/py_executor.py`
  * `tensorrt_llm/llmapi/__init__.py`
  * `tensorrt_llm/llmapi/llm_args.py`
  * `tensorrt_llm/llmapi/llm_utils.py`
  * `tensorrt_llm/usage/llm_args_golden_manifest.json`
  * `tests/unittest/kv_cache_manager_v2_tests/test_native_kv_events.py`


Comment thread [tensorrt_llm/_torch/pyexecutor/kv_cache_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-868581853480e4093f8014cd5918f31fca188aa624b94a8771f624d73a0ea236) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tensorrt_llm/_torch/pyexecutor/kv_cache_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-868581853480e4093f8014cd5918f31fca188aa624b94a8771f624d73a0ea236) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tensorrt_llm/_torch/pyexecutor/kv_cache_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-868581853480e4093f8014cd5918f31fca188aa624b94a8771f624d73a0ea236) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tensorrt_llm/_torch/pyexecutor/kv_cache_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-868581853480e4093f8014cd5918f31fca188aa624b94a8771f624d73a0ea236) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tensorrt_llm/_torch/pyexecutor/kv_cache_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-868581853480e4093f8014cd5918f31fca188aa624b94a8771f624d73a0ea236) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tensorrt_llm/llmapi/llm_args.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-bf140a489175e4c6e5931cd19cc57e06c1634483517f06c4e6fda96d5d26b056) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tests/unittest/kv_cache_manager_v2_tests/test_streaming_kv_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-513fd8c8154465cf4d47bdf9f8cc4058d4135fd69ecddf57947e84ab223e2b28) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tests/unittest/kv_cache_manager_v2_tests/test_streaming_kv_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-513fd8c8154465cf4d47bdf9f8cc4058d4135fd69ecddf57947e84ab223e2b28) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tests/unittest/kv_cache_manager_v2_tests/test_streaming_kv_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-513fd8c8154465cf4d47bdf9f8cc4058d4135fd69ecddf57947e84ab223e2b28) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
Comment thread [tests/unittest/kv_cache_manager_v2_tests/test_streaming_kv_events.py](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/files/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf#diff-513fd8c8154465cf4d47bdf9f8cc4058d4135fd69ecddf57947e84ab223e2b28) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
` test: align native KV event wire-hash assertions with truncation change[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/c70dc1557dcd047e30edbb579301523040c657e4 "test: align native KV event wire-hash assertions with truncation change

8f3474b switched the wire hash to truncate_sha256_hash_to_int64 \(first 8
bytes of the radix key\) but left the test asserting the old last-8-byte
values, so the test failed deterministically in CI. Update the synthetic
keys and expected hashes to the first-8-byte convention, keeping the
signed-wraparound branch covered.

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[tanmayv25](https://github.com/tanmayv25) Tanmay Verma 
GPG key ID: 027936A1E5A505AB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).

```
[`8f3474b`](https://github.com/tanmayv25/TensorRT-LLM/commit/8f3474b3a3cef77abf63e5da85bfed41ebbe01cf) switched the wire hash to truncate_sha256_hash_to_int64 (first 8
bytes of the radix key) but left the test asserting the old last-8-byte
values, so the test failed deterministically in CI. Update the synthetic
keys and expected hashes to the first-8-byte convention, keeping the
signed-wraparound branch covered.

Signed-off-by: tanmayv25 <tanmay2592@gmail.com>
```

` fix: preserve joint KV cache reuse during merge resolution[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/d5d3337ac3d30f88ffc69689b7eef9bd9728efa2 "fix: preserve joint KV cache reuse during merge resolution

Signed-off-by: Allison Lim <allim@nvidia.com>") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).

```
Signed-off-by: Allison Lim <allim@nvidia.com>
```

### 
commented [Sep 7, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5574323167)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #71997](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/71997/) [ run ] triggered by Bot. Commit: `d5d3337` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5574264617)  |  
| --- |  
### 
commented [Sep 7, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5574349825)
Copy link  Copy Markdown 
Contributor  
|  /bot run --disable-fail-fast  |  
| --- |  
### 
commented [Sep 7, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5574398201)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #71998](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/71998/) [ run ] triggered by Bot. Commit: `d5d3337` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5574349825)  |  
| --- |  
### 
commented [Sep 7, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5574430661)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #71997](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/71997/) [ run ] completed with state `ABORTED`. Commit: `d5d3337` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5574264617)  |  
| --- |  
### 
commented [Sep 7, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5576515210)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #71998](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/71998/) [ run ] completed with state `SUCCESS`. Commit: `d5d3337` [/LLM/main/L0_MergeRequest_PR pipeline #59059](https://nv/trt-llm-cicd/job/main/job/L0_MergeRequest_PR/59059/) completed with status: 'FAILURE' ⚠️ **Action Required:**
  * Please check the failed tests and fix your PR
  * If you cannot view the failures, ask the CI triggerer to share details
  * Once fixed, request an NVIDIA team member to trigger CI again

[CI Agent Failure Analysis](https://pbss.s8k.io/v1/AUTH_svc_tensorrt/sw-tensorrt-ci-analysis/LLM/main/L0_MergeRequest_PR/59059/failure_analysis.html) [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5574349825)  |  
| --- |  
[allisonlim-nv](https://github.com/allisonlim-nv) changed the title ~~[None][feat] Improve KV event publish performance via StreamingEvent~~ [TRTLLM-15331][feat] Improve KV event publish performance via StreamingEvent [Sep 7, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#event-30717919984)
### 
commented [Sep 7, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5576547793)
Copy link  Copy Markdown 
Contributor  
|  /bot run --disable-fail-fast  |  
| --- |  
### 
commented [Sep 7, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5576627217)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72006](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72006/) [ run ] triggered by Bot. Commit: `d5d3337` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5576547793)  |  
| --- |  
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5578638814)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72006](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72006/) [ run ] completed with state `FAILURE`. Commit: `d5d3337` [/LLM/main/L0_MergeRequest_PR pipeline #59067](https://nv/trt-llm-cicd/job/main/job/L0_MergeRequest_PR/59067/) completed with status: 'FAILURE' ⚠️ **Action Required:**
  * Please check the failed tests and fix your PR
  * If you cannot view the failures, ask the CI triggerer to share details
  * Once fixed, request an NVIDIA team member to trigger CI again

[CI Agent Failure Analysis](https://pbss.s8k.io/v1/AUTH_svc_tensorrt/sw-tensorrt-ci-analysis/LLM/main/L0_MergeRequest_PR/59067/failure_analysis.html) [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5576547793)  |  
| --- |  
` Merge branch 'main' into feat/native-kv-events-clean[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/7139362a61ae30a529aff9277945b2275703c7ad "Merge branch 'main' into feat/native-kv-events-clean") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5579267544)
Copy link  Copy Markdown 
Contributor  
|  /bot run --disable-fail-fast  |  
| --- |  
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5579319458)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72048](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72048/) [ run ] triggered by Bot. Commit: `7139362` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5579267544)  |  
| --- |  
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5582238972)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72048](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72048/) [ run ] completed with state `FAILURE`. Commit: `7139362` [/LLM/main/L0_MergeRequest_PR pipeline #59106](https://nv/trt-llm-cicd/job/main/job/L0_MergeRequest_PR/59106/) completed with status: 'FAILURE' ⚠️ **Action Required:**
  * Please check the failed tests and fix your PR
  * If you cannot view the failures, ask the CI triggerer to share details
  * Once fixed, request an NVIDIA team member to trigger CI again

[CI Agent Failure Analysis](https://pbss.s8k.io/v1/AUTH_svc_tensorrt/sw-tensorrt-ci-analysis/LLM/main/L0_MergeRequest_PR/59106/failure_analysis.html) [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5579267544)  |  
| --- |  
` Merge branch 'main' into feat/native-kv-events-clean[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/bd3daf272eaf0216bbbd6d504e45884692958dd5 "Merge branch 'main' into feat/native-kv-events-clean") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5588373346)
Copy link  Copy Markdown 
Contributor  
|  /bot run --disable-fail-fast  |  
| --- |  
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5588468960)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72197](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72197/) [ run ] triggered by Bot. Commit: `bd3daf2` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5588373346)  |  
| --- |  
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5592832341)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72197](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72197/) [ run ] completed with state `SUCCESS`. Commit: `bd3daf2` [/LLM/main/L0_MergeRequest_PR pipeline #59239](https://nv/trt-llm-cicd/job/main/job/L0_MergeRequest_PR/59239/) completed with status: 'FAILURE' ⚠️ **Action Required:**
  * Please check the failed tests and fix your PR
  * If you cannot view the failures, ask the CI triggerer to share details
  * Once fixed, request an NVIDIA team member to trigger CI again

[CI Agent Failure Analysis](https://pbss.s8k.io/v1/AUTH_svc_tensorrt/sw-tensorrt-ci-analysis/LLM/main/L0_MergeRequest_PR/59239/failure_analysis.html) [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5588373346)  |  
| --- |  
` Merge branch 'main' into feat/native-kv-events-clean[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/b27049bb4a9b154a4a49194f87f30a0f56a08433 "Merge branch 'main' into feat/native-kv-events-clean") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5593250834)
Copy link  Copy Markdown 
Collaborator Author  
|  /bot run  |  
| --- |  
### 
commented [Sep 8, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5593330914)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72264](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72264/) [ run ] triggered by Bot. Commit: `b27049b` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5593250834)  |  
| --- |  
### 
commented [Sep 9, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5593858220)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72264](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72264/) [ run ] completed with state `SUCCESS`. Commit: `b27049b` [/LLM/main/L0_MergeRequest_PR pipeline #59300](https://nv/trt-llm-cicd/job/main/job/L0_MergeRequest_PR/59300/) completed with status: 'FAILURE' ⚠️ **Action Required:**
  * Please check the failed tests and fix your PR
  * If you cannot view the failures, ask the CI triggerer to share details
  * Once fixed, request an NVIDIA team member to trigger CI again

[CI Agent Failure Analysis](https://pbss.s8k.io/v1/AUTH_svc_tensorrt/sw-tensorrt-ci-analysis/LLM/main/L0_MergeRequest_PR/59300/failure_analysis.html) [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5593250834)  |  
| --- |  
` Merge branch 'main' into feat/native-kv-events-clean[](https://github.com/NVIDIA/TensorRT-LLM/pull/17023/commits/4f6e9f6a4d72e81db9732e78d4c398b37d441f50 "Merge branch 'main' into feat/native-kv-events-clean") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
### 
commented [Sep 9, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5595015794)
Copy link  Copy Markdown 
Collaborator  
|  /bot run --disable-fail-fast  |  
| --- |  
### 
commented [Sep 9, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5595072783)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72297](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72297/) [ run ] triggered by Bot. Commit: `4f6e9f6` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5595015794)  |  
| --- |  
### 
commented [Sep 9, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5598009808)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72297](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72297/) [ run ] completed with state `SUCCESS`. Commit: `4f6e9f6` [/LLM/main/L0_MergeRequest_PR pipeline #59329](https://nv/trt-llm-cicd/job/main/job/L0_MergeRequest_PR/59329/) completed with status: 'FAILURE' ⚠️ **Action Required:**
  * Please check the failed tests and fix your PR
  * If you cannot view the failures, ask the CI triggerer to share details
  * Once fixed, request an NVIDIA team member to trigger CI again

[CI Agent Failure Analysis](https://pbss.s8k.io/v1/AUTH_svc_tensorrt/sw-tensorrt-ci-analysis/LLM/main/L0_MergeRequest_PR/59329/failure_analysis.html) [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5595015794)  |  
| --- |  
### 
commented [Sep 9, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5606665370)
Copy link  Copy Markdown 
Collaborator Author  
|  /bot run  |  
| --- |  
### 
commented [Sep 9, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5606743449)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72494](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72494/) [ run ] triggered by Bot. Commit: `4f6e9f6` [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5606665370)  |  
| --- |  
### 
commented [Sep 10, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5610716685)
Copy link  Copy Markdown 
Collaborator  
|  [PR_Github #72494](https://nv/trt-llm-cicd/job/helpers/job/PR_Github/72494/) [ run ] completed with state `SUCCESS`. Commit: `4f6e9f6` [/LLM/main/L0_MergeRequest_PR pipeline #59507](https://nv/trt-llm-cicd/job/main/job/L0_MergeRequest_PR/59507/) completed with status: 'SUCCESS' [Link to invocation](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#issuecomment-5606665370)  |  
| --- |  
Hide details View details
[allisonlim-nv](https://github.com/allisonlim-nv) merged commit [`05293ce`](https://github.com/NVIDIA/TensorRT-LLM/commit/05293ce32ec2674b3ccc77cfc10be493322eb03b) into  NVIDIA:main [Sep 10, 2026](https://github.com/NVIDIA/TensorRT-LLM/pull/17023#event-30868079305)
7 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/NVIDIA/TensorRT-LLM/pull/17023).
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2FNVIDIA%2FTensorRT-LLM%2Fpull%2F17023)
###  Labels 
[ api-compatible ](https://github.com/NVIDIA/TensorRT-LLM/issues?q=state%3Aopen%20label%3Aapi-compatible) Accepted LLM API contract change that is backwards-compatible [ ci: full pre-merge approved ](https://github.com/NVIDIA/TensorRT-LLM/issues?q=state%3Aopen%20label%3A%22ci%3A%20full%20pre-merge%20approved%22)
###  14 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
