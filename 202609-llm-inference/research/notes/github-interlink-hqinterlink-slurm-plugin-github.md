---
title: GitHub - interlink-hq/interlink-slurm-plugin · GitHub
id: github-interlink-hqinterlink-slurm-plugin-github
tags:
- llmd-router-nonk8s-backends
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:46:54.153906Z'
updated: '2026-09-15T04:55:23.796644Z'
source: https://github.com/interlink-hq/interlink-slurm-plugin
source_domain: github.com
fetched_at: '2026-09-11T19:46:54.134103Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'interlink-slurm-plugin is the Slurm sidecar implementation for interLink:
  it submits Slurm batch jobs that run the pod containers via Singularity/Apptainer
  (configurable SingularityPath/SingularityPrefix), exports pod ConfigMaps/Secrets
  as mounted files, and supports HostPath volumes. Directly answers the "is the virtual
  pod reachable" question: the plugin ships a Tsocks config option (TsocksPath, TsocksLoginNode)
  described as enabling "proxy networking," documented as "your window to the external
  world" via SSH to a designated login node, and the docs state this is "only implemented
  for the Slurm sidecar at the moment." This confirms Slurm-backed virtual pods do
  not get native, directly-routable cluster-network IP addresses -- network access
  is asymmetric and egress-oriented, tunneled outward through an SSH proxy to a login
  node, not inbound-routable from the Kubernetes pod network. This is a hard blocker
  for using interLink/Slurm as a live routing target for an inference gateway, which
  needs to open a connection INTO the backend, not just let the backend reach out.'
utility_score: 10.0
---

