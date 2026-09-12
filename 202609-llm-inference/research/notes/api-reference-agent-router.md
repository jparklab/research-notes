---
title: API Reference | Agent Router
id: api-reference-agent-router
tags:
- llmd-router-nonk8s-backends-54b774
- epp
- envoy-ai-gateway
- api-reference
- inferencepool
created: '2026-09-11T19:44:05.096081Z'
updated: '2026-09-12T03:37:37.555922Z'
source: https://aigateway.envoyproxy.io/docs/api/
source_domain: aigateway.envoyproxy.io
fetched_at: '2026-09-11T19:44:05.074123Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Full CRD API reference (v1.1, project renamed to Agent Router / Agentic
  AI Foundation, formerly Envoy AI Gateway) for aigateway.envoyproxy.io v1alpha1/v1beta1.
  Load-bearing for the layered-backend question: AIGatewayRouteRuleBackendRef.BackendRefs
  can point at EITHER an AIServiceBackend (default; group=aigateway.envoyproxy.io)
  OR an InferencePool (group inference.networking.k8s.io) from Gateway API Inference
  Extension -- but only one InferencePool is allowed per rule, InferencePool cannot
  be mixed with AIServiceBackend refs in the same rule, and ''Fallback behavior is
  handled by the InferencePool''s endpoint picker'' whereas AIServiceBackend fallback
  is done via Envoy Gateway''s BackendTrafficPolicy (failover/retry). The backend-level
  ''priority'' field (sets Envoy upstream priority, overrides Envoy Gateway''s ''failover''
  property) is explicitly ''ignored when referencing InferencePool resources'' --
  confirming InferencePool backends are opaque to the gateway''s own priority/fallback
  mechanics. AIServiceBackendSpec.BackendRef today must be an Envoy Gateway Backend
  resource; the doc notes ''k8s Service will be supported as a backend in the future''
  citing envoyproxy/ai-gateway issue #902 -- i.e., even the external/self-hosted (non-InferencePool)
  backend path is still k8s-object-shaped today. BackendSecurityPolicy covers auth
  to external managed providers (API key, AWS IAM/STS, Anthropic, Azure AD/API key,
  GCP credentials, OIDC). Long/dense reference (~18k words) -- flagged for possible
  source-analyst follow-up on the full CRD field list.'
utility_score: 9.0
---

