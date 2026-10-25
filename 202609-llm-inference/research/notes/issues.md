---
title: issues
id: issues
tags:
- llmd-kv-cache-routing-c665d5
- trtllm-gap
- github-pr-search
- negative-finding
created: '2026-09-14T13:03:06.196068Z'
updated: '2026-10-10T14:52:17.316385Z'
source: https://api.github.com/search/issues?q=repo:llm-d/llm-d-router+trtllm+in:title,body+type:pr
source_domain: api.github.com
fetched_at: '2026-09-14T13:03:06.175797Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub Search API results (api.github.com/search/issues), query repo:llm-d/llm-d-router
  trtllm in:title,body type:pr, fetched 14 Sep 2026: 5 total PRs match ''trtllm''
  anywhere in llm-d-router''s PR history. All 5 concern Prometheus METRICS extraction
  / load-scoring for trtllm-serve and triton-tensorrt-llm as ''engines'' (queue depth,
  running-request counts, KV-cache-capacity gauges like trtllm_kv_cache_max_blocks
  feeding approx-prefix-cache LRU autotuning) -- e.g. PR #2292 ''size approx-prefix-cache
  LRU to offload-extended KV cache capacity'' documents trtllm-serve''s trtllm_kv_cache_max_blocks
  metric, and PR #1230 fixes legacy-metrics-mode support for the Triton backend. NONE
  of the 5 touch pkg/kvevents/engineadapter or add a kvevents.EngineAdapter implementation
  for TensorRT-LLM/trtllm/triton -- TensorRT-LLM support in llm-d-router is confined
  to the metrics/load-scoring data-layer, not the event-driven precise KV-cache index.'
---

