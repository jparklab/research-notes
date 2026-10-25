---
title: semantic-conventions-genai/docs/gen-ai/gen-ai-metrics.md at main · open-telemetry/semantic-conventions-genai
  · GitHub
id: semantic-conventions-genaidocsgen-aigen-ai-metricsmd-at-main-open-telemetryseman
tags:
- observability
- opentelemetry
- genai-semconv
created: '2026-10-09T23:16:43.997677Z'
updated: '2026-10-10T14:52:17.250080Z'
source: https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md
source_domain: github.com
fetched_at: '2026-10-09T23:16:43.976465Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'OTel GenAI metrics spec (docs/gen-ai/gen-ai-metrics.md, 820 lines): document
  Status is explicitly ''Development'' (pre-Experimental maturity), same as the spans
  doc — confirms the whole GenAI semconv surface is still unstable as of Oct 2026.
  Explicit disclaimer: ''These are initial Generative AI client metric instruments
  and attributes but more may be added in the future.'' Defines four metric families:
  (1) client-side gen_ai.client.operation.duration (non-inference operations only
  — inference duration moved to a separate gen_ai.client.inference.duration per a
  recent refactor); (2) MODEL-SERVER metrics — gen_ai.server.request.duration, gen_ai.server.time_per_output_token,
  gen_ai.server.time_to_first_token — i.e. engine-level latency/TTFT/TPOT histograms
  with standard bucket boundaries [0.01s...81.92s]; (3) workflow metrics (gen_ai.invoke_workflow.duration);
  (4) agent and tool metrics (invoke_agent.duration/inference_calls/tool_calls, execute_tool.duration).
  Notably absent: no engine-internal metrics (KV-cache utilization, queue depth, batch/running-request
  counts, GPU occupancy) — the standardized surface stops at request-level duration/token-timing,
  which is consistent with inference engines like vLLM needing engine-native Prometheus
  exporters (and operators like Netflix needing custom proxies) for anything below
  that line.'
---

*Suggested by [[github-open-telemetrysemantic-conventions-genai-github]] — repo's primary docs page for metrics conventions; need stability status*

