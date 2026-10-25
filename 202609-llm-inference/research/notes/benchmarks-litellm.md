---
title: Benchmarks | liteLLM
id: benchmarks-litellm
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- performance
- benchmarks
created: '2026-10-09T23:00:32.419523Z'
updated: '2026-10-10T14:52:17.153494Z'
source: https://docs.litellm.ai/docs/benchmarks
source_domain: docs.litellm.ai
fetched_at: '2026-10-09T23:00:32.396370Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'LiteLLM''s own published performance-test results page -- the primary source
  behind the README''s ''8ms P95 latency at 1k RPS'' headline claim, and directly
  responsive to the task''s ask for stated performance limits of the proxy under load.
  Critical nuance the README strips out: the 8ms P95 gateway-overhead figure comes
  from the ''Short-prompt chat'' test running FOUR gateway instances (1,170 RPS, 8ms
  p95 overhead, 150ms p95 request latency); the SAME test with only TWO instances
  produces materially worse results -- 1,035.7 RPS, 29ms p95 gateway overhead, and
  630ms p95 total request latency -- showing per-instance throughput does not scale
  linearly and horizontal replication is required to hit the advertised headline number,
  not single-process performance. A separate high-throughput profile (3,000 RPS, 13.5B
  tokens/minute, p95 latency 54ms) required 33 gateway pods (4 workers/4 vCPU/16GiB
  each) behind an AWS ALB, with virtual-key budgets, token counting, and spend tracking
  all enabled, Postgres+Redis backing the proxy, and response caching disabled --
  i.e. the full multi-tenant accounting machinery evaluated in this batch was active
  during the throughput test, not stripped out for the benchmark. All tests use in-process
  mock models or a mock OpenAI/realtime endpoint, so these are gateway-overhead-only
  numbers, excluding real upstream LLM latency. A realtime-API (WebSocket) test reached
  1,207 RPS with 67ms p95 end-to-end latency on 4 instances. A logging-callback overhead
  test found GCS bucket logging and LangSmith logging added negligible overhead versus
  the base proxy (~1,133-1,137 RPS, ~132-140ms median latency across all three configs).'
---

*Suggested by [[github-berriailitellm-the-fastest-litest-ai-gateway-rust-core-with-python-sdk-ca]] — source of the 8ms P95 at 1k RPS performance claim cited in the README*

