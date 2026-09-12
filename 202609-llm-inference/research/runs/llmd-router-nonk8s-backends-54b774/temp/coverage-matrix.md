## Coverage Matrix — query phrase → atomic item mapping

| Query phrase (verbatim) | Mapped atomic item(s) | Scope check | Gap? |
|---|---|---|---|
| "use llm-d components" | Sub-Q1 (component inventory); Entity: llm-d; Heading 1 | OK — all llm-d components, not only the router | No |
| "as much as possible" | Scope condition 1 (maximize reuse, minimize divergence); Sub-Q7 (reuse level comparison) | OK | No |
| "customize router" | Sub-Q2, Sub-Q3, Sub-Q6; Entity: llm-d router; Scope condition 2 | OK — covers scheduler/EPP AND proxy data plane | No |
| "so that it can support backends" | Sub-Q5 (per-backend requirements); Sub-Q6 (strategies) | OK | No |
| "backends other than k8s pods" | Entity: backends other than k8s pods; Sub-Q5; Scope conditions 3-4 | OK — broad (VMs, bare metal, HPC, remote clusters, external APIs) and both deployment readings (router in-cluster vs. router outside k8s) | No |
| "k8s pods" | Entity: Kubernetes pods; Sub-Q2 (where pod assumption is baked in) | OK | No |
| "Research the strategy" | Sub-Q6, Sub-Q7, Sub-Q8; required_formats (recommendation, comparison table, roadmap); Headings 6-8 | OK — committed recommendation + roadmap + risks | No |
| (implicit) upstream dependency GIE | Entity: Gateway API Inference Extension; Sub-Q4; Heading 4 | OK — the router's k8s coupling is inherited from GIE, so it must be covered | No |
