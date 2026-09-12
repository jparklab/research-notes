# Scaffold — llmd-router-nonk8s-backends-54b774 (PRIVATE — never in final report)

## User Prompt (VERBATIM — gospel)

> I would like to use llm-d components as much as possible, but would like to customize router so that it can support backends other than k8s pods. Research the strategy

## Run config

- vault_tag: `llmd-router-nonk8s-backends-54b774`
- query_file_path: `research/runs/llmd-router-nonk8s-backends-54b774/query.md`
- scale gear: `full`
- modality: **synthesize** (with a compare sub-component over candidate strategies)
- final report path: `research/notes/final_report_llmd-router-nonk8s-backends-54b774.md`
- citation format: vault default (no wrapper contract)
- terminal sections: none mandated by wrapper

## Modality classification rationale

The user asks for "the strategy" — a defended, actionable recommendation on how to maximize reuse of llm-d
components (inference scheduler / EPP, Gateway API Inference Extension, KV-cache indexer, prefix-aware
scorers, disaggregated P/D sidecar, etc.) while customizing the router layer to target backends that are not
Kubernetes pods (e.g. bare-metal vLLM hosts, VMs, Slurm jobs, external/managed endpoints, other clusters).
This is a synthesize task: a thesis about which extension seam(s) to use, evidence chains from the code and
design docs, and a committed recommendation — with a comparison of alternative strategies (fork EPP vs.
plug a custom datastore/discovery source vs. use standalone mode vs. use Envoy/other proxy with EPP as ext-proc
vs. model non-pod backends as Kubernetes objects such as EndpointSlices/selectorless Services/virtual pods).

## Tier rationale

Tier `full`, format `argumentative`. The query is a design-strategy question whose answer lives in source
code, design proposals, and issue threads rather than in any single authoritative doc; it requires comparing
several competing strategies (k8s-object shims vs. EPP plugin/fork vs. standalone EPP vs. custom router on
llm-d libraries) and committing to one with a roadmap — a defended thesis with trade-offs, not a bounded lookup.

## Wrapper requirements

- None supplied (no research/prompt.txt, no wrapper_contract.json).
- Save final report to `research/notes/final_report_llmd-router-nonk8s-backends-54b774.md`.
