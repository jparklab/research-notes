---
title: LiteLLM AI Gateway | liteLLM
id: litellm-ai-gateway-litellm
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- ai-gateway
created: '2026-10-09T22:53:57.097185Z'
updated: '2026-10-10T14:52:17.009585Z'
source: https://docs.litellm.ai/docs/simple_proxy
source_domain: docs.litellm.ai
fetched_at: '2026-10-09T22:53:57.072570Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'LiteLLM''s canonical gateway-overview doc. The AI Gateway is a self-hosted
  server giving one OpenAI-compatible endpoint to 100+ providers, MCP tools, and A2A
  agents; clients that speak the OpenAI API need no code changes. Core gateway functions
  enumerated: access control (per user/team/project virtual keys, each with its own
  models/budgets/rate limits), spend tracking (per key/team/user/tag), reliability
  (load balancing, fallbacks, retries across deployments), guardrails (content filters,
  PII masking), logging (Langfuse, Datadog, OpenTelemetry, Prometheus), and an Admin
  UI. Deployment path: Docker/Helm/Terraform + a connected Postgres database, i.e.
  a standard self-hostable stack with no mandated external SaaS dependency described
  on this page. States the gateway-vs-SDK decision rule explicitly: use the gateway
  once more than one application/person calls LLMs or you need virtual keys/spend
  tracking/centralized logs/guardrails; use the plain Python SDK for a single Python
  app that doesn''t need those controls.'
utility_score: 11.0
---

