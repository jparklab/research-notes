---
title: Chapter 3. Deploy OpenShift AI in a disconnected environment | Deploy or decommission
  OpenShift AI in disconnected environments | Red Hat OpenShift AI Self-Managed |
  3.5 | Red Hat Documentation
id: chapter-3-deploy-openshift-ai-in-a-disconnected-environment-deploy-or-decommissi
tags:
- onprem-llm-inference-stacks-859099
- onprem
- vendor-doc
created: '2026-10-23T18:12:49.322341Z'
updated: '2026-10-23T18:13:05.083869Z'
source: https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install
source_domain: docs.redhat.com
fetched_at: '2026-10-23T18:12:49.301063Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Red Hat OpenShift AI Self-Managed 3.5 docs (fetched 2026-10-23), ch.3 disconnected
  install. Vendor documentation = recommendation, not deployment evidence. Requires
  OCP 4.19-4.20 (llm-d needs 4.20+); min 2 worker nodes 8 CPU/32GiB (SNO 32 CPU/128GiB);
  default dynamic storage class; oc-mirror v2 mirroring of all images to private registry;
  KServe model serving needs cert-manager; llm-d distributed inference needs Connectivity
  Link + Leader Worker Set operators; NVIDIA GPU Operator + NFD for GPU nodes; NVIDIA
  domains NOT needed for standard install (CUDA images prebuilt on registry.redhat.io).
  Example imageset pins RHOAI 2.25.0 on OCP 4.19 catalog. Layer replaced: the Kubernetes/MLOps
  control plane, not the engine (vLLM/llm-d run on top).'
---

1. [Red Hat OpenShift AI Self-Managed](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed)  
  2. [Deploy or decommission OpenShift AI in disconnected environments](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment)
  3. Chapter 3. Deploy OpenShift AI in a disconnected environment 


Ask AI
# Chapter 3. Deploy OpenShift AI in a disconnected environment
Read this section to understand how to deploy Red Hat OpenShift AI as a development and testing environment for data scientists in a disconnected environment. Disconnected clusters are on a restricted network, typically behind a firewall. In this case, clusters cannot access the remote registries where Red Hat provided OperatorHub sources reside. Instead, the Red Hat OpenShift AI Operator can be deployed to a disconnected environment using a private registry to mirror the images. 
Installing OpenShift AI in a disconnected environment involves the following high-level tasks: 
  1. Confirm that your OpenShift cluster meets all requirements. 
  2. Mirror images to a private registry. 
  3. Install the Red Hat OpenShift AI Operator. 
  4. Install OpenShift AI components. 
  5. Complete any additional configuration required for the components you enabled. 
  6. Configure user and administrator groups to provide user access to OpenShift AI. 
  7. Provide your users with the URL for the OpenShift cluster on which you deployed OpenShift AI. 


