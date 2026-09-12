---
title: gateway-api-inference-extension/site-src/guides at main · kubernetes-sigs/gateway-api-inference-extension
  · GitHub
id: gateway-api-inference-extensionsite-srcguides-at-main-kubernetes-sigsgateway-api
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- source-hub
- docs-cleanup
created: '2026-09-11T19:44:32.370251Z'
updated: '2026-09-12T03:37:37.575660Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/site-src/guides
source_domain: github.com
fetched_at: '2026-09-11T19:44:32.349636Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub directory listing for site-src/guides at HEAD (2026-09-11). Directory
  contents at HEAD are only conformance-tests.md and implementers.md (confirmed via
  GitHub contents API) — standalone.md, flags.md, and most other guide pages were
  deleted in commit ebbb148c (''Cleanup Documentation and Miscellaneous Tools / Manifests'',
  PR #2978, 2026-06-30). This is a documented absence: the standalone-mode user guide
  and the EPP command-line-flags reference guide (site-src/guides/epp-configuration/flags.md,
  which would have documented the ''Without Inference APIs Support'' flag-only standalone
  config) no longer exist on the docs site as of this run''s fetch date, consistent
  with the guides/standalone/ URL returning empty/junk content when fetched directly.
  The last surviving version of standalone.md (commit 91ad0da4, immediately pre-deletion)
  was fetched separately as a primary-source substitute.'
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/site-src/guides#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/site-src/guides) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/site-src/guides) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/site-src/guides) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/site-src/guides).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# guides
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[docs: fix InferencePool YAML examples in api-types and implementers g…](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ffe3e7f3b790061495546bb1362cad3e7be3db7e)
Open commit detailssuccess
Jul 16, 2026
[ffe3e7f](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ffe3e7f3b790061495546bb1362cad3e7be3db7e) · Jul 16, 2026
## History
[History](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commits/main/site-src/guides)
Open commit details
History
/
# guides
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
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
|   |   | [docs: fix InferencePool YAML examples in api-types and implementers g…](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/ffe3e7f3b790061495546bb1362cad3e7be3db7e "docs: fix InferencePool YAML examples in api-types and implementers guide \(#2992\)

* docs: fix InferencePool YAML examples in api-types and implementers guide

Update stale examples to match the v1 API: selector.matchLabels,
endpointPickerRef.port.number, and targetPorts field naming.

* docs: restore targetPort 8000 in InferencePool examples

Keep the original model server port from the published docs.")  | Jul 16, 2026  |  
| View all files |  
You can’t perform that action at this time. 
