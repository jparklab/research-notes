---
title: llm-d-router/pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go at
  main · llm-d/llm-d-router · GitHub
id: llm-d-routerpkgeppframeworkpluginsschedulingfilterbylabelrolesgo-at-main-llm-dll
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- bylabel
- go-source
created: '2026-09-11T20:48:32.042678Z'
updated: '2026-09-15T04:55:23.833680Z'
source: https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go
source_domain: github.com
fetched_at: '2026-09-11T20:48:32.018399Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE: full Go source of pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go
  on llm-d/llm-d-router main branch (fetched Sep 11 2026, unpinned to a specific commit
  -- treat as current-as-of-fetch). Defines the exact llm-d.ai/role label constant
  (RoleLabel = "llm-d.ai/role") and the complete enumeration of six role values used
  for P/D and E/P/D disaggregation filtering: RoleDecode="decode", RolePrefill="prefill",
  RoleEncode="encode", RolePrefillDecode="prefill-decode", RoleEncodePrefill="encode-prefill",
  RoleEncodePrefillDecode="encode-prefill-decode". Shows the three role-filter factory
  functions and their accepted role sets: NewDecodeRole() accepts pods labeled decode,
  prefill-decode, or encode-prefill-decode; NewPrefillRole() accepts prefill, encode-prefill,
  or prefill-decode; NewEncodeRole() accepts encode, encode-prefill, or encode-prefill-decode
  (documented pipeline order: Encode -> Prefill -> Decode). Each filter is registered
  under a distinct plugin type string: decode-filter, prefill-filter, encode-filter
  (constants DecodeRoleType/PrefillRoleType/EncodeRoleType). This confirms, at the
  source level, the exact string values a custom non-Kubernetes backend must attach
  to its FileDiscovery YAML entries -- e.g. labels: {llm-d.ai/role: prefill-decode}
  -- to be correctly picked up by the router''s built-in P/D role filters without
  any code changes to the filter itself, since the filter reads role purely from the
  generic label map on EndpointMetadata and is not Kubernetes-specific.'
utility_score: 15.0
---