[Skip to main content](https://theagentrouter.ai/docs/api/#__docusaurus_skipToContent_fallback)
**Formerly Envoy AI Gateway** — now an Agentic AI Foundation project. Same code, same maintainers. [Read the announcement →](https://theagentrouter.ai/blog/envoy-ai-gateway-is-now-agent-router)
Version: 1.1
On this page
## Versions[​](https://theagentrouter.ai/docs/api/#versions "Direct link to Versions")
  * [aigateway.envoyproxy.io/v1alpha1](https://theagentrouter.ai/docs/api/#aigatewayenvoyproxyiov1alpha1)
  * [aigateway.envoyproxy.io/v1beta1](https://theagentrouter.ai/docs/api/#aigatewayenvoyproxyiov1beta1)


## aigateway.envoyproxy.io/v1alpha1[​](https://theagentrouter.ai/docs/api/#aigatewayenvoyproxyiov1alpha1 "Direct link to aigateway.envoyproxy.io/v1alpha1")
Package v1alpha1 contains API schema definitions for the aigateway.envoyproxy.io API group.
## Resource Kinds[​](https://theagentrouter.ai/docs/api/#resource-kinds "Direct link to Resource Kinds")
### Available Kinds[​](https://theagentrouter.ai/docs/api/#available-kinds "Direct link to Available Kinds")
  * [BackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicy)
  * [BackendSecurityPolicyList](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicylist)


### Kind Definitions[​](https://theagentrouter.ai/docs/api/#kind-definitions "Direct link to Kind Definitions")
####  AIGatewayRoute[​](https://theagentrouter.ai/docs/api/#aigatewayroute "Direct link to aigatewayroute")
**Appears in:**


AIGatewayRoute combines multiple AIServiceBackends and attaching them to Gateway(s) resources.
This serves as a way to define a "unified" AI API for a Gateway which allows downstream clients to use a single schema API to interact with multiple AI backends.
Envoy AI Gateway will generate the following k8s resources corresponding to the AIGatewayRoute:
  * HTTPRoute of the Gateway API as a top-level resource to bind all backends. The name of the HTTPRoute is the same as the AIGatewayRoute.
  * HTTPRouteFilter of the Envoy Gateway API per namespace for automatic hostname rewrite. The name of the HTTPRouteFilter is `ai-eg-host-rewrite-${AIGatewayRoute.Name}`.


All of these resources are created in the same namespace as the AIGatewayRoute. Note that this is the implementation detail subject to change. If you want to customize the default behavior of the Envoy AI Gateway, you can use these resources as a reference and create your own resources. Alternatively, you can use EnvoyPatchPolicy API of the Envoy Gateway to patch the generated resources. For example, you can configure the retry fallback behavior by attaching BackendTrafficPolicy API of Envoy Gateway to the generated HTTPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `AIGatewayRoute` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
Spec defines the details of the AIGatewayRoute.
`status`required
[AIGatewayRouteStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayroutestatus)
Status defines the status details of the AIGatewayRoute.
####  AIGatewayRouteList[​](https://theagentrouter.ai/docs/api/#aigatewayroutelist "Direct link to aigatewayroutelist")
AIGatewayRouteList contains a list of AIGatewayRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-1 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `AIGatewayRouteList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[AIGatewayRoute](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayroute) array
####  AIServiceBackend[​](https://theagentrouter.ai/docs/api/#aiservicebackend "Direct link to aiservicebackend")
**Appears in:**


AIServiceBackend is a resource that represents a single backend for AIGatewayRoute. A backend is a service that handles traffic with a concrete API specification.
A AIServiceBackend is "attached" to a Backend which is either a k8s Service or a Backend resource of the Envoy Gateway.
When a backend with an attached AIServiceBackend is used as a routing target in the AIGatewayRoute (more precisely, the HTTPRouteSpec defined in the AIGatewayRoute), the ai-gateway will generate the necessary configuration to do the backend specific logic in the final HTTPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-2 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `AIServiceBackend` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
[AIServiceBackendSpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aiservicebackendspec)
Spec defines the details of AIServiceBackend.
`status`required
[AIServiceBackendStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aiservicebackendstatus)
Status defines the status details of the AIServiceBackend.
####  AIServiceBackendList[​](https://theagentrouter.ai/docs/api/#aiservicebackendlist "Direct link to aiservicebackendlist")
AIServiceBackendList contains a list of AIServiceBackends.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-3 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `AIServiceBackendList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[AIServiceBackend](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aiservicebackend) array
####  BackendSecurityPolicy[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicy "Direct link to backendsecuritypolicy")
**Appears in:**
  * [BackendSecurityPolicyList](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicylist)


BackendSecurityPolicy specifies configuration for authentication and authorization rules on the traffic exiting the gateway to the backend.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-4 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `BackendSecurityPolicy` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
[BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)
`status`required
[BackendSecurityPolicyStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicystatus)
Status defines the status details of the BackendSecurityPolicy.
####  BackendSecurityPolicyList[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicylist "Direct link to backendsecuritypolicylist")
BackendSecurityPolicyList contains a list of BackendSecurityPolicy
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-5 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `BackendSecurityPolicyList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[BackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicy) array
####  GatewayConfig[​](https://theagentrouter.ai/docs/api/#gatewayconfig "Direct link to gatewayconfig")
**Appears in:**


GatewayConfig provides configuration for the AI Gateway external processor container that is deployed alongside the Gateway.
A GatewayConfig is referenced by a Gateway via the annotation "aigateway.envoyproxy.io/gateway-config". The GatewayConfig must be in the same namespace as the Gateway that references it.
This allows gateway-level configuration of the external processor, including environment variables (e.g., for tracing configuration) and resource requirements.
Multiple Gateways can reference the same GatewayConfig to share configuration.
Environment Variable Precedence: When merging environment variables, the following precedence applies (highest to lowest):
  1. GatewayConfig.Spec.ExtProc.Kubernetes.Env (this resource)
  2. Global controller flags (extProcExtraEnvVars)


If the same environment variable name exists in both sources, the GatewayConfig value takes precedence.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-6 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `GatewayConfig` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
Spec defines the configuration for the external processor.
`status`required
Status defines the status of the GatewayConfig.
####  GatewayConfigList[​](https://theagentrouter.ai/docs/api/#gatewayconfiglist "Direct link to gatewayconfiglist")
GatewayConfigList contains a list of GatewayConfig.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-7 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `GatewayConfigList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[GatewayConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gatewayconfig) array
**Appears in:**


MCPRoute defines how to route MCP requests to the backend MCP servers.
This serves as a way to define a "unified" AI API for a Gateway which allows downstream clients to use a single schema API to interact with multiple MCP backends.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-8 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `MCPRoute` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
Spec defines the details of the MCPRoute.
`status`required
Status defines the status details of the MCPRoute.
MCPRouteList contains a list of MCPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-9 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `MCPRouteList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[MCPRoute](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcproute) array
**Appears in:**


QuotaPolicy specifies token quota configuration for inference services. Generates rate limit configuration and tracks quota usage. Reject requests with 429 once all related quota to that request has been exceeded.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-10 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `QuotaPolicy` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
`status`required
Status defines the status details of the QuotaPolicy.
####  QuotaPolicyList[​](https://theagentrouter.ai/docs/api/#quotapolicylist "Direct link to quotapolicylist")
QuotaPolicyList contains a list of QuotaPolicy
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-11 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1alpha1` of the API.
`kind`required
String
This is a `QuotaPolicyList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[QuotaPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-quotapolicy) array
## Supporting Types[​](https://theagentrouter.ai/docs/api/#supporting-types "Direct link to Supporting Types")
### Available Types[​](https://theagentrouter.ai/docs/api/#available-types "Direct link to Available Types")
  * [AIGatewayRouteRuleBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayrouterulebackendref)
  * [AIGatewayRouteRuleMatch](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayrouterulematch)
  * [AIServiceBackendStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aiservicebackendstatus)
  * [AzureOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-azureoidcexchangetoken)
  * [BackendSecurityPolicyAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyapikey)
  * [BackendSecurityPolicyAWSCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyawscredentials)
  * [BackendSecurityPolicyAnthropicAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyanthropicapikey)
  * [BackendSecurityPolicyAzureAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyazureapikey)
  * [BackendSecurityPolicyAzureCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyazurecredentials)
  * [BackendSecurityPolicyGCPCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicygcpcredentials)
  * [BackendSecurityPolicyOIDC](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyoidc)
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)
  * [BackendSecurityPolicyStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicystatus)
  * [BackendSecurityPolicyType](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicytype)
  * [GCPServiceAccountImpersonationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpserviceaccountimpersonationconfig)
  * [GCPWorkloadIdentityFederationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpworkloadidentityfederationconfig)
  * [GCPWorkloadIdentityProvider](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpworkloadidentityprovider)
  * [GatewayConfigForwardProxy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gatewayconfigforwardproxy)
  * [MCPAuthorizationSource](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpauthorizationsource)
  * [MCPAuthorizationTarget](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpauthorizationtarget)
  * [MCPBackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpbackendsecuritypolicy)
  * [MCPBackendSelectorRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpbackendselectorrule)
  * [MCPRouteAuthorization](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcprouteauthorization)
  * [MCPRouteAuthorizationRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcprouteauthorizationrule)
  * [MCPRouteSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcproutesecuritypolicy)
  * [ProtectedResourceMetadata](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-protectedresourcemetadata)
  * [ServiceQuotaDefinition](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-servicequotadefinition)


### Type Definitions[​](https://theagentrouter.ai/docs/api/#type-definitions "Direct link to Type Definitions")
####  AIGatewayRouteRule[​](https://theagentrouter.ai/docs/api/#aigatewayrouterule "Direct link to aigatewayrouterule")
**Appears in:**


AIGatewayRouteRule is a rule that defines the routing behavior of the AIGatewayRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-12 "Direct link to Fields")
`name`optional
Name is the name of the route rule. This name must be unique within the route.When specified, it is copied to the generated HTTPRoute rule name.
`backendRefs`optional
[AIGatewayRouteRuleBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayrouterulebackendref) array
BackendRefs is the list of backends that this rule will route the traffic to.Each backend can have a weight that determines the traffic distribution.The namespace of each backend defaults to the same namespace as the AIGatewayRoute when not specified.Cross-namespace references are supported by specifying the namespace field.When a namespace different than the AIGatewayRoute's namespace is specified,a ReferenceGrant object is required in the referent namespace to allow thatnamespace's owner to accept the reference.BackendRefs can reference either AIServiceBackend resources (default) or InferencePool resourcesfrom the Gateway API Inference Extension. When referencing InferencePool resources:- Only one InferencePool backend is allowed per rule- Cannot mix InferencePool with AIServiceBackend references in the same rule- Fallback behavior is handled by the InferencePool's endpoint pickerFor AIServiceBackend references, you can achieve fallback behavior by configuring multiple backendscombined with the BackendTrafficPolicy of Envoy Gateway.Please refer to https://gateway.envoyproxy.io/docs/tasks/traffic/failover/ as well ashttps://gateway.envoyproxy.io/docs/tasks/traffic/retry/.
`matches`optional
[AIGatewayRouteRuleMatch](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayrouterulematch) array
Matches is the list of AIGatewayRouteMatch that this rule will match the traffic to.This is a subset of the HTTPRouteMatch in the Gateway API. See for the details:https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.HTTPRouteMatch
`timeouts`optional
Timeouts defines the timeouts that can be configured for an HTTP request.If this field is not set, or the timeout.requestTimeout is nil, Envoy AI Gateway defaults toset 60s for the request timeout as opposed to 15s of the Envoy Gateway's default value.For streaming responses (like chat completions with stream=true), consider settinglonger timeouts as the response may take time until the completion. Timeouts.Requestacts as the maximum total time the gateway will wait for the entire response,including all streamed chunks.
`streamIdleTimeout`optional
StreamIdleTimeout is the maximum time Envoy will wait without receiving any bytes from the upstream.If the timer fires before the first response byte arrives, Envoy resets the upstream stream and aretry policy can fall over to the next backend. If it fires mid-stream afterbytes have already arrived, the stream is cut and the client receives a 504.The AI Gateway extension server sets route.retry_policy.per_try_idle_timeout to this value onevery xDS route generated from this rule before it is sent to the data plane.Pair this field with Timeouts.Request, which acts as the overall deadline.If this field is not set, no per-try idle timeout is applied.
`modelsOwnedBy`optional
string
Envoy AI Gateway
ModelsOwnedBy represents the owner of the running models serving by the backends,which will be exported as the field of `OwnedBy` in openai-compatible API `/models`.This is used only when this rule contains `x-ai-eg-model` in its header matchingwhere the header value will be recognized as a `model` in `/models` endpoint.All the matched models will share the same owner.Default to `Envoy AI Gateway` if not set.
`modelsCreatedAt`optional
ModelsCreatedAt represents the creation timestamp of the running models serving by the backends,which will be exported as the field of `Created` in openai-compatible API `/models`.It follows the format of RFC 3339, for example `2024-05-21T10:00:00Z`.This is used only when this rule contains `x-ai-eg-model` in its header matchingwhere the header value will be recognized as a `model` in `/models` endpoint.All the matched models will share the same creation time.Default to the creation timestamp of the AIGatewayRoute if not set.
####  AIGatewayRouteRuleBackendRef[​](https://theagentrouter.ai/docs/api/#aigatewayrouterulebackendref "Direct link to aigatewayrouterulebackendref")
**Appears in:**


AIGatewayRouteRuleBackendRef is a reference to a backend with a weight. It can reference either an AIServiceBackend or an InferencePool resource.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-13 "Direct link to Fields")
`name`required
string
Name is the name of the backend resource.When Group and Kind are not specified, this refers to an AIServiceBackend.When Group and Kind are specified, this refers to the resource of the specified type.
`namespace`optional
Namespace is the namespace of the backend resource.When unspecified (or empty string), this refers to the local namespace of the AIGatewayRoute.Note that when a namespace different than the local namespace is specified,a ReferenceGrant object is required in the referent namespace to allow thatnamespace's owner to accept the reference. See the ReferenceGrantdocumentation for details.
`group`optional
string
Group is the group of the backend resource.When not specified, defaults to aigateway.envoyproxy.io (AIServiceBackend).Currently, only `inference.networking.k8s.io` is supported for InferencePool resources.
`kind`optional
string
Kind is the kind of the backend resource.When not specified, defaults to AIServiceBackend.Currently, only `InferencePool` is supported when Group is specified.
`modelNameOverride`optional
string
Name of the model in the backend. If provided this will override the name provided in the request.This field is ignored when referencing InferencePool resources.
`headerMutation`optional
HeaderMutation defines the request header mutation to be applied to this backend.When both route-level and backend-level HeaderMutation are defined,route-level takes precedence over backend-level for conflicting operations.This field is ignored when referencing InferencePool resources.
`bodyMutation`optional
BodyMutation defines the request body mutation to be applied to this backend.This allows modification of JSON fields in the request body before sending to the backend.When both route-level and backend-level BodyMutation are defined,route-level takes precedence over backend-level for conflicting operations.This field is ignored when referencing InferencePool resources.
`weight`optional
integer
1
Weight is the weight of the backend. This is exactly the same as the weight inthe BackendRef in the Gateway API. See for the details:https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.BackendRefDefault is 1.
`priority`optional
integer
0
Priority is the priority of the backend. This sets the priority on the underlying endpoints.See: https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/load_balancing/priorityNote: This will override the `faillback` property of the underlying Envoy Gateway BackendThis field is ignored when referencing InferencePool resources.Default is 0.
####  AIGatewayRouteRuleMatch[​](https://theagentrouter.ai/docs/api/#aigatewayrouterulematch "Direct link to aigatewayrouterulematch")
**Appears in:**


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-14 "Direct link to Fields")
`headers`optional
[HTTPHeaderMatch](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#httpheadermatch) array
Headers specifies HTTP request header matchers. See HeaderMatch in the Gateway API for the details:https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.HTTPHeaderMatch
####  AIGatewayRouteSpec[​](https://theagentrouter.ai/docs/api/#aigatewayroutespec "Direct link to aigatewayroutespec")
**Appears in:**


AIGatewayRouteSpec details the AIGatewayRoute configuration.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-15 "Direct link to Fields")
`parentRefs`optional
[ParentReference](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#parentreference) array
ParentRefs are the names of the Gateway resources this AIGatewayRoute is being attached to.Currently, each reference's Kind must be Gateway.
`hostnames`optional
Hostname array
Hostnames is a list of hostnames matched against the HTTP Host header to select an AIGatewayRouteused to process the request. This is equivalent to the Hostnames field in the Gateway API HTTPRouteSpec.When specified, the generated HTTPRoute will include these hostnames for hostname-based filtering.See https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.HTTPRouteSpecfor the details of the Hostnames field in the Gateway API.
`rules`required
[AIGatewayRouteRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayrouterule) array
Rules is the list of AIGatewayRouteRule that this AIGatewayRoute will match the traffic to.Each rule is a subset of the HTTPRoute in the Gateway API (https://gateway-api.sigs.k8s.io/api-types/httproute/).AI Gateway controller will generate a HTTPRoute based on the configuration given here with the additionalmodifications to achieve the necessary jobs, notably inserting the AI Gateway filter responsible forthe transformation of the request and response, etc.In the matching conditions in the AIGatewayRouteRule, `x-ai-eg-model` header is availableif we want to describe the routing behavior based on the model name. The model name is extractedfrom the request content before the routing decision.How multiple rules are matched is the same as the Gateway API. See for the details:https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.HTTPRouteAt most 15 rules are allowed per AIGatewayRoute, corresponding to the Gateway API's limit onHTTPRoute.spec.rules (one slot is reserved for a controller-injected catch-all rule). Toconfigure more rules on the same Gateway, split them across multiple AIGatewayRoute resources.
`llmRequestCosts`optional
[LLMRequestCost](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-llmrequestcost) array
LLMRequestCosts specifies how to capture the cost of the LLM-related request, notably the token usage.The AI Gateway filter will capture each specified number and store it in the Envoy's dynamicmetadata per HTTP request. The namespaced key is `io.envoy.ai_gateway`.These route-level costs override any global defaults defined in GatewayConfig.Spec.GlobalLLMRequestCostsfor the same metadataKey. If a metadataKey is not defined in either place, no cost is calculated for it.This allows you to define common cost formulas once at the gateway level (e.g., via GatewayConfig)and only override them in specific routes when needed (e.g., premium routes with different pricing).For example, let's say we have the following LLMRequestCosts configuration:
```
	llmRequestCosts:	- metadataKey: llm_input_token	  type: InputToken	- metadataKey: llm_output_token	  type: OutputToken	- metadataKey: llm_total_token	  type: TotalToken	- metadataKey: llm_cached_input_token	  type: CachedInputToken- metadataKey: llm_cache_creation_input_token   type: CacheCreationInputToken
```
Then, with the following BackendTrafficPolicy of Envoy Gateway, you can have threerate limit buckets for each unique x-tenant-id header value. One bucket is for the input token,the other is for the output token, and the last one is for the total token.Each bucket will be reduced by the corresponding token usage captured by the AI Gateway filter.
```
	apiVersion: gateway.envoyproxy.io/v1alpha1	kind: BackendTrafficPolicy	metadata:	  name: some-example-token-rate-limit	  namespace: default	spec:	  targetRefs:	  - group: gateway.networking.k8s.io	     kind: HTTPRoute	     name: usage-rate-limit	  rateLimit:	    type: Global	    global:	      rules:	        - clientSelectors:	            # Do the rate limiting based on the x-tenant-id header.	            - headers:	                - name: x-tenant-id	                  type: Distinct	          limit:	            # Configures the number of tokens allowed per hour.	            requests: 10000	            unit: Hour	          cost:	            request:	              from: Number	              # Setting the request cost to zero allows to only check the rate limit budget,	              # and not consume the budget on the request path.	              number: 0	            # This specifies the cost of the response retrieved from the dynamic metadata set by the AI Gateway filter.	            # The extracted value will be used to consume the rate limit budget, and subsequent requests will be rate limited	            # if the budget is exhausted.	            response:	              from: Metadata	              metadata:	                namespace: io.envoy.ai_gateway	                key: llm_input_token	        - clientSelectors:	            - headers:	                - name: x-tenant-id	                  type: Distinct	          limit:	            requests: 10000	            unit: Hour	          cost:	            request:	              from: Number	              number: 0	            response:	              from: Metadata	              metadata:	                namespace: io.envoy.ai_gateway	                key: llm_output_token	        - clientSelectors:	            - headers:	                - name: x-tenant-id	                  type: Distinct	          limit:	            requests: 10000	            unit: Hour	          cost:	            request:	              from: Number	              number: 0	            response:	              from: Metadata	              metadata:	                namespace: io.envoy.ai_gateway	                key: llm_total_token
```
Note that when multiple AIGatewayRoute resources are attached to the same Gateway, anddifferent costs are configured for the same metadata key, each route's rule is carried inthe filter configuration with the route identity; the data plane selects the matching ruleper request (by route), so each route can define its own cost for the same metadata key.
####  AIGatewayRouteStatus[​](https://theagentrouter.ai/docs/api/#aigatewayroutestatus "Direct link to aigatewayroutestatus")
**Appears in:**


AIGatewayRouteStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-16 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
####  AIServiceBackendSpec[​](https://theagentrouter.ai/docs/api/#aiservicebackendspec "Direct link to aiservicebackendspec")
**Appears in:**


AIServiceBackendSpec details the AIServiceBackend configuration.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-17 "Direct link to Fields")
`schema`required
APISchema specifies the API schema of the output format of requests fromEnvoy that this AIServiceBackend can accept as incoming requests.Based on this schema, the ai-gateway will perform the necessary transformation forthe pair of AIGatewayRouteSpec.APISchema and AIServiceBackendSpec.APISchema.This is required to be set.
`backendRef`required
BackendRef is the reference to the Backend resource that this AIServiceBackend corresponds to.A backend must be a Backend resource of Envoy Gateway. Note that k8s Service will be supportedas a backend in the future. See https://github.com/envoyproxy/ai-gateway/issues/902 for more details.This is required to be set.
`headerMutation`optional
HeaderMutation defines the mutation of HTTP headers that will be applied to the requestbefore sending it to the backend.
`bodyMutation`optional
BodyMutation defines the mutation of HTTP request body JSON fields that will be applied to the requestbefore sending it to the backend.
####  AIServiceBackendStatus[​](https://theagentrouter.ai/docs/api/#aiservicebackendstatus "Direct link to aiservicebackendstatus")
**Appears in:**


AIServiceBackendStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-18 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
**Underlying type:** string
**Appears in:**


APISchema defines the API schema.
##### Possible Values[​](https://theagentrouter.ai/docs/api/#possible-values "Direct link to Possible Values")
`OpenAI`
APISchemaOpenAI is the OpenAI schema.https://github.com/openai/openai-openapi 
`Cohere`
APISchemaCohere is the Cohere schema.https://docs.cohere.com/v2 
`AWSBedrock`
APISchemaAWSBedrock is the AWS Bedrock schema.https://docs.aws.amazon.com/bedrock/latest/APIReference/API_Operations_Amazon_Bedrock_Runtime.html 
`AzureOpenAI`
APISchemaAzureOpenAI APISchemaAzure is the Azure OpenAI schema.https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#api-specs 
`GCPVertexAI`
APISchemaGCPVertexAI is the schema followed by Gemini models hosted on GCP's Vertex AI platform.Note: Using this schema requires a BackendSecurityPolicy to be configured and attached,as the transformation will use the gcp-region and project-name from the BackendSecurityPolicy.https://cloud.google.com/vertex-ai/docs/reference/rest/v1/projects.locations.endpoints/generateContent?hl=en 
`GCPAnthropic`
APISchemaGCPAnthropic is the schema for Anthropic models hosted on GCP's Vertex AI platform.Returns native Anthropic format responses for seamless integration.https://docs.anthropic.com/en/api/claude-on-vertex-ai 
`Anthropic`
APISchemaAnthropic is the native Anthropic API schema.https://docs.claude.com/en/home 
`AWSAnthropic`
APISchemaAWSAnthropic is the schema for Anthropic models hosted on AWS Bedrock.Uses the native Anthropic Messages API format for requests and responses.When used with /v1/chat/completions endpoint, translates OpenAI format to Anthropic.When used with /v1/messages endpoint, passes through native Anthropic format.https://aws.amazon.com/bedrock/anthropic/https://docs.claude.com/en/api/claude-on-amazon-bedrock 
####  AWSCredentialsFile[​](https://theagentrouter.ai/docs/api/#awscredentialsfile "Direct link to awscredentialsfile")
**Appears in:**
  * [BackendSecurityPolicyAWSCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyawscredentials)


AWSCredentialsFile specifies the credentials file to use for the AWS provider. Envoy reads the secret file, and the profile to use is specified by the Profile field.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-19 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the credential file.The secret should contain the AWS credentials file keyed on `credentials`.
`profile`required
string
default
Profile is the profile to use in the credentials file.
####  AWSOIDCExchangeToken[​](https://theagentrouter.ai/docs/api/#awsoidcexchangetoken "Direct link to awsoidcexchangetoken")
**Appears in:**
  * [BackendSecurityPolicyAWSCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyawscredentials)


AWSOIDCExchangeToken specifies credentials to obtain oidc token from a sso server. For AWS, the controller will query STS to obtain AWS AccessKeyId, SecretAccessKey, and SessionToken, and store them in a temporary credentials file.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-20 "Direct link to Fields")
`oidc`required
OIDC is used to obtain oidc tokens via an SSO server which will be used to exchange for provider credentials.
`grantType`optional
string
GrantType is the method application gets access token.
`aud`optional
string
Aud defines the audience that this ID Token is intended for.
`awsRoleArn`required
string
AwsRoleArn is the AWS IAM Role with the permission to use specific resources in AWS accountwhich maps to the temporary AWS security credentials exchanged using the authentication token issued by OIDC provider.
####  AzureOIDCExchangeToken[​](https://theagentrouter.ai/docs/api/#azureoidcexchangetoken "Direct link to azureoidcexchangetoken")
**Appears in:**
  * [BackendSecurityPolicyAzureCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyazurecredentials)


AzureOIDCExchangeToken specifies credentials to obtain oidc token from a sso server. For Azure, the controller will query Azure Entra ID to get an Azure Access Token, and store them in a secret.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-21 "Direct link to Fields")
`oidc`required
OIDC is used to obtain oidc tokens via an SSO server which will be used to exchange for provider credentials.
`grantType`optional
string
GrantType is the method application gets access token.
`aud`optional
string
Aud defines the audience that this ID Token is intended for.
####  BackendSecurityPolicyAPIKey[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyapikey "Direct link to backendsecuritypolicyapikey")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)


BackendSecurityPolicyAPIKey specifies the API key.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-22 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the secret containing the API key.ai-gateway must be given the permission to read this secret.The key of the secret should be `apiKey`.
####  BackendSecurityPolicyAWSCredentials[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyawscredentials "Direct link to backendsecuritypolicyawscredentials")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)


BackendSecurityPolicyAWSCredentials contains the supported authentication mechanisms to access AWS.
When neither CredentialsFile nor OIDCExchangeToken is specified, the AWS SDK's default credential chain will be used. This automatically supports various authentication methods in the following order:
  1. Environment variables (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN)
  2. EKS Pod Identity - automatically rotates credentials for pods in EKS clusters
  3. IAM Roles for Service Accounts (IRSA) - injects credentials via mounted service account tokens
  4. EC2 instance metadata (IAM instance roles)
  5. ECS task roles


The default credential chain is recommended for Kubernetes deployments as it supports automatic credential rotation without manual configuration. Credentials are refreshed automatically when they approach expiration (typically hourly for IRSA and Pod Identity).
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-23 "Direct link to Fields")
`region`required
string
Region specifies the AWS region associated with the policy.
`credentialsFile`optional
CredentialsFile specifies the credentials file to use for the AWS provider.When specified, this takes precedence over the default credential chain.
`oidcExchangeToken`optional
[AWSOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-awsoidcexchangetoken)
OIDCExchangeToken specifies the oidc configurations used to obtain an oidc token. The oidc token will beused to obtain temporary credentials to access AWS.When specified, this takes precedence over the default credential chain.
####  BackendSecurityPolicyAnthropicAPIKey[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyanthropicapikey "Direct link to backendsecuritypolicyanthropicapikey")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)


BackendSecurityPolicyAnthropicAPIKey specifies the Anthropic API key.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-24 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the secret containing the Anthropic API key.ai-gateway must be given the permission to read this secret.The key of the secret should be `apiKey`.
####  BackendSecurityPolicyAzureAPIKey[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyazureapikey "Direct link to backendsecuritypolicyazureapikey")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)


BackendSecurityPolicyAzureAPIKey specifies the Azure OpenAI API key.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-25 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the secret containing the Azure API key.ai-gateway must be given the permission to read this secret.The key of the secret should be `apiKey`.
####  BackendSecurityPolicyAzureCredentials[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyazurecredentials "Direct link to backendsecuritypolicyazurecredentials")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)


BackendSecurityPolicyAzureCredentials contains the supported authentication mechanisms to access Azure. Only one of ClientSecretRef or OIDCExchangeToken must be specified. Credentials will not be generated if neither are set.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-26 "Direct link to Fields")
`clientID`required
string
ClientID is a unique identifier for an application in Azure.
`tenantID`required
string
TenantId is a unique identifier for an Azure Active Directory instance.
`clientSecretRef`optional
ClientSecretRef is the reference to the secret containing the Azure client secret.ai-gateway must be given the permission to read this secret.The key of secret should be `client-secret`.
`oidcExchangeToken`optional
[AzureOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-azureoidcexchangetoken)
OIDCExchangeToken specifies the oidc configurations used to obtain an oidc token. The oidc token will beused to obtain temporary credentials to access Azure.
####  BackendSecurityPolicyGCPCredentials[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicygcpcredentials "Direct link to backendsecuritypolicygcpcredentials")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)


BackendSecurityPolicyGCPCredentials contains the supported authentication mechanisms to access GCP.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-27 "Direct link to Fields")
`projectName`required
string
ProjectName is the GCP project name.
`region`required
string
Region is the GCP region associated with the policy.
`credentialsFile`optional
CredentialsFile specifies the service account credentials file to use for the GCP provider.
`workloadIdentityFederationConfig`optional
[GCPWorkloadIdentityFederationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpworkloadidentityfederationconfig)
WorkloadIdentityFederationConfig is the configuration for the GCP Workload Identity Federation.
####  BackendSecurityPolicyOIDC[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyoidc "Direct link to backendsecuritypolicyoidc")
**Appears in:**
  * [AzureOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-azureoidcexchangetoken)
  * [GCPWorkloadIdentityProvider](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpworkloadidentityprovider)


BackendSecurityPolicyOIDC specifies OIDC related fields.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-28 "Direct link to Fields")
`oidc`required
OIDC is used to obtain oidc tokens via an SSO server which will be used to exchange for provider credentials.
`grantType`optional
string
GrantType is the method application gets access token.
`aud`optional
string
Aud defines the audience that this ID Token is intended for.
####  BackendSecurityPolicySpec[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyspec "Direct link to backendsecuritypolicyspec")
**Appears in:**
  * [BackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicy)


BackendSecurityPolicySpec specifies authentication rules on access the provider from the Gateway. Only one mechanism to access a backend(s) can be specified.
Only one type of BackendSecurityPolicy can be defined.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-29 "Direct link to Fields")
`targetRefs`optional
[LocalPolicyTargetReference](https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io/v1alpha2.LocalPolicyTargetReference) array
TargetRefs are the names of the AIServiceBackend or InferencePool resources this BackendSecurityPolicy is being attached to.Attaching multiple BackendSecurityPolicies to the same resource is invalid and will result in an errorduring the reconciliation of the resource.
`type`required
[BackendSecurityPolicyType](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicytype)
Type specifies the type of the backend security policy.
`apiKey`optional
[BackendSecurityPolicyAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyapikey)
APIKey is a mechanism to access a backend(s). The API key will be injected into the Authorization header.
`awsCredentials`optional
[BackendSecurityPolicyAWSCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyawscredentials)
AWSCredentials is a mechanism to access a backend(s). AWS specific logic will be applied.
`azureAPIKey`optional
[BackendSecurityPolicyAzureAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyazureapikey)
AzureAPIKey is a mechanism to access Azure OpenAI backend(s). The API key will be injected into the api-key header.
`azureCredentials`optional
[BackendSecurityPolicyAzureCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyazurecredentials)
AzureCredentials is a mechanism to access a backend(s). Azure OpenAI specific logic will be applied.
`gcpCredentials`optional
[BackendSecurityPolicyGCPCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicygcpcredentials)
GCPCredentials is a mechanism to access a backend(s). GCP specific logic will be applied.
`anthropicAPIKey`optional
[BackendSecurityPolicyAnthropicAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyanthropicapikey)
AnthropicAPIKey is a mechanism to access Anthropic backend(s). The API key will be injected into the `x-api-key` header.https://docs.claude.com/en/api/overview#authentication
####  BackendSecurityPolicyStatus[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicystatus "Direct link to backendsecuritypolicystatus")
**Appears in:**
  * [BackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicy)


BackendSecurityPolicyStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-30 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
####  BackendSecurityPolicyType[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicytype "Direct link to backendsecuritypolicytype")
**Underlying type:** string
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicyspec)


BackendSecurityPolicyType specifies the type of auth mechanism used to access a backend.
##### Possible Values[​](https://theagentrouter.ai/docs/api/#possible-values-1 "Direct link to Possible Values")
`APIKey`
`AWSCredentials`
`AzureAPIKey`
`AnthropicAPIKey`
`AzureCredentials`
`GCPCredentials`
####  GCPCredentialsFile[​](https://theagentrouter.ai/docs/api/#gcpcredentialsfile "Direct link to gcpcredentialsfile")
**Appears in:**
  * [BackendSecurityPolicyGCPCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicygcpcredentials)


GCPCredentialsFile specifies the service account key json file to authenticate with GCP provider.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-31 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the credential file.The secret should contain the GCP service account credentials file keyed on `service_account.json`.
####  GCPOIDCExchangeToken[​](https://theagentrouter.ai/docs/api/#gcpoidcexchangetoken "Direct link to gcpoidcexchangetoken")
**Appears in:**
  * [GCPWorkloadIdentityFederationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpworkloadidentityfederationconfig)


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-32 "Direct link to Fields")
`oidc`required
OIDC is used to obtain oidc tokens via an SSO server which will be used to exchange for provider credentials.
`grantType`optional
string
GrantType is the method application gets access token.
`aud`optional
string
Aud defines the audience that this ID Token is intended for.
####  GCPServiceAccountImpersonationConfig[​](https://theagentrouter.ai/docs/api/#gcpserviceaccountimpersonationconfig "Direct link to gcpserviceaccountimpersonationconfig")
**Appears in:**
  * [GCPWorkloadIdentityFederationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpworkloadidentityfederationconfig)


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-33 "Direct link to Fields")
`serviceAccountName`required
string
ServiceAccountName is the name of the service account to impersonate.
####  GCPWorkloadIdentityFederationConfig[​](https://theagentrouter.ai/docs/api/#gcpworkloadidentityfederationconfig "Direct link to gcpworkloadidentityfederationconfig")
**Appears in:**
  * [BackendSecurityPolicyGCPCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-backendsecuritypolicygcpcredentials)


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-34 "Direct link to Fields")
`projectID`required
string
ProjectID is the GCP project ID.
`workloadIdentityProviderName`required
string
WorkloadIdentityProviderName is the name of the external identity provider as registered on Google Cloud Platform.
`oidcExchangeToken`required
[GCPOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpoidcexchangetoken)
OIDCExchangeToken specifies the oidc configurations used to obtain an oidc token. The oidc token will beused to obtain temporary credentials to access GCP.
`workloadIdentityPoolName`required
string
WorkloadIdentityPoolName is the name of the workload identity pool defined in GCP.https://cloud.google.com/iam/docs/workload-identity-federation?hl=en
`serviceAccountImpersonation`optional
[GCPServiceAccountImpersonationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gcpserviceaccountimpersonationconfig)
ServiceAccountImpersonation is the service account impersonation configuration.This is used to impersonate a service account when getting access token.
####  GatewayConfigExtProc[​](https://theagentrouter.ai/docs/api/#gatewayconfigextproc "Direct link to gatewayconfigextproc")
**Appears in:**


GatewayConfigExtProc holds runtime-specific configuration for the external processor.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-35 "Direct link to Fields")
`kubernetes`optional
[KubernetesContainerSpec](https://gateway.envoyproxy.io/docs/api/extension_types/#kubernetescontainerspec)
Kubernetes defines the configuration for running the external processor as a Kubernetes container.
####  GatewayConfigForwardProxy[​](https://theagentrouter.ai/docs/api/#gatewayconfigforwardproxy "Direct link to gatewayconfigforwardproxy")
**Appears in:**


GatewayConfigForwardProxy configures an HTTP CONNECT forward proxy for upstream egress.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-36 "Direct link to Fields")
`address`required
string
Address is the `host:port` of the HTTP CONNECT proxy. The host may be a hostname or anIP address; the port is required. Upstream connections are tunnelled through this proxyvia Envoy's http_11_proxy transport socket, preserving the upstream TLS session.
####  GatewayConfigSpec[​](https://theagentrouter.ai/docs/api/#gatewayconfigspec "Direct link to gatewayconfigspec")
**Appears in:**


GatewayConfigSpec defines the configuration for the AI Gateway.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-37 "Direct link to Fields")
`extProc`optional
[GatewayConfigExtProc](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gatewayconfigextproc)
ExtProc defines the configuration for the external processor container.
`globalLLMRequestCosts`optional
[LLMRequestCost](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-llmrequestcost) array
GlobalLLMRequestCosts defines default LLM request costs that apply to allroutes referencing this GatewayConfig. These costs can be overridden on aper-route basis via AIGatewayRoute.Spec.LLMRequestCosts.When a request matches a route, the cost calculation proceeds as follows: 1. If the route defines LLMRequestCosts with a matching metadataKey, use that. 2. Otherwise, fall back to the global cost with that metadataKey (if defined here). 3. If neither exists, the cost is not calculated for that metadataKey.This allows you to define common cost formulas once at the gateway level(e.g., billing_charges = input_tokens + output_tokens) and only overridethem in specific routes when needed (e.g., premium routes with different pricing).
`forwardProxy`optional
[GatewayConfigForwardProxy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-gatewayconfigforwardproxy)
ForwardProxy routes all upstream AI/LLM traffic from Gateways referencing thisGatewayConfig through an HTTP CONNECT forward proxy. This is intended for data planesin locked-down networks where direct egress to providers is not permitted and alloutbound traffic must traverse a proxy.
####  GatewayConfigStatus[​](https://theagentrouter.ai/docs/api/#gatewayconfigstatus "Direct link to gatewayconfigstatus")
**Appears in:**


GatewayConfigStatus defines the observed state of GatewayConfig.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-38 "Direct link to Fields")
`conditions`optional
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions describe the current conditions of the GatewayConfig.
####  HTTPBodyField[​](https://theagentrouter.ai/docs/api/#httpbodyfield "Direct link to httpbodyfield")
**Appears in:**


HTTPBodyField represents a JSON field name and value for body mutation
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-39 "Direct link to Fields")
`path`required
string
Path is the top-level field name to set in the request body.Examples: `service_tier`, `max_tokens`, `temperature`
`value`required
string
Value is the JSON value to set at the specified field. This can be any valid JSON value:string, number, boolean, object, array, or null.The value will be parsed as JSON and inserted at the specified field.Examples: - `\`scale\\`` (string)  - `42` (number)  - `true` (boolean)  - `\\{\`key\`: \`value\`\\}` (object)  - `[1, 2, 3]` (array)  - `null` (null)
####  HTTPBodyMutation[​](https://theagentrouter.ai/docs/api/#httpbodymutation "Direct link to httpbodymutation")
**Appears in:**
  * [AIGatewayRouteRuleBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayrouterulebackendref)


HTTPBodyMutation defines the mutation of HTTP request body JSON fields that will be applied to the request
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-40 "Direct link to Fields")
`set`optional
[HTTPBodyField](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-httpbodyfield) array
Set overwrites/adds the request body with the given JSON field (name, value)before sending to the backend. Only top-level fields are currently supported.Input: \\{ `model`: `gpt-4`, `service_tier`: `default` \\}Config: set: - path: `service_tier` value: `scale`Output: \\{ `model`: `gpt-4`, `service_tier`: `scale` \\}
`remove`optional
string array
Remove the given JSON field(s) from the HTTP request body before sending to the backend.The value of Remove is a list of top-level field names to remove.Input: \\{ `model`: `gpt-4`, `service_tier`: `default`, `internal_flag`: true \\}Config: remove: [`service_tier`, `internal_flag`]Output: \\{ `model`: `gpt-4` \\}
####  HTTPHeaderMutation[​](https://theagentrouter.ai/docs/api/#httpheadermutation "Direct link to httpheadermutation")
**Appears in:**
  * [AIGatewayRouteRuleBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-aigatewayrouterulebackendref)


HTTPHeaderMutation defines the mutation of HTTP headers that will be applied to the request
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-41 "Direct link to Fields")
`set`optional
[HTTPHeader](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#httpheader) array
Set overwrites/adds the request with the given header (name, value)before the action.Input: GET /foo HTTP/1.1 my-header: fooConfig: set: - name: `my-header` value: `bar`Output: GET /foo HTTP/1.1 my-header: bar
`remove`optional
string array
Remove the given header(s) from the HTTP request before the action. Thevalue of Remove is a list of HTTP header names. Note that the headernames are case-insensitive (seehttps://datatracker.ietf.org/doc/html/rfc2616#section-4.2).Input: GET /foo HTTP/1.1 my-header1: foo my-header2: bar my-header3: bazConfig: remove: [`my-header1`, `my-header3`]Output: GET /foo HTTP/1.1 my-header2: bar
**Appears in:**


JWKS defines how to obtain JSON Web Key Sets (JWKS) either from a remote HTTP/HTTPS endpoint or from a local source.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-42 "Direct link to Fields")
`remoteJWKS`optional
RemoteJWKS defines how to fetch and cache JSON Web Key Sets (JWKS) from a remoteHTTP/HTTPS endpoint.
`localJWKS`optional
LocalJWKS defines how to get the JSON Web Key Sets (JWKS) from a local source.
**Appears in:**
  * [MCPAuthorizationSource](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpauthorizationsource)


JWTSource defines the MCP authorization source for JWT tokens. At least one of scopes or claims must be provided. Scopes and claims are AND-ed: when both are specified, both sets must match.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-43 "Direct link to Fields")
`scopes`optional
JWTScope array
Scopes defines the list of JWT scopes required for the rule.If multiple scopes are specified, all scopes must be present in the JWT for the rule to match.
`claims`optional
JWTClaim array
Claims defines the list of JWT claims required for the rule. Each claim must exist on the tokenand have at least one of the expected values. Use to enforce tenant or subject-based access.
####  LLMRequestCost[​](https://theagentrouter.ai/docs/api/#llmrequestcost "Direct link to llmrequestcost")
**Appears in:**


LLMRequestCost configures each request cost.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-44 "Direct link to Fields")
`metadataKey`required
string
MetadataKey is the key of the metadata to store this cost of the request.
`type`required
Type specifies the type of the request cost. The default is `OutputToken`,and it uses `output token` as the cost. The other types are `InputToken`, `TotalToken`,`CachedInputToken`, `CacheCreationInputToken`, `ReasoningToken`, and `CEL`.
`cel`optional
string
CEL is the CEL expression to calculate the cost of the request.The CEL expression must return a signed or unsigned integer. If thereturn value is negative, it will be error.The expression can use the following variables: * model: the model name extracted from the request content. Type: string. * backend: the backend name in the form of `name.namespace`. Type: string. * input_tokens: the number of input tokens. Type: unsigned integer. * cached_input_tokens: the number of cached read input tokens. Type: unsigned integer. * cache_creation_input_tokens: the number of cache creation input tokens. Type: unsigned integer. * output_tokens: the number of output tokens. Type: unsigned integer. * total_tokens: the total number of tokens. Type: unsigned integer. * reasoning_tokens: the number of reasoning tokens. Type: unsigned integer.For example, the following expressions are valid: * `model == 'llama' ?  input_tokens + output_token * 0.5 : total_tokens` * `backend == 'foo.default' ?  input_tokens + output_tokens : total_tokens` * `backend == 'bar.default' ?  (input_tokens - cached_input_tokens) + cached_input_tokens * 0.1 + cache_creation_input_tokens * 1.25 + output_tokens : total_tokens` * `input_tokens + output_tokens + total_tokens` * `input_tokens * output_tokens`
####  LLMRequestCostType[​](https://theagentrouter.ai/docs/api/#llmrequestcosttype "Direct link to llmrequestcosttype")
**Underlying type:** string
**Appears in:**


LLMRequestCostType specifies the type of the LLMRequestCost.
##### Possible Values[​](https://theagentrouter.ai/docs/api/#possible-values-2 "Direct link to Possible Values")
`InputToken`
LLMRequestCostTypeInputToken is the cost type of the input token. 
`CachedInputToken`
LLMRequestCostTypeCachedInputToken is the cost type of the cached input token. 
`CacheCreationInputToken`
LLMRequestCostTypeCacheCreationInputToken is the cost type of the cached input token. 
`OutputToken`
LLMRequestCostTypeOutputToken is the cost type of the output token. 
`TotalToken`
LLMRequestCostTypeTotalToken is the cost type of the total token. 
`ReasoningToken`
LLMRequestCostTypeReasoningToken is the cost type of the reasoning token. 
LLMRequestCostTypeCEL is for calculating the cost using the CEL expression. 
####  MCPAuthorizationSource[​](https://theagentrouter.ai/docs/api/#mcpauthorizationsource "Direct link to mcpauthorizationsource")
**Appears in:**
  * [MCPRouteAuthorizationRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcprouteauthorizationrule)


MCPAuthorizationSource defines the source of an authorization rule.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-45 "Direct link to Fields")
`jwt`required
JWT defines the JWT scopes required for this rule to match.
####  MCPAuthorizationTarget[​](https://theagentrouter.ai/docs/api/#mcpauthorizationtarget "Direct link to mcpauthorizationtarget")
**Appears in:**
  * [MCPRouteAuthorizationRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcprouteauthorizationrule)


MCPAuthorizationTarget defines the target of an authorization rule.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-46 "Direct link to Fields")
`tools`required
[ToolCall](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-toolcall) array
Tools defines the list of tools this rule applies to.
####  MCPBackendAPIKey[​](https://theagentrouter.ai/docs/api/#mcpbackendapikey "Direct link to mcpbackendapikey")
**Appears in:**
  * [MCPBackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpbackendsecuritypolicy)


