---
title: 'Self-Hosted AI on Kubernetes: Production vLLM'
id: self-hosted-ai-on-kubernetes-production-vllm
tags:
- onprem-llm-inference-stacks-859099
- reference-architecture
- vendor-blog
created: '2026-10-23T17:24:28.103685Z'
updated: '2026-10-23T17:25:56.745689Z'
source: https://stribog.com/blog/self-hosted-llm-kubernetes-vllm-production-deployment
source_domain: stribog.com
fetched_at: '2026-10-23T17:24:28.080753Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'RELIABILITY: vendor blog (Stribog). Production vLLM on Kubernetes walkthrough
  referenced by its HIPAA article: GPU scheduling, gateways. Prescriptive, not deployment
  evidence.'
---

*Suggested by [[hipaa-compliant-llm-on-prem-phi-inference-without-a-baa]] — stack walkthrough*

[Skip to content](https://stribog.com/blog/self-hosted-llm-kubernetes-vllm-production-deployment#main-content)
Sovereign AI
# Self-Hosted AI on Kubernetes: Production vLLM
How to run self-hosted LLM inference on Kubernetes with vLLM at production grade: GPU sizing, cost crossover analysis, audit logging for EU AI Act compliance.
Stribog·2 Jun 2026·20 min read·Updated 23 Aug 2026
There are two dominant stories about self-hosted large language models. The first comes from the homelab crowd: "It is easy. Install Ollama, pull a model, done." The second comes from hyperscaler marketing: "You need a $2M GPU cluster and a team of ML engineers before you can think about running your own inference." Both are wrong in the same direction. They describe the tails of the distribution and ignore the middle, which is where the actual decision lives for most engineering organizations.
The real question is narrower: what does a **production-grade** self-hosted LLM deployment look like on Kubernetes — request routing, autoscaling, observability, model versioning, and audit logging done correctly, not a board demo? The answer, in 2026, is that vLLM with standard Kubernetes primitives and a thin inference gateway gets you there at a cost point that makes cloud API pricing irrational beyond moderate volume.
This article walks through the architecture in enough detail that you can validate the decisions independently. There is math. There are manifests. There are honest trade-offs. If you are at a regulated company that has been told to "get AI" but cannot send patient records or transaction data to OpenAI, this is the engineering path.
## The Two Lies About Self-Hosted AI: "It's Easy" and "It's Impossible"
The homelab narrative is seductive because Ollama genuinely is easy: pull a four-bit 7B model, run it on a consumer GPU in minutes, and simple tasks look impressive. What it does not give you: batched throughput, GPU memory management under concurrent load, model versioning with rollback, structured audit logging, authentication, rate limiting, or production operational guarantees. Ollama is a personal tool — a scope statement, not a criticism. Where exactly that scope ends is a question with a documented answer: see [vLLM vs Ollama and the concurrency line](https://stribog.com/blog/vllm-vs-ollama-self-hosted-llm-serving-production-choice).
The enterprise narrative fails in the opposite direction. Hyperscaler sales material routinely implies that self-hosted AI requires H100 clusters and dedicated ML Ops teams. This is true for frontier model training and cutting-edge research. It is not true for inference workloads running open-weight models at task-specific scale. Two A100 80GB cards in tensor-parallel configuration handle Llama-3.1-70B in eight-bit quantization with enough throughput for most enterprise internal tooling; a single A100 80GB suffices at four-bit quantization. The hardware exists, the software stack is mature, and [financial-services data residency requirements and the compliance pressures](https://stribog.com/blog/nis2-dora-eu-ai-act-self-hosted-kubernetes-compliance) described below are driving a measurable shift toward on-premises inference infrastructure.
## Why the EU AI Act Makes Cloud LLM APIs a Compliance Liability for High-Risk Workloads
Following the Digital Omnibus on AI (Parliament 16 June 2026; Council 29 June 2026), high-risk obligations — including Article 12 record-keeping and Article 19 log retention for Annex III systems — apply from 2 December 2027 (Annex I product-embedded systems from 2 August 2028). Article 50 transparency duties still apply from 2 August 2026. Article 12 requires automatic event logging for [high-risk AI systems](https://stribog.com/blog/eu-ai-act-high-risk-ai-systems-onprem-inference-compliance); the AI Act itself sets a six-month minimum (Article 19(1)), and sectoral law can extend it. Sector-specific record-keeping law can push retention well past that floor — check the instrument that applies to you (DORA requires ICT-incident recording and process controls, but does not itself set a universal ten-year AI-log retention period). For medical devices, MDR Article 10(8) requires technical documentation retention of at least 10 years after the last device was placed on the market, and 15 years for implantables. (Last reviewed: 2026-07-29.)
The structural problem with cloud LLM APIs is not that vendors refuse to log. Several offer logging options. The problem is **where the logs live**. When you send a prompt to a cloud API, the input transits and is processed on infrastructure owned by a US-parent company subject to CLOUD Act jurisdiction. The completion and associated metadata are generated on that same infrastructure. Even if you receive a copy of the log, the source-of-truth record sits outside your legal perimeter. An EU supervisory authority conducting an Article 12 audit expects to verify the completeness and integrity of the log chain from inside your organization's control. A pointer to an API provider's log export is not that.
This is not a theoretical concern. Article 12's record-keeping and Article 19's retention obligations point toward a specific architecture: an in-cluster audit store with append-only semantics, cryptographic log integrity verification, and an export path available only to authorized auditors. None of that is achievable when the inference endpoint is a third-party API. Self-hosted inference is not just a cost optimization at this point. For certain workload categories in regulated industries, it is the only architecture that satisfies the legal requirement.
## Hardware Sizing Reality: VRAM, Quantization, and What You Actually Need
VRAM is the binding constraint in LLM inference, and the math is straightforward once you strip away the marketing. A model's minimum VRAM footprint is approximately `(parameters * bytes_per_parameter) * 1.2`, where the 1.2 factor covers framework and activation overhead on the weight footprint (KV cache is sized separately below). At full precision (float16), a 7B model needs roughly 17GB including the 1.2 overhead factor (14GB raw weights). At four-bit quantization (GGUF Q4_K_M or AWQ), raw weights are about 3.5GB (~4GB with the 1.2 factor). At eight-bit, raw weights are about 7GB (~8GB with overhead). For the full hardware buy-decision beneath the serving software — VRAM math per model size and quantization, consumer-versus-datacenter economics, and throughput-versus-latency capacity planning — see the [dedicated guide to sizing the iron](https://stribog.com/blog/self-hosted-llm-hardware-gpu-vram-sizing-sovereign-inference-capacity).
For a 70B model — which covers the quality range needed for most enterprise code assistance, document analysis, and reasoning tasks — the numbers are: float16 requires ~140GB VRAM, which means two A100 80GBs in tensor-parallel configuration, or an H100 NVL pair bridged over NVLink (2 × 94 GB) with headroom. Eight-bit quantization brings this to ~75GB of weight storage alone, leaving limited KV cache headroom on a single A100 80GB — two A100 80GBs in tensor-parallel is the comfortable configuration at this precision. Four-bit quantization reaches ~38GB for the weights, which fits on a single A6000 48GB or two consumer-grade GPUs with 24GB each, with meaningful KV cache budget remaining. The quality trade-off at four-bit on a well-quantized open-weight model is measurable but typically acceptable for enterprise task-specific workloads.
NVIDIA's RTX Spark, announced at Computex 2026, represents a significant inflection for the on-premises case: a Blackwell GPU with 128GB unified memory in a single device, enabling 120B+ parameter models locally without tensor parallelism. At the time of writing, this sits in the prosumer segment at a cost basis that makes single-device 100B+ inference viable for teams that previously assumed they needed a multi-GPU server. For Kubernetes deployment, it slots into the standard GPU device plugin model as a single allocatable unit.
bashCopy

```
#!/usr/bin/env bash
# Usage: ./vram-budget.sh <param_billions> <quant_bits> <max_concurrent> <ctx_len>
PARAMS=${1}   # e.g. 70
BITS=${2}     # e.g. 8  (weight quantization: 16=fp16, 8=int8, 4=int4/AWQ)
CONC=${3}     # e.g. 64
CTX=${4}      # e.g. 2048

# Weight memory: parameters * bytes-per-weight * 1.2 overhead factor
BYTES_PER_PARAM=$(echo "scale=4; $BITS / 8" | bc)
MODEL_GB=$(echo "scale=1; $PARAMS * $BYTES_PER_PARAM * 1.2" | bc)

# KV cache: 2 * num_kv_heads * head_dim * ctx_len * bytes_per_kv_elem * num_layers
# NOTE: KV cache elements are stored in BF16 (2 bytes) by default in vLLM
# regardless of weight quantization — do NOT use BYTES_PER_PARAM here.
# Approximate for a 70B Llama-3.1 class model (80 layers, 8 KV heads GQA, head_dim 128)
KV_BYTES_PER_ELEM=2  # BF16 always
KV_PER_TOKEN_GB=$(echo "scale=8; 2 * 8 * 128 * 80 * $KV_BYTES_PER_ELEM / 1073741824" | bc)
KV_TOTAL_GB=$(echo "scale=1; $KV_PER_TOKEN_GB * $CTX * $CONC" | bc)

TOTAL_GB=$(echo "scale=1; $MODEL_GB + $KV_TOTAL_GB" | bc)

echo "Model weights (${BITS}-bit):  ${MODEL_GB} GB"
echo "KV cache (BF16, ${CONC} concurrent x ${CTX} ctx):  ${KV_TOTAL_GB} GB"
echo "Total required:  ${TOTAL_GB} GB VRAM"
```

Quick VRAM budget estimate for a target model and concurrency
## vLLM on Kubernetes: Architecture, GPU Resource Scheduling, and the Production Deployment Model
vLLM is the correct inference engine choice for production Kubernetes deployments as of 2026. Its PagedAttention algorithm manages GPU memory with the same logic a virtual memory system uses for RAM: it allocates KV cache in fixed-size pages, reclaims unused pages, and allows multiple requests to share cache pages when their prefixes match. The practical effect is 2-4x higher throughput versus a naive implementation at the same VRAM budget, with a preemption model that degrades gracefully under overload rather than OOM-killing.
The Kubernetes integration relies on the NVIDIA device plugin, which exposes GPUs as `nvidia.com/gpu` extended resources. Each vLLM pod requests whole GPU units; fractional GPU allocation is technically possible with [MIG, time-slicing, or DRA](https://stribog.com/blog/nvidia-gpu-operator-mig-time-slicing-kubernetes-sharing) on A100/H100 hardware and becomes worth the operational complexity precisely when a node runs mixed inference and embedding workloads rather than one dedicated model. The standard pattern is: one vLLM pod per GPU (or per GPU pair for tensor parallelism), scheduled via node selectors and tolerations onto a dedicated GPU node pool. If single-vendor GPU supply is itself a risk you carry, vLLM also ships a ROCm build — [AMD ROCm as a second source](https://stribog.com/blog/amd-rocm-gpu-alternative-nvidia-dependency-sovereign-ai-compute) works through what that changes about the resource name, the container device surface, and which vLLM features are documented as unsupported on AMD hardware.
yamlCopy

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: vllm-llama3-70b
  namespace: ai-inference
  labels:
    app: vllm
    model: llama3-70b-instruct
    model-version: "3.1.0"
spec:
  replicas: 2
  selector:
    matchLabels:
      app: vllm
      model: llama3-70b-instruct
  template:
    metadata:
      labels:
        app: vllm
        model: llama3-70b-instruct
        model-version: "3.1.0"
      annotations:
        # Scraped by Prometheus via port 8000 /metrics
        prometheus.io/scrape: "true"
        prometheus.io/port: "8000"
        prometheus.io/path: "/metrics"
    spec:
      # Schedule only onto GPU-capable nodes in the inference pool
      nodeSelector:
        node-role: gpu-inference
        nvidia.com/gpu.product: "A100-SXM4-80GB"
      tolerations:
        - key: "gpu-inference"
          operator: "Exists"
          effect: "NoSchedule"
      # Pull model weights from shared PVC; never bundle in the image
      volumes:
        - name: model-weights
          persistentVolumeClaim:
            claimName: llm-model-store
            readOnly: true
        - name: hf-cache
          emptyDir: {}
      initContainers:
        # Verify weight-shard digests before each start (SHA256SUMS produced out-of-band)
        - name: verify-model-digest
          image: alpine:3.21
          command:
            - sh
            - -c
            - |
              cd /models/llama3-70b
              sha256sum -c /models/llama3-70b/SHA256SUMS || {
                echo "Model digest mismatch — aborting" >&2
                exit 1
              }
          volumeMounts:
            - name: model-weights
              mountPath: /models
              readOnly: true
      containers:
        - name: vllm
          image: vllm/vllm-openai:v0.6.4
          args:
            - "--model=/models/llama3-70b"
            - "--served-model-name=llama3-70b-instruct"
            - "--tokenizer=/models/llama3-70b"
            - "--dtype=bfloat16"
            - "--quantization=awq_marlin"
            - "--max-model-len=8192"
            - "--gpu-memory-utilization=0.90"
            - "--enable-chunked-prefill"
            - "--max-num-batched-tokens=8192"
            - "--tensor-parallel-size=1"
            - "--port=8000"
          ports:
            - containerPort: 8000
              name: http
          resources:
            requests:
              cpu: "8"
              memory: "32Gi"
              nvidia.com/gpu: "1"
            limits:
              cpu: "16"
              memory: "64Gi"
              nvidia.com/gpu: "1"
          volumeMounts:
            - name: model-weights
              mountPath: /models
              readOnly: true
            - name: hf-cache
              mountPath: /root/.cache/huggingface
          readinessProbe:
            httpGet:
              path: /health
              port: 8000
            initialDelaySeconds: 90
            periodSeconds: 10
            failureThreshold: 6
          livenessProbe:
            httpGet:
              path: /health
              port: 8000
            initialDelaySeconds: 120
            periodSeconds: 30
            failureThreshold: 3
          env:
            - name: VLLM_LOGGING_LEVEL
              value: "WARNING"
            - name: OTEL_EXPORTER_OTLP_ENDPOINT
              value: "http://otel-collector.observability:4317"
            - name: OTEL_SERVICE_NAME
              value: "vllm-llama3-70b"
```

vLLM Deployment manifest with GPU resource requests, node scheduling, and model mount
Several decisions in this manifest are worth calling out explicitly. Leave `--trust-remote-code` unset (vLLM defaults to False); do not pass `=false` — that form is rejected by argparse and the container exits before start. `--quantization=awq_marlin` selects the Marlin-optimised AWQ kernel; specifying `awq` explicitly forces the slower reference kernel — vLLM only auto-selects Marlin when quantization is left unset. [The initContainer that verifies model weight digests via `SHA256SUMS`](https://stribog.com/blog/oss-supply-chain-security-sbom-sigstore-slsa-kubernetes) is the kind of paranoia that looks unnecessary until it catches a corrupted or tampered weight shard in production — at which point it pays for itself many times over.
The model weights live on a PVC, not inside the container image. This is load-bearing. A 70B model at four-bit quantization is roughly 38GB of weight data; at eight-bit it is roughly 75GB. Either way, bundling weights in a Docker image defeats layer caching, makes pulls absurdly slow, and prevents sharing across deployments. The PVC pattern allows multiple pods to mount the same weights read-only, and lets you version the PVC independently of the inference server image.
Click to zoomFull production architecture. The inference gateway handles authentication and rate limiting before requests reach vLLM. The audit store is append-only and stays inside the legal perimeter.
## The Inference Gateway: Rate Limiting, Authentication, and RAG Integration
vLLM's built-in HTTP server exposes an OpenAI-compatible API. That is convenient for client compatibility but it is not a production-ready gateway. It has no authentication, no per-user or per-service rate limiting, no request enrichment for audit logging, and no circuit breaking. The inference gateway layer fills that gap.
Envoy proxy with the JWT authentication filter (`envoy.filters.http.jwt_authn`) is the right choice here. It intercepts every request, validates a JWT issued by your identity provider, and passes user identity downstream as request headers that vLLM passes through to its log output. Rate limiting in this excerpt uses Envoy local_ratelimit (per process). For multi-replica global limits, add envoy.filters.http.ratelimit with a Redis-backed RLS. The gateway also handles context enrichment for RAG: for workloads that use retrieval-augmented generation, the gateway or a sidecar calls pgvector before forwarding to vLLM, injecting the retrieved chunks into the prompt template. This keeps RAG logic out of the vLLM server itself, which should stay stateless.
pgvector running inside the cluster as a PostgreSQL extension is the practical choice for regulated environments. Managed vector databases (Pinecone, Weaviate Cloud, etc.) reintroduce the same data-residency problem that motivated self-hosted inference in the first place: your retrieval corpus, which may contain sensitive documents, transits to a third-party SaaS. A PostgreSQL pod with pgvector satisfies the same retrieval workload for most enterprise RAG use cases, [runs inside your perimeter](https://stribog.com/blog/cilium-ebpf-kubernetes-zero-trust-networking-service-mesh), and benefits from the same backup and access-control infrastructure you already operate — see [our comparison of Qdrant and pgvector for the retrieval tier](https://stribog.com/blog/qdrant-pgvector-self-hosted-vector-database-on-prem-rag) for when it is worth reaching for a purpose-built store instead.
yamlCopy

```
# envoy-ratelimit-config.yaml — mounted as ConfigMap into the Envoy gateway pod
static_resources:
  listeners:
    - name: ingress
      address:
        socket_address:
          address: 0.0.0.0
          port_value: 8080
      filter_chains:
        - filters:
            - name: envoy.filters.network.http_connection_manager
              typed_config:
                "@type": type.googleapis.com/envoy.extensions.filters.network.http_connection_manager.v3.HttpConnectionManager
                stat_prefix: ai_gateway
                http_filters:
                  # 1. JWT validation — extracts sub, email, roles
                  - name: envoy.filters.http.jwt_authn
                    typed_config:
                      "@type": type.googleapis.com/envoy.extensions.filters.http.jwt_authn.v3.JwtAuthentication
                      providers:
                        internal_idp:
                          issuer: "https://auth.internal.example.com"
                          audiences:
                            - "ai-inference"
                          remote_jwks:
                            http_uri:
                              uri: "https://auth.internal.example.com/.well-known/jwks.json"
                              cluster: auth_cluster
                              timeout: 5s
                          forward_payload_header: "x-jwt-payload"
                          claim_to_headers:
                            - header_name: "x-jwt-sub"
                              claim_name: "sub"
                  # Gateway-wide default bucket: 60 req/min shared across all callers (not per-user)
                  - name: envoy.filters.http.local_ratelimit
                    typed_config:
                      "@type": type.googleapis.com/envoy.extensions.filters.http.local_ratelimit.v3.LocalRateLimit
                      stat_prefix: ai_ratelimit
                      token_bucket:
                        max_tokens: 60
                        tokens_per_fill: 60
                        fill_interval: 60s
                      filter_enabled:
                        default_value:
                          numerator: 100
                          denominator: HUNDRED
                      filter_enforced:
                        default_value:
                          numerator: 100
                          denominator: HUNDRED
                      response_headers_to_add:
                        - header:
                            key: X-RateLimit-Limit
                            value: "60"
                  # 3. Audit enrichment — injects request_id for log correlation
                  - name: envoy.filters.http.lua
                    typed_config:
                      "@type": type.googleapis.com/envoy.extensions.filters.http.lua.v3.Lua
                      default_source_code:
                        inline_string: |
                          function envoy_on_request(handle)
                            local req_id = handle:headers():get("x-request-id")
                            handle:headers():add("x-audit-request-id", req_id)
                            handle:headers():add("x-audit-ts", tostring(os.time()))
                          end
                  - name: envoy.filters.http.router
```

Envoy JWT authn + local_ratelimit for the inference gateway — process-wide default bucket
## Model Versioning and A/B Testing Without the MLflow Complexity Tax
Model versioning is where teams reach for MLflow and end up running a complex experiment-tracking platform for what is really a Kubernetes deployment problem. Models are weights on a PVC; versions are labeled PVCs and Deployments; the A/B split is traffic weights on the Service — the same canary pattern as any application.
The concrete pattern: each model version gets its own Deployment with a `model-version` label. The Service selector matches `app: vllm` without pinning to a specific version. Traffic weighting between the current stable model and a candidate is handled by the inference gateway routing rules, which can split by percentage or by a specific request header (useful for internal dogfooding before general rollout). The vLLM `--served-model-name` argument controls what name the model advertises to clients; using a stable alias (`llama3-70b-instruct`) decoupled from the specific weight version means clients do not need to change when you promote a new checkpoint.
Rollback is a `kubectl rollout undo` or a gateway routing change — no MLflow state, experiment database, or pipeline artifact store to manage. Richer experiment tracking remains valuable for model evaluation; for production routing it is overkill. Optionality means the simplest infrastructure that gives the control you need, not an entire MLOps platform because the concept sounds sophisticated.
## Observability: What to Instrument, What to Log, and Why the Audit Trail Matters
Observability for LLM inference has three distinct layers: operational metrics (is the system healthy?), performance metrics (is it fast enough?), and audit logs (what exactly ran, for whom, and what did it produce?). The first two are standard OpenTelemetry work. The third is distinct and compliance-critical.
vLLM exposes a Prometheus-compatible `/metrics` endpoint with the key operational signals: `vllm:num_requests_running` (current active batch), `vllm:gpu_cache_usage_perc` (KV cache utilization — if this reaches 100% you are blocking or preempting), `vllm:time_to_first_token_seconds` (histogram), and `vllm:request_success_total` (labelled by `finish_reason`) and gateway HTTP 5xx counters — vLLM has no request-failure counter. These wire directly into a standard Prometheus scrape configuration via the pod annotations shown in the Deployment manifest above. Loki captures structured logs from both vLLM and the gateway.
The audit log is a different beast. For high-risk AI systems, Article 12's automatic event-logging duty is satisfied in production by a durable, integrity-protected record that — as an engineering interpretation for LLM post-market monitoring — includes: the input prompt (or a cryptographic digest if the prompt is too large), the model output, the model version and configuration hash, the user identity (from the JWT `sub` claim), the risk classification of the workload, and the timestamp with microsecond precision. This record must be immutable, retained for the period prescribed by sectoral law (at least six months under Article 19(1)), and accessible only to authorized auditors.
The implementation is a structured event emitted by the gateway's Lua enrichment filter, collected by OpenTelemetry, and written to an in-cluster audit store configured with append-only semantics. [The audit store can be as simple as a PostgreSQL instance](https://stribog.com/blog/kubernetes-secrets-management-external-secrets-operator-vault) with an append-only table enforced by row-level security, or a Loki instance with immutability rules enabled. What it cannot be is a third-party SaaS or a cloud-managed service, because the legal perimeter requirement is absolute: the record must stay inside your organization's infrastructure for the retention period. See the [sovereignty thesis](https://stribog.com/thesis) for the broader framing of why infrastructure ownership is the prerequisite for this class of compliance.
yamlCopy

```
# otel-collector-config.yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: "0.0.0.0:4317"
      http:
        endpoint: "0.0.0.0:4318"

processors:
  # Extract audit-relevant attributes from inference spans
  attributes/audit:
    actions:
      - key: audit.model_version
        from_attribute: model.version
        action: insert
      - key: audit.user_id
        from_attribute: http.request.header.x-jwt-sub
        action: insert
      - key: audit.risk_class
        from_attribute: http.request.header.x-risk-class
        action: insert
      - key: audit.request_id
        from_attribute: http.request.header.x-audit-request-id
        action: insert
  # Batch for efficiency
  batch:
    timeout: 1s
    send_batch_size: 512

exporters:
  # Operational metrics → Prometheus
  prometheus:
    endpoint: "0.0.0.0:9090"
    namespace: "vllm"

  # Logs → Loki via OTLP HTTP (native loki exporter removed in collector-contrib v0.131+)
  otlphttp/loki:
    endpoint: "http://loki.observability:3100/otlp"

  # Audit trail → append-only store inside the legal perimeter (base URL; otlphttp appends /v1/{signal})
  otlphttp/audit:
    endpoint: "https://audit-store.ai-compliance:8443"
    headers:
      x-audit-sink: "true"
    tls:
      insecure: false
      ca_file: /etc/ssl/audit-store-ca.crt

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [attributes/audit, batch]
      exporters: [otlphttp/audit]
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [prometheus]
    logs:
      receivers: [otlp]
      processors: [attributes/audit, batch]
      exporters: [otlphttp/loki, otlphttp/audit]
```

OpenTelemetry Collector pipeline — routes inference spans to both Prometheus/Loki and the audit log storeClick to zoomEvery inference request produces a structured audit event containing prompt, completion, model version, user identity, risk classification, and timestamp. The store never leaves the legal perimeter; authorized export is the only egress path.
### Connecting to the EU AI Act Article 12 Audit Logging Requirement
Article 12 requires automatic event logging suitable for risk identification and post-market monitoring; the specific field list in Article 12(3) applies only to remote biometric identification. Logging prompt, completion, model version, user identity and risk class is an engineering interpretation for LLM post-market monitoring — not a statutory enumeration for LLM systems generally. In practice the record must still be complete enough to reconstruct what the model was asked and what it answered, in context, at a later date.
The engineering implementation that supports post-market monitoring in practice is: input logging (either full prompt or a verifiable digest plus a stored copy), output logging (the full completion, since the risk may be in the model's answer, not the question), model version pinning (so the log references a specific, reproducible model configuration rather than just a model name), user identity binding (the log must connect to a natural or organizational person who can be contacted for investigation), and retention with integrity guarantees (the log cannot be modified or deleted during the retention period).
The architecture described here satisfies all five of those engineering properties within a single legal perimeter. The OTel pipeline captures input and output at the gateway layer, where the full prompt and response are available before any filtering or truncation. The `model-version` label on the Deployment manifest, combined with a configuration hash in the audit event, pins the model configuration. The JWT `sub` claim provides user identity. The append-only audit store with restricted delete permissions satisfies the integrity requirement. And critically, none of this touches infrastructure outside the organization's control.
> The audit trail is not a compliance artifact you bolt on at the end. It is an architectural primitive you design in from the start, because retrofitting immutable logging onto a system that was not built to produce it is significantly harder than the inference engineering itself.
Stribog engineering practice
## The Cost Crossover: At What Volume Does Self-Hosted Beat Cloud APIs
The economic argument for self-hosted inference is straightforward once you work the numbers honestly. Cloud API pricing is per-token with no fixed cost. On-premises infrastructure has high fixed cost (hardware capital or colo lease) and near-zero marginal cost per request. The crossover point depends on your request volume, your model quality requirements, and your amortization period.
For this analysis, the baseline is a Llama-3.1-8B-Instruct class model — competitive quality with GPT-4o-mini on most enterprise task-specific workloads, which is where the volume actually lives. Cloud API cost is $0.30 per million tokens blended (input + output at current GPT-4o-mini pricing). Hyperscaler GPU is $2.00/hr for an A100 on-demand instance. Colocation amortized is $700/month for a server carrying a single A100 (≈$694/mo hardware amortisation on a $25K server over 36 months, rounded; facility colo is modelled separately if you lease rack space on top).
At 10,000 requests per month averaging 1,000 tokens per request, cloud API costs $3. Colo fixed cost is $700 regardless of volume — obviously not justified. At 100,000 requests per month (100M tokens), cloud API is $30. Colo is still $700. At 1,000,000 requests per month, cloud API is $300 and colo is $700 plus about $24 in power ($724) — not yet a crossover, but close. At 10,000,000 requests per month, cloud API is $3,000 and colo is about $780 all-in ($700 fixed plus power at scale). The hyperscaler GPU instance is even more expensive than cloud API at that volume because you pay for idle time.
The crossover between cloud API and amortized colo lands at approximately 2.3 million requests per month at the token volumes above — around $700/mo in API cost. For an internal tooling workload that serves 50 engineers each making 100 AI-assisted queries per workday, that is roughly 100,000 requests per month, well below crossover. For a customer-facing product with 10,000 active users making 10 AI requests per day, that is 3 million requests per month, firmly in colo-wins territory. The break-even is not particularly high.
Click to zoomAssumes Llama-3.1-8B-class model, $0.30/1M tokens cloud API blended, $2.00/hr A100 on-demand, $700/mo amortized colo. At 10M req/mo, colo is about 4x cheaper than cloud API ($3,000 vs ~$780).
This is also where the [optionality argument](https://stribog.com/thesis) has concrete teeth. Once you own the inference hardware and are running at scale, switching from Llama-3.1 to a future open-weight model is a PVC swap and a Deployment update. Switching cloud API providers requires contract negotiation, API adapter changes, and potential re-evaluation of data processing agreements. Owning the inference layer is owning the exit ramp from every future foundation model negotiation.
## What Production-Grade Actually Requires: The Non-Negotiable List
"Production-grade" is often used as a vague quality signal. For self-hosted LLM inference, it has a specific meaning. A deployment is production-grade when it can handle all of the following without human intervention: a single GPU pod crash (the other replica keeps serving while Kubernetes reschedules), a model weight corruption (the initContainer catches it and blocks the pod start before it serves bad output), a traffic spike beyond the rate limit (the gateway returns 429 rather than OOM-killing the inference pod), a GPU memory exhaustion event (vLLM's preemption model degrades gracefully to sequential processing rather than crashing), and an audit request from a regulator (the export path produces a complete, integrity-verified log without requiring any operational changes to the running system).
None of these are exotic — they are the same properties you require of any production database or API service. The LLM stack is younger and the defaults less hardened. The Deployment and gateway configs here encode most of them; the audit log architecture handles compliance. What remains is operational maturity: runbooks, vLLM metric alerts, load testing before production, and GPU node monitoring for memory bandwidth saturation.
The [capabilities we bring to this problem](https://stribog.com/capabilities) include GPU infrastructure design, vLLM production configuration, inference gateway integration, audit logging architecture, and EU AI Act compliance validation. If you are at a regulated company building toward sovereign AI infrastructure, the path is tractable. It requires engineering rigor, not a $2M cluster. [The Kubernetes primitives you already operate are the foundation](https://stribog.com/blog/platform-engineering-golden-paths-policy-enforcement-kubernetes). vLLM is the inference engine. The audit log architecture is the compliance proof. Everything else is configuration.
§FAQ/Common questions
## Frequently asked
Can vLLM run on consumer GPUs like RTX 4090?+
Yes. vLLM runs on any CUDA-capable GPU with sufficient VRAM. An RTX 4090 with 24GB VRAM runs Llama-3.1-8B at full bfloat16 precision comfortably, or a 13B model with four-bit quantization. For a 70B model at eight-bit or higher you need at minimum two 48GB cards (A6000 or RTX 6000 Ada) in tensor-parallel mode, or a single 80GB A100/H100. At four-bit (~38GB of weights) a single 48GB card works for low-concurrency use — see the sizing section for the KV-cache budget that decides this. Consumer GPUs work well for development and low-concurrency internal tooling; for production throughput at >20 concurrent requests, workstation-class or data-center GPUs are more economical per unit of throughput.
How does vLLM handle model loading time? Can I avoid cold starts in Kubernetes?+
Model loading on a 70B eight-bit model takes 45-90 seconds depending on storage bandwidth. The readinessProbe with a 90-second initialDelaySeconds in the manifest handles this: the pod only receives traffic after the model is warm. The higher initialDelaySeconds avoids spurious restarts when storage is slow. For faster pod restarts, use a node-local NVMe PVC (storage class with local-ssd provisioner) rather than network-attached storage — loading from local NVMe is typically 3-5x faster than from a network PVC. For zero-cold-start requirements, keep a minimum of 2 replicas at all times and use PodDisruptionBudgets to prevent both from restarting simultaneously.
What does the EU AI Act Article 12 audit log need to contain exactly?+
Article 12 requires automatic recording of events relevant to risk identification and post-market monitoring; Article 12(3)'s detailed field list is scoped to remote biometric identification, not LLMs generally. For LLM post-market monitoring, an engineering interpretation that holds up under audit is: complete input (or a cryptographic hash plus a stored copy), complete output, model identifier and version, timestamp precise enough for incident reconstruction, user or system identity, and workload risk class. The log must be tamper-evident (append-only with integrity checks) and retained at least six months under Article 19(1), longer where sectoral law requires it.
Can I use a managed Kubernetes service (EKS, GKE, AKS) and still be EU AI Act compliant?+
This depends on what the workload is classified as and who the operator is. For high-risk AI systems under the EU AI Act, the data sovereignty requirement means the audit log must remain within a legal perimeter that the deploying organization controls and that is not subject to conflicting foreign jurisdiction. A managed K8s service in an EU region with EU data residency guarantees gets you data location compliance, but the CLOUD Act issue — where a US-parent provider may be compelled to produce data regardless of storage location — remains. For strict compliance, in-region data residency plus a European-incorporated cloud provider, or on-premises/colo, is the architecturally safe choice. Consult your legal team for workload-specific determination.
How do I handle model updates without downtime?+
Use the labeled Deployment pattern described in the article. Create a new Deployment with the updated model version label, pointing to a new PVC snapshot of the updated weights. Verify the new Deployment is healthy and passing readiness checks. Update the gateway routing rule to shift a percentage of traffic (start at 5-10%) to the new version. Monitor quality metrics and error rates for 24-48 hours. If stable, shift to 100% and then delete the old Deployment. If the new version has issues, shifting the gateway back to 0% is an immediate rollback with no Kubernetes rollback required. The entire process is zero-downtime if you keep at least 1 replica of the old version running until the new version is fully promoted.
Is vLLM the only inference server worth considering?+
No. TGI (Text Generation Inference by Hugging Face) and Triton Inference Server are mature alternatives. TGI has strong Hugging Face ecosystem integration and good performance on Llama-family models. Triton is better suited to multi-model serving across different frameworks (PyTorch, TensorRT, ONNX) and is the right choice if you are running heterogeneous model types on the same infrastructure. vLLM's PagedAttention and OpenAI-compatible API make it the lowest-friction choice for teams already building against OpenAI client libraries, which is why it leads in enterprise inference deployments as of 2026. The Kubernetes deployment patterns in this article apply with minor changes to TGI and Triton as well.
self-hosted LLM Kubernetes vLLM productionon-premises AI inference enterprise 2026vLLM Kubernetes deployment GPUlocal LLM inference sovereign AIEU AI Act compliant LLM deploymentself-hosted llama kubernetes
Further reading
  * [Slurm vs Kubernetes for GPU Work: Batch Queue or Service Fleet](https://stribog.com/blog/slurm-vs-kubernetes-gpu-scheduling-batch-versus-service)
  * [AI Governance Best Practices: Inventory What Is Running](https://stribog.com/blog/ai-governance-best-practices-model-inventory-self-hosted)
  * [EU AI Act Summary: What Actually Binds a Deployer](https://stribog.com/blog/eu-ai-act-summary-deployer-obligations-self-hosted-inference)
  * [On-Premise AI: The Decision Order for the Whole Stack](https://stribog.com/blog/on-premise-ai-infrastructure-decision-guide-sovereign-stack)
  * [vLLM vs Ollama: drawing the concurrency line](https://stribog.com/blog/vllm-vs-ollama-self-hosted-llm-serving-production-choice)
  * [ISO 42001 for self-hosted inference: clause to artefact](https://stribog.com/blog/iso-42001-ai-management-system-self-hosted-inference-evidence)
  * [QLoRA infrastructure: fine-tuning you actually own](https://stribog.com/blog/qlora-fine-tuning-infrastructure-self-hosted-sovereign-model-customization)
  * [KEDA and Descheduler: Two-Tier Autoscaling on Bare Metal](https://stribog.com/blog/keda-descheduler-bare-metal-event-driven-autoscaling-scale-to-zero)
  * [MIG vs Time-Slicing: Sharing GPUs on Kubernetes](https://stribog.com/blog/nvidia-gpu-operator-mig-time-slicing-kubernetes-sharing)
  * [GPU and VRAM Sizing for Self-Hosted LLM Inference](https://stribog.com/blog/self-hosted-llm-hardware-gpu-vram-sizing-sovereign-inference-capacity)
  * [Tabby vs Continue: Self-Hosted Coding Assistants on vLLM](https://stribog.com/blog/self-hosted-coding-llm-continue-tabby-vllm-sovereign-developer-assistant)
  * [HIPAA-Compliant LLM: On-Prem PHI Inference Without a BAA](https://stribog.com/blog/hipaa-compliant-self-hosted-llm-phi-on-prem-inference-baa)
  * [LiteLLM as an MCP Gateway: Sovereign AI Data Residency](https://stribog.com/blog/litellm-self-hosted-mcp-model-gateway-ai-data-residency)
  * [Confidential computing: protecting data-in-use with hardware TEEs](https://stribog.com/blog/confidential-computing-kubernetes-remote-attestation-tee)
  * [On-Prem RAG: Qdrant vs pgvector for Sovereign Retrieval](https://stribog.com/blog/qdrant-pgvector-self-hosted-vector-database-on-prem-rag)
  * [Worked scenario: sovereign AI for a regulated healthcare SaaS](https://stribog.com/work/healthcare-saas-sovereign-ai)
  * [The sovereignty thesis: why engineering teams must own the systems they depend on](https://stribog.com/thesis)
  * [Sovereign AI: self-hosted inference service deep-dive](https://stribog.com/capabilities/sovereign-ai)
  * [The EU AI Act case for self-hosting inference](https://stribog.com/blog/nis2-dora-eu-ai-act-self-hosted-kubernetes-compliance)
  * [EU AI Act High-Risk Systems: An On-Prem Compliance Path](https://stribog.com/blog/eu-ai-act-high-risk-ai-systems-onprem-inference-compliance)
  * [Securing model and DB credentials](https://stribog.com/blog/kubernetes-secrets-management-external-secrets-operator-vault)
  * [AMD ROCm as a Second Source for GPU Inference](https://stribog.com/blog/amd-rocm-gpu-alternative-nvidia-dependency-sovereign-ai-compute)
  * [Zero-trust networking for inference](https://stribog.com/blog/cilium-ebpf-kubernetes-zero-trust-networking-service-mesh)
  * [Running AI on platform primitives](https://stribog.com/blog/platform-engineering-golden-paths-policy-enforcement-kubernetes)


Executive Briefing
## Thirty minutes to clarify your infrastructure risk
Walk us through your vendor footprint and regulatory constraints. We will tell you honestly where sovereignty creates leverage — and where it does not. No pitch deck. No obligation.
