---
title: llm-d-router/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go
  at main · llm-d/llm-d-router · GitHub
id: llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcacheex
tags:
- llmd-router-nonk8s-backends-54b774
- locus-kv-event-path-off-k8s
- precise-prefix-cache-producer
- endpoint-extractor
- go-source
- zmq-subscriber-manager
- non-k8s-discovery
created: '2026-09-11T21:24:58.209178Z'
updated: '2026-09-12T03:37:37.775232Z'
source: https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go
source_domain: github.com
fetched_at: '2026-09-11T21:24:58.179401Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Primary source, most load-bearing file for this investigation: extractor.go
  (96 lines) for llm-d-router''s precise-prefix-cache-producer. Contains the assertion
  ''var _ fwkdl.EndpointExtractor = &Producer{}'' — Producer implements the ROUTER''s
  own fwkdl.EndpointExtractor interface (package github.com/llm-d/llm-d-router/pkg/epp/framework/interface/datalayer),
  confirming this is the router-level data-layer extractor pattern, not a direct call
  into the llm-d-kv-cache library''s Kubernetes PodDiscoveryConfig reconciler. Extract(ctx,
  event fwkdl.EndpointEvent) is called for endpoint lifecycle events from the endpoint-notification-source:
  on fwkdl.EventAddOrUpdate it calls p.ensureSubscriber(meta), on fwkdl.EventDelete
  it calls p.subscribersManager.RemoveSubscriber(ctx, endpointKey) and clears index
  entries keyed ''address:port''. Two important findings: (1) the whole Extract()
  short-circuits to a no-op unless BOTH p.kvEventsConfig.DiscoverPods is true AND
  p.kvEventsConfig.PodDiscoveryConfig != nil — PodDiscoveryConfig here supplies only
  config values (SocketPort, EffectiveReplayPort) for constructing the ZMQ dial address
  (SocketPort + meta.GetRankIndex() for per-DP-rank port offsetting), it is NOT itself
  the Kubernetes reconciler; (2) the subscription identity key is endpointKey := meta.ID.String()
  (EndpointMetadata.ID, not a literal address:port string), while the ZMQ dial target
  is separately built from meta.Address + computed port. Since Extract is driven purely
  by fwkdl.EndpointEvent/EndpointMetadata supplied by whatever EndpointSource feeds
  endpoint-notification-source, this plugin is backend-agnostic as long as a non-Kubernetes
  EndpointDiscovery plugin (e.g. file-discovery) populates EndpointMetadata.ID/Address/Port/RankIndex
  and fires EventAddOrUpdate/EventDelete through that same data-layer path.'
---