MCPBackendAPIKey defines the configuration for the API Key Authentication to a backend. When both `header` and `queryParam` are unspecified, the API key will be injected into the "Authorization" header by default.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-47 "Direct link to Fields")
`secretRef`optional
secretRef is the Kubernetes secret which contains the API keys.The key of the secret should be `apiKey`.
`inline`optional
string
Inline contains the API key as an inline string.
`header`optional
string
Header is the HTTP header to inject the API key into. If not specified,defaults to `Authorization`.When the header is `Authorization`, the injected header value will beprefixed with `Bearer `.Either one of Header or QueryParam can be specified to inject the API key.
`queryParam`optional
string
QueryParam is the HTTP query parameter to inject the API key into.For example, if QueryParam is set to `api_key`, and the API key is `mysecretkey`, the request URL will be modified to include`?api_key=mysecretkey`.Either one of Header or QueryParam can be specified to inject the API key.
####  MCPBackendSecurityPolicy[​](https://theagentrouter.ai/docs/api/#mcpbackendsecuritypolicy "Direct link to mcpbackendsecuritypolicy")
**Appears in:**


MCPBackendSecurityPolicy defines the security policy for a backend MCP server.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-48 "Direct link to Fields")
`apiKey`optional
APIKey is a mechanism to access a backend. The API key will be injected into the request headers.
####  MCPBackendSelector[​](https://theagentrouter.ai/docs/api/#mcpbackendselector "Direct link to mcpbackendselector")
**Appears in:**


MCPBackendSelector defines which of a MCPRoute's backends a request may fan out to.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-49 "Direct link to Fields")
`defaultAction`optional
Deny
DefaultAction is the action to take when no rules match. If unspecified, defaults to Deny.
`rules`optional
[MCPBackendSelectorRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpbackendselectorrule) array
Rules defines a list of backend selection rules.These rules are evaluated in order, the first matching rule will be applied,and the rest will be skipped.If no rules are defined, DefaultAction is applied to every candidate backend(defaults to Deny).
####  MCPBackendSelectorRule[​](https://theagentrouter.ai/docs/api/#mcpbackendselectorrule "Direct link to mcpbackendselectorrule")
**Appears in:**


MCPBackendSelectorRule defines a single backend selection rule.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-50 "Direct link to Fields")
`cel`optional
string
CEL specifies a Common Expression Language (CEL) expression, evaluated once foreach backend already listed in backendRefs. It does not parse a list of backendsout of the request. Each evaluation binds request.mcp.backend to the name of theone candidate backend under test, so the expression should answer `is this backendallowed`, not `which backends should be used`. The expression must return aboolean; evaluation errors or non-boolean results are treated as `no match`. Amatch means the rule applies, and Action below decides whether that's an Allow ora Deny for that backend.Example CEL expressions: * `request.mcp.backend in request.auth.jwt.claims.mcp_backends` * `(`,` + request.headers[`x-ai-eg-mcp-backend-subset`] + `,`).contains(`,` + request.mcp.backend + `,`)`Available attributes are the same as documented on MCPRouteAuthorizationRule.CEL,except request.mcp.method, request.mcp.tool, and request.mcp.params are notpopulated (no MCP method has been selected yet at backend-selection time).Note: The CEL expression support is experimental, and the attributesavailable to the expression may change in future releases.
`action`optional
Allow
Action is the decision for matching backends. If unspecified, defaults to Allow.
####  MCPHeaderForward[​](https://theagentrouter.ai/docs/api/#mcpheaderforward "Direct link to mcpheaderforward")
**Appears in:**


MCPHeaderForward specifies a header to extract from the incoming request and forward to a backend.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-51 "Direct link to Fields")
`name`required
string
Name is the header name to extract from the incoming client request.
`backendHeader`optional
string
BackendHeader is the header name to use when forwarding to the backend.If not specified, the original header name is used.
####  MCPRouteAuthorization[​](https://theagentrouter.ai/docs/api/#mcprouteauthorization "Direct link to mcprouteauthorization")
**Appears in:**
  * [MCPRouteSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcproutesecuritypolicy)


MCPRouteAuthorization defines the authorization configuration for a MCPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-52 "Direct link to Fields")
`defaultAction`optional
Deny
DefaultAction is the action to take when no rules match. If unspecified, defaults to Deny.
`rules`optional
[MCPRouteAuthorizationRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcprouteauthorizationrule) array
Rules defines a list of authorization rules.These rules are evaluated in order, the first matching rule will be applied,and the rest will be skipped.If no rules are defined, the default action will be applied to all requests.
####  MCPRouteAuthorizationRule[​](https://theagentrouter.ai/docs/api/#mcprouteauthorizationrule "Direct link to mcprouteauthorizationrule")
**Appears in:**
  * [MCPRouteAuthorization](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcprouteauthorization)


MCPRouteAuthorizationRule defines an authorization rule for MCPRoute based on the MCP authorization spec. Reference: <https://modelcontextprotocol.io/specification/draft/basic/authorization#scope-challenge-handling>
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-53 "Direct link to Fields")
`source`required
[MCPAuthorizationSource](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpauthorizationsource)
Source defines the authorization source for this rule.If not specified, the rule will match all sources.
`target`required
[MCPAuthorizationTarget](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpauthorizationtarget)
Target defines the authorization target for this rule.If not specified, the rule will match all targets.
`cel`optional
string
CEL specifies a Common Expression Language (CEL) expression evaluated for this rule.The expression must return a boolean; evaluation errors or non-boolean resultsare treated as `no match`.Example CEL expressions: * `request.method == `POST`request.headers[`x-custom-header`] == `AllowedValue`request.mcp.tool in [`toolA`, `toolB`]`Available attributes in the CEL expression:	* request.method: HTTP method such as GET or POST. Type: string.	* request.headers: map of headers with lowercased keys, first value only. Type: map[string]string.	* request.headers_all: map of headers with lowercased keys, all values. Type: map[string][]string.	* request.path: request path such as /mcp. Type: string.	* request.auth.jwt.claims: JWT claims when a bearer JWT is present. Type: map[string]any.	* request.auth.jwt.scopes: JWT scopes when a bearer JWT is present. Type: []string.	* request.mcp.method: MCP method such as tools/list or tools/call. Type: string.	* request.mcp.backend: upstream backend name (for example, `kiwi` or `github`). Type: string.	* request.mcp.tool: tool name without backend prefix (for example, `list_issues`). Type: string.	* request.mcp.params: parameters of the MCP method, including keys like `_meta` and `arguments`. Type: object.Note: The CEL expression support is experimental, and the attributesavailable to the expression may change in future releases.
`action`optional
Allow
Action is the authorization decision for matching requests. If unspecified, defaults to Allow.
####  MCPRouteBackendRef[​](https://theagentrouter.ai/docs/api/#mcproutebackendref "Direct link to mcproutebackendref")
**Appears in:**


