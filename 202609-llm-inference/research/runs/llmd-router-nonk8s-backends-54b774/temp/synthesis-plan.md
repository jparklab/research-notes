# Synthesis plan

## Core thesis (1-2 sentences)

llm-d's routing intelligence is genuinely platform-neutral and its Kubernetes dependence is concentrated in one place
— endpoint discovery — so the strategy is to customize exactly that seam (`EndpointDiscovery`) and reuse the entire
scheduling stack, KV-cache indexer, proxy contract and disaggregation machinery unchanged. What is *not* portable is
llm-d's operations layer (leader-elected HA, Helm packaging, CRD-driven policy inputs, the P/D sidecar's SSRF
allowlist), which is why where the router runs is a second, separate decision driven by the availability requirement
rather than by the backend estate.

## The strongest argumentative beats

1. **The seam is real and upstream-sanctioned** — Draft A. `EndpointDiscovery` (Start/Ready + Upsert/Delete notifier)
   is a shipped plugin interface with a documented custom-plugin procedure; when a discovery plugin is configured the
   endpoint picker starts no controller manager and needs no API server, CRD or RBAC. This is the whole argument for
   "customize one thing".
2. **The flagship feature survives the move** — Draft A/C. Precise prefix-cache routing is driven by the generic
   endpoint-notification path and keys on `Address:Port`, not Kubernetes identity, so it works with file or custom
   discovery today — with the wiring caveat (`prefixMatchInfoProducerName`) and the rank-index limit for
   data-parallel engines.
3. **Availability, not the backend estate, decides topology** — Draft C, confirmed by Draft B's evidence. All
   documented multi-replica HA modes are Kubernetes-coupled (Lease leader election), but #1290's measurements show
   Active-Active with *precise* prefix routing improving as replicas rise. So: single instance off-cluster; Kubernetes
   control plane if you want leader-elected HA; Active-Active behind an L4 load balancer with leader election
   explicitly disabled if Kubernetes is unavailable.
4. **You would be the first** — Draft B. Zero importers on the package index, nothing in CONTRIBUTING.md, issue #2105
   untriaged, no panic recovery on any plugin path. Concede it, price it, mitigate it — do not wave it away.
