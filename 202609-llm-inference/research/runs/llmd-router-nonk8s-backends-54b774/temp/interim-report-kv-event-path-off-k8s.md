# Interim report: kv-event-path-off-k8s

**Locus question:** Does precise prefix-cache-aware routing (llm-d's flagship scorer) work when endpoints come from file/custom discovery — i.e. does the EPP-embedded KV indexer use Static Endpoint Mode (backends push to a bound ZMQ SUB) or a Kubernetes pod reconciler, and do KV-event identities (kv@ip:port@model) line up with EndpointDiscovery's NamespacedName ids?

**Flavor:** technical (resolves contradiction-graph cluster `kv-event-subscription-mode-off-k8s`)

## What the corpus already said

The width sweep left this as an open, unresolved tension, logged in the contradiction graph as cluster `kv-event-subscription-mode-off-k8s`: side A ("router-side pull via a Kubernetes pod reconciler, RBAC-gated") vs. side B ("push via Static Endpoint Mode, zero k8s API, opaque pod identity"), with the graph flagging the open question as "whether the EPP-embedded indexer in file-discovery mode can use Static Endpoint Mode... and whether identity strings line up (ip:port vs NamespacedName)."

[[llm-dguidesprecise-prefix-cache-routingreadmemd-at-main-llm-dllm-d-github]] (the guide) states pod-discovery "registers the precise-prefix-cache-producer as an extractor on the data-layer `endpoint-notification-source`" — a phrase that, read without the code, could be mistaken for describing the standalone kv-cache library's own Kubernetes reconciler. [[llm-d-kv-cachedocsconfigurationmd-at-main-llm-dllm-d-kv-cache-github]] and [[llm-d-kv-cachedocsarchitecturemd-at-main-llm-dllm-d-kv-cache-github]] independently document the standalone `github.com/llm-d/llm-d-kv-cache` Go library's own two delivery modes (Centralized/static-bind vs. Kubernetes-label-selector pod-discovery), and [[llm-d-kv-cacheexamplesvalkey_examplereadmemd-at-v040-llm-dllm-d-kv-cache-github]] shows the library's scoring API treats pod identity as an opaque string. [[data-layer-llm-d]] independently established that the EPP's generic data-layer `EndpointSource`+`EndpointExtractor` pattern ("endpoint-notification-source") is backend-agnostic — it "only reacts to endpoints already in the EPP's own datastore regardless of how they got there." None of these sources, individually, closed the loop between "the router says it uses `endpoint-notification-source`" and "the standalone library's docs describe a Kubernetes-only pod-discovery mode" — that gap is exactly what this locus was opened to resolve, and closing it required reading the actual `llm-d-router` plugin source rather than either repo's docs pages alone.

## What the new sources say

I fetched (via a delegated hyperresearch-fetcher spawn against the exact directory the loci-analyst flagged) the `precise-prefix-cache-producer` plugin's directory listing, its `README.md`, and — critically — its two Go source files, `producer.go` and `extractor.go`, at `github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache`. These are now [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducer-at-main-llm-dllm-d]], [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcachere]], [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcachepr]], and [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcacheex]].

**The README states, in its own words**, under "Pipeline per request": "(`EndpointExtractor`) Per-pod ZMQ subscriber lifecycle on add/delete." `extractor.go` confirms this literally: `var _ fwkdl.EndpointExtractor = &Producer{}`, where `fwkdl` is `github.com/llm-d/llm-d-router/pkg/epp/framework/interface/datalayer` — the same generic, backend-agnostic data-layer package [[data-layer-llm-d]] documents. The full `Extract` method:

```go
// Extract processes endpoint lifecycle events emitted by the
// endpoint-notification-source: add/update installs a per-pod ZMQ KV-events
// subscriber, delete tears one down. No-op unless per-pod discovery is
// enabled.
func (p *Producer) Extract(ctx context.Context, event fwkdl.EndpointEvent) error {
    if !p.kvEventsConfig.DiscoverPods || p.kvEventsConfig.PodDiscoveryConfig == nil {
        return nil
    }
    meta := event.Endpoint.GetMetadata()
    if meta == nil || meta.ID.Name == "" {
        return nil
    }
    endpointKey := meta.ID.String()
    switch event.Type {
    case fwkdl.EventAddOrUpdate:
        if err := p.ensureSubscriber(ctx, meta); err != nil { return err }
    case fwkdl.EventDelete:
        p.subscribersManager.RemoveSubscriber(ctx, endpointKey)
        if meta.Address != "" {
            p.kvCacheIndexer.KVBlockIndex().Clear(ctx, fmt.Sprintf("%s:%s", meta.Address, meta.Port))
        }
    }
    return nil
}
```

and `ensureSubscriber`:

```go
// ensureSubscriber idempotently installs a KV-events subscriber for the given
// endpoint, dialing SocketPort + RankIndex to match standard inference-engine
// port offsetting (one ZMQ PUB socket per DP rank on the same pod IP).
func (p *Producer) ensureSubscriber(ctx context.Context, meta *fwkdl.EndpointMetadata) error {
    endpointKey := meta.ID.String()
    port := p.kvEventsConfig.PodDiscoveryConfig.SocketPort + meta.GetRankIndex()
    zmqEndpoint := "tcp://" + net.JoinHostPort(meta.Address, strconv.Itoa(port))
    sourceEndpoint := fmt.Sprintf("%s:%s", meta.Address, meta.Port)
    return p.subscribersManager.EnsureSubscriber(
        p.subscriberCtx, endpointKey, sourceEndpoint, zmqEndpoint, replayEndpoint,
        p.kvEventsConfig.TopicFilter, true, // remoteSocket=true: dial out to a pod-bound PUB
    )
}
```

This is a direct, primary-source answer to every part of the locus question, with no inference required:

1. **Discovery mechanism**: `Extract` is invoked by the generic `endpoint-notification-source` on `fwkdl.EventAddOrUpdate`/`fwkdl.EventDelete` — the *same* event stream that fires for every `EndpointDiscovery` plugin, Kubernetes or file, because all of them write through the identical `notifier.Upsert(*EndpointMetadata)`/`Delete(types.NamespacedName)` contract into one datastore ([[llm-d-routerpkgeppframeworkinterfacedatalayerdiscoverygo-at-main-llm-dllm-d-rout]]). `PodDiscoveryConfig` survives in the router's own internal `kvevents.Config` type purely as a config carrier for `SocketPort`/`EffectiveReplayPort()` — it is read for two integer values, never used to open a Kubernetes watch. The gate `if !p.kvEventsConfig.DiscoverPods || p.kvEventsConfig.PodDiscoveryConfig == nil` is a **config toggle** the operator sets in the plugin's YAML params, not a code path that spins up `ctrl.Manager` or any Kubernetes client. This confirms the guide's "registers the precise-prefix-cache-producer as an extractor on the data-layer endpoint-notification-source" is a precise engineering description, not marketing gloss over a hidden Kubernetes dependency.

2. **Port derivation, not carried by file-discovery's schema**: the ZMQ dial address is `SocketPort + meta.GetRankIndex()` applied to `meta.Address` — a single operator-configured base port (`PodDiscoveryConfig.SocketPort`, part of the router plugin's own YAML config, defaulting to the same 5557 convention as the standalone library) offset by a per-endpoint `RankIndex` for data-parallel multi-rank engines. Every source I read for FileDiscovery's YAML schema ([[plugingo]], [[file-discovery-34-add-filediscovery-plugin-with-yamljson-file-loading-and-fsnoti]], [[llm-d-routerpkgeppframeworkpluginsdatalayerdiscoveryfile-at-main-llm-dllm-d-rout]]) lists only `name`, `namespace`, `address`, `port` (inference-serving port), `labels` — no `rankIndex` field. For a single-rank backend `meta.GetRankIndex()` defaults to 0 (an accessor method, not a struct field the file must supply), so `port = SocketPort`, and file-discovery works with zero schema changes for the overwhelmingly common non-DP deployment shape. For a genuinely multi-rank data-parallel backend declared via file-discovery, nothing in the corpus shows how (or whether) `RankIndex` can be populated from the file schema — that is a real, narrow, well-scoped gap, not the broad "silently degrades" risk the locus was framed around.

