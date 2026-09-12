---
title: '[Project Proposal] Agent Router (previously Envoy AI Gateway) · Issue #18
  · aaif/project-proposals'
id: project-proposal-agent-router-previously-envoy-ai-gateway-issue-18-aaifproject-p
tags:
- llmd-router-nonk8s-backends-54b774
- envoy-ai-gateway
- two-tier-gateway
- governance
- aaif
created: '2026-09-11T19:45:48.381919Z'
updated: '2026-09-12T03:37:37.615139Z'
source: https://github.com/aaif/project-proposals/issues/18
source_domain: github.com
fetched_at: '2026-09-11T19:45:48.359881Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'AAIF (AI/Agent open-source foundation) project-proposal GitHub issue (opened
  Apr 24, 2026, status: Approved) for admitting Envoy AI Gateway (since renamed Agent
  Router) as a foundation project. Explicitly names and defines the ''Two-tier gateway
  pattern'' that structures this research: Tier One Gateway = centralized entry point
  for auth, top-level routing, and global token-based rate limiting across all 16+
  external AI providers (OpenAI, Azure OpenAI, AWS Bedrock, Google Gemini/Vertex AI,
  Anthropic, Mistral, Cohere, Groq, DeepSeek, etc.) under one OpenAI-compatible API;
  Tier Two Gateway = fine-grained control over self-hosted model access with native
  InferencePool (endpoint picker) support for LLM inference optimization. Lists ''Self-Hosted
  Model Clusters'' as a named use case: ''Infrastructure teams deploy the Tier Two
  gateway pattern to manage traffic to self-hosted model serving clusters (e.g., KServe/InferencePool),
  with intelligent endpoint routing for inference optimization.'' Confirms AWS ''has
  embraced it as the preferred AI GW system... for EKS.'' Governance/provenance data:
  originated Oct 2024 (Bloomberg + Tetrate, from the Envoy community''s ''Cloud Native
  LLM Gateway'' proposal), v0.1.0 shipped Feb 2025, v0.5.0 by Jan 2026 (21 releases
  total), 1500+ GitHub stars, 1300+ commits, 97 contributors, maintainers from Bloomberg/Tetrate/Tencent/Nutanix/Google
  (no single-company control), key runtime dependency sigs.k8s.io/gateway-api-inference-extension
  v1.0.2. Also documents a standalone (non-Kubernetes) ''aigw CLI'' mode including
  stdio MCP server proxying, and an MCP Gateway with CEL-based per-tool authorization
  and OAuth. Roadmap includes graduating CRDs from v1alpha1 to v1beta1/stable and
  an ''Enhanced Quota management API.'''
utility_score: 9.0
---

[Skip to content](https://github.com/aaif/project-proposals/issues/18#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/aaif/project-proposals/issues/18) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/aaif/project-proposals/issues/18) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/aaif/project-proposals/issues/18) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/aaif/project-proposals/issues/18).
/ **[project-proposals](https://github.com/aaif/project-proposals) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Faaif%2Fproject-proposals) You must be signed in to change notification settings
  * [ Fork 3 ](https://github.com/login?return_to=%2Faaif%2Fproject-proposals)
  * [ Star  39 ](https://github.com/login?return_to=%2Faaif%2Fproject-proposals)


#  [Project Proposal] Agent Router (previously Envoy AI Gateway)
Copy link
Copy link
Closed
Closed
[[Project Proposal] Agent Router (previously Envoy AI Gateway)](https://github.com/aaif/project-proposals/issues/18#top)#18
Copy link
Assignees
Labels
[ApprovedThe proposal has been reviewed and approved for inclusion in the foundation.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Approved%22)The proposal has been reviewed and approved for inclusion in the foundation.[](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Growth%22)[contribution-agreement/signedThe contribution agreement is signed.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22contribution-agreement%2Fsigned%22)The contribution agreement is signed.
## Description
opened [on Apr 24, 2026](https://github.com/aaif/project-proposals/issues/18#issue-4324168093)
Issue body actions
### Project Name
Envoy AI Gateway
### Project Description
Envoy AI Gateway is an open-source project that enables Envoy Gateway to handle request traffic from application clients to Generative AI (GenAI) services. Built on top of Envoy Proxy and Envoy Gateway, it provides a secure, scalable, and operationally excellent gateway for managing LLM and AI inference traffic at enterprise scale.
The project was initiated in October 2024 as a collaboration between Bloomberg and Tetrate, inspired by the 'Cloud Native LLM Gateway' proposal in the Envoy community. Its first stable release (v0.1.0) shipped in February 2025, and it has since grown to v0.5.0 (January 2026).
Two-tier gateway pattern:
  * Tier One Gateway: Centralized entry point handling authentication, top-level routing, and global token-based rate limiting across all AI providers.
  * Tier Two Gateway: Fine-grained control over self-hosted model access, with InferencePool (endpoint picker) support for LLM inference optimization.


Key capabilities:
  * Unified API: Abstracts 16+ AI providers (OpenAI, Azure OpenAI, AWS Bedrock, Google Gemini/Vertex AI, Anthropic, Mistral, Cohere, Groq, DeepSeek, and more) under a single OpenAI-compatible API.
  * Token-based (usage-based) rate limiting: Controls LLM cost and consumption with per-user, per-model, and per-team limits.
  * Upstream authentication: Secures connections to AI providers (API keys, AWS IAM/STS, Azure AD, GCP service accounts, OAuth 2.0 token exchange).
  * Model virtualization and provider fallback: Resilient multi-provider routing with automatic failover.
  * MCP Gateway: First-class support for Model Context Protocol with server multiplexing, CEL-based authorization, OAuth, and OpenTelemetry observability.
  * InferencePool integration: Native integration with Gateway API Inference Extension for intelligent inference routing.
  * Comprehensive observability: Prometheus metrics and OpenTelemetry tracing following OpenTelemetry GenAI Semantic Conventions.
  * Standalone mode (aigw CLI): Run without Kubernetes, including stdio MCP server proxying.


### Alignment with AAIF Mission
Envoy AI Gateway directly advances the AAIF's mission by providing vendor-neutral, enterprise-grade infrastructure for the AI ecosystem:
  * Vendor neutrality: Supports 16+ AI providers (OpenAI, AWS Bedrock, Azure OpenAI, Google Gemini/Vertex AI, Anthropic, Mistral, Cohere, Groq, DeepSeek, and more) under a single OpenAI-compatible API, preventing lock-in.
  * MCP ecosystem alignment: Full compliance with the MCP specification (June 2025), making it a natural network-layer counterpart to MCP-based agentic AI systems and a direct infrastructure enabler for AAIF's MCP project.
  * Enterprise governance for AI agents: Provides the security (OAuth, CEL authorization, JWT), observability (OpenTelemetry GenAI metrics, distributed tracing), and policy enforcement (rate limiting, content routing) that enterprises require to confidently adopt AI agents in production.
  * Open, multi-company collaboration: Founded and maintained by engineers from Bloomberg, Tetrate, Google, Tencent, and Nutanix — demonstrating the kind of cross-industry, open collaboration the AAIF was built to foster.


### Relation to Existing AAIF Projects
  * MCP (Anthropic): Envoy AI Gateway has first-class, production-grade MCP Gateway support. It provides server multiplexing, tool routing, OAuth/JWT authorization, CEL-based fine-grained per-tool access control, upstream authentication, and OpenTelemetry observability. The implementation follows the MCP June 2025 specification and is actively used with MCP clients, including Claude, Goose, and custom agents.
  * Goose (Block): Envoy AI Gateway is directly usable as the network-layer governance layer for Goose-based agentic workflows. Goose agents connecting to MCP servers or LLMs route through Envoy AI Gateway to gain security, governance, rate limiting, and observability. The projects are architecturally complementary.
  * agents.md(OpenAI): Envoy AI Gateway can serve as the secure, observable proxy layer for agents following the agents.md standard, providing the network-level security and routing controls that enterprise deployments require.


### Example Use Cases and Evidence of Adoption
Use cases:
  * Enterprise AI Platform Teams: Platform engineers use Envoy AI Gateway to provide a centralized, governed gateway for all LLM/AI traffic - managing authentication, rate limiting, cost control, and multi-provider routing from a single control point.
  * Multi-Provider AI Applications: Application developers build AI features with seamless failover between providers (e.g., OpenAI → Azure OpenAI → AWS Bedrock) without code changes, with token-level rate limiting per team or user.
  * Secure MCP-Based Agentic Systems: AI agents (using Goose, Claude, or custom frameworks) connect to multiple MCP tool servers through the gateway, which enforces OAuth authentication, CEL-based per-tool authorization, and full OpenTelemetry tracing.
  * Self-Hosted Model Clusters: Infrastructure teams deploy the Tier Two gateway pattern to manage traffic to self-hosted model serving clusters (e.g., KServe/InferencePool), with intelligent endpoint routing for inference optimization.


Evidence of adoption:
  * **Bloomberg:** Multiple maintainers; presented at KubeCon NA 2024 (End User Keynote) and KubeCon EU 2025/2026 on production deployment for enterprise LLM traffic management.
  * **Tencent Cloud:** Listed production adopter; Envoy Gateway maintainer Xunzhuo Liu is a project maintainer.
  * **Tetrate:** Commercial products built on Envoy AI Gateway; multiple founding maintainers.
  * **Nutanix:** Multiple maintainers; Listed production adopter; Johnu George (Kubeflow and KServe maintainer) is a maintainer.
  * **LY Corporation:** Production adopter.
  * **National Research Platform (nrp.ai):** AI research infrastructure adopter.
  * **Alan by Comma Soft, Paper Compute Co., Simplifai:** Listed production adopters.
  * Envoy AI Gateway has reached 1500+ github stars, 1300+ commits across 95+ contributors and 20+ releases.
  * In addition, AWS has embraced it as the preferred AI GW system in for EKS


### Technical Committee Sponsor (if identified)
Manik from Block as a user of the Tetrate service on top of Envoy AI GW in Goose. Sambhav from Bloomberg, as a user.
### GitHub Repository URL
<https://github.com/envoyproxy/ai-gateway>
### License
Apache 2.0
### Governance Model
  * Maintainers are listed in MAINTAINERS.md and represent multiple independent organizations (Bloomberg, Tetrate, Tencent, Nutanix).
  * Decision-making is by consensus among maintainers, with a CODEOWNERS file defining per-area review responsibilities.
  * The project follows the CNCF Code of Conduct.
  * Community meetings are held weekly (Mondays) and are open to all, with public meeting notes at <https://docs.google.com/document/d/10e1sfsF-3G3Du5nBHGmLjXw5GVMqqCvFDqp_O65B0_w>
  * Contributions require DCO sign-off as documented in CONTRIBUTING.md.
  * Upon acceptance into AAIF, the project is prepared to adopt AAIF-standard governance and formalize a GOVERNANCE.md.


_Note: Several maintainers are active in related CNCF/Linux Foundation communities (Envoy, KServe, Kubeflow, Gateway API Inference Extension)._
### CI/CD & Release Workflow
CI/CD (GitHub Actions workflows):
  * build_and_test.yaml: Runs on every PR and push to main/release branches. Includes unit tests (ubuntu-latest and macos-latest), integration tests, data plane tests, and e2e tests.
  * precommit_check.yaml: Linting, formatting, license headers, and spell-checking.
  * pr_style_check.yaml: Enforces conventional commit format for PR titles.
  * codeql.yaml: Automated security scanning on push to main and weekly schedule.
  * docker_build_job.yaml: Multi-platform container image builds pushed to docker.io/envoyproxy.
  * Dependabot: Weekly automated dependency updates for Go modules, GitHub Actions, and npm.


Release cadence:
  * Follows the Envoy Gateway release cycle (approximately every 2–3 months), plus additional patch releases as needed.
  * 21 total releases from February 2025 to January 2026 (v0.1.0 through v0.5.0, including patch releases).
  * Release branches (release/vX.Y) maintained for non-EOL versions with backported patch releases.
  * Container images published to Docker Hub (docker.io/envoyproxy/ai-gateway-*).


### Public-Facing Contribution Process for Specifications
  * Contributions are governed by CONTRIBUTING.md: <https://github.com/envoyproxy/ai-gateway/blob/main/CONTRIBUTING.md>
  * Pull requests require Developer Certificate of Origin (DCO) sign-off (git commit -s).
  * All PRs must pass precommit checks (make precommit) and include tests for all new code paths.
  * PRs are reviewed by maintainers listed in CODEOWNERS; consensus-based approval is required.
  * Feature proposals and architectural changes follow a design proposal process (docs/proposals/ directory).
  * Community engagement via weekly Monday meetings, the #envoy-ai-gateway Slack channel, and GitHub Issues.


### Publicly Accessible Issue Tracker
<https://github.com/envoyproxy/ai-gateway/issues>
### External Project Dependencies
All dependencies are under permissive open-source licenses (Apache 2.0, MIT, or BSD). Key runtime dependencies:
  * Core infrastructure: github.com/envoyproxy/gateway v1.7.0 (Apache 2.0), github.com/envoyproxy/go-control-plane v0.14.0 (Apache 2.0), sigs.k8s.io/gateway-api v1.4.1 (Apache 2.0), sigs.k8s.io/gateway-api-inference-extension v1.0.2 (Apache 2.0), sigs.k8s.io/controller-runtime v0.23.3 (Apache 2.0), k8s.io/api + k8s.io/client-go v0.35.3 (Apache 2.0).
  * AI provider SDKs: github.com/openai/openai-go v1.12.0 (Apache 2.0), github.com/anthropics/anthropic-sdk-go v1.27.1 (MIT), github.com/aws/aws-sdk-go-v2 v1.41.4 (Apache 2.0), github.com/Azure/azure-sdk-for-go/sdk/azcore v1.21.0 (MIT), google.golang.org/api v0.273.0 (BSD-3-Clause), google.golang.org/genai v1.51.0 (Apache 2.0), github.com/cohere-ai/cohere-go/v2 v2.18.0 (MIT).
  * MCP: github.com/modelcontextprotocol/go-sdk v1.4.1 (MIT).
  * Observability: go.opentelemetry.io/otel v1.43.0 (Apache 2.0), github.com/prometheus/client_golang v1.23.2 (Apache 2.0).
  * Authorization: github.com/google/cel-go v0.27.0 (Apache 2.0), github.com/golang-jwt/jwt/v5 v5.3.1 (MIT), github.com/coreos/go-oidc/v3 v3.17.0 (Apache 2.0).


### Maintainers & Contributors
Full maintainer list (from MAINTAINERS.md):
  * Takeshi Yoneda ([@mathetake](https://github.com/mathetake)) - Netflix. Area: Everything. Also an Envoy Proxy maintainer.
  * Dan Sun ([@yuzisun](https://github.com/yuzisun)) - Bloomberg. Area: Everything, enterprise integration & core LLM features. Also a KServe maintainer.
  * Erica Hughberg ([@missBerg](https://github.com/missBerg)) - Tetrate. Area: Documentation, Website, Community.
  * Aaron Choo ([@aabchoo](https://github.com/aabchoo)) - Bloomberg. Area: Control Plane, Security Policy, Testing.
  * Yao Weng ([@wengyao04](https://github.com/wengyao04)) - Bloomberg. Area: Control Plane, Testing.
  * Xunzhuo (Bit) Liu ([@Xunzhuo](https://github.com/Xunzhuo)) - Tencent. Area: Control Plane, Inference Pool & Gateway API Inference Extension. Also an Envoy Gateway maintainer.
  * Ignasi Barrera ([@nacx](https://github.com/nacx)) - Tetrate. Area: MCP, aigw CLI (standalone mode).
  * Johnu George ([@johnugeorge](https://github.com/johnugeorge)) - Nutanix. Area: Enterprise features and integration, LLM features. Kubeflow and KServe maintainer.
  * Gavrish Prabhu ([@gavrissh](https://github.com/gavrissh)) - Nutanix. Area: Control plane, Testing.


Contributors: 97 unique contributors as of April 2026, representing Bloomberg, Tetrate, Google, Tencent, Nutanix, and the broader open-source community. Full contributor list: <https://github.com/envoyproxy/ai-gateway/graphs/contributors>
### Leadership Team & Decision Process
Leadership is vested in the maintainer group, representing at least 4 independent organizations (Bloomberg, Tetrate, Tencent, Nutanix, Google), ensuring no single company controls the project.
Decision process:
  * Day-to-day decisions (PR reviews, bug fixes, minor features) require approval from at least one maintainer.
  * Significant changes (API changes, major features, architectural decisions) follow the design proposal process (docs/proposals/) and require broader maintainer consensus.
  * Disputes are resolved by maintainer consensus, consistent with CNCF/Envoy community norms.
  * Governance questions are discussed in weekly community meetings and the #envoy-ai-gateway Slack channel.
  * The project is committed to adopting AAIF's formal governance process upon acceptance.


### Roadmap
  * API stability and path to v1.0: Graduate CRDs from v1alpha1 to v1beta1 and stable, with documented migration paths. Stabilize the AIGatewayRoute, LLMBackend, and MCPRoute APIs.
  * Enhanced Quota management API for seamless quota-based routing.
  * Expanded MCP capabilities: Advanced authorization patterns (A2A protocol support, more CEL primitives), Token Exchange support in MCP.
  * Support for Batch APis.
  * Expanded AI provider coverage: Additional providers and OpenAI-compatible backends; improved vendor-specific field support.
  * Prompt guard and content safety: Integration points for content filtering, PII detection, and safety policy enforcement.
  * Performance and scalability: Continued benchmarking and optimization of the external processor and control plane.
  * Community growth: Growing the adopter base, adding case studies and architecture patterns, and internationalizing documentation.
  * OpenSSF Best Practices badge: Pursue Silver badge or better as part of AAIF onboarding.


Full roadmap discussion happens in weekly community meetings: <https://docs.google.com/document/d/10e1sfsF-3G3Du5nBHGmLjXw5GVMqqCvFDqp_O65B0_w>
### Security
The project has not yet received an OpenSSF Best Practices badge but plans to pursue one as part of the AAIF donation process. Current security practices include:
  * CodeQL static analysis runs on every push to main and on a weekly schedule (GitHub Actions codeql.yaml workflow).
  * Dependabot automated dependency updates (weekly) for Go modules, GitHub Actions, and npm packages.
  * No secrets committed to the repository; credentials are managed via Kubernetes Secrets and referenced by CRDs.
  * CI pipeline runs linting, unit, integration, and e2e tests on every PR.
  * Container images are published with pinned SHA-based base image references for reproducibility.
  * Developer Certificate of Origin (DCO) required on all commits.
  * A formal security vulnerability reporting policy will be established upon AAIF donation.


### Website URL
<https://aigateway.envoyproxy.io/>
### Documented Governance Practices (if any)
The project currently follows Envoy/CNCF community governance norms. Relevant governance documents:
  * MAINTAINERS.md: <https://github.com/envoyproxy/ai-gateway/blob/main/MAINTAINERS.md>
  * CONTRIBUTING.md: <https://github.com/envoyproxy/ai-gateway/blob/main/CONTRIBUTING.md>
  * CODE_OF_CONDUCT.md (references CNCF Code of Conduct): <https://github.com/envoyproxy/ai-gateway/blob/main/CODE_OF_CONDUCT.md>
  * CODEOWNERS: <https://github.com/envoyproxy/ai-gateway/blob/main/CODEOWNERS>


A formal GOVERNANCE.md will be created upon acceptance into AAIF, adopting AAIF-standard governance.
### Links to Social Media Accounts
LinkedIn: <https://www.linkedin.com/company/envoy-cloud-native>
### Details of Existing Financial Sponsorship
The project is currently hosted under the CNCF/Envoy project umbrella. Infrastructure and engineering contributions come from:
  * Bloomberg: Significant engineering investment; multiple maintainers (Dan Sun, Aaron Choo, Yao Weng).
  * Tetrate: Provides cloud accounts for CI e2e tests. Founding contributor and significant engineering investment; multiple maintainers; commercial product built on the project.
  * Google: Engineering contributions from Google employees (Yan Avlasov and others). Provides Gemini AI account for code reviews.
  * Tencent: Engineering contributions; Envoy Gateway maintainer (Xunzhuo Liu) as project maintainer.
  * Nutanix: Engineering contributions; Multiple maintainers; Kubeflow and KServe maintainer (Johnu George) as maintainer.
  * Website hosting is provided via Netlify; container images are published to Docker Hub.


### Infrastructure Needs or Requests
Current infrastructure:
  * CI/CD: GitHub Actions (GitHub-hosted runners, ubuntu-latest and macos-latest).
  * Container registry: Docker Hub (docker.io/envoyproxy/ai-gateway-*).
  * Website hosting: Netlify (<https://aigateway.envoyproxy.io>).
  * Community meetings: Zoom via LFX platform (weekly Mondays).


Requests upon joining AAIF:
  * Shared CI/CD resources for expanded end-to-end testing (particularly tests requiring cloud provider credentials for OpenAI, AWS Bedrock, Azure OpenAI, GCP Vertex AI, etc.).


### Additional Information
As opposed to single vendor projects like agent gateway, Kong gateway, Cloudflare gateway, this is a truly community-driven project of end users, vendors, and cloud providers. We would love to continue to grow Envoi AI Gateway in the AAIF, happy to adopt whichever governance model AAIF recommends.
Envoy AI Gateway stands out as a uniquely mature and production-proven open-source AI gateway:
  * Built on Envoy Proxy - the most widely deployed proxy in cloud-native infrastructure (used by Istio, AWS App Mesh, Google Traffic Director, and hundreds of enterprises), giving Envoy AI Gateway immediate advantages in reliability, performance, and ecosystem compatibility.
  * Multi-company from day one: Founded as a collaboration between Bloomberg and Tetrate, with contributions from Google, Tencent, Nutanix, and others - demonstrating genuine community ownership and preventing single-vendor capture.
  * Comprehensive AI-native feature set: The only open-source gateway with native support for token-based rate limiting, multi-provider failover, cloud-native upstream authentication (AWS IAM/STS, Azure AD, GCP Service Accounts), MCP gateway with CEL-based authorization, InferencePool integration, and OpenTelemetry GenAI metrics - all in a single, Kubernetes-native package.
  * Rapidly growing ecosystem: 16+ AI provider integrations, 9+ production adopters, presented at 8+ major conferences in under 18 months of existence.
  * Roadmap to v1.0: The project is actively working toward API stability and a v1.0 release, signaling readiness for long-term enterprise commitment.


The AAIF is the ideal home for Envoy AI Gateway, providing a neutral governance structure that reflects the project's multi-stakeholder nature and aligning it with the emerging standards for AI agent infrastructure. We welcome the opportunity to collaborate with other AAIF projects - particularly MCP and Goose - to build the open, secure, and observable connectivity layer that enterprise AI adoption requires.
🎉React with 🎉1Reacted by Anibal Angulo
## Activity
assigned [on Apr 24, 2026](https://github.com/aaif/project-proposals/issues/18#event-24844158479)
added 
[New Project Proposal; has not yet been reviewed by AAIF staff or the TC.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22New%22)New Project Proposal; has not yet been reviewed by AAIF staff or the TC.
[on Apr 24, 2026](https://github.com/aaif/project-proposals/issues/18#event-24844159582)
[github-project-automation](https://github.com/apps/github-project-automation)
added this to [on Apr 24, 2026](https://github.com/aaif/project-proposals/issues/18#event-24844162265)
[github-project-automation](https://github.com/apps/github-project-automation)
moved this to 📥 New in [on Apr 24, 2026](https://github.com/aaif/project-proposals/issues/18)
### aeoess commented on Apr 25, 2026 
Last edited by aeoess
More actions
[@nacx](https://github.com/nacx) the network-layer + action-layer split between Envoy AI Gateway and agent-action-authorization gateways like APS is a clean composition seam, and the AAIF stack benefits from making it explicit so neither project is implicitly framed as duplicate of the other.
What the two control:
  * **Envoy AI Gateway** sits at the network boundary. It controls _which clients can reach which providers, at what rate, with what transport auth_ (OAuth, JWT, AWS IAM, Azure AD), and provides the OpenAI-compatible abstraction across 16+ providers. The MCP Gateway support handles tool routing and CEL-based per-tool access at the protocol layer. This is a service-mesh primitive applied to LLM/MCP traffic.
  * **APS / Agent Passport System** sits at the action authorization boundary. It controls _which agent is delegated to perform which action with which data scope_ , evaluating a cryptographic delegation chain (Ed25519, monotonic narrowing of scope, parent-signs-child) and emitting a signed receipt for each evaluated action. It does not route traffic; it answers "is this agent authorized to take this action right now under its delegation?"


The two compose in series: an enterprise deployment routes LLM/MCP traffic through Envoy AI Gateway for transport-level auth and rate limiting, and routes agent-initiated actions through an APS-style gateway for delegation-chain authorization and receipt issuance. Same agent, two checks, different questions.
Where the integration would help both projects:
  1. **OpenTelemetry GenAI traces carrying delegation context.** If Envoy AI Gateway's OpenTelemetry traces emit a `delegation_chain_root_hash` span attribute when the upstream caller passes one (today APS receipts already produce this), enterprise observability stacks can correlate network-layer traffic with action-layer authorization in a single trace. No protocol changes; just a documented optional attribute.
  2. **MCP Gateway authorization composition.** Envoy AI Gateway's CEL-based per-tool authorization is policy at the network boundary. APS-style delegation chains are policy at the agent boundary. Both can co-exist: CEL deny-lists at the gateway as the outer ring; delegation-chain narrowing at the action gateway as the inner ring. Worth one sentence in the docs so enterprise architects know they don't have to pick one.
  3. **Provider failover + delegation invariants.** When Envoy AI Gateway fails over from OpenAI → Azure OpenAI → Bedrock, the agent's delegation chain doesn't change; only the upstream provider does. Worth confirming in the spec that provider-fallback events do not invalidate downstream delegation scope (they shouldn't — provider is a transport detail, not a delegation parameter — but spelling it out keeps enterprise audit teams unblocked).


We've shipped APS as Apache 2.0 since February (`agent-passport-system` on npm/PyPI, 7 papers, IETF draft `draft-pidlisnyi-aps-00`). Happy to engage on the integration points above in whatever surface makes sense — Envoy AI Gateway's docs/proposals/, AAIF cross-project working group, or a sample reference deployment as a separate repo.
— Model Citizen
[christinaharter](https://github.com/christinaharter)
moved this from 📥 New to 🔜 Upcoming in [on May 4, 2026](https://github.com/aaif/project-proposals/issues/18)
[christinaharter](https://github.com/christinaharter)
added 
[Under ReviewProposal is being actively reviewed by AAIF staff or prepared for a TC meeting.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Under%20Review%22)Proposal is being actively reviewed by AAIF staff or prepared for a TC meeting.
[contribution-agreement/unsignedThe contribution agreement remains unsigned.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22contribution-agreement%2Funsigned%22)The contribution agreement remains unsigned.
and removed 
[New Project Proposal; has not yet been reviewed by AAIF staff or the TC.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22New%22)New Project Proposal; has not yet been reviewed by AAIF staff or the TC.
[on May 4, 2026](https://github.com/aaif/project-proposals/issues/18#event-25145608911)
### christinaharter commented on May 4, 2026 
Contributor
More actions
Hi [@nacx](https://github.com/nacx) - Thank you for your submission. The Contribution Agreement draft has been sent by email (to nacx@apache.org) for your review so you and your legal team have time to review it while the Technical Committee and Governing Board consider the proposal. It will be executed if both bodies vote to accept.
👍React with 👍1Reacted by Ignasi Barrera
[christinaharter](https://github.com/christinaharter)
moved this from 🔜 Upcoming to ⏳ Waiting in [on May 4, 2026](https://github.com/aaif/project-proposals/issues/18)
[christinaharter](https://github.com/christinaharter)
added 
[Paperwork-in-reviewPaperwork is currently under review by project submitter.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Paperwork-in-review%22)Paperwork is currently under review by project submitter.
and removed 
[Under ReviewProposal is being actively reviewed by AAIF staff or prepared for a TC meeting.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Under%20Review%22)Proposal is being actively reviewed by AAIF staff or prepared for a TC meeting.
[on May 5, 2026](https://github.com/aaif/project-proposals/issues/18#event-25196864762)
[maniksurtani](https://github.com/maniksurtani)
moved this from ⏳ Waiting to 🗳️ TC Voting in [on May 12, 2026](https://github.com/aaif/project-proposals/issues/18)
###  7 remaining items
Load more
[christinaharter](https://github.com/christinaharter)
added 
[TC ApprovedThe TC has approved this project for inclusion into the AAIF.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22TC%20Approved%22)The TC has approved this project for inclusion into the AAIF.
[on May 21, 2026](https://github.com/aaif/project-proposals/issues/18#event-25813327501)
[christinaharter](https://github.com/christinaharter)
moved this from ⏳ Waiting to 🗳️ GB Voting in [on Jul 23, 2026](https://github.com/aaif/project-proposals/issues/18)
[christinaharter](https://github.com/christinaharter)
removed 
[Paperwork-in-reviewPaperwork is currently under review by project submitter.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Paperwork-in-review%22)Paperwork is currently under review by project submitter.
[on Jul 23, 2026](https://github.com/aaif/project-proposals/issues/18#event-28391867157)
### christinaharter commented on Jul 23, 2026 
Contributor
More actions
Hi [@nacx](https://github.com/nacx) - The new name, Agent Router, is approved and the paperwork is now finalized. We will move this to a Governing Board vote. The GB has requested 10 days to review to allow enough time for internal review. I will update you as soon as the vote is complete.
[christinaharter](https://github.com/christinaharter)
changed the title ~~[-][Project Proposal] Envoy AI Gateway[/-]~~ [+][Project Proposal] Agent Router (previously Envoy AI Gateway)[/+] [on Jul 23, 2026](https://github.com/aaif/project-proposals/issues/18#event-28391950576)
[maniksurtani](https://github.com/maniksurtani)
added 
[ApprovedThe proposal has been reviewed and approved for inclusion in the foundation.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Approved%22)The proposal has been reviewed and approved for inclusion in the foundation.
and removed 
[GB VoteProject is approved by the TC; now under final review & vote by the AAIF Governing Board.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22GB%20Vote%22)Project is approved by the TC; now under final review & vote by the AAIF Governing Board.
[TC ApprovedThe TC has approved this project for inclusion into the AAIF.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22TC%20Approved%22)The TC has approved this project for inclusion into the AAIF.
[on Aug 18, 2026](https://github.com/aaif/project-proposals/issues/18#event-29598183701)
[maniksurtani](https://github.com/maniksurtani)
moved this from 🗳️ GB Voting to 🔏 Contributor Agreement Unsigned in [on Aug 18, 2026](https://github.com/aaif/project-proposals/issues/18)
[christinaharter](https://github.com/christinaharter)
added 
[contribution-agreement/signedThe contribution agreement is signed.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22contribution-agreement%2Fsigned%22)The contribution agreement is signed.
and removed 
[contribution-agreement/unsignedThe contribution agreement remains unsigned.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22contribution-agreement%2Funsigned%22)The contribution agreement remains unsigned.
[on Aug 18, 2026](https://github.com/aaif/project-proposals/issues/18#event-29628199944)
[maniksurtani](https://github.com/maniksurtani)
moved this from 🔏 Contributor Agreement Unsigned to Onboarding in Progress in [on Aug 19, 2026](https://github.com/aaif/project-proposals/issues/18)
[christinaharter](https://github.com/christinaharter)
moved this from Onboarding in Progress to ✅ Done in [on Sep 10, 2026](https://github.com/aaif/project-proposals/issues/18)
[christinaharter](https://github.com/christinaharter)
closed this as [completed](https://github.com/aaif/project-proposals/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)by moving to ✅ Done in [Project-Proposals-Board](https://github.com/orgs/aaif/projects/1)[on Sep 10, 2026](https://github.com/aaif/project-proposals/issues/18#event-30926515792)
[Sign up for free](https://github.com/signup?return_to=https://github.com/aaif/project-proposals/issues/18)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/aaif/project-proposals/issues/18)
## Metadata
## Metadata
### Assignees
  * [christinaharter](https://github.com/christinaharter)
  * [maniksurtani](https://github.com/maniksurtani)


### Labels
[ApprovedThe proposal has been reviewed and approved for inclusion in the foundation.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Approved%22)The proposal has been reviewed and approved for inclusion in the foundation.[](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22Growth%22)[contribution-agreement/signedThe contribution agreement is signed.](https://github.com/aaif/project-proposals/issues?q=state%3Aopen%20label%3A%22contribution-agreement%2Fsigned%22)The contribution agreement is signed.
No type
### Projects
  * Status
Show more project fields


### Milestone
No milestone
### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
