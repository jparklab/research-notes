# Coverage report — llmd-router-nonk8s-backends-54b774 (final, after wave 1 + wave 2-A/2-B/2-C)

Corpus at step 2.7: 147 tagged notes (146 live, 1 deprecated duplicate), 142 claims files → 622 claims ingested,
0 retractions (`sources score`: 2 DOI-bearing sources scored, none retracted), `graph rank` over 148 notes. Redundancy audit: 1 derivative pair (duplicate
llm-d-router README capture), no item loses independent coverage (see redundancy-audit.md).

| Atomic item | Status | Independent sources | Load-bearing sources |
|---|---|---|---|
| Q1 component inventory & k8s coupling | Well-covered | 15+ | llm-d-router README + source-analysis, architecture, EPP/Router/Data Layer/Configuration docs, v0.7/v0.8/v0.9 blogs, KServe v0.17, KAITO, RH LLMInferenceService, routing-sidecar (archived → in router), llm-d-kv-cache docs |
| Q2 discovery & tracking today | Well-covered | 12 | InferencePool v1, GIE datalayer + interface/datalayer godoc (EndpointMetadata = "Kubernetes Pod state"), proposals 0683/004, llm-d-router architecture.md, docs/discovery.md (k8s readiness semantics), EKS walkthrough |
| Q3 extension seams | Well-covered (code-level) | 14 | discovery.go, file plugin.go + README, docs/discovery.md (custom plugin skeleton), registry.go (exported Register), runner.go (unexported in-tree registration), create_new_filter.md, PR #908, PR #1007, PR #1218, PR #2434 (config path), plugins README (multicluster family), configuration + data-layer docs |
| Q4 upstream state | Well-covered | 12 | v0.7/v0.8/v0.9 blogs, no-k8s blog + guide + design doc, v0.8 goals #1520, GIE→llm-d-router consolidation (GIE PR #2906), GIE standalone guide (deleted 2026-06-30), agentgateway #620/#1661, GIE v1.3/v1.4, issue #1403 (fixed v0.9.0) |
| Q5 backend types & requirements | Well-covered | 16 | Model Server Protocol 003, vLLM kv_events.py, llm-d-kv-cache architecture/configuration (static endpoint mode), precise-prefix guide, vLLM RFC #16669, vllm-ascend RFC #9905, disagg docs, networking blog (NIXL decentralized pairing), predicted-latency docs, llm-d-rl, Slurm/NVIDIA, RAMSES arXiv 2511.21413, AKS/Agent Router/LiteLLM (managed providers) |
| Q6 candidate strategies | Well-covered | 18 | file-discovery generator pattern (blog/guide), custom plugin (discovery.md, registry.go), proxy wiring (envoy.yaml ORIGINAL_DST, agentgateway standalone/passthrough), k8s-shim family (EndpointSlices, k8s #123954, interLink + slurm plugin, virtual-kubelet, Slinky slurm-operator), alternative routers (vLLM Router, Dynamo, production-stack), two-tier gateway (AAIF proposal, Agent Router, AKS) |
| Q7 comparison | Adequate+ | 9 | vLLM Router benchmark (vendor-internal), Dynamo discovery backends, production-stack StaticServiceDiscovery, microserving paper, Mooncake, spheron comparisons, RAMSES overhead figure. Residual gap: no independent measurement of EPP/ext_proc overhead (audited absence). |
| Q8 risks & open questions | Well-covered | 10 | v0.9 HA, plugin stability lifecycle, agentgateway #1661 fail-closed, #1403, metrics plain-HTTP default + ZMQ no-auth (audited absence), k8s-only inactive features (InferenceModelRewrite/InferenceObjective), --pool-namespace fallback, doc drift (discovery.md skeleton doesn't compile vs HEAD), no out-of-tree precedent |

## Gap resolution (from the preliminary report)
1. EndpointDiscovery / FileDiscovery source + No-Kubernetes guide → RESOLVED (discovery.go, plugin.go, guide, design doc, discovery.md, PRs #908/#1007/#1218/#2434).
2. Non-file discovery implementation → AUDITED ABSENCE: none exists as of 2026-09-11 (Ray/Slurm/Consul/etcd plugins are "future work"; today = generator scripts writing the file). Independent precedent outside llm-d: RAMSES Postgres-fed discovery; Dynamo etcd/NATS; production-stack static discovery.
3. Independent EPP/ext_proc overhead evidence → AUDITED ABSENCE (no independent benchmark found; only vendor-internal vLLM Router comparison and unrelated gateway anecdotes).
4. Security outside k8s → PARTIALLY RESOLVED as audited absences: metrics scraping plain HTTP by default (TLS opt-in), ZMQ KV-events document no TLS/auth, Envoy↔EPP TLS dropped in colocated guide config (restore for split host), Istio/NGF default insecureSkipVerify to EPP.

## Contradictions surfaced during fetching (feed step 3)
- Discovery config path: blog/guide `dataLayer.discovery.pluginRef` vs discovery.go comment `EndpointPickerConfig.discovery.pluginRef` vs discovery.md/PR #2434 `dataLayer.discovery.endpoints.pluginRef` → resolved in favor of PR #2434 (legacy form accepted by a migration shim).
- KV-event topic identity: RH Oct-2025 article `kv@pod-id@model` vs HEAD `kv@<ip>:<port>@<model>` → resolved in favor of HEAD.
- File-discovery addressing: single-cluster plugin IPv4-only vs multicluster variant hostnames allowed.
- FlowControl off-k8s: llm-d no-k8s doc says "InferenceObjective-driven FlowControl" out of scope vs discovery.md lists only InferenceObjective objectives as inactive.
- Out-of-tree registration: technically possible (exported registry) vs only documented path is in-repo edit (create_new_filter.md).

## Retractions
None expected; confirm at step 2.7 `sources score`.

## Escalations
5 queued (browser lane unavailable this session — subagent lacks Chrome tools); 3 retried via CLI, still failing. Accepted gaps: IPP repo README (covered indirectly), vLLM kv_events config page (covered by kv_events.py source), AIBrix kv-event-sync (competitor detail), llm-d #1154 (IPP repo creation), production-stack GIE page.
