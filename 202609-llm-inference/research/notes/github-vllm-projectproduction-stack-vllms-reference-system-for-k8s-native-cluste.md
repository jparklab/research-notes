---
title: 'GitHub - vllm-project/production-stack: vLLM’s reference system for K8S-native
  cluster-wide deployment with community-driven performance optimization · GitHub'
id: github-vllm-projectproduction-stack-vllms-reference-system-for-k8s-native-cluste
tags:
- internal-llm-inference-stacks-7f0c90
- production-stack
- lmcache
- layer-boundaries
created: '2026-10-09T23:02:43.469838Z'
updated: '2026-10-10T14:52:17.342357Z'
source: https://github.com/vllm-project/production-stack
source_domain: github.com
fetched_at: '2026-10-09T23:02:43.445665Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'vLLM production-stack GitHub repo (Apache 2.0, 2.7k stars, 29 watchers,
  vllm-project org), released 22 Jan 2025 per its own release blog at lmcache.ai --
  the third ''alternative'' named in llm-d''s founding proposal alongside Dynamo and
  AIBrix, maintained by the LMCache team (contacts @ruizhang0101, @ApostaC, @YuhanLiu11,
  @Shaoting-Feng; blog hosted at lmcache.github.io, confirming University of Chicago
  LMCache-team governance claimed elsewhere in this batch). Self-describes narrowly
  as ''a reference implementation on how to build an inference stack on top of vLLM''
  that lets users ''scale from a single vLLM instance to a distributed vLLM deployment
  without changing any application code.'' Architecture is three parts: Serving engine
  (vLLM), Request router (routes by routing key/session ID to maximize KV cache reuse;
  supports round-robin, session-ID-based, and prefix-aware routing marked Work-In-Progress),
  and Observability stack (Prometheus+Grafana). At fetch time its own Roadmap lists
  autoscaling, disaggregated prefill support, and router improvements (non-Python
  router, KV-cache-aware routing, better fault tolerance) as NOT YET BUILT (''We are
  actively working on this project and will release the following features soon'')
  -- confirming production-stack is deliberately the lightest-weight of the four Kubernetes-native
  options discussed in this batch (llm-d/AIBrix/Dynamo/production-stack), explicitly
  scoped to ''the easiest way to deploy vLLM on Kubernetes'' per llm-d''s own characterization,
  not a full control plane with disaggregation or advanced routing yet. Deployment
  is a single Helm chart install against an existing Kubernetes+GPU cluster.'
---

*Suggested by [[llm-d-kubernetes-native-distributed-inference-at-scale-llm-d]] — named as the third alternative in llm-d founding proposal's Alternatives section, UChicago LMCache team project*

