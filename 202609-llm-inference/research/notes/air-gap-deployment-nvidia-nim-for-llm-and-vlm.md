---
title: Air-Gap Deployment — NVIDIA NIM for LLM and VLM
id: air-gap-deployment-nvidia-nim-for-llm-and-vlm
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- vendor-docs
- reliability-high
created: '2026-10-23T17:23:07.923016Z'
updated: '2026-10-23T17:25:08.132963Z'
source: https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html
source_domain: docs.nvidia.com
fetched_at: '2026-10-23T17:23:07.901268Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'NVIDIA primary docs (NIM LLM 2.1.4): two-phase air-gap workflow; download-to-cache
  -p <profile hash>, create-model-store, model-specific vs model-free NIM (NIM_MODEL_PATH
  local path = no download), do NOT set NGC_API_KEY/HF_TOKEN in air-gapped phase;
  Helm chart image mirroring incl. init containers, GPU Operator; airgap-bundle.sh
  helper scripts. Reliable for mechanics.'
---

*Suggested by [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]] — cited as primary source*

[Skip to main content](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#main-content)
Back to top
[ NVIDIA NIM for LLM and VLM ](https://docs.nvidia.com/nim/large-language-models/latest/index.html)
2.1.4
[2.1.4](https://docs.nvidia.com/nim/large-language-models/2.1.4/deployment/air-gap-deployment.html)[2.0.13](https://docs.nvidia.com/nim/large-language-models/2.0.13/deployment/air-gap-deployment.html)[2.0.12](https://docs.nvidia.com/nim/large-language-models/2.0.12/deployment/air-gap-deployment.html)[2.0.11](https://docs.nvidia.com/nim/large-language-models/2.0.11/deployment/air-gap-deployment.html)[2.0.10](https://docs.nvidia.com/nim/large-language-models/2.0.10/deployment/air-gap-deployment.html)[2.0.9](https://docs.nvidia.com/nim/large-language-models/2.0.9/deployment/air-gap-deployment.html)[2.0.8](https://docs.nvidia.com/nim/large-language-models/2.0.8/deployment/air-gap-deployment.html)[2.0.7](https://docs.nvidia.com/nim/large-language-models/2.0.7/deployment/air-gap-deployment.html)[2.0.6](https://docs.nvidia.com/nim/large-language-models/2.0.6/deployment/air-gap-deployment.html)[2.0.5](https://docs.nvidia.com/nim/large-language-models/2.0.5/deployment/air-gap-deployment.html)[2.0.4-pb6](https://docs.nvidia.com/nim/large-language-models/2.0.4-pb6/deployment/air-gap-deployment.html)[2.0.4](https://docs.nvidia.com/nim/large-language-models/2.0.4/deployment/air-gap-deployment.html)[2.0.3](https://docs.nvidia.com/nim/large-language-models/2.0.3/deployment/air-gap-deployment.html)[2.0.2](https://docs.nvidia.com/nim/large-language-models/2.0.2/deployment/air-gap-deployment.html)[2.0.1](https://docs.nvidia.com/nim/large-language-models/2.0.1/deployment/air-gap-deployment.html)[1.15.0](https://docs.nvidia.com/nim/large-language-models/1.15.0/deployment/air-gap-deployment.html)[1.14.0](https://docs.nvidia.com/nim/large-language-models/1.14.0/deployment/air-gap-deployment.html)[1.13.0](https://docs.nvidia.com/nim/large-language-models/1.13.0/deployment/air-gap-deployment.html)[1.12.0](https://docs.nvidia.com/nim/large-language-models/1.12.0/deployment/air-gap-deployment.html)[1.11.0](https://docs.nvidia.com/nim/large-language-models/1.11.0/deployment/air-gap-deployment.html)[1.10.0](https://docs.nvidia.com/nim/large-language-models/1.10.0/deployment/air-gap-deployment.html)[1.8.0](https://docs.nvidia.com/nim/large-language-models/1.8.0/deployment/air-gap-deployment.html)[1.7.0](https://docs.nvidia.com/nim/large-language-models/1.7.0/deployment/air-gap-deployment.html)[Archive](https://docs.nvidia.com/nim/large-language-models/latest/reference/archived-versions.htmldeployment/air-gap-deployment.html)


Light Dark System Settings
# Air-Gap Deployment[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#air-gap-deployment "Link to this heading")
Air-gap deployment lets you run a NIM without an internet connection, for example, with no connection to remote model registries such as NGC or Hugging Face Hub. For a product overview, refer to [Overview](https://docs.nvidia.com/nim/large-language-models/latest/about-nim-llm/overview.html).
This guide describes how to prepare, transfer, and run NIM LLM and VLM in an air-gapped environment with either vLLM or SGLang. Use the same container image and model in both phases so the cached profiles match your deployment.
Air-gap deployment follows a two-phase workflow:
  1. **Network-connected phase** : On a machine with internet access and credentials, download model assets and prepare them for transfer.
  2. **Air-gapped phase** : On the isolated machine, mount the pre-staged model assets and run the NIM container with no outbound network access and no API keys.


Transfer the model assets between these phases using any allowed channel, such as an archive copy, `scp`, `rsync`, or physical media.
Important
In the air-gapped phase, do **not** set `NGC_API_KEY` or `HF_TOKEN`. The NIM must load all model assets from local storage only.
## Prerequisites[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#prerequisites "Link to this heading")
Before you start, complete the following prerequisites:
  1. On the network-connected host, install Docker and obtain credentials for the model source you will pre-stage (`NGC_API_KEY` or `HF_TOKEN` as required).
  2. Decide whether you will use a model-specific image or a model-free image, as described in [Choose a Deployment Modality](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#choose-a-deployment-modality).
  3. On the air-gapped host, provide a place to mount the transferred cache, model store, or local model directory. Do not set `NGC_API_KEY` or `HF_TOKEN` in that phase.


## Choose a Deployment Modality[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#choose-a-deployment-modality "Link to this heading")
NIM LLM and VLM supports two deployment modalities. Your choice affects how you prepare model assets for an air-gapped deployment.
### Model-Specific NIM[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#model-specific-nim "Link to this heading")
For model-specific NIM, keep the following in mind:
  * The NIM container ships with a model manifest that defines the model and one or more profiles (tensor parallel size, backend, and so on).
  * Use a model-specific container image for this workflow, such as `${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4`.
  * Model weights are **not** shipped inside the container. At startup, NIM loads the model from cache if present, or downloads it when the system has network and credentials.
  * For air-gap, you must pre-stage the model using `download-to-cache` (and optionally `create-model-store`) during the connected phase, then transfer the cache or model store to the air-gapped system.


### Model-Free NIM[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#model-free-nim "Link to this heading")
For model-free NIM, keep the following in mind:
  * The model is **configured at runtime** by the user, using `NIM_MODEL_PATH` or the positional model argument on the container command line (refer to Model Path as a Container Argument in [Model-Free NIM](https://docs.nvidia.com/nim/large-language-models/latest/deployment/model-free-nim.html#model-free-nim)). The container does not ship a fixed model manifest.
  * Use a model-free container image for this workflow, such as `${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4`.
  * When `NIM_MODEL_PATH` points to a **local path** (for example, `/mnt/models/my-llama`), NIM serves directly from that path. **No download occurs** , so if the model directory is already present on the air-gapped system, you do not need to use `download-to-cache` or `create-model-store`.
  * When `NIM_MODEL_PATH` is a **remote URI** (for example, `ngc://org/team/model:tag`), NIM generates a runtime manifest and saves a copy in `NIM_CACHE_PATH`. If manifest generation fails during an offline restart, NIM falls back to the cached manifest. Mount the cache prepared for the same model and container image, including all required model assets.


Note
Set `${NIM_LLM_MODEL_FREE_IMAGE}` or `${NIM_LLM_MODEL_SPECIFIC_IMAGE}` to the image for your model and inference engine listed in the [Support Matrix](https://docs.nvidia.com/nim/large-language-models/latest/reference/support-matrix.html#support-matrix).
### Quick Reference[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#quick-reference "Link to this heading")
The following table summarizes the model source location at runtime, the container image to use, and how the model is accessed:  
| NIM Type  | Container Image  | `NIM_MODEL_PATH` value  | Download at Startup?  | Air-Gap support  |  
| --- | --- | --- | --- | --- |  
| Regular NIM  | `${NIM_LLM_MODEL_SPECIFIC_IMAGE}`  | _(not used)_  | Yes, if not in cache  | **Yes** , pre-stage cache or model store  |  
| Model-free NIM  | `${NIM_LLM_MODEL_FREE_IMAGE}`  | Local path (for example, `/mnt/model`)  | No  | **Yes** , ensure local path is mounted  |  
| Model-free NIM  | `${NIM_LLM_MODEL_FREE_IMAGE}`  | Remote URI (for example, `ngc://...`), first deploy  | Yes, generates manifest  | **Requires network** on first deploy  |  
| Model-free NIM  | `${NIM_LLM_MODEL_FREE_IMAGE}`  | Remote URI (for example, `ngc://...`), redeploy with manifest on PVC  | No  | **Yes** , cached manifest and model assets required  |  
## Download Model Assets (Network-Connected Phase)[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#download-model-assets-network-connected-phase "Link to this heading")
On a machine with internet access, use the NIM container to download model assets and optionally create a model store directory.
To download model assets on a network-connected host, complete the following steps.
Use a container image that matches your workflow:
  * For model-specific NIM, use `${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4`.
  * For model-free NIM, use `${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4`.


### Download a Model to Cache[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#download-a-model-to-cache "Link to this heading")
The `download-to-cache` command downloads selected or default model profiles to the NIM cache.
  1. Run `download-to-cache` for the profile you need:

```
dockerrun-it--rm--gpusall\
-eNGC_API_KEY\
-v"$LOCAL_NIM_CACHE:/opt/nim/.cache"\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4\
download-to-cache-p<PROFILE_HASH>

```
Copy to clipboard
You can also download all profiles with `--all`, or let NIM auto-select a profile based on the available hardware by omitting the flag.


Tip
Run `list-model-profiles` to discover available profiles and their hashes before downloading.
Use a profile ID or description from `list-model-profiles` in the image you plan to deploy. Profile descriptions identify the inference engine with a `vllm-` or `sglang-` prefix.
### Create a Model Store[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#create-a-model-store "Link to this heading")
The `create-model-store` command extracts files from a cached model profile and creates a properly formatted model directory. If the profile is not already cached, the command downloads it first.

```
dockerrun-it--rm--gpusall\
-eNGC_API_KEY\
-v"$LOCAL_NIM_CACHE:/opt/nim/.cache"\
-v"$MODEL_REPO:/model-repo"\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4\
create-model-store-p<PROFILE_HASH>-m/model-repo

```
Copy to clipboard
The resulting directory is ready for transfer to the air-gapped system.
## Prepare Container Images[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#prepare-container-images "Link to this heading")
For Kubernetes deployments, every image referenced by the downloaded Helm chart must be available from a registry that the air-gapped cluster can reach. At a minimum, mirror the NIM image. Also mirror any images used by enabled init containers, extra init containers, sidecars, Helm tests, or platform components such as the GPU Operator. For example, when `initContainers.ngcInit` is enabled, the chart adds an `ngc-model-puller` init container that uses `buildpack-deps:bookworm` by default. Each user-defined `initContainers.extraInit` entry can reference an additional image.
Use the network-connected machine to pull and archive the images. The provided helper script packages the Helm chart, records the image list, optionally copies values and manifests, saves images, and creates a transfer archive. Because the documentation site does not distribute executable shell files, copy `airgap-bundle.sh` and `airgap-load-images.sh` from [Air-Gap Helper Scripts](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-helper-scripts.html) into the same local directory first:

```
exportAIRGAP_TOOLS_DIR=/path/to/airgap-tools
exportNIM_LLM_CHART_DIR=/path/to/unpacked/nim-llm-chart

"$AIRGAP_TOOLS_DIR/airgap-bundle.sh"\
--chart-dir"$NIM_LLM_CHART_DIR"\
--image"${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4"\
--values/path/to/airgap-values.yaml\
--manifest/path/to/deny-egress.yaml\
--output-dir./nim-airgap-bundle

```
Copy to clipboard
The script copies each values file into `$AIRGAP_BUNDLE/values/` using its original filename. Note the filename you pass so you can reference it during installation.
You can repeat `--image`, `--values`, and `--manifest`, or pass an image list with `--images-file`. Add images for enabled init containers, extra init containers, sidecars, Helm tests, and platform components such as the GPU Operator.
The following commands show the equivalent manual image steps:

```
exportNIM_IMAGE="${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4"
exportAIRGAP_BUNDLE="${AIRGAP_BUNDLE:-$(pwd)/nim-airgap-bundle}"
mkdir-p"$AIRGAP_BUNDLE/images"

cat>"$AIRGAP_BUNDLE/images/images.txt"<<EOF
$NIM_IMAGE
EOF

# Add these only if your values enable them:
# buildpack-deps:bookworm        # initContainers.ngcInit default image
# curlimages/curl:8.6.0          # helm test hooks
# <images from initContainers.extraInit>
# <platform images, for example GPU Operator images>

whileIFS=read-rimage;do
[-n"$image"]||continue
dockerpull"$image"
done<"$AIRGAP_BUNDLE/images/images.txt"

dockersave\
--output"$AIRGAP_BUNDLE/images/nim-images.tar"\
$(tr'\n'' '<"$AIRGAP_BUNDLE/images/images.txt")

```
Copy to clipboard
In the air-gapped environment, extract the bundle, load the image archive, and push the images to the local registry:

```
tar-xzfnim-airgap-bundle.tar.gz
exportAIRGAP_BUNDLE="$(pwd)/nim-airgap-bundle"
exportAIRGAP_REGISTRY=registry.airgap.example.com/nim

"$AIRGAP_BUNDLE/tools/airgap-load-images.sh"\
--bundle-dir"$AIRGAP_BUNDLE"\
--registry"$AIRGAP_REGISTRY"

```
Copy to clipboard
The following commands show the equivalent manual image load and push steps:

```
dockerload--input"$AIRGAP_BUNDLE/images/nim-images.tar"

whileIFS=read-rimage;do
[-n"$image"]||continue
name_tag="${image##*/}"
dockertag"$image""$AIRGAP_REGISTRY/$name_tag"
dockerpush"$AIRGAP_REGISTRY/$name_tag"
done<"$AIRGAP_BUNDLE/images/images.txt"

```
Copy to clipboard
Record the rewritten image name because you will use it in the Helm values. For example, if `${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4` becomes `registry.airgap.example.com/nim/nim-llm:2.1.4`, set:

```
image:
repository:registry.airgap.example.com/nim/nim-llm
tag:"2.1.4"
pullPolicy:IfNotPresent

```
Copy to clipboard
Keep the chart’s default probes on `/v1/health/ready` and `/v1/health/live` for either inference engine.
If the air-gapped registry requires authentication, create an image pull secret in the target namespace and reference it with `imagePullSecrets`.
## Bundle Helm Charts and Deployment Recipes[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#bundle-helm-charts-and-deployment-recipes "Link to this heading")
Package the Helm chart and any site-specific values files, manifests, or deployment recipes on the network-connected machine. If you used the provided `airgap-bundle.sh`, this is already done. To perform the steps manually, complete the following:

```
exportAIRGAP_BUNDLE="${AIRGAP_BUNDLE:-$(pwd)/nim-airgap-bundle}"
mkdir-p"$AIRGAP_BUNDLE/charts""$AIRGAP_BUNDLE/values""$AIRGAP_BUNDLE/manifests"

helmpackage"$NIM_LLM_CHART_DIR"--destination"$AIRGAP_BUNDLE/charts"

# Copy the exact values files and manifests you plan to apply offline.
cp"$NIM_LLM_CHART_DIR/values.yaml""$AIRGAP_BUNDLE/values/values.yaml"
cp/path/to/your-airgap-values.yaml"$AIRGAP_BUNDLE/values/airgap-values.yaml"

```
Copy to clipboard
Create a transfer archive after images, model assets, charts, and values are ready:

```
tar-C"$(dirname"$AIRGAP_BUNDLE")"\
-czfnim-airgap-bundle.tar.gz\
"$(basename"$AIRGAP_BUNDLE")"

```
Copy to clipboard
In the air-gapped environment, extract the bundle before installing:

```
tar-xzfnim-airgap-bundle.tar.gz
exportAIRGAP_BUNDLE="$(pwd)/nim-airgap-bundle"

```
Copy to clipboard
## Transfer Model Assets[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#transfer-model-assets "Link to this heading")
Transfer the model store directory (or cache) from the connected machine to the air-gapped environment. To transfer model assets, use one of the following methods:
  * **Archive and copy** : Create a `tar` or `zip` archive, transfer it through an allowed channel, then extract it on the air-gapped host.
  * **`scp`or`rsync`** : Use direct directory transfer when a one-way or out-of-band SSH path is available. `rsync` supports incremental synchronization.
  * **Physical media** : Copy the directory or archive onto portable storage (for example, a USB drive), physically move it to the air-gapped environment, then extract it.


## Run in an Air-Gapped Environment[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#run-in-an-air-gapped-environment "Link to this heading")
On the air-gapped system, mount the pre-staged model directory and run the NIM container.
To run NIM LLM and VLM without outbound network access, complete the following steps.
### Use a Model Store[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#use-a-model-store "Link to this heading")
Run the following commands to use a model store:
Note
Use the same container image that you used to create the model store. The model store can contain settings specific to that image’s inference engine.
  1. Set the runtime variables:

```
exportCONTAINER_NAME=nim-airgap
exportMODEL_REPO=/path/to/model-repository
exportNIM_SERVED_MODEL_NAME=my-model

```
Copy to clipboard
  2. Start the NIM container:

```
dockerrun-it--rm--name="$CONTAINER_NAME"\
--gpusall\
--shm-size=16GB\
-p8000:8000\
-eNIM_MODEL_PATH=/model-repo\
-eNIM_SERVED_MODEL_NAME\
-v"$MODEL_REPO:/model-repo"\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4

```
Copy to clipboard


Important
Do **not** set `NGC_API_KEY` or `HF_TOKEN` in this phase.
### Use Model-Free NIM with a Remote URI (PVC-Based Redeploy)[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#use-model-free-nim-with-a-remote-uri-pvc-based-redeploy "Link to this heading")
If you use model-free NIM with `NIM_MODEL_PATH` set to a remote URI (for example, `ngc://org/team/model:tag`), NIM generates a runtime manifest on the **first** deployment and automatically saves a copy inside `NIM_CACHE_PATH`.
On subsequent restarts, NIM attempts to regenerate the manifest. If generation fails because the registry is unavailable, NIM falls back to the manifest in the mounted cache. Prepare that cache with the same model and container image before moving to the isolated environment.
**Prerequisites:**
  * The NIM cache from the first (network-connected) deployment must be on a persistent volume (PVC) that is re-mounted between deployments. Both model assets and the manifest are stored there.


  1. Set the runtime variables:

```
exportCONTAINER_NAME=nim-airgap-model-free
exportLOCAL_NIM_CACHE=/path/to/persistent-nim-cache# same PVC used on first deploy
exportNIM_MODEL_PATH=ngc://org/team/model:tag

```
Copy to clipboard
  2. Start the NIM container:

```
dockerrun-it--rm--name="$CONTAINER_NAME"\
--gpusall\
--shm-size=16GB\
-p8000:8000\
-eNIM_MODEL_PATH\
-v"$LOCAL_NIM_CACHE:/opt/nim/.cache"\
${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4

```
Copy to clipboard
When manifest generation fails offline, NIM uses the cached manifest and model assets in `/opt/nim/.cache`.


Important
Do **not** set `NGC_API_KEY` in the air-gap phase. If the cache volume is not mounted or was not populated by a prior network-connected deployment, NIM cannot fall back to the required manifest and model assets and fails in a strict air-gap environment.
Tip
Use a separate cache for each model and container image. Before changing either in an air-gapped deployment, prepare and transfer a matching cache on a network-connected host.
### Use the Offline Cache[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#use-the-offline-cache "Link to this heading")
If you prefer to transfer the cache rather than a model store, mount the cache and specify the profile as shown in the following commands:
Note
Use the same container image that you used to populate the cache. Select a profile from that image’s `list-model-profiles` output.
  1. Set the runtime variables:

```
exportCONTAINER_NAME=nim-airgap
exportAIR_GAP_NIM_CACHE=/path/to/transferred-cache
exportPROFILE_HASH=<PROFILE_HASH>

```
Copy to clipboard
  2. Start the NIM container:

```
dockerrun-it--rm--name="$CONTAINER_NAME"\
--gpusall\
--shm-size=16GB\
-p8000:8000\
-eNIM_MODEL_PROFILE="$PROFILE_HASH"\
-v"$AIR_GAP_NIM_CACHE:/opt/nim/.cache"\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4

```
Copy to clipboard


### Model-Free NIM with a Local Path[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#model-free-nim-with-a-local-path "Link to this heading")
When using a model-free NIM with a local model path, no download occurs at startup. If the model directory is already on the air-gapped system, you can run without `download-to-cache` or `create-model-store` and without any API keys:
  1. Set the container name and model repository path:

```
exportCONTAINER_NAME=nim-airgap
exportMODEL_REPO=/path/to/model-repo

```
Copy to clipboard
  2. Run the NIM container with the local model path:

```
dockerrun-it--rm--name="$CONTAINER_NAME"\
--gpusall\
--shm-size=16GB\
-p8000:8000\
-eNIM_SERVED_MODEL_NAME=my-model\
-eNIM_MODEL_PATH=/model-repo\
-v"$MODEL_REPO:/model-repo"\
${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4

```
Copy to clipboard


## Validate Offline Docker Networking[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#validate-offline-docker-networking "Link to this heading")
To verify the container has no outbound network access, add `--network none` and omit `-p 8000:8000`. Docker does not publish host ports in that mode, so probe from inside the container:

```
dockerexecnim-airgapcurl-sfhttp://127.0.0.1:8000/v1/health/ready

```
Copy to clipboard
On a physically isolated host, keep `-p 8000:8000` and skip `--network none`.
## SGLang Attention Backend Failures on Grace Hopper and ARM[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#sglang-attention-backend-failures-on-grace-hopper-and-arm "Link to this heading")
On some `linux/arm64` and Grace Hopper (GH200) hosts, SGLang’s default FlashAttention-3 backend fails at scheduler start with the following error:

```
ImportError: Can not import FA3 in sgl_kernel. Please check your installation.

```
Copy to clipboard
This error concerns the attention backend on the host GPU. It can occur even when all model assets are available locally.
Select a supported attention backend with a container argument:

```
dockerrun-it--rm--name=nim-airgap\
--gpusall\
--shm-size=16GB\
-p8000:8000\
-eNIM_MODEL_PATH=/model-repo\
-eNIM_SERVED_MODEL_NAME=my-model\
-v"$MODEL_REPO:/model-repo"\
${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4\
--attention-backendtriton

```
Copy to clipboard
For the Helm chart, pass the same argument through `customArgs`:

```
customArgs:
-"--attention-backend"
-"triton"

```
Copy to clipboard
Select an attention backend supported by your GPU and container image. Refer to [Advanced Configuration](https://docs.nvidia.com/nim/large-language-models/latest/reference/advanced-configuration.html#advanced-configuration) for other deployment interfaces.
## Deploy on Kubernetes in an Air-Gapped Environment[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#deploy-on-kubernetes-in-an-air-gapped-environment "Link to this heading")
Use this workflow when the air-gapped environment is a Kubernetes cluster with a private registry mirror.
To deploy on Kubernetes in an air gap, complete the following steps.
### Prepare Air-Gap Values[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#prepare-air-gap-values "Link to this heading")
Create a values file that points to the private registry and uses only local model storage. Do not include `model.ngcAPIKey`, `model.ngcAPISecret`, `model.hfTokenSecret`, `NGC_API_KEY`, or `HF_TOKEN` in the air-gapped values.
For a model-free NIM with a pre-staged local model directory, use the following:

```
image:
repository:registry.airgap.example.com/nim/nim-llm
tag:"2.1.4"
pullPolicy:IfNotPresent

imagePullSecrets:[]

model:
modelPath:/model-store/my-model
name:my-model
ngcAPISecret:""
hfTokenSecret:""

persistence:
enabled:true
existingClaim:nim-model-store

env:
-name:NIM_SERVED_MODEL_NAME
value:my-model

```
Copy to clipboard
If you are using a pre-populated NIM cache instead of a local model directory, mount the cache at `model.nimCache`. When a model-specific NIM requires a specific profile, set `NIM_MODEL_PROFILE` through `env`. For single-node deployments, `model.profile` is only applied to multi-node deployments, so use `env` here instead:

```
model:
nimCache:/model-store

env:
-name:NIM_MODEL_PROFILE
value:"<PROFILE_HASH>"

persistence:
enabled:true
existingClaim:nim-model-cache

```
Copy to clipboard
### Install from the Bundled Chart[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#install-from-the-bundled-chart "Link to this heading")
Install with the chart archive and values file from the transfer bundle:

```
kubectlcreatenamespacenim-service

helmupgrade--installnim-llm\
"$AIRGAP_BUNDLE"/charts/nim-llm-*.tgz\
--namespacenim-service\
--values"$AIRGAP_BUNDLE/values/airgap-values.yaml"

```
Copy to clipboard
Replace `airgap-values.yaml` with the filename you passed to `--values` when creating the bundle. Run `ls "$AIRGAP_BUNDLE/values/"` to confirm the exact name.
Wait for the pod to become ready:

```
kubectlrolloutstatusstatefulset/nim-llm-nnim-service--timeout=60m

```
Copy to clipboard
If your values render a `Deployment` instead of a `StatefulSet`, use the following:

```
kubectlrolloutstatusdeployment/nim-llm-nnim-service--timeout=60m

```
Copy to clipboard
### Validate Without Internet Access[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#validate-without-internet-access "Link to this heading")
Port-forward the service and validate health, model listing, and inference:

```
kubectlport-forward-nnim-serviceservice/nim-llm8000:8000

```
Copy to clipboard
In another terminal, run the following:

```
curl-sfhttp://127.0.0.1:8000/v1/health/ready
curl-sfhttp://127.0.0.1:8000/v1/models

curl-sfhttp://127.0.0.1:8000/v1/chat/completions\
-H'Content-Type: application/json'\
-d'{
    "model": "my-model",
    "messages": [{"role": "user", "content": "Say hello in one sentence."}],
    "max_tokens": 16
  }'

```
Copy to clipboard
For a stronger validation, block egress from the NIM namespace with a default-deny egress `NetworkPolicy` before starting the release. Keep DNS and private registry access only if your cluster requires those paths for image pulls or service discovery.

```
apiVersion:networking.k8s.io/v1
kind:NetworkPolicy
metadata:
name:deny-egress
namespace:nim-service
spec:
podSelector:{}
policyTypes:
-Egress

```
Copy to clipboard
This policy blocks **all** egress. If your cluster needs DNS resolution or in-cluster registry access after the image is pulled, add matching `egress` rules (for example, to `kube-dns` on UDP/TCP 53 and to your private registry) so those paths still work.
Then restart the pod and repeat the smoke test:

```
kubectlrolloutrestartstatefulset/nim-llm-nnim-service
kubectlrolloutstatusstatefulset/nim-llm-nnim-service--timeout=60m

```
Copy to clipboard
## Configure a Proxy and Certificates[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#configure-a-proxy-and-certificates "Link to this heading")
If the network-connected phase runs behind a corporate or outbound proxy, you must configure the standard proxy environment variables so that downloads are routed correctly.
### HTTP/HTTPS Proxy[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#http-https-proxy "Link to this heading")
When running the network-connected phase behind a proxy, you can specify environment variables to direct HTTP and HTTPS traffic through your organization’s proxy servers. The following variables are commonly used for proxy configuration:  
| Variable  | Description  |  
| --- | --- |  
| `HTTP_PROXY`  | Proxy server for HTTP connections  |  
| `HTTPS_PROXY`  | Proxy server for HTTPS connections  |  
| `NO_PROXY`  | Comma-separated list of hostnames, domains, or IPs that should bypass the proxy  |  
Note
In the air-gapped phase, there is no outbound traffic so proxy variables are not needed.
### CA Certificate Injection[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#ca-certificate-injection "Link to this heading")
When outbound HTTPS traffic goes through a TLS-inspecting proxy, or when downloading from a corporate registry whose TLS certificate is signed by an internal CA, you must inject the corporate Certificate Authority (CA) certificate so that TLS verification succeeds. Set `SSL_CERT_FILE` to point to a **combined CA bundle** inside the container.
Warning
Setting `SSL_CERT_FILE` **replaces** the container’s default trust store. If you point it at a file containing only your corporate CA, connections to public endpoints (such as `api.ngc.nvidia.com`) will fail because the public CAs are no longer trusted. Always create a combined bundle that includes the default CAs and your corporate CA.
**Create a combined CA bundle (one-time, on the host):**
To add your corporate CA without losing trust in public CAs, concatenate the container’s default bundle with your corporate CA certificate:

```
# Extract the default CA bundle from the container
dockerrun--rm--entrypointbash\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4\
-c'cat /etc/ssl/certs/ca-certificates.crt'>combined-ca-bundle.pem

# Append your corporate CA
cat/path/to/corporate-ca.pem>>combined-ca-bundle.pem

```
Copy to clipboard
**Run with the combined bundle:**
Mount and reference the combined bundle:

```
dockerrun--rm--gpusall\
-v"$LOCAL_NIM_CACHE:/opt/nim/.cache"\
-v./combined-ca-bundle.pem:/etc/ssl/certs/custom-ca-bundle.pem:ro\
-eSSL_CERT_FILE=/etc/ssl/certs/custom-ca-bundle.pem\
-eREQUESTS_CA_BUNDLE=/etc/ssl/certs/custom-ca-bundle.pem\
-eNGC_API_KEY\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4\
download-to-cache--all

```
Copy to clipboard
If downloading through a proxy, also add `-e HTTP_PROXY=...` and `-e HTTPS_PROXY=...`. A proxy is **not** required for `SSL_CERT_FILE` to take effect.
Note
CA certificate injection is only relevant during the network-connected phase. After the deployment is fully air-gapped, there is no outbound HTTPS traffic and no CA configuration is needed.
These variables affect **outbound** TLS (model downloads). They are unrelated to the `NIM_SSL_*` variables, which configure **inbound** TLS at the nginx proxy layer. For more information, refer to [Environment Variables: SSL and TLS](https://docs.nvidia.com/nim/large-language-models/latest/reference/environment-variables.html#ssl-and-tls).
## Environment Variables[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#environment-variables "Link to this heading")
The following environment variables are relevant to air-gap deployment:  
| Variable  | Description  |  
| --- | --- |  
| `NIM_MODEL_PATH`  | Path to the local model repository or a remote URI. In air-gapped mode, set to the local mount path.  |  
| `NIM_CACHE_PATH`  | Directory path for model cache storage inside the container. Default: `/opt/nim/.cache`.  |  
| `NIM_MODEL_PROFILE`  | Profile ID to use. Required when running from cache without a model store.  |  
| `NIM_SERVED_MODEL_NAME`  | Name used for the served model in API responses.  |  
| `NGC_API_KEY`  | NGC API key for downloading models. **Do not set in air-gapped phase.**  |  
| `HF_TOKEN`  | Hugging Face token for downloading models. **Do not set in air-gapped phase.**  |  
## Examples[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#examples "Link to this heading")
Use the following examples to deploy a NIM in an air-gapped environment for different scenarios.
### Download and Create a Model Store[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#download-and-create-a-model-store "Link to this heading")
Use this workflow to create a model store on a network-connected system and then run NIM from that model store in the air-gapped environment.
  1. In the network-connected phase, set the required variables and create the local directories:

```
exportNGC_API_KEY=your_api_key
exportPROFILE_HASH=<your_profile_hash>
exportLOCAL_NIM_CACHE="${LOCAL_NIM_CACHE:-$(pwd)/.cache}"
exportMODEL_REPO=/path/to/model-repository
mkdir-p"$MODEL_REPO""$LOCAL_NIM_CACHE"

```
Copy to clipboard
  2. Download the model to the local cache:

```
dockerrun-it--rm--gpusall\
-eNGC_API_KEY\
-v"$LOCAL_NIM_CACHE:/opt/nim/.cache"\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4\
download-to-cache-p"$PROFILE_HASH"

```
Copy to clipboard
  3. Create the model store:

```
dockerrun-it--rm--gpusall\
-eNGC_API_KEY\
-v"$LOCAL_NIM_CACHE:/opt/nim/.cache"\
-v"$MODEL_REPO:/model-repo"\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4\
create-model-store-p"$PROFILE_HASH"-m/model-repo

```
Copy to clipboard
  4. Transfer the model store to the air-gapped system.
  5. On the air-gapped system, set the runtime variables and start NIM:

```
exportCONTAINER_NAME=nim-airgap
exportMODEL_REPO=/path/to/model-repository
exportNIM_SERVED_MODEL_NAME=my-model

```
Copy to clipboard
  6. Start NIM:

```
dockerrun-it--rm--name="$CONTAINER_NAME"\
--gpusall\
--shm-size=16GB\
-p8000:8000\
-eNIM_MODEL_PATH=/model-repo\
-eNIM_SERVED_MODEL_NAME\
-v"$MODEL_REPO:/model-repo"\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4

```
Copy to clipboard


### Use the Offline Cache[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#id3 "Link to this heading")
Use this workflow when you want to transfer the NIM cache instead of creating a model store.
  1. In the network-connected phase, set the required variables and create the cache directory:

```
exportLOCAL_NIM_CACHE="${LOCAL_NIM_CACHE:-$(pwd)/.cache}"
exportPROFILE_HASH=<your_profile_hash>
mkdir-p"$LOCAL_NIM_CACHE"

```
Copy to clipboard
  2. Download the model to the cache:

```
dockerrun--rm--gpusall\
-v"$LOCAL_NIM_CACHE:/opt/nim/.cache"\
-eNGC_API_KEY\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4\
download-to-cache-p"$PROFILE_HASH"

```
Copy to clipboard
  3. Transfer the cache to the air-gapped system.
  4. On the air-gapped system, set the runtime variables and start NIM:

```
exportAIR_GAP_NIM_CACHE=/path/to/transferred-cache
exportCONTAINER_NAME=nim-airgap
exportPROFILE_HASH=<your_profile_hash>

```
Copy to clipboard
  5. Start NIM:

```
dockerrun-it--rm--name="$CONTAINER_NAME"\
--gpusall\
--shm-size=16GB\
-p8000:8000\
-eNIM_MODEL_PROFILE="$PROFILE_HASH"\
-v"$AIR_GAP_NIM_CACHE:/opt/nim/.cache"\
${NIM_LLM_MODEL_SPECIFIC_IMAGE}:2.1.4

```
Copy to clipboard


### Model-Free NIM with a Local Path (No Download Needed)[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#model-free-nim-with-a-local-path-no-download-needed "Link to this heading")
If the model directory is already on the air-gapped system (for example, copied using secure transfer), you can run directly without any prior download steps:
  1. Set the runtime variables:

```
exportCONTAINER_NAME=nim-airgap
exportMODEL_REPO=/path/to/model-repo

```
Copy to clipboard
  2. Start NIM by using the local model path:

```
dockerrun-it--rm--name="$CONTAINER_NAME"\
--gpusall\
--shm-size=16GB\
-p8000:8000\
-eNIM_SERVED_MODEL_NAME=my-model\
-eNIM_MODEL_PATH=/model-repo\
-v"$MODEL_REPO:/model-repo"\
${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4

```
Copy to clipboard


### Network-Connected Phase with Proxy and CA Certificate[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#network-connected-phase-with-proxy-and-ca-certificate "Link to this heading")
For environments that require an HTTP/HTTPS proxy and custom CA certificates during the download phase, use a **combined CA bundle** (refer to [CA Certificate Injection](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#ca-certificate-injection)):

```
dockerrun--rm--gpusall\
--add-host=host.docker.internal:host-gateway\
-v./combined-ca-bundle.pem:/etc/ssl/certs/custom-ca-bundle.pem:ro\
-eNIM_SERVER_PORT=8000\
-eNIM_SERVED_MODEL_NAME=my-model\
-eNIM_MODEL_PATH=hf://TinyLlama/TinyLlama-1.1B-Chat-v1.0\
-eHTTP_PROXY=http://host.docker.internal:8888\
-eHTTPS_PROXY=http://host.docker.internal:8888\
-eSSL_CERT_FILE=/etc/ssl/certs/custom-ca-bundle.pem\
-eREQUESTS_CA_BUNDLE=/etc/ssl/certs/custom-ca-bundle.pem\
${NIM_LLM_MODEL_FREE_IMAGE}:2.1.4

```
Copy to clipboard
Note
After the model is pre-staged and transferred, the air-gapped phase does not require proxy or CA certificate configuration.
## Next Steps[#](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-deployment.html#next-steps "Link to this heading")
Continue with helper scripts, source-specific downloads, or model-free URI workflows:
  * [Air-Gap Helper Scripts](https://docs.nvidia.com/nim/large-language-models/latest/deployment/air-gap-helper-scripts.html) — Copy `airgap-bundle.sh` and `airgap-load-images.sh` for packaging and loading images.
  * [Model Download](https://docs.nvidia.com/nim/large-language-models/latest/deployment/model-download.html#model-download) — Source-specific download, mirror, and cache commands.
  * [About Model-Free NIM](https://docs.nvidia.com/nim/large-language-models/latest/deployment/model-free-nim.html#model-free-nim) — Model-free URI sources and PVC-based manifest reuse.


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
