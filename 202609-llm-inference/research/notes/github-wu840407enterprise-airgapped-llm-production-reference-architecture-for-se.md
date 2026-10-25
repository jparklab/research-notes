---
title: 'GitHub - wu840407/enterprise-airgapped-llm: Production reference architecture
  for self-hosted LLM in air-gapped enterprise environments. Dell R740 + Turing GPUs
  + vLLM + Qwen3-Coder + AD/LDAPS. · GitHub'
id: github-wu840407enterprise-airgapped-llm-production-reference-architecture-for-se
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- practitioner-repo
- reliability-medium
created: '2026-10-23T17:23:34.628910Z'
updated: '2026-10-23T17:25:08.883847Z'
source: https://github.com/wu840407/enterprise-airgapped-llm
source_domain: github.com
fetched_at: '2026-10-23T17:23:34.606507Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: GitHub reference build by individual (wu840407, 14 commits, last Oct 20 2026;
  reliability medium, single-author, self-reported field-tested). Dell R740 + 2x Quadro
  RTX 6000 Turing, vLLM serving Qwen3-Coder-30B-A3B AWQ, Open WebUI, Postgres, Redis,
  Nginx, AD/LDAPS; mdadm RAID10; scripts split online-phase download vs offline deploy;
  claims 80-120 tok/s single user.
---

*Suggested by [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]] — cited as primary source*

