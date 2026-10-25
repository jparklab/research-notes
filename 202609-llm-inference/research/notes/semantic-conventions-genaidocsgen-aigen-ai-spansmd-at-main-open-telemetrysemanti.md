---
title: semantic-conventions-genai/docs/gen-ai/gen-ai-spans.md at main · open-telemetry/semantic-conventions-genai
  · GitHub
id: semantic-conventions-genaidocsgen-aigen-ai-spansmd-at-main-open-telemetrysemanti
tags:
- observability
- opentelemetry
- genai-semconv
created: '2026-10-09T23:16:36.458406Z'
updated: '2026-10-10T14:52:17.248631Z'
source: https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md
source_domain: github.com
fetched_at: '2026-10-09T23:16:36.437567Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'OTel GenAI spans spec (docs/gen-ai/gen-ai-spans.md, 1015 lines): document
  Status is explicitly ''Development'' (per OTel''s document-status taxonomy, a lower
  maturity tier than ''Experimental'' — meaning breaking changes can land at any time),
  NOT stable. Defines span conventions for embeddings, execute-tool, and inference
  operations (inference spans/metrics are factored into a separate client-inference.md
  doc as of an Oct 6 2026 refactor). Attribute-level requirement levels are granular
  (Required / Conditionally Required / Recommended) covering gen_ai.operation.name,
  gen_ai.provider.name, error.type, request model name, server.port, response model
  name, server.address. Also documents three content-capture modes: full buffered
  content on span attributes, uploading content to external storage, and streaming-chunk
  capture. The gen_ai.provider.name attribute is explicitly designed to discriminate
  telemetry ''flavor'' even when a client is configured against a proxy that transparently
  relays to a different upstream provider — directly relevant to gateway/proxy-layer
  tracing.'
---

*Suggested by [[github-open-telemetrysemantic-conventions-genai-github]] — repo's primary docs page for span conventions; need stability status*

