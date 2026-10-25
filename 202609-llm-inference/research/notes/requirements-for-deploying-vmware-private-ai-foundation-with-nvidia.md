---
title: Requirements for Deploying VMware Private AI Foundation with NVIDIA
id: requirements-for-deploying-vmware-private-ai-foundation-with-nvidia
tags:
- onprem-llm-inference-stacks-859099
- onprem
- vendor-doc
created: '2026-10-23T18:13:19.458275Z'
updated: '2026-10-23T18:13:41.351519Z'
source: https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/requirements-for-deploying-private-ai-foundation-with-nvidia.html
source_domain: techdocs.broadcom.com
fetched_at: '2026-10-23T18:13:19.436777Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Broadcom TechDocs VCF/PAIF 9.0 ''Requirements for Deploying VMware Private
  AI Foundation with NVIDIA'' (fetched 2026-10-23). Vendor doc = requirement statement.
  Needs: VCF subscription + PAIF add-on licence + NVIDIA vGPU/NVAIE licence for vGPU
  workloads; >=3 GPU-enabled ESX hosts in initial workload-domain cluster; NVIDIA
  vGPU host driver VIB; GPUs per Broadcom compat guide; MIG cannot be used with NVIDIA
  NIM; Supervisor needed for VKS/Private AI Services. Confirms the briefed constraint.
  PAIF is a layer on top of the VMware virtualization/Kubernetes stack, replacing
  bare-metal K8s/Slurm scheduling with vGPU VMs.'
---

