---
title: VMware Private AI Foundation with NVIDIA 9.1
id: vmware-private-ai-foundation-with-nvidia-91
tags:
- onprem-llm-inference-stacks-859099
- onprem
- vendor-doc
created: '2026-10-23T18:13:21.479020Z'
updated: '2026-10-23T18:13:41.582048Z'
source: https://techdocs.broadcom.com/content/dam/broadcom/techdocs/us/en/pdf/vmware/private-ai/private-ai-nvidia/vmware-private-ai-foundation-with-nvidia-9-1.pdf
source_domain: techdocs.broadcom.com
fetched_at: '2026-10-23T18:13:21.478491Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'Broadcom PAIF with NVIDIA 9.1 full guide PDF (~98k words, fetched 2026-10-23).
  Reaffirms >=3 GPU-enabled ESX hosts, VCF+PAIF add-on+NVAIE licences. Private AI
  Services 2.1 (releases 20 Mar and 12 May 2026) provides a ''model runtime'' with
  initial support for vLLM, Infinity and llama.cpp behind an ML API gateway; Triton
  deep-learning VM catalog item; disconnected-environment procedures (bootstrap Harbor,
  Ubuntu repo mirror). Vendor doc: recommendation, not deployment evidence. Notable:
  VMware wraps open-source vLLM/llama.cpp rather than building an engine.'
raw_file: raw/vmware-private-ai-foundation-with-nvidia-91.pdf
---

VMware Private AI Foundation with NVIDIA 9.1
VMware by Broadcom


---

VMware Private AI Foundation with NVIDIA 9.1
Contents
Release Notes.......................................................................................................................................6
VMware Private AI Foundation with NVIDIA 9.1.x Release Notes.............................................................................. 6
Introduction.................................................................................................................................................................. 6
What's New................................................................................................................................................................. 6
..............................................................................................................................................................................6
VMware Private AI Foundation with NVIDIA 9.1.1.....................................................................................................7
What's New.......................................................................................................................................................... 7
Compatibility................................................................................................................................................................ 8
VMware Deep Learning VM Images.......................................................................................................................... 8
VMware Private AI Services....................................................................................................................................... 8
Installation and Upgrade.............................................................................................................................................8
License Information.....................................................................................................................................................8
Documentation.............................................................................................................................................................8
Product Support Notices.............................................................................................................................................8
..............................................................................................................................................................................8
VMware Deep Learning VM Image Release Notes....................................................................................................... 9
VMware Private AI Services Release Notes................................................................................................................23
What is VMware Private AI Foundation with NVIDIA?...................................................................38
VMware Private AI Foundation with NVIDIA Documentation.................................................................................... 38
Preparing VCF for Private AI Workload Deployment.....................................................................40
Setup Workflows for Deep Learning VMs and VKS Clusters with GPU...................................................................41
Setup Workflows for Private AI Services.................................................................................................................... 47
System Architecture of VMware Private AI Foundation with NVIDIA.......................................................................56
Physical Infrastructure Resources for AI Workloads.................................................................................................59
Requirements for Deploying VMware Private AI Foundation with NVIDIA.............................................................. 63
Assign a VMware Private AI Foundation with NVIDIA License to the Management Domain................................. 65
Follow the Guided Setup in the vSphere Client......................................................................................................... 66
Deploy a Workload Domain for VMware Private AI Foundation with NVIDIA..........................................................66
Configure NVIDIA vGPU or GPU Passthrough for AI Workloads............................................................................. 68
Add a vSphere Cluster with Advanced NIC Passthrough......................................................................................... 69
Configure VM Classes for AI Workloads.....................................................................................................................71
Configure a Content Library with VKr for a Disconnected Environment.................................................................72
Setting Up VCF Automation for VMware Private AI Foundation with NVIDIA......................................................... 73
Set Up Your VCF Automation Organization............................................................................................................. 74
Set Up a Local Platform Harbor Registry....................................................................................................................80
Finalizing the Setup for Deep Learning VMs and VKS Clusters with GPU..............................................................82
Upgrading from VMware Aria Automation 8.18 to VCF Automation 9.1.................................................................. 82
Import Private AI Foundation Namespaces into VCF Automation.................................................................... 82
Deployment Artifacts for Deep Learning VMs and VKS Clusters in a Disconnected Environment.......................... 89
VMware by Broadcom
2


---

VMware Private AI Foundation with NVIDIA 9.1
Upload the Components for Deep Learning VMs and VKS Clusters to a Disconnected Environment.....................91
Provide an Ubuntu Package Repository for VKS Clusters in a Disconnected Environment.................................... 91
Provide vGPU and GPU Drivers for Deep Learning VMs in a Disconnected Environment......................................92
Create AI Catalog Items in VCF Automation using the Private AI Foundation Quickstart....................................... 92
Installing and Configuring Private AI Services...........................................................................................................97
Identity and Access Management Requirements for Private AI Services................................................................ 98
Configuring Private AI Services for Sharing Models Across Organizations........................................................... 103
Deployment Artifacts for Private AI Services..........................................................................................................104
Deployment Artifacts for Private AI Services 3.0 in a Disconnected Environment..........................................104
Deployment Artifacts for Private AI Services 2.1 in a Disconnected Environment..........................................105
Provide an Ubuntu Package Repository for Private AI Services in a Disconnected Environment......................... 107
Upload Private AI Services Components in a Disconnected Environment............................................................ 107
Upload the Private AI Services 3.0 Components to a Disconnected Environment.........................................107
Upload the Private AI Services 2.1 Components to a Disconnected Environment.........................................109
Install Private AI Services on the Supervisors of the Workload Domain................................................................111
Activating Private AI Services 3.0.......................................................................................................................... 113
Activate Private AI Services 3.0 by Using the UI............................................................................................113
Activate Private AI Services 3.0 by Using kubectl.......................................................................................... 121
Activating Private AI Services 2.1.......................................................................................................................... 133
Activate Private AI Services 2.1 by Using the VCF Automation UI................................................................ 133
Activate Private AI Services 2.1 by Using kubectl.......................................................................................... 138
Turn on GPU for a Private AI Services 3.0 Instance by Using the UI................................................................... 147
Switching Private AI Services 3.0 to CPU..............................................................................................................150
Enable the Full Set of Private AI Services Capabilities......................................................................................... 150
Activate Metrics Collection and LLM Traces.......................................................................................................... 151
Visualizing Private AI Services Metrics in Observability Platforms........................................................................ 152
Deploying a Deep Learning VM in VMware Private AI Foundation with NVIDIA........................159
About Deep Learning VM Images in VMware Private AI Foundation with NVIDIA................................................159
Deploy Deep Learning Virtual Machines by Using Self-Service Catalog Items in VCF Automation....................161
Deploy a Deep Learning VM by Using a Self-Service Catalog Item in VCF Automation....................................... 162
Deploy a Deep Learning VM with NVIDIA Triton Inference Server by Using a Self-Service Catalog Item in VCF 
Automation...............................................................................................................................................................162
Deploy a Deep Learning VM Directly on a vSphere Cluster in VMware Private AI Foundation with NVIDIA.......163
Deploy a Deep Learning VM by Using the kubectl Command in VMware Private AI Foundation with 
NVIDIA............................................................................................................................................................................165
Monitoring the vGPU Performance of a Deep Learning VM....................................................................................170
Customizing Deep Learning VM Deployment in VMware Private AI Foundation with NVIDIA............................. 171
OVF Properties of Deep Learning VMs..................................................................................................................172
Deep Learning Workloads in VMware Private AI Foundation with NVIDIA............................................................176
DCGM Exporter................................................................................................................................................179
Triton Inference Server.................................................................................................................................... 181
Assign a Static IP Address to a Deep Learning VM in VMware Private AI Foundation with NVIDIA..................... 183
Configure a Deep Learning VM with a Proxy Server.............................................................................................185
VMware by Broadcom
3


---

VMware Private AI Foundation with NVIDIA 9.1
Troubleshooting Deep Learning VM Deployment in VMware Private AI Foundation with NVIDIA.......................185
DL Workload Automation Is Not Performed........................................................................................................... 185
Downloading a DL Workload Fails Because of Invalid Authentication Credentials................................................186
The NVIDIA vGPU Guest Driver Is Shown as Unlicensed.................................................................................... 187
Deploying AI Workloads on VKS Clusters in VMware Private AI Foundation with NVIDIA.......188
Deploy a GPU-Accelerated VKS Cluster by Using a Self-Service Catalog Item in VCF Automation................... 188
Provision a VKS Cluster with vGPU by Using kubectl in a Connected Environment...........................................189
Log in to the Namespace in Your Organization by using the VCF Consumption CLI............................................ 189
Provision the VKS Cluster with vGPU in a Connected Environment..................................................................... 190
Install and Configure the NVIDIA GPU Operator from the NVIDIA Registry..........................................................191
Provision a VKS Cluster with vGPU by Using kubectl in a Disconnected Environment......................................193
Log in to the Namespace in Your Organization by using the VCF Consumption CLI............................................ 194
Provision the VKS Cluster with vGPU in a Disconnected Environment.................................................................194
Install and Configure the NVIDIA GPU Operator from the Local Registry.............................................................196
Provision a VKS Cluster with GPU Passthrough by Using kubectl in a Connected Environment...................... 199
Log in to the Namespace in Your Organization by using the VCF Consumption CLI............................................ 199
Provision a VKS Cluster with GPU Passthrough in a Connected Environment.....................................................200
Install the NVIDIA GPU Operator from the NVIDIA Helm Chart Repository.......................................................... 202
Provision a VKS Cluster with GPU Passthrough by Using kubectl in a Disconnected Environment................. 203
Log in to the Namespace in Your Organization by using the VCF Consumption CLI............................................ 203
Provision the VKS Cluster with GPU Passthrough in a Disconnected Environment..............................................204
Install the NVIDIA GPU Operator from the Local Repository.................................................................................206
Delivering Generative AI Applications by Using Private AI Services.........................................210
Storing ML Models in VMware Private AI Foundation with NVIDIA........................................................................210
Before You Begin Storing ML Models in VMware Private AI Foundation with NVIDIA.......................................... 212
Validate and Upload an ML Model to a Model Gallery.......................................................................................... 212
Deploy an ML Model to Run Inference.................................................................................................................. 214
Import a CA Trust Bundle in Private AI Services.....................................................................................................215
Running Completion or Embedding Models by Using Model Endpoints.............................................................. 215
Deploy a Completion or Embedding Model Endpoint............................................................................................ 217
Deploy Completion or Embedding Model Endpoints by Using the kubectl Command...........................................220
Troubleshooting Model Endpoint Deployments...................................................................................................... 224
Generate API Tokens for VCF Automation or Local Private AI Services Accounts..............................................226
Share a Model with Other Private AI Services Instances........................................................................................228
Connect to a Shared Model Running in Another Private AI Services....................................................................228
Connect to a Remote Model Running in the Cloud................................................................................................. 232
Integrating Specialized Data and Capabilities over MCP........................................................................................ 236
Connect an MCP Server to Private AI Services.................................................................................................... 236
Explore the MCP Tools Available to Private AI Services Agents........................................................................... 237
Adding Context to Model Responses by Using Knowledge Bases........................................................................238
Add a Data Source for a Knowledge Base............................................................................................................ 239
Create a Knowledge Base with Linked Data Sources........................................................................................... 240
Create a Knowledge Base with Local Files............................................................................................................241
VMware by Broadcom
4


---

VMware Private AI Foundation with NVIDIA 9.1
Considerations When Modifying and Indexing Knowledge Bases......................................................................... 242
Create an Agent for a Generative AI Application.....................................................................................................244
Pre-Load Tool Results in the Context of an Agent.................................................................................................. 252
Observing the Performance of Private AI Services Components by Using Metrics.............................................255
Tracing Agent Operation and Knowledge Base Indexing........................................................................................258
Monitoring VMware Private AI Foundation with NVIDIA..............................................................262
Documentation Legal Notice.......................................................................................................... 265
VMware by Broadcom
5


---

VMware Private AI Foundation with NVIDIA 9.1
Release Notes
Release highlights, such as what's new, supported software components, known issues, and other, for VMware Private AI 
Foundation with NVIDIA provide information about each release.
In This Chapter
•
VMware Private AI Foundation with NVIDIA 9.1.x Release Notes
•
VMware Deep Learning VM Image Release Notes
•
VMware Private AI Services Release Notes
VMware Private AI Foundation with NVIDIA 9.1.x Release Notes
This document contains the following sections
•
Introduction
•
What's New
•
VMware Private AI Foundation with NVIDIA 9.1.1
•
Compatibility
•
VMware Deep Learning VM Images
•
VMware Private AI Services
•
Installation and Upgrade
•
License Information
•
Documentation
•
Product Support Notices
Introduction
You can use VMware Private AI Foundation with NVIDIA to run AI/ML workloads by using accelerated computing from 
NVIDIA, and virtual infrastructure management and cloud management from VMware Cloud Foundation (VCF).
VMware Private AI Foundation with NVIDIA is based on VCF, with added GPU- and AI-centric functionality in VCF 
Operations, VCF Automation, and VMware Data Services Manager.
VMware Private AI Foundation with NVIDIA 9.1 | 12 MAY 2026
VMware Private AI Foundation with NVIDIA 9.1.1 | 03 SEP 2026
Check for additions and updates to these release notes.
What's New
Support for the HGX platform with NVIDIA Blackwell GPUs and NVIDIA NVSwitch
VCF supports NVIDIA HGXTM
™ platforms with Blackwell GPUs and NVIDIA NVSwitchTM
™. You can now get all the 
benefits of large-scale AI deployments and virtualization.
DirectPath enablement for GPUs
VMs and Kubernetes cluster nodes can get high-performance, exclusive access to GPU resources without the need for an 
NVIDIA AI Enterprise (NVAIE) license.
VMware by Broadcom
6


---

VMware Private AI Foundation with NVIDIA 9.1
High-speed networking with Enhanced DirectPath and NVIDIA BlueField-3 and NVIDIA ConnectX-7
You maintain the benefits of vSphere vMotion while getting ultra-low latency direct data pathways between NVIDIA GPUs 
using NVIDIA BlueField-3 and NVIDIA® ConnectX®-7 SmartNIC.
Kubernetes AI Conformance for VKS using Dynamic Resource Allocation (DRA)
VCF is now a fully interoperable platform for enterprise AI, enabling organizations to run machine learning and generative 
AI workloads using the same open standards as other cloud providers.
Updated component versions in the deep learning VM image for VCF 9.1
New versions of the Ubuntu OS and the ML libraries, frameworks, and toolkits in the image are available. See the Deep 
Learning VM Image Release Notes.
Support for Private AI Services installation in disconnected (air-gapped) environments
You can use the artifact mirroring tool for Private AI Services to replicate the installation artifacts to a local OCI registry, 
such as Harbor. 
Private AI Services enablement in the VCF Automation UI
As an organization administrator, you can use the VCF Automation UI to enable all services for consumers of private AI, 
including Private AI Services.
Support for secure AI agent workflows using Model Context Protocol (MCP) servers and agent tools
Integrate AI agents with both internal and third-party MCP servers, while MLOps retains full governance on which tools 
each agent can access, ensuring secure and compliant workflows across the enterprise.
Improved observability for models running in Private AI Services
You can understand which models are being most heavily  used (using tokens, API requests, etc.) to inform scaling and 
cost decisions. 
Support for running models on CPUs by using llama.cpp
Not all AI applications require GPUs – many work well with CPUs. Running models on CPUs expands your operational 
flexibility for AI workloads and reduces cost. 
VMware Private AI Foundation with NVIDIA 9.1.1
What's New
•
New version of Private AI Services, version 3.0, is available. See VMware Private AI Services Release Notes.
•
New version of VMware Deep Learning VM image is available. See VMware Deep Learning VM Image Release Notes.
VMware by Broadcom
7


---

VMware Private AI Foundation with NVIDIA 9.1
Compatibility
The functionality of VMware Private AI Foundation with NVIDIA is available in VCF and certain versions of VMware Data 
Services Manager.
VCF Version
Versions of VMware Data Services Manager
VCF 9.1.0
VCF 9.1.1
VMware Data Services Manager 9.1.x
VMware Deep Learning VM Images
VMware Private AI Foundation with NVIDIA supports provisioning pre-configured deep learning VMs that data scientists 
can use for AI development or MLOps engineers - for validation of ML mode. For release information about published 
deep learning VM images, see VMware Deep Learning VM Image Release Notes.
VMware Private AI Services
VMware Private AI Services is a platform for deployment of AI workloads providing public cloud like experience. For 
release information on the Private AI Services versions, see VMware Private AI Services Release Notes.
Installation and Upgrade
VMware Private AI Foundation with NVIDIA runs on top of VCF and is based on the components included in the bill of 
materials of VCF.
For information about deploying or upgrading VCF, see the VCF Deployment, Convergence, and Upgrade documentation.
For information about adding the functionality of VMware Private AI Foundation with NVIDIA on top of VCF, see Preparing 
VMware Cloud Foundation for Private AI Workload Deployment.
License Information
VMware Private AI Foundation with NVIDIA is available under a solution license for VCF. If you plan to run AI workloads 
on NVIDIA vGPU, you also need an NVIDIA AI Enterprise license for the host driver VIB file for ESX hosts and the guest 
OS drivers, and for downloading AI container images from the NVIDIA NGC catalog. See Requirements for Deploying 
VMware Private AI Foundation with NVIDIA.
Documentation
Examine the VMware Private AI Foundation with NVIDIA Guide for an overview and how-to instructions on running AI 
workloads and storing ML models in a VCF environment.
Product Support Notices
NVIDIA GPU Operator v25.3.1 that is used in the AI Kubernetes Cluster VCF Automation catalog item 
has reached End-of-Life.
You can update the GPU Operator version in the blueprint to v26.3.1. See Broadcom knowledge base article 439984.
VCF Automation Catalog Items for NVIDIA RAG Are Removed in This Release
NVIDIA is no longer providing support for the blueprints that back these catalog items.
VMware by Broadcom
8


---

VMware Private AI Foundation with NVIDIA 9.1
VMware Deep Learning VM Image Release Notes
This document contains the following sections
•
Introduction
•
Content Library
•
Compatibility and Upgrade
•
Deep Learning VM Image Releases
•
Installation
•
VMware Deep Learning VM 9.1.1
– Content Library URL
– Image Snapshot
– What's New
– Components of VMware Deep Learning VM 9.1.1
•
VMware Deep Learning VM 9.1
– Content Library URL
– Image Snapshot
– What's New
– Components of VMware Deep Learning VM 9.1
– Security Fixes
– Support Notes
– Resolved Issues
•
VMware Deep Learning VM 9.1.0.0200
– Content Library URL
– Image Snapshot
– What's New
– Components of VMware Deep Learning VM 9.1.0.0200
– Security Fixes
•
VMware Deep Learning VM 9.0
– Content Library URL
– Image Snapshot
– What's New
– Support Notes
– Supported NVIDIA GPU Devices
– Components of VMware Deep Learning VM 9.0
– Known Issues
– Resolved Issues
•
VMware Deep Learning VM 9.0.1
– Content Library URL
– Image Snapshot
– What's New
– Support Notes
– Resolved Issues
•
VMware Deep Learning VM 9.0.2
– Content Library URL
– Image Snapshot
– What's New
– Components of VMware Deep Learning VM 9.0.2
– Security Fixes
•
VMware Deep Learning VM 1.2
– Image Snapshot
VMware by Broadcom
9


---

VMware Private AI Foundation with NVIDIA 9.1
– What's New
– Supported NVIDIA GPU Devices
– Components of VMware Deep Learning VM 1.2
– Resolved Issues
•
VMware Deep Learning VM 1.1
– Image Snapshot
– What's New
– Supported NVIDIA GPU Devices
– Components of VMware Deep Learning VM 1.1
– Resolved Issues
•
VMware Deep Learning VM 1.0.1
– Image Snapshot
– What's New
– Supported NVIDIA GPU Devices
– Components of VMware Deep Learning VM 1.0.1
– Resolved Issues
•
VMware Deep Learning VM 1.0
– Image Snapshot
– Supported NVIDIA GPU Devices
– Components of VMware Deep Learning VM 1.0
•
Known Issues
•
License Information
•
Documentation
Introduction
The VMware Deep Learning VM images are delivered as part of VMware Private AI Foundation with NVIDIA. They are 
preconfigured with popular DL workloads, and are optimized and validated by NVIDIA and VMware for GPU acceleration 
in a VMware Cloud Foundation (VCF) environment.
VMware Deep Learning VM 9.1.1 | 03 SEP 2026
VMware Deep Learning VM 9.1 | 12 MAY 2026
VMware Deep Learning VM 9.1.0.0200 | 13 JUL 2026
VMware Deep Learning VM 9.0 | 17 JUN 2025
VMware Deep Learning VM 9.0.1 | 29 SEP 2025
VMware Deep Learning VM 9.0.2 | 20 JAN 2026
Check for additions and updates to these release notes.
Content Library
Deep learning VM images are delivered as vSphere VM templates, hosted and published by VMware in a content library 
that is specific to the VCF release. You can use these images to deploy deep learning VMs by using VCF Automation or 
the vSphere Client.
In a connected environment, you create a subscribed content library connected to this URL.
For a disconnected environment, download the OVA image of the deep learning VM and upload it to a local content 
library:
•
For version 9.0 and version 9.1 of the VMware Deep Learning VM Image, you download the OVA image from the
VMware Private AI area on the Broadcom Support Portal or from the content library HTTP URL.
•
For versions earlier than 9.0, you download the OVA image from the content library HTTP URL.
Compatibility and Upgrade
Use the latest release of VMware Deep Learning VM if supported by your environment.
VMware by Broadcom
10


---

VMware Private AI Foundation with NVIDIA 9.1
Updating a running deep learning VM to a later image is not supported. You must deploy a new deep learning virtual 
machine by using a later deep learning VM image release.
Deep Learning VM Image Releases
Deep learning VM images are distributed separately from the VMware Private AI Foundation with NVIDIA core 
functionality. A deep learning VM image is available at the release date of VMware Private AI Foundation with NVIDIA. 
After VMware Private AI Foundation with NVIDIA is released, you can install newer deep learning VM images without the 
need to update VMware Private AI Foundation with NVIDIA.
Installation
You deploy a deep learning VM image from a content library on the vCenter instance for the AI-ready workload domain in 
VCF. You can deploy a deep learning VM on the following systems:
•
As a data scientists, MLOps engineer, or DevOps engineer
– In a namespace by using the VCF Automation catalog or the kubectl command.
•
As a VI administrator
– Directly on a vSphere cluster
See Deploying a Deep Learning VM in VMware Private AI Foundation with NVIDIA.
VMware Deep Learning VM 9.1.1
Content Library URL
The content library with deep learning VM images for VMware Private AI Foundation with NVIDIA is available at the
https://dl.broadcom.com/<download_token>/PROD/COMP/DLVM/9.1.1.0/lib.json URL.
Image Snapshot
VMware Deep Learning VM 9.1.1 is available for use with VCF 9.1.1
Snapshot
Release Date
Compatible VCF Version
VMware-Deep-Learning-VM-Image-nv-9.1. 
1.0.25664150
03 SEP 2026
VCF 9.1.1
What's New
Added deep-learning-container image into the VMware Deep Learning VM product.
This container image is an upgraded version of the tkg-gpu-console container image that is used in the AI 
Kubernetes Cluster catalog item in VCF Automation. For information on how to configure the AI Kubernetes Cluster 
blueprint to use tkg-gpu-console, see .
Other Changes
•
Using the latest Ubuntu 26.04 LTS image as the base OS image.
– Embedded component versions are also updated
•
Extended set of VCF Consumption CLI plugins.
•
Added the helm command and the kubectl vSphere plug-in.
•
Functional and security bug fixes.
Components of VMware Deep Learning VM 9.1.1
Software Component 
Category
Software Component
Version
Canonical Ubuntu
26.04 LTS
Embedded
NVIDIA Container Toolkit
1.19.0
VMware by Broadcom
11


---

VMware Private AI Foundation with NVIDIA 9.1
Software Component 
Category
Software Component
Version
Docker Community Engine
29.6.0
Miniforge
26.1.1
VCF Consumption (vcf) CLI
v9.1.0
helm
v4.2.0
Kubernetes CLI
v1.32.9+vmware.2-fips
NVIDIA vGPU guest driver
According to the version of 
the NVIDIA vGPU host driver
This release of VMware Deep 
Learning VM supports NVAIE 
8.0 and later versions of 
vGPU host drivers (595.x.x).
NVIDIA data center GPU driver
595.71.05
PyTorch Conda Environment
2.12.1 (Python 3.14)
CUDA Sample
12.5.0-ubi8
PyTorch
Production Branch 6 
26.03.02-py3
26.04-py3
DCGM Exporter
4.5.3-4.8.2-distroless
Can be pre-installed 
automatically
Deep learning (DL) workload 
from NVIDIA NGC
Triton Server
Production Branch 6 
26.03.02-py3
26.04-py3
VMware Deep Learning VM 9.1
Content Library URL
The content library with deep learning VM images for VMware Private AI Foundation with NVIDIA is available at the
https://dl.broadcom.com/<download_token>/PROD/COMP/DLVM/9.1.0.0/lib.json URL.
Image Snapshot
VMware Deep Learning VM 9.1 is available for use with VCF 9.1.
Snapshot
Release Date
Compatible VCF Version
VMware-Deep-Learning-VM-Image-nv-9.1. 
0.0.25217931
12 MAY 2026
VCF 9.1
What's New
Support NVIDIA GPU in passthrough mode.
If the NVIDIA GPU device that is attached to the deep learning VM is running in passthrough mode, the NVIDIA data 
center GPU driver is automatically installed.
•
By default, the deep learning VM image is configured to install the NVIDIA data center GPU driver version 580.95.05.
•
The deep learning VM image uses a new OVF parameter gpu-driver-url for the location of the NVIDIA data center 
GPU driver in a disconnected environment.
Use the latest Ubuntu 24.04 LTS image as the base OS image.
Embedded component versions are also updated.
Support the generation of support bundles.
Now includes /opt/dlvm/dlvm-support.sh for generating a DLVM support bundle.
VMware by Broadcom
12


---

VMware Private AI Foundation with NVIDIA 9.1
VMware Private AI Services (pais) CLI is removed from the image.
You can use the pais plug-in for the VCF Consumption CLI (vcf pais) instead.
GPU driver installation log changed from /var/log/vgpu-install.log to /var/log/gpu-install.log.
/var/log/gpu-install.log contains the logs for the installation of both vGPU guest driver or the data center GPU 
driver.
Components of VMware Deep Learning VM 9.1
Software Component 
Category
Software Component
Version
Canonical Ubuntu
24.04 LTS 3
NVIDIA Container Toolkit
1.18.1
Docker Community Engine
27.5.1
Miniforge
24.11.3
VCF Consumption (vcf) CLI
9.0.1
Embedded
Private AI Services (pais) plug-in for the VCF Consumption CLI
2.0.89
NVIDIA vGPU guest driver
According to the version 
of the NVIDIA vGPU host 
driver
This release of VMware 
Deep Learning VM 
supports both NVAIE 
7.x vGPU host driver 
(580.x.x) and NVAIE 
8.0 vGPU host driver 
(595.58.02).
NVIDIA data center GPU driver
580.95.05
PyTorch Conda Environment
2.7.0 (Python 3.12), 
2.3.0 (Python 3.12), 
1.13.1(Python 3.10)
TensorFlow Conda Environment
2.19.0 (Python 3.12), 
2.16.0 (Python 3.12), 
1.15.5 (Python 3.7)
CUDA Sample
-
PyTorch
-
DCGM Exporter
-
Can be pre-installed 
automatically
Deep learning (DL) workload 
from NVIDIA NGC
Triton Inference Server
-
Security Fixes
The VMware Deep Learning VM 9.1 image contains fixes of security vulnerabilities.
Support Notes
Deep learning VM console is deprecated.
The deep learning VM console is deprecated starting from this release. It will be removed in a future release of the deep 
learning VM image.
Miniforge is deprecated.
The Miniforge component in the deep learning VM image is deprecated starting from this release. It will be removed in a 
future release of the deep learning VM image.
VMware by Broadcom
13


---

VMware Private AI Foundation with NVIDIA 9.1
Resolved Issues
Deep learning VM content libraries cannot be created or updated because of an expired signing certificate
The key used to sign all published VMware Deep Learning VM Image releases expired on January 3, 2026. These images 
can no longer be synced to or uploaded to content libraries that are configured to enforce certificate validity, that is, have a 
security policy applied.
VMware Deep Learning VM 9.1.0.0200
Content Library URL
The content library with deep learning VM images for VMware Private AI Foundation with NVIDIA is available at https://
dl.broadcom.com/<download_token>/PROD/COMP/DLVM/9.1.0.0200/lib.json URL.
Image Snapshot
VMware Deep Learning VM 9.1.0.0200 is available for use with VCF 9.1.
Snapshot
Release Date
Compatible VCF Version
VMware-Deep-Learning-VM-Image-nv-9.1.0.0200.2 
5510948
13 JUL 2026
VCF 9.1
What's New
The VMware Deep Learning VM 9.1.0.0200 image contains fixes of security vulnerabilities.
Components of VMware Deep Learning VM 9.1.0.0200
The component configuration of the VMware Deep Learning VM image 9.1.0.0200 is the same as the configuration of 
VMware Deep Learning VM image 9.1.
Security Fixes
The VMware Deep Learning VM 9.1.0.0200 image contains fixes of security vulnerabilities.
VMware Deep Learning VM 9.0
Content Library URL
The content library with deep learning VM images for VMware Private AI Foundation with NVIDIA is available at the
https://dl.broadcom.com/<download_token>/PROD/COMP/DLVM/9.0.0.0/lib.json URL.
Image Snapshot
VMware Deep Learning VM 9.0 is available for use with VCF 9.0.
Snapshot
Release Date
Compatible VCF Version
VMware-Deep-Learning-VM-Image-nv-vgp 
u-9.0.0.0.24628237
17 JUN 2025
VCF 9.0
What's New
•
You can use an NGC API key for vGPU guest driver download.
– You provide the key in the new NGC API key OVF parameter.
– Using an API from the NVIDIA Licensing Portal is deprecated.
VMware by Broadcom
14


---

VMware Private AI Foundation with NVIDIA 9.1
•
For step-by-step guidance for configuring, launching, customizing, and debugging each workload container 
component, you can use a VM console in the form of a pre-installed JupyterLab instance.
•
The image contains VCF Consumption CLI 3.1.0 with pais plug-in 2.0.0 installed for storing models in a model gallery.
•
The NVIDIA Container Toolkit is upgraded to v1.17.1.
Support Notes
•
The Private AI Services (pais) CLI 1.0.0 component is deprecated and will be removed in next release of the deep 
learning VM image.
•
In VCF Automation, the preinstalled catalog item for AI workstation with TensorFlow is deprecated and will be removed 
in the next release of VCF.
Supported NVIDIA GPU Devices
VMware Deep Learning VM 9.0 supports the following GPUs on your ESX hosts:
NVIDIA Component
Supported Option
NVIDIA GPUs
•
NVIDIA A100
•
NVIDIA L40S
•
NVIDIA H100
GPU sharing mode
•
Time slicing
•
Multi-Instance GPU
Components of VMware Deep Learning VM 9.0
This version of the deep learning virtual machine image contains the following software:
Software Component 
Category
Software Component
Version
Canonical Ubuntu
22.04
NVIDIA Container Toolkit
1.17.1
Docker Community Engine
26.0.2
Miniforge
24.3.0 (Python 3.10)
VCF Consumption (vcf) CLI
3.1.0
Private AI Services (pais) plug-in for the VCF Consumption CLI
2.0.0
Embedded
Deprecated. VMware Private AI Services (pais) CLI
1.0.0
NVIDIA vGPU guest driver
According to the version 
of the NVIDIA vGPU host 
driver
PyTorch Conda Environment
2.3.0 (Python 3.12), 
1.13.1 (Python 3.10)
TensorFlow Conda Environment
2.16.1 (Python 3.12), 
1.15.5 (Python 3.7)
CUDA Sample
-
PyTorch
-
TensorFlow
-
DCGM Exporter
-
Triton Inference Server
-
Can be pre-installed 
automatically
Deep learning (DL) workload 
from NVIDIA NGC
NVIDIA RAG
-
Known Issues
Cloned deep learning VM does not have an IP address or gets the IP address of the source deep learning VM
VMware by Broadcom
15


---

VMware Private AI Foundation with NVIDIA 9.1
Workaround: Before the cloning operation, perform the following steps on the source deep learning VM, or if the VM has 
already been cloned, on the cloned VM.
1. From the /etc/netplan/50-cloud-init.yaml file, remove the lines for matching a network device by MAC 
address and for setting a network device name.
For example, change this configuration from:
network: 
    ethernets: 
        ens33: 
            dhcp4: true 
            match: 
                macaddress: 04:50:56:00:28:00 
            set-name: ens33 
    version: 2
to:
network: 
    ethernets: 
        ens33: 
            dhcp4: true 
    version: 2
2. In the command prompt, verify the update by running the following command.
echo -n > /etc/machine-id
3. If you have made the change on the cloned VM, restart it.
Unable to download the vGPU guest driver over HTTP when using an HTTPS proxy
During deployment, when using an HTTPS proxy to download the vGPU guest driver from a download server over HTTP 
by using the vgpu-url OVF parameter, the download operation fails with the following error:
ValueError: Cannot set verify_mode to CERT_NONE when check_hostname is enabled.
Workaround: In the cloud-init code, set the variable_noProxy environment variable or the noProxy_url code 
variable to the vGPU driver URL.
Resolved Issues
•
Fixed Failed to initialize NVML: Unknown Error in GPU containers.
VMware Deep Learning VM 9.0.1
As a maintenance release, VMware Deep Learning VM 9.0.1 supports the same OVF parameters, supported NVIDIA 
GPU devices, and component versions as VMware Deep Learning VM 9.0.0.
Content Library URL
The content library with deep learning VM images for VMware Private AI Foundation with NVIDIA is available at the
https://dl.broadcom.com/<download_token>/PROD/COMP/DLVM/9.0.1.0/lib.json URL.
Image Snapshot
VMware Deep Learning VM 9.0.1 is available for use with VCF 9.0.x.
Snapshot
Release Date
Compatible VCF Version
VMware-Deep-Learning-VM-Image-nv-vgp 
u-9.0.1.0.24882593
29 SEP 2025
VCF 9.0.x
What's New
•
In a connected environment, the download URL of the vGPU guest driver is retrieved from a file on NVIDIA NGC 
instead of on packages.vmware.com.
VMware by Broadcom
16


---

VMware Private AI Foundation with NVIDIA 9.1
•
You can now use personal and service NVIDIA NGC API keys in addition to legacy API keys for vGPU guest driver 
installation.
Support Notes
Deploying deep learning VMs is supported only on hosts running NVIDIA AI Enterprise (NVAIE) 6.x or earlier of the vGPU 
host driver.
Resolved Issues
Unable to download the vGPU guest driver over HTTP when using an HTTPS proxy
During deployment, when using an HTTPS proxy to download the vGPU guest driver from a download server over HTTP 
by using the vgpu-url OVF parameter, the download operation fails with the following error:
ValueError: Cannot set verify_mode to CERT_NONE when check_hostname is enabled.
VMware Deep Learning VM 9.0.2
As a maintenance release, VMware Deep Learning VM 9.0.2 supports the same OVF parameters and NVIDIA GPU 
devices as VMware Deep Learning VM image 9.0.0 and VMware Deep Learning VM image 9.0.1.
Content Library URL
The content library with deep learning VM images for VMware Private AI Foundation with NVIDIA is available at the
https://dl.broadcom.com/<download_token>/PROD/COMP/DLVM/9.0.2.0/lib.json URL.
Image Snapshot
VMware Deep Learning VM 9.0.2 is available for use with VCF 9.0.x.
Snapshot
Release Date
Compatible VCF Version
VMware-Deep-Learning-VM-Image-nv-vgp 
u-9.0.2.0.25067982
20 JAN 2026
VCF 9.0.x
What's New
•
You can now deploy a deep learning VM on hosts running NVIDIA AI Enterprise (NVAIE) 7.x of the vGPU host driver.
•
The versions of some of the components in the VMware Deep Learning VM 9.0.2 image have been updated.
Components of VMware Deep Learning VM 9.0.2
Software Component 
Category
Software Component
Version
Canonical Ubuntu
22.04 - 20251031
NVIDIA Container Toolkit
1.18.0
Docker Community Engine
26.0.2
Miniforge
24.3.0 (Python 3.10)
VCF Consumption (vcf) CLI
3.1.0
Private AI Services (pais) plug-in for the VCF Consumption CLI
2.0.0
Embedded
Deprecated. VMware Private AI Services (pais) CLI
1.0.0
NVIDIA vGPU guest driver
According to the version 
of the NVIDIA vGPU host 
driver
Can be pre-installed 
automatically
PyTorch Conda Environment
2.3.0 (Python 3.12), 
1.13.1 (Python 3.10)
VMware by Broadcom
17


---

VMware Private AI Foundation with NVIDIA 9.1
Software Component 
Category
Software Component
Version
TensorFlow Conda Environment
2.16.1 (Python 3.12), 
1.15.5 (Python 3.7)
CUDA Sample
-
PyTorch
-
TensorFlow
-
DCGM Exporter
-
Triton Inference Server
-
Deep learning (DL) workload 
from NVIDIA NGC
NVIDIA RAG
-
Security Fixes
The VMware Deep Learning VM 9.0.2 image contains fixes of security vulnerabilities.
VMware Deep Learning VM 1.2
Image Snapshot
VMware Deep Learning VM 1.2 is available for use with VMware Cloud Foundation 5.2.1.
Snapshot
Release Date
Compatible VMware Cloud Foundation 
Version
common-container-nv-vgpu-ubuntu-2204-v 
20240814
09 OCT 2024
VMware Cloud Foundation 5.2.1
What's New
•
The deep learning VM image includes the Broadcom EULA and VMware Private AI Foundation with NVIDIA SPD 
(specific program documentation).
•
The embedded Miniconda 24.3.0 component is updated to Miniforge3 24.3.0.
•
In addition to pytorch2.3.0_py3.12, by using the Conda Environment Install OVF parameter, you can also 
have the pytorch1.13.1_py3.10, tf2.16.1_py3.12, and tf1.15.5_py3.7 Conda environments installed 
during VM deployment.
•
Private AI Services (pais) CLI version 1.0.0 for storing ML models in a central Harbor registry is now available.
•
In a connected environment, downloading the vGPU guest driver now requires only NVIDIA AI Enterprise entitlement.
•
In a connected environment, error messages that appear while downloading the vGPU guest driver are improved.
Supported NVIDIA GPU Devices
VMware Deep Learning VM 1.2 supports the following GPUs on your ESXi hosts:
NVIDIA Component
Supported Option
NVIDIA GPUs
•
NVIDIA A100
•
NVIDIA L40S
•
NVIDIA H100
GPU sharing mode
•
Time slicing
•
Multi-Instance GPU
VMware by Broadcom
18


---

VMware Private AI Foundation with NVIDIA 9.1
Components of VMware Deep Learning VM 1.2
This version of the deep learning virtual machine image contains the following software:
Software Component 
Category
Software Component
Version
Canonical Ubuntu
22.04
NVIDIA Container Toolkit
1.15.0
Docker Community Engine
26.0.2
Miniforge
24.3.0-0 (Python 3.10)
Embedded
VMware Private AI Services (pais) CLI
1.0.0
NVIDIA vGPU guest driver
According to the version of the 
NVIDIA vGPU host driver
PyTorch Conda Environment
2.3.0 (Python 3.12), 1.13.1 
(Python 3.10)
TensorFlow Conda Environment
2.16.1 (Python 3.12), 1.15.5 
(Python 3.7)
CUDA Sample
-
PyTorch
-
TensorFlow
-
DCGM Exporter
-
Triton Inference Server
-
Can be pre-installed 
automatically
Deep learning (DL) workload 
from NVIDIA NGC
NVIDIA RAG
-
Resolved Issues
•
Containers deployed by using cloud-init are running as root.
•
When the deep learning VM is restarted, only log information from the most recent boot is visible in /var/log/
dl.log. DL workload log information from earlier boots is overwritten.
•
Installation of Conda environments fails if the password OVF parameter is set.
VMware Deep Learning VM 1.1
Image Snapshot
VMware Deep Learning VM 1.1 is available for use with VMware Cloud Foundation 5.2.
Snapshot
Release Date
Compatible VMware Cloud Foundation 
Version
common-container-nv-vgpu-ubuntu-2204-v 
20240613
23 JUL 2024
VMware Cloud Foundation 5.2
What's New
•
The deep learning VM image now contains a built-in Miniconda installation.
•
The deep learning VM image now contains a verified PyTorch Conda environment manifest.
•
You can use the Conda Environment Install OVF parameter to specify a comma-separated list of Conda 
environments to automatically install during VM deployment. Currently, you can install a pytorch2.3_py3.12
environment.
•
More detailed logs on the initialization script are available in /var/log/vgpu-install.log.
VMware by Broadcom
19


---

VMware Private AI Foundation with NVIDIA 9.1
Supported NVIDIA GPU Devices
VMware Deep Learning VM 1.1 supports the following GPUs on your ESXi hosts:
NVIDIA Component
Supported Option
NVIDIA GPUs
•
NVIDIA A100
•
NVIDIA L40S
•
NVIDIA H100
GPU sharing mode
•
Time slicing
•
Multi-Instance GPU
Components of VMware Deep Learning VM 1.1
This version of the deep learning virtual machine image contains the following software:
Software Component 
Category
Software Component
Version
Canonical Ubuntu
22.04
NVIDIA Container Toolkit
1.15.0
Docker Community Engine
26.0.2
Embedded
Miniconda
24.3.0-0 (Python 3.12)
NVIDIA vGPU guest driver
According to the version of the 
NVIDIA vGPU host driver
PyTorch Conda Environment
2.3.0 (Python 3.12)
CUDA Sample
-
PyTorch
-
TensorFlow
-
DCGM Exporter
-
Triton Inference Server
-
Can be pre-installed 
automatically
Deep learning (DL) workload 
from NVIDIA NGC
NVIDIA RAG
-
Resolved Issues
•
Earlier versions of the NVIDIA vGPU driver are not downloaded from the NVIDIA License Portal.
•
The GuestBootstrap status is shown incorrectly in certain cases.
•
NVIDIA vGPU driver download might fail because of network issues.
•
The authorized_keys SSH file, used during the image build process, is available in the ~/.ssh/ directory.
VMware Deep Learning VM 1.0.1
Image Snapshot
VMware Deep Learning VM 1.0.1 is available for use with VMware Cloud Foundation 5.1.1.
Snapshot
Release Date
Compatible VMware Cloud Foundation 
Version
common-container-nv-vgpu-ubuntu-2204-v 
20240419
06 MAY 2024
VMware Cloud Foundation 5.1.1
What's New
•
The versions of the NVIDIA Container Toolkit and Docker Community Engine are updated.
VMware by Broadcom
20


---

VMware Private AI Foundation with NVIDIA 9.1
•
The description of the OVF properties shown when deploying a deep learning VM by using the OVF deployment 
wizard is improved.
•
The download URL format for vGPU guest drivers for disconnected environments now supports directory index listings, 
as generated by Web servers, such as NGINX or Apache HTTP Server.
•
A link to documentation of the VMware deep learning VM appears as a "message of the day" in the Ubuntu operating 
system.
Supported NVIDIA GPU Devices
VMware Deep Learning VM 1.0.1 supports the following GPUs on your ESXi hosts:
NVIDIA Component
Supported Option
NVIDIA GPUs
•
NVIDIA A100
•
NVIDIA L40S
•
NVIDIA H100
GPU sharing mode
•
Time slicing
•
Multi-Instance GPU
Components of VMware Deep Learning VM 1.0.1
This version of the deep learning virtual machine image contains the following software:
Software Component 
Category
Software Component
Version
Canonical Ubuntu
22.04
NVIDIA Container Toolkit
1.15.0
Embedded
Docker Community Engine
26.0.2
NVIDIA vGPU guest driver
According to the version of the 
NVIDIA vGPU host driver
CUDA Sample
-
PyTorch
-
TensorFlow
-
DCGM Exporter
-
Triton Inference Server
-
Can be pre-installed 
automatically
Deep learning (DL) workload 
from NVIDIA NGC
NVIDIA RAG
-
Resolved Issues
•
Unable to log in to a Docker private container registry if the registry password set in the OVF properties of the deep 
learning VM contains special characters, such as & < > " ' .
•
The OVF properties for a secondary container registry are not processed.
•
Running apt update fails because of errors and security warnings.
•
The execution status of the get-vgpu-driver.sh script, run at VM startup, is not reflected in the
guestinfo.vmservice.bootstrap.condition setting of VM Tools.
VMware by Broadcom
21


---

VMware Private AI Foundation with NVIDIA 9.1
VMware Deep Learning VM 1.0
Image Snapshot
VMware Deep Learning VM 1.0 is available for use with VMware Cloud Foundation 5.1.1.
Snapshot
Release Date
Compatible VMware Cloud Foundation 
Version
common-container-nv-vgpu-ubuntu-2204-v 
20240217
26 MAR 2024
VMware Cloud Foundation 5.1.1
Supported NVIDIA GPU Devices
VMware Deep Learning VM 1.0 supports the following GPUs on your ESXi hosts:
NVIDIA Component
Supported Option
NVIDIA GPUs
•
NVIDIA A100
•
NVIDIA L40S
•
NVIDIA H100
GPU sharing mode
•
Time slicing
•
Multi-Instance GPU
Components of VMware Deep Learning VM 1.0
The initial version of the deep learning virtual machine image contains the following software:
Software Component 
Category
Software Component
Version
Canonical Ubuntu
22.04
NVIDIA Container Toolkit
1.13.5
Embedded
Docker Community Engine
25.03
NVIDIA vGPU guest driver
According to the version of the 
NVIDIA vGPU host driver
CUDA Sample
-
PyTorch
-
TensorFlow
-
DCGM Exporter
-
Triton Inference Server
-
Can be pre-installed 
automatically
Deep learning (DL) workload 
from NVIDIA NGC
NVIDIA RAG
-
Known Issues
Deep learning VM content libraries cannot be created or updated because of an expired signing certificate
The key used to sign all published VMware Deep Learning VM Image releases expired on January 3, 2026. These images 
can no longer be synced to or uploaded to content libraries that are configured to enforce certificate validity, that is, have a 
security policy applied.
See Broadcom knowledge base article 426373.
Fixed in VMware Deep Learning VM 9.1.
License Information
VMware Deep Learning VM releases are available under a VMware Private AI Foundation with NVIDIA license. See
VMware Private AI Foundation with NVIDIA Guide.
VMware by Broadcom
22


---

VMware Private AI Foundation with NVIDIA 9.1
Documentation
Examine the VMware Private AI Foundation with NVIDIA Guide for an overview and how-to instructions for running deep 
learning VMs in a VMware Cloud Foundation environment.
VMware Private AI Services Release Notes
This document contains the following sections
•
Introduction
•
Overview of Private AI Services
•
Private AI Services Releases
•
Installation and Upgrade
•
License Information
•
Private AI Services 3.0
– Release Information
– What's New
– Supported VKS Versions
– Supported NVIDIA GPU Operator Versions
– Supported PostgreSQL Versions
– Supported Inference Engine Versions
– Completion Model Endpoint Evaluation and Benchmark Results
– Supported File Types and Data Sources
– Behavior Changes and Upgrade Notes
– Deprecation Notes
– Known Issues
•
Private AI Services 2.1
– Release Information
– What's New
– Supported VKS Versions
– Supported NVIDIA GPU Operator Versions
– Supported PostgreSQL Versions
– Supported Inference Engine Versions
– Supported File Types and Data Sources
– Caveats and Limitations
– Resolved Issues
– Known Issues
•
Private AI Services 2.1.2
– Release Information
– Resolved Issues
•
Private AI Services 2.0.89
– Release Information
– What's New
– Supported VKS Versions
– Supported GPU Operator Versions
– Supported PostgreSQL Versions
– Supported Inference Server Versions
– Supported File Types and Data Sources
– Known Issues
•
Private AI Services 2.0.51
– Release Information
– Supported VKS Versions
– Supported NVIDIA GPU Operator Versions
VMware by Broadcom
23


---

VMware Private AI Foundation with NVIDIA 9.1
– Supported PostgreSQL Versions
– Supported Inference Server Versions
– Supported File Types and Data Sources
•
Known Issues Across Private AI Services Versions
•
Documentation
Introduction
VMware Private AI Services provides features and capabilities to assist you with the deployment of AI workloads.
VMware Private AI Services 3.0 | 03 SEP 2026
VMware Private AI Services 2.1 | 20 MAR 2026, 12 MAY 2026
VMware Private AI Services 2.0.89 | 11 SEP 2025
Check for additions and updates to these release notes.
Overview of Private AI Services
•
Model Runtime and API Gateway
Your IT organization can now run models as a service on our model runtime, which provides initial support for vLLM, 
Infinity, and llama.cpp to run your inference and embedding models. These models run behind an ML API Gateway, 
so your users or AI applications can interact directly with the models using API, while your IT team can transparently 
update model versions and horizontally scale up and down based on demand, without impacting the end user's 
workloads.
•
Data Indexing and Retrieval
You can connect unstructured data in Word documents, PDF files, and other formats, to a vector database. You can 
have this data automatically parsed, embeddings added to the vector database, and data refreshed on a regular 
cadence.
•
MCP Tool Gallery.
Available in Private AI Services 2.1 and later.
You can extend the capabilities of your agents by using MCP tools available on one or more MCP servers. Data 
Indexing and Retrieval  is integrated in Private AI Services as an MCP tool, allowing agents to decide whether to 
retrieve content from a knowledge base and what search term to use.
•
Agent Builder
Application developers can easily compose a model with specific prompting and instructions, and knowledge bases 
created and maintained by Data Indexing and Retrieval. In a user-friendly UI, developers see what is available to them 
to compose and can then create an agent. They can easily iterate with that agent within an in-app playground where 
they can see results and experiment with different models and prompt instructions to see what works best for their use 
case. They can save their agent and use it as the back end for their AI-powered applications.
•
Observability.
Available in Private AI Services 2.1 and later.
You can perform monitoring and diagnostics of models, used in agents, to track the health, quality, and behavior of AI 
applications in observability platforms like Grafana.
Private AI Services Releases
Private AI Services is installed as a package, separately from the VCF and VMware Private AI Foundation with NVIDIA 
core functionality. A Private AI Services version is available at the release date of VCF. After VCF is released, you 
can install or update to newer versions of Private AI Services without the need to update VCF and VMware Private AI 
Foundation with NVIDIA.
Starting with Private AI Services 3.0, you can also install the package from the software depot of your VCF environment.
Installation and Upgrade
You can download the YAML file for installing the Private AI Services on a GPU-accelerated Supervisor instance from the
Private AI Services download area of the Broadcom Support Portal. Starting with Private AI Services 3.0, you can also 
download the installation YAML file from the VMware Cloud Foundation download area.
VMware by Broadcom
24


---

VMware Private AI Foundation with NVIDIA 9.1
For instructions on installing the Private AI Services service and making it available in VCF Automation organizations, see
Install Private AI Services on the Supervisor of the GPU-Accelerated Workload Domain and Activate Private AI Services 
in an Organization Namespace.
For information on updating the Private AI Services service in the Supervisor instance assigned to the VCF Automation 
organization, see Upgrade a Supervisor Service to a Newer Version.
License Information
Private AI Services releases are available under a VMware Private AI Foundation with NVIDIA license. See VMware 
Private AI Foundation with NVIDIA Guide.
Private AI Services 3.0
Release Information
Release Element
Description
Release date
03 SEP 2026
Compatible VCF version
VCF 9.1.x
What's New
Sharing models across namespaces
You can host a common set of models in a central location and make those shared models accessible from different 
Private AI Services instances and namespaces. This capability reduces complexity and lets you preserve scarce 
hardware resources.
Cloud LLM consumption with token tracking
You can use third-party cloud large language models (LLMs) in Private AI Services for consumption and agent 
development, with token usage tracking.
Private AI Services 3.0 now natively supports Google Gemini, Google Cloud Gemini Enterprise Agent Platform (formerly 
Vertex AI), and OpenAI-compatible inference services. 
Observability and monitoring enhancements
Private AI Services 3.0 expands observability with live model and agent metrics dashboards, example Grafana 
dashboards, real-time backend health monitoring for model availability, full LLM interaction tracing, and new metrics for 
agents, knowledge bases, and Model Context Protocol (MCP) servers.
Data plane scalability and flexibility
You can scale data-plane ingress independently and inject a custom model gateway. You can enable or disable Agent 
Builder and Data Indexing and Retrieval independently.
Faster time to value with vSphere and vSphere Kubernetes Service
You can enable Private AI Services and model deployment through the vSphere Local Consumption Interface. Private AI 
Services 3.0 also supports deployment with multiple authentication providers and with static authentication.
Bring your own certificates for ingress TLS termination
You can bring your own certificates for ingress TLS termination.
Simplified support functions when using the pais VCF Consumption CLI plug-in
The pais plug-in for the VCF Consumption CLI offers the following simplified support functions: The CLI can retrieve a 
kubeconfig file for the VKS guest cluster, letting support staff use kubectl to inspect pods, events, and logs without cluster 
administrator access on the supervisor. Support-bundle collection using the CLI is also improved.
•
The CLI can retrieve a kubeconfig file for the VKS guest cluster. In this way, support personnel can inspect pods, 
events, and logs inside the VKS cluster without having to ask the VI administrator to access the Supervisor cluster.
VMware by Broadcom
25


---

VMware Private AI Foundation with NVIDIA 9.1
•
Improved support bundle collection experience by using CLI.
Configurable TLS validation for external connections
You can configure TLS validation when connecting Private AI Services to an OIDC server.
OpenAI compatibility API improvements
•
Model listing responses now include a status field for each model.
•
Embedding requests can now specify encoding_format as float or base64.
•
Extra and non-OpenAI properties on chat-completion calls to the LLM are accepted and propagated.
•
Throughput when streaming LLM responses is improved.
Data Indexing and Retrieval improvements
•
Search results now include the underlying node ID with each citation.
•
Configuration of TLS validation when connecting to data sources.
•
Knowledge bases and indexes can be cloned.
•
More metrics are exposed during document indexing.
•
Throughput and robustness when embedding large documents are improved.
•
Robustness when cancelling and re-starting indexing operations is improved.
Agent Builder improvements
•
Knowledge base tools linked to an agent can be configured with a force_static_execution flag. As a result, 
agents using LLMs that were not trained for MCP or tool use to still include context from a Private AI Services 
knowledge base index.
•
Robustness of knowledge base tool calling for LLMs that did not understand the pre-configured top-k or similarity-cutoff 
parameters is improved.
•
Agent responses when the maximum iteration limit is reached are improved. An agent makes one final best-effort 
attempt to summarize and respond to the user request.
•
OpenTelemetry traces for agent interactions are exposed with better nested spans to reflect dependencies.
•
More metrics are exposed during MCP server interactions and monitoring.
Supported VKS Versions
This version of Private AI Services uses VMware vSphere Kubernetes release (VKr) v1.34 clusters with builtin-
generic-v3.5.0 ClusterClass introduced with VMware vSphere Kubernetes Service (VKS) 3.5.0.
Verify that your VKr content library contains images for Ubuntu 24.04.
Private AI Services requires a VMClass of best-effort-large or an equivalent in terms of resources for the VKS 
control plane nodes, so that all components, including the observability components, come up successfully.
Supported NVIDIA GPU Operator Versions
Private AI Services 3.0.0 provides an NVIDIA GPU Operator package that supports multiple GPU Operator versions and a 
subset of GPU driver versions.
•
GPU Operator versions: v25.10.1 (default) and v26.3.1
•
GPU Operator version can be configured by using the
PAISConfiguration.spec.nvidiaConfig.gpuOperatorVersion property. See the API documentation.
•
GPU driver versions:
– Data center (OSS driver type): 580.105.8 and 580.126.20
– vGPU (NVAIE driver type): 580.105.8
•
GPU driver version can be configured by using the
PAISConfiguration.spec.nvidiaConfig.driver.version property. See the API documentation.
•
For all driver versions supported by the upstream GPU Operator, see the NVIDIA GPU Operator v25.10.x 
documentation and NVIDIA GPU Operator v26.3.x documentation.
For vGPU usage, the NVIDIA vGPU host driver must be compatible with the GPU Operator deployed driver version. See 
the NVIDIA vGPU software and driver documentation. Evaluate the GPU Operator configuration carefully.
VMware by Broadcom
26


---

VMware Private AI Foundation with NVIDIA 9.1
Private AI Services 3.0.0 detects and reports host-guest driver compatibility and reports mismatches through the
ModelEndpoint custom resource status field.
The Private AI Services Artifact Mirroring Tool (vcf pais amt pull and vcf pais amt push) mirrors the 
GPU Operator and driver versions listed above as part of the standard disconnected installation flow. This gives air-
gapped deployments operational flexibility through the choice of operator and driver versions after disconnecting your 
environment, without having to mirror them manually.
Supported PostgreSQL Versions
This version of Private AI Services supports PostgreSQL 16.8 with pgvector extension 0.8.0.
Supported Inference Engine Versions
This version of Private AI Services is delivered with the following inference engines:
•
vLLM v0.20.0 (completions and embeddings)
•
llama.cpp ver. b9309 (completions and embeddings)
Note:
The llama.cpp image delivered with Private AI Services is CPU-only. If you want to use llama.cpp with NVIDIA 
GPU, you must upload the llama.cpp CUDA image to an OCI registry and specify its location when creating the 
model endpoint.
•
Infinity 0.0.76 (embeddings only)
You can also override the version of the inference engine by specifying a custom image when creating the model endpoint 
in the UI or using a YAML specification. See Deploy Completion or Embedding Model Endpoints.
Completion Model Endpoint Evaluation and Benchmark Results
The following evaluation provides information on the hardware, inference engines, and specific configurations that are 
verified with specific machine learning models for running certain completion models as model endpoints.
Evaluated Configuration
Evaluation Element
Value
Inference engine
•
vLLM 0.20.0
•
llama.cpp b9309
Hardware profile
•
CPU Intel Xeon Skylake
•
NVIDIA A100 80G
Modes
Multiple model architectures and sizes
Load scenarios
OpenAI-compatible completion request
Metrics
•
Total Tokens Per Second (TTS)
•
Output Tokens Per Second (OTPS)
•
Time To First Token (TTFT)
Workload shape
Synchronous fixed payload of 256 prompt tokens and 128 output tokens 
Metrics Definitions
Metric
Definition
Total Tokens Per Second (TTPS)
Combined rate of processed prompt and output tokens per second 
across the system.
Output Tokens Per Second (OTPS)
Average number of output tokens generated per second by the 
server.
Time to First Token (TTFT)
Duration from sending a request until the first streamed output 
token is received.
VMware by Broadcom
27


---

VMware Private AI Foundation with NVIDIA 9.1
Running completion models on CPU
Table 1: Model Endpoint Configuration with CPU
Model
Inference Engine
Quantization and Precision
Hardware
Gemma 3 (1B, 4B)
llama.cpp
Q4_0 (QAT) 4-bit
16 vCPU, 128GB RAM 
GPT-OSS (20B, 120B)
llama.cpp
MXFP4 4-bit
16 vCPU, 128GB RAM 
Nemotron 3 Nano (4B)
llama.cpp
Q4_K_M 4-bit
16 vCPU, 128GB RAM 
Table 2: Performance Metrics for Models on CPU
Model
TTPS
OTPS
TTFT (ms)
Gemma 3 (1B)
205.14
66.83
453.10
Gemma 3 (4B)
61.71
20.10
1699.01
GPT-OSS (20B)
74.07
21.02
2738.46
GPT-OSS (120B)
51.96
14.75
4216.43
Nemotron 3 Nano (4B)
64.72
20.66
2429.22
Running completion models on GPU
Table 3: Model Endpoint Configuration with GPU
Model
Inference Engine
Hardware
inferenceServerCustomization
Gemma 3 (1B)
vLLM
NVIDIA A100 80G
None
Gemma 3 (4B)
vLLM
NVIDIA A100 80G
--language-model-only
Gemma 3 (12B)
vLLM
NVIDIA A100 80G
--max-model-len=32768 --
language-model-only
Gemma 3 (27B)
vLLM
NVIDIA A100 80G
--max-model-len=32768 --max-
num-batched-tokens=8192 --
gpu-memory-utilization=0.90 --
language-model-only
Gemma 4 (26B MoE, 31B)
vLLM
NVIDIA A100 80G
--max-model-len=16384 --max-
num-batched-tokens=8192 --
gpu-memory-utilization=0.90 
--enable-auto-tool-choice --
reasoning-parser=gemma4 --
tool-call-parser=gemma4 --
language-model-only
OpenAI GPT-OSS (20B, 120B)
vLLM
NVIDIA A100 80G
--max-model-len=32768 --max-
num-batched-tokens=8192 --
gpu-memory-utilization=0.90 --
enable-auto-tool-choice --tool-
call-parser=openai
Table 4: Performance Metrics for Models on GPU
Model
TTPS
OTPS
TTFT (ms)
Gemma 3 (1B)
807.09
263.61
97.44
Gemma 3 (4B)
382.05
124.78
109.87
Gemma 3 (12B)
159.02
51.94
230.68
Gemma 3 (27B)
78.30
25.58
292.94
VMware by Broadcom
28


---

VMware Private AI Foundation with NVIDIA 9.1
Model
TTPS
OTPS
TTFT (ms)
Gemma 4 (26B MoE)
372.37
120.09
248.84
Gemma 4 (31B)
70.35
22.69
421.93
OpenAI GPT-OSS (20B)
750.41
212.98
186.83
OpenAI GPT-OSS (120B)
517.85
146.97
314.24
Supported File Types and Data Sources
The Data Retrieval and Indexing component of Private AI Services supports retrieving files from specific types of data 
sources and extracting text content from the specific file types.
Data Retrieval and Indexing Element
Supported Options
File types
•
Available locally or uploaded to a data source
PDF, DOCX, PPTX, HTML, Markdown, CSV, Plaintext
•
Google native formats
Google Docs, Google Sheets, and Google Slides
Microsoft SharePoint
•
Hosted Microsoft SharePoint Enterprise 
16.0.14326.20450
•
Versions that have a compatible API interface and 
use user name and password credentials
Google Drive
Google Drive API v3, using Google service accounts 
for authentication.
Atlassian Confluence
•
Atlassian Confluence 7.19.6, using user name and 
password credentials or Personal Access Tokens 
for authentication
•
Versions offering a compatible API interface
Data source types
S3 Compatible Server
S3 API v4, using Access Key ID/Secret Key 
credentials for authentication
Behavior Changes and Upgrade Notes
•
Metrics collection in Prometheus starts only after the VKS cluster that is managed by Private AI Services is available. 
This behavior is different from Private AI Services 2.1, where the Prometheus server does not wait for the VKS cluster 
to come up.
•
The new vLLM version defaults to CUDA 13.0, which drops support for NVIDIA GPU Driver versions before 580. 
Ensure that your hardware and host drivers are compatible, and select the Private AI Services NVIDIA GPU Operator 
and driver versions appropriately. Model endpoints with only one replica experience downtime during upgrade.
Deprecation Notes
•
Legacy non-chat completions are marked as deprecated in the OpenAI compatibility and Agent Builder APIs.
•
The completion_role field is removed from the agent API. The response role is now always assistant.
•
Invalid values for boolean fields, such as 1 for true, are now rejected more strictly.
Known Issues
You cannot set the default base URL for local accounts when activating Private AI Services by using the UI
To use the default base URL that is based on the IP address assigned to Private AI Services by the Ingress controller, you 
must leave the base URL empty. However, you must always enter a URL in the activation wizard.
Enter any value, for example, https://pais.local/, and after you complete the setup, remove it by using kubectl. 
See Activate Private AI Services 3.0 by Using the UI.
API tokens are not turned on when you active Private AI Services by using the UI
VMware by Broadcom
29


---

VMware Private AI Foundation with NVIDIA 9.1
If you have added local accounts in the wizard for activating Private AI Services, API tokens are not turned on and you 
cannot access the Private AI Services API or use models that are running in another namespace.
Turn on API tokens for authentication type by using kubectl. See Activate Private AI Services 3.0 by Using the UI.
Private AI Services 2.1
Release Information
Release Element
Description
Release date
•
12 MAY 2026 for VCF 9.1
•
20 MAR 2026 for VCF 9.0.2
Compatible VCF version
•
VCF 9.1
•
VCF 9.0.2
What's New
Available for VCF 9.1. Updated Model Endpoint UI
End users can now use an extended set of configuration settings when running model endpoints in the VCF Automation 
UI.
Available for VCF 9.1. UI-Driven Self-Service Enablement
In VCF 9.1, organization administrators can enable and manage Private AI Services within namespaces by using the 
VCF Automation UI. End users have a seamless and intuitive UI experience that supports the end-to-end configuration, 
deployment, and lifecycle management of Private AI Services.
Support for disconnected (air-gapped) environments
Private AI Services 2.1 introduces the artifact mirroring tool (AMT), enabling VI administrators to install and run full private 
AI capabilities, including NVIDIA GPU-powered model endpoints and agents, within secure, air-gapped environments.
Integrated observability and analytics
You can use the observability framework of Private AI Services for health and performance insights across the entire AI 
workload footprint, from inference engines and GPU utilization to knowledge base indexing and agents.
Integrated LLM tracing
You can use OpenTelemetry Collector platforms to trace all interactions between end-users and LLMs, agents, and 
knowledge bases.
Model Context Protocol (MCP) integration
By using MCP, MLOps engineers can connect agents to a wide array of external data sources and tools by using an 
industry-standard interface. This version of Private AI Services introduces a tool gallery for centralized MCP server 
management and tool enablement, and exposes knowledge bases over MCP so that AI application developers can build 
context-aware agents.
Support for CPU-based inference and embeddings
You can use llama.cpp as an inference engine for running GPU and CPU-based LLMs.
Data indexing and retrieval for Google native formats
You can upload Google Docs, Google Sheets, and Google Slides in knowledge bases.
Supported VKS Versions
This version of Private AI Services uses VMware vSphere Kubernetes release (VKr) 1.33 clusters with builtin-generic-
v3.2.0 ClusterClass supported with VMware vSphere Kubernetes Service (VKS) 3.5.0 and later.
Verify that your VKr content library contains images for Ubuntu 24.04.
VMware by Broadcom
30


---

VMware Private AI Foundation with NVIDIA 9.1
Consider upgrading to VKS 3.5.0 and later. Otherwise, the VKS clusters provisioned by Private AI Services for model 
endpoints will remain unreconciled.
Supported NVIDIA GPU Operator Versions
By default, this version of Private AI Services uses NVIDIA GPU Operator 25.10.1. For supported driver versions, see the
NVIDIA GPU Operator documentation.
If you are upgrading Private AI Services in your VCF environment, you might need to evaluate upgrading the NVIDIA 
software for ESX host to ensure that the right vGPU drivers are installed on the ESX host and the guest VM. By default, 
NVIDIA GPU Operator 25.10.1 uses the GPU driver v580.x series on the guest OS side which might be incompatible 
with earlier NVIDIA software on the ESX host. For compatibility information, see the NVIDIA vGPU software driver 
documentation. If you provision new model endpoints in Private AI Services 2.1.0, the Model Runtime will report driver 
version mismatches.
Supported PostgreSQL Versions
This version of Private AI Services supports PostgreSQL 16.8 with pgvector extension 0.8.0.
Supported Inference Engine Versions
This version of Private AI Services is delivered with the following inference engines:
•
vLLM v0.11.2 (completions and embeddings)
•
llama.cpp ver. b7739 (completions and embeddings)
Note:
The llama.cpp image delivered with Private AI Services is CPU-only. If you want to use llama.cpp with NVIDIA 
GPU, you must upload the llama.cpp CUDA image to an OCI registry and specify its location when creating the 
model endpoint.
•
Infinity 0.0.76 (embeddings only)
You can also override the version of the inference engine by specifying a custom image when creating a model endpoint 
in the UI or using YAML. See Deploy Completion or Embedding Model Endpoints.
Supported File Types and Data Sources
The Data Retrieval and Indexing component of Private AI Services supports retrieving files from specific types of data 
sources and extracting text content from the specific file types.
Data Retrieval and Indexing Element
Supported Options
File types
•
Available locally or uploaded to a data source
PDF, DOCX, PPTX, HTML, Markdown, CSV, Plaintext
•
Google native formats
Google Docs, Google Sheets, and Google Slides
Microsoft SharePoint
•
Hosted Microsoft SharePoint Enterprise 
16.0.14326.20450
•
Versions that have a compatible API interface and 
use user name and password credentials
Google Drive
Google Drive API v3, using Google service accounts 
for authentication.
Atlassian Confluence
•
Atlassian Confluence 7.19.6, using user name and 
password credentials or Personal Access Tokens 
for authentication
•
Versions offering a compatible API interface
Data source types
S3 Compatible Server
S3 API v4, using Access Key ID/Secret Key 
credentials for authentication
VMware by Broadcom
31


---

VMware Private AI Foundation with NVIDIA 9.1
Caveats and Limitations
You can run up to 15 model endpoint replicas per namespace
Each model endpoint replica consumes a /24 CIDR block in your network address space, and the default network 
assignments for the VKS cluster used by Private AI Services is /20. As a result, you will run out of network address space 
after 15 model endpoints. To run more endpoints, set the vks.candidatePodCIDRs property in the configuration of the 
Private AI Services Supervisor Service to allocate a larger network range.
1. Log in to the vCenter instance for the management domain at https://<vcenter_fqdn>/ui as
administrator@vsphere.local.
2. In the vSphere Client side panel, click Supervisor Management and click the Services tab.
3. In the Private AI Services card, select Actions > Manage Service.
4. In the manage wizard, select the Supervisor instance and click Next.
5. On the Review page, in the YAML Service Config text area, add the following YAML code and click Finish.
vks: 
  candidatePodCIDRs:  
  - <CIDR-1> 
  - <CIDR-2> 
  - <CIDR-N> 
Additional configuration is required for agents with models that do not support tool calling.
To use a RAG agent with data indexing and retrieval and with a model that does not support MCP tool calling or an agent 
for non-chat completions (legacy behavior), set the x-pais-force-static-tool-execution metadata header to
true in the agent configuration. See Pre-Load Tool Results in the Context of an Agent.
In a disconnected environment, the NVIDIA GPU Operator can be deployed only from an OCI registry without 
authentication.
In the VKr version in VCF 9.0.2, the bundled kapp-controller v0.58.1 contains a known upstream issue where Helm 
v3.18.0 fails to pull charts from OCI registries that require authentication.
Resolved Issues
Private AI Services internally provisions a VKS Cluster that uses IP addresses within CIDR blocks  
192.168.144.0/20 and 10.96.0.0/20. If those ranges are used by other elements of your virtual infrastructure, for 
example, Load Balancers, Private AI Services fails to start
Private AI Services internally uses mutual TLS (mTLS) between the microservices running in a user’s namespace. 
These certificates expire 3 months after the initial installation.
Known Issues
In Private AI Services version 2.1 with GPU Operator v25.10.1, ModelRuntime pods using GPUs might fail to start.
Kubernetes pods using NVIDIA GPUs fail to start with the following error that appears in the pod events.
failed to create containerd container: CDI device injection failed: unresolvable CDI devices
Customize the GPU Operator Helm chart values to deactive the Container Device Interface (CDI) by performing the 
following steps.
1. Get an API token for access to the namespace in VCF Automation where Private AI Services is available.
a. Click your account in the top right corner and select User Settings > My Account.
b. On the API Tokens tab, create a token and save it on your machine.
2. On a machine running kubectl and the VCF Consumption CLI, create a ConfigMap with the following YAML code in a 
file, for example, called helm-values.yaml .
See Installing and Using VCF CLI in the VCF documentation.
apiVersion: v1
kind: ConfigMap
metadata: 
  name: helm-values
data: 
VMware by Broadcom
32


---

VMware Private AI Foundation with NVIDIA 9.1
  values.yaml: | 
    cdi: 
      enabled: false 
    toolkit: 
      version: "v1.18.2" 
      env: 
        - name: NVIDIA_CONTAINER_RUNTIME_MODE 
          value: "legacy" 
        - name: CDI_ENABLED 
          value: "false"
3. Create a context for the VCF Automation namespace where the Private AI Services instance is available.
vcf context create <vcf_automation_context_name> --endpoint <vcf_automation_fqdn_or_ip> --
ca-certificate <vcf_automation_certificate>.pem --api-token <vcf_automation_api_token> --
type cci
4. Log in to the VCF Automation namespace where Private AI Services is running by using the VCF Consumption CLI.
vcf context use <vcf_automation_context_name>:<pais_namespace_name>:<project_name>
5. Save the ConfigMap in the namespace by running the following command.
kubectl apply -f helm-values.yaml
6. Prepare YAML code to modify the PAISConfiguration CR in the namespace to reference the customized Helm 
chart, for example, in a paisconfiguration-update.yaml file.
spec: 
  nvidiaConfig: 
    gpuOperatorOverridesRef: 
      name: helm-values
7. Apply the update to the PAISConfiguration CR.
kubectl patch PAISConfiguration default --patch-file paisconfiguration-update.yaml --type='merge'
After upgrading to Private AI Services 2.1, a running model endpoint might fail to re-deploy because of 
insufficient memory.
Because of increased VRAM requirements in the vLLM engine version in Private AI Services 2.1, some model endpoints 
might fail to reconcile. The logs for the main container of the model endpoint pod that you get by running the following 
command:
kubectl logs -n <modelendpoint namespace> <modelendpoint pod> -c main
contain the following error:
RuntimeError: CUDA out of memory occurred with warming up sample with <number> dummy 
requests.
1. Deploy a new model endpoint with the same routing name but with CLI engine arguments in the
spec.inferenceServerCustomization section of the model endpoint YAML code that are adjusted to 
accommodate the new memory overhead, for example, by including --gpu_memory_utilization. For information 
about the CLI arguments, see the vLLM documentation.
– If you are using the model endpoint UI, on the Model Specification page, toggle Advanced Customization and, in 
the Flags or Arguments for Engine Customization section, include --gpu_memory_utilization=0.8.
For information on how to deploy a model endpoint by using the UI, see Deploy a Completion or Embedding Model 
Endpoint.
– If you are using YAML files and the kubectl command, add the following code to
spec.inferenceServerCustomization.cliArgs section of the YAML manifest for the model endpoint.
spec: 
  inferenceServerCustomization: 
    cliArgs: 
    - "--gpu-memory-utilization=0.8"
For information on how to deploy a model endpoint by using kubectl, see Deploy a Completion or Embedding 
Model Endpoint by using kubectl.
VMware by Broadcom
33


---

VMware Private AI Foundation with NVIDIA 9.1
2. Decommission the original model endpoint.
LLM traces from Private AI Services are not visible in the OpenTelemtry Collector
If the configuration of the target OpenTelemetry Collector in the spec.observability.llmTraces property of the 
Private AI Services activation YAML file is incorrect, an error is not reported. Distributed tracing uses only OpenTelemetry
traceparent propagation solely between Private AI Services components.
You can only verify that the configuration is successful by seeing instrument_code_for_tracing spans in the 
OpenTelemetry Collector.
If the instrument_code_for_tracing spans are not visible in the OpenTelemetry Collector, check the logs after the
setup_tracing lines for the Private AI Services activation by running the following command in the context of the VCF 
Automation namespace.
kubectl logs -l "pais.vmware.com/component in (api,rex-worker,rex-mcp-server)" -c main --prefix --tail=-1 | 
 grep setup_tracing -A 5
Expected downtime during upgrade from version 2.0.x to 2.1 of Private AI Services
The upgrade process deletes and re-creates the VKS cluster hosting model endpoints causing downtime while nodes are 
being re-created and models downloaded again.
Workaround: None
Private AI Services 2.1.2
Private AI Services 2.1.2 is an express patch release that addresses the following known issues.
Release Information
Release Element
Description
Release date
17 AUG 2026
Compatible VCF version
•
VCF 9.1
•
VCF 9.0.2
Resolved Issues
Incorrect package download address in the Private AI Services 2.1.1 YAML file
The 2.1.1 release contained an incorrect URL in the imgPkgUrl field of the downloadable YAML artifact on the Broadcom 
Support Portal. The 2.1.2 release YAML file on the on the Broadcom Support Portal provides a correct URL that points to 
the Private AI Services repository.
Potential collision on port 5000
On a VMware Kubernetes Services (VKS) node, the ModelEndpoint NGINX sidecar hardcoded hostPort 5000 could 
collide with the local registry mirror.
Renewal of intermediate CA certificate
When the issuer CA certificate is rotated and the owner Private AI Services certificate is reissued, you must manually re-
sync the ca.crt value of the owner certificate.
Reasoning model timeout
When a request is submitted to the agent, the reasoning LLM might take a long time to respond if the model runs on CPU 
and MCP tools are invoked. As a result, the agent API and UI might time out without considering the received keepalive 
messages. 
Updated components and fixes
The 2.1.2 release updates the Photon OS version and includes open-source software (OSS) package updates and 
security fixes.
Improved log collection in support bundle
VMware by Broadcom
34


---

VMware Private AI Foundation with NVIDIA 9.1
The 2.1.2 release improves support bundle log collection.
Private AI Services 2.0.89
Release Information
Release Element
Description
Release date
11 SEP 2025
Compatible VCF version
9.0
What's New
Improved Supervisor Service Security and Compatibility
The Private AI Services deliverables are now signed with Broadcom certificates.
Generating Support Bundles for Private AI Services
You can gather diagnostic information for Broadcom technical support about Private AI Services operation. For 
information on how to generate and export a support bundle, see Broadcom knowledge base article 408731.
Supported VKS Versions
This version of Private AI Services uses VMware vSphere Kubernetes release (VKr) 1.32 clusters with VMware vSphere 
Kubernetes Service (VKS) ClusterClass builtin-generic-v3.2.0.
Supported GPU Operator Versions
By default, this version of Private AI Services uses NVIDIA GPU Operator 24.9.0. For supported driver versions, see the
NVIDIA GPU Operator documentation.
Supported PostgreSQL Versions
This version of Private AI Services supports PostgreSQL 16.8 with pgvector extension 0.8.0.
Supported Inference Server Versions
This version of Private AI Services supports the vLLM inference server 0.6.5 for generating completions and the Infinity 
inference server 0.0.43 for generating embeddings. Both servers are installed with supported default configurations.
Supported File Types and Data Sources
The Data Retrieval and Indexing component of Private AI Services supports retrieving files from specific types of data 
sources and extracting text content from the specific file types.
Data Retrieval and Indexing Element
Supported Options
File types
•
PDF
•
DOCX
•
PPTX
•
HTML
•
Markdown
•
CSV
•
Plaintext
Data source types
Microsoft SharePoint
•
Hosted Microsoft SharePoint Enterprise 
16.0.14326.20450
•
Versions that have a compatible API interface and 
use user name and password credentials
VMware by Broadcom
35


---

VMware Private AI Foundation with NVIDIA 9.1
Data Retrieval and Indexing Element
Supported Options
Google Drive
Google Drive API v3, using Google service accounts 
for authentication.
Atlassian Confluence
•
Atlassian Confluence 7.19.6, using user name and 
password credentials or Personal Access Tokens 
for authentication
•
Versions offering a compatible API interface
S3 Compatible Server
S3 API v4, using Access Key ID/Secret Key 
credentials for authentication
Known Issues
Expected downtime during upgrade from version 2.0.51 to 2.0.89 of Private AI Services
Model endpoints with single replicas, which is the default configuration, cannot maintain availability during cluster node 
upgrades, which replace nodes one at a time so that you do not have to over-provision GPUs.
Workaround: None
Private AI Services internally provisions a VKS Cluster that uses IP addresses within CIDR blocks  
192.168.144.0/20 and 10.96.0.0/20. If those ranges are used by other elements of your virtual infrastructure, for 
example, Load Balancers, Private AI Services fails to start
Workaround: None.
Private AI Services internally uses mutual TLS (mTLS) between the microservices running in a user’s namespace. 
These certificates expire 3 months after the initial installation.
Workaround: Delete your PAISConfiguration and ModelEndpoints, and re-apply them but keep your PostgreSQL database 
so that you do not lose your data. This operation introduces a short outage.
Private AI Services 2.0.51
Release Information
Release Element
Description
Release date
17 JUN 2025
Compatible VCF version
9.0
Supported VKS Versions
This version of Private AI Services uses VMware vSphere Kubernetes release (VKr) 1.32 clusters with VMware vSphere 
Kubernetes Service (VKS) ClusterClass builtin-generic-v3.2.0.
Supported NVIDIA GPU Operator Versions
By default, this version of Private AI Services uses NVIDIA GPU Operator 24.9.0. For supported driver versions, see the
NVIDIA GPU Operator documentation.
Supported PostgreSQL Versions
This version of Private AI Services supports PostgreSQL 16.8 with pgvector extension 0.8.0.
Supported Inference Server Versions
This version of Private AI Services supports the vLLM inference server 0.6.5 for generating completions and the Infinity 
inference server 0.0.43 for generating embeddings. Both servers are installed with supported default configurations.
VMware by Broadcom
36


---

VMware Private AI Foundation with NVIDIA 9.1
Supported File Types and Data Sources
The Data Retrieval and Indexing component of Private AI Services supports retrieving files from specific types of data 
sources and extracting text content from the specific file types.
Data Retrieval and Indexing Element
Supported Options
File types
•
PDF
•
DOCX
•
PPTX
•
HTML
•
Markdown
•
CSV
•
Plaintext
Microsoft SharePoint
•
Hosted Microsoft SharePoint Enterprise 
16.0.14326.20450
•
Versions that have a compatible API interface and 
use user name and password credentials
Google Drive
Google Drive API v3, using Google service accounts 
for authentication.
Atlassian Confluence
•
Atlassian Confluence 7.19.6, using user name and 
password credentials or Personal Access Tokens 
for authentication
•
Versions offering a compatible API interface
Data source types
S3 Compatible Server
S3 API v4, using Access Key ID/Secret Key 
credentials for authentication
Known Issues Across Private AI Services Versions
Data Retrieval and Indexing reports a General Crawling Error instead of a Credentials Error when indexing a 
knowledge base if the provided credentials expire after the initial configuration of the data source
Data source credentials can be validated by the Private AI Services API and UI upon data source creation. If the provided 
credentials are invalid or do not grant access to the data source backend, a relevant error message appears.
However, if credentials expire or if access to the data source backend is revoked after data source creation, the indexing 
process fails with a General Crawling Error instead of reporting an error about credentials becoming invalid.
Workaround: Validate the credentials by using the data source creation workflow. If the data source credentials have 
become invalid, update the data source with the new ones.
Data Retrieval and Indexing does not validate if data source credentials for Google Drive grant permissions to 
read a folder
The Google Drive API does not explicitly reject API requests for listing files in a folder if the requesting client does not 
have permissions on the folder. Instead, the API grants access but returns an empty list.
This means that the validation of data source credentials passes successfully if the credentials are valid but do not grant 
access to any files.
As a side effect, if the permissions granted to credentials used by a knowledge base are changed after a knowledge base 
indexing, any subsequent indexing no-longer sees the files from the earlier indexing, and the indexer assumes that files 
are removed from the folder, and it subsequently removes documents from the knowledge base.
Workaround: Update the credentials used by the data source or ensure that the credentials in use grant access to the 
expected folders. If an indexing is started and documents get removed, start another indexing after fixing the credentials 
to ensure that all files are re-added to the knowledge base.
Documentation
Examine the VMware Private AI Foundation with NVIDIA Guide for an overview and how-to instructions on running 
models, creating knowledge bases, and creating agents.
VMware by Broadcom
37


---

VMware Private AI Foundation with NVIDIA 9.1
What is VMware Private AI Foundation with NVIDIA?
As a multi-component solution, VMware Private AI Foundation with NVIDIA builds on virtual infrastructure management 
and cloud management from VCF to run generative AI workloads on systems with NVIDIA GPU devices.
VMware Private AI Foundation with NVIDIA provides a platform for provisioning AI workloads on ESX hosts with NVIDIA 
GPUs. In addition, running AI workloads based on NVIDIA GPU Cloud (NGC) containers is specifically validated by 
VMware.
VMware Private AI Foundation with NVIDIA supports two use cases:
Development use case
VI administrators and organization administrators, and DevOps engineers can provision AI workloads, 
including Retrieval-Augmented Generation (RAG), in the form of deep learning virtual machines. Data 
scientists and AI application developers can use these deep learning virtual machines for AI development. 
See About Deep Learning VM Images in VMware Private AI Foundation with NVIDIA and Delivering 
Generative AI Applications by Using Private AI Services.
Production use case
VI administrators and organization administrators can provide DevOps engineers with a VMware Private AI 
Foundation with NVIDIA environment for provisioning production-ready AI workloads on Kubernetes cluster 
created by vSphere Kubernetes Service (VKS). See Deploying AI Workloads on VKS Clusters in VMware 
Private AI Foundation with NVIDIA and Delivering Generative AI Applications by Using Private AI Services.
For information about the components that are part of the VMware Private AI Foundation with NVIDIA solution and their 
architecture on top of VCF, see System Architecture of VMware Private AI Foundation with NVIDIA.
VMware Private AI Foundation with NVIDIA Documentation
The VMware Private AI Foundation with NVIDIA documentation provides an overview of the components of VMware 
Private AI Foundation with NVIDIA and high-level workflows for development and production use cases.
Intended Audience
The information in the VMware Private AI Foundation with NVIDIA documentation is intended for data center VI 
administrators, organization administrators, data scientists, MLOps engineers, and DevOps engineers who are familiar 
with:
•
VI administrators
– Concepts of virtualization and software-defined data centers (SDDCs)
– Hardware components such as top-of-rack (ToR) switches, inter-rack switches, servers with direct attached storage, 
cables, and power supplies
– Methods for setting up NVIDIA GPUs on servers in a data center
– Methods for setting up networking for AI workloads.
– Using VMware vSphere
® to work with virtual machines.
– Using vSphere Supervisor to configure and assign vSphere resources to namespaces in VCF Automation.
As a VI administrator, see the following information:
– Preparing VCF for Private AI Workload Deployment
– Deploying a Deep Learning VM in VMware Private AI Foundation with NVIDIA
– Monitoring VMware Private AI Foundation with NVIDIA
•
Data scientists, AI application developers, or MLOps engineers
– Containers, including Docker, Helm charts, and Open Container Initiative (OCI) registries, such as Harbor
See the following information:
– Deploying a Deep Learning VM in VMware Private AI Foundation with NVIDIA
– Delivering Generative AI Applications by Using Private AI Services
•
DevOps engineers
– Provisioning virtual machines in vSphere using the Kubernetes API.
VMware by Broadcom
38


---

VMware Private AI Foundation with NVIDIA 9.1
– Containers, including Docker, Helm charts, and OCI registries
– Working with vSphere Supervisor for provisioning VMs and vSphere Kubernetes Service (VKS) clusters.
As a DevOps engineer, see the following information:
– Deploying AI Workloads on VKS Clusters in VMware Private AI Foundation with NVIDIA
– Deploy a Deep Learning VM by Using the kubectl Command in VMware Private AI Foundation with NVIDIA
VMware Software Components According to Job Function
The functionality of the VMware Private AI Foundation with NVIDIA solution is available across several software 
components according to your role in your organization.
Target User Role
Software Category
Supported Software Versions
VI administrators and organization 
administrators
Components that are deployed in VCF
See the following information:
•
VMware Components in VMware 
Private AI Foundation with NVIDIA
•
VMware Private AI Foundation with 
NVIDIA 9.1.x Release Notes
Data scientists, AI application developers, 
MLOps engineers
•
Deep learning VM components
•
Model endpoints and agents in Private 
AI Services for MLOps engineers and 
data scientists
•
Private AI Services UI and API for AI 
application developers
See the following information:
•
VMware Deep Learning VM Image 
Release Notes.
•
VMware Private AI Services Release 
Notes.
DevOps engineers
vSphere Kubernetes releases (VKr)
See VMware VKrs Release Notes.
Sizing Your VMware Private AI Foundation with NVIDIA Environment
See Sizing Guide for VMware Private AI Foundation with NVIDIA.
Private AI Design
For guidelines on designing an environment for running private AI workloads on top of VMware Private AI Foundation with 
NVIDIA, see the Design section of the VCF documentation.
Related VMware Documentation
The VMware Private AI Foundation with NVIDIA solution includes a stack of VMware software components that are part of
VCF. The documentation for those software components is as follows:
•
VMware Cloud Foundation Documentation
•
VMware Data Services Manager Documentation
VMware by Broadcom
39


---

VMware Private AI Foundation with NVIDIA 9.1
Preparing VCF for Private AI Workload Deployment
As a VI administrator, you must deploy specific software and configure the target workload domains so that data 
scientists, DevOps engineers, and MLOps engineers can deploy AI workloads on top of VMware Private AI Foundation 
with NVIDIA.
VMware Components in VMware Private AI Foundation with NVIDIA
The VMware Private AI Foundation with NVIDIA 9.1 solution runs on top of VCF 9.1. The functionality of the solution is 
available across several VCF components - vCenter, ESX, VCF Operations, and VCF Automation. For more information 
about VCF, see the VCF documentation.
For RAG applications, you can use VMware Data Services Manager for running PostgreSQL databases with 
pgvector extension. For information about VMware Data Services Manager, see the VMware Data Services Manager 
documentation.
For information about the VMware Private AI Foundation with NVIDIA architecture and components, see System 
Architecture of VMware Private AI Foundation with NVIDIA.
Sizing Your VMware Private AI Foundation with NVIDIA Environment
See Sizing Guide for VMware Private AI Foundation with NVIDIA.
Running AI Workloads on vGPU or GPU Passthrough
Starting with VCF 9.1, you can run AI workloads on one or more vGPUs or directly on a GPU device using passthrough 
technology.
Using vGPUs has the following advantages:
•
You can share GPU capacity among several workloads that do not need the power of a full GPU device.
•
A limited number of GPU devices can be shared among multiple teams.
Using vGPUs has the following considerations:
•
You need an NVIDIA AI Enterprise license.
•
You run DL workloads from NVIDIA from the production branch which requires an NVIDIA NGC API key. For 
example, the image URL of the Pytorch container on the May 2025 production branch could be nvcr.io/nvidia/pytorch-
pb25h1:25.03.03-py3.
Using GPU passthrough has the following advantages and considerations:
•
A workload is allocated the full capacity of the GPU device on the underlying server which leads to improved 
performance.
•
You do not need an NVIDIA AI Enterprise license.
•
You do not need to include NVIDIA VIB file for the vGPU host manager driver in the cluster image in vSphere Lifecycle 
Manager.
•
You run DL workloads from NVIDIA from the feature branch instead of the production branch. For example, the image 
URL of the Pytorch container on the Production Branch May 2025 production branch could be nvcr.io/nvidia/pytorch-
pb25h1:25.03.03-py3 and you use it with vGPU. The image URL of the Pytorch container on the feature branch could 
be nvcr.io/nvidia/pytorch:25.03-py3 and you use it with GPU passthrough.
For information about feature branches and production branches of the NVIDIA NGC containers, see the NVIDIA AI 
Enterprise Release Notes.
•
You cannot migrate live workloads across hosts by using vSphere vMotion.
Deployment Workflows for VMware Private AI Foundation with NVIDIA
You follow either of the following independent workflows. You can implement either or both workflows on the same 
infrastructure.
VMware by Broadcom
40


---

VMware Private AI Foundation with NVIDIA 9.1
•
Setup Workflows for Deep Learning VMs and VKS Clusters with GPU
Provision AI workloads in the form of deep learning VMs or AI applications on VKS clusters
•
Setup Workflows for Private AI Services
Run AI models and create agents for them in Private AI Services
Setup Workflows for Deep Learning VMs and VKS Clusters with GPU
By using deep learning VMs and VKS Clusters with GPU access in VMware Private AI Foundation with NVIDIA your 
organization can build an AI workload platform. As a VI administrator, you add infrastructure with GPU to VCF. Then, 
when running the Private AI Foundation Quickstart wizard, a VCF Automation organization administrator specifies the 
deployment namespace and creates the catalog items. When a user requests a catalog item, the associated AI workloads 
are deployed in the designated namespace.
For information about deep learning VMs, see About Deep Learning VM Images in VMware Private AI Foundation with 
NVIDIA.
For guidelines on designing an environment for running private AI workloads on top of VMware Private AI Foundation with 
NVIDIA, see the Design section of the VCF documentation.
Deep learning VMS and AI VKS clusters can be provisioned on top of different compute infrastructure, which can include 
GPU devices in vGPU or passthrough mode.
You set up a platform for running deep learning VMs and AI applications on VKS clusters according to the Internet 
connectivity of your environment and according to the compute model for your infrastructure in either of the following 
ways:
•
Setting Up an AI Workload Platform in a Connected Environment
•
Setting Up an AI Workload Platform in a Disconnected (Air-Gapped) Environment
Setting Up an AI Workload Platform in a Connected Environment
Setting up a platform for provisioning AI workloads, such as deep learning VMs and containerized applications on VKS 
clusters, includes a foundational set of components with additional components that are specific to the platform.
Note:
When you run the Private AI Foundation Quickstart in VCF Automation, you must prepare the environment for 
both AI workload platform models, that is, deep learning VMs and AI applications on VKS clusters, although you 
might plan to apply only one of them.
VMware by Broadcom
41


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 1: Setup Workflow for an AI Workload Platform in a Connected Environment
Generate an API key for 
access to the NVIDIA 
NGC catalog .
1. Configure a License 
Service instance on the 
NVIDIA Licensing Portal
. 
2. Generate a client 
configuration token
.
Set up a machine that 
has access to the 
Supervisor instance , and 
has Helm , Docker , VCF 
Consumption CLI , and 
kubectl .
Deploy a workload 
domain with a Supervisor 
instance .
Configure VM classes for 
AI workloads .
Verify if VCF Automation 
is deployed or deploy 
VCF Automation
Set up VCF Automation 
to use the 
workload domain in an 
organization and a 
namespace .
VCF environment meeting the 
requirements for deep learning 
VMs and VKS clusters with GPU
Configure NVIDIA vGPU
or NVIDIA GPU 
passthrough .
Add hosts with 
Advanced NIC 
Passthrough .
Add AI catalog items in a 
VCF Automation 
namespace .
VCF environment ready for 
provisioning deep learning VMs 
and VKS clusters with GPU
Required for vGPU and GPU passthrough
Required only for vGPU
VMware by Broadcom
42


---

VMware Private AI Foundation with NVIDIA 9.1
Table 5: AI Workload Platform Setup Workflow for a Connected Environment
Task
AI Workload Type
Compute Models
Steps
Review the architecture and 
requirements for deploying
VMware Private AI Foundation 
with NVIDIA.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
•
System Architecture of VMware 
Private AI Foundation with 
NVIDIA
•
Requirements for Deploying 
VMware Private AI Foundation 
with NVIDIA
•
Physical Infrastructure Resources 
for AI Workloads in VMware 
Private AI Foundation with 
NVIDIA
•
Verify that VCF Automation
was deployed in your VCF
fleet as a part of the VCF
deployment workflow.
•
If VCF Automation is not 
a part of your fleet, deploy
VCF Automation by using
VCF Operations fleet 
management.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
VCF Deployment, Convergence, and 
Upgrade
Deploy VCF Automation into an 
Existing VCF Fleet
If you plan to run models on 
NVIDIA vGPUs, configure 
a License Service instance 
on the NVIDIA Licensing 
Portal and generate a client 
configuration token.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
vGPU
NVIDIA License System User Guide.
If you plan to run models on 
NVIDIA vGPUs, generate 
an API key for access to the 
NVIDIA NGC catalog.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
vGPU
Pulling and Running NVIDIA AI 
Enterprise Containers
Deploy a GPU-accelerated 
workload domain with a 
Supervisor instance.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Deploy a Workload Domain for 
VMware Private AI Foundation with 
NVIDIA.
Configure NVIDIA vGPU or 
NVIDIA GPU passthrough 
for AI workloads on the ESX 
hosts.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Configure NVIDIA vGPU or GPU 
Passthrough for AI Workloads on the 
ESX Hosts
Optionally, add hosts with 
Advanced NIC Passthrough.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Add a vSphere Cluster with 
Advanced NIC Passthrough to a 
Workload Domain in VMware Private 
AI Foundation with NVIDIA
Configure VM classes for AI 
workloads.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Configure VM Classes for AI 
Workloads for VMware Private AI 
Foundation with NVIDIA
Set up a machine that has 
access to the Supervisor 
instance, and has Docker, 
Helm, VCF Consumption CLI, 
and kubectl.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Getting Started with the VCF 
Consumption CLI
Set up VCF Automation to 
use the workload domain 
in an organization and a 
namespace.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Set Up Your VCF Automation 
Organization for VMware Private AI 
Foundation with NVIDIA
VMware by Broadcom
43


---

VMware Private AI Foundation with NVIDIA 9.1
Task
AI Workload Type
Compute Models
Steps
Add AI catalog items to VCF 
Automation.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Create AI Catalog Items in VCF 
Automation using the Private AI 
Foundation Quickstart
Setting Up an AI Workload Platform in a Disconnected (Air-Gapped) Environment
In a disconnected (air-gapped) environment, you add an extended set of components and capabilities. You perform 
additional steps for setting up and deploying AI workloads locally.
Note:
When you run the Private AI Foundation Quickstart in VCF Automation, you must prepare the environment for 
both AI workload platform models, that is, deep learning VMs and AI applications on VKS clusters, although you 
might plan to consume only one of them.
In an environment that is physically disconnected from the Internet, you set up the following three host machines for 
centralized setup. You can use one host with a combined purpose, for example, if the machine is a server in a DMZ with 
access to the Internet and temporary access to the isolated environment over a firewall. You might already have these 
machines configured so that you can access the VCF platform functionality in the disconnected environment.
Bastion host
A VM, usually running Linux, that is connected to the Internet or has access to download packages, 
binaries, and images from the Internet.
Admin host
A VM, usually running Linux, within the air-gapped environment that has no Internet access. The admin 
host is connected to all the hosts in the environment. Files are downloaded from the Internet to the bastion 
host and are then transferred to the administrator host. Then, VI administrators can use the administrator 
host to interact with the environment.
The admin host and the bastion host run the same Linux OS image, usually Ubuntu.
Jump host
A machine for end-user access to the environment and its UI.
VMware by Broadcom
44


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 2: Setup Workflow for an AI Workload Platform in a Disconnected Environment
Generate an API key for 
access to the NVIDIA 
NGC catalog .
Deploy a workload 
domain with a Supervisor 
instance .
Configure VM classes for 
AI workloads .
Verify if VCF Automation 
is deployed or deploy 
VCF Automation
Set up VCF Automation 
to use the 
workload domain in an 
organization and a 
namespace .
VCF environment meeting the 
requirements for deep learning 
VMs and VKS clusters with GPU
Configure NVIDIA vGPU
or NVIDIA GPU 
passthrough .
Add AI catalog items in a 
VCF Automation 
namespace .
VCF environment ready for 
provisioning deep learning VMs 
and VKS clusters with GPU
Set up bastion and 
admin hosts for access to 
the Internet and 
environment .
Set up a local Harbor 
registry as a service in 
VCF Automation .
Configure a content 
library for vSphere 
Kubernetes releases 
(VKr) for Ubuntu .
Upload vGPU guest 
drivers or data center
GPU driver to a Web 
server .
Upload the NVIDIA Helm 
charts and container 
images to the 
environment .
Provide a local Ubuntu 
package repository .
Add hosts with 
performance NIC 
offload .
Deploy an NVIDIA 
Delegated License 
Service Instance and 
register it on the NVIDIA 
Licensing Portal .
1. Bind and install a 
license server on it . 
2. Generate a client 
configuration token
.
Required for vGPU and GPU passthrough
Required only for vGPU
VMware by Broadcom
45


---

VMware Private AI Foundation with NVIDIA 9.1
Table 6: AI Workload Platform Setup Workflow for a Disconnected Environment
Task
AI Workload Type
Compute Models
Steps
Review the architecture and 
requirements for deploying
VMware Private AI Foundation 
with NVIDIA.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
•
System Architecture of VMware 
Private AI Foundation with 
NVIDIA
•
Requirements for Deploying 
VMware Private AI Foundation 
with NVIDIA
•
Verify that VCF Automation
was deployed in your VCF
fleet as a part of the VCF
deployment workflow.
•
If VCF Automation is not 
a part of your fleet, deploy
VCF Automation by using
VCF Operations fleet 
management.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
VCF Deployment, Convergence, and 
Upgrade
Deploy VCF Automation into an 
Existing VCF Fleet
If you plan to run models on 
NVIDIA vGPUs, deploy an 
NVIDIA Delegated License 
Service Instance.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
vGPU
Installing and Configuring the DLS 
Virtual Appliance
You can deploy the virtual appliance 
in the same workload domain as the 
AI workloads or in the management 
domain.
1.
Register an NVIDIA DLS 
instance on the NVIDIA 
Licensing Portal, and bind 
and install a license server 
on it.
2.
Generate a client 
configuration token.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
vGPU
•
Configuring a Service Instance
•
Managing Licenses on a License 
Server.
Configure NVIDIA vGPU or 
NVIDIA GPU passthrough 
for AI workloads on the ESX 
hosts.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Configure NVIDIA vGPU or GPU 
Passthrough for AI Workloads on the 
ESX Hosts
Optionally, add hosts with 
Advanced NIC Passthrough.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Add a vSphere Cluster with 
Advanced NIC Passthrough to a 
Workload Domain in VMware Private 
AI Foundation with NVIDIA
Configure VM classes for AI 
workloads.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Configure VM Classes for AI 
Workloads for VMware Private AI 
Foundation with NVIDIA
Configure a content library for 
vSphere Kubernetes releases 
(VKr) for Ubuntu.
GPU-accelerated VKS clusters •
vGPU
•
GPU passthrough
Configure a Content Library with 
Ubuntu VKr for a Disconnected 
VMware Private AI Foundation with 
NVIDIA Environment
Set up VCF Automation to 
use the workload domain 
in an organization and a 
namespace.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Setting Up VCF Automation for 
Private AI Foundation with NVIDIA
VMware by Broadcom
46


---

VMware Private AI Foundation with NVIDIA 9.1
Task
AI Workload Type
Compute Models
Steps
Set up hosts for access to the 
Internet and the environment.
•
For a bastion host, set up 
a machine that has access 
to the Internet and has 
Docker, Helm, and VCF 
Consumption CLI installed.
•
For an admin host, set up 
a machine that has access 
to vCenter for the workload 
domain, the Supervisor 
instance, and the local 
container registry.
The machine must have 
Docker, VCF Consumption 
CLI, and kubectl.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
•
CPU-only
Getting Started with the VCF 
Consumption CLI
Set up a local Harbor registry.
You can also OCI-compliant 
registries from other vendors.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Set Up a Local Platform Harbor 
Registry for VMware Private AI 
Foundation with NVIDIA
Set up a web server hosting 
a local Ubuntu package 
repository. The web server 
must be accessible by the 
VKS clusters with GPU.
GPU-accelerated VKS clusters •
vGPU
•
GPU passthrough
Provide an Ubuntu Package 
Repository Mirror for VKS Clusters 
with GPU in a Disconnected 
Environment
For using vGPU in deep 
learning VMs, provide a 
location to download the 
vGPU guest drivers from.
Deep learning VMs
vGPU
Provide vGPU and GPU Data Center 
Drivers for Deep Learning VMs in a 
Disconnected Environment
For using GPU passthrough 
in deep learning VMs, provide 
a location to download the 
NVIDIA data center driver.
Deep learning VMs
GPU passthrough
Provide vGPU and GPU Data Center 
Drivers for Deep Learning VMs in a 
Disconnected Environment
1.
Upload the NVIDIA GPU 
Operator.
2.
Upload the NVIDIA NGC 
container images to a 
private container registry, 
such as the Harbor 
Registry service of the 
Supervisor.
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Upload the Components for Deep 
Learning VMs and VKS Clusters with 
GPU to a Disconnected Environment
Add AI catalog items to VCF 
Automation
•
Deep learning VMs
•
GPU-accelerated VKS 
clusters
•
vGPU
•
GPU passthrough
Create AI Catalog Items in VCF 
Automation using the Private AI 
Foundation Quickstart
Setup Workflows for Private AI Services
Private AI Services in VMware Private AI Foundation with NVIDIA supports application developers, MLOps, and data 
scientists in building generative AI agent-based applications. As a VI administrator, you add infrastructure with specific 
AI-centric features to VCF. Then, you install Private AI Services on the Supervisor clusters. Next, Private AI Services
instances can be created, one per namespace in VCF Automation or directly in the Supervisor cluster.
For information on the Private AI Services capabilities, see Delivering Generative AI Applications by Using Private AI 
Services.
For guidelines on designing an environment for running private AI workloads on top of VMware Private AI Foundation with 
NVIDIA, see the Design section of the VCF documentation.
VMware by Broadcom
47


---

VMware Private AI Foundation with NVIDIA 9.1
AI workloads can run on top of different compute infrastructure, which can include GPU devices in vGPU or passthrough 
mode or can be CPU-only.
You set up Private AI Services according to the connectivity of your environment to the Internet and according to the 
compute model for your infrastructure in either of the following ways:
•
Setting Up Private AI Services in a Connected Environment
•
Setting Up Private AI Services in a Disconnected (Air-Gapped) Environment
Setting Up Private AI Services in a Connected Environment
The setup of Private AI Services includes a foundational set of components for running AI workloads with additional 
components that are specific to the platform.
Figure 3: Setup Workflow for Private AI Services 3.0 in a Connected Environment
Generate an API key for 
access to the NVIDIA 
NGC catalog .
1. Configure a License 
Service instance on the 
NVIDIA Licensing Portal
. 
2. Generate a client 
configuration token
.
Set up a machine that 
has access to the 
Supervisor instance , and 
has Docker , VCF 
Consumption CLI , and 
kubectl .
Deploy a workload 
domain with a Supervisor 
instance .
Configure VM classes for 
AI workloads .
Verify if VCF Automation 
is deployed or deploy 
VCF Automation
Set up VCF Automation 
to use the 
workload domain in an 
organization and a 
namespace .
Deploy VMware Data 
Services Manager .
VCF environment meeting the 
requirements for Private AI 
Services
Configure NVIDIA vGPU
or NVIDIA GPU 
passthrough .
Install the Supervisor 
Service for Private AI 
Services.
Activate Private AI 
Services in a namespace
in VCF Automation or 
vSphere Supervisor .
Visualize metrics in 
observability platforms
. 
VCF environment running Private 
AI Services
Required for vGPU and GPU passthrough
Required only for vGPU
Add hosts with 
Advanced NIC 
Passthrough .
Set up a Harbor registry
.
VMware by Broadcom
48


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 4: Setup Workflow for Private AI Services 2.1 in a Connected Environment
Generate an API key for 
access to the NVIDIA 
NGC catalog .
1. Configure a License 
Service instance on the 
NVIDIA Licensing Portal
. 
2. Generate a client 
configuration token
.
Set up a machine that 
has access to the 
Supervisor instance , and 
has Docker , VCF 
Consumption CLI , and 
kubectl .
Deploy a workload 
domain with a Supervisor 
instance .
Configure VM classes for 
AI workloads .
Verify if VCF Automation 
is deployed or deploy 
VCF Automation
Set up VCF Automation 
to use the 
workload domain in an 
organization and a 
namespace .
Deploy VMware Data 
Services Manager .
VCF environment meeting the 
requirements for Private AI 
Services
Configure NVIDIA vGPU
or NVIDIA GPU 
passthrough .
Install the Supervisor 
Service for Private AI 
Services.
Activate Private AI 
Services in a VCF 
Automation namespace .
Visualize metrics in 
observability platforms
. 
VCF environment running Private 
AI Services
Required for vGPU and GPU passthrough
Required only for vGPU
Add hosts with 
Advanced NIC 
Passthrough .
Set up a Harbor registry .
VMware by Broadcom
49


---

VMware Private AI Foundation with NVIDIA 9.1
Table 7: Private AI Services Setup Workflow for a Connected Environment
Task
Compute Models
Steps
Review the architecture and 
requirements for deploying VMware 
Private AI Foundation with NVIDIA.
•
vGPU
•
GPU passthrough
•
CPU-only
•
System Architecture of VMware Private AI 
Foundation with NVIDIA
•
Requirements for Deploying VMware Private 
AI Foundation with NVIDIA
•
Physical Infrastructure Resources for AI 
Workloads in VMware Private AI Foundation 
with NVIDIA
If you plan to use Private AI Services
from VCF Automation, perform the 
following tasks:
•
Verify that VCF Automation was 
deployed in your VCF fleet as a part 
of the VCF deployment workflow.
•
If VCF Automation is not a part of 
your fleet, deploy VCF Automation
from VCF Operations.
•
VCF Automation is optional for
Private AI Services 3.0.
•
VCF Automation is required for
Private AI Services 2.1 if you plan to 
use the UI.
•
vGPU
•
GPU passthrough
•
CPU-only
Deploy VCF Automation into an Existing VCF 
Fleet
If you plan to run models on NVIDIA 
vGPUs, configure a License Service 
instance on the NVIDIA Licensing Portal 
and generate a client configuration 
token.
vGPU
NVIDIA License System User Guide.
If you plan to run models on NVIDIA 
vGPUs, generate an API key for access 
to the NVIDIA NGC catalog.
vGPU
Pulling and Running NVIDIA AI Enterprise 
Containers
Deploy a workload domain with a 
Supervisor instance.
•
vGPU
•
GPU passthrough
•
CPU-only
Deploy a Workload Domain for VMware Private 
AI Foundation with NVIDIA.
Set up a machine that has access to the 
Supervisor instance, and has Docker,
VCF Consumption CLI, and kubectl.
•
vGPU
•
GPU passthrough
•
CPU-only
Getting Started with the VCF Consumption CLI
Configure NVIDIA vGPU or NVIDIA GPU 
passthrough for AI workloads on the 
ESX hosts.
•
vGPU
•
GPU passthrough
Configure NVIDIA vGPU or GPU Passthrough for 
AI Workloads on the ESX Hosts
Optionally, add hosts with Advanced NIC 
Passthrough.
•
vGPU
•
GPU passthrough
Add a vSphere Cluster with Advanced NIC 
Passthrough to a Workload Domain in VMware 
Private AI Foundation with NVIDIA
Configure VM classes for AI workloads.
•
vGPU
•
GPU passthrough
•
CPU-only
Configure VM Classes for AI Workloads for 
VMware Private AI Foundation with NVIDIA
VMware by Broadcom
50


---

VMware Private AI Foundation with NVIDIA 9.1
Task
Compute Models
Steps
Deploy VMware Data Services Manager.
•
VMware Data Services Manager
is required for Private AI Services
3.0 if you use knowledge bases 
and agents. If you plan to use
Private AI Services to deploy model 
endpoints only, VMware Data 
Services Manager is not required.
•
VMware Data Services Manager is 
required for Private AI Services 2.1
You can also use PostgreSQL 
databases with pgvector support from 
another vendor.
•
vGPU
•
GPU passthrough
•
CPU-only
1.
Installing and Configuring VMware Data 
Services Manager
You deploy a VMware Data Services 
Manager instance in the management 
domain.
You can also use the guided deployment UI in 
the vSphere Client.
2.
Install the Data Services Manager 
Consumption Operator as a Supervisor 
Service
3.
Configure VMware Data Services Manager
with at least one infrastructure policy
Set up a Harbor registry as a Supervisor 
Service.
You use it for model galleries for storing 
validated ML models and running them 
in Private AI Services.
You can also use Private AI Services
with OCI-compliant registries from other 
vendors.
•
vGPU
•
GPU passthrough
•
CPU-only
Set Up a Local Platform Harbor Registry for 
VMware Private AI Foundation with NVIDIA
If you plan to use Private AI Services
from VCF Automation, set up VCF 
Automation to use the workload domain 
in an organization and a namespace..
•
VCF Automation is optional for
Private AI Services 3.0.
Starting with Private AI Services
3.0, the configuration wizard is also 
available in the vSphere Client.
•
VCF Automation is required for
Private AI Services 2.1 if you plan to 
use the UI for setup.
To set up Private AI Services without 
VCF Automation, you must use the 
kubectl CLI.
•
vGPU
•
GPU passthrough
•
CPU-only
Set Up Your VCF Automation Organization for 
VMware Private AI Foundation with NVIDIA
Install the Private AI Services Supervisor 
service in the Supervisor.
•
vGPU
•
GPU passthrough
•
CPU-only
Install Private AI Services on the Supervisors of 
the Workload Domain
Activate Private AI Services in a 
namespace.
•
vGPU
•
GPU passthrough
•
CPU-only
Private AI Services 3.0
•
Activate Private AI Services 3.0 in an 
Organization Namespace by Using the UI
•
Activate Private AI Services 3.0 in an 
Organization Namespace by Using the 
kubectl Command
Private AI Services 2.1
•
Activate Private AI Services 2.1 in an 
Organization Namespace by Using the VCF 
Automation UI
•
Activate Private AI Services in an 
Organization Namespace by Using the 
kubectl Command
Visualize metrics in observability 
platforms, such as Grafana and VCF 
Operations.
•
vGPU
•
GPU passthrough
•
CPU-only
Visualizing Private AI Services Metrics in 
Observability Platforms
VMware by Broadcom
51


---

VMware Private AI Foundation with NVIDIA 9.1
Setting Up Private AI Services in a Disconnected (Air-Gapped) Environment
In a disconnected (air-gapped) environment, you add an extended set of components and capabilities. You perform 
additional steps for setting up and deploying appliances and providing resources locally, so that Private AI Services can 
access them.
In an environment that is physically disconnected from the Internet, you set up the following three host machines for 
centralized setup. You can use one host with a combined purpose, for example, if the machine is a server in a DMZ with 
access to the Internet and temporary access to the isolated environment over a firewall. You might already have these 
machines configured so that you can access the VCF platform functionality in the disconnected environment.
Bastion host
A VM, usually running Linux, that is connected to the Internet or has access to download packages, 
binaries, and images from the Internet.
Admin host
A VM, usually running Linux, within the air-gapped environment that has no Internet access. The admin 
host is connected to all the hosts in the environment. Files are downloaded from the Internet to the bastion 
host and are then transferred to the administrator host. Then, VI administrators can use the administrator 
host to interact with the environment.
The admin host and the bastion host run the same Linux OS image, usually Ubuntu.
Jump host
A machine for end-user access to the environment and its UI.
Figure 5: Setup Workflow for Private AI Services 3.0 in a Disconnected Environment
1. Bind and install a 
license server on it .
2. Generate a client 
configuration token
.
Deploy an NVIDIA 
Delegated License 
Service Instance and 
register it on the NVIDIA 
Licensing Portal .
.
Set up a local Harbor 
registry as a service in 
VCF Automation
Deploy a workload 
domain with Supervisor
.
Configure VM classes for 
AI workloads .
Verify if VCF Automation 
is deployed or deploy 
VCF Automation
Set up VCF Automation 
to use the 
workload domain in an 
organization and a 
namespace .
Deploy VMware Data 
Services Manager .
VCF environment meeting the 
requirements for Private AI 
Services
Configure vGPU or GPU 
passthrough
Activate Private AI 
Services in a namespace
in VCF Automation or 
vSphere Supervisor .
Install the Supervisor 
Service for Private AI 
Services.
Visualize metrics in 
observability platforms
. 
VCF environment running Private 
AI Services
Required for vGPU and GPU passthrough
Required only for vGPU
Set up a bastion and 
admin hosts for access to 
the Internet and 
environment .
Upload the components 
of Private AI Services to 
Harbor by using the 
artifact mirroring tool
.
Configure a content 
library for vSphere 
Kubernetes releases 
(VKr) for Ubuntu .
Provide a Web server 
with a local Ubuntu 
package repository .
Add hosts with 
performance NIC 
offload .
VMware by Broadcom
52


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 6: Setup Workflow for Private AI Services 2.1 in a Disconnected Environment
1. Bind and install a 
license server on it .
2. Generate a client 
configuration token
.
Deploy an NVIDIA 
Delegated License 
Service Instance and 
register it on the NVIDIA 
Licensing Portal .
.
Set up a local Harbor 
registry as a service in 
VCF Automation
Deploy a workload 
domain with Supervisor
.
Configure VM classes for 
AI workloads .
Verify if VCF Automation 
is deployed or deploy 
VCF Automation
Set up VCF Automation 
to use the 
workload domain in an 
organization and a 
namespace .
Deploy VMware Data 
Services Manager .
VCF environment meeting the 
requirements for Private AI 
Services
Configure vGPU or GPU 
passthrough
Activate Private AI 
Services in a VCF 
Automation namespace .
Install the Supervisor 
Service for Private AI 
Services.
Visualize metrics in 
observability platforms
. 
VCF environment running Private 
AI Services
Required for vGPU and GPU passthrough
Required only for vGPU
Set up a bastion and 
admin hosts for access to 
the Internet and 
environment .
Upload the components 
of Private AI Services to 
Harbor by using the 
artifact mirroring tool
.
Configure a content 
library for vSphere 
Kubernetes releases 
(VKr) for Ubuntu .
Provide a Web server 
with a local Ubuntu 
package repository .
Add hosts with 
performance NIC 
offload .
VMware by Broadcom
53


---

VMware Private AI Foundation with NVIDIA 9.1
Table 8: Private AI Services Setup Workflow for a Disconnected Environment
Task
Compute Models
Steps
Review the architecture and 
requirements for deploying VMware 
Private AI Foundation with NVIDIA.
•
vGPU
•
GPU passthrough
•
CPU-only
•
System Architecture of VMware Private AI 
Foundation with NVIDIA
•
Requirements for Deploying VMware Private 
AI Foundation with NVIDIA
If you plan to use Private AI Services
from VCF Automation, perform the 
following tasks:
•
Verify that VCF Automation was 
deployed in your VCF fleet as a part 
of the VCF deployment workflow.
•
If VCF Automation is not a part of 
your fleet, deploy VCF Automation
by using VCF Operations fleet 
management.
•
vGPU
•
GPU passthrough
•
CPU-only
Deploy VCF Automation into an Existing VCF 
Fleet
If you plan to run models on NVIDIA 
vGPUs, deploy an NVIDIA Delegated 
License Service Instance.
vGPU
Installing and Configuring the DLS Virtual 
Appliance
You can deploy the virtual appliance in the same 
workload domain as the AI workloads or in the 
management domain.
1.
Register an NVIDIA DLS instance 
on the NVIDIA Licensing Portal, and 
bind and install a license server on it.
2.
Generate a client configuration 
token.
vGPU
•
Configuring a Service Instance
•
Managing Licenses on a License Server.
Deploy a workload domain with a 
Supervisor instance.
•
vGPU
•
GPU passthrough
•
CPU-only
Deploy a Workload Domain for VMware Private 
AI Foundation with NVIDIA.
Configure NVIDIA vGPU or NVIDIA GPU 
passthrough for AI workloads on the 
ESX hosts.
•
vGPU
•
GPU passthrough
Configure NVIDIA vGPU or GPU Passthrough for 
AI Workloads on the ESX Hosts
Optionally, add hosts with Advanced NIC 
Passthrough.
•
vGPU
•
GPU passthrough
Add a vSphere Cluster with Advanced NIC 
Passthrough to a Workload Domain in VMware 
Private AI Foundation with NVIDIA
Configure VM classes for AI workloads.
•
vGPU
•
GPU passthrough
•
CPU-only
Configure VM Classes for AI Workloads for 
VMware Private AI Foundation with NVIDIA
Deploy VMware Data Services Manager.
•
VMware Data Services Manager
is required for Private AI Services
3.0 if you use knowledge bases 
and agents. If you plan to use
Private AI Services to deploy model 
endpoints only, VMware Data 
Services Manager is not required.
•
VMware Data Services Manager is 
required for Private AI Services 2.1
You can also use PostgreSQL 
databases with pgvector support from 
another vendor.
•
vGPU
•
GPU passthrough
•
CPU-only
1.
Installing and Configuring VMware Data 
Services Manager
You deploy a VMware Data Services 
Manager instance in the management 
domain.
You can also use the guided deployment UI in 
the vSphere Client.
2.
Install the Data Services Manager 
Consumption Operator as a Supervisor 
Service
3.
Configure VMware Data Services Manager
with at least one infrastructure policy
VMware by Broadcom
54


---

VMware Private AI Foundation with NVIDIA 9.1
Task
Compute Models
Steps
Set up hosts for access to the Internet 
and the environment.
•
For a bastion host, set up a machine 
that has access to the Internet, 
and has Docker, Helm and VCF 
Consumption CLI installed.
•
For an admin host, set up a machine 
that has access to vCenter for the 
workload domain, the Supervisor 
instance, and the local container 
registry.
The machine must have Docker,
VCF Consumption CLI, and
kubectl.
.
•
vGPU
•
GPU passthrough
•
CPU-only
Getting Started with the VCF Consumption CLI
Configure a content library for vSphere 
Kubernetes releases (VKr) for Ubuntu
•
vGPU
•
GPU passthrough
•
CPU-only
Configure a Content Library with Ubuntu VKr for a 
Disconnected VMware Private AI Foundation with 
NVIDIA Environment
If you plan to use Private AI Services
from VCF Automation, set up VCF 
Automation for private AI workload 
provisioning.
•
vGPU
•
GPU passthrough
•
CPU-only
Set Up Your VCF Automation Organization for 
VMware Private AI Foundation with NVIDIA
Set up a Harbor registry as a service in 
VCF Automation or vSphere Supervisor.
You use it for deployment artifacts 
images from the software depot and the 
Broadcom Artifactory for model galleries 
for storing validated ML models and 
running them in Private AI Services.
You can also use Private AI Services
with OCI-compliant registries from other 
vendors.
•
vGPU
•
GPU passthrough
•
CPU-only
Set Up a Local Platform Harbor Registry for 
VMware Private AI Foundation with NVIDIA
Set up a web server hosting a local 
Ubuntu package repository. The web 
server must be accessible by the VKS 
clusters with GPU.
•
vGPU
•
GPU passthrough
Provide an Ubuntu Package Repository Mirror 
for Private AI Services in a Disconnected 
Environment
Upload the components of Private AI 
Services to the environment
•
vGPU
•
GPU passthrough
•
CPU-only
Upload the Private AI Services 3.0 Components 
to a Disconnected Environment
Upload the Private AI Services 2.1 Components 
to a Disconnected Environment
Install the Private AI Services service in 
the Supervisor
•
vGPU
•
GPU passthrough
•
CPU-only
Install Private AI Services on the Supervisors of 
the Workload Domain
Activate Private AI Services in a 
namespace
•
vGPU
•
GPU passthrough
•
CPU-only
Private AI Services 3.0
•
Activate Private AI Services 3.0 in an 
Organization Namespace by Using the UI
•
Activate Private AI Services 3.0 in an 
Organization Namespace by Using the 
kubectl Command
Private AI Services 2.1
•
Activate Private AI Services 2.1 in an 
Organization Namespace by Using the VCF 
Automation UI
•
Activate Private AI Services in an 
Organization Namespace by Using the 
kubectl Command
VMware by Broadcom
55


---

VMware Private AI Foundation with NVIDIA 9.1
Task
Compute Models
Steps
Pull metrics in observability platforms, 
such as Grafana and VCF Operations.
•
vGPU
•
GPU passthrough
•
CPU-only
Visualizing Private AI Services Metrics in 
Observability Platforms
System Architecture of VMware Private AI Foundation with NVIDIA
VMware Private AI Foundation with NVIDIA runs on top of VCF adding support for AI workloads in workload domains with
vSphere Supervisor provisioned by using VCF Automation or kubectl.
VMware by Broadcom
56


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 7: Example Architecture for VMware Private AI Foundation with NVIDIA
Workload Domain
vSphere Cluster
Node 1
Node 2
NSX Edge Cluster or 
VNA Cluster
Namespace
Namespace
Namespace
Namespace
Private AI Services Service
ESX Host with 
NVIDIA GPUs
ESX Host with 
NVIDIA GPUs
ESX Host with 
NVIDIA GPUs
. . . 
Supervisor
Harbor Registry Service
(Disconnected 
Environments , Model 
Gallery )
vSphere 
Kubernetes Service 
Cluster
NVIDIA GPU 
Operator
vSphere 
Kubernetes Service  
Cluster for Model 
Endpoints
Deep Learning Virtual Machines and 
Container AI Workloads
Chatbot Applications
NVIDIA GPU Cloud (NGC) Container Images
NVIDIA Licensing Portal
vSphere Cluster
ESX Host
ESX Host
ESX Host
. . . 
Vector Database
(PostgreSQL 
with pgvector )
Vector Database
(PostgreSQL 
with pgvector )
NVIDIA Delegated License 
Service
(Disconnected Environments
)
Shared Datastore
Content Library 
for vSphere 
Kubernetes 
Releases
Content Library 
for Deep 
Learning VM 
Images
vSphere Cluster
ESX Host
ESX Host
ESX Host
. . . 
Management vCenter
Management 
NSX Manager
VMware Data Services 
Manage r
Workload Domain NSX 
Manager
Workload Domain 
vCenter
VCF Operations
VCF Automation
Management Domain
VMware by Broadcom
57


---

VMware Private AI Foundation with NVIDIA 9.1
Table 9: Components for Running AI Workloads in VMware Private AI Foundation with NVIDIA
Component
VCF Domain
Description
GPU-enabled ESX hosts
ESX hosts can have an NVIDIA GPU that is supported for
VMware Private AI Foundation with NVIDIA.
See Supported NVIDIA GPU Devices.
If you are using vGPUs, you must have the NVIDIA vGPU 
host driver installed and an NVIDIA AI Enterprise license. 
A GPU device can then be shared between workloads 
by using the time slicing or Multi-Instance GPU (MIG) 
mechanism.
If you are using GPU passthrough, ESX host drivers and 
NVIDIA AI Enterprise license are not required.
Supervisor
One or more vSphere clusters that are enabled for
vSphere Supervisor so that you can run virtual machines 
and containers on VCF by using the Kubernetes API. A 
Supervisor serves as the control plane to manage VKS 
clusters with GPU and virtual machines.
Harbor registry
You can use a Harbor registry in the following cases:
•
In a disconnected environment, as a local image 
registry where you host the container images 
downloaded from the NVIDIA NGC catalog and the 
images for installing Private AI Services.
•
For storing validated ML models for Private AI 
Services for both connected and disconnected 
environments.
You can also use a container registry from another vendor.
Private AI Services Service
A Supervisor service for running AI applications with 
specified LLMs and knowledge bases with profiled 
business data.
NSX Edge cluster or Virtual Network 
Appliance (VNA) cluster
A cluster of NSX Edge nodes or Virtual Network Appliance 
nodes that provides network services for the Supervisor 
instance and the workloads it runs. It is required for 
Supervisor clusters that use NSX VPC networking.
For information about the Supervisor network 
configuration, see Supervisor Architecture with VCF 
Networking with VPC.
NVIDIA GPU Operator
Automates the management of all NVIDIA software 
components that are needed to provision GPU to 
containers in a Kubernetes cluster. NVIDIA GPU Operator 
is deployed on a VKS cluster.
NVIDIA Network Operator
Helps configure the right Mellanox drivers for containers 
using virtual functions for high-speed networking, RDMA, 
and GPUDirect. The Network Operator works together 
with the GPU Operator to enable GPUDirect RDMA 
on compatible systems. NVIDIA Network Operator is 
deployed on a VKS cluster.
Vector database
Workload domain
A PostgreSQL database that has the pgvector extension 
enabled so that you can use it in RAG AI workloads.
You use VMware Data Services Manager for deploying 
such databases.
VMware by Broadcom
58


---

VMware Private AI Foundation with NVIDIA 9.1
Component
VCF Domain
Description
•
NVIDIA Licensing Portal
•
NVIDIA Delegated License 
Service (DLS)
Required for running AI workloads on vGPU. You use the 
NVIDIA Licensing Portal to generate a client configuration 
token to assign a license to the guest vGPU driver in the 
deep learning virtual machine and the GPU Operators on 
VKS clusters.
In a disconnected environment or to have your workloads 
getting license information without using an Internet 
connection, you host the NVIDIA licenses locally on a 
Delegated License Service (DLS) appliance.
Content library
Content libraries store the images for the deep learning 
virtual machines and for the VMware vSphere Kubernetes 
releases (VKrs). You use these images for AI workload 
deployment within the VMware Private AI Foundation with 
NVIDIA environment.
In a connected environment, content libraries pull their 
content from VMware managed public content libraries. In 
a disconnected environment, you must pull the required 
images from the software depot or upload them manually.
NVIDIA GPU Cloud (NGC) catalog
A portal for GPU-optimized containers for AI, and machine 
learning that are tested and ready to run on supported 
NVIDIA GPUs on premises on top of VMware Private AI 
Foundation with NVIDIA.
VCF Operations
VCF Operations provides a central location for preparing 
and monitoring your VCF environment in the following 
way:
•
Deploy a GPU-enabled workload domain that is based 
on vSphere Lifecycle Manager images.
•
Monitor the GPU consumption in the GPU-enabled 
workload domains.
•
Deploy VCF Automation after initial VCF deployment 
and update VCF Operations and VCF Automation.
VCF Automation
You use VCF Automation to set up and consume the 
following private AI capabilities:
•
Add self-service catalog items for deploying AI 
workloads such as AI VKS clusters and deep learning 
VMs.
•
Activate Private AI Services in a namespace.
•
Deploy model endpoints in Private AI Services.
VMware Data Services Manager
By using VMware Data Services Manager, you can create 
vector databases, such as a PostgreSQL database with 
pgvector extension.
VCF core components
Management domain
Core components manage your VCF Instance. They 
include management vCenter and NSX Manager, and 
workload domain vCenter and NSX Manager.
Private AI Design
For guidelines on designing an environment for running private AI workloads on top of VMware Private AI Foundation with 
NVIDIA, see the Design section of the VCF documentation.
Physical Infrastructure Resources for AI Workloads in VMware Private 
AI Foundation with NVIDIA
Physical infrastructure resources for private AI ready infrastructure for VCF include hardware components and features to 
support AI workloads on a virtualized platform. You choose specific resource infrastructure according to the requirements 
of your organization and of the AI workloads that will be running on your platform.
VMware by Broadcom
59


---

VMware Private AI Foundation with NVIDIA 9.1
SR-IOV
Single Root I/O Virtualization (SR-IOV) is a specification that allows a single PCIe physical device under a single root port 
to appear as multiple separate virtual functions (VFs) to provide hardware acceleration to workloads.
SR-IOV is required to enable virtual functions from an NVIDIA NIC that supports network offloading. As an alternative 
for hardware acceleration with the support for major virtualization features, see Enhanced DirectPath I/O for Accelerated 
NICs.
SR-IOV is also required for both MIG and time-slicing modes of NVIDIA vGPU. For MIG, a vGPU is associated with a 
virtual function at boot time.
For information on how to enable SR-IOV at the BIOS/UEFI level, see the documentation from your server vendor.
NVIDIA NVLink and NVSwitch
The combination of NVLink
™, NVLink Switch (NVSwitch), and GPUDirect RDMA (Remote Direct Memory Access) can be 
suitable for AI/ML workloads.
•
NVLink. High-speed, low-latency GPU-to-GPU internconnect within a host, bypassing PCIe.
•
NVSwitch. A crossbar switch that enables NVLink connectivity across more than two GPUs within a host, providing full 
all-to-all bandwidth.
VCF also includes support for NVIDIA Blackwell GPUs. The NVIDIA Blackwell architecture supports up to 16 GPUs 
per host, which can be partitioned and can serve as a high-performance memory fabric across all GPUs to AI and HPC 
workloads.
•
GPUDirect RDMA. Provides support for direct memory transfers between GPUs across hosts over a network fabric like 
InfiniBand, bypassing the CPU and memory of the host. Within each host, GPUs might still communicate over NVLink 
or NVSwitch.
Enhanced DirectPath I/O for Accelerated NICs
Enhanced DirectPath I/O provides the highest networking throughput and packet rate, and the lowest latency by providing 
direct hardware access to workloads. Enhanced DirectPath I/O is compatible with GPUDirect RDMA supported with 
accelerated NICs like NVIDIA Connect X-6 DX, ConnectX-7, and BlueField3 in NIC mode.
Note:
You can use Enhanced DirectPath I/O only with RDMA over Converged Ethernet (RoCE). Using Enhanced 
DirectPath I/O with InfiniBand is not supported.
For information about using NIC offloading, see Add a vSphere Cluster with Advanced NIC Passthrough to a Workload 
Domain in VMware Private AI Foundation with NVIDIA.
Table 10: Features and Considerations for Enhanced DirectPath
Features
Details
Supported Features
•
Business continuity
– vSphere vMotion
– Storage vMotion
– vSphere HA
– vSphere DRS
– vSphere DPM
– Virtual Machine fast-suspend- resume during live patching
•
Network virtualization
– Distributed port groups, NSX segments, and NSX VPCs
– Hot removal of virtual devices, memory, and vCPU
– MAC-based VLAN for passthrough virtual functions
– SR-IOV passthrough network adapter for VMs
Benefits
GPUDirect RDMA support
VMware by Broadcom
60


---

VMware Private AI Foundation with NVIDIA 9.1
Features
Details
Considerations
•
You must install a Mellanox guestOS driver on connected VMs.
•
Migration of VMs between hosts is restricted by physical NIC or 
Mellanox host driver version.
•
Not included in the automated workflow for creating workload domains 
with a default cluster in VCF Operations and SDDC clusters in the 
vSphere Client.
•
Snapshots and hot add of virtual devices, memory, and vCPU is not 
supported.
For AI workloads that might co-exist in your VMware Private AI Foundation with NVIDIA environment but do not transmit 
RDMA traffic, you might also consider Uniform Passthrough (UPT). See Enhanced Network Offloads and Advanced NIC 
Passthrough.
Device Groups with NVIDIA GPUs and NVLink
Device groups simplify the use of virtual machines with complementary hardware devices. These groups can include 
hardware devices connected using a common PCIe switch or direct interconnect, such as NVLink and NVSwitch, and 
RDMA. They are identified at the hardware level and presented to vSphere as a unified entity, which can be added 
to virtual machines and VM classes by using the standard workflow for adding a PCI device. Both vSphere DRS and 
vSphere HA recognize device groups and ensure that virtual machine placement aligns with device group requirements.
VMware by Broadcom
61


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 8: Device Group for Interconnected GPUs
VMware by Broadcom
62


---

VMware Private AI Foundation with NVIDIA 9.1
A device group can comprise at least two GPUs connected by NVIDIA NVLink, enhancing inter-GPU communication 
vital for ML/AI performance. NVLinks enable GPUs to share high-bandwidth memory (HBM), facilitating larger machine 
learning models. NVLink can be programmatically activated or deactivated to form new device groups.
Requirements for Deploying VMware Private AI Foundation with 
NVIDIA
You deploy components of VMware Private AI Foundation with NVIDIA in your VCF environment in a workload domain 
where you must have certain NVIDIA and VCF components installed.
Required VMware Software Versions
See VMware Components in VMware Private AI Foundation with NVIDIA.
Supported NVIDIA GPU Devices
Before you start using VMware Private AI Foundation with NVIDIA, verify in the Broadcom Compatibility Guide for GPUs 
and Accelerators for AI/ML Compute that the GPUs on your ESX hosts are supported for AI and machine learning use 
cases by Broadcom.
Supported NVIDIA Network Adapters for Advanced NIC Passthrough
For intensive AI and distributed computing workloads that require very low latency and GPUDirect RDMA, VCF 9.1 
provides Enhanced DirectPath I/O.
The following network adapters are compatible with Advanced NIC Passthrough:
•
NVIDIA ConnectX-6 DX - only for Enhanced DirectPath I/O
•
NVIDIA ConnectX-7
•
NVIDIA BlueField 3 in NIC mode
For the full list of network adapters that you can use for performance NIC offload, see Broadcom Compatibility Guide for I/
O Devices for Uniform Passthrough and Enhanced DirectPath.
Note:
You can use Enhanced DirectPath I/O only with RDMA over Converged Ethernet (RoCE). Using Enhanced 
DirectPath I/O with InfiniBand is not supported.
Required NVIDIA Software
You use NVIDIA software that is specific to GPU virtualization mode. If you plan to use accelerated NIC offload, a driver 
for the NICs is also required.
Device Type
Description
GPU in vGPU mode
The GPU device must support the latest NVIDIA AI Enterprise (NVAIE) vGPU profiles. See 
the NVIDIA Virtual GPU Software Supported GPUs documentation for guidance.
•
NVIDIA vGPU host driver (including the VIB file for ESX hosts), that is compatible with 
your VCF version. See Virtual GPU Software for VMware vSphere Release Notes.
•
NVIDIA GPU Operator that is compatible with the Kubernetes version of the deployed 
VKS clusters. See NVIDIA GPU Operator Release Notes and VMware VKrs Release 
Notes.
GPU in passthrough mode
NVIDIA GPU Operator that is compatible with the Kubernetes version of the deployed VKS 
clusters. See NVIDIA GPU Operator Release Notes and VMware VKrs Release Notes.
If required for your AI workloads. 
Network adapters for Advanced NIC 
Passthrough
Use the NVIDIA ESX inbox driver for VMware vSphere (nmlx5-core and nmlx5-rdma) 
that is compatible with your VCF version. See Broadcom Compatibility Guide for I/O Devices 
for Enhanced DirectPath. Use the version of the nmxcli tool that is compatible with ESX 9.1 
to configure Enhanced DirectPath I/O. See NVIDIA Management Tools.
VMware by Broadcom
63


---

VMware Private AI Foundation with NVIDIA 9.1
Required VCF Setup
Before you deploy VMware Private AI Foundation with NVIDIA, a specific configuration must be available in VCF.
•
VCF 9.1.x
•
A VCF license, VMware Private AI Foundation with NVIDIA add-on license, and NVIDIA vGPU license if you plan to 
run AI workloads on vGPU. See Licensing and License Management below.
•
VCF Single Sign-On configured on the VCF Instance containing the GPU-enabled ESX hosts for the workload domain. 
See
Configure a New VCF Single Sign-On for a VCF Instance in the VCF documentation.
•
The VIB file of the NVIDIA vGPU host driver downloaded from https://nvid.nvidia.com/.
•
A vSphere Lifecycle Manager image with the VIB file of the vGPU host manager driver available in SDDC Manager. 
See Managing vSphere Lifecycle Manager Images in VMware Cloud Foundation.
•
At least 3 GPU-enabled ESX hosts to include in the initial cluster of a workload domain.
•
For NVIDIA vGPU, NVIDIA vGPU host driver installed and vGPU configured on each ESX host in the cluster for AI 
workloads. See Configuring vGPU on the ESX Hosts in a GPU-Accelerated Cluster below.
•
For Advanced NIC Passthrough, Mellanox Native ESX Drivers for VMware vSphere ( nmlx5-core and nmlx5-rdma) 
installed on the ESX hosts in the cluster for AI workloads. See Configuring Advanced NIC Passthrough on the ESX 
Hosts below.
Licensing and License Management
To be able to use the VMware Private AI Foundation with NVIDIA functionality, you must have the following licenses:
•
A VCF subscription
For more information about licensing management in VCF, see Licensing in the VCF documentation.
•
A VMware Private AI Foundation with NVIDIA add-on license
Even if VMware Private AI Foundation with NVIDIA is available as part of VCF for your geographical region, you still 
need a VMware Private AI Foundation with NVIDIA add-on license to access the following functionality:
– Private AI setup in VCF Automation for catalog items for easy provisioning of GPU-accelerated deep learning virtual 
machines and VKS clusters.
– Provisioning of PostgreSQL databases with the pgvector extension with enterprise support in VMware Data 
Services Manager.
– Deploying and using the deep learning virtual machine image delivered by VMware.
– Guided deployment workflow in the vSphere Client.
– Private AI Services
You can deploy AI workloads with and without a Supervisor enabled and use the GPU metrics in vCenter and VCF 
Operations under the VCF license.
You assign your VMware Private AI Foundation with NVIDIA to a GPU-enabled workload domain. Assigning a
VMware Private AI Foundation with NVIDIA license to the management domain is required only to activate the guided 
deployment UI in the vSphere Client.
•
For running AI workloads on vGPU, an NVIDIA AI Enterprise (NVAIE) license that is required for the host driver VIB file 
on ESX hosts and the guest OS drivers.
Configuring vGPU on the ESX Hosts in a GPU-Accelerated Cluster
To be able to use VMware Private AI Foundation with NVIDIA with NVIDIA vGPUs on your ESX hosts, perform the 
following steps:
1. On each ESX host, enable SR-IOV in the BIOS according to the documentation from your hardware vendor and on the 
graphics devices for AI operations by using the VMware Host Client.
2. Install the NVIDIA vGPU host driver on each ESX host.
Add the VIB file of the driver to the vSphere Lifecycle image for the cluster and remediate the hosts.
3. If you want to use the Multi-Instance GPU (MIG) sharing, enable it on each ESX host in the cluster.
See NVIDIA MIG User Guide.
VMware by Broadcom
64


---

VMware Private AI Foundation with NVIDIA 9.1
Configuring Advanced NIC Passthrough on the ESX Hosts
If required for your AI workloads, to be able to use VMware Private AI Foundation with NVIDIA with Advanced NIC 
Passthrough on your ESX hosts, perform the following steps:
1. Verify that the NVIDIA firmware on the hosts is up to date. See https://network.nvidia.com/support/firmware/firmware-
downloads/.
2. Verify that the VIB file with the Mellanox driver compatible with the VCF version is installed on each host. See
Broadcom Compatibility Guide for I/O Devices for Uniform Passthrough and Enhanced DirectPath and https://
docs.nvidia.com/networking/display/vmwareesxiinboxdriversreleasenotes.
3. Download the Mellanox NIC ESX Management Tools extension, NMLXCLI, to ESXCLI from https://network.nvidia.com/
products/ethernet-drivers/vmware/esxi-server/ and install it on the hosts with network adapters that are compatible with 
Advanced NIC Passthrough.
4. On each ESX host, enable SR-IOV in the BIOS and on the physical adapters on the hosts, for example, by using the 
VMware Host Client.
By default, half the VFs that you set are allocated to Enhanced DirectPath and the other half - to UPTv2.
5. Log in to an ESX host console over SSH.
6. Set the nvconfig profile of the accelerated NICs.
a. Get the PCI address, such as, 0000:4b:00.0 or 0000:4b:00.1, for the NICs, by running the following command.
Note the PCI Device value for the NIC in the output.
esxcli network nic list
b. For a ConnectX NIC, run the following commands.
You reset the NIC to its default state and you then apply the ConnectX profile.
/opt/mellanox/bin/nmlxcli --sbdf 0000:4b:00.0 --nv-config-profile 0  
/opt/mellanox/bin/nmlxcli --sbdf 0000:4b:00.0 --nv-config-profile 3 
c. For a BlueField DPU, run the following commands.
You reset the DPU to its default state and you then set it in NIC mode.
/opt/mellanox/bin/nmlxcli --sbdf 0000:4b:00.0 --nv-config-profile 0  
/opt/mellanox/bin/nmlxcli --sbdf 0000:4b:00.0 --nv-config-profile 2 
d. Restart the host to apply the configuration.
7. Repeat the step to configure the accelerated NICs on the other hosts.
Assign a VMware Private AI Foundation with NVIDIA License to the 
Management Domain
To unlock the functionality of VMware Private AI Foundation with NVIDIA, you must add a VMware Private AI Foundation 
with NVIDIA license to your VCF environment. In addition, activating the guided deployment UI the vSphere Client and 
quickstart wizard in VCF Automation requires assigning the license to the management domain.
•
Verify that the VCF Operations instance is registered with the VCF Business Services console.
•
Verify that the VMware Private AI Foundation with NVIDIA license is added to the VCF Operations instance in the VCF 
Business Services console.
•
In a disconnected environment, verify that you downloaded the license file that contains the VMware Private AI 
Foundation with NVIDIA license.
For information about license management in VCF, see Licensing in the VCF documentation.
Note:
The license capacity is allocated only to the GPU-enabled workload domains and not to the management domain.
For information about license management in VCF, see Licensing in the VCF documentation.
1. Register your VCF Operations instance in the VCF Business Console.
In a disconnected environment, generate a license file on the VCF Business Console and import it in VCF Operations. 
You can combine a VCF, VMware Private AI Foundation with NVIDIA, and other licenses in one file.
VMware by Broadcom
65


---

VMware Private AI Foundation with NVIDIA 9.1
See Register VCF Operations in Connected Mode and Register VCF Operations in Disconnected Mode in the VCF
documentation.
2. If you have not done already so, assign your VCF license as primary to the management domain vCenter.
See Assign a Primary License to a vCenter Instance in the VCF documentation.
3. Assign the VMware Private AI Foundation with NVIDIA as an add-on license to the management domain vCenter.
See Assign an Add-on License to a vCenter Instance in the VCF documentation.
Set up VMware Private AI Foundation with NVIDIA in your VCF environment by using the guided deployment UI in the 
vSphere Client. For information on the required steps, see the instructions that are next.
Follow the Guided VMware Private AI Foundation with NVIDIA Setup in 
the vSphere Client
You can fully set up the VMware Private AI Foundation with NVIDIA components by using the guided deployment UI in the 
vSphere Client. The guided deployment UI connects to SDDC Manager to perform the requested operations.
Before you begin:
•
Verify that you have an administrator account that is added to the management vCenter and to SDDC Manager.
•
Assign a License to the Management Domain.
1. Log in to the management vCenter by using the vSphere Client at https://<management_vcenter>/ui as
administrator@vsphere.local.
2. In the vSphere Client side panel, click Private AI Foundation.
3. In an environment running VCF, follow the guided deployment to complete the setup of VMware Private AI Foundation 
with NVIDIA.
For information on the required steps, see the instructions that are next in Preparing for Private AI Workload 
Deployment.
Deploy a Workload Domain for VMware Private AI Foundation with 
NVIDIA
As a VI administrator, in VCF, deploy a workload domain with the GPU-accelerated ESX where data scientists, MLOps 
engineers, and DevOps engineers will run AI workloads. If you plan to run Private AI Services on CPU, GPU resources 
are not required.
•
Verify that your environment satisfies Requirements for Deploying VMware Private AI Foundation with NVIDIA.
•
Verify that your environment satisfies the requirements for deploying a workload domain. See Create a New Workload 
Domain Using VCF Operations.
•
Verify that your environment satisfies the requirements for deploying an NSX Edge cluster or Virtual Network Appliance 
cluster. See .
•
Assign a VMware Private AI Foundation with NVIDIA License to the Management Domain.
The vCenter instance for the workload domain is deployed in the vCenter Single Sign-On domain of the management 
domain.
Note:
If you plan to use hosts with Advanced NIC Passthrough enabled as the first cluster of the workload domain, you 
must create a workload domain without an initial cluster, only with management infrastructure. See Create a New 
Workload Domain without a vSphere Cluster Using VCF Operations in the VCF documentation. This setup is 
required because you add the hosts to a vSphere Distributed Switch with performance NIC offloading manually. 
For more details on the setup, see Add a vSphere Cluster with Advanced NIC Passthrough to a Workload Domain 
in VMware Private AI Foundation with NVIDIA.
VMware by Broadcom
66


---

VMware Private AI Foundation with NVIDIA 9.1
1. Log in to the vCenter instance for the management domain at https://<management_vcenter_fqdn>/ui.
2. Select vSphere Client > Private AI Foundation.
3. In the Private AI Foundation workflow, click the Set Up a Workload Domain section.
4. If you do not have one for the workload domain hosts, create a network pool to have static IP addresses automatically 
assigned to vSAN, NFS, iSCSI, and vMotion VMkernel ports of the ESX hosts in the workload domain.
See Network Pool Management in the VCF documentation. The wizard in the guided deployment workflow has the 
same options as the analogous wizard in the vSphere Client hosts inventory.
5. Commission the ESX hosts to add them to the inventory of SDDC Manager.
See Commission Hosts in the VCF documentation. The wizard in the guided deployment workflow has the same 
options as the analogous wizard in the Hosts and Clusters inventory in the vSphere Client.
6. Deploy the workload domain with a Supervisor instance.
The Supervisor is available on the domain's default cluster.
a) Configure storage, networking, and compute, and vCenter deployment.
The wizard in the guided deployment workflow provides the same options as in the VCF Operations UI except for 
the following settings that are specific to VMware Private AI Foundation with NVIDIA:
•
The vSphere Supervisor option is mandatory.
•
The workload domain is deployed as isolated. You must later add it to VCF Single Sign-on and to a vCenter
group in vCenter Linking by using VCF Operations.
•
You can use centralized connectivity, based on an NSX Edge cluster, and distributed connectivity, requiring a 
Virtual Network Appliance cluster.
You deploy the NSX Edge cluster after you create the workload domain. However, deploying the Virtual Network 
Appliance cluster is a part of workload domain deployment and you set its configuration in the wizard.
•
For running AI workloads on vGPU, select hosts whose NVIDIA vGPU state is Ready.
For more information on deploying a workload domain, see Create a New Workload Domain Using VCF 
Operations in the VCF documentation.
b) Configure the Supervisor deployment on the default cluster of the workload domain.
If you have selected centralized connectivity, Supervisor enablement remains in progress until you deploy an NSX 
Edge cluster in the workload domain and set up a Tier-0 gateway according to the requirements.
•
The wizard for workload domain creation deploys the Supervisor instance with a single zone. You can add 
clusters to the workload domain and make them zones later.
•
You use static IP address assignment for the management network.
c) Track the deployment process in the Build > Tasks area of VCF Operations and wait until it is completed.
7. If you configured the workload domain with centralized connectivity, deploy the NSX Edge cluster in the workload 
domain and create a Tier-0 gateway in active-standby mode for workload domain connectivity.
The wizard in the guided deployment workflow has the same options as the analogous wizard in the Hosts and 
Clusters inventory in the vSphere Client.
•
For information about how to add the NSX Edge cluster in the vSphere Client, see Managing Centralized Network 
Connectivity with Edge Clusters in the VCF documentation.
•
For information about the settings of the NSX Edge cluster and the Tier-0 gateway for a Supervisor deployment, 
see Configure and Deploy an NSX Edge Transport Node for Supervisor Deployment in the VCF documentation.
The Supervisor instance is automatically connected to the Tier-0 gateway you created during NSX Edge cluster 
deployment.
VMware by Broadcom
67


---

VMware Private AI Foundation with NVIDIA 9.1
8. In VCF Operations, connect vCenter and NSX Manager for the workload domain to the VCF Identity Broker instance 
for the VCF Instance.
a) In VCF Operations, on the Manage tab at the top, click Identity & Access.
b) Select the VCF Instance that the workload domain is a part of.
c) On the Component Configuration tab, select the vCenter and NSX Manager instances for the workload domain 
and click Configure component.
See Configure VCF Single Sign-On for NSX and vCenter in the VCF documentation.
9. In VCF Operations, add the vCenter instance for the workload domain to the vCenter group of the management
vCenter instance.
a) In VCF Operations, on the Operate tab at the top, click Administration > Configurations, and under Logical 
Groupings, click the vCenter Linking card.
b) If a group with the management vCenter instance is available, click the name of the group and add the workload 
domain vCenter instance.
c) If the management vCenter instance is not a member of a vCenter group, create a group, adding the management 
and workload domain vCenter instances to it.
See Linking vCenter Instances in VCF Operations in the VCF documentation.
10. For running AI workloads on vGPU, in the vSphere Client, on the vCenter instance for the workload domain, set the
vgpu.hotmigrate.enabled advanced setting to true so that virtual machines with vGPU can be migrated by 
using vSphere vMotion.
a) In the Hosts and Clusters inventory, navigate to the vCenter instance.
b) On the Configure tab, click Advanced Settings and click Edit Settings.
c) Set the vgpu.hotmigrate.enabled setting to Enabled and click Save.
11. Assign the VMware Private AI Foundation with NVIDIA license to the workload domain.
a) Verify that the vCenter instance for the GPU-accelerated workload domain and the management domain are 
connected in a vCenter group in vCenter Linking.
The license capacity is allocated only to the GPU-accelerated workload domain.
b) In VCF Operations, on the Manage tab at the top, click Licensing > Licenses & Registration.
c) On the Version 9+ tab, from the vCenter Systems table, select the vCenter instance for the workload domain and 
click Assign Add-on License.
d) Select the VMware Private AI Foundation with NVIDIA license, and click Assign.
12. If required by your organization, add GPU-accelerated clusters to the workload domain and make them workload 
zones in the Supervisor instance.
For information about adding a cluster to a workload domain, see Add an SDDC Cluster to a VCF Domain in the VCF
documentation.
For information about adding a workload zone to a Supervisor instance, see Add a Cluster as a vSphere Zone to a 
Supervisor in the VCF documentation.
Configure NVIDIA vGPU or GPU Passthrough for AI Workloads on the 
ESX Hosts
As a VI administrator, you configure the access to the GPU resources in the ESX hosts in the workload domain so that 
GPU is available for use in AI workloads over passthrough or as vGPUs.
•
Verify that your environment satisfies Requirements for Deploying VMware Private AI Foundation with NVIDIA.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for 
deep learning VMs and VKS clusters with GPU. See Setup Workflows for Deep Learning VMs and VKS Clusters with 
GPU or Setup Workflows for Private AI Services.
VMware by Broadcom
68


---

VMware Private AI Foundation with NVIDIA 9.1
1. Log in to the vCenter instance for the workload domain at https://<workload_domain_vcenter_fqdn>/ui.
2. In the Hosts and Clusters inventory, click the ESX host that has the NVIDIA GPU devices installed.
3. If the workloads in your environment will use NVIDIA GPU passthrough, turn on Fixed DirectPath or Dynamic 
DirectPath for the NVDIA GPU devices.
a) On the Configure tab for the host, under Hardware, click DirectPath.
b) Select the box for a GPU device and click Configure DirectPath Mode.
c) Select Fixed/Dynamic DirectPath and click Configure.
d) Repeat the steps for the other GPU devices on the host.
e) Place the host in maintenance mode and restart it.
4. If the workloads in your environment will use NVIDIA vGPUs, turn on Shared Direct on the GPU devices.
a) On the Configure tab for the host, under Hardware, click Graphics.
b) Under Host Graphics, select the box for a GPU device and click Edit.
c) In the Edit Host Graphics Settings dialog box, set the following configuration.
Setting
Value
Device Type
Shared Direct
vGPU Mode
For dual-purpose GPUs. Activate a mode according to your requirements.
vGPU profiles on a single physical GPU can have different types or sizes. For example, you 
can run vGPU VM profiles with 16-GB compute for compute-intensive work like machine 
learning (C-type vGPU profile) and 4-GB graphics (Q-type vGPU profile) simultaneously on 
a single device. Activating this feature reduces the maximum number of instances of some 
profiles.
d) Repeat the steps for the other GPU devices on the host.
e) Place the host in maintenance mode and restart it.
5. If you want to switch from MIG to passthrough mode on an ESX host, turn off MIG mode for all the GPUs on the host 
and uninstall the vGPU host driver.
a) Place the host in maintenance mode.
b) Log in to the command line interface of the ESX host.
c) Get the GPU IDs from the output of the nvidia-smi command.
nvidia-smi
d) For each GPU, turn off MIG mode and verify.
If you uninstalled the NVIDIA vGPU host driver before you turn MIG mode off for all GPUs, then you can create a 
VM with nvidia-smi command on the host, and run the nvidia-smi -i <GPU_ID> -mig 0 command from 
within the VM to turn MIG mode off for the GPUs.
nvidia-smi -i <gpu_id> -mig 0
Verify the result by running the following command:
nvidia-smi -i <gpu_id>
e) Restart the host.
reboot 
f)
Perform Step 3 to set up NVIDIA GPU passthrough for AI workloads on the host.
6. Repeat the procedure for the other ESX hosts in the cluster.
Add a vSphere Cluster with Advanced NIC Passthrough to a Workload 
Domain in VMware Private AI Foundation with NVIDIA
To use the Enhanced DirectPath I/O NIC offload mode, you manually add a cluster of ESX hosts to a dedicated vSphere 
Distributed Switch in vCenter and a dedicated transport node profile in NSX Manager, with Advanced NIC Offload and 
Enhanced Datapath enabled.
VMware by Broadcom
69


---

VMware Private AI Foundation with NVIDIA 9.1
•
Verify that your environment satisfies Requirements for Deploying VMware Private AI Foundation with NVIDIA.
– On the hosts, SR-IOV must be active for the ConnectX and BlueField network adapters. The firmware must be 
configured with DirectPath I/O profile.
– The cluster of hosts must be added to the vCenter instance for the workload domain.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for 
deep learning VMs and VKS clusters with GPU. See Setup Workflows for Deep Learning VMs and VKS Clusters with 
GPU or Setup Workflows for Private AI Services.
•
If the vSphere cluster is going to be the first cluster in a workload domain, create a workload domain without an initial 
cluster of hosts, only with management infrastructure. See Create a New Workload Domain without a vSphere Cluster 
Using VCF Operations in the VCF documentation.
Note:
You can use the performance NIC offload functionality only when provisioning deep learning VMs directly on a 
vSphere cluster or by using kubectl and when provisioning a VKS cluster by using kubectl.
For information about Advanced NIC Passthrough, see Enhanced DirectPath I/O for Accelerated NICs.
You do not use the guided deployment UI in the vSphere Client or the automated workflow for adding SDDC clusters in 
the vSphere Client.
Note:
You can use Enhanced DirectPath I/O only with RDMA over Converged Ethernet (RoCE). Using Enhanced 
DirectPath I/O with InfiniBand is not supported.
1. Log in to the vCenter instance for the workload domain at https://<workload_vcenter_fqdn>/ui by using the 
vSphere Client.
2. In the Networking inventory, create a vSphere Distributed Switch with version 9.1.0 and Network Offloads 
compatibility set to Advanced NIC Passthrough.
You can add only hosts that are compatible with Advanced NIC Passthrough.
See Create a vSphere Distributed Switch in the vSphere documentation.
3. Add the hosts with the distributed switch, selecting the smart NIC physical adapters.
You can select only physical adapters with active SR-IOV.
See Add Hosts to a vSphere Distributed Switch in the vSphere documentation.
4. Log in to the NSX Manager for the workload domain at https://<workload_nsx_manager_vip_fqdn>.
5. On the System tab, go to Fabric > Hosts and under Applied Profile note the transport node profile name.
6. On the Transport Node Profile tab, create a transport node profile and configure the following Host Switch settings.
a) In the Host Switch dialog box, select the vCenter instance for the workload domain and the vSphere Distributed 
Switch with network offloading activated that you created.
b) In the Advanced Configuration section of settings, set Mode to Enhanced Datapath - Standard and activate
Advanced NIC Passthrough.
Configure the rest of the transport node profile settings according to the requirements of your organization.
See Add a Transport Node Profile in the VCF documentation.
c) Click Apply.
7. Apply the transport node profile to the hosts that are connected to the distributed switch.
a) Back on the Hosts page, on the Clusters tab, click the vertical ellipsis drop-down menu and select Configure 
NSX.
See Prepare ESX Cluster Hosts as Transport Nodes by Using TNP in the VCF documentation.
b) Select the transport node profile for NIC offloading and click Save.
•
Аdd a PCI Passthrough network adapter to the VM class. See Configure VM Classes for AI Workloads for VMware 
Private AI Foundation with NVIDIA.
VMware by Broadcom
70


---

VMware Private AI Foundation with NVIDIA 9.1
•
For deep learning VMs, in the guest OS, install the NVIDIA guest OS driver that is compatible with the ConnectX or 
BlueField device.
Configure VM Classes for AI Workloads for VMware Private AI 
Foundation with NVIDIA
In these VM classes, as a VI administrator, you set the compute, networking, and GPU requirements according to the 
GPU configuration on the ESX hosts in the Supervisor cluster.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for 
deep learning VMs and VKS clusters with GPU. See Setup Workflows for Deep Learning VMs and VKS Clusters with 
GPU or Setup Workflows for Private AI Services.
•
To use Advanced NIC Passthrough with Enhanced DirectPath I/O, verify that you have a cluster with compatible 
accelerated NICs configured. See Add a vSphere Cluster with Advanced NIC Passthrough to a Workload Domain in 
VMware Private AI Foundation with NVIDIA.
Then, you can reserve capacity for VMs in the zones of the Supervisor instance so that the resources required by the VMs 
are always available and the VMs can be powered off and then back on successfully.
You add NVIDIA vGPUs or GPU devices in passthrough mode as PCI devices. If you plan to run models in Private AI 
Services on CPU using llama.cpp, set up CPU-based VM classes.
You can configure individual workloads for Advanced NIC Passthrough with Enhanced DirectPath I/O using GPUDirect 
RDMA communication. See Physical Infrastructure Resources for AI Workloads in VMware Private AI Foundation with 
NVIDIA.
1. Log in to the vCenter instance for the management domain at https://<management_vcenter_fqdn>/ui.
2. For VM classes with GPU reservation, create a DirectPath Profile for the GPU device on the vCenter instance for the 
workload domain.
a) In the vSphere Client side panel, click Policies and Profiles.
b) On the Policies and Profiles page, click DirectPath Profiles and click Create.
c) On the vCenter Server and DirectPath Device page of the wizard, configure the following settings, and click
Next.
•
Select the vCenter instance for the workload domain.
•
Select the underlying NVIDIA vGPU device or GPU device in passthrough mode available on the vCenter
instance.
d) On the Name and Configuration page, enter a name and a description, and click Finish.
3. If you plan to run AI workloads on GPU, create the VM classes with vGPUs or GPU passthrough.
If you plan to run models in Private AI Services on CPU, make sure that enough CPU resources are allocated in the 
VM class.
a) In the vSphere Client side panel, click Private AI Foundation.
b) In the Private AI Foundation workflow, in the Set Up Environment card, click Start, and next to Create VM 
classes with NVIDIA vGPUs, click Start.
c) Select the GPU-accelerated workload domain and click Next.
d) Configure the VM class that defines the requirements for virtual machines that are associated with it in the 
following way.
Table 11: Common VM Class Settings
Common VM Class Settings
Specific Guidelines for VMware Private AI Foundation with 
NVIDIA
Name and description
-
Compatibility
ESX 9.0 and later
VMware by Broadcom
71


---

VMware Private AI Foundation with NVIDIA 9.1
Common VM Class Settings
Specific Guidelines for VMware Private AI Foundation with 
NVIDIA
Virtual Hardware > CPU
Virtual Hardware > Memory
For a VM class with GPU reservation, enter the same non-zero 
values for reservation and limit for the required CPU resource and 
for the required memory resources.
Virtual Hardware > Add New Device > PCI Device
NVIDIA vGPU profile or GPU passthrough profile.
Virtual Hardware > Add New Device > Network Adapter > PCI 
Device passthrough
Network adapter with Enhanced DirectPath I/O on an accelerated 
NIC.
1.
Click Change Device and select the accelerated NIC.
The access type for the NIC appears as Enhanced 
DirectPath.
2.
Reserve all guest memory.
Advanced Parameters
For a GPU device in passthrough mode that maps more than 16 
GB of memory regions, configure that the following settings if you 
want to map system memory to GPU frame buffer memory that is 
smaller than 512 GB:
•
Set pciPassthru.use64bitMMIO is set to true.
•
Set pciPassthru.use64bitMMIOSizeGB to <n> if 
needed.
You calculate the value of the pciPassthru.use64bitM 
MIOSizeGB parameter in the following way:
a.
Count the number of passthrough GPU devices that you 
intend to require in the VM class.
b.
Multiply that number by the frame buffer memory of the 
individual GPU device.
c.
If the total GPU frame buffer memory is equal to a power-
of-two number, set the parameter to the next power-of-two 
number.
d.
If the total GPU frame buffer memory is between two 
powers-of-two numbers, round up to the next power-of-two 
number and then again to the next power-of-two number.
For example, for a VM class with two A100 GPU devices in 
passthrough mode, the total GPU frame buffer memory is
2 * 40 = 80 GB. You round it up to 128 GB and 
then again to 256 GB. You set pciPassthru.use64 
bitMMIOSizeGB to 256.
4. For VM classes with GPU reservation, set the capacity for a zone in the Supervisor.
a) In the vSphere Client side panel, click Supervisor Management.
b) On the Supervisor tab, click the Supervisor instance.
c) On the Zones tab, click the vertical ellipsis menu and select Add Reserved VM Class.
d) Select a VM classes with a vGPU or GPU passthrough profile and set the number of VM class instances to be 
reserved in the zone.
Later, you can distribute these instances to organizations in VCF Automation.
e) Repeat the step for other VM classes.
f)
Click Add Reservation.
Allocate the reserved VM classes to an organization in VCF Automation. See Setting Up VCF Automation for Private AI 
Foundation with NVIDIA.
Configure a Content Library with Ubuntu VKr for a Disconnected
VMware Private AI Foundation with NVIDIA Environment
As a cloud administrator, if your environment has no Internet connectivity, you provide a local content library where you 
manually upload vSphere Kubernetes releases (VKr) and associate it with the Supervisor.
VMware by Broadcom
72


---

VMware Private AI Foundation with NVIDIA 9.1
Deploying NVIDIA-aware AI workloads on VKS clusters requires the use of the Ubuntu edition of vSphere Kubernetes
releases.
CAUTION:
The VKr content library is used across all vSphere namespaces in the Supervisor when you provision new VKS 
clusters.
Prerequisites
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for 
deep learning VMs and VKS clusters with GPU. See Setup Workflows for Deep Learning VMs and VKS Clusters with 
GPU or Setup Workflows for Private AI Services.
Procedure
1. Download the Ubuntu-based VKr images with the required Kubernetes versions.
– For VCF 9.1.0, download the Ubuntu-based VKr images with the required Kubernetes versions from https://wp-
content.vmware.com/v2/latest/.
– For VCF 9.1.1, pull the available VKr images from the Broadcom central portal in a software depot in offline or 
disconnected mode by using the VCF Download Tool.
See Download Binaries to an Offline Depot by Using the VCF Download Tool and Download Binaries to Software 
Depot in Disconnected Mode by Using the VCF Download Tool.
2. Log in to the vCenter instance for the management domain at https://<vcenter_fqdn>/ui as
administrator@vsphere.local.
3. In the vSphere Client side panel, click Private AI Foundation.
4. In the Private AI Foundation workflow, select Cloud disconnected at the top, and in the Set Up Environment
section, click Start.
5. Create a content library with the VKr images.
– For VCF 9.1.0, create a local content library and upload the VKr images into it.
See Create a Content Library for an Air-Gapped Environment in the VMware Cloud Foundation Service 
Administration and Development 9.1 documentation.
– For VCF 9.1.1, create a subscribed content library for the VKr images in the software depot in your VCF
environment.
Set the subscription URL to https://depot-fqdn/depot-service/content-gateway/PROD/COMP/VKR/
lib.json.
The wizard in the guided deployment workflow is the same as the analogous wizard in the Content Libraries area of 
the vSphere Client.
Setting Up VCF Automation for VMware Private AI Foundation with 
NVIDIA
VCF Automation provides support for self-service catalog items that DevOps engineers and data scientists can use to 
provision AI workloads in VMware Private AI Foundation with NVIDIA in a user-friendly and customizable way.
Note:
•
Starting with version 3.0, you can also configure Private AI Services and develop generative AI applications by 
using the vSphere Client.
•
You can alternatively use provision AI workloads by using kubectl.
1. Set up your VCF Automation organization for VMware Private AI Foundation with NVIDIA.
2. Activate Private AI Services in a namespace in your VCF Automation organization. See Activate Private AI Services 
3.0 in an Organization Namespace by Using the UI and Activate Private AI Services 2.1 in an Organization 
Namespace by Using the VCF Automation UI.
3. Add the catalog items for provisioning AI applications by running the Private AI Foundation Quickstart in VCF 
Automation. See Create AI Catalog Items in VCF Automation using the Private AI Foundation Quickstart.
VMware by Broadcom
73


---

VMware Private AI Foundation with NVIDIA 9.1
Set Up Your VCF Automation Organization for VMware Private AI Foundation 
with NVIDIA
As a VCF Automation administrator, you configure your VCF Automation organization so that you can create catalog items 
for provisioning AI applications.
•
Verify that your vCenter instance has a Supervisor with GPUs.
•
Verify that you configured the necessary VM classes according to your GPU configuration. See Configure VM Classes 
for AI Workloads for VMware Private AI Foundation with NVIDIA.
– If you intend to use GPU reservations, ensure that you defined VM classes with GPU reservations.
•
Ensure that there is unreserved GPU capacity in the vSphere cluster to accommodate both planned upgrades 
and unplanned host failover. For example, in a 16-host cluster, ensure that one host always serves as a 
dedicated failover host and the remaining hosts handle zonal GPU reservations without exceeding the capacity 
of 14 hosts.
– If you intend to use regular VM classes without any GPU reservations, ensure that a VM class is defined with the 
right vGPU profiles.
•
Verify that the matching NSX Manager for the Supervisor is registered in your VCF Automation environment.
•
Verify that the zones status is Ready in VCF Automation Provider Management. If zones are not ready, VCF 
Automation cannot correctly display the CPU and memory capacity.
•
Verify that you have created an All Apps organization in the VCF Automation Provider Management UI with region 
quota in the zone which has GPUs.
•
Verify that you configured VPC networking for the organization with a Public IP range, a transit gateway IP range, and 
a VPC private IP range.
•
Optionally, if you plan to use the VMware Data Services Manager catalog item, verify that the VMware Data Services 
Manager Consumption operator is configured in vCenter and that the VMware Data Services Manager plug-in is 
configured in the VCF Automation Provider Management UI.
•
Verify that the option to create subscribed content libraries is enabled for the organization in the VCF Automation
Provider Management UI.
•
To use a local content library for the VM images in a disconnected environment, download the deep learning VM .ova
image from the VMware Private AI area on the Broadcom Support Portal.
VCF Automation discovers all supervisors, zones, VM classes, storage policies, and reserved VM Classes for a given 
vCenter instance. Data in VCF Automation is refreshed every 15 minutes. If you, for example, add or remove a reserved 
VM class from vCenter or update the count for a reserved VM class, and the new values are not showing in VCF 
Automation, you can sync manually by clicking the Refresh option for the vCenter instance in the VCF Automation
Provider Management UI.
In VCF Automation Provider Management, you, as a provider administrator, configure a region and assign region quota 
for the organization. The procedure described here uses only one region but you can create multiple regions and create 
quotas for each.
Region quotas are based on the zones you select for one or multiple supervisors in a region. If a region contains multiple 
supervisors from different vCenter instances, zones with the same name are aggregated for their CPU, memory, and 
reserved VM class resources and are presented as a single unit at the region level.
VMware by Broadcom
74


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 9: Namespaces with in Multi-Cluster Aggregated Zones
Namespace (VCF Automation Organization Management
)
Region (VCF Automation Provider Management
)
Workload Domain / vCenter
Supervisor 1
zone 1
h100-4c (8)
h100-40 c (4)
l40 -10c (2)
l40 -20c (2)
CPU: 200 GHz
Memory : 200 GB
zone 2
l40 -10c (4)
l40 -20c (2)
CPU: 100 GHz
Memory : 150 GB
h100-40 c (4)
h100-4c (8)
l40 -10c (2)
l40 -20c (2)
cluster 1
cluster 2
cluster 3
cluster 4
l40 -10c (4)
l40 -20c (2)
Workload Domain / vCenter
Supervisor 1
zone 1
s2-h100-4c (1)
s2-h100-4c (1)
cluster 5
cluster 6
cluster 7
cluster 8
s2-l40 -10c (1)
zone 3
zone 2
s2-l40 -10c (1)
zone 4
CPU: 50 GHz
Memory : 100 GB
CPU: 50 GHz
Memory : 50 GB
zone 1 (aggregated )
h100-4c (8)
h100-40 c (4)
l40 -10c (2)
l40 -20c (2)
CPU: 250 GHz
Memory : 300 GB
s2-h100-4c (1)
zone 2 (aggregated )
l40 -10c (4)
l40 -20c (2)
CPU: 150 GHz
Memory : 200 GB
s2-l40 -10c (1)
Region Quota
zone 1
CPU Reservation : 220 GHz
Memory Reservation : 250 GB
h100-4c (4), h100-40 c (2),
l40 -10c (2), l40-20c (2)
zone 2
CPU Reservation : 120 GHz
Memory Reservation : 150 GB
l40 -10c (4), l40-20c (2)
zone 1
h100-40 c (2)
h100-4c (4)
CPU Reservation : 200 GHz
Memory Reservation : 200 GB
zone 2
CPU Reservation : 100 GHz
Memory Reservation : 150 GB
Max CPU Reservation : 200 GHz
Max Memory Reservation : 200 GB
Max CPU Reservation : 100 GHz
Max Memory Reservation : 150 GB
h100-4c (8)
s2-h100-4c (1)
Assigned to Region Quota
Not Assigned to Region Quota
Legend :
VMware by Broadcom
75


---

VMware Private AI Foundation with NVIDIA 9.1
Each zone can contain one or multiple vSphere clusters. If you have GPUs spread across multiple clusters, ensure that 
you group GPUs of similar type in each cluster. For example, one cluster can contain only H100 GPUs and another cluster 
can contain only L40 GPUs. You can combine vGPUs and passthrough GPUs in the same cluster. Because a namespace 
is created on an individual cluster, if you want to create a namespace with GPU reservations, the reserved VM classes 
must contain GPUs of the same type and must be on the same cluster.
For each zone, you specify CPU and memory limits and reservations and reserved or non-reserved VM classes. You 
cannot assign unlimited resources to the organization. While CPU and memory limits can be over-provisioned, CPU and 
memory reservations cannot be over-provisioned. Storage can be over-provisioned. For reserved VM classes, the count 
given to the organization for each VM class must fit the capacity of that zone on the supervisor, and not the aggregated 
capacity of the zone.
In the VCF Automation organization, you create a dedicated project with a namespace where VMware Private AI 
Foundation with NVIDIA images and VM classes are available. The namespace is based on a namespace class, a VPC, 
and is associated with at least one zone.
The namespace class can define non-GPU classes and either non-reserved GPU-enabled VM classes or reserved GPU-
enabled VM classes. It cannot contain both non-reserved and reserved GPU-enabled VM classes.
•
Regular VM classes that are not reserved in the vSphere zone, the region quota for the organization, or the 
namespace, are considered non-reserved. If a VM class is reserved in the region quota, then it becomes a reserved 
VM class. If the namespace includes both a non-reserved GPU class and a reserved GPU VM class, VCF Automation
assumes it is a reserved VM class namespace and using the non-reserved GPU class to request Private AI workloads 
fails.
•
Non-reserved VM classes are fungible and are available in all vCenter instances in a region.
•
VM classes that specify GPU reservations ensure that only a particular namespace uses the GPU. Reserved GPU VM 
classes do not have to be fungible and can be confined to a single zone or supervisor.
GPU-capable VM classes can support the following GPU modes:
•
VM classes with vGPUs support use cases where the GPU must be shared among several workloads or when pooling 
multiple GPUs on an HGX system.
•
VM classes with GPU passthrough support use cases where an entire GPU is required to run a single VM. Such VM 
classes must be configured in the region quota for the organization.
You can create a namespace with or without GPU reservations. When a namespace is created with GPU reservations, the 
system performs the following checks:
1. The reservation and limits values and GPU count for a namespace are checked against what is available for the 
selected zone.
a. If non-zero memory reservation is specified, at least 2 GB must be specified to account for VM guest memory 
overhead.
b. If zero reservation is specified, then VM classes with reservation configured are ignored.
2. The payload with the zone and reservation values are sent to VCF Automation Provider Management, which then 
makes a request to the supervisor to create the namespace in the specified zones.
3. In vCenter, a resource pool is created for the namespace in the clusters corresponding to the specified zones, and all 
the reservation and limit values are set on the resource pool.
AI/ML workloads with GPU accelerators require VM classes with full memory reservations.
For information on how to set up a namespace with reservations, you can also watch the following video.
1. Log in to VCF Automation Provider Management as a provider administrator.
2. Create a region.
a) Select Infrastructure > Regions and click Create Region.
b) Enter a name and, optionally, a description for the region.
For example, you call your region paif-region.
VMware by Broadcom
76


---

VMware Private AI Foundation with NVIDIA 9.1
c) Select the NSX Manager instance that integrates with the vCenter instances you want to use for the region.
A single NSX Manager instance must be integrated with the vCenter instances within the region.
d) Select one or more supervisors.
The list shows all available supervisors for the NSX Manager instance.
e) Select one or more storage classes.
The list shows all storage classes across the selected supervisors.
The storage classes within a region must be present and have the same names across all vCenter instances within 
a region.
f)
Click Submit.
3. Create a region quota.
You can assign the organization quota from a single or multiple supervisors.
a) Select Infrastructure > Organizations and select the organization.
b) Select Region Quota and click New.
c) Select a region.
d) Next to Region Capacity, select Share a portion of the capacity with this organization.
e) If you want to assign specific supervisors to the organization, turn off the Add all available supervisors toggle 
and select one or more supervisors.
f)
Add one or more zones and assign CPU and memory limits and reservations for each zone.
Only finite values are allowed. You cannot assign unlimited resources to the organization.
g) Click Assign and Continue.
h) Select one or more VM classes and storage classes applicable to the organization.
The list shows all non-reserved VM classes and storage classes available in the selected supervisors.
When adding storage classes, you can also edit the storage limit for the storage class.
i)
Click Assign and Continue.
j)
Optional: Select one or more reserved VM classes and assign reservation counts for each reserved class.
You can reserve vGPU VM classes and VM classes with GPU passthrough.
You cannot assign a guaranteed VM class to a region quota that has zero reservation. Guaranteed VM classes are 
not allowed in the quota if the CPU and memory reservations are not given to all zones.
k) Click Continue.
4. Log in to the VCF Automation organization as an organization administrator.
5. Create a content library that contains deep learning VM images.
Deep learning VM images are distributed in a shared content library published by VMware. In VCF Automation, you 
create a content library for deep learning VMs for use in the organization.
Depending on your environment, you might need to create either a subscribed content library or a local content library.
•
For a connected environment, create a subscribed content library that is connected to https://
dl.broadcom.com/<download_token>/PROD/COMP/DLVM/9.0.0.0/lib.json.
•
For a disconnected environment, import the deep learning VM .ova image that you downloaded from the VMware 
Private AI area on the Broadcom Support Portal to your local content library.
a) Select Build and Deploy > Content Hub > Content Libraries, and click Create Content Library.
b) Enter a name.
c) To assign the content library to all current and future namespaces, turn on the toggle.
d) Depending on your environment, decide if you need to subscribe to an external content library.
•
For a connected environment, turn on the Subscribe to an external library toggle and then click Next to 
configure the connection to the shared content library.
VMware by Broadcom
77


---

VMware Private AI Foundation with NVIDIA 9.1
In the Subscribe to an external library section, enter https://dl.broadcom.com/<download_token>/
PROD/COMP/DLVM/9.0.0.0/lib.json as the content library URL. Authentication is not required.
•
For a disconnected environment, leave the Subscribe to an external library toggle off.
e) Click Next.
f)
Select one or more regions and a corresponding storage class for each region.
g) Click Next.
h) Review your settings and click Confirm.
i)
For a disconnected environment, upload the deep learning VM .ova image that you downloaded from the 
Broadcom Support Portal to the local content library that you just created.
i. Select Build & Deploy > Content Hub > VM Images.
ii. Click the arrow next to Select Source and select the deep learning VM .ova image.
iii. Select the content library.
iv. Enter a name for the image.
v. Click Submit.
6. Create a namespace class.
You cannot mix non-reserved and reserved GPU-enabled classes in the same namespace class. Depending on your 
goals, you must create a namespace class using one or the other type of GPU class.
You can create the namespace class using the VCF Automation organization UI or the VCF Automation API.
•
Create a namespace class with reserved or non-reserved GPU classes using the VCF Automation organization UI.
a. Select Manage & Govern > Namespace Class and click New.
b. Enter a name for the namespace class.
c. Define CPU and memory limits and reservations.
d. Add at least one VM class and a storage class.
You can use unbound GPU classes and non-GPU VM classes.
e. Add the content library that you created in Step 5.
f.
Click Create.
•
Create a namespace class with reserved GPU-enabled VM classes using the CLI or the VCF Automation API.
a. On a machine running the VCF Consumption CLI and kubectl, create a context for the organization.
vcf context create cci -e vcf_automation_endpoint -t cci --api-token 
 ${vcf_automation_organization_api_token} --tenant-name us-west --ca-certificate /tmp/vcfa-ca-
cert.pem
vcf context use cci
b. List the regions to which you have access.
kubectl get regions
Verify that you see the region called paif-region that you created in Step 2.
c. List the zones for the region.
kubectl get zones --field-selector spec.regionName=paif-region
d. List the VM classes and storage classes you have access to in the specific region.
kubectl get regionvirtualmachineclasssummaries --field-selector spec.regionName=paif-region
kubectl get regionvirtualmachineclassquotas --field-selector spec.regionName=paif-region
kubectl get regionstorageclassquotas --field-selector spec.regionName=paif-region
e. List the VPCs that are available for the region.
kubectl get vpcs --field-selector spec.regionName=paif-region
f.
Prepare a YAML file, for example, called ns-class.yaml, for creating a new namespace class.
Your ns-class.yaml file might look like the following example.
apiVersion: infrastructure.cci.vmware.com/v1alpha2
kind: SupervisorNamespaceClass
metadata: 
VMware by Broadcom
78


---

VMware Private AI Foundation with NVIDIA 9.1
  name: medium-gpu-workloads
spec: 
  description: supervisor namespace class
g. Prepare a YAML file, for example, called ns-class-cfg.yaml, for configuring the namespace class.
cat ~/Downloads/cci-yaml/ns-class-cfg.yaml
kubectl apply -f ~/Downloads/cci-yaml/ns-class-cfg.yaml
For the namespace class, you configure the following specification:
•
contentSources. The content source is the content library you created in Step 5 that contains the deep 
learning VMs.
•
storageClasses and vmClasses. Specify the storage classes and VM classes that you want your users 
to have access to. If you want to give your users the option to override storage class limitations or view all 
available VM classes, for example, you can enter a star ('*') instead of specific values.
•
zones. For each zone, specify CPU and memory limits and reservations and VM class reservations.
Your full specification might look like the following example.
apiVersion: infrastructure.cci.vmware.com/v1alpha2
kind: SupervisorNamespaceClassConfig
metadata: 
  name: medium-gpu-workloads
spec: 
  contentSources: 
    - name: "paif-content-library" 
      type: ContentLibrary 
  storageClasses: 
    - name: 'vSAN Default Storage Policy' 
      limit: 2000Gi 
  vmClasses: 
    - name: 'best-effort-large' 
    - name: 'vgpu-1xa100-40c' 
    - name: 'vgpu-1xa100-4c' 
  zones: 
    - name: domain-c10 
      cpuLimit: 40G 
      cpuReservation: 10G 
      memoryLimit: 50Gi 
      memoryReservation: 10Gi     
      vmClassReservations: 
        - count: 1 
          vmClassName: vgpu-1xa100-40c 
        - count: 1 
          vmClassName: vgpu-1xa100-4c 
    - name: domain-c54 
      cpuLimit: 40G 
      cpuReservation: 10G 
      memoryLimit: 50Gi 
      memoryReservation: 10Gi     
      vmClassReservations: 
        - count: 1 
          vmClassName: vgpu-1xa100-40c 
        - count: 1 
          vmClassName: vgpu-1xa100-4c
VMware by Broadcom
79


---

VMware Private AI Foundation with NVIDIA 9.1
7. In the VCF Automation organization, create a project.
a) Select Manage & Govern > Projects and click New.
b) Enter a name for the project.
c) Click Create.
8. Add a namespace to the project.
a) Select Manage & Govern > Projects and click your project name.
b) On the Namespaces tab, click New Namespace.
c) Enter a name and description for the namespace.
d) Select the namespace class that you created in Step 6.
e) Select the region you created in Step 2.
f)
Select at least one zone.
g) To override reserved VM classes in namespaces with reservations, turn on the toggle.
If the namespace class has no reservations, then you can use this option to add reserved VM classes.
Review the zones that have reserved VM classes.
1. Expand the zone to which you want to assign a reserved VM class.
2. Select the VM classes that you want to assign to the namespace.
3. In the Instances to Assign column, specify the number of instances that you want to assign for each VM class. 
The number must be lower or equal to the maximum allowed number of instances shown in the Max Available 
column.
h) Select a networking type.
i)
If you want to override the resource limits defined for the selected zones, turn on the Override toggle and enter 
new values.
Resource limits can only be overridden if the zones are not explicitly defined in the namespace class config.
j)
Click Create.
You configured your VCF Automation organization for VMware Private AI Foundation with NVIDIA.
Enable users in your organization to provision AI workloads.
•
Add catalog items for provisioning AI applications by using VCF Automation. See Create AI Catalog Items in using the 
Private AI Foundation Quickstart.
•
Activate Private AI Services in a namespace. See Activate Private AI Services in an Organization Namespace.
Set Up a Local Platform Harbor Registry for VMware Private AI 
Foundation with NVIDIA
You deploy and set up a local Harbor registry in a workload domain to store ML models in model galleries for Private AI 
Services. In addition, in a disconnected environment, a local registry is required to host the artifacts for provisioning deep 
learning VMs and VKS clusters with GPU, or for installing Private AI Services.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for 
deep learning VMs and VKS clusters with GPU. See Setup Workflows for Deep Learning VMs and VKS Clusters with 
GPU or Setup Workflows for Private AI Services.
VMware by Broadcom
80


---

VMware Private AI Foundation with NVIDIA 9.1
•
Allocate enough storage space for hosting the following artifacts:
Environment Connectivity
Artifacts
Disconnected
•
Private AI Services Carvel package
•
NVIDIA GPU Operator
•
Private AI Services 3.0 inference engines package
The Carvel package of Private AI Services 2.1 includes the 
images of the inference engines.
•
NVIDIA NGC containers and other artifacts you plan to deploy 
on a deep learning VM or on a VKS cluster. See Deployment 
Artifacts for Deep Learning VMs and VKS Clusters with GPU 
in a Disconnected Environment and Deployment Artifacts for 
Private AI Services 2.1 in a Disconnected Environment.
•
Connected
•
Disconnected
•
Models for endpoints in Private AI Services
•
Custom inference engine for running models in Private AI 
Services
Starting with VCF 9.1, VCF Automation Provider Management contains a pre-built VCF Service for deploying a Harbor 
registry in a region. You can use this installation method to set up a platform registry for deploying deep learning VMs and 
VKS clusters with GPU or for Private AI Services and for your model gallery in Private AI Services.
1. In the VCF Automation Provider Management Portal, install the Harbor service.
See Install Regional Harbor in VCF Automation in the VMware Cloud Foundation Service Administration and 
Development documentation. This Harbor registry automatically pulls OCI images, such as Carvel packages for 
Supervisor Services and dependent container images, from the software depot in your VCF environment.
2. If you do not have VCF Automation in your VCF 9.1.0 environment, deploy the Harbor Supervisor Service from a 
bootstrap Harbor registry.
•
In a connected environment, install the Supervisor Services for Contour and Harbor using the manifest YAML files 
from the Broadcom Support Portal and the Carvel packages from Broadcom Artifactory.
•
In a disconnected environment, install the Supervisor Services for Contour and Harbor using the Carvel packages 
saved in a boostrap registry and the manifest YAML files updated with the location of the boostrap registry and 
available on your jump host.
See Setting Up a Local Harbor Registry in VMware Private AI Foundation with NVIDIA in the VMware Private AI 
Foundation with NVIDIA 9.0 documentation.
3. If you do not have VCF Automation in your VCF 9.1.1 environment, deploy the Harbor Supervisor Service from the 
software depot.
You can configure this Harbor registry to automatically pull OCI images, such as Carvel packages for Supervisor 
Services and dependent container images from the software depot in your VCF environment. For more information, 
see Deploying VKS in Air-Gapped Environments.
If you plan to use the Harbor registry without connecting it to the software depot, follow Step 5, Configure Harbor on 
the Supervisor, in the Deploy VKS in AirGapped Environments procedure.
4. For running models in Private AI Services, create a robot account in the registry for pulling images from model 
galleries.
•
To be able to pre-install DL workloads in deep learning VMs or provisioning VKS clusters with GPU in a disconnected 
environment, Upload the Components for Deep Learning VMs and VKS Clusters with GPU to a Disconnected 
Environment.
•
To be able to install and use Private AI Services in a disconnected environment, Upload the Private AI Services 3.0 
Components to a Disconnected Environment or Upload the Private AI Services 2.1 Components to a Disconnected 
Environment.
VMware by Broadcom
81


---

VMware Private AI Foundation with NVIDIA 9.1
Finalizing the Setup for Deep Learning VMs and VKS Clusters with 
GPU
As a VI administrator and as organization administrator, add the required configuration to make the VMware Private AI 
Foundation with NVIDIA environment ready for provisioning deep learning VMs and GPU-accelerated VKS clusters.
You add the AI catalog items to VCF Automation by running the Private AI Foundation Quickstart, uploading in advance 
the required images to a Harbor registry in a disconnected environment.
In This Chapter
•
Upgrading from VMware Aria Automation 8.18 to VCF Automation 9.1
•
Deployment Artifacts for Deep Learning VMs and VKS Clusters with GPU in a Disconnected Environment
•
Upload the Components for Deep Learning VMs and VKS Clusters with GPU to a Disconnected Environment
•
Provide an Ubuntu Package Repository Mirror for VKS Clusters with GPU in a Disconnected Environment
•
Provide vGPU and GPU Data Center Drivers for Deep Learning VMs in a Disconnected Environment
•
Create AI Catalog Items in VCF Automation using the Private AI Foundation Quickstart
Upgrading from VMware Aria Automation 8.18 to VCF Automation 9.1
When you upgrade to VCF Automation 9.1, your VMware Aria Automation 8.18 configuration transitions into a VM Apps 
organization. To continue provisioning AI workloads, you must provision a new All Apps organization, where you will 
create new Private AI catalog items in VCF Automation 9.1.
The cloud templates that are backing the catalog items for provisioning AI workloads in VMware Aria Automation 
are using Cloud Consumption Interface (CCI). However, in VCF Automation 9.1, CCI is not available for VM Apps 
organizations but only for All Apps organizations.
You can use one or both of the following options for provisioning AI workloads:
•
Import your Private AI Foundation namespaces into a VCF Automation All Apps organization.
Use this option for namespaces with running AI workloads that must remain intact.
•
Switch to provisioning new AI workloads only from a VCF Automation All Apps organization.
Use this option to set up a clean All Apps based environment and reprovision deep learning VMs and VKS clusters.
If you are upgrading to VCF Automation 9.1.0, before you transition to an All Apps organization, you must delete the 
cloud templates for the AI workload catalog items. Perform the following high-level workflow to switch to using the 
catalog items in an All Apps organization:
a. Back up any customizations you have made in the cloud templates for the AI workload catalog items in VMware 
Aria Automation.
b. In Automation Assembler, delete the cloud templates for the AI workload catalog items from the Templates list on 
the Design tab.
c. Upgrade to VCF Automation 9.1.0. See Upgrading to VMware Cloud Foundation 9.1.x in the VCF documentation.
d. In an All Apps organization, run the Private AI Foundation Quickstart wizard to have the latest catalog items 
created. See Create AI Catalog Items in VCF Automation using the Private AI Foundation Quickstart.
e. Transfer the customizations that you have backed up.
Because the code of the blueprints that are generated in VCF Automation 9.1 is compatible with a newer 
configuration for AI workloads than that in VMware Aria Automation, apply the backed-up customizations 
considering the blueprint changes in the 9.1.0 release. For example, updated deep learning VM cloud-init scripts, 
new variables, using the existing namespace instead of creating a namespace, and others.
If you upgrade to VCF Automation 9.1.1, cleaning up cloud templates in the VM Apps organization is not required.
Import Private AI Foundation Namespaces into VCF Automation
If you want to continue using the namespaces that you used for VMware Private AI Foundation with NVIDIA in VMware 
Aria Automation 8.18 in VCF Automation 9.1, you can migrate these namespaces to VCF Automation.
VMware by Broadcom
82


---

VMware Private AI Foundation with NVIDIA 9.1
Important:
Before you begin importing your namespaces to VCF Automation 9, ensure that you are using vCenter 8.0 
Update 3 or later. If you are using an earlier version, upgrade your vCenter instance to version 8.0 Update 3 or 
later.
1. Back up the following configuration and information:
a. Project users. If you want to rebuild the same project membership, back up project membership using the following 
mapping:
•
For users with the Project Administrator role in a project in VMware Aria Automation 8.18, assign the Project 
Administrator role in VCF Automation.
•
For users with the Project Member role in a project in VMware Aria Automation 8.18, assign the Project 
Advanced User role in VCF Automation.
•
For users with the Project Viewer role in a project in VMware Aria Automation 8.18, assign the Project Auditor 
role in VCF Automation.
b. Deployment information. For example, the Overview tab of a PyTorch deployment, contains information about SSH 
and the IP address, and the Overview tab of a TKG cluster has the kubectl login command, which you can continue 
to use after the workload is migrated.
c. Blueprints that are generated by the Private AI Foundation Quickstart and any customizations that are applied on 
top of the generated blueprints.
Note:
RAG and Tensorflow blueprints are deprecated and may not be supported in the future.
2. Detach the vCenter from VMware Aria Automation 8.x.
a. In vCenter, remove the identity provider.
Select Workload Management > Supervisors and open the supervisor which has the Private AI Foundation 
namespaces. Next, delete any identity providers that are listed on the Configure > Identity Providers page.
b. In VMware Aria Automation Service Broker, remove the region from the supervisor.
On the Supervisor Namespaces page, click the information icon, download the CCI kubectl plug-in, and use it to 
log in to VMware Aria Automation 8.x. Get the list of supervisors and edit the supervisor which hosts the Private AI 
namespaces and remove the region from the supervisor.
c. In VMware Aria Automation Assembler, remove the vCenter cloud account.
On the Infrastructure > Cloud Accounts page, locate the cloud account corresponding to the vCenter instance, 
and delete it.
The vCenter instance is removed from VMware Aria Automation 8.x. All namespaces, resources under the 
namespaces, and the permission of namespaces are preserved in the vCenter instance.
Important:
Reserved VM classes and VM classes with passthrough GPU are not supported for imported namespaces.
1. In the VCF Automation Provider Management UI, accept the following certificates.
If you are evaluating this tutorial in a staging environment, without using certificates signed by a well-known certificate 
authority, you must follow the steps below to accept these certificates.
If all certificates are signed by a well-known certificate authority, you can skip this step.
a) Trust the Supervisor certificate.
1. In vCenter, navigate to Developer Center > API Explorer > namespace_management/clusters.
If you have multiple vCenter instances under the same vCenter console, select the one registered with VCF 
Automation.
2. Execute GET /api/vcenter/namespace-management/clusters.
3. Write down the cluster value, for example, domain-c52.
4. Execute GET /api/vcenter/namespace-management/clusters/domain-c52.
5. Download the response.
6. Copy the tls_management_endpoint_certificate value in a new file.
VMware by Broadcom
83


---

VMware Private AI Foundation with NVIDIA 9.1
7. Replace all \n with an actual line feed character and save the file.
8. In the VCF Automation Provider Management UI, select Certificate Management > Trusted Certificates.
9. Click Import, then click Select File, and select the file you saved in Step vii.
10. Name the file, for example k8s-cacert, and click Import to save it.
b) Trust the vCenter certificate.
1. Log in to vCenter and save the vCenter certificate to your local system.
2. In the VCF Automation Provider Management UI, select Certificate Management > Trusted Certificates.
3. Click Import, then click Select File, and select the file you saved.
4. Name the file and click Import to save it.
2. Obtain access tokens.
a) Obtain the access token of the provider administrator.
1. Log in to the VCF Automation Provider Management UI as a provider administrator.
2. Select My Account > API Token.
3. Under API Tokens tab, click New and create a new API token.
4. Save the token to your local system.
5. Get an access token.
In the code samples in this procedure:
•
VCFA_FQDN is the FQDN of your VCF Automation instance.
•
PROVIDER_ADMIN_REFRESH_TOKEN is the provider administrator's API token.
•
PROVIDER_ADMIN_ACCESS_TOKEN is the provider administrator's access token.
curl --location 'https://${VCFA_FQDN}/oauth/provider/token' \
--header 'Accept: application/json;version=41.0.0-alpha' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=refresh_token' \
--data-urlencode 'refresh_token=${PROVIDER_ADMIN_REFRESH_TOKEN}'
b) Obtain the access token of the organization administrator.
•
TENANT_ADMIN_ACCESS_TOKEN is the organization administrator's access token.
curl -k -i --location --request POST 'https://${VCFA_FQDN}/tm/cloudapi/1.0.0/sessions' \
--header 'Accept: application/json;version=41.0.0-alpha;' \
--header 'Authorization: Basic <BASE64 encode tenant_admin_username@orgName:userPass>' | grep x-vmware-
vcloud-access-token
3. Attach the vCenter to VCF Automation.
•
For PROVIDER_ADMIN_ACCESS_TOKEN, use the provider administrator's access token you obtained in Step 2.
•
For VC_FQDN, use the FQDN name of the vCenter instance, instead of the IP address.
curl --location 'https://${VCFA_FQDN}/tm/api/admin/extension/action/registervimserver' \
--header 'Accept: application/*+xml;version=41.0.0-alpha' \
--header 'Content-Type: application/vnd.vmware.admin.registerVimServerParams+xml;charset=UTF-8' \
--header 'Authorization: Bearer ${PROVIDER_ADMIN_ACCESS_TOKEN}' \
--data-raw '<root:RegisterVimServerParams xmlns:root="http://www.vmware.com/vcloud/extension/v1.5" 
 xmlns:ns0="http://www.vmware.com/vcloud/v1.5"> 
    <root:VimServer name="${VC_NAME}"> 
        <ns0:Description/> 
        <root:Username>${VC_ADMIN_USERNAME}</root:Username> 
        <root:Password>${VC_ADMIN_PASSWORD}</root:Password> 
        <root:Url>https://${VC_FQDN}</root:Url> 
        <root:IsEnabled>true</root:IsEnabled> 
        <root:tenantScoped>false</root:tenantScoped> 
        <root:proxyEnabled>false</root:proxyEnabled> 
    </root:VimServer>
VMware by Broadcom
84


---

VMware Private AI Foundation with NVIDIA 9.1
</root:RegisterVimServerParams>'
In VCF Automation Provider Management:
•
On the Connections > vCenter page, verify that vCenter is attached and in an Enabled state.
•
On the Supervisors page, verify that the supervisors from vCenter are listed. If no supervisors are listed, navigate 
to Connections > vCenter, click the vertical ellipsis next to the vCenter, and click Refresh.
4. In the VCF Automation organization, create a project.
Later in the procedure, you import the namespace into this project.
a) Select Manage & Govern > Projects and click New.
b) Enter a name for the project.
c) Add users to the project using the project memberships that you backed up at the beginning.
d) Click Create.
5. In VCF Automation Provider Management, import the namespace from vCenter.
You must make a separate API call for each namespace you want to import.
curl --location 'https://${VCFA_FQDN}/tm/cloudapi/vcf/namespaces/import' \
--header 'Accept: application/*+json;version=41.0.0-alpha' \
--header 'X-VMWARE-VCLOUD-TENANT-CONTEXT: ${ORG_ID}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer ${PROVIDER_ADMIN_ACCESS_TOKEN}' \
--data '{ 
    "name": "${NAMESPACE_NAME}", 
    "org": { 
        "id": "urn:vcloud:org:${ORG_ID}", 
        "name": "${ORG_NAME}" 
    }, 
    "vcenter": { 
        "id": "urn:vcloud:vimserver:${VC_ID}", 
        "name": "${VC_NAME}" 
    }, 
    "projectAssignment": { 
        "id": "urn:vcloud:projectAssignment:${PROJECT_ID}", 
        "name": "${PROJECT_NAME}" 
    }
}'
•
For PROVIDER_ADMIN_ACCESS_TOKEN, use the provider administrator's access token you obtained in Step 2.
•
For ORG_ID, copy the value from the organization URL. For example, if the organization URL 
is https://VCFA_FQDN/provider/organizations/urn:vcloud:org:9b864f19-
c42f-4ab0-868a-8085eb4328b5/overview, the organization ID is 9b864f19-
c42f-4ab0-868a-8085eb4328b5.
•
For VC_ID, copy the ID from the URL of the vCenter instance where you want to import the namespace. 
For example, if the vCenter URL is https://VCFA_FQDN/provider/connections/vcenter/
urn:vcloud:vimserver:2a0fedf4-3b6b-4461-84ac-42c6aa7655d9, the vCenter ID is
2a0fedf4-3b6b-4461-84ac-42c6aa7655d9.
•
For PROJECT_ID, copy the ID from the URL of the project where you want to import the namespace. 
For example, the URL of the project page is https://VCFA_FQDN/automation/#/manage-and-
govern/projects/edit/aa5a1845-4bb3-44c3-8773-d207f5c7fc5f/summary, the project ID is
aa5a1845-4bb3-44c3-8773-d207f5c7fc5f.
6. In VCF Automation Provider Management, verify that an NSX Manager, region, and region quota were created.
If this namespace is the first namespace of a supervisor that is imported into VCF Automation, a mock NSX Manager, 
a region, and a region quota are created.
VMware by Broadcom
85


---

VMware Private AI Foundation with NVIDIA 9.1
In VCF Automation, each region can only be associated with a single supervisor. If a vCenter 8.x instance has multiple 
supervisors, when the first namespace in a supervisor is imported to VCF Automation, a new region and region quota 
is created for each supervisor.
All namespaces from the same supervisor share the same region, region quota, and NSX Manager.
•
Verify that the NSX Manager is listed on the Connections > NSX Manager page. The name of the NSX Manager 
is a concatenation of the supervisor name and the first five characters of the vCenter ID. For example, the name of 
the created NSX Manager is supervisor-b999a, where the associated supervisor name is supervisor and the 
first five characters of the vCenter ID are b999a.
•
Verify that a region is listed on the Regions page. The name of the created region is the same as the NSX 
Manager name. For example, the name of the created region is supervisor-b999a, which is the same as the 
NSX Manager.
•
Verify that a region quota is created under Region Quota on the Organizations page. The region quota name is 
the same as the name of the region and NSX Manager.
7. In vSphere, verify that the identity provider is created.
When a region is created for a supervisor, an identity provider is created in vCenter for that supervisor.
In vCenter, select Workload Management > Supervisors, open the supervisor, and verify that there is an identity 
provider entry under Configure > Identity Providers.
8. Import the namespace from VCF Automation Provider Management to the organization.
You must make a separate API call for each namespace you want to import.
•
You can only use the organization administrator's token to make this call, which is the value for
TENANT_ADMIN_ACCESS_TOKEN that you acquired in Step 2b.
•
REGION_NAME is the name of the automatically created region.
curl --location 'https://${VCFA_FQDN}/cci/kubernetes/apis/infrastructure.cci.vmware.com/v1alpha2/
namespaces/${PROJECT_NAME}/supervisornamespaces' \
--header 'accept: application/json, text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer ${TENANT_ADMIN_ACCESS_TOKEN}' \
--data '{ 
    "apiVersion": "infrastructure.cci.vmware.com/v1alpha2", 
    "kind": "SupervisorNamespace", 
    "metadata": { 
        "name": "${NAMESPACE_NAME}", 
        "namespace": "${PROJECT_NAME}" 
    }, 
    "spec": { 
        "regionName": "${REGION_NAME}", 
        "className": ":imported" 
    }
}'
Namespace classes for imported namespaces are marked as :imported in the VCF Automation organization UI.
9. Configure SSO so that you can access the namespace and its resources from VCF Automation.
Important:
You must configure SSO only if you are running vCenter 8.x. If you are running vCenter 9.x, you can skip this 
step.
a) Delete any existing CCI supervisor service.
In vCenter, select Workload Management > Services > Consumption Interface, click Actions, and then click
Delete.
VMware by Broadcom
86


---

VMware Private AI Foundation with NVIDIA 9.1
b) Create jwtauthenticator on the attached vCenter.
vCenter 8.0 Updade 3 doesn't support creating jwtauthenticator using the API, so you must do it manually after 
you log in to the floating IP of the Control Plane VM of the supervisor cluster.
1. To get the floating IP and login credentials of the Control Plane VM of the supervisor, log in to the vSphere over 
ssh as root and run /usr/lib/vmware-wcp/decryptK8Pwd.py.
2. Prepare the jwtauthenticator configuration file.
apiVersion: authentication.concierge.pinniped.dev/v1alpha1
kind: JWTAuthenticator
metadata: 
  name: ${JWT_AUTHENTICATOR_NAME} 
  namespace: vmware-system-pinniped
spec: 
  issuer: https://${VCFA_FQDN}/oidc 
  audience: ${AUDIENCE_ID} 
  claims: 
    username: preferred_username 
    groups: groups 
  tls: 
    certificateAuthorityData: ${TM_CERT_BASE64}
– For JWT_AUTHENTICATOR_NAME and AUDIENCE_ID, locate the warning banner in the VCF Automation
Provider Management UI. For example, the JWT_AUTHENTICATOR_NAME value is 87bbc252-99f4-4dd2-
bb9e-0e58a0939628-ce, the AUDIENCE_ID value is 4925d0d5-bb1b-423e-b662-a260dcb258b6, 
and the region is paif-test-region2.
– For TM_CERT_BASE64, run openssl s_client -connect ${VCFA_FQDN}:443 -servername 
${VCFA_FQDN} 2>/dev/null | openssl x509 | base64.
3. On the Regions page in VCF Automation Provider Management, locate the supervisor associated with the 
region. In this example, the region is called paif-test-region2.
4. Save and apply the configuration YAML file in the control plane VM.
kubectl -n vmware-system-pinniped apply -f jwtauthenticator.yaml 
  
###example output after running the bash cmd
jwtauthenticator.authentication.concierge.pinniped.dev/brownfield-jwt-authenticator created
VMware by Broadcom
87


---

VMware Private AI Foundation with NVIDIA 9.1
10. Optional: Update the content library that contains the deep learning VM images in vCenter, such as https://
packages-pstg.broadcom.com/dlvm/lib.json.
11. To create Private AI workloads in your newly imported namespace, follow the procedure in Create AI Catalog Items in 
using the Private AI Foundation Quickstart.
Important:
The GPU operator cannot be deployed with versions 1.27.11, 1.28.8, and 1.29.12, so when deploying a VKS 
cluster, do not use these three versions.
•
You can create non-Private AI Foundation resources in the imported namespace.
•
If you want to remove an imported namespace, perform the following steps:
a. Detach the imported namespace from vCenter and remove it from the CCI database. Use the organization 
administrator's token. Note that the namespace still exists in vCenter.
curl --location --request DELETE 'https://${VCFA_FQDN}/cci/kubernetes/apis/
infrastructure.cci.vmware.com/v1alpha2/namespaces/${PROJECT_NAME}/supervisornamespaces/
${NAMESPACE_NAME}' \
--header 'accept: application/json, text/plain' \
--header 'Authorization: Bearer ${TENANT_ADMIN_ACCESS_TOKEN}'
b. Remove the imported namespace of a vCenter from VCF Automation.
a. Get the details of the namespace.
curl --location 'https://${VCFA_FQDN}/tm/cloudapi/vcf/namespaceSummaries?filter=name==
${NAMESPACE_NAME}' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json;version=41.0.0-alpha' \
--header 'Authorization: ${PROVIDER_ADMIN_ACCESS_TOKEN}'
b. Delete the namespace from VCF Automation.
curl --location --request DELETE 'https://${VCFA_FQDN}/tm/cloudapi/vcf/namespaces/
urn:vcloud:namespace:${NAMESPACE_UUID_TM}?detachOnly=true' \
--header 'accept: application/json;version=41.0.0-alpha' \
--header 'Authorization: ${PROVIDER_ADMIN_ACCESS_TOKEN}'
c. If you want to delete the project where the namespace is imported, ensure that the namespace is deleted both 
in VCF Automation Provider Management and in the VCF Automation organization. If a namespace is not 
deleted in VCF Automation Provider Management and only in the organization, the project cannot be deleted.
•
If you want to detach the vCenter from VCF Automation, perform the following steps:
a. Detach all namespaces associated with the vCenter instance one by one.
b. In the Region Quota section on the organization details page in VCF Automation Provider Management, delete any 
region quotas associated with the vCenter instance.
c. On the Regions page in VCF Automation Provider Management, remove any regions associated with the vCenter 
instance.
d. On the Identity Providers page in vCenter, verify that the identity provider associated with the region is deleted in 
vCenter.
e. Dismiss the warning banner in VCF Automation Provider Management.
f.
In vCenter, log in to the control plane VM, find the jwtauthenticator you created, and delete it.
root@420d691471b1878108260635af5c23b0 [ ~ ]# kubectl get jwtauthenticator
root@420d691471b1878108260635af5c23b0 [ ~ ]# kubectl delete jwtauthenticator 87bbc252-99f4-4dd2-
bb9e-0e58a0939628-ce
g. On the NSX Manager page in VCF Automation Provider Management, remove the NSX Manager
h. On the vCenter page in VCF Automation Provider Management, first disable and then unregister the vCenter 
instance.
VMware by Broadcom
88


---

VMware Private AI Foundation with NVIDIA 9.1
Deployment Artifacts for Deep Learning VMs and VKS Clusters with GPU in a 
Disconnected Environment
Both AI workload platforms in VMware Private AI Foundation with NVIDIA, that is, based on deep learning VMs and on 
VKS clusters with GPU, pull resources from the Internet in a connected environment. In a disconnected (air-gapped) 
environment, as a VI administrator, you must make these resources available locally.
Note:
If you plan to use the Private AI Foundation Quickstart wizard in VCF Automation to add AI catalog items, you 
must make the resources for both deep learning VMs and for VKS clusters with GPU available locally.
Required Download Locations
You must provide two nodes for hosting the deployment artifacts:
•
Local content library for VKr images for the VKS cluster nodes
•
A Harbor registry for storing the components of the NVIDIA DL workloads for deep learning VMs and NVIDIA GPU 
Operator for VKS clusters with GPU
•
A Web server for storing the .run files of vGPU guest driver for deep learning VMs and the Ubuntu local package 
repository for VKS clusters with GPU
Local Artifacts for Deep Learning VMs
By default, during its startup, a deep learning VM installs the certain deployment artifacts from the Internet. In a 
disconnected enviornment, they must be available locally.
Figure 10: Artifacts for Deep Learning VMs in a Disconnected VMware Private AI Foundation with NVIDIA
Environment
NVIDIA DL Workloads
(PyTorch , TensorFlow , CUDA Sample , 
Triton Server , DCGM Exporter )
NVIDIA vGPU Guest Driver 
.run Files
Platform Harbor Registry
Web Server
Deployment Artifact
Source Location
Staging Location
Mirroring Method
vGPU driver or GPU data center .run
files
NVIDIA Licensing Portal
Web server
Manually
See Provide vGPU and GPU Data 
Center Drivers for Deep Learning VMs 
in a Disconnected Environment.
Container images of NVIDIA DL 
workloads, for example, PyTorch with 
an added DCGM Exporter
Some DL workloads have separate 
images for vGPU and for GPU 
passthrough. For more details on the 
differences in the download URLs, 
see Running a Deep Learning VM on 
vGPU or GPU Passthrough.
NVIDIA NGC catalog
Harbor registry
Using the resource mirroring tool
See Upload the Components for Deep 
Learning VMs and VKS Clusters with 
GPU to a Disconnected Environment.
Local Artifacts for VKS Clusters with GPU
When you provision a VKS cluster with GPU, you must have certain artifacts available locally.
VMware by Broadcom
89


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 11: Artifacts for VKS Clusters with GPU in a Disconnected VMware Private AI Foundation with NVIDIA
Environment
Ubuntu Package 
Repository
GPU Operator Helm Chart
vGPU Guest Drivers
Platform Harbor Registry
Web Server
Kubernetes Node Feature 
Discovery
DCGM Exporter
MIG Device Plugin
Data  Center GPU driver
tkg -gpu -console or 
deep -learning -console
VKr Image
Local Content Library
vGPU Device Manager
Deployment Artifact
Source Location
Download Location
Download Method
VKr images
Public VKr content library Local content library in 
vCenter
Manually
See Configure a Content Library 
with Ubuntu VKr for a Disconnected 
VMware Private AI Foundation with 
NVIDIA Environment.
NVIDIA GPU Operator Helm chart and 
required container images
NVIDIA NGC catalog
Harbor registry
tkg-gpu-console
tkg-gpu-console for VCF 9.1.0 and 
deep-deep-learning-container for
VCF 9.1.1 is required for installing 
the NVIDIA GPU Operator when 
provisioning VKS clusters in VCF 
Automation.
Broadcom Artifactory
Harbor registry
Using the resource mirroring tool
See Upload the Components for Deep 
Learning VMs and VKS Clusters with 
GPU to a Disconnected Environment.
VMware by Broadcom
90


---

VMware Private AI Foundation with NVIDIA 9.1
Deployment Artifact
Source Location
Download Location
Download Method
Ubuntu package repository
archive.ubuntu.com
Web server
Manually
See Provide an Ubuntu Package 
Repository Mirror for VKS Clusters with 
GPU in a Disconnected Environment.
Upload the Components for Deep Learning VMs and VKS Clusters with GPU to a 
Disconnected Environment
In a disconnected VMware Private AI Foundation with NVIDIA environment, upload the components of the NVIDIA GPU 
Operator for GPU-accelerated VKS clusters and DL workloads for deep learning VM deployments to a Harbor project by 
using resource mirroring utility.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for 
deep learning VMs and VKS clusters with GPU. See Setup Workflows for Deep Learning VMs and VKS Clusters with 
GPU.
•
Verify that the Harbor administrator has created a project for the VMware Private AI Foundation with NVIDIA resource 
in the local Harbor registry.
Download the resource mirroring utility from Broadcom knowledge base article 388470 and run the utility to upload the 
resources for the NVIDIA GPU Operator and DL workloads for deep learning VM deployments to the allocated Harbor 
project.
If you are using the same machine for your bastion and admin hosts, you can both pull and push artifacts from this 
machine. If the bastion host and admin hosts are separate machines, the .tar images must be transferred from the 
bastion to the admin host manually.
Provide an Ubuntu Package Repository Mirror for VKS Clusters with GPU in a 
Disconnected Environment
A local Ubuntu repository is required for compiling the vGPU or GPU data center guest driver before installing it on the 
cluster nodes.
•
Verify that you have at least 800 GB storage space on the machine where you plan to mirror the Ubuntu package 
repository.
•
Install the ORAS CLI on your machine. See the ORAS CLI installation documentation.
•
Set up a web server for hosting the local Ubuntu package repository. The web server must be accessible by the VKS 
clusters with GPU.
1. Provide a local Ubuntu package repository on a Web server and prepare the index file for the package repository.
The index file will be hosted on the platform Harbor registry. This file will be used for installing the NVIDIA GPU 
Operator when your DevOps engineers provision a VKS cluster with GPU using kubectl.
For example, you can create the following custom-repo.list file in a text editor.
You can use a package repository mirror for Ubuntu 22.04.
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy-updates main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy-security main universe
or for Ubuntu 24.04.
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble-updates main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble-security main universe
If you are using GPU passthrough, this repository can be a partial mirror with only the packages that are required for 
the NVIDIA GPU Operator driver compilation.
For more information, see Install NVIDIA GPU Operator in Air-Gapped Environments in the NVIDIA documentation.
VMware by Broadcom
91


---

VMware Private AI Foundation with NVIDIA 9.1
2. To make this file accessible to deployments requested by using the VCF Automation self-service catalog, push
custom-repo.list to a resources repository in the allocated project in Harbor.
When you provision VKS clusters with GPU in VCF Automation, the index file is referenced by the NVIDIA GPU driver 
installer script to locate the Ubuntu package repository.
oras login <your_private_harbor_url> -u <your_private_harbor_username> -p <your_private_harbor_password> 
  
oras push <your_private_harbor_url_with_prefix>/<your_project-name>/resources/custom-repo.list:1.0 custom-
repo.list:text/plain
Share the location of the Ubuntu package repository mirror with the DevOps engineers in your organization if they plan to 
provision VKS clusters with GPU by using the kubectl command.
If you provision a VKS cluster with GPU in VCF Automation, the location of the Ubuntu package repository is discovered 
automatically. In this case, DevOps engineers do not need to know it in advance.
Provide vGPU and GPU Data Center Drivers for Deep Learning VMs in a 
Disconnected Environment
You must also provide a location to download the vGPU guest drivers or the GPU data center drivers from to the deep 
learning VMs in your organization.
1. To support deep learning VM deployments, upload to a local Web server the required vGPU or GPU data center guest 
driver versions, which are downloaded from the NVIDIA Licensing Portal.
•
For vGPU guest drivers, you can upload .run or .zip files.
•
For GPU data center drivers, you upload .run files.
2. For vGPU guest drivers, create an index file in one of the following formats and upload it to the Web server.
The index file is required for deploying deep learning VMs using kubectl or directly on a vSphere cluster. For GPU 
passthrough, you directly provide the URL to the .run file of the data center driver.
•
An index .txt file with a list of the .run or .zip files of the vGPU guest drivers.
host-driver-version-1
guest-driver-download-URL-1
host-driver-version-2
guest-driver-download-URL-2
host-driver-version-3
guest-driver-download-URL-3                      
•
A directory index in the format generated by Web servers, such as NGINX and Apache HTTP Server. The version-
specific vGPU driver files must be uploaded as .zip files.
Share the location of the vGPU guest drivers with the DevOps engineers in your organization if they plan to provision 
deep learning VMs by using the kubectl command.
Create AI Catalog Items in VCF Automation using the Private AI Foundation 
Quickstart
As an organization administrator, you use the Private AI Foundation Quickstart in VCF Automation to set up and provide 
GPU-enabled deep learning virtual machines and VMware Kubernetes Service (VKS) clusters as catalog items that data 
scientists and DevOps teams in your organization can request in the self-service VCF Automation catalog.
How does the Private AI Foundation Quickstart work
Using the Private AI Foundation Quickstart, you do the following tasks:
1. Select a project and namespace where Private AI Foundation images and GPU-accelerated VM classes are available.
The project links your users with the vSphere namespace, so that they can deploy blueprints with a network and 
storage resources to your vCenter instance.
2. Add an NVIDIA Client Configuration token.
3. Optionally, configure a VMware Data Services Manager integration to supply vector databases.
VMware by Broadcom
92


---

VMware Private AI Foundation with NVIDIA 9.1
4. Select content to add to the VCF Automation catalog.
After you run the Private AI Foundation Quickstart the first time, the following catalog items are created in the VCF 
Automation catalog that become available for users in your organization to deploy:
•
AI Workstation – an AI workstation powered by a deep learning VM that can be configured with desired vCPU, vGPU, 
memory, and the option to pre-install AI/ML frameworks like PyTorch and CUDA Samples.
•
Triton Inference Server – a GPU-enabled virtual machine with Triton Inference Server.
•
AI Kubernetes Cluster – a VKS cluster with GPU-capable worker nodes to run AI/ML cloud-native workloads.
•
DSM Database – a pgvector database managed by VMware Data Services Manager.
Each time you run the quickstart, new catalog items are created for you in the VCF Automation catalog in addition to any 
previously created items. You can run the quickstart again multiple times if you need any of the following:
•
Enable provisioning of AI workloads on another supervisor.
•
Accommodate a change in your NVIDIA AI Enterprise license, including the client configuration .tok file and license 
server, or the download URL for the vGPU guest drivers or the local registry hosting the images and models for a 
disconnected environment.
•
Accommodate a deep learning VM image change.
•
Use other vGPU or non-GPU VM classes, storage policy, or container registry.
•
Generate blueprints for both passthrough and vGPU in the same namespace.
•
Create catalog items in a new project.
•
Use a different proxy server.
•
Use different VMware Data Services Manager settings.
After you run the wizard and the catalog items are created and available in the catalog, you can modify the blueprints that 
back the catalog items to meet the specific needs of your organization.
Note:
In VCF 9.1.1, after you complete Private AI Foundation Quickstart, you must replace the reference to tkg-gpu-
console image in the AI Kubernetes Cluster blueprint, with a reference to deep-learning-container. See 
Step 9.
Note:
In VCF 9.0 and VCF 9.1.0, by default, the NVIDIA GPU Operator version that is installed on the VKS cluster 
by using the AI Kubernetes Cluster blueprint is v25.3.1. You can update the NVIDIA GPU Operator version to 
v26.3.1. See Broadcom knowledge base article 439984.
Data scientists can use deep learning catalog items for deployment of deep learning VMs. DevOps engineers can use the 
catalog items for provisioning AI-ready VKS clusters.
Before you begin
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for 
deep learning VMs and VKS clusters with GPU. See Setup Workflows for Deep Learning VMs and VKS Clusters with 
GPU.
•
Verify that the VCF Automation organization is configured. See Set Up Your VCF Automation Organization for VMware 
Private AI Foundation with NVIDIA .
•
In a VCF 9.1.1 environment, to use the deep-learning-container image that is required for provisioning a VKS 
cluster with GPU, verify that the following conditions are in place:
– You have a platform Harbor registry in both the connected and disconnected (air-gapped) cases with a public 
project for hosting deep-learning-container. See Set Up a Local Platform Harbor Registry for VMware Private 
AI Foundation with NVIDIA.
– In a connected environment, you have a machine with Docker installed.
Procedure
1. Log into VCF Automation organization UI as an organization administrator.
VMware by Broadcom
93


---

VMware Private AI Foundation with NVIDIA 9.1
2. On the Private AI Foundation Quickstart card on the Overview tab, click Start.
3. Review the list of catalog items that the quickstart creates and click Next.
4. Select where you want to provision the catalog items.
a. Select a VCF Automation project.
Projects are used to manage people, assign resources, cloud templates, and deployments.
b. Select a namespace with vGPU-enabled VM classes and Private AI images.
On the right, you see a summary of how many VM classes with vGPU-mode devices and with passthrough devices 
are available to the namespace.
c. Click Next.
5. Provide information about your NVIDIA license server and container registry.
a. Select NVIDIA GPU DirectPath mode.
If the namespace contains both vGPU and passthrough VM classes, you have to select which mode to use. If the 
namespace contains vGPU classes only, then the vGPU mode is automatically applied.
Mode
Configuration
vGPU
In vGPU mode, you can use NVIDIA GPUs of the same or mixed 
memory size.
You must copy and paste the contents of the NVIDIA client 
configuration token. The client configuration token is used to 
assign a license to the guest vGPU driver in the deep learning 
virtual machine and the GPU Operators on VKS clusters.
Fixed/Dynamic DirectPath
In Fixed/Dynamic DirectPath mode, the DL VM can run in 
passthrough mode on an entire GPU, without using vGPU and an 
NVIDIA AI Enterprise license.
VMware by Broadcom
94


---

VMware Private AI Foundation with NVIDIA 9.1
b. Select connectivity type for your environment.
Option
Configuration
Cloud
The NVIDIA vGPU/GPU driver is hosted on the NVIDIA Licensing 
Portal and container images are pulled from the NVIDIA NGC
catalog.
If you want to connect via proxy, turn on the Proxy toggle and 
configure the proxy server. The proxy server is used to download 
the vGPU driver and pull the AI Workstation containers from NGC.
Only unauthenticated HTTP proxy servers are supported for
NVIDIA vGPU license registration.
If you decide to use a proxy server, you must exclude the non-
proxy servers.
1.
In a new browser tab, navigate to Manage & Govern > Netwo 
rking > IP Management > IP Address Blocks.
2.
Copy the CIDR values for the external IP block, private TGW 
IP block, and the private VPC IP CIDR of the Private AI VPC.
This is necessary for AI Kubernetes deployments so that 
internal communication for Kubernetes clusters can bypass 
the proxy.
3.
Paste the CIDR values into the Do not use proxy for text 
box, separating them with comas.
Air-gapped
For air-gapped environments, the vGPU/GPU driver must 
be available on your private network or data center, and the 
containers are pulled from a private registry.
1.
Provide the location of the NVIDIA vGPU drivers and the deep 
learning VM components.
For more information, see the NVIDIA License System
documentation.
2.
Provide the location of the self-hosted registry.
If the registry requires authentication, you must also provide 
login credentials.
You can use Harbor as a local registry for container images 
from the NVIDIA NGC catalog. See Setting Up a Private 
Harbor Registry in .
c. Click Next.
6. Configure the catalog items.
a. Enter a suffix for the catalog item names.
The suffix is a unique identifier for the catalog items that are created when you complete the quickstart. If you run 
the quickstart multiple times for the same project and namespace, the suffix helps you to distinguish between the 
catalog items.
b. Select the Kubernetes version you want to use for AI Kubernetes Cluster deployments.
You can select from the three most recent Kubernetes release versions that are supported for VMware Private AI 
Foundation.
c. Select the VM image you want to use to create the AI workstation.
d. Select a storage class.
The storage class defines storage placement and resources for the virtual machines.
e. Select the VM classes you want to make available to your catalog users.
You must add at least one GPU-capable class and one non-GPU-capable class.
•
GPU-enabled VM classes are used for the deep learning VM and for the worker nodes of the VKS cluster. When 
the catalog item is deployed, the VKS cluster is created with the selected VM classes.
Depending on the namespace you selected, you can use either reserved or non-reserved GPU classes, but not 
both. Available GPU classes are filtered based on the mode you selected in Step 5a, so you can select from 
only vGPU or only Passthrough VM classes, but not both.
VMware by Broadcom
95


---

VMware Private AI Foundation with NVIDIA 9.1
•
Non-GPU-capable nodes are required to run the Kubernetes control planes.
•
VM classes with Unified Virtual Memory (UVM) enabled are required to run AI workstations with Triton Inference 
Server. UVM creates a single memory space that combines the GPU VRAM and the host system RAM. UVM 
only applies to vGPU.
f.
Configure a VMware Data Services Manager (DSM) database for RAG applications to generate separate database 
catalog items.
You use VMware Data Services Manager to create vector databases, such as a PostgreSQL database with 
pgvector extension.
Databases are provisioned in a different workload domain than the deep learning VMs, so you must use different 
VM classes and storage profiles for each. For VMware Data Services Manager databases, select a storage policy 
and VM class that are suitable for RAG use cases.
a. Turn on the toggle.
If you want your developers and catalog users to share a single DSM database, use this option.
Note:
When the toggle is on, a new DSM database catalog item is created.
If a DSM integration is not required for your project, you can turn the toggle off and continue with the next step. In 
this case, a DSM catalog item is not generated.
b. Select a replica mode and topology.
Configuration of the database nodes depends on the replica mode.
•
Single Server – one primary node without replicas.
•
Single vSphere Cluster – three nodes (1 primary, 1 monitor, 1 replica) on a single vSphere cluster providing 
non-disruptive upgrades.
c. Select an infrastructure policy.
The infrastructure policy defines the quality and quantity of resources the database can consume from vSphere 
clusters.
Once you select an infrastructure policy, you can choose from storage policies and VM classes that are 
associated with the infrastructure policy.
d. (Optional) Select a default storage policy.
The storage policy defines storage placement and resources for the database.
e. (Optional) Select a default VM class.
A VM class is not available to provision the newly requested database on.
Note that DSM VM classes are different from Supervisor VM classes.
g. Click Next.
7. Verify your configuration on the Review page.
Consider saving the details for your configuration before running the wizard.
8. Click Finish.
9. If you are using VCF 9.1.1, in the AI Kubernetes Cluster blueprint, replace the reference to the tkg-gpu-console
container image with a reference to the deep-learning-container container image that is hosted in the 
authenticated dlvm.packages.broadcom.com Broadcom Artifactory.
Similarly to tkg-gpu-console, deep-learning-container provides an environment for installing the NVIDIA 
GPU Operator when provisioning VKS clusters in VCF Automation together with security fixes.
a. In a connected environment, get your access token for the Broadcom OCI registries, such as 
dlvm.packages.broadcom.com.
a. Go to the My Downloads page on the Broadcom Support Portal and click Registry Tokens.
b. On the Generate Registry Tokens page, click Generate Token.
c. Click the Copy Token icon in front of the token and save it.
b. On a machine with Docker installed, log in to the dlvm.packages.broadcom.com registry by running the 
followig docker command.
echo your_broadcom_registry_token | docker login dlvm.packages.broadcom.com --
username email_address_for_login_to_Broadcom_support_portal --password-stdin
c. Upload deep-learning-container to an assigned public Harbor project.
VMware by Broadcom
96


---

VMware Private AI Foundation with NVIDIA 9.1
Cloud-Connected 
Environment
1.
Pull the deep-learning-container image by running the following command.
docker pull dlvm.packages.broadcom.com/dlvm/ga/9.1.1.0/deep-learning-
container:9.1.1.0
2.
Tag the downloaded image.
docker tag dlvm.packages.broadcom.com/dlvm/ga/9.1.1.0/deep-learning-
container:9.1.1.0 your-harbor-FQDN:/your-project-name/deep-learning-
container:9.1.1.0
3.
Push the image to your Harbor registry.
docker push your-harbor-FQDN:/your-project-name/deep-learning-
container:9.1.1.0
Disconnected (Air-
Gapped) Environment
If you have not done so already, upload the deployment artifacts for deep learning VMs and VKS 
clusters with GPU to a public Harbor project by using the resource mirroring tool with the BOM for
VCF 9.1.1. See Upload the Components for Deep Learning VMs and VKS Clusters with GPU to a 
Disconnected Environment.
d. Replace the reference to the tkg-gpu-console image with a reference to deep-learning-container in the
AI Kubernetes Cluster blueprint and save the changes.
Cloud-Connected 
Environment
Change the image: paif.packages.broadcom.com/tkg-gpu-console:9.0.0.0
property to image: your-harbor-FQDN:/your-project-name/deep-learning-
container:9.1.1.0.
Disconnected (Air-
Gapped) Environment
Change the image: ${variable.registryUri}/images/tkg-gpu-
console:9.0.0.0 property to "image: ${variable.registryUri}/dlvm/
ga/9.1.1.0/deep-learning-container:9.1.1.0".
e. Publish the blueprint.
Results
The following catalog items – AI Workstation, DSM Database, Triton Inferencing Server, and AI Kubernetes Cluster, 
are created in the VCF Automation catalog and users in the selected namespace can now deploy them.
What to do next
•
Verify that the Private AI catalog items are available in the catalog to the members of the selected project with whom 
you shared the content and monitor the provisioning process to ensure successful deployment.
•
To view the blueprints that back the Private AI catalog items, navigate to Build & Deploy > Content Hub > Blueprint 
Design > Blueprints.
•
In a VCF 9.0 or VCF 9.1.0 environment, update the version in the blueprint to v26.3.1. See Broadcom knowledge base 
article 439984.
The default version v25.3.1 of the NVIDIA GPU Operator in the AI Kubernetes Cluster blueprint has reached End-of-
Life.
•
All passwords entered in the quickstart, like the private container registry password and the proxy password, are saved 
as secrets in VCF Automation.
To view secrets, navigate to Administer > Certificates & Secrets > Secrets and locate the secret. For example, your 
private container registry password is named registry-password-<catalog-item-suffix>. You cannot view 
the secret key:value pair but you can update it.
•
If you want to control how long a deployment can exist, create a lease.
•
To modify user inputs at request time, you can modify the custom forms of the Private AI blueprints.
Installing and Configuring Private AI Services
As a VI administrator, after you configure your GPU-accelerated workload domain and clusters with support for vSphere 
Supervisor and VCF Automation, you can install Private AI Services on a Supervisor for an organization. Then, the 
organization administrator or MLOps engineer can activate Private AI Services in the namespaces that are created for the 
organization.
VMware by Broadcom
97


---

VMware Private AI Foundation with NVIDIA 9.1
In This Chapter
•
Deployment Artifacts for Private AI Services 3.0 in a Disconnected Environment
•
Deployment Artifacts for Private AI Services 2.1 in a Disconnected Environment
•
Provide an Ubuntu Package Repository Mirror for Private AI Services in a Disconnected Environment
•
Upload the Private AI Services 3.0 Components to a Disconnected Environment
•
Upload the Private AI Services 2.1 Components to a Disconnected Environment
•
Install Private AI Services on the Supervisors of the Workload Domain
•
Activate Private AI Services 3.0 in an Organization Namespace by Using the UI
•
Activate Private AI Services 3.0 in an Organization Namespace by Using the kubectl Command
•
Activate Private AI Services 2.1 in an Organization Namespace by Using the VCF Automation UI
•
Activate Private AI Services in an Organization Namespace by Using the kubectl Command
•
Visualizing Private AI Services Metrics in Observability Platforms
Identity and Access Management Requirements for Private AI Services
Private AI Services uses OpenID Connect (OIDC) based authentication to control access to its functional modules. You 
can either connect Private AI Services to an external OIDC provider or VCF Automation, or, for proof-of-concept, use local 
user accounts that are managed by a built-in OIDC provider.
Private AI Services works with OIDC clients, one client per OIDC provider, with support for the Authorization Code grant 
type with PKCE and enabled OIDC Discovery.
You control access to the entire Private AI Services instance, including its UI, API, running model endpoints, and 
integrated features.
Note:
•
You can configure Private AI Services 3.0 with multiple OIDC providers for a single namespace.
•
You can configure Private AI Services 2.1 only with a single OIDC provider.
Note:
Connecting Private AI Services directly to VCF Single Sign-on and identity broker is not supported because 
the two entities are connected to different networks. VCF Single Sign-On uses the management network in the 
management domain while Private AI Services uses Supervisor networking in the workload domain.
VMware by Broadcom
98


---

VMware Private AI Foundation with NVIDIA 9.1
Private AI Services supports the following authentication provider types:
Provider Type
Private AI Services Version
Description
VCF Automation
Private AI Services 3.0
Connect to the identity provider proxy server of VCF Automation. The 
proxy server redirects authentication requests to the identity providers 
for the organizations that can access the Private AI Services instance 
in the namespace according to the setup. You use a single OIDC client 
while providing organization-aware access to a Private AI Services
instance. You can use VCF Automation for authentication in simple 
single-organization deployments too.
Note:
To use VCF Automation for authentication in Private AI 
Services 2.1, register it as an OIDC provider.
OIDC
•
Private AI Services 2.1
•
Private AI Services 3.0
Connect directly to a provider that is running on third-party software and 
is compliant with the OIDC specification. The Private AI Services client 
on a standard OIDC Provider must support the Authorization Code 
grant type with PKCE and OIDC Discovery.
•
In Private AI Services 3.0, you can connect to one or more OIDC 
providers.
•
In Private AI Services 2.1, you can connect to only one OIDC 
provider.
In Private AI Services 2.1, you can also use this provider type for 
authentication by using VCF Automation. VCF Automation redirects 
requests to its connected identity providers.
Local user accounts
Private AI Services 3.0
Use the OIDC Provider that is built in Private AI Services . It maps 
local user accounts to an OIDC client. You can use this authentication 
mechanism for proof-of-concept to work around the requirement to 
have an OIDC provider.
Custom OIDC
Private AI Services 3.0
Connect directly to one or more external third-party providers that are 
generally compliant with the OIDC specification but are deviating from 
the OIDC standard. The Private AI Services client on a custom OIDC 
Provider must be configured with a public (non-confidential) OIDC 
client providing fine-grained control of the authentication component, 
including custom authorization rules, for non-standard identity provider 
setups.
Important:
To use this type of OIDC provider, contact Broadcom Technical 
Support.
Note:
You cannot configure Private AI Services with this provider in 
the UI.
General Requirements for OIDC Providers
Private AI Services requires an OIDC client supporting the Authorization Code grant type with PKCE to obtain an ID Token 
and Access Token as well as a Refresh Token. For more information about OIDC-based communication, see the OIDC 
specification.
Requirements for OIDC Providers
Work with your OIDC provider administrator to set up identity and access management.
Verify that your environment has an OpenID Connect (OIDC) provider that is configured in the following way:
•
Public (non-confidential) OIDC client application.
VMware by Broadcom
99


---

VMware Private AI Foundation with NVIDIA 9.1
•
Authorization Code grant type with PKCE.
•
Enabled Refresh Token and Authorization Code grant types.
•
If supported by the OIDC provider, enabled Offline Access scope, that is required for Refresh Tokens enablement.
•
Optional. Private AI Services users are organized in one or more groups.
•
A claim for the groups for Private AI Services, called groups, included in the Access Token.
•
A scope, called groups, containing the claim for the groups for Private AI Services, created even if using groups is not 
required for your environment.
•
Enabled HTTPS-based communication.
•
Supported <issuer_url>/.well-known/openid-configuration discovery endpoint.
•
Redirect URI set to https://<private_ai_services_FQDN_or_IP_address>/. Including the trailing /
character is mandatory.
Using VCF Automation for Authentication
VCF Automation supports several standards for authentication. In this way, you can authenticate users or service 
accounts to Private AI Services without having to maintain an external OIDC provider. By using VCF Automation, you 
can also connect Private AI Services to VCF Single Sign-On and the identity providers that VCF Single Sign-On redirects 
requests to.
You can use VCF Automation for sharing the capabilities of a Private AI Services instance across organizations in the 
following way:
•
If you are running Private AI Services on VCF Automation in VCF 9.1.1, access to model endpoints in the Private AI 
Services instance is granted to members of entitled organizations.
If the list with organizations is empty, members of any organization can access the Private AI Services instance.
•
Specifically for Private AI Services 3.0 on VCF Automation in VCF 9.1.1, you can also use OIDC proxy servers that are 
created in entitled organizations.
Then, in entitled organizations, you can grant roles or groups access to Private AI Services. When Private AI Services 
forwards an authentication request to VCF Automation, then it verifies if the account is a member of an organization and if 
the account is a member of an LDAP group or has a role in this organization.
Configure VCF Automation as a Provider
You configure identity and access management in VCF Automation Provider Management and organizations according to 
the requirements of your private cloud design. You can use one or more identity providers or groups in the organizations.
VI administrator
If you plan to use an OIDC provider proxy server running in Provider Management , perform the following 
steps:
1.
In the VCF Automation Provider Management, set up identity and access management as follows:
– VCF Single Sign-On
See Identity Broker Models and Configuring VCF Single Sign-On in the VCF documentation.
– External identity providers
See Managing Identity Providers in VCF Automation in the VCF documentation.
2.
In the OIDC Services > Relying Parties UI, configure VCF Automation Provider 
Management as an OIDC provider proxy server, setting the redirect URI to
https://private_ai_services_FQDN_or_IP_address/.
Select Is Public and make sure that Requires PKCE is on.
In a VCF 9.1.1 environment, select also the Is Global check box.
See configure VCF Automation as an OIDC provider proxy server in the VCF documentation.
VMware by Broadcom
100


---

VMware Private AI Foundation with NVIDIA 9.1
3.
Again in the OIDC Services > Relying Parties UI, from the vertical ellipsis menu next to the name of 
the relying party ID, select Copy ID, and copy and save the OIDC client ID.
VMware by Broadcom
101


---

VMware Private AI Foundation with NVIDIA 9.1
4.
In the Access Control > Global Roles UI section, create a global role specifically for Private AI 
Services and publish it so that it is available to organizations. You do not have to add rights to the role.
See Managing Global VCF Automation Roles in the VCF documentation.
Organization 
administrator
If you plan to use an OIDC provider proxy server in an individual organization , perform the following steps:
1.
In the VCF Automation organization UI, set up identity and access management. See Configure an 
organization to use an OpenID identity provider and import users and groups from the OIDC provider in 
the VCF documentation.
2.
On the Administer tab in the organization portal, click OIDC Services > Relying Parties.
3.
Configure VCF Automation Provider Management as an OIDC provider proxy server, setting the 
redirect URI to https://private_ai_services_FQDN_or_IP_address/.
See configure VCF Automation as an OIDC provider proxy server in the VCF documentation.
VMware by Broadcom
102


---

VMware Private AI Foundation with NVIDIA 9.1
4.
Again in the OIDC Services > Relying Parties UI, from the vertical ellipsis menu next to the name of 
the relying party ID, select Copy ID, and copy and save the OIDC client ID.
5.
On the Administer tab, click Access Control and assign the role for Private AI Services access to 
users in your organizations.
Configuring Private AI Services for Sharing Models Across Organizations
To enable sharing model endpoints with another Private AI Services instance, you must configure Private AI Services in a 
specific way.
Cross-organization model sharing is a capability in Private AI Services 3.0 that lets a central provider Private AI Services
instance share its running models with one or more consumer Private AI Services instances without running the required 
models in each instance.
Each deployed model consumes significant CPU, memory, and, if required, GPU resources within its namespace. 
When multiple organizations need access to the same LLM or embedding model, deploying it independently in each 
namespace multiplies resource consumption and operational overhead. Organizations can share a model endpoint across 
organizational boundaries in the following way:
•
Designate a namespace and the Private AI Services instance in it as the provider.
•
Configure consumer Private AI Services instances to route inference requests to the provider.
A consumer Private AI Services instance can still use additional model endpoints running locally in its namespace.
For more information about using models in another Private AI Services instance, see Connect to a Shared Model 
Running in Another Private AI Services.
Provider and Consumer Instances
In a model sharing use case, a Private AI Services instance can have one of the following roles. A Private AI Services 
instance can act as a provider, a consumer, or both.
Role
Description
Who configures and uses it
Provider instance
Hosts models and shares them with entitled 
consumer organizations and the Private AI 
Services instance in them. The namespace of the 
provider instance must have sufficient GPU or 
CPU resources.
Configured by:
•
VI administrator for environments without 
VCF Automation
•
Provider administrator or organization 
administrator
Next, provider administrators or organization 
administrators deploy models using model 
endpoints. See Running Completion or 
Embedding Models by Using Model Endpoints.
Consumer instance
Routes inference requests for shared models to 
the provider instance. A consumer instance does 
not have to run the models locally.
Configured by:
•
VI administrator for environments without 
VCF Automation
•
Organization administrator
Next, organization administrators route models in 
the namespace for end-user consumption. See
Connect to a Shared Model Running in Another 
Private AI Services.
Authentication Options for Model Sharing
In general, you can use the following authentication configurations for model sharing across organizations:
•
For VCF 9.1.1 environments with multiple organizations. You specify which VCF Automation organizations are 
entitled to access the provider Private AI Services instance. You create a central OIDC provider proxy server in VCF 
Automation Provider Management or an organization-specific OIDC provider proxy server, and API tokens.
In VCF 9.1.0 environments, all organizations can access a provider Private AI Services instance.
•
For environments without VCF Automation. You use any OIDC server that supports JWT-based API tokens or local 
accounts with API tokens for the built-in OIDC provider of Private AI Services.
VMware by Broadcom
103


---

VMware Private AI Foundation with NVIDIA 9.1
For information about identity and access management in Private AI Services, see Identity and Access Management 
Requirements for Private AI Services.
For information about configuring a Private AI Services instance during activation, see Activate Private AI Services 3.0 
in an Organization Namespace by Using the UI and Activate Private AI Services 3.0 in an Organization Namespace by 
Using the kubectl Command.
Deployment Artifacts for Private AI Services
Deployment Artifacts for Private AI Services 3.0 in a Disconnected Environment
Private AI Services uses resources from the Internet in a connected environment. In a disconnected (air-gapped) 
environment, as a VI administrator, you must make these resources available locally.
The core functionality of Private AI Services 3.0 is distributed in two ways: integrated in the software depot for VCF
9.1.1 and separately from VCF. For disconnected environments, use the option where Private AI Services is separately 
distributed. Private AI Services 3.0 also uses NVIDIA and Kubernetes images and inference engine images. These 
artifacts must be available locally too.
In Private AI Services 3.0, deployment artifacts are distributed into two streams to comply with software distribution 
policies regarding third-party proprietary software:
•
Private AI Services Supervisor Service package
– Published directly using the software depot of your VCF environment or externally on the Broadcom Artifactory 
instance.
– Contains no third-party images.
•
Third-party packages
– Available on the Broadcom Artifactory instance.
– Inference engines package. Distributed as a thick bundle containing the images of the llama.cpp, vLLM, and Infinity 
inference engines.
– NVIDIA GPU Operator package. Distributed as a thin bundle referencing container images on the NVIDIA NGC 
registry.
The NVIDIA GPU Operator package has two variants - vGPU package and GPU passthrough. Using the vGPU 
package requires an NVIDIA AI Enterprise license.
VMware by Broadcom
104


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 12: Artifacts for Private AI Services 3.0 in a Disconnected VMware Private AI Foundation with NVIDIA
Environment
Ubuntu Package 
Repository
GPU Operator Helm Chart
vGPU Guest Driver
Platform Harbor Registry
Web Server
Private AI Services 
Package
VKr Image
Local Content Library
Data  Center GPU driver
llama.cpp
vLLM
Infinity
GPU Operator Package
Inference Engine Package
Deployment Artifact
Source Location
Staging Location
Mirroring Method
VKr images
VKr content library in a 
software depot in offline 
or disconnected mode
Subscribed content 
library in vCenter
Manually
See Configure a Content Library 
with Ubuntu VKr for a Disconnected 
VMware Private AI Foundation with 
NVIDIA Environment.
Private AI Services Carvel package
Broadcom Artifactory
Platform Harbor registry
NVIDIA GPU Operator Helm chart
Broadcom Artifactory
Platform Harbor registry
NVIDIA GPU Driver package
NVIDIA NGC catalog
This package can be 
hosted in another source 
OCI registry.
Platform Harbor registry
Inference engines package
Broadcom Artifactory
Platform Harbor registry
Using the artifact mirroring tool (AMT)
See Upload the Private AI Services 
3.0 Components to a Disconnected 
Environment.
Ubuntu package repository
archive.ubuntu.com
Web server
Manually
See Provide an Ubuntu Package 
Repository Mirror for Private 
AI Services in a Disconnected 
Environment.
Deployment Artifacts for Private AI Services 2.1 in a Disconnected Environment
Private AI Services uses resources from the Internet in a connected environment. In a disconnected (air-gapped) 
environment, as a VI administrator, you must make these resources available locally.
The core functionality of Private AI Services 2.1 is distributed separately from VCF. The package also uses NVIDIA and 
Kubernetes images. These artifacts must be available locally.
VMware by Broadcom
105


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 13: Artifacts for Private AI Services 2.1 in a Disconnected VMware Private AI Foundation with NVIDIA
Environment
Ubuntu Package 
Repository
GPU Operator Helm Chart
vGPU Guest Drivers
Platform Harbor Registry
Web Server
Kubernetes Node Feature 
Discovery Helm Chart
DCGM Exporter
MIG Device Plugin
vGPU Device Manager
Private AI Services 
Package 
Kubernetes Node Feature 
Discovery Image
VKr Image
Local Content Library
Data  Center GPU driver
Deployment Artifact
Source Location
Staging Location
Mirroring Method
VKr images
Public VKr content library Local content library in 
vCenter
Manually
See Configure a Content Library 
with Ubuntu VKr for a Disconnected 
VMware Private AI Foundation with 
NVIDIA Environment.
Private AI Services Carvel package
Broadcom Artifactory
Platform Harbor registry
NVIDIA GPU Operator Helm chart and 
required container images
NVIDIA NGC catalog
Platform Harbor registry
Kubernetes Node Feature Discover 
Kubernetes container 
Platform Harbor registry
Using the artifact mirroring tool
See Upload the Private AI Services 
2.1 Components to a Disconnected 
Environment.
VMware by Broadcom
106


---

VMware Private AI Foundation with NVIDIA 9.1
Deployment Artifact
Source Location
Staging Location
Mirroring Method
Ubuntu package repository
archive.ubuntu.com
Web server
Manually
See Provide an Ubuntu Package 
Repository Mirror for Private 
AI Services in a Disconnected 
Environment.
Provide an Ubuntu Package Repository Mirror for Private AI Services in a 
Disconnected Environment
A local Ubuntu repository is required for installing the vGPU guest driver or GPU data center driver in the VKS cluster that 
runs model endpoints in Private AI Services.
Verify that you have at least 800 GB storage space on the machine where you plan to mirror the Ubuntu package 
repository.
For more information, including the required packages, see Install NVIDIA GPU Operator in Air-Gapped Environments in 
the NVIDIA documentation.
1. Provide a local Ubuntu package repository on a Web server.
You can create the following custom-repo.list file in a text editor.
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy-updates main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy-security main universe
If you are using GPU passthrough, this repository can be a partial mirror with only the packages that are required for 
the NVIDIA GPU Operator driver compilation.
2. Prepare an index file for the repository.
You pass the index file to the artifact mirroring utility when you push the artifacts for Private AI Services to the private 
Harbor registry. The utility inserts the content of the index file in a generated custom Supervisor Service configuration 
to point Private AI Services to the Ubuntu package repository.
For example, you can create the following custom-repo.list file in a text editor.
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy-updates main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu jammy-security main universe
Upload Private AI Services Components in a Disconnected Environment
Upload the Private AI Services 3.0 Components to a Disconnected Environment
In a disconnected VMware Private AI Foundation with NVIDIA environment, as a VI administrator, before you install the
Private AI Services 3.0 on a Supervisor instance, upload the Private AI Services package and the NVIDIA GPU Operator 
and the inference engines packages to a Harbor project by using the artifact mirroring tool (AMT).
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for
Private AI Services. See Setup Workflows for Private AI Services.
•
Verify that you have a local Ubuntu package repository available and an index file, for example, called custom-
repo.list, for it available on the machine that you use to access the disconnected environment, that is, your admin 
host. See Provide an Ubuntu Package Repository Mirror for Private AI Services in a Disconnected Environment.
•
Verify that the Harbor administrator has created a project for Private AI Services in the platform Harbor registry.
•
Verify that the platform Harbor registry has 40 GB of storage for hosting the Private AI Services artifacts.
•
Install VCF Consumption CLI on the hosts that you use to download files from the Internet and upload them to the 
environment, such as bastion and admin hosts.s
You use VCF Consumption CLI for downloading packages to a host and then for uploading them to OCI registries.
VMware by Broadcom
107


---

VMware Private AI Foundation with NVIDIA 9.1
•
Verify that the admin and bastion hosts have Docker installed.
Private AI Services is installed as a package on top of VCF and a Private AI Services version is available at the release 
date of VCF. You can install or update to later versions of Private AI Services without the need to update VCF. See
VMware Private AI Services Release Notes.
Table 12: Artifacts Downloaded or Used by the Artifact Mirroring Tool
Artifact
Default Download Location
Private AI Services package
pais-docker.packages.broadcom.com
Inference engines package
packages.broadcom.com
NVIDIA GPU Operator Helm chart
https://helm.ngc.nvidia.com/nvidia
NVIDIA container images
nvcr.io
Ubuntu package repository
archive.ubuntu.com
For more information about the deployment artifacts of Private AI Services in a disconnected environment, see
Deployment Artifacts for Private AI Services 3.0 in a Disconnected Environment.
1. In a connected environment, from the Broadcom Support Portal, get your access token for the Broadcom OCI 
registries including pais-docker.packages.broadcom.com.
a) Go to the My Downloads page on the Broadcom Support Portal and click Registry Tokens.
b) On the Generate Registry Tokens page, click Generate Token.
c) Click the Copy Token icon in front of the token and save it.
2. From the Broadcom Support Portal, download the YAML definition file for Private AI Services.
a) Go to the VMware Cloud Foundation download area and locate the Private AI Services group.
You can also download the YAML definition file from the Private AI Services download area.
b) Open the Private AI Services group and download the pais-3.0.0-external.yaml YAML file for the service 
on a machine that has Internet access and that is running the VCF Consumption CLI.
•
pais-3.0.0-external.yaml points to the Private AI Services package on pais-
docker.packages.broadcom.com.
3. On the bastion and admin hosts, install the pais plug-in in the VCF Consumption CLI or if you have it installed, 
upgrade it to its version for Private AI Services 3.0.0.
The artifact mirroring tool (AMT) for Private AI Services is available as part of the pais CLI plug-in.
a) Verify that the pais plug-in with its right version is available for installation.
vcf plugin search
b) Install the plug-in.
vcf plugin install pais --version v3.0.0
c) Verify that the plug-in is installed.
vcf plugin list
4. Go to the directory where you want to download the artifacts.
5. Log in to the source registries by running the docker command.
pais-docker.packages.broadcom.com stores the Private AI Services package and nvcr.io/nvidia/vgpu
stores the NVIDIA vGPU guest driver container.
export BROADCOM_TOKEN="your-broadcom-registry-token"
export NGC_API_KEY="your-ngc-api-key"
docker login pais-docker.packages.broadcom.com -u email_address_for_login_to_Broadcom_support_portal --
password-stdin <<< "${BROADCOM_TOKEN}"
docker login nvcr.io/nvidia/vgpu -u $oauthtoken --password-stdin <<< "${NGC_API_KEY}"
VMware by Broadcom
108


---

VMware Private AI Foundation with NVIDIA 9.1
6. Pull the artifacts for installing Private AI Services on the Supervisor in the workload domain by running some of the 
following commands.
The following command downloads the artifacts from their default location to a sub-directory, called pais-store.
vcf pais amt pull path-to-private-ai-services-YAML-file --local-store ./pais-store
The following command downloads the artifacts from their default location downloading only a specific NVIDIA GPU 
Operator variant - vGPU or GPU passthrough according to your deployment type.
vcf pais amt pull path-to-private-ai-services-YAML-file --gpu-operator-variant nvaie --local-store ./pais-
store
vcf pais amt pull path-to-private-ai-services-YAML-file --gpu-operator-variant oss --local-store ./pais-
store
7. Copy the content of the pais-store directory to the admin host.
8. Copy the custom-repo.list file to the admin host.
9. On the admin host, open a command line and go to the parent directory of pais-store.
10. Log in to the platform Harbor registry by running the docker command.
docker login your_private_harbor_fqdn_or_ip_address -u your_harbor_user_name --password-stdin
11. Run the following command to push the locally stored artifacts to the allocated Harbor project and the local Ubuntu 
package repository.
vcf pais amt push your_private_harbor_fqdn_or_ip_address_with_prefix/your_project-name --ubuntu-repo-list 
 "$(cat path-to-custom-repo-list-directory/custom-repo.list)" 
•
The artifacts required for Private AI Services are uploaded to the allocated Harbor project.
•
The pais-store now contains an updated version of the pais-3.0.0-external.yaml installation file and a
yaml-svc-cfg.yaml service configuration file that you will use when installing Private AI Services on a Supervisor in 
the workload domain. See Install Private AI Services on the Supervisors of the Workload Domain.
Upload the Private AI Services 2.1 Components to a Disconnected Environment
In a disconnected VMware Private AI Foundation with NVIDIA environment, as a VI administrator, before you install the
Private AI Services 2.1 on a Supervisor instance, upload the Private AI Services package and the components for the 
NVIDIA GPU Operator to a Harbor project by using the artifact mirroring utility.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for
Private AI Services. See Setup Workflows for Private AI Services.
•
Verify that you have a local Ubuntu package repository available and an index file, for example, called custom-
repo.list, for it available on the machine that you use to access the disconnected environment. See Provide an 
Ubuntu Package Repository Mirror for Private AI Services in a Disconnected Environment.
•
Verify that the Harbor administrator has created a project for Private AI Services in the private Harbor registry.
•
Verify that the platform Harbor registry has 20 GB of storage for hosting the Private AI Services artifacts.
•
Install VCF Consumption CLI on the hosts that you use to download files from the Internet and upload them to the 
environment, such as bastion and admin hosts.
You use VCF Consumption CLI for downloading packages to a host and then for uploading them to OCI registries.
•
Verify that the admin and bastion hosts have Docker installed.
Private AI Services is installed as a package on top of VCF and a Private AI Services version is available at the release 
date of VCF. You can install or update to later versions of Private AI Services without the need to update VCF. See
VMware Private AI Services Release Notes.
The Private AI Services 2.1 Supervisor Service artifact is hosted on the pais-docker.packages.broadcom.com OCI 
registry, which requires authentication for accessing the artifacts in it. You must add the credentials for authentication to 
the registry to the Supervisor configuration before installing the service.
VMware by Broadcom
109


---

VMware Private AI Foundation with NVIDIA 9.1
Table 13: Artifacts Downloaded or Used by the Artifact Mirroring Tool
Artifact
Download Location
Private AI Services package
packages.broadcom.com
NVIDIA GPU Operator Helm chart
https://helm.ngc.nvidia.com/nvidia/charts/gpu-operator-v25.3.0.tgz
NVIDIA container images
nvcr.io
Kubernetes Node Feature Discovery Helm chart
https://github.com/kubernetes-sigs/node-feature-discovery/releases/download/v 
0.17.2/node-feature-discovery-chart-0.17.2.tgz
Kubernetes Node Feature Discovery image
registry.k8s.io/nfd/node-feature-discovery:v0.17.2
Ubuntu package repository
archive.ubuntu.com
For more information about the deployment artifacts of Private AI Services in a disconnected environment, see
Deployment Artifacts for Private AI Services 2.1 in a Disconnected Environment.
1. In a connected environment, from the Broadcom Support Portal, get your access token for the Broadcom OCI 
registries.
a) Go to the My Downloads page on the Broadcom Support Portal and click Registry Tokens.
b) On the Generate Registry Tokens page, click Generate Token.
c) Click the Copy Token icon in front of the token and save it.
2. From the Broadcom Support Portal, download the YAML definition file for Private AI Services and the OCI registry 
credentials for authentication to pais-docker.packages.broadcom.com.
a) Go to the Private AI Services download area and locate the Private AI Services version that you want to install.
b) Click the Private AI Services version and download the YAML file, pais.yml, for the service on a machine that 
has Internet access and that is running the VCF Consumption CLI.
3. On the bastion and admin hosts, install the pais plug-in in the VCF Consumption CLI or if you have it installed, 
upgrade it to its version for Private AI Services 2.1.
The artifact mirroring tool (AMT) for Private AI Services is available as part of the pais CLI plug-in.
a) Verify that the pais plug-in with its right version is available for installation.
vcf plugin search
b) Install the plug-in.
vcf plugin install pais --version v2.1.0
c) Verify that the plug-in is installed.
vcf plugin list
4. Go to the directory where you want to download the artifacts.
5. Log in to the pais-docker.packages.broadcom.com and nvcr.io registries by running the docker command.
pais-docker.packages.broadcom.com stores the Private AI Services package and nvcr.io/nvidia/vgpu
stores the NVIDIA vGPU guest driver container.
export BROADCOM_TOKEN="your-broadcom-registry-token"
export NGC_API_KEY="your-ngc-api-key"
docker login pais-docker.packages.broadcom.com -u email_address_for_login_to_Broadcom_support_portal --
password-stdin <<< "${BROADCOM_TOKEN}"
docker login nvcr.io/nvidia/vgpu -u $oauthtoken --password-stdin <<< "${NGC_API_KEY}"
6. Run the following command to pull the artifacts for installing Private AI Services on the Supervisor in the workload 
domain.
The artifact mirroring tool downloads the artifacts to a sub-directory, called pais-store.
vcf pais amt pull path-to-private-ai-services-YAML-file
For example:
VMware by Broadcom
110


---

VMware Private AI Foundation with NVIDIA 9.1
vcf pais amt pull ./pais.yml
7. Copy the content of the pais-store directory to the admin host.
8. Copy the custom-repo.list file to the machine that has access to the disconnected environment.
9. On the admin host, open a command line and go to the parent directory of pais-store.
10. Log in to your local Harbor registry by running the docker command.
docker login your_private_harbor_fqdn_or_ip_address -u your_harbor_user_name --password-stdin
11. Run the following command to push the locally stored artifacts to the allocated Harbor project and the local Ubuntu 
package repository.
vcf pais amt push your_private_harbor_fqdn_or_ip_address_with_prefix/your_project-name --ubuntu-repo-list 
 "$(cat path-to-custom-repo-list-directory/custom-repo.list)" 
•
The artifacts required for Private AI Services are uploaded to the allocated Harbor project.
•
The pais-store contains an updated version of the pais.yaml installation file and a yaml-svc-cfg.yaml service 
configuration file that you use when installing Private AI Services on a Supervisor in the workload domain.
Install Private AI Services on the Supervisors of the Workload Domain
As a VI administrator, to enable the functionality of Private AI Services , add the Private AI Services Supervisor Service to 
the Supervisors in the workload domain.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for
Private AI Services. See Setup Workflows for Private AI Services.
•
If you are installing Private AI Services 3.0 on VCF 9.1.1 from the software depot, verify if the following conditions are 
in place:
– You have a regional Harbor registry connected to the software depot and that the vcf project there contains the 
package.
– The regional Harbor registry is added as the default container registry to the Supervisor cluster.
If you are installing Private AI Services 3.0 from pais-docker.packages.broadcom.com, you add the platform 
Harbor registry as the default container registry to the Supervisor cluster manually.
Private AI Services is installed as a package on top of VCF. A Private AI Services version is available at the release date 
of VCF. AfterVMware Private AI Foundation with NVIDIA is released, you can install or update to later versions of Private 
AI Services without the need to update VMware Private AI Foundation with NVIDIA. See VMware Private AI Services 
Release Notes.
Installation Options
Description
Private AI Services 3.0
The Private AI Services 3.0 Supervisor Service deployment artifact is hosted on two 
alternative locations - on the Broadcom Artifactory pais-docker.packages.broadco 
m.com and in the software depot in your VCF environment.
•
The deployment artifact in the software depot is accessed using a regional Harbor registry 
installed in VCF Automation or directly in vSphere Supervisor. The regional Harbor 
registry is a proxy to the online software depot. The credentials for authentication to the 
registry must be added to the target Supervisor cluster before installing the service.
•
Installing the deployment artifact from pais-docker.packages.broadcom.com
requires authentication for accessing the artifacts in it. You must add the credentials for 
authentication to the registry to the Supervisor configuration before installing the service.
Private AI Services 2.1
The Private AI Services 2.1 Supervisor Service artifact is hosted on the pais-docker.p 
ackages.broadcom.com OCI registry, which requires authentication for accessing the 
artifacts in it. You must add the credentials for authentication to the registry to the Supervisor 
configuration before installing the service.
VMware by Broadcom
111


---

VMware Private AI Foundation with NVIDIA 9.1
1. In a connected environment, if you are installing Private AI Services from pais-
docker.packages.broadcom.com, from the Broadcom Support Portal, get your access token for the Broadcom 
OCI registries, such as pais-docker.packages.broadcom.com.
a) Go to the My Downloads page on the Broadcom Support Portal and click Registry Tokens.
b) On the Generate Registry Tokens page, click Generate Token.
c) Click the Copy Token icon in front of the token and save it.
2. From the Broadcom Support Portal, download the YAML definition file for Private AI Services.
a) Go to the VMware Cloud Foundation download area and locate the Private AI Services group.
You can also download the YAML definition file from the Private AI Services download area.
b) Open the Private AI Services group and download the YAML file for the service.
For Private AI Services 3.0, download one of the following files according to your requirements:
•
pais-3.0.0.yml points to the Private AI Services package on a regional Harbor registry which in its turn 
points to the online software depot. Use this option with VCF 9.1.1 in a connected environment.
•
pais-3.0.0-external.yaml points to the Private AI Services package on pais-
docker.packages.broadcom.com. Use this option in connected or disconnected environments with VCF
9.1.0 and VCF 9.1.1.
3. In a disconnected environment, provide the following resources by using the artifact mirroring tool for Private AI 
Services.
1. Upload the Private AI Services and the third-party artifacts to a local Harbor registry.
2. Locate the pais.yml and yaml-svc-cfg.yaml files created by the tool.
For Private AI Services 3.0, the artifact mirroring tool works with the pais-3.0.0-external.yaml file.
See Upload the Private AI Services 3.0 Components to a Disconnected Environment and Upload the Private AI 
Services 2.1 Components to a Disconnected Environment .
4. Log in to the vCenter instance for the management domain at https://<vcenter_fqdn>/ui as Administrator.
5. In the vSphere Client side panel, click Supervisor Management and navigate to the Supervisor for the workload 
domain.
6. On the Configure tab, select Supervisor > Container Registries.
7. If you are installing Private AI Services from pais-docker.packages.broadcom.com, click Add Registry, enter 
the following information, and click Add.
Setting
Value
Registry Name
pais-reg-credentials
Registry Host URL
•
pais-docker.packages.broadcom.com for a 
connected environment
•
your-harbor-registry-url for a disconnected 
environment
Username
•
email_address_for_login_to_Broadcom_suppo 
rt_portal  for a connected environment
•
your-harbor-registry-username for a disconnected 
environment
Password
your_OCI_registry_access_token
VMware by Broadcom
112


---

VMware Private AI Foundation with NVIDIA 9.1
8. In the side panel of the vSphere Client, click Private AI Foundation.
9. In the Private AI Foundation workflow, in the Set Up Environment section, click Start.
10. Next to Install the Supervisor Service for Private AI Services, click Start.
11. Select the workload domain and click Next.
12. Upload the service definition YAML file for Private AI Services and click Next.
For a connected environment, use the pais.yml file you downloaded from the Broadcom Support Portal.
For a disconnected environment, use the pais.yml file created by the artifact mirroring tool.
13. Configure the service installation and click Next.
For a connected environment, select the service version and the Supervisor of the workload domain, leave the YAML 
Service Config box empty.
For a disconnected environment, in the YAML Service Config box, select the service version and the Supervisor of 
the workload domain, and paste the content of the yaml-svc-cfg.yaml file created by the artifact mirroring tool .
•
After you set up an organization for VMware Private AI Foundation with NVIDIA in VCF Automation, activate Private AI 
Services in the namespace. See Activate Private AI Services in an Organization Namespace.
•
To manage the Supervisor Service for Private AI Services, navigate to the Supervisor Management section of the 
vSphere Client, and on the Services tab, locate the Private AI Services card.
Activating Private AI Services 3.0
Activate Private AI Services 3.0 in an Organization Namespace by Using the UI
After you configure a namespace in an organization in VCF Automation as an organization administrator or directly in 
the vSphere Supervisor as a VI administrator, you can activate Private AI Services in this namespace for provisioning 
generative AI applications including model endpoints and agents communicating to the endpoints, and for RAG 
applications, knowledge bases.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for
Private AI Services. See Setup Workflows for Private AI Services.
•
For activating Private AI Services in a namespace directly in vSphere Supervisor, verify that you have the Local 
Consumption Interface installed on the Supervisor instance. See Installing and Configuring the Local Consumption 
Interface in the VMware Cloud Foundation Service Administration and Development documentation.
•
Verify that the namespace includes a VM class and storage policy as a storage class for the control plane of the VKS 
cluster running model endpoints. The control plane nodes do not require GPU resources.
•
If required by your organization, verify that an FQDN is allocated for the Private AI Services instance.
You map it to an IP address after the Private AI Services instance is active.
•
If you plan to use knowledge bases in Data Indexing and Retrieval, MCP tools, or agents in Agent Builder, create 
a PostgreSQL database with pgvector extension or get the connection details to an available one. The database is 
required to store vector embeddings for the knowledge bases in Data Indexing and Retrieval and for Agent Builder.
– If you are using VMware Data Services Manager, create the database in VCF Automation. See Create a Database 
Using VMware Data Services Manager in VMware Cloud Foundation Automation documentation. The pgvector 
extension is installed automatically.
– You can also create a PostgreSQL database with pgvector extension using third-party software. The PostgreSQL 
database must have the pgvector extension installed.
Private AI Services activates the installed pgvector extension during its activation in the namespace.
•
Get the CA certificate of the PostgreSQL database.
•
If you plan to use an OIDC provider for managed access to Private AI Services, verify that a third-party OIDC provider 
or VCF Automation is configured according to the Identity and Access Management Requirements for Private AI 
Services.
– For an external OIDC provider, ask your OIDC provider administrator for the following details:
•
Required. Issuer URL.
VMware by Broadcom
113


---

VMware Private AI Foundation with NVIDIA 9.1
•
Required. Client ID for the Private AI Services application.
•
Required. The list of groups with Private AI Services users. If groups are not used, you can provide an empty 
group list in the Private AI Services activation YAML code.
•
Name of the scope for the Private AI Services groups if the name is different from groups.
•
Name of the claim with the Private AI Services groups if the name is different from groups.
•
Required. Issuer certificate of the OIDC provider.
– For VCF Automation as an authentication provider, according to where the OIDC relaying party is configured, ask 
your VI administrator or organization administration for the following details:
•
Required. Client ID for the Private AI Services application. You can retrieve it from the dedicated OIDC provider 
proxy server in VCF Automation Provider Management. See Configure VCF Automation as a Provider.
•
Required. Issuer certificate of your VCF Automation instance.
For example, on the machine that you use to access your environment, run the following OpenSSL command.
openssl s_client -connect vcf_automation_fqdn:443 -showcerts </dev/null 2>/dev/null | awk 
 'BEGIN{c=0;p=0} /-----BEGIN CERTIFICATE-----/{c++;if(c>1)p=1} p{print} /-----END CERTIFICATE-----/
{p=0}'
•
For sharing models with other organizations, the names of these organizations and entitled global roles or LDAP 
groups in them.
•
For activating API token generation for local accounts in the VCF Automation UI, verify that the following conditions are 
in place:
– You have a machine with installed VCF Consumption CLI and kubectl. See Getting Started with the VCF 
Consumption CLI.
You activate API token generation by using commands after you activate Private AI Services in the VCF Automation
UI.
– Verify that you have the certificate of your VCF Automation instance saved to your machine, for example, as a
vcf_automation_certificate.pem file in your current working folder.
In an environment with VCF Automation, when you log in to the organization in the VCF Automation UI and switch to 
the namespace, GPU-accelerated or CPU-only, under Services, you see the Private AI entry. However, you cannot yet 
create model endpoints that are required for provisioning generative AI applications because Private AI Services is not yet 
active in the namespace. Similarly, if you use the Local Consumption Interface in the vSphere Client, when you switch to 
the namespace for the Supervisor in the Supervisor Management section of the vSphere Client, on the Resources tab, 
you see the Private AI entry but you must configure Private AI Services first before you can create model endpoints and 
other constructs.
The Private AI Services configuration wizard supports two activation paths - full set of capabilities and Model Runtime 
only. For example, you can use the Model Runtime only path if you plan only to run models. For information about the 
capabilities of Private AI Services, see Delivering Generative AI Applications by Using Private AI Services.
•
The Model Runtime only option includes requirements for Private AI Services node resources, authentication, driver for 
running model endpoints according to the GPU consumption model, and observability.
•
In addition to Model Runtime, the full set of capabilities includes MCP server integration, Data Indexing and Retrieval, 
and Agent Builder.
This activation path also includes the configuration of the PostgreSQL database for storing vector embeddings for 
knowledge bases and information for Agent Builder.
1. Open the wizard for activating Private AI Services in VCF Automation or in the Supervisor Management section of 
the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation as an organization administrator.
2.
On the Build&Deploy tab, select the namespace where you plan to activate Private AI Services.
3.
Under Services, click Private AI and, on the Private AI Services page, click Configure Service.
vSphere Client
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at 
https://<vcenter_fqdn>/ui as a user with the Administrator role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace where you plan to activate Private AI Services.
3.
On the Resources tab, click Private AI and, on the Private AI Services page, click Configure 
Service.
VMware by Broadcom
114


---

VMware Private AI Foundation with NVIDIA 9.1
2. On the Service Resources page of the Private AI Services configuration wizard, select whether you want to deploy 
the full set of capabilities of Private AI Services or only Model Runtime for running models, optionally with metrics 
collection included.
3. Select the default storage policy for the Kubernetes storage class to assign to the Private AI Services management 
components that are deployed in the namespace.
4. In the VKS Control Plane section, select the VM class and optionally a storage policy, different from the default one, 
for the control plane nodes of the VKS cluster for running model endpoints, and click Next.
Worker nodes are created when you deploy a model endpoint. The resources that are required for the worker nodes 
are specified individually for each endpoint at its deployment time.
5. On the Database page of the wizard that is shown for the full capabilities path provide information about the 
PostgreSQL database with pgvector extension installed and click Next.
You provide a different set of properties according to the vendor and location of the vector database.
Database Location
Configuration Details
Database in VMware Data Services Manager
•
If the database has been deployed by using VCF Automation in the same 
namespace, select PostgreSQL Cluster managed by Data Services Manager
and select the database from the drop-down menu.
•
If the database is in a different namespace or has been deployed by the Data 
Services Manager appliance directly, select Bring your own PostgreSQL 
database and enter the details for connecting to the database.
Provide the CA certificate of the database if it is self-signed or the issuer's 
certificate is not publicly available.
Database created by using third-party 
software
Select Bring your own PostgreSQL database and enter the details for connecting to 
the database.
Provide the CA certificate of the database if it is self-signed or the issuer certificate is 
not publicly available.
6. On the User Access page of the wizard, provide the following settings for authentication to the Private AI Services 
instance in the namespace and click Next.
External OIDC 
Providers
To use an external provider that is running on third-party software, in the OIDC Configuration pane, add 
an OIDC provider with the following settings.
Setting
Description
Provider type
OIDC
OIDC Issuer URL
Required. Issuer URL of the OIDC provider as provided by your OIDC 
administrator.
Client ID
Required. Client ID of the OIDC provider as provided by your OIDC 
administrator.
Certificate
Issuer certificate for the OIDC provider as provided by your OIDC 
administrator.
Group Name
If you are going to organize user accounts for Private AI Services in 
groups, activate Use authorized groups and enter the group names, 
separated with commas. For example, a group-for-pais-
access-01, group-for-pais-access-02 value means that 
the user must be a member of the group-for-pais-access-01 or g 
roup-for-pais-access-02 group on the OIDC provider to be able 
to access Private AI Services.
Group Claim
groups
Extra Audiences
Use it if required by your OIDC provider for additional OAuth2.0 clients 
that Private AI Service will accept when validating the Access Token.
VMware by Broadcom
115


---

VMware Private AI Foundation with NVIDIA 9.1
VCF Automation
To configure authentication based on the authentication providers of VCF Automation organizations, in 
the OIDC Configuration pane, add an OIDC provider with the following settings.
Setting
Description
Provider type
VCF Automation
Issuer URL
Required. https://<vcf_automation_fqdn>/oidc.
Client ID
Required. ID of the relying party for Private AI Services in VCF 
Automation Provider Management.
Allowed organizations
Organizations whose members can access the Private AI Services 
instance in the namespace.
If the list is empty, all organizations can access this instance.
Authorized roles
Members of the specified organizations must have at least one of these 
roles to access Private AI Services.
Use it for authentication of users in an external OIDC provider or local 
accounts.
Authorized groups
Members of the specified organizations must be members of at least 
one of these groups to access Private AI Services.
Use it for authentication of users that are members of LDAP groups.
TLS verification mode
For a trusted connection between the Private AI Services instance and 
VCF Automation, use the fully verify option.
Important:
You must add the issuer certificate of VCF Automation as a 
CA trust bundle to the Private AI Services instance itself after 
you activate Private AI Services in the namespace. If you are 
deploying multiple instances of Private AI Services, each in 
its own namespace, you must add the VCF Automation CA 
trust bundle in each one that is using VCF Automation as an 
authentication provider.
Local accounts
You can use local accounts for proof-of-concept purposes and for model sharing across organizations, 
for example, in environments without VCF Automation.
Enter the following settings.
Setting
Description
Issuer base URL
Set to the HTTPS FQDN-based URL at which the 
Private AI Services instance will be accessible. 
You can then access OpenAI compatible Private 
AI Services APIs at this URL.
To set the URL to use the IP address that will be 
assigned to the Private AI Services instance by the 
LoadBalancer Ingress controller, set an arbitrary 
HTTPS URL and remove the value after activation 
by using CLI.
Users
Add user accounts that can access the capabilities 
of the Private AI Services instance. There are no 
special password requirements.
7. In the GPU Driver section of the Private AI Services configuration wizard, set the driver type for running model 
endpoints according to the GPU consumption model and click Next.
Note:
To save GPU resources for other tasks, you can still run CPU-only models using llama.cpp inside namespaces 
configured to support NVIDIA vGPUs or NVIDIA GPU Passthrough.
VMware by Broadcom
116


---

VMware Private AI Foundation with NVIDIA 9.1
Run Models only on 
CPUs
For example, the Use CPUs Only option is suitable for running small models.
Models are run on llama.cpp.
Run Models on NVIDIA 
vGPUs and potentially 
CPU
If you plan to run models on ESX hosts with NVIDIA GPUs that support vGPU mode and that can also 
be used with GPU passthrough with Fixed DirectPath or Dynamic DirectPath as needed, provide the 
following configuration:
•
Select NVIDIA vGPU guest driver.
•
In the vGPU Driver License Token Config Map text box, enter the NVIDIA vGPU client 
configuration token.
•
Provide NVIDIA GPU Operator settings.
For a GPU Operator Overrides template, see https://github.com/NVIDIA/gpu-
operator/blob/v<operator-version>/deployments/gpu-operator/
values.yaml for a supported version according to the Private AI Services release notes. 
For example: https://github.com/NVIDIA/gpu-operator/blob/v25.10.1/deployments/gpu-operator/
values.yaml.
Note:
To configure a different GPU Operator version or use a non-default driver version or image, use 
the CLI after you activate Private AI Services in the namespace.
Setting driver.version inside GPU Operator Overrides will not be accommodated in the
Private AI Services configuration.
Note:
Leave the location of and credentials for a local OCI image registry for custom images for 
NVIDIA GPU Operator empty. The GPU Operator components are already available in the 
Supervisor instance.
Environment Connectivity
Connectivity
Connected
•
Fill in YAML code in the GPU Operator 
Overrides text area if you want to configure 
the NVIDIA GPU Operator itself.
•
Enter $oathtoken as the user name and 
the API token for access to the NVIDIA NGC 
catalog.
Disconnected
Fill in YAML code in the GPU Operator Overrides
text area if you want to configure the NVIDIA GPU 
Operator itself.
Run Models on NVIDIA 
GPU Passthrough and 
potentially CPU
If you plan to run models only on GPU passthrough with Fixed DirectPath or Dynamic DirectPath, 
provide the following configuration:
1.
Select NVIDIA data center GPU driver.
2.
Fill in YAML code in the GPU Operator Overrides text area if you want to customize the NVIDIA 
GPU Operator itself.
Note:
To configure a different GPU Operator version or use a non-default driver version or image, use 
the CLI after you activate Private AI Services in the namespace.
Setting driver.version inside GPU Operator Overrides will not be accommodated in the
Private AI Services configuration.
Note:
Leave the location of and credentials for a local OCI image registry for custom images for 
NVIDIA GPU Operator empty. The GPU Operator components are already available in the 
Supervisor instance.
VMware by Broadcom
117


---

VMware Private AI Foundation with NVIDIA 9.1
8. On the Observability page, if necessary, activate metrics collection for observability platforms and LLM traces for 
tracing in an OpenTelemetry (OTel) Collector the execution flow of agents across components and knowledge base 
indexing, and click Next.
Metrics
Turn on the Metrics toggle and optionally set a storage policy for metrics retention and the time to retain 
metrics data.
When you turn on metrics collection, a Prometheus server is deployed in the namespace with its own 
metrics database. The database storage is backed by a Persistent Volume whose size is inferred from 
the metrics retention period.
LLM Traces
Turn on the LLM Tracing toggle and configure the following settings for sending traces to an 
OpenTelemetry Collector:
Setting
Description
Endpoint
HTPP URL of the collector endpoint
Protocol
grpc or http/protobuf
Project Name
Project for Private AI Services on the collector
Authorization Secret
Send HTTP headers for authorization to 
the OpenTelemetry Collector. You can 
enter new header key-value pairs, such as
Authorization=Bearer <access-tok 
en>; <X-Custom-Header>=<custom-he 
ader-value>, or reuse headers from a secret 
already available in the namespace.
9. Review the Private AI Services configuration and click Configure Private AI Services to start the activation in the 
namespace.
You are redirected to the Service Configuration page where you can monitor the activation progress. The status and 
status reason show the current stage of the activation process.
10. Locate the external IP address that is assigned to the Private AI Services instance and associate it with the FQDN on 
the DNS server.
a) Back in the VCF Automation organization UI or the vSphere Client, on the Service Configuration tab, locate
Private AI Services UI IP, and copy the IP address.
b) Add a DNS entry for the IP address on the DNS server.
If required by your organization, configure the following features by using kubectl.
– Map the FQDN assigned to Private AI Services to the IP address that is assigned .
– Turn on API tokens for local accounts.
– Set the Private AI Services issuer base URL to use the IP address assigned.
– Turn on TLS certificate verification for connected external OIDC providers.
– Use a different version of the NVIDIA GPU Operator or the GPU driver.
– Replace the self-signed certificate for the Private AI Services UI or API that is exposed to external clients.
11. Open the PAISConfiguration resource in the namespace in edit mode.
a) On the machine with VCF Consumption CLI and kubectl, create a context for the namespace where the instance 
is available by running the following command.
Use the --ca-certificate option if the VCF Automation or vSphere Supervisor certificate is not added to the 
truststore on your machine.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token  <vcf_automation_api_token> --ca-
certificate <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
VMware by Broadcom
118


---

VMware Private AI Foundation with NVIDIA 9.1
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --
ca-certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access 
to are created. An additional context is available as <context_name>:<namespace_name>.
b) Switch to the context for the target namespace.
 VCF Automation
vcf context use <vcf_automation_context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <supervisor_context_name>:<namespace_name>
c) Open the PAISConfiguration resource in the namespace in edit mode.
kubectl edit paisconfiguration default
12. To use an FQDN instead of your LoadBalancer external IP address when switching between the components of the
Private AI Services UI, add the paisconfigurations.pais.vmware.com/cname annotation with value the load 
balancer FQDN.
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  annotations: 
    pais.vmware.com/default-failure-domain: <failure-domain>  
    paisconfigurations.pais.vmware.com/cname: <load_balancer_external_fqdn> 
    ui.pais.vmware.com/pais-enablement-environment-connectivity: connected
13. Turn on API tokens for local accounts in the Private AI Services built-in OIDC provider.
a) Locate the spec.auth.providers.staticCredentials.sc section and turn on the API token feature by 
adding the following properties.
apiTokens: 
  expirationDefault: 720h 
  expirationMax: 8760h
As a result, the complete spec.auth.providers.staticCredentials.sc section contains the following 
configuration:
spec: 
  auth: 
    providers: 
    - name: sc 
      staticCredentials: 
        accessTokens: 
          ttl: 1h 
        apiTokens: 
          expirationDefault: 720h 
          expirationMax: 8760h 
        credentialsSecretRef: 
          key: credentials.json 
          name: pais-static-credentials-<secret_id>
        refreshTokens: 
          ttl: 720h
14. Turn on TLS certificate verification of the connection to the assigned external OIDC providers if required by your 
organization.
Locate the spec.auth.providers.oidc section for the relevant provider and add the following property:
insecureSkipTLSVerify: false
As a result, the complete spec.auth.providers.oidc provider-specific section contains the following 
configuration:
spec: 
VMware by Broadcom
119


---

VMware Private AI Foundation with NVIDIA 9.1
  auth: 
    providers: 
    - name: oidc 
      oidc: 
        authorizedGroups: 
        - pais-access 
        clientId: 2KGaR4HAEPTUFbyQ8nVlNpglE6q8tkFBaW5U0k3f 
        groupsClaim: groups 
        insecureSkipTLSVerify: false
15. If required by your organization, switch to a different GPU Operator version, supported by Private AI Services
according to the Private AI Services release notes, for example, to 26.3.1.
Locate the spec.nvidiaConfig section and add the following property:
gpuOperatorVersion: 26.3.1
As a result the spec.nvidiaConfig section contains the following configuration:
spec: 
  <rest of the PAISConfiguration resource specification> 
  nvidiaConfig: 
   gpuOperatorVersion: 26.3.1 
  <rest of the nvidiaConfig specification>
16. If required by your organization, set a driver version for vGPU or GPU Passthrough that is different from the default 
one for the GPU Operator version.
NVIDIA vGPU
If you want to use a vGPU driver version that is different from the default one, 580.105.08, in 
the spec.nvidiaConfig.driver.image property provide the location of the driver 
container image in the NVIDIA NGC registry or in your private container registry and in the
spec.nvidiaConfig.imagePullSecretRef property - a Secret with the credentials for this 
registry. For example, if you want to use a driver version which contains a fix from NVIDIA.
spec: 
  <rest of the PAISConfiguration resource specification> 
  nvidiaConfig: 
    imagePullSecretRef: 
      name: my-registry-pull-secret 
    driver: 
      image: my-registry.example.com/nvidia/custom-driver:580.105.08-ubuntu24.04 
  <rest of the nvidiaConfig specification
NVIDIA GPU 
Passthrough
If you want to use a data center GPU driver version that is different from the default one, 580.105.08, 
you have two options:
•
Use a validated driver version that is packaged within the NVIDIA GPU Operator 
package for Private AI Services. In this case, you set the driver version in the
spec.nvidiaConfig.driver.version property.
You can switch to driver version 580.126.20.
•
Use a non-validated for Private AI Services driver version that is available in the NVIDIA NGC 
registry or in another location. For example, if you built your driver from a custom source. In this 
case, set the following properties:
– spec.nvidiaConfig.driver.image. Provide the location of the driver container image 
in your container registry
– spec.nvidiaConfig.imagePullSecretRef . Provide a Secret with the credentials for 
this registry.
spec: 
  <rest of the PAISConfiguration resource specification> 
  nvidiaConfig: 
    imagePullSecretRef: 
      name: my-registry-pull-secret 
    driver: 
VMware by Broadcom
120


---

VMware Private AI Foundation with NVIDIA 9.1
      image: my-registry.example.com/nvidia/custom-driver:580.105.08-ubuntu24.04
For information about other driver versions that are compatible with an NVIDIA GPU Operator version, see the NVIDIA 
GPU Operator documentation, for example, https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/25.10/
platform-support.html#gpu-operator-component-matrix.
17. Save the PAISConfiguration changes in the editor.
18. If your organization requires Private AI Services to present a certificate issued by your own CA, for example to satisfy 
internal PKI policies or to share a wildcard certificate across services, add a Secret with the certificate details.
By default, Private AI Services assigns its ingress TLS certificate automatically.
a) Use the VCF Consumption CLI to switch to the namespace of the Private AI Services instance.
b) Create a TLS Secret from your own CA-signed certificate and private key.
The tls.crt file contains the full certificate chain - server certificate and intermediate CA certificates, so that 
clients can build the trust path to your CA.
kubectl create secret tls my-pais-tls \ 
  --cert=<path-to-tls.crt> \ 
  --key=<path-to-tls.key> \
c) Update the PAISConfiguration resource.
kubectl patch paisconfiguration default --type=merge \ 
  -p '{"spec":{"ingress":{"tlsSecretRef":{"name":"my-pais-tls"}}}}'
d) Connect to the Private AI Services ingress service by using an HTTPS client that trusts your CA to verify that the 
configuration is saved.
curl --cacert <path-to-tls.crt> https://<pais-ingress-ip>/api/
19. If you have not included some of them during Private AI Services activation, add the issuer certificate of the following 
components as CA trusted bundles.
•
VCF Automation if TLS verification mode is turned on
•
OIDC provider
•
Database for Data Indexing and Retrieval
See Import a CA Trust Bundle in Private AI Services.
•
If observability is activated in Private AI Services, MLOps engineers can visualize the Private AI Services metrics 
in Grafana. For information about available metrics and setting up metrics dashboards, see Visualizing Private AI 
Services Metrics in Observability Platforms.
•
If LLM traces collection is activated, MLOps engineers can analyze traces from agents and knowledge base indexing 
in the OpenTelemtry Collector. For information about available traces, see Tracing Agent Operation and Knowledge 
Base Indexing.
•
If you want to change the Private AI Services configuration later, for example, to extend Private AI Services to its full 
set of capabilities, perform the following steps:
– In the VCF Automation UI, select Private AI in the namespace in your VCF Automation organization and click
Service Configuration.
– In the Supervisor Management UI in the vSphere Client, on the Resources tab for the namespace, go to the
Private AI service and click the Service Configuration tab.
•
In the Supervisor Management UI in the vSphere Client, provide users and groups with access to the Private AI 
Services capabilities in the namespace by assigning them the Namespaces Edit role and share the namespace UI 
URL with your consumers available under Launch Standalone Client > Copy link to clipboard. For information 
about configuring a namespace in the vSphere Supervisor, see Create and Configure a vSphere Namespace on the 
Supervisor .
Activate Private AI Services 3.0 in an Organization Namespace by Using the kubectl Command
As an organization administrator, after you configure a GPU-accelerated namespace in an organization in VCF 
Automation, by using the kubectl command utility in this namespace, you can activate Private AI Services for 
VMware by Broadcom
121


---

VMware Private AI Foundation with NVIDIA 9.1
provisioning generative AI applications including model endpoints and agents communicating to the endpoints, and for 
RAG applications, knowledge bases.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for
Private AI Services. See Setup Workflows for Private AI Services.
•
Verify that you have a machine with installed VCF Consumption CLI and kubectl. See Getting Started with the VCF 
Consumption CLI.
•
If you plan to use knowledge bases in Data Indexing and Retrieval , MCP tools, or agents in Agent Builder, create a 
PostgreSQL database with pgvector extension or get the connection details to an available one.
– If you are using VMware Data Services Manager, create the database in VCF Automation. See Create a Database 
Using VMware Data Services Manager in VMware Cloud Foundation Automation documentation. The pgvector 
extension is installed automatically.
– You can also create a PostgreSQL database with pgvector extension using third-party software. The PostgreSQL 
database must have the pgvector extension installed.
Private AI Services activates the installed pg vector extension during its activation in the namespace.
•
Get the CA certificate of the PostgreSQL database if it is self-signed or the issuer's certificate is not publicly available.
•
Verify that an Ingress controller with LoadBalancer support is available in the namespace.
•
Verify that the namespace includes a VM class and storage policy as a storage class for the control plane of the VKS 
cluster running model endpoints. The control plane nodes do not require GPU resources.
•
If you are using VCF Automation, verify that you have the certificate of your VCF Automation instance saved to your 
machine, for example, as a vcf_automation_certificate.pem file in your current working folder.
•
If you plan to activate Private AI Services on a vSphere namespace directly on vSphere Supervisor, verify 
that you have the issuer certificate of the Supervisor instance saved to your machine, for example, as a
supervisor_ca_certificate.pem file in your current working folder.
•
If you are activating Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces 
Edit role in the namespace.
•
If required by your organization, verify that an FQDN is allocated for the Private AI Services instance.
You map it to an IP address after the Private AI Services instance is active.
•
If you plan to run models on NVIDIA vGPUs, ask the VI administrator about the version of the NVIDIA vGPU host 
driver on the hosts in the GPU-accelerated cluster for your organization.
Private AI Services uses a default version of the NVIDIA vGPU guest driver for its model endpoint VKS worker nodes. 
If the version of the vGPU host driver is not compatible with this default version, then you must include an additional 
override config snippet in the YAML file for Private AI Services activation.
As a VI administrator, to view the version of the vGPU host driver, follow these steps:
a. Go to the vCenter inventory in the vSphere Client and select a host.
b. On the Configure tab, click System > Packages.
c. Note the version of the NVIDIA AI Enterprise vGPU driver for VMware ESX entry.
•
If you plan to use an OIDC provider for managed access to Private AI Services, verify that a third-party OIDC provider 
or VCF Automation is configured according to the Identity and Access Management Requirements for Private AI 
Services.
– For an external OIDC provider, ask your OIDC provider administrator for the following details:
•
Required. Issuer URL.
•
Required. Client ID for the Private AI Services application.
•
Required. The list of groups with Private AI Services users. If groups are not used, you can provide an empty 
group list in the Private AI Services activation YAML code.
•
Name of the scope for the Private AI Services groups if the name is different from groups.
•
Name of the claim with the Private AI Services groups if the name is different from groups.
•
Required. Issuer certificate of the OIDC provider.
– For VCF Automation, ask your VI administrator for the following details:
•
Required. Client ID for the Private AI Services application. You can retrieve it from the dedicated OIDC provider 
proxy server in VCF Automation Provider Management.
•
Required. Issuer certificate of your VCF Automation instance.
For example, on the machine that you use to access your environment, run the following OpenSSL command.
VMware by Broadcom
122


---

VMware Private AI Foundation with NVIDIA 9.1
openssl s_client -connect <vcf_automation_fqdn>:443 -showcerts </dev/null 2>/dev/null | awk 
 'BEGIN{c=0;p=0} /-----BEGIN CERTIFICATE-----/{c++;if(c>1)p=1} p{print} /-----END CERTIFICATE-----/
{p=0}'
•
For sharing models with other organizations, the names of these organizations and global roles or LDAP groups 
in them for access to Private AI Services.
•
When you log in to In an environment with VCF Automation, the organization in the VCF Automation UI and switch to 
the GPU-accelerated namespace, under Services, you see the Private AI entry. However, you cannot create model 
endpoints, required for provisioning generative AI applications.
Similarly, if you use the Local Consumption Interface in the vSphere Client, when you switch to the namespace for the 
Supervisor in the Supervisor Management section of the vSphere Client, on the Resources tab, you must configure 
the Private AI entry before you can create items in Private AI Services.
•
If you want to activate Private AI Services in another namespace by using kubectl, you can download the 
configuration YAML from the right-hand side panel of the wizard in either the VCF Automation UI or Local Consumption 
Interface.
1. If you are using VCF Automation, get an API token for access to the namespace where you are deploying a Private AI 
Services configuration.
a) Log in to your organization in VCF Automation as an organization administrator.
b) Click your account in the top-right corner and select User Settings > My Account.
c) On the API Tokens tab, create a token and save it on your machine.
2. Log in to a machine running the VCF Consumption CLI and kubectl.
3. Create a context for the namespace where the Private AI Services instance must be available in VCF Automation or 
directly on vSphere Supervisor.
Use the --ca-certificate option if the VCF Automation or vSphere Supervisor certificate is not added to the 
truststore on your machine.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token  <vcf_automation_api_token> --ca-certificate 
 <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --ca-
certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access to 
are created. An additional context is available as <context_name>:<namespace_name>.
4. Switch to the context for the target namespace.
 VCF Automation
vcf context use <context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <context_name>:<namespace_name>
5. If you are running Private AI Services on vGPU, prepare a configuration map for the NVIDIA license and a secret for 
the NVIDIA NGC API token in YAML format.
For example, nvidia_license_config_map.yaml and nvidia_ngc_access_token_secret.yaml.
apiVersion: v1
kind: ConfigMap
metadata: 
  name: NVIDIA_license_config_map
data: 
  client_configuration_token.tok: license-server-access-token 
  gridd.conf: ""
apiVersion: v1
VMware by Broadcom
123


---

VMware Private AI Foundation with NVIDIA 9.1
kind: Secret
metadata: 
  name: ngc_access_token_secret
type: kubernetes.io/dockerconfigjson
data: 
    .dockerconfigjson: base-64-encoded-auths-for-nvcr.io 
base-64-encoded-auths-for-nvcr.io represents the Base64 value of the following YAML code. There, the
auth property in its turn contains the $oauthtoken:<ngc-access-key> pair in the Base64 format.
{
"auths": { 
 "nvcr.io/nvidia/vgpu": { 
   "username": "$oauthtoken", 
   "password": "<ngc-access-key>", 
   "auth": "base-64-encoded-$oauthtoken:<ngc-access-key>" 
   } 
  }
}
6. Prepare configurations in YAML format for the following trust bundles for secure HTTPS connection.
•
Your OIDC provider
•
Your Harbor registry
•
For Data Indexing and Retrieval or Agent Builder
– Your PostgreSQL database
– Content management systems hosting data for your knowledge databases code
– MCP servers providing tools to models for access to specialized data and capabilities
A ca-trust-bundle-for-harbor.yaml example file might contain the following details:
apiVersion: v1
kind: ConfigMap
metadata: 
  name: ca-trust-bundle-for-harbor
data: 
  ca.crt: | 
    -----BEGIN CERTIFICATE----- 
    <Harbor-registry-CA-certificate-in-PEM-format> 
    -----END CERTIFICATE-----
7. If you are using local accounts for authentication, prepare a JSON file with account credentials.
You save this JSON file as a Secret in the namespace before activating Private AI Services.
For example, the following JSON file, called pais-static-credentials.json, contains credentials for pais-
user-01 and pais-user-02.
[ 
  { 
    "userID": "pais-user-01-id", 
    "username": "pais-user-01", 
    "hash": "pais-user-01_password_in_SHA-512", 
    "email": "pais-user-01@pais.local" 
  }, 
  { 
    "userID": "pais-user-02-id", 
    "username": "pais-user-02", 
    "hash": "pais-user-02_password_in_SHA-512", 
    "email": "pais-user-02@pais.local" 
  }
VMware by Broadcom
124


---

VMware Private AI Foundation with NVIDIA 9.1
]
8. Prepare a YAML file, for example, called paisconfiguration.yaml, in compliance with the PAISConfiguration 
CRD.
You configure Private AI Services by using the following sections in the YAML file:
Configuration Area
YAML Section
ConfigMap resources for the CA trust bundles
spec.clientTls.caBundleRefs
Settings for integration with one or more OIDC providers, such as OIDC 
providers, VCF Automation, or local accounts.
Important:
You must configure a custom OIDC provider as an oidcCustom
entry only in coordination with Broadcom Technical Support.
spec.auth.providers
Required ingress service type
spec.ingress.serviceType
VM classes and Kubernetes storage class for the control plane nodes of the 
VKS cluster for running model endpoints
spec.vksControlPlane
Default storage class for the management components of Private AI Services
You can override the storage class for the VKS control plane.
spec.defaultStorageClassName
NVIDIA GPU Operator version and GPU guest driver configuration for running 
model endpoints according to the GPU consumption model
spec.nvidiaConfig
Settings for turning on metrics collection and LLM traces.
spec.observability
Settings for connecting to the assigned PostgreSQL database with pgvector 
extension
spec.database
Storage policy for Data Indexing and Retrieval worker nodes
spec.worker.storageClassName
Settings for skipping deployment of Data Indexing and Retrieval and Agent 
Builder, and configuring telemetry collection, logging verbosity, and operational 
behavior.
Important:
You must configure logging verbosity, and operational behavior in this 
section in coordination with Broadcom Technical Support.
spec.runtimeConfig
You can run the following command to see the properties you must include in the YAML file.
kubectl explain paisconfiguration
Note:
You can still run CPU-only models using llama.cpp inside namespaces configured to support NVIDIA vGPUs or 
NVIDIA GPU Passthrough to save GPU resources for other tasks.
a) If you plan to run Private AI Services on NVIDIA vGPUs and potentially on CPU, use the one of following YAML 
templates according to your NVIDIA vGPU host driver configuration.
You can deploy a Private AI Services instance with Model Runtime only capabilities, excluding Agent Builder 
and Data Indexing and Retrieval from the Private AI Services capabilities by using the properties in the
spec.runtimeConfig section.
All Component 
Configuration
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  # Setting "name" to "default" is mandatory 
  name: default 
  # Remove the comments in the annotations section 
  # to use an FQDN instead of your LoadBalancer external IP address when 
 switching 
VMware by Broadcom
125


---

VMware Private AI Foundation with NVIDIA 9.1
  # between the components of the Private AI Services UI 
  # annotations: 
  #   paisconfigurations.pais.vmware.com/cname: <load_balancer_external_fqdn>
spec: 
  defaultStorageClassName: default_storage_policy_components 
  worker: 
    storageClassName: 
 <storage_policy_for_Data_Indexing_and_Retrieval_worker_nodes> 
  clientTls: 
    # Add references to the trust bundles. It is a good practice 
    # to break the bundles per application. You can also consolidate trust 
 information 
    # in fewer bundles according to your authentication setup.   
    # Make sure that you update a trust bundle if the associated certificate 
    # is about to expire. 
    caBundleRefs: 
      - name: ca-trust-bundle-for-oidc-provider 
      - name: ca-trust-bundle-for-harbor 
      - name: ca-trust-bundle-for-dsm 
      # - name: ca-trust-bundle-for-another-application 
      # To reference a Secret instead of a ConfigMap, set kind to Secret. 
      # - name: ca-trust-bundle-in-a-secret 
      #   kind: Secret 
  database: 
    host: pgvector_server_host_name 
    username: pgvector_db_admin_name 
    dbname: pgvector_db_name 
    passwordRef: 
      name: pgvector_credentials_secret 
      fieldPath: field_with_pgvector_db_admin_password 
    # port: 5432 
    # sslMode: VerifyFull 
  # Use oidcCustom with assistance from Broadcom Technical Support. 
  auth: 
    providers: 
    - name: oidc 
      displayName: OIDC Provider 
      oidc: 
        issuerUrl: OIDC_discovery_endpoint_URL 
        scope: 
        - openid 
        - profile 
        - groups 
        # Issuing refresh tokens is a requirement for the Private AI Services UI. 
        # Many OIDC servers require the "offline_access" scope for issuing 
        # refresh tokens but not all servers support this scope. 
        # Check in the documentation of your OIDC server  
        # if this scope is supported and required.         
        - offline_access 
        clientId: pais_authentication_id 
        groupsClaim: groups 
        # If no groups are required,  
        # provide an empty authorizedGroups array. 
        # authorizedGroups: [] 
        # In this example, user accounts that are members of the 
        # group-for-pais-access-01 or group-for-pais-access-02 group  
        # can access Private AI Services.         
VMware by Broadcom
126


---

VMware Private AI Foundation with NVIDIA 9.1
        authorizedGroups: 
        - group-for-pais-access-01 
        - group-for-pais-access-02 
        insecureSkipTLSVerify: false 
        # To accept access tokens issued by additional OAuth2.0 clients 
        # or additional OIDC issuers, uncomment and set the following fields. 
        # extraAudiences: 
        # - additional-oauth2-client-id 
        # extraIssuers: 
        # - https://additional-oidc-issuer.example.com 
    # Use this option for model sharing. 
    - name: vcfa 
      displayName: VCF Automation 
      vcfa: 
        issuerUrl: https://vcf-automation-fqdn/oidc 
        clientId: client_id_of_vcf_automation_relying_party 
        # In this example, user accounts that are members of the  
        # consumer-org-01 or consumer-org-02 organization  
        # can access Private AI Services. 
        allowedOrgs: 
        - consumer-org-01 
        - consumer-org-02 
        authorizedRoles: 
        - Private AI Service Access 
        # authorizedGroups: 
        # - vcf-automation-group-for-pais-access 
        insecureSkipTLSVerify: false 
        # scope: 
        # - openid 
        # - profile 
        # - groups 
        # To hot-reload the list of allowed orgs without restarting the 
        # auth service, reference a ConfigMap key instead of using 
        # allowedOrgs. allowedOrgs and orgConfigRef are mutually exclusive. 
        # orgConfigRef: 
        #   name: vcfa-allowed-orgs 
        #   key: orgs 
    # Use staticCredentials for proof-of-concept.  
    # You can use this option to explore model sharing. 
    - name: sc 
      staticCredentials: 
        # If you want to use the IP address assigned  
        # by the Ingress controller at  
        # Private AI Services activation, 
        # do not include this property. 
        issuerBaseUrl: https://private-ai-services-fqdn 
        # Provide a Secret with user ID, name, and password in SHA-512. 
        credentialsSecretRef: 
          name: pais-static-credentials 
          key: credentials.json 
        # To set a ttl value different from the default ones,  
        # update the relevant default value. 
        refreshTokens: 
          ttl: 720h 
        accessTokens: 
          ttl: 1h 
        # To turn on API token generation for local accounts, 
VMware by Broadcom
127


---

VMware Private AI Foundation with NVIDIA 9.1
        # the apiTokens section must be present.  
        # You can set an empty value for expirationDefault and expirationMax. 
        apiTokens: 
        #   expirationDefault: 720h 
        #   expirationMax: 8760h   
  ingress: 
    serviceType: LoadBalancer 
    # To use your own TLS certificate for TLS termination instead of the 
    # cert-manager-managed certificate, uncomment and set tlsSecretRef. 
    # The Secret must be of type kubernetes.io/tls and contain 
    # tls.crt (PEM certificate chain) and tls.key (PEM private key). 
    # tlsSecretRef: 
    #   name: pais-ingress-tls-secret 
  # Private AI Services deploys a vSphere Kubernetes Service (VKS) cluster  
  # to run models. Initially, the VKS cluster has only control plane nodes,  
  # used for managing model endpoints.  
  # Select the VM class and storage class  
  # which represent the resources  
  # required to run the control plane of this VKS cluster. 
  vksControlPlane: 
    virtualMachineClassName: vm_class_VKS_control_plane_node 
    storageClassName: storage_policy_VKS_control_plane_node 
    # To associate the VKS cluster to a specific Kubernetes release version, 
    # uncomment and set upgradeStrategy.manual.version. 
    # Changing this field causes temporary downtime for ModelEndpoints 
    # with only 1 replica. Test in a non-production environment and plan 
    # for an outage window. 
    # upgradeStrategy: 
    #   manual: 
    #     version: v1.32.0+vmware.6-fips-vkr.2 
  nvidiaConfig: 
    licenseConfigRef: 
      name: NVIDIA_license_config_map 
    imagePullSecretRef: 
      name: ngc_access_token_secret 
  # Remove the comments to activate observability. 
  # observability: 
    # Use this section for activating metrics collection in Prometheus. 
    # prometheusRuntime: 
      # storageClassName: storage_policy_metrics 
      # metricsRetention: 30d 
    # Use this section for sending LLM traces to  
    # an OpenTelemetry Collector.       
    # llmTraces: 
      # endpoint: "<otlp-http-endpoint>" 
      # You can send traces over "http/protobuf" or "grpc".       
      # protocol: "http/protobuf" 
      # projectName: "<pais-project-in-otel-collector>" 
      # headersSecretRef: 
        # name: <otel-collector-auth-secret> 
        # key: headers 
  # Remove the comments to configure telemetry data collection. 
  # runtimeConfig: 
    # telemetryRuntimeConfig: 
      # Set to true to stop telemetry collection. 
      # disabled: true 
      # Schedule for the telemetry CronJob 
VMware by Broadcom
128


---

VMware Private AI Foundation with NVIDIA 9.1
      # (e.g. "*/1 * * * *" for every minute).  
      # By default, collection takes place 
      # at a random time every Sunday. 
      # Remove the comment and change the value to set a maximum time in seconds 
 for 
      # telemetry collection (all collectors).  
        # collectionTimeoutSeconds: 60 
        # Remove the comment and change the value to set a maximum time in 
 seconds for uploading 
        # telemetry to SuperCollider that is different from 35 seconds.  
        # uploadTimeoutSeconds: 35 
        # Remove the comment and change the value to set a different URL for the 
 SuperCollider telemetry 
        # collector.  
        # supercolliderUrl: https://supercollider.example.com
# Secret for authenticating to the OpenTelemetry Collector
# to send LLM traces to.
#---
# apiVersion: v1
# kind: Secret
# metadata:
#   name: <otel-collector-auth-secret>
#   # Semi-colon separated list of headers
# stringData:  
#   headers: Authorization=Bearer <access-token>; <X-Custom-Header>=<custom-
header-value>
 
Model Runtime Only 
Configuration
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  # Setting "name" to "default" is mandatory 
  name: default 
  # Remove the comments in the annotations section 
  # to use an FQDN instead of your LoadBalancer external IP address when 
 switching 
  # between the components of the Private AI Services UI 
  # annotations: 
  #   paisconfigurations.pais.vmware.com/cname: <load_balancer_external_fqdn>
spec: 
  runtimeConfig: 
    # Note: If disableAgentBuilder or disableIndexing is set to false, 
    # the "database" block becomes required. 
    # If you want to use MCP tools in agents, disableAgentBuilder 
    # must be false. 
    disableAgentBuilder: true 
    disableIndexing: true 
  # The rest of the example is identical to the case where all 
  # PAIS components are enabled 
  # Required default storage class for PAIS components 
  defaultStorageClassName: <default_storage_policy> 
  worker: 
    storageClassName: 
 <storage_policy_for_Data_Indexing_and_Retrieval_worker_nodes> 
  <rest of the PAISConfiguration resource manifest>
VMware by Broadcom
129


---

VMware Private AI Foundation with NVIDIA 9.1
If you want to use a NVIDIA GPU Operator version that is different from the default one 25.10.1, such as 26.3.1, 
include the spec.nvidiaConfig.gpuOperatorVersion property.
spec: 
  <rest of the PAISConfiguration resource specification> 
  nvidiaConfig: 
   gpuOperatorVersion: 26.3.1
If you want to use a non-default vGPU driver version, other than 580.105.08, configure the following properties. For 
example, if you want to use a driver version which contains a fix from NVIDIA.
•
spec.nvidiaConfig.driver.image. Set to the location of the driver container image in the NVIDIA NGC 
registry or in your private container registry
•
spec.nvidiaConfig.imagePullSecretRef. Specify a Secret with the credentials for the registry 
containing the driver image.
spec: 
  <rest of the PAISConfiguration resource specification> 
  nvidiaConfig: 
    imagePullSecretRef: 
      name: my-registry-pull-secret 
    driver: 
      image: my-registry.example.com/nvidia/custom-driver:580.105.08-ubuntu24.04
For information about other driver versions that are compatible with an NVIDIA GPU Operator version, see 
the NVIDIA GPU Operator documentation, for example, https://docs.nvidia.com/datacenter/cloud-native/gpu-
operator/25.10/platform-support.html#gpu-operator-component-matrix.
If you want to configure the GPU Operator and other driver properties, such as usePrecompiled, you can add 
a spec.nvidiaConfig.gpuOperatorOverridesRef ConfigMap. See https://docs.nvidia.com/datacenter/
cloud-native/gpu-operator/latest/getting-started.html#common-chart-customization-options. For a GPU Operator 
Overrides template, see https://github.com/NVIDIA/gpu-operator/blob/v<operator-version>/
deployments/gpu-operator/values.yaml for the version supported by your Private AI Services version 
according to the Private AI Services release notes. For example: https://github.com/NVIDIA/gpu-operator/blob/
v25.10.1/deployments/gpu-operator/values.yaml.
Note:
Setting driver.version inside the spec.nvidiaConfig.gpuOperatorOverridesRef ConfigMap will not 
be accommodated in the Private AI Services configuration.
b) If you plan to run model endpoints on NVIDIA GPU devices in passthrough mode and potentially on CPU, modify 
the spec.nvidiaConfig section in the template from the previous substep as follows.
Private AI Services maps GPU resources to models by using an NVIDIA data center driver.
Private AI Services pulls the driver from the public NVIDIA repository.
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  name: default 
  # Remove the comments in the annotations section 
  # to use an FQDN instead of your LoadBalancer external IP address when switching 
  # between the components of the Private AI Services UI 
  # annotations: 
    # paisconfigurations.pais.vmware.com/cname:<load_balancer_external_fqdn>
spec: 
  <rest of the PAISConfiguration resource specification> 
  nvidiaConfig: 
    gpuDriverType: OSS   
VMware by Broadcom
130


---

VMware Private AI Foundation with NVIDIA 9.1
If you want to use a data center GPU driver version that is different from the default one for the GPU Operator 
version, you have two options:
•
Use a validated driver version that is packaged within the NVIDIA GPU Operator package for Private AI 
Services. In this case, you set the driver version in the spec.nvidiaConfig.driver.version property.
You can switch to driver version 580.126.20.
•
Use a non-default driver version available in the NVIDIA NGC registry or in another location. For example, 
if you built your driver from a custom source. In this case, in the spec.nvidiaConfig.driver.image
property, provide the location of the driver image in your container registry and in the
spec.nvidiaConfig.imagePullSecretRef property - a Secret with the credentials for this registry.
For information about other driver versions that are compatible with an NVIDIA GPU Operator version, see 
the NVIDIA GPU Operator documentation, for example, https://docs.nvidia.com/datacenter/cloud-native/gpu-
operator/25.10/platform-support.html#gpu-operator-component-matrix.
If you want to configure the GPU Operator nd other driver properties, you can add a
spec.nvidiaConfig.gpuOperatorOverridesRef ConfigMap. See https://docs.nvidia.com/datacenter/
cloud-native/gpu-operator/latest/getting-started.html#common-chart-customization-options. For a GPU Operator 
Overrides template, see https://github.com/NVIDIA/gpu-operator/blob/v<operator-version>/
deployments/gpu-operator/values.yaml for the version supported by your Private AI Services version 
according to the Private AI Services release notes. For example: https://github.com/NVIDIA/gpu-operator/blob/
v25.10.1/deployments/gpu-operator/values.yaml.
Note:
Do not try setting driver.version inside the spec.nvidiaConfig.gpuOperatorOverridesRef
ConfigMap.
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  name: default 
  # Remove the comments in the annotations section 
  # to use an FQDN instead of your LoadBalancer external IP address when switching 
  # between the components of the Private AI Services UI 
  # annotations: 
    # paisconfigurations.pais.vmware.com/cname:<load_balancer_external_fqdn>
spec: 
  <the rest of the specification part> 
  nvidiaConfig: 
    gpuDriverType: OSS   
  imagePullSecretRef: 
    name: my-registry-pull-secret 
  driver: 
    image: my-registry.example.com/nvidia/custom-driver:580.105.08-ubuntu24.04
c) If you plan to run model endpoints on CPU resources, you can use one of the YAML templates in the first substep 
leaving the spec.nvidiaConfig section out.
Models are run on llama.cpp.
9. To use an FQDN instead of your LoadBalancer external IP address when switching between the components of the
Private AI Services UI, remove the comment from and update the paisconfigurations.pais.vmware.com/
cname section of paisconfiguration.yaml in the previous step.
The Model Runtime UI uses the FQDN, specified in the annotation, in the external links to the Agent Builder and to the 
Data Indexing and Retrieval UIs, instead of the default behavior of using the LoadBalancer external IP address.
10. If your organization requires Private AI Services to present a certificate issued by your own CA, for example to satisfy 
internal PKI policies or to share a wildcard certificate across services, add a Secret with the certificate details.
By default, Private AI Services assigns its ingress TLS certificate automatically.
VMware by Broadcom
131


---

VMware Private AI Foundation with NVIDIA 9.1
a) Create a TLS Secret from your own CA-signed certificate and private key in the namespace of the Private AI 
Services instance. The tls.crt file contains the full certificate chain - server certificate and intermediate CA 
certificates, so that clients can build the trust path to your CA.
kubectl create secret tls pais-ingress-tls-secret \ 
  --cert=<path-to-tls.crt> \ 
  --key=<path-to-tls.key> \
b) Remove the comments from spec.ingress.tlsSecretRef section of paisconfiguration.yaml and 
provide the Secret name.
11. If you want to activate metrics collection for observability, uncomment the
spec.observability.prometheusRuntime section of paisconfiguration.yaml.
A Prometheus server is deployed in the namespace as a part of Private AI Services with its own metrics database. 
The database storage is backed by a Persistent Volume whose size is inferred from the metrics retention property in 
the PAISConfiguration CR. For information about provided metrics and setting up metrics dashboards, see Visualizing 
Private AI Services Metrics in Observability Platforms.
12. If you want to activate LLM traces collection in a OpenTelemetry Collector, uncomment the
spec.observability.llmTraces section of paisconfiguration.yaml.
13. If you want to configure collection of telemetry data for the customer experience improvement program, uncomment 
the spec.runtimeConfig.telemetryRuntimeConfig section of paisconfiguration.yaml.
By default, Private AI Services is configured to send telemetry data to Broadcom.
14. To configure Private AI Services with a custom OIDC provider by using the spec.auth.providers.oidcCustom
section of the PAISConfiguration CR file, contact Broadcom Technical Support.
15. Configure the following areas of spec.runtimeConfig only in coordination with Broadcom Technical Support.
•
apiRuntimeConfig
•
ingressRuntimeConfig
•
indexingMCPServerRuntimeConfig
•
indexingWorkersRuntimeConfig
•
logVerbosity
16. Save the configurations for the trust bundles, and if you are running Private AI Services on vGPU, for the NVIDIA 
license and NVIDIA NGC secret, by running the following command for each entry.
kubectl apply -f <dependency_yaml_file>
17. If you are using local accounts for authentication to Private AI Services, create the Secret with the credentials in the 
namespace.
kubectl create secret generic pais-static-credentials \ 
        --namespace pais-tenant1 \ 
        --from-file=credentials.json=pais-static-credentials.json \ 
        --dry-run=client -o yaml | kubectl apply -f -
18. Activate Private AI Services in the namespace by running the following command.
kubectl apply -f paisconfiguration.yaml
19. To monitor the progress of Private AI Services's components deployment, run the following commands.
•
kubectl get pods
In the output, verify that the API, ingress, and rex worker pods for indexing knowledge bases if Data Indexing and 
Retrieval is turned on, and the Prometheus server pod if observability is turned on, are ready and running.
•
kubectl get paisconfiguration
In the output, verify that the Ready property for the default PAISConfiguration is paisAvailable. For example:
kubectl get paisconfiguration
NAME      SERVICE                READY   REASON
VMware by Broadcom
132


---

VMware Private AI Foundation with NVIDIA 9.1
default   pais-ingress-default   True    paisAvailable 
•
kubectl describe paisconfiguration
In the output, locate the Status.Conditions set of properties and verify that Status is True, Type is Ready, 
and Reason is paisAvailable. For example:
  Conditions: 
    Last Transition Time:  <current-date> 
    Message:               all components are available 
    Observed Generation:   2 
    Reason:                paisAvailable 
    Status:                True 
    Type:                  Ready
20. Locate the IP address that is assigned to the Private AI Services instance by the ingress service and associate it with 
the FQDN on the DNS server.
Using kubectl
Run the following command.
kubectl get svc pais-ingress-default
In the output, locate the EXTERNAL-IP property.
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)         
 AGE
pais-ingress-default   LoadBalancer   x.x.x.x         x.x.x.x          443:30591/TCP 
      10d
Using Private AI 
Services UI
1.
Back in the VCF Automation organization UI, perform the following steps:
a.
On the Build & Deploy page, locate the namespace where you activated Private AI Services.
b.
Click Services > Private AI and on the Service Configuration page, locate Private AI 
Services UI IP and copy the IP address.
2.
In the vSphere Client, perform the following steps:
a.
In the side panel, click Supervisor Management and, on the Namespaces tab, select the 
namespace where you activated Private AI Services.
b.
On the Resources tab, click Private AI and, on the Private AI Services page, click Service 
Configuration.
c.
Locate Private AI Services UI IP and copy the IP address.
3.
Add a DNS entry for the IP address on the DNS server.
•
MLOps engineers and data scientists can provision generative AI applications for application developers on top of 
specific models and data sources. See Delivering Generative AI Applications by Using Private AI Services.
•
If observability is activated in Private AI Services, MLOps engineers can visualize the Private AI Services metrics 
in Grafana. For information about provided metrics and setting up metrics dashboards, see Visualizing Private AI 
Services Metrics in Observability Platforms.
•
If LLM traces collection is activated, MLOps engineers can analyze traces from agents and knowledge base indexing 
in the OpenTelemtry Collector.
•
If required by your organization, to change the Private AI Services configuration in the namespace, edit the 
PAISConfiguration resource.
Activating Private AI Services 2.1
Activate Private AI Services 2.1 in an Organization Namespace by Using the VCF Automation 
UI
As an organization administrator, after you configure a namespace in an organization in VCF Automation, you can activate
Private AI Services in this namespace for provisioning generative AI applications including model endpoints and agents 
communicating to the endpoints, and for RAG applications, knowledge bases.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for
Private AI Services. See Setup Workflows for Private AI Services.
•
Create a PostgreSQL database with pgvector extension or get the connection details to an available one.
VMware by Broadcom
133


---

VMware Private AI Foundation with NVIDIA 9.1
– If you are using VMware Data Services Manager, create the database in VCF Automation. See Create a Database 
Using VMware Data Services Manager in VMware Cloud Foundation Automation documentation. The pgvector 
extension is installed automatically.
– You can also create a PostgreSQL database with pgvector extension using third-party software. The PostgreSQL 
database must have the pgvector extension installed.
Private AI Services activates the installed pgvector extension during its activation in the namespace.
•
Get the CA certificate of the PostgreSQL database.
•
Verify with your VI administrator that an Ingress controller with LoadBalancer support is available in the namespace.
•
Verify that the namespace includes a VM class and storage policy as a storage class for the control plane of the VKS 
cluster running model endpoints. The control plane nodes do not require GPU resources.
•
Verify that you have the certificate of your VCF Automation instance saved to your machine, for example, as a
vcf_automation_certificate.pem file in your current working folder.
•
Verify that an FQDN is allocated for the Private AI Services instance.
You map it to an IP address after the Private AI Services instance is active.
•
If you plan to run models on NVIDIA vGPUs, ask the VI administrator about the version of the NVIDIA vGPU host 
driver on the hosts in the GPU-accelerated cluster for your organization.
Private AI Services uses a default version of the NVIDIA vGPU guest driver for its model endpoint VKS worker nodes. 
If the version of the vGPU host driver is not compatible with this default version, then you must include an additional 
override config snippet in the YAML file for Private AI Services activation.
As a VI administrator, to view the version of the vGPU host driver, follow these steps:
a. Go to the vCenter inventory in the vSphere Client and select a host.
b. On the Configure tab, click System > Packages.
c. Note the version of the NVIDIA AI Enterprise vGPU driver for VMware ESX entry.
•
Set up an OIDC provider, VCF Automation, or local accounts for managed access to Private AI Services.
For information about the required OIDC provider configuration, see Identity and Access Management Requirements 
for Private AI Services.
•
Ask your OIDC provider administrator for the following details:
– Required. Issuer URL.
– Required. Client ID for the Private AI Services application.
– Required. The list of groups with Private AI Services users. If groups are not used, you can provide an empty group 
list in the Private AI Services activation YAML code.
– Name of the scope for the Private AI Services groups if the name is different from groups.
– Name of the claim with the Private AI Services groups if the name is different from groups.
– Required. Issuer certificate for the OIDC provider.
•
For activating metrics collection, verify that you have a machine with installed VCF Consumption CLI and kubectl. See
Getting Started with the VCF Consumption CLI.
You activate metrics collection by using commands after you activate Private AI Services in the VCF Automation UI.
When you log in to the organization in the VCF Automation UI and switch to the namespace, GPU-accelerated or 
CPU-only, under Services, you see the Private AI entry. However, you cannot create model endpoints, required for 
provisioning generative AI applications.
1. Log in to the All Apps organization in VCF Automation as an organization administrator.
2. On the Build&Deploy tab, select the namespace where you plan to activate Private AI Services.
3. Under Services, click Private AI and, on the Private AI Services page, click Configure Service.
4. In the Control Plane Resource section of the Private AI Services configuration wizard, select the VM classes and 
storage policy for the Kubernetes storage class for the control plane nodes of the VKS cluster for running model 
endpoints, and click Next.
Worker nodes are created when you deploy a model endpoint. The resources required for the worker nodes are 
specified individually for each endpoint at its deployment time.
VMware by Broadcom
134


---

VMware Private AI Foundation with NVIDIA 9.1
5. In the Database section of the Private AI Services configuration wizard, provide information about the PostgreSQL 
database with pgvector extension installed where the Private AI Services instance is going to store vector embeddings 
for knowledge bases used for RAG agents, and click Next.
You provide a different set of properties according to the vendor and location of the vector database.
Database Location
Configuration Details
Database in VMware Data Services Manager
•
If the database is in the same namespace, select PostgreSQL Cluster managed 
by Data Services Manager and select the database from the drop-down menu.
•
If the database is in a different namespace, select Bring your own PostgreSQL 
database and enter the details for connecting to the database.
Provide the CA certificate of the database if it is self-signed or the issuer's 
certificate is not publicly available.
Database created by using third-party 
software
Select Bring your own PostgreSQL database and enter the details for connecting to 
the database.
Provide the CA certificate of the database if it is self-signed or the issuer certificate is 
not publicly available.
6. In the OIDC section of the Private AI Services configuration wizard, provide the settings for authentication to the 
Private AI Services instance in the namespace according to the configuration of your OIDC provider and click Next.
Setting
Description
OIDC Issuer URL
Issuer URL of the OIDC provider as provided by your OIDC administrator.
Client ID
Client ID of the OIDC provider as provided by your OIDC administrator.
Certificate
Issuer certificate for the OIDC provider as provided by your OIDC administrator.
Group Name
If you are going to organize user accounts for Private AI Services in groups, activate Use 
authorized groups and enter the group names, separated with commas. For example, a
group-for-pais-access-01, group-for-pais-access-02 value means that 
the user must be a member of the group-for-pais-access-01 or group-for-pais-ac 
cess-02 group on the OIDC provider to be able to access Private AI Services.
Group Claim
groups
Extra Audiences
Use it if required by your OIDC provider for additional OAuth2.0 clients that Private AI Service 
will accept when validating the Access Token.
7. In the GPU Driver section of the Private AI Services configuration wizard, set the driver type for running model 
endpoints according to the GPU consumption model and click Next.
Note:
You can run models on CPU using llama.cpp in a namespace with GPU resources, for example, if they do not 
require GPU resources.
Run Models only on 
CPUs
For example, the Use CPUs Only option is suitable for running small models.
Models are run on llama.cpp.
Run Models on NVIDIA 
vGPUs or NVIDIA GPU 
Passthrough
If you plan to run models on ESX hosts with NVIDIA GPUs that support vGPU mode and that can also 
be used with GPU passthrough with Fixed DirectPath or Dynamic DirectPath as needed, provide the 
following configuration:
•
Select NVIDIA vGPU guest driver.
•
In the vGPU Driver License Token text box, enter the NVIDIA vGPU client configuration token.
•
Provide the following NVIDIA GPU Operator settings according to the external connectivity of your 
environment.
For a GPU Operator Overrides template, see https://github.com/NVIDIA/gpu-
operator/blob/v<operator-version>/deployments/gpu-operator/
values.yaml for the version supported by your Private AI Services version according to the 
VMware by Broadcom
135


---

VMware Private AI Foundation with NVIDIA 9.1
Private AI Services release notes. For example: https://github.com/NVIDIA/gpu-operator/blob/
v25.10.1/deployments/gpu-operator/values.yaml.
Environment Connectivity
Connectivity
Connected
•
Fill in YAML code in the GPU Operator 
Overrides text area if you want to use 
container images from another registry, a 
different version of the vGPU guest driver, 
change specific settings, and so on.
•
Enter $oathtoken as the user name and 
the API token for access to the NVIDIA NGC 
catalog.
Disconnected
If you plan to use a local registry to host the 
NVIDIA GPU Operator components, provide the 
following configuration:
•
Fill in YAML code in the GPU Operator 
Overrides if you want to use container images 
from a local registry.
•
Enter the registry URL, where the GPU 
Operator components are available, user name 
and password.
Run Models on NVIDIA 
GPU Passthrough
If you plan to run models only on GPU passthrough with Fixed DirectPath or Dynamic DirectPath, 
provide the following configuration:
1.
Select NVIDIA data center GPU driver.
2.
In a disconnected environment, if you plan to use a local registry to host the NVIDIA GPU Operator 
components, you provide a configuration similar to the one for running models on NVIDIA vGPUs or 
NVIDIA GPU passthrough.
8. Review the Private AI Services configuration and click Configure Private AI Services to start the activation in the 
namespace.
You are redirected to the Service Configuration page where you can monitor the activation progress. The status and 
status reason show the current stage of the activation process.
9. If you want to activate metrics collection for observability platforms or LLM traces for tracing in an OpenTelemetry 
(OTel) Collector the execution flow of agents across components and knowledge base indexing, you must update the
Private AI Services configuration by using the kubectl command-line tool.
You activate observability with a specific set of parameters in the Private AI Services configuration CR, called
PAISConfiguration, running in the namespace.
a) In VCF Automation UI, click your account in the top-right corner and select User Settings > My Account.
b) On the API Tokens tab, create a token and save it on your machine.
c) Log in to a machine running the VCF Consumption CLI and kubectl.
d) Create a context for the namespace where the instance is available by running the following command.
vcf context create vcf_automation_context_name --endpoint vcf_automation_endpoint --api-token 
 vcf_automation_api_token --ca-certificate vcf_automation_certificate.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces you have access to are created. 
The additional contexts are available as vcf_automation_context:namespace:project.
e) Switch to the context for the target namespace.
vcf context use <vcf_automation_context_name>:<namespace_name>:<project_name>
f)
Create a YAML file for activating observability, for example, called observability_enablement.yaml.
Metrics
spec: 
  observability: 
    prometheusRuntime: 
      storageClassName: storage_policy_metrics
VMware by Broadcom
136


---

VMware Private AI Foundation with NVIDIA 9.1
      metricsRetention: period_to_store_metrics_for
For example, add a configuration for a storage policy
storage_policy_for_metrics_retention and for retaining metrics for 14 days.
spec: 
  observability: 
    prometheusRuntime: 
      storageClassName: storage_policy_for_metrics_retention 
      metricsRetention: 14d
LLM Traces
spec: 
  observability: 
    llmTraces: 
      endpoint: "<otlp-http-endpoint>" 
      #  You can also use "grpc" to send traces. 
      # protocol: "grpc" 
      protocol: "http/protobuf" 
      projectName: "<pais-project-in-otel-collector>" 
      headersSecretRef: 
        name: <otel-collector-auth-secret>
        key: headers
---
apiVersion: v1
kind: Secret
metadata: 
  name: <otel-collector-auth-secret>
  # Semi-colon separated  list of headers 
  stringData:  
    headers: Authorization=Bearer <access-token>; <X-Custom-Header>=<custom-
header-value>
g) Apply the update to the PAISConfiguration CR.
kubectl patch PAISConfiguration default --patch-file observability_enablement.yaml --type='merge'
If you have turned on metrics collection, a Prometheus server is deployed in the namespace with its own metrics 
database. The database storage is backed by a Persistent Volume whose size is inferred from the metrics retention 
property in the PAISConfiguration CR.
h) Monitor the progress of the PAISConfiguration update.
•
kubectl get pods
In the output, verify that the API, ingress, rex worker, and Prometheus server pods are ready and running.
•
kubectl get paisconfiguration
In the output, verify that the Ready property for the default PAISConfiguration is paisAvailable. For 
example:
kubectl get paisconfiguration
NAME      SERVICE                READY   REASON
default   pais-ingress-default   True    paisAvailable 
•
kubectl describe paisconfiguration
In the output, locate the Status.Conditions set of properties and verify that Status is True, Type is
Ready, and Reason is paisAvailable. For example:
  Conditions: 
    Last Transition Time:   current-date
    Message:               all components are available 
    Observed Generation:   2 
    Reason:                paisAvailable 
    Status:                True 
VMware by Broadcom
137


---

VMware Private AI Foundation with NVIDIA 9.1
    Type:                  Ready
10. Locate the IP address that is assigned to the Private AI Services instance by the ingress service and associate it with 
the FQDN on the DNS server.
a) Back in the VCF Automation organization UI, on the Build & Deploy page, locate the namespace where you 
activated Private AI Services.
b) Click Services > Private AI and, on the Service Configuration page, locate Private AI Services UI IP, and copy 
the IP address.
c) Add a DNS entry for the IP address on the DNS server.
11. To use an FQDN instead of the external IP address assigned by the Ingress controller when switching between the 
components of the Private AI Services UI, add the paisconfigurations.pais.vmware.com/cname annotation to 
the PAISConfiguration resource with value the load balancer FQDN.
kubectl annotate PAISConfiguration default paisconfigurations.pais.vmware.com/
cname=<load_balancer_external_fqdn>
•
If you have not included some of them during Private AI Services activation, add the issuer certificate for the OIDC 
provider and for the database as CA trust bundles. See Import a CA Trust Bundle in Private AI Services.
•
If observability is activated in Private AI Services, MLOps engineers can visualize the Private AI Services metrics 
in Grafana. For information about available metrics and setting up metrics dashboards, see Visualizing Private AI 
Services Metrics in Observability Platforms.
•
If LLM traces collection is activated, MLOps engineers can analyze traces from agents and knowledge base indexing 
in the OpenTelemtry Collector. For information about available traces, see Tracing Agent Operation and Knowledge 
Base Indexing.
•
If you want to change the Private AI Services configuration later, select Private AI in the namespace in your VCF 
Automation organization and click Service Configuration.
Activate Private AI Services in an Organization Namespace by Using the kubectl Command
As an organization administrator, after you configure a GPU-accelerated namespace in an organization in VCF 
Automation, by using the kubectl command utility in this namespace, you can activate Private AI Services for 
provisioning generative AI applications including model endpoints and agents communicating to the endpoints, and for 
RAG applications, knowledge bases.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for
Private AI Services. See Setup Workflows for Private AI Services.
•
Verify that you have a machine with installed VCF Consumption CLI and kubectl. See Getting Started with the VCF 
Consumption CLI.
•
Create a PostgreSQL database with pgvector extension or get the connection details to an available one.
– If you are using VMware Data Services Manager, create the database in VCF Automation. See Create a Database 
Using VMware Data Services Manager in VMware Cloud Foundation Automation documentation. The pgvector 
extension is installed automatically.
– You can also create a PostgreSQL database with pgvector extension using third-party software. The PostgreSQL 
database must have the pgvector extension installed.
Private AI Services activates the installed pgvector extension during its activation in the namespace.
•
Verify that an Ingress controller with LoadBalancer support is available in the namespace.
•
Verify that the namespace includes a VM class and storage policy as a storage class for the control plane of the VKS 
cluster running model endpoints. The control plane nodes do not require GPU resources.
•
Verify that you have the certificate of your VCF Automation instance saved to your machine, for example, as a
vcf_automation_certificate.pem file in your current working folder.
•
Verify that an FQDN is allocated for the Private AI Services instance.
You map it to an IP address after the Private AI Services instance is active.
•
If you plan to run models on NVIDIA vGPUs, ask the VI administrator about the version of the NVIDIA vGPU host 
driver on the hosts in the GPU-accelerated cluster for your organization.
Private AI Services uses a default version of the NVIDIA vGPU guest driver for its model endpoint VKS worker nodes. 
If the version of the vGPU host driver is not compatible with this default version, then you must include an additional 
override config snippet in the YAML file for Private AI Services activation.
VMware by Broadcom
138


---

VMware Private AI Foundation with NVIDIA 9.1
As a VI administrator, to view the version of the vGPU host driver, follow these steps:
a. Go to the vCenter inventory in the vSphere Client and select a host.
b. On the Configure tab, click System > Packages.
c. Note the version of the NVIDIA AI Enterprise vGPU driver for VMware ESX entry.
•
Work with your OIDC provider administrator to set up identity and access management.
Private AI Services requires an OIDC client supporting the Authorization Code grant type with PKCE, initiated by
Private AI Services VCF Automation, to obtain an ID Token and Access Token as well as a Refresh Token. For more 
information about OIDC-based communication, see the OIDC specification.
You can configure Private AI Services only with one OIDC provider.
Verify that your environment has an OpenID Connect (OIDC) provider configured in the following way:
– Public OIDC client with PKCE enabled.
– Enabled Refresh Token and Authorization Code grant types.
– If supported by the OIDC provider, enabled Offline Access scope, that is required for Refresh Tokens enablement.
Issuing Refresh Tokens is a requirement for the Private AI Services UI. Many OIDC servers require the
offline_access scope for issuing Refresh Tokens, but not all servers support this scope. Check in the 
documentation of your OIDC service if this scope is supported and required.
– Optional. Private AI Services users are organized in one or more groups.
– A claim for the groups for Private AI Services, called groups, included in the Access Token.
– A scope, called groups, containing the claim for the groups for Private AI Services, created even if using groups is 
not required for your environment.
– Enabled HTTPS-based communication
– Supported <issuer_url>/.well-known/openid-configuration discovery endpoint
– Redirect URI set to https://<private_ai_services_FQDN_or_IP_address>/. Including the trailing /
character is mandatory.
•
Ask your OIDC provider administrator for the following details:
– Required. Issuer URL.
– Required. Client ID for the Private AI Services application.
– Required. The list of groups with Private AI Services users. If groups are not used, you can provide an empty group 
list in the Private AI Services activation YAML code.
– Required. Issuer certificate for the OIDC provider.
•
When you log in to the organization in the VCF Automation UI and switch to the GPU-accelerated namespace, under
Services, you see the Private AI entry. However, you cannot create model endpoints, required for provisioning 
generative AI applications.
•
If you want to activate Private AI Services in another namespace by using kubectl, you can download the 
configuration YAML from the right-hand side panel of the wizard.
1. Log in to your organization in VCF Automation as an organization administrator.
2. Get an API token for access to the namespace in VCF Automation where you are deploying a Private AI Services 
configuration.
a) Click your account in the top-right corner and select User Settings > My Account.
b) On the API Tokens tab, create a token and save it on your machine.
3. Log in to a machine running the VCF Consumption CLI and kubectl.
4. Create a context for the namespace where the Private AI Services instance must be available in VCF Automation.
vcf context create vcf_automation_context_name --endpoint vcf_automation_endpoint --api-token 
   vcf_automation_api_token --ca-certificate vcf_automation_certificate.pem --type cci
 
A primary VCF Automation context and additional contexts for the namespaces you have access to are created. The 
additional contexts are available as
<vcf_automation_context_name>:<namespace_name>:<project_name>.
5. Switch to the context for the target namespace.
vcf context use <vcf_automation_context_name>:<namespace_name>:<project_name>
VMware by Broadcom
139


---

VMware Private AI Foundation with NVIDIA 9.1
6. If you are running Private AI Services on vGPU, prepare a configuration map for the NVIDIA license and a secret for 
the NVIDIA NGC API token in YAML format.
For example, nvidia_license_config_map.yaml and nvidia_ngc_access_token_secret.yaml.
apiVersion: v1
kind: ConfigMap
metadata: 
  name:NVIDIA_license_config_map
data: 
  client_configuration_token.tok:license-server-access-token
  gridd.conf: ""
apiVersion: v1
kind: Secret
metadata: 
  name:ngc_access_token_secret
type: kubernetes.io/dockerconfigjson
data: 
    .dockerconfigjson:base-64-encoded-auths-for-nvcr.io
base-64-encoded-auths-for-nvcr.io represents the Base64 value of the following YAML code. There, the
auth property in its turn contains the $oauthtoken:<ngc-access-key> pair in the Base64 format.
{
"auths": { 
 "nvcr.io/nvidia/vgpu": { 
   "username": "$oauthtoken", 
   "password": "ngc-access-key", 
   "auth": "base-64-encoded-$oauthtoken:<ngc-access-key>" 
   } 
  }
}
7. Prepare configurations in YAML format for the following trust bundles for secure HTTPS connection.
•
Your OIDC provider
•
Your Harbor registry
•
Content management systems hosting data for your knowledge databases code
•
MCP servers providing tools to models for access to specialized data and capabilities
A ca-trust-bundle-for-harbor.yaml example file might contain the following details:
apiVersion: v1
kind: ConfigMap
metadata: 
  name:ca-trust-bundle-for-harbor
data: 
  ca.crt: | 
    -----BEGIN CERTIFICATE----- 
     <Harbor-registry-CA-certificate-in-PEM-format>
    -----END CERTIFICATE-----
8. Prepare a YAML file, for example, called paisconfiguration.yaml, in compliance with the PAISConfiguration 
CRD.
You configure Private AI Services by using the following sections in the YAML file:
Configuration Area
YAML Section
Storage policy for Data Indexing and Retrieval worker nodes
spec.worker.storageClassName
ConfigMap resources for the CA trust bundles
spec.clientTls.caBundleRefs
VMware by Broadcom
140


---

VMware Private AI Foundation with NVIDIA 9.1
Configuration Area
YAML Section
Settings for connecting to the assigned PostgreSQL database with pgvector 
extension
spec.database
Settings for integration with the OIDC provider
spec.auth.oidc
Required ingress service type
spec.ingress.serviceType
VM classes and Kubernetes storage class for the control plane nodes of the 
VKS cluster for running model endpoints
spec.vksControlPlane
GPU guest driver for running model endpoints according to the GPU 
consumption model
spec.nvidiaConfig
You can run the following command to see the properties you must include in the YAML file.
kubectl explain paisconfiguration
Note:
You can run models on CPU using llama.cpp in a namespace with GPU resources, for example, if they do not 
require GPU resources or if your GPUs do not provide enough VRAM for some massive models.
a) If you plan to run Private AI Services on NVIDIA vGPUs, use the one of following YAML templates according to 
your NVIDIA vGPU host driver configuration.
If the version of the NVIDIA vGPU host driver on the ESX hosts for your organization is 570.x, you can use the 
following example template.
If you want to pull the GPU operator from a custom container registry, for example, in a disconnected environment, 
you can add a spec.nvidiaConfig.gpuOperatorOverridesRef ConfigMap. See https://docs.nvidia.com/
datacenter/cloud-native/gpu-operator/latest/getting-started.html#common-chart-customization-options. For a GPU 
Operator Overrides template, see https://github.com/NVIDIA/gpu-operator/blob/v<operator-
version>/deployments/gpu-operator/values.yaml for the version supported by your Private AI Services 
version according to the Private AI Services release notes. For example: https://github.com/NVIDIA/gpu-operator/
blob/v25.10.1/deployments/gpu-operator/values.yaml.
# Configure Private AI Service for a namespace.
# It depends on several other resources.
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  # Setting "name" to "default" is mandatory 
  name: default 
  # Remove the comments in the annotations section 
  # to use an FQDN instead of your LoadBalancer external IP address when switching 
  # between the components of the Private AI Services UI 
  # annotations: 
    # paisconfigurations.pais.vmware.com/cname: load_balancer_external_fqdn
spec: 
  worker: 
    storageClassName:storage_policy_for_Data_Indexing_and_Retrieval_worker_nodes
  clientTls: 
    # Add references to the trust bundles. It is a good practice 
    # to break the bundles per application. You can also consolidate trust information 
    # in fewer bundles according to your authentication setup.   
    # Make sure that you update a trust bundle if the associated certificate 
    # is about to expire. 
    caBundleRefs: 
      - name:ca-trust-bundle-for-oidc-provider
      - name:ca-trust-bundle-for-harbor
      - name:ca-trust-bundle-for-dsm
      # - name:ca-trust-bundle-for-another-application
VMware by Broadcom
141


---

VMware Private AI Foundation with NVIDIA 9.1
  database: 
    host:pgvector_server_host_name
    username:pgvector_db_admin_name
    dbname:pgvector_db_name
    passwordRef: 
      name:pgvector_credentials_secret
      fieldPath:field_with_pgvector_db_admin_password
  auth: 
    oidc: 
      issuerUrl:OIDC_discovery_endpoint_URL
      clientId: pais-authentication-id
    scope: 
      - openid 
      - groups 
      # Issuing refresh tokens is a requirement for the Private AI Services UI. 
      # Many OIDC servers require the "offline_access" scope for issuing 
      # refresh tokens but 
      # not all servers support this scope. 
      # Check in the documentation of your OIDC server  
      # if this scope is supported and required 
      - offline_access 
      # If no groups are required,  
      # provide an empty authorizedGroups array. 
      # authorizedGroups: [] 
      # In this example, user accounts that are members of the 
      # group-for-pais-access-01 or group-for-pais-access-02 group  
      # can access Private AI Services. 
      authorizedGroups: 
        - group-for-pais-access-01
        - group-for-pais-access-02
  ingress: 
    serviceType: LoadBalancer 
  # Private AI Services deploys a vSphere Kubernetes Service (VKS) cluster  
  # to run models. Initially, the VKS cluster has only control plane nodes,  
  # used for managing model endpoints.  
  # Select the VM class and storage class  
  # which represent the resources  
  # required to run the control plane of this VKS cluster. 
  vksControlPlane: 
    virtualMachineClassName: vm_class_VKS_control_plane_node
    storageClassName: storage_policy_VKS_control_plane_node
  nvidiaConfig: 
    licenseConfigRef: 
      name: NVIDIA_license_config_map
      imagePullSecretRef: 
        name: ngc_access_token_secret
  # Remove the comments to activate observability . 
  # observability: 
    #  Use this section for activating metrics collection in Prometheus. 
    # prometheusRuntime: 
      # storageClassName: storage_policy_metrics
      # metricsRetention: period_to_store_metrics_for
    # llmTraces: 
      # Use this section for sending LLM traces to  
      # an OpenTelemetry Collector.       
      # endpoint: "<otlp-http-endpoint>" 
      # You can send traces over "http/protobuf" or "grpc".       
VMware by Broadcom
142


---

VMware Private AI Foundation with NVIDIA 9.1
      # protocol: "http/protobuf" 
      # projectName: "<pais-project-in-otel-collector>" 
      # headersSecretRef: 
        # name: <otel-collector-auth-secret>
        # key: headers
# Secret for authenticating to the OpenTelemetry Collector
# to send LLM traces to.
#---
# apiVersion: v1
# kind: Secret
# metadata: 
  # name: <otel-collector-auth-secret>
  # Semi-colon separated  list of headers 
  # stringData:  
    # headers: Authorization=Bearer <access-token>; <X-Custom-Header>=<custom-header-value>
If you plan to run Private AI Services on NVIDIA vGPUs and the version of the NVIDIA vGPU host driver on the 
ESX hosts for your organization is 550.x or 535.x, for example, 550.144.03, use the following YAML template.
In addition to the configuration of the Private AI Services service, you include a ConfigMap object for the NVIDIA 
GPU Operator and reference it as spec.nvidiaConfig.gpuOperatorOverridesRef. The ConfigMap 
specifies the version of the host driver and the image of the vGPU guest driver used by the operator which is 
specific to the driver version.
# Configure Private AI Service for a namespace.
# It depends on several other resources.
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  # Setting "name" to "default" is mandatory 
  name: default 
  # Remove the comments in the annotations section 
  # to use an FQDN instead of your LoadBalancer external IP address when switching 
  # between the components of the Private AI Services UI 
  # annotations: 
    # paisconfigurations.pais.vmware.com/cname=load_balancer_external_fqdn:load_balancer_external_fqdn
spec: 
  worker: 
    storageClassName: storage_class_for_Data_Indexing_and_Retrieval_worker_nodes
  clientTls: 
    caBundleRefs: 
      - name: ca-trust-bundle-for-oidc-provider
      - name: ca-trust-bundle-for-harbor
      - name: ca-trust-bundle-for-dsm
    database: 
      host: pgvector_server_host_name
      username: pgvector_db_admin_name
      dbname: pgvector_db_name
      passwordRef: 
        name: pgvector_credentials_secret
        fieldPath: field_with_pgvector_db_admin_password
    auth: 
      oidc: 
        issuerUrl: OIDC_discovery_endpoint_URL
        clientId: "pais-authentication-id" 
      scope: 
        - openid 
        - groups 
VMware by Broadcom
143


---

VMware Private AI Foundation with NVIDIA 9.1
        # Issuing refresh tokens is a requirement for the Private AI Services UI. 
        # Many OIDC servers require the "offline_access" scope for issuing 
        # refresh tokens but 
        # not all servers support this scope. 
        # Check in the documentation of your OIDC server  
        # if this scope is supported and required 
        - offline_access 
        # If no groups are required,  
        # provide an empty authorizedGroups array. 
        # authorizedGroups: []  
        # In this example, user accounts that are members of the 
        # group-for-pais-access-01 or group-for-pais-access-02 group  
        # can access Private AI Services. 
        authorizedGroups: 
          - group-for-pais-access-01
          - group-for-pais-access-02
  ingress: 
    serviceType: LoadBalancer 
  # Private AI Services deploys a vSphere Kubernetes Service (VKS) cluster  
  # to run models. Initially, the VKS cluster has only control plane nodes,  
  # used for managing model endpoints.  
  # Select the VM class and storage class  
  # which represent the resources  
  # required to run the control plane of this VKS cluster. 
  vksControlPlane: 
    virtualMachineClassName: vm_class_VKS_control_plane_node
    storageClassName: storage_policy_VKS_control_plane_node
  nvidiaConfig: 
    licenseConfigRef: 
      name: NVIDIA_license_config_map
    imagePullSecretRef: 
      name: ngc_access_token_secret
    gpuOperatorOverridesRef: 
      name: gpu-operator-chart-values 
  # Remove the comments to activate observability  
  # by providing metrics in Prometheus. 
  # observability: 
    # prometheusRuntime: 
      # storageClassName: storage_policy_metrics
      # metricsRetention: period_to_store_metrics_for
    # llmTraces: 
      # Use this section for sending LLM traces to  
      # an OpenTelemetry Collector. 
      # endpoint: "<otlp-http-endpoint>" 
      # You can send traces over "http/protobuf" or "grpc".       
      # protocol: "http/protobuf" 
      # projectName: "<pais-project-in-otel-collector>" 
      # headersSecretRef: 
        # name: <otel-collector-auth-secret>
        # key: headers
---
apiVersion: v1
kind: ConfigMap
metadata: 
  name: gpu-operator-chart-values
data: 
VMware by Broadcom
144


---

VMware Private AI Foundation with NVIDIA 9.1
  values.yaml: | 
    # If the version of the host vGPU driver is 550.x, 
    # use "image: vgpu-guest-driver-5" 
    # If the version of the host vGPU driver is 535.x, 
    # use "image: vgpu-guest-driver-4" 
    driver: 
      version: "550.144.03" 
      image: vgpu-guest-driver-5
# Secret for authenticating to the OpenTelemtry Collector
# to send LLM traces to.
#---
# apiVersion: v1
# kind: Secret
# metadata: 
  # name: <otel-collector-auth-secret>
  # Semi-colon separated  list of headers 
  # stringData:  
    # headers: Authorization=Bearer <access-token>; <X-Custom-Header>=<custom-header-value>
b) If you plan to run model endpoints on NVIDIA GPU devices in passthrough mode using an NVIDIA data center 
driver, you can use one of the YAML templates in the previous substep modifying the spec.nvidiaConfig
section in the following way.
Private AI Services pulls the driver from the public NVIDIA repository.
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  name: default 
  # Remove the comments in the annotations section 
  # to use an FQDN instead of your LoadBalancer external IP address when switching 
  # between the components of the Private AI Services UI 
  # annotations: 
    # paisconfigurations.pais.vmware.com/cname=load_balancer_external_fqdn:load_balancer_external_fqdn
spec:
  <rest of the specification part>
  nvidiaConfig: 
    useOSSDriver: true   
If you want to pull the GPU operator from a custom container registry, for example, in a 
disconnected environment, or to use a customer version of the NVIDIA driver, you can add a
spec.nvidiaConfig.gpuOperatorOverridesRef section. See https://docs.nvidia.com/datacenter/cloud-
native/gpu-operator/latest/getting-started.html#common-chart-customization-options. For a GPU Operator 
Overrides template, see https://github.com/NVIDIA/gpu-operator/blob/v<operator-version>/
deployments/gpu-operator/values.yaml for the version supported by your Private AI Services version 
according to the Private AI Services release notes. For example: https://github.com/NVIDIA/gpu-operator/blob/
v25.10.1/deployments/gpu-operator/values.yaml.
apiVersion: pais.vmware.com/v1alpha1
kind: PAISConfiguration
metadata: 
  name: default 
  # Remove the comments in the annotations section 
  # to use an FQDN instead of your LoadBalancer external IP address when switching 
  # between the components of the Private AI Services UI 
  # annotations: 
    # paisconfigurations.pais.vmware.com/cname=load_balancer_external_fqdn:load_balancer_external_fqdn
spec:
  <the rest of the specification part>
VMware by Broadcom
145


---

VMware Private AI Foundation with NVIDIA 9.1
  nvidiaConfig: 
    useOSSDriver: true   
    gpuOperatorOverridesRef: 
      name: gpu-operator-chart-values
---
apiVersion: v1
kind: ConfigMap
metadata: 
  name: gpu-operator-chart-values
data: 
  values.yaml: | 
    driver: 
      version: "580.105.08"
c) If you plan to run model endpoints on CPU resources, you can use one of the YAML templates in the first substep 
leaving the spec.nvidiaConfig section out.
Models are run on llama.cpp.
9. If you want to use an FQDN instead of your LoadBalancer external IP address when switching between the 
components of the Private AI Services UI, uncomment and update the paisconfigurations.pais.vmware.com/
cname section of the PAISConfiguration CR file in the previous step.
The Model Runtime UI uses the FQDN, specified in the annotation, in the external links to the Agent Builder and to the 
Data Indexing and Retrieval UIs, instead of the default behavior of using the LoadBalancer external IP address.
10. If you want to activate metrics collection for observability, uncomment the
spec.observability.prometheusRuntime section of the PAISConfiguration CR file in Step 8.
A Prometheus server is deployed in the namespace as a part of Private AI Services with its own metrics database. 
The database storage is backed by a Persistent Volume whose size is inferred from the metrics retention property in 
the PAISConfiguration CR. For information about provided metrics and setting up metrics dashboards, see Visualizing 
Private AI Services Metrics in Observability Platforms.
11. If you want to activate LLM traces collection in a OpenTelemetry Collector, uncomment the
spec.observability.llmTraces section of the PAISConfiguration CR file in Step 8.
12. Save the configurations for the trust bundles, and if you are running Private AI Services on vGPU, for the NVIDIA 
license and NVIDIA NGC secret, by running the following command for each entry.
kubectl apply -f yaml_file
13. Activate Private AI Services in the namespace by running the following command.
kubectl apply -f paisconfiguration.yaml
14. To monitor the progress of Private AI Services's components deployment, run the following commands.
•
kubectl get pods
In the output, verify that the API, ingress, and rex worker pods, and the Prometheus server pod if observability is 
turned on, are ready and running.
•
kubectl get paisconfiguration
In the output, verify that the Ready property for the default PAISConfiguration is paisAvailable. For example:
kubectl get paisconfiguration
NAME      SERVICE                READY   REASON
default   pais-ingress-default   True    paisAvailable 
•
kubectl describe paisconfiguration
In the output, locate the Status.Conditions set of properties and verify that Status is True, Type is Ready, 
and Reason is paisAvailable. For example:
  Conditions: 
    Last Transition Time:   current-date
    Message:               all components are available 
    Observed Generation:   2 
VMware by Broadcom
146


---

VMware Private AI Foundation with NVIDIA 9.1
    Reason:                paisAvailable 
    Status:                True 
    Type:                  Ready
15. Locate the IP address that is assigned to the Private AI Services instance by the ingress service and associate it with 
the FQDN on the DNS server.
Using kubectl
Run the following command.
kubectl get svc pais-ingress-default
In the output, locate the EXTERNAL-IP property.
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)         
 AGE
pais-ingress-default   LoadBalancer   x.x.x.x         x.x.x.x          443:30591/TCP 
      10d
Using Private AI 
Services UI
1.
Back in the VCF Automation organization UI, on the Build & Deploy page, locate the namespace 
where you activated Private AI Services.
2.
Click Services > Private AI and on the Service Configuration page, locate Private AI Services UI 
IP and copy the IP address.
3.
Add a DNS entry for the IP address on the DNS server.
•
MLOps engineers and data scientists can provision generative AI applications for application developers on top of 
specific models and data sources. See Delivering Generative AI Applications by Using Private AI Services.
•
If observability is activated in Private AI Services, MLOps engineers can visualize the Private AI Services metrics 
in Grafana. For information about provided metrics and setting up metrics dashboards, see Visualizing Private AI 
Services Metrics in Observability Platforms.
•
If LLM traces collection is activated, MLOps engineers can analyze traces from agents and knowledge base indexing 
in the OpenTelemtry Collector.
Turn on GPU for a Private AI Services 3.0 Instance by Using the UI
If you have initially configured your Private AI Services instance to run model endpoints only on CPU resources, you can 
later turn on GPU support so that you can start running models that require GPU resources.
You can continue to run certain models only on CPU resources in the namespace using llama.cpp.
Switch to GPU mode by using the Private AI Services service configuration UI and the default configuration for the 
vGPU or GPU passthrough.
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation as an organization administrator.
2.
On the Build&Deploy tab, select the namespace where you plan to activate Private AI Services.
3.
Under Services, click Private AI and, on the Private AI Services page, click Configure Service.
vSphere Client for
Private AI Services
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Administrator role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace where you plan to activate Private AI Services.
3.
On the Resources tab, click Private AI and, on the Private AI Services page, click Configure 
Service.
2. On the Private AI Services page, click the Service Configuration tab.
3. In the GPU Configurations section, click Edit.
4. Set the GPU driver type for running model endpoints according to your GPU consumption model and click Save.
Run Models on NVIDIA 
vGPUs and potentially 
CPU
If you plan to run models on ESX hosts with NVIDIA GPUs that support vGPU mode and that can also 
be used with GPU passthrough with Fixed DirectPath or Dynamic DirectPath as needed, provide the 
following configuration:
•
Select NVIDIA vGPU guest driver.
VMware by Broadcom
147


---

VMware Private AI Foundation with NVIDIA 9.1
•
In the vGPU Driver License Token Config Map text box, enter the NVIDIA vGPU client 
configuration token.
•
Provide NVIDIA GPU Operator settings.
For a GPU Operator Overrides template, see https://github.com/NVIDIA/gpu-
operator/blob/v<operator-version>/deployments/gpu-operator/
values.yaml for the version that is supported according to the Private AI Services release notes. 
For example: https://github.com/NVIDIA/gpu-operator/blob/v25.10.1/deployments/gpu-operator/
values.yaml.
Note:
To configure a different GPU Operator version or use a non-default driver version or image, use 
the CLI after you complete the setup in the Private AI Services service configuration UI.
Setting driver.version inside the
spec.nvidiaConfig.gpuOperatorOverridesRef ConfigMap will not be 
accommodated in the Private AI Services configuration.
Note:
Leave the location of and credentials for a local OCI image registry for custom images for 
NVIDIA GPU Operator empty. The GPU Operator components are already available in the 
Supervisor instance.
Environment Connectivity
Connectivity
Connected
•
Fill in YAML code in the GPU Operator 
Overrides text area if you want to configure 
the NVIDIA GPU Operator itself.
•
Enter $oathtoken as the user name and 
the API token for access to the NVIDIA NGC 
catalog.
Disconnected
Fill in YAML code in the GPU Operator Overrides
text area if you want to configure the NVIDIA GPU 
Operator itself.
Run Models on NVIDIA 
GPU Passthrough and 
potentially CPU
If you plan to run models only on GPU passthrough with Fixed DirectPath or Dynamic DirectPath, 
provide the following configuration:
1.
Select NVIDIA data center GPU driver.
2.
Fill in YAML code in the GPU Operator Overrides text area if you want to use the NVIDIA GPU 
Operator itself.
Note:
Leave the location of and credentials for a local OCI image registry for custom images for 
NVIDIA GPU Operator empty. The GPU Operator components are already available in the 
Supervisor instance.
If required by your organization, configure a custom NVIDIA GPU Operator version or a custom GPU driver version 
or image.
5. Open the PAISConfiguration resource in the namespace in edit mode.
a) On the machine with VCF Consumption CLI and kubectl, create a context for the namespace where the instance 
is available by running the following command.
Use the --ca-certificate option if the VCF Automation or vSphere Supervisor certificate is not added to the 
truststore on your machine.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token  <vcf_automation_api_token> --ca-
certificate <vcf_automation_certificate>.pem --type cci
VMware by Broadcom
148


---

VMware Private AI Foundation with NVIDIA 9.1
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --
ca-certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access 
to are created. An additional context is available as <context_name>:<namespace_name>.
b) Switch to the context for the target namespace.
 VCF Automation
vcf context use <vcf_automation_context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <supervisor_context_name>:<namespace_name>
c) Open the PAISConfiguration resource in the namespace in edit mode.
kubectl edit paisconfiguration default
6. If required by your organization, switch to a different GPU Operator version, for example, to 26.3.1.
Locate the spec.nvidiaConfig section and add the following property:
gpuOperatorVersion: 26.3.1
As a result the nvidiaConfig section contains the following configuration:
spec: 
  <rest of the PAISConfiguration resource specification> 
  nvidiaConfig: 
   gpuOperatorVersion: 26.3.1 
  <rest of the nvidiaConfig specification>
7. If required by your organization, set a driver version for vGPU or GPU Passthrough that is different from the default 
one for the GPU Operator version,
NVIDIA vGPU
If you want to use a vGPU driver version that is different from the default one, 580.105.08, in the
spec.nvidiaConfig.driver.image property provide the location of the driver container image 
in the NVIDIA NGC registry or in your private container registry and, if you have not done so in the 
activation UI, in the spec.nvidiaConfig.imagePullSecretRef property - a Secret with the 
credentials for this registry. For example, if you want to use a driver version which contains a fix from 
NVIDIA.
spec: 
  <rest of the PAISConfiguration resource specification> 
  nvidiaConfig: 
    imagePullSecretRef: 
      name: my-registry-pull-secret 
    driver: 
      image: my-registry.example.com/nvidia/custom-driver:580.105.08-ubuntu24.04
NVIDIA GPU 
Passthrough
If you want to use a data center GPU driver version that is different from the default one, 580.105.08, 
you have two options:
•
Use a validated driver version that is packaged within the NVIDIA GPU Operator 
package for Private AI Services. In this case, you set the driver version in the
spec.nvidiaConfig.driver.version property.
You can switch to driver version 580.126.20.
•
Use a non-validated for Private AI Services driver version that is available in the NVIDIA NGC 
registry or in another location. For example, if you built your driver from a custom source. In this 
case, set the following properties:
– spec.nvidiaConfig.driver.image. Provide the location of the driver container image 
in your container registry
– spec.nvidiaConfig.imagePullSecretRef . If you have not already done so in the 
activation UI, provide a Secret with the credentials for this registry.
spec: 
  <rest of the PAISConfiguration resource specification> 
VMware by Broadcom
149


---

VMware Private AI Foundation with NVIDIA 9.1
  nvidiaConfig: 
    imagePullSecretRef: 
      name: my-registry-pull-secret 
    driver: 
      image: my-registry.example.com/nvidia/custom-driver:580.105.08-ubuntu24.04
8. Save the PAISConfiguration changes in the editor.
If you have entered a custom NVIDIA GPU Operator custom configuration in the GPU Operator Overrides text area, the 
configuration is saved in a ConfigMap resource in the namespace.
Switching Private AI Services 3.0 to CPU
If you want to use only CPU resources for model endpoints, consider model endpoints that are running on GPU and 
freeing up their resources.
If you turn off GPU for a Private AI Services instance by using the service configuration UI or kubectl, model endpoints 
that are running on GPU will continue to use GPU. To free up GPU resources, you must delete the model endpoints and 
must redeploy them with llama.cpp inference engine, CPU-only VM classes for the worker nodes, and GGUF model 
images.
Enable the Full Set of Private AI Services 3.0 Capabilities by Using the UI
You can turn on the capabilities for using knowledge bases for RAG applications or create agents including MCP tools 
integration in a Private AI Services instance that has been initially configured only for running model endpoints.
Create a PostgreSQL database with pgvector extension or get the connection details to an available one. The database is 
required to store vector embeddings for the knowledge bases in Data Indexing and Retrieval and for Agent Builder.
•
If you are using VMware Data Services Manager, create the database in VCF Automation. See Create a Database 
Using VMware Data Services Manager in VMware Cloud Foundation Automation documentation. The pgvector 
extension is installed automatically.
•
You can also create a PostgreSQL database with pgvector extension using third-party software. The PostgreSQL 
database must have the pgvector extension installed.
Private AI Services activates the installed pgvector extension during its activation in the namespace.
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation as an organization administrator.
2.
On the Build&Deploy tab, select the namespace where you plan to activate Private AI Services.
3.
Under Services, click Private AI and, on the Private AI Services page, click Configure Service.
vSphere Client for
Private AI Services
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Administrator role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace where you plan to activate Private AI Services.
3.
On the Resources tab, click Private AI and, on the Private AI Services page, click Configure 
Service.
VMware by Broadcom
150


---

VMware Private AI Foundation with NVIDIA 9.1
2. On the Private AI Services page, click the Service Configuration tab.
3. In the Service Capabilities row, click Enable Full Service.
4. Provide information about the PostgreSQL database with pgvector extension installed and click Enable.
You provide a different set of properties according to the vendor and location of the vector database.
Database Location
Configuration Details
Database in VMware Data Services Manager
•
If the database is in the same namespace, select PostgreSQL Cluster managed 
by Data Services Manager and select the database from the drop-down menu.
•
If the database is in a different namespace, select Bring your own PostgreSQL 
database and enter the details for connecting to the database.
Provide the CA certificate of the database if it is self-signed or the issuer's 
certificate is not publicly available.
Database created by using third-party 
software
Select Bring your own PostgreSQL database and enter the details for connecting to 
the database.
Provide the CA certificate of the database if it is self-signed or the issuer certificate is 
not publicly available.
5. After the operation is complete, verify that the new capabilities are available in the Private AI Services UI.
a) Click Agent Builder at the top of the page and log in using your credentials in the identity provider for Private AI 
Services.
b) Verify that the Data Indexing and Retrieval, MCP, and Agent Builder areas are active.
Enable Metrics Collection and LLM Traces in Private AI Services 3.0 by Using the 
UI
You can see observe the behavior of your Private AI Services instance directly in the Private AI Services UI or in 
observability platforms, such as Grafana, by using a Prometheus server deployed in the namespace. You can also 
turn on LLM traces for tracing the execution flow of agents across components and knowledge base indexing in an 
OpenTelemetry (OTel) Collector .
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation as an organization administrator.
2.
On the Build&Deploy tab, select the namespace where you plan to activate Private AI Services.
3.
Under Services, click Private AI and, on the Private AI Services page, click Configure Service.
vSphere Client for
Private AI Services
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Administrator role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace where you plan to activate Private AI Services.
3.
On the Resources tab, click Private AI and, on the Private AI Services page, click Configure 
Service.
2. If required by your organization, in the Metrics section, turn on metrics collection.
a) Click Enable.
b) Set the time to retain metrics data and optionally a storage policy for metrics retention.
c) Click Save.
3. If required by your organization, in the LLM Traces section, turn on LLM traces.
a) Click Enable.
b) Configure the following settings for sending traces to an OpenTelemetry Collector.
Setting
Description
Endpoint
HTPP URL of the collector endpoint
Protocol
grpc or http/protobuf
Project Name
Project for Private AI Services on the collector
VMware by Broadcom
151


---

VMware Private AI Foundation with NVIDIA 9.1
Setting
Description
Authorization Secret
Send HTTP headers for authorization to the OpenTelemetry Collector. You can 
enter new header key-value pairs, such as Authorization=Bearer <ac 
cess-token>; <X-Custom-Header>=<custom-header-value>,
or reuse headers from a secret already available in the namespace.
c) Click Save.
4. After the operation is complete, verify that the new capabilities are available in the Private AI Services UI.
a) Click Agent Builder at the top of the page and log in using your credentials in the identity provider for Private AI 
Services.
b) Verify that the Observability area is active.
•
If observability is activated in Private AI Services, MLOps engineers can visualize the Private AI Services metrics 
in Grafana. For information about available metrics and setting up metrics dashboards, see Visualizing Private AI 
Services Metrics in Observability Platforms.
•
If LLM traces collection is activated, MLOps engineers can analyze traces from agents and knowledge base indexing 
in the OpenTelemtry Collector. For information about available traces, see Tracing Agent Operation and Knowledge 
Base Indexing.
Visualizing Private AI Services Metrics in Observability Platforms
As a DevOps engineer, you can visualize in an observability platform, such as Grafana, health and performance metrics 
for the components of the Private AI Services instance running in a VCF Automation namespace. As a VI administrator, 
you can visualize in VCF Operations metrics for the operation of the Private AI Services controller pod.
Some of the example use cases where you need metrics analysis are as follows:
•
As a DevOps engineer or application developer
– LLM completion API response measured at 95th percentile
– Inference engine tokens-per-second generation rate
– GPU compute and memory resources that are undersized or oversized
– While benchmarking, inference engine pod memory usage
•
As a VI administrator or platform operator
– Pods requesting frequent container restarts
– vGPU licensing status issues to prevent performance degradation of model endpoints
– Idle or underutilized GPU devices in passthrough mode to avoid wasting allocated hardware capacity
Set Up Grafana for Private AI Services
To monitor or troubleshoot your Private AI Services deployment, you can connect a third-party tool for metrics 
visualization, such as Grafana. One Grafana instance can visualize metrics from multiple Private AI Services instances as 
long as they use the same OIDC provider.
To set up Grafana for observing Private AI Services, perform the following high-level steps:
1. For retrieving specific information about Private AI Services, set up a machine that has access to VCF Automation , 
and has VCF Consumption CLI and kubectl. See
Getting Started with the VCF Consumption CLI.
2. Verify that Grafana instance has access to the Private AI Services endpoint.
3. Verify that observability is active in the Private AI Services instance in the namespace in VCF Automation or vSphere 
Supervisor.
Observability is configured as part of the Private AI Services activation in VCF Automation or vSphere Supervisor. 
For more information about activating Private AI Services, see Activate Private AI Services 3.0 in an Organization 
Namespace by Using the UI and Activate Private AI Services 3.0 in an Organization Namespace by Using the kubectl 
Commandfor Private AI Services 3.0, and Activate Private AI Services 2.1 in an Organization Namespace by Using the 
VCF Automation UI and Activate Private AI Services in an Organization Namespace by Using the kubectl Command
for Private AI Services 2.1.
a. Log in to the namespace with Private AI Services by using the VCF Consumption CLI.
VMware by Broadcom
152


---

VMware Private AI Foundation with NVIDIA 9.1
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token <vcf_automation_api_token> --ca-
certificate <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --
ca-certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access 
to are created. An additional context is available as <context_name>:<namespace_name>.
b. Switch to the context of the namespace.
 VCF Automation
vcf context use <context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <context_name>:<namespace_name>
c. Run the following command and in the output verify that the observability settings are present in the default 
PAISConfiguration CR.
kubectl get paisconfiguration default -o jsonpath='{.spec.observability}'
d. Run the following command and in the output verify that the Prometheus server pod is ready and running.
kubectl get pods
4. Configure Grafana to use the same OIDC provider for authentication as the Private AI Services instance.
Private AI Services configures the Prometheus server to provide API access to metrics and protects that API endpoint 
with the same authentication mechanism that is used by the API for accessing Private AI Services functionality. As a 
result, Grafana must be configured with the same OIDC provider as Private AI Services.
Task
Description
Work with your OIDC provider administrator to 
extend the configuration in the OIDC provider for 
Private AI Services.
1.
Add a scope, called email.
The email information is only used for Grafana. It is not required by the
Private AI Services instance.
2.
A redirect URI set to https://<grafana_FQDN_or_IP_address> 
/.*.
For information about the OIDC provider configuration for Private AI Services, 
see Activate Private AI Services in an Organization Namespace in VCF 
Automation.
Ask your OIDC provider administrator for the details 
you need to be able to configure Grafana.
•
Required. Authentication URL, token URL, and API URL.
•
Required. Client ID for the Private AI Services application.
•
List of groups with Private AI Services users.
•
Required. Issuer certificate for the OIDC provider.
Configure Grafana with Generic OAuth 
authentication using the information from the OIDC 
provider administrator.
For information about setting OAuth authentication for Grafana, see Grafana's 
Generic OAuth authentication in the Grafana documentation.
In addition, set the following configuration:
•
Turn on sign-up.
•
Have the Admin role in Grafana automatically assigned to a subset of OIDC 
users and the Viewer role to the rest of the users. Set the following value to 
the role attribute path user mapping property:
contains(groups[*], 'pais-access-group') && 'Admin' || 
 'Viewer'
where pais-access-group represents an example group for Private AI 
Services users.
•
Enable PKCE and refresh tokens
VMware by Broadcom
153


---

VMware Private AI Foundation with NVIDIA 9.1
5. Add a Prometheus data source in Grafana with the following configuration.
Setting
Value
Prometheus server URL
https://<private_ai_services_FQDN_or_IP_address>/promethe 
us/
You can view the Private AI Services FQDN or IP address in one of the following ways:
•
In the VCF Automation UI, click Services > Private AI and on the Service 
Configuration page, locate Private AI Services UI IP and copy the IP address.
•
Copy the external IP address from the output of the following command.
kubectl get svc pais-ingress-default
Authentication method
Forward OAuth identity
TLS settings
•
Self-signed certificate authentication turned on.
CA certificate must be set to the base64 decoded content of the ca.crt
property from the pais-ingress-default Secret in the Private AI Services 
namespace.
To get the self-signed certificate for the Private AI Services instance, run the 
following command and copy the content of the ca.crt property:
kubectl get secrets/pais-ingress-default -o yaml
•
TLS client authentication and certificate validation turned off.
For information about available metrics, see Observing the Performance of Private AI Services Components by Using 
Metrics.
Visualize Private AI Services Controller Metrics in VCF Operations
Тhe Supervisor Service of Private AI Services has a controller pod running in a dedicated system namespace. This 
controller pod is responsible for ensuring that the PAISConfiguration and ModelEndpoint Kubernetes CRs that are 
part of this service, eventually translate into the Private AI Services application getting deployed in the namespaces of the 
organization.
Because the same controller pod serves all namespaces across the organizations on a Supervisor cluster, as a VI 
administrator, you can access service-level metrics so that you can monitor the health of the Private AI Services
Supervisor Service. You send these metrics from the controller pod to VCF Operations for visualization by using the 
Telegraf agent running on the Supervisor instance. For information about streaming metrics by using Telegraf on the 
Supervisor, see Streaming Supervisor Metrics to a Custom Observability Platform in the VMware Cloud Foundation 
documentation.
As a VI administrator, perform these steps to stream metrics from the Private AI Services controller pod on the Supervisor 
cluster to VCF Operations.
1. Use the VCF Consumption CLI to connect to the Supervisor endpoint.
a. In the command line, get the Supervisor certificate by using the OpenSSL command-line utility and save it to a file, 
for example, called supervisor_certificate.pem.
b.
openssl s_client -connect supervisor_ip_address:6443 </dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END 
 CERTIFICATE-/p' > supervisor_certificate.pem
c. Log in to the Supervisor instance by using the VCF Consumption CLI.
Use the --ca-certificate option if the Supervisor certificate is not added to the truststore on your machine.
vcf context create supervisor_context_name --endpoint supervisor_fqdn_or_ip --ca-
certificate supervisor_certificate.pem --type k8s
d. Switch to the context for the Supervisor instance.
vcf context use supervisor_context_name
2. Find the system namespace for the Supervisor Service of Private AI Services by running the following commands.
a. Run the following command to get the system namespace of the controller pod.
b.
kubectl get namespace | grep "svc-pais-"
Write down the namespace name from the command output. For example:
svc-pais-<suffix>                              Active   16d
VMware by Broadcom
154


---

VMware Private AI Foundation with NVIDIA 9.1
c. Run the following command to get the pods and services in the system namespace and verify that the controller 
pod is running in the namespace.
kubectl -n svc-pais-<suffix> get pods,services
NAME                                      READY   STATUS    RESTARTS   AGE
pod/controller-manager-6c6bfbfc4c-wvx98   1/1     Running   0          23h
NAME                              TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    
 AGE
service/pais-controller-manager   ClusterIP   10.96.1.71   <none>        8443/TCP   
 16d
 
3. Add the endpoint for the Private AI Services controller pod to the Telegraf configuration in the Supervisor instance.
a. Run the following command and in the output verify that the Telegraf deployment pod is available and running and 
that the required ConfigMap resources are available.
kubectl -n vmware-system-monitoring get deployment,configmaps
Verify also that the telegraf-user-config ConfigMap is available. telegraf-user-config contains the 
user-managed configuration for Telegraf.
For example, the command might return the following output:
NAME                                                             READY   UP-TO-DATE  
  AVAILABLE   AGE
deployment.apps/telegraf-deploy                                  1/1     1           
  1           18d
deployment.apps/vmware-system-observability-controller-manager   1/1     1           
  1           18d
NAME                                    DATA   AGE
configmap/default-telegraf-config       3      18d
configmap/kube-rbac-proxy-config        1      18d
configmap/kube-root-ca.crt              1      18d
configmap/telegraf-config               3      18d
configmap/telegraf-user-config          3      18d
configmap/wcp-remote-endpoints-config   0      18d
b. View the telegraf-user-config ConfigMap in YAML format.
kubectl -n vmware-system-monitoring get configmap/telegraf-user-config -o yaml
c. Open the telegraf-user-config ConfigMap for editing.
kubectl edit cm/telegraf-user-config -n vmware-system-monitoring
d. Modify the ConfigMap in the following way.
pais-system-namespace represents the name of the svc-pais-<suffix> namespace containing the 
controller pod for the Private AI Services service.
apiVersion: v1
data: 
  telegraf-daemonset.conf: "" 
  telegraf-deployment.conf:  | 
    [inputs] 
      [[inputs.prometheus]] 
        alias = "pais_controller_metrics" 
        kubernetes_services = ["https://pais-controller-manager.<pais-system-
namespace>.svc.cluster.local:8443/metrics"] 
        bearer_token = "/run/secrets/kubernetes.io/serviceaccount/token" 
        insecure_skip_verify = true 
        name_prefix = "pais_controller_metrics_" 
        namepass = ["controller_runtime_active_workers", 
 "controller_runtime_max_concurrent_reconciles", "workqueue_depth", 
 "workqueue_longest_running_processor_seconds", "workqueue_unfinished_work_seconds"] 
  telegraf.conf: ""
VMware by Broadcom
155


---

VMware Private AI Foundation with NVIDIA 9.1
kind: ConfigMap
metadata: 
  labels: 
    name: telegraf 
  name: telegraf-user-config 
  namespace: vmware-system-monitoring
 
e. Restart the deployment to accommodate the changes in the telegraf-user-config ConfigMap.
kubectl -n vmware-system-monitoring rollout restart deployment telegraf-deploy
f.
Verify again that the Telegraf deployment pod is available and running and the required ConfigMap resources are 
available.
4. View the metrics in the VCF Operations UI.
a. On the Operate tab, go to the Inventory > Integrations section.
b. Locate the vSphere Supervisor > PAIS Supervisor Service > pais.vmware.com metrics.
The metrics for the controller pod of the Private AI Services instance are Model Endpoint Controller and
PAIS Configuration Controller.
VMware by Broadcom
156


---

VMware Private AI Foundation with NVIDIA 9.1
VMware by Broadcom
157


---

VMware Private AI Foundation with NVIDIA 9.1
VMware by Broadcom
158


---

VMware Private AI Foundation with NVIDIA 9.1
Deploying a Deep Learning VM in VMware Private AI 
Foundation with NVIDIA
VMware Private AI Foundation with NVIDIA supports provisioning pre-configured deep learning VMs that data scientists 
can use for AI development or MLOps engineers - for validation of ML models.
As a data scientist or MLOps engineer, you have the following options to start using a deep learning VM:
•
Deploy a deep learning VM by using a self-service catalog item in VCF Automation.
•
Request your DevOps engineer to deploy a deep learning VM in a namespace by using the kubectl command.
•
Request your cloud administrator to deploy a deep learning VM on a vSphere cluster to quickly explore the deep 
learning VM templates.
In This Chapter
•
About Deep Learning VM Images in VMware Private AI Foundation with NVIDIA
•
Deploy Deep Learning Virtual Machines by Using Self-Service Catalog Items in VCF Automation
•
Deploy a Deep Learning VM Directly on a vSphere Cluster in VMware Private AI Foundation with NVIDIA
•
Deploy a Deep Learning VM by Using the kubectl Command in VMware Private AI Foundation with NVIDIA
•
Monitoring the vGPU Performance of a Deep Learning VM
•
Customizing Deep Learning VM Deployment in VMware Private AI Foundation with NVIDIA
•
Troubleshooting Deep Learning VM Deployment in VMware Private AI Foundation with NVIDIA
About Deep Learning VM Images in VMware Private AI Foundation with 
NVIDIA
The deep learning virtual machine images delivered as part of VMware Private AI Foundation with NVIDIA are 
preconfigured with popular ML libraries, frameworks, and toolkits, and are optimized and validated by NVIDIA and 
VMware for GPU acceleration in a VCF environment.
As a data scientist, you can use the deep learning VMs provisioned from these images for AI prototyping, fine-tuning, 
validation, and inference.
The software stack for running AI applications on top of NVIDIA GPUs is validated in advance. As a result, you directly 
start AI development without spending time installing and validating the compatibility of operating systems, software 
libraries, ML frameworks, toolkits, and GPU drivers.
Running a Deep Learning VM on vGPU or GPU Passthrough
Starting with VCF 9.1, you can run a deep learning VM on one or more vGPUs or directly on a single GPU or multiple 
GPU devices using passthrough technology.
Using vGPUs has the following advantages and considerations:
•
You can share the GPU capacity among several deep learning VMs that do not need the power of a full GPU device.
In addition, this model is suitable when only a limited number of GPU devices are available to multiple teams.
•
You need an NVIDIA AI Enterprise license.
•
You can have pre-installed DL workloads from NVIDIA from the production branch which require an NVIDIA NGC API 
key. For example, the image URL of the Pytorch container on the May 2025 production branch could be nvcr.io/nvidia/
pytorch-pb25h1:25.03.03-py3.
Using GPU passthrough has the following advantages and considerations:
•
Your deep learning VM is allocated the full capacity of the GPU device on the underlying server which leads to 
improved performance.
VMware by Broadcom
159


---

VMware Private AI Foundation with NVIDIA 9.1
•
You do not need an NVIDIA AI Enterprise license.
•
You can have pre-installed DL workloads from NVIDIA from the feature branch instead of the production branch. For 
example, the image URL of the Pytorch container on the Production Branch May 2025 production branch could be
nvcr.io/nvidia/pytorch-pb25h1:25.03.03-py3 while the image URL of the Pytorch container on the feature branch could 
be nvcr.io/nvidia/pytorch:25.03-py3.
For information about feature branches and production branches of the NVIDIA NGC containers, see the NVIDIA AI 
Enterprise Release Notes.
•
You cannot migrate deep learning VMs across hosts by using vSphere vMotion.
Contact your VI administrator to make sure that the GPU configuration in your environment accommodates your 
requirements.
What Does a Deep Learning VM Image Contain?
The latest deep learning virtual machine image contains the following software. For information on the component 
versions in each deep learning VM image release, see VMware Deep Learning VM Image Release Notes.
Software Component Category
Software Component
Embedded
•
Canonical Ubuntu
•
NVIDIA Container Toolkit
•
Docker Community Engine
•
Miniforge, and PyTorch and TensorFlow Conda manifests.
•
VCF Consumption CLI with the pais plug-in for storing models in a model gallery 
pre-installed
See Storing ML Models in VMware Private AI Foundation with NVIDIA.
NVIDIA vGPU guest driver according to the version of the vGPU host driver
Note:
The NVIDIA data center guest driver for GPU passthrough is downloaded from 
the NVIDIA download portal or from a local URL specified in the OVF Properties 
of Deep Learning VMs.
CUDA Sample
You can use a deep learning VM with 
running CUDA samples to explore vector 
addition, gravitational n-body simulation, or 
other examples on a VM. See the CUDA 
Samples page in the NVIDIA NGC catalog.
Can be pre-installed automatically when 
you start the deep learning VM for the first 
time
Deep learning (DL) workloads
PyTorch.
You can use a deep learning VM with a 
PyTorch library to explore conversational 
AI, NLP, and other types of AI models, on 
a VM. See the PyTorch page in the NVIDIA 
NGC catalog.
You can use a ready JupyterLab instance 
with PyTorch installed and configured at ht 
tp://dl_vm_ip:8888.
VMware by Broadcom
160


---

VMware Private AI Foundation with NVIDIA 9.1
Software Component Category
Software Component
DCGM Exporter
You can use a deep learning VM with 
a Data Center GPU Manager (DCGM) 
exporter to monitor the health of and get 
metrics from GPUs used by a DL workload, 
using NVIDIA DCGM, Prometheus, and 
Grafana. See the DCGM Exporter page in 
the NVIDIA NGC catalog.
In a deep learning VM, you run the DCGM 
Exporter container together with a DL 
workload that performs AI operations. After 
the deep learning VM is started, DCGM 
Exporter is ready to collect vGPU metrics 
and export the data to another application 
for further monitoring and visualization.
For information how to use DGCM Exporter 
to visualize metrics with Prometheus and 
Grafana, see DCGM Exporter Installation 
and Initial Setup.
Triton Inference Server
You can use a deep learning VM with a 
Triton Inference Server for loading a model 
repository and receive inference requests. 
See the Triton Inference Server page in the 
NVIDIA NGC catalog.
For information how to use Triton Inference 
Server for inference requests for AI models, 
see Triton Inference Server Installation and 
Initial Setup.
Deploying a Deep Learning VM
As a data scientist or MLOps engineer, you can deploy a deep learning VM on your own by using catalog items in VCF 
Automation. Otherwise, a VI administrator or DevOps engineer deploys such a VM for you.
In a disconnected environment, your administrators must make sure that the following conditions are in place.
•
Local URL to download the vGPU or passthrough guest GPU drivers from. See OVF Properties of Deep Learning 
VMs.
•
For using vGPUs, NVIDIA DLS instance deployed and configured, and a client configuration token generated. See
NVIDIA License System User Guide.
•
All required images and models downloaded to a local Harbor registry by using the resource mirroring utility. See
Upload the Components for Deep Learning VMs and VKS Clusters with GPU to a Disconnected Environment.
Deploy Deep Learning Virtual Machines by Using Self-Service Catalog 
Items in VCF Automation
As a data scientist or DevOps engineer, you can deploy a deep learning VM from VCF Automation by using the AI 
workstation self-service catalog item.
•
Verify that your organization administrator has configured VMware Private AI Foundation with NVIDIA catalog items for 
your project.
•
Verify that you have permissions to request AI catalog items.
When you request a deep learning VM in the VCF Automation catalog, you provision a GPU-enabled deep learning VM 
that can be configured with the desired vCPU, vGPU, Memory, and AI/ML NGC containers from NVIDIA. The vGPU guest 
driver and the specified deep learning workload are installed the first time you start the deep learning VM.
VMware by Broadcom
161


---

VMware Private AI Foundation with NVIDIA 9.1
Deploy a Deep Learning VM by Using a Self-Service Catalog Item in VCF 
Automation
As a data scientist, you can deploy a single GPU software-defined development environment from the self-service VCF 
Automation catalog.
You can customize the GPU-enabled virtual machine with machine parameters to model development requirements, pre-
install AI/ML frameworks to meet training and inference requirements, and specify the AI/ML packages from the NVIDIA
NGC registry via a portal access key.
1. In VCF Automation, select Build & Deploy > Catalog, locate the AI Workstation card and click Request.
2. Select a project.
3. Enter a name and description for your deployment.
4. Configure the AI workstation parameters.
Setting
Sample value
VM class
A100 Small - 1 vGPU (16 GB), 8 CPUs and 16 GB 
Memory
Data disk size
32 GB
User password
Enter a password for the default user. You might be prompted to 
reset your password when you first log in.
SSH public key
This setting is optional.
An SSH public key is added to the ~/.ssh/authorized_ke 
ys file to allow the user to log in using public key authentication. 
For example, ssh-rsa AAAAB3... foouser@computer.
5. Select a software bundle to install on your workstation.
Supported deep learning workloads include PyTorch and CUDA samples. For morе information, see Customizing 
Deep Learning VM Deployment in VMware Private AI Foundation with NVIDIA.
6. Optional: Enter a custom cloud-init that you want to install in addition to the cloud-init defined for the software bundle.
VCF Automation merges the cloud-init from the software bundle and the custom cloud-init.
7. Provide your NVIDIA NGC Portal access key.
8. Optional: If you are installing the PyTorch NGC software bundle, activate JupyterLab authentication.
9. Click Submit.
Open your deployed catalog item on the Instances page. The deployment Overview tab contains a summary of the 
software that was installed, along with instructions on how to access the application, services, and the workstation VM.
Deploy a Deep Learning VM with NVIDIA Triton Inference Server by Using a Self-
Service Catalog Item in VCF Automation
As a data scientist, you can deploy a GPU-enabled deep learning VM with NVIDIA Triton Inference Server from the self-
service VCF Automation catalog.
The deployed workstation includes Ubuntu 24.04, an NVIDIA vGPU driver, Docker Engine, NVIDIA Container Toolkit, and
NVIDIA Triton Inference Server.
VMware by Broadcom
162


---

VMware Private AI Foundation with NVIDIA 9.1
1. In VCF Automation, select Build & Deploy > Catalog, locate the Triton Inferencing Server card and click Request.
2. Select a project.
3. Enter a name and description for your deployment.
4. Configure the AI workstation parameters.
Setting
Sample value
VM class
A100 Small - 1 vGPU (16 GB), 8 CPUs and 16 GB 
Memory
VM classes with Unified Virtual Memory (UVM) support are 
required for running Triton Inferencing Server.
Data disk size
32 GB
User password
Enter a password for the default user. You might be prompted to 
reset your password when you first log in.
SSH public key
This setting is optional.
An SSH public key is added to the ~/.ssh/authorized_ke 
ys file to allow the user to log in using public key authentication. 
For example, ssh-rsa AAAAB3... foouser@computer.
5. Optional: Enter a custom cloud-init that you want to install in addition to the cloud-init defined for the software bundle.
VMware Aria Automation merges the cloud-init from the software bundle and the custom cloud-init.
6. Provide your NGC Portal access key.
7. Click Submit.
Deploy a Deep Learning VM Directly on a vSphere Cluster in VMware 
Private AI Foundation with NVIDIA
As a VI administrator, to quickly give data scientists the opportunity to test the deep learning VM templates in VMware 
Private AI Foundation with NVIDIA, you can deploy a deep learning VM directly on a vSphere cluster in the GPU-
accelerated workload domain by using the vSphere Client. You can deploy a deep learning VM directly on a vSphere 
cluster without having to set up VCF Automation or vSphere Supervisor.
•
Verify that VMware Private AI Foundation with NVIDIA is deployed and configured. See Preparing VCF for Private AI 
Workload Deployment.
•
In a disconnected environment, contact your Harbor administrator for the Harbor project URL and credentials.
For information about deep learning VM images in VMware Private AI Foundation with NVIDIA, see About Deep Learning 
VM Images in VMware Private AI Foundation with NVIDIA.
Deploying a deep learning VM with NVIDIA RAG requires a vector database. You can use a PostgreSQL database with 
pgvector in VMware Data Services Manager. For information about deploying such a database and integrating it in a deep 
learning VM, see .
1. Generate the software download token on the Broadcom Support Portal and save it on your machine.
See Broadcom knowledge base article 390098.
2. Log in to the vCenter instance for the GPU-accelerated workload domain.
3. From the vSphere Client home menu, select Content Libraries.
4. Create a content library with deep learning VM images on the vCenter instance for the workload domain.
Deep learning VM images in VMware Private AI Foundation with NVIDIA are distributed in a shared content library 
published by VMware.
VMware by Broadcom
163


---

VMware Private AI Foundation with NVIDIA 9.1
a) On the Content Libraries page, click Create.
b) For a connected environment, create a subscribed content library that is connected to https://
dl.broadcom.com/<download_token>/PROD/COMP/DLVM/9.1.x.x/lib.json.
Authentication is not required.
c) For a disconnected environment, download the deep learning VM .ova image from the VMware Private AI area on 
the Broadcom Support Portal and import it in to a local content library.
5. Deploy the deep learning VM on the cluster using an image in the content library.
a) Navigate to the deep learning VM image in the content library.
b) Right-click an OVF template and select New VM from This Template.
c) On the Select name and folder page of wizard that appears, enter a name and select a VM folder, select
Customize this virtual machine's hardware, and click Next.
d) Select a GPU-enabled cluster in the workload domain, select if the virtual machine must be powered-on after 
deployment is complete, and click Next.
e) Follow the wizard to select a datastore and a network on the distributed switch for the cluster.
f)
On the Customize template page, enter the custom OVF properties that are required for setting up the AI 
functionality.
•
In a disconnected environment, you must provide the URL for the vGPU driver or GPU passthrough driver.
•
If you want to run a predefined DL workload after the virtual machine is up running, set the user-data
property with the cloud-init script in base 64 format together with access information.
For full details on the OVF properties for deep learning VMs, see OVF Properties of Deep Learning VMs.
g) For running the VM on vGPU, on the Customize hardware page, assign an NVIDIA vGPU device to the virtual 
machine as a New PCI Device .
h) For running the VM on GPU passthrough, on the Advanced Parameters tab, set the following parameters.
Parameter
Value
pciPassthru.use64bitMMIO
true
pciPassthru.use64bitMMIOSizeGB
Calculate the value in the following way:
1.
Count the number of passthrough GPU devices that you intend to require in the 
VM class.
2.
Multiply that number by the frame buffer memory of the individual GPU device.
3.
If the total GPU frame buffer memory is equal to a power-of-two number, set the 
parameter to the next power-of-two number.
4.
If the total GPU frame buffer memory is between two powers-of-two numbers, 
round up to the next power-of-two number and then again to the next power-of-two 
number.
For example, for a VM class with two A100 GPU devices in passthrough mode, the 
total GPU frame buffer memory is
2 * 40 = 80 GB. You round it up to 128 GB and then again to 256 GB. You 
set pciPassthru.use64bitMMIOSizeGB to 256.
i)
For a deep learning VM that is using Triton Inference Server with the TensorRT backend, in the Advanced 
Parameters tab of the virtual machine settings, set the pciPassthru<vgpu-id>.cfg.enable_uvm parameter 
to 1.
j)
On the Customize Hardware tab, click Next.
k) Review the deployment specification and click Finish.
The vGPU guest driver or the data center GPU driver and the specified deep learning workload are installed the first time 
you start the deep learning VM.
You can examine the logs or open the JupyterLab instance that comes with some of the images. You can share access 
details with data scientists in your organization. See Deep Learning Workloads in VMware Private AI Foundation with 
NVIDIA.
•
Connect to the deep learning VM over SSH and verify that all components are installed and running as expected.
VMware by Broadcom
164


---

VMware Private AI Foundation with NVIDIA 9.1
•
Send access details to your data scientists.
•
If the deep learning VM has PyTorch pre-installed and JupyterLab notebook authentication is not deactivated, to 
get the authentication token, examine the /var/log/dl.log file or run the vmtoolsd --cmd "info-get 
guestinfo.dlworkload.jupyterlab.token" command in the virtual machine console.
•
For DL workloads other than PyTorch and TensorFlow, if the dlvmconsole-enable OVF property is set to true in the 
deployment YAML file, use the deep learning VM console in the JupyterLab notebook.
•
View vGPU performance of AI workloads running on the virtual machine. See Monitoring the vGPU Performance of a 
Deep Learning VM.
•
In a disconnected environment, you can run the resource mirroring utility from Broadcom knowledge base article 
388470 to upload required container images to a project in a local Harbor registry.
Deploy a Deep Learning VM by Using the kubectl Command in VMware 
Private AI Foundation with NVIDIA
The VM service in the Supervisor of the workload domain cluster enables DevOps engineers to deploy and run deep 
learning VMs by using the Kubernetes API.
•
Verify with the VI and organization administrator that the VMware Private AI Foundation with NVIDIA is deployed and 
configured. See Preparing VCF for Private AI Workload Deployment.
•
Verify that you can access the GPU-accelerated namespace in VCF Automation or that you have the namespace 
details from your organization administrator.
•
In a disconnected environment, contact your Harbor administrator for the URL and credentials of the Harbor project.
•
In a disconnected environment, contact your VI administrator to obtain the client configuration token from the local 
NVIDIA DLS instance and the URL where the vGPU guest drivers are available.
•
In a proxy-based environment, contact your administrator for the proxy configuration of your environment.
•
You have the certificate of your VCF Automation instance saved to your machine, for example, as a
vcf_automation_certificate.pem file in your current working folder.
If your machine has OpenSSL installed, you can use the following command:
openssl s_client -connect <vcf_automation_FQDN>:443 -showcerts </dev/null 2>/dev/null | openssl x509 -
outform PEM > vcf_automation_certificate.pem
As a DevOps engineer, you use kubectl to deploy a deep learning VM on the namespace configured by the organization 
administrator VCF Automation.
For information about deep learning VM images in VMware Private AI Foundation with NVIDIA, see About Deep Learning 
VM Images in VMware Private AI Foundation with NVIDIA.
Deploying a deep learning VM with NVIDIA RAG requires a vector database. You can use a PostgreSQL database with 
pgvector in VMware Data Services Manager. For information about deploying such a database and integrating it in a deep 
learning VM, see .
1. On a machine running the VCF Consumption CLI and kubectl, create a context for the namespace in VCF 
Automation where you are deploying the deep learning VM.
vcf context create vcf_automation_context_name --endpoint vcf_automation_endpoint --api-token 
   vcf_automation_api_token --ca-certificate vcf_automation_certificate.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces you have access to are created. The 
additional contexts are available as vcf_automation_context:namespace:project.
2. Switch to the context for the target namespace in VCF Automation.
vcf context use vcf_automation_context_name:namespace_name:project_name
3. Get the names of the custom VM classes in the namespace and identify those with vGPU profile.
kubectl get virtualmachineclass
If you want to get the VM classes with vGPUs, run the following command.
VMware by Broadcom
165


---

VMware Private AI Foundation with NVIDIA 9.1
Linux
kubectl get virtualmachineclass -o json | jq -r '.items[] 
 | select(.spec.hardware.devices.vgpuDevices != null and 
 (.spec.hardware.devices.vgpuDevices | length) > 0) | .metadata.name'
Windows in PowerShell
$jqFilter = ".items[] | select(.spec.hardware.devices.vgpuDevices != null and 
 (.spec.hardware.devices.vgpuDevices | length) > 0) | .metadata.name"
kubectl get virtualmachineclass -o json | jq -r $jqFilter
4. Get the storage class for the namespace from the output of the following command.
kubectl describe namespaces namespace_name
The storage class name is available in the storage_class_name.storageclass.storage.k8s.io/
requests.storage resource name in the namespace_name-storagequota resource quota.
5. View the deep learning VM images available in your namespace by running the following command and examining its 
output.
kubectl get virtualmachineimages
6. Prepare the YAML file for the deep learning VM.
For example, you can create a YAML specification example-dl-vm.yaml for an example deep learning VM running 
PyTorch in an example-dl-vm-namespace namespace.
a) Create a VM deployment specification for the namespace based on the v1alpha5 version of the VM Operator API,
Include the following parameters:
•
spec.className. The vGPU-based VM class for the VM in the namespace.
•
spec.imageName. The image from the deep learning VM content library you want to deploy.
•
spec.powerState. Optional. The target power state of the VM.
•
spec.storageClass. The storage class to be used for storage of the persistent volumes.
•
spec.bootstrap.vAppConfig.rawProperties. A Secret resource with the OVF properties of the VM.
The OVF properties are in a Secret object called example-dl-vm-config.
apiVersion: vmoperator.vmware.com/v1alpha5
kind: VirtualMachine
metadata: 
  name: example-dl-vm 
  namespace: example-dl-vm-namespace 
  labels: 
    app: example-dl-vm
spec: 
  className: gpu-a100 
  imageName: vmi-xxxxxxxxxxxxx 
  powerState: PoweredOn 
  storageClass: tanzu-storage-policy 
  bootstrap: 
    linuxPrep: {} 
    vAppConfig: 
      rawProperties: example-dl-vm-config
b) Add the Secret definition with the OVF properties of the deep learning VM to example-dl-vm.yaml.
In a connected environment, you must include user-data containing the cloud-init script and stringData
entries for the other required OVF properties as follows. For information on available OVF properties, see OVF 
Properties of Deep Learning VMs.
•
password. Password of the default vmware user account on the VM.
•
For using vGPU, vgpu-license and ngc-api-key OVF properties.
•
For using GPU passthrough
VMware by Broadcom
166


---

VMware Private AI Foundation with NVIDIA 9.1
– vgpu-license and ngc-api-key OVF properties with empty values
– gpu-driver-url with empty value if you will use the default version of the NVIDIA data center GPU 
driver or with the download URL to the .run file for the required driver version according to https://
docs.nvidia.com/datacenter/tesla/drivers/releases.json.
For information about the default version of the data center GPU driver that is downloaded into the deep learning 
VM, see the VMware Deep Learning VM Image Release Notes.
Note:
user-data must be in base64 format. In the example, user-data contains the base64 encoded value of the 
cloud-init code for PyTorch available in the sample script package in Broadcom knowledge base article 388813.
In a connected environment, use the following example YAML code. If you are using a proxy-based environment, 
add the proxy settings to the cloud-init script in the example. See Configure a Deep Learning VM with a Proxy 
Server.
For using vGPU
apiVersion: v1
kind: Secret
metadata: 
  name: example-dl-vm-config 
  namespace: example-dl-vm-namespace
stringData: 
  user-data: 
 I2Nsb3VkLWNvbmZpZwp3cml0ZV9maWxlczoKLSBwYXRoOiAvb3B0L2Rsdm0vZGxfYXBwLnNoCiAgcGVybWlzc2lvbn
+JjIKICAgICAgdm10b29sc2QgLS1jbWQgImluZm8tc2V0IGd1ZXN0aW5mby52bXNlcnZpY2UuYm9vdHN0cmFwLmNvbm
+L2Rldi9udWxsCiAgICAgICAgaWYgW1sgLW4gIiR7VkVSU0lPTjotfSIgXV07IHRoZW4KICAgICAgICAgICAgZWNoby
+PiAvb3B0L2Rsdm0vZW52aXJvbm1lbnQgPDwtRU9GCiAgaHR0cF9wcm94eT0ke2h0dHBQcm94eV91cmx9CiAgaHR0cH
+PiAvZXRjL3N5c3RlbWQvc3lzdGVtL3JjLWxvY2FsLnNlcnZpY2UuZC9lbnZpcm9ubWVudC5jb25mIDw8LUVPRgogIF
+IC9ldGMvc3lzdGVtZC9zeXN0ZW0vZG9ja2VyLnNlcnZpY2UuZC9wcm94eS5jb25mIDw8LUVPRiAgCiAgW1NlcnZpY2
PD1odHRwOi8vKVteOl0rIikKICAgIFByb3h5U2VydmVyUG9ydD0kKGVjaG8gIiR7aHR0cFByb3h5X3VybH0iIHwgZ3J
PD06KVswLTldKyIpCiAgRU9GCiAgICB9CiAgZmknCi0gfCAKICBjbG91ZC1pbml0LXBlciBvbmNlIGFkZC1jYS1jZXJ
  vgpu-license: NVIDIA-client-configuration-token
  ngc-api-key: API-key-from-NVIDIA-NGC
  password: password-for-vmware-user
For using GPU 
passthrough
apiVersion: v1
kind: Secret
metadata: 
  name: example-dl-vm-config 
  namespace: example-dl-vm-namespace
stringData: 
  user-data: 
 I2Nsb3VkLWNvbmZpZwp3cml0ZV9maWxlczoKLSBwYXRoOiAvb3B0L2Rsdm0vZGxfYXBwLnNoCiAgcGVybWlzc2lvbn
+JjIKICAgICAgdm10b29sc2QgLS1jbWQgImluZm8tc2V0IGd1ZXN0aW5mby52bXNlcnZpY2UuYm9vdHN0cmFwLmNvbm
+L2Rldi9udWxsCiAgICAgICAgaWYgW1sgLW4gIiR7VkVSU0lPTjotfSIgXV07IHRoZW4KICAgICAgICAgICAgZWNoby
+PiAvb3B0L2Rsdm0vZW52aXJvbm1lbnQgPDwtRU9GCiAgaHR0cF9wcm94eT0ke2h0dHBQcm94eV91cmx9CiAgaHR0cH
+PiAvZXRjL3N5c3RlbWQvc3lzdGVtL3JjLWxvY2FsLnNlcnZpY2UuZC9lbnZpcm9ubWVudC5jb25mIDw8LUVPRgogIF
+IC9ldGMvc3lzdGVtZC9zeXN0ZW0vZG9ja2VyLnNlcnZpY2UuZC9wcm94eS5jb25mIDw8LUVPRiAgCiAgW1NlcnZpY2
PD1odHRwOi8vKVteOl0rIikKICAgIFByb3h5U2VydmVyUG9ydD0kKGVjaG8gIiR7aHR0cFByb3h5X3VybH0iIHwgZ3J
PD06KVswLTldKyIpCiAgRU9GCiAgICB9CiAgZmknCi0gfCAKICBjbG91ZC1pbml0LXBlciBvbmNlIGFkZC1jYS1jZXJ
  vgpu-license:  
  ngc-api-key:  
  gpu-driver-url: 
  password: password-for-vmware-user
In a disconnected environment, use the following example YAML code providing the following details as OVF 
properties in addition to the OVF properties for a connected environment.
•
registry-uri. URL of the Harbor project containing the required DL workload and containers for the deep 
learning VM.
VMware by Broadcom
167


---

VMware Private AI Foundation with NVIDIA 9.1
•
registry-user and registry-passwd. Your Harbor credentials
•
For using vGPUs
– vgpu-url. A local vGPU driver download URL
– vgpu-license. The client configuration token from the local NVIDIA DLS instance
•
For using GPU passthrough
– gpu-driver-url. A local download URL for the NVIDIA data center GPU driver.
For using vGPU
apiVersion: v1
kind: Secret
metadata: 
  name: example-dl-vm-config 
  namespace: example-dl-vm-namespace
stringData: 
  user-data: 
 I2Nsb3VkLWNvbmZpZwp3cml0ZV9maWxlczoKLSBwYXRoOiAvb3B0L2Rsdm0vZGxfYXBwLnNoCiAgcGVybWlzc2lvbn
+JjIKICAgICAgdm10b29sc2QgLS1jbWQgImluZm8tc2V0IGd1ZXN0aW5mby52bXNlcnZpY2UuYm9vdHN0cmFwLmNvbm
+L2Rldi9udWxsCiAgICAgICAgaWYgW1sgLW4gIiR7VkVSU0lPTjotfSIgXV07IHRoZW4KICAgICAgICAgICAgZWNoby
+PiAvb3B0L2Rsdm0vZW52aXJvbm1lbnQgPDwtRU9GCiAgaHR0cF9wcm94eT0ke2h0dHBQcm94eV91cmx9CiAgaHR0cH
+PiAvZXRjL3N5c3RlbWQvc3lzdGVtL3JjLWxvY2FsLnNlcnZpY2UuZC9lbnZpcm9ubWVudC5jb25mIDw8LUVPRgogIF
+IC9ldGMvc3lzdGVtZC9zeXN0ZW0vZG9ja2VyLnNlcnZpY2UuZC9wcm94eS5jb25mIDw8LUVPRiAgCiAgW1NlcnZpY2
PD1odHRwOi8vKVteOl0rIikKICAgIFByb3h5U2VydmVyUG9ydD0kKGVjaG8gIiR7aHR0cFByb3h5X3VybH0iIHwgZ3J
PD06KVswLTldKyIpCiAgRU9GCiAgICB9CiAgZmknCi0gfCAKICBjbG91ZC1pbml0LXBlciBvbmNlIGFkZC1jYS1jZXJ
  vgpu-license: NVIDIA-client-configuration-token
  vgpu-url: Local download URL for the vGPU guest drivers
  password: password-for-vmware-user
  registry-uri: Harbor project URL
  registry-user: Harbor project user name if basic authentication is required
  registry-passwd: Harbor project password if basic authentication is required
For using GPU 
passthrough
apiVersion: v1
kind: Secret
metadata: 
  name: example-dl-vm-config 
  namespace: example-dl-vm-namespace
stringData: 
  user-data: 
 I2Nsb3VkLWNvbmZpZwp3cml0ZV9maWxlczoKLSBwYXRoOiAvb3B0L2Rsdm0vZGxfYXBwLnNoCiAgcGVybWlzc2lvbn
+JjIKICAgICAgdm10b29sc2QgLS1jbWQgImluZm8tc2V0IGd1ZXN0aW5mby52bXNlcnZpY2UuYm9vdHN0cmFwLmNvbm
+L2Rldi9udWxsCiAgICAgICAgaWYgW1sgLW4gIiR7VkVSU0lPTjotfSIgXV07IHRoZW4KICAgICAgICAgICAgZWNoby
+PiAvb3B0L2Rsdm0vZW52aXJvbm1lbnQgPDwtRU9GCiAgaHR0cF9wcm94eT0ke2h0dHBQcm94eV91cmx9CiAgaHR0cH
+PiAvZXRjL3N5c3RlbWQvc3lzdGVtL3JjLWxvY2FsLnNlcnZpY2UuZC9lbnZpcm9ubWVudC5jb25mIDw8LUVPRgogIF
+IC9ldGMvc3lzdGVtZC9zeXN0ZW0vZG9ja2VyLnNlcnZpY2UuZC9wcm94eS5jb25mIDw8LUVPRiAgCiAgW1NlcnZpY2
PD1odHRwOi8vKVteOl0rIikKICAgIFByb3h5U2VydmVyUG9ydD0kKGVjaG8gIiR7aHR0cFByb3h5X3VybH0iIHwgZ3J
PD06KVswLTldKyIpCiAgRU9GCiAgICB9CiAgZmknCi0gfCAKICBjbG91ZC1pbml0LXBlciBvbmNlIGFkZC1jYS1jZXJ
  gpu-driver-url: Local download URL for the NVIDIA data center GPU driver
  vgpu-license:  
  vgpu-url: 
  password: password-for-vmware-user
  registry-uri: Harbor project URL
  registry-user: Harbor project user name if basic authentication is required
  registry-passwd: Harbor project password if basic authentication is required
c) Add a VirtualMachineService definition for the network service required for the deep learning VM to example-dl-
vm.yaml.
Use the following example YAML code to provide access to the VM using an external load balancer. You set up two 
types of connections:
•
General access to the virtual machine over SSH on port 22 and HTTP using a Web browser on port 80
VMware by Broadcom
168


---

VMware Private AI Foundation with NVIDIA 9.1
•
Access to the DL workload components. For example:
– JupyterLab notebook on port 8888 for the notebook that comes with the PyTorch module.
– Metrics from the pre-installed DCGM exporter available on port 9400
apiVersion: vmoperator.vmware.com/v1alpha5
kind: VirtualMachineService
metadata: 
  name: example-dl-vm-service 
  namespace: example-dl-vm-namespace
spec: 
  ports: 
  - name: ssh 
    port: 22 
    protocol: TCP 
    targetPort: 22 
  - name: web 
    port: 80 
    protocol: TCP 
    targetPort: 80
# Ports specific to the PyTorch DL workload.
# You can configure additional ports according to your requirements. 
  - name: pytorch 
    port: 8888 
    protocol: TCP 
    targetPort: 8888 
  - name: dcgm 
    port: 9400 
    protocol: TCP 
    targetPort: 9400 
  selector: 
    app: example-dl-vm 
  type: LoadBalancer
7. Deploy the deep learning VM.
kubectl apply -f example-dl-vm.yaml
8. Verify that the VM has been created by running these commands.
kubectl get vm -n example-dl-vm-namespace
kubectl describe virtualmachine example-dl-vm
9. Ping the IP address of the virtual machine that is assigned by the requested networking service.
To get the public address and the ports for access to the deep learning VM, get the details about the load balancer 
service that has been created.
kubectl get services
NAME   TYPE           CLUSTER-IP              EXTERNAL-IP          PORT(S)                       AGE
example-dl-vm   LoadBalancer   <internal-ip-address><public-ip-address>22:30473/TCP,8888:32180/TCP   9m40s
 
The vGPU guest driver or the data center GPU driver and the specified DL workload are installed the first time you start 
the deep learning VM.
•
You can examine the logs or open the JupyterLab notebook that comes with some of the images. See Deep Learning 
Workloads in VMware Private AI Foundation with NVIDIA.
•
Send access details to your data scientists.
•
If the dlvmconsole-enable OVF property is set to true in the deployment YAML file, use the deep learning VM 
console in the JupyterLab notebook.
VMware by Broadcom
169


---

VMware Private AI Foundation with NVIDIA 9.1
•
If the deep learning VM has PyTorch pre-installed and JupyterLab notebook authentication is not deactivated, to 
get the authentication token, examine the /var/log/dl.log file or run the vmtoolsd --cmd "info-get 
guestinfo.dlworkload.jupyterlab.token" command in the virtual machine console.
•
View vGPU performance of AI workloads running on the virtual machine. See Monitoring the vGPU Performance of a 
Deep Learning VM.
•
In a disconnected environment, you can run the resource mirroring utility from Broadcom knowledge base article 
388470 to upload required container images to a project in a local Harbor registry.
Monitoring the vGPU Performance of a Deep Learning VM
You can use the NVIDIA DCGM Exporter that is pre-installed in each deep learning VM deployment to monitor GPU 
metrics and understand workload behavior.
By default, the DCGM Exporter instance in the deep learning VM exposes metrics on public interfaces. You can then 
access the metrics in a Web browser at <dl_vm_ip>:9400/metrics, and you can collect them in Prometheus and 
visualize them in Grafana.
Run a DL Workload on the Deep Leaning VM
If not pre-installed in the deep learning VM, run the DL workload you want to collect vGPU metrics for and export the data 
to another application for further monitoring and visualization.
1. Log in to the deep learning VM as vmware over SSH.
2. Run the container for the DL workload, pulling it from the NVIDIA NGC catalog or from a local container registry.
For example, to run the following command to run the nvcr.io/nvidia/pytorch-pb25h1:25.03.03-py3 image 
from NVIDIA NGC:
docker run -d --gpus all -p 8888:8888 nvcr.io/nvidia/pytorch-pb25h1:25.03.03-py3 /usr/local/
bin/jupyter lab --allow-root --ip=* --port=8888 --no-browser --NotebookApp.token="$TOKEN" --
NotebookApp.allow_origin="*" --notebook-dir=/workspace
3. Start using the DL workload for AI development.
Install Prometheus and Grafana
You can visualize and monitor the vGPU metrics from the DCGM Exporter virtual machine on a virtual machine running 
Prometheus and Grafana.
1. Create a visualization VM with Docker Community Engine installed.
2. Connect to the VM over SSH and create a YAML file for Prometheus.
$ cat > prometheus.yml << EOF
global: 
  scrape_interval: 15s 
  external_labels: 
    monitor: 'codelab-monitor'
scrape_configs: 
  - job_name: 'dcgm' 
    scrape_interval: 5s 
    metrics_path: /metrics 
    static_configs: 
      - targets: [dl_vm_with_dcgm_exporter_ip:9400']
EOF
3. Create a data path.
$ mkdir grafana_data prometheus_data && chmod 777 grafana_data prometheus_data
4. Create a Docker compose file to install Prometheus and Grafana.
$ cat > compose.yaml << EOF
services: 
  prometheus: 
    image: prom/prometheus:v2.47.2 
    container_name: "prometheus0" 
VMware by Broadcom
170


---

VMware Private AI Foundation with NVIDIA 9.1
    restart: always 
    ports: 
      - "9090:9090" 
    volumes: 
      - "./prometheus.yml:/etc/prometheus/prometheus.yml" 
      - "./prometheus_data:/prometheus" 
  grafana: 
    image: grafana/grafana:10.2.0-ubuntu 
    container_name: "grafana0" 
    ports: 
      - "3000:3000" 
    restart: always 
    volumes: 
      - "./grafana_data:/var/lib/grafana"
EOF
5. Start the Prometheus and Grafana containers.
$ sudo docker compose up -d         
View vGPU Metrics in Prometheus
You can access Prometheus at http://visualization-vm-ip:9090. You can view the following vGPU information 
in the Prometheus UI:
Information
UI Section
Raw vGPU metrics from the deep learning VM
Status > Target
To view the raw vGPU metrics from the deep learning VM, click 
the endpoint entry.
Graph expressions
1.
On the main navigation bar, click the Graph tab.
2.
Enter an expression and click Execute
For more information on using Prometheus, see the Prometheus documentation.
Visualize Metrics in Grafana
Set Prometheus as a data source for Grafana and visualize the vGPU metrics from the deep learning VM in a dashboard.
1. Access Grafana at http://visualization-vm-ip:3000 by using the default user name admin and password
admin.
2. Add Prometheus as the first data source, connecting to visualization-vm-ip on port 9090.
3. Create a dashboard with the vGPU metrics.
For more information on configuring a dashboard using a Prometheus data source, see the Grafana documentation.
Customizing Deep Learning VM Deployment in VMware Private AI 
Foundation with NVIDIA
When you deploy a deep learning VM in vSphere Supervisor by using kubectl or directly on a vSphere cluster, you must 
fill in custom VM properties.
For information about deep learning VM images in VMware Private AI Foundation with NVIDIA, see About Deep Learning 
VM Images in VMware Private AI Foundation with NVIDIA.
In This Chapter
•
OVF Properties of Deep Learning VMs
•
Deep Learning Workloads in VMware Private AI Foundation with NVIDIA
•
Assign a Static IP Address to a Deep Learning VM in VMware Private AI Foundation with NVIDIA
VMware by Broadcom
171


---

VMware Private AI Foundation with NVIDIA 9.1
•
Configure a Deep Learning VM with a Proxy Server
OVF Properties of Deep Learning VMs
When you deploy a deep learning VM, you must fill in custom VM properties to automate the configuration of the Linux 
operating system, the deployment of the vGPU guest driver, and the deployment and configuration of NGC containers for 
the DL workloads.
The latest deep learning VM image has the following OVF properties:
Category
Parameter
Label in the vSphere Client
Description
instance-id
Instance ID
Required. A unique instance ID 
for the VM instance.
An instance ID uniquely 
identifies an instance. When 
an instance ID changes, cloud-
init treats the instance as a new 
instance and runs the cloud-init 
process to again.
hostname
Hostname
Required. The host name of the 
appliance.
seedfrom
URL to seed instance data from
Optional. An URL to pull 
the value for the user-data 
parameter and metadata from.
Base OS Properties
public-keys
SSH public key
If provided, the instance 
populates the default user's 
SSH authorized_keys with 
this value.
VMware by Broadcom
172


---

VMware Private AI Foundation with NVIDIA 9.1
Category
Parameter
Label in the vSphere Client
Description
user-data
Encoded user-data
A set of scripts or other 
metadata that is inserted into 
the VM at provisioning time.
This property is the actual 
contents of the cloud-ini 
t script. This value must be 
base64 encoded.
•
You can use the cloud- 
init script to specify the 
DL workload container you 
want to deploy, such as 
PyTorch. See Deep Learning 
Workloads in VMware 
Private AI Foundation with 
NVIDIA.
•
You can modify the clo 
ud-init script to set a 
static IP address to a virtual 
machine that is deployed 
directly on a vSphere cluster. 
See Assign a Static IP 
Address to a Deep Learning 
VM in VMware Private AI 
Foundation with NVIDIA.
•
You can modify the cloud 
-init script to configure 
a proxy connection. See
Configure a Deep Learning 
VM with a Proxy Server.
You can review the cloud-in 
it examples that are available 
in the sample script package 
in Broadcom knowledge base 
article 388813.
password
Default user's password
Required. The password for the 
default vmware user account.
vgpu-license
vGPU license
Required. The NVIDIA vGPU 
client configuration token. The 
token is saved in the /etc/nv 
idia/ClientConfigToke 
n/client_configuratio 
n_token.tok file.
ngc-api-key
NGC API key
Required in a connected 
environment. The API key you 
downloaded from the NVIDIA 
NGC. The key is required for 
vGPU guest driver installation.
For VCF 9.0, provide a legacy 
API key. For VCF 9.0.1, provide 
a personal, service, or legacy 
API key.
vGPU Driver Installation
vgpu-host-driver-version
vGPU host driver version 
(Optional)
Optional. Install directly this 
version of the vGPU guest 
driver.
VMware by Broadcom
173


---

VMware Private AI Foundation with NVIDIA 9.1
Category
Parameter
Label in the vSphere Client
Description
vgpu-url
Custom URL for vGPU driver
Required in a disconnected 
environment. The URL to 
download the vGPU guest driver 
from. For information on the 
required configuration of the 
local Web server, see Preparing 
VCF for Private AI Workload 
Deployment.
Passthrough GPU Driver 
Installation
gpu-driver-url
Custom URL for Passthrough 
GPU Driver
Required in a disconnected 
environment or if you want to 
use a version of the NVIDIA 
data center GPU driver that is 
different from the default one. 
The URL to download the .run
file of the driver. For information 
on the required configuration 
of the local Web server, see
Preparing VCF for Private AI 
Workload Deployment.
registry-uri
Registry URI
Required in a disconnected 
environment or if you plan to 
use a private container registry 
to avoid downloading images 
from the Internet. The URI of a 
private container registry with 
the deep learning workload 
container images.
Required if you are referring to a 
private registry in user-data
or image-oneliner.
registry-user
Registry username
Required if you are using a 
private container registry that 
requires basic authentication.
registry-passwd
Registry password
Required if you are using a 
private container registry that 
requires basic authentication.
registry-2-uri
Secondary registry URI
Required if you are using a 
second private container registry 
that is based on Docker and 
requires basic authentication.
For example, when deploying 
a deep learning VM with the 
NVIDIA RAG DL workload pre-
installed, a pgvector image is 
downloaded from Docker Hub. 
You can use the registry-2 
- parameters to work around a 
pull rate limit for docker.io.
registry-2-user
Secondary registry username
Required if you are using 
a second private container 
registry.
DL Workload Automation
registry-2-passwd
Secondary registry password
Required if you are using 
a second private container 
registry.
VMware by Broadcom
174


---

VMware Private AI Foundation with NVIDIA 9.1
Category
Parameter
Label in the vSphere Client
Description
image-oneliner
Encoded one-line command
A one-line bash command that 
is run at VM provisioning. This 
value must be base64 encoded.
You can use this property 
to specify the DL workload 
container you want to 
deploy, such as PyTorch or 
TensorFlow. See Deep Learning 
Workloads in VMware Private AI 
Foundation with NVIDIA.
CAUTION:
Avoid using both
user-data and ima 
ge-oneliner.
docker-compose-uri
Encoded Docker compose file
Required if you need a Docker 
compose file to start the DL 
workload container. The 
contents of the docker-co 
mpose.yaml file that will be 
inserted into the virtual machine 
at provisioning after the virtual 
machine is started with GPU 
enabled. This value must be 
base64 encoded.
config-json
Encoded config.json
The contents of a configuration 
file for adding the following 
details:
•
Make the metrics exported 
by DCGM Exporter available 
only locally in the virtual 
machine by setting expor 
t_dcgm_to_public to
false.
•
Deactivate JupyterLab 
authentication by setting en 
able_jupyter_auth to
false.
You can use the config.j 
son file for the DL workload 
type in the sample script 
package available in Broadcom 
knowledge base article 388813.
This value must be base64 
encoded.
conda-environment-install
Conda Environment Install
A comma-separated list of 
Conda environments to be 
automatically installed after VM 
deployment is complete.
Available environments: pytor 
ch2.3.0_py3.12, pytorc 
h1.13.1_py3.10, tf2.16 
.1_py3.12, and tf1.15.5 
_py3.7.
VMware by Broadcom
175


---

VMware Private AI Foundation with NVIDIA 9.1
Category
Parameter
Label in the vSphere Client
Description
dlvmconsole-enable
Enable DL VM Console
Set to true if you want to 
use a pre-installed JupyterLab 
instance for step-by-step 
guidance for configuring, 
launching, customizing, and 
debugging each workload 
container component.
The console can be accessed at 
port 8866.
The PyTorch and TensorFlow 
DL workloads include a 
JupyterLab notebook by default.
Deep Learning Workloads in VMware Private AI Foundation with NVIDIA
You can provision a deep learning virtual machine with a supported deep learning (DL) workload in addition to its 
embedded components. The DL workloads are downloaded from the NVIDIA NGC catalog and are GPU-optimized and 
validated by NVIDIA and VMware by Broadcom.
For an overview of the deep learning VM images, see About Deep Learning VM Images in VMware Private AI Foundation 
with NVIDIA.
CUDA Sample
You can use a deep learning VM with running CUDA samples to explore vector addition, gravitational n-body simulation, 
or other examples on a VM. See the CUDA Samples page.
After the deep learning VM is launched, it runs a CUDA sample workload to test the vGPU guest driver. You can examine 
the test output in the /var/log/dl.log file.
VMware by Broadcom
176


---

VMware Private AI Foundation with NVIDIA 9.1
Table 14: CUDA Sample Container Image
Component
Description
Container image
nvcr.io/nvidia/k8s/cuda-sample:ngc_image_tag
For example:
nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-ubi8
For information on the CUDA Sample container images that are supported for deep learning VMs, see
VMware Deep Learning VM Image Release Notes.
Required inputs
To deploy a CUDA Sample workload, you must set the OVF properties for the deep learning virtual machine 
in the following way:
•
Use one of the following properties that are specific for the CUDA Sample image.
– Cloud-init script, encoded in base64 format.
To create the cloud-init script, use the contents of the cloud-config.yaml file for CUDA Sample 
in the sample script package available in Broadcom knowledge base article 388813.
– Image one-liner. Encode it in base64 format
docker run -d nvcr.io/nvidia/k8s/cuda-sample:ngc_image_tag
For example, for vectoradd-cuda11.7.1-ubi8, provide the following script in base64 format:
ZG9ja2VyIHJ1biAtZCBudmNyLmlvL252aWRpYS9rOHMvY3VkYS1zYW1wbGU6dmVjdG9yYWRkLWN1ZGExMS 
43LjEtdWJpOA==
which corresponds to the following script in plain-text format:
docker run -d nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-ubi8
•
If you plan to use vGPUs, enter the vGPU guest driver installation properties, such as vgpu-license
and ngc-api-key.
•
If you plan to use GPU passthrough, provide the following properties:
– Include and leave vgpu-license, ngc-api-key empty.
– To use the default version of the NVIDIA data center GPU driver, include and leave gpu-driver-u 
rl empty. To use a version of the NVIDIA data center GPU driver that is different from the default one, 
set gpu-driver-url to the download URL of the driver .run file according to https://docs.nvidia. 
com/datacenter/tesla/drivers/releases.json.
•
By default, the DCGM Exporter instance in the deep learning VM exposes metrics on public interfaces. 
You can then access the metrics in a web browser at <dlvm_ip>:9400/metrics, and you can 
collect them in Prometheus and visualize them in Grafana.
Because you can access the metrics without authentication, to activate local-only access to the metrics in 
the deep learning VM for security reasons, in the configuration JSON input, set the export_dcgm_to_ 
public property to false.
The DCGM Exporter listens exclusively on localhost (127.0.0.1:9400), restricting access to the local 
machine only.
•
Provide values for the properties required for a disconnected environment as needed, such as:
– vgpu-url for vGPU or gpu-driver-url for GPU passthrough
– registry-uri, registry-user, and registry-passwd
See OVF Properties of Deep Learning VMs.
Output
•
Installation logs for the vGPU guest driver or the NVIDIA data center GPU driver in /var/log/gpu-in 
stall.log.
To verify that the vGPU guest driver is installed, and the license is allocated, run the following command:
nvidia-smi -q |grep -i license
To verify that the data center GPU driver is installed, run the nvidia-smi command.
•
Cloud-init script logs in /var/log/dl.log.
PyTorch
You can use a deep learning VM with a PyTorch library to explore conversational AI, NLP, and other types of AI models, 
on a VM. If you plan to run the deep learning VM on vGPUs, see the PyTorch Production Branch page. If you plan to run 
the deep learning VM on GPU passthrough, see the PyTorch Feature Branch page.
After the deep learning VM is launched, it starts a JupyterLab instance with PyTorch packages installed and configured.
VMware by Broadcom
177


---

VMware Private AI Foundation with NVIDIA 9.1
Table 15: PyTorch Container Image
Component
Description
Container image
If using vGPU, run the following container image:
nvcr.io/nvidia/pytorch-pb25h1:ngc_image_tag
For example:
nvcr.io/nvidia/pytorch-pb25h1:25.03.03-py3
If using GPU passthrough, run the following container image:
nvcr.io/nvidia/pytorch:ngc_image_tag
For example:
nvcr.io/nvidia/pytorch:25.03-py3
For information on the PyTorch container images that are supported for deep learning VMs, see VMware 
Deep Learning VM Image Release Notes.
Required inputs
To deploy a PyTorch workload, you must set the OVF properties for the deep learning virtual machine in the 
following way:
•
Use one of the following properties that are specific for the PyTorch image.
– Cloud-init script, encoded in base64 format.
To create the cloud-init script, use the contents of the cloud-config.yaml file for PyTorch in the 
sample script package available in Broadcom knowledge base article 388813.
– Image one-liner. Encode it in base64 format.
docker run -d -p 8888:8888 nvcr.io/nvidia/pytorch-pb25h1:ngc_image_tag/
usr/local/bin/jupyter lab --allow-root --ip=* --port=8888 --no-browser --
NotebookApp.token='' --NotebookApp.allow_origin='*' --notebook-dir=/workspace
For example, for pytorch-pb25h1:25.03.03-py3, provide the following script in base 64 format:
ZG9ja2VyIHJ1biAtZCAtcCA4ODg4Ojg4ODggbnZjci5pby9udmlkaWEvcHl0b3JjaC1wYjI1aDE6MjUuMD 
MuMDMtcHkzIC91c3IvbG9jYWwvYmluL2p1cHl0ZXIgbGFiIC0tYWxsb3ctcm9vdCAtLWlwPSogLS1wb3J0 
PTg4ODggLS1uby1icm93c2VyIC0tTm90ZWJvb2tBcHAudG9rZW49JycgLS1Ob3RlYm9va0FwcC5hbGxvd1 
9vcmlnaW49JyonIC0tbm90ZWJvb2stZGlyPS93b3Jrc3BhY2U=
which corresponds to the following script in plain-text format:
docker run -d -p 8888:8888 nvcr.io/nvidia/pytorch-pb25h1:25.03.03-py3 /
usr/local/bin/jupyter lab --allow-root --ip=* --port=8888 --no-browser --
NotebookApp.token='' --NotebookApp.allow_origin='*' --notebook-dir=/workspace
•
If you plan to use vGPUs, enter the vGPU guest driver installation properties, such as vgpu-license
and ngc-api-key.
•
If you plan to use GPU passthrough, provide the following properties:
– Include and leave vgpu-license, ngc-api-key empty.
– To use the default version of the NVIDIA data center GPU driver, include and leave gpu-driver-u 
rl empty. To use a version of the NVIDIA data center GPU driver that is different from the default one, 
set gpu-driver-url to the download URL of the driver .run file according to https://docs.nvidia. 
com/datacenter/tesla/drivers/releases.json.
•
Provide values for the properties required for a disconnected environment as needed, such as:
– vgpu-url for vGPU or gpu-driver-url for GPU passthrough
– registry-uri, registry-user, and registry-passwd
•
By default, the DCGM Exporter instance in the deep learning VM exposes metrics on public interfaces. 
You can then access the metrics in a web browser at <dlvm_ip>:9400/metrics, and you can 
collect them in Prometheus and visualize them in Grafana.
Because you can access the metrics without authentication, to activate local-only access to the metrics in 
the deep learning VM for security reasons, in the configuration JSON input, set the export_dcgm_to_ 
public property to false.
The DCGM Exporter listens exclusively on localhost (127.0.0.1:9400), restricting access to the local 
machine only.
•
Deactivate authentication to the installed JupyterLab notebook to directly access JupyterLab by setting e 
nable_jupyter_auth to false in the configuration JSON input.
See OVF Properties of Deep Learning VMs.
VMware by Broadcom
178


---

VMware Private AI Foundation with NVIDIA 9.1
Component
Description
Output
•
Installation logs for the vGPU guest driver or the NVIDIA data center GPU driver in /var/log/gpu-in 
stall.log.
To verify that the vGPU guest driver or data center GPU driver is installed, run the nvidia-smi
command.
•
Cloud-init script logs in /var/log/dl.log.
•
PyTorch container.
To verify that the PyTorch container is running, run the sudo docker ps -a and sudo docker 
logs container_id command.
•
А JupyterLab notebook, included in the PyTorch container by default, that you can access at
http://dl_vm_ip:8888
By default, JupyterLab authentication is enabled. To get the authentication token, examine /var/log/ 
dl.log or run the vmtoolsd --cmd "info-get guestinfo.dlworkload.jupyterlab.token"
command in the deep learning VM console.
In the terminal of JupyterLab, verify that the following functionality is available in the notebook:
– To verify that JupyterLab can access the vGPU resource, run !nvidia-smi.
– To verify that the PyTorch related packages are installed, run pip list.
DCGM Exporter
You can use a deep learning VM with a Data Center GPU Manager (DCGM) exporter to monitor the health of and get 
metrics from GPUs used by a DL workload, using NVIDIA DCGM, Prometheus, and Grafana.
See the DCGM Exporter page.
In a deep learning VM, you run the DCGM Exporter container together with a DL workload that performs AI operations. 
After the deep learning VM is started, DCGM Exporter is ready to collect vGPU metrics and export the data to another 
application for further monitoring and visualization. You can run the monitored DL workload as a part of the cloud-init 
process or from the command line after the virtual machine is started.
VMware by Broadcom
179


---

VMware Private AI Foundation with NVIDIA 9.1
Table 16: DCGM Exporter Container Image
Component
Description
Container image
nvcr.io/nvidia/k8s/dcgm-exporter:ngc_image_tag
For example:
nvcr.io/nvidia/k8s/dcgm-exporter:3.2.5-3.1.8-ubuntu22.04
For information on the DCGM Exporter container images that are supported for deep learning VMs, see
VMware Deep Learning VM Image Release Notes.
Required inputs
To deploy a DCGM Exporter workload, you must set the OVF properties for the deep learning virtual machine 
in the following way:
•
Use one of the following properties that are specific for the DCGM Exporter image.
– Cloud-init script, encoded in base64 format.
To create the cloud-init script, use the contents of the cloud-config.yaml file for DCGM 
Exporter in the sample script package available in Broadcom knowledge base article 388813.
Note:
You can also add the instructions for running the DL workload whose GPU performance you want to 
measure with DCGM Exporter to the cloud-init script.
– Image one-liner. Encode it in base64 format.
docker run -d --gpus all --cap-add SYS_ADMIN --rm -p 9400:9400 nvcr.io/nvidia/k8s/
dcgm-exporter:ngc_image_tag
For example, for dcgm-exporter:3.2.5-3.1.8-ubuntu22.04, provide the following script in base64 format:
ZG9ja2VyIHJ1biAtZCAtLWdwdXMgYWxsIC0tY2FwLWFkZCBTWVNfQURNSU4gLS1ybSAtcCA5NDAwOjk0MD 
AgbnZjci5pby9udmlkaWEvazhzL2RjZ20tZXhwb3J0ZXI6My4yLjUtMy4xLjgtdWJ1bnR1MjIuMDQ=
which corresponds to the following script in plain-text format:
docker run -d --gpus all --cap-add SYS_ADMIN --rm -p 9400:9400 nvcr.io/nvidia/k8s/
dcgm-exporter:3.2.5-3.1.8-ubuntu22.04
•
If you plan to use vGPUs, enter the vGPU guest driver installation properties, such as vgpu-license
and ngc-api-key.
•
If you plan to use GPU passthrough, provide the following properties:
– Include and leave vgpu-license, ngc-api-key empty.
– To use the default version of the NVIDIA data center GPU driver, include and leave gpu-driver-u 
rl empty. To use a version of the NVIDIA data center GPU driver that is different from the default one, 
set gpu-driver-url to the download URL of the driver .run file according to https://docs.nvidia. 
com/datacenter/tesla/drivers/releases.json.
•
Provide values for the properties required for a disconnected environment as needed, such as:
– vgpu-url for vGPU or gpu-driver-url for GPU passthrough
– registry-uri, registry-user, and registry-passwd
•
By default, the DCGM Exporter instance in the deep learning VM exposes metrics on public interfaces. 
You can then access the metrics in a web browser at <dlvm_ip>:9400/metrics, and you can 
collect them in Prometheus and visualize them in Grafana.
Because you can access the metrics without authentication, to activate local-only access to the metrics in 
the deep learning VM for security reasons, in the configuration JSON input, set the export_dcgm_to_ 
public property to false.
The DCGM Exporter listens exclusively on localhost (127.0.0.1:9400), restricting access to the local 
machine only.
See OVF Properties of Deep Learning VMs.
Output
•
Installation logs for the vGPU guest driver or the NVIDIA data center GPU driver in /var/log/gpu-in 
stall.log.
To verify that the vGPU guest driver or the NVIDIA data center GPU driver is installed, log in to the VM 
over SSH and run the nvidia-smi command.
•
Cloud-init script logs in /var/log/dl.log.
•
DCGM Exporter that you can access at http://dl_vm_ip:9400.
Next, in the deep learning VM, you run a DL workload, and visualize the data on another virtual machine by 
using Prometheus at http://visualization_vm_ip:9090 and Grafana at http://visualiza 
tion_vm_ip:3000. See Monitoring the vGPU Performance of a Deep Learning VM.
VMware by Broadcom
180


---

VMware Private AI Foundation with NVIDIA 9.1
Triton Inference Server
You can use a deep learning VM with a Triton Inference Server for loading a model repository and receive inference 
requests.
If you plan to run the deep learning VM on vGPUs, see the Triton Inference Server Production Branch page. If you plan to 
run the deep learning VM on GPU passthrough, see theTriton Inference Server Feature Branch page.
VMware by Broadcom
181


---

VMware Private AI Foundation with NVIDIA 9.1
Table 17: Triton Inference Server Container Image
Component
Description
Container image
For using vGPUs, run the following container image:
nvcr.io/nvidia/tritonserver-pb25h1:ngc_image_tag
For example:
nvcr.io/nvidia/tritonserver-pb25h1:25.03.03-py3
For using GPU passthrough, run the following container image:
nvcr.io/nvidia/pytorch:ngc_image_tag
For example:
nvcr.io/nvidia/pytorch:25.03-py3
For information on the Triton Inference Server container images that are supported for deep learning VMs, 
see VMware Deep Learning VM Release Notes.
Required inputs
To deploy a Triton Inference Server workload, you must set the OVF properties for the deep learning virtual 
machine in the following way:
•
Use one of the following properties that are specific for the Triton Inference Server image.
– Cloud-init script, encoded in base64 format.
To create the cloud-init script, use the contents of the cloud-config.yaml file for Triton Inference 
Server in the sample script package available in Broadcom knowledge base article 388813.
– Image one-liner encoded in base64 format
docker run -d --gpus all --rm -p8000:8000 -p8001:8001 -p8002:8002 -v /home/
vmware/model_repository:/models nvcr.io/nvidia/tritonserver-pb25h1:ngc_image_tag
 tritonserver --model-repository=/models --model-control-mode=poll
For example, for tritonserver:25.03.03-py3, provide the following script in base 64 format:
ZG9ja2VyIHJ1biAtZCAtLWdwdXMgYWxsIC0tcm0gLXA4MDAwOjgwMDAgLXA4MDAxOjgwMDEgLXA4MDAyOj 
gwMDIgLXYgL2hvbWUvdm13YXJlL21vZGVsX3JlcG9zaXRvcnk6L21vZGVscyBudmNyLmlvL252aWRpYS90 
cml0b25zZXJ2ZXItcGIyNWgxOjI1LjAzLjAzLXB5MyB0cml0b25zZXJ2ZXIgLS1tb2RlbC1yZXBvc2l0b3 
J5PS9tb2RlbHMgLS1tb2RlbC1jb250cm9sLW1vZGU9cG9sbA==
which corresponds to the following script in plain-text format:
docker run -d --gpus all --rm -p8000:8000 -p8001:8001 -p8002:8002 -v /home/
vmware/model_repository:/models nvcr.io/nvidia/tritonserver-pb25h1:25.03.03-py3 
 tritonserver --model-repository=/models --model-control-mode=poll
•
If you plan to use vGPUs, enter the vGPU guest driver installation properties, such as vgpu-license
and ngc-api-key.
•
If you plan to use GPU passthrough, provide the following properties:
– Include and leave vgpu-license, ngc-api-key empty.
– To use the default version of the NVIDIA data center GPU driver, include and leave gpu-driver-u 
rl empty. To use a version of the NVIDIA data center GPU driver that is different from the default one, 
set gpu-driver-url to the download URL of the driver .run file according to https://docs.nvidia. 
com/datacenter/tesla/drivers/releases.json.
•
Provide values for the properties required for a disconnected environment as needed, such as:
– vgpu-url for vGPU or gpu-driver-url for GPU passthrough
– registry-uri, registry-user, and registry-passwd
•
By default, the DCGM Exporter instance in the deep learning VM exposes metrics on public interfaces. 
You can then access the metrics in a web browser at <dlvm_ip>:9400/metrics, and you can 
collect them in Prometheus and visualize them in Grafana.
Because you can access the metrics without authentication, to activate local-only access to the metrics in 
the deep learning VM for security reasons, in the configuration JSON input, set the export_dcgm_to_ 
public property to false.
The DCGM Exporter listens exclusively on localhost (127.0.0.1:9400), restricting access to the local 
machine only.
See OVF Properties of Deep Learning VMs.
VMware by Broadcom
182


---

VMware Private AI Foundation with NVIDIA 9.1
Component
Description
Output
•
Installation logs for the vGPU guest driver or the NVIDIA data center GPU driver in /var/log/gpu-in 
stall.log.
To verify that the vGPU guest driver is installed, log in to the VM over SSH and run the nvidia-smi
command.
•
Cloud-init script logs in /var/log/dl.log.
•
Triton Inference Server container.
To verify that the Triton Inference Server container is running, run the docker ps -a and docker 
logs container_id commands.
The model repository for the Triton Inference Server is in /home/vmware/model_repository. 
Initially, the model repository is empty and the initial log of the Triton Inference Server instance shows that no 
model is loaded.
Create a Model Repository
To load your model for model inference, perform these steps:
1. Create the model repository for your model.
See the NVIDIA Triton Inference Server Model Repository documentation .
2. Copy the model repository to /home/vmware/model_repository so that the Triton Inference Server can load it.
cp -r path_to_your_created_model_repository/* /home/vmware/model_repository/
 
Send Model Inference Requests
1. Verify that the Triton Inference Server is healthy and models are ready by running this command in the deep learning 
VM console.
curl -v localhost:8000/v2/simple_sequence
2. Send a request to the model by running this command on the deep learning VM.
curl -v localhost:8000/v2/models/simple_sequence
For more information on using the Triton Inference Server, see NVIDIA Triton Inference Server Model Repository 
documentation.
Assign a Static IP Address to a Deep Learning VM in VMware Private AI 
Foundation with NVIDIA
By default, the deep learning VM images are configured with DHCP address assignment. If you want to deploy a deep 
learning VM with a static IP address directly on a vSphere cluster, you must add additional code to the cloud-init section.
On vSphere Supervisor, IP address assignment is determined by the network configuration for the Supervisor in NSX.
1. Create a cloud-init script in plain-text format for the DL workload you plan to use.
See Deep Learning Workloads in VMware Private AI Foundation with NVIDIA.
2. Add the following additional code to the cloud-init script.
#cloud-config
<instructions_for_your_DL_workload>
manage_etc_hosts: true 
  
write_files: 
  - path: /etc/netplan/50-cloud-init.yaml 
    permissions: '0600' 
    content: | 
      network: 
        version: 2 
        renderer: networkd 
VMware by Broadcom
183


---

VMware Private AI Foundation with NVIDIA 9.1
        ethernets: 
          ens33: 
            dhcp4: false # disable DHCP4 
            addresses: [x.x.x.x/x]  # Set the static IP address and mask 
            routes: 
                - to: default 
                  via: x.x.x.x # Configure gateway 
            nameservers: 
              addresses: [x.x.x.x, x.x.x.x] # Provide the DNS server address. Separate multiple DNS server 
 addresses with commas. 
  
runcmd: 
  - netplan apply
3. Encode the resulting cloud-init script in base64 format.
4. Set the cloud-init script in base64 format as a value to the user-data OVF parameter of the deep learning VM image.
Assigning a Static IP Address to a CUDA Sample Workload
For an example deep learning VM with a CUDA Sample DL workload:
Deep Learning VM Element
Example Value
DL workload image
nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-u 
bi8
IP address
10.199.118.245
Subnet prefix
/25
Gateway
10.199.118.253
DNS servers
•
10.142.7.1
•
10.132.7.1
you provide the following cloud-init code:
I2Nsb3VkLWNvbmZpZwp3cml0ZV9maWxlczoKLSBwYXRoOiAvb3B0L2Rsdm0vZGxfYXBwLnNoCiAgcGVybWlzc2lvbnM6ICcwNzU1JwogIGNvbnRlbnQ6
which corresponds to the following script in plain-text format:
#cloud-config
write_files:
- path: /opt/dlvm/dl_app.sh 
  permissions: '0755' 
  content: | 
    #!/bin/bash 
    docker run -d nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-ubi8
manage_etc_hosts: true 
  
write_files: 
  - path: /etc/netplan/50-cloud-init.yaml 
    permissions: '0600' 
    content: | 
      network: 
        version: 2 
        renderer: networkd 
        ethernets: 
          ens33: 
            dhcp4: false # disable DHCP4 
            addresses: [10.199.118.245/25]  # Set the static IP address and mask 
VMware by Broadcom
184


---

VMware Private AI Foundation with NVIDIA 9.1
            routes: 
                - to: default 
                  via: 10.199.118.253 # Configure gateway 
            nameservers: 
              addresses: [10.142.7.1, 10.132.7.1] # Provide the DNS server address. Separate 
 mulitple DNS server addresses with commas. 
  
runcmd: 
  - netplan apply
Configure a Deep Learning VM with a Proxy Server
To connect your deep learning VM to the Internet in a disconnected environment where Internet access is over a proxy 
server, you must provide the proxy server details in the cloud-init script that you provide at virtual machine deployment.
1. Create the cloud-init script.
You can use the cloud-init code for the relevant DL workload type in the sample script package in Broadcom 
knowledge base article 388813.
In the cloud-init code, set proxy URLs to the variable_httpProxy, variable_httpsProxy,
variable_noProxy, and variable_enableProxyForNVIDIALicense environment variables, or directly set 
values to the relevant code variables.
If proxy support is enabled for NVIDIA License Server registration, the proxy server set with variable_httpProxy
must be an unauthenticated HTTP proxy.
Use this URL format:
Proxy Server Configuration
URL Format
Proxy server that does not require authentication
protocol:// ip-address-or-fqdn:port
Proxy server that requires authentication
protocol://username:password@ip-address-or-fqdn:port
where:
•
protocol is the communication protocol used by the proxy server, such as http or https.
•
username and password are the credentials for authentication to the proxy server. If the proxy server does not 
require authentication, skip these parameters.
•
ip-address-or-fqdn: The IP address or host name of the proxy server.
•
port: The port number on which the proxy server is listening for incoming requests.
2. Encode the resulting cloud-init code in base64 format.
3. When you deploy the deep learning VM image, add the encoded value to the user-data OVF property.
Troubleshooting Deep Learning VM Deployment in VMware Private AI 
Foundation with NVIDIA
The troubleshooting information about deployment of deep learning VM in VMware Private AI Foundation with NVIDIA
provides solutions to potential problems that you might encounter.
In This Chapter
•
DL Workload Automation Is Not Performed
•
Downloading a DL Workload Fails Because of Invalid Authentication Credentials
•
The NVIDIA vGPU Guest Driver Is Shown as Unlicensed
DL Workload Automation Is Not Performed
After you deploy a deep learning VM in VMware Private AI Foundation with NVIDIA, the specified DL workload is not 
running.
VMware by Broadcom
185


---

VMware Private AI Foundation with NVIDIA 9.1
You deploy a deep learning VM with a DL workload to be pre-installed at initial startup. After the deep learning VM is 
started, the DL workload is not carried out.
1. The base64-encoded user-data or values of other OVF parameters, such as image-oneliner or config-json
are saved or decoded incorrectly in the /opt/dlvm/dl_app.sh file. As a result, the DL workload script is not run.
2. The vGPU driver installation failed, causing the cloud-init script passed in the user-data OVF parameter to not be 
run. The cloud-init script relies on the successful installation of the NVIDIA vGPU driver.
On the deep learning VM, verify whether the DL workload is installed on the virtual machine and apply a solution 
accordingly.
Availability of the DL Workload
Solution
The DL workload components are not created on the 
virtual machine.
•
If you are using a cloud-init script as input to the user-data OVF 
parameter, verify the following values:
– Check the script that is encoded and input as user-data.
Make sure that #cloud-config appears on the first line and is 
included in the base64 equivalent.
– Check the path parameter.
– Check the base64 encoded string and make sure that the user-
data value is correctly saved in /opt/dlvm/dl_app.sh.
•
If you are using other OVF parameters, verify the following values:
– image-oneliner. Check the base64 encoded string and make 
sure that the one-line command is correctly saved in /opt/dlvm 
/dl_app.sh.
– config-json. Check the base64 encoded string. If provided, the 
value is saved in /root/dlvm/ovf-env.xml.
For information about the OVF parameters of the latest deep learning VM 
image, see OVF Properties of Deep Learning VMs.
The DL workload components are created but the 
workload is not running.
•
Check the error messages in /var/log/gpu-install.log.
•
If you are using a cloud-init script as input to the user-data OVF 
parameter, check if the NVIDIA vGPU driver is installed and is working 
correctly. The cloud-init script is not run if the NVIDIA vGPU driver 
installation is unsuccessful.
Downloading a DL Workload Fails Because of Invalid Authentication Credentials
After you deploy a deep learning VM in VMware Private AI Foundation with NVIDIA, downloading the specified DL 
workload on the virtual machine fails with error log messages indicating invalid authentication credentials.
If you are installing a DL workload container image, such as Triton Inference Server, TensorFlow or Pytorch, the /var/
log/dl.log file contains the following message:
Unable to find image 'nvcr.io/nvidia/tritonserver-pb24h1:24.03.02-py3' locally
docker: Error response from daemon: unauthorized: <html>
<head><title>401 Authorization Required</title></head>
<body>
For NVIDIA RAG, the /var/log/dl.log file contains the following message:
Error response from daemon: Get "https://nvcr.io/v2/": unauthorized.
The authentication to the nvcr.io container registry has failed. As a result, the DL workload image cannot be downloaded 
on the virtual machine.
•
Verify the credentials for login to the nvcr.io registry passed as OVF parameters or to the catalog setup wizard for 
private AI in VCF Automation.
– Registry: nvcr.io
– Registry user account: $oauthtoken
– Registry password: NGC API key
•
Verify that the NVIDIA NGC API key has the permissions to access the required resources and that the key has not 
expired.
VMware by Broadcom
186


---

VMware Private AI Foundation with NVIDIA 9.1
The NVIDIA vGPU Guest Driver Is Shown as Unlicensed
After a deep learning VM is deployed in VMware Private AI Foundation with NVIDIA, the NVIDIA vGPU guest driver status 
is unlicensed.
The /var/log/gpu-install.log file contains one of the following messages:
License Status: Unlicensed
Unlicensed (Restricted)
The NVIDIA vGPU client configuration token that you pass as a value to the vgpu-license OVF property or to the 
catalog setup wizard for private AI in VCF Automation is invalid, expired, or incorrectly formatted.
•
Verify the validity of the client configuration token.
•
Verify that the vGPU license is correctly formatted and follows the JWT token format, which typically looks like
eyxxxx.eyxxxxx.xxxxx.
You can decode the JWT token at jwt.io to check the expiration date and node server URL.
•
The vGPU license token also saved in /etc/nvidia/ClientConfigToken/
client_configuration_token.tok.
•
To troubleshoot the problem further, run this command to check for specific error messages related to the 
communication to the NVIDIA license server.
cat /var/log/syslog | grep -i nvidia
To apply a new token, following these steps:
1. Replace the content of the /etc/nvidia/ClientConfigToken/client_configuration_token.tok file with a 
new token, run the following command:
echo -n $vgpu_license_token > /etc/nvidia/ClientConfigToken/client_configuration_token.tok
2. Restart the NVIDIA service.
/etc/init.d/nvidia-gridd restart
3. Verify the license status of the NVIDIA vGPU guest driver.
nvidia-smi -q | grep -i "license status" | sed 's/^[ \t]*//'
VMware by Broadcom
187


---

VMware Private AI Foundation with NVIDIA 9.1
Deploying AI Workloads on VKS Clusters in VMware Private AI 
Foundation with NVIDIA
As a DevOps engineer, you can deploy container AI workloads on vSphere Kubernetes Service (VKS) clusters whose 
worker nodes are accelerated with NVIDIA GPUs.
In This Chapter
•
Deploy a GPU-Accelerated VKS Cluster by Using a Self-Service Catalog Item in VCF Automation
•
Provision a VKS Cluster with vGPU by Using the kubectl Command in a Connected VMware Private AI Foundation 
with NVIDIA Environment
•
Provision a VKS Cluster with vGPU by Using the kubectl Command in a Disconnected VMware Private AI Foundation 
with NVIDIA Environment
•
Provision a VKS Cluster with GPU Passthrough by Using the kubectl Command in a Connected VMware Private AI 
Foundation with NVIDIA Environment
•
Provision a VKS Cluster with GPU Passthrough by Using the kubectl Command in a Disconnected VMware Private AI 
Foundation with NVIDIA Environment
Deploy a GPU-Accelerated VKS Cluster by Using a Self-Service 
Catalog Item in VCF Automation
As a DevOps engineer, you can provision a VKS cluster accelerated with NVIDIA GPUs by using the AI Kubernetes 
Cluster catalog item in the self-service VCF Automation catalog. Then, you can deploy AI container images from NVIDIA
NGC on the cluster.
•
Verify that your organization administrator has configured VMware Private AI Foundation with NVIDIA catalog items for 
your project. See Preparing VCF for Private AI Workload Deployment.
•
Verify with your organization administrator that you have permissions to request AI catalog items. See Preparing VCF 
for Private AI Workload Deployment.
The VKS cluster contains an NVIDIA GPU operator, which is a Kubernetes operator that is responsible for setting up the 
right NVIDIA driver for the NVIDIA GPU hardware on the VKS cluster nodes. The deployed cluster is ready to use for AI/
ML workloads without needing additional GPU-related setup.
The deployment contains a vSphere namespace, a VKS cluster, multiple resources inside the VKS cluster, and the
NVIDIA GPU operator.
1. In VCF Automation, select Build & Deploy > Catalog, locate the AI Kubernetes Cluster card and click Request.
2. Select a project.
3. Enter a name and description for your deployment.
4. Select the number of control plane nodes.
Setting
Sample value
Node count
1
VM class
best-effort-4xlarge - 16 CPUs and 128 GB Memory
The class selection defines the resources available within the 
virtual machine.
VMware by Broadcom
188


---

VMware Private AI Foundation with NVIDIA 9.1
5. Select the number of worker nodes.
The worker nodes need GPU access.
Setting
Sample value
Node count
3
VM class
best-effort-4xlarge-a100-40c - 1 vGPU (40 GB), 16 
CPUs and 120 GB Memory
Time-slicing replicas
1
Time-slicing defines a set of replicas for a GPU that is shared 
between workloads.
6. Provide the NVIDIA NGC API key.
7. Click Submit.
Run an AI container image. In a connected environment, use the NVIDIA NGC catalog. In a disconnected environment, 
use the Harbor Registry on the Supervisor.
Provision a VKS Cluster with vGPU by Using the kubectl Command in a 
Connected VMware Private AI Foundation with NVIDIA Environment
In VMware Private AI Foundation with NVIDIA, as a DevOps engineer, you provision a VKS cluster that uses NVIDIA 
GPUs in a namespace in an organization in VCF Automation. Then, you can deploy container AI workloads from the 
NVIDIA NGC catalog.
Verify with the organization administrator that the following prerequisites are in place for the AI-ready infrastructure.
•
VMware Private AI Foundation with NVIDIA is configured. See Preparing VCF for Private AI Workload Deployment.
•
You are added to a project and a namespace in your All Apps organization in VCF Automation.
•
You have the VCF Consumption CLI and kubectl installed on your machine. See Installing and Using VCF CLI in the
VCF documentation.
•
You have the certificate of your VCF Automation instance saved to your machine, for example, as a
vcf_automation_certificate.pem file in your current working folder.
If your machine has OpenSSL installed, you can use the following command:
openssl s_client -connect <vcf_automation_FQDN>:443 -showcerts </dev/null 2>/dev/null | openssl x509 -
outform PEM > vcf_automation_certificate.pem
You use kubectl to deploy the VKS cluster in the namespace configured by the organization administrator.
Log in to the Namespace in Your Organization by using the VCF Consumption 
CLI
To deploy a GPU-accelerated VKS cluster, you use the kubectl CLI in the context of a namespace in your organization in
VCF Automation. The namespace is represented by a context that is created by using the VCF Consumption CLI.
1. Generate an API token for your account in your VCF Automation organization.
a) Log in to your VCF Automation organization.
b) In the top-right corner, select User Settings > My Account from the account drop-down menu.
c) On the API Tokens tab, generate a token.
2. On the command line, log in to the namespace in your organization in VCF Automation by using the VCF Consumption 
CLI.
Use the --ca-certificate option if the VCF Automation certificate is not added to the truststore on your machine.
vcf context create vcf_automation_context_name --endpoint vcf_automation_fqdn_or_ip --ca-
certificate vcf_automation_certificate.pem --api-token vcf_automation_api_token --type cci
VMware by Broadcom
189


---

VMware Private AI Foundation with NVIDIA 9.1
3. Install the cluster plug-in for the VCF Consumption CLI.
vcf plugin install cluster
4. Switch to the context for the target namespace in VCF Automation.
vcf context use vcf_automation_context_name:namespace_name:project_name
Provision the VKS Cluster with vGPU in a Connected Environment
After you enter the context for the allocated namespace in your organization in VCF Automation, you can proceed with 
provisioning a VKS cluster that uses vGPU devices.
1. Get the names of the custom VM classes in the namespace and identify those with vGPU profile.
kubectl get virtualmachineclass
If you want to get the VM classes with vGPUs, run the following command.
Linux
kubectl get virtualmachineclass -o json | jq -r '.items[] 
 | select(.spec.hardware.devices.vgpuDevices != null and 
 (.spec.hardware.devices.vgpuDevices | length) > 0) | .metadata.name'
Windows in PowerShell
$jqFilter = ".items[] | select(.spec.hardware.devices.vgpuDevices != null and 
 (.spec.hardware.devices.vgpuDevices | length) > 0) | .metadata.name"
kubectl get virtualmachineclass -o json | jq -r $jqFilter
2. Get the storage class for the namespace from the output of the following command.
kubectl describe namespaces namespace_name
The storage class name is available in the storage_class_name.storageclass.storage.k8s.io/
requests.storage resource name in the namespace_name-storagequota resource quota.
3. Create a YAML file for provisioning the VKS cluster using a vGPU-based VM class and a storage class.
An example YAML file, gpu-vks-cluster-name.yaml, contains the specification of a VKS cluster called gpu-
accelerated-cluster whose worker nodes must deployed by using a vGPU-enabled VM class for A100, a100-
medium, and whose storage class is paif-cls01-vsan-storage-policy. The cluster requires at least VKS 3.4 
and VKr 1.33.1 and its nodes are running Ubuntu 24.04.
For information about the latest VKS releases, see VMware vSphere Kubernetes Service Release Notes. For 
information about the latest VKr releases, see VMware VKr Release Notes.
apiVersion: cluster.x-k8s.io/v1beta1
kind: Cluster
metadata: 
  name: gpu-accelerated-cluster
spec: 
  clusterNetwork: 
    pods: 
      cidrBlocks: 
      - 192.0.2.0/16 
    serviceDomain: cluster.local 
    services: 
      cidrBlocks: 
      - 198.51.100.0/12 
  topology: 
    class: builtin-generic-v3.4.0 
    version: v1.33.1---vmware.1-fips-vkr.2 
    controlPlane: 
      metadata: 
        annotations: 
          run.tanzu.vmware.com/resolve-os-image: os-name=ubuntu,os-version=24.04 
      replicas: 1 
VMware by Broadcom
190


---

VMware Private AI Foundation with NVIDIA 9.1
    variables: 
    - name: vmClass 
      value: guaranteed-large 
    - name: storageClass 
      value: paif-cls01-vsan-storage-policy 
    # Uncomment this part if the VKS cluster must use  
    # a proxy server to connect to the NVIDIA Web site.  
    # - name: osConfiguration 
      # value: 
        # systemProxy: 
          # http: http://<fqdn_or_ip_address_of_http_proxy>/ 
          # https: http://<fqdn_or_ip_address_of_https_proxy>/ 
          # noProxy: [<subnet_CIDR_1>,<subnet_CIDR_2>,<subnet_CIDR_3>,<subnet_CIDR_4>] 
    workers: 
      machineDeployments: 
      - class: node-pool 
        metadata: 
          annotations: 
            run.tanzu.vmware.com/resolve-os-image: os-name=ubuntu,os-version=24.04 
        name: worker-01 
        replicas: 1 
        variables: 
          overrides: 
          - name: vmClass 
            value: a100-medium 
          - name: volumes 
            value: 
            - capacity: 100Gi 
              mountPath: /var/lib/kubelet 
              name: kubelet-dir 
              storageClass: paif-cls01-vsan-storage-policy 
           - capacity: 100Gi 
              mountPath: /var/lib/containerd 
              name: containerd-dir 
              storageClass: paif-cls01-vsan-storage-policy
4. Provision the VKS cluster.
vcf cluster create -f gpu-vks-cluster-name.yaml
You can monitor cluster provisioning by running the following commands where gpu-accelerated-cluster is an example 
cluster name:
vcf cluster list
vcf cluster get gpu-accelerated-cluster
vcf cluster machinehealthcheck node get gpu-accelerated-cluster
vcf cluster machinehealthcheck control-plane get gpu-accelerated-cluster
5. Run the following commands to be able to log in to the VKS cluster.
vcf cluster register-vcfa-jwt-authenticator gpu-accelerated-cluster
vcf cluster kubeconfig get gpu-accelerated-cluster --export-file gpu-accelerated-cluster-kubconfig
kubectl --kubeconfig=gpu-accelerated-cluster-kubconfig get namespaces
Install and Configure the NVIDIA GPU Operator from the NVIDIA Registry
After you provision a VKS cluster using a vGPU-enabled VM class, you install the NVIDIA GPU Operator that supports 
vGPU devices.
VMware by Broadcom
191


---

VMware Private AI Foundation with NVIDIA 9.1
1. Install the NVIDIA GPU Operator for vGPU.
a) Log in to the VKS cluster.
vcf context create gpu-cluster --kubeconfig "gpu-accelerated-cluster-kubconfig" --
kubecontext kubecontext-from-kubeconfig
vcf context use gpu-cluster
b) Prepare the environment.
export NGC_API_KEY=<your_ngc_api_key>
export NGC_USER_EMAIL=<your_ngc_user_email>
echo -n "your_client_configuration_token" > client_configuration_token.tok
## There are default versions set in the installer script, but you can optionally specify versions.
# export VGPU_DRIVER_VERSION=<your_vgpu_driver_version>
# export NVAIE_VERSION=<your_nvaie_version>
# export GPU_OPERATOR_VERSION=<your_gpu_operator_version> 
  
## For proxy case only
# export https_proxy=<your_https_or_http_proxy>
If you are using a proxy server, create a file with the proxy settings for driver installation, for example, env-proxy-
values.yaml.
driver: 
   env: 
   - name: HTTPS_PROXY 
     value: <your_https_proxy> 
   - name: HTTP_PROXY 
     value: <your_http_proxy> 
   - name: https_proxy 
     value: <your_https_proxy> 
   - name: http_proxy 
     value: <your_http_proxy>
If you use a cloud license server (CLS), set the proxy information into the gridd.conf file. For more information, 
see NVIDIA Licensing User Guide.
ProxyServerAddress=<your_proxy_server_address>
ProxyServerPort=<your_proxy_server_port>
c) Install the NVIDIA GPU Cloud (NGC) CLI.
See Getting Started with the NGC CLI.
d) Install the NVIDIA GPU Operator.
If you are using a proxy, remove the comment from the export VALUES_FILE=env-proxy-values.yaml line.
ngc --org=<nvidia registry resource download-version> nvidia/vgpu/gpu-operator-installer-6:6.3 
  
## For proxy case only
# export VALUES_FILE=env-proxy-values.yaml 
  
bash ./gpu-operator-installer-6_v6.3/gpu-operator-nvaie-6.3.sh install
e) Verify the installation status.
kubectl get pods -n gpu-operator
2. Configure the NVIDIA GPU Operator.
a) Verify the NVIDIA vGPU license.
Replace 8118f below with the number you got from the output of the kubectl get pods -n gpu-operator
command.
VMware by Broadcom
192


---

VMware Private AI Foundation with NVIDIA 9.1
kubectl -n gpu-operator exec nvidia-driver-daemonset-8ll8f -- nvidia-smi -q | grep License
b) If you want to run multiple NVIDIA NIMs on one GPU device, configure a time slicing policy.
This example modifies the number of replicas to 2.
cat > time-slicing-config-all.yaml << EOF
apiVersion: v1
kind: ConfigMap
metadata: 
  name: time-slicing-config-all
data: 
  any: |- 
    version: v1 
    flags: 
      migStrategy: none 
    sharing: 
      timeSlicing: 
        resources: 
        - name: nvidia.com/gpu 
          replicas: 2
EOF 
    
kubectl create -n gpu-operator -f time-slicing-config-all.yaml 
    
kubectl patch clusterpolicy/cluster-policy \ 
    -n gpu-operator --type merge \ 
    -p '{"spec": {"devicePlugin": {"config": {"name": "time-slicing-config-all", "default": "any"}}}}' 
    
kubectl get nodes -l nvidia.com/gpu.present -o json |   jq '.items[0].status.allocatable | 
    with_entries(select(.key | startswith("nvidia.com/"))) | 
    with_entries(select(.value != "0"))'
# The number you got here should be (number of vGPUs x number of replicas specified above)
Deploy an AI container image from the NVIDIA NGC catalog.
Provision a VKS Cluster with vGPU by Using the kubectl Command in a 
Disconnected VMware Private AI Foundation with NVIDIA Environment
In VMware Private AI Foundation with NVIDIA, as a DevOps engineer, by using the Kubernetes API, you provision a VKS 
cluster that uses NVIDIA vGPUs.
You use VCF Consumption CLI and kubectl to deploy the VKS cluster in the namespace configured by the organization 
administrator.
Prerequisites
Verify with the cloud administrator that the following prerequisites are in place for the AI-ready infrastructure.
•
VMware Private AI Foundation with NVIDIA is configured for a disconnected environment. See Preparing VCF for 
Private AI Workload Deployment.
•
Verify that you can access the GPU-accelerated namespace in VCF Automation or that you have the namespace 
details from your organization administrator.
•
Download the certificate of your VCF Automation instance.
•
In a disconnected environment, a content library with Ubuntu TKr images is added to the vSphere namespace for AI 
workloads. See Configure a Content Library with Ubuntu VKr for a Disconnected VMware Private AI Foundation with 
NVIDIA Environment.
•
Verify that you have a machine that has access to the Supervisor endpoint and to the local Harbor registry hosting the 
NVIDIA GPU Operator chart definitions.
VMware by Broadcom
193


---

VMware Private AI Foundation with NVIDIA 9.1
•
Verify that you have a license token for the NVIDIA vGPU driver from the DLS.
•
Get the IP address of the local Ubuntu package repository from the administrator who set up the repository.
•
Contact your Harbor administrator for the URL and credentials for the Harbor project that contains the container 
images in the NVIDIA GPU Operator package.
Log in to the Namespace in Your Organization by using the VCF Consumption 
CLI
To deploy a GPU-accelerated VKS cluster, you use the kubectl CLI in the context of a namespace in your organization in
VCF Automation. The namespace is represented by a context that is created by using the VCF Consumption CLI.
1. Generate an API token for your account in your VCF Automation organization.
a) Log in to your VCF Automation organization.
b) In the top-right corner, select User Settings > My Account from the account drop-down menu.
c) On the API Tokens tab, generate a token.
2. On the command line, log in to the namespace in your organization in VCF Automation by using the VCF Consumption 
CLI.
Use the --ca-certificate option if the VCF Automation certificate is not added to the truststore on your machine.
vcf context create vcf_automation_context_name --endpoint vcf_automation_fqdn_or_ip --ca-
certificate vcf_automation_certificate.pem --api-token vcf_automation_api_token --type cci
3. Install the cluster plug-in for the VCF Consumption CLI.
vcf plugin install cluster
4. Switch to the context for the target namespace in VCF Automation.
vcf context use vcf_automation_context_name:namespace_name:project_name
Provision the VKS Cluster with vGPU in a Disconnected Environment
After you enter the context for the allocated namespace in your organization in VCF Automation, you can proceed with 
provisioning a VKS cluster that uses vGPU devices.
1. Get the names of the custom VM classes in the namespace and identify those with vGPU profile.
kubectl get virtualmachineclass
If you want to get the VM classes with vGPUs, run the following command.
Linux
kubectl get virtualmachineclass -o json | jq -r '.items[] 
 | select(.spec.hardware.devices.vgpuDevices != null and 
 (.spec.hardware.devices.vgpuDevices | length) > 0) | .metadata.name'
Windows in PowerShell
$jqFilter = ".items[] | select(.spec.hardware.devices.vgpuDevices != null and 
 (.spec.hardware.devices.vgpuDevices | length) > 0) | .metadata.name"
kubectl get virtualmachineclass -o json | jq -r $jqFilter
2. Get the storage class for the namespace from the output of the following command.
kubectl describe namespaces namespace_name
The storage class name is available in the storage_class_name.storageclass.storage.k8s.io/
requests.storage resource name in the namespace_name-storagequota resource quota.
3. Create a YAML file for provisioning the VKS cluster using a vGPU-based VM class and a storage class.
An example YAML file, gpu-vks-cluster-name.yaml, contains the specification of a VKS cluster, called gpu-
accelerated-cluster, whose worker nodes must run on a vGPU-enabled VM class for A100, a100-medium, and 
whose storage class is paif-cls01-vsan-storage-policy. The cluster requires at least VKS 3.4.1 and VKr 
1.33.1 and its nodes are running Ubuntu 24.04.
VMware by Broadcom
194


---

VMware Private AI Foundation with NVIDIA 9.1
For information about the latest VKS releases, see VMware vSphere Kubernetes Service Release Notes. For 
information about the latest VKr releases, see VMware VKr Release Notes.
apiVersion: cluster.x-k8s.io/v1beta1
kind: Cluster
metadata: 
  name: gpu-accelerated-cluster
spec: 
  clusterNetwork: 
    pods: 
      cidrBlocks: 
      - 192.0.2.0/16 
    serviceDomain: cluster.local 
    services: 
      cidrBlocks: 
      - 198.51.100.0/12 
  topology: 
    class: builtin-generic-v3.4.0 
    version: v1.33.1---vmware.1-fips-vkr.2 
    controlPlane: 
      metadata: 
        annotations: 
          run.tanzu.vmware.com/resolve-os-image: os-name=ubuntu,os-version=24.04 
      replicas: 1 
    variables: 
    - name: vmClass 
      value: guaranteed-large 
    - name: storageClass 
      value: paif-cls01-vsan-storage-policy 
    # Uncomment this part if the VKS cluster must use  
    # a proxy server to connect to the NVIDIA Web site.  
    # - name: osConfiguration 
      # value: 
        # systemProxy: 
          # http: http://<fqdn_or_ip_address_of_http_proxy>/ 
          # https: http://<fqdn_or_ip_address_of_https_proxy>/ 
          # noProxy: [<subnet_CIDR_1>,<subnet_CIDR_2>,<subnet_CIDR_3>,<subnet_CIDR_4>] 
    workers: 
      machineDeployments: 
      - class: node-pool 
        metadata: 
          annotations: 
            run.tanzu.vmware.com/resolve-os-image: os-name=ubuntu,os-version=24.04 
        name: worker-01 
        replicas: 1 
        variables: 
          overrides: 
          - name: vmClass 
            value: a100-medium 
          - name: volumes 
            value: 
            - capacity: 100Gi 
              mountPath: /var/lib/kubelet 
              name: kubelet-dir 
              storageClass: paif-cls01-vsan-storage-policy 
            - capacity: 100Gi 
              mountPath: /var/lib/containerd 
              name: containerd-dir 
VMware by Broadcom
195


---

VMware Private AI Foundation with NVIDIA 9.1
              storageClass: paif-cls01-vsan-storage-policy
4. Provision the VKS cluster.
vcf cluster create -f gpu-vks-cluster-name.yaml
You can monitor cluster provisioning by running the following commands where gpu-accelerated-cluster is an example 
cluster name:
vcf cluster list
vcf cluster get gpu-accelerated-cluster
vcf cluster machinehealthcheck node get gpu-accelerated-cluster
vcf cluster machinehealthcheck control-plane get gpu-accelerated-cluster
5. Run the following commands to be able to log in to the VKS cluster.
vcf cluster register-vcfa-jwt-authenticator gpu-accelerated-cluster
vcf cluster kubeconfig get gpu-accelerated-cluster --export-file gpu-accelerated-cluster-kubconfig
kubectl --kubeconfig=gpu-accelerated-cluster-kubconfig get namespaces
Install and Configure the NVIDIA GPU Operator from the Local Registry
After you provision a VKS cluster using a vGPU-enabled VM class, you install the NVIDIA GPU Operator that supports 
vGPU devices. In a disconnected environment, you must additionally set up a local Ubuntu package repository and use 
the Harbor Registry that is associated with the Supervisor.
1. Log in to the VKS cluster.
vcf context create gpu-cluster --kubeconfig "gpu-accelerated-cluster-kubconfig" --kubecontext kubecontext-
from-kubeconfig
vcf context use gpu-cluster
2. Prepare the vGPU license token file.
echo -n "<your_client_configuration_token>" > client_configuration_token.tok
3. Prepare the index file for the local Ubuntu package repository.
For example, you can create the following custom-repo.list file in a text editor.
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble-updates main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble-security main universe
4. Export the details for the project on the private Harbor container registry as environment variables.
export REGISTRY_URI=<your_private_harbor_url_with_prefix>
export REGISTRY_USERNAME=<your_private_harbor_username>
export REGISTRY_PASSWORD=<your_private_harbor_password>
export registry_name=${REGISTRY_URI%%/*}
export registry_secret_name=ngc-secret
export gpu_operator_version="v25.3.1"
export vgpu_driver_version="570.158.01"
5. Prepare the Helm chart values template file for the GPU Operator.
For example, you can create the following gpu-operator-values-template.yaml file.
validator: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: gpu-operator-validator 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
operator: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: gpu-operator 
VMware by Broadcom
196


---

VMware Private AI Foundation with NVIDIA 9.1
  imagePullSecrets: 
  - ${registry_secret_name} 
  
driver: 
  repository: ${REGISTRY_URI}/images/nvidia/vgpu 
  version: ${vgpu_driver_version} 
  image: vgpu-guest-driver-5 
  imagePullSecrets: 
  - ${registry_secret_name} 
  repoConfig: 
    configMapName: "repo-config" 
  licensingConfig: 
    configMapName: "licensing-config" 
  manager: 
    image: k8s-driver-manager 
    repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  
toolkit: 
  repository: ${REGISTRY_URI}/images/nvidia/k8s 
  image: container-toolkit 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
devicePlugin: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: k8s-device-plugin 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
dcgm: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: dcgm 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
  
dcgmExporter: 
  repository: ${REGISTRY_URI}/images/nvidia/k8s 
  image: dcgm-exporter 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
gfd: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: k8s-device-plugin 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
migManager: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: k8s-mig-manager 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
nodeStatusExporter: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: gpu-operator-validator 
VMware by Broadcom
197


---

VMware Private AI Foundation with NVIDIA 9.1
  imagePullSecrets: 
  - ${registry_secret_name} 
  
gds: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: nvidia-fs 
  imagePullSecrets: 
  - name: ${registry_secret_name} 
  
gdrcopy: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: gdrdrv 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
vgpuManager: 
  driverManager: 
    image: k8s-driver-manager 
    imagePullSecrets: 
    - name: ${registry_secret_name} 
    repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  
vgpuDeviceManager: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: vgpu-device-manager 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
vfioManager: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: cuda 
  imagePullSecrets: 
  - ${registry_secret_name} 
  driverManager: 
    image: k8s-driver-manager 
    repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  
kataManager: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: k8s-kata-manager 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
sandboxDevicePlugin: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: kubevirt-gpu-device-plugin 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
ccManager: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: k8s-cc-manager 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
node-feature-discovery: 
  image: 
VMware by Broadcom
198


---

VMware Private AI Foundation with NVIDIA 9.1
    repository: ${REGISTRY_URI}/images/nfd/node-feature-discovery 
  imagePullSecrets: 
  - name: ${registry_secret_name}
6. Create the gpu-operator namespace, the image pull secret and all other required resources.
kubectl create namespace gpu-operator
kubectl label --overwrite ns gpu-operator pod-security.kubernetes.io/enforce=privileged
kubectl -n gpu-operator \ 
        create secret docker-registry "${registry_secret_name}" \ 
        --docker-server="${registry_name}" \ 
        --docker-username="${REGISTRY_USERNAME}" \ 
        --docker-password="${REGISTRY_PASSWORD}"
envsubst < gpu-operator-values-template.yaml > gpu-operator-values.yaml
touch gridd.conf
kubectl -n gpu-operator create configmap licensing-config \ 
        --from-file=gridd.conf \ 
        --from-file=client_configuration_token.tok
kubectl -n gpu-operator create configmap repo-config \ 
        --from-file=./custom-repo.list
7. Deploy the gpu-operator Helm chart by running the helm command.
helm -n gpu-operator \ 
     install gpu-operator oci://"${REGISTRY_URI}"/charts/gpu-operator \ 
     --version "${gpu_operator_version}" \ 
     -f ./gpu-operator-values.yaml
8. Monitor the installation progress and wait until all nine pods are running and are in a complete state.
kubectl get pods -n gpu-operator
Deploy an AI container image from the Harbor Registry associated with the Supervisor.
Provision a VKS Cluster with GPU Passthrough by Using the kubectl 
Command in a Connected VMware Private AI Foundation with NVIDIA
Environment
In VMware Private AI Foundation with NVIDIA, as a DevOps engineer, you provision a VKS cluster that uses NVIDIA GPU 
devices in passthrough mode in a namespace in an organization in VCF Automation. Then, you can deploy container AI 
workloads.
Verify with the organization administrator that the following prerequisites are in place for the AI-ready infrastructure.
•
VMware Private AI Foundation with NVIDIA is configured. See Preparing VCF for Private AI Workload Deployment.
•
You are added to a project and a namespace in your All Apps organization in VCF Automation.
•
You have the VCF Consumption CLI and kubectl installed on your machine. See Installing and Using VCF CLI in the
VCF documentation.
•
You have Helm installed on your machine.
•
You have the certificate of your VCF Automation instance saved to your machine, for example, as a
vcf_automation_certificate.pem file in your current working folder.
You use kubectl to deploy the VKS cluster in the namespace configured by the organization administrator.
Log in to the Namespace in Your Organization by using the VCF Consumption 
CLI
To deploy a GPU-accelerated VKS cluster, you use the kubectl CLI in the context of a namespace in your organization in
VCF Automation. The namespace is represented by a context that is created by using the VCF Consumption CLI.
VMware by Broadcom
199


---

VMware Private AI Foundation with NVIDIA 9.1
1. Generate an API token for your account in your VCF Automation organization.
a) Log in to your VCF Automation organization.
b) In the top-right corner, select User Settings > My Account from the account drop-down menu.
c) On the API Tokens tab, generate a token.
2. On the command line, log in to the namespace in your organization in VCF Automation by using the VCF Consumption 
CLI.
Use the --ca-certificate option if the VCF Automation certificate is not added to the truststore on your machine.
vcf context create vcf_automation_context_name --endpoint vcf_automation_fqdn_or_ip --ca-
certificate vcf_automation_certificate.pem --api-token vcf_automation_api_token --type cci
3. Install the cluster plug-in for the VCF Consumption CLI.
vcf plugin install cluster
4. Switch to the context for the target namespace in VCF Automation.
vcf context use vcf_automation_context_name:namespace_name:project_name
Provision a VKS Cluster with GPU Passthrough in a Connected Environment
After you enter the context for the allocated namespace in your organization in VCF Automation, you can proceed with 
provisioning a VKS cluster that uses GPU devices in passthrough mode.
1. Get the names of the custom VM classes in the namespace and identify those with GPU passthrough.
kubectl get virtualmachineclass
If you want to get the VM classes with GPU devices in passthrough mode, run the following command.
Linux
kubectl get virtualmachineclass -o json | jq -r '.items[] | 
 select(.spec.hardware.devices.dynamicDirectPathIODevices != null and 
 (.spec.hardware.devices.dynamicDirectPathIODevices | length) > 0) | .metadata.name'
Windows in PowerShell
$jqFilter = ".items[] | select(.spec.hardware.devices.dynamicDirectPathIODevices !
= null and (.spec.hardware.devices.dynamicDirectPathIODevices | length) > 0) 
 | .metadata.name"
kubectl get virtualmachineclass -o json | jq -r $jqFilter
2. Get the storage class for the namespace from the output of the following command.
kubectl describe namespaces namespace_name
The storage class name is available in the storage_class_name.storageclass.storage.k8s.io/
requests.storage resource name in the namespace_name-storagequota resource quota.
3. Create a YAML file for provisioning the VKS cluster using a VM class with GPU passthrough and a storage class.
An example YAML file, gpu-vks-cluster-name.yaml, contains the specification of a VKS cluster called gpu-
accelerated-cluster whose worker nodes must be deployed by using a VM class for two interconnected H100 
GPUs, h100-2xgpu-nvlink, and whose storage class is paif-cls01-vsan-storage-policy. The cluster 
requires at least VKS 3.4.1 and VKr 1.33.1 and its nodes are running Ubuntu 24.04.
For information about the latest VKS releases, see VMware vSphere Kubernetes Service Release Notes. For 
information about the latest VKr releases, see VMware VKr Release Notes.
apiVersion: cluster.x-k8s.io/v1beta1
kind: Cluster
metadata: 
  name: gpu-accelerated-cluster
spec: 
  clusterNetwork: 
    pods: 
      cidrBlocks: 
VMware by Broadcom
200


---

VMware Private AI Foundation with NVIDIA 9.1
      - 192.0.2.0/16 
    serviceDomain: cluster.local 
    services: 
      cidrBlocks: 
      - 198.51.100.0/12 
  topology: 
    class: builtin-generic-v3.4.0 
    version: v1.33.1---vmware.1-fips-vkr.2 
    controlPlane: 
      metadata: 
        annotations: 
          run.tanzu.vmware.com/resolve-os-image: os-name=ubuntu,os-version=24.04 
      replicas: 1 
    variables: 
    - name: vmClass 
      value: guaranteed-large 
    - name: storageClass 
      value: paif-cls01-vsan-storage-policy 
    # Uncomment this part if the VKS cluster must use  
    # a proxy server to connect to the NVIDIA Web site.  
    # - name: osConfiguration 
      # value: 
        # systemProxy: 
          # http: http://<fqdn_or_ip_address_of_http_proxy>/ 
          # https: http://<fqdn_or_ip_address_of_https_proxy>/ 
          # noProxy: [<subnet_CIDR_1>,<subnet_CIDR_2>,<subnet_CIDR_3>,<subnet_CIDR_4>] 
    workers: 
      machineDeployments: 
      - class: node-pool 
        metadata: 
          annotations: 
            run.tanzu.vmware.com/resolve-os-image: os-name=ubuntu,os-version=24.04 
        name: worker-01 
        replicas: 1 
        variables: 
          overrides: 
          - name: vmClass 
            value: h100-2xgpu-nvlink 
          - name: volumes 
            value: 
            - capacity: 100Gi 
              mountPath: /var/lib/kubelet 
              name: kubelet-dir 
              storageClass: paif-cls01-vsan-storage-policy 
           - capacity: 100Gi 
              mountPath: /var/lib/containerd 
              name: containerd-dir 
              storageClass: paif-cls01-vsan-storage-policy
4. Provision the VKS cluster.
vcf cluster create -f gpu-vks-cluster-name.yaml
You can monitor cluster provisioning by running the following commands where gpu-accelerated-cluster is an example 
cluster name:
vcf cluster list
vcf cluster get gpu-accelerated-cluster
vcf cluster machinehealthcheck node get gpu-accelerated-cluster
vcf cluster machinehealthcheck control-plane get gpu-accelerated-cluster
VMware by Broadcom
201


---

VMware Private AI Foundation with NVIDIA 9.1
5. Run the following commands to be able to log in to the VKS cluster.
vcf cluster register-vcfa-jwt-authenticator gpu-accelerated-cluster
vcf cluster kubeconfig get gpu-accelerated-cluster --export-file gpu-accelerated-cluster-kubconfig
kubectl --kubeconfig=gpu-accelerated-cluster-kubconfig get namespaces
Install the NVIDIA GPU Operator from the NVIDIA Helm Chart Repository
After you provision a VKS cluster using a VM class with GPU passthrough, you install a NVIDIA GPU Operator version 
that contains the NVIDIA data center GPU driver. This driver is required to run workloads that are accessing GPU on the 
VKS cluster.
You can have the NVIDIA data center GPU driver compiled during GPU Operator installation or use a precompiled GPU 
driver. For information on precompiled driver in the NVIDIA GPU Operator, see Precompiled Driver Containers in the 
NVIDIA documentation.
1. Log in to the VKS cluster.
vcf context create gpu-cluster --kubeconfig "gpu-accelerated-cluster-kubconfig" --kubecontext kubecontext-
from-kubeconfig
vcf context use gpu-cluster
2. Download the Helm chart file of the NVIDIA GPU Operator version that supports GPU passthrough, for example, 
NVIDIA GPU Operator v25.3.1.
helm fetch https://helm.ngc.nvidia.com/nvidia/charts/gpu-operator-v25.3.1.tgz
3. Create the gpu-operator namespace and label the namespace for the NVIDIA GPU Operator to set the 
enforcement policy to privileged.
kubectl create ns gpu-operator
kubectl label --overwrite ns gpu-operator pod-security.kubernetes.io/enforce=privileged
4. If you are using a proxy server, create a file with the proxy settings for driver installation, for example, env-proxy-
values.yaml.
driver: 
   env: 
   - name: HTTPS_PROXY 
     value: <your_https_proxy> 
   - name: HTTP_PROXY 
     value: <your_http_proxy> 
   - name: https_proxy 
     value: <your_https_proxy> 
   - name: http_proxy 
     value: <your_http_proxy>
5. Install the NVIDIA GPU Operator.
The following command installs an NVIDIA GPU Operator, compiling the NVIDIA data center GPU driver during 
installation.
helm install --wait gpu-operator -n gpu-operator ./gpu-operator-v25.3.1.tgz 
If your environment uses a proxy server for access to the Internet, use the following command.
helm install --wait gpu-operator -n gpu-operator ./gpu-operator-v25.3.1.tgz -f env-proxy-values.yaml
If you want to install the NVIDIA GPU Operator with a precompiled GPU driver container, use the following command.
helm install --wait gpu-operator -n gpu-operator ./gpu-operator-v25.3.1.tgz --set 
 driver.usePrecompiled=true --set driver.version="570"
6. Monitor the installation progress and wait until all pods are running and are in a complete state.
kubectl get pods -n gpu-operator
VMware by Broadcom
202


---

VMware Private AI Foundation with NVIDIA 9.1
Deploy an AI container image.
Provision a VKS Cluster with GPU Passthrough by Using the kubectl 
Command in a Disconnected VMware Private AI Foundation with 
NVIDIA Environment
In VMware Private AI Foundation with NVIDIA, as a DevOps engineer, by using the Kubernetes API, you provision a VKS 
cluster that uses NVIDIA GPU devices in passthrough mode.
Verify with the organization administrator that the following prerequisites are in place for the AI-ready infrastructure.
•
VMware Private AI Foundation with NVIDIA is configured for a disconnected environment. See Preparing VCF for 
Private AI Workload Deployment.
•
Verify that you can access the GPU-accelerated namespace in VCF Automation or that you have the namespace 
details from your organization administrator.
•
Download the certificate of your VCF Automation instance.
•
Provide a content library with Ubuntu TKr images is added to the vSphere namespace for AI workloads. See Configure 
a Content Library with Ubuntu VKr for a Disconnected VMware Private AI Foundation with NVIDIA Environment.
•
Provide a machine that has access to the Supervisor endpoint and to the local Harbor registry hosting the NVIDIA 
GPU Operator chart definitions.
•
Get the IP address of the local Ubuntu package repository from the administrator who set up the repository.
•
Contact your Harbor administrator for the URL and credentials for the Harbor project that contains the container 
images in the NVIDIA GPU Operator package.
•
By default, the resource mirroring utility downloads the components of NVIDIA GPU Operator 25.3.1 where the 
precompiled NVIDIA data center GPU driver version is 570.x. If you want to use another precompiled driver version 
according to the Ubuntu version on the VKS cluster nodes and the VKr version, contact your Harbor administrator to 
upload this version to the Harbor project.
Follow these steps to identify the precompiled driver version:
a. Identify the Linux kernel version used by the VKr image in the VMware VKr Release Notes.
b. On the NVIDIA GPU Driver container page, identify whether a pre-compiled driver container is available for this 
Linux kernel version.
For example, for the v1.34.1+vmware.1-vkr.4 VKr version and Ubuntu 24.04 with kernel version 6.8.0-86, 
download the 580-6.8.0-86-generic-ubuntu24.04 driver container.
•
To be able to retrieve available options or details on running entities precisely, install the jq utility on the machine that 
you use for provisioning the VKS cluster.
You use kubectl to deploy the VKS cluster in the namespace configured by the organization administrator.
Log in to the Namespace in Your Organization by using the VCF Consumption 
CLI
To deploy a GPU-accelerated VKS cluster, you use the kubectl CLI in the context of a namespace in your organization in
VCF Automation. The namespace is represented by a context that is created by using the VCF Consumption CLI.
1. Generate an API token for your account in your VCF Automation organization.
a) Log in to your VCF Automation organization.
b) In the top-right corner, select User Settings > My Account from the account drop-down menu.
c) On the API Tokens tab, generate a token.
2. On the command line, log in to the namespace in your organization in VCF Automation by using the VCF Consumption 
CLI.
Use the --ca-certificate option if the VCF Automation certificate is not added to the truststore on your machine.
vcf context create vcf_automation_context_name --endpoint vcf_automation_fqdn_or_ip --ca-
certificate vcf_automation_certificate.pem --api-token vcf_automation_api_token --type cci
VMware by Broadcom
203


---

VMware Private AI Foundation with NVIDIA 9.1
3. Install the cluster plug-in for the VCF Consumption CLI.
vcf plugin install cluster
4. Switch to the context for the target namespace in VCF Automation.
vcf context use vcf_automation_context_name:namespace_name:project_name
Provision the VKS Cluster with GPU Passthrough in a Disconnected 
Environment
After you enter the context for the allocated namespace in your organization in VCF Automation, you can proceed with 
provisioning a VKS cluster that uses GPU devices in passthrough mode.
1. Get the names of the custom VM classes in the namespace and identify those with GPU passthrough.
kubectl get virtualmachineclass
If you want to get the VM classes with GPU devices in passthrough mode, run the following command.
Linux
kubectl get virtualmachineclass -o json | jq -r '.items[] | 
 select(.spec.hardware.devices.dynamicDirectPathIODevices != null and 
 (.spec.hardware.devices.dynamicDirectPathIODevices | length) > 0) | .metadata.name'
Windows in PowerShell
$jqFilter = ".items[] | select(.spec.hardware.devices.dynamicDirectPathIODevices !
= null and (.spec.hardware.devices.dynamicDirectPathIODevices | length) > 0) 
 | .metadata.name"
kubectl get virtualmachineclass -o json | jq -r $jqFilter
2. Get the storage class for the namespace from the output of the following command.
kubectl describe namespaces namespace_name
The storage class name is available in the storage_class_name.storageclass.storage.k8s.io/
requests.storage resource name in the namespace_name-storagequota resource quota.
3. Create a YAML file for provisioning the VKS cluster using a VM class with GPU passthrough and a storage class.
An example YAML file, gpu-vks-cluster-name.yaml, contains the specification of a VKS cluster called gpu-
accelerated-cluster whose worker nodes must be deployed by using a VM class for two interconnected H100 
GPUs, h100-2xgpu-nvlink, and whose storage class is paif-cls01-vsan-storage-policy. The cluster 
requires at least VKS 3.4 and VKr 1.33.1 and its nodes are running Ubuntu 24.04.
For information about the latest VKS releases, see VMware vSphere Kubernetes Service Release Notes. For 
information about the latest VKr releases, see VMware VKr Release Notes.
apiVersion: cluster.x-k8s.io/v1beta1
kind: Cluster
metadata: 
  name: gpu-accelerated-cluster
spec: 
  clusterNetwork: 
    pods: 
      cidrBlocks: 
      - 192.0.2.0/16 
    serviceDomain: cluster.local 
    services: 
      cidrBlocks: 
      - 198.51.100.0/12 
  topology: 
    class: builtin-generic-v3.4.0 
    version: v1.33.1---vmware.1-fips-vkr.2 
    controlPlane: 
      metadata: 
VMware by Broadcom
204


---

VMware Private AI Foundation with NVIDIA 9.1
        annotations: 
          run.tanzu.vmware.com/resolve-os-image: os-name=ubuntu,os-version=24.04 
      replicas: 1 
    variables: 
    - name: vmClass 
      value: guaranteed-large 
    - name: storageClass 
      value: paif-cls01-vsan-storage-policy 
    # Uncomment this part if the VKS cluster must use  
    # a proxy server to connect to the NVIDIA Web site.  
    # - name: osConfiguration 
      # value: 
        # systemProxy: 
          # http: http://<fqdn_or_ip_address_of_http_proxy>/ 
          # https: http://<fqdn_or_ip_address_of_https_proxy>/ 
          # noProxy: [<subnet_CIDR_1>,<subnet_CIDR_2>,<subnet_CIDR_3>,<subnet_CIDR_4>] 
    workers: 
      machineDeployments: 
      - class: node-pool 
        metadata: 
          annotations: 
            run.tanzu.vmware.com/resolve-os-image: os-name=ubuntu,os-version=24.04 
        name: worker-01 
        replicas: 1 
        variables: 
          overrides: 
          - name: vmClass 
            value: h100-2xgpu-nvlink 
          - name: volumes 
            value: 
            - capacity: 100Gi 
              mountPath: /var/lib/kubelet 
              name: kubelet-dir 
              storageClass: paif-cls01-vsan-storage-policy 
           - capacity: 100Gi 
              mountPath: /var/lib/containerd 
              name: containerd-dir 
              storageClass: paif-cls01-vsan-storage-policy
4. Provision the VKS cluster.
vcf cluster create -f gpu-vks-cluster-name.yaml
You can monitor cluster provisioning by running the following commands where gpu-accelerated-cluster is an example 
cluster name:
vcf cluster list
vcf cluster get gpu-accelerated-cluster
vcf cluster machinehealthcheck node get gpu-accelerated-cluster
vcf cluster machinehealthcheck control-plane get gpu-accelerated-cluster
5. Run the following commands to be able to log in to the VKS cluster.
vcf cluster register-vcfa-jwt-authenticator gpu-accelerated-cluster
vcf cluster kubeconfig get gpu-accelerated-cluster --export-file gpu-accelerated-cluster-kubconfig
kubectl --kubeconfig=gpu-accelerated-cluster-kubconfig get namespaces
VMware by Broadcom
205


---

VMware Private AI Foundation with NVIDIA 9.1
Install the NVIDIA GPU Operator from the Local Repository
After you provision a VKS cluster using a VM class with GPU passthrough, you install the NVIDIA GPU Operator that 
contains the NVIDIA data center GPU driver. In a disconnected environment, you must additionally set up a local Ubuntu 
package repository and use the Harbor Registry for the Supervisor.
1. Log in to the VKS cluster.
vcf context create gpu-cluster --kubeconfig "gpu-accelerated-cluster-kubconfig" --kubecontext kubecontext-
from-kubeconfig
vcf context use gpu-cluster
2. If you plan to have the NVIDIA data center GPU driver compiled during GPU operator installation, prepare the index 
file for the local Ubuntu package repository.
For example, you can create the following custom-repo.list file in a text editor.
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble-updates main universe
deb [arch=amd64] http://local_package_repository_ip_address/ubuntu noble-security main universe
3. Export the details for the project on the private Harbor container registry as environment variables.
If you plan to use the precompiled NVIDIA data center GPU driver, set the following environment variables:
export REGISTRY_URI=<your_private_harbor_url_with_prefix>
export REGISTRY_USERNAME=<your_private_harbor_username>
export REGISTRY_PASSWORD=<your_private_harbor_password>
export registry_name=${REGISTRY_URI%%/*}
export registry_secret_name=ngc-secret
export gpu_operator_version="v25.3.1"
export gpu_driver_version="580"
If you plan to have the NVIDIA data center GPU driver compiled during GPU operator installation, set following 
environment variables:
export REGISTRY_URI=<your_private_harbor_url_with_prefix>
export REGISTRY_USERNAME=<your_private_harbor_username>
export REGISTRY_PASSWORD=<your_private_harbor_password>
export registry_name=${REGISTRY_URI%%/*}
export registry_secret_name=ngc-secret
export gpu_operator_version="v25.3.1"
export gpu_driver_version="580.95.05"
4. Prepare the Helm chart values template file for the GPU Operator.
For example, you can create the following gpu-operator-values-template.yaml file.
validator: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: gpu-operator-validator 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
operator: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: gpu-operator 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
driver: 
  repository: ${REGISTRY_URI}/images/nvidia/nvidia 
  version: ${gpu_driver_version} 
VMware by Broadcom
206


---

VMware Private AI Foundation with NVIDIA 9.1
  image: driver 
  imagePullSecrets: 
  - ${registry_secret_name} 
  manager: 
    image: k8s-driver-manager 
    repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  
toolkit: 
  repository: ${REGISTRY_URI}/images/nvidia/k8s 
  image: container-toolkit 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
devicePlugin: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: k8s-device-plugin 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
dcgm: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: dcgm 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
dcgmExporter: 
  repository: ${REGISTRY_URI}/images/nvidia/k8s 
  image: dcgm-exporter 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
gfd: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: k8s-device-plugin 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
migManager: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: k8s-mig-manager 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
nodeStatusExporter: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: gpu-operator-validator 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
gds: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: nvidia-fs 
  imagePullSecrets: 
  - name: ${registry_secret_name} 
  
gdrcopy: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
VMware by Broadcom
207


---

VMware Private AI Foundation with NVIDIA 9.1
  image: gdrdrv 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
vgpuManager: 
  driverManager: 
    image: k8s-driver-manager 
    imagePullSecrets: 
    - name: ${registry_secret_name} 
    repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  
vgpuDeviceManager: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: vgpu-device-manager 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
vfioManager: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: cuda 
  imagePullSecrets: 
  - ${registry_secret_name} 
  driverManager: 
    image: k8s-driver-manager 
    repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  
kataManager: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: k8s-kata-manager 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
sandboxDevicePlugin: 
  repository: ${REGISTRY_URI}/images/nvidia 
  image: kubevirt-gpu-device-plugin 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
ccManager: 
  repository: ${REGISTRY_URI}/images/nvidia/cloud-native 
  image: k8s-cc-manager 
  imagePullSecrets: 
  - ${registry_secret_name} 
  
node-feature-discovery: 
  image: 
    repository: ${REGISTRY_URI}/images/nfd/node-feature-discovery 
  imagePullSecrets: 
  - name: ${registry_secret_name}
5. Create the gpu-operator namespace, the image pull secret and all other required resources.
kubectl create namespace gpu-operator
kubectl label --overwrite ns gpu-operator pod-security.kubernetes.io/enforce=privileged
kubectl -n gpu-operator \ 
        create secret docker-registry "${registry_secret_name}" \ 
        --docker-server="${registry_name}" \ 
        --docker-username="${REGISTRY_USERNAME}" \ 
VMware by Broadcom
208


---

VMware Private AI Foundation with NVIDIA 9.1
        --docker-password="${REGISTRY_PASSWORD}"
envsubst < gpu-operator-values-template.yaml > gpu-operator-values.yaml
kubectl -n gpu-operator create configmap repo-config \ 
        --from-file=./custom-repo.list
6. Deploy the gpu-operator Helm chart by running the helm command.
helm -n gpu-operator \ 
     install gpu-operator oci://"${REGISTRY_URI}"/charts/gpu-operator \ 
     --version "${gpu_operator_version}" \ 
     --set driver.repoConfig.configMapName="repo-config" \ 
     -f ./gpu-operator-values.yaml
If you want to install the NVIDIA GPU Operator with a precompiled GPU driver container, use the following command.
helm -n gpu-operator \ 
     install gpu-operator oci://"${REGISTRY_URI}"/charts/gpu-operator \ 
     --version "${gpu_operator_version}" \ 
     -f ./gpu-operator-values.yaml \ 
     --set driver.usePrecompiled=true \ 
     --set driver.version="${gpu_driver_version}"
7. Monitor the installation progress and wait until all pods are running and are in a complete state.
kubectl get pods -n gpu-operator
Deploy an AI container image from the Harbor Registry to the Supervisor.
VMware by Broadcom
209


---

VMware Private AI Foundation with NVIDIA 9.1
Delivering Generative AI Applications by Using Private AI 
Services
You can use the Private AI Services capabilities in VMware Private AI Foundation with NVIDIA to develop and deliver 
generative AI applications including all required operations end-to-end in a model-as-a-service way.
Private AI Services includes the following modules:
Module
Description
Model Gallery in Harbor
As an MLOps engineer, you can distribute ML models across deep learning VMs, 
Kubernetes clusters, and RAG applications. See Storing ML Models in VMware Private 
AI Foundation with NVIDIA.
Model Runtime
As an MLOps engineer, you can run LLMs paired with inference engines as model 
endpoints. See Running Completion or Embedding Models by Using Model Endpoints.
You can also use models running in another Private AI Services instance. See Share 
a Model with Other Private AI Services Instances and Connect to a Shared Model 
Running in Another Private AI Services.
Using Models Running in the Cloud
You can use models running in the cloud platforms, such as Google Cloud Gemini 
Enterprise Agent Platform (formely Vertex AI), Google Cloud using the native Gemini 
API, and others. See Connect to a Remote Model Running in the Cloud.
Data Indexing and Retrieval
As a data scientist, you can connect to and index data from multiple sources so that 
you can use it in knowledge bases for contextual search in RAG applications using the 
PostgreSQL vector database of the Private AI Services instance. See Adding Context 
to Model Responses by Using Knowledge Bases.
MCP Servers and Tool Gallery
As an MLOps engineer, you can add data and capabilities in the form of MCP tools 
available on one or more MCP servers for use in agents and AI applications.
Private AI Services provides MCP integration that is based on tool calling. MCP 
prompts, sampling, or resources are not applicable to the use cases Private AI 
Services is designed for.
See Integrating Specialized Data and Capabilities over MCP.
Agent Builder
As an application developer, you can create an agent that uses running models from
Model Runtime with context from MCP tools and knowledge bases in Data Indexing 
and Retrieval and prompt tuning that you can access over an API. See Create an 
Agent for a Generative AI Application.
Observability
As an MLOps engineer or application developer, you can perform monitoring and 
diagnostics of models, which are used in agents, in observability platforms like 
Grafana to track the health, quality, and behavior of AI applications. See Observing the 
Performance of Private AI Services Components by Using Metrics.
Private AI Services Releases
Private AI Services is installed as a package, separately from the VMware Private AI Foundation with NVIDIA core 
functionality. A Private AI Services version supports specific versions of the platform components, PostegreSQL with 
pgvector extension, Inference servers, file types, and data source types. See VMware Private AI Services Release Notes.
Installing Private AI Services
Private AI Services must be installed as a Supervisor Service by your VI administrator and activated in a namespace in 
your VCF Automation organization by your organization administrator or directly on the Supervisor instance by your VI 
administrator. See Setup Workflows for Private AI Services.
Storing ML Models in VMware Private AI Foundation with NVIDIA
As an MLOps engineer, you can distribute ML models across deep learning VMs, Kubernetes clusters, and RAG 
applications in Private AI Services by using a central Harbor container registry.
VMware by Broadcom
210


---

VMware Private AI Foundation with NVIDIA 9.1
Using a central model gallery for your ML models has the following benefits:
•
Distribute models within your organization that you validated on a deep learning VM. Models on the Internet could 
contain malicious code or could be tuned for malicious behavior.
•
Distribute models for continuous delivery across organizations or promoting it between platforms or environments.
•
Reduce redundant copies of models that might later become redundant model endpoints.
•
Maintain model integrity in your controlled environment.
•
By using the Harbor project access capabilities, you can restrict access to sensitive data that is used to train and tune 
a model.
•
Store metadata in the Open Container Initiative (OCI) format describing the contents and dependencies of your ML 
models. In this way, you can identify the platforms that can run a target model.
You can use Harbor to store models from the NVIDIA NGC catalog, Hugging Face, and other ML model catalogs.
What Is a Model Gallery?
A model gallery in VMware Private AI Foundation with NVIDIA is a Harbor project with the following configuration:
Table 18: ML Model Gallery in Harbor
Mode Gallery Entity
Harbor or OCI Entity
Requirements
Model gallery
Project with configured user access
Must have a unique name in the Harbor 
registry.
Model
OCI repository
Must have a unique name in the project 
according to the OCI format.
Revision
OCI artifact
Immutable manifest that is identified by 
content digest.
If the same model data is pushed multiple 
times, only one revision is stored. If the 
model data changes, each push operation 
creates a new revision.
You can tag a model revision. Unlike in a 
container ecosystem, although supported, 
the latest tag is not the default option 
when pulling a model.
File
OCI layer and blob
-
For example, Harbor uses the following format for an example Llama model revision:
Pushing to and Pulling Models from a Model Gallery
In VMware Private AI Foundation with NVIDIA, you can use a deep learning VM or another machine to validate and fine 
tune models, downloaded from a public container registry or from a container registry in your organization. Then, you use 
the vcf pais command line utility on your machine or on the the deep learning VM to push to and pull models from and 
across model galleries in Harbor.
Install the vcf pais Commands
To be able to use the vcf pais commands on a machine different from a deep learning VM, you must install the pais
plug-in for the VCF Consumption CLI.
VMware by Broadcom
211


---

VMware Private AI Foundation with NVIDIA 9.1
1. On the machine running the VCF Consumption CLI and kubectl command utilities, run the following console 
command.
vcf plugin install pais
2. Verify that the pais plugin is included in the list of installed.
vcf plugin list
For information about the vcf pais commands, see Getting Started with Using the VCF Consumption CLI in the VCF
documentation.
What to Do Next
Storing models in a model gallery includes the following tasks:
•
Before You Begin Storing ML Models in VMware Private AI Foundation with NVIDIA
•
Validate and Upload an ML Model to a Model Gallery
•
Deploy an ML Model to Run Inference
Before You Begin Storing ML Models in VMware Private AI Foundation with 
NVIDIA
Storing ML models in Harbor requires the use of deep learning VMs that you can deploy by using VCF Automation or 
request from your organization administrator or DevOps engineer.
•
Verify that VMware Private AI Foundation with NVIDIA is configured up to this step of the deployment workflow for 
deep learning VMs and VKS clusters with GPU. See Setup Workflows for Deep Learning VMs and VKS Clusters with 
GPU or Setup Workflows for Private AI Services.
•
Verify that you have access to a Harbor registry with read-write permissions to a project in Harbor. See Set Up a Local 
Platform Harbor Registry for VMware Private AI Foundation with NVIDIA.
•
On the Harbor registry with the model gallery, create a robot account for the project that is allocated for the model 
gallery.
•
If your organization uses VCF Automation for AI development, verify that the following prerequisites are in place:
– Verify that your organization administrator has set up the VCF Automation catalog for private AI application 
deployment according to Set Up Your VCF Automation Organization for VMware Private AI Foundation with 
NVIDIA.
– Verify that your organization administrator has assigned the user role that is required for deploying deep learning 
VMs.
Validate and Upload an ML Model to a Model Gallery
As an MLOps engineer, you validate ML models that are onboarded against the security, privacy, and technical 
requirements of your organization. You can then upload the models to a dedicated model gallery for use by AI application 
developers or MLOps engineers for automated CI/CD based deployment of model runtimes.
1. Deploy a deep learning VM with a Triton Inference Server and open an SSH connection to it as vmware.
You can use one of the following workflows. As an MLOps engineer, you can directly deploy a database from VCF 
Automation. Otherwise, you request VM deployment from your cloud administrator or DevOps engineer.
Deployment Workflow
Required User Role
Description
Deploy by using a self-service catalog item 
in VCF Automation
MLOps engineer
See Deploy a Deep Learning VM with Triton 
Inference Server by Using a Self-Service 
Catalog Item in.
Deploy directly on a vSphere cluster.
VI administrator
See Deploy a Deep Learning VM Directly 
on a vSphere Cluster in.
Deploy by using the kubectl command.
DevOps engineer
See Deploy a Deep Learning VM by Using 
the kubectl Command in.
VMware by Broadcom
212


---

VMware Private AI Foundation with NVIDIA 9.1
If the model is hosted in Hugging Face, you can install the huggingface-cli command utility as part of the cloud-init 
script and use it to download open-weights models hosted on the Hugging Face Hub. Use the --local-dir flag to 
download the model without symbolic links so that the pais CLI can process the model.
2. Add the issuer certificate of the Harbor registry to the certificate trust store on the deep learning VM.
a) Request the CA certificate from the Harbor registry administrator.
b) Upload the certificate to the virtual machine, for example, by using a secure copy protocol (scp) client on your 
workstation.
For example:
scp infra/my-harbor-issuing-ca.crt vmware@dev-validation-dl-vm.mycompany.com:
c) Copy the certificate to the /usr/local/share/ca-certificates directory and add it to the trust store.
For example:
sudo cp my-harbor-issuing-ca.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
 
d) To save the changes, restart the docker service.
sudo systemctl restart docker
3. Download the model you plan to validate into the deep learning VM from the NVIDIA NGC catalog, Hugging Face, or 
another model hub.
4. Perform runtime tests in the deep learning VM as a sandbox.
a) Validate the integrity of the model by checking the hash code of the model files.
b) Scan the model files for malware.
c) Scan the model for serialization attacks.
d) Validate that inference works as expected by using the Triton Inference Server.
See Triton Inference Server.
e) Evaluate model's performance and safety for your business use case.
For example, you can examine inference requests for malicious behavior and perform hands-on functional tests.
5. Log in to the Harbor registry by using docker login.
docker login -u my_harbor_user_name my-harbor-repo-mycompany.com
6. From the directory that contains the model files, push the model to the Harbor project for the target organization.
For example, to upload a bge-small-en-v1.5 model from Beijing Academy of Artificial Intelligence (BAAI) to a store for 
AI application developers:
cd ./baai/bge-small-en-v1.5
vcf pais models push --modelName baai/bge-small-en-v1.5 --modelStore my-harbor-repo-mycompany.com/dev-
models --tag approved
Pushing again to the same modelName will create new revisions of the model which can be tagged according to your 
preference. Each revision is assigned a unique digest to maintain integrity.
7. List the models available in the model gallery and verify whether your model was uploaded.
vcf pais models list --modelStore my-harbor-repo-mycompany.com/dev-models
For a detailed example about uploading an LLM to a model gallery, see Onboarding Llama3 to the Private AI Model 
Gallery on VMware Blogs.
By using the vcf pais command-line utility in deep learning VM instances, copy models between model galleries to make 
them available across organizations or functional areas.
VMware by Broadcom
213


---

VMware Private AI Foundation with NVIDIA 9.1
Deploy an ML Model to Run Inference
After an ML model is validated and uploaded to the model gallery in your Harbor registry, an MLOps engineer can run that 
model for inference in Private AI Services or on a deep learning VM running a Triton Inference Server from NVIDIA NGC 
catalog.
For information about running a model in Private AI Services, see Running Completion or Embedding Models by Using 
Model Endpoints.
1. Deploy a deep learning VM with a Triton Inference Server and open an SSH connection to it as vmware.
You can use one of the following workflows. As an MLOps engineer, you can directly deploy a database from VCF 
Automation. Otherwise, you request VM deployment from your cloud administrator or DevOps engineer.
Deployment Workflow
Required User Role
Description
Deploy by using a self-service catalog item 
in VCF Automation
MLOps engineer
See Deploy a Deep Learning VM with Triton 
Inference Server by Using a Self-Service 
Catalog Item in.
Deploy directly on a vSphere cluster.
VI administrator
See Deploy a Deep Learning VM Directly 
on a vSphere Cluster in.
Deploy by using the kubectl command.
DevOps engineer
See Deploy a Deep Learning VM by Using 
the kubectl Command in.
If the model is hosted in Hugging Face, you can install the huggingface-cli command utility as part of the cloud-init 
script and use it to download open-weights models hosted on the Hugging Face Hub. Use the --local-dir flag to 
download the model without symbolic links so that the pais CLI can process the model.
2. Add the issuer certificate of the Harbor registry to the certificate trust store on the deep learning VM.
a) Request the CA certificate from the Harbor registry administrator.
b) Upload the certificate to the virtual machine, for example, by using a secure copy protocol (scp) client on your 
workstation.
For example:
scp infra/my-harbor-issuing-ca.crt vmware@dev-validation-dl-vm.mycompany.com:
c) Copy the certificate to the /usr/local/share/ca-certificates directory and add it to the trust store.
For example:
sudo cp my-harbor-issuing-ca.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
 
d) To save the changes, restart the docker service.
sudo systemctl restart docker
3. Log in to the Harbor registry by using docker login.
docker login -u my_harbor_user_name my-harbor-repo-mycompany.com
4. Pull the model you plan to run inference on.
vcf pais models pull --modelName baai/bge-small-en-v1.5 --modelStore my-harbor-repo-mycompany.com/dev-
models --tag approved
5. Create a model repository for the Triton Inference Server and start sending model inference requests.
See Create a Model Repository.
VMware by Broadcom
214


---

VMware Private AI Foundation with NVIDIA 9.1
Import a CA Trust Bundle in Private AI Services
As an MLOps engineer, to establish a trusted connection to an entity, add the issuer certificate for the entity to the 
certificate trust store of the Private AI Services instance in your namespace. You also use CA trust bundles to update 
certificate content.
You add CA trust bundles for the following entities:
•
Private Harbor registry for your model gallery,
•
Content management system for a data source in Data Indexing and Retrieval, such as Confluence or SharePoint.
•
MCP server with tools for providing context in agents.
•
If not provided during Private AI Services activation:
– OIDC provider that Private AI Services is using for authentication.
– Database server where Private AI Services stores vector embeddings for knowlege bases.
To update certificate content, delete the bundle with the old certificate and then add a bundle with the updated one.
1. Request the CA certificate from the administrator of the Harbor registry or the content management system.
You can use a certificate file or a ConfigMap file that contains the certificate.
2. If you are using a ConfigMap file, save it in the namespace where Private AI Services is available.
3. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
4. On the Service Configuration tab, in the CA Trust Bundles pane, click Add CA trust bundle.
5. If you are uploading a certificate file, select Upload CA Trust Bundle, enter a name for the bundle, select the file from 
your local file system, and click Add.
A ConfigMap containing the certificate with the name you entered is added to the namespace in VCF Automation and 
the Private AI Services is updated with the new ConfigMap.
6. If you have saved certificate information as a ConfigMap in the namespace after the Private AI Services was activated, 
click Select ConfigMap in Namespace, select the CA certificate entry, and click Add.
The Private AI Services is updated with the new ConfigMap resource.
Running Completion or Embedding Models by Using Model Endpoints
As an MLOps engineer, you can use Model Runtime to run models as model endpoints that can be used to create 
applications in Agent Builder and for indexing knowledge bases in Data Indexing and Retrieval. The models used in the 
endpoints are retrieved from your model gallery.
Activating Private AI Services in Your Namespace
To be able to run models in Model Runtime, Private AI Services must be activated in the selected namespace in your 
organization by your VI administrator or organization administrator. See Installing and Configuring Private AI Services.
Running Models on GPU or CPU Resources
You can run models on GPU or CPU, or only on CPU hardware resources according to the Private AI Services
configuration in the namespace.
VMware by Broadcom
215


---

VMware Private AI Foundation with NVIDIA 9.1
If Private AI Services is configured as GPU dependent, you must have a namespace with NVIDIA GPU devices either in 
vGPU or GPU passthrough as implemented by your VI administrator.
•
You run models on GPU by using the open-source vLLM inference engine for completion models.
•
You run embedding models by using the open-source Infinity inference engine on GPU or CPU.
•
You run embedding or completion models on CPU by using the open-source llama.cpp inference engine. Models must 
be available in the GGUF file format.
You can run models on CPU in a namespace that is configured as CPU-only or as GPU-accelerated.
What Is a Model Endpoint?
A model endpoint is an API endpoint for serving inference requests. The model is hosted in a container running on one 
or more VKS cluster nodes that are preferably GPU-enabled and deployed in a namespace you have access to in your 
organization for all applications in VCF Automation.
Table 19: Model Endpoint Attributes
Model Attribute
Description
Model URL
The URL to the model in your model gallery. For a model that you will run on CPU, the URL must 
point to a GGUF model.
See Storing ML Models in VMware Private AI Foundation with NVIDIA.
Routing name
Identifies the model in API calls. The naming convention is provider_name/model_id, for 
example, openai/gpt-oss-20b or google/gemma-3-1b.
Completion model type
Completion models generate text based on a given prompt 
without keeping a conversational history or provide chat 
capabilities.
•
Use with agents in Agent Builder in the same or another 
namespace in VCF Automation.
•
To run the model on GPU, use with the open-source vLLM 
inference engine.
•
To run the model on CPU, use with the llama.cpp inference 
engine.
•
Use over the OpenAI API. See Private AI Services REST API.
Model type and inference engine
Embedding model type
Embedding models map text into fixed-size vectors in a high-
dimensional space.
•
Use with knowledge bases in Data Indexing and Retrieval.
•
To run the model on GPU or CPU, use with Infinity.
•
To run the model on CPU, use with llama.ccp.
•
Use over the OpenAI API. See Private AI Services REST API.
Note:
If you want to use llama.cpp with NVIDIA GPU, upload the CUDA image of llama.cpp to an OCI registry, such as 
Harbor, and specify the image location during model endpoint deployment in the UI or using YAML.
Supported Models in Model Endpoints
The models that you plan to run must be supported by the inference engine.
•
Completion models supported by vLLM
•
Embedding models supported by Inifinity
•
Models supported by llama.cpp.
Selecting an Embedding Model
Choosing the right embedding model is important for the success of any natural language processing (NLP) application 
because it directly affects the quality of semantic representations and, consequently, the performance of downstream 
VMware by Broadcom
216


---

VMware Private AI Foundation with NVIDIA 9.1
tasks such as search, recommendation, and clustering. Consider the following best practices when selecting an 
embedding model:
•
Understand your use case and data
– Application requirements. What is the primary goal of your application? Are you looking for highly accurate semantic 
similarity, robust classification, or efficient retrieval? Different applications might benefit from different types of 
embeddings. For example, a search engine might prioritize models that capture fine-grained semantic distinctions 
while a sentiment analysis model might need embeddings that effectively represent emotional tone.
– Data characteristics. Analyze the nature of your data.
•
Domain specificity. Is your data general-purpose text, such as news articles or Wikipedia, or highly specialized, 
such as legal documents, medical records, and technical manuals? Models pre-trained on generic corpora might 
not perform well on domain-specific jargon or concepts.
•
Language. What languages is your data in? Ensure the model supports your target languages or is multilingual 
as needed.
•
Text length. Are you working with short phrases, sentences, paragraphs, or entire documents? Some models are 
optimized for shorter texts, while others excel at processing longer sequences.
•
Data size. The amount of data you have available for fine-tuning or training can influence your model choice. 
Larger datasets might enable more complex models, while smaller datasets might require pre-trained models 
with strong generalization capabilities.
•
Evaluate model architectures and pre-training objectives
– Sentence-BERT (only option). This is a modification of BERT specifically designed to produce semantically 
meaningful sentence embeddings that can be efficiently compared using cosine similarity. It is often a strong choice 
for tasks requiring sentence-level similarity.
•
Consider performance metrics and benchmarks
– Standard benchmarks. Consult established NLP benchmarks, such as GLUE, SuperGLUE, or MTEB (Massive Text 
Embedding Benchmark), to gain an initial understanding of a model's overall performance across various tasks. 
However, remember that these benchmarks might not perfectly reflect your specific use case. See Embedding 
Leaderboard on Hugging Face.
– Task-specific metrics. Evaluate models that are based on metrics relevant to your application.
– Semantic similarity. Cosine similarity, Euclidean distance, Spearman's correlation.
– Classification. Accuracy, F1-score, precision, and recall.
– Retrieval. Mean Average Precision (MAP), Recall@k, Precision@k.
– Computational Efficiency. Consider the speed of inference, memory usage, and the number of parameters. Larger 
models often offer better performance but come with higher computational costs. Performance is important for real-
time applications or resource-constrained environments.
•
Experimentation and iteration
– Empirical evaluation. The best way to select an embedding model is through empirical evaluation on your own data. 
Do not rely solely on benchmarks or theoretical considerations.
– A/B testing. If possible, implement A/B testing to compare the performance of different embedding models in a real-
world setting.
– Iterative refinement. Model selection is often an iterative process. Start with a strong baseline, evaluate its 
performance, and then experiment with different models, fine-tuning strategies, and hyperparameter adjustments to 
find the optimal solution.
By carefully considering these best practices, you can make an informed decision when selecting an embedding model 
that best suits your application's needs and contributes to its overall success.
Deploy a Completion or Embedding Model Endpoint
As an MLOps engineer, you can use Model Runtime to run models as model endpoints that can be used in application 
agents in Agent Builder and for indexing knowledge bases in Data Indexing and Retrieval, or that can be consumed 
directly by using the OpenAI API. The models used in the endpoints are retrieved from your model gallery in an OCI 
registry, such as Harbor.
•
Verify that the model image is stored in your model gallery in the OCI registry.
VMware by Broadcom
217


---

VMware Private AI Foundation with NVIDIA 9.1
•
If you plan to use a custom inference engine image, make sure that the image is stored in an image registry, such as 
Harbor.
•
Verify that the CA certificate of the Harbor registry with your model gallery is added to Private AI Services trust store.
•
If your registry requires authentication, verify that you have a robot account in the Harbor registry for pulling model 
images.
•
If you plan to run a model on CPU resources and llama.cpp, make sure that the Harbor registry with your model gallery 
contains the GGUF model.
•
Verify that the Secret for the robot account in the registry for pulling models is added to the VCF Automation 
namespace.
To add the secret, perform the following steps:
a. Create a YAML file with the Secret, for example, my-oci-registry-pull-secret.yaml.
apiVersion: v1
kind: Secret
metadata: 
  name: my-oci-registry-pull-secret-readonly
type: kubernetes.io/dockerconfigjson
data: 
    .dockerconfigjson:base-64-encoded-auths-for-harbor
base-64-encoded-auths-for-harbor represents the Base64 value of the following YAML code. There, the
auth property in its turn contains the robot$<project-name>+<robot-account-name>:<robot-account-
secret> pair in Base64 format.
{
"auths": { 
 "your_private_harbor_fqdn_or_ip_address": { 
   "auth": "base-64-encoded-robot$<project-name>+<robot-account-name>:<robot-account-secret>" 
   } 
  }
}
b. Run the following command to add the Secret to the namespace.
kubectl apply -f my-oci-registry-pull-secret.yaml
For information about the properties of model endpoints, see Table 19: Model Endpoint Attributes.
Note:
•
To use a model with MCP tools in an agent, the model must support tool calling.
•
Because knowledge bases in Data Indexing and Retrieval are accessible over MCP too, to use them in agents 
where a model with tool calling support is unavailable, you configure the agent to use static tool execution. See
Pre-Load Tool Results in the Context of an Agent.
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
VMware by Broadcom
218


---

VMware Private AI Foundation with NVIDIA 9.1
2. On the Local Models tab, click New Model Endpoint.
3. In the Deploy Model Endpoint wizard, enter the model URL in the model gallery and select the secret you have 
saved in the namespace, and click Next.
4. On the Model Specification page, enter the properties of the model and connected inference engine, and click Next.
a) Enter the properties of the model and connected inference engine according to the model type and GPU 
configuration of Private AI Services.
If your Private AI Services instance is CPU-only, the NVIDIA GPU Operator is not running on the VKS cluster for 
model endpoints. As a result, you cannot run models that require GPU resources.
b) If required by your organization, to customize the deployed model endpoint, turn on the Advanced Customization
toggle and provide some of the following configurations.
You can customize the deployed model in the following ways:
•
Use a custom image of the inference engine from an image registry, such as Harbor. In this way, for example, you 
can use a newer image of the engine that is not included in the Private AI Services releases or you can switch to 
the CUDA image of llama.cpp.
CAUTION:
Take into account the following considerations:
•
Version mismatch between the worker node and host drivers might occur.
•
Private AI Services sets certain command-line flags when running the inference engine that might not work for 
your use case.
•
Using arguments and flags to the inference engine
•
Overriding the values of environment variables for the inference engine
•
Using YAML code to override pod resource configuration
For information about the configuration options for the selected inference engine, see the documentation of the 
inference engine.
5. On the Deployment Configuration page, select the VM class and storage class for the Kubernetes cluster, and click
Next.
Setting
Description
VM class
Represents the CPU, memory, and reservations for the VMs 
created for the Kubernetes cluster nodes that will run the model.
•
For running a model on GPU, use a GPU-enabled VM 
class for running the inference engine. For example for 2 
interconnected NVIDIA H100 GPUs: h100-2xgpu-nvlin 
k.
•
For running a model on CPU, you can also use CPU-based 
VM classes for a certain model type. For example: guarante 
ed-4xlarge.
Number of instances
Number of instances, that is, replicas, of the model endpoint. All 
instances of a model endpoint run in a single vSphere Zone.
Storage class
Storage classes policies that describe different storage 
requirements and classes of services for the VMs created for the 
Kubernetes cluster nodes that will run the model.
6. Review the endpoint details and click Deploy model endpoint.
•
Use the created model over the Private AI Services REST API. See the Private AI Services API documentation.
•
If you have created a completion model endpoint, you can access the model's capabilities by the deploying an agent in
Agent Builder. See Create an Agent for a Generative AI Application.
•
If you have created an embedding model endpoint, you can use it for knowledge base indexing in Data Indexing and 
Retrieval . See Adding Context to Model Responses by Using Knowledge Bases.
VMware by Broadcom
219


---

VMware Private AI Foundation with NVIDIA 9.1
•
You can use the YAML code that is generated in UI for the endpoint deployment for automation based on the kubectl
command.
Deploy Completion or Embedding Model Endpoints by Using the kubectl 
Command
As an MLOps engineer, you can use the kubectl command to run models as model endpoints in Model Runtime
for automation purposes. The model endpoints can be used in application agents in Agent Builder and for indexing 
knowledge bases in Data Indexing and Retrieval, or over the OpenAI API. The models used in the endpoints are retrieved 
from your model gallery.
•
Verify that the model image is stored in your model gallery.
•
If you plan to use a custom inference engine image, make sure that the image is stored in an image registry, such as 
Harbor.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
If your registry requires authentication, verify that you have a robot account in the Harbor registry for pulling model 
images.
•
Verify that the CA certificate of the Harbor registry with your model gallery is added to the Private AI Services trust 
store.
•
You have the VCF Consumption CLI and kubectl installed on your machine. See Installing and Using VCF CLI in the
VCF documentation.
•
If you plan to deploy model endpoints in VCF Automation, verify that you have the certificate of your VCF Automation
instance saved to your machine, for example, as a vcf_automation_certificate.pem file in your current working 
folder.
•
If you plan to deploy model endpoints on a vSphere namespace directly on vSphere Supervisor, verify 
that you have the issuer certificate of the Supervisor instance saved to your machine, for example, as a
supervisor_ca_certificate.pem file in your current working folder.
•
If you plan to run a model on CPU resources and llama.cpp, make sure that the Harbor registry with your model gallery 
contains the GGUF model.
•
Verify that the Secret for the robot account in the registry for pulling models is added to the VCF Automation 
namespace.
To add the secret, perform the following steps:
a. Create a YAML file with the Secret, for example, my-oci-registry-pull-secret.yaml.
apiVersion: v1
kind: Secret
metadata: 
  name: my-oci-registry-pull-secret-readonly
type: kubernetes.io/dockerconfigjson
data: 
    .dockerconfigjson:base-64-encoded-auths-for-harbor
base-64-encoded-auths-for-harbor represents the Base64 value of the following YAML code. There, the
auth property in its turn contains the robot$<project-name>+<robot-account-name>:<robot-account-
secret> pair in Base64 format.
{
"auths": { 
 "your_private_harbor_fqdn_or_ip_address": { 
   "auth": "base-64-encoded-robot$<project-name>+<robot-account-name>:<robot-account-secret>" 
   } 
  }
}
b. Run the following command to add the Secret to the namespace.
kubectl apply -f my-oci-registry-pull-secret.yaml
For information about the properties of model endpoints, see Table 19: Model Endpoint Attributes.
VMware by Broadcom
220


---

VMware Private AI Foundation with NVIDIA 9.1
Note:
•
To use a model with MCP tools in an agent, the model must support tool calling.
•
Because knowledge bases in Data Indexing and Retrieval are accessible over MCP too, to use them in agents 
where a model with tool calling support is unavailable, you configure the agent to use static tool execution. See
Pre-Load Tool Results in the Context of an Agent.
1. Generate an API token for your account in your VCF Automation organization.
a) Log in to your VCF Automation organization.
b) In the top-right corner, select User Settings > My Account from the account drop-down menu.
c) On the API Tokens tab, generate a token.
2. On the machine running the VCF Consumption CLI and kubectl, create a context for the namespace where the 
consumerPrivate AI Services instance is available in VCF Automation or directly on vSphere Supervisor.
Use the --ca-certificate option if the VCF Automation or vSphere Supervisor certificate is not added to the 
truststore on your machine.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token <vcf_automation_api_token> --ca-certificate 
 <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --ca-
certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access to 
are created. An additional context is available as <context_name>:<namespace_name>.
3. Switch to the context for the target namespace.
 VCF Automation
vcf context use <context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <context_name>:<namespace_name>
4. Get the names of the custom VM classes in the namespace and identify those with vGPU profile.
kubectl get virtualmachineclass
5. Get the storage class for the namespace from the output of the following command.
kubectl describe namespaces namespace_name
The storage class name is available in the storage_class_name.storageclass.storage.k8s.io/
requests.storage resource name in the namespace_name-storagequota resource quota.
6. Review the ModelEndpoint CRD by running the following command.
kubectl describe crd modelendpoints.pais.vmware.com
7. Create a YAML file for deploying the model endpoint according to the format of the ModelEndpoint CRD.
For information about the main properties of a model endpoint, see Running Completion or Embedding Models by 
Using Model Endpoints.
You can customize the deployed model in the following ways:
•
Use a custom image of the inference engine from a registry, such as Harbor. In this way, for example, you can use 
a newer image of the engine that is not included in the Private AI Services releases or use the CUDA image of 
llama.cpp.
CAUTION:
Take into account the following considerations:
•
Version mismatch between the node and host drivers might occur.
VMware by Broadcom
221


---

VMware Private AI Foundation with NVIDIA 9.1
•
Private AI Services sets certain command-line flags when running the inference engine that might not work for 
your use case.
•
Using arguments and flags to the inference engine
•
Overriding the values of environment variables for the inference engine
•
Using YAML code to override pod resource configuration
For information about the configuration options for the selected inference engine, see the documentation of the 
inference engine.
You can also run the following command to see the properties you must include in the YAML file.
kubectl explain modelendpoint
Model on GPU
An example YAML file, mistral-modelendpoint.yaml, contains the model endpoint 
specification for running a Mistral completion model on a namespace with vGPU resources. The worker 
nodes for the model endpoint in the VKS cluster for Model Runtime are based on a vGPU-enabled VM 
class for allocating one H100 GPU, h100-1xgpu, and a storage class vsan-default-storage-
policy. The model is available for API calls under the mistralai/mistral-7b-instruct-
v0.3 routing name.
If you want to use a custom image of the vLLM inference engine, uncomment and set a value to the
spec.inferenceServerCustomization.engineImage properties.
apiVersion: pais.vmware.com/v1alpha1
kind: ModelEndpoint
metadata: 
  name: mistral-7b-instruct-v0.3--vllm
spec: 
  model: 
    ociRef: your_private_harbor_fqdn_or_ip_address/models/mistralai/mistral-7b-
instruct-v0.3:approved 
    pullSecrets: 
    # Optional, image pull secret if this OCI registry requires authentication 
    - name: my-oci-registry-pull-secret-readonly
  engine: vLLM 
  type: Completions 
  routingName: mistralai/mistral-7b-instruct-v0.3 
  virtualMachineClassName: h100-1xgpu 
  storageClassName: vsan-default-storage-policy 
  # All replicas of a model endpoint  
  # will run in a single vSphere Zone 
  replicas: 1 
  podResourceOverrides: 
    nvidia.com/gpu: "1" 
  # Set up according to the options supported  
  # by the inference engine 
  inferenceServerCustomization: 
    # Uncomment if you want to use a custom image of  
    # the inference engine. 
    # engineImage: your_private_harbor_fqdn_or_ip_address/project-name/image-
name:tag
    # Uncomment if the engine image is compressed.  
    # Compressed size is typically visible  
    # in the Web UI of container registries like Docker Hub,  
    # or you can use the 'docker manifest inspect' command.  
    # The full size of the /var/lib/containerd size  
    # on the VKS worker nodes is calculated as  
    # containerdMountSize = 32Gi + 3 * 
    engineImageCompressedSize 
    # engineImageCompressedSize: 10Gi 
VMware by Broadcom
222


---

VMware Private AI Foundation with NVIDIA 9.1
    # Add arguments to vLLM as needed. 
    # Each line starting with "-" represents a space 
    # in arguments to the inference engine. 
    cliArgs: 
    #! Enable tool-calling support (see https://docs.vllm.ai/en/latest/cli/serve/
#arguments) 
    - --enable-auto-tool-choice 
    - --tool-call-parser=mistral 
    #! Further example customizations 
    - "--dtype=half"  #! Required for V100 (Tesla generation) GPUs, but not for A30s 
   
    envVars: 
    - name: "VLLM_LOGGING_LEVEL" 
      value: "DEBUG"  #! The logging level for vLLM inference server  
                      # can be set by specifying VLLM_LOGGING_LEVEL env variable 
    # Determines the size of the /dev/shm mount point  
    # in the inference engine container. Default is 64Mi. 
    # sharedMemoryMountSize: 1Gi 
    # Determines the size in bytes of the /tmp mount point 
    # available on the inference server. Default is 1Gi. 
    # tempMountSize: 2Gi
Model on CPU
An example YAML file, gemma-llama-cpp-modelendpoint.yaml, contains the model endpoint 
specification for running a Gemma 3 completion model on a namespace with CPU-only resources 
or with allocated GPU resources which are required for other tasks. The worker nodes for the model 
endpoint in the VKS cluster for Model Runtime are based on a CPU-only VM class for allocating 16 
CPUs and 128 GB memory, best-effort-4xlarge, and a storage class vsan-default-
storage-policy. The model is available for API calls under the google/gemma-3-1b-it-
qat-q4_0 routing name.
If you want to use a custom image of the llama.cpp inference engine, uncomment and set a value to the
spec.inferenceServerCustomization.engineImage properties.
apiVersion: pais.vmware.com/v1alpha1
kind: ModelEndpoint
metadata: 
  name: google--gemma-3-1b-it-qat-q4-0--llama.cpp
spec: 
  model: 
    ociRef: your_private_harbor_fqdn_or_ip_address/models/google/gemma-3-1b-it-qat-
q4_0-gguf:approved 
    # Optional, image pull secret if this OCI registry requires authentication 
    pullSecrets: 
    - name: my-oci-registry-pull-secret-readonly
  engine: LlamaCPP 
  type: Completions 
  routingName: google/gemma-3-1b-it-qat-q4_0 
  virtualMachineClassName: best-effort-4xlarge 
  storageClassName: vsan-default-storage-policy 
  # All replicas of a model endpoint  
  # will run in a single vSphere Zone 
  replicas: 1 
  inferenceServerCustomization: 
    # Uncomment if you want to use a custom image of  
    # the inference engine. 
    # engineImage: your_private_harbor_fqdn_or_ip_address/project-name/image-
name:tag
    # Uncomment if the engine image is compressed.  
    # Compressed size is typically visible  
    # in the Web UI of container registries like Docker Hub,  
VMware by Broadcom
223


---

VMware Private AI Foundation with NVIDIA 9.1
    # or you can use the 'docker manifest inspect' command.  
    # The full size of the /var/lib/containerd size  
    # on the VKS worker nodes is calculated as  
    # containerdMountSize = 32Gi + 3 * 
    engineImageCompressedSize 
    # engineImageCompressedSize: 10Gi 
    # Add CLI arguments for llama.cpp as needed. 
    # Each line starting with "-" represents a space 
    # in arguments to the inference engine. 
    cliArgs: 
    - "--override-kv" 
    - gemma3.attention.sliding_window=int:512 
    - "--ctx-size" 
    - "16384" 
    - "--jinja" 
    # Determines the size of the /dev/shm mount point  
    # in the inference engine container. Default is 64Mi. 
    # sharedMemoryMountSize: 1Gi 
    # Determines the size in bytes of the /tmp mount point 
    # available on the inference server. Default is 1Gi. 
    # tempMountSize: 2Gi
8. Deploy the model endpoint.
Model on GPU
kubectl apply -f mistral-modelendpoint.yaml
Model on CPU
kubectl apply -f gemma-llama-cpp-modelendpoint.yaml
9. Verify that the model endpoint is created.
Locate the model endpoint in the output of the following command. Verify that the Ready status for the model endpoint 
is True.
kubectl get modelendpoints
•
Use the created model over the Private AI Services REST API. See the Private AI Services API documentation.
•
If you have created a completion model endpoint, you can access the model's capabilities by the deploying an agent in
Agent Builder. See Create an Agent for a Generative AI Application.
•
If you have created an embedding model endpoint, you can use it for knowledge base indexing in Data Indexing and 
Retrieval . See Adding Context to Model Responses by Using Knowledge Bases.
Troubleshooting Model Endpoint Deployments
After you deploy a model endpoint, you can troubleshoot its deployment by viewing the status of the model endpoint and 
the logs from the pods running the model endpoints.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that you have the VCF Consumption CLI and kubectl installed on your machine. See Installing and Using VCF 
CLI in the VCF documentation.
•
If you are deploying model endpoints on a namespace in a VCF Automation organization, verify that 
you have the certificate of your VCF Automation instance saved to your machine, for example, as a
vcf_automation_certificate.pem file in your current working folder.
•
If you are deploying model endpoints on a vSphere namespace directly on vSphere Supervisor, verify 
that you have the issuer certificate of the Supervisor instance saved to your machine, for example, as a
supervisor_ca_certificate.pem file in your current working folder.
VMware by Broadcom
224


---

VMware Private AI Foundation with NVIDIA 9.1
1. If you are deploying model endpoints on a namespace in a VCF Automation organization, generate an API token for 
your account in your VCF Automation organization.
a) Log in to your VCF Automation organization.
b) In the top right corner, select User Settings > My Account from the account drop-down menu.
c) On the API Tokens tab, generate a token.
2. On the machine running the VCF Consumption CLI and kubectl, create a context for the namespace where the 
consumerPrivate AI Services instance is available in VCF Automation or directly on vSphere Supervisor.
Use the --ca-certificate option if the VCF Automation or vSphere Supervisor certificate is not added to the 
truststore on your machine.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token <vcf_automation_api_token> --ca-certificate 
 <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --ca-
certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access to 
are created. An additional context is available as <context_name>:<namespace_name>.
3. Switch to the context for the target namespace.
 VCF Automation
vcf context use <context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <context_name>:<namespace_name>
4. Verify that the model endpoint is running.
Locate the model endpoint in the output of the following command. Verify that the Ready status for the model endpoint 
is True.
kubectl get modelendpoints
5. Save the kubeconfig output to a YAML file, for example, pais-kubeconfig.yml.
kubectl get secret $(kubectl get secret | grep pais'.*'kubeconfig | awk '{print $1}') -o 
 jsonpath='{.data.value}' | base64 -d > pais-kubeconfig.yml
6. Get the list of running model endpoints and their corresponding pods in the current namespace.
kubectl --kubeconfig pais-kubeconfig.yml get pods -L pais.vmware.com/modelendpoint.name
7. After you determinte which pod is runing the model endpoint you want to troubleshoot, review the logs for this pod in 
the pais namespace.
kubectl --kubeconfig pais-kubeconfig.yml -n pais logs -f <pod name>
VMware by Broadcom
225


---

VMware Private AI Foundation with NVIDIA 9.1
Generate API Tokens for VCF Automation or Local Private AI Services
Accounts
Starting with Private AI Services 3.0 and VCF 9.1.1, you can generate API tokens for authenticating to the Private AI 
Services API.
Verify that API token generation in the Private AI Services instance is turned on.
You can use API tokens for VCF Automation or local acccoutns for the following purposes:
•
Authenticate to the Private AI Services API from applications that support OpenAI-compatible endpoints, such as a 
Visual Studio Code extension on a developer machine or Open WebUI.
•
Access models running in another, provider, Private AI Services instance in a namespace in VCF Automation or 
directly on vSphere Supervisor.
•
Run commands in a VCF Automation namespace by using the VCF Consumption CLI.
For information about getting an API token from an external OIDC provider, see the documentation of the provider.
Note:
You cannot use an API token from external OIDC providers to access a model in another Private AI Services
instance.
See the Private AI Services API documentation for API-based access.
1. If you are using VCF Automation, generate an API token from your account's settings in the organization.
a) Log in to your VCF Automation organization.
b) In the top-right corner, select User Settings > My Account from the account drop-down menu.
c) On the API Tokens tab, generate a token and save it on your machine.
d) If required for your application, pass the generated token to a call to the Private AI Services API as
vcfa:<org>:<vcf-automation-user-api_token>.
For example, when you list the models that are running in a organization called pais-org and a Private AI 
Services instance accessible at https://pais.local:
curl \ 
    https://pais.local/api/v1/compatibility/openai/v1/models \ 
    -k \ 
    -H 'authorization: Bearer vcfa-pais-org-eyJhbGciOi...QrTIg'
{ 
  "object": "list", 
  "data": [ 
    { 
      "id": "BAAI/bge-en-icl", 
      "object": "model", 
      "created": 1787730008, 
      "owned_by": "", 
      "model_type": "EMBEDDINGS", 
      "model_engine": "OPENAI", 
      "status": "AVAILABLE" 
    }, 
    { 
      "id": "ggml-org/gpt-oss-20b-GGUF", 
      "object": "model", 
      "created": 1787730008, 
      "owned_by": "", 
      "model_type": "COMPLETIONS", 
      "model_engine": "OPENAI", 
      "status": "AVAILABLE" 
    } 
  ]
}
VMware by Broadcom
226


---

VMware Private AI Foundation with NVIDIA 9.1
2. If you are using a Private AI Services local account, generate an API token from the Private AI Services UI.
a) Log in to the Private AI Services UI at https://<private-ai-services_fqdn_or_ip_address>/#/api-
tokens with the local account.
b) Click your account in the top-right corner and select API tokens.
c) On the API tokens page, create a token and save it on your machine.
The format of the generated token is pais-<auth-provider-name-in-provider-namespace>-<token>. 
If you have used the Private AI Services UI to set up local accounts, the token has the following format pais-sc-
<token>.
d) If required for your application, pass the generated token to a call to the Private AI Services API.
For example, when you list the models that are running in a Private AI Services instance accessible at https://
pais.local authentication to the default authentication provider sc that is created by the Private AI Services UI:
curl \ 
    https://pais.local/api/v1/compatibility/openai/v1/models \ 
    -k \ 
    -H 'authorization: Bearer pais-sc-eyJhbGciOi...QrTIg'
{ 
  "object": "list", 
  "data": [ 
    { 
      "id": "BAAI/bge-en-icl", 
      "object": "model", 
      "created": 1787730008, 
      "owned_by": "", 
      "model_type": "EMBEDDINGS", 
      "model_engine": "OPENAI", 
      "status": "AVAILABLE" 
    }, 
    { 
      "id": "ggml-org/gpt-oss-20b-GGUF", 
VMware by Broadcom
227


---

VMware Private AI Foundation with NVIDIA 9.1
      "object": "model", 
      "created": 1787730008, 
      "owned_by": "", 
      "model_type": "COMPLETIONS", 
      "model_engine": "OPENAI", 
      "status": "AVAILABLE" 
    } 
  ]
}
•
Use the API token in calls to the Private AI Services API documentation.
•
In a consumer Private AI Services instance, include the API token in an InferenceGatewayRoute resource to connect 
to models that are running in this Private AI Services instance.
Share a Model with Other Private AI Services Instances
Starting with Private AI Services 3.0 and VCF 9.1, as an organization administrator in VCF Automation or as a VI 
administrator of vSphere Supervisor, you can share models running in a central, provider, Private AI Services instance 
with other organizations.
•
Verify that for authentication the Private AI Services instance uses VCF Automation or local accounts.
For more information on configuring, see Activate Private AI Services 3.0 in an Organization Namespace by Using the 
UI and Activate Private AI Services 3.0 in an Organization Namespace by Using the kubectl Command.
•
Deploy your initial set of model endpoints. See Deploy a Completion or Embedding Model Endpoint and Deploy 
Completion or Embedding Model Endpoints by Using the kubectl Command.
1. For a local account, get an API token secret that consumer namespaces can reference and share it with the 
administrator of the consumer Private AI Services instance.
VCF Automation users can directly generate their API tokens from the UI. See Generate API Tokens for VCF 
Automation or Local Private AI Services Accounts.
2. Share the issuer certificate for the base URL of the provider Private AI Services instance with the administrator of the 
consumer instance.
Connect to a Shared Model Running in Another Private AI Services
Starting with Private AI Services 3.0 and VCF 9.1.1, as an organization administrator in VCF Automation or a VI 
administrator in vSphere, you can connect a namespace to completion or embedding models running in another 
namespace in your VCF Automation instance or vSphere Supervisor.
•
Private AI Services must be active in the namespace you are working in.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that the Private AI Services provider instance is configured to use VCF Automation or local accounts for 
authentication.
•
Get the following information from the provider namespace owner:
– URL of the Private AI Services instance in the model sharing namespace
– Routing name of the model
– If the namespace is directly in vSphere Supervisor, an API token for consuming local accounts and the name of the 
provider .
If you are using VCF Automation, you can generate an API token on your own.
•
Get the issuer certificate for the base URL where models are available and add it as a CA trust bundle in your 
namespace. See Import a CA Trust Bundle in Private AI Services.
You can download the certificate by using the openssl utility or from your Web browser.
•
Verify that you have the VCF Consumption CLI and kubectl installed on your machine. See Installing and Using VCF 
CLI in the VCF documentation.
•
Verify that you have the issuer certificate of the component that manages the namespace where the model is running.
– Verify that you have the issuer certificate of your VCF Automation instance saved to your machine, for example, as 
a vcf_automation_certificate.pem file in your current working folder.
VMware by Broadcom
228


---

VMware Private AI Foundation with NVIDIA 9.1
– Verify that you have the issuer certificate of vSphere Supervisor to your machine, for example, as a
supervisor_ca_certificate.pem file in your current working folder.
•
If the models are accessible over VCF Automation, generate an API token for your account in your organization. See
Generate API Tokens for VCF Automation or Local Private AI Services Accounts.
You can also connect to a model running in the cloud. See Connect to a Remote Model Running in the Cloud.
Note:
Use VCF Automation in VCF 9.1.1 for organization-aware authentication.
1. On the machine running the VCF Consumption CLI and kubectl, create a context for the namespace where the 
consumerPrivate AI Services instance is available in VCF Automation or directly on vSphere Supervisor.
Use the --ca-certificate option if the VCF Automation or vSphere Supervisor certificate is not added to the 
truststore on your machine.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token <vcf_automation_api_token> --ca-certificate 
 <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --ca-
certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access to 
are created. An additional context is available as <context_name>:<namespace_name>.
2. Switch to the context for the target namespace.
 VCF Automation
vcf context use <context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <context_name>:<namespace_name>
3. Create an API token Secret for each consumer account.
 VCF Automation
The following example vcf-automation-token-credentials.yaml file contains Secrets 
for VCF Automation accounts. You generate vcf-automation-user-01_api_token and vcf-
automation-user-02_api_token in the consumer organization. The consumer Private AI Services
instance will use the tokens to authenticate requests to the remote models that are running in the 
provider instance.
# Secret 1: VCF Automation Account Token
apiVersion: v1
kind: Secret
metadata: 
  name: vcf-automation-user-01-secret
type: pais.vmware.com/api-token-credentials
stringData: 
  api_token: "vcfa:<consumer-org>:<vcf-automation-user-01_api_token>"
---
# Secret 2: VCF Automation Account Token
apiVersion: v1
kind: Secret
metadata: 
  name: vcf-automation-user-02-secret
type: pais.vmware.com/api-token-credentials
stringData: 
  api_token: "vcfa:<consumer-org>:<vcf-automation-user-02_api_token>"
vSphere Supervisor
The following example static-token-credentials.yaml file contains a Secret for a local 
account in the provider Private AI Services instance.
VMware by Broadcom
229


---

VMware Private AI Foundation with NVIDIA 9.1
The format of the api_token property is pais-<auth-provider-name-in-provider-
namespace>-<token>. For example, if the name of the authentication provider in the provider 
namespace is sc, which is the case when you create the initial local accounts in the Private AI Services
UI, set the property to pais-sc-<static_credentials_api_token>.
You can directly paste the token string generated in the Private AI Services UI.
# Static Credentials Account Token  
# (provider name in the sharing Private AI Services: "sc")
apiVersion: v1
kind: Secret
metadata: 
  name: local-user
type: pais.vmware.com/api-token-credentials
stringData: 
  api_token: "pais-sc-<api_token>"
4. Prepare a YAML file, for example, called inference-gateway-routes-consumer.yaml, in compliance with the 
InferenceGatewayRoute CRD.
For each remote model, create an InferenceGatewayRoute resource with the following configuration:
Configuration Area
YAML Section
Description
Resource name
metadata.name
The name for the model-specific InferenceGatewayRoute 
resource.
Base URL
spec.backend.httpBaseUrl
Enter the endpoint URL of the Private AI Services instance 
in that namespace.
https://<provider-private-ai-services-f 
qdn-or-ip-address>/api/v1/compatibility 
/openai
Model ID in the provider Private 
AI Services namespace or in the 
cloud
spec.backend.modelId
Set it to the routing name of the model endpoint in the 
provider Private AI Services instance.
Model endpoint name in the 
consumer Private AI Services 
instance
spec.matches.routingName
Enter a display name in the consumer namespace.
Model type
spec.type
•
To generate text based on a given prompt without 
keeping a conversational history or provide chat 
capabilities, select Completions.
•
To map text into fixed-size vectors in a high-
dimensional space, select Embeddings.
Inference engine
spec.engine
Set to OpenAI.
API token
spec.backend.auth.apiTokenRef.name
The name of the Secret containing the API token for an 
account in the consumer organization in VCF Automation
or a local account in the provider Private AI Services
instance.
TLS verification mode
spec.backend.tls.verification
•
If you want the consumer Private AI Services instance 
to verify if the certificate for the base URL is trusted 
without verifying the host name, set to caOnly.
•
If you want to skip certificate verification, set to none.
Note:
Use strict verification only with remote models 
running in the cloud.
The following example YAML code shares models with the VCF Automation users vcf-automation-user-01 and vcf-
automation-user-02.
VMware by Broadcom
230


---

VMware Private AI Foundation with NVIDIA 9.1
# vcf-automation-user-01 - Embeddings Model Route
apiVersion: pais.vmware.com/v1alpha1
kind: InferenceGatewayRoute
metadata: 
  name: baai--bge-small-en-v1.5--shared
spec: 
  type: Embeddings 
  engine: OpenAI 
  matches: 
    # Users will see the model in knowledge bases as BAAI/bge-small-en-v1.5-shared 
    routingName: BAAI/bge-small-en-v1.5-shared 
  backend: 
    httpBaseUrl: https://<private-ai-services-fqdn>/api/v1/compatibility/openai 
    # The model's routing name in the provider Private AI Services. 
    modelId: BAAI/bge-small-en-v1.5 
    tls: 
      verification: caOnly 
    auth: 
      apiTokenRef: 
        name: vcf-automation-user-01-secret
---
# vcf-automation-user-02 - Completion Model 1 Route
apiVersion: pais.vmware.com/v1alpha1
kind: InferenceGatewayRoute
metadata: 
  name: meta-llama-3-1-8b-instruct--shared
spec: 
  type: Completions 
  engine: OpenAI 
  # Users will see the model in agents as meta-llama/Meta-Llama-3.1-8B-Instruct-shared 
  matches: 
    routingName: meta-llama/Meta-Llama-3.1-8B-Instruct-shared 
  backend: 
    httpBaseUrl: https://<private-ai-services-fqdn>/api/v1/compatibility/openai 
    # The model's routing name in the provider Private AI Service 
    modelId: meta-llama/Meta-Llama-3.1-8B-Instruct 
    tls: 
      verification: caOnly 
    auth: 
      apiTokenRef: 
        name: vcf-automation-user-02-secret
---
# vcf-automation-user-02 - Completion Model 2 Route
apiVersion: pais.vmware.com/v1alpha1
kind: InferenceGatewayRoute
metadata: 
  name: ggml-gpt-oss-20b-gguf--shared
spec: 
  type: Completions 
  engine: OpenAI 
   # Users will see the model in agents as ggml-org/gpt-oss-20b-GGUF-shared 
  matches: 
    routingName: ggml-org/gpt-oss-20b-GGUF-shared 
  backend: 
    httpBaseUrl: https://pais-ingress-default.pais-tenant2.svc.cluster.local/api/v1/compatibility/openai 
VMware by Broadcom
231


---

VMware Private AI Foundation with NVIDIA 9.1
    # The model's routing name in the provider Private AI Service 
    modelId: ggml-org/gpt-oss-20b-GGUF 
    tls: 
      verification: caOnly 
    auth: 
      apiTokenRef: 
        name: vcf-automation-user-02-secret
5. Save the credentials Secrets in the namespace.
kubectl apply -f vcf-automation-token-credentials.yaml
6. Create the InferenceGatewayRoute resources in the namespace.
kubectl apply -f inference-gateway-routes-consumer.yaml
•
If you have connected to a completion model endpoint, you can access the model's capabilities by the deploying an 
agent in Agent Builder. See Create an Agent for a Generative AI Application.
•
If you have connected to an embedding model endpoint, you can use it for knowledge base indexing in Data Indexing 
and Retrieval . See Adding Context to Model Responses by Using Knowledge Bases.
Connect to a Remote Model Running in the Cloud
Starting with Private AI Services 3.0, as an organization administrator in VCF Automation or a VI administrator in vSphere, 
you can connect a namespace to completion or embedding models running in the cloud.
•
Get the following information from the service owner:
– The API base URL of the service endpoint on the cloud provider
– Identifier of the model according to the specifics of the cloud provider platform
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
If the issuer certifcate of a base URL where models are available is not trused, add the certificate as a CA trust bundle 
in your namespace. See Import a CA Trust Bundle in Private AI Services.
You can download the certificate by using the openssl utility or from your Web browser.
•
To be able to securely log in to VCF Automation or vSphere Supervisor using the VCF Consumption CLI, verify that 
you have the issuer certificate of the component that manages the namespace where the consuming Private AI 
Services is running.
– Verify that you have the issuer certificate of your VCF Automation instance saved to your machine, for example, as 
a vcf_automation_certificate.pem file in your current working folder.
– Verify that you have the issuer certificate of vSphere Supervisor to your machine, for example, as a
supervisor_ca_certificate.pem file in your current working folder.
Private AI Services supports the following cloud platforms:
•
Google Cloud Gemini Enterprise Agent Platform (formely Vertex AI)
•
Google Cloud using the native Gemini API
•
Google Cloud using its OpenAI compatibility API.
Note:
Google OpenAI compatibility layer does not support all features of the official OpenAI API.
•
Third-party platforms whose API endpoints are compatible with the OpenAI API.
For each model, you create an InferenceGatewayRoute resource with the following properties.
VMware by Broadcom
232


---

VMware Private AI Foundation with NVIDIA 9.1
Table 20: InferenceGatewayRoute Properties for Models in the Cloud
Configuration Area
YAML Section
Description
Resource name
metadata.name
The name for the model-specific InferenceGatewayRoute 
resource.
Base URL
spec.backend.httpBaseUrl
Enter the API base URL of the service endpoint.
Model ID in the cloud
spec.backend.modelId
Enter the model identifier according to the specifics of the 
cloud provider platform.
Model endpoint name in the 
consumer Private AI Services 
instance
spec.matches.routingName
Enter a display name in the consumer namespace.
Model type
spec.type
•
To generate text based on a given prompt without 
keeping a conversational history or provide chat 
capabilities, select Completions.
•
To map text into fixed-size vectors in a high-
dimensional space, select Embeddings.
Inference engine
spec.engine
Set the engine to one of the following values according to 
the model hosting platform.
•
GoogleVertexAI
•
GoogleGemini
•
GoogleOpenAI
•
OpenAI
API token
spec.backend.auth.apiTokenRef.name
The name of the Secret containing the API token or 
service account key JSON file according to the platform 
running the model.
TLS verification mode
spec.backend.tls.verification
•
If you want Private AI Services instance to verify if 
the certificate for the base URL is trusted and if the 
subject DNS name in the certificate is a valid one, set 
to strict.
•
If you want the Private AI Services instance to verify 
if the certificate for the base URL is trusted without 
verifying the host name, set to caOnly.
•
If you want to skip certificate verification, set to none.
1. On the machine running the VCF Consumption CLI and kubectl, create a context for the namespace where the 
consumerPrivate AI Services instance is available in VCF Automation or directly on vSphere Supervisor.
Use the --ca-certificate option if the VCF Automation or vSphere Supervisor certificate is not added to the 
truststore on your machine.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token <vcf_automation_api_token> --ca-certificate 
 <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --ca-
certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access to 
are created. An additional context is available as <context_name>:<namespace_name>.
2. Switch to the context for the target namespace.
 VCF Automation
vcf context use <context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <context_name>:<namespace_name>
VMware by Broadcom
233


---

VMware Private AI Foundation with NVIDIA 9.1
3. For models running on the Google Cloud Gemini Enterprise Agent Platform, create an API token Secret for a service 
account and InferenceGatewayRoute resource for a model that you can access with the service account.
For information about accessing models that are running on the Google Cloud Gemini Enterprise Agent Platform, see 
the Models section of the Gemini Enterprise Agent Platform documentation.
a) Create an API token Secret for each consumer account.
Use the full contents of the JSON file with your Google Cloud service account key. Private AI Services 
automatically detects that the value is a service-account key and exchanges it for a short-lived OAuth2 access 
token before each request, refreshing it transparently as it is about to expire.
For information about service accounts, see the Identity and Access Management section of the Google Cloud 
documentation.
The following example google-vertex-token-credentials.yaml file contains a Secret for a Google Cloud 
Gemini Enterprise Agent Platform service account.
apiVersion: v1
kind: Secret
metadata: 
  name: google-vertex-ai-service-account-key
type: pais.vmware.com/api-token-credentials
stringData: 
  api_token:  
    { 
      "type": "service_account", 
      "project_id": "<my_gcp_project_id>", 
      "private_key_id": "ae9464...", 
      "private_key": "-----BEGIN PRIVATE KEY-----\n...oqR5XUIzyNirIXCk6mJvc=\n-----END PRIVATE KEY-----
\n", 
      ... 
      "universe_domain": "googleapis.com" 
    }
b) Prepare a YAML file, for example, called inference-gateway-routes-google-vertex.yaml, in compliance 
with the InferenceGatewayRoute CRD and the model endpoint access properties in the Google Cloud Gemini 
Enterprise Agent Platform.
For each remote model, create an InferenceGatewayRoute resource according to Table 20: 
InferenceGatewayRoute Properties for Models in the Cloud.
The following example YAML code establishes a connection to the Google Gemini 3 Flash model.
apiVersion: pais.vmware.com/v1alpha1
kind: InferenceGatewayRoute
metadata: 
  name: google-gemini-3-flash-vertex
spec: 
  backend: 
    auth: 
      apiTokenRef: 
        name: google-vertex-ai-service-account-key 
    httpBaseUrl: https://aiplatform.googleapis.com/v1/projects/<my-gcp_project_id>/locations/global/
publishers/google 
    modelId: models/gemini-3-flash-preview 
    tls: 
      verification: strict 
  engine: GoogleVertexAI 
  matches: 
    routingName: google/gemini-3-flash-vertex 
  type: Completions
VMware by Broadcom
234


---

VMware Private AI Foundation with NVIDIA 9.1
c) Save the credentials Secrets in the namespace.
kubectl apply -f google-vertex-token-credentials.yaml
d) Save the credentials Secrets in the namespace.
kubectl apply -f inference-gateway-routes-google-vertex.yaml
4. For models available over the Google Gemini API or Google OpenAI API, create an API token Secret for an account 
and InferenceGatewayRoute resource for a model that you can access with the account.
a) Create an API token Secret for each consumer account.
The following example google-gemini-token-credentials.yaml file contains a Secret for a plain 
Google Gemini API key available in Google AI Studio. See Using Gemini API keys in the Google Gemini API 
documentation.
apiVersion: v1
kind: Secret
type: pais.vmware.com/api-token-credentials
metadata: 
  name: google-gemini-api-key
stringData: 
  api_token: <api_token>
b) Prepare a YAML file, for example, called inference-gateway-routes-google-gemini.yaml, in compliance 
with the InferenceGatewayRoute CRD and the model endpoint access properties in the Google Gemini API.
For each remote model, create an InferenceGatewayRoute resource according to Table 20: 
InferenceGatewayRoute Properties for Models in the Cloud.
Google Gemini API
The following example YAML code establishes a connection to the Google Gemini 3 Flash model.
See the Google Gemini API documentation for the relevant model.
apiVersion: pais.vmware.com/v1alpha1
kind: InferenceGatewayRoute
metadata: 
  name: google-gemini-3-flash-preview-gemini-openai
spec: 
  backend: 
    auth: 
      apiTokenRef: 
        name: google-gemini-api-key 
    httpBaseUrl: https://generativelanguage.googleapis.com/v1beta 
    modelId: models/gemini-3-flash-preview 
    tls: 
      verification: strict 
  engine: GoogleGemini 
  matches: 
    routingName: google/gemini-3-flash-gemini-native 
  type: Completions
Google OpenAI API
The following example YAML code establishes a connection to the Google Gemini 2.5 Flash model 
using the OpenAI compatibility layer provided by Google.
For information about accessing models in the Google Gemini API over OpenAI, see OpenAI 
compatibility in the Google Gemini API documentation.
apiVersion: pais.vmware.com/v1alpha1
kind: InferenceGatewayRoute
metadata: 
  name: google-gemini-2.5-flash-gemini-openai
spec: 
  backend: 
    auth: 
VMware by Broadcom
235


---

VMware Private AI Foundation with NVIDIA 9.1
      apiTokenRef: 
        name: google-gemini-api-key 
    httpBaseUrl: https://generativelanguage.googleapis.com/v1beta/openai 
    modelId: models/gemini-2.5-pro 
    tls: 
      verification: strict 
  engine: GoogleOpenAI 
  matches: 
    routingName: google/gemini-2.5-pro-gemini-openai 
  type: Completions
c) Save the credentials Secrets in the namespace.
kubectl apply -f google-gemini-token-credentials.yaml
d) Create the InferenceGatewayRoute resources in the namespace.
kubectl apply -f inference-gateway-routes-google-gemini.yaml
•
If you have connected to a completion model endpoint, you can access the model's capabilities by the deploying an 
agent in Agent Builder. See Create an Agent for a Generative AI Application.
•
If you have connected to an embedding model endpoint, you can use it for knowledge base indexing in Data Indexing 
and Retrieval . See Adding Context to Model Responses by Using Knowledge Bases.
Integrating Specialized Data and Capabilities over MCP
You can connect your generative AI application to real-time data and specialized capabilities over the Model Context 
Protocol (MCP). You can connect Private AI Services to MCP servers and set a list of approved tools for use in AI agents 
in Agent Builder.
For information about MCP, see the Model Context Protocol Web site.
Availability of MCP Server Integration
To be able to connect to external MCP server, Agent Builder must be active in your Private AI Services instance. See
Activate Private AI Services 3.0 in an Organization Namespace by Using the UI, Activate Private AI Services 3.0 in an 
Organization Namespace by Using the kubectl Command, and Enable the Full Set of Private AI Services 3.0 Capabilities 
by Using the UI.
In This Chapter
•
Connect an MCP Server to Private AI Services
•
Explore the MCP Tools Available to Private AI Services Agents
Connect an MCP Server to Private AI Services
To use special features in Agent Builder from outside the namespace with Private AI Services, you must first connect to 
the MCP servers that provide them.
•
Verify that Private AI Services is available in your namespace in VCF Automation or vSphere Supervisor.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that the target MCP server and the system it is connected to are up and running.
•
If you connect to the MCP server over HTTPS, verify that the issuer certificate for the server is added as a CA trust 
bundle. See Import a CA Trust Bundle in Private AI Services.
Private AI Services supports remote MCP servers with the following configuration:
•
Must be accessible over HTTP or HTTPS, supporting the Streamable HTTP or Server-Sent Events (SSE) transport.
•
Optionally use static authentication tokens.
Private AI Services provides MCP integration that is based on tool calling. MCP prompts, sampling, or resources are not 
applicable to the use cases Private AI Services is designed for.
VMware by Broadcom
236


---

VMware Private AI Foundation with NVIDIA 9.1
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
2. Click Agent Builder at the top of the page.
3. On the newly open page of the Private AI Services UI, click MCP > MCP Servers.
4. On the MCP Servers page, click Add server.
5. Add the following details for the MCP server, and click Add.
a) Enter a name and optionally a description.
b) Select the remote endpoint URL and transport for communication according to the capabilities of the server.
•
Streamable HTTP. The server operates as an independent process that can handle multiple client connections.
The URL has the http://<mcp_server_fqdn_or_ip_address>/mcp format.
•
Server-Sent Events (SSE). The server sends multiple server messages, communicating with the client over an 
SSE stream.
The URL has the http://<mcp_server_fqdn_or_ip_address>/sse or https://
<mcp_server_fqdn_or_ip_address>/sse format.
c) Include optional headers in the requests according to the requirements of the server.
For example, you can have a header User-Agent set to MCP-Client/1.0 and a credential header
Authorization set to Apikey 6833f8df3eba486d804e32664252574c.
You cannot modify header keys or values. To modify a header, delete it and create it again with updated key or 
value.
6. Examine the capabilities and parameters of tools that are provided by the server and approve those of them which are 
suitable and safe for use in agents for your organization.
•
Use approved tools in agents in Agent Builder. See Create an Agent for a Generative AI Application.
•
Change the settings for the MCP server or test the connection to the server.
Explore the MCP Tools Available to Private AI Services Agents
In the tool gallery of Private AI Services, you can examine the collection of tools that are provided by the connected MCP 
servers and Data Indexing and Retrieval and approved.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that Private AI Services is available in your namespace in VCF Automation or vSphere Supervisor.
•
Verify that the connection to added MCP servers.
Data Indexing and Retrieval implements an MCP server which exports a set of search tools for each knowledge base.
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
VMware by Broadcom
237


---

VMware Private AI Foundation with NVIDIA 9.1
2. Click Agent Builder at the top of the page.
3. On the newly open page of the Private AI Services UI, click MCP > Tools Gallery.
4. Select a tool to view its capabilities including providing MCP server, agents using it, and description including input and 
output schemas.
Adding Context to Model Responses by Using Knowledge Bases
As a data scientist, you can use Data Indexing and Retrieval to include data from multiple sources for introducing context 
to the model-based search for RAG applications that use an agent in Agent Builder.
Knowledge bases are linked to data sources with documents published on a shared location or local files and have 
indexes saved in the vector database of the Private AI Services instance. You can attach a knowledge base to an agent 
for a RAG application to help narrow the context for the completion LLM associated with agent.
Data source
Represents a reference to data in its original location. A data source stores the credentials that are required 
for retrieving data from this location when a knowledge base is being indexed.
Knowledge base
Consists of metadata, text chunks, and vector embeddings created for each processed document in the 
scope of a data source.
You can provide documents to a knowledge base by using linked data sources or files locally stored in your 
machine. A knowledge base can be linked to multiple data sources of multiple types.
Indexing a knowledge base includes splitting the input content into chunks of text and generating an 
embedding for each chunk using an embedding model endpoint in Model Runtime. Data sources can be 
automatically indexed according to a refresh policy or indexing can started manually.
If the linked data sources are large or the GPU resources in the namespace for Private AI are insufficient, 
indexing a knowledge base might take hours. To enable developers to experiment with their data as soon 
as possible in such cases, Data Indexing and Retrieval makes chunks available for retrieval as they are 
processed.
Supported file formats
You can upload PDF, DOCX, PPTX, TXT, HTML, MD, and CSV files. You can also use Google Workspace 
for Google Docs, Google Slides, and Google Sheets.
Information Considered During an Indexing Operation
Indexing a knowledge base includes the following set of tasks.
•
Number of documents
– Related to data sources
•
Number of crawled documents
– Related to a knowledge bases:
•
Number of new documents found
•
Number of deleted documents
•
Number of existing and unmodified documents
•
Number of existing but modified documents (embeddings update required)
•
Number of skipped documents (file type not supported)
•
Number of failed-to-download documents
•
Number of failed embeddings generations
•
Total number of processed documents including crawled and deleted
•
Number of documents in the index PRE-run
•
Number of documents in the index POST-run
•
Time
– Total time for downloading files
– Total time for generating embeddings
– Total time of the index operation
•
Size
– Total MBs downloaded
VMware by Broadcom
238


---

VMware Private AI Foundation with NVIDIA 9.1
– Number of new chunks indexed (equivalent to the number of invokes to the inference server for `/embeddings`)
– Final index size (size of the LlamaIndex DB table at the end once the removed document embeddings are deleted)
•
Other
– Document counter for times returned by retrieval query
– Number of documents per media type, that is PDF, PPTX, and so on, in the knowledge base.
Add a Data Source for a Knowledge Base
For production use cases, upload the files for your knowledge bases to a content management system and create data 
sources that you can link to the knowledge bases.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that Private AI Services is available in your namespace in VCF Automation.
•
Verify that the content management system is supported and that you have credentials for access to the content 
location.
Table 21: Required Setup for Data Sources in Data Indexing and Retrieval
Data Source Type
Data Source Properties
Special Requirements for Setup and Credentials
Google Drive folder
•
URL to a Google Drive 
folder
•
Service account key file in 
JSON format
Work with your Google Cloud organization administrator to implement 
the following configuration:
1.
In a Google Cloud project, create a service account with no 
assigned roles.
See Create a service account in the Google Cloud documentation.
2.
Generate and download a service key file in JSON format.
See Create and delete service account keys in the Google Cloud 
documentation.
3.
Activate the Google Drive API in the project.
See Enabling an API in your Google Cloud project.
4.
Share the Google Drive folder with the files for the data source with 
the service account as view-only.
5.
Use the information from the service account key file during data 
source creation.
Confluence space or 
individual pages
•
URL to a Confluence site
•
Space key or a list of 
page IDs
•
User name and password, 
or a personal access 
token
For authentication, you use a user name and password or a personal 
access token, according to the configuration made by your Confluence 
administrator. You generate a personal access token from the settings 
of your account.
Microsoft SharePoint site
•
Site URL
•
Subsites and folders
•
User name and password
•
You can add data sources only for a SharePoint On-Premises.
•
Supported authentication is by using a user name and password.
Amazon S3 compatible store
•
URL to the store in the 
http://host:port format.
•
Access key and password 
as secret key
•
Bucket name that 
contains the files for the 
data source
•
Optional prefix of the file 
names that should be 
indexed
Contact your S3 administrator on the information required to add a data 
source. The access key must have permissions to list and read objects 
from the specified S3 bucket.
For example, you can create a data source for a specific information area, for example, HR, Marketing, Finance, and 
similar.
VMware by Broadcom
239


---

VMware Private AI Foundation with NVIDIA 9.1
You can link one data source to several knowledge bases.
After a data source is created, you cannot change its type.
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
2. Click Data Indexing and Retrieval at the top of the page.
3. On the newly-open Data Indexing and Retrieval page, click Data Sources.
4. On the Data Sources page, click Add data source.
5. Add a name and description of the data source, and click Next.
6. Select the data source type that corresponds to the location that stores the source files and enter the required 
properties, and click Next.
7. Review the data source details and click Add data source.
Link the data source to a knowledge base.
Create a Knowledge Base with Linked Data Sources
By using a knowledge base with linked data sources, you can have data updates automatically collected and indexed over 
a period of time. You also use data that is stored on an enterprise-ready content management system.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that the required embedding model endpoint is running in Model Runtime.
You can link up to 10 data sources to a knowledge base.
The embeddings, generated by the indexing operation, are saved in the vector database of the Private AI Services 
instance in the namespace.
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
VMware by Broadcom
240


---

VMware Private AI Foundation with NVIDIA 9.1
2. Click Data Indexing and Retrieval at the top of the page.
3. On the Data Indexing and Retrieval page, click Knowledge Bases .
4. Create a knowledge base.
a) On the Knowledge Bases page, click Create knowledge base.
b) In the Create Knowledge Base dialog box, enter name and description of the knowledge base, set Knowledge 
base type to Create from data source.
c) Use Index refresh policy to configure the frequency with which updates in the linked data sources are collected 
and an index operation is performed.
If you want to manually index the knowledge base, for example, to save GPU resources, select the manual refresh 
policy.
d) Click Create.
5. Link data sources to the knowledge base.
a) On the knowledge base page, click Link data source.
b) In the Link Data Sources dialog box, select the data sources for the knowledge base and click Add.
6. After you have linked all required data source, configure the indexing operation and index the knowledge base for the 
first time.
For guidance on the values for the index settings according to the embedding model, see Embedding Leaderboard on 
Hugging Face and LlamaIndex documentation.
After you configure indexing, you cannot change its configuration. If you want to process the same data differently, 
create another knowledge database with the same data sources and configure indexing as required.
a) On the knowledge base page, click Index knowledge base.
b) Select the embedding endpoint model running in Model Runtime, set values to the indexing settings, and click
Start index.
•
If you have selected the manual index refresh policy or if you must incorporate a change in the linked data sources 
sooner than the period in the index refresh policy, index the knowledge base manually. Because indexing is a GPU-
intensive operation, index the knowledge base after all linked data sources are added and input content is updated in 
its original location.
•
Use the knowledge base as a set of search MCP tools in a chatbot agent in Agent Builder.
Create a Knowledge Base with Local Files
By using a knowledge base with local files, you can test your RAG applications during development by uploading files 
from your local file system. Using a content platform is not required.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that the required embedding model endpoint is running in Model Runtime.
The embeddings, generated during indexing, are saved in the vector database for the Private AI Services instance in the 
namespace.
Indexing is a GPU-intensive operation.
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
VMware by Broadcom
241


---

VMware Private AI Foundation with NVIDIA 9.1
2. Click Data Indexing and Retrieval at the top of the page.
The Private AI Services UI for Data Indexing and Retrievale and Agent Builder opens.
3. Log in to the Data Indexing and Retrieval UI.
4. On the Data Indexing and Retrieval page, click Knowledge Bases .
5. Create a knowledge base.
a) On the Knowledge Bases page, click Create knowledge base.
b) In the Create Knowledge Base dialog box, enter name and description of the knowledge base, set Knowledge 
base type to Create from local files, and click Create.
6. Configure the index operation.
For guidance on the values for the index settings according to the embedding model, see Embedding Leaderboard on 
Hugging Face and LlamaIndex documentation.
After you configure indexing, you cannot change its configuration. If you want to process the same data differently, 
create another knowledge database with the same local files and configure indexing as required.
a) On the knowledge base page, click Configure Index Settings.
b) Select the embedding endpoint model running in Model Runtime, set values to the index operation settings, and 
click Save settings.
7. Upload local files the knowledge base.
a) On the knowledge base page, click Add local files.
b) Locate all files with input content in your local file system and click Start upload.
As soon as a file is upload, the index operation for is started in parallel with uploading the rest of the files.
c) Wait until all files are indexed.
•
Add files to the knowledge base. The index operation is automatically performed every time you upload files.
•
Use the knowledge base as a set of search MCP tools in a chatbot agent in Agent Builder.
Considerations When Modifying and Indexing Knowledge Bases
Certain considerations exist when you add or remove a data source from a knowledge base, or when you perform an 
operation that impacts indexing that is in progress.
•
If you add a data source to a knowledge base after initial index generation, the data in the data source is collected for 
indexing during the next scheduled indexing or if you run indexing manually.
•
Unlinking data sources
– If you unlink a data source from a knowledge base during an index operation, Data Indexing and Retrieval will finish 
processing the documents that are already in progress and it will not proceed to the remaining documents from the 
data source. The relevant chunks will be removed from the database during the next indexing operation.
– If you unlink a data source, the relevant chunks are removed during the next scheduled index operation or if you run 
indexing manually.
– If you must fully remove the data of a data source that is linked to multiple knowledge bases, in each knowledge 
base, unlink the data source and run indexing manually or wait for the next scheduled indexing.
•
If you cancel an indexing operation, Data Indexing and Retrieval will finish processing the documents that are already 
in progress and it will not proceed to the remaining documents. As a result, the indexing operation will complete after 
some time.
Cancel an Indexing Job Immediately
Sometimes, canceling an indexing job in the Data Indexing and Retrieval UI might not result in cancelled the job 
immediately. To cancel an indexing job immediately, use the Private AI Services REST API. For more information on the 
API, see the Indexes section of the Private API documentation.
1. Get the knowledge base ID from the VCF Automation UI.
a. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere 
Client.
VMware by Broadcom
242


---

VMware Private AI Foundation with NVIDIA 9.1
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is 
available at https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
b. Click Data Indexing and Retrieval at the top of the page.
c. On the Data Indexing and Retrieval page, click Knowledge Bases and open the knowledge base where 
cancelling indexingis still in progress.
d. Copy the knowledge base ID from the https://<private_ai_services_fqdn_or_ip_address>/#rex/
knowledge-based/<knowledge-base-id> URL in the browser's address bar.
2. List all indexes of the knowledge base by making the following API call.
For information on the required Access Token, see the Getting Started section of the Private API documentation.
curl -X 'GET' 'https://<private_ai_services_fqdn_or_ip_address>/api/v1/control/knowledge-
bases/<knowledgebase-id>/indexes' \ 
  -H 'accept: application/json' \ 
  -H 'Authorization: Bearer <access_token>' \ 
  -d ''
You get the following example response that contains the properies of the index where the indexing job is to be 
cancelled. Locate the index ID in the response.
{ 
  "object": "list", 
  "has_more": false, 
  "num_objects": null, 
  "data": [ 
    { 
      "id": "<index_id>", 
      "name": null, 
      "description": null, 
      "embeddings_model_endpoint": "sentence-transformers/all-minilm-l6-v2", 
      "text_splitting": "SENTENCE", 
      "chunk_size": 256, 
      "chunk_overlap": 25, 
      "knowledge_base_id": "<knowledge_base_id>", 
      "object": "index", 
      "created_at": 1751989726, 
      "last_indexed_at": 1756226122, 
      "last_indexed_by_id": null, 
      "num_documents": 663, 
      "status": "AVAILABLE", 
      "status_errors": null 
    } 
  ], 
  "first_id": "<ID of the first index in the list>", 
  "last_id": "<ID of the last index in the list>"
}
3. Get the active indexing job for the index by making the following API call using the knowledge base ID and the index 
ID.
curl -X 'GET' 'https://<private_ai_services_fqdn_or_ip_address>/api/v1/control/knowledge-
bases/<knowledgebase-id>/indexes/<index-id>/active-indexing' \ 
  -H 'accept: application/json' \ 
  -H 'Authorization: Bearer <access_token>' \ 
  -d ''
VMware by Broadcom
243


---

VMware Private AI Foundation with NVIDIA 9.1
You get the following example response that contains the properies of the active indexing job. Locate the ID of the 
active indexing job.
{ 
  "id": "<indexing_id>", 
  "state": "CANCELLING", 
  "created_at": 1756226122, 
  "knowledge_base_id": "<knowledge_base_id>", 
  "index_id": "<index_id>", 
  "completed_at": null
}
4. Force the cancellation of the indexing job by making the following API call using the knowledge base ID, the index ID, 
and the ID of the indexing job.
curl -X 'POST' 'https://<private_ai_services_fqdn_or_ip_address>/api/v1/control/knowledge-
bases/<knowledgebase-id>/indexes/<index-id>/indexings/<indexing-id>/cancel?force=true' \ 
  -H 'accept: application/json' \ 
  -H 'Authorization: Bearer <access_token>' \ 
  -d ''
You get the following example response that shows that the indexing job has been cancelled.
{ 
  "id": "<indexing-id>", 
  "state": "CANCELLED", 
  "created_at": 1756226122, 
  "knowledge_base_id": "<knowledgebase-id>", 
  "index_id": "<indexing-id>", 
  "completed_at": null
}
Create an Agent for a Generative AI Application
As an application developer, for prompt tuning and testing, you can create an agent in Agent Builder to use a completion 
model running in Model Runtime and communicate with MCP tools. For a RAG application, you can combine the model 
with one or more knowledge bases from Data Indexing and Retrieval.
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that Private AI Services is available in your namespace in VCF Automation.
•
Verify that you have a completion model endpoint running in Model Runtime.
•
If you plan to use MCP tools, use a model endpoint that supports tool calling.
Otherwise, for an agent that is configured with knowledge bases, you can activate static tool invocation after you 
create the agent. See Pre-Load Tool Results in the Context of an Agent.
•
If you plan to add an MCP tool, verify that the tool is approved and visible in the tool gallery.
•
If you plan to add a knowledge base to the agent, verify that the knowledge base is created in Data Indexing and 
Retrieval.
The agent calls APIs to send the input from the prompt to the model endpoint and uses data from MCP tools to introduce 
context into generated responses. The model selects MCP tools according to their description and provided instructions 
and the agent calls them. The model prepares a response, accommodating the information that is returned by the MCP 
tools.
You can fine-tune the responses of the model by using the settings of the agent that is created in Agent Builder for the 
relevant use case.
VMware by Broadcom
244


---

VMware Private AI Foundation with NVIDIA 9.1
Table 22: Parameters for Fine-Tuning the Responses of the Model in the Agent
Agent Parameter
Description
Instructions
By providing instructions to the agent, you can configure it to interact with the model 
and added tools, and process queries. According to the capabilities of the model, you 
can provide instructions as a system message or by adding a prefix to each query.
Session management
•
Chat history max length. Represents the maximum number of characters in 
previous messages that is retained for the current session and used to generate 
responses. If this number is exceeded, the chat history is truncated.
•
Truncation strategy. Determines how messages are removed when the chat history 
exceeds its maximum length. First-in, first-out (FIFO) is the only option that is 
supported in this release.
•
Session expiration. Represents the number of days after which an idle session is 
deleted from your database.
Parameters for Knowledge Bases
Number of chunks to retrieve (Top K)
Top-K sampling limits the selection of tokens by the AI model to the K most probable 
tokens at each step of text generation. In a RAG system, the top-k value determines 
the number of most relevant document chunks to retrieve.
Fine-tune responses by using the top-k chunks parameter in the following way:
•
Increase for more comprehensive answers. A higher top-k value, for example, 10 
instead of 3, means that the AI considers more information from your knowledge 
base which can improve recall and provide a more comprehensive answer. This 
approach is best for complex or ambiguous queries. A higher number of chunks 
might slow response generation.
•
Decrease for more focused answers. A lower top-k, such as 3, limits the AI model 
to only the most relevant documents. This approach can help reduce the inclusion 
of irrelevant information, making the response more precise and concise.
•
Balance creativity and factual accuracy. A low top-k value, such as 1, makes the 
output deterministic and factual, while a higher value adds variety. For factual tasks 
like summarization, use a lower top-k value. For creative tasks, use a higher value.
Similarity cutoff
Similarity cutoff (or thresholding) filters out retrieved document chunks that do not meet 
a minimum relevance score. It ensures that the retrieved information is not just "one of 
the most relevant" but is relevant enough to be useful.
Fine-tune responses by using the similarity cutoff parameter in the following way:
•
Increase for high precision. A high similarity cutoff, for example, 0.8 on a scale of 0 
to 1, retrieves only the most highly related information. This approach reduces the 
risk of the AI model including irrelevant or incorrect details in its response and is 
suitable to provide precise and accurate answers.
•
Decrease for higher recall. A lower cutoff, such as 0.5, retrieves more documents, 
including those that are only moderately related. This approach is useful for 
general or exploratory queries that require considering broader context.
•
Achieve both precision and recall. Combine a moderate top-k value with a high 
similarity cutoff value. For example, setting the top-k parameter to 10 but with a 
cutoff value of 0.8 ensures that the AI model considers up to 10 documents, but 
only if they are all highly relevant.
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
VMware by Broadcom
245


---

VMware Private AI Foundation with NVIDIA 9.1
2. At the top of the Private AI Services page, click Agent Builder.
VMware by Broadcom
246


---

VMware Private AI Foundation with NVIDIA 9.1
3. Log in to the Agent Builder UI.
4. On the newly open Agents page, click Create Agent.
VMware by Broadcom
247


---

VMware Private AI Foundation with NVIDIA 9.1
5. In the Create Agent wizard, enter a name and description of the agent.
6. From the Model Endpoint drop-down menu, select the completion model endpoint for your application.
7. To fine-tune generated responses, activate Add Instructions and enter instructions to the model.
Tell the agent how to process the queries and interact with the model.
VMware by Broadcom
248


---

VMware Private AI Foundation with NVIDIA 9.1
VMware by Broadcom
249


---

VMware Private AI Foundation with NVIDIA 9.1
You can use the following example instructions to configure the model to act as a technical support agent:
As an internal technical support agent, you have been provided with access to the company's knowledge 
 base. Your task is to diagnose and provide a detailed, step-by-step solution to the user's technical 
 problem.  
  
**Retrieve:** Search the internal knowledge base for relevant articles, troubleshooting guides, and past 
 solutions related to the user's issue.  **Analyze:** Use the retrieved information to identify the most 
 likely cause and the best resolution path.  **Synthesize:** Present a solution clearly and concisely, 
 using a friendly and supportive tone.  **Format:** Output the response as a bulleted list of steps. If no 
 solution is found, state this clearly and provide the next steps for escalation. 
  
**User's Problem:**
<Insert user's problem description here, including any error messages, software, and hardware details.> 
  
**Knowledge Base Content:**
<Insert relevant knowledge base articles, document text, or API retrieval results here.>
8. To add business data from an MCP tool, click the Tools tab and click Add tools.
9. If you select an MCP tool for searching in a knowledge base, configure data usage in the agent.
The agent uses the configured number of chunks to retrieve (top-k chunks) and similarity cutoff to select the top results 
to send to the model. You can make the references visible in Markdown or JSON format.
10. Configure the session parameters of the agent.
You can configure the chat history length and session expiration period. To keep the chat history within the configured 
maximum length, the oldest messages are deleted.
11. Click Create.
•
For an agent that is configured to use knowledge bases from Data Indexing and Retrieval and its model does not 
support tool calling reliably, activate static tool execution. See Pre-Load Tool Results in the Context of an Agent.
•
Interact with the agent in the Playground pane and fine-tune as needed.
VMware by Broadcom
250


---

VMware Private AI Foundation with NVIDIA 9.1
VMware by Broadcom
251


---

VMware Private AI Foundation with NVIDIA 9.1
•
You can use the agent over the Private AI Services REST API. See the Private AI Services API documentation.
Pre-Load Tool Results in the Context of an Agent
When the model is not trained for reliable tool calling or has no tool calling support at all, you can use static tool execution 
in the agent for deterministic tool invocation for searching in knowledge bases. Tools that are attached to the model are 
called automatically before the language model processes your request rather than allowing the model to decide whether 
tools are needed.
•
Verify that Private AI Services is available in your namespace in VCF Automation.
•
Verify that you can access the Private AI Services FQDN or IP address from your machine.
•
Verify that the target agent is available in Private AI Services.
•
Verify that you have the curl command line utility that is installed on your machine.
However, using models with full tool calling support enables the AI to choose the specific search parameters to use for the 
knowledge retrieval. As a result, you have improved search results and agent responses.
Enable static tool execution when you need predictable, automatic knowledge base searches for every agent interaction 
or when using language models that do not reliably support dynamic tool calling. Knowledge base search tools run 
immediately when you send a message. The returned results are automatically included in the context that is provided to 
the language model. This approach ensures that every response from the model incorporates the latest information from 
your knowledge bases.
The static tool invocation mode has the following limitations:
•
Only supported for search tools for knowledge bases in Data Indexing and Retrieval.
•
Intended for use only with a single knowledge base.
•
Tools cannot require dynamic parameters from the conversation.
•
Best suited for simple retrieval augmentation workflows.
You enable static tool invocation over the Private AI Services REST API by setting the x-pais-force-static-tool-
execution property to true in the metadata of the agent. For more information, see the Private AI Services REST API 
documentation.
1. List the configured agents in Agent Builder by running the following command and in the response locate the ID of the 
agent you want to configure.
curl –-request GET \ 
 ‘https://<private_ai_services_FQDN_or_IP_address>/api/v1/compatibility/openai/v1/assistants’  \  
  --header ‘Authorization: Bearer <access-token>' \ 
  --header 'accept: application/json' \ 
  -k | jq
You get the following example response. For example, you want to use static tool execution for the agent with ID
demo_agent_with_legacy_KB_id.
{ 
  "object": "list", 
  "has_more": false, 
  "num_objects": null, 
  "data": [ 
    { 
      "name": "Demo Agent", 
      "description": "Agent used in manual demos and testing. Agent is using tools", 
      "instructions": "You are a test bot. Be concise and precise, and do exactly as told.", 
      "session_max_ttl": null, 
      "completion_role": "assistant", 
      "tools": [ 
        { 
          "link_type": "GENERIC_MCP_TOOL_LINK", 
          "tool_id": "fcedbc29-99de-4a57-84ac-bcef05d07465" 
        }, 
VMware by Broadcom
252


---

VMware Private AI Foundation with NVIDIA 9.1
        { 
          "link_type": "GENERIC_MCP_TOOL_LINK", 
          "tool_id": "c86e28c6-7ec5-41e7-aee0-949815dd054d" 
        } 
      ], 
      "index_id": null, 
      "index_top_n": null, 
      "index_similarity_cutoff": null, 
      "index_reference_format": "structured", 
      "index_reference_delimiter": "\\n", 
      "session_max_length": 10000, 
      "session_summarization_strategy": "delete_oldest", 
      "metadata": {}, 
      "id": "<demo_agent_id>", 
      "object": "agent", 
      "created_at": 1771142878, 
      "model": "completions-any-ggml-org/gpt-oss-20b-gguf-pbj6mppaoo2v3f23g", 
      "status": "AVAILABLE", 
      "status_errors": null, 
      "chat_system_instruction_mode": "system-message" 
    }, 
    { 
      "name": "Demo Agent using only REX KB Search MCP Tool", 
      "description": "Agent used in manual demos and testing. Agent is linked to an S3-based KB", 
      "instructions": "You are a test bot. Be concise and precise, and do exactly as told.", 
      "session_max_ttl": 2419200, 
      "completion_role": "assistant", 
      "tools": [ 
        { 
          "link_type": "PAIS_KNOWLEDGE_BASE_INDEX_SEARCH_TOOL_LINK", 
          "tool_id": "aa4c06a0-1642-4bba-b99c-98b876f738b1", 
          "top_n": 3, 
          "similarity_cutoff": 0.001 
        } 
      ], 
      "index_id": "e13bd156-577f-45c6-9428-7b974fc3a7e1", 
      "index_top_n": 3, 
      "index_similarity_cutoff": 0.001, 
      "index_reference_format": "structured", 
      "index_reference_delimiter": "\\n", 
      "session_max_length": 10000, 
      "session_summarization_strategy": "delete_oldest", 
      "metadata": {}, 
      "id": "<demo_agent_with_MCP_KB_id>", 
      "object": "agent", 
      "created_at": 1771142880, 
      "model": "completions-any-ggml-org/gpt-oss-20b-gguf-pbj6mppaoo2v3f23g", 
      "status": "AVAILABLE", 
      "status_errors": null, 
      "chat_system_instruction_mode": "system-message" 
    }, 
    { 
      "name": "Demo Agent using legacy KB integration", 
      "description": "Agent used in manual demos and testing. Agent is linked to an S3-based KB", 
      "instructions": "You are a test bot. Be concise and precise, and do exactly as told.", 
      "session_max_ttl": 2419200, 
      "completion_role": "assistant", 
VMware by Broadcom
253


---

VMware Private AI Foundation with NVIDIA 9.1
      "tools": [ 
        { 
          "link_type": "PAIS_KNOWLEDGE_BASE_INDEX_SEARCH_TOOL_LINK", 
          "tool_id": "aa4c06a0-1642-4bba-b99c-98b876f738b1", 
          "top_n": 1, 
          "similarity_cutoff": 0 
        } 
      ], 
      "index_id": "e13bd156-577f-45c6-9428-7b974fc3a7e1", 
      "index_top_n": 1, 
      "index_similarity_cutoff": 0, 
      "index_reference_format": "structured", 
      "index_reference_delimiter": "\\n", 
      "session_max_length": 10000, 
      "session_summarization_strategy": "delete_oldest", 
      "metadata": { 
        "x-pais-force-static-tool-execution": "true" 
      }, 
      "id": "<demo_agent_with_legacy_KB_id>", 
      "object": "agent", 
      "created_at": 1771142880, 
      "model": "completions-any-ggml-org/gpt-oss-20b-gguf-pbj6mppaoo2v3f23g", 
      "status": "AVAILABLE", 
      "status_errors": null, 
      "chat_system_instruction_mode": "system-message" 
    }, 
    { 
      "name": "Non-tools Demo Agent", 
      "description": "Agent used in manual demos and testing. Agent is not linked to any tools", 
      "instructions": "You are a test bot. Be concise and precise, and do exactly as told.", 
      "session_max_ttl": 2419200, 
      "completion_role": "assistant", 
      "tools": null, 
      "index_id": null, 
      "index_top_n": null, 
      "index_similarity_cutoff": null, 
      "index_reference_format": null, 
      "index_reference_delimiter": "\\n", 
      "session_max_length": 10000, 
      "session_summarization_strategy": "delete_oldest", 
      "metadata": {}, 
      "id": "<demo_agent_without_tools>", 
      "object": "agent", 
      "created_at": 1771142880, 
      "model": "completions-any-ggml-org/gpt-oss-20b-gguf-pbj6mppaoo2v3f23g", 
      "status": "AVAILABLE", 
      "status_errors": null, 
      "chat_system_instruction_mode": "system-message" 
    } 
  ], 
  "first_id": "a18caa2f-7ffe-4bf3-9ee6-c18011b4b459", 
  "last_id": "5204771e-6a1a-4aaf-b1ad-d3d1696f2f07"
}
2. Set the x-pais-force-static-tool-execution metadata option on the agent.
curl –-request POST \ 
VMware by Broadcom
254


---

VMware Private AI Foundation with NVIDIA 9.1
  ‘https://<private_ai_services_FQDN_or_IP_address>/api/v1/compatibility/openai/v1/
assistants/<demo_agent_with_legacy_KB_id>?=’  \   
  --header ‘Authorization: Bearer <access-token>' \ 
  --header 'accept: application/json' \ 
  --data '{ 
    "metadata": { 
      "x-pais-force-static-tool-execution": "true" 
    } 
  }'
Verify in the response that the metadata option is configured.
3. Optional: If you want to deactivate static tool execution, run the following command.
curl –-request POST \ 
  ‘https://<private_ai_services_FQDN_or_IP_address>/api/v1/compatibility/openai/v1/
assistants/<demo_agent_with_legacy_KB_id>?=’  \  
  --header ‘Authorization: Bearer <access-token>'  \ 
  --header 'accept: application/json' \ 
  --data '{ 
    "metadata": { 
      "x-pais-force-static-tool-execution": null 
    } 
  }'
Observing the Performance of Private AI Services Components by 
Using Metrics
As an MLOps engineer or application developer, by using metrics collected in a Prometheus server running in Private AI 
Services and visualized in the Private AI Services UI or in an observability platform, such as Grafana, you can perform 
monitoring and diagnostics of models which are used in agents. In this way, you can track the health, quality, and behavior 
of AI applications.
For example, you can use the metrics to analyze the following use cases:
•
LLM completion API response measured at 95th percentile
•
Inference engine token/seconds generation rate
•
GPU compute and memory resources that are undersized or oversized
Prerequisites
•
If you are using Private AI Services directly on vSphere Supervisor, verify that your account has the Namespaces Edit
role in the namespace.
•
Verify that Private AI Services is available in your namespace in VCF Automation.
•
Verify that observability metrics collection is active in Private AI Services.
a. Log in to the namespace with Private AI Services by using the VCF Consumption CLI.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token <vcf_automation_api_token> --ca-
certificate <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --
ca-certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access 
to are created. An additional context is available as <context_name>:<namespace_name>.
b. Switch to the context of the namespace.
VMware by Broadcom
255


---

VMware Private AI Foundation with NVIDIA 9.1
 VCF Automation
vcf context use <context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <context_name>:<namespace_name>
c. Run the following command and in the output verify that the observability settings are present in the default 
PAISConfiguration CR.
kubectl get paisconfiguration default -o jsonpath='{.spec.observability.prometheusRuntime}'
d. Run the following command and in the output verify that the Prometheus server pod is ready and running.
kubectl get pods
For more information about activating Private AI Services 3.0, see Activate Private AI Services 3.0 in an Organization 
Namespace by Using the UI and Activate Private AI Services 3.0 in an Organization Namespace by Using the kubectl 
Command.
For more information about activating Private AI Services 2.1, see Activate Private AI Services 2.1 in an Organization 
Namespace by Using the VCF Automation UI and Activate Private AI Services in an Organization Namespace by 
Using the kubectl Command.
•
Verify that an observability platform, such as Grafana, is configured to visualize the metrics from Private AI Services. 
See Visualizing Private AI Services Metrics in Observability Platforms.
•
Verify that you have running models in Model Runtime and agents in Agent Builder, optionally with attached knowledge 
bases from Data Indexing and Retrieval.
Available Observability Metrics
You can use categories of Private AI Services metrics according to audience and type of components.
Table 23: Sources of Private AI Services
Group of metrics
Description
High-level metrics
•
pais-api-object. Private AI Services API objects metrics, such as number of knowledge 
bases, number of agents, and other.
•
inference-gateway. Private AI Services inference gateway request level metrics, such as 
time to first token for requests, number of requests, and other.
•
rex-worker. Knowledge base document indexing metrics, such as failure count, indexing 
times, and other.
•
rex-mcp-server-metrics. Knowledge base search MCP tool metrics, such as number of 
search requests.
Specialized backend metrics, related 
to the VKS cluster running the models, 
NVIDIA GPU Operator running on the 
cluster, and to the inference engine
1.
modelendpoint-vllm. vLLM inference engine level metrics, such as tokens/second rate of 
the vLLM server instance of a model endpoint replica.
2.
modelendpoint-llamacpp. Llama.cpp inference engine level metrics, such as tokens/
second rate of the llama.cpp server instance of a model endpoint.
3.
nvidia-gpu. NVIDIA DCGM GPU metrics from the model runtime VM running model 
endpoint inference engines, such GPU compute, memory utilization, temperature, 
network traffic, and other.
4.
vkc-prometheus-kube-state-metrics. Kubernetes objects metrics, such as status of pods, 
deployments, jobs, and other.
5.
vkc-prometheus-node-exporter. Kubernetes node level metrics, such as VM node 
utilization metrics for CPU, memory, network, and storage.
6.
vkc-kubelet. Kubernetes node Kubelet level metrics, such as Kubelet inference engine 
image pull metrics.
7.
vkc-cadvisor. Kubernetes container level metrics, such as container CPU, memory 
utilization, and other.
8.
vkc-kube-apiserver. Kubernetes kube-apiserver level metrics, such as kube-apiserver 
requests, load, and other.
9.
prometheus-server. Private AI Services managed Prometheus server metrics, such as 
configuration, metrics scraping job health, metrics query requests, and other.
VMware by Broadcom
256


---

VMware Private AI Foundation with NVIDIA 9.1
Viewing Metrics in the Private AI Services UI
To view metrics directly in the Private AI Services UI, perform the following steps:
1. Open the Private AI Services UI in VCF Automation or in the Supervisor Management section of the vSphere Client.
VCF Automation UI
1.
Log in to the All Apps organization in VCF Automation.
2.
On the Build&Deploy tab, select the namespace with Private AI Services.
3.
Under Services, click Private AI.
vSphere Client for
Private AI Services 3.0
1.
Log in to the vCenter instance for the workload domain where the Supervisor instance is available at
https://<vcenter_fqdn>/ui as a user with the Namespaces Edit role.
2.
In the vSphere Client side panel, click Supervisor Management and, on the Namespaces tab, 
select the namespace with Private AI Services.
3.
On the Resources tab, click Private AI.
2. At the top of the Private AI Services page, click Agent Builder.
3. Log in to the Agent Builder UI.
4. On the newly open page of the Private AI Services UI, click Observability > Metrics.
You can view general metrics, metrics related to agents' or models' operation.
VMware by Broadcom
257


---

VMware Private AI Foundation with NVIDIA 9.1
Tracing Agent Operation and Knowledge Base Indexing
As an MLOps engineer or application developer, by using LLM traces, collected in an OpenTelemetry Collector platform, 
you can follow user requests as they are being processed by the agent and the components it uses.
Each segment of work that is performed by the agent is recorded as a span. Spans are then organized in a parent-
child hierarchy to form a trace. In this way, you can trace the multiple steps that are involved in a high-level task that is 
performed by the agents in Private AI Services, and evaluate an agent, model, or knowledge base index.
Prerequisites
•
Verify that Private AI Services is available in your namespace in VCF Automation or directly in vSphere Supervisor.
•
Verify that LLM traces are active in Private AI Services.
a. Log in to the namespace with Private AI Services by using the VCF Consumption CLI and switch to the context of 
the namespace.
 VCF Automation
vcf context create <vcf_automation_context_name> --endpoint 
 <vcf_automation_endpoint> --api-token <vcf_automation_api_token> --ca-
certificate <vcf_automation_certificate>.pem --type cci
A primary VCF Automation context and additional contexts for the namespaces 
you have access to are created. An additional context is available as
<context_name>:<namespace_name>:<project_name>.
 vSphere Supervisor
vcf context create <supervisor_context_name> --endpoint <supervisor_endpoint> --
ca-certificate <supervisor_ca_certificate>.pem --type k8s --auth-type basic
A primary vSphere Supervisor context and additional contexts for the namespaces you have access 
to are created. An additional context is available as <context_name>:<namespace_name>.
 VCF Automation
vcf context use <context_name>:<namespace_name>:<project_name>
 vSphere Supervisor
vcf context use <context_name>:<namespace_name>
b. Run the following command and in the output verify that the observability settings are present in the default 
PAISConfiguration CR.
kubectl get paisconfiguration default -o jsonpath='{.spec.observability.llmTraces}'
VMware by Broadcom
258


---

VMware Private AI Foundation with NVIDIA 9.1
c. If LLM traces are not active, to activate them, your VI administrator can use the Private AI Services UI or modify the 
PAISConfiguration resource in the namespace.
•
For information about activating LLM traces in the Private AI Services UI, see Enable Metrics Collection and 
LLM Traces in Private AI Services 3.0 by Using the UI.
•
To directly modify the PAISConfiguration resource, run the kubectl patch command with the required YAML 
configuration.
If trace exporting requires authentication headers, you can use the following example Secret in an otel-
collector-auth-secret.yaml file:
apiVersion: v1
kind: Secret
metadata: 
  name: <otel-collector-auth-secret>
  # Semi-colon separated  list of headers 
  stringData:  
    headers: Authorization=Bearer <access-token>; <X-Custom-Header>=<custom-header-value>
You can use the following YAML code for activating LLM tracers in a nllm_traces.yaml files:
spec: 
  observability: 
    llmTraces: 
      endpoint: "<otlp-http-endpoint>" 
      #  You can also use "grpc" to send traces. 
      # protocol: "grpc" 
      protocol: "http/protobuf" 
      projectName: "<pais-project-in-otel-collector>" 
      headersSecretRef: 
        name: <otel-collector-auth-secret>
        key: headers
Then, you can run the following example kubectl patch command:
kubectl patch PAISConfiguration default --patch-file llm_traces.yaml --type='merge'
•
Verify that you have an OpenTelemetry Collector platform with network access to the Private AI Services instance.
•
Verify that you can see spans, called instrument_code_for_tracing, in the traces reported to your 
OpenTelemetry Collector. These spans are generated by the Private AI Services instances after they succeed to 
connect to the configured collector.
•
Verify that you have running models in Model Runtime and agents in Agent Builder, optionally with attached knowledge 
bases from Data Indexing and Retrieval.
Available Traces
You can filter traces from Private AI Services in your OpenTelemetry Collector in the following way:
•
During knowledge base indexing, traces are associated with com.vmware.pais attributes for index_id,
indexing_id, document_id, and knowledge_base_id.
VMware by Broadcom
259


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 14: Example trace filter for a knowledge base
•
When an agent is used, its traces are associated with the com.vmware.pais.agent_id attribute.
VMware by Broadcom
260


---

VMware Private AI Foundation with NVIDIA 9.1
Figure 15: Example agent-based trace filter
VMware by Broadcom
261


---

VMware Private AI Foundation with NVIDIA 9.1
Monitoring VMware Private AI Foundation with NVIDIA
You can monitor GPU metrics at the cluster and host level in the vSphere Client and VCF Operations.
In VCF Operations, you can monitor GPU metrics at the cluster, host system and host properties levels. For more 
information, see the following resources:
•
Private AI (GPU) Dashboards
•
Virtual Machine Metrics
•
Host System Metrics
•
Cluster Compute Resource Metrics
In the vSphere Client, you can monitor GPU metrics in the following way:
•
At the host level. See Hosts Performance Charts in vSphere.
•
At the cluster level in custom charts. See Working with Advanced and Custom Charts in vSphere.
Monitoring Private AI Services Controller in VCF Operations
Тhe Supervisor Service of Private AI Services has a controller pod running in a dedicated system namespace. Because 
the same controller pod serves all namespaces across the organizations on a Supervisor cluster, as a VI administrator, 
you can access service-level metrics so that you can monitor the health of the Private AI Services Supervisor Service.
1. On the Operate tab, go to the Inventory > Integrations section.
2. Locate the PAIS Supervisor Service > pais.vmware.com metrics.
If streaming controller pod metrics is active, to view the metrics, perform the following steps:
1. On the Operate tab, go to the Inventory > Integrations section.
2. Locate the vSphere Supervisor > PAIS Supervisor Service > pais.vmware.com metrics.
For more information on the activation of controller pod metrics in VCF Operations, see Visualize Private AI Services 
Controller Metrics in VCF Operations.
VMware by Broadcom
262


---

VMware Private AI Foundation with NVIDIA 9.1
VMware by Broadcom
263


---

VMware Private AI Foundation with NVIDIA 9.1
VMware by Broadcom
264


---

VMware Private AI Foundation with NVIDIA 9.1
Documentation Legal Notice
Information about the documentation legal notice.
This Documentation, which includes embedded help systems and electronically distributed materials, (hereinafter referred 
to as the “Documentation”) is for your informational purposes only and is subject to change or withdrawal by Broadcom 
at any time. This Documentation is proprietary information of Broadcom and may not be copied, transferred, reproduced, 
disclosed, modified or duplicated, in whole or in part, without the prior written consent of Broadcom.
If you are a licensed user of the software product(s) addressed in the Documentation, you may print or otherwise make 
available a reasonable number of copies of the Documentation for internal use by you and your employees in connection 
with that software, provided that all Broadcom copyright notices and legends are affixed to each reproduced copy.
The right to print or otherwise make available copies of the Documentation is limited to the period during which the 
applicable license for such software remains in full force and effect. Should the license terminate for any reason, it is your 
responsibility to certify in writing to Broadcom that all copies and partial copies of the Documentation have been returned 
to Broadcom or destroyed.
TO THE EXTENT PERMITTED BY APPLICABLE LAW, BROADCOM PROVIDES THIS DOCUMENTATION “AS 
IS” WITHOUT WARRANTY OF ANY KIND, INCLUDING WITHOUT LIMITATION, ANY IMPLIED WARRANTIES OF 
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, OR NONINFRINGEMENT. IN NO EVENT WILL 
BROADCOM BE LIABLE TO YOU OR ANY THIRD PARTY FOR ANY LOSS OR DAMAGE, DIRECT OR INDIRECT, 
FROM THE USE OF THIS DOCUMENTATION, INCLUDING WITHOUT LIMITATION, LOST PROFITS, LOST 
INVESTMENT, BUSINESS INTERRUPTION, GOODWILL, OR LOST DATA, EVEN IF BROADCOM IS EXPRESSLY 
ADVISED IN ADVANCE OF THE POSSIBILITY OF SUCH LOSS OR DAMAGE.
The use of any software product referenced in the Documentation is governed by the applicable license agreement and 
such license agreement is not modified in any way by the terms of this notice
The manufacturer of this Documentation is Broadcom Inc.
Provided with “Restricted Rights.” Use, duplication or disclosure by the United States Government is subject to the 
restrictions set forth in FAR Sections 12.212, 52.227-14, and 52.227-19(c)(1) - (2) and DFARS Section 252.227-7014(b)
(3), as applicable, or their successors.
Copyright © 2005–2026 Broadcom. All Rights Reserved. The term “Broadcom” refers to Broadcom Inc. and/or its 
subsidiaries. All trademarks, trade names, service marks, and logos referenced herein belong to their respective 
companies.
VMware by Broadcom
265
