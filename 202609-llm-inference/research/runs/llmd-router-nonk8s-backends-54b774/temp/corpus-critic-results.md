# Step 8 — corpus critic results (targeted gap fill)

The corpus critic read comparisons.md, loci.json, source-tensions.json, consensus-claims.json and all six interim
notes, then named 5 gaps against the draft's emerging recommendation. Four fetchers ran in parallel. Outcome summary:
**one recommendation was narrowed (the P/D sidecar is not reusable unchanged), one was factually corrected (the version
/ config-key mapping), two were strengthened from inference to evidence (Active-Active HA off Kubernetes; out-of-tree
plugin registration), and the single known exception to the reuse claim (RankIndex) was pinned down precisely.**

---

## Gap 1 (CRITICAL, overturning) — does issue #1290 break the Active-Active HA claim?

- **Target position:** off-Kubernetes multi-replica EPP HA is achievable as Active-Active behind a plain L4 load
  balancer provided *precise* (not approximate) prefix routing is used, because each replica independently subscribes
  to every endpoint's KV-event stream.
- **Searched/fetched:** llm-d-router issue #1290 (cited by docs/operations.md at exactly the Active-Active warning);
  docs/peer-discovery.md (already in vault); plus, found while drilling the datalayer package listing,
  `cross_replica_syncer.go` and `peer.go`.