3. **The identity question, resolved precisely — two separate identity spaces, not one that must "line up"**: `endpointKey := meta.ID.String()` (derived from `types.NamespacedName`) is used **only** as the `subscribersManager`'s bookkeeping key — to decide whether a live ZMQ subscription for this logical endpoint already exists, and to look it up again on delete (`RemoveSubscriber(ctx, endpointKey)`). It is never used to key the KV-block index or the scoring lookup. That job goes to a *second*, independent literal string, `fmt.Sprintf("%s:%s", meta.Address, meta.Port)` — called `sourceEndpoint` in `ensureSubscriber`, and it appears identically in three independent places: here, in `Extract`'s `Clear(ctx, fmt.Sprintf("%s:%s", meta.Address, meta.Port))` call on delete, and in `producer.go`'s scoring lookup, `matches[fmt.Sprintf("%s:%s", md.Address, md.Port)]` (see [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcachepr]]). So `kv@<ip>:<port>@<model>` (the vLLM ZMQ topic format, confirmed in [[vllmvllmdistributedkv_eventspy-at-main-vllm-projectvllm-github]]) and `NamespacedName` were never designed to be string-equal, and don't need to be — the router bridges them via the `Address`/`Port` fields every `EndpointMetadata` object carries regardless of which discovery plugin populated it, while `NamespacedName` is a orthogonal bookkeeping key for subscriber lifecycle management only. The locus's framing ("do identities line up") presupposes a single identity space that must reconcile; the actual design deliberately keeps two, and only one of them (Address:Port) is load-bearing for routing correctness.

