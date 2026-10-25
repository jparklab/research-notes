---
title: llm-d/docs/infrastructure/no-kubernetes-deployment.md at main · llm-d/llm-d
  · GitHub
id: llm-ddocsinfrastructureno-kubernetes-deploymentmd-at-main-llm-dllm-d-github
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- no-kubernetes
- architecture
- file-discovery
created: '2026-09-11T20:48:12.462606Z'
updated: '2026-09-15T04:55:23.828145Z'
source: https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/no-kubernetes-deployment.md
source_domain: github.com
fetched_at: '2026-09-11T20:48:12.441264Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE: docs/infrastructure/no-kubernetes-deployment.md on llm-d/llm-d
  main branch (47 lines / 2.84 KB; last commit 3cb74f7761e80d8a0f660841147f3eb5d78908a7,
  PR #1917 docs: fold Traffic Control and Autoscaling into Foundations, merged Jun
  24 2026 -- this file itself was last touched incidentally by a docs reorg, not a
  content rewrite, so treat Jun 24 2026 as a lower bound on currency rather than the
  date of the described design). This is the short companion architecture/rationale
  doc the no-Kubernetes blog and the deployment guide both point to. States the motivating
  environments explicitly: HPC schedulers (Slurm, LSF) that launch workers dynamically,
  Ray-based stacks running workers as actors, bare-metal inference farms, and single-workstation
  development setups. Gives the exact request-flow diagram: client -> Envoy listener
  :8081 -> ext_proc gRPC :9002 (EPP picks endpoint, sets header) -> ORIGINAL_DST cluster
  -> reads x-gateway-destination-endpoint from EPP response -> <address>:<port> of
  the vLLM worker chosen by the EPP -- this is the literal HTTP header name (x-gateway-destination-endpoint)
  Envoy uses to learn the EPP''s routing decision, a concrete wire-protocol detail
  not stated in the blog post. Explicitly lists what is OUT OF SCOPE without Kubernetes:
  InferenceObjective-driven FlowControl, the InferenceModelRewrite model-name rewriter,
  and PodMonitor-based Prometheus discovery (Prometheus metrics on --metrics-port
  itself still work; only the PodMonitor CRD-based scrape-target discovery is unavailable)
  -- this is a tighter, more precise list than the blog''s vaguer ''a few features''
  framing, and specifically calls FlowControl a casualty (the blog did not mention
  FlowControl as affected). Confirms the plugin set, weights, and scheduling profile
  ''match the Optimized Baseline... so routing behaviour is identical to the Kubernetes-based
  deployment.'' Links directly to the file-discovery plugin source at pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go
  (the underlying .go implementation file, distinct from the README already captured
  in a sibling note).'
utility_score: 18.0
---

*Suggested by [[no-kubernetes-no-problem-llm-d-now-runs-anywhere-llm-d]] — architecture and design rationale doc linked from the no-Kubernetes blog post*

[Skip to content](https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/no-kubernetes-deployment.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/no-kubernetes-deployment.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/no-kubernetes-deployment.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/no-kubernetes-deployment.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/blob/main/docs/infrastructure/no-kubernetes-deployment.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 761 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# no-kubernetes-deployment.md
Copy path
More file actions
More file actions
## Latest commit
and
[docs: fold Traffic Control & Autoscaling into Foundations (](https://github.com/llm-d/llm-d/commit/3cb74f7761e80d8a0f660841147f3eb5d78908a7)[#1917](https://github.com/llm-d/llm-d/pull/1917)[)](https://github.com/llm-d/llm-d/commit/3cb74f7761e80d8a0f660841147f3eb5d78908a7)
Open commit details
Jun 24, 2026
[3cb74f7](https://github.com/llm-d/llm-d/commit/3cb74f7761e80d8a0f660841147f3eb5d78908a7) · Jun 24, 2026
## History
[History](https://github.com/llm-d/llm-d/commits/main/docs/infrastructure/no-kubernetes-deployment.md)
Open commit details
History
47 lines (34 loc) · 2.84 KB
/
# no-kubernetes-deployment.md
Copy path
## File metadata and controls
  * Preview
  * 

47 lines (34 loc) · 2.84 KB
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# No-Kubernetes Deployment
llm-d's reference deployment runs on Kubernetes — workers are managed by Kubernetes `Deployments`, the EPP discovers them through an `InferencePool`, and the platform handles networking and lifecycle. Many environments don't have a Kubernetes control plane, though: HPC schedulers like Slurm or LSF launch workers dynamically, Ray-based stacks run workers as actors, bare-metal inference farms operate without K8s, and a single workstation with a couple of GPUs is often enough for development.
The No-Kubernetes path runs the same routing stack — the llm-d EPP, Envoy, and one or more model servers — directly as host processes or containers. The EPP gets its endpoint inventory from a YAML file on disk via the [file-discovery plugin](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go) instead of watching an `InferencePool` over the Kubernetes API; everything else (EPP plugin set, scoring, Envoy ext_proc, vLLM arguments) is unchanged.
Important
Without Kubernetes, some pieces of the llm-d stack are out of scope: `InferenceObjective`-driven FlowControl, the `InferenceModelRewrite` model-name rewriter, and `PodMonitor`-based Prometheus discovery. Scoring, prefix-cache affinity, saturation-based admission, and Prometheus metrics on `--metrics-port` all work; see the [parity caveats](https://llm-d.ai/blog/running-llm-d-without-kubernetes#parity-with-the-kubernetes-native-llm-d-deployment) for the full list.
## Deploy
See the [no-Kubernetes deployment guide](https://github.com/llm-d/llm-d/blob/main/guides/no-kubernetes-deployment) for manifests and step-by-step deployment.
## Architecture
Traffic flows the same way as the optimized-baseline path:

```
client -> Envoy listener :8081
       -> ext_proc gRPC :9002 (EPP picks endpoint, sets header)
       -> ORIGINAL_DST cluster
       -> reads x-gateway-destination-endpoint from EPP response
       -> <address>:<port> of the vLLM worker chosen by the EPP

```

The EPP's datastore is populated entirely from `endpoints.yaml`. With `watchFile: true` the file is hot-reloaded on every atomic rewrite — adding, removing, or relabelling a worker takes effect without restarting the EPP. The plugin set, weights, and scheduling profile match the [Optimized Baseline](https://github.com/llm-d/llm-d/blob/main/docs/well-lit-paths/foundations/optimized-baseline.md), so routing behaviour is identical to the Kubernetes-based deployment.
## Further Reading
  * [`file-discovery` plugin source](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go)
  * ["No Kubernetes? No Problem"](https://llm-d.ai/blog/running-llm-d-without-kubernetes) — full background on the design


You can’t perform that action at this time. 
