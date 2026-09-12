---
title: llm-d-router/pkg/epp/framework/plugins/requesthandling/parsers/README.md at
  main · llm-d/llm-d-router · GitHub
id: llm-d-routerpkgeppframeworkpluginsrequesthandlingparsersreadmemd-at-main-llm-dll
tags:
- llmd-router-nonk8s-backends-54b774
- epp-plugin-pattern
- request-parsers
- configuration
created: '2026-09-11T20:48:58.005704Z'
updated: '2026-09-12T03:37:37.683213Z'
source: https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requesthandling/parsers/README.md
source_domain: github.com
fetched_at: '2026-09-11T20:48:57.982553Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Upstream README (llm-d-router pkg/epp/framework/plugins/requesthandling/parsers,
  latest commit ebab6879, Aug 22 2026, PR #2507) documenting the request-handling
  parser-plugin pattern used for prefix-cache-aware scheduling and usage tracking.
  Parsers registered: openai-parser, anthropic-parser, vllmgrpc-parser, vllmhttp-parser
  (handles vLLMs non-OpenAI /inference/v1/generate path only), sglanghttp-parser (SGLang
  native /generate), vertexai-parser (Vertex AI PredictionService/ChatCompletions
  gRPC; unsupported Vertex APIs pass through unparsed to a random endpoint), and passthrough-parser
  (model-agnostic, claims no paths, must be listed last, forwards unparsed traffic
  without payload-derived scoring signals such as prefix-cache-scorer). Default parser
  list (if none specified) is openai-parser, anthropic-parser, vllmhttp-parser, passthrough-parser.
  Resolution uses longest-suffix path matching against each parsers claimed paths;
  registration/matching stops at the first parser claiming no paths, so any parser
  after a no-path fallback is skipped -- an explicit requestHandler.parsers list with
  no trailing fallback will 400-reject any path no listed parser claims, whereas relying
  on the default lists trailing passthrough-parser routes unclaimed paths through
  unparsed instead of rejecting. Gives three annotated EndpointPickerConfig YAML examples
  (vllmgrpc-only; vllmhttp+openai; openai+anthropic) showing how to register plugins
  and reference them in requestHandler.parsers. Directly informs plugin-registration
  pattern applicable to writing a custom EndpointDiscovery-style plugin: plugins are
  declared in a top-level plugins block with name+type, then referenced by name elsewhere
  in the EndpointPickerConfig.'