**File-discovery's schema is otherwise fully compatible with this wiring.** [[plugingo]] and [[llm-d-routerdocsdiscoverymd-at-main-llm-dllm-d-router-github]] confirm `FileDiscovery` constructs the *identical* `EndpointMetadata{ID: types.NamespacedName, Address, Port, MetricsHost, Labels}` struct the Kubernetes discovery plugins construct — same Go type, same fields, same consumption by every downstream data-layer plugin including this one. FileDiscovery's IPv4-literal-only constraint ([[file-discovery-34-add-filediscovery-plugin-with-yamljson-file-loading-and-fsnoti]]) is a *good fit*, not a limitation, for the KV-event path specifically, since vLLM's own topic format is IP-literal by construction. `Extract`'s guard `meta.ID.Name == ""` is trivially satisfied since FileDiscovery's `name` field is required and validated at load time.

## Evidence synthesis

Putting the standalone-library docs and the router's own plugin source together, the two "sides" of the contradiction-graph cluster describe two different layers, and the width sweep's sources were each individually accurate but not code-adjacent enough to see the seam:

- **Side B's evidence** (Static Endpoint Mode, zero k8s API, opaque pod identity) is real, and describes both the standalone `llm-d-kv-cache` library used as a library, and the router's own `New()`-time fallback: `if config.KVEventsConfig.ZMQEndpoint != "" { subscribersManager.EnsureSubscriber(ctx, "local-subscriber", "", config.KVEventsConfig.ZMQEndpoint, "", topicFilter, false) }` — `remoteSocket=false`, one static subscriber bound to a single configured global endpoint, no discovery involvement at all.
- **Side A's evidence** (a mechanism that discovers backends and opens one ZMQ SUB connection per discovered endpoint) is also real — but the mechanism that discovers them, in the actual code path exercised by `llm-d-router`, is `fwkdl.EndpointExtractor.Extract`, fired by the generic `endpoint-notification-source` for *any* `EndpointDiscovery` plugin's `Upsert`/`Delete` calls — not a live Kubernetes pod watch embedded in this plugin or in a library dependency it calls into. `remoteSocket=true` in this path confirms the router dials *out* to a pod/backend-bound PUB socket, matching the "pod discovery" delivery-mode semantics described for the standalone library, but driven by the router's own discovery-agnostic event stream rather than a Kubernetes-only reconciler.

