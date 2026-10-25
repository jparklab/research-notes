---
title: sglang/python/sglang/srt/mem_cache/events.py at main · sgl-project/sglang ·
  GitHub
id: sglangpythonsglangsrtmem_cacheeventspy-at-main-sgl-projectsglang-github
tags:
- llmd-kv-cache-routing-c665d5
- locus-approximate-vs-precise-index
- kv-cache-indexer
- sglang
- zmq-kv-events
created: '2026-09-14T12:44:27.196897Z'
updated: '2026-09-14T12:45:58.902387Z'
source: https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/mem_cache/events.py
source_domain: github.com
fetched_at: '2026-09-14T12:44:27.175614Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE: SGLang production code (python/sglang/srt/mem_cache/events.py,
  main branch, commit 0b415fa, Sep 12 2026) confirming SGLang natively implements
  BlockStored/BlockRemoved/AllBlocksCleared KV-cache placement events. Module docstring:
  ''Produces the BlockStored / BlockRemoved / AllBlocksCleared events consumed by
  KV-aware routers (e.g. dynamo).'' -- note the named consumer in-code is NVIDIA Dynamo,
  not llm-d (llm-d''s SGLangAdapter consumes the same wire schema per llm-d-kv-cache''s
  architecture.md, but SGLang''s own docstring doesn''t credit llm-d specifically).\n\nKVCacheEventRecorder.record_store()
  builds one BlockStored per page_size-token chunk from the radix-tree node, computing
  block_hashes via hash_str_to_int64(event_hash_values[page_index]) where event_hash_values
  come from compute_node_hash_values()/compute_node_event_hash_values() in sglang.srt.mem_cache.utils
  -- an SGLang-internal hashing scheme, NOT vLLM''s sha256/xxhash chain reused verbatim.
  parent_block_hash chains blocks together (root node contributes no link). BlockStoredWithMetadata
  carries an optional cache_salt (used for the ''extra_key''/salted-request differentiation)
  and BlockStoredMetadata; LoRA is represented via a lora_id field. record_remove()
  emits BlockRemoved carrying only block_hashes (no tokens), matching the vLLM event
  shape. Adjacent compatible events are coalesced in a queue (extending block_hashes/token_ids)
  ''while preserving ordering and the parent-linked store chains consumers use to
  rebuild the cache tree.'' page_size is a recorder-level constructor parameter (not
  hardcoded), confirming SGLang''s block/page granularity is configurable rather than
  a fixed built-in default matching vLLM''s 16.\n\nThis is direct code evidence that
  SGLang''s KV-event publishing is production reality (not aspirational), but the
  underlying block-hash VALUES are computed by SGLang''s own hash_str_to_int64/compute_node_hash_values
  machinery, independent of vLLM''s parent_hash+token_chunk sha256/xxhash chain --
  consistent with llm-d''s dual-key design (the indexer only needs token_ids from
  the wire payload, not engine-hash bit-compatibility).'
---