[Skip to content](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md) to refresh your session. Dismiss alert
##  Expand file tree
/
# gen-ai-metrics.md
Copy path
More file actions
More file actions
## Latest commit
## History
[History](https://github.com/open-telemetry/semantic-conventions-genai/commits/main/docs/gen-ai/gen-ai-metrics.md)
History
820 lines (616 loc) · 63.5 KB
/
# gen-ai-metrics.md
Copy path
## File metadata and controls
  * Preview
  * 

820 lines (616 loc) · 63.5 KB
Copy raw file
Download raw file
Outline
Edit and raw actions
# Semantic conventions for generative AI metrics
**Status** : [Development](https://opentelemetry.io/docs/specs/otel/document-status)
  * [Generative AI client metrics](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#generative-ai-client-metrics)
    * [Metric: `gen_ai.client.operation.duration`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiclientoperationduration)
  * [Generative AI model server metrics](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#generative-ai-model-server-metrics)
    * [Metric: `gen_ai.server.request.duration`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiserverrequestduration)
    * [Metric: `gen_ai.server.time_per_output_token`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiservertime_per_output_token)
    * [Metric: `gen_ai.server.time_to_first_token`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiservertime_to_first_token)
  * [Generative AI workflow metrics](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#generative-ai-workflow-metrics)
    * [Metric: `gen_ai.invoke_workflow.duration`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiinvoke_workflowduration)
  * [Generative AI agent metrics](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#generative-ai-agent-metrics)
    * [Metric: `gen_ai.invoke_agent.duration`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiinvoke_agentduration)
    * [Metric: `gen_ai.invoke_agent.inference_calls`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiinvoke_agentinference_calls)
    * [Metric: `gen_ai.invoke_agent.tool_calls`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiinvoke_agenttool_calls)
  * [Generative AI tool metrics](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#generative-ai-tool-metrics)
    * [Metric: `gen_ai.execute_tool.duration`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#metric-gen_aiexecute_toolduration)


## Generative AI client metrics
The conventions described in this section are specific to Generative AI client applications.
For inference metrics (duration, time-to-first-chunk, time-per-output-chunk), see [Client Inference](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/client-inference.md#metrics).
**Disclaimer:** These are initial Generative AI client metric instruments and attributes but more may be added in the future.
The following metric instruments describe Generative AI operations. An operation may be a request to an LLM, a function call, or some other distinct action within a larger Generative AI workflow.
Individual systems may include additional system-specific attributes. It is recommended to check system-specific documentation, if available.
Inference token metrics (`gen_ai.client.inference.usage.*` counters and `gen_ai.client.inference.operation.*` histograms) are defined in [Inference Token Metrics](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-token-metrics.md).
### Metric: `gen_ai.client.operation.duration`
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [0.01, 0.02, 0.04, 0.08, 0.16, 0.32, 0.64, 1.28, 2.56, 5.12, 10.24, 20.48, 40.96, 81.92].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.client.operation.duration`  | Histogram  | GenAI operation duration. [1]  |  
**[1]:** This metric SHOULD NOT be reported for inference operations; use `gen_ai.client.inference.duration` instead.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [2]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
|  `Conditionally Required` [3]  | string  | The Generative AI provider as identified by the client or server instrumentation. [4]  |  `openai`; `gcp.gen_ai`; `gcp.vertex_ai`  |  
|  `Conditionally Required` If available.  | string  | The name of the GenAI model a request is being made to. [5]  | `gpt-4`  |  
|  `Conditionally Required` If `server.address` is set.  | int  | GenAI server port. [6]  |  `80`; `8080`; `443`  |  
| `Recommended`  | string  | The name of the model that generated the response.  | `gpt-4-0613`  |  
| `Recommended`  | string  | GenAI server address. [7]  |  `example.com`; `10.1.2.80`; `/tmp/my.sock`  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[3]`gen_ai.provider.name` :** If the operation involves a call to a GenAI provider.
**[4]`gen_ai.provider.name` :** Semantic conventions for individual GenAI operations SHOULD clarify which kinds of providers (e.g. inference, embeddings, retrieval, memory, hosted agent providers) apply when it is not clear from context.
The attribute SHOULD be set based on the instrumentation's best knowledge and may differ from the actual upstream provider. For example, a client SDK may be configured against a proxy or hosting platform that transparently relays requests to a different provider.
The `gen_ai.provider.name` attribute acts as a discriminator that identifies the GenAI telemetry format flavor specific to that provider within GenAI semantic conventions. It SHOULD be set consistently with provider-specific attributes and signals. For example, GenAI spans, metrics, and events related to AWS Bedrock should have the `gen_ai.provider.name` set to `aws.bedrock` and include applicable `aws.bedrock.*` attributes and are not expected to include `openai.*` attributes.
**[5]`gen_ai.request.model` :** The name of the GenAI model a request is being made to. If the model is supplied by a vendor, then the value must be the exact name of the model requested. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.
**[6]`server.port` :** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.
**[7]`server.address` :** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.
`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `_OTHER`  | A fallback error value to be used when the instrumentation doesn't define a custom value.  |  
`gen_ai.operation.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `chat`  | Chat completion operation such as [OpenAI Chat API](https://platform.openai.com/docs/api-reference/chat)  |  
| `create_agent`  | Create GenAI agent  |  
| `create_memory`  | Create new memory records  |  
| `create_memory_store`  | Create or initialize a memory store  |  
| `delete_memory`  | Delete memory records  |  
| `delete_memory_store`  | Delete or deprovision a memory store  |  
| `embeddings`  | Embeddings operation such as [OpenAI Create embeddings API](https://platform.openai.com/docs/api-reference/embeddings/create)  |  
| `execute_tool`  | Execute a tool  |  
| `fetch_response`  | Fetch a previously generated model response by its identifier, without performing inference, such as [OpenAI Get a model response](https://platform.openai.com/docs/api-reference/responses/get) [8]  |  
| `generate_content`  | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content)  |  
| `invoke_agent`  | Invoke GenAI agent  |  
| `invoke_workflow`  | Invoke GenAI workflow  |  
| `plan`  | Agent planning or task decomposition phase  |  
| `retrieval`  | Retrieval operation such as [OpenAI Search Vector Store API](https://platform.openai.com/docs/api-reference/vector-stores/search)  |  
| `search_memory`  | Search/query memories from a memory store  |  
| `text_completion`  | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions)  |  
| `update_memory`  | Update existing memory records  |  
| `upsert_memory`  | Create or update memory records without the caller choosing which  |  
**[8]:** Instrumentations SHOULD NOT report token usage (as attributes or metrics) for this operation.
`gen_ai.provider.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `anthropic`  |  
| `aws.bedrock`  |  
| `azure.ai.inference`  | Azure AI Inference  |  
| `azure.ai.openai`  |  
| `cohere`  |  
| `deepseek`  |  
| `gcp.gemini`  |  
| `gcp.gen_ai`  | Any Google generative AI endpoint [10]  |  
| `gcp.vertex_ai`  |  [Vertex AI](https://cloud.google.com/vertex-ai) [11]  |  
| `groq`  |  
| `ibm.watsonx.ai`  |  
| `mistral_ai`  |  
| `moonshot_ai`  |  
| `openai`  |  
| `perplexity`  |  
| `x_ai`  |  
**[9]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.
**[10]:** May be used when specific backend is unknown.
**[11]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.
## Generative AI model server metrics
The following metric instruments describe Generative AI model servers' operational metrics. It includes both functional and performance metrics.
### Metric: `gen_ai.server.request.duration`
This metric reports the model server latency in terms of time spent per request.
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [0.01, 0.02, 0.04, 0.08, 0.16, 0.32, 0.64, 1.28, 2.56, 5.12, 10.24, 20.48, 40.96, 81.92].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.server.request.duration`  | Histogram  | Generative AI server request duration such as time-to-last byte or last output token.  |  
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
| `Required`  | string  | The Generative AI provider as identified by the client or server instrumentation. [2]  |  `openai`; `gcp.gen_ai`; `gcp.vertex_ai`  |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [3]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
|  `Conditionally Required` If available.  | string  | The name of the GenAI model a request is being made to. [4]  | `gpt-4`  |  
|  `Conditionally Required` If `server.address` is set.  | int  | GenAI server port. [5]  |  `80`; `8080`; `443`  |  
| `Recommended`  | string  | The name of the model that generated the response.  | `gpt-4-0613`  |  
| `Recommended`  | string  | GenAI server address. [6]  |  `example.com`; `10.1.2.80`; `/tmp/my.sock`  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`gen_ai.provider.name` :** Semantic conventions for individual GenAI operations SHOULD clarify which kinds of providers (e.g. inference, embeddings, retrieval, memory, hosted agent providers) apply when it is not clear from context.
The attribute SHOULD be set based on the instrumentation's best knowledge and may differ from the actual upstream provider. For example, a client SDK may be configured against a proxy or hosting platform that transparently relays requests to a different provider.
The `gen_ai.provider.name` attribute acts as a discriminator that identifies the GenAI telemetry format flavor specific to that provider within GenAI semantic conventions. It SHOULD be set consistently with provider-specific attributes and signals. For example, GenAI spans, metrics, and events related to AWS Bedrock should have the `gen_ai.provider.name` set to `aws.bedrock` and include applicable `aws.bedrock.*` attributes and are not expected to include `openai.*` attributes.
**[3]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[4]`gen_ai.request.model` :** The name of the GenAI model a request is being made to. If the model is supplied by a vendor, then the value must be the exact name of the model requested. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.
**[5]`server.port` :** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.
**[6]`server.address` :** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.
`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `_OTHER`  | A fallback error value to be used when the instrumentation doesn't define a custom value.  |  
`gen_ai.operation.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `chat`  | Chat completion operation such as [OpenAI Chat API](https://platform.openai.com/docs/api-reference/chat)  |  
| `create_agent`  | Create GenAI agent  |  
| `create_memory`  | Create new memory records  |  
| `create_memory_store`  | Create or initialize a memory store  |  
| `delete_memory`  | Delete memory records  |  
| `delete_memory_store`  | Delete or deprovision a memory store  |  
| `embeddings`  | Embeddings operation such as [OpenAI Create embeddings API](https://platform.openai.com/docs/api-reference/embeddings/create)  |  
| `execute_tool`  | Execute a tool  |  
| `fetch_response`  | Fetch a previously generated model response by its identifier, without performing inference, such as [OpenAI Get a model response](https://platform.openai.com/docs/api-reference/responses/get) [7]  |  
| `generate_content`  | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content)  |  
| `invoke_agent`  | Invoke GenAI agent  |  
| `invoke_workflow`  | Invoke GenAI workflow  |  
| `plan`  | Agent planning or task decomposition phase  |  
| `retrieval`  | Retrieval operation such as [OpenAI Search Vector Store API](https://platform.openai.com/docs/api-reference/vector-stores/search)  |  
| `search_memory`  | Search/query memories from a memory store  |  
| `text_completion`  | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions)  |  
| `update_memory`  | Update existing memory records  |  
| `upsert_memory`  | Create or update memory records without the caller choosing which  |  
**[7]:** Instrumentations SHOULD NOT report token usage (as attributes or metrics) for this operation.
`gen_ai.provider.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `anthropic`  |  
| `aws.bedrock`  |  
| `azure.ai.inference`  | Azure AI Inference  |  
| `azure.ai.openai`  |  
| `cohere`  |  
| `deepseek`  |  
| `gcp.gemini`  |  
| `gcp.gen_ai`  | Any Google generative AI endpoint [9]  |  
| `gcp.vertex_ai`  |  [Vertex AI](https://cloud.google.com/vertex-ai) [10]  |  
| `groq`  |  
| `ibm.watsonx.ai`  |  
| `mistral_ai`  |  
| `moonshot_ai`  |  
| `openai`  |  
| `perplexity`  |  
| `x_ai`  |  
**[8]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.
**[9]:** May be used when specific backend is unknown.
**[10]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.
### Metric: `gen_ai.server.time_per_output_token`
This metric reports the model server latency in terms of time per token generated after the first token for any model servers which support serving LLMs. It is measured by subtracting the time taken to generate the first output token from the request duration and dividing the rest of the duration by the number of output tokens generated after the first token. This is important in measuring the performance of the decode phase of LLM inference.
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [0.01, 0.025, 0.05, 0.075, 0.1, 0.15, 0.2, 0.3, 0.4, 0.5, 0.75, 1.0, 2.5].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.server.time_per_output_token`  | Histogram  | Time per output token generated after the first token for successful responses.  |  
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
| `Required`  | string  | The Generative AI provider as identified by the client or server instrumentation. [2]  |  `openai`; `gcp.gen_ai`; `gcp.vertex_ai`  |  
|  `Conditionally Required` If available.  | string  | The name of the GenAI model a request is being made to. [3]  | `gpt-4`  |  
|  `Conditionally Required` If `server.address` is set.  | int  | GenAI server port. [4]  |  `80`; `8080`; `443`  |  
| `Recommended`  | string  | The name of the model that generated the response.  | `gpt-4-0613`  |  
| `Recommended`  | string  | GenAI server address. [5]  |  `example.com`; `10.1.2.80`; `/tmp/my.sock`  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`gen_ai.provider.name` :** Semantic conventions for individual GenAI operations SHOULD clarify which kinds of providers (e.g. inference, embeddings, retrieval, memory, hosted agent providers) apply when it is not clear from context.
The attribute SHOULD be set based on the instrumentation's best knowledge and may differ from the actual upstream provider. For example, a client SDK may be configured against a proxy or hosting platform that transparently relays requests to a different provider.
The `gen_ai.provider.name` attribute acts as a discriminator that identifies the GenAI telemetry format flavor specific to that provider within GenAI semantic conventions. It SHOULD be set consistently with provider-specific attributes and signals. For example, GenAI spans, metrics, and events related to AWS Bedrock should have the `gen_ai.provider.name` set to `aws.bedrock` and include applicable `aws.bedrock.*` attributes and are not expected to include `openai.*` attributes.
**[3]`gen_ai.request.model` :** The name of the GenAI model a request is being made to. If the model is supplied by a vendor, then the value must be the exact name of the model requested. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.
**[4]`server.port` :** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.
**[5]`server.address` :** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.
`gen_ai.operation.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `chat`  | Chat completion operation such as [OpenAI Chat API](https://platform.openai.com/docs/api-reference/chat)  |  
| `create_agent`  | Create GenAI agent  |  
| `create_memory`  | Create new memory records  |  
| `create_memory_store`  | Create or initialize a memory store  |  
| `delete_memory`  | Delete memory records  |  
| `delete_memory_store`  | Delete or deprovision a memory store  |  
| `embeddings`  | Embeddings operation such as [OpenAI Create embeddings API](https://platform.openai.com/docs/api-reference/embeddings/create)  |  
| `execute_tool`  | Execute a tool  |  
| `fetch_response`  | Fetch a previously generated model response by its identifier, without performing inference, such as [OpenAI Get a model response](https://platform.openai.com/docs/api-reference/responses/get) [6]  |  
| `generate_content`  | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content)  |  
| `invoke_agent`  | Invoke GenAI agent  |  
| `invoke_workflow`  | Invoke GenAI workflow  |  
| `plan`  | Agent planning or task decomposition phase  |  
| `retrieval`  | Retrieval operation such as [OpenAI Search Vector Store API](https://platform.openai.com/docs/api-reference/vector-stores/search)  |  
| `search_memory`  | Search/query memories from a memory store  |  
| `text_completion`  | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions)  |  
| `update_memory`  | Update existing memory records  |  
| `upsert_memory`  | Create or update memory records without the caller choosing which  |  
**[6]:** Instrumentations SHOULD NOT report token usage (as attributes or metrics) for this operation.
`gen_ai.provider.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `anthropic`  |  
| `aws.bedrock`  |  
| `azure.ai.inference`  | Azure AI Inference  |  
| `azure.ai.openai`  |  
| `cohere`  |  
| `deepseek`  |  
| `gcp.gemini`  |  
| `gcp.gen_ai`  | Any Google generative AI endpoint [8]  |  
| `gcp.vertex_ai`  |  [Vertex AI](https://cloud.google.com/vertex-ai) [9]  |  
| `groq`  |  
| `ibm.watsonx.ai`  |  
| `mistral_ai`  |  
| `moonshot_ai`  |  
| `openai`  |  
| `perplexity`  |  
| `x_ai`  |  
**[7]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.
**[8]:** May be used when specific backend is unknown.
**[9]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.
### Metric: `gen_ai.server.time_to_first_token`
This metric reports the model server latency in terms of time spent to generate the first token of the response for any model servers which support serving LLMs. It helps measure the time spent in the queue and the prefill phase. It is important especially for streaming requests. It is calculated at a request level and is reported as a histogram using the buckets mentioned below.
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [0.001, 0.005, 0.01, 0.02, 0.04, 0.06, 0.08, 0.1, 0.25, 0.5, 0.75, 1.0, 2.5, 5.0, 7.5, 10.0].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.server.time_to_first_token`  | Histogram  | Time to generate first token for successful responses.  |  
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
| `Required`  | string  | The Generative AI provider as identified by the client or server instrumentation. [2]  |  `openai`; `gcp.gen_ai`; `gcp.vertex_ai`  |  
|  `Conditionally Required` If available.  | string  | The name of the GenAI model a request is being made to. [3]  | `gpt-4`  |  
|  `Conditionally Required` If `server.address` is set.  | int  | GenAI server port. [4]  |  `80`; `8080`; `443`  |  
| `Recommended`  | string  | The name of the model that generated the response.  | `gpt-4-0613`  |  
| `Recommended`  | string  | GenAI server address. [5]  |  `example.com`; `10.1.2.80`; `/tmp/my.sock`  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`gen_ai.provider.name` :** Semantic conventions for individual GenAI operations SHOULD clarify which kinds of providers (e.g. inference, embeddings, retrieval, memory, hosted agent providers) apply when it is not clear from context.
The attribute SHOULD be set based on the instrumentation's best knowledge and may differ from the actual upstream provider. For example, a client SDK may be configured against a proxy or hosting platform that transparently relays requests to a different provider.
The `gen_ai.provider.name` attribute acts as a discriminator that identifies the GenAI telemetry format flavor specific to that provider within GenAI semantic conventions. It SHOULD be set consistently with provider-specific attributes and signals. For example, GenAI spans, metrics, and events related to AWS Bedrock should have the `gen_ai.provider.name` set to `aws.bedrock` and include applicable `aws.bedrock.*` attributes and are not expected to include `openai.*` attributes.
**[3]`gen_ai.request.model` :** The name of the GenAI model a request is being made to. If the model is supplied by a vendor, then the value must be the exact name of the model requested. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.
**[4]`server.port` :** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.
**[5]`server.address` :** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.
`gen_ai.operation.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `chat`  | Chat completion operation such as [OpenAI Chat API](https://platform.openai.com/docs/api-reference/chat)  |  
| `create_agent`  | Create GenAI agent  |  
| `create_memory`  | Create new memory records  |  
| `create_memory_store`  | Create or initialize a memory store  |  
| `delete_memory`  | Delete memory records  |  
| `delete_memory_store`  | Delete or deprovision a memory store  |  
| `embeddings`  | Embeddings operation such as [OpenAI Create embeddings API](https://platform.openai.com/docs/api-reference/embeddings/create)  |  
| `execute_tool`  | Execute a tool  |  
| `fetch_response`  | Fetch a previously generated model response by its identifier, without performing inference, such as [OpenAI Get a model response](https://platform.openai.com/docs/api-reference/responses/get) [6]  |  
| `generate_content`  | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content)  |  
| `invoke_agent`  | Invoke GenAI agent  |  
| `invoke_workflow`  | Invoke GenAI workflow  |  
| `plan`  | Agent planning or task decomposition phase  |  
| `retrieval`  | Retrieval operation such as [OpenAI Search Vector Store API](https://platform.openai.com/docs/api-reference/vector-stores/search)  |  
| `search_memory`  | Search/query memories from a memory store  |  
| `text_completion`  | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions)  |  
| `update_memory`  | Update existing memory records  |  
| `upsert_memory`  | Create or update memory records without the caller choosing which  |  
**[6]:** Instrumentations SHOULD NOT report token usage (as attributes or metrics) for this operation.
`gen_ai.provider.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `anthropic`  |  
| `aws.bedrock`  |  
| `azure.ai.inference`  | Azure AI Inference  |  
| `azure.ai.openai`  |  
| `cohere`  |  
| `deepseek`  |  
| `gcp.gemini`  |  
| `gcp.gen_ai`  | Any Google generative AI endpoint [8]  |  
| `gcp.vertex_ai`  |  [Vertex AI](https://cloud.google.com/vertex-ai) [9]  |  
| `groq`  |  
| `ibm.watsonx.ai`  |  
| `mistral_ai`  |  
| `moonshot_ai`  |  
| `openai`  |  
| `perplexity`  |  
| `x_ai`  |  
**[7]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.
**[8]:** May be used when specific backend is unknown.
**[9]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.
## Generative AI workflow metrics
### Metric: `gen_ai.invoke_workflow.duration`
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [1, 5, 10, 30, 60, 120, 300, 600, 1800, 3600, 7200].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.invoke_workflow.duration`  | Histogram  | Records duration of GenAI workflow. [1]  |  
**[1]:** Represents the end-to-end duration of a workflow execution, measured from the point where application code initiates the workflow to the point where the workflow completes, independent of workflow complexity. A workflow is a coordinated process composed of multiple agents or other operations involving generative AI. If instrumentation measures only a single provider-facing client operation (for example, one model API call), `gen_ai.client.operation.duration` SHOULD be used instead. Instrumentation MAY emit both metrics for the same request path when both boundaries are available. When this metric is reported alongside a `gen_ai.invoke_workflow.internal` span, the metric value SHOULD be the same as the span duration. Individual systems may include additional system-specific attributes. Refer to system-specific documentation, if available.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [1]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
|  `Conditionally Required` If available.  | string  | Human-readable name of the GenAI workflow provided by the application. [2]  |  `multi_agent_rag`; `customer_support_pipeline`  |  
**[1]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[2]`gen_ai.workflow.name` :** The workflow name is usually a static, application-unique identifier defined in a framework-specific way.
For example, it can be the name of the first chain in LangChain, the name of the crew in CrewAI, or the entry point agent in ADK or OpenAI Agents when no explicit workflow name is provided.
This attribute MUST have low cardinality. It is NOT RECOMMENDED to use instrumentation-time constants or names of types representing the workflow, such as "StateGraph". When no meaningful, low-cardinality workflow name is available for a given framework, this attribute MUST NOT be captured by default.
Semantic conventions for individual Generative AI frameworks SHOULD document what `gen_ai.workflow.name` means in the context of that framework.
`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `_OTHER`  | A fallback error value to be used when the instrumentation doesn't define a custom value.  |  
## Generative AI agent metrics
Individual systems may include additional system-specific attributes. It is recommended to check system-specific documentation, if available.
### Metric: `gen_ai.invoke_agent.duration`
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [0.1, 0.2, 0.4, 0.8, 1.6, 3.2, 6.4, 12.8, 25.6, 51.2, 102.4, 204.8, 409.6].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.invoke_agent.duration`  | Histogram  | The end-to-end duration of a single in-process agent invocation, from the moment the invocation starts until the agent emits the last chunk of its final response or terminates with an error. [1]  |  
**[1]:** Intended for instrumentations of agent frameworks (for example, ADK, LangChain agents, CrewAI agents) that can reliably bound a single agent invocation.
If instrumentation can only measure a single provider-facing client operation (for example, one model API call), `gen_ai.client.operation.duration` SHOULD be used instead. If instrumentation can reliably bound a higher-level workflow that coordinates multiple agents, `gen_ai.invoke_workflow.duration` SHOULD be used for that workflow. Instrumentation MAY emit several of these metrics for the same request path when more than one boundary is available.
When this metric is reported alongside a `gen_ai.invoke_agent.internal` span, the metric value SHOULD be the same as the span duration.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [1]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
|  `Conditionally Required` When available.  | string  | The human-readable name of the invoked GenAI agent.  |  `Math Tutor`; `Fiction Writer`  |  
|  `Recommended` If applicable.  | string  | The name of the GenAI model configured for the agent. [2]  | `gpt-4`  |  
**[1]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[2]`gen_ai.request.model` :** This attribute SHOULD be populated if and only if the instrumented library allows to set only a single model per agent. It SHOULD NOT be populated for agents that support multiple models or dynamic selection.
`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `_OTHER`  | A fallback error value to be used when the instrumentation doesn't define a custom value.  |  
### Metric: `gen_ai.invoke_agent.inference_calls`
This metric is [recommended][MetricRecommended] for instrumentations of agent frameworks that can reliably attribute inference (model) calls to a single agent invocation.
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [1, 2, 4, 8, 16, 32, 64, 128].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.invoke_agent.inference_calls`  | Histogram  | `{inference_call}`  | The number of inference (model) calls a GenAI agent makes during a single invocation. [1]  |  
**[1]:** The distribution is scoped to a single agent invocation and SHOULD include only the inference calls the agent itself issued including failed ones; calls made by sub-agents or transferred-to agents are recorded against those agents' own invocations so that each inference call is counted exactly once across the call tree.
This metric SHOULD be emitted together with the `gen_ai.invoke_agent.internal` span for the same invocation.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Recommended`  | string  | Human-readable name of the GenAI agent provided by the application.  |  `Math Tutor`; `Fiction Writer`  |  
### Metric: `gen_ai.invoke_agent.tool_calls`
This metric is [recommended][MetricRecommended] for instrumentations of agent frameworks that can reliably attribute tool calls to a single agent invocation.
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [1, 2, 4, 8, 16, 32, 64, 128].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.invoke_agent.tool_calls`  | Histogram  | `{tool_call}`  | The number of tool calls a GenAI agent makes during a single invocation. [1]  |  
**[1]:** The distribution is scoped to a single agent invocation and SHOULD include only the tool calls the agent itself triggers including failed ones; calls made by sub-agents or transferred-to agents are recorded against those agents' own invocations so that each tool call is counted exactly once across the call tree.
Only client-side tool calls (tools executed by the agent or framework) are counted. Tools executed server-side by the model provider (for example, provider built-in web search or code execution) are not counted here.
This metric SHOULD be emitted together with the `gen_ai.invoke_agent.internal` span for the same invocation.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Recommended`  | string  | Human-readable name of the GenAI agent provided by the application.  |  `Math Tutor`; `Fiction Writer`  |  
## Generative AI tool metrics
Individual systems may include additional system-specific attributes. It is recommended to check system-specific documentation, if available.
### Metric: `gen_ai.execute_tool.duration`
This metric SHOULD be specified with [ExplicitBucketBoundaries](https://github.com/open-telemetry/opentelemetry-specification/blob/v1.55.0/specification/metrics/api.md#instrument-advisory-parameters) of [0.01, 0.02, 0.04, 0.08, 0.16, 0.32, 0.64, 1.28, 2.56, 5.12, 10.24, 20.48, 40.96, 81.92].  
| Name  | Instrument Type  | Unit (UCUM)  | Description  | Stability  | Entity Associations  |  
| --- | --- | --- | --- | --- | --- |  
| `gen_ai.execute_tool.duration`  | Histogram  | The duration of a single tool execution. [1]  |  
**[1]:** Instrumentation that can reliably bound a single tool call SHOULD record this metric for every tool execution they can observe.
When this metric is reported alongside a `gen_ai.execute_tool` span, the metric value SHOULD be the same as the span duration.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | Name of the tool utilized by the agent.  | `Flights`  |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [1]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
|  `Conditionally Required` When applicable.  | string  | The human-readable name of the agent executing the tool.  |  `Math Tutor`; `Fiction Writer`  |  
|  `Recommended` If available.  | string  | Type of the tool utilized by the agent [2]  |  `function`; `extension`; `datastore`  |  
**[1]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[2]`gen_ai.tool.type` :** Extension: A tool executed on the agent-side to directly call external APIs, bridging the gap between the agent and real-world systems. Agent-side operations involve actions that are performed by the agent on the server or within the agent's controlled environment. Function: A tool executed on the client-side, where the agent generates parameters for a predefined function, and the client executes the logic. Client-side operations are actions taken on the user's end or within the client application. Datastore: A tool used by the agent to access and query structured or unstructured external data for retrieval-augmented tasks or knowledge updates.
`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `_OTHER`  | A fallback error value to be used when the instrumentation doesn't define a custom value.  |  
You can’t perform that action at this time. 