Three independently-dated, independently-motivated sources converge on this reading without sharing an author or a publication moment: the plugin's own README (docs), the plugin's own `producer.go`+`extractor.go` source (code, both fetched live at HEAD), and [[endpoint-metadata-updates-do-not-trigger-endpoint-notification-events-issue-1403]] — a merged bug-fix issue about a *different* failure mode (metadata updates not re-firing notifications) whose problem description independently states "precise-prefix-cache components... install/refresh per-endpoint KV-events (ZMQ) subscribers keyed on endpoint address/port" via the same `endpoint-notification-source`. A bug report written by someone debugging an unrelated issue, describing the same wiring from a completely different angle, is strong corroboration precisely because it wasn't written to answer this question.

The place the evidence stays genuinely open is **demonstration**, not mechanism. [[llm-dguidesno-kubernetes-deployment-at-main-llm-dllm-d-github]] and [[configyaml]] both confirm the shipped no-Kubernetes reference deployment uses the *approximate* `prefix-cache-scorer` (character-hash, no ZMQ, no tokenizer dependency) instead of the precise producer. Given the code now confirms the precise producer's dynamic subscription path has no Kubernetes dependency, the most likely reading is that the maintainers chose the simpler, zero-network-topology-assumption scorer for a reference guide aimed at minimizing moving parts for first-time non-Kubernetes adopters — not that precise routing is broken off-Kubernetes. But this is inference from absence, not a source that says so directly, and no guide, blog, benchmark report, or issue in the corpus shows the two paired end-to-end.

## Committed position

The evidence supports a confident, non-hedged answer: precise prefix-cache-aware routing is **architecturally decoupled from Kubernetes** in llm-d-router's actual implementation, and works with file-discovery or any custom `EndpointDiscovery` plugin today, without router code changes, for the common (non-data-parallel) deployment shape. The producer's per-endpoint ZMQ subscription lifecycle is driven entirely by the generic `fwkdl.EndpointExtractor`/`endpoint-notification-source` data-layer pattern — confirmed by reading `extractor.go` directly, not inferred from docs — and its scoring/index lookups key on a literal `Address:Port` string built from fields every `EndpointDiscovery` plugin populates identically, never on `NamespacedName`. The "flagship scorer silently degrades off-k8s" worry that motivated this locus is not borne out by the code: the recommendation for a router-customization strategy that wants precise prefix-cache routing with non-k8s backends should be to **use it directly**, not to fall back to the approximate scorer out of unexamined caution. The one concrete, scoped condition to design around: `RankIndex`-based port offsetting (`SocketPort + meta.GetRankIndex()`) has no confirmed carrier in FileDiscovery's YAML schema, so a custom or extended discovery plugin needs to either keep every backend at rank 0 (single ZMQ port per pod IP, the default and by far the common case) or add a `rankIndex`-bearing field/label convention before file-discovery can address data-parallel multi-rank engines correctly.

