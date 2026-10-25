---
title: Setting Team Budgets | liteLLM
id: setting-team-budgets-litellm
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- budget-hierarchy
created: '2026-10-09T23:00:15.412957Z'
updated: '2026-10-10T14:52:17.146527Z'
source: https://docs.litellm.ai/docs/proxy/team_budgets
source_domain: docs.litellm.ai
fetched_at: '2026-10-09T23:00:15.389049Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'LiteLLM''s hands-on config doc for team-level budgets, confirming the mechanism
  is real and OSS (uses the plain /team/new and /key/generate API, no enterprise tag).
  Budgets are set via max_budget (dollar ceiling) and budget_duration (reset cadence:
  1s/1m/1h/1d/30d), require a connected Postgres database as a prerequisite. On exceeding
  budget, the default response is HTTP 422 with a budget_exceeded error naming the
  team_id, current cost, and max budget; this is configurable to HTTP 429 via budget_exceeded_status_code.
  Enforcement is per-key: a key created with a team_id inherits that team''s budget.
  Exposes a Prometheus gauge litellm_remaining_team_budget_metric labeled by team_alias/team_id
  for dashboards. Also covers default budgets for JWT-auto-provisioned teams via default_team_settings
  in config.yaml (team_id_upsert).'
---

*Suggested by [[multi-tenant-architecture-with-litellm-litellm]] — linked as the team-budgets configuration doc from the multi-tenant architecture page*

