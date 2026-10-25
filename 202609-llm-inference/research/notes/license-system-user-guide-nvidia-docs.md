---
title: License System User Guide - NVIDIA Docs
id: license-system-user-guide-nvidia-docs
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- vendor-docs
- reliability-high
created: '2026-10-23T17:23:22.844861Z'
updated: '2026-10-23T17:25:08.372250Z'
source: https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html
source_domain: docs.nvidia.com
fetched_at: '2026-10-23T17:23:22.816700Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'NVIDIA License System user guide (very long, 92k words). Key: DLS instance
  on-prem is fully disconnected from NVIDIA Licensing Portal, licences downloaded
  and uploaded manually; CLS is cloud-hosted; node-locked licensing from local file
  for air-gapped clients (vGPU 15.0+); DLS 3.4+ license server files valid three years;
  HA up to nine DLS instances; vGPU 18+ requires DLS 3.4+. Note: this is vGPU-oriented
  guide; applicability to NIM/AI Enterprise licensing is inferred by the beri.net
  article.'
---

*Suggested by [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]] — cited as primary source*

Menu
NVIDIA License System v4.0.0
[NVIDIA Docs Hub Homepage](https://docs.nvidia.com/) [NVIDIA License System](https://docs.nvidia.com/license-system/index.html) [NVIDIA License System v4.0.0](https://docs.nvidia.com/license-system/latest/index.html) [License System User Guide](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html)
[Is this page helpful?](https://surveys.hotjar.com/4904bf71-6484-47a7-83ff-4715cceabdb5?utm_source=https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html)
# [License System User Guide](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html)
##  [NVIDIA License System User Guide](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#abstract)
Documentation for system administrators that explains how to install, configure, and manage the NVIDIA License System.
## [1. Introduction to NVIDIA License System](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#introduction-to-nvidia-license-system)
The NVIDIA License System is used to provide software licenses to licensed NVIDIA software products. The licenses that the NVIDIA License System provides are obtained from the NVIDIA Licensing Portal.
Note:
NVIDIA vGPU software releases earlier than 13.0 do **not** support NVIDIA License System. For full details of NVIDIA vGPU software releases that support NVIDIA License System, refer to [NVIDIA License System Release Notes](https://docs.nvidia.com/license-system/latest/nvidia-license-system-release-notes/index.html). 
###  [1.1. Introduction to NVIDIA Software Licensing](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#introduction-to-nvidia-software-licensing)
To activate licensed functionalities, a licensed client must obtain a software license when it is booted. NVIDIA License System provides role-based access control (RBAC) for DLS instances, enabling organizations to create multiple application users with different privilege levels. These application users are separate from OS-level users (dls_admin, rsu_admin) and are used solely for DLS Web UI and API access. Administrators can create local users or integrate with enterprise LDAP/Active Directory systems for centralized authentication. For more information, see User Accounts on a DLS Virtual Appliance.
NVIDIA License System supports the types of licensing for licensed clients: 
  * **Networked-licensing:** A client with a network connection obtains a license by leasing it from a NVIDIA License System service instance. The service instance serves the license to the client over the network from a pool of floating licenses obtained from the NVIDIA Licensing Portal. The license is returned to the service instance when the licensed client no longer requires the license.
  * **Node locked-licensing:** A client system without a network connection or on an air-gapped network can obtain a node-locked NVIDIA vGPU software license from a file installed locally on the client system. 
Note:
Support for node-locked licensing was introduced in NVIDIA vGPU software 15.0. It is **not** supported in earlier NVIDIA vGPU software releases. 


###  [1.1.1. Pre-Requisites for using vGPU 18 or Later](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#pre-reqs-for-vgpu-18)
  1. Upgrade to DLS 3.4 or Later 
     * To avoid licensing failures with vGPU 18.0 and later releases, upgrade your license server to a minimum version of DLS 3.4 _before_ upgrading vGPU to version 18.0 or later.
     * Refer to the [_Migrating a DLS Instance_](https://docs.nvidia.com/license-system/dls/3.4.0/nvidia-dls-user-guide/index.html#migrating-dls-instance) section of the DLS 3.4 documentation for upgrade instructions.
  2. Update the License Server File (DLS only) 
     * After upgrading your DLS instance to version 3.4 or later: 
       * Download a fresh license server file from the [_NVIDIA Licensing Portal_](https://nvid.nvidia.com/dashboard)
       * Install the license server file on your newly upgraded DLS appliance.
     * Refer to the [_Installing a License Server on a DLS Instance_](https://docs.nvidia.com/license-system/dls/3.4.0/nvidia-dls-user-guide/index.html#installing-license-server-on-dls-instance) section of the NLS 3.4 documentation for instructions.


###  [NLS Security Policy Enhancement](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#unique_512421771)
  * For NLS release 3.4 and later, the license server files bound to DLS SI will have a validity of three years.
  * The three-year validity will begin from the time the file is downloaded from the NLP portal.
  * The NVIDIA Licensing Portal will display the validity period in the details section for license servers bound to DLS SI after its download. 
    * The validity period for the pre-existing license servers which are already downloaded will not be displayed unless it is downloaded again.


Note:
Please contact NVIDIA Support for any required updates to the security policy and provide the appropriate business justification. 
###  [1.2. About License Server Instances (CLS/DLS)](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-service-instances)
A license server instance is required to serve licenses to licensed clients.
NVIDIA License System supports the following types of license server instances: 
  * **Cloud License Service (CLS) instance.** A CLS instance is hosted on the NVIDIA Licensing Portal.
  * **Delegated License Service (DLS) instance.** A DLS instance is hosted on-premises at a location that is accessible from your private network, such as inside your data center.


For example, you can deploy license server instances in distinct physical locations by deploying a DLS instance in each of your data centers. You can also use a mixture of CLS and DLS instances to serve your licenses to licensed clients. 
Note:
You cannot mix CLS and DLS instances in a high availability (HA) cluster. 
###  [1.2.1. About Cloud License Service (CLS) Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-cls-instances)
A Cloud License Service (CLS) instance is hosted on the NVIDIA Licensing Portal.
Because a CLS instance is hosted on the NVIDIA Licensing Portal, you do not need to download licenses from the NVIDIA Licensing Portal and upload them to the instance. 
Hosting a CLS instance on a cloud service provides robustness and dynamic scalability for the CLS instance. Because a CLS instance is maintained by NVIDIA and the cloud service provider, feature and maintenance updates are generally transparent to users. 
###  [1.2.2. About Delegated License Service (DLS) Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-dls-instances)
A Delegated License Service (DLS) instance is hosted on-premises at a location that is accessible from your private network, such as inside your data center.
Because a DLS instance is fully disconnected from the NVIDIA Licensing Portal, you must download licenses from the NVIDIA Licensing Portal and upload them to the instance manually. 
###  [1.3. About the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-nvidia-licensing-portal)
The NVIDIA Licensing Portal provides access to the entitlements that you purchased and the licenses that they contain.
To be able to download NVIDIA vGPU software licenses, you must create at least one license allocation on the NVIDIA Licensing Portal and allocate licenses in your entitlements to the allocation. You can also distribute your licenses across multiple license allocations as necessary, add new licensed products to an existing allocation, and delete license allocations that you no longer require. 
To help you manage your entitlements and licenses on the NVIDIA Licensing Portal, you can add other users as registered contacts in the organization associated with your NVIDIA Enterprise Account. To secure your entitlements and licenses, NVIDIA Licensing Portal provides role-based access for all registered contacts. For more information, see [Managing Contacts on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#managing-users-nvidia-license-portal "To help you manage your entitlements and licenses on the NVIDIA Licensing Portal, you can add other users as registered contacts in the organization associated with your NVIDIA Enterprise Account. You can also remove users who no longer require access from your account on the NVIDIA Licensing Portal."). 
By default, all entitlements are associated with a top-level organization and are accessible to all contacts in the organization. If you need to allow only specific groups of contacts within your organization to access specific entitlements, you can partition your entitlements into isolated segments. However, if a single collection of entitlements that spans your entire organization meets your business needs, you can leave all your entitlements in the top-level organization. 
To partition your entitlements into isolated segments, NVIDIA Licensing Portal provides the ability to create virtual groups and assign entitlements and contacts to them. For more information, see [Managing Virtual Groups](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#managing-virtual-groups "Virtual groups provide the means for segmenting your organization's entitlements into partitions. The virtual groups in an organization are isolated from each other and from the organization. An entitlement cannot be partitioned and cannot be shared between partitions. All licensed products in an entitlement are moved with the entitlement when the entitlement is added to a virtual group or returned to the organization."). 
###  [1.4. High Availability for NVIDIA License System DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#nls-service-instance-high-availability)
To provide licensed clients with continued access to licenses if a DLS instance fails, you can configure DLS instances for high availability.
For resilience **within a single Kubernetes service instance** (multiple application copies and a highly available database), see [Deploying Multi-Replica DLS Instance on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-multi-replica-dls-instance-on-kubernetes-platforms). For an HA cluster of **separate** DLS instances, see [Configuring an HA Cluster of DLS Instances](https://sw-docs-dgx-station.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ha-cluster-dls-instances)
High availability requires at least two DLS instances in a failover configuration: 
  * A primary DLS instance, which is actively serving licenses to licensed clients
  * One or more secondary DLS instances, which act as a backup for the primary DLS instance


Note:
  * NVIDIA License System supports a maximum cluster size of nine DLS instances.
  * Support for clusters of more than two instances was introduced in NVIDIA vGPU software 16.1. Earlier NVIDIA vGPU software releases support clusters of only **two** instances.


Configuring multiple DLS instances in a failover configuration increases availability because simultaneous failure of multiple instances is rare. The primary and secondary DLS instances work together to ensure that licenses in the enterprise remain continually available to licensed clients. 
If the primary DLS instance fails, failover occurs. One of the secondary DLS instances becomes the primary instance and begins to serve licenses. In a cluster of more than two instances, the secondary instance that has the longest uptime becomes the primary instance. The DLS instance that failed becomes a secondary instance when it is returned to service. The next time that failover occurs, the primary DLS instance becomes a secondary DLS instance again. 
Note:
To ensure that licenses in the enterprise remain continually available after failure of the primary DLS instance, return the failed DLS instance to service as quickly as possible to restore high availability support. After failure of a DLS instance in a cluster of two instances, the remaining instance becomes a single point of failure. 
During normal operation, the primary DLS instance continually updates the secondary DLS instances with information about the licenses that are being served to clients. 
For more information about configuring DLS instances for high availability, see [Configuring an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ha-cluster-dls-instances "To provide licensed clients with continued access to licenses if a DLS instance fails, you can configure a highly available \(HA\) cluster of two or more DLS instances in a failover configuration. A failover configuration consists of a primary instance, which is actively serving licenses to licensed clients, and one or more secondary instances, which act as a backup for the primary instance."). 
To enable transparent client failover, you can configure Virtual IP Management for your HA cluster. Virtual IP Management automatically migrates a floating IP address between cluster nodes during failover events, eliminating the need for client reconfiguration. For more information, see Configuring Virtual IP Management for High Availability. 
###  [1.5. Heartbeat Checks for DLS Failover](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#failover)
In a High Availability (HA) cluster, the Heartbeat service, running on every node, monitors the health of DLS appliances and triggers a failover when an unhealthy node is detected based on one of the following criteria. In the event of a failover, one of the secondary nodes assumes the role of the primary node to serve the requests, and the previous primary node takes on the role of the secondary node.
###  Failover Criteria 
  * **Service health check using health end point of service:**
    * This check verifies whether all services are running in a healthy state and are responsive.
    * Every service has a health endpoint. The Heartbeat service will monitor the endpoint to check if the service is responsive.
  * **Database connection check:**
    * The database connection check verifies whether the database is responsive.
    * As part of this check, a lightweight query will be executed.
    * If the database is not responsive, it will result in a client request failure.
  * **Resource exhaustion check:**
    * This check verifies whether any resources (CPU, RAM, and disk) have exceeded the predefined thresholds.
    * Exhaustion of resources impacts system stability, so this is a proactive action for a failover.
  * **Application failure:**
    * Application failure check verifies whether client requests are serviced successfully.
    * If the percentage of failures over a predefined time interval crosses the predefined threshold, a failover gets triggered.
    * This is distinct from the service health check, which verifies if the service is responsive.
    * This check monitors failures in servicing requests. Only authentication and lease operations are monitored for application failure.
  * **Secondary node monitoring of primary node health:**
    * This check verifies if the primary node is running and if the lease service is responsive.
    * As part of the lease response check, a read query (`get lease count`) is executed by the lease monitoring service.


In summary, a failover occurs when any of the following conditions is detected: 
  * Services are not responsive.
  * The database is not responsive.
  * Resources are exhausted.
  * Failures are observed for authentication and lease operations.
  * A secondary node observes that the primary node is down.


###  [1.5.1. Recovery Actions for DLS Failover](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#recovery-actions-dls-failover)
You can perform the following actions for recovering a DLS virtual appliance from a failover.
  * **Resource exhaustion:**
Take appropriate actions based on the respective resources:
    * For CPU or RAM exhaustion: Increase the assigned vCPU or RAM, or check the maximum supported load for a single DLS instance and reduce the load.
For the performance numbers, see [Sizing Guidelines for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-sizing-guidelines "Use the measured performance numbers to determine the optimum configuration for your container-based or VM-based DLS appliances based on the expected number and frequency of requests from licensed clients.").
    * For disk space exhaustion: Increase the available disk space or free up more disk space.
For the disk space cleanup script, see [Expanding the Disk Space on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-disk-expansion "You can use the management interface to the NVIDIA Licensing application to expand the disk space of the DLS virtual appliance.").
    * Application restart might help free up RAM and CPU resources.
  * **Database connection failure:**
    * Restart the application to bring the appliance to a healthy state if the database is recoverable.
    * Check for resource limits, such as memory, CPU, and disk space, to ensure they are not exhausted in unlikely scenarios where failover check of resource exhaustion does not detect in advance.
    * Follow the instructions described in [Restarting the DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#restarting-dls-virtual-appliance "To restart a DLS virtual appliance, choose an appropriate action based on your appliance.") to restart.
    * To verify if the database has been restored, you can do either of the following checks: 
      * Check whether the user interface is performing correctly.
      * Check database connection by using the health endpoint after 10 minutes.
Copy
Copied!

```
            
            
curl -k --location "https://<Node_IP>/service_instance_manager/v1/health?op=db"

        
```

The expected response code is displayed as follows:
Copy
Copied!

```
            
            
{"isServiceHealthy":true}

        
```

  * **Service health check failure:**
    * Restart the application to bring the appliance to a healthy state by following the instructions described in [Restarting the DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#restarting-dls-virtual-appliance "To restart a DLS virtual appliance, choose an appropriate action based on your appliance.").
  * **Application failure:**
    * Restart the application to bring the appliance to a healthy state by following the instructions described in [Restarting the DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#restarting-dls-virtual-appliance "To restart a DLS virtual appliance, choose an appropriate action based on your appliance.").
  * **Power failure:**
    * Ensure that power is restored.


If none of the preceding actions can recover the appliance successfully, contact NVIDIA Enterprise Support. 
###  [1.5.2. Restarting the DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#restarting-dls-virtual-appliance)
To restart a DLS virtual appliance, choose an appropriate action based on your appliance.
####  VM-Based DLS Appliances 
There are two methods to initiate the restart: 
  * Method 1: Reboot the VM.
  * Method 2: Log in using the rsu_admin user, and then execute the `systemctl` command to restart applicationStartup. 
Copy
Copied!

```
            
            
sudo systemctl restart applicationStartup

        
```



####  Container-Based DLS Appliances 
  * **Docker containers:**
Run the following command to restart.
Copy
Copied!

```
            
            
$ docker-compose -f /path/to/docker-compose.yml restart

        
```



## [2. Installing and Configuring the DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-configuring-dls-virtual-appliance)
To simplify the installation and administration of the DLS, the DLS is distributed as a software image to be installed or deployed on a supported platform. The DLS is a secure, hardened environment in which access to the application software is strictly controlled. 
The following types of DLS software image are available: 
  * A virtual appliance image to be installed in a VM on a supported hypervisor
  * A containerized software image for deployment on a supported container orchestration platform
  * An installable package for installation on a supported OS running in a virtualized server environment on your choice of hypervisor or on a bare-metal server


Each DLS software image is configured with a single standard user account for accessing the DLS appliance and an account with sudo user privileges for installing updates to the DLS appliance software. Modifications to these accounts are strictly controlled. You cannot add other user accounts to the software image. However, you can use a lightweight directory access protocol (LDAP) directory instead of the configured accounts for managing user access to a DLS appliance. DLS Virtual Appliance supports user account management at two levels: **OS-level user accounts** (dls_admin, rsu_admin) exist on the underlying operating system and are used for appliance management, while **application-level user accounts** (introduced in DLS 3.6.0) are created via DLS Web UI or API for accessing DLS management interfaces. Application users exist only in the DLS application database and support role-based access control (RBAC) with three pre-defined roles: DLS_ADMIN, DLS_OPERATOR, and DLS_USER. 
In each VM-based virtual appliance image, the DLS application software is containerized to allow limited access to the OS so that non-root users can install security compliance and scanning tools in the VM. However, a container orchestration platform cannot control or restrict access to the OS on which the platform is running. 
In the package for installation on a supported OS, the DLS application software is containerized to ensure that all software dependencies are met and to isolate the DLS application software from the underlying OS. 
As far as possible, the DLS appliances based on all types of software image are functionally equivalent. Therefore, whether to use a container-based DLS appliance, a VM-based DLS appliance, or a DLS appliance from a package for installation on a supported OS depends on the requirements of your IT infrastructure or the policies of your IT department. 
For additional guidelines, refer to the following resources from the vendors of platforms that support NVIDIA License System: 
  * **Citrix:**[Containers or Virtual Machines? It Doesn’t Have to Be One or the Other.](https://www.citrix.com/blogs/2016/05/12/containers-or-virtual-machines-its-not-necessarily-one-or-the-other-get-the-best-of-both-worlds/)
  * **Microsoft:**[Containers vs. virtual machines](https://docs.microsoft.com/en-us/virtualization/windowscontainers/about/containers-vs-vm)
  * **Red Hat:**[Containers vs VMs](https://www.redhat.com/en/topics/containers/containers-vs-vms)
  * **VMware:**[Why use containers vs. VMs?](https://www.vmware.com/topics/glossary/content/vms-vs-containers.html)


###  [2.1. Platform Requirements for a DLS Virtual Appliance or Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-platform-requirements)
Before proceeding, ensure that you have a platform suitable for hosting a DLS virtual appliance or containerized DLS software image.
  * The hosting platform must be a physical host running a supported hypervisor or container orchestration platform.
  * The minimum resource requirements for the VM in which the DLS virtual appliance or DLS container will run is as follows: 
    * **Number of vCPUs:** 4
    * **RAM:** 8 Gbytes
    * **Disk Size:** 15 Gbytes
For additional guidelines, refer to [Sizing Guidelines for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-sizing-guidelines "Use the measured performance numbers to determine the optimum configuration for your container-based or VM-based DLS appliances based on the expected number and frequency of requests from licensed clients.").
  * The platform must have a fixed (unchanging) IP address. The IP address may be assigned dynamically by DHCP or statically configured, but must be constant.
  * The platform’s date and time must be set accurately. NTP is recommended.


Note:
Before proceeding with the installation, refer to [NVIDIA License System Release Notes](https://docs.nvidia.com/license-system/latest/nvidia-license-system-release-notes/index.html) for details of supported hypervisors, details of supported container orchestration platforms, and known issues. 
###  [2.2. Host Name Resolution Requirements for a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#host-name-resolution-requirements-for-dls-virtual-appliance)
The platform that hosts a DLS virtual appliance must be identified by its IP address or its fully qualified domain name. It can also be identified by its CNAME. If you want to identify the platform by its fully qualified domain name, ensure that the required DNS entries are set before installing the DLS virtual appliance. If you want to identify the platform by its default host name, you must set a DNS entry that maps the default host name to the fully qualified domain name.
The process for setting these DNS entries is separate from the process for installing the DLS virtual appliance. Use the standard interfaces of the name resolution service that you are using to set the required DNS entries. 
Tip:
Whenever possible, set DNS entries for the platform that hosts a DLS virtual appliance. If the DNS entries are set, the DLS appliance can be accessed through both its IP address and its fully qualified domain name. 
###  Determining Whether the Forward Pointer and Reverse Pointer DNS Entries Are Correct 
For each mapping between a domain name and an IP address, ensure that you set both the forward pointer and reverse pointer DNS entries. A DLS virtual appliance requires the reverse pointer entry to determine the domain name of the DLS virtual appliance when creating a client configuration token. 
Note:
For the reverse pointer DNS entry on a Windows DNS server, use all lowercase for the fully qualified domain name. 
To determine whether the forward pointer and reverse pointer DNS entries have been set correctly, type the following commands in a shell on any UNIX or Linux host on the same network as the DLS virtual appliance: 
  * For the forward pointer entry, type:
Copy
Copied!

```
            
            
$ host _domain-name_

        
```


_domain-name_ 
    The domain name for which you want to determine whether the forward pointer DNS entry is correct.
If the DNS entry has been set correctly, the command displays the IP address that is mapped to the domain name.
  * For the reverse pointer entry, type:
Copy
Copied!

```
            
            
$ host _ip-address_

        
```


_ip-address_ 
    The IP address for which you want to determine whether the reverse pointer DNS entry is correct.
If the DNS entry has been set correctly, the command displays the domain name that is mapped to the IP address.


###  How a DLS Instance Determines Whether a Fully Qualified Domain Name Is Set 
How a DLS instance determines whether a fully qualified domain name is set depends on the type of DLS instance. 
  * **VM-based DLS virtual appliance:** When the VM that hosts a DLS instance starts, the DLS instance checks whether a fully qualified domain name is mapped to the IP address of the VM. If a name is mapped to the IP address of the VM, the DLS instance retrieves the name to display in the user interface of the NVIDIA Licensing application on the appliance.
  * **Containerized DLS Software Image:** The fully qualified domain name is set through an environment variable. When the container within which the DLS software image is deployed is started, the DLS instance checks whether this environment variable is set.
Note:
Licensed clients outside the container must be able to resolve the fully qualified domain name. The environment variable **must not** specify a name that can be resolved only by the container orchestrator. 
    * If this environment variable is set, the DLS instance retrieves the name to display in the user interface of the NVIDIA Licensing application on the appliance.
    * If this environment variable is not set, a user **cannot** connect to the container in which the DLS software image is deployed through a fully qualified domain name, even if a fully qualified domain name is mapped to an IP address in the DNS server.
For more information, refer to [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.").


###  How the Host Name of the VM or Container that Hosts a DLS Instance Is Set 
How the host name of the VM or container that hosts a DLS instance is set depends on the type of DLS instance. 
  * **VM-based DLS virtual appliance:** The host name is preset in the virtual appliance image. 
    * The host name of a standalone DLS virtual appliance is preset to `nls-si-0`.
    * The host names of the DLS virtual appliances in an HA cluster are preset to `nls-si-0` and `nls-si-1`.
The host name can be changed as explained in [Changing the Host Name of a VM-Based DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-host-name-of-vm-based-dls-virtual-appliance "The host name of a VM-based DLS virtual appliance is preset in the virtual appliance image. If you require a specific host name, you can change the name from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose.").
  * **Containerized DLS Software Image:** The host name is set through an environment variable. If this environment variable is not set, the container orchestration platform sets the host name to the container ID. For more information, refer to [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.").
  * **CNAME Identification:** The platform that hosts a DLS virtual appliance can also be identified by its CNAME.


###  [2.3. Communications Ports Requirements](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#communications-ports-requirements-nls)
To enable communication between a licensed client and a CLS or DLS instance, specific ports must be open in your firewall or proxy server. If you are using an HA cluster of DLS instances with a firewall or proxy server between the DLS instances, additional ports must be open in your firewall or proxy server.
###  Communications Ports Between a Licensed Client and a CLS Instance 
To enable communication between a licensed client and a CLS instance, the following ports must be open in your firewall or proxy server:   
| Port  | Protocol  | Egress / Ingress  | Protocol / Service  | From  | To  |  
| --- | --- | --- | --- | --- | --- |  
| 80  | TLS, TCP  | Egress  | License release  | Client  | CLS  |  
| 443  | TLS, TCP  | Egress  |  License acquisition  License renewal  | Client  | CLS  |  
Note:
  * Port 80 is used by all Windows VMs to release the licenses during the time the VM is shutdown.
  * Port 443 is used for returning the license at the time of license renewal and for license releases during normal operations by all VMs except Windows VMs.


###  Communications Ports Between a Licensed Client and a DLS Instance 
To enable communication between a licensed client and a DLS instance, the following ports must be open in your firewall or proxy server:   
| Port  | Protocol  | Egress / Ingress  | Protocol / Service  | From  | To  |  
| --- | --- | --- | --- | --- | --- |  
| 80  | TLS, TCP  | Egress  | License release  | Client  | DLS  |  
| 443  | TLS, TCP  | Egress  |  License acquisition  License renewal  License release  | Client  | DLS  |  
Note:
  * Port 80 is used by all Windows VMs to release the licenses during the time the VM is shutdown.
  * Port 443 is used for returning the license at the time of license renewal and for license releases during normal operations by all VMs except Windows VMs.
  * The following ports for client to DLS connections are no longer required, but are supported for backward compatibility: 8081, 8082.
  * For a container-based DLS appliance, ports 80 and 443 might not be available for the DLS appliance to bind to, either because the container orchestrator administrator has restricted access to these ports or because the ports are in use by other containers. In this situation, you must set the environment variables `DLS_HTTP_PORT` and `DLS_HTTPS_PORT` to the ports that the DLS appliance must bind to. For more information, refer to [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.").


###  Communications Ports Between DLS Instances in an HA Cluster 
If you are using an HA cluster of DLS instances with a firewall or proxy server between the DLS instances, the following ports must also be open in the firewall or proxy server:   
| Port  | Containerized Instance  | VM-Based Instance  | Protocol  | Egress/Ingress  | Protocol/Service  | From  | To  |  
| --- | --- | --- | --- | --- | --- | --- | --- |  
| 443  | -  | ✔  | TLS, TCP  | Both  | Licensing Services  | Primary DLS  | Secondary DLS  |  
| 4369  | ✔  | ✔  | EPMD (peer discovery)  | Both  | RabbitMQ, Erlang  | Primary DLS  | Secondary DLS  |  
| 5671  | ✔  | ✔  | AMQP, TLS  | Both  | RabbitMQ  | Primary DLS  | Secondary DLS  |  
| 8080  | ✔  | ✔  | HTTPS  | Both  | Licensing Services  | Primary DLS  | Secondary DLS  |  
| 8081  | ✔  | ✔  | HTTPS  | Both  | Licensing Services  | Primary DLS  | Secondary DLS  |  
| 8082  | ✔  | ✔  | HTTPS  | Both  | Licensing Services  | Primary DLS  | Secondary DLS  |  
| 8083  | ✔  | ✔  | HTTPS  | Both  | Licensing Services  | Primary DLS  | Secondary DLS  |  
| 8084  | ✔  | ✔  | HTTPS  | Both  | Licensing Services  | Primary DLS  | Secondary DLS  |  
| 8085  | ✔  | ✔  | HTTPS  | Both  | Licensing Services  | Primary DLS  | Secondary DLS  |  
✔ Port is used. 
- Port is not used. 
The following ports that were required to be open in earlier releases are no longer used: 
  * 22
  * 1883
  * 5672
  * 8883
  * 15672
  * 25671
  * 25672
  * 61613
  * 61614


###  Communications Ports in the Container Orchestrator 
The following ports must be available in the container orchestrator to allow the DLS appliance container to bind to them:   
| Port  | Service  |  
| --- | --- |  
| 8080, 18080  | Administration Service  |  
| 8081, 18081  | Authorization Service  |  
| 8082, 18082  | Licensing Services  |  
| 8083, 18083  | File Installation  |  
| 8084, 18084  | Service Instance  |  
| 8085  | Virtual Appliance Service  |  
| 9001  | Process Manager (supervisord)  |  
| 25672  | RabbitMQ  |  
###  [2.4. Sizing Guidelines for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-sizing-guidelines)
Use the measured performance numbers to determine the optimum configuration for your container-based or VM-based DLS appliances based on the expected number and frequency of requests from licensed clients.
Note:
Sections 2.4.1 through 2.4.3 apply to a single VM-based or container-based DLS appliance. For a DLS multi-replica deployment, which is scaled by the number of pods rather than by the size of one instance, see Default Resource Configuration for a DLS Multi-Replica Deployment, Scalability for a DLS Multi-Replica Deployment, and Burst Load Performance for a DLS Multi-Replica Deployment. 
Note:
For the corresponding data for a CLS instance, refer to [Performance Data for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#performance-reliability-data-cls-instance "Use the measured performance numbers to determine whether the CLS meets your requirements based on the number and frequency of requests from licensed clients. Hosting a CLS instance on a cloud service provides robustness and dynamic scalability for the CLS instance. The cloud service dynamically scales CLS instances to maintain the measured performance numbers."). 
###  [2.4.1. Throughput for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-throughput)
Throughput measures the number of clients that a VM-based or container-based DLS appliance can process in 1 second.
Note:
  * All measurements were taken with a CPU clock speed of 3 GHz.
  * For a VM-based DLS appliance, the disk size is preset to 15 Gbytes in the virtual appliance image. If you want to retain diagnostic log files for longer than the default period of three months, increase the disk size in the appliance.
  * The measurements for a container-based DLS appliance were taken from a Docker container in an Ubuntu VM. Only the containers for the DLS appliance were running in the VM while the measurements were taken.
  * If you increase the number of vCPUs, you must increase the total RAM in GB to twice the number of vCPUs. For a container-based DLS appliance, increasing the number of vCPUs increases the number of worker threads. If the number of vCPUs is increased without an increase in the total RAM, some workers or services might fail.

  
| Number of vCPUs  | Total RAM (GB)  | RAM Consumed (GB)  | Throughput (Clients per Second)  | Average Response Time (ms)  |  
| --- | --- | --- | --- | --- |  
| 4  | 8  | 4.5  | 5  | 74  |  
| 6  | 12  | 6  | 9  | 84  |  
| 8  | 16  | 8  | 13  | 87  |  
###  [2.4.2. Scalability for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-scalability)
Scalability measures the maximum number of licensed clients that a VM-based or container-based DLS appliance can serve in a specific interval. A DLS appliance serves a licensed client by performing a licensing operation for the client, namely the borrowing, return, or renewal of a license. Registration of a licensed client is not considered a licensing operation because it occurs only once for any client.
These measurements capture the maximum number of licensed clients that DLS appliances with varying numbers of vCPUs and amounts of RAM can serve when licenses are borrowed for 12 hours. The maximum number of clients is directly proportional to the length of time for which licenses are borrowed. If the length of time increases, the maximum number of clients also increases. 
To calculate the maximum number of clients from the length of time for which licenses are borrowed, use the following formula: 
_max-clients_ =_clients-per-second_ ⨯60⨯(_renewal-interval-percentage_ /100)⨯ _borrow-time-in-hours_ ⨯60  

_max-clients_ 
    The maximum number of licensed clients that a DLS appliance can serve. 

_clients-per-second_ 
    The number of clients per second that the DLS appliance can serve as measured by scalability runs for each configuration. Use the number of clients per second in the table for the appropriate configuration. 

_renewal-interval-percentage_ 
     The percentage of the length of time for which a license is borrowed to which the renewal interval is set. 
When these measurements were taken, the renewal interval was set to 15% of the length of time in hours for which licenses were borrowed. 

_borrow-time-in-hours_ 
     The length of time in hours for which licenses are borrowed. 
When these measurements were taken, licenses were borrowed for 12 hours.
This formula prevents the accumulation of requests on a DLS instance, ensuring smooth operation. 
Note:
  * The measurements were taken with the throughput measured in [Throughput for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-throughput "Throughput measures the number of clients that a VM-based or container-based DLS appliance can process in 1 second.").
  * All measurements were taken with a CPU clock speed of 2.6 GHz.
  * The measurements for a container-based DLS appliance were taken from a Docker container in an Ubuntu VM. Only the containers for the DLS appliance were running in the VM while the measurements were taken.

  
| Number of vCPUs  | Total RAM (GB)  | Clients per Second  | Calculation  | Maximum Number of Clients  |  
| --- | --- | --- | --- | --- |  
| 4  | 8  | 5  | 5⨯60⨯0.15⨯12⨯60  | 32,000  |  
| 6  | 12  | 9  | 9⨯60⨯0.15⨯12⨯60  | 58,000  |  
| 8  | 16  | 13  | 13⨯60⨯0.15⨯12⨯60  | 84,000  |  
###  [2.4.3. Burst Load Performance for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-burst-load-performance)
Burst load performance measures the time that a VM-based or container-based DLS appliance requires to process the requests received from a large number of clients in a short interval of time. Burst load performance does **not** measure concurrency or throughput.
Note:
  * Burst processing times are illustrative only because they are for retry logic in performance tests that use simulated client drivers. Times may differ with real client drivers.
  * The test systems were configured with 4 vCPUs and 8 GB of RAM.
  * The measurements for a container-based DLS appliance were taken from a Docker container in an Ubuntu VM. Only the containers for the DLS appliance were running in the VM while the measurements were taken.

  
| Number of Clients  | Interval  | Processing Time  |  
| --- | --- | --- |  
| 100  | 0-1 second  | 5 seconds  |  
| 1,000  | 0-20 seconds  | 50 seconds  |  
| 5,000  | 0-5 minutes  | 5 minutes  |  
| 10,000  | 0-10 minutes  | 8 minutes and 42 seconds  |  
###  [2.4.4. Default Resource Configuration for a DLS Multi-Replica Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#default-resource-configuration-for-dls-multi-replica-deployment)
A DLS multi-replica deployment consists of DLS appliance pods and DLS database pods, each of which is deployed with a default CPU limit and a default memory limit. All the performance numbers in the sections that follow were measured with these default limits in place.   
| **Pod**  | **Default CPU Limit (Cores)**  | **Default Memory Limit**  |  
| --- | --- | --- |  
| DLS appliance pod  | 4  | 5.5 GB (5,632 MiB)  |  
| DLS database pod  | 4  | 2.5 GB (2,560 MiB)  |  
Note:
  * In the default deployment, the CPU request and the memory request for each pod are equal to the corresponding limit.
  * You can increase the CPU limit and the memory limit for the appliance pods and the database pods to allocate more resources to a DLS multi-replica deployment. If you change the default limits, the measured numbers in Throughput for a DLS Multi-Replica Deployment and Burst Load Performance for a DLS Multi-Replica Deployment no longer apply.
  * Each node on which a pod is scheduled must have enough allocatable CPU and memory to satisfy the limits configured for that pod.


###  [2.4.5. Scalability for a DLS Multi-Replica Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#scalability-for-dls-multi-replica-deployment)
Scalability measures the maximum number of licensed clients that a DLS multi-replica deployment can serve in a specific interval. A DLS multi-replica deployment serves a licensed client by performing a licensing operation for the client, namely the borrowing, return, or renewal of a license. Registration of a licensed client is not considered a licensing operation because it occurs only once for any client. 
These measurements capture the maximum number of licensed clients that DLS multi-replica deployments with varying numbers of appliance pods and database pods can serve when licenses are borrowed for 12 hours. The maximum number of clients is directly proportional to the length of time for which licenses are borrowed. If the length of time increases, the maximum number of clients also increases. 
To calculate the maximum number of clients from the length of time for which licenses are borrowed, use the following formula: 
**max-clients = clients-per-second ⨯ 60 ⨯ (renewal-interval-percentage / 100) ⨯ borrow-time-in-hours ⨯ 60**
**max-clients** The maximum number of licensed clients that a DLS multi-replica deployment can serve. 
**clients-per-second** The number of clients per second that the DLS multi-replica deployment can serve as measured by scalability runs for each topology. Use the number of clients per second in the table for the appropriate topology. 
**renewal-interval-percentage** The percentage of the length of time for which a license is borrowed to which the renewal interval is set. 
When these measurements were taken, the renewal interval was set to 15% of the length of time in hours for which licenses were borrowed. 
**borrow-time-in-hours** The length of time in hours for which licenses are borrowed. 
When these measurements were taken, licenses were borrowed for **12 hours.**
This formula prevents the accumulation of requests on a DLS multi-replica deployment, ensuring smooth operation. 
Note:
  * The measurements were taken with the throughput measured in Throughput for a DLS Multi-Replica Deployment.
  * All measurements were taken with the default per-pod CPU and memory limits described in Default Resource Configuration for a DLS Multi-Replica Deployment.

  
| **Number of DLS Appliance Pods**  | **Number of DLS Database Pods**  | **Clients per Second**  | **Calculation**  | **Maximum Number of Clients**  |  
| --- | --- | --- | --- | --- |  
| 3  | 3  | 40  | 40⨯60⨯0.15⨯12⨯60  | 259,000  |  
| 4  | 3  | 70  | 70⨯60⨯0.15⨯12⨯60  | 453,000  |  
###  [2.4.6. Burst Load Performance for a DLS Multi-Replica Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#burst-load-performance-for-dls-multi-replica-deployment)
Burst load performance measures the ability of a DLS multi-replica deployment to process the requests received from a large number of clients in a short interval of time. Burst load performance does not measure concurrency or throughput. 
For a burst of 10,000 licensed clients, the following table gives the recommended interval over which the clients should be started for each supported topology. If the clients are started over a shorter interval than the recommended interval, licensing operations might not complete within the interval and average response times increase. 
Note:
  * Burst processing times are illustrative only because they are for retry logic in performance tests that use simulated client drivers. Times may differ with real client drivers.
  * All measurements were taken with the default per-pod CPU and memory limits described in Default Resource Configuration for a DLS Multi-Replica Deployment.

  
| **Number of DLS Appliance Pods**  | **Number of DLS Database Pods**  | **Number of Clients**  | **Interval**  | **Processing Time**  |  
| --- | --- | --- | --- | --- |  
| 3  | 3  | 10,000  | 0-5 minutes  | 5 min 4 s  |  
| 4  | 3  | 10,000  | 0-4 minutes  | 4 min 26 s  |  
###  [2.5. Installing a VM-Based DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-vm-based-dls-virtual-appliance)
To simplify the installation process, a VM-based DLS virtual appliance is supplied as a virtual appliance image to be installed on a supported hypervisor.
After verifying the requirements in [Platform Requirements for a DLS Virtual Appliance or Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-platform-requirements "Before proceeding, ensure that you have a platform suitable for hosting a DLS virtual appliance or containerized DLS software image.") and reviewing the guidelines in [Sizing Guidelines for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-sizing-guidelines "Use the measured performance numbers to determine the optimum configuration for your container-based or VM-based DLS appliances based on the expected number and frequency of requests from licensed clients."), install and configure the DLS virtual appliance by following this sequence of instructions: 
  1. [Installing the DLS Virtual Appliance Image on a Supported Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance "DLS virtual appliance images are available for several hypervisors. You use standard interfaces of the hypervisor to install the DLS virtual appliance on your chosen hypervisor.")
  2. **If necessary:**[Setting the IP Address of a DLS Virtual Appliance from the Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address-from-hypervisor "If the VM that hosts a DLS virtual appliance has not been assigned an IP address automatically, you must set the IP address from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose and is configured with a user account for running the script.")
  3. [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.")
  4. **Optional:**[Creating or Expanding an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-ha-cluster-dls-instances "Any time after creating or configuring a standalone DLS instance, you can create an HA Cluster of DLS instances by converting the instance into a node in a cluster and adding a second instance to the cluster. You can also expand an existing cluster by adding a DLS instance to it.")
  5. **Optional:**[Setting the Static IP Address of a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address "You can use the management interface to the NVIDIA Licensing application to replace the existing IP address of the appliance with a new static IP address. The existing IP address can be an address assigned by DHCP or another static IP address.")


Note:
See [DLS Appliance Setup Tool](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool "The DLS Appliance Setup tool consists of standalone programs you run on a Unix computer to configure the Delegated License Service \(DLS\) appliance. The tools register the appliance with the NVIDIA Licensing Portal \(NLP\), bind it to a license allocation, install the license file on the appliance, and can configure appliance settings such as NTP, network file sharing \(NFS\), and LDAP. You do not need to install Python on the computer where you run these tools.") for more information. 
###  [2.5.1. Installing the DLS Virtual Appliance Image on a Supported Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance)
DLS virtual appliance images are available for several hypervisors. You use standard interfaces of the hypervisor to install the DLS virtual appliance on your chosen hypervisor.
The DLS virtual appliance image for each supported hypervisor **except** Red Hat Enterprise Linux KVM specifies the minimum configuration for the VM as listed in [Platform Requirements for a DLS Virtual Appliance or Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-platform-requirements "Before proceeding, ensure that you have a platform suitable for hosting a DLS virtual appliance or containerized DLS software image."). You are not required to specify the VM configuration when you install the DLS virtual appliance for these hypervisors. After installing the DLS virtual appliance, you can use standard interfaces of the hypervisor to change the configuration of the VM if necessary. 
Note:
If subnet conflicts occur, contact your network administrator for an appropriate subnet address and then follow these steps. 
  1. Modify the `back-tier` property of the `networks` element in the /etc/dls/config/docker-compose.yml file using the rsu_admin user.
  2. Restart the VM for the change to take effect.


###  [2.5.1.1. Installing the DLS Virtual Appliance on Citrix Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance-on-citrix-hypervisor)
The DLS image for Citrix Hypervisor is distributed as a ZIP archive that contains an XVA file, which is a format that is specific to Xen-based hypervisors.
Use the Citrix XenCenter Import wizard to perform this task on the Citrix Hypervisor host on which you want to run the DLS virtual appliance. 
For additional information, see [Import VMs From XVA](https://docs.citrix.com/en-us/xencenter/current-release/vms-import.html) on the Citrix product documentation website. 
  1. Download the ZIP archive that contains the XVA file that contains the DLS virtual appliance image to the hypervisor host.
  2. Extract the contents of the ZIP archive that you downloaded.
  3. In Citrix XenCenter, from the **File** menu, choose **Import**.
  4. Browse for and select the downloaded XVA file, and click **Next**.
  5. Select the server on which the imported VM will be placed and click **Next**.
  6. Select the storage repository where the virtual disks for the newly imported VM will be stored and click **Import**.
  7. Select the default virtual network interfaces in the template for the virtual appliance and click **Next**.
  8. Review the settings for importing the virtual machine and click **Finish** to create the virtual machine.
  9. Start the VM that you created.


Allow approximately 15 minutes after the VM is started for the installation of the DLS virtual appliance to complete and for the DLS virtual appliance to start. What to do after the DLS virtual appliance starts depends on whether the VM has been assigned an IP address automatically, for example, by a DHCP server: 
  * If the VM has been assigned an IP address, what to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance:
    * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
    * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").
  * Otherwise, set the IP address of the DLS virtual appliance from the hypervisor as explained in [Setting the IP Address of a DLS Virtual Appliance from the Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address-from-hypervisor "If the VM that hosts a DLS virtual appliance has not been assigned an IP address automatically, you must set the IP address from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose and is configured with a user account for running the script.").


###  [2.5.1.2. Installing the DLS Virtual Appliance on Microsoft Windows Server with Hyper-V](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance-on-windows-server)
The DLS image for Microsoft Windows Server with Hyper-V is distributed as a ZIP archive.
Use Hyper-V Manager to perform this task on the Microsoft Windows Server with Hyper-V host on which you want to run the DLS virtual appliance. 
For additional information, see [Import a Virtual Machine](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/deploy/export-and-import-virtual-machines#import-a-virtual-machine) on the Microsoft documentation website. 
  1. Download the ZIP archive that contains the DLS virtual appliance image to the hypervisor host.
  2. Extract the contents of the ZIP archive that you downloaded.
  3. From the Hyper-V Manager**Actions** menu, choose **Import Virtual Machine**.
  4. Browse for and select the folder to which you extracted the DLS virtual appliance image and click **Next**.
  5. When prompted to choose the type of import, select the **Copy the virtual machine (create a new unique ID)** option and click **Next**.
  6. Browse for and select the folders in which you want to store virtual machine (VM) files and click **Next**. 
  7. Browse for and select the folder for storing the virtual disk. 
Note:
If you are creating two VMs for a cluster of DLS instances, ensure that you choose a unique folder for each VM to prevent errors from Hyper-V Manager. 
  8. Review the settings for importing the VM and click **Finish** to create the VM.
  9. After the VM has been created, specify the virtual switch that the VM should use. 
    1. From the **Actions** menu under the name of the imported VM, choose **Settings**.
    2. Under **Hardware** in the left navigation bar of the Settings window, select **Network Adapter** , select the virtual switch from the **Virtual switch** drop-down list, and click **Apply**.
  10. Start the imported VM. 
    1. From the **Actions** menu under the name of the imported VM, choose **Connect**.
    2. In the Virtual Machine Connection window that opens, click **Start**.


A command window opens when the installation of the imported VM is started. Use this window to log in to the DLS virtual appliance **only** if you need to set the IP address of the DLS virtual appliance from the hypervisor. 
Allow approximately 15 minutes after the VM is started for the installation of the DLS virtual appliance to complete and for the DLS virtual appliance to start. What to do after the DLS virtual appliance starts depends on whether the VM has been assigned an IP address automatically, for example, by a DHCP server: 
  * If the VM has been assigned an IP address, what to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance:
    * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
    * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").
  * Otherwise, set the IP address of the DLS virtual appliance from the hypervisor as explained in [Setting the IP Address of a DLS Virtual Appliance from the Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address-from-hypervisor "If the VM that hosts a DLS virtual appliance has not been assigned an IP address automatically, you must set the IP address from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose and is configured with a user account for running the script.").


After the VM has started, you can get its IP address from the **Networking** tab for the VM in Hyper-V Manager. 
###  [2.5.1.3. Installing the DLS Virtual Appliance on Red Hat Enterprise Linux KVM](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance-on-red-hat-el-kvm)
The DLS image for Red Hat Enterprise Linux KVM is distributed as a ZIP archive that contains a QEMU copy-on-write (QCOW2) image file. After preparing the QCOW2 file, install the image by using Virtual Machine Manager to create a VM from the QCOW2 file.
Perform this task from the hypervisor host. 
  1. Download the ZIP archive that contains the QCOW2 image file to the hypervisor host.
  2. Extract the contents of the ZIP archive that you downloaded.
  3. Copy the QCOW2 image file to the /var/lib/libvirt/images directory on the hypervisor host.
  4. Start Virtual Machine Manager.
  5. Add a connection to the hypervisor host. 
    1. In the Virtual Machine Manager window, from the **File** menu, choose **Add Connection**.
    2. In the Add Connection window that opens, set the options in the following table and click **Connect**.   
| Option  | Setting  |  
| --- | --- |  
| **Hypervisor**  | From the drop-down list, select **QEMU/KVM**.  |  
| **Connect to remote host over SSH**  | Select this option.  |  
| **User Name**  | In this text-entry field, type root.  |  
| **Hostname**  | In this text-entry field, type the IP address or the fully qualified host name of the Red Hat Enterprise Linux KVM host.  |  
The connection is added to the Virtual Machine Manager window.
  6. Context click the connection that you added in the previous step and choose **New**. The Create a new virtual machine wizard starts.
  7. In the first New VM window, select the **Import existing disk image** option and click **Forward**.
  8. In the second New VM window, import the downloaded QCOW2 image file and choose the operating system to install. 
    1. Click **Browse**.
    2. In the Choose Storage Volume window that opens, select the downloaded QCOW2 image file and click **Choose Volume**.
    3. Back in the second New VM window, type Ubuntu 22.04 in the search box and from the list of operating systems that opens, select **Ubuntu 22.04 (ubuntu)**.
    4. Click **Forward**.
  9. In the third New VM window, set **Memory** to 8192 MiB and **CPUs** to 4, and click **Forward**.
  10. In the final New VM window, specify the VM name and the network that the VM will use. 
    1. In the **Name** text-entry field, type your choice of name for the VM that you are creating.
    2. Select the **Customize configuration before install** option.
    3. From the Network Selection drop-down list, select the network that the VM will use.
    4. Click **Finish**.
  11. In the window for reviewing a new VM, modify the VM as follows. 
    1. Remove **USB Redirector 1** , **USB Redirector 2** , and **Channel spice**. For each item to remove, context-click the item in the left navigation bar and from the menu that pops up, choose **Remove Hardware**.
    2. Change **Video QXL** to **VGA**. In the left navigation bar, select **Video QXL** and from the **Model** drop-down list, select **VGA**.
    3. In the the Overview tab update the Firmware to UEFI.
  12. Click **Apply** to save your changes to the configuration and click **Begin Installation**.
  13. After the VM is created, start the VM and log in to the VM to get the IP address of the VM. You must log in to the VM to get its IP address because the IP address is not shown in the Virtual Machine Manager console. 
    1. Click the play button to start the VM on the hypervisor host.
    2. Use the command window that opens when the VM starts to log in to the DLS virtual appliance as the dls_admin user with the preset password welcome.
    3. Get the address of the VM. 
Copy
Copied!

```
            
            
$ ip addr

        
```



Allow approximately 15 minutes after the VM is started for the installation of the DLS virtual appliance to complete and for the DLS virtual appliance to start. What to do after the DLS virtual appliance starts depends on whether the VM has been assigned an IP address automatically, for example, by a DHCP server: 
  * If the VM has been assigned an IP address, what to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance:
    * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
    * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").
  * Otherwise, set the IP address of the DLS virtual appliance from the hypervisor as explained in [Setting the IP Address of a DLS Virtual Appliance from the Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address-from-hypervisor "If the VM that hosts a DLS virtual appliance has not been assigned an IP address automatically, you must set the IP address from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose and is configured with a user account for running the script.").


###  [2.5.1.4. Installing the DLS Virtual Appliance on Red Hat Virtualization](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance-on-rhv)
The DLS image for Red Hat Virtualization is distributed as a ZIP archive that contains a QEMU copy-on-write (QCOW2) image file. After preparing the QCOW2 file, install the image by using Virtual Machine Manager to create a VM from the QCOW2 file.
Perform this task from the hypervisor host. 
  1. Download the ZIP archive that contains the QCOW2 image file to the hypervisor host.
  2. Extract the contents of the ZIP archive that you downloaded.
  3. Copy the QCOW2 image file to the /var/lib/libvirt/images directory on the hypervisor host.
  4. Start Virtual Machine Manager.
  5. Add a connection to the hypervisor host. 
    1. In the Virtual Machine Manager window, from the **File** menu, choose **Add Connection**.
    2. In the Add Connection window that opens, set the options in the following table and click **Connect**.   
| Option  | Setting  |  
| --- | --- |  
| **Hypervisor**  | From the drop-down list, select **QEMU/KVM**.  |  
| **Connect to remote host over SSH**  | Select this option.  |  
| **User Name**  | In this text-entry field, type root.  |  
| **Hostname**  | In this text-entry field, type the IP address or the fully qualified host name of the Red Hat Enterprise Linux KVM host.  |  
The connection is added to the Virtual Machine Manager window.
  6. Context click the connection that you added in the previous step and choose **New**. The Create a new virtual machine wizard starts.
  7. In the first New VM window, select the **Import existing disk image** option and click **Forward**.
  8. In the second New VM window, import the downloaded QCOW2 image file and choose the operating system to install. 
    1. Click **Browse**.
    2. In the Choose Storage Volume window that opens, select the downloaded QCOW2 image file and click **Choose Volume**.
    3. Back in the second New VM window, type Ubuntu 22.04 in the search box and from the list of operating systems that opens, select **Ubuntu 22.04 (ubuntu)**.
    4. Click **Forward**.
  9. In the third New VM window, set **Memory** to 8192 MiB and **CPUs** to 4, and click **Forward**.
  10. In the final New VM window, specify the VM name and the network that the VM will use. 
    1. In the **Name** text-entry field, type your choice of name for the VM that you are creating.
    2. Select the **Customize configuration before install** option.
    3. From the Network Selection drop-down list, select the network that the VM will use.
    4. Click **Finish**.
  11. In the window for reviewing a new VM, modify the VM as follows. 
    1. Remove **USB Redirector 1** , **USB Redirector 2** , and **Channel spice**. For each item to remove, context-click the item in the left navigation bar and from the menu that pops up, choose **Remove Hardware**.
    2. Change **Video QXL** to **VGA**. In the left navigation bar, select **Video QXL** and from the **Model** drop-down list, select **VGA**.
    3. In the the Overview tab update the Firmware to UEFI.
  12. Click **Apply** to save your changes to the configuration and click **Begin Installation**.
  13. After the VM is created, start the VM and log in to the VM to get the IP address of the VM. You must log in to the VM to get its IP address because the IP address is not shown in the Virtual Machine Manager console. 
    1. Click the play button to start the VM on the hypervisor host.
    2. Use the command window that opens when the VM starts to log in to the DLS virtual appliance as the dls_admin user with the preset password welcome.
    3. Get the address of the VM. 
Copy
Copied!

```
            
            
$ ip addr

        
```



Allow approximately 15 minutes after the VM is started for the installation of the DLS virtual appliance to complete and for the DLS virtual appliance to start. What to do after the DLS virtual appliance starts depends on whether the VM has been assigned an IP address automatically, for example, by a DHCP server: 
  * If the VM has been assigned an IP address, what to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance:
    * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
    * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").
  * Otherwise, set the IP address of the DLS virtual appliance from the hypervisor as explained in [Setting the IP Address of a DLS Virtual Appliance from the Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address-from-hypervisor "If the VM that hosts a DLS virtual appliance has not been assigned an IP address automatically, you must set the IP address from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose and is configured with a user account for running the script.").


###  [2.5.1.5. Installing the DLS Virtual Appliance on Ubuntu Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance-on-ubuntu)
The DLS image for Ubuntu Hypervisor is distributed as a ZIP archive that contains a QEMU copy-on-write (QCOW2) image file. After preparing the QCOW2 file, install the image by using Virtual Machine Manager to create a VM from the QCOW2 file.
Perform this task from the hypervisor host. 
  1. Download the ZIP archive that contains the QCOW2 image file to the hypervisor host.
  2. Extract the contents of the ZIP archive that you downloaded.
  3. Copy the QCOW2 image file to the /var/lib/libvirt/images directory on the hypervisor host.
  4. Start Virtual Machine Manager.
  5. Add a connection to the hypervisor host. 
    1. In the Virtual Machine Manager window, from the **File** menu, choose **Add Connection**.
    2. In the Add Connection window that opens, set the options in the following table and click **Connect**.   
| Option  | Setting  |  
| --- | --- |  
| **Hypervisor**  | From the drop-down list, select **QEMU/KVM**.  |  
| **Connect to remote host over SSH**  | Select this option.  |  
| **User Name**  | In this text-entry field, type root.  |  
| **Hostname**  | In this text-entry field, type the IP address or the fully qualified host name of the Red Hat Enterprise Linux KVM host.  |  
The connection is added to the Virtual Machine Manager window.
  6. Context click the connection that you added in the previous step and choose **New**. The Create a new virtual machine wizard starts.
  7. In the first New VM window, select the **Import existing disk image** option and click **Forward**.
  8. In the second New VM window, import the downloaded QCOW2 image file and choose the operating system to install. 
    1. Click **Browse**.
    2. In the Choose Storage Volume window that opens, select the downloaded QCOW2 image file and click **Choose Volume**.
    3. Back in the second New VM window, type Ubuntu 22.04 in the search box and from the list of operating systems that opens, select **Ubuntu 22.04 (ubuntu)**.
    4. Click **Forward**.
  9. In the third New VM window, set **Memory** to 8192 MiB and **CPUs** to 4, and click **Forward**.
  10. In the final New VM window, specify the VM name and the network that the VM will use. 
    1. In the **Name** text-entry field, type your choice of name for the VM that you are creating.
    2. Select the **Customize configuration before install** option.
    3. From the Network Selection drop-down list, select the network that the VM will use.
    4. Click **Finish**.
  11. In the window for reviewing a new VM, modify the VM as follows. 
    1. Remove **USB Redirector 1** , **USB Redirector 2** , and **Channel spice**. For each item to remove, context-click the item in the left navigation bar and from the menu that pops up, choose **Remove Hardware**.
    2. Change **Video QXL** to **VGA**. In the left navigation bar, select **Video QXL** and from the **Model** drop-down list, select **VGA**.
    3. In the the Overview tab update the Firmware to UEFI.
  12. Click **Apply** to save your changes to the configuration and click **Begin Installation**.
  13. After the VM is created, start the VM and log in to the VM to get the IP address of the VM. You must log in to the VM to get its IP address because the IP address is not shown in the Virtual Machine Manager console. 
    1. Click the play button to start the VM on the hypervisor host.
    2. Use the command window that opens when the VM starts to log in to the DLS virtual appliance as the dls_admin user with the preset password welcome.
    3. Get the address of the VM. 
Copy
Copied!

```
            
            
$ ip addr

        
```



Allow approximately 15 minutes after the VM is started for the installation of the DLS virtual appliance to complete and for the DLS virtual appliance to start. What to do after the DLS virtual appliance starts depends on whether the VM has been assigned an IP address automatically, for example, by a DHCP server: 
  * If the VM has been assigned an IP address, what to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance:
    * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
    * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").
  * Otherwise, set the IP address of the DLS virtual appliance from the hypervisor as explained in [Setting the IP Address of a DLS Virtual Appliance from the Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address-from-hypervisor "If the VM that hosts a DLS virtual appliance has not been assigned an IP address automatically, you must set the IP address from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose and is configured with a user account for running the script.").


###  [2.5.1.6. Installing the DLS Virtual Appliance on VMware vSphere](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance-on-vmware-vsphere)
The DLS image for VMware vSphere is distributed as a ZIP archive that contains an Open Virtual Appliance (OVA) file.
Use the VMware vSphere Client to perform this task on the ESXi server on which you want to run the DLS virtual appliance. 
For additional information, see the following topics on the VMware Docs site: 
  * [Log in to vCenter Server by Using the vSphere Web Client](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vcenterhost.doc/GUID-CE128B59-E236-45FF-9976-D134DADC8178.html)
  * [Deploy an OVF or OVA Template](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-17BEDA21-43F6-41F4-8FB2-E01D275FE9B4.html)


  1. Download the ZIP archive that contains the OVA file that contains the DLS image for VMware vSphere.
  2. Extract the contents of the ZIP archive that you downloaded.
  3. Log in to vCenter Server by using the VMware vSphere Client.
  4. From the VMware vSphere Client **Actions** menu, choose **Deploy OVF Template**.
  5. Select the **Local file** option, browse for and select the downloaded OVA file, and click **Next**.
  6. Enter the your choice of virtual machine name, select a location for the virtual machine, and click **Next**.
  7. Select a compute resource where the virtual machine will be created and click **Next**.
  8. Review the details of the template that you are deploying and click **Next**.
  9. Select the storage for the virtual appliance configuration and disk files and click **Next**.
  10. Leave the destination network as-is, set the **IP allocation** option to **Static - Manual** , and click **Next**.
  11. Set the virtual network and IP allocation properties for the VM and click **Next**. 
    1. In the **ipaddress** text-entry field, type the IP address that you want to assign to the VM. 
Note:
To change this address after completing the installation, you **must** follow the instructions in [Changing the IP Address of a VMware VM Set During DLS Appliance Installation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-vmware-vm-ip-address-set-at-dls-installation "The IP address of a VMware VM that hosts a DLS appliance can be set when the appliance is installed from an OVF template. If the IP address was set this way, you can change it only by editing vApps options within VM that hosts the DLS appliance. If you use any other method to change this address, it reverts to its original setting when the VM is restarted."). If you use any other method to change this address, it reverts to its original setting when the VM is restarted. 
    2. In the **netmask** text-entry field, type the subnet mask of the VM's network in classless inter-domain routing (CIDR) format **without the leading slash character (/)**. To get a subnet mask in CIDR format from its decimal equivalent, refer to the table on page 2 of [IETF RFC 1878: Variable Length Subnet Table For IPv4](https://www.ietf.org/rfc/rfc1878.txt). For example, the subnet mask in CIDR format of the decimal equivalent 255.255.255.0 is 24.
    3. In the **gateway** text-entry field, type the IP address of the VM's default gateway. 
Note:
If you leave the **gateway** field empty, the DLS virtual appliance uses DHCP settings. 
    4. **Optional:** In the **dns_server_one** text-entry field, type the IP address of the first DNS server to be used for name resolution.
    5. **Optional:** In the **dns_server_two** text-entry field, type the IP address of the second DNS server to be used for name resolution.
  12. Review all the details of the virtual machine that you are creating and click **Finish**.
  13. Start the VM that you created. 
Note:
If subnet conflicts occur, contact your network administrator for an appropriate subnet address and then follow these steps. 
    1. Modify the `back-tier` property of the `networks` element in the /etc/dls/config/docker-compose.yml file using the rsu_admin user.
    2. Restart the VM for the change to take effect.


Allow approximately 15 minutes after the VM is started for the installation of the DLS virtual appliance to complete and for the DLS virtual appliance to start. What to do after the DLS virtual appliance starts depends on whether the VM has been assigned an IP address automatically, for example, by a DHCP server: 
  * If the VM has been assigned an IP address, what to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance:
    * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
    * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").
  * Otherwise, set the IP address of the DLS virtual appliance from the hypervisor as explained in [Setting the IP Address of a DLS Virtual Appliance from the Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address-from-hypervisor "If the VM that hosts a DLS virtual appliance has not been assigned an IP address automatically, you must set the IP address from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose and is configured with a user account for running the script.").


###  [2.5.2. Setting the IP Address of a DLS Virtual Appliance from the Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address-from-hypervisor)
If the VM that hosts a DLS virtual appliance has **not** been assigned an IP address automatically, you must set the IP address from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose and is configured with a user account for running the script.
Note:
See [DLS Appliance Setup Tool](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool "The DLS Appliance Setup tool consists of standalone programs you run on a Unix computer to configure the Delegated License Service \(DLS\) appliance. The tools register the appliance with the NVIDIA Licensing Portal \(NLP\), bind it to a license allocation, install the license file on the appliance, and can configure appliance settings such as NTP, network file sharing \(NFS\), and LDAP. You do not need to install Python on the computer where you run these tools.") for more information. 
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
Note:
The IP address of a VMware VM that hosts a DLS appliance can be set when the appliance is installed from an OVF template. If the IP address was set this way, you **must** follow the instructions in [Changing the IP Address of a VMware VM Set During DLS Appliance Installation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-vmware-vm-ip-address-set-at-dls-installation "The IP address of a VMware VM that hosts a DLS appliance can be set when the appliance is installed from an OVF template. If the IP address was set this way, you can change it only by editing vApps options within VM that hosts the DLS appliance. If you use any other method to change this address, it reverts to its original setting when the VM is restarted."). If you use any other method to change this address, it reverts to its original setting when the VM is restarted. 
  1. Use the hypervisor management console of the appliance to log in as the user dls_admin to the VM that hosts the DLS virtual appliance. If the dls_admin user has not been registered, you can still log in to the VM as the dls_admin user with the default password welcome.
  2. Run the /etc/adminscripts/set-static-ip-cli.sh script. 
Copy
Copied!

```
            
            
$ /etc/adminscripts/set-static-ip-cli.sh

        
```

  3. When prompted, enter the details of the IP address. The script presents any default values that are already set for the virtual appliance's network. 
    1. Enter the number that denotes the IP version that the virtual appliance's network uses. 
       * For an IPv4 network, type 4.
       * For an IPv6 network, type 6.
    2. Enter the IP address that you want to assign to the DLS virtual appliance.
    3. Enter the IP address of the DLS virtual appliance's default gateway. 
Note:
If you omit the default gateway address, the DLS virtual appliance uses DHCP settings. 
    4. Enter the IP address of the first DNS server to be used for name resolution.
    5. Enter the IP address of the second DNS server to be used for name resolution.
    6. Enter the subnet mask of the DLS virtual appliance's network in classless inter-domain routing (CIDR) format **without the leading slash character (/)**. To get a subnet mask in CIDR format from its decimal equivalent, refer to the table on page 2 of [IETF RFC 1878: Variable Length Subnet Table For IPv4](https://www.ietf.org/rfc/rfc1878.txt). For example, the subnet mask in CIDR format of the decimal equivalent 255.255.255.0 is 24.


After the IP address has been set, log files containing progress message from the script are available in the /tmp/static-ip-cli-logs directory. 
What to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance: 
  * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
  * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").


###  [2.5.3. Changing the IP Address of a VMware VM Set During DLS Appliance Installation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-vmware-vm-ip-address-set-at-dls-installation)
The IP address of a VMware VM that hosts a DLS appliance can be set when the appliance is installed from an OVF template. If the IP address was set this way, you can change it **only** by editing vApps options within VM that hosts the DLS appliance. If you use any other method to change this address, it reverts to its original setting when the VM is restarted.
  1. Log in to vCenter Server by using the VMware vSphere Web Client. For detailed instructions, refer to [Log in to vCenter Server by Using the vSphere Web Client](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vcenterhost.doc/GUID-CE128B59-E236-45FF-9976-D134DADC8178.html).
  2. From the vCenter Server inventory, navigate to the VM that hosts the DLS appliance.
  3. Shut down the VM to which you navigated in the previous step.
  4. On the **Configure** tab, expand **Settings** , select **vApp options** , and click **Edit**.
  5. In the list of vApp options, select **Networkproperties.IPaddress** and click **Set Value**.
  6. Enter the IP address that you want to assign to the VM that hosts the DLS appliance.
  7. Save your changes and start the VM.


###  [2.6. Deploying a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-containerized-dls-software-image)
For deployments on a supported container orchestration platform, the DLS is supplied as a containerized software image.
After verifying the requirements in [Platform Requirements for a DLS Virtual Appliance or Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-platform-requirements "Before proceeding, ensure that you have a platform suitable for hosting a DLS virtual appliance or containerized DLS software image."), deploy a containerized DLS software image by following this sequence of instructions: 
  1. [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.")
  2. [Deploying the Containerized DLS Software Image on a Supported Platform](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-supported-containerization-platform "The containerized DLS software image is distributed with default deployment files for several container orchestration platforms. You use standard interfaces of the container orchestration platform to deploy the containerized DLS software image on your chosen platform.")
  3. [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.")
  4. **Optional:** [Creating or Expanding an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-ha-cluster-dls-instances "Any time after creating or configuring a standalone DLS instance, you can create an HA Cluster of DLS instances by converting the instance into a node in a cluster and adding a second instance to the cluster. You can also expand an existing cluster by adding a DLS instance to it.")


###  [2.6.1. Contents of the Containerized DLS Software Image Download](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#contents-of-containerized-dls-software-image-download)
The containerized DLS software image is distributed as the ZIP archive nls-4.0.0-bios.zip.
This ZIP archive contains the following artifacts:  

dls_appliance_4.0.0.tar.gz
    The DLS application container. 

dls_pgsql_4.0.0.tar.gz
    The PostgreSQL database container.
Note:
The version of both containers must be the same. 
The DLS application container and the PostgreSQL database container share a common volume for exchanging information between them. If both containers are deployed on a multinode cluster, they must be run on the same worker node. 
###  [2.6.2. Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image)
To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.
Set the properties for a containerized DLS software image by editing the appropriate deployment file for your container orchestration platform.   
| Container Orchestration Platform  | File  |  
| --- | --- |  
| Docker  | docker-compose.yml  |  
| Kubernetes  | nls-si-0-deployment.yaml  |  
| Podman  | nls-si-0-deployment/compose.yml  |  
| Red Hat OpenShift Container Platform  | nls-si-0-deployment.yaml  |  
| VMware Tanzu Application Platform  | nls-si-0-deployment.yaml  |  
###  [2.6.2.1. Setting Environment Variables to Specify String Properties](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-string-properties)
You can set environment variables to specify string properties for all supported container orchestration platforms.
Set environment variables to specify string properties by editing the appropriate section of the deployment file for your container platform.   
| Deployment File  | Section to Edit  |  
| --- | --- |  
| docker-compose.yml  | `environment:`  |  
| nls-si-0-deployment/compose.yml  | `environment:`  |  
| nls-si-0-deployment.yaml  | `- env:`  |  
The environment variables for specifying string properties are as follows:  

Required: `DLS_DB_HOST` 
     The IP address of the database container. 
Ensure that the IP address of the database container is a private IP address on the same subnet as the DLS appliance container. The database container must **not** be accessible over any public subnet. 

Required: `DLS_PUBLIC_IP` 
    The IP address of the container orchestration platform. To access the NVIDIA Licensing application, the user connects to the container in which the DLS software image is deployed through this address. 

Optional: `DLS_PRIVATE_HOSTNAME` 
     The host name of the container in which the DLS software image is deployed. 
If this environment variable is not set, the container orchestration platform sets the host name to the container ID. 

Optional: `FQDN` 
     The fully qualified domain name that is mapped to the IP address that is specified by the `DLS_PUBLIC_IP` environment variable. 
Ensure that this name can be resolved by licensed clients. Do **not** specify a name that can be resolved only by the container orchestrator.
If this environment variable is not set, a user **cannot** connect to the container in which the DLS software image is deployed through a fully qualified domain name.
###  [2.6.2.2. Specifying Integer Values as Environment Variables](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#specifying-port-numbers-by-setting-environment-variables)
You can specify integer values such as port numbers for Docker, Podman, Red Hat OpenShift Container Platform, and VMware Tanzu Application Platform as environment variables.
Note:
Kubernetes does not accept strings for integer values. Therefore, you cannot specify integer values for Kubernetes as environment variables. Instead, you must specify integer values for Kubernetes as explained in [Specifying Integer Values as Actual Values](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#specifying-port-numbers-as-integer-values "Kubernetes does not accept strings for integer values. Therefore, you cannot specify integer values for Kubernetes as environment variables. Instead, you must specify integer values for Kubernetes as actual values."). 
To specify integer values by setting environment variables, edit the appropriate section of the deployment file for your container platform.   
| Deployment File  | Section to Edit  |  
| --- | --- |  
| docker-compose.yml  | `environment:`  |  
| nls-si-0-deployment/compose.yml  | `environment:`  |  
| nls-si-0-deployment.yaml  | `- env:`  |  
The following environment variables apply to Docker, Red Hat OpenShift Container Platform, and VMware Tanzu Application Platform:  

Optional: `CPU_CORE_LIMIT` 
     The maximum number of CPU cores that can be assigned to the DLS appliance container pod. 
Default: 4 

Optional: `DLS_HA_MAX_NODES_ENV` 
    The maximum number of instances in a cluster to which this instance is to be added.     
Allowed values: 2-9
Default: 2 

Optional: `DLS_HTTP_PORT` 
     The port on which the container in which the DLS software image is deployed listens for HTTP requests. 
Default: 80 

Optional: `DLS_HTTPS_PORT` 
     The port on which the container in which the DLS software image is deployed listens for HTTPS requests. 
Default: 443 

Optional: `DLS_RABBITMQ_SSL_PORT` 
     The Secure Sockets Layer (SSL) port through which the nodes in and HA cluster of DLS instances communicate with each other. 
Default: 5671
The following environment variables apply **only** to Red Hat OpenShift Container Platform and VMware Tanzu Application Platform:  

Optional: `DLS_EXPOSED_HTTP_PORT` 
     The port on which the container orchestrator listens for and forwards the request to the port that is specified by the `DLS_HTTP_PORT` environment variable. 
Not applicable to Docker or Podman.
Default: 30000 

Optional: `DLS_EXPOSED_HTTPS_PORT` 
     The port on which the container orchestrator listens for and forwards the request to the port that is specified by the `DLS_HTTP_PORTS` environment variable. 
Not applicable to Docker or Podman.
Default: 30001 

Optional: `DLS_VA_SERVICE_PORT` 
     The port in the container orchestrator to which the DLS appliance container binds. 
Not applicable to Docker or Podman.
Default: 8085
###  [2.6.2.3. Specifying Integer Values as Actual Values](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#specifying-port-numbers-as-integer-values)
Kubernetes does not accept strings for integer values. Therefore, you cannot specify integer values for Kubernetes as environment variables. Instead, you must specify integer values for Kubernetes as actual values.
To specify integer values as actual values, replace the references to environment variables with actual values in the `ports:` section of the nls-si-0-deployment.yaml and nls-si-0-service.yaml deployment files. 
  * In nls-si-0-deployment.yaml, the references to environment variables in the `ports:` section are as follows: 
Copy
Copied!

```
            
            
ports:
   - containerPort: ${DLS_HTTP_PORT:-80}
     hostPort: ${DLS_EXPOSED_HTTP_PORT:-80}
   - containerPort: ${DLS_HTTPS_PORT:-443}
     hostPort: ${DLS_EXPOSED_HTTPS_PORT:-443}
   - containerPort: ${DLS_RABBITMQ_SSL_PORT:-5671}
     hostPort: ${DLS_RABBITMQ_SSL_PORT:-5671}
   - containerPort: 8085
     hostPort: ${DLS_VA_SERVICE_PORT:-8085}

        
```

  * In nls-si-0-service.yaml, the references to environment variables in the `ports:` section are as follows: 
Copy
Copied!

```
            
            
ports:
    - name: "DLS_HTTP_CONTAINER"
      port: ${DLS_HTTP_PORT:-80}
      targetPort: ${DLS_HTTP_PORT:-80}
      nodePort: ${DLS_EXPOSED_HTTP_PORT:-30000}
    - name: "DLS_HTTPS_CONTAINER"
      port: ${DLS_HTTPS_PORT:-443}
      targetPort: ${DLS_HTTPS_PORT:-443}
      nodePort: ${DLS_EXPOSED_HTTPS_PORT:-30001}
    - name: "DLS_RABBITMQ_SSL_PORT"
      port: ${DLS_RABBITMQ_SSL_PORT:-5671}
      targetPort: ${DLS_RABBITMQ_SSL_PORT:-5671}
      nodePort: ${DLS_RABBITMQ_SSL_PORT:-5671}
    - name: "DLS_VA_SERVICE_PORT"
      port: 8085
      targetPort: 8085
      nodePort: ${DLS_VA_SERVICE_PORT:-8085}

        
```



The values with which to replace these references to environment variables are as follows:  

Both files: `${DLS_HTTP_PORT:-80}` 
    The port on which the container in which the DLS software image is deployed listens for HTTP requests. 

nls-si-0-deployment.yaml: `${DLS_EXPOSED_HTTP_PORT:-80}` 


nls-si-0-service.yaml: `${DLS_EXPOSED_HTTP_PORT:-30000}` 
     The port on which the container orchestrator listens for and forwards the request to the port that is specified by the `DLS_HTTP_PORT` environment variable.  

Both files: `${DLS_HTTPS_PORT:-443}` 
    The port on which the container in which the DLS software image is deployed listens for HTTPS requests. 

nls-si-0-deployment.yaml: `${DLS_EXPOSED_HTTPS_PORT:-443}` 


nls-si-0-service.yaml: `${DLS_EXPOSED_HTTPS_PORT:-30001}` 
     The port on which the container orchestrator listens for and forwards the request to the port that is specified by the `DLS_HTTP_PORTS` environment variable.  

Both files: `${DLS_RABBITMQ_SSL_PORT:-5671}` 
    The Secure Sockets Layer (SSL) port through which the nodes in and HA cluster of DLS instances communicate with each other. 

Both files: `${DLS_VA_SERVICE_PORT:-8085}` 
    The port in the container orchestrator to which the DLS appliance container binds.
This example shows the `ports:` sections in the nls-si-0-deployment.yaml and nls-si-0-service.yaml deployment files in which the references to environment variables are replaced with actual values. 
  * In nls-si-0-deployment.yaml, the `ports:` section is as follows: 
Copy
Copied!

```
            
            
ports:
   - containerPort: 80
     hostPort: 80
   - containerPort: 443
     hostPort: 443
   - containerPort: 5671
     hostPort: 5671
   - containerPort: 8085
     hostPort: 8085

        
```

  * In nls-si-0-service.yaml, the `ports:` section is as follows: 
Copy
Copied!

```
            
            
ports:
    - name: "DLS_HTTP_CONTAINER"
      port: 80
      targetPort: 80
      nodePort: 30000
    - name: "DLS_HTTPS_CONTAINER"
      port: 443
      targetPort: 443
      nodePort: 30001
    - name: "DLS_RABBITMQ_SSL_PORT"
      port: 5671
      targetPort: 5671
      nodePort: 30002
    - name: "DLS_VA_SERVICE_PORT"
      port: 8085
      targetPort: 8085
      nodePort: 30003

        
```



###  [2.6.3. Deploying the Containerized DLS Software Image on a Supported Platform](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-supported-containerization-platform)
The containerized DLS software image is distributed with default deployment files for several container orchestration platforms. You use standard interfaces of the container orchestration platform to deploy the containerized DLS software image on your chosen platform.
The default deployment file for each supported container orchestration platform specifies the resource reservations for the container that are listed in [Platform Requirements for a DLS Virtual Appliance or Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-platform-requirements "Before proceeding, ensure that you have a platform suitable for hosting a DLS virtual appliance or containerized DLS software image."). You are not required to specify the resource reservations for the container when you deploy the containerized DLS software image. 
Follow the instructions for the container orchestration platform that you are using.   
| Container Orchestration Platform  | Instructions  |  
| --- | --- |  
| Docker  | [Deploying the Containerized DLS Software Image on Docker](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-docker)  |  
| Kubernetes  | [Deploying Multi-Replica DLS Instance on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-multi-replica-dls-instance-on-kubernetes-platforms)  |  
| Podman  | [Deploying the Containerized DLS Software Image on Podman](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-podman)  |  
| Red Hat OpenShift Container Platform  | [Deploying Multi-Replica DLS Instance on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-multi-replica-dls-instance-on-kubernetes-platforms)  |  
| VMware Tanzu Application Platform  | [Deploying Multi-Replica DLS Instance on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-multi-replica-dls-instance-on-kubernetes-platforms)  |  
###  [2.6.3.1. Deploying the Containerized DLS Software Image on Docker](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-docker)
Ensure that the required environment variables have been set as explained in [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.").
  1. Import the DLS appliance artifact and the PostgreSQL database artifact. 
    1. Import the DLS appliance artifact. 
Copy
Copied!

```
            
            
docker load --input dls_appliance_4.0.0.tar.gz

        
```

    2. Import the PostgreSQL database artifact. 
Copy
Copied!

```
            
            
$ docker load --input dls_pgsql_4.0.0.tar.gz

        
```

  2. Change to the directory that contains the docker-compose.yml file.
  3. Start the DLS appliance and PostgreSQL database containers. 
Copy
Copied!

```
            
            
$ docker-compose up

        
```



What to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance: 
  * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
  * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").


###  [2.6.3.2. Deploying the Containerized DLS Software Image on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-kubernetes-platforms)
Perform this task if you are using Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. This procedure installs a single-replica DLS deployment (Active - Passive) using the legacy YAML files. To provide high availability with this method, deploy separate DLS instances and configure them as an HA cluster.
Ensure that the following prerequisites are met: 
  * The required environment variables have been set as explained in [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.").
  * A private repository for the DLS appliance artifact and the PostgreSQL database artifact has been created.
  * If a registry secret is required for pulling the artifacts from the private repository, the registry secret has been created. 
In the deployment files for the DLS appliance artifact and the PostgreSQL database artifact, the name of the registry secret is preset to `registry-secret`. If you create a secret with this name, you don't need to edit the deployment files to reference the secret.
For the Active-Active deployment model, see Deploying Multi-Replica DLS Instance on Kubernetes Platforms. In that model, the DLS operator manages multiple application replicas that share a PostgreSQL database cluster.


Note:
The DLS application container and the PostgreSQL database container share a common volume for exchanging information between them. If both containers are deployed on a multinode cluster, they must be run on the same worker node. 
  1. Import the DLS appliance artifact and the PostgreSQL database artifact into your private repository.
  2. Edit the deployment files for the DLS appliance artifact and the PostgreSQL database artifact to pull these artifacts from the private repository into which they were imported. In each file, replace the string `<POPULATE THIS WITH PRIVATE REPOSITORY>` with the name of the private repository. 
    1. For the DLS appliance artifact, edit the following line in the file nls-si-0-deployment.yaml: 
Copy
Copied!

```
            
            
image: <POPULATE THIS WITH PRIVATE REPOSITORY>:appliance

        
```

    2. For the PostgreSQL database artifact, edit the following line in the file postgres-nls-si-0-deployment.yaml: 
Copy
Copied!

```
            
            
image: <POPULATE THIS WITH PRIVATE REPOSITORY>:pgsql

        
```

  3. If a registry secret is required for pulling the artifacts from the private repository, edit the deployment files for the DLS appliance artifact and the PostgreSQL database artifact to reference the secret. In each file, replace the string `registry-secret` with the name of your registry secret. 
    1. For the DLS appliance artifact, edit the file nls-si-0-deployment.yaml.
    2. For the PostgreSQL database artifact, edit the file postgres-nls-si-0-deployment.yaml.
  4. Start the PostgreSQL database container pod with the supplied deployment file postgres-nls-si-0-deployment.yaml. 
Copy
Copied!

```
            
            
$ kubectl create -f _directory_/postgres-nls-si-0-deployment.yaml

        
```


_directory_ 
    The full path to the directory that contains the file postgres-nls-si-0-deployment.yaml.
  5. Get the IP address of the PostgreSQL database container pod. 
Copy
Copied!

```
            
            
$ kubectl get pods -o wide

        
```

  6. Set the `DLS_DB_HOST` environment variable in the file nls-si-0-deployment.yaml to the IP address that you got in the previous step. 
Note:
To enable the DLS appliance container pod to create and modify files on the volumes mapped to it, ensure that the required file access permissions are set on these volumes. 
  7. Start the DLS appliance container pod with the supplied deployment file nls-si-0-deployment.yaml. 
Copy
Copied!

```
            
            
$ kubectl create -f _directory_/nls-si-0-deployment.yaml

        
```


_directory_ 
    The full path to the directory that contains the file nls-si-0-deployment.yaml.


What to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance: 
  * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
  * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").


###  [2.6.3.2.1. Deploying Multi-Replica DLS Instance on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-multi-replica-dls-instance-on-kubernetes-platforms)
Ensure that the following requirements are met before you deploy a multi-replica DLS instance on Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. Multi-replica DLS uses an Active-Active deployment model. 
  * _Platform Requirements for a DLS Virtual Appliance or Containerized DLS Software Image_ in the NVIDIA License System User Guide.
  * _Host Name Resolution Requirements for a DLS Virtual Appliance_ in the NVIDIA License System User Guide
  * _Communications Ports Requirements_ in the NVIDIA License System User Guide
  * The NVIDIA License System Release Notes for your release (supported platforms and known issues)


###  [2.6.3.2.1.1. About Multi-Replica DLS Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-multi-replica-dls-deployment)
Perform this reading task before you deploy a multi-replica DLS instance on Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu. 
**What is multi-replica deployment?**
In a multi-replica deployment, NVIDIA License System runs **several copies** of the DLS application for **one** DLS service instance.   
| Term  | Plain-language meaning  |  
| --- | --- |  
| Replica  | One running copy of the DLS application.  |  
| Service instance  | One logical license server that your clients use.  |  
| Shared database  | One highly available PostgreSQL database that all copies use.  |  
| Client address  | The single IP address or host name that licensed clients connect to.  |  
Licensed clients (for example, vGPU clients) still connect to **one** address. They do not need to know how many copies of DLS are running behind that address. 
By default, a multi-replica deployment runs:
  * **Three** DLS application copies
  * **Three** database copies (for database high availability)


Your organization can change these numbers during installation if your platform team advises a different size. 
**Why use multi-replica?**
A multi-replica deployment is intended for production Kubernetes environments when you need:
  * Continued licensing service if one application copy fails
  * Continued database service if one database copy fails
  * Version upgrades that replace application copies **one at a time** , so licensing can stay available during the upgrade


**How this differs from other DLS options**  
| Deployment option  | Best for  | Notes  |  
| --- | --- | --- |  
| Multi-replica on Kubernetes (this documentation)  | Production on Kubernetes, OpenShift, or Tanzu  | Several application copies and a highly available database for one service instance.  |  
| VM-based DLS virtual appliance  | Traditional hypervisor deployments  | Same licensing features; different install and management path.  |  
| HA cluster of DLS instances  | Failover across separate DLS service instances.  | Different feature. See the note below.  |  
Note:
**Multi-replica is not an HA cluster of DLS instances**
An **HA cluster** (described in _Configuring an HA Cluster of DLS Instances_ in the NVIDIA License System User Guide) joins two or more **separate** DLS virtual machine appliances so that one can take over if another fails. 
A **multi-replica** deployment runs multiple application copies inside **one** Kubernetes cluster, with one shared database. 
Do not use Section 2.9 procedures to create multi-replica. Do not assume that multi-replica replaces an HA cluster of separate instances when your design requires separate instances on different Kubernetes clusters. 
**Roles and responsibilities**
Many organizations have one person perform all these roles. The split below helps when teams are separate.   
| Role  | Typical responsibilities  |  
| --- | --- |  
| Kubernetes / platform administrator  | Cluster access, storage classes, load balancers or routes, firewall rules, image registry, and command-line access (kubectl or oc). Optionally: node labels and taints for dedicated DLS pools.  |  
| DLS / NLS administrator  | Install and upgrade with Helm, set the client-facing address, confirm that the deployment is ready, register the administrator user, upload the license server file, and manage the DLS Web UI. Optionally: Helm scheduling values when the platform team requires dedicated nodes.  |  
| Network / DNS administrator  | DNS records for the client-facing address, and firewall rules for HTTPS and Windows license return.  |  
**What the software installs for you**
You install DLS by using **Helm charts** from the NLS software download artifact. After installation:
  * Automation software (an **operator**) creates and maintains the application copies, the database, and the required storage and network objects.
  * You do **not** need to start the database by hand before the application, as you do in the older single-pod Kubernetes YAML procedure.
  * You manage the deployment mainly through Helm settings and the DLS Web UI after the system reports that it is ready.


. 
###  [2.6.3.2.1.2. **Before You Begin**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-begin)
Ensure that the following requirements are met before you deploy a multi-replica DLS instance on Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu. 
Also review:
  * _Platform Requirements for a DLS Virtual Appliance or Containerized DLS Software Image_ in the NVIDIA License System User Guide
  * _Host Name Resolution Requirements for a DLS Virtual Appliance_ in the NVIDIA License System User Guide
  * _Communications Ports Requirements_ in the NVIDIA License System User Guide


**Sizing Guidelines**
Scale a DLS multi-replica deployment by adjusting the quantity of appliance and database pods rather than the vCPU and RAM assigned to a single instance. Avoid using sizing data from _Throughput for a DLS Appliance_ , _Scalability for a DLS Appliance_ , or _Burst Load Performance for a DLS Appliance_ for multi-replica topologies. 
Find pod-level resource limits and measured capacity for supported multi-replica configurations in these subsections:
  * [Default Resource Configuration for a DLS Multi-Replica Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#default-resource-configuration-for-dls-multi-replica-deployment)
  * [Scalability for a DLS Multi-Replica Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#scalability-for-dls-multi-replica-deployment)
  * [Burst Load Performance for a DLS Multi-Replica Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#burst-load-performance-for-dls-multi-replica-deployment)


located under [Sizing Guidelines for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-sizing-guidelines "Use the measured performance numbers to determine the optimum configuration for your container-based or VM-based DLS appliances based on the expected number and frequency of requests from licensed clients."). 
During a version upgrade, the system may run **one extra** application copy for a short time while it replaces copies one by one. Allow a small amount of spare CPU, memory, and database connection capacity for that window. 
**Storage requirements**
DLS needs **two kinds** of persistent storage. Your platform administrator must provide the correct **storage classes** (named storage templates in the cluster). 
**Important**
  * Do **not** use the same NFS / RWX class for database data. Database storage must use a block (RWO) class.
  * If your cluster has **no** RWX storage class, multi-replica deployment is not supported with the default settings. Add NFS or another shared file storage option before you continue.


Platform example values for Tanzu and OpenShift are described in kubernetes/helm-charts/README.md in the NLS download package. 
**Network, DNS, and client access**
Licensed clients reach DLS through a single published address that you configure during install.   
| Setting  | Guidance  |  
| --- | --- |  
| Client-facing IP (publicIP)  | The load balancer VIP or IP address that clients use. Prefer a numeric IPv4 or IPv6 address.  |  
| Optional DNS name (fqdn)  | A host name that resolves to the same address, when clients connect by name.  |  
| DNS  | Ensure forward (and reverse, when required) DNS entries are correct, as described in the User Guide host name resolution section.  |  
| Firewall  | Allow client traffic to the ports listed below.  |  
**Ports that licensed clients need**
These ports match the User Guide communications requirements for a licensed client and a DLS instance.   
| Port  | Used for  |  
| --- | --- |  
| 443  | License acquisition, renewal, and release (non-Windows); DLS Web UI; HTTPS management  |  
| 80  | License return when a Windows VM shuts down  |  
Note:
  * Port **80** is required if any licensed clients are Windows VMs that return licenses at shutdown.
  * Ports **808****1** and **8082** are supported only for backward compatibility. New clients should use port **443**.
  * Do not expose the database or internal messaging ports on your public load balancer. Those ports are for cluster-internal use only.


**Session stickiness**
Your load balancer should keep a given client on the same healthy application copy when possible (sticky sessions / session persistence) or preserve the client source IP so that Kubernetes can do the same. 
Licensing usually still works without stickiness, but sticky sessions reduce edge cases when several application copies are active. 
**OpenShift note**
On OpenShift, you typically publish HTTPS through a Route to the DLS service on port **443** , and you still need a path for HTTP lease return on port **80** (or the port you configure for lease shutdown). Set the client-facing address to the host name that clients will use. 
**Container images**
Load these images from the NLS ARTIFACT package into a registry that your cluster can pull from:   
| Image  | Role  |  
| --- | --- |  
| dls:operator_<version>  | Automation components that install and maintain DLS  |  
| dls:appliance_<version>  | DLS application  |  
| dls:pgsql_<version>  | PostgreSQL database  |  
The Helm charts in the package are already matched to these image versions. You normally only need to point the charts at your registry path. 
If the registry is private, create the image pull secret as described in the Helm README for your release. 
**Information to collect before install**
Ask your platform team for: 
  1. Registry path for DLS images (for example `harbor.example.com/dls`)
  2. RWX storage class name for configurations
  3. RWO storage class name for the database
  4. Client-facing VIP or IP address
  5. Optional DNS host name for that address
  6. Whether the platform is vanilla Kubernetes, OpenShift, or Tanzu (so you can use the matching values file)


**What to do next**
  * Refer to the [Sizing Guidelines for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-sizing-guidelines "Use the measured performance numbers to determine the optimum configuration for your container-based or VM-based DLS appliances based on the expected number and frequency of requests from licensed clients.") for the application and db pods and deployment topology
  * Deploy the multi-replica instance as explained in [Deploying a Multi-Replica DLS Instance on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-multi-replica-dls-instance-on-kubernetes-platforms).
  * If your platform team requires dedicated nodes or tainted node pools, also review [Controlling Where DLS Pods Run](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#controlling-where-dls-pods-run) before you finalize Helm values.


###  [2.6.3.2.1.3. Controlling Where DLS Pods Run](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#controlling-where-dls-pods-run)
Perform this task if your platform team wants DLS application copies, database copies, or setup jobs to run only on **selected worker nodes** (for example a dedicated node pool), including nodes that use **taints**. 
If you do not set these options, DLS keeps the default placement behavior shipped with the release. Existing deployments that omit these settings continue to work as before. 
Exact Helm value names for your release are in `kubernetes/helm-charts/README.md` and in the `dls-appliance-instance / dls-platform` values files in the NLS download package. 
**What you can control**
  * | Workload  | **Helm / configuration block**  | What it affects  |  
| --- | --- | --- |  
| DLS application copies  | applianceScheduling  | Application pods, and appliance-related setup jobs  |  
| Database copies  | postgresScheduling  | PostgreSQL pods, and database-related setup or upgrade jobs  |  
| Connection poolers (optional)  | poolerScheduling  | PgBouncer pooler pods, when poolers are enabled  |  


For each block you can set, when needed:   
| **Setting**  | ****Plain-language purpose****  |  
| --- | --- |  
|  **Node labels (** nodeSelector**)**  | Run pods only on nodes that have matching labels.  |  
| **Tolerations**  | Allow pods to run on nodes that have matching **taints**.  |  
|  **Anti-affinity mode (** podAntiAffinity**)**  | Prefer or require that copies run on different nodes (Preferred, Required, or Disabled).  |  
| **Full affinity / topology rules**  | Advanced placement rules when your platform team supplies them.  |  
|  **Disruption budget (** pdb**)**  | How many copies must stay available during voluntary maintenance (optional override).  |  
Note:
  * **Taints** are configured on **nodes** by the platform administrator. The DLS configuration does **not** create node taints.
  * A **node label alone is not enough** on a tainted node. You must also set a matching **toleration** on the DLS workload.
  * Setup and upgrade **jobs** inherit placement from the matching block: appliance jobs use`applianceScheduling`; database jobs use `postgresScheduling`. You do not configure job placement separately.


**Default behavior (no scheduling settings)**
When `applianceScheduling`, postgresScheduling, and `poolerScheduling` are empty or omitted:
  * Application copies use the built-in **preferred** spread across nodes and zones (soft rules; the scheduler may still place more than one copy on the same node if the cluster is small).
  * Database copies use the database automation defaults (preferred spread across nodes).
  * An application disruption budget is created only when application autoscaling (`enableHPA`) is enabled — the same behavior as earlier multi-replica releases.


This is the safe path for most installs and for upgrades of clusters that already run multi-replica DLS. 
**Before you begin (platform preparation)**
Ask your platform administrator to: 
  1. Identify or create the worker nodes that should run DLS application and/or database workloads.
  2. **Label** those nodes (example labels below).
  3. Optionally **taint** those nodes so other workloads do not schedule there.
  4. Confirm that enough nodes exist if you plan to use **Required** anti-affinity (you need at least as many eligible nodes as copies).


Example labels and taints (adjust names to your site standard): 
Copy
Copied!

```
            
            
# Application nodes
kubectl label node <app-node> dls.nvidia.com/workload=appliance
kubectl taint nodes <app-node> dls.nvidia.com/dedicated=appliance:NoSchedule

# Database nodes
kubectl label node <db-node> dls.nvidia.com/workload=database
kubectl taint nodes <db-node> dls.nvidia.com/dedicated=database:NoSchedule

        
```

On OpenShift, use instead of `kubectl` when that is your site practice. 
**Configuring placement in Helm values**
Add the scheduling blocks under your instance settings (for the platform chart, under instance.spec). 
****Example: dedicated application and database node pools****
Copy
Copied!

```
            
            
# Fragment for my-dls-values.yaml (dls-platform: under instance.spec)
# or for the dls-appliance-instance chart: under spec

applianceScheduling:
  nodeSelector:
    dls.nvidia.com/workload: appliance
  tolerations:
    - key: dls.nvidia.com/dedicated
      operator: Equal
      value: appliance
      effect: NoSchedule
  podAntiAffinity: Required

postgresScheduling:
  nodeSelector:
    dls.nvidia.com/workload: database
  tolerations:
    - key: dls.nvidia.com/dedicated
      operator: Equal
      value: database
      effect: NoSchedule
  podAntiAffinity: Required

# Optional — only if you enabled poolers (poolerInstances > 0)
poolerScheduling:
  nodeSelector:
    dls.nvidia.com/workload: database
  tolerations:
    - key: dls.nvidia.com/dedicated
      operator: Equal
      value: database
      effect: NoSchedule

        
```

**Optional: application disruption budget without autoscaling**
By default, the application disruption budget follows autoscaling (enableHPA). To keep a disruption budget when autoscaling is off: 
Copy
Copied!

```
            
            
enableHPA: false
applianceScheduling:
  pdb:
    enabled: true
    minAvailable: "2"   # keep at least two application copies during voluntary drains

        
```

**Optional: pooler disruption budget**
Pooler disruption budgets are **off** unless you enable them: 
Copy
Copied!

```
            
            
poolerInstances: 3
poolerScheduling:
  pdb:
    enabled: true
    minAvailable: "1"

        
```

**Applying the settings**
**New install**
Include the scheduling blocks in the values file you pass to helm install, as described in **Deploying a Multi-Replica DLS Instance on Kubernetes Platforms**. 
**Existing deployment**
  1. Add or update the scheduling blocks in your values file.
  2. Run `helm upgrade` with the same chart and values you use for normal upgrades (see **Upgrading a Multi-Replica DLS Deployment** and the Helm README).
  3. Wait until the DLS instance returns to **Ready**.
  4. Confirm that pods land on the intended nodes: 
Copy
Copied!

```
            
            
kubectl get pods -n dls-system -o wide

        
```



Note:
  * Changing placement may cause pods to restart on different nodes. Plan this change in a maintenance window if licensed clients are sensitive to brief reconnects.
  * If you **remove** tolerations or selectors later, automation clears them on long-running workloads so pods stop preferring the old dedicated nodes.


**Choosing Preferred vs Required anti-affinity**  
| Mode  | **Behavior**  | **When to use**  |  
| --- | --- | --- |  
|  **Preferred** (or leave unset with no other affinity override)  | Scheduler **tries** to put copies on different nodes but may pack them if capacity is tight.  | Default / smaller clusters.  |  
| **Required**  | Scheduler **must** put copies on different nodes; pods stay **Pending** if there are not enough matching nodes.  | Strict HA on dedicated pools with enough nodes.  |  
| **Disabled**  | No hostname anti-affinity from this convenience setting.  | Only when your platform team provides a full custom affinity policy.  |  
Important:
If you set `podAntiAffinity: Required` and you have fewer eligible nodes than application or database copies, pods remain **Pending**. Either add nodes (or relax labels/taints) or change the mode back to `Preferred`. 
**Relationship to multi-node storage spread**
`multiNodeSpread` controls **storage** for RabbitMQ (shared PVC versus local emptyDir). It does **not** pin pods to specific nodes. 
To pin pods to nodes, use the scheduling blocks in this topic. You may use both: for example `multiNodeSpread: true` together with `applianceScheduling` on a dedicated application pool. 
**What to do next**
  * After install or upgrade, verify pods with [Verifying the Multi-Replica Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-the-multi-replica-deployment).
  * If pods stay **Pending** after you set scheduling, see [Troubleshooting Multi-Replica DLS Deployments](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/topics/troubleshooting-multi-replica-dls-deployments.html).


###  [2.6.3.2.1.4. Installing the multi-replica deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-the-multi-replica-deployment)
Perform this task to install a multi-replica DLS instance on Kubernetes, Red Hat OpenShift, or VMware Tanzu. 
Before installing, import the release images into a repository that the cluster can access. Obtain a client-facing virtual IP or LoadBalancer IP address, a storage class that supports ReadWriteMany (RWX) for the shared configuration, log, and RabbitMQ volumes, and a block-storage class that supports ReadWriteOnce (RWO) for the PostgreSQL volumes. 
Replace each placeholder below with the value for your environment. <DLS-image-repository> is the complete image repository path, including the DLS image name, for example registry.example.com/licensing/dls, without an image tag. <client-facing-IP-address> is the numeric IP address that clients use to reach DLS. <RWX-storage-class> and <RWO-database-storage-class> are storage class names available in your cluster.
Run only the example for your platform. Start in the root directory of the extracted deployment package, which contains the kubernetes directory. 
For Kubernetes without the OpenShift or Tanzu platform overrides, run: 
Copy
Copied!

```
            
            
cd kubernetes/helm-charts

helm install dls ./dls-platform \
  --namespace default \
  --set-string global.image.repository='<DLS-image-repository>' \
  --set-string instance.spec.publicIP='<client-facing-IP-address>' \
  --set-string instance.spec.storageClass='<RWX-storage-class>' \
  --set-string instance.spec.dbStorageClass='<RWO-database-storage-class>'

        
```

For Red Hat OpenShift, run: 
Copy
Copied!

```
            
            
cd kubernetes/helm-charts

helm install dls ./dls-platform \
  --namespace default \
  -f dls-platform/values-openshift.yaml \
  --set-string global.image.repository='<DLS-image-repository>' \
  --set-string instance.spec.publicIP='<client-facing-IP-address>' \
  --set-string instance.spec.storageClass='<RWX-storage-class>' \
  --set-string instance.spec.dbStorageClass='<RWO-database-storage-class>'

        
```

For VMware Tanzu, run: 
Copy
Copied!

```
            
            
cd kubernetes/helm-charts

helm install dls ./dls-platform \
  --namespace default \
  -f dls-platform/values-tanzu.yaml \
  --set-string global.image.repository='<DLS-image-repository>' \
  --set-string instance.spec.publicIP='<client-facing-IP-address>' \
  --set-string instance.spec.storageClass='<RWX-storage-class>' \
  --set-string instance.spec.dbStorageClass='<RWO-database-storage-class>'

        
```

The OpenShift and Tanzu values files keep multiNodeSpread set to false and use shared RWX volumes. The OpenShift values file also enables the OpenShift-specific operator and database settings. Specify the RWO database storage class explicitly; do not use the shared RWX class for the database volumes. 
To configure a DNS name at installation time, append --set-string instance.spec.fqdn='<DLS-fully-qualified-domain-name>' to the same helm install command for your platform before running it.
Configure DNS so that the fully qualified domain name resolves to the client-facing IP address. 
Watch the DLS instance status. On Kubernetes or VMware Tanzu, run: 
`kubectl get dlsappliance dls-main -n default -w`
On Red Hat OpenShift, run: 
`oc get dlsappliance dls-main -n default -w`
Wait until PHASE shows Ready, then press Ctrl+C to stop watching. If PHASE shows Failed, see Troubleshooting Multi-Replica DLS Deployments. 
Confirm that the application, database, and operator pods are ready. On Kubernetes or VMware Tanzu, run: 
`kubectl get pods -n dls-system`
`kubectl get pods -n cnpg-system`
`kubectl get pods -n dls-appliance-operator`
On Red Hat OpenShift, run: 
`oc get pods -n dls-system`
`oc get pods -n cnpg-system`
`oc get pods -n dls-appliance-operator`
Application, database, and operator pods should show Running with all containers ready. One-time setup jobs may show Completed. Ready indicates that the platform deployment is ready; administrator registration and license allocation installation are still required
###  [2.6.3.2.1.5. Verifying the Multi-Replica Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-the-multi-replica-deployment)
Perform these checks after a new multi-replica install or after an upgrade return to **Ready**. 
**Health checklist**
  * Expand  
| Check  | What to do  | Expected result  |  
| --- | --- | --- |  
| Instance phase  | kubectl get dlsappliance dls-main -n default (or oc on OpenShift)  | PHASE is Ready (use your CR namespace if not default)  |  
| Application copies  | kubectl get pods -n dls-system  | DLS application pods are Running (use your workload namespace if not dls-system)  |  
| Database  | Inspect database pods / cluster status in dls-system  | Database pods are healthy / Ready  |  
| Controllers (optional)  | kubectl get pods -n cnpg-system and -n dls-appliance-operator  | CNPG and appliance operator pods are Running (use custom namespaces if you changed them)  |  
| Storage  | kubectl get pvc -n dls-system  | Claims are Bound  |  
| Node placement (optional)  | kubectl get pods -n dls-system -o wide  | If you configured dedicated nodes, application and database pods run on the expected nodes  |  
| Web UI  | Open https://<publicIP>/  | Login or registration page loads  |  
| Test license  | Lease a license from a test client  | Success  |  


**What Ready does not guarantee**
  1. Register the DLS administrator user (new installs).
  2. Install the license server file from the NVIDIA Licensing Portal.
  3. Configure users, roles, and any LDAP or SSO settings your organization requires.
  4. Confirm that licensed clients can reach the published address on ports **443** and, for Windows shutdown return, **80**.


**Quick connectivity checks**
From a machine on the client network: 
  1. Confirm HTTPS reaches DLS: 
Copy
Copied!

```
            
            
curl -k https://<publicIP>/

        
```

  2. Confirm DNS, if you use a host name: 
Copy
Copied!

```
            
            
nslookup <fqdn>

        
```

  3. Confirm that the host name resolves to the same address you set as publicIP.


If these checks fail, see **Troubleshooting Multi-Replica DLS Deployments**. 
**What to do next**
  * Day-to-day administration follows the same User Guide topics as other DLS deployments (users, licenses, alerts, and so on).
  * For version upgrades, see [**_Upgrading a Multi-Replica DLS Deployment_** _._](https://docs.google.com/document/d/1xFv3KSS2FZ43HbeJ4CQFHlEfdknvW3GF7jtLrQNwU04/edit?tab=t.0#heading=h.lkhydpr2zsud)
  * For optional dedicated-node placement, see [**_Controlling Where DLS Pods Run_**](https://docs.google.com/document/d/1xFv3KSS2FZ43HbeJ4CQFHlEfdknvW3GF7jtLrQNwU04/edit?tab=t.0#heading=h.e6gw9zdaahkg).


###  [2.6.3.2.1.6. Accessing the DLS Web UI](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#accessing-the-dls-web-ui)
  1. From a browser that can reach the client-facing address, open: `https://<publicIP>/` If you configured a DNS name, you may also use: `https://<fqdn>/`
  2. Register the DLS administrator user as explained in _Registering the DLS Administrator User_ in the NVIDIA License System User Guide.
  3. Install the license server file from the NVIDIA Licensing Portal and complete the remaining configuration steps as you would for any other DLS instance.


Note:
**Ready** means the platform install is finished. It does **not** mean licenses are already installed. You must still upload the license server file and configure users as described in the User Guide. 
**What to do next**
  * Verify health using [**Verifying the Multi-Replica Deployment**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-the-multi-replica-deployment)
  * To pin workloads to dedicated or tainted nodes (optional), see [**Controlling Where DLS Pods Run**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#controlling-where-dls-pods-run)
  * For later version upgrades, see [**Upgrading a Multi-Replica DLS Deployment**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#upgrading-multi-replica-dls-deployment)
  * If the phase stays Failed or the Web UI does not load, see [**Troubleshooting Multi-Replica DLS Deployments**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/topics/troubleshooting-multi-replica-dls-deployments.html)


###  [2.6.3.2.1.7. Upgrading a Multi-Replica DLS Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#upgrading-multi-replica-dls-deployment)
Perform this task to move an existing multi-replica DLS deployment to a newer NLS release on Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu. 
Important:
  * **Downgrades are not supported.** Do not install charts or set a version older than the version already running.
  * Upgrade the **automation controllers last**. Do not upgrade every component in a single step. Follow the two-phase procedure below (also described in kubernetes/helm-charts/README.md in the NLS download package).


**Before you upgrade**
  1. Read the NVIDIA License System Release Notes for the target version.
  2. Load the **new** container images into your registry: 
     * `dls:operator_<newVersion>`
     * `dls:appliance_<newVersion>`
     * `dls:pgsql_<newVersion>`\
  3. Use the Helm charts from the **new** NLS ARTIFACT.
  4. Confirm that you still have your previous values (registry, publicIP, storage classes, replica count, and any optional scheduling / node-placement settings).
  5. Choose an upgrade strategy:   
| Strategy  | When to use  |  
| --- | --- |  
| Rolling (recommended)  | Routine upgrades that stay on the same PostgreSQL major version  |  
| File-based upgrade  | PostgreSQL major version change, or when Rolling is not suitable  |  


**Rolling upgrade requirements**
  * At least **two** DLS application copies (replicas ≥ 2), **or** autoscaling with a minimum of two copies.
  * Same PostgreSQL major version as the target images.


If these requirements are not met, the upgrade may fail. Fix the configuration before you continue. 
**What happens during a Rolling upgrade**
Understanding the order helps when you monitor progress:
  1. **Credentials** — New database credentials are prepared so both the old and new application versions can connect during the transition.
  2. **Database first** — Database copies move to the new database image, one at a time.
  3. **Application second** — DLS application copies upgrade **one at a time**. Licensing can stay available while other copies continue to serve clients.
  4. **Cleanup** — When every application copy is on the new version, the system finishes the upgrade and removes temporary credential material.


During the roll, you may briefly have one extra application copy. This is expected. 
**Two-phase upgrade procedure**
Exact flags for your release are in the Helm README. The flow is always: 
**Phase 1 — Upgrade the application and database (controllers stay on the old image)**
  1. From the **new** kubernetes/helm-charts directory, run a Helm upgrade that: 
     * Applies the new charts / appliance version
     * Keeps the operator and database-controller images on the **currently running** (old) tag
  2. Wait until the instance phase returns to **Ready** on the new appliance version: 
Copy
Copied!

```
            
            
kubectl get dlsappliance dls-main -n default -w

        
```



**Phase 2 — Upgrade the automation controllers last**
  1. After Phase 1 is **Ready** , run a second Helm upgrade that moves the operator and database-controller images to the **new** tag.
  2. Confirm that pods in the operator and database-controller namespaces are running on the new image.
  3. Re-run the [**Verifying the Multi-Replica Deployment**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-the-multi-replica-deployment).


Note:
A single helm upgrade of the full platform chart without pinning the old controller tags can update the controllers too early. Always follow the two-phase procedure in the Helm README for your release. 
**Monitoring an upgrade**
Copy
Copied!

```
            
            
kubectl get dlsappliance dls-main -n default -w
kubectl describe dlsappliance dls-main -n default

        
```

You may see phases such as **UpgradingRolling** while work is in progress. When finished, the phase returns to **Ready**. 
If the phase becomes **Failed** , or stays in an upgrading state for an unusually long time, see **Troubleshooting Multi-Replica DLS Deployments**. 
**Upgrade checklist**
  1. Read release notes for the target version.
  2. Load new images into the registry.
  3. Use Helm charts from the new version artifact.
  4. Run **Phase 1** (application and database; controllers pinned to the old image).
  5. Wait for **PHASE = Ready** on the new version.
  6. Run **Phase 2** (upgrade controller images).
  7. Verify Web UI access and a test license lease.


**What to do next**
Continue normal DLS administration using the NVIDIA License System User Guide. No separate multi-replica steps are required for day-to-day license and user management after a successful upgrade. 
###  [2.6.4. Deploying the Containerized DLS Software Image on Podman](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-podman)
Ensure that the required environment variables have been set as explained in [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.").
  1. Import the DLS appliance artifact and the PostgreSQL database artifact. 
    1. Import the DLS appliance artifact. 
Copy
Copied!

```
            
            
$ podman load --input dls_appliance_4.0.0.tar.gz

        
```

    2. Import the PostgreSQL database artifact. 
Copy
Copied!

```
            
            
$ podman load --input dls_pgsql_4.0.0.tar.gz

        
```

  2. Change to the podman directory.
  3. **Optional:** Edit the postgres-nls-si-0-deployment/compose.yml file to customize the subnets with which you want to create the `postgres-nls-si-0-deployment_back-tier` network. By default, the `postgres-nls-si-0-deployment_back-tier` network is created with the following subnets: 
     * **IPv4:**`172.16.238.0/28`
     * **IPv6:**`2001:3984:3989::/64`
  4. Create the `postgres-nls-si-0-deployment_back-tier` network, configurations volume mapping, and postgres-data volume mapping, and deploy the PostgreSQL database container. 
Copy
Copied!

```
            
            
$ podman-compose -f postgres-nls-si-0-deployment/compose.yml up -d

        
```

For information about the configurations and postgres-datavolume mappings, refer to [Volume Mappings for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#volume-mappings-for-containerized-dls-software-image "When a containerized DLS software image is deployed, several volume mappings are created for maintaining the state of the NVIDIA Licensing application in the DLS appliance.").
  5. Confirm that the `postgres-nls-si-0-deployment_back-tier` network, configurations volume mapping, and postgres-data volume mapping were created and that the PostgreSQL database container was deployed. 
    1. Confirm that the `postgres-nls-si-0-deployment_back-tier` network has been created. 
Copy
Copied!

```
            
            
# podman network ls
NETWORK ID    NAME                                    DRIVER
2f259bab93aa  podman                                  bridge
272c45824a80  postgres-nls-si-0-deployment_back-tier  bridge

        
```

    2. Confirm that the properties of the `postgres-nls-si-0-deployment_back-tier` network, for example, its subnets, are correct. 
Copy
Copied!

```
            
            
# podman network inspect postgres-nls-si-0-deployment_back-tier
[
    {
        "name": "postgres-nls-si-e-deployment_back-tier”,
        "id": "272cu5824a80fd8117261f28e820f920999fudes6a03ua78lesecf1015a76201",
        "driver": "bridge",
        "network_interface": "cni-podmanl",
        "created": "2023-03-08T07:42:03.88777482",
        "subnets": [
            {
                "subnet": "172.16.238.0/28",
                "gateway": "172.16.238.1"
            },
            {
                "subnet": "2001:3984:3989::/64",
                "gateway": "2001:3984:3989::1"
            }
        ],
        "ipv6_enabled": true,
        "internal": false,
        "dns_enabled": true,
        "labels": {
            "com.docker.compose.project": "postgres-nls-si-0-deployment",
            "io.podman.compose.project": "postgres-nls-si-0-deployment"
        },
        "ipam_options": {
            "driver": "host-local"
        }
    }
]

        
```

    3. Confirm that the configurations volume mapping and postgres-data volume mapping were created. 
Copy
Copied!

```
            
            
# podman volume ls
DRIVER       VOLUME NAME
local        configurations
local        postgres-data

        
```

    4. Confirm that the PostgreSQL database container was deployed. 
Copy
Copied!

```
            
            
# podman container ls

        
```

  6. Get the IP address of the PostgreSQL database container pod. 
Copy
Copied!

```
            
            
$ podman inspect postgres-nls-si-0-deployment_postgres-nls-si-0_1

        
```

  7. Set the `DLS_DB_HOST` environment variable in the file nls-si-0-deployment/compose.yml to the IP address that you got in the previous step.
  8. Create the logs volume mapping and rabbitmq_data volume mapping, and deploy the DLS appliance container. 
Copy
Copied!

```
            
            
$ podman-compose  -f nls-si-0-deployment/compose.yml up -d

        
```

For information about the logs and rabbitmq_data volume mappings, refer to [Volume Mappings for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#volume-mappings-for-containerized-dls-software-image "When a containerized DLS software image is deployed, several volume mappings are created for maintaining the state of the NVIDIA Licensing application in the DLS appliance.").
  9. Confirm that the logs volume mapping and rabbitmq_data volume mapping were created and that the DLS appliance container was deployed. 
    1. Confirm that the logs volume mapping and rabbitmq_data volume mapping were created. 
Copy
Copied!

```
            
            
# podman volume ls
DRIVER       VOLUME NAME
...
local        logs
...
local        rabbitmq_data

        
```

    2. Confirm that the DLS appliance container was deployed. 
Copy
Copied!

```
            
            
# podman container ls

        
```



What to do next depends on whether you are performing a new installation or are upgrading an existing DLS instance: 
  * If you are performing a new installation, register the DLS administrator user on the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
  * If you are upgrading an existing DLS instance, migrate the existing DLS instance as explained in [Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance "Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are not migrated.").


###  [2.6.5. Volume Mappings for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#volume-mappings-for-containerized-dls-software-image)
When a containerized DLS software image is deployed, several volume mappings are created for maintaining the state of the NVIDIA Licensing application in the DLS appliance.
The mapping of the volumes to the container filesystem can be found in the respective **deployment files** depending on the Orchestrator platform that the containerized appliance is being deployed on.   
| Platform  | File  |  
| --- | --- |  
| Docker  | docker-compose.yml  |  
| Kubernetes  | nls-si-0-deployment.yaml  |  
| Podman  | compose.yml  |  
| Openshift  | nls-si-0-deployment.yaml  |  
| Tanzu  | nls-si-0-deployment.yaml  |  
| Platform  | File  |  
| --- | --- |  
| Docker  | docker-compose.yml  |  
| Kubernetes  | nls-si-0-deployment.yaml  |  
| Podman  | compose.yml  |  
| Openshift  | nls-si-0-deployment.yaml  |  
| Tanzu  | nls-si-0-deployment.yaml  |  
The volume mappings that are created are as follows:  

configurations
    Contains the state of the DLS appliance container to enable its dynamic properties to be retrieved if the container fails. 

logs
    Contains log files created by the NVIDIA Licensing application in the DLS appliance. 

postgres-data
    Contains NVIDIA Licensing application data created by the PostgreSQL database. Because this volume contains information about available and checked out licenses, ensure that access to this volume is limited. 

rabbitmq_data
    Contains information about the real-time replication of data between nodes in an HA cluster of DLS instances.
Note:
These volume mappings maintain the state of the NVIDIA Licensing application in the DLS appliance. Therefore, do **not** modify these volume mappings. 
###  [2.6.6. Troubleshooting Issues with Deploying the Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-deployments-of-containerized-dls-software-image)
###  [2.6.6.1. Containerized DLS Appliance Fails to Start on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#containerized-dls-appliance-fails-to-start-on-kubernetes-platforms)
When this issue occurs, the service enters a crash loop, container creating, pending sequence before the container enters an error state.
This issue might affect Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. It does **not** affect Docker. 
  1. Ensure that the file access permissions on the volumes allow write access by the database container and the DLS appliance container, for example `707 (rwx---rwx)`.
  2. If the containerized DLS software image is deployed on a multinode cluster, ensure that the database container and the DLS appliance container are running on the same worker node.
  3. If the Kubernetes cluster does not dynamically provision persistent volumes when creating persistent volume claims, ensure that the persistent volumes are created manually with file access permissions that allow the database container and the DLS appliance container to write to them.
  4. Ensure that the versions of the database container and the DLS appliance container are identical.


###  [2.6.6.2. Volume Mode Access for DLS Appliance Resources on Kubernetes Platforms Is Incorrect](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#volume-mode-access-on-kubernetes-platforms-is-incorrect)
The DLS application container and the PostgreSQL database container share a common volume for exchanging information between them. If the volume mode access is incorrect, start-up of only the first container pod to be started is successful. The remaining container pod fails to start because the volume is already mounted on the first container pod to be started.
This issue might affect Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. It does **not** affect Docker. 
  1. If both containers are deployed on a multinode cluster, ensure that the DLS application container and the PostgreSQL database container run on the same worker node.
  2. Ensure that the PostgreSQL database container pod is started **before** the DLS appliance container pod. For more information, refer to [Deploying the Containerized DLS Software Image on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-kubernetes-platforms "Perform this task if you are using Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. This procedure installs a single-replica DLS deployment \(Active - Passive\) using the legacy YAML files. To provide high availability with this method, deploy separate DLS instances and configure them as an HA cluster.").


###  [2.6.6.3. Data Validation Errors Prevent the DLS Appliance Container Pod from Starting on Kubernetes](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#data-errors-prevent-dls-appliance-start-on-kubernetes)
This issue occurs if port numbers are specified as references to environment variables in the deployment files. Because Kubernetes does not accept string values for port numbers, you cannot specify port numbers for Kubernetes by setting environment variables.
This issue might affect Kubernetes. It does **not** affect other supported container orchestration platforms. 
When this issue occurs, the following error messages are displayed: 
Copy
Copied!

```
            
            
error: error validating ".": error validating data:[
ValidationError(Deployment.spec.template.spec.containers[0].ports[0].containerPort): 
invalid type for io.k8s.api.core.v1.ContainerPort.containerPort: got "string", expected "integer", 
ValidationError(Deployment.spec.template.spec.containers[0].ports[0].hostPort):
invalid type for io.k8s.api.core.v1.ContainerPort.hostPort: got "string", expected "integer",
ValidationError(Deployment.spec.template.spec.containers[0].ports[1].containerPort): 
invalid type for io.k8s.api.core.v1.ContainerPort.containerPort: got "string", expected "integer",
ValidationError(Deployment.spec.template.spec.containers[0].ports[1].hostPort): 
invalid type for io.k8s.api.core.v1.ContainerPort.hostPort: got "string", expected "integer",
ValidationError(Deployment.spec.template.spec.containers[0].ports[2].containerPort): 
invalid type for io.k8s.api.core.v1.ContainerPort.containerPort: got "string", expected "integer", 
ValidationError(Deployment.spec.template.spec.containers[0].ports[2].hostPort): 
invalid type for io.k8s.api.core.v1.ContainerPort.hostPort: got "string", expected "integer", 
ValidationError(Deployment.spec.template.spec.containers[0].ports[3].hostPort): 
invalid type for io.k8s.api.core.v1.ContainerPort.hostPort: got "string", expected "integer"];

        
```

  1. Edit the deployment files to specify port numbers as integer values instead of references to environment variables. For instructions, refer to [Specifying Integer Values as Actual Values](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#specifying-port-numbers-as-integer-values "Kubernetes does not accept strings for integer values. Therefore, you cannot specify integer values for Kubernetes as environment variables. Instead, you must specify integer values for Kubernetes as actual values.").
  2. Start the DLS appliance container pod with the supplied deployment file nls-si-0-deployment.yaml. 
Copy
Copied!

```
            
            
$ kubectl create -f _directory_/nls-si-0-deployment.yaml

        
```


_directory_ 
    The full path to the directory that contains the file nls-si-0-deployment.yaml.


###  [2.6.6.4. The Management Interface of the NVIDIA Licensing Application Fails to Load](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#management-interface-of-nvdia-licensing-app-fails-to-load)
When this issue occurs, the containerized DLS appliance starts correctly but the management interface of the NVIDIA Licensing application is not loaded into the browser.
  1. Ensure that the `DLS_DB_HOST` environment variable set to the IP address of the database container.
  2. Ensure that the environment variables for port mappings set in the deployment files are correct and that the ports are open on the Kubernetes node.
  3. Ensure that the port specified by the `DLS_EXPOSED_HTTPS_PORT` environment variable is open in the firewall on the node that is hosting the DLS appliance container.
  4. If the Kubernetes cluster does not dynamically provision persistent volumes when creating persistent volume claims, ensure that the persistent volumes are created manually with file access permissions that allow the database container and the DLS appliance container to write to them.
  5. Ensure that the versions of the database container and the DLS appliance container are identical.


###  [2.6.6.5. HA Configuration or Online Upgrade Fails](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#ha-configuration-or-online-upgrade-fails)
High availability (HA) configuration or online upgrade between two containerized DLS appliances fails if port mappings or volume configurations are incorrect.
  1. Ensure that the same ports are exposed and mapped internally for both containers. The following environment variables must be the same for both containers. 
     * `DLS_EXPOSED_HTTP_PORT`
     * `DLS_EXPOSED_HTTPS_PORT`
  2. Ensure that the `DLS_RABBITMQ_SSL_PORT` environment variable is set to 5671 for both containers
  3. Ensure that ports 5671, 8081 and 8084 are open on the containers within which the containerized DLS appliances are deployed.
  4. Ensure that Ports 8080 through 8085 are open on each worker node.
  5. Ensure that the sizes of the volumes rabbitmq-data, postgres-data, logs and configurations are equal to or exceed the minimum recommended storage space.   
| Volume  | Minimum Recommended Storage Space  |  
| --- | --- |  
| rabbitmq-data  | 2 GiB  |  
| postgres-data  | 10 GiB  |  
| logs  | 500 MiB  |  
| configurations  | 1 GiB  |  
  6. Ensure that the file access permissions on the folders inside the rabbitmq-data volume allow write access by the containers.


###  [2.6.6.6. Windows Client Cannot Return Licenses Because the Return Endpoint Is Unavailable](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#windows-client-cannot-return-licenses-because-return-endpoint-is-unavailable)
A licensed Windows client returns a license by sending an HTTP request to the DLS instance on port 80. If the container orchestrator within which a containerized DLS appliance is deployed cannot receive requests on this port, the client cannot return a license.
How to resolve this issue depends on whether you can control the port on which the container orchestrator listens for HTTP requests. 
  * If you can control the port on which the container orchestrator listens for HTTP requests from licensed clients, ensure that the `DLS_EXPOSED_HTTP_PORT` environment variable is set to 80.
  * Otherwise, use a load balancer to do path based routing of all license return requests to the URL `https://_dls-ip-address_:_dls-exposed-https-port_/leasing/v1/lessor/shutdown` 

_dls-ip-address_ 
     The IP address that is specified in the `DLS_PUBLIC_IP` environment variable.  

_dls-exposed-https-port_ 
     The port number that is specified in the `DLS_EXPOSED_HTTPS_PORT` environment variable. 
For more information about these environment variables, refer to [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.").


###  [2.6.6.7. Client Cannot Obtain License if Client Configuration Token Specifies the Domain Name](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#client-cannot-obtain-license-if-cct-specifies-fqdn)
A configuration error for a containerized DLS instance can prevent a licensed client from obtaining a license if the client configuration token specifies a fully qualified domain name. However, this error does not prevent a client from obtaining a license if the client configuration token specifies an IP address.
  1. Set the `FQDN` environment variable to the fully qualified domain name that is mapped to the IP address of the container in which the DLS software image is deployed. 
Ensure that this name can be resolved by licensed clients. Do **not** specify a name that can be resolved only by the container orchestrator.
  2. Restart the container in which the DLS appliance is deployed.


###  [2.6.6.8. Node Health Information Is Missing from the Home Page](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#node-health-information-missing-from-service-instance-page)
Insufficient storage for a containerized DLS instance can prevent node health information from being displayed on the Service Instance page of the management interface of the NVIDIA Licensing application.
  1. Force reload the Home page in the browser.
  2. If the node health information is still missing after the page is reloaded, check the amount of storage space available in the volumes created for the DLS appliance container. For more information about these volumes, refer to[ _Volume Mappings for a Containerized DLS Software Image_](https://sw-docs-dgx-station.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#volume-mappings-for-containerized-dls-software-image).
  3. If any volumes are full, resize the volumes and restart the container in which the DLS appliance is deployed.


###  [2.6.6.9. The podman-compose Command Fails with Missing Networks Error](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#podman-compose-fails-missing-networks)
In some deployments, the podman-compose up command might fail with the error `RuntimeError: missing networks: back-tier`.
This failure is caused by a known issue with Podman Compose 1.0.3 and earlier versions. Ensure that you are using one of the supported versions of Podman Compose listed in [NVIDIA License System Release Notes](https://docs.nvidia.com/license-system/latest/nvidia-license-system-release-notes/index.html). If necessary, install the latest version of Podman from GitHub. 
Copy
Copied!

```
            
            
# pip3 install https://github.com/containers/podman-compose/archive/devel.tar.gz

        
```

###  [2.6.6.10. Re-deploying a Containerized DLS Software Image on Red Hat OpenShift Container Platform Fails](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#openshift-container-orchestrator-permissions-issue)
Re-deploying a containerized DLS software image on Red Hat OpenShift Container Platform might fail because permission issues prevent a new PostgreSQL database container from being deployed. This issue can cause an in-place upgrade to fail because an in-place upgrade requires the containerized DLS software image to be re-deployed.
When this issue occurs, the following error messages are displayed: 
Copy
Copied!

```
            
            
chown: /etc/dls/config/ldap3-2.9.1.dist-info: Permission denied 
chown: /etc/dls/config/ldap3: Permission denied 
chown: /etc/dls/config/chardet-4.0.0.dist-info: Permission denied 
chown: /etc/dls/config/chardet: Permission denied 
chown: /etc/dls/config/certifi-2022.12.7.dist-info: Permission denied 
chown: /etc/dls/config/certifi: Permission denied

        
```

  1. Change to the directory where the configuration volume is mounted.
  2. Change to the /etc/dls/config/ directory. 
Copy
Copied!

```
            
            
$ cd /etc/dls/config/

        
```

  3. Forcibly remove the directories for which permission is denied and their contents. 
Copy
Copied!

```
            
            
$ \rm -rf ldap3-2.9.1.dist-info ldap3 \
chardet-4.0.0.dist-info chardet \
certifi-2022.12.7.dist-info certifi

        
```

  4. Try again to deploy the containerized DLS software image on Red Hat OpenShift Container Platform. For detailed instructions, refer to [Deploying the Containerized DLS Software Image on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-kubernetes-platforms "Perform this task if you are using Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. This procedure installs a single-replica DLS deployment \(Active - Passive\) using the legacy YAML files. To provide high availability with this method, deploy separate DLS instances and configure them as an HA cluster.").


###  [2.6.6.11. Containerized DLS Appliance fails to start on Openshift Platform](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#containerized-dls-appliance-fails-on-openshift-platform)
Containerized DLS Appliance fails to start on Openshift Platform with "Error: The directory named as part of the path `/var/log/supervisor/supervisord.log` does not exist".
  1. Navigate to the **logs** volume and create folders **supervisor** and**nginx**.
  2. Ensure that the file access permissions on the log volumes allow write access by the DLS appliance container, for example: 
Copy
Copied!

```
            
            
$ 707 (rwx---rwx)

        
```



###  [2.6.6.12. Multi-replica deployment does not reach Ready status](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#multi-replica-deployment-does-not-reach-ready-status)
**When this issue occurs**
After helm install or helm upgrade, the DLS instance phase stays in a starting or upgrading state for a long time, or changes to **Failed**. 
**Causes**
Common causes include: 
  * Missing or incorrect storage classes
  * Images that the cluster cannot pull
  * Replica count too low for a Rolling upgrade
  * Attempt to install an older version than the one already deployed
  * Temporary wait for database automation to become ready on a cold start


**Resolution**
  1. Run: 
Copy
Copied!

```
            
            
kubectl describe dlsappliance dls-main -n default

        
```

  2. Read the **status message** and events carefully.
  3. If the message mentions storage, see the storage topics below.
  4. If the message mentions image pull errors, confirm that the images exist in your registry and that any pull secret is configured.
  5. If the message mentions replicas or Rolling upgrade requirements, set at least **two** application copies (or an autoscaler minimum of two), then retry.
  6. If the message indicates a version downgrade, install only a version that is the same as or newer than the version already running.
  7. On a brand-new cluster, wait several more minutes for database automation to finish starting, then re-check the phase.


###  [2.6.6.13. Configurations storage remains Pending](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configurations-storage-remains-pending)
**When this issue occurs**
The “configurations” persistent volume claim stays **Pending** , and application copies cannot start. 
**Causes**
The cluster has no suitable **ReadWriteMany (RWX)** storage class, or the storage class name in your Helm values is wrong. 
**Resolution**
  1. Ask your platform administrator to list available storage classes.
  2. Identify an RWX / file-based class (for example NFS or file CSI).
  3. Set `instance.spec.storageClass` to that class name.
  4. Re-apply the Helm values and wait for the claim to become **Bound**.


Note:
Without RWX storage, multi-replica deployment is not supported with the default settings. 
###  [2.6.6.14. Database storage remains Pending](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#database-storage-remains-pending)
**When this issue occurs**
Database volume claims stay **Pending** , or database pods cannot start. 
**Causes**
The database storage class is missing, misspelled, or points to NFS / RWX storage instead of block **ReadWriteOnce (RWO)** storage. 
**Resolution**
  1. Set instance.spec.dbStorageClass to a **block (RWO)** storage class.
  2. Do **not** reuse the “configuration” NFS / RWX class for database data.
  3. Re-apply Helm values and confirm that database claims become **Bound**.


###  [2.6.6.15. Image pull errors prevent pods from starting](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#image-pull-errors-prevent-pods-from-starting)
**When this issue occurs**
Pods show ImagePullBackOff, ErrImagePull, or similar errors. 
**Causes**
The cluster cannot download one or more of these images from your registry:
  * `dls:operator_<version>`
  * `dls:appliance_<version>`
  * `dls:pgsql_<version>`


**Resolution**
  1. Confirm that the images were loaded into the registry path used in `global.image.repository`.
  2. Confirm that the image tags match the charts in your NLS artifact
  3. If the registry is private, create and reference the image pull secret as described in the Helm README.
  4. Delete failing pods after fixing registry access so they are recreated, or re-run the Helm install/upgrade.


###  [2.6.6.16. Management interface fails to load while pods are running](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#management-interface-fails-to-load-while-pods-are-running)
**When this issue occurs**
Pods are running, but the NVIDIA Licensing application management interface does not load in the browser. 
**Resolution**
  1. Confirm HTTPS connectivity to the published address (see the previous topic).
  2. Confirm that ports on the load balancer match the service ports for HTTPS and, if needed, HTTP lease return.
  3. Confirm that configurations and database volumes are **Bound** and writable.
  4. Confirm that the appliance and database image versions match the release you installed.
  5. Review operator logs and pod logs in the dls-system namespace for application start errors.


###  [2.6.6.17. Web UI is unreachable after a successful install](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#web-ui-is-unreachable-after-successful-install)
**When this issue occurs**
The instance phase is **Ready** , but a browser cannot load `https://<publicIP>/`or`https://<fqdn>/`. 
**Causes**
The client-facing address, DNS, firewall, load balancer, or OpenShift Route does not match the running service. 
**Resolution**
  1. Confirm that instance.spec.publicIP is the address clients actually use.
  2. If you use a host name, confirm that fqdn is set and that DNS resolves to the same address.
  3. From a client network, test: 
Copy
Copied!

```
            
            
curl -k https://<publicIP>/

        
```

  4. Confirm that firewall rules allow **443** (and **80** for Windows license return) to that address.
  5. Confirm that your load balancer or OpenShift Route forwards to the DLS service on the correct ports.
  6. Confirm reverse DNS if client configuration tokens depend on matching host names, as described in the User Guide host name resolution section.


###  [2.6.6.18. Rolling upgrade is stuck or fails](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#rolling-upgrade-is-stuck-or-fails)
**When this issue occurs**
During an upgrade, the phase stays in an upgrading state for a long time, or changes to **Failed**. 
**Causes**
  * Fewer than two application copies while using Rolling
  * PostgreSQL major version change attempted with Rolling
  * Controllers upgraded before the application and database finished migrating
  * Image pull or storage problems during the roll


**Resolution**
  1. Read the status message: 
Copy
Copied!

```
            
            
kubectl describe dlsappliance dls-main -n default

        
```

  2. Ensure replicas is at least **2** (or autoscaling minimum is at least 2).
  3. For a PostgreSQL **major** version change, use the **File-based upgrade** strategy as documented in the Helm README for your release—not Rolling.
  4. Confirm that you followed the **two-phase** upgrade: application and database first, automation controllers last.
  5. Check that database pods and application pods in dls-system are progressing (some may be terminating or starting during a healthy roll).
  6. Do not attempt a downgrade. Move only forward to a supported newer version.


###  [2.6.6.19. Licensed clients cannot obtain licenses](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#licensed-clients-cannot-obtain-licenses)
**When this issue occurs**
The Web UI may work for administrators, but licensed clients fail to acquire licenses. 
**Resolution**
  1. Confirm that the license server file is installed on the DLS instance.
  2. Confirm that clients use the correct client configuration token and the published address.
  3. Confirm that clients can reach port **443** on that address.
  4. Confirm DNS if the token embeds a host name.
  5. Prefer load-balancer sticky sessions (or source-IP preservation) so a client stays on one healthy application copy when possible.
  6. Test from a machine on the same network as the failing clients.


###  [2.6.6.20. Windows clients cannot return licenses at shutdown](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#windows-clients-cannot-return-licenses-at-shutdown)
**When this issue occurs**
Windows VMs cannot return licenses when they shut down. 
**Cause**
Windows clients return licenses over **HTTP on port 80**. The published endpoint is missing, blocked, or mapped to the wrong port. 
**Resolution**
  1. Ensure your load balancer, NodePort, or Route path forwards port **80** (or the lease-shutdown port you configured) to the DLS service.
  2. Ensure firewalls allow that port from client networks.
  3. If port 80 cannot be used in your environment, set the lease-shutdown port in Helm values and update the load balancer to match, as described in the Helm README.


This issue is the multi-replica form of the User Guide topic _Windows Client Cannot Return Licenses Because the Return Endpoint Is Unavailable_. 
###  [2.6.6.21. OpenShift security policy denies pods](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#opshift-security-policy-denies-pods)
**When this issue occurs**
On Red Hat OpenShift Container Platform, pods fail to start with security-context or SCC (Security Context Constraint) errors. 
**Resolution**
  1. Follow the OpenShift section of kubernetes/helm-charts/README.md for your release.
  2. Ensure the recommended SCC / permissions for the DLS charts are applied.
  3. Retry the install or upgrade after the platform policy is corrected.


###  [2.6.6.22. Temporary webhook errors at first install](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#temporary-webhook-errors-at-first-install)
**When this issue occurs**
Immediately after a cold install, you briefly see errors about the database webhook or controller not being ready. 
**Cause**
Database automation is still starting. The install charts wait for this in normal conditions, but a slow cluster can surface short-lived errors. 
**Resolution**
  1. Wait a few minutes.
  2. Re-check pods in `cnpg-system` and `dls-appliance-operator`.
  3. Re-check the DLS instance phase.
  4. Escalate only if the errors continue after the controllers are running and several minutes have passed.


Note:
If you moved the database controller out of `cnpg-system`, confirm that`operator.waitForCNPG.namespace` matches `cnpg.namespace`. A mismatch can leave the appliance operator waiting indefinitely. 
###  [2.6.6.23. Application or database pods stay Pending after you set node placement](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#application-or-database-pods-stay-pending-after-you-set-node-placement)
**When this issue occurs**
After you configure applianceScheduling, postgresScheduling, or poolerScheduling (for example dedicated node labels or taints), one or more pods in dls-system remain **Pending** , and the DLS instance may not become **Ready**. Setup or upgrade jobs may also stay Pending. 
**Cause**
Common causes include:
  * Node **labels** in Helm values do not match the labels on any worker node
  * Nodes are **tainted** , but the matching **tolerations** are missing or incorrect
  * `podAntiAffinity` is set to **Required** , and there are fewer eligible nodes than application or database copies
  * Dedicated pools do not have enough CPU or memory for the requested copies


**Resolution**
  1. List pods and why they are waiting: 
Copy
Copied!

```
            
            
kubectl get pods -n dls-system -o wide
kubectl describe pod <pending-pod-name> -n dls-system

        
```

Look for events such as **FailedScheduling** , untolerated taint, or insufficient nodes for anti-affinity.
  2. Confirm node labels and taints with your platform team: 
Copy
Copied!

```
            
            
kubectl get nodes --show-labels
kubectl describe node <node-name>

        
```

  3. Ensure every tainted dedicated node has a matching **toleration** in the corresponding scheduling block. A label alone is not enough on a tainted node.
  4. If you used `podAntiAffinity: Required,` either add enough matching nodes or change the mode to `Preferred`.
  5. For correct examples and defaults, see **Controlling Where DLS Pods Run**.


**Related documentation**  
| Document  | Location  |  
| --- | --- |  
| NVIDIA License System User Guide  | <https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html>  |  
| Helm commands for your release  |  `kubernetes/helm-charts/README.md` in the NLS BIOS package  |  
| NVIDIA License System Release Notes  | Docs site / download portal for your release  |  
###  [2.6.6.24. When to escalate](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#when-to-escalate)  
| Issue type  | Contact  |  
| --- | --- |  
| Storage class, load balancer, Route, cluster capacity, registry auth  | Your Kubernetes / platform team  |  
| DLS licensing behavior, operator status messages, upgrade logic  | NVIDIA support  |  
When you contact NVIDIA support for a multi-replica issue, include:
  * NLS / DLS version
  * Platform (Kubernetes, OpenShift, or Tanzu) and version
  * Output of kubectl describe dlsappliance dls-main -n default (or oc)
  * Recent operator logs
  * Your Helm values with secrets removed


###  [2.6.6.25. Related documentation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#related-documentation)  
| Issue type  | Contact  |  
| --- | --- |  
| NVIDIA License System User Guide  | https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html  |  
| Helm commands for your release  | kubernetes/helm-charts/README.md in the NLS BIOS package  |  
| NVIDIA License System Release Notes  | Docs site / download portal for your release  |  
###  [2.7. Installing the DLS Software Image on Red Hat Enterprise Linux OS](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-software-image-on-red-hat-enterprise-linux-os)
For installation on a supported operating system, the Delegated License Server (DLS) component of the NVIDIA License System is supplied as an installable package. The package includes the containerization software and container images that are required to run the NVIDIA Licensing application on the operating system. The operating system can be running in a virtualized server environment on your choice of hypervisor or on a bare-metal server.
Ensure that the server on which you are installing the DLS software image has at least 15 GB of free disk space.
The DLS software image ZIP archive for the Red Hat Enterprise Linux OS contains the following artifacts: 
  * Docker container orchestrator RPM packages
  * Appliance container images 
    * Database container image
    * Application container image
  * Installation script with the helper scripts
  * README file
  * VERSION file


  1. Extract the contents of the ZIP archive to a local folder.
  2. Update the file access permissions of the installApplicance.sh file to allow execute permission. 
Copy
Copied!

```
            
            
$ chmod +x installAppliance.sh

        
```

  3. Run the installApplicance.sh installation script.
  4. When the installation is complete, confirm that messages on the console indicate that the NVIDIA Licensing application has started.


###  [2.8. Configuring User Accounts on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#user-accounts-on-dls-virtual-appliance)
Each DLS software image is configured with a single standard user account for accessing the DLS appliance and an account with sudo user privileges for installing updates to the DLS appliance software. Modifications to these accounts are strictly controlled. You cannot add other user accounts to the software image. However, you can use a lightweight directory access protocol (LDAP) directory instead of the configured accounts for managing user access to a DLS appliance. DLS Virtual Appliance supports user account management at two levels: **OS-level user accounts** (dls_admin, rsu_admin) exist on the underlying operating system and are used for appliance management, while **application-level user accounts** (introduced in DLS 3.6.0) are created via DLS Web UI or API for accessing DLS management interfaces. Application users exist only in the DLS application database and support role-based access control (RBAC) with three pre-defined roles: DLS_ADMIN, DLS_OPERATOR, and DLS_USER.  
| User Account  | Purpose  |  
| --- | --- |  
| `dls_admin`  | DLS administrator account. This account provides access through a web-based management interface to the NVIDIA Licensing application on a DLS virtual appliance. The DLS administrator user name can be changed from the preset `dls_admin` name.  |  
| `rsu_admin`  | DLS sudo user account. This user account has the elevated privileges required to install updates to the DLS appliance software that NVIDIA releases periodically. To comply with the terms of the GPL/LGPL v3 license under which the GPL/LGPL v3 licensed Open Source Software (OSS) libraries within the DLS virtual appliance are released, this account also has the elevated privileges required to update and upgrade these libraries.  Note: This account is not available for containerized DLS software images.   |  
###  [2.8.1. Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user)
Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.
If you intend to configure a cluster of DLS instances, you need perform this task only for the DLS instance from which you will configure the cluster. The registration of the DLS administrator user is propagated from this instance to the other instance when you configure the cluster. 
Note:
See [DLS Appliance Setup Tool](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool "The DLS Appliance Setup tool consists of standalone programs you run on a Unix computer to configure the Delegated License Service \(DLS\) appliance. The tools register the appliance with the NVIDIA Licensing Portal \(NLP\), bind it to a license allocation, install the license file on the appliance, and can configure appliance settings such as NTP, network file sharing \(NFS\), and LDAP. You do not need to install Python on the computer where you run these tools.") for detailed information. 
  1. Open a web browser and connect to the URL `https://_dls-vm-ip-address_`. 

_dls-vm-ip-address_ 
     The IP address or, if defined, the fully qualified domain name or the CNAME of the VM on which the DLS virtual appliance is installed. 
You can get the IP address from the management console of your hypervisor.
  2. On the Set Up page that opens, click **NEW INSTALLATION**.
  3. On the Register User page that opens, provide the credentials for the DLS administrator user. 
Note:
If the DLS administrator user has already been registered, the login page opens instead of the Register User page. 
    1. **Optional:** If you want to change the user name from the preset name dls_admin, replace the text in the **Username** field with your choice of user name.
    2. Provide a password for the DLS administrator user and confirm the password. 
The password is case sensitive and must contain 15 to 64 characters, including at least one uppercase letter, one lowercase letter, one digit, and one special character. It must not contain whitespace or the username, and it must pass the appliance's password-strength validation.
  4. On the Register User page that opens, provide the credenttials for the DLS administrator user. 
Note:
If the DLS administrator user has already been registered, the login page opens instead of the Register User page. 
    1. **Optional:** If you want to change the user name from the preset name dls_admin, replace the text in the **Username** field with your choice of user name.
    2. Provide a password for the DLS administrator user and confirm the password. The password must be at least eight characters long and is case sensitive.
Note:
You can change the DLS administrator user name and password at any time after the DLS administrator user is registered. For instructions, refer to [Changing the DLS Administrator User Name and Password](https://sw-docs-dgx-station.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-dls-administrator-user-name-and-password). 


If you want to use the virtual appliance for a single DLS instance, what to do next depends on whether you intend to use a static IP address for the virtual appliance that is hosting the DLS instance. 
  * If you want to use the virtual appliance in an HA cluster of DLS instances, configure the cluster as explained in [Configuring an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ha-cluster-dls-instances "To provide licensed clients with continued access to licenses if a DLS instance fails, you can configure a highly available \(HA\) cluster of two or more DLS instances in a failover configuration. A failover configuration consists of a primary instance, which is actively serving licenses to licensed clients, and one or more secondary instances, which act as a backup for the primary instance.").
  * If you want to use a static IP address for the virtual appliance that is hosting the DLS instance, set the address as explained in [Setting the Static IP Address of a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address "You can use the management interface to the NVIDIA Licensing application to replace the existing IP address of the appliance with a new static IP address. The existing IP address can be an address assigned by DHCP or another static IP address.").
  * Otherwise, configure the DLS instance as explained in [Configuring a Service Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-service-instance "How you configure a service instance depends on whether the service is a Cloud License Service \(CLS\) instance or a Delegated License Service \(DLS\) instance.").


If you need to reset the DLS administrator user's password, follow the **Forgot Password?** link on the login page and, when prompted, type the local reset secret, provide a new password for this user, and confirm the new password. 
###  [2.8.2. Retrieving the DLS Administrator User's Reset Secret](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#retrieving-dls-administrator-user-reset-secret)
If you need to reset the DLS administrator user's password but do not have the local reset secret, you can download a reset secret from the NVIDIA Licensing Portal.
  1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
  2. Click **License Allocations**.
  3. Locate the DLS allocation you are resetting the password for, open it, click More DLS actions, and then select Download reset secret token.


###  [2.8.3. Changing the DLS Administrator Password](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-dls-administrator-user-name-and-password)
The administrator account created during appliance registration is a protected account. Its username cannot be changed after registration. You can change its password while signed in to that account, even when additional local users exist. You do not need to delete those users first. . 
Note:
If there is more than one user in the system, changing the username or password will not be allowed. For more details on how to delete a user when multiple users exist, please refer to [User Deletion in Appliances with Multiple Users Registered](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#user-deletion-in-appliances-with-multiple-users-registered "The deletion of an additional user in a system with multiple users is facilitated through a shell script. This script must be executed on the primary node within a High Availability \(HA\) cluster. It ensures that user deletion is only performed if the specified user exists and if the system has more than one registered user. If the user does not exist or if the system contains only a single user, the script will not execute the deletion, thereby maintaining the integrity of the user management process."). 
Note:
For information about managing multiple application-level users with RBAC, see Section 2.8.4 and subsequent subsections. 
  1. Open a web browser and connect to the URL `https://_dls-vm-ip-address_`. 

_dls-vm-ip-address_ 
     The IP address or, if defined, the fully qualified domain name or the CNAME of the VM on which the DLS virtual appliance is installed. 
You can get the IP address from the management console of your hypervisor.
  2. Sign in to the DLS Web UI with the protected administrator account.
  3. Select your username at the top right to open User Profile.
  4. Select Security, then Password.
  5. Enter the current password in Current password. Enter the new password in New password and enter it again in Confirm new password. The new password must meet the requirements shown in the form.
  6. Select Change Password.
  7. Choose the protected administrator username during registration. It cannot be changed after registration. You can subsequently change the password as described in Changing the DLS Administrator Password. 
    1. Select the signed-in username at the top right to open User Profile.
    2. In User Profile, choose Security; the tab is highlighted.
    3. Use the current, new, and confirmation password fields, then Change Password. The protected username restriction and password requirements are shown.


###  [2.8.4. Creating the DLS sudo User Account](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-dls-sudo-user)
The predefined DLS sudo user account `rsu_admin` has the elevated privileges required to install updates to the DLS appliance software that NVIDIA releases periodically. To comply with the terms of the GPL/LGPL v3 license under which the GPL/LGPL v3 licensed Open Source Software (OSS) libraries within the DLS virtual appliance are released, this account also has the elevated privileges required to update and upgrade these libraries.
Perform this task for each DLS virtual appliance for which you want to create the DLS sudo user account. If the DLS virtual appliance is hosting a node in an HA cluster, the creation of the user is **not** automatically propagated to the other node in the cluster. 
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
  1. From the hypervisor, log in as the user dls_admin to the VM that hosts the DLS virtual appliance.
  2. Run the /etc/adminscripts/enable_sudo.sh script. 
Copy
Copied!

```
            
            
$ /etc/adminscripts/enable_sudo.sh

        
```

  3. When prompted, provide a password for the `rsu_admin` user.


The sudo user with elevated privileges `rsu_admin` is created. 
###  [2.8.5. Understanding Role-Based Access Control (RBAC)](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#understanding-role-based-access-control-rbac)
Role-Based Access Control (RBAC) is an access control method in which permissions to perform operations are assigned to roles, and users are assigned to roles. In DLS, RBAC provides:
  * **Separation of duties** between administrators, operators, and viewers
  * **Least privilege principle** by granting only the permissions needed for each role
  * **Simplified administration** by managing permissions at the role level instead of per-user
  * **Audit compliance** through clear role definitions and assignment tracking


Note:
Roles assigned by Single Sign-On group or user mappings are the same. See [Mapping Identity Provider Groups to DLS Roles](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#mapping-identity-provider-groups-to-dls-roles). 
###  [2.8.5.1. DLS Roles and Permissions](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-roles-and-permissions)
DLS provides three pre-defined roles with distinct permission levels.  
| Role  | Privilege Level  | Primary Use Case  |  
| --- | --- | --- |  
| **DLS_ADMIN**  | 1 (Highest)  | Full system administration and configuration  |  
| **DLS_OPERATOR**  | 2 (Medium)  | Day-to-day license operations and management  |  
| **DLS_USER**  | 3 (Lowest)  | Read-only access for viewing and reporting  |  
Note:
Roles are pre-defined and cannot be customized. Organizations should assign users to the appropriate pre-defined role based on their job responsibilities. 
###  [2.8.5.2. Role Capabilities and Use Cases](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#role-capabilities-and-use-cases)
**DLS_ADMIN (Administrator)**
**Purpose** : Complete system administration with full control over all DLS operations. 
**Key Capabilities** : 
  * **User Management** : Create, modify, and delete local user accounts
  * **Role Management** : Assign and modify user role assignments
  * **LDAP Configuration** : Configure LDAP/Active Directory integration and group mappings
  * **System Configuration** : Modify all system settings including SSL/TLS, email alerts, and general settings
  * **Node Management** : Configure high availability, IP addresses, and node operations
  * **License Management** : Full CRUD (create, read, update, delete) operations on license pools and servers
  * **Maintenance Operations** : Perform system upgrades, database backups, snapshots, and migrations
  * **Security Configuration** : Manage SSL certificates, TLS cipher settings, and API keys
  * **Audit and Compliance** : Export, archive, and manage audit event logs


**Typical Job Roles** : 
  * DLS System Administrators
  * IT Infrastructure Administrators
  * Security Administrators
  * DevOps Engineers (with administrative responsibilities)


**Access Level** : Full read and write access to all resources and configurations. 
**DLS_OPERATOR (Operator)**
**Purpose** : Operational access for day-to-day license management without ability to modify security settings, users, or system infrastructure. 
**Key Capabilities** : 
  * **License Operations** : Create, update, and delete license pools and servers
  * **Service Instance Management** : Modify service instance configurations
  * **Settings Configuration** : Configure general settings, email alerts (excludes SSL, TLS, LDAP)
  * **Client Configuration** : Generate client configuration tokens
  * **Lease Operations** : View all leases and perform bulk release operations: Configure high availability, IP addresses, and node operations
  * **Monitoring** : View metrics, events, and system status
  * **API Key Management** : Import and export API keys


**Restrictions** (Cannot perform): 
  * User or role management
  * LDAP configuration changes
  * Node management or HA configuration
  * SSL/TLS certificate or cipher configuration
  * System upgrades or maintenance operations
  * Individual lease creation, modification, or deletion
  * Event export, import, or archiving


**Typical Job Roles** : 
  * License Administrators
  * Operations Team Members
  * Technical Support Staff
  * Application Administrators


**Access Level** : Read access to all resources; write access to license pools, service instances, and operational settings only. 
**DLS_USER (User/Viewer)**
**Purpose** : Read-only access for viewing license information, system status, and generating reports. 
**Key Capabilities** : 
  * **View Operations** : View all license pools, servers, leases, and configurations
  * **Reporting** : Generate reports on license usage and system status
  * **Monitoring** : View system metrics, events, and alerts: Configure general settings, email alerts (excludes SSL, TLS, LDAP)
  * **Information Access** : Access all user, role, and system information in read-only mode


**Restrictions** (Cannot perform): 
  * Any create, update, or delete operations
  * Configuration changes
  * User or role management
  * License pool or server modifications
  * System administration tasks


**Typical Job Roles** : 
  * End Users viewing their license allocations
  * Business Stakeholders monitoring license usage
  * Technical Support Staff
  * Reporting Analysts


**Access Level** : Read-only access to all resources; no write or configuration capabilities. 
**Permission Hierarchy and Precedence**
When a user has multiple role assignments (ONLY possible with LDAP multi-group membership), DLS applies the role with the **highest privilege** : 
Copy
Copied!

```
            
            
Priority 1 (Highest): DLS_ADMIN (privilege_level = 1)
	↓
Priority 2 (Medium):  DLS_OPERATOR (privilege_level = 2)
	↓
Priority 3 (Lowest):  DLS_USER (privilege_level = 3)

        
```

If an LDAP user belongs to both “License_Admins” (mapped to DLS_ADMIN) and “License_Users” (mapped to DLS_USER), the user will be assigned the DLS_ADMIN role (highest privilege). 
###  [2.8.6. Managing Local Users](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#managing-local-users)
Local users are application-level user accounts created and managed directly within the DLS instance. Local users authenticate using credentials stored in the DLS database (separate from LDAP/Active Directory). 
To manage local accounts, sign in as an administrator and select Operations > Users. The Users page displays each account's username, account type, and role. Accounts marked Protected can be viewed but cannot be edited or deleted from this page. 
Important:
Local users are application users that exist only in the DLS application database. They are NOT created on the underlying operating system. The only OS-level users are _dls_admin_ (protected user) and _rsu_admin_ (sudo user), which are created during appliance setup. SCIM only rotates passwords on users that already exist. 
**Prerequisites**
To manage local users, you must:: 
  * Have the **DLS_ADMIN** role assigned to your account
  * Be logged into the DLS Web UI


###  [2.8.6.1. Creating Local Users (DLS Web UI)](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-local-users)
**Procedure**
  1. Open Operations > Users and select Create User.
  2. In the Create User panel, enter a unique Username and a Password that meets the requirements shown in the form.
  3. Select Admin, Operator, or User from the Role list.
  4. Select Create User. Confirm that the new account appears on the Users page with the intended role.


**Post-Creation Steps**
  * Securely communicate the credentials to the user
  * Instruct the user to change their password on first login (recommended)


###  [2.8.6.2. Assigning Roles to Users](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#assigning-roles-to-users)
**During User Creation**
  * Select the appropriate role from the “Role” dropdown during user creation
  * The role assignment is automatically created when the user is saved


**Modifying an Existing User’s Role** : 
  1. Open Operations > Users.
  2. Open the action menu for a Standard account and select Edit.
  3. In the Edit User panel, select the required Role. Leave New Password empty if you are only changing the role.
  4. Select Save Changes.


Important:
  * Each user can have only one role assigned at a time
  * Changing a user’s role immediately affects their access permissions
  * Active user sessions continue with old permissions until token expiration (typically 1 hour)
  * For immediate effect, the user should log out and log back in


###  [2.8.6.3. Modifying User Accounts](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#modifying-user-accounts)
**Procedure** : 
  1. Open Operations > Users, open the Standard account's action menu, and select Edit.
  2. Change Role or enter a New Password as required. Leave New Password empty to keep the current password. The username cannot be edited in this panel.
  3. Select Save Changes.


To change your own password, select your signed-in username at the top right, then select 
  * User Profile > Security > Password 
    * Enter the current password, the new password. and its confirmation.
    * Select Change Password.
  * **Change Username** : Navigate to **User Profile** > **Change Username**
    * Requires current password verification
    * New username must be unique


###  [2.8.6.4. Deleting User Accounts](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deleting-user-accounts)
**Prerequisites** : 
  * Must have DLS_ADMIN role
  * Cannot delete the system application user (first application user created during setup)
  * Cannot delete your own user account (must be deleted by another administrator)


Note:
This function deletes application-level users from the DLS database. OS-level users (_dls_admin_ , _rsu_admin_) cannot be deleted through DLS Web UI or APIs as they exist on the underlying operating system and are required for appliance management. 
**Procedure** : 
  1. Open Operations > Users and locate the Standard account to delete.
  2. Verify that you have selected the correct account, then open its action menu and select Delete.
  3. Confirm that the success notification appears and the account is removed from the Users list.
  4. In the confirmation dialog, review the warning: 
     * User account will be permanently deleted
     * All role assignments will be removed
     * Action cannot be undone
  5. Click **Confirm** to proceed with deletion
  6. A success message confirms the deletion
  7. The user is removed from the user list


**Impact of User Deletion** : 
  * User can no longer log in to DLS
  * Active sessions for the deleted user are invalidated
  * Audit logs retain records of the user’s past actions
  * Role assignments are automatically removed (cascade delete)


###  [2.8.6.5. Password Management](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#password-management)
###  [2.8.6.5.1. Password Policy Requirements](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#password-policy-requirements)
Also see : [About SCIM Password Rotation on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-scim-password-rotation-on-dls-virtual-appliance). 
**Password Policy Requirements** : 
All passwords (local users and API-generated) must comply with DLS password policy: 
**Length Requirements** : 
  * Minimum: 15 characters
  * Maximum: 64 characters


**Complexity Requirements** (must contain at least one of each): 
  * Uppercase letter (A-Z)
  * Lowercase letter (a-z)
  * Digit (0-9)
  * Special character: _!@#$%^ &*()_+-=[]{}|;:,.<>?_


**Security Requirements** : (must contain at least one of each): 
  * Must not contain whitespace characters
  * Must not contain the username
  * Must not contain repetitive patterns (e.g., “aaa”, “111”)
  * Must not match any of the last 5 passwords (password history)
  * Must differ by at least 8 characters from the previous password


**Example Valid Passwords** : (must contain at least one of each): 
  * MySecureP@ssw0rd2024!
  * ComplexLicense#Admin99
  * Str0ng!DLS$Access2024


**Example Invalid Passwords** : 
  * short (too short, less than 15 characters)
  * NoSpecialCharacter123 (missing special character)
  * all lowercase password! (missing uppercase letter)
  * john.doe@Example123! (contains username “john.doe”)


###  [2.8.6.5.2. **Changing User Passwords (Web UI)**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-user-passwords)
Also see : [About SCIM Password Rotation on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-scim-password-rotation-on-dls-virtual-appliance). 
**Self-Service Password Change**
  1. Sign in to the DLS Web UI and select your username at the top right to open User Profile.
  2. Select Security, then Password.
  3. . Enter: 
     * **Current Password**
     * **New Password** :
     * **Confirm new password**
  4. Select **Change Password**
  5. Success message confirms the password change
  6. You are logged out automatically (in some implementations)
  7. Log back in with the new password


**Administrator Password Reset**
An administrator can set a new password for a Standard local account through the Edit User panel. The administrator supplies the password; DLS does not generate or display a temporary password in this workflow. Protected accounts cannot be reset from the Users page. 
  1. Open Operations > Users
  2. Open the target Standard account's action menu and select Edit.
  3. Enter the required password in New Password and select Save Changes.
  4. Communicate the new password securely to the user and ask the user to change it after signing in.


###  [2.8.6.5.3. Configuring SCIM 2.0 Password Rotation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-scim-20-password-rotation)
This section discusses SCIM password rotation. See this topics: 
  * [About SCIM Password Rotation on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-scim-password-rotation-on-dls-virtual-appliance)
  * [Before You Configure SCIM Password Rotation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-scim-password-rotation)
  * [Integrating with an Enterprise Password Vault](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#integrating-with-enterprise-password-vault)
  * [Verifying SCIM Password Rotation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-scim-password-rotation)
  * [Troubleshooting SCIM Password Rotation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-scim-password-rotation)


###  [2.8.6.5.3.1. About SCIM Password Rotation on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-scim-password-rotation-on-dls-virtual-appliance)
Read this before you enable SCIM 2.0 password rotation for local DLS users. 
**What is SCIM on DLS?**
**SCIM** (System for Cross-domain Identity Management) is an open standard that identity and password-management products use to talk to applications in a consistent way. 
Note:
SCIM password rotation applies to local DLS users and is independent of which [Web UI sign-in method](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-settings) (local, LDAP, or SSO) used. 
On DLS, SCIM is used so an **Enterprise Password Vault (EPV)** — for example CyberArk Privileged Access Manager or Privilege Cloud — can **look up** an existing local DLS user and **set a new password** on a schedule.   
| Term  | Plain-language meaning  |  
| --- | --- |  
| **Enterprise Password Vault (EPV)**  | A privileged-access system that stores and rotates passwords for important accounts.  |  
| **Local DLS user**  | An application user stored on the DLS appliance (not an LDAP directory user).  |  
| **SCIM Service Provider**  | In this integration, DLS is the application that receives SCIM requests from the vault.  |  
| **Bearer token**  | A secret string the vault sends so DLS accepts SCIM requests.  |  
Licensed clients (for example vGPU clients) continue to obtain licenses as they do today. SCIM does not change how licensed clients check out licenses. 
**What DLS does and does not do**
**DLS does:**
  * Allow the vault to find an existing local user by username.
  * Accept a new password from the vault and apply it after password-policy checks.
  * Update the local DLS account (and the related system account password when applicable).
  * Keep an audit trail of vault-driven password changes.
  * Omit the password from SCIM responses (the vault already has the new secret).


**DLS does not:**
  * Create, rename, disable, or delete users through SCIM in this release.
  * Rotate passwords for **LDAP / Active Directory** users (rotate those in the directory, not via DLS SCIM).
  * Provide SCIM on **Cloud License Service (CLS)**.
  * Replace the vault — DLS only accepts the password the vault generates.


**How password rotation works**
  1. An administrator creates the local DLS user in the DLS Web UI (or existing multi-user API), if the account does not already exist.
  2. The administrator enables SCIM on DLS and stores a bearer token in the vault.
  3. The vault onboards the DLS account and, on schedule, generates a new password.
  4. The vault asks DLS for the user, then sends the new password to DLS.
  5. DLS validates the password against DLS policy and saves it.
  6. Operators later sign in to DLS with the password currently stored in the vault.


**How SCIM relates to other password methods**  
| Method  | Who uses it  | Typical purpose  |  
| --- | --- | --- |  
| SCIM password update  | Enterprise Password Vault / PAM  | Scheduled privileged rotation (recommended for EPV).  |  
| Web UI change / reset  | Human administrators  | Day-to-day password changes.  |  
| Proprietary password-rotate API  | Custom scripts  | Non-SCIM automation; not the EPV path.  |  
Note:
For EPV compliance programs, configure the vault to use **SCIM** , not the proprietary password-rotate API 
**How SCIM relates to LDAP**  
| Feature  | Purpose  |  
| --- | --- |  
| **Local users + SCIM**  | Vault rotates passwords for local appliance accounts.  |  
| **LDAP**  | Users sign in against a corporate directory.  |  
Also see: 
  * [Configuring Single Sign-On (SSO) Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-sso-authentication "Single Sign-On allows administrators and operators to open the DLS Web UI by signing in with your corporate identity provider, instead of \(or in addition to\) a password stored on the DLS appliance.")


SCIM password rotation targets **local** DLS users. Directory users authenticate elsewhere; their passwords are not managed by DLS SCIM. 
**What to do next**
  1. Review [**Before You Configure SCIM Password Rotation**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-scim-password-rotation).
  2. Continue with [**Enabling SCIM on DLS**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#enabling-scim-on-dls "Perform this task to turn on SCIM 2.0 on a DLS virtual appliance and create a bearer token for your Enterprise Password Vault.").
  3. Continue with [**Integrating with an Enterprise Password Vault**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#integrating-with-enterprise-password-vault).
  4. Continue with [**Verifying SCIM Password Rotation**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-scim-password-rotation).


###  [2.8.6.5.3.2. Before You Configure SCIM Password Rotation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-scim-password-rotation)
Ensure that the following requirements are met before you enable SCIM 2.0 password rotation on a DLS virtual appliance. 
**Who should perform these tasks?**
  * A DLS administrator who can manage local users and authentication settings.
  * A PAM / vault administrator who can onboard a target application and rotation policy.
  * Optionally, a network or platform administrator for HTTPS connectivity from the vault to DLS.


**General requirements**
  1. You are using a **DLS Virtual Appliance** (or containerized DLS software image). SCIM password rotation is **not** supported on Cloud License Service (CLS).
  2. The local DLS user accounts that the vault will manage **already exist** (created in the DLS Web UI or existing multi-user API). SCIM does not create users in this release.
  3. Those accounts are **local** DLS users — not LDAP-only directory users.
  4. The vault can reach the DLS management address over **HTTPS** (TLS 1.2 or later).
  5. You have DLS **Administrator** rights and, for API enablement, an Enterprise-scoped API key if your process uses the REST API.
  6. Vault password-generation rules will be aligned with the DLS password policy (see below).


**Password policy alignment**
DLS rejects passwords that do not meet appliance policy. Align the vault’s password generator with DLS requirements documented under **Password Management** in the User Guide, including typical rules such as:
  * Length within the DLS-allowed range.
  * Uppercase, lowercase, digit, and special character requirements.
  * No username substring.
  * Sufficient difference from the previous password.
  * Not reusing recent passwords from DLS password history.


**Information to collect**  
| Item  | Example / notes  |  
| --- | --- |  
| DLS public HTTPS address  | <https://dls.example.com>  |  
| SCIM base URL  | <https://dls.example.com/scim/v2>  |  
| Local usernames to rotate  | Exact DLS userName values  |  
| Enterprise API key  | To enable SCIM and create a bearer token (if using API)  |  
| Vault platform type  | SCIM 2.0 custom / target application pattern  |  
**Security planning**
  1. Store the SCIM bearer token only in the vault (or equivalent secret store).
  2. Plan operational rotation or replacement of the bearer token on a schedule (for example every 90–365 days) or after suspected compromise.
  3. Limit which local accounts are onboarded in the vault to those that truly need privileged rotation.
  4. Ensure audit logging is retained for both DLS and the vault so rotations can be correlated.


**What to do next**
  * Create any missing local users in the DLS Web UI.
  * Continue with [**Enabling SCIM on DLS**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#enabling-scim-on-dls "Perform this task to turn on SCIM 2.0 on a DLS virtual appliance and create a bearer token for your Enterprise Password Vault.").


###  [2.8.6.5.3.3. Enabling SCIM on DLS](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#enabling-scim-on-dls)
Perform this task to turn on SCIM 2.0 on a DLS virtual appliance and create a bearer token for your Enterprise Password Vault.
Before you begin, complete **Before You Configure SCIM Password Rotation** and **About SCIM Password Rotation on a DLS Virtual Appliance**. 
**Overview of the setup**
  1. Confirm the target local users already exist on DLS.
  2. Enable SCIM on the appliance.
  3. Create a SCIM bearer token and store it in the vault.
  4. Do not onboard LDAP-only users for DLS SCIM rotation.


Note:
If your release provides Web UI controls for SCIM, use those labels when screenshots are available. The steps below describe the administrator API flow used for enablement. 
**Confirm local users exist**
  1. Sign in to the DLS Web UI as an Administrator.
  2. Open user management and confirm each account the vault will rotate.
  3. If an account is missing, create it as a **local** user and assign the correct DLS role.
  4. Do not onboard LDAP-only users for DLS SCIM rotation.


**Enable SCIM**
Using an Enterprise API key with permission to configure authentication, enable SCIM on the appliance. 
Example: 
Copy
Copied!

```
            
            
PUT https://{DLS_FQDN}/auth/v1/scim/config
x-api-key: {enterprise_api_key}
Content-Type: application/json
 
{
  "enabled": true
}

        
```

Replace {DLS_FQDN} with the host name administrators use for the appliance. 
Note:
On Cloud License Service (CLS), SCIM is not supported. Enablement or SCIM calls should fail with a forbidden response. 
**Create a bearer token**
Create a SCIM bearer token and copy it immediately into your vault secret store. 
Example: 
Copy
Copied!

```
            
            
POST https://{DLS_FQDN}/auth/v1/scim/token
x-api-key: {enterprise_api_key}

        
```

Treat the token like a password:
  1. Do not commit it to source control or share it in chat.
  2. Store it only in the vault configuration for the DLS target application.
  3. Replace it if it may have been exposed.


**Note the SCIM base URL**
Your vault needs:   
| Setting  | Value  |  
| --- | --- |  
| SCIM base URL  | https://{DLS_FQDN}/scim/v2  |  
| Authentication  | Bearer token (HTTP Authorization header)  |  
| User unique identifier  | DLS local userName  |  
**What to do next**
Configure the vault target application. See [**Integrating with an Enterprise Password Vault**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#integrating-with-enterprise-password-vault). 
###  [2.8.6.5.3.4. **Integrating with an Enterprise Password Vault**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#integrating-with-enterprise-password-vault)
Perform this task to configure an Enterprise Password Vault (EPV) or PAM platform so it rotates passwords for existing local DLS users using SCIM 2.0. 
Before you begin, complete **Enabling SCIM on DLS**. 
**Overview of the setup**
  1. Create a privileged account entry in the vault for each DLS local user.
  2. Point the account at DLS as a **SCIM 2.0** target application.
  3. Align vault password complexity with DLS password policy.
  4. Run a manual rotation, then enable the schedule.


Exact screens differ by vault product. The guidance below maps common CyberArk / PAM concepts to DLS values. 
**DLS values for the vault connector**  
| Vault setting (typical name)  | DLS value  |  
| --- | --- |  
| SCIM Base URL  | <https://%7BDLS_FQDN%7D/scim/v2>  |  
| Authentication  | HTTP header: Authorization: Bearer {scim_bearer_token}  |  
| User unique ID / correlation  | userName (exact local DLS username)  |  
| Password sync / update  | SCIM PATCH on the User resource password attribute  |  
| Capability discovery  | Vault may call Service Provider Config / Schemas automatically  |  
Replace {DLS_FQDN} with the fully qualified domain name of your DLS appliance. Use HTTPS. 
Onboard each DLS local account in the vault 
For every local account to rotate:
  1. Create a privileged account record whose username matches the DLS local username exactly.
  2. Select a platform / target type suitable for **SCIM 2.0 custom application** (wording varies by product).
  3. Enter the SCIM base URL and bearer token from DLS.
  4. Do **not** mark LDAP directory users for this DLS SCIM target — rotate those accounts in Active Directory instead.


**Align password generation with DLS policy**
  1. In the vault password policy / platform policy:
  2. Match length and character-class rules to DLS **Password Management** requirements.
  3. Allow the vault to regenerate and retry when DLS rejects a candidate password.
  4. Avoid generating passwords that embed the username.


If DLS rejects a password, rotation fails until the vault produces a compliant value. 
**Understand what the vault calls on DLS**
  1. A typical successful rotation looks like this: 
    1. **Find the user** — the vault looks up the account by userName
    2. **Set the password** — the vault sends the new password to DLS.
    3. **Confirm** — DLS returns the user record **without** including the password.
    4. **Store** — the vault keeps the new secret as the current password for that account.


Administrators do not need to paste passwords into DLS during vault rotation. 
**Dual-account or rotational groups (if your vault uses them)**
  1. Some vaults rotate paired active/inactive accounts. On DLS: 
    1. Each local user is a separate SCIM user.
    2. DLS does not implement vault-specific rotational-group logic.
    3. The vault decides which account is active; onboard each DLS username separately.


**System user vs other local users**  
| Account type  | SCIM password rotation  |  
| --- | --- |  
| Local application users created in the Web UI  | Supported  |  
| Initial / system-style local appliance admin user  | Supported when it exists as a local DLS user  |  
| LDAP / AD users  | Not via DLS SCIM — rotate in the directory  |  
**Next Steps**
Verify lookup and rotation before relying on the schedule. See [**Verifying SCIM Password Rotation**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-scim-password-rotation). 
###  [2.8.6.5.3.5. Verifying SCIM Password Rotation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-scim-password-rotation)
Perform this task after SCIM is enabled and the Enterprise Password Vault is configured for DLS. 
**Before you verify**
  * SCIM is enabled on the DLS appliance.
  * The bearer token is configured in the vault (or available for a controlled test).
  * The target local username exists on DLS.
  * You can sign in to the DLS Web UI to confirm the new password afterward.


**Discover SCIM capabilities (optional)**
Confirm that DLS advertises patch support by requesting the service provider configuration from the SCIM base URL with the bearer token. 
**Look up the user**
Ask the vault (or a controlled test client) to find the user by exact userName. 
Success criteria:
  * DLS returns the user record.
  * The response includes an id the vault can use for the password update.
  * The password field is not returned.


**Run a manual rotation**
  * Trigger a **manual** password rotation in the vault for the onboarded DLS account.
  * Confirm the vault reports success.
  * Sign out of the DLS Web UI (if signed in).
  * Sign in with the local username and the **new** password from the vault.
  * Confirm that the previous password no longer works.


**Confirm audit visibility**
  * Check DLS audit / event logs for a SCIM password-set event (wording may vary by release).
  * Correlate the timestamp with the vault rotation log.
  * Confirm passwords are not written in clear text to DLS logs.


**Enable the schedule**
After a successful manual rotation:
  * Enable the vault’s scheduled rotation policy for the account.
  * Document the break-glass process for retrieving the current password from the vault if interactive login is needed during an incident.
  * Document bearer-token replacement procedures for your operations team.


**What to do next**
If verification fails, see [Troubleshooting SCIM password rotation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-scim-password-rotation). 
###  [2.8.6.5.3.6. Troubleshooting SCIM Password Rotation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-scim-password-rotation)
Use these topics when Enterprise Password Vault rotation against DLS does not work as expected. 
Also see [**Password Management**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#password-management) and [Troubleshooting Authentication and Authorization Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-authentication-and-authorization-issues "Common Issues and Resolutions") for authentication troubleshooting topics in the published User Guide.
###  [2.8.7. Configuring LDAP Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-authentication)
You can use a lightweight directory access protocol (LDAP) directory instead of the configured accounts for managing user access to a DLS appliance. When integrated with an LDAP server, the DLS appliance uses Windows Challenge/Response, formerly NT (New Technology) LAN Manager, (NTLM) for authenticating users. LDAP here is for the DLS Web UI, not licensed-client checkout.
The dls_diagnostics and dls_system users have been replaced with the default user, which is the dls_admin user. For the initial installation of DLS and prior to registration on the UI, the default password for the dls_admin user is `welcome`
. Once registration on the portal is done, the same password and username can be used to login to the VM Appliance from the hypervisor console. 
  * Since a new user role is used in DLS 3.X, the password for dls_admin must be reset in order to login to the CLI of the DLS Appliance following a migration from DLS 2.X and older.
  * The dls_admin user has the same capabilities as that of dls_system and dls_diagnostics. In other words, all are able to configure appliance network properties, enable the `sudo` user, configure disk expansion, view log files, configuration files, etc.
  * The rsu_admin user role would be used for an in-place upgrade, applying security patches, and the execution of NVIDIA-provided scripts. This is true unless it is used for OS-based package updates; in that case, there is no concern for DLS support.


If you want to enable secure LDAP (LDAPS), ensure that the following prerequisites are met: 
  * You have obtained the required certificate file in the correct format.
For a TLS certificate, the authentication certificate is the Root CA certificate in Base-64 encoded X.509 (.cer) format.
  * The certificate file has the correct name.

  
| Certificate File  | File Name  |  
| --- | --- |  
|  TLS certificate (The Root CA Server Certificate in Base-64 encoded X.509 format)  | A certificate with an extension .cer  |  
Note:
DLS Appliance LDAP uses direct binding (user credential bind) approach for authentication. 
LDAP (Lightweight Directory Access Protocol) integration enables DLS to authenticate users against enterprise directory services such as: 
  * Microsoft Active Directory


**Benefits of LDAP Integration** : 
  * Centralized user authentication
  * Automatic role assignment based on group membership
  * Simplified user lifecycle management
  * Compliance with enterprise security policies


**Configuring LDAP Settings**
LDAP authentication can be configured through either the DLS Web UI or programmatically via RESTful API. Both methods provide the same configuration capabilities. 
###  [2.8.7.1. Configuring LDAP via DLS Web UI](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-settings)
LDAP authentication can be configured through either the DLS Web UI or programmatically via RESTful API. Both methods provide the same configuration capabilities. **Access LDAP Configuration**
  1. Sign in to the DLS Web UI as an administrator.
  2. Select Operations > Settings > Security. In the on-page navigation, select Authentication.
  3. Select Add identity provider, then select LDAP.
  4. Enter a Display name, then enter the LDAP connection and directory-search settings supplied by your directory administrator.


**Configure LDAP Connection**
  1. **LDAP Server** : Enter hostname or IP address of your LDAP server
  2. **Port** : Enter LDAP port (389 for LDAP, 636 for LDAPS)
  3. **Use Secure Connection (LDAPS)** : Enable for encrypted communication
  4. **Authentication Type** : Select NTLM or SASL
  5. **Domain** : Enter domain name (required for NTLM/Active Directory)


**Configure Search Settings**
  1. **Root DN** : Enter root distinguished name (e.g., “dc=example,dc=com”)
  2. **Search Base** : Enter base DN for user searches (e.g., “CN=Users,DC=example,DC=com”)
  3. **Search Filter** : Enter the LDAP query.
  4. **Username Attribute** : Enter attribute name.
  5. **Follow Referrals** : Enable if your LDAP server uses referrals


**Upload CA Certificate (for LDAPS)**
  * If using secure connection, upload your LDAP server’s root CA certificate in PEM format


**Configure Group-to-Role Mappings** (Optional):: 
  1. To check the connection, select Test LDAP connection. Enter the LDAP username and password in the test dialog, select Test LDAP connection, and review the result.
  2. Under Enable for sign-in, leave Enable this provider when saving off while you finish configuring the provider, or turn it on to enable the provider as part of this save.
  3. Select Add identity provider.
  4. Enable this provider when saving is off by default. A provider saved with this option off remains disabled. If enabling it changes the active protocol, review the warning and the list of providers that will remain enabled before saving.
  5. In the Identity providers table, open the LDAP provider's action menu and select Map groups to roles:
  6. To restrict LDAP access by group membership, turn on Enable group based access. Enter the full LDAP group DN in the field for each required DLS role. At least one group mapping is required when this option is on.
  7. Select Save mapping. 
With no user or group role mappings configured, authenticated LDAP users receive Administrator access. When mappings are configured, a user who matches no mapping is denied. Turning off Enable group based access removes the group mappings; any separately configured user mappings still apply.
  8. To enable a saved provider for the current sign-in protocol, open its action menu, select Enable, and confirm with Enable. To disable it, select Disable and confirm with Disable.
  9. To switch protocols, select Change sign-in method. Select the required protocol, choose the providers to enable, and review the effect on the other protocols. Select Switch to LDAP, Switch to SAML, or Switch to OIDC, as appropriate. 
Only one of LDAP, SAML, or OIDC can have enabled providers at a time. Providers that you leave unchecked remain disabled. Local password sign-in remains available for emergency access through the current Web UI.


Security 
  1. Select Operations > Settings > Security > Authentication.
  2. Select Add identity provider, then select SAML.
  3. Enter a Display name and configure the identity provider using a metadata URL, metadata content, or manual details.
  4. Leave Enable this provider when saving off and select Add identity provider. Complete the identity-provider registration and role mappings before enabling the saved provider.
  5. In the Identity providers table, open the provider's action menu and select Map groups to roles. Enter the groups attribute or claim and the required group-to-role mappings, then select Save mapping.
  6. To enable a saved provider for the current sign-in protocol, open its action menu, select Enable, and confirm with Enable. To disable it, select Disable and confirm with Disable.
  7. To switch protocols, select Change sign-in method. Select the required protocol, choose the providers to enable, and review the effect on the other protocols. Select Switch to LDAP, Switch to SAML, or Switch to OIDC, as appropriate.
  8. In the Identity providers table, open the provider's action menu and select Map groups to roles. Enter the groups attribute or claim and the required group-to-role mappings, then select Save mapping.
  9. To enable a saved provider for the current sign-in protocol, open its action menu, select Enable, and confirm with Enable. To disable it, select Disable and confirm with Disable.
  10. To switch protocols, select Change sign-in method. Select the required protocol, choose the providers to enable, and review the effect on the other protocols. Select Switch to LDAP, Switch to SAML, or Switch to OIDC, as appropriate. 
Only one of LDAP, SAML, or OIDC can have enabled providers at a time. Providers that you leave unchecked remain disabled. Local password sign-in remains available for emergency access through the current Web UI.


###  [2.8.7.2. Mapping LDAP Groups to DLS Roles](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#mapping-ldap-groups-to-dls-roles)
**Overview** : 
Group-to-role mapping enables automatic role assignment based on LDAP group membership. When a user authenticates via LDAP, DLS: 
  1. Retrieves the user’s LDAP group memberships
  2. Checks if any groups are mapped to DLS roles
  3. Assigns the appropriate role based on mapping configuration


**Configuration**
Add the _ldap_group_role_mapping_ object to your LDAP configuration request: 
Copy
Copied!

```
            
            
{
  "ldap_group_role_mapping": {
	"CN=LicenseAdmins,OU=Groups,DC=example,DC=com": "DLS_ADMIN",
	"CN=LicenseOperators,OU=Groups,DC=example,DC=com": "DLS_OPERATOR",
	"CN=LicenseUsers,OU=Groups,DC=example,DC=com": "DLS_USER"
  }
}

        
```

**Key Points** : 
  * **Group DN Format** : Use the full Distinguished Name (DN) of the LDAP group
  * **Case Sensitivity** : Group DNs are case-sensitive; ensure exact match with your LDAP directory
  * **Role Values** : Must be one of: “DLS_ADMIN”, “DLS_OPERATOR”, “DLS_USER”


**Complete Example** : 
Copy
Copied!

```
            
            
curl -X POST 'https://192.0.2.10/auth/v1/ldap' \
  --header 'x-api-key: 0ae280b9-58e2-809c-3c24-4069c5bd40c2' \
  --header 'Content-Type: application/json' \
  --data-raw '{
	"enable_ldap": "true",
	"ldap_server": "192.0.2.50",
	"ldap_port": 389,
	"use_secure": "false",
	"authentication_type": "NTLM",
	"domain": "example.com",
	"root_dn": "dc=example,dc=com",
	"search_base": "CN=Users,DC=example,DC=com",
	"search_filter": "(&(objectCategory=Person)(userPrincipalName=*))",
	"username_attribute": "sAMAccountName",
	"search_scope": "SUBTREE",
	"ldap_group_role_mapping": {
  	"CN=DLS-Admins,OU=Security Groups,DC=example,DC=com": "DLS_ADMIN",
  	"CN=DLS-Operators,OU=Security Groups,DC=example,DC=com": "DLS_OPERATOR",
  	"CN=DLS-Users,OU=Security Groups,DC=example,DC=com": "DLS_USER"
	}
  }'

        
```

**Behavior Scenarios** : 
**Scenario 1: No Group Mapping Configured**
If _ldap_group_role_mapping_ is empty or not provided: 
  * All authenticated LDAP users receive **DLS_ADMIN** role by default
  * Use case: Quick LDAP setup or environments where all LDAP users should have admin access


**Scenario 2: Partial Group Mapping**
If only some groups are mapped: 
  * Only users in mapped groups can log in
  * Users in unmapped groups are denied access to the DLS Web UI portal.
  * Use case: Controlled access where only specific teams should access DLS


**Scenario 3: Multiple Group Membership**
If a user belongs to multiple groups with different role mappings: 
  * User receives the role with **highest privilege** (lowest privilege_level number)
  * Example: User in both “DLS-Admins” (DLS_ADMIN, level 1) and “DLS-Users” (DLS_USER, level 3) receives DLS_ADMIN role
  * Use case: Users with overlapping responsibilities


**Role Determination Logic** : 
Copy
Copied!

```
            
            
Step 1: Check group-to-role mapping
	↓
	If user belongs to one or more mapped groups:
  	→ Assign role with highest privilege (lowest privilege_level number)
	↓
	If user belongs to no mapped groups:
  	↓
  	Check if any mappings are configured:
    	→ If NO mappings: Assign DLS_ADMIN (default)
    	→ If mappings exist: Access Denied

        
```

**Example Scenarios** : 
**Scenario 1: Multiple Group Membership with Highest Privilege** User: jane.smith LDAP Groups: [CN=LicenseAdmins,DC=example,DC=com, CN=LicenseUsers,DC=example,DC=com] 
Group Mapping: 
CN=LicenseAdmins,... → DLS_ADMIN (privilege_level=1) 
CN=LicenseUsers,... → DLS_USER (privilege_level=3) 
Result: DLS_ADMIN (highest privilege = lowest level number) 
**Scenario 2: No Mappings Configured**
User: new.user 
ldap_group_role_mapping: {} (empty) 
Result: DLS_ADMIN (default when no mappings exist) 
**Scenario 3: Access Denied**
User: unknown.user 
LDAP Groups: [CN=Some_Other_Group,DC=example,DC=com] 
User Mapping: (no match) 
Group Mapping: CN=LicenseAdmins,... → DLS_ADMIN (no match) 
Result: Access Denied (403 Forbidden error) 
###  [2.8.7.3. Testing LDAP Configuration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#testing-ldap-configuration)
**Testing Before Saving Configuration** :: 
Use the _test_ldap_ parameter to validate LDAP connectivity before saving configuration: 
Copy
Copied!

```
            
            
{
  "test_ldap": true,
  "test_ldap_creds": {
	"username": "testuser",
	"password": "TestP@ssw0rd123"
  }
}

        
```

**What Gets Tested** : 
  * LDAP server connectivity
  * Search base validity
  * Search filter correctness
  * Username attribute mapping
  * Authentication mechanism (NTLM/SASL)
  * Group retrieval (if configured)
  * Role mapping logic


**Success Response** (HTTP 200): 
Copy
Copied!

```
            
            
{
  "message": "LDAP configuration updated successfully",
  "enable_ldap": true,
  "test_result": {
	"status": "Success",
	"username": "testuser",
	"ldap_role": "DLS_OPERATOR",
	"groups": [
  	"CN=LicenseOperators,DC=example,DC=com"
	]
  }
}

        
```

**Error Response** (HTTP 400): 
Copy
Copied!

```
            
            
{
  "error": "Bad Request",
  "message": "LDAP connection test failed: Unable to connect to LDAP server at 192.0.2.50:389"
}

        
```

**Testing After Configuration Saved** : 
After saving LDAP configuration, test authentication: 
  1. **Attempt Login** : 
     * Navigate to DLS Web UI login page
     * Enter LDAP username and password
     * Click **Login**
  2. **Verify Successful Login** : 
     * User is redirected to DLS Dashboard
     * User’s role is displayed in user profile
     * User can access features appropriate to their assigned role
  3. **Check Audit Logs** : 
     * Navigate to **Events** in DLS Web UI
     * Filter by “Login” events
     * Verify LDAP authentication events are logged


###  [2.8.7.4. Retrieving LDAP Configuration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#retrieving-ldap-configuration)
To view the current LDAP configuration: 
  1. Log in to DLS Web UI with DLS_ADMIN role
  2. Navigate to **Settings** page
  3. Locate the **LDAP Configuration** section
  4. View all configured settings including: 
     * Server connection details
     * Search configuration
     * Group-to-role mappings
  5. Sensitive fields (passwords, certificates) are masked for security


Note:
Sensitive fields like bind passwords and _ldap_tls_ca_certs_ are not returned in the response for security reasons. 
**To Completely Remove LDAP Configuration** : 
  1. Navigate to **Settings** page
  2. Locate the **LDAP Configuration** section
  3. Click **Delete LDAP Configuration** or **Remove**
  4. Confirm deletion
  5. All LDAP settings and group mappings are removed


###  [2.8.7.5. About Multi-Domain LDAP on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-multi-domain-ldap-on-dls-virtual-appliance)
Perform this reading task before you configure LDAP for more than one domain or more than one directory on DLS. 
**What is multi-domain LDAP on DLS?**
Organizations often have users in more than one Active Directory **domain** , or in more than one **directory** altogether. DLS can authenticate those users for the **management Web UI** in two different ways, depending on how your directories are set up.   
| Term  | Plain-language meaning  |  
| --- | --- |  
| Domain  | A security boundary in Active Directory (for example corp.example.com).  |  
| Forest  | A set of domains that trust each other and share one Active Directory “tree of trees.”  |  
| LDAP directory  | The server or service that stores user accounts (Active Directory, OpenLDAP, and similar).  |  
| Identity source  | One LDAP directory configuration on DLS (servers, search settings, certificates, and role mappings).  |  
| DLS Web UI  | The management interface for the DLS appliance or containerized DLS instance.  |  
LDAP authentication does not change license checkout. 
**Select the correct situation for your needs**  
| Your environment  | What to configure on DLS  | Why  |  
| --- | --- | --- |  
| One Active Directory forest with a root domain and child domains (users in several domains, but one company forest).  | One LDAP configuration using the Global Catalog (ports 3268 / 3269) and the forest-root search base.  | The Global Catalog can see users across domains in that forest.  |  
| Separate directories — for example two AD forests after a merger, or Active Directory plus OpenLDAP  | Multiple LDAP identity sources — one per directory.  | Each directory has its own servers, search settings, and group mappings.  |  
| Several domain controllers for the same directory.  | Still one identity source, with multiple server hostnames for failover.  | Backup DCs are not a second directory.  |  
Note:
  * Do not add a second identity source only because you have child domains in one forest. Use Global Catalog settings instead. See [**Configuring LDAP for One Forest with Multiple Domains**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-for-one-forest-with-multiple-domains).
  * Do not put hosts from two different forests into one identity source. Create two identity sources. See [**Configuring Multiple LDAP Identity Sources**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-multiple-ldap-identity-sources).


**DLS does:**
  * Bind to your directory with the user’s credentials (or the authentication type you configure).
  * Search for the user in the configured search base.
  * Map LDAP groups (and optional user overrides) to DLS roles: Administrator, Operator, or User.
  * When multiple identity sources exist, DLS uses enabled LDAP identity sources in priority order for a plain username. Use`DOMAIN\username` or `user@domain` to select a directory explicitly.


**DLS does not:**
  * Replace your Active Directory or LDAP servers.
  * Automatically merge two forests into one login without domain-qualified usernames (when names could overlap).
  * Use Kerberos/SASL across **multiple** identity sources (use **NTLM** for Active Directory when more than one source is configured).


**How login works when you have more than one directory**  
| How the user signs in  | Which directory DLS uses  |  
| --- | --- |  
| Plain username (for example jsmith)  | Enabled LDAP identity sources in priority order.  |  
| CORP\jsmith  | The source whose domain / alias matches CORP  |  
| jsmith@corp.example.com  | The source whose DNS domain matches  |  
**Best practice:** Use `<DOMAIN>\<username>` or `<username>@<domain>` to select a directory explicitly, especially when usernames overlap across directories. 
**How this relates to local passwords and SSO**  
| Method  | Purpose  |  
| --- | --- |  
| Local DLS users  | Built-in accounts on the appliance (including emergency administrator access).  |  
| LDAP  | Sign in against one or more corporate directories.  |  
**What to do next**
  1. Review [**Before You Configure Multi-Domain LDAP**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-multi-domain-ldap).
  2. If you have **one forest** with child domains, see [**Configuring LDAP for One Forest with Multiple Domains**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-for-one-forest-with-multiple-domains).
  3. If you have **separate directories** , see [**Configuring Multiple LDAP Identity Sources**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-multiple-ldap-identity-sources).
  4. Set [**Mapping LDAP Groups to DLS Roles**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/topics/mapping-ldap-groups-dls-roles.html) for each directory.
  5. Review [**Signing In with LDAP**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#signing-in-with-ldap).


###  [2.8.7.6. Before You Configure Multi-Domain LDAP](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-multi-domain-ldap)
Ensure that the following requirements are met before you configure LDAP for multiple domains or multiple identity sources on a DLS virtual appliance or containerized DLS instance. 
**Who should perform these tasks?**
  * A DLS administrator who can change authentication settings on the appliance.
  * A directory / Active Directory administrator who knows domain names, Global Catalog hosts, and group Distinguished Names (DNs).
  * Optionally, a network administrator for firewall rules between DLS and each LDAP host.


**General requirements**
  1. The DLS management Web UI is reachable from your administrators’ browsers over **HTTPS**.
  2. You know whether you have **one Active Directory forest** or **separate directories** (see **About Multi-Domain LDAP on a DLS Virtual Appliance**).
  3. Network connectivity exists from the DLS appliance to every LDAP or Global Catalog host and port you will use.
  4. You have DLS **Administrator** rights (or equivalent permission to configure LDAP).
  5. For API configuration, you have an Enterprise-scoped API key from the NVIDIA Licensing Portal, if your organization uses the REST API.


**Secure LDAP (LDAPS)**
If you enable a secure connection:
  1. Use the correct port (commonly **636** for LDAPS, or **3269** for Global Catalog over TLS).
  2. Upload the **CA certificate** that issues your directory’s TLS certificate.
  3. When you use **multiple identity sources** , upload a CA certificate **per source** if each directory uses a different trust chain.


**Information to collect from your directory team**
**For one forest with child domains**
  * Forest root DNS name and forest-root Distinguished Name (for example DC=corp,DC=example,DC=com).
  * Hostname of a Global Catalog server.
  * Whether to use LDAP (**3268**) or LDAPS (**3269**) to the Global Catalog.
  * Group DNs used for DLS Administrator, Operator, and User access.
  * Approximate number of users in scope (so you can set a search page limit if the directory is large).


**For multiple identity sources**
For **each** directory, collect: 
  * Display name (admin-friendly label).
  * Domain / NetBIOS alias and DNS domain (for login routing).
  * One or more LDAP hostnames (ordered for failover).
  * Port, TLS, search base, search filter, search scope.
  * Group-to-role mappings for that directory.
  * Test account credentials for connection testing.


Decide which source is the **default** for plain-username login (for example jsmith with no domain prefix). 
**Upgrade note**
If the appliance already has a single LDAP configuration, upgrading to a release that supports multiple identity sources **migrates** that configuration automatically into one default identity source. Existing logins continue to work. You can add more sources afterward without replacing the migrated settings. 
**What to do next**
  * For one forest with child domains, see [**Configuring LDAP for One Forest with Multiple Domains**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-for-one-forest-with-multiple-domains).
  * For separate directories, see [**Configuring Multiple LDAP Identity Sources**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-multiple-ldap-identity-sources).


###  [2.8.7.7. Configuring Multiple LDAP Identity Sources](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-multiple-ldap-identity-sources)
If you only have child domains in one forest, skip thi topic and procede to [ Configuring LDAP for One Forest with Multiple Domains](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-for-one-forest-with-multiple-domains)
Perform this task when DLS must authenticate users against **more than one LDAP directory** — for example two Active Directory forests 
Before you begin, complete [**Before You Configure Multi-Domain LDAP**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-multi-domain-ldap) and [**About Multi-Domain LDAP on a DLS Virtual Appliance**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-multi-domain-ldap-on-dls-virtual-appliance). 
Note:
If you have **one** Active Directory forest with child domains, do **not** use this procedure. Use **Configuring LDAP for One Forest with Multiple Domains** instead. 
**Setup Overview**
  1. Confirm that each directory needs its own identity source..
  2. Add the first (or default) identity source and test it.
  3. Add additional identity sources for the other directories.
  4. Map groups to DLS roles **per source**.


LDAP users enter their username and password in the standard DLS sign-in form. There is no LDAP provider selector. For a plain username, DLS uses enabled LDAP sources in priority order. To target a directory explicitly, enter <DOMAIN>\<username> or <username>@<domain>. Use the domain or alias configured for that directory. SAML and OIDC sign-in instead use the separate Sign in with <provider> buttons.
  1. Save the identity source.
  2. Repeat for each additional directory.


Verify sign-in with a plain username and confirm the assigned role. Then verify each directory with a domain-qualified username. Use domain-qualified usernames when the same username exists in more than one directory. 
  1. Multi-source Active Directory login requires NTLM.
  2. Up to 10 hostnames per identity source (API limit).Enter connection settings
  3. Overlapping plain usernames across forests require domain-qualified login.
  4. Optionally run **Test connection** with a valid directory user before saving.
  5. Save the identity source.
  6. Repeat for each additional directory.


To make a source handle plain usernames, select **Set as default** (during create or afterward). 
**Edit or delete an identity source**
  * **Edit** — Changes apply only to that source. Other sources are unchanged.
  * **Delete** — Removes one source. Deleting the **last** source disables LDAP authentication on the appliance.


**Map roles and verify**
  1. Configure group (and optional user) mappings for **each** source. See **Mapping LDAP Groups to DLS Roles**.
  2. Sign out and sign in with a user from the default directory using a plain username.
  3. Sign in with a user from a second directory using `DOMAIN\username` or `user@domain`.
  4. Confirm each user receives the expected DLS role.


If verification fails, see **Troubleshooting Multi-Domain LDAP**. 
**Configuring via the RESTful API (optional)**
Administrators who automate configuration can use the identity-sources API under`/auth/v1/identity-sources` (list, create, update, delete, set default, test, and policy). These operations require LDAP configuration permission and, for programmatic access, an Enterprise API key. 
The legacy`/auth/v1/ldap` endpoints remain for compatibility and operate on the **default** identity source only; they do not remove sibling sources. 
**Limitations**
  * Multi-source Active Directory login requires **NTLM**.
  * Up to **10** hostnames per identity source (API limit).
  * Overlapping plain usernames across forests require **domain-qualified** login.


**What to do next**
  * Document required login formats for each user population. See [**Signing In with LDAP**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#signing-in-with-ldap).
  * Keep a local Administrator break-glass account available until multi-directory login is proven in your environment.


###  [2.8.7.8. Configuring LDAP for One Forest with Multiple Domains](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-for-one-forest-with-multiple-domains)
Perform this task when your Active Directory has **one forest** with a root domain and one or more **child domains** , and you want users from those domains to sign in to the DLS Web UI with one LDAP configuration. 
Before you begin, complete [**Before You Configure Multi-Domain LDAP**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-multi-domain-ldap). 
Note:
This is **not** the same as configuring multiple LDAP identity sources. Child domains in one forest usually need **Global Catalog** settings on a **single** LDAP configuration. Use [**Configuring Multiple LDAP Identity Sources**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-multiple-ldap-identity-sources) only for **separate** forests or different directory products. 
**Why use the Global Catalog?**  
| Connection  | What it can see  |  
| --- | --- |  
| Domain controller on port 389 / 636  | Mostly users in that domain. Users in child domains may not be found.  |  
| Global Catalog on port 3268 / 3269  | A partial view of every domain in the forest. One search can find users in the root and child domains  |  
For forest-wide login, connect DLS to a Global Catalog host, set the search base to the **forest root** , and use **SUBTREE** scope. 
**Recommended settings**  
| Setting  | Recommendation  |  
| --- | --- |  
| Server / host  | Hostname of a Global Catalog server (or forest-root host that provides GC).  |  
| Port  | 3268 (LDAP) or 3269 (LDAPS).  |  
| Search base  | Forest-root Distinguished Name (for example DC=corp,DC=example,DC=com).  |  
| Search scope  | SUBTREE.  |  
| Follow referrals  | Enabled (helpful; primary coverage still comes from using the Global Catalog).  |  
| Group-to-role mapping  | Full group Distinguished Names (DNs).  |  
**Procedure (Web UI)**
  1. Sign in to the DLS Web UI as an Administrator.
  2. Open **Settings** → **LDAP configuration** (same page as the existing single-directory procedure in the User Guide).
  3. Set **LDAP Server** to a Global Catalog host.
  4. Set **Port** to **3268** or **3269**
  5. Enable secure connection if you use **3269** , and upload the CA certificate.
  6. Set **Search base** to the forest-root DN.
  7. Set **Search scope** to **SUBTREE**.
  8. Enable **Follow referrals**.
  9. Configure the search filter so it matches the users who should access DLS. Include memberOf for the groups used for DLS access when your directory team recommends it.
  10. Map LDAP groups to DLS roles using full group DNs. See **Mapping LDAP Groups to DLS Roles**.
  11. Test with a user from the **root** domain and a user from a **child** domain, then save.


**Search filter guidance**
Prefer filters that include the groups allowed to use DLS. Example pattern (replace the group DN with your own): 
Copy
Copied!

```
            
            
(&(objectCategory=Person)(sAMAccountName=*)(memberOf=CN=YourGroup,OU=Groups,DC=corp,DC=example,DC=com))

        
```

Note:
In Active Directory, memberOf lists **direct** membership. Nested group membership may not appear unless your directory team accounts for nesting in the filter or group design. 
**Checklist**
  1. Port is **3268** or **3269** (Global Catalog).
  2. Search base is the **forest root** DN.
  3. Search scope is **SUBTREE**.
  4. Follow referrals is enabled.
  5. Group DNs in role mapping match the directory exactly
  6. Page limit is set when the user population is large.
  7. Test logins succeed for root-domain and child-domain users.


**What to do next**
  * If you later need a **second forest** or OpenLDAP alongside AD, add a second identity source. See [**Configuring Multiple LDAP Identity Sources**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-multiple-ldap-identity-sources).
  * Review [**Signing In with LDAP**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#signing-in-with-ldap) for username formats.


###  [2.8.7.9. Signing In with LDAP](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#signing-in-with-ldap)
Perform this task after LDAP authentication is configured on the DLS virtual appliance. 
This topic supplements [**Logging in to a DLS Virtual Appliance**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-virtual-appliance "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance.") in the DLS User Guide. 
**Before you log in**
  * Your administrator has enabled LDAP and mapped your directory groups to a DLS role.
  * You know whether your organization uses one directory or multiple identity sources.
  * You use the DLS management URL provided by your administrator.


**Sign in with a plain username**
  1. In a supported web browser, go to the DLS management interface.
  2. Enter your directory username without a domain prefix (for example jsmith).
  3. Enter your directory password.
  4. Submit the form.


DLS uses enabled LDAP identity sources in priority order for a plain username. Use a domain-qualified username to select a directory explicitly. Invalid credentials or a role-mapping access denial stop the sign-in attempt; DLS does not treat them as a reason to try another source. 
**Sign in with a domain-qualified username**
Use these formats when multiple LDAP identity sources are configured, or when your administrator requires them.   
| Format  | Example  | When to use it  |  
| --- | --- | --- |  
| Domain backslash  | PARTNER\jsmith  | Common for Active Directory NetBIOS / alias names.  |  
| UPN (email-style)  | jsmith@partner.example.com  | Common when your DNS domain is well known.  |  
  1. On the DLS login page, enter the domain-qualified username exactly as your administrator documented it.
  2. Enter your password for **that** directory.
  3. Submit the form.


DLS selects the identity source that matches the domain portion of the username. 
**What happens if the password is wrong?**
Login fails immediately for the directory DLS selected. DLS does **not** try another identity source or another LDAP hostname after invalid credentials. 
**After you are signed in**
  * Your permissions follow the DLS role from LDAP group mapping: Administrator, Operator, or User.
  * Day-to-day pages behave the same as for locally authenticated users with that role.
  * Sign out using the Web UI sign-out control when you finish.


**What to do next**
If sign-in fails, contact your DLS administrator and see [**Troubleshooting Multi-Domain LDAP**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-multi-domain-ldap). 
###  [2.8.8. Configuring Single Sign-On (SSO) Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-sso-authentication)
Single Sign-On allows administrators and operators to open the DLS Web UI by signing in with your **corporate identity provider** , instead of (or in addition to) a password stored on the DLS appliance.
###  [2.8.8.1. About Single Sign-On on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-sso-dls-virtual-appliance)
Perform this reading task before you configure Single Sign-On (SSO) for the DLS management interface. See [Password Management](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#password-management), [Configuring LDAP Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-authentication "You can use a lightweight directory access protocol \(LDAP\) directory instead of the configured accounts for managing user access to a DLS appliance. When integrated with an LDAP server, the DLS appliance uses Windows Challenge/Response, formerly NT \(New Technology\) LAN Manager, \(NTLM\) for authenticating users. LDAP here is for the DLS Web UI, not licensed-client checkout."), and [Configuring Single Sign-On (SSO) Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-sso-authentication "Single Sign-On allows administrators and operators to open the DLS Web UI by signing in with your corporate identity provider, instead of \(or in addition to\) a password stored on the DLS appliance."). 
**What is Single Sign-On on DLS?**
Single Sign-On allows administrators and operators to open the DLS Web UI by signing in with your **corporate identity provider** , instead of (or in addition to) a password stored on the DLS appliance.   
| Term  | Plain-language meaning  |  
| --- | --- |  
| Identity provider (IdP)  | Your company’s login system (for example Microsoft Entra ID, ADFS, Okta, Ping, or Keycloak).  |  
| SAML  | A widely used enterprise standard for browser sign-in between applications and an identity provider.  |  
| OIDC (OpenID Connect)  | A modern sign-in standard built on OAuth 2.0, often used with cloud identity providers.  |  
| DLS Web UI  | The management interface for the DLS appliance or containerized DLS instance  |  
Licensed clients (for example vGPU clients) continue to obtain licenses as they do today. SSO does **not** change how licensed clients check out licenses. 
**What DLS does and does not do**
**DLS does:**
  * Redirect the browser to your identity provider when the user chooses an SSO sign-in option.
  * Accept a successful sign-in result from that provider.
  * Map the signed-in user (and their groups, if configured) to an existing DLS role: Administrator, Operator, or User.
  * Create the normal DLS Web UI session after a successful mapping.


**DLS does not:**
  * Replace your identity provider.
  * Implement its own multi-factor authentication (MFA), smart card, or conditional-access policies. Those remain policies on your identity provider.
  * Support starting login from the identity provider alone (users must start from the DLS login page).


**SAML or OIDC — which should you choose?**  
| Option  | Typical when  |  
| --- | --- |  
| SAML  | Your organization already federates applications with SAML (common with ADFS and many enterprise IdPs).  |  
| OIDC  | Your identity provider offers an OpenID Connect application registration (common with Entra ID, Okta, and similar).  |  
Ask your identity team which standard they prefer for new applications. DLS supports both; you normally enable **one** of them for the appliance. 
**How SSO relates to LDAP, SCIM, and local passwords**  
| Method  | Purpose  |  
| --- | --- |  
| Local DLS users  | Built-in accounts on the appliance (including emergency administrator access).  |  
| LDAP  | Sign in against a corporate directory over LDAP/LDAPS  |  
| SCIM password rotation  | Lets an Enterprise Password Vault update passwords for existing local users. SCIM is not a Web UI sign-in method.  |  
| SSO (SAML or OIDC)  | Sign in through a corporate identity provider in the browser.  |  
Note:
  * Only one identity-source protocol may be enabled at a time: LDAP or SAML or OIDC. You can keep other sources configured but disabled while you prepare a cutover.
  * Your administrators can leave local password login available as break-glass access if the identity provider is unreachable. NVIDIA recommends keeping emergency access available until SSO is proven in your environment.


**What to do next**
  1. Review [Before You Configure Single Sign-On](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-single-sign-on).
  2. Configure [Configuring SAML Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-saml-authentication) or [Configuring OpenID Connect (OIDC) Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-oidc-authentication).
  3. Set [Mapping Identity Provider Groups to DLS Roles](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#mapping-identity-provider-groups-to-dls-roles).
  4. Verify [Logging in with Single Sign-On](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-with-sso).


###  [2.8.8.2. Before You Configure Single Sign-On](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-single-sign-on)
Ensure that the following requirements are met before you configure SAML or OpenID Connect (OIDC) authentication on a DLS virtual appliance or containerized DLS instance. 
**Who should perform these tasks?**
  * A DLS administrator who can change authentication settings on the appliance.
  * An identity administrator who can create an application (relying party / client) in your corporate identity provider.
  * Optionally, a network or platform administrator for DNS and firewall rules.


**General requirements**
  1. The DLS management Web UI is reachable from your administrators’ browsers over **HTTPS**.
  2. You know the **stable public address** that browsers use for DLS (host name preferred). Users and the identity provider must use the same address that DLS advertises for sign-in return URLs.
  3. Clocks on the DLS host and the identity provider are synchronized with a reliable time source (for example NTP). Large time differences can cause sign-in to fail.
  4. You have decided whether to use **SAML** or **OIDC** for this appliance.
  5. You understand that enabling SAML or OIDC identity sources means LDAP identity sources must not be enabled at the same time (see protocol exclusivity below).


**Network and connectivity**  
| Direction  | Required for  | Notes  |  
| --- | --- | --- |  
| Browser → DLS  | Always  | Users open the DLS login page and return after IdP sign-in.  |  
| Browser → identity provider  | Always  | Users enter credentials (and MFA) at the IdP.  |  
| DLS → identity provider  | SAML: required for metadata download. OIDC: required.  | For SAML, DLS needs outbound connectivity when it downloads identity-provider metadata. Pasted metadata or manual SAML details avoid that download. For OIDC, DLS must reach the provider's discovery, token, and signing-key endpoints. During sign-in, DLS exchanges the authorization code for tokens and validates the ID token. Browser connectivity alone is insufficient.  |  
Note:
The browser carries the SAML response or OIDC authorization response back to DLS. OIDC also requires a server-to-server token exchange and access to provider metadata and signing keys. Manual SAML configuration does not provide an alternative to OIDC server connectivity. 
**Stable public address**
DLS must advertise consistent URLs to your identity provider, including:
  * Where the identity provider should send the user after sign-in (assertion / callback URL).
  * The DLS application identifier (entity ID / audience), when applicable.


**Ensure that:**
  1. Administrators always open DLS with the same host name (or address) you register with the identity provider.
  2. Any load balancer or ingress in front of DLS preserves HTTPS and the public host name that users see.


If your deployment requires an explicit public base URL setting for SSO, set it to the HTTPS address users type in the browser (no trailing slash), for example **https://dls.example.com**. 
**Protocol exclusivity**
At most one of the following may have **enabled** identity sources at a time:
  * LDAP
  * SAML
  * OIDC


You may keep disabled draft configurations for a future cutover. 
Local DLS password login can remain available for emergency access when your organization allows it. 
**Information to collect from your identity team**
**For SAML**
  * Whether you will use a metadata URL, a metadata file, or manual IdP details.
  * IdP entity ID (issuer), single sign-on URL, and signing certificate (if not using metadata).
  * How usernames and groups appear in the SAML response (attribute names and example values).
  * Which AD / IdP groups should map to DLS Administrator, Operator, and User.


**For OIDC**
  * Issuer URL (discovery base).
  * Client ID and client secret (if the client is confidential).
  * Redirect / callback URL that DLS will use (your identity team registers this on the IdP).
  * Which token claims carry username and groups.
  * Which groups should map to DLS roles.


**Multi-factor authentication (MFA)**
If your organization requires MFA, enable it on the **identity provider**. After MFA succeeds, DLS only sees a successful SSO completion. No MFA secrets are stored on DLS. 
**What to do next**
  * To configure SAML, see [Configuring SAML Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-saml-authentication).
  * To configure OIDC, see [Configuring OpenID Connect (OIDC) Authentication.](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-oidc-authentication)


###  [2.8.8.3. **Configuring SAML Authentication**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-saml-authentication)
Perform this task to allow users to sign in to the DLS Web UI through a SAML 2.0 identity provider. Before you begin, complete **Before You Configure Single Sign-On** and **About Single Sign-On on a DLS Virtual Appliance**. 
**Setup Overview**
  1. Create a SAML application (relying party) for DLS in your identity provider.
  2. Configure a SAML identity source on DLS with your IdP details.
  3. Register DLS’s service provider information (entity ID and return URL) with the identity provider.
  4. Map groups or users to DLS roles.
  5. Enable the SAML identity source and verify sign-in.


Note:
Users must start sign-in from the DLS login page. Starting only from the identity provider portal is not supported. 
**Create the application in your identity provider**
In your identity provider’s administration console, create a **SAML** application or relying party trust for DLS. 
Typical choices:
  * Use a claims-aware / SAML web application profile (not a legacy non-claims profile).
  * Prefer importing DLS metadata from a file or entering identifiers manually if your IdP cannot consume DLS’s metadata URL format.
  * Allow access for the user groups that should use DLS (for example Permit everyone in a lab, or a specific group in production).


Finish claim (attribute) rules after you know which username and group values DLS expects. See **Mapping Identity Provider Groups to DLS Roles**. 
**Add a SAML identity source on DLS**
In the DLS Web UI, open the settings area used for **identity sources** / authentication (same family of settings as LDAP). 
Create a new identity source and choose **SAML**.   
| Setting (plain language)  | What to enter  |  
| --- | --- |  
| Display name  | A label users may see (for example Corporate ADFS).  |  
| Enabled  | Leave off until registration and mapping are complete, then turn on.  |  
**How DLS learns about your identity provider**
Choose **one** approach:   
| Approach  | When to use it  |  
| --- | --- |  
| Metadata URL  | DLS can reach your IdP over the network and download federation metadata.  |  
| Metadata file / paste  | DLS cannot reach the IdP, or you prefer to avoid outbound download.  |  
| Manual details  | You enter the IdP issuer (entity ID), sign-in URL, and signing certificate yourself.  |  
If the metadata URL uses a private or internal HTTPS certificate, you may also need to provide trusted CA certificates so DLS can verify that HTTPS connection. This is separate from the IdP **signing** certificate used to verify login messages. **Optional settings you may need**  
| Setting  | Guidance  |  
| --- | --- |  
| Username attribute  | Which SAML attribute becomes the DLS username. Leave empty to use the SAML Name ID.  |  
| Groups attribute  | Which SAML attribute carries group membership for role mapping.  |  
| Require signed assertion / response  | Follow your IdP’s signing practice. At least one signature requirement should remain enabled.  |  
| Sign login requests / require encrypted assertions  | Enable only if your IdP requires them; then supply the matching DLS certificates and private keys your security team provides.  |  
| Clock skew  | Small allowance for time difference between DLS and the IdP (default is typically two minutes).  |  
**Register DLS with the identity provider**
From DLS, obtain the service provider values for this identity source:   
| Value  | Purpose  |  
| --- | --- |  
| Entity ID  | How the IdP identifies the DLS application.  |  
| Assertion Consumer Service (ACS) URL  | Where the IdP sends the browser after successful sign-in.  |  
| Metadata (optional download)  | Convenience package of the above for IdPs that accept a file.  |  
On the identity provider, set:
  1. The relying party identifier / audience to the DLS **entity ID**.
  2. The SAML ACS (HTTP-POST) endpoint to the DLS **ACS URL**.
  3. Claim rules so the assertion includes the username and groups DLS will map.


Note:
The ACS URL and entity ID must match exactly (including https, host name, and path). A mismatch is a common cause of failed sign-in. 
**Map roles and enable the source**
  1. Configure group and optional user mappings as described in **Mapping Identity Provider Groups to DLS Roles**.
  2. Disable any enabled LDAP or OIDC identity sources if you are switching to SAML.
  3. Enable the SAML identity source.
  4. Keep local password login available until you have verified SSO in your environment.


**Verify the configuration**
  1. Sign out of the DLS Web UI.
  2. Open the DLS login page.
  3. Confirm that a button such as **Sign in with <your display name>** appears.
  4. Complete sign-in at the identity provider (including MFA if required).
  5. Confirm that you land in the DLS Web UI with the expected role.


If verification fails, see **Troubleshooting Single Sign-On**. 
**What to do next**
  * Document the break-glass administrator procedure for your operations team.
  * If you use multiple replicas of DLS, ensure all copies share the same public address and site configuration so sign-in can complete on any healthy copy.


###  [2.8.8.4. Configuring OpenID Connect (OIDC) Authentication](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-oidc-authentication)
Perform this task to allow users to sign in to the DLS Web UI through an OpenID Connect identity provider. 
Before you begin, complete [**Before You Configure Single Sign-On**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#before-you-configure-single-sign-on) and [**About Single Sign-On on a DLS Virtual Appliance**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#about-sso-dls-virtual-appliance). 
**Setup Overview**
  1. Create an OIDC application (client) for DLS in your identity provider.
  2. Configure an OIDC identity source on DLS with issuer, client credentials, and claims.
  3. Register DLS’s redirect URL with the identity provider.
  4. Map groups or users to DLS roles.
  5. Enable the OIDC identity source and verify sign-in.


DLS uses the authorization code flow with Proof Key for Code Exchange (PKCE), which is the usual pattern for browser applications. 
**Create the application in your identity provider**
In your identity provider, create a client / application with:   
| Setting  | Typical guidance for DLS  |  
| --- | --- |  
| Application type  | Web / confidential client (or your IdP’s equivalent for server-side apps).  |  
| Grant / flow  | Authorization code  |  
| Redirect URI  | The DLS OIDC callback URL provided by your DLS build / settings (must match exactly).  |  
| Scopes  | Include openid. Add profile/email scopes if your IdP requires them for username claims.  |  
Note:
Group membership is usually returned as a token claim, not as an OAuth scope named groups. Requesting a non-existent groups scope often causes the identity provider to reject sign-in. 
Configure any MFA or access policies on the identity provider as required by your organization. 
**Add an OIDC identity source on DLS**
In the DLS Web UI, open identity source / authentication settings and create a source with protocol **OIDC**. Provide:   
| Setting (plain language)  | What to enter  |  
| --- | --- |  
| Display name  | Label for the login button (for example Corporate Entra ID).  |  
| Issuer  | The IdP issuer URL used for discovery (often ends without a trailing path beyond the issuer root your IdP documents).  |  
| Client ID  | From the IdP application registration.  |  
| Client secret  | From the IdP (stored securely on DLS; not shown in clear text after save).  |  
| Username claim  | Token field that becomes the DLS username (for example preferred_username or email).  |  
| Groups claim  | Token field that lists groups for role mapping (for example groups).  |  
| Enabled  | Leave off until mapping and redirect registration are complete.  |  
**Optional** : restrict or extend scopes according to your IdP documentation. Always keep **openid**. 
**Register the redirect URL**
Your identity provider must allow the exact DLS redirect / callback URL.
  1. Copy the redirect URL from DLS settings or product documentation for your release.
  2. Paste it into the IdP application’s allowed redirect URIs.
  3. Ensure users open DLS with the same public host name used in that URL.


**Map roles and enable the source**
  1. Configure group and optional user mappings as described in [**Mapping Identity Provider Groups to DLS Roles**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#mapping-identity-provider-groups-to-dls-roles).
  2. Disable enabled LDAP or SAML sources if you are switching to OIDC.
  3. Enable the OIDC identity source.
  4. Keep local password login available until SSO is verified.


**Verify the configuration**
  1. Sign out of the DLS Web UI.
  2. Open the login page and select **Sign in with <your display name>**.
  3. Complete sign-in at the identity provider.
  4. Confirm that you return to DLS with the expected role.


If verification fails, see [**Troubleshooting Single Sign-On**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-single-sign-on). 
**What to do next**
  * Train operators on which button to use when multiple OIDC providers are shown.
  * Review break-glass access for identity provider outages.


###  [2.8.8.5. Mapping Identity Provider Groups to DLS Roles](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#mapping-identity-provider-groups-to-dls-roles)
Perform this task so that users who sign in with SSO receive the correct DLS permissions. 
DLS uses the same roles as local and LDAP authentication:   
| DLS role  | Typical responsibility  |  
| --- | --- |  
| Administrator (DLS_ADMIN)  | Full appliance and license-server administration.  |  
| Operator (DLS_OPERATOR)  | Day-to-day operations with a reduced permission set.  |  
| User (DLS_USER)  | Limited access as defined by your DLS RBAC configuration.  |  
**How mapping works**
After successful identity-provider authentication, DLS first checks for a user-specific role mapping. If none matches, it checks group mappings. For SAML and OIDC, the lowest-privilege role wins when several groups match: 
  1. SAML: Providers configured through the current DLS Web UI assign User (DLS_USER) when no user or group mapping matches
  2. OIDC: When no mapping matches, the default is User (DLS_USER), unless the source or deployment default-role policy changes or disables this fallback.
  3. LDAP: With no user or group mappings configured for the authenticating LDAP identity source, authenticated users receive Administrator (DLS_ADMIN). If mappings exist but no user or group mapping matches, access is denied. A matching user-specific mapping takes precedence over group mappings. Otherwise, when several LDAP groups match, the highest-privilege active role wins: DLS_ADMIN, then DLS_OPERATOR, then DLS_USER.


Note:
After successful identity-provider authentication, DLS first checks for a user-specific role mapping. If none matches, it checks group mappings. For SAML and OIDC, the lowest-privilege role wins when several groups match. 
SAML: Providers configured through the current DLS Web UI assign User (DLS_USER) when no user or group mapping matches. 
OIDC: When no mapping matches, the default is User (DLS_USER), unless the source or deployment default-role policy changes or disables this fallback. 
LDAP: With no user or group mappings configured for the authenticating LDAP identity source, authenticated users receive Administrator (DLS_ADMIN). If mappings exist but no user or group mapping matches, access is denied. A matching user-specific mapping takes precedence over group mappings. Otherwise, when several LDAP groups match, the highest-privilege active role wins: DLS_ADMIN, then DLS_OPERATOR, then DLS_USER. 
**Before you configure mapping**
  1. Sign in a test user at the identity provider and confirm which **username** and **group names** appear in the SAML assertion or OIDC token.
  2. Use those **exact** strings in DLS. Group matching is case-sensitive.
  3. For SAML, confirm the **groups attribute name** (for example a short name such as groups, or a longer URI your IdP emits).
  4. For OIDC, confirm the **groups claim** name in the ID token.


Tip:
A common ADFS lab mistake is mapping DLS-Admins in DLS while the assertion contains DlsAdmins, or looking for attribute groups while the IdP sends `http://schemas.xmlsoap.org/claims/Group`. 
**Configure group for role mapping**
In the SAML or OIDC identity source settings:
  1. Set the groups attribute or groups claim to the name your IdP uses.
  2. Add rows that map each IdP group to a DLS role, for example:   
| Identity provider group (example)  | DLS role  |  
| --- | --- |  
| DlsAdmins  | Administrator  |  
| DlsOperators  | Operator  |  
| DlsUsers  | User  |  
  3. Save the identity source.


**Optional: user for role mapping**
Use user mapping for exceptions (for example a break-glass corporate account that must always be Administrator). User mapping overrides group mapping for that username. 
**Optional: default role**  
| Choice  | Effect  |  
| --- | --- |  
| No default (recommended)  | Users with no matching map cannot open DLS.  |  
| Default User / Operator / Administrator  | Any authenticated IdP user without a map receives that role.  |  
CAUTION:
Adding SAML or OIDC group mappings does not by itself reject users who match none of them. DLS uses the applicable source default role or deployment fallback when no user or group mapping matches. Leaving the source default role unset does not disable the deployment fallback. 
The current SAML mapping form sets the unmatched-user role to User and does not provide a default-role selector. The OIDC mapping form also has no default-role selector. If a source or deployment policy is configured outside these forms, verify the effective policy before relying on rejection of unmapped users. A fallback of Administrator grants administrative access to every authenticated user who matches no mapping. 
**Verify mapping**
  1. Sign in with a user who belongs only to an Administrator-mapped group → confirm Administrator capabilities.
  2. Sign in with a User-mapped account → confirm reduced access.
  3. Sign in with an authenticated account that matches no user or group mapping. For a SAML provider configured through the current Web UI, confirm User access. For OIDC, confirm the effective source or deployment fallback, which is User by default.
  4. If the intended policy rejects unmapped SSO users, verify that both the source default and deployment fallback are disabled, then confirm that the test account is denied.


**What to do next**
Return to **Logging in with Single Sign-On** for the end-user sign-in experience. 
###  [2.8.8.6. Logging in with Single Sign-On](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-with-sso)
Perform this task after SAML or OIDC authentication is configured and enabled on the DLS virtual appliance. 
This topic supplements **Logging in to a DLS Virtual Appliance** in the DLS User Guide. 
**Before you log in**
  * Your administrator has enabled SSO and assigned you access in the corporate identity provider.
  * You use the same DLS address that was registered with the identity provider (correct host name and **https**).
  * If your organization uses MFA, have your second factor ready at the identity provider prompt.


**Sign in with SSO**
  1. In a supported web browser, go to the DLS management interface URL provided by your administrator.
  2. On the login page, select the button for your organization (for example **Sign in with Corporate ADFS**).
  3. If more than one SSO button appears, choose the provider your administrator told you to use. DLS does not automatically try another provider after a failure.
  4. Complete sign-in at the corporate identity provider, including any MFA challenge.
  5. When prompted, allow the browser to return to DLS. You should see the DLS Web UI for your role.


You do **not** enter your corporate password into a DLS password field during SSO. The password is entered only at the identity provider. 
**Sign in with a local password (break-glass)**
The current DLS Web UI keeps password sign-in available for emergency access. It does not provide a control to disable or re-enable this sign-in path. Use the protected local administrator account when the identity provider is unavailable.
  1. Open the DLS login page.
  2. Enter the local administrator username and password in the username and password fields, then select Sign In. Do not select an SSO provider button for local sign-in.


Note:
If password login was turned off by policy, only SSO (and any remaining local emergency procedure your organization defined offline) will work. 
**After you are signed in**
  * Your permissions follow the DLS role assigned by SSO mapping (Administrator, Operator, or User).
  * Day-to-day pages behave the same as for locally authenticated users with that role.
  * Sign out using the Web UI sign-out control when you finish.


**Next Steps**
If sign-in fails, contact your DLS administrator and see **Troubleshooting Single Sign-On**. 
###  [2.8.9. Logging in to a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-virtual-appliance)
Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance.
Also see [Logging in with Single Sign-On](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-with-sso) for information on using SSO login and signing in with a local password.
Ensure that the DLS administrator user has been registered for the appliance as explained in [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface.").
  1. Open a web browser and connect to the URL `https://_dls-vm-ip-address_`. 

_dls-vm-ip-address_ 
     The IP address or, if defined, the fully qualified domain name or the CNAME of the VM on which the DLS virtual appliance is installed. 
You can get the IP address from the management console of your hypervisor.
  2. On the login page that opens, provide the user credentials for the DLS administrator user on the DLS virtual appliance and click **LOGIN**.


If LDAP is used as an alternative to the fixed set of user accounts for user management, you can use the rsu_admin account to administer the DLS virtual appliance instead. See [_Integrating a DLS Instance with an LDAP Server_](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#integrating-dls-instance-with-ldap-server) for more information. 
###  [2.8.10. Gathering Log Files for Analysis and Troubleshooting](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#gathering-log-files-for-analysis-and-troubleshooting)
To collect the log files of a DLS Virtual Appliance to troubleshoot a problem, use the dls_admin user and run the `sudo /etc/adminscripts/collect_dls_logs.sh` command.
The collect_dls_logs.sh script collects the following information for analysis: 
  * **Critical information:**
    * CPU information from /proc/cpuinfo
    * RAM information from /proc/meminfo
    * DLS startup log file /var/log/applicationStartup.log
    * DLS VM appliance log file /var/log/applianceOps.log
    * IP address information log file /var/log/ip_address.log
    * Disk consumption (directories with more than 100 MB usage and database disk consumption) and free space
    * Docker logs for both containers
  * **Non-critical logs:**
    * Chrony, timesyncd, and timedatectl output
    * Syslog information
    * Audit log files
  * **Service log files:**
    * Log files for the DLS services


###  [2.8.11. User Deletion in Appliances with Multiple Users Registered](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#user-deletion-in-appliances-with-multiple-users-registered)
The deletion of an additional user in a system with multiple users is facilitated through a shell script. This script must be executed on the primary node within a High Availability (HA) cluster. It ensures that user deletion is only performed if the specified user exists and if the system has more than one registered user. If the user does not exist or if the system contains only a single user, the script will not execute the deletion, thereby maintaining the integrity of the user management process.
  1. Enable the sudo user `rsu_admin` on the DLS appliance.
  2. Log in to the appliance using `rsu_admin`.
  3. Run the `/etc/adminscripts/delete_user.sh script`. It accepts an argument of username to be deleted: `$ /etc/adminscripts/delete_user.sh dls_user_to_be_deleted`


###  [2.9. Configuring an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ha-cluster-dls-instances)
To provide licensed clients with continued access to licenses if a DLS instance fails, you can configure a highly available (HA) cluster of two or more DLS instances in a failover configuration. A failover configuration consists of a _primary_ instance, which is actively serving licenses to licensed clients, and one or more _secondary_ instances, which act as a backup for the primary instance.
If you are configuring an HA cluster of containerized DLS instances, you **must** deploy the containers that host the instances on different Kubernetes clusters. To prevent the NVIDA Licensing application from behaving abnormally, do **not** deploy the containers on different worker nodes in the same Kubernetes cluster. The application would behave abnormally because the orchestrator forwards requests to ports that are mapped onto the corresponding Kubernetes service. 
If an attempt to configure an HA cluster of containerized DLS instances fails, refer to [HA Configuration or Online Upgrade Fails](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#ha-configuration-or-online-upgrade-fails "High availability \(HA\) configuration or online upgrade between two containerized DLS appliances fails if port mappings or volume configurations are incorrect.") for troubleshooting information. 
Note:
After configuring high availability, you can optionally enable Virtual IP Management to provide transparent failover for licensed clients. Virtual IP Management automatically migrates a single IP address between cluster nodes during failover, allowing clients to maintain connectivity without reconfiguration. See Section 2.9.x Configuring Virtual IP Management for High Availability for details. 
###  [2.9.1. Setting the Maximum Cluster Size for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-maximum-cluster-size-dls-instance)
By default, a DLS instance can be added only to an HA cluster of two instances. If you want to add a DLS instance to a cluster of more than two DLS instances, you must set the maximum cluster size explicitly.
After setting the maximum cluster size for a DLS instance, you cannot change it. 
How to set the maximum cluster size for a DLS instance depends on whether the instance is a VM-based instance or a containerized instance. For detailed instructions, refer to: 
  * [Setting the Maximum Cluster Size for a VM-Based DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-maximum-cluster-size-vm-based-dls-instance)
  * [Setting the Maximum Cluster Size for a Containerized DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-maximum-cluster-size-containerized-dls-instance)


###  [2.9.1.1. Setting the Maximum Cluster Size for a VM-Based DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-maximum-cluster-size-vm-based-dls-instance)
Perform this task on each DLS instance that you want to add to a cluster of more than two DLS instances. 
  1. Use the hypervisor management console of the appliance to log in as the user dls_admin to the VM that hosts the DLS instance's virtual appliance. If the dls_admin user has not been registered, you can still log in to the VM as the dls_admin user with the default password welcome.
  2. Run script /etc/adminscripts/enable_ha_max.sh. 
Copy
Copied!

```
            
            
$ /etc/adminscripts/enable_ha_max.sh

        
```

  3. When prompted, enter an integer in the range 2-9 that specifies the maximum number of instances in the cluster to which you want to add this instance.


After setting the maximum cluster size for the instance, add the instance to a cluster as explained in [Creating or Expanding an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-ha-cluster-dls-instances "Any time after creating or configuring a standalone DLS instance, you can create an HA Cluster of DLS instances by converting the instance into a node in a cluster and adding a second instance to the cluster. You can also expand an existing cluster by adding a DLS instance to it."). 
###  [2.9.1.2. Setting the Maximum Cluster Size for a Containerized DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-maximum-cluster-size-containerized-dls-instance)
Perform this task on each DLS instance that you want to add to a cluster of more than two DLS instances. 
  1. Edit the appropriate deployment file for your container orchestration platform to uncomment the line that sets the environment variable `DLS_HA_MAX_NODES_ENV`. To determine the deployment file for your container orchestration platform, refer to [Setting Properties for a Containerized DLS Software Image](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-environment-variables-for-containerized-dls-software-image "To enable communications with the container within which the DLS appliance is running, you must set properties such as host names, IP addresses, and port numbers. Set these properties before deploying the containerized DLS software image on a supported platform.")
  2. Set the `DLS_HA_MAX_NODES_ENV` environment variable to an integer in the range 2-9 that specifies the maximum number of instances in the cluster to which you want to add this instance. 
How to set an integer value depends on your container orchestration platform. For more information, refer to the following topics:
     * [Specifying Integer Values as Environment Variables](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#specifying-port-numbers-by-setting-environment-variables "You can specify integer values such as port numbers for Docker, Podman, Red Hat OpenShift Container Platform, and VMware Tanzu Application Platform as environment variables.")
     * [Specifying Integer Values as Actual Values](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#specifying-port-numbers-as-integer-values "Kubernetes does not accept strings for integer values. Therefore, you cannot specify integer values for Kubernetes as environment variables. Instead, you must specify integer values for Kubernetes as actual values.")
This example sets the maximum cluster size for a containerized DLS instance to 3. 
Copy
Copied!

```
            
            
DLS_HA_MAX_NODES_ENV=${DLS_HA_MAX_NODES_ENV:-3}

        
```

  3. Restart the container.


After setting the maximum cluster size for the instance, add the instance to a cluster as explained in [Creating or Expanding an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-ha-cluster-dls-instances "Any time after creating or configuring a standalone DLS instance, you can create an HA Cluster of DLS instances by converting the instance into a node in a cluster and adding a second instance to the cluster. You can also expand an existing cluster by adding a DLS instance to it."). 
###  [2.9.2. Creating or Expanding an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-ha-cluster-dls-instances)
Any time after creating or configuring a standalone DLS instance, you can create an HA Cluster of DLS instances by converting the instance into a node in a cluster and adding a second instance to the cluster. You can also expand an existing cluster by adding a DLS instance to it.
Ensure that the following prerequisites are met: 
  * The DLS virtual appliance that will host the DLS instance to be added to the cluster has been installed and started. 
Note:
The version of this virtual appliance must be identical to the version of the virtual appliances that are hosting the instances that are already members of the cluster. You cannot configure an HA cluster in which the versions of the virtual appliances are different. 
  * The DLS administrator user has **not** been registered on the DLS virtual appliance that will host the DLS instance to be added to the cluster. The registration of the DLS administrator user is propagated to the other instance when you configure the cluster. 
Note:
If you are creating a cluster from a pair of newly created standalone DLS instances, ensure that the DLS administrator user has been registered **only** on one virtual appliance. 
  * If you are configuring a cluster of more than two nodes, ensure that the maximum cluster size has been set for each DLS instance that you want to be a member of the cluster. For detailed instructions, refer to [Setting the Maximum Cluster Size for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-maximum-cluster-size-dls-instance "By default, a DLS instance can be added only to an HA cluster of two instances. If you want to add a DLS instance to a cluster of more than two DLS instances, you must set the maximum cluster size explicitly.") .


  1. Log in to the DLS virtual appliance that will host or is already hosting the primary DLS instance. 
     * If you are creating a cluster, log in to the DLS virtual appliance that hosts the DLS instance that you want to convert. 
After the instance is converted, it will **initially** be the primary DLS instance.
     * If you want to expand an existing cluster, log in to the DLS virtual appliance that hosts the **primary** node in the cluster.
  2. Open Home and locate the Appliance section..
  3. For a standalone appliance, select Configure High Availability. To expand an existing cluster, select Add Node to the Cluster when the cluster has capacity for another node.
  4. In Network location, enter the IP address or fully qualified domain name (FQDN) of the appliance to add as a secondary node. For an FQDN, choose IPv4 or IPv6 under FQDN resolution address type.
  5. Select Ping to check that the appliance is reachable. Configure becomes available after the reachability check succeeds.
  6. Select Configure and wait for cluster configuration to complete. Check the cluster and node status in the Appliance section on Home.
  7. When the configuration is complete, the Appliance section on Home shows the node health of the cluster.


If you intend to use static IP addresses for the virtual appliances that are hosting the DLS instances in the cluster, set the address of each virtual appliance as explained in [Setting the Static IP Address of a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address "You can use the management interface to the NVIDIA Licensing application to replace the existing IP address of the appliance with a new static IP address. The existing IP address can be an address assigned by DHCP or another static IP address."). Otherwise, configure the DLS instance on the virtual appliance that is hosting the **primary** DLS instance as explained in [Configuring a License Server Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-service-instance "How you configure a service instance depends on whether the service is a Cloud License Service \(CLS\) instance or a Delegated License Service \(DLS\) instance."). 
If a client configuration token has already been generated for any instance in the cluster, regenerate the client configuration token for the instance as explained in [Generating a Client Configuration Token](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token). 
To fail over or change the roles of the DLS instances, restart the DLS virtual appliance that is hosting the **primary** DLS instance. 
Note:
If the instances in an HA cluster of DLS instances fail or are shut down at the same time, avoid a race condition by restarting only one instance and waiting until the startup of that instance is complete before starting the next instance. 
###  [2.9.3. Removing a Node from an HA Cluster](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#removing-node-from-ha-cluster)
You can remove a **secondary** node from an HA cluster. If the secondary node is removed from a two-node cluster, the primary node is converted to a standalone DLS instance. You can perform this task to remove a secondary node even if it is down. 
  1. Log in to the DLS virtual appliance that hosts the **primary** node in the cluster.
  2. Open Home and locate the Appliance section..
  3. In the Nodes table, open the three-dot Node actions menu for the secondary node to remove, and select Remove.
  4. Review the Remove cluster node confirmation and select Confirm


What happens to the node that has been removed depends on the type of platform that is hosting the nodes in the cluster: 
  * For a cluster of instances on VM-based DLS virtual appliances, the virtual appliance that hosts the node is shut down and all data on the node is removed.
  * For a cluster of instances on different container orchestrators, the container that hosts the node is **not** shut down. You must shut down the container manually.


If the secondary node is removed from a two-node cluster, the primary node is converted to a standalone DLS instance. 
If a client configuration token has already been generated for any instance in the cluster, regenerate the client configuration token for the instance as explained in [Generating a Client Configuration Token](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token). 
If all nodes fail or are shut down, you must restart the last node to fail or be shut down **first**. If you restart the first node to fail or be shut down first, the node will not be functional until the other nodes are started. 
###  [2.9.4. Marking a Node as the Primary Node in an HA Cluster](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#marking-node-as-primary-in-ha-cluster)
Initially, the primary node in an HA cluster is the node that is hosted on the DLS appliance from which the cluster was created. It remains the primary node unless or until it fails, at which point failover occurs and the secondary node becomes the primary node. If you want to control which node is the primary node a cluster, you can mark a secondary node as the primary node in the cluster.
  1. Log in to the DLS virtual appliance that hosts the secondary node that you want to mark as the primary node.
  2. Open Home and locate the Appliance section.
  3. In the Nodes table, open the three-dot Node actions menu for the secondary node that you are signed in to..
  4. Select Mark as Primary, then select Confirm in the confirmation dialog. The node assumes the primary role and starts to serve licenses to clients. All other nodes in the cluster, including the node that was the primary node, are marked as secondary nodes.


###  [2.9.5. Enabling the DoD-Approved Login Banner Warning](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#enabling-dod-approved-login-banner-warning)
By default, the DoD-approved login banner warning is disabled. If a VM-based DLS appliance is running on a US Government (USG) Information System (IS) that is provided for USG-authorized use only, enable this warning for logins through the hypervisor management console and through secure shell (SSH). When enabled, the banner warning is displayed whenever a user tries to log in to the system.
Ensure that the sudo DLS user account `rsu_admin` has been created as explained in [Creating the DLS sudo User Account](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-dls-sudo-user "The predefined DLS sudo user account rsu_admin has the elevated privileges required to install updates to the DLS appliance software that NVIDIA releases periodically. To comply with the terms of the GPL/LGPL v3 license under which the GPL/LGPL v3 licensed Open Source Software \(OSS\) libraries within the DLS virtual appliance are released, this account also has the elevated privileges required to update and upgrade these libraries.").
  1. Enable the DoD-approved login banner warning for logins through the hypervisor management console. 
    1. Use the hypervisor management console of the appliance to log in as the user dls_admin to the VM that hosts the DLS virtual appliance.
    2. Run the /etc/adminscripts/enable_stig_custom_message.sh script. 
Copy
Copied!

```
            
            
$ /etc/adminscripts/enable_stig_custom_message.sh

        
```

  2. Enable the DoD-approved login banner warning for logins through SSH. 
    1. Use the hypervisor management console of the appliance to log in as the user rsu_admin to the VM that hosts the DLS appliance.
    2. As root, grant read access for all users to the message of the day file (/etc/motd) and allow the owner of the file also to write to and execute the file. 
Copy
Copied!

```
            
            
$ sudo chmod 644 /etc/motd

        
```



If you need to disable the DoD-approved login banner warning for logins through the hypervisor management console, run the /etc/adminscripts/disable_stig_custom_message.sh script as the dls_admin user. 
###  [2.9.6. Configuring High Availability with Virtual IP Management](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-high-availability-with-virtual-ip-management)
The Virtual IP (VIP) Management feature enables seamless failover for NVIDIA Delegated License Service (DLS) instances configured in a high availability (HA) cluster. By assigning a single virtual IP address that automatically migrates between cluster nodes during failover events, client applications maintain uninterrupted access to licensing services without manual reconfiguration.
####  Key Benefits 
  * **Transparent Failover** : Licensed clients connect to a single virtual IP address that automatically moves to the active primary node during failover events
  * **Zero Client Reconfiguration** : Client configuration tokens remain valid across failover events, eliminating the need to update client systems
  * **Automatic Network Convergence** : Gratuitous ARP (GARP) ensures immediate network routing updates when the virtual IP migrates
  * **Split-Brain Prevention** : Built-in safeguards prevent multiple nodes from simultaneously claiming the virtual IP address
  * **Centralized Management** : RESTful API and web UI for configuring and monitoring virtual IP assignments


####  Supported Platforms 
Virtual IP Management is available on: 
  * DLS Virtual Appliances (VM-based) version 3.6.0 and later
  * All supported hypervisors (VMware vSphere, Microsoft Hyper-V, Nutanix AHV, Red Hat Virtualization)


Note:
Virtual IP Management is not available on containerized DLS deployments in pure container environments (Docker Compose without VM). 
###  [2.9.6.1. Prerequisites of Virtual IP Management](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#prerequisites-of-virtual-ip-management)
Before configuring Virtual IP Management, ensure the following requirements are met.
#####  Network Requirements 
  * **Available IP Address** : One unused IPv4 or IPv6 address on the same subnet as your DLS cluster nodes
  * **Network Permissions** : The IP address must not be in use by any other device or reserved by DHCP: Client configuration tokens remain valid across failover events, eliminating the need to update client systems
  * **Layer 2 Connectivity** : All nodes in the HA cluster must be on the same Layer 2 network segment
  * **ARP Support** : Network infrastructure must support Address Resolution Protocol (ARP) and Gratuitous ARP


#####  HA Cluster Requirements 
A functional high availability cluster with: 
  * Two Or more DLS instances configured as primary and secondary nodes


For information on configuring an HA cluster, see _Configuring High Availability for a DLS Instance_ in the License System User Guide. 
#####  System Requirements 
  * **LS Version** : 3.6.0 or later


#####  User Permissions 
  * **DLS Administrator** : Access to the DLS web UI with DLS_ADMIN role
  * **Portal Access** :Not required (virtual IP configuration is managed entirely on the DLS instance)


###  [2.9.6.2. Architecture and Operation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#architecture-and-operation)
#####  How Virtual IP Management Works 
When configured, the Virtual IP Management system operates as follows: 
  1. **Initial Configuration** : An administrator assigns a virtual IP address to the HA cluster from the settings page of DLS Web UI of current primary node, which is automatically replicated to all secondary nodes.
  2. **Primary Assignment** : The current primary node automatically assigns the virtual IP to its network interface.
  3. **Network Announcement** : The primary node sends Gratuitous ARP packets to inform the network of the IP-to-MAC address mapping.
  4. **Client Connectivity** : Licensed clients connect to the virtual IP address for license requests.
  5. **Failover Detection** : When the secondary node detects primary node failure through heartbeat loss, it initiates failover.
  6. **VIP Migration** : The secondary node (now primary) assigns the virtual IP to its network interface.
  7. **Network Update** : Gratuitous ARP packets announce the new MAC address for the virtual IP.
  8. **Client Continuity** : Licensed clients continue using the same virtual IP address without reconfiguration.


#####  Split-Brain Prevention Mechanisms 
The VIP Management system includes multiple safeguards to prevent split-brain scenarios where both nodes attempt to claim the virtual IP: 
  * **Startup Safety Check** : Nodes verify their role (primary/secondary) on system startup and remove VIP assignments if configured as secondary.
  * **Duplicate Detection** : Before assigning the VIP, nodes use _arping_ to detect if another system is already using the IP address.
  * **Network Isolation** : During VIP removal, _iptables_ rules temporarily block traffic to/from the VIP to prevent packet loss.
  * **Role-Based Assignment** : Only the node with the primary role can assign the virtual IP.
  * **ARP Cache Flush** : Stale ARP entries are cleared during VIP removal to prevent routing issues.


#####  Network Traffic Flow 
**Normal Operation (Primary Node Active)** : 
Licensed Client > Virtual IP (192.0.2.100) > Primary Node (eth0) > DLS Services 
**During Failover** : 
  * Primary Node Failure Detected
  * Secondary Promotes to Primary
  * Virtual IP Migrates to New Primary
  * GARP Sent (3 packets)
  * Network Updates ARP Cache
  * Licensed Client > Virtual IP (192.0.2.100) > New Primary Node (eth0) > DLS Services


**Old Primary Returns Online** : 
  * Old Primary Boots
  * Detects self having SECONDARY role
  * Removes Any Stale VIP Assignment
  * Remains as Secondary Node
  * Does NOT Reclaim Virtual IP


###  [2.9.6.3. Configuring Virtual IP Management](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-virtual-ip-management)
Before configuring Virtual IP Management, ensure the following requirements are met.
#####  Task Overview 
Configuring Virtual IP Management for an HA cluster involves the following tasks: 
1. Assigning the virtual IP address via API or web UI 
2. Verifying VIP assignment and network connectivity 
3. Testing failover behavior 
#####  Assigning Virtual IP address using DLS - Web UI 
  1. Navigate to the **Settings** page on the Primary node’s DLS Web UI
  2. Expand the UI card **“Virtual IP Address Configuration”**
  3. Set the Virtual IP and click on “**Configure** ”


#####  Removing a Virtual IP Address 
To remove the virtual IP assignment from the HA cluster: 
  1. Use the web UI to delete the configured virtual IP from **Settings** page
. 
  2. Expand the UI card **Virtual IP Address Configuration**
. 


Click **Delete Virtual IP**
Important:
Deleting the virtual IP triggers immediate removal from both primary and secondary nodes. Licensed clients will lose connectivity to the virtual IP address. 
###  [2.9.6.4. Verifying Virtual IP Assignment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-virtual-ip-assignment)
To verify that the virtual IP is correctly assigned to the primary node: 
**On the Primary DLS Node** : 
Check if VIP is assigned to network interface 
Copy
Copied!

```
            
            
ip addr show | grep <virtual-ip>

        
```

Expected output: 
Copy
Copied!

```
            
            
inet 192.0.2.100/24 scope global secondary eth0

        
```

Verify VIP configuration file: 
Copy
Copied!

```
            
            
cat /var/lib/docker/volumes/configurations/_data/vip/ip.txt

        
```

Expected output: 192.0.2.100 
**From Another System on the Network** : 
Verify ARP entry points to primary node 
Copy
Copied!

```
            
            
arping -c 3 <virtual-ip>

        
```

Expected output shows primary node's MAC address. 
#####  Monitoring VIP Events 
Virtual IP events are logged to multiple locations: 
**System Logs** : 
VIP reassignment script logs `tail -f /var/log/vip_reassignment.log`
Systemd service logs `journalctl -u vipReassignment.path -f`
`journalctl -u vipReassignment.service -f`
**DLS Application Logs** : 
Logs can be accessed at `/var/lib/docker/volumes/logs/_data` using OS user _dls_admin_ or sudo user _rsu_admin_. 
Heartbeat service logs `tail -f /var/lib/docker/volumes/logs/_data/heartbeat.log`
HA configuration logs `tail -f /var/lib/docker/volumes/logs/_data/ha_config.log`
**DLS Web UI** : 
Navigate to **Dashboard** > **Events** - Filter by **Operation** : “Configure Virtual IP” or “Delete Virtual IP” 
Review event details including timestamp and user 
###  [2.9.6.5. Monitoring VIP During Failover](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#monitoring-vip-during-failover)
To observe VIP behavior during a failover event: 
  1. **Monitor Primary Node Logs** : 
Copy
Copied!

```
            
            
tail -f /var/log/vip_reassignment.log

        
```

  2. **Trigger Failover** (for testing): 
Restart the primary node virtual machine to simulate a failure and trigger failover.
  3. **Expected Log Sequence**
**On Primary Node** (before failure):
     * Virtual IP 192.0.2.100 assigned successfully
     * Sent 3 gratuitous ARP packets for 192.0.2.100
**On Secondary Node** (after failover detection):
     * Failover detected, promoting to primary
     * Virtual IP 192.0.2.100 assigned successfully
     * Sent 3 gratuitous ARP packets for 192.0.2.100
  4. **Verify Client Connectivity** : 
Licensed clients should maintain connectivity throughout the failover event (typically 30-60 seconds for complete failover).


###  [2.9.6.6. Best Practices for Virtual IP Deployment](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#best-practices-for-virtual-ip-deployment)
#####  Planning Virtual IP Deployment 
· **IP Address Selection** : Choose an IP address that: 
  * Is on the same subnet as your DLS cluster nodes
  * Is not allocated by DHCP
  * Is outside your organization’s dynamic IP ranges
  * Has been verified as unused via network scanning
  * Follows your organization’s IP address naming conventions


· **Documentation** : Maintain documentation of: 
  * Virtual IP address assignment
  * Physical IP addresses of primary and secondary nodes
  * Network subnet configuration


#####  Network Configuration 
  * **ARP Cache Timeout** : Configure network switches with appropriate ARP cache timeouts (recommended: 60-300 seconds) to balance between network efficiency and failover speed.
  * **Port Security** : If using port security on network switches, configure: 
    * Allow multiple MAC addresses per port (for VIP migration)
    * Use “sticky” MAC learning with a sufficient MAC address limit
    * Exclude ports connecting to DLS nodes from strict port security


  * **Network Monitoring** : Configure network monitoring to:


  * Alert on duplicate IP address detection
  * Monitor ARP table changes for the virtual IP
  * Track failover events and VIP migration timing


#####  High Availability Configuration 
· **Testing Schedule** : Regularly test failover behavior: 
  * Monthly: Graceful failover testing (stop services on primary)
  * Quarterly: Forced failover testing (power off primary VM)
  * After any network changes: Full failover validation


· **Maintenance Windows** : During maintenance: 
  * Test VIP assignment after any DLS version upgrades
  * Verify VIP configuration after HA cluster changes
  * Validate client connectivity after network infrastructure updates


· **Monitoring and Alerts** : Configure monitoring for: 
  * VIP-related events in DLS event logs
  * Abnormal failover frequency (may indicate network issues)


#####  Security Considerations 
· **Network Segmentation** : Deploy DLS instances in a secure network segment: 
  * Use firewalls to control access to DLS management interfaces
  * Allow VIP traffic only from authorized client subnets
  * Implement network ACLs for inter-node communication


· **Audit Trail** : Regularly review: 
  * Virtual IP configuration change events
  * Failover event logs
  * Unauthorized access attempts to VIP management APIs


#####  Backup and Recovery 
**Configuration Backup** : Include VIP configuration in DLS backups: 
  * VIP configuration is stored in the DLS configuration database
  * Use DLS backup/migration features to preserve VIP settings
  * Document VIP assignment separately for disaster recovery


**Disaster Recovery** : In DR scenarios: 
  * VIP assignment can be retained when restoring to a new cluster
  * Ensure the new network segment can accommodate the VIP address
  * Update network infrastructure (switches, routers) if changing subnets


###  [2.9.6.7. Limitations and Known Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#limitations-and-known-issues)
#####  Planning Virtual IP Deployment 
· **IP Address Selection** : Choose an IP address that: 
  * Is on the same subnet as your DLS cluster nodes
  * Is not allocated by DHCP
  * Is outside your organization’s dynamic IP ranges
  * Has been verified as unused via network scanning
  * Follows your organization’s IP address naming conventions


· **Documentation** : Maintain documentation of: 
  * Virtual IP address assignment
  * Physical IP addresses of primary and secondary nodes
  * Network subnet configuration


#####  Network Configuration 
  * **ARP Cache Timeout** : Configure network switches with appropriate ARP cache timeouts (recommended: 60-300 seconds) to balance between network efficiency and failover speed.
  * **Port Security** : If using port security on network switches, configure: 
    * Allow multiple MAC addresses per port (for VIP migration)
    * Use “sticky” MAC learning with a sufficient MAC address limit
    * Exclude ports connecting to DLS nodes from strict port security


  * **Network Monitoring** : Configure network monitoring to:


  * Alert on duplicate IP address detection
  * Monitor ARP table changes for the virtual IP
  * Track failover events and VIP migration timing


#####  High Availability Configuration 
· **Testing Schedule** : Regularly test failover behavior: 
  * Monthly: Graceful failover testing (stop services on primary)
  * Quarterly: Forced failover testing (power off primary VM)
  * After any network changes: Full failover validation


· **Maintenance Windows** : During maintenance: 
  * Test VIP assignment after any DLS version upgrades
  * Verify VIP configuration after HA cluster changes
  * Validate client connectivity after network infrastructure updates


· **Monitoring and Alerts** : Configure monitoring for: 
  * VIP-related events in DLS event logs
  * Abnormal failover frequency (may indicate network issues)


#####  Security Considerations 
· **Network Segmentation** : Deploy DLS instances in a secure network segment: 
  * Use firewalls to control access to DLS management interfaces
  * Allow VIP traffic only from authorized client subnets
  * Implement network ACLs for inter-node communication


· **Audit Trail** : Regularly review: 
  * Virtual IP configuration change events
  * Failover event logs
  * Unauthorized access attempts to VIP management APIs


#####  Backup and Recovery 
**Configuration Backup** : Include VIP configuration in DLS backups: 
  * VIP configuration is stored in the DLS configuration database
  * Use DLS backup/migration features to preserve VIP settings
  * Document VIP assignment separately for disaster recovery


**Disaster Recovery** : In DR scenarios: 
  * VIP assignment can be retained when restoring to a new cluster
  * Ensure the new network segment can accommodate the VIP address
  * Update network infrastructure (switches, routers) if changing subnets


###  [2.10. Configuring a VM-Based DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-vm-based-dls-virtual-appliance)
A VM-based DLS virtual appliance strictly controls access to the underlying OS. Therefore, the management interface of the NVIDIA Licensing application enables you to perform configuration tasks that change the configuration of the underlying OS. During initial setup, you create both an OS-level user (dls_admin) and a system administrator application account (DLS_ADMIN role). After setup is complete, you can create additional application users with different roles and configure LDAP authentication if needed. See User Accounts on a DLS Virtual Appliance for details.
Note:
You can perform these tasks **only** on a VM-based DLS virtual appliance. You **cannot** perform them on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make equivalent changes for a containerized DLS software image. 
Note:
For VM-based DLS virtual appliances configured in an HA cluster, you can also configure Virtual IP Management to enable transparent failover for licensed clients. Virtual IP Management automatically migrates a floating IP address between cluster nodes during failover events. See Section 2.9.x Configuring Virtual IP Management for High Availability for details. 
###  [2.10.1. Setting the Static IP Address of a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-dls-appliance-static-ip-address)
You can use the management interface to the NVIDIA Licensing application to replace the existing IP address of the appliance with a new static IP address. The existing IP address can be an address assigned by DHCP or another static IP address.
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
Note:
The IP address of a VMware VM that hosts a DLS appliance can be set when the appliance is installed from an OVF template. If the IP address was set this way, you **must** follow the instructions in [Changing the IP Address of a VMware VM Set During DLS Appliance Installation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-vmware-vm-ip-address-set-at-dls-installation "The IP address of a VMware VM that hosts a DLS appliance can be set when the appliance is installed from an OVF template. If the IP address was set this way, you can change it only by editing vApps options within VM that hosts the DLS appliance. If you use any other method to change this address, it reverts to its original setting when the VM is restarted."). If you use any other method to change this address, it reverts to its original setting when the VM is restarted. In this situation, the Edit IP address action is unavailable. 
The instance that the DLS virtual appliance is hosting must already be configured as a standalone DLS instance or as an instance in a HA cluster.
Note:
You can set the static IP of the secondary node in an HA cluster from the primary node in the cluster. 
  1. If you aren't logged in already, log in to the DLS virtual appliance.
  2. Open Home and locate the Appliance section..
  3. In the Nodes table, open the three-dot Node actions menu for the target node and select Edit IP address. 
CAUTION:
If the DLS virtual appliance for which you are setting a static IP address is a node in an HA cluster and the type of any node is unknown, do not attempt to set the static IP address. Any change to the static IP address is not propagated to the node whose type is unknown because the node is unreachable.
  4. In the Edit IP address panel, enter the Static IP address and the applicable Gateway, Netmask prefix, DNS server one, and DNS server two values. 
    1. In the **Static IP address** text-entry field, type the IP address that you want to assign to the DLS virtual appliance.
    2. In the **Gateway** text-entry field, type the IP address of the DLS virtual appliance's default gateway. 
Note:
If you leave the **Gateway** field empty, the DLS virtual appliance uses DHCP settings. 
    3. In the **Netmask Prefix** text-entry field, type the subnet mask of the DLS virtual appliance's network in classless inter-domain routing (CIDR) format **without the leading slash character (/)**. The netmask prefix must be an integer in the range 2-32. To get a subnet mask in CIDR format from its decimal equivalent, refer to the table on page 2 of [IETF RFC 1878: Variable Length Subnet Table For IPv4](https://www.ietf.org/rfc/rfc1878.txt). For example, the subnet mask in CIDR format of the decimal equivalent 255.255.255.0 is 24.
    4. In the first **DNS Server** text-entry field, type the IP address of the first DNS server to be used for name resolution.
    5. In the second **DNS Server** text-entry field, type the IP address of the second DNS server to be used for name resolution.
If you are setting the IP address of the instance that you are logged in to, your browser will be disconnected from the instance after the update is complete. In this situation, you will need to log in to the DLS appliance again at the IP address that you set. 
Note:
Setting the IP address of an instance in an HA cluster causes a failover of the cluster. As a result of the failover, the roles of the primary and secondary instances in the cluster are reversed. 
  5. Select Set node IP address. If you changed the address of the node you are signed in to, reconnect to the DLS UI at its new address.
  6. If necessary, log in to the DLS virtual appliance again by connecting to the URL `https://_dls-vm-static-ip-address_`. 

_dls-vm-static-ip-address_ 
    The static IP address that you set for the DLS virtual appliance.


If the DLS instance hasn't already been configured and is a standalone instance or the **primary** instance in an HA cluster, configure the instance as explained in [Configuring a License Server Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-service-instance "How you configure a service instance depends on whether the service is a Cloud License Service \(CLS\) instance or a Delegated License Service \(DLS\) instance."). 
###  [2.10.2. Reverting the Network Configuration of a DLS Appliance to DHCP](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#reverting-network-configuration-dls-appliance-to-dhcp)
If necessary, you can revert the network configuration of a DLS appliance to DHCP after a static IP address has been assigned to the DLS appliance. After the network configuration is reverted to DHCP, the IP address of the appliance's network interface is assigned automatically. To revert the network configuration to DHCP, use the operating system command nmcli. Each DLS virtual appliance is configured with a user account with the elevated privileges required for running the nmcli command.
Perform this task from the hypervisor console, **not** from a secure shell (SSH) session. This task requires the requires DLS appliance's network service to be restarted, which would disconnect an SSH session. 
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
In the nmcli commands in the following steps for changing the configuration of the DLS appliance's network interface, you can use the modify subcommand as an alternative to the edit subcommand. 
Ensure that the sudo DLS user account `rsu_admin` has been created.
  1. Use the hypervisor management console of the appliance to log in as the user rsu_admin to the VM that hosts the DLS appliance.
  2. Get the connection name of the DLS appliance's network interface. 
Copy
Copied!

```
            
            
$ sudo nmcli connection show

        
```

The connection name of the DLS appliance's network interface in this example is Wired connection 1. 
Copy
Copied!

```
            
            
$ sudo nmcli connection show
NAME                UUID                                  TYPE      DEVICE** Wired connection 1**  458e8070-3a5b-41a2-9946-25b519bfc8f4  ethernet  --

        
```

  3. Change the assignment of an IP address to the DLS appliance's network interface from manual to automatic. 
Copy
Copied!

```
            
            
$ sudo nmcli conn edit "_connection-name_" ipv4.method auto

        
```


_connection-name_ 
    The connection name that you obtained in the previous step.
This example changes the assignment of an IP address to the DLS appliance's network interface Wired connection 1 from manual to automatic. 
Copy
Copied!

```
            
            
$ sudo nmcli conn edit "Wired connection 1" ipv4.method auto

        
```

  4. Remove the static IP address that was set for the DLS appliance from the DNS settings of the appliance's network interface. 
Copy
Copied!

```
            
            
$ sudo nmcli conn edit "_connection-name_" -ipv4.addresses _ip-address-to-remove_

        
```


_connection-name_ 
    The connection name that you specified in the previous step. 

_ip-address-to-remove_ 
    The IP address that you want to remove, which is the static IP address that was set for the DLS appliance's network interface.
This example removes the IP address `192.0.2.89` from the DNS settings of the DLS appliance's network interface Wired connection 1. 
Copy
Copied!

```
            
            
$ sudo nmcli conn edit "Wired connection 1" -ipv4.addresses 192.0.2.89

        
```

  5. Restart the DLS appliance's networking service. 
    1. Stop the DLS appliance's networking service. 
Copy
Copied!

```
            
            
$ sudo nmcli networking off

        
```

    2. Start the networking service. 
Copy
Copied!

```
            
            
$ sudo nmcli networking on

        
```

  6. Restart the VM that hosts the DLS appliance.


###  [2.10.3. Changing the Host Name of a VM-Based DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-host-name-of-vm-based-dls-virtual-appliance)
The host name of a VM-based DLS virtual appliance is preset in the virtual appliance image. If you require a specific host name, you can change the name from the hypervisor. Each DLS virtual appliance provides a shell script specifically for this purpose.
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
  1. Use the hypervisor management console of the appliance to log in as the user dls_admin to the VM that hosts the DLS appliance. If the dls_admin user has not been registered, you can still log in to the VM as the dls_admin user with the default password welcome.
  2. Run the /etc/adminscripts/set-hostname.sh script. 
Copy
Copied!

```
            
            
$ /etc/adminscripts/set-hostname.sh

        
```

  3. When prompted, enter your choice of new host name for the VM-based DLS virtual appliance.


###  [2.10.4. Expanding the Disk Space on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-disk-expansion)
You can use the management interface to the NVIDIA Licensing application to expand the disk space of the DLS virtual appliance. 
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
Perform this task on the Hypervisor where your DLS appliance is installed. 
  1. Turn off the virtual machine.
  2. Expand the virtual hard disk associated to the VM through the hypervisor console.
  3. Right-click on the VM and navigate to the **Edit Settings**.
  4. Expand the disk space from the console.
  5. Click OK to confirm.
  6. Start the virtual machine.
  7. Use the hypervisor management console of the appliance to log in as the user dls_admin to the VM that hosts the DLS virtual appliance. If the dls_admin user has not been registered, you can still log in to the VM as the dls_admin user with the default password welcome.
  8. Run the following script: /etc/adminscripts/expand_disk.sh Ignore the log message that says: 
Copy
Copied!

```
            
            
Information: you may need to update /etc/fstab

        
```

  9. Validate the disk size for the `/dev/mapper/vgnls--si--0-root` using the `df -h` command.


Note:
In the case of ESXi, Hyper V, and KVM, each DLS virtual machine should be imported from the respective image of the hypervisor. If the virtual machines are cloned or created by snapshot, you will not be able to edit or expand the disk from the hypervisor console. 
If you need to free up more disk space, you can log in as the dls_admin user and run the `sudo /etc/dls/scripts/db_data_purge.sh` command to clean up the disk. 
###  [2.11. DLS Appliance Setup Tool](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool)
The DLS Appliance Setup tool consists of standalone programs you run on a Unix computer to configure the Delegated License Service (DLS) appliance. The tools register the appliance with the NVIDIA Licensing Portal (NLP), bind it to a license allocation, install the license file on the appliance, and can configure appliance settings such as NTP, network file sharing (NFS), and LDAP. You do not need to install Python on the computer where you run these tools.
Note:
For registering the dls_admin account through the appliance user interface instead, see [Registering the DLS Administrator User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-administrator-user "Each DLS virtual appliance is configured with a user account specifically for administering the DLS. This account provides access through a web-based management interface to the NVIDIA Licensing application on the appliance. Before administering a DLS virtual appliance, you must register this user to be able to access this management interface."). 
###  [Prerequisites](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool__section_dvf_ls2_x3c)
  * Use a Unix system that can reach both the DLS appliance and the NVIDIA Licensing Portal over the network.
  * Create a license allocation on the [NVIDIA Licensing Portal](https://ui.licensing.nvidia.com/) before you run these tools. The setup tools do not create license allocations.
  * Obtain an NLP API key of type DlsInstallAutomation from the [NVIDIA Licensing Portal](https://ui.licensing.nvidia.com/) after you sign-in in a browser. You will enter this key when the tool prompts for it.


###  [Getting Started](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool__section_mxr_vs2_x3c)
You receive two archives, for example `dls_registration-<version>.tar.gz` and `dls_configuration-<version>.``<u class="ph u">tar.gz</u>[](http://tar.gz)` packaged in DLS Appliance Virtual Machine. These can be found in the home directory (“/home/dls_admin”) of “dls_admin” user.
Extract them in the current working directory (“/home/dls_admin”) on the DLS Appliance Virtual Machine OR copy them on your Unix system and run the executable from the extracted directory. 
Copy
Copied!

```
            
            
tar -xzf dls_registration-<version>.tar.gz
./dls_registration [options]
tar -xzf dls_configuration-<version>.tar.gz
./dls_configuration [options]

        
```

Replace `<version>` with the version that matches your delivery. 
###  [DLS Registration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool__section_hhk_msj_x3c)
The registration tool registers the DLS instance with the NLP, binds the DLS instance to your license allocation, and installs the license file on the appliance. 
  1. The tool prompts for an existing dls_admin password (from a previous run) or generates a new password and uses it for registration. If a new password is generated, save it when it is shown. You need it for later runs and for other operations that require dls_admin.
  2. The tool prompts for the NLP API key before it uploads service instance information.
  3. The DLS instance is registered on the NLP. If it is already registered, this step is skipped.
  4. You enter the ID of the license allocation you created on the portal.
  5. The tool binds the service instance to that license allocation. If it is already bound, this step is skipped.
  6. The tool downloads the license file from the NLP and installs it on the DLS.


Re-running the tool: You can run the tool again after it exits. Enter the password that was saved from an earlier run when prompted. Steps that are already complete (registration, bind) are skipped. 
**Command-line options (DLS Registration)**
Copy
Copied!

```
            
            
./dls_registration [--dls-url URL] [--nlp-url URL] [--org ORG] [--vg-id VG_ID] [--verbose]

        
```

Optional environment variables: `DLS_BASE_URL`, `NLP_BASE_URL`, `NLP_ORG`, `NLP_VG_ID`
If the default temporary directory is not suitable for running the packaged executable (for example because `/tmp` or `/var/tmp` is mounted `noexec`), set `TMPDIR` to a directory that is writable and executable. See _Troubleshooting_. 
Unless you supply values through options or environment variables, the tool prompts for: DLS URL, organization name, virtual group ID, NLP API key, and License Server ID. 
**Example: DLS Registration**
The following example uses placeholder values. Replace them with your own URLs, organization name, virtual group ID, license server ID, and passwords. 
Copy
Copied!

```
            
            
$ PYINSTALLER_RUNTIME_TMPDIR=${TMPDIR} ./dls_registration --nlp-url https://<nlp-url> --org <your-org> --vg-id <your-vg-id> --verbose
Enter DLS base URL (e.g. https://dls-ip): https://<dls-ip>
Enter NLP API key (from NVIDIA Licensing Portal):
INFO: DLS health check passed.
Enter dls_admin password (from a previous run), or press Enter to generate and register:
--- Save this password for future runs ---
dls_admin password: <generated-password>
--- ------------------------------------ ---
INFO: CA registration and login completed.
INFO: DLS service instance xid: <si-xid>
INFO: Uploaded SIIT to NLP.
INFO: SI registration on NLP complete: SI=<si-xid> (SIDT will be applied with license file install)
Enter License Server ID (created on NLP portal): <license-server-id>
INFO: Bound SI <si-xid> to LS <license-server-id>.
INFO: Downloaded license file from NLP.
INFO: License file installed on DLS.
INFO: DLS Registration Complete — DLS is ready to serve licenses.
--- Save this password for future runs ---
dls_admin password: <generated-password>
--- ------------------------------------ ---

        
```

###  [DLS Configuration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool__section_x3q_svj_x3c)
The configuration tool sets NTP, NFS (filesystem), TLS server certificate, TLS ciphers, the lease shutdown (return) port, and LDAP on the DLS appliance. You need the DLS base URL and the `dls_admin` password (from the registration tool or from manual appliance setup). **Command-line options (DLS Configuration)**
Copy
Copied!

```
            
            
./dls_configuration [--dls-url URL] [--skip-ntp] [--skip-nfs] [--skip-ssl-cert] [--skip-tls-ciphers] [--skip-lease-port] [--skip-ldap] [--verbose]

        
```

Optional environment variable: `DLS_BASE_URL`
After you enter the password, the tool may prompt for: NTP servers; NFS or filesystem settings (including platform type—for Windows shares, username and password, with password input hidden); TLS certificate (domain name; PEM certificate and private key from files or paste); TLS ciphers; lease shutdown (return) port; and LDAP settings. LDAP values are entered at prompts; you do not need a JSON configuration file. 
**Example: DLS Configuration**
The following example uses placeholder values. 
Copy
Copied!

```
            
            
$ ./dls_configuration --dls-url https://<dls-ip> --verbose
Enter dls_admin password:
INFO: DLS health check passed.
INFO: Logged in to DLS.
Enter NTP servers (comma-separated, or Enter to skip): pool.ntp.org
INFO: NTP ping succeeded.
INFO: NTP configuration applied.
Configure NFS/filesystem? (y/N): n
Install SSL certificate? (y/N): n
Configure TLS ciphers? (y/N): n
Configure lease shutdown (return) port? (y/N): y
Current lease shutdown port: 8080
Lease shutdown port (1024-65535) [8080]: 9090
INFO: Lease shutdown port configured.
Configure LDAP? (y/N): n
INFO: DLS Configuration Complete. Applied: NTP, Lease port

        
```

To use a Windows SMB share over NFS, answer y for NFS, choose platform windows, enter a path such as //server/share and the mount point, then enter the share username and password (password input is hidden). To configure TLS certificate, LDAP, or TLS ciphers, answer y at the corresponding prompts and follow the on-screen prompts (for example domain name and certificate or key paths or pasted PEM text for TLS; server, port, search base, and related LDAP settings). 
###  DLS and NLP base URLs 
  * DLS: Use the base URL of your DLS appliance, for example `https://dls-ip` or `https://dls-ip:8085`. If your deployment uses different ports for different services, use the gateway or main API base URL that serves the appliance, authorization, and file installation APIs.
  * NLP: The default NLP API base URL is `https://api.licensing.nvidia.com`. For other environments, set the `NLP_BASE_URL` environment variable or use the `--nlp-url` option.


  * Passwords and API keys are read from interactive prompts or environment variables; they are not written to log files or disk by these tools.
  * Communication uses HTTPS. If the DLS appliance presents a self-signed TLS certificate, the tools may disable strict certificate verification and may show a warning.
  * Authentication tokens are kept in memory only.


###  [Troubleshooting](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool__section_a5r_lyj_x3c)
Error: failed to map segment from shared object (for example involving `libz.so.1` or `libpython3.13.so`) 
The single-file executable unpacks its runtime to a temporary directory. If that directory is on a filesystem mounted `noexec`, or is otherwise not executable (sometimes true for `/tmp` or `/var/tmp`), the dynamic loader cannot map the extracted libraries. 
Set `TMPDIR` to a directory that you control, that is writable and executable, then run the tool again: 
Copy
Copied!

```
            
            
mkdir -p "$HOME/dls_extract"
export TMPDIR="$HOME/dls_extract"
./dls_registration

        
```

Use the same `TMPDIR` setting for `./dls_configuration` if you see the same error. The Unix host must also provide `libz.so.1` (zlib). 
## [3. Configuring a License Server Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-service-instance)
How you configure a service instance depends on whether the service is a Cloud License Service (CLS) instance or a Delegated License Service (DLS) instance.
To use a DLS instance, install and configure the virtual appliance as described in Installing and Configuring the DLS Virtual Appliance. You administer the appliance through the NVIDIA Licensing application on the appliance and through the NVIDIA Licensing Portal. 
  1. Optional: change the appliance name and description.
  2. Choose Deploy to DLS or (Legacy) NLL on DLS. Select a registered DLS appliance that is not used by another allocation, or register a new appliance from that option. For a connected DLS, upload the SIIT. For an air-gapped DLS, register the appliance on the portal from that same option. Deploy to DLS enables network leasing. (Legacy) NLL on DLS supports NLL leasing after the license file is installed. Selecting or registering the appliance is the same for both.
  3. Download the license file and install it on the DLS appliance.


##  DLS Instance Instructions 
You administer and manage a DLS instance through the NVIDIA Licensing application on the virtual appliance that hosts the DLS instance and through the NVIDIA Licensing Portal. 
Before configuring a DLS instance, ensure that the virtual appliance that will host the instance has been installed and configured as explained in [Installing and Configuring the DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-configuring-dls-virtual-appliance "To simplify the installation and administration of the DLS, the DLS is distributed as a software image to be installed or deployed on a supported platform. The DLS is a secure, hardened environment in which access to the application software is strictly controlled."). 
To configure a DLS instance, follow this sequence of instructions: 
  1. **Optional:**[Changing the Name and Description of a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-name-and-description-of-dls-instance "By default, a DLS instance is created with the name DEFAULT_timestamp and the description ON_PREM_SERVICE_INSTANCE. To distinguish a DLS instance on the NVIDIA Licensing Portal when multiple DLS instances are configured, change these defaults to a meaningful name and the description.")
  2. Instructions for creating a license allocation or converting a legacy NVIDIA vGPU software license allocation: 
     * [Creating a License Allocation on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-nls-license-server-on-portal "To be able to allot licenses to an NVIDIA License System instance, you must create at least one license allocation on the NVIDIA Licensing Portal. Creating a license allocation defines the set of licenses to be allotted.")
  3. Instructions for registering the DLS instance: 
     * [Registering a DLS License Server Instance on an Air-Gapped Network](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-instance-on-premises "If your DLS license server instance is on an air-gapped network, you cannot download a DLS instance token to upload to the NVIDIA Licensing Portal to register the instance. Instead, you must register the instance by entering its details manually on the NVIDIA Licensing Portal..")
     * [Registering a DLS Instance on an Air-Gapped Network with the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-instance-on-classified-network)
  4. [Installing a License Allocation on a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-license-server-on-dls-instance)


###  [3.1. Roles Required for Configuring a License Server Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#roles-for-configuring-service-instance)
Unless stated otherwise, the role that these tasks require depends on whether they are being performed for an organization or a virtual group.
  * For an organization, these tasks require the [Organization Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-administrator "An organization administrator has the highest level of visibility and access within an organization. The person that created the organization's NVIDIA Enterprise Account is initially assigned the organization administrator role.") or the [Organization User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-user "In version 3.4.0 and later, the Organization User in the default virtual group is redefined as a Virtual Group User.") role.
  * For a virtual group, these tasks requires the [Virtual Group Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-administrator "A virtual group administrator has restricted visibility within an organization and can access items and manage contacts only in the virtual group to which the virtual group administrator is assigned. A virtual group administrator is a contact that has been added to the virtual group as an admin user.") or the [Virtual Group User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-user "A virtual group user has no visibility within an organization and can view and access items only in a virtual group. A virtual group user is a contact that has been added to the virtual group as a base user.") role.


Note:
These roles are required **only** for tasks that are performed on the NVIDIA Licensing Portal. They are **not** required for tasks that are performed on the NVIDIA Licensing application on the virtual appliance that hosts a DLS instance. 
###  [3.2. Proxy Server Requirements and Firewall Rules for a License Server Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#proxy-server-requirements-firewall-rules-for-cls-instance)
To enable communication between a licensed client and a license server instance through a proxy server, the proxy server must meet certain requirements. To enable communication through a firewall, firewall rules that allow traffic on specific URLs through specific ports must be defined.
The processes for configuring a proxy server and defining firewall rules are separate from the process for configuring a license server Instance. Use the standard interfaces of the proxy server and the firewall that you are using to perform these processes. 
Note:
**Only** HTTP proxy servers are supported. HTTPS proxy servers are **not** supported. 
###  Proxy Server Requirements for a License Server Instance 
NVIDIA License System supports transparent proxy servers and non-transparent proxy servers. 
  * **A transparent proxy server** identifies itself to the server and does **not** modify client requests and responses.
  * **A non-transparent proxy server** does not reveal the IP address of the client and modifies client requests and responses.


Any proxy server between a licensed client and a license server instance must allow programmatic calls to the URL through which the license server instance is accessed.   
| License Server Instance Type  | URL  |  
| --- | --- |  
| CLS instance  | `api.cls.licensing.nvidia.com`  |  
| DLS instance  | The IP address or, if defined, the fully qualified domain name or the CNAME of the VM on which the DLS virtual appliance is installed  |  
###  Non-Transparent Proxy Server Support 
NVIDIA License System supports both authenticated and unauthenticated non-transparent proxy servers. 
The following authenticated proxy servers are supported: 
  * Squid


The following authentication methods are supported for authenticated proxy servers: 
  * Basic
  * Microsoft Windows Challenge/Response (Microsoft NTLM) (Windows clients only)
  * Kerberos (only for clients that are a member of an Active Directory domain)


###  Firewall Rules for a License Server Instance 
To enable communication between a licensed client and a license server instance through a firewall, firewall rules that allow traffic on the URLs through the ports specified in the following table must be defined.   
| CLS URL  | DLS URL  | Port  | Traffic  |  
| --- | --- | --- | --- |  
| `api.cls.licensing.nvidia.com`  | The IP address or, if defined, the fully qualified domain name or the CNAME of the VM on which the DLS virtual appliance is installed  | 443  | 
  * Licensing operations, namely, the borrowing, renewal, and return of a license.
  * Licensed client authentication

 |  
| `api.licensing.nvidia.com`  | The IP address or, if defined, the fully qualified domain name or the CNAME of the VM on which the DLS virtual appliance is installed  | 80  | License return from a Windows licensed client that has not been shut down cleanly  |  
###  [3.3. Changing the Name and Description of a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-name-and-description-of-dls-instance)
By default, a DLS instance is created with the name DEFAULT__timestamp_ and the description ON_PREM_SERVICE_INSTANCE. To distinguish a DLS instance on the NVIDIA Licensing Portal when multiple DLS instances are configured, change these defaults to a meaningful name and the description.
Perform this task from the DLS virtual appliance. 
  1. Log in to the DLS appliance and open Home.
  2. In the Appliance section, select Edit.
  3. In Edit appliance, enter the Name and Description. 
Note:
The instance name cannot contain special characters. 
The updated name and description appear in the Appliance section.
  4. Select Save.


After changing the name of a DLS instance, follow the instructions in [Creating a License Allocation on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-nls-license-server-on-portal "To be able to allot licenses to an NVIDIA License System instance, you must create at least one license allocation on the NVIDIA Licensing Portal. Creating a license allocation defines the set of licenses to be allotted."). 
###  [3.4. Creating a License Allocation on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-nls-license-server-on-portal)
To be able to allot licenses to an NVIDIA License System instance, you must create at least one license allocation on the NVIDIA Licensing Portal. Creating a license allocation defines the set of licenses to be allotted.
You can also create license allocations on the NVIDIA Licensing Portal and distribute your licenses across them as necessary, for example to group licenses functionally or geographically. 
  1. In the NVIDIA Licensing Portal, navigate to the organization or virtual group for which you want to create the license allocation. 
    1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
    2. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Info window.
If no license allocations have been created for your organization or virtual group, the NVIDIA Licensing Portal dashboard displays a message asking if you want to create a license allocation.
  2. In the left navigation pane of the NVIDIA Licensing Portal dashboard, expand **LICENSE ALLOCATION** and click **CREATE ALLOCATION**. If you are adding a license allocation to an organization or virtual group for which a license allocation has already been created, click **CREATE ALLOCATION**.
If you are adding a license allocation to an organization or virtual group for which a license allocation has already been created, click **CREATE ALLOCATION**. 
The Create License Allocation wizard opens.
  3. On the Create License Allocation page of the wizard, step through the configuration requirements to provide the details of your license allocation. 
    1. **Step 1 – Identification** : In the **Name** field, enter your choice of name for the license allocation and in the **Description** field, enter a text description of the license allocation. The description is required and will be displayed on the details page for the license allocation that you are creating.
    2. **Step 2 – Features** : Select one or more available features from your entitlements to allot to this license allocation.


Creating an allocation is the same process for every product family. Deploy to CLS and Deploy to DLS are not available for Network product families (UFM, Rivermax, and others) or for Bright. 
Click **REVIEW SUMMARY** to review the configuration summary before creating the license allocation. 
  1. On the Create License Allocation page, from the **Step 4 – Configuration** menu, click the **CREATE ALLOCATION** option to create this license allocation. Alternatively, you can click **CREATE ALLOCATION** on the Server Summary page.


After creating a license allocation on the NVIDIA Licensing Portal, follow the instructions in the topic for the type of License Server Instance that you are configuring: 
  * **CLS instance:**[index.html#creating-cls-instance-on-portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-cls-instance-on-portal)
  * **DLS instance:**[Registering a DLS License Server Instance on an Air-Gapped Network](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-instance-on-premises "If your DLS license server instance is on an air-gapped network, you cannot download a DLS instance token to upload to the NVIDIA Licensing Portal to register the instance. Instead, you must register the instance by entering its details manually on the NVIDIA Licensing Portal..")


###  [3.5. Creating or Registering a Service Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-or-registering-service-instance)
If you are hosting your service instance in the cloud on the NVIDIA Licensing Portal, you must create a CLS instance. If you are hosting your service instance at a location that is accessible from your organization's private network, you must register a DLS instance. If you are hosting your service instance in the cloud on the NVIDIA Licensing Portal, you must create a CLS instance only if you are not using the default CLS instance.
###  [3.5.1. **Registering a DLS License Server Instance on an Air-Gapped Network**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-instance-on-premises)
If your DLS license server instance is on an air-gapped network, you cannot download a DLS instance token to upload to the NVIDIA Licensing Portal to register the instance. Instead, you must register the instance by entering its details manually on the NVIDIA Licensing Portal..
  1. Log in to the DLS appliance and open **Home**.
  2. Under **Get started** , in **Register with NLS Cloud Portal** , select **Pre-register DLS appliance**.
  3. Pre-registration is irreversible. It is intended for an appliance on an air-gapped network that blocks outbound traffic. Completing this operation signs you out of the appliance.
  4. In the confirmation dialog, select **Pre-register**.
  5. Sign in again. Copy the DLS appliance ID and record the appliance name and description. Use these details to register the appliance manually in the NVIDIA Licensing Portal.
  6. In the NVIDIA Licensing Portal, navigate to the organization or virtual group for which you are registering the DLS license server instance. a. If you are not already logged in, log in to the NVIDIA Enterprise Application Hub and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal. b. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Info window..
  7. For your created license allocation, select either **Deploy to DLS** or **(Legacy) NLL on DLS** to open the **Add DLS appliance** window, select the **Air-gapped DLS** option, and complete the manual pre-registration. Alternatively, navigate to **License Allocation** in the left navigation pane of the NVIDIA Licensing Portal dashboard, choose **Register DLS for air-gapped network** from the **ACTIONS** menu, and enter the name, DLS license server instance ID, and description obtained in Step 5, then click **REGISTER**.
  8. After portal registration, bind the license allocation to the appliance, download the allocation file, and install it on the DLS appliance.
  9. After registration, return to the **Deploy to DLS** screen.
  10. Select the acknowledgement checkbox (_"I acknowledge that the downloaded license allocation file is valid for up to 3 years from the date of download"_).
  11. Select the pre-registered appliance acknowledgement checkbox (_"Important note: This license file must be retained until it is installed on [appliance name]. The system automatically acknowledges air-gapped registrations, making this initial installation file unavailable for re-download"_).
  12. Click **Download License File** to complete the workflow (no additional installation steps are required).


###  [3.5.2. Registering a DLS Instance on an Air-Gapped Network with the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-instance-on-classified-network)
If your DLS instance is on an air-gapped network, you cannot download a DLS instance token to upload to the NVIDIA Licensing Portal to register the instance. Instead, you must register the instance by entering its details manually on the NVIDIA Licensing Portal. 
  1. If you are not already logged in, log in to the NVIDIA Licensing application at the IP address of the VM on which the DLS virtual appliance is installed.
  2. In the left navigation pane of the NVIDIA Licensing dashboard, click **License Allocation**.
  3. On the License Allocation page that opens, from the **ACTIONS** menu, choose **Pre-Register Service Instance**.
  4. In the Pre-Register DLS Service Instance window that opens, click **PRE-REGISTER SERVICE INSTANCE**.
  5. On the Service Instance page, note the information about the DLS instance that you will need when you register it by entering its details manually on the NVIDIA Licensing Portal. 
     * Service Instance ID
     * Name
     * Description
  6. In the NVIDIA Licensing Portal, navigate to the organization or virtual group for which you are registering the service instance. 
    1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
    2. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Infowindow.
  7. In the left navigation pane of the NVIDIA Licensing Portal dashboard, click **License Allocation**.
  8. On License Allocations page that opens, from the **ACTIONS** menu, choose **Register DLS for air-gapped network**.
  9. In the Register DLS Instance for Air-Gapped Network window that opens, enter the name, service instance ID, and description that you obtained in Step [5](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-dls-instance-on-classified-network__get-dls-info-step), and click **REGISTER**.


After registering the DLS instance, perform the following sequence of tasks to complete the configuration of the instance: 
  1. Bind a license server to the DLS instance that you registered as explained in [index.html#binding-license-server-on-portal-to-service-instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#binding-license-server-on-portal-to-service-instance).
  2. Download and install the license server on the virtual appliance that hosts DLS instance as explained in [Installing a License Allocation on a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-license-server-on-dls-instance).


###  [3.6. Freeing a License Allocation from an Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#freeing-license-server-from-service-instance)
**To free a license allocation from an associated DLS appliance, you must contact NVIDIA Support. Only NVIDIA Support is authorized to perform the action required to free the license allocation from the appliance.**
###  [3.7. **Creating and Installing a License Allocation on an Appliance**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-license-server-on-service-instance)
To make licenses available for use, you must first create a license allocation on the NVIDIA Licensing Portal and then install it on your target appliance.
###  [3.7.1. **Creating a License Allocation**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-a-license-allocation)
  1. In the NVIDIA Licensing Portal, navigate to the organization or virtual group for which you want to create the license allocation. a. If you are not already logged in, log in to the NVIDIA Enterprise Application Hub and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal. b. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Info window.
  2. Click **Create allocations**.
  3. Select a deployment target option (for example: **Create NLL File** , **Deploy to CLS** , **Deploy to DLS** , or **NLL on DLS**(Legacy).
  4. In the **Configure allocation** section, name the allocation and add a description.
  5. Select a **Product Family** from the selection on the top.
  6. In the **Details** box, select an entitlement available in your organization.
  7. In the **Select entitlement** box, use the dropdown menu to select a feature instance.
  8. Select a key type available for the selected product family and generate a license file from the selected entitlement.


###  [3.7.2. Installing a License Allocation on a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-license-server-on-cls-instance)
To install a license allocation on a CLS instance, select the **Deploy to CLS** option.
###  [3.7.3. Installing a License Allocation on a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-license-server-on-dls-instance)
To install or update a license allocation on a DLS appliance, perform the following steps:
  1. In the NVIDIA Licensing Portal, select either **Deploy to DLS** or **(Legacy) NLL on DLS** for your created allocation.
  2. From the options provided, either select an existing registered DLS instance that is unused or register a new DLS instance using one of the supported methods (standard or air-gapped registration).
  3. Download the license allocation .bin file (the file must not exceed 20 MB).
  4. Log in to the DLS appliance and open **Home**.
  5. For the first installation, locate **No license allocation installed** on the **Home** page. In **License allocation file** , select the downloaded .bin file, and then select **Install allocation**.
  6. To update an installed allocation, open **License Allocation**. Under **Allocation controls** , select **Update License File**.
  7. In **Update License File** , choose the downloaded .bin file or drag it into the upload area (up to 20 MB). Selecting a valid file starts the update immediately; there is no separate installation confirmation.
  8. After the installation or update succeeds, open **License Allocation** and verify the allocation details.


## [4. Managing Licenses on a License Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#managing-licenses-on-license-server)
After installing a license allocation on a DLS license server instance, you can manage the licenses served from the allocation to support your licensed clients.
For example: 
  * An organization in which some users are using graphics-intensive computer-aided design (CAD) tools, while other users are using only office productivity tools can create a pool of NVIDIA RTX Virtual Workstation licenses for the CAD tool users and a pool of GRID Virtual PC licenses for the users of office productivity tools.
  * An organization with mission-critical workloads can create a dedicated license allocation for those licensed clients and a separate allocation for general use. Alternatively - the licenses could be partitioned into different virtual groups. By distributing each allocation’s client configuration token only to the intended clients, the organization can prevent general-purpose clients from consuming licenses reserved for mission-critical workloads.


When a license allocation is installed on a license server instance, you can generate a client configuration token to allow your licensed clients to be served licenses from the allocation. 
###  [4.1. Where to Perform Tasks for Managing Licenses](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#where-to-perform-tasks-for-managing-licenses)
Where to perform the tasks for managing licenses on a license allocation depends on the type of license server instance on which the license allocation is installed.
  * On a CLS instance, perform the tasks on the NVIDIA Licensing Portal.
  * On a DLS instance, perform the tasks on the NVIDIA Licensing application on the virtual appliance that hosts the DLS instance.


###  [4.2. Roles Required for Managing Licenses on a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#roles-for-managing-licenses-on-service-instance)
The role that these tasks require depends on whether they are being performed for an organization or a virtual group.
  * For an organization, these tasks require the [Organization Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-administrator "An organization administrator has the highest level of visibility and access within an organization. The person that created the organization's NVIDIA Enterprise Account is initially assigned the organization administrator role.") or the [Organization User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-user "In version 3.4.0 and later, the Organization User in the default virtual group is redefined as a Virtual Group User.") role.
  * For a virtual group, these tasks require the [Virtual Group Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-administrator "A virtual group administrator has restricted visibility within an organization and can access items and manage contacts only in the virtual group to which the virtual group administrator is assigned. A virtual group administrator is a contact that has been added to the virtual group as an admin user.") or the [Virtual Group User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-user "A virtual group user has no visibility within an organization and can view and access items only in a virtual group. A virtual group user is a contact that has been added to the virtual group as a base user.") role.


Note:
These roles are required **only** for tasks that are performed for a CLS instance on the NVIDIA Licensing Portal. They are **not** required for tasks that are performed for a DLS instance on the NVIDIA Licensing application on the virtual appliance that hosts the DLS instance. 
###  [4.3. Navigating to the License Allocation Details Page for a License Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#navigating-to-license-server-details-page)
How to navigate to the License Allocation page for an appliance depends on whether you are performing the task on the NVIDIA Licensing Portal on a DLS instance.
  1. On a DLS appliance, log in to the NVIDIA Licensing application. Home opens.
  2. Select License Allocation in the top navigation, or select View License Allocation on Home, to open the installed allocation. No further navigation is required on DLS.


###  [4.4. Generating a Client Configuration Token](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token)
A client configuration token designates the license allocation required for serving licenses upon receiving requests from licensed clients. Exchanging these details between the License Server instance and the client enables proper license serving. 
After generating a client configuration token, you copy it to each licensed client that you want to use the token. Each client then provides data from the token back to the allocation whenever the client requests a license. Create **one** client configuration token **for each** combination of license allocations and fulfillment conditions that you want to use to serve licenses in response to requests from licensed clients. 
Note:
You **cannot** generate a client configuration token for a Node-locked license allocation. Instead, generate a node-locked license as explained in[ _Generating Node-Locked Licenses_](https://sw-docs-dgx-station.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-node-locked-license). 
How to generate a client configuration token depends on whether you are generating the token for a CLS or a DLS instance. For detailed instructions, see: 
  * [Generating a Client Configuration Token for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token-for-cls-instance)
  * [Generating a Client Configuration Token for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token-for-dls-instance)


After creating a client configuration token for a license allocation, copy the token to each licensed client that should obtain licenses from that allocation. For more information, see [_Configuring a Licensed Client with a Networked License_](https://sw-docs-dgx-station.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client). 
###  [4.4.1. Generating a Client Configuration Token for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token-for-cls-instance)
  1. Log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Select the CLS license allocation for which you want to generate a client configuration token.
  4. Click **Create client config token**.
  5. **Optional:** In the **Expiration** section, select an expiration date for the client configuration token. If you do not select a date, the default token expiration time is 12 years.
  6. Click **Download Token**. A file named `client_configuration_token_mm-dd-yyyy-hh-mm-ss.tok` is saved to your default downloads folder.


###  [4.4.2. Generating a Client Configuration Token for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token-for-dls-instance)
  1. Log in to the DLS appliance and open License Allocation.
  2. Under Allocation controls, select Create client config token.
  3. In Create Client Configuration Token, choose the connection address for each DLS node. Select an available FQDN, IPv4, or IPv6 address that licensing clients can reach. When more than one address is available for the selected IP version, choose the required address.
  4. For Leasing port, select 443 (default) or 8082. Ensure that clients can reach the selected port at the connection addresses included in the token.
  5. Under Token expiration, select an Expiration date, or leave it blank to use the default expiration of 12 years. To create a token without expiration, enable Token never expires. 
Token expiration does not change entitlement expiration or the duration of an individual license lease. Expiration dates more than 12 years in the future, including Token never expires, are not recognized by NVIDIA vGPU releases 13.0 through 13.7 and 15.0 through 15.2. Review the driver compatibility notice before using these options.
  6. Select Download Token. The generated client configuration token is downloaded as a .tok file. 
The allocation scope is resolved automatically. You do not select scope references or fulfillment class references in this form.
For a DLS virtual appliance, port 443 supports both the web UI and client authentication and leasing. Port 8082 supports client authentication and leasing separately from the web UI. Select 8082 in the client configuration token when using the separate leasing port. Port 443 remains available for backward-compatible leasing.


###  [4.5. Generating Node-Locked Licenses](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-node-locked-license)
Note:
Support for node-locked licensing was introduced in NVIDIA vGPU software 15.0. It is **not** supported in earlier NVIDIA vGPU software releases. 
Note:
You can generate a node-locked license (NLL) directly from a license allocation in the NVIDIA Licensing Portal without needing to deploy or install a Cloud License Service (CLS) or Delegated License Service (DLS) instance first. 
  * **Direct Portal Generation:** One license allocation corresponds to one downloadable NLL file. Any unused feature quantity on the allocation can be returned to the entitlement during the generation process.
  * **Air-Gapped Environments:** Generating NLL files via a DLS instance remains fully supported for air-gapped networks and upgrade scenarios.


###  [Key Concepts and Expiry Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#task_hsr_jm2_vkc)
  1. **Node-Locked File Token Expiry:**
     * **Definition:** The timeframe within which the generated .bin license file must be installed on the target machine. If not installed within this window, the file becomes invalid and cannot be used.
     * **Purpose:** A security measure to prevent uninstalled NLL files from remaining exposed or misused.
     * **Expiry Options:**
       * **Default Expiry (1 Year):** Applied automatically if no other duration is selected.
       * **Custom Expiry:** Allows setting a specific expiration date (cannot extend beyond December 31, 2105).`
       * **No-Expiry:** Sets the maximum token expiry to December 31, 2105 (disabled by default).
  2. **Node-Locked License Expiry:**
     * **Definition:** The validity duration of the underlying software feature, governed by the entitlement associated with your NVIDIA product.
     * **Behavior:** Once installed within the file token expiry window, the license remains active on the client system until the feature expiration date, regardless of the token expiry setting.


###  [Procedure: Generating a Node-Locked License](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#task_n2h_gn2_vkc)
  1. In the NVIDIA Licensing Portal, navigate to the **License Allocation Details** page for the allocation from which you want to generate a node-locked license. For details, see _Navigating to the License Allocation Details Page for a License Allocation_.
  2. Ensure that the license allocation is **enabled**.
  3. Select **Create NLL File** as your deployment target (or from the **ACTIONS** menu, choose **Generate node-locked license**).
  4. In the **Generate Node-Locked License** window, configure your settings: 
     * Select your desired **NLL File Token Expiry** duration (_Default_ , _Custom_ , or _No-expiry_).
     * In the MAC address text field, enter the client MAC address and click **Add MAC Address(es)**. MAC addresses can be formatted as: 
       * Six colon-separated pairs (e.g., 00:00:5E:00:53:10)
       * Six hyphen-separated pairs (e.g., 00-00-5E-00-53-10)
       * Three period-separated quadruplets (e.g., 0000.5E00.5310)
     * Select the product features you want to include from the **Available features** drop-down list.
  5. Click **GENERATE** to create and download the license package.
  6. Extract the downloaded ZIP package (nll.zip) and install the license file locally on the client system. For client setup instructions, see _Configuring a Licensed Client with an NVIDIA vGPU Software Node-Locked License_.


###  [Existing Cloud NLL Deployments](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#task_xgq_k42_vkc)
Legacy Cloud node-locked licenses generated prior to version 4.0 appear as standard allocations in the NVIDIA Licensing Portal.
  * **Client Continuity:** Installed licenses on existing client machines continue to function normally without requiring reinstallation.
  * **Generation History:** Past generation details (configured MAC addresses and allocated features) remain visible on the Allocation Details page.
  * **Return Unused Features:** You can continue to return unused features from these legacy allocations back to your entitlement pool.
  * **Restrictions:** You **cannot** generate new NLL files from existing legacy Cloud allocations, nor can you redownload the original installation ZIP file. All new NLL file generation must take place on a newly created NLL allocation.


###  [4.6. **Disabling and Enabling Leasing for a CLS/DLS License Allocation Update**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#disabling-enabling-license-server-license-pool-fulfillment-condition)
When modifying a license allocation, you must first disable leasing before making any updates. After your modifications are complete, you must re-enable leasing to ensure that the license server instance can continue serving licenses to licensed clients.
You disable and enable leasing from the License Allocation Details page. For information about how to navigate to the License Allocation Details page, see Navigating to the License Allocation Details Page for a License Allocation. 
  * **To disable leasing:** Navigate to the Overview tab of the License Allocation Details page for the license allocation. Click **DISABLE LEASING** and, when prompted, confirm that you want to disable leasing. While leasing is disabled, the allocation cannot serve licenses to licensed clients.
  * **To enable leasing:** Navigate to the Overview tab of the License Allocation Details page for the license allocation. Click **ENABLE LEASING** and, when prompted, confirm that you want to enable leasing. The license allocation can now serve licenses to licensed clients.


###  [4.7. Editing License Allocation Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#editing-license-server-settings)
License allocation settings control how a license server instance handles licenses that have been served to licensed clients. You can edit these settings for an individual license allocation.
Note:
These settings do **not** affect license allocations that are being used for node-locked licenses, that is, license allocations for which the **Node-locked licensing mode** option is set. 
  1. Edit the settings that you want to change and click **SAVE SETTINGS**. 
    1. View the **Feature Overage** setting.  

Maximum Allowed Feature Overage
    This setting cannot be changed from its preset value and is displayed for information only. For counted licenses, it is the maximum percentage of the number of licenses available that can be leased to license clients for a limited period of time when all available licenses are checked out. For example, if you have 100 concurrent user licenses and all licenses are checked out, up to an additional 10 licenses can leased to license clients for a limited period of time. During periods when overage allowances are being used, administrative warnings may be generated.
    2. On DLS, under Lease Settings, configure Lease Duration, Maximum Lease Duration, and Minimum Lease Duration as required. On the NVIDIA Licensing Portal, in the left navigation pane, click Lease Duration Settings and edit the following settings. 
The effect of these settings depends on whether the request for a license from a client specifies how long the license shall remain valid. A request from a client specifies how long the license shall remain valid only if the `LicenseInterval` Windows registry value or Linux configuration parameter is set. For more information, refer to [Virtual GPU Client Licensing User Guide](https://docs.nvidia.com/grid/latest/grid-licensing-user-guide/index.html). 

Lease Duration
     The period of time for which a license remains valid after the license has been served if this period is not specified in the request for a license. At the end of the lease period, the license becomes invalid at the client and becomes available to be served to other licensed clients. 
This setting applies only if the request for a license from the client does not specify how long the license shall remain valid. Otherwise, this setting is ignored. 

Maximum Lease Duration
    The maximum period of time for which a license remains valid at a licensed client after the license has been served to the client.     
If the request for a license from the client specifies that the license shall remain valid for a longer period, this setting overrides the period specified in the request. 

Minimum Lease Duration
    The minimum period of time for which a license remains valid at a licensed client after the license has been served to the client.     
If the request for a license from the client specifies that the license shall remain valid for a shorter period, this setting overrides the period specified in the request.
    3. On DLS, under Lease Settings, configure Default Renewal Period and Offline Lease as required. On the NVIDIA Licensing Portal, in the left navigation pane, click Other Settings and edit the following settings.  

Default Renewal Period
     The percentage of the lease period that must elapse before a licensed client can renew a license. By renewing a license before the lease period has elapsed, a licensed client can extend its license beyond the original expiration time of the license. Extending a license ensures that if a licensed client temporarily loses network connectivity to the licensing service, there is enough time for connectivity to be restored before the license expires. 
For example, if the lease period is one day and the renewal period is 20%, the client attempts to renew its license every 4.8 hours. If network connectivity is lost, the loss of connectivity is detected during license renewal and the client has 19.2 hours in which to re-establish connectivity before its license expires. 

Offline Lease
    Enable or disable offline lease of licenses to clients. When offline lease of licenses to clients is enabled, clients can keep their licenses even when powered off.
  2. **Individual license allocation only:** On the License Allocation Details page, enable the license allocation by clicking **Enable Allocation** and, when prompted, confirm that you want to enable the license allocation. The license allocation can now serve licenses to licensed clients.


###  [4.8. Active Leases and Historical Leases](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#active-leases-and-historical-leases)
The NVIDIA License System maintains two types of lease records to provide license usage tracking and audit capabilities.
###  [4.8.1. Active Leases](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#active-leases)
**Active leases** are licenses that are currently borrowed and in use by licensed clients. These leases remain active until:
  * The client explicitly releases the lease
  * The lease expires based on its configured expiration time
  * An administrator forcefully releases the lease
  * The lease is automatically reclaimed by the lease sweeper service


Active leases provide real-time visibility into current license consumption and can be monitored through the Leases or License Allocation Details page. 
###  [4.8.2. Historical Leases](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#historical-leases)
**Historical leases** offer a comprehensive audit trail of prior license usage, displaying all leases that were either borrowed or released within the specified interval. Select the **Include active leases in duration option** , to include leases initiated before the interval start that remain active beyond the interval end, regardless of their borrowing date. This approach ensures documentation of both completed and ongoing lease activity within the chosen timeframe.
###  [4.8.2.1. **Exporting Historical Leases to CSV**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#exporting-historical-leases-to-csv)
Historical lease records can be exported to a CSV file for reporting, auditing, or archival purposes. The export operation is performed as an asynchronous background job.
#####  For DLS: 
  1. Log in to the DLS appliance and open License Allocation.
  2. Select the Lease History tab..
  3. Choose a time preset, or choose Custom and set the start and end of the reporting period.
  4. If required, select Include leases active during period..
  5. Select Export selected period. The export runs in the background, and its status appears on the Lease History page.
  6. When the status indicates that the export is ready, select Download export to download the CSV file.


#####  For CLS: 
  1. Log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Select the CLS license allocation for which you want to download the lease history.
  4. On the license-allocation page, navigate to **Lease History** , and then locate **Historical Lease Data**.
  5. Specify the time range for the data that you want to export.
  6. Click **Export CSV**.


The lease-history CSV file is saved to your default downloads folder. 
###  [4.8.2.2. Configuring the Retention Period for Historical Leases](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-the-retention-period-for-historical-leases)
The retention period for historical leases can be configured at the Service Instance level through configuration settings on Settings page. 
To configure the retention period: 
#####  For DLS: 
  1. Log in to the DLS appliance.
  2. Open Operations > Settings and select General..
  3. Under Event Settings, set Lease History Purge Time to the number of days to retain historical lease records. Enter a value from 1 to 180
  4. Select Save Settings to apply the retention period.


Note:
Longer retention periods require additional database storage capacity on DLS Appliances. Consider your storage requirements when configuring the retention period. Historical lease records are automatically removed when they exceed the configured retention period. 
If disk usage exceeds 1 GB free disk space, all lease history data will be purged to ensure critical leasing operations are not impacted. 
#####  For CLS: 
  1. Log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Find the CLS license allocation that you want to configure.
  4. Click **More CLS Actions** , and then select **Open Settings**.
  5. Select **Purge Settings**.
  6. Update the **Lease History Purge Time** property by entering the number of days for which historical lease records should be retained.
  7. Click **Save Settings** to apply the new retention period.


###  [4.8.2.3. Purging Lease Usage Metrics and Historical Leases Data](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#purging-lease-usage-metrics-and-historical-leases-data)
Purging outdated usage metrics might be necessary to clear needed disk space.
To purge outdated lease usage metrics and lease history data, follow these steps: 
  1. Use the hypervisor management console to access the VM hosting the DLS virtual appliance, and log in as the **dls_admin** user.
  2. Run the following script to remove records older than the specified number of days. Replace `<num_of_days>` with the desired retention period (for example, for 30 days): 
Copy
Copied!

```
            
            
sudo /etc/adminscripts/purge_lease_metrics.sh <num_of_days>

        
```

CAUTION:
Running this script will permanently delete lease usage metrics and lease history data older than the specified number of days. Please ensure you have reviewed your retention requirements and downloaded any necessary lease history reports before proceeding. 


###  [4.9. Manually Releasing Leases from an Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#manually-releasing-leases-from-server)
This section will describe options to manually release licenses using the License Allocation GUI if immediate license freeing is needed.
In the example where a License Client VM has been un-gracefully stopped and deleted from existence, the license will remain in-use on the allocation and will not be freed **until the lease has reached expiration**. Because of this, manual admin release from the allocation is useful and these steps will describe the procedure. 
###  [4.9.1. Manual Release of Specific Clients Licensed to a License Server Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#manual-release-specific-clients)
This section will describe how to locate and manually release specific VMs from the server.
Note:
There is a daily 10% rolling limit of client VMs that can be released manually. This 10% is based on total allocated license amount onto the server. (Example: 100 licenses allocated to the server, 10 Leases can be specifically manually released). 
####  For DLS: 
  1. Log in to the DLS appliance with an account authorized to force-release leases..
  2. Open License Allocation and select Active Leases..
  3. Locate the lease to release and select its Force release row action..
  4. Review the selected lease in the confirmation dialog, then select Force release. The affected client may need to check out a license again.


####  For CLS: 
  1. Log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Select the CLS license allocation from which you want to release leases.
  4. Select the **Active Leases** tab and scroll to **Active leases**.
  5. Select the leases that you want to release.
  6. Click **Release selected**.
  7. Confirm the release when prompted.


###  [4.9.2. Manual Force Bulk Release of All Clients Licensed to a License Server Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#manual-force-bulk-release)
This section will describe how to locate and force release all leases that are in-use on a given NVIDIA License System Service Instance.
####  For DLS: 
  1. Navigate to the DLS GUI and login with `dls_admin`.
  2. Navigate to the **Leases** tab from the left navigation pane.
  3. Use the Search Bar to search for specific License Clients to release for. To filter, you can search by: ID, Feature Name, Client Origin Ref, Client Hostname, Client MAC Addresses, or Client IP Addresses.
  4. Select a number of licenses to release and click the red **Bulk Release** button.


####  For CLS: 
  1. Log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Select the CLS license allocation from which you want to release all active leases.
  4. Select the **Active Leases** tab and scroll to **Active leases**.
  5. Click **Release all**.
  6. Confirm the release when prompted.


####  Resetting the Bulk Release Limit 
A license allocation permits up to five bulk force-release operations. After the limit is reached, reset it to restore the ability to release multiple leases simultaneously. Individual leases can still be released while bulk release is unavailable. 
Resetting the limit does not release any active leases. 
**For DLS:**
  1. Log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Select the DLS license allocation for which you want to reset the limit.
  4. Under **DLS appliance actions** , click **More DLS actions**.
  5. Select **Download force-release reset token**.
  6. Log in to the DLS appliance.
  7. When the bulk force-release limit has been reached, use the displayed upload option to upload the downloaded reset token.


The bulk force-release limit is restored. 
**For CLS:**
  1. Log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Select the CLS license allocation for which you want to reset the limit.
  4. Select the **Active Leases** tab and scroll to **Active leases**.
  5. When the bulk force-release limit has been reached, click **Reset limit** in the warning banner.
  6. In the confirmation dialog, click **Reset limit**.


The bulk force-release limit is restored to five. 
###  [4.9.3. Forcibly Releasing a Node-Locked License](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#forcibly-releasing-node-locked-license)
If a client that is licensed with a node-locked license fails or is shut down abruptly, the license remains in use and is not released **until it expires**. If you can't release the license by restarting and gracefully shutting down the client, you can use the license allocation to which the license is allotted to forcibly release it.
Note:
NVIDIA License System does not limit the number of node-locked license that you can forcibly release in a given period. 
  1. Navigate to the License Allocation Details page of the license allocation to which the license is allotted. For instructions, see [Navigating to the License Allocation Details Page for a License Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#navigating-to-license-server-details-page "How to navigate to the License Allocation page for an appliance depends on whether you are performing the task on the NVIDIA Licensing Portal on a DLS instance.").
  2. In the License Allocation Details page that opens, click the **Leases** tab.
  3. In the list of lease files on the **Leases** tab, follow the **Release** link for the license that you want to release.
  4. When asked if you want to forcibly release the license, click **FORCE RELEASE**.


###  [4.10. Supporting Non-Persistent Desktop Pools](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#supporting-non-persistent-desktop-pools)
Non-persistent desktop pools are used by VM instances with reused MAC addresses, such as VM instances created by using Citrix Machine Creation Services (MCS) or VMware Instant Clone technology. If you plan to serve licenses to non-persistent desktop pools from a license server instance, prevent the License server instance from serving multiple licenses to clients that have the same MAC address.
When a license server instance supports non-persistent desktop pools, it checks the MAC address of the client in each request to check out a license. If the request specifies the MAC address of a client that has already checked out a license, the existing checkout is cleared before the request is fulfilled. 
Note:
You **cannot** enable support for non-persistent desktop pools for a license server instance that is bound to a license allocation for which the **Node-locked licensing mode?** option is set. 
How to support non-persistent desktop pools depends on whether you want to support them from a CLS or a DLS instance. For detailed instructions, see: 
  * [Supporting Non-Persistent Desktop Pools from a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#supporting-non-persistent-desktop-pools-cls-instance)
  * [Supporting Non-Persistent Desktop Pools from a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#supporting-non-persistent-desktop-pools-dls-instance)


###  [4.10.1. Supporting Non-Persistent Desktop Pools from a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#supporting-non-persistent-desktop-pools-cls-instance)
When deploying a license allocation to a CLS instance using the **Deploy to CLS** action, you have the option to enable the Non-Persistent Desktop Pools (NPDP) flag and select the appropriate collision filter directly during the initial setup. 
Because this setting is configured at the allocation level, you can also enable or modify this setting post-installation at any point in time. 
To enable or modify support for non-persistent desktop pools on an existing CLS allocation:
  1. In the NVIDIA Licensing Portal, navigate to the organization or virtual group for which you are managing licenses. a. If you are not already logged in, log in to the NVIDIA Enterprise Application Hub and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal. b. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Info window.
  2. In the left navigation pane, click **License Allocation** and navigate to the License Allocation Details page for the specific allocation you want to modify.
  3. On the License Allocation Details page, from the **Actions** menu, choose **Toggle Non-Persistent Desktop Pools**.
  4. When asked if you want to support non-persistent desktop pools, click **ENABLE NON-PERSISTENT DESKTOP POOLS**.
  5. From the Toggle Non-Persistent Desktop Pools menu, select one of the following options to detect address conflicts and avoid duplicate leases based on your setup: 
     * **Hostname** : The host name
     * **Mac Address** : The MAC address of the client
     * **Hostname/Mac Address** : The host name or the MAC address of the client
Note:
The status of non-persistent desktop pools is displayed on the Allocation Details page. 


###  [4.10.2. Supporting Non-Persistent Desktop Pools from a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#supporting-non-persistent-desktop-pools-dls-instance)
  1. Log in to the DLS appliance and open License Allocation.
  2. Under Allocation controls, locate Non-persistent desktop pool support and turn on the toggle.
  3. Under Collision filters, select MAC to match client MAC addresses, HOSTNAME to match client host names, or both to match either identifier. Select at least one filter.
  4. Select Save to apply the configuration. Select Discard instead to abandon unsaved changes. 
The saved support status is displayed under Allocation controls.


## [5. Configuring a Licensed Client of NVIDIA License System](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client-all-license-types)
To use a licensed product, each NVIDIA License System client system must be able to obtain a license for the product either from a license server instance of NVIDIA License System or from a license file installed locally on the client system. A client system can be a VM that is configured with NVIDIA vGPU, a VM that is configured for GPU pass through, or a physical host to which a physical GPU is assigned in a bare-metal deployment.
If your DLS cluster is configured with Virtual IP Management, configure clients to use the virtual IP address instead of individual node addresses. Client configuration tokens will remain valid across failover events. See Section 2.9.x.7 Configuring Licensed Clients with Virtual IP for details. 
###  [5.1. Configuring a Licensed Client with a Networked License](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client)
A client with a network connection obtains a license by leasing it from a NVIDIA License System service instance. The service instance serves the license to the client over the network from a pool of floating licenses obtained from the NVIDIA Licensing Portal. The license is returned to the service instance when the licensed client no longer requires the license.
Note:
NVIDIA vGPU software releases earlier than 13.0 do **not** support NVIDIA License System. For full details of NVIDIA vGPU software releases that support NVIDIA License System, refer to [NVIDIA License System Release Notes](https://docs.nvidia.com/license-system/latest/nvidia-license-system-release-notes/index.html). 
Before configuring a licensed client, ensure that the following prerequisites are met: 
  * If you are using a Delegated License Service (DLS) instance to serve licenses, ensure that you are using at minimum release 3.4 of the DLS virtual appliance. Earlier releases of the DLS virtual appliance are **incompatible** with this release of NVIDIA vGPU software. 
For more information about the prerequisites for using this release of NVIDIA vGPU software, refer to . These requirements are met automatically for Cloud License Service (CLS) instances, which are hosted on the NVIDIA Licensing Portal.
  * The NVIDIA vGPU software graphics driver is installed on the client.
  * The client configuration token that you want to deploy on the client has been created from the NVIDIA Licensing Portal or the DLS as explained in [Generating a Client Configuration Token](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token).
  * Ports 443 and 80 in your firewall or proxy must be open to allow HTTPS traffic between a service instance and its licensed clients. These ports must be open for both CLS instances and DLS instances. 
Note:
For DLS releases **before** DLS 1.1, ports 8081 and 8082 were also required to be open to allow HTTPS traffic between a DLS instance and its licensed clients. Although these ports are no longer required, they remain supported for backward compatibility. 


The NVIDIA vGPU software graphics driver creates a default location in which to store the client configuration token on the client. If you want to use this location for the client configuration token and, on Windows, are configuring the client with NVIDIA vGPU, you can configure the client with default settings. Otherwise, you must configure the client with custom settings as explained in [Configuring a Licensed Client with a Networked License with Custom Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#custom-configuring-nls-licensed-network-client). 
The process for configuring a licensed client is the same for CLS and DLS instances but depends on the OS that is running on the client. 
###  [5.1.1. Configuring a Licensed Client with a Networked License on Windows with Default Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client-on-windows)
Perform this task from the client. 
  1. Copy the client configuration token to the %SystemDrive%\Program Files\NVIDIA Corporation\vGPU Licensing\ClientConfigToken folder.
  2. Restart the NvDisplayContainer service.


The NVIDIA service on the client should now automatically obtain a license from the CLS or DLS instance. 
###  [5.1.2. Configuring a Licensed Client with a Networked License on Linux with Default Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client-on-linux)
Perform this task from the client. 
  1. As root, open the file /etc/nvidia/gridd.conf in a plain-text editor, such as vi. 
Copy
Copied!

```
            
            
$ sudo vi /etc/nvidia/gridd.conf

        
```

Note:
You can create the /etc/nvidia/gridd.conf file by copying the supplied template file /etc/nvidia/gridd.conf.template. 
  2. Add the `FeatureType` configuration parameter to the file /etc/nvidia/gridd.conf on a new line as `FeatureType="_value_"`.
_value_ depends on the type of the GPU assigned to the licensed client that you are configuring.  
| GPU Type  | Value  |  
| --- | --- |  
| NVIDIA vGPU  |  1. NVIDIA vGPU software automatically selects the correct type of license based on the vGPU type.  |  
| Physical GPU  | The feature type of a GPU in pass-through mode or a bare-metal deployment: 
     * 0: NVIDIA Virtual Applications
     * 2: NVIDIA RTX Virtual Workstation
     * 4: NVIDIA Virtual Compute Server
 |  
This example shows how to configure a licensed Linux client for NVIDIA RTX Virtual Workstation.
Copy
Copied!

```
            
            
# /etc/nvidia/gridd.conf.template - Configuration file for NVIDIA Grid Daemon
…
# Description: Set Feature to be enabled
# Data type: integer
# Possible values:
# 0 => for unlicensed state
# 1 => for NVIDIA vGPU
# 2 => for NVIDIA RTX Virtual Workstation
# 4 => for NVIDIA Virtual Compute Server
FeatureType=2
...

        
```

  3. Copy the client configuration token to the /etc/nvidia/ClientConfigToken directory.
  4. Ensure that the file access modes of the client configuration token allow the owner to read, write, and execute the token, and the group and others only to read the token. 
    1. Determine the current file access modes of the client configuration token. 
Copy
Copied!

```
            
            
# ls -l _client-configuration-token-directory_

        
```

    2. If necessary, change the mode of the client configuration token to 744. 
Copy
Copied!

```
            
            
# chmod 744 _client-configuration-token-directory_/client_configuration_token_*.tok

        
```


_client-configuration-token-directory_ 
    The directory to which you copied the client configuration token in the previous step.
  5. Save your changes to the /etc/nvidia/gridd.conf file and close the file.
  6. Restart the nvidia-gridd service.


The NVIDIA service on the client should now automatically obtain a license from the CLS or DLS instance. 
###  [5.1.3. Configuring a Licensed Client with a Networked License with Custom Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#custom-configuring-nls-licensed-network-client)
NVIDIA License System provides custom settings for the following configuration properties of a licensed client: 
  * The feature type of a physical GPU
  * The directory in which to store the client configuration token
  * Details for a proxy server between a licensed client and a service instance


If you want to use the default directory in which to store the client configuration token and, on Windows, are configuring the client with NVIDIA vGPU, follow the instructions for a simplified configuration in [Configuring a Licensed Client with a Networked License](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client "A client with a network connection obtains a license by leasing it from a NVIDIA License System service instance. The service instance serves the license to the client over the network from a pool of floating licenses obtained from the NVIDIA Licensing Portal. The license is returned to the service instance when the licensed client no longer requires the license."). 
###  [5.1.3.1. Configuring a Licensed Client with a Networked License on Windows with Custom Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#custom-configuring-nls-licensed-network-client-on-windows)
Perform this task from the client. 
  1. **Physical GPUs only:** Add the `FeatureType` DWord (`REG_DWORD`) registry value to the Windows registry key `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\nvlddmkm\Global\GridLicensing`. 
Note:
     * If you're licensing an NVIDIA vGPU, the `FeatureType` DWord (`REG_DWORD`) registry value is **not** required. NVIDIA vGPU software automatically selects the correct type of license based on the vGPU type.
     * If you are upgrading an existing driver, this value is already set.
     * For NVIDIA vGPU software releases before 15.0, add the registry value to the Windows registry key `HKEY_LOCAL_MACHINE\SOFTWARE\NVIDIA Corporation\Global\GridLicensing`.
Set this value to the feature type of a GPU in pass-through mode or a bare-metal deployment:
     * 0: NVIDIA Virtual Applications
     * 2: NVIDIA RTX Virtual Workstation
     * 4: NVIDIA Virtual Compute Server
  2. **Optional:** If you want store the client configuration token in a custom location, add the `ClientConfigTokenPath` String (`REG_SZ`) registry value to the Windows registry key `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\nvlddmkm\Global\GridLicensing`. 
Note:
For NVIDIA vGPU software releases before 15.0, add the registry value to the Windows registry key `HKEY_LOCAL_MACHINE\SOFTWARE\NVIDIA Corporation\Global\GridLicensing`. 
Set the value to the full path to the folder in which you want to store the client configuration token for the client. You can use the syntax \_fully-qualified-domain-name_ \_share-name_ for the path to the folder. By default, the client searches for the client configuration token in the %SystemDrive%\Program Files\NVIDIA Corporation\vGPU Licensing\ClientConfigToken folder.
By specifying a shared network drive mapped on the client, you can simplify the deployment of the same client configuration token on multiple clients. Instead of copying the client configuration token to each client individually, you can keep only one copy in the shared network drive.
  3. If you are storing the client configuration token in a custom location, create the folder in which you want to store the client configuration token. 
If the folder is a shared network drive, ensure that the following conditions are met:
     * The folder is mapped locally on the client to the path specified in the `ClientConfigTokenPath` registry value.
     * The `COMPUTER` object has the rights to access the folder on the shared network drive. The `COMPUTER` object requires these rights because the license service runs before any user logs in.
If you are storing the client configuration token in the default location, omit this step. The default folder in which the client configuration token is stored is created automatically after the NVIDIA vGPU software graphics driver is installed.
  4. Copy the client configuration token to the folder in which you want to store the client configuration token. Ensure that this folder contains only the client configuration token that you want to deploy on the client and no other files or folders. If the folder contains more than one client configuration token, the client uses the newest client configuration token in the folder. 
     * If you want to store the client configuration token in the default location, copy the client configuration token to the %SystemDrive%\Program Files\NVIDIA Corporation\vGPU Licensing\ClientConfigToken folder.
     * If you want to store the client configuration token in a custom location, copy the token to the folder that you created in the previous step.
  5. **Optional:** If you want the licensed client to check out a license when a user logs in to the client, add the `EnableLicenseOnLogin` DWord (`REG_DWORD`) registry value to the Windows registry key `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\nvlddmkm\Global\GridLicensing` and set this registry value to 1. By default, a licensed client checks out a license when the client is booted.
  6. If a non-transparent proxy server is configured between your licensed client and a service instance, provide the information about the proxy server that the licensed client requires. 
Provide this information by adding the following registry values to the Windows registry key `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\nvlddmkm\Global\GridLicensing`.
Note:
For NVIDIA vGPU software releases before 15.0, add the registry value to the Windows registry key `HKEY_LOCAL_MACHINE\SOFTWARE\NVIDIA Corporation\Global\GridLicensing`. 
    1. For all non-transparent proxy servers, provide the address and port number of the proxy server in the following Windows registry values.  

`ProxyServerAddress` String `(REG_SZ)` 
     The address of the proxy server. The address can be a fully qualified domain name such as `iproxy1.example.com`, or an IP address such as `10.31.20.45`.  

`ProxyServerPort` String `(REG_SZ)` 
    The port number of the proxy server.
    2. If necessary, provide the credentials of the user that will log in to the proxy server. 
This information is required for proxy servers that use the following authentication methods:
       * Basic authentication
       * Microsoft Windows Challenge/Response (Microsoft NTLM) authentication for a client that is not a member of an Active Directory domain 

`ProxyUserName` String `(REG_SZ)` 
    The username of the user that will log in to the proxy server. 

`ProxyCredentialsFilePath` String `(REG_SZ)` 
     The full path to the file that contains the encrypted credentials of the user that will log in to the proxy server, for example, C:\Program Files\NVIDIA Corporation\vGPU Licensing\ProxySettings\proxy-credentials.dat. 
This file is generated as explained in [Generating an Encrypted Credentials File](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-an-encrypted-credentials-file "Some authentication methods require a licensed client to provide user credentials when the client authenticates with a proxy server. To enable the client to provide these credentials securely without input from a user, you must generate a file that contains these credentials in an encrypted form that the client can read.").
  7. Restart the NvDisplayContainer service.


The NVIDIA service on the client should now automatically obtain a license from the CLS or DLS instance. 
###  [5.1.3.2. Configuring a Licensed Client with a Networked License on Linux with Custom Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#custom-configuring-nls-licensed-network-client-on-linux)
Perform this task from the client. 
  1. As root, open the file /etc/nvidia/gridd.conf in a plain-text editor, such as vi. 
Copy
Copied!

```
            
            
$ sudo vi /etc/nvidia/gridd.conf

        
```

Note:
You can create the /etc/nvidia/gridd.conf file by copying the supplied template file /etc/nvidia/gridd.conf.template. 
  2. Add the `FeatureType` configuration parameter to the file /etc/nvidia/gridd.conf on a new line as `FeatureType="_value_"`.
_value_ depends on the type of the GPU assigned to the licensed client that you are configuring.  
| GPU Type  | Value  |  
| --- | --- |  
| NVIDIA vGPU  |  1. NVIDIA vGPU software automatically selects the correct type of license based on the vGPU type.  |  
| Physical GPU  | The feature type of a GPU in pass-through mode or a bare-metal deployment: 
     * 0: NVIDIA Virtual Applications
     * 2: NVIDIA RTX Virtual Workstation
     * 4: NVIDIA Virtual Compute Server
 |  
This example shows how to configure a licensed Linux client for NVIDIA RTX Virtual Workstation.
Copy
Copied!

```
            
            
# /etc/nvidia/gridd.conf.template - Configuration file for NVIDIA Grid Daemon
…
# Description: Set Feature to be enabled
# Data type: integer
# Possible values:
# 0 => for unlicensed state
# 1 => for NVIDIA vGPU
# 2 => for NVIDIA RTX Virtual Workstation
# 4 => for NVIDIA Virtual Compute Server
FeatureType=2
...

        
```

  3. **Optional:** If you want store the client configuration token in a custom location, add the `ClientConfigTokenPath` configuration parameter to the file /etc/nvidia/gridd.conf on a new line as `ClientConfigTokenPath="_path_"` 

_path_ 
    The full path to the directory in which you want to store the client configuration token for the client. By default, the client searches for the client configuration token in the /etc/nvidia/ClientConfigToken/ directory.
By specifying a shared network directory that is mounted locally on the client, you can simplify the deployment of the same client configuration token on multiple clients. Instead of copying the client configuration token to each client individually, you can keep only one copy in the shared network directory.
This example shows how to configure a licensed Linux client to search for the client configuration token in the /mnt/nvidia/ClientConfigToken/ directory. This directory is a mount point on the client for a shared network directory.
Copy
Copied!

```
            
            
# /etc/nvidia/gridd.conf.template - Configuration file for NVIDIA Grid Daemon
…
ClientConfigTokenPath=/mnt/nvidia/ClientConfigToken/
...

        
```

  4. If you are storing the client configuration token in a custom location, create the directory in which you want to store the client configuration token. 
If the directory is a shared network directory, ensure that it is mounted locally on the client at the path specified in the `ClientConfigTokenPath` configuration parameter.
If you are storing the client configuration token in the default location, omit this step. The default directory in which the client configuration token is stored is created automatically after the NVIDIA vGPU software graphics driver is installed.
  5. Copy the client configuration token to the directory in which you want to store the client configuration token. Ensure that this directory contains only the client configuration token that you want to deploy on the client and no other files or directories. If the directory contains more than one client configuration token, the client uses the newest client configuration token in the directory. 
     * If you want to store the client configuration token in the default location, copy the client configuration token to the /etc/nvidia/ClientConfigToken directory.
     * If you want to store the client configuration token in a custom location, copy the token to the directory that you created in the previous step.
  6. Ensure that the file access modes of the client configuration token allow the owner to read, write, and execute the token, and the group and others only to read the token. 
    1. Determine the current file access modes of the client configuration token. 
Copy
Copied!

```
            
            
# ls -l _client-configuration-token-directory_

        
```

    2. If necessary, change the mode of the client configuration token to 744. 
Copy
Copied!

```
            
            
# chmod 744 _client-configuration-token-directory_/client_configuration_token_*.tok

        
```


_client-configuration-token-directory_ 
    The directory to which you copied the client configuration token in the previous step.
  7. **Optional:** If you want the licensed client to check out a license when a user logs in to the client, add the `EnableLicenseOnLogin` configuration parameter to the file /etc/nvidia/gridd.conf on a new line as `EnableLicenseOnLogin=TRUE`. By default, a licensed client checks out a license when the client is booted.
  8. If a non-transparent proxy server is configured between your licensed client and a service instance, provide the information about the proxy server that the licensed client requires. 
    1. For all non-transparent proxy servers, provide the address and port number of the proxy server. 
Provide this information by adding the following configuration parameters to the file /etc/nvidia/gridd.conf on separate lines.
Copy
Copied!

```
            
            
ProxyServerAddress=_address_
ProxyServerPort=_port_

        
```


_address_ 
     The address of the proxy server. The address can be a fully qualified domain name such as `iproxy1.example.com`, or an IP address such as `10.31.20.45`.  

_port_ 
    The port number of the proxy server.
This example sets the address of a proxy server to `10.31.20.45` and the port number to `3128`.
Copy
Copied!

```
            
            
# /etc/nvidia/gridd.conf.template - Configuration file for NVIDIA Grid Daemon
…
ProxyServerAddress=10.31.20.45
ProxyServerPort=3128
...

        
```

    2. If necessary, provide the credentials of the user that will log in to the proxy server. 
This information is required for proxy servers that use basic authentication.
Provide this information by adding the following configuration parameters to the file /etc/nvidia/gridd.conf on separate lines.
Copy
Copied!

```
            
            
ProxyUserName=_domain_\_username_
ProxyCredentialsFilePath=_path_

        
```


_domain_ 
     The domain to which the user belongs, for example, `example.com`.  

_username_ 
     The username of the user that will log in to the proxy server, for example, `clsuser`.  

_path_ 
     The full path to the file that contains the encrypted credentials of the user that will log in to the proxy server, for example, /etc/nvidia/proxy-credentials.dat. 
This file is generated as explained in [Generating an Encrypted Credentials File](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-an-encrypted-credentials-file "Some authentication methods require a licensed client to provide user credentials when the client authenticates with a proxy server. To enable the client to provide these credentials securely without input from a user, you must generate a file that contains these credentials in an encrypted form that the client can read.").
This example sets the domain and username of the user that will log in to the proxy server to `example.com\clsuser` and the path to the file that contains the encrypted credentials of the user to /etc/nvidia/proxy-credentials.dat.
Copy
Copied!

```
            
            
# /etc/nvidia/gridd.conf.template - Configuration file for NVIDIA Grid Daemon
…
ProxyUserName=example.com\clsuser
ProxyCredentialsFilePath=/etc/nvidia/proxy-credentials.dat
...

        
```

  9. Save your changes to the /etc/nvidia/gridd.conf file and close the file.
  10. Restart the nvidia-gridd service.


The NVIDIA service on the client should now automatically obtain a license from the CLS or DLS instance. 
###  [5.1.4. Generating an Encrypted Credentials File](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-an-encrypted-credentials-file)
Some authentication methods require a licensed client to provide user credentials when the client authenticates with a proxy server. To enable the client to provide these credentials securely without input from a user, you must generate a file that contains these credentials in an encrypted form that the client can read.
The following authentication methods require an encrypted credentials file: 
  * Basic authentication
  * Microsoft Windows Challenge/Response (NTLM) authentication for a client that is not a member of an Active Directory domain


How to generate an encrypted credentials file depends on the OS that client is running. For detailed instructions, refer to the following topics: 
  * [Generating an Encrypted Credentials File on Windows](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-an-encrypted-credentials-file-on-windows)
  * [Generating an Encrypted Credentials File on Linux](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-an-encrypted-credentials-file-on-linux)


###  [5.1.4.1. Generating an Encrypted Credentials File on Windows](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-an-encrypted-credentials-file-on-windows)
Perform this task in a Windows PowerShell window as the Administrator user on the client. 
  1. Change to the C:\Program Files\NVIDIA Corporation\vGPU Licensing\ProxySettings folder. 
Copy
Copied!

```
            
            
PS C:\> cd "C:\Program Files\NVIDIA Corporation\vGPU Licensing\ProxySettings"

        
```

  2. Run the grid-proxy-credentials Windows PowerShell script. 
Copy
Copied!

```
            
            
PS C:\> .\grid-proxy-credentials.ps1

        
```

  3. In the Select Output File Path window that opens, navigate to the directory in which you want to generate the credentials file, enter the file name, and click **Save**. 
  4. When prompted in the Windows PowerShell window, specify the password of the user that will log in to the proxy server when the licensed client requests a license.


Provide the path to this file when configuring a licensed client that will use the file as explained in [Configuring a Licensed Client with a Networked License on Windows with Default Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client-on-windows). 
###  [5.1.4.2. Generating an Encrypted Credentials File on Linux](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-an-encrypted-credentials-file-on-linux)
Perform this task in a Linux command shell on the client. 
  1. Run the grid-proxy-credentials.sh command. 
Copy
Copied!

```
            
            
# /usr/lib/nvidia/grid-proxy-credentials.sh -o _output-file-path_

        
```


_output-file-path_ 
    The full path to the credentials file that you are generating. Ensure that the directory in the path exists.
Tip:
To get help information for this command, type /usr/lib/nvidia/grid-proxy-credentials.sh --help. 
This example creates the credentials file /etc/nvidia/proxy-credentials.dat. 
Copy
Copied!

```
            
            
# /usr/lib/nvidia/grid-proxy-credentials.sh -o /etc/nvidia/proxy-credentials.dat

        
```

  2. When prompted, specify the password of the user that will log in to the proxy server when the licensed client requests a license.


Provide the path to this file when configuring a licensed client that will use the file as explained in [Configuring a Licensed Client with a Networked License on Linux with Default Settings](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client-on-linux). 
###  [5.2. Configuring a Licensed Client with an NVIDIA vGPU Software Node-Locked License](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client-with-node-locked-license)
A client system without a network connection or on an air-gapped network can obtain a node-locked NVIDIA vGPU software license from a file installed locally on the client system.
Note:
Support for node-locked licensing was introduced in NVIDIA vGPU software 15.0. It is **not** supported in earlier NVIDIA vGPU software releases. 
Before configuring a licensed client with a node-locked license, ensure that the following prerequisites are met: 
  * The NVIDIA vGPU software graphics driver is installed on the client.
  * The ZIP archive that contains the node-locked license file with which you want to license the client has been generated and downloaded, and its contents extracted as explained in [Generating Node-Locked Licenses](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-node-locked-license).
  * All users can read the extracted node-locked license files.


The NVIDIA vGPU software graphics driver creates a default location in which to store the license file on the client. You can specify a custom location for the license file by adding a registry value on Windows or by setting a configuration parameter on Linux. 
The process for configuring a licensed client with a node-locked license depends on the OS that is running on the client. 
###  [5.2.1. Configuring a Licensed Client with a Node-Locked License on Windows](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-node-locked-license-on-windows)
Perform this task from the VM or physical host. 
  1. **Optional:** If you want to store the license file in a custom location, create a local folder on the VM or physical host in which to store the license file. 
If you want to store the license file in the default location, omit this step. The default folder in which the license file is stored is created automatically after the NVIDIA vGPU software graphics driver is installed.
  2. **Optional:** If you want store the license file in a custom location, add the `LicenseFilePath` String (`REG_SZ`) registry value to the Windows registry key `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\nvlddmkm\Global\GridLicensing`. 
Note:
For NVIDIA vGPU software releases before 15.0, add the registry value to the Windows registry key `HKEY_LOCAL_MACHINE\SOFTWARE\NVIDIA Corporation\Global\GridLicensing`. 
Set the value to the full path to the folder that you created in the previous step.
If you want to store the license file in the default location, omit this step. By default, the client searches for the license file in the %SystemDrive%\Program Files\NVIDIA Corporation\vGPU Licensing\License folder.
  3. Use the index file named index.txt to identify the license file for the client from its MAC address. This example shows the index.txt file that identifies the license file generated for each client in the following table.   
| Client MAC Address  | License File Name  |  
| --- | --- |  
| `00:00:5E:00:53:10`  | 1a6e8f90-599e-4459-b2e0-06f67fc183fd.lic  |  
| `00:00:5E:00:53:27`  | 32894809-3f79-4461-9d53-100b7f6bc338.lic  |  
Copy
Copied!

```
            
            
[{"id": "VM0", "pevi": [{"id": "MAC", "val": "00:00:5E:00:53:27", "mod": null}], "file_name": "32894809-3f79-4461-9d53-100b7f6bc338.lic"}, 
{"id": "VM1", "pevi": [{"id": "MAC", "val": "00:00:5E:00:53:10", "mod": null}], "file_name": "1a6e8f90-599e-4459-b2e0-06f67fc183fd.lic"}]

        
```

  4. Copy the NVIDIA license .lic file that was generated for the licensed client to a local folder on the client. Ensure that this folder contains only the license file that you want to deploy on the client and no other files or folders. If the folder contains more than one license file, the client uses the newest license file in the folder. 
     * If you want to store the license file in the default location, copy the license file to the %SystemDrive%\Program Files\NVIDIA Corporation\vGPU Licensing\License folder.
     * If you want to store the license file in a custom location, copy the license file to the folder that you specified in the previous step.
  5. Reboot the VM or physical host.


The NVIDIA service on the VM or physical host should now automatically obtain a license from the license file. 
After a Windows VM or physical host has been configured with a node-locked NVIDIA vGPU software license, options for configuring licensing for a network-based license allocation are no longer available in NVIDIA Control Panel. 
###  [5.2.2. Configuring a Licensed Client with a Node-Locked License on Linux](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-node-locked-license-on-linux)
Perform this task from the VM or physical host. 
  1. **Optional:** If you want to store the license file in a custom location, create a local directory on the VM or physical host in which to store the license file. 
If you are storing the license file in the default location, omit this step. The default directory in which the license file is stored is created automatically after the NVIDIA vGPU software graphics driver is installed.
  2. **Optional:** If you want store the license file in a custom location, add the `LicenseFilePath` configuration parameter to the file /etc/nvidia/gridd.conf. If you want to store the license file in the default location, omit this step. By default, the client searches for the license file in the /etc/nvidia/vGPULicense directory. 
    1. As root, open the file /etc/nvidia/gridd.conf in a plain-text editor, such as vi. 
Copy
Copied!

```
            
            
$ sudo vi /etc/nvidia/gridd.conf

        
```

Note:
You can create the /etc/nvidia/gridd.conf file by copying the supplied template file /etc/nvidia/gridd.conf.template. 
    2. Add the following line to the file /etc/nvidia/gridd.conf. 
Copy
Copied!

```
            
            
LicenseFilePath="_path_"

        
```


_path_ 
    The full path to the directory that you created in the previous step.
This example shows how to configure a licensed Linux client to search for the license file in the /etc/nvidia/nll directory.
Copy
Copied!

```
            
            
# /etc/nvidia/gridd.conf.template - Configuration file for NVIDIA Grid Daemon
…
# Description: Used to specify the directory path which is checked for node-locked licensing files.
# Data type: string
# Format: "<directory path>"
LicenseFilePath="/etc/nvidia/nll"
...

        
```

    3. Save your changes to the /etc/nvidia/gridd.conf file and close the file.
  3. Use the index file named index.txt to identify the license file for the client from its MAC address. This example shows the index.txt file that identifies the license file generated for each client in the following table.   
| Client MAC Address  | License File Name  |  
| --- | --- |  
| `00:00:5E:00:53:10`  | 1a6e8f90-599e-4459-b2e0-06f67fc183fd.lic  |  
| `00:00:5E:00:53:27`  | 32894809-3f79-4461-9d53-100b7f6bc338.lic  |  
Copy
Copied!

```
            
            
[{"id": "VM0", "pevi": [{"id": "MAC", "val": "00:00:5E:00:53:27", "mod": null}], "file_name": "32894809-3f79-4461-9d53-100b7f6bc338.lic"}, 
{"id": "VM1", "pevi": [{"id": "MAC", "val": "00:00:5E:00:53:10", "mod": null}], "file_name": "1a6e8f90-599e-4459-b2e0-06f67fc183fd.lic"}]

        
```

  4. Copy the NVIDIA license .lic file that was generated for the licensed client to a local directory on the client. Ensure that this directory contains only the license file that you want to deploy on the client and no other files or directories. If the directory contains more than one license file, the client uses the newest license file in the directory. 
     * If you want to store the license file in the default location, copy the license file to the /etc/nvidia/vGPULicense directory.
     * If you want to store the license file in a custom location, copy the license file to the directory that you specified in the previous step.
  5. Ensure that the file access modes of the license file allow the owner to read, write, and execute the file, and the group and others only to read the file. 
    1. Determine the current file access modes of the license file. 
Copy
Copied!

```
            
            
# ls -l _license-file-directory_

        
```

    2. If necessary, change the mode of the license file to 744. 
Copy
Copied!

```
            
            
# chmod 744 _license-file-directory_/*.lic

        
```


_license-file-directory_ 
    The directory to which you copied the license file in the previous step.
  6. Reboot the VM or physical host.


The NVIDIA service on the VM or physical host should now automatically obtain a license from the license file. 
After a Linux VM or physical host has been configured with a file-based NVIDIA vGPU software license, options for configuring licensing for a networked license allocation are no longer available in NVIDIA X Server Settings. 
###  [5.3. Verifying the NVIDIA vGPU Software License Status of a Licensed Client](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#verifying-license-status)
After configuring a client with an NVIDIA vGPU software license, verify the license status by displaying the licensed product name and status.
To verify the license status of a licensed client, run nvidia-smi with the –q or --query optionfrom the licensed client, **not** the hypervisor host. If the product is licensed, the expiration date is shown in the license status. 
Expand
Copy
Copied!

```
            
            
nvidia-smi -q
==============NVSMI LOG==============

Timestamp                                 : Wed Nov 23 10:52:59 2022
Driver Version                            : 525.60.06
CUDA Version                              : 12.0

Attached GPUs                             : 2
GPU 00000000:02:03.0
    Product Name                          : 
    Product Brand                         : NVIDIA RTX Virtual Workstation
    Product Architecture                  : Ampere
    Display Mode                          : Enabled
    Display Active                        : Disabled
    Persistence Mode                      : Enabled
    MIG Mode
        Current                           : Disabled
        Pending                           : Disabled
    Accounting Mode                       : Disabled
    Accounting Mode Buffer Size           : 4000
    Driver Model
        Current                           : N/A
        Pending                           : N/A
    Serial Number                         : N/A
    GPU UUID                              : GPU-ba5b1e9b-1dd3-11b2-be4f-98ef552f4216
    Minor Number                          : 0
    VBIOS Version                         : 00.00.00.00.00
    MultiGPU Board                        : No
    Board ID                              : 0x203
    Board Part Number                     : N/A
    GPU Part Number                       : 25B6-890-A1
    Module ID                             : N/A
    Inforom Version
        Image Version                     : N/A
        OEM Object                        : N/A
        ECC Object                        : N/A
        Power Management Object           : N/A
    GPU Operation Mode
        Current                           : N/A
        Pending                           : N/A
    GSP Firmware Version                  : N/A
    GPU Virtualization Mode
        Virtualization Mode               : VGPU
        Host VGPU Mode                    : N/A
    **vGPU Software Licensed Product Product Name : NVIDIA RTX Virtual Workstation License Status : Licensed (Expiry: 2022-11-23 10:41:16 GMT)**
    …
    …

        
```

## [6. Administering a License Server Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#administering-service-instance)
Perform these routine administration tasks as needed during the lifetime of the service instance after completing the initial configuration of the service instance.
###  [6.1. Migrating a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance)
Migrating a DLS instance simplifies the upgrade of a DLS appliance. After installing a new version of the DLS appliance, you can transfer the license servers, user registration, IP address, and service instance from the existing DLS appliance to a new DLS appliance. However, event records on the existing DLS appliance are **not** migrated.
Note:
In case of failure during in-place upgrade, if you restore the previous snapshot to use the old version of DLS Appliance virtual machine, a banner message as below will appear: 
_A change in the underlying infrastructure has been detected, and hence the Client Configuration Token generation is disabled. To enable it: Use selected Cloud allocation > More DLS actions > Download snapshot reset token; follow the supported local recovery/import step. _
This banner message will be displayed on the user interface until the user uploads the DLS Appliance Snapshot Reset Token. This token should be downloaded from the Action’s menu of the relevant Service Instance on the Service Instance Page of Nvidia Licensing Portal. Use these steps to download the DLS Appliance Snapshot Reset token: 
  1. Open Operations > Upgrade. The Upgrade & Migration page opens..
  2. In the In-Place Upgrade card, click Start Upgrade.
  3. In the Confirm In-Place Upgrade dialog, click Confirm Upgrade.


Note:
Virtual IP configuration is preserved during migration and automatically restored on the new instance. No additional steps are required to maintain Virtual IP functionality after migration. 
Note:
See [DLS Appliance Setup Tool](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-appliance-setup-tool "The DLS Appliance Setup tool consists of standalone programs you run on a Unix computer to configure the Delegated License Service \(DLS\) appliance. The tools register the appliance with the NVIDIA Licensing Portal \(NLP\), bind it to a license allocation, install the license file on the appliance, and can configure appliance settings such as NTP, network file sharing \(NFS\), and LDAP. You do not need to install Python on the computer where you run these tools.") for more information. 
Important:
**Automatic Conversion from Advance Mode to Standard Mode (NLS 4.0 or Later)**
Starting with NLS version 4.0.0, **Advance leasing mode is deprecated**. All license server instances operate exclusively in **Standard leasing mode**. 
If you upgrade an existing DLS appliance running in Advance mode to version 4.0 or higher, the appliance automatically converts to Standard mode during the upgrade process: 
  * **Lease Continuity:** Active client leases are preserved and moved directly onto the server's main features. Client licensing continues uninterrupted.
  * **Pool Consolidation:** Multiple license pools are merged into a single combined feature pool.
  * **Condition Consolidation:** Multiple fulfillment conditions are simplified into one universal condition.
  * **Management Impact:** You can no longer segment clients across multiple pools or manage fulfillment conditions after the upgrade.


###  Considerations for Migrating an HA Cluster of DLS Instances 
If you are upgrading the DLS appliance for the DLS instances in an HA cluster, migrate **only** the primary instance. During the migration process, all data is removed from the secondary DLS instance and the instance is removed from the cluster. After completing the migration process for the primary instance, you can configure an HA cluster from the new primary instance. 
###  Types of Migrations Supported by NVIDIA License System 
The type of migration to perform depends on whether the new version of the DLS virtual appliance is a software update that contains security updates and bug fixes or is a new major version.  

In-place upgrade
     An in-place upgrade enables you apply a software update to a DLS appliance when NVIDIA provides security updates and bug fixes. Upgrading a DLS appliance in place overinstalls the software updates on the existing appliance without any disruption in service. All data on the appliance is preserved during the upgrade. Because the updates are overinstalled on the existing appliance, you **cannot** change the platform type of a DLS appliance by performing an in-place upgrade. 
Note:
In-place upgrades are supported starting with NVIDIA License System 3.0.0. NVIDIA License System versions earlier than 3.0.0 do not support in-place upgrades.  

File Based Upgrade
    This is a new form of Upgrade that replaces Portal Assisted Upgrade and is available from DLS Virtual Appliance 3.3.0 onwards. In this Upgrade, you need not upload any file to the NVIDIA Licensing Portal.
Note:
There could be some downtime associated with performing a File Based Upgrade. 
###  Before You Begin 
Ensure that the port mappings for ports 8081 and 8084 are not changed from their defaults. 
To prevent loss of data if a migration fails, create a snapshot of your DLS appliance before migrating it. If a migration fails, you can restore your DLS appliance from the snapshot. For information about how to create a snapshot and restore a DLS appliance from the snapshot, refer to the instructions for your appliance type in the following table.   
| Appliance Type  | Creating a Snapshot  | Restoring an Appliance  |  
| --- | --- | --- |  
| Citrix Hypervisor VM  | [Restore a VM to its previous state](https://docs.citrix.com/en-us/citrix-hypervisor/dr/snapshots.html#restore-a-vm-to-its-previous-state)  |  
| Linux Kernel-based Virtual Machine (KVM) VM  |  [How to create snapshots of QEMU/KVM guests](https://linuxconfig.org/how-to-create-snapshots-of-qemu-kvm-guests) (linuxconfig.org)  |  [How to create snapshots of QEMU/KVM guests](https://linuxconfig.org/how-to-create-snapshots-of-qemu-kvm-guests) (linuxconfig.org)  |  
| Microsoft Windows Server with Hyper-V VM  | [Using checkpoints to revert virtual machines to a previous state](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/checkpoints)  |  
| Red Hat Enterprise Linux Kernel-based Virtual Machine (KVM) VM  |  
| Red Hat Virtualization VM  |  
| Ubuntu Hypervisor VM  |  [How to create snapshots of QEMU/KVM guests](https://linuxconfig.org/how-to-create-snapshots-of-qemu-kvm-guests) (linuxconfig.org)  |  [How to create snapshots of QEMU/KVM guests](https://linuxconfig.org/how-to-create-snapshots-of-qemu-kvm-guests) (linuxconfig.org)  |  
| VMware vSphere Hypervisor (ESXi) VM  | [Take a Snapshot in the VMware Host Client](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.hostclient.doc/GUID-A0D8E8E7-629B-466D-A50C-38705ACA7613.html)  |  
| Container  | Back up the DLS container volume  | Stop the DLS container, restore the DLS container volume, and start the container again.  |  
###  Considerations for Migrating a DLS 1.0.0 or 1.0.1 Instance 
If you are migrating a DLS 1.0.0 or 1.0.1 instance, the version shown on the VA Upgrade Job Progress page is 1.0.0 to preserve backwards compatibility. 
After migrating a DLS 1.0.0 or 1.0.1 instance, you must change the password for the `dls_admin` user to enable or disable the `dls_system` user on the new instance. 
###  Instructions for Upgrading from Earlier Releases 
The instructions in this document explain how to upgrade from this release to a later release. For upgrading from an earlier release, follow the instructions specified for that release: 
  * [Instructions for upgrading from DLS 3.1.0](https://docs.nvidia.com/license-system/dls/3.1.0/nvidia-dls-user-guide/index.html#migrating-dls-instance-offline)
  * [Instructions for upgrading from DLS 3.0.0](https://docs.nvidia.com/license-system/dls/3.0.0/nvidia-dls-user-guide/index.html#migrating-dls-instance-offline)
  * [Instructions for upgrading from DLS 2.1.0](https://docs.nvidia.com/license-system/dls/2.1.0/nvidia-dls-user-guide/index.html#migrating-dls-instance-online)
  * [Instructions for upgrading from DLS 2.0.1](https://docs.nvidia.com/license-system/dls/2.0.1/nvidia-dls-user-guide/index.html#migrating-dls-instance-online)
  * [Instructions for upgrading from DLS 2.0.0](https://docs.nvidia.com/license-system/dls/2.0.0/nvidia-dls-user-guide/index.html#migrating-dls-instance-online)
  * [Instructions for upgrading from DLS 1.1](https://docs.nvidia.com/license-system/dls/1.1/nvidia-dls-user-guide/index.html#migrating-dls-instance-online)
  * [Instructions for upgrading from DLS 1.0.1](https://docs.nvidia.com/license-system/dls/1.0.1/nvidia-dls-user-guide/index.html#migrating-dls-instance-offline)
  * [Instructions for upgrading from DLS 1.0](https://docs.nvidia.com/license-system/dls/1.0.1/nvidia-dls-user-guide/index.html#migrating-dls-instance-offline)


###  [6.1.1. Upgrading a DLS Appliance in Place](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-dls-instance-online)
To provide security updates and bug fixes, NVIDIA periodically releases updates to the DLS appliance software. To apply a software update to a DLS appliance, upgrade the DLS appliance in place. Upgrading a DLS appliance in place overinstalls the software updates on the existing appliance without any disruption in service. All data on the appliance is preserved during the upgrade.
To upgrade a DLS appliance in place, follow this sequence of instructions: 
  1. [Initiating an In-Place Upgrade to a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#initiating-in-place-upgrade-to-dls-appliance "Initiating an in-place upgrade to a DLS appliance puts the instance that is hosted on the appliance into maintenance mode. If the instance is a node in an HA cluster of DLS instances, all nodes in the cluster are put into maintenance mode. In maintenance mode, a DLS instance can perform only licensing operations. All other operations are disabled.")
  2. The instructions for the type of platform on which the DLS appliance is hosted: 
     * [Upgrading a Container-Based DLS Appliance in Place](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#upgrading-container-based-dls-appliance-in-place)
     * [Upgrading a VM-Based DLS Appliance in Place](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#upgrading-vm-based-dls-appliance-in-place)


###  [6.1.1.1. Initiating an In-Place Upgrade to a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#initiating-in-place-upgrade-to-dls-appliance)
Initiating an in-place upgrade to a DLS appliance puts the instance that is hosted on the appliance into maintenance mode. If the instance is a node in an HA cluster of DLS instances, **all** nodes in the cluster are put into maintenance mode. In maintenance mode, a DLS instance can perform **only** licensing operations. All other operations are disabled.
**For an HA cluster:** Perform this task **only** from the DLS virtual appliance that hosts the current **primary** instance. 
  1. Log in to the DLS virtual appliance for which you want to initiate an in-place upgrade.
  2. On the source appliance, open Operations > Upgrade.
  3. In Cluster nodes, click Sync changes for the node that requires synchronization.
  4. Click **START IN-PLACE UPGRADE**.


Follow the instructions for the type of platform on which the DLS appliance is hosted: 
  * [Upgrading a Container-Based DLS Appliance in Place](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#upgrading-container-based-dls-appliance-in-place)
  * [Upgrading a VM-Based DLS Appliance in Place](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#upgrading-vm-based-dls-appliance-in-place)


###  [6.1.1.2. Upgrading a Container-Based DLS Appliance in Place](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#upgrading-container-based-dls-appliance-in-place)
Ensure that the following prerequisites are met:
  * The ZIP archive that contains the update to NVIDIA License System has been downloaded from the NVIDIA Licensing Portal.
  * The upgrade has been initiated as explained in [Initiating an In-Place Upgrade to a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#initiating-in-place-upgrade-to-dls-appliance "Initiating an in-place upgrade to a DLS appliance puts the instance that is hosted on the appliance into maintenance mode. If the instance is a node in an HA cluster of DLS instances, all nodes in the cluster are put into maintenance mode. In maintenance mode, a DLS instance can perform only licensing operations. All other operations are disabled.").


**For an HA cluster:** Perform this task on each DLS appliance that hosts an instance in the cluster. Upgrade the DLS appliance for every other instance in the cluster **before** upgrading the DLS appliance that hosts the current **primary** instance. 
  1. Stop the DLS appliance container.
  2. As the postgres user, run the /etc/dls/take-snapshot.sh script to create a snapshot of the PostgreSQL database container. The command for running the script depends on the container orchestration platform that you are using.   
| Container Orchestration Platform  | Command  |  
| --- | --- |  
| **Docker**  |  Copy Copied! 
```
            
            $ docker exec -it --user postgres config-postgres-nls-si-0-1 /etc/dls/take-snapshot.sh
        
```
 |  
| **Kubernetes**  |  Copy Copied! 
```
            
            $ kubectl exec -it _db-container-pod_ -- su postgres -c "/etc/dls/take-snapshot.sh"
        
```


_db-container-pod_ 
    The name of the PostgreSQL database container pod.  |  
| **Podman**  |  Copy Copied! 
```
            
            $ podman exec -it --user postgres config-postgres-nls-si-0-1 /etc/dls/take-snapshot.sh
        
```
 |  
| **Red Hat OpenShift Container Platform**  |  Copy Copied! 
```
            
            $ kubectl exec -it _db-container-pod_ -- su postgres -c "/etc/dls/take-snapshot.sh"
        
```


_db-container-pod_ 
    The name of the PostgreSQL database container pod.  |  
| **VMware Tanzu Application Platform**  |  Copy Copied! 
```
            
            $ kubectl exec -it _db-container-pod_ -- su postgres -c "/etc/dls/take-snapshot.sh"
        
```


_db-container-pod_ 
    The name of the PostgreSQL database container pod.  |  
  3. Stop the PostgreSQL database container.
  4. Back up the postgres-data, logs, rabbitmq_data, and configurations volumes.
  5. Clean up the postgres-data and rabbitmq_data volumes. 
Note:
Do **not** clean up the configurations volume. 
  6. Extract the contents of the ZIP archive that contains the update to NVIDIA License System . 
Copy
Copied!

```
            
            
$ sudo unzip nls-_version_-bios.zip

        
```


_version_ 
     The version number of NVIDIA License System that you are upgrading to, for example, 2.2.1. 
  7. Deploy the updated containers on the existing host with the existing volume mappings. Follow the instructions for the container orchestration platform that you are using.   
| Container Orchestration Platform  | Instructions  |  
| --- | --- |  
| **Docker**  | [Deploying the Containerized DLS Software Image on Docker](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-docker)  |  
| **Kubernetes**  | [Deploying the Containerized DLS Software Image on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-kubernetes-platforms "Perform this task if you are using Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. This procedure installs a single-replica DLS deployment \(Active - Passive\) using the legacy YAML files. To provide high availability with this method, deploy separate DLS instances and configure them as an HA cluster.")  |  
| **Podman**  | [Deploying the Containerized DLS Software Image on Podman](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-podman)  |  
| **Red Hat OpenShift Container Platform**  | [Deploying the Containerized DLS Software Image on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-kubernetes-platforms "Perform this task if you are using Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. This procedure installs a single-replica DLS deployment \(Active - Passive\) using the legacy YAML files. To provide high availability with this method, deploy separate DLS instances and configure them as an HA cluster.")  |  
| **VMware Tanzu Application Platform**  | [Deploying the Containerized DLS Software Image on Kubernetes Platforms](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deploying-dls-software-image-on-kubernetes-platforms "Perform this task if you are using Kubernetes, Red Hat OpenShift Container Platform, or VMware Tanzu Application Platform. This procedure installs a single-replica DLS deployment \(Active - Passive\) using the legacy YAML files. To provide high availability with this method, deploy separate DLS instances and configure them as an HA cluster.")  |  
  8. Confirm that the NVIDIA Licensing application and associated software have been upgraded and that the existing data has been preserved on the DLS appliance. 
    1. Log in as the dls_admin user to the DLS appliance that you just upgraded.
    2. In the left navigation pane, click **SUPPORT**.
    3. On the Support page that opens, confirm that the **NVIDIA Delegated License System** field displays the correct software version.
    4. In the left navigation pane, click **SERVICE INSTANCE** and confirm that the details of the DLS instance on the appliance have been preserved.
    5. In the left navigation pane, click **DASHBOARD** and confirm that the details of the installed license allocation have been preserved.


###  [6.1.1.3. Upgrading a VM-Based DLS Appliance in Place](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#upgrading-vm-based-dls-appliance-in-place)
Initiating an in-place upgrade to a DLS appliance puts the instance that is hosted on the appliance into maintenance mode. If the instance is a node in an HA cluster of DLS instances, **all** nodes in the cluster are put into maintenance mode. In maintenance mode, a DLS instance can perform **only** licensing operations. All other operations are disabled.
**For an HA cluster:** Perform this task **only** from the DLS virtual appliance that hosts the current **primary** instance.
  1. Log in to the DLS virtual appliance for which you want to initiate an in-place upgrade
  2. In the left navigation pane of the NVIDIA Licensing dashboard, click **MAINTENANCE**.
  3. On the **Maintenance** page that opens, scroll down to the **Virtual Appliance Upgrade** section of the page.
  4. Ensure that the **File-Based Upgrade Option** is not selected.
  5. Click **START IN-PLACE UPGRADE**.


Ensure that the following prerequisites are met: 
  * The ZIP archive that contains the update to NVIDIA License System has been downloaded from the NVIDIA Licensing Portal.
  * DLS Virtual Machine Appliance source node is on version 3.3.0 with license server installed.
  * DLS Appliance Virtual Machine snapshot is created.
  * In-place upgrade requires port 8443 to be open in the firewall.



In-place upgrade limitations
    The operating system packages are not changed. Upgrade to the latest VM.     The LDAP configuration settings are not changed. Any issues or enhancements related to LDAP require migration to a new VM      The `dls_admin` scripts and all other scripts are not upgraded, updated, or added.      The NTP configuration settings are not changed. Any issues or enhancements that are related to NTP require migration to a new VM.     The NFS configuration settings are not changed. Any issues or enhancements related to NFS require migration to a new VM.     The OS users configuration and settings are not changed.     If you are performing a DLS upgrade to address an issue related to these fields, a File-Based Upgrade is required.
**For an HA cluster:** Perform this task on each DLS appliance that hosts an instance in the cluster. Upgrade the DLS appliance for every other instance in the cluster **before** upgrading the DLS appliance that hosts the current **primary** instance. 
  1. Login to the DLS node.
  2. Navigate to the **MAINTENANCE** tab.
  3. In the **Virtual Appliance Upgrade** section select **In-Place Upgrade** toggle.
  4. Trigger In-place Upgrade. Users can do this by clicking on the user interface button **START IN-PLACE UPGRADE PROCESS**. 
    1. Doing this will put all nodes in maintenance mode for upgrade in addition to enabling the navigation interface for **Upgrade Workflow**.
    2. During this time, the standalone node will continue serving the leases as long as the node is up during the upgrade.
    3. Users won’t be able to perform any admin operations on the license server.
    4. On the Upgrade & Migration page, wait for the Open DLS upgrade workflow to become available. Click it to open the upgrade workflow in a separate browser tab.
  5. On the **Upgrade** workflow screen users should upload the container images zip.
  6. Upgrade will start as soon as the container images zip file upload is completed. This process is composed of 10 steps, the details of which is seen on the right side on the **Upgrade Workflow** screen.
  7. **Upgrade** screen workflow display’s step by step details as it progresses.
  8. In step 10 of the upgrade process, the system will wait for new containers to start up. It takes up to 10 minutes for the appliance services and containers to complete the startup. 
    1. You can opt to **Rollback** the upgrade if the new containers/services don’t startup on time.
    2. You can opt to **Restart** the containers in—order to resolve any transient issues.
  9. Once step 10 completes with a successful container startup, the upgrade is deemed to be completed and users will be able to navigate to the DLS UI portal using the UI button visible on **Upgrade Workflow** screen. 
    1. This step will close the**Upgrade Workflow** screen and it will not be accessible anymore.


###  [6.1.1.4. Troubleshooting the In-Place Upgrade of a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-in-place-upgrade-of-dls-appliance)
If the in-place upgrade of a DLS appliance fails, you can revert the appliance to its previous version. How to revert the appliance to its previous version depends on whether the upgrade failed for an entire HA cluster of DLS instances or only for some instances in the cluster. 
  * **Container-based appliance:**
    1. Stop the upgraded DLS appliance container.
    2. Restore the backed up postgres-data, logs, and rabbitmq_data volumes.
    3. Deploy the previous version of the containers on the existing host with the existing volume mappings.
  * If the upgrade failed only for some instances in the cluster, recreate the cluster from a node that was successfully upgraded. 
    1. Mark the upgraded node as the primary node in the cluster.
    2. Remove from the cluster any node for which the upgrade failed.
    3. Install or deploy a new DLS appliance with the same network configuration as the appliance that failed to be created.
    4. Add the new DLS appliance to the cluster.


###  [6.1.2. Performing a File-Based Migration of a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#performing-file-based-migration-dls-instance)
File based migration of a DLS instance enables you to perform an upgrade based on a binary migration file of a DLS instance when NVIDIA releases a new major version of the DLS virtual appliance.
Note:
For HA based File Based migration, licensing clients could see some transient failures when re-creating the cluster after the migration. 
These transient failures would be resolved once HA cluster is restored after the Upgrade process is complete. If some clients are still failing they would need to be restarted. 
To perform a file-based upgrade of a DLS instance, follow these steps: 
  1. [Initiating a File-Based Upgrade on a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#initiating-an-upgrade-on-dls-instance)
  2. Instructions for preparing a new appliance from which to generate a DLS Migration token: 
     * [Preparing a new DLS Instance for File-Based Migration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#preparing-new-appliance-for-standalone-dls-instance "Migrating a DLS instance requires a DLS Migration token to be generated from an appliance that is running the new version of the DLS software. Preparing a new appliance for a standalone DLS instance involves installing or deploying the new version of the DLS appliance in the same way as for a first-time installation of a DLS appliance.")
  3. [Generating a Migration File for the DLS Instance that you are Migrating](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-migration-file-for-dls-instance)


###  [6.1.2.1. Initiating a File-Based Upgrade on a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#initiating-an-upgrade-on-dls-instance)
  1. Log in to the existing DLS virtual appliance that hosts the DLS instance that you want to migrate. The DLS instance must be either a standalone DLS instance or the primary node in an HA cluster.
  2. In the left navigation pane of the NVIDIA Licensing dashboard, click **MAINTENANCE**.
  3. On the Maintenance page that opens, scroll down to the **Virtual Appliance Upgrade** section of the page. 
  4. Set the **File Based Upgrade** option.
  5. If the DLS instance is the primary node in an HA cluster, a table that lists the nodes in the cluster is added to the Maintenance page. Each node in the table **except the primary node** has an **UPGRADE** button. In the Trigger File Based Upgrade window that opens, click **UPGRADE**.
  6. Modifications to the existing DLS virtual appliance are blocked until the file based migration is complete. The state of the instance during the migration process depends on whether the instance is a standalone instance or a node in an HA cluster. 
    1. If the instance is a standalone DLS instance, all operations by the instance, including licensing operations, are disabled.
    2. If the instance is a node in an HA cluster of DLS instances, all data is removed from the secondary nodes and the secondary nodes are removed from the cluster. The **primary** node in the cluster is put into maintenance mode and continues to operate as a standalone DLS instance. In maintenance mode, a DLS instance can perform **only** licensing operations. All other operations are disabled.


Prepare a new appliance from which to generate a DLS instance token. How to prepare the appliance depends on the type of the appliance and the type of node that the appliance will host. 
###  [6.1.2.2. Preparing a new DLS Instance for File-Based Migration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#preparing-new-appliance-for-standalone-dls-instance)
Migrating a DLS instance requires a DLS Migration token to be generated from an appliance that is running the new version of the DLS software. Preparing a new appliance for a standalone DLS instance involves installing or deploying the new version of the DLS appliance in the same way as for a first-time installation of a DLS appliance.
Note:
For Container-based deployments of DLS, it is recommended to perform an In-Place Upgrade as described here: Upgrading a Container-Based DLS Appliance in Place, however below steps are still possible for container-based deployments. 
Ensure that the ZIP archive that contains the latest DLS virtual appliance image has been downloaded from the NVIDIA Licensing Portal.
  1. Install or deploy the new version of the DLS appliance. For detailed instructions, refer to the following topic: [Installing the DLS Virtual Appliance Image on a Supported Hypervisor](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-dls-virtual-appliance "DLS virtual appliance images are available for several hypervisors. You use standard interfaces of the hypervisor to install the DLS virtual appliance on your chosen hypervisor.")
Note:
If you are migrating an HA Cluster, **SHUTDOWN** the **secondary** DLS Instance. Once shutdown, deploy the new DLS Instance using these same Network Properties as the DLS Instance that was the secondary. 
  2. Start the new DLS appliance and ensure the web interface is accessible.
  3. Once you are able to see the web interface for the node installed above, click Upgrade. From the Upgrade page, select **File Based Upgrade** and download the **DLS Migration Token**.
  4. This **DLS Migration Token** will be used in subsequent steps. Please continue to [**Generating a Migration File for the DLS Instance that You are Migrating**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-migration-file-for-dls-instance)


###  [6.1.2.3. Generating a Migration File for the DLS Instance that you are Migrating](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-migration-file-for-dls-instance)
After generating the migration token for the upgraded DLS instance, you must generate a migration file for the instance. Generating the **Migration file** involves generating and uploading the DLS migration **token** from the **new DLS instance** onto the **existing DLS Instance** you are migrating from. 
Ensure that you have the DLS migration token from **upgraded DLS instance** as explained in the following topics:
  * [Preparing a new DLS Instance for File-Based Migration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#preparing-new-appliance-for-standalone-dls-instance "Migrating a DLS instance requires a DLS Migration token to be generated from an appliance that is running the new version of the DLS software. Preparing a new appliance for a standalone DLS instance involves installing or deploying the new version of the DLS appliance in the same way as for a first-time installation of a DLS appliance.")


  1. Log in to the existing DLS virtual appliance that hosts the DLS instance that you want to migrate.
  2. Open Operations > Upgrade.
  3. In the file-based upgrade controls, use Migration token file to select the .tok file downloaded from the new appliance, then click Upload Token.
  4. Click Generate Migration File.
  5. Wait until the Migration file ready for download is displayed, then click Download Migration File.


Once you have the migration file, follow the instructions in below in **Transferring Migration Data to Upgraded DLS Instance**. . 
###  [Transferring Migration Data to Upgraded DLS Instance ](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#task_o5b_3rr_fgc)
  1. Return to the Upgrade from the previous DLS page on the new appliance.
  2. Select File-based under Upgrade path. 
  3. Under Migration file, click the upload area to browse for the .bin migration file downloaded from the source appliance, or drag the file into the upload area.
  4. Click Upload Migration File and keep the page open while the file is uploaded.
  5. In the file browser that opens, navigate to the folder that contains the migration file that is named on_prem_migration_file__mm_ -_dd_ -_yyyy_ -_hh_ -_mm_ -_ss_.bin that you downloaded and select the file.
  6. Back in the Upload Migration File, click **UPLOAD**.


If you are transferring migration data for a standalone DLS instance, no further action is required. The virtual appliance is ready for use. 
If you are transferring migration data for a node in an HA cluster of DLS instances, synchronize data changes with the primary node in the cluster as explained in [Synchronizing Changes with the Primary Node in an HA Cluster](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#synchronizing-changes-with-primary-node-in-ha-cluster). The data changes to synchronize arise from licensing operations that were performed during the migration. 
###  [6.1.2.4. Synchronizing Changes with the Primary Node in an HA Cluster](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#synchronizing-changes-with-primary-node-in-ha-cluster)
  1. Log in to the **existing** DLS virtual appliance that hosts the DLS instance from which you want to synchronize data. This instance is operating in maintenance mode.
  2. In the left navigation pane of the NVIDIA Licensing dashboard, click **MAINTENANCE**.
  3. On the Maintenance page that opens, scroll down to the **Virtual Appliance Upgrade** section of the page.
  4. In table that lists the nodes in the cluster, click **Synchronize changes**. Any changed data on the existing DLS instance is copied to the new DLS instance. After the changed data has been copied, the old and the new instance are operating in maintenance mode.
  5. Log in to the **new** DLS virtual appliance that hosts the DLS instance to which the data was synchronized. 
If you are already logged in, reload the browser page.
  6. On the upgraded appliance, open Operations > Upgrade. When Acknowledge upgrade is displayed, click it, then click Acknowledge upgrade in the confirmation dialog. All data on the existing DLS instance is removed. The new DLS instance is now the primary node in the cluster and continues to be updated with changes from the existing DLS instance. When the new DLS instance is updated with all changes from the existing instance, the existing DLS instance is deleted from the DLS appliance that hosts it.
  7. Delete the DLS appliance that hosted the existing DLS instance.


For seven days after the new DLS instance has been updated, the Operations > Upgrade page displays information about the cluster nodes before migration. You can use this information to re-create the cluster after the migration. 
  1. Install or deploy a new DLS virtual appliance for the secondary nodes in the cluster as explained in the following topics: [Installing and Configuring the DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-configuring-dls-virtual-appliance "To simplify the installation and administration of the DLS, the DLS is distributed as a software image to be installed or deployed on a supported platform. The DLS is a secure, hardened environment in which access to the application software is strictly controlled.")
To get information such as IP addresses about the nodes in the cluster before the upgrade, refer to the Maintenance page for the upgraded primary node.


###  [6.2. Setting the Validity Period of a Lease Authorization Token for a Service Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-validity-period-lease-authorization-token)
You can set the validity period of a lease authorization token to either enhance performance or increase security. Increasing the validity period enhances performance by decreasing the frequency with which clients are authorized before the service instance grants a licensing request. Decreasing the expiration time increases security by increasing the frequency with which clients are authorized before the service instance grants a licensing request.
The default validity period is one hour. You can set the validity period to any value up to 24 hours. 
How to set the validity period of a lease authorization token for a service instance depends on whether you are setting it for a CLS instance or a DLS instance. For detailed instructions, see: 
  * [Setting the Validity Period of a Lease Authorization Token for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-validity-period-lease-authorization-token-cls-instance)
  * [Setting the Validity Period of a Lease Authorization Token for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-validity-period-lease-authorization-token-dls-instance)


###  [6.2.1. Setting the Validity Period of a Lease Authorization Token for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-validity-period-lease-authorization-token-cls-instance)
Perform this task in the NVIDIA Licensing Portal. 
  1. If you are not already logged in, log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click NVIDIA LICENSING PORTAL.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Find the cloud license allocation that you want to modify.
  4. On the right side of the allocation, open the **More CLS Actions** menu and select **Open Settings**.
  5. Select **Other Settings** , modify **Auth Token Expiry Time** as needed, and click **SAVE SETTINGS**.


###  [6.2.2. Setting the Validity Period of a Lease Authorization Token for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-validity-period-lease-authorization-token-dls-instance)
Perform this task on the NVIDIA Licensing application on the virtual appliance that hosts the DLS instance. 
  1. Open Operations > Settings..
  2. Select General and locate Lease Settings..
  3. Set Auth Token Expiry Time using the Days, Hours, and Minutes fields.
  4. Click Save Settings.


###  [6.3. Specifying the Retention Period for Client Registrations](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#specifying-retention-period-client-registrations)
To prevent excessive consumption of memory in its database, a DLS instance periodically removes details about licensed clients that have been inactive for a specific period of time. An inactive client does not have any licenses checked out from a license server that is bound to the DLS instance. You can control how long a client must be inactive before its details are removed.
  1. Enable the sudo user rsu_admin on the DLS appliance.
  2. Log in to the appliance using the rsu_admin.
  3. You can either list the current configuration values or update these values. 
     * **List:** Get the current values by running this command. 
Copy
Copied!

```
            
            
sudo python /etc/adminscripts/configure_client_registrations_clean_up_settings.py --action list --username dls_user --password dls_password

        
```

     * **Update:** Update the existing values by running this command. 
Copy
Copied!

```
            
            
sudo python /etc/adminscripts/configure_client_registrations_clean_up_settings.py --action update --username dls_user --password dls_password -time-window-for-purging-inactive-clients P1D -time-threshold-for-inactive-clients-registration P5D

        
```

Specify the following configuration values in the format described in [ISO8601 Duration Format](https://en.wikipedia.org/wiki/ISO_8601#Durations). 

_time-window-for-purging-inactive-clients_ 
    Licensed clients who do not have any active lease, and there is no lease activity in this configured window to be purged. 

_time-threshold-for-inactive-clients-registration_ 
    A time window of client registration to consider when purging inactive clients.
Note:
The previous settings from the /etc/dls/network/registered_origin_cleanup_config.properties file in DLS 3.1.0 will not be migrated to DLS versions 3.2.0 and later. The current settings defined through the script, which are part of DLS 3.2.0, will be migrated to newer DLS versions. 


###  [6.4. Gathering License Usage Statistics](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#gathering-license-usage-statistics)
You can use license usage statistics to estimate the number of licenses that you need to purchase.
###  [6.4.1. Feature Metrics](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#feature-metrics)
You can use license usage statistics to estimate the number of licenses that you need to purchase. These statistics are available from DLS and CLS instances.
Please note that if the system’s disk space falls below a certain threshold, this data source may be deleted to ensure DLS continues to operate.To ensure that the statistics gathered are correct, set the retention period for events to 90 as explained in [Setting the Retention Period of Events on a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-retention-period-events-dls-instance "A DLS instance records events related to administration of the instance and the serving of licenses from the instance to licensed clients. The events are captured in real-time and displayed on the Events page of the instance within 5 seconds. You can control the number of events that are displayed on this page by setting the retention period of events on a DLS instance. Any event older than the retention period is deleted from the instance."). 
For a DLS instance 
  1. In the top navigation bar of the NVIDIA Licensing dashboard, click **METRICS**.
  2. Use the time-range controls at the top of the page to select the period to display.
  3. Under **Feature Utilization** , select the required products from **Features**.


For a CLS instance:
  1. If you are not already logged in, log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**..
  3. Find the cloud license allocation for which you want to view feature-utilization metrics.
  4. Open the license allocation, scroll down, and click **Lease History**
  5. Use the time-range controls to select the period to display.
  6. Under **Feature Utilization** , select the required products from **Features**. 
The feature-utilization cards update automatically when you change the time range or selected features. Each card displays its metric for the selected features.


###  [6.4.2. Lease Usage Metrics](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#lease-usage-metrics)
Lease Usage Metrics displays a graph of maximum peak usage, minimum peak usage, and average peak usage for the feature across three granularities: Minutes, Hours, and Days.
Minutes” data is only available for the last 15 days; selecting a date range prior to this will result in no data displayed. 
Retention periods of these metrics::
  * Minute-level data: Default period of 15 days.
  * Hour-level data: Default period of 30 days
  * Day-level data: Default period of 180 days.


###  [6.5. Configuring Email Alerts for Licensing Events](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-email-alerts-for-licensing-events)
To avoid disruption to your services, you can configure email alerts for licensing events that relate to the expiration or exhaustion of NLS resources. These alerts remind you to renew the affected resources before they expire or are exhausted. By default, NLS does not send any alerts. If you want NLS to send alerts, you must create them. After creating an alert, you can edit it and, if you want NLS to stop sending it, disable it or stop the alert.
You can configure email alerts for the NVIDIA Licensing Portal and for a DLS appliance. The types of licensing events for which you can configure email alerts depend on the scope of the alerts.   
| Licensing Event Type  | Scope  |  
| --- | --- |  
| Lease consumption  | 
  * NVIDIA Licensing Portal
  * DLS appliance

 |  
| License expiration  | NVIDIA Licensing Portal  |  
| License acquistion failure  | NVIDIA Licensing Portal  |  
| License renewal failure  | NVIDIA Licensing Portal  |  
| API key expiration  | NVIDIA Licensing Portal  |  
| SSL certificate expiration  | NVIDIA Licensing Portal  |  
| Failover event  | DLS appliance  |  
| Service availability  | DLS appliance  |  
| System resources  | DLS appliance  |  
For each type of event, you can specify who should receive email alerts and other properties specific to each type of event that determine the criteria for sending the alerts. However, the content of the alert for each event type is preset and cannot be changed. 
SSL certificate expiration alerts are supported only on DLS appliances. They are not available in the NVIDIA Licensing Portal. 
If you are configuring email alerts for a DLS appliance, ensure that the DLS appliance can reach a Simple Mail Transfer Protocol (SMTP) server for sending the alerts.
  1. Navigate to the part of the NVIDIA Licensing application for configuring email alerts for the scope that you are interested in.   
| Scope  | Steps  |  
| --- | --- |  
| **NVIDIA Licensing Portal**  | 
    1. If you are not already logged in, log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**. The NVIDIA Licensing Portal dashboard opens.
    2. From the top navigation bar, select **Operations** , and then select **Email Alerts**.
    3. In the left navigation pane of the NVIDIA Licensing Portal dashboard, click **EMAIL ALERTS**.
 |  
| **DLS appliance**  | 
    1. Log in to the DLS appliance.
    2. Open Operations > Settings and select Alerts.
    3. If SMTP is not configured, enter the SMTP server details and click Configure SMTP. To change existing SMTP settings, click Update SMTP configuration.
 |  
  2. If prompted, provide details of the SMTP server for sending alerts from this DLS appliance and click **CONFIGURE SMTP**. 
You can change details that were provided previously by following the **Update SMTP configuration** link.You can also change the from email address used for sending emails from DLS instances with the SMTP Relay. 

Host
     The fully qualified domain name, for example, smtp.example.com, or the IP address of the SMTP server for sending alerts from the appliance.  

Username
    
The name of the user that the DLS appliance will log in as to the SMTP server to send alerts from the appliance. Username must identify a valid user account on the SMTP server.
This field is optional. 

Password
    
The user's password for logging in to the SMTP server.
This field is optional. 

Security
    
     * **Forced SSL:**
A client will try to establish a secure connection without asking a server about its compatibility. If it succeeds, a secure connection will be set up and a handshake will follow. If a server is not compatible or a connection times out, a transmission will be abandoned.
     * **Opportunistic SSL:**
A client will run a STARTTLS command to upgrade a connection to an encrypted one. If a server is compatible and no errors occur, the secured SSL connection will be established. If anything fails in the process, a plain-text transmission will be established.
     * **No SSL:**
No SSL cryptographic protocol is used in email transmission. 

Port Number
    The SMTP port that manages information transfers from one server to another. The available port numbers are 587, 465, 25, and 2525. 

From Email Address
    
Change the from email address used for sending emails from DLS instances if needed. The default email address value is **noreply@nvidia.com**.
  3. Create or edit the alert that you are interested in. To create an alert, click Create. To edit an alert, click Edit. For SSL certificate expiration alerts on a DLS appliance, use the following procedure: 
    1. On the Alerts tab, in SSL Certificate Expiry, click Configure.
    2. In Email recipients, add each address that should receive the alert.
    3. In Notify when SSL certificate expires within days, enter a value from 5 to 90.
    4. Click Create alert. A newly created alert is enabled automatically. When editing an existing alert, ensure that Enabled is selected, then click Edit alert.
DLS checks the certificate daily and sends a notification to the configured recipients when the certificate is within the selected expiry threshold and the alert is enabled. SMTP must be configured for delivery.
Expand  
| Event Type  | Steps  |  
| --- | --- |  
| **NVIDIA Licensing Portal only:**API Key Expiration****  | 
    1. **Editing an alert only:** If you want to enable or disable the alert, select or deselect the **Enabled** option.  Note: When you create an alert, the alert is enabled automatically. 
    2. For each intended recipient of the alert, type the recipient's email address into the text-entry field and click the plus-sign (**+**) next to the field. Ensure that the email address corresponds to a registered contact in your organization. The address is added to the list of addresses in the **E-mail addresses to notify** field.
    3. In the **Number of days prior to expiration to receive notification** text-entry field, type the number of days before the API key expires that NLS should send an alert.
 |  
| **NVIDIA Licensing Portal only:**Feature Expiration****  | 
    1. **Editing an alert only:** If you want to enable or disable the alert, select or deselect the **Enabled** option.  Note: When you create an alert, the alert is enabled automatically. 
    2. For each intended recipient of the alert, type the recipient's email address into the text-entry field and click the plus-sign (**+**) next to the field. Ensure that the email address corresponds to a registered contact in your organization. The address is added to the list of addresses in the **E-mail addresses to notify** field.
    3. In the **Number of days prior to expiration to receive notification** text-entry field, type the number of days before the license expires that NLS should send an alert.  Note: You can use the **Actions** button to acknowledge and stop an email alert. A list of entitlement product key IDs is displayed on the alert page below the Alert Panel with the option to acknowledge and stop receiving the alert. 
 |  
| **Lease Consumption**  | 
    1. **Editing an alert only:** If you want to enable or disable the alert, select or deselect the **Enabled** option.  Note: When you create an alert, the alert is enabled automatically. 
    2. For each intended recipient of the alert, type the recipient's email address into the text-entry field and click the plus-sign (**+**) next to the field. Ensure that the email address corresponds to a registered contact in your organization. The address is added to the list of addresses in the **E-mail addresses to notify** field.
    3. In the **Notify when feature lease consumption exceeds percentage** text-entry field, specify the percentage of available licenses consumed for a specific product that must be exceeded for NLS to send an alert.
    4. In the **Ignore additional alerts occurring within the next hours** text-entry field, specify the number of hours after an alert is sent during which any further consumption of available licenses is ignored.
    5. Select **Enable Lease Failure Alert**
    6. **Optionally for CLS:** Click **Select Scope** to enter a lease scope for each email ID in the **Available scopes** dialog. The email ID will receive alerts for the selected scope only, otherwise it will alerts for all service instances and servers available in the organization for email alerts. Click **Configure Scope**.
 |  
| **Failover Event**  | 
    1. **Editing an alert only:** If you want to enable or disable the alert, select or deselect the **Enabled** option.  Note: When you create an alert, the alert is enabled automatically. 
    2. For each intended recipient of the alert, type the recipient's email address into the text-entry field and click the plus-sign (**+**) next to the field. Ensure that the email address corresponds to a registered contact in your organization. The address is added to the list of addresses in the **E-mail addresses to notify** field.
 |  
| **Service Availability**  | 
    1. **Editing an alert only:** If you want to enable or disable the alert, select or deselect the **Enabled** option.  Note: When you create an alert, the alert is enabled automatically. 
    2. For each intended recipient of the alert, type the recipient's email address into the text-entry field and click the plus-sign (**+**) next to the field. Ensure that the email address corresponds to a registered contact in your organization. The address is added to the list of addresses in the **E-mail addresses to notify** field.
    3. In the **Ignore additional alerts occurring within the next minutes** text-entry field, type the number of minutes after an alert is sent during which any further inactive service is ignored.
 |  
| **System Resources**  | 
    1. **Editing an alert only:** If you want to enable or disable the alert, select or deselect the **Enabled** option.  Note: When you create an alert, the alert is enabled automatically. 
    2. For each intended recipient of the alert, type the recipient's email address into the text-entry field and click the plus-sign (**+**) next to the field. Ensure that the email address corresponds to a registered contact in your organization. The address is added to the list of addresses in the **E-mail addresses to notify** field.
    3. In the **Notify when system resource usage exceeds percentage** text-entry field, specify the percentage of CPU and memory usage that must be exceeded for NLS to send an alert. To specify the disk space threshold, refer to [Disk Space Threshold Configuration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#disk-space-threshold-configuration "Effective with DLS 3.6.0, the logic for DLS force purge and disk space alerting has shifted from percentage-based utilization thresholds to absolute free disk space values. This ensures more consistent operation regardless of total volume size.").
    4. In the **Ignore additional alerts occurring within the next minutes** text-entry field, type the number of minutes after an alert is sent during which any further system resource usage exceeding the monitoring threshold is ignored.
 |  


When you receive an alert, renew the affected resources before they expire or are exhausted. The affected resources and how to renew them depend on the type of the licensing event that caused NLS to send the alert: 
  * **API key expiration:** Renew the API key.
  * **License expiration:** Renew your existing entitlements or purchase new entitlements.
  * **Lease consumption:** If additional licenses are available, add licenses to the license allocation and synchronize its deployment. Otherwise, purchase additional licenses and add them to the allocation.
  * **Service availability:** Check DLS Home > Appliance for appliance/node identity and health, or the selected Cloud/DLS allocation for licensing operations.
  * **SSL certificate expiration:** Configure the SSL certification from the web UI or from the API . Also see [Configuring a DLS Virtual Appliance with a Third-Party Signed SSL Certificate](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-dls-virtual-appliance-with-3rd-party-ssl-certificate "By default, a DLS virtual appliance is configured with a self-signed SSL certificate that is included in the DLS virtual appliance image from which the DLS virtual appliance is created. If necessary, you can replace the self-signed certificate with an SSL certificate that is signed by a third party, such as a certificate authority \(CA\).")
  * **System resources:**
    * To expand CPU and RAM on the DLS appliance, increase the number of vCPUs and the total RAM on your hypervisor.
    * To increase the hard disk storage on the DLS appliance, follow the instructions in [Expanding the Disk Space on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-disk-expansion "You can use the management interface to the NVIDIA Licensing application to expand the disk space of the DLS virtual appliance."). If the disk usage reaches 1 GB free disk space, the audit events will be purged by the system. Use the export event feature to retain the events.
  * **Failover event:** When the primary DLS instance fails, an email alert is sent to the user indicating the cause of failover. When the secondary DLS instance becomes the primary instance and begins to serve licenses, an email alert is sent confirming a successful failover event.


###  [6.5.1. Disk Space Threshold Configuration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#disk-space-threshold-configuration)
Effective with DLS 3.6.0, the logic for DLS force purge and disk space alerting has shifted from percentage-based utilization thresholds to absolute free disk space values. This ensures more consistent operation regardless of total volume size.
****Revised Behavior****
Previously, the system triggered a purge at 80% disk utilization and an alert at 75%. The updated behavior is as follows: 
  * Data Purge Policy: An internal maintenance job is triggered when free disk space falls below 1 GB. This process purges all events and historical lease data to recover space.
  * Alerting Policy: The default free-disk-space alert threshold is 3 GB. When free disk space falls below this threshold, a critical event is recorded on the Events page.
  * Email delivery also requires a configured SMTP server and an enabled System Resources alert with recipients. Configure the alert as follows: 
    * Open Operations > Settings > Alerts.
    * In System Resources, click Configure.
    * Add the required Email recipients and set Ignore additional alerts occurring within the next minutes.
    * Click Create alert to create an enabled rule. When editing an existing rule, select Enabled and click Edit alert.


Disk alerts use the free-space threshold. The usage-percentage field applies to CPU and memory. Email notifications respect the rule's Ignore additional alerts interval, so configuring SMTP alone does not guarantee an immediate email. The critical event recorded on the Events page is separate from email delivery. 
**Configuration Management**
These thresholds utilize default values but can be customized by defining the following environment variables in the deployment YAML file: 
`DISK_FREE_SPACE_THRESHOLD_GB`: Defines the free space limit for triggering data purge (Default: 1) 
`DISK_FREE_SPACE_ALERT_THRESHOLD_GB`: Defines the free space limit for triggering alerts and logging critical events (Default: 3) 
###  [6.6. Setting the Retention Period of Events on a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-retention-period-events-dls-instance)
A DLS instance records events related to administration of the instance and the serving of licenses from the instance to licensed clients. The events are captured in real-time and displayed on the Events page of the instance within 5 seconds. You can control the number of events that are displayed on this page by setting the retention period of events on a DLS instance. Any event older than the retention period is deleted from the instance.
  1. Open a web browser and connect to the URL `https://_dls-vm-ip-address_`. 

_dls-vm-ip-address_ 
     The IP address or, if defined, the fully qualified domain name or the CNAME of the VM on which the DLS virtual appliance is installed. 
You can get the IP address from the management console of your hypervisor.
  2. On the login page that opens, provide the user credentials for the DLS administrator user on the DLS virtual appliance and click **LOGIN**.
  3. Open Operations > Settings
  4. Select General and locate Event Settings.
  5. Set Lease Event Purge Time to the number of days to retain lease events and Admin Event Purge Time to the number of days to retain administration events.
  6. Click Save Settings. 
Lease History Purge Time controls historical lease data separately from the lease-event records controlled by Lease Event Purge Time.


###  [6.7. Configuring a DLS Virtual Appliance with a Third-Party Signed SSL Certificate](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-dls-virtual-appliance-with-3rd-party-ssl-certificate)
By default, a DLS virtual appliance is configured with a self-signed SSL certificate that is included in the DLS virtual appliance image from which the DLS virtual appliance is created. If necessary, you can replace the self-signed certificate with an SSL certificate that is signed by a third party, such as a certificate authority (CA).
To configure a DLS virtual appliance with a third-party signed SSL certificate, follow this sequence of instructions: 
  1. [Obtaining a Third-Party Signed SSL Certificate for a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#obtaining-3rd-party-ssl-certificate-for-dls-virtual-appliance "Obtain a third-party signed SSL certificate by submitting a certificate signing request \(CSR\) to a suitable third party, such as a certificate authority \(CA\).")
  2. [Installing a Third-Party Signed SSL Certificate on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-3rd-party-ssl-certificate-on-dls-virtual-appliance)


###  [6.7.1. Obtaining a Third-Party Signed SSL Certificate for a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#obtaining-3rd-party-ssl-certificate-for-dls-virtual-appliance)
Obtain a third-party signed SSL certificate by submitting a certificate signing request (CSR) to a suitable third party, such as a certificate authority (CA).
Ensure that the IP address of any DLS virtual appliance that will be configured with the certificate is mapped to the domain name that you will specify in the certificate.
For an HA cluster of DLS instances, you can choose to obtain a single wildcard domain certificate for all nodes in the cluster or one fully qualified domain name certificate for each node in the cluster. 
Note:
If intermediate certificates are specified in addition to a primary certificate, they should be specified in the same file in the following order: 
  1. The primary certificate for your domain name
  2. Intermediate certificates
  3. Root certificate


For each certificate that you require, submit a certificate signing request (CSR) to a CA. 
Ensure that each certificate that you request meets these requirements: 
  * The certificate must be a PEM text file (**not** in Java keystore format) and secured with a private key.
  * The certificate and the private key must be in separate files.
  * To ensure that web browsers trust the domain, the domain name must be part of the Subject Alternate Name (SAN) attribute, **not** the Common Name (CN) attribute of the CSR.
  * The SAN attribute of the CSR must specify the fully qualified domain name of any DLS virtual appliance that will be configured with the certificate. 
Do **not** specify an IP address in the SAN attribute of the CSR. A DLS virtual appliance cannot be configured with an SSL certificate that specifies an IP address.
  * The certificate must use RSA, DSA, and DH keys that are at least 2048 bits long.
  * The certificate must use ECC keys greater that are longer than 224 bits long.
  * The certificate must not be passphrase protetcted.


If necessary, contact the CA that will provide your certificate for information about how to request a certificate that meets these requirements or convert an existing certificate to meet these requirements. 
###  [6.7.2. Installing a Third-Party Signed SSL Certificate on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-3rd-party-ssl-certificate-on-dls-virtual-appliance)
Ensure that the following prerequisites are met: 
  * You have obtained the SSL certificate that you are installing and its private key file.
  * Ensure that a license allocation is installed on the service instance as explained in [Creating and Installing a License Allocation on an Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-license-server-on-service-instance "To make licenses available for use, you must first create a license allocation on the NVIDIA Licensing Portal and then install it on your target appliance.").


If you are installing a wildcard domain certificate for all nodes in an HA cluster, perform this task from the primary node in the cluster **only**. The certificate is propagated automatically to the secondary node in the cluster. If you are installing one fully qualified domain name certificate for each node in the cluster, perform this task separately from each node. 
Note:
If intermediate certificates are specified in addition to a primary certificate, they should be specified in the same file in the following order: 
  1. The primary certificate for your domain name
  2. Intermediate certificates
  3. Root certificate


  1. Log in to the DLS virtual appliance on which you are installing the SSL certificate.
  2. Open Operations > Settings > Security.
  3. Locate SSL Configuration. If the installed certificate summary is displayed, click Upload New Certificates
  4. Ensure that Domain Name matches the name in the certificate, including its letter case. If you are installing a wildcard certificate for the configured domain, select Apply Wildcard.
  5. Under Certificate File (.pem), select the certificate file.
  6. Click Upload Certificates.


###  [6.8. Hiding Organization and Virtual Group Information on a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#hiding-organization-virtual-group-information-on-dls-instance)
You can use REST APIs to hide the organization and virtual group to which the licenses served by a DLS instance belong.
  1. Submit an HTTP POST request with basic authentication to log in to the DLS appliance that hosts the DLS instance as the DLS administrator user. 
Copy
Copied!

```
            
            
POST https://_host_:_port_/auth/v1/login

        
```


_host_ 
    The IP address or fully qualified domain name of the VM or container that is hosting the DLS appliance. 

_port_ 
    The port on which the VM or container that is hosting the DLS appliance listens for HTTPS requests.
In the request, pass the user name and password of the DLS administrator user. The default user name is `dls_admin`.
The following example uses the curl command to log in to the DLS appliance `dls.example.com` on port 8081 as the user `dls_admin` with the password `changeit`. 
Copy
Copied!

```
            
            
curl -k --location --request POST 'https://dls.example.com:8081/auth/v1/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "dls_admin"
    "password": "changeit"}'

        
```

The response sets the authorization token in the LIC-TOKEN cookie and includes the token in the response body.
  2. Submit an HTTP PUT request to hide the organization and virtual group on the DLS instance. 
Copy
Copied!

```
            
            
PUT https://_host_/service_instance_manager/v1/config/hide-name-on-ui

        
```


_host_ 
    The IP address or fully qualified domain name of the VM or container that is hosting the DLS appliance.
If the tool that you use to submit the request supports cookies, you can submit the request without the authorization token. Otherwise, you must pass the authorization token that was returned in the previous step in the header as a bearer token.
The following example uses the curl command to hide the organization and virtual group on the DLS instance on the appliance `dls.example.com`. For clarity, the example shows only the placeholder _token_. 
Copy
Copied!

```
            
            
curl -k --location --request PUT \
' https://dls.example.com/service_instance_manager/v1/config/hide-name-on-ui' \
--header 'Authorization: Bearer _token_'

        
```



If the organization and virtual group are hidden after the DLS administrator user has logged in to the DLS appliance, the DLS administrator user must reload the web-based management interface to see the changes. 
To show the organization and virtual group again, submit a PUT request on the REST endpoint for hiding the organization and virtual group with the query parameter hide=false. 
Copy
Copied!

```
            
            
PUT https://_host_/service_instance_manager/v1/config/hide-name-on-ui?hide=false

        
```

###  [6.9. Reconfiguring the Rsyslog Tool in a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#reconfiguring-rsyslog-tool-in-dls-virtual-appliance)
A VM-based DLS virtual appliance uses the Rsyslog tool for forwarding log messages in an IP network. If you want to export events and alerts for the DLS instance on the appliance to an external syslog or security information and event management (SIEM) server, you can reconfigure the Rsyslog tool in the appliance.
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
The format of each type of log message from a DLS virtual appliance is as follows: 
  * syslog messages are in the standard syslog format, with the fields in each message separated by a space: 
Copy
Copied!

```
            
            
_timestamp_ _hostname_ _program-name_ _message-text_

        
```

  * NLS service messages are formatted as JSON strings: 
Copy
Copied!

```
            
            
SEMANTIC:_service-name_: _semantic-json-string_

        
```



Note:
Common Event Format (CEF) is **not** supported. However, you can configure the format or transfer the data to the SIEM server after the logs have been forwarded to the syslog server. 
  1. Use the hypervisor management console of the appliance to log in as the user dls_admin to the VM that hosts the DLS virtual appliance.
  2. Edit the file /etc/rsyslog.conf to update the address and port of the external syslog server. Add _$LocalHostName_ entry in the very first line of /etc/rsyslog.conf so that rsyslog server shows the hostname of the appliance.
  3. Restart the Rsyslog tool. 
Copy
Copied!

```
            
            
# sudo -u root /etc/adminscripts/restart_rsyslog.sh

        
```



###  [6.10. Configuring NTP on a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-ntp-configuration)
Network Time Protocol (NTP) Configuration is when the customer needs to sync the clock of their License Server to a desired time-keeping server. 
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
Ensure that a license allocation is installed on the service instance as explained in [Creating and Installing a License Allocation on an Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#installing-license-server-on-service-instance "To make licenses available for use, you must first create a license allocation on the NVIDIA Licensing Portal and then install it on your target appliance.").
  1. Open a web browser and connect to the URL `https://_dls-vm-ip-address_`. 

_dls-vm-ip-address_ 
     The IP address or, if defined, the fully qualified domain name or the CNAME of the VM on which the DLS virtual appliance is installed. 
You can get the IP address from the management console of your hypervisor.
  2. On the login page that opens, provide the user credentials for the DLS administrator user on the DLS virtual appliance and click **LOGIN**.
  3. Open Operations > Settings > Infrastructure.
  4. In NTP Server Configuration, enter the hostnames or IP addresses in NTP Server(s). Separate multiple servers with commas.
  5. Click Ping Server. The page displays Reachable or Unreachable for each server.
  6. When every server is reachable, click Configure. 
The Synchronized badge in NTP Server Configuration indicates that the appliance reports synchronized time.


###  [6.11. Setting the Maximum Size of the PostgreSQL Database Volume](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#setting-maximum-size-of-postgresql-database-volume)
For a container-based DLS appliance, configure the PostgreSQL volume size used for storage monitoring. The default value is 10 GB. This setting supplies the volume size used in storage-usage calculations; changing it does not resize the database volume or its persistent volume claim (PVC), and does not enforce a filesystem quota. For VM-based appliances, the system computes the volume size.
Note:
You can perform this task **only** on a containerized DLS software appliance. You **cannot** perform this task on a VM-based DLS virtual appliance. 
  1. Log in to the DLS appliance for which you want to set the maximum size of the PostgreSQL database volume.
  2. Open Operations > Settings
  3. Select General and locate Event Settings.
  4. In Volume Disk Allocation In Gb, enter the size in GB of the PostgreSQL volume allocated to the container.
  5. Click Save Settings.


###  [6.12. Supporting TLS 1.2 Cipher Configuration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#supporting-tls-cipher-configuration)
This new feature enables administrators to configure TLS 1.2 cipher on the virtual appliance to meet specific security requirements. This allows for fine-tuning the cryptographic strength of TLS 1.2 communication between the appliance and other systems, providing flexibility to comply with corporate security policies or regulatory standards. However, TLS 1.3 cipher suites are not configurable, as they are governed by their own default settings designed to ensure optimal performance and security. 
  1. Log in to the DLS virtual appliance on which you want to configure TLS Ciphers.
  2. Open Operations > Settings > Security..
  3. In TLS 1.2 Cipher Configuration, enter the required cipher list in TLS Ciphers (colon-separated). Use the NGINX ssl_ciphers directive format.
  4. Leave Skip cipher validation turned off and click Save.


###  [6.12.1. CLI Custom Script for Cipher Reset](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#cli-custom-script-for-cipher-reset)
In the event of a misconfiguration or if there's a need to revert changes, administrators can utilize a custom script via the command-line interface (CLI) to reset the configured TLS 1.2 ciphers. 
  1. Enable the sudo user rsu_admin on the DLS appliance, if not already configured.
  2. Log in to the appliance using the rsu_admin.
  3. Run the `/etc/adminscripts/reset_tls_ciphers.sh` script. It will be reset within 5 minutes. 
This script provides an efficient way to restore the appliance to its default cipher settings, reducing downtime and ensuring a quick recovery from potential configuration errors, without requiring a full system reboot.
If the ciphers are reset, the system will fall back to the default NGINX and OpenSSL cipher suites for TLS 1.2 communications. For HA setups, the script should be manually executed on each node where the misconfiguration needs to be reverted, while for standalone setups, it only needs to be run on the standalone node.


###  [6.13. Troubleshooting a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-dls-instance)
To facilitate troubleshooting, the DLS provides access to log files, event records, and the status of a DLS instance's internal services. If the DLS instance's internal services have failed, you can restart them from the NVIDIA Licensing application on the DLS virtaual appliance.
###  [6.13.1. Log File Locations and Types for a DLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#log-file-locations-and-types-dls-virtual-appliance)
The log files for a DLS virtual appliance contain diagnostic information to help with troubleshooting. The DLS administrator user can access these log files through the hypervisor console or secure shell (SSH).
The log files for a DLS virtual appliance are in the locations in the following table.   
| Log Message Type  | Log File Location  |  
| --- | --- |  
| NVIDIA License System licensing messages  | /var/lib/docker/volumes/logs/_data/licensing  |  
| NVIDIA License System service messages  | /var/lib/docker/volumes/logs/_data/op_log_archive  |  
| /var/lib/docker/volumes/logs/_data/op_log_capture |  
| /var/lib/docker/volumes/logs/_data/op_log_ingest |  
| DLS web server messages  | /var/lib/docker/volumes/logs/_data/nginx  |  
| DLS virtual appliance internal messages related to: 
  * HA configuration
  * Inconsistent data between the primary and secondary nodes in an HA cluster
  * Online migration of a DLS instance

 | /var/lib/docker/volumes/logs/_data/rabbitmq_stdout.log  |  
| /var/lib/docker/volumes/logs/_data/rabbitmq_stderr.log |  
Files in the following standard Linux directories contain log messages from the operating system: 
  * /tmp
  * /var/log
  * /var/tmp


###  [6.13.2. Storing Log Files for a DLS Virtual Appliance on a Network File Share](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#storing-dls-virtual-appliance-log-files-on-network-share)
Because the amount of disk space available in a DLS virtual appliance might be limited, the DLS virtual appliance does not retain all log messages generated during the lifetime of a DLS virtual appliance. If you want to retain all log messages, you can configure the virtual appliance to store log files for a DLS virtual appliance on a network file share. 
If a virtual appliance is configured to store log files for a DLS virtual appliance on a network file share, it periodically aggregates the log files and moves them from the local disk of the DLS virtual appliance to the share. 
Ensure that the following prerequisites are met: 
  * The network file share has been created on a network attached storage server that is accessible from the DLS virtual appliance.
  * The DLS administrator user on the DLS virtual appliance is granted full access to the network file share.


  1. Log in to the DLS virtual appliance.
  2. Open Operations > Settings > Infrastructure.
  3. In Log Archival, select the share type from Platform.
  4. For an NFS share, select Unix / Linux. In Network Share Path, enter nas-server:/export/path, replacing nas-server with the storage server's IP address or fully qualified domain name and /export/path with the exported path..
  5. For a CIFS or SMB share, select Windows. In Network Share Path, enter //nas-server/share/path, replacing nas-server with the storage server's IP address or fully qualified domain name and share/path with the share name and path. Enter Username and Password for an account with access to the share.
  6. Click Mount.


The network file share is mounted at /var/log/licensing on the DLS virtual appliance. This mount point is preset in the DLS virtual appliance and cannot be changed. 
###  [6.13.3. Exporting and Importing Event Records from a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#exporting-importing-events-from-dls-instance)
To help NVIDIA Enterprise Support personnel troubleshoot issues with a DLS instance, you can export event records from the DLS instance and import them into the NVIDIA Licensing Portal.
Note:
The maximum size of a file that can be exported from a DLS instance or imported into the NVIDIA Licensing Portal is 250 MBytes. 
Note:
If no filters are selected, records will be exported for the last 7 days. 
  1. Log in to the DLS virtual appliance that hosts the DLS instance.
  2. Open Events and click Export.
  3. In Export format, select Encrypted (portal) for a file that will be imported into the NVIDIA Licensing Portal. Use Unencrypted (.log) only when you need readable event records for local analysis.
  4. Select the required Categories and Date range.
  5. Click Generate Export and wait until the export is ready.
  6. Click Download export and save the generated file. 
Use the encrypted file downloaded in this step in the following NVIDIA Licensing Portal import procedure.
  7. In the file browser, locate and select the encrypted event export downloaded from the DLS appliance.


###  [6.13.4. Restarting a DLS Instance's Internal Services](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#restarting-dls-instance-internal-services)
If a DLS instance has failed because its internal services are no longer active, you can restart the inactive services to recover from the failure.
  1. Log in to the DLS appliance that hosts the node whose services you want to restart.
  2. Open Home. In Appliance, locate the node and check its critical-services and other-services status.
  3. Open that node's Node actions menu. Select Restart critical services or Restart other services for the affected service group.
  4. Review the restart dialog and click Confirm. 
Restart actions are available only for the node to which you are signed in. For a remote node, use Open node UI in new tab, sign in to that node, and then perform the restart from its Node actions menu.


###  [6.13.5. Recovering from an Incorrect DLS Name or Address in a Client Configuration Token](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#recovering-from-incorrect-dls-id-in-client-configuration-token)
A client configuration token uses an IP address or fully qualified domain name to identify the DLS instance from which to serve a license to a client. If the IP address or fully qualified domain name in the client configuration token is incorrect, licensing requests from all clients that are configured with the token fail to be fulfilled.
When this issue occurs, an error message similar to the following example is written to the licensing event log on the client. 
Copy
Copied!

```
            
            
Failed to acquire license from 192.0.2.206
(Info: NVIDIA RTX Virtual Workstation - 
Error: Could not communicate to server, timeout has been reached)

        
```

Compare the address in the token/error with the DLS Home > Appliance / Nodes address information and the allocation token connection settings. 
Recover from this issue in one of the following ways: 
  * Replace the client configuration token with a new token in which the IP address or fully qualified domain name is correct. 
    1. Generate a new client configuration token as explained in [Generating a Client Configuration Token for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#generating-client-configuration-token-for-dls-instance) to ensure that the IP address or fully qualified domain name in the token is correct.
    2. Configure each licensed client with the new token as explained in [Configuring a Licensed Client with a Networked License](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-nls-licensed-client "A client with a network connection obtains a license by leasing it from a NVIDIA License System service instance. The service instance serves the license to the client over the network from a pool of floating licenses obtained from the NVIDIA Licensing Portal. The license is returned to the service instance when the licensed client no longer requires the license.").
  * Change the IP address or fully qualified domain name of the VM on which the DLS instance is hosted to match the IP address or fully qualified domain name in the client configuration token.


###  [6.13.6. Recovering from an HA Configuration Failure in an IPv6 Network](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#recovering-from-ha-configuration-failure-in-ipv6-network)
An attempt to create an HA cluster of VM-based DLS instances in which the VMs are communicating over IPv6 might fail. The failure occurs because the DLS virtual appliance that is hosting one of the instances cannot ping the DLS virtual appliance that is hosting the other instance. To recover from this failure, use the shell script that each DLS virtual appliance provides specifically for this purpose.
Note:
You can perform this task **only** on a VM-based DLS virtual appliance. You **cannot** perform this task on a containerized DLS software image. Instead, you can use standard interfaces of the OS on which the container orchestration platform is running to make this change. 
Perform this task for each VM on which the DLS virtual appliance for each DLS instance in the cluster is installed. 
  1. Use the hypervisor management console of the appliance to log in as the user dls_admin to the VM that hosts the DLS virtual appliance. If the dls_admin user has not been registered, you can still log in to the VM as the dls_admin user with the default password welcome.
  2. As root, run the /etc/dls/scripts/support_only_ipv6.sh script. 
Copy
Copied!

```
            
            
$ sudo -u root /etc/dls/scripts/support_only_ipv6.sh

        
```

  3. Repeat the attempt to create the cluster by following the instructions in [Creating or Expanding an HA Cluster of DLS Instances](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-ha-cluster-dls-instances "Any time after creating or configuring a standalone DLS instance, you can create an HA Cluster of DLS instances by converting the instance into a node in a cluster and adding a second instance to the cluster. You can also expand an existing cluster by adding a DLS instance to it.").**Note:** If using Virtual IP Management, see Section 6.13.8 Troubleshooting Virtual IP Management for VIP-specific troubleshooting procedures.


###  [6.13.7. IP Conflict Detected in an HA Cluster](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#ip-conflict-detected-in-an-ha-cluster)
NVIDIA DLS deployed as a high availability (HA) cluster on multiple appliances.
This applies to NVIDIA DLS deployed as a high availability (HA) cluster on multiple appliances. 
  * **Condition**
Two or more DLS appliances are using the same IP address, or an address that the cluster already associates with another peer. This often appears when a replacement or duplicate appliance is brought online at an IP that belongs to a cluster node that is offline or unreachable. If a node reboots and, during startup, observes this conflict (including connecting in a way that matches the stored peer address), the system treats it as an IP conflict.
  * **System Behavior**
To avoid split-brain or data corruption, DLS disables the license server on the node that detected the conflict and shows a warning banner in the web UI.
  * **Summary**
If an HA node detects that another DLS is using the same IP as a cluster peer, it disables the local license server and displays an IP conflict banner; resolve the duplicate IP, then use Acknowledge & Enable on the affected node.


###  [6.13.8. DLS has overlapping disk partitions](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-has-overlapping-disk-partitions)
During the backup process you may get a partition overlaps the last partition warning message. Use these steps to resolve the warning.
  1. Log in as the rsu_admin user and execute this command: 
`**sudo sed -i 's/100/99/g' /etc/dls/scripts/expand_disk.sh**`
  2. Increase the VM disk space by 1 GB or more using the steps in [Expanding the Disk Space on a DLS Virtual Applicance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-disk-expansion)
  3. **Optional:** The warning should now be resolved. Execute this command: 
`**`sudo gdisk /dev/sda`**` as the rsu_admin user to confirm.


###  [6.13.9. Troubleshooting Virtual IP Management](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-virtual-ip-management)
**Common Issues and Resolutions**
###  [6.13.9.1. Virtual IP Not Assigned After Configuration](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-ip-not-assigned-after-configuration)
During the backup process you may get a partition overlaps the last partition warning message. Use these steps to resolve the warning.
  1. **VIP Management Services Not Running** : 
_Check service status_
     * systemctl status vipReassignment.path
     * systemctl status vipReassignment.service
_Restart services if needed_
     * systemctl restart vipReassignment.path
     * systemctl start vipReassignment.service
  2. **Trigger File Not Created** : 
_Verify VIP directory exists_
Copy
Copied!

```
            
            
ls -l /var/lib/docker/volumes/configurations/_data/vip/

        
```

     * _# Should contain: ip.txt and VIP_REASSIGNMENT files_
     * _# Manually trigger VIP assignment (for testing)_ echo "add" >
Copy
Copied!

```
            
            
/var/lib/docker/volumes/configurations/_data/vip/VIP_REASSIGNMENT

        
```

  3. **Network Interface Not Detected** : 
_Check network interface detection_
Copy
Copied!

```
            
            
/etc/dls/scripts/network_info.sh

        
```

_Verify output shows correct network device_


###  [6.13.9.2. Virtual IP Already in Use Error](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-ip-already-in-use-error)
During the backup process you may get a partition overlaps the last partition warning message. Use these steps to resolve the warning.
Symptoms . 
  * VIP assignment fails with “duplicate IP address detected”
  * Logs show arping detected existing MAC address


  1. ****Identify Conflicting System**** : 
_Check service status_
     * systemctl status vipReassignment.path
     * systemctl status vipReassignment.service
  2. ****Locate Device with Conflicting MAC**** : 
_Verify VIP directory exists_
Copy
Copied!

```
            
            
ls -l /var/lib/docker/volumes/configurations/_data/vip/

        
```

     * _# Should contain: ip.txt and VIP_REASSIGNMENT files_
     * _# Manually trigger VIP assignment (for testing)_ echo "add" >
Copy
Copied!

```
            
            
/var/lib/docker/volumes/configurations/_data/vip/VIP_REASSIGNMENT

        
```

  3. ****Resolve Conflict**** : 
     * If another system is using the IP: Choose a different IP address
     * If old primary is holding the IP: Reboot the old primary node to trigger cleanup
     * If stale ARP entry: Clear ARP caches on network devices
  4. **Retry VIP Assignment from DLS Web UI on the Primary node** : 
     * Navigate to the Settings page on the Primary node’s DLS Web UI
     * Delete the existing virtual IP configuration
     * Reconfigure with the corrected IP address


###  [6.13.9.3. VIP Not Migrating During Failover](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#vip-not-migrating-during-failover)
During the backup process you may get a partition overlaps the last partition warning message. Use these steps to resolve the warning.
Symptoms 
  * Secondary promotes to primary but VIP remains on failed primary
  * Clients cannot connect after failover


**Possible Causes and Solutions** : 
  1. **Heartbeat Service Not Running** :: 
     * Restart the DLS Appliance to restore heartbeat service
  2. **Network Connectivity Issues** :: 
_From secondary node, verify network connectivity_
Copy
Copied!

```
            
            
ping -c 3 <primary-node-ip>

        
```

_Check if secondary can reach primary's network segment_
  3. **Split-Brain Detected** :: 
_Check for duplicate IP detection logs_
Copy
Copied!

```
            
            
grep "duplicate" /var/log/vip_reassignment.log

        
```

**If both nodes think they’re primary** : Restart one of the DLS Appliance nodes to resolve the split-brain condition
  4. **Retry VIP Assignment from DLS Web UI on the Primary node** : 
     * Navigate to the Settings page on the Primary node’s DLS Web UI
     * Delete the existing virtual IP configuration
     * Reconfigure with the corrected IP address


###  [6.13.9.4. Clients Cannot Connect to Virtual IP](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#clients-cannot-connect-to-virtual-ip)
Symptoms 
  * VIP is assigned and visible on primary node
  * Clients timeout when connecting to VIP
  * Physical IP addresses work correctly


Ensure sudo user _rsu_admin_ is enabled on DLS Virtual Appliance for executing administrative commands 
**Possible Causes and Solutions** : 
  1. **Firewall Blocking VIP Traffic** :: 
_Check iptables rules_
Copy
Copied!

```
            
            
sudo iptables -L -n | grep <virtual-ip>

        
```

Ensure no DROP rules for VIP (should only appear during removal). If DROP rules exist, remove them:
Copy
Copied!

```
            
            
sudo iptables -D INPUT -d <virtual-ip> -j DROP
sudo iptables -D OUTPUT -s <virtual-ip> -j DROP

        
```

  2. **Network Switch Not Learning New MAC** ::: 
_Re-send gratuitous ARP_
Copy
Copied!

```
            
            
sudo arping -U -I eth0 -c 5 <virtual-ip>

        
```

_Check switch ARP table or clear MAC address table_
  3. **Client Using Stale ARP Entry** : 
_On client system (Windows):_
Copy
Copied!

```
            
            
sudo arp -d <virtual-ip>
sudo ping <virtual-ip>

        
```

_On client system (Linux):_
Copy
Copied!

```
            
            
sudo ip neigh flush <virtual-ip>
ping <virtual-ip>

        
```

  4. **VIP on Different Subnet** : 
_Verify VIP is on same subnet as DLS nodes_
Copy
Copied!

```
            
            
sudo ip addr show

        
```

_VIP must be on same Layer 2 network segment_ _If not, choose IP from correct subnet_
     * Navigate to the Settings page on the Primary node’s DLS Web UI
     * Delete the existing virtual IP configuration
     * Reconfigure with the corrected IP address


###  [6.13.9.5. Old Primary Reclaiming VIP After Reboot](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#old-primary-reclaiming-vip-after-reboot)
Symptoms 
  * Old primary node returns online and reclaims VIP
  * Both nodes have VIP assigned (split-brain)


**Resolutions** : 
  1. **Verify Startup Safety Check** : 
_On old primary, check for safety check execution_
Copy
Copied!

```
            
            
grep "secondary on startup" /var/log/applicationStartup.log

        
```

You should see: _"Node is secondary on startup, removing any stale VIP assignment"_
  2. **Force VIP Removal on Old Primary** : 
_Manually trigger VIP removal_
Copy
Copied!

```
            
            
echo "remove" > /var/lib/docker/volumes/configurations/_data/vip/VIP_REASSIGNMENT

        
```

_Wait 5 seconds, verify VIP removed_
Copy
Copied!

```
            
            
ip addr show | grep <virtual-ip>

        
```

_This should return no results_.
  3. **Update applicationStartup.sh if Needed** : 
_Verify startup script includes safety check_
Copy
Copied!

```
            
            
grep -A 10 "Safety check" /etc/dls/scripts/applicationStartup.sh

        
```

_If missing, deploy updated script version_
  4. **VIP on Different Subnet** : 
_Verify VIP is on same subnet as DLS nodes_
Copy
Copied!

```
            
            
sudo ip addr show

        
```

_VIP must be on same Layer 2 network segment_ _If not, choose IP from correct subnet_
     * Navigate to the Settings page on the Primary node’s DLS Web UI
     * Delete the existing virtual IP configuration
     * Reconfigure with the corrected IP address


###  [6.13.9.6. Log File Locations](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#log-file-locations)  
| Log File  | Location  | Contents  |  
| --- | --- | --- |  
| VIP Reassignment Log  | _/var/log/vip_reassignment.log_  | VIP add/remove operations, GARP status, duplicate detection  |  
| Heartbeat Log  |  _/var/log/dls/heartbeat.log_ (in container)  | Failover detection, role changes, VIP trigger events  |  
| HA Configuration Log  |  _/var/log/dls/ha_config.log_ (in container)  | VIP API operations, replication status, configuration changes  |  
| Systemd Path Log  | _journalctl -u vipReassignment.path_  | File monitoring events, service trigger history  |  
| Systemd Service Log  | _journalctl -u vipReassignment.service_  | Service execution history, error messages  |  
| Application Startup Log  | _/var/log/applicationStartup.log_  | Startup safety checks, VIP removal on boot  |  
###  [6.13.10. Troubleshooting Authentication and Authorization Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-authentication-and-authorization-issues)
**Common Issues and Resolutions**
Also see: [Troubleshooting Single Sign-On](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-single-sign-on), [Troubleshooting Multi-Domain LDAP](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-multi-domain-ldap)
###  [6.13.10.1. User Cannot Log In](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#user-cannot-log-in)
During the backup process you may get a partition overlaps the last partition warning message. Use these steps to resolve the warning.
**Symptoms** : 
  * Login fails with “Invalid username or password”
  * 401 Unauthorized error
  * Possible Causes and Solutions:


  * **Incorrect Credentials** :
    * **Verify** : Username and password are correct
    * **Solution** : Use password reset function if forgotten
  * **User Account Disabled** :
    * **Verify :** Check user status in User Management page
    * **Solution** : Administrator re-enables the account
  * **Account Locked Due to Failed Login Attempts** : 
    * **Verify** : Check audit logs for repeated failed login attempts
    * **Solution** : Administrator unlocks the account (if implemented)
  * **LDAP Server Unavailable**(for LDAP users): 
    * **Verify** : Test LDAP connectivity from DLS instance
    * **Command** : _telnet 389_ or _telnet 636_
    * **Solution** : Restore LDAP server connectivity or use local account for emergency access
  * **User Not in Any Mapped LDAP Group** : 
    * **Verify** : Check user’s LDAP group memberships
    * **Verify** : Check if any groups are mapped in LDAP configuration
    * **Solution** : Add user to appropriate mapped LDAP group
  * **No Role Assigned** (local users): 
    * **Verify** : Check role assignments in User Management page
    * **Solution** : Administrator assigns appropriate role to user


###  [6.13.10.2. User Can Login But Receives 403 Forbidden Errors](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#user-can-login-but-receives-403-forbidden-errors)
During the backup process you may get a partition overlaps the last partition warning message. Use these steps to resolve the warning.
**Symptoms** : 
  * User successfully logs in
  * Operations fail with “403 Forbidden” or “Access Denied” errors
  * User cannot access expected features


**Possible Causes and Solutions** : 
  * **Insufficient Permissions for Operation**
    * **Verify** : Check user’s assigned role (DLS_ADMIN, DLS_OPERATOR, or DLS_USER)
    * **Verify** : Check permission requirements for the operation (see _Role Capabilities_)
    * **Solution** : Assign higher-privilege role if justified by business need


  * **Role Assignment Not Yet Effective** : 
    * **Verify** : Check effective date/time on role assignment
    * **Solution** : Wait for effective date or update effective date


  * **Outdated Session Token** : 
    * **Verify** : Check token expiration time
    * **Solution** : Log out and log back in to get new token with updated permissions


  * **LDAP User Mapped to Wrong Role** : 
    * **Verify** : Check LDAP group memberships
    * **Verify** : Check group-to-role mappings in LDAP configuration
    * **Solution** : Update LDAP group mappings or user’s group membership in LDAP directory


###  [6.13.10.3. LDAP Authentication Failures](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#ldap-authentication-failures)
**Symptoms** : 
  * LDAP users cannot log in
  * Error: “LDAP authentication failed”
  * Users with valid AD credentials rejected


**Possible Causes and Solutions** : 
  * Test LDAP connectivity telnet 192.0.2.50 389
  * Or for LDAPS telnet 192.0.2.50 636 
    * **Solution** : Verify network connectivity, firewall rules, and LDAP server is running


  * **Incorrect Search Base DN** : 
    * **Verify** : Use LDAP browser tool (e.g., Apache Directory Studio) to validate DN
    * **Example** : If users are in “OU=Employees,DC=example,DC=com”, use that as search base
    * **Solution** : Update LDAP configuration with correct search_base


  * **Incorrect Search Filter** : 
    * **Common Filters** : 
      * **Active Directory** : _( &(objectCategory=Person)(sAMAccountName=*))_
      * **OpenLDAP** : _(objectClass=inetOrgPerson)_
      * **Specific user** : _(uid={0})_ where {0} is replaced with username
    * **Solution** : Test filter using LDAP browser tool, then update configuration


  * **Wrong Username Attribute** : 
**Common Attributes**
    * **Active Directory** : _sAMAccountName_ or _userPrincipalName_
    * **OpenLDAP** : _uid_
  * **Solution** : Verify attribute in LDAP schema, update _username_attribute_ parameter


  * **CA Certificate Issues (for LDAPS)** : 
    * **Error** : “Certificate verification failed”
    * **Solution** : Ensure _ldap_tls_ca_certs_ contains the complete certificate chain in PEM format
    * **Verify** : Certificate is not expired


  * **Domain Mismatch (NTLM authentication)** : 
    * **Verify** : _domain_ parameter matches Active Directory domain
    * **Example** : For users in “EXAMPLE.COM”, set _"domain": "EXAMPLE"_
    * **Solution** : Update domain parameter to match AD domain


###  [6.13.10.4. LDAP User Assigned Wrong Role](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#ldap-user-assigned-wrong-role)
**Symptoms** : 
  * LDAP user logs in successfully
  * User has DLS_USER role but should have DLS_ADMIN
  * User has more or fewer permissions than expected


**Possible Causes and Solutions** : 
  * **Incorrect Group-to-Role Mapping** : 
    * **Verify** : Retrieve LDAP configuration: _GET /auth/v1/ldap_
    * **Verify** : Check _ldap_group_role_mapping_ for user’s groups
    * **Solution** : Update group mappings to reflect intended role assignments


  * **User Not in Expected LDAP Group** : 
    * **Verify** : Check user’s group memberships in Active Directory or LDAP
    * **Command (AD)** : _dsquery user -name "username" | dsget user -memberof -expand_
    * **Solution** : Add user to correct LDAP group


  * **Group DN Mismatch** : 
    * **Common Issue** : Group DN in configuration doesn’t match actual DN (case-sensitive!)
    * **Example** : Configuration has “cn=licenseadmins” but LDAP has “CN=LicenseAdmins”
    * **Solution** : Update group DN to match exactly (including case)


  * **Multiple Group Membership with Unexpected Privilege** : 
    * **Verify** : User may be in multiple groups with different roles
    * **Remember** : System assigns highest privilege role (lowest privilege_level number)
    * **Solution** : Remove user from groups that grant unintended elevated access


  * **Default Role Assignment** : 
    * **Scenario** : No group mappings configured
    * **Behavior** : All LDAP users get DLS_ADMIN by default
    * **Solution** : Configure _ldap_group_role_mapping_ to enforce specific role assignments


###  [6.13.10.5. API Key Authentication Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#api-key-authentication-issues)
**Symptoms** : 
  * API calls
  * return “401 Unauthorized”
  * Error: “Invalid API Key”
  * Error: “API Key does not have Enterprise scope”


**Possible Causes and Solutions** : 
  * **API Key Not Imported** : 
    * **Verify** : Check API Keys page in DLS Web UI
    * **Solution** : Import Enterprise API Key from NVIDIA Licensing Portal


  * **API Key Not Enterprise-Scoped** : 
    * **Verify** : Check API Key scope in NVIDIA Licensing Portal
    * **Solution** : Create new API Key with “Enterprise” entity access scope
    * **Note** : User/LDAP management APIs require Enterprise scope


  * **API Key Expired or Revoked** : 
    * **Verify** : Check API Key status in NVIDIA Licensing Portal
    * **Solution** : Regenerate or create new API Key


  * **Incorrect Header Format** : 
    * **Correct Format** : _x-api-key:_
    * **Incorrect** : _Authorization: Bearer_ (wrong header name)
    * **Solution** : Use _x-api-key_ header for API Key authentication


  * **API Key Permissions Issue** : 
    * **Verify** : API Key has appropriate permissions in Licensing Portal
    * **Solution** : Contact organization administrator to update API Key permissions


###  [6.13.10.6. Password-Related Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#password-related-issues)
**Symptoms** : 
  * Password change fails with validation error
  * Password rotation API returns 400 Bad Request
  * “Invalid new password” error message


**Possible Causes and Solutions** : 
  * **Password Doesn’t Meet Policy Requirements**
    * **Verify** : Password is 15-64 characters with uppercase, lowercase, digit, and special character
    * **Solution** : Generate stronger password or use auto-generated option


  * **Password Contains Username** : 
    * **Error** : “Password must not contain username”
    * **Solution** : Choose password that doesn’t include any part of username


  * **Password Too Similar to Previous** : 
    * **Error** : “Password must differ by at least 8 characters”
    * **Solution** : Choose significantly different password


  * **Password in History** : 
    * **Error** : “Password cannot be one of the last 5 passwords”
    * **Solution** : Choose a password not used recently


  * **Current Password Incorrect** : 
    * **Error** : “Current password verification failed”
    * **Solution** : Verify current password is correct
    * **Alternative** : Use password reset function (requires administrator)


###  [6.13.10.7. Session and Token Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#session-and-token-issues)
**Symptoms** : 
  * User randomly logged out
  * Operations fail with “401 Unauthorized” after successful login
  * “Token expired” error message


**Possible Causes and Solutions** : 
  * **Token Expiration** : 
    * **Default** : Tokens typically expire after 8 hours
    * **Solution** : Log out and log back in to get new token
    * **Prevention** : Enable “Remember Me” if supported


  * **Concurrent Sessions Limit Exceeded** : 
    * **Verify** : Check if multiple sessions are active
    * **Solution** : Log out from other sessions or close other browser windows


  * **Browser Cookie Issues** : 
    * **Solution** : Clear browser cookies for DLS domain
    * **Solution** : Try different browser or incognito mode


  * **System Time Mismatch** : 
    * **Verify** : Client system time matches DLS appliance time
    * **Solution** : Synchronize system clocks using NTP


###  [6.13.10.8. Limitations and Known Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#limitations-and-known-issues-trouble)
**Current Limitations**
  * **Pre-Defined Roles Only** : 
    * Only three roles available: DLS_ADMIN, DLS_OPERATOR, DLS_USER
    * Role permissions cannot be customized
    * Custom roles cannot be created
    * Users cannot be assigned to multiple roles simultaneously


  * **Single Role Assignment** : 
    * Each local user can have only one role assigned at a time
    * LDAP users receive a single role based on mapping logic
    * No role inheritance or combination supported


  * **LDAP Configuration Methods** : 
    * LDAP settings can be configured via DLS Web UI or RESTful API
    * Web UI provides user-friendly interface for interactive configuration
    * API enables programmatic configuration for automation and infrastructure-as-code
    * Enterprise API Key required for LDAP configuration


  * **Limited Self-Service for Local Users** : 
    * Users can change their own password and username
    * Users cannot change their own role assignment
    * Users cannot disable or delete their own accounts


  * **Group Membership Detection** : 
    * LDAP group membership is retrieved during authentication only
    * Changes to LDAP group membership require user to log out and log back in
    * Group membership is not cached or refreshed during active session


  * **API Key Requirements** : 
    * Enterprise-scoped API Keys required for user and LDAP management operations
    * Organization-scoped API Keys cannot be used for these operations
    * API Keys must be imported from NVIDIA Licensing Portal (cannot be generated locally)


###  [6.13.10.9. Known Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#known-issues-trouble)
  * **Issue** : LDAP authentication may fail immediately after DLS restart if network services are not fully initialized 
    * **Workaround** : Wait 30-60 seconds after DLS restart before attempting LDAP authentication
    * **Workaround** : Use local admin account for immediate post-restart access


  * **Issue** : Users with long usernames (>64 characters) may experience truncation in audit logs 
    * **Workaround** : Use usernames under 64 characters
    * **Impact** : Audit log entries may show truncated usernames


  * **Issue** : LDAP group DN comparison is case-sensitive; minor case differences prevent matching 
    * **Workaround** : Ensure exact case match in _ldap_group_role_mapping_ configuration
    * **Workaround** : Use LDAP browser tool to copy exact DN format


  * **Issue** : Password rotation for system user may require appliance restart in specific configurations 
    * **Workaround** : Schedule system user password rotation during maintenance windows
    * **Impact** : SSH access may be temporarily affected


  * **Issue** : If LDAP server becomes unavailable during active LDAP user session, token refresh may fail 
    * **Workaround** : Ensure LDAP server high availability
    * **Workaround** : Maintain local admin account for emergency access


###  [6.13.10.10. Rotation fails with forbidden (403)](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#rotation-fails-with-forbidden-403)
During the backup process you may get a partition overlaps the last partition warning message. Use these steps to resolve the warning.
**Description**
The vault cannot call SCIM, or DLS returns a forbidden response. 
**How to resolve**
  1. Confirm the target is a **DLS Virtual Appliance** , not Cloud License Service (CLS).
  2. Confirm SCIM is **enabled** on the appliance.
  3. Confirm the bearer token is current and correctly configured in the vault.
  4. Confirm the vault uses HTTPS to the correct DLS host name.


###  [6.13.10.11. User not found (404)](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#user-not-found-404)
During the backup process you may get a partition overlaps the last partition warning message. Use these steps to resolve the warning.
**Description**
The vault cannot resolve the account before setting a password. 
**How to resolve**
  * Confirm the vault username matches the DLS local userName **exactly** (spelling and case as stored on DLS).
  * Confirm the local user was created in the DLS Web UI or multi-user API before rotation.
  * Confirm you are not targeting an LDAP-only user for DLS SCIM rotation.


###  [6.13.10.12. Password rejected (400 / invalid value)](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#password-rejected-400-invalid-value)
**Description**
The vault generates a password, but DLS rejects it. 
**How to resolve**
  * Align vault password complexity with DLS password policy (length, character classes, username rules).
  * Allow the vault to regenerate and retry after rejection.
  * If history or similarity rules are involved, widen generation entropy so the new password differs enough from recent passwords.
  * Retry manual rotation after policy alignment.


###  [6.13.10.13. Rotation succeeds but login with the new password fails](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#rotation-succeeds-but-login-with-the-new-password-fails)
**Description**
The vault reports success, but interactive DLS login with the vault password fails. 
**How to resolve**
  1. Confirm you are signing in as the same local username the vault rotated.
  2. Confirm you are using the password currently stored in the vault (not an older checkout).
  3. Gather DLS log files as described in the User Guide and check for password-apply or system-account sync errors.
  4. Contact NVIDIA Support if the password appears updated in the vault but DLS login still fails after these checks.


###  [6.13.10.14. LDAP account mistakenly onboarded for DLS SCIM](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#ldap-account-mistakenly-onboarded-for-dls-scim)
**Description**
An Active Directory user was added to the vault as a DLS SCIM target. 
**How to resolve**
  1. Remove or retarget that account so password rotation happens in Active Directory / the identity platform.
  2. Use DLS SCIM only for **local** DLS users.


###  [6.13.10.15. Bearer token may be compromised](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#bearer-token-may-be-compromised)
**Description**
The SCIM token was exposed or a former administrator retained access. 
**How to resolve**
  1. Disable or replace the SCIM bearer token using your DLS SCIM token administration procedure for the release.
  2. Update the vault with the new token.
  3. Review recent SCIM password-set audit events for unexpected activity.


###  [6.13.11. Troubleshooting Single Sign-On](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-single-sign-on)
Use these topics when Single Sign-On to the DLS Web UI does not work as expected. 
Also see [**Troubleshooting Authentication and Authorization Issues**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-authentication-and-authorization-issues "Common Issues and Resolutions") in the published User Guide for non-SSO authentication problems. 
**SSO sign-in button does not appear**
**Description**
The DLS login page shows only username and password fields. `No Sign in with …` button is visible. 
**How to resolve**
  1. Confirm with an administrator that SSO is enabled on the appliance and that at least one SAML or OIDC identity source is **enabled**.
  2. Confirm that you are opening the correct DLS URL.
  3. Refresh the browser or try a private/incognito window to avoid a stale page.
  4. If LDAP identity sources are enabled, remember that LDAP and SSO cannot be enabled together; the appliance may be on LDAP-only mode.


**Sign-in fails after completing identity provider login**
**Description**
You authenticate successfully at the corporate identity provider, but DLS shows an error or returns you to a failed login state. 
**How to resolve**
  1. Confirm that the return URL (ACS or OIDC callback) registered on the IdP **exactly** matches DLS (HTTPS, host name, path).
  2. Confirm that the application / entity ID (audience) on the IdP matches DLS.
  3. Confirm that users open DLS with that same public host name.
  4. Check clock synchronization on DLS and the IdP.
  5. For SAML, confirm the IdP signing certificate configured on DLS matches the certificate the IdP uses to sign assertions.
  6. Gather DLS log files as described in the User Guide and provide them to NVIDIA Support if the issue persists.


**User signs in with the wrong DLS role**
**Description**
Sign-in succeeds, but the user has User access instead of Administrator (or another unexpected role). 
**How to resolve**
  1. Confirm the user’s group membership in the identity provider.
  2. Confirm DLS group mapping uses the **exact** group names from the assertion or token (case-sensitive).
  3. Confirm the groups attribute / claim name on DLS matches what the IdP sends.
  4. Remember that membership in multiple mapped groups yields the **lowest-privilege** matching role.
  5. If no user or group mapping matches, check the effective source default role and deployment fallback. These defaults apply only after mapping lookup finds no match.


**Sign-in is denied for an authenticated corporate user**
**Description**
The identity provider accepts the user, but DLS rejects the session. 
**How to resolve**
  1. Check the user's direct mapping, group mappings, and effective default-role policy. An unset source default alone does not imply rejection.
  2. DLS may still assign the deployment fallback, which is User by default. Providers configured through the current SAML
  3. Web UI set User as the default. If the effective policy rejects unmapped users, assign an appropriate user or group mapping before retrying.


**Multiple SSO buttons — which one to use?**
**Description**
More than one `Sign in with …` button appears. 
**How to resolve**
  1. Use the provider your administrator specified for your organization or environment.
  2. DLS does not automatically try the other buttons after a failure; pick the correct one deliberately.


**Identity provider requires MFA every time**
**Description**
Users always see a second-factor prompt at the IdP. 
**How to resolve**
  1. This is controlled by the identity provider’s MFA / access policy, not by DLS.
  2. Adjust MFA policy in the IdP administration console (for example remember device, intranet vs extranet rules).


**Cannot download identity provider metadata from DLS**
**Description**
Automatic metadata or discovery download from DLS fails; browsers can still reach the IdP. 
**How to resolve**
  1. For SAML, configure pasted metadata or manual identity-provider details if DLS cannot download metadata. If you use a metadata URL, verify DNS, network access, and the trusted CA certificates required for its HTTPS connection.
  2. For OIDC, restore DLS connectivity to the issuer's discovery, token, and signing-key endpoints. Check DNS, firewall rules, and HTTPS certificate trust. Successful browser access to the provider does not verify access from DLS.
  3. Retry sign-in after resolving connectivity. Pasting SAML metadata or entering manual SAML details cannot resolve an OIDC connectivity failure.


**Password login no longer works**
**Description**
SSO works or is enabled, but local username/password fields fail or are missing. 
**How to resolve**
  1. Open the DLS login page and use the username and password fields for the protected local administrator account. Verify that you are using the local account credentials, not the identity-provider credentials.
  2. If you have forgotten the protected administrator password, use Forgot password? and the local reset secret saved during registration, as described in Registering the DLS Administrator User.


The current Web UI has no password-login enable or disable setting. If your deployment applies a separate authentication policy outside the Web UI, follow the procedure for that specific deployment. 
###  [6.13.12. DLS license server setup on Open Shift deployment fails](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#openshift-deployment-fails)
During NVIDIA DLS license server setup on OpenShift, the DLS appliance pod fails to start or reports permission errors.
  * Could not modify permissions for /var/log : [Errno 1] Operation not permitted: '/var/log
  * Could not modify permissions for /bitnami/rabbitmq/mnesia : [Errno 1] Operation not permitted: '/bitnami/rabbitmq/mnesia'


Resolution 
  1. Set ownership as follows: 
Copy
Copied!

```
            
            
chown -R 65532:65532 /etc/dls/network 

chown -R 65532:65532 /bitnami/rabbitmq/mnesia 

chown -R 65532:65532 /var/log 

chown -R 70:70 /var/lib/postgresql/data

        
```

If the volumes are backed by external storage, update the ownership from the storage backend or from a node/admin maintenance pod with sufficient permissions.
  2. After correcting ownership, restart the DLS pod:`$ oc delete pod <dls-pod-name> -n <namespace>`
OpenShift recreates the pod, and the DLS appliance should be able to write to the required directories without permission errors.
Note:
If permission issues continue, verify that the OpenShift SCC assigned to the DLS service account allows the required non-root UID behaviour and volume access. This applies Red Hat OpenShift and NVIDIA Delegated License Service . 


###  [6.13.13. Troubleshooting Multi-Domain LDAP](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-multi-domain-ldap)
Use these topics when LDAP sign-in to the DLS Web UI does not work as expected in multi-domain or multi-identity-source deployments. 
Also see [Troubleshooting Authentication and Authorization Issues](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#troubleshooting-authentication-and-authorization-issues "Common Issues and Resolutions") in the published User Guide for general LDAP problems. 
###  [6.13.13.1. Child-domain users cannot sign in one forest](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#child-domain-users-cannot-sign-in-one-forest)
**Description**
Users in the forest root can sign in, but users in a child domain cannot. Credentials and group membership appear correct in Active Directory. 
**How to resolve**
  1. Confirm DLS connects to a **Global Catalog** host on port **3268** or **3269** , not only a domain-specific port **389** / **636**.
  2. Confirm the search base is the **forest root** DN and search scope is **SUBTREE**.
  3. Confirm **Follow referrals** is enabled.
  4. Confirm the search filter and group DNs still match users in the child domain.
  5. If the directory is large, increase or set the **max page limit** so paged search can return the user.


###  [6.13.13.2. **User in a second directory cannot sign in with a plain username**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#user-in-second-directory-cannot-sign-in-with-plain-username)
**Description**
A user in one of the configured LDAP directories cannot sign in with a plain username. Plain usernames use enabled sources in priority order and can be ambiguous when the same username exists in more than one directory. 
**How to resolve**
  1. Sign in with `<DOMAIN>\<username>` or `<username>@<domain>`, using the domain or alias configured for the intended identity source.
  2. Confirm that the intended LDAP identity source is enabled and its domain, alias, and directory-search settings match the account.
  3. Verify the user's password and role mappings for that source. Invalid credentials and role-mapping denial stop the attempt rather than triggering another-source retry.


###  [6.13.13.3. Correct password rejected after adding a second identity source](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#correct-password-rejected-after-adding-second-identity-source)
**Description**
The user could sign in before a second directory was added; afterward, login fails even with the correct password. 
**How to resolve**
  1. Confirm the user is using the correct **domain-qualified** username for their directory.
  2. Confirm domain / alias / DNS domain fields on the identity source match what users type.
  3. Confirm wrong passwords do not fall through — verify the password on the intended directory.
  4. Confirm group-to-role mapping exists on the source that handles this user.


###  [6.13.13.4. User receives an unexpected DLS role](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#user-receives-unexpected-dls-role)
**Description**
Sign-in succeeds, but the role is not the one the administrator expected. 
**How to resolve**
  1. Confirm group membership in the directory that actually authenticated the user.
  2. Confirm group DNs on **that identity source** match exactly (case-sensitive).
  3. Remember mappings do not automatically copy from one identity source to another.
  4. Review multi-group privilege behavior in the published LDAP mapping topic for your release.


###  [6.13.13.5. Test connection succeeds but login fails](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#test-connection-succeeds-but-login-fails)
**Description**
The LDAP test on the configuration page succeeds, but interactive Web UI login fails for some users. 
**How to resolve**
  1. Confirm the test used a representative user from the same domain population that is failing.
  2. Confirm search filter and group mapping allow that user for interactive login.
  3. For multi-source setups, test each source and try domain-qualified login for non-default directories.
  4. Gather DLS log files as described in the User Guide and provide them to NVIDIA Support if the issue persists.


## [7. Managing License Allocations on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#registering-license-server-and-getting-license-files)
After creating a license allocation on the NVIDIA Licensing Portal, you can add licenses to and remove licenses from the applicance, add new licensed products to and remove licensed products from the server, and delete the appliance when you no longer require it. 
###  [7.1. Where to Perform Tasks for Managing a License Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#where-to-perform-tasks-for-managing-license-server)
Where to perform the tasks for managing a license server depends on the task and on the type of service instance on which the license allocation is installed.  
| Task  | CLS Instance  | DLS Instance  |  
| --- | --- | --- |  
| Adding licenses to a license allocation  | NVIDIA Licensing Portal  | NVIDIA Licensing Portal  |  
| Removing licenses from a license allocation  | NVIDIA Licensing Portal  | The NVIDIA Licensing application on the virtual appliance that hosts the DLS instance  |  
| Adding licensed products to a license allocation  | NVIDIA Licensing Portal  | NVIDIA Licensing Portal  |  
| Removing licensed products from a license allocation  | NVIDIA Licensing Portal  | The NVIDIA Licensing application on the virtual appliance that hosts the DLS instance  |  
| Moving a license allocation to another virtual group  | NVIDIA Licensing Portal  | NVIDIA Licensing Portal  |  
| Deleting a license allocation  | NVIDIA Licensing Portal  | NVIDIA Licensing Portal  |  
###  [7.2. Roles Required for Managing License Allocations on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#roles-for-managing-license-servers-on-portal)
The role that these tasks require depends on whether they are being performed for an organization or a virtual group.
  * For an organization, these tasks require the [Organization Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-administrator "An organization administrator has the highest level of visibility and access within an organization. The person that created the organization's NVIDIA Enterprise Account is initially assigned the organization administrator role.") or the [Organization User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-user "In version 3.4.0 and later, the Organization User in the default virtual group is redefined as a Virtual Group User.") role.
  * For a virtual group, these tasks require the [Virtual Group Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-administrator "A virtual group administrator has restricted visibility within an organization and can access items and manage contacts only in the virtual group to which the virtual group administrator is assigned. A virtual group administrator is a contact that has been added to the virtual group as an admin user.") or the [Virtual Group User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-user "A virtual group user has no visibility within an organization and can view and access items only in a virtual group. A virtual group user is a contact that has been added to the virtual group as a base user.") role.


###  [7.3. Managing Licenses and Licensed Products on a License Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#adding-removing-licenses-on-license-server)
Manage the licenses on a license allocation to add or return individual licenses for a specific licensed product. You can also add licensed products to or remove licensed products from an allocation. In the NVIDIA Licensing Portal, licensed products are represented by feature instances. When adding a feature instance, specify the number of licenses to assign to it. When removing a feature instance, all licenses assigned to that feature instance are returned to the entitlement.
###  [**Adding Licensed Products**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#adding-removing-licenses-on-license-server__section_a4y_fqj_vkc)
Perform this task in the NVIDIA Licensing Portal. 
  1. Open the actions menu for the allocation and select **Update License Allocation**.
  2. Add compatible feature instances or increase the number of licenses assigned to existing feature instances.
  3. Review the feature instances and quantities, and then save the allocation update. 
If the allocation is deployed to CLS, the update takes effect automatically. No further action is required.
If the allocation is deployed to DLS:
    1. Download the updated license allocation file from the NVIDIA Licensing Portal.
    2. Log in to the DLS appliance and open **License Allocation**.
    3. Under **License Allocation** , select **Update License File**.
    4. Select the downloaded .bin file or drag it into the upload area.
Selecting a valid file starts the update immediately. After the update succeeds, open **License Allocation** and verify the updated feature instances and quantities.


###  [**Returning Licensed Products from a CLS Allocation**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#task_hws_krj_vkc)
Perform this task in the NVIDIA Licensing Portal.
  1. If you are not already logged in, log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Find the cloud license allocation that you want to update.
  4. Open the **More CLS Actions** menu and select **Update License Allocation**.
  5. Select **Disable leasing** and confirm the action when prompted. While leasing is disabled, the allocation cannot fulfill new client licensing requests. Existing leases are not forcibly released.
  6. Reduce the license quantities assigned to the applicable feature instances. To remove a feature instance, return all licenses assigned to it.
  7. Review the changes and save the allocation update.
  8. Select **Enable leasing** to allow the allocation to fulfill new client licensing requests. The returned licenses are added to the applicable entitlements and can be assigned to other license allocations.


###  [Returning Licensed Products from a DLS Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#task_frz_qtj_vkc)
Perform the first part of this task on the DLS appliance.
  1. Log in to the DLS appliance and open **License Allocation**.
  2. Under **Allocation controls** , select **Return Features**.
  3. If leasing is enabled, select **Disable leasing** and confirm the action when prompted. While leasing is disabled, the allocation cannot fulfill new client licensing requests.
  4. Select each feature instance from which you want to return licenses.
  5. In **Quantity to return** , enter the number of licenses to remove from DLS, not the number of licenses that should remain. 
For example, to return 4 of 10 licenses and leave 6 licenses on DLS, enter 4.
For each selected feature instance, enter a quantity from 1 through the displayed maximum, or select **Use max**. If the entire feature instance is unused, **Use max** returns all its licenses.
  6. Review the quantities and select **Generate return file**.Generating the return file immediately removes the selected quantities from DLS. The licenses do not become available for other allocations until the return file is processed in the NVIDIA Licensing Portal.
  7. Select **Download Return File** and save the generated file.
  8. If you disabled leasing, return to **Allocation controls** and select **Enable leasing**. You can resume leasing without waiting for the return file to be processed in the NVIDIA Licensing Portal.
  9. Process the saved return file as described in[ _Returning Licenses from a License Allocation on a DLS Instance to the NVIDIA Licensing Portal_](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#returning-licenses-from-dls-instance-to-portal). If the operation is interrupted, reopen **Return Features** and check the return status before entering any quantities again: 
     * If **Export pending return** is displayed, the quantities have already been removed from DLS. Select **Export pending return** , and then select **Download Return File**.
     * If the return file was already exported but must be downloaded again, use **Re-download** under **the Previous return file** , when available.
     * If DLS cannot determine the return status, select **Check return status** before continuing.
     * Do not submit the same return quantities more than once.


###  [7.4. Returning Licenses from a License Allocation on a DLS Instance to the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#returning-licenses-from-dls-instance-to-portal)
After generating a license-return file on the DLS appliance, process the file on the NVIDIA Licensing Portal. The returned licenses are not available for assignment to another license allocation until the return file has been processed.
###  [Getting the return file](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#task_dc3_p4d_vkc)
  1. Use the file saved with Download Return File in the DLS Return Features procedure. Do not generate a second return for the same quantities.
  2. If a return is still pending on DLS, open License Allocation > Allocation controls > Return Features, select Export pending return, then select Download Return File.
  3. If the return was already exported but you need the file again, reopen Return Features and use Re-download under the Previous return file when it is available.
  4. Continue with the following NVIDIA Licensing Portal import steps using the downloaded return file.


###  [Processing the return file](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#task_ytb_44d_vkc)
  1. If you are not already logged in, log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL**.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Find the DLS license allocation from which the licenses were returned.
  4. Click on **Upload feature return**.
  5. Select or drag and drop the license-return file generated by the DLS appliance.
  6. Review the returned feature instances and quantities, and then confirm the return.
  7. Verify that the returned licenses are available in the applicable entitlements. 
Process each return file only once.


###  [7.5. Moving a License Allocation to Another Virtual Group](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#moving-license-server-to-other-virtual-group)
  1. If you are not already logged in, log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click NVIDIA LICENSING PORTAL to go to the NVIDIA Licensing Portal.
  2. From the top navigation bar, select **License Management** , and then select **License Allocations**.
  3. Select the license allocation that you want to move.
  4. Click **Move allocation**.
  5. Select the virtual group to which you want to move the license allocation.
  6. Click **Move allocation** to confirm.
  7. The license allocation is moved to the selected virtual group.


###  [7.6. Deleting a License Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deleting-license-server)
**Deleting an eligible license allocation:**
  1. In the NVIDIA Licensing Portal, navigate to the organization or virtual group for which you want to delete the license allocation. a. If you are not already logged in, log in to the NVIDIA Enterprise Application Hub and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal. b. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Info window.
  2. In the list of license allocations on the License Allocations page that opens, click the name of the license allocation that you want to delete.
  3. In the License Allocations page that opens, from the **Actions** menu, choose **Delete**.
  4. When asked to confirm that you want to delete the license allocation, click **DELETE LICENSE ALLOCATION**.


## [8. Managing Contacts on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#managing-users-nvidia-license-portal)
To help you manage your entitlements and licenses on the NVIDIA Licensing Portal, you can add other users as registered contacts in the organization associated with your NVIDIA Enterprise Account. You can also remove users who no longer require access from your account on the NVIDIA Licensing Portal.
To secure your entitlements and licenses, NVIDIA Licensing Portal provides role-based access for all registered contacts. Each role has a scope that determines whether the role applies to a virtual group within an organization or the organization itself. For more information, see [Role-Based Access to an Organization and Virtual Groups](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#role-based-access-to-virtual-groups "Role-based access helps secure the entitlements and licenses in your organization on the NVIDIA Licensing Portal. If you partition your entitlements into isolated segments, role-based access also provides isolation between the segments into which your entitlements are partitioned. It does so by ensuring that only specific contacts in your organization are allowed to view or perform actions on the entitlements and contacts that are allocated to a virtual group."). 
###  [8.1. Role-Based Access to an Organization and Virtual Groups ](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#role-based-access-to-virtual-groups)
Role-based access helps secure the entitlements and licenses in your organization on the NVIDIA Licensing Portal. If you partition your entitlements into isolated segments, role-based access also provides isolation between the segments into which your entitlements are partitioned. It does so by ensuring that only specific contacts in your organization are allowed to view or perform actions on the entitlements and contacts that are allocated to a virtual group.
A role is a collection of actions or capabilities within the NVIDIA Licensing Portal. Each role has a scope that determines the context to which the actions and capabilities of the role apply, specifically, a virtual group within an organization or the organization itself. 
Every registered contact has at least one role, but can have multiple roles if the scope of each role is a virtual group. As a result, a contact can be a member of multiple virtual groups. However, roles with a virtual group scope and roles with an organization scope are mutually exclusive. A contact that has a virtual group role cannot also have an organization role. 
To enable role-based access to an organization and virtual groups, the NVIDIA Licensing Portal provides pre-defined roles. 
###  [8.1.1. Organization Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-administrator)
An organization administrator has the highest level of visibility and access within an organization. The person that created the organization's NVIDIA Enterprise Account is initially assigned the organization administrator role. 
Each organization must have at least one organization administrator. Multiple organization administrators in an organization are allowed. To prevent the absence of a single user from denying you access to your organization's entitlements, consider adding at least two organization administrators to your organization. 
An organization administrator can see all of the following items for the organization on the NVIDIA Licensing Portal: 
  * Entitlements
  * Users
  * Virtual groups
  * License servers provisioned from the entitlements that have not been assigned to a virtual group


An organization administrator can mange virtual groups as follows: 
  * Create a virtual group.
  * Delete a virtual group.
  * Assign an entitlement at the organization level to a virtual group.
  * Remove an entitlement from a virtual group and return it to the organization.


An organization administrator can manage other contacts in the organization as follows: 
  * Invite a contact currently not within the organization to register at the organization level.
  * Add users and administrators to a virtual group when creating the virtual group.
  * Delete any administrator or user at either the organization level or the virtual group level **except** the last virtual group administrator in a virtual group.
  * Manage the role of any organization-level contact.


An organization administrator also has all the capabilities of an organization user. 
###  [8.1.2. Organization User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-user)
In version 3.4.0 and later, the Organization User in the default virtual group is redefined as a Virtual Group User. 
###  [8.1.3. Virtual Group Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-administrator)
A virtual group administrator has restricted visibility within an organization and can access items and manage contacts only in the virtual group to which the virtual group administrator is assigned. A virtual group administrator is a contact that has been added to the virtual group as an admin user. 
Each virtual group must have at least one virtual group administrator. Multiple virtual group administrators in a virtual group are allowed. To prevent the absence of a single user from denying you access to a virtual group, consider adding at least two virtual group administrators to each virtual group in your organization. 
A virtual group administrator can see the following items on the NVIDIA Licensing Portal: 
  * All organization administrators
  * All other contacts in the virtual group
  * All entitlements assigned to the virtual group
  * All license servers provisioned from entitlements assigned to the virtual group


A virtual group administrator can manage other contacts in the virtual group as follows: 
  * Add an exiting contact within the organization who is **not** an organization administrator to the virtual group.
  * Invite a contact currently not within the organization to register and join the virtual group.
  * Remove any other contact in the virtual group, regardless of the contact's role. 
Virtual group administrators cannot remove themselves from a virtual group.
  * Manage the role of any other contact in the virtual group. 
Virtual group administrators cannot manage their own roles.


A virtual group administrator also has all the capabilities of a virtual group user. 
###  [8.1.4. Virtual Group User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-user)
A virtual group user has no visibility within an organization and can view and access items only in a virtual group. A virtual group user is a contact that has been added to the virtual group as a base user. 
Note:
An Organization User is defined as a Virtual Group user. 
A virtual group can have no virtual group users, only one virtual group user, or multiple virtual group users. 
A virtual group user can see the following items for the virtual group on the NVIDIA Licensing Portal: 
  * All other contacts in the virtual group
  * All entitlements assigned to the virtual group
  * All license servers provisioned from entitlements assigned to the virtual group


A virtual group user can mange entitlements within a virtual group as follows: 
  * Create a license server.
  * Delete a license server.
  * Add licensed products to a license server.
  * Remove licensed products from a license server.
  * Download a license file.
  * Download software.


A virtual group user **cannot** manage other contacts. 
###  [8.2. Roles Required for Managing Contacts on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#roles-for-managing-users-nvidia-license-portal)
The role that these tasks require depends on whether they are being performed for an organization or a virtual group.
  * For an organization, these tasks require the [Organization Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-administrator "An organization administrator has the highest level of visibility and access within an organization. The person that created the organization's NVIDIA Enterprise Account is initially assigned the organization administrator role.") role.
  * For a virtual group, these tasks require the [Virtual Group Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-administrator "A virtual group administrator has restricted visibility within an organization and can access items and manage contacts only in the virtual group to which the virtual group administrator is assigned. A virtual group administrator is a contact that has been added to the virtual group as an admin user.") role.


###  [8.3. Adding a Contact on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#adding-user-nvidia-licesning-portal)
  1. In the NVIDIA Licensing Portal, navigate to the organization or virtual group to which you want to add a contact. 
    1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
    2. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Info window.
  2. From the top navigation bar, select **Organization** , and then select **Users**.
  3. On the **Users** page, click **Add User**
  4. In the Add User pop-up window that opens, provide the e-mail address and the name of the contact, select the contact's role, and click **Add User**. 
The role to select depends on whether you are adding the contact to an organization or a virtual group.
     * For an organization, select one of the following roles:  

**ORG ADMIN** 
     Assigns the contact the [Organization Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-administrator "An organization administrator has the highest level of visibility and access within an organization. The person that created the organization's NVIDIA Enterprise Account is initially assigned the organization administrator role.") role.  

**ORG USER** 
     Assigns the contact the [Organization User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-user "In version 3.4.0 and later, the Organization User in the default virtual group is redefined as a Virtual Group User.") role. 
     * For a virtual group, select one of the following roles:  

**VIRTUAL GROUP ADMIN** 
     Assigns the contact the [Virtual Group Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-administrator "A virtual group administrator has restricted visibility within an organization and can access items and manage contacts only in the virtual group to which the virtual group administrator is assigned. A virtual group administrator is a contact that has been added to the virtual group as an admin user.") role.  

**VIRTUAL GROUP USER** 
     Assigns the contact the [Virtual Group User](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-virtual-group-user "A virtual group user has no visibility within an organization and can view and access items only in a virtual group. A virtual group user is a contact that has been added to the virtual group as a base user.") role. 


  * When a user is added to the NVIDIA Licensing Portal, an email is sent to the organization administrators providing the user and virtual group details.
  * If you added a contact who is already registered to a virtual group, the contact will be able the select the virtual group after next logging in.
  * If the contact is a new contact, an e-mail is sent to the contact at the e-mail address that you provided. 
Note:
The link to the NVIDIA Enterprise Support Portal in this e-mail provides information about how to contact NVIDIA Enterprise Support. 


If the contact that you added is a new contact, tell the contact to follow the directions in the e-mail to create an NVIDIA Enterprise Account. 
###  [8.4. Removing a Contact on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#removing-user-nvidia-licesning-portal)
  1. In the NVIDIA Licensing Portal, navigate to the organization or virtual group from which you want to remove a contact. 
    1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
    2. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Info window.
  2. From the top navigation bar, select **Organization** , and then select **Users**.
  3. On the **Users** page, find the user that you want to remove and select **Remove user** from the **Actions** menu. 
Note:
You cannot remove the only virtual group administrator from a virtual group. The **Remove** link for that contact is inactive and dimmed. 
  4. When prompted, confirm that you want to remove the contact.


The contact is removed from the list of contacts on the USER MANAGEMENT page. 
  * A contact that is removed from an organization is removed from the list of registered contacts for the organization.
  * A contact that was a member of multiple virtual groups when removed from a virtual group remains a member of the other virtual groups.
  * A contact that was a member of only the virtual group from which you removed the contact is returned to the organization with the organization user role.


###  [8.5. Changing the Role of a Contact on the NVIDIA Licensing Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#changing-user-nvidia-licensing-portal-role)
You can change the role of a contact on the NVIDIA Licensing Portal within the scope of the contact's current role. For example, you can change the role from organization user to organization administrator or from virtual group administrator to virtual group user. However, you **cannot** change the scope of the contact's current role, for example, from organization administrator to virtual group user.
  1. In the NVIDIA Licensing Portal, navigate to the organization or virtual group to which the contact belongs. 
    1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
    2. **Optional:** If your assigned roles give you access to multiple virtual groups, click **View settings** at the top right of the page and in the My Info window that opens, select the virtual group from the **Virtual Group** drop-down list, and close the My Info window.
  2. From the top navigation bar, select **Organization** , and then select **Users**.
  3. On the **Users** page, find the user whose role you want to change and select **Change role** from the **Actions** menu You are shown the contact's current role and the role to which you can change it.
  4. In the Change user role window, select the new role for the user and click **Save changes** to change the user’s role.


###  [8.6. Requesting Access to the NVIDIA Enterprise Support Portal](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#requesting-access-to-nvidia-enterprise-support-portal)
If you created your NVIDIA Enterprise Account when your entitlement contained only evaluation licenses, you do not have access to the NVIDIA Enterprise Support Portal. If you now have an entitlement that contains purchased licenses, you can request access to the NVIDIA Enterprise Support Portal.
Ensure that you have an entitlement that contains purchased licenses. You cannot request access to the NVIDIA Enterprise Support Portal if your entitlements contain only evaluation licenses.
  1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
  2. From the top navigation bar, select **Organization** , and then select **Users**.
  3. On the **Users** page that opens, click **Request Enterprise Support Access**.


## [9. Managing Virtual Groups](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#managing-virtual-groups)
Virtual groups provide the means for segmenting your organization's entitlements into partitions. The virtual groups in an organization are isolated from each other and from the organization. An entitlement cannot be partitioned and cannot be shared between partitions. All licensed products in an entitlement are moved with the entitlement when the entitlement is added to a virtual group or returned to the organization.
You are free to determine how many virtual groups among which to partition your entitlements and what those virtual groups represent. For example, you might create virtual groups to partition your entitlements by location, division, product, or some combination of factors. Irrespective of how you choose to partition your entitlements among virtual groups, every virtual group isolates the entitlements assigned to it from other virtual groups. 
The following diagram shows the relationship between an organization, the virtual groups in an organization, and the components of a virtual group. 
###  [9.1. Roles for Managing Virtual Groups](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#roles-for-managing-virtual-groups)
These tasks require the [Organization Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-administrator "An organization administrator has the highest level of visibility and access within an organization. The person that created the organization's NVIDIA Enterprise Account is initially assigned the organization administrator role.") role. 
###  [9.2. Creating a Virtual Group](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-a-virtual-group)
Ensure that the following prerequisites are met: 
  * Your organization contains at least one registered contact to whom you can assign the virtual group administrator role.
  * No licensed products in any of the entitlements that you want to add to the group have been added to a license server.


  1. If you are not already logged in, log in to the NVIDIA Licensing Portal.
  2. From the top navigation bar, select **Organization** , and then select **Virtual Groups**.
  3. On the Virtual Groups page, click **Create virtual group**.
  4. In the **Name** field, enter a name for the virtual group.
  5. In the **Description** field, enter a brief description of the virtual group.
  6. Under **Entitlement product keys** , select at least one product key to move into the virtual group.
  7. Under **Users** , optionally select the users to add to the virtual group and assign their virtual group roles.
  8. On the Preview server creation page, click **CREATE VIRTUAL GROUP**.


After you create a virtual group, you can perform only the following operations on the virtual group: 
  * Deleting the virtual group
  * Assigning an entitlement at the organization level to the virtual group
  * Removing an entitlement from the virtual group and returning it to the organization


Other operations on the virtual group require the virtual group administrator or virtual group user role. 
###  [9.3. Deleting a Virtual Group](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#deleting-a-virtual-group)
Delete a virtual group if it is no longer needed. When the group is deleted, all entitlements assigned to the group and any contacts who are members only of this group are returned to the organization. Contacts who are returned to the organization are assigned the organization user role. Default virtual groups cannot be deleted.
  1. If you are not already logged in, log in to the NVIDIA Licensing Portal.
  2. From the top navigation bar, select Organization, and then select Virtual Groups.
  3. On the Virtual Groups page, find the virtual group that you want to delete and select Delete virtual group from its Actions menu.
  4. In the confirmation dialog, click Delete virtual group.


###  [9.4. Adding a Contact to a Virtual Group](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#adding-contact-to-virtual-group)
If you have the Organization Administrator role, you can add users to any virtual group in your organization without being a member of the group. Organization Administrators already have organization-wide access to every virtual group. Their roles and access cannot be changed from the virtual group page.
The contact that you add must **not** have the [Organization Administrator](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#virtual-groups-organization-administrator "An organization administrator has the highest level of visibility and access within an organization. The person that created the organization's NVIDIA Enterprise Account is initially assigned the organization administrator role.") role. 
  1. Log in to the NVIDIA Licensing Portal.
  2. From the top navigation bar, select **Organization** , and then select **Virtual Groups**.
  3. Select the virtual group to which you want to add the user.
  4. Click **Manage users**.
  5. Under **Add users** , select **New user**.
  6. Enter the user’s **Email** and **Name**.
  7. Select one of the following roles: 
    1. **Virtual Group Admin:** Assigns the Virtual Group Administrator role.
    2. **Virtual Group User:** Assigns the Virtual Group User role.
  8. Click **Add user**. An email is sent to the contact at the e-mail address that you provided. 
    1. If the contact is **not** already registered on the NVIDIA Licensing Portal, the e-mail provides directions for creating an NVIDIA Enterprise Account.
    2. Otherwise, the e-mail provides details about the contact's role change
If the contact is **not** already registered on the NVIDIA Licensing Portal, tell the contact to follow the directions in the e-mail to create an NVIDIA Enterprise Account.


###  [9.5. Removing a Contact from a Virtual Group](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#removing-contact-from-virtual-group)
If you have the Organization Administrator role, you can remove a contact from a virtual group in your organization without the need to be a member of the group. The contact that you remove is returned to the organization and assigned the Organization User role.
  1. Log in to the NVIDIA Licensing Portal.
  2. From the top navigation bar, select **Organization** , and then select **Virtual Groups**.
  3. Select the virtual group from which you want to remove the user.
  4. Click **Manage users**.
  5. Select the **Remove users** tab.
  6. Select the user that you want to remove.
  7. Click **Remove user** , and then confirm the removal when prompted.


###  [9.6. Managing Entitlements in a Virtual Group](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#adding-virtual-group-entitlements)
Remove an entitlement from a virtual group to return it to the organization either to make it available to users at the organization level or to transfer it to a different virtual group.
Ensure that the following prerequisites are met: 
  * The entitlements that you want to add belong to the organization and not to a virtual group.
  * No licensed products in any of the entitlements that you want to add to the group have been added to a license server.


Ensure that no licensed products in the entitlement that you want to remove have been added to a license server. 
  1. If you are not already logged in, log in to the NVIDIA Licensing Portal.
  2. From the top navigation bar, select **Organization** , and then select **Virtual Groups**.
  3. Select the virtual group whose entitlements you want to manage.
  4. Click **Manage entitlements**.
  5. To add entitlement product keys: 
     * Select the **Add entitlements** tab.
     * Select the available product keys that you want to move from the organization pool into the virtual group.
  6. Product keys that cannot be moved are not listed.
  7. To return entitlement product keys: 
     * Select the **Return entitlements** tab.
     * Select the product keys that you want to return to the organization pool.
  8. Click **Apply changes**. 
You cannot return a product key if its features are assigned to a license allocation or if it is the only product key in the virtual group.


###  [9.7. Sample Business Scenario for Virtual Groups](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#mnc-scenario-vgroups)
A common business scenario for virtual groups is a multinational corporation with subsidiaries in which licenses are managed centrally.
###  Organization Administrators 
The organization administrators are responsible for setting up virtual groups, managing entitlements, and managing license servers for the entire organization. The individuals chosen to be organization administrators must understand the organization structure and purchasing process, so that they are capable of routing newly purchased entitlements appropriately. 
To ensure that someone is always available to move newly purchased entitlements into the correct virtual group, consider designating **at least three** organization administrators. 
###  Virtual Groups 
To simplify the allocation entitlements to the entity for which they were purchased, consider creating a virtual group for every subsidiary or geographic region, as appropriate. 
###  Virtual Group Contacts 
To ensure redundancy at every level in your organization, designate **at least two** virtual group administrators for each virtual group. 
After a virtual group is created, its virtual group administrators are free to add contacts who are **not** organization administrator as required. 
## [A. Migrating Licenses from a Legacy NVIDIA vGPU Software License Allocation](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#migrating-licenses-from-legacy-vgpu-software-license-server)
Legacy license servers are migrated automatically by the system. No manual conversion is required.
This work flow consists of several separate phases. Work through the phases in the order in which they are presented. 
###  [A.1. Tasks for Decommissioning a Legacy NVIDIA vGPU software License Server](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#tasks-decommissioning-legacy-vgpu-software-license-server)
Once your licenses have been migrated and successfully tested on the NVIDIA License System, you can proceed with decommissioning your existing legacy NVIDIA vGPU software license servers.s.   
| Task  | Cross-Reference  |  
| --- | --- |  
| Remove the legacy NVIDIA vGPU software license server from your Windows or Linux platforms.  | 
  * [Uninstalling the NVIDIA vGPU Software License Server on Windows](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#uninstalling-nvidia-grid-license-server-windows)
  * [Uninstalling the NVIDIA vGPU Software License Server on Linux](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#uninstalling-nvidia-grid-license-server-linux)

 |  
###  [A.1.1. Uninstalling the NVIDIA vGPU Software License Server on Windows](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#uninstalling-nvidia-grid-license-server-windows)
If you want to uninstall the license server in silent mode, ensure that the license server installation directory contains the installer.properties file.
For information about the installer.properties file, refer to [Virtual GPU Software License Server User Guide](https://docs.nvidia.com/grid/ls/latest/grid-license-server-user-guide/index.html). 
  1. Start the license server uninstaller. 
You can start the license server uninstaller from Windows Control Panel, Windows Explorer, or a Command Prompt window.
If you start the license server uninstaller from Windows Control Panel or Windows Explorer, how it runs depends on the mode in which license server software was installed. If the license server software was installed in console mode or silent mode, the uninstaller runs in **silent** mode. Otherwise, the Configure License Server dialog box opens.
To ensure that you start the license server uninstaller in the required mode, start it from a Command Prompt window.
     * In Windows Control Panel, open the Programs and Features pane, select **License Server** from the publisher NVIDIA, and click **Uninstall/Change**. 
_Figure 1. Starting the Uninstaller from Windows Control Panel_
     * In Windows Explorer, open the license server installation directory and double-click the **Change License Server Installation** application. 
The default license server installation folder is %SystemDrive%:\\.
     * In a Command Prompt window, change to the license server installation directory and start the license server uninstaller in **interactive console** mode. 
The default license server installation folder is %SystemDrive%:\\.
Copy
Copied!

```
            
            
C:\>"Change License Server Installation.exe" -i console

        
```

     * In a Command Prompt window, change to the license server installation directory and start the license server uninstaller in **silent** mode. 
The default license server installation folder is %SystemDrive%:\\.
Copy
Copied!

```
            
            
C:\>"Change License Server Installation.exe" -i silent

        
```

If you start the license server uninstaller in silent mode, it runs without prompting you for any information. The remaining steps are **not** required.
  2. If the Configure License Server dialog box opens, ensure that the **Uninstall Product** option is selected and click **Next**. 
_Figure 2. Running the License Server Uninstaller on Windows_
  3. When prompted, confirm that you want to uninstall the license server and specify whether you want to uninstall Apache Tomcat.


###  [A.1.2. Uninstalling the NVIDIA vGPU Software License Server on Linux](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#uninstalling-nvidia-grid-license-server-linux)
If you want to uninstall the license server in silent mode, ensure that the license server installation directory contains the installer.properties file.
For information about the installer.properties file, refer to [Virtual GPU Software License Server User Guide](https://docs.nvidia.com/grid/ls/latest/grid-license-server-user-guide/index.html). 
  1. Change to the license server installation directory. The default license server installation directory is /opt/flexnetls/nvidia. 
Copy
Copied!

```
            
            
[nvidia@localhost ~]$ cd /opt/flexnetls/nvidia

        
```

  2. As root, start the license server uninstaller. 
     * If the license server software was installed in graphical mode or console mode, and you want to run the uninstaller in the same mode, run the Change License Server Installation command without any arguments.
Copy
Copied!

```
            
            
[nvidia@localhost ~]$ sudo ./Change\ License\ Server\ Installation

        
```

If the license server software was installed in graphical mode, the Configure License Server window opens.
     * If you want to run the uninstaller in silent mode, run the Change License Server Installation command with the -i silent argument.
Copy
Copied!

```
            
            
[nvidia@localhost ~]$ sudo ./Change\ License\ Server\ Installation -i silent

        
```

If you start the license server uninstaller in silent mode, it runs without prompting you for any information. The remaining steps are **not** required.
  3. If the Configure License Server window opens, ensure that the **Uninstall Product** option is selected and click **Next**. 
_Figure 3. Running the License Server Uninstaller on Linux_
  4. When prompted, confirm that you want to uninstall the license server.


## [B. Getting Started with the NLS RESTful APIs](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-started-with-nls-restful-apis)
NVIDIA License System (NLS) provides RESTful APIs for managing its Cloud License Service (CLS) and Delegated License Service (DLS) components. The CLS RESTful API is provided by the NVIDIA Licensing Portal. The DLS RESTful API is provided by the NVIDIA Licensing application on the DLS appliance.
Comprehensive reference information generated by Swagger for these APIs is available as follows: 
  * **CLS RESTful API:**[NVIDIA License System APIs](https://ui.licensing.nvidia.com/api-doc/nls-api-docs.html)
  * **DLS RESTful API:**[NVIDIA License System Virtual Appliance APIs](https://ui.licensing.nvidia.com/api-doc/dls-api-docs.html)


###  [B.1. Getting Started with the CLS RESTful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-started-with-cls-restful-apis)
The NVIDIA Licensing Portal provides a RESTful API for managing the Cloud License Service (CLS) component of NVIDIA License System.
Comprehensive reference information generated by Swagger for this API is available in [NVIDIA License System APIs](https://ui.licensing.nvidia.com/api-doc/nls-api-docs.html). 
###  [B.1.1. Creating a Licensing State API Key](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-licensing-state-api-key)
Before using the CLS RESTful API to manage a CLS instance, you must create a licensing state API key from the NVIDIA Licensing Portal. This key allows **only** read access to an organization's resources on the NVIDIA Licensing Portal. Therefore, you can use this key for authentication **only** in HTTP GET requests.
  1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
  2. From the top navigation bar, select **Operations > API keys**.
  3. On the **API keys** page, click **Create API key**.
  4. In **Key name** , enter a name for the API key.
  5. Under **Access type** , select **Licensing State**.
  6. To restrict the key to specific resources: 
    1. Select **Limit where this key can be used**.
    2. Under **Allowed scopes** , select one or more virtual groups or DLS appliances.
If this option is not selected, the key can be used across the organization.
  7. To set an expiration period, select **Expire this key after a specified number of days** and enter the required value in **Days until expiration**.
  8. Click **Create API key**
  9. In the API-keys table, copy the key from the **Key** column. You can also open the row menu and select the **Copy key**.


Pass the licensing state API key in the `x-api-key` header in all subsequent requests. 
###  [B.1.2. Getting Information Required in Other RESTful API Calls to a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-cls-instance)
The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a CLS instance.
Ensure that you have created the licensing state API key that you need for authentication as explained in [Creating a Licensing State API Key](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-licensing-state-api-key "Before using the CLS RESTful API to manage a CLS instance, you must create a licensing state API key from the NVIDIA Licensing Portal. This key allows only read access to an organization's resources on the NVIDIA Licensing Portal. Therefore, you can use this key for authentication only in HTTP GET requests.").
  1. Get the ID of the organization to which the user that created the API key belongs. 
    1. If you are not already logged in, log in to the [NVIDIA Enterprise Application Hub](http://nvid.nvidia.com/dashboard/) and click **NVIDIA LICENSING PORTAL** to go to the NVIDIA Licensing Portal.
    2. On the NVIDIA Licensing Portal dashboard, locate the ID in parentheses next to the organization name. 
  2. Submit an HTTP GET request to get information about all virtual groups that can access the license servers bound to the CLS instance. 
Copy
Copied!

```
            
            
GET https://api.licensing.nvidia.com/v1/org/_org-name_/virtual-groups

        
```


_org-name_ 
     The ID of the organization to which the user that created the API key belongs, which you obtained in the previous step. For example: `lic-0023x00002kuavttex`. 
Pass the required API key in the `x-api-key` header. For information about how to create this key, refer to [Creating a Licensing State API Key](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-licensing-state-api-key "Before using the CLS RESTful API to manage a CLS instance, you must create a licensing state API key from the NVIDIA Licensing Portal. This key allows only read access to an organization's resources on the NVIDIA Licensing Portal. Therefore, you can use this key for authentication only in HTTP GET requests.").
This example uses the curl command to get information for other RESTful API calls to the CLS instance for a user in the organization `lic-0023x00002kuavttex`. The user is authenticated by an API key. For clarity, the example shows only the placeholder _api-key_. 
Copy
Copied!

```
            
            
curl -k --location --request GET \
'https://api.licensing.nvidia.com/v1/org/lic-0023x00002kuavttex/virtual-groups' \
--header 'x-api-key: _api-key_'

        
```

The information required is returned in the following fields in the body of the response:  

`licenseServerId` 
     The ID of the license server that is bound to the CLS instance. For example: 
Copy
Copied!

```
            
            
"licenseServerId":"5244e9b3-8675-11ed-857a-03a0ffbeaa4c"

        
```


`virtualGroupId` 
     The ID of the virtual group to which the license server that is bound to the CLS instance belongs. For example: 
Copy
Copied!

```
            
            
"virtualGroupId":89

        
```



###  [B.1.3. Getting License Server Information for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-license-server-information-for-cls-instance)
Ensure that the following prerequisites are met: 
  * You have created the licensing state API key that you need for authentication as explained in [Creating a Licensing State API Key](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-licensing-state-api-key "Before using the CLS RESTful API to manage a CLS instance, you must create a licensing state API key from the NVIDIA Licensing Portal. This key allows only read access to an organization's resources on the NVIDIA Licensing Portal. Therefore, you can use this key for authentication only in HTTP GET requests.").
  * You have obtained the information that you need to complete this task as explained in [Getting Information Required in Other RESTful API Calls to a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-cls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a CLS instance.").


Submit an HTTP GET request to get information about the license server that you are interested in, which is bound to the CLS instance. 
Copy
Copied!

```
            
            
GET https://api.licensing.nvidia.com/v1/org/_org-name_/virtual-groups/_virtual-group-id_/license-servers/_license-server-id_

        
```


_org-name_ 
     The code for the organization to which the user that created the API key belongs. For example: `lic-0023x00002kuavttex`.  

_virtual-group-id_ 
     The identifier of the virtual group to which the license server that is bound to the CLS instance belongs. For example: .  

_license-server-id_ 
     The identifier of the license server that you are interested in, which is bound to the CLS instance. For example: `5244e9b3-8675-11ed-857a-03a0ffbeaa4c`. 
You can obtain this information as explained in [Getting Information Required in Other RESTful API Calls to a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-cls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a CLS instance."). 
Pass the required API key in the `x-api-key` header. For information about how to create this key, refer to [Creating a Licensing State API Key](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-licensing-state-api-key "Before using the CLS RESTful API to manage a CLS instance, you must create a licensing state API key from the NVIDIA Licensing Portal. This key allows only read access to an organization's resources on the NVIDIA Licensing Portal. Therefore, you can use this key for authentication only in HTTP GET requests."). This example uses the curl command to get information for license server `5244e9b3-8675-11ed-857a-03a0ffbeaa4c`. The license server is bound to the CLS instance for a user in virtual group 89 of the organization `lic-0023x00002kuavttex`. The user is authenticated by an API key. For clarity, the example shows only the placeholder _api-key_
Copy
Copied!

```
            
            
curl -k --location --request GET \
'https://api.licensing.nvidia.com/v1/org/lic-0023x00002kuavttex/virtual-groups/89/license-servers/5244e9b3-8675-11ed-857a-03a0ffbeaa4c' \
--header 'x-api-key: _api-key_'

        
```

###  [B.1.4. Listing Licenses that Are Being Served by a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#listing-licenses-served-by-cls-instance)
Ensure that the following prerequisites are met: 
  * You have created the licensing state API key that you need for authentication as explained in [Creating a Licensing State API Key](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-licensing-state-api-key "Before using the CLS RESTful API to manage a CLS instance, you must create a licensing state API key from the NVIDIA Licensing Portal. This key allows only read access to an organization's resources on the NVIDIA Licensing Portal. Therefore, you can use this key for authentication only in HTTP GET requests.").
  * You have obtained the information that you need to complete this task as explained in [Getting Information Required in Other RESTful API Calls to a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-cls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a CLS instance.").


  1. Get the ID of the CLS instance that you are interested in. 
    1. Submit an HTTP GET request to get information about all license servers that you can access from your virtual group. 
Copy
Copied!

```
            
            
GET https://api.licensing.nvidia.com/v1/org/_org-name_/virtual-groups/_virtual-group-id_/license-servers

        
```


_org-name_ 
     The code for the organization to which the user that created the API key belongs. For example: `lic-0023x00002kuavttex`.  

_virtual-group-id_ 
     The identifier of the virtual group to which the license server that is bound to the CLS instance belongs. For example: . 
You can obtain this information as explained in [Getting Information Required in Other RESTful API Calls to a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-cls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a CLS instance.").
Pass the API key required for authentication in the `x-api-key` header. For information about how to create this key, refer to [Creating a Licensing State API Key](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#creating-licensing-state-api-key "Before using the CLS RESTful API to manage a CLS instance, you must create a licensing state API key from the NVIDIA Licensing Portal. This key allows only read access to an organization's resources on the NVIDIA Licensing Portal. Therefore, you can use this key for authentication only in HTTP GET requests.").
This example uses the curl command to get information for all license servers that can be accessed by a user in virtual group 89 of the organization `lic-0023x00002kuavttex`. The user is authenticated by an API key. For clarity, the example shows only the placeholder _api-key_
Copy
Copied!

```
            
            
curl -k --location --request GET \
'https://api.licensing.nvidia.com/v1/org/lic-0023x00002kuavttex/virtual-groups/89/license-servers' \
--header 'x-api-key: _api-key_'

        
```

    2. Examine each pair of `serviceInstanceId` and `serviceInstanceName` fields in the body of the response to find the ID of the CLS instance that you are interested in. For example: 
Copy
Copied!

```
            
            
"serviceInstanceId": "1d85d904-84db-405f-b1dc-8ef9ded4f4cd",
"serviceInstanceName": "CWBI-DLS-2-1-0"

        
```

  2. Submit an HTTP GET request to list the licenses that are being served by the CLS instance that you're interested in. 
Copy
Copied!

```
            
            
GET https://api.licensing.nvidia.com/v1/org/_org-name_/virtual-groups/_virtual-group-id_/leases

        
```


_org-name_ 
     The _org-name_ that you specified in the previous step.  

_virtual-group-id_ 
     The _virtual-group-id_ that you specified in the previous step. 
In the request, pass the headers listed in the following table.   
| Header  | Value  |  
| --- | --- |  
| `x-nv-service-instance-id`  | The ID of the service instance that you are interested in, which you obtained in the previous step.  |  
| `x-api-key`  | The API key required for authentication that you passed in the previous step.  |  
This example uses the curl command to list the licenses that are being served by the CLS instance `1d85d904-84db-405f-b1dc-8ef9ded4f4cd`. The user is authenticated by an API key. For clarity, the example shows only the placeholder _api-key_. 
Copy
Copied!

```
            
            
curl -k --location --request GET \
'https://api.licensing.nvidia.com/v1/org/lic-0023x00002kuavttex/virtual-groups/89/license-servers/5244e9b3-8675-11ed-857a-03a0ffbeaa4c' \
--header 'x-nv-service-instance-id: 1d85d904-84db-405f-b1dc-8ef9ded4f4cd' \
--header 'x-api-key: _api-key_'

        
```



###  [B.2. Getting Started with the DLS RESTful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-started-with-dls-restful-apis)
The NVIDIA Licensing application on the Delegated License Service (DLS) appliance provides a RESTful API for managing the DLS component of NVIDIA License System.
Comprehensive reference information generated by Swagger for this API is available in [NVIDIA License System Virtual Appliance APIs](https://ui.licensing.nvidia.com/api-doc/dls-api-docs.html). 
###  [B.2.1. Exporting and Importing API Keys for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#exporting-importing-api-keys-for-dls-instance)
API keys provide an alternative method for authentication and for getting the information required in other RESTful API calls. Any API keys that are created after a DLS instance has been registered with the NVIDIA Licensing Portal are not recognized by the DLS instance. To ensure that these API keys are recognized, you must export them from the NVIDIA Licensing Portal and import them into the DLS instance.
Note:
Any existing API keys are exported and imported automatically for a DLS instance when the following tasks are performed on the instance: 
  * The DLS instance is registered with the NVIDIA Licensing Portal.
  * A license server is installed on the DLS instance.


You can export and import only API keys whose access type contains **Licensing State** , **Enterprise** or both **Licensing State** and **Enterprise**. You cannot export and import API keys whose access type contains only **Software Downloads**. However, you can export and import API keys whose access type contains **Software Downloads** and at least one other access type. 
###  [B.2.1.1. Exporting API Keys for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#exporting-api-keys-for-dls-instance)
Ensure that the API keys that you want to export have been created.
  1. If you are not already logged in, log in to the[ _NVIDIA Enterprise Application Hub_](http://nvid.nvidia.com/dashboard/) and click NVIDIA LICENSING PORTAL to go to the NVIDIA Licensing Portal.
  2. Select **Operations > API keys**.
  3. Click **Export DLS keys**.
  4. In the **Export DLS keys** dialog, select the appliance into which the key file will be imported from **DLS appliance** drop-down. 
API keys that have only **Software Downloads** access are not included in the export.
A file named `dls_key_export_<UUID>.tok` is saved to the downloads folder.


###  [B.2.1.2. Importing API Keys for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#importing-api-keys-for-dls-instance)
Ensure that the API keys that you want to import have been exported as explained in [Exporting API Keys for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#exporting-api-keys-for-dls-instance). The DLS instance that was selected when the API keys were exported **must** be the DLS instance into which you want to import the API keys.
  1. Log in to that DLS appliance.
  2. Open Operations > Settings and select API Keys.
  3. Select Import API Key.
  4. In the file browser, choose the exported `dls_key_export_*.tok` file for this appliance. Selecting the file starts the import.


###  [B.2.2. Logging in to a DLS Appliance from the RESTful API ](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-appliance-from-restful-api)
Before using the DLS RESTful API to manage a DLS instance, you must log in as the DLS administrator user to the DLS appliance that hosts the DLS instance. Logging in also returns authentication information that you must use in all subsequent calls to the DLS RESTful API.
Submit an HTTP POST request with basic authentication to log in as the DLS administrator user to the DLS appliance. 
Copy
Copied!

```
            
            
POST https://_host_:_port_/auth/v1/login

        
```


_host_ 
    The IP address or fully qualified domain name of the VM or container that is hosting the DLS appliance. 

_port_ 
    The port on which the VM or container that is hosting the DLS appliance listens for HTTPS requests.
In the body of the request, pass the user name and password of the DLS administrator user. The default user name is `dls_admin`. The following example uses the curl command to log in to the DLS appliance `dls.example.com` on port 8081 as the user `dls_admin` with the password `changeit`. 
Copy
Copied!

```
            
            
curl -k --location --request POST 'https://dls.example.com:8081/auth/v1/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "dls_admin",
    "password": "changeit"}'

        
```

The response sets the authorization token in the LIC-TOKEN cookie and includes the token in the response body. This example shows the format of the LIC-TOKEN cookie. Line breaks have been added for clarity. 
Copy
Copied!

```
            
            
{ 
    "cookie": { 
    "expires": null, 
    "name": "LIC-TOKEN", 
    "value": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC0wMDAwLTAwMDAwMDAwMDAwMCIsInR5cCI6IkpXVCJ9.
eyJpYXQiOjE2NTQwMjk5NDcsImV4cCI6MTY1NDAzMzU0Nywicm9sZSI6IjAwMDAwMDAwLTAwMDAtMDAwMC0wMDAwLTAwMDAwMDAwMDAwMCIsIm
tpZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC0wMDAwLTAwMDAwMDAwMDAwMCIsInVzZXIiOiJkbHNfYWRtaW4ifQ.qKaRj2sVYWhnfv5udFkN5o1YZ
p_DfBRCUl4vIdSMb_sSGZSKUnuTPBTwYG8PiQ2SYN7bbD8RfoSZOL8FuexDZsx4EloF67UjaHCBT2KvwC75p_pgBKvb93elS7rtQt1-Bm72kyr
VGdG8K6ylJSqVp4T0SBGEqxQ8Yq07vzXIlkWURh3FelSSE9oQolEC2h8xvpFR0Z2JUnUArrGI2aQKFj-K-vXteWsHI0GWBhnyTi2iRJhao4bt
Fg6xJpbS60NF4dt0BtzaB1RyIA8zFAVU2pFBclpn6h27QV2ZMAn_fvf7ryxIT3kGc4Jppj9aeoKnJfq9Z874kTiMWYE9mI33wA" 
},

        
```

If the tool that you use to submit HTTP requests supports cookies, you can submit subsequent requests with the LIC-TOKEN cookie. Otherwise, you must pass the required authorization token in the `Authorization: Bearer` header in all subsequent requests. 
###  [B.2.3. Getting Information Required in Other RESTful API Calls to a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-dls-instance)
The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a DLS instance.
Ensure that you have logged in to the DLS instance and obtained the cookie or authorization token that you need for authentication as explained in [Logging in to a DLS Appliance from the RESTful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-appliance-from-restful-api "Before using the DLS RESTful API to manage a DLS instance, you must log in as the DLS administrator user to the DLS appliance that hosts the DLS instance. Logging in also returns authentication information that you must use in all subsequent calls to the DLS RESTful API.").
Submit an HTTP GET request to obtain information about the DLS administrator user on the DLS appliance. 
Copy
Copied!

```
            
            
GET https://_host_:_port_/auth/v1/user/me

        
```


_host_ 
    The IP address or fully qualified domain name of the VM or container that is hosting the DLS appliance. 

_port_ 
    The port on which the VM or container that is hosting the DLS appliance listens for HTTPS requests.
If the tool that you use to submit the request supports cookies, you can submit the request with the LIC-TOKEN cookie. Otherwise, you must pass the required authorization token in the `Authorization: Bearer` header. The LIC-TOKEN cookie and the required authorization token are returned when you log in to a DLS appliance as explained in [Logging in to a DLS Appliance from the RESTful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-appliance-from-restful-api "Before using the DLS RESTful API to manage a DLS instance, you must log in as the DLS administrator user to the DLS appliance that hosts the DLS instance. Logging in also returns authentication information that you must use in all subsequent calls to the DLS RESTful API."). This example uses the curl command to get information for other RESTful API calls to the DLS appliance `dls.example.com` on port 8081. The user is authenticated by the LIC-TOKEN cookie. For clarity, the example shows only the placeholder _cookie_. 
Copy
Copied!

```
            
            
curl -k --location --request GET 'https://dls.example.com:8081/auth/v1/user/me' \
--cookie "LIC-TOKEN=_cookie_"

        
```

The information required is returned in the following fields in the body of the response:  

`orgName` 
     The ID of the organization to which the DLS administrator user that logged into the DLS appliance belongs. For example: 
Copy
Copied!

```
            
            
"orgName":"lic-0011w00001roiwqqay"

        
```


`licenseServerId` 
     The ID of the license server that is bound to the DLS instance. For example: 
Copy
Copied!

```
            
            
"licenseServerId":"4133f8a4-7564-11ed-968a-02a0eeafbb5d"

        
```


`virtualGroupId` 
     The ID of the virtual group to which the license server that is bound to the DLS instance belongs. For example: 
Copy
Copied!

```
            
            
"virtualGroupId":3

        
```

###  [B.2.4. Getting License Server Information for a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-license-server-information-for-dls-instance)
Ensure that the following prerequisites are met: 
  * You have logged in to the DLS instance and obtained the cookie or authorization token that you need for authentication as explained in [Logging in to a DLS Appliance from the RESTful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-appliance-from-restful-api "Before using the DLS RESTful API to manage a DLS instance, you must log in as the DLS administrator user to the DLS appliance that hosts the DLS instance. Logging in also returns authentication information that you must use in all subsequent calls to the DLS RESTful API.").
  * You have obtained the information that you need to complete this task as explained in [Getting Information Required in Other RESTful API Calls to a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-dls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a DLS instance.").


Send an HTTP GET request to get information about the license server that is bound to the DLS instance. 
Copy
Copied!

```
            
            
GET https://_host_:_port_/admin/v1/org/_org-name_/virtual-groups/_virtual-group-id_/license-servers

        
```


_host_ 
    The IP address or fully qualified domain name of the VM or container that is hosting the DLS appliance. 

_port_ 
    The port on which the VM or container that is hosting the DLS appliance listens for HTTPS requests. 

_org-name_ 
     The ID for the organization to which the user that logged in to the DLS instance belongs. For example: `lic-0011w00001roiwqqay`. 
You can obtain this ID as explained in [Getting Information Required in Other RESTful API Calls to a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-dls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a DLS instance."). 

_virtual-group-id_ 
     The ID of the virtual group to which the license server that is bound to the DLS instance belongs. For example: . 
You can obtain this ID as explained in [Getting Information Required in Other RESTful API Calls to a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-dls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a DLS instance.").
If the tool that you use to submit the request supports cookies, you can submit the request with the LIC-TOKEN cookie. Otherwise, you must pass the required authorization token in the `Authorization: Bearer` header. The LIC-TOKEN cookie and the required authorization token are returned when you log in to a DLS appliance as explained in [Logging in to a DLS Appliance from the RESTful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-appliance-from-restful-api "Before using the DLS RESTful API to manage a DLS instance, you must log in as the DLS administrator user to the DLS appliance that hosts the DLS instance. Logging in also returns authentication information that you must use in all subsequent calls to the DLS RESTful API."). This example uses the curl command to get license server information for the DLS instance on the DLS appliance `dls.example.com` on port 8081. The user belongs to virtual group 3 in the organization lic-0011w00001roiwqqay and is authenticated by the LIC-TOKEN cookie. For clarity, the example shows only the placeholder _cookie_. 
Copy
Copied!

```
            
            
curl -k --location --request GET \
'https://dls.example.com:8081/admin/v1/org/lic-0011w00001roiwqqay/virtual-groups/3/license-servers' \
--cookie "LIC-TOKEN=_cookie_"

        
```

###  [B.2.5. Listing Licenses that Are Being Served by a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#listing-licenses-served-by-dls-instance)
Ensure that the following prerequisites are met: 
  * You have logged in to the DLS instance and obtained the cookie or authorization token that you need for authentication as explained in [Logging in to a DLS Appliance from the RESTful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-appliance-from-restful-api "Before using the DLS RESTful API to manage a DLS instance, you must log in as the DLS administrator user to the DLS appliance that hosts the DLS instance. Logging in also returns authentication information that you must use in all subsequent calls to the DLS RESTful API.").
  * You have obtained the information that you need to complete this task as explained in [Getting Information Required in Other RESTful API Calls to a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-dls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a DLS instance.").


Send an HTTP GET request to list the licenses that are being served by the DLS instance that you're interested in. 
Copy
Copied!

```
            
            
GET https://_host_:_port_/admin/v1/org/_org-name_/virtual-groups/_virtual-group-id_/leases

        
```


_host_ 
    The IP address or fully qualified domain name of the VM or container that is hosting the DLS appliance. 

_port_ 
    The port on which the VM or container that is hosting the DLS appliance listens for HTTPS requests. 

_org-name_ 
     The code for the organization to which the user that logged in to the DLS instance belongs. For example: `lic-0011w00001roiwqqay`. 
You can obtain this code as explained in [Getting Information Required in Other RESTful API Calls to a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-dls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a DLS instance."). 

_virtual-group-id_ 
     The identifier of the virtual group to which the license server that is bound to the DLS instance belongs. For example: . 
You can obtain this identifier as explained in [Getting Information Required in Other RESTful API Calls to a DLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#getting-information-for-restful-api-calls-to-dls-instance "The organization ID, virtual group ID, and license server ID are required in other RESTful API calls to a DLS instance.").
If the tool that you use to submit the request supports cookies, you can submit the request with the LIC-TOKEN cookie. Otherwise, you must pass the required authorization token in the `Authorization: Bearer` header. The LIC-TOKEN cookie and the required authorization token are returned when you log in to a DLS appliance as explained in [Logging in to a DLS Appliance from the RESTful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#logging-in-to-dls-appliance-from-restful-api "Before using the DLS RESTful API to manage a DLS instance, you must log in as the DLS administrator user to the DLS appliance that hosts the DLS instance. Logging in also returns authentication information that you must use in all subsequent calls to the DLS RESTful API."). This example uses the curl command to list the licenses that are being served by the DLS instance on the DLS appliance `dls.example.com` on port 8081. The user belongs to virtual group 3 in the organization lic-0011w00001roiwqqay and is authenticated by the LIC-TOKEN cookie. For clarity, the example shows only the placeholder _cookie_. 
Copy
Copied!

```
            
            
curl -k --location --request GET \
'https://dls.example.com:8081/admin/v1/org/lic-0011w00001roiwqqay/virtual-groups/3/leases' \
--cookie "LIC-TOKEN=_cookie_"

        
```

###  [B.2.6. **Rotating Passwords Programmatically (API)**](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#rotating-passwords-programmatically)
The Password Rotation API enables automated password management for local users using Enterprise-scoped API Keys. This is useful for: 
  * Compliance requirements mandating regular password rotation
  * Automated password management workflows
  * Integration with secret management systems (e.g., HashiCorp Vault, AWS Secrets Manager)
  * Service account password rotation


**Prerequisites** : 
  * Enterprise-scoped API Key from NVIDIA Licensing Portal
  * API Key imported into DLS instance: New password meeting policy requirements
  * HTTPS connection to DLS instance


**Request Headers** : 
`x-api-key: <your-enterprise-api-key>`
Content-Type: application/json 
**Request Body Parameters** :   
| Parameter  | Type  | Required  | Description  |  
| --- | --- | --- | --- |  
| _username_  | string  | Yes  | Target username for password rotation  |  
| _current_password_  | string  | Yes  | Current password for verification  |  
| _new_password_  | string  | No  | Optional: Specify new password (if omitted, system generates one)  |  
**Option 1: Auto-Generated Password** (Recommended) 
The system automatically generates a strong password that meets all policy requirements: 
Copy
Copied!

```
            
            
curl -X POST 'https://192.0.2.10/service_instance_manager/v1/pw-rotate' \
  --header 'x-api-key: 0ae280b9-58e2-809c-3c24-4069c5bd40c2' \
  --header 'Content-Type: application/json' \
  --data-raw '{
	"username": "license_operator1",
	"current_password": "CurrentP@ssw0rd2024"
  }'

        
```

**Success Response** (HTTP 200): 
Copy
Copied!

```
            
            
{
  "new_password": "X7k#mP9$vL2@qR5&nW8",
  "local_reset_secret": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "message": "Password rotated successfully."
}

        
```

**Option 2: User-Provided Password**
Specify a custom password that must meet all policy requirements: 
Copy
Copied!

```
            
            
curl -X POST 'https://192.0.2.10/service_instance_manager/v1/pw-rotate' \
  --header 'x-api-key: 0ae280b9-58e2-809c-3c24-4069c5bd40c2' \
  --header 'Content-Type: application/json' \
  --data-raw '{
	"username": "license_operator1",
	"current_password": "CurrentP@ssw0rd2024",
	"new_password": "MyNewStr0ng!Password123"
  }'

        
```

**Important** : Store the returned _new_password_ and _local_reset_secret_ securely. The password cannot be retrieved later. 
**Error Responses** : 
**400 Bad Request** - Invalid password: 
Copy
Copied!

```
            
            
{
  "error": "Bad Request",
  "message": "Invalid new password. Password must contain at least one uppercase letter, one lowercase letter, one digit, and one special character."
}

        
```

**401 Unauthorized** - Invalid API Key: 
Copy
Copied!

```
            
            
{
  "error": "Unauthorized",
  "message": "Invalid API Key"
}

        
```

**403 Forbidden** - Insufficient permissions: 
Copy
Copied!

```
            
            
{
  "error": "Forbidden",
  "message": "API Key does not have Enterprise scope"
}

        
```

###  [B.2.7. Configuring LDAP via Restful API](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#configuring-ldap-via-restful-api)
To view the current LDAP configuration: **Via RESTful API**
**API Endpoint** : 
Copy
Copied!

```
            
            
GET https://<dls-appliance-ip>/auth/v1/ldap

        
```

**Request Headers** : 
Copy
Copied!

```
            
            
x-api-key: <your-enterprise-api-key>

        
```

**Example Request** : 
Copy
Copied!

```
            
            
curl -X GET 'https://192.0.2.10/auth/v1/ldap' \
  --header 'x-api-key: 0ae280b9-58e2-809c-3c24-4069c5bd40c2'

        
```

**Success Response** (HTTP 200): 
Copy
Copied!

```
            
            
{
  "enable_ldap": true,
  "ldap_server": "192.0.2.50",
  "ldap_port": 389,
  "use_secure": false,
  "authentication_type": "NTLM",
  "referrals": false,
  "domain": "example.com",
  "root_dn": "dc=example,dc=com",
  "search_base": "CN=Users,DC=example,DC=com",
  "search_filter": "(&(objectCategory=Person)(userPrincipalName=*))",
  "username_attribute": "sAMAccountName",
  "search_scope": "SUBTREE",
  "ldap_group_role_mapping": {
	"CN=LicenseAdmins,DC=example,DC=com": "DLS_ADMIN",
	"CN=LicenseOperators,DC=example,DC=com": "DLS_OPERATOR",
	"CN=LicenseUsers,DC=example,DC=com": "DLS_USER"
  }
}

        
```

Note:
LDAP configuration can be modified directly from the Web UI by updating fields and clicking **Save**. 
**API Endpoint** : 
Copy
Copied!

```
            
            
POST https://<dls-appliance-ip>/auth/v1/ldap

        
```

**Request Headers** : 
Copy
Copied!

```
            
            
x-api-key: <your-enterprise-api-key>
Content-Type: application/json

        
```

**Request Body Parameters**
Expand  
| Parameter  | Type  | Required  | Description  |  
| --- | --- | --- | --- |  
| _enable_ldap_  | string  | Yes  | Enable LDAP authentication: “true” or “false”  |  
| _ldap_server_  | string  | Yes  | LDAP server hostname or IP address  |  
| _ldap_port_  | integer  | Yes  | LDAP server port (389 for LDAP, 636 for LDAPS)  |  
| _use_secure_  | string  | No  | Use LDAPS/TLS: “true” or “false” (default: “false”)  |  
| _authentication_type_  | string  | Yes  | Authentication mechanism: “NTLM” or “SASL”  |  
| _domain_  | string  | Conditional  | Domain name for NTLM authentication (required for Windows AD)  |  
| _root_dn_  | string  | No  | Root Distinguished Name (e.g., “dc=example,dc=com”)  |  
| _search_base_  | string  | Yes  | Base DN for user searches (e.g., “CN=Users,DC=example,DC=com”)  |  
| _search_filter_  | string  | Yes  | LDAP search filter (e.g., “(uid={0})” or “(&(objectCategory=Person)(userPrincipalName=*))“)  |  
| _search_scope_  | string  | Yes  | Search depth: “SUBTREE” (recursive) or “BASE” (single level)  |  
| _username_attribute_  | string  | Yes  | Attribute containing username (e.g., “uid”, “sAMAccountName”)  |  
| _referrals_  | string  | No  | Follow LDAP referrals: “true” or “false” (default: “false”)  |  
| _ldap_tls_ca_certs_  | string  | Conditional  | PEM-encoded root CA certificate (required when use_secure is “true”)  |  
| _ldap_group_role_mapping_  | object  | No  | Map LDAP group DNs to DLS roles (see below)  |  
| _test_ldap_  | boolean  | No  | Test connection before saving: true or false (recommended: true)  |  
| _test_ldap_creds_  | object  | No  | Test credentials for validation (username/password)  |  
**Example: Basic LDAP Configuration (NTLM with Active Directory)**
Copy
Copied!

```
            
            
curl -X POST 'https://192.0.2.10/auth/v1/ldap' \
  --header 'x-api-key: 0ae280b9-58e2-809c-3c24-4069c5bd40c2' \
  --header 'Content-Type: application/json' \
  --data-raw '{
	"enable_ldap": "true",
	"test_ldap": true,
	"ldap_server": "192.0.2.50",
	"ldap_port": 389,
	"use_secure": "false",
	"authentication_type": "NTLM",
	"domain": "example.com",
	"root_dn": "dc=example,dc=com",
	"search_base": "CN=Users,DC=example,DC=com",
	"search_filter": "(&(objectCategory=Person)(userPrincipalName=*))",
	"username_attribute": "sAMAccountName",
	"search_scope": "SUBTREE",
	"referrals": "false",
	"test_ldap_creds": {
  	"username": "testuser",
  	"password": "TestP@ssw0rd123"
	}
  }'

        
```

**Success Response** (HTTP 200): 
Copy
Copied!

```
            
            
{
  "message": "LDAP configuration updated successfully",
  "enable_ldap": true
}

        
```

**Example: Secure LDAP Configuration (LDAPS with TLS)**
Copy
Copied!

```
            
            
curl -X POST 'https://192.0.2.10/auth/v1/ldap' \
  --header 'x-api-key: 0ae280b9-58e2-809c-3c24-4069c5bd40c2' \
  --header 'Content-Type: application/json' \
  --data-raw '{
	"enable_ldap": "true",
	"ldap_server": "ldap.example.com",
	"ldap_port": 636,
	"use_secure": "true",
	"authentication_type": "SIMPLE",
	"root_dn": "dc=example,dc=com",
	"search_base": "ou=users,dc=example,dc=com",
	"search_filter": "(objectClass=inetOrgPerson)",
	"username_attribute": "uid",
	"search_scope": "SUBTREE",
	"ldap_tls_ca_certs": "-----BEGIN CERTIFICATE-----\nMIIE...certificate content...==\n-----END CERTIFICATE-----"
  }'

        
```

## [C. Performance Data for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#performance-reliability-data-cls-instance)
Use the measured performance numbers to determine whether the CLS meets your requirements based on the number and frequency of requests from licensed clients. Hosting a CLS instance on a cloud service provides robustness and dynamic scalability for the CLS instance. The cloud service dynamically scales CLS instances to maintain the measured performance numbers. 
Note:
For the corresponding data for a DLS virtual appliance, refer to [Sizing Guidelines for a DLS Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#dls-virtual-appliance-sizing-guidelines "Use the measured performance numbers to determine the optimum configuration for your container-based or VM-based DLS appliances based on the expected number and frequency of requests from licensed clients."). 
###  [C.1. Throughput for a CLS Virtual Appliance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#cls-instance-throughput)
Throughput measures the number of clients that a CLS instance can process in 1 second.
A CLS instance can process up to 40 clients in 1 second. 
The average response time is 436 ms. 
###  [C.2. Scalability for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#cls-instance-scalability)
Scalability measures the maximum number of licensed clients that a CLS instance can serve in a specific interval. A CLS instance serves a licensed client by performing a licensing operation for the client, namely the borrowing, return, or renewal of a license. Registration of a licensed client is not considered a licensing operation because it occurs only once for any client.
These measurements capture the maximum number of licensed clients that a CLS instance can serve when 40 clients connect concurrently to the CLS instance. The maximum number of licensed clients is measured for different lengths of time up to 24 hours for which a license is borrowed. 
Note:
  * Intervals in the table are the renewal intervals when a client contacts the DLS appliance to request a licensing operation.
  * The renewal interval is set to 15% of the length of time for which a license is borrowed.

  
| Length of Time License Is Borrowed  | Interval  | Maximum Number of Licensed Clients  |  
| --- | --- | --- |  
| 10 minutes  | 1.5 minutes  | 3,500  |  
| 1 hour  | 9 minutes  | 21,000  |  
| 12 hours  | 1.8 hours  | 259,000  |  
| 24 hours  | 3.6 hours  | 518,000  |  
###  [C.3. Burst Load Performance for a CLS Instance](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#cls-instance-burst-load-performance)
Burst load performance measures the time that a CLS instance requires to process the requests received from a large number of clients in a short interval of time. Burst load performance does **not** measure concurrency or throughput.
Note:
Burst processing times are illustrative only because they are for retry logic in performance tests that use simulated client drivers. Times may differ with real client drivers.   
| Number of Clients  | Interval  | Processing Time  |  
| --- | --- | --- |  
| 100  | 0-1 second  | 10 seconds  |  
| 1,000  | 0-2 seconds  | 1 minute  |  
| 5,000  | 100 seconds (1 minute and 40 seconds )  | 5 minutes  |  
| 10,000  | 200 seconds (3 minutes and 20 seconds)  | 10 minutes  |  
The NVIDIA Licensing Portal limits the maximum number of concurrent requests to 100. 
###  [C.4. NVIDIA Cloud Licensing Service (CLS) Service Level Objectives](https://docs.nvidia.com/license-system/latest/nvidia-license-system-user-guide/index.html#cls-service-level-objectives)
The NVIDIA Cloud Licensing Service (CLS) is a managed service supporting automated license borrow, renewal, and return operations for NVIDIA licensed software and hardware products. It is a globally-distributed platform built to be scalable to meet customer demand. 
NVIDIA strives to maintain the following Service Level Objectives for the CLS public endpoints that are used for typical license operations.   
|  **Monthly Service Availability** We define availability as the percentage of requests successfully processed, as a proportion of all valid requests received, during a given time period. A response is considered successful if it **completes in under 3 seconds** (as measured at our service edge) and **does not return an unexpected error** (e.g. 5XX code).  |  **> 99.95%**  |  
| --- | --- |  
To calculate Monthly Service Availability, NVIDIA uses a combination of methods, including information from NVIDIA’s event monitoring systems and logs and observing external causes that may impact the services (to the extent known to NVIDIA). Availability metrics are rolled-up to daily and monthly statistics that are reported on our public status page [_https://status.licensing.nvidia.com_](https://status.licensing.nvidia.com). 
If your use of the CLS does not achieve the objectives of this SLO, you may notify NVIDIA. NVIDIA may make further improvements to the CLS in its sole discretion. You will not be entitled to receive any further support or remedies from NVIDIA based on the SLO. 
###  Notice 
This document is provided for information purposes only and shall not be regarded as a warranty of a certain functionality, condition, or quality of a product. NVIDIA Corporation (“NVIDIA”) makes no representations or warranties, expressed or implied, as to the accuracy or completeness of the information contained in this document and assumes no responsibility for any errors contained herein. NVIDIA shall have no liability for the consequences or use of such information or for any infringement of patents or other rights of third parties that may result from its use. This document is not a commitment to develop, release, or deliver any Material (defined below), code, or functionality. 
NVIDIA reserves the right to make corrections, modifications, enhancements, improvements, and any other changes to this document, at any time without notice. 
Customer should obtain the latest relevant information before placing orders and should verify that such information is current and complete. 
NVIDIA products are sold subject to the NVIDIA standard terms and conditions of sale supplied at the time of order acknowledgement, unless otherwise agreed in an individual sales agreement signed by authorized representatives of NVIDIA and customer (“Terms of Sale”). NVIDIA hereby expressly objects to applying any customer general terms and conditions with regards to the purchase of the NVIDIA product referenced in this document. No contractual obligations are formed either directly or indirectly by this document. 
NVIDIA products are not designed, authorized, or warranted to be suitable for use in medical, military, aircraft, space, or life support equipment, nor in applications where failure or malfunction of the NVIDIA product can reasonably be expected to result in personal injury, death, or property or environmental damage. NVIDIA accepts no liability for inclusion and/or use of NVIDIA products in such equipment or applications and therefore such inclusion and/or use is at customer’s own risk. 
NVIDIA makes no representation or warranty that products based on this document will be suitable for any specified use. Testing of all parameters of each product is not necessarily performed by NVIDIA. It is customer’s sole responsibility to evaluate and determine the applicability of any information contained in this document, ensure the product is suitable and fit for the application planned by customer, and perform the necessary testing for the application in order to avoid a default of the application or the product. Weaknesses in customer’s product designs may affect the quality and reliability of the NVIDIA product and may result in additional or different conditions and/or requirements beyond those contained in this document. NVIDIA accepts no liability related to any default, damage, costs, or problem which may be based on or attributable to: (i) the use of the NVIDIA product in any manner that is contrary to this document or (ii) customer product designs. 
No license, either expressed or implied, is granted under any NVIDIA patent right, copyright, or other NVIDIA intellectual property right under this document. Information published by NVIDIA regarding third-party products or services does not constitute a license from NVIDIA to use such products or services or a warranty or endorsement thereof. Use of such information may require a license from a third party under the patents or other intellectual property rights of the third party, or a license from NVIDIA under the patents or other intellectual property rights of NVIDIA. 
Reproduction of information in this document is permissible only if approved in advance by NVIDIA in writing, reproduced without alteration and in full compliance with all applicable export laws and regulations, and accompanied by all associated conditions, limitations, and notices. 
THIS DOCUMENT AND ALL NVIDIA DESIGN SPECIFICATIONS, REFERENCE BOARDS, FILES, DRAWINGS, DIAGNOSTICS, LISTS, AND OTHER DOCUMENTS (TOGETHER AND SEPARATELY, “MATERIALS”) ARE BEING PROVIDED “AS IS.” NVIDIA MAKES NO WARRANTIES, EXPRESSED, IMPLIED, STATUTORY, OR OTHERWISE WITH RESPECT TO THE MATERIALS, AND EXPRESSLY DISCLAIMS ALL IMPLIED WARRANTIES OF NONINFRINGEMENT, MERCHANTABILITY, AND FITNESS FOR A PARTICULAR PURPOSE. TO THE EXTENT NOT PROHIBITED BY LAW, IN NO EVENT WILL NVIDIA BE LIABLE FOR ANY DAMAGES, INCLUDING WITHOUT LIMITATION ANY DIRECT, INDIRECT, SPECIAL, INCIDENTAL, PUNITIVE, OR CONSEQUENTIAL DAMAGES, HOWEVER CAUSED AND REGARDLESS OF THE THEORY OF LIABILITY, ARISING OUT OF ANY USE OF THIS DOCUMENT, EVEN IF NVIDIA HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES. Notwithstanding any damages that customer might incur for any reason whatsoever, NVIDIA’s aggregate and cumulative liability towards customer for the products described herein shall be limited in accordance with the Terms of Sale for the product. 
###  VESA DisplayPort 
DisplayPort and DisplayPort Compliance Logo, DisplayPort Compliance Logo for Dual-mode Sources, and DisplayPort Compliance Logo for Active Cables are trademarks owned by the Video Electronics Standards Association in the United States and other countries. 
###  HDMI 
HDMI, the HDMI logo, and High-Definition Multimedia Interface are trademarks or registered trademarks of HDMI Licensing LLC. 
###  OpenCL 
OpenCL is a trademark of Apple Inc. used under license to the Khronos Group Inc. 
###  Trademarks 
NVIDIA, the NVIDIA logo, NVIDIA Maxwell, NVIDIA Pascal, NVIDIA Turing, NVIDIA Volta, Quadro, and Tesla are trademarks or registered trademarks of NVIDIA Corporation in the U.S. and other countries. Other company and product names may be trademarks of the respective companies with which they are associated. 
© 2021-2026 NVIDIA Corporation. All rights reserved. Last updated on Oct 19, 2026
Close
content here 
NVIDIA uses cookies to improve your experience on our web site. We and our third-party partners also use cookies and other tools to collect and record information you provide as well as information about your interactions with our websites for performance improvement, analytics, and to assist in marketing efforts. By clicking "Accept All", you consent to our use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/). You can manage your cookie settings by clicking on "Manage Settings." By continuing to use this site or by clicking one of the buttons below, you agree to our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control (GPC) signal and recorded your rejection of all optional cookies on this site for this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. To opt out of non-cookie personal information "sales" / "sharing" for targeted advertising purposes, please visit the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies. You can manage these settings in the NVIDIA [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies which overrides at least one of your previous settings. You can manage them in the [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
Manage Settings
Reject Optional Accept All
Cookie Settings
We and our third-party partners (including social media, advertising, and analytics partners) use cookies and other tracking technologies to collect, store, monitor, and process certain information about you when you visit our website. The information collected might relate to you, your preferences, or your device. We use that information to make the site work, analyze performance and traffic on our website, provide a more personalized web experience, and assist in our marketing efforts.Under certain privacy laws, you have the right to direct us not to "sell" or "share" your personal information for targeted advertising. To opt-out of the "sale" and "sharing" of personal information through cookies, you must opt-out of optional cookies using the toggles below. To opt out of the "sale" and "sharing" of data collected by other means (e.g., online forms) you must also update your data sharing preferences through the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/).Click on the different category headings below to find out more and change the settings according to your preference. You cannot opt out of Required Cookies as they are deployed to ensure the proper functioning of our website (such as prompting the cookie banner and remembering your settings, etc.). By clicking "Save and Accept" or "Decline All" at the bottom, you consent to the use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) in accordance with your settings and accept our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). For more information about our privacy practices, please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/).
Required Cookies
Always Active
These cookies enable core functionality such as security, network management, and accessibility. These cookies are required for the site to function and cannot be turned off.
Cookies Details
Performance Cookies
Performance Cookies
These cookies are used to provide quantitative measures of our website visitors, such as the number of times you visit, time on page, your mouse movements, scrolling, clicks and keystroke activity on the websites; other browsing, search, or product research behavior; and what brought you to our site. These cookies may store a unique ID so that our system will remember you when you return. Information collected with these cookies is used to measure and find ways to improve website performance.
Cookies Details
Personalization Cookies
Personalization Cookies
These cookies collect data about how you have interacted with our website to help us improve your web experience, such as which pages you have visited. These cookies may store a unique ID so that our system will remember you when you return. They may be set by us or by third party providers whose services we have added to our pages. These cookies enable us to provide enhanced website functionality and personalization as well as make the marketing messages we send to you more relevant to your interests. If you do not allow these cookies, then some or all of these services may not function properly.
Cookies Details
Advertising Cookies
Advertising Cookies
These cookies record your visit to our websites, the pages you have visited and the links you have followed to influence the advertisements that you see on other websites. These cookies and the information they collect may be managed by other companies, including our advertising partners, and may be used to build a profile of your interests and show you relevant advertising on other sites. We and our advertising partners will use this information to make our websites and the advertising displayed on it, more relevant to your interests.
Cookies Details
Back Button
Cookie List
Search Icon
Filter Icon
Clear
  * checkbox label label


Apply Cancel
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Decline All Save and Accept
CHAT
