---
title: Interim report — classified-airgap-onprem-practice-vs-mechanics
id: interim-report-classified-airgap-onprem-practice-vs-mechanics
tags:
- onprem-llm-inference-stacks-859099
- locus-classified-airgap-onprem-practice-vs-mechanics
created: '2026-10-23T17:56:56.424199Z'
status: draft
type: interim
deprecated: false
summary: Air-gap mechanics are vendor-documented and consistent; organisational practice
  and classified stacks are unevidenced (audited absence); operations claims are practitioner
  hypotheses
---

# Interim report: classified-airgap-onprem-practice-vs-mechanics

**Locus question:** What can be said rigorously about air-gapped and classified LLM serving beyond vendor mechanics, and what exactly is missing from the public record?
**Flavor:** convergent

## What the corpus already said
The vault holds precise, uncontested vendor mechanics (NVIDIA NIM, Red Hat AI Inference 3.4, NVIDIA licence system, vLLM telemetry docs), a set of SEO/practitioner essays on air-gap operations, one single-author reference build, one academic isolation paper, and an audited-absence note on classified networks. No named-organisation air-gapped deployment with disclosed scale exists. Source budget: 0 of 2 new fetches spent. Re-fetching the OpenShift AI and NSA/DoD PDFs was not needed, because the Red Hat disconnected guide is already in the vault in full and the NSA PDF is only cited second-hand.

## What the sources say (all read in full from vault notes)

**NVIDIA NIM air-gap guide** [[air-gap-deployment-nvidia-nim-for-llm-and-vlm]] (vendor docs, NIM LLM 2.1.4, reliability high for mechanics). Two phases: on a connected host run `download-to-cache -p <profile-hash>` then `create-model-store`; with `NIM_MODEL_PATH` at a local path nothing downloads, while a remote URI still needs network on first deploy. Do not set `NGC_API_KEY` or `HF_TOKEN` in the air-gapped phase. On Kubernetes every Helm-referenced image must be mirrored (init containers, sidecars, Helm tests, GPU Operator), with `airgap-bundle.sh` helpers. Profiles are per-GPU, so a hardware change can mean a new download (per [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]]).

**Red Hat AI Inference 3.4 disconnected guide** [[deploy-the-standalone-red-hat-ai-inference-container-in-a-disconnected-environme]] (vendor docs). A bastion host runs the mirror registry; `oc-mirror` v2 mirrors the AI Inference image plus NFD Operator and NVIDIA GPU Operator. Models ship as OCI "modelcar" images, validated ones under `registry.redhat.io/rhelai1`, at "10-100 GB depending on model size and applied quantization"; alternatively model files go on NFS or other persistent volumes. Verbatim: "Currently, only NVIDIA CUDA AI accelerators are supported for OpenShift Container Platform in disconnected environments."

**NVIDIA License System guide** [[license-system-user-guide-nvidia-docs]] (vendor docs). "Because a DLS instance is fully disconnected from the NVIDIA Licensing Portal, you must download licenses ... and upload them to the instance manually." Node-locked file licences for air-gapped clients arrived in vGPU 15.0; for NLS 3.4+ licence-server files are valid three years; clusters max nine DLS instances; vGPU 18+ requires DLS 3.4+. **Caveat:** this guide is vGPU-oriented. That it governs NIM or AI Enterprise licensing is an analyst inference, not something the guide says. The "bare-metal/container needs no licence server; unlicensed vGPU for Compute drops to idle after 20 minutes" claim is from the task brief; I could not locate the 20-minute figure in the guide text, so treat it as unverified here.

**vLLM usage stats** [[usage-stats-collection-vllm]] (project docs). "vLLM collects anonymous usage data by default"; fields include GPU type/count, CPU, model architecture, a UUID. Opt-out: `VLLM_NO_USAGE_STATS=1`, `DO_NOT_TRACK=1`, or a config file. Companion switches `HF_HUB_OFFLINE=1` and `HF_HUB_DISABLE_TELEMETRY=1` are cited in [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]]. The tianpan essay [[the-air-gapped-llm-blueprint-what-egress-free-deployments-actually-need]] states "Most teams find five to fifteen distinct egress dependencies they did not know they had" (practitioner estimate, no measurement, no named deployment).

