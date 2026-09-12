# Draft C — must_read_note_ids (n=45)

Angle: Synthesis-reconciler. Argue that both positions hold under different conditions, and build the decision rules: which backend estate, scale, availability requirement, security boundary and team capability select file-discovery vs a custom plugin vs Slinky vs a different router, and where the router should run (in-cluster vs out). Focus on boundary conditions, thresholds and the operational obligations each choice creates.

- interim-report-custom-discovery-plugin-design: Custom EndpointDiscovery plugin design: llm-d-router's only shipped plugin is file-discovery (poll+fsnotify); the K8s watch-based plugin PR (#1007) wa
- interim-report-kv-event-path-off-k8s: extractor.go confirms precise-prefix-cache-producer's per-endpoint ZMQ subscription is driven by the backend-agnostic fwkdl.EndpointExtractor/endpoint
- interim-report-topology-and-k8s-shims: Topology decision rule: EndpointSlice-shimming is a category error (InferencePool/EPP discovery watches Pods directly, confirmed at pod_reconciler.go 
- interim-report-availability-and-lifecycle-off-k8s: RESOLVED: llm-d-router's default EPP HA (--ha-enable-leader-election) literally uses Kubernetes coordination.k8s.io/Lease (confirmed in docs/operation
- interim-report-lost-k8s-policy-inputs: Only per-request priority assignment (InferenceObjective) is a genuine off-cluster gap needing new code; PodMonitor is solved by static Prometheus con
- interim-report-extension-mechanism: Out-of-tree EndpointDiscovery plugin (thin downstream main() importing cmd/epp/runner) beats in-tree fork and custom-router embedding for non-pod back
- source-analysis-llm-d-router-github-repo-readme-commit-history: Digest of the llm-d-router GitHub repo (README + commit history through Sep 11 2026, tag v0.10.0): confirms the Inference-Scheduler-to-Router rename a
- llm-dguidespredicted-latency-routing-at-main-llm-dllm-d-github: LONG (6188 words) upstream operator guide (llm-d/llm-d, guides/predicted-latency-routing, README last commit Sep 9 2026) for deploying predicted-laten
- llm-d-v07-from-feature-introduction-to-production-hardening-llm-d: llm-d v0.7 release blog ('From Feature Introduction to Production Hardening') -- highly load-bearing for router/non-k8s research.

RENAME: v0.7 rename
- routing-agent-traffic-is-really-three-decisions-aks-engineering-blog: AKS Engineering Blog (Jun 29, 2026), validated end-to-end mid-2026 against Gateway API Inference Extension v1.0.0 and agentgateway v1.3.1. Directly de
- how-llm-d-routes-model-inference-traffic-on-amazon-eks-red-hat-developer: Red Hat Developer article (Aug 13, 2026) traces a live request through llm-d on Amazon EKS via KServe LLMInferenceService, Istio, and Gateway API Infe
- epp-http-headers-reference-llm-d: llm-d Router v0.9 canonical EPP HTTP header reference. Priority/objective routing uses x-llm-d-inference-objective (names an InferenceObjective resour
- github-llm-dllm-d-latency-predictor-latency-prediction-service-for-ml-model-base: Upstream source repo (llm-d/llm-d-latency-predictor, Python 92.8 percent / Shell 4.7 percent, latest release v0.9.0, Aug 11 2026; generated from the l
- fix-harden-header-sanitization-and-handling-logic-by-lukeavandrie-pull-request-2: GIE PR #2025 (LukeAVanDrie, merged Dec 19 2025, later cherry-picked into llm-d/llm-d-router Apr 23 2026) hardens ext_proc header handling so a client-
- predicted-latency-based-scheduling-llm-d: Official llm-d docs (v0.9) overview of predicted-latency-based scheduling: an alternative/complement to the optimized-baseline heuristic scorer, using
- configuration-llm-d: llm-d dev docs, the canonical EndpointPickerConfig schema/mental-model reference (fetched Sep 2026) -- the single richest source on plugin taxonomy an
- llm-d-routerdocsdiscoverymd-at-main-llm-dllm-d-router-github: PRIMARY SOURCE, HIGHEST VALUE FOR THE RESEARCH QUERY. Canonical docs/discovery.md on llm-d/llm-d-router main branch (fetched Sep 11 2026, unpinned com
- centralized-routing-for-external-and-self-hosted-llms-on-openshift-ai-red-hat-de: Red Hat Developer article (published May 25, 2026, updated Jun 1 2026) demonstrates the generic 'gateway abstraction' half of the layered pattern -- N
- add-standalone-agentgateway-proxy-support-by-danehans-pull-request-2882-kubernet: GIE PR #2882 (danehans, opened Apr 24 2026, fixes issue #2883, approved) adds standalone Helm chart support for running agentgateway as the EPP sideca
- metrics-llm-d: Official llm-d docs (v0.9, current) walkthrough of the metrics pipeline: model-server metrics (vLLM: num_requests_running, num_requests_waiting, kv_ca
- llm-d-routerpkgeppframeworkpluginsreadmemd-at-main-llm-dllm-d-router-github: llm-d-router pkg/epp/framework/plugins/README.md (fetched Sep 2026, latest commit 950c9636, Aug 25 2026, 'consolidate discovery plugins under a single
- intelligent-inference-scheduling-with-llm-d-on-red-hat-ai-red-hat-developer: Red Hat Developer article (June 11 2026) demonstrating that OpenShift AI / Red Hat AI Inference embed llm-d's EPP by exposing it through a single LLMI
- 2025diff: Full unified diff for GIE PR #2025. Key addition in pkg/epp/util/request/headers.go: OutputInjectionHeaders = {destination-endpoint, destination-endpo
- llm-ddocsinfrastructureno-kubernetes-deploymentmd-at-main-llm-dllm-d-github: PRIMARY SOURCE: docs/infrastructure/no-kubernetes-deployment.md on llm-d/llm-d main branch (47 lines / 2.84 KB; last commit 3cb74f7761e80d8a0f66084114
- gateway-api-inference-extension-with-llm-d-router-kaito: KAITO official docs (v0.12.x, ground-truth/docs tier) describing how KAITO integrates Gateway API Inference Extension (GWIE) with 'the llm-d Router EP
- readmemd: README for llm-d-router's built-in 'file-discovery' EndpointDiscovery plugin (type 'file-discovery', interface EndpointDiscovery, disabled by default)
- tlsgo: Go source (pkg/sidecar/proxy/tls.go, llm-d-router@main): CreateSelfSignedTLSCertificate generates a 10-year self-signed RSA-4096 cert for the sidecar'
- optionsgo: Go source (pkg/sidecar/proxy/options.go, llm-d-router@main) defining the pd-sidecar CLI flags. --enable-ssrf-protection is a bool flag defaulting to f
- file-discovery-34-add-filediscovery-plugin-with-yamljson-file-loading-and-fsnoti: PRIMARY SOURCE. Merged PR #1135 (llm-d/llm-d-router), "file-discovery (3/4): add FileDiscovery plugin with YAML/JSON file loading and fsnotify watch",
- networking-for-distributed-inference-in-llm-d-llm-d: Dense llm-d engineering blog post (co-authored with NVIDIA) on the networking stack underlying P/D disaggregation -- highly relevant to non-k8s backen
- refresh-support-for-kgateway-agentgateway-gie-and-gw-api-by-danehans-pull-reques: llm-d PR #421 (danehans/solo.io, opened Oct 31 2025, targeted at llm-d v0.4/v0.5 release tracker, fixes issue #399) is llm-d's official gateway-provid
- llm-dguidesno-kubernetes-deployment-at-main-llm-dllm-d-github: PRIMARY SOURCE: full README.md of llm-d/llm-d/guides/no-kubernetes-deployment (main branch, fetched Sep 11 2026, unpinned commit). This is the actual 
- chapter-7-configuring-scheduler-settings-for-llm-inference-services-deploy-model: Red Hat OpenShift AI 3.4 docs (KServe LLMInferenceService CRD layer, not raw llm-d) on configuring EPP scheduler settings two ways: inline (router.sch
- inference-serving-with-superpowers-agentgateway-agent-connectivity-solved: Vendor blog (agentgateway, Mar 23 2026) positioning agentgateway + llm-d + Gateway API Inference Extension (GIE) as a composable inference stack: agen
- llm-d-routerpkgeppframeworkpluginsdatalayerdiscoveryfile-at-main-llm-dllm-d-rout: PRIMARY SOURCE: pkg/epp/framework/plugins/datalayer/discovery/file/README.md on llm-d/llm-d-router main branch (fetched Sep 11 2026, unpinned to a com
- v08-release-goals-mid-june-issue-1520-llm-dllm-d: Primary-source GitHub tracking issue #1520 ('v0.8 Release Goals (Mid-June)'), opened May 16 2026 by robertgshaw2-redhat, closed Jun 23 2026 as the v0.
- datalayer-package-sigsk8siogateway-api-inference-extensionpkgeppframeworkinterfa: godoc for pkg/epp/framework/interface/datalayer (the abstract interface package underlying pkg/epp/datalayer's Runtime; discovered via cross-reference
- best-of-both-worlds-cloud-native-ai-inference-at-scale-using-kserve-and-llm-d-ks: KServe blog post (post-v0.16, referenced by the Red Hat June 2026 article) providing the clearest concise layer-responsibility table for the KServe+ll
- pd-disaggregation-llm-d: llm-d v0.9 'well-lit path' quickstart for P/D disaggregation (short, links to the fuller architecture doc). Gives a concrete reference topology: 2 vLL
- releases-llm-dllm-d: llm-d/llm-d GitHub Releases page, fetched covering v0.3.0 (10 Oct 2025) through v0.9.0 (17 Aug 2026). Dated timeline: v0.3.0 (10 Oct 2025), v0.3.1 (6 
- rfc-mooncake-conductor-design-and-implementation-of-a-global-scheduler-module-fo: IMPORTANT PROVENANCE FLAG: this GitHub RFC (opened Oct 28 2025, kvcache-ai/Mooncake issue #977) was CLOSED AS 'NOT PLANNED' -- it is an unmerged, reje
- gateway-api-inference-extensionsite-srcguidesepp-configurationconfig-textmd-at-9: Historical (last-surviving, pre-deletion) version of GIE's site-src/guides/epp-configuration/config-text.md, fetched from GitHub commit 91ad0da4 (pare
- llm-d-flow-control-priority-queuing-for-shared-gpu-inference-red-hat-developer: Red Hat Developer article (Aug 27 2026) explaining llm-d's Flow Control layer via the v0.10.0 release tag, giving a durable Go source path: pkg/epp/fr
- inference-support-agentgateway-as-a-standalone-provider-issue-620-agentgatewayag: Closed agentgateway design/implementation issue (#620, Nov 2025 - Apr 2026) tracing the full origin of standalone (non-Gateway-API) EPP support, which
- gateway-api-inference-extensionsite-srcguidesstandalonemd-at-91ad0da44f64d41642d: Last surviving version of site-src/guides/standalone.md (commit 91ad0da4, the parent of deletion-commit ebbb148c dated 2026-06-30) -- the file 'Deploy
