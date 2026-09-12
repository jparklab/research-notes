---
title: 'GitHub - interlink-hq/interLink: InterLink aims to provide an abstraction
  for the execution of a Kubernetes pod on any remote resource capable of managing
  a Container execution lifecycle · GitHub'
id: github-interlink-hqinterlink-interlink-aims-to-provide-an-abstraction-for-the-ex
tags:
- llmd-router-nonk8s-backends
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:45:52.195063Z'
updated: '2026-09-12T03:37:37.618703Z'
source: https://github.com/interlink-hq/interLink
source_domain: github.com
fetched_at: '2026-09-11T19:45:52.172905Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'interLink (CNCF-hosted, v0.6.2 as of fetch) is a Virtual Kubelet provider
  plugin architecture: a Virtual Kubelet "Virtual Node" translates K8s pod execution
  requests into calls to a separate interLink API server, which dispatches to provider-specific
  sidecar plugins (Slurm, HTCondor via Apptainer/Enroot/Singularity; on-demand VMs
  via Docker/Podman; remote K8s clusters; serverless). Recent commit history (Jul
  2026) shows active work "stabilizing shadow pod networking lifecycle," indicating
  pod-network behavior for these remote-backed pods is nontrivial and still being
  hardened. Critically, the project explicitly scopes itself OUT of two things directly
  relevant to using it as an llm-d backend: (1) "Long-running Services: Persistent
  services with continuous availability requirements" are listed as Out of Scope --
  a serious mismatch since vLLM/model-serving pods are exactly long-running persistent
  services, not the batch/HPC-training jobs interLink targets; and (2) "Kubernetes
  Federation: Multi-cluster resource management and federation" is also Out of Scope,
  meaning it is not a multi-cluster answer either. In Scope use cases are HPC/GPU
  batch and hybrid-cloud workload distribution.'
utility_score: 13.0
---

