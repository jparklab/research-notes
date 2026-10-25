---
title: Role-based Access Controls (RBAC) | liteLLM
id: role-based-access-controls-rbac-litellm
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- rbac
- oss-boundary
- virtual-keys
created: '2026-10-09T23:00:42.550810Z'
updated: '2026-10-10T14:52:17.156763Z'
source: https://docs.litellm.ai/docs/proxy/access_control
source_domain: docs.litellm.ai
fetched_at: '2026-10-09T23:00:42.520764Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'LiteLLM''s RBAC doc, and a more precise statement of the OSS/Enterprise
  boundary than the multi-tenant-architecture page: Global Proxy Roles (proxy_admin,
  proxy_admin_viewer, internal_user, deprecated internal_user_viewer) are platform-wide
  and OSS. But Organization/Team Specific Roles -- org_admin AND team_admin -- are
  both explicitly flagged ''(Premium Feature)'' here, meaning delegated TEAM administration
  itself, not just the Organization layer, is gated behind Enterprise (this sharpens/partially
  contradicts the multi_tenant_architecture page''s framing that ''Teams alone can
  serve as your top-level tenant boundary'' in OSS -- OSS teams exist and can hold
  budgets, but a non-proxy-admin cannot administer even a single team''s membership/keys/budgets
  without the premium team_admin role). Documents three virtual-key scoping patterns
  with concrete lifecycle rules: user-only keys (deleted when user is deleted, tracked
  to the user -- for individual developer use), team/service-account keys (team_id
  but no user_id, persist across team member turnover, tracked to the team only --
  for production apps/CI pipelines), and user+team keys (both IDs, tracked to both,
  deleted with the user -- for individual accountability inside a team budget). Usage-dashboard
  visibility is scoped to four levels (personal/team/organization/global) gated by
  role; notes the Admin UI has no custom-dashboard capability, so per-user-within-team
  usage requires querying the management API directly or exporting via Prometheus/OpenTelemetry
  to Grafana.'
---

*Suggested by [[multi-tenant-architecture-with-litellm-litellm]] — linked as the RBAC/role-matrix doc from the multi-tenant architecture page*

