---
title: 'The Air-Gapped LLM Blueprint: What Egress-Free Deployments Actually Need'
id: the-air-gapped-llm-blueprint-what-egress-free-deployments-actually-need
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- practitioner-blog
- reliability-medium
created: '2026-10-23T17:23:28.687044Z'
updated: '2026-10-23T17:25:08.641419Z'
source: https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment
source_domain: tianpan.co
fetched_at: '2026-10-23T17:23:28.667403Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: Tian Pan practitioner essay (May 1 2026, partially paywalled; reliability
  medium, no named first-hand deployment). Claims teams find 5-15 unknown egress dependencies;
  no widely adopted weight signing at load time; MLBOM (CycloneDX); artifacts 10-400
  GB moved on encrypted media or data diode; every model update is a release; self-hosted
  Prometheus/Grafana/Loki/Tempo; mentions Los Alamos moving LLM stack on-prem early
  2025 (unsourced here). Cites NSA/DoD AI/ML supply chain PDF.
---

*Suggested by [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]] — cited as primary source*

[Skip to main content](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment#main)
Ask AI
The cloud AI playbook assumes one primitive that nobody writes down: outbound HTTPS. Vendor APIs, hosted judges, telemetry pipelines, model registries, vector stores, dashboard SaaS, secret managers — every one of them quietly resolves to a domain on the public internet. Pull that one cable and the stack does not degrade gracefully. It collapses.
That is the moment most teams discover their architecture has an egress dependency they never accounted for. A "small" prompt update needs to call out to a hosted classifier. The eval suite hits an LLM judge over the wire. The observability agent phones home. The model registry pulls weights from a CDN. None of it is malicious, and none of it is unusual. It is just what the cloud-native stack looks like when you stop noticing the cable.
Defense, healthcare, and financial-services deployments increasingly cannot tolerate that cable. The reasons are non-negotiable: data classification, residency rules, contractual exclusivity, lateral-movement risk, regulator-defined custody chains. Los Alamos National Laboratory moved its LLM stack on-prem in early 2025 to handle Controlled Unclassified Information, ITAR-flagged data, and Unclassified Controlled Nuclear Information. A hospital running diagnostic copilots over PHI cannot route prompts through a vendor inference endpoint. A broker-dealer governed by FINRA Rule 3110 and SEC Regulation S-P does not have a clean answer when client portfolios traverse a third-party API.
"Just self-host an open-weight model" understates the operational surface by an order of magnitude. The model is the easy part. What follows is the blueprint for everything else.
## The Egress Audit Comes First, Not the Inference Server[​](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment#the-egress-audit-comes-first-not-the-inference-server "Direct link to The Egress Audit Comes First, Not the Inference Server")
Before a single GPU is racked, the team has to answer a question most teams cannot: list every outbound network call your AI stack makes today. Not just the inference API. The hosted judge. The Hugging Face download. The prompt-monitoring SaaS. The vector database's telemetry. The OpenTelemetry collector that ships traces to a managed backend. The Slack webhook your eval pipeline pings on regression. The npm postinstall script in the SDK.
A useful exercise: run the existing stack inside a network namespace with a default-deny egress rule and watch what breaks. The breakages map almost one-for-one to the new primitives the air-gapped version has to grow. Most teams find five to fifteen distinct egress dependencies they did not know they had.
The temptation is to handle each as a one-off — a local mirror here, a configuration flag there. The discipline that actually scales is treating the egress surface as a first-class architectural concern, with an explicit list of allowed destinations (often: the empty set, or a tightly-controlled internal mirror), a CI check that fails the build when a new dependency is introduced, and a network policy that enforces the same rule in production. Without that gate, the air-gap claim erodes one quietly-added dependency at a time.
## Model Artifact Provenance Is the Hardest Problem[​](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment#model-artifact-provenance-is-the-hardest-problem "Direct link to Model Artifact Provenance Is the Hardest Problem")
Inside the boundary, the model file is no longer something you `pip install`. It is a regulated artifact whose provenance the team has to defend in an audit. Three problems compound here.
**The supply chain is poisoned by default.** Open-weight model repositories are the new target. Researchers have already demonstrated that the Hugging Face Safetensors conversion service can be compromised to hijack submitted models, and OWASP's LLM Top 10 lists supply-chain risks (LLM03:2025) as a primary class of attacks. SafeTensors format mitigates the worst of pickle-style code execution, but it does not solve provenance. There is still no widely-adopted mechanism for cryptographically signing weights and verifying that signature at load time. The team has to build that gate themselves: hash-pin every model, sign the artifact against an internal key during ingestion, and refuse to load anything whose signature does not verify.
**The dependency tree is wider than software.** A traditional SBOM tracks libraries. An AI/ML Bill of Materials (MLBOM, increasingly published in CycloneDX format) has to track the model, the tokenizer, the safety classifier, the LoRA adapters, the merged checkpoints, the quantization tooling that produced the deployed weights, the eval suite that gated the release, and the licenses attached to every link in that chain. A fine-tune of a fine-tune of a fine-tune can drag in a license clause from a base model the team never agreed to. The MLBOM is the only artifact that makes the chain auditable.
**Updates are not idempotent.** In the cloud version, a model bump is a config change. In the air-gapped version, a model bump is an artifact transfer that has to clear a signed-bundle release process, a security re-review, and a chain-of-custody log. The model weights themselves can be 10–400 GB and have to move on encrypted media or through a one-way data diode. Every update is a release; every release is a paperwork event. The team that built monthly model bumps into their roadmap discovers they shipped a process the security organization will not approve.
## The Eval Stack Has to Live Inside the Boundary[​](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment#the-eval-stack-has-to-live-inside-the-boundary "Direct link to The Eval Stack Has to Live Inside the Boundary")
The cloud AI eval pipeline almost always has a hosted dependency: an LLM judge that calls GPT-4 or Claude over the public internet, a benchmark dataset pulled from Hugging Face at runtime, a results dashboard that uploads metrics to a SaaS observability tool. None of it survives the air-gap.
Building the eval stack inside the boundary means three concrete things. The judge model has to be a model you self-host — usually a smaller, locally-deployed variant whose calibration against the cloud judge has been measured offline before the boundary closed. The Judge Reliability Harness research from RAND is a useful reference for stress-testing whether a judge's grading stays stable under prompt perturbations, which matters more when you cannot just swap in a frontier model to settle disputes. The evaluation harness — most teams adapt EleutherAI's lm-evaluation-harness or build something similar — has to ship with its benchmark data baked in, not pulled at runtime. The dashboard has to be self-hosted too, often a Grafana stack pointed at a local time-series database.
The most underappreciated piece is the ground truth. In a cloud workflow, ground-truth labels often come from human reviewers using a SaaS annotation tool. In the air-gapped workflow, the annotation tool has to live inside the boundary, which means either deploying an open-source annotator (Argilla, Doccano) on-prem or building an internal one. This is usually the longest-pole task in standing up a regulated eval pipeline, and it is the one most teams forget to scope.
## Fleet, Promotion, and the Death of `kubectl apply`[​](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment#fleet-promotion-and-the-death-of-kubectl-apply "Direct link to Fleet, Promotion, and the Death of kubectl apply")
Cloud LLM serving has converged on a Kubernetes-native pattern: KServe and llm-d on top of vLLM, with GitOps for promotion across environments. The pattern transfers to the air-gap with one critical change: the GitOps controller cannot reach back to a managed Git host. The cluster has to pull from an internal mirror, and the mirror has to be populated by an out-of-band sync that is itself a release event.
The fleet manager — Rancher Fleet, Argo CD, Flux, or a vendor's Kubernetes Fleet Manager — promotes a model the same way it promotes a service: a manifest references a model artifact by digest, the controller reconciles the desired state, the inference cluster pulls the artifact from an internal registry. What is different is that the registry is now part of the trust boundary. A leaked artifact pull from an unauthorized cluster is a data-classification incident.
The release process has to enforce this with policy, not procedure. Admission controllers verify that the model digest in the manifest matches a signature on an allowlist. Network policies prevent inference pods from making egress calls to anywhere except the local model server and the local telemetry collector. Pod security standards block sidecar injection by anything not explicitly approved. The cluster's role bindings restrict who can promote a model to production to a smaller set of humans than the cloud version, because the consequences of a misconfigured promotion are heavier.
A useful operating rule: if the cloud version of the system can ship a model bump with a single approving reviewer, the air-gapped version probably cannot. Air-gapped releases are slower by design, and the team that pretends otherwise discovers it the first time a regulator asks for the change-control evidence.
## The Hidden Egress Dependencies That Break the Air-Gap Claim[​](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment#the-hidden-egress-dependencies-that-break-the-air-gap-claim "Direct link to The Hidden Egress Dependencies That Break the Air-Gap Claim")
Even after the explicit cloud dependencies are gone, three hidden categories quietly reintroduce egress.
**SDKs and frameworks.** Modern Python packages ship background telemetry, auto-update checks, and "anonymous usage statistics" by default. Disabling all of them is a per-package exercise that has to be repeated every dependency upgrade. A `pip install` inside the boundary is a misnomer if the package's first action is to call home; the local PyPI mirror has to be the only resolvable index, and the build has to fail closed if it tries to resolve elsewhere.
**Embeddings.** A lot of "RAG inside the boundary" architectures quietly call a hosted embedding API for the document corpus, or for incoming queries, because the team picked a vector database whose default embedding integration is a SaaS endpoint. The embedding model has to be self-hosted alongside the generative model, and the architectural diagram should show it explicitly. If the diagram has an arrow leaving the boundary labeled "embeddings," the air-gap claim is fictional.
**Telemetry and observability.** The cloud-native observability story (Datadog, Honeycomb, New Relic) does not survive the boundary. Replacing it means a self-hosted Prometheus + Grafana + Loki + Tempo stack, plus a log retention policy that complies with the data-classification rules of the environment. The retention policy is non-trivial: classified inference logs may themselves be classified, which means the observability backend lives at the same classification level as the inference cluster. Routing them to a lower-classification analyst dashboard is a cross-domain transfer event, not a Grafana data source.
## What the Cloud Stack Took for Granted[​](https://tianpan.co/blog/2026/05/01/air-gapped-llm-blueprint-egress-free-deployment#what-the-cloud-stack-took-for-granted "Direct link to What the Cloud Stack Took for Granted")
The architectural realization is the one most teams resist: the cloud AI stack is not a generic platform that happens to be hosted in the cloud. It is a platform whose every layer assumes egress as a free primitive — for updates, for evaluation, for telemetry, for secrets, for identity, for everything. Pulling the cable forces each of those layers to grow primitives the cloud version never needed: a model registry that is also a sovereign artifact store, an eval harness that is also a self-contained judge, a fleet manager that is also an air-tight release pipeline, an observability stack that respects the same data-classification rules as the data plane.
The teams that succeed treat the air-gap not as a constraint to work around but as the design requirement that determines the architecture from day one. The teams that bolt it on after a regulator's letter discover that "make this air-gapped" is not a configuration flag. It is a rebuild of half the platform, on a deadline, by people whose Kubernetes runbooks suddenly do not apply.
The good news: the primitives now exist. Self-hosted model serving (vLLM, llm-d, KServe) is production-grade. Open-weight models (Llama, Mistral, Qwen, the long tail of fine-tunes) are within striking distance of frontier quality for most enterprise tasks. MLBOM tooling (CycloneDX, AIRS) is maturing. Air-gapped Kubernetes patterns are well understood. The blueprint is not theoretical anymore. What is missing is the discipline to treat egress as a deliberate architectural choice rather than a default, and the willingness to plan for the operational primitives the cloud version hides under a managed-service abstraction. The teams that build that discipline now will own the regulated AI stack of the next decade. The teams that do not will keep discovering, one egress dependency at a time, that "self-hosted" was always a marketing term, not an architecture.
For members
### The rest is for members.
Members get the rest — the frameworks, decisions, and reasoning behind every idea I publish in public.
  * Complete essays, including the parts I keep off the public archive
  * Working frameworks with the trade-offs spelled out
  * Early access to new pieces before they go public

Sign in to continue
Cancel anytime · One subscription, every essay
## References
  * <https://blog.dreamfactory.com/government-and-defense-air-gapped-llm-data-access-dreamfactory>
  * <https://www.getdynamiq.ai/post/mastering-llm-security-an-air-gapped-solution-for-high-security-deployments>
  * <https://thesoogroup.com/blog/sandboxed-ai-deploying-llms-airgapped>
  * <https://datacendia.com/learn/air-gapped-ai-deployment/>
  * <https://iternal.ai/best-ai-air-gapped-environments>
  * <https://cyclonedx.org/capabilities/mlbom/>
  * <https://www.paloaltonetworks.com/cyberpedia/what-is-an-ai-bom>
  * <https://www.wiz.io/academy/ai-security/ai-bom-ai-bill-of-materials>
  * <https://genai.owasp.org/llmrisk/llm032025-supply-chain/>
  * <https://media.defense.gov/2026/Mar/04/2003882809/-1/-1/0/AI_ML_SUPPLY_CHAIN_RISKS_AND_MITIGATIONS.PDF>
  * <https://www.wiz.io/academy/ai-security/malicious-ai-models>
  * <https://www.secondfront.com/resources/blog/understanding-dod-cloud-computing-impact-levels/>
  * <https://terrazone.io/zero-trust-dual/>
  * <https://edenlab.io/blog/hipaa-compliant-ai-best-practices>
  * <https://www.mindstudio.ai/blog/local-ai-regulated-professionals-compliance>
  * <https://blog.premai.io/ai-data-residency-requirements-by-region-the-complete-enterprise-compliance-guide/>
  * <https://www.mdpi.com/2079-9292/15/1/56>
  * <https://github.com/EleutherAI/lm-evaluation-harness>
  * <https://www.rand.org/pubs/tools/TLA4547-1.html>
  * <https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm>


Let's stay in touch and follow me for more
## Related articles
[ 9 min read Sovereignty Collapse: Logging Where Your Prompt Actually Went Application logs say the request went to eu-west-1. The provider routed it through US and Singapore during a failover. Build the per-request sovereignty path… insider llm ](https://tianpan.co/blog/2026/04/26/prompt-sovereignty-path-token-routing-audit)[ 12 min read Where Production LLM Pipelines Leak User Data: PII, Residency, and the Compliance Patterns That Hold Up Most LLM data leaks don't come from the model — they come from unredacted RAG chunks, verbatim prompt logs, and injectable retrieval pipelines. A practical… insider llm ](https://tianpan.co/blog/2026/04/09/llm-pipeline-privacy-pii-data-residency-compliance)[ 9 min read The Agent Scratch Directory: The Unowned Filesystem PII Surface Nobody Inventoried Agent workers accumulate ephemeral filesystem state — extracted PDFs, transcribed audio, cached attachments — on disks no one classified. The fix is naming the… insider ai-agents ](https://tianpan.co/blog/2026/05/10/agent-scratch-directory-unowned-pii-asset-register)[ 10 min read The Copyright Exposure in AI-Generated Content: A Risk Framework for Engineering Teams LLM outputs can reproduce verbatim training data, and the output liability can land with you — not the model provider. A practical engineering framework for… insider ai ](https://tianpan.co/blog/2026/04/17/copyright-exposure-ai-generated-content-risk-framework)[ 9 min read Warm Pools and Cold Truths: The Hidden Latency Floor of Serverless LLM Inference Scaling GPU inference to zero converts a steady dollar cost into a spiky latency cost hidden in the p99 tail. Here is the break-even math and the mitigation… insider llm ](https://tianpan.co/blog/2026/05/17/warm-pools-cold-truths-serverless-llm-cold-start)