[Skip to content](https://github.com/interlink-hq/interlink-slurm-plugin#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/interlink-hq/interlink-slurm-plugin) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/interlink-hq/interlink-slurm-plugin) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/interlink-hq/interlink-slurm-plugin) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/interlink-hq/interlink-slurm-plugin).
/ **[interlink-slurm-plugin](https://github.com/interlink-hq/interlink-slurm-plugin) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Finterlink-hq%2Finterlink-slurm-plugin) You must be signed in to change notification settings
  * [ Fork 8 ](https://github.com/login?return_to=%2Finterlink-hq%2Finterlink-slurm-plugin)
  * [ Star  4 ](https://github.com/login?return_to=%2Finterlink-hq%2Finterlink-slurm-plugin)


[**66** Branches](https://github.com/interlink-hq/interlink-slurm-plugin/branches)[](https://github.com/interlink-hq/interlink-slurm-plugin/tags)
Go to file
Open more actions menu
## Latest commit
[Remove Unicode patching for 085-secret-envfrom.yaml](https://github.com/interlink-hq/interlink-slurm-plugin/commit/a7746143276cab94452a9b6b295e320b71135703)
Open commit detailssuccess
Aug 26, 2026
[a774614](https://github.com/interlink-hq/interlink-slurm-plugin/commit/a7746143276cab94452a9b6b295e320b71135703) · Aug 26, 2026
## History
[241 Commits](https://github.com/interlink-hq/interlink-slurm-plugin/commits/main/)
Open commit details
241 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [Add K3s-based e2e test suite (no Dagger) (](https://github.com/interlink-hq/interlink-slurm-plugin/commit/5fde818a6131b8e73d701302f1792c459f8b1a29 "Add K3s-based e2e test suite \(no Dagger\) \(#139\)

* Initial plan

* Add K3s-based e2e test scripts and workflow without Dagger

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/e9d03db8-e30a-416f-86a2-abf34403115b

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>")[#139](https://github.com/interlink-hq/interlink-slurm-plugin/pull/139)[)](https://github.com/interlink-hq/interlink-slurm-plugin/commit/5fde818a6131b8e73d701302f1792c459f8b1a29 "Add K3s-based e2e test suite \(no Dagger\) \(#139\)

* Initial plan

* Add K3s-based e2e test scripts and workflow without Dagger

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/e9d03db8-e30a-416f-86a2-abf34403115b

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>")  | Apr 5, 2026  |  
|   |   |   | Apr 2, 2024  |  
| [Handle SLURM TIMEOUT (TO) and OOM states for graceful pod rescheduling](https://github.com/interlink-hq/interlink-slurm-plugin/commit/0cb7f04c5143c992a704a4e71cd3308c22731d89 "Handle SLURM TIMEOUT \(TO\) and OOM states for graceful pod rescheduling

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/56fd89ba-609b-462d-b2ec-e08cb6375ed9")  | Mar 26, 2026  |  
| [Add system-info endpoint with sinfo -s information](https://github.com/interlink-hq/interlink-slurm-plugin/commit/fc7513426ce73b078adee3be5b4b7fa60b1dff41 "Add system-info endpoint with sinfo -s information

- Add /system-info endpoint that provides SLURM cluster health status
- Include sinfo -s output showing partition summary with node counts
- Add SinfoPath configuration field with environment variable override
- Return JSON response with cluster connectivity status and summary
- Support both health monitoring and node status annotation use cases
- Integrate sinfo -s call into StatusHandler for empty requests

The response includes:
- Service status and timestamp
- SLURM connectivity status
- sinfo -s output with partition summary \(A/I/O/T node counts\)
- Error information if SLURM commands fail

When StatusHandler receives empty pod list, return sinfo -s output
for node annotations compatibility.

🤖 Generated with \[Claude Code\]\(https://claude.ai/code\)

Co-Authored-By: Claude <noreply@anthropic.com>
Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Jun 23, 2025  |  
|   |   |  [Fix e2e tests: add slurmctld to Dockerfile (](https://github.com/interlink-hq/interlink-slurm-plugin/commit/6f18bcc7f40f2e7972de6d504133823dc775c2f5 "Fix e2e tests: add slurmctld to Dockerfile \(#143\)

* Initial plan

* Fix Dockerfile and startup.sh to include slurmctld for e2e tests

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/e67acbc0-d57b-421e-9fd6-966420d3af8e

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* Add timeout warning to slurmctld readiness wait loop

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/e67acbc0-d57b-421e-9fd6-966420d3af8e

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>")[#143](https://github.com/interlink-hq/interlink-slurm-plugin/pull/143)[)](https://github.com/interlink-hq/interlink-slurm-plugin/commit/6f18bcc7f40f2e7972de6d504133823dc775c2f5 "Fix e2e tests: add slurmctld to Dockerfile \(#143\)

* Initial plan

* Fix Dockerfile and startup.sh to include slurmctld for e2e tests

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/e67acbc0-d57b-421e-9fd6-966420d3af8e

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* Add timeout warning to slurmctld readiness wait loop

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/e67acbc0-d57b-421e-9fd6-966420d3af8e

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>")  | Apr 14, 2026  |  
| [add probe/readiness checks and documentation for state machine](https://github.com/interlink-hq/interlink-slurm-plugin/commit/34b61a6437c347058e0759a56217cd3b51dd881e "add probe/readiness checks and documentation for state machine

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Sep 17, 2025  |  
|   |   |  [refactor: change from GID to UID following RFC](https://github.com/interlink-hq/interlink-slurm-plugin/commit/6eba90ab3f3ebf9280c5908995e3bce750b975e5 "refactor: change from GID to UID following RFC #58

This commit refactors the implementation to use UID \(User ID\) instead of
GID \(Group ID\) to match the requirements in RFC #58:
https://github.com/interlink-hq/interlink-slurm-plugin/discussions/58

Key changes:
- Changed DefaultGID → DefaultUID in configuration
- Removed AllowGIDOverride \(no longer needed\)
- Use Kubernetes standard spec.securityContext.runAsUser instead of custom annotations
- Changed --gid flag to --uid flag in SBATCH scripts
- Added file ownership \(chown\) for job directories as required by RFC
- Priority: pod securityContext > flavor UID > default UID

Implementation details:
- Updated types.go: Changed GID fields to UID
- Updated func.go: Changed validation from GID to UID
- Updated prepare.go:
  * Read UID from pod.Spec.SecurityContext.RunAsUser
  * Add --uid flag to SBATCH
  * Set file ownership via os.Chown for job directories
- Updated tests: Changed all GID tests to UID tests
- Updated configuration examples and documentation

This follows the RFC requirement for user attribution and file ownership
in scenarios where the SLURM plugin runs as root.") [#58](https://github.com/interlink-hq/interlink-slurm-plugin/discussions/58)  | Dec 1, 2025  |  
| [Add envFrom support for Secret and ConfigMap injection into container…](https://github.com/interlink-hq/interlink-slurm-plugin/commit/f8ac3bdc0592bc3a645b7832473610e51ac4615d "Add envFrom support for Secret and ConfigMap injection into container envfile \(#158\)

* Initial plan

* Apply remaining changes

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* Fix EnvFrom support: inject secret/configmap keys into envfile for containers using envFrom

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* Fix EnvFrom support and test script DATABASE_URL patch

- Add EnvFrom \(secretRef/configMapRef\) support in createEnvFile/prepareEnvs
- Fix pre-existing missing closing braces in prepare_test.go and resources_test.go
- Patch vk-test-set's 085 test YAML after clone: fix DATABASE_URL value from
  '******localhost:5432/mydb' to 'postgresql://localhost:5432/mydb' to match
  the log-check regex '✓ DATABASE_URL: postgresql'

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* Fix 085 test encoding: replace Unicode checkmarks with ASCII PASS/FAIL

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>")  | Aug 26, 2026  |  
|   |   | [Remove Unicode patching for 085-secret-envfrom.yaml](https://github.com/interlink-hq/interlink-slurm-plugin/commit/a7746143276cab94452a9b6b295e320b71135703 "Remove Unicode patching for 085-secret-envfrom.yaml

Removed the patching script for Unicode checkmarks in YAML.")  | Aug 26, 2026  |  
|   |   | [Fix SLURM annotation flag parsing](https://github.com/interlink-hq/interlink-slurm-plugin/commit/cd7b4d2574afcacfd933018cd78e7d8aca4ad21f "Fix SLURM annotation flag parsing")  | Mar 24, 2026  |  
|   |   | [feat: add support for Kubernetes startup probes](https://github.com/interlink-hq/interlink-slurm-plugin/commit/5e70cfd4825be43c900f914c9c201d60c9c5c4c0 "feat: add support for Kubernetes startup probes

Add full support for Kubernetes startup probes with proper initialDelaySeconds
handling. Startup probes now:
- Respect initialDelaySeconds configuration before first execution
- Run in background without blocking container startup
- Block readiness and liveness probes until successful
- Support both HTTP and Exec probe types
- Include proper cleanup and error handling

Changes:
- Add vendor/ to .gitignore to exclude vendored dependencies
- Update ContainerCommand struct to include startupProbes field
- Extend translateKubernetesProbes to handle startup probe translation
- Implement runStartupProbe and waitForStartupProbes functions
- Update probe metadata storage/loading for startup probe counts
- Add startup probe example demonstrating 300s initial delay
- Fix probe script generation to properly sequence probe execution

Startup probes follow Kubernetes semantics where they must succeed before
other probes begin execution, preventing premature readiness/liveness checks
during application initialization.

Signed-off-by: Diego Ciangottini <diego.ciangottini@pg.infn.it>")  | Oct 2, 2025  |  
|   |   |   | Jun 8, 2025  |  
|   |   |   | Oct 14, 2024  |  
|   |   | [now fetches all pod statuses when an empty request is received](https://github.com/interlink-hq/interlink-slurm-plugin/commit/2d0ecd51dcaa774b0d17d4f368596ca9b9d5ceb3 "now fetches all pod statuses when an empty request is received

Signed-off-by: Surax98 <giacomo.surace@gmail.com>")  | Apr 11, 2024  |  
|   |   |   | Apr 2, 2024  |  
|   |   |   | Apr 2, 2024  |  
|   |   | [docs: add release notes for version 0.6.0](https://github.com/interlink-hq/interlink-slurm-plugin/commit/4b244b2eb2e13b9311c253853dc9aba2a1bea143 "docs: add release notes for version 0.6.0

Added comprehensive changelog entry for version 0.6.0 documenting:
- Major features: UID configuration, flavor system, probe support, enroot runtime
- Bug fixes: probe behavior, startup delays, job script handling
- Breaking changes: GID to UID migration
- New configuration options")  | Dec 6, 2025  |  
|   |   |   | Apr 2, 2024  |  
|   |   |   | Apr 2, 2024  |  
|   |   |   | Apr 2, 2024  |  
|   |   |   | Apr 2, 2024  |  
|   |   |  [Add K3s-based e2e test suite (no Dagger) (](https://github.com/interlink-hq/interlink-slurm-plugin/commit/5fde818a6131b8e73d701302f1792c459f8b1a29 "Add K3s-based e2e test suite \(no Dagger\) \(#139\)

* Initial plan

* Add K3s-based e2e test scripts and workflow without Dagger

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/e9d03db8-e30a-416f-86a2-abf34403115b

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>")[#139](https://github.com/interlink-hq/interlink-slurm-plugin/pull/139)[)](https://github.com/interlink-hq/interlink-slurm-plugin/commit/5fde818a6131b8e73d701302f1792c459f8b1a29 "Add K3s-based e2e test suite \(no Dagger\) \(#139\)

* Initial plan

* Add K3s-based e2e test scripts and workflow without Dagger

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/e9d03db8-e30a-416f-86a2-abf34403115b

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>")  | Apr 5, 2026  |  
|   |   |  [feat: support custom job working directory via annotation (](https://github.com/interlink-hq/interlink-slurm-plugin/commit/c976ba4800e512f890efde16afd29be5d6585ce7 "feat: support custom job working directory via annotation \(#137\)

* Initial plan

* feat: pass job working dir via annotation slurm-job.vk.io/job-workdir

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/12e0a146-7ebc-4bc8-9e42-87b0eef0e7f5

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* fix: address code review - path validation, error handling, parameter naming

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/12e0a146-7ebc-4bc8-9e42-87b0eef0e7f5

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* feat: recover workdir from annotations/disk when cache wiped, add no-clean-workdir annotation

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/4b5c9be5-429f-4d38-9041-25ede0beb360

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* fix: address review comments on workdir safety and JID handling

- Add os.MkdirAll in handleJidAndPodUid to ensure metadata dir exists
- Fix deleteContainer panic when JID map entry is missing
- Validate WorkDir.path on load in LoadJIDs \(absolute, no .. elements\)

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* docs: add custom job working directory section to README

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Co-authored-by: Diego Ciangottini <diego.ciangottini@gmail.com>
Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>")[#137](https://github.com/interlink-hq/interlink-slurm-plugin/pull/137)[)](https://github.com/interlink-hq/interlink-slurm-plugin/commit/c976ba4800e512f890efde16afd29be5d6585ce7 "feat: support custom job working directory via annotation \(#137\)

* Initial plan

* feat: pass job working dir via annotation slurm-job.vk.io/job-workdir

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/12e0a146-7ebc-4bc8-9e42-87b0eef0e7f5

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* fix: address code review - path validation, error handling, parameter naming

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/12e0a146-7ebc-4bc8-9e42-87b0eef0e7f5

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* feat: recover workdir from annotations/disk when cache wiped, add no-clean-workdir annotation

Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/4b5c9be5-429f-4d38-9041-25ede0beb360

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* Apply suggestion from @Copilot

Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>

* fix: address review comments on workdir safety and JID handling

- Add os.MkdirAll in handleJidAndPodUid to ensure metadata dir exists
- Fix deleteContainer panic when JID map entry is missing
- Validate WorkDir.path on load in LoadJIDs \(absolute, no .. elements\)

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

* docs: add custom job working directory section to README

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Co-authored-by: Diego Ciangottini <diego.ciangottini@gmail.com>
Co-authored-by: Copilot <175728472+Copilot@users.noreply.github.com>")  | Aug 26, 2026  |  
|   |   | [Handle SLURM TIMEOUT (TO) and OOM states for graceful pod rescheduling](https://github.com/interlink-hq/interlink-slurm-plugin/commit/0cb7f04c5143c992a704a4e71cd3308c22731d89 "Handle SLURM TIMEOUT \(TO\) and OOM states for graceful pod rescheduling

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/56fd89ba-609b-462d-b2ec-e08cb6375ed9")  | Mar 26, 2026  |  
|   |   | [Handle SLURM TIMEOUT (TO) and OOM states for graceful pod rescheduling](https://github.com/interlink-hq/interlink-slurm-plugin/commit/0cb7f04c5143c992a704a4e71cd3308c22731d89 "Handle SLURM TIMEOUT \(TO\) and OOM states for graceful pod rescheduling

Co-authored-by: dciangot <4144326+dciangot@users.noreply.github.com>
Agent-Logs-Url: https://github.com/interlink-hq/interlink-slurm-plugin/sessions/56fd89ba-609b-462d-b2ec-e08cb6375ed9")  | Mar 26, 2026  |  
| View all files |  
## Repository files navigation
# ℹ️ Overview
## Introduction
InterLink aims to provide an abstraction for the execution of a Kubernetes pod on any remote resource capable of managing a Container execution lifecycle. We target to facilitate the development of provider specific plugins, so the resource providers can leverage the power of virtual kubelet without a black belt in kubernetes internals.
The project consists of two main components:
  * **A Kubernetes Virtual Node:** based on the [VirtualKubelet](https://virtual-kubelet.io/) technology. Translating request for a kubernetes pod execution into a remote call to the interLink API server.
  * **The interLink API server:** a modular and pluggable REST server where you can create your own Container manager plugin (called sidecars), or use the existing ones: remote docker execution on a remote host, singularity Container on a remote SLURM batch system. This repository aims to maintain the SLURM sidecar as a standalone plugin.


The project got inspired by the [KNoC](https://github.com/CARV-ICS-FORTH/knoc) and [Liqo](https://github.com/liqotech/liqo/tree/master) projects, enhancing that with the implemention a generic API layer b/w the virtual kubelet component and the provider logic for the container lifecycle management.
##  Usage
###  ‼️ Requirements
  * **[Our Kubernetes Virtual Node and the interLink API server](https://github.com/interTwin-eu/interLink)**
  * **[The Go programming language](https://go.dev/doc/install)** (to build binaries)
  * (optional)


Note: if you want a quick start setup (using a Docker container), Go is not necessary
####  ⚠️ Pods Requirements ⚠️
  * It is very important for you to remember to set CPU and Memory Limits in your Pod/Deployment YAML, otherwise default resources will be applied; specifically, if you don't set a CPU limit, only 1 CPU will be used for each task, while if you don't set any Memory limit, only 1MB will be used for each task.
  * Docker entrypoints are not supported by Singularity. This means you have to manually specify a command to be executed. If you don't, /bin/sh is assumed as the default one.


The following is a simple example of a Pod with a specified command and limits properly set:

```
    apiVersion: v1
    kind: Pod
    metadata:
    name: test-pod
    annotations:
        slurm-job.knoc.io/flags: "--job-name=test-pod-cfg -t 2800  --ntasks=8 --nodes=1 --mem-per-cpu=2000"
    spec:
    restartPolicy: Never
    containers:
    - image: docker://busybox:latest 
        command: ["echo"]
        args: ["hello world"]
        imagePullPolicy: Always
        name: "hello world"
        resources:
            limits: 
                memory: 100Mi
                cpu: 2
```

### ⏩ Quick Start
Just run:

```
cd docker  docker compose up -d
```

This way, a docker container with a full SLURM environment will be started. You can find the used SLURM configuration in /docker/SlurmConfig.yaml. If you want to update the config after you already started it once, run:

```
docker compose up -d --build --force-recreate
```

So, the old container will be deleted, the image rebuilt and a new container with the updated config will be deployed.
### 🔨 Building binaries
It is of course possible to use binaries as a standalone application. Just run

```
make all
```

and you will be able to find the built slurm-sd binary inside the bin directory. Before executing it, remember to check if the configuration file is correctly set according to your needs. You can find an example one under examples/config/SlurmConfig.yaml. Do not forget to set the SLURMCONFIGPATH environment variable to point to your config.
### ⚙️ A SLURM config example

```
SidecarPort: "4000"
Socket: 
SbatchPath: "/usr/bin/sbatch"
ScancelPath: "/usr/bin/scancel"
SqueuePath: "/usr/bin/squeue"
SinfoPath: "/usr/bin/sinfo"
CommandPrefix: 
ImagePrefix: "docker://"
SingularityPath: "singularity"
SingularityPrefix: 
SingularityDefaultOptions: []
ExportPodData: true
DataRootFolder: ".local/interlink/jobs/"
Namespace: 
Tsocks: false
TsocksPath: "$WORK/tsocks-1.8beta5+ds1/libtsocks.so"
TsocksLoginNode: "login01"
BashPath: /bin/bash
VerboseLogging: true
ErrorsOnlyLogging: false
EnableProbes: true
```

### ✏️ Annotations
It is possible to specify Annotations when submitting Pods to the K8S cluster. A list of all Annotations follows:  
| Annotation  | Description  |  
| --- | --- |  
| slurm-job.vk.io/singularity-commands  | Used to add specific Commands to be executed before the actual SLURM Job starts. It adds Commands on the Singularity exection line, in the SLURM bastch file  |  
| slurm-job.vk.io/pre-exec  | Used to add commands to be executed before the Job starts. It adds a command in the SLURM batch file after the #SBATCH directives  |  
| slurm-job.vk.io/singularity-mounts  | Used to add mountpoints to the Singularity Containers  |  
| slurm-job.vk.io/singularity-options  | Used to specify Singularity arguments  |  
| slurm-job.vk.io/image-root  | Used to specify the root path of the Singularity Image  |  
| slurm-job.vk.io/flags  | Used to specify SLURM flags. These flags will be added to the SLURM script in the form of #SBATCH flag1, #SBATCH flag2, etc  |  
| slurm-job.vk.io/mpi-flags  | Used to prepend "mpiexec -np $SLURM_NTASKS *flags*" to the Singularity Execution  |  
| slurm-job.vk.io/flavor  | Used to explicitly select a flavor configuration (e.g., "gpu-nvidia", "high-io")  |  
| slurm-job.vk.io/job-workdir  | Used to specify a custom base directory for the job's working directory. The full path will be `<value>/<namespace>-<podUID>`. Job scripts, output files, and status files are written there. Log, status, and delete operations also use this path. Useful in conjunction with `--gid` to run jobs in a directory accessible by a specific group.  |  
| slurm-job.vk.io/no-clean-workdir  | When set to `"true"` and a custom `job-workdir` is in use, the job working directory is **not** removed when the pod is deleted. The plugin metadata directory (`DataRootFolder`) is still cleaned up. Useful when the job output must be preserved after pod deletion.  |  
**Note** : To specify a custom User ID (UID) for SLURM jobs, use the Kubernetes standard `spec.securityContext.runAsUser` field in your pod specification (see UID Configuration section below).
### 🎨 Flavor System
The SLURM plugin supports "flavors" - predefined configurations that provide default resource values and SLURM-specific settings. This simplifies pod definitions and ensures consistent resource allocation across jobs.
#### How Flavors Work
Flavors are resolved in the following priority order:
  1. **Explicit annotation** : `slurm-job.vk.io/flavor: "flavor-name"`
  2. **Auto-detection** : GPU resources automatically select GPU flavors (exact GPU count match preferred)
  3. **Default flavor** : Falls back to the flavor specified in `DefaultFlavor` config


#### Configuring Flavors
Add flavors to your `SlurmConfig.yaml`:

```
DefaultFlavor: "default"
Flavors:
  default:
    Name: "default"
    Description: "Standard CPU job (4 cores, 16GB RAM)"
    CPUDefault: 4
    MemoryDefault: 
    SlurmFlags:
      - "--partition=cpu"
      - "--time=01:00:00"

  gpu-nvidia:
    Name: "gpu-nvidia"
    Description: "GPU job with NVIDIA GPU (8 cores, 64GB RAM, 1 GPU)"
    CPUDefault: 8
    MemoryDefault: 
    UID: 2000  # Optional: Set a specific UID for this flavor
    SlurmFlags:
      - "--gres=gpu:1"
      - "--partition=gpu"
      - "--time=04:00:00"

  high-io:
    Name: "high-io"
    Description: "High I/O job (16 cores, 32GB RAM, fast storage)"
    CPUDefault: 16
    MemoryDefault: 
    SlurmFlags:
      - "--partition=fast-io"
      - "--constraint=ssd"
```

#### Flavor Behavior
  * **Default Resources** : Flavor CPU/memory defaults apply ONLY when pod doesn't specify resource limits
  * **Pod Overrides** : If pod specifies resource limits, those take precedence over flavor defaults
  * **SLURM Flag Priority** : Flavor flags < Annotation flags < Pod resource limits
  * **Flag Deduplication** : Duplicate flags are automatically removed, with later flags overriding earlier ones


#### Example: Using Flavors
**Example 1: Auto-detected GPU flavor**

```
apiVersion: v1
kind: Pod
metadata:
  name: gpu-job
spec:
  containers:
  - name: pytorch
    image: docker://pytorch/pytorch:latest
    resources:
      limits:
        nvidia.com/gpu: 1  # Automatically selects "gpu-nvidia" flavor
```

**Example 2: Explicit flavor selection**

```
apiVersion: v1
kind: Pod
metadata:
  name: io-intensive-job
  annotations:
    slurm-job.vk.io/flavor: "high-io"
spec:
  containers:
  - name: data-processor
    image: docker://myapp:latest
    # Will use high-io flavor's 16 CPU and 32GB RAM defaults
```

**Example 3: Pod resources override flavor defaults**

```
apiVersion: v1
kind: Pod
metadata:
  name: custom-resources
  annotations:
    slurm-job.vk.io/flavor: "default"
spec:
  containers:
  - name: app
    image: docker://myapp:latest
    resources:
      limits:
        cpu:         # Overrides flavor's 4 CPU default
        memory: "128Gi"  # Overrides flavor's 16GB default
```

### 👥 UID (User ID) Configuration
**RFC** : [#58](https://github.com/interlink-hq/interlink-slurm-plugin/discussions/58)
The SLURM plugin supports setting a custom User ID (UID) for SLURM jobs. This allows jobs to run as specific users, enabling proper file ownership, permission management, and user attribution for HPC resource accounting.
#### How UID Works
UIDs are resolved with the following priority (highest to lowest):
  1. **Pod securityContext** : `spec.securityContext.runAsUser` (Kubernetes standard)
  2. **Flavor UID** : Configured in the flavor definition
  3. **Default UID** : Configured globally in `DefaultUID`


When a UID is configured, the plugin:
  * Adds `--uid=<value>` to the SBATCH script
  * Sets file ownership (`chown`) of job directories and scripts to the specified UID
  * Ensures the SLURM job runs as the specified user


#### Configuring UID
**Global Configuration** (in `SlurmConfig.yaml`):

```
# Optional: Set a default UID for all jobs
DefaultUID: 1000
```

**Per-Flavor Configuration** :

```
Flavors:
  user-jobs:
    Name: "user-jobs"
    Description: "Jobs for regular users"
    CPUDefault: 8
    MemoryDefault: 
    UID: 1001  # Jobs run as this user
    SlurmFlags:
      - "--partition=users"
```

#### Using UID in Pods
**Example 1: Using Kubernetes securityContext (recommended)**

```
apiVersion: v1
kind: Pod
metadata:
  name: custom-uid-job
spec:
  # Standard Kubernetes securityContext
  securityContext:
    runAsUser: 1001  # SLURM job will run as UID 1001
  containers:
  - name: app
    image: docker://myapp:latest
    resources:
      limits:
        cpu: 4
        memory: 16Gi
```

**Example 2: Using flavor UID**

```
apiVersion: v1
kind: Pod
metadata:
  name: user-job
  annotations:
    slurm-job.vk.io/flavor: "user-jobs"  # Uses UID 1001 from flavor
spec:
  containers:
  - name: computation
    image: docker://scientific-app:latest
```

**Example 3: Complete UID priority example**

```
# Config: DefaultUID: 1000
# Flavor "compute": UID: 2000
# Pod securityContext.runAsUser: 3000

# Result: Job runs with UID 3000 (securityContext wins)
apiVersion: v1
kind: Pod
metadata:
  annotations:
    slurm-job.vk.io/flavor: "compute"
spec:
  securityContext:
    runAsUser: 3000  # This takes precedence
  containers:
  - name: app
    image: docker://myapp:latest
```

#### UID Use Cases
  * **User Attribution** : Jobs appear in SLURM accounting under the correct user
  * **File Ownership** : Output files are owned by the correct user
  * **Permission Management** : Jobs respect filesystem permissions
  * **Multi-user Environments** : Different users submit jobs through the same Kubernetes cluster
  * **HPC Integration** : Integrate with existing HPC user management systems


#### Important Notes
  * UID must be a non-negative integer
  * Invalid UID values in `securityContext.runAsUser` are ignored (falls back to flavor or default)
  * The SLURM cluster must be configured to allow UID specification (plugin typically runs as root)
  * File ownership is set via `chown` for job directories (`job.slurm`, `job.sh`)
  * The `--uid` flag is added to the SBATCH script as `#SBATCH --uid=<value>`
  * This feature is designed for scenarios where the plugin runs as root and needs to impersonate users


### 📁 Custom Job Working Directory
By default the plugin places all job files (scripts, output, logs, mounts) under `DataRootFolder/<namespace>-<podUID>`. The `slurm-job.vk.io/job-workdir` annotation lets you redirect these job files to a different base directory — for example a shared group-accessible path — while keeping a small metadata directory under `DataRootFolder` for reliable restart recovery.
#### How it works  
| Directory  | Location  | Contents  |  
| --- | --- | --- |  
| Metadata directory  | `DataRootFolder/<namespace>-<podUID>`  |  `JobID.jid`, `PodUID.uid`, `PodNamespace.ns`, timing files, `WorkDir.path` pointer  |  
| Job working directory  | `<job-workdir>/<namespace>-<podUID>`  | Job scripts, SLURM stdout/stderr, container logs, env files, mount data  |  
The metadata directory is **always** created so that `LoadJIDs` can recover job state after a plugin restart. The `WorkDir.path` file inside it stores the custom path so that status, log, and delete operations resolve the correct directory even after the in-memory cache is wiped.
#### Usage

```
apiVersion: v1
kind: Pod
metadata:
  name: group-job
  annotations:
    slurm-job.vk.io/job-workdir: /scratch/mygroup
    slurm-job.vk.io/flags: "--gid=mygroup"
spec:
  containers:
  - name: app
    image: docker://myapp:latest
```

Job files land in `/scratch/mygroup/<namespace>-<podUID>/`. Plugin metadata stays in `DataRootFolder/<namespace>-<podUID>/`.
#### Preserving the job output directory on deletion
By default, both the metadata directory and the job working directory are removed when the pod is deleted. Set `slurm-job.vk.io/no-clean-workdir: "true"` to keep the job working directory:

```
metadata:
  annotations:
    slurm-job.vk.io/job-workdir: /scratch/mygroup
    slurm-job.vk.io/no-clean-workdir: "true"
```

With this annotation the plugin only removes the metadata directory; `/scratch/mygroup/<namespace>-<podUID>/` is left intact.
#### Path validation
The annotation value must be an absolute path. Relative paths and paths containing `..` traversal elements are silently ignored and the default `DataRootFolder` is used instead.
### ⚙️ Explanation of the SLURM Config file
Detailed explanation of the SLURM config file key values. Edit the config file before running the binary or before building the docker image (`docker compose up -d --build --force-recreate` will recreate and re-run the updated image)  
| Key  | Value  |  
| --- | --- |  
| SidecarPort  | the sidecar listening port. Sidecar and Interlink will communicate on this port. Set $SIDECARPORT environment variable to specify a custom one  |  
| Socket  | Unix socket path for communication (optional)  |  
| SbatchPath  | path to your Slurm's sbatch binary  |  
| ScancelPath  | path to your Slurm's scancel binary  |  
| SqueuePath  | path to your Slurm's squeue binary  |  
| SinfoPath  | path to your Slurm's sinfo binary  |  
| CommandPrefix  | here you can specify a prefix for the programmatically generated script (for the slurm plugin). Basically, if you want to run anything before the script itself, put it here.  |  
| ImagePrefix  | here you can specify a prefix if you want to prefix the container image name. For example: "docker://". This will do something only if the prefix is not added yet, and if there is no "/" as the first letter of the image name (e.g.: "/root/image.tgz"), which would be an absolute path. Warning: using this field will not allow relative path anymore (e.g.: ./image.tgz and ImagePrefix set to "docker://" will generate "docker://./image.tgz instead of relative path. Use absolute path instead of relative path). Warning2: the the container annotation "slurm-job.vk.io/image-root" is set, this take precedence over ImagePrefix.  |  
| SingularityPath  | path to your Singularity binary  |  
| SingularityPrefix  | prefix to add to Singularity image names  |  
| SingularityDefaultOptions  | array of default options to pass to Singularity commands  |  
| ExportPodData  | Set it to true if you want to export Pod's ConfigMaps and Secrets as mountpoints in your Singularity Container  |  
| DataRootFolder  | Specify where to store the exported ConfigMaps/Secrets locally  |  
| Namespace  | Namespace where Pods in your K8S will be registered  |  
| Tsocks  | true or false values only. Enables or Disables the use of tsocks library to allow proxy networking. Only implemented for the Slurm sidecar at the moment.  |  
| TsocksPath  | path to your tsocks library.  |  
| TsocksLoginNode  | specify an existing node to ssh to. It will be your "window to the external world"  |  
| BashPath  | Path to your Bash shell  |  
| VerboseLogging  | Enable or disable Debug messages on logs. True or False values only  |  
| ErrorsOnlyLogging  | Specify if you want to get errors only on logs. True or false values only  |  
| EnableProbes  | Enable or disable health and readiness probes. True or False values only  |  
| DefaultUID  | Optional default User ID (UID) for all SLURM jobs. Must be a non-negative integer. See [RFC #58](https://github.com/interlink-hq/interlink-slurm-plugin/discussions/58)  |  
| Flavors  | Map of flavor configurations. Each flavor can specify CPUDefault, MemoryDefault, UID, and SlurmFlags. See Flavor System and UID Configuration sections for details  |  
| DefaultFlavor  | Name of the default flavor to use when no explicit flavor is specified and no auto-detection applies  |  
### 🔧 Environment Variables list
Here's the complete list of every customizable environment variable. When specified, it overwrites the listed key within the SLURM config file.  
| Env  | Value  |  
| --- | --- |  
| SLURMCONFIGPATH  | your SLURM config file path. Default is `/etc/interlink/SlurmConfig.yaml`  |  
| SIDECARPORT  | the Sidecar listening port. Docker default is 4000, Slurm default is 4001.  |  
| SBATCHPATH  | path to your Slurm's sbatch binary. Overwrites SbatchPath.  |  
| SCANCELPATH  | path to your Slurm's scancel binary. Overwrites ScancelPath.  |  
| SHARED_FS  | set this env to "true" to save configmaps values inside files directly mounted to Singularity containers instead of using ENVS to create them later  |  
| CUSTOMKUBECONF  | path to a service account kubeconfig  |  
| TSOCKS  | true or false, to use tsocks library allowing proxy networking. Working on Slurm sidecar at the moment. Overwrites Tsocks.  |  
| TSOCKSPATH  | path to your tsocks library. Overwrites TsocksPath.  |  
### :storage: HostPath Volume Support
The SLURM sidecar plugin has been updated to support Pods that require a HostPath volume. This allows you to run Pods that need access to specific directories on the host machine, which is useful for scenarios where data needs to be shared between the host and the Pod. It is also possible to specify if the volume is read-only or not, by setting the `readOnly` field in the `volumeMounts` section of the Pod spec. The following is an example of a Pod that uses a HostPath volume:

```
apiVersion: v1
kind: Pod
metadata:
  name: hostpath-pod
  namespace: interlink
  annotations: {"slurm-job.knoc.io/flags": "--job-name=test-pod"}

spec:
  restartPolicy: Never
  nodeSelector:
    kubernetes.io/hostname: vk-slurm-node # specify the virtual node name where the pod should run

  containers:
  - name: hello-world
    image: docker://ghcr.io/grycap/cowsay
    volumeMounts:
    - mountPath: /foo
      name: hostpath-volume
      readOnly: true
    command: ["/bin/cat"]
    args: ["/foo/test"]
    imagePullPolicy: Always
    resources:
      limits:
        memory: 
        cpu: 
  
  volumes:
  - name: hostpath-volume
    hostPath:
      path: /data/foo # directory location on host
      type: DirectoryOrCreate # this field is optional

  dnsPolicy: ClusterFirst

  tolerations:
    - key: virtual-node.interlink/no-schedule
      operator: Exists
      effect: NoSchedule
      
```

## About
No description, website, or topics provided.
### Resources
### Code of conduct
### Contributing
### Stars
**4** stars
### Watchers
**3** watching
### Forks
[ forks](https://github.com/interlink-hq/interlink-slurm-plugin/forks)
## 
[ 0.6.1Latest Apr 8, 2026 ](https://github.com/interlink-hq/interlink-slurm-plugin/releases/tag/0.6.1)
## 
## Languages
  * [Go90.2%](https://github.com/interlink-hq/interlink-slurm-plugin/search?l=go)
  * [Shell8.9%](https://github.com/interlink-hq/interlink-slurm-plugin/search?l=shell)
  * [Other0.9%](https://github.com/interlink-hq/interlink-slurm-plugin/search?l=Other)


Generated from [interTwin-eu/repository-template](https://github.com/interTwin-eu/repository-template)
You can’t perform that action at this time. 