*Suggested by [[no-kubernetes-no-problem-llm-d-now-runs-anywhere-llm-d]] — full set of llm-d.ai/role label values referenced in the blog*

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# roles.go
Copy path
More file actions
More file actions
## Latest commit
[epp: remove code deprecated since v0.7.0 (](https://github.com/llm-d/llm-d-router/commit/61350aa3f6ff7d05e69b4e50fa52b1a25417a043)[#2509](https://github.com/llm-d/llm-d-router/pull/2509)[)](https://github.com/llm-d/llm-d-router/commit/61350aa3f6ff7d05e69b4e50fa52b1a25417a043)
Open commit detailssuccess
Aug 25, 2026
[61350aa](https://github.com/llm-d/llm-d-router/commit/61350aa3f6ff7d05e69b4e50fa52b1a25417a043) · Aug 25, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go)
Open commit details
History
65 lines (53 loc) · 2.54 KB
Code owner: @llm-d/router-maintainers
/
# roles.go
Copy path
## File metadata and controls
  * 

65 lines (53 loc) · 2.54 KB
Code owner: @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Open symbols panel
Edit and raw actions
package bylabel import ( "encoding/json" "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin" ) const ( // RoleLabel name RoleLabel = "llm-d.ai/role" // RoleDecode set for designated decode workers RoleDecode = "decode" // RolePrefill set for designated prefill workers RolePrefill = "prefill" // RoleEncode set for designated encode workers (first stage in pipeline) RoleEncode = "encode" // RolePrefillDecode set for workers that can handle prefill and decode stages RolePrefillDecode = "prefill-decode" // RoleEncodePrefill set for workers that can handle encode+prefill (EP/D or P/D disaggregation) RoleEncodePrefill = "encode-prefill" // RoleEncodePrefillDecode set for workers that can handle encode+prefill+decode RoleEncodePrefillDecode = "encode-prefill-decode" // DecodeRoleType is the type of the DecodeFilter DecodeRoleType = "decode-filter" // PrefillRoleType is the type of the PrefillFilter PrefillRoleType = "prefill-filter" // EncodeRoleType is the type of the EncodeFilter EncodeRoleType = "encode-filter" ) // DecodeRoleFactory defines the factory function for the Decode filter. func DecodeRoleFactory(name string, _ *json.Decoder, _ plugin.Handle) (plugin.Plugin, error) { return NewDecodeRole().WithName(name), nil } // NewDecodeRole creates and returns an instance of the Filter configured for decode role. func NewDecodeRole() *ByLabel { return NewByLabel(DecodeRoleType, RoleLabel, true, RoleDecode, RolePrefillDecode, RoleEncodePrefillDecode) } // PrefillRoleFactory defines the factory function for the Prefill filter. func PrefillRoleFactory(name string, _ *json.Decoder, _ plugin.Handle) (plugin.Plugin, error) { return NewPrefillRole().WithName(name), nil } // NewPrefillRole creates and returns an instance of the Filter configured for prefill role. func NewPrefillRole() *ByLabel { return NewByLabel(PrefillRoleType, RoleLabel, false, RolePrefill, RoleEncodePrefill, RolePrefillDecode, RoleEncodePrefillDecode) } // EncodeRoleFactory defines the factory function for the Encode filter. func EncodeRoleFactory(name string, _ *json.Decoder, _ plugin.Handle) (plugin.Plugin, error) { return NewEncodeRole().WithName(name), nil } // NewEncodeRole creates and returns an instance of the Filter configured for encode role. // Encode is the first stage in the pipeline: Encode -> Prefill -> Decode. // Accepts pods with roles: encode, encode-prefill, or encode-prefill-decode. func NewEncodeRole() *ByLabel { return NewByLabel(EncodeRoleType, RoleLabel, false, RoleEncode, RoleEncodePrefill, RoleEncodePrefillDecode) }
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
package bylabel
import (
"encoding/json"
"github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin"
const (
// RoleLabel name
RoleLabel = "llm-d.ai/role"
// RoleDecode set for designated decode workers
RoleDecode = "decode"
// RolePrefill set for designated prefill workers
RolePrefill = "prefill"
// RoleEncode set for designated encode workers (first stage in pipeline)
RoleEncode = "encode"
// RolePrefillDecode set for workers that can handle prefill and decode stages
RolePrefillDecode = "prefill-decode"
// RoleEncodePrefill set for workers that can handle encode+prefill (EP/D or P/D disaggregation)
RoleEncodePrefill = "encode-prefill"
// RoleEncodePrefillDecode set for workers that can handle encode+prefill+decode
RoleEncodePrefillDecode = "encode-prefill-decode"
// DecodeRoleType is the type of the DecodeFilter
DecodeRoleType = "decode-filter"
// PrefillRoleType is the type of the PrefillFilter
PrefillRoleType = "prefill-filter"
// EncodeRoleType is the type of the EncodeFilter
EncodeRoleType = "encode-filter"
// DecodeRoleFactory defines the factory function for the Decode filter.
func DecodeRoleFactory(name string, _ *json.Decoder, _ plugin.Handle) (plugin.Plugin, error) {
return NewDecodeRole().WithName(name), nil
// NewDecodeRole creates and returns an instance of the Filter configured for decode role.
func NewDecodeRole() *ByLabel {
return NewByLabel(DecodeRoleType, RoleLabel, true, RoleDecode, RolePrefillDecode, RoleEncodePrefillDecode)
// PrefillRoleFactory defines the factory function for the Prefill filter.
func PrefillRoleFactory(name string, _ *json.Decoder, _ plugin.Handle) (plugin.Plugin, error) {
return NewPrefillRole().WithName(name), nil
// NewPrefillRole creates and returns an instance of the Filter configured for prefill role.
func NewPrefillRole() *ByLabel {
return NewByLabel(PrefillRoleType, RoleLabel, false, RolePrefill, RoleEncodePrefill, RolePrefillDecode, RoleEncodePrefillDecode)
// EncodeRoleFactory defines the factory function for the Encode filter.
func EncodeRoleFactory(name string, _ *json.Decoder, _ plugin.Handle) (plugin.Plugin, error) {
return NewEncodeRole().WithName(name), nil
// NewEncodeRole creates and returns an instance of the Filter configured for encode role.
// Encode is the first stage in the pipeline: Encode -> Prefill -> Decode.
// Accepts pods with roles: encode, encode-prefill, or encode-prefill-decode.
func NewEncodeRole() *ByLabel {
return NewByLabel(EncodeRoleType, RoleLabel, false, RoleEncode, RoleEncodePrefill, RoleEncodePrefillDecode)
You can’t perform that action at this time. 
While the code is focused, press Alt+F1 for a menu of operations.
