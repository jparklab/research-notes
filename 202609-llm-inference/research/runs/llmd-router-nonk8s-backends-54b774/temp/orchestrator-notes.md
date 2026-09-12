# Orchestrator notes — llmd-router-nonk8s-backends-54b774

## 2026-09-11 ~19:50Z — wave 1 dispatched (11 fetchers, ~100 URLs)

### Emerging thesis (pre-evidence, to be tested)
The question's premise ("customize the router so it supports non-pod backends") is partly overtaken by upstream:
llm-d v0.8 (mid-2026) formalized an `EndpointDiscovery` plugin interface in the EPP with two built-ins
(Kube discovery over InferencePool; FileDiscovery over a YAML endpoints file). The EPP core code has also moved
out of GIE into llm-d/llm-d-router, which means the discovery abstraction is now owned by llm-d rather than the
k8s-sigs project. So the strategy is likely: DON'T fork — write a custom EndpointDiscovery plugin (or drive
FileDiscovery from an external controller that writes the file) and keep scheduling plugins, KV indexer,
disagg sidecar, IPP, and proxy integration unmodified.

### Hypotheses to test / what could overturn it
1. Is EndpointDiscovery a real compile-time Go plugin interface (registerable from an out-of-tree build), or is it
   internal and only file/kube are selectable? If internal-only → "write your own plugin" means a thin fork / custom
   main.go build. Need source code evidence (batch 1).
2. Do OTHER components still assume pods even when discovery is file-based?
   - KV-cache indexer: endpoint identity keys (pod name vs address) and ZMQ subscription model (batch 5).
   - Metrics scraping: needs reachable /metrics on each endpoint — Model Server Protocol (batch 4).
   - P/D sidecar SSRF allowlist watches InferencePool → outside k8s must be disabled or fed otherwise (batch 6).
   - Proxy: Envoy ORIGINAL_DST / agentgateway passthrough must trust EPP-returned ip:port (batch 7).
   - InferenceObjective / flow control CRDs → unavailable without k8s API? (batch 2/3)
3. Is the "file" mechanism production-grade (hot reload, health, churn)? v0.9 HA replicas + file sync?
4. For external managed APIs (OpenAI etc.), the EPP is the wrong layer: they have no vLLM metrics/KV events; belongs
   at gateway/IPP layer (batch 8). This is a likely dialectical point: "backends other than pods" splits into
   (a) self-hosted engines on non-k8s infra → EndpointDiscovery; (b) opaque managed endpoints → gateway-level
   routing beside the EPP.
5. Competing routers (vLLM Router Rust, Dynamo) claim better perf; if user's goal is only non-pod support, why
   stay with llm-d? Answer hinges on plugin ecosystem breadth (precise prefix, predicted latency, flow control, P/D).