MCPRouteBackendRef wraps a EG's BackendObjectReference to reference an MCP server.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-54 "Direct link to Fields")
`group`optional
Group is the group of the referent. For example, `gateway.networking.k8s.io`.When unspecified or empty string, core API group is inferred.
`kind`optional
Service
Kind is the Kubernetes resource kind of the referent. For example`Service`.Defaults to `Service` when not specified.ExternalName services can refer to CNAME DNS records that may liveoutside of the cluster and as such are difficult to reason about interms of conformance. They also may not be safe to forward to (seeCVE-2021-25740 for more information). Implementations SHOULD NOTsupport ExternalName Services.Support: Core (Services with a type other than ExternalName)Support: Implementation-specific (Services with type ExternalName)
`name`required
Name is the name of the referent.
`namespace`optional
Namespace is the namespace of the backend. When unspecified, the localnamespace is inferred.Note that when a namespace different than the local namespace is specified,a ReferenceGrant object is required in the referent namespace to allow thatnamespace's owner to accept the reference. See the ReferenceGrantdocumentation for details.Support: Core
`port`optional
Port specifies the destination port number to use for this resource.Port is required when the referent is a Kubernetes Service. In thiscase, the port number is the service port number, not the target port.For other resources, destination port might be derived from the referentresource or this field.
`path`optional
string
/mcp
Path is the HTTP endpoint path of the backend MCP server.If not specified, the default is `/mcp`.
`toolSelector`optional
ToolSelector filters the tools exposed by this MCP server.Supports exact matches and RE2-compatible regular expressions for both include and exclude patterns.If not specified, all tools from the MCP server are exposed.
`securityPolicy`optional
[MCPBackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpbackendsecuritypolicy)
SecurityPolicy is the security policy to apply to this MCP server.
`forwardHeaders`optional
[MCPHeaderForward](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpheaderforward) array
ForwardHeaders specifies HTTP headers to extract from the incoming client requestand forward to this backend MCP server.This enables per-user authentication passthrough (e.g., personal access tokens)without requiring OAuth configuration.Each entry specifies a header name to extract and an optional rename for the backend.
####  MCPRouteOAuth[​](https://theagentrouter.ai/docs/api/#mcprouteoauth "Direct link to mcprouteoauth")
**Appears in:**
  * [MCPRouteSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcproutesecuritypolicy)


MCPRouteOAuth defines a MCP spec compatible OAuth authentication configuration for a MCPRoute. Reference: <https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization>
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-55 "Direct link to Fields")
`issuer`required
string
Issuer is the authorization server's issuer identity.
`audiences`optional
string array
Audiences is a list of JWT audiences allowed access.It is recommended to set this field for token audience validation, as it is a security best practice to prevent token misuse.Reference: https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization#token-audience-binding-and-validation
`jwks`optional
JWKS defines how a JSON Web Key Sets (JWKS) can be obtained to verify the access tokens presented by the clients.If not specified, the JWKS URI will be discovered from the OAuth 2.0 Authorization Server Metadataas per RFC 8414 by querying the `/.well-known/oauth-authorization-server` endpoint on the Issuer.
`protectedResourceMetadata`required
[ProtectedResourceMetadata](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-protectedresourcemetadata)
ProtectedResourceMetadata defines the OAuth 2.0 Resource Server Metadata as per RFC 8414.This is used to expose the metadata endpoint for mcp clients to discover the authorization servers,supported scopes, and JWKS URI.
`claimToHeaders`optional
ClaimToHeader array
ClaimToHeaders specifies JWT claims to extract and forward as HTTP headers to backend MCP servers.This enables backends to access user identity for authorization, auditing, or personalization.Security considerations:- Any client-provided headers matching the configured header names will be stripped to prevent forgery- Only the specified claims are extracted; the full JWT is not forwarded to backends- Consider using a header prefix (e.g., `X-Jwt-Claim-`) to avoid conflicts with other headers
####  MCPRouteSecurityPolicy[​](https://theagentrouter.ai/docs/api/#mcproutesecuritypolicy "Direct link to mcproutesecuritypolicy")
**Appears in:**


MCPRouteSecurityPolicy defines the security policy for a MCPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-56 "Direct link to Fields")
`oauth`optional
OAuth defines the configuration for the MCP spec compatible OAuth authentication.
`apiKeyAuth`optional
APIKeyAuth defines the configuration for the API Key Authentication.
`extAuth`optional
ExtAuth defines the configuration for External Authorization.
`authorization`optional
[MCPRouteAuthorization](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcprouteauthorization)
Authorization defines the configuration for the MCP spec compatible authorization.
**Appears in:**


MCPRouteSpec details the MCPRoute configuration.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-57 "Direct link to Fields")
`parentRefs`required
[ParentReference](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#parentreference) array
ParentRefs are the names of the Gateway resources this MCPRoute is being attached to.Cross namespace references are not supported. In other words, the Gateway resources must be in thesame namespace as the MCPRoute. Currently, each reference's Kind must be Gateway.
`path`optional
string
/mcp
Path is the HTTP endpoint path that serves MCP requests over the Streamable HTTP transport.If not specified, the default is `/mcp`.
`headers`optional
[HTTPHeaderMatch](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#httpheadermatch) array
Headers are HTTP headers that must match for this route to be selected.Multiple match values are ANDed together, meaning, a request must match all the specified headers to select the route.
`hostnames`optional
Hostname array
Hostnames is a list of hostnames matched against the HTTP Host header to select this MCPRoute.This is equivalent to the Hostnames field in the Gateway API HTTPRouteSpec. When specified, thegenerated HTTPRoute includes these hostnames, so the MCP route lands in the per-host virtual hostinstead of the wildcard vhost (the same scoping AIGatewayRoute.Hostnames provides).See https://gateway-api.sigs.k8s.io/reference/api-types/httproute/#hostnamesfor the details of the Hostnames field in the Gateway API.
`backendRefs`required
[MCPRouteBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcproutebackendref) array
BackendRefs is a list of backend references to the MCP servers.These MCP servers will be aggregated and exposed as a single MCP endpoint to the clients.From the client's perspective, they only need to configure a single MCP server URL, e.g. `https://api.example.com/mcp`,and the Envoy AI Gateway will route the requests to the appropriate MCP server based on the requests.All names must be unique within this list to avoid potential tools, resources, etc. name collisions.Also, cross-namespace references are not supported. In other words, the backend MCP servers must be in thesame namespace as the MCPRoute.
`securityPolicy`optional
[MCPRouteSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcproutesecuritypolicy)
SecurityPolicy defines the security policy for this MCPRoute.
`backendSelector`optional
BackendSelector restricts which of this route's backends a given request may fanout to, evaluated once per candidate backend when a client session is initialized.If unspecified, all backends on the route are considered.
####  MCPRouteStatus[​](https://theagentrouter.ai/docs/api/#mcproutestatus "Direct link to mcproutestatus")
**Appears in:**


MCPRouteStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-58 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
####  MCPToolFilter[​](https://theagentrouter.ai/docs/api/#mcptoolfilter "Direct link to mcptoolfilter")
**Appears in:**


MCPToolFilter filters tools using include and exclude patterns with exact matches or regular expressions. Exclude rules take precedence over include rules (deny-wins). When both include and exclude are specified, a tool must match an include rule AND not match any exclude rule to be allowed.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-59 "Direct link to Fields")
`include`optional
string array
Include is a list of tool names to include. Only the specified tools will be available.
`includeRegex`optional
string array
IncludeRegex is a list of RE2-compatible regular expressions that, when matched, include the tool.Only tools matching these patterns will be available.
`exclude`optional
string array
Exclude is a list of tool names to exclude. The specified tools will not be available.Exclude rules take precedence over include rules.
`excludeRegex`optional
string array
ExcludeRegex is a list of RE2-compatible regular expressions that, when matched, exclude the tool.Tools matching these patterns will not be available. Exclude rules take precedence over include rules.
####  PerModelQuota[​](https://theagentrouter.ai/docs/api/#permodelquota "Direct link to permodelquota")
**Appears in:**


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-60 "Direct link to Fields")
`modelName`required
string
Model name for which the quota is specified.This must match the ModelNameOverride set in the AIGatewayRoute's BackendReffor the quota policy to apply to that backend.
`quota`required
Expression for computing request cost and rules for matching requests to quota buckets.
####  ProtectedResourceMetadata[​](https://theagentrouter.ai/docs/api/#protectedresourcemetadata "Direct link to protectedresourcemetadata")
**Appears in:**


ProtectedResourceMetadata represents the Protected Resource Metadata of the MCP server as per RFC 9728.
References:
  * <https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization#authorization-server-location>
  * <https://datatracker.ietf.org/doc/html/rfc9728#name-protected-resource-metadata>


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-61 "Direct link to Fields")
`resource`required
string
Resource is the identifier of the protected resource.This should match the MCPRoute's URL. For example, if the MCPRoute's URL is`https://api.example.com/mcp`, the Resource should be `https://api.example.com/mcp`.
`resourceName`optional
string
ResourceName is a human-readable name for the protected resource.
`scopesSupported`optional
string array
ScopesSupported defines the minimal set of scopes required for the basic functionality of the MCPRoute.It should avoid broad or overly permissive scopes to prevent clients from requesting tokens with excessive privileges.If an operation requires additional scopes that are not present in the access token, the client will receive a403 Forbidden response that includes the required scopes in the `scope` field of the `WWW-Authenticate` header.This enables incremental privilege elevation through targeted `WWW-Authenticate: scope=`...`` challenges whenprivileged operations are first attempted.
`resourceSigningAlgValuesSupported`optional
string array
ResourceSigningAlgValuesSupported is a list of JWS signing algorithms supported by the resource server.These algorithms are used in the `alg` field of the JOSE header in signed tokens.
`resourceDocumentation`optional
string
ResourceDocumentation is a URL that provides human-readable documentation for the resource.
`resourcePolicyUri`optional
string
ResourcePolicyURI is a URL that points to the resource server's policy document.
####  QuotaBucketMode[​](https://theagentrouter.ai/docs/api/#quotabucketmode "Direct link to quotabucketmode")
**Underlying type:** string
**Appears in:**


QuotaBucketMode specifies whether the default and per request buckets values are exclusive or inclusive.
##### Possible Values[​](https://theagentrouter.ai/docs/api/#possible-values-3 "Direct link to Possible Values")
`Shared`
####  QuotaDefinition[​](https://theagentrouter.ai/docs/api/#quotadefinition "Direct link to quotadefinition")
**Appears in:**


QuotaDefinition specified expression for computing request cost and rules for matching requests to quota buckets.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-62 "Direct link to Fields")
`costExpression`optional
string
CostExpression specifies a CEL expression for computing the quota burndown of the LLM-related request.If no expression is specified the `total_tokens` value is used.For example: `input_tokens + cached_input_tokens + output_tokens`
`mode`optional
Shared
The `Mode` determines how quota is charged to the `DefaultBucket` and matching `BucketRules`.In the `shared` mode the quota is charged to all matching `BucketRules` AND the `DefaultBucket`and request is allowed only if the quota is available in at least one matching buckets.Defaults to `Shared`.
`defaultBucket`optional
Quota applicable to all traffic. This value can be overridden for specific classes of requestsusing the `BucketRules` configuration.
`bucketRules`optional
[QuotaRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-quotarule) array
BucketRules are a list of client selectors and quotas. If a requestmatches multiple rules, each of their associated quotas get applied, so asingle request might burn down the quota for multiple rules.Client selectors that match under the same model / service backend will becombined with the first limit taking precedence.
####  QuotaPolicySpec[​](https://theagentrouter.ai/docs/api/#quotapolicyspec "Direct link to quotapolicyspec")
**Appears in:**


QuotaPolicySpec specifies rules for computing token based costs of requests.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-63 "Direct link to Fields")
`targetRefs`optional
[LocalPolicyTargetReference](https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io/v1alpha2.LocalPolicyTargetReference) array
TargetRefs are the names of the AIServiceBackend resources this QuotaPolicy is being attached to.
`serviceQuota`optional
[ServiceQuotaDefinition](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-servicequotadefinition)
Quota for all models served by AIServiceBackend(s). This value can be overridden for specific models using the `PerModelQuotas`configuration.Currently, the rate limit configuration is properly set up, but the descriptor set is not being set in the Envoy Configuration
`perModelQuotas`optional
[PerModelQuota](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-permodelquota) array
PerModelQuotas specifies quota for different models served by the AIServiceBackend(s) where thispolicy is attached.When multiple QuotaPolicies define the same Model for the same AIServiceBackend,the policy whose namespace/name is alphabetically first takes precedence.Keys are sorted to ensure deterministic snapshot generation.
####  QuotaPolicyStatus[​](https://theagentrouter.ai/docs/api/#quotapolicystatus "Direct link to quotapolicystatus")
**Appears in:**


QuotaPolicyStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-64 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
**Appears in:**


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-65 "Direct link to Fields")
`clientSelectors`optional
RateLimitSelectCondition array
ClientSelectors holds the list of conditions to selectspecific clients using attributes from the traffic flow.All individual select conditions must hold True for this ruleand its limit to be applied.Currently, only header types are honored.If no client selectors are specified, the rule applies to all traffic ofthe targeted AIServiceBackend.
`quota`required
Quota value for given client selectors.This quota is applied for traffic flows when the selectorscompute to True, causing the request to be counted towards the limit.A response with 429 HTTP status code is sent back to the client whenthe selected requests have exceeded the quota.
`shadowMode`optional
boolean
ShadowMode indicates whether this quota rule runs in shadow mode.When enabled, all quota checks are performed (cache lookups,counter updates, telemetry generation), but the outcome is never enforced.The request always succeeds, even if the configured quota is exceeded.
**Appears in:**
  * [ServiceQuotaDefinition](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-servicequotadefinition)


QuotaValue defines the quota limits using sliding window.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-66 "Direct link to Fields")
`limit`required
integer
The limit alloted for a specified time window.
`duration`required
string
Time window. Must be exactly one of: `1s` (1 second), `1m` (1 minute), `1h` (1 hour), or `1d` (1 day).
####  ServiceQuotaDefinition[​](https://theagentrouter.ai/docs/api/#servicequotadefinition "Direct link to servicequotadefinition")
**Appears in:**


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-67 "Direct link to Fields")
`costExpression`optional
string
CostExpression specifies a CEL expression for computing the quota burndown of the LLM-related request.If no expression is specified the `total_tokens` value is used.For example: `input_tokens + cached_input_tokens + output_tokens`
`quota`required
Quota value applicable to all requests.A response with 429 HTTP status code is sent back to the client whenthe selected requests have exceeded the quota.
**Appears in:**
  * [MCPAuthorizationTarget](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1alpha1-mcpauthorizationtarget)


ToolCall represents a tool call in the MCP authorization target.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-68 "Direct link to Fields")
`backend`required
string
Backend is the name of the backend this tool belongs to.
`tool`required
string
Tool is the name of the tool.
####  VersionedAPISchema[​](https://theagentrouter.ai/docs/api/#versionedapischema "Direct link to versionedapischema")
**Appears in:**