```
{
  "total_count": 5,
  "incomplete_results": false,
  "items": [
    {
      "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2292",
      "repository_url": "https://api.github.com/repos/llm-d/llm-d-router",
      "labels_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2292/labels{/name}",
      "comments_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2292/comments",
      "events_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2292/events",
      "html_url": "https://github.com/llm-d/llm-d-router/pull/2292",
      "id": 5075868291,
      "node_id": "PR_kwDOOkxMpc77Uam3",
      "number": 2292,
      "title": "feat(scheduler): size approx-prefix-cache LRU to offload-extended KV cache capacity",
      "user": {
        "login": "BenjaminBraunDev",
        "id": 187570160,
        "node_id": "U_kgDOCy4X8A",
        "avatar_url": "https://avatars.githubusercontent.com/u/187570160?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/BenjaminBraunDev",
        "html_url": "https://github.com/BenjaminBraunDev",
        "followers_url": "https://api.github.com/users/BenjaminBraunDev/followers",
        "following_url": "https://api.github.com/users/BenjaminBraunDev/following{/other_user}",
        "gists_url": "https://api.github.com/users/BenjaminBraunDev/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/BenjaminBraunDev/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/BenjaminBraunDev/subscriptions",
        "organizations_url": "https://api.github.com/users/BenjaminBraunDev/orgs",
        "repos_url": "https://api.github.com/users/BenjaminBraunDev/repos",
        "events_url": "https://api.github.com/users/BenjaminBraunDev/events{/privacy}",
        "received_events_url": "https://api.github.com/users/BenjaminBraunDev/received_events",
        "type": "User",
        "user_view_type": "public",
        "site_admin": false
      },
      "labels": [
        {
          "id": 9034267324,
          "node_id": "LA_kwDOOkxMpc8AAAACGnv6vA",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/kind/bug",
          "name": "kind/bug",
          "color": "e11d21",
          "default": false,
          "description": "Categorizes issue or PR as related to a bug.",
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 9107540209,
          "node_id": "LA_kwDOOkxMpc8AAAACHtoI8Q",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/lifecycle/stale",
          "name": "lifecycle/stale",
          "color": "ededed",
          "default": false,
          "description": null,
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 10876293184,
          "node_id": "LA_kwDOOkxMpc8AAAACiEcQQA",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/size/XXL",
          "name": "size/XXL",
          "color": "ee0000",
          "default": false,
          "description": "Denotes a PR that changes 1000+ lines, ignoring generated files.",
          "archived_at": null,
          "archived_by": null
        }
      ],
      "state": "open",
      "locked": false,
      "assignees": [

      ],
      "milestone": null,
      "comments": 2,
      "created_at": "2026-08-05T22:46:40Z",
      "updated_at": "2026-08-29T01:17:21Z",
      "closed_at": null,
      "assignee": null,
      "author_association": "CONTRIBUTOR",
      "issue_field_values": [

      ],
      "type": null,
      "active_lock_reason": null,
      "draft": true,
      "pull_request": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/pulls/2292",
        "html_url": "https://github.com/llm-d/llm-d-router/pull/2292",
        "diff_url": "https://github.com/llm-d/llm-d-router/pull/2292.diff",
        "patch_url": "https://github.com/llm-d/llm-d-router/pull/2292.patch",
        "merged_at": null
      },
      "body": "**What type of PR is this?**\r\n/kind bug\r\n\r\n**What this PR does / why we need it**:\r\n\r\nThe `approx-prefix-cache-producer` autotune sizes its per-endpoint LRU from `num_gpu_blocks` (HBM only). With KV cache offloading the actual cache is HBM plus a host tier, so the LRU evicts prefixes the server still holds and consumers (`prefix-cache-scorer`, `prefix-cache-affinity-filter`) get under-counted matches. See the issue for root cause and benchmarks.\r\n\r\nApproach: when the model server reports its total cache size (GPU plus CPU), we size each pod's LRU to that total. When it reports only the GPU cache size but we can see offloading is enabled, we keep the GPU-only size and log a warning to set `lruCapacityPerServer` manually. If a pod's reported size changes later, its LRU is resized in place to match.\r\n\r\nPer-engine behavior:\r\n\r\n- **SGLang**: v0.5.10 and later report the CPU cache size as `sglang:hicache_host_total_tokens`, so we size to `max(hicache_host_total_tokens, max_total_num_tokens)`. Max rather than sum, because the CPU cache also holds a copy of everything in the GPU cache. Earlier versions only report the GPU cache size (`sglang:max_total_num_tokens`), so we size to that, and `lruCapacityPerServer` covers the rest.\r\n- **vLLM**: no version reports the CPU cache size. We can see that offloading is enabled (the connector's `vllm:kv_offload_*` metrics appear at startup) but not how big it is, so we size to the GPU cache (`num_gpu_blocks`) and log the warning. Exact sizing needs a new `OffloadingConnector` metric: vllm-project/vllm#29605 tried populating the legacy `num_cpu_blocks` field and was rejected because offloading is now a connector concern, the maintainer wants the offload connector to export its own capacity metric instead.\r\n- **trtllm-serve**: `trtllm_kv_cache_max_blocks` already counts GPU and CPU cache blocks together, so sizing was already correct and nothing changes. The exception is the experimental V2 cache manager, where this metric is GPU-only, so those deployments need `lruCapacityPerServer`.\r\n\r\nUnrelated fix picked up along the way: newer SGLang (v0.5.16) renamed its block size and block count metrics, so the default SGLang mapping now reads both the old and new names.\r\n\r\n**Which issue(s) this PR fixes**:\r\nFixes #1650\r\n\r\n**Testing**:\r\n\r\n- [x] Unit tests: extractor (max semantics, detection via series and label) and producer (capacity order, LRU resize)\r\n- [x] Fixture tests: default engine mappings run against real `/metrics` scrapes, checked in as `testdata/*.prom`\r\n- [x] Live verification on 8 deployments (2x8xH200), summarized below\r\n\r\nAll vLLM rows are v0.26.0 (latest release at time of testing). SGLang versions tested were v0.5.16 and v0.5.9 respectively. Both trtllm rows are v1.3.0rc23.\r\n\r\n| Deployment | Observed | Outcome |\r\n| --- | --- | --- |\r\n| vLLM, no offload | no `kv_offload_*` series, label `None` | correct |\r\n| vLLM, offload enabled | `kv_offload_*` series present via both `--kv-offloading-size` and `--kv-transfer-config`, label set or `None` respectively | undercounts + warning |\r\n| SGLang v0.5.10+, hicache on | `hicache_host_total_tokens = 400001`, matching `--hicache-ratio 2` x 200000 GPU tokens | correct |\r\n| SGLang pre-v0.5.10, hicache on | CPU metric absent, only `max_total_num_tokens = 200000` | undercounts |\r\n| trtllm legacy manager, 4GiB host cache | `max_blocks = 21129`, host blocks included | correct |\r\n| trtllm V2 manager, 4GiB host cache | `max_blocks = 19958`, GPU blocks only | undercounts |\r\n\r\nThe 4GiB host cache is ~1170 blocks at ~3.6MiB per 32-token block, matching the 21129 vs 19958 gap between the two managers.\r\n\r\nIn the undercount cases, user must turn `autotune: false` and manually set `lruCapacityPerServer` to override the metric manually. There is unfortunately no Prometheus visibility into the SGLang and trtllm undercounts so we can't log a warning.\r\n\r\n**PR size breakdown** (most of the diff is tests):\r\n\r\n- ~295 lines of code and docs (extractor specs, producer sizing, LRU resize, README and metric protocol docs)\r\n- ~557 lines of test code (unit tables plus fixture tests)\r\n- ~307 lines of `testdata/*.prom`, real `/metrics` scrapes from the deployments above, so the default engine mappings stay verified against actual engine output\r\n\r\n**Release note**:\r\n```release-note\r\nThe approx-prefix-cache-producer now sizes its per-endpoint LRU to the offload-extended KV cache capacity: SGLang `hicache_host_total_tokens` is used when present (v0.5.10+), vLLM KV uses GPU blocks + undercount warning, trtllm is already the combined amount unless v2 kv-cache manager is used which undercounts.\r\n```\r\n\r\n\r\n",
      "reactions": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2292/reactions",
        "total_count": 0,
        "+1": 0,
        "-1": 0,
        "laugh": 0,
        "hooray": 0,
        "confused": 0,
        "heart": 0,
        "rocket": 0,
        "eyes": 0
      },
      "timeline_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2292/timeline",
      "performed_via_github_app": null,
      "state_reason": null,
      "score": 1.0
    },
    {
      "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2476",
      "repository_url": "https://api.github.com/repos/llm-d/llm-d-router",
      "labels_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2476/labels{/name}",
      "comments_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2476/comments",
      "events_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2476/events",
      "html_url": "https://github.com/llm-d/llm-d-router/pull/2476",
      "id": 5200473457,
      "node_id": "PR_kwDOOkxMpc8AAAABAYL41Q",
      "number": 2476,
      "title": "feat(metrics): add ATOM as a built-in engine",
      "user": {
        "login": "Jasen2201",
        "id": 38908462,
        "node_id": "MDQ6VXNlcjM4OTA4NDYy",
        "avatar_url": "https://avatars.githubusercontent.com/u/38908462?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/Jasen2201",
        "html_url": "https://github.com/Jasen2201",
        "followers_url": "https://api.github.com/users/Jasen2201/followers",
        "following_url": "https://api.github.com/users/Jasen2201/following{/other_user}",
        "gists_url": "https://api.github.com/users/Jasen2201/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/Jasen2201/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/Jasen2201/subscriptions",
        "organizations_url": "https://api.github.com/users/Jasen2201/orgs",
        "repos_url": "https://api.github.com/users/Jasen2201/repos",
        "events_url": "https://api.github.com/users/Jasen2201/events{/privacy}",
        "received_events_url": "https://api.github.com/users/Jasen2201/received_events",
        "type": "User",
        "user_view_type": "public",
        "site_admin": false
      },
      "labels": [
        {
          "id": 10876285519,
          "node_id": "LA_kwDOOkxMpc8AAAACiEbyTw",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/size/M",
          "name": "size/M",
          "color": "eebb00",
          "default": false,
          "description": "Denotes a PR that changes 30-99 lines, ignoring generated files.",
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 11058723766,
          "node_id": "LA_kwDOOkxMpc8AAAACkya7tg",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/area/epp",
          "name": "area/epp",
          "color": "ebd368",
          "default": false,
          "description": "",
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 11740301369,
          "node_id": "LA_kwDOOkxMpc8AAAACu8bIOQ",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/area/datalayer",
          "name": "area/datalayer",
          "color": "ebd368",
          "default": false,
          "description": "",
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 11740305283,
          "node_id": "LA_kwDOOkxMpc8AAAACu8bXgw",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/area/docs",
          "name": "area/docs",
          "color": "ebd368",
          "default": false,
          "description": "",
          "archived_at": null,
          "archived_by": null
        }
      ],
      "state": "closed",
      "locked": false,
      "assignees": [

      ],
      "milestone": null,
      "comments": 1,
      "created_at": "2026-08-20T06:37:49Z",
      "updated_at": "2026-09-11T15:06:01Z",
      "closed_at": "2026-09-11T15:06:01Z",
      "assignee": null,
      "author_association": "CONTRIBUTOR",
      "issue_field_values": [

      ],
      "type": null,
      "active_lock_reason": null,
      "draft": false,
      "pull_request": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/pulls/2476",
        "html_url": "https://github.com/llm-d/llm-d-router/pull/2476",
        "diff_url": "https://github.com/llm-d/llm-d-router/pull/2476.diff",
        "patch_url": "https://github.com/llm-d/llm-d-router/pull/2476.patch",
        "merged_at": "2026-09-11T15:06:00Z"
      },
      "body": "ATOM is an LLM inference engine for AMD GPUs built on AITER kernels. It exposes the load signals this extractor needs -- queue depth, running count, KV-cache utilization, and cache geometry -- under an `atom:` prefix, following the same convention as `sglang:` and `trtllm_`.\r\n\r\nRegistering it here means a deployment only has to label its pods with `llm-d.ai/engine-type: atom`, rather than carrying an engineConfigs block that restates metric names the engine already fixes.\r\n",
      "reactions": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2476/reactions",
        "total_count": 1,
        "+1": 1,
        "-1": 0,
        "laugh": 0,
        "hooray": 0,
        "confused": 0,
        "heart": 0,
        "rocket": 0,
        "eyes": 0
      },
      "timeline_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2476/timeline",
      "performed_via_github_app": null,
      "state_reason": null,
      "score": 1.0
    },
    {
      "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2619",
      "repository_url": "https://api.github.com/repos/llm-d/llm-d-router",
      "labels_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2619/labels{/name}",
      "comments_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2619/comments",
      "events_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2619/events",
      "html_url": "https://github.com/llm-d/llm-d-router/pull/2619",
      "id": 5292699238,
      "node_id": "PR_kwDOOkxMpc8AAAABBgnYHw",
      "number": 2619,
      "title": "fix(metrics): read SGLang cache capacity from dedicated gauges",
      "user": {
        "login": "satyamg1620",
        "id": 97394641,
        "node_id": "U_kgDOBc4f0Q",
        "avatar_url": "https://avatars.githubusercontent.com/u/97394641?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/satyamg1620",
        "html_url": "https://github.com/satyamg1620",
        "followers_url": "https://api.github.com/users/satyamg1620/followers",
        "following_url": "https://api.github.com/users/satyamg1620/following{/other_user}",
        "gists_url": "https://api.github.com/users/satyamg1620/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/satyamg1620/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/satyamg1620/subscriptions",
        "organizations_url": "https://api.github.com/users/satyamg1620/orgs",
        "repos_url": "https://api.github.com/users/satyamg1620/repos",
        "events_url": "https://api.github.com/users/satyamg1620/events{/privacy}",
        "received_events_url": "https://api.github.com/users/satyamg1620/received_events",
        "type": "User",
        "user_view_type": "public",
        "site_admin": false
      },
      "labels": [
        {
          "id": 9034267324,
          "node_id": "LA_kwDOOkxMpc8AAAACGnv6vA",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/kind/bug",
          "name": "kind/bug",
          "color": "e11d21",
          "default": false,
          "description": "Categorizes issue or PR as related to a bug.",
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 10876285519,
          "node_id": "LA_kwDOOkxMpc8AAAACiEbyTw",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/size/M",
          "name": "size/M",
          "color": "eebb00",
          "default": false,
          "description": "Denotes a PR that changes 30-99 lines, ignoring generated files.",
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 11058723766,
          "node_id": "LA_kwDOOkxMpc8AAAACkya7tg",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/area/epp",
          "name": "area/epp",
          "color": "ebd368",
          "default": false,
          "description": "",
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 11740301369,
          "node_id": "LA_kwDOOkxMpc8AAAACu8bIOQ",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/area/datalayer",
          "name": "area/datalayer",
          "color": "ebd368",
          "default": false,
          "description": "",
          "archived_at": null,
          "archived_by": null
        },
        {
          "id": 11740305283,
          "node_id": "LA_kwDOOkxMpc8AAAACu8bXgw",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/area/docs",
          "name": "area/docs",
          "color": "ebd368",
          "default": false,
          "description": "",
          "archived_at": null,
          "archived_by": null
        }
      ],
      "state": "closed",
      "locked": false,
      "assignees": [

      ],
      "milestone": null,
      "comments": 0,
      "created_at": "2026-08-30T14:53:38Z",
      "updated_at": "2026-08-31T18:15:25Z",
      "closed_at": "2026-08-31T18:15:25Z",
      "assignee": null,
      "author_association": "CONTRIBUTOR",
      "issue_field_values": [

      ],
      "type": null,
      "active_lock_reason": null,
      "draft": false,
      "pull_request": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/pulls/2619",
        "html_url": "https://github.com/llm-d/llm-d-router/pull/2619",
        "diff_url": "https://github.com/llm-d/llm-d-router/pull/2619.diff",
        "patch_url": "https://github.com/llm-d/llm-d-router/pull/2619.patch",
        "merged_at": "2026-08-31T18:15:25Z"
      },
      "body": "**What type of PR is this?**\r\n\r\n/kind bug\r\n\r\n**What this PR does / why we need it**:\r\n\r\nSGLang exposes KV cache capacity as two dedicated gauges: `sglang:page_size` (\"KV cache page size in tokens\") and `sglang:num_pages` (\"Number of KV cache pages\"). The built-in `sglang` engine config instead reads both from labels on\r\n`sglang:cache_config_info`, a metric SGLang does not emit; sgl-project/sglang#24197 replaced that info gauge with the dedicated ones.\r\n\r\nOn an SGLang pool this has two effects:\r\n\r\n- `CacheBlockSize` and `CacheNumBlocks` stay at 0, so `approximate-prefix` auto-tuning falls back to the configured `BlockSizeTokens` and `LRUCapacityPerServer` rather than sizing the index from the endpoint.\r\n- Every scrape of every SGLang endpoint fails with `metric family \"sglang:cache_config_info\" not found`, which increments `datalayer_extract_errors_total` and logs at DEBUG. The counter is keyed by source and extractor type with no metric name, so this pins it non-zero and masks unrelated extraction failures.\r\n\r\nThe `sglang` config now reads the two gauges through the existing `cacheBlockSizeSpec` / `cacheNumBlocksSpec` direct-gauge path, which `trtllm-serve` and `triton-tensorrt-llm` already use, and leaves `cacheInfoSpec` empty. No new extraction plumbing.\r\n\r\nDeclaring `cacheInfoSpec` alongside the gauges would keep both metric shapes working, but the absent family would then bump `datalayer_extract_errors_total` on every scrape against either SGLang version, so the built-in config targets the emitted metrics only. An operator running a build that still emits `sglang:cache_config_info` can restore the label mapping with an `engineConfigs` entry named `sglang`, which replaces the built-in one wholesale:\r\n\r\n```yaml\r\ntype: core-metrics-extractor\r\nparameters:\r\n  engineConfigs:\r\n    - name: \"sglang\"\r\n      queuedRequestsSpec: \"sglang:num_queue_reqs\"\r\n      runningRequestsSpec: \"sglang:num_running_reqs\"\r\n      kvUsageSpec: \"sglang:token_usage\"\r\n      loraSpec: \"\"\r\n      cacheInfoSpec: \"sglang:cache_config_info\"\r\n      cacheBlockSizeLabelName: \"page_size\"\r\n      cacheNumBlocksLabelName: \"num_pages\"\r\n```\r\n\r\n**Which issue(s) this PR fixes**:\r\n\r\nFixes #2294\r\n\r\n**Test plan**:\r\n\r\n- [x] Added coverage for the built-in SGLang engine config reading cache block size and block count from the dedicated gauges, and extracting without error when no cache-config info gauge is present. The test fails on the previous mapping with `metric family \"sglang:cache_config_info\" not found`.\r\n- [x] `make test-unit-epp`\r\n- [x] `make lint`\r\n- [x] Verified on a kind cluster against an endpoint labeled `llm-d.ai/engine-type: sglang` serving the current SGLang metric set: the EPP records `CacheBlockSize: 64` and `CacheNumBlocks: 11147` from `sglang:page_size` and `sglang:num_pages`, and `datalayer_extract_errors_total` stays unset where the previous mapping incremented it on every scrape. vLLM endpoints on the same pool continue to resolve cache capacity from `vllm:cache_config_info` labels.\r\n\r\n**Release note**:\r\n\r\n```release-note\r\nFix KV cache capacity metadata for SGLang endpoints, which is read from the `sglang:page_size` and `sglang:num_pages` metrics. Current SGLang releases do not emit `sglang:cache_config_info`, so approximate prefix cache auto-tuning fell back to its configured defaults and each scrape of an SGLang endpoint recorded a data layer extraction error.\r\n```\r\n",
      "reactions": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2619/reactions",
        "total_count": 0,
        "+1": 0,
        "-1": 0,
        "laugh": 0,
        "hooray": 0,
        "confused": 0,
        "heart": 0,
        "rocket": 0,
        "eyes": 0
      },
      "timeline_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/2619/timeline",
      "performed_via_github_app": null,
      "state_reason": null,
      "score": 1.0
    },
    {
      "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/1230",
      "repository_url": "https://api.github.com/repos/llm-d/llm-d-router",
      "labels_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/1230/labels{/name}",
      "comments_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/1230/comments",
      "events_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/1230/events",
      "html_url": "https://github.com/llm-d/llm-d-router/pull/1230",
      "id": 4482790052,
      "node_id": "PR_kwDOOkxMpc7dVseu",
      "number": 1230,
      "title": "Fix Triton Routing Failure in Legacy Metrics Mode",
      "user": {
        "login": "weizhoublue",
        "id": 45163302,
        "node_id": "MDQ6VXNlcjQ1MTYzMzAy",
        "avatar_url": "https://avatars.githubusercontent.com/u/45163302?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/weizhoublue",
        "html_url": "https://github.com/weizhoublue",
        "followers_url": "https://api.github.com/users/weizhoublue/followers",
        "following_url": "https://api.github.com/users/weizhoublue/following{/other_user}",
        "gists_url": "https://api.github.com/users/weizhoublue/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/weizhoublue/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/weizhoublue/subscriptions",
        "organizations_url": "https://api.github.com/users/weizhoublue/orgs",
        "repos_url": "https://api.github.com/users/weizhoublue/repos",
        "events_url": "https://api.github.com/users/weizhoublue/events{/privacy}",
        "received_events_url": "https://api.github.com/users/weizhoublue/received_events",
        "type": "User",
        "user_view_type": "public",
        "site_admin": false
      },
      "labels": [
        {
          "id": 10876279244,
          "node_id": "LA_kwDOOkxMpc8AAAACiEbZzA",
          "url": "https://api.github.com/repos/llm-d/llm-d-router/labels/size/XS",
          "name": "size/XS",
          "color": "009900",
          "default": false,
          "description": "Denotes a PR that changes 0-9 lines, ignoring generated files.",
          "archived_at": null,
          "archived_by": null
        }
      ],
      "state": "closed",
      "locked": false,
      "assignees": [

      ],
      "milestone": null,
      "comments": 1,
      "created_at": "2026-05-20T03:10:50Z",
      "updated_at": "2026-06-05T23:55:29Z",
      "closed_at": "2026-05-20T03:32:13Z",
      "assignee": null,
      "author_association": "CONTRIBUTOR",
      "issue_field_values": [

      ],
      "type": null,
      "active_lock_reason": null,
      "draft": false,
      "pull_request": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/pulls/1230",
        "html_url": "https://github.com/llm-d/llm-d-router/pull/1230",
        "diff_url": "https://github.com/llm-d/llm-d-router/pull/1230.diff",
        "patch_url": "https://github.com/llm-d/llm-d-router/pull/1230.patch",
        "merged_at": "2026-05-20T03:32:13Z"
      },
      "body": "**What type of PR is this?**\r\n \r\n/kind bug\r\n \r\n \r\n**What this PR does / why we need it**:\r\n \r\nThe issue is triggered when a user deploys the application using the legacy metrics collection mode (latencyPredictor.enabled = false or enableLegacyMetrics = true) while specifically configuring the inference pool to use the standard triton model server type.\r\n\r\nThe EPP  fails to collect accurate load metrics (such as queued requests and running requests) from the Triton backends. Without accurate load data, the queue-scorer makes incorrect routing decisions, which can lead to severe request queuing, load imbalances across the backends, and potential performance degradation (hotspotting).\r\n\r\n\r\n Root Cause: \r\nThe Helm template (config/charts/epplib/templates/_deployment.yaml) responsible for rendering the legacy metrics CLI arguments explicitly handled other engine types (like vllm, sglang, triton-tensorrt-llm, and trtllm-serve) but completely omitted the conditional block for the standard  triton type. \r\nConsequently, EPP defaulted to using vLLM metric names, which do not exist on a Triton server.\r\n\r\n   * Fix Method: Added a missing conditional block ({{- if eq $modelServerType \"triton\" }}) in the _deployment.yaml template to explicitly map the total-queued-requests-metric and total-running-requests-metric parameters to their correct Triton native Prometheus metric names, aligning them with the existing internal data layer definitions.\r\n\r\n\r\n**Which issue(s) this PR fixes**:\r\n<!--\r\n*Automatically closes linked issue when PR is merged.\r\nUsage: `Fixes #<issue number>`, or `Fixes (paste link of issue)`.\r\n-->\r\nFixes #\r\n\r\n**Release note** _(write `NONE` if no user-facing change)_:\r\n```release-note\r\nNONE\r\n```\r\n",
      "reactions": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/1230/reactions",
        "total_count": 0,
        "+1": 0,
        "-1": 0,
        "laugh": 0,
        "hooray": 0,
        "confused": 0,
        "heart": 0,
        "rocket": 0,
        "eyes": 0
      },
      "timeline_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/1230/timeline",
      "performed_via_github_app": null,
      "state_reason": null,
      "score": 1.0
    },
    {
      "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/966",
      "repository_url": "https://api.github.com/repos/llm-d/llm-d-router",
      "labels_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/966/labels{/name}",
      "comments_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/966/comments",
      "events_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/966/events",
      "html_url": "https://github.com/llm-d/llm-d-router/pull/966",
      "id": 4371243019,
      "node_id": "PR_kwDOOkxMpc7XvvQh",
      "number": 966,
      "title": "improve metrics logging",
      "user": {
        "login": "andreyod",
        "id": 16204273,
        "node_id": "MDQ6VXNlcjE2MjA0Mjcz",
        "avatar_url": "https://avatars.githubusercontent.com/u/16204273?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/andreyod",
        "html_url": "https://github.com/andreyod",
        "followers_url": "https://api.github.com/users/andreyod/followers",
        "following_url": "https://api.github.com/users/andreyod/following{/other_user}",
        "gists_url": "https://api.github.com/users/andreyod/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/andreyod/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/andreyod/subscriptions",
        "organizations_url": "https://api.github.com/users/andreyod/orgs",
        "repos_url": "https://api.github.com/users/andreyod/repos",
        "events_url": "https://api.github.com/users/andreyod/events{/privacy}",
        "received_events_url": "https://api.github.com/users/andreyod/received_events",
        "type": "User",
        "user_view_type": "public",
        "site_admin": false
      },
      "labels": [

      ],
      "state": "closed",
      "locked": false,
      "assignees": [

      ],
      "milestone": null,
      "comments": 2,
      "created_at": "2026-05-03T08:09:17Z",
      "updated_at": "2026-05-03T13:26:10Z",
      "closed_at": "2026-05-03T13:26:09Z",
      "assignee": null,
      "author_association": "CONTRIBUTOR",
      "issue_field_values": [

      ],
      "type": null,
      "active_lock_reason": null,
      "draft": false,
      "pull_request": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/pulls/966",
        "html_url": "https://github.com/llm-d/llm-d-router/pull/966",
        "diff_url": "https://github.com/llm-d/llm-d-router/pull/966.diff",
        "patch_url": "https://github.com/llm-d/llm-d-router/pull/966.patch",
        "merged_at": "2026-05-03T13:26:09Z"
      },
      "body": "**What type of PR is this?**\r\n/kind cleanup\r\n\r\n**What this PR does / why we need it**:\r\n- Skip deprecated metric flags (e.g. `--total-queued-requests-metric`) from the\r\n  EPP startup \"Flags processed\" log.\r\n- Add per-endpoint metric names to the existing TRACE-level \"Refreshed metrics\" log\r\n  so operators can see which Prometheus metrics are being collected for each endpoint.\r\n- Fix a small bug, causing model server metrics mappings to incorrectly report \"all specs enabled\".\r\n\r\nBefore:\r\n```\r\n{\"level\":\"info\",\"ts\":1777474399.9769688,\"logger\":\"setup\",\"caller\":\"runner/runner.go:177\",\"msg\":\"Flags processed\",\"flags\":{\"cache-info-metric\":\"vllm:cache_config_info\",\"cert-path\":\"\",\"config-file\":\"/config/optimized-baseline-plugins.yaml\",\"config-text\":\"\",\"disable-endpoint-subset-filter\":false,\"enable-cert-reload\":false,\"enable-pprof\":true,\"endpoint-selector\":\"\",\"endpoint-target-ports\":{},\"grpc-health-port\":9003,\"grpc-port\":9002,\"ha-enable-leader-election\":false,\"health-checking\":false,\"kv-cache-usage-percentage-metric\":\"vllm:kv_cache_usage_perc\",\"lora-info-metric\":\"vllm:lora_requests_info\",\"metrics-endpoint-auth\":true,\"metrics-port\":9090,\"metrics-staleness-threshold\":2000000000,\"model-server-metrics-https-insecure-skip-verify\":true,\"model-server-metrics-path\":\"/metrics\",\"model-server-metrics-port\":0,\"model-server-metrics-scheme\":\"http\",\"pool-group\":\"inference.networking.k8s.io\",\"pool-name\":\"optimized-baseline\",\"pool-namespace\":\"andreyo\",\"refresh-metrics-interval\":50000000,\"refresh-prometheus-metrics-interval\":5000000000,\"secure-serving\":true,\"total-queued-requests-metric\":\"vllm:num_requests_waiting\",\"total-running-requests-metric\":\"vllm:num_requests_running\",\"tracing\":false,\"v\":5,\"zap-devel\":{},\"zap-encoder\":{},\"zap-log-level\":{},\"zap-stacktrace-level\":{},\"zap-time-encoding\":{}}}\r\n\r\n{\"level\":\"info\",\"ts\":1777474399.9879482,\"caller\":\"metrics/factories.go:197\",\"msg\":\"Registered engine mapping\",\"engine\":\"vllm\",\"mapping\":\"Mapping{all specs enabled}\"}\r\n{\"level\":\"info\",\"ts\":1777474399.987983,\"caller\":\"metrics/factories.go:197\",\"msg\":\"Registered engine mapping\",\"engine\":\"sglang\",\"mapping\":\"Mapping{all specs enabled}\"}\r\n{\"level\":\"info\",\"ts\":1777474399.9880147,\"caller\":\"metrics/factories.go:197\",\"msg\":\"Registered engine mapping\",\"engine\":\"trtllm-serve\",\"mapping\":\"Mapping{all specs enabled}\"}\r\n{\"level\":\"info\",\"ts\":1777474399.9880602,\"caller\":\"metrics/factories.go:197\",\"msg\":\"Registered engine mapping\",\"engine\":\"triton-tensorrt-llm\",\"mapping\":\"Mapping{all specs enabled}\"}\r\n\r\n{\"level\":\"trace\",\"ts\":1777474400.2647107,\"caller\":\"metrics/extractor.go:190\",\"msg\":\"Refreshed metrics\",\"endpoint\":{\"name\":\"optimized-baseline-nvidia-gpu-sglang-decode-64c79b8665-hprht-rank-0\",\"namespace\":\"andreyo\"},\"updated\":\"{ActiveModels:map[] WaitingModels:map[] MaxActiveModels:0 RunningRequestsSize:0 WaitingQueueSize:0 KVCacheUsagePercent:0 KvCacheMaxTokenCapacity:0 CacheBlockSize:1 CacheNumBlocks:16839 UpdateTime:2026-04-29 14:53:20.264708482 +0000 UTC m=+0.311144575}\"}\r\n```\r\n\r\nAfter:\r\n```\r\n{\"level\":\"info\",\"ts\":1777792640.84343,\"logger\":\"setup\",\"caller\":\"runner/runner.go:187\",\"msg\":\"Flags processed\",\"flags\":{\"cert-path\":\"\",\"config-file\":\"/config/optimized-baseline-plugins.yaml\",\"config-text\":\"\",\"disable-endpoint-subset-filter\":false,\"enable-cert-reload\":false,\"enable-pprof\":true,\"endpoint-selector\":\"\",\"endpoint-target-ports\":{},\"grpc-health-port\":9003,\"grpc-port\":9002,\"ha-enable-leader-election\":false,\"health-checking\":false,\"metrics-endpoint-auth\":true,\"metrics-port\":9090,\"metrics-staleness-threshold\":2000000000,\"model-server-metrics-https-insecure-skip-verify\":true,\"model-server-metrics-path\":\"/metrics\",\"model-server-metrics-port\":0,\"model-server-metrics-scheme\":\"http\",\"pool-group\":\"inference.networking.k8s.io\",\"pool-name\":\"optimized-baseline\",\"pool-namespace\":\"andreyo\",\"refresh-metrics-interval\":50000000,\"refresh-prometheus-metrics-interval\":5000000000,\"secure-serving\":true,\"tracing\":false,\"v\":5,\"zap-devel\":{},\"zap-encoder\":{},\"zap-log-level\":{},\"zap-stacktrace-level\":{},\"zap-time-encoding\":{}}}\r\n\r\n{\"level\":\"info\",\"ts\":1777792640.851209,\"caller\":\"metrics/factories.go:197\",\"msg\":\"Registered engine mapping\",\"engine\":\"vllm\",\"mapping\":\"Mapping{all specs enabled}\"}\r\n{\"level\":\"info\",\"ts\":1777792640.8512416,\"caller\":\"metrics/factories.go:197\",\"msg\":\"Registered engine mapping\",\"engine\":\"sglang\",\"mapping\":\"Mapping{disabled: [lora]}\"}\r\n{\"level\":\"info\",\"ts\":1777792640.851273,\"caller\":\"metrics/factories.go:197\",\"msg\":\"Registered engine mapping\",\"engine\":\"trtllm-serve\",\"mapping\":\"Mapping{disabled: [lora, cacheInfo]}\"}\r\n{\"level\":\"info\",\"ts\":1777792640.8513193,\"caller\":\"metrics/factories.go:197\",\"msg\":\"Registered engine mapping\",\"engine\":\"triton-tensorrt-llm\",\"mapping\":\"Mapping{disabled: [lora, cacheInfo]}\"}\r\n\r\n{\"level\":\"trace\",\"ts\":1777792641.3718956,\"caller\":\"metrics/extractor.go:190\",\"msg\":\"Refreshed metrics\",\"endpoint\":{\"name\":\"optimized-baseline-nvidia-gpu-sglang-decode-64c79b8665-pglf7-rank-0\",\"namespace\":\"andreyo\"},\"metrics\":[\"sglang:num_queue_reqs\",\"sglang:num_running_reqs\",\"sglang:token_usage\",\"sglang:cache_config_info\"],\"updated\":\"{ActiveModels:map[] WaitingModels:map[] MaxActiveModels:0 RunningRequestsSize:0 WaitingQueueSize:0 KVCacheUsagePercent:0 KvCacheMaxTokenCapacity:0 CacheBlockSize:1 CacheNumBlocks:16839 UpdateTime:2026-05-03 07:17:21.371894228 +0000 UTC m=+0.553513174}\"}\r\n```\r\n**Release note** _(write `NONE` if no user-facing change)_:\r\n```release-note\r\nNONE\r\n```\r\n",
      "reactions": {
        "url": "https://api.github.com/repos/llm-d/llm-d-router/issues/966/reactions",
        "total_count": 0,
        "+1": 0,
        "-1": 0,
        "laugh": 0,
        "hooray": 0,
        "confused": 0,
        "heart": 0,
        "rocket": 0,
        "eyes": 0
      },
      "timeline_url": "https://api.github.com/repos/llm-d/llm-d-router/issues/966/timeline",
      "performed_via_github_app": null,
      "state_reason": null,
      "score": 1.0
    }
  ],
  "search_type": "lexical"
}

```

