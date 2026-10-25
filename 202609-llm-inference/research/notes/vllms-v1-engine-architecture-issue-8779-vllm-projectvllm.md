---
title: 'vLLM''s V1 Engine Architecture · Issue #8779 · vllm-project/vllm'
id: vllms-v1-engine-architecture-issue-8779-vllm-projectvllm
tags:
- internal-llm-inference-stacks-7f0c90
- vllm
- v1-architecture
- first-party-limitations
- adversarial-evidence
created: '2026-10-09T22:54:31.104911Z'
updated: '2026-10-10T14:52:17.025091Z'
source: https://github.com/vllm-project/vllm/issues/8779
source_domain: github.com
fetched_at: '2026-10-09T22:54:31.081074Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'First-party vLLM maintainer design doc (GitHub issue #8779, opened by Woosuk
  Kwon Sep 24, 2024, title changed from ''V2'' to ''V1'' Nov 13, 2024; still open/pinned
  as of the latest activity in the thread). This is rare first-person adversarial
  evidence of vLLM''s own pre-rewrite architectural limitations, stated directly by
  the maintainers as ''lessons learned'' from the original engine:\n\n- ''Python is
  slow'' -- CPU-side scheduling, de-tokenization, and sampler post-processing were
  identified as the actual bottlenecks, not GPU kernels: ''Fast GPUs like H100 do
  not necessarily have fast CPUs... GPUs will get faster and faster, while CPUs will
  not.''\n- ''Scheduling is not cheap'' -- ''For every step, the vLLM scheduler goes
  over the whole self.running queue and performs some operations for each request...
  written in Python.''\n- ''Input broadcasting is expensive'' -- sending full request
  state from scheduler to workers every step was wasteful; the fix was making workers
  stateful so only diffs are sent.\n- ''Preparing the model & sampler inputs... is
  expensive'' -- block-table/input construction needed incremental caching rather
  than rebuilding from scratch per step.\n- ''De-tokenization is expensive'' -- per-step
  de-tokenization and stop-criteria checks become a significant overhead at large
  batch sizes.\n- ''Sampler is expensive'' -- GPU sampler ops are cheap, but ''pythonizing''
  outputs and launching many small CUDA-synchronizing kernels is not.\n- Supporting
  heterogeneous model state (KV cache, Mamba cache, encoder cache) ''is challenging''
  and required native, type-specific cache managers to avoid fragmentation.\n\nThe
  V1 redesign''s stated goal was ''to minimize GPU idle time'' via async single-step
  scheduling (scheduler schedules step n+1 while workers execute step n), stateful
  SPMD workers (driver + TP*PP workers; even single-GPU runs use 2 processes), moving
  the detokenizer to the driver process, first-class prefix caching, and dropping
  native beam-search support (pushed to an external emulation layer) to reduce code
  complexity. Maintainer youkaichao states the rearchitecture ''will only affect vLLM
  developers'' with no breaking changes for direct API users except beam search. Community
  friction visible in the thread: a parallel community-proposed ''Workflow Defined
  Engine'' (WDE) refactor for encode-only/embedding/reranker models was explicitly
  rejected for being too large to merge at once (~10,000 LOC) per maintainer DarkLight1337.
  Issue was marked stale by bot after 90 days inactivity (Apr 2025) then manually
  kept open; a commit referencing this issue (GQA C8 fullgraph bugfix) was still landing
  as recently as Aug 2026, indicating the V1 architecture lineage is still actively
  touched years after the original 2024 proposal.'
utility_score: 16.0
---

