---
title: 'GitHub - kubernetes-sigs/gateway-api-inference-extension: Gateway API Inference
  Extension · GitHub'
id: github-kubernetes-sigsgateway-api-inference-extension-gateway-api-inference-exte
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- epp-migration
- llm-d-router
created: '2026-09-11T19:43:30.266856Z'
updated: '2026-09-15T04:55:23.685944Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension
source_domain: github.com
fetched_at: '2026-09-11T19:43:30.245740Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Main GIE repo README (fetched 2026-09-11, HEAD as of commit 2711ff86, Sep
  11 2026). Load-bearing statement (added in commit c489161c, 2026-05-02, PR #2906,
  worded then as moving to ''llm-d/llm-d-inference-scheduler''; the link target was
  renamed to ''llm-d/llm-d-router'' in commit ebbb148c on 2026-06-30 as part of PR
  #2978''s mass docs cleanup, reflecting the upstream repo rename): ''> [!IMPORTANT]
  The Endpoint Picker (EPP), InferenceObjective and InferenceModelRewrite APIs, and
  Body Based Router (BBR) packages have moved to new repositories: EPP and associated
  APIs: llm-d/llm-d-router; BBR: llm-d/llm-d-inference-payload-processor. No new code
  will be accepted to these packages in this repository, and they will be archived
  soon. This move was proposed and discussed in issue #2430.'' The README states this
  repo ''will continue to host the lightweight EPP (LWEPP) and the InferencePool API,
  and will remain the primary location for development and maintenance of conformance
  tests.'' Also (added in the same ebbb148c cleanup commit) redefines EPP in the terms
  glossary as ''a data-plane component that communicates via the Envoy external processing
  protocol, and acts as the Router. It intercepts incoming inference requests and
  routes each request to the optimal model server replica'' — replacing the older
  ''Inference Scheduler'' framing. States ''IGW''s pluggable architecture was leveraged
  to enable the llm-d Router'' and that community meetings ''have moved to the llm-d
  Router community meeting.'' Lists 8 IGW capabilities including ''Disaggregated serving
  support with independently scaling pools'' and ''heterogeneous accelerators.'' A
  separate commit (57058491, Mar 29 2026, #2707) explicitly restored public Go API
  surfaces removed by a dead-code sweep because ''Downstream projects (like llm-d)
  rely on these exported symbols as a framework SDK contract'' — restoring PluginByType,
  ReadCycleStateKey, and Runner With* config options specifically for llm-d compatibility.'
utility_score: 13.0
---

*Suggested by [[llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared]] — GA v1 InferencePool/EPP standard cited as convergence layer above llm-d/AIBrix/Dynamo*

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


[**19** Branches](https://github.com/kubernetes-sigs/gateway-api-inference-extension/branches)[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tags)
Go to file
Open more actions menu
## Latest commit
[fix: EppUnAvailableFailOpen conformance nit (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/2711ff86a50ae23c5d32387779df8d3db9ca28e4)[#3037](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3037)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/2711ff86a50ae23c5d32387779df8d3db9ca28e4)
Open commit detailssuccess
Sep 11, 2026
[2711ff8](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/2711ff86a50ae23c5d32387779df8d3db9ca28e4) · Sep 11, 2026
## History
[1,790 Commits](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commits/main/)
Open commit details
1,790 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [cleanup: remove stale chart references and obsolete e2e workflows (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/afc39f52bdea298789aaea310983a57443288a72 "cleanup: remove stale chart references and obsolete e2e workflows \(#3009\)

Signed-off-by: Nicole Xin <nxin@google.com>")[#3009](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3009)  | Jul 30, 2026  |  
|   | Jun 11, 2026  |  
| [Remove inference objective, model rewrite and endpoint picker config …](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/88fd4792d1629f807589f0a1741fb4426f1d26d1 "Remove inference objective, model rewrite and endpoint picker config APIs \(#2973\)

* remove inference objective and model rewrite APIs

Signed-off-by: Nicole Xin <nxin@google.com>

* remove EndpointPickerConfig API

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 17, 2026  |  
|   |   |  [deps: update to Gateway API v1.6-rc.1 (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/3cc2b886d1e7e73b22dd3b745c5b3eba1be9c384 "deps: update to Gateway API v1.6-rc.1 \(#2964\)

* deps: update to Gateway API v1.6-rc.1

This is required for importers to import both of these modules together.

* Update generated clients for Go 1.26")[#2964](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2964)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/3cc2b886d1e7e73b22dd3b745c5b3eba1be9c384 "deps: update to Gateway API v1.6-rc.1 \(#2964\)

* deps: update to Gateway API v1.6-rc.1

This is required for importers to import both of these modules together.

* Update generated clients for Go 1.26")  | Jun 29, 2026  |  
| [cmd/lwepp](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/cmd/lwepp "This path skips through empty directories")  | [cmd/lwepp](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/cmd/lwepp "This path skips through empty directories")  |  [chore: add nicolexin as owner for pkg/lwepp and cmd/lwepp (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/d1da5ee7918d16440fa05f5c917f1a404ff883c0 "chore: add nicolexin as owner for pkg/lwepp and cmd/lwepp \(#3014\)

Signed-off-by: Nicole Xin <nxin@google.com>")[#3014](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3014)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/d1da5ee7918d16440fa05f5c917f1a404ff883c0 "chore: add nicolexin as owner for pkg/lwepp and cmd/lwepp \(#3014\)

Signed-off-by: Nicole Xin <nxin@google.com>")  | Aug 12, 2026  |  
| [config/crd](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/config/crd "This path skips through empty directories")  | [config/crd](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/config/crd "This path skips through empty directories")  |  [Cleanup Documentation and Miscellaneous Tools / Manifests (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")[#2978](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2978)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 30, 2026  |  
|   |   |  [fix: EppUnAvailableFailOpen conformance nit (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/2711ff86a50ae23c5d32387779df8d3db9ca28e4 "fix: EppUnAvailableFailOpen conformance nit \(#3037\)

Signed-off-by: zirain <zirain2009@gmail.com>")[#3037](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3037)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/2711ff86a50ae23c5d32387779df8d3db9ca28e4 "fix: EppUnAvailableFailOpen conformance nit \(#3037\)

Signed-off-by: zirain <zirain2009@gmail.com>")  | Sep 11, 2026  |  
|  [docs: clarify EPP endpoint backend scope (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/f540ee91d86f0112df98da8f65d7e689149cee2c "docs: clarify EPP endpoint backend scope \(#3003\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")[#3003](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3003)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/f540ee91d86f0112df98da8f65d7e689149cee2c "docs: clarify EPP endpoint backend scope \(#3003\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")  | Jul 23, 2026  |  
|  [Cleanup Documentation and Miscellaneous Tools / Manifests (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")[#2978](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2978)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 30, 2026  |  
|   |   |  [cleanup: remove stale dead code (>1 mo old) (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/57058491b83bda26bd743a3d202e41defa4d73b3 "cleanup: remove stale dead code \(>1 mo old\) \(#2707\)

* cleanup: remove stale dead code \(>1 mo old\)

Removes unreachable functions identified by a time-bounded dead code
audit to minimize the codebase delta before migration.

Methods from runners, metrics, and testing wrappers were removed after
verifying they were not modified in the last 30 days.

* revert: restore public API surfaces for downstream compatibility

Reverts removals of public/exported APIs and runner  configuration
setters from the dead code sweep, while  preserving safe internal dead
code cleanup.

Downstream projects \(like `llm-d`\) rely on these exported symbols as a
framework SDK contract.

Restored APIs:
- `PluginByType` in `pkg/epp/framework/interface/plugin/handle.go`
- `ReadCycleStateKey` in `pkg/epp/framework/interface/scheduling/cycle_state.go`
- `With*` configuration options for `Runner` in `cmd/epp/runner/runner.go`
- `WithExecutableName` option for `Runner` in `cmd/bbr/runner/runner.go`")[#2707](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2707)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/57058491b83bda26bd743a3d202e41defa4d73b3 "cleanup: remove stale dead code \(>1 mo old\) \(#2707\)

* cleanup: remove stale dead code \(>1 mo old\)

Removes unreachable functions identified by a time-bounded dead code
audit to minimize the codebase delta before migration.

Methods from runners, metrics, and testing wrappers were removed after
verifying they were not modified in the last 30 days.

* revert: restore public API surfaces for downstream compatibility

Reverts removals of public/exported APIs and runner  configuration
setters from the dead code sweep, while  preserving safe internal dead
code cleanup.

Downstream projects \(like `llm-d`\) rely on these exported symbols as a
framework SDK contract.

Restored APIs:
- `PluginByType` in `pkg/epp/framework/interface/plugin/handle.go`
- `ReadCycleStateKey` in `pkg/epp/framework/interface/scheduling/cycle_state.go`
- `With*` configuration options for `Runner` in `cmd/epp/runner/runner.go`
- `WithExecutableName` option for `Runner` in `cmd/bbr/runner/runner.go`")  | Mar 29, 2026  |  
|  [fix(lwepp): answer request headers before request body (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/e8d8f0632693d9773269831eb6610694f4bbe50e "fix\(lwepp\): answer request headers before request body \(#3033\)

* fix\(lwepp\): answer request headers before request body in ext_proc streams

Envoy's ext_proc filter accepts responses in the order it asked for them. A
request_body response that arrives while the filter is still waiting for the
request_headers response is out of order: Envoy counts it in
spurious_msgs_received, logs "Spurious response message 3 received on gRPC
stream", and abandons the stream for the rest of the request. Whether that
surfaces as a fail-open passthrough or an HTTP 500 depends on how far the
request has already progressed; with a body in flight it is a 500 with no body,
on every request.

In FULL_DUPLEX_STREAMED mode the server only learns the target endpoint once the
whole body has arrived, so the request_headers response carrying the endpoint
mutation is deferred to end of stream - while body chunks were being echoed back
as they arrived, ahead of it.

Hold the body chunks until the deferred request_headers response has been sent,
then flush them in arrival order. The bytes sent back are unchanged; only the
ordering is.

Reproduced against Istio 1.30.2, whose InferencePool ext_proc override sets
requestBodyMode FULL_DUPLEX_STREAMED: every conformance request carrying a body
returned 500 until this change.

Signed-off-by: Bartosz Majsak <bartosz.majsak@gmail.com>

* test\(lwepp\): cover chunked request bodies with a deferred headers response

The existing deferred-headers test sends the whole body in one chunk that ends
the stream, so the headers response is sent before any body response and the
ordering is never exercised.

Cover the chunked case: a body arriving in two chunks, only the second ending
the stream. While the first is in flight the endpoint is still unknown and the
headers response is deferred, which is when a body response sent ahead of it is
rejected by Envoy as spurious.

Asserts the first frame sent is the headers response and that both chunks are
forwarded in arrival order. Fails without the accompanying fix.

Signed-off-by: Bartosz Majsak <bartosz.majsak@gmail.com>

---------

Signed-off-by: Bartosz Majsak <bartosz.majsak@gmail.com>")[#3033](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3033)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/e8d8f0632693d9773269831eb6610694f4bbe50e "fix\(lwepp\): answer request headers before request body \(#3033\)

* fix\(lwepp\): answer request headers before request body in ext_proc streams

Envoy's ext_proc filter accepts responses in the order it asked for them. A
request_body response that arrives while the filter is still waiting for the
request_headers response is out of order: Envoy counts it in
spurious_msgs_received, logs "Spurious response message 3 received on gRPC
stream", and abandons the stream for the rest of the request. Whether that
surfaces as a fail-open passthrough or an HTTP 500 depends on how far the
request has already progressed; with a body in flight it is a 500 with no body,
on every request.

In FULL_DUPLEX_STREAMED mode the server only learns the target endpoint once the
whole body has arrived, so the request_headers response carrying the endpoint
mutation is deferred to end of stream - while body chunks were being echoed back
as they arrived, ahead of it.

Hold the body chunks until the deferred request_headers response has been sent,
then flush them in arrival order. The bytes sent back are unchanged; only the
ordering is.

Reproduced against Istio 1.30.2, whose InferencePool ext_proc override sets
requestBodyMode FULL_DUPLEX_STREAMED: every conformance request carrying a body
returned 500 until this change.

Signed-off-by: Bartosz Majsak <bartosz.majsak@gmail.com>

* test\(lwepp\): cover chunked request bodies with a deferred headers response

The existing deferred-headers test sends the whole body in one chunk that ends
the stream, so the headers response is sent before any body response and the
ordering is never exercised.

Cover the chunked case: a body arriving in two chunks, only the second ending
the stream. While the first is in flight the endpoint is still unknown and the
headers response is deferred, which is when a body response sent ahead of it is
rejected by Envoy as spurious.

Asserts the first frame sent is the headers response and that both chunks are
forwarded in arrival order. Fails without the accompanying fix.

Signed-off-by: Bartosz Majsak <bartosz.majsak@gmail.com>

---------

Signed-off-by: Bartosz Majsak <bartosz.majsak@gmail.com>")  | Sep 8, 2026  |  
|   |   | [add ACK Gateway v1.5.0 conformance report and restore ACK in the conf…](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/2c04351067be6f4fda9b397a677babd97f758cb1 "add ACK Gateway v1.5.0 conformance report and restore ACK in the conformant Gateway list \(#3016\)")  | Sep 2, 2026  |  
| [test/cel](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/test/cel "This path skips through empty directories")  | [test/cel](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/test/cel "This path skips through empty directories")  | [Remove inference objective, model rewrite and endpoint picker config …](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/88fd4792d1629f807589f0a1741fb4426f1d26d1 "Remove inference objective, model rewrite and endpoint picker config APIs \(#2973\)

* remove inference objective and model rewrite APIs

Signed-off-by: Nicole Xin <nxin@google.com>

* remove EndpointPickerConfig API

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 17, 2026  |  
|   |   |  [Cleanup Documentation and Miscellaneous Tools / Manifests (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")[#2978](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2978)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 30, 2026  |  
|   |   |  [Update CRD BundleVersion to main-dev. (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/6d9787bafc7edccb0524fd178242944aacfc9cdf "Update CRD BundleVersion to main-dev. \(#1216\)")[#1216](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1216)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/6d9787bafc7edccb0524fd178242944aacfc9cdf "Update CRD BundleVersion to main-dev. \(#1216\)")  | Jul 26, 2025  |  
|   |   |  [Enable kubeapilinter for GIE APIs (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/bc242de9b9f92c24ff07132e43783dab3decb976 "Enable kubeapilinter for GIE APIs \(#1366\)

* Enable kubeapilinter github action

* Fix findings on stable APIs

* Fix findings after rebase")[#1366](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1366)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/bc242de9b9f92c24ff07132e43783dab3decb976 "Enable kubeapilinter for GIE APIs \(#1366\)

* Enable kubeapilinter github action

* Fix findings on stable APIs

* Fix findings after rebase")  | Aug 14, 2025  |  
|   |   |   | Oct 21, 2024  |  
|   |   |  [Cleanup Documentation and Miscellaneous Tools / Manifests (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")[#2978](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2978)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 30, 2026  |  
|   |   |   | Aug 23, 2025  |  
|   |   |   | Jul 29, 2025  |  
|   |   |   | Aug 28, 2024  |  
|   |   |   | Aug 28, 2024  |  
|   |   |  [fix: pin go toolchain version to use the same version of go.mod (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a8ee59dc92f03ac3e68cf5d9d5742fd6d63b46ac "fix: pin go toolchain version to use the same version of go.mod \(#3048\)

Signed-off-by: Ricardo Pchevuzinske Katz <katz@redhat.com>")[#3048](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3048)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a8ee59dc92f03ac3e68cf5d9d5742fd6d63b46ac "fix: pin go toolchain version to use the same version of go.mod \(#3048\)

Signed-off-by: Ricardo Pchevuzinske Katz <katz@redhat.com>")  | Sep 11, 2026  |  
|   |   |  [remove wg serving leads from owners file (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/3bb9e19bdedbc96bc4234ce3e52edce47902a377 "remove wg serving leads from owners file \(#1445\)

Signed-off-by: Nir Rozenbaum <nirro@il.ibm.com>")[#1445](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1445)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/3bb9e19bdedbc96bc4234ce3e52edce47902a377 "remove wg serving leads from owners file \(#1445\)

Signed-off-by: Nir Rozenbaum <nirro@il.ibm.com>")  | Aug 24, 2025  |  
|   |   |  [Move elevran from reviewers to maintainers. (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/5285ebee87c9a2a9135d3f8a5b8e722abdaa78b1 "Move elevran from reviewers to maintainers. \(#2861\)

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2861](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2861)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/5285ebee87c9a2a9135d3f8a5b8e722abdaa78b1 "Move elevran from reviewers to maintainers. \(#2861\)

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Apr 19, 2026  |  
|   |   | [Remove inference objective, model rewrite and endpoint picker config …](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/88fd4792d1629f807589f0a1741fb4426f1d26d1 "Remove inference objective, model rewrite and endpoint picker config APIs \(#2973\)

* remove inference objective and model rewrite APIs

Signed-off-by: Nicole Xin <nxin@google.com>

* remove EndpointPickerConfig API

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 17, 2026  |  
|   |   |  [Cleanup Documentation and Miscellaneous Tools / Manifests (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")[#2978](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2978)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 30, 2026  |  
|   |   |  [release: align conformance module tagging (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/98a4cc33b62d08f69ff7e9e0b1ad1ed1257339d7 "release: align conformance module tagging \(#2767\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")[#2767](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2767)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/98a4cc33b62d08f69ff7e9e0b1ad1ed1257339d7 "release: align conformance module tagging \(#2767\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")  | Apr 3, 2026  |  
|   |   |   | Aug 28, 2024  |  
|   |   | [Update OWNERS, OWNERS_ALIASES, and SECURITY_CONTACTS](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/37a857506c200453c78728ab723a30dbb9100259 "Update OWNERS, OWNERS_ALIASES, and SECURITY_CONTACTS")  | Aug 28, 2024  |  
|   |   |  [Cleanup EPP and Latency Predictor (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a70292c1e00905bcd3412f498df0d5cca6679c33 "Cleanup EPP and Latency Predictor \(#2967\)

* remove epp and latency predictor

Signed-off-by: Nicole Xin <nxin@google.com>

* fix conformance test flakiness

Signed-off-by: Nicole Xin <nxin@google.com>

* fix ci issues with trivy scan and remove sidecar from dockerfile

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")[#2967](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2967)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a70292c1e00905bcd3412f498df0d5cca6679c33 "Cleanup EPP and Latency Predictor \(#2967\)

* remove epp and latency predictor

Signed-off-by: Nicole Xin <nxin@google.com>

* fix conformance test flakiness

Signed-off-by: Nicole Xin <nxin@google.com>

* fix ci issues with trivy scan and remove sidecar from dockerfile

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 16, 2026  |  
|   |   |   | Aug 28, 2024  |  
|   |   | [Remove inference objective, model rewrite and endpoint picker config …](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/88fd4792d1629f807589f0a1741fb4426f1d26d1 "Remove inference objective, model rewrite and endpoint picker config APIs \(#2973\)

* remove inference objective and model rewrite APIs

Signed-off-by: Nicole Xin <nxin@google.com>

* remove EndpointPickerConfig API

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 17, 2026  |  
|   |   |  [chore(deps): bump google.golang.org/grpc from 1.83.1 to 1.83.2 (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a4b0e4386dc924fa009ef0d80a98960dc551c9e7 "chore\(deps\): bump google.golang.org/grpc from 1.83.1 to 1.83.2 \(#3029\)

Bumps \[google.golang.org/grpc\]\(https://github.com/grpc/grpc-go\) from 1.83.1 to 1.83.2.
- \[Release notes\]\(https://github.com/grpc/grpc-go/releases\)
- \[Commits\]\(https://github.com/grpc/grpc-go/compare/v1.83.1...v1.83.2\)

---
updated-dependencies:
- dependency-name: google.golang.org/grpc
  dependency-version: 1.83.2
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#3029](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3029)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a4b0e4386dc924fa009ef0d80a98960dc551c9e7 "chore\(deps\): bump google.golang.org/grpc from 1.83.1 to 1.83.2 \(#3029\)

Bumps \[google.golang.org/grpc\]\(https://github.com/grpc/grpc-go\) from 1.83.1 to 1.83.2.
- \[Release notes\]\(https://github.com/grpc/grpc-go/releases\)
- \[Commits\]\(https://github.com/grpc/grpc-go/compare/v1.83.1...v1.83.2\)

---
updated-dependencies:
- dependency-name: google.golang.org/grpc
  dependency-version: 1.83.2
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Sep 1, 2026  |  
|   |   |  [chore(deps): bump google.golang.org/grpc from 1.83.1 to 1.83.2 (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a4b0e4386dc924fa009ef0d80a98960dc551c9e7 "chore\(deps\): bump google.golang.org/grpc from 1.83.1 to 1.83.2 \(#3029\)

Bumps \[google.golang.org/grpc\]\(https://github.com/grpc/grpc-go\) from 1.83.1 to 1.83.2.
- \[Release notes\]\(https://github.com/grpc/grpc-go/releases\)
- \[Commits\]\(https://github.com/grpc/grpc-go/compare/v1.83.1...v1.83.2\)

---
updated-dependencies:
- dependency-name: google.golang.org/grpc
  dependency-version: 1.83.2
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#3029](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3029)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a4b0e4386dc924fa009ef0d80a98960dc551c9e7 "chore\(deps\): bump google.golang.org/grpc from 1.83.1 to 1.83.2 \(#3029\)

Bumps \[google.golang.org/grpc\]\(https://github.com/grpc/grpc-go\) from 1.83.1 to 1.83.2.
- \[Release notes\]\(https://github.com/grpc/grpc-go/releases\)
- \[Commits\]\(https://github.com/grpc/grpc-go/compare/v1.83.1...v1.83.2\)

---
updated-dependencies:
- dependency-name: google.golang.org/grpc
  dependency-version: 1.83.2
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Sep 1, 2026  |  
|   |   |  [deps: update to Gateway API v1.6-rc.1 (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/3cc2b886d1e7e73b22dd3b745c5b3eba1be9c384 "deps: update to Gateway API v1.6-rc.1 \(#2964\)

* deps: update to Gateway API v1.6-rc.1

This is required for importers to import both of these modules together.

* Update generated clients for Go 1.26")[#2964](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2964)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/3cc2b886d1e7e73b22dd3b745c5b3eba1be9c384 "deps: update to Gateway API v1.6-rc.1 \(#2964\)

* deps: update to Gateway API v1.6-rc.1

This is required for importers to import both of these modules together.

* Update generated clients for Go 1.26")  | Jun 29, 2026  |  
|   |   | [fix(lwepp): respect buildx TARGETARCH instead of hardcoding GOARCH=am…](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/7b02621a11fb4fd6870c285ee50318c0941003a9 "fix\(lwepp\): respect buildx TARGETARCH instead of hardcoding GOARCH=amd64 \(#3032\)

* fix\(lwepp\): respect buildx TARGETARCH instead of hardcoding GOARCH=amd64

The lwepp.Dockerfile builder stage hardcodes ENV GOARCH=amd64, so the
compiled binary is always amd64 regardless of the --platform flag
passed to docker buildx. This breaks native arm64 builds: the final
image manifest correctly reports the requested platform \(the base
image resolves per-platform\), but the binary inside is still amd64,
producing 'exec format error' at container startup on arm64 hosts.

Wire GOARCH to the buildx-provided TARGETARCH automatic build arg so
a build for --platform=linux/arm64 actually produces an arm64 binary.
This matches the pattern already used correctly in llm-d/llm-d-router's
Dockerfiles \(the sibling repo hosting the full EPP\).

Verified locally: native arm64 build \(NVIDIA DGX Spark, Ubuntu 24.04,
aarch64\) via 'make lwepp-image-local-load PLATFORMS=linux/arm64'.
Before: exec format error at container startup. After: extracted the
compiled binary from the image and confirmed via 'file':
'ELF 64-bit LSB executable, ARM aarch64' \(previously x86-64\).

* Update lwepp.Dockerfile

As Per review feedback, Added amd64 default to TARGETARCH for plain docker build compatibility

Co-authored-by: Etai Lev Ran <elevran@gmail.com>

---------

Co-authored-by: Tamil Dhanabalan <tamil-deds@users.noreply.github.com>
Co-authored-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 7, 2026  |  
|   |   |  [Cleanup Documentation and Miscellaneous Tools / Manifests (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")[#2978](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2978)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ebbb148c87d7d4b91567a4a741e54bd96a3010af "Cleanup Documentation and Miscellaneous Tools / Manifests \(#2978\)

* remove latency predictor docs

Signed-off-by: Nicole Xin <nxin@google.com>

* cleanup guides and unused tools

Signed-off-by: Nicole Xin <nxin@google.com>

---------

Signed-off-by: Nicole Xin <nxin@google.com>")  | Jun 30, 2026  |  
|   |   |   | Dec 19, 2024  |  
| View all files |  
## Repository files navigation
# Gateway API Inference Extension
Gateway API Inference Extension optimizes self-hosting Generative Models on Kubernetes. This is achieved by leveraging Envoy's [External Processing](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) (ext-proc) to extend any gateway that supports both ext-proc and [Gateway API](https://github.com/kubernetes-sigs/gateway-api) into an **[inference gateway](https://github.com/kubernetes-sigs/gateway-api-inference-extension#concepts-and-definitions)**.
Inference Gateway has partnered with vLLM to accelerate LLM serving optimizations with [llm-d](https://llm-d.ai/blog/llm-d-announce)!
Important
The Endpoint Picker (EPP), InferenceObjective and InferenceModelRewrite APIs, and Body Based Router (BBR) packages have moved to new repositories:
  * EPP and associated APIs: [llm-d/llm-d-router](https://github.com/llm-d/llm-d-router)
  * BBR: [llm-d/llm-d-inference-payload-processor](https://github.com/llm-d/llm-d-inference-payload-processor)


No new code will be accepted to these packages in this repository, and they will be archived soon. This move was proposed and discussed in [issue #2430](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430).
This repository will continue to host the **lightweight EPP (LWEPP)** and the **InferencePool API** , and will remain the primary location for the development and maintenance of **conformance tests**.
## Concepts and Definitions
The following specific terms to this project:
  * **Inference Gateway (IGW)** : A proxy/load-balancer which has been coupled with an `Endpoint Picker`. It provides optimized routing and load balancing for serving Kubernetes self-hosted generative Artificial Intelligence (AI) workloads. It simplifies the deployment, management, and observability of AI inference workloads.
  * **Inference Scheduler** : An extendable component that makes decisions about which endpoint is optimal (best cost / best performance) for an inference request based on `Metrics and Capabilities` from [Model Serving](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/003-model-server-protocol/README.md).
  * **Metrics and Capabilities** : Data provided by model serving platforms about performance, availability and capabilities to optimize routing. Includes things like [Prefix Cache](https://docs.vllm.ai/en/stable/design/v1/prefix_caching.html) status or [LoRA Adapters](https://docs.vllm.ai/en/stable/features/lora.html) availability.
  * **Endpoint Picker (EPP)** : A data-plane component that communicates via the [Envoy external processing protocol](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter), and acts as the `Router`. It intercepts incoming inference requests and routes each request to the optimal model server replica. This repository provides a reference **Lightweight Endpoint Picker (lwepp)** for conformance test purposes.


The following are key industry terms that are important to understand for this project:
  * **Model** : A generative AI model that has learned patterns from data and is used for inference. Models vary in size and architecture, from smaller domain-specific models to massive multi-billion parameter neural networks that are optimized for diverse language tasks.
  * **Inference** : The process of running a generative AI model, such as a large language model, diffusion model etc, to generate text, embeddings, or other outputs from input data.
  * **Model server** : A service (in our case, containerized) responsible for receiving inference requests and returning predictions from a model.
  * **Accelerator** : specialized hardware, such as Graphics Processing Units (GPUs) that can be attached to Kubernetes nodes to speed up computations, particularly for training and inference tasks.


For deeper insights and more advanced concepts, refer to our [proposals](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals).
## Technical Overview
This extension upgrades an [ext-proc](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) capable proxy or gateway - such as Envoy Gateway, kgateway, or the GKE Gateway - to become an **[inference gateway](https://github.com/kubernetes-sigs/gateway-api-inference-extension#concepts-and-definitions)** - supporting inference platform teams self-hosting Generative Models (with a current focus on large language models) on Kubernetes. This integration makes it easy to expose and control access to your local [OpenAI-compatible chat completion endpoints](https://platform.openai.com/docs/api-reference/chat) to other workloads on or off cluster, or to integrate your self-hosted models alongside model-as-a-service providers in a higher level **AI Gateway** like LiteLLM, Solo AI Gateway, or Apigee.
The Inference Gateway:
  * Improves the tail latency and throughput of LLM completion requests against Kubernetes-hosted model servers using an extensible request scheduling algorithm that is kv-cache and request cost aware, avoiding evictions or queueing as load increases
  * Provides [Kubernetes-native declarative APIs](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/) to route client model names to use-case specific LoRA adapters and control incremental rollout of new adapter versions, A/B traffic splitting, and safe blue-green base model and model server upgrades
  * Adds end to end observability around service objective attainment
  * Ensures operational guardrails between different client model names, allowing a platform team to safely serve many different GenAI workloads on the same pool of shared foundation model servers for higher utilization and fewer required accelerators


### Model Server Integration
IGW’s pluggable architecture was leveraged to enable the [llm-d Router](https://github.com/llm-d/llm-d-router).
Llm-d customizes vLLM & IGW to create a disaggregated serving solution. We've worked closely with this team to enable this integration. IGW will continue to work closely with llm-d to generalize the disaggregated serving plugin(s), & set a standard for disaggregated serving to be used across any [protocol-adherent](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/003-model-server-protocol) model server.
IGW has enhanced support for vLLM via llm-d, and broad support for any model servers implementing the protocol. More details can be found in [model server integration](https://gateway-api-inference-extension.sigs.k8s.io/implementations/model-servers/).
## Status
This project is GA'd! The latest release can be found [here](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/latest). Please file any bugs or feature requests you have. We are always happy to welcome new collaborators and users.
## Getting Started
Follow our [Getting Started Guide](https://gateway-api-inference-extension.sigs.k8s.io/guides/) to get the inference-extension up and running on your cluster!
See [our website](https://gateway-api-inference-extension.sigs.k8s.io/) for detailed API documentation on leveraging our Kubernetes-native declarative APIs
## Roadmap
As Inference Gateway builds towards a GA release. We will continue to expand our capabilities, namely:
  1. Prefix-cache aware load balancing with interfaces for remote caches
  2. Recommended LoRA adapter pipeline for automated rollout
  3. Fairness and priority between workloads within the same criticality band
  4. HPA support for autoscaling on aggregate metrics derived from the load balancer
  5. Support for large multi-modal inputs and outputs
  6. Support for other GenAI model types (diffusion and other non-completion protocols)
  7. Heterogeneous accelerators - serve workloads on multiple types of accelerator using latency and request cost-aware load balancing
  8. Disaggregated serving support with independently scaling pools


## Contributing
Community meetings have moved to the llm-d Router community meeting. See the [llm-d Router contributing section](https://github.com/llm-d/llm-d-router/blob/main/README.md#contributing) for current meeting details.
We currently utilize the [#gateway-api-inference-extension](https://kubernetes.slack.com/?redir=%2Fmessages%2Fgateway-api-inference-extension) channel in Kubernetes Slack workspace for communications.
Contributions are readily welcomed!
### Code of conduct
Participation in the Kubernetes community is governed by the [Kubernetes Code of Conduct](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/code-of-conduct.md).
## About
Gateway API Inference Extension
[gateway-api-inference-extension.sigs.k8s.io/](https://gateway-api-inference-extension.sigs.k8s.io/)
### Topics
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**766** stars
### Watchers
**23** watching
### Forks
[ forks](https://github.com/kubernetes-sigs/gateway-api-inference-extension/forks)
## 
[ v1.6.0Latest Aug 17, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.6.0)
## 
## Languages
  * [Go88.8%](https://github.com/kubernetes-sigs/gateway-api-inference-extension/search?l=go)
  * [Shell4.5%](https://github.com/kubernetes-sigs/gateway-api-inference-extension/search?l=shell)
  * [Makefile3.2%](https://github.com/kubernetes-sigs/gateway-api-inference-extension/search?l=makefile)
  * [Python1.9%](https://github.com/kubernetes-sigs/gateway-api-inference-extension/search?l=python)
  * [HTML1%](https://github.com/kubernetes-sigs/gateway-api-inference-extension/search?l=html)
  * [Dockerfile0.4%](https://github.com/kubernetes-sigs/gateway-api-inference-extension/search?l=dockerfile)
  * [CSS0.2%](https://github.com/kubernetes-sigs/gateway-api-inference-extension/search?l=css)


Generated from [kubernetes/kubernetes-template-project](https://github.com/kubernetes/kubernetes-template-project)
You can’t perform that action at this time. 
