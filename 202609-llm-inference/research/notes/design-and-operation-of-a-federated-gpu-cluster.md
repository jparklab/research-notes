---
title: Design and Operation of a Federated GPU Cluster
id: design-and-operation-of-a-federated-gpu-cluster
tags:
- onprem-llm-inference-stacks-859099
- hpc-inference
- national-lab
created: '2026-10-23T17:22:05.046137Z'
updated: '2026-10-23T17:23:50.812487Z'
source: https://arxiv.org/pdf/2609.10552
source_domain: arxiv.org
fetched_at: '2026-10-23T17:22:05.045873Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'DHinfra.at (Univ. Graz/Krems; arXiv 2609.10552, Jul/Sep 2026, preprint):
  small federated GPU cluster for digital humanities, ~1.5 FTE. Hardware: Graz 8x
  H200 NVL, 4xH200+4xRTX PRO 6000, 4xL40S; Krems 6xH200 NVL; 7 more RTX PRO 6000 on
  order; EU tender; Ubuntu 24.04. All-open stack: Authentik (SAML/OIDC ACOnet), Slurm+SlurmDBD,
  Enroot/Pyxis, JupyterHub, custom FastAPI console/gateway with token budgets per
  project, llama-swap router, vLLM/llama.cpp run as Slurm jobs; two-tier serving (always-on
  vs on-demand swap). Docker Compose+systemd chosen over K8s. Weights via NFS/RDMA
  140GB cold start ~5s vs ~20s local NVMe. Apache-2.0 release.'
raw_file: raw/design-and-operation-of-a-federated-gpu-cluster.pdf
doi: arXiv:2609.10552
---

