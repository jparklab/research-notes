---
title: 'Audited absence: classified-network LLM serving stack disclosures'
id: audited-absence-classified-network-llm-serving-stack-disclosures
tags:
- onprem-llm-inference-stacks-859099
- audited-absence
- classified-absence
created: '2026-10-23T17:25:11.477217Z'
status: draft
type: note
deprecated: false
summary: Searched for NIPRNet/SIPRNet LLM serving details; only DISA INDOPACOM R&D
  chatbot and NIPRGPT stubs found; no engine/GPU/scale disclosed.
---

# Audited absence: classified-network (NIPRNet/SIPRNet/JWICS) LLM serving stacks

Finding: no public source discloses the serving stack (engine, GPUs, scale, models) of any US classified-network LLM deployment.

Searched (Oct 2026, batch 9): "NIPRGPT OR GenAI.mil OR SIPRNet LLM deployment classified on-premise vLLM"; "CamoGPT OR GenAI.mil OR Ask Sage air-gapped classified IL6 open-weight GPUs"; plus reading of arXiv 2602.13033v2 (no ATO/IL content), ibl.ai federal blog (vendor, no named deployments), DefenseScoop.

What exists (all thin):
- DISA SIPRNet chatbot for INDOPACOM (DefenseScoop, 25 Mar 2025): R&D, in accreditation, on DISA Joint Operational Edge cloud, built with AFRL. No model/GPU/engine named.
- NIPRGPT (AFRL/DAF, June 2024): unclassified experimental; stub press text only.
- Web-search summaries (not fetched, unverified): reports that an AFRL effort put an LLM on SIPRNet in early 2025; CamoGPT reportedly able to process SIPRNet data; Ask Sage vendor claims FedRAMP High and IL5/IL6; GenAI.mil (Dec 2025) is commercial Gemini at IL5, with classified (IL6/IL7) programs reportedly announced for later.
Reliability: trade press and vendor claims; accreditation status unconfirmed.
Implication: DoD's visible path is buy (GenAI.mil, hosted commercial) plus experimental in-house; self-hosted classified internals are undisclosed.
