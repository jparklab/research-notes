---
title: cross_replica_syncer.go
id: cross_replica_syncergo
tags:
- llmd-router-nonk8s-backends-54b774
- cross-replica-syncer
- epp-ha
- active-active
- gap-1
created: '2026-09-11T21:49:04.401464Z'
updated: '2026-09-15T04:55:23.968593Z'
source: https://raw.githubusercontent.com/llm-d/llm-d-router/main/pkg/epp/framework/interface/datalayer/cross_replica_syncer.go
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-11T21:49:04.378841Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Defines CrossReplicaSyncer, a SEPARATE and more general cross-replica state-sharing
  mechanism than PeerDiscovery/peer-discovery.md -- directly load-bearing for gap-1.
  Interface: Set(ctx, key StateKey, endpointID string, value any, aggregate func([]any)
  any) error (runtime calls this ''periodically, once per live endpoint, with a fresh
  local snapshot''), Get(ctx, key, endpointID) (any, bool, error) (returns ''the prepared
  aggregate for the given key and endpoint across all replicas''), Delete, and GetOrSet(ctx,
  key, id, candidate) (actual any, existed bool, err error) for ''global request-level
  state shared across EPP replicas... Implementations must provide linearizable behavior
  across every EPP replica sharing the syncer.'' CrossReplicaContributor is an OPT-IN
  interface (''The plugin''s Extract method is unchanged; the runtime detects this
  interface and wires the store transparently'') that any endpoint-extractor/data-producer
  plugin can implement to get its per-endpoint attribute synced across replicas via
  CrossReplicaSpec{StateKey, AttributeKey, Supply, Aggregate, SyncDisabled}. Doc comment:
  ''Prefer it for per-endpoint state that can tolerate periodic synchronization''
  (i.e. NOT for hard-realtime correctness). CRITICAL: this is a distinct, general-purpose
  plugin surface separate from PeerDiscovery/peer-discovery.md''s peer-set membership
  tracking -- PeerDiscovery only discovers WHICH replicas exist; CrossReplicaSyncer
  is what actually MOVES per-endpoint state between them, and it is opt-in per plugin
  (SyncDisabled defaults false = enabled once implemented). The precise-prefix-cache
  extractor.go (already in vault) implements only fwkdl.EndpointExtractor, NOT CrossReplicaContributor
  -- confirming precise prefix cache achieves active-active consistency purely via
  independent per-replica KV-event subscription, not via this cross-replica-syncer
  path, consistent with the draft''s claim. This mechanism is the most plausible framework-level
  hook for syncing session-affinity or scorer state across replicas that the draft
  flagged as an open question, but no plugin in the vault so far is confirmed to implement
  CrossReplicaContributor.'
---

*Suggested by [[datalayer]] — file listing led to cross-replica state sync interface - directly relevant to gap-1 active-active correctness*


```
/*
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
*/

package datalayer

import (
	"context"

	fwkplugin "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin"
)

// StateKey namespaces cross-EPP shared state.
type StateKey string

// CrossReplicaSyncer synchronizes shared state across EPP replicas.
// Implementations own the storage mechanism and must provide the atomic
// consistency required by GetOrSet.
type CrossReplicaSyncer interface {
	fwkplugin.Plugin

	// Set writes a value for the given key and endpoint and prepares the
	// aggregate returned by Get. The runtime calls this periodically, once per
	// live endpoint, with a fresh local snapshot.
	Set(ctx context.Context, key StateKey, endpointID string, value any, aggregate func([]any) any) error

	// Get returns the prepared aggregate for the given key and endpoint across
	// all replicas. Returns (value, true, nil) on hit, (nil, false, nil) on
	// miss, or (nil, false, err) on failure.
	Get(ctx context.Context, key StateKey, endpointID string) (any, bool, error)

	// Delete removes the value for the given key and endpoint.
	Delete(ctx context.Context, key StateKey, endpointID string) error

	// GetOrSet atomically returns the value already stored for key and id, or
	// stores candidate and returns it. This is global request-level state shared
	// across EPP replicas, not per-endpoint state. Use it only when exact
	// coordination is required. Implementations own a fixed expiration period.
	// The bool reports whether the returned value already existed. Implementations
	// must provide linearizable behavior across every EPP replica sharing the syncer.
	GetOrSet(ctx context.Context, key StateKey, id string, candidate any) (actual any, existed bool, err error)
}

// CrossReplicaContributor is an opt-in interface for endpoint extractors that
// want their installed attributes to reflect cross-replica aggregate state.
// The plugin's Extract method is unchanged; the runtime detects this interface
// and wires the store transparently. Prefer it for per-endpoint state that can
// tolerate periodic synchronization.
type CrossReplicaContributor interface {
	CrossReplicaState() CrossReplicaSpec
}

// CrossReplicaSpec declares what a CrossReplicaContributor publishes and where.
type CrossReplicaSpec struct {
	// StateKey namespaces this contributor's data in the store.
	StateKey StateKey

	// AttributeKey is the attribute map key the plugin installs in Extract.
	// The runtime overwrites this key with a store-reading closure.
	AttributeKey fwkplugin.DataKey

	// Supply returns a closure that reads the live local value for the given
	// endpoint. The runtime calls this closure after Produce to snapshot
	// the current local state and Set it into the store.
	Supply func(endpointID string) func() Cloneable

	// Aggregate combines per-replica values into a single aggregate.
	// Called by the store's Set to fold values from all replicas.
	Aggregate func(values []any) any

	// SyncDisabled opts this contributor out of cross-replica synchronization
	// even though it implements CrossReplicaContributor. When true, the runtime
	// neither publishes the plugin's state nor installs the aggregate-reading
	// attribute, so the plugin's local value is used as-is. The zero value
	// (false) keeps synchronization enabled.
	SyncDisabled bool
}

```