[Skip to content](https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/mem_cache/events.py#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/mem_cache/events.py) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/mem_cache/events.py) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/mem_cache/events.py) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/mem_cache/events.py).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fsgl-project%2Fsglang) You must be signed in to change notification settings
  * [ Fork 8.8k ](https://github.com/login?return_to=%2Fsgl-project%2Fsglang)
  * [ Star  35.9k ](https://github.com/login?return_to=%2Fsgl-project%2Fsglang)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# events.py
Copy path
More file actions
More file actions
## Latest commit
and
[[HiCache][LoRA] Isolate storage pages by extra key (](https://github.com/sgl-project/sglang/commit/0b415fa57312337903478ed30e00ab3ab714548e)[#38577](https://github.com/sgl-project/sglang/pull/38577)[)](https://github.com/sgl-project/sglang/commit/0b415fa57312337903478ed30e00ab3ab714548e)
Open commit detailsfailure
Sep 12, 2026
[0b415fa](https://github.com/sgl-project/sglang/commit/0b415fa57312337903478ed30e00ab3ab714548e) · Sep 12, 2026
## History
[History](https://github.com/sgl-project/sglang/commits/main/python/sglang/srt/mem_cache/events.py)
Open commit details
History
204 lines (175 loc) · 7.58 KB
Code owner: @merrymercy, @Ying1123, @hnyls2002, @xiezhq-hermann, @hanming-lu, @yizhang2077, @hzh0425, @ispobock, @alphabetc1, and @huangtingwei9988
/
# events.py
Copy path
## File metadata and controls
  * 

204 lines (175 loc) · 7.58 KB
Code owner: @merrymercy, @Ying1123, @hnyls2002, @xiezhq-hermann, @hanming-lu, @yizhang2077, @hzh0425, @ispobock, @alphabetc1, and @huangtingwei9988
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Open symbols panel
Edit and raw actions
# Copyright 2025 SGLang Team # Licensed under the Apache License, Version 2.0 (the "License"); # you may not use this file except in compliance with the License. # You may obtain a copy of the License at # # http://www.apache.org/licenses/LICENSE-2.0 # # Unless required by applicable law or agreed to in writing, software # distributed under the License is distributed on an "AS IS" BASIS, # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. # See the License for the specific language governing permissions and # limitations under the License. # ============================================================================== """KV cache placement event recording. Produces the ``BlockStored`` / ``BlockRemoved`` / ``AllBlocksCleared`` events consumed by KV-aware routers (e.g. dynamo). A cache holds one recorder and calls it; the recorder owns the queue and needs nothing back from its owner. """ from typing import Any, Optional from sglang.srt.disaggregation.kv_events import ( AllBlocksCleared, BlockRemoved, BlockStored, BlockStoredMetadata, BlockStoredWithMetadata, StorageMedium, ) from sglang.srt.mem_cache.utils import ( compute_node_event_hash_values, compute_node_hash_values, hash_str_to_int64, ) class KVCacheEventRecorder: """Collects KV placement events for one cache. ``enabled=False`` makes every ``record_*`` call a no-op and ``take`` return an empty list, so callers never have to guard. """ def __init__(self, *, enabled: bool, page_size: int): self.enabled = enabled self.page_size = page_size self._queue: list = [] def enqueue(self, event) -> None: """Append an event, coalescing it with a compatible queue tail. KV event batches already support multiple block hashes. Combining them here avoids emitting one event per page while preserving ordering and the parent-linked store chains consumers use to rebuild the cache tree. """ if self._queue: tail = self._queue[-1] if isinstance(tail, BlockRemoved) and isinstance(event, BlockRemoved): if tail.medium == event.medium: tail.block_hashes.extend(event.block_hashes) return elif isinstance(tail, BlockStored) and isinstance(event, BlockStored): tail_metadata = ( tail.metadata if isinstance(tail, BlockStoredWithMetadata) else None ) event_metadata = ( event.metadata if isinstance(event, BlockStoredWithMetadata) else None ) if ( tail.medium == event.medium and tail.lora_id == event.lora_id and tail.block_size == event.block_size and tail_metadata == event_metadata and tail.block_hashes and event.parent_block_hash == tail.block_hashes[-1] ): tail.block_hashes.extend(event.block_hashes) tail.token_ids.extend(event.token_ids) return self._queue.append(event) def _node_event_hash_values(self, node: Any) -> list: """Hash values to publish for ``node``, computing them if not yet set.""" if node.hash_value is None: node.hash_value = compute_node_hash_values(node, self.page_size) if node.key.extra_key is None and node.key.cache_salt is None: return node.hash_value return compute_node_event_hash_values(node, self.page_size) def _parent_block_hash(self, node: Any) -> Optional[int]: """The hash the first page of ``node`` links back to. ``None`` when the parent is the tree root: a root carries an empty ``hash_value`` and no event hash, so it contributes no link. Every other node on the path has a parent, which is what distinguishes the two. """ parent = node.parent if parent is None or parent.parent is None: return None if node.key.extra_key is not None or node.key.cache_salt is not None: parent_hash_values = parent.event_hash_value assert parent_hash_values is not None else: parent_hash_values = parent.hash_value if not parent_hash_values: return None return hash_str_to_int64(parent_hash_values[-1]) def record_store(self, node: Any, medium=None) -> None: # One BlockStored per ``page_size`` chunk. # ``medium`` defaults to StorageMedium.GPU but callers may override # for lower-tier insertions (e.g. StorageMedium.CPU for host/L2 cache). if not self.enabled: return if medium is None: medium = StorageMedium.GPU event_hash_values = self._node_event_hash_values(node) parent_block_hash = self._parent_block_hash(node) page_index = 0 logical_len = len(node.key) is_bigram = node.key.is_bigram raw = node.key.token_ids for start in range(0, logical_len, self.page_size): end = min(start + self.page_size, logical_len) if end <= start: continue # Preserve historical event payload: bigram pages expose tuples. if is_bigram: page_tokens = [(raw[j], raw[j + 1]) for j in range(start, end)] else: page_tokens = list(raw[start:end]) block_hash = hash_str_to_int64(event_hash_values[page_index]) event_args = { "block_hashes": [block_hash], "parent_block_hash": parent_block_hash, "token_ids": page_tokens, "block_size": len(page_tokens), "lora_id": None, "medium": medium, } if node.key.cache_salt is None: event = BlockStored(**event_args) else: event = BlockStoredWithMetadata( **event_args, metadata=BlockStoredMetadata(cache_salt=node.key.cache_salt), ) self.enqueue(event) parent_block_hash = block_hash page_index += 1 def record_remove(self, node: Any, medium=None) -> None: # One BlockRemoved per radix node. # ``medium`` defaults to StorageMedium.GPU but callers may override for # lower-tier removals (e.g. StorageMedium.CPU when evicting from host). if not self.enabled: return if medium is None: medium = StorageMedium.GPU # Hash values must match what was stored. event_hash_values = self._node_event_hash_values(node) block_hashes = [] logical_len = len(node.key) page_index = 0 for start in range(0, logical_len, self.page_size): end = min(start + self.page_size, logical_len) if end <= start: continue block_hashes.append(hash_str_to_int64(event_hash_values[page_index])) page_index += 1 if block_hashes: self.enqueue(BlockRemoved(block_hashes=block_hashes, medium=medium)) def record_all_cleared(self) -> None: if not self.enabled: return self.enqueue(AllBlocksCleared()) def take(self) -> list: """Atomically takes all events and clears the queue. Returns: A list of KV cache events. """ if not self.enabled: return [] events = self._queue self._queue = [] return events
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
122
123
124
125
126
127
128
129
130
131
132
133
134
135
136
137
138
139
140
141
142
143
144
145
146
147
148
149
150
151
152
153
154
155
156
157
158
159
160
161
162
163
164
165
166
167
168
169
170
171
172
173
174
175
176
177
178
179
180
181
182
183
184
185
186
187
188
189
190
191
192
193
194
195
196
197
198
199
200
201
202
203
204
# Copyright 2025 SGLang Team
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
# http://www.apache.org/licenses/LICENSE-2.0
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# ==============================================================================
"""KV cache placement event recording.
Produces the ``BlockStored`` / ``BlockRemoved`` / ``AllBlocksCleared`` events
consumed by KV-aware routers (e.g. dynamo). A cache holds one recorder and calls
it; the recorder owns the queue and needs nothing back from its owner.
from typing import Any, Optional
from sglang.srt.disaggregation.kv_events import (
AllBlocksCleared,
BlockRemoved,
BlockStored,
BlockStoredMetadata,
BlockStoredWithMetadata,
StorageMedium,
from sglang.srt.mem_cache.utils import (
compute_node_event_hash_values,
compute_node_hash_values,
hash_str_to_int64,
class KVCacheEventRecorder:
"""Collects KV placement events for one cache.
``enabled=False`` makes every ``record_*`` call a no-op and ``take`` return an
empty list, so callers never have to guard.
def __init__(self, *, enabled: bool, page_size: int):
self.enabled = enabled
self.page_size = page_size
self._queue: list = []
def enqueue(self, event) -> None:
"""Append an event, coalescing it with a compatible queue tail.
KV event batches already support multiple block hashes. Combining them
here avoids emitting one event per page while preserving ordering and
the parent-linked store chains consumers use to rebuild the cache tree.
if self._queue:
tail = self._queue[-1]
if isinstance(tail, BlockRemoved) and isinstance(event, BlockRemoved):
if tail.medium == event.medium:
tail.block_hashes.extend(event.block_hashes)
return
elif isinstance(tail, BlockStored) and isinstance(event, BlockStored):
tail_metadata = (
tail.metadata if isinstance(tail, BlockStoredWithMetadata) else None
event_metadata = (
event.metadata
if isinstance(event, BlockStoredWithMetadata)
else None
tail.medium == event.medium
and tail.lora_id == event.lora_id
and tail.block_size == event.block_size
and tail_metadata == event_metadata
and tail.block_hashes
and event.parent_block_hash == tail.block_hashes[-1]
tail.block_hashes.extend(event.block_hashes)
tail.token_ids.extend(event.token_ids)
return
self._queue.append(event)
def _node_event_hash_values(self, node: Any) -> list:
"""Hash values to publish for ``node``, computing them if not yet set."""
if node.hash_value is None:
node.hash_value = compute_node_hash_values(node, self.page_size)
if node.key.extra_key is None and node.key.cache_salt is None:
return node.hash_value
return compute_node_event_hash_values(node, self.page_size)
def _parent_block_hash(self, node: Any) -> Optional[int]:
"""The hash the first page of ``node`` links back to.
``None`` when the parent is the tree root: a root carries an empty
``hash_value`` and no event hash, so it contributes no link. Every other
node on the path has a parent, which is what distinguishes the two.
parent = node.parent
if parent is None or parent.parent is None:
return None
if node.key.extra_key None or node.key.cache_salt None:
parent_hash_values = parent.event_hash_value
assert parent_hash_values None
else:
parent_hash_values = parent.hash_value
if not parent_hash_values:
return None
return hash_str_to_int64(parent_hash_values[-1])
def record_store(self, node: Any, medium=None) -> None:
# One BlockStored per ``page_size`` chunk.
# ``medium`` defaults to StorageMedium.GPU but callers may override
# for lower-tier insertions (e.g. StorageMedium.CPU for host/L2 cache).
if not self.enabled:
return
if medium is None:
medium = StorageMedium.GPU
event_hash_values = self._node_event_hash_values(node)
parent_block_hash = self._parent_block_hash(node)
page_index = 0
logical_len = len(node.key)
is_bigram = node.key.is_bigram
raw = node.key.token_ids
for start in range(0, logical_len, self.page_size):
end = min(start + self.page_size, logical_len)
if end <= start:
continue
# Preserve historical event payload: bigram pages expose tuples.
if is_bigram:
page_tokens = [(raw[j], raw[j + 1]) for j in range(start, end)]
else:
page_tokens = list(raw[start:end])
block_hash = hash_str_to_int64(event_hash_values[page_index])
event_args = {
"block_hashes": [block_hash],
"parent_block_hash": parent_block_hash,
"token_ids": page_tokens,
"block_size": len(page_tokens),
"lora_id": None,
"medium": medium,
if node.key.cache_salt is None:
event = BlockStored(**event_args)
else:
event = BlockStoredWithMetadata(
**event_args,
metadata=BlockStoredMetadata(cache_salt=node.key.cache_salt),
self.enqueue(event)
parent_block_hash = block_hash
page_index += 1
def record_remove(self, node: Any, medium=None) -> None:
# One BlockRemoved per radix node.
# ``medium`` defaults to StorageMedium.GPU but callers may override for
# lower-tier removals (e.g. StorageMedium.CPU when evicting from host).
if not self.enabled:
return
if medium is None:
medium = StorageMedium.GPU
# Hash values must match what was stored.
event_hash_values = self._node_event_hash_values(node)
block_hashes = []
logical_len = len(node.key)
page_index = 0
for start in range(0, logical_len, self.page_size):
end = min(start + self.page_size, logical_len)
if end <= start:
continue
block_hashes.append(hash_str_to_int64(event_hash_values[page_index]))
page_index += 1
if block_hashes:
self.enqueue(BlockRemoved(block_hashes=block_hashes, medium=medium))
def record_all_cleared(self) -> None:
if not self.enabled:
return
self.enqueue(AllBlocksCleared())
def take(self) -> list:
"""Atomically takes all events and clears the queue.
Returns:
A list of KV cache events.
if not self.enabled:
return []
events = self._queue
self._queue = []
return events
You can’t perform that action at this time. 
While the code is focused, press Alt+F1 for a menu of operations.
