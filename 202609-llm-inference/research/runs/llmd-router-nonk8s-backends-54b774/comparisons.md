# Cross-locus comparisons — llmd-router-nonk8s-backends-54b774

Six depth investigations committed positions. They assemble into one staged strategy (discovery seam customized,
everything above it reused), but five cross-locus dynamics complicate the simple reading and must each become a
visible argumentative beat in the report.

Committed positions, one line each:
- [[interim-report-extension-mechanism]] — Staged: file-discovery fed by an external generator first; otherwise an
  out-of-tree EndpointDiscovery plugin in a thin downstream main() importing cmd/epp/runner (beats in-tree fork and
  library embedding); upstream in parallel. Mitigate with namespaced plugin types + own panic recovery (#2105).
  Confidence: medium-high technical, medium on ranking.
- [[interim-report-kv-event-path-off-k8s]] — Precise prefix-cache routing works with file/custom discovery today
  (endpoint-notification-source + EndpointExtractor; keyed on Address:Port, not NamespacedName). Gap: RankIndex for
  data-parallel multi-rank engines. Confidence: high (extractor.go read verbatim).
- [[interim-report-availability-and-lifecycle-off-k8s]] — Default EPP HA uses Kubernetes coordination.k8s.io/Lease; all
  three documented HA mechanisms are Kubernetes-coupled. llm-d's Envoy standalone path already ships scoped fail-open
  (router.proxy.failOpen) for the lease-transition window; agentgateway passthrough stays fail-closed (#1661).
  Client-supplied x-gateway-destination-endpoint is discarded (spoofing closed). Confidence: high on Lease coupling
  (quoted), medium on the fail-open synthesis.
- [[interim-report-custom-discovery-plugin-design]] — Three-tier rule: T1 poll+diff (~2-4 days, default), T2 long-poll
  where the source supports it (Consul; +1-2 days), T3 persistent watch (~1-2 weeks, no in-repo precedent — PR #1007
  watch-based k8s plugin was closed unmerged). Floor: router ≥ v0.9.0 (#1403/#1404). Confidence: high on facts, medium
  on day estimates.
- [[interim-report-topology-and-k8s-shims]] — Router in-cluster vs out-of-cluster is an organizational choice orthogonal
  to discovery; drop EndpointSlice shims (EPP watches Pods via pod_reconciler.go, never EndpointSlices); virtual-kubelet
  only as a Liqo-class exception; Slinky is a different question ("convert the estate"). Confidence: high on shims,
  medium on the hybrid topology (capability inference, no case study).
- [[interim-report-lost-k8s-policy-inputs]] — Only per-request priority (InferenceObjective lookup → nil off-cluster) is
  a real gap → small static/file-backed priority plugin (Alpha tier); FlowControl bands are static YAML, fairness header
  free, model selection via x-llm-d-model-name-rewrite, weighted canary at gateway tier, PodMonitor → Prometheus
  static/file SD. Confidence: medium-high.

---

## Tension 1: The "thin downstream main()" is really a small plugin bundle

- **Locus A** ([[interim-report-extension-mechanism]]) commits: an out-of-tree EndpointDiscovery plugin in a thin
  downstream build is lower-cost than a fork, and its risks are handled by two mitigations.
- **Locus B** ([[interim-report-availability-and-lifecycle-off-k8s]]) and **Locus C**
  ([[interim-report-lost-k8s-policy-inputs]]) commit: off-Kubernetes HA needs either Kubernetes for the EPP control plane
  or a custom coordination primitive, and per-request priority needs a custom Admitter/DataProducer plugin (Alpha tier,
  `--allow-experimental-plugins`). **Locus D** ([[interim-report-custom-discovery-plugin-design]]) adds that the watch
  tier has no in-repo precedent at all.
- **The cross-locus dynamic:** complication, not contradiction. Locus A costed ONE plugin; the other loci each add a
  potential second or third custom Go component. The custom surface can grow from a single discovery plugin to a bundle
  (discovery + priority + possibly coordination), which multiplies interface-drift exposure against a framework
  that reshaped its discovery config three times in four months and has no panic isolation. The out-of-tree-over-fork
  argument survives (a fork carries the same bundle plus rebase burden), but "thin" and "low-risk" do not.
- **How the draft should engage this:** The recommendation section must present the custom code as a staged bundle with
  an explicit budget per component, argue out-of-tree beats fork *at bundle scale*, and make "keep the bundle to one
  plugin" the design goal (e.g. run the EPP control plane on Kubernetes to avoid writing coordination; defer the priority
  plugin until priority tiers are actually needed).
- **Calibration:** Locus A is medium on the ranking and names issue #2105 triage as a change condition; Locus B is high on
  Lease coupling. Both would move if upstream ships a non-Kubernetes coordination primitive or a supported out-of-tree
  plugin policy — the report should flag both as open, watch-worthy items.

## Tension 2: Topology is "organizational" — until you need EPP high availability

- **Locus A** ([[interim-report-topology-and-k8s-shims]]) commits: router-in-cluster vs router-out-of-cluster is an
  organizational decision (who owns the router's lifecycle); discovery works identically either way.
- **Locus B** ([[interim-report-availability-and-lifecycle-off-k8s]]) commits: every documented multi-replica EPP HA
  mechanism depends on Kubernetes (Lease, StatefulSet priority routing, active-active), so off-cluster you get a single
  EPP instance or you engineer coordination yourself.
- **The cross-locus dynamic:** conflict on the framing. Locus B's evidence turns Locus A's organizational choice into a
  technical one for any deployment with an availability requirement: the only documented way to run HA EPP for non-pod
  backends is the hybrid topology (EPP on Kubernetes, backends off it, discovery via file/ConfigMap or a custom plugin).
  Locus A's hybrid branch was its lowest-confidence claim (no case study); Locus B supplies the missing reason to prefer it.
- **How the draft should engage this:** Present a decision rule where HA is the first branch: need multi-replica router
  HA → hybrid (EPP on a small Kubernetes control plane, backends anywhere); single-replica/batch/RL/HPC-job scope →
  router fully outside Kubernetes (file discovery, Slurm/Ray-generated endpoints). Say explicitly that this reverses the
  topology locus's "organizational only" framing for production serving.
- **Calibration:** Locus B high (docs/operations.md quoted); Locus A medium on hybrid. Both name the same open question —
  whether anyone has run the hybrid shape in production — so the draft must flag it as undemonstrated.

## Tension 3: Independent paths converge — llm-d's routing value survives off Kubernetes, with named exceptions

- **Locus A** ([[interim-report-kv-event-path-off-k8s]]) commits: precise prefix routing is Kubernetes-independent at the
  code level. **Locus B** ([[interim-report-lost-k8s-policy-inputs]]) commits: FlowControl bands and fairness are static
  config/headers, not CRDs. **Locus C** ([[interim-report-extension-mechanism]]) commits: `Run()` already supplies the
  ext_proc server, config loader, draining and health wiring, so reuse is maximal above the discovery seam.
- **The cross-locus dynamic:** convergence from three independent code-level paths (the KV producer/extractor, the
  FlowControl config and header reference, the runner). This is the strongest affirmative finding in the corpus and it
  directly answers "use llm-d components as much as possible": essentially the entire scheduling stack, including the
  flagship precise scorer, is reusable unchanged. The exceptions are specific and small: data-parallel RankIndex,
  InferenceObjective-driven per-request priority, InferenceModelRewrite's weighted splits, and the version floor
  (≥ v0.9.0 for the #1403 notification fix that precise-prefix subscribers depend on).
- **How the draft should engage this:** State the reuse claim confidently (it is triangulated), enumerate the exceptions
  in a feature-parity table, and use it to answer the adversarial "why not vLLM Router/Dynamo" question: non-pod support is
  table stakes in those routers, but llm-d's plugin depth is what survives the move — at the price of the discovery seam.
  **One carve-out, added by the step-8 gap fetch and mandatory in the draft:** the P/D disaggregation sidecar is NOT reusable
  unchanged. Its SSRF allowlist requires a Kubernetes client watching a named InferencePool and its pods (`allowlist.go` at
  HEAD); there is no static/flag/CIDR alternative, it is off by default, and when off `IsAllowed()` returns true
  unconditionally while `disaggregatedPrefillHandler` proxies to whatever `x-prefiller-host-port` says — the exploit
  disclosed in issue #242 (Jul 2025). The upstream threat model puts the primary control in the gateway/EPP stripping that
  header on ingress (PR #244), with the allowlist only "an additional safety net"; and open issue #979 shows the allowlist
  matches host but not port even when enabled. So the draft should present this as a network-isolation requirement (the
  sidecar's port reachable only from the trusted gateway path, by firewall/security-group/mesh ACL) rather than a
  Kubernetes requirement, and recommend `--enable-tls=prefill` since sidecar TLS is plain flags and fully portable.
- **Calibration:** Locus A high; B medium-high; C medium-high technical. Shared open question: no end-to-end report of
  precise routing + file discovery exists (audited absence) — recommend a validation test in the roadmap rather than hedging
  the claim.

## Tension 4: Poll vs watch is the wrong axis — lifecycle correctness is the real requirement

- **Locus A** ([[interim-report-custom-discovery-plugin-design]]) commits: default to Tier-1 poll+diff; use a watch only
  when staleness has a named cost.
- **Locus B** ([[interim-report-kv-event-path-off-k8s]]) and **Locus C**
  ([[interim-report-availability-and-lifecycle-off-k8s]]) commit: off Kubernetes, a dead backend stays in the datastore —
  with its ZMQ subscriber and KV-index entries still live — until discovery calls Delete; readiness gating is gone; the
  datastore only propagates metadata changes on ≥ v0.9.0.
- **The cross-locus dynamic:** complication. Locus A frames staleness as a routing-latency cost to trade against watch
  complexity; Loci B and C show the dominant cost is correctness (routing to dead or unready workers, index pollution,
  subscriber leaks), and that it arises from missing health filtering far more than from poll interval. A Tier-1 poller
  that filters on health and deletes promptly beats a Tier-3 watch that forwards whatever the registry says.
- **How the draft should engage this:** In the plugin design section, make "only Upsert healthy endpoints, Delete on first
  failed health signal, pin router ≥ v0.9.0" the non-negotiable requirements, and choose poll/long-poll/watch by churn rate
  second. Name the unresolved question of repeated-Upsert idempotency on file reload.
- **Calibration:** Locus A high on facts, medium on estimates; Locus B high on mechanism but notes the dead-worker path is
  inferred from the lifecycle statement, not tested. Flag as a design requirement plus a test to write.

## Tension 5: The proxy choice silently decides the failure mode

- **Locus A** ([[interim-report-availability-and-lifecycle-off-k8s]]) commits: llm-d's own Envoy standalone path ships
  scoped fail-open (router.proxy.failOpen, lease-transition window); agentgateway's standalone passthrough is fail-closed
  with fail-open still open in #1661; client-supplied destination headers are discarded.
- **Locus B** ([[interim-report-extension-mechanism]]) and **Locus C** ([[interim-report-topology-and-k8s-shims]]) treat
  the proxy as a fixed, interchangeable data plane and focus on the EPP.
- **The cross-locus dynamic:** complication. Both supported non-pod data planes honor an EPP-chosen ip:port (Envoy
  ORIGINAL_DST + header; agentgateway passthrough), but they differ on behavior when the EPP is unavailable. And
  llm-d's scoped fail-open is tied to the Kubernetes lease window, which does not exist in the no-Kubernetes static
  envoy.yaml. Whether that config's ext_proc `failure_mode_allow` is set is an open check. For non-pod backends the
  proxy has no endpoint list of its own to fall back to, so fail-open requires deliberately configuring a fallback pool.
- **How the draft should engage this:** Treat proxy selection as a named decision in the strategy (not a footnote):
  recommend Envoy with the llm-d reference config for the no-Kubernetes path, state that the data path is fail-closed
  unless you configure a fallback cluster, and pair that with the HA decision from Tension 2.
- **Calibration:** Locus A high on the documented behaviors, medium on the synthesis; the no-k8s envoy.yaml
  failure_mode_allow default is an explicit open question — the draft must not claim fail-open off Kubernetes.

---

## Corrections and consistency notes for drafting
- PR #1007 (watch-based Kubernetes discovery under EndpointDiscovery) was closed unmerged (verified in
  [[interim-report-custom-discovery-plugin-design]]). [[interim-report-topology-and-k8s-shims]]'s committed position
  refers to "the merged PR #1007 implementation" — that phrase is wrong; its evidence stands on `pod_reconciler.go` at
  HEAD. Drafts must cite pod_reconcilergo, not #1007, for Kubernetes discovery mechanics.
- Distinguish `endpoint-notification-source` (generic; works off Kubernetes; drives the precise-prefix producer) from
  `k8s-notification-source` (inactive off Kubernetes).
- Config key — CORRECTED by the step-8 gap fetch, and the draft must state it this way: `dataLayer.discovery.endpoints.pluginRef`
  was merged by PR #2434 on 2026-08-25, **eight days after v0.10.0 was tagged (2026-08-17), and v0.10.0 is still the newest
  release as of 2026-09-11** — so that key exists only on unreleased `main`. A deployment pinned to v0.10.0 must use the
  legacy `dataLayer.discovery.pluginRef`; the nested form applies to `main` builds and to whatever release follows v0.10.0
  (the migration shim keeps the legacy key working there).
- Version floor across loci: llm-d-router ≥ v0.9.0 (FileDiscovery shipped in v0.9.0, 2026-06-23; the #1403/#1404
  endpoint-metadata notification fix that precise-prefix subscribers depend on also landed for v0.9.0).
- Peer discovery (step-8 gap fetch): v0.10.0 ships `dataLayer.peerDiscovery.pluginRef` (PR #2341, merged 2026-08-16), but the
  only peer-discovery plugin is Kubernetes Pod-watch and is still an OPEN, unmerged, Alpha PR (#2501) — so off-cluster peer
  discovery would be a second custom plugin. This sharpens Tension 1 (the bundle) and bears directly on Tension 2/3's HA claim.
- Audited absence confirmed at PR level: zero Consul/etcd/Slurm/Ray discovery PRs exist — every discovery PR targets flat files
  or Kubernetes primitives. A dynamic non-Kubernetes discovery plugin is greenfield work, with no upstream branch to track.
- Tension 2/3's HA claim is STRENGTHENED, not overturned — the draft may now assert it rather than hedge it. The adversarial fetch
  of issue #1290 (which operations.md cites for "replica state sync and scaling limitations") found a QPS/latency benchmarking
  issue that never claims the precise-prefix index is partitioned and never requires peer coordination for Active-Active. Its data
  runs the other way: precise-prefix Active-Active failures at 500 QPS fall as replicas rise (1r broken/3,148 → 2r 57 → 3r 31 →
  4r 18) with TTFT P50 165ms → 30ms. `cross_replica_syncer.go` shows an opt-in CrossReplicaSyncer surface that the precise-prefix
  extractor deliberately does not implement — its consistency comes from independent KV-event subscription. Two cautions to carry
  into the draft: the standalone Helm chart auto-injects `--ha-enable-leader-election` when replicas > 1 (silently forcing
  Active-Passive unless disabled), and flow control remains per replica. One genuine open gap: cross-replica SESSION AFFINITY was
  never evaluated (#1290 used independent synthetic requests), so session-pinned workloads must not assume Active-Active correctness.
- RankIndex (the one exception to the precise-routing reuse claim) is now precisely characterised: `EndpointMetadata.RankIndex` is
  positional — "this endpoint's position in the pool's TargetPorts... the pod-local rank in multi-port deployments" — not
  label-derived, and the file-discovery entry schema has no rank field at all (`plugin.go` contains no occurrence of "rank"), so
  every file-discovered endpoint keeps RankIndex 0. A custom discovery plugin can set it directly. Corroborating first-party
  signal that non-Kubernetes discovery is a recognised concept: `EndpointMetadata.NodeAddress` is documented as "Empty for
  non-Kubernetes discovery sources (e.g. file discovery)", and `EndpointDiscovery.Ready()` names "file load for FileDiscovery"
  alongside Kubernetes list+watch.
- Tension 1 refined by the step-8 gap fetch — two documents were being conflated, and the distinction matters to the draft's
  central recommendation: `create_new_filter.md`'s "edit runner.go" instruction is the tutorial for SCHEDULING FILTERS, while the
  merged `docs/discovery.md` has an official "Writing a custom discovery plugin" section teaching exactly the
  `fwkplugin.Register(PluginType, Factory)`-before-`Run()` pattern. Registering a custom DISCOVERY plugin is therefore documented
  upstream, not merely tolerated; only the packaging choice (your own downstream `main()` instead of editing the repo) is an
  inference from `cmd/epp/main.go`. Draft accordingly: assert the mechanism, hedge only the packaging.
- Nothing structurally blocks out-of-tree import (verified): `cmd/epp/runner` and `pkg/epp/framework/interface/{plugin,datalayer}`
  are not under any `internal/` path and go.mod carries no replace directives; the costs are a `go 1.26.6` floor and pulling the
  controller-runtime dependency tree unconditionally. Precedent absence is now properly established rather than assumed —
  pkg.go.dev reports "No known importers" for all three packages as of 2026-09-11 (GitHub code search hit a sign-in wall, which is
  a tool limitation, not evidence). The user would be the first; issue #2105 (panic isolation) is one of three competing proposals,
  still needs-triage with zero comments, so "at your own risk" is not ratified policy either.