- **Found:** #1290 is a QPS/latency **benchmarking** issue (opened 2026-05-24, closed 2026-07-21). It never uses the
  phrase "replica state sync", never says the precise-prefix index is partitioned, and never names peer coordination as
  a correctness requirement for Active-Active — so operations.md's citation of it oversells what it contains. Its
  measurements support the draft: precise-prefix Active-Active failures at 500 QPS fall as replicas rise (1r
  broken/3,148 → 2r 57 → 3r 31 → 4r 18) with TTFT P50 165 ms → 30 ms. `cross_replica_syncer.go` defines a separate,
  opt-in `CrossReplicaSyncer`/`CrossReplicaContributor` surface; the precise-prefix extractor implements only
  `EndpointExtractor`, so its cross-replica consistency is by-design independent subscription. `peer.go`'s doc comment
  names file-based peer discovery as a first-class concept ("the k8s reconciler and any file-based discovery feed the
  same sink"), though no built-in implementation ships.
- **Effect:** position **strengthened — upgraded from inference to evidence-backed**; the draft may assert it. Two
  cautions added: the standalone Helm chart auto-injects `--ha-enable-leader-election` when replicas > 1 (silently
  forcing Active-Passive), and flow control remains per replica. One new open gap recorded: cross-replica **session
  affinity** was never evaluated (#1290 used independent synthetic requests), so session-pinned workloads must not
  assume Active-Active correctness.

## Gap 2 (HIGH, overturning) — is out-of-tree plugin registration blocked or forbidden?

- **Target position:** a custom EndpointDiscovery plugin registered from a downstream `main()` importing
  `cmd/epp/runner` beats an in-tree fork.
- **Searched/fetched:** pkg.go.dev "Imported By" for `cmd/epp/runner`, `pkg/epp/framework/interface/plugin` and
  `.../datalayer`; GitHub code search for `llm-d-router/cmd/epp/runner` and `fwkplugin.Register`; go.mod at HEAD; a full
  GitHub Trees listing (1,819 paths) to test for `internal/` placement; issue #2105's current state.
- **Found:** no structural blocker — the runner and both interface packages are **not** under any `internal/` path and
  go.mod carries no replace directives (costs: `go 1.26.6` floor, unconditional controller-runtime tree). Precedent
  absence is now properly established rather than assumed: all three packages report "No known importers" on pkg.go.dev
  as of 2026-09-11 (verified with a second fetch provider and a raw request). GitHub code search hit a sign-in wall —
  recorded as a tool limitation, not as evidence. Refinement: the "only documented path is in-tree" claim conflated two
  documents — `create_new_filter.md` is the tutorial for **scheduling filters**, while merged `docs/discovery.md`
  officially documents "Writing a custom discovery plugin" with the `fwkplugin.Register(...)`-before-`Run()` pattern.
  Issue #2105 is one of three competing proposals, still needs-triage with zero comments.
- **Effect:** position **strengthened and refined**. The draft asserts the mechanism (documented upstream for discovery
  plugins) and hedges only the packaging choice (downstream `main()`, inferred from `cmd/epp/main.go`). "At your own
  risk" is not ratified policy; "zero precedent" stands, verified.

## Gap 3 (HIGH, overturning) — is the P/D sidecar reusable unchanged off Kubernetes?

- **Target position:** reuse the whole stack unchanged, including the P/D disaggregation sidecar.
- **Searched/fetched:** the real source at HEAD (the path is `cmd/pd-sidecar`, not `cmd/pd_sidecar`) — `allowlist.go`,
  `options.go`, `tls.go`, `chat_completions.go`, `proxy.go`, `main.go`, `resolver.go` and others; plus issue #242 (the
  original SSRF disclosure, Jul 2025), PR #244 (the primary fix), PR #1647 (the current allowlist), and open issue #979.
  The llm-d.ai routing-sidecar docs page failed again and is recorded as unfetchable.
- **Found — this gap's thesis is CONFIRMED:** `NewAllowlistValidator` requires a Kubernetes client and watches one named
  InferencePool CRD plus its selected pods through a SharedInformer; there is no alternate code path and no static file,
  flag-list or CIDR option. The only flags are `--enable-ssrf-protection` (default **false**) and `--inference-pool`.
  When disabled, `IsAllowed()` returns true unconditionally ("backward compatibility", per the code comment), and
  `disaggregatedPrefillHandler` reads `x-prefiller-host-port` straight off the client request and reverse-proxies there —
  exactly the exploit disclosed in #242 with a working PoC. Two qualifiers matter: the maintainers' threat model treats
  the EPP/gateway stripping that header on ingress (PR #244) as the primary control and the allowlist as "an additional
  safety net"; and even enabled, open issue #979 shows the allowlist matches host/IP but **not port**. TLS for the
  prefill hop is fully portable and Kubernetes-independent (`--enable-tls=prefill,decode,encode`,
  `--tls-insecure-skip-verify`, `--tls-min-version`, `--tls-cipher-suites`, `--cert-path`; standard crypto/tls, TLS 1.2
  floor, real verification unless explicitly skipped).
- **Effect:** the "reuse everything unchanged" claim is **narrowed** — the P/D sidecar is the one component that loses a
  security control off Kubernetes. The draft must (a) carve the sidecar out of the blanket reuse claim, (b) state the
  mitigation as a network-isolation requirement (the sidecar's port reachable only from the trusted gateway/EPP path,
  enforced by firewall, security group or mesh ACL — NetworkPolicy is just one implementation), (c) recommend
  `--enable-tls=prefill` regardless since it is orthogonal and free, and (d) date and attribute #242 (historical,
  partially fixed) and #979 (open, unresolved) separately rather than conflating them.

## Gap 4 (HIGH, overturning) — has v0.10 reshaped the discovery config surface again?

- **Target position:** version floor llm-d-router ≥ v0.9.0 with `dataLayer.discovery.endpoints.pluginRef` (PR #2434).
- **Searched/fetched:** llm-d-router releases, tags, milestones and issue/PR searches for `dataLayer`/discovery; the
  llm-d meta-repo releases and blog index; PRs #2341, #2434, #2501.
- **Found — the draft was factually wrong:** v0.10.0 was tagged 2026-08-17 and is still the newest release as of
  2026-09-11; PR #2434 merged 2026-08-25, **eight days later**, so `dataLayer.discovery.endpoints.pluginRef` exists only
  on unreleased `main`. A deployment pinned to v0.10.0 must use the legacy `dataLayer.discovery.pluginRef`. FileDiscovery
  shipped in v0.9.0 (2026-06-23). v0.10.0 does include `dataLayer.peerDiscovery.pluginRef` (PR #2341, merged 2026-08-16),
  but the only peer-discovery plugin (PR #2501, Kubernetes Pod-watch, Alpha) is still open and unmerged. No fourth
  reshaping is proposed (zero open `dataLayer.discovery` issues; the v0.11 milestone, due 2026-09-30, has none).
- **Effect:** recommendation **corrected** in comparisons.md and source-tensions.json — the draft must state both keys
  with their version applicability. Also confirmed at PR level: zero Consul/etcd/Slurm/Ray discovery PRs exist, so a
  dynamic non-Kubernetes discovery plugin is greenfield work (strengthens the effort estimate and the "you will be
  first" framing), and off-cluster peer discovery would be a second custom plugin.

## Gap 5 (MEDIUM, independent-verification) — does RankIndex have any carrier in file/custom discovery?

- **Searched/fetched:** `endpoint_metadata.go`, `discovery.go`, the built-in file-discovery package README and
  `plugin.go`; a GitHub code search for RankIndex (auth wall).
- **Found:** `EndpointMetadata.RankIndex` is **positional**, documented as "this endpoint's position in the pool's
  TargetPorts, identifying the pod-local rank in multi-port deployments" — not label-derived. The file-discovery entry
  schema is exactly `{name, namespace, address, port, labels}`; `plugin.go` contains no occurrence of "rank", so every
  file-discovered endpoint keeps RankIndex 0. No label convention populates it anywhere. A custom plugin can set it
  directly. Corroborating first-party signals that non-Kubernetes discovery is a recognised shape:
  `EndpointMetadata.NodeAddress` is "Empty for non-Kubernetes discovery sources (e.g. file discovery)", and
  `EndpointDiscovery.Ready()` names "file load for FileDiscovery" beside Kubernetes list+watch. A separate
  `multicluster-file-discovery` variant accepts RFC-1123 hostnames and `metricsAddress`/`metricsPort` labels.
- **Effect:** the one exception to the flagship-reuse claim is **pinned down precisely** rather than left vague — it is
  a multi-port/per-rank ZMQ addressing limit of the file schema, not a limit of the EPP, and it is fixable in a custom
  plugin.

---

## Adversarial-search verdict

Of five gaps, the two aimed squarely at the draft's most load-bearing claims (Active-Active HA off Kubernetes;
out-of-tree plugin registration) returned **no substantive challenge** — both positions gained confidence, one on
empirical benchmark data (#1290) and one on a merged upstream doc (docs/discovery.md). One attack **landed**: gap 3
showed the P/D disaggregation sidecar keeps a Kubernetes-coupled SSRF allowlist, so the blanket "reuse unchanged" claim
is now explicitly carved out and paired with a network-isolation requirement. One factual correction was made (the
`discovery.endpoints` key ships in no release yet), and the RankIndex exception was characterised exactly. Confidence
changes are recorded inline in comparisons.md and in source-tensions.json; the pre-draft direction survives with a
sharper security section and a corrected version table, which is precisely what this step exists to produce.
