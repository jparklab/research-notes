---
title: 'GitHub - SlinkyProject/slurm-operator: Run Slurm on Kubernetes. A Slinky project.
  · GitHub'
id: github-slinkyprojectslurm-operator-run-slurm-on-kubernetes-a-slinky-project-gith
tags:
- llmd-router-nonk8s-backends-54b774
- hpc-slurm
- inverse-strategy
- router-extensibility
created: '2026-09-11T20:55:22.314774Z'
updated: '2026-09-15T04:55:23.899388Z'
source: https://github.com/SlinkyProject/slurm-operator
source_domain: github.com
fetched_at: '2026-09-11T20:55:22.293462Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'SlinkyProject/slurm-operator (NVIDIA/SchedMD ''Slinky'' project, 359 stars,
  1419 commits, active as of Sep 11 2026) runs a full Slurm cluster''s control-plane
  and worker nodes as first-class Kubernetes resources via CRDs: NodeSet (homogeneous
  Slurm compute/worker nodes, supporting a scalingMode of either StatefulSet-style
  replica scaling with stable ordinal identity, or DaemonSet-style one-pod-per-node
  scaling tied 1:1 to K8s nodes) and LoginSet (SSSD-backed login/submit nodes), both
  supporting scale-to-zero and HPA. Slurm node states (Idle, Allocated, Mixed, Down,
  Drain, Not Responding) are surfaced as native Kubernetes pod conditions/status on
  each NodeSet pod. Explicitly supports ''Hybrid'' deployments where only some Slurm
  components run in Kubernetes. This is the INVERSE integration strategy to a custom
  llm-d-router discovery plugin: instead of teaching the EPP to discover non-pod backends,
  Slinky makes the Slurm daemons themselves real Kubernetes Pods (via NodeSet), so
  a stock, unmodified InferencePool / K8s-based EndpointDiscovery can select them
  with zero router customization -- the tradeoff being that Slurm workers must run
  inside the K8s cluster''s pod networking/scheduling domain rather than on an independently-managed
  HPC allocation.'
utility_score: 12.0
---

