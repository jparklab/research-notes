---
title: 'GitHub - vllm-project/router: A high-performance and light-weight router for
  vLLM large scale deployment · GitHub'
id: github-vllm-projectrouter-a-high-performance-and-light-weight-router-for-vllm-la
tags:
- llmd-router-nonk8s-backends-54b774
- vllm-router
- service-discovery
created: '2026-09-11T20:47:52.737694Z'
updated: '2026-09-15T04:55:23.824130Z'
source: https://github.com/vllm-project/router
source_domain: github.com
fetched_at: '2026-09-11T20:47:52.715415Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Primary GitHub repo for vLLM Router (vllm-project/router, 401 stars, 159
  forks, fetched at HEAD Sept 2026), the code behind the Dec 2025 blog announcement.
  README''s basic/''Standard'' usage examples are entirely static-URL-based and Kubernetes-independent:
  ''--worker-urls http://worker1:8000 http://worker2:8000 --policy consistent_hash''
  for standard DP routing, and for P/D disaggregation ''--prefill http://127.0.0.1:8081
  --decode http://127.0.0.1:8083 ...'' with an explicit ''--intra-node-data-parallel-size''
  flag — no K8s API calls involved. Kubernetes Service Discovery (''--service-discovery
  --selector app=vllm-worker role=inference --service-discovery-namespace default'')
  is documented separately under ''Advanced Features,'' i.e. opt-in, not the default
  or required path. Documents three distinct backend-discovery mechanisms for P/D
  disaggregation depending on the vLLM KV connector: (1) NIXL connector — static prefill/decode
  URL lists passed as CLI flags; (2) NCCL connector — ''ZMQ based discovery is supported''
  via a ''--vllm-discovery-address'' endpoint; (3) Mooncake connector — the router
  itself ''queries each prefill node''s Mooncake bootstrap server at startup to learn
  engine_id per DP rank, and injects transfer_id / remote_bootstrap_addr / remote_engine_id
  into each request''s kv_transfer_params'' — a router-initiated pull-based discovery
  pattern with no Kubernetes or etcd involvement. Confirms the load-balancing policies
  (cache-aware, power-of-two, consistent hashing, random, round robin) are implemented
  independently of which discovery/backend-addressing mode is in use.'
---

*Suggested by [[vllm-router-a-high-performance-and-prefilldecode-aware-load-balancer-for-large-s]] — primary repo for the vLLM Router announced in the blog post; needed to verify actual K8s-independence and discovery code*

