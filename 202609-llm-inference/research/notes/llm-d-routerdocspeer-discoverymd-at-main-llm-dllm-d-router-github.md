---
title: llm-d-router/docs/peer-discovery.md at main · llm-d/llm-d-router · GitHub
id: llm-d-routerdocspeer-discoverymd-at-main-llm-dllm-d-router-github
tags:
- llmd-router-nonk8s-backends-54b774
- locus-availability-and-lifecycle-off-k8s
- peer-discovery
- epp-ha
created: '2026-09-11T21:23:50.214089Z'
updated: '2026-09-15T04:55:23.924225Z'
source: https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md
source_domain: github.com
fetched_at: '2026-09-11T21:23:50.191984Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Defines the PeerDiscovery interface used ONLY for active-active EPP cross-replica
  state sync (a future prefix-cache metadata syncer), not for leader election. Configured
  via dataLayer.discovery.peers.pluginRef in the EndpointPickerConfig, mirroring the
  endpoint-discovery plugin model: ''It is configured via dataLayer.discovery.peers.pluginRef...
  Implementations are registered in the plugin registry and selected by name. When
  omitted, peer discovery is disabled and no peer set is maintained.'' No built-in
  plugin implementations (no ''kubernetes'', ''file'', or ''static'' plugin type)
  are named anywhere in the doc -- the only example given is a hand-written custom
  plugin (MyPeerDiscovery) that a developer would author and register via fwkplugin.Register.
  PeerStore (peers, keyed by EPP replica identity, consumed by a cross-replica syncer)
  is explicitly kept separate from Datastore (endpoints, keyed by model-serving pod,
  consumed by the scheduler): ''Keeping them separate avoids coupling unrelated subsystems.''
  The PeerNotifier is explicitly ''not goroutine-safe'' and requires all Upsert/Delete
  calls to come from a single goroutine in order, but the doc gives no reconciliation
  or consistency mechanism for divergence across replicas using file/custom discovery
  sources -- that topic is not addressed here. Does NOT mention Kubernetes, Lease,
  coordination.k8s.io, or leader election anywhere; that mechanism lives in a sibling
  doc (operations.md), fetched separately.'
---

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# peer-discovery.md
Copy path
More file actions
More file actions
## Latest commit
[config: consolidate discovery plugins under a single discovery sectio…](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19)
Open commit detailsfailure
Aug 25, 2026
[950c963](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19) · Aug 25, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/docs/peer-discovery.md)
Open commit details
History
215 lines (161 loc) · 6.26 KB
Code owner: @llm-d/router-maintainers
/
# peer-discovery.md
Copy path
## File metadata and controls
  * Preview
  * 