[Skip to content](https://github.com/SlinkyProject/slurm-operator#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/SlinkyProject/slurm-operator) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/SlinkyProject/slurm-operator) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/SlinkyProject/slurm-operator) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/SlinkyProject/slurm-operator).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2FSlinkyProject%2Fslurm-operator) You must be signed in to change notification settings
  * [ Fork 100 ](https://github.com/login?return_to=%2FSlinkyProject%2Fslurm-operator)
  * [ Star  359 ](https://github.com/login?return_to=%2FSlinkyProject%2Fslurm-operator)


[**8** Branches](https://github.com/SlinkyProject/slurm-operator/branches)[](https://github.com/SlinkyProject/slurm-operator/tags)
Go to file
Open more actions menu
## Latest commit
[Merge branch 'alan/pin-golangci-lint' into 'main'](https://github.com/SlinkyProject/slurm-operator/commit/75e144dc497953969b75b75a3dbaa1af0bd41b17)
Open commit details
Sep 11, 2026
[75e144d](https://github.com/SlinkyProject/slurm-operator/commit/75e144dc497953969b75b75a3dbaa1af0bd41b17) · Sep 11, 2026
## History
[1,419 Commits](https://github.com/SlinkyProject/slurm-operator/commits/main/)
Open commit details
1,419 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [docs(github): tidy issue/PR templates and fix typos](https://github.com/SlinkyProject/slurm-operator/commit/4c8e00c10270124a2dcccac245d8b34aee50dd0e "docs\(github\): tidy issue/PR templates and fix typos

Format issue and PR templates for readability \(bold field headings on
their own line, unwrapped mid-sentence/mid-link breaks\) and correct
typos in the feature request and documentation request forms.

Signed-off-by: Giulio Calzolari <gcalzolari@nvidia.com>")  | Jul 6, 2026  |  
|   |   | [fix: add self-assign to MR template](https://github.com/SlinkyProject/slurm-operator/commit/e6ec0d0b8bf7b75c923454af0415644ae215937c "fix: add self-assign to MR template

Changelog: NONE
Signed-off-by: Killian Muldoon <kmuldoon@nvidia.com>")  | Jul 10, 2026  |  
|   |   | [feat: add skaffold debug profile and make targets](https://github.com/SlinkyProject/slurm-operator/commit/8e9fcec9444d15ccd8cfe5eeda677f1acbb3ae45 "feat: add skaffold debug profile and make targets

add a skaffold debug profile and make targets

Signed-off-by: Killian Muldoon <kmuldoon@nvidia.com>")  | Jul 10, 2026  |  
|   |   | [chore(changelog): sync changelogs to main](https://github.com/SlinkyProject/slurm-operator/commit/eb8babf4579b6573bf7d3dae4952880f6d740f29 "chore\(changelog\): sync changelogs to main

Changelog: NONE")  | Sep 2, 2026  |  
|   |   |   | Nov 8, 2024  |  
| [api/v1beta1](https://github.com/SlinkyProject/slurm-operator/tree/main/api/v1beta1 "This path skips through empty directories")  | [api/v1beta1](https://github.com/SlinkyProject/slurm-operator/tree/main/api/v1beta1 "This path skips through empty directories")  |   | Aug 14, 2026  |  
| [Merge branch 'smalinowski/update-client' into 'main'](https://github.com/SlinkyProject/slurm-operator/commit/bbb85a04e5df15b00acb75fdce92807bb362e0c1 "Merge branch 'smalinowski/update-client' into 'main'

Update Slurm Client

See merge request nvidia/schedmd/slinky/slurm-operator!679")  | Aug 20, 2026  |  
|   |   | [fix: make pods/binding webhook dry-run aware](https://github.com/SlinkyProject/slurm-operator/commit/9bcffa69cb4eabb078103a32229fbdd664e5f0c8 "fix: make pods/binding webhook dry-run aware

Presently the pods/binding webhook violates the webhook admissions
contract of `sideEffects: none`. The pods/binding webhook makes an
out-of-band update to the pod associated with the binding that it
triggers on. Presently, the pods/binding webhook will trigger on
requests utilizing the --dry-run=server flag and mutate live pods on the
cluster.

This commit updates the pods/binding webhook's
MutatingWebhookConfiguration and changes sideEffects to NoneOnDryRun.
This commit also modifies the webhook itself to detect and early-exit on
requests with the DryRun flag set.

Changelog: Fixed - Make pods/binding webhook dry-run aware")  | Aug 11, 2026  |  
| [docs: document sshd configuration and client keepalives](https://github.com/SlinkyProject/slurm-operator/commit/334c26f918bda2efd940863c19e3a7a05c3102e1 "docs: document sshd configuration and client keepalives

The operator generates the whole `sshd_config` and mounts it over
`/etc/ssh/sshd_config`, so the image's own defaults never apply. Nothing
documents what that generated file contains, or how `extraSshdConfig` interacts
with it.

Add a usage guide covering how the file is assembled, which settings the
operator already defines, and why `sshd_config`'s first-value-wins rule means
`extraSshdConfig` cannot override those. Document client-alive probes as the
remedy for long-lived sessions dropped by a load balancer's idle timeout, and
for sessions whose client went away while holding a worker's adopted job.

Changelog: NONE
Signed-off-by: Giulio Calzolari <gcalzolari@nvidia.com>")  | Sep 4, 2026  |  
| [Merge branch 'alan/fix-jwt-rotation-e2e' into 'main'](https://github.com/SlinkyProject/slurm-operator/commit/1deab535521d5b79543607adbf0a0f518118d29a "Merge branch 'alan/fix-jwt-rotation-e2e' into 'main'

Fix e2e JWT test

See merge request nvidia/schedmd/slinky/slurm-operator!747")  | Sep 10, 2026  |  
| [feat(metrics): enable webhook metrics by default](https://github.com/SlinkyProject/slurm-operator/commit/705eea619e29ecf0637c81ec4cf9703e744b5b22 "feat\(metrics\): enable webhook metrics by default

Changelog: Changed - Enable Slurm Operator webhook metrics by default.")  | Sep 2, 2026  |  
|   |   | [fix(nodeset): consider active pods when statefulset scaling](https://github.com/SlinkyProject/slurm-operator/commit/f116aef131004d950965b58c84d9013d5fd4dffb "fix\(nodeset\): consider active pods when statefulset scaling

This logic is adaptived from the Kubernetes ReplicaSet controller.

Changelog: Fixed - Fixed case where a NodeSet in StatefulSet mode would
 count an out of band pod termination against the total when deciding to
 scale, causing spurious pod recreations.")  | Sep 9, 2026  |  
|   | Aug 14, 2026  |  
| [Merge branch 'alan/fix-jwt-rotation-e2e' into 'main'](https://github.com/SlinkyProject/slurm-operator/commit/1deab535521d5b79543607adbf0a0f518118d29a "Merge branch 'alan/fix-jwt-rotation-e2e' into 'main'

Fix e2e JWT test

See merge request nvidia/schedmd/slinky/slurm-operator!747")  | Sep 10, 2026  |  
|   |   | [feat!: controller Slurm cluster creation via operator](https://github.com/SlinkyProject/slurm-operator/commit/2f608766c90f086fcd72a1e117a41be55e070f67 "feat!: controller Slurm cluster creation via operator

Slurm components were converted from pure builtin Kubernetes resources
into CRDs. Moreover, Controller, Accounting, Restapi, and LoginSet were
added to express components of the Slurm cluster. The Cluster CRD was
removed as it is no longer needed.

JWT token creation, for slurm-client, was internalized in the operator.

NodeSet spec.Selector was removed because those labels are internally
controlled templated.

Slurm Helm chart parameters were flattened and simplified. Some
configurations were removed, omitted, or changed. Along with these
changes are the use of helm recommended types like string and map
instead of list.

See https://helm.sh/docs/chart_best_practices/values/ for details.

Exporter and its token creation remains the same.")  | Aug 1, 2025  |  
|   |   | [feat: add slurm-operator-crds helm chart](https://github.com/SlinkyProject/slurm-operator/commit/d0ec5d5544dc0cd863f4393ffcb7b45652d0f8d6 "feat: add slurm-operator-crds helm chart")  | Sep 3, 2025  |  
|   |   | [perf: reduce docker build context](https://github.com/SlinkyProject/slurm-operator/commit/497b91cd832de81c0bc9f9e3cc2299a600d984f5 "perf: reduce docker build context")  | Sep 30, 2025  |  
|   |   | [chore: update Go merge attributes](https://github.com/SlinkyProject/slurm-operator/commit/023adb17b66495b0e147c30a9197a0a06ca82b24 "chore: update Go merge attributes

Changelog: NONE
Signed-off-by: Killian Muldoon <kmuldoon@nvidia.com>")  | Jul 31, 2026  |  
|   |   | [test(e2e): capture diagnostics before cleanup](https://github.com/SlinkyProject/slurm-operator/commit/3ed684149ddbf239831c4ca5d5e7f1c4d83b4b7c "test\(e2e\): capture diagnostics before cleanup")  | Aug 12, 2026  |  
|   |   |   | Aug 14, 2026  |  
|   |   | [chore(legal): add NOTICE and THIRD_PARTY_LICENSES generation](https://github.com/SlinkyProject/slurm-operator/commit/6a55fb05dd9c346ab4137ee8d28abdb95f8f8e83 "chore\(legal\): add NOTICE and THIRD_PARTY_LICENSES generation")  | Jun 22, 2026  |  
|   |   | [chore: adds structure for readthedocs site](https://github.com/SlinkyProject/slurm-operator/commit/3c99ed601f1799f77eaf34d91d7c94364163bdf4 "chore: adds structure for readthedocs site

ReadTheDocs requires a certain structure be followed in the /docs
directory of a repo in order to generate documentation for it. This
commit implements this structure for the slurm-operator repo.")  | Aug 15, 2025  |  
|   |   | [chore(changelog): sync changelogs to main](https://github.com/SlinkyProject/slurm-operator/commit/ce229a076fc8612cebf0765550892ae1455f8a21 "chore\(changelog\): sync changelogs to main")  | Jul 6, 2026  |  
|   |   | [chore: add in OSRB required repo templates and files](https://github.com/SlinkyProject/slurm-operator/commit/838de891ee9b4d907db9fd251139180393d6da2a "chore: add in OSRB required repo templates and files")  | Apr 15, 2026  |  
|   |   | [chore: add in OSRB required repo templates and files](https://github.com/SlinkyProject/slurm-operator/commit/838de891ee9b4d907db9fd251139180393d6da2a "chore: add in OSRB required repo templates and files")  | Apr 15, 2026  |  
|   |   | [chore: add in OSRB required repo templates and files](https://github.com/SlinkyProject/slurm-operator/commit/838de891ee9b4d907db9fd251139180393d6da2a "chore: add in OSRB required repo templates and files")  | Apr 15, 2026  |  
|   |   | [docs: document the Changelog trailer requirement](https://github.com/SlinkyProject/slurm-operator/commit/6d1efbe3bdc3752ced253a3048e44a5bdb6ffb00 "docs: document the Changelog trailer requirement

The check-changelog job rejects a merge request whose commits carry no
Changelog trailer, but the rule appears nowhere in the repository.
CONTRIBUTING.md covers the DCO sign-off and stops there, so a first
contribution fails CI on a rule it was never told about.

Add a Release notes section to CONTRIBUTING.md with the accepted
categories, the NONE escape hatch, and the requirement that the trailer
sit in the commit's final, contiguous trailer block. It also warns that
git commit --amend -s appends a second Signed-off-by once Changelog
follows it, because the sign-off is no longer the last trailer.

Signed-off-by: Giulio Calzolari <gcalzolari@nvidia.com>
Changelog: NONE")  | Sep 1, 2026  |  
|   |   | [feat: add skaffold debug profile and make targets](https://github.com/SlinkyProject/slurm-operator/commit/8e9fcec9444d15ccd8cfe5eeda677f1acbb3ae45 "feat: add skaffold debug profile and make targets

add a skaffold debug profile and make targets

Signed-off-by: Killian Muldoon <kmuldoon@nvidia.com>")  | Jul 10, 2026  |  
|   |   | [chore: update files according to OSRB](https://github.com/SlinkyProject/slurm-operator/commit/cbcff24d0ed62a47177e505e98f44de04e01066f "chore: update files according to OSRB")  | Apr 28, 2026  |  
|   |   | [chore: add in OSRB required repo templates and files](https://github.com/SlinkyProject/slurm-operator/commit/838de891ee9b4d907db9fd251139180393d6da2a "chore: add in OSRB required repo templates and files")  | Apr 15, 2026  |  
|   |   | [Merge branch 'alan/pin-golangci-lint' into 'main'](https://github.com/SlinkyProject/slurm-operator/commit/75e144dc497953969b75b75a3dbaa1af0bd41b17 "Merge branch 'alan/pin-golangci-lint' into 'main'

Unified golangci-lint

See merge request nvidia/schedmd/slinky/slurm-operator!754")  | Sep 11, 2026  |  
|   |   | [ci: fix vulns identified by govulncheck](https://github.com/SlinkyProject/slurm-operator/commit/947414424ac6859da6371036e3b67c4848f0c3a2 "ci: fix vulns identified by govulncheck

Fixed vulnerabilities:
* GO-2026-6094 via github.com/google/cel-go@v0.30.0

Changelog: Fixed - GO-2026-6094")  | Aug 26, 2026  |  
|   |   | [chore: update v1alpha1 => v1beta1](https://github.com/SlinkyProject/slurm-operator/commit/11748d24a1417721adda7c1e26d724bdbe7a0c0e "chore: update v1alpha1 => v1beta1

Continuation of commit 741d8cf.")  | Oct 30, 2025  |  
|   |   | [feat(controller): native Slurm active/passive HA via Replicas](https://github.com/SlinkyProject/slurm-operator/commit/31e87811dc0c1543f7a9609cac9f1f9aeb0bdf5a "feat\(controller\): native Slurm active/passive HA via Replicas

Add an optional HA configuration object to the Controller CRD. With
ha.enabled to enable Slurm native HA:

- one SlurmctldHost line per pod, addressed by per-pod DNS from the
  headless governing Service
- required hostname anti-affinity so a primary and backup never share a
  node \(user affinity overrides it\)
- a shared RWX StateSaveLocation via persistence.existingClaim, enforced
  by CEL \(per-pod volumeClaimTemplates are incoherent for HA\)
- readiness gates on /livez instead of /readyz: a standby never answers
  /readyz 2xx, which would leave it permanently NotReady and wedge
  StatefulSet rolling updates; clients select the active controller via
  the SlurmctldHost list, not Service readiness
- sackd/slurmd receive every pod FQDN as a comma-separated --conf-server
  list and fall through past a standby

Signed-off-by: Austin Fuller <afuller@crusoe.ai>
Co-authored-by: Skyler Malinowski <smalinowski@nvidia.com>
Changelog: Added - Added support for native Slurm High Availability \(HA\) mode.

Closes https://github.com/SlinkyProject/slurm-operator/pull/208")  | Jul 30, 2026  |  
|   |   | [chore: add in OSRB required repo templates and files](https://github.com/SlinkyProject/slurm-operator/commit/838de891ee9b4d907db9fd251139180393d6da2a "chore: add in OSRB required repo templates and files")  | Apr 15, 2026  |  
|   |   | [ci: fix vulns identified by govulncheck](https://github.com/SlinkyProject/slurm-operator/commit/2269ddfc48e7a033c2a1690f1dc502bf7e5ba5f4 "ci: fix vulns identified by govulncheck

Fixed vulnerabilities:
* GO-2026-6354 via golang.org/x/crypto@v0.56.0
* GO-2026-6355 via golang.org/x/crypto@v0.56.0

Changelog: Fixed - GO-2026-6354 GO-2026-6355")  | Sep 3, 2026  |  
|   |   | [fix: align operator helm version and appVersion with slinky version](https://github.com/SlinkyProject/slurm-operator/commit/ddfdb12ce3d107f23b51afb8abe899037f44a354 "fix: align operator helm version and appVersion with slinky version

Update the Helm charts to use the Slinky version as the chart version and the operator chart appVersion.

The Slurm chart retains appVersion as the Slurm version.

Changelog: Fixed - Align Helm AppVersion and Version with Slinky Version
Signed-off-by: Killian Muldoon <kmuldoon@nvidia.com>")  | Jul 29, 2026  |  
|   |   |   | Nov 8, 2024  |  
|   |   |   | Sep 19, 2025  |  
|   |   | [Merge branch 'marlow/controller_optimizations' into 'main'](https://github.com/SlinkyProject/slurm-operator/commit/ef4535bc5aad80bf547871e8c7233d15af8f7910 "Merge branch 'marlow/controller_optimizations' into 'main'

perf\(controller\): use tuned exponential-backoff rate limiter

See merge request nvidia/schedmd/slinky/slurm-operator!720")  | Sep 8, 2026  |  
|   |   | [ci: fix vulns identified by govulncheck](https://github.com/SlinkyProject/slurm-operator/commit/2269ddfc48e7a033c2a1690f1dc502bf7e5ba5f4 "ci: fix vulns identified by govulncheck

Fixed vulnerabilities:
* GO-2026-6354 via golang.org/x/crypto@v0.56.0
* GO-2026-6355 via golang.org/x/crypto@v0.56.0

Changelog: Fixed - GO-2026-6354 GO-2026-6355")  | Sep 3, 2026  |  
| View all files |  
## Repository files navigation
# Kubernetes Operator for Slurm Clusters
Run [Slurm](https://slurm.schedmd.com/overview.html) on [Kubernetes](https://kubernetes.io/), by [SchedMD](https://schedmd.com/). A [Slinky](https://slinky.ai/) project.
## Table of Contents
  * [Kubernetes Operator for Slurm Clusters](https://github.com/SlinkyProject/slurm-operator#kubernetes-operator-for-slurm-clusters)
    * [Table of Contents](https://github.com/SlinkyProject/slurm-operator#table-of-contents)
    * [Overview](https://github.com/SlinkyProject/slurm-operator#overview)
      * [Slurm Cluster](https://github.com/SlinkyProject/slurm-operator#slurm-cluster)
    * [Features](https://github.com/SlinkyProject/slurm-operator#features)
      * [NodeSets](https://github.com/SlinkyProject/slurm-operator#nodesets)
        * [StatefulSet (default)](https://github.com/SlinkyProject/slurm-operator#statefulset-default)
      * [Hybrid Support](https://github.com/SlinkyProject/slurm-operator#hybrid-support)
    * [Compatibility Matrix](https://github.com/SlinkyProject/slurm-operator#compatibility-matrix)
    * [Upgrades](https://github.com/SlinkyProject/slurm-operator#upgrades)
    * [Documentation](https://github.com/SlinkyProject/slurm-operator#documentation)
    * [Support and Development](https://github.com/SlinkyProject/slurm-operator#support-and-development)


## Overview
[Slurm](https://slurm.schedmd.com/overview.html) and [Kubernetes](https://kubernetes.io/) are workload managers originally designed for different kinds of workloads. In broad strokes: Kubernetes excels at scheduling workloads that typically run for an indefinite amount of time, with potentially vague resource requirements, on a single node, with loose policy, but can scale its resource pool infinitely to meet demand; Slurm excels at quickly scheduling workloads that run for a finite amount of time, with well defined resource requirements and topology, on multiple nodes, with strict policy, but its resource pool is known.
This project enables the best of both workload managers, unified on Kubernetes. It contains a [Kubernetes](https://kubernetes.io/) operator to deploy and manage certain components of [Slurm](https://slurm.schedmd.com/overview.html) clusters. This repository implements [custom-controllers](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#custom-controllers) and [custom resource definitions (CRDs)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions) designed for the lifecycle (creation, upgrade, graceful shutdown) of Slurm clusters.
For additional architectural notes, see the [architecture](https://github.com/SlinkyProject/slurm-operator/blob/main/docs/concepts/architecture.md) docs.
### Slurm Cluster
Slurm clusters are very flexible and can be configured in various ways. Our Slurm helm chart provides a reference implementation that is highly customizable and tries to expose everything Slurm has to offer.
For additional information about Slurm, see the [slurm](https://github.com/SlinkyProject/slurm-operator/blob/main/docs/concepts/slurm.md) docs.
## Features
### Controller
The Slurm control-plane is responsible for scheduling Slurm workload onto its worker nodes and managing their states.
Slurm High Availability (HA) can be enabled by setting `ha.enabled=true` and `persistence.existingClaim` to a PVC with ReadWriteMany (RWX) access mode.
Changes to the Slurm configuration files are automatically detected and the Slurm cluster is reconfigured seamlessly with zero downtime of the Slurm control-plane.
Note
The kubelet's `configMapAndSecretChangeDetectionStrategy` and `syncFrequency` settings directly affect when pods have their mounted ConfigMaps and Secrets updated. By default, the kubelet is in `Watch` mode with a polling frequency of 60 seconds.
### NodeSets
A set of homogeneous Slurm workers (compute nodes), which are delegated to execute the Slurm workload.
The operator will take into consideration the running workload among Slurm nodes as it needs to scale-in, upgrade, or otherwise handle node failures. Slurm nodes will be marked as [drain](https://slurm.schedmd.com/scontrol.html#OPT_DRAIN) before their eventual termination pending scale-in or upgrade.
Slurm node states (e.g. Idle, Allocated, Mixed, Down, Drain, Not Responding, etc...) are applied to each NodeSet pod via their pod conditions; each NodeSet pod contains a pod status that reflects their own Slurm node state.
The NodeSet CRD supports a `scalingMode` field that controls how many pods are created and how they are scaled. This allows you to choose between replica-based scaling (like a StatefulSet) or one-pod-per-node scaling (like a DaemonSet).
#### StatefulSet (default)
  * **Behavior** : The controller maintains a fixed number of pods according to the `replicas` field.
  * **Use when** : A fixed or scalable number of Slurm worker pods is needed. Scale-to-zero and horizontal autoscaling (e.g. HPA) apply to this mode.
  * **Note** : Each pod has a stable identity (e.g. ordinal-based naming)


#### DaemonSet
  * **Behavior** : The controller schedules one pod per Kubernetes node that matches the NodeSet's pod template (e.g. `nodeSelector`, `tolerations`). Pod count follows the number of matching nodes. Adding or removing nodes automatically adds or removes pods.
  * **Use when** : 1:1 alignment between Kubernetes and Slurm (slurmd) nodes is needed.
  * **Note** : The `replicas` field is ignored. Pod identity is tied to the node (e.g. node name) rather than an ordinal.


The operator supports NodeSet scale to zero, scaling the resource down to zero replicas. Hence, any Horizontal Pod Autoscaler (HPA) that also support scale to zero can be best paired with NodeSets.
NodeSets can be resolved by hostname. This enables hostname-based resolution between login pods and worker pods, enabling direct pod-to-pod communication using predictable hostnames (e.g., `cpu-1-0`, `gpu-2-1`).
### LoginSets
A set of homogeneous login nodes (submit node, jump host) for Slurm, which manage user identity via SSSD.
The operator supports LoginSet scale to zero, scaling the resource down to zero replicas. Hence, any Horizontal Pod Autoscaler (HPA) that also support scale to zero can be best paired with LoginSets.
### Hybrid Support
Sometimes a Slurm cluster has some, but not all, of its components in Kubernetes. The operator and its CRDs are designed support these use cases.
### Slurm
Slurm is a full featured HPC workload manager. To highlight a few features:
  * [**Accounting**](https://slurm.schedmd.com/accounting.html): collect accounting information for every job and job step executed.
  * [**Partitions**](https://slurm.schedmd.com/quickstart.html#arch): job queues with sets of resources and constraints (e.g. job size limit, job time limit, users permitted).
  * [**Reservations**](https://slurm.schedmd.com/reservations.html): reserve resources for jobs being executed by select users and/or select accounts.
  * [**Job Dependencies**](https://slurm.schedmd.com/sbatch.html#OPT_dependency): defer the start of jobs until the specified dependencies have been satisfied.
  * [**Job Containers**](https://slurm.schedmd.com/containers.html): jobs which run an unprivileged OCI container bundle.
  * [**MPI**](https://slurm.schedmd.com/mpi_guide.html): launch parallel MPI jobs, supports various MPI implementations.
  * [**Priority**](https://slurm.schedmd.com/priority_multifactor.html): assigns priorities to jobs upon submission and on an ongoing basis (e.g. as they age).
  * [**Preemption**](https://slurm.schedmd.com/preempt.html): stop one or more low-priority jobs to let a high-priority job run.
  * [**QoS**](https://slurm.schedmd.com/qos.html): sets of policies affecting scheduling priority, preemption, and resource limits.
  * [**Fairshare**](https://slurm.schedmd.com/fair_tree.html): distribute resources equitably among users and accounts based on historical usage.
  * [**Node Health Check**](https://slurm.schedmd.com/slurm.conf.html#OPT_HealthCheckProgram): periodically check node health via script.


## Compatibility Matrix  
| Release  | Minimum Kubernetes  | Minimum Slurm (Data Parser)  |  
| --- | --- | --- |  
 |  
 |  
 |  
Please refer to the following lifecycle documents for support guidance:


## Quick Start
Install the [cert-manager](https://cert-manager.io/docs/installation/helm/) with its CRDs:

```
helm install \
  cert-manager oci://quay.io/jetstack/charts/cert-manager \
  --namespace cert-manager --create-namespace \
  --set crds.enabled=true
```

Install the slurm-operator and its CRDs:

```
helm install slurm-operator-crds oci://ghcr.io/slinkyproject/charts/slurm-operator-crds
helm install slurm-operator oci://ghcr.io/slinkyproject/charts/slurm-operator \
  --namespace=slinky --create-namespace
```

Install a Slurm cluster:

```
helm install slurm oci://ghcr.io/slinkyproject/charts/slurm \
  --set-json 'nodesets={"slinky":{}}' \
  --set "partitions.all.enabled=true" \
  --namespace=slurm --create-namespace
```

For additional instructions, see the [installation](https://github.com/SlinkyProject/slurm-operator/blob/main/docs/installation.md) guide.
## Upgrades
Slinky versions are expressed as **X.Y.Z** , where **X** is the major version, **Y** is the minor version, and **Z** is the patch version, following [Semantic Versioning](https://semver.org/) terminology.
See [versioning](https://github.com/SlinkyProject/slurm-operator/blob/main/docs/versioning.md) for more details.
### 1.Y Releases
New Slinky versions may update the Slinky [CRDs](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions) with new fields and deprecate old ones. During CRD version changes (e.g. `v1beta1` => `v1beta2`), deprecated fields may be removed. Through the Kubernetes API, CRD versions are automatically converted to the stored version. Therefore old CRD versions will still work, but it is recommended to use the new CRD version as indicated by the installed Slinky CRDs.
To upgrade between Slinky `v1.Y` versions (e.g. `v1.0.Z` => `v1.1.Z`), upgrade the slurm-operator-crds chart followed by the slurm-operator chart, or both at the same time by upgrading the slurm-operator chart when using `crds.enabled=true`.

```
helm upgrade slurm-operator-crds oci://ghcr.io/slinkyproject/charts/slurm-operator-crds \
  --version $SLINKY_VERSION
helm upgrade slurm-operator oci://ghcr.io/slinkyproject/charts/slurm-operator \
  --namespace slinky --version $SLINKY_VERSION
```

All Slurm charts may remain on the old Slinky release series (e.g. `v1.0.x`) despite the slurm-operator and its CRDs being on a newer Slinky release series (e.g. `v1.1.x`). It is still recommended to upgrade Slurm charts to the new Slinky release series coinciding with the slurm-operator's Slinky release series to make use of the new fields, features, and functionality.
Please review changes made to the CRDs and the Slurm chart. Update your `values.yaml` appropriately and upgrade the Slurm chart.

```
helm upgrade slurm oci://ghcr.io/slinkyproject/charts/slurm \
  --namespace slurm --version $SLINKY_VERSION
```

### 0.Y Releases
Breaking changes may be introduced into existing Slinky [CRDs](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions) versions. To upgrade between `v0.Y` versions (e.g. `v0.1.Z` => `v0.2.Z`), uninstall all Slinky charts and delete Slinky CRDs, then install the new release like normal.

```
helm --namespace=slurm uninstall slurm
helm --namespace=slinky uninstall slurm-operator
helm uninstall slurm-operator-crds
```

If the CRDs were not installed via `slurm-operator-crds` helm chart:

```
kubectl delete customresourcedefinitions.apiextensions.k8s.io accountings.slinky.slurm.net
kubectl delete customresourcedefinitions.apiextensions.k8s.io clusters.slinky.slurm.net # defunct
kubectl delete customresourcedefinitions.apiextensions.k8s.io loginsets.slinky.slurm.net
kubectl delete customresourcedefinitions.apiextensions.k8s.io nodesets.slinky.slurm.net
kubectl delete customresourcedefinitions.apiextensions.k8s.io restapis.slinky.slurm.net
kubectl delete customresourcedefinitions.apiextensions.k8s.io tokens.slinky.slurm.net
```

## Documentation
Project documentation is located in the docs directory of this repository.
[Slinky documentation](https://slinky.schedmd.com/) is hosted on the web.
## Support and Development
Feature requests, code contributions, and bug reports are welcome!
Github/Gitlab submitted issues and PRs/MRs are handled on a best effort basis.
The SchedMD official issue tracker is at <https://support.schedmd.com/>.
To schedule a demo or simply to reach out, please [contact SchedMD](https://www.schedmd.com/slurm-resources/contact-schedmd/).
## License
Copyright (C) SchedMD LLC.
Licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0) you may not use project except in compliance with the license.
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
## About
Run Slurm on Kubernetes. A Slinky project.
### Topics
[helm-charts](https://github.com/topics/helm-charts)[high-performance-computing](https://github.com/topics/high-performance-computing)[high-throughput-computing](https://github.com/topics/high-throughput-computing)[hpc](https://github.com/topics/hpc)[k8s-operator](https://github.com/topics/k8s-operator)[kubernetes](https://github.com/topics/kubernetes)[schedmd](https://github.com/topics/schedmd)[slinky](https://github.com/topics/slinky)[slurm](https://github.com/topics/slurm)[slurm-workload-manager](https://github.com/topics/slurm-workload-manager)
### Resources
### Code of conduct
### Contributing
### Security policy
Cite this repository
### Stars
**359** stars
### Watchers
**19** watching
### Forks
[ forks](https://github.com/SlinkyProject/slurm-operator/forks)
## 
[24tags](https://github.com/SlinkyProject/slurm-operator/tags)
## 
## Languages
  * [Go94.9%](https://github.com/SlinkyProject/slurm-operator/search?l=go)
  * [Go Template1.8%](https://github.com/SlinkyProject/slurm-operator/search?l=go-template)
  * [Shell1.7%](https://github.com/SlinkyProject/slurm-operator/search?l=shell)
  * [Makefile1.3%](https://github.com/SlinkyProject/slurm-operator/search?l=makefile)
  * [HCL0.2%](https://github.com/SlinkyProject/slurm-operator/search?l=hcl)
  * [Dockerfile0.1%](https://github.com/SlinkyProject/slurm-operator/search?l=dockerfile)


You can’t perform that action at this time. 