Design and Operation of a Federated GPU Cluster
for Digital Humanities within DHinfra.at
Florian Atzenhofer-Baumgartner∗a
David Fleischhacker a
Max Resch b
Lukas Waldhofer a
Michael Otto a
aDepartment of Digital Humanities, University of Graz, Austria
bCenter for Cultures and Technologies of Collecting, University for Continuing Education Krems,
Austria
September 11, 2026
Abstract
We describe the design, implementation, and operation of a small federated GPU cluster
built for Digital Humanities (DH) research within the Austrian DHinfra.at project. The
system spans two university sites, brokers logins from a national identity federation,
and exposes compute through three interfaces: interactive notebooks, SSH, and an
OpenAI-compatible inference API. The core of the platform is a control plane that
maps federated identity, per-project quotas, and a model catalogue onto self-service
interfaces. We document this control plane in detail, including the request-and-approval
queue that mediates every privileged cluster effect and the two-tier model-serving setup,
which combines always-on services with on-demand model swapping and lets researchers
consume large language models over HTTPS without holding a cluster account. We
report the constraints that shaped the build: a fixed power budget, EU-wide procurement,
a team of about 1.5 full-time equivalents, and the relationship between prioritized access
and utilization. We also record the main build decisions against the alternatives we
researched and did not adopt. The platform is released under the Apache License 2.0.
We close with a playbook for teams building a domain-specific GPU cluster on a limited
budget. This is a living technical report, versioned and updated as the platform evolves.
Keywords: GPU clusters, federated identity, LLM serving, MLOps, research infrastruc-
ture, Digital Humanities, interactive HPC
1
Introduction
GPU compute has become a routine research dependency well outside its original use in large-
scale numerical simulation. Handwritten text recognition, corpus-scale language modelling,
image and video classification, and the everyday use of large language models (LLMs) now
appear in projects whose authors do not identify as HPC users. These researchers want a
model behind a URL, a notebook with a GPU already attached, and a quota they can read
∗Corresponding author: atzenhofer@acm.org.
1
arXiv:2609.10552v1  [cs.DC]  7 Jul 2026


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
off a dashboard. HPC centres organised around batch submission, allocation proposals, and
queue scheduling serve these expectations only in part.
DHinfra.at, Digital Humanities Infrastructure Austria, is a 2023–2026 infrastructure
project funded by the Austrian Federal Ministry (BMBWF) through the EU Recovery and
Resilience Facility and led by the Department of Digital Humanities at the University of
Graz. It serves a consortium of Austrian institutions with close ties to CLARIAH-AT, the
Austrian node of CLARIN [3] and DARIAH. Its remit covers data-as-a-service, digitisation
hardware, and free/open-source software development. This report concerns its GPU/HPC
component: a federated cluster with primary compute in Graz and secondary compute and
storage in Krems. We presented the plans for this system at ASHPC24 [1]; this report
documents what was built.
This is the extended version of our ASHPC26 technical-track contribution. The talk
and abstract compress the material; here we give the architecture, the operational history,
and the reasoning behind specific decisions enough space to be reproduced. We address
two readerships: systems and MLOps practitioners interested in the mechanism, and
infrastructure builders in adjacent communities (FAIR data, Digital Humanities, GLAM)
who need to know what a GPU platform requires of them.
Contributions.
This report provides (1) a reproducible architecture for a small federated
GPU cluster that brokers logins from a national identity federation; (2) a detailed account of
the platform’s control plane, comprising a request/approval queue and an inference gateway;
(3) an account of operations, covering procurement, staffing, and the relationship between
prioritized access and utilization; (4) a record of the main build decisions against researched
alternatives (Section 5.8), and an Apache-licensed release of the platform; and (5) a playbook
(Section 8) for building a domain-specific GPU cluster for LLMs.
2
Background and Related Work
We position the system against three areas of research computing and the specific gap it
addresses.
Interactive and accessible HPC.
A long line of work lowers the barrier between
researchers and batch schedulers, from web portals that wrap job submission [4] to notebook
interfaces that spawn compute on demand [5]. The underlying resource manager is typically
Slurm [10], with isolation increasingly provided by rootless containers [6]. Our interactive
tier follows this line. We extend it by binding the interactive layer to a federated identity
broker and a per-project quota model.
AI/ML on shared and multi-tenant platforms.
The convergence of HPC and AI
has produced large multi-tenant platforms that serve many institutions from shared, often
Kubernetes-based fabric [9]. We reuse the same multi-tenancy mechanisms, namely scoped
credentials, per-tenant accounting, and heterogeneous hardware, at a smaller scale and with
an operational surface sized to our team and user base.
2


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
LLM serving and MLOps.
Efficient LLM inference is a developed systems area, with
paged-attention serving engines [7] and mature tooling for routing, batching, and accounting.
Our serving layer reuses these components. The work specific to our platform is the policy
layer around them: per-project token budgets, model access-control lists, and an OpenAI-
compatible gateway through which a researcher on another institution’s virtual machine can
consume a model without a cluster account.
HPC in the humanities.
Despite abundant data, the humanities have been slow to adopt
HPC, for reasons that are organisational and cultural as much as technical [8]: workloads are
often data-heavy rather than FLOP-heavy, expertise is thin, and the batch-allocation model
fits exploratory, iterative research poorly. The platform described here is built for these
users. Section 6 discusses the resulting emphasis on accessibility and its effect on utilization.
3
DHinfra.at: Context and Requirements
The project moved through four phases: working-group and consortium formation with
governance principles (2023); requirements engineering, training, and hardware/software
research in preparation for procurement (2024); FOSS investment, acquisition, and test
environments (2025); and integration with operations toward a ready-to-use service, with a
public launch targeted for mid-2026.
Three requirements distinguish this setting from a conventional HPC centre and recur
throughout the design.
1. Federated identity. Users belong to many institutions and authenticate with credentials
they already hold, through the national research network (ACOnet). There is no separate
account application.
2. Self-service configuration. Quotas, keys, and project setup are visible and adjustable
through a console. The operational default is prioritized access with monitoring.
3. Models served over HTTPS. Many users want an inference endpoint and no shell
access. The platform serves models over HTTPS to clients that never authenticate to
the cluster.
4
Hardware and Network
The hardware was procured through EU-wide public tendering, which delivered current-
generation accelerators at reasonable cost, comparable to national “AI Factory” deploy-
ments [2] and with upgrade pathways.
The hardware in Table 1 is heterogeneous by design.
The H200 NVL nodes carry
large-context and concurrent inference workloads and can be partitioned with MIG for many
small tenants; the L40S and RTX PRO 6000 (Blackwell) cards handle cheaper, single-tenant
exploratory and serving work, and the incoming RTX PRO 6000 expansion roughly triples
that pool. This keeps the expensive cards available for jobs that need them. It also adds
3


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
Site
Node
Accelerators
Storage / fabric
Graz (primary)
comp02
8× H200 NVL (141 GB, MIG)
ZFS over 400 Gbit/s
comp01
4× H200 NVL + 4× RTX PRO 6000 (96 GB)
NDR InfiniBand
infer01
4× L40S (48 GB)
(NFS/RDMA, NCCL)
Krems (secondary)
—
6× H200 NVL (141 GB)
Ceph; dedicated VMs
Table 1: GPU inventory across the two sites (current). A further 7× RTX PRO 6000
Blackwell are on order for Graz. H200 and RTX PRO 6000 are MIG-capable; L40S is not.
Both sites run Ubuntu 24.04 and are intended to converge on a common stack.
complexity to scheduling and model placement: a model that fits one card’s memory may
not fit another, and cost accounting reflects this (Section 5.4).
Several constraints shaped the deployment alongside the hardware specification: a fixed
power budget, the provisioning of 400 Gbit fabric and an ACOnet high-speed link between
the sites, and emergency-power signalling.
5
Software Stack
The stack is built entirely from open-source components. Most are standard tools; the code
we wrote is the integration layer that connects them. We describe the layers from the bottom
up, then the control plane in detail (Section 5.4).
5.1
Identity and access
Authentik1 is the identity authority. It brokers ACOnet logins over SAML and OpenID
Connect (OIDC), owns users, groups, and login flows, and exposes an LDAP outpost that
SSSD consumes to materialise POSIX accounts on cluster nodes. At the edge, Traefik
terminates TLS and applies Authentik forwardAuth as middleware, so authentication is
checked once, centrally, before traffic reaches any service.
Authentik is the sole owner of identity, and no other component duplicates it. Group
membership, leadership, and SSH public keys all live in Authentik; the rest of the platform
reads from it and keeps only a cache, reconciled two ways (Section 5.4). An institution’s
users appear and disappear in one place.
5.2
Scheduling and compute
Slurm is the resource authority for both user jobs and model-serving jobs, with SlurmDBD
for accounting (backed by MariaDB, which SlurmDBD supports natively, while the rest of the
platform uses Postgres), GPUs tracked as TRES, and pre- and post-job GPU health checks.
SlurmDBD is the authority for job history, which the control plane does not duplicate.
User containers use the standard rootless HPC split: Enroot2, a daemonless runtime
that runs entirely as the calling user, and Pyxis3, a Slurm SPANK plugin that adds
1https://goauthentik.io
2https://github.com/NVIDIA/enroot
3https://github.com/NVIDIA/pyxis
4


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
–container-image to srun/sbatch and drives Enroot from inside slurmstepd. On the
command line, containers are opt-in: a job that names no image runs on the host. JupyterHub
sessions, by contrast, always run inside a container selected at spawn time. Host execution for
CLI jobs is acceptable on a semi-trusted research cluster and consistent with the operations
stance in Section 6. Curated CUDA/PyTorch images live on NFS as squashfs files; users
layer their own pip packages on top and save the result. Proxmox VMs and LXC containers,
reached over a WireGuard VPN, cover cases that need stronger isolation or a long-lived
dedicated environment.
5.3
Interactive access
Researchers reach the cluster three ways: JupyterHub (with a SlurmSpawner, so a notebook
is a Slurm allocation), SSH over VPN, and the inference API (Section 5.5). The first two
are the standard interactive-HPC interfaces; the third serves users who do not work from a
terminal.
5.4
The control plane: console, gateway, and approval queue
The control plane is a FastAPI service we call the console. It maps identity, quotas, and
a model catalogue onto self-service interfaces. It presents two faces over one data model:
a public LLM API (bearer-key auth, proxied to the internal model router) and a session
console for members, group leaders, and admins. A dedicated Postgres database holds
budgets, models, usage, API keys, request queues, an audit log, and the identity cache; Redis
holds the auth cache, budget hot-counters, and a login rate-limit bucket. Background work is
minimal: budget windows are created lazily on first request, usage is written synchronously,
and the only scheduled jobs are an identity reconcile sweep and a project-expiry sweep, both
in-process. No Celery process or additional container is required.
Source of truth and the cache.
The platform assigns one authority per domain (Table 2).
Identity belongs to Authentik, job history to SlurmDBD, and workspace bytes to ZFS; the
console owns LLM budgets, the model catalogue, usage, and request intent. Identity is
mirrored into a local cache and kept current two ways: an Authentik webhook updates
the cache on user and group changes, and a periodic reconcile pulls each group with its
members, repairs drift, issues exactly one API key per (user, group) pair, and revokes
keys for dropped memberships. A user who disappears from Authentik is flagged missing
and retained; membership workflows then decline to act on the stale target.
Domain
Authority
Control-plane role
Users, groups, membership
Authentik
cache + API keys + console
Job history
SlurmDBD
not duplicated
Workspace bytes
ZFS / Ceph
not duplicated
LLM budgets, models, usage
console Postgres
full ownership
Table 2: One authority per domain. The control plane stores approval intent and derived
caches and does not become a second source of truth for identity or storage.
5


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
The approval queue.
Privileged cluster changes, such as adding a member, creating
a project, granting SSH access, changing a storage quota, or archiving a project, are not
applied by the web process that receives them.
The console records the request as
a queued row; a separate worker, pm-agent, polls an internal endpoint and applies the
approved effect on the cluster side (membership, slurm.*, home.*, ssh.grant/revoke,
project archive/restore/destroy). The split has three consequences. The public-facing service
holds no cluster credentials and performs no privileged side effects; every privileged action is
recorded as a row with an approver and a reason; and the cluster-side worker can be tested,
rate-limited, and reasoned about independently of the web tier.
Request authentication path.
For inference, the path is short and cache-friendly.
A client sends Authorization:
Bearer <key> to the public API host. Traefik applies
forwardAuth to an internal verify endpoint, which returns the resolved user, group, and key
(cached in Redis on the key’s HMAC for 60 s) or a 401. Traefik strips any client-supplied
identity headers, so the gateway never trusts identity from outside. The handler resolves
the model (an ACL check for private models), checks the group’s daily and weekly token
budgets, takes a per-model concurrency semaphore, and streams to the internal router.
Usage is recorded synchronously on completion. A budget exceeded mid-stream causes the
next request to be rejected; requests already streaming run to completion.
Budgets.
Every budget belongs to a group (a project), never an individual, in two windows:
daily (local midnight) and weekly (ISO week). The cap is derived at window rollover and
frozen for the window, so a user who joins late in a window receives no extra allowance
retroactively:
cap = member_count × default_per_user_per_window.
Per-group overrides (absolute or a multiplier, set per counter) are a routine workflow, logged
with the author and reason. Each model also carries input and output cost factors, so a
cheap small model can be priced below 1.0 to stretch the same budget; accounted tokens are
raw tokens times the factor. Cost factors let one budget cover models with very different
serving costs.
5.5
Inference services
Model serving is split into two tiers, and the gateway routes and meters requests without
running models itself.
• Always-on tier: a set of open-weight models stays resident, each holding its GPUs
through a long-lived Slurm allocation, behind the internal router, exposing OpenAI-
compatible chat, completion, and embedding endpoints with streaming, including multi-
modal (image) input. Footprints vary with model size and quantization: a quantized
small model serves from a single L40S, while the largest resident models occupy a full
eight-H200 node each. Partner-institution researchers call these endpoints with a scoped
key and need no cluster account; this is the most-used interface in practice.
6


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
• On-demand tier: models outside the resident set load on first request and unload
to free their GPUs when idle — the long tail of models too large or too rarely used
to keep resident, running across multiple H200s and optionally pinned to a node. We
validated on-demand swapping at a DHd2026 workshop with concurrent heavy users and
teacher/student model swapping on shared H200 nodes.
The router (llama-swap4) holds no GPUs; it is an HTTP front for engine lifecycle. Each
model’s launch command allocates GPUs as a Slurm job under a single trusted service
account and runs the engine — vLLM [7], or llama.cpp5 for one-slot models — in a container
on the assigned node (the salloc →srun →docker run chain, against the root Docker
daemon, separate from the unprivileged Enroot path used for user jobs). A GPU is therefore
held only while its model is loaded. Always-on versus on-demand is a per-model loading
policy: resident models are exempt from idle unloading, and Slurm, not the router, arbitrates
the GPUs between them. Tensor-parallel width, partition (L40S vs H200), and optional
node pinning are set per model; cold loads allow a long health-check window because a first
load means a weight download plus CUDA-graph compilation.
The gateway’s controls over this layer are deliberately small: a per-model concurrency
limit (a bounded semaphore — generous for batched vLLM, one or two for single-slot
llama.cpp), the public/private visibility and per-user ACL of each catalogue entry, and the
input/output cost factors used for budget accounting (Section 5.4). The roadmap integrates
both tiers under one controller with project management and asynchronous batch scheduling.
5.6
Monitoring and configuration
Prometheus and Grafana with DCGM exporters provide GPU and node observability. The
metrics surface inefficient resource use, which we raise with the relevant users. Platform
services run as Docker Compose plus systemd on the head node. We are also evaluating
NixOS for declarative node configuration, with a single description, atomic updates within a
release, and per-user environments; it is currently under test for the monitoring and identity
components, and both sites are intended to converge on the same stack.
5.7
Local testbed and reproducibility
A local testbed reproduces the identity, provisioning, and gateway chain in Docker Compose,
so the platform can be exercised from a cold start without cluster hardware. Dex6 stands
in for the institutional SSO, a mock model router replaces the GPU serving path, and the
provisioning worker runs against mock ZFS and Slurm backends. The mock/live boundary
is one switch per backend, and the component contract is identical on both sides. A test
harness brings the stack up, seeds projects, and runs end-to-end checks over the real HTTP
surfaces: identity reconcile, the approval queue, key issuance, and budget enforcement. The
result is a control plane that is testable in continuous integration, and a contributor can
validate a change before it reaches the cluster.
4https://github.com/mostlygeek/llama-swap
5https://github.com/ggml-org/llama.cpp
6https://github.com/dexidp/dex
7


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
5.8
Design decisions and rejected alternatives
Several components were chosen against a credible alternative that we researched and did
not adopt. Table 3 records the main ones with the deciding reason.
Concern
Chosen
Not adopted
Deciding reason
Service
orchestra-
tion
Docker
Compose
+
systemd
Kubernetes / K3s
About ten services on a hand-
ful of nodes; Compose is suf-
ficient and simpler. K3s is a
growth path past roughly fif-
teen services or per-user pod
isolation.
User containers
Enroot + Pyxis (root-
less, SPANK)
Docker for user jobs
Docker’s root daemon is equiv-
alent to root on the node; un-
safe for multi-tenant user con-
tainers.
Model-weight stor-
age
Served from NFS over
RDMA
Local NVMe cache
on compute nodes
Benchmarked: the head node’s
22-drive ZFS pool over Infini-
Band beats a compute node’s
two-drive local NVMe (140 GB
cold start ∼5 s vs ∼20 s).
Identity host
Head node or dedi-
cated VM
External VPS over a
tunnel
A tunnel outage longer than
the SSSD cache TTL stales all
identity and blocks new logins.
Slurm
accounting
DB
MariaDB
Postgres (used else-
where)
SlurmDBD supports MySQL/-
MariaDB natively; Postgres is
not a first-class backend.
LDAP
outpost
mode
Direct search
Cached
Cached mode delays member-
ship propagation by minutes
to hours.
Inference admission
Gateway does ACL,
budget, and metering
Generic
external
router
Keeps admission policy and ac-
counting in one service rather
than split across a router and
a sidecar.
Table 3: Main build decisions and the alternatives considered. Reasons are specific to this
deployment’s scale and team; a larger site might decide differently.
6
Operations
The operating model reflects a small team, roughly four to five core developers at about 1.5
full-time equivalents, and a community-oriented approach. The clusters run in dedicated
networks, effectively a DMZ, independent of central university HPC while using institutional
network, identity federation, and facilities. The stated priorities are simplicity, security, and
maintainability. Infrastructure-as-code is a goal that is partially realised.
Prioritized access and utilization.
The platform grants prioritized access and accepts
lower peak utilization as a result. Early operation showed the expected effect: prioritized
8


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
access exposes optimisation gaps in user code, and some resource use is inefficient. We
respond by monitoring usage and contacting the users involved.
Procurement and tendering.
EU-wide public procurement delivered state-of-the-art
hardware at reasonable cost, at the price of substantial process overhead. Offer quality
varied widely, one site’s tender required a second round, and the practical lessons were
straightforward: vet bidders, keep direct contact, and involve the legal department early.
Specifications and governance were re-negotiated after the proposal was accepted, and teams
should plan for this.
Concurrent development.
Much of the effort was concurrent: hardware and software
developed together, technical and policy work developed together, and the partner institutions
moved at different paces. For an infrastructure project with limited personnel funding, a
large share of the cost is staff training. Investment in staff and in detailed specifications
repaid itself: it built in-house knowledge, reduced downtime, and lowered dependence on
external experts for intricate problems.
7
Use Cases
A representative DH pipeline exercises every tier in turn. (1) In an exploratory phase the
researcher tests models interactively on JupyterHub, varying architectures, sizes, quantiza-
tions, and prompts, and benchmarks them on a domain task such as information extraction.
(2) They select a model on the evidence. (3) The model is deployed through the controller
as an API or a batch job; a project running on another institution’s VM then calls the
DHinfra inference endpoint over a whitelisted HTTPS route. (4) Secondary services, such
as project storage or custom VMs at Krems, support the work. (5) Results feed downstream
into research outputs.
The pilots cover a range of workloads: automatic text recognition, image and video
classification, VR applications, and adaptive AI tutoring, alongside DH projects on historical
job ads, digitised newspapers, archival registers, and diplomatics. The scheduling and serving
policies must accommodate this range.
8
A Playbook for a Domain-Specific GPU Cluster
This section distils the build into guidance for a team standing up a GPU cluster for LLMs
and DH/GLAM-style workloads on a modest budget. We organise it around complexity
tiers: start with the simplest robust option and add components when a workload requires
them.
1. Decide whether you deliver nodes or inference endpoints.
If your users are
HPC-native, give them Slurm directly. If they are not, as in the DH and GLAM case, the
primary deliverable is an inference endpoint with a quota, and the cluster sits behind it.
This choice determines the rest of the design.
9


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
2. Federate identity and keep a single authority.
Run one identity authority (we
used Authentik over SAML/OIDC against the national federation) and let other components
hold only a reconciled cache. Assign one authority per domain (Table 2); this is what keeps
a federated system debuggable.
3. Separate intent from effect.
Do not let a web request change cluster state directly.
Record the request, approve it, and have a separate worker that holds the credentials apply
it. This boundary provides auditability, least privilege, and a cluster-side surface that can
be tested on its own.
4. Meter consumption per project.
Attach quotas to projects, account in tokens
with per-model cost factors so that cheap models stretch a budget, and freeze caps for the
duration of each window. Treat overrides as a routine, logged workflow.
5. Serve models in two tiers.
Run an always-on tier (a batched serving engine such as
vLLM behind a router) for steady demand and an on-demand swapping tier for the long
tail. Keep the gateway separate from the GPU path: it routes and meters requests, and the
scheduler runs the serving engine.
6. Account for heterogeneous hardware.
Mixed GPUs, with large cards for concur-
rency and smaller cards for exploration, lower cost, provided the accounting and placement
track which card serves a given model.
7. Set an explicit utilization target.
A domain platform can accept lower utilization in
exchange for accessibility and fast iteration. State the chosen target explicitly and support
it with monitoring and direct user contact.
8.
Make the control plane testable without the cluster.
Mock the privileged
backends (identity, storage, scheduler) behind the same interface the live ones use, and run
the full request and approval chain in containers. A cold-start testbed lets you validate
control-plane changes in continuous integration before they reach the hardware.
9. Budget for staff and specifications.
EU-wide procurement can deliver excellent
hardware but requires time, legal support, and detailed specifications. The staff who write
a good tender are the staff who operate the cluster afterwards. Expect to re-negotiate
specifications and governance after funding is approved.
9
Open-source release
The platform is released under the Apache License 2.0 and will be public at the launch; the
cluster-side configuration and operator runbooks are published alongside it. Capable multi-
tenant GPU and LLM-serving stacks already exist, but equivalent functionality is commonly
available only under enterprise licensing or as a managed cloud product. A permissively
licensed, self-hostable stack aimed at small and mid-size domain-specific deployments fills a
10


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
gap in that space: another institution can run the same identity-federated, quota-metered
inference platform on its own hardware, with no per-seat or per-GPU licensing.
The
components the platform integrates (Slurm, Authentik, Enroot/Pyxis, vLLM, Traefik) are
themselves open-source, so the contributed part is the control plane and the integration that
binds them.
10
Open Questions and Outlook
Three questions remain open, and we bring them to the systems and research-computing
communities. The first concerns the relationship between federated identity and institutional
quotas. We keep a single identity authority and derive per-project budgets from it, but a
national federation spans institutions that may each want to set and account for quotas over
their own members. How should a platform reconcile fine-grained, per-institution accounting
with one authoritative identity source, without letting each institution become a second
place where membership and entitlements are defined? We do not have a clean answer, and
the tension grows as the user base widens across the consortium.
The second concerns caching on heterogeneous GPUs. The same model may be served
from cards with quite different memory profiles, so the working set a KV cache or a weight
cache can hold changes from one placement to the next. What is the right caching and
cache-admission strategy when a model’s effective capacity depends on the card it is placed
on, and how much of that placement should the scheduler be told about rather than leaving
it to each serving engine? We suspect the answer is largely platform-specific, but a more
general treatment would help.
The third concerns mixed scheduling.
Latency-sensitive inference and throughput-
oriented batch work compete for the same pool, and the two have opposite tolerances for
queueing delay. How can a scheduler interleave them on shared hardware so that interactive
requests stay responsive without leaving accelerators idle between batch jobs? Fair-share
accounting answers part of this, but the interaction between admission control at the gateway
and job scheduling in Slurm is not something we have resolved.
Our current work expands the user base to the full CLARIAH-AT consortium, deepens
Krems integration (distributed storage and coordinated, tracked GPU use), maintains a
CLI- and API-first interface with a GUI as a secondary layer, and targets a public launch
the second half of 2026. Two trends are converging: general HPC is broadening access, and
domain-specific systems are streamlining particular use cases. Small-scale, domain-specific
HPC complements large-scale general HPC.
11
Conclusion
Most of the engineering effort in this platform went into the control plane. The accelerators
and network fabric are standard hardware. The cluster becomes usable by researchers who
are not HPC users through the layer that federates their identity, meters consumption per
project, and serves models over HTTPS, with a separation between recording a request and
applying its privileged effect. We have described this layer in enough detail to reproduce,
and we have stated the operational trade-offs in accessibility, utilization, and staffing that a
11


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
small team has to decide explicitly. As a living document, this report will be updated as the
platform moves to public operation.
Acknowledgements
DHinfra is funded 2023–2026 by the Austrian Federal Ministry of Education, Science and
Research (BMBWF) under “(Digitale) Forschungsinfrastruktur”, in cooperation with the
EU Recovery and Resilience Facility (RRF), NextGenerationEU. We thank EuroCC, VSC,
and ASC for guidance; the ASHPC community for feedback at and since ASHPC; and the
DHinfra core team and the IT Services of the University of Graz and the University for
Continuing Education Krems. We acknowledge an ASHPC26 scholarship.
References
[1] Florian Atzenhofer-Baumgartner. HPC and DHinfra. In Austrian-Slovenian HPC
Meeting 2024 (ASHPC24), Grundlsee, Austria, 2024.
[2] European High Performance Computing Joint Undertaking (EuroHPC JU). AI factories.
https://www.eurohpc-ju.europa.eu/ai-factories_en, 2025. Accessed June 2026.
[3] Erhard Hinrichs and Steven Krauwer. The CLARIN research infrastructure: Resources
and tools for eHumanities scholars. In Proceedings of the Ninth International Conference
on Language Resources and Evaluation (LREC’14), pages 1525–1531, Reykjavik, Iceland,
2014. European Language Resources Association (ELRA). https://aclanthology.org/
L14-1356/.
[4] Dave Hudak, Doug Johnson, Alan Chalker, Jeremy Nicklas, Eric Franz, Trey Dockendorf,
and Brian L. McMichael. Open OnDemand: A web-based client portal for HPC centers.
Journal of Open Source Software, 3(25):622, 2018. https://doi.org/10.21105/joss.00622.
[5] Thomas Kluyver, Benjamin Ragan-Kelley, Fernando Pérez, Brian Granger, Matthias
Bussonnier, Jonathan Frederic, Kyle Kelley, Jessica Hamrick, Jason Grout, Sylvain Cor-
lay, Paul Ivanov, Damián Avila, Safia Abdalla, and Carol Willing. Jupyter notebooks—a
publishing format for reproducible computational workflows. In Positioning and Power
in Academic Publishing: Players, Agents and Agendas, pages 87–90. IOS Press, 2016.
https://doi.org/10.3233/978-1-61499-649-1-87.
[6] Gregory M. Kurtzer, Vanessa Sochat, and Michael W. Bauer. Singularity: Scientific
containers for mobility of compute. PLoS ONE, 12(5):e0177459, 2017. https://doi.org/
10.1371/journal.pone.0177459.
[7] Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng, Cody Hao Yu,
Joseph E. Gonzalez, Hao Zhang, and Ion Stoica. Efficient memory management for large
language model serving with PagedAttention. In Proceedings of the 29th Symposium
on Operating Systems Principles (SOSP ’23). ACM, 2023. https://doi.org/10.1145/
3600006.3613165.
12