**Practitioner/SEO essays** (reliability medium-low). The tianpan blueprint: weights 10-400 GB moved on encrypted media or a one-way data diode; MLBOM in CycloneDX; self-hosted Prometheus + Grafana + Loki + Tempo; "Air-gapped releases are slower by design"; its framing that "every update is a release". It mentions Los Alamos moving its LLM stack on-prem in early 2025 with no source. Its summary in the vault records "no widely adopted weight signing at load time" (single-author claim, not a survey). The vDF.ai page [[air-gapped-llm-models-hardware-serving-vdf-ai]] (vendor marketing) states "The overlooked cost is model refresh - without a controlled offline update pipeline, enclaves end up running year-old models" and that an enclave "has no way to accept a click-through licence at runtime." The chain-of-custody recipe (sha256sum manifest, GPG detached signature, verify inside the gap, data diode) is from [[the-definitive-guide-to-air-gapped-ai-deployment-with-no-internet-in-2026-discre]] (SEO guide, labelled). The monthly-healthcare to quarterly-defense update cadence is unsourced vendor assertion: omit or label it.

**Licence-compatibility angle** [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]] (analyst blog, opinionated, argues against NVIDIA's fee): Apache-2.0 (gpt-oss, Qwen3-235B-A22B, Mistral Small 3.2) is "the licence you can hand to an accreditor without a cover memo"; Llama 4 incorporates the AUP by reference (can change after certification); NVIDIA Open Model License is revocable on guardrail circumvention; Command A open weights are CC-BY-NC. Gemma's remote restriction was also asserted there. Attribute to the analyst; it is a legal reading, not counsel.

**Burn-After-Use / SMTA paper** [[burn-after-use-for-preventing-data-leakage-through-a]] (arXiv preprint, not peer-reviewed): 55 infrastructure-level attack tests, 92% defence success; BAU averages 76.75% on ephemeral-context attempts over 72 iterations; abstract highlights "residual risks from credential misconfiguration and observability pipelines" and a measured "Tenant Logging Leakage Rate". Synthetic attack suite, not field data, but it independently supports the point that shared logging is the soft spot.

**Single-author reference build** [[github-wu840407enterprise-airgapped-llm-production-reference-architecture-for-se]] (GitHub, reliability medium): Dell R740, 2x Quadro RTX 6000 (Turing), vLLM serving Qwen3-Coder-30B-A3B AWQ, Open WebUI, Postgres, Redis, Nginx, AD/LDAPS; self-reported "field-tested" and 80-120 tok/s single-user. Usable with provenance; not an organisational deployment.

**Nutanix** [[nutanix-enterprise-ai-faqs]] vs [[introducing-nutanix-enterprise-ai-a-simple-way-to-deploy-llms-and-secure-endpoin]]. FAQ: "Can Nutanix Enterprise AI be run in a dark-site/air-gapped environment? Yes. The dark site bundle is located on the Nutanix support portal requiring a login and license entitlement." Launch blog: "The edge, on-premises, or air-gapped use cases will have to find other solutions". Same vendor, neither independent; the blog sentence sits in a passage about public-cloud GenAI services and may not describe the product. Unresolved.

**Audited absence** [[audited-absence-classified-network-llm-serving-stack-disclosures]]: searched "NIPRGPT OR GenAI.mil OR SIPRNet LLM deployment classified on-premise vLLM", "CamoGPT OR GenAI.mil OR Ask Sage air-gapped classified IL6 open-weight GPUs", arXiv 2602.13033v2, ibl.ai, DefenseScoop. Found only: DISA SIPRNet chatbot for INDOPACOM (DefenseScoop, 25 Mar 2025; R&D, in accreditation, no model/GPU/engine named); NIPRGPT (AFRL, June 2024, unclassified); unverified web summaries on CamoGPT and GenAI.mil (Gemini at IL5, Dec 2025). Cross-reference: the baseline report already records GovTech Singapore running TGI "on-premise for classified workloads", the most concrete public case, but TGI was archived in March 2026 so that stack is stale.

## Evidence synthesis
The mechanics are consistent across NVIDIA, Red Hat and the vLLM project: air-gapping is a staging problem (connected-side pull, offline-side load) plus an exhaustive mirroring problem (every image, operator and sidecar), and it works identically in shape across stacks. Differences are in what the offline side must carry: NIM adds a licence server option and per-GPU profiles; Red Hat adds a mirror registry and OpenShift and currently supports only CUDA; DIY vLLM carries nothing extra but bears all the packaging labour.

What is missing is practice. Every source describing how organisations actually operate air-gapped (refresh cadence, accreditation, scale, staffing) is vendor marketing, SEO content or a single-author repo. The three issues vendors underplay all come from this weaker tier: model refresh as the recurring cost (vDF.ai, vendor, and tianpan's "every update is a release"), absent weight-signing at load (a single essay's assertion, plausible but unmeasured), and logging pipelines as the leak path (the one claim with independent, if synthetic, empirical support from the SMTA paper). The Nutanix pair shows why vendor air-gap claims need verification: one vendor, two incompatible sentences.

## Committed position
What can be stated rigorously is the vendor-documented mechanics: they are consistent, specific and cross-validated by the vLLM and Hugging Face switches, and they can be reported as fact attributed to the owning vendor. What cannot be stated is anything about how real organisations, least of all classified ones, run these systems: no named air-gapped LLM deployment with disclosed scale exists in the corpus, and no US classified-network serving stack is disclosed after targeted searches (this is a finding, not a gap in effort). The best-supported insight is that the air-gap cost sits in operations rather than serving: egress discovery (estimate: 5-15 hidden dependencies), model refresh through a signed-bundle release, licence acceptability (Apache-2.0 avoids per-version legal review) and shared logging. These operational claims rest on practitioner and vendor assertion and should be written as "practitioners and vendors report", with the SMTA paper as the only independent support for the logging point. The Nutanix contradiction should be left unresolved, and GovTech Singapore's TGI (now archived) should be flagged as the only named case.

- **Position:** Air-gapped LLM serving is mechanically well-documented but empirically unevidenced at the organisational level; the report should present vendor procedures as procedures, treat operations claims (refresh, signing, logging) as practitioner hypotheses, and record the absence of named or classified deployments as a finding.
- **Confidence:** high on the mechanics and on the absence finding as of Oct 2026 search; medium-low (about 40-50%) that the operational claims (year-old models, no weight signing, 5-15 egress dependencies) hold generally.
- **Boundary conditions:** Applies to public sources reachable in this vault as of 2026-10-23. Classified programmes may have mature practice that is simply unpublished; absence of disclosure is not absence of practice.
- **What would change this position:** a named agency, lab or bank publishing GPU count, engine and model for an enclave deployment (e.g., a Los Alamos or national lab technical report); an accreditation document (ATO or IL5/IL6 package) naming a serving stack; a measured survey of enclave model age; or evidence that signed-model schemes (e.g., Sigstore model signing) are in routine air-gap use.
- **Evidence weight:** 3 vendor primary docs and 1 project doc (strong for mechanics), 1 arXiv preprint (synthetic, supports logging), 1 single-author repo, 4 SEO/practitioner essays (operations claims, lowest tier), 2 Nutanix vendor pages (contradictory), 1 audited-absence note. 0 independent organisational case studies.

## Open questions
- Is the 20-minute unlicensed-vGPU idle behaviour documented in the NVIDIA guide? Not located in the vault text.
- Is the NSA/DoD AI/ML supply-chain guidance PDF (cited by tianpan) retrievable, and does it recommend model signing?
- Does Nutanix's dark-site bundle documentation exist beyond the login-gated portal?
- Does any national lab publish a technical report on its on-prem LLM stack (Los Alamos claim unsourced)?

## Sources
1. [[air-gap-deployment-nvidia-nim-for-llm-and-vlm]]
2. [[deploy-the-standalone-red-hat-ai-inference-container-in-a-disconnected-environme]]
3. [[license-system-user-guide-nvidia-docs]]
4. [[usage-stats-collection-vllm]]
5. [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]]
6. [[the-air-gapped-llm-blueprint-what-egress-free-deployments-actually-need]]
7. [[the-definitive-guide-to-air-gapped-ai-deployment-with-no-internet-in-2026-discre]]
8. [[air-gapped-llm-models-hardware-serving-vdf-ai]]
9. [[github-wu840407enterprise-airgapped-llm-production-reference-architecture-for-se]]
10. [[nutanix-enterprise-ai-faqs]]
11. [[introducing-nutanix-enterprise-ai-a-simple-way-to-deploy-llms-and-secure-endpoin]]
12. [[burn-after-use-for-preventing-data-leakage-through-a]]
13. [[audited-absence-classified-network-llm-serving-stack-disclosures]]
