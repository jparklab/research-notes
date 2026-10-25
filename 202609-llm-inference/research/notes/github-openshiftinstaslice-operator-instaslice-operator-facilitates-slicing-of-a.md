---
title: 'GitHub - openshift/instaslice-operator: InstaSlice Operator facilitates slicing
  of accelerators using stable APIs · GitHub'
id: github-openshiftinstaslice-operator-instaslice-operator-facilitates-slicing-of-a
tags:
- onprem-llm-inference-stacks-859099
- gpu-sharing
- provenance-vendor-docs
- primary-source
created: '2026-10-23T17:24:22.582723Z'
updated: '2026-10-23T17:25:06.788642Z'
source: https://github.com/openshift/instaslice-operator
source_domain: github.com
fetched_at: '2026-10-23T17:24:22.554194Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Red Hat openshift/instaslice-operator (upstream of the Dynamic Accelerator
  Slicer): just-in-time MIG slice allocation for pods; repo shows archived by owner
  Aug 11, 2026 (read-only), last commit May 2, 2026 - signal of project lifecycle
  churn.'
---

*Suggested by [[gpu-sharing-on-k8s-mig-vs-mps-vs-time-slicing-luca-berton]] — primary/vendor doc for GPU sharing claims*

[Skip to content](https://github.com/openshift/instaslice-operator#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/openshift/instaslice-operator) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/openshift/instaslice-operator) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/openshift/instaslice-operator) to refresh your session. Dismiss alert
This repository was archived by the owner on Aug 11, 2026. It is now read-only. 
[**50** Branches](https://github.com/openshift/instaslice-operator/branches)[](https://github.com/openshift/instaslice-operator/tags)
Go to file
Open more actions menu
## Latest commit
[NO-JIRA: Update instaslice-webhook-next to 828bc64 (](https://github.com/openshift/instaslice-operator/commit/4956790960a1a84a2fbdc28376f8005a49f64b4b)[#1091](https://github.com/openshift/instaslice-operator/pull/1091)[)](https://github.com/openshift/instaslice-operator/commit/4956790960a1a84a2fbdc28376f8005a49f64b4b)
Open commit detailssuccess
May 2, 2026
[4956790](https://github.com/openshift/instaslice-operator/commit/4956790960a1a84a2fbdc28376f8005a49f64b4b) · May 2, 2026
## History
[966 Commits](https://github.com/openshift/instaslice-operator/commits/next/)
Open commit details
966 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| [.github/workflows](https://github.com/openshift/instaslice-operator/tree/next/.github/workflows "This path skips through empty directories")  | [.github/workflows](https://github.com/openshift/instaslice-operator/tree/next/.github/workflows "This path skips through empty directories")  |   | Jun 26, 2025  |  
|   |   |  [NO-JIRA: Update Konflux references (](https://github.com/openshift/instaslice-operator/commit/f66744be97511be83d66bfe92f0e9f4062965a5f "NO-JIRA: Update Konflux references \(#1089\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")[#1089](https://github.com/openshift/instaslice-operator/pull/1089)[)](https://github.com/openshift/instaslice-operator/commit/f66744be97511be83d66bfe92f0e9f4062965a5f "NO-JIRA: Update Konflux references \(#1089\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")  | May 2, 2026  |  
|   |   |  [OCPNODE-4123: add network policies (](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#1013](https://github.com/openshift/instaslice-operator/pull/1013)[)](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | Mar 16, 2026  |  
|   |   |  [OCPNODE-4123: add network policies (](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#1013](https://github.com/openshift/instaslice-operator/pull/1013)[)](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | Mar 16, 2026  |  
|  [fix lints and remove old controller (](https://github.com/openshift/instaslice-operator/commit/741cc43064f000a96247029f1ded1eab2e7ab655 "fix lints and remove old controller \(#800\)

* fix lints

* remove instaslicecontroller since it is not used")[#800](https://github.com/openshift/instaslice-operator/pull/800)[)](https://github.com/openshift/instaslice-operator/commit/741cc43064f000a96247029f1ded1eab2e7ab655 "fix lints and remove old controller \(#800\)

* fix lints

* remove instaslicecontroller since it is not used")  | Jul 25, 2025  |  
|   |   |  [OCPNODE-4123: add network policies (](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#1013](https://github.com/openshift/instaslice-operator/pull/1013)[)](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | Mar 16, 2026  |  
|   | Jun 26, 2025  |  
|  [Ensure the dockerfiles are built for dev purpose from non-x86 arch (](https://github.com/openshift/instaslice-operator/commit/1aee9a71eddcf5ac74631efef45af1b2de99e2d4 "Ensure the dockerfiles are built for dev purpose from non-x86 arch \(#924\)

Signed-off-by: Harshal Patil <12152047+harche@users.noreply.github.com>")[#924](https://github.com/openshift/instaslice-operator/pull/924)  | Sep 19, 2025  |  
|   |   |  [Sync libnvidia-ml to 580.126.20 for GPU Operator 26.3.0(OCP-4.21) (](https://github.com/openshift/instaslice-operator/commit/8e80e6c3b7b59ed785506ffce237bc8d81bd01bc "Sync libnvidia-ml to 580.126.20 for GPU Operator 26.3.0\(OCP-4.21\) \(#1033\)

- Update Dockerfile.daemonset.ocp: driver 580.105.08 -> 580.126.20
- Update rpms.lock.yaml to match \(will be regenerated by cachi2\)
- Fixes NVML driver/library version mismatch in OCP 4.21 e2e tests

GPU Operator 26.3.0 uses NVIDIA driver 580.126.20 as the default.
The das-daemonset was failing with "nvml init failed: Driver/library
version mismatch" when compiled with golang 1.24 for OCP 4.21.

Signed-off-by: Sai Ramesh Vanka <svanka@redhat.com>")[#1033](https://github.com/openshift/instaslice-operator/pull/1033)  | Apr 2, 2026  |  
|   |   |  [Remove the emulated mode from API (](https://github.com/openshift/instaslice-operator/commit/b3e3d6799bd62b19a04c188205ee6390df087293 "Remove the emulated mode from API \(#777\)

Signed-off-by: Harshal Patil <harpatil@redhat.com>
Co-authored-by: Harshal Patil <harpatil@redhat.com>")[#777](https://github.com/openshift/instaslice-operator/pull/777)[)](https://github.com/openshift/instaslice-operator/commit/b3e3d6799bd62b19a04c188205ee6390df087293 "Remove the emulated mode from API \(#777\)

Signed-off-by: Harshal Patil <harpatil@redhat.com>
Co-authored-by: Harshal Patil <harpatil@redhat.com>")  | Jul 22, 2025  |  
|  [OCPNODE-4123: add network policies (](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#1013](https://github.com/openshift/instaslice-operator/pull/1013)[)](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | Mar 16, 2026  |  
|   |   |  [set CUDA_VISIBLE_DEVICES to device index instead of UUID (](https://github.com/openshift/instaslice-operator/commit/0ec1415f2ac51687e11f2f30a08f94d3fad33df4 "set CUDA_VISIBLE_DEVICES to device index instead of UUID \(#907\)

Signed-off-by: Harshal Patil <harpatil@redhat.com>")[#907](https://github.com/openshift/instaslice-operator/pull/907)[)](https://github.com/openshift/instaslice-operator/commit/0ec1415f2ac51687e11f2f30a08f94d3fad33df4 "set CUDA_VISIBLE_DEVICES to device index instead of UUID \(#907\)

Signed-off-by: Harshal Patil <harpatil@redhat.com>")  | Aug 25, 2025  |  
|  [OCPNODE-4123: add network policies (](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#1013](https://github.com/openshift/instaslice-operator/pull/1013)[)](https://github.com/openshift/instaslice-operator/commit/5c773c6570e3d4aa40865d42026081fd78759282 "OCPNODE-4123: add network policies \(#1013\)

* OCPNODE-4123: Add NetworkPolicy manifests for DAS operator

Add default-deny and allow NetworkPolicy resources to the das-operator
namespace to restrict ingress and egress traffic per OCPSTRAT-2061
requirements. This addresses CIS Kubernetes Benchmark 5.3.2 and
Red Hat ProdSec guidance for control plane hardening.

Network policies added:
- 99-deny-all: Default deny all ingress and egress
- 10-allow-egress-api: Allow egress to kube-apiserver \(port 6443\)
- 10-allow-egress-cluster-dns: Allow egress to openshift-dns
- 10-allow-ingress-webhook: Allow ingress to webhook \(port 8443\)

The operator reconciles these policies as part of its sync loop,
following the same pattern used by kueue-operator.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* OCPNODE-4002: Add e2e tests for NetworkPolicy validation

Add e2e tests that verify:
- All 4 expected NetworkPolicy resources are created in the
  das-operator namespace after operator deployment
- The operator reconciles deleted NetworkPolicies \(delete one,
  verify it gets recreated\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* add network policies rbac

---------

Co-authored-by: Harshal Patil <12152047+harche@users.noreply.github.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | Mar 16, 2026  |  
|   |   |  [OCPNODE-3918: Implement centralized TLS security profile support (](https://github.com/openshift/instaslice-operator/commit/97b94c8e941cc27cd04bb7344fbd0db68a80b273 "OCPNODE-3918: Implement centralized TLS security profile support \(#998\)

Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#998](https://github.com/openshift/instaslice-operator/pull/998)[)](https://github.com/openshift/instaslice-operator/commit/97b94c8e941cc27cd04bb7344fbd0db68a80b273 "OCPNODE-3918: Implement centralized TLS security profile support \(#998\)

Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | Mar 13, 2026  |  
|   |   |   | Mar 13, 2024  |  
|   |   |  [cleanup justfile and add groups (](https://github.com/openshift/instaslice-operator/commit/018e2bac84d03c399197d6f86ebce46c8d622c1c "cleanup justfile and add groups \(#775\)

* cleanup justfile and add groups

* lint and fix the shell scripts

tweaks

blah")[#775](https://github.com/openshift/instaslice-operator/pull/775)[)](https://github.com/openshift/instaslice-operator/commit/018e2bac84d03c399197d6f86ebce46c8d622c1c "cleanup justfile and add groups \(#775\)

* cleanup justfile and add groups

* lint and fix the shell scripts

tweaks

blah")  | Jul 22, 2025  |  
|   |   |  [cleanup justfile and add groups (](https://github.com/openshift/instaslice-operator/commit/018e2bac84d03c399197d6f86ebce46c8d622c1c "cleanup justfile and add groups \(#775\)

* cleanup justfile and add groups

* lint and fix the shell scripts

tweaks

blah")[#775](https://github.com/openshift/instaslice-operator/pull/775)[)](https://github.com/openshift/instaslice-operator/commit/018e2bac84d03c399197d6f86ebce46c8d622c1c "cleanup justfile and add groups \(#775\)

* cleanup justfile and add groups

* lint and fix the shell scripts

tweaks

blah")  | Jul 22, 2025  |  
|   |   |   | Jun 9, 2025  |  
|   |   |  [initial refactor of kind e2e tests (](https://github.com/openshift/instaslice-operator/commit/75a0f0a4dc17f65b8c3b30ce262b7af6f7a45c13 "initial refactor of kind e2e tests \(#203\)

* initial refactor of kind tests

sync

sync

sync

sync

* fix emulator mode")[#203](https://github.com/openshift/instaslice-operator/pull/203)[)](https://github.com/openshift/instaslice-operator/commit/75a0f0a4dc17f65b8c3b30ce262b7af6f7a45c13 "initial refactor of kind e2e tests \(#203\)

* initial refactor of kind tests

sync

sync

sync

sync

* fix emulator mode")  | Oct 30, 2024  |  
|   |   |  [cleanup justfile and add more functionality (](https://github.com/openshift/instaslice-operator/commit/4b8b640c508ced1b6704a0ed0702e4968c44c290 "cleanup justfile and add more functionality \(#711\)

* cleanup justfile and add more functionality

* update README")[#711](https://github.com/openshift/instaslice-operator/pull/711)[)](https://github.com/openshift/instaslice-operator/commit/4b8b640c508ced1b6704a0ed0702e4968c44c290 "cleanup justfile and add more functionality \(#711\)

* cleanup justfile and add more functionality

* update README")  | Jul 14, 2025  |  
|   |   |  [tekton: configure sast-snyk-check to upload results to Snyk UI (](https://github.com/openshift/instaslice-operator/commit/9365b242c17f780e0575735056d6712e69113370 "tekton: configure sast-snyk-check to upload results to Snyk UI \(#1060\)

Add ARGS parameter to all sast-snyk-check tasks across all 12 Tekton
pipeline files \(6 components x PR + push\). This configures the Snyk
CLI to upload SAST scan results to the Snyk UI with proper project
names and organization targeting.

Each component gets a unique --project-name to keep results organized:
- instaslice-operator
- instaslice-daemonset
- instaslice-scheduler
- instaslice-webhook
- instaslice-operator-bundle
- instaslice-operator-bundle-developer

All results are uploaded to the "Hybrid Platforms - Red Hat OpenShift
Container Platform" Snyk org \(86a5b6bf-8aad-4842-ab41-e5c7358c202e\).

Also remove vendor/** from .snyk exclusions so SAST scans cover
vendored Go dependencies. Test file exclusions \(**/*_test.go\) are
kept to avoid false positives from test fixtures.

Ref: https://konflux.pages.redhat.com/docs/users/testing/sast-tasks.html

Co-authored-by: lcelant <lcelant@redhat.com>")[#1060](https://github.com/openshift/instaslice-operator/pull/1060)[)](https://github.com/openshift/instaslice-operator/commit/9365b242c17f780e0575735056d6712e69113370 "tekton: configure sast-snyk-check to upload results to Snyk UI \(#1060\)

Add ARGS parameter to all sast-snyk-check tasks across all 12 Tekton
pipeline files \(6 components x PR + push\). This configures the Snyk
CLI to upload SAST scan results to the Snyk UI with proper project
names and organization targeting.

Each component gets a unique --project-name to keep results organized:
- instaslice-operator
- instaslice-daemonset
- instaslice-scheduler
- instaslice-webhook
- instaslice-operator-bundle
- instaslice-operator-bundle-developer

All results are uploaded to the "Hybrid Platforms - Red Hat OpenShift
Container Platform" Snyk org \(86a5b6bf-8aad-4842-ab41-e5c7358c202e\).

Also remove vendor/** from .snyk exclusions so SAST scans cover
vendored Go dependencies. Test file exclusions \(**/*_test.go\) are
kept to avoid false positives from test fixtures.

Ref: https://konflux.pages.redhat.com/docs/users/testing/sast-tasks.html

Co-authored-by: lcelant <lcelant@redhat.com>")  | Apr 16, 2026  |  
| [Dockerfile.daemonset.ocp](https://github.com/openshift/instaslice-operator/blob/next/Dockerfile.daemonset.ocp "Dockerfile.daemonset.ocp")  | [Dockerfile.daemonset.ocp](https://github.com/openshift/instaslice-operator/blob/next/Dockerfile.daemonset.ocp "Dockerfile.daemonset.ocp")  |  [NO-JIRA: Update registry.redhat.io/ubi9/ubi Docker digest to 0879eaf (](https://github.com/openshift/instaslice-operator/commit/6c66977d7149944429a24dae49fae5071c65299b "NO-JIRA: Update registry.redhat.io/ubi9/ubi Docker digest to 0879eaf \(#1086\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")[#…](https://github.com/openshift/instaslice-operator/pull/1086)  | Apr 30, 2026  |  
|   |   | [NO-JIRA: Update registry.redhat.io/ubi9/ubi-minimal Docker digest to …](https://github.com/openshift/instaslice-operator/commit/d7e00436f5e487b66d09711dec097b2a9fcc7e3e "NO-JIRA: Update registry.redhat.io/ubi9/ubi-minimal Docker digest to 7d4e475 \(#1074\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")  | Apr 24, 2026  |  
| [Dockerfile.scheduler.ocp](https://github.com/openshift/instaslice-operator/blob/next/Dockerfile.scheduler.ocp "Dockerfile.scheduler.ocp")  | [Dockerfile.scheduler.ocp](https://github.com/openshift/instaslice-operator/blob/next/Dockerfile.scheduler.ocp "Dockerfile.scheduler.ocp")  | [NO-JIRA: Update registry.redhat.io/ubi9/ubi-minimal Docker digest to …](https://github.com/openshift/instaslice-operator/commit/d7e00436f5e487b66d09711dec097b2a9fcc7e3e "NO-JIRA: Update registry.redhat.io/ubi9/ubi-minimal Docker digest to 7d4e475 \(#1074\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")  | Apr 24, 2026  |  
|   |   | [NO-JIRA: Update registry.redhat.io/ubi9/ubi-minimal Docker digest to …](https://github.com/openshift/instaslice-operator/commit/d7e00436f5e487b66d09711dec097b2a9fcc7e3e "NO-JIRA: Update registry.redhat.io/ubi9/ubi-minimal Docker digest to 7d4e475 \(#1074\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")  | Apr 24, 2026  |  
|   |   |  [Increase the timeout to run e2e tests (](https://github.com/openshift/instaslice-operator/commit/dceadf44909f9231ce88dc2e200d79a26afad732 "Increase the timeout to run e2e tests \(#1043\)

Signed-off-by: Sai Ramesh Vanka <svanka@redhat.com>")[#1043](https://github.com/openshift/instaslice-operator/pull/1043)[)](https://github.com/openshift/instaslice-operator/commit/dceadf44909f9231ce88dc2e200d79a26afad732 "Increase the timeout to run e2e tests \(#1043\)

Signed-off-by: Sai Ramesh Vanka <svanka@redhat.com>")  | Apr 8, 2026  |  
|   |   |   | Oct 8, 2024  |  
|   |   |   | Jul 17, 2025  |  
|   |   |   | Jul 8, 2025  |  
|   |   | [move away from codeflare and org.instaslice](https://github.com/openshift/instaslice-operator/commit/a961349165bebb08b84e2e9ea5a074044ec1c69a "move away from codeflare and org.instaslice")  | Oct 21, 2024  |  
|   |   |  [Update README.md to include missing step for Operator Custom Resource (](https://github.com/openshift/instaslice-operator/commit/d6d6f4f7208d0911684b303fead468dcf40016a9 "Update README.md to include missing step for Operator Custom Resource \(#897\)

Added steps to install NFD and Nvidia GPU operator

Co-authored-by: Bhargavi Gudi <bgudi@bgudi-thinkpadp16vgen1.bengluru.csb>")[…](https://github.com/openshift/instaslice-operator/pull/897)  | Aug 14, 2025  |  
|   |   |  [update bundle description, images and maintainers (](https://github.com/openshift/instaslice-operator/commit/99405c97edb2362c07fe16a0bc4fbcc871258b51 "update bundle description, images and maintainers \(#233\)

Signed-off-by: MohammedAbdi <mohammma@usc.edu>")[#233](https://github.com/openshift/instaslice-operator/pull/233)[)](https://github.com/openshift/instaslice-operator/commit/99405c97edb2362c07fe16a0bc4fbcc871258b51 "update bundle description, images and maintainers \(#233\)

Signed-off-by: MohammedAbdi <mohammma@usc.edu>")  | Nov 6, 2024  |  
|   |   |  [NO-JIRA: Update registry.redhat.io/ubi9/ubi Docker digest to 0879eaf (](https://github.com/openshift/instaslice-operator/commit/6c66977d7149944429a24dae49fae5071c65299b "NO-JIRA: Update registry.redhat.io/ubi9/ubi Docker digest to 0879eaf \(#1086\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")[#…](https://github.com/openshift/instaslice-operator/pull/1086)  | Apr 30, 2026  |  
| [bundle.developer.Dockerfile](https://github.com/openshift/instaslice-operator/blob/next/bundle.developer.Dockerfile "bundle.developer.Dockerfile")  | [bundle.developer.Dockerfile](https://github.com/openshift/instaslice-operator/blob/next/bundle.developer.Dockerfile "bundle.developer.Dockerfile")  |  [NO-JIRA: Update registry.redhat.io/ubi9/ubi Docker digest to 0879eaf (](https://github.com/openshift/instaslice-operator/commit/6c66977d7149944429a24dae49fae5071c65299b "NO-JIRA: Update registry.redhat.io/ubi9/ubi Docker digest to 0879eaf \(#1086\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")[#…](https://github.com/openshift/instaslice-operator/pull/1086)  | Apr 30, 2026  |  
|   |   |   | Aug 11, 2025  |  
|   |   |   | Aug 11, 2025  |  
|   |   | [add deploy directory and run make generate](https://github.com/openshift/instaslice-operator/commit/ff71db83eb310179b5b4984316fefb7cd5838e2d "add deploy directory and run make generate")  | May 21, 2025  |  
| [related_images.developer.json](https://github.com/openshift/instaslice-operator/blob/next/related_images.developer.json "related_images.developer.json")  | [related_images.developer.json](https://github.com/openshift/instaslice-operator/blob/next/related_images.developer.json "related_images.developer.json")  |  [NO-JIRA: Update instaslice-webhook-next to 828bc64 (](https://github.com/openshift/instaslice-operator/commit/4956790960a1a84a2fbdc28376f8005a49f64b4b "NO-JIRA: Update instaslice-webhook-next to 828bc64 \(#1091\)

Image created from 'https://github.com/openshift/instaslice-operator?rev=f66744be97511be83d66bfe92f0e9f4062965a5f'

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")[#1091](https://github.com/openshift/instaslice-operator/pull/1091)[)](https://github.com/openshift/instaslice-operator/commit/4956790960a1a84a2fbdc28376f8005a49f64b4b "NO-JIRA: Update instaslice-webhook-next to 828bc64 \(#1091\)

Image created from 'https://github.com/openshift/instaslice-operator?rev=f66744be97511be83d66bfe92f0e9f4062965a5f'

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")  | May 2, 2026  |  
|   |   |  [NO-JIRA: Update instaslice-webhook-next to 828bc64 (](https://github.com/openshift/instaslice-operator/commit/4956790960a1a84a2fbdc28376f8005a49f64b4b "NO-JIRA: Update instaslice-webhook-next to 828bc64 \(#1091\)

Image created from 'https://github.com/openshift/instaslice-operator?rev=f66744be97511be83d66bfe92f0e9f4062965a5f'

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")[#1091](https://github.com/openshift/instaslice-operator/pull/1091)[)](https://github.com/openshift/instaslice-operator/commit/4956790960a1a84a2fbdc28376f8005a49f64b4b "NO-JIRA: Update instaslice-webhook-next to 828bc64 \(#1091\)

Image created from 'https://github.com/openshift/instaslice-operator?rev=f66744be97511be83d66bfe92f0e9f4062965a5f'

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>")  | May 2, 2026  |  
|   |   |  [add matchUpdateTypes to renovate (](https://github.com/openshift/instaslice-operator/commit/60393d6fe3ae454a88b5a37a7e5834c142f12449 "add matchUpdateTypes to renovate \(#828\)")[#828](https://github.com/openshift/instaslice-operator/pull/828)[)](https://github.com/openshift/instaslice-operator/commit/60393d6fe3ae454a88b5a37a7e5834c142f12449 "add matchUpdateTypes to renovate \(#828\)")  | Jul 30, 2025  |  
| View all files |  
## Repository files navigation
# Dynamic Accelerator Slicer (DAS) Operator
Dynamic Accelerator Slicer (DAS) is an operator that dynamically partitions GPU accelerators in Kubernetes and OpenShift. It currently ships with a reference implementation for NVIDIA Multi-Instance GPU (MIG) and is designed to support additional technologies such as NVIDIA MPS or GPUs from other vendors.
Minimum supported OpenShift versions: 4.18.21 and 4.19.6.
## Table of Contents
  * [Dynamic Accelerator Slicer (DAS) Operator](https://github.com/openshift/instaslice-operator#dynamic-accelerator-slicer-das-operator)
    * [Table of Contents](https://github.com/openshift/instaslice-operator#table-of-contents)
    * [Getting Started](https://github.com/openshift/instaslice-operator#getting-started)
      * [OpenShift with GPU Hardware](https://github.com/openshift/instaslice-operator#openshift-with-gpu-hardware)
      * [Development Workflow](https://github.com/openshift/instaslice-operator#development-workflow)
      * [Operator Bundle Development](https://github.com/openshift/instaslice-operator#operator-bundle-development)
        * [Using a base CSV for bundle generation](https://github.com/openshift/instaslice-operator#using-a-base-csv-for-bundle-generation)
      * [Emulated Mode](https://github.com/openshift/instaslice-operator#emulated-mode)
    * [Justfile Usage](https://github.com/openshift/instaslice-operator#justfile-usage)
      * [Prerequisites](https://github.com/openshift/instaslice-operator#prerequisites)
      * [Available Commands](https://github.com/openshift/instaslice-operator#available-commands)
      * [Development and Testing](https://github.com/openshift/instaslice-operator#development-and-testing)
      * [Bundle Operations](https://github.com/openshift/instaslice-operator#bundle-operations)
      * [NVIDIA GPU Operator Management](https://github.com/openshift/instaslice-operator#nvidia-gpu-operator-management)
      * [Cert Manager Operations](https://github.com/openshift/instaslice-operator#cert-manager-operations)
      * [Node Feature Discovery](https://github.com/openshift/instaslice-operator#node-feature-discovery)
      * [Building and Pushing Images](https://github.com/openshift/instaslice-operator#building-and-pushing-images)
      * [Use custom developer images](https://github.com/openshift/instaslice-operator#use-custom-developer-images)
      * [Configuration](https://github.com/openshift/instaslice-operator#configuration)
    * [Architecture](https://github.com/openshift/instaslice-operator#architecture)
      * [MIG scheduler plugin](https://github.com/openshift/instaslice-operator#mig-scheduler-plugin)
      * [AllocationClaim resource](https://github.com/openshift/instaslice-operator#allocationclaim-resource)
    * [Running Tests](https://github.com/openshift/instaslice-operator#running-tests)
      * [End-to-End Tests](https://github.com/openshift/instaslice-operator#end-to-end-tests)


## Features
  * On-demand partitioning of GPUs via a custom Kubernetes operator.
  * Scheduler integration that allocates NVIDIA MIG slices through a plugin located at [`pkg/scheduler/plugins/mig/mig.go`](https://github.com/openshift/instaslice-operator/blob/next/pkg/scheduler/plugins/mig/mig.go).
  * `AllocationClaim` custom resource to track slice reservations (`pkg/apis/dasoperator/v1alpha1/allocation_types.go`).
  * Emulated mode to exercise the workflow without real hardware.


## Getting Started
This project uses [just](https://github.com/casey/just) for task automation. Install just first:

```
# On macOS
brew install just

# On Fedora/RHEL
dnf install just

# On Ubuntu/Debian
apt install just

# Or via cargo
cargo install just
```

### Quick Start
  1. **Configure your images** by editing `related_images.your-username.json` with your registry:

```
[
  {"name": "instaslice-operator-next", "image": "quay.io/your-username/instaslice-operator:latest"},
  {"name": "instaslice-webhook-next", "image": "quay.io/your-username/instaslice-webhook:latest"},
  {"name": "instaslice-scheduler-next", "image": "quay.io/your-username/instaslice-scheduler:latest"},
  {"name": "instaslice-daemonset-next", "image": "quay.io/your-username/instaslice-daemonset:latest"}
]
```

  2. **Build and push all images** :

```
just build-push-parallel
```

  3. **Deploy to OpenShift** (with emulated mode for development):

```
export EMULATED_MODE=enabled
export RELATED_IMAGES=related_images.your-username.json
just deploy-das-ocp
```

  4. **Test the installation** :

```
kubectl apply -f test/test-pod-emulated.yaml
```



### OpenShift with GPU Hardware
For OpenShift clusters with GPU hardware:
  1. **Deploy prerequisites** :

```
just deploy-cert-manager-ocp
just deploy-nfd-ocp
just deploy-nvidia-ocp
```

  2. **Deploy DAS operator** :

```
export EMULATED_MODE=disabled
export RELATED_IMAGES=related_images.your-username.json
just deploy-das-ocp
```

  3. **Test with GPU workload** :

```
kubectl apply -f test/test-pod.yaml
```



### Development Workflow
For local development:
  1. **Run operator locally** (requires scheduler, webhook, and daemonset images to be built and pushed beforehand):

```
# Build and push images first
just build-push-parallel

# Run operator locally
# Set EMULATED_MODE to control hardware emulation
EMULATED_MODE=enabled just run-local
```

  2. **Run tests** :

```
just test-e2e
```

  3. **Check code quality** :

```
just lint
```



### Operator Bundle Development
  1. Login into podman and have a repository created for the operator bundle.
  2. Set `BUNDLE_IMAGE` to point to your repository and tag of choice.
  3. Run `just bundle-generate` to generate the bundle manifests.
  4. Run `just build-push-bundle` to build and push the bundle image to your repository.
  5. Run `just deploy-cert-manager-ocp` to install cert-manager on OpenShift.
  6. Run `just deploy-nfd-ocp` to install Node Feature Discovery (NFD) on OpenShift.
  7. Run `just deploy-nvidia-ocp` to install NVIDIA GPU operator on Openshift.
  8. Run `operator-sdk run bundle --namespace <namespace> ${BUNDLE_IMAGE}` to deploy the operator.
  9. Apply the `DASOperator` custom resource to initialize the operator 

```
kubectl apply -f deploy/03_instaslice_operator.cr.yaml
```



#### Using a base CSV for bundle generation
Running `generate bundle` is the first step to publishing an operator to a catalog and deploying it with OLM. A CSV manifest is generated by collecting data from the set of manifests passed to this command, such as CRDs, RBAC, etc., and applying that data to a "base" CSV manifest. The steps to provide a base CSV:
  * create a base CSV file that contains the desired metadata, the base CSV file name can be arbitrary, we can follow the convention `{operator-name}.base.clusterserviceverison.yaml`
  * put the base CSV file in the `deploy` folder. This is the folder from which the `generate bundle` command will collect the k8s manifests. Note that the base CSV file can be placed inside a sub-directory within the `deploy` folder.
  * make sure that the `metadata.name` of the base CSV is the same name as the package name provided to the `generate bundle` command, otherwise the `generate bundle` command will ignore the base CSV and will generate on an empty CSV.


Layout of an example `deploy` folder:

```
tree deploy/
  deploy/
  ├── crds
  │   └── foo-operator.crd.yaml
  ├── base-csv
  │   └── foo-operator.base.clusterserviceversion.yaml
  ├── deployment.yaml
  ├── role.yaml
  ├── role_binding.yaml
  ├── service_account.yaml
  └── webhooks.yaml
```

The bundle generation command:

```
operator-sdk generate bundle --input-dir deploy --version 0.1.0 --output-dir=bundle --package foo-operator
```

The base CSV yaml:

```
apiVersion: operators.coreos.com/v1alpha1
kind: ClusterServiceVersion
metadata:
  name: foo-operator.base
  annotations:
    alm-examples:
    # other annotations can be placed here
spec:
  displayName: Instaslice
  version: 0.0.2
  apiservicedefinitions:
  customresourcedefinitions:
  install:
  installModes:
  - supported: false
    type: OwnNamespace
  - supported: false
    type: SingleNamespace
  - supported: false
    type: MultiNamespace
  - supported: true
    type: AllNamespaces
  maturity: alpha
  minKubeVersion: 1.16.0
  provider:
    name: Codeflare
    url: https://github.com/openshift/instaslice-operator
  relatedImages:
  keywords:
  - Foo
  links:
  - name: My Operator
    url: https://github.com/foo/bar
  maintainers:
  description:
  icon:
```

  * There is no need to provide any permission, or deployment spec inside the base CSV.
  * Note that the `metadata.name` of the base CSV has a prefix of `foo-operator.` which adheres to the format `{package name}.`
  * if there are multiple CSV files inside the deploy folder, the one encountered first in lexical order will be selected as the base CSV


The CSV generation details can be found by inspecting the bundle generation code here: <https://github.com/operator-framework/operator-sdk/blob/0eefc52889ff3dfe4af406038709e6c5ba7398e5/internal/generate/clusterserviceversion/clusterserviceversion.go#L148-L159>
### Emulated Mode
Emulated mode allows the operator to publish synthetic GPU capacity and skip NVML calls. This is handy for development and CI environments with no hardware. Emulated mode is controlled via the `EMULATED_MODE` environment variable.
#### Configuration
The `EMULATED_MODE` environment variable is read by the operator at startup and determines how the daemonset components behave:
  * **`disabled`**(default): Normal operation mode that requires real MIG compatible GPUs hardware and makes NVML calls
  * **`enabled`**: Emulated mode that simulates MIG capable GPUs capacity without requiring actual hardware


#### Setting Emulated Mode
**For local development:**

```
# Run operator locally with emulation
EMULATED_MODE=enabled just run-local
```

**For deployment:**

```
# Deploy with emulated mode enabled
export EMULATED_MODE=enabled
export RELATED_IMAGES=related_images.your-username.json
just deploy-das-ocp
```

**For production with MIG Compatible GPUs:**

```
# Deploy with emulated mode disabled (default)
export EMULATED_MODE=disabled
export RELATED_IMAGES=related_images.your-username.json
just deploy-das-ocp
```

#### How it Works
The operator reads the `EMULATED_MODE` environment variable at startup and passes this configuration to the daemonset pods running on each node. When emulated mode is enabled:
  1. The daemonset skips hardware detection and NVML library calls
  2. Synthetic GPU resources are published to simulate hardware capacity
  3. MIG slicing operations are simulated rather than performed on real hardware


This allows for testing and development of the operator functionality without requiring physical GPU hardware.
## Justfile Usage
This project includes a [Justfile](https://github.com/casey/just) for convenient task automation. The Justfile provides several commands for building, pushing, and deploying the operator components.
### Prerequisites
Install [just](https://github.com/casey/just) command runner:

```
# On macOS
brew install just

# On Fedora/RHEL
dnf install just

# On Ubuntu/Debian
apt install just

# Or via cargo
cargo install just
```

### Available Commands
List all available commands:

```
just
```

View current configuration:

```
just info
```

### Development and Testing
Run the operator locally for development:

```
# Set EMULATED_MODE to 'enabled' for simulated GPUs or 'disabled' for real hardware
EMULATED_MODE=enabled just run-local
```

Run end-to-end tests:

```
just test-e2e
```

Run tests with a specific focus:

```
just test-e2e focus="GPU slices"
```

### Bundle Operations
Generate operator bundle:

```
just bundle-generate
```

Build and push bundle image:

```
just build-push-bundle
```

Build and push developer bundle:

```
just build-push-developer-bundle
```

### NVIDIA GPU Operator Management
Deploy NVIDIA GPU operator to OpenShift:

```
just deploy-nvidia-ocp
```

Remove NVIDIA GPU operator from OpenShift:

```
just undeploy-nvidia-ocp
```

### Cert Manager Operations
Deploy cert-manager for OpenShift:

```
just deploy-cert-manager-ocp
```

Remove cert-manager from OpenShift:

```
just undeploy-cert-manager-ocp
```

Deploy cert-manager for Kubernetes:

```
just deploy-cert-manager
```

### Node Feature Discovery
Deploy Node Feature Discovery (NFD) operator for OpenShift:

```
just deploy-nfd-ocp
```

### Code Quality
Run all linting (markdown and Go):

```
just lint
```

Run all linting with automatic fixes:

```
just lint-fix
```

Run only Go linting:

```
just lint-go
```

Run only markdown linting:

```
just lint-md
```

Run Go linting and automatically fix issues:

```
just lint-go-fix
```

Run markdown linting and automatically fix issues:

```
just lint-md-fix
```

### Cleanup
Clean up all deployed Kubernetes resources:

```
just undeploy
```

### Building and Pushing Images
Build and push individual component images:

```
just build-push-scheduler   # Build and push scheduler image
just build-push-daemonset   # Build and push daemonset image
just build-push-operator    # Build and push operator image
just build-push-webhook     # Build and push webhook image
```

Build and push all images in parallel:

```
just build-push-parallel
```

### Deployment
Deploy DAS on OpenShift Container Platform:

```
just deploy-das-ocp
```

Generate CRDs and clients:

```
just regen-crd           # Generate CRDs into manifests directory
just regen-crd-k8s       # Generate CRDs directly into deploy directory
just generate-clients    # Generate client code
just verify-codegen      # Verify generated client code is up to date
just generate            # Generate all - CRDs and clients
```

### Use custom developer images
Copy related_images.developer.json to related_images.username.json to use as a template and modify it to contain the target developer image repositories to use.

```
cp related_images.developer.json related_images.username.json
# Edit related_images.username.json with your registry
quay.io/username/image:latest
```

Then set the RELATED_IMAGES environment variable to related_images.username.json.

```
RELATED_IMAGES=related_images.username.json just
```

### Configuration
The Justfile uses environment variables for configuration. You can customize these by setting them in your environment or creating a `.env` file:
  * `PODMAN` - Container runtime (default: `podman`)
  * `KUBECTL` - Kubernetes CLI (default: `oc`)
  * `EMULATED_MODE` - Enable emulated mode (default: `disabled`)
  * `RELATED_IMAGES` - Path to related images JSON file (default: `related_images.json`)
  * `DEPLOY_DIR` - Deployment directory (default: `deploy`)
  * `OPERATOR_SDK` - Operator SDK binary (default: `operator-sdk`)
  * `OPERATOR_VERSION` - Operator version for bundle generation (default: `0.1.0`)
  * `GOLANGCI_LINT` - Golangci-lint binary (default: `golangci-lint`)


Example:

```
export EMULATED_MODE=enabled
just deploy-das-ocp
```

## Architecture
The diagram below summarizes how the operator components interact. Pods requesting GPU slices are mutated by a webhook to use the `mig.das.com` extended resource. The scheduler plugin tracks slice availability and creates `AllocationClaim` objects processed by the device plugin on each node.
### MIG scheduler plugin
The plugin integrates with the Kubernetes scheduler and runs through three framework phases:
  * **Filter** – ensures the node is MIG capable and stages `AllocationClaim`s for suitable GPUs.
  * **Score** – prefers nodes with the most free MIG slice slots after considering existing and staged claims.
  * **PreBind** – promotes staged claims on the selected node to `created` and removes the rest.


Once promoted, the device plugin provisions the slices.
The daemonset advertises GPU resources only after the NVIDIA GPU Operator's `ClusterPolicy` reports a **Ready** state. This prevents the scheduler from scheduling pods on a node before the GPU Operator has initialized the drivers.
### AllocationClaim resource
`AllocationClaim` is a namespaced CRD that records which MIG slice will be prepared for a pod. Claims start in the `staged` state and transition to `created` once all requests are satisfied. Each claim stores the GPU UUID, slice position and pod reference.
Example:

```
$ kubectl get allocationclaims -n das-operator
NAME                                          AGE
8835132e-8a7a-4766-a78f-0cb853d165a2-busy-0   61s
```


```
$ kubectl get allocationclaims -n das-operator -o yaml
apiVersion: inference.redhat.com/v1alpha1
kind: AllocationClaim
...
```

## Debugging
All components run in the `das-operator` namespace:

```
kubectl get pods -n das-operator
```

Inspect the active claims:

```
kubectl get allocationclaims -n das-operator
```

On the node, verify that the CDI devices were created:

```
ls -l /var/run/cdi/
```

Increase verbosity by editing the `DASOperator` resource and setting `operatorLogLevel` to `Debug` or `Trace`.
## Running Tests
### Unit Tests
Run all unit tests for the project:

```
make test
```

Run unit tests with verbose output:

```
go test -v ./pkg/...
```

Run unit tests with coverage:

```
go test -cover ./pkg/...
```

### End-to-End Tests
A running cluster with a valid `KUBECONFIG` is required:

```
just test-e2e
```

You can focus on specific tests:

```
just test-e2e focus="GPU slices"
```

### Known Issues
Due to [kubernetes/kubernetes#128043](https://github.com/kubernetes/kubernetes/issues/128043) pods may enter an `UnexpectedAdmissionError` state if admission fails. Pods managed by higher level controllers such as Deployments will be recreated automatically. Naked pods, however, must be cleaned up manually with `kubectl delete pod`. Using controllers is recommended until the upstream issue is resolved.
## Uninstalling
Remove the deployed resources with:

```
just undeploy
```

## Contributing
Contributions are welcome! Please open issues or pull requests.
## License
This project is licensed under the Apache 2.0 License.
## About
InstaSlice Operator facilitates slicing of accelerators using stable APIs
### Resources
### Security policy
### Stars
**51** stars
### Watchers
**7** watching
### Forks
[ forks](https://github.com/openshift/instaslice-operator/forks)
## 
[3tags](https://github.com/openshift/instaslice-operator/tags)
## 
## Languages
  * [Go90.5%](https://github.com/openshift/instaslice-operator/search?l=go)
  * [Shell5%](https://github.com/openshift/instaslice-operator/search?l=shell)
  * [Just2.6%](https://github.com/openshift/instaslice-operator/search?l=just)
  * [Dockerfile1.5%](https://github.com/openshift/instaslice-operator/search?l=dockerfile)
  * [Makefile0.4%](https://github.com/openshift/instaslice-operator/search?l=makefile)


You can’t perform that action at this time. 
openshift/instaslice-operator [ openshift/instaslice-operator ](https://github.com/openshift/instaslice-operator) on  Jun 23, 2025 
#### [Moving to new design #633 ](https://github.com/openshift/instaslice-operator/pull/633)
Merged 
This PR moves the implementation to the new design. Details in the included README.
next openshift:next  ← device_plugin_next harche:device_plugin_next 
size/XXL  approved 
Press escape to close this hovercard 