[Skip to main content](https://docs.litellm.ai/docs/benchmarks#__docusaurus_skipToContent_fallback)
[**Decisions API is here.** Call Jev, Clef or any decision model through one /v1/decisions endpoint. →](https://docs.litellm.ai/docs/decisions)
On this page
Copy page
Performance test results for LiteLLM Gateway.
## High-throughput performance test results[​](https://docs.litellm.ai/docs/benchmarks#high-throughput-profile-3000-rps-with-50k-to-100k-token-prompts "Direct link to High-throughput performance test results")
**13.5 billion tokens per minute (TPM).**  
| Metric  | Result  |  
| --- | --- |  
| Requests per second  | 3,000  |  
| Client HTTP 200 rate  | 100.00%  |  
| Gateway request latency, p50  | 30.581 ms  |  
| Gateway request latency, p95  | 54.029 ms  |  
| Gateway request latency, p99  | 91.645 ms  |  
| Client time to first token, p50  | 31.667 ms  |  
### Methodology[​](https://docs.litellm.ai/docs/benchmarks#methodology "Direct link to Methodology")  
| Setting  | Configuration  |  
| --- | --- |  
| Deployment  | 33 gateway pods; 4 workers, 4 vCPUs, and 16 GiB memory requested per pod  |  
| Traffic  | Distributed Locust with 30 load-generator workers; 3,000 users at one request per second each  |  
| Requests  |  `/v1/chat/completions`; equal shares of 50K, 75K, and 100K-token prompts; 50% streaming; `max_tokens: 16`  |  
| Model and network  | In-process mock model; public AWS Application Load Balancer; 60-second client timeout  |  
| Configuration  |  [High-throughput Helm values](https://docs.litellm.ai/docs/proxy/high_throughput); PostgreSQL and Redis; virtual-key budgets, token counting, spend tracking, and metrics; response caching disabled  |  
| Duration  | 24 minutes 22 seconds  |  
| Measurement  | Prometheus: throughput and gateway latency. Locust: client success and time to first token, including upload and load-balancer time  |  
## Realtime API performance test results[​](https://docs.litellm.ai/docs/benchmarks#realtime-api-benchmarks "Direct link to Realtime API performance test results")
**1,207 requests per second.**  
| Metric  | Result  |  
| --- | --- |  
| End-to-end latency, p50  | 59 ms  |  
| End-to-end latency, p95  | 67 ms  |  
| End-to-end latency, p99  | 99 ms  |  
| Average end-to-end latency  | 63 ms  |  
### Methodology[​](https://docs.litellm.ai/docs/benchmarks#methodology-1 "Direct link to Methodology")  
| Setting  | Configuration  |  
| --- | --- |  
| Deployment  | 4 gateway instances; 4 workers, 4 vCPUs, and 8 GB RAM per instance  |  
| Traffic  | Locust; 1,000 users with a 0.5 to 1 second pause between requests  |  
| Requests  |  `/v1/realtime` over persistent WebSockets; text requests for a two-sentence story with a random ID  |  
| Model and network  | Mock realtime endpoint  |  
| Configuration  | PostgreSQL; Redis disabled  |  
| Duration  | Not recorded  |  
| Measurement  |  [Locust script](https://gist.github.com/AlexsanderHamir/73b83ada21d9b84d4fe09665cf1745f5): requests per second and latency through `response.done`, including connection setup when needed  |  
## Short-prompt chat performance test results[​](https://docs.litellm.ai/docs/benchmarks#short-prompt-chat-performance-test-results "Direct link to Short-prompt chat performance test results")
**1,170 requests per second with 8 ms p95 gateway overhead across four instances.**  
| Metric  | 2 instances  | 4 instances  |  
| --- | --- | --- |  
| Requests per second  | 1,035.7  | 1,170  |  
| Request latency, p50  | 200 ms  | 100 ms  |  
| Request latency, p95  | 630 ms  | 150 ms  |  
| Request latency, p99  | 1,200 ms  | 240 ms  |  
| Average request latency  | 262.46 ms  | 111.73 ms  |  
| Gateway overhead, p50  | 12 ms  | 2 ms  |  
| Gateway overhead, p95  | 29 ms  | 8 ms  |  
| Gateway overhead, p99  | 43 ms  | 13 ms  |  
| Average gateway overhead  | 14.74 ms  | 3.32 ms  |  
### Methodology[​](https://docs.litellm.ai/docs/benchmarks#methodology-2 "Direct link to Methodology")  
| Setting  | Configuration  |  
| --- | --- |  
| Deployment  | 2 or 4 instances; 4 vCPUs and 8 GB RAM per instance. The 4-instance test uses 4 workers per instance; the 2-instance worker count is not recorded  |  
| Traffic  | Locust; 1,000 users with a 0.5 to 1 second pause between requests  |  
| Requests  |  `/chat/completions`; the [4-instance script](https://gist.github.com/AlexsanderHamir/42c33d7a4dc7a57f56a78b560dee3a42) sends text with a random ID repeated 150 times. The 2-instance payload is not recorded  |  
| Model and network  | Mock OpenAI endpoint  |  
| Configuration  | PostgreSQL; Redis disabled  |  
| Duration  | Not recorded  |  
| Measurement  | Locust: request throughput and full request latency. Gateway overhead: `x-litellm-overhead-duration-ms` response header  |  
## Logging callback performance test results[​](https://docs.litellm.ai/docs/benchmarks#logging-callbacks "Direct link to Logging callback performance test results")  
| Configuration  | Requests per second  | Median request latency  |  
| --- | --- | --- |  
| Base proxy  | 1,133.2  | 140 ms  |  
| GCS bucket logging  | 1,137.3  | 138 ms  |  
| LangSmith logging  | 1,135  | 132 ms  |  
### Methodology[​](https://docs.litellm.ai/docs/benchmarks#methodology-3 "Direct link to Methodology")  
| Setting  | Configuration  |  
| --- | --- |  
| Deployment  | Not recorded  |  
| Traffic  | Not recorded  |  
| Requests  | Not recorded  |  
| Model and network  | Not recorded  |  
| Configuration  | Base proxy compared with [GCS bucket logging](https://docs.litellm.ai/docs/observability/gcs_bucket_integration) and [LangSmith logging](https://docs.litellm.ai/docs/observability/langsmith_integration)  |  
| Duration  | Not recorded  |  
| Measurement  | Requests per second and median request latency  |  
  * [High-throughput performance test results](https://docs.litellm.ai/docs/benchmarks#high-throughput-profile-3000-rps-with-50k-to-100k-token-prompts)
  * [Realtime API performance test results](https://docs.litellm.ai/docs/benchmarks#realtime-api-benchmarks)
  * [Short-prompt chat performance test results](https://docs.litellm.ai/docs/benchmarks#short-prompt-chat-performance-test-results)
  * [Logging callback performance test results](https://docs.litellm.ai/docs/benchmarks#logging-callbacks)


LiteLLM Enterprise
SSO/SAML, audit logs, spend tracking, multi-team management, and guardrails, built for production.
[Learn more →](https://docs.litellm.ai/docs/enterprise)