##  [3.1. Requirements for OpenShift AI Self-Managed](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#requirements-for-openshift-ai-self-managed_install)
Copy linkLink copied to clipboard!
You must meet the following requirements before you can install Red Hat OpenShift AI on your Red Hat OpenShift cluster in a disconnected environment. 
###  [3.1.1. Platform requirements](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#platform_requirements)
Copy linkLink copied to clipboard!
**Subscriptions** * A subscription for Red Hat OpenShift AI Self-Managed is required. 
Contact your Red Hat account manager to purchase new subscriptions. If you do not yet have an account manager, complete the form at [https://www.redhat.com/en/contact](https://www.redhat.com/en/contact/) to request one. 
**Cluster administrator access** * Cluster administrator access is required to install OpenShift AI. * You can use an existing cluster or create a new one that meets the supported version requirements. 
**Supported OpenShift versions** The following OpenShift versions are supported for installing OpenShift AI: 
  * OpenShift Container Platform 4.19 to 4.20. See [Installing a cluster in a disconnected environment](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/disconnected_environments/installing-disconnected-environments). 
    * To deploy models by using Distributed Inference with llm-d, your cluster must be running version 4.20 or later. 
  * After installing the cluster, configure the Cluster Samples Operator as described in [Configuring Samples Operator for a restricted cluster](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/images/configuring-samples-operator#samples-operator-restricted-network-install). 
  * OpenShift Kubernetes Engine (OKE). See [About OpenShift Kubernetes Engine](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/overview/oke-about#feature-summary). 
Note
While OpenShift Kubernetes Engine (OKE) typically restricts the installation of certain post-installation Operators, Red Hat provides a specific licensing exception for Red Hat OpenShift AI users. This exception exclusively applies to Operators used to support Red Hat OpenShift AI workloads. Installing or using these Operators for purposes unrelated to OpenShift AI is a violation of the OKE service agreement. 
    * The following Operators are required dependencies for Red Hat OpenShift AI 2.x and 3.x. These Operators are _not supported_ on OKE, but can be installed if given an exception. 
Expand  
| Red Hat OpenShift AI version  | Operator (Unsupported, Exception Required)  |  
| --- | --- |  
|  Authorino Operator, Service Mesh Operator, Serverless Operator   |  
|  Job-set-operator, openshift-custom-metrics-autoscaler-operator, cert-manager Operator, Leader Worker Set Operator, Red Hat Connectivity Link Operator, Kueue Operator (RHBOK), SR-IOV Operator, GPU Operator (with custom configurations), OpenTelemetry, Tempo, Cluster Observability Operator, IBM Spyre Operator.   |  
Show more


Note
On OpenShift 4.21 and later, the OLMv1 catalog is enabled by default as a Technology Preview feature. If you plan to install dependency Operators, such as the Node Feature Discovery (NFD) Operator or the NVIDIA GPU Operator from OperatorHub, you might be redirected to a `ClusterExtensions` page instead of the standard installation form. To restore the standard installation experience, disable the OLMv1 catalog before installing these Operators. For more information, see [Troubleshooting common installation problems](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed/troubleshooting-common-installation-problems_install). 
Important
In OpenStack, CodeReady Containers (CRC), and other private cloud environments without integrated external DNS, you must manually configure DNS A or CNAME records after installing the Operator and components, when the LoadBalancer IP becomes available. For more information, see [Configuring External DNS for RHOAI 3.x on OpenStack and Private Clouds](https://access.redhat.com/articles/7133770). 
**Cluster configuration** * A minimum of 2 worker nodes with at least 8 CPUs and 32 GiB RAM each is required to install the Operator. * For single-node OpenShift clusters, the node must have at least 32 CPUs and 128 GiB RAM. * Additional resources are required depending on your workloads. * Open Data Hub must not be installed on the cluster. 
**Storage requirements** * Your cluster must have a default storage class that supports dynamic provisioning. To confirm that a default storage class is configured, run the following command: 
+ 

```
oc get storageclass
```

+ If no storage class is marked as the default, see [Changing the default storage class](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/storage/dynamic-provisioning#change-default-storage-class_dynamic-provisioning) in the OpenShift Container Platform documentation. 
**Identity provider configuration** * An identity provider must be configured for your OpenShift cluster, which provides authentication for OpenShift AI. See [Understanding identity provider configuration](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/authentication_and_authorization/understanding-identity-provider). * You must access the cluster as a user with the `cluster-admin` role; the `kubeadmin` user is not allowed. For more information, see the relevant documentation: **OpenShift Container Platform:[Creating a cluster admin](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/authentication_and_authorization/using-rbac#creating-cluster-admin_using-rbac)** OpenShift Dedicated: [Managing OpenShift Dedicated administrators](https://docs.redhat.com/en/documentation/openshift_dedicated/4/html/authentication_and_authorization/osd-admin-roles#dedicated-administrators-adding-user_osd-admin-roles) ** ROSA: [Creating a cluster administrator user for quick cluster access](https://docs.redhat.com/en/documentation/red_hat_openshift_service_on_aws_classic_architecture/4/html/getting_started/rosa-quickstart-guide-ui)
**Internet access on the mirroring machine** * Along with internet access, the following domains must be accessible to mirror images required for the OpenShift AI installation: 
  * `cdn.redhat.com`
  * `subscription.rhn.redhat.com`
  * `registry.access.redhat.com`
  * `registry.redhat.io`
  * `quay.io`
    * For environments that build or customize CUDA-based images using NVIDIA’s base images, or that directly pull artifacts from the NVIDIA NGC catalog, the following domains must also be accessible: 
  * `ngc.download.nvidia.cn`
  * `developer.download.nvidia.com`


Note
Access to these NVIDIA domains is not required for standard OpenShift AI installations. The CUDA-based container images used by OpenShift AI are prebuilt and hosted on Red Hat’s registry at `registry.redhat.io`. 
**Image mirroring** * For disconnected environments, you must mirror all required images to your private registry before installing OpenShift AI. See the RHOAI disconnected installation guide for details. 
**Object storage** * Several components of OpenShift AI require or can use S3-compatible object storage, such as AWS S3, MinIO, Ceph, or IBM Cloud Storage. Object storage provides HTTP-based access to data by using the S3 API, which is the standard interface for most object storage services. 
  * Object storage must be reachable from the OpenShift cluster and deployed within the same disconnected network. 
  * Object storage is required for: 
    * Single-model serving platform, for storing and deploying models. 
    * AI pipelines, for storing artifacts, logs, and intermediate results. 
  * Object storage can also be used by: 
    * Workbenches, for accessing large datasets. 
    * Kueue-based workloads, for reading input data and writing output results. 
    * Code executed inside pipelines, for persisting generated models or other artifacts. 


**Custom namespaces** * By default, OpenShift AI uses predefined namespaces, but you can define custom namespaces for the Operator, applications, and workbenches if needed. Namespaces created by OpenShift AI typically include `openshift` or `redhat` in their name. Do not rename these system namespaces because they are required for OpenShift AI to function properly. * If you use custom namespaces, create and label them before installing the OpenShift AI Operator. See [Configuring custom namespaces](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed/installing-and-deploying-openshift-ai_install#configuring-custom-namespaces). 
###  [3.1.2. Component requirements](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#component_requirements)
Copy linkLink copied to clipboard!
Meet the requirements for the components and capabilities that you plan to use. 
**Workbenches (`workbenches`)** * To use a custom workbench namespace, create the namespace before installing the OpenShift AI Operator. See [Configuring custom namespaces](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed/installing-and-deploying-openshift-ai_install#configuring-custom-namespaces). 
**AI Pipelines (`aipipelines`)** * To store your pipeline artifacts in an S3-compatible object storage bucket so that you do not consume local storage, configure write access to your S3 bucket on your storage account. * If your cluster is running in FIPS mode, any custom container images for data science pipelines must be based on UBI 9 or RHEL 9. This ensures compatibility with FIPS-approved pipeline components and prevents errors related to mismatched OpenSSL or GNU C Library (glibc) versions. * To use your own Argo Workflows instance, after installing the OpenShift AI Operator see [Configuring pipelines with your own Argo Workflows instance](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed/configuring-pipelines-with-your-own-argo-workflows-instance_install). 
**Kueue-based workloads (`kueue` , `ray`, `trainingoperator`)** * Install the Red Hat build of Kueue Operator. * Install the cert-manager Operator. * See [Configuring workload management with Kueue](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/managing_openshift_ai/managing-workloads-with-kueue#configuring-workload-management-with-kueue_kueue) and [Installing the distributed workloads components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed/installing-the-distributed-workloads-components_install). 
**Trainer (`trainer`)** * Install the JobSet Operator. For more information, see [Installing the JobSet Operator](https://docs.redhat.com/en/documentation/openshift_container_platform/4.21/html/ai_workloads/jobset-operator). 
**Model serving platform (`kserve`)** * Install the cert-manager Operator. * To configure custom CPU and memory resource allocations for the OAuth proxy sidecar container in KServe inference service pods, see [Configure OAuth proxy sidecar resources for KServe](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/configuring_your_model-serving_platform/configuring-your-model-serving-platform_rhoai-admin#configuring-oauth-proxy-sidecar-resources_rhoai-admin). 
**Distributed Inference with llm-d (advanced`kserve`)** * Install the cert-manager Operator. * Install the Red Hat Connectivity Link Operator. * Install the Red Hat Leader Worker Set Operator. * To use the built-in observability dashboards for Distributed Inference with llm-d deployments, install the Cluster Observability Operator. For more information, see [Installing the Cluster Observability Operator](https://docs.redhat.com/en/documentation/red_hat_openshift_cluster_observability_operator/1-latest/html/installing_red_hat_openshift_cluster_observability_operator/installing-cluster-observability-operators). * See [Deploying models by using Distributed Inference with llm-d](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/deploying_models/deploying_models#deploying-models-using-distributed-inference_rhoai-user). 
**OGX and RAG workloads (`ogxoperator`)** * Install the OGX Operator. * Install the Red Hat OpenShift Service Mesh Operator 3.x. * Install the cert-manager Operator. * Ensure you have GPU-enabled nodes available on your cluster. * Install the Node Feature Discovery Operator. * Install the NVIDIA GPU Operator. * Configure access to S3-compatible object storage for your model artifacts. * See [Working with OGX](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/working_with_ogx). 
**Model registry (`modelregistry`)** * Configure access to an external MySQL database 5.x or later; 8.x is recommended. * Configure access to S3-compatible object storage. * See [Creating a model registry](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/managing_model_registries/creating-a-model-registry_managing-model-registries). 
##  [3.2. Mirror images to a private registry for a disconnected installation](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#mirroring-images-to-a-private-registry-for-a-disconnected-installation_install)
Copy linkLink copied to clipboard!
You can install the Red Hat OpenShift AI Operator to your OpenShift cluster in a disconnected environment by mirroring the required container images to a private container registry. After mirroring the images to a container registry, you can install Red Hat OpenShift AI Operator by using OperatorHub. 
You can use the _mirror registry for Red Hat OpenShift_ , a small-scale container registry, as a target for mirroring the required container images for OpenShift AI in a disconnected environment. Using the mirror registry for Red Hat OpenShift is optional if another container registry is already available in your installation environment. 
**Prerequisites**
  * You have cluster administrator access to a running OpenShift Container Platform cluster, version 4.19 or greater. 
  * You have credentials for Red Hat OpenShift Cluster Manager (<https://console.redhat.com/openshift/>). 
  * Your mirroring machine is running Linux, has 100 GB of space available, and has access to the Internet so that it can obtain the images to populate the mirror repository. 
  * You have installed the OpenShift CLI (`oc`). 
  * You have reviewed the component requirements and identified all operators you must mirror in addition to the Red Hat OpenShift AI Operator. See [Requirements for OpenShift AI Self-Managed](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#requirements-for-openshift-ai-self-managed_install). 


Note
This procedure uses the oc-mirror plugin v2; the oc-mirror plugin v1 is now deprecated. For more information, see [Changes from oc-mirror plugin v1 to v2](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/disconnected_environments/oc-mirror-migration-v1-to-v2#oc-mirror-migration-differences_oc-mirror-migration-v1-to-v2) in the OpenShift documentation. 
**Procedure**
  1. Create a mirror registry. See [Creating a mirror registry with mirror registry for Red Hat OpenShift](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/disconnected_environments/installing-mirroring-creating-registry) in the OpenShift Container Platform documentation. 
  2. To mirror registry images, install the `oc-mirror` OpenShift CLI plugin v2 on your mirroring machine running Linux. See [Installing the oc-mirror OpenShift CLI plugin](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/disconnected_environments/about-installing-oc-mirror-v2#installation-oc-mirror-installing-plugin_about-installing-oc-mirror-v2) in the OpenShift Container Platform documentation. 
Important
The oc-mirror plugin v1 is deprecated. Red Hat recommends that you use the oc-mirror plugin v2 for continued support and improvements. 
  3. Create a container image registry credentials file that allows mirroring images from Red Hat to your mirror. See [Configuring credentials that allow images to be mirrored](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/disconnected_environments/about-installing-oc-mirror-v2#installation-adding-registry-pull-secret_about-installing-oc-mirror-v2) in the OpenShift Container Platform documentation. 
  4. Open the example image set configuration file (`rhoai-<version>.md`) from the [disconnected installer helper](https://github.com/red-hat-data-services/rhoai-disconnected-install-helper/tree/main) repository and examine its contents. 
The disconnected installer helper file includes a list of **Additional images** required to install OpenShift AI in a disconnected environment, as well as a list of older **Unsupported images** provided for reference only. These older images are no longer maintained by Red Hat but are included for convenience, such as when importing older resources or maintaining compatibility with previous environments. 
  5. Using the example image set configuration file, create a file called `imageset-config.yaml` and populate it with values suitable for the image set configuration in your deployment. 
     * To view a list of the available OpenShift versions, run the following command. This might take several minutes. If the command returns errors, repeat the steps in [Configuring credentials that allow images to be mirrored](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/disconnected_environments/about-installing-oc-mirror-v2#installation-adding-registry-pull-secret_about-installing-oc-mirror-v2). 

```
oc-mirror list operators
```

     * To see the available channels for a package in a specific version of OpenShift Container Platform (for example, 4.19), run the following command: 

```
oc-mirror list operators --catalog=registry.redhat.io/redhat/redhat-operator-index:v4.19 --package=<package_name>
```

     * For information about subscription update channels, see [Understanding update channels](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/understanding-update-channels_install). 
Important
The example image set configurations are for demonstration purposes only and might need further alterations depending on your deployment. 
To identify the attributes most suitable for your deployment, see [Image set configuration parameters](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html/disconnected_environments/installing-mirroring-disconnected#oc-mirror-imageset-config-params_installing-mirroring-disconnected) and [Image set configuration examples](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html/disconnected_environments/installing-mirroring-disconnected#oc-mirror-image-set-examples_installing-mirroring-disconnected) in the OpenShift Container Platform documentation. 
**Example imageset-config.yaml**

```
 ImageSetConfiguration
apiVersion mirror.openshift.io/v1alpha2
mirror
  operators
     catalog registry.redhat.io/redhat/redhatoperatorindexv4.19
      packages
          rhodsoperator
          channels
              stable
              minVersion 2.25.0
              maxVersion 2.25.0
          <additional_operator_name
          channels
              stable
  additionalImages
      <additional_image_name
```

  6. Download the specified image set configuration to a local file on your mirroring machine: 
     * Replace `<mirror_rhoai>` with the target directory where you want to output the image set file. 
     * The target directory path must start with `file://`. 
     * The download might take several minutes. 

```
$ oc mirror -c imageset-config.yaml file://<mirror_rhoai> --v2
```

Tip
If the `tls: failed to verify certificate: x509: certificate signed by unknown authority` error is returned and you want to ignore it, set `skipTLS` to `true` in your image set configuration file and run the command again. 
  7. Verify that the image set `.tar` files were created: 

```
$ ls <mirror_rhoai>
```

The following is an example of the output: 

```
mirror_000001.tar, mirror_000002.tar
```

If an `archiveSize` value was specified in the image set configuration file, the image set might be separated into multiple `.tar` files. 
  8. Optional: Verify that total size of the image set `.tar` files is around 75 GB: 

```
$ du -h --max-depth=1 ./<mirror_rhoai>/
```

If the total size of the image set is significantly less than 75 GB, run the `oc mirror` command again. 
  9. Upload the contents of the generated image set to your target mirror registry: 
     * Replace `<mirror_rhoai>` with the directory that contains your image set `.tar` files. 
     * Replace `<registry.example.com:5000>` with your mirror registry. 

```
$ oc mirror -c imageset-config.yaml --from file://<mirror_rhoai> docker://<registry.example.com:5000> --v2
```

Tip
If the `tls: failed to verify certificate: x509: certificate signed by unknown authority` error is returned and you want to ignore it, run the following command: 

```
$ oc mirror --dest-tls-verify false --from=./<mirror_rhoai> docker://<registry.example.com:5000> --v2
```

  10. Log in to your target OpenShift cluster using the OpenShift CLI as a user with the `cluster-admin` role. 
  11. Verify that the YAML files are present for the `ImageDigestMirrorSet` and `CatalogSource` resources: 
     * Replace `<mirror_rhoai>` with the directory that contains your image set `.tar` files. 

```
$ ls <mirror_rhoai>/working-dir/cluster-resources/
```

The following is an example of the output: 

```
cs-redhat-operator-index.yaml
idms-oc-mirror.yaml
```

  12. Install the generated resources into the cluster: 
     * Replace `<oc_mirror_workspace_path>` with the path to your oc mirror workspace. 

```
$ oc apply -f <oc_mirror_workspace_path>/working-dir/cluster-resources
```



**Verification**
  * Verify that the `CatalogSource` and pod were created successfully: 

```
$ oc get catalogsource,pod -n openshift-marketplace
```

This should return at least one catalog and two pods. 
  * Check that the Red Hat OpenShift AI Operator exists in the OperatorHub: 
    1. Log in to the OpenShift web console. 
    2. Click **Operators** **OperatorHub**. 
The **OperatorHub** page opens. 
    3. Confirm that the Red Hat OpenShift AI Operator is shown. 
  * If you mirrored additional operators, check that those operators exist in the OperatorHub. 


##  [3.3. Configure custom namespaces](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#configuring-custom-namespaces)
Copy linkLink copied to clipboard!
By default, OpenShift AI uses the following predefined namespaces: 
  * `redhat-ods-operator` contains the Red Hat OpenShift AI Operator 
  * `redhat-ods-applications` includes the dashboard and other required components of OpenShift AI 
  * `rhods-notebooks` is where basic workbenches are deployed by default 


If needed, you can define custom namespaces to use instead of the predefined ones before installing OpenShift AI. This flexibility supports environments with naming policies or conventions and allows cluster administrators to control where components such as workbenches are deployed. 
Namespaces created by OpenShift AI typically include `openshift` or `redhat` in their name. Do not rename these system namespaces because they are required for OpenShift AI to function properly. 
**Prerequisites**
  * You have access to an OpenShift AI cluster with cluster administrator privileges. 
  * You have installed the OpenShift CLI (`oc`) as described in the appropriate documentation for your cluster: 
    * [Installing the OpenShift CLI](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/cli_tools/openshift-cli-oc#installing-openshift-cli) for OpenShift Container Platform 
    * [Installing the OpenShift CLI](https://docs.redhat.com/en/documentation/red_hat_openshift_service_on_aws/4/html/cli_tools/openshift-cli-oc#installing-openshift-cli) for Red Hat OpenShift Service on AWS 
  * You have not yet installed the Red Hat OpenShift AI Operator. 


**Procedure**
  1. In a terminal window, if you are not already logged in to your OpenShift cluster as a cluster administrator, log in to the OpenShift CLI (`oc`) as shown in the following example: 

```
oc login _<openshift_cluster_url>_ -u _<admin_username>_ -p _<password>_
```

  2. Optional: To configure a custom operator namespace: 
    1. Create a namespace YAML file named `operator-namespace.yaml`. 

```
apiVersion v1
 Namespace
metadata
   <operatornamespace
```

       * Defines the operator namespace. 
    2. Create the namespace in your OpenShift cluster. 

```
$ oc create -f operator-namespace.yaml
```

You see output similar to the following: 

```
namespace/_<operator-namespace>_ created
```

    3. When you install the Red Hat OpenShift AI Operator, use this namespace instead of `redhat-ods-operator`. 
  3. Optional: To configure a custom applications namespace: 
    1. Create a namespace YAML file named `applications-namespace.yaml`. 

```
apiVersion v1
 Namespace
metadata
   <applicationsnamespace
  labels
    opendatahub.io/application-namespace 'true'
```

       * `name` - Defines the applications namespace. 
       * `opendatahub.io/application-namespace` - Adds the required label. 
    2. Create the namespace in your OpenShift cluster. 

```
$ oc create -f applications-namespace.yaml
```

You see output similar to the following: 

```
namespace/_<applications-namespace>_ created
```

  4. Optional: To configure a custom workbench namespace: 
    1. Create a namespace YAML file named `workbench-namespace.yaml`. 

```
apiVersion v1
 Namespace
metadata
   <workbenchnamespace
```

       * Defines the workbench namespace. 
    2. Create the namespace in your OpenShift cluster. 

```
$ oc create -f workbench-namespace.yaml
```

You see output similar to the following: 

```
namespace/_<workbench-namespace>_ created
```

    3. When you install the Red Hat OpenShift AI components, specify this namespace for the `spec.workbenches.workbenchNamespace` field. You cannot change the default workbench namespace after you have installed the Red Hat OpenShift AI Operator. 


**Next step**
[Installing the Red Hat OpenShift AI Operator](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-the-openshift-ai-operator_operator-install)
##  [3.4. Install the Red Hat OpenShift AI Operator](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-the-openshift-ai-operator_operator-install)
Copy linkLink copied to clipboard!
This section shows how to install the Red Hat OpenShift AI Operator on your OpenShift cluster using the command-line interface (CLI) and the OpenShift web console. 
Note
If your OpenShift cluster uses a proxy to access the Internet, you can configure the proxy settings for the Red Hat OpenShift AI Operator. For more information, see _Overriding proxy settings of an Operator_ in the OpenShift Container Platform documentation. 
###  [3.4.1. Installing the Red Hat OpenShift AI Operator by using the CLI](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-openshift-ai-operator-using-cli_operator-install)
Copy linkLink copied to clipboard!
The following procedure shows how to use the OpenShift CLI (`oc`) to install the Red Hat OpenShift AI Operator on your OpenShift cluster. You must install the Operator before you can install OpenShift AI components on the cluster. 
**Prerequisites**
  * You have a running OpenShift cluster, version 4.19 or greater, configured with a default storage class that can be dynamically provisioned. 
  * You have cluster administrator privileges for your OpenShift cluster. 
  * You have installed the OpenShift CLI (`oc`) as described in the appropriate documentation for your cluster: 
    * [Installing the OpenShift CLI](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/cli_tools/openshift-cli-oc#installing-openshift-cli) for OpenShift Container Platform 
    * [Installing the OpenShift CLI](https://docs.redhat.com/en/documentation/red_hat_openshift_service_on_aws/4/html/cli_tools/openshift-cli-oc#installing-openshift-cli) for Red Hat OpenShift Service on AWS 
  * If you are using custom namespaces, you have created and labeled them as required. 
Note
The example commands in this procedure use the predefined operator namespace. If you are using a custom operator namespace, replace `redhat-ods-operator` with your namespace. 
  * You have mirrored the required container images to a private registry. See [Mirroring images to a private registry for a disconnected installation](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#mirroring-images-to-a-private-registry-for-a-disconnected-installation_install). 


**Procedure**
  1. Open a new terminal window. 
  2. Follow these steps to log in to your OpenShift cluster as a cluster administrator: 
    1. In the upper-right corner of the OpenShift web console, click your user name and select **Copy login command**. 
    2. After you have logged in, click **Display token**. 
    3. Copy the **Log in with this token** command and paste it in your terminal. 

```
$ oc login --token=_<token>_ --server=_<openshift_cluster_url>_
```

  3. Create a namespace for installation of the Operator by performing the following actions: 
Note
If you have already created a custom namespace for the Operator, you can skip this step. 
    1. Create a namespace YAML file named `rhods-operator-namespace.yaml`. 

```
apiVersion v1
 Namespace
metadata
   redhatodsoperator
```

       * Defines the operator namespace. 
    2. Create the namespace in your OpenShift cluster. 

```
$ oc create -f rhods-operator-namespace.yaml
```

You see output similar to the following: 

```
namespace/redhat-ods-operator created
```

  4. Create an operator group for installation of the Operator by performing the following actions: 
    1. Create an `OperatorGroup` object custom resource (CR) file, for example, `rhods-operator-group.yaml`. 

```
apiVersion operators.coreos.com/v1
 OperatorGroup
metadata
   rhodsoperator
  namespace redhatodsoperator
```

       * Defines the operator namespace. 
    2. Create the `OperatorGroup` object in your OpenShift cluster. 

```
$ oc create -f rhods-operator-group.yaml
```

You see output similar to the following: 

```
operatorgroup.operators.coreos.com/rhods-operator created
```

  5. Create a subscription for installation of the Operator by performing the following actions: 
    1. Create a `Subscription` object CR file, for example, `rhods-operator-subscription.yaml`. 

```
apiVersion operators.coreos.com/v1alpha1
 Subscription
metadata
   rhodsoperator
  namespace redhatodsoperator

   rhodsoperator
  channel _<channel_
  source csredhatoperatorindex
  sourceNamespace openshiftmarketplace
  startingCSV rhodsoperator.x.y.z
```

       * `namespace` - Defines the operator namespace. 
       * `channel` - Sets the update channel. You must specify a value of `fast`, `fast-x.y`, `stable`, `stable-x.y` `eus-x.y`, or `alpha`. For more information, see [Understanding update channels](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/understanding-update-channels_install). 
       * `startingCSV` - Optional: Sets the operator version. If you do not specify a value, the subscription defaults to the latest operator version. For more information, see the [Red Hat OpenShift AI Self-Managed Life Cycle](https://access.redhat.com/support/policy/updates/rhoai-sm/lifecycle) Knowledgebase article. 
    2. Create the `Subscription` object in your OpenShift cluster to install the Operator. 

```
$ oc create -f rhods-operator-subscription.yaml
```

You see output similar to the following: 

```
subscription.operators.coreos.com/rhods-operator created
```



**Verification**
  * In the OpenShift web console, confirm that the Red Hat OpenShift AI Operator shows one of the following statuses: 
    * **Installing** - installation is in progress; wait for this to change to **Succeeded**. This might take several minutes. 
    * **Succeeded** - installation is successful. 
The navigation path depends on your OpenShift version: 
      * On OpenShift 4.20 and later, click **Ecosystem** **Installed Operators**. 
      * On OpenShift 4.19, click **Operators** **Installed Operators**. 


**Next step**
  * [Installing and managing Red Hat OpenShift AI components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-and-managing-openshift-ai-components_component-install)


###  [3.4.2. Installing the Red Hat OpenShift AI Operator by using the web console](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-openshift-ai-operator-using-web-console_operator-install)
Copy linkLink copied to clipboard!
The following procedure shows how to use the OpenShift web console to install the Red Hat OpenShift AI Operator on your cluster. You must install the Operator before you can install OpenShift AI components on the cluster. 
**Prerequisites**
  * You have a running OpenShift cluster, version 4.19 or greater, configured with a default storage class that can be dynamically provisioned. 
  * You have cluster administrator privileges for your OpenShift cluster. 
  * If you plan to use Llama Stack and RAG workloads, install the Red Hat OpenShift Service Mesh Operator 3.x before installing the OpenShift AI Operator. See [Installing Red Hat OpenShift Service Mesh 3.x](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/service_mesh/service-mesh-3-x#installing-ossm-v3). 
  * If you are using custom namespaces, you have created and labeled them as required. 
  * You have mirrored the required container images to a private registry. See [Mirroring images to a private registry for a disconnected installation](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#mirroring-images-to-a-private-registry-for-a-disconnected-installation_install). 


**Procedure**
  1. Log in to the OpenShift web console as a cluster administrator. 
  2. In the web console, click **Operators** **OperatorHub**. 
  3. On the **OperatorHub** page, locate the Red Hat OpenShift AI Operator by scrolling through the available Operators or by typing _Red Hat OpenShift AI_ into the **Filter by keyword** box. 
  4. Click the **Red Hat OpenShift AI** tile. The **Red Hat OpenShift AI** information pane opens. 
  5. Select a **Channel**. For information about subscription update channels, see [Understanding update channels](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/understanding-update-channels_install). 
  6. Select a **Version**. 
  7. Click **Install**. The **Install Operator** page opens. 
  8. Review or change the selected channel and version as needed. 
  9. For **Installation mode** , note that the only available value is **All namespaces on the cluster (default)**. This installation mode makes the Operator available to all namespaces in the cluster. 
  10. For **Installed Namespace** , choose one of the following options: 
     * To use the predefined operator namespace, select the **Operator recommended Namespace: redhat-ods-operator** option. 
     * To use the custom operator namespace that you created, select the **Select a Namespace** option, and then select the namespace from the drop-down list. 
  11. For **Update approval** , select one of the following update strategies: 
     * **Automatic** : Your environment attempts to install new updates when they are available based on the content of your mirror. 
     * **Manual** : A cluster administrator must approve any new updates before installation begins. 
Important
By default, the Red Hat OpenShift AI Operator follows a sequential update process. This means that if there are several versions between the current version and the target version, Operator Lifecycle Manager (OLM) upgrades the Operator to each of the intermediate versions before it upgrades it to the final, target version. 
If you configure automatic upgrades, OLM automatically upgrades the Operator to the _latest_ available version. If you configure manual upgrades, a cluster administrator must manually approve each sequential update between the current version and the final, target version. 
For information about supported versions, see the [Red Hat OpenShift AI Life Cycle](https://access.redhat.com/support/policy/updates/rhoai-sm/lifecycle) Knowledgebase article. 
  12. Click **Install**. 
The **Installing Operators** pane appears. When the installation finishes, a checkmark appears next to the Operator name. 


**Verification**
  * In the OpenShift web console, click **Operators** **Installed Operators** and confirm that the Red Hat OpenShift AI Operator shows one of the following statuses: 
    * **Installing** - installation is in progress; wait for this to change to **Succeeded**. This might take several minutes. 
    * **Succeeded** - installation is successful. 


**Next step**
  * [Installing and managing Red Hat OpenShift AI components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-and-managing-openshift-ai-components_component-install)


##  [3.5. Install and manage Red Hat OpenShift AI components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-and-managing-openshift-ai-components_component-install)
Copy linkLink copied to clipboard!
You can use the OpenShift command-line interface (CLI) or OpenShift web console to install and manage components of Red Hat OpenShift AI on your OpenShift cluster. 
###  [3.5.1. Installing Red Hat OpenShift AI components by using the CLI](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-openshift-ai-components-using-cli_component-install)
Copy linkLink copied to clipboard!
To install Red Hat OpenShift AI components by using the OpenShift CLI (`oc`), you must create and configure a `DataScienceCluster` object. 
Important
The following procedure describes how to create and configure a `DataScienceCluster` object to install Red Hat OpenShift AI components as part of a _new_ installation. 
For information about changing the installation status of OpenShift AI components _after_ installation, see [Updating the installation status of Red Hat OpenShift AI components by using the web console](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#updating-installation-status-of-openshift-ai-components-using-web-console_component-install). 
**Prerequisites**
  * The Red Hat OpenShift AI Operator is installed on your OpenShift cluster. See [Installing the Red Hat OpenShift AI Operator](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-the-openshift-ai-operator_operator-install). 
  * You have cluster administrator privileges for your OpenShift cluster. 
  * You have installed the OpenShift CLI (`oc`) as described in the appropriate documentation for your cluster: 
    * [Installing the OpenShift CLI](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/cli_tools/openshift-cli-oc#installing-openshift-cli) for OpenShift Container Platform 
    * [Installing the OpenShift CLI](https://docs.redhat.com/en/documentation/red_hat_openshift_service_on_aws/4/html/cli_tools/openshift-cli-oc#installing-openshift-cli) for Red Hat OpenShift Service on AWS 
  * If you are using custom namespaces, you have created the namespaces. 


**Procedure**
  1. Open a new terminal window. 
  2. Follow these steps to log in to your OpenShift cluster as a cluster administrator: 
    1. In the upper-right corner of the OpenShift web console, click your user name and select **Copy login command**. 
    2. After you have logged in, click **Display token**. 
    3. Copy the **Log in with this token** command and paste it in your terminal. 

```
$ oc login --token=_<token>_ --server=_<openshift_cluster_url>_
```

  3. Create a `DataScienceCluster` object custom resource (CR) file, for example, `rhods-operator-dsc.yaml`. 

```
apiVersion datasciencecluster.opendatahub.io/v2
 DataScienceCluster
metadata
   defaultdsc

  components
    aipipelines
      argoWorkflowsControllers
        managementState Removed
      managementState Removed
    dashboard
      managementState Removed
    feastoperator
      managementState Removed
    kserve
      managementState Removed
    kueue
      managementState Removed
      # Queue fields below take effect only when managementState is Unmanaged
      # autoCreateQueues: false
      # defaultClusterQueueName: default
      # defaultLocalQueueName: default
    
      managementState Removed
    mlflowoperator
      managementState Removed
    modelregistry
      managementState Removed
      registriesNamespace rhoaimodelregistries
    
      managementState Removed
    trainer
      managementState Removed
    trainingoperator
      managementState Removed
    trustyai
      managementState Removed
    workbenches
      managementState Removed
      workbenchNamespace rhodsnotebooks
```

     * `kueue` queue configuration fields: `autoCreateQueues`, `defaultClusterQueueName`, `defaultLocalQueueName`. These fields take effect only when `managementState` is set to `Unmanaged`. The `autoCreateQueues` field controls whether the Operator automatically creates default `ClusterQueue` and `LocalQueue` resources. Defaults to `false`. Set to `true` to enable Operator-managed queue creation. 
     * `argoWorkflowsControllers.managementState`: To use your own Argo Workflows instance with the `aipipelines` component, set this to `Removed`. This allows you to integrate with a managed Argo Workflows installation already on your OpenShift cluster and avoid conflicts with the embedded controller. See _Configuring pipelines with your own Argo Workflows instance_. 
     * `workbenchNamespace`: To use the predefined workbench namespace, set this value to `rhods-notebooks` or omit this line. To use a custom workbench namespace, set this value to your namespace. 
  4. In the `spec.components` section of the CR, for each OpenShift AI component shown, set the value of the `managementState` field to either `Managed` or `Removed`. These values are defined as follows:  

Managed
     The Operator actively manages the component, installs it, and tries to keep it active. The Operator will upgrade the component only if it is safe to do so.  

Removed
     The Operator actively manages the component but does not install it. If the component is already installed, the Operator will try to remove it. 
Important
     * To learn how to install the distributed workloads components, see [Installing the distributed workloads components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/installing-the-distributed-workloads-components_install). 
     * To learn how to run distributed workloads in a disconnected environment, see [Running distributed data science workloads in a disconnected environment](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/working_with_distributed_workloads/running-ray-based-distributed-workloads_distributed-workloads#running-distributed-data-science-workloads-disconnected-env_distributed-workloads). 
  5. Create the `DataScienceCluster` object in your OpenShift cluster to install the specified OpenShift AI components. 

```
$ oc create -f rhods-operator-dsc.yaml
```

You see output similar to the following: 

```
datasciencecluster.datasciencecluster.opendatahub.io/default-dsc created
```



**Verification**
  1. Confirm that there is at least one running pod for each component: 
    1. In the OpenShift web console, click **Workloads** **Pods**. 
    2. In the **Project** list at the top of the page, select `redhat-ods-applications`. 
    3. In the applications namespace, confirm that there are one or more running pods for each of the OpenShift AI components that you installed. 
  2. Confirm the status of all installed components: 
    1. Go to the **Installed Operators** page. The navigation path depends on your OpenShift version: 
       * On OpenShift 4.20 and later, click **Ecosystem** **Installed Operators**. 
       * On OpenShift 4.19, click **Operators** **Installed Operators**. 
    2. Click the **Red Hat OpenShift AI** Operator. 
    3. Click the **Data Science Cluster** tab. 
    4. For the `DataScienceCluster` object called `default-dsc`, verify that the status is `Phase: Ready`. 
Note
When you edit the `spec.components` section to change the installation status of a component, the `default-dsc` status also changes. During the initial installation, it might take a few minutes for the status phase to change from `Progressing` to `Ready`. You can access the OpenShift AI dashboard before the `default-dsc` status phase is `Ready`, but all components might not be ready. 
    5. Click the `default-dsc` link to display the data science cluster details. 
    6. Select the **YAML** tab. 
    7. In the `status.installedComponents` section, confirm that the components you installed have a status value of `true`. 
Note
If a component shows with the `component-name: {}` format in the `spec.components` section of the CR, the component is not installed. 
  3. In the OpenShift AI dashboard, users can view the list of the installed OpenShift AI components, their corresponding source (upstream) components, and the versions of the installed components, as described in [Viewing installed OpenShift AI components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#viewing-installed-components_component-install). 


**Next steps**
  * If you are using OpenStack, CodeReady Containers (CRC), or other private cloud environments without integrated external DNS, manually configure DNS A or CNAME records after the LoadBalancer IP becomes available. For more information, see [Configuring External DNS for RHOAI 3.x on OpenStack and Private Clouds](https://access.redhat.com/articles/7133770). 
  * To configure custom CPU and memory resource allocations for the OAuth proxy sidecar container in KServe inference service pods, see [Configure OAuth proxy sidecar resources for KServe](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/configuring_your_model-serving_platform_in_a_disconnected_environment/configuring-your-model-serving-platform_rhoai-admin#configuring-oauth-proxy-sidecar-resources_rhoai-admin). 
  * Complete any additional configuration required for the components you enabled. See the component-specific configuration sections for details. 


####  [3.5.1.1. Configure OAuth proxy sidecar resources for KServe](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#configuring-oauth-proxy-sidecar-resources_component-install)
Copy linkLink copied to clipboard!
To prevent out-of-memory conditions when deploying large language models (LLMs), customize the CPU and memory resource requests and limits for the OAuth proxy sidecar container in KServe inference service pods. 
Edit the DataScienceCluster custom resource (CR) to keep the KServe component in Managed state and preserve Operator reconciliation, avoiding the need to manually edit the inferenceservice-config ConfigMap. 
Important
This update affects running services and will rollout every inference service with authentication enabled. 
**Prerequisites**
  * You have cluster administrator privileges for your OpenShift cluster. 
  * The Red Hat OpenShift AI Operator is installed. 
  * A `DataScienceCluster` CR exists with the KServe component in Managed state. 
  * You have installed the OpenShift CLI (`oc`). 


**Procedure**
  1. Edit the `DataScienceCluster` CR to add custom OAuth proxy sidecar resource values. 

```
$ oc edit DataScienceCluster default-dsc
```

  2. In the `spec.components.kserve` section, add the `oauthProxy.resources` field with the values that you need. 
The following example increases the memory limit to `512Mi` and the memory request to `256Mi`: 

```
apiVersion datasciencecluster.opendatahub.io/v2
 DataScienceCluster
metadata
   defaultdsc

  components
    kserve
      managementState "Managed"
      oauthProxy
        resources
          requests
            memory "256Mi"
             "200m"
          limits
            memory "512Mi"
             "500m"
```

where:  

`oauthProxy`
     Optional. Specifies the OAuth proxy sidecar configuration for KServe inference services. If you omit the `oauthProxy` block entirely, all Operator default values are preserved. If the field is absent from older `DataScienceCluster` configurations, the Operator skips the override to maintain backward compatibility.  

`resources.requests.memory`
     Specifies the minimum memory allocated to the sidecar container. The default is `64Mi`.  

`resources.requests.cpu`
     Specifies the minimum CPU allocated to the sidecar container. The default is `100m`.  

`resources.limits.memory`
     Specifies the maximum memory that the sidecar container can use. The default is `128Mi`. Increase this value if the sidecar container runs out of memory.  

`resources.limits.cpu`
    
Specifies the maximum CPU that the sidecar container can use. The default is `200m`. 
Note
Only fields that you explicitly set override the defaults. For example, specifying only `limits.memory` changes only the memory limit while preserving the default values for `requests.memory`, `requests.cpu`, and `limits.cpu`. 
Important
Resource quantities must use valid Kubernetes resource formats, for example `64Mi`, `1Gi`, `100m`, or `200m`. The API server rejects invalid values such as non-parseable strings. 
  3. Save and close the editor. 
The Operator applies the overrides to the `inferenceservice-config` ConfigMap and automatically triggers a rollout of the `kserve-controller-manager` deployment. Existing `InferenceService` pods are rolled out with the updated resource configuration. 


**Verification**
  1. Verify that the updated values are applied to the `inferenceservice-config` ConfigMap. 

```
$ oc get configmap inferenceservice-config -n redhat-ods-applications -o jsonpath='{.data.oauthProxy}' | python3 -m json.tool
```

The output shows the updated resource values: 

```
{
    "image": "<operator-managed-image>",
    "memoryRequest": "256Mi",
    "memoryLimit": "512Mi",
    "cpuRequest": "100m",
    "cpuLimit": "200m"
}
```

  2. Confirm that the `kserve-controller-manager` deployment was rolled out. 

```
$ oc rollout status deployment/kserve-controller-manager -n redhat-ods-applications
```

The output confirms a successful rollout: 

```
deployment "kserve-controller-manager" successfully rolled out
```

  3. Verify that new `InferenceService` pods pick up the updated sidecar resource configuration by restarting a pod or deploying a new `InferenceService` pod and inspecting the sidecar container resources. 
Note
This step requires at least one `InferenceService` pod running in the target namespace. If no `InferenceService` is deployed, the command produces empty output. 
In the following command, replace `<your_namespace>` with the namespace where your `InferenceService` is deployed, for example, a data science project namespace: 

```
$ oc get pods -l serving.kserve.io/inferenceservice -n your_namespace -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{range .spec.containers[*]}  {.name}: memory={.resources.limits.memory}{"\n"}{end}{end}'
```

The output shows the sidecar container resource allocations for each `InferenceService` pod: 

```
my-model-predictor-00001-abcde
  kserve-container: memory=4Gi
  oauth-proxy: memory=512Mi
```



###  [3.5.2. Installing Red Hat OpenShift AI components by using the web console](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-openshift-ai-components-using-web-console_component-install)
Copy linkLink copied to clipboard!
To install Red Hat OpenShift AI components by using the OpenShift web console, you must create and configure a `DataScienceCluster` object. 
Important
The following procedure describes how to create and configure a `DataScienceCluster` object to install Red Hat OpenShift AI components as part of a _new_ installation. 
  * For information about changing the installation status of OpenShift AI components _after_ installation, see [Updating the installation status of Red Hat OpenShift AI components by using the web console](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#updating-installation-status-of-openshift-ai-components-using-web-console_component-install). 


**Prerequisites**
  * The Red Hat OpenShift AI Operator is installed on your OpenShift cluster. See [Installing the Red Hat OpenShift AI Operator](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-the-openshift-ai-operator_operator-install). 
  * You have cluster administrator privileges for your OpenShift cluster. 
  * If you are using custom namespaces, you have created the namespaces. 


**Procedure**
  1. Log in to the OpenShift web console as a cluster administrator. 
  2. Go to the **Installed Operators** page. The navigation path depends on your OpenShift version: 
     * On OpenShift 4.20 and later, click **Ecosystem** **Installed Operators**. 
     * On OpenShift 4.19, click **Operators** **Installed Operators**. 
  3. Click the **Red Hat OpenShift AI** Operator. 
  4. Click the **Data Science Cluster** tab. 
  5. Click **Create DataScienceCluster**. 
  6. For **Configure via** , select **YAML view**. 
An embedded YAML editor opens showing a default custom resource (CR) for the `DataScienceCluster` object, similar to the following example: 

```
apiVersion datasciencecluster.opendatahub.io/v2
 DataScienceCluster
metadata
   defaultdsc

  components
    aipipelines
      argoWorkflowsControllers
        managementState Removed
      managementState Removed
    dashboard
      managementState Removed
    feastoperator
      managementState Removed
    kserve
      managementState Removed
    kueue
      managementState Removed
      # Queue fields below take effect only when managementState is Unmanaged
      # autoCreateQueues: false
      # defaultClusterQueueName: default
      # defaultLocalQueueName: default
    mcplifecycleoperator
      managementState Removed
    mlflowoperator
      managementState Removed
    
      managementState Removed
    modelregistry
      managementState Removed
      registriesNamespace rhoaimodelregistries
    
      managementState Removed
    trainer
      managementState Removed
    trainingoperator
      managementState Removed
    trustyai
      managementState Removed
    workbenches
      managementState Removed
      workbenchNamespace rhodsnotebooks
```

     * `kueue` queue configuration fields: `autoCreateQueues`, `defaultClusterQueueName`, `defaultLocalQueueName`. These fields take effect only when `managementState` is set to `Unmanaged`. The `autoCreateQueues` field controls whether the Operator automatically creates default `ClusterQueue` and `LocalQueue` resources. Defaults to `false`. Set to `true` to enable Operator-managed queue creation. 
     * `argoWorkflowsControllers.managementState`: To use your own Argo Workflows instance with the `aipipelines` component, set this to `Removed`. This allows you to integrate with a managed Argo Workflows installation already on your OpenShift cluster and avoid conflicts with the embedded controller. See _Configuring pipelines with your own Argo Workflows instance_. 
     * `workbenchNamespace`: To use the predefined workbench namespace, set this value to `rhods-notebooks` or omit this line. To use a custom workbench namespace, set this value to your namespace. 
  7. In the `spec.components` section of the CR, for each OpenShift AI component shown, set the value of the `managementState` field to either `Managed` or `Removed`. These values are defined as follows:  

Managed
     The Operator actively manages the component, installs it, and tries to keep it active. The Operator will upgrade the component only if it is safe to do so.  

Removed
     The Operator actively manages the component but does not install it. If the component is already installed, the Operator will try to remove it. 
Important
     * To learn how to install the distributed workloads components, see [Installing the distributed workloads components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/installing-the-distributed-workloads-components_install). 
     * To learn how to run distributed workloads in a disconnected environment, see [Running distributed data science workloads in a disconnected environment](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/working_with_distributed_workloads/running-ray-based-distributed-workloads_distributed-workloads#running-distributed-data-science-workloads-disconnected-env_distributed-workloads). 
  8. Click **Create**. 


**Verification**
  1. Confirm the status of all installed components: 
    1. Go to the **Installed Operators** page. The navigation path depends on your OpenShift version: 
       * On OpenShift 4.20 and later, click **Ecosystem** **Installed Operators**. 
       * On OpenShift 4.19, click **Operators** **Installed Operators**. 
    2. Click the **Red Hat OpenShift AI** Operator. 
    3. Click the **Data Science Cluster** tab. 
    4. For the `DataScienceCluster` object called `default-dsc`, verify that the status is `Phase: Ready`. 
Note
When you edit the `spec.components` section to change the installation status of a component, the `default-dsc` status also changes. During the initial installation, it might take a few minutes for the status phase to change from `Progressing` to `Ready`. You can access the OpenShift AI dashboard before the `default-dsc` status phase is `Ready`, but all components might not be ready. 
    5. Click the `default-dsc` link to display the data science cluster details. 
    6. Select the **YAML** tab. 
    7. In the `status.installedComponents` section, confirm that the components you installed have a status value of `true`. 
Note
If a component shows with the `component-name: {}` format in the `spec.components` section of the CR, the component is not installed. 
  2. Confirm that there is at least one running pod for each component: 
    1. In the OpenShift web console, click **Workloads** **Pods**. 
    2. In the **Project** list at the top of the page, select `redhat-ods-applications` or your custom applications namespace. 
    3. In the applications namespace, confirm that there are one or more running pods for each of the OpenShift AI components that you installed. 
  3. In the OpenShift AI dashboard, users can view the list of the installed OpenShift AI components, their corresponding source (upstream) components, and the versions of the installed components, as described in [Viewing installed OpenShift AI components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#viewing-installed-components_component-install). 


**Next steps**
  * If you are using OpenStack, CodeReady Containers (CRC), or other private cloud environments without integrated external DNS, manually configure DNS A or CNAME records after the LoadBalancer IP becomes available. For more information, see [Configuring External DNS for RHOAI 3.x on OpenStack and Private Clouds](https://access.redhat.com/articles/7133770). 
  * Complete any additional configuration required for the components you enabled. See the component-specific configuration sections for details. 


###  [3.5.3. Update the installation status of Red Hat OpenShift AI components by using the web console](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#updating-installation-status-of-openshift-ai-components-using-web-console_component-install)
Copy linkLink copied to clipboard!
You can use the OpenShift web console to update the installation status of components of Red Hat OpenShift AI on your OpenShift cluster. 
**Prerequisites**
  * The Red Hat OpenShift AI Operator is [installed](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#installing-the-openshift-ai-operator_operator-install) on your OpenShift cluster. 
  * You have cluster administrator privileges for your OpenShift cluster. 


**Procedure**
  1. Log in to the OpenShift web console as a cluster administrator. 
  2. Go to the **Installed Operators** page. The navigation path depends on your OpenShift version: 
     * On OpenShift 4.20 and later, click **Ecosystem** **Installed Operators**. 
     * On OpenShift 4.19, click **Operators** **Installed Operators**. 
  3. Click the **Red Hat OpenShift AI** Operator. 
  4. Click the **Data Science Cluster** tab. 
  5. On the **DataScienceClusters** page, click the `default-dsc` object. 
  6. Click the **YAML** tab. 
An embedded YAML editor opens showing the default custom resource (CR) for the `DataScienceCluster` object, similar to the following example: 

```
apiVersion datasciencecluster.opendatahub.io/v2
 DataScienceCluster
metadata
   defaultdsc

  components
    aipipelines
      argoWorkflowsControllers
        managementState Removed
      managementState Removed
    dashboard
      managementState Removed
    feastoperator
      managementState Removed
    kserve
      managementState Removed
    kueue
      managementState Removed
      # Queue fields below take effect only when managementState is Unmanaged
      # autoCreateQueues: false
      # defaultClusterQueueName: default
      # defaultLocalQueueName: default
    
      managementState Removed
    mlflowoperator
      managementState Removed
    modelregistry
      managementState Removed
      registriesNamespace rhoaimodelregistries
    
      managementState Removed
    trainer
      managementState Removed
    trainingoperator
      managementState Removed
    trustyai
      managementState Removed
    workbenches
      managementState Removed
      workbenchNamespace rhodsnotebooks
```

  7. In the `spec.components` section of the CR, for each OpenShift AI component shown, set the value of the `managementState` field to either `Managed` or `Removed`. These values are defined as follows:  

Managed
     The Operator actively manages the component, installs it, and tries to keep it active. The Operator will upgrade the component only if it is safe to do so.  

Removed
     The Operator actively manages the component but does not install it. If the component is already installed, the Operator will try to remove it. 
Important
     * To learn how to install the distributed workloads feature, see [Installing the distributed workloads components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/installing-the-distributed-workloads-components_install). 
     * To learn how to run distributed workloads in a disconnected environment, see [Running distributed data science workloads in a disconnected environment](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/working_with_distributed_workloads/running-ray-based-distributed-workloads_distributed-workloads#running-distributed-data-science-workloads-disconnected-env_distributed-workloads). 
  8. Click **Save**. 
For any components that you updated, OpenShift AI initiates a rollout that affects all pods to use the updated image. 


**Verification**
  1. Confirm that there is at least one running pod for each component: 
    1. In the OpenShift web console, click **Workloads** **Pods**. 
    2. In the **Project** list at the top of the page, select `redhat-ods-applications` or your custom applications namespace. 
    3. In the applications namespace, confirm that there are one or more running pods for each of the OpenShift AI components that you installed. 
  2. Confirm the status of all installed components: 
  3. In the OpenShift web console, go to the **Installed Operators** page. The navigation path depends on your OpenShift version: 
     * On OpenShift 4.20 and later, click **Ecosystem** **Installed Operators**. 
     * On OpenShift 4.19, click **Operators** **Installed Operators**. 
    1. Click the **Red Hat OpenShift AI** Operator. 
    2. Click the **Data Science Cluster** tab and select the `DataScienceCluster` object called `default-dsc`. 
    3. Select the **YAML** tab. 
    4. In the `status.installedComponents` section, confirm that the components you installed have a status value of `true`. 
Note
If a component shows with the `component-name: {}` format in the `spec.components` section of the CR, the component is not installed. 
  4. In the OpenShift AI dashboard, users can view the list of the installed OpenShift AI components, their corresponding source (upstream) components, and the versions of the installed components, as described in [Viewing installed OpenShift AI components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#viewing-installed-components_component-install). 


###  [3.5.4. View installed OpenShift AI components](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.5/html/installing_and_uninstalling_openshift_ai_self-managed_in_a_disconnected_environment/deploying-openshift-ai-in-a-disconnected-environment_install#viewing-installed-components_component-install)
Copy linkLink copied to clipboard!
In the Red Hat OpenShift AI dashboard, you can view a list of the installed OpenShift AI components, their corresponding source (upstream) components, and the versions of the installed components. 
**Prerequisites**
  * OpenShift AI is installed in your OpenShift cluster. 


**Procedure**
  1. Log in to the OpenShift AI dashboard. 
  2. In the top navigation bar, click the help icon ( ) and then select **About**. 


**Verification**
The **About** page shows a list of the installed OpenShift AI components along with their corresponding upstream components and upstream component versions. 
### Learn
  * [Developer resources](https://developers.redhat.com/learn)
  * [Cloud learning hub](https://docs.redhat.com/learn/learning-paths)
  * [Training and certification](https://www.redhat.com/services/training-and-certification)
  * [Customer support](https://access.redhat.com/support)
  * [See all documentation](https://docs.redhat.com/en/products)


### Try, buy, & sell
  * [Product trial center](https://redhat.com/en/products/trials)
  * [Red Hat Ecosystem Catalog](https://catalog.redhat.com/)
  * [Buy online (Japan)](https://www.redhat.com/about/japan-buy)


### Communities
  * [Customer Portal Community](https://access.redhat.com/community)


### About Red Hat
We deliver hardened solutions that make it easier for enterprises to work across platforms and environments, from the core datacenter to the network edge.
### Making open source more inclusive
Red Hat is committed to replacing problematic language in our code, documentation, and web properties. For more details, see the [Red Hat Blog](https://www.redhat.com/en/blog/making-open-source-more-inclusive-eradicating-problematic-language). 
### About Red Hat Documentation
We help Red Hat users innovate and achieve their goals with our products and services with content they can trust. [Explore our recent updates](https://www.redhat.com/en/blog/whats-new-docsredhatcom). 
### Theme
System default 
### Red Hat legal and privacy links
  * [Inclusion at Red Hat](https://redhat.com/en/about/our-culture/diversity-equity-inclusion)
  * [Cool Stuff Store](https://coolstuff.redhat.com/)

© 2026 Red Hat
### Red Hat legal and privacy links
  * [Privacy statement](https://redhat.com/en/about/privacy-policy)
  * [All policies and guidelines](https://redhat.com/en/about/all-policies-guidelines)
  * [Digital accessibility](https://redhat.com/en/about/digital-accessibility)
  * Cookie Preferences and Opt-Out Rights

Back to top  
| Red Hat OpenShift AI version  | Operator (Unsupported, Exception Required)  |  
| --- | --- |  
|  Authorino Operator, Service Mesh Operator, Serverless Operator   |  
|  Job-set-operator, openshift-custom-metrics-autoscaler-operator, cert-manager Operator, Leader Worker Set Operator, Red Hat Connectivity Link Operator, Kueue Operator (RHBOK), SR-IOV Operator, GPU Operator (with custom configurations), OpenTelemetry, Tempo, Cluster Observability Operator, IBM Spyre Operator.   |