VersionedAPISchema defines the API schema of either AIGatewayRoute (the input) or AIServiceBackend (the output).
This allows the ai-gateway to understand the input and perform the necessary transformation depending on the API schema pair (input, output).
Note that this is vendor specific, and the stability of the API schema is not guaranteed by the ai-gateway, but by the vendor via proper versioning.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-69 "Direct link to Fields")
`name`required
Name is the name of the API schema of the AIGatewayRoute or AIServiceBackend.
`version`optional
string
Version is the version of the API schema.When the name is set to AzureOpenAI, this version maps to `API Version` in theAzure OpenAI API documentation (https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#rest-api-versioning).This field is ignored for OpenAI, AWSBedrock, GCPVertexAI, and Anthropic.For OpenAI and Anthropic, use prefix to configure custom request paths.See https://aigateway.envoyproxy.io/docs/capabilities/llm-integrations/supported-providers for details.
`prefix`optional
string
Prefix is the prefix for the API.When the name is set to `OpenAI`, `chat completions` API endpoint will be `$\{this_field\}/chat/completions`.When the name is set to `Anthropic`, the `messages` API endpoint will be `$\{this_field\}/messages`.It can be with or without a leading slash (`/`).This field is ignored for AWSAnthropic and GCPAnthropic.This is especially useful when routing to a backend that has an OpenAI or Anthropic compatible API but has a differentprefix. For example, Gemini OpenAI compatible API (https://ai.google.dev/gemini-api/docs/openai) uses`/v1beta/openai` prefix. Another example is that Cohere AI (https://docs.cohere.com/v2/docs/compatibility-api)uses `/compatibility/v1` prefix. On the other hand, DeepSeek (https://api-docs.deepseek.com/) doesn'tuse prefix, so you can leave this field unset.See https://aigateway.envoyproxy.io/docs/capabilities/llm-integrations/supported-providers for details.
## aigateway.envoyproxy.io/v1beta1[​](https://theagentrouter.ai/docs/api/#aigatewayenvoyproxyiov1beta1 "Direct link to aigateway.envoyproxy.io/v1beta1")
Package v1beta1 contains API schema definitions for the aigateway.envoyproxy.io API group. This is the beta version of the API, preferred over v1alpha1 for new resources.
## Resource Kinds[​](https://theagentrouter.ai/docs/api/#resource-kinds-1 "Direct link to Resource Kinds")
### Available Kinds[​](https://theagentrouter.ai/docs/api/#available-kinds-1 "Direct link to Available Kinds")
  * [BackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicy)
  * [BackendSecurityPolicyList](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicylist)


### Kind Definitions[​](https://theagentrouter.ai/docs/api/#kind-definitions-1 "Direct link to Kind Definitions")
####  AIGatewayRoute[​](https://theagentrouter.ai/docs/api/#aigatewayroute-1 "Direct link to aigatewayroute-1")
**Appears in:**


AIGatewayRoute combines multiple AIServiceBackends and attaching them to Gateway(s) resources.
This serves as a way to define a "unified" AI API for a Gateway which allows downstream clients to use a single schema API to interact with multiple AI backends.
Envoy AI Gateway will generate the following k8s resources corresponding to the AIGatewayRoute:
  * HTTPRoute of the Gateway API as a top-level resource to bind all backends. The name of the HTTPRoute is the same as the AIGatewayRoute.
  * HTTPRouteFilter of the Envoy Gateway API per namespace for automatic hostname rewrite. The name of the HTTPRouteFilter is `ai-eg-host-rewrite-${AIGatewayRoute.Name}`.


All of these resources are created in the same namespace as the AIGatewayRoute. Note that this is the implementation detail subject to change. If you want to customize the default behavior of the Envoy AI Gateway, you can use these resources as a reference and create your own resources. Alternatively, you can use EnvoyPatchPolicy API of the Envoy Gateway to patch the generated resources. For example, you can configure the retry fallback behavior by attaching BackendTrafficPolicy API of Envoy Gateway to the generated HTTPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-70 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `AIGatewayRoute` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
Spec defines the details of the AIGatewayRoute.
`status`required
[AIGatewayRouteStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayroutestatus)
Status defines the status details of the AIGatewayRoute.
####  AIGatewayRouteList[​](https://theagentrouter.ai/docs/api/#aigatewayroutelist-1 "Direct link to aigatewayroutelist-1")
AIGatewayRouteList contains a list of AIGatewayRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-71 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `AIGatewayRouteList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[AIGatewayRoute](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayroute) array
####  AIServiceBackend[​](https://theagentrouter.ai/docs/api/#aiservicebackend-1 "Direct link to aiservicebackend-1")
**Appears in:**


AIServiceBackend is a resource that represents a single backend for AIGatewayRoute. A backend is a service that handles traffic with a concrete API specification.
A AIServiceBackend is "attached" to a Backend which is either a k8s Service or a Backend resource of the Envoy Gateway.
When a backend with an attached AIServiceBackend is used as a routing target in the AIGatewayRoute (more precisely, the HTTPRouteSpec defined in the AIGatewayRoute), the ai-gateway will generate the necessary configuration to do the backend specific logic in the final HTTPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-72 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `AIServiceBackend` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
[AIServiceBackendSpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aiservicebackendspec)
Spec defines the details of AIServiceBackend.
`status`required
[AIServiceBackendStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aiservicebackendstatus)
Status defines the status details of the AIServiceBackend.
####  AIServiceBackendList[​](https://theagentrouter.ai/docs/api/#aiservicebackendlist-1 "Direct link to aiservicebackendlist-1")
AIServiceBackendList contains a list of AIServiceBackends.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-73 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `AIServiceBackendList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[AIServiceBackend](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aiservicebackend) array
####  BackendSecurityPolicy[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicy-1 "Direct link to backendsecuritypolicy-1")
**Appears in:**
  * [BackendSecurityPolicyList](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicylist)


BackendSecurityPolicy specifies configuration for authentication and authorization rules on the traffic exiting the gateway to the backend.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-74 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `BackendSecurityPolicy` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
[BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)
`status`required
[BackendSecurityPolicyStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicystatus)
Status defines the status details of the BackendSecurityPolicy.
####  BackendSecurityPolicyList[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicylist-1 "Direct link to backendsecuritypolicylist-1")
BackendSecurityPolicyList contains a list of BackendSecurityPolicy
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-75 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `BackendSecurityPolicyList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[BackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicy) array
####  GatewayConfig[​](https://theagentrouter.ai/docs/api/#gatewayconfig-1 "Direct link to gatewayconfig-1")
**Appears in:**


GatewayConfig provides configuration for the AI Gateway external processor container that is deployed alongside the Gateway.
A GatewayConfig is referenced by a Gateway via the annotation "aigateway.envoyproxy.io/gateway-config". The GatewayConfig must be in the same namespace as the Gateway that references it.
This allows gateway-level configuration of the external processor, including environment variables (e.g., for tracing configuration) and resource requirements.
Multiple Gateways can reference the same GatewayConfig to share configuration.
Environment Variable Precedence: When merging environment variables, the following precedence applies (highest to lowest):
  1. GatewayConfig.Spec.ExtProc.Kubernetes.Env (this resource)
  2. Global controller flags (extProcExtraEnvVars)


If the same environment variable name exists in both sources, the GatewayConfig value takes precedence.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-76 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `GatewayConfig` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
Spec defines the configuration for the external processor.
`status`required
Status defines the status of the GatewayConfig.
####  GatewayConfigList[​](https://theagentrouter.ai/docs/api/#gatewayconfiglist-1 "Direct link to gatewayconfiglist-1")
GatewayConfigList contains a list of GatewayConfig.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-77 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `GatewayConfigList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[GatewayConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gatewayconfig) array
**Appears in:**


MCPRoute defines how to route MCP requests to the backend MCP servers.
This serves as a way to define a "unified" AI API for a Gateway which allows downstream clients to use a single schema API to interact with multiple MCP backends.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-78 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `MCPRoute` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`spec`required
Spec defines the details of the MCPRoute.
`status`required
Status defines the status details of the MCPRoute.
MCPRouteList contains a list of MCPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-79 "Direct link to Fields")
`apiVersion`required
String
We are on version `aigateway.envoyproxy.io/v1beta1` of the API.
`kind`required
String
This is a `MCPRouteList` resource
`metadata`required
Refer to Kubernetes API documentation for fields of `metadata`.
`items`required
[MCPRoute](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcproute) array
## Supporting Types[​](https://theagentrouter.ai/docs/api/#supporting-types-1 "Direct link to Supporting Types")
### Available Types[​](https://theagentrouter.ai/docs/api/#available-types-1 "Direct link to Available Types")
  * [AIGatewayRouteRuleBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayrouterulebackendref)
  * [AIGatewayRouteRuleMatch](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayrouterulematch)
  * [AIServiceBackendStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aiservicebackendstatus)
  * [AzureOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-azureoidcexchangetoken)
  * [BackendSecurityPolicyAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyapikey)
  * [BackendSecurityPolicyAWSCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyawscredentials)
  * [BackendSecurityPolicyAnthropicAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyanthropicapikey)
  * [BackendSecurityPolicyAzureAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyazureapikey)
  * [BackendSecurityPolicyAzureCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyazurecredentials)
  * [BackendSecurityPolicyCredentialOverride](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicycredentialoverride)
  * [BackendSecurityPolicyGCPCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicygcpcredentials)
  * [BackendSecurityPolicyOIDC](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyoidc)
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)
  * [BackendSecurityPolicyStatus](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicystatus)
  * [BackendSecurityPolicyType](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicytype)
  * [CredentialOverrideFromDynamicMetadata](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-credentialoverridefromdynamicmetadata)
  * [CredentialOverrideFromRequestHeaders](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-credentialoverridefromrequestheaders)
  * [GCPServiceAccountImpersonationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpserviceaccountimpersonationconfig)
  * [GCPWorkloadIdentityFederationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpworkloadidentityfederationconfig)
  * [GCPWorkloadIdentityProvider](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpworkloadidentityprovider)
  * [GatewayConfigForwardProxy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gatewayconfigforwardproxy)
  * [MCPAuthorizationSource](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpauthorizationsource)
  * [MCPAuthorizationTarget](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpauthorizationtarget)
  * [MCPBackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpbackendsecuritypolicy)
  * [MCPBackendSelectorRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpbackendselectorrule)
  * [MCPRouteAuthorization](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcprouteauthorization)
  * [MCPRouteAuthorizationRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcprouteauthorizationrule)
  * [MCPRouteSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcproutesecuritypolicy)
  * [ProtectedResourceMetadata](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-protectedresourcemetadata)


### Type Definitions[​](https://theagentrouter.ai/docs/api/#type-definitions-1 "Direct link to Type Definitions")
####  AIGatewayRouteRule[​](https://theagentrouter.ai/docs/api/#aigatewayrouterule-1 "Direct link to aigatewayrouterule-1")
**Appears in:**


AIGatewayRouteRule is a rule that defines the routing behavior of the AIGatewayRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-80 "Direct link to Fields")
`name`optional
Name is the name of the route rule. This name must be unique within the route.When specified, it is copied to the generated HTTPRoute rule name.
`backendRefs`optional
[AIGatewayRouteRuleBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayrouterulebackendref) array
BackendRefs is the list of backends that this rule will route the traffic to.Each backend can have a weight that determines the traffic distribution.The namespace of each backend defaults to the same namespace as the AIGatewayRoute when not specified.Cross-namespace references are supported by specifying the namespace field.When a namespace different than the AIGatewayRoute's namespace is specified,a ReferenceGrant object is required in the referent namespace to allow thatnamespace's owner to accept the reference.BackendRefs can reference either AIServiceBackend resources (default) or InferencePool resourcesfrom the Gateway API Inference Extension. When referencing InferencePool resources:- Only one InferencePool backend is allowed per rule- Cannot mix InferencePool with AIServiceBackend references in the same rule- Fallback behavior is handled by the InferencePool's endpoint pickerFor AIServiceBackend references, you can achieve fallback behavior by configuring multiple backendscombined with the BackendTrafficPolicy of Envoy Gateway.Please refer to https://gateway.envoyproxy.io/docs/tasks/traffic/failover/ as well ashttps://gateway.envoyproxy.io/docs/tasks/traffic/retry/.
`matches`optional
[AIGatewayRouteRuleMatch](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayrouterulematch) array
Matches is the list of AIGatewayRouteMatch that this rule will match the traffic to.This is a subset of the HTTPRouteMatch in the Gateway API. See for the details:https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.HTTPRouteMatch
`timeouts`optional
Timeouts defines the timeouts that can be configured for an HTTP request.If this field is not set, or the timeout.requestTimeout is nil, Envoy AI Gateway defaults toset 60s for the request timeout as opposed to 15s of the Envoy Gateway's default value.For streaming responses (like chat completions with stream=true), consider settinglonger timeouts as the response may take time until the completion. Timeouts.Requestacts as the maximum total time the gateway will wait for the entire response,including all streamed chunks.
`streamIdleTimeout`optional
StreamIdleTimeout is the maximum time Envoy will wait without receiving any bytes from the upstream.If the timer fires before the first response byte arrives, Envoy resets the upstream stream and aretry policy can fall over to the next backend. If it fires mid-stream afterbytes have already arrived, the stream is cut and the client receives a 504.The AI Gateway extension server sets route.retry_policy.per_try_idle_timeout to this value onevery xDS route generated from this rule before it is sent to the data plane.Pair this field with Timeouts.Request, which acts as the overall deadline.If this field is not set, no per-try idle timeout is applied.
`modelsOwnedBy`optional
string
Envoy AI Gateway
ModelsOwnedBy represents the owner of the running models serving by the backends,which will be exported as the field of `OwnedBy` in openai-compatible API `/models`.This is used only when this rule contains `x-ai-eg-model` in its header matchingwhere the header value will be recognized as a `model` in `/models` endpoint.All the matched models will share the same owner.Default to `Envoy AI Gateway` if not set.
`modelsCreatedAt`optional
ModelsCreatedAt represents the creation timestamp of the running models serving by the backends,which will be exported as the field of `Created` in openai-compatible API `/models`.It follows the format of RFC 3339, for example `2024-05-21T10:00:00Z`.This is used only when this rule contains `x-ai-eg-model` in its header matchingwhere the header value will be recognized as a `model` in `/models` endpoint.All the matched models will share the same creation time.Default to the creation timestamp of the AIGatewayRoute if not set.
####  AIGatewayRouteRuleBackendRef[​](https://theagentrouter.ai/docs/api/#aigatewayrouterulebackendref-1 "Direct link to aigatewayrouterulebackendref-1")
**Appears in:**


