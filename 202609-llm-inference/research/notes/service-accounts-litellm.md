---
title: Service Accounts | liteLLM
id: service-accounts-litellm
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- virtual-keys
created: '2026-10-09T23:00:49.223242Z'
updated: '2026-10-10T14:52:17.158265Z'
source: https://docs.litellm.ai/docs/proxy/service_accounts
source_domain: docs.litellm.ai
fetched_at: '2026-10-09T23:00:49.201285Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'LiteLLM''s service-account (team-scoped virtual key) doc -- an OSS feature
  (no enterprise tag on this page). A service-account key always has team_id set and
  user_id null, and applies TEAM-level limits only (not per-member limits), which
  is the point: ''Prevent key from being deleted when user is deleted'' and ''Apply
  team limits, not team member limits to key.'' max_budget, tpm_limit, and rpm_limit
  can be set directly on the key or inherited from the team. The one enterprise-feature
  cross-reference on this page: team_member_key_duration (an enterprise feature controlling
  how long team-member keys live) explicitly does NOT apply to service-account keys.
  Supports an immutable service_account_id metadata field (once set, any attempt to
  overwrite or null it returns HTTP 400; omitting metadata on update safely preserves
  the existing value) for giving shared production/CI keys a stable human-readable
  identity distinct from the key hash itself. Can enforce that all service-account
  requests include a user param (service_account_settings.enforced_params) -- useful
  for per-end-customer attribution even on a shared service-account key.'
---

*Suggested by [[multi-tenant-architecture-with-litellm-litellm]] — linked as the virtual-key/service-account types doc from the multi-tenant architecture page*