[Skip to content](https://github.com/vllm-project/router#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/router) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/router) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/router) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/router).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fvllm-project%2Frouter) You must be signed in to change notification settings
  * [ Fork 159 ](https://github.com/login?return_to=%2Fvllm-project%2Frouter)
  * [ Star  401 ](https://github.com/login?return_to=%2Fvllm-project%2Frouter)


[**31** Branches](https://github.com/vllm-project/router/branches)[](https://github.com/vllm-project/router/tags)
Go to file
Open more actions menu
## Latest commit
3 people
[[Bugfix]: strip](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db) [@rank](https://github.com/rank) [suffix in DP intra-node GET proxy URLs (](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db)[#221](https://github.com/vllm-project/router/pull/221)[)](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db)
Open commit detailsfailure
Sep 11, 2026
[83944c4](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db) · Sep 11, 2026
## History
[196 Commits](https://github.com/vllm-project/router/commits/main/)
Open commit details
196 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [[CI]: Fix CI failures from EOL bullseye base images (](https://github.com/vllm-project/router/commit/0519da5397f429af9e54d1e9601b0a135e338b65 "\[CI\]: Fix CI failures from EOL bullseye base images \(#246\)

* ci: switch RUST_IMAGE from bullseye to bookworm

Debian 11 \(bullseye\) LTS ended on 2026-08-31 and the bullseye-security
repository is no longer refreshed. Its InRelease metadata has expired,
so `apt-get update` fails inside rust:1.95.0-bullseye and the chained
`apt-get install python3-pip` never runs, breaking the Python wheel
build job \(see router build #773\). All other apt-based jobs only passed
by luck because the rust image already bundles their dependencies.

Move to rust:1.95.0-bookworm \(Debian 12\), which is actively supported.

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: move Dockerfile.router and release pipeline off bullseye

Debian 11 \(bullseye\) LTS ended on 2026-08-31 and bullseye-security is no
longer refreshed, so `apt-get update` fails inside bullseye-based images.
This would break the nightly Docker builds \(Dockerfile.router runs
apt-get in both stages\) and leaves the release smoke test on an EOL base.

- Dockerfile.router: rustlang/rust:nightly-bullseye -> nightly-bookworm,
  python:3.12-slim-bullseye -> python:3.12-slim-bookworm
- release-pipeline.yml: smoke-test the release binary on debian:bookworm
  \(manylinux_2_28-built binary, glibc 2.28, still well within bookworm's
  glibc 2.36\)

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: drop bullseye EOL comment from pipeline.yml

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: use a venv for pip on bookworm \(PEP 668\)

Debian 12 marks the system Python as externally managed, so pip3
installs into it are rejected with externally-managed-environment.
Create a venv \(python3-venv\) in the wheel-build and python-test jobs
and run pip/python/pytest inside it. Also add python3-dev explicitly
for building the PyO3 extension.

Per review: use apt-get instead of apt in Dockerfile.router scripting.

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: drop pip from the router runtime image

The Trivy scan of the built image flags pip's vendored msgpack 1.1.2
\(GHSA-6v7p-g79w-8964\) and setuptools 70.3.0 \(CVE-2025-47273,
CVE-2026-59890\). These live inside pip's own _vendor tree, so upgrading
or installing around them cannot clear the findings, and pip will not
bump the vendored setuptools \(pypa/pip#14031\).

The runtime image only needs 'requests' for the optional Python
launcher; the CMD runs the Rust binary directly. Install requests,
then uninstall pip entirely and remove ensurepip's bundled wheel \(which
carries the same vendored copies\). This also removes setuptools-rust,
wheel, and build from the image; they were build-time tools.

Signed-off-by: WU Hang <whlbx@hotmail.com>

---------

Signed-off-by: WU Hang <whlbx@hotmail.com>")[#246](https://github.com/vllm-project/router/pull/246)[)](https://github.com/vllm-project/router/commit/0519da5397f429af9e54d1e9601b0a135e338b65 "\[CI\]: Fix CI failures from EOL bullseye base images \(#246\)

* ci: switch RUST_IMAGE from bullseye to bookworm

Debian 11 \(bullseye\) LTS ended on 2026-08-31 and the bullseye-security
repository is no longer refreshed. Its InRelease metadata has expired,
so `apt-get update` fails inside rust:1.95.0-bullseye and the chained
`apt-get install python3-pip` never runs, breaking the Python wheel
build job \(see router build #773\). All other apt-based jobs only passed
by luck because the rust image already bundles their dependencies.

Move to rust:1.95.0-bookworm \(Debian 12\), which is actively supported.

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: move Dockerfile.router and release pipeline off bullseye

Debian 11 \(bullseye\) LTS ended on 2026-08-31 and bullseye-security is no
longer refreshed, so `apt-get update` fails inside bullseye-based images.
This would break the nightly Docker builds \(Dockerfile.router runs
apt-get in both stages\) and leaves the release smoke test on an EOL base.

- Dockerfile.router: rustlang/rust:nightly-bullseye -> nightly-bookworm,
  python:3.12-slim-bullseye -> python:3.12-slim-bookworm
- release-pipeline.yml: smoke-test the release binary on debian:bookworm
  \(manylinux_2_28-built binary, glibc 2.28, still well within bookworm's
  glibc 2.36\)

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: drop bullseye EOL comment from pipeline.yml

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: use a venv for pip on bookworm \(PEP 668\)

Debian 12 marks the system Python as externally managed, so pip3
installs into it are rejected with externally-managed-environment.
Create a venv \(python3-venv\) in the wheel-build and python-test jobs
and run pip/python/pytest inside it. Also add python3-dev explicitly
for building the PyO3 extension.

Per review: use apt-get instead of apt in Dockerfile.router scripting.

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: drop pip from the router runtime image

The Trivy scan of the built image flags pip's vendored msgpack 1.1.2
\(GHSA-6v7p-g79w-8964\) and setuptools 70.3.0 \(CVE-2025-47273,
CVE-2026-59890\). These live inside pip's own _vendor tree, so upgrading
or installing around them cannot clear the findings, and pip will not
bump the vendored setuptools \(pypa/pip#14031\).

The runtime image only needs 'requests' for the optional Python
launcher; the CMD runs the Rust binary directly. Install requests,
then uninstall pip entirely and remove ensurepip's bundled wheel \(which
carries the same vendored copies\). This also removes setuptools-rust,
wheel, and build from the image; they were build-time tools.

Signed-off-by: WU Hang <whlbx@hotmail.com>

---------

Signed-off-by: WU Hang <whlbx@hotmail.com>")  | Sep 8, 2026  |  
|   |   |   | Dec 5, 2025  |  
|   |   |  [test: add codeql-analysis github action (](https://github.com/vllm-project/router/commit/c88cba2b1a7ac5e5bbe0668d713841937a2c7574 "test: add codeql-analysis github action \(#124\)

Signed-off-by: andyzhangx <xiazhang@microsoft.com>

fix")[#124](https://github.com/vllm-project/router/pull/124)[)](https://github.com/vllm-project/router/commit/c88cba2b1a7ac5e5bbe0668d713841937a2c7574 "test: add codeql-analysis github action \(#124\)

Signed-off-by: andyzhangx <xiazhang@microsoft.com>

fix")  | Mar 20, 2026  |  
|   |   | [fix: preserve unknown fields in ChatCompletionRequest via serde flatt…](https://github.com/vllm-project/router/commit/e8992cafa76bd3e32b414f01aa88f4123e8a12ad "fix: preserve unknown fields in ChatCompletionRequest via serde flatten \(#162\)

Signed-off-by: SumanthRH <sumanthrh@anyscale.com>
Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>")  | Apr 30, 2026  |  
|  [docs: refresh WeChat group QR code (](https://github.com/vllm-project/router/commit/16e690aa73ba2eb052749bfb44e443bf367eb5da "docs: refresh WeChat group QR code \(#267\)

Signed-off-by: David Chen <530634352@qq.com>")[#267](https://github.com/vllm-project/router/pull/267)[)](https://github.com/vllm-project/router/commit/16e690aa73ba2eb052749bfb44e443bf367eb5da "docs: refresh WeChat group QR code \(#267\)

Signed-off-by: David Chen <530634352@qq.com>")  | Sep 11, 2026  |  
|   |   |  [Add rendezvous hashing (HRW) routing policy (](https://github.com/vllm-project/router/commit/6139a086563ee71d07f36c115291b0f59c814c9b "Add rendezvous hashing \(HRW\) routing policy \(#128\)

* test: add consistent hash balance simulation with multi-trial support

Add simulation example to evaluate consistent hash distribution across
decode workers with configurable vnodes, sessions, workers, DP size,
and header name. Supports aggregated stats \(CV, imbalance\) over multiple
trials. Also expose fbi_hash and VIRTUAL_NODES_PER_WORKER as public API
for simulation use.

Signed-off-by: Pengchao Wang <wpc@meta.com>

* feat: add rendezvous hashing \(HRW\) routing policy

Add a new "rendezvous_hash" load balancing policy that uses Highest
Random Weight \(HRW\) hashing. For each request, it scores every healthy
worker with fbi_hash\("{hash_key}:{worker_url}"\) and picks the highest
score. This is fully deterministic and sticky — same session always
maps to the same worker — with ~1/n redistribution on worker add/remove.

Compared to ring-based consistent hashing \(160 vnodes/rank\), rendezvous
hashing provides significantly better balance, especially as session
count grows. Simulation results \(10 workers, 4 DP ranks, 1000 trials\):

  Sessions/rank | Ring CV | Rendezvous CV | Improvement
  16            | 0.2546  | 0.2463        | -3%
  32            | 0.1873  | 0.1737        | -7%
  64            | 0.1421  | 0.1229        | -14%
  128           | 0.1135  | 0.0865        | -24%
  256           | 0.0946  | 0.0615        | -35%
  512           | 0.0841  | 0.0432        | -49%

The ring-based approach has a CV floor \(~0.08\) caused by uneven arc
distribution that more sessions cannot fix. Rendezvous hashing has zero
arc bias, so its CV approaches the theoretical minimum \(pure binomial
noise\). It also requires no tuning — no vnodes, no ring size to
configure.

Also updates the simulation example to support --mode ring|rendezvous
for benchmarking both approaches.

Signed-off-by: Pengchao Wang <wpc@meta.com>

* refactor: extract shared hash key extraction into policies/hash_key module

Move duplicated hash key extraction logic from ConsistentHashPolicy and
RendezvousHashPolicy into a shared hash_key module. Both policies now
call hash_key::extract_hash_key\(\) for the same extraction priority:
HTTP headers > body fields \(session_params.session_id, user, session_id,
user_id\) > request content fallback.

This also upgrades RendezvousHashPolicy to support nested
session_params.session_id extraction and single-quote JSON fields that
it was previously missing.

Adds 19 unit tests covering field extraction \(double/single/unquoted\),
nested fields, JSON object parsing, header priority, body priority,
fallback behavior, and edge cases.

Signed-off-by: Pengchao Wang <wpc@meta.com>

* style: apply cargo fmt to simulation example

Signed-off-by: Pengchao Wang <wpc@meta.com>

* fix: resolve clippy needless_range_loop warnings in simulation example

Signed-off-by: Pengchao Wang <wpc@meta.com>

* fix: add rendezvous_hash to CLI --policy value parser

The rendezvous_hash policy was added to PolicyConfig but was missing
from the CLI argument parser, causing the router to reject
--policy=rendezvous_hash with "invalid value". Add it to all three
value_parser lists \(--policy, --prefill-policy, --decode-policy\) and
the parse_policy\(\) match arm.

Signed-off-by: Pengchao Wang <wpc@meta.com>

* style: fix cargo fmt formatting in simulation example

Signed-off-by: Pengchao Wang <wpc@meta.com>

---------

Signed-off-by: Pengchao Wang <wpc@meta.com>
Co-authored-by: Pengchao Wang <wpc@meta.com>")[#128](https://github.com/vllm-project/router/pull/128)[)](https://github.com/vllm-project/router/commit/6139a086563ee71d07f36c115291b0f59c814c9b "Add rendezvous hashing \(HRW\) routing policy \(#128\)

* test: add consistent hash balance simulation with multi-trial support

Add simulation example to evaluate consistent hash distribution across
decode workers with configurable vnodes, sessions, workers, DP size,
and header name. Supports aggregated stats \(CV, imbalance\) over multiple
trials. Also expose fbi_hash and VIRTUAL_NODES_PER_WORKER as public API
for simulation use.

Signed-off-by: Pengchao Wang <wpc@meta.com>

* feat: add rendezvous hashing \(HRW\) routing policy

Add a new "rendezvous_hash" load balancing policy that uses Highest
Random Weight \(HRW\) hashing. For each request, it scores every healthy
worker with fbi_hash\("{hash_key}:{worker_url}"\) and picks the highest
score. This is fully deterministic and sticky — same session always
maps to the same worker — with ~1/n redistribution on worker add/remove.

Compared to ring-based consistent hashing \(160 vnodes/rank\), rendezvous
hashing provides significantly better balance, especially as session
count grows. Simulation results \(10 workers, 4 DP ranks, 1000 trials\):

  Sessions/rank | Ring CV | Rendezvous CV | Improvement
  16            | 0.2546  | 0.2463        | -3%
  32            | 0.1873  | 0.1737        | -7%
  64            | 0.1421  | 0.1229        | -14%
  128           | 0.1135  | 0.0865        | -24%
  256           | 0.0946  | 0.0615        | -35%
  512           | 0.0841  | 0.0432        | -49%

The ring-based approach has a CV floor \(~0.08\) caused by uneven arc
distribution that more sessions cannot fix. Rendezvous hashing has zero
arc bias, so its CV approaches the theoretical minimum \(pure binomial
noise\). It also requires no tuning — no vnodes, no ring size to
configure.

Also updates the simulation example to support --mode ring|rendezvous
for benchmarking both approaches.

Signed-off-by: Pengchao Wang <wpc@meta.com>

* refactor: extract shared hash key extraction into policies/hash_key module

Move duplicated hash key extraction logic from ConsistentHashPolicy and
RendezvousHashPolicy into a shared hash_key module. Both policies now
call hash_key::extract_hash_key\(\) for the same extraction priority:
HTTP headers > body fields \(session_params.session_id, user, session_id,
user_id\) > request content fallback.

This also upgrades RendezvousHashPolicy to support nested
session_params.session_id extraction and single-quote JSON fields that
it was previously missing.

Adds 19 unit tests covering field extraction \(double/single/unquoted\),
nested fields, JSON object parsing, header priority, body priority,
fallback behavior, and edge cases.

Signed-off-by: Pengchao Wang <wpc@meta.com>

* style: apply cargo fmt to simulation example

Signed-off-by: Pengchao Wang <wpc@meta.com>

* fix: resolve clippy needless_range_loop warnings in simulation example

Signed-off-by: Pengchao Wang <wpc@meta.com>

* fix: add rendezvous_hash to CLI --policy value parser

The rendezvous_hash policy was added to PolicyConfig but was missing
from the CLI argument parser, causing the router to reject
--policy=rendezvous_hash with "invalid value". Add it to all three
value_parser lists \(--policy, --prefill-policy, --decode-policy\) and
the parse_policy\(\) match arm.

Signed-off-by: Pengchao Wang <wpc@meta.com>

* style: fix cargo fmt formatting in simulation example

Signed-off-by: Pengchao Wang <wpc@meta.com>

---------

Signed-off-by: Pengchao Wang <wpc@meta.com>
Co-authored-by: Pengchao Wang <wpc@meta.com>")  | Apr 16, 2026  |  
| [py_src/vllm_router](https://github.com/vllm-project/router/tree/main/py_src/vllm_router "This path skips through empty directories")  | [py_src/vllm_router](https://github.com/vllm-project/router/tree/main/py_src/vllm_router "This path skips through empty directories")  | [chore: bump version to 0.1.15](https://github.com/vllm-project/router/commit/5188b44754084699d7195d7c2694170ba22bd05e "chore: bump version to 0.1.15

Signed-off-by: Bugen Zhao <i@bugenzhao.com>")  | Jul 13, 2026  |  
|   |   |   | Aug 10, 2026  |  
|   |   | [Merge remote-tracking branch 'upstream/main' into bz/xerothermic-bat](https://github.com/vllm-project/router/commit/a735d7baac0e086b55368f8fb9874005682fb74f "Merge remote-tracking branch 'upstream/main' into bz/xerothermic-bat

# Conflicts:
#	src/main.rs
#	src/routers/factory.rs")  | Apr 29, 2026  |  
|  [[Bugfix]: strip](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db "\[Bugfix\]: strip @rank suffix in DP intra-node GET proxy URLs \(#221\)

Signed-off-by: WU Hang <whlbx@hotmail.com>
Co-authored-by: herotai214 <herotai214@gmail.com>
Co-authored-by: Chenguang Zheng <645327136@qq.com>") [@rank](https://github.com/rank) [suffix in DP intra-node GET proxy URLs (](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db "\[Bugfix\]: strip @rank suffix in DP intra-node GET proxy URLs \(#221\)

Signed-off-by: WU Hang <whlbx@hotmail.com>
Co-authored-by: herotai214 <herotai214@gmail.com>
Co-authored-by: Chenguang Zheng <645327136@qq.com>")[#221](https://github.com/vllm-project/router/pull/221)[)](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db "\[Bugfix\]: strip @rank suffix in DP intra-node GET proxy URLs \(#221\)

Signed-off-by: WU Hang <whlbx@hotmail.com>
Co-authored-by: herotai214 <herotai214@gmail.com>
Co-authored-by: Chenguang Zheng <645327136@qq.com>")  | Sep 11, 2026  |  
|   |   |  [[Bugfix]: strip](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db "\[Bugfix\]: strip @rank suffix in DP intra-node GET proxy URLs \(#221\)

Signed-off-by: WU Hang <whlbx@hotmail.com>
Co-authored-by: herotai214 <herotai214@gmail.com>
Co-authored-by: Chenguang Zheng <645327136@qq.com>") [@rank](https://github.com/rank) [suffix in DP intra-node GET proxy URLs (](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db "\[Bugfix\]: strip @rank suffix in DP intra-node GET proxy URLs \(#221\)

Signed-off-by: WU Hang <whlbx@hotmail.com>
Co-authored-by: herotai214 <herotai214@gmail.com>
Co-authored-by: Chenguang Zheng <645327136@qq.com>")[#221](https://github.com/vllm-project/router/pull/221)[)](https://github.com/vllm-project/router/commit/83944c4145baf51045261a51bab83d1e3ad6a7db "\[Bugfix\]: strip @rank suffix in DP intra-node GET proxy URLs \(#221\)

Signed-off-by: WU Hang <whlbx@hotmail.com>
Co-authored-by: herotai214 <herotai214@gmail.com>
Co-authored-by: Chenguang Zheng <645327136@qq.com>")  | Sep 11, 2026  |  
|   |   |   | Dec 5, 2025  |  
|   |   |   | Mar 18, 2026  |  
| [.pre-commit-config.yaml](https://github.com/vllm-project/router/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/vllm-project/router/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |   | Dec 5, 2025  |  
|   |   | [chore: bump version to 0.1.15](https://github.com/vllm-project/router/commit/5188b44754084699d7195d7c2694170ba22bd05e "chore: bump version to 0.1.15

Signed-off-by: Bugen Zhao <i@bugenzhao.com>")  | Jul 13, 2026  |  
|   |   | [chore: bump version to 0.1.15](https://github.com/vllm-project/router/commit/5188b44754084699d7195d7c2694170ba22bd05e "chore: bump version to 0.1.15

Signed-off-by: Bugen Zhao <i@bugenzhao.com>")  | Jul 13, 2026  |  
|   |   |  [[CI]: Fix CI failures from EOL bullseye base images (](https://github.com/vllm-project/router/commit/0519da5397f429af9e54d1e9601b0a135e338b65 "\[CI\]: Fix CI failures from EOL bullseye base images \(#246\)

* ci: switch RUST_IMAGE from bullseye to bookworm

Debian 11 \(bullseye\) LTS ended on 2026-08-31 and the bullseye-security
repository is no longer refreshed. Its InRelease metadata has expired,
so `apt-get update` fails inside rust:1.95.0-bullseye and the chained
`apt-get install python3-pip` never runs, breaking the Python wheel
build job \(see router build #773\). All other apt-based jobs only passed
by luck because the rust image already bundles their dependencies.

Move to rust:1.95.0-bookworm \(Debian 12\), which is actively supported.

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: move Dockerfile.router and release pipeline off bullseye

Debian 11 \(bullseye\) LTS ended on 2026-08-31 and bullseye-security is no
longer refreshed, so `apt-get update` fails inside bullseye-based images.
This would break the nightly Docker builds \(Dockerfile.router runs
apt-get in both stages\) and leaves the release smoke test on an EOL base.

- Dockerfile.router: rustlang/rust:nightly-bullseye -> nightly-bookworm,
  python:3.12-slim-bullseye -> python:3.12-slim-bookworm
- release-pipeline.yml: smoke-test the release binary on debian:bookworm
  \(manylinux_2_28-built binary, glibc 2.28, still well within bookworm's
  glibc 2.36\)

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: drop bullseye EOL comment from pipeline.yml

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: use a venv for pip on bookworm \(PEP 668\)

Debian 12 marks the system Python as externally managed, so pip3
installs into it are rejected with externally-managed-environment.
Create a venv \(python3-venv\) in the wheel-build and python-test jobs
and run pip/python/pytest inside it. Also add python3-dev explicitly
for building the PyO3 extension.

Per review: use apt-get instead of apt in Dockerfile.router scripting.

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: drop pip from the router runtime image

The Trivy scan of the built image flags pip's vendored msgpack 1.1.2
\(GHSA-6v7p-g79w-8964\) and setuptools 70.3.0 \(CVE-2025-47273,
CVE-2026-59890\). These live inside pip's own _vendor tree, so upgrading
or installing around them cannot clear the findings, and pip will not
bump the vendored setuptools \(pypa/pip#14031\).

The runtime image only needs 'requests' for the optional Python
launcher; the CMD runs the Rust binary directly. Install requests,
then uninstall pip entirely and remove ensurepip's bundled wheel \(which
carries the same vendored copies\). This also removes setuptools-rust,
wheel, and build from the image; they were build-time tools.

Signed-off-by: WU Hang <whlbx@hotmail.com>

---------

Signed-off-by: WU Hang <whlbx@hotmail.com>")[#246](https://github.com/vllm-project/router/pull/246)[)](https://github.com/vllm-project/router/commit/0519da5397f429af9e54d1e9601b0a135e338b65 "\[CI\]: Fix CI failures from EOL bullseye base images \(#246\)

* ci: switch RUST_IMAGE from bullseye to bookworm

Debian 11 \(bullseye\) LTS ended on 2026-08-31 and the bullseye-security
repository is no longer refreshed. Its InRelease metadata has expired,
so `apt-get update` fails inside rust:1.95.0-bullseye and the chained
`apt-get install python3-pip` never runs, breaking the Python wheel
build job \(see router build #773\). All other apt-based jobs only passed
by luck because the rust image already bundles their dependencies.

Move to rust:1.95.0-bookworm \(Debian 12\), which is actively supported.

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: move Dockerfile.router and release pipeline off bullseye

Debian 11 \(bullseye\) LTS ended on 2026-08-31 and bullseye-security is no
longer refreshed, so `apt-get update` fails inside bullseye-based images.
This would break the nightly Docker builds \(Dockerfile.router runs
apt-get in both stages\) and leaves the release smoke test on an EOL base.

- Dockerfile.router: rustlang/rust:nightly-bullseye -> nightly-bookworm,
  python:3.12-slim-bullseye -> python:3.12-slim-bookworm
- release-pipeline.yml: smoke-test the release binary on debian:bookworm
  \(manylinux_2_28-built binary, glibc 2.28, still well within bookworm's
  glibc 2.36\)

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: drop bullseye EOL comment from pipeline.yml

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: use a venv for pip on bookworm \(PEP 668\)

Debian 12 marks the system Python as externally managed, so pip3
installs into it are rejected with externally-managed-environment.
Create a venv \(python3-venv\) in the wheel-build and python-test jobs
and run pip/python/pytest inside it. Also add python3-dev explicitly
for building the PyO3 extension.

Per review: use apt-get instead of apt in Dockerfile.router scripting.

Signed-off-by: WU Hang <whlbx@hotmail.com>

* ci: drop pip from the router runtime image

The Trivy scan of the built image flags pip's vendored msgpack 1.1.2
\(GHSA-6v7p-g79w-8964\) and setuptools 70.3.0 \(CVE-2025-47273,
CVE-2026-59890\). These live inside pip's own _vendor tree, so upgrading
or installing around them cannot clear the findings, and pip will not
bump the vendored setuptools \(pypa/pip#14031\).

The runtime image only needs 'requests' for the optional Python
launcher; the CMD runs the Rust binary directly. Install requests,
then uninstall pip entirely and remove ensurepip's bundled wheel \(which
carries the same vendored copies\). This also removes setuptools-rust,
wheel, and build from the image; they were build-time tools.

Signed-off-by: WU Hang <whlbx@hotmail.com>

---------

Signed-off-by: WU Hang <whlbx@hotmail.com>")  | Sep 8, 2026  |  
|   |   |   | Dec 5, 2025  |  
|   |   | [Remove unused gRPC router paths](https://github.com/vllm-project/router/commit/0b9d2ec81c3260b01414c9e2efa309b34b419c4b "Remove unused gRPC router paths

Signed-off-by: Bugen Zhao <i@bugenzhao.com>")  | Apr 28, 2026  |  
|   |   |   | Dec 5, 2025  |  
|   |   |  [docs: add community links and WeChat QR code (](https://github.com/vllm-project/router/commit/ab7e4537654caf8a2096bbe7c65e138754c72d6c "docs: add community links and WeChat QR code \(#245\)

Signed-off-by: David Chen <530634352@qq.com>")[#245](https://github.com/vllm-project/router/pull/245)[)](https://github.com/vllm-project/router/commit/ab7e4537654caf8a2096bbe7c65e138754c72d6c "docs: add community links and WeChat QR code \(#245\)

Signed-off-by: David Chen <530634352@qq.com>")  | Sep 8, 2026  |  
|   |   | [ci(lint): pin ruff ruleset to ruff's historical default](https://github.com/vllm-project/router/commit/ab82bd9f8e748f6394b0d812575166d22ccf48fb "ci\(lint\): pin ruff ruleset to ruff's historical default

`.buildkite/pipeline.yml` runs `pip install black ruff` \(unpinned\) then
`ruff check py_src/ py_test/`, and the repo has no `\[tool.ruff\]` config —
so the lint uses whatever ruleset the latest ruff defaults to. Recent
ruff releases expanded the default ruleset \(FA100, BLE001, S110, C408,
...\), which reddened py_src/py_test repo-wide \(174 errors\) with no code
change, failing CI on unrelated PRs.

Pin the lint selection to ruff's historical default \(E4/E7/E9 + F\) so the
check is deterministic across ruff versions. `ruff check` and
`black --check` both pass. A broader ruleset + cleanup can follow.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_013ccyYZCbycaMRgaJj2kE9z")  | Jul 23, 2026  |  
|   |   |   | Dec 5, 2025  |  
|   |   |   | Dec 5, 2025  |  
|   |   |   | Dec 5, 2025  |  
| View all files |  
## Repository files navigation
# vLLM Router
| [**Documentation**](https://github.com/vllm-project/router/blob/main/docs/load_balancing/README.md) | [**DeepWiki**](https://deepwiki.com/vllm-project/router) | [**User Forum**](https://discuss.vllm.ai) | [**Developer Slack**](https://vllm-dev.slack.com/archives/C085AUU43NK) | [**WeChat**](https://github.com/vllm-project/router/blob/main/docs/assets/WeChat.png) | 
A high-performance and light-weight request forwarding system for vLLM large scale deployments, providing advanced load balancing methods and prefill/decode disaggregation support.
### Key Features
  * **Core Architecture** : Request routing framework and async processing patterns
  * **Load Balancing** : Multiple algorithms (cache-aware, power of two, consistent hashing, random, round robin)
  * **Prefill-Decode Disaggregation** : Specialized routing for separated processing phases
  * **Service Discovery** : Kubernetes-native worker management and health monitoring
  * **Enterprise Features** : Circuit breakers, retry logic, metrics collection


## Quick Start
### Prerequisites
**Rust and Cargo:**

```
# Install rustup (Rust installer and version manager)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Follow the installation prompts, then reload your shell
source $HOME/.cargo/env

# Verify installation
rustc --version
cargo --version

```

**Python with pip installed**
### Installation & Basic Usage
#### Rust Binary

```
# Build Rust components
cargo build --release
```

#### Python Package
Install from PyPI

```
pip install vllm-router                                                                                                                                                        

To build from source:
bash    
pip install setuptools-rust wheel build
python -m build
pip install dist/*.whl

# Rebuild & reinstall in one step during development
python -m build  pip install --force-reinstall dist/*.whl
```

### Usage Examples
#### Standard Data Parallelism Routing

```
# Launch router with data parallelism (8 replicas per worker URL)
# When data-parallel-size > 1, the router automatically creates DP-aware workers
./target/release/vllm-router \
    --worker-urls http://worker1:8000 http://worker2:8000 \
    --policy consistent_hash \
    --intra-node-data-parallel-size 8

# Alternative: using cargo run
cargo run --release -- \
    --worker-urls http://worker1:8000 http://worker2:8000 \
    --policy consistent_hash \
    --intra-node-data-parallel-size 8

# Alternative: using python launcher
vllm-router \
  --worker-urls http://worker1:8000 http://worker2:8000 \
    --policy consistent_hash \
    --intra-node-data-parallel-size 8
```

#### Prefill-Decode Disaggregation

```
# When vLLM runs the NIXL connector, prefill/decode URLs are required.
# See a working example in scripts/llama3.1/ folder.
cargo run --release -- \
    --policy consistent_hash \
    --vllm-pd-disaggregation \
    --prefill http://127.0.0.1:8081 \
    --prefill http://127.0.0.1:8082 \
    --decode http://127.0.0.1:8083 \
    --decode http://127.0.0.1:8084 \
    --decode http://127.0.0.1:8085 \
    --decode http://127.0.0.1:8086 \
    --host 127.0.0.1 \
    --port 8090 \
    --intra-node-data-parallel-size 1 \


# When vLLM runs the NCCL connector, ZMQ based discovery is supported.
# See a working example in scripts/install.sh
cargo run --release -- \
    --policy consistent_hash \
    --vllm-pd-disaggregation \
    --vllm-discovery-address 0.0.0.0:30001 \
    --host 0.0.0.0 \
    --port 10001 \
    --prefill-policy consistent_hash \
    --decode-policy consistent_hash

# When vLLM runs the Mooncake connector, pass --kv-connector mooncake.
# The router queries each prefill node's Mooncake bootstrap server at startup
# to learn engine_id per DP rank, and injects transfer_id / remote_bootstrap_addr /
# remote_engine_id into each request's kv_transfer_params for P/D coordination.
cargo run --release -- \
    --policy consistent_hash \
    --vllm-pd-disaggregation \
    --kv-connector mooncake \
    --prefill http://127.0.0.1:8081 \
    --prefill http://127.0.0.1:8082 \
    --decode http://127.0.0.1:8083 \
    --decode http://127.0.0.1:8084 \
    --host 127.0.0.1 \
    --port 8090 \
    --intra-node-data-parallel-size 1
```

## Configuration
### Authentication
Enable bearer-token validation by listing validation URLs (comma-separated) in `.env` via `API_KEY_VALIDATION_URLS` or passing `--api-key-validation-urls`. When set, all HTTP endpoints require `Authorization: Bearer <token>` and tokens are validated with HTTP 200 responses.

```
# .env
API_KEY_VALIDATION_URLS=https://codebase.helmholtz.cloud/api/v4/user

# CLI override
vllm-router --api-key-validation-urls https://codebase.helmholtz.cloud/api/v4/user
```

### Metrics
Prometheus metrics endpoint available at `127.0.0.1:29000` by default.

```
# Custom metrics configuration
vllm-router \
    --worker-urls http://localhost:8080 http://localhost:8081 \
    --prometheus-host 0.0.0.0 \
    --prometheus-port 9000
```

### Retries and Circuit Breakers
#### Retry Configuration
Retries are enabled by default with exponential backoff and jitter:

```
vllm-router \
  --worker-urls http://localhost:8080 http://localhost:8081 \
  --retry-max-retries 3 \
  --retry-initial-backoff-ms 100 \
  --retry-max-backoff-ms 10000 \
  --retry-backoff-multiplier 2.0 \
  --retry-jitter-factor 0.1
```

#### Circuit Breaker Configuration
Circuit breakers protect workers and provide automatic recovery:

```
vllm-router \
  --worker-urls http://localhost:8080 http://localhost:8081 \
  --cb-failure-threshold 5 \
  --cb-success-threshold 2 \
  --cb-timeout-duration-secs 30 \
  --cb-window-duration-secs 60
```

**Circuit Breaker State Machine:**
  * `Closed` → `Open` after N consecutive failures (failure-threshold)
  * `Open` → `HalfOpen` after timeout (timeout-duration-secs)
  * `HalfOpen` → `Closed` after M consecutive successes (success-threshold)


**Retry Policy:** Retries on HTTP status codes 408/429/500/502/503/504, with backoff/jitter between attempts.
### Request ID Tracking
Track requests across distributed systems with configurable headers:

```
# Use custom request ID headers
vllm-router \
    --worker-urls http://localhost:8080 \
    --request-id-headers x-trace-id x-request-id
```

**Default headers:** `x-request-id`, `x-correlation-id`, `x-trace-id`, `request-id`
### Load Balancing Policies
The router supports multiple load balancing policies:  
| Policy  | Description  | Session Affinity  | Use Case  |  
| --- | --- | --- | --- |  
| `round_robin`  | Sequential distribution across workers  | No  | General purpose, even distribution  |  
| `random`  | Uniform random selection  | No  | Simple deployments  |  
| `consistent_hash`  | Routes same session/user to same worker  | Yes  | Multi-turn chat, KV cache reuse  |  
| `power_of_two`  | Picks least loaded of two random workers  | No  | Load-sensitive workloads  |  
| `cache_aware`  | Optimizes for prefix cache hits  | Yes  | Repeated prompts, few-shot  |  

```
# Example: Using consistent_hash with HTTP header for session affinity
curl -X POST http://router:8000/v1/chat/completions \
  -H "X-Session-ID: my-session-123" \
  -H "Content-Type: application/json" \
  -d '{"model": "llama-3", "messages": [{"role": "user", "content": "Hello!"}]}'
```

For detailed configuration options, hash key priorities, and usage examples, see [Load Balancing Documentation](https://github.com/vllm-project/router/blob/main/docs/load_balancing/README.md).
## Advanced Features
### Kubernetes Service Discovery
Automatic worker discovery and management in Kubernetes environments.
#### Basic Service Discovery

```
vllm-router \
    --service-discovery \
    --selector app=vllm-worker role=inference \
    --service-discovery-namespace default
```

### Command Line Arguments Reference
#### Service Discovery
  * `--service-discovery`: Enable Kubernetes service discovery
  * `--service-discovery-port`: Port for worker URLs (default: 8000)
  * `--service-discovery-namespace`: Kubernetes namespace to watch
  * `--selector`: Label selectors for regular mode (format: `key1=value1 key2=value2`)


## Development
### Troubleshooting
**VSCode Rust Analyzer Issues:** Set `rust-analyzer.linkedProjects` to the absolute path of `Cargo.toml`:

```
{
  "rust-analyzer.linkedProjects": ["/workspaces/vllm/vllm-router/Cargo.toml"]
}
```

### CI/CD Pipeline
The continuous integration pipeline includes comprehensive testing, benchmarking, and publishing:
#### Build & Test
  1. **Build Wheels** : Uses `cibuildwheel` for manylinux x86_64 packages
  2. **Build Source Distribution** : Creates source distribution for pip fallback
  3. **Rust HTTP Server Benchmarking** : Performance testing of router overhead
  4. **Basic Inference Testing** : End-to-end validation through the router
  5. **PD Disaggregation Testing** : Benchmark and sanity checks for prefill-decode load balancing


#### Publishing
  * **PyPI Publishing** : Wheels and source distributions published when version changes in `pyproject.toml`
  * **Container Images** : Docker images published using `/docker/Dockerfile.router`


## Acknowledgement
This project is a fork of [SGLang Model Gateway](https://github.com/sgl-project/sglang/tree/main/sgl-model-gateway), and we would like to explicitly acknowledge and thank the original authors for their work. At this stage, our fork includes only minimal changes to preserve the existing interface and ensure compatibility with vLLM. We anticipate further divergence as we pursue the roadmap we have in mind, which is the reason for creating the fork.
## About
A high-performance and light-weight router for vLLM large scale deployment
### Resources
### Stars
**401** stars
### Watchers
**6** watching
### Forks
[ forks](https://github.com/vllm-project/router/forks)
## 
[7tags](https://github.com/vllm-project/router/tags)
## 
## Languages
  * [Rust75.4%](https://github.com/vllm-project/router/search?l=rust)
  * [Python14.1%](https://github.com/vllm-project/router/search?l=python)
  * [Shell9.6%](https://github.com/vllm-project/router/search?l=shell)
  * [Other0.9%](https://github.com/vllm-project/router/search?l=Other)


You can’t perform that action at this time. 
