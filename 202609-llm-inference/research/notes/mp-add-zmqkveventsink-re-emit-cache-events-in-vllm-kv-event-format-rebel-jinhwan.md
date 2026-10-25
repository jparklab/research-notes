---
title: '[MP] Add ZmqKVEventSink: re-emit cache events in vLLM KV-event format… · rebel-jinhwan/LMCache@4f5040f
  · GitHub'
id: mp-add-zmqkveventsink-re-emit-cache-events-in-vllm-kv-event-format-rebel-jinhwan
tags:
- llmd-kv-cache-routing-c665d5
- lmcache-mp-mode
- kv-events-gap
- unmerged
created: '2026-09-14T13:02:01.301416Z'
updated: '2026-09-15T04:55:24.138554Z'
source: https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472
source_domain: github.com
fetched_at: '2026-09-14T13:02:01.279659Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Commit on a THIRD-PARTY FORK (rebel-jinhwan/LMCache, referenced from upstream
  issues #3532 and #4352 on Aug 27 2026, but not itself an upstream LMCache/LMCache
  PR) implementing ZmqKVEventSink: a second CacheEventSink in lmcache/v1/mp_coordinator/kv_event_sink.py
  that re-emits the MP coordinator''s internal CacheEventBatch stream as standard
  vLLM KVEventBatch msgpack over a ZMQ PUB socket on topic kv@<emitter_id>@<model>
  -- the exact wire format/topic llm-d''s kv-cache indexer already subscribes to.
  The design doc''s own text explicitly names ''llm-d''s EPP'' as a target consumer
  (''so an unmodified vLLM adapter indexes LMCache''s tiers next to the engines''
  GPU tier''), citing LMCache issue #4352 as ''the RFC'' -- but #4352 is Dynamo-specific
  and does not itself mention llm-d, so this llm-d claim is this fork author''s own
  unverified assertion, not confirmed by LMCache maintainers or by the actual RFC.
  Medium strings are LMCache-tier-specific, not the standard gpu/cpu bucket: medium_for()
  returns ''lmcache-l1'' for all L1 placements (tier-stable across DRAM/CXL backends)
  and ''lmcache-l2-<backend>'' (e.g. ''lmcache-l2-fs'') for L2, verified by unit tests
  (test_shared_l2_store_uses_backend_medium, test_medium_is_stable_between_store_and_delete).
  Includes BlockRemoved support (delete batches for both L1 eviction and L2 delete),
  a bounded-ring ROUTER-socket replay protocol matching vLLM''s own replay semantics,
  and non-blocking PUB sends so a slow router can''t stall the bus. Deliberately out
  of scope: engine identity via REGISTER_KV_CACHE, and crediting shared L2 fleet-wide
  via the coordinator''s POST /events. No evidence in this diff that it was ever merged
  into LMCache/LMCache upstream; a raw-content check of LMCache''s dev branch for
  lmcache/v1/mp_coordinator/kv_event_sink.py returned nothing, and the actual upstream
  fix that landed as a PR (see companion note on PR #5076) took a different, simpler
  approach using canonical medium="CPU" rather than these lmcache-l1/lmcache-l2-<backend>
  strings.'
---

*Suggested by [[lmcache-vllm-kv-events-missing-cpu-blockstored-event-only-gpu-event-emitted-issu]] — commit referenced as the Aug 27 2026 MP-mode ZmqKVEventSink fix for the CPU-tier drop bug*

[Skip to content](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472) to refresh your session. Dismiss alert
/ Public
forked from [LMCache/LMCache](https://github.com/LMCache/LMCache)
  * [ Notifications ](https://github.com/login?return_to=%2Frebel-jinhwan%2FLMCache) You must be signed in to change notification settings
  * [ Fork 0 ](https://github.com/login?return_to=%2Frebel-jinhwan%2FLMCache)
  * [ Star  0 ](https://github.com/login?return_to=%2Frebel-jinhwan%2FLMCache)


## File tree
Expand file treeCollapse file tree
Open diff view settings
Filter options
  *     * design/v1/mp_coordinator
      *     * source/mp
      *   * lmcache/v1
    * mp_coordinator
      *       *       *     * multiprocess
      *       *   * tests/v1
    * mp_coordinator
      * fixtures/kv_events
        *         *         *         *         *       *       *     * multiprocess
      * 

Expand file treeCollapse file tree
Open diff view settings
Collapse file
### [`‎docs/design/v1/mp_coordinator/cache_events.md‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-c78d02ea8743946437658f7a53b14d8a4558eadf58d812264865050f64dede7d)
Copy file name to clipboardExpand all lines: docs/design/v1/mp_coordinator/cache_events.md
+46-2Lines changed: 46 additions & 2 deletions
  * Display the source diff
  * Display the rich diff

  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -167,13 +167,57 @@ records per `L1BackendType`, so it stamps `shared` on the pooled` |  
| `167`  | `167`  | `backend's runs — the vocabulary, batching identity, and directory`  |  
| `168`  | `168`  | `semantics need no change.`  |  
| `169`  | `169`  |  
| `170`  | `+## Engine-format KV event sink`  |  
| `172`  | `+Module: `lmcache/v1/mp_coordinator/kv_event_sink.py` `  |  
| `174`  | `+KV-cache-aware routers (llm-d's EPP; see the RFC linked from`  |  
| `175`  | `+ [issue #4352](https://github.com/LMCache/LMCache/issues/4352)) learn`  |  
| `176`  | `+placements only from engine KV events: vLLM `KVEventBatch` msgpack over`  |  
| `177`  | `+a ZMQ PUB socket, topic `kv@<emitter_id>@<model>`. In MP mode the MP`  |  
| `178`  | `+connectors emit nothing there, so LMCache tiers are invisible to such a`  |  
| `179`  | `+router. `ZmqKVEventSink` is a second `CacheEventSink` that re-emits the`  |  
| `180`  | `+same `CacheEventBatch` stream in that wire format, so an unmodified vLLM`  |  
| `181`  | `+adapter indexes LMCache's tiers next to the engines' GPU tier.`  |  
| `182`  | `+ `CompositeCacheEventSink` fans one subscriber out to both sinks when the`  |  
| `183`  | `+coordinator sink is also configured.`  |  
| `185`  | `+Translation (vLLM positional layout, tag first; HMA fields never set):`  |  
| `187`  | `+ | vLLM wire | Source | `  |  
| `188`  | `+ |---|---| `  |  
| `189`  | `+ | topic `kv@<emitter_id>@<model>` | one message per configured emitter id (`--kv-events-emitter-ids`, default `node:<node name>`); `model` from the entry's `ObjectKey` | `  |  
| `190`  | `+ | `BlockStored.block_hashes` | `[chunk_hash]` (raw bytes; routers truncate to the last 8) | `  |  
| `191`  | `+ | `BlockStored.parent_block_hash` | `CacheEventEntry.parent_hash_hex`: the previous chunk in the same `mp.tokens` binding event, `nil` for the first (sequence start or a predecessor stored by an earlier request) | `  |  
| `192`  | `+ | `BlockStored.token_ids` / `block_size` | the entry's `token_ids` / their count (the chunk size) | `  |  
| `193`  | `+ | `medium` | `lmcache-l1` for L1, `lmcache-l2-<backend>` for L2 — identical on store and delete (routers refcount per medium) | `  |  
| `194`  | `+ | `BlockRemoved` | `delete` batches (L1 evictions, L2 deletes), one event per batch | `  |  
| `195`  | `+ | `access`, `config` | not forwarded | `  |  
| `197`  | `+A tokenless `store` entry (token-binding cache miss) is skipped: llm-d`  |  
| `198`  | `+recomputes its own keys from the tokens and cannot index an unknown hash`  |  
| `199`  | `+without them. A `delete` is never skipped. The `seq` frame is one counter`  |  
| `200`  | `+across all topics, numbered from 0 like vLLM; with`  |  
| `201`  | `+ `--kv-events-replay-port` a ROUTER socket answers vLLM-style replay`  |  
| `202`  | `+requests (`[b"", start_seq:8]` → the buffered `[topic, seq, payload]` `  |  
| `203`  | `+frames from `start_seq` on, then `[b"", -1:8, b""]`) from a bounded`  |  
| `204`  | `+ring of recent messages. Sends are non-blocking (PUB drops at its`  |  
| `205`  | `+high-water mark), so a slow router never stalls the bus drain thread.`  |  
| `207`  | `+Deliberately out of scope here, tracked in the RFC: engine identity from`  |  
| `208`  | `+ `REGISTER_KV_CACHE` (fan-out currently uses configured ids only), and`  |  
| `209`  | `+mounting the same sink on the coordinator's `POST /events` so shared L2`  |  
| `210`  | `+is credited fleet-wide once.`  |  
| `170`  | `212`  | `## Wiring and configuration`  |  
| `171`  | `213`  |  
| `172`  | `214`  | `Enabled in the MP HTTP server lifespan when a coordinator URL is set`  |  
| `173`  | `215`  | `and `--coordinator-event-reporting` (or`  |  
| `174`  | `- `LMCACHE_COORDINATOR_EVENT_REPORTING`) is on;`  |  
| `216`  | `+ `LMCACHE_COORDINATOR_EVENT_REPORTING`) is on, and/or when`  |  
| `217`  | `+ `--kv-events-endpoint` (or `LMCACHE_KV_EVENTS_ENDPOINT`) is set;`  |  
| `175`  | `218`  | ` `--coordinator-event-flush-interval` paces the subscriber's`  |  
| `176`  | `-event-driven flushes (default 1s).`  |  
| `219`  | `+event-driven flushes (default 1s). Both ride the observability bus, so`  |  
| `220`  | `+both are rejected with `--disable-observability`.`  |  
| `177`  | `221`  |  
| `178`  | `222`  | `## Known limitations (follow-ups)`  |  
| `179`  | `223`  |  
Collapse file
### [`‎docs/source/mp/coordinator.rst‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-3a3ea48805a37e37113721f7593226b68b4edd82b60a6e8b1050fed574939cef)
Copy file name to clipboardExpand all lines: docs/source/mp/coordinator.rst
+22Lines changed: 22 additions & 0 deletions
  * Display the source diff
  * Display the rich diff

  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -177,6 +177,28 @@ Kubernetes downward API); an explicit flag wins over the env var.` |  
| `177`  | `177`  | `     - ``LMCACHE_COORDINATOR_EVENT_FLUSH_INTERVAL```  |  
| `178`  | `178`  | `     - Seconds between cache-event batch flushes (must be ``> 0``, default`  |  
| `179`  | `179`  | `       ``1``).`  |  
| `180`  | `+   * - ``--kv-events-endpoint```  |  
| `181`  | `+     - ``LMCACHE_KV_EVENTS_ENDPOINT```  |  
| `182`  | `+     - ZMQ PUB bind address (e.g. ``tcp://*:5557``) on which the same cache`  |  
| `183`  | `+       events are re-emitted in vLLM KV-event wire format for KV-cache-aware`  |  
| `184`  | `+       routers such as llm-d. Works with or without a coordinator URL; unset`  |  
| `185`  | `+       disables it.`  |  
| `186`  | `+   * - ``--kv-events-emitter-ids```  |  
| `187`  | `+     - ``LMCACHE_KV_EVENTS_EMITTER_IDS```  |  
| `188`  | `+     - Comma-separated router identities the events are published under`  |  
| `189`  | `+       (topic ``kv@<id>@<model>``, one message per id). Default`  |  
| `190`  | `+       ``node:$NODE_NAME`` or ``node:<hostname>``.`  |  
| `191`  | `+   * - ``--kv-events-replay-port```  |  
| `192`  | `+     - ``LMCACHE_KV_EVENTS_REPLAY_PORT```  |  
| `193`  | `+     - TCP port of a ZMQ ROUTER socket answering vLLM-style replay requests`  |  
| `194`  | `+       from a ring of recent messages; ``0`` (default) disables replay.`  |  
| `196`  | `+The engine-format sink publishes ``BlockStored`` (with the chunk's token ids`  |  
| `197`  | `+and its predecessor as ``parent_block_hash``) for every L1/L2 store and`  |  
| `198`  | `+``BlockRemoved`` for every eviction/delete, under medium ``lmcache-l1`` or`  |  
| `199`  | `+``lmcache-l2-<backend>``; ``access`` and capacity events are not forwarded.`  |  
| `200`  | `+Like ``--coordinator-event-reporting`` it rides the observability event bus,`  |  
| `201`  | `+so it is rejected together with ``--disable-observability``.`  |  
| `180`  | `202`  |  
| `181`  | `203`  | `The server registers under its stable identity (``--instance-id`` / OTel`  |  
| `182`  | `204`  | ```service.instance.id``); if the flag is not passed, the server mints a`  |  
Collapse file
### [`‎lmcache/v1/mp_coordinator/api.py‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-83bce78c2b95176e003a94e826d8cef8428f35d3b4822c6cf00dbc0020610d48)
Copy file name to clipboardExpand all lines: lmcache/v1/mp_coordinator/api.py
+16-2Lines changed: 16 additions & 2 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -75,22 +75,36 @@ class CacheEventEntry:` |  
| `        token_offset: Token position of the chunk's first token in the`  |  
| `            sequence it was stored under; prefix-chained chunk hashes do`  |  
| `            not reveal it. :data:`UNKNOWN_TOKEN_OFFSET` when unreported.`  |  
| `+        parent_hash_hex: Hex-encoded chunk hash of the chunk stored`  |  
| `+            immediately before this one in the same sequence (``store```  |  
| `+            only); empty for a sequence's first chunk or when the emitter`  |  
| `+            does not know the predecessor. Lets engine-format KV event`  |  
| `+            consumers chain blocks the way vLLM's ``parent_block_hash```  |  
| `+            does.`  |  
 |  
| ` key: EncodedObjectKey `  |  
| ` size_bytes: int = 0 `  |  
| ` token_ids: list[int] = field(default_factory=list)`  |  
| ` token_offset: int = UNKNOWN_TOKEN_OFFSET `  |  
| `+ parent_hash_hex: str = "" `  |  
| ` def __post_init__(self) -> None:`  |  
| ` """Enforce intrinsic invariants. `  |  
| `        Raises:`  |  
| `-            ValueError: If ``size_bytes`` is negative, or ``token_offset```  |  
| `-                is negative and not :data:`UNKNOWN_TOKEN_OFFSET`.`  |  
| `+            ValueError: If ``size_bytes`` is negative, ``token_offset```  |  
| `+                is negative and not :data:`UNKNOWN_TOKEN_OFFSET`, or`  |  
| `+                ``parent_hash_hex`` is not valid hex.`  |  
 |  
| `100`  | ` if self.size_bytes  0:`  |  
| `101`  | ` raise ValueError(f"size_bytes must be >= 0 (got {self.size_bytes})")`  |  
| `102`  | `+ try:`  |  
| `103`  | `+ bytes.fromhex(self.parent_hash_hex)`  |  
| `104`  | `+ except ValueError as e:`  |  
| `105`  | `+ raise ValueError(`  |  
| `106`  | `+ f"parent_hash_hex must be hex (got {self.parent_hash_hex!r})" `  |  
| `107`  | `+            ) from e `  |  
| `108`  | ` if self.token_offset  0 and self.token_offset != UNKNOWN_TOKEN_OFFSET:`  |  
| `109`  | ` raise ValueError(`  |  
| `110`  | ` f"token_offset must be >= 0 or UNKNOWN_TOKEN_OFFSET " `  |  
Collapse file
### [`‎lmcache/v1/mp_coordinator/cache_events.py‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-8109fc206fe5852461cc6d37d4d86729ec36508fe626d09548b094062cc30a22)
Copy file name to clipboardExpand all lines: lmcache/v1/mp_coordinator/cache_events.py
+54-1Lines changed: 54 additions & 1 deletion  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -117,6 +117,51 @@ def close(self) -> None:` |  
| `117`  | `117`  | ` self._client.close()`  |  
| `118`  | `118`  |  
| `119`  | `119`  |  
| `120`  | `+ class CompositeCacheEventSink(CacheEventSink):`  |  
| `121`  | `+ """Sink that fans every publish out to several sinks. `  |  
| `123`  | `+    Each sink is attempted even when an earlier one fails, so one broken`  |  
| `124`  | `+    transport (say, an unreachable coordinator) does not silence the`  |  
| `125`  | `+    others. Failures are logged and re-raised together at the end.`  |  
| `127`  | `+    Args:`  |  
| `128`  | `+        sinks: The sinks to deliver to, in order. Must be non-empty.`  |  
| `130`  | `+    Raises:`  |  
| `131`  | `+        ValueError: If ``sinks`` is empty.`  |  
| `132`  |  
| `134`  | `+ def __init__(self, sinks: list[CacheEventSink]) -> None:`  |  
| `135`  | `+ if not sinks:`  |  
| `136`  | `+ raise ValueError("CompositeCacheEventSink needs at least one sink")`  |  
| `137`  | `+ self._sinks = list(sinks)`  |  
| `139`  | `+ def publish(self, batches: list[CacheEventBatch]) -> None:`  |  
| `140`  | `+ """Deliver ``batches`` to every sink. `  |  
| `142`  | `+        Args:`  |  
| `143`  | `+            batches: The batches to deliver.`  |  
| `145`  | `+        Raises:`  |  
| `146`  | `+            CacheEventPublishError: If any sink failed (after all were`  |  
| `147`  | `+                attempted).`  |  
| `148`  |  
| `149`  | `+ failures: list[str] = []`  |  
| `150`  | `+ for sink in self._sinks:`  |  
| `151`  | `+ try:`  |  
| `152`  | `+ sink.publish(batches)`  |  
| `153`  | `+ except CacheEventPublishError as e:`  |  
| `154`  | `+ logger.warning("%s failed to publish: %s", type(sink).__name__, e)`  |  
| `155`  | `+ failures.append(f"{type(sink).__name__}: ")`  |  
| `156`  | `+ if failures:`  |  
| `157`  | `+ raise CacheEventPublishError("; ".join(failures))`  |  
| `159`  | `+ def close(self) -> None:`  |  
| `160`  | `+ """Close every sink.""" `  |  
| `161`  | `+ for sink in self._sinks:`  |  
| `162`  | `+ sink.close()`  |  
| `120`  | `165`  | `@dataclass(frozen=True)`  |  
| `121`  | `166`  | ` class _ChunkTokens:`  |  
| `122`  | `167`  | ` """One chunk's token content, held between its token-binding event `  |  
| `@@ -125,10 +170,15 @@ class _ChunkTokens:` |  
| `125`  | `170`  | `    Attributes:`  |  
| `126`  | `171`  | `        token_ids: The chunk's token ids.`  |  
| `127`  | `172`  | `        token_offset: Position of its first token in the stored sequence.`  |  
| `173`  | `+        parent_hash: Chunk hash of the chunk preceding it in the same`  |  
| `174`  | `+            token-binding event; empty for the event's first chunk (the`  |  
| `175`  | `+            sequence start, or a predecessor stored by an earlier request`  |  
| `176`  | `+            and therefore unknown here).`  |  
| `128`  | `177`  |  
| `129`  | `178`  |  
| `130`  | `179`  | ` token_ids: tuple[int, ...]`  |  
| `131`  | `180`  | ` token_offset: int `  |  
| `181`  | `+ parent_hash: bytes = b"" `  |  
| `132`  | `182`  |  
| `133`  | `183`  |  
| `134`  | `184`  | `# Stands in for a chunk the binding cache does not know, so a STORE entry`  |  
| `@@ -351,13 +401,15 @@ def _on_tokens(self, event: Event) -> None:` |  
| `351`  | `401`  | ` chunk_hashes: list[bytes] = event.metadata["chunk_hashes"]`  |  
| `352`  | `402`  | ` token_chunks: list[list[int]] = event.metadata["token_chunks"]`  |  
| `353`  | `403`  | ` token_offsets: list[int] = event.metadata["token_offsets"]`  |  
| `404`  | `+ parent_hash = b"" `  |  
| `354`  | `405`  | ` for chunk_hash, chunk, offset in zip(`  |  
| `355`  | `406`  | ` chunk_hashes, token_chunks, token_offsets, strict=True `  |  
| `356`  | `407`  |  
| `357`  | `408`  | ` self._token_bindings[chunk_hash] = _ChunkTokens(`  |  
| `358`  | `- token_ids=tuple(chunk), token_offset=offset `  |  
| `409`  | `+ token_ids=tuple(chunk), token_offset=offset, parent_hash=parent_hash `  |  
| `359`  | `410`  |  
| `360`  | `411`  | ` self._token_bindings.move_to_end(chunk_hash)`  |  
| `412`  | `+ parent_hash = chunk_hash `  |  
| `361`  | `413`  | ` if len(self._token_bindings) <= _TOKEN_BINDING_CACHE_SIZE:`  |  
| `362`  | `414`  | ` return `  |  
| `363`  | `415`  | ` # Evict in one batch down to half the bound: the next eviction is `  |  
| `@@ -419,6 +471,7 @@ def _store_entry(self, key: ObjectKey, size_bytes: int) -> CacheEventEntry:` |  
| `419`  | `471`  | ` size_bytes=size_bytes,`  |  
| `420`  | `472`  | ` token_ids=list(binding.token_ids),`  |  
| `421`  | `473`  | ` token_offset=binding.token_offset,`  |  
| `474`  | `+ parent_hash_hex=binding.parent_hash.hex(),`  |  
| `422`  | `475`  |  
| `423`  | `476`  |  
| `424`  | `477`  | ` def _record(`  |  
Collapse file
### [`‎lmcache/v1/mp_coordinator/kv_event_sink.py‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-82186b7b64b04d4ee823fca4863206e94c66cf58e661c55278b8156d9d38b045)
Copy file name to clipboard
+302Lines changed: 302 additions & 0 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -0,0 +1,302 @@` |  
| `+# SPDX-License-Identifier: Apache-2.0`  |  
| `+"""Engine-format KV event sink: re-emits the MP server's cache events in`  |  
| `+vLLM's ``KVEventBatch`` wire format over ZMQ.`  |  
| `+KV-cache-aware routers (llm-d's EPP, for one) index engine KV events`  |  
| `+they read from a ZMQ PUB socket: msgpack ``[ts, [event, ...]]`` batches`  |  
| `+under a ``kv@<emitter_id>@<model_name>`` topic. This sink translates`  |  
| `+:class:`CacheEventBatch` lists into that format so an unmodified vLLM`  |  
| `+adapter indexes LMCache's L1/L2 tiers next to the engines' GPU tier. See`  |  
| `+``docs/design/v1/mp_coordinator/cache_events.md`` ("Engine-format KV`  |  
| `+event sink").`  |  
 |  
| `+# Standard`  |  
| `+ from collections import deque `  |  
| `+ import struct `  |  
| `+ import threading `  |  
| `+# Third Party`  |  
| `+ import msgspec `  |  
| `+ import zmq `  |  
| `+# First Party`  |  
| `+ from lmcache.logging import init_logger `  |  
| `+ from lmcache.v1.distributed.api import Tier `  |  
| `+ from lmcache.v1.mp_coordinator.api import (`  |  
| `+ CacheEventBatch,`  |  
| `+ CacheEventEntry,`  |  
| `+ CacheEventType,`  |  
| `+ from lmcache.v1.mp_coordinator.cache_events import (`  |  
| `+ CacheEventPublishError,`  |  
| `+ CacheEventSink,`  |  
| `+ logger = init_logger(__name__)`  |  
| `+# vLLM's default ZMQ send high-water mark for KV events.`  |  
| `+ _DEFAULT_HWM = 100_000 `  |  
| `+# vLLM's default replay ring depth (batches).`  |  
| `+ _DEFAULT_REPLAY_DEPTH = 10_000 `  |  
| `+# End-of-replay marker: vLLM sends seq ``-1`` as 8 signed big-endian bytes.`  |  
| `+ _END_OF_REPLAY_SEQ = struct.pack(">q", -1)`  |  
| `+ _REPLAY_POLL_INTERVAL_MS = 100 `  |  
| `+ def medium_for(tier: Tier, backend: str) -> str:`  |  
| `+ """Return the ``medium`` string a placement in ``tier``/``backend`` is `  |  
| `+    published under.`  |  
| `+    Stable between a placement's ``BlockStored`` and ``BlockRemoved```  |  
| `+    (routers refcount per medium) and distinct from vLLM's own ``gpu`` /`  |  
| `+    ``cpu`` tiers so weights can be configured per LMCache tier.`  |  
| `+    Args:`  |  
| `+        tier: The placement's tier (``l1`` or ``l2``).`  |  
| `+        backend: The batch's backend name (``"dram"``, ``"fs"``, ...).`  |  
| `+    Returns:`  |  
| `+        ``"lmcache-l1"`` for L1, ``"lmcache-l2-<backend>"`` for L2.`  |  
 |  
| `+ if tier is Tier.L1:`  |  
| `+ return "lmcache-l1" `  |  
| `+ return f"lmcache-l2-{backend.lower()}" `  |  
| `+ def encode_batch(`  |  
| `+ batch: CacheEventBatch,`  |  
| `+) -> tuple[str, bytes]:`  |  
| `+ """Translate one :class:`CacheEventBatch` into a vLLM ``KVEventBatch``. `  |  
| `+    ``STORE`` entries become ``BlockStored`` events (one per entry, so`  |  
| `+    each carries its own ``parent_block_hash`` and ``token_ids``);`  |  
| `+    ``DELETE`` entries become one ``BlockRemoved`` event. Tokenless`  |  
| `+    ``STORE`` entries (the emitter no longer held the chunk's token`  |  
| `+    binding) are skipped: llm-d recomputes its own keys from the tokens`  |  
| `+    and cannot index an unknown hash without them. ``DELETE`` entries are`  |  
| `+    never skipped, so a removal always reaches the router even when its`  |  
| `+    store did not. HMA fields are never set.`  |  
| `+    Args:`  |  
| `+        batch: A ``store`` or ``delete`` batch.`  |  
| `+    Returns:`  |  
| `+        ``(model_name, payload)`` where ``payload`` is the msgpack-encoded`  |  
| `+        ``[ts, events]`` batch; ``payload`` is empty when nothing is`  |  
| `+        publishable (e.g. every entry was tokenless).`  |  
| `+    Raises:`  |  
| `+        ValueError: If ``batch`` is not a ``store``/``delete`` batch, has`  |  
| `+            no entries, or its entries span several model names (a topic`  |  
| `+            names exactly one model).`  |  
 |  
| `+ if batch.event_type  (CacheEventType.STORE, CacheEventType.DELETE):`  |  
| `+ raise ValueError(f"cannot encode {batch.event_type.value} batches")`  |  
| `+ if not batch.entries:`  |  
| `+ raise ValueError("cannot encode an empty batch")`  |  
| `+ model_names = {entry.key.model_name for entry in batch.entries}`  |  
| `+ if len(model_names) != 1:`  |  
| `100`  | `+ raise ValueError(f"batch spans several models: {sorted(model_names)}")`  |  
| `101`  | `+ medium = medium_for(batch.tier, batch.backend)`  |  
| `102`  | `+ events: list[list[object]] = []`  |  
| `103`  | `+ if batch.event_type is CacheEventType.STORE:`  |  
| `104`  | `+ for entry in batch.entries:`  |  
| `105`  | `+ if not entry.token_ids:`  |  
| `106`  | `+ continue `  |  
| `107`  | `+ events.append(_block_stored(entry, medium))`  |  
| `108`  | `+ else:`  |  
| `109`  | `+ events.append(`  |  
| `110`  |  
| `111`  | `+ "BlockRemoved",`  |  
| `112`  | `+                [bytes.fromhex(entry.key.chunk_hash_hex) for entry in batch.entries],`  |  
| `113`  | `+ medium,`  |  
| `114`  |  
| `115`  |  
| `116`  | `+ if not events:`  |  
| `117`  | `+ return model_names.pop(), b"" `  |  
| `118`  | `+ return model_names.pop(), msgspec.msgpack.encode([batch.ts, events])`  |  
| `121`  | `+ def _block_stored(entry: CacheEventEntry, medium: str) -> list[object]:`  |  
| `122`  | `+ """Lay out one ``BlockStored`` event in vLLM's positional order: `  |  
| `123`  | `+    ``[tag, block_hashes, parent_block_hash, token_ids, block_size,`  |  
| `124`  | `+    lora_id, medium]``."""`  |  
| `125`  | `+ parent = bytes.fromhex(entry.parent_hash_hex) if entry.parent_hash_hex else None `  |  
| `126`  | `+ return [`  |  
| `127`  | `+ "BlockStored",`  |  
| `128`  | `+        [bytes.fromhex(entry.key.chunk_hash_hex)],`  |  
| `129`  | `+ parent,`  |  
| `130`  | `+ list(entry.token_ids),`  |  
| `131`  | `+ len(entry.token_ids),`  |  
| `132`  | `+ None,`  |  
| `133`  | `+ medium,`  |  
| `134`  |  
| `137`  | `+ class ZmqKVEventSink(CacheEventSink):`  |  
| `138`  | `+ """Sink that publishes cache events as vLLM KV events on a ZMQ PUB socket. `  |  
| `140`  | `+    Every publishable batch is sent once per emitter id (fan-out) as a`  |  
| `141`  | `+    3-frame message ``[topic, seq, payload]`` with ``seq`` an 8-byte`  |  
| `142`  | `+    big-endian counter shared by all topics (routers detect gaps per`  |  
| `143`  | `+    endpoint, not per topic). ``ACCESS`` and ``CONFIG`` batches are not`  |  
| `144`  | `+    forwarded. Sends never block: the PUB socket drops at its high-water`  |  
| `145`  | `+    mark, so a slow router cannot stall the event bus's drain thread.`  |  
| `147`  | `+    With a ``replay_endpoint``, a ROUTER socket answers vLLM-style replay`  |  
| `148`  | `+    requests (one frame holding the 8-byte start seq) from a bounded ring`  |  
| `149`  | `+    of recently sent messages, in seq order, followed by an end marker.`  |  
| `151`  | `+    Args:`  |  
| `152`  | `+        endpoint: ZMQ bind address for the PUB socket (``tcp://*:5557``).`  |  
| `153`  | `+        emitter_ids: Router identities to publish under (topic segment`  |  
| `154`  | `+            two); one message per id per batch. Must be non-empty.`  |  
| `155`  | `+        replay_endpoint: ZMQ bind address for the replay ROUTER socket;`  |  
| `156`  | `+            empty disables replay.`  |  
| `157`  | `+        replay_depth: Number of most recent messages kept for replay.`  |  
| `158`  | `+        hwm: PUB send high-water mark (messages).`  |  
| `160`  | `+    Raises:`  |  
| `161`  | `+        ValueError: If ``emitter_ids`` is empty or contains ``@``, or`  |  
| `162`  | `+            ``replay_depth`` / ``hwm`` is not positive.`  |  
| `163`  |  
| `165`  | `+ def __init__(`  |  
| `166`  | `+ self,`  |  
| `167`  | `+ endpoint: str,`  |  
| `168`  | `+ emitter_ids: list[str],`  |  
| `169`  | `+ replay_endpoint: str = "",`  |  
| `170`  | `+ replay_depth: int = _DEFAULT_REPLAY_DEPTH,`  |  
| `171`  | `+ hwm: int = _DEFAULT_HWM,`  |  
| `172`  | `+    ) -> None:`  |  
| `173`  | `+ if not emitter_ids:`  |  
| `174`  | `+ raise ValueError("emitter_ids must be non-empty")`  |  
| `175`  | `+ if any("@" in emitter_id or not emitter_id for emitter_id in emitter_ids):`  |  
| `176`  | `+ raise ValueError(`  |  
| `177`  | `+ f"emitter ids must be non-empty and free of '@' (got {emitter_ids})" `  |  
| `178`  |  
| `179`  | `+ if replay_depth  1:`  |  
| `180`  | `+ raise ValueError(f"replay_depth must be >= 1 (got {replay_depth})")`  |  
| `181`  | `+ if hwm  1:`  |  
| `182`  | `+ raise ValueError(f"hwm must be >= 1 (got )")`  |  
| `183`  | `+ self._emitter_ids = list(emitter_ids)`  |  
| `184`  | `+ self._ctx = zmq.Context()`  |  
| `185`  | `+ self._pub = self._ctx.socket(zmq.PUB)`  |  
| `186`  | `+ self._pub.setsockopt(zmq.LINGER, 0)`  |  
| `187`  | `+ self._pub.setsockopt(zmq.SNDHWM, hwm)`  |  
| `188`  | `+ self._pub.bind(endpoint)`  |  
| `189`  | `+ # vLLM numbers messages from 0; llm-d accepts a first live seq of 0 `  |  
| `190`  | `+ # without requesting a full replay. `  |  
| `191`  | `+ self._seq = 0 `  |  
| `192`  | `+ # Recently sent frames for replay, shared with the replay thread. `  |  
| `193`  | `+ self._replay_buffer: deque[tuple[int, list[bytes]]] = deque(maxlen=replay_depth)`  |  
| `194`  | `+ self._lock = threading.Lock()`  |  
| `195`  | `+ self._stop = threading.Event()`  |  
| `196`  | `+ self._replay_thread: threading.Thread | None = None `  |  
| `197`  | `+ self._router: zmq.Socket | None = None `  |  
| `198`  | `+ if replay_endpoint:`  |  
| `199`  | `+ self._router = self._ctx.socket(zmq.ROUTER)`  |  
| `200`  | `+ self._router.setsockopt(zmq.LINGER, 0)`  |  
| `201`  | `+ self._router.bind(replay_endpoint)`  |  
| `202`  | `+ self._replay_thread = threading.Thread(`  |  
| `203`  | `+ target=self._serve_replay, name="kv-events-replay", daemon=True `  |  
| `204`  |  
| `205`  | `+ self._replay_thread.start()`  |  
| `206`  | `+ logger.info(`  |  
| `207`  | `+ "KV event sink bound to %s (emitters %s, replay %s)",`  |  
| `208`  | `+ endpoint,`  |  
| `209`  | `+ self._emitter_ids,`  |  
| `210`  | `+ replay_endpoint or "off",`  |  
| `211`  |  
| `213`  | `+ @property `  |  
| `214`  | `+ def endpoint(self) -> str:`  |  
| `215`  | `+ """The PUB socket's bound endpoint (resolved, so a ``*`` port is `  |  
| `216`  | `+        the actual one)."""`  |  
| `217`  | `+ return self._pub.getsockopt_string(zmq.LAST_ENDPOINT)`  |  
| `219`  | `+ @property `  |  
| `220`  | `+ def replay_endpoint(self) -> str:`  |  
| `221`  | `+ """The replay ROUTER socket's bound endpoint; empty when replay is `  |  
| `222`  | `+        disabled."""`  |  
| `223`  | `+ if self._router is None:`  |  
| `224`  | `+ return "" `  |  
| `225`  | `+ return self._router.getsockopt_string(zmq.LAST_ENDPOINT)`  |  
| `227`  | `+ def publish(self, batches: list[CacheEventBatch]) -> None:`  |  
| `228`  | `+ """Publish ``batches`` as vLLM KV event batches, in list order. `  |  
| `230`  | `+        Args:`  |  
| `231`  | `+            batches: The batches to deliver; non-placement batches`  |  
| `232`  | `+                (``access``, ``config``) are ignored.`  |  
| `234`  | `+        Raises:`  |  
| `235`  | `+            CacheEventPublishError: If the socket is closed or a send fails.`  |  
| `236`  |  
| `237`  | `+ for batch in batches:`  |  
| `238`  | `+ if batch.event_type  (CacheEventType.STORE, CacheEventType.DELETE):`  |  
| `239`  | `+ continue `  |  
| `240`  | `+ model_name, payload = encode_batch(batch)`  |  
| `241`  | `+ if not payload:`  |  
| `242`  | `+ continue `  |  
| `243`  | `+ for emitter_id in self._emitter_ids:`  |  
| `244`  | `+ self._send(f"kv@{emitter_id}@{model_name}".encode(), payload)`  |  
| `246`  | `+ def _send(self, topic: bytes, payload: bytes) -> None:`  |  
| `247`  | `+ with self._lock:`  |  
| `248`  | `+ if self._pub.closed:`  |  
| `249`  | `+ raise CacheEventPublishError("KV event sink is closed")`  |  
| `250`  | `+ frames = [topic, struct.pack(">Q", self._seq), payload]`  |  
| `251`  | `+ try:`  |  
| `252`  | `+ self._pub.send_multipart(frames, flags=zmq.NOBLOCK)`  |  
| `253`  | `+ except zmq.ZMQError as e:`  |  
| `254`  | `+ raise CacheEventPublishError(f"KV event send failed: ") from e `  |  
| `255`  | `+ self._replay_buffer.append((self._seq, frames))`  |  
| `256`  | `+ self._seq += 1 `  |  
| `258`  | `+ def _serve_replay(self) -> None:`  |  
| `259`  | `+ """Replay thread: answer each request with every buffered message `  |  
| `260`  | `+        whose seq is >= the requested start, then the end marker."""`  |  
| `261`  | `+ router = self._router `  |  
| `262`  | `+ if router is None:`  |  
| `263`  | `+ return `  |  
| `264`  | `+ poller = zmq.Poller()`  |  
| `265`  | `+ poller.register(router, zmq.POLLIN)`  |  
| `266`  | `+ while not self._stop.is_set():`  |  
| `267`  | `+ if not poller.poll(_REPLAY_POLL_INTERVAL_MS):`  |  
| `268`  | `+ continue `  |  
| `269`  | `+ try:`  |  
| `270`  | `+ request = router.recv_multipart()`  |  
| `271`  | `+ except zmq.ZMQError:`  |  
| `272`  | `+ return `  |  
| `273`  | `+ # DEALER request ``[b"", seq8]`` arrives as ``[identity, b"", seq8]``. `  |  
| `274`  | `+ if len(request)  2 or len(request[-1]) != 8:`  |  
| `275`  | `+ logger.warning("Ignoring malformed KV event replay request")`  |  
| `276`  | `+ continue `  |  
| `277`  | `+ identity = request[0]`  |  
| `278`  | `+ start_seq = struct.unpack(">Q", request[-1])[0]`  |  
| `279`  | `+ with self._lock:`  |  
| `280`  | `+ replay = [`  |  
| `281`  | `+ frames for seq, frames in self._replay_buffer if seq >= start_seq `  |  
| `282`  |  
| `283`  | `+ try:`  |  
| `284`  | `+ for frames in replay:`  |  
| `285`  | `+ router.send_multipart([identity, b"", *frames])`  |  
| `286`  | `+ router.send_multipart([identity, b"", b"", _END_OF_REPLAY_SEQ, b""])`  |  
| `287`  | `+ except zmq.ZMQError as e:`  |  
| `288`  | `+ logger.warning("KV event replay to a subscriber failed: %s", e)`  |  
| `290`  | `+ def close(self) -> None:`  |  
| `291`  | `+ """Close the sockets and stop the replay thread. Idempotent.""" `  |  
| `292`  | `+ self._stop.set()`  |  
| `293`  | `+ if self._replay_thread  None:`  |  
| `294`  | `+ self._replay_thread.join()`  |  
| `295`  | `+ self._replay_thread = None `  |  
| `296`  | `+ if self._router  None:`  |  
| `297`  | `+ self._router.close()`  |  
| `298`  | `+ self._router = None `  |  
| `299`  | `+ with self._lock:`  |  
| `300`  | `+ self._pub.close()`  |  
| `301`  | `+ if not self._ctx.closed:`  |  
| `302`  | `+ self._ctx.term()`  |  
Collapse file
### [`‎lmcache/v1/multiprocess/config.py‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-8c191ce38e4ad5f8861703f014284cb38e229aa95b384f8eee9db381928bed4f)
Copy file name to clipboardExpand all lines: lmcache/v1/multiprocess/config.py
+91-2Lines changed: 91 additions & 2 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -11,6 +11,7 @@` |  
| ` import json `  |  
| ` import math `  |  
| ` import os `  |  
| `+ import socket `  |  
| ` import uuid `  |  
| `# First Party`  |  
| `@@ -242,10 +243,35 @@ class CoordinatorConfig:` |  
| `242`  | `243`  | ` """Max fleet CacheBlend match round-trips the blend coordinator client keeps `  |  
| `243`  | `244`  | `    in flight at once. Must be strictly positive."""`  |  
| `244`  | `245`  |  
| `246`  | `+ kv_events_endpoint: str = "" `  |  
| `247`  | `+ """ZMQ PUB bind address (e.g. ``tcp://*:5557``) on which cache events are `  |  
| `248`  | `+    re-emitted in vLLM KV-event wire format for KV-cache-aware routers. Empty`  |  
| `249`  | `+    disables the engine-format sink. Rides the same cache-event stream as`  |  
| `250`  | `+    :attr:`event_reporting`, so it also requires the observability bus."""`  |  
| `252`  | `+ kv_events_emitter_ids: list[str] = field(default_factory=list)`  |  
| `253`  | `+ """Router identities the KV events are published under (topic `  |  
| `254`  | `+    ``kv@<emitter_id>@<model>``), one message per id. Empty resolves to`  |  
| `255`  | `+    ``node:$NODE_NAME`` (or ``node:<hostname>``) at parse time."""`  |  
| `257`  | `+ kv_events_replay_port: int = 0 `  |  
| `258`  | `+ """TCP port of the ZMQ ROUTER socket answering KV-event replay requests `  |  
| `259`  | `+    (``tcp://*:<port>``); ``0`` disables replay."""`  |  
| `245`  | `261`  |  
| `246`  | `262`  | ` DEFAULT_COORDINATOR_CONFIG = CoordinatorConfig()`  |  
| `247`  | `263`  |  
| `248`  | `264`  |  
| `265`  | `+ def default_kv_events_emitter_id() -> str:`  |  
| `266`  | `+ """Return the default router identity for engine-format KV events. `  |  
| `268`  | `+    Returns:`  |  
| `269`  | `+        ``node:$NODE_NAME`` when ``NODE_NAME`` is set (the Kubernetes`  |  
| `270`  | `+        downward API convention), else ``node:<hostname>``.`  |  
| `271`  |  
| `272`  | `+ return "node:" + (os.getenv("NODE_NAME") or socket.gethostname())`  |  
| `249`  | `275`  | ` def add_mp_server_args(`  |  
| `250`  | `276`  | ` parser: argparse.ArgumentParser,`  |  
| `251`  | `277`  | `) -> argparse.ArgumentParser:`  |  
| `@@ -660,6 +686,36 @@ def add_coordinator_args(` |  
| `660`  | `686`  | ` # (operator <= v0.5.2, charts) still render these names into server args, `  |  
| `661`  | `687`  | ` # and rejecting them crashes the pod on startup. Remove once those `  |  
| `662`  | `688`  | ` # deployers are out of the support matrix. `  |  
| `689`  | `+ kv_group = parser.add_argument_group(`  |  
| `690`  | `+ "KV events",`  |  
| `691`  | `+ "Re-emit cache events in vLLM KV-event wire format (ZMQ PUB) for " `  |  
| `692`  | `+ "KV-cache-aware routers such as llm-d",`  |  
| `693`  |  
| `694`  | `+ kv_group.add_argument(`  |  
| `695`  | `+ "--kv-events-endpoint",`  |  
| `696`  | `+ type=str,`  |  
| `697`  | `+ default=None,`  |  
| `698`  | `+ help="ZMQ PUB bind address for engine-format KV events (e.g. " `  |  
| `699`  | `+ "tcp://*:5557). Defaults to LMCACHE_KV_EVENTS_ENDPOINT; unset " `  |  
| `700`  | `+ "disables the sink. Requires the observability event bus.",`  |  
| `701`  |  
| `702`  | `+ kv_group.add_argument(`  |  
| `703`  | `+ "--kv-events-emitter-ids",`  |  
| `704`  | `+ type=str,`  |  
| `705`  | `+ default=None,`  |  
| `706`  | `+ help="Comma-separated router identities to publish under (topic " `  |  
| `707`  | `+ "kv@<id>@<model>; one message per id). Defaults to " `  |  
| `708`  | `+ "LMCACHE_KV_EVENTS_EMITTER_IDS, then node:$NODE_NAME or " `  |  
| `709`  | `+ "node:<hostname>.",`  |  
| `710`  |  
| `711`  | `+ kv_group.add_argument(`  |  
| `712`  | `+ "--kv-events-replay-port",`  |  
| `713`  | `+ type=int,`  |  
| `714`  | `+ default=None,`  |  
| `715`  | `+ help="TCP port of the ZMQ ROUTER socket answering KV-event replay " `  |  
| `716`  | `+ "requests. Defaults to LMCACHE_KV_EVENTS_REPLAY_PORT; 0 disables " `  |  
| `717`  | `+ "replay.",`  |  
| `718`  |  
| `663`  | `719`  | ` group.add_argument(`  |  
| `664`  | `720`  | ` "--coordinator-l2-event-reporting",`  |  
| `665`  | `721`  | ` dest="coordinator_l2_event_reporting",`  |  
| `@@ -701,8 +757,10 @@ def parse_args_to_coordinator_config(` |  
| `701`  | `757`  |  
| `702`  | `758`  | `    Raises:`  |  
| `703`  | `759`  | `        ValueError: If the heartbeat interval, the event flush interval or the`  |  
| `704`  | `-            blend timeout is not a positive finite number, or if the blend`  |  
| `705`  | `-            match concurrency is less than 1.`  |  
| `760`  | `+            blend timeout is not a positive finite number, if the blend`  |  
| `761`  | `+            match concurrency is less than 1, if a KV-events emitter id is`  |  
| `762`  | `+            empty or contains ``@``, or if the KV-events replay port is`  |  
| `763`  | `+            outside ``[0, 65535]``.`  |  
| `706`  | `764`  |  
| `707`  | `765`  | ` url = (`  |  
| `708`  | `766`  | ` args.coordinator_url `  |  
| `@@ -789,6 +847,34 @@ def parse_args_to_coordinator_config(` |  
| `789`  | `847`  | ` "got %s" % blend_match_concurrency `  |  
| `790`  | `848`  |  
| `791`  | `849`  |  
| `850`  | `+ kv_events_endpoint = (`  |  
| `851`  | `+ args.kv_events_endpoint `  |  
| `852`  | `+ if args.kv_events_endpoint  None `  |  
| `853`  | `+ else os.getenv("LMCACHE_KV_EVENTS_ENDPOINT", "")`  |  
| `854`  |  
| `855`  | `+ raw_emitter_ids = (`  |  
| `856`  | `+ args.kv_events_emitter_ids `  |  
| `857`  | `+ if args.kv_events_emitter_ids  None `  |  
| `858`  | `+ else os.getenv("LMCACHE_KV_EVENTS_EMITTER_IDS", "")`  |  
| `859`  |  
| `860`  | `+ kv_events_emitter_ids = [`  |  
| `861`  | `+ part.strip() for part in raw_emitter_ids.split(",") if part.strip()`  |  
| `862`  | `+    ] or [default_kv_events_emitter_id()]`  |  
| `863`  | `+ if any("@" in emitter_id for emitter_id in kv_events_emitter_ids):`  |  
| `864`  | `+ raise ValueError(`  |  
| `865`  | `+ "kv-events emitter ids must not contain '@' " `  |  
| `866`  | `+ "(it separates topic segments), got %s" % kv_events_emitter_ids `  |  
| `867`  |  
| `868`  | `+ if args.kv_events_replay_port  None:`  |  
| `869`  | `+ kv_events_replay_port = args.kv_events_replay_port `  |  
| `870`  | `+ else:`  |  
| `871`  | `+ kv_events_replay_port = int(os.getenv("LMCACHE_KV_EVENTS_REPLAY_PORT", "0"))`  |  
| `872`  | `+ if not 0 <= kv_events_replay_port <= 65535:`  |  
| `873`  | `+ raise ValueError(`  |  
| `874`  | `+ "kv-events replay port must be in [0, 65535], got %s" `  |  
| `875`  | `+ % kv_events_replay_port `  |  
| `876`  |  
| `792`  | `878`  | ` return CoordinatorConfig(`  |  
| `793`  | `879`  | ` url=url,`  |  
| `794`  | `880`  | ` advertise_ip=advertise_ip,`  |  
| `@@ -797,4 +883,7 @@ def parse_args_to_coordinator_config(` |  
| `797`  | `883`  | ` event_flush_interval=event_flush_interval,`  |  
| `798`  | `884`  | ` blend_timeout=blend_timeout,`  |  
| `799`  | `885`  | ` blend_match_concurrency=blend_match_concurrency,`  |  
| `886`  | `+ kv_events_endpoint=kv_events_endpoint,`  |  
| `887`  | `+ kv_events_emitter_ids=kv_events_emitter_ids,`  |  
| `888`  | `+ kv_events_replay_port=kv_events_replay_port,`  |  
| `800`  | `889`  |  
Collapse file
### [`‎lmcache/v1/multiprocess/http_server.py‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-34b7e6e18937890ebc16f119680dd26d8e1ed5487c171a619e963fcada451027)
Copy file name to clipboardExpand all lines: lmcache/v1/multiprocess/http_server.py
+34-12Lines changed: 34 additions & 12 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -21,9 +21,12 @@` |  
| ` parse_args_to_config,`  |  
 |  
| ` from lmcache.v1.mp_coordinator.cache_events import (`  |  
| `+ CacheEventSink,`  |  
| ` CacheEventSubscriber,`  |  
| `+ CompositeCacheEventSink,`  |  
| ` HttpCacheEventSink,`  |  
 |  
| `+ from lmcache.v1.mp_coordinator.kv_event_sink import ZmqKVEventSink `  |  
| ` from lmcache.v1.mp_coordinator.registrar import keep_registered `  |  
| ` from lmcache.v1.mp_observability.config import (`  |  
| ` ObservabilityConfig,`  |  
| `@@ -40,6 +43,7 @@` |  
| ` add_http_frontend_args,`  |  
| ` add_mp_server_args,`  |  
| ` add_p2p_args,`  |  
| `+ default_kv_events_emitter_id,`  |  
| ` parse_args_to_coordinator_config,`  |  
| ` parse_args_to_http_frontend_config,`  |  
| ` parse_args_to_mp_server_config,`  |  
| `@@ -141,15 +145,29 @@ async def lifespan(app: FastAPI):` |  
| `141`  | `145`  | ` on_registered=engine.storage_manager.publish_capacity,`  |  
| `142`  | `146`  |  
| `143`  | `147`  |  
| `144`  | `- # Optionally report cache events to the coordinator `  |  
| `145`  |  
| `146`  | `- coordinator_client  None `  |  
| `147`  | `- and coordinator_config.url `  |  
| `148`  | `- and coordinator_config.event_reporting `  |  
| `149`  |  
| `148`  | `+ # Optionally stream cache events: to the coordinator's key directory `  |  
| `149`  | `+ # and/or, in engine KV-event format, to KV-cache-aware routers. One `  |  
| `150`  | `+ # subscriber feeds every configured sink. `  |  
| `151`  | `+ sinks: list[CacheEventSink] = []`  |  
| `152`  | `+ if coordinator_client  None and coordinator_config.event_reporting:`  |  
| `153`  | `+ sinks.append(HttpCacheEventSink(coordinator_config.url))`  |  
| `154`  | `+ if coordinator_config.kv_events_endpoint:`  |  
| `155`  | `+ sinks.append(`  |  
| `156`  | `+ ZmqKVEventSink(`  |  
| `157`  | `+ endpoint=coordinator_config.kv_events_endpoint,`  |  
| `158`  | `+ emitter_ids=coordinator_config.kv_events_emitter_ids `  |  
| `159`  | `+ or [default_kv_events_emitter_id()],`  |  
| `160`  | `+ replay_endpoint=(`  |  
| `161`  | `+ f"tcp://*:{coordinator_config.kv_events_replay_port}" `  |  
| `162`  | `+ if coordinator_config.kv_events_replay_port `  |  
| `163`  | `+ else "" `  |  
| `164`  |  
| `165`  |  
| `166`  |  
| `167`  | `+ if sinks:`  |  
| `150`  | `168`  | ` get_event_bus().register_subscriber(`  |  
| `151`  | `169`  | ` CacheEventSubscriber(`  |  
| `152`  | `- sink=HttpCacheEventSink(coordinator_config.url),`  |  
| `170`  | `+ sink=sinks[0] iflen(sinks) ==1elseCompositeCacheEventSink(sinks),`  |  
| `153`  | `171`  | ` instance_id=mp_config.instance_id,`  |  
| `154`  | `172`  | ` # Server start time: fences out placements this instance `  |  
| `155`  | `173`  | ` # reported before a restart (its pools restarted empty). `  |  
| `@@ -219,8 +237,9 @@ def run_http_server(` |  
| `219`  | `237`  | `    Raises:`  |  
| `220`  | `238`  | `        ValueError: If P2P is enabled without a coordinator URL, or with an L1`  |  
| `221`  | `239`  | `            tier that is not a single registerable memory region; or if`  |  
| `222`  | `-            coordinator event reporting is enabled with observability`  |  
| `223`  | `-            disabled (the cache-event stream rides the event bus).`  |  
| `240`  | `+            coordinator event reporting or the KV-events endpoint is enabled`  |  
| `241`  | `+            with observability disabled (the cache-event stream rides the`  |  
| `242`  | `+            event bus).`  |  
| `224`  | `243`  |  
| `225`  | `244`  | ` if mp_config.p2p_config.enabled:`  |  
| `226`  | `245`  | ` if not coordinator_config.url:`  |  
| `@@ -235,10 +254,13 @@ def run_http_server(` |  
| `235`  | `254`  | ` "can register; it is incompatible with GDS L1 (--gds-l1-path) " `  |  
| `236`  | `255`  | ` "and Device-DAX L1 (--l1-devdax-path)." `  |  
| `237`  | `256`  |  
| `238`  | `- if coordinator_config.event_reporting and not obs_config.enabled:`  |  
| `257`  |  
| `258`  | `+ coordinator_config.event_reporting or coordinator_config.kv_events_endpoint `  |  
| `259`  | `+    ) and not obs_config.enabled:`  |  
| `239`  | `260`  | ` raise ValueError(`  |  
| `240`  | `- "--coordinator-event-reporting rides the observability event " `  |  
| `241`  | `- "bus: remove --disable-observability to report cache events." `  |  
| `261`  | `+ "--coordinator-event-reporting and --kv-events-endpoint ride the " `  |  
| `262`  | `+ "observability event bus: remove --disable-observability to " `  |  
| `263`  | `+ "report cache events." `  |  
| `242`  | `264`  |  
| `243`  | `265`  | ` _configs["mp"] = mp_config `  |  
| `244`  | `266`  | ` _configs["storage_manager"] = storage_manager_config `  |  
Collapse file
### [`‎tests/v1/mp_coordinator/fixtures/kv_events/README.md‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-e48ca6818e02e86113107b81f6820b4f5d4f2890eb101a6cdc4b0d325132e1cc)
Copy file name to clipboard
+26Lines changed: 26 additions & 0 deletions
  * Display the source diff
  * Display the rich diff

  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -0,0 +1,26 @@` |  
| `+# Golden KV-event payloads (vLLM wire format)`  |  
| `+Each `*.msgpack` file is one `KVEventBatch` payload exactly as`  |  
| `+ `lmcache.v1.mp_coordinator.kv_event_sink.ZmqKVEventSink` sends it in the`  |  
| `+third ZMQ frame (`[topic, seq, payload]`). They are produced and checked by`  |  
| `+ `tests/v1/mp_coordinator/test_kv_event_sink.py` (`test_golden_fixture`) and`  |  
| `+are meant to be decoded unchanged by a consumer's vLLM adapter (llm-d:`  |  
| `+ `pkg/kvevents/engineadapter/vllm_adapter.go`, `ParseMessage`).`  |  
| `+Layout: `[ts: float, events: [event, ...]]`, each event a positional`  |  
| `+array with the tag first:`  |  
| `+ | File | Event | Fields | `  |  
| `+ |---|---|---| `  |  
| `+ | `store_offset0.msgpack` | `BlockStored` | `["BlockStored", [hash(0x01)*32], nil, [1,2,3,4], 4, nil, "lmcache-l1"]` | `  |  
| `+ | `store_offset256_parent.msgpack` | `BlockStored` | same, hash `0x02*32`, `parent_block_hash = 0x01*32`, tokens `[5,6,7,8]` | `  |  
| `+ | `delete.msgpack` | `BlockRemoved` | `["BlockRemoved", [0x01*32, 0x02*32], "lmcache-l1"]` | `  |  
| `+ | `store_shared_l2.msgpack` | `BlockStored` | hash `0x03*32`, tokens `[9,10,11,12]`, medium `"lmcache-l2-fs"` | `  |  
| `+All batches have `ts = 1700000000.5`; hashes are 32-byte `bin` values`  |  
| `+(consumers truncate to the last 8 bytes); `block_size = len(token_ids)`;`  |  
| `+ `lora_id` is `nil`; no HMA fields (`group_idx`, ...) are present. The`  |  
| `+corresponding topics are `kv@<emitter_id>@m`.`  |  
| `+Regenerate after an intended wire change with`  |  
| `+ `LMCACHE_UPDATE_FIXTURES=1 pytest tests/v1/mp_coordinator/test_kv_event_sink.py`.`  |  
Collapse file
### [`‎tests/v1/mp_coordinator/fixtures/kv_events/delete.msgpack‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-43a6fa7f166b932b1477f6b8db383f280b2fa021fd1a07ac428ef96c0ee9bb93)
Copy file name to clipboard
`105 Bytes`
Binary file not shown.
Collapse file
### [`‎tests/v1/mp_coordinator/fixtures/kv_events/store_offset0.msgpack‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-f6ba5043505c06319fb4d866694c91e0247657373f62f4d39929aea3b9ebce1b)
Copy file name to clipboard
`78 Bytes`
Binary file not shown.
Collapse file
### [`‎tests/v1/mp_coordinator/fixtures/kv_events/store_offset256_parent.msgpack‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-18040c2c6d4cc431904853323c6968e5fed71324505f3d35d5a7adabd9623340)
Copy file name to clipboard
`111 Bytes`
Binary file not shown.
Collapse file
### [`‎tests/v1/mp_coordinator/fixtures/kv_events/store_shared_l2.msgpack‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-cc9e854e09e4f387d26fc4f21c837beee36dc083c44f4e6d70948a95c40fd297)
Copy file name to clipboard
`81 Bytes`
Binary file not shown.
Collapse file
### [`‎tests/v1/mp_coordinator/test_cache_events.py‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-34cec0ea79ed3a0d29c88c8e744b155432aa001d5658261c63d9e8876be20091)
Copy file name to clipboardExpand all lines: tests/v1/mp_coordinator/test_cache_events.py
+89Lines changed: 89 additions & 0 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -34,6 +34,7 @@` |  
| ` CacheEventPublishError,`  |  
| ` CacheEventSink,`  |  
| ` CacheEventSubscriber,`  |  
| `+ CompositeCacheEventSink,`  |  
| ` HttpCacheEventSink,`  |  
 |  
| ` from lmcache.v1.mp_coordinator.config import MPCoordinatorConfig `  |  
| `@@ -926,3 +927,91 @@ def test_a_declaration_survives_a_publish_failure():` |  
| `926`  | `927`  | ` # Re-emitted at a fresh revision; the coordinator takes the newer one. `  |  
| `927`  | `928`  | ` subscriber.flush()`  |  
| `928`  | `929`  | ` assert [b.capacity_revision for b in sink.published[0]] == [2]`  |  
| `932`  | `+# -- Parent chain and composite sink ------------------------------------------`  |  
| `935`  | `+ def test_tokens_event_chains_parent_hashes():`  |  
| `936`  | `+ """Within one binding event each chunk's parent is the chunk before `  |  
| `937`  | `+    it; the first chunk has none (sequence start, or a predecessor stored`  |  
| `938`  | `+    by an earlier request)."""`  |  
| `939`  | `+ sink = _RecordingSink()`  |  
| `940`  | `+ subscriber = _subscriber(sink)`  |  
| `942`  | `+ keys = [_key(1), _key(2), _key(3)]`  |  
| `943`  | `+ _dispatch(`  |  
| `944`  | `+ subscriber,`  |  
| `945`  | `+ Event(`  |  
| `946`  | `+ event_type=EventType.MP_TOKENS,`  |  
| `947`  | `+ metadata={`  |  
| `948`  | `+ "chunk_hashes": [key.chunk_hash for key in keys],`  |  
| `949`  | `+ "token_chunks": [[1], [2], [3]],`  |  
| `950`  | `+ "token_offsets": [256, 512, 768],`  |  
| `951`  |  
| `952`  |  
| `953`  | `+ Event(`  |  
| `954`  | `+ event_type=EventType.L1_WRITE_FINISHED,`  |  
| `955`  | `+ metadata={"keys": keys, "meta": [_meta(100)] * 3},`  |  
| `956`  |  
| `957`  |  
| `958`  | `+ subscriber.flush()`  |  
| `960`  | `+    [batches] = sink.published `  |  
| `961`  | `+ assert [e.parent_hash_hex for e in batches[-1].entries] == [`  |  
| `962`  |  
| `963`  | `+ _key(1).chunk_hash.hex(),`  |  
| `964`  | `+ _key(2).chunk_hash.hex(),`  |  
| `965`  |  
| `968`  | `+ def test_unknown_binding_has_no_parent():`  |  
| `969`  | `+ sink = _RecordingSink()`  |  
| `970`  | `+ subscriber = _subscriber(sink)`  |  
| `971`  | `+ _dispatch(`  |  
| `972`  | `+ subscriber,`  |  
| `973`  | `+ Event(`  |  
| `974`  | `+ event_type=EventType.L1_WRITE_FINISHED,`  |  
| `975`  | `+ metadata={"keys": [_key(1)], "meta": [_meta(100)]},`  |  
| `976`  |  
| `977`  |  
| `978`  | `+ subscriber.flush()`  |  
| `979`  | `+    [batches] = sink.published `  |  
| `980`  | `+ assert batches[0].entries[0].parent_hash_hex == "" `  |  
| `983`  | `+ def test_parent_hash_hex_must_be_hex():`  |  
| `984`  | `+ with pytest.raises(ValueError, match="parent_hash_hex"):`  |  
| `985`  | `+ CacheEventEntry(key=_key(1).to_encoded_object_key(), parent_hash_hex="zz")`  |  
| `988`  | `+ def test_composite_sink_attempts_every_sink_and_reraises():`  |  
| `989`  | `+ first, second = _RecordingSink(), _RecordingSink()`  |  
| `990`  | `+ first.fail_next = True `  |  
| `991`  | `+ composite = CompositeCacheEventSink([first, second])`  |  
| `992`  | `+ batches = [`  |  
| `993`  | `+ CacheEventBatch(`  |  
| `994`  | `+ instance_id="node-a",`  |  
| `995`  | `+ incarnation=1,`  |  
| `996`  | `+ seq=1,`  |  
| `997`  | `+ event_type=CacheEventType.STORE,`  |  
| `998`  | `+ tier=Tier.L1,`  |  
| `999`  | `+ backend="dram",`  |  
| `1000`  | `+ entries=[_entry(1, 10)],`  |  
| `1001`  |  
| `1002`  |  
| `1003`  |  
| `1004`  | `+ with pytest.raises(CacheEventPublishError, match="injected failure"):`  |  
| `1005`  | `+ composite.publish(batches)`  |  
| `1006`  | `+ assert first.published == [] and second.published == [batches]`  |  
| `1007`  |  
| `1008`  | `+ composite.publish(batches)`  |  
| `1009`  | `+ assert first.published == [batches]`  |  
| `1010`  |  
| `1011`  | `+ composite.close()`  |  
| `1012`  | `+ assert first.closed and second.closed `  |  
| `1013`  |  
| `1014`  |  
| `1015`  | `+ def test_composite_sink_rejects_no_sinks():`  |  
| `1016`  | `+ with pytest.raises(ValueError):`  |  
| `1017`  | `+ CompositeCacheEventSink([])`  |  
Collapse file
### [`‎tests/v1/mp_coordinator/test_kv_event_sink.py‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-5e0c578ca5b58c011ffc5b2fa9af9cb186547e5978c66e12821ca4993b1e2e5a)
Copy file name to clipboard
+338Lines changed: 338 additions & 0 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -0,0 +1,338 @@` |  
| `+# SPDX-License-Identifier: Apache-2.0`  |  
| `+"""Tests for the engine-format (vLLM KV event) sink: wire layout of the`  |  
| `+msgpack payload against the positional field order llm-d's vLLM adapter`  |  
| `+parses, golden fixtures, ZMQ framing / fan-out, and replay."""`  |  
| `+# Standard`  |  
| `+ from pathlib import Path `  |  
| `+ import os `  |  
| `+ import struct `  |  
| `+ import time `  |  
| `+# Third Party`  |  
| `+ import msgspec `  |  
| `+ import pytest `  |  
| `+ import zmq `  |  
| `+# First Party`  |  
| `+ from lmcache.v1.distributed.api import ObjectKey, Tier `  |  
| `+ from lmcache.v1.mp_coordinator.api import (`  |  
| `+ CacheEventBatch,`  |  
| `+ CacheEventEntry,`  |  
| `+ CacheEventType,`  |  
| `+ from lmcache.v1.mp_coordinator.cache_events import CacheEventPublishError `  |  
| `+ from lmcache.v1.mp_coordinator.kv_event_sink import (`  |  
| `+ ZmqKVEventSink,`  |  
| `+ encode_batch,`  |  
| `+ medium_for,`  |  
| `+ _FIXTURES = Path(__file__).parent / "fixtures" / "kv_events" `  |  
| `+ _TS = 1700000000.5 `  |  
| `+ def _hash(byte: int) -> bytes:`  |  
| `+ """A 32-byte chunk hash (blake3-sized), like production keys carry.""" `  |  
| `+ return bytes([byte]) * 32 `  |  
| `+ def _store_entry(`  |  
| `+ byte: int, token_ids: list[int], parent: int = 0, model: str = "m" `  |  
| `+) -> CacheEventEntry:`  |  
| `+ key = ObjectKey(chunk_hash=_hash(byte), model_name=model, kv_rank=0)`  |  
| `+ return CacheEventEntry(`  |  
| `+ key=key.to_encoded_object_key(),`  |  
| `+ size_bytes=1024,`  |  
| `+ token_ids=token_ids,`  |  
| `+ token_offset=0,`  |  
| `+ parent_hash_hex=_hash(parent).hex() if parent else "",`  |  
| `+ def _delete_entry(byte: int, model: str = "m") -> CacheEventEntry:`  |  
| `+ key = ObjectKey(chunk_hash=_hash(byte), model_name=model, kv_rank=0)`  |  
| `+ return CacheEventEntry(key=key.to_encoded_object_key())`  |  
| `+ def _batch(`  |  
| `+ event_type: CacheEventType,`  |  
| `+ entries: list[CacheEventEntry],`  |  
| `+ tier: Tier = Tier.L1,`  |  
| `+ backend: str = "dram",`  |  
| `+ shared: bool = False,`  |  
| `+) -> CacheEventBatch:`  |  
| `+ return CacheEventBatch(`  |  
| `+ instance_id="node-a",`  |  
| `+ incarnation=1,`  |  
| `+ seq=1,`  |  
| `+ event_type=event_type,`  |  
| `+ tier=tier,`  |  
| `+ backend=backend,`  |  
| `+ entries=entries,`  |  
| `+ shared=shared,`  |  
| `+ ts=_TS,`  |  
| `+ def _decode(payload: bytes) -> list:`  |  
| `+ return msgspec.msgpack.decode(payload)`  |  
| `+# -- Wire layout ----------------------------------------------------------------`  |  
| `+ def test_store_at_offset_zero_has_no_parent():`  |  
| `+ model, payload = encode_batch(`  |  
| `+ _batch(CacheEventType.STORE, [_store_entry(1, [10, 11, 12, 13])])`  |  
| `+ assert model == "m" `  |  
| `+ ts, events = _decode(payload)`  |  
| `+ assert ts == _TS `  |  
| `+ # vLLM BlockStored positional layout, as read by llm-d's vllm_adapter.go: `  |  
| `+ # [tag, block_hashes, parent_block_hash, token_ids, block_size, lora_id, medium] `  |  
| `+ assert events == [`  |  
| `+        ["BlockStored", [_hash(1)], None, [10, 11, 12, 13], 4, None, "lmcache-l1"]`  |  
| `+ def test_store_chain_carries_parent_hash():`  |  
| `100`  | `+ _, payload = encode_batch(`  |  
| `101`  | `+ _batch(`  |  
| `102`  | `+ CacheEventType.STORE,`  |  
| `103`  | `+            [_store_entry(1, [1, 2]), _store_entry(2, [3, 4], parent=1)],`  |  
| `104`  |  
| `105`  |  
| `106`  | `+ _, events = _decode(payload)`  |  
| `107`  | `+ assert [e[0] for e in events] == ["BlockStored", "BlockStored"]`  |  
| `108`  | `+ assert events[0][2] is None `  |  
| `109`  | `+ assert events[1][1] == [_hash(2)] and events[1][2] == _hash(1)`  |  
| `110`  | `+ # No HMA fields (group_idx, ...) are ever appended. `  |  
| `111`  | `+ assert all(len(e) == 7 for e in events)`  |  
| `114`  | `+ def test_delete_becomes_one_block_removed():`  |  
| `115`  | `+ _, payload = encode_batch(`  |  
| `116`  | `+ _batch(CacheEventType.DELETE, [_delete_entry(1), _delete_entry(2)])`  |  
| `117`  |  
| `118`  | `+ _, events = _decode(payload)`  |  
| `119`  | `+ # [tag, block_hashes, medium] `  |  
| `120`  | `+ assert events == [["BlockRemoved", [_hash(1), _hash(2)], "lmcache-l1"]]`  |  
| `123`  | `+ def test_shared_l2_store_uses_backend_medium():`  |  
| `124`  | `+ _, payload = encode_batch(`  |  
| `125`  | `+ _batch(`  |  
| `126`  | `+ CacheEventType.STORE,`  |  
| `127`  | `+            [_store_entry(1, [1])],`  |  
| `128`  | `+ tier=Tier.L2,`  |  
| `129`  | `+ backend="FS",`  |  
| `130`  | `+ shared=True,`  |  
| `131`  |  
| `132`  |  
| `133`  | `+ _, events = _decode(payload)`  |  
| `134`  | `+ assert events[0][6] == "lmcache-l2-fs" `  |  
| `137`  | `+ def test_medium_is_stable_between_store_and_delete():`  |  
| `138`  | `+ assert medium_for(Tier.L1, "dram") == medium_for(Tier.L1, "cxl") == "lmcache-l1" `  |  
| `139`  | `+ assert medium_for(Tier.L2, "mooncake") == "lmcache-l2-mooncake" `  |  
| `142`  | `+ def test_tokenless_store_is_skipped_but_delete_never_is():`  |  
| `143`  | `+ _, payload = encode_batch(_batch(CacheEventType.STORE, [_store_entry(1, [])]))`  |  
| `144`  | `+ assert payload == b"" `  |  
| `146`  | `+ _, payload = encode_batch(`  |  
| `147`  | `+ _batch(CacheEventType.STORE, [_store_entry(1, []), _store_entry(2, [7])])`  |  
| `148`  |  
| `149`  | `+ _, events = _decode(payload)`  |  
| `150`  | `+ assert [e[1] for e in events] == [[_hash(2)]]`  |  
| `152`  | `+ _, payload = encode_batch(_batch(CacheEventType.DELETE, [_delete_entry(1)]))`  |  
| `153`  | `+ assert _decode(payload)[1][0][1] == [_hash(1)]`  |  
| `156`  | `+ def test_non_placement_and_malformed_batches_rejected():`  |  
| `157`  | `+ with pytest.raises(ValueError):`  |  
| `158`  | `+ encode_batch(`  |  
| `159`  | `+ CacheEventBatch(`  |  
| `160`  | `+ instance_id="node-a",`  |  
| `161`  | `+ incarnation=1,`  |  
| `162`  | `+ seq=1,`  |  
| `163`  | `+ event_type=CacheEventType.ACCESS,`  |  
| `164`  | `+ tier=Tier.L1,`  |  
| `165`  | `+ backend="",`  |  
| `166`  | `+ entries=[_delete_entry(1)],`  |  
| `167`  |  
| `168`  |  
| `169`  | `+ with pytest.raises(ValueError):`  |  
| `170`  | `+ encode_batch(_batch(CacheEventType.DELETE, []))`  |  
| `171`  | `+ with pytest.raises(ValueError):`  |  
| `172`  | `+ encode_batch(`  |  
| `173`  | `+ CacheEventBatch(`  |  
| `174`  | `+ instance_id="node-a",`  |  
| `175`  | `+ incarnation=1,`  |  
| `176`  | `+ seq=1,`  |  
| `177`  | `+ event_type=CacheEventType.CONFIG,`  |  
| `178`  | `+ tier=Tier.L1,`  |  
| `179`  | `+ backend="dram",`  |  
| `180`  |  
| `181`  |  
| `182`  | `+ with pytest.raises(ValueError, match="several models"):`  |  
| `183`  | `+ encode_batch(`  |  
| `184`  | `+ _batch(`  |  
| `185`  | `+ CacheEventType.DELETE,`  |  
| `186`  | `+                [_delete_entry(1, model="a"), _delete_entry(2, model="b")],`  |  
| `187`  |  
| `188`  |  
| `191`  | `+# -- Golden fixtures (shared with llm-d's adapter tests) ------------------------`  |  
| `194`  | `+ _GOLDEN = {`  |  
| `195`  | `+ "store_offset0": _batch(CacheEventType.STORE, [_store_entry(1, [1, 2, 3, 4])]),`  |  
| `196`  | `+ "store_offset256_parent": _batch(`  |  
| `197`  | `+ CacheEventType.STORE, [_store_entry(2, [5, 6, 7, 8], parent=1)]`  |  
| `198`  |  
| `199`  | `+ "delete": _batch(CacheEventType.DELETE, [_delete_entry(1), _delete_entry(2)]),`  |  
| `200`  | `+ "store_shared_l2": _batch(`  |  
| `201`  | `+ CacheEventType.STORE,`  |  
| `202`  | `+        [_store_entry(3, [9, 10, 11, 12])],`  |  
| `203`  | `+ tier=Tier.L2,`  |  
| `204`  | `+ backend="fs",`  |  
| `205`  | `+ shared=True,`  |  
| `206`  |  
| `207`  |  
| `210`  | `+@pytest.mark.parametrize("name", sorted(_GOLDEN))`  |  
| `211`  | `+ def test_golden_fixture(name):`  |  
| `212`  | `+ """The encoded bytes match the checked-in fixture. Set `  |  
| `213`  | `+    ``LMCACHE_UPDATE_FIXTURES=1`` to regenerate after an intended change."""`  |  
| `214`  | `+ _, payload = encode_batch(_GOLDEN[name])`  |  
| `215`  | `+ path = _FIXTURES / f"{name}.msgpack" `  |  
| `216`  | `+ if os.environ.get("LMCACHE_UPDATE_FIXTURES") == "1":`  |  
| `217`  | `+ path.write_bytes(payload)`  |  
| `218`  | `+ assert payload == path.read_bytes()`  |  
| `221`  | `+# -- ZMQ transport ---------------------------------------------------------------`  |  
| `224`  | `+ def _subscriber(ctx: zmq.Context, endpoint: str, topic: bytes = b"kv@") -> zmq.Socket:`  |  
| `225`  | `+ sub = ctx.socket(zmq.SUB)`  |  
| `226`  | `+ sub.setsockopt(zmq.LINGER, 0)`  |  
| `227`  | `+ sub.setsockopt(zmq.RCVTIMEO, 5000)`  |  
| `228`  | `+ sub.connect(endpoint)`  |  
| `229`  | `+ sub.setsockopt(zmq.SUBSCRIBE, topic)`  |  
| `230`  | `+ time.sleep(0.3)  # PUB/SUB slow-joiner: let the subscription propagate `  |  
| `231`  | `+ return sub `  |  
| `234`  | `+@pytest.fixture`  |  
| `235`  | `+ def zmq_ctx():`  |  
| `236`  | `+ ctx = zmq.Context()`  |  
| `237`  | `+ yield ctx `  |  
| `238`  | `+ ctx.term()`  |  
| `241`  | `+ def test_publish_fans_out_three_frame_messages(zmq_ctx):`  |  
| `242`  | `+ sink = ZmqKVEventSink("tcp://127.0.0.1:*", emitter_ids=["pod-a", "pod-b"])`  |  
| `243`  | `+ try:`  |  
| `244`  | `+ sub = _subscriber(zmq_ctx, sink.endpoint)`  |  
| `245`  | `+ sink.publish(`  |  
| `246`  |  
| `247`  | `+ _batch(CacheEventType.STORE, [_store_entry(1, [1, 2])]),`  |  
| `248`  | `+ # Ignored: not a placement. `  |  
| `249`  | `+ CacheEventBatch(`  |  
| `250`  | `+ instance_id="node-a",`  |  
| `251`  | `+ incarnation=1,`  |  
| `252`  | `+ seq=2,`  |  
| `253`  | `+ event_type=CacheEventType.ACCESS,`  |  
| `254`  | `+ tier=Tier.L1,`  |  
| `255`  | `+ backend="",`  |  
| `256`  | `+ entries=[_delete_entry(1)],`  |  
| `257`  |  
| `258`  | `+ _batch(CacheEventType.DELETE, [_delete_entry(1)]),`  |  
| `259`  |  
| `260`  |  
| `261`  | `+ messages = [sub.recv_multipart() for _ in range(4)]`  |  
| `262`  | `+ sub.close()`  |  
| `263`  | `+ finally:`  |  
| `264`  | `+ sink.close()`  |  
| `266`  | `+ # [topic, seq (8-byte big-endian, from 0), payload] `  |  
| `267`  | `+ assert [m[0] for m in messages] == [`  |  
| `268`  | `+ b"kv@pod-a@m",`  |  
| `269`  | `+ b"kv@pod-b@m",`  |  
| `270`  | `+ b"kv@pod-a@m",`  |  
| `271`  | `+ b"kv@pod-b@m",`  |  
| `272`  |  
| `273`  | `+ assert [struct.unpack(">Q", m[1])[0] for m in messages] == [0, 1, 2, 3]`  |  
| `274`  | `+ assert [_decode(m[2])[1][0][0] for m in messages] == [`  |  
| `275`  | `+ "BlockStored",`  |  
| `276`  | `+ "BlockStored",`  |  
| `277`  | `+ "BlockRemoved",`  |  
| `278`  | `+ "BlockRemoved",`  |  
| `279`  |  
| `282`  | `+ def test_replay_returns_buffered_messages_from_start_seq(zmq_ctx):`  |  
| `283`  | `+ sink = ZmqKVEventSink(`  |  
| `284`  | `+ "tcp://127.0.0.1:*",`  |  
| `285`  | `+ emitter_ids=["pod-a"],`  |  
| `286`  | `+ replay_endpoint="tcp://127.0.0.1:*",`  |  
| `287`  | `+ replay_depth=8,`  |  
| `288`  |  
| `289`  | `+ try:`  |  
| `290`  | `+ # No subscriber: PUB drops, but the ring still records every send. `  |  
| `291`  | `+ for _ in range(3):`  |  
| `292`  | `+ sink.publish([_batch(CacheEventType.DELETE, [_delete_entry(1)])])`  |  
| `294`  | `+ dealer = zmq_ctx.socket(zmq.DEALER)`  |  
| `295`  | `+ dealer.setsockopt(zmq.LINGER, 0)`  |  
| `296`  | `+ dealer.setsockopt(zmq.RCVTIMEO, 5000)`  |  
| `297`  | `+ dealer.connect(sink.replay_endpoint)`  |  
| `298`  | `+ dealer.send_multipart([b"", struct.pack(">Q", 1)])`  |  
| `299`  | `+ replies = []`  |  
| `300`  | `+ while True:`  |  
| `301`  | `+ frames = dealer.recv_multipart()`  |  
| `302`  | `+ replies.append(frames)`  |  
| `303`  | `+ if frames[-1] == b"":`  |  
| `304`  | `+ break `  |  
| `305`  | `+ dealer.close()`  |  
| `306`  | `+ finally:`  |  
| `307`  | `+ sink.close()`  |  
| `309`  | `+ *events, end = replies `  |  
| `310`  | `+ # Each replayed message is the live frame set behind an empty delimiter. `  |  
| `311`  | `+ assert [f[0] for f in events] == [b"", b""]`  |  
| `312`  | `+ assert [f[1] for f in events] == [b"kv@pod-a@m"] * 2 `  |  
| `313`  | `+ assert [struct.unpack(">Q", f[2])[0] for f in events] == [1, 2]`  |  
| `314`  | `+ # End marker: empty topic, seq -1, empty payload. `  |  
| `315`  | `+ assert end == [b"", b"", struct.pack(">q", -1), b""]`  |  
| `318`  | `+ def test_publish_after_close_raises():`  |  
| `319`  | `+ sink = ZmqKVEventSink("tcp://127.0.0.1:*", emitter_ids=["pod-a"])`  |  
| `320`  | `+ sink.close()`  |  
| `321`  | `+ with pytest.raises(CacheEventPublishError):`  |  
| `322`  | `+ sink.publish([_batch(CacheEventType.DELETE, [_delete_entry(1)])])`  |  
| `323`  | `+ sink.close()  # idempotent `  |  
| `326`  | `+@pytest.mark.parametrize(`  |  
| `327`  | `+ "kwargs",`  |  
| `328`  |  
| `329`  | `+        {"emitter_ids": []},`  |  
| `330`  | `+        {"emitter_ids": ["a@b"]},`  |  
| `331`  | `+        {"emitter_ids": [""]},`  |  
| `332`  | `+        {"emitter_ids": ["a"], "replay_depth": 0},`  |  
| `333`  | `+        {"emitter_ids": ["a"], "hwm": 0},`  |  
| `334`  |  
| `335`  |  
| `336`  | `+ def test_invalid_construction_rejected(kwargs):`  |  
| `337`  | `+ with pytest.raises(ValueError):`  |  
| `338`  | `+ ZmqKVEventSink("tcp://127.0.0.1:*", **kwargs)`  |  
Collapse file
### [`‎tests/v1/multiprocess/test_config.py‎`](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472#diff-2ef75394244e73a8522a677e9e714b2cba61e990e25fb3856478af48137cdc6a)
Copy file name to clipboardExpand all lines: tests/v1/multiprocess/test_config.py
+74Lines changed: 74 additions & 0 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -256,3 +256,77 @@ def emit(self, record: logging.LogRecord) -> None:` |  
| `256`  | `256`  | ` def test_deprecated_flush_interval_flag_rejects_nonpositive():`  |  
| `257`  | `257`  | ` with pytest.raises(ValueError):`  |  
| `258`  | `258`  | ` _parse(["--coordinator-l2-event-flush-interval", "0"])`  |  
| `261`  | `+# -- KV events (engine-format sink) -------------------------------------------`  |  
| `264`  | `+ _KV_ENV = (`  |  
| `265`  | `+ "LMCACHE_KV_EVENTS_ENDPOINT",`  |  
| `266`  | `+ "LMCACHE_KV_EVENTS_EMITTER_IDS",`  |  
| `267`  | `+ "LMCACHE_KV_EVENTS_REPLAY_PORT",`  |  
| `268`  | `+ "NODE_NAME",`  |  
| `269`  |  
| `272`  | `+@pytest.fixture(autouse=True)`  |  
| `273`  | `+ def _clear_kv_env(monkeypatch):`  |  
| `274`  | `+ for name in _KV_ENV:`  |  
| `275`  | `+ monkeypatch.delenv(name, raising=False)`  |  
| `278`  | `+ def test_kv_events_default_to_disabled_with_node_emitter_id():`  |  
| `279`  | `+ config = _parse([])`  |  
| `280`  | `+ assert config.kv_events_endpoint == "" `  |  
| `281`  | `+ assert config.kv_events_replay_port == 0 `  |  
| `282`  | `+    [emitter_id] = config.kv_events_emitter_ids `  |  
| `283`  | `+ assert emitter_id.startswith("node:") and len(emitter_id)  len("node:")`  |  
| `286`  | `+ def test_kv_events_emitter_id_defaults_to_node_name(monkeypatch):`  |  
| `287`  | `+ monkeypatch.setenv("NODE_NAME", "worker-7")`  |  
| `288`  | `+ assert _parse([]).kv_events_emitter_ids == ["node:worker-7"]`  |  
| `291`  | `+ def test_kv_events_flags_are_parsed():`  |  
| `292`  | `+ config = _parse(`  |  
| `293`  |  
| `294`  | `+ "--kv-events-endpoint",`  |  
| `295`  | `+ "tcp://*:5557",`  |  
| `296`  | `+ "--kv-events-emitter-ids",`  |  
| `297`  | `+ "pod-a, pod-b,",`  |  
| `298`  | `+ "--kv-events-replay-port",`  |  
| `299`  | `+ "5558",`  |  
| `300`  |  
| `301`  |  
| `302`  | `+ assert config.kv_events_endpoint == "tcp://*:5557" `  |  
| `303`  | `+ assert config.kv_events_emitter_ids == ["pod-a", "pod-b"]`  |  
| `304`  | `+ assert config.kv_events_replay_port == 5558 `  |  
| `307`  | `+ def test_kv_events_env_fallback_and_flag_precedence(monkeypatch):`  |  
| `308`  | `+ monkeypatch.setenv("LMCACHE_KV_EVENTS_ENDPOINT", "tcp://*:1")`  |  
| `309`  | `+ monkeypatch.setenv("LMCACHE_KV_EVENTS_EMITTER_IDS", "env-a")`  |  
| `310`  | `+ monkeypatch.setenv("LMCACHE_KV_EVENTS_REPLAY_PORT", "2")`  |  
| `311`  | `+ config = _parse([])`  |  
| `312`  | `+ assert (`  |  
| `313`  | `+ config.kv_events_endpoint,`  |  
| `314`  | `+ config.kv_events_emitter_ids,`  |  
| `315`  | `+ config.kv_events_replay_port,`  |  
| `316`  | `+    ) == ("tcp://*:1", ["env-a"], 2)`  |  
| `317`  | `+ assert _parse(["--kv-events-emitter-ids", "flag-a"]).kv_events_emitter_ids == [`  |  
| `318`  | `+ "flag-a" `  |  
| `319`  |  
| `322`  | `+@pytest.mark.parametrize(`  |  
| `323`  | `+ "argv",`  |  
| `324`  |  
| `325`  | `+        ["--kv-events-emitter-ids", "a@b"],`  |  
| `326`  | `+        ["--kv-events-replay-port", "-1"],`  |  
| `327`  | `+        ["--kv-events-replay-port", "65536"],`  |  
| `328`  |  
| `329`  |  
| `330`  | `+ def test_kv_events_invalid_values_rejected(argv):`  |  
| `331`  | `+ with pytest.raises(ValueError):`  |  
| `332`  | `+ _parse(argv)`  |  
## 0 commit comments
Comments
0 (0)
Please [sign in](https://github.com/login?return_to=https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472) to comment.
You can’t perform that action at this time. 
4 files remain
