---
title: Internal User Self-Serve | liteLLM
id: internal-user-self-serve-litellm
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- self-serve
created: '2026-10-09T23:00:59.205002Z'
updated: '2026-10-10T14:52:17.161742Z'
source: https://docs.litellm.ai/docs/proxy/self_serve
source_domain: docs.litellm.ai
fetched_at: '2026-10-09T23:00:59.182159Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'LiteLLM''s internal-user self-serve onboarding doc (OSS core flow): a proxy_admin
  creates an Internal User with role internal_user (grants self-service create/view/delete
  of own keys and own spend visibility, but not creation of other users), generates
  a time-boxed (7-day) invitation link via /invitation/new, and the user completes
  onboarding via email+password auth -- with LiteLLM Enterprise SSO as the named upgrade
  path for identity-provider login instead of email+password. Confirms the OSS role
  taxonomy: admin roles (proxy_admin, proxy_admin_viewer) vs internal-user roles (internal_user,
  internal_user_viewer), matching the RBAC doc.'
---

*Suggested by [[multi-tenant-architecture-with-litellm-litellm]] — linked as the internal-user self-serve onboarding doc from the multi-tenant architecture page*

[Skip to main content](https://docs.litellm.ai/docs/proxy/self_serve#__docusaurus_skipToContent_fallback)
[**Decisions API is here.** Call Jev, Clef or any decision model through one /v1/decisions endpoint. →](https://docs.litellm.ai/docs/decisions)
On this page
Copy page
## Allow users to create their own keys on [Proxy UI](https://docs.litellm.ai/docs/proxy/ui).[​](https://docs.litellm.ai/docs/proxy/self_serve#allow-users-to-create-their-own-keys-on-proxy-ui "Direct link to allow-users-to-create-their-own-keys-on-proxy-ui")
  1. Add user with permissions to a team on proxy


  * UI
  * API


Go to `Internal Users` -> `+New User`
Create a new Internal User on LiteLLM and assign them the role `internal_user`.

```
curl -X POST '<PROXY_BASE_URL>/user/new' \-H 'Authorization: Bearer <PROXY_MASTER_KEY>' \-H 'Content-Type: application/json' \-D '{    "user_email": "krrishdholakia@gmail.com",    "user_role": "internal_user" # 👈 THIS ALLOWS USER TO CREATE/VIEW/DELETE THEIR OWN KEYS + SEE THEIR SPEND
```

Expected Response

```
    "user_id": "e9d45c7c-b20b-4ff8-ae76-3f479a7b1d7d", 👈 USE IN STEP 2    "user_email": "<YOUR_USERS_EMAIL>",    "user_role": "internal_user",
```

Here's the available UI roles for a LiteLLM Internal User:
Admin Roles:
  * `proxy_admin`: admin over the platform
  * `proxy_admin_viewer`: can login, view all keys, view all spend. **Cannot** create/delete keys, add new users.


Internal User Roles:
  * `internal_user`: can login, view/create/delete their own keys, view their spend. **Cannot** add new users.
  * `internal_user_viewer`: can login, view their own keys, view their own spend. **Cannot** create/delete keys, add new users.


  1. Share invitation link with user


  * UI
  * API


Copy the invitation link with the user

```
curl -X POST '<PROXY_BASE_URL>/invitation/new' \-H 'Authorization: Bearer <PROXY_MASTER_KEY>' \-H 'Content-Type: application/json' \-D '{    "user_id": "e9d45c7c-b20b..." # 👈 USER ID FROM STEP 1
```

Expected Response

```
    "id": "a2f0918f-43b0-4770-a664-96ddd192966e",    "user_id": "e9d45c7c-b20b..",    "is_accepted": false,    "accepted_at": null,    "expires_at": "2024-06-13T00:02:16.454000Z", # 👈 VALID FOR 7d    "created_at": "2024-06-06T00:02:16.454000Z",    "created_by": "116544810872468347480",    "updated_at": "2024-06-06T00:02:16.454000Z",    "updated_by": "116544810872468347480"
```

Invitation Link:

```
http://0.0.0.0:4000/ui/onboarding?id=a2f0918f-43b0-4770-a664-96ddd192966e# <YOUR_PROXY_BASE_URL>/ui/onboarding?id=<id>
```

info
Use [Email Notifications](https://docs.litellm.ai/docs/proxy/email) to email users onboarding links
  1. User logs in via email + password auth


info
LiteLLM Enterprise: Enable [SSO login](https://docs.litellm.ai/docs/proxy/admin_ui_sso)
  1. User can now create their own keys


## Allow users to View Usage, Caching Analytics[​](https://docs.litellm.ai/docs/proxy/self_serve#allow-users-to-view-usage-caching-analytics "Direct link to Allow users to View Usage, Caching Analytics")
  1. Go to Internal Users -> +Invite User


Set their role to `Admin Viewer` - this means they can only view usage, caching analytics
  1. Share invitation link with user


  1. User logs in via email + password auth


  1. User can now view Usage, Caching Analytics


## Available Roles[​](https://docs.litellm.ai/docs/proxy/self_serve#available-roles "Direct link to Available Roles")
Here's the available UI roles for a LiteLLM Internal User:
**Admin Roles:**
  * `proxy_admin`: admin over the platform
  * `proxy_admin_viewer`: can login, view all keys, view all spend. **Cannot** create/delete keys, add new users.


**Internal User Roles:**
  * `internal_user`: can login, view/create/delete their own keys, view their spend. **Cannot** add new users.
  * `internal_user_viewer`: can login, view their own keys, view their own spend. **Cannot** create/delete keys, add new users.


**Team Roles:**
  * `admin`: can add new members to the team, can control Team Permissions, can add team-only models (useful for onboarding a team's finetuned models).
  * `user`: can login, view their own keys, view their own spend. **Cannot** create/delete keys (controllable via Team Permissions), add new users.


## Auto-add SSO users to teams[​](https://docs.litellm.ai/docs/proxy/self_serve#auto-add-sso-users-to-teams "Direct link to Auto-add SSO users to teams")
This walks through setting up sso auto-add for **Okta, Google SSO**
### Okta, Google SSO[​](https://docs.litellm.ai/docs/proxy/self_serve#okta-google-sso "Direct link to Okta, Google SSO")
  1. Specify the JWT field that contains the team ids, that the user belongs to.



```
general_settings:master_key: os.environ/LITELLM_MASTER_KEYlitellm_jwtauth:team_ids_jwt_field:"groups"# 👈 CAN BE ANY FIELD
```

This is assuming your SSO token looks like this. **If you need to inspect the JWT fields received from your SSO provider by LiteLLM, follow these instructions[here](https://docs.litellm.ai/docs/proxy/self_serve#debugging-sso-jwt-fields)**

```
  "groups": ["team_id_1", "team_id_2"]
```

  1. Create the teams on LiteLLM



```
curl -X POST '<PROXY_BASE_URL>/team/new' \-H 'Authorization: Bearer <PROXY_MASTER_KEY>' \-H 'Content-Type: application/json' \-D '{    "team_alias": "team_1",    "team_id": "team_id_1" # 👈 MUST BE THE SAME AS THE SSO GROUP ID
```

  1. Test the SSO flow


Here's a walkthrough of [how it works](https://www.loom.com/share/8959be458edf41fd85937452c29a33f3?sid=7ebd6d37-569a-4023-866e-e0cde67cb23e)
### Microsoft Entra ID SSO group assignment[​](https://docs.litellm.ai/docs/proxy/self_serve#microsoft-entra-id-sso-group-assignment "Direct link to Microsoft Entra ID SSO group assignment")
Follow this [tutorial for auto-adding sso users to teams with Microsoft Entra ID](https://docs.litellm.ai/docs/tutorials/msft_sso)
### Debugging SSO JWT fields[​](https://docs.litellm.ai/docs/proxy/self_serve#debugging-sso-jwt-fields "Direct link to Debugging SSO JWT fields")
## Advanced[​](https://docs.litellm.ai/docs/proxy/self_serve#advanced "Direct link to Advanced")
### Setting custom logout URLs[​](https://docs.litellm.ai/docs/proxy/self_serve#setting-custom-logout-urls "Direct link to Setting custom logout URLs")
Set `PROXY_LOGOUT_URL` in your .env if you want users to get redirected to a specific URL when they click logout

```
export PROXY_LOGOUT_URL="https://www.google.com"
```

### Set default max budget for internal users[​](https://docs.litellm.ai/docs/proxy/self_serve#set-default-max-budget-for-internal-users "Direct link to Set default max budget for internal users")
Automatically apply budget per internal user when they sign up. By default the table will be checked every 10 minutes, for users to reset. To modify this, [see this](https://docs.litellm.ai/docs/proxy/users#reset-budgets)

```
litellm_settings:max_internal_user_budget:10internal_user_budget_duration:"1mo"# reset every month
```

This sets a max budget of $10 USD for internal users when they sign up.
You can also manage these settings visually in the UI:
This budget only applies to personal keys created by that user - seen under `Default Team` on the UI.
This budget does not apply to keys created under non-default teams.
A user's personal `max_budget` is also the ceiling for the personal keys they create from the UI. A user with a $500 budget can create a personal key with a $100 `max_budget`, while a $600 request is rejected with `max_budget (600.0) cannot exceed the caller's own max_budget (500.0)`. A user with no personal budget falls back to the UI session budget ([`max_ui_session_budget`](https://docs.litellm.ai/docs/proxy/config_settings), default $1; `null` removes the ceiling) as the cap for an explicit key `max_budget`. Leaving `max_budget` off the key sets no key-level cap; spend on that key is still limited by the user's own budget. Changing a user's `max_budget` through `/user/update`, `/user/bulk_update`, or the Internal Users page takes effect on the instance that handled the update immediately. With Redis configured the change is broadcast to the other proxy instances as well; without Redis, other instances can keep enforcing the previous ceiling until their cached user record expires (about 60 seconds)
### Set max budget for teams[​](https://docs.litellm.ai/docs/proxy/self_serve#set-max-budget-for-teams "Direct link to Set max budget for teams")
### Default Team[​](https://docs.litellm.ai/docs/proxy/self_serve#default-team "Direct link to Default Team")
  * UI
  * YAML


Go to `Internal Users` -> `Default User Settings` and set the default team to the team you just created.
Let's also set the default models to `no-default-models`. This means a user can only create keys within a team.
info
Team must be created before setting it as the default team.

```
litellm_settings:default_internal_user_params:# Default Params used when a new user signs in Via SSOuser_role:"internal_user"# one of "internal_user", "internal_user_viewer", models:["no-default-models"]# Optional[List[str]], optional): models to be used by the userteams:# Optional[List[NewUserRequestTeam]], optional): teams to be used by the user-team_id:"team_id_1"# Required[str]: team_id to be used by the useruser_role:"user"# Optional[str], optional): Default role in the team. Values: "user" or "admin". Defaults to "user"
```

### Team Member Budgets[​](https://docs.litellm.ai/docs/proxy/self_serve#team-member-budgets "Direct link to Team Member Budgets")
Set a default max budget that applies to each member of a team.
You can do this when creating a new team, or by updating an existing team.
`team_member_budget` is a single team-wide default. Every member added without their own `max_budget_in_team` is linked to it, so changing it through `/team/update` (or the team's Default Budget field in the UI) applies to those members on their next request, not just to members added afterwards. A member given `max_budget_in_team` on `/team/member_add`, or later edited through [`/team/member_update`](https://docs.litellm.ai/docs/proxy/users#update-a-team-members-budget), gets their own budget and stops following the team default
  * UI
  * API



```
curl -X POST '<PROXY_BASE_URL>/team/new' \-H 'Authorization: Bearer <PROXY_MASTER_KEY>' \-H 'Content-Type: application/json' \-D '{    "team_alias": "team_1",    "budget_duration": "10d",    "team_member_budget": 10
```

info
Setting `team_member_budget` on an existing team links it to every member that has no budget yet, and the spend those members already accrued counts against it right away. See [Existing spend counts against a budget added later](https://docs.litellm.ai/docs/proxy/users#existing-spend-counts-against-a-budget-added-later) for how to unblock a member who is already over the new budget.
### Team Member Rate Limits[​](https://docs.litellm.ai/docs/proxy/self_serve#team-member-rate-limits "Direct link to Team Member Rate Limits")
Set a default tpm/rpm limit for an individual team member.
You can do this when creating a new team, or by updating an existing team.
  * UI
  * API



```
curl -X POST '<PROXY_BASE_URL>/team/new' \-H 'Authorization: Bearer <PROXY_MASTER_KEY>' \-H 'Content-Type: application/json' \-D '{    "team_alias": "team_1",    "team_member_rpm_limit": 100,    "team_member_tpm_limit": 1000
```

### Set default params for new teams[​](https://docs.litellm.ai/docs/proxy/self_serve#set-default-params-for-new-teams "Direct link to Set default params for new teams")
When you connect litellm to your SSO provider, litellm can auto-create teams. Use this to set the default `models`, `max_budget`, `budget_duration` for these auto-created teams.
**How it works**
  1. When litellm fetches `groups` from your SSO provider, it will check if the corresponding group_id exists as a `team_id` in litellm.
  2. If the team_id does not exist, litellm will auto-create a team with the default params you've set.
  3. If the team_id already exist, litellm will not apply any settings on the team.


**Usage**
Default Params for new teams

```
litellm_settings:default_team_params:# Applied to all /team/new calls (including SSO auto-created teams) when the field is omitted or null; an explicit budget_duration: null is honoredmax_budget:100# Optional[float]: $100 budget for the teambudget_duration:# Optional[str]: 30 days budget_duration for the teammodels:["gpt-5.6-luna"]# Optional[List[str]]: models for the team (only applied to SSO auto-created teams)tpm_limit:100000# Optional[int]: tokens per minute limitrpm_limit:1000# Optional[int]: requests per minute limitteam_member_permissions:# Optional[List[str]]: permissions granted to non-admin team members-"/team/daily/activity"# Allow members to view team usage-"/key/generate"# Allow members to generate API keys
```

info
These defaults fill any field that is missing or `null` in the `/team/new` request. `budget_duration` is the one exception: sending an explicit `"budget_duration": null` (the "Never resets" option in the UI create form) creates a team whose budget never resets, skipping the configured default.
### Restrict Users from creating personal keys[​](https://docs.litellm.ai/docs/proxy/self_serve#restrict-users-from-creating-personal-keys "Direct link to Restrict Users from creating personal keys")
This is useful if you only want users to create keys under a specific team.
This will also prevent users from using their session tokens on the test keys chat pane.
👉 [**See this**](https://docs.litellm.ai/docs/proxy/virtual_keys#restricting-key-generation)
##  **All Settings for Self Serve / SSO Flow**[​](https://docs.litellm.ai/docs/proxy/self_serve#all-settings-for-self-serve--sso-flow "Direct link to all-settings-for-self-serve--sso-flow")
All Settings for Self Serve / SSO Flow

```
litellm_settings:max_internal_user_budget:10# max budget for internal usersinternal_user_budget_duration:"1mo"# reset every monthdefault_internal_user_params:# Default Params used when a new user signs in Via SSOuser_role:"internal_user"# one of "internal_user", "internal_user_viewer", "proxy_admin", "proxy_admin_viewer". New SSO users not in litellm will be created as this usermax_budget:100# Optional[float], optional): $100 budget for a new SSO sign in userbudget_duration:# Optional[str], optional): 30 days budget_duration for a new SSO sign in usermodels:["gpt-5.6-luna"]# Optional[List[str]], optional): models to be used by a new SSO sign in userteams:# Optional[List[NewUserRequestTeam]], optional): teams to be used by the user-team_id:"team_id_1"# Required[str]: team_id to be used by the usermax_budget_in_team:100# Optional[float], optional): $100 budget for the team. Defaults to None.user_role:"user"# Optional[str], optional): "user" or "admin". Defaults to "user"default_team_params:# Applied to all /team/new calls (including SSO auto-created teams) when the field is omitted or null; an explicit budget_duration: null is honoredmax_budget:100# Optional[float]: $100 budget for the teambudget_duration:# Optional[str]: 30 days budget_duration for the teammodels:["gpt-5.6-luna"]# Optional[List[str]]: models for the team (only applied to SSO auto-created teams)tpm_limit:100000# Optional[int]: tokens per minute limitrpm_limit:1000# Optional[int]: requests per minute limitteam_member_permissions:# Optional[List[str]]: permissions granted to non-admin team members-"/team/daily/activity"upperbound_key_generate_params:# Upperbound for /key/generate requests when self-serve flow is onmax_budget:100# Optional[float], optional): upperbound of $100, for all /key/generate requestsbudget_duration:"10d"# Optional[str], optional): upperbound of 10 days for budget_duration valuesduration:"30d"# Optional[str], optional): upperbound of 30 days for all /key/generate requestsmax_parallel_requests:1000# (Optional[int], optional): Max number of requests that can be made in parallel. Defaults to None.tpm_limit:1000#(Optional[int], optional): Tpm limit. Defaults to None.rpm_limit:1000#(Optional[int], optional): Rpm limit. Defaults to None.key_generation_settings:# Restricts who can generate keys. [Further docs](./virtual_keys.md#restricting-key-generation)team_key_generation:allowed_team_member_roles:["admin"]personal_key_generation:# maps to 'Default Team' on UI allowed_user_roles:["proxy_admin"]
```

## Further Reading[​](https://docs.litellm.ai/docs/proxy/self_serve#further-reading "Direct link to Further Reading")
  * [Onboard Users for AI Exploration](https://docs.litellm.ai/docs/tutorials/default_team_self_serve)


  * [Allow users to create their own keys on Proxy UI.](https://docs.litellm.ai/docs/proxy/self_serve#allow-users-to-create-their-own-keys-on-proxy-ui)
  * [Allow users to View Usage, Caching Analytics](https://docs.litellm.ai/docs/proxy/self_serve#allow-users-to-view-usage-caching-analytics)
  * [Auto-add SSO users to teams](https://docs.litellm.ai/docs/proxy/self_serve#auto-add-sso-users-to-teams)
    * [Microsoft Entra ID SSO group assignment](https://docs.litellm.ai/docs/proxy/self_serve#microsoft-entra-id-sso-group-assignment)
    * [Debugging SSO JWT fields](https://docs.litellm.ai/docs/proxy/self_serve#debugging-sso-jwt-fields)
  * [Advanced](https://docs.litellm.ai/docs/proxy/self_serve#advanced)
    * [Setting custom logout URLs](https://docs.litellm.ai/docs/proxy/self_serve#setting-custom-logout-urls)
    * [Set default max budget for internal users](https://docs.litellm.ai/docs/proxy/self_serve#set-default-max-budget-for-internal-users)
    * [Set max budget for teams](https://docs.litellm.ai/docs/proxy/self_serve#set-max-budget-for-teams)
    * [Team Member Rate Limits](https://docs.litellm.ai/docs/proxy/self_serve#team-member-rate-limits)
    * [Set default params for new teams](https://docs.litellm.ai/docs/proxy/self_serve#set-default-params-for-new-teams)
    * [Restrict Users from creating personal keys](https://docs.litellm.ai/docs/proxy/self_serve#restrict-users-from-creating-personal-keys)
  * [**All Settings for Self Serve / SSO Flow**](https://docs.litellm.ai/docs/proxy/self_serve#all-settings-for-self-serve--sso-flow)


LiteLLM Enterprise
SSO/SAML, audit logs, spend tracking, multi-team management, and guardrails, built for production.
[Learn more →](https://docs.litellm.ai/docs/enterprise)
