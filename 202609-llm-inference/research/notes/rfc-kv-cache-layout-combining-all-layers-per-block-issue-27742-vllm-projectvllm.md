---
title: '[RFC]: KV cache layout combining all layers per block · Issue #27742 · vllm-project/vllm'
id: rfc-kv-cache-layout-combining-all-layers-per-block-issue-27742-vllm-projectvllm
tags:
- kv-cache
- vllm-issue
- rfc
- memory-layout
- kv-transfer
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:58:22.771266Z'
updated: '2026-09-15T04:55:24.108172Z'
source: https://github.com/vllm-project/vllm/issues/27742
source_domain: github.com
fetched_at: '2026-09-14T11:58:22.751254Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'vLLM RFC #27742 (opened Oct 29, 2025 by orozery, closed ''not planned''/stale
  May 2026 after the uniform-model case shipped) is the original design rationale
  for the KV cache memory-layout change repeatedly cited elsewhere as the source of
  order-of-magnitude KV-transfer throughput gains. Motivation: for simple (non-Hybrid-Memory-Allocator)
  models, vLLM''s default KV cache layout breaks each logical block''s data into num_layers
  non-contiguous per-layer segments (further fragmented by k/v and kv_heads dimensions),
  which is costly for two transfer-heavy consumers -- NixlConnector (P/D disaggregation,
  GPU-to-host-buffer/other-agent) and OffloadingConnector (GPU-to-CPU). Proposes allocating
  one contiguous tensor holding all layers'' KV data per block instead of one tensor
  per layer, reducing fragments to copy by a factor of num_layers. Author reports
  an initial measured 4x transfer-time improvement for both NixlConnector and OffloadingConnector
  on Llama-3.1-8B-Instruct, with no noticeable degradation in plain model-execution
  performance. New layout is opt-in, selected only when (1) the model has a simple
  uniform (non-HMA) layout and (2) the configured KV connector class explicitly requests
  it via a new classmethod. Discussion thread with maintainers (ApostaC, heheda12345,
  ronen tlrmchlsmth) covers extending the approach to Hybrid Memory Allocator models
  with heterogeneous per-layer page sizes (e.g., DeepSeek V3.2''s two KV cache types
  with different hidden sizes), concluding the uniform-page-size case (is_kv_cache_page_size_uniform)
  is tractable but arbitrary heterogeneous page sizes per layer (UniformTypeKVCacheSpecs)
  are not solved by this scheme. The uniform-model implementation shipped in PR #27743
  (linked from this RFC), which is the change the Jan 2026 vLLM offloading-connector
  blog post credits with the observed 0.11.0->0.12.0 throughput jump.'
---

*Suggested by [[inside-vllms-new-kv-offloading-connector-smarter-memory-transfer-for-maximizing]] — RFC for the KV cache memory layout change cited as the cause of the offloading connector's order-of-magnitude throughput gain*

