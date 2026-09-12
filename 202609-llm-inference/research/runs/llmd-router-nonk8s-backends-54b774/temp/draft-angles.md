# Step 10 — three analytical angles

The corpus has genuine, live disagreements (see comparisons.md and source-tensions.json), so the ensemble uses the
tension-shaped assignment: strongest thesis / steelman of the minority / synthesis on boundary conditions.

## Draft A — Strongest-thesis

Argue that the right strategy is to customize llm-d at exactly one seam — the `EndpointDiscovery` plugin interface —
and reuse everything above it unchanged, shipping in phases: file-discovery fed by an external endpoint generator
first (zero Go), then a custom discovery plugin registered out-of-tree from a thin downstream `main()` importing
`cmd/epp/runner`, with upstreaming pursued in parallel. Make the affirmative case from code-level evidence that the
routing stack is platform-neutral — including the flagship precise prefix-cache scorer, which keys on `Address:Port`
through the generic `endpoint-notification-source` path rather than on Kubernetes identity.

## Draft B — Steelman-contrarian

Defend the minority position seriously: do NOT customize llm-d's router for non-pod backends. llm-d is
Kubernetes-native exactly where production operations live — leader-elected HA via `coordination.k8s.io/Lease`, Helm
charts that expose no discovery knobs, the P/D sidecar's InferencePool-bound SSRF allowlist, CRD-driven policy inputs
(InferenceObjective, InferenceModelRewrite), PodMonitor-based observability. The out-of-tree plugin has zero known
precedent (pkg.go.dev reports no importers) and no ratified support policy (issue #2105 is untriaged). The honest
alternatives are to convert the estate into pods (Slinky), keep the router in Kubernetes, or adopt a router that
already treats static/file backends as first-class (vLLM Router, Dynamo, vLLM production-stack).

## Draft C — Synthesis-reconciler

Argue that both positions hold under different conditions and build the decision rules. Which backend estate (fixed
bare-metal rack, Slurm allocation, Ray cluster, remote clusters, managed APIs), which scale and churn rate, which
availability requirement, which security boundary, and which team capability select file-discovery vs a custom plugin
vs Slinky vs a different router — and where the router itself should run (in-cluster for HA vs fully outside for
job-scoped and HPC work). Focus on thresholds, boundary conditions, and the operational obligations each choice
creates (health gating and eviction, network isolation for the prefill hop, per-replica flow control, version pinning).

---

Curated source lists: `draft-a-source-list.md`, `draft-b-source-list.md`, `draft-c-source-list.md`. All six interim
notes and the source-analysis note appear in every list; differentiation comes from the angle-specific remainder.
