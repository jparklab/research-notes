---
title: 'GitHub - llm-d/llm-d-routing-sidecar: Incubating P/D sidecar for llm-d · GitHub'
id: github-llm-dllm-d-routing-sidecar-incubating-pd-sidecar-for-llm-d-github
tags:
- llm-d
- router
- routing-sidecar
- ssrf
- pd-disaggregation
- nixl
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:43:31.451819Z'
updated: '2026-09-12T03:37:37.535738Z'
source: https://github.com/llm-d/llm-d-routing-sidecar
source_domain: github.com
fetched_at: '2026-09-11T19:43:31.425438Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub repo for llm-d-routing-sidecar (Go, Apache-2.0), now a public archive:
  the README''s deprecation banner states the sidecar''s code moved to llm-d-inference-scheduler
  under cmd/pd_sidecar and pkg/sidecar, with ''all future development'' there and
  a request not to file issues/PRs against this repo. The sidecar is a reverse proxy
  that redirects requests to the prefill worker named in the x-prefiller-host-port
  HTTP header (host:port format, any port allowed on an allowed host). SSRF protection
  (PR #49, merged Jul 18 2025) is opt-in via --enable-ssrf-protection (disabled by
  default): when enabled it watches a specific InferencePool CRD (via INFERENCE_POOL_NAMESPACE/INFERENCE_POOL_NAME
  env vars or CLI flags) and builds a host/IP allowlist from the pods matching that
  InferencePool''s selector; unauthorized targets get HTTP 403; the allowlist auto-updates
  on pod add/remove/update. This makes the SSRF protection mechanism intrinsically
  Kubernetes-coupled -- it requires a real InferencePool CRD and a pod-watching client
  (RBAC for pod/InferencePool list-watch, namespace field via k8s Downward API) to
  compute the allowlist; without SSRF protection, the sidecar accepts any host:port
  in the header, so a non-k8s backend can run the sidecar unprotected but loses the
  built-in allowlist defense entirely. Quick-start requires 2 GPUs and runs vLLM with
  NixlConnector (kv_role=kv_both) for KV transfer between prefill and decode; supported
  --connector values are nixl (deprecated), nixlv2 (default as of PR #53, merged before
  Jul 22 2025), and lmcache (deprecated). Latest tagged release is v0.3.0 (Sep 25,
  2025); repo archived Feb 3, 2026.'
utility_score: 14.0
---

