## Run directives

Auto-selected for this run in step 1; binding wherever they adjust a default in your prompt. Absent instructions here leave your prompt's defaults untouched.

- register: advocate
- inference depth: deep

### Register posture

ADVOCATE register. The report defends ONE thesis, named early and
argued throughout. Every section either advances the thesis or
disarms an objection to it. A steel-man treatment of the strongest
opposing case is MANDATORY: state the objection at full strength,
then answer it with evidence. Commitment is maximal; hedges belong
only on unverified secondhand specifics, never on the thesis or its
supporting chain.

### Domain notes

Sourcing: primary sources are the GitHub repos and docs of llm-d (llm-d/llm-d, llm-d-inference-scheduler, llm-d-kv-cache-manager, routing sidecar) and kubernetes-sigs/gateway-api-inference-extension — source code, design proposals under docs/proposals, issues, merged PRs, and release notes; academic literature is thin and secondary. Evidence norms: code at HEAD and merged PRs outrank design docs, which outrank vendor blogs and conference talks; flag any claim that depends on an unmerged proposal. Recency window: last 12-18 months — llm-d and GIE APIs changed quickly (InferencePool v1 GA, EPP plugin/data-layer refactors), so prefer the newest release and date every API-shape claim.

### Inference depth

DEEP inference. Explicit inference chains are licensed WITH
provenance stated: when no source states X but sourced claims A and B
jointly imply it, say so in exactly that shape, citing A and B. Never
present an inference as a sourced fact; the citation checker verifies
number-bearing sentences pair-by-pair and an inference dressed as a
quote or finding will be flagged. Audited absences (what the record
should contain but does not) are reportable findings.