AIGatewayRouteRuleBackendRef is a reference to a backend with a weight. It can reference either an AIServiceBackend or an InferencePool resource.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-81 "Direct link to Fields")
`name`required
string
Name is the name of the backend resource.When Group and Kind are not specified, this refers to an AIServiceBackend.When Group and Kind are specified, this refers to the resource of the specified type.
`namespace`optional
Namespace is the namespace of the backend resource.When unspecified (or empty string), this refers to the local namespace of the AIGatewayRoute.Note that when a namespace different than the local namespace is specified,a ReferenceGrant object is required in the referent namespace to allow thatnamespace's owner to accept the reference. See the ReferenceGrantdocumentation for details.
`group`optional
string
Group is the group of the backend resource.When not specified, defaults to aigateway.envoyproxy.io (AIServiceBackend).Currently, only `inference.networking.k8s.io` is supported for InferencePool resources.
`kind`optional
string
Kind is the kind of the backend resource.When not specified, defaults to AIServiceBackend.Currently, only `InferencePool` is supported when Group is specified.
`modelNameOverride`optional
string
Name of the model in the backend. If provided this will override the name provided in the request.This field is ignored when referencing InferencePool resources.
`headerMutation`optional
HeaderMutation defines the request header mutation to be applied to this backend.When both route-level and backend-level HeaderMutation are defined,route-level takes precedence over backend-level for conflicting operations.This field is ignored when referencing InferencePool resources.
`bodyMutation`optional
BodyMutation defines the request body mutation to be applied to this backend.This allows modification of JSON fields in the request body before sending to the backend.When both route-level and backend-level BodyMutation are defined,route-level takes precedence over backend-level for conflicting operations.This field is ignored when referencing InferencePool resources.
`weight`optional
integer
1
Weight is the weight of the backend. This is exactly the same as the weight inthe BackendRef in the Gateway API. See for the details:https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.BackendRefDefault is 1.
`priority`optional
integer
0
Priority is the priority of the backend. This sets the priority on the underlying endpoints.See: https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/load_balancing/priorityNote: This will override the `faillback` property of the underlying Envoy Gateway BackendThis field is ignored when referencing InferencePool resources.Default is 0.
####  AIGatewayRouteRuleMatch[​](https://theagentrouter.ai/docs/api/#aigatewayrouterulematch-1 "Direct link to aigatewayrouterulematch-1")
**Appears in:**


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-82 "Direct link to Fields")
`headers`optional
[HTTPHeaderMatch](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#httpheadermatch) array
Headers specifies HTTP request header matchers. See HeaderMatch in the Gateway API for the details:https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.HTTPHeaderMatch
####  AIGatewayRouteSpec[​](https://theagentrouter.ai/docs/api/#aigatewayroutespec-1 "Direct link to aigatewayroutespec-1")
**Appears in:**


AIGatewayRouteSpec details the AIGatewayRoute configuration.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-83 "Direct link to Fields")
`parentRefs`optional
[ParentReference](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#parentreference) array
ParentRefs are the names of the Gateway resources this AIGatewayRoute is being attached to.Currently, each reference's Kind must be Gateway.
`hostnames`optional
Hostname array
Hostnames is a list of hostnames matched against the HTTP Host header to select an AIGatewayRouteused to process the request. This is equivalent to the Hostnames field in the Gateway API HTTPRouteSpec.When specified, the generated HTTPRoute will include these hostnames for hostname-based filtering.See https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.HTTPRouteSpecfor the details of the Hostnames field in the Gateway API.
`rules`required
[AIGatewayRouteRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayrouterule) array
Rules is the list of AIGatewayRouteRule that this AIGatewayRoute will match the traffic to.Each rule is a subset of the HTTPRoute in the Gateway API (https://gateway-api.sigs.k8s.io/api-types/httproute/).AI Gateway controller will generate a HTTPRoute based on the configuration given here with the additionalmodifications to achieve the necessary jobs, notably inserting the AI Gateway filter responsible forthe transformation of the request and response, etc.In the matching conditions in the AIGatewayRouteRule, `x-ai-eg-model` header is availableif we want to describe the routing behavior based on the model name. The model name is extractedfrom the request content before the routing decision.How multiple rules are matched is the same as the Gateway API. See for the details:https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io%2fv1.HTTPRouteAt most 15 rules are allowed per AIGatewayRoute, corresponding to the Gateway API's limit onHTTPRoute.spec.rules (one slot is reserved for a controller-injected catch-all rule). Toconfigure more rules on the same Gateway, split them across multiple AIGatewayRoute resources.
`llmRequestCosts`optional
[LLMRequestCost](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-llmrequestcost) array
LLMRequestCosts specifies how to capture the cost of the LLM-related request, notably the token usage.The AI Gateway filter will capture each specified number and store it in the Envoy's dynamicmetadata per HTTP request. The namespaced key is `io.envoy.ai_gateway`.These route-level costs override any global defaults defined in GatewayConfig.Spec.GlobalLLMRequestCostsfor the same metadataKey. If a metadataKey is not defined in either place, no cost is calculated for it.This allows you to define common cost formulas once at the gateway level (e.g., via GatewayConfig)and only override them in specific routes when needed (e.g., premium routes with different pricing).For example, let's say we have the following LLMRequestCosts configuration:
```
	llmRequestCosts:	- metadataKey: llm_input_token	  type: InputToken	- metadataKey: llm_output_token	  type: OutputToken	- metadataKey: llm_total_token	  type: TotalToken	- metadataKey: llm_cached_input_token	  type: CachedInputToken- metadataKey: llm_cache_creation_input_token   type: CacheCreationInputToken
```
Then, with the following BackendTrafficPolicy of Envoy Gateway, you can have threerate limit buckets for each unique x-tenant-id header value. One bucket is for the input token,the other is for the output token, and the last one is for the total token.Each bucket will be reduced by the corresponding token usage captured by the AI Gateway filter.
```
	apiVersion: gateway.envoyproxy.io/v1alpha1	kind: BackendTrafficPolicy	metadata:	  name: some-example-token-rate-limit	  namespace: default	spec:	  targetRefs:	  - group: gateway.networking.k8s.io	     kind: HTTPRoute	     name: usage-rate-limit	  rateLimit:	    type: Global	    global:	      rules:	        - clientSelectors:	            # Do the rate limiting based on the x-tenant-id header.	            - headers:	                - name: x-tenant-id	                  type: Distinct	          limit:	            # Configures the number of tokens allowed per hour.	            requests: 10000	            unit: Hour	          cost:	            request:	              from: Number	              # Setting the request cost to zero allows to only check the rate limit budget,	              # and not consume the budget on the request path.	              number: 0	            # This specifies the cost of the response retrieved from the dynamic metadata set by the AI Gateway filter.	            # The extracted value will be used to consume the rate limit budget, and subsequent requests will be rate limited	            # if the budget is exhausted.	            response:	              from: Metadata	              metadata:	                namespace: io.envoy.ai_gateway	                key: llm_input_token	        - clientSelectors:	            - headers:	                - name: x-tenant-id	                  type: Distinct	          limit:	            requests: 10000	            unit: Hour	          cost:	            request:	              from: Number	              number: 0	            response:	              from: Metadata	              metadata:	                namespace: io.envoy.ai_gateway	                key: llm_output_token	        - clientSelectors:	            - headers:	                - name: x-tenant-id	                  type: Distinct	          limit:	            requests: 10000	            unit: Hour	          cost:	            request:	              from: Number	              number: 0	            response:	              from: Metadata	              metadata:	                namespace: io.envoy.ai_gateway	                key: llm_total_token
```
Note that when multiple AIGatewayRoute resources are attached to the same Gateway, anddifferent costs are configured for the same metadata key, each route's rule is carried inthe filter configuration with the route identity; the data plane selects the matching ruleper request (by route), so each route can define its own cost for the same metadata key.
####  AIGatewayRouteStatus[​](https://theagentrouter.ai/docs/api/#aigatewayroutestatus-1 "Direct link to aigatewayroutestatus-1")
**Appears in:**


AIGatewayRouteStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-84 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
####  AIServiceBackendSpec[​](https://theagentrouter.ai/docs/api/#aiservicebackendspec-1 "Direct link to aiservicebackendspec-1")
**Appears in:**


AIServiceBackendSpec details the AIServiceBackend configuration.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-85 "Direct link to Fields")
`schema`required
APISchema specifies the API schema of the output format of requests fromEnvoy that this AIServiceBackend can accept as incoming requests.Based on this schema, the ai-gateway will perform the necessary transformation forthe pair of AIGatewayRouteSpec.APISchema and AIServiceBackendSpec.APISchema.This is required to be set.
`backendRef`required
BackendRef is the reference to the Backend resource that this AIServiceBackend corresponds to.A backend must be a Backend resource of Envoy Gateway. Note that k8s Service will be supportedas a backend in the future. See https://github.com/envoyproxy/ai-gateway/issues/902 for more details.This is required to be set.
`headerMutation`optional
HeaderMutation defines the mutation of HTTP headers that will be applied to the requestbefore sending it to the backend.
`bodyMutation`optional
BodyMutation defines the mutation of HTTP request body JSON fields that will be applied to the requestbefore sending it to the backend.
####  AIServiceBackendStatus[​](https://theagentrouter.ai/docs/api/#aiservicebackendstatus-1 "Direct link to aiservicebackendstatus-1")
**Appears in:**


AIServiceBackendStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-86 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
**Underlying type:** string
**Appears in:**


APISchema defines the API schema.
##### Possible Values[​](https://theagentrouter.ai/docs/api/#possible-values-4 "Direct link to Possible Values")
`OpenAI`
APISchemaOpenAI is the OpenAI schema.https://github.com/openai/openai-openapi 
`Cohere`
APISchemaCohere is the Cohere schema.https://docs.cohere.com/v2 
`AWSBedrock`
APISchemaAWSBedrock is the AWS Bedrock schema.https://docs.aws.amazon.com/bedrock/latest/APIReference/API_Operations_Amazon_Bedrock_Runtime.html 
`AzureOpenAI`
APISchemaAzureOpenAI APISchemaAzure is the Azure OpenAI schema.https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#api-specs 
`GCPVertexAI`
APISchemaGCPVertexAI is the schema followed by Gemini models hosted on GCP's Vertex AI platform.Note: Using this schema requires a BackendSecurityPolicy to be configured and attached,as the transformation will use the gcp-region and project-name from the BackendSecurityPolicy.https://cloud.google.com/vertex-ai/docs/reference/rest/v1/projects.locations.endpoints/generateContent?hl=en 
`GCPAnthropic`
APISchemaGCPAnthropic is the schema for Anthropic models hosted on GCP's Vertex AI platform.Returns native Anthropic format responses for seamless integration.https://docs.anthropic.com/en/api/claude-on-vertex-ai 
`Anthropic`
APISchemaAnthropic is the native Anthropic API schema.https://docs.claude.com/en/home 
`AWSAnthropic`
APISchemaAWSAnthropic is the schema for Anthropic models hosted on AWS Bedrock.Uses the native Anthropic Messages API format for requests and responses.When used with /v1/chat/completions endpoint, translates OpenAI format to Anthropic.When used with /v1/messages endpoint, passes through native Anthropic format.https://aws.amazon.com/bedrock/anthropic/https://docs.claude.com/en/api/claude-on-amazon-bedrock 
####  AWSCredentialsFile[​](https://theagentrouter.ai/docs/api/#awscredentialsfile-1 "Direct link to awscredentialsfile-1")
**Appears in:**
  * [BackendSecurityPolicyAWSCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyawscredentials)


AWSCredentialsFile specifies the credentials file to use for the AWS provider. Envoy reads the secret file, and the profile to use is specified by the Profile field.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-87 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the credential file.The secret should contain the AWS credentials file keyed on `credentials`.
`profile`required
string
default
Profile is the profile to use in the credentials file.
####  AWSOIDCExchangeToken[​](https://theagentrouter.ai/docs/api/#awsoidcexchangetoken-1 "Direct link to awsoidcexchangetoken-1")
**Appears in:**
  * [BackendSecurityPolicyAWSCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyawscredentials)


AWSOIDCExchangeToken specifies credentials to obtain oidc token from a sso server. For AWS, the controller will query STS to obtain AWS AccessKeyId, SecretAccessKey, and SessionToken, and store them in a temporary credentials file.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-88 "Direct link to Fields")
`oidc`required
OIDC is used to obtain oidc tokens via an SSO server which will be used to exchange for provider credentials.
`grantType`optional
string
GrantType is the method application gets access token.
`aud`optional
string
Aud defines the audience that this ID Token is intended for.
`awsRoleArn`required
string
AwsRoleArn is the AWS IAM Role with the permission to use specific resources in AWS accountwhich maps to the temporary AWS security credentials exchanged using the authentication token issued by OIDC provider.
####  AzureOIDCExchangeToken[​](https://theagentrouter.ai/docs/api/#azureoidcexchangetoken-1 "Direct link to azureoidcexchangetoken-1")
**Appears in:**
  * [BackendSecurityPolicyAzureCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyazurecredentials)


AzureOIDCExchangeToken specifies credentials to obtain oidc token from a sso server. For Azure, the controller will query Azure Entra ID to get an Azure Access Token, and store them in a secret.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-89 "Direct link to Fields")
`oidc`required
OIDC is used to obtain oidc tokens via an SSO server which will be used to exchange for provider credentials.
`grantType`optional
string
GrantType is the method application gets access token.
`aud`optional
string
Aud defines the audience that this ID Token is intended for.
####  BackendSecurityPolicyAPIKey[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyapikey-1 "Direct link to backendsecuritypolicyapikey-1")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)


BackendSecurityPolicyAPIKey specifies the API key.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-90 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the secret containing the API key.ai-gateway must be given the permission to read this secret.The key of the secret should be `apiKey`.
####  BackendSecurityPolicyAWSCredentials[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyawscredentials-1 "Direct link to backendsecuritypolicyawscredentials-1")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)


BackendSecurityPolicyAWSCredentials contains the supported authentication mechanisms to access AWS.
When neither CredentialsFile nor OIDCExchangeToken is specified, the AWS SDK's default credential chain will be used. This automatically supports various authentication methods in the following order:
  1. Environment variables (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN)
  2. EKS Pod Identity - automatically rotates credentials for pods in EKS clusters
  3. IAM Roles for Service Accounts (IRSA) - injects credentials via mounted service account tokens
  4. EC2 instance metadata (IAM instance roles)
  5. ECS task roles


The default credential chain is recommended for Kubernetes deployments as it supports automatic credential rotation without manual configuration. Credentials are refreshed automatically when they approach expiration (typically hourly for IRSA and Pod Identity).
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-91 "Direct link to Fields")
`region`required
string
Region specifies the AWS region associated with the policy.
`credentialsFile`optional
CredentialsFile specifies the credentials file to use for the AWS provider.When specified, this takes precedence over the default credential chain.
`oidcExchangeToken`optional
[AWSOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-awsoidcexchangetoken)
OIDCExchangeToken specifies the oidc configurations used to obtain an oidc token. The oidc token will beused to obtain temporary credentials to access AWS.When specified, this takes precedence over the default credential chain.
####  BackendSecurityPolicyAnthropicAPIKey[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyanthropicapikey-1 "Direct link to backendsecuritypolicyanthropicapikey-1")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)


BackendSecurityPolicyAnthropicAPIKey specifies the Anthropic API key.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-92 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the secret containing the Anthropic API key.ai-gateway must be given the permission to read this secret.The key of the secret should be `apiKey`.
####  BackendSecurityPolicyAzureAPIKey[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyazureapikey-1 "Direct link to backendsecuritypolicyazureapikey-1")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)


BackendSecurityPolicyAzureAPIKey specifies the Azure OpenAI API key.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-93 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the secret containing the Azure API key.ai-gateway must be given the permission to read this secret.The key of the secret should be `apiKey`.
####  BackendSecurityPolicyAzureCredentials[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyazurecredentials-1 "Direct link to backendsecuritypolicyazurecredentials-1")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)


BackendSecurityPolicyAzureCredentials contains the supported authentication mechanisms to access Azure. Only one of ClientSecretRef or OIDCExchangeToken must be specified. Credentials will not be generated if neither are set.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-94 "Direct link to Fields")
`clientID`required
string
ClientID is a unique identifier for an application in Azure.
`tenantID`required
string
TenantId is a unique identifier for an Azure Active Directory instance.
`clientSecretRef`optional
ClientSecretRef is the reference to the secret containing the Azure client secret.ai-gateway must be given the permission to read this secret.The key of secret should be `client-secret`.
`oidcExchangeToken`optional
[AzureOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-azureoidcexchangetoken)
OIDCExchangeToken specifies the oidc configurations used to obtain an oidc token. The oidc token will beused to obtain temporary credentials to access Azure.
####  BackendSecurityPolicyCredentialOverride[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicycredentialoverride "Direct link to backendsecuritypolicycredentialoverride")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)


BackendSecurityPolicyCredentialOverride configures per-request credential sourcing. A trusted upstream filter (ext_authz or a preceding ext_proc) resolves the caller's credential and delivers it via one of the two sources below. Exactly one source must be configured.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-95 "Direct link to Fields")
`fromRequestHeaders`optional
[CredentialOverrideFromRequestHeaders](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-credentialoverridefromrequestheaders)
FromRequestHeaders sources the credential from a request header injected by a trustedingress filter. The gateway strips the header before forwarding to the upstream backend.Because any client can set request headers, this source is only safe when a trusted filterremoves any client-supplied copy and writes the canonical value.
`fromDynamicMetadata`optional
[CredentialOverrideFromDynamicMetadata](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-credentialoverridefromdynamicmetadata)
FromDynamicMetadata sources the credential from Envoy dynamic metadata set by a trustedfilter (e.g. ext_authz). Metadata cannot be forged by the client and is the preferred source.
####  BackendSecurityPolicyGCPCredentials[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicygcpcredentials-1 "Direct link to backendsecuritypolicygcpcredentials-1")
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)


BackendSecurityPolicyGCPCredentials contains the supported authentication mechanisms to access GCP.
When neither CredentialsFile nor WorkloadIdentityFederationConfig is specified, Application Default Credentials (ADC) will be used. This supports GKE Workload Identity, environment variables, and default service account credentials when running on GCP.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-96 "Direct link to Fields")
`projectName`required
string
ProjectName is the GCP project name.
`region`required
string
Region is the GCP region associated with the policy.
`credentialsFile`optional
CredentialsFile specifies the service account credentials file to use for the GCP provider.
`workloadIdentityFederationConfig`optional
[GCPWorkloadIdentityFederationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpworkloadidentityfederationconfig)
WorkloadIdentityFederationConfig is the configuration for the GCP Workload Identity Federation.
####  BackendSecurityPolicyOIDC[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyoidc-1 "Direct link to backendsecuritypolicyoidc-1")
**Appears in:**
  * [AzureOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-azureoidcexchangetoken)
  * [GCPWorkloadIdentityProvider](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpworkloadidentityprovider)


BackendSecurityPolicyOIDC specifies OIDC related fields.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-97 "Direct link to Fields")
`oidc`required
OIDC is used to obtain oidc tokens via an SSO server which will be used to exchange for provider credentials.
`grantType`optional
string
GrantType is the method application gets access token.
`aud`optional
string
Aud defines the audience that this ID Token is intended for.
####  BackendSecurityPolicySpec[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicyspec-1 "Direct link to backendsecuritypolicyspec-1")
**Appears in:**
  * [BackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicy)


BackendSecurityPolicySpec specifies authentication rules on access the provider from the Gateway. Only one mechanism to access a backend(s) can be specified.
Only one type of BackendSecurityPolicy can be defined.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-98 "Direct link to Fields")
`targetRefs`optional
[LocalPolicyTargetReference](https://gateway-api.sigs.k8s.io/reference/spec/#gateway.networking.k8s.io/v1alpha2.LocalPolicyTargetReference) array
TargetRefs are the names of the AIServiceBackend or InferencePool resources this BackendSecurityPolicy is being attached to.Attaching multiple BackendSecurityPolicies to the same resource is invalid and will result in an errorduring the reconciliation of the resource.
`type`required
[BackendSecurityPolicyType](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicytype)
Type specifies the type of the backend security policy.
`apiKey`optional
[BackendSecurityPolicyAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyapikey)
APIKey is a mechanism to access a backend(s). The API key will be injected into the Authorization header.
`awsCredentials`optional
[BackendSecurityPolicyAWSCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyawscredentials)
AWSCredentials is a mechanism to access a backend(s). AWS specific logic will be applied.
`azureAPIKey`optional
[BackendSecurityPolicyAzureAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyazureapikey)
AzureAPIKey is a mechanism to access Azure OpenAI backend(s). The API key will be injected into the api-key header.
`azureCredentials`optional
[BackendSecurityPolicyAzureCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyazurecredentials)
AzureCredentials is a mechanism to access a backend(s). Azure OpenAI specific logic will be applied.
`gcpCredentials`optional
[BackendSecurityPolicyGCPCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicygcpcredentials)
GCPCredentials is a mechanism to access a backend(s). GCP specific logic will be applied.
`anthropicAPIKey`optional
[BackendSecurityPolicyAnthropicAPIKey](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyanthropicapikey)
AnthropicAPIKey is a mechanism to access Anthropic backend(s). The API key will be injected into the `x-api-key` header.https://docs.claude.com/en/api/overview#authentication
`credentialOverride`optional
[BackendSecurityPolicyCredentialOverride](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicycredentialoverride)
CredentialOverride, when set, sources the upstream credential per-request instead of usingthe static credential configured above.For AWSCredentials the credential is the three-part SigV4 input rather than a single string;see the source types for how each carries it.
####  BackendSecurityPolicyStatus[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicystatus-1 "Direct link to backendsecuritypolicystatus-1")
**Appears in:**
  * [BackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicy)


BackendSecurityPolicyStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-99 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
####  BackendSecurityPolicyType[​](https://theagentrouter.ai/docs/api/#backendsecuritypolicytype-1 "Direct link to backendsecuritypolicytype-1")
**Underlying type:** string
**Appears in:**
  * [BackendSecurityPolicySpec](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicyspec)


BackendSecurityPolicyType specifies the type of auth mechanism used to access a backend.
##### Possible Values[​](https://theagentrouter.ai/docs/api/#possible-values-5 "Direct link to Possible Values")
`APIKey`
`AWSCredentials`
`AzureAPIKey`
`AnthropicAPIKey`
`AzureCredentials`
`GCPCredentials`
####  CredentialOverrideFromDynamicMetadata[​](https://theagentrouter.ai/docs/api/#credentialoverridefromdynamicmetadata "Direct link to credentialoverridefromdynamicmetadata")
**Appears in:**
  * [BackendSecurityPolicyCredentialOverride](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicycredentialoverride)


CredentialOverrideFromDynamicMetadata sources the per-request credential from Envoy dynamic metadata produced by a trusted filter running earlier in the filter chain.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-100 "Direct link to Fields")
`namespace`required
string
Namespace is the Envoy metadata namespace written by the trusted filter.Example: `envoy.filters.http.ext_authz`
`key`optional
string
Key is the metadata key within the namespace. Defaults to the x-aigw-* name for the auth type.For every type except AWSCredentials the value must be a string holding the credential.For AWSCredentials it must be a struct, since SigV4 needs three inputs: accessKeyId secretAccessKey sessionToken (optional; omit for long-lived credentials)Field names are fixed. accessKeyId without secretAccessKey, or the reverse, fails therequest instead of falling back. Defaults to `x-aigw-aws-credentials`.
`fallbackToConfigured`optional
boolean
true
FallbackToConfigured controls behaviour when the metadata key is absent.true (default) falls back to the static credential configured in the policy.false returns a 401 to the client without forwarding the request.
####  CredentialOverrideFromRequestHeaders[​](https://theagentrouter.ai/docs/api/#credentialoverridefromrequestheaders "Direct link to credentialoverridefromrequestheaders")
**Appears in:**
  * [BackendSecurityPolicyCredentialOverride](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicycredentialoverride)


CredentialOverrideFromRequestHeaders sources the per-request credential from a request header.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-101 "Direct link to Fields")
`header`optional
string
Header is the name of the request header that carries the credential.Defaults to the x-aigw-* header for the configured auth type: APIKey → x-aigw-api-key AnthropicAPIKey → x-aigw-anthropic-api-key AzureAPIKey → x-aigw-azure-api-key AzureCredentials → x-aigw-azure-access-token GCPCredentials → x-aigw-gcp-access-tokenFor AWSCredentials this is a prefix, not a single header name, since SigV4 needs threeinputs. Defaults to `x-aigw-aws-`, yielding: x-aigw-aws-access-key-id x-aigw-aws-secret-access-key x-aigw-aws-session-token (optional; omit for long-lived credentials)All three are stripped before the backend sees them. An access key ID without a secretaccess key, or the reverse, fails the request instead of falling back.Prefer FromDynamicMetadata for AWSCredentials. Headers are client-settable, and a secretaccess key on the wire is a worse leak than an API key.
`fallbackToConfigured`optional
boolean
true
FallbackToConfigured controls behaviour when the header is absent.true (default) falls back to the static credential configured in the policy.false returns a 401 to the client without forwarding the request.
####  GCPCredentialsFile[​](https://theagentrouter.ai/docs/api/#gcpcredentialsfile-1 "Direct link to gcpcredentialsfile-1")
**Appears in:**
  * [BackendSecurityPolicyGCPCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicygcpcredentials)