utility_score: 10.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requesthandling/parsers/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requesthandling/parsers/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requesthandling/parsers/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requesthandling/parsers/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requesthandling/parsers/README.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# README.md
Copy path
More file actions
More file actions
## Latest commit
[Add passthrough-parser to the default parser list (](https://github.com/llm-d/llm-d-router/commit/ebab687927f5331c1b39defcb579a07951bce56c)[#2507](https://github.com/llm-d/llm-d-router/pull/2507)[)](https://github.com/llm-d/llm-d-router/commit/ebab687927f5331c1b39defcb579a07951bce56c)
Open commit detailssuccess
Aug 22, 2026
[ebab687](https://github.com/llm-d/llm-d-router/commit/ebab687927f5331c1b39defcb579a07951bce56c) · Aug 22, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/requesthandling/parsers/README.md)
Open commit details
History
93 lines (76 loc) · 5.11 KB
Code owner: @zetxqx and @llm-d/router-maintainers
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

93 lines (76 loc) · 5.11 KB
Code owner: @zetxqx and @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Parsers
This directory contains parser plugins used to parse and understand the payloads of requests and responses. This understanding is key for empowering features like prefix-cache aware request scheduling and response usage tracking.
## Supported Parsers
  * **`openai-parser`**: A parser supporting the[OpenAI API](https://developers.openai.com/api/reference/overview). Along with `anthropic-parser` and `vllmhttp-parser`, it is registered by default if no parsers are explicitly specified.
  * **`anthropic-parser`**: A parser designed to handle requests for the[Anthropic Messages API](https://docs.anthropic.com/en/api/messages). It supports both standard JSON and streaming SSE responses.
  * **`vllmgrpc-parser`**: A parser designed to handle requests specifically for the[vLLM gRPC API](https://docs.vllm.ai/en/latest/api/vllm/entrypoints/grpc_server/).
  * **`vllmhttp-parser`**: A parser for vLLM HTTP endpoints that are not part of the OpenAI-compatible API surface — specifically`/inference/v1/generate` (which accepts pre-tokenized prompts and multimodal features). Because it only handles this path, you must also configure `openai-parser` if you want to support OpenAI-compatible paths on the same route.
  * **`sglanghttp-parser`**: A parser for SGLang's native HTTP`/generate` endpoint, which is not part of the OpenAI-compatible API. It currently supports a single pre-tokenized prompt or a batch of pre-tokenized prompts. Configure `openai-parser` alongside it if you also want to handle OpenAI-compatible paths on the same route.
  * **`vertexai-parser`**: A parser designed to handle requests for the Vertex AI gRPC API, specifically supporting[PredictionService/ChatCompletions](https://github.com/googleapis/googleapis/blob/89c3153888201c9e80bc5ec78d6ffca0debe6b52/google/cloud/aiplatform/v1beta1/prediction_service.proto#L235). For unsupported Vertex AI APIs, it skips parsing and lets the request pass through without interpretation resulting in routing to a random endpoint.
  * **`passthrough-parser`**: A model-agnostic parser that supports any request format by passing the request body through without interpretation. It claims no paths, so it acts as the fallback for unmatched traffic and is included last in the default parser list.
    * **Drawback** : EPP cannot parse the payload, so payload-related scheduling scorers (e.g., `prefix-cache-scorer`) are not supported.


### Serving mixed vLLM-specific and OpenAI-compatible traffic
`vllmhttp-parser` only parses the `/inference/v1/generate` path. To serve both vLLM-specific and OpenAI-compatible traffic on the same route, configure both `vllmhttp-parser` and `openai-parser` under the `requestHandler.parsers` list.
## Configuration
Parsers are configured via the `requestHandler.parsers` list in the `EndpointPickerConfig` YAML file. You must first instantiate the parser plugin in the `plugins` section, and then reference its name in the `requestHandler.parsers` list.
The EPP resolves incoming request paths to the matching parser using longest-suffix matching. Suffixes are defined by each parser plugin's claims. If a parser does not define specific paths, it acts as a fallback for any unmatched traffic.
Registration stops at the first parser claiming no paths. Any parser listed after it is skipped, so a fallback belongs last.
If no parsers are specified, `openai-parser`, `anthropic-parser`, `vllmhttp-parser`, and `passthrough-parser` are used by default. The trailing `passthrough-parser` means a path none of the others claim is forwarded to the model server without interpretation, and is scheduled without payload-derived signals, rather than rejected with `400`.
An explicit `requestHandler.parsers` list is used as given. It gains no fallback, so a path no listed parser claims is rejected. Add `passthrough-parser` as the last entry to forward that traffic instead.
Here is an example configuration using the `vllmgrpc-parser`:

```
apiVersion: llm-d.ai/v1alpha1
kind: EndpointPickerConfig
plugins:
- name: maxScore
  type: max-score-picker
- name: vllmgrpcParser
  type: vllmgrpc-parser
schedulingProfiles:
- name: default
  plugins:
  - pluginRef: maxScore
requestHandler:
  parsers:
  - pluginRef: vllmgrpcParser
```

Configuration using both `vllmhttp-parser` and `openai-parser` (enables `/inference/v1/generate` while keeping OpenAI-compatible paths working):

```
apiVersion: llm-d.ai/v1alpha1
kind: EndpointPickerConfig
plugins:
- name: maxScore
  type: max-score-picker
- name: vllmhttpParser
  type: vllmhttp-parser
- name: openaiParser
  type: openai-parser
schedulingProfiles:
- name: default
  plugins:
  - pluginRef: maxScore
requestHandler:
  parsers:
  - pluginRef: vllmhttpParser
  - pluginRef: openaiParser
```

Configuration with multiple parsers (e.g. OpenAI and Anthropic API support on the same route):

```
apiVersion: llm-d.ai/v1alpha1
kind: EndpointPickerConfig
plugins:
- name: maxScore
  type: max-score-picker
- name: openaiParser
  type: openai-parser
- name: anthropicParser
  type: anthropic-parser
schedulingProfiles:
- name: default
  plugins:
  - pluginRef: maxScore
requestHandler:
  parsers:
  - pluginRef: openaiParser
  - pluginRef: anthropicParser
```

You can’t perform that action at this time. 
