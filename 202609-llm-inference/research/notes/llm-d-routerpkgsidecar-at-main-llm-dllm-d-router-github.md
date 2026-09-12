---
title: llm-d-router/pkg/sidecar at main · llm-d/llm-d-router · GitHub
id: llm-d-routerpkgsidecar-at-main-llm-dllm-d-router-github
tags:
- llmd-router-nonk8s-backends-54b774
- pd-sidecar
- source-hub
created: '2026-09-11T21:45:44.011687Z'
updated: '2026-09-12T03:37:37.791132Z'
source: https://github.com/llm-d/llm-d-router/tree/main/pkg/sidecar
source_domain: github.com
fetched_at: '2026-09-11T21:45:43.983156Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: GitHub directory listing (commit history only) for llm-d-router/pkg/sidecar
  at main. Confirms via commit db208e3d (#379, Shmuel Kallner) that 'Routing Sidecar'
  was moved from its own repo into the (then) inference-scheduler repo, which has
  since become llm-d-router; and via commit 73df64d (#1888) that the sidecar gained
  a P2P/OffloadingConnector KV-transfer mode (--kv-connector=p2p renamed to 'offloading')
  on Jul 4 2026, alongside NIXLv2 and Mooncake connectors. Directory tree itself not
  rendered (GitHub JS-only file list); used GitHub API instead to enumerate actual
  files.
---

[Skip to content](https://github.com/llm-d/llm-d-router/tree/main/pkg/sidecar#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/sidecar) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/sidecar) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/sidecar) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/tree/main/pkg/sidecar).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# sidecar
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[telemetry: migrate remaining llm_d and gen_ai attributes to semconv (](https://github.com/llm-d/llm-d-router/commit/db208e3d5cb5975c88a784dfd56b1977829d828a)[#…](https://github.com/llm-d/llm-d-router/pull/2516)
Open commit detailsfailure
Sep 11, 2026
[db208e3](https://github.com/llm-d/llm-d-router/commit/db208e3d5cb5975c88a784dfd56b1977829d828a) · Sep 11, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/sidecar)
Open commit details
History
/
# sidecar
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
|   |   | [feat(sidecar): add P2P connector for OffloadingConnector disaggregati…](https://github.com/llm-d/llm-d-router/commit/73df64d67691f99b00c593d714d7c19ebe7ea390 "feat\(sidecar\): add P2P connector for OffloadingConnector disaggregation \(#1888\)

* feat\(sidecar\): add P2P connector for OffloadingConnector disaggregation

Implements the vLLM OffloadingConnector P/D disaggregation protocol
\(PR #42285\) as a new --kv-connector=p2p mode in the decode sidecar.

The sidecar concurrently dispatches a prefill leg \(kv_request_id only,
capped to 1 token\) and a decode leg \(kv_request_id + remote_host +
remote_port\) so the vLLM decoder can pull KV from the prefiller's p2p
secondary tier via ZMQ/NIXL. Prefill failures are silent; decode
failures propagate to the client.

Adds --p2p-connector-port / P2P_CONNECTOR_PORT flag \(default 7777\) and
documents the required vLLM --kv-transfer-config on the serving pods.

Fixes #1197

Signed-off-by: nilig <nili.ifergan@gmail.com>

* test\(sidecar\): extract repeated request body to shared constant

Extract the chat completions request body with max_completion_tokens into
a package-level constant to satisfy goconst \(3 occurrences across
connector_test, connector_mooncake_test, connector_p2p_test\).

Signed-off-by: nilig <nili.ifergan@gmail.com>

* refactor\(sidecar\): rename kv connector p2p to offloading

Rename the connector value from 'p2p' to 'offloading' to match the vLLM
connector name \(OffloadingConnector\) and follow the existing naming
convention \(nixlv2->NixlConnector, mooncake->MooncakeConnector\).
The 'p2p' name conflicted with issue #1574 which reserves it for a
different mode.

The --p2p-connector-port flag name is unchanged; it maps to the vLLM
p2p secondary tier type, not the sidecar connector name.

Cluster-tested: sidecar starts with KVConnector=offloading and the
running P2P protocol log fires correctly end-to-end.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(sidecar\): reject offloading connector with data-parallel-size > 1

Wide-EP pods bind every DP rank to the same POD_IP:<p2p-connector-port>,
which causes a port conflict at startup. Reject this combination in
Validate\(\) until DP-aware support is implemented.

Document the restriction in disaggregation.md and fix a stale connector
name in the connector-specific flags table \(p2p -> offloading\).

Signed-off-by: nilig <niliguy@users.noreply.github.com>
Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(sidecar\): fix stale P2PConnector references after rename

Update flag help text and handleP2P doc comment to say
OffloadingConnector instead of P2PConnector, and drop the stale
"--kv-connector=p2p" reference from the flag description.

Signed-off-by: nilig <niliguy@users.noreply.github.com>
Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(sidecar\): fix stale P2PConnector references after rename

Update comments and docs to say OffloadingConnector instead of
P2PConnector, and replace "--kv-connector=p2p" references with
"--kv-connector=offloading". Flag/field/constant identifiers that
are intentionally named p2p \(they refer to the vLLM tier type\) are
left unchanged.

Signed-off-by: nilig <niliguy@users.noreply.github.com>
Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>
Signed-off-by: nilig <niliguy@users.noreply.github.com>")  | Jul 4, 2026  |  
|   |   |  [telemetry: migrate remaining llm_d and gen_ai attributes to semconv (](https://github.com/llm-d/llm-d-router/commit/db208e3d5cb5975c88a784dfd56b1977829d828a "telemetry: migrate remaining llm_d and gen_ai attributes to semconv \(#2516\)

Complete migration of all remaining llm_d.* and gen_ai.* OpenTelemetry
attributes across llm-d-router to centralized semantic conventions in
pkg/common/observability/semconv:
- Expand semconv/llm_d.go with typed keys and constructors for all EPP
  scheduling/filtering/scoring, token producer, KV cache indexing/lookup,
  KV events pub/sub, and sidecar proxy attributes.
- Migrate EPP scheduling, profile handler, data producers, and tests to
  semconv keys and constructors.
- Migrate kvcache indexer, prefix match, traced scorer, and kvblock to
  semconv.
- Migrate kvevents pool and ZeroMQ subscriber to semconv.
- Migrate sidecar proxy connectors \(nixlv2, p2p, decode, pd_common,
  chat completions\) to semconv.
- Add unit tests for all new attribute constructors in semconv/llm_d_test.go.

Signed-off-by: Yuan Gao <plateau451@gmail.com>")[#…](https://github.com/llm-d/llm-d-router/pull/2516)  | Sep 11, 2026  |  
|   |   | [feat: Moved the Routing Sidecar from its own repo to the inference-sc…](https://github.com/llm-d/llm-d-router/commit/6e2bb8a9beb911b435f80a82fe03d85a0de32936 "feat: Moved the Routing Sidecar from its own repo to the inference-scheduler repo \(#379\)

* Moved prefill header definition to common import

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Moved Routing Sidecar into this repo

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Moved Routing Sidecar tests into this repo

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Moved Routing Sidecar Dockerfile into this repo

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Added Routing Sidecar to Makefile

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Added Routing Sidecar to CI stream

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Fixed lint error

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Review fixes and added version info

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Test Nixl V2 instead of the deleted Nixl V1

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

* Fixed lint errors

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>

---------

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>")  | Oct 23, 2025  |  
| View all files |  
You can’t perform that action at this time. 