---

Design and Operation of a Federated GPU Cluster for Digital Humanities
Preprint
[8] Melissa Terras, James Baker, James Hetherington, David Beavan, Martin Zaltz Austwick,
Anne Welsh, Helen O’Neill, Will Finley, Oliver Duke-Williams, and Adam Farquhar.
Enabling complex analysis of large-scale digital collections: humanities research, high-
performance computing, and transforming access to British Library digital collections.
Digital Scholarship in the Humanities, 33(2):456–466, 2018. https://doi.org/10.1093/
llc/fqx020.
[9] Derek Weitzel, Ashton Graves, Sam Albin, Huijun Zhu, Frank Würthwein, Mahidhar
Tatineni, Dmitry Mishin, John Graham, Elham E. Khoda, Mohammad Firas Sada,
Larry Smarr, and Thomas DeFanti. The national research platform: Stretched, multi-
tenant, scientific Kubernetes cluster. In Practice and Experience in Advanced Research
Computing (PEARC ’25). ACM, 2025. https://doi.org/10.1145/3708035.3736060.
[10] Andy B. Yoo, Morris A. Jette, and Mark Grondona. SLURM: Simple Linux utility for
resource management. In Job Scheduling Strategies for Parallel Processing (JSSPP),
volume 2862 of Lecture Notes in Computer Science, pages 44–60. Springer, 2003.
https://doi.org/10.1007/10968987_3.
13
