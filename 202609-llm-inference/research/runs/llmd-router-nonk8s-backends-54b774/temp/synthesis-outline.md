# Synthesis outline

## Opening (no heading — first ~150 words)
State the thesis directly: customize the `EndpointDiscovery` seam, reuse everything above it, and treat where the
router runs as a separate decision driven by the availability requirement. Name the one carve-out (the P/D sidecar)
and the one correction (the nested discovery key ships in no release yet).

## I. llm-d Component Inventory and Kubernetes Coupling
Establish the split that the whole report rests on: routing layer portable, operations layer Kubernetes-first. A
component table with a reuse verdict per component anchors it.

## II. How the llm-d Router Discovers and Tracks Backends Today
Show where the pod assumption actually lives — a reconciler watching Pods by label, and pod-shaped identity types —
and that the wire contract to the proxy is already just an `ip:port` header honored by an ORIGINAL_DST cluster.

## III. Extension Seams in the Router for Non-Pod Backends
The `EndpointDiscovery` interface, the file plugin as the working template, the exported registry and importable
runner, and the documented custom-plugin procedure — paired immediately with the first-adopter concession.

## IV. Upstream State: Proposals and Work on Non-Pod and External Endpoints
The merged chain that shipped file discovery, the watch-based plugin that was abandoned, and the release-versus-`main`
gap on the discovery config key — presented as facts a reader must act on when pinning a version.

## V. Non-Pod Backend Types and What Each Requires from the Router
The backend-side contract (OpenAI-compatible HTTP plus Prometheus gauges, KV events over ZMQ, NIXL for P/D) and what
each class of non-pod backend must supply — with the rank-index exception and managed APIs placed at the gateway tier.

## VI. Candidate Strategies Compared
Six strategies ranked, not listed: file plus generator, out-of-tree plugin, in-tree fork, library embedding, Kubernetes
shims, different router. Dispatch the shims on evidence and close with why the llm-d bet stays reversible.

## VII. Recommended Strategy and Implementation Roadmap
Decision rules first (availability → topology; churn and addressing → discovery rung; named gates for escalating to a
plugin), then phases with effort numbers and the standing obligations: health-gated discovery, version pin, deliberate
proxy failure mode, network isolation plus prefill TLS wherever disaggregation is used.

## VIII. Risks, Trade-offs, and Open Questions
First-adopter risk and the unresolved panic-isolation question, interface churn, the sidecar security carve-out with
its two issues dated separately, per-replica flow control, and the audited absences — ending in the committed synthesis.

## Sources
Not applicable: `citation_style` is `wikilink`, so `[[note-id]]` markers in the body are self-resolving and no Sources
section is emitted.
