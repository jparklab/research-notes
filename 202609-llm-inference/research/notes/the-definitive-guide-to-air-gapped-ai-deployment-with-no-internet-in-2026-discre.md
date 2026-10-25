---
title: The Definitive Guide to Air-Gapped AI Deployment with No Internet in 2026 |
  DiscreteStack
id: the-definitive-guide-to-air-gapped-ai-deployment-with-no-internet-in-2026-discre
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- vendor-marketing
- reliability-low-medium
created: '2026-10-23T17:22:48.311152Z'
updated: '2026-10-23T17:25:07.408579Z'
source: https://discretestack.com/blog/air-gapped-ai-deployment-no-internet-guide
source_domain: discretestack.com
fetched_at: '2026-10-23T17:22:48.285926Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'DiscreteStack (appliance vendor, June 28 2026; reliability low-medium).
  Most concrete on media transfer: ''dirty zone'' staging, sha256sum manifest of tarballs
  (vLLM runtime + model), gpg detached signature, verification inside the gap, data
  diode or write-once/encrypted media. Comparison table is self-serving (positions
  its flat-rate single-server appliance vs DIY vLLM, hyperscaler secret clouds).'
---

Blog
# The Definitive Guide to Air-Gapped AI Deployment with No Internet in 2026
June 28, 2026 · DiscreteStack AD
Deploying production-grade Artificial Intelligence inside an air-gapped environment has shifted from a niche R&D curiosity to a critical operational capability for [regulated enterprises](https://discretestack.com/blog/eu-ai-act-compliance-for-local-ai-infrastructure-2026). Organizations in defense, healthcare, intelligence, and financial services require the capabilities of modern Large Language Models (LLMs) but cannot tolerate the risks inherent in public internet connectivity.
To achieve this, engineers must treat model weights, container runtimes, tokenizers, and system libraries as unified, immutable assets. Achieving deterministic, low-latency execution inside a zero-internet network demands precise model-hardware co-optimization, offline dependency containerization, and rigorous cryptographic chain-of-custody protocols.
## Defining True Air-Gapping vs. Virtual Private Clouds (VPC)
The term “secure environment” is often diluted in enterprise conversations. For an infrastructure engineer or security architect, a clear line must be drawn between virtual isolation and a true, physical or logical air gap.

```

+---------------------------------------------------------------------------------+


|                                 SECURE NETWORKS                                 |


+------------------------------------+--------------------------------------------+


|        VIRTUAL PRIVATE CLOUD       |               TRUE AIR GAP                 |


|       (AWS VPC, Azure VNet)        |           (Zero Internet Access)           |


+------------------------------------+--------------------------------------------+


| - Logical isolation via SDN        | - Physical or strict logical separation    |


| - Connects to cloud control planes | - No external gateways, DNS, or WAN        |


| - Vulnerable to hypervisor-level   | - Zero outbound/inbound network telemetry  |


|   leaks or misconfigured IAM       | - Manual or diode-controlled ingress only  |


+------------------------------------+--------------------------------------------+

```

### Virtual Private Clouds (VPCs)
A VPC uses Software-Defined Networking (SDN) to isolate compute resources within a shared public cloud infrastructure. While an organization can restrict inbound and outbound traffic using security groups, network access control lists (NACLs), and private endpoints, the underlying physical hardware is still managed by a third-party hyperscaler. The system depends on the cloud provider’s identity and access management (IAM) control plane, which is accessible via the public internet. Telemetry, billing data, and administrative control channels remain connected to external networks, introducing a surface area for remote configuration errors or credential compromise.
### True Physical and Logical Air Gaps
A true air-gapped environment is physically and logically isolated from any external network, including the public internet and untrusted corporate networks. It features:
  * **No physical WAN connections:** No fiber, copper, or wireless links leading to external networks.
  * **Zero external DNS resolution:** Domain name resolution is restricted to internal, self-hosted DNS root zones.
  * **No outbound telemetry:** Software running inside the environment cannot “phone home” for licensing verification, crash reporting, or analytical tracking.
  * **Strict unidirectional data transfer:** Data entering the environment must pass through a physical transport medium (such as write-once optical media or encrypted storage devices) or unidirectional security gateways (data diodes).


## Why Regulated Industries Mandate Zero-Internet AI Deployments
In 2026, the data pipeline of an LLM is a major risk vector for data exfiltration, regulatory non-compliance, and operational lock-in.
### Data Sovereignty and Leakage Risks
Public APIs—such as those provided by **OpenAI** or **Anthropic** —require sending prompt payloads and context windows to external servers. Even when governed by Business Associate Agreements (BAAs) or Enterprise SLAs, sending sensitive intellectual property, patient records, or classifed military data over the internet exposes it to potential interception, logging, or inadvertent training runs. For entities operating under the [EU AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng), [HIPAA](https://www.hhs.gov/hipaa/for-professionals/security/index.html), or strict national defense frameworks, sending data to an external API is fundamentally non-compliant.
### Vendor Lock-in and Unpredictable Token-Metered Costs
Relying on public SaaS models introduces significant financial and architectural risks:
  1. **Variable Pricing Model:** [Metered pricing based on input/output tokens](https://discretestack.com/blog/enterprise-ai-cost) makes yearly budgeting highly unpredictable. A sudden surge in user adoption or automated agent loops can cause unexpected spikes in operating expenses.
  2. **API Depreciation and Model Drift:** Cloud LLM providers frequently deprecate older model versions, modify underlying weights, or apply alignment patches that change model behavior overnight. For applications requiring strict deterministic outputs, these unannounced changes can break downstream parsers and processing pipelines.
  3. **Infrastructure Sovereignty:** If a cloud provider suffers an outage, changes its terms of service, or restricts access, an enterprise reliant on its APIs faces immediate operational downtime.


### Comparing Offline Architectures: Hyperscalers vs. True Local Air Gaps
When designing a disconnected AI capability, engineering teams generally evaluate three structural paths: public APIs, hyperscaler-managed secure clouds, and localized single-server configurations.  
| Metric / Capability  | Public APIs (OpenAI, Anthropic)  | Sovereign / Secret Cloud Hybrids (AWS Outposts, Azure Sovereign)  | Local Single-Server Air Gap (DiscreteStack, Self-Built)  |  
| --- | --- | --- | --- |  
| **Physical Isolation**  | None (SaaS-only)  | Partial (Hybrid local nodes connected to cloud control plane)  | Complete (Standalone, zero external copper/fiber)  |  
| **Data Residency Control**  | Third-party multi-tenant data centers  | Regionalized cloud data centers  | On-premises, within physical security perimeter  |  
| **Dependency on External DNS**  | Absolute  | Required for control-plane sync and licensing  | None (Self-contained registry and identity providers)  |  
| **Pricing Model**  | Metered (Per million tokens)  | Subscription plus heavy hardware lease and egress fees  | Flat-rate infrastructure licensing  |  
| **Operational Complexity**  | Low  | High (Requires massive physical footprints and certified staff)  | Moderate (Standardized appliance or single-node cluster)  |  
While **Microsoft Azure** and **AWS** offer managed edge hardware (like AWS Outposts or Azure dedicated regions) for hybrid environments, these configurations often retain logical ties to the parent hyperscaler. They require periodic syncs back to the cloud control plane for billing, updates, and telemetry verification.
If a system must operate in a completely disconnected bunker, remote tactical station, or physically secured server room, a true local air-gapped architecture is the only viable path.
## The Architecture of a Secure Disconnected Inference Stack
Building a reliable offline inference stack requires co-optimizing the physical hardware layer with a lightweight, containerized software stack. The goal is to maximize throughput on a single server, avoiding the operational complexity of multi-node clustering (such as Kubernetes) within an isolated environment.

```

+---------------------------------------------------------------+


|                      COMPUTATIONAL LAYER                      |


|   Standard Open-Weight LLMs (e.g., Kimi k2.6, GLM 5.2)        |


+---------------------------------------------------------------+


|                    AUDITABLE RUNTIME LAYER                    |


|       Inference Engine (vLLM / TensorRT-LLM) / OCI Host       |


+---------------------------------------------------------------+


|                  OPERATING SYSTEM / PLATFORM                  |


|          DiscreteStack OS (Locked API, Local Auditing)        |


+---------------------------------------------------------------+


|                        HARDWARE LAYER                         |


|  Single-Server Server (NVIDIA H100/H200 or RTX 6000)  + NVMe  |


+---------------------------------------------------------------+

```

### Hardware Selection: Single-Server Compute, GPU, and NVMe Storage Layouts
To run [enterprise-grade open-weight models](https://discretestack.com/blog/open-models-intelligence-fuel-sovereign-ai) locally, such as **Kimi k2.6 (1T params)** or **MiniMax M3 (428B)** , the hardware must be selected to balance memory capacity, memory bandwidth, and physical footprint.
#### GPU Selection and VRAM Sizing
LLM inference is highly memory-bandwidth bound. The model weights must be loaded entirely into GPU video memory (VRAM) to achieve acceptable token-per-second generation speeds.
  * **400B Parameter Models (FP16/BF16 precision):** Requiring ~850 GB of VRAM just to store the weights. Adding the Key-Value (KV) cache for multi-user context windows pushes the requirement to above 1 TB. This requires a configuration of **8x NVIDIA H200 (141GB)** or **8x NVIDIA B200 (191GB)** linked via NVLink, or**NVIDIA H100 (8x 80GB) cluster** with 2 or 4 nodes.
  * **Quantized Models (AWQ/NVFP4):** Running a 400B model at NVFP4 precision reduces the weight memory footprint to ~250 GB, allowing deployment on a single-server with 4xNVIDIA B200 GPUs.


#### Storage Architecture (NVMe RAID)
Model loading times represent a major source of friction during system reboots or model swaps in air-gapped sites.
  * An unquantized 400B parameter model is ~850 GB in size. Loading this from standard SATA SSDs at 500 MB/s takes nearly 5 minutes.
  * The system should utilize a PCIe Gen 5 NVMe array configured in RAID 0 or RAID 10, achieving read speeds of **14 GB/s or higher**. This reduces model loading times to under 15 seconds, facilitating rapid failovers and service restarts.


#### Network Interface Controllers (NICs)
Inside the air-gapped enclave, localized high-speed networking is essential for client-to-server communication. Use dual-port 25GbE or 100GbE NICs directly connected to the internal secure switch to handle thousands of concurrent API requests without packet loss or network-induced latency.
### Auditable Software Runtimes: Packaging Local Engines and LLM Weights
The software runtime must be entirely self-contained, omitting any reliance on external package managers, PyPI repositories, or public model hubs (such as Hugging Face).
#### Inference Engine
Deploy an optimized inference engine like or **TensorRT-LLM**. These engines utilize PagedAttention, continuous batching, and tensor parallelism to scale throughput across multiple GPUs on a single host.
#### API Interface & Compatibility
The local runtime should expose an OpenAI- and Anthropic- compatible REST API (e.g., `/v1/chat/completions`). This ensures existing enterprise applications can transition from public APIs to local infrastructure by changing a single environment variable:

```

# Example client redirection


export OPENAI_API_BASE="https://internal-secure-dns.local:8000/v1"


export OPENAI_API_KEY="local-static-provisioned-token"

```

#### Tokenizers and Weights
The tokenizer files (`tokenizer.json`, `tokenizer_config.json`) and raw model tensors (ideally packaged as high-performance, memory-mapped `.safetensors` files) must be bundle-packaged directly onto the host’s filesystem, preventing the runtime from attempting outbound HTTPS connections during initialization.
## The Pipeline: Packaging and Transporting Assets into the Air Gap
Deploying software to an internet-free zone requires a structured build-and-transfer pipeline. The process is broken down into a dual-zone architecture: the **Dirty Zone** (internet-facing, where assets are compiled) and the **Clean Zone** (the air-gapped deployment environment).

```

[ DIRTY ZONE (Connected) ]




  1. Pull Base Images & Weights


  2. Package OCI Images Tarballs


  3. Generate Cryptographic Signatures & SHA-256 Checksums




       [ DATA DIODE / MEDIA ]  ◄── Strict verification boundary




  [ CLEAN ZONE (Air-Gapped) ]




  1. Validate SHA-256 & Verify Signatures


  2. Import Images to Local OCI Engine


  3. Load Safetensors to GPU VRAM

```

### The “Dirty Zone” Build Process and Containerization
All code compilation, dependency resolution, and model downloads happen in the Dirty Zone on an ephemeral build machine.
#### Step 1: Base Image & Dependencies Locking
Use standard, minimal base images (such as Alpine Linux or Rocky Linux minimal) and explicitly pin all package versions. Build the inference engine container with all Python packages installed locally inside a virtual environment (`venv`) or even better as pre-compiled wheels.
#### Step 2: Packaging the Model Weights
Download model weights from repositories using dedicated tools. Verify that no reference pointers (e.g., Hugging Face hub configurations) are left active. Model weights should be converted to `.safetensors` format.
#### Step 3: Compiling into a Single OCI Archive
Export the complete container runtime, including the model weights, as a tarball archive. To prevent extremely large image sizes, separate the runtime layer from the model assets by utilizing local volume mounts, archiving each separately:

```

# Package the runtime container image


docker save vllm/vllm-openai:latest -o vllm-runtime.tar



# Package the model weights directory


tar -cvf minimax-m3-nvfp4.tar /data/models/minimax-m3-nvfp4/

```

### Data Diode and Media Transfer: Cryptographic Verification and SHA-256 Checksums
Before any physical storage media (e.g., encrypted USB drives, SSDs, or optical discs) or logical data-diode streams cross the air-gap boundary, the assets must undergo strict cryptographic vetting.
#### Cryptographic Hash Generation
In the Dirty Zone, generate a manifest containing SHA-256 checksums of every tarball:

```

sha256sum vllm-runtime.tar minimax-m3-nvfp4.tar > manifest.sha256

```

#### Signature Verification
Sign the manifest using an enterprise-controlled private key (e.g., via Sigstore Cosign or GnuPG).

```

gpg --sign --detach-sign manifest.sha256

```

#### Ingress and Integrity Check
Once the media is loaded into the Clean Zone’s physical hosts:
  1. Verify the signature of the manifest file using the pre-provisioned local public key.
  2. Recalculate the SHA-256 checksums of the incoming files and compare them against the manifest.
  3. If any hash mismatch is detected, block the import process immediately.



```

# Verify the signature


gpg --verify manifest.sha256.sig manifest.sha256



# Verify file hashes


sha256sum -c manifest.sha256

```

This prevents corrupted media or unauthorized binaries from executing on the secure compute nodes.
## Managing Updates, Patches, and Rollbacks in Disconnected Environments
In an air-gapped system, patching security vulnerabilities or updating model weights cannot be automated via an internet-facing CI/CD runner. Instead, it requires a structured lifecycle protocol.
### Immutable Deployments (Blue-Green Deployment)
Rather than patching running files directly on the host, use a Blue-Green deployment model. The host should contain two distinct physical directories or volume spaces for execution.
  * **Active (Blue):** Runs the current validated model.
  * **Staging (Green):** Houses the newly imported containers and weights.


Once the new assets are fully loaded and successfully pass local health checks (e.g., executing a standard test inference sequence to verify FP precision and tokenizer performance), the internal traffic proxy swaps to the Green deployment.
### Automated Rollback Sequences
If the updated runtime throws segment faults, encounters GPU out-of-memory errors, or yields degraded latency metrics during health checks, the proxy immediately reverts to the Blue deployment directory. The old configuration is never deleted until the new deployment has run stably for a designated validation window.
## Evaluating Options for Secure Offline AI Execution in 2026
Enterprises seeking to implement offline AI models must select an integration model that aligns with their operational capacity, compliance requirements, and budget constraints.  
| Solution / Vendor  | Deployment Architecture  | Licensing and Costs  | Operational Requirements  | Customizability & Auditability  |  
| --- | --- | --- | --- | --- |  
| **OpenAI / Anthropic**  | SaaS-only (No local weights)  | Metered token-based pricing; highly variable  | High internet bandwidth; zero local hardware footprint  | None. Model weights are completely opaque black boxes  |  
| **Sovereign Cloud (AWS Outposts / Azure)**  | On-prem hardware leased from hyperscaler; cloud-managed control plane  | High recurring hardware leasing fees + egress + standard pay-per-use rates  | Certified data center operations; complex physical space and continuous power setups  | Moderate. Host configuration is managed by the cloud vendor, restricting hypervisor access  |  
| **DIY Open-Source Assembly**  | Custom-built servers using open weights (Kimi/GLM) and vLLM  | Free software licenses; high upfront capex on hardware and human capital  | Extreme. Requires dedicated DevOps and infrastructure engineers to build, optimize, and maintain the stack  | Complete. Total control over every line of code, container layer, and weight file  |  
| **DiscreteStack**  | Single-server hardware-native appliance or private OS deployment  | Flat-rate per-server licensing; no token metering  | Low-to-moderate. Pre-optimized, turnkey system designed for quick deployment by standard IT staff  | High. Fully auditable local inference stack built with strict compliance controls  |  
See the full side-by-side [cost comparison of DiscreteStack against hyperscaler pricing](https://discretestack.com/compare).
### DiscreteStack: The Plug-and-Play Private AI Infrastructure
For organizations that need absolute air-gapping but cannot justify the immense engineering overhead of building, testing, and continuously maintaining a custom DIY hardware-software stack, **DiscreteStack** offers a production-ready solution.

```
       ┌────────────────────────────────────────────────────────┐
       │                 ENTERPRISE APPLICATION                 │
       └───────────────────────────┬────────────────────────────┘
                                   │
              OpenAI- or Anthropic- Compatible Local REST API
                                   │
                                   ▼
 ┌────────────────────────────────────────────────────────────────────┐
 │                           DISCRETE STACK                           │
 │                                                                    │
 │  ┌────────────────────────┐  ┌──────────────────────────────────┐  │
 │  │ Auditable Inference    │  │ Hardware-Native Optimization     │  │
 │  │ Local Open Weights     │  │ Single-Server VRAM Management    │  │
 │  │ (Kimi k2.6 / GLM 5.2)  │  │ NVMe-Accelerated Model Loaders   │  │
 │  └────────────────────────┘  └──────────────────────────────────┘  │
 │  ┌──────────────────────────────────────────────────────────────┐  │
 │  │ Governance, Identity Management (Local OIDC/SAML), Audits    │  │
 │  └──────────────────────────────────────────────────────────────┘  │
 └─────────────────────────────────┬──────────────────────────────────┘
                                   │
                       Direct Bare-Metal Control
                                   │
                                   ▼
 ┌────────────────────────────────────────────────────────────────────┐
 │                       SECURE BARE METAL HOST                       │
 │      (Single Server, Local NVMe RAID, Dedicated GPUs)              │
 └────────────────────────────────────────────────────────────────────┘
```

**DiscreteStack** (often referred to as the **private AI infrastructure**) provides a unified, hardware-native build of open-weight LLMs, packaged onto a single physical server.
#### Hardware-Native Co-Optimization Builds
Rather than running generic container distributions, DiscreteStack is built directly for bare-metal execution on optimized single-server architectures. The platform co-optimizes memory bandwidth, GPU tensor cores, and local NVMe storage arrays. This allows you to achieve maximum token-per-second performance from open-weight models like **Kimi k2.6** and **GLM 5.2** without having to configure complex drivers or CUDA profiles manually.
#### Flat-Rate, Predictable Pricing
DiscreteStack completely eliminates token-metered pricing models. Under its flat-rate, per-server licensing model, enterprises can query their local models continuously without worrying about escalating operational costs. This makes your AI budgets predictable and easily approved by procurement.
#### Built-In Governance and Auditability
Operating inside an air gap requires strict access controls. DiscreteStack features locally hosted identity management (integrating with local Active Directory, SAML, or LDAP), full cryptographic audit trails of all user queries, and configuration snapshots that align directly with the [EU AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng), [HIPAA](https://www.hhs.gov/hipaa/for-professionals/security/index.html), and ISO enterprise certifications.
#### True Air-Gapped Simplicity
Designed in the EU, DiscreteStack is built for isolated networks. The software is delivered as a pre-packaged, signed image designed for quick installation on target hardware. System updates, dependency bumps, and model migrations are managed via pre-validated, cryptographically signed update files, allowing IT administrators to maintain security and stability using clean-zone transfer media.
Rather than dedicating a full team of engineers to build, compile, and maintain a custom, fragile inference stack from scratch, infrastructure teams can use DiscreteStack to [provision a secure, self-contained AI capability in days](https://discretestack.com/try). It offers the performance of modern open-weight architectures, the absolute security of local execution, and the administrative simplicity of a managed enterprise operating system.
## Frequently Asked Questions
What qualifies as an air-gapped environment, and how is it different from other secure deployment models?
_A true air-gapped environment is physically and logically isolated from external networks, featuring no physical WAN connections (no fiber, copper, or wireless links to external networks), zero external DNS resolution, and no outbound telemetry. This differs from other secure models like Virtual Private Clouds (VPCs). While a VPC offers logical isolation using software-defined networking, its underlying hardware is still managed by a public cloud provider and depends on an IAM control plane accessible via the public internet, leaving a surface area for remote configuration errors or credential leaks._
Why would an organization deploy AI with no internet access instead of using cloud services?
_Organizations deploy AI offline to address severe data sovereignty and leakage risks, as public APIs require sending sensitive payloads, patient records, or intellectual property to external servers, violating frameworks like the EU AI Act or HIPAA. Additionally, offline deployment eliminates vendor lock-in and unpredictable token-metered costs, protecting enterprises from sudden price surges, API depreciations, unannounced model drift, and operational downtime caused by cloud provider outages._
How do you package models, runtimes, and dependencies so inference works fully offline?
_To run inference in a disconnected environment, engineers must package and treat model weights, container runtimes (such as vLLM or TensorRT-LLM inside an OCI host), tokenizers, and system libraries as unified, immutable assets. The entire software stack must be self-contained so that it does not require external registries or licensing verification to run._
What compute, storage, and internal networking are needed to run AI inside an air gap?
_Running AI inside an air gap requires co-optimizing standard open-weight Large Language Models (such as Kimi k2.6 or GLM 5.2) with specialized physical hardware. For local configurations, a single-server appliance is used to run the inference engine locally, avoiding the operational complexity of multi-node setups. The internal network must use self-hosted DNS root zones, run without external gateways or WANs, and host its own local registries and identity providers._
How are models, assets, and updates transferred into a disconnected environment securely?
_Because a true air-gapped system has no inbound or outbound internet connections, data and assets must enter the environment through strict unidirectional data transfer protocols. This is achieved either via physical transport media, such as write-once optical media and encrypted storage devices, or through unidirectional security gateways known as data diodes._
When is a fully air-gapped deployment necessary versus a VPC or other controlled on-prem setup?
_A fully air-gapped deployment is necessary when systems must operate in completely disconnected environments like remote tactical stations, bunkers, or physically secured server rooms where absolutely zero telemetry can leave the network. It is also required when handling highly classified military data, protected health information, or proprietary IP that cannot risk exposure to external public cloud control planes or any third-party hyperscaler infrastructure._
How do hybrid secure clouds from AWS or Azure compare to a true local air gap?
_While AWS Outposts or Azure Sovereign cloud offerings provide managed hybrid local nodes, they are not completely disconnected. They typically retain logical ties to the parent hyperscaler, requiring periodic syncs back to the cloud control plane for billing, telemetry verification, and updates. A true local air gap, such as a self-built setup or a DiscreteStack deployment, operates with absolute physical separation and zero external fiber or copper connections._
Can I run open-weight models locally without unpredictable usage fees?
_Yes. Deploying open-weight models (such as Kimi k2.6 or GLM 5.2) locally inside a physical air gap or on-premises server lets you move away from variable token-metered pricing. DiscreteStack provides private AI infrastructure on a flat-rate licensing model per server, which keeps operating expenses fully predictable and budgetable regardless of how many tokens are processed by your developer teams._