[Skip to content](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md) to refresh your session. Dismiss alert
## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# gen-ai-spans.md
Copy path
More file actions
More file actions
## Latest commit
3 people
[Use `gen_ai.client.inference.duration` metric instead of `gen_ai.clie…](https://github.com/open-telemetry/semantic-conventions-genai/commit/4f85037ef86e92c510d2ef881a58f1076f6fc0e4)
Open commit detailssuccess
Oct 6, 2026
[4f85037](https://github.com/open-telemetry/semantic-conventions-genai/commit/4f85037ef86e92c510d2ef881a58f1076f6fc0e4) · Oct 6, 2026
## History
[History](https://github.com/open-telemetry/semantic-conventions-genai/commits/main/docs/gen-ai/gen-ai-spans.md)
Open commit details
History
1015 lines (745 loc) · 82 KB
Code owner: @open-telemetry/semconv-genai-approvers
/
# gen-ai-spans.md
Copy path
## File metadata and controls
  * Preview
  * 

1015 lines (745 loc) · 82 KB
Code owner: @open-telemetry/semconv-genai-approvers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Semantic conventions for generative client AI spans
**Status** : [Development](https://opentelemetry.io/docs/specs/otel/document-status)
  * [Spans](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#spans)
    * [Fetch response](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#fetch-response)
    * [Execute tool span](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#execute-tool-span)
  * [Capturing instructions, inputs, and outputs](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#capturing-instructions-inputs-and-outputs)
    * [Full (buffered) content](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#full-buffered-content)
      * [Recording content on attributes](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#recording-content-on-attributes)
      * [Uploading content to external storage](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#uploading-content-to-external-storage)
    * [Streaming chunks](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#streaming-chunks)


## Spans
GenAI spans represent logical operations as observed by the caller. They SHOULD cover the duration of the operation, starting when it is initiated and ending when the response is fully received or the operation is terminated due to an error or cancellation.
If a transient issue happened and the request was retried automatically, the corresponding span SHOULD cover the duration of the logical operation with all retries.
### Inference
Inference span and metric conventions are defined in [Client Inference](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/client-inference.md).
### Embeddings
**Status:**
Describes GenAI embeddings span - a request to a Generative AI model or service that generates an embeddings based on the input.
The `gen_ai.operation.name` SHOULD be `embeddings`.
**Span name** SHOULD be `{gen_ai.operation.name} {gen_ai.request.model}`.
**Span kind** SHOULD be `CLIENT`.
**Span status** SHOULD follow the [Recording Errors](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/recording-errors.md) document.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
| `Required`  | string  | The Generative AI provider as identified by the client or server instrumentation. [2]  |  `openai`; `gcp.gen_ai`; `gcp.vertex_ai`  |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [3]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
|  `Conditionally Required` If available.  | string  | The name of the GenAI model a request is being made to. [4]  | `gpt-4`  |  
|  `Conditionally Required` If `server.address` is set.  | int  | GenAI server port. [5]  |  `80`; `8080`; `443`  |  
| [`gen_ai.embeddings.dimension.count`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Recommended`  | int  | The number of dimensions the resulting output embeddings should have.  |  `512`; `1024`  |  
| [`gen_ai.request.encoding_formats`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Recommended`  | string[]  | The encoding formats requested in an embeddings operation, if specified. [6]  |  `["base64"]`; `["float", "binary"]`  |  
| `Recommended`  | string  | The name of the model that generated the response.  | `gpt-4-0613`  |  
| [`gen_ai.usage.input_tokens`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Recommended`  | int  | The number of tokens used in the GenAI input (prompt). [7]  | `100`  |  
| `Recommended`  | string  | GenAI server address. [8]  |  `example.com`; `10.1.2.80`; `/tmp/my.sock`  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`gen_ai.provider.name` :** Semantic conventions for individual GenAI operations SHOULD clarify which kinds of providers (e.g. inference, embeddings, retrieval, memory, hosted agent providers) apply when it is not clear from context.
The attribute SHOULD be set based on the instrumentation's best knowledge and may differ from the actual upstream provider. For example, a client SDK may be configured against a proxy or hosting platform that transparently relays requests to a different provider.
The `gen_ai.provider.name` attribute acts as a discriminator that identifies the GenAI telemetry format flavor specific to that provider within GenAI semantic conventions. It SHOULD be set consistently with provider-specific attributes and signals. For example, GenAI spans, metrics, and events related to AWS Bedrock should have the `gen_ai.provider.name` set to `aws.bedrock` and include applicable `aws.bedrock.*` attributes and are not expected to include `openai.*` attributes.
**[3]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[4]`gen_ai.request.model` :** The name of the GenAI model a request is being made to. If the model is supplied by a vendor, then the value must be the exact name of the model requested. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.
**[5]`server.port` :** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.
**[6]`gen_ai.request.encoding_formats` :** In some GenAI systems the encoding formats are called embedding types. Also, some GenAI systems only accept a single format per request.
**[7]`gen_ai.usage.input_tokens` :** This value SHOULD include all types of input tokens, including cached tokens. Instrumentations SHOULD make a best effort to populate this value, using a total provided by the provider when available or, depending on the provider API, by summing different token types parsed from the provider output.
When the provider reports both billed token counts and model-consumed token counts (for example, Cohere exposes both `usage.billed_units` and `usage.tokens`), instrumentations SHOULD report the billed count so the value matches the units the customer is charged for.
Detailed usage attributes are subsets of total and aggregate counts. For example, if a request has 100 text tokens (40 cached) and 200 image tokens:
  * `gen_ai.usage.input_tokens`: 300
  * `gen_ai.usage.cache_read.input_tokens`: 40
  * `gen_ai.usage.text.input_tokens`: 100
  * `gen_ai.usage.text.cache_read.input_tokens`: 40
  * `gen_ai.usage.image.input_tokens`: 200


**[8]`server.address` :** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.
The following attributes can be important for making sampling decisions and SHOULD be provided **at span creation time** (if provided at all):


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
| `fetch_response`  | Fetch a previously generated model response by its identifier, without performing inference, such as [OpenAI Get a model response](https://platform.openai.com/docs/api-reference/responses/get) [9]  |  
| `generate_content`  | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content)  |  
| `invoke_agent`  | Invoke GenAI agent  |  
| `invoke_workflow`  | Invoke GenAI workflow  |  
| `plan`  | Agent planning or task decomposition phase  |  
| `retrieval`  | Retrieval operation such as [OpenAI Search Vector Store API](https://platform.openai.com/docs/api-reference/vector-stores/search)  |  
| `search_memory`  | Search/query memories from a memory store  |  
| `text_completion`  | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions)  |  
| `update_memory`  | Update existing memory records  |  
| `upsert_memory`  | Create or update memory records without the caller choosing which  |  
**[9]:** Instrumentations SHOULD NOT report token usage (as attributes or metrics) for this operation.
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
| `gcp.gen_ai`  | Any Google generative AI endpoint [11]  |  
| `gcp.vertex_ai`  |  [Vertex AI](https://cloud.google.com/vertex-ai) [12]  |  
| `groq`  |  
| `ibm.watsonx.ai`  |  
| `mistral_ai`  |  
| `moonshot_ai`  |  
| `openai`  |  
| `perplexity`  |  
| `x_ai`  |  
**[10]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.
**[11]:** May be used when specific backend is unknown.
**[12]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.
### Retrievals
**Status:**
Describes GenAI retrieval span - a request to a Generative AI service or framework that retrieves relevant information or context from a vector database or search system.
The `gen_ai.operation.name` SHOULD be `retrieval`.
**Span name** SHOULD be `{gen_ai.operation.name} {gen_ai.data_source.id}`. Semantic conventions for individual GenAI providers and retrievers MAY specify different span name format.
**Span kind** SHOULD be `CLIENT`.
**Span status** SHOULD follow the [Recording Errors](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/recording-errors.md) document.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [2]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
|  `Conditionally Required` When applicable.  | string  | The data source identifier. [3]  | `H7STPQYOND`  |  
|  `Conditionally Required` When applicable.  | string  | The Generative AI provider as identified by the client or server instrumentation. [4]  |  `openai`; `gcp.gen_ai`; `gcp.vertex_ai`  |  
|  `Conditionally Required` If available.  | string  | The name of the GenAI model a request is being made to. [5]  | `gpt-4`  |  
|  `Conditionally Required` If `server.address` is set.  | int  | GenAI server port. [6]  |  `80`; `8080`; `443`  |  
| `Recommended`  | int  | The maximum number of documents the retriever was asked to return for the query (also known as `k`, `limit`, or `max_num_results`).  |  
| `Recommended`  | string  | GenAI server address. [7]  |  `example.com`; `10.1.2.80`; `/tmp/my.sock`  |  
| [`gen_ai.retrieval.documents`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Opt-In`  | any  | The documents retrieved. [8]  | [ { "id": "doc_123", "score": 0.95 }, { "id": "doc_456", "score": 0.87 }, { "id": "doc_789", "score": 0.82 }]  |  
| [`gen_ai.retrieval.query.text`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Opt-In`  | string  | The query text used for retrieval. [9]  |  `What is the capital of France?`; `weather in Paris`  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[3]`gen_ai.data_source.id` :** Data sources are used by AI agents and RAG applications to store grounding data. A data source may be an external database, object store, document collection, website, or any other storage system used by the GenAI agent or application. The `gen_ai.data_source.id` SHOULD match the identifier used by the GenAI system rather than a name specific to the external storage, such as a database or object store. Semantic conventions referencing `gen_ai.data_source.id` MAY also leverage additional attributes, such as `db.*`, to further identify and describe the data source.
**[4]`gen_ai.provider.name` :** Semantic conventions for individual GenAI operations SHOULD clarify which kinds of providers (e.g. inference, embeddings, retrieval, memory, hosted agent providers) apply when it is not clear from context.
The attribute SHOULD be set based on the instrumentation's best knowledge and may differ from the actual upstream provider. For example, a client SDK may be configured against a proxy or hosting platform that transparently relays requests to a different provider.
The `gen_ai.provider.name` attribute acts as a discriminator that identifies the GenAI telemetry format flavor specific to that provider within GenAI semantic conventions. It SHOULD be set consistently with provider-specific attributes and signals. For example, GenAI spans, metrics, and events related to AWS Bedrock should have the `gen_ai.provider.name` set to `aws.bedrock` and include applicable `aws.bedrock.*` attributes and are not expected to include `openai.*` attributes.
**[5]`gen_ai.request.model` :** The name of the GenAI model a request is being made to. If the model is supplied by a vendor, then the value must be the exact name of the model requested. If the model is a fine-tuned custom model, the value should have a more specific name than the base model that's been fine-tuned.
**[6]`server.port` :** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.
**[7]`server.address` :** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.
**[8]`gen_ai.retrieval.documents` :** Each document object SHOULD contain the following properties when available: `id` (string): A unique identifier for the document, `score` (double): The relevance score of the document
Instrumentations MUST follow [JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-retrieval-documents.json).
When the attribute is recorded on events, it MUST be recorded in structured form. When recorded on spans, it MAY be recorded as a JSON string if structured format is not supported and SHOULD be recorded in structured form otherwise.
**[9]`gen_ai.retrieval.query.text` :**
Warning
This attribute may contain sensitive information.
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
| `fetch_response`  | Fetch a previously generated model response by its identifier, without performing inference, such as [OpenAI Get a model response](https://platform.openai.com/docs/api-reference/responses/get) [10]  |  
| `generate_content`  | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content)  |  
| `invoke_agent`  | Invoke GenAI agent  |  
| `invoke_workflow`  | Invoke GenAI workflow  |  
| `plan`  | Agent planning or task decomposition phase  |  
| `retrieval`  | Retrieval operation such as [OpenAI Search Vector Store API](https://platform.openai.com/docs/api-reference/vector-stores/search)  |  
| `search_memory`  | Search/query memories from a memory store  |  
| `text_completion`  | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions)  |  
| `update_memory`  | Update existing memory records  |  
| `upsert_memory`  | Create or update memory records without the caller choosing which  |  
**[10]:** Instrumentations SHOULD NOT report token usage (as attributes or metrics) for this operation.
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
| `gcp.gen_ai`  | Any Google generative AI endpoint [12]  |  
| `gcp.vertex_ai`  |  [Vertex AI](https://cloud.google.com/vertex-ai) [13]  |  
| `groq`  |  
| `ibm.watsonx.ai`  |  
| `mistral_ai`  |  
| `moonshot_ai`  |  
| `openai`  |  
| `perplexity`  |  
| `x_ai`  |  
**[11]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.
**[12]:** May be used when specific backend is unknown.
**[13]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.
### Fetch response
**Status:**
Describes a request to a Generative AI provider that fetches a previously generated model response by its identifier.
The `gen_ai.operation.name` SHOULD be `fetch_response`.
**Span name** SHOULD be `{gen_ai.operation.name}`. The response identifier is high cardinality, so it is not included in the span name. Semantic conventions for individual GenAI providers MAY specify a different span name format.
**Span kind** SHOULD be `CLIENT`.
**Span status** SHOULD follow the [Recording Errors](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/recording-errors.md) document.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
| `Required`  | string  | The Generative AI provider as identified by the client or server instrumentation. [2]  |  `openai`; `gcp.gen_ai`; `gcp.vertex_ai`  |  
| `Required`  | string  | The unique identifier for the completion. [3]  | `chatcmpl-123`  |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [4]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
| [`gen_ai.request.stream_cursor`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  |  `Conditionally Required` [5]  | string  | The cursor identifying the last streamed event already received, used to resume a streamed response from that position. [6]  |  `42`; `event-abc123`  |  
|  `Conditionally Required` If `server.address` is set.  | int  | GenAI server port. [7]  |  `80`; `8080`; `443`  |  
| [`gen_ai.response.finish_reasons`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Recommended`  | string[]  | Array of reasons the model stopped generating tokens, corresponding to each generation received. [8]  |  `["stop"]`; `["stop", "length"]`; `["stop", "length", "error"]`  |  
| `Recommended`  | string  | The name of the model that generated the response.  | `gpt-4-0613`  |  
| `Recommended`  | string  | The lifecycle status of a generated response, as reported by the provider when the response is fetched or polled. [9]  |  `completed`; `in_progress`  |  
| `Recommended`  | string  | GenAI server address. [10]  |  `example.com`; `10.1.2.80`; `/tmp/my.sock`  |  
| `Opt-In`  | any  | Messages returned by the model where each message represents a specific model response (choice, candidate). [11]  | [ { "role": "assistant", "parts": [ { "type": "text", "content": "The weather in Paris is currently rainy with a temperature of 57°F." } ] }]  |  
| [`gen_ai.system_instructions`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Opt-In`  | any  | The system message or instructions provided to the GenAI model separately from the chat history. [12]  | [ { "type": "text", "content": "You are an Agent that greet users, always use greetings tool to respond" }]; [ { "type": "text", "content": "You are a language translator." }, { "type": "text", "content": "Your mission is to translate text in English to French." }]  |  
| [`gen_ai.tool.definitions`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Opt-In`  | any  | The list of tool definitions available to the GenAI agent or model. [13]  | [ { "type": "function", "name": "get_current_weather", "description": "Get the current weather in a given location", "parameters": { "type": "object", "properties": { "location": { "type": "string", "description": "The city and state, e.g. San Francisco, CA" }, "unit": { "type": "string", "enum": [ "celsius", "fahrenheit" ] } }, "required": [ "location", "unit" ] } }]  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`gen_ai.provider.name` :** Semantic conventions for individual GenAI operations SHOULD clarify which kinds of providers (e.g. inference, embeddings, retrieval, memory, hosted agent providers) apply when it is not clear from context.
The attribute SHOULD be set based on the instrumentation's best knowledge and may differ from the actual upstream provider. For example, a client SDK may be configured against a proxy or hosting platform that transparently relays requests to a different provider.
The `gen_ai.provider.name` attribute acts as a discriminator that identifies the GenAI telemetry format flavor specific to that provider within GenAI semantic conventions. It SHOULD be set consistently with provider-specific attributes and signals. For example, GenAI spans, metrics, and events related to AWS Bedrock should have the `gen_ai.provider.name` set to `aws.bedrock` and include applicable `aws.bedrock.*` attributes and are not expected to include `openai.*` attributes.
**[3]`gen_ai.response.id` :** The identifier of the response being fetched, as used in the request and returned in the response.
**[4]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[5]`gen_ai.request.stream_cursor` :** When the fetch resumes a streamed response from a prior position.
**[6]`gen_ai.request.stream_cursor` :** The cursor identifying the last streamed event already received, used to resume a streamed fetch from that position. Maps to `starting_after` in the OpenAI Responses API and `last_event_id` in the Google GenAI Interactions API.
**[7]`server.port` :** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.
**[8]`gen_ai.response.finish_reasons` :** Conveys the outcome of the original generation as recorded on the fetched response, derived from its status: a completed generation maps to its stop reason, an incomplete one to why it was cut short (for example `length` or `content_filter`), and a failed or cancelled generation to `error`.
**[9]`gen_ai.response.status` :** The lifecycle status of the fetched response as reported by the provider, mapped onto the cross-provider enum. For a background or streamed response this conveys whether generation is still queued or in progress, or has reached a terminal state such as completed, failed, or cancelled.
**[10]`server.address` :** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.
**[11]`gen_ai.output.messages` :** Each message represents a single output choice/candidate generated by the model. Each message corresponds to exactly one generation (choice/candidate) and vice versa - one choice cannot be split across multiple messages or one message cannot contain parts from multiple choices.
Instrumentations MAY provide a way for users to filter or truncate output messages. `gen_ai.response.finish_reasons` remains aligned with the generations returned by the provider, not with a filtered or truncated `gen_ai.output.messages` value.
When an operation ends in an error, this attribute SHOULD only be set when the operation produced output, including partial streamed output. Instrumentation SHOULD NOT synthesize output content for a failed operation.
Warning
This attribute is likely to contain sensitive information including user/PII data.
See [Recording content on attributes](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#recording-content-on-attributes) section for more details.
Instrumentations MUST follow [JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-output-messages.json).
When the attribute is recorded on events, it MUST be recorded in structured form. When recorded on spans, it MAY be recorded as a JSON string if structured format is not supported and SHOULD be recorded in structured form otherwise.
**[12]`gen_ai.system_instructions` :** Only content carried on the fetched response is recorded. A fetched response contains the system instructions, output messages, and tool definitions but not the original input messages, so `gen_ai.input.messages` is not set on this span.
Instrumentations MUST follow [JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-system-instructions.json).
When the attribute is recorded on events, it MUST be recorded in structured form. When recorded on spans, it MAY be recorded as a JSON string if structured format is not supported and SHOULD be recorded in structured form otherwise.
**[13]`gen_ai.tool.definitions` :**
Warning
This attribute may contain sensitive information.
Since this attribute could be large, it's NOT RECOMMENDED to populate non-required properties by default. Instrumentations MAY provide a way to enable populating optional properties.
Instrumentations MUST follow [JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-tool-definitions.json).
When the attribute is recorded on events, it MUST be recorded in structured form. When recorded on spans, it MAY be recorded as a JSON string if structured format is not supported and SHOULD be recorded in structured form otherwise.
The following attributes can be important for making sampling decisions and SHOULD be provided **at span creation time** (if provided at all):


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
| `fetch_response`  | Fetch a previously generated model response by its identifier, without performing inference, such as [OpenAI Get a model response](https://platform.openai.com/docs/api-reference/responses/get) [14]  |  
| `generate_content`  | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content)  |  
| `invoke_agent`  | Invoke GenAI agent  |  
| `invoke_workflow`  | Invoke GenAI workflow  |  
| `plan`  | Agent planning or task decomposition phase  |  
| `retrieval`  | Retrieval operation such as [OpenAI Search Vector Store API](https://platform.openai.com/docs/api-reference/vector-stores/search)  |  
| `search_memory`  | Search/query memories from a memory store  |  
| `text_completion`  | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions)  |  
| `update_memory`  | Update existing memory records  |  
| `upsert_memory`  | Create or update memory records without the caller choosing which  |  
**[14]:** Instrumentations SHOULD NOT report token usage (as attributes or metrics) for this operation.
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
| `gcp.gen_ai`  | Any Google generative AI endpoint [16]  |  
| `gcp.vertex_ai`  |  [Vertex AI](https://cloud.google.com/vertex-ai) [17]  |  
| `groq`  |  
| `ibm.watsonx.ai`  |  
| `mistral_ai`  |  
| `moonshot_ai`  |  
| `openai`  |  
| `perplexity`  |  
| `x_ai`  |  
**[15]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.
**[16]:** May be used when specific backend is unknown.
**[17]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.
`gen_ai.response.status` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.  
| Value  | Description  | Stability  |  
| --- | --- | --- |  
| `cancelled`  | The response generation was cancelled before it completed.  |  
| `completed`  | The response finished generating successfully.  |  
| `failed`  | The response generation failed with an error.  |  
| `in_progress`  | The response is still being generated, for example a background or streamed response that has not finished.  |  
| `incomplete`  | The response stopped before generation completed, for example because a token limit or content filter was reached.  |  
| `queued`  | The response has been accepted by the provider but generation has not started yet.  |  
### Memory
**Status:**
Describes a memory operation, including memory store lifecycle operations and memory record operations.
The `gen_ai.operation.name` SHOULD be one of `create_memory_store`, `search_memory`, `create_memory`, `update_memory`, `upsert_memory`, `delete_memory`, or `delete_memory_store`.
`create_memory_store` and `delete_memory_store` cover memory store lifecycle operations. `create_memory`, `update_memory`, `upsert_memory`, `search_memory`, and `delete_memory` cover memory record operations.
Use `create_memory` when the caller requests creation of new memory records. Use `update_memory` when the caller requests modification of known existing memory records. Use `upsert_memory` when the caller invokes a public API that may create, update, or consolidate memory records without the caller choosing which.
**Span kind** SHOULD be `CLIENT` and MAY be set to `INTERNAL` on spans representing calls to memory systems running in the same process. It's RECOMMENDED to use `CLIENT` kind when the memory system being instrumented usually runs in a different process than its client or when the memory call happens over instrumented protocol such as HTTP.
For `delete_memory`, lack of `gen_ai.memory.record.id` may indicate that the operation intends to delete all memory records in the specified store.
**Span name** SHOULD be `{gen_ai.operation.name}`.
**Span status** SHOULD follow the [Recording Errors](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/recording-errors.md) document.
**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [2]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
| [`gen_ai.memory.record.id`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  |  `Conditionally Required` [3]  | string  | The unique identifier of the memory record.  | `mem_5j66UpCpwteGg4YSxUnt7lPY`  |  
|  `Conditionally Required` If applicable.  | string  | The unique identifier of the memory store. [4]  |  `ms_abc123`; `user-preferences-store`  |  
|  `Conditionally Required` [5]  | string  | The Generative AI provider as identified by the client or server instrumentation. [6]  |  `openai`; `gcp.gen_ai`; `gcp.vertex_ai`  |  
|  `Conditionally Required` If `server.address` is set.  | int  | GenAI server port. [7]  |  `80`; `8080`; `443`  |  
| [`gen_ai.memory.record.count`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  |  `Recommended` [8]  | int  | The number of memory records relevant to the operation. [9]  |  
| `Recommended`  | string  | GenAI server address. [10]  |  `example.com`; `10.1.2.80`; `/tmp/my.sock`  |  
| [`gen_ai.memory.query.text`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Opt-In`  | string  | The search query used to retrieve memories. [11]  |  `user dietary preferences`; `past flight bookings`  |  
| `Opt-In`  | any  | The memory records stored or retrieved in a memory operation. [12]  | [ { "content": "User prefers dark mode", "id": "mem_123", "score": 0.95 }, { "content": { "preference": "vegetarian meals", "confidence": 0.9 }, "metadata": { "source": "profile" } }]  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[3]`gen_ai.memory.record.id` :** When the operation applies to a specific memory record.
**[4]`gen_ai.memory.store.id` :** Semantic conventions for individual components SHOULD document what `gen_ai.memory.store.id` maps to within the implementation.
**[5]`gen_ai.provider.name` :** If the operation is handled by a named external GenAI provider or service.
**[6]`gen_ai.provider.name` :** Semantic conventions for individual GenAI operations SHOULD clarify which kinds of providers (e.g. inference, embeddings, retrieval, memory, hosted agent providers) apply when it is not clear from context.
The attribute SHOULD be set based on the instrumentation's best knowledge and may differ from the actual upstream provider. For example, a client SDK may be configured against a proxy or hosting platform that transparently relays requests to a different provider.
The `gen_ai.provider.name` attribute acts as a discriminator that identifies the GenAI telemetry format flavor specific to that provider within GenAI semantic conventions. It SHOULD be set consistently with provider-specific attributes and signals. For example, GenAI spans, metrics, and events related to AWS Bedrock should have the `gen_ai.provider.name` set to `aws.bedrock` and include applicable `aws.bedrock.*` attributes and are not expected to include `openai.*` attributes.
**[7]`server.port` :** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.
**[8]`gen_ai.memory.record.count` :** If the operation involves memory records and the count is available.
**[9]`gen_ai.memory.record.count` :** For `search_memory` operations, this is the number of memory records returned by the operation. For `create_memory` operations, this is the number of memory records the operation attempted to create. For `update_memory` operations, this is the number of memory records the operation attempted to modify. For `upsert_memory` operations, this is the number of memory records the operation attempted to create or update. For `delete_memory` operations, this is the number of memory records the operation attempted to delete.
**[10]`server.address` :** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.
**[11]`gen_ai.memory.query.text` :** Instrumentations SHOULD NOT capture this attribute by default. Capture SHOULD be gated by an explicit user opt-in, for example `OTEL_INSTRUMENTATION_GENAI_CAPTURE_MESSAGE_CONTENT`.
Warning
This attribute may contain sensitive information.
**[12]`gen_ai.memory.records` :** Instrumentations SHOULD NOT capture this attribute by default. Capture SHOULD be gated by an explicit user opt-in, for example `OTEL_INSTRUMENTATION_GENAI_CAPTURE_MESSAGE_CONTENT`.
Warning
This attribute may contain sensitive information including user/PII data.
Instrumentations MUST follow [JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-memory-records.json).
When the attribute is recorded on events, it MUST be recorded in structured form. When recorded on spans, it MAY be recorded as a JSON string if structured format is not supported and SHOULD be recorded in structured form otherwise.
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
| `fetch_response`  | Fetch a previously generated model response by its identifier, without performing inference, such as [OpenAI Get a model response](https://platform.openai.com/docs/api-reference/responses/get) [13]  |  
| `generate_content`  | Multimodal content generation operation such as [Gemini Generate Content](https://ai.google.dev/api/generate-content)  |  
| `invoke_agent`  | Invoke GenAI agent  |  
| `invoke_workflow`  | Invoke GenAI workflow  |  
| `plan`  | Agent planning or task decomposition phase  |  
| `retrieval`  | Retrieval operation such as [OpenAI Search Vector Store API](https://platform.openai.com/docs/api-reference/vector-stores/search)  |  
| `search_memory`  | Search/query memories from a memory store  |  
| `text_completion`  | Text completions operation such as [OpenAI Completions API (Legacy)](https://platform.openai.com/docs/api-reference/completions)  |  
| `update_memory`  | Update existing memory records  |  
| `upsert_memory`  | Create or update memory records without the caller choosing which  |  
**[13]:** Instrumentations SHOULD NOT report token usage (as attributes or metrics) for this operation.
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
| `gcp.gen_ai`  | Any Google generative AI endpoint [15]  |  
| `gcp.vertex_ai`  |  [Vertex AI](https://cloud.google.com/vertex-ai) [16]  |  
| `groq`  |  
| `ibm.watsonx.ai`  |  
| `mistral_ai`  |  
| `moonshot_ai`  |  
| `openai`  |  
| `perplexity`  |  
| `x_ai`  |  
**[14]:** Used when accessing the 'generativelanguage.googleapis.com' endpoint. Also known as the AI Studio API.
**[15]:** May be used when specific backend is unknown.
**[16]:** Used when accessing the 'aiplatform.googleapis.com' endpoint.
### Execute tool span
**Status:**
Describes tool execution span.
`gen_ai.operation.name` SHOULD be `execute_tool`.
GenAI instrumentations that can instrument tool execution calls SHOULD do so, unless another instrumentation can reliably cover all supported tool types. MCP tool executions may also be traced by the [corresponding MCP instrumentation](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/mcp.md#client).
Tools are often executed directly by application code. Application developers are encouraged to follow this semantic convention for tools invoked by their own code and to manually instrument any tool calls that automatic instrumentations do not cover.
Some tools are specialized, such as [Agent Skills](https://agentskills.io) that agentic frameworks expose as tools. Instrumentations SHOULD distinguish generic tools from specialized ones using framework-specific tool names or other heuristics and record the applicable [refinement](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-agent-spans.md#execute-tool-span). They SHOULD NOT record two different spans for one call.
**Span name** SHOULD be `execute_tool {gen_ai.tool.name}`.
**Span kind** SHOULD be `INTERNAL`.
**Span status** SHOULD follow the [Recording Errors](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/recording-errors.md) document.
**Entity Associations** The span SHOULD be associated with one of the following entities:


**Requirement level:** [Recommended](https://github.com/open-telemetry/semantic-conventions/blob/v1.44.0/docs/general/signal-requirement-level.md).
**Attributes:**  
| Key  | Stability  | Value Type  | Description  | Example Values  |  
| --- | --- | --- | --- | --- |  
| `Required`  | string  | The name of the operation being performed. [1]  |  `chat`; `generate_content`; `text_completion`  |  
| `Required`  | string  | Name of the tool utilized by the agent.  | `Flights`  |  
|  `Conditionally Required` If the operation ended in an error.  | string  | Describes a class of error the operation ended with. [2]  |  `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500`  |  
|  `Conditionally Required` When applicable.  | string  | The human-readable name of the agent executing the tool.  |  `Math Tutor`; `Fiction Writer`  |  
|  `Conditionally Required` If available.  | string  | The unique identifier for a conversation (session, thread), used to store and correlate messages within this conversation. [3]  | `conv_5j66UpCpwteGg4YSxUnt7lPY`  |  
|  `Recommended` If available.  | string  | The tool call identifier.  | `call_mszuSIzqtI65i1wAUOE8w5H4`  |  
| [`gen_ai.tool.description`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  |  `Recommended` If available.  | string  | The tool description. [4]  | `Multiply two numbers`  |  
|  `Recommended` If available.  | string  | Type of the tool utilized by the agent [5]  |  `function`; `extension`; `datastore`  |  
| [`gen_ai.tool.call.arguments`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Opt-In`  | any  | Parameters passed to the tool call. [6]  | { "location": "San Francisco?", "date": "2025-10-01"}  |  
| [`gen_ai.tool.call.result`](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/registry/attributes/gen-ai.md)  | `Opt-In`  | any  | The result returned by the tool call (if any and if execution was successful). [7]  | { "temperature_range": { "high": 75, "low": 60 }, "conditions": "sunny"}  |  
**[1]`gen_ai.operation.name` :** If one of the predefined values applies, but specific system uses a different name it's RECOMMENDED to document it in the semantic conventions for specific GenAI system and use system-specific name in the instrumentation. If a different name is not documented, instrumentation libraries SHOULD use applicable predefined value.
**[2]`error.type` :** The `error.type` SHOULD match the error code returned by the Generative AI provider or the client library, the canonical name of exception that occurred, or another low-cardinality error identifier. Instrumentations SHOULD document the list of errors they report.
**[3]`gen_ai.conversation.id` :** Instrumentations SHOULD populate conversation id when they have an identifier for the conversation readily available for a given operation, for example:
  * when the client framework being instrumented manages conversation history (see [LlamaIndex chat store](https://docs.llamaindex.ai/en/stable/module_guides/storing/chat_stores/), [LangChain `session_id`](https://reference.langchain.com/python/langchain-core/runnables/history/RunnableWithMessageHistory), and [Google ADK sessions](https://adk.dev/sessions/session))
  * when instrumenting GenAI client libraries that maintain a conversation on the backend (see [AWS Bedrock agent sessions](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-session-state.html), [OpenAI Assistant threads](https://platform.openai.com/docs/api-reference/threads))


When no identifier for the conversation is available, instrumentations SHOULD NOT populate conversation id. For example, a new UUID, a trace identifier, or a hash of request content SHOULD NOT be used as a fallback value.
Application developers that manage conversation history MAY add conversation id to GenAI and other spans or logs using custom span or log record processors or hooks provided by instrumentation libraries.
**[4]`gen_ai.tool.description` :**
Warning
This attribute may contain sensitive information.
**[5]`gen_ai.tool.type` :** Extension: A tool executed on the agent-side to directly call external APIs, bridging the gap between the agent and real-world systems. Agent-side operations involve actions that are performed by the agent on the server or within the agent's controlled environment. Function: A tool executed on the client-side, where the agent generates parameters for a predefined function, and the client executes the logic. Client-side operations are actions taken on the user's end or within the client application. Datastore: A tool used by the agent to access and query structured or unstructured external data for retrieval-augmented tasks or knowledge updates.
**[6]`gen_ai.tool.call.arguments` :**
Warning
This attribute may contain sensitive information.
It's expected to be an object - in case a serialized string is available to the instrumentation, the instrumentation SHOULD do the best effort to deserialize it to an object.
Instrumentations MUST follow [JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-tool-call-arguments.json).
When the attribute is recorded on events, it MUST be recorded in structured form. When recorded on spans, it MAY be recorded as a JSON string if structured format is not supported and SHOULD be recorded in structured form otherwise.
**[7]`gen_ai.tool.call.result` :**
Warning
This attribute may contain sensitive information.
It's expected to be an object - in case a serialized string is available to the instrumentation, the instrumentation SHOULD do the best effort to deserialize it to an object.
Instrumentations MUST follow [JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-tool-call-result.json).
When the attribute is recorded on events, it MUST be recorded in structured form. When recorded on spans, it MAY be recorded as a JSON string if structured format is not supported and SHOULD be recorded in structured form otherwise.
The following attributes can be important for making sampling decisions and SHOULD be provided **at span creation time** (if provided at all):


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
## Capturing instructions, inputs, and outputs
### Full (buffered) content
Model instructions, user messages, and model outputs are considered sensitive and are often large in size.
Recording large or sensitive content in telemetry may be problematic due to high storage costs, regulatory requirements, or the need to enforce different access models for operational and user data.
OpenTelemetry instrumentations SHOULD NOT capture them by default, but SHOULD provide an option for users to opt in.
Application developers should choose an appropriate usage pattern based on application needs and maturity:
  1. [Default] Don't record instructions, inputs, or outputs.
  2. Record instructions, inputs, and outputs on the GenAI spans using corresponding attributes (`gen_ai.system_instructions`, `gen_ai.input.messages`, `gen_ai.output.messages`).
This approach is best suited for situations where telemetry volume is manageable and either privacy regulations do not apply or the telemetry storage complies with them, for example, in pre-production environments.
See [Recording content on attributes](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#recording-content-on-attributes) section for more details.
  3. Store content externally and record references on the spans.
This pattern is recommended in production environments where telemetry volume is a concern or sensitive data needs to be handled securely. Using external storage enables separate access controls.
See [Uploading content to external storage](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md#uploading-content-to-external-storage) section for more details.


#### Recording content on attributes
The content captured in `gen_ai.system_instructions`, `gen_ai.input.messages`, and `gen_ai.output.messages` attributes is likely to be large.
It may contain media, and even in the text form, it may be larger than observability backend limits for telemetry envelopes or attribute values.
The inputs and outputs attributes follow common structure formally defined in [inputs JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-input-messages.json) and [outputs JSON schema](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-output-messages.json). See also their representation in [Python code](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/non-normative/models.py).
Note
Recording structured attributes is supported on events (or logs) and may not yet be supported on spans. See [OTEP: Extending attributes to support complex values](https://github.com/open-telemetry/opentelemetry-specification/blob/main/oteps/4485-extending-attributes-to-support-complex-values.md) for the details.
If structured attributes are not yet supported on spans in a given language, the corresponding attribute value SHOULD be serialized to JSON string on spans and recorded in its structured form on events.
Instrumentation MAY provide a configuration option allowing to truncate properties such as individual message contents, preserving JSON structure.
#### Uploading content to external storage
Instrumentations MAY support user-defined in-process hooks to handle content upload.
The hook SHOULD operate independently of the opt-in flags that control capturing of `gen_ai.system_instructions`, `gen_ai.input.messages`, and `gen_ai.output.messages`.
If such a hook is supported and configured, instrumentations SHOULD invoke it regardless of the span sampling decision with:
  * the instructions, [inputs](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-input-messages.json), and [outputs](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model/gen-ai/gen-ai-output-messages.json) object using formats defined in this convention and before they are serialized to JSON string;
  * the span instance


The hook SHOULD receive the same instructions, inputs, and outputs that the instrumentation would record in these attributes.
The hook implementation SHOULD be able to enrich and modify provided span, instructions, and message objects.
If instrumentation is configured to also record `gen_ai.system_instructions`, `gen_ai.input.messages`, and `gen_ai.output.messages` attributes, it SHOULD do it after calling the hook and SHOULD record values that were potentially modified within the hook implementation.
The hook API SHOULD be generic. The application or distro is responsible for the hook implementation including
  * the uploading process either in synchronous or asynchronous way,
  * recording references to the uploaded content on the span,
  * handling content in a different way.


Application or OpenTelemetry distributions MAY also implement content uploading in the telemetry processing pipeline (in-process or via a collector), based on the `gen_ai.system_instructions`, `gen_ai.input.messages`, and `gen_ai.output.messages` attributes. Given the potential data volume, it is RECOMMENDED to tune batching and export settings accordingly in the OpenTelemetry SDK pipeline.
TODO: document a common approach to record references to externally stored content.
Check out [LLM call examples](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/non-normative/examples-llm-calls.md).
### Streaming chunks
TODO
You can’t perform that action at this time. 
