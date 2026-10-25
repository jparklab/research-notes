---
title: llm-d-router/pkg/epp/framework/interface/datalayer/discovery.go at main · llm-d/llm-d-router
  · GitHub
id: llm-d-routerpkgeppframeworkinterfacedatalayerdiscoverygo-at-main-llm-dllm-d-rout
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- endpoint-discovery
- go-source
created: '2026-09-11T20:48:26.816550Z'
updated: '2026-09-15T04:55:23.831711Z'
source: https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/datalayer/discovery.go
source_domain: github.com
fetched_at: '2026-09-11T20:48:26.794025Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE: full Go source of pkg/epp/framework/interface/datalayer/discovery.go
  on llm-d/llm-d-router main branch (Copyright 2025 The Kubernetes Authors, Apache
  2.0; fetched Sep 11 2026, exact commit not pinned since URL targets main -- treat
  API shape as current-as-of-fetch, subject to change). Defines the exact EndpointDiscovery
  plugin contract referenced by the no-Kubernetes blog, with fuller doc comments than
  the blog quoted: type EndpointDiscovery interface { fwkplugin.Plugin; Start(ctx
  context.Context, notifier DiscoveryNotifier) error; Ready() <-chan struct{} }. Comment
  states implementations are "registered in the plugin registry and selected via EndpointPickerConfig.discovery.pluginRef"
  (note: the blog post phrased the equivalent config key as dataLayer.discovery.pluginRef
  -- a possible naming discrepancy between the source comment and blog prose that
  should be verified against the actual EndpointPickerConfig schema/example before
  being treated as authoritative). Documents explicit implementation requirements:
  Start SHOULD enumerate all currently known endpoints via notifier.Upsert before
  entering its watch loop (implementations with a guaranteed-no-missed-events watch,
  e.g. Kubernetes list+watch, MAY fold initial enumeration into the watch sequence
  instead); Ready()s channel closes at most once per Start call, only after a successful
  initial sync, and "zero endpoints is a valid outcome." Defines DiscoveryNotifier
  as NOT goroutine-safe -- "All calls must be made sequentially from a single goroutine"
  -- with a strict ordering contract: "the datastore processes Upsert and Delete calls
  in the order they are received... an Upsert followed by a Delete for the same endpoint
  must arrive in that order, or the endpoint will be incorrectly left in the datastore."
  Also defines a helper DiscoveryEndpointStore narrow interface (EndpointUpsert(ctx,
  *EndpointMetadata), EndpointDelete(id types.NamespacedName)) and NewDiscoveryNotifier(store
  DiscoveryEndpointStore) DiscoveryNotifier constructor that adapts any conforming
  store into a DiscoveryNotifier -- this is the concrete extension point a custom
  backend integration would implement against. EndpointDiscovery embeds fwkplugin.Plugin
  (from pkg/epp/framework/interface/plugin), meaning a custom discovery plugin must
  also satisfy whatever base Plugin interface that package defines (not yet independently
  confirmed in this note).'
utility_score: 18.0
---