[Skip to content](https://github.com/llm-d/llm-d-routing-sidecar#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-routing-sidecar) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-routing-sidecar) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-routing-sidecar) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-routing-sidecar).
This repository was archived by the owner on Feb 3, 2026. It is now read-only. 
/ **[llm-d-routing-sidecar](https://github.com/llm-d/llm-d-routing-sidecar) ** Public archive
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-routing-sidecar) You must be signed in to change notification settings
  * [ Fork 33 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-routing-sidecar)
  * [ Star  17 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-routing-sidecar)


[**22** Branches](https://github.com/llm-d/llm-d-routing-sidecar/branches)[](https://github.com/llm-d/llm-d-routing-sidecar/tags)
Go to file
Open more actions menu
## Latest commit
[chore: updates deprecation notice in README.md (](https://github.com/llm-d/llm-d-routing-sidecar/commit/78051b86ff13c4511fb1c8c7def56e04b0850b67)[#68](https://github.com/llm-d/llm-d-routing-sidecar/pull/68)[)](https://github.com/llm-d/llm-d-routing-sidecar/commit/78051b86ff13c4511fb1c8c7def56e04b0850b67)
Open commit details
Nov 13, 2025
[78051b8](https://github.com/llm-d/llm-d-routing-sidecar/commit/78051b86ff13c4511fb1c8c7def56e04b0850b67) · Nov 13, 2025
## History
[111 Commits](https://github.com/llm-d/llm-d-routing-sidecar/commits/main/)
Open commit details
111 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [include linux/arm64 platform in multiarch image (](https://github.com/llm-d/llm-d-routing-sidecar/commit/3004256a35a68903dbba56247224f8ff043c96b9 "include linux/arm64 platform in multiarch image \(#63\)")[#63](https://github.com/llm-d/llm-d-routing-sidecar/pull/63)[)](https://github.com/llm-d/llm-d-routing-sidecar/commit/3004256a35a68903dbba56247224f8ff043c96b9 "include linux/arm64 platform in multiarch image \(#63\)")  | Sep 25, 2025  |  
| [cmd/llm-d-routing-sidecar](https://github.com/llm-d/llm-d-routing-sidecar/tree/main/cmd/llm-d-routing-sidecar "This path skips through empty directories")  | [cmd/llm-d-routing-sidecar](https://github.com/llm-d/llm-d-routing-sidecar/tree/main/cmd/llm-d-routing-sidecar "This path skips through empty directories")  |  [Enable](https://github.com/llm-d/llm-d-routing-sidecar/commit/66d51e248e6fcffa153b44df3336c73c1a0b4c1e "Enable `nixlv2` by default instead of `nixl` connector \(#53\)

In the next release, we will have both but enabling the latest protocol
by default.

Part one of https://github.com/llm-d/llm-d-routing-sidecar/issues/21

Signed-off-by: Pierangelo Di Pilato <pierdipi@redhat.com>") [by default instead of](https://github.com/llm-d/llm-d-routing-sidecar/commit/66d51e248e6fcffa153b44df3336c73c1a0b4c1e "Enable `nixlv2` by default instead of `nixl` connector \(#53\)

In the next release, we will have both but enabling the latest protocol
by default.

Part one of https://github.com/llm-d/llm-d-routing-sidecar/issues/21

Signed-off-by: Pierangelo Di Pilato <pierdipi@redhat.com>") [connector (](https://github.com/llm-d/llm-d-routing-sidecar/commit/66d51e248e6fcffa153b44df3336c73c1a0b4c1e "Enable `nixlv2` by default instead of `nixl` connector \(#53\)

In the next release, we will have both but enabling the latest protocol
by default.

Part one of https://github.com/llm-d/llm-d-routing-sidecar/issues/21

Signed-off-by: Pierangelo Di Pilato <pierdipi@redhat.com>")[#53](https://github.com/llm-d/llm-d-routing-sidecar/pull/53)[)](https://github.com/llm-d/llm-d-routing-sidecar/commit/66d51e248e6fcffa153b44df3336c73c1a0b4c1e "Enable `nixlv2` by default instead of `nixl` connector \(#53\)

In the next release, we will have both but enabling the latest protocol
by default.

Part one of https://github.com/llm-d/llm-d-routing-sidecar/issues/21

Signed-off-by: Pierangelo Di Pilato <pierdipi@redhat.com>")  | Jul 22, 2025  |  
|   |   |  [Add SSRF protection with InferencePool-based allowlisting (](https://github.com/llm-d/llm-d-routing-sidecar/commit/8400f1f849f4c0a8ed4b60109593524d1d4ac597 "Add SSRF protection with InferencePool-based allowlisting \(#49\)

* Add SSRF protection with InferencePool-based allowlisting

Implements SSRF protection for prefill requests using a dynamic allowlist
based on InferencePool resource selectors. Key features:

- Feature flag --enable-ssrf-protection \(disabled by default\)
- Watches InferencePool CRDs to build allowlist of valid prefill targets
- Host-only allowlist \(any port allowed on authorized hosts\)
- Backward compatible when disabled
* Add example RBAC

Addresses SSRF vulnerability in `x-prefiller-host-port` header processing
by restricting targets to pods matching InferencePool selectors.

Signed-off-by: Pierangelo Di Pilato <pierdipi@redhat.com>")[#49](https://github.com/llm-d/llm-d-routing-sidecar/pull/49)[)](https://github.com/llm-d/llm-d-routing-sidecar/commit/8400f1f849f4c0a8ed4b60109593524d1d4ac597 "Add SSRF protection with InferencePool-based allowlisting \(#49\)

* Add SSRF protection with InferencePool-based allowlisting

Implements SSRF protection for prefill requests using a dynamic allowlist
based on InferencePool resource selectors. Key features:

- Feature flag --enable-ssrf-protection \(disabled by default\)
- Watches InferencePool CRDs to build allowlist of valid prefill targets
- Host-only allowlist \(any port allowed on authorized hosts\)
- Backward compatible when disabled
* Add example RBAC

Addresses SSRF vulnerability in `x-prefiller-host-port` header processing
by restricting targets to pods matching InferencePool selectors.

Signed-off-by: Pierangelo Di Pilato <pierdipi@redhat.com>")  | Jul 18, 2025  |  
|   | Jul 17, 2025  |  
|   |   |   | Apr 29, 2025  |  
|   |   |  [Ensure max_completion_tokens=1 for prefill (](https://github.com/llm-d/llm-d-routing-sidecar/commit/e7151bd2fdc455cbb5cea967a4a29e3db496c26d "Ensure max_completion_tokens=1 for prefill \(#67\)

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>")[#67](https://github.com/llm-d/llm-d-routing-sidecar/pull/67)[)](https://github.com/llm-d/llm-d-routing-sidecar/commit/e7151bd2fdc455cbb5cea967a4a29e3db496c26d "Ensure max_completion_tokens=1 for prefill \(#67\)

Signed-off-by: Shmuel Kallner <kallner@il.ibm.com>")  | Oct 29, 2025  |  
|   | Jul 17, 2025  |  
|   |   |   | May 1, 2025  |  
|   |   |   | May 19, 2025  |  
|   |   |   | May 14, 2025  |  
|   |   |   | May 19, 2025  |  
|   |   | [[Feat] Replace with ubi9/ubi-micro image to reduce the image size.](https://github.com/llm-d/llm-d-routing-sidecar/commit/853c669c4664bcc2dcf4b9b2ef3bc03b93309bcf "\[Feat\] Replace with ubi9/ubi-micro image to reduce the image size.

Signed-off-by: yafengio <yafengdoudou@gmail.com>")  | Jul 4, 2025  |  
|   |   |   | May 13, 2025  |  
|   |   |   | May 20, 2025  |  
|   |   |  [chore: updates deprecation notice in README.md (](https://github.com/llm-d/llm-d-routing-sidecar/commit/78051b86ff13c4511fb1c8c7def56e04b0850b67 "chore: updates deprecation notice in README.md \(#68\)

Maybe very last PR? :\)

Signed-off-by: Bartosz Majsak <bartosz.majsak@gmail.com>")[#68](https://github.com/llm-d/llm-d-routing-sidecar/pull/68)[)](https://github.com/llm-d/llm-d-routing-sidecar/commit/78051b86ff13c4511fb1c8c7def56e04b0850b67 "chore: updates deprecation notice in README.md \(#68\)

Maybe very last PR? :\)

Signed-off-by: Bartosz Majsak <bartosz.majsak@gmail.com>")  | Nov 13, 2025  |  
|   |   | [Bump golang.org/x/oauth2 from 0.21.0 to 0.27.0](https://github.com/llm-d/llm-d-routing-sidecar/commit/657f74445a82bff4b867ed6b0c909486eca8e51b "Bump golang.org/x/oauth2 from 0.21.0 to 0.27.0

Bumps \[golang.org/x/oauth2\]\(https://github.com/golang/oauth2\) from 0.21.0 to 0.27.0.
- \[Commits\]\(https://github.com/golang/oauth2/compare/v0.21.0...v0.27.0\)

---
updated-dependencies:
- dependency-name: golang.org/x/oauth2
  dependency-version: 0.27.0
  dependency-type: indirect
...

Signed-off-by: dependabot\[bot\] <support@github.com>")  | Jul 18, 2025  |  
|   |   | [Bump golang.org/x/oauth2 from 0.21.0 to 0.27.0](https://github.com/llm-d/llm-d-routing-sidecar/commit/657f74445a82bff4b867ed6b0c909486eca8e51b "Bump golang.org/x/oauth2 from 0.21.0 to 0.27.0

Bumps \[golang.org/x/oauth2\]\(https://github.com/golang/oauth2\) from 0.21.0 to 0.27.0.
- \[Commits\]\(https://github.com/golang/oauth2/compare/v0.21.0...v0.27.0\)

---
updated-dependencies:
- dependency-name: golang.org/x/oauth2
  dependency-version: 0.27.0
  dependency-type: indirect
...

Signed-off-by: dependabot\[bot\] <support@github.com>")  | Jul 18, 2025  |  
| View all files |  
## Repository files navigation
# llm-d-routing-sidecar
Warning
The code of the routing sidecar has been moved to the [llm-d-inference-scheduler repository](https://github.com/llm-d/llm-d-inference-scheduler), under the `cmd/pd_sidecar` and `pkg/sidecar` directories. This repository is deprecated and shall soon be archived. All future development will done under the llm-d-inference-scheduler repository. Please do **not** submit issues or PRs against this repository.
This project provides a reverse proxy redirecting incoming requests to the prefill worker specified in the `x-prefiller-host-port` HTTP request header.
## Security Features
### SSRF Protection
The sidecar includes SSRF (Server-Side Request Forgery) protection that can be enabled via feature flag. When enabled, it watches a specific InferencePool resource and maintains an allowlist of valid prefill targets based on pods matching the InferencePool selectors.
**Note** : The allowlist tracks **hosts/IPs only** (not specific ports), so any port on an allowed host is permitted. The `x-prefiller-host-port` header should be in `host:port` format.
To enable SSRF protection:

```
# Using environment variables (recommended for Kubernetes deployments)
export INFERENCE_POOL_NAMESPACE=your-namespace
export INFERENCE_POOL_NAME=your-inference-pool
./bin/llm-d-routing-sidecar -enable-ssrf-protection=true

# Using command line flags (overrides environment variables)
./bin/llm-d-routing-sidecar -enable-ssrf-protection=true -inference-pool-namespace=your-namespace -inference-pool-name=your-inference-pool
```

#### Kubernetes Deployment with Downward API
For automatic namespace injection in Kubernetes deployments, use the downward API:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: llm-d-routing-sidecar
spec:
  template:
    spec:
      containers:
      - name: sidecar
        image: quay.io/llm-d/llm-d-routing-sidecar:latest
        args:
        - "-enable-ssrf-protection=true"
        env:
        - name: INFERENCE_POOL_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: INFERENCE_POOL_NAME
          value: "my-inference-pool"  # Set to your specific InferencePool name
```

When SSRF protection is enabled:
  * Only prefill targets that match **hosts/IPs** from pods in the specified InferencePool resource are allowed (any port on allowed hosts)
  * Requests to unauthorized targets return HTTP 403 Forbidden
  * The allowlist is automatically updated when pods are added/removed/updated
  * When disabled (default), all targets are allowed for backward compatibility


## Getting Started
### Requirements
  * a container engine (docker, podman, etc...)
  * a server with at least 2 GPUs


### Quick Start (From source)
  1. Start two vLLM servers with P/D enabled via the NIXLConnector.


In one terminal, run this command to start the decoder:

```
$ podman run --network host --device nvidia.com/gpu=0 -v $HOME/models:/models \
    -e UCX_TLS="cuda_ipc,cuda_copy,tcp" \
    -e VLLM_NIXL_SIDE_CHANNEL_PORT=5555 \
    -e VLLM_NIXL_SIDE_CHANNEL_HOST=localhost \
    -e VLLM_LOGGING_LEVEL=DEBUG \
    -e HF_HOME=/models ghcr.io/llm-d/llm-d:0.0.8 --model Qwen/Qwen3-0.6B \
    --enforce-eager \
    --port 8001 \
    --kv-transfer-config='{"kv_connector":"NixlConnector","kv_role":"kv_both"}'

```

In another terminal, run this command to start the prefiller:

```
$ podman run --network host --device nvidia.com/gpu=1 -v $HOME/models:/models \
    -e UCX_TLS="cuda_ipc,cuda_copy,tcp" \
    -e VLLM_NIXL_SIDE_CHANNEL_PORT=5556 \
    -e VLLM_NIXL_SIDE_CHANNEL_HOST=localhost \
    -e VLLM_LOGGING_LEVEL=DEBUG \
    -e HF_HOME=/models ghcr.io/llm-d/llm-d:0.0.8 --model Qwen/Qwen3-0.6B \
    --enforce-eager \
    --port 8002 \
    --kv-transfer-config='{"kv_connector":"NixlConnector","kv_role":"kv_both"}'

```

  1. Clone and start the routing proxy.


In another terminal, clone this repository and build the routing proxy:

```
$ git clone https://github.com/llm-d/llm-d-routing-sidecar.git && \
  cd llm-d-routing-sidecar && \
  make build

```

In the same terminal, start the routing proxy:

```
$ ./bin/llm-d-routing-sidecar -port=8000 -vllm-port=8001 -connector=nixlv2

```

  1. Send a request.


Finally, in another terminal, send a chat completions request to the router proxy on port 8000 and tell it to use the prefiller on port 8002:

```
$ curl  http://localhost:8000/v1/completions \
      -H "Content-Type: application/json" \
      -H "x-prefiller-host-port: http://localhost:8002" \
       -d '{
        "model": "Qwen/Qwen3-0.6B",
        "prompt": "Author-contribution statements and acknowledgements in research papers should state clearly and specifically whether, and to what extent, the authors used AI technologies such as ChatGPT in the preparation of their manuscript and analysis. They should also indicate which LLMs were used. This will alert editors and reviewers to scrutinize manuscripts more carefully for potential biases, inaccuracies and improper source crediting. Likewise, scientific journals should be transparent about their use of LLMs, for example when selecting submitted manuscripts. Mention the large language model based product mentioned in the paragraph above:"
      }'

```

  1. Verification


Observe the request is processed by both the prefiller and then the decoder:
_Prefiller logs (trimmed for clarity purpose):_

```
...
Received request cmpl-386a7566-31a0-11f0-b783-0200017aca02-0: prompt: ...
...
NIXLConnector request_finished, request_status=7 ...

```

_Decoder logs (trimmed for clarity purpose):_

```
...
Received request cmpl-386a7566-31a0-11f0-b783-0200017aca02-0: prompt: ...
...
DEBUG 05-15 15:21:06 [nixl_connector.py:646] start_load_kv for request cmpl-386a7566-31a0-11f0-b783-0200017aca02-0 from remote engine c53ac287-36cd-4b52-811f-16de4e4bd3a5. Num local_block_ids: 6. Num remote_block_ids: 6
...

```

## Development
### Building the routing proxy
Build the routing proxy from source:

```
$ make build
```

Check the build was successful by running it locally:

```
$ ./bin/llm-d-routing-sidecar -help
Usage of ./bin/llm-d-routing-sidecar:
  -add_dir_header
        If true, adds the file directory to the header of the log messages
  -alsologtostderr
        log to standard error as well as files (no effect when -logtostderr=true)
  -cert-path string
        The path to the certificate for secure proxy. The certificate and private key files are assumed to be named tls.crt and tls.key, respectively. If not set, and secureProxy is enabled, then a self-signed certificate is used (for testing).
  -connector string
        the P/D connector being used. Either nixl, nixlv2 or lmcache (default "nixl")
  -decoder-tls-insecure-skip-verify
        configures the proxy to skip TLS verification for requests to decoder
  -decoder-use-tls
        whether to use TLS when sending requests to the decoder
  -enable-ssrf-protection
        enable SSRF protection using InferencePool allowlisting
  -inference-pool-name string
        the specific InferencePool name to watch (defaults to INFERENCE_POOL_NAME env var)
  -inference-pool-namespace string
        the Kubernetes namespace to watch for InferencePool resources (defaults to INFERENCE_POOL_NAMESPACE env var)
  -log_backtrace_at value
        when logging hits line file:N, emit a stack trace
  -log_dir string
        If non-empty, write log files in this directory (no effect when -logtostderr=true)
  -log_file string
        If non-empty, use this log file (no effect when -logtostderr=true)
  -log_file_max_size uint
        Defines the maximum size a log file can grow to (no effect when -logtostderr=true). Unit is megabytes. If the value is 0, the maximum file size is unlimited. (default 1800)
  -logtostderr
        log to standard error instead of files (default true)
  -one_output
        If true, only write logs to their native severity level (vs also writing to each lower severity level; no effect when -logtostderr=true)
  -port string
        the port the sidecar is listening on (default "8000")
  -prefiller-tls-insecure-skip-verify
        configures the proxy to skip TLS verification for requests to prefiller
  -prefiller-use-tls
        whether to use TLS when sending requests to prefillers
  -secure-proxy
        Enables secure proxy. Defaults to true. (default true)
  -skip_headers
        If true, avoid header prefixes in the log messages
  -skip_log_headers
        If true, avoid headers when opening log files (no effect when -logtostderr=true)
  -stderrthreshold value
        logs at or above this threshold go to stderr when writing to files and stderr (no effect when -logtostderr=true or -alsologtostderr=true) (default 2)
  -v value
        number for the log level verbosity
  -vllm-port string
        the port vLLM is listening on (default "8001")
  -vmodule value
        comma-separated list of pattern=N settings for file-filtered logging

```

> **Note:** lmcache and nixl connectors are deprecated. Use nixlv2
## License
This project is licensed under the Apache License 2.0. See the [LICENSE](https://github.com/llm-d/llm-d-routing-sidecar/blob/main/LICENSE) file for details.
## About
Incubating P/D sidecar for llm-d
### Topics
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**17** stars
### Watchers
**27** watching
### Forks
[ forks](https://github.com/llm-d/llm-d-routing-sidecar/forks)
## 
[ v0.3.0Latest Sep 25, 2025 ](https://github.com/llm-d/llm-d-routing-sidecar/releases/tag/v0.3.0)
## 
## Languages
  * [Go81.8%](https://github.com/llm-d/llm-d-routing-sidecar/search?l=go)
  * [Makefile16.1%](https://github.com/llm-d/llm-d-routing-sidecar/search?l=makefile)
  * [Dockerfile1.3%](https://github.com/llm-d/llm-d-routing-sidecar/search?l=dockerfile)
  * [Shell0.8%](https://github.com/llm-d/llm-d-routing-sidecar/search?l=shell)


You can’t perform that action at this time. 
