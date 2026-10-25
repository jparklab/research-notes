---
title: Enterprise | liteLLM
id: enterprise-litellm
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- enterprise-tier
- oss-boundary
created: '2026-10-09T23:00:09.162081Z'
updated: '2026-10-10T14:52:17.143477Z'
source: https://docs.litellm.ai/docs/enterprise
source_domain: docs.litellm.ai
fetched_at: '2026-10-09T23:00:09.140117Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'LiteLLM''s own canonical statement of the OSS/Enterprise boundary -- the
  single most direct piece of boundary evidence in this batch. Architecture is layered,
  not forked: ''Enterprise is a license key on the same Gateway image you may already
  run... each layer includes everything below it'' (Python SDK OSS -> AI Gateway OSS
  self-hosted, adds budgets+rate limits -> Enterprise license key, adds org/team admins
  + per-team logging). Enterprise is explicitly scoped for ''100+ users or 10+ production
  AI use cases''; SSO is free for up to 5 users, beyond that an Enterprise license
  is required. Full enterprise feature list spans four buckets: (1) Security/access
  -- SSO (Okta/Azure AD/Google Workspace/OIDC/SAML), JWT auth, audit logs with retention,
  RBAC (Organizations/teams/roles), IP allowlisting, automated virtual-key rotation,
  external secret managers (AWS KMS/Secrets Manager, Azure Key Vault, Google KMS,
  HashiCorp Vault, CyberArk); (2) Governance/cost -- Multi-tenant Architecture (orgs/teams/projects/keys),
  Project Management (budget+owners+rate-limits+model-allowlist per project), Tag-based
  Budgets, Model-specific Budgets per Virtual Key, Temporary Budget Increases, Soft
  Budget Email Alerts, programmatic Spend Reports; (3) Observability/compliance --
  SOC 2 Type II audit, Team-Based Logging (route each team''s logs to its own Langfuse
  project), per-team logging opt-out (GDPR), log export to GCS/Azure Blob, several
  named enterprise-only guardrail integrations; (4) Operations/branding -- custom
  API-docs branding, custom email branding, request/response size limits, team-managed
  models (bring-your-own-key/fine-tune). Explicitly names which built-in guardrail
  callbacks require a license: llmguard_moderations, llamaguard_moderations, hide_secrets,
  openai_moderations, google_text_moderation, lakera_prompt_injection, aporia_prompt_injection
  (Presidio PII masking and custom guardrails remain OSS). Confirms fully self-hosted/air-gapped
  operation is explicit: ''No data leaves your environment,'' SOC 2 Type II audited,
  Docker images cryptographically signed and independently verifiable, procurable
  via AWS/Azure Marketplace or direct invoice -- this is the closest any source in
  this batch comes to an explicit air-gap claim for a self-hosted LLM gateway.'
---

*Suggested by [[multi-tenant-architecture-with-litellm-litellm]] — linked as the enterprise-tier feature doc from the multi-tenant architecture page*

