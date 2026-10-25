---
title: '[RFC]: Cache Salting for Secure and Flexible Prefix Caching in vLLM · Issue
  #16016 · vllm-project/vllm'
id: rfc-cache-salting-for-secure-and-flexible-prefix-caching-in-vllm-issue-16016-vll
tags:
- kv-cache
- vllm-issue
- cache-salting
- security
- rfc
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:53:55.025343Z'
updated: '2026-09-15T04:55:24.058463Z'
source: https://github.com/vllm-project/vllm/issues/16016
source_domain: github.com
fetched_at: '2026-09-14T11:53:55.003872Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'vLLM GitHub RFC #16016 (opened Apr 3, 2025 by dr75, closed May 6, 2025)
  proposing cache salting to defend vLLM''s prefix cache against the timing side-channel
  attack described in ''Leaking Secrets from Prefix Caches'' (arXiv:2411.18191): an
  attacker can guess candidate token blocks and infer cache hits (hence prior use
  by another tenant) from response latency, potentially reconstructing private prompts
  block by block. Proposes two designs: (A) single-barrier -- one cache_salt string
  per request, injected into the hash of the first block, restricting reuse to requests
  sharing that salt; (B) multi-barrier -- a cache_salt_map keyed by message index,
  allowing hierarchical isolation (e.g., an org-level salt on a shared document plus
  a user-level salt on the follow-up question, so the document''s KV is shared org-wide
  while the question stays private even from other org members). Salts are base64-encoded
  256-bit strings, not passed to the model, used only for block hashing. Maintainer
  comaniac initially suggested piggybacking on auth tokens (à la OpenAI''s org-scoped
  cache sharing) instead; author dr75 countered that vLLM has only one global API
  key and that org-scoped-only sharing (OpenAI''s model) is less flexible than salt
  hierarchies (e.g., nested tenant-of-tenants isolation). RFC concludes design (B)
  is the long-term target but recommends shipping (A) first for simplicity; (A) shipped
  in PR #17045. This directly explains the cache_salt / extra_hashes mechanism documented
  in vLLM''s Automatic Prefix Caching design doc. A 2026 follow-up comment (user vpfkfl753,
  PR #52655) reports implementing multi-barrier as token-range salt regions and claims
  a synthetic timing-side-channel probe showed unsalted cross-tenant cache reuse is
  fully distinguishable via response usage/timing (reported timing AUC 1.0) while
  salted isolation reduces distinguishability to near-chance (AUC 0.462-0.551) with
  a measured throughput cost from lost cross-tenant cache reuse; this is a single
  commenter''s self-reported, non-peer-reviewed benchmark and should be treated as
  an unverified claim, not confirmed vLLM behavior.'
utility_score: 13.0
---

[Skip to content](https://github.com/vllm-project/vllm/issues/16016#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/16016) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/16016) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/16016) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/issues/16016).
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


#  [RFC]: Cache Salting for Secure and Flexible Prefix Caching in vLLM #16016
Copy link
Copy link
Closed
Closed
[[RFC]: Cache Salting for Secure and Flexible Prefix Caching in vLLM](https://github.com/vllm-project/vllm/issues/16016#top)#16016
Copy link
Assignees
Labels
## Description
opened [on Apr 3, 2025](https://github.com/vllm-project/vllm/issues/16016#issue-2969715019)
Contributor
Issue body actions
### Motivation.
vLLM’s prefix caching (APC) improves inference performance by reusing previously computed KV cache accross requests. The prefix cache operates by hashing input in fixed-size blocks (typically 16 tokens), and can use strong hashing (e.g., SHA-256) to guard against hash collisions.
However, as demonstrated in [Leaking Secrets from Prefix Caches](https://arxiv.org/html/2411.18191v1), the cache remains vulnerable to timing-based side-channel attacks. An attacker can infer cache reuse by guessing popular inputs and measuring latency — potentially compromising privacy especially in shared or multi-tenant environments.
What’s missing in vLLM today is a simple, effective way to **segment cache reuse across groups** while preserving performance. Note that other providers do also limit cache sharing, e.g., OpenAI is [limiting cache sharing to organizations](https://platform.openai.com/docs/guides/prompt-caching).
### Proposed Change.
We propose to salt block hashes by introducing an optional top-level field in the request schema, which allows users to set the salts to be used for hashing per request. By sharing the salt, users can enable prefix caching while protecting the prompt
We analyzed **two different designs** , which have different benefits and drawbacks.
### (A) Single-barrier design - one salt per request

```
{
  messages=[{
    "role": "system",
    "content": "You are a helpful assistant."
  },  {
    "role": "user",
    "content": "Here is a document with details about the world series: ...",
  }, {
    "role": "user",
    "content": "Who won the world series in 2020?",
  }],
  "cache_salt": "Z3V2bmV3aGxza3ZubGFoZ3Zud3V3ZWZ2bmd0b3V2bnZmc2xpZ3RoZ2x2aQ=="
}
```

If cache_salt is provided, this value:
  * Is injected into the hash of the first block of a request.
  * Ensures that cache reuse is restricted to requests using the same salt.
  * Prevents cache state inference across salt boundaries, i.e., every user who provides the same salt can use the cache and is protected against timing attacks from users who don't share the salt.


The single-barrier design protects requests that provide a salt value while allowing cache reuse between requests that use the same salt. The single-barrier design is easy to use and implement in vLLM but comes with limited flexibility as it does not allow cache reuse between users while protecting user prompts at the same time.
### (B) Muli-barrier design - per message salt by message index
To overcome the limitation of design (A), we propose an alternative design that allows an arbitrary number of salts, each salt assigned to a certain message.

```
{
  messages=[{
    "role": "system",
    "content": "You are a helpful assistant."
  },  {
    "role": "user",
    "content": "Here is a document with details about the world series: ...",
  }, {
    "role": "user",
    "content": "Who won the world series in 2020?",
  }],
  "cache_salt_map": {
    "1": "org-salt",
    "2": "user-salt"
  }
}
```

If `cache_salt_map` is provided, each salt:
  * Is injected into the hash of the corresponding message defined by the message index. 
    * The system message (index 0) is not protected by a salt such that the corresponding KV cache is reused accross all users.
    * `"org-salt"` is assigned to message 1 (zero-based index; `"Here is a document ..."`). Only requests that use `"org-salt"` can use the KV cache of the document. The message (and following messages) is protected against attacks from users outside the org that don't have acces to the salt.
    * `"user-salt"` is assigned to message 2 (`"Who won ..."`). Only requests with `"user-salt"` reuse the cache of this and following messages. Prompts are protected against attacks even from within the same org and by attackers with access to the shared `org-salt`.
  * Ensures that cache reuse is restricted to prompt prefixes that share the same salt.
  * Prevents cache state inference across salt boundaries.
  * Allows for cache reuse between a group of users while protecting the user's or subgroups prompts, up to an arbitrary hierarchy of user groups. This includes protecting most sensitive information by keeping salts local without sharing.


## Comparison
### (A) Single-barrier design
  * Single salt means only one barrier per request. 
    * Allows for cache sharing & prompt protection within a group of users OR for a single user, but not both (e.g., share doc but protect question).
    * No hierarchical cache reuse (stages of shared caches).
    * It practically disables large scale cache reuse if small scale protection is required.
  * Simple API
  * Simple implementation as only one salt needs to be added to the first block.


### (B) Multi-barrier design
  * High flexibility 
    * Hierarchical cache reuse (stages of shared caches) and prompt protection (stages of security).
    * E.g., allows for cache reuse accross users AND user prompt protection at the same time (e.g., share doc but protect question).
  * More complex API with salt-message mapping.
  * Complex implementation as the mapping of salts to messages has to be propagated to tokens through chat template rendering to be able to inject the salt into the block hash at the right locations.


## Design Details
  * Salts are base64-encoded 256-bit (32-byte) strings.
  * If no salt is provided, default global caching behavior applies.
  * Salts are not passed to the model but only used internally for block hashing.
  * Only the first hashed block of a message (or globally for the single-barrier design) is directly affected. The following blocks are protected due to inclusion of the predecessor hash.


## Implementation Considerations
  * If cache_salt is omitted, the cache is globally shared (status quo), not causing any performance impact.
  * With both solutions, salts can be generated by clients per different user groups. E.g., 
    * per organization or group of users, enabling cache reuse inside that group
    * per user, isolating a user from other users of the same org, while allowing reuse of the user's cache
    * Only the multi-barrier design supports reuse within one group while protecting subgroups or users.
  * Service providers that use vLLM to offer their service can apply a default salting scheme, e.g., per organization, while still allowing flexibility by overriding the salt as needed.


## Conclusion
As the multi-barrier design allows for protecting user prompts, while sharing cache between users, we propose to implement the multi-barrier design (option B) in the long run. Due to the complexity of the implementation (e.g., forwarding message boundaries through template rendering), we would implement it with an iterative approach starting with the single-barrier design and adding the multi-barrier implementation afterwards.
Both designs are incompatible in terms of the API (assuming a union of string and dict types for the same field is not desired). To be able to provide both in the API (to first implement A and later B), there are two options:
  * use two different request fields (cache_salt and cache_salt_map) for both designs
  * start with cache_salt_map (or just cache_salts) but allow only a single value (`"0" : "<SALT>"`) until the multi-barrier design is implemented.


We suggest the latter approach to keep the API simpler in the long run.
### Feedback Period.
One week from April 3, 2025.
### CC List.
### Any Other Things.
_No response_
### Before submitting a new issue...
  * Make sure you already searched for relevant issues, and asked the chatbot living at the bottom right corner of the [documentation page](https://docs.vllm.ai/en/latest/), which can answer lots of frequently asked questions.
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


👍React with 👍1Reacted by Chauncey
## Activity
added [on Apr 3, 2025](https://github.com/vllm-project/vllm/issues/16016#event-17114546462)
### comaniac commented on Apr 7, 2025 
Collaborator
More actions
Thanks for brining this up. My two cents:
  1. `An attacker can infer cache reuse by guessing popular inputs and measuring latency — potentially compromising privacy especially in shared or multi-tenant environments.` Could you help educate me a bit more on this? The issue is if attackers could know which tokens are cached, they could somehow steal the questions/responses from others?
  2. The most important concern in the design of this PR is that this is not OpenAI protocol compatible. Based on OpenAI's strategy, I feel we could probably just add auth token to the prefix hashing, so that users with different auth tokens won't hit the same blocks. It aligns to OpenAI's behavior and doesn't require any change in the user interface.


[comaniac](https://github.com/comaniac)
self-assigned this
[on Apr 7, 2025](https://github.com/vllm-project/vllm/issues/16016#event-17157528471)
### dr75 commented on Apr 8, 2025 
ContributorAuthor
More actions
>   1. `An attacker can infer cache reuse by guessing popular inputs and measuring latency — potentially compromising privacy especially in shared or multi-tenant environments.` Could you help educate me a bit more on this? The issue is if attackers could know which tokens are cached, they could somehow steal the questions/responses from others?
> 

Yes, guessing blocks of tokens using a dictionary, grammar, and even an LLM for sampling candidate phrases. A faster response means a cache hit, revealing a block match. As the first block often includes a system prompt, it might be easier to guess due to a smaller number of tokens until the block is full. For subsequent blocks, the context guides guesses such that an attacker can potentially reconstruct an entire conversation iteratively.
>   1. The most important concern in the design of this PR is that this is not OpenAI protocol compatible. Based on OpenAI's strategy, I feel we could probably just add auth token to the prefix hashing, so that users with different auth tokens won't hit the same blocks. It aligns to OpenAI's behavior and doesn't require any change in the user interface.
> 

Great idea in principle, but vLLM has just one global api key so it cannot be used for separating users (and makes sense to have auth in a different service). More importantly, we can do better than OpenAI’s one-org-per-cache that doesn't cover many use cases, e.g., sharing a base document across an entire org while keeping individual user prompts private. Another example is a tenant that has themselves multiple tenants, where each tenant needs cache isolation.
I fully agree that it should be compatible with the OpenAI API, which is why I propose an additional `optional` field so it is fully backwards compatible. Other fields like `top_k` are also [beyond the OpenAI API](https://docs.vllm.ai/en/latest/serving/openai_compatible_server.html). The OpenAI client can still be used:

```
    response = client.chat.completions.create(
        model=model,
        messages=messages,
        max_tokens=256,
        extra_body={
            "cache_salt": salt,
        },
    )
```

Finally, as multi-tenant setups will be behind some form of API gateway, service providers can set the salt as needed and do not have to expose the functionality to their clients.
### dr75 commented on Apr 10, 2025 
ContributorAuthor
More actions
Hey [@WoosukKwon](https://github.com/WoosukKwon), would be great if you could take a look as well. Thanks!
### dr75 commented on May 6, 2025 
ContributorAuthor
More actions
The single barrier isolation (part A) is implemented in [[Core] Prevent side-channel attacks via cache salting #17045](https://github.com/vllm-project/vllm/pull/17045) as descibed above.
Closing now and getting back to multi barrier design (part B) two only if needed.
closed this as [completed](https://github.com/vllm-project/vllm/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on May 6, 2025](https://github.com/vllm-project/vllm/issues/16016#event-17536755352)
### vpfkfl753 commented on Aug 17, 2026 
More actions
Update on option B: I implemented it and am upstreaming it as a stack of small PRs.
[#52655](https://github.com/vllm-project/vllm/pull/52655) (open) is the core contract: token-range salt regions in block hashing. Flat `cache_salt` is untouched; a region at offset 0 hashes identically. Engine propagation and the chat-level `cache_salt_map` (message-index barriers resolved to token offsets in one render/tokenize pass) are implemented and follow once the core settles.
Why ranges instead of one flat salt per tenant (synthetic counterbalanced probe, Qwen2.5-7B chat completions on my prototype fork — the PRs are a cleaned-up port of it — 30 hit + 30 miss probes per scenario, prefix-cache reset per sample):
  * unsalted: median cached-token delta 1088, timing AUC 1.0, in all five isolation scenarios — cross-tenant reuse is visible straight from `usage`.
  * multi-salt (shared global prefix + per-tenant regions): delta 0 in all five, timing AUC 0.462–0.551.
  * reuse cost (`mcp_collapse`, n=30): flat per-tenant salt keeps median 0 cached prompt tokens on a shared global/org prefix; multi-salt keeps 3200 (unsalted: 3216).


Happy to share harness details.
[Sign up for free](https://github.com/signup?return_to=https://github.com/vllm-project/vllm/issues/16016)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/vllm-project/vllm/issues/16016)
## Metadata
## Metadata
### Assignees
  * [comaniac](https://github.com/comaniac)


### Labels
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