*Suggested by [[no-kubernetes-no-problem-llm-d-now-runs-anywhere-llm-d]] — the actual Go source defining the EndpointDiscovery interface, quoted in the blog*

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/datalayer/discovery.go#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/datalayer/discovery.go) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/datalayer/discovery.go) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/datalayer/discovery.go) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/datalayer/discovery.go).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# discovery.go
Copy path
More file actions
More file actions
## Latest commit
[FileDiscovery plugin (4/4) - wiring into the runner (](https://github.com/llm-d/llm-d-router/commit/bda7c04a9f8b4de93c71ed01a0990f0f7b5a3695)[#1218](https://github.com/llm-d/llm-d-router/pull/1218)[)](https://github.com/llm-d/llm-d-router/commit/bda7c04a9f8b4de93c71ed01a0990f0f7b5a3695)
Open commit detailsfailure
May 25, 2026
[bda7c04](https://github.com/llm-d/llm-d-router/commit/bda7c04a9f8b4de93c71ed01a0990f0f7b5a3695) · May 25, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/interface/datalayer/discovery.go)
Open commit details
History
100 lines (84 loc) · 4.07 KB
Code owner: @llm-d/router-maintainers
/
# discovery.go
Copy path
## File metadata and controls
  * 

100 lines (84 loc) · 4.07 KB
Code owner: @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Open symbols panel
Edit and raw actions
/* Copyright 2025 The Kubernetes Authors. Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0 Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License. */ package datalayer import ( "context" "k8s.io/apimachinery/pkg/types" fwkplugin "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin" ) // DiscoveryEndpointStore is the narrow interface required by NewDiscoveryNotifier. // Any store that implements EndpointUpsert and EndpointDelete satisfies it. type DiscoveryEndpointStore interface { EndpointUpsert(ctx context.Context, meta *EndpointMetadata) EndpointDelete(id types.NamespacedName) } // NewDiscoveryNotifier wraps a DiscoveryEndpointStore as a DiscoveryNotifier. func NewDiscoveryNotifier(store DiscoveryEndpointStore) DiscoveryNotifier { return &discoveryNotifier{store: store} } type discoveryNotifier struct { store DiscoveryEndpointStore } func (n *discoveryNotifier) Upsert(endpoint *EndpointMetadata) { n.store.EndpointUpsert(context.Background(), endpoint) } func (n *discoveryNotifier) Delete(id types.NamespacedName) { n.store.EndpointDelete(id) } // EndpointDiscovery discovers inference endpoints and drives their lifecycle in the datastore. // Implementations are registered in the plugin registry and selected via // EndpointPickerConfig.discovery.pluginRef. type EndpointDiscovery interface { fwkplugin.Plugin // Start begins discovery and blocks in the caller's goroutine until ctx is // cancelled or a fatal error occurs. It is the caller's responsibility to // invoke Start in a dedicated goroutine. // // Implementations SHOULD enumerate all currently known endpoints via // notifier.Upsert before entering the watch loop, to avoid serving an empty // datastore at startup. Implementations that guarantee no missed events // through their watch mechanism (e.g. a Kubernetes list+watch) may fold the // initial enumeration into the watch sequence instead. Start(ctx context.Context, notifier DiscoveryNotifier) error // Ready returns a channel that is closed once after the plugin has completed // its initial reconciliation with the underlying source, regardless of how // many endpoints that produced. Callers use it to gate request-serving // components until the datastore has been populated for the first time. // // Contract: // - The channel is closed at most once per Start invocation. // - It is closed only after a successful initial sync. If Start returns // an error before that point, the channel remains open and callers // waiting on it should also observe ctx cancellation. // - "Initial sync" means whatever the plugin considers a complete first // pass (e.g. file load for FileDiscovery; first list+watch reconcile // for a Kubernetes plugin). Zero endpoints is a valid outcome. Ready() <-chan struct{} } // DiscoveryNotifier is the callback through which EndpointDiscovery communicates // endpoint state to the datastore. // // DiscoveryNotifier is NOT goroutine-safe. All calls must be made sequentially // from a single goroutine. This is the source of the ordering contract below. // // Ordering contract: the datastore processes Upsert and Delete calls in the order // they are received. Plugin implementations MUST preserve event order -- do not // buffer or dispatch calls concurrently in a way that could reorder them. For // example, an Upsert followed by a Delete for the same endpoint must arrive in // that order, or the endpoint will be incorrectly left in the datastore. type DiscoveryNotifier interface { // Upsert adds or updates an endpoint in the datastore. Upsert(endpoint *EndpointMetadata) // Delete removes an endpoint from the datastore by its namespaced name. Delete(id types.NamespacedName) }
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
Copyright 2025 The Kubernetes Authors.
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
package datalayer
import (
"context"
"k8s.io/apimachinery/pkg/types"
fwkplugin "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin"
// DiscoveryEndpointStore is the narrow interface required by NewDiscoveryNotifier.
// Any store that implements EndpointUpsert and EndpointDelete satisfies it.
type DiscoveryEndpointStore interface {
EndpointUpsert(ctx context.Context, meta *EndpointMetadata)
EndpointDelete(id types.NamespacedName)
// NewDiscoveryNotifier wraps a DiscoveryEndpointStore as a DiscoveryNotifier.
func NewDiscoveryNotifier(store DiscoveryEndpointStore) DiscoveryNotifier {
return discoveryNotifier{store: store}
type discoveryNotifier struct {
store DiscoveryEndpointStore
func (n *discoveryNotifier) Upsert(endpoint *EndpointMetadata) {
n.store.EndpointUpsert(context.Background(), endpoint)
func (n *discoveryNotifier) Delete(id types.NamespacedName) {
n.store.EndpointDelete(id)
// EndpointDiscovery discovers inference endpoints and drives their lifecycle in the datastore.
// Implementations are registered in the plugin registry and selected via
// EndpointPickerConfig.discovery.pluginRef.
type EndpointDiscovery interface {
fwkplugin.Plugin
// Start begins discovery and blocks in the caller's goroutine until ctx is
// cancelled or a fatal error occurs. It is the caller's responsibility to
// invoke Start in a dedicated goroutine.
// Implementations SHOULD enumerate all currently known endpoints via
// notifier.Upsert before entering the watch loop, to avoid serving an empty
// datastore at startup. Implementations that guarantee no missed events
// through their watch mechanism (e.g. a Kubernetes list+watch) may fold the
// initial enumeration into the watch sequence instead.
Start(ctx context.Context, notifier DiscoveryNotifier) error
// Ready returns a channel that is closed once after the plugin has completed
// its initial reconciliation with the underlying source, regardless of how
// many endpoints that produced. Callers use it to gate request-serving
// components until the datastore has been populated for the first time.
// Contract:
// - The channel is closed at most once per Start invocation.
// - It is closed only after a successful initial sync. If Start returns
// an error before that point, the channel remains open and callers
// waiting on it should also observe ctx cancellation.
// - "Initial sync" means whatever the plugin considers a complete first
// pass (e.g. file load for FileDiscovery; first list+watch reconcile
// for a Kubernetes plugin). Zero endpoints is a valid outcome.
Ready() <-chan struct{}
// DiscoveryNotifier is the callback through which EndpointDiscovery communicates
// endpoint state to the datastore.
// DiscoveryNotifier is NOT goroutine-safe. All calls must be made sequentially
// from a single goroutine. This is the source of the ordering contract below.
// Ordering contract: the datastore processes Upsert and Delete calls in the order
// they are received. Plugin implementations MUST preserve event order -- do not
// buffer or dispatch calls concurrently in a way that could reorder them. For
// example, an Upsert followed by a Delete for the same endpoint must arrive in
// that order, or the endpoint will be incorrectly left in the datastore.
type DiscoveryNotifier interface {
// Upsert adds or updates an endpoint in the datastore.
Upsert(endpoint *EndpointMetadata)
// Delete removes an endpoint from the datastore by its namespaced name.
Delete(id types.NamespacedName)
You can’t perform that action at this time. 
While the code is focused, press Alt+F1 for a menu of operations.
