---
title: Deploy the standalone Red Hat AI Inference container in a disconnected environment
  | Red Hat AI Inference | 3.4 | Red Hat Documentation
id: deploy-the-standalone-red-hat-ai-inference-container-in-a-disconnected-environme
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- vendor-docs
- reliability-high
created: '2026-10-23T17:24:02.154840Z'
updated: '2026-10-23T17:25:06.433429Z'
source: https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index
source_domain: docs.redhat.com
fetched_at: '2026-10-23T17:24:02.128234Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Red Hat primary doc (3.4): disconnected deployment via bastion host + mirror
  registry (oc-mirror v2), mirroring operator, NFD/NVIDIA GPU Operator and OCI ''modelcar''
  model images (registry.redhat.io/rhelai1, 10-100 GB each). Only NVIDIA CUDA supported
  in disconnected OpenShift at this release. Alternative: models on NFS/PVs for custom/fine-tuned
  models, transferred separately. Vendor docs; reliable for mechanics, silent on failure
  modes.'
---

*Suggested by [[red-hat-ai-inference-34-red-hat-documentation]] — disconnected deploy guide*

  1. [Red Hat AI Inference](https://docs.redhat.com/en/documentation/red_hat_ai_inference)
  2. Deploy the standalone Red Hat AI Inference container in a disconnected environment 


Ask AI
# Deploy the standalone Red Hat AI Inference container in a disconnected environment
Red Hat AI Inference 3.4
## Deploy Red Hat AI Inference in a disconnected environment using OpenShift Container Platform and a disconnected mirror image registry
Red Hat AI Documentation Team
**Abstract**
Learn how to work with Red Hat AI Inference for model serving and inferencing in a disconnected environment. 
##  [Preface](https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index#idm140585225161616)
Copy linkLink copied to clipboard!
You can deploy the standalone Red Hat AI Inference container in a disconnected OpenShift Container Platform environment that does not have direct access to the internet by mirroring Operator and OCI model container images to a local mirror registry and configuring the cluster to use the mirrored images. 
Important
Currently, only NVIDIA CUDA AI accelerators are supported for OpenShift Container Platform in disconnected environments. 
After mirroring the required images, you can install the Node Feature Discovery Operator and NVIDIA GPU Operator from the mirrored sources, then deploy Red Hat AI Inference for inference serving the OCI-compliant model. 
##  [Chapter 1. Storing models in disconnected environments](https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index#storing-models-in-disconnected-environments_disconnected-deploy)
Copy linkLink copied to clipboard!
You can store language models in disconnected environments using OCI-compliant model container images or persistent storage volumes. Each approach has different tradeoffs for deployment complexity, storage efficiency, and operational workflows. 
Important
Use OCI model images when you want to use Red Hat validated models and prefer a unified container-based workflow. 
Use persistent storage when you need to deploy custom models, fine-tuned models, or models not available as OCI images.  

OCI model container images
    
OCI-compliant model container images, also known as _modelcars_ , package language models as container images that you can store in your mirror registry alongside other container images. This approach integrates with existing container image workflows and infrastructure: 
  * Uses the same mirroring workflow as other container images 
  * Leverages existing container registry infrastructure for versioning and distribution 
  * Enables faster pod startup through image caching on nodes 
  * Simplifies model updates through standard image pull mechanisms 


Note
OCI model container images require additional registry storage capacity for large model images. Model images can be 10-100 GB depending on model size and applied quantization. 
Red Hat provides validated OCI model images in the `registry.redhat.io/rhelai1` namespace that you can mirror to your disconnected registry.  

Persistent storage volumes
    
You can store model files directly with persistent storage such as Network File System (NFS) volumes or other persistent volume types supported by OpenShift Container Platform. This approach requires transferring model files to the disconnected environment separately from container images. You can share a single copy of a model across multiple inference pods with the same persistent storage volume. You can store models downloaded from Hugging Face or other sources, or you can store custom or fine-tuned models not available as OCI images. 
Persistent storage volumes require separate transfer and setup workflow for model files, with appropriate storage provisioning and access configuration. 
##  [Chapter 2. Setting up a mirror registry for your disconnected environment](https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index#setting-up-mirror-registry_disconnected-deploy)
Copy linkLink copied to clipboard!
To serve container images in a disconnected environment, you must configure a disconnected mirror registry on a bastion host. The bastion host acts as a secure gateway between your disconnected environment and the internet. You then mirror images from Red Hat’s online image registries, and serve them in the disconnected environment. 
**Prerequisites**
  * You have deployed the bastion host. 
  * You have [installed the `oc` CLI](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/cli_tools/index#installing-openshift-cli) in the bastion host. 
  * You have [installed Podman](https://podman.io/docs/installation) in the bastion host. 
  * You have [installed OpenShift Container Platform](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html/disconnected_environments/installing-disconnected-environments) in the disconnected environment. 


**Procedure**
  1. Open a shell prompt on the bastion host and [create the disconnected mirror registry](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/disconnected_environments/index#installing-mirroring-creating-registry). 
  2. [Configure credentials that allow images to be mirrored](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/disconnected_environments/index#installation-adding-registry-pull-secret_about-installing-oc-mirror-v2). 


##  [Chapter 3. Mirror the required images for model inference](https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index#mirroring-operator-images_disconnected-deploy)
Copy linkLink copied to clipboard!
After you create a mirror registry for the disconnected environment, you can mirror the required AI Inference image, AI accelerator Operator images, and OCI model container image. 
**Prerequisites**
  * You have installed the OpenShift CLI (`oc`). 
  * You have logged in as a user with `cluster-admin` privileges. 
  * You have installed a mirror registry on the bastion host. 
  * You have installed the [oc-mirror OpenShift CLI plugin](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/disconnected_environments/index#about-installing-oc-mirror-v2). 
  * You have configured credentials for the mirror registry and source registries in the default Podman authentication path. 


**Procedure**
  1. Identify the operator catalog index images for the following Operators: 
     * [Node Feature Discovery (NFD) Operator](https://catalog.redhat.com/search?gs&q=Node+Feature+Discovery+%28NFD%29+Operator&searchType=all) — available from the `redhat-operator-index` catalog 
     * [NVIDIA GPU Operator](https://catalog.redhat.com/search?gs&q=gpu%20operator) — available from the `certified-operator-index` catalog 
  2. Select an OCI model container image, for example `registry.redhat.io/rhelai1/granite-3-1-8b-instruct-quantized-w8a8:1.5`. 
Note
You can select any OCI model container image from the validated models list that matches your requirements. For available options, see [Validated models for AI Inference](https://docs.redhat.com/en/documentation/red_hat_ai/3/html/validated_models/index). 
  3. [Create an image set configuration custom resource (CR)](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/disconnected_environments/index#oc-mirror-building-image-set-config-v2_about-installing-oc-mirror-v2) that includes the NFD Operator, NVIDIA GPU Operator, AI Inference image, and the OCI model image. For example, save the following `ImageSetConfiguration` CR as the file `imageset-config.yaml`: 

```
apiVersion mirror.openshift.io/v2alpha1
 ImageSetConfiguration
mirror
  operators
  # Node Feature Discovery (NFD) Operator
  # Enables OpenShift to detect hardware capabilities such as GPUs
   catalog registry.redhat.io/redhat/redhatoperatorindexv4.20
    packages
        nfd

  # GPU Operator
  # Manages NVIDIA GPUs on OpenShift
   catalog registry.redhat.io/redhat/certifiedoperatorindexv4.20
    packages
        gpuoperatorcertified
  additionalImages
  # Red Hat AI Inference image
    registry.redhat.io/rhaii/vllmcudarhel93.4.0
  # Model image
    registry.redhat.io/rhelai1/granite318binstructquantizedw8a8
```

If you selected a different model in step 2, replace the model image path in the `additionalImages` section. 
Note
The catalog image tag, for example `v4.20`, must match the major release version of your target OpenShift Container Platform cluster. 
Note
If you have already installed the NFD and NVIDIA GPU Operators in the cluster, you can omit the `operators` section and mirror only the AI Inference and OCI model container images: 

```
apiVersion mirror.openshift.io/v2alpha1
 ImageSetConfiguration
mirror
  additionalImages
    registry.redhat.io/rhaiiregistrynamespace/vllmcudarhel9rhaiisversion
    registry.redhat.io/rhelai1/granite318binstructquantizedw8a8
```

Important
If your deployment also requires the Red Hat OpenShift AI Operator, add `rhods-operator` as an additional package under the `redhat-operator-index` catalog entry. The `rhods-operator` is about 275 GB in size. Plan for adequate storage and data transfer capacity in your disconnected environment. 
  4. Mirror the required images into the mirror registry. 
In a partially disconnected environment, run the following command to mirror directly from the source registries to the mirror registry: 

```
$ oc mirror -c imageset-config.yaml --workspace file://workspace_path docker://target_mirror_registry_url --v2
```


`<workspace_path>`
     The local directory where `oc-mirror` stores metadata and generated cluster resources.  

`<target_mirror_registry_url>`
    
The URL of your mirror registry, for example `mirror.example.com:5000`. 
In a fully disconnected environment, use a two-step process to mirror images to disk first and then transfer them to the mirror registry. For more information, see [Mirroring an image set in a fully disconnected environment](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/disconnected_environments/index#oc-mirror-workflows-fully-disconnected-v2_about-installing-oc-mirror-v2). 
  5. Verify that the mirroring operation completed successfully by listing the generated cluster resources: 

```
$ ls workspace_path/working-dir/cluster-resources/
```

The output should include `ImageContentSourcePolicy` or `ImageDigestMirrorSet` YAML files for configuring the cluster. 
  6. [Configure the cluster for the mirror registry](https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/disconnected_environments/index#connected-to-disconnected-config-registry_connected-to-disconnected). 


##  [Chapter 4. Installing the Node Feature Discovery Operator](https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index#installing-nfd-operator_disconnected-deploy)
Copy linkLink copied to clipboard!
Install the Node Feature Discovery Operator so that the cluster can use the AI accelerators that are available in the cluster. 
**Prerequisites**
  * You have installed the OpenShift CLI (`oc`). 
  * You have logged in as a user with `cluster-admin` privileges. 


**Procedure**
  1. Create the `Namespace` CR for the Node Feature Discovery Operator: 

```
oc apply f  <<EOF
apiVersion v1
 Namespace
metadata
   openshiftnfd
  labels
     openshiftnfd
    openshift.io/cluster-monitoring "true"
EOF
```

  2. Create the `OperatorGroup` CR: 

```
oc apply f  <<EOF
apiVersion operators.coreos.com/v1
 OperatorGroup
metadata
  generateName openshiftnfd
   openshiftnfd
  namespace openshiftnfd

  targetNamespaces
   openshiftnfd
EOF
```

  3. Create the `Subscription` CR: 

```
oc apply f  <<EOF
apiVersion operators.coreos.com/v1alpha1
 Subscription
metadata
   nfd
  namespace openshiftnfd

  channel "stable"
  installPlanApproval Automatic
   nfd
  source redhatoperators
  sourceNamespace openshiftmarketplace
EOF
```



**Verification**
Verify that the Node Feature Discovery Operator deployment is successful by running the following command: 

```
$ oc get pods -n openshift-nfd
```

**Example output**

```
NAME                                      READY   STATUS    RESTARTS   AGE
nfd-controller-manager-7f86ccfb58-vgr4x   2/2     Running   0          10m
```

##  [Chapter 5. Installing the NVIDIA GPU Operator](https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index#installing-nvidia-gpu-operator_disconnected-deploy)
Copy linkLink copied to clipboard!
Install the NVIDIA GPU Operator to use the underlying NVIDIA CUDA AI accelerators that are available in the cluster. 
**Prerequisites**
  * You have installed the OpenShift CLI (`oc`). 
  * You have logged in as a user with `cluster-admin` privileges. 
  * You have installed the Node Feature Discovery Operator. 


**Procedure**
  1. Create the `Namespace` CR for the NVIDIA GPU Operator: 

```
oc apply f  <<EOF
apiVersion v1
 Namespace
metadata
   nvidiagpuoperator
EOF
```

  2. Create the `OperatorGroup` CR: 

```
oc apply f  <<EOF
apiVersion operators.coreos.com/v1
 OperatorGroup
metadata
   gpuoperatorcertified
  namespace nvidiagpuoperator

 targetNamespaces
  nvidiagpuoperator
EOF
```

  3. Create the `Subscription` CR: 

```
oc apply f  <<EOF
apiVersion operators.coreos.com/v1alpha1
 Subscription
metadata
   gpuoperatorcertified
  namespace nvidiagpuoperator

  channel "stable"
  installPlanApproval Manual
   gpuoperatorcertified
  source certifiedoperators
  sourceNamespace openshiftmarketplace
EOF
```



**Verification**
Verify that the NVIDIA GPU Operator deployment is successful by running the following command: 

```
$ oc get pods -n nvidia-gpu-operator
```

**Example output**

```
NAME                                                  READY   STATUS     RESTARTS   AGE
gpu-feature-discovery-c2rfm                           1/1     Running    0          6m28s
gpu-operator-84b7f5bcb9-vqds7                         1/1     Running    0          39m
nvidia-container-toolkit-daemonset-pgcrf              1/1     Running    0          6m28s
nvidia-cuda-validator-p8gv2                           0/1     Completed  0          99s
nvidia-dcgm-exporter-kv6k8                            1/1     Running    0          6m28s
nvidia-dcgm-tpsps                                     1/1     Running    0          6m28s
nvidia-device-plugin-daemonset-gbn55                  1/1     Running    0          6m28s
nvidia-device-plugin-validator-z7ltr                  0/1     Completed  0          82s
nvidia-driver-daemonset-410.84.202203290245-0-xxgdv   2/2     Running    0          6m28s
nvidia-node-status-exporter-snmsm                     1/1     Running    0          6m28s
nvidia-operator-validator-6pfk6                       1/1     Running    0          6m28s
```

##  [Chapter 6. Inference serving the model in the disconnected environment](https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index#inference-serving-the-model-disconnected-environment_disconnected-deploy)
Copy linkLink copied to clipboard!
Use Red Hat AI Inference deployed in a disconnected OpenShift Container Platform environment to inference serve large language models with Red Hat AI Inference without any connection to the outside internet by installing OpenShift Container Platform and configuring a mirrored container image registry in the disconnected environment. 
Important
Currently, only NVIDIA CUDA AI accelerators are supported for OpenShift Container Platform in disconnected environments. 
Note
This procedure uses OCI model images mirrored to your disconnected registry. Alternatively, you can download model files from Hugging Face, transfer them to persistent storage in your disconnected cluster, and mount the storage in your deployment. 
Disconnected deployments require setting up a mirror registry to host container images and operator catalogs that would normally be pulled from internet-accessible registries. After mirroring the required images, you can install the Node Feature Discovery Operator and NVIDIA GPU Operator from the mirrored sources, then deploy Red Hat AI Inference for inference serving. 
**Prerequisites**
  * You have installed a mirror registry on the bastion host that is accessible to the disconnected cluster. 
  * You have mirrored the Red Hat AI Inference image and OCI model images to your mirror registry. 
  * You have installed the Node Feature Discovery Operator and NVIDIA GPU Operator in the disconnected cluster. 


**Procedure**
  1. Create a namespace for the AI Inference deployment: 

```
$ oc create namespace rhaii-namespace
```

  2. Create the `Deployment` CR using an init container to load the model from the mirrored OCI image: 

```
oc apply f  <<EOF
apiVersion apps/v1
 Deployment
metadata
   granite
  namespace rhaiinamespace
  labels
     granite

  replicas 
  selector
    matchLabels
       granite
  template
    metadata
      labels
         granite
    
      initContainers
          modelloader
          image '<MIRROR_REGISTRY_URL>/rhelai1/granite-3-1-8b-instruct-quantized-w8a8:1.5'
          command   '/models/.' '/mnt/models/'
          volumeMounts
              modelvolume
              mountPath /mnt/models
      containers
          granite
          image '<MIRROR_REGISTRY_URL>/rhaii/vllm-cuda-rhel9:latest'
          imagePullPolicy IfNotPresent
          command
             python
             
             vllm.entrypoints.openai.api_server
          
             '--port=8000'
             '--model=/mnt/models'
             '--served-model-name=granite-3.1-8b-instruct-quantized-w8a8'
             '--tensor-parallel-size=1'
          resources
            limits
               
              nvidia.com/gpu 
            requests
               
              memory 6Gi
              nvidia.com/gpu 
          volumeMounts
              modelvolume
              mountPath /mnt/models
              shm
              mountPath /dev/shm
      volumes
          modelvolume
          emptyDir 
          shm
          emptyDir
            medium Memory
            sizeLimit 2Gi
      restartPolicy Always
EOF
```

     * `<MIRROR_REGISTRY_URL>`: Replace with the URL of your mirror registry. The init container copies model files from the OCI image to a shared volume before the inference server starts. 
     * `mountPath: /dev/shm`: Mounts the shared memory volume required by the NVIDIA Collective Communications Library (NCCL). Tensor parallel deployments fail without this volume mount. 
  3. Create a `Service` CR for the model inference: 

```
oc apply f  <<EOF
apiVersion v1
 Service
metadata
   granite
  namespace rhaiinamespace

  selector
     granite
  ports
     protocol TCP
       
      targetPort 
EOF
```

  4. Optional: Create a `Route` CR to enable access to the model from outside the cluster: 

```
oc apply f  <<EOF
apiVersion route.openshift.io/v1
 Route
metadata
   granite
  namespace rhaiinamespace

  
     Service
     granite
  
    targetPort 
EOF
```



**Verification**
  1. Get the URL for the exposed route: 

```
$ oc get route granite -n rhaii-namespace -o jsonpath='{.spec.host}'
```

**Example output**

```
granite-rhaii-namespace.apps.example.com
```

  2. Query the model to verify the deployment: 

```
$ curl -X POST http://granite-rhaii-namespace.apps.example.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "granite-3.1-8b-instruct-quantized-w8a8",
    "messages": [{"role": "user", "content": "What is AI?"}],
    "temperature": 0.1
  }'
```

The model returns an answer in a valid JSON response. 


##  [Legal Notice](https://docs.redhat.com/en/documentation/red_hat_ai_inference/3.4/html-single/deploy_the_standalone_red_hat_ai_inference_container_in_a_disconnected_environment/index#idm140585229856832)
Copy linkLink copied to clipboard!
Copyright © Red Hat. 
Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version. 
Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law. 
Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries. 
Linux® is the registered trademark of Linus Torvalds in the United States and other countries. 
XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries. 
The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license. 
All other trademarks are the property of their respective owners. 
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