GCPCredentialsFile specifies the service account key json file to authenticate with GCP provider.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-102 "Direct link to Fields")
`secretRef`required
SecretRef is the reference to the credential file.The secret should contain the GCP service account credentials file keyed on `service_account.json`.
####  GCPOIDCExchangeToken[​](https://theagentrouter.ai/docs/api/#gcpoidcexchangetoken-1 "Direct link to gcpoidcexchangetoken-1")
**Appears in:**
  * [GCPWorkloadIdentityFederationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpworkloadidentityfederationconfig)


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-103 "Direct link to Fields")
`oidc`required
OIDC is used to obtain oidc tokens via an SSO server which will be used to exchange for provider credentials.
`grantType`optional
string
GrantType is the method application gets access token.
`aud`optional
string
Aud defines the audience that this ID Token is intended for.
####  GCPServiceAccountImpersonationConfig[​](https://theagentrouter.ai/docs/api/#gcpserviceaccountimpersonationconfig-1 "Direct link to gcpserviceaccountimpersonationconfig-1")
**Appears in:**
  * [GCPWorkloadIdentityFederationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpworkloadidentityfederationconfig)


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-104 "Direct link to Fields")
`serviceAccountName`required
string
ServiceAccountName is the name of the service account to impersonate.
####  GCPWorkloadIdentityFederationConfig[​](https://theagentrouter.ai/docs/api/#gcpworkloadidentityfederationconfig-1 "Direct link to gcpworkloadidentityfederationconfig-1")
**Appears in:**
  * [BackendSecurityPolicyGCPCredentials](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-backendsecuritypolicygcpcredentials)


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-105 "Direct link to Fields")
`projectID`required
string
ProjectID is the GCP project ID.
`workloadIdentityProviderName`required
string
WorkloadIdentityProviderName is the name of the external identity provider as registered on Google Cloud Platform.
`oidcExchangeToken`required
[GCPOIDCExchangeToken](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpoidcexchangetoken)
OIDCExchangeToken specifies the oidc configurations used to obtain an oidc token. The oidc token will beused to obtain temporary credentials to access GCP.
`workloadIdentityPoolName`required
string
WorkloadIdentityPoolName is the name of the workload identity pool defined in GCP.https://cloud.google.com/iam/docs/workload-identity-federation?hl=en
`serviceAccountImpersonation`optional
[GCPServiceAccountImpersonationConfig](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gcpserviceaccountimpersonationconfig)
ServiceAccountImpersonation is the service account impersonation configuration.This is used to impersonate a service account when getting access token.
####  GatewayConfigExtProc[​](https://theagentrouter.ai/docs/api/#gatewayconfigextproc-1 "Direct link to gatewayconfigextproc-1")
**Appears in:**


GatewayConfigExtProc holds runtime-specific configuration for the external processor.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-106 "Direct link to Fields")
`kubernetes`optional
[KubernetesContainerSpec](https://gateway.envoyproxy.io/docs/api/extension_types/#kubernetescontainerspec)
Kubernetes defines the configuration for running the external processor as a Kubernetes container.
####  GatewayConfigForwardProxy[​](https://theagentrouter.ai/docs/api/#gatewayconfigforwardproxy-1 "Direct link to gatewayconfigforwardproxy-1")
**Appears in:**


GatewayConfigForwardProxy configures an HTTP CONNECT forward proxy for upstream egress.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-107 "Direct link to Fields")
`address`required
string
Address is the `host:port` of the HTTP CONNECT proxy. The host may be a hostname or anIP address; the port is required. Upstream connections are tunnelled through this proxyvia Envoy's http_11_proxy transport socket, preserving the upstream TLS session.
####  GatewayConfigSpec[​](https://theagentrouter.ai/docs/api/#gatewayconfigspec-1 "Direct link to gatewayconfigspec-1")
**Appears in:**


GatewayConfigSpec defines the configuration for the AI Gateway.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-108 "Direct link to Fields")
`extProc`optional
[GatewayConfigExtProc](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gatewayconfigextproc)
ExtProc defines the configuration for the external processor container.
`globalLLMRequestCosts`optional
[LLMRequestCost](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-llmrequestcost) array
GlobalLLMRequestCosts defines default LLM request costs that apply to allroutes referencing this GatewayConfig. These costs can be overridden on aper-route basis via AIGatewayRoute.Spec.LLMRequestCosts.When a request matches a route, the cost calculation proceeds as follows: 1. If the route defines LLMRequestCosts with a matching metadataKey, use that. 2. Otherwise, fall back to the global cost with that metadataKey (if defined here). 3. If neither exists, the cost is not calculated for that metadataKey.This allows you to define common cost formulas once at the gateway level(e.g., billing_charges = input_tokens + output_tokens) and only overridethem in specific routes when needed (e.g., premium routes with different pricing).
`forwardProxy`optional
[GatewayConfigForwardProxy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-gatewayconfigforwardproxy)
ForwardProxy routes all upstream AI/LLM traffic from Gateways referencing thisGatewayConfig through an HTTP CONNECT forward proxy. This is intended for data planesin locked-down networks where direct egress to providers is not permitted and alloutbound traffic must traverse a proxy.
####  GatewayConfigStatus[​](https://theagentrouter.ai/docs/api/#gatewayconfigstatus-1 "Direct link to gatewayconfigstatus-1")
**Appears in:**


GatewayConfigStatus defines the observed state of GatewayConfig.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-109 "Direct link to Fields")
`conditions`optional
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions describe the current conditions of the GatewayConfig.
####  HTTPBodyField[​](https://theagentrouter.ai/docs/api/#httpbodyfield-1 "Direct link to httpbodyfield-1")
**Appears in:**


HTTPBodyField represents a JSON field name and value for body mutation
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-110 "Direct link to Fields")
`path`required
string
Path is the top-level field name to set in the request body.Examples: `service_tier`, `max_tokens`, `temperature`
`value`required
string
Value is the JSON value to set at the specified field. This can be any valid JSON value:string, number, boolean, object, array, or null.The value will be parsed as JSON and inserted at the specified field.Examples: - `\`scale\\`` (string)  - `42` (number)  - `true` (boolean)  - `\\{\`key\`: \`value\`\\}` (object)  - `[1, 2, 3]` (array)  - `null` (null)
####  HTTPBodyMutation[​](https://theagentrouter.ai/docs/api/#httpbodymutation-1 "Direct link to httpbodymutation-1")
**Appears in:**
  * [AIGatewayRouteRuleBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayrouterulebackendref)


HTTPBodyMutation defines the mutation of HTTP request body JSON fields that will be applied to the request
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-111 "Direct link to Fields")
`set`optional
[HTTPBodyField](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-httpbodyfield) array
Set overwrites/adds the request body with the given JSON field (name, value)before sending to the backend. Only top-level fields are currently supported.Input: \\{ `model`: `gpt-4`, `service_tier`: `default` \\}Config: set: - path: `service_tier` value: `scale`Output: \\{ `model`: `gpt-4`, `service_tier`: `scale` \\}
`remove`optional
string array
Remove the given JSON field(s) from the HTTP request body before sending to the backend.The value of Remove is a list of top-level field names to remove.Input: \\{ `model`: `gpt-4`, `service_tier`: `default`, `internal_flag`: true \\}Config: remove: [`service_tier`, `internal_flag`]Output: \\{ `model`: `gpt-4` \\}
####  HTTPHeaderMutation[​](https://theagentrouter.ai/docs/api/#httpheadermutation-1 "Direct link to httpheadermutation-1")
**Appears in:**
  * [AIGatewayRouteRuleBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-aigatewayrouterulebackendref)


HTTPHeaderMutation defines the mutation of HTTP headers that will be applied to the request
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-112 "Direct link to Fields")
`set`optional
[HTTPHeader](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#httpheader) array
Set overwrites/adds the request with the given header (name, value)before the action.Input: GET /foo HTTP/1.1 my-header: fooConfig: set: - name: `my-header` value: `bar`Output: GET /foo HTTP/1.1 my-header: bar
`remove`optional
string array
Remove the given header(s) from the HTTP request before the action. Thevalue of Remove is a list of HTTP header names. Note that the headernames are case-insensitive (seehttps://datatracker.ietf.org/doc/html/rfc2616#section-4.2).Input: GET /foo HTTP/1.1 my-header1: foo my-header2: bar my-header3: bazConfig: remove: [`my-header1`, `my-header3`]Output: GET /foo HTTP/1.1 my-header2: bar
**Appears in:**


JWKS defines how to obtain JSON Web Key Sets (JWKS) either from a remote HTTP/HTTPS endpoint or from a local source.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-113 "Direct link to Fields")
`remoteJWKS`optional
RemoteJWKS defines how to fetch and cache JSON Web Key Sets (JWKS) from a remoteHTTP/HTTPS endpoint.
`localJWKS`optional
LocalJWKS defines how to get the JSON Web Key Sets (JWKS) from a local source.
**Appears in:**
  * [MCPAuthorizationSource](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpauthorizationsource)


JWTSource defines the MCP authorization source for JWT tokens. At least one of scopes or claims must be provided. Scopes and claims are AND-ed: when both are specified, both sets must match.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-114 "Direct link to Fields")
`scopes`optional
JWTScope array
Scopes defines the list of JWT scopes required for the rule.If multiple scopes are specified, all scopes must be present in the JWT for the rule to match.
`claims`optional
JWTClaim array
Claims defines the list of JWT claims required for the rule. Each claim must exist on the tokenand have at least one of the expected values. Use to enforce tenant or subject-based access.
####  LLMRequestCost[​](https://theagentrouter.ai/docs/api/#llmrequestcost-1 "Direct link to llmrequestcost-1")
**Appears in:**


LLMRequestCost configures each request cost.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-115 "Direct link to Fields")
`metadataKey`required
string
MetadataKey is the key of the metadata to store this cost of the request.
`type`required
Type specifies the type of the request cost. The default is `OutputToken`,and it uses `output token` as the cost. The other types are `InputToken`, `TotalToken`,`CachedInputToken`, `CacheCreationInputToken`, `ReasoningToken`, and `CEL`.
`cel`optional
string
CEL is the CEL expression to calculate the cost of the request.The CEL expression must return a signed or unsigned integer. If thereturn value is negative, it will be error.The expression can use the following variables: * model: the model name extracted from the request content. Type: string. * backend: the backend name in the form of `name.namespace`. Type: string. * input_tokens: the number of input tokens. Type: unsigned integer. * cached_input_tokens: the number of cached read input tokens. Type: unsigned integer. * cache_creation_input_tokens: the number of cache creation input tokens. Type: unsigned integer. * output_tokens: the number of output tokens. Type: unsigned integer. * total_tokens: the total number of tokens. Type: unsigned integer. * reasoning_tokens: the number of reasoning tokens. Type: unsigned integer.For example, the following expressions are valid: * `model == 'llama' ?  input_tokens + output_token * 0.5 : total_tokens` * `backend == 'foo.default' ?  input_tokens + output_tokens : total_tokens` * `backend == 'bar.default' ?  (input_tokens - cached_input_tokens) + cached_input_tokens * 0.1 + cache_creation_input_tokens * 1.25 + output_tokens : total_tokens` * `input_tokens + output_tokens + total_tokens` * `input_tokens * output_tokens`
####  LLMRequestCostType[​](https://theagentrouter.ai/docs/api/#llmrequestcosttype-1 "Direct link to llmrequestcosttype-1")
**Underlying type:** string
**Appears in:**


LLMRequestCostType specifies the type of the LLMRequestCost.
##### Possible Values[​](https://theagentrouter.ai/docs/api/#possible-values-6 "Direct link to Possible Values")
`InputToken`
LLMRequestCostTypeInputToken is the cost type of the input token. 
`CachedInputToken`
LLMRequestCostTypeCachedInputToken is the cost type of the cached input token. 
`CacheCreationInputToken`
LLMRequestCostTypeCacheCreationInputToken is the cost type of the cached input token. 
`OutputToken`
LLMRequestCostTypeOutputToken is the cost type of the output token. 
`TotalToken`
LLMRequestCostTypeTotalToken is the cost type of the total token. 
`ReasoningToken`
LLMRequestCostTypeReasoningToken is the cost type of the reasoning token. 
LLMRequestCostTypeCEL is for calculating the cost using the CEL expression. 
####  MCPAuthorizationSource[​](https://theagentrouter.ai/docs/api/#mcpauthorizationsource-1 "Direct link to mcpauthorizationsource-1")
**Appears in:**
  * [MCPRouteAuthorizationRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcprouteauthorizationrule)


MCPAuthorizationSource defines the source of an authorization rule.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-116 "Direct link to Fields")
`jwt`required
JWT defines the JWT scopes required for this rule to match.
####  MCPAuthorizationTarget[​](https://theagentrouter.ai/docs/api/#mcpauthorizationtarget-1 "Direct link to mcpauthorizationtarget-1")
**Appears in:**
  * [MCPRouteAuthorizationRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcprouteauthorizationrule)


MCPAuthorizationTarget defines the target of an authorization rule.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-117 "Direct link to Fields")
`tools`required
[ToolCall](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-toolcall) array
Tools defines the list of tools this rule applies to.
####  MCPBackendAPIKey[​](https://theagentrouter.ai/docs/api/#mcpbackendapikey-1 "Direct link to mcpbackendapikey-1")
**Appears in:**
  * [MCPBackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpbackendsecuritypolicy)


MCPBackendAPIKey defines the configuration for the API Key Authentication to a backend. When both `header` and `queryParam` are unspecified, the API key will be injected into the "Authorization" header by default.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-118 "Direct link to Fields")
`secretRef`optional
secretRef is the Kubernetes secret which contains the API keys.The key of the secret should be `apiKey`.
`inline`optional
string
Inline contains the API key as an inline string.
`header`optional
string
Header is the HTTP header to inject the API key into. If not specified,defaults to `Authorization`.When the header is `Authorization`, the injected header value will beprefixed with `Bearer `.Either one of Header or QueryParam can be specified to inject the API key.
`queryParam`optional
string
QueryParam is the HTTP query parameter to inject the API key into.For example, if QueryParam is set to `api_key`, and the API key is `mysecretkey`, the request URL will be modified to include`?api_key=mysecretkey`.Either one of Header or QueryParam can be specified to inject the API key.Note: Embedding credentials in URLs (including query parameters) is generally not recommended because URLs can be exposed in logsand intermediary systems; prefer header-based injection when possible.
####  MCPBackendSecurityPolicy[​](https://theagentrouter.ai/docs/api/#mcpbackendsecuritypolicy-1 "Direct link to mcpbackendsecuritypolicy-1")
**Appears in:**


MCPBackendSecurityPolicy defines the security policy for a backend MCP server.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-119 "Direct link to Fields")
`apiKey`optional
APIKey is a mechanism to access a backend. The API key will be injected into the request headers.
####  MCPBackendSelector[​](https://theagentrouter.ai/docs/api/#mcpbackendselector-1 "Direct link to mcpbackendselector-1")
**Appears in:**


MCPBackendSelector defines which of a MCPRoute's backends a request may fan out to.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-120 "Direct link to Fields")
`defaultAction`optional
Deny
DefaultAction is the action to take when no rules match. If unspecified, defaults to Deny.
`rules`optional
[MCPBackendSelectorRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpbackendselectorrule) array
Rules defines a list of backend selection rules.These rules are evaluated in order, the first matching rule will be applied,and the rest will be skipped.If no rules are defined, DefaultAction is applied to every candidate backend(defaults to Deny).
####  MCPBackendSelectorRule[​](https://theagentrouter.ai/docs/api/#mcpbackendselectorrule-1 "Direct link to mcpbackendselectorrule-1")
**Appears in:**


MCPBackendSelectorRule defines a single backend selection rule.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-121 "Direct link to Fields")
`cel`optional
string
CEL specifies a Common Expression Language (CEL) expression, evaluated once foreach backend already listed in backendRefs. It does not parse a list of backendsout of the request. Each evaluation binds request.mcp.backend to the name of theone candidate backend under test, so the expression should answer `is this backendallowed`, not `which backends should be used`. The expression must return aboolean; evaluation errors or non-boolean results are treated as `no match`. Amatch means the rule applies, and Action below decides whether that's an Allow ora Deny for that backend.Example CEL expressions: * `request.mcp.backend in request.auth.jwt.claims.mcp_backends` * `(`,` + request.headers[`x-ai-eg-mcp-backend-subset`] + `,`).contains(`,` + request.mcp.backend + `,`)`Available attributes are the same as documented on MCPRouteAuthorizationRule.CEL,except request.mcp.method, request.mcp.tool, and request.mcp.params are notpopulated (no MCP method has been selected yet at backend-selection time).Note: The CEL expression support is experimental, and the attributesavailable to the expression may change in future releases.
`action`optional
Allow
Action is the decision for matching backends. If unspecified, defaults to Allow.
####  MCPHeaderForward[​](https://theagentrouter.ai/docs/api/#mcpheaderforward-1 "Direct link to mcpheaderforward-1")
**Appears in:**


MCPHeaderForward specifies a header to extract from the incoming request and forward to a backend.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-122 "Direct link to Fields")
`name`required
string
Name is the header name to extract from the incoming client request.
`backendHeader`optional
string
BackendHeader is the header name to use when forwarding to the backend.If not specified, the original header name is used.
####  MCPRouteAuthorization[​](https://theagentrouter.ai/docs/api/#mcprouteauthorization-1 "Direct link to mcprouteauthorization-1")
**Appears in:**
  * [MCPRouteSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcproutesecuritypolicy)