[Skip to content](https://github.com/vllm-project/production-stack#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/production-stack) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/production-stack) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/production-stack) to refresh your session. Dismiss alert
[**3** Branches](https://github.com/vllm-project/production-stack/branches)[](https://github.com/vllm-project/production-stack/tags)
Go to file
Open more actions menu
## Latest commit
[Fix: Use dashes instead of underscores for --gpu-memory-utilization f…](https://github.com/vllm-project/production-stack/commit/4e076e8de3698a3729b575e3a5ade65ffbb929cf)
Open commit detailssuccess
Oct 7, 2026
[4e076e8](https://github.com/vllm-project/production-stack/commit/4e076e8de3698a3729b575e3a5ade65ffbb929cf) · Oct 7, 2026
## History
[715 Commits](https://github.com/vllm-project/production-stack/commits/main/)
Open commit details
715 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [[Bugfix][CI] Fix k8s e2e engine OOM and raise test timeouts (](https://github.com/vllm-project/production-stack/commit/46e1f0585a68b181e45f09f4936e8f619c0e3f4d "\[Bugfix\]\[CI\] Fix k8s e2e engine OOM and raise test timeouts \(#1119\)

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>")[#1119](https://github.com/vllm-project/production-stack/pull/1119)[)](https://github.com/vllm-project/production-stack/commit/46e1f0585a68b181e45f09f4936e8f619c0e3f4d "\[Bugfix\]\[CI\] Fix k8s e2e engine OOM and raise test timeouts \(#1119\)

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>")  | Oct 5, 2026  |  
| [benchmarks/multi-round-qa](https://github.com/vllm-project/production-stack/tree/main/benchmarks/multi-round-qa "This path skips through empty directories")  | [benchmarks/multi-round-qa](https://github.com/vllm-project/production-stack/tree/main/benchmarks/multi-round-qa "This path skips through empty directories")  | [fix(benchmark/multi-round-qa): fix TTFT NoneType crash caused by reas…](https://github.com/vllm-project/production-stack/commit/e92d43fb3dff3372a2b67bc653e9ad6283d3afc4 "fix\(benchmark/multi-round-qa\): fix TTFT NoneType crash caused by reasoning models emitting reasoning_content instead of content \(#873\)

Signed-off-by: Kosseila \(CloudThrill\) <klouddude@gmail.com>")  | Mar 13, 2026  |  
|   |   |  [Update calendar link for community meetings (](https://github.com/vllm-project/production-stack/commit/da032f02bfee6694d94a18b79346ffa2181d8f24 "Update calendar link for community meetings \(#783\)

* Update calendar link for community meetings

Signed-off-by: Rui Zhang <51696593+zerofishnoodles@users.noreply.github.com>

* Update calendar link for community event

Signed-off-by: Rui Zhang <51696593+zerofishnoodles@users.noreply.github.com>

* Update calendar link for community meetings

Signed-off-by: Rui Zhang <51696593+zerofishnoodles@users.noreply.github.com>

---------

Signed-off-by: Rui Zhang <51696593+zerofishnoodles@users.noreply.github.com>")[#783](https://github.com/vllm-project/production-stack/pull/783)[)](https://github.com/vllm-project/production-stack/commit/da032f02bfee6694d94a18b79346ffa2181d8f24 "Update calendar link for community meetings \(#783\)

* Update calendar link for community meetings

Signed-off-by: Rui Zhang <51696593+zerofishnoodles@users.noreply.github.com>

* Update calendar link for community event

Signed-off-by: Rui Zhang <51696593+zerofishnoodles@users.noreply.github.com>

* Update calendar link for community meetings

Signed-off-by: Rui Zhang <51696593+zerofishnoodles@users.noreply.github.com>

---------

Signed-off-by: Rui Zhang <51696593+zerofishnoodles@users.noreply.github.com>")  | Jan 15, 2026  |  
|   |   |  [[bugfix] deprecate disable log request (](https://github.com/vllm-project/production-stack/commit/80e2f84ea09b1e1e82406ef22175f27f449db2af "\[bugfix\] deprecate disable log request \(#885\)

* bugfix: deprecate disable log request

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* Update helm/README.md

Co-authored-by: gemini-code-assist\[bot\] <176961590+gemini-code-assist\[bot\]@users.noreply.github.com>
Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>

---------

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>
Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>
Co-authored-by: gemini-code-assist\[bot\] <176961590+gemini-code-assist\[bot\]@users.noreply.github.com>")[#885](https://github.com/vllm-project/production-stack/pull/885)[)](https://github.com/vllm-project/production-stack/commit/80e2f84ea09b1e1e82406ef22175f27f449db2af "\[bugfix\] deprecate disable log request \(#885\)

* bugfix: deprecate disable log request

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* Update helm/README.md

Co-authored-by: gemini-code-assist\[bot\] <176961590+gemini-code-assist\[bot\]@users.noreply.github.com>
Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>

---------

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>
Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>
Co-authored-by: gemini-code-assist\[bot\] <176961590+gemini-code-assist\[bot\]@users.noreply.github.com>")  | Mar 12, 2026  |  
|   |   |  [[Bugfix] kv aware routing for lmcache 0.3.9 (](https://github.com/vllm-project/production-stack/commit/1d01a0cae733a6a02e33d01930816dbf10a37be7 "\[Bugfix\] kv aware routing for lmcache 0.3.9 \(#697\)

* \[bugfix\] fix kvaware routing

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* Modify CI to be compatible

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* Revert "fix dynamic config"

This reverts commit 9ac02f992e58fa102a891d3dfe903142bf913f0b.

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* modify CI

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* bugfix: fix bug for kvaware routing to be compatiable with lmcache 0.3.9

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* bugfix: fix ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* fix ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* update controller manager init

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* increase startup probe time

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify debug messaage

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

---------

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>
Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>")[#697](https://github.com/vllm-project/production-stack/pull/697)[)](https://github.com/vllm-project/production-stack/commit/1d01a0cae733a6a02e33d01930816dbf10a37be7 "\[Bugfix\] kv aware routing for lmcache 0.3.9 \(#697\)

* \[bugfix\] fix kvaware routing

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* Modify CI to be compatible

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* Revert "fix dynamic config"

This reverts commit 9ac02f992e58fa102a891d3dfe903142bf913f0b.

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* modify CI

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* bugfix: fix bug for kvaware routing to be compatiable with lmcache 0.3.9

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* bugfix: fix ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* fix ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify ci

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* update controller manager init

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* increase startup probe time

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

* modify debug messaage

Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>

---------

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>
Signed-off-by: Rui Zhang <rzhan229@ucsc.edu>")  | Nov 14, 2025  |  
| [[Doc][Bugfix] Pin KubeRay tutorial to a vLLM image that ships the ray…](https://github.com/vllm-project/production-stack/commit/1b87c11a24c144f6b63a64dbae4fc8c875059731 "\[Doc\]\[Bugfix\] Pin KubeRay tutorial to a vLLM image that ships the ray CLI \(#1056\)

Signed-off-by: Rishabh Sinha <rsinha17@terpmail.umd.edu>")  | Aug 29, 2026  |  
|   |   |  [[Feat] Implement OpenAI external provider (](https://github.com/vllm-project/production-stack/commit/cbab5434a63197bb577e8985d735e9333548a744 "\[Feat\] Implement OpenAI external provider \(#902\)

* feat: Implement openai provider

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>

* test: Update tests

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>

* test: Add unit test for external providers

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>

* fix: Increase timeout

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>

---------

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")[#902](https://github.com/vllm-project/production-stack/pull/902)[)](https://github.com/vllm-project/production-stack/commit/cbab5434a63197bb577e8985d735e9333548a744 "\[Feat\] Implement OpenAI external provider \(#902\)

* feat: Implement openai provider

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>

* test: Update tests

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>

* test: Add unit test for external providers

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>

* fix: Increase timeout

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>

---------

Signed-off-by: Shern Shiou Tan <shernshiou@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")  | Apr 20, 2026  |  
| [Fix: Use dashes instead of underscores for --gpu-memory-utilization f…](https://github.com/vllm-project/production-stack/commit/4e076e8de3698a3729b575e3a5ade65ffbb929cf "Fix: Use dashes instead of underscores for --gpu-memory-utilization flag \(#1124\)

Signed-off-by: antar-ghuman <antarghuman82@gmail.com>")  | Oct 7, 2026  |  
|   |   |  [feat(helm): refactor monitoring installation (](https://github.com/vllm-project/production-stack/commit/07b4d0698b982a5a30dc9efe84e8e85dc24c473c "feat\(helm\): refactor monitoring installation \(#860\)

Signed-off-by: enneitex <etienne.divet@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")[#860](https://github.com/vllm-project/production-stack/pull/860)[)](https://github.com/vllm-project/production-stack/commit/07b4d0698b982a5a30dc9efe84e8e85dc24c473c "feat\(helm\): refactor monitoring installation \(#860\)

Signed-off-by: enneitex <etienne.divet@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")  | Mar 19, 2026  |  
|   |   | [Fix: Use dashes instead of underscores for --gpu-memory-utilization f…](https://github.com/vllm-project/production-stack/commit/4e076e8de3698a3729b575e3a5ade65ffbb929cf "Fix: Use dashes instead of underscores for --gpu-memory-utilization flag \(#1124\)

Signed-off-by: antar-ghuman <antarghuman82@gmail.com>")  | Oct 7, 2026  |  
|   |   |   | Aug 18, 2026  |  
|   |   |  [[Feat] Use sidecar to download lora model for operator deployment (](https://github.com/vllm-project/production-stack/commit/4dddec83f443bdacaaf0607958e5381b434f4e2d "\[Feat\] Use sidecar to download lora model for operator deployment \(#622\)

* Use sidecar to download lora model for operator deployment

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* modify

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* modify

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

---------

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>")[#622](https://github.com/vllm-project/production-stack/pull/622)[)](https://github.com/vllm-project/production-stack/commit/4dddec83f443bdacaaf0607958e5381b434f4e2d "\[Feat\] Use sidecar to download lora model for operator deployment \(#622\)

* Use sidecar to download lora model for operator deployment

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* modify

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

* modify

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>

---------

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>")  | Jul 31, 2025  |  
| [[Bugfix][Router] loadaware: score bursts against live load, not the a…](https://github.com/vllm-project/production-stack/commit/c32a53713950002daed6d43a5de7ab49e5112765 "\[Bugfix\]\[Router\] loadaware: score bursts against live load, not the arrival snapshot \(#1108\)

Signed-off-by: Muhammad Ibrahim <ibrahimnd2000@gmail.com>")  | Oct 7, 2026  |  
|   |   |  [[Bugfix][CI] Fix k8s e2e engine OOM and raise test timeouts (](https://github.com/vllm-project/production-stack/commit/46e1f0585a68b181e45f09f4936e8f619c0e3f4d "\[Bugfix\]\[CI\] Fix k8s e2e engine OOM and raise test timeouts \(#1119\)

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>")[#1119](https://github.com/vllm-project/production-stack/pull/1119)[)](https://github.com/vllm-project/production-stack/commit/46e1f0585a68b181e45f09f4936e8f619c0e3f4d "\[Bugfix\]\[CI\] Fix k8s e2e engine OOM and raise test timeouts \(#1119\)

Signed-off-by: Rui Zhang <zrfishnoodles@gmail.com>")  | Oct 5, 2026  |  
|   |   | [[Doc][Bugfix] Pin KubeRay tutorial to a vLLM image that ships the ray…](https://github.com/vllm-project/production-stack/commit/1b87c11a24c144f6b63a64dbae4fc8c875059731 "\[Doc\]\[Bugfix\] Pin KubeRay tutorial to a vLLM image that ships the ray CLI \(#1056\)

Signed-off-by: Rishabh Sinha <rsinha17@terpmail.umd.edu>")  | Aug 29, 2026  |  
|   |   |  [fix(utils): make install-minikube-cluster.sh work on macOS (](https://github.com/vllm-project/production-stack/commit/5261497ed04f835fc54e596b1702631875d802bf "fix\(utils\): make install-minikube-cluster.sh work on macOS \(#970\)

* fix\(utils\): make install-minikube-cluster.sh work on macOS

Fixes #931.

The script previously assumed Linux throughout and failed on macOS in
three distinct ways:

1. **Minikube binary selection.** Unconditionally downloaded
   minikube-linux-amd64 even on Darwin. Also short-circuited on
   'minikube already on PATH' without checking the binary is executable,
   so a Linux ELF copied to /usr/local/bin/minikube would never get
   replaced. Now detects OS \(uname -s\) and arch \(uname -m\), downloads
   the matching minikube-{linux,darwin}-{amd64,arm64} release, and
   requires the existing binary to actually run before considering it
   installed.

2. **calculate_safe_memory.** Read /proc/meminfo, which doesn't exist
   on macOS. Now uses 'sysctl -n hw.memsize' on Darwin \(with ~60%
   available estimate, since Docker Desktop's VM budget has no clean
   equivalent to MemAvailable\) and keeps the existing /proc/meminfo
   path on Linux. cgroup-v2 detection stays Linux-only.

3. **systemctl restart docker / sudo sysctl fs.protected_regular=0.**
   Both Linux-only — systemctl isn't on macOS, and fs.protected_regular
   is a Linux sysctl. Now guarded on HOST_OS == linux, with a hint on
   macOS to restart Docker Desktop manually.

BPF section was already guarded on /proc/sys path existence, no change
needed there.

Signed-off-by: HumphreySun98 <humphreysun98@gmail.com>

* fix\(utils\): use curl -fLO so minikube download fails fast on HTTP errors

Without -f, curl saves the server's error page \(e.g. a 404/500 HTML body\)
to minikube-${HOST_OS}-${HOST_ARCH} and exits 0, so the script then tries
to install that HTML as the minikube binary. -f makes curl exit non-zero on
HTTP errors, letting set -e abort cleanly.

Addresses review feedback on #970.

Signed-off-by: HumphreySun98 <humphreysun98@gmail.com>

---------

Signed-off-by: HumphreySun98 <humphreysun98@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")[#970](https://github.com/vllm-project/production-stack/pull/970)[)](https://github.com/vllm-project/production-stack/commit/5261497ed04f835fc54e596b1702631875d802bf "fix\(utils\): make install-minikube-cluster.sh work on macOS \(#970\)

* fix\(utils\): make install-minikube-cluster.sh work on macOS

Fixes #931.

The script previously assumed Linux throughout and failed on macOS in
three distinct ways:

1. **Minikube binary selection.** Unconditionally downloaded
   minikube-linux-amd64 even on Darwin. Also short-circuited on
   'minikube already on PATH' without checking the binary is executable,
   so a Linux ELF copied to /usr/local/bin/minikube would never get
   replaced. Now detects OS \(uname -s\) and arch \(uname -m\), downloads
   the matching minikube-{linux,darwin}-{amd64,arm64} release, and
   requires the existing binary to actually run before considering it
   installed.

2. **calculate_safe_memory.** Read /proc/meminfo, which doesn't exist
   on macOS. Now uses 'sysctl -n hw.memsize' on Darwin \(with ~60%
   available estimate, since Docker Desktop's VM budget has no clean
   equivalent to MemAvailable\) and keeps the existing /proc/meminfo
   path on Linux. cgroup-v2 detection stays Linux-only.

3. **systemctl restart docker / sudo sysctl fs.protected_regular=0.**
   Both Linux-only — systemctl isn't on macOS, and fs.protected_regular
   is a Linux sysctl. Now guarded on HOST_OS == linux, with a hint on
   macOS to restart Docker Desktop manually.

BPF section was already guarded on /proc/sys path existence, no change
needed there.

Signed-off-by: HumphreySun98 <humphreysun98@gmail.com>

* fix\(utils\): use curl -fLO so minikube download fails fast on HTTP errors

Without -f, curl saves the server's error page \(e.g. a 404/500 HTML body\)
to minikube-${HOST_OS}-${HOST_ARCH} and exits 0, so the script then tries
to install that HTML as the minikube binary. -f makes curl exit non-zero on
HTTP errors, letting set -e abort cleanly.

Addresses review feedback on #970.

Signed-off-by: HumphreySun98 <humphreysun98@gmail.com>

---------

Signed-off-by: HumphreySun98 <humphreysun98@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")  | Jun 25, 2026  |  
|   |   |  [[Feat] Add initial CRD support for production stack (](https://github.com/vllm-project/production-stack/commit/5b7af2b1b3a05cc0936e6b240b40a6651928d165 "\[Feat\] Add initial CRD support for production stack \(#415\)

* add CRD support for production stack

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* move opertor to a secondary dir instead of in root dir

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* rename api group from serving.vllm.ai to production-stack.vllm.ai

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* enable lmcache cpu offloading

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* enable lmcache remote cache server offloading

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* fix service discorvery issue by adding readiness probe to vllm pod

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* fix readiness probe

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

---------

Signed-off-by: royyhuang <roy.y.huang@gmail.com>")[#415](https://github.com/vllm-project/production-stack/pull/415)[)](https://github.com/vllm-project/production-stack/commit/5b7af2b1b3a05cc0936e6b240b40a6651928d165 "\[Feat\] Add initial CRD support for production stack \(#415\)

* add CRD support for production stack

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* move opertor to a secondary dir instead of in root dir

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* rename api group from serving.vllm.ai to production-stack.vllm.ai

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* enable lmcache cpu offloading

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* enable lmcache remote cache server offloading

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* fix service discorvery issue by adding readiness probe to vllm pod

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

* fix readiness probe

Signed-off-by: royyhuang <roy.y.huang@gmail.com>

---------

Signed-off-by: royyhuang <roy.y.huang@gmail.com>")  | May 16, 2025  |  
|   |   |  [[CI/Build] Add .dockerignore to exclude test files from Docker builds (](https://github.com/vllm-project/production-stack/commit/421a1df4a51c89fab3e111ec1a63f5c9e533d871 "\[CI/Build\] Add .dockerignore to exclude test files from Docker builds \(#895\)

* \[CI/Build\] Add .dockerignore to exclude test files from Docker builds
Exclude src/tests/, src/examples/, and src/gateway_inference_extension/
from Docker build context to reduce image size.
Fixes #615

Signed-off-by: EzgiTastan <gezgit.tech@gmail.com>

* Address review: exclude .dockerignore and Dockerfiles

Signed-off-by: EzgiTastan <gezgit.tech@gmail.com>

---------

Signed-off-by: EzgiTastan <gezgit.tech@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")[…](https://github.com/vllm-project/production-stack/pull/895)  | Mar 31, 2026  |  
|   |   |  [feat(helm): refactor monitoring installation (](https://github.com/vllm-project/production-stack/commit/07b4d0698b982a5a30dc9efe84e8e85dc24c473c "feat\(helm\): refactor monitoring installation \(#860\)

Signed-off-by: enneitex <etienne.divet@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")[#860](https://github.com/vllm-project/production-stack/pull/860)[)](https://github.com/vllm-project/production-stack/commit/07b4d0698b982a5a30dc9efe84e8e85dc24c473c "feat\(helm\): refactor monitoring installation \(#860\)

Signed-off-by: enneitex <etienne.divet@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")  | Mar 19, 2026  |  
|   |   |  [Enable Dockerfile, Helm and shellcheck](https://github.com/vllm-project/production-stack/commit/0b5fae2dd5a579645645f0cb2e37f49c3c1768c7 "Enable Dockerfile, Helm and shellcheck `pre-commit` hooks \(#55\)

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>") [hooks (](https://github.com/vllm-project/production-stack/commit/0b5fae2dd5a579645645f0cb2e37f49c3c1768c7 "Enable Dockerfile, Helm and shellcheck `pre-commit` hooks \(#55\)

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>")[#55](https://github.com/vllm-project/production-stack/pull/55)[)](https://github.com/vllm-project/production-stack/commit/0b5fae2dd5a579645645f0cb2e37f49c3c1768c7 "Enable Dockerfile, Helm and shellcheck `pre-commit` hooks \(#55\)

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>")  | Feb 3, 2025  |  
|   |   |  [Add](https://github.com/vllm-project/production-stack/commit/3da6238ac92aa4f2ec13648f08a5e6ceded6b255 "Add `pre-commit` based linting and formatting \(#35\)

* Add pre-commit workflow

* Add actionlint

* Add generic hooks

* Add black, isort, shellcheck

* Add requirements and markdown linting

* Add toml

* Add Dockerfile

* Add codespell

* Use Node.js version of `markdownlint`

* Add `requirements-lint.txt`

* Use CLI version of Node.js `markdownlint`

* Add `pre-commit` instructions to `Contributing`

* `pre-commit run -a` automatic fixes

* Exclude helm templates from `check-yaml`

* Comment hooks that require installed tools

* Make `codespell` happy

* Make `actionlint` happy

* Disable `shellcheck` until it can be installed properly

* Make `markdownlint` happy

* Add note about running pre-commit

---------

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>") [based linting and formatting (](https://github.com/vllm-project/production-stack/commit/3da6238ac92aa4f2ec13648f08a5e6ceded6b255 "Add `pre-commit` based linting and formatting \(#35\)

* Add pre-commit workflow

* Add actionlint

* Add generic hooks

* Add black, isort, shellcheck

* Add requirements and markdown linting

* Add toml

* Add Dockerfile

* Add codespell

* Use Node.js version of `markdownlint`

* Add `requirements-lint.txt`

* Use CLI version of Node.js `markdownlint`

* Add `pre-commit` instructions to `Contributing`

* `pre-commit run -a` automatic fixes

* Exclude helm templates from `check-yaml`

* Comment hooks that require installed tools

* Make `codespell` happy

* Make `actionlint` happy

* Disable `shellcheck` until it can be installed properly

* Make `markdownlint` happy

* Add note about running pre-commit

---------

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>")[#35](https://github.com/vllm-project/production-stack/pull/35)[)](https://github.com/vllm-project/production-stack/commit/3da6238ac92aa4f2ec13648f08a5e6ceded6b255 "Add `pre-commit` based linting and formatting \(#35\)

* Add pre-commit workflow

* Add actionlint

* Add generic hooks

* Add black, isort, shellcheck

* Add requirements and markdown linting

* Add toml

* Add Dockerfile

* Add codespell

* Use Node.js version of `markdownlint`

* Add `requirements-lint.txt`

* Use CLI version of Node.js `markdownlint`

* Add `pre-commit` instructions to `Contributing`

* `pre-commit run -a` automatic fixes

* Exclude helm templates from `check-yaml`

* Comment hooks that require installed tools

* Make `codespell` happy

* Make `actionlint` happy

* Disable `shellcheck` until it can be installed properly

* Make `markdownlint` happy

* Add note about running pre-commit

---------

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>")  | Jan 29, 2025  |  
|   |   |  [[Feat] Migrate inference kgateway to agentgateway (](https://github.com/vllm-project/production-stack/commit/ab5bfe217a6786d6b3fb9f6e5595510948b3635b "\[Feat\] Migrate inference kgateway to agentgateway \(#1033\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")[#1033](https://github.com/vllm-project/production-stack/pull/1033)[)](https://github.com/vllm-project/production-stack/commit/ab5bfe217a6786d6b3fb9f6e5595510948b3635b "\[Feat\] Migrate inference kgateway to agentgateway \(#1033\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")  | Aug 10, 2026  |  
|   |   |  [Add](https://github.com/vllm-project/production-stack/commit/81e5e574fecdc903b597f6b156630e324a69f163 "Add `.readthedocs.yaml` \(#296\)

* Add `.readthedocs.yaml`

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* Delete unnecessary action

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

---------

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>") [(](https://github.com/vllm-project/production-stack/commit/81e5e574fecdc903b597f6b156630e324a69f163 "Add `.readthedocs.yaml` \(#296\)

* Add `.readthedocs.yaml`

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* Delete unnecessary action

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

---------

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>")[#296](https://github.com/vllm-project/production-stack/pull/296)[)](https://github.com/vllm-project/production-stack/commit/81e5e574fecdc903b597f6b156630e324a69f163 "Add `.readthedocs.yaml` \(#296\)

* Add `.readthedocs.yaml`

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

* Delete unnecessary action

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>

---------

Signed-off-by: Harry Mellor <19981378+hmellor@users.noreply.github.com>")  | Mar 17, 2025  |  
|   |   |   | Feb 17, 2025  |  
|   |   |  [Update contact information in README.md (](https://github.com/vllm-project/production-stack/commit/1e92773df603e4d5f9377ea1bd6b867316bd6e41 "Update contact information in README.md \(#821\)

* Update contact information in README.md

Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>

* Update reviewer mentions in contributing guide

Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>

* Update reviewer mentions in CONTRIBUTING.md

Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>

---------

Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")[#821](https://github.com/vllm-project/production-stack/pull/821)[)](https://github.com/vllm-project/production-stack/commit/1e92773df603e4d5f9377ea1bd6b867316bd6e41 "Update contact information in README.md \(#821\)

* Update contact information in README.md

Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>

* Update reviewer mentions in contributing guide

Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>

* Update reviewer mentions in CONTRIBUTING.md

Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>

---------

Signed-off-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")  | Feb 5, 2026  |  
|   |   |   | Jan 21, 2025  |  
|   |   |  [feat(helm): refactor monitoring installation (](https://github.com/vllm-project/production-stack/commit/07b4d0698b982a5a30dc9efe84e8e85dc24c473c "feat\(helm\): refactor monitoring installation \(#860\)

Signed-off-by: enneitex <etienne.divet@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")[#860](https://github.com/vllm-project/production-stack/pull/860)[)](https://github.com/vllm-project/production-stack/commit/07b4d0698b982a5a30dc9efe84e8e85dc24c473c "feat\(helm\): refactor monitoring installation \(#860\)

Signed-off-by: enneitex <etienne.divet@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")  | Mar 19, 2026  |  
|   |   |  [Add Artifact Hub metadata for verified publisher (](https://github.com/vllm-project/production-stack/commit/0182c75c930663a0e80d7deced88bc7e1590b44c "Add Artifact Hub metadata for verified publisher \(#540\)

Signed-off-by: Kobe Chen <xiaokunchen0@gmail.com>
Co-authored-by: Yuhan Liu <32589867+YuhanLiu11@users.noreply.github.com>")[#540](https://github.com/vllm-project/production-stack/pull/540)[)](https://github.com/vllm-project/production-stack/commit/0182c75c930663a0e80d7deced88bc7e1590b44c "Add Artifact Hub metadata for verified publisher \(#540\)

Signed-off-by: Kobe Chen <xiaokunchen0@gmail.com>
Co-authored-by: Yuhan Liu <32589867+YuhanLiu11@users.noreply.github.com>")  | Jun 30, 2025  |  
|   |   |  [[Bugfix][Router] soften fastapi pin to allow patched starlette (](https://github.com/vllm-project/production-stack/commit/ebbb8624010514c2d4fa7edf3de95a6440a755b2 "\[Bugfix\]\[Router\] soften fastapi pin to allow patched starlette \(#1088\)

Signed-off-by: Dundy Pasupuleti <dundysm@gmail.com>")[#1088](https://github.com/vllm-project/production-stack/pull/1088)[)](https://github.com/vllm-project/production-stack/commit/ebbb8624010514c2d4fa7edf3de95a6440a755b2 "\[Bugfix\]\[Router\] soften fastapi pin to allow patched starlette \(#1088\)

Signed-off-by: Dundy Pasupuleti <dundysm@gmail.com>")  | Sep 14, 2026  |  
|   |   | [[Bugfix] Align vllm version in requirements-test.txt with pyproject.t…](https://github.com/vllm-project/production-stack/commit/bc3abfaa59703393ba46febcfd17987e381145e5 "\[Bugfix\] Align vllm version in requirements-test.txt with pyproject.toml \(#1009\)

Signed-off-by: arijitroy003 <arijitroy003@gmail.com>
Co-authored-by: Rui Zhang <51696593+ruizhang0101@users.noreply.github.com>")  | Jul 28, 2026  |  
|   |   |  [[Feat][Router]: add extra support for YAML config file (](https://github.com/vllm-project/production-stack/commit/16e592eb1532dd1266056552dd6084e8b0cac8c5 "\[Feat\]\[Router\]: add extra support for YAML config file \(#621\)

* feat\(vllm-router\): add support for --dynamic-config-yaml option

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* feat\(vllm-router\): watch dynamic config YAML file

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* docs\(vllm-router\): update README with dynamic config YAML file

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* feat\(vllm-router\): support callbacks defined in config files

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* feat\(vllm-router\): support static_model_types defined in config files

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* feat\(discovery\): do not perform health checks on missing info

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* fix\(vllm-router\): fix and document static_aliases for dynamic config files

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* ci: fix ruff check job

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

---------

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>")[#621](https://github.com/vllm-project/production-stack/pull/621)[)](https://github.com/vllm-project/production-stack/commit/16e592eb1532dd1266056552dd6084e8b0cac8c5 "\[Feat\]\[Router\]: add extra support for YAML config file \(#621\)

* feat\(vllm-router\): add support for --dynamic-config-yaml option

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* feat\(vllm-router\): watch dynamic config YAML file

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* docs\(vllm-router\): update README with dynamic config YAML file

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* feat\(vllm-router\): support callbacks defined in config files

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* feat\(vllm-router\): support static_model_types defined in config files

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* feat\(discovery\): do not perform health checks on missing info

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* fix\(vllm-router\): fix and document static_aliases for dynamic config files

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

* ci: fix ruff check job

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>

---------

Signed-off-by: Antoine Auger <antoineauger@users.noreply.github.com>")  | Aug 8, 2025  |  
| View all files |  
## Repository files navigation
# vLLM Production Stack: reference stack for production vLLM deployment
| [**Blog**](https://lmcache.github.io) | [**Docs**](https://docs.vllm.ai/projects/production-stack) | [**Production-Stack Slack Channel**](https://communityinviter.com/apps/vllm-dev/join-vllm-developers-slack) | [**LMCache Slack**](https://join.slack.com/t/lmcacheworkspace/shared_invite/zt-2viziwhue-5Amprc9k5hcIdXT7XevTaQ) | [**Interest Form**](https://forms.gle/mQfQDUXbKfp2St1z7) |
## Latest News
  * 📄 [Official documentation](https://docs.vllm.ai/projects/production-stack) released for production-stack!
  * ✨ [Cloud Deployment Tutorials](https://github.com/vllm-project/production-stack/blob/main/tutorials) for Lambda Labs, AWS EKS, Google GCP are out!
  * 🛤️ 2026 roadmap is released! [Join the discussion now](https://github.com/vllm-project/production-stack/issues/855)!
  * 🔥 vLLM Production Stack is released! Check out our [release blogs](https://blog.lmcache.ai/2025-01-21-stack-release) posted on January 22, 2025.


## Community Events
We host **bi-weekly** community meetings at the following timeslot:
  * Every other Tuesdays at 5:30 PM PT – [Add to Calendar](https://drive.google.com/uc?export=download&id=1D4SqQiqzdSx_xsEwS0QTd592zd3Xourh)


All are welcome to join!
## Introduction
**vLLM Production Stack** project provides a reference implementation on how to build an inference stack on top of vLLM, which allows you to:
  * 🚀 Scale from a single vLLM instance to a distributed vLLM deployment without changing any application code
  * 💻 Monitor the metrics through a web dashboard
  * 😄 Enjoy the performance benefits brought by request routing and KV cache offloading


## Step-By-Step Tutorials
  1. How To [_Install Kubernetes (kubectl, helm, minikube, etc)_](https://github.com/vllm-project/production-stack/blob/main/tutorials/00-install-kubernetes-env.md)?
  2. How to [_Deploy Production Stack on Major Cloud Platforms (AWS, GCP, Lambda Labs, Azure)_](https://github.com/vllm-project/production-stack/blob/main/tutorials/cloud_deployments)?
  3. How To [_Set up a Minimal vLLM Production Stack_](https://github.com/vllm-project/production-stack/blob/main/tutorials/01-minimal-helm-installation.md)?
  4. How To [_Customize vLLM Configs (optional)_](https://github.com/vllm-project/production-stack/blob/main/tutorials/02-basic-vllm-config.md)?
  5. How to [_Load Your LLM Weights_](https://github.com/vllm-project/production-stack/blob/main/tutorials/03-load-model-from-pv.md)?
  6. How to [_Launch Different LLMs in vLLM Production Stack_](https://github.com/vllm-project/production-stack/blob/main/tutorials/04-launch-multiple-model.md)?
  7. How to [_Enable KV Cache Offloading with LMCache_](https://github.com/vllm-project/production-stack/blob/main/tutorials/05-offload-kv-cache.md)?


## Architecture
The stack is set up using [Helm](https://helm.sh/docs/), and contains the following key parts:
  * **Serving engine** : The vLLM engines that run different LLMs.
  * **Request router** : Directs requests to appropriate backends based on routing keys or session IDs to maximize KV cache reuse.
  * **Observability stack** : monitors the metrics of the backends through [Prometheus](https://github.com/prometheus/prometheus) + [Grafana](https://grafana.com/)


## Roadmap
We are actively working on this project and will release the following features soon. Please stay tuned!
  * **Autoscaling** based on vLLM-specific metrics
  * Support for **disaggregated prefill**
  * **Router improvements** (e.g., more performant router using non-python languages, KV-cache-aware routing algorithm, better fault tolerance, etc)


## Deploying the stack via Helm
### Prerequisites
  * A running Kubernetes (K8s) environment with GPUs 
    * Run `cd utils && bash install-minikube-cluster.sh`
    * Or follow our [tutorial](https://github.com/vllm-project/production-stack/blob/main/tutorials/00-install-kubernetes-env.md)


### Deployment
vLLM Production Stack can be deployed via helm charts. Clone the repo to local and execute the following commands for a minimal deployment:

```
git clone https://github.com/vllm-project/production-stack.git
cd production-stack/
helm repo add vllm https://vllm-project.github.io/production-stack
helm install vllm vllm/vllm-stack -f tutorials/assets/values-01-minimal-example.yaml
```

The deployed stack provides the same [**OpenAI API interface**](https://docs.vllm.ai/en/latest/serving/openai_compatible_server.html?ref=blog.mozilla.ai#openai-compatible-server) as vLLM, and can be accessed through kubernetes service.
To validate the installation and send a query to the stack, refer to [this tutorial](https://github.com/vllm-project/production-stack/blob/main/tutorials/01-minimal-helm-installation.md).
For more information about customizing the helm chart, please refer to [values.yaml](https://github.com/vllm-project/production-stack/blob/main/helm/values.yaml) and our other [tutorials](https://github.com/vllm-project/production-stack/tree/main/tutorials).
### Uninstall

```
helm uninstall vllm
```

## Grafana Dashboard
### Features
The Grafana dashboard provides the following insights:
  1. **Available vLLM Instances** : Displays the number of healthy instances.
  2. **Request Latency Distribution** : Visualizes end-to-end request latency.
  3. **Time-to-First-Token (TTFT) Distribution** : Monitors response times for token generation.
  4. **Number of Running Requests** : Tracks the number of active requests per instance.
  5. **Number of Pending Requests** : Tracks requests waiting to be processed.
  6. **GPU KV Usage Percent** : Monitors GPU KV cache usage.
  7. **GPU KV Cache Hit Rate** : Displays the hit rate for the GPU KV cache.


### Configuration
See the details in [`helm/README.md`](https://github.com/vllm-project/production-stack/blob/main/helm/README.md#Observability)
## Router
The router ensures efficient request distribution among backends. It supports:
  * Routing to endpoints that run different models
  * Exporting observability metrics for each serving engine instance, including QPS, time-to-first-token (TTFT), number of pending/running/finished requests, and uptime
  * Automatic service discovery and fault tolerance via the Kubernetes API
  * Model aliases
  * Multiple routing algorithms: 
    * Round-robin routing
    * Session-ID based routing
    * Prefix-aware routing (WIP)


Please refer to the [router documentation](https://github.com/vllm-project/production-stack/blob/main/src/vllm_router/README.md) for more details.
## Contributing
We welcome and value any contributions and collaborations. Please check out [CONTRIBUTING.md](https://github.com/vllm-project/production-stack/blob/main/CONTRIBUTING.md) for how to get involved.
## License
This project is licensed under Apache License 2.0. See the `LICENSE` file for details.
## Sponsors
We are grateful to our sponsors who support our development and benchmarking efforts:
For any issues or questions, feel free to open an issue or contact us ([@ruizhang0101](https://github.com/ruizhang0101), [@ApostaC](https://github.com/ApostaC), [@YuhanLiu11](https://github.com/YuhanLiu11), [@Shaoting-Feng](https://github.com/Shaoting-Feng)).
## About
vLLM’s reference system for K8S-native cluster-wide deployment with community-driven performance optimization
[docs.vllm.ai/projects/production-stack](https://docs.vllm.ai/projects/production-stack)
### Resources
### Code of conduct
### Contributing
### Stars
**2.7k** stars
### Watchers
**29** watching
### Forks
[ forks](https://github.com/vllm-project/production-stack/forks)
## 
[ vllm-stack-0.1.13Latest Sep 29, 2026 ](https://github.com/vllm-project/production-stack/releases/tag/vllm-stack-0.1.13)
## 
## Languages
  * [Python51.3%](https://github.com/vllm-project/production-stack/search?l=python)
  * [Shell17.4%](https://github.com/vllm-project/production-stack/search?l=shell)
  * [Go15.3%](https://github.com/vllm-project/production-stack/search?l=go)
  * [HCL11.5%](https://github.com/vllm-project/production-stack/search?l=hcl)
  * [Smarty2%](https://github.com/vllm-project/production-stack/search?l=smarty)
  * [Go Template1.4%](https://github.com/vllm-project/production-stack/search?l=go-template)
  * [Other1.1%](https://github.com/vllm-project/production-stack/search?l=Other)


You can’t perform that action at this time. 