[Skip to main content](https://docs.litellm.ai/docs/proxy/service_accounts#__docusaurus_skipToContent_fallback)
[**Decisions API is here.** Call Jev, Clef or any decision model through one /v1/decisions endpoint. →](https://docs.litellm.ai/docs/decisions)
On this page
Copy page
Use this if you want to create Virtual Keys that are not owned by a specific user but instead created for production projects
Why use a service account key?
  * Prevent key from being deleted when user is deleted.
  * Apply team limits, not team member limits to key.


## Service Account vs Regular Keys[​](https://docs.litellm.ai/docs/proxy/service_accounts#service-account-vs-regular-keys "Direct link to Service Account vs Regular Keys")  
| Feature  | Regular Key  | Service Account Key  |  
| --- | --- | --- |  
| `user_id`  | Optional  | Always `null`  |  
| `team_id`  | Optional  | Required  |  
| Applied limits  | User + Team limits  | Team limits only  |  
| Key deleted when user is deleted?  | Yes  | No — persists  |  
|  `service_account_id` in metadata  | Not set  | Immutable once set  |  
| `team_member_key_duration`  | Inherits  | Does not inherit  |  
## Budgets & Limits[​](https://docs.litellm.ai/docs/proxy/service_accounts#budgets--limits "Direct link to Budgets & Limits")
Service account keys apply budgets and rate limits at the **team level** , not per user or per key member.
  * Set `max_budget`, `tpm_limit`, `rpm_limit` on the key itself, or inherit them from the team.
  * `team_member_key_duration` (an enterprise feature that controls how long team-member keys last) does **not** apply to service account keys.


## Usage[​](https://docs.litellm.ai/docs/proxy/service_accounts#usage "Direct link to Usage")
Use the `/key/service-account/generate` endpoint to generate a service account key.

```
curl -L -X POST 'http://localhost:4000/key/service-account/generate' \-H "Authorization: Bearer $LITELLM_API_KEY" \-H 'Content-Type: application/json' \-d '{    "team_id": "my-unique-team"
```

###  `service_account_id` field[​](https://docs.litellm.ai/docs/proxy/service_accounts#service_account_id-field "Direct link to service_account_id-field")
You can optionally provide a `service_account_id` inside `metadata` to give the key a stable, human-readable identifier:

```
curl -L -X POST 'http://localhost:4000/key/service-account/generate' \-H "Authorization: Bearer $LITELLM_API_KEY" \-H 'Content-Type: application/json' \-d '{    "team_id": "my-unique-team",    "metadata": {        "service_account_id": "my-ci-pipeline"
```

**Immutability rules.** Once `service_account_id` is set, it cannot be changed:  
| Operation  | Result  |  
| --- | --- |  
| Overwrite with a different value  |  `400` error  |  
| Set to `null` explicitly  |  `400` error  |  
| Send `metadata: null` (would clear it)  |  `400` error  |  
| Omit `metadata` entirely on update  | Safe — existing value is preserved  |  
| Resend the same value  | Allowed (no-op)  |  
## Example - require `user` param for all service account requests[​](https://docs.litellm.ai/docs/proxy/service_accounts#example---require-user-param-for-all-service-account-requests "Direct link to example---require-user-param-for-all-service-account-requests")
### 1. Set settings for Service Accounts[​](https://docs.litellm.ai/docs/proxy/service_accounts#1-set-settings-for-service-accounts "Direct link to 1. Set settings for Service Accounts")
Set `service_account_settings` if you want to create settings that only apply to service account keys

```
general_settings:service_account_settings:enforced_params:["user"]# this means the "user" param is enforced for all requests made through any service account keys
```

### 2. Create Service Account Key on LiteLLM Proxy Admin UI[​](https://docs.litellm.ai/docs/proxy/service_accounts#2-create-service-account-key-on-litellm-proxy-admin-ui "Direct link to 2. Create Service Account Key on LiteLLM Proxy Admin UI")
### 3. Test Service Account Key[​](https://docs.litellm.ai/docs/proxy/service_accounts#3-test-service-account-key "Direct link to 3. Test Service Account Key")
  * Unsuccessful call
  * Successful call



```
curl --location 'http://localhost:4000/chat/completions' \    --header 'Authorization: Bearer <sk-your-service-account>' \    --header 'Content-Type: application/json' \    --data '{    "model": "gpt-5.6-luna",    "messages": [        "role": "user",        "content": "hello"
```

Expected Response

```
"error":{"message":"BadRequest please pass param=user in request body. This is a required param for service account","type":"bad_request_error","param":"user","code":"400"
```


```
curl --location 'http://localhost:4000/chat/completions' \    --header 'Authorization: Bearer <sk-your-service-account>' \    --header 'Content-Type: application/json' \    --data '{    "model": "gpt-5.6-luna",    "messages": [        "role": "user",        "content": "hello"    "user": "test-user"
```

Expected Response

```
"id":"chatcmpl-ad9595c7e3784a6783b469218d92d95c","choices":["finish_reason":"stop","index":0,"message":{"content":"\n\nHello there, how may I assist you today?","role":"assistant","tool_calls":null,"function_call":null"created":1677652288,"model":"gpt-5.6-luna","object":"chat.completion","system_fingerprint":"fp_44709d6fcb","usage":{"completion_tokens":12,"prompt_tokens":9,"total_tokens":21,"completion_tokens_details":null"service_tier":null
```

  * [Service Account vs Regular Keys](https://docs.litellm.ai/docs/proxy/service_accounts#service-account-vs-regular-keys)
  * [Usage](https://docs.litellm.ai/docs/proxy/service_accounts#usage)
    * [`service_account_id` field](https://docs.litellm.ai/docs/proxy/service_accounts#service_account_id-field)
  * [Example - require `user` param for all service account requests](https://docs.litellm.ai/docs/proxy/service_accounts#example---require-user-param-for-all-service-account-requests)
    * [1. Set settings for Service Accounts](https://docs.litellm.ai/docs/proxy/service_accounts#1-set-settings-for-service-accounts)
    * [2. Create Service Account Key on LiteLLM Proxy Admin UI](https://docs.litellm.ai/docs/proxy/service_accounts#2-create-service-account-key-on-litellm-proxy-admin-ui)
    * [3. Test Service Account Key](https://docs.litellm.ai/docs/proxy/service_accounts#3-test-service-account-key)


LiteLLM Enterprise
SSO/SAML, audit logs, spend tracking, multi-team management, and guardrails, built for production.
[Learn more →](https://docs.litellm.ai/docs/enterprise)
