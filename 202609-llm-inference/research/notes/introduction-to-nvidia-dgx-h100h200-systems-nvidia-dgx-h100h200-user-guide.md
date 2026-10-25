---
title: Introduction to NVIDIA DGX H100/H200 Systems — NVIDIA DGX H100/H200 User Guide
id: introduction-to-nvidia-dgx-h100h200-systems-nvidia-dgx-h100h200-user-guide
tags:
- onprem-llm-inference-stacks-859099
- datacenter-facilities
- primary-source
- hardware-spec
created: '2026-10-23T17:23:12.098006Z'
updated: '2026-10-23T17:23:51.846919Z'
source: https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html
source_domain: docs.nvidia.com
fetched_at: '2026-10-23T17:23:12.077077Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'NVIDIA DGX H100/H200 user guide (vendor primary spec): 6 x 3.3 kW PSUs,
  10.2 kW max system power, 1105 CFM front-to-back airflow, 200-240V AC. Primary spec
  for per-node power (vs Inflect''s 6.5-8 kW per server).'
---

*Suggested by [[inflect-colocation]] — cited as primary source*

[Skip to main content](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#main-content)
Back to top
[ NVIDIA DGX H100/H200 User Guide ](https://docs.nvidia.com/dgx/dgxh100-user-guide/index.html)
Light Dark System Settings
# Introduction to NVIDIA DGX H100/H200 Systems[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#introduction-to-nvidia-dgx-h100-h200-systems "Link to this heading")
The NVIDIA DGX™ H100/H200 Systems are the universal systems purpose-built for all AI infrastructure and workloads from analytics to training to inference. The DGX H100/H200 systems are built on eight NVIDIA H100 Tensor Core GPUs or eight NVIDIA H200 Tensor Core GPUs.
## Hardware Overview[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#hardware-overview "Link to this heading")
### DGX H100/H200 Component Descriptions[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#dgx-h100-h200-component-descriptions "Link to this heading")
The NVIDIA DGX H100 (640 GB)/H200 (1,128 GB) systems include the following components.  
Table 1. Component Description[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#id2 "Link to this table")  
| Component  | Description  |  
| --- | --- |  
| GPU  |  For H100: 8 x NVIDIA H100 GPUs that provide 640 GB total GPU memory For H200: 8 x NVIDIA H200 GPUs that provide 1,128 GB total GPU memory  |  
| CPU  | 2 x Intel Xeon 8480C PCIe Gen5 CPUs with 56 cores each 2.0/2.9/3.8 GHz (base/all core turbo/Max turbo)  |  
| NVSwitch  | 4 x 4th generation NVLinks that provide 900 GB/s GPU-to-GPU bandwidth  |  
| Storage (OS)  | 2 x 1.92 TB NVMe M.2 SSD (ea) in RAID 1 array  |  
| Storage (Data Cache)  | 8 x 3.84 TB NVMe U.2 SED (ea) in RAID 0 array  |  
| Network (Cluster) card  |  4 x OSFP ports for 8 x NVIDIA® ConnectX®-7 Single Port InfiniBand Cards Each card provides the following speeds:
  * InfiniBand (default): Up to 400Gbps
  * Ethernet: 400GbE, 200GbE, 100GbE, 50GbE, 40GbE, 25GbE, and 10GbE

 |  
| Network (storage and in-band management) card  |  2 x NVIDIA® ConnectX®-7 Dual Port Ethernet Cards Each card provides the following speeds:
  * Ethernet (default): 400GbE, 200GbE, 100GbE, 50GbE, 40GbE, 25GbE, and 10GbE
  * InfiniBand: Up to 400Gbps

 |  
| System memory (DIMM)  | 2 TB using 32 x DIMMs  |  
| BMC (out-of-band system management)  |  1 GbE RJ45 interface Supports Redfish, IPMI, SNMP, KVM, and Web user interface  |  
| System management interfaces  | Dual port 100GbE in slot 3 and 10 GbE RJ45 interface  |  
| Power supply  | 6 x 3.3 kW  |  
### Mechanical Specifications[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#mechanical-specifications "Link to this heading")  
Table 2. Mechanical Specifications[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#id3 "Link to this table")  
| Feature  | Description  |  
| --- | --- |  
| Form Factor  | 8U Rackmount  |  
| Height  | 14” (356 mm)  |  
| Width  | 19” (482.3 mm) max  |  
| Depth  | 35.3” (897.1 mm) max  |  
| System Weight  | 287.6 lbs (130.45 kg) max  |  
### Power Specifications[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#power-specifications "Link to this heading")
The DGX H100/H200 system contains six power supplies with balanced distribution of the power load.  
Table 3. Power Specifications[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#id4 "Link to this table")  
| Input  | Specification for Each Power Supply  |  
| --- | --- |  
| 200-240 volts AC  | 10.2 kW max.  | 3300 W @ 200-240 V, 16 A, 50-60 Hz  |  
#### Support for PSU Redundancy and Continuous Operation[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#support-for-psu-redundancy-and-continuous-operation "Link to this heading")
The system includes six power supply units (PSU) configured for 4+2 redundancy.
Refer to the following additional considerations:
  * If a PSU fails, troubleshoot the cause and replace the failed PSU immediately.
  * If three PSUs lose power as a result of a data center issue or power distribution unit failure, the system continues to function, but at a reduced performance level.
  * If only three PSUs have power, shut down the system before replacing an operational PSU.
  * The system only boots if at least three PSUs are operational. If fewer than three PSUs are operational, only the BMC is available.
  * Do not operate the system with PSUs depopulated.


### DGX H100/H200 Locking Power Cord Specification[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#dgx-h100-h200-locking-power-cord-specification "Link to this heading")
The DGX H100/H200 system is shipped with a set of six (6) locking power cords that have been qualified for use with the DGX H100/H200 system to ensure regulatory compliance.
Warning
To avoid electric shock or fire, only use the NVIDIA-provided power cords to connect power to the DGX H100/H200. For more details, refer to [Electrical Precautions](https://docs.nvidia.com/dgx/dgxh100-user-guide/safety.html#electrical-precautions).
Important
Do not use the provided cables with any other product or for any other purpose.
Power Cord Specification  
| Power Cord Feature  | Specification  |  
| --- | --- |  
| Electrical  | 250VAC, 20A  |  
| Plug Standard  | C19/C20  |  
| Dimension  | 1200mm length  |  
| Compliance  |  Cord: UL62, IEC60227 Connector/Plug: IEC60320-1  |  
### Using the Locking Power Cords[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#using-the-locking-power-cords "Link to this heading")
This section provides information about how to use the locking power cords.
Locking and Unlocking the PDU Side
Power Distribution Unit side
  * To INSERT, push the cable into the PDU socket.
  * To REMOVE, press the clips together and pull the cord out of the socket.


Locking/Unlocking the PSU Side (Cords with Twist-Lock Mechanism)
Power Supply (System) side - Twist locking
  * To INSERT or REMOVE make sure the cable is UNLOCKED and push/ pull into/out of the socket.


### Environmental Specifications[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#environmental-specifications "Link to this heading")
Here are the environmental specifications for your DGX H100/H200 system.  
| Feature  | Specification  |  
| --- | --- |  
| Operating Temperature  | 5° C to 30° C (41° F to 86° F)  |  
| Relative Humidity  | 20% to 80% non-condensing  |  
| Airflow  | 1105 CFM Front-to-Back @ 80% fan PWM  |  
| Heat Output  | 38,557 BTU/hr  |  
### Front Panel Connections and Controls[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#front-panel-connections-and-controls "Link to this heading")
This section provides information about the front panel, connections, and controls of the DGX H100/H200 system.
#### With a Bezel[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#with-a-bezel "Link to this heading")
Here is an image of the DGX H100/H200 system with a bezel.  
| Control  | Description  |  
| --- | --- |  
| Power Button  |  Press to turn the DGX H100/H200 system On or Off.
  * Green flashing (1 Hz): Standby (BMC booted)
  * Green flashing (4 Hz): POST in progress
  * Green solid On: Power On

 |  
| ID Button  |  Press to have the blue LED turn On or blink (configurable through the BMC) as an identifier during servicing. Also causes an LED on the back of the unit to flash as an identifier during servicing.  |  
| Fault LED  | Amber On: System or component faulted  |  
#### With the Bezel Removed[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#with-the-bezel-removed "Link to this heading")
Here is an image of the DGX H100/H200 system without a bezel.
Important
Refer to the section [First Boot Setup](https://docs.nvidia.com/dgx/dgxh100-user-guide/first-boot-setup.html) for instructions on how to properly turn the system on or off.
### Rear Panel Modules[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#rear-panel-modules "Link to this heading")
Here is an image that shows the real panel modules on DGX H100/H200.
### Motherboard Connections and Controls[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#motherboard-connections-and-controls "Link to this heading")
Here is an image that shows the motherboard connections and controls in a DGX H100/H200 system.  
Table 4. Motherboard Controls[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#id5 "Link to this table")  
| Control  | Description  |  
| --- | --- |  
| Power Button  | Press to turn the system On or Off.  |  
| ID LED Button  | Blinks when ID button is pressed from the front of the unit as an aid in identifying the unit needing servicing.  |  
| BMC Reset button  | Press to manually reset the BMC.  |  
See [Network Connections, Cables, and Adaptors](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#network-conn-cables-adaptors) for details on the network connections.
### Motherboard Tray Components[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#motherboard-tray-components "Link to this heading")
Here is an image that shows the motherboard tray components in a DGX H100/H200 system.
### GPU Tray Components[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#gpu-tray-components "Link to this heading")
Here is an image of the GPU tray components in a DGX H100/H200 system.
## Network Connections, Cables, and Adaptors[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#network-connections-cables-and-adaptors "Link to this heading")
This section provides information about network connections, cables, and adaptors.
### Network Ports[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#network-ports "Link to this heading")
Here is an image that shows the network ports on a DGX H100/H200 system.  
Table 5. Network Port Mapping[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#id6 "Link to this table")  
| Port Designation  |  
| --- |  
| Port  | PCI Bus  | Default  | Optional  | RDMA  |  
| OSFP1P1  | dc:00.0  | ibp220s0  | enp220s0np0  | mlx5_11  |  
| OSFP1P2  | 9a:00.0  | ibp154s0  | enp154s0np0  | mlx5_6  |  
| OSFP2P1  | ce:00.0  | ibp206s0  | enp206s0np0  | mlx5_10  |  
| OSFP2P2  | c0:00.0  | ibp192s0  | enp192s0np0  | mlx5_9  |  
| OSFP3P1  | 4f:00.0  | ibp79s0  | enp79s0np0  | mlx5_4  |  
| OSFP3P2  | 40:00.0  | ibp64s0  | enp64s0np0  | mlx5_3  |  
| OSFP4P1  | 5e:00.0  | ibp94s0  | enp94s0np0  | mlx5_5  |  
| OSFP4P2  | 18:00.0  | ibp24s0  | enp24s0np0  | mlx5_0  |  
| Slot1 P1  | aa:00.0  | ibp170s0f0  | enp170s0f0np0  | mlx5_7  |  
| Slot1 P2  | aa:00.1  | enp170s0f1np1  | ibp170s0f1  | mlx5_8  |  
| Slot2 P1  | 29:00.0  | ibp41s0f0  | enp41s0f0np0  | mlx5_1  |  
| Slot2 P2  | 29:00.1  | enp41s0f1np1  | ibp41s0f1  | mlx5_2  |  
| Slot3 P1  | 82:00.0  | ens6f0  | N/A  | irdma0  |  
| Slot3 P2  | 82:00.1  | ens6f1  | N/A  | irdma1  |  
| On-board  | 0b:00.0  | eno3  | N/A  |  
### Compute and Storage Networking[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#compute-and-storage-networking "Link to this heading")
### Network Modules[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#network-modules "Link to this heading")
  * New form factor for aggregate PCIe network devices
  * Consolidates four ConnectX-7 networking cards into a single device
  * Two networking modules are installed on interposer board
  * Interposer board connects to CPUs on one end and to GPU tray on the other
  * DensiLink cables are used to go directly from ConnectX-7 networking cards to OSFP connectors at the back of the system


Each DensiLink cable has two ports, one from each ConnectX-7 card  
Table 6. Network Modules[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#id7 "Link to this table")  
| Port  | ConnectX Device  | Network Module/CPU  | GPU  | Default  | RDMA  |  
| --- | --- | --- | --- | --- | --- |  
| OSFP1P1  | CX0  | ibp220s0  | mlx5_11  |  
| OSFP1P2  | CX1  | ibp154s0  | mlx5_6  |  
| OSFP2P1  | CX2  | ibp206s0  | mlx5_10  |  
| OSFP2P2  | CX3  | ibp192s0  | mlx5_9  |  
| OSFP3P1  | CX2  | ibp79s0  | mlx5_4  |  
| OSFP3P2  | CX3  | ibp64s0  | mlx5_3  |  
| OSFP4P1  | CX0  | ibp94s0  | mlx5_5  |  
| OSFP4P2  | CX1  | ibp24s0  | mlx5_0  |  
### BMC Port LEDs[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#bmc-port-leds "Link to this heading")
The BCM RJ-45 port has two LEDs.
The LED on the left indicates the speed. Solid green indicates the speed is 100M. Solid amber indicates the speed is 1G.
The LED on the right is green and flashes to indicate activity.
### Supported Network Cables and Adaptors[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#supported-network-cables-and-adaptors "Link to this heading")
The DGX H100/H200 system is not shipped with network cables or adaptors. You will need to purchase supported cables or adaptors for your network.
The ConnectX-7 firmware determines which cables and adaptors are supported. For a list of cables and adaptors compatible with the NVIDIA ConnectX cards installed in the DGX H100/H200 system,
  1. Visit the [NVIDIA Adapter Firmware Release](https://docs.nvidia.com/networking/category/adapterfw) page.
  2. Click the ConnectX model and select the corresponding firmware included in the DGX H100/H200 system.
  3. From the left **Topics** pane, select the Validated and Supported Cables and Switches topic.


## DGX H100/200 System Topology[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#dgx-h100-200-system-topology "Link to this heading")
The following figure shows the DGX H100/H200 system topology.
## DGX OS Software[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#dgx-os-software "Link to this heading")
The DGX H100/H200 system comes pre-installed with a DGX software stack incorporating the following components:
  * An Ubuntu server distribution with supporting packages.
  * The following system management and monitoring software:
    * NVIDIA System Management (NVSM)
Provides active health monitoring and system alerts for NVIDIA DGX nodes in a data center. It also provides simple commands for checking the health of the DGX H100/H200 system from the command line.
    * Data Center GPU Management (DCGM)
This software enables node-wide administration of GPUs and can be used for cluster and data-center level management.
  * DGX H100/H200 system support packages.
  * The NVIDIA GPU driver
  * Docker Engine
  * NVIDIA Container Toolkit
  * NVIDIA Networking OpenFabrics Enterprise Distribution for Linux (MOFED)
  * NVIDIA Networking Software Tools (MST)
  * cachefilesd (daemon for managing cache data storage)


## Customer Support[#](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html#customer-support "Link to this heading")
Contact NVIDIA Enterprise Support for assistance in reporting, troubleshooting, or diagnosing problems with your DGX H100/H200 system. Also contact NVIDIA Enterprise Support for assistance in moving the DGX H100/H200 system.
  * For contracted Enterprise Support questions, you can send an email to enterprisesupport@nvidia.com.
  * For additional details about how to obtain support, go to [NVIDIA Enterprise Support](https://www.nvidia.com/en-us/support/enterprise/).


Our support team can help collect appropriate information about your issue and involve internal resources as needed.
On this page 
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
