---
title: ALCF Inference Endpoints - ALCF User Guides
id: alcf-inference-endpoints-alcf-user-guides
tags:
- onprem-llm-inference-stacks-859099
- hpc-inference
- national-lab
created: '2026-10-23T17:22:55.586045Z'
updated: '2026-10-23T17:23:51.054427Z'
source: https://docs.alcf.anl.gov/services/inference-endpoints/
source_domain: docs.alcf.anl.gov
fetched_at: '2026-10-23T17:22:55.562811Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: policy
deprecated: false
summary: 'ALCF user guide for Inference Endpoints: Open WebUI front end at inference.alcf.anl.gov;
  OpenAI-compatible API incl /chat/completions,/responses,/messages,/batches; backends
  Sophia (A100, vLLM), Metis (SambaNova SN40L, DirectAPI), Minerva (NVIDIA B200),
  Tara (NVIDIA GH200); Globus tokens 48h, 30-day reauth; login from ~19 DOE lab identity
  providers; model status Live/Starting/Queued/Offline.'
---

*Suggested by [[first-federated-inference-resource-scheduling-toolkit-for]] — cited as primary source*

# ALCF Inference Endpoints[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#alcf-inference-endpoints "Permanent link")
Unlock Powerful AI Inference at Argonne Leadership Computing Facility (ALCF). This service provides API access to a variety of state-of-the-art open-source models running on dedicated ALCF hardware. Join our [mailing list](https://mailman.cels.anl.gov/mailman3/lists/inference-service-notify.lists.alcf.anl.gov/) to receive updates and maintenance notifications.
## Quick Start[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#quick-start "Permanent link")
This guide will walk you through the fastest ways to start using the ALCF Inference Endpoints.
### Supported Identity Providers[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#supported-identity-providers "Permanent link")
To access the ALCF Inference Endpoints via the Web UI or API, you can log in using credentials from any of these institutional identity providers:  
| alcf.anl.gov  | ameslab.gov  | anl.gov  | bnl.gov  | fnal.gov  | hpc-dtn-auth.lanl.gov  | inl.gov  |  
| --- | --- | --- | --- | --- | --- | --- |  
| **jlab.org**  | **lbl.gov**  | **llnl.gov**  | **nersc.gov**  | **netl.doe.gov**  | **nrel.gov**  | **nlr.gov**  |  
| **ornl.gov**  | **pppl.gov**  | **pnnl.gov**  | **sandia.gov**  | **slac.stanford.edu**  |  
### Web UI[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#web-ui "Permanent link")
The easiest way to get started is through the web interface, accessible at <https://inference.alcf.anl.gov/>
The UI is based on the popular Open WebUI platform. After logging in with your ANL or ALCF credentials, you can:
  1. Select a model from the dropdown menu at the top of the screen.
  2. Start a conversation directly in the chat interface.


In the model selection dropdown, you can see the status of each model:
  * **Live:** These models are "hot" and ready for immediate use.
  * **Starting:** A node has been acquired and the model is being loaded into memory.
  * **Queued:** The model is in a queue waiting for resources to become available.
  * **Offline:** The model is available but not currently loaded. It will be queued for loading when a user sends a request.
  * **All:** Lists all available models regardless of their status.


For Advanced UI Features
For a full guide on advanced features like RAG (Retrieval-Augmented Generation), function calling, and more, please refer to the official [Open WebUI documentation](https://docs.openwebui.com/).
### API Access[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#api-access "Permanent link")
For programmatic access, you can use the API endpoints directly.
Interactive API Documentation
The service publishes a [Swagger UI](https://inference-api.alcf.anl.gov/resource_server/docs) generated from its OpenAPI schema. See [Interactive API Reference](https://docs.alcf.anl.gov/services/inference-endpoints/api/#interactive-api-reference).
Using the alcf-ai CLI or SDK
The [`alcf-ai`](https://pypi.org/project/alcf-ai/) package provides a CLI and an OpenAI-compatible Python client for the Inference Service, and uses the shared [`alcf-tokens`](https://pypi.org/project/alcf-tokens/) CLI for authentication. See [alcf-ai CLI and SDK](https://docs.alcf.anl.gov/services/inference-endpoints/alcf-ai/) for details.
#### 1. Setup Your Environment[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#1-setup-your-environment "Permanent link")
You can run the following setup from anywhere (your local machine, or an ALCF machine).
Installing `alcf-ai` and `alcf-tokens`
The ALCF guides run `alcf-ai` and `alcf-tokens` directly, so both need to be installed and on your `PATH`. Any Python package manager works:

```
uvtoolinstallalcf-ai# uv: one package per command, into ~/.local/bin
uvtoolinstallalcf-tokens
pipxinstallalcf-aialcf-tokens# pipx: both at once, into ~/.local/bin

```

With `pip` or `conda`, create or activate an environment, then run `pip install alcf-ai alcf-tokens`. To run a command without installing it, prefix it with `uvx` or `pipx run`, for example `uvx alcf-tokens login inference`.
See [Python Environments](https://docs.alcf.anl.gov/dev-environment/python-environments/) for the other supported package managers.
[Auth script (Deprecated)](https://docs.alcf.anl.gov/services/inference-endpoints/#__tabbed_1_2)
Install `alcf-ai` and `alcf-tokens` as above. For the Python examples on this page, install the OpenAI SDK in the environment where you run Python:

```
pipinstallopenai

```

With `uv`, `uv run --with openai --with alcf-tokens python` starts Python with both packages in a throwaway environment instead.

```
# Create and activate a virtual environment
python-mvenv.venv
source.venv/bin/activate
# Install necessary packages
pipinstallopenaiglobus-sdk
# Download the deprecated authentication helper script
wgethttps://raw.githubusercontent.com/argonne-lcf/inference-endpoints/refs/heads/main/inference_auth_token.py
# If `wget` is unavailable on your system, try `curl -O` instead.

```

#### 2. Authenticate[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#2-authenticate "Permanent link")
To access the endpoints, you need an authentication token.
[Auth script (Deprecated)](https://docs.alcf.anl.gov/services/inference-endpoints/#__tabbed_2_2)
Use this command to login to the inference service:

```
alcf-tokenslogininference

```

This requests the inference scopes only, so you are not asked to authorize IRI, Globus Compute, or Globus Flows. See [alcf-ai CLI and SDK](https://docs.alcf.anl.gov/services/inference-endpoints/alcf-ai/) for token verification, and for the `--authorize-transfer` collections that batch processing and data staging require.

```
# Authenticate with your Globus account
pythoninference_auth_token.pyauthenticate
# Retrieve a token, or check how long it is valid for
# (`units` can be seconds, minutes, or hours)
pythoninference_auth_token.pyget_time_until_token_expiration--unitsseconds
pythoninference_auth_token.pyget_access_token

```

Separate token caches
`alcf-tokens`/`alcf-ai` and the `inference_auth_token.py` helper use **different** Globus token caches, so authenticating with one does not authenticate the other.
Token Validity
  * Access tokens are valid for 48 hours. Both `alcf-tokens get-token inference` and `python inference_auth_token.py get_access_token` will automatically refresh your token if it has expired.
  * An internal policy requires re-authentication every 30 days. If you encounter permission errors, logout from Globus at [app.globus.org/logout](https://app.globus.org/logout) and re-run `alcf-tokens login inference` (or `alcf-tokens login` to authorize all services).


#### 3. Make a Test Call[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#3-make-a-test-call "Permanent link")
Once authenticated, you can make a test call using cURL or Python.
[Python (OpenAI SDK)](https://docs.alcf.anl.gov/services/inference-endpoints/#__tabbed_3_2)  
| 
```












```
 | 
```
#!/bin/bash
# Get your access token
access_token=$(alcf-tokensget-tokeninference)
curl-XPOST"https://inference-api.alcf.anl.gov/resource_server/metis/api/v1/chat/completions"\
-H"Authorization: Bearer ${access_token}"\
-H"Content-Type: application/json"\
            "model": "gpt-oss-120b",
            "messages":[{"role": "user", "content": "Explain quantum computing in simple terms."}]

```
 |  
| --- | --- |  
| 
```

















```
 | 
```
fromopenaiimport OpenAI
fromalcf_tokens.authimport get_access_token
# Get your access token
access_token = get_access_token("inference")
client = OpenAI(
    api_key=access_token,
    base_url="https://inference-api.alcf.anl.gov/resource_server/metis/api/v1"
response = client.chat.completions.create(
    model="gpt-oss-120b",
    messages=[{"role": "user", "content": "Explain quantum computing in simple terms."}]
print(response.choices[0].message.content)

```
 |  
| --- | --- |  
## System Details[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#system-details "Permanent link")
### Available Clusters[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#available-clusters "Permanent link")
Four clusters are currently available, with additional systems coming soon:  
| Cluster  | Status  | Framework  | Base URL  | Supported Endpoints  |  
| --- | --- | --- | --- | --- |  
| **[NVIDIA A100 (Sophia)](https://docs.alcf.anl.gov/sophia/getting-started/)**  | Active  | vLLM  | `/resource_server/sophia/vllm/v1`  |  `/chat/completions``/responses``/messages``/completions``/embeddings``/batches`  |  
| **[SambaNova SN40L (Metis)](https://docs.alcf.anl.gov/ai-testbed/sn40l_inference/)**  | Active  | DirectAPI  | `/resource_server/metis/api/v1`  |  `/chat/completions``/responses`  |  
| **[NVIDIA B200 (Minerva)](https://www.alcf.anl.gov/minerva)**  | Active  | DirectAPI  | `/resource_server/minerva/api/v1`  |  `/chat/completions``/responses``/messages``/completions`  |  
| **[NVIDIA GH200 (Tara)](https://www.alcf.anl.gov/tara)**  | Active  | DirectAPI  | `/resource_server/tara/api/v1`  |  `/chat/completions``/responses``/messages`  |  
## Important Notes[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#important-notes "Permanent link")
  * If you’re interested in extended model runtimes, reservations, or private model deployments, please contact ALCF Support.


## Troubleshooting[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#troubleshooting "Permanent link")
  * **Connection Timeout:** The model you are requesting may be queued as the cluster has too many pending jobs. You can check model status by querying the `/jobs` endpoint. See [Querying Endpoint Status](https://docs.alcf.anl.gov/services/inference-endpoints/api/#querying-endpoint-status) for an example.
  * **Permission Denied:** Your token may have expired. Logout from Globus at [app.globus.org/logout](https://app.globus.org/logout) and re-authenticate. With `alcf-tokens`/`alcf-ai`, re-run `alcf-tokens login inference` (or `alcf-tokens login` to authorize all services). With the `inference_auth_token.py` helper, re-run `python inference_auth_token.py authenticate --force`.
  * **Batch Permission Error:** Ensure your input/output paths are in a readable location like `/eagle/argonne_tpc`. It is currently internal only to ALCF and will be made public in the future.
  * **IdentityMismatchError: Detected a change in identity:** This happens when trying to get an access token using a Globus identity that is not linked to the one you previously used to generate your access tokens. Delete the cached tokens file and restart the authentication process. The file depends on which client you use: the `inference_auth_token.py` helper stores tokens at `~/.globus/app/58fdd3bc-e1c3-4ce5-80ea-8d6b87cfb944/inference_app/tokens.json`, while `alcf-tokens` and `alcf-ai` store them at `~/.globus/app/7f3e61f5-e0de-4e8f-9150-0a62c65dda63/alcf_tokens/tokens.json` (or run `alcf-tokens clear-tokens`).
  * **Too Many Requests (429):** You have exceeded the per-second request limit or a tokens-per-minute limit. Wait for the window to reset, then retry with smaller requests. See [Rate Limits](https://docs.alcf.anl.gov/services/inference-endpoints/api/#rate-limits).


## Notifications[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#notifications "Permanent link")
To receive notifications regarding new model support, maintenances, and policy updates, please join our [mailing list](https://mailman.cels.anl.gov/mailman3/lists/inference-service-notify.lists.alcf.anl.gov/).
## Acknowledgements[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#acknowledgements "Permanent link")
This work was supported by the U.S. Department of Energy, Office of Science, Office of Advanced Scientific Computing Research, under Contract No. DE-AC02-06CH11357. This research used resources of the Argonne Leadership Computing Facility, which is a DOE Office of Science User Facility.
### Citations[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#citations "Permanent link")
If you use the Inference Service for your publications, please add the Non-INCITE, Non-ALCC ALCF Acknowledgement from the [ALCF Policies](https://docs.alcf.anl.gov/policies/alcf-acknowledgement-policy).
Additionally, please cite our paper:

```
@inproceedings{10.1145/3731599.3767346,
  author = {Tanikanti, Aditya and C\^{o}t\'{e}, Benoit and Guo, Yanfei and Chen, Le and Saint, Nickolaus and Chard, Ryan and Raffenetti, Ken and Thakur, Rajeev and Uram, Thomas and Foster, Ian and Papka, Michael E. and Vishwanath, Venkatram},
  title = {FIRST: Federated Inference Resource Scheduling Toolkit for Scientific AI Model Access},
  year = {2025},
  isbn = {9798400718717},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  url = {https://doi.org/10.1145/3731599.3767346},
  doi = {10.1145/3731599.3767346},
  abstract = {We present the Federated Inference Resource Scheduling Toolkit (FIRST), a framework enabling Inference-as-a-Service across distributed High-Performance Computing (HPC) clusters. FIRST provides cloud-like access to diverse AI models, like Large Language Models (LLMs), on existing HPC infrastructure. Leveraging Globus Auth and Globus Compute, the system allows researchers to run parallel inference workloads via an OpenAI-compliant API on private, secure environments. This cluster-agnostic API allows requests to be distributed across federated clusters, targeting numerous hosted models. FIRST supports multiple inference backends (e.g., vLLM), auto-scales resources, maintains "hot" nodes for low-latency execution, and offers both high-throughput batch and interactive modes. The framework addresses the growing demand for private, secure, and scalable AI inference in scientific workflows, allowing researchers to generate billions of tokens daily on-premises without relying on commercial cloud infrastructure.},
  booktitle = {Proceedings of the SC '25 Workshops of the International Conference for High Performance Computing, Networking, Storage and Analysis},
  pages = {52–60},
  numpages = {9},
  keywords = {Inference as a Service, High Performance Computing, Job Schedulers, Large Language Models, Globus, Scientific Computing},
  series = {SC Workshops '25}

```

## Contact Us[¶](https://docs.alcf.anl.gov/services/inference-endpoints/#contact-us "Permanent link")
For questions or support, please contact ALCF Support.
Back to top 
  *[DOE]: Department of Energy
  *[INCITE]: Innovative and Novel Computational Impact on Theory and Experiment
  *[ALCC]: ASCR Leadership Computing Challenge