[Skip to main content](https://docs.litellm.ai/docs/simple_proxy#__docusaurus_skipToContent_fallback)
[**Decisions API is here.** Call Jev, Clef or any decision model through one /v1/decisions endpoint. →](https://docs.litellm.ai/docs/decisions)
On this page
Copy page
The LiteLLM AI Gateway is a self-hosted server. It gives your applications one OpenAI-compatible endpoint for 100+ LLM providers, MCP tools, and A2A agents.
Your applications send requests to the gateway. The gateway sends each request to the provider of the model. Then the gateway sends the response back to your application in the OpenAI format. If a client can use the OpenAI API, the client can also use the gateway. You do not change the client code.
Start the gateway now
macOS & LinuxmacOS/LinuxWindows

```
irm https://raw.githubusercontent.com/BerriAI/litellm/main/scripts/quickstart.ps1 | iex
```
Copy
When the command completes, open the Admin UI at `http://localhost:4000/ui`. For each step, refer to the [Quickstart](https://docs.litellm.ai/docs/proxy/docker_quick_start).
Copy agent prompt
## What the gateway does[​](https://docs.litellm.ai/docs/simple_proxy#what-the-gateway-does "Direct link to What the gateway does")
  * **Access control.** Give each user, team, and project a virtual key. Each key has its models, budgets, and rate limits.
  * **Spend tracking.** The gateway records the cost of each request. You can see the spend for each key, team, user, and tag.
  * **Reliability.** Load balancing, fallbacks, and retries move traffic between deployments when a provider has a problem.
  * **Guardrails.** Apply content filters, PII masking, and policies to requests and responses.
  * **Logging.** Send logs and metrics to Langfuse, Datadog, OpenTelemetry, Prometheus, and other tools.
  * **Admin UI.** Add models, add keys, and look at spend and logs in a browser.
  * **MCP and agents.** The same gateway also gives access to [MCP tools](https://docs.litellm.ai/docs/mcp) and [A2A agents](https://docs.litellm.ai/docs/a2a). It uses the same keys and spend records.


## Set up the gateway[​](https://docs.litellm.ai/docs/simple_proxy#set-up-the-gateway "Direct link to Set up the gateway")
The pages in this section follow the steps of a new deployment. Do the steps in the table that follows:  
| Step  | What you do  | Start here  |  
| --- | --- | --- |  
| 1. Deploy  | Start the gateway with Docker, Helm, or Terraform. Connect a Postgres database.  |  [Quickstart](https://docs.litellm.ai/docs/proxy/docker_quick_start), [Production deployment](https://docs.litellm.ai/docs/proxy/deploy)  |  
| 2. Add models and providers  | Add the models that your users can call. Add MCP tools and agents if necessary.  |  [Model management](https://docs.litellm.ai/docs/proxy/model_management), [Providers](https://docs.litellm.ai/docs/providers)  |  
| 3. Connect clients  | Set the gateway URL and a virtual key in your applications and coding tools.  |  
| 4. Set up authentication  | Add virtual keys. Connect your identity provider for single sign-on.  |  [Virtual keys](https://docs.litellm.ai/docs/proxy/virtual_keys), [Admin UI SSO](https://docs.litellm.ai/docs/proxy/admin_ui_sso)  |  
| 5. Track spend and set budgets  | Assign costs to teams and projects. Set budgets and rate limits.  |  [Spend tracking](https://docs.litellm.ai/docs/proxy/cost_tracking), [Budgets](https://docs.litellm.ai/docs/proxy/users)  |  
## Call the gateway[​](https://docs.litellm.ai/docs/simple_proxy#call-the-gateway "Direct link to Call the gateway")
After you start the gateway, use the OpenAI client with the gateway URL and a virtual key:

```
import openaiclient = openai.OpenAI(api_key="sk-your-virtual-key", base_url="http://localhost:4000")response = client.chat.completions.create(    model="gpt-5.6-luna",    messages=[{"role":"user","content":"Write a short poem"}],print(response.choices[0].message.content)
```

## Admin UI[​](https://docs.litellm.ai/docs/simple_proxy#admin-ui "Direct link to Admin UI")
The Admin UI is part of the gateway. Use it to set up and monitor the gateway in a browser:
  * Models
  * Virtual keys
  * MCP servers
  * Guardrails
  * Usage
  * Logs


Add models from each provider. The table shows the price of input and output tokens for each model.
Give each application or person a virtual key. Each key has a team, a budget, and a list of models.
Add MCP servers. Agents use the tools of these servers through the gateway, with the same virtual keys.
Add guardrails that examine requests before the model receives them, for example PII masking.
Look at the spend, the requests, and the tokens for all teams, keys, and models.
Find each request with its cost, duration, team, key, and model.
## Gateway or Python SDK[​](https://docs.litellm.ai/docs/simple_proxy#gateway-or-python-sdk "Direct link to Gateway or Python SDK")
Use the gateway if more than one application or person sends requests to LLMs. Also use the gateway if virtual keys, spend tracking, logs in one location, or guardrails are necessary. Use the [Python SDK](https://docs.litellm.ai/docs/python_sdk) if you send requests from one Python application and these controls are not necessary.
## Next steps[​](https://docs.litellm.ai/docs/simple_proxy#next-steps "Direct link to Next steps")
[Quickstart Start the gateway and send your first request in approximately five minutes.](https://docs.litellm.ai/docs/proxy/docker_quick_start)[Production deployment Deploy with Helm or Terraform, and size the database and Redis.](https://docs.litellm.ai/docs/proxy/deploy)[Client setup Connect Claude Code, Codex, and other clients to the gateway.](https://docs.litellm.ai/docs/proxy/client_setup/overview)[MCP Gateway Give agents access to MCP tools through one endpoint.](https://docs.litellm.ai/docs/mcp)[Agent Gateway Add and call A2A agents through the gateway.](https://docs.litellm.ai/docs/a2a)[Enterprise SSO, audit logs, and support for production deployments.](https://docs.litellm.ai/docs/enterprise)
  * [What the gateway does](https://docs.litellm.ai/docs/simple_proxy#what-the-gateway-does)
  * [Gateway or Python SDK](https://docs.litellm.ai/docs/simple_proxy#gateway-or-python-sdk)


LiteLLM Enterprise
SSO/SAML, audit logs, spend tracking, multi-team management, and guardrails, built for production.
[Learn more →](https://docs.litellm.ai/docs/enterprise)