*Suggested by [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcachepr]] — producer.go doc comment: per-pod ZMQ subscriber lifecycle lives in extractor.go*

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# extractor.go
Copy path
More file actions
More file actions
## Latest commit
[fix: IPv6 address in remaining endpoints (](https://github.com/llm-d/llm-d-router/commit/31bc02c2f1dc8c118ea89f535a928672e81d0613)[#2658](https://github.com/llm-d/llm-d-router/pull/2658)[)](https://github.com/llm-d/llm-d-router/commit/31bc02c2f1dc8c118ea89f535a928672e81d0613)
Open commit detailsfailure
Sep 2, 2026
[31bc02c](https://github.com/llm-d/llm-d-router/commit/31bc02c2f1dc8c118ea89f535a928672e81d0613) · Sep 2, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go)
Open commit details
History
96 lines (83 loc) · 3.63 KB
Code owner: @bongwoobak, @vMaroon, and @llm-d/router-maintainers
/
# extractor.go
Copy path
## File metadata and controls
  * 

96 lines (83 loc) · 3.63 KB
Code owner: @bongwoobak, @vMaroon, and @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Open symbols panel
Edit and raw actions
/* Copyright 2026 The llm-d Authors. Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0 Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License. */ package preciseprefixcache import ( "context" "fmt" "net" "strconv" "sigs.k8s.io/controller-runtime/pkg/log" "github.com/llm-d/llm-d-router/pkg/common/observability/logging" fwkdl "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/datalayer" ) var _ fwkdl.EndpointExtractor = &Producer{} // Extract processes endpoint lifecycle events emitted by the // endpoint-notification-source: add/update installs a per-pod ZMQ KV-events // subscriber, delete tears one down. No-op unless per-pod discovery is // enabled. func (p *Producer) Extract(ctx context.Context, event fwkdl.EndpointEvent) error { if !p.kvEventsConfig.DiscoverPods || p.kvEventsConfig.PodDiscoveryConfig == nil { return nil } meta := event.Endpoint.GetMetadata() if meta == nil || meta.ID.Name == "" { return nil } logger := log.FromContext(ctx).WithName(p.typedName.String()) endpointKey := meta.ID.String() switch event.Type { case fwkdl.EventAddOrUpdate: if err := p.ensureSubscriber(ctx, meta); err != nil { return err } logger.V(logging.DEBUG).Info("Adding subscriber", "endpoint", endpointKey) case fwkdl.EventDelete: p.subscribersManager.RemoveSubscriber(ctx, endpointKey) if meta.Address != "" { if err := p.kvCacheIndexer.KVBlockIndex().Clear(ctx, fmt.Sprintf("%s:%s", meta.Address, meta.Port)); err != nil { logger.Error(err, "Failed to clear index entries for removed endpoint", "endpoint", endpointKey, "address", meta.Address, "port", meta.Port) } } logger.V(logging.DEBUG).Info("Removed KV-events subscriber", "endpoint", endpointKey) } return nil } // ensureSubscriber idempotently installs a KV-events subscriber for the given // endpoint, dialing SocketPort + RankIndex to match standard inference-engine port offsetting // (one ZMQ PUB socket per DP rank on the same pod IP). func (p *Producer) ensureSubscriber(ctx context.Context, meta *fwkdl.EndpointMetadata) error { if meta == nil || meta.Address == "" { return nil } endpointKey := meta.ID.String() port := p.kvEventsConfig.PodDiscoveryConfig.SocketPort + meta.GetRankIndex() zmqEndpoint := "tcp://" + net.JoinHostPort(meta.Address, strconv.Itoa(port)) replayEndpoint := "" if replayPort := p.kvEventsConfig.PodDiscoveryConfig.EffectiveReplayPort(); replayPort > 0 { replayEndpoint = "tcp://" + net.JoinHostPort(meta.Address, strconv.Itoa(replayPort+meta.GetRankIndex())) } sourceEndpoint := fmt.Sprintf("%s:%s", meta.Address, meta.Port) logger := log.FromContext(ctx).WithName(p.typedName.String()) // subscriberCtx is plugin-lifetime; caller ctx would tear subscribers // down on request completion. if err := p.subscribersManager.EnsureSubscriber(p.subscriberCtx, endpointKey, sourceEndpoint, zmqEndpoint, replayEndpoint, p.kvEventsConfig.TopicFilter, true); err != nil { logger.Error(err, "Failed to ensure KV-events subscriber for endpoint", "endpoint", endpointKey, "address", meta.Address) return fmt.Errorf("ensure subscriber for %s: %w", endpointKey, err) } logger.V(logging.DEBUG).Info("Ensured KV-events subscriber", "endpoint", endpointKey, "zmq", zmqEndpoint, "replay", replayEndpoint) return nil }
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
Copyright 2026 The llm-d Authors.
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
package preciseprefixcache
import (
"context"
"fmt"
"net"
"strconv"
"sigs.k8s.io/controller-runtime/pkg/log"
"github.com/llm-d/llm-d-router/pkg/common/observability/logging"
fwkdl "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/datalayer"
var _ fwkdl.EndpointExtractor = Producer{}
// Extract processes endpoint lifecycle events emitted by the
// endpoint-notification-source: add/update installs a per-pod ZMQ KV-events
// subscriber, delete tears one down. No-op unless per-pod discovery is
// enabled.
func (p *Producer) Extract(ctx context.Context, event fwkdl.EndpointEvent) error {
if !p.kvEventsConfig.DiscoverPods || p.kvEventsConfig.PodDiscoveryConfig == nil {
return nil
meta := event.Endpoint.GetMetadata()
if meta == nil || meta.ID.Name == "" {
return nil
logger := log.FromContext(ctx).WithName(p.typedName.String())
endpointKey := meta.ID.String()
switch event.Type {
case fwkdl.EventAddOrUpdate:
if err := p.ensureSubscriber(ctx, meta); err != nil {
return err
logger.V(logging.DEBUG).Info("Adding subscriber", "endpoint", endpointKey)
case fwkdl.EventDelete:
p.subscribersManager.RemoveSubscriber(ctx, endpointKey)
if meta.Address != "" {
if err := p.kvCacheIndexer.KVBlockIndex().Clear(ctx, fmt.Sprintf("%s:%s", meta.Address, meta.Port)); err != nil {
logger.Error(err, "Failed to clear index entries for removed endpoint",
"endpoint", endpointKey, "address", meta.Address, "port", meta.Port)
logger.V(logging.DEBUG).Info("Removed KV-events subscriber", "endpoint", endpointKey)
return nil
// ensureSubscriber idempotently installs a KV-events subscriber for the given
// endpoint, dialing SocketPort + RankIndex to match standard inference-engine port offsetting
// (one ZMQ PUB socket per DP rank on the same pod IP).
func (p *Producer) ensureSubscriber(ctx context.Context, meta *fwkdl.EndpointMetadata) error {
if meta == nil || meta.Address == "" {
return nil
endpointKey := meta.ID.String()
port := p.kvEventsConfig.PodDiscoveryConfig.SocketPort + meta.GetRankIndex()
zmqEndpoint := "tcp://" + net.JoinHostPort(meta.Address, strconv.Itoa(port))
replayEndpoint := ""
if replayPort := p.kvEventsConfig.PodDiscoveryConfig.EffectiveReplayPort(); replayPort 0 {
replayEndpoint = "tcp://" + net.JoinHostPort(meta.Address, strconv.Itoa(replayPort+meta.GetRankIndex()))
sourceEndpoint := fmt.Sprintf("%s:%s", meta.Address, meta.Port)
logger := log.FromContext(ctx).WithName(p.typedName.String())
// subscriberCtx is plugin-lifetime; caller ctx would tear subscribers
// down on request completion.
if err := p.subscribersManager.EnsureSubscriber(p.subscriberCtx, endpointKey,
sourceEndpoint, zmqEndpoint, replayEndpoint, p.kvEventsConfig.TopicFilter, true); err != nil {
logger.Error(err, "Failed to ensure KV-events subscriber for endpoint",
"endpoint", endpointKey, "address", meta.Address)
return fmt.Errorf("ensure subscriber for %s: %w", endpointKey, err)
logger.V(logging.DEBUG).Info("Ensured KV-events subscriber",
"endpoint", endpointKey, "zmq", zmqEndpoint, "replay", replayEndpoint)
return nil
You can’t perform that action at this time. 
While the code is focused, press Alt+F1 for a menu of operations.