[Skip to main content](https://docs.litellm.ai/docs/enterprise#__docusaurus_skipToContent_fallback)
[**Decisions API is here.** Call Jev, Clef or any decision model through one /v1/decisions endpoint. →](https://docs.litellm.ai/docs/decisions)
On this page
Copy page
## What you get on top of open source[​](https://docs.litellm.ai/docs/enterprise#what-you-get-on-top-of-open-source "Direct link to What you get on top of open source")
Enterprise is a license key on the same Gateway image you may already run. Nothing to migrate: each layer includes everything below it.
EnterpriseLicense key on the Gateway
Everything in AI Gateway, plus
  * [Org and team admins](https://docs.litellm.ai/docs/proxy/access_control)
  * [Per-team logging](https://docs.litellm.ai/docs/proxy/team_logging)


AI GatewayOpen source, self-hosted
Everything in the SDK, plus
  * [Budgets and rate limits](https://docs.litellm.ai/docs/proxy/users)


Python SDKOpen source library
In your Python code
  * [100+ providers](https://docs.litellm.ai/docs/providers)
  * [Router and fallbacks](https://docs.litellm.ai/docs/routing)
  * [Logging callbacks](https://docs.litellm.ai/docs/observability/callbacks)


## What changes for your organization[​](https://docs.litellm.ai/docs/enterprise#what-changes-for-your-organization "Direct link to What changes for your organization")
[Audited security you can hand to your review team](https://trust.litellm.ai/)SOC 2 Type II audited, self-hosted so no data leaves your environment, and signed Docker images you can verify.[Verify the signed images](https://docs.litellm.ai/docs/proxy/docker_image_security)
[Everyone signs in with your identity providerSSO and SCIM for Okta, Entra ID, Google Workspace, or any OIDC or SAML provider. Access follows your org chart.](https://docs.litellm.ai/docs/proxy/admin_ui_sso)[Every admin action is on recordAudit logs of key, team, and model changes, with retention policies your compliance team sets.](https://docs.litellm.ai/docs/proxy/multiple_admins)[Teams run their own keys and budgetsTeam admins manage keys, models, and budgets per project, tag, and model without the master key, with alerts before anyone hits a cap.](https://docs.litellm.ai/docs/proxy/access_control)[Provider keys stay in your vaultAWS, Azure, Google, HashiCorp Vault, and CyberArk secret managers, with automatic virtual key rotation.](https://docs.litellm.ai/docs/secret_managers/overview)[Engineers on callA dedicated Slack or Teams channel with the people who build LiteLLM, and 24/7 SLAs down to one hour.](https://docs.litellm.ai/docs/enterprise/support)
## Who is Enterprise for?[​](https://docs.litellm.ai/docs/enterprise#who-is-enterprise-for "Direct link to Who is Enterprise for?")
For teams running LiteLLM at scale (100+ users or 10+ production AI use cases) that need SSO, audit logs, fine-grained access control, and professional support on top of open source. SSO is free for up to 5 users; beyond that, an Enterprise license is required. Engineers evaluating a trial can follow the [Production rollout](https://docs.litellm.ai/docs/learn/enterprise_quickstart) guide, and teams already on open source can start with [Moving from OSS](https://docs.litellm.ai/docs/enterprise/moving_from_oss).
## Full feature list[​](https://docs.litellm.ai/docs/enterprise#full-feature-list "Direct link to Full feature list")
Everything below is enabled by the license key. Each item links to its setup guide.
### Security and access[​](https://docs.litellm.ai/docs/enterprise#security-and-access "Direct link to Security and access")
  * **[SSO for the Admin UI](https://docs.litellm.ai/docs/proxy/admin_ui_sso)**. Okta, Azure AD, Google Workspace, and any OIDC/SAML provider
  * **[JWT-based Authentication](https://docs.litellm.ai/docs/proxy/token_auth)**. Authenticate requests with your identity provider's tokens
  * **[Audit Logs with retention policies](https://docs.litellm.ai/docs/proxy/multiple_admins)**. Track every admin action and key-level change
  * **[Role-Based Access Control](https://docs.litellm.ai/docs/proxy/access_control)**. Organizations, teams, and user roles
  * **[Public and private route controls](https://docs.litellm.ai/docs/proxy/public_routes)**. Restrict admin routes and lock down surface area
  * **[IP address-based access control lists](https://docs.litellm.ai/docs/proxy/ip_address)**. Restrict proxy access to specific CIDR ranges
  * . Automate rotation for virtual keys
  * . AWS KMS, AWS Secrets Manager, Azure Key Vault, Google KMS, Google Secret Manager, HashiCorp Vault, CyberArk, or a custom secret manager
  * . Share a public, branded page of available models, MCP servers, agents, and skills


### Governance and cost[​](https://docs.litellm.ai/docs/enterprise#governance-and-cost "Direct link to Governance and cost")
  * **[Multi-tenant Architecture](https://docs.litellm.ai/docs/proxy/multi_tenant_architecture)**. Organizations, teams, projects, and keys
  * **[Project Management](https://docs.litellm.ai/docs/proxy/project_management)**. Group keys by application or use-case, with a budget, owners, rate limits, a model allowlist, and an isolated spend view. See the [UI walkthrough](https://docs.litellm.ai/docs/proxy/ui_project_management)
  * **[Tag-based Budgets](https://docs.litellm.ai/docs/proxy/provider_budget_routing)**. Budgets and spend tracking by custom tag
  * **[Model-specific Budgets per Virtual Key](https://docs.litellm.ai/docs/proxy/users)**. Different limits per model, per key
  * **[Temporary Budget Increases](https://docs.litellm.ai/docs/proxy/temporary_budget_increase)**. Time-boxed spend bumps without permanent changes
  * **[Soft Budget Email Alerts](https://docs.litellm.ai/docs/proxy/ui_team_soft_budget_alerts)**. Warn teams before they hit hard limits
  * **[Generate Spend Reports](https://docs.litellm.ai/docs/proxy/cost_tracking#-enterprise-generate-spend-reports)**. Programmatic access to spend by key, team, tag, or model


### Observability and compliance[​](https://docs.litellm.ai/docs/enterprise#observability-and-compliance "Direct link to Observability and compliance")
  * . Audited controls; request the report through the Trust Center
  * **[Team-Based Logging](https://docs.litellm.ai/docs/proxy/team_logging)**. Route each team's logs to their own Langfuse project or callback
  * **[Disable logging per team](https://docs.litellm.ai/docs/proxy/team_logging#disable-logging-for-a-team)**. GDPR-friendly opt-out at the team level
  * **[Log export to GCS / Azure Blob](https://docs.litellm.ai/docs/observability/gcs_bucket_integration)**. Durable storage for compliance
  * **[Guardrails per key/team](https://docs.litellm.ai/docs/enterprise#guardrails-oss-vs-enterprise)**. Secret redaction, content moderation, banned keywords
  * **Enforced required params**. Reject requests missing required metadata


### Operations and branding[​](https://docs.litellm.ai/docs/enterprise#operations--branding "Direct link to Operations and branding")
  * **Custom Swagger branding**. Set your own title, description, and filtered routes on the API docs page
  * **[Custom email branding](https://docs.litellm.ai/docs/proxy/email#email-customization)**. Your logo and colors on system emails
  * **Max request/response size limits**. Protect the proxy from runaway payloads
  * **[Team-managed models](https://docs.litellm.ai/docs/proxy/team_model_add)**. Let teams bring their own keys and fine-tunes


### Which guardrails need a license?[​](https://docs.litellm.ai/docs/enterprise#guardrails-oss-vs-enterprise "Direct link to Which guardrails need a license?")
The OSS guardrail framework includes custom guardrails and Presidio for PII masking. These built-in callback integrations require a LiteLLM Enterprise license: `llmguard_moderations`, `llamaguard_moderations`, `hide_secrets`, `openai_moderations`, `google_text_moderation`, `lakera_prompt_injection`, and `aporia_prompt_injection`.
## Run it[​](https://docs.litellm.ai/docs/enterprise#run-it "Direct link to Run it")
Deploy the Docker image, or build from the pip package, on your own infrastructure. A license key enables the features above and includes a dedicated support channel.

```
LITELLM_LICENSE="eyJ..."
```

No data leaves your environment, and LiteLLM is [SOC 2 Type II](https://trust.litellm.ai/) audited. [Procurement is available through AWS and Azure Marketplace.](https://docs.litellm.ai/docs/data_security#legalcompliance-faqs)
Pricing depends on your deployment size. [Get in touch](https://enterprise.litellm.ai/demo) to scope it.
## Support[​](https://docs.litellm.ai/docs/enterprise#professional-support "Direct link to Support")
Every license includes a dedicated Slack or Teams channel with the engineering team. With the optional 24/7 SLAs, the response time is 1 hour for Sev 0 and 72 hours for security patches. See [Support and SLA](https://docs.litellm.ai/docs/enterprise/support) for hours, the full severity table, and custom SLAs.
## Version support[​](https://docs.litellm.ai/docs/enterprise#version-support "Direct link to Version support")
LiteLLM supports the four most recent stable minor lines, and Enterprise and open source share one image and one version number. See [Version support](https://docs.litellm.ai/docs/enterprise/version_support).
## FAQ[​](https://docs.litellm.ai/docs/enterprise#faq "Direct link to FAQ")
### How do I set up and verify an Enterprise License?[​](https://docs.litellm.ai/docs/enterprise#how-do-i-set-up-and-verify-an-enterprise-license "Direct link to How do I set up and verify an Enterprise License?")
Add the license key to your environment, then restart the proxy.

```
LITELLM_LICENSE="eyJ..."
```

Open `http://<your-proxy-host>:<port>/`. The API docs page should show **Enterprise Edition** in the description. If it does not, confirm the key is correct and unexpired, and that the proxy was fully restarted.
### Is LiteLLM SOC 2 compliant?[​](https://docs.litellm.ai/docs/enterprise#is-litellm-soc-2-compliant "Direct link to Is LiteLLM SOC 2 compliant?")
Yes. LiteLLM is SOC 2 Type II audited. Request the current report through the [LiteLLM Trust Center](https://trust.litellm.ai/). For data handling, vulnerability reporting, and the rest of the security review, see [Data Security, Legal, and Compliance FAQs](https://docs.litellm.ai/docs/data_security).
### Can we buy through AWS or Azure Marketplace?[​](https://docs.litellm.ai/docs/enterprise#can-we-buy-through-aws-or-azure-marketplace "Direct link to Can we buy through AWS or Azure Marketplace?")
Yes. You can buy an Enterprise license through AWS Marketplace or Azure Marketplace, or directly by invoice. See [Procurement Options](https://docs.litellm.ai/docs/data_security#legalcompliance-faqs).
### Where can I read more about data security and compliance?[​](https://docs.litellm.ai/docs/enterprise#where-can-i-read-more-about-data-security-and-compliance "Direct link to Where can I read more about data security and compliance?")
See [Data Security, Legal, and Compliance FAQs](https://docs.litellm.ai/docs/data_security).
### How is pricing structured?[​](https://docs.litellm.ai/docs/enterprise#how-is-pricing-structured "Direct link to How is pricing structured?")
Pricing is based on usage. [Contact us](https://enterprise.litellm.ai/demo) for a quote tailored to your team.
### How do I get day-0 support for new models without restarting?[​](https://docs.litellm.ai/docs/enterprise#how-do-i-get-day-0-support-for-new-models-without-restarting "Direct link to How do I get day-0 support for new models without restarting?")
Use [Auto Sync New Models](https://docs.litellm.ai/docs/proxy/sync_models_github) to pull the latest pricing and context-window data from GitHub on demand or on a schedule, with no restart required. Trigger a manual sync with `POST /reload/model_cost_map`, or schedule periodic syncs with `POST /schedule/model_cost_map_reload?hours=6`.
  * [What you get on top of open source](https://docs.litellm.ai/docs/enterprise#what-you-get-on-top-of-open-source)
  * [What changes for your organization](https://docs.litellm.ai/docs/enterprise#what-changes-for-your-organization)
  * [Who is Enterprise for?](https://docs.litellm.ai/docs/enterprise#who-is-enterprise-for)
  * [Full feature list](https://docs.litellm.ai/docs/enterprise#full-feature-list)
    * [Observability and compliance](https://docs.litellm.ai/docs/enterprise#observability-and-compliance)
    * [Operations and branding](https://docs.litellm.ai/docs/enterprise#operations--branding)
    * [Which guardrails need a license?](https://docs.litellm.ai/docs/enterprise#guardrails-oss-vs-enterprise)
  * [FAQ](https://docs.litellm.ai/docs/enterprise#faq)
    * [How do I set up and verify an Enterprise License?](https://docs.litellm.ai/docs/enterprise#how-do-i-set-up-and-verify-an-enterprise-license)
    * [Is LiteLLM SOC 2 compliant?](https://docs.litellm.ai/docs/enterprise#is-litellm-soc-2-compliant)
    * [Can we buy through AWS or Azure Marketplace?](https://docs.litellm.ai/docs/enterprise#can-we-buy-through-aws-or-azure-marketplace)
    * [Where can I read more about data security and compliance?](https://docs.litellm.ai/docs/enterprise#where-can-i-read-more-about-data-security-and-compliance)
    * [How is pricing structured?](https://docs.litellm.ai/docs/enterprise#how-is-pricing-structured)
    * [How do I get day-0 support for new models without restarting?](https://docs.litellm.ai/docs/enterprise#how-do-i-get-day-0-support-for-new-models-without-restarting)


LiteLLM Enterprise
SSO/SAML, audit logs, spend tracking, multi-team management, and guardrails, built for production.
[Learn more →](https://docs.litellm.ai/docs/enterprise)
