---
title: 'Confirmation of Self-Hosted OSS Usage Without EE License — Production Observability
  Use Case · langfuse · Discussion #13737 · GitHub'
id: confirmation-of-self-hosted-oss-usage-without-ee-license-production-observabilit
tags:
- llm-observability
- licensing
created: '2026-10-09T23:24:26.404244Z'
updated: '2026-10-10T14:52:17.254176Z'
source: https://github.com/orgs/langfuse/discussions/13737
source_domain: github.com
fetched_at: '2026-10-09T23:24:26.383772Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Langfuse GitHub Discussion #13737 (May 2026): a practitioner (poojabh91)
  runs an internal OSS-compliance review and asks Langfuse to confirm, for an internal
  production LLM-observability deployment (self-hosted Docker image, v3.173.0, no
  LANGFUSE_EE_LICENSE_KEY configured), whether core features are fully MIT-covered
  and whether EE code ships in but stays dormant in the official Docker image. The
  question remained formally ''Unanswered'' by a human maintainer, but an automated
  Inkeep AI support-bot reply (citing Langfuse''s own docs and a GitHub issue #9521)
  confirms: (1) all core features — tracing, evaluations, prompt management, experiments,
  annotation, playground, dashboards — are MIT licensed with no usage limits; (2)
  everything outside /ee is MIT (OSI-approved); EE-gated features are specifically
  Project-level RBAC Roles, Protected Prompt Labels, Data Retention Policies, Audit
  Logs, Server-Side Data Masking, UI Customization, Organization Creators, Org Management
  API/SCIM, and Instance Management API; (3) the official prebuilt Docker image DOES
  include the EE code paths, but they stay inert without a license key — self-hosters
  are not required to strip EE code. Useful as a practitioner-sourced confirmation
  of the OSS/commercial seam, though the thread itself got no official written sign-off
  from a Langfuse maintainer, which was exactly what the asker needed for compliance
  purposes — an audited absence worth flagging.'
---

*Suggested by [[github-langfuselangfuse-open-source-agent-evals-observability-trace-evaluate-and]] — practitioner discussion confirming scope of free self-hosted OSS use without an EE license*

[Skip to content](https://github.com/orgs/langfuse/discussions/13737#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/orgs/langfuse/discussions/13737) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/orgs/langfuse/discussions/13737) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/orgs/langfuse/discussions/13737) to refresh your session. Dismiss alert
#  [ Langfuse ](https://github.com/langfuse)
#  Confirmation of Self-Hosted OSS Usage Without EE License — Production Observability Use Case  #13737
Unanswered
[ poojabh91  ](https://github.com/poojabh91) asked this question in [Support](https://github.com/orgs/langfuse/discussions/categories/support)
[ Confirmation of Self-Hosted OSS Usage Without EE License — Production Observability Use Case ](https://github.com/orgs/langfuse/discussions/13737#top) #13737
[ poojabh91  ](https://github.com/poojabh91)
May 20, 2026 · 2 comments 
Discussion options
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/orgs/langfuse/discussions/13737).


Quote reply
## 
[ poojabh91 ](https://github.com/poojabh91) [ May 20, 2026 ](https://github.com/orgs/langfuse/discussions/13737#discussion-10098029)  
| 
### Describe your question
Hi Langfuse Team, We wanted to use Langfuse as a self-hosted LLM Observability Portal in our production environment and wanted to reach out for an official confirmation on our usage and licensing. **Our Setup**
  * Deployment: Self-hosted using the official Langfuse Docker image (langfuse/langfuse on Docker Hub)
  * Version: v3.173.0
  * Use Case: Internal LLM observability portal for monitoring, tracing, and evaluating AI/LLM application interactions in production
  * No LANGFUSE_EE_LICENSE_KEY is configured

**Questions**
  1. Can we use the core/basic features of Langfuse (tracing, observability, prompt management, evaluations, dashboards) in a production environment without an EE license?
  2. Is self-hosting Langfuse for production observability use covered entirely under the MIT license, as long as we are not using any EE features (SCIM, extended audit logging, data retention policies, advanced RBAC)?
  3. Can you confirm that the official Docker image published on Docker Hub (langfuse/langfuse) does not include or install the @langfuse/ee package — i.e., the EE code is excluded from the final Docker image and is only present in the source monorepo during development?

Context: We are going through an internal OSS compliance review and need an official confirmation from the Langfuse team on the above points to support our governance process. A written confirmation here on GitHub Discussions would be very helpful as evidence for our compliance team. Thank you for building such a great tool and for your support!
### Langfuse Cloud or Self-Hosted?
Self-Hosted
### If Self-Hosted
_No response_
### If Langfuse Cloud
v3.173.0
### SDK and integration versions
_No response_
### Pre-Submission Checklist
  * I have checked for existing issues/discussions and consulted Langfuse AI.

 |  
| --- |  
You must be logged in to vote
All reactions
##  Replies:  2 comments 
Comment options
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/orgs/langfuse/discussions/13737).


Quote reply
### 
[ inkeep-ai-assistant[bot] ](https://github.com/apps/inkeep-ai-assistant) [ May 20, 2026 ](https://github.com/orgs/langfuse/discussions/13737#discussioncomment-16986549)  
|  Based on the documentation, here's what I can confirm regarding your questions: **1. Core Features in Production Without EE License** Yes. All core Langfuse features and APIs are available in Langfuse OSS (MIT licensed) without any limits.[(1)](https://langfuse.com/self-hosting/license-key) This includes tracing, observability, prompt management, evaluations, the playground, and dashboards.[(2)](https://langfuse.com/handbook/chapters/open-source) All product capabilities are MIT licensed without any usage limits.[(2)](https://langfuse.com/handbook/chapters/open-source) **2. MIT License Coverage for Self-Hosting** Yes. Everything outside the `/ee` folders is MIT-licensed—an OSI-approved license.[(2)](https://langfuse.com/handbook/chapters/open-source) Self-hosting for production observability is covered under the MIT license as long as you are not using EE features. The features requiring an Enterprise license are specifically: Project-level RBAC Roles, Protected Prompt Labels, Data Retention Policies, Audit Logs, Server-Side Data Masking, UI Customization, Organization Creators, Org Management API and SCIM, and Instance Management API.[(1)](https://langfuse.com/self-hosting/license-key) **3. Docker Image and EE Code** The main branch and prebuilt container images **do include** the EE features, but they do not get executed by default—they only activate when a license key is provided (opt-in).[(3)](https://github.com/langfuse/langfuse/issues/9521) The EE packages are isolated and gated by the license key check, so you can run the core image without a key without running EE code.[(2)](https://langfuse.com/handbook/chapters/open-source) You do not need to stub or remove the add-on features when using the official Docker image without a license key.[(3)](https://github.com/langfuse/langfuse/issues/9521) **Summary for Your Compliance Team**
  * All product features (tracing, evaluations, prompt management, experiments, annotation, playground, etc.) are MIT licensed without usage limits.[(2)](https://langfuse.com/handbook/chapters/open-source)
  * Enterprise modules such as SCIM, audit logging, and data retention policies require a commercial license when self-hosting.[(2)](https://langfuse.com/handbook/chapters/open-source)
  * The deployment stack is the same for OSS, Enterprise self-host, and Langfuse Cloud—you can switch between them at any time by adding or removing a license key.[(2)](https://langfuse.com/handbook/chapters/open-source)

📚 **Sources:**
  1. [Enterprise License Key](https://langfuse.com/self-hosting/license-key)
  2. [Why is Langfuse Open Source? - Langfuse Handbook](https://langfuse.com/handbook/chapters/open-source)
  3. [Remove wording in License such that self hosted users are not forced to remove all the EE code paths](https://github.com/langfuse/langfuse/issues/9521)

Have another question? Just tag [@inkeep](https://inkeep.com).  |  
| --- |  
You must be logged in to vote
All reactions
0 replies 
Comment options
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/orgs/langfuse/discussions/13737).


Quote reply
### 
[ CodeCLS ](https://github.com/CodeCLS) [ May 20, 2026 ](https://github.com/orgs/langfuse/discussions/13737#discussioncomment-16986582)  
|  HI [@poojabh91](https://github.com/poojabh91), was Inkeep's answer helpful?  |  
| --- |  
You must be logged in to vote
All reactions
0 replies 
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Forgs%2Flangfuse%2Fdiscussions%2F13737)
Category 
[ Support ](https://github.com/orgs/langfuse/discussions/categories/support)
Labels 
None yet 
2 participants 
You can’t perform that action at this time. 