[Skip to main content](https://docs.litellm.ai/docs/proxy/access_control#__docusaurus_skipToContent_fallback)
[**Decisions API is here.** Call Jev, Clef or any decision model through one /v1/decisions endpoint. →](https://docs.litellm.ai/docs/decisions)
On this page
Copy page
Role-based access control (RBAC) is based on Organizations, Teams and Internal User Roles
### Video Walkthrough[​](https://docs.litellm.ai/docs/proxy/access_control#video-walkthrough "Direct link to Video Walkthrough")
Organizationenterprise
Team: productionbudget + models
Users
alicebob
Keysuser keys and team service accounts
sk-alicesk-svc-prod
Team: experimentsbudget + models
Users
carol
Keys
sk-carol
Every request's spend is attributed to its key, user, team, and organization at once, and budgets are enforced at each level; a request is blocked when any level on its path is over budget. Teams are the top-level boundary in open source; Organizations add the outer layer and are an enterprise feature.
  * `Organizations` are the top-level entities that contain Teams.
  * `Team` - A Team is a collection of multiple `Internal Users`
  * `Internal Users` - users that can create keys, make LLM API calls, view usage on LiteLLM. Users can be on multiple teams.
  * `Virtual Keys` - Keys are used for authentication to the LiteLLM API. Each key can optionally be associated with a `user_id`, a `team_id`, or both: 
    * **User-only key** : Has a `user_id` but no `team_id`. Tracked individually, deleted when the user is deleted.
    * **Team key (Service Account)** : Has a `team_id` but no `user_id`. Shared by the team, not deleted when users are removed. [Learn more about service account keys](https://docs.litellm.ai/docs/proxy/virtual_keys#service-account-keys).
    * **User + Team key** : Has both `user_id` and `team_id`. Belongs to a specific user within a team context.


### When to Use Each Key Type[​](https://docs.litellm.ai/docs/proxy/access_control#when-to-use-each-key-type "Direct link to When to Use Each Key Type")  
| Key Type  | Use Case  | Spend Tracking  | Lifecycle  |  
| --- | --- | --- | --- |  
| **User-only**  | Personal API keys for individual developers  | Tracked to the user  | Deleted when user is deleted  |  
| **Team (Service Account)**  | Production apps, CI/CD pipelines, shared services  | Tracked to the team only  | Persists even when team members leave  |  
| **User + Team**  | User working within a team context  | Tracked to both user and team  | Deleted when user is deleted  |  
**Example scenarios:**
  * Use **user-only keys** for developers testing locally
  * Use **team service account keys** for your production application that shouldn't break when employees leave
  * Use **user + team keys** when you want individual accountability within a team budget


## User Roles[​](https://docs.litellm.ai/docs/proxy/access_control#user-roles "Direct link to User Roles")
LiteLLM has two types of roles:
  1. **Global Proxy Roles** - Platform-wide roles that apply across all organizations and teams
  2. **Organization/Team Specific Roles** - Roles scoped to specific organizations or teams (**Premium Feature**)


### Global Proxy Roles[​](https://docs.litellm.ai/docs/proxy/access_control#global-proxy-roles "Direct link to Global Proxy Roles")  
| Role Name  | Permissions  |  
| --- | --- |  
| `proxy_admin`  | Admin over the entire platform. Full control over all organizations, teams, and users  |  
| `proxy_admin_viewer`  | Can login, view all keys, view all spend across the platform. **Cannot** create keys/delete keys/add new users  |  
| `internal_user`  | Can login, view/create (when allowed by team-specific permissions)/delete their own keys, view their spend. **Cannot** add new users  |  
| `internal_user_viewer`  | ⚠️ **DEPRECATED** - Use team/org specific roles instead. Can login, view their own keys, view their own spend. **Cannot** create/delete keys, add new users  |  
### Organization/Team Specific Roles[​](https://docs.litellm.ai/docs/proxy/access_control#organizationteam-specific-roles "Direct link to Organization/Team Specific Roles")  
| Role Name  | Permissions  |  
| --- | --- |  
| `org_admin`  | Admin over a specific organization. Can create teams and users within their organization ✨ **Premium Feature**  |  
| `team_admin`  | Admin over a specific team. Can manage team members, update team member permissions, and create keys for their team. ✨ **Premium Feature**  |  
## Usage dashboard visibility[​](https://docs.litellm.ai/docs/proxy/access_control#usage-dashboard-visibility "Direct link to Usage dashboard visibility")
The Usage page shows different data depending on the selected view and the signed-in user's role:  
| View  | What it shows  |  
| --- | --- |  
| Personal usage  | The signed-in user's aggregate usage. If the user belongs to multiple teams, this view does not split their personal usage by team.  |  
| Team usage  | The complete usage for the selected team, not only the signed-in user's contribution to that team.  |  
| Organization usage  | Aggregate usage for an organization, when the user's organization role permits access.  |  
| Global usage  | Platform-wide usage for proxy admins and proxy admin viewers.  |  
The LiteLLM Admin UI does not host custom dashboards. For a custom view such as per-user usage within each team, query the spend data through the management API or export telemetry to an external system. See [Prometheus metrics](https://docs.litellm.ai/docs/proxy/prometheus) and [OpenTelemetry](https://docs.litellm.ai/docs/observability/opentelemetry_v2) for Grafana-compatible exports.
## What Can Each Role Do?[​](https://docs.litellm.ai/docs/proxy/access_control#what-can-each-role-do "Direct link to What Can Each Role Do?")
Here's what each role can actually do. Think of it like levels of access.
## Global Proxy Roles[​](https://docs.litellm.ai/docs/proxy/access_control#global-proxy-roles-1 "Direct link to Global Proxy Roles")
These roles apply across the entire LiteLLM platform, regardless of organization or team boundaries.
### Proxy Admin - Full Access[​](https://docs.litellm.ai/docs/proxy/access_control#proxy-admin---full-access "Direct link to Proxy Admin - Full Access")
The proxy admin controls everything. They're like the owner of the whole platform.
**What they can do:**
  * Create and manage all organizations
  * Create and manage all teams (across all organizations)
  * Create and manage all users
  * View all spend and usage across the platform
  * Create and delete keys for anyone
  * Update team budgets, rate limits, and models
  * Manage team members and assign roles


**Who should be a proxy admin:** Only the people running the LiteLLM instance.
### Proxy Admin Viewer - Platform-Wide Read Access[​](https://docs.litellm.ai/docs/proxy/access_control#proxy-admin-viewer---platform-wide-read-access "Direct link to Proxy Admin Viewer - Platform-Wide Read Access")
The proxy admin viewer can see everything across the platform but cannot make changes.
**What they can do:**
  * View all organizations, teams, and users
  * View all spend and usage across the platform
  * View all API keys
  * Login to the admin dashboard


**What they cannot do:**
  * Create or delete keys
  * Add or remove users
  * Modify budgets, rate limits, or settings
  * Make any changes to the platform


**Who should be a proxy admin viewer:** Finance teams, auditors, or stakeholders who need platform-wide visibility without modification rights.
### Internal User[​](https://docs.litellm.ai/docs/proxy/access_control#internal-user "Direct link to Internal User")
An internal user can create API keys (when allowed by team-specific permissions) and make calls. They see their own stuff only. They can become a team admin or org admin if they are assigned the respective roles.
**What they can do:**
  * Create API keys for themselves
  * Delete their own API keys
  * View their own spend and usage
  * Make API calls using their keys


**Who should be an internal user:** Anyone who needs UI access for team/org specific operations **OR** for developers you plan to give multiple keys to.
### Internal User Viewer - Read-Only Access[​](https://docs.litellm.ai/docs/proxy/access_control#internal-user-viewer---read-only-access "Direct link to Internal User Viewer - Read-Only Access")
DEPRECATED
This role is deprecated in favor of team/org specific roles. Use `org_admin` or `team_admin` roles for better granular control over user permissions within organizations and teams.
An internal user viewer can view their own information but cannot create or delete keys.
**What they can do:**
  * View their own API keys
  * View their own spend and usage
  * Login to see their dashboard


**What they cannot do:**
  * Create or delete API keys
  * Make changes to any settings
  * Create teams or add users
  * View other people's information


**Who should be an internal user viewer (deprecated):** Consider using team/org specific roles instead for better access control.
## Organization/Team Specific Roles[​](https://docs.litellm.ai/docs/proxy/access_control#organizationteam-specific-roles-1 "Direct link to Organization/Team Specific Roles")
Enterprise feature
This feature requires a LiteLLM Enterprise license. Start a [free 30-day trial](https://www.litellm.ai/enterprise#trial) or [book a demo](https://enterprise.litellm.ai/demo). [See what Enterprise includes](https://docs.litellm.ai/docs/enterprise).
These roles are scoped to specific organizations or teams. Users with these roles can only manage resources within their assigned organization or team.
### Org Admin - Organization Level Access[​](https://docs.litellm.ai/docs/proxy/access_control#org-admin---organization-level-access "Direct link to Org Admin - Organization Level Access")
An org admin manages one or more organizations. They can create teams within their organization but can't touch other organizations.
**What they can do:**
  * Create teams within their organization
  * Add users to teams in their organization
  * View spend for their organization
  * Create keys for users in their organization


**What they cannot do:**
  * Create or manage other organizations
  * Modify org budgets / rate limits
  * Modify org allowed models (e.g. adding a proxy-level model to the org)


**Who should be an org admin:** Department leads or managers who need to manage multiple teams.
### Team Admin - Team Level Access[​](https://docs.litellm.ai/docs/proxy/access_control#team-admin---team-level-access "Direct link to Team Admin - Team Level Access")
✨ **This is a Premium Feature**
A team admin manages a specific team. They're like a team lead who can add people, update settings, but only for their team.
**What they can do:**
  * Add or remove team members from their team
  * Update team members' budgets and rate limits within the team
  * Change team rate limits (TPM/RPM) and the team's `max_budget`, limited to the fields a proxy admin enabled under [team admin editable fields](https://docs.litellm.ai/docs/proxy/access_control#choosing-what-team-admins-can-edit)
  * Create and delete keys for team members
  * Onboard a [team-BYOK](https://docs.litellm.ai/docs/proxy/team_model_add) model to LiteLLM (e.g. onboarding a team's finetuned model)
  * Configure [team member permissions](https://docs.litellm.ai/docs/proxy/access_control#team-member-permissions) to control what regular team members can do


**What they cannot do:**
  * Create new teams
  * Raise the team's `max_budget` unless a proxy admin enabled `raise_max_budget`, or remove the budget cap (`max_budget: null`), which only a proxy admin can do
  * Add/remove global proxy models to their team


Team budget raises
On `/team/update`, a team admin can only change the fields a proxy admin enabled under [team admin editable fields](https://docs.litellm.ai/docs/proxy/access_control#choosing-what-team-admins-can-edit). With `max_budget` enabled they may keep or lower it; raising it also needs the `raise_max_budget` entry and is capped by the organization's `max_budget` on org-scoped teams. Clearing the cap stays with proxy admins.
**Who should be a team admin:** Team leads who need to manage their team's API access without bothering IT.
How to create a team admin
You need to be a LiteLLM Enterprise user to assign team admins. [Get a 30 day trial here](https://www.litellm.ai/#trial).

```
curl -X POST 'http://0.0.0.0:4000/team/member_add' \    -H "Authorization: Bearer $LITELLM_API_KEY" \    -H 'Content-Type: application/json' \    -d '{"team_id": "team-123", "member": {"role": "admin", "user_id": "user@company.com"}}'
```

#### Choosing what team admins can edit[​](https://docs.litellm.ai/docs/proxy/access_control#choosing-what-team-admins-can-edit "Direct link to Choosing what team admins can edit")
By default team admins cannot change team settings at all: `/team/update` from a team admin returns 403 and names the setting a proxy admin needs to enable. A proxy admin picks the fields in the Admin UI under Settings > UI > "Team admin editable fields", or sets `team_admin_editable_team_fields` through the API. The same key also works under `general_settings` in the proxy config yaml; a value saved through the UI or API takes precedence over the yaml. The list applies to every team admin on the proxy, only for the teams they administer. Proxy admins and org admins are not affected by it.

```
curl -X PATCH 'http://localhost:4000/update/ui_settings' \  -H "Authorization: Bearer $LITELLM_API_KEY" \  -H 'Content-Type: application/json' \  -d '{"team_admin_editable_team_fields": ["tpm_limit", "rpm_limit", "max_budget", "raise_max_budget"]}'
```

Read the current value back with `GET /get/ui_settings`.  
| Entry  | What a team admin may do on the teams they administer  |  
| --- | --- |  
| `tpm_limit`  | Change the team's tokens-per-minute limit on `/team/update`  |  
| `rpm_limit`  | Change the team's requests-per-minute limit on `/team/update`  |  
| `max_budget`  | Keep or lower the team's `max_budget` on `/team/update`. Raising or removing it still returns 403  |  
| `raise_max_budget`  | Also raise `max_budget`. Only valid together with `max_budget`: the API returns 400 if it is enabled alone, and the UI checkbox sits under Max Budget and stays disabled until Max Budget is checked. Removing the budget (`max_budget: null`) always stays with proxy admins  |  
| `projects`  | Create and update projects for those teams (`/project/new`, `/project/update`)  |  
| `member_key_budgets`  | Update budget fields on keys owned by other members of those teams (`/key/update`)  |  
A raise is capped by the organization's `max_budget` when the team belongs to an organization that has one: going above it returns 400 with `Team max_budget (...) exceeds organization's max_budget (...)`. Teams outside an organization, or in an organization without a budget, have no ceiling. Org-scoped teams must also stay within the organization's budget and TPM/RPM limits no matter who edits them.
A team admin can check what they may edit on `/team/info`: `team_info.caller_edit_access` is `{"kind": "team_admin", "editable_fields": ["max_budget", "rpm_limit"], "may_raise_max_budget": true}` for an enabled team admin, `{"kind": "team_admin_disabled"}` when nothing is enabled, `{"kind": "unrestricted"}` for proxy and org admins, and `{"kind": "none"}` for everyone else.
## Team Member Permissions[​](https://docs.litellm.ai/docs/proxy/access_control#team-member-permissions "Direct link to Team Member Permissions")
✨ **This is a Premium Feature**
Team member permissions allow you to control what regular team members (with role=`user`) can do with API keys in their team. By default, team members can only view key information, but you can grant them additional permissions to create, update, or delete keys.
### How It Works[​](https://docs.litellm.ai/docs/proxy/access_control#how-it-works "Direct link to How It Works")
  * **Applies to** : Team members with role=`user` (not team admins or org admins)
  * **Scope** : Permissions only apply to keys belonging to their team
  * **Configuration** : Set at the team level using `team_member_permissions`
  * **Override** : Team admins and org admins always have full permissions regardless of these settings


### Available Permissions[​](https://docs.litellm.ai/docs/proxy/access_control#available-permissions "Direct link to Available Permissions")  
| Permission  | Method  | Description  |  
| --- | --- | --- |  
| `/key/info`  | GET  | View information about virtual keys in the team  |  
| `/key/health`  | GET  | Check health status of virtual keys in the team  |  
| `/key/list`  | GET  | List all virtual keys belonging to the team  |  
| `/key/generate`  | POST  | Create new virtual keys for the team  |  
| `/key/service-account/generate`  | POST  | Create service account keys (not tied to a specific user) for the team  |  
| `/key/update`  | POST  | Modify existing virtual keys in the team  |  
| `/key/delete`  | POST  | Delete virtual keys belonging to the team  |  
| `/key/regenerate`  | POST  | Regenerate virtual keys in the team  |  
| `/key/block`  | POST  | Block virtual keys in the team  |  
| `/key/unblock`  | POST  | Unblock virtual keys in the team  |  
| `/team/daily/activity`  | GET  | View the whole team's usage, not only the member's own  |  
| `/spend/logs`  | GET  | View the whole team's spend logs and its agent traces in [Lens](https://docs.litellm.ai/docs/proxy/lens/api#trace-access)  |  
### Default Permissions[​](https://docs.litellm.ai/docs/proxy/access_control#default-permissions "Direct link to Default Permissions")
By default, team members can only:
  * `/key/info` - View key information
  * `/key/health` - Check key health


### Common Permission Scenarios[​](https://docs.litellm.ai/docs/proxy/access_control#common-permission-scenarios "Direct link to Common Permission Scenarios")
**Read-only access** (default):

```
["/key/info","/key/health"]
```

**Allow key creation but not deletion** :

```
["/key/info","/key/health","/key/generate","/key/update"]
```

**Full key management** :

```
["/key/info","/key/health","/key/generate","/key/update","/key/delete","/key/regenerate","/key/block","/key/unblock","/key/list"]
```

### How to Configure Team Member Permissions[​](https://docs.litellm.ai/docs/proxy/access_control#how-to-configure-team-member-permissions "Direct link to How to Configure Team Member Permissions")
#### View Current Permissions[​](https://docs.litellm.ai/docs/proxy/access_control#view-current-permissions "Direct link to View Current Permissions")

```
curl --location 'http://0.0.0.0:4000/team/permissions_list?team_id=team-123' \    --header "Authorization: Bearer $LITELLM_API_KEY"
```

Expected Response:

```
"team_id":"team-123","team_member_permissions":["/key/info","/key/health"],"all_available_permissions":["/key/generate","/key/update","/key/delete", ...]
```

#### Update Team Member Permissions[​](https://docs.litellm.ai/docs/proxy/access_control#update-team-member-permissions "Direct link to Update Team Member Permissions")

```
curl --location 'http://0.0.0.0:4000/team/update' \    --header "Authorization: Bearer $LITELLM_API_KEY" \    --header 'Content-Type: application/json' \    --data '{        "team_id": "team-123",        "team_member_permissions": ["/key/info", "/key/health", "/key/generate", "/key/update"]
```

This allows team members to:
  * View key information
  * Create new keys
  * Update existing keys
  * But NOT delete keys


### Who Can Configure These Permissions?[​](https://docs.litellm.ai/docs/proxy/access_control#who-can-configure-these-permissions "Direct link to Who Can Configure These Permissions?")
  * **Proxy Admin** : Can configure permissions for any team
  * **Org Admin** : Can configure permissions for teams in their organization
  * **Team Admin** : Can configure permissions for their own team


## Quick Comparison[​](https://docs.litellm.ai/docs/proxy/access_control#quick-comparison "Direct link to Quick Comparison")
Here's the quick version:
### Global Proxy Roles[​](https://docs.litellm.ai/docs/proxy/access_control#global-proxy-roles-2 "Direct link to Global Proxy Roles")  
| Action  | Proxy Admin  | Proxy Admin Viewer  | Internal User  | Internal User Viewer ⚠️ (Deprecated)  |  
| --- | --- | --- | --- | --- |  
| Create organizations  | ✅  | ❌  | ❌  | ❌  |  
| Create teams  | ✅  | ❌  | ❌  | ❌  |  
| Manage all teams  | ✅  | ❌  | ❌  | ❌  |  
| Create/delete any keys  | ✅  | ❌  | ❌  | ❌  |  
| Create/delete own keys  | ✅  | ❌  | ✅  | ❌  |  
| View all platform spend  | ✅  | ✅  | ❌  | ❌  |  
| View own spend  | ✅  | ✅  | ✅  | ✅  |  
| View all keys  | ✅  | ✅  | ❌  | ❌  |  
| View own keys  | ✅  | ✅  | ✅  | ✅  |  
| Add/remove users  | ✅  | ❌  | ❌  | ❌  |  
> **Note:** The `internal_user_viewer` role is deprecated. Use team/org specific roles for better granular access control.
### Organization/Team Specific Roles[​](https://docs.litellm.ai/docs/proxy/access_control#organizationteam-specific-roles-2 "Direct link to Organization/Team Specific Roles")  
| Action  | Org Admin  | Team Admin  |  
| --- | --- | --- |  
| Create teams (in their org)  | ✅  | ❌  |  
| Manage teams in their org  | ✅  | ❌  |  
| Manage their specific team  | ✅  | ✅  |  
| Add/remove team members  | ✅ (in their org)  | ✅ (their team only)  |  
| Keep / lower team `max_budget`  | ✅ (in their org)  | ✅ (their team only, when `max_budget` is [enabled](https://docs.litellm.ai/docs/proxy/access_control#choosing-what-team-admins-can-edit))  |  
| Raise team `max_budget`  | ✅ within org limits (org-scoped); proxy admin for standalone  | ✅ (their team only, when `raise_max_budget` is enabled; capped by the org budget)  |  
| Update team rate limits  | ✅ (in their org)  | ✅ (their team only, when `tpm_limit` / `rpm_limit` are enabled)  |  
| Create keys for team members  | ✅ (in their org)  | ✅ (their team only)  |  
| View organization spend  | ✅ (their org)  | ❌  |  
| View team spend  | ✅ (in their org)  | ✅ (their team)  |  
| Create organizations  | ❌  | ❌  |  
| View all platform spend  | ❌  | ❌  |  
## Onboarding Organizations[​](https://docs.litellm.ai/docs/proxy/access_control#onboarding-organizations "Direct link to Onboarding Organizations")
✨ **This is a Premium Feature**
### 1. Creating a new Organization[​](https://docs.litellm.ai/docs/proxy/access_control#1-creating-a-new-organization "Direct link to 1. Creating a new Organization")
Any user with role=`proxy_admin` can create a new organization
**Usage**
[**API Reference for /organization/new**](https://docs.litellm.ai/api-reference/#/organization%20management/new_organization_organization_new_post)

```
curl --location 'http://0.0.0.0:4000/organization/new' \    --header "Authorization: Bearer $LITELLM_API_KEY" \    --header 'Content-Type: application/json' \    --data '{        "organization_alias": "marketing_department",        "models": ["gpt-5.6-terra"],        "max_budget": 20
```

Expected Response

```
"organization_id":"ad15e8ca-12ae-46f4-8659-d02debef1b23","organization_alias":"marketing_department","budget_id":"98754244-3a9c-4b31-b2e9-c63edc8fd7eb","metadata":{},"models":["gpt-5.6-terra""created_by":"109010464461339474872","updated_by":"109010464461339474872","created_at":"2024-10-08T18:30:24.637000Z","updated_at":"2024-10-08T18:30:24.637000Z"
```

### 2. Adding an `org_admin` to an Organization[​](https://docs.litellm.ai/docs/proxy/access_control#2-adding-an-org_admin-to-an-organization "Direct link to 2-adding-an-org_admin-to-an-organization")
Create a user (ishaan@berri.ai) as an `org_admin` for the `marketing_department` Organization (from [step 1](https://docs.litellm.ai/docs/proxy/access_control#1-creating-a-new-organization))
Users with the following roles can call `/organization/member_add`
  * `proxy_admin`
  * `org_admin` only within their own organization



```
curl -X POST 'http://0.0.0.0:4000/organization/member_add' \    -H "Authorization: Bearer $LITELLM_API_KEY" \    -H 'Content-Type: application/json' \    -d '{"organization_id": "ad15e8ca-12ae-46f4-8659-d02debef1b23", "member": {"role": "org_admin", "user_id": "ishaan@berri.ai"}}'
```

Now a user with user_id = `ishaan@berri.ai` and role = `org_admin` has been created in the `marketing_department` Organization
Create a Virtual Key for user_id = `ishaan@berri.ai`. The User can then use the Virtual key for their Organization Admin Operations

```
curl --location 'http://0.0.0.0:4000/key/generate' \        --header "Authorization: Bearer $LITELLM_API_KEY" \        --header 'Content-Type: application/json' \        --data '{            "user_id": "ishaan@berri.ai"
```

Expected Response

```
"models":[],"user_id":"ishaan@berri.ai","key":"sk-7shH8TGMAofR4zQpAAo6kQ","key_name":"sk-...o6kQ"
```

### 3. `Organization Admin` - Create a Team[​](https://docs.litellm.ai/docs/proxy/access_control#3-organization-admin---create-a-team "Direct link to 3-organization-admin---create-a-team")
The organization admin will use the virtual key created in [step 2](https://docs.litellm.ai/docs/proxy/access_control#2-adding-an-org_admin-to-an-organization) to create a `Team` within the `marketing_department` Organization

```
curl --location 'http://0.0.0.0:4000/team/new' \    --header 'Authorization: Bearer sk-7shH8TGMAofR4zQpAAo6kQ' \    --header 'Content-Type: application/json' \    --data '{        "team_alias": "engineering_team",        "organization_id": "ad15e8ca-12ae-46f4-8659-d02debef1b23"
```

This will create the team `engineering_team` within the `marketing_department` Organization
Expected Response

```
"team_alias":"engineering_team","team_id":"01044ee8-441b-45f4-be7d-c70e002722d8","organization_id":"ad15e8ca-12ae-46f4-8659-d02debef1b23"
```

### 4. `Organization Admin` - Add a Team Admin[​](https://docs.litellm.ai/docs/proxy/access_control#4-organization-admin---add-a-team-admin "Direct link to 4-organization-admin---add-a-team-admin")
✨ **This is a Premium Feature**
The organization admin can now add a team admin who will manage the `engineering_team`.
  * We assign role=`admin` to make them a team admin for this specific team
  * `team_id` is from [step 3](https://docs.litellm.ai/docs/proxy/access_control#3-organization-admin---create-a-team)



```
curl -X POST 'http://0.0.0.0:4000/team/member_add' \    -H 'Authorization: Bearer sk-7shH8TGMAofR4zQpAAo6kQ' \    -H 'Content-Type: application/json' \    -d '{"team_id": "01044ee8-441b-45f4-be7d-c70e002722d8", "member": {"role": "admin", "user_id": "john@company.com"}}'
```

Now `john@company.com` is a team admin. They can manage the `engineering_team` (add members, create keys, and change whichever team settings a proxy admin enabled under [team admin editable fields](https://docs.litellm.ai/docs/proxy/access_control#choosing-what-team-admins-can-edit)) but they can't touch other teams.
Create a Virtual Key for the team admin:

```
curl --location 'http://0.0.0.0:4000/key/generate' \    --header 'Authorization: Bearer sk-7shH8TGMAofR4zQpAAo6kQ' \    --header 'Content-Type: application/json' \    --data '{"user_id": "john@company.com"}'
```

Expected Response:

```
"models":[],"user_id":"john@company.com","key":"sk-TeamAdminKey123","key_name":"sk-...Key123"
```

### 5. `Team Admin` - Add Team Members[​](https://docs.litellm.ai/docs/proxy/access_control#5-team-admin---add-team-members "Direct link to 5-team-admin---add-team-members")
Now the team admin can use their key to add team members without needing to ask the org admin.

```
curl -X POST 'http://0.0.0.0:4000/team/member_add' \    -H 'Authorization: Bearer sk-TeamAdminKey123' \    -H 'Content-Type: application/json' \    -d '{"team_id": "01044ee8-441b-45f4-be7d-c70e002722d8", "member": {"role": "user", "user_id": "krrish@berri.ai"}}'
```

The team admin can also create keys for their team members:

```
curl --location 'http://0.0.0.0:4000/key/generate' \    --header 'Authorization: Bearer sk-TeamAdminKey123' \    --header 'Content-Type: application/json' \    --data '{        "user_id": "krrish@berri.ai",        "team_id": "01044ee8-441b-45f4-be7d-c70e002722d8"
```

### 6. `Team Admin` - Update Team Settings[​](https://docs.litellm.ai/docs/proxy/access_control#6-team-admin---update-team-settings "Direct link to 6-team-admin---update-team-settings")
This step needs a proxy admin to have enabled `rpm_limit` and `max_budget` under [team admin editable fields](https://docs.litellm.ai/docs/proxy/access_control#choosing-what-team-admins-can-edit). With those two entries the team admin can change the RPM limit and keep or lower the team budget; raising `max_budget` also needs the `raise_max_budget` entry.

```
curl --location 'http://0.0.0.0:4000/team/update' \    --header 'Authorization: Bearer sk-TeamAdminKey123' \    --header 'Content-Type: application/json' \    --data '{        "team_id": "01044ee8-441b-45f4-be7d-c70e002722d8",        "max_budget": 100,        "rpm_limit": 1000
```

In this example, `max_budget: 100` succeeds when the team's current budget is already `100` or higher. Going above the current budget needs `raise_max_budget` as well, and on an org-scoped team the new value must stay within the organization's `max_budget`. With no fields enabled the call returns 403 and tells the team admin which setting to ask a proxy admin for.
  * [When to Use Each Key Type](https://docs.litellm.ai/docs/proxy/access_control#when-to-use-each-key-type)
  * [User Roles](https://docs.litellm.ai/docs/proxy/access_control#user-roles)
    * [Organization/Team Specific Roles](https://docs.litellm.ai/docs/proxy/access_control#organizationteam-specific-roles)
  * [Usage dashboard visibility](https://docs.litellm.ai/docs/proxy/access_control#usage-dashboard-visibility)
  * [What Can Each Role Do?](https://docs.litellm.ai/docs/proxy/access_control#what-can-each-role-do)
  * [Global Proxy Roles](https://docs.litellm.ai/docs/proxy/access_control#global-proxy-roles-1)
    * [Proxy Admin - Full Access](https://docs.litellm.ai/docs/proxy/access_control#proxy-admin---full-access)
    * [Proxy Admin Viewer - Platform-Wide Read Access](https://docs.litellm.ai/docs/proxy/access_control#proxy-admin-viewer---platform-wide-read-access)
    * [Internal User Viewer - Read-Only Access](https://docs.litellm.ai/docs/proxy/access_control#internal-user-viewer---read-only-access)
  * [Organization/Team Specific Roles](https://docs.litellm.ai/docs/proxy/access_control#organizationteam-specific-roles-1)
    * [Org Admin - Organization Level Access](https://docs.litellm.ai/docs/proxy/access_control#org-admin---organization-level-access)
    * [Team Admin - Team Level Access](https://docs.litellm.ai/docs/proxy/access_control#team-admin---team-level-access)
  * [Team Member Permissions](https://docs.litellm.ai/docs/proxy/access_control#team-member-permissions)
    * [Available Permissions](https://docs.litellm.ai/docs/proxy/access_control#available-permissions)
    * [Common Permission Scenarios](https://docs.litellm.ai/docs/proxy/access_control#common-permission-scenarios)
    * [How to Configure Team Member Permissions](https://docs.litellm.ai/docs/proxy/access_control#how-to-configure-team-member-permissions)
    * [Who Can Configure These Permissions?](https://docs.litellm.ai/docs/proxy/access_control#who-can-configure-these-permissions)
  * [Quick Comparison](https://docs.litellm.ai/docs/proxy/access_control#quick-comparison)
    * [Organization/Team Specific Roles](https://docs.litellm.ai/docs/proxy/access_control#organizationteam-specific-roles-2)
  * [Onboarding Organizations](https://docs.litellm.ai/docs/proxy/access_control#onboarding-organizations)
    * [1. Creating a new Organization](https://docs.litellm.ai/docs/proxy/access_control#1-creating-a-new-organization)
    * [2. Adding an `org_admin` to an Organization](https://docs.litellm.ai/docs/proxy/access_control#2-adding-an-org_admin-to-an-organization)
    * [3. `Organization Admin` - Create a Team](https://docs.litellm.ai/docs/proxy/access_control#3-organization-admin---create-a-team)
    * [4. `Organization Admin` - Add a Team Admin](https://docs.litellm.ai/docs/proxy/access_control#4-organization-admin---add-a-team-admin)
    * [5. `Team Admin` - Add Team Members](https://docs.litellm.ai/docs/proxy/access_control#5-team-admin---add-team-members)
    * [6. `Team Admin` - Update Team Settings](https://docs.litellm.ai/docs/proxy/access_control#6-team-admin---update-team-settings)


LiteLLM Enterprise
SSO/SAML, audit logs, spend tracking, multi-team management, and guardrails, built for production.
[Learn more →](https://docs.litellm.ai/docs/enterprise)