5. **The P/D sidecar is the one component you cannot reuse unchanged** — Draft B. Its SSRF allowlist requires an
   InferencePool, is off by default, and ignores port even when enabled (#979). Network isolation plus prefill TLS.
6. **The bet is reversible** — Draft C. The endpoint-picker protocol is the industry seam (NVIDIA Dynamo ships its own
   picker behind it), so choosing llm-d costs one plugin if you later switch, not the architecture.
7. **Docs trail code, and it matters practically** — all three. The nested discovery key ships in no release; the
   published plugin skeleton does not compile; the README never mentions discovery. Pin a tag, read the code, run CI
   against `main`.

## Section structure

The eight literal H2 headings from `required_section_headings`, in order:
1. llm-d Component Inventory and Kubernetes Coupling
2. How the llm-d Router Discovers and Tracks Backends Today
3. Extension Seams in the Router for Non-Pod Backends
4. Upstream State: Proposals and Work on Non-Pod and External Endpoints
5. Non-Pod Backend Types and What Each Requires from the Router
6. Candidate Strategies Compared
7. Recommended Strategy and Implementation Roadmap
8. Risks, Trade-offs, and Open Questions

Open with a short thesis statement before section 1 (no "Executive summary" heading is required, but the thesis must
be stated in the first ~150 words). Close section 8 with the committed synthesis and the open questions.

## Per-section commitments

### 1. Component inventory and Kubernetes coupling
- Evidence: Draft C's component table (reuse verdict per component), Draft A's v0.7 rename and unmodified-engine point.
- Beat: beat 1 framing — separate the routing layer (portable) from the operations layer (Kubernetes-first).
- Tension engaged: "platform-invariant vs lost inputs and lifecycle" — state both halves here, resolve in 5 and 8.

### 2. How the router discovers and tracks backends today
- Evidence: pod reconciler watching Pods by label; `EndpointMetadata` shape; the ext_proc contract and
  `x-gateway-destination-endpoint`; ORIGINAL_DST honoring an arbitrary ip:port.
- Beat: the pod assumption lives in discovery and in identity types, nowhere else — this sets up section 3.

### 3. Extension seams
- Evidence: the interface, the file plugin as template, the registry and runner, `docs/discovery.md`'s procedure,
  the multicluster variant (hostnames, separate metrics address).
- Beat: beats 1 and 4 together — mechanism asserted, first-adopter status conceded.
- Tension engaged: out-of-tree vs in-tree (the filter-tutorial/discovery-doc scoping distinction is the resolution).

### 4. Upstream state
- Evidence: the merged chain (#908, #1080/#1081, #1135, #1154, #1218), #1007 closed unmerged, PR #2434's key
  consolidation vs the v0.10.0 tag date, peer-discovery infrastructure shipped but its only plugin still open, zero
  Consul/etcd/Slurm/Ray discovery PRs.
- Beat: beat 7 — the version/config-key correction, stated as a table the reader can act on.

### 5. Non-pod backend types and requirements
- Evidence: the model-server contract (OpenAI HTTP + Prometheus gauges, no Kubernetes objects); KV events over ZMQ
  keyed `kv@ip:port@model`; NIXL/RDMA for P/D; what each backend class (bare metal, Slurm, Ray, VMs, remote clusters,
  managed APIs) must supply.
- Beat: beat 2 — the flagship survives; name the two exceptions (rank index; managed APIs belong at the gateway tier).

### 6. Candidate strategies compared
- Evidence: the six-way comparison — file+generator, out-of-tree plugin, in-tree fork, library embedding, Kubernetes
  shims (EndpointSlice dead end, virtual-kubelet caveat, Slinky), different router entirely.
- Beat: beat 6 — reversibility; and dispatch the shims with the pod-reconciler fact rather than hand-waving.
- Must end in a ranked verdict, not a flat table.

### 7. Recommended strategy and roadmap
- Structure: decision rules first (availability → topology; churn/addressing → discovery rung; escalation gates for
  the plugin), then a phased roadmap with effort numbers (poll+diff 2-4 days; long-poll +1-2 days; watch 1-2 weeks;
  1-3 weeks end-to-end for the plugin phase including health gating, tests, CI).
- Standing obligations stated here, not deferred: health-gated Upsert/Delete, version pin, proxy failure mode chosen
  deliberately, network isolation plus `--enable-tls=prefill` wherever P/D is used.
- Tensions engaged: topology-vs-HA, poll-vs-watch subordinated to health gating.

### 8. Risks, trade-offs, open questions
- Evidence: first-adopter risk and #2105; interface churn (three config shapes in four months; datalayer reworked on
  2026-09-11); the sidecar carve-out with #242 vs #979 dated separately; per-replica flow control; session affinity
  never evaluated across replicas; audited absences (no end-to-end precise+file demonstration; no non-file discovery
  plugin anywhere).
- Beat: beats 4, 5 and 7 land here; end with the committed synthesis.

## Where drafts disagreed

See `synthesis-conflicts.md` for the four divergences and verdicts. In short: adopt Draft C's decision-procedure
shape, populate it with Draft A's phases and Draft B's gates; assert the out-of-tree mechanism *and* concede the
precedent gap; use the tiered effort numbers; promote the P/D sidecar carve-out into the recommendation.

## Length target

- response_format: `argumentative` (5000-10000 words)
- Pass 1 target: ~8000 words
- Pass 2 final target: **7000-7500 words** (hard ceiling 10000; aim for the middle so the ship gate passes)
- citation_style: `wikilink` — `[[note-id]]` markers, no Sources section, source notes only (never interim/comparisons).
