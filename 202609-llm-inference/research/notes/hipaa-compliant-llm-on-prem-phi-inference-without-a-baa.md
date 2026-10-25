---
title: 'HIPAA-Compliant LLM: On-Prem PHI Inference Without a BAA'
id: hipaa-compliant-llm-on-prem-phi-inference-without-a-baa
tags:
- onprem-llm-inference-stacks-859099
- healthcare
- hipaa
- baa
- vendor-blog
- reliability-low
- reference-architecture
created: '2026-10-23T17:23:22.303518Z'
updated: '2026-10-23T17:25:27.942135Z'
source: https://stribog.com/blog/hipaa-compliant-self-hosted-llm-phi-on-prem-inference-baa
source_domain: stribog.com
fetched_at: '2026-10-23T17:23:22.281673Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'RELIABILITY: vendor blog (Stribog, sovereign-AI/Kubernetes consultancy;
  28 Jun 2026, upd 6 Aug 2026); no named customer or scale disclosed; architecture
  prescriptive, not evidence of deployment. Useful as the regulatory-driver argument
  and a reference pattern: a BAA is a liability contract not architecture (OpenAI
  and Azure OpenAI will sign BAAs; so BAA availability is NOT the real blocker) ->
  real drivers are party-count (BAA dependency graph), CLOUD Act jurisdiction, and
  OCR audit evidence. Maps 45 CFR 164.312 safeguards (access control, audit controls,
  integrity, authentication, transmission security) plus 164.502(b) minimum necessary
  and 164.514 de-identification to design: vLLM on GPU node pool with 2 replicas,
  weights on encrypted read-only volume, mTLS everywhere, OIDC/RBAC PHI-aware Envoy-style
  gateway that fails closed if audit sink unreachable, append-only hash-chained Postgres
  audit store with RLS, 6-year retention per 164.316(b)(2). Stack: vLLM, Kubernetes,
  Vault/External Secrets, OpenTelemetry/Prometheus/Grafana/Loki, Qdrant/pgvector.'
---