- **Position:** Precise prefix-cache-aware routing is not Kubernetes-coupled in llm-d-router's real implementation — its per-endpoint KV-event subscription lifecycle runs through the generic `fwkdl.EndpointExtractor` interface fed by `endpoint-notification-source` (confirmed by direct read of `extractor.go`), and its scoring/index lookups key on a literal `Address:Port` string rather than `NamespacedName` (confirmed in three independent code locations), so file-discovery — or any custom `EndpointDiscovery` plugin populating standard `EndpointMetadata` — is a fully valid, unmodified source of endpoints for it today, for single-rank (non-data-parallel) backends.
- **Confidence:** high. This is no longer triangulated inference; `extractor.go` was read directly and quoted verbatim above, closing the one gap ("I was not able to fetch extractor.go") that would have kept this at medium.
- **Boundary conditions:** Holds for llm-d-router at HEAD (~Sep 2026, both files fetched live) using the shipped vLLM/SGLang KV-events wire format, for endpoints at `RankIndex` 0 (the default when unset — i.e., one ZMQ PUB socket per pod IP, no data-parallel sharding). Does not resolve: (a) how a multi-rank data-parallel backend would set a non-zero `RankIndex` through file-discovery's current schema — no evidence either way, since the schema has no such field; (b) dead-worker cleanup off-Kubernetes — [[no-kubernetes-no-problem-llm-d-now-runs-anywhere-llm-d]] states endpoint-lifecycle removal is the orchestrator's job off-Kubernetes, so a backend that dies without the operator's tooling calling `Delete` (or rewriting the endpoints file) leaves its ZMQ subscriber and index entries live until an operator-driven `EventDelete` triggers `Extract`'s cleanup path.
- **What would change this position:** A maintainer statement or code path (not found in `producer.go`/`extractor.go`) showing `PodDiscoveryConfig` is ever used for anything beyond the two integer fields read here (`SocketPort`, `EffectiveReplayPort()`) — e.g., if a build tag or alternate code path actually instantiates a live Kubernetes watch inside this plugin for some deployment mode I didn't fetch — would overturn the "fully decoupled" claim. A benchmark or incident report showing precise routing returning stale/zero matches specifically against file-discovery-sourced endpoints (as opposed to Kubernetes-sourced ones) would falsify the "safe to use directly" recommendation.
- **Evidence weight:** 2 primary-source Go files (`producer.go`, `extractor.go`, read verbatim, code at HEAD) + 1 plugin README (self-description matches the code) + 1 independently-motivated bug-fix issue (#1403, corroborates from an orthogonal angle) all agree on the mechanism, with zero sources in the corpus contradicting it. The "undemonstrated end-to-end" caveat rests on an audited absence: 2 independent shipped artifacts (the no-k8s guide's `config.yaml` and its companion architecture doc) both ship the approximate scorer instead, and no source in the corpus shows the precise producer paired with file-discovery in a running deployment.

## Open questions

- Can `RankIndex` be set on a file-discovery-sourced `EndpointMetadata` at all — via a `labels` entry, a schema extension, or is it hard-coded to 0 for every non-Kubernetes discovery plugin today? Not confirmed anywhere in the corpus; this is the sharpest remaining gap and would need either a source-analyst pass over `EndpointMetadata`'s definition and `GetRankIndex()`'s implementation, or a direct question to the llm-d-router maintainers.
- Has anyone in the llm-d community (issues, Slack, KubeCon talks) actually run precise-prefix-cache-producer against file-discovery or a Slurm/Ray-backed custom discovery plugin end-to-end? An audited absence — searched the vault, found none.
- What is `subscriberManager.EnsureSubscriber`'s actual dedup/idempotency behavior when `Extract` fires `EventAddOrUpdate` repeatedly for the same endpoint with an unchanged `Address:Port` (e.g., a metadata-only update)? Not fetched; relevant to whether file-discovery's reload-on-write behavior (which re-Upserts every entry on each file change, per [[file-discovery-34-add-filediscovery-plugin-with-yamljson-file-loading-and-fsnoti]]) could cause subscriber churn.

## Sources

1. [[llm-dguidesprecise-prefix-cache-routingreadmemd-at-main-llm-dllm-d-github]] — llm-d guide, precise-prefix-cache-routing README
2. [[llm-d-kv-cachedocsconfigurationmd-at-main-llm-dllm-d-kv-cache-github]] — standalone kv-cache library configuration reference
3. [[llm-d-kv-cachedocsarchitecturemd-at-main-llm-dllm-d-kv-cache-github]] — standalone kv-cache library architecture doc
4. [[llm-d-kv-cacheexamplesvalkey_examplereadmemd-at-v040-llm-dllm-d-kv-cache-github]] — Valkey backend example, opaque pod-identity demonstration
5. [[llm-d-routerpkgeppframeworkinterfacedatalayerdiscoverygo-at-main-llm-dllm-d-rout]] — EndpointDiscovery Go interface source
6. [[llm-d-routerpkgeppframeworkpluginsdatalayerdiscoveryfile-at-main-llm-dllm-d-rout]] — FileDiscovery plugin README
7. [[llm-d-routerdocsdiscoverymd-at-main-llm-dllm-d-router-github]] — canonical discovery.md, resolves config-path discrepancy
8. [[discovery-plugins-k8s-and-file-based-endpoint-discovery-by-ezrasilvera-pull-requ]] — PR #1007, delivers K8s + file discovery plugins
9. [[data-layer-llm-d]] — Data Layer design doc (PollingDataSource/NotificationSource/EndpointSource)
10. [[configyaml]] — shipped no-k8s EPP config.yaml (approximate scorer + file-discovery)
11. [[plugingo]] — file-discovery plugin.go source
12. [[registrygo]] — plugin registry source (out-of-tree registration mechanism)
13. [[runnergo]] — cmd/epp/runner/runner.go source (resolveDiscovery, registerInTreePlugins)
14. [[endpoint-metadata-updates-do-not-trigger-endpoint-notification-events-issue-1403]] — merged bug fix corroborating EndpointExtractor wiring
15. [[llm-ddocsinfrastructureno-kubernetes-deploymentmd-at-main-llm-dllm-d-github]] — no-k8s architecture/rationale doc
16. [[llm-dguidesno-kubernetes-deployment-at-main-llm-dllm-d-github]] — no-k8s step-by-step guide (approximate scorer confirmed)
17. [[no-kubernetes-no-problem-llm-d-now-runs-anywhere-llm-d]] — llm-d.ai blog introducing EndpointDiscovery
18. [[prefix-cache-aware-routing-llm-d]] — llm-d docs, approximate vs. precise routing overview
19. [[rfc-kv-aware-routing-for-pd-disaggregated-serving-via-sourced-kv-events-issue-99]] — vLLM-Ascend RFC on source-instance-keyed KV events
20. [[master-kv-cache-aware-routing-with-llm-d-for-efficient-ai-inference-red-hat-deve]] — Red Hat Developer article (stale topic-format claim flagged)
21. [[vllmvllmdistributedkv_eventspy-at-main-vllm-projectvllm-github]] — vLLM kv_events.py source (topic format, bind/connect heuristic)
22. [[feat-add-pluggable-discoveryplugin-interface-by-ezrasilvera-pull-request-908-llm]] — PR #908, original EndpointDiscovery interface + NamespacedName identity debate
23. [[file-discovery-34-add-filediscovery-plugin-with-yamljson-file-loading-and-fsnoti]] — PR #1135, FileDiscovery implementation + k8s-residue note
24. [[filediscovery-plugin-44-wiring-into-the-runner-by-ezrasilvera-pull-request-1218]] — PR #1218, runner wiring + Ready() semantics
25. [[config-consolidate-discovery-plugins-under-a-single-discovery-sectio-llm-dllm-d]] — commit resolving config path to dataLayer.discovery.endpoints.pluginRef
26. [[endpoint-picker-epp-llm-d]] — EPP architecture overview (4 pluggable components)
27. [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducer-at-main-llm-dllm-d]] — dataproducer directory listing (new fetch)
28. [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcachere]] — precise-prefix-cache-producer README (new fetch)
29. [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcachepr]] — precise-prefix-cache-producer producer.go source (new fetch)
30. [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcacheex]] — precise-prefix-cache-producer extractor.go source (new fetch, highest-value, resolves the locus definitively)