215 lines (161 loc) · 6.26 KB
Code owner: @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Peer Discovery
## Table of Contents
  * [The PeerDiscovery Interface](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md#the-peerdiscovery-interface)
    * [PeerDiscovery](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md#peerdiscovery)
    * [Ordering contract](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md#ordering-contract)
  * [Selecting a peer discovery plugin in the EPP config](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md#selecting-a-peer-discovery-plugin-in-the-epp-config)
  * [Writing a peer discovery plugin](https://github.com/llm-d/llm-d-router/blob/main/docs/peer-discovery.md#writing-a-peer-discovery-plugin)


## Overview
EPP replicas operating in an active-active topology need to know about each other so a future cross-replica syncer can exchange state (e.g. prefix-cache metadata) between them. **Peer discovery** is the mechanism that tracks the set of live EPP replicas and feeds it to consumers through a `PeerNotifier`.
Peer discovery mirrors the [endpoint discovery](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md) plugin model:
  * It is configured via `dataLayer.discovery.peers.pluginRef` in the `EndpointPickerConfig`.
  * Implementations are registered in the plugin registry and selected by name.
  * When omitted, peer discovery is disabled and no peer set is maintained.


### Why PeerStore is separate from Datastore
`PeerStore` and `Datastore` share the same Upsert/Delete/notifier shape, but they serve different consumers and have different lifecycles:  
| Endpoints (Datastore)  | Peers (PeerStore)  |  
| --- | --- |  
| **Consumer**  | Scheduler, request routing  | Cross-replica syncer  |  
| **Lifecycle**  | Bound to the InferencePool  | Bound to the EPP Deployment  |  
| **Identity**  | Model-serving pod  | EPP replica  |  
Keeping them separate avoids coupling unrelated subsystems.
## The PeerDiscovery Interface
### PeerDiscovery

```
// pkg/epp/framework/interface/datalayer/peer.go

type PeerDiscovery interface {
    plugin.Plugin   // provides TypedName() TypedName

    // Start begins discovery and blocks until ctx is cancelled or a fatal
    // error occurs. The caller invokes Start in a dedicated goroutine.
    Start(ctx context.Context, notifier PeerNotifier) error

    // Ready returns a channel that is closed once after the plugin has
    // completed its initial reconciliation with the underlying source.
    Ready() <-chan struct{}
}
```

### PeerNotifier

```
type PeerNotifier interface {
    // Upsert adds or updates a peer.
    Upsert(peer *PeerMetadata)
    // Delete removes a peer by its identity.
    Delete(id types.NamespacedName)
}
```

### PeerMetadata  
| Field  | Type  | Description  |  
| --- | --- | --- |  
| `types.NamespacedName`  | Stable identity of the peer, unique across the peer set.  |  
| `Address`  | `string`  | IP address of the peer replica.  |  
| `Port`  | `string`  | Port the peer listens on for state sync. Empty when the discovery source does not expose a port.  |  
### Ordering contract
`PeerNotifier` is **not goroutine-safe**. All `Upsert` and `Delete` calls must be made sequentially from a single goroutine so that an `Upsert` followed by a `Delete` for the same peer is applied in that order.
### PeerStore
`PeerStore` is the narrow interface consumed by `NewPeerNotifier`:

```
type PeerStore interface {
    PeerUpsert(ctx context.Context, meta *PeerMetadata)
    PeerDelete(id types.NamespacedName)
}
```

The in-tree `MemoryPeerStore` (`pkg/epp/statesync/peerstore.go`) is a goroutine-safe, in-memory implementation that also exposes a `Peers()` method returning a deterministically ordered snapshot of the current peer set.
## Selecting a peer discovery plugin in the EPP config
Add a `peers` entry inside `dataLayer.discovery` in the `EndpointPickerConfig`. The `pluginRef` field names a plugin instance defined in the top-level `plugins` list.

```
plugins:
  - name: my-peer-disc
    type: <peer-discovery-plugin-type>
    parameters:
      # plugin-specific parameters

dataLayer:
  discovery:
    peers:
      pluginRef: my-peer-disc  # must match a name in plugins above
```

When `dataLayer.discovery.peers` is absent, peer discovery is disabled and no peer set is maintained (backwards compatible).
## Writing a peer discovery plugin
Implement `fwkdl.PeerDiscovery` and register the factory with the plugin registry. A runnable example of this pattern is in the test suite:
  * [`pkg/epp/framework/interface/datalayer/peer_test.go`](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/datalayer/peer_test.go) tests the interface contract with a fake store.
  * [`pkg/epp/statesync/peerstore_test.go`](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/statesync/peerstore_test.go) (`TestPeerDiscoveryFullStack`) exercises the full plugin-to-store pipeline with `MemoryPeerStore`.



```
package myplugin

import (
    "context"
    "encoding/json"
    "sync"

    "k8s.io/apimachinery/pkg/types"

    fwkdl    "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/datalayer"
    fwkplugin "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin"
)

const PluginType = "my-peer-discovery"

type MyPeerDiscovery struct {
    typedName fwkplugin.TypedName
    ready     chan struct{}
    readyOnce sync.Once
}

var _ fwkdl.PeerDiscovery = (*MyPeerDiscovery)(nil)

func Factory(name string, params *json.Decoder, _ fwkplugin.Handle) (fwkplugin.Plugin, error) {
    if name == "" {
        name = PluginType
    }
    return MyPeerDiscovery{
        typedName: fwkplugin.TypedName{Type: PluginType, Name: name},
        ready:     make(chan struct{}),
    }, nil
}

func (d *MyPeerDiscovery) TypedName() fwkplugin.TypedName { return d.typedName }
func (d *MyPeerDiscovery) Ready() <-chan struct{}           { return d.ready }

func (d *MyPeerDiscovery) Start(ctx context.Context, notifier fwkdl.PeerNotifier) error {
    // 1. Enumerate existing peers.
    notifier.Upsert(fwkdl.PeerMetadata{
        ID:      types.NamespacedName{Name: "epp-1", Namespace: "default"},
        Address: "10.0.0.2",
        Port:    "9002",
    })
    d.readyOnce.Do(func() { close(d.ready) })

    // 2. Watch for changes and call Upsert/Delete as peers come and go.
    //    All calls must be from this goroutine (PeerNotifier is not
    //    goroutine-safe).
    <-ctx.Done()
    return nil
}
```

Register the factory in the runner:

```
fwkplugin.Register(myplugin.PluginType, fwkplugin.StabilityAlpha, myplugin.Factory)
```

Then reference it in the EPP config:

```
plugins:
  - name: my-peer-disc
    type: my-peer-discovery
    parameters: {}

dataLayer:
  discovery:
    peers:
      pluginRef: my-peer-disc
```

You can’t perform that action at this time. 