VMware Private AI Foundation with NVIDIA 9.0
CHANGE VERSION
9.1  9.0  5.2  5.1 
English 
  *     * [VMware Private AI Foundation with NVIDIA 9.0.x Release Notes](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-release-notes/vmware-private-ai-foundation-with-nvidia-90-release-notes.html)
    * [VMware Deep Learning VM Image Release Notes](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-release-notes/vmware-deep-learning-vm-image-release-notes.html)
    * [VMware Private AI Services Release Notes](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-release-notes/vmware-private-ai-services-release-notes.html)
  * [VMware Private AI Foundation with NVIDIA Guide](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x.html)
    * [What is VMware Private AI Foundation with NVIDIA?](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/overview-of-private-ai-foundation-with-nvidia.html)
    * [Preparing VCF for Private AI Workload Deployment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia.html)
      * [Setup Workflows for Deep Learning VMs and VKS Clusters with GPU](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/setup-workflows.html)
      * [Setup Workflows for Private AI Services](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/setup-workflow-for-private-ai-services.html)
      * [System Architecture of VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/system-architecture-of-vmware-private-ai-foundation-with-nvidia.html)
      * [Requirements for Deploying VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/requirements-for-deploying-private-ai-foundation-with-nvidia.html)
      * [Add a VMware Private AI Foundation with NVIDIA License to Your Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/assign-the-private-ai-foundation-license-to-the-management-domain.html)
      * [Follow the Guided VMware Private AI Foundation with NVIDIA Setup in the vSphere Client](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/guided-private-ai-deployment-in-the-vsphere-client.html)
      * [Deploy a GPU-Accelerated Workload Domain for VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/deploy-a-vi-workload-domain-in-vmware-cloud-foundation.html#GUID-b4174728-6367-4729-bf29-23b3fdbeb1b4-en)
      * [Configure NVIDIA vGPU or GPU Passthrough for AI Workloads on the ESX Hosts](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/configure-nvidia-vgpu-or-gpu-passthrough-on-the-esx-hosts.html)
      * [Configure VM Classes for AI Workloads for VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/configure-vgpu-based-vm-classes-for-ai-workloads.html)
      * [Setting Up a Local Harbor Registry in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/enable-a-private-harbor-registry-in-paif.html)
        * [Set Up a Bootstrap Harbor Registry in a Disconnected VMware Private AI Foundation with NVIDIA Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/enable-a-private-harbor-registry-in-paif/set-up-a-harbor-registry-in-a-disconnected-environment.html)
        * [Set Up a Local Platform Harbor Registry as a Supervisor Service for VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/enable-a-private-harbor-registry-in-paif/set-up-a-harbor-registry-in-a-connected-environmnet-for-storing-models-for-private-ai-services.html)
      * [Configure a Content Library with Ubuntu VKr for a Disconnected VMware Private AI Foundation with NVIDIA Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/create-a-content-library-with-ubuntu-tkr.html)
      * [Setting Up VCF Automation for VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/setting-up-vcf-automation-for-private-ai-foundation-with-nvidia.html)
        * [Set Up Your VCF Automation Organization for VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/setting-up-vcf-automation-for-private-ai-foundation-with-nvidia/set-up-vmware-aria-automation-for-private-ai-foundation-with-nvidia.html)
          * [Import Private AI Foundation Namespaces into VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/setting-up-vcf-automation-for-private-ai-foundation-with-nvidia/set-up-vmware-aria-automation-for-private-ai-foundation-with-nvidia/import-namespaces-into-vcf-automation-9.html)
      * [Finalizing the Setup for Deep Learning VMs and VKS Clusters with GPU](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/finalizing-the-setup-for-deep-learning-vms-and-vks-cluster-with-gpu.html)
        * [Deployment Artifacts for Deep Learning VMs and VKS Clusters](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/finalizing-the-setup-for-deep-learning-vms-and-vks-cluster-with-gpu/location-of-the-deployment-artifacts-for-deep-learning-vms-and-vks-clusters-with-vgpu-in-a-disconnected-environment.html)
        * [Upload the NVIDIA GPU Operator and Application Container Images for Deep Learning VMs and VKS Clusters with GPU to the Platform Harbor Registry in a Disconnected Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/finalizing-the-setup-for-deep-learning-vms-and-vks-cluster-with-gpu/upload-the-nvidia-gpu-operator-components-to-the-environemnt.html)
        * [Provide an Ubuntu Package Repository in a Disconnected Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/finalizing-the-setup-for-deep-learning-vms-and-vks-cluster-with-gpu/provide-an-ubuntu-package-repository--mirror-and-vgpu-drivers-for---deep-learning-vms-and-vks-clusters-with-gpu-in-a-disconnected-environment.html)
        * [Create AI Catalog Items in VMware Aria Automation using the Private AI Foundation Quickstart](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/finalizing-the-setup-for-deep-learning-vms-and-vks-cluster-with-gpu/how-do-i-use-the-paif-quickstart-in-aria-automation.html)
      * [Installing and Configuring Private AI Services](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/installing-and-configuring-private-ai-services.html)
        * [Deployment Artifacts for Private AI Services](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/installing-and-configuring-private-ai-services/deployment-artifacts-for-private-ai-services--in-a-disconnected-environment.html)
        * [Provide an Ubuntu Package Repository for Private AI Services in a Disconnected Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/installing-and-configuring-private-ai-services/provide-an-ubuntu-package-repository-mirror.html)
        * [Upload the Private AI Services Components to a Disconnected Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/installing-and-configuring-private-ai-services/upload-the-private-ai-services-components-to-a-disconnected-environment.html)
        * [Install Private AI Services on the Supervisor of the GPU-Accelerated Workload Domain](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/installing-and-configuring-private-ai-services/install-private-ai-services-on-the-supervisor.html)
        * [Activate Private AI Services in an Organization Namespace by Using the kubectl Command](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/installing-and-configuring-private-ai-services/configure-private-ai-services-for-a-namespace-in-an-organization.html)
        * [Visualizing Private AI Services Metrics in Observability Platforms](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/installing-and-configuring-private-ai-services/observing-private-ai-services.html)
    * [Deploying a Deep Learning VM in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine.html)
      * [About Deep Learning VM Images in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/details-about-the-deep-learning-virtual-machine-image.html#GUID-A85A63F7-BBCF-4E88-B122-96FEDCED8A21-en)
      * [Deploy Deep Learning Virtual Machines by Using Self-Service Catalog Items in VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/service-broker-paif-data-scientist-workflow.html#GUID-4B239386-17AA-47FC-9E94-530EC4A7FD06-en)
        * [Deploy a Deep Learning VM by Using VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/service-broker-paif-data-scientist-workflow.html#GUID-DF74493C-DDE4-45C1-B380-332A908643F8-en)
        * [Deploy a Deep Learning VM with Triton Inference Server by Using VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/service-broker-paif-data-scientist-workflow.html#GUID-68D36795-89FF-4FD4-8CE0-9DA035D240D3-en)
      * [Deploy a Deep Learning VM Directly on a vSphere Cluster in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/deploy-a-deep-learning-virtual-machine-on-vsphere.html)
      * [Deploy a Deep Learning VM by Using the kubectl Command in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/deploy-a-deep-learning-virtual-machine-on-vsphere-with-tanzu.html)
      * [Monitoring the vGPU Performance of a Deep Learning VM](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/monitoring-vgpu-performance-of-a-deep-learning-vm.html)
      * [Customizing Deep Learning VM Deployment in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/customizing-deep-learning-vm-deployment.html#GUID-5347C2DB-BED3-4CAD-BD1E-2A1ECD7E9E0B-en)
        * [OVF Properties of Deep Learning VMs](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/customizing-deep-learning-vm-deployment.html#GUID-663F20B6-F212-4D91-9909-5D9895CC2DA9-en)
        * [Deep Learning Workloads in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/customizing-deep-learning-vm-deployment.html#GUID-1917B207-13D4-4BCD-AF54-F291B0703086-en)
          *           *           *         * [Assign a Static IP Address to a Deep Learning VM in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/customizing-deep-learning-vm-deployment.html#GUID-996EF091-31F6-4DC5-BBBB-DFB823D61870-en)
        * [Configure a Deep Learning VM with a Proxy Server](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/customizing-deep-learning-vm-deployment.html#GUID-ED1AF2B5-9307-47C4-A215-C078C73ED6D4-en)
      * [Troubleshooting Deep Learning VM Deployment in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/troubleshooting-deep-learning-vm-deployment.html)
        * [DL Workload Automation Is Not Performed](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/troubleshooting-deep-learning-vm-deployment/dl-workload-is-not-installed-automatically.html)
        * [Downloading a DL Workload Fails Because of Invalid Authentication Credentials](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/troubleshooting-deep-learning-vm-deployment/downloading-a-dl-workload-fails-because-of-invalid-authentication-credentials.html)
        * [The NVIDIA vGPU Guest Driver Is Shown as Unlicensed ](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-a-deep-learning-virtual-machine/troubleshooting-deep-learning-vm-deployment/the-nvidia-vgpu-driver-is-shown-as-unlicensed.html)
    * [Deploying AI Workloads on VKS Clusters in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-ai-workloads-on-tkg-clusters.html)
      * [Deploy a GPU-Accelerated VKS Cluster by Using a Self-Service Catalog Item in VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-ai-workloads-on-tkg-clusters/service-broker-paif-deploy-tkg-cluster.html)
      * [Provision a GPU-Accelerated VKS Cluster by Using the kubectl Command in a Connected VMware Private AI Foundation with NVIDIA Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-ai-workloads-on-tkg-clusters/deploy-a-gpu-accelerated-tkg-cluster-with-kubectl-connected.html)
      * [Provision a GPU-Accelerated VKS Cluster by Using the kubectl Command in a Disconnected VMware Private AI Foundation with NVIDIA Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-ai-workloads-on-tkg-clusters/deploy-a-gpu-accelerated-tkg-cluster-with-kubectl-disconnected.html)
    * [Deploying RAG Workloads in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia.html)
      * [Deploy a Vector Database in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/deploy-a-vector-database-for-paif.html#GUID-337AAADE-AB79-4480-AEAB-265035AC3439-en)
        * [Deploy a Vector Database by Using a Self-Service Catalog Item in VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/deploy-a-vector-database-for-paif.html#GUID-433E6F96-E954-401F-A78B-209EC12AA0D2-en)
      * [Deploy a Deep Learning VM with a RAG Workload](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/deploy-a-deep-learning-vm-with-a-rag-workload.html)
      * [Deploy a RAG Workload on a VKS Cluster in a Connected VMware Private AI Foundation with NVIDIA Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/deploy-a-rag-workload-on-a-tkg-cluster.html)
      * [Deploy a RAG Workload on a VKS Cluster in a Disconnected VMware Private AI Foundation with NVIDIA Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/deploy-a-rag-workload-on-a-vks-cluster-in-a-disconnected-private-ai-environment.html)
      * [Deploying RAG Workloads by Using Self-Service Catalog Items in VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/service-broker-paif-rag-workflow.html)
        * [Deploy a Deep Learning VM with a RAG Workload Using a Self-Service Catalog Item in VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/service-broker-paif-rag-workflow/service-broker-paif-deploy-rag-workstation.html)
        * [Deploy a Deep Learning VM with a RAG Workload and a Vector Database by Using a Self-Service Catalog Item in VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/service-broker-paif-rag-workflow/service-broker-paif-rag-workstation-with-dsm.html)
        * [Deploy a RAG Workload on a VKS Cluster Using a Self-Service Catalog Item in VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/service-broker-paif-rag-workflow/service-broker-paif-deploy-rag-cluster.html)
        * [Deploy a RAG Cluster with a Vector Database by Using a Self-Service Catalog Item in VCF Automation](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-rag-workloads-in-private-ai-foundation-with-nvidia/service-broker-paif-rag-workflow/service-broker-paif-rag-cluster-with-dsm.html)
    * [Delivering Generative AI Applications by Using Private AI Services](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services.html)
      * [Storing ML Models in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/storing-ml-models-in-vmware-private-ai-foundation.html)
        * [Before You Begin Storing ML Models in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/storing-ml-models-in-vmware-private-ai-foundation/prerequisites-for-storing-ml-models.html)
        * [Validate and Upload an ML Model to a Model Gallery](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/storing-ml-models-in-vmware-private-ai-foundation/validate-approve-and-upload-an-ml-model.html)
        * [Deploy an ML Model to Run Inference](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/storing-ml-models-in-vmware-private-ai-foundation/deploy-an-ml-model-to-run-inference.html)
      * [Import a CA Trust Bundle in Private AI Services](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/importing-a-ca-trust-bundle-in-private-ai-services.html)
      * [Running Completion or Embedding Models by Using Model Endpoints](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/deploying-model-endpoints.html)
        * [Deploy Completion or Embedding Model Endpoints by Using the VCF Automation UI](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/deploying-model-endpoints/deploy-a-model-endpoint.html)
        * [Deploy Completion or Embedding Model Endpoints by Using the kubectl Command](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/deploying-model-endpoints/deploy-completion-or-embedding-model-endpoints-by-using-the-kubectl-command.html)
        * [Troubleshooting Model Endpoint Deployments](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/deploying-model-endpoints/troubleshooting-model-endpoint-deployment.html)
      * [Integrating Specialized Data and Capabilities over MCP](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/adding-mcp-servers-for-real-time-data-access-and-specialized-ai-capabilities.html)
        * [Connect an MCP Server to Private AI Services](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/adding-mcp-servers-for-real-time-data-access-and-specialized-ai-capabilities/connect-to-an-mcp-server.html)
        * [Explore the MCP Tools Available in Your Namespace](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/adding-mcp-servers-for-real-time-data-access-and-specialized-ai-capabilities/exploring-the-mcp-tools-avaiable-in-your-namespace.html)
      * [Adding Context to Model Responses by Using Knowledge Bases](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/adding-context-to-model-responses-by-using-knowledge-bases.html)
        * [Add a Data Source for a Knowledge Base](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/adding-context-to-model-responses-by-using-knowledge-bases/add-a-data-source-for-a-knowledge-base.html)
        * [Create a Knowledge Base with Linked Data Sources](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/adding-context-to-model-responses-by-using-knowledge-bases/create-a-knowledge-base-with-linked-data-sources.html)
        * [Create a Knowledge Base with Local Files](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/adding-context-to-model-responses-by-using-knowledge-bases/create-a-knowledge-base-with-local-files.html)
        * [Considerations When Modifying and Indexing Knowledge Bases](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/adding-context-to-model-responses-by-using-knowledge-bases/considerations-when-using-knowledge-bases.html)
      * [Create an Agent for a Generative AI Application](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/deploy-an-agent-for-a-rag-application.html)
      * [Pre-Load Tool Results in the Context of an Agent](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/pre-loading-mcp-tool-results-in-agent-context.html)
      * [Observing the Performance of Private AI Services Components by Using Metrics](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/observing-the-performance-of-private-ai-services-components.html)
      * [Tracing Agent Operation and Knowledge Base Indexing](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/what-is-private-ai-services/tracing-agent-operation-and-knowledge-base-indexing.html)
    * [Monitoring VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/operating-private-ai-foundation-with-nvidia.html)
  * [Documentation Legal Notice](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/documentation-legal-notice-english-public.html)


# Requirements for Deploying VMware Private AI Foundation with NVIDIA
You deploy components of 
VMware Private AI Foundation with NVIDIA
in your 
VCF
environment in a workload domain where you must have certain NVIDIA and 
VCF
components installed. 
[ Download PDF ](https://techdocs.broadcom.com/content/dam/broadcom/techdocs/us/en/pdf/vmware/private-ai/private-ai-nvidia/vmware-private-ai-foundation-with-nvidia-9-0.pdf)
# 
Requirements for Deploying 
VMware Private AI Foundation with NVIDIA
You deploy components of 
VMware Private AI Foundation with NVIDIA
in your 
VCF
environment in a workload domain where you must have certain NVIDIA and 
VCF
components installed.
## 
Required VMware Software Versions
See [VMware Components in VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia.html#GUID-8147D5AB-07B9-473A-B94E-6CE738D43B0D-en_paif-bom).
## 
Supported NVIDIA GPU Devices 
Before you start using 
VMware Private AI Foundation with NVIDIA
, make sure that the GPUs on your ESXi hosts are supported by VMware by Broadcom:   
Supported NVIDIA Components for 
VMware Private AI Foundation with NVIDIA  
| NVIDIA Component  | Supported Options  |  
| --- | --- |  
| NVIDIA GPUs  |  See [Broadcom Compatibility Guide for GPU and Accelerators for AI/ML and Compute](https://compatibilityguide.broadcom.com/search?program=sptg&persona=live&column=&order=asc&productReleaseVersion=%5BESXi+9.0%5D&compute=%5BAI%2FML%5D&gpuWorkload=%5BCompute%5D&activePage=1&activeDelta=20).  |  
| GPU sharing mode  | 
  * Time slicing
  * Multi-Instance GPU (MIG)

 |  
## 
Required NVIDIA Software
You use NVIDIA software that is specific to the GPU virtualization mode.   
| Device Type  | Description  |  
| --- | --- |  
| GPU in vGPU mode  |  The GPU device must support the latest [NVIDIA AI Enterprise](https://www.nvidia.com/en-us/data-center/products/ai-enterprise-suite/) (NVAIE) vGPU profiles. See the [NVIDIA Virtual GPU Software Supported GPUs](https://docs.nvidia.com/grid/gpus-supported-by-vgpu.html) documentation for guidance.
  * NVIDIA vGPU host driver (including the VIB for ESX hosts), that is compatible with your VCF version. See [Virtual GPU Software for VMware vSphere Release Notes](https://docs.nvidia.com/grid/latest/grid-vgpu-release-notes-vmware-vsphere/index.html). 
  * NVIDIA GPU Operator that is compatible with the Kubernetes version of the deployed VKS clusters. See [NVIDIA GPU Operator Release Notes](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/release-notes.html) and [VMware VKrs Release Notes](https://techdocs.broadcom.com/us/en/vmware-cis/vcf/vsphere-supervisor-services-and-standalone-components/latest/release-notes/vmware-vkr-release-notes.html).

 |  
| GPU in passthrough mode, supported for Private AI Services  | NVIDIA GPU Operator that is compatible with the Kubernetes version of the deployed VKS clusters. See [NVIDIA GPU Operator Release Notes](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/release-notes.html) and [VMware VKrs Release Notes](https://techdocs.broadcom.com/us/en/vmware-cis/vcf/vsphere-supervisor-services-and-standalone-components/latest/release-notes/vmware-vkr-release-notes.html).  |  
## 
Required 
VCF
Setup
Before you deploy 
VMware Private AI Foundation with NVIDIA
, a specific configuration must be available in 
VCF
. 
  * VCF
9.0
  * A 
VCF
license, 
VMware Private AI Foundation with NVIDIA
add-on license, and NVIDIA vGPU license if you plan to run AI workloads on vGPU. See 
Licensing and License Management
.
  * VCF
Single Sign-On configured on the 
VCF
Instance containing the GPU-enabled ESX hosts for the workload domain. See 
[Configure a New VCF Single Sign-On for a VCF Instance](https://techdocs.broadcom.com/bin/gethidpage?ux-context-string=vcom_840&appid=vcf-9-0&language=&format=rendered) in the 
VCF
documentation.
  * The VIB file of the NVIDIA vGPU host driver downloaded from <https://nvid.nvidia.com/>
  * A vSphere Lifecycle Manager image with the VIB file of the vGPU host manager driver available in SDDC Manager. See [Managing vSphere Lifecycle Manager Images in VMware Cloud Foundation](https://techdocs.broadcom.com/us/en/vmware-cis/vcf/vcf-5-2-and-earlier/5-2/map-for-administering-vcf-5-2/image-management-admin.html). 
  * At least 3 GPU-enabled ESX hosts to include in the initial cluster of a workload domain.
  * For running AI workloads on vGPU, NVIDIA vGPU host driver installed and vGPU configured on each ESX host in the cluster for AI workloads.
    1. On each ESX host, enable SR-IOV in the BIOS according to the documentation from your hardware vendor and on the graphics devices for AI operations by using the VMware Host Client.
    2. Install the NVIDIA vGPU host driver on each ESXi host in one of the following ways: 
       * Install the driver on each host and add the VIB file of the driver to the vSphere Lifecycle image for the cluster. 
See [NVIDIA Virtual GPU Software Quick Start Guide](https://docs.nvidia.com/grid/latest/grid-software-quick-start-guide/index.html).
       * Add the VIB file of the driver to the vSphere Lifecycle image for the cluster and remediate the hosts.
    3. If you want to use the Multi-Instance GPU (MIG) sharing, enable it on each ESXi host in the cluster. 
See [NVIDIA MIG User Guide](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/index.html).


## 
Limitations
  * You cannot use the MIG sharing mode for NVIDIA GPUs with NVIDIA NIMs. See [NVIDIA NIM for LLMs Release Notes](https://docs.nvidia.com/nim/large-language-models/latest/release-notes.html).


## 
Licensing and License Management
To be able to use the 
VMware Private AI Foundation with NVIDIA
functionality, you must have the following licenses: 
  * A 
VCF
subscription
For more information about licensing management in 
VCF
, see [Licensing](https://techdocs.broadcom.com/bin/gethidpage?ux-context-string=licensing_main&appid=vcf-9-0&language=&format=rendered) in the 
VCF
documentation.
  * A 
VMware Private AI Foundation with NVIDIA
add-on license
You need the 
VMware Private AI Foundation with NVIDIA
add-on license to access the following functionality:
    * Private AI setup in 
VCF Automation
for catalog items for easy provisioning of GPU-accelerated deep learning virtual machines and VKS clusters.
    * Provisioning of PostgreSQL databases with the pgvector extension with enterprise support in 
VMware Data Services Manager
.
    * Deploying and using the deep learning virtual machine image delivered by VMware.
    * Guided deployment workflow in the vSphere Client.
    * VM classes with GPU reservation.
    * Private AI Services
You can deploy AI workloads with and without a Supervisor enabled and use the GPU metrics in vCenter and 
VCF Operations
under the 
VCF
license.
You assign your 
VMware Private AI Foundation with NVIDIA
to a GPU-enabled workload domain. Assigning a 
VMware Private AI Foundation with NVIDIA
license to the management domain is required only to activate the guided deployment UI in the vSphere Client.
  * For running AI workloads on vGPU, an [NVIDIA AI Enterprise (NVAIE) license](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/licensing.html) that is required for the host driver VIB file on ESX hosts and the guest OS drivers.


[Preparing VCF for Private AI Workload Deployment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia.html "As a VI administrator, you must deploy specific software and configure the target workload domains so that data scientists, DevOps engineers, and MLOps engineers can deploy AI workloads on top of VMware Private AI Foundation with NVIDIA.")
PREVIOUS
[System Architecture of VMware Private AI Foundation with NVIDIA](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/system-architecture-of-vmware-private-ai-foundation-with-nvidia.html)
NEXT
[Add a VMware Private AI Foundation with NVIDIA License to Your Environment](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/assign-the-private-ai-foundation-license-to-the-management-domain.html)
Was this page helpful?
Documentation Feedback 
Last Updated July 15, 2026
Other Product Support Resources 
Support
[Support Portal](https://support.broadcom.com) [Product Communities](https://community.broadcom.com) [Knowledge Base](https://www.broadcom.com/support/knowledgebase)
Learning
[Brocade Education](https://www.broadcom.com/support/education/brocade) [Mainframe Software Education](https://www.broadcom.com/support/education/mainframe/education-program) [Software Education](https://www.broadcom.com/support/education/software) [VMware Learning](https://www.broadcom.com/support/education/vmware)
Need more help?
[Virtual Agent](https://support.broadcom.com) [Advanced Support](https://www.broadcom.com/support/services-support/ca-support/support-programs) [Contact Us](https://support.broadcom.com/web/ecx/contact-support)
On this page
  * [Required VMware Software Versions](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/requirements-for-deploying-private-ai-foundation-with-nvidia.html#GUID-C4C49ED8-E3CD-4F70-B170-CD50A07AAC51-en_TITLE_DEFBDE33-9A22-4A74-95DD-B8ED49ADDB5D)
  * [Supported NVIDIA GPU Devices](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/requirements-for-deploying-private-ai-foundation-with-nvidia.html#GUID-C4C49ED8-E3CD-4F70-B170-CD50A07AAC51-en_TITLE_91D684DE-1E69-43A5-8464-1E36E454DAD2)
  * [Required NVIDIA Software](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/requirements-for-deploying-private-ai-foundation-with-nvidia.html#GUID-C4C49ED8-E3CD-4F70-B170-CD50A07AAC51-en_TITLE_C8A3CC6A-A6E6-48A9-BCA6-7EF8659E5BE2)
  * [Licensing and License Management](https://techdocs.broadcom.com/us/en/vmware-cis/private-ai/foundation-with-nvidia/9-0/private-ai-foundation-9-x/deploying-private-ai-foundation-with-nvidia/requirements-for-deploying-private-ai-foundation-with-nvidia.html#GUID-C4C49ED8-E3CD-4F70-B170-CD50A07AAC51-en_id-f2dd4866-3b16-4268-9e28-50f8082cdbf5)


Was this page helpful?
Documentation Feedback 
Last Updated July 15, 2026
## Cookies
Broadcom and our partners use technology, including cookies to, among other things, operate the site, analyze site usage, view and retain your site interactions, improve your experience and help us advertise. Click “Cookie Settings” to manage your privacy choices. By continuing to use our site, you agree to these data practices as described in our[ Cookie Notice](https://www.broadcom.com/company/legal/cookie-policy)
Cookies Settings
## Privacy Preference Center
## Privacy Preference Center
  * ### Your Privacy
  * ### Strictly Necessary Cookies
  * ### Functional Cookies
  * ### Performance Cookies
  * ### Targeting Cookies


#### Your Privacy
When you interact with Broadcom as set forth in the Privacy Policy through visiting any website, it may store or retrieve information on your browser, mostly in the form of cookies. This information might be about you, your preferences or your device and is mostly used to make the site work as you expect it to. The information does not usually directly identify you, but it can give you a more personalized web experience. [Cookie Policy](https://www.broadcom.com/company/legal/privacy/cookie-policy)[Privacy Policy](https://www.broadcom.com/company/legal/privacy/policy)
#### Strictly Necessary Cookies
Always Active
These cookies are necessary for the website to function and cannot be switched off in Broadcom’s systems. They are usually only set in response to actions made by you which amount to a request for services, such as setting your privacy preferences, logging in or filling in forms. You can set your browser to block or alert you about these cookies, but some parts of the site will not then work. These cookies do not store any personally identifiable information.
#### Functional Cookies
Functional Cookies
These cookies enable the website to provide enhanced functionality and personalization. They may be set by us or by third party providers whose services we have added to our pages. If you do not allow these cookies then some or all of these services may not function properly.
#### Performance Cookies
Performance Cookies
These cookies allow Broadcom to count visits and traffic sources so Broadcom can measure and improve the performance of its site. They help Broadcom to know which pages are the most and least popular and see how visitors move around the site. All information these cookies collect is aggregated and therefore anonymous. If you do not allow these cookies Broadcom will not know when you have visited our site and will not be able to monitor its performance.
#### Targeting Cookies
Targeting Cookies
These cookies may be set through Broadcom’s site by its advertising partners. They may be used by those companies to build a profile of your interests and show you relevant adverts on other sites. They do not store directly personal information, but are based on uniquely identifying your browser and internet device. If you do not allow these cookies, you will experience less targeted advertising.
Back Button
### Cookie List
Filter Button
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Clear
  * checkbox label label


Apply Cancel
Confirm My Choices
Required Only Allow All