MCPRouteAuthorization defines the authorization configuration for a MCPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-123 "Direct link to Fields")
`defaultAction`optional
Deny
DefaultAction is the action to take when no rules match. If unspecified, defaults to Deny.
`rules`optional
[MCPRouteAuthorizationRule](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcprouteauthorizationrule) array
Rules defines a list of authorization rules.These rules are evaluated in order, the first matching rule will be applied,and the rest will be skipped.If no rules are defined, the default action will be applied to all requests.
####  MCPRouteAuthorizationRule[​](https://theagentrouter.ai/docs/api/#mcprouteauthorizationrule-1 "Direct link to mcprouteauthorizationrule-1")
**Appears in:**
  * [MCPRouteAuthorization](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcprouteauthorization)


MCPRouteAuthorizationRule defines an authorization rule for MCPRoute based on the MCP authorization spec. Reference: <https://modelcontextprotocol.io/specification/draft/basic/authorization#scope-challenge-handling>
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-124 "Direct link to Fields")
`source`required
[MCPAuthorizationSource](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpauthorizationsource)
Source defines the authorization source for this rule.If not specified, the rule will match all sources.
`target`required
[MCPAuthorizationTarget](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpauthorizationtarget)
Target defines the authorization target for this rule.If not specified, the rule will match all targets.
`cel`optional
string
CEL specifies a Common Expression Language (CEL) expression evaluated for this rule.The expression must return a boolean; evaluation errors or non-boolean resultsare treated as `no match`.Example CEL expressions: * `request.method == `POST`request.headers[`x-custom-header`] == `AllowedValue`request.mcp.tool in [`toolA`, `toolB`]`Available attributes in the CEL expression:	* request.method: HTTP method such as GET or POST. Type: string.	* request.headers: map of headers with lowercased keys, first value only. Type: map[string]string.	* request.headers_all: map of headers with lowercased keys, all values. Type: map[string][]string.	* request.path: request path such as /mcp. Type: string.	* request.auth.jwt.claims: JWT claims when a bearer JWT is present. Type: map[string]any.	* request.auth.jwt.scopes: JWT scopes when a bearer JWT is present. Type: []string.	* request.mcp.method: MCP method such as tools/list or tools/call. Type: string.	* request.mcp.backend: upstream backend name (for example, `kiwi` or `github`). Type: string.	* request.mcp.tool: tool name without backend prefix (for example, `list_issues`). Type: string.	* request.mcp.params: parameters of the MCP method, including keys like `_meta` and `arguments`. Type: object.Note: The CEL expression support is experimental, and the attributesavailable to the expression may change in future releases.
`action`optional
Allow
Action is the authorization decision for matching requests. If unspecified, defaults to Allow.
####  MCPRouteBackendRef[​](https://theagentrouter.ai/docs/api/#mcproutebackendref-1 "Direct link to mcproutebackendref-1")
**Appears in:**


MCPRouteBackendRef wraps a EG's BackendObjectReference to reference an MCP server.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-125 "Direct link to Fields")
`group`optional
Group is the group of the referent. For example, `gateway.networking.k8s.io`.When unspecified or empty string, core API group is inferred.
`kind`optional
Service
Kind is the Kubernetes resource kind of the referent. For example`Service`.Defaults to `Service` when not specified.ExternalName services can refer to CNAME DNS records that may liveoutside of the cluster and as such are difficult to reason about interms of conformance. They also may not be safe to forward to (seeCVE-2021-25740 for more information). Implementations SHOULD NOTsupport ExternalName Services.Support: Core (Services with a type other than ExternalName)Support: Implementation-specific (Services with type ExternalName)
`name`required
Name is the name of the referent.
`namespace`optional
Namespace is the namespace of the backend. When unspecified, the localnamespace is inferred.Note that when a namespace different than the local namespace is specified,a ReferenceGrant object is required in the referent namespace to allow thatnamespace's owner to accept the reference. See the ReferenceGrantdocumentation for details.Support: Core
`port`optional
Port specifies the destination port number to use for this resource.Port is required when the referent is a Kubernetes Service. In thiscase, the port number is the service port number, not the target port.For other resources, destination port might be derived from the referentresource or this field.
`path`optional
string
/mcp
Path is the HTTP endpoint path of the backend MCP server.If not specified, the default is `/mcp`.
`toolSelector`optional
ToolSelector filters the tools exposed by this MCP server.Supports exact matches and RE2-compatible regular expressions for both include and exclude patterns.If not specified, all tools from the MCP server are exposed.
`securityPolicy`optional
[MCPBackendSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpbackendsecuritypolicy)
SecurityPolicy is the security policy to apply to this MCP server.
`forwardHeaders`optional
[MCPHeaderForward](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpheaderforward) array
ForwardHeaders specifies HTTP headers to extract from the incoming client requestand forward to this backend MCP server.This enables per-user authentication passthrough (e.g., personal access tokens)without requiring OAuth configuration.Each entry specifies a header name to extract and an optional rename for the backend.
####  MCPRouteOAuth[​](https://theagentrouter.ai/docs/api/#mcprouteoauth-1 "Direct link to mcprouteoauth-1")
**Appears in:**
  * [MCPRouteSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcproutesecuritypolicy)


MCPRouteOAuth defines a MCP spec compatible OAuth authentication configuration for a MCPRoute. Reference: <https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization>
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-126 "Direct link to Fields")
`issuer`required
string
Issuer is the authorization server's issuer identity.
`audiences`optional
string array
Audiences is a list of JWT audiences allowed access.It is recommended to set this field for token audience validation, as it is a security best practice to prevent token misuse.Reference: https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization#token-audience-binding-and-validation
`jwks`optional
JWKS defines how a JSON Web Key Sets (JWKS) can be obtained to verify the access tokens presented by the clients.If not specified, the JWKS URI will be discovered from the OAuth 2.0 Authorization Server Metadataas per RFC 8414 by querying the `/.well-known/oauth-authorization-server` endpoint on the Issuer.
`protectedResourceMetadata`required
[ProtectedResourceMetadata](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-protectedresourcemetadata)
ProtectedResourceMetadata defines the OAuth 2.0 Resource Server Metadata as per RFC 8414.This is used to expose the metadata endpoint for mcp clients to discover the authorization servers,supported scopes, and JWKS URI.
`claimToHeaders`optional
ClaimToHeader array
ClaimToHeaders specifies JWT claims to extract and forward as HTTP headers to backend MCP servers.This enables backends to access user identity for authorization, auditing, or personalization.Security considerations:- Any client-provided headers matching the configured header names will be stripped to prevent forgery- Only the specified claims are extracted; the full JWT is not forwarded to backends- Consider using a header prefix (e.g., `X-Jwt-Claim-`) to avoid conflicts with other headers
####  MCPRouteSecurityPolicy[​](https://theagentrouter.ai/docs/api/#mcproutesecuritypolicy-1 "Direct link to mcproutesecuritypolicy-1")
**Appears in:**


MCPRouteSecurityPolicy defines the security policy for a MCPRoute.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-127 "Direct link to Fields")
`oauth`optional
OAuth defines the configuration for the MCP spec compatible OAuth authentication.
`apiKeyAuth`optional
APIKeyAuth defines the configuration for the API Key Authentication.
`extAuth`optional
ExtAuth defines the configuration for External Authorization.
`authorization`optional
[MCPRouteAuthorization](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcprouteauthorization)
Authorization defines the configuration for the MCP spec compatible authorization.
**Appears in:**


MCPRouteSpec details the MCPRoute configuration.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-128 "Direct link to Fields")
`parentRefs`required
[ParentReference](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#parentreference) array
ParentRefs are the names of the Gateway resources this MCPRoute is being attached to.Cross namespace references are not supported. In other words, the Gateway resources must be in thesame namespace as the MCPRoute. Currently, each reference's Kind must be Gateway.
`path`optional
string
/mcp
Path is the HTTP endpoint path that serves MCP requests over the Streamable HTTP transport.If not specified, the default is `/mcp`.
`headers`optional
[HTTPHeaderMatch](https://gateway-api.sigs.k8s.io/reference/spec/?h=httproutetimeouts#httpheadermatch) array
Headers are HTTP headers that must match for this route to be selected.Multiple match values are ANDed together, meaning, a request must match all the specified headers to select the route.
`hostnames`optional
Hostname array
Hostnames is a list of hostnames matched against the HTTP Host header to select this MCPRoute.This is equivalent to the Hostnames field in the Gateway API HTTPRouteSpec. When specified, thegenerated HTTPRoute includes these hostnames, so the MCP route lands in the per-host virtual hostinstead of the wildcard vhost (the same scoping AIGatewayRoute.Hostnames provides).See https://gateway-api.sigs.k8s.io/reference/api-types/httproute/#hostnamesfor the details of the Hostnames field in the Gateway API.
`backendRefs`required
[MCPRouteBackendRef](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcproutebackendref) array
BackendRefs is a list of backend references to the MCP servers.These MCP servers will be aggregated and exposed as a single MCP endpoint to the clients.From the client's perspective, they only need to configure a single MCP server URL, e.g. `https://api.example.com/mcp`,and the Envoy AI Gateway will route the requests to the appropriate MCP server based on the requests.All names must be unique within this list to avoid potential tools, resources, etc. name collisions.Also, cross-namespace references are not supported. In other words, the backend MCP servers must be in thesame namespace as the MCPRoute.
`securityPolicy`optional
[MCPRouteSecurityPolicy](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcproutesecuritypolicy)
SecurityPolicy defines the security policy for this MCPRoute.
`backendSelector`optional
BackendSelector restricts which of this route's backends a given request may fanout to, evaluated once per candidate backend when a client session is initialized.If unspecified, all backends on the route are considered.
####  MCPRouteStatus[​](https://theagentrouter.ai/docs/api/#mcproutestatus-1 "Direct link to mcproutestatus-1")
**Appears in:**


MCPRouteStatus contains the conditions by the reconciliation result.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-129 "Direct link to Fields")
`conditions`required
[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#condition-v1-meta) array
Conditions is the list of conditions by the reconciliation result.Currently, at most one condition is set.Known .status.conditions.type are: `Accepted`, `NotAccepted`.
####  MCPToolFilter[​](https://theagentrouter.ai/docs/api/#mcptoolfilter-1 "Direct link to mcptoolfilter-1")
**Appears in:**


MCPToolFilter filters tools using include and exclude patterns with exact matches or regular expressions. Exclude rules take precedence over include rules (deny-wins). When both include and exclude are specified, a tool must match an include rule AND not match any exclude rule to be allowed.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-130 "Direct link to Fields")
`include`optional
string array
Include is a list of tool names to include. Only the specified tools will be available.
`includeRegex`optional
string array
IncludeRegex is a list of RE2-compatible regular expressions that, when matched, include the tool.Only tools matching these patterns will be available.
`exclude`optional
string array
Exclude is a list of tool names to exclude. The specified tools will not be available.Exclude rules take precedence over include rules.
`excludeRegex`optional
string array
ExcludeRegex is a list of RE2-compatible regular expressions that, when matched, exclude the tool.Tools matching these patterns will not be available. Exclude rules take precedence over include rules.
####  ProtectedResourceMetadata[​](https://theagentrouter.ai/docs/api/#protectedresourcemetadata-1 "Direct link to protectedresourcemetadata-1")
**Appears in:**


ProtectedResourceMetadata represents the Protected Resource Metadata of the MCP server as per RFC 9728.
References:
  * <https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization#authorization-server-location>
  * <https://datatracker.ietf.org/doc/html/rfc9728#name-protected-resource-metadata>


##### Fields[​](https://theagentrouter.ai/docs/api/#fields-131 "Direct link to Fields")
`resource`required
string
Resource is the identifier of the protected resource.This should match the MCPRoute's URL. For example, if the MCPRoute's URL is`https://api.example.com/mcp`, the Resource should be `https://api.example.com/mcp`.
`resourceName`optional
string
ResourceName is a human-readable name for the protected resource.
`scopesSupported`optional
string array
ScopesSupported defines the minimal set of scopes required for the basic functionality of the MCPRoute.It should avoid broad or overly permissive scopes to prevent clients from requesting tokens with excessive privileges.If an operation requires additional scopes that are not present in the access token, the client will receive a403 Forbidden response that includes the required scopes in the `scope` field of the `WWW-Authenticate` header.This enables incremental privilege elevation through targeted `WWW-Authenticate: scope=`...`` challenges whenprivileged operations are first attempted.
`resourceSigningAlgValuesSupported`optional
string array
ResourceSigningAlgValuesSupported is a list of JWS signing algorithms supported by the resource server.These algorithms are used in the `alg` field of the JOSE header in signed tokens.
`resourceDocumentation`optional
string
ResourceDocumentation is a URL that provides human-readable documentation for the resource.
`resourcePolicyUri`optional
string
ResourcePolicyURI is a URL that points to the resource server's policy document.
**Appears in:**
  * [MCPAuthorizationTarget](https://theagentrouter.ai/docs/api/#github-com-envoyproxy-ai-gateway-api-v1beta1-mcpauthorizationtarget)


ToolCall represents a tool call in the MCP authorization target.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-132 "Direct link to Fields")
`backend`required
string
Backend is the name of the backend this tool belongs to.
`tool`required
string
Tool is the name of the tool.
####  VersionedAPISchema[​](https://theagentrouter.ai/docs/api/#versionedapischema-1 "Direct link to versionedapischema-1")
**Appears in:**


VersionedAPISchema defines the API schema of either AIGatewayRoute (the input) or AIServiceBackend (the output).
This allows the ai-gateway to understand the input and perform the necessary transformation depending on the API schema pair (input, output).
Note that this is vendor specific, and the stability of the API schema is not guaranteed by the ai-gateway, but by the vendor via proper versioning.
##### Fields[​](https://theagentrouter.ai/docs/api/#fields-133 "Direct link to Fields")
`name`required
Name is the name of the API schema of the AIGatewayRoute or AIServiceBackend.
`version`optional
string
Version is the version of the API schema.When the name is set to AzureOpenAI, this version maps to `API Version` in theAzure OpenAI API documentation (https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#rest-api-versioning).This field is ignored for OpenAI, AWSBedrock, GCPVertexAI, and Anthropic.For OpenAI and Anthropic, use prefix to configure custom request paths.See https://aigateway.envoyproxy.io/docs/capabilities/llm-integrations/supported-providers for details.
`prefix`optional
string
Prefix is the prefix for the API.When the name is set to `OpenAI`, `chat completions` API endpoint will be `$\{this_field\}/chat/completions`.When the name is set to `Anthropic`, the `messages` API endpoint will be `$\{this_field\}/messages`.It can be with or without a leading slash (`/`).This field is ignored for AWSAnthropic and GCPAnthropic.This is especially useful when routing to a backend that has an OpenAI or Anthropic compatible API but has a differentprefix. For example, Gemini OpenAI compatible API (https://ai.google.dev/gemini-api/docs/openai) uses`/v1beta/openai` prefix. Another example is that Cohere AI (https://docs.cohere.com/v2/docs/compatibility-api)uses `/compatibility/v1` prefix. On the other hand, DeepSeek (https://api-docs.deepseek.com/) doesn'tuse prefix, so you can leave this field unset.See https://aigateway.envoyproxy.io/docs/capabilities/llm-integrations/supported-providers for details.
  * [aigateway.envoyproxy.io/v1alpha1](https://theagentrouter.ai/docs/api/#aigatewayenvoyproxyiov1alpha1)
  * [Resource Kinds](https://theagentrouter.ai/docs/api/#resource-kinds)
    * [Kind Definitions](https://theagentrouter.ai/docs/api/#kind-definitions)
      * BackendSecurityPolicy
      * BackendSecurityPolicyList
  * [Supporting Types](https://theagentrouter.ai/docs/api/#supporting-types)
    * [Type Definitions](https://theagentrouter.ai/docs/api/#type-definitions)
      * AIGatewayRouteRuleBackendRef
      * AIGatewayRouteRuleMatch
      * AIServiceBackendStatus
      * AzureOIDCExchangeToken
      * BackendSecurityPolicyAPIKey
      * BackendSecurityPolicyAWSCredentials
      * BackendSecurityPolicyAnthropicAPIKey
      * BackendSecurityPolicyAzureAPIKey
      * BackendSecurityPolicyAzureCredentials
      * BackendSecurityPolicyGCPCredentials
      * BackendSecurityPolicyOIDC
      * BackendSecurityPolicySpec
      * BackendSecurityPolicyStatus
      * BackendSecurityPolicyType
      * GCPServiceAccountImpersonationConfig
      * GCPWorkloadIdentityFederationConfig
      * GatewayConfigForwardProxy
      * MCPAuthorizationSource
      * MCPAuthorizationTarget
      * MCPBackendSecurityPolicy
      * MCPBackendSelectorRule
      * MCPRouteAuthorization
      * MCPRouteAuthorizationRule
      * MCPRouteSecurityPolicy
      * ProtectedResourceMetadata
      * ServiceQuotaDefinition
  * [aigateway.envoyproxy.io/v1beta1](https://theagentrouter.ai/docs/api/#aigatewayenvoyproxyiov1beta1)
  * [Resource Kinds](https://theagentrouter.ai/docs/api/#resource-kinds-1)
    * [Kind Definitions](https://theagentrouter.ai/docs/api/#kind-definitions-1)
      * BackendSecurityPolicy
      * BackendSecurityPolicyList
  * [Supporting Types](https://theagentrouter.ai/docs/api/#supporting-types-1)
    * [Type Definitions](https://theagentrouter.ai/docs/api/#type-definitions-1)
      * AIGatewayRouteRuleBackendRef
      * AIGatewayRouteRuleMatch
      * AIServiceBackendStatus
      * AzureOIDCExchangeToken
      * BackendSecurityPolicyAPIKey
      * BackendSecurityPolicyAWSCredentials
      * BackendSecurityPolicyAnthropicAPIKey
      * BackendSecurityPolicyAzureAPIKey
      * BackendSecurityPolicyAzureCredentials
      * BackendSecurityPolicyCredentialOverride
      * BackendSecurityPolicyGCPCredentials
      * BackendSecurityPolicyOIDC
      * BackendSecurityPolicySpec
      * BackendSecurityPolicyStatus
      * BackendSecurityPolicyType
      * CredentialOverrideFromDynamicMetadata
      * CredentialOverrideFromRequestHeaders
      * GCPServiceAccountImpersonationConfig
      * GCPWorkloadIdentityFederationConfig
      * GatewayConfigForwardProxy
      * MCPAuthorizationSource
      * MCPAuthorizationTarget
      * MCPBackendSecurityPolicy
      * MCPBackendSelectorRule
      * MCPRouteAuthorization
      * MCPRouteAuthorizationRule
      * MCPRouteSecurityPolicy
      * ProtectedResourceMetadata