[Skip to content](https://github.com/interlink-hq/interLink#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/interlink-hq/interLink) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/interlink-hq/interLink) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/interlink-hq/interLink) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/interlink-hq/interLink).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Finterlink-hq%2FinterLink) You must be signed in to change notification settings
  * [ Fork 25 ](https://github.com/login?return_to=%2Finterlink-hq%2FinterLink)
  * [ Star  129 ](https://github.com/login?return_to=%2Finterlink-hq%2FinterLink)


[**79** Branches](https://github.com/interlink-hq/interLink/branches)[**126** Tags](https://github.com/interlink-hq/interLink/tags)
Go to file
Open more actions menu
## Latest commit
[Bump golangci/golangci-lint-action from 8 to 9 (](https://github.com/interlink-hq/interLink/commit/badedec3fffe509c140ba5e945ffab15388fa792)[#508](https://github.com/interlink-hq/interLink/pull/508)[)](https://github.com/interlink-hq/interLink/commit/badedec3fffe509c140ba5e945ffab15388fa792)
Open commit detailssuccess
Aug 26, 2026
[badedec](https://github.com/interlink-hq/interLink/commit/badedec3fffe509c140ba5e945ffab15388fa792) · Aug 26, 2026
## History
[765 Commits](https://github.com/interlink-hq/interLink/commits/main/)
Open commit details
765 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [Documentation update for 0.5.1, updated in-cluster setup and test pod…](https://github.com/interlink-hq/interLink/commit/0dd542ccc526812cb277e4c2cf4471e93de0831d "Documentation update for 0.5.1, updated in-cluster setup and test pod instructions \(#433\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Jul 10, 2025  |  
|   |   |  [Bump golangci/golangci-lint-action from 8 to 9 (](https://github.com/interlink-hq/interLink/commit/badedec3fffe509c140ba5e945ffab15388fa792 "Bump golangci/golangci-lint-action from 8 to 9 \(#508\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#508](https://github.com/interlink-hq/interLink/pull/508)[)](https://github.com/interlink-hq/interLink/commit/badedec3fffe509c140ba5e945ffab15388fa792 "Bump golangci/golangci-lint-action from 8 to 9 \(#508\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Aug 26, 2026  |  
|   |   |   | May 20, 2024  |  
|   |   |   | Jul 5, 2023  |  
| [Change `Accelerator.Available` from `int` to Kubernetes quantity stri…](https://github.com/interlink-hq/interLink/commit/3601914c50c4d9be7878f63a8c34d2057eb11a3b "Change `Accelerator.Available` from `int` to Kubernetes quantity string \(#492\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@gmail.com>
Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Co-authored-by: Diego Ciangottini <dciangot@cern.ch>
Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>")  | Apr 1, 2026  |  
|  [feat: expose interLink server version via API (](https://github.com/interlink-hq/interLink/commit/e15c7a6a304c073ae29b2ee7352751cb5db2fc14 "feat: expose interLink server version via API \(#552\)

Signed-off-by: w3lld1 <42353747+w3lld1@users.noreply.github.com>")[#552](https://github.com/interlink-hq/interLink/pull/552)[)](https://github.com/interlink-hq/interLink/commit/e15c7a6a304c073ae29b2ee7352751cb5db2fc14 "feat: expose interLink server version via API \(#552\)

Signed-off-by: w3lld1 <42353747+w3lld1@users.noreply.github.com>")  | Aug 26, 2026  |  
|   |   |  [Update Go base image version to 1.26 (](https://github.com/interlink-hq/interLink/commit/0dc30a8acb242c7f3de70b2899f110316ab3e744 "Update Go base image version to 1.26 \(#512\)

Signed-off-by: Giulio <34304760+Bianco95@users.noreply.github.com>")[#512](https://github.com/interlink-hq/interLink/pull/512)[)](https://github.com/interlink-hq/interLink/commit/0dc30a8acb242c7f3de70b2899f110316ab3e744 "Update Go base image version to 1.26 \(#512\)

Signed-off-by: Giulio <34304760+Bianco95@users.noreply.github.com>")  | Apr 1, 2026  |  
| [532 stabilize shadow pod networking lifecycle and add optional tls in…](https://github.com/interlink-hq/interLink/commit/c418cc724e8c934513563624924ae23a96cbe7e5 "532 stabilize shadow pod networking lifecycle and add optional tls ingress support \(#538\)


Signed-off-by: Giulio Bianchini <giulio.bianchini@hotmail.it>")  | Jul 29, 2026  |  
|   |   | [Fix CSR certificate retrieval and pod deletion JSON parsing errors + …](https://github.com/interlink-hq/interLink/commit/2c95c0d3501a8469652915b6e015db9908df7a87 "Fix CSR certificate retrieval and pod deletion JSON parsing errors + local tests \(#466\)

✅ Certificates properly issued after CSR approval
✅ Per-node CSR isolation maintained via Common Name filtering
✅ Clean pod deletion without JSON parsing errors
✅ Unit tests and local test script available for slurm
 
Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>
Co-authored-by: Copilot Autofix powered by AI <62310815+github-advanced-security\[bot\]@users.noreply.github.com>")  | Oct 24, 2025  |  
|   |   | [Update helm chart submodule to 0.6.1](https://github.com/interlink-hq/interLink/commit/0c66b8e78363b5ae9aabac10e8d089b467beccf4 "Update helm chart submodule to 0.6.1")  | Apr 9, 2026  |  
|  [feat: expose interLink server version via API (](https://github.com/interlink-hq/interLink/commit/e15c7a6a304c073ae29b2ee7352751cb5db2fc14 "feat: expose interLink server version via API \(#552\)

Signed-off-by: w3lld1 <42353747+w3lld1@users.noreply.github.com>")[#552](https://github.com/interlink-hq/interLink/pull/552)[)](https://github.com/interlink-hq/interLink/commit/e15c7a6a304c073ae29b2ee7352751cb5db2fc14 "feat: expose interLink server version via API \(#552\)

Signed-off-by: w3lld1 <42353747+w3lld1@users.noreply.github.com>")  | Aug 26, 2026  |  
|   |   | [532 stabilize shadow pod networking lifecycle and add optional tls in…](https://github.com/interlink-hq/interLink/commit/c418cc724e8c934513563624924ae23a96cbe7e5 "532 stabilize shadow pod networking lifecycle and add optional tls ingress support \(#538\)


Signed-off-by: Giulio Bianchini <giulio.bianchini@hotmail.it>")  | Jul 29, 2026  |  
|   |   | [Fix logs forwarding: input validation, missing return bug, and isSafe…](https://github.com/interlink-hq/interLink/commit/5910f3b40ef7704a17b79a1c1d845c74c255def7 "Fix logs forwarding: input validation, missing return bug, and isSafeURL localhost regression \(#515\)

Signed-off-by: Diego Ciangottini <dciangot@cern.ch>")  | Jun 3, 2026  |  
| [532 stabilize shadow pod networking lifecycle and add optional tls in…](https://github.com/interlink-hq/interLink/commit/c418cc724e8c934513563624924ae23a96cbe7e5 "532 stabilize shadow pod networking lifecycle and add optional tls ingress support \(#538\)


Signed-off-by: Giulio Bianchini <giulio.bianchini@hotmail.it>")  | Jul 29, 2026  |  
|   |   | [https by default in installation script](https://github.com/interlink-hq/interLink/commit/ae4505ba1f40f08814c2a8792903f75fcc093a3c "https by default in installation script")  | Jul 13, 2023  |  
|   |   |   | Nov 29, 2023  |  
|   |   | [Allow to Disable CSR creation for VirtualKubelet and improve cert maa…](https://github.com/interlink-hq/interLink/commit/ad06f94b83dbf2918e2d00e7efbda0946d53bf23 "Allow to Disable CSR creation for VirtualKubelet and improve cert maangement \(#475\)

Doc included.

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Dec 10, 2025  |  
|   |   | [Fix CSR certificate retrieval and pod deletion JSON parsing errors + …](https://github.com/interlink-hq/interLink/commit/2c95c0d3501a8469652915b6e015db9908df7a87 "Fix CSR certificate retrieval and pod deletion JSON parsing errors + local tests \(#466\)

✅ Certificates properly issued after CSR approval
✅ Per-node CSR isolation maintained via Common Name filtering
✅ Clean pod deletion without JSON parsing errors
✅ Unit tests and local test script available for slurm
 
Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>
Co-authored-by: Copilot Autofix powered by AI <62310815+github-advanced-security\[bot\]@users.noreply.github.com>")  | Oct 24, 2025  |  
|   |   |  [Create GitHub Actions e2e test suite (K3s-based, no Dagger) (](https://github.com/interlink-hq/interLink/commit/7761acdc39998551cacc2a7bcfdcda305479ace2 "Create GitHub Actions e2e test suite \(K3s-based, no Dagger\) \(#514\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>
Signed-off-by: Diego Ciangottini <diego.ciangottini@gmail.com>
Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Co-authored-by: Diego Ciangottini <dciangot@cern.ch>
Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>")[#514](https://github.com/interlink-hq/interLink/pull/514)[)](https://github.com/interlink-hq/interLink/commit/7761acdc39998551cacc2a7bcfdcda305479ace2 "Create GitHub Actions e2e test suite \(K3s-based, no Dagger\) \(#514\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>
Signed-off-by: Diego Ciangottini <diego.ciangottini@gmail.com>
Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Co-authored-by: Diego Ciangottini <dciangot@cern.ch>
Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>")  | Apr 10, 2026  |  
|   |   | [Update .goreleaser.yaml to revert aarch inclusion](https://github.com/interlink-hq/interLink/commit/dfbc04cd8a3f96b78c7e5a0cacbe38b857c0d169 "Update .goreleaser.yaml to revert aarch inclusion")  | Oct 23, 2024  |  
|   |   |  [Major Linting and security fixes (](https://github.com/interlink-hq/interLink/commit/d19004bb3a3f01be2ef6a4e102266c3609e44154 "Major Linting and security fixes \(#299\)

* go linting
* fix integration tests
* lint in dagger ci tests
* update doc for 0.3.1

---------

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")[#299](https://github.com/interlink-hq/interLink/pull/299)[)](https://github.com/interlink-hq/interLink/commit/d19004bb3a3f01be2ef6a4e102266c3609e44154 "Major Linting and security fixes \(#299\)

* go linting
* fix integration tests
* lint in dagger ci tests
* update doc for 0.3.1

---------

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Oct 10, 2024  |  
|   |   |   | Jul 5, 2023  |  
|   |   |  [OpenAPI generation, doc and README rework, CNCF templating (](https://github.com/interlink-hq/interLink/commit/368bd44dcb711c009a70697502ba3e066d02a51f "OpenAPI generation, doc and README rework, CNCF templating \(#403\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")[#403](https://github.com/interlink-hq/interLink/pull/403)[)](https://github.com/interlink-hq/interLink/commit/368bd44dcb711c009a70697502ba3e066d02a51f "OpenAPI generation, doc and README rework, CNCF templating \(#403\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Apr 30, 2025  |  
|   |   |  [docs: add changelog for 0.6.1 (](https://github.com/interlink-hq/interLink/commit/b49984c0c6ee27d68e52523426bd0031b75be48c "docs: add changelog for 0.6.1 \(#519\)")[#519](https://github.com/interlink-hq/interLink/pull/519)[)](https://github.com/interlink-hq/interLink/commit/b49984c0c6ee27d68e52523426bd0031b75be48c "docs: add changelog for 0.6.1 \(#519\)")  | Apr 9, 2026  |  
|   |   |  [OpenAPI generation, doc and README rework, CNCF templating (](https://github.com/interlink-hq/interLink/commit/368bd44dcb711c009a70697502ba3e066d02a51f "OpenAPI generation, doc and README rework, CNCF templating \(#403\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")[#403](https://github.com/interlink-hq/interLink/pull/403)[)](https://github.com/interlink-hq/interLink/commit/368bd44dcb711c009a70697502ba3e066d02a51f "OpenAPI generation, doc and README rework, CNCF templating \(#403\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Apr 30, 2025  |  
|   |   |  [OpenAPI generation, doc and README rework, CNCF templating (](https://github.com/interlink-hq/interLink/commit/368bd44dcb711c009a70697502ba3e066d02a51f "OpenAPI generation, doc and README rework, CNCF templating \(#403\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")[#403](https://github.com/interlink-hq/interLink/pull/403)[)](https://github.com/interlink-hq/interLink/commit/368bd44dcb711c009a70697502ba3e066d02a51f "OpenAPI generation, doc and README rework, CNCF templating \(#403\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Apr 30, 2025  |  
|   |   | [Governance draft - to be confirmed on Dec2025](https://github.com/interlink-hq/interLink/commit/20b78bed34a8229dc4c994a9e2cf8ab9d7ca0655 "Governance draft - to be confirmed on Dec2025

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Jun 27, 2025  |  
|   |   |   | Apr 18, 2024  |  
|   |   |   | Apr 9, 2025  |  
|   |   |  [Create GitHub Actions e2e test suite (K3s-based, no Dagger) (](https://github.com/interlink-hq/interLink/commit/7761acdc39998551cacc2a7bcfdcda305479ace2 "Create GitHub Actions e2e test suite \(K3s-based, no Dagger\) \(#514\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>
Signed-off-by: Diego Ciangottini <diego.ciangottini@gmail.com>
Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Co-authored-by: Diego Ciangottini <dciangot@cern.ch>
Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>")[#514](https://github.com/interlink-hq/interLink/pull/514)[)](https://github.com/interlink-hq/interLink/commit/7761acdc39998551cacc2a7bcfdcda305479ace2 "Create GitHub Actions e2e test suite \(K3s-based, no Dagger\) \(#514\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>
Signed-off-by: Diego Ciangottini <diego.ciangottini@gmail.com>
Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Co-authored-by: Diego Ciangottini <dciangot@cern.ch>
Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>")  | Apr 10, 2026  |  
|   |   |  [v0.6x doc + update slack and sff badges (](https://github.com/interlink-hq/interLink/commit/daba78acba5d6c4609ac37b1866a0faac5102516 "v0.6x doc + update slack and sff badges \(#486\)

also security golangci lint fixes

Signed-off-by: dciangot <dciangot@cern.ch>")[#486](https://github.com/interlink-hq/interLink/pull/486)[)](https://github.com/interlink-hq/interLink/commit/daba78acba5d6c4609ac37b1866a0faac5102516 "v0.6x doc + update slack and sff badges \(#486\)

also security golangci lint fixes

Signed-off-by: dciangot <dciangot@cern.ch>")  | Mar 12, 2026  |  
|   |   |  [OpenAPI generation, doc and README rework, CNCF templating (](https://github.com/interlink-hq/interLink/commit/368bd44dcb711c009a70697502ba3e066d02a51f "OpenAPI generation, doc and README rework, CNCF templating \(#403\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")[#403](https://github.com/interlink-hq/interLink/pull/403)[)](https://github.com/interlink-hq/interLink/commit/368bd44dcb711c009a70697502ba3e066d02a51f "OpenAPI generation, doc and README rework, CNCF templating \(#403\)

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Apr 30, 2025  |  
|   |   |  [v0.6x doc + update slack and sff badges (](https://github.com/interlink-hq/interLink/commit/daba78acba5d6c4609ac37b1866a0faac5102516 "v0.6x doc + update slack and sff badges \(#486\)

also security golangci lint fixes

Signed-off-by: dciangot <dciangot@cern.ch>")[#486](https://github.com/interlink-hq/interLink/pull/486)[)](https://github.com/interlink-hq/interLink/commit/daba78acba5d6c4609ac37b1866a0faac5102516 "v0.6x doc + update slack and sff badges \(#486\)

also security golangci lint fixes

Signed-off-by: dciangot <dciangot@cern.ch>")  | Mar 12, 2026  |  
|   |   |   | Jun 9, 2025  |  
|   |   |  [Major Linting and security fixes (](https://github.com/interlink-hq/interLink/commit/d19004bb3a3f01be2ef6a4e102266c3609e44154 "Major Linting and security fixes \(#299\)

* go linting
* fix integration tests
* lint in dagger ci tests
* update doc for 0.3.1

---------

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")[#299](https://github.com/interlink-hq/interLink/pull/299)[)](https://github.com/interlink-hq/interLink/commit/d19004bb3a3f01be2ef6a4e102266c3609e44154 "Major Linting and security fixes \(#299\)

* go linting
* fix integration tests
* lint in dagger ci tests
* update doc for 0.3.1

---------

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Oct 10, 2024  |  
| View all files |  
## Repository files navigation
# Welcome to the interLink Project
interLink is an abstraction layer for executing Kubernetes pods on remote resources capable of managing container execution lifecycles.
It facilitates the development of provider-specific plugins for the [Kubernetes Virtual Kubelet interface](https://virtual-kubelet.io/), enabling resource providers to leverage Virtual Kubelet capabilities without deep Kubernetes internals knowledge.
## Architecture
The project consists of two main components:
  * **Virtual Kubelet (Virtual Node)** : Translates Kubernetes pod execution requests into remote calls to the interLink API server
  * **interLink API Server** : A modular, pluggable REST server with provider-specific plugins (sidecars) for different execution environments


## Key Features
  * **Plugin-based Architecture** : Extensible sidecar system for different remote providers
  * **Multiple Deployment Patterns** : Edge-node, in-cluster, and tunneled configurations
  * **Built-in Observability** : OpenTelemetry integration with distributed tracing and metrics
  * **Secure Communication** : TLS/mTLS encryption and authentication between components
  * **Authentication** : OAuth2 integration and bearer token support
  * **Standard Kubernetes API** : Maintains full compatibility with existing K8s tooling


interLink is hosted by the [Cloud Native Computing Foundation (CNCF)](https://cncf.io).
## Getting Started
For usage and development guides please refer to [our site](https://interlink-hq.github.io/interLink/)
## Development

```
# Build all components
make all

# Run tests (uses Dagger for containerized testing)
make test

# Generate OpenAPI specifications
make openapi

# Clean build artifacts
make clean
```

## Adopters
Please find all our current adopters [here](https://github.com/interlink-hq/interLink/blob/main/ADOPTERS.md)
## Contributing
Our project welcomes contributions from any member of our community. To get started contributing, please see our [Contributor Guide](https://github.com/interlink-hq/interLink/blob/main/CONTRIBUTING.md).
## Supported Providers
interLink supports a wide range of remote execution environments through its plugin architecture:
  * **HPC Batch Systems** : SLURM, HTCondor with Apptainer, Enroot, or Singularity containers
  * **Virtual Machines** : On-demand VMs with Docker, Podman, or other container runtimes
  * **Remote Kubernetes** : Offload workloads to external Kubernetes clusters
  * **Serverless** : Lambda-like services for event-driven container execution
  * **Custom Providers** : Extensible plugin system for any container-capable remote resource


## Use Cases
### In Scope
  * **HPC Workloads** : AI training, ML inference, scientific simulations requiring specialized hardware
  * **GPU-intensive Tasks** : Remote execution on powerful GPU resources for ML training, data analysis, rendering
  * **Batch Processing** : On-demand container execution with specific compute requirements
  * **Hybrid Cloud** : Workload distribution across multiple infrastructure providers


### Out of Scope
  * **Long-running Services** : Persistent services with continuous availability requirements
  * **Kubernetes Federation** : Multi-cluster resource management and federation


## Communications
## Resources
## License
This project is licensed under [Apache2](https://github.com/interlink-hq/interLink/blob/main/LICENSE)
## Conduct
We follow the [CNCF Code of Conduct](https://github.com/interlink-hq/interLink/blob/main/CODE_OF_CONDUCT.md)
## About
InterLink aims to provide an abstraction for the execution of a Kubernetes pod on any remote resource capable of managing a Container execution lifecycle
### Resources
### Code of conduct
### Contributing
### Stars
**129** stars
### Watchers
**5** watching
### Forks
[ forks](https://github.com/interlink-hq/interLink/forks)
## 
[ 0.6.2Latest Jul 10, 2026 ](https://github.com/interlink-hq/interLink/releases/tag/0.6.2)
## 
## Languages
  * [Go86.3%](https://github.com/interlink-hq/interLink/search?l=go)
  * [Shell10.7%](https://github.com/interlink-hq/interLink/search?l=shell)
  * [Python2.7%](https://github.com/interlink-hq/interLink/search?l=python)
  * [Makefile0.3%](https://github.com/interlink-hq/interLink/search?l=makefile)


Generated from [interTwin-eu/repository-template](https://github.com/interTwin-eu/repository-template)
You can’t perform that action at this time. 