### 19:45Z checkpoint
3 notes landed (batch 8: OpenShift external+self-hosted routing, Agent Router API ref, Agent Router issue #423 —
note: Envoy AI Gateway appears renamed "Agent Router"; record that rename in the corpus). Escalation queue empty.
Anticipated thin items for wave 2 (decide after coverage check, don't pre-fetch): concrete non-file discovery
implementations (Consul/etcd/cloud SD), KV-indexer endpoint identity outside k8s, SGLang/TRT-LLM engines on
non-k8s hosts, and security/mTLS for EPP->backend metrics scraping across network boundaries.

### 19:48Z — first notes read
- Envoy AI Gateway now "Agent Router" (Agentic AI Foundation project; same code/maintainers). AIGatewayRoute generates
  HTTPRoute + per-backend resources; supports external providers via AIServiceBackend/BackendSecurityPolicy — i.e., the
  managed-provider lane lives at the gateway layer BESIDE the EPP, consistent with hypothesis 4.
- Agent Router issue #423 (Feb 2025, closed): translate GIE CRDs into Envoy config → InferencePool as one backend kind
  among AI backends. This is the composition seam for "external API + self-hosted non-pod pool" in one front door.
- Red Hat OpenShift AI article (May 2026): centralized routing for external + self-hosted LLMs — read full body at
  coverage-check time for the concrete wiring.
Monitor armed on vault count (milestones of 20, exits at 80).

### 20:45Z — rate-limit recovery
- All 11 wave-1 fetchers hit an account session limit (HTTP 429 on subagent model; "resets 8:30pm ET") mid-batch.
- Recovery audit: 89 notes actually landed (fetchers used inconsistent tags) → retagged all 89 with the run tag.
  84/100 queued URLs fetched; 3 more superseded by equivalent GitHub copies; 13 truly missing → fetching directly via
  CLI in background (no model needed). 2 escalations queued for browser lane.
- 19 notes lacked summaries, only 33 claims files → resumed all 11 fetchers via SendMessage to finish summaries/claims.
- If resumed agents fail again on 429: finish summaries myself (orchestrator) for the thin set and proceed; claims
  coverage will be partial → step 2.6 redundancy audit runs on what exists.
- Batch 9 fetcher flagged adversarial finding: named-port mismatch between Service and manual EndpointSlice silently
  breaks kube-proxy routing (k8s issue #123954) → evidence against the "fake it as k8s objects" strategy family.
- Batch 11: Mooncake as academic anchor (KV-centric disaggregation).

### 21:00Z — subagents back; batch 4 + 7 reports
- Rate limit lifted (resumed agents completing). 94 tagged notes, 5 escalations queued (IPP repo, vLLM kv_events cfg,
  AIBrix, llm-d #1154, production-stack GIE) → browser-lane at step 2.8.
- Batch 4: Model Server Protocol (003) is PURE HTTP + Prometheus (OpenAI APIs; queue/running/KV-util gauges; LoRA
  metrics) — no k8s objects → the backend-side contract is portable. InferencePool selector = Pods only;
  endpointPickerRef OPTIONAL since GIE v1.5.0. Azure AGC verbatim: managed-provider endpoints "outside the scope".
  api-overview: InferencePoolImport (alpha, multi-cluster) is the only documented beyond-local-pods seam in GIE.
- Batch 7: Istio / NGINX GF / kgateway = InferencePool-only (no standalone). agentgateway is the ONLY proxy with a
  standalone EPP mode (PR #1429 config, PR #1658 passthrough trust); FAIL-CLOSED by design (#1661 open, contested).
  Envoy header-override path: "Override Host LB policy" (Envoy PR #38757 / GIE PR #761) abandoned by kgateway because it
  dropped its Envoy data plane in v2.1 → need to verify how llm-d's no-k8s guide wires Envoy (ORIGINAL_DST + header?).
  Istio/NGF default insecureSkipVerify to the EPP → security caveat.
- Thesis sharpening: "use llm-d as much as possible" is achievable end-to-end only with (EPP w/ custom
  EndpointDiscovery) + (agentgateway standalone OR Envoy configured for dynamic original-dst) — the proxy choice is
  the second decision the report must make, not an afterthought. Gateway-mode proxies (Istio/NGF/kgateway) cannot
  target non-pod endpoints at all except via k8s shims.
- Dialectical candidate locus: fail-closed vs fail-open for non-pod backends (EPP becomes a hard dependency of the
  data path once the proxy has no local endpoint list to fall back to).

### 21:05Z — batch 8 report
- Two concrete "gateway picks external-vs-self-hosted, EPP picks within pool" implementations:
  (1) agentgateway on AKS: backend types `ai:` (Azure OpenAI) vs `service:` + `inferenceRouting` policy calling EPP over
  ext-proc — no Gateway API Gateway object needed; (2) Agent Router AIGatewayRoute.BackendRefs → AIServiceBackend OR
  InferencePool, never both in one rule; InferencePool owns fallback.
- Agent Router issue #902: plain k8s Service backends DISABLED (Envoy Gateway EDS/CDS split), unresolved through mid-2026
  → Envoy-AI-Gateway lane is weak for arbitrary non-pod backends. Strengthens "agentgateway standalone" as proxy choice.
- api-reference-agent-router is 18k words — candidate for source-analyst? Probably NOT load-bearing enough (CRD ref);
  skip unless step 4 needs it.

### 21:15Z — no-k8s blog read in full; batch 10; browser lane
- EndpointDiscovery (pkg/epp/framework/interface/datalayer/discovery.go): `Start(ctx, DiscoveryNotifier) error`,
  `Ready() <-chan struct{}`; notifier `Upsert(*EndpointMetadata)`, `Delete(types.NamespacedName)` ← k8s type residue in
  the "platform-neutral" API. Selected by `dataLayer.discovery.pluginRef: file-discovery` in EndpointPickerConfig.
- File plugin: name + literal address:port (NO hostname resolution), labels surfaced to scheduler (llm-d.ai/role for
  P/D, roles in bylabel/roles.go), `watchFile: true` via fsnotify; atomic-rename rewrite pattern.
- Blog's own spectrum: static file → generated at startup → regenerated on change → orchestrator-native plugin
  (SlurmDiscovery/RayDiscovery = FUTURE WORK); Consul/etcd/cloud-SD plugins "exploring"; Kube watch moving behind the
  interface = "longer term". → As of Sep 2026 the ONLY shipped non-k8s plugin is file.
- Two stated gaps outside k8s: (1) InferenceObjective priority + InferenceModelRewrite still CRD-configured → static
  config or unavailable; (2) endpoint LIFECYCLE ownership shifts to the user (health agent must drop dead workers).
  → Direct input to Risks section and to the "custom plugin vs file-generator sidecar" dialectic.
- Batch 10: vLLM Router / production-stack / Dynamo all ship static-or-file discovery natively (Dynamo also etcd/NATS
  for non-k8s) → non-pod support is table stakes, NOT an llm-d differentiator; llm-d's moat is plugin depth above
  discovery (precise prefix, predicted latency, flow control, P/D profiles). vLLM Router +25% claim: vendor-internal,
  unpublished harness, llm-d engineers assisted.
- Browser-lane: subagent lacks Chrome tools this session → 5 escalations remain queued (status quo). Retrying 3 via CLI.

### 21:25Z — batch 3 report (119 tagged notes)
- GIE interface/datalayer godoc: EndpointMetadata = "the relevant Kubernetes Pod state" {NamespacedName, PodName,
  Address, MetricsHost, Labels}; NotificationSource is k8s-GVK/watch-bound; PollingDataSource is generic (portable seam
  for metrics). Concrete EndpointPool/PoolInfo are selector/namespace-shaped; PoolInfo.PodList marked transitional.
  → A custom discovery plugin must SYNTHESIZE pod-shaped identity (fake namespace/name) for non-pod endpoints — this is
  the concrete "impedance" cost; verify llm-d-router's copy (EPP moved there) has the same EndpointMetadata shape.
- GIE standalone guide: experimental at v1.3.0 (2026-01-21), flag-only mode still needed a pod label selector; guide
  DELETED 2026-06-30 (PR #2978) with no replacement in GIE → standalone story now lives in llm-d-router charts + llm-d
  no-k8s guide. EPP move announced 2026-05-02 (GIE PR #2906). Protocol amendment 2026-07-23 (PR #3003): ordered
  endpoint lists per backend for retry/failover.
- 3 escalated URLs (IPP repo, llm-d #1154, vLLM kv_events cfg) still unfetchable → accepted gaps; IPP covered
  indirectly via search snippets + other notes; kv_events covered by kv_events.py source note.

### 21:35Z — primary code/docs confirmed
- discovery.go (llm-d-router main, fetched 2026-09-11): EndpointDiscovery embeds fwkplugin.Plugin; "registered in the
  plugin registry and selected via EndpointPickerConfig.discovery.pluginRef" (blog says dataLayer.discovery.pluginRef →
  verify in guide config.yaml). Contract: Start SHOULD Upsert all known endpoints before watch loop; Ready closes once
  after initial sync ("zero endpoints is a valid outcome"); DiscoveryNotifier NOT goroutine-safe — single goroutine,
  strict Upsert/Delete ordering or endpoints leak. Helper NewDiscoveryNotifier(DiscoveryEndpointStore).
- file plugin: address must parse as IP; port 1-65535; file >1 MiB aborts startup; invalid entries logged+skipped;
  entries missing on reload → Delete; re-attaches watcher after atomic rename / ConfigMap symlink swap.
- No-k8s data path (llm-d docs/infrastructure/no-kubernetes-deployment.md): client → Envoy :8081 → ext_proc gRPC :9002
  (EPP) → ORIGINAL_DST cluster reading x-gateway-destination-endpoint → worker ip:port. Envoy distroless v1.33.2,
  vLLM v0.19.1, EPP from ghcr.io/llm-d/llm-d-router-endpoint-picker or cmd/epp. Plugin mix = optimized baseline
  (prefix-cache-scorer w3; queue, kv-cache-utilization, no-hit-lru w2; max-score-picker). k8s liveness/readiness probes
  must be re-created by the user's runtime.
- => The PROXY question is solved upstream for plain Envoy (ORIGINAL_DST + header) — no custom proxy code needed.
  Open: out-of-tree plugin registration (fetching cmd/epp/main.go now).

### 21:45Z — batches 2, 5, 9, 11 reports
- KV indexer (llm-d-kv-cache, renamed from -manager): Static Endpoint Mode (discoverPods:false + zmqEndpoint) — indexer
  BINDS SUB, backends CONNECT OUT as PUB; zero k8s. Auto-Discovery mode = k8s pod reconciler (pods get/list/watch RBAC)
  dialing each pod's PUB. Topic kv@<ip>:<port>@<model>; identity = opaque string → MUST equal the EPP endpoint
  identity. Red Hat Oct-2025 article's "kv@pod-id@model" is stale → contradiction to record (resolved in favor of HEAD).
- EPP: compile-time registration in cmd/epp/runner/runner.go; Data Layer = 3 pairs (PollingDataSource+Extractor,
  NotificationSource+NotificationExtractor [only k8s-bound one], EndpointSource+EndpointExtractor); Auto-Wiring by
  interface reflection; plugin stability Alpha/Beta/Stable. llm-d.ai dataLayer docs lag the router repo's Aug 25 2026
  discovery-consolidation commit.
- plugins README: SHIPPING multicluster family (multicluster-file-discovery, -metrics-data-source, -metrics-extractor,
  scorers/filters) where candidate endpoints are CLUSTERS; its file format = pod file discovery but address MAY BE A
  HOSTNAME → precedent (and a contradiction with single-cluster file plugin's IP-only rule).
- k8s-shim family weakened: interLink out-of-scope "Long-running Services"; interlink-slurm-plugin networking is
  egress-only (SSH/tsocks via login node) → router can't dial virtual pods. VK PodIP is provider-defined. Selectorless
  Service + EndpointSlice irrelevant anyway (InferencePool selects Pods only). Slinky slurm-operator = inverse strategy
  (Slurm daemons as real pods → stock InferencePool works).
- KAITO: pinned upstream llm-d-router-gateway chart + EPP image via Flux = "wrap, don't fork" exemplar. KServe v0.17
  LLMInferenceService reuses EPP as-is. vllm-ascend RFC #9905: KV-event collector near router keyed by arbitrary
  instance_id.
- Dispatched: source-analyst on llm-d-router 12.8k-word repo note; wave2-C on runner.go/registry/plugin guide/discovery
  commit/Slinky/arXiv 2511.21413. Skipped source-analyst for Mooncake (background), WRP (tangential), Agent Router ref.
- EMERGING RECOMMENDATION SHAPE: (1) Envoy/agentgateway + stock EPP image + file-discovery fed by a small
  "endpoint-sync" controller for day 1 (zero Go code); (2) custom EndpointDiscovery plugin compiled into a thin
  downstream EPP build for dynamic fleets (Consul/Slurm/Ray/cloud SD) — fork-avoidance depends on runner.go hooks;
  (3) KV indexer in static-endpoint mode with identity = ip:port; (4) managed/external APIs at the gateway tier
  beside the EPP, never inside it; (5) contribute the plugin upstream to exit maintenance.

### 21:55Z — batch 6 report
- llm-d-router PR #908 (merged): pluggable EndpointDiscovery; maintainer: "We aim to also support non k8s in the future
  so we need to keep the datalayer clean: we want K8s, file, Slurm, and Ray discovery all as plugins." ctrl.Manager
  isolated to the k8s plugin. Unresolved: identity still k8s types.NamespacedName.
- FileDiscovery PR: YAML/JSON + fsnotify; IPv4-only; metrics scraped from SAME address:port as inference (no separate
  metrics port) → constraint for backends whose /metrics lives elsewhere (e.g. behind a sidecar or different port);
  hardened for ConfigMap rotation (dual-purpose in-cluster too).
- Config-key CONTRADICTION: discovery.go comment + PR #908 say `EndpointPickerConfig.discovery.pluginRef`; blog + file
  PR say `dataLayer.discovery.pluginRef`; Aug 25 2026 consolidation commit may have changed it → wave2-C resolving.
- v0.7 blog: Standalone Mode (Envoy, no GAIE) became DEFAULT; rename motivated by clash with k8s scheduling concepts;
  v0.8 roadmap: "Non-Kubernetes deployment mode ... through file-based service discovery."
- networking blog: NIXL P/D pairing is lazy/decentralized via ZMQ, "no centralized service-discovery plane required"
  → P/D portable off-k8s given RDMA reachability.
- AUDITED ABSENCE: docs/proposals/non-kubernetes-mode.md 404 on llm-d and llm-d-router main.
- Prompt-injection payload spotted in routing-sidecar README example body — inert, ignored.
- Unfetched leads: llm-d-router PR #1218 (file discovery wiring into runner), #1080/#1081 (DiscoveryBackendStore),
  llm-d PR #1618 (no-k8s guide) — pass to wave2-C via SendMessage if runner.go alone doesn't answer registration.

### 22:05Z — ground truth from code (maingo, configyaml, envoyyaml, plugingo)
- CONFIG KEY RESOLVED: shipped guide config uses `dataLayer: {injectDefaults: false, discovery: {pluginRef:
  file-discovery}, sources: [...]}`. discovery.go comment ("EndpointPickerConfig.discovery.pluginRef") is stale.
- main.go = ctrl.SetupSignalHandler() + runner.NewRunner().Run(ctx); built-ins auto-registered in
  runner.parsePluginsConfiguration(). → Out-of-tree path likely = own main() importing cmd/epp/runner + registering an
  extra factory before Run (confirm registry API via wave2-C). Still imports controller-runtime even off-k8s.
- Factory signature: Factory(name string, params *json.Decoder, h fwkplugin.Handle) (fwkplugin.Plugin, error).
- file plugin: IPv4 literal only ("matching the pod discovery contract"); validator INJECTED "so a variant can loosen
  it" (multicluster variant → hostnames); namespace defaults "default"; MetricsHost = address:port (same port).
- envoy.yaml: ORIGINAL_DST + CLUSTER_PROVIDED + use_http_header x-gateway-destination-endpoint; ext_proc
  FULL_DUPLEX_STREAMED; EPP gRPC health checks; TLS to EPP dropped for colocation — restore for split host.
- RISK to record: ORIGINAL_DST-by-header is an open-proxy/SSRF surface if a request can reach the route without the
  EPP overwriting the header (e.g. misconfigured failure_mode_allow) → mitigate by strip-on-ingress + network ACLs to
  worker CIDRs. Also ALL metrics scraping + ZMQ must traverse the network outside k8s NetworkPolicy/mTLS assumptions.

### 22:10Z — source-analyst (llm-d-router repo page, 12.8k words)
- README/landing capture has ZERO mentions of discovery/EndpointDiscovery/file discovery → docs lag code (PR #908,
  file plugin, no-k8s guide all exist). Record as a "documentation gap" tension, not a contradiction of capability.
- ATOM engine extractor (PR #2476, merged 2026-09-11) keyed by pod label llm-d.ai/engine-type → new engine support
  requires an in-repo PR (compiled-in registry). Off-k8s, the label can be supplied via file-discovery labels.
- Token-producer/render service already addressed by URL (VLLM_RENDER_URL) — dev harness only.
- PR #2620 "structured plugin config" (HEAD, 2026-09-11) → forwarded to wave2-C with PR #1218 (runner wiring).
- CRDs/RBAC/InferencePool/HTTPRoute remain hard deps of GATEWAY mode (PRs #1043/#1169/#1105) — consistent with the
  two-mode picture: Gateway Mode = k8s-bound; Standalone/no-k8s = discovery-plugin-bound.

### 22:20Z — batch 1 report (resumed; 16 notes, 97 claims)
- Canonical llm-d-router docs/discovery.md fetched — has a complete custom-discovery-plugin code skeleton (THE
  implementation anchor for the recommendation). PR #1007 (k8s + file discovery plugins shipped; 5.4k words).
- pkg.go.dev v0.10.0: discovery/file is the ONLY public discovery-plugin package.
- Helm charts: ZERO discovery/dataLayer knobs (k8s-only via pod labels) → audited absence; non-k8s = hand-written
  config, not chart values.
- llm-d-rl north-star proposal superseded/abandoned (experimental/ → legacy/).
- CONFIG-PATH CONTRADICTION now 3-4 way: blog `dataLayer.discovery.pluginRef`; guide config.yaml (read by me, HEAD)
  `dataLayer.discovery.pluginRef`; discovery.go comment `EndpointPickerConfig.discovery.pluginRef`; batch 1 says
  PR #1007 review + docs/discovery.md → `dataLayer.discovery.endpoints.pluginRef`. Reading discovery.md now; wave2-C
  checking the Aug 25 consolidation commit. Report must state the key with its source+date and advise pinning a release.

### 22:25Z — docs/discovery.md read (llm-d-router, last commit 2026-09-02)
- Registration: `fwkplugin.Register(myplugin.PluginType, myplugin.Factory)` before `Run()` → out-of-tree custom main
  importing the runner is the documented path (confirming exact wording from the "Writing a custom discovery plugin"
  section now).
- When dataLayer.discovery is set: EPP does NOT call ctrl.GetConfig(), no controller manager, no InferencePool CRD, no
  RBAC; scheduling/request-control/metrics unchanged; absent → k8s discovery (backward compatible).
- EndpointMetadata.MetricsHost defaults to address:port but a CUSTOM plugin can set a separate metrics host:port
  (file schema just doesn't expose it) → custom plugin beats file-discovery for backends with split metrics ports.
- Inactive off-k8s: InferenceModelRewrite, InferenceObjective (per-request SLO objectives), k8s-notification-source.
  FlowControl itself NOT listed as disabled (vs llm-d no-k8s doc naming "InferenceObjective-driven FlowControl") →
  minor discrepancy; interpret as: FlowControl runs, priority inputs from InferenceObjective absent.
- --pool-namespace resolution: flag > NAMESPACE env > "default" (warns). Always set it off-k8s.
- Readiness: k8s mode admits pods only when aggregate Ready (honors readinessGates) → custom/file discovery has NO
  equivalent gate; plugin must only Upsert healthy endpoints (health agent). Load-bearing for roadmap.
- CONFIG PATH CONTRADICTION (unresolved, two HEAD artifacts): discovery.md → `dataLayer.discovery.endpoints.pluginRef`;
  llm-d guide config.yaml → `dataLayer.discovery.pluginRef`. discovery.md's interface listing omits Ready() that
  discovery.go has → parts of discovery.md lag code. Resolve via config struct (wave2-C) — else report both + "validate
  against your pinned release; the EPP rejects/ignores unknown keys" (check which).

### 22:30Z — "Writing a custom discovery plugin" skeleton (discovery.md)
- Documented path: implement fwkdl.EndpointDiscovery in your own package importing
  github.com/llm-d/llm-d-router/pkg/epp/framework/interface/{datalayer,plugin}; call
  `fwkplugin.Register(myplugin.PluginType, myplugin.Factory)` "in the runner before calling Run"; reference
  `type: my-discovery` in plugins + point dataLayer.discovery.endpoints.pluginRef at it.
  → fwkplugin.Register is an exported package-level registry, so a DOWNSTREAM main() (import runner, Register, then
  runner.NewRunner().Run(ctx)) = out-of-tree, NO FORK. Pending wave2-C confirmation that runner doesn't reset registry.
- The doc skeleton does NOT compile against HEAD: Factory returns value MyDiscovery{} while methods use pointer
  receivers; Upsert passed a value but takes *EndpointMetadata; no Ready() although discovery.go requires it.
  → Report guidance: treat upstream docs as illustrative; copy the file plugin (plugin.go) as the real template and
  compile against a PINNED llm-d-router tag; add a CI job that builds your plugin against upstream main weekly to catch
  interface drift (interface is ~3 months old, Alpha/Beta lifecycle applies).
- Remaining k8s residue a custom plugin must fake: types.NamespacedName identity (pick a stable namespace per
  backend-source, e.g. "slurm"/"consul"), --pool-namespace, controller-runtime import (still linked, not started).

### 22:40Z — wave2-A report
- Issue #1403: datastore fired EventAddOrUpdate only on first add, not on metadata mutation of an existing endpoint;
  fixed PR #1404 in v0.9.0 → MINIMUM VERSION for a custom discovery plugin that relabels endpoints = v0.9.0.
- AUDITED ABSENCE confirmed: no non-file EndpointDiscovery plugin (Ray/Slurm/Consul/etcd/cloud SD) exists as of
  2026-09-11; all "future work". Ray/Slurm today = generator scripts writing the file.
- Predicted-latency scheduling: training/prediction sidecars run next to the EPP (FastAPI/uvicorn), model server
  unchanged → portable off-k8s in principle; reference deployment k8s-native. "Standalone Mode" = no Gateway API
  object, NOT no Kubernetes (terminology trap to explain in report).
- Security audited absences: metrics scraping built on PodMonitor/ServiceMonitor CRDs (lost off-cluster), plain HTTP by
  default, TLS opt-in; ZMQ KV-event channel config documents NO TLS/auth → off-k8s the user must supply network
  isolation (private VLAN/WireGuard/mTLS proxy) — Risks section.
- Misc: parser plugin registration pattern (longest-suffix, passthrough fallback); dev.to 400ms piece = low-authority
  generic; solo.io duplicate deprecated. routing-sidecar docs page never fetched (content covered by archived repo note).

### 22:50Z — wave2-C report (DECISIVE)
- registry.go: `Registry` = exported package-level map[string]FactoryFunc; `Register()` exported; NO duplicate-key guard.
  runner.go: exported builder (NewRunner, WithExecutableName, WithRequestControlConfig, WithSchedulerConfig, Run) but
  in-tree registration is UNEXPORTED registerInTreePlugins(); no WithPlugins() hook.
  → OUT-OF-TREE IS TECHNICALLY SOUND: downstream module `init()`-registers its factory into the global map, own main()
  calls runner.NewRunner().Run(ctx). No fork. Risks: unique type name (no collision guard), internal-package drift.
- create_new_filter.md (official tutorial) instructs editing runner.go IN-REPO — only documented path.
- AUDITED ABSENCE: no downstream repo found importing llm-d-router as a library to register plugins (2 searches).
- CONFIG PATH RESOLVED: PR #2434 (commit 950c963644, merged 2026-08-25T18:27:45Z) → `dataLayer.discovery.endpoints.
  pluginRef`; migrateDiscoveryConfig shim still honors legacy `dataLayer.discovery.pluginRef` (why guide config works);
  runner.resolveDiscovery() reads rawConfig.DataLayer.Discovery.Endpoints.PluginRef. Blog/guide = legacy form.
- PR #2620 (HEAD 2026-09-11) = Helm-only, 0 Go files — no registration change.
- PR #1218: interface = Start + Ready() (closed once after initial sync); runner bypasses controller-manager when
  discovery configured.
- arXiv 2511.21413 (RAMSES): HPC gateway discovers vLLM endpoints from a Postgres table fed by a Slurm-bridging Job
  Worker — independent precedent for "registry-fed discovery"; ~500 ms overhead at 1000 concurrent (their stack, not EPP).
- Slinky slurm-operator: NodeSet CRD → Slurm workers as real pods (StatefulSet/DaemonSet) — the inverse strategy when a
  k8s control plane is acceptable.
- DIALECTICAL LOCUS CONFIRMED: "out-of-tree plugin (technically clean, undocumented, unprecedented) vs in-tree/fork
  (documented, merge burden) vs no-code file generator (supported, weaker lifecycle/health semantics) vs upstreaming".

### 23:00Z — wave2-B report (last fetcher)
- GIE #1838: standalone EPP design rationale — 4 deployment modes, 3 discovery mechanisms, motivating use cases
  Mistral / RL / batch. GIE #1779: pod-selector discovery for standalone; maintainers pushed back on CLI selectors.
  GIE PR #1821: standalone EPP still depended on InferencePool CRD as of Nov 2025. GIE PR #2882: Helm support for
  agentgateway as standalone EPP sidecar.
- agentgateway PR #1429 (standalone local config; fail-closed by design; maintainers REJECTED restricting
  InferenceRouting to Service-backed types) + PR #1658 (destinationMode: passthrough — trust EPP destination without
  mirrored k8s Endpoint data).
- Envoy PR #38757 = dynamic_forwarding LB policy vs Override Host debate; GIE PR #761 = fallback endpoint list in
  x-gateway-destination-endpoint. CORRECTION: kgateway #11824 — kgateway abandoned Envoy-as-AI-data-plane for
  agentgateway (Sep 2025) rather than converging on Override Host.
- llm-d PR #421: kgateway → agentgateway migration. Releases: v0.7.0 (2026-05-12) standalone mode = DEFAULT;
  capability row "Non-Kubernetes mode (RL/Slurm) — llm-d-router (FileDiscovery plugin)". llm-d-router versioned
  independently (v0.10.0) ahead of meta-repo v0.9.0.
- NVIDIA Dynamo docs describe its own "standalone EPP" pattern for vanilla vLLM → EPP reused by a competitor.
- interLink intro: long-running services excluded (second source).
- PROXY SIDE SETTLED: two supported data planes for non-pod endpoints — (a) plain Envoy ORIGINAL_DST + header
  (llm-d guide), (b) agentgateway standalone + passthrough (fail-closed). Gateway-mode proxies (Istio/NGF/kgateway)
  are InferencePool-bound.

### 23:15Z — step 3 contradiction graph
- 622 claims (258 supports / 314 neutral / 50 refutes); independence: only cluster = duplicate README pair.
- Fights ranked HIGH: out-of-tree registration; KV-event subscription mode off-k8s (pull via pod reconciler vs push via
  Static Endpoint Mode — OPEN: which works with file discovery + identity alignment); no-k8s feature parity (logic vs
  inputs/lifecycle); k8s-object shim viability; fail-open vs fail-closed. MEDIUM: "standalone" terminology;
  k8s-native-by-design; proxy mechanism; llm-d vs alt routers (both sides vendor benchmarks); external providers
  placement (really consensus); metrics-endpoint coupling. LOW (resolved): IPv4/hostname, config path, rl-controller.
- Loci candidates for step 4: (1) KV-event path off-k8s — the biggest unresolved TECHNICAL fork; (2) extension strategy
  dialectic (file generator vs out-of-tree plugin vs in-tree fork vs upstream); (3) lifecycle/health + fail-closed
  HA design off-k8s; (4) policy-input gap (InferenceObjective/ModelRewrite substitutes) — maybe fold into (3).

### 23:20Z — step 4: two loci analysts dispatched; my baseline candidates for the merge
Scoring = importance + uncertainty + disagreement + decision_impact (0-40); total budget 40 sources.
1. [technical] KV-event path & identity alignment off-k8s — can precise prefix routing run with file/custom discovery
   (push Static Endpoint Mode vs pod-reconciler pull; kv@ip:port topic vs NamespacedName endpoint id)?
   provisional I9 U9 D6 DI8 = 32 → budget ~12. Needs code: EPP precise-prefix plugin wiring of the kv-cache indexer.
2. [dialectical] Extension strategy — file-generator sidecar (no Go) vs out-of-tree plugin (exported registry, no
   precedent) vs in-tree fork (documented) vs upstream contribution; maintenance cost over llm-d's release cadence.
   provisional I10 U6 D7 DI10 = 33 → budget ~12.
3. [technical/dialectical] Operating non-pod backends without k8s lifecycle: health/readiness, fail-closed data path,
   EPP HA (v0.9 multi-replica) + file/plugin consistency across replicas, security of metrics/ZMQ/ORIGINAL_DST.
   provisional I8 U6 D6 DI7 = 27 → budget ~8.
4. [synthesis] Policy-input gap: substitutes for InferenceObjective priority and InferenceModelRewrite off-k8s (static
   config? IPP payload processor? gateway tier?) — plus where managed providers sit (two-tier).
   provisional I6 U5 D4 DI5 = 20 → budget ~5. Could merge into 3 if analysts don't raise it.
5. [synthesis] Hybrid: router in k8s targeting out-of-cluster backends vs router fully outside — which topology when
   (Slinky real-pods inverse strategy, InferencePoolImport, file-discovery ConfigMap in-cluster).
   provisional I7 U5 D4 DI6 = 22 → budget ~5.
Dialectical requirement satisfied by #2 (and #1 has genuine pull/push fork).

### 23:35Z — step 4 done, step 5 dispatched
- loci.json: extension-mechanism (34/10), kv-event-path-off-k8s (32/10), availability-and-lifecycle-off-k8s (28/7),
  custom-discovery-plugin-design (24/6), topology-and-k8s-shims (24/4), lost-k8s-policy-inputs (21/3). 11 skip_loci.
- 6 depth investigators running in parallel. On return: list interim notes (`note list --type interim`), verify each
  ends with "## Committed position", batch-read, then step 6 cross-locus reconcile.
- Cross-locus tensions to watch for in step 6: (a) extension-mechanism may recommend file-generator-first while
  custom-discovery-plugin-design recommends a plugin — reconcile as a phased roadmap, not a contradiction;
  (b) kv-event-path may find precise routing unsupported off-k8s, which weakens the "use llm-d for its moat" argument
  in extension-mechanism; (c) availability may conclude fail-closed + HA needs k8s Lease, colliding with topology's
  "router fully outside k8s" branch.

### step 5 returns
- [1/6] lost-k8s-policy-inputs → interim-report-lost-k8s-policy-inputs (1 new fetch: epp-http-headers-reference-llm-d).
  FlowControl priority bands = static YAML (not CRD); PodMonitor → plain Prometheus scrape config; model selection via
  shipped `x-llm-d-model-name-rewrite` header; tenant fairness `x-llm-d-inference-fairness-id` works off-cluster. REAL gap:
  per-request priority (`x-llm-d-inference-objective` needs CRD lookup → nil off-cluster) → small static/file-backed
  priority-resolution plugin. Weighted canary → gateway tier. Resolves "platform-invariant vs inactive" as equivocation.

- [2/6] topology-and-k8s-shims → interim-report-topology-and-k8s-shims (1 new fetch: pod_reconcilergo @ HEAD).
  EndpointSlice shim = category error (EPP watches Pods by label via pod_reconciler.go, never Services/EndpointSlices).
  Virtual-kubelet = architecturally viable (label match + aggregate Ready; PodIP consumed unvalidated) but no production
  HPC provider (interLink egress-only). Slinky = "convert estate to pods", a different question. Real fork =
  router-in-cluster vs out-of-cluster = ORGANIZATIONAL (who owns router lifecycle); file/custom discovery serves both;
  in-cluster hybrid works today via Helm `pluginsCustomConfig` + `volumes` (capability inference, no case study).
  CORRECTION: PR #1007 note summary describes inference-pool-discovery/static-selector-discovery k8s plugins NOT on main;
  default k8s path is still pod_reconciler.go → confirms "k8s watch behind the interface" is still future. CURATION: fix
  that note's summary at the end-of-run curation pass; drafters should cite pod_reconcilergo for k8s discovery.

- [3/6] kv-event-path-off-k8s → interim-report-kv-event-path-off-k8s (4 new fetches: precise-prefix-cache-producer
  dir, README, producer.go, extractor.go). PRECISE ROUTING WORKS OFF-K8S TODAY: producer implements generic
  fwkdl.EndpointExtractor driven by endpoint-notification-source (fed by k8s OR file discovery); index/score keyed on
  literal Address:Port, never NamespacedName; router connects out to each endpoint's ZMQ PUB at Address:SocketPort
  (+RankIndex). Narrow gap: RankIndex (DP multi-rank engines) has no carrier in file-discovery YAML → custom plugin can set
  it; single-rank unaffected. Audited absence: no end-to-end report of precise routing + file discovery. Open: repeated
  Upsert on file reload → subscriber dedup/idempotency. Resolves tension (b): llm-d's moat survives off-k8s.
  NOTE for drafting: distinguish `endpoint-notification-source` (generic, works off-k8s) from
  `k8s-notification-source` (inactive off-k8s) — easy to conflate.

- [4/6] custom-discovery-plugin-design → interim-report-custom-discovery-plugin-design (5 new sources + GitHub REST API
  verification). CORRECTION (verified via pulls/1007, files, main tree, closing comment): PR #1007 (watch-based k8s
  discovery plugin under EndpointDiscovery) was CLOSED UNMERGED/abandoned — vault note fixed in place. ⇒ zero in-repo
  precedent for a watch-based EndpointDiscovery plugin; only file (poll + fsnotify) shipped. Committed 3-tier rule:
  T1 poll+diff ≈2-4 days; T2 long-poll where source supports it (Consul blocking queries) +1-2 days; T3 persistent watch
  ≈1-2 weeks, reserve for a named staleness cost ("the interface owners tried watch and backed out").
  DRAFTING WARNING: any claim citing PR #1007 as "merged"/"shipped k8s + file discovery" is WRONG — cite the file
  plugin PRs (#1135/#1218) and pod_reconcilergo instead. Check consensus-claims / contradiction-graph for #1007 reliance
  before step 10 (neither cites the #1007 note id directly; OK).

- [5/6] extension-mechanism → interim-report-extension-mechanism (3 new fetches: CONTRIBUTING.md, registry.go re-read,
  issue #2105). Committed (medium-high tech / medium ranking): STAGED — (1) file-discovery fed by external generator for
  static/semi-dynamic estates (zero Go); (2) else out-of-tree EndpointDiscovery plugin in thin downstream main()
  importing cmd/epp/runner (init()-before-main + exported Registry) — beats in-tree fork (same dep graph + rebase burden;
  discovery config reshaped 3x in ~4 months) and library embedding (same deps, larger break surface, reimplements Run());
  (3) upstream IN PARALLEL. Mitigations: namespaced plugin type strings (no dup-key guard); own panic recovery in Start()
  (issue #2105: NO plugin invocation path has panic recovery; out-of-tree framed "at-your-own-risk", untriaged).
  Audited absence confirmed: no downstream example, no CONTRIBUTING guidance, no ratified maintainer statement.

- [6/6] availability-and-lifecycle-off-k8s → interim-report-availability-and-lifecycle-off-k8s (note complete with
  committed position; agent completion notice pending). Committed: v0.9 default Active-Passive HA "coordinates lease
  status", standbys NOT_SERVING → k8s-Lease-coupled by default (medium-high; exact API inferred, not quoted); no documented
  non-k8s leader election → off-k8s options = single replica, Active-Active with poll-only/stateless plugin allowlist, or
  own coordination plugin. v0.9 blog "no additional coordination" framing inaccurate for the default. Fail-open verdict:
  neither pure side — opt-in FAIL-OPEN GATED ON AN EXPLICIT FALLBACK POOL (sketched in #1661); fail-closed stays default.
  Header spoofing CLOSED: merged test asserts client-supplied x-gateway-destination-endpoint discarded (post
  2026-04-23 cherry-pick); older/vendor-forked EPPs vulnerable.

- [6/6 REVISED] availability: docs/operations.md QUOTED — default HA "Uses Kubernetes coordination.k8s.io/Lease
  coordination" (confidence now HIGH). ALL THREE HA mechanisms (Lease leader-election, StatefulSet Priority Routing,
  Fully Active-Active) are k8s-coupled. llm-d's Envoy standalone path ALREADY SHIPS scoped fail-open
  (`router.proxy.failOpen: true`, default in standalone) gated to the lease-transition window = the "fallback-gated
  fail-open" design. agentgateway passthrough still fail-closed (#1661 open). Off-k8s: single EPP works; multi-replica HA
  needs k8s-for-the-control-plane OR a custom coordination primitive via exported Register/Runner. Open: non-k8s lock
  proposal? in-tree PeerDiscovery plugin? no-k8s envoy.yaml failure_mode_allow default? ZMQ auth option?
  → Topology implication: "router in-cluster, backends out-of-cluster" (hybrid) is the ONLY documented way to get EPP HA
  for non-pod backends today — strengthens the hybrid branch and ties topology to availability.

### Step 5 synthesis — cross-locus picture for step 6
- Coherent staged strategy: discovery = file+generator → Tier-1/2 custom plugin (out-of-tree, thin main) → upstream;
  routing plugins (incl. PRECISE prefix) reused unchanged; proxy = Envoy ORIGINAL_DST (or agentgateway passthrough);
  policy gaps = static priority plugin + gateway-tier canary; topology = organizational choice.
- Tension (a) plugin-vs-file: RESOLVED as phases (extension-mechanism + custom-plugin-design agree).
- Tension (b) moat off-k8s: RESOLVED (precise routing works; RankIndex gap only for DP multi-rank).
- Tension (c) HA off-k8s: CONFIRMED as the largest undocumented cost → must be a headline risk + roadmap item.
- New cross-locus tension: extension-mechanism says "out-of-tree plugin is low-risk with 2 mitigations", but the
  availability note adds a THIRD place custom Go may be needed (coordination/leader election) and lost-inputs adds a
  FOURTH (static priority Admitter, Alpha tier, --allow-experimental-plugins) → the "thin downstream main" becomes a
  small plugin BUNDLE; maintenance-cost argument must be re-run at bundle scale (still favors out-of-tree over fork?).
- INCONSISTENCY to flag in comparisons.md: topology note's committed position cites "merged PR #1007" but #1007 was
  closed unmerged (custom-plugin-design verified via GitHub API). Evidence that stands: pod_reconcilergo @ HEAD.
- Version floor emerging across loci: router ≥ v0.9.0 (#1403/#1404 fix; HA; header-spoof fix predates), config key
  dataLayer.discovery.endpoints.pluginRef (post 2026-08-25).

### Step 7 — source tensions (7) + key new synthesis
- Orphan scan (full reads of #1838, #620, #1779, PR #38757, #2105, operations.md, v0.9, vLLM Router, Dynamo, CONTRIBUTING):
  * #1838 debate: ahg-g "meet customers where they are... end goal is to land users on k8s" vs nirrozenbaum (zero-k8s
    out of scope for kubernetes-sigs; proposed "a discovery layer... similar to scheduler pluggability" = what shipped)
    vs howardjohn "All Kubernetes / No Kubernetes; a hybrid is very odd" (later "alright I'm convinced").
    → routing layer platform-neutral & endorsed; operations layer k8s-first. Hybrid-for-HA must be defended explicitly.
  * operations.md: Active-Active near-linear (1.0/2.0/2.7/3.5x), NO leader election, BUT flow control per replica and
    "avoid Active-Active with APPROXIMATE prefix routing" (state partitioned). Precise prefix: every replica subscribes to
    all KV streams → full index per replica.
    → KEY SYNTHESIS: off-k8s HA = Active-Active behind plain L4 LB + PRECISE prefix routing (inference; needs a test).
      Narrows "HA needs k8s" to "Active-Passive HA needs k8s". Precise prefix becomes REQUIRED for off-k8s HA.
  * Dynamo ships its OWN EPP behind the GIE protocol → the Endpoint Picker Protocol is the portable seam; llm-d choice is
    reversible at the cost of the discovery plugin only.
  * #2105 verbatim option 2: "in-tree plugins are the supported surface; out-of-tree plugins that panic take the process
    down"; flow-control plugins run inside the single dispatch goroutine holding queue mutexes.
- Tensions: (1) registry vs in-tree [high]; (2) non-k8s first-class vs on-ramp [high]; (3) Lease HA vs Active-Active
  [high]; (4) platform-invariant vs lost inputs/lifecycle [high]; (5) fail-closed vs fail-open+fallback [medium];
  (6) llm-d EPP vs alt routers / protocol portability [medium]; (7) docs vs code [medium].

### Step 8 gap results
- [D/4] VERSION FLOOR CORRECTED (overturning, must fix in draft): v0.9.0 (Jun 23 2026) shipped FileDiscovery;
  v0.10.0 tagged Aug 17 2026 is STILL the newest release (no v0.10.1/v0.11 as of Sep 11); PR #2434 (discovery config
  consolidation) merged Aug 25 — EIGHT DAYS AFTER the v0.10.0 tag → `dataLayer.discovery.endpoints.pluginRef` exists
  ONLY on unreleased main. A user pinned to v0.10.0 MUST use legacy `dataLayer.discovery.pluginRef`.
  Draft must state both keys with their version applicability (this is exactly the docs-vs-code tension in practice).
- v0.10.0 DOES include `dataLayer.peerDiscovery.pluginRef` (PR #2341, merged Aug 16). The ONLY peer-discovery plugin
  (PR #2501, k8s Pod-watch, Alpha) is still OPEN/unmerged → off-k8s peer discovery = another custom plugin →
  reinforces Tension 1 (bundle) and complicates the Active-Active-off-k8s inference (peer discovery may be how
  replicas coordinate; gap-1 fetcher will confirm via #1290).
- No fourth config reshaping proposed (0 open issues on dataLayer.discovery; v0.11 milestone due Sep 30 has none).
- CONFIRMED AUDITED ABSENCE (strengthening the effort estimate): zero Consul/etcd/Slurm/Ray discovery PRs — every
  discovery PR targets flat files or k8s primitives. A non-k8s dynamic plugin is greenfield.

- [B/2] OUT-OF-TREE: no structural blocker (cmd/epp/runner + framework/interface/{plugin,datalayer} NOT under internal/;
  go.mod has no replace directives; costs = go 1.26.6 floor + unconditional controller-runtime tree). Precedent absence
  CONFIRMED properly: pkg.go.dev "No known importers" for all three packages (re-fetched with builtin provider + raw
  verification); GitHub code search = auth wall (tool limitation, NOT absence).
  REFINEMENT (strengthens recommendation): the "only documented path is in-tree" claim conflated two docs —
  create_new_filter.md is the SCHEDULING-FILTER tutorial; docs/discovery.md officially documents "Writing a custom
  discovery plugin" with fwkplugin.Register(...) before Run(). So registering a custom DISCOVERY plugin is documented
  upstream; only the downstream-main() packaging is inferred. #2105 = 1 of 3 proposals, needs-triage, 0 comments.
  → source-tensions tension 1 updated (side_b position/evidence + resolution).
  Housekeeping: fetcher deprecated 3 duplicate notes; 4 unsummarized notes (maingo-2, file, discovery, commongo) came
  from the concurrent gap-A fetcher — check they get summaries before step 9/10 curation.

- [A/1+5] HA INFERENCE SURVIVES AND IS UPGRADED. #1290 = QPS/latency benchmark issue (May 24–Jul 21 2026), NOT a
  state-sync design doc; operations.md's citation of it for "replica state sync" oversells it. Empirics favor us:
  precise-prefix Active-Active @500 QPS failures 1r=3148(broken) → 2r=57 → 3r=31 → 4r=18; TTFT P50 165→30ms.
  cross_replica_syncer.go: separate opt-in CrossReplicaSyncer/CrossReplicaContributor surface; precise-prefix extractor
  implements ONLY EndpointExtractor → consistency by independent KV subscription, by design. peer.go doc: "the k8s
  reconciler and any file-based discovery feed the same sink" → file-based peer discovery is a named concept (no impl).
  TRAP for the report: standalone Helm chart auto-injects --ha-enable-leader-election when replicas>1 → silently
  Active-Passive unless disabled. OPEN GAP: cross-replica SESSION AFFINITY never evaluated (#1290 used independent
  synthetic requests) → session-pinned workloads must not assume active-active correctness.
- RankIndex characterized: positional ("position in the pool's TargetPorts... pod-local rank in multi-port deployments"),
  NOT label-derived; file schema has no rank field (plugin.go has no "rank" at all) → always 0 under file discovery;
  custom plugin can set it. NodeAddress doc: "Empty for non-Kubernetes discovery sources (e.g. file discovery)" and
  Ready() names "file load for FileDiscovery" → first-party confirmation non-k8s discovery is a recognized shape.
  multicluster-file-discovery variant: RFC-1123 hostnames + metricsAddress/metricsPort labels.
- Note: GitHub code-search auth wall queued as escalation #18 — NOT worth human attention (direct file reads answered it).

- [C/3] P/D SIDECAR CARVE-OUT (the one attack that landed): allowlist.go at HEAD needs a k8s client + dynamic informer
  watching ONE named InferencePool + its pods; no static/flag/CIDR alternative; flags are only
  --enable-ssrf-protection (default FALSE) and --inference-pool. Disabled ⇒ IsAllowed() returns true unconditionally
  ("backward compatibility") and disaggregatedPrefillHandler proxies to whatever x-prefiller-host-port says = the
  exploit in issue #242 (Jul 2025, PoC). Upstream threat model: PRIMARY control is the gateway/EPP stripping the header
  on ingress (PR #244); allowlist is "an additional safety net". Open issue #979: allowlist matches host/IP but NOT port.
  TLS for the prefill hop is portable plain flags (--enable-tls=prefill,decode,encode, --tls-min-version, etc.).
  → Draft: carve the sidecar out of "reuse unchanged"; requirement = NETWORK ISOLATION (firewall/SG/mesh ACL, not
  necessarily k8s NetworkPolicy) + enable prefill TLS; cite #242 and #979 separately with dates.
- Step 8 closed. Housekeeping: renamed 3 orphan claims files to real note ids and re-ingested (+7 claims). 3 remain
  orphaned (gomod-tree-search, llmd-router-ha-lease-supplement, milestones-issues-absence) — their content spans several
  notes or duplicates an existing file, so they stay on disk for the digest but are absent from the claims TABLE;
  harmless (absence/verification claims already captured in comparisons.md + corpus-critic-results.md), but step 14.5
  cite-check will not auto-pass sentences resting on them.
- Corpus at step 9: 225 tagged / 219 live notes, 183 claims files, 740 claims in table.

### Step 11 synthesis pre-plan (written while A/B/C draft; refine once their report-backs land)

**Thesis the synthesis should defend** (survives all three angles because B's objections are about the operations
layer, not the routing layer): *Customize llm-d at one seam — EndpointDiscovery — and reuse everything above it. The
router's intelligence is genuinely platform-neutral; what is Kubernetes-native is the operations layer around it, and
that is a deployment choice you make separately from the discovery decision.* Phases: (1) file-discovery fed by a
generator, zero Go; (2) a custom discovery plugin registered out-of-tree from a thin downstream main(); (3) upstream it.

**Where A and B genuinely collide (the beats the report must land, not gesture at):**
1. Out-of-tree plugin: A says documented mechanism + no structural blocker; B says zero importers, untriaged #2105,
   no panic recovery. Beat: assert the mechanism, concede first-adopter status, price the two mitigations.
2. HA: A leans on Active-Active + precise prefix (now backed by #1290's numbers); B leans on Lease-coupled default and
   the chart auto-injecting --ha-enable-leader-election. Beat: HA requirement decides topology, not taste.
3. P/D sidecar: B's strongest card and it is CORRECT — SSRF allowlist is InferencePool-bound, off by default, and #979
   shows it ignores port even when on. Beat: carve it out of "reuse unchanged"; network isolation + --enable-tls=prefill.
4. "Why not vLLM Router/Dynamo": B says non-pod is table stakes there. Beat: the EPP protocol is the portable seam, so
   the llm-d bet is reversible; the cost of switching later is one plugin, not the architecture.
5. Docs vs code: version/config-key correction is the sharpest practical fact in the whole report — state both keys with
   their release applicability, pin a tag, CI against main.

**Contentious atomic items to watch in the drafts:** section 6 (strategies) risks becoming a flat list — it must end in
a ranked recommendation; section 7 (roadmap) must carry real effort numbers (T1 2-4 days / T2 +1-2 / T3 1-2 weeks) and
the version floor; section 8 must separate #242 (historical, partially fixed) from #979 (open) rather than conflating.

**Hygiene for the synthesizer:** wikilink citations to SOURCE notes only — never [[interim-*]], never comparisons or
scaffold. Target ~7000-7500 words. Every one of the 8 required H2 headings, in order. Quotation marks only around
verbatim source text (the ship gate's quote-integrity check fails rhetorical quotes).

### Step 11 dispatched — what to check before and during step 12

**Mechanical gates I run MYSELF on the final report the moment the synthesizer returns** (cheapest to fix now):
- Word count 7000-7500 (hard ceiling 10000). If over → ONE permitted compression re-spawn; that is the only
  regeneration allowed and only for length.
- Pass2 < pass1 (a positive delta means the cleanup pass didn't cut).
- All 8 H2 headings, exact wording and order; thesis stated in the first ~150 words.
- Citation validity: re-run the wikilink validator (every [[id]] resolves, no deprecated, no interim-*/comparisons/
  scaffold/draft artifacts). Drafts were clean at 479 citations — the synthesizer must not invent ids.
- No adjacent citation stacks `]][[`; no YAML frontmatter; no pipeline vocabulary; quotation marks only on verbatim
  source text (ship-gate quote-integrity check).

**What each critic is likely to hit, and what I should accept vs push back on:**
- *instruction-critic*: coverage of every atomic item — especially the BOTH-readings scope condition (router inside
  k8s targeting external backends AND router fully outside), managed/external APIs, and all backend classes (bare
  metal, VMs, Slurm, Ray, non-k8s containers, remote clusters). Also the required formats: component-coupling table,
  strategy comparison table, phased roadmap. Accept these findings — they're structural and cheap to patch.
- *dialectic-critic*: whether the steelman of "don't customize llm-d at all" is stated at full strength before being
  answered, and whether the 5 cross-locus tensions + 7 source tensions are each a visible beat rather than a gesture.
  Highest-value critic for this run given the advocate register.
- *depth-critic*: technical specificity — interface method signatures, the exact config keys per version, the ZMQ
  topic shape kv@ip:port@model, RankIndex positional semantics, #1290's numbers, tiered effort estimates, the
  prefixMatchInfoProducerName wiring requirement (silent fallback to approximate if omitted).
- *width-critic*: corpus-supported topics at risk of being dropped — the payload processor (IPP) as a second seam,
  multicluster-file-discovery (hostnames + separate metrics address), InferencePoolImport, the KServe/KAITO
  "wrap don't fork" precedent, predicted-latency sidecars running EPP-side, per-replica flow control, observability
  without PodMonitor, and the never-evaluated cross-replica session affinity.
- PUSH BACK on any finding that would re-open a settled fact (version/config key, #1007 unmerged, sidecar carve-out)
  or that asks for hedging on the thesis — the register is advocate and the corrections are verified.

### Step 11 validation + step 12 returns
- Synthesizer self-report was WRONG on length: claimed ~7,300 words, actual 9,575; pass2 (+59) GREW vs pass1 instead
  of cutting. Within the real ship-gate band (5000-10000 for argumentative, ±20%), so no compression re-spawn spent.
  Cause per its own report: pass 2 added inline definitions + 2 evidence items. Recorded, not escalated.
- My mechanical gates on the final report: 8/8 headings verbatim and in order; 198 citations / 111 distinct, zero
  invalid, zero deprecated, zero artifact cites; no adjacent stacks; no Sources section (correct for wikilink); no
  frontmatter; no pipeline vocabulary; citation density 2.71/1000 chars (floor 1.5).
- QUOTE-INTEGRITY was the one real failure (verify reported 7 errors; my own scan found 14 spans). Cause: wording
  matched sources but markdown emphasis/splices broke verbatim matching. Fixed by UNQUOTING all 14 (wording and
  citations preserved) per the ship-gate rule that quotes are reserved for exactly-verbatim text. Now 10 quoted spans,
  all verbatim. Re-check after patcher/polish, since both may reintroduce quotes.
- [instruction-critic] 0 critical / 4 major / 2 minor; structural mirror PASS; all 4 required formats honored
  (18-row component inventory, 9-strategy comparison table, phased roadmap with effort + exit criteria, committed
  recommendation). Majors for the patcher: (1) comparison table works 4 of 7 named dimensions — latency absent as an
  axis, failure handling global not per-strategy; (2) auth dropped from per-backend requirements (survives only as a
  Phase 4 clause); (3) section 3 never enumerates scheduling-framework plugin types nor names EndpointPickerConfig;
  (4) hybrid (router in-cluster, backends outside) reading is one sentence vs a whole phase for fully-outside — and it
  hides a real open point: whether InferenceObjective reconcilers become usable again in-cluster, or whether the
  discovery-plugin branch skipping controller-manager setup forecloses them even there.

- [dialectic-critic] 12 findings (2 critical / 7 major / 3 minor).
  ORCHESTRATOR DECISION on its escalation (#2, the "can the estate simply be pods?" gate): ACCEPT as a patch, not a
  restructure. Instruct the patcher to insert a **Rule 0** at the head of section 7's decision procedure — if the
  compute can become Kubernetes pods and the organisation accepts Kubernetes as the primary scheduler (Slinky), that is
  the maximum-reuse option and requires zero customization; it is a governance decision to make explicitly rather than
  default past. Do NOT renumber the existing Rules 1-4 (avoids a cascading multi-hunk edit); Rule 0 sits above them.
  Rationale: the user's constraint is "use llm-d components as much as possible", and conversion maximises reuse — the
  report already says so in §6C but never surfaces it where the reader makes the decision.
  Other findings the patcher must land: (#3, critical) state vLLM Router's SPECIFIC adverse claim (+25% throughput,
  1200 ms TTFT, 8P/8D Llama 3.1 8B) and answer it with the vault's mitigants (vendor-internal, no published harness,
  excluded vLLM's own DP/EP coordinator, llm-d engineers assisted) instead of genericising it; (#5) say flatly that
  off Kubernetes the sidecar allowlist cannot be enabled at all, so it runs permanently in unconditional-allow mode —
  the inventory verdict "Yes, with network isolation" is softer than the draft's own evidence; (#9) use operations.md's
  first-party Active-Active scaling table (1.0x/2.0x/2.7x/3.5x) and its approximate-only warning, which is stronger
  support than #1290 alone and supplies the Active-Passive no-scaling contrast; (#8) name the derivation when
  extrapolating the off-cluster Active-Active recommendation from an in-cluster benchmark; (#7) reconcile §5E's
  "single genuine gap" with the inventory and §8; (#6) price the first-adopter risk in the opening thesis paragraph.
  Its verdict on honest hedges (hybrid unobserved, precise+file unpublished, magnitudes indicative): KEEP — correct
  honesty, not findings.

- [depth-critic] 12 findings (9 major / 3 minor); all six interim notes represented, no structural escalation.
  Diagnosis is uniform and highly patchable: the report keeps conclusions but paraphrases NAMED IDENTIFIERS out of
  existence — 0 HTTP header names (though it recommends using one), 0 Helm value keys (the hybrid branch rests on two),
  only 2 CLI flags (--pool-name/--pool-namespace; missing --enable-ssrf-protection, --metrics-endpoint-auth,
  --allow-experimental-plugins, --ha-enable-leader-election and the full reference invocation), 0 KV-event config keys
  in §5C where it says the thesis is won. Quantitatively rich otherwise, so this is a register problem in a document
  whose deliverable is an implementation roadmap. Patcher should restore identifiers inline.
  Top three: (1) Rule 1's Active-Active branch loses the quantitative decomposition — Active-Passive gives ZERO
  throughput gain, Active-Active scales 2.0x/2.7x/3.5x, mechanism is standbys answering NOT_SERVING so they never join
  the Service endpoint list; (2) the coordinator architecture is a FOURTH option Rule 1 omits while presenting three as
  exhaustive (an interim note explicitly recommended surfacing it, with its hedge); (3) the DiscoverPods/
  PodDiscoveryConfig misnomer is an advocacy win left on the table — the only k8s-shaped artifact on the flagship path
  is a config block's NAME, read for two integers, never used to open a watch.
  PROVENANCE (ship-gate relevant, patcher must apply): label the 2-4 day / 1-2 week tier costs as calibrated estimates,
  not measurements — the draft already disciplines vendor benchmarks this way; and bound the audited absences by stating
  what was searched, explicitly including that the authenticated GitHub code-search check for runner importers hit an
  auth wall and did not complete (the flat "no known importers" currently absorbs that hole silently).

- [width-critic] 10 findings (1 critical / 7 major / 2 minor): 8 coverage gaps + 2 bloat.
  CRITICAL: RAMSES (arXiv:2511.21413, note `automated-dynamic-ai-inference-scaling-on`) is the corpus's ONE independent
  published benchmarked precedent for exactly the recommended architecture — a gateway reading vLLM endpoints from a
  Postgres table instead of Pod objects, with a Job Worker as the sole component talking to Slurm, benchmarked at
  100/500/1000 concurrent requests for ~500 ms added latency — and it is cited nowhere while the draft concedes "no
  precedent" three times. coverage-gaps.md had named it under gap resolution #2. Patcher must land it.
  Other majors: Coordinator (third plugin seam, docs 2026-09-10) and Inference Payload Processor missing from the
  component inventory; pluggable kvblock.Index with shipped Valkey/RDMA backend (an llm-d-native shared-index option
  for the recommended Active-Active topology) absent; predicted-latency sold twice without its published disqualifier
  (assumes a single pod shape; heterogeneous pools unsupported) and with no off-k8s colocation story; KAITO + KServe
  v0.17 consuming the EPP unmodified (precedent for the "use llm-d components" premise) unused; InferencePoolImport
  never dispatched in the shim section; ZERO mentions of autoscaling in ~9.6k words though v0.9 makes the router the
  source of truth for KEDA signals and KEDA/WVA do not travel off-cluster.
  BLOAT (funds the coverage patches): §8 has five successive concluding passages, three making the same point; §7C
  re-lists Rules 1-4; the value-proposition list is near-verbatim at two places. ~400-500 words recoverable.
  Also noted (not filed): Rule 4 assumes ORIGINAL_DST is the only proxy option; Envoy PR #38757 dynamic_forwarding is
  the alternative and is in the vault.

### STEP 13 DETERMINATION — no gap-fetch required
Every topic the four critics named is already covered by vault notes (RAMSES, coordinator docs, kv-cache architecture +
Valkey example, predicted-latency guides, KServe/KAITO, InferencePoolImport/api-overview, KEDA via v0.9 notes,
operations.md, vLLM Router). Step 13 exists to fetch sources for gaps where the vault has ZERO coverage so the patcher
has ammunition; there are none. Proceed to step 14 with the existing corpus and log this as a no-op.

### Step 14 + 14.5
- Patcher: 40 findings → 20 applied (all 3 criticals), 20 skipped (ALL for the word ceiling, not merits), 2 conflicts
  resolved, 2 escalations. Rule 0 landed without renumbering; RAMSES landed distinguishing "no precedent for an
  out-of-tree llm-d plugin" from "registry-fed discovery has published precedent"; vLLM Router's specific claim stated
  then answered on four mitigants. Identifiers restored broadly. Quote count unchanged at 10, all verbatim.
- Orchestrator ruled on the escalations with 375 words of headroom: applied 2 of 3 by hand (section 6A gained
  `Added request latency` + `Failure handling` axes — the finding they expose is that A-D and I add NO request-path
  latency because they all end at the same picker hop, so latency only moves if you hand-build (E) or switch (H);
  section 1B gained the Coordinator row). DECLINED the IPP inventory row: the IPP repo fetch was escalated and never
  landed, and no vault note documents it well enough to cite — omission beats a citation that doesn't support the claim.
  Deferred KAITO/KServe precedent (now bounded by RAMSES). Logged in orchestrator-restructure-log.md.
- Gates after edits: 9,765 words (235 headroom), 8/8 headings, 204 citations / 115 distinct, zero invalid, zero
  deprecated, zero artifacts, 10 quoted spans all verbatim, no adjacent stacks.
- citecheck extract: 204 pairs — 43 auto-passed, **0 dangling**, 108 sampled for LLM → split across two checkers.
- [cite-checker A, pairs 0-53] 50 supported / 3 partial / 1 wrong-source (critical: reload-failure claim cites the
  file-discovery folder README; correct source is docs/discovery.md or plugingo).

### Section mapping (draft)
1 inventory → table: component | k8s coupling | reusable outside pods?
2 discovery today → InferencePool selector → datastore → metrics collectors → ext-proc header
3 seams → EndpointDiscovery, datalayer sources, scheduler plugins, IPP, proxy config
4 upstream → v0.8 FileDiscovery, EPP code move, v0.9 HA, standalone mode, multi-cluster import
5 backend types → matrix
6 strategies → table A-F
7 recommendation → tiered roadmap
8 risks