[Skip to content](https://github.com/vllm-project/vllm/issues/27742#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/27742) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/27742) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/27742) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/issues/27742).
/ Public
  * Sponsor
#  Sponsor vllm-project/vllm 
##### GitHub Sponsors
[Learn more about Sponsors](https://github.com/sponsors)
[ vllm-project ](https://github.com/vllm-project)
[ vllm-project ](https://github.com/vllm-project)
[ Sponsor  ](https://github.com/sponsors/vllm-project)
##### External links
[opencollective.com/**vllm**](https://opencollective.com/vllm)
[Learn more about funding links in repositories](https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/displaying-a-sponsor-button-in-your-repository). 
[Report abuse](https://github.com/contact/report-abuse?report=vllm-project%2Fvllm+%28Repository+Funding+Links%29)
  * [ Notifications ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm) You must be signed in to change notification settings
  * [ Fork 22.2k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm)
  * [ Star  91.7k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm)


#  [RFC]: KV cache layout combining all layers per block #27742
Copy link
Copy link
Not planned
Not planned
[[RFC]: KV cache layout combining all layers per block](https://github.com/vllm-project/vllm/issues/27742#top)#27742
Copy link
Labels
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
## Description
opened [on Oct 29, 2025](https://github.com/vllm-project/vllm/issues/27742#issue-3565618316)
Last edited by orozery
Collaborator
Issue body actions
### Motivation.
High-level goal: optimize the transfer time of KV data from/to the vllm KV cache by reducing per-transfer fragmentation at the vllm KV cache side.
The 2 main use-cases we're considering are:
  1. NixlConnector
  2. OffloadingConnector


So our goal is to try and optimize the time each of these connectors spend for a single transfer (GPU->CPU of the offloading connector, and GPU->host_buffer/other agent for NixlConnector). The transfer unit is a block of KV-data corresponding to some aligned (default)16-tokens out of some request. We consider the case of simple models where all layers have the same layout (i.e. not using the Hybrid Memory Allocator), although this can be explored in the future. For this case, the current layout for the vllm KV cache is (roughly) this:
As you can see, the KV data that needs to be transferred is breaked down to num_layers non-contiguous segments. This does not take into account the k_or_v and num_kv_heads dimensions which may further cause extra fragmentation.
In this RFC, we propose a new KV cache layout that for each block, will place all KV data for all layers contiguously:
This will reduce the number of fragments to copy by a factor of num_layers.
We have tested this with both the NixlConnector and the OffloadingConnector and saw that the transfer time is improved by a **factor of X4** (tested on Llama-3.1-8B-Instruct). Furthermore, we tested plain model execution and saw no noticeable degradation in performance.
### Proposed Change.
We propose to change the KV tensor allocation in `gpu_model_runner.py` to support this new layout, allocating a single tensor for all layers (instead of 1 per layer as it is today). This new layout will only be selected if:
  1. The model has a simple uniform layout (not HMA)
  2. A KV connector is configured and, the KV connector class explicitly prefers (via a new classmethod) to use the new layout.


### Feedback Period.
_No response_
### CC List.
[@njhill](https://github.com/njhill) [@NickLucche](https://github.com/NickLucche) [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) [@heheda12345](https://github.com/heheda12345) [@LucasWilkinson](https://github.com/LucasWilkinson) [@tdoublep](https://github.com/tdoublep) [@tlrmchlsmth](https://github.com/tlrmchlsmth)
### Any Other Things.
_No response_
### Before submitting a new issue...
  * Make sure you already searched for relevant issues, and asked the chatbot living at the bottom right corner of the [documentation page](https://docs.vllm.ai/en/latest/), which can answer lots of frequently asked questions.
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


## Activity
[orozery](https://github.com/orozery)
added [on Oct 29, 2025](https://github.com/vllm-project/vllm/issues/27742#event-20577659177)
### ApostaC commented on Oct 29, 2025 
Collaborator
More actions
This idea is interesting. How will it support HMA models in the future?
cc [@heheda12345](https://github.com/heheda12345)
### orozery commented on Oct 29, 2025 
CollaboratorAuthor
More actions
> This idea is interesting. How will it support HMA models in the future?
The challenge is how to support the layout with minimal code changes. For uniform models, it's a small change since we create views with the old shape. More generally, we can use this trick to combine any layers that have the same page size. For layers with different page sizes I currently don't see an easy way.
### heheda12345 commented on Oct 30, 2025 
Collaborator
More actions
I think it can be compatible with HMA theoretically. One goal of HMA is to make all different types of kv cache agree on the same page size so it may help on this layout.
### orozery commented on Oct 30, 2025 
CollaboratorAuthor
More actions
> I think it can be compatible with HMA theoretically. One goal of HMA is to make all different types of kv cache agree on the same page size so it may help on this layout.
Looking at `get_kv_cache_groups` I see 4 cases:
  1. is_kv_cache_type_attention_free
  2. is_kv_cache_spec_uniform
  3. UniformTypeKVCacheSpecs
  4. is_kv_cache_page_size_uniform


(1) is not interesting. (2) is the case we're currently targeting. (4) seems feasible (I guess this is what you meant)
But in (3) you could have different page sizes per layers.
### heheda12345 commented on Oct 30, 2025 
Collaborator
More actions
BTW deepseek v3.2 has two types of kv cache with different hidden size. For a model with different hidden size , , ..., , we can do something like:

```
total = h1 + h2 + ... + h_n
buffer = torch.empty(shape=((num_blocks + 1) * total,))
kv_cache_2 = buffer[h1: h1 + num_blocks * total].view(num_blocks, total)
# the kv cache of layer 2 page p can be accessed via:
kv_cache_2[p, :h2]

```

### heheda12345 commented on Oct 30, 2025 
Collaborator
More actions
lol we have the same concern
### tlrmchlsmth commented on Oct 30, 2025 
Member
More actions
> We have tested this with both the NixlConnector and the OffloadingConnector and saw that the transfer time is improved by a factor of X4 (tested on Llama-3.1-8B-Instruct).
Nice! Can you link the performance numbers you have for this? This change makes sense to me if we can resolve the non-homogenous page size per layer issue
### orozery commented on Oct 30, 2025 
CollaboratorAuthor
More actions
> This change makes sense to me if we can resolve the non-homogenous page size per layer issue
I think we can start off the uniform layers case. It is a low-hanging fruit which I guess covers most of the current use-cases. I already have a small PR prepared ([#27743](https://github.com/vllm-project/vllm/pull/27743)). Another PR adapting the NixlConnector is in progress, and it should be relatively small as well.
### orozery commented on Oct 30, 2025 
CollaboratorAuthor
More actions
> Nice! Can you link the performance numbers you have for this?
Liran has been working on this. I will ask him to post it here.
### liran-s commented on Oct 30, 2025 
More actions
> > Nice! Can you link the performance numbers you have for this?
> Liran has been working on this. I will ask him to post it here.
Here are initial performance numbers we did: <https://docs.google.com/spreadsheets/d/1i9Pd7vC1R1OnhLEE4tJTecL2COVw-j1lQLxtE-daPOE/edit?usp=sharing>
[nv-yna](https://github.com/nv-yna)
added a commit that references this issue [on Dec 23, 2025](https://github.com/vllm-project/vllm/issues/27742#event-21721428087)
[feat: add FullyContiguous layout detection for vLLM cross-layer KV ca…](https://github.com/ai-dynamo/dynamo/commit/91fca2832f791d95b48c8fa9513cbbecd04e454a)
### github-actions commented on Jan 28, 2026 
[on Jan 28, 2026](https://github.com/vllm-project/vllm/issues/27742#issuecomment-3815004953) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue has been automatically marked as stale because it has not had any activity within 90 days. It will be automatically closed if no further activity occurs within 30 days. Leave a comment if you feel this issue should remain open. Thank you!
[github-actions](https://github.com/apps/github-actions)
added 
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
[on Jan 28, 2026](https://github.com/vllm-project/vllm/issues/27742#event-22363483459)
### orozery commented on Jan 29, 2026 
CollaboratorAuthor
More actions
We added support for uniform models in [#27743](https://github.com/vllm-project/vllm/pull/27743). Keeping this RFC open as we plan to extend support for hybrid models as well.
[orozery](https://github.com/orozery)
removed 
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
[on Jan 29, 2026](https://github.com/vllm-project/vllm/issues/27742#event-22369923300)
### github-actions commented on Apr 29, 2026 
[on Apr 29, 2026](https://github.com/vllm-project/vllm/issues/27742#issuecomment-4349247515) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue has been automatically marked as stale because it has not had any activity within 90 days. It will be automatically closed if no further activity occurs within 30 days. Leave a comment if you feel this issue should remain open. Thank you!
[github-actions](https://github.com/apps/github-actions)
added 
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
[on Apr 29, 2026](https://github.com/vllm-project/vllm/issues/27742#event-25016098285)
### github-actions commented on May 29, 2026 
[on May 29, 2026](https://github.com/vllm-project/vllm/issues/27742#issuecomment-4581333814) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue has been automatically closed due to inactivity. Please feel free to reopen if you feel it is still relevant. Thank you!
[github-actions](https://github.com/apps/github-actions)
closed this as [not planned](https://github.com/vllm-project/vllm/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Anot-planned)[on May 29, 2026](https://github.com/vllm-project/vllm/issues/27742#event-26134727703)
[Sign up for free](https://github.com/signup?return_to=https://github.com/vllm-project/vllm/issues/27742)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/vllm-project/vllm/issues/27742)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
No type
### Projects
No projects
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