[Skip to content](https://github.com/vllm-project/vllm/issues/8779#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/8779) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/8779) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/8779) to refresh your session. Dismiss alert
#  vLLM's V1 Engine Architecture
Copy link
Copy link
[vLLM's V1 Engine Architecture](https://github.com/vllm-project/vllm/issues/8779#top)#8779
Copy link
Labels
[keep-openPrevents stale label being applied](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22keep-open%22)Prevents stale label being applied
## Description
opened [on Sep 24, 2024](https://github.com/vllm-project/vllm/issues/8779#issue-2546072133)
Last edited by WoosukKwon
Collaborator
Issue body actions
This issues describes the high level directions that "create LLM Engine V1". We want the design to be as transparent as possible and created this issue to track progress and solicit feedback.
Goal:
  * The new engine will be simple and performant. We found the first iteration of the engine to be simple, the multistep engine to be performant, but we want best of the both worlds. For it to be performat, we want to **minimize GPU idle time**.
  * The new architecture will be extensible and modular. We found the current codebase becoming difficult to extend and add new features (both production and experimental features) due to the hard tangling of different features. In the new design, features should be compatible with each other.
  * Tech debts will be cleaned up. We will remove optimizations that compromise code readability. We will also redo ad-hoc implementations to support certain features/models.


Non-goals, the following are important but orthogonal:
  * Optimize GPU time/kernels
  * Add new features/optimizations
  * Performance in rare cases


The scope is exclusively in the scheduler, memory manager, distributed architecture. We will not touch APIs, models, kernels, and most parts of the model runner.
Highlights of the new design:
  * **Driver process + SPMD workers**
    * When TP=n & PP=m, vLLM engine will have n*m + 1 processes in total. 
      * Corollary: even when using a single GPU, we will have 2 processes.
    * The driver process will have the scheduler, memory manager, etc.
    * The workers are stateful, maintaining most of the request states. 
      * The driver will only send the “diffs” 
        * New request: input token IDs & block tables & sampling params, etc.
        * In-flight request: scheduled request IDs, new block IDs (no token IDs, sampling params, etc.)
    * Clean up data structures like SeqGroupMetadata
  * **Async single-step scheduling** , instead of multi-step scheduling 
    * Scheduler will schedule the n+1-th step, while the worker is executing the n-th step.
    * We will reuse the code from multi-step scheduling to incrementally update the model inputs.
    * Needs a special care for PP, since the output token IDs from the last stage should be sent to the first stage.
  * **De-tokenizer moves to the driver process**
    * Async de-tokenization can be regarded as part of async scheduling
  * **Native support for different types of model states**
    * Regular KV cache, Mamba cache, encoder cache, etc.
    * Dedicated memory manager & block table for each type of cache
  * **Drop beam search from vLLM engine**
    * Provide a solution to emulate beam search outside vLLM engine
  * **Prefix-caching as a first-class feature**
    * Implement parallel sampling via prefix caching
    * Remove the concept of SequenceGroup
    * Optimize prefix caching overheads
  * **Remove/minimize PyObjectCache**


Lessons we learned from V1:
  * To achieve high GPU utilization, we should care about everything happening on the CPU. 
    * Python is slow.
    * Fast GPUs like H100 do not necessarily have fast CPUs. They may have hundreds of CPU cores, but each with low clock speed.
    * Moreover, GPUs will get faster and faster, while CPUs will not.
  * Scheduling is not cheap. 
    * For every step, the vLLM scheduler goes over the whole `self.running` queue and performs some operations for each request (e.g., allocating a new block). And this is written in Python.
  * Input broadcasting is expensive. 
    * Instead of sending request information from scheduler to workers every step, **the workers should be stateful** and maintain most of the request states.
  * Preparing the model & sampler inputs (e.g., block table) is expensive. 
    * We should cache the inputs of the previous steps, and** build new inputs incrementally from the cached inputs**, if possible.
    * However, not every state should be kept in GPU memory. It’s OK to cache & incrementally build some inputs in CPU memory, and send them to GPU every step.
  * De-tokenization is expensive. 
    * For every step, vLLM de-tokenizes the generated output token IDs and checks the stop criteria.
    * The overhead becomes significant for large batch sizes.
  * Sampler is expensive. 
    * The GPU operations themselves are not very expensive.
    * However, “pythonizing” the sampler outputs is expensive.
    * Plus, the sampler can launch many small GPU kernels with CPU-GPU synchronizations.
  * Supporting different types of model states (e.g., KV cache, Mamba cache, encoder cache) is challenging. 
    * We need native cache managers for these different types of caches.
    * We need to deal with memory fragmentation due to the different sizes of the different states


Timeline wise, we plan to execute the changes incrementally. Overtime we will add PRs and issues related to the new architecture here.
  * [re-implement beam search on top of vllm core #8726](https://github.com/vllm-project/vllm/pull/8726)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


The design is led by the vLLM maintainers [@WoosukKwon](https://github.com/WoosukKwon) [@zhuohan123](https://github.com/zhuohan123) [@youkaichao](https://github.com/youkaichao) [@simon-mo](https://github.com/simon-mo) [@LiuXiaoxuanPKU](https://github.com/LiuXiaoxuanPKU) [@comaniac](https://github.com/comaniac) @alexm-neuralmagic [@njhill](https://github.com/njhill) @robertgshaw2-neuralmagic [@rkooo567](https://github.com/rkooo567) and many others!
👍React with 👍56Reacted by Sem Mulder, Jee Jee Li, Woosuk Kwon, Pastel！, Roger Wang, Wang Shoutong, Shangming Cai, Edward Park, Zhuohan Li, jyf111 and 46 more
🚀React with 🚀34Reacted by Tyler Michael Smith, Jee Jee Li, jiewang, Woosuk Kwon, Roger Wang, Shangming Cai, Edward Park, chottolabs, Zhuohan Li, Yuan and 24 more
## Activity
  1. [simon-mo](https://github.com/simon-mo)
added [on Sep 24, 2024](https://github.com/vllm-project/vllm/issues/8779#event-14386149896)
  2. ### youkaichao commented on Sep 24, 2024 
Member
More actions
I want to highlight that, the re-arch will only affect vllm developers who need to change vLLM's code, in a positive way to make their lives easier. **For vLLM users who use vLLM directly, there would be no breaking changes** except for beam-search. And we hope to bring better performance for users as well as an extensible architecture for developers.
👍React with 👍26Reacted by Sem Mulder, Simon Mo, Jee Jee Li, Woosuk Kwon, Robin, Edward Park, Wayne-Ho, SangBin Cho, Nikita Titov, Sanket Rai and 16 more
  3. [simon-mo](https://github.com/simon-mo)
pinned this issue [on Sep 24, 2024](https://github.com/vllm-project/vllm/issues/8779#event-14389876780)
  4. [simon-mo](https://github.com/simon-mo)
changed the title ~~[-][Draft] vLLM's V2 Engine Architecture[/-]~~ [+]vLLM's V2 Engine Architecture[/+] [on Sep 24, 2024](https://github.com/vllm-project/vllm/issues/8779#event-14389877712)
  5. ### noooop commented on Sep 24, 2024 
Last edited by noooop
Contributor
More actions
As vllm supports more and more models and functions, they require different attention, scheduler, executor, and input output processor. . These modules are becoming increasingly complex, and sometimes new features must be compromised for compatibility. ultimately leading to suboptimal results
Take support for encode only models as an example
Although the encode only models is much simpler than the decode model, they are very different.
The simplest way to support the encode only models is to implement different modules for models of different architectures and load the required modules on demand.
I call this architecture Workflow Defined Engine, or WDE for short.
PTAL [#8453](https://github.com/vllm-project/vllm/issues/8453) [#8452](https://github.com/vllm-project/vllm/pull/8452)
I'm implementing async scheduler (Async single-step scheduling). Beam search and SeqGroupMetadata drive me crazy. Awesome to hear about Beam Search and SeqGroupMetadata removed
  6. ### llllllooooox commented on Sep 24, 2024 
More actions
  7. ### noooop commented on Sep 27, 2024 
Contributor
More actions
Workflow Definition Engine draft pull request is almost complete and there are almost 10,000 lines of code.
as [@DarkLight1337](https://github.com/DarkLight1337) said:
> Hi, as mentioned earlier, there is basically no way we can merge all these changes at once. You should break up this refactoring into multiple stages.
Therefore, we hope to invite more people to participate, including but not limited to providing suggestions, participating in discussions, align with vLLM's V2 engine architecture goals, and discussing how to break it into stages, help review code for future PRs
Let me briefly introduce the content of this PR. Including
    1. what new models need to be supported,
    2. what new features these new models have, and
    3. how engine Architecture needs to support these features flexibly and efficiently.
### What new models need to be supported
These models are all from issues and are also very famous:
     * xlm_roberta
     * bge-m3
     * bge-reranker-v2-m3
     * bert
     * bge v1.5 family
     * Snowflake Arctic Embed (Family)
     * gte-Qwen2
     * This list is still growing
These models is roughly divided into three categories：
     * Encode only models. (Bidirectional Transformers, causal=False), Often fine-tuned as retriever and reranker etc.
     * Decode only models. (masked multi-head attention, causal=True). There are two interesting uses: 
       * Output last hidden states as a feature extractor
       * Decode only retriever （I don't know of a better name），E.g. e5-mistral-7b （The only Embed model currently supported by vllm)
       * Whether it has been fine-tuned or not, there is almost no difference in the code.
     * Enable bidirectional. [LLM2Vec](https://arxiv.org/abs/2404.05961) propose a simple unsupervised approach that can transform any decoder-only LLM into a strong text encoder.
### What new features these new models have
What the above three categories have in common is that there is only the prefill stage. In order to make the terminology more precise, prefill only is used below.
You can think of prefill only as encode only fancy writing.
New features:
    1. attention 
       * Prefill only models requires simpler attention implementations, no need to consider kvcache, no decoding phase
       * We need to support enable_bidirectional flag manually or read hf config automatically, enable bidirectional.
    2. scheduler 
       * Prefill only models requires simpler scheduler, no need to consider kvcache and preemption
       * Prefill only models, there is no correlation between tasks, so it is easy to implement async scheduling
    3. executer 
       * In order to support async scheduling, model_input_builder needs to be separated from the runner.
       * The main thread executes scheduling and all CPU processing, and the gpu thread only executes h2d, execution model, d2h
       * If async scheduling and async execution are implemented, data parallelism is also easy to implement. Data parallelism is more efficient for small models
### How engine Architecture needs to support these features flexibly and efficiently.
If we directly add new functions to existing modules, these modules are becoming increasingly complex, and sometimes new features must be compromised for compatibility. ultimately leading to suboptimal results
The most flexible and efficient way to support the prefill only models is to implement different modules for models of different architectures and load the required modules on demand.
I call this architecture Workflow Defined Engine, or WDE for short.
I divided the Engine into the following modules.
     * InputProcessor: The llm models inputs strings, the reranker inputs pairs, and the multimodal model input is more complex...
     * OutputProcessor: The retriever(embedding) models output embeddings, reranker models and classification models output Scores...
     * ModelInputBuilder: Building model inputs and attention metadata
     * AttnBackend: Support different AttnBackend and enable bidirectional
     * Tokenizer: There may be different tokenizers
     * Executor: Sync\Async\TP\PP\DP\maybe more
     * Worker & runner: Support different devices\maybe more
     * EngineArgs: Different models, different config may accept different parameters
     * maybe more
With wde, there is no need for one module to be compatible with all functions. You can use the dynamic loading feature of python to load different modules at the highest level, for different models and different needs.
     * Modules can be configured through Workflow, plug and play
     * Flexibly support plug-ins, and developers can load their own modules.
     * Workflow is really the best place to hide dirty codes. Some models cannot use the common Workflow. When you don’t know where to put the dirty code, you can always create a new workflow and link the model architecture to the new workflow to avoid leaving dirty code everywhere for the sake of compatibility.
PTAL [#8453](https://github.com/vllm-project/vllm/issues/8453) [#8452](https://github.com/vllm-project/vllm/pull/8452)
👍React with 👍4Reacted by Zhang Wenbin, EvanSong77, Lehua Ding and Iván Baldo
  8. ### Yang-x-Zhao commented on Sep 29, 2024 
More actions
Given Driver process + SPMD workers, it's there a chance to **separate LLMEngine process and worker processes on different nodes(servers)**? To be more concrete, the OpenAPI server process and LLMEngine process should live on a node with high performance CPU only, while the worker processes should live on normal GPU node(s).
I guess this idea is somehow related to ray spmd worker: [#6556](https://github.com/vllm-project/vllm/issues/6556), even though I suspect their current implementation is not supporting a distributed LLMEngine process.
  9. ### Venkat2811 commented on Oct 3, 2024 
More actions
> Lessons we learned from V1:
> To achieve high GPU utilization, we should care about everything happening on the CPU.
>      * Python is slow.
> Scheduling is not cheap.
>      * For every step, the vLLM scheduler goes over the whole self.running queue and performs some operations for each request (e.g., allocating a new block). And this is written in Python.
> Sampler is expensive.
>      * However, “pythonizing” the sampler outputs is expensive.
[@simon-mo](https://github.com/simon-mo) is the team considering moving away from python ?
👍React with 👍14Reacted by Maxime, Haiyang Huang, DreamGenX, Patrick Barker, hanifanggawi, Kai Wu, James Westbrook, HelloCard, Tao Li @ NVIDIA, Marcin Swiniarski and 4 more
👀React with 👀7Reacted by James Westbrook, Tao Li @ NVIDIA, 若可, 靳西, YangWeiliang_DeepNova@Deepexi, tsepten and joe
  10. ### yuki252111 commented on Oct 9, 2024 
More actions
  11. ### sleepwalker2017 commented on Nov 11, 2024 
Contributor
More actions
We notice that, when input lengths are short, for example less than 200, the prefill stages costs too much GPU idle. If python code is too slow to make gpu busy, can we take prefills for short sequences into cuda graph?
  12. [WoosukKwon](https://github.com/WoosukKwon)
changed the title ~~[-] vLLM's V2 Engine Architecture[/-]~~ [+]vLLM's V1 Engine Architecture[/+] [on Nov 13, 2024](https://github.com/vllm-project/vllm/issues/8779#event-15287012332)
  13. ### Stonesjtu commented on Dec 20, 2024 
Last edited by Stonesjtu
More actions
> Drop beam search from vLLM engine Provide a solution to emulate beam search outside vLLM engine
[@simon-mo](https://github.com/simon-mo) v1 seems a huge performance bumps in terms of sampling and multi-modality support.
However beam search provides flexibility for users who don't care overall speed, do we have the solution for a stand-alone beam search right now?
  14. ### kuangdao commented on Jan 13, 2025 
More actions
  15. ### renjie0 commented on Jan 23, 2025 
More actions
"Implement parallel sampling via prefix caching " What does it mean?
  16. [simon-mo](https://github.com/simon-mo)
unpinned this issue [on Jan 29, 2025](https://github.com/vllm-project/vllm/issues/8779#event-16117222211)
  17. ### github-actions commented on Apr 23, 2025 
[on Apr 23, 2025](https://github.com/vllm-project/vllm/issues/8779#issuecomment-2825978839) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue has been automatically marked as stale because it has not had any activity within 90 days. It will be automatically closed if no further activity occurs within 30 days. Leave a comment if you feel this issue should remain open. Thank you!
  18. [github-actions](https://github.com/apps/github-actions)
added 
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
[on Apr 23, 2025](https://github.com/vllm-project/vllm/issues/8779#event-17380998888)
  19. [DarkLight1337](https://github.com/DarkLight1337)
added 
[unstaleRecieved activity after being labelled stale](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22unstale%22)Recieved activity after being labelled stale
[keep-openPrevents stale label being applied](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22keep-open%22)Prevents stale label being applied
and removed 
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
[unstaleRecieved activity after being labelled stale](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22unstale%22)Recieved activity after being labelled stale
[on Apr 23, 2025](https://github.com/vllm-project/vllm/issues/8779#event-17381255633)
  20. ### jayxsinha commented on Jul 8, 2025 
More actions
I see Qwen3 Rerankers are [now supported on vllm](https://docs.vllm.ai/en/latest/models/supported_models.html#sentence-pair-scoring). Is there a way to modify the instruction used for these rerankers while using `vllm serve` like we do [here](https://github.com/vllm-project/vllm/blob/main/examples/offline_inference/qwen3_reranker.py#L61)?
👍React with 👍1Reacted by Iván Baldo
  21. ### DarkLight1337 commented on Jul 8, 2025 
Member
More actions
This is currently not handled inside vLLM, so you have to apply the template from client side.
  22. [wenhow](https://github.com/wenhow)
added a commit that references this issue [on Aug 4, 2026](https://github.com/vllm-project/vllm/issues/8779#event-28982764141)
[[BugFix] Fixed a bug in GQA C8 fullgraph mode. ([vllm-project#8779](https://github.com/vllm-project/vllm/issues/8779))](https://github.com/wenhow/vllm/commit/3583cfb03bc0f1e6880b61d2cd44644694e763d1)
Verified[3583cfb](https://github.com/wenhow/vllm/commit/3583cfb03bc0f1e6880b61d2cd44644694e763d1)


[Sign up for free](https://github.com/signup?return_to=https://github.com/vllm-project/vllm/issues/8779)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/vllm-project/vllm/issues/8779)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[keep-openPrevents stale label being applied](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22keep-open%22)Prevents stale label being applied
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