[Skip to content](https://stribog.com/blog/hipaa-compliant-self-hosted-llm-phi-on-prem-inference-baa#main-content)
Sovereign AI
# HIPAA-Compliant LLM: On-Prem PHI Inference Without a BAA
How a covered entity runs a HIPAA-compliant LLM on-premises: PHI never leaves your perimeter, no third-party BAA to manage, every inference logged for audit.
Stribog·28 Jun 2026·18 min read·Updated 6 Aug 2026
Ask a healthcare CISO whether they can use a large language model on patient data and you will usually get one of two answers, both wrong. The first is "no, HIPAA forbids it" — which is false; HIPAA is technology-neutral and says nothing about transformer models. The second is "yes, we signed a BAA with the vendor" — which is the more dangerous answer, because a signed Business Associate Agreement is a contract, not an architecture. It allocates liability after a breach. It does not stop the breach, and it does not, by itself, satisfy a single one of the Security Rule's technical safeguards.
The real question for a covered entity is structural: where does the PHI physically go when a clinician asks the model to summarize a discharge note, and who can be compelled to produce it later? If the answer involves a token leaving your network and being processed on infrastructure owned by a US-parent company, you have not removed risk — you have signed a paper that relocates it. The alternative, increasingly tractable in 2026, is to keep the inference inside the perimeter entirely: run the model yourself, on hardware you control, and never create a business associate relationship in the first place.
This is the healthcare-specific implementation of self-hosted inference. The general production architecture — GPU scheduling, vLLM, gateways — is covered in our walkthrough of [running vLLM at production grade on Kubernetes](https://stribog.com/blog/self-hosted-llm-kubernetes-vllm-production-deployment). What follows is narrower: the de-identification boundaries, the prompt/response retention rules, and the access controls a covered entity can actually hand to an OCR auditor. There is regulation. There are control mappings. There is config. If you have been told to "add AI" to a clinical product but cannot send PHI to a hosted API, this is the engineering path that holds up under audit.
## The BAA Gap: A Signed Agreement Is Not the Same as Compliance
The Business Associate Agreement exists because of a specific HIPAA mechanic. Under §164.502(e) and §164.308(b), a covered entity may disclose PHI to a vendor that performs a function on its behalf only if that vendor signs a BAA contractually binding it to the Security and Privacy Rules. The major model vendors will now sign one: OpenAI executes a BAA on request and routes covered traffic to zero-data-retention endpoints, and Azure OpenAI is HIPAA-eligible under Microsoft's BAA. So the contract is available. The problem is what the contract does and does not do.
A BAA is a liability instrument. It says that if the business associate mishandles your PHI, they are now on the hook for it under the regulation. What it does not do is reduce the number of parties that hold your patients' data, or shorten the jurisdictional reach over that data. In 2026 the practical reality is a chain: every party that creates, receives, maintains, or transmits PHI needs a signed BAA — the model provider, the cloud provider underneath it, and any inference middleware in between. You are not managing one agreement; you are managing a dependency graph of them, and your compliance is only as strong as the weakest link you do not directly control.
There is also a jurisdiction problem the BAA cannot paper over. When PHI is processed on infrastructure operated by a US-parent provider, that data sits within reach of the CLOUD Act regardless of which region it nominally lives in. For most domestic providers this is a manageable risk; for cross-border health data, or for an entity that has promised patients their records never leave a defined boundary, it is a structural exposure the contract simply does not address. The same reasoning drives the European regulatory pressure we mapped in [NIS2, DORA, and the EU AI Act](https://stribog.com/blog/nis2-dora-eu-ai-act-self-hosted-kubernetes-compliance): when the legal requirement is about *where the data is and who can reach it*, a vendor promise is not a control. Self-hosting closes the gap by deleting the business associate from the diagram. No disclosure, no BAA, no chain.
## What HIPAA Actually Requires of an AI System That Touches PHI
HIPAA's Security Rule does not name AI, but its technical safeguards under §164.312 apply to any electronic system that handles ePHI — an inference server is no exception. Five safeguards define the bar, each mapped to architecture, not a policy document; [ISO 42001](https://stribog.com/blog/iso-42001-ai-management-system-self-hosted-inference-evidence) is the separate, AI-specific management system alongside them. Treating safeguards as architecture, not paperwork, is the point of self-hosting.
  * **Access Control (§164.312(a)(1))** — unique user identification, automatic logoff, and encryption/decryption of ePHI. The inference gateway must know *which clinician* made each request and enforce that only authorized users reach the model.
  * **Audit Controls (§164.312(b))** — hardware, software, and procedural mechanisms that record and examine activity in systems containing ePHI. Every inference is an event that must be recorded.
  * **Integrity (§164.312(c)(1))** — protect ePHI from improper alteration or destruction. The audit record, once written, must be tamper-evident.
  * **Person or Entity Authentication (§164.312(d))** — verify that a user is who they claim to be before any PHI is processed.
  * **Transmission Security (§164.312(e)(1))** — integrity controls and encryption for ePHI in transit, including between the gateway and the model pod.


Two more rules shape the design even though they live outside §164.312. The minimum-necessary standard (§164.502(b)) requires that you disclose only the PHI needed for the task — which, for an LLM, means the prompt should carry the smallest clinically sufficient context, not a whole record dumped in for convenience. And de-identification (§164.514(a)–(b)) offers an escape hatch: data stripped of the eighteen Safe Harbor identifiers, or certified de-identified by Expert Determination, is no longer PHI and falls outside the rule entirely. Where you draw that de-identification line is the central architectural decision, and it is one you can only make freely when the data has not already left your network.
## The De-Identification Boundary: Where PHI Stops and Inference Begins
Every clinical AI design has a line on the diagram where PHI either stops or continues. Place it well and you get strong privacy guarantees with full clinical utility; place it badly and you either leak identifiers or starve the model of context. The two Safe Harbor and Expert Determination paths give you the legal definitions; the architecture gives you the enforcement point.
Safe Harbor de-identification removes eighteen specified identifier categories — names, geographic subdivisions smaller than a state, all date elements finer than a year, contact numbers, record numbers, biometric identifiers, and so on. It is mechanical and defensible, but it is lossy: stripping dates and ages can remove exactly the temporal signal a model needs to reason about a disease course. Expert Determination instead has a qualified statistician certify that re-identification risk is "very small," which permits retaining more structure (shifted-but-consistent dates, age bands) at the cost of a formal analysis and documentation. For a self-hosted deployment, the powerful position is that you do not have to choose under duress: because identified PHI never leaves the perimeter, you can run identified inference for the workloads that need it and reserve de-identification for the data flows that genuinely cross a boundary — research exports, vendor analytics, model evaluation sets.
Architecturally, the de-identification boundary belongs at the gateway, not inside the model server, for the same reason [RAG retrieval](https://stribog.com/blog/qdrant-pgvector-self-hosted-vector-database-on-prem-rag) and rate-limiting belong there: the model pod should stay stateless and unaware of policy. The gateway inspects the request, applies the minimum-necessary filter, and — only on flows configured to cross a boundary — invokes a de-identification pass before forwarding. Inside the perimeter, the boundary is a no-op: the request goes straight to vLLM with full context, and the audit store records that it was an in-perimeter identified request so the lineage is explicit. This is the same defense-in-depth posture as protecting data in use with [confidential computing and hardware attestation](https://stribog.com/blog/confidential-computing-kubernetes-remote-attestation-tee) — the PHI is bounded not by a promise but by an enforced control point.
Click to zoomThe covered entity's legal perimeter is the only boundary. The gateway enforces authentication, minimum-necessary, and the de-identification line. The edge to a third-party model API is severed by design — PHI never crosses it, so no business associate relationship is ever created.
## Implementation: vLLM Behind a PHI-Aware Gateway
The inference engine itself is unremarkable and that is the point — vLLM on a GPU node pool, model weights on an encrypted read-only volume, two replicas for availability. The HIPAA-specific work lives in the gateway and the surrounding controls, not in the model server. The gateway terminates mTLS, authenticates the clinician against the organization's OIDC provider, enforces RBAC for who may invoke which model, applies the minimum-necessary filter, emits the audit event, and only then forwards to vLLM. The model never sees an unauthenticated request and never makes a policy decision.
Transmission security (§164.312(e)) is satisfied by mTLS on every hop, including gateway-to-pod inside the cluster — the audit team should be able to point at a service mesh policy that makes plaintext PHI in transit impossible, not improbable. Encryption at rest (§164.312(a)(2)(iv)) covers both the model volume and the audit store: LUKS-encrypted block storage or an encrypted PVC class, with keys held in your own KMS. This is the same credential and key discipline described in [secrets management with External Secrets and Vault](https://stribog.com/blog/kubernetes-secrets-management-external-secrets-operator-vault) — the model and audit stores are just two more consumers of short-lived, audited credentials.
yamlCopy

```
# phi-gateway-policy.yaml — mounted into the inference gateway
# Every request is authenticated, authorized, tagged, and audited
# BEFORE it is allowed to reach the vLLM pod. The model pod itself is
# only reachable from the gateway (NetworkPolicy enforced).
auth:
  oidc:
    issuer: "https://idp.hospital.internal"
    audience: "clinical-inference"
    # §164.312(d): authenticate the natural person, not a shared service account
    require_claims: ["sub", "clinician_npi", "department"]
  # §164.312(a)(2)(iii): automatic logoff — tokens are short-lived
  max_token_age: 900s

authorization:
  # §164.312(a)(1): only these roles may invoke PHI-context models
  rbac:
    - role: attending_physician
      allow_models: ["clinical-summarizer", "note-assistant"]
      phi_context: identified          # in-perimeter identified inference allowed
    - role: research_analyst
      allow_models: ["cohort-explorer"]
      phi_context: deidentified         # this flow MUST cross the de-id boundary
    - role: default
      deny: true                        # deny-by-default

minimum_necessary:
  # §164.502(b): strip fields not needed for the task before the prompt is built
  drop_unless_requested: ["ssn", "full_address", "guarantor", "insurance_id"]

deidentification:
  # No-op for identified in-perimeter flows; engaged only when phi_context=deidentified
  engine: safe_harbor          # or "expert_determination" with a documented ruleset
  on_context: deidentified

audit:
  # §164.312(b): emit BEFORE forwarding — fail closed if the sink is unreachable
  required: true
  fail_closed: true
  sink: "https://audit-store.phi-audit.svc:8443/v1/events"
```

PHI-aware gateway policy (Envoy-style): OIDC auth, RBAC, minimum-necessary header tagging, mandatory audit emission before forwarding to vLLM
The `fail_closed: true` line is load-bearing. If the audit sink is unreachable, the gateway must refuse the request rather than serve it un-logged. An inference that happened but was not recorded is, from an auditor's perspective, an inference that escaped the controls — and §164.312(b) does not have a "best effort" clause. Failing closed converts a logging outage into a clean denial instead of a silent evidence gap. For PHI, the audit record is part of the primary path, not optional telemetry that may degrade quietly.
## Audit Controls (§164.312(b)): Logging Every Inference for the Auditor
The audit store is the artifact you hand to OCR. Every inference produces one immutable record: who asked, what model and version answered, the prompt and completion (or a cryptographic digest plus a sealed copy if size or sensitivity demands it), the PHI context classification, and a microsecond timestamp. The store is append-only — writes succeed for the append role; no grantable application role may UPDATE/DELETE — enforced by GRANT/REVOKE plus FORCE RLS policies that allow INSERT (and SELECT only so drivers can use INSERT … RETURNING), not merely by application convention. §164.316(b)(2) requires six-year retention of Security Rule documentation; the Rule sets no explicit retention period for audit logs themselves, so most covered entities align inference logs to the same six-year horizon as a defensible default set by their risk analysis — and the store must hold at least that horizon with its integrity intact.
Integrity (§164.312(c)) is the difference between a log and evidence. Chaining each record to the hash of its predecessor — with row_hash computed in a BEFORE INSERT trigger over a session-stable canonical serialization the writer cannot supply — turns the store into tamper-evident evidence: content changes or broken predecessor links fail a verification pass that recomputes hashes with the same serialization; whole-tail truncation still needs an external high-water mark. The same observability pipeline that ships operational telemetry can carry these events — see [self-hosted observability with OpenTelemetry, Prometheus, Grafana, and Loki](https://stribog.com/blog/self-hosted-observability-opentelemetry-prometheus-grafana-loki-kubernetes) — but the audit sink itself must be the integrity-protected, access-restricted store, not a general logging backend that ops can mutate.
sqlCopy

```
-- PHI inference audit store: append-only + hash-chained for tamper-evidence.
CREATE TABLE phi_inference_audit (
  id            BIGSERIAL PRIMARY KEY,
  occurred_at   TIMESTAMPTZ(6) NOT NULL DEFAULT clock_timestamp(),
  clinician_sub TEXT  NOT NULL,           -- §164.312(d) authenticated identity
  clinician_npi TEXT  NOT NULL,
  model_name    TEXT  NOT NULL,
  model_version TEXT  NOT NULL,           -- pin to a reproducible config hash
  phi_context   TEXT  NOT NULL            -- 'identified' | 'deidentified'
                CHECK (phi_context IN ('identified','deidentified')),
  prompt_digest BYTEA NOT NULL,           -- SHA-256 of the prompt
  output_digest BYTEA NOT NULL,           -- SHA-256 of the completion
  prev_hash     BYTEA NOT NULL DEFAULT ''::bytea,  -- trigger overwrites; '' = head
  row_hash      BYTEA NOT NULL DEFAULT ''::bytea   -- trigger overwrites on INSERT
);

-- §164.312(c): bind mutation. Create the table as a dedicated non-login owner
-- (audit_owner), grant INSERT+SELECT to audit_writer (SELECT needed for
-- INSERT ... RETURNING used by most drivers/ORMs), revoke the owner's own
-- DML, and force RLS so the owner is policy-bound. Superusers still bypass
-- everything — defend that with off-DB chain-head anchoring, not SQL.
ALTER TABLE phi_inference_audit OWNER TO audit_owner;
REVOKE UPDATE, DELETE, TRUNCATE ON phi_inference_audit FROM audit_owner;
GRANT INSERT, SELECT ON phi_inference_audit TO audit_writer;
GRANT USAGE ON SEQUENCE phi_inference_audit_id_seq TO audit_writer;
GRANT SELECT ON phi_inference_audit TO audit_verifier;
ALTER TABLE phi_inference_audit ENABLE ROW LEVEL SECURITY;
ALTER TABLE phi_inference_audit FORCE ROW LEVEL SECURITY;
CREATE POLICY audit_append ON phi_inference_audit
  FOR INSERT TO audit_writer WITH CHECK (true);
CREATE POLICY audit_writer_select ON phi_inference_audit
  FOR SELECT TO audit_writer USING (true);  -- INSERT ... RETURNING
CREATE POLICY audit_read ON phi_inference_audit
  FOR SELECT TO audit_verifier USING (true);

-- Chain hashes in BEFORE INSERT; never trust client-supplied hash columns
-- (DEFAULTs exist only so INSERT may omit them; the trigger always overwrites).
-- Own this SECURITY DEFINER function as superuser or a BYPASSRLS migration role:
-- under FORCE RLS a non-bypass owner cannot see prior rows, so every insert
-- would falsely look like a genesis row. Hash payload uses UTC to_char — not
-- occurred_at::text — so DateStyle/TimeZone cannot make a clean chain fail
-- verification under a different session GUC.
CREATE OR REPLACE FUNCTION phi_audit_chain() RETURNS trigger
LANGUAGE plpgsql SECURITY DEFINER SET search_path = pg_catalog, public AS $$
DECLARE prior BYTEA;
BEGIN
  SELECT p.row_hash INTO prior FROM phi_inference_audit p
  ORDER BY p.id DESC LIMIT 1;
  NEW.prev_hash := COALESCE(prior, ''::bytea);  -- empty bytea = chain-head sentinel
  NEW.row_hash := sha256(convert_to(
    NEW.id::text || '|' ||
    to_char(NEW.occurred_at AT TIME ZONE 'UTC',
            'YYYY-MM-DD"T"HH24:MI:SS.US"Z"') || '|' ||
    NEW.clinician_sub || '|' ||
    NEW.clinician_npi || '|' || NEW.model_name || '|' || NEW.model_version || '|' ||
    NEW.phi_context || '|' || encode(NEW.prompt_digest, 'hex') || '|' ||
    encode(NEW.output_digest, 'hex') || '|' || encode(NEW.prev_hash, 'hex'),
    'UTF8'));
  RETURN NEW;
END;
$$;
CREATE TRIGGER phi_audit_chain_bi BEFORE INSERT ON phi_inference_audit
  FOR EACH ROW EXECUTE FUNCTION phi_audit_chain();

-- Integrity verification: flag broken predecessor links and content/hash mismatch.
-- Genesis = no lower-id prior (not id = 1 — sequences continue after wipe/rollback);
-- genesis rows must carry the ''::bytea chain-head sentinel. Mid-row deletion shows
-- up as prev_hash ≠ live prior. (Run nightly; alert on any non-empty result. Also
-- reconcile count(*)/max(id) to an external high-water mark for whole-tail wipe.)
SELECT a.id
FROM phi_inference_audit a
LEFT JOIN LATERAL (
  SELECT p.row_hash
  FROM phi_inference_audit p
  WHERE p.id < a.id
  ORDER BY p.id DESC
  LIMIT 1
) p ON true
WHERE (p.row_hash IS NOT NULL AND a.prev_hash IS DISTINCT FROM p.row_hash)
   OR (p.row_hash IS NULL AND a.prev_hash IS DISTINCT FROM ''::bytea)
   OR a.row_hash IS DISTINCT FROM sha256(convert_to(
        a.id::text || '|' ||
        to_char(a.occurred_at AT TIME ZONE 'UTC',
                'YYYY-MM-DD"T"HH24:MI:SS.US"Z"') || '|' ||
        a.clinician_sub || '|' ||
        a.clinician_npi || '|' || a.model_name || '|' || a.model_version || '|' ||
        a.phi_context || '|' || encode(a.prompt_digest, 'hex') || '|' ||
        encode(a.output_digest, 'hex') || '|' || encode(a.prev_hash, 'hex'),
        'UTF8'));
```

Append-only, hash-chained audit table — tamper-evident inference evidence for §164.312(b)/(c). Writers get INSERT plus SELECT for RETURNING; verification recomputes hashes and flags broken chain links.
Note what is not stored in plaintext: the prompt and completion are SHA-256 digests in the hot table, with full text in a separately encrypted, access-gated blob store keyed by the digest. The high-volume audit index stays queryable and small; clinical text is protected at rest and reachable only through an authorized export path. An auditor verifying a specific incident requests the sealed copy by digest; routine integrity checks never need to touch PHI. That separation is minimum-necessary applied to your own audit team.
Click to zoomEach §164.312 technical safeguard maps to a specific component, not a policy document. This is the table you hand an auditor: control on the left, the artifact that satisfies it on the right.
## Access Controls and Authentication: Minimum Necessary, Enforced
Access control under §164.312(a) is not a login screen; it is the requirement that the system can prove, per request, that an authorized and identified person invoked it for a permitted purpose. The unique user identification requirement means shared service accounts are disqualifying — every inference must trace to a natural person via the JWT `sub` and, in clinical settings, an NPI or equivalent credential. The gateway enforces this before the model is reachable, and the audit store records it, so access control and audit controls reinforce each other: the same identity that authorizes the request is the identity written into the tamper-evident chain.
Two clinical-specific requirements deserve explicit handling. Automatic logoff (§164.312(a)(2)(iii)) maps to short token lifetimes — fifteen minutes is a defensible default — so an unattended workstation cannot keep issuing inferences indefinitely. And emergency access, the "break-glass" provision (§164.312(a)(2)(ii)), must exist for clinical-continuity reasons: a physician must be able to reach the system in an emergency even if normal authorization is degraded. Break-glass is not an exception to logging; it is the most heavily logged path in the system. A break-glass invocation writes a distinguished audit record, triggers an immediate access-review alert, and is reconciled after the fact. Designing the emergency path to be loud rather than silent is what keeps it from becoming the hole every control eventually leaks through.
Runtime enforcement closes the loop. Network policy restricts the vLLM pod so it accepts connections only from the gateway, and runtime detection — the kind described in [Falco and Tetragon for Kubernetes runtime threat detection](https://stribog.com/blog/falco-tetragon-kubernetes-runtime-threat-detection-ebpf) — watches for the anomalies a static policy cannot anticipate: an unexpected process reading the model volume, an egress attempt from an inference pod, a shell spawned in a container that should never have one. In a PHI environment those are not curiosities; they are potential breach events, and detecting them in real time is part of the §164.308 administrative safeguard story that surrounds the technical controls.
## The Exit Ramp: No Vendor, No Model, No Jurisdiction Lock-In
The optionality argument for self-hosted clinical inference is sharper than the generic cost case, because in healthcare the lock-in is not only commercial — it is regulatory. When your PHI flows through a vendor's model, switching models means renegotiating a BAA, re-running a security assessment of the new processor, and updating your breach-notification posture to reflect a new party in the chain. When you own the inference layer, switching from one open-weight model to another is a volume swap and a deployment update. The model is a dependency you can replace on a maintenance window; the vendor relationship you never created cannot lock you in.
There is a breach-notification dimension that is easy to miss. Under the Breach Notification Rule, an impermissible disclosure of PHI by a business associate is your reportable event — the covered entity carries the notification obligation. Every business associate you add is another party whose incident becomes your 60-day clock and your reputational exposure. Removing the business associate does not just simplify contracts; it removes an entire category of breach you would otherwise have to monitor, insure against, and potentially report on behalf of someone else's mistake. The data that never left cannot be breached at a vendor, because there is no vendor.
## The Long Game: An Audit Posture That Survives Model Churn
Clinical AI will churn faster than clinical regulation. Models that feel essential this year will be obsolete in eighteen months; the Security Rule, by contrast, changes on a decade cadence, and when it does change it tightens. The December 2024 Notice of Proposed Rulemaking to strengthen the HIPAA Security Rule signals the direction: OCR proposed removing the long-standing distinction between "required" and "addressable" implementation specifications and making nearly all of them mandatory, with encryption of ePHI at rest and in transit required subject to narrow exceptions. Architectures that treated encryption and audit as optional because they were "addressable" are exactly the ones that proposal is aimed at.
A self-hosted inference platform that already encrypts the model and audit volumes, already authenticates every request to a natural person, and already produces a tamper-evident audit chain is not scrambling to meet that proposed bar — it is over it. That is what designing for the long game buys: the control plane is built to the strictest reading of the rule, so regulatory tightening is a documentation exercise rather than a re-architecture. The model underneath can be replaced indefinitely; the perimeter, the audit chain, and the access controls persist. The broader case for owning the systems you depend on — rather than renting them and inheriting their constraints — is the [sovereignty thesis](https://stribog.com/thesis) this practice is built on.
The honest summary is that a HIPAA-compliant LLM is less an AI problem than a control-plane problem. The inference engine is a solved component; the work is the boundary you draw around it, the identity you bind to every request, and the evidence you can produce on demand. A covered entity that runs the model itself does not need to trust a vendor's compliance posture, manage a chain of BAAs, or hope a hosted endpoint was configured for zero retention. The [capabilities we bring](https://stribog.com/capabilities) to this are the GPU and Kubernetes design, the gateway and audit architecture, and the de-identification boundaries — the same regulated-data discipline behind our [sovereign AI work](https://stribog.com/capabilities/sovereign-ai). The PHI stays inside the walls. The auditor gets a clean chain. The model is replaceable. That is the posture worth building.
§FAQ/Common questions
## Frequently asked
Does signing a BAA with OpenAI or Azure make our LLM use HIPAA compliant?+
A BAA is necessary if you disclose PHI to a vendor, but it is not sufficient for compliance. The BAA is a liability contract that binds the business associate to the Security and Privacy Rules; it does not, by itself, satisfy any §164.312 technical safeguard, and it does not remove the data from the vendor's infrastructure or from CLOUD Act jurisdiction. You still owe access control, audit controls, integrity, authentication, and transmission security on your side, plus management of the full BAA chain (model provider, cloud provider, any middleware). Self-hosting avoids the disclosure entirely, so no BAA is needed because no business associate ever receives the PHI.
Can we just de-identify the data and use a cloud LLM API instead?+
Yes, and for some workloads that is the right answer — properly de-identified data (Safe Harbor's eighteen identifiers removed, or Expert Determination certified) is no longer PHI and falls outside HIPAA. The catch is that de-identification is lossy: stripping dates, ages, and geographic detail removes clinical signal the model often needs, and de-identification must happen before the network boundary, which means a robust, audited pipeline you have to trust on every request. On-prem inference lets you reserve de-identification for flows that genuinely cross a boundary (research, analytics) while running identified inference in-perimeter where clinical context matters — turning a forced trade-off into a deliberate choice.
What exactly must the §164.312(b) audit log contain, and how long do we keep it?+
Audit controls require recording and examining activity in systems that hold ePHI. For inference that means, per event: the authenticated identity that made the request, the model name and version (pinned to a reproducible configuration), the prompt and completion (full text in an encrypted store, with a digest in the queryable index), the PHI-context classification, and a high-precision timestamp. The record must be tamper-evident — hash-chaining each row to its predecessor satisfies the §164.312(c) integrity requirement. §164.316(b)(2) mandates six-year retention of Security Rule documentation, not a literal audit-log retention rule; covered entities typically keep inference audit logs for the same six years as a risk-analysis default, with integrity preserved across that window.
Is on-premises inference required by HIPAA, or is it just one option?+
HIPAA is technology-neutral and does not mandate on-premises anything; a cloud deployment with a proper BAA and correct configuration can be compliant. On-prem is an architectural choice that makes compliance structurally easier: it removes the business associate (and its BAA, breach exposure, and jurisdiction), keeps PHI inside your legal perimeter, and lets you enforce the §164.312 safeguards directly rather than relying on a vendor's attestations. For covered entities that have promised data residency, operate across borders, or want to minimize the parties holding PHI, on-prem is the lower-risk path even though the regulation does not compel it.
How does the 2025 HIPAA Security Rule update affect this architecture?+
The December 2024 NPRM proposes to strengthen the Security Rule by removing the 'required' versus 'addressable' distinction and making nearly all implementation specifications mandatory, including encryption of ePHI at rest and in transit with limited exceptions, alongside stronger documentation and risk-analysis duties. A self-hosted design that already encrypts the model and audit volumes, authenticates every request to a named person, and maintains a tamper-evident audit chain already meets the proposed bar — so the change becomes a documentation update rather than a re-architecture. Confirm specifics against the final rule and your counsel, since the proposal may shift before adoption.
hipaa compliant llmself-hosted llm healthcare PHI on-premiseson-prem LLM HIPAA BAA third-party gapprivate LLM patient data residency auditHIPAA AI inference logging 164.312vLLM healthcare regulated deployment 2026
Further reading
  * [ISO 42001 for self-hosted inference: clause to artefact](https://stribog.com/blog/iso-42001-ai-management-system-self-hosted-inference-evidence)
  * [GPU and VRAM Sizing for Self-Hosted LLM Inference](https://stribog.com/blog/self-hosted-llm-hardware-gpu-vram-sizing-sovereign-inference-capacity)
  * [Self-hosted coding assistants: keep proprietary source in-house](https://stribog.com/blog/self-hosted-coding-llm-continue-tabby-vllm-sovereign-developer-assistant)
  * [Self-hosted LLM inference on Kubernetes with vLLM](https://stribog.com/blog/self-hosted-llm-kubernetes-vllm-production-deployment)
  * [LiteLLM as an MCP Gateway: Sovereign AI Data Residency](https://stribog.com/blog/litellm-self-hosted-mcp-model-gateway-ai-data-residency)
  * [Confidential computing: protecting data-in-use with hardware TEEs](https://stribog.com/blog/confidential-computing-kubernetes-remote-attestation-tee)
  * [NIS2, DORA, and the EU AI Act on self-hosted Kubernetes](https://stribog.com/blog/nis2-dora-eu-ai-act-self-hosted-kubernetes-compliance)
  * [EU AI Act High-Risk Systems: An On-Prem Compliance Path](https://stribog.com/blog/eu-ai-act-high-risk-ai-systems-onprem-inference-compliance)
  * [Secrets management with External Secrets and Vault](https://stribog.com/blog/kubernetes-secrets-management-external-secrets-operator-vault)
  * [Falco and Tetragon: runtime threat detection on Kubernetes](https://stribog.com/blog/falco-tetragon-kubernetes-runtime-threat-detection-ebpf)
  * [Self-hosted observability for the audit pipeline](https://stribog.com/blog/self-hosted-observability-opentelemetry-prometheus-grafana-loki-kubernetes)
  * [On-prem RAG retrieval: Qdrant vs pgvector for regulated corpora](https://stribog.com/blog/qdrant-pgvector-self-hosted-vector-database-on-prem-rag)
  * [Sovereign AI: self-hosted inference service deep-dive](https://stribog.com/capabilities/sovereign-ai)
  * [The sovereignty thesis: own the systems you depend on](https://stribog.com/thesis)


Executive Briefing
## Thirty minutes to clarify your infrastructure risk
Walk us through your vendor footprint and regulatory constraints. We will tell you honestly where sovereignty creates leverage — and where it does not. No pitch deck. No obligation.