[Skip to main content](https://docs.litellm.ai/docs/proxy/team_budgets#__docusaurus_skipToContent_fallback)
[**Decisions API is here.** Call Jev, Clef or any decision model through one /v1/decisions endpoint. →](https://docs.litellm.ai/docs/decisions)
On this page
Copy page
## Pre-Requisites[​](https://docs.litellm.ai/docs/proxy/team_budgets#pre-requisites "Direct link to Pre-Requisites")
  * You must set up a Postgres database (e.g. Supabase, Neon, etc.)


## Default Budget for Auto-Generated JWT Teams[​](https://docs.litellm.ai/docs/proxy/team_budgets#default-budget-for-auto-generated-jwt-teams "Direct link to Default Budget for Auto-Generated JWT Teams")
When using JWT authentication with `team_id_upsert: true`, you can automatically assign a default budget to any newly created team.
This is configured in `default_team_settings` in your `config.yaml`.
**Example:**

```
# in your config.yamllitellm_jwtauth:team_id_upsert:trueteam_id_jwt_field:"team_id"# ... other jwt settingslitellm_settings:default_team_settings:-team_id:"default-settings"max_budget:100.0
```

Track spend, set budgets for your Internal Team
## Setting Monthly Team Budgets[​](https://docs.litellm.ai/docs/proxy/team_budgets#setting-monthly-team-budgets "Direct link to Setting Monthly Team Budgets")
### 1. Create a team[​](https://docs.litellm.ai/docs/proxy/team_budgets#1-create-a-team "Direct link to 1. Create a team")
  * Set `max_budget=000000001` ($ value the team is allowed to spend)
  * Set `budget_duration="1d"` (How frequently the budget should update)


  * API
  * Admin UI


Create a new team and set `max_budget` and `budget_duration`

```
curl -X POST 'http://0.0.0.0:4000/team/new' \     -H "Authorization: Bearer $LITELLM_API_KEY" \     -H 'Content-Type: application/json' \     -d '{            "team_alias": "QA Prod Bot",             "max_budget": 0.000000001,             "budget_duration": "1d"
```

Response

```
 "team_alias": "QA Prod Bot", "team_id": "de35b29e-6ca8-4f47-b804-2b79d07aa99a", "max_budget": 0.0001, "budget_duration": "1d", "budget_reset_at": "2024-06-14T22:48:36.594000Z"
```

Possible values for `budget_duration`  
| `budget_duration`  | When Budget will reset  |  
| --- | --- |  
| `budget_duration="1s"`  | every 1 second  |  
| `budget_duration="1m"`  | every 1 min  |  
| `budget_duration="1h"`  | every 1 hour  |  
| `budget_duration="1d"`  | every 1 day  |  
| `budget_duration="30d"`  | every 1 month  |  
### 2. Create a key for the `team`[​](https://docs.litellm.ai/docs/proxy/team_budgets#2-create-a-key-for-the-team "Direct link to 2-create-a-key-for-the-team")
Create a key for Team=`QA Prod Bot` and `team_id="de35b29e-6ca8-4f47-b804-2b79d07aa99a"` from Step 1
  * API
  * Admin UI


💡 **The Budget for Team="QA Prod Bot" budget will apply to this team**

```
curl -X POST 'http://0.0.0.0:4000/key/generate' \     -H "Authorization: Bearer $LITELLM_API_KEY" \     -H 'Content-Type: application/json' \     -d '{"team_id": "de35b29e-6ca8-4f47-b804-2b79d07aa99a"}'
```

Response

```
{"team_id":"de35b29e-6ca8-4f47-b804-2b79d07aa99a", "key":"sk-5qtncoYjzRcxMM4bDRktNQ"}
```

### 3. Test It[​](https://docs.litellm.ai/docs/proxy/team_budgets#3-test-it "Direct link to 3. Test It")
Use the key from step 2 and run this Request twice
  * API
  * Admin UI



```
curl -X POST 'http://0.0.0.0:4000/chat/completions' \     -H 'Authorization: Bearer sk-mso-JSykEGri86KyOvgxBw' \     -H 'Content-Type: application/json' \     -d ' {           "model": "llama3",           "messages": [               "role": "user",               "content": "hi"
```

On the 2nd response - expect an HTTP 422 with the following error

```
 "error": {   "message": "Budget has been exceeded! Team=de35b29e-6ca8-4f47-b804-2b79d07aa99a Current cost: 3.5e-06, Max budget: 1e-09",   "type": "budget_exceeded",   "param": null,   "code": "422"
```

To return 429 instead, set `budget_exceeded_status_code: 429` under `litellm_settings` in your config.yaml
## Advanced[​](https://docs.litellm.ai/docs/proxy/team_budgets#advanced "Direct link to Advanced")
### Prometheus metrics for `remaining_budget`[​](https://docs.litellm.ai/docs/proxy/team_budgets#prometheus-metrics-for-remaining_budget "Direct link to prometheus-metrics-for-remaining_budget")
[More info about Prometheus metrics here](https://docs.litellm.ai/docs/proxy/prometheus)
You'll need the following in your proxy config.yaml

```
litellm_settings:success_callback:["prometheus"]failure_callback:["prometheus"]
```

Expect to see this metric on prometheus to track the Remaining Budget for the team

```
litellm_remaining_team_budget_metric{team_alias="QA Prod Bot",team_id="de35b29e-6ca8-4f47-b804-2b79d07aa99a"} 9.699999999999992e-06
```

## See Also[​](https://docs.litellm.ai/docs/proxy/team_budgets#see-also "Direct link to See Also")
  * [Per-model TPM/RPM for teams](https://docs.litellm.ai/docs/proxy/users#set-rate-limits) - Set rate limits per model for all keys in a team


  * [Default Budget for Auto-Generated JWT Teams](https://docs.litellm.ai/docs/proxy/team_budgets#default-budget-for-auto-generated-jwt-teams)
  * [Setting Monthly Team Budgets](https://docs.litellm.ai/docs/proxy/team_budgets#setting-monthly-team-budgets)
    * [2. Create a key for the `team`](https://docs.litellm.ai/docs/proxy/team_budgets#2-create-a-key-for-the-team)
  * [Advanced](https://docs.litellm.ai/docs/proxy/team_budgets#advanced)
    * [Prometheus metrics for `remaining_budget`](https://docs.litellm.ai/docs/proxy/team_budgets#prometheus-metrics-for-remaining_budget)


LiteLLM Enterprise
SSO/SAML, audit logs, spend tracking, multi-team management, and guardrails, built for production.
[Learn more →](https://docs.litellm.ai/docs/enterprise)
