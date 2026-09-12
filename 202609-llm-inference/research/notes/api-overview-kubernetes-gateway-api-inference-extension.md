---
title: API Overview - Kubernetes Gateway API Inference Extension
id: api-overview-kubernetes-gateway-api-inference-extension
tags:
- llmd-router-nonk8s-backends
- inferencepoolimport
- ext-proc
- ai-gateway-layering
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:13.172551Z'
updated: '2026-09-12T03:37:37.560839Z'
source: https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/
source_domain: gateway-api-inference-extension.sigs.k8s.io
fetched_at: '2026-09-11T19:44:13.153521Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'GIE concepts/api-overview doc. Confirms the extension is built on Envoy''s
  External Processing Protocol (ext-proc), extending any gateway supporting both ext-proc
  and Gateway API (named examples: Envoy Gateway, kgateway, GKE Gateway) into an Inference
  Gateway. States InferencePool ''represents a set of Inference-focused Pods'' and
  is explicitly positioned as a drop-in replacement for a Kubernetes Service (''you''d
  replace a Kubernetes Service with an InferencePool'') -- reinforcing Pod-only selector
  semantics at the spec level. Introduces InferencePoolImport: a cluster-local, CONTROLLER-managed
  (not user-authored) representation of an InferencePool imported from another cluster,
  populated by a multi-cluster controller, allowing traffic to be routed to model
  servers running in a remote cluster -- this is the one documented mechanism by which
  InferencePool-adjacent routing can reach endpoints outside the local cluster''s
  Pod set, worth investigating further as a potential integration seam for non-k8s
  backends. Also notes self-hosted models are meant to be composed with model-as-a-service
  providers via higher-level AI Gateways (LiteLLM, Gloo AI Gateway, Apigee) sitting
  above the Inference Gateway -- i.e., non-k8s/external providers are explicitly pushed
  to a different layer (an AI Gateway), not into InferencePool itself.'
utility_score: 10.0
---

# API Overview[¶](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/#api-overview "Permanent link")
## Background[¶](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/#background "Permanent link")
Gateway API Inference Extension optimizes self-hosting Generative AI Models on Kubernetes. It provides optimized load-balancing for self-hosted Generative AI Models on Kubernetes. The project’s goal is to improve and standardize routing to inference workloads across the ecosystem.
This is achieved by leveraging Envoy's [External Processing Protocol](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) to extend any gateway that supports both ext-proc and [Gateway API](https://github.com/kubernetes-sigs/gateway-api) into an [inference gateway](https://gateway-api-inference-extension.sigs.k8s.io/#concepts-and-definitions). This extension extends popular gateways like Envoy Gateway, kgateway, and GKE Gateway - to become [Inference Gateway](https://gateway-api-inference-extension.sigs.k8s.io/#concepts-and-definitions) - supporting inference platform teams self-hosting Generative Models (with a current focus on large language models) on Kubernetes. This integration makes it easy to expose and control access to your local [OpenAI-compatible chat completion endpoints](https://platform.openai.com/docs/api-reference/chat) to other workloads on or off cluster, or to integrate your self-hosted models alongside model-as-a-service providers in a higher level **AI Gateways** like [LiteLLM](https://www.litellm.ai/), [Gloo AI Gateway](https://www.solo.io/products/gloo-ai-gateway), or [Apigee](https://cloud.google.com/apigee).
## API Resources[¶](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/#api-resources "Permanent link")
Gateway API Inference Extension introduces inference-focused API resources with distinct responsibilities, each aligning with a specific user persona in the Generative AI serving workflow.
### InferencePool[¶](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/#inferencepool "Permanent link")
InferencePool represents a set of Inference-focused Pods and an extension that will be used to route to them. Within the broader Gateway API resource model, this resource is considered a "backend". In practice, that means that you'd replace a Kubernetes Service with an InferencePool. This resource has some similarities to Service (a way to select Pods and specify a port), but has some unique capabilities. With InferencePool, you can configure a routing extension as well as inference-specific routing optimizations. For more information on this resource, refer to our [InferencePool documentation](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool) or go directly to the [InferencePool spec](https://gateway-api-inference-extension.sigs.k8s.io/reference/spec/#inferencepool).
### InferencePoolImport[¶](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/#inferencepoolimport "Permanent link")
The `InferencePoolImport` resource is a cluster-local, controller-managed representation of an imported `InferencePool` from another cluster, allowing traffic to be routed to model servers running in a remote cluster. Unlike `InferencePool`, it is not user-authored but populated by the multi-cluster controller. For more information on this resource, refer to our [InferencePoolImport documentation](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepoolimport) or go directly to the [InferencePoolImport spec](https://gateway-api-inference-extension.sigs.k8s.io/reference/x-v1a1-spec/#inferencepoolimport).
Back to top 