[Skip to content](https://github.com/wu840407/enterprise-airgapped-llm#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/wu840407/enterprise-airgapped-llm) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/wu840407/enterprise-airgapped-llm) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/wu840407/enterprise-airgapped-llm) to refresh your session. Dismiss alert
[**2** Branches](https://github.com/wu840407/enterprise-airgapped-llm/branches)[](https://github.com/wu840407/enterprise-airgapped-llm/tags)
Go to file
Open more actions menu
## Latest commit
[Describe the GPUs by model and generation](https://github.com/wu840407/enterprise-airgapped-llm/commit/8f1e523fa5289bdd0cec1d7a87d899ec66196953)
Oct 20, 2026
[8f1e523](https://github.com/wu840407/enterprise-airgapped-llm/commit/8f1e523fa5289bdd0cec1d7a87d899ec66196953) · Oct 20, 2026
## History
[14 Commits](https://github.com/wu840407/enterprise-airgapped-llm/commits/main/)
Open commit details
14 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [benchmarks: move to dedicated repo llm-inference-benchmark](https://github.com/wu840407/enterprise-airgapped-llm/commit/94cf9a51309667049d1c77383cf45a25dd433fbc "benchmarks: move to dedicated repo llm-inference-benchmark

The benchmarking work outgrew this repository — it now covers three engine
configurations, run-to-run variance, and a reproducible harness. Keeping a
pointer here instead of a duplicate.")  | Aug 20, 2026  |  
| [Replace real network addresses with documentation placeholders](https://github.com/wu840407/enterprise-airgapped-llm/commit/f835765eb98681d9247d5f3b3f85432e9f7da46d "Replace real network addresses with documentation placeholders")  | Aug 12, 2026  |  
|   |   | [Add deployment scripts and architecture documentation](https://github.com/wu840407/enterprise-airgapped-llm/commit/3f51ba26400607b7df0201778e8788d02028c458 "Add deployment scripts and architecture documentation

Scripts:
- 01-system-prep.sh: base system + toolchain
- 02-raid-setup.sh: mdadm RAID 10 + LVM provisioning
- 03-nvidia-docker.sh: NVIDIA driver + Docker + Container Toolkit
- 04-download-models.sh: Hugging Face model download \(online phase\)
- 05-deploy.sh: TLS cert + secrets + compose up

Docs:
- architecture.md: Mermaid diagrams for component, storage, inference flow
- airgap-deployment.md: full online-to-offline transition runbook")  | Apr 25, 2026  |  
|   |   | [Initial commit: enterprise air-gapped LLM reference architecture](https://github.com/wu840407/enterprise-airgapped-llm/commit/22bbaf9d7091e3bd60584a43fd4075255acd745f "Initial commit: enterprise air-gapped LLM reference architecture

- Dell R740 + 2x Quadro RTX 6000 \(Turing, sm_75\) reference deployment
- vLLM serving Qwen3-Coder-30B-A3B-Instruct \(AWQ, MoE 30B/3B-active\)
- Open WebUI + PostgreSQL 16 + Redis 7 + Nginx
- mdadm RAID 10 + LVM data tier on HBA330 \(6x 750GB SAS SSD\)
- Hardware RAID 1 boot tier on Dell BOSS-S1 \(2x M.2 SATA\)
- Streaming-aware Nginx reverse proxy with rate limiting
- Air-gap deployment discipline with online-to-offline transition protocol")  | Apr 25, 2026  |  
|   |   | [Initial commit: enterprise air-gapped LLM reference architecture](https://github.com/wu840407/enterprise-airgapped-llm/commit/22bbaf9d7091e3bd60584a43fd4075255acd745f "Initial commit: enterprise air-gapped LLM reference architecture

- Dell R740 + 2x Quadro RTX 6000 \(Turing, sm_75\) reference deployment
- vLLM serving Qwen3-Coder-30B-A3B-Instruct \(AWQ, MoE 30B/3B-active\)
- Open WebUI + PostgreSQL 16 + Redis 7 + Nginx
- mdadm RAID 10 + LVM data tier on HBA330 \(6x 750GB SAS SSD\)
- Hardware RAID 1 boot tier on Dell BOSS-S1 \(2x M.2 SATA\)
- Streaming-aware Nginx reverse proxy with rate limiting
- Air-gap deployment discipline with online-to-offline transition protocol")  | Apr 25, 2026  |  
|   |   | [Initial commit: enterprise air-gapped LLM reference architecture](https://github.com/wu840407/enterprise-airgapped-llm/commit/22bbaf9d7091e3bd60584a43fd4075255acd745f "Initial commit: enterprise air-gapped LLM reference architecture

- Dell R740 + 2x Quadro RTX 6000 \(Turing, sm_75\) reference deployment
- vLLM serving Qwen3-Coder-30B-A3B-Instruct \(AWQ, MoE 30B/3B-active\)
- Open WebUI + PostgreSQL 16 + Redis 7 + Nginx
- mdadm RAID 10 + LVM data tier on HBA330 \(6x 750GB SAS SSD\)
- Hardware RAID 1 boot tier on Dell BOSS-S1 \(2x M.2 SATA\)
- Streaming-aware Nginx reverse proxy with rate limiting
- Air-gap deployment discipline with online-to-offline transition protocol")  | Apr 25, 2026  |  
|   |   | [Describe the GPUs by model and generation](https://github.com/wu840407/enterprise-airgapped-llm/commit/8f1e523fa5289bdd0cec1d7a87d899ec66196953 "Describe the GPUs by model and generation")  | Oct 20, 2026  |  
|   |   | [Initial commit: enterprise air-gapped LLM reference architecture](https://github.com/wu840407/enterprise-airgapped-llm/commit/22bbaf9d7091e3bd60584a43fd4075255acd745f "Initial commit: enterprise air-gapped LLM reference architecture

- Dell R740 + 2x Quadro RTX 6000 \(Turing, sm_75\) reference deployment
- vLLM serving Qwen3-Coder-30B-A3B-Instruct \(AWQ, MoE 30B/3B-active\)
- Open WebUI + PostgreSQL 16 + Redis 7 + Nginx
- mdadm RAID 10 + LVM data tier on HBA330 \(6x 750GB SAS SSD\)
- Hardware RAID 1 boot tier on Dell BOSS-S1 \(2x M.2 SATA\)
- Streaming-aware Nginx reverse proxy with rate limiting
- Air-gap deployment discipline with online-to-offline transition protocol")  | Apr 25, 2026  |  
|   |   | [Initial commit: enterprise air-gapped LLM reference architecture](https://github.com/wu840407/enterprise-airgapped-llm/commit/22bbaf9d7091e3bd60584a43fd4075255acd745f "Initial commit: enterprise air-gapped LLM reference architecture

- Dell R740 + 2x Quadro RTX 6000 \(Turing, sm_75\) reference deployment
- vLLM serving Qwen3-Coder-30B-A3B-Instruct \(AWQ, MoE 30B/3B-active\)
- Open WebUI + PostgreSQL 16 + Redis 7 + Nginx
- mdadm RAID 10 + LVM data tier on HBA330 \(6x 750GB SAS SSD\)
- Hardware RAID 1 boot tier on Dell BOSS-S1 \(2x M.2 SATA\)
- Streaming-aware Nginx reverse proxy with rate limiting
- Air-gap deployment discipline with online-to-offline transition protocol")  | Apr 25, 2026  |  
| View all files |  
## Repository files navigation
# Enterprise Air-Gapped LLM Stack
**TL;DR** — 30B MoE coding LLM · 2× Quadro RTX 6000 (Turing) · 100% offline · 80–120 tok/s · AD/LDAPS auth · field-tested, not a tutorial.
> **Production-grade self-hosted Large Language Model platform engineered for air-gapped, regulated, and security-critical environments.** Built on existing enterprise hardware (Dell R740 + dual Turing-class GPUs) with no cloud dependencies, no telemetry, and no external API calls. Delivers a state-of-the-art coding assistant comparable to mid-tier proprietary services — entirely on-premises.
## Why This Project Exists
Most "self-hosted LLM" tutorials assume Ampere or Hopper-class GPUs, public internet access, and a permissive security posture. **None of those assumptions hold in regulated enterprise environments** — defense, finance, healthcare, semiconductor manufacturing — where:
  * The network is **physically isolated** (air-gapped) from the public internet
  * Data sovereignty laws prohibit any model inference traffic leaving the perimeter
  * The available hardware is whatever the procurement cycle provided 3–5 years ago
  * Authentication must integrate with existing **Active Directory** infrastructure
  * Every binary, every container image, and every model weight must enter through a controlled diode-style transfer process


This repository is the reference architecture for that scenario. It is the result of an end-to-end production deployment, not a thought experiment.
## System Architecture
Loading

```
flowchart TB
    B["🖥️ Internal users<br/>(AD group-restricted)"]
    AD[("Active Directory<br/>LDAPS :636")]
    subgraph Host["Dell R740 · Ubuntu 24.04 · air-gapped VLAN"]
        N["Nginx<br/>TLS 1.2/1.3 · rate-limit · token streaming"]
        subgraph Net["Docker internal network — no egress"]
            W["Open WebUI<br/>multi-user · RBAC"]
            V["vLLM<br/>Qwen3-Coder-30B-A3B (AWQ)<br/>tensor parallel ×2 · fp16 · FlashInfer"]
            P[("PostgreSQL 16<br/>history · audit")]
            R[("Redis 7<br/>sessions")]
        end
        subgraph HW["Hardware"]
            G["2× Quadro RTX 6000<br/>Turing sm_75 · 24 GB each"]
            S["RAID10 + LVM · 2.25 TB<br/>~2.5 GB/s seq read"]
        end
    end
    B -->|HTTPS| N --> W
    W <--> V
    W --> P
    W --> R
    W -.->|memberOf filter| AD
    V === G
    V --- S

```

## Architectural Highlights
### 1. Turing-Era GPU Constraint Engineering
The deployment target is **2× NVIDIA Quadro RTX 6000 Passive (Turing, sm_75, 24 GB each)** — a generation with no BF16 or FP8 support and no FlashAttention-2.
Most modern LLM stacks silently assume Ampere+ features. We don't have that luxury. Concrete decisions forced by the hardware:  
| Constraint  | Decision  | Trade-off Resolved  |  
| --- | --- | --- |  
| No BF16 support  | Force `--dtype float16`  | Compatible with Turing ALUs; must validate AWQ kernels don't underflow  |  
| No FP8 quantization  | Use AWQ 4-bit (`compressed-tensors`)  | Native Turing INT4 throughput; ~4× memory reduction  |  
| Flash Attention 2 unstable  | Auto-fallback to **FlashInfer + Marlin** kernels  | vLLM detects sm_75 and routes correctly  |  
| CUDA Graphs partially broken  | `--enforce-eager`  | Trades minor latency for stability — the right call in production  |  
| 48 GB total VRAM ceiling  | Tensor parallel across both GPUs  | Fits 30B-class model with 16K context  |  
**Net result:** A 30B-parameter coding model loads in **~31 seconds** and serves at production latency on hardware that's nominally 4 generations behind the curve.
### 2. Tensor Parallelism on a Constrained Budget
vLLM's tensor parallelism requires `tensor_parallel_size` to evenly divide the model's attention head count. With 48 GB total VRAM, the only viable strategies were:
  * **Dense 32B FP16:** 64 GB needed → infeasible
  * **Dense 32B INT4:** ~19 GB → fits on a single GPU, but throughput-limited
  * **MoE 30B-A3B INT4:** ~17 GB weights, 3 B activated parameters per token → **chosen**


The MoE choice (Qwen3-Coder-30B-A3B-Instruct) is the architectural inflection point. It activates only 10% of parameters per forward pass while preserving the quality of a 30B dense model — a pattern that maps unusually well to bandwidth-limited Turing memory subsystems and yields **decoder throughput that exceeds dense 14B models** on identical hardware.
### 3. Quantization Format Conflict Resolution
A non-obvious pitfall encountered during deployment: the model's `config.json` declared `compressed-tensors` quantization (the modern unified format from `llm-compressor`), but vLLM's CLI parser still accepts the legacy `--quantization awq` flag. Specifying both causes a **silent precedence collision** that surfaces as a Pydantic `ValidationError` deep inside `ModelConfig`.
**Resolution:** Omit `--quantization` entirely. Modern vLLM (0.10+) auto-detects the quantization scheme from `quant_method` in the config. Specifying it manually is now an **anti-pattern** that older tutorials still propagate.
This kind of issue is exactly why air-gapped deployments need defensive engineering — there is no `pip install --upgrade` rescue path when the compose stack fails at 02:00.
### 4. Storage Subsystem: Tiered RAID + LVM

```
┌─ Boot tier ────────────────────────────┐
│ Dell BOSS-S1 (hardware RAID 1)         │
│ 2× M.2 SATA SSD (240 GB)               │
│ → /, /boot, /boot/efi, swap            │
└────────────────────────────────────────┘
┌─ Data tier ────────────────────────────┐
│ HBA330 (IT mode) + mdadm RAID 10       │
│ 6× 750 GB SAS SSD → /dev/md0 (2.25 TB) │
│ → LVM volume group vg_data             │
│   ├─ lv_models   600 GB → /opt/ai/models│
│   ├─ lv_owui     400 GB → /opt/ai/data │
│   ├─ lv_docker   300 GB → /var/lib/docker│
│   └─ lv_backup   800 GB → /opt/ai/backup│
└────────────────────────────────────────┘

```

**Why this layout:**
  * **Hardware RAID 1 for OS** — boot survives any single SSD failure with zero OS intervention; iDRAC handles monitoring out-of-band.
  * **Software RAID 10 for data** — mdadm is more recoverable than vendor RAID firmware in a 5–10 year horizon. With pure SSDs and a hot spare strategy, RAID 10 wins on rebuild time and write amplification versus RAID 5/6.
  * **Separate`/var/lib/docker` LV** — prevents image bloat from filling the OS partition. Docker daemon root must be on the data tier _before_ Docker is installed; reordering this step is a common rollback trigger.
  * **LVM on top of mdadm** — enables online volume expansion without downtime as model collections grow. Snapshots provide pre-upgrade rollback.


Measured throughput: **~2.5 GB/s sequential read** across the RAID — sufficient for ~31 second cold-start of 17 GB model weights.
### 5. Air-Gap Deployment Discipline
The deployment follows a strict **online → offline transition** protocol:
  1. **Online phase** — full apt/Docker/HuggingFace fetches via temporary connectivity
  2. **Snapshot phase** — `docker save` every image, `pip download` every wheel, `tar` every config
  3. **Migration phase** — assets transferred via approved removable media, hash-verified
  4. **Offline phase** — network interface migrated to isolated VLAN; NTP/DNS pivoted to internal services; any outbound traffic is a misconfiguration alarm


Backup discipline matters: there is no AWS S3 versioning to fall back on. PostgreSQL `pg_dump` runs nightly; mdadm scrub runs weekly; SMART health is checked every 24 hours. Every administrative procedure is reversible without internet access.
### 6. Authentication & Network Boundary
  * **Reverse proxy** — Nginx terminates TLS 1.2/1.3 with internal-CA-signed certificates (3650-day validity for air-gapped reality). HSTS, strict CSP headers, and connection-level rate limiting are default-on.
  * **AD / LDAPS integration** — Open WebUI binds to the corporate domain controller over port 636 with full certificate validation. Group-based access control (`memberOf` filter) restricts the platform to a designated user group, with no self-registration permitted.
  * **Service isolation** — vLLM, PostgreSQL, and Redis bind only to the internal Docker network (`backend`, marked `internal: true` so containers cannot egress even if compromised). Only Nginx is exposed on the host.
  * **Streaming-aware proxy** — `proxy_buffering off` ensures token-by-token streaming reaches the browser without TTFB inflation.


## Stack  
| Layer  | Component  | Version  | Purpose  |  
| --- | --- | --- | --- |  
| OS  | Ubuntu Server LTS  | 24.04.3  | Long-term support, mature NVIDIA driver chain  |  
| GPU driver  | NVIDIA proprietary  | 550-server / 570  | Turing + CUDA 12.x compatible  |  
| Container runtime  | Docker Engine  | 27.x  | + NVIDIA Container Toolkit  |  
| Inference engine  | vLLM  | latest  | PagedAttention, tensor parallelism, OpenAI-compatible API  |  
| Foundation model  | Qwen3-Coder-30B-A3B-Instruct (AWQ)  | 2025.08  | MoE coding LLM, Apache 2.0  |  
| Frontend  | Open WebUI  | latest  | Multi-user chat, RBAC, AD-integrated  |  
| Database  | PostgreSQL  | 16  | Conversation history, audit trail  |  
| Cache / sessions  | Redis  | 7  | Multi-worker coordination  |  
| Reverse proxy  | Nginx  | stable  | TLS termination, WebSocket upgrade, rate limiting  |  
## Quickstart
> **Note:** This is a reference architecture, not a turnkey installer. Read [`docs/airgap-deployment.md`](https://github.com/wu840407/enterprise-airgapped-llm/blob/main/docs/airgap-deployment.md) before running anything in production.

```
# 1. Clone
git clone https://github.com/wu840407/enterprise-airgapped-llm.git
cd enterprise-airgapped-llm

# 2. Generate secrets
cp .env.example .env
# Edit .env with your generated PG_PASSWORD, REDIS_PASSWORD, WEBUI_SECRET

# 3. Provision storage (mdadm RAID 10 + LVM)
sudo bash scripts/02-raid-setup.sh

# 4. Install NVIDIA driver + Docker + Container Toolkit
sudo bash scripts/03-nvidia-docker.sh

# 5. Download models (online phase only)
bash scripts/04-download-models.sh

# 6. Generate self-signed certificate for nginx
mkdir -p certs
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
  -keyout certs/server.key -out certs/server.crt \
  -subj "/CN=ai.local" \
  -addext "subjectAltName=DNS:ai.local,IP:<your-host-ip>"

# 7. Bring up the stack
docker compose up -d
docker compose logs -f vllm   # wait for "Uvicorn running on http://0.0.0.0:8000"
```

Browse to `https://<host-ip>/` and complete first-admin setup.
Multi-user chat served entirely offline — a 30B MoE coding model answering a real question, no cloud API involved:
Retrieval settings for the on-premises document pipeline (chunking, local embedding model, reranking) — everything runs inside the air-gapped network:
## Performance Profile
Measured on the reference hardware (2× Quadro RTX 6000 Passive, Xeon Gold 6248R, 384 GB DDR4):  
| Metric  | Value  |  
| --- | --- |  
| Cold model load (17 GB weights from RAID 10)  | ~31 s  |  
| Single-user generation throughput  | 80–120 tokens/s  |  
| 10 concurrent users  | ~30 tokens/s per user  |  
| Time to first token (warm)  | < 500 ms  |  
| Maximum context window  | 16,384 tokens  |  
| GPU memory utilization  | 90 % per card  |  
For most internal coding-assistant workloads (10–30 active users at peak), the bottleneck is human reading speed rather than the inference engine.
## Documentation
  * [`docs/architecture.md`](https://github.com/wu840407/enterprise-airgapped-llm/blob/main/docs/architecture.md) — Detailed component diagrams and data flow
  * [`docs/hardware-considerations.md`](https://github.com/wu840407/enterprise-airgapped-llm/blob/main/docs/hardware-considerations.md) — Turing-specific tuning and VRAM budget analysis
  * [`docs/troubleshooting.md`](https://github.com/wu840407/enterprise-airgapped-llm/blob/main/docs/troubleshooting.md) — Field-tested fixes for the issues you'll actually hit
  * [`docs/airgap-deployment.md`](https://github.com/wu840407/enterprise-airgapped-llm/blob/main/docs/airgap-deployment.md) — Online-to-offline transition checklist


## What This Project Demonstrates
For the technically inclined reader (recruiters, hiring managers, fellow engineers), this repository is a single-engineer, end-to-end demonstration of:
  * **Linux systems engineering** — mdadm + LVM + EFI on enterprise storage controllers, netplan migrations, systemd service hardening
  * **GPU infrastructure** — NVIDIA driver lifecycle, CUDA / Container Toolkit integration, tensor parallelism on heterogeneous PCIe topologies
  * **Container orchestration** — Multi-service Docker Compose with healthcheck-gated startup ordering, internal-network isolation, log rotation, secrets management
  * **LLM serving** — vLLM tuning for legacy hardware, quantization format reconciliation, MoE deployment economics
  * **Enterprise security posture** — AD/LDAPS integration, internal-CA TLS, air-gap operational discipline, reproducible offline upgrades
  * **Production debugging** — Reading deep stack traces (Pydantic ValidationError, CUDA OOM, Docker entrypoint conflicts) and resolving them without external help


This is the kind of work that doesn't fit on a résumé bullet point. The code, the documentation, and the architecture decisions speak for themselves.
## License
MIT — see [`LICENSE`](https://github.com/wu840407/enterprise-airgapped-llm/blob/main/LICENSE).
The model weights, vLLM, Open WebUI, and all referenced components retain their respective upstream licenses. This repository contains only orchestration, configuration, and documentation.
## Acknowledgments
  * The Qwen team at Alibaba for releasing genuinely competitive open-weight coding models under Apache 2.0
  * The vLLM project for making production LLM serving accessible
  * The Open WebUI maintainers for the enterprise authentication features that make this stack viable in regulated environments


Maintained by [ChengRung Wu](https://wu840407.github.io) — questions and issues welcome.
_If you're building something similar in a regulated environment and have questions, issues are welcome._
## About
Production reference architecture for self-hosted LLM in air-gapped enterprise environments. Dell R740 + Turing GPUs + vLLM + Qwen3-Coder + AD/LDAPS.
### Topics
[active-directory](https://github.com/topics/active-directory)[airgap](https://github.com/topics/airgap)[devops](https://github.com/topics/devops)[docker-compose](https://github.com/topics/docker-compose)[enterprise-deployment](https://github.com/topics/enterprise-deployment)[infrastructure-as-code](https://github.com/topics/infrastructure-as-code)[llm](https://github.com/topics/llm)[lvm](https://github.com/topics/lvm)[mdadm](https://github.com/topics/mdadm)[mlops](https://github.com/topics/mlops)[nvidia-gpu](https://github.com/topics/nvidia-gpu)[on-premise](https://github.com/topics/on-premise)[open-webui](https://github.com/topics/open-webui)[qwen](https://github.com/topics/qwen)[raid](https://github.com/topics/raid)[self-hosted](https://github.com/topics/self-hosted)[ubuntu-server](https://github.com/topics/ubuntu-server)[vllm](https://github.com/topics/vllm)
### Resources
### Stars
**0** stars
### Watchers
**0** watching
### Forks
[ forks](https://github.com/wu840407/enterprise-airgapped-llm/forks)
## 
No releases published
## 
  * [**wu840407** ChengRung Wu](https://github.com/wu840407)
  * [**claude** Claude](https://github.com/claude)


## Languages
  * [Shell100%](https://github.com/wu840407/enterprise-airgapped-llm/search?l=shell)


You can’t perform that action at this time. 
