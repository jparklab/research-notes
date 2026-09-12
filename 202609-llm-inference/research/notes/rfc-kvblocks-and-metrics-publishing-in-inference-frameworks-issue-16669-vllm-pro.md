---
title: '[RFC]: KVBlocks and Metrics Publishing In Inference Frameworks · Issue #16669
  · vllm-project/vllm'
id: rfc-kvblocks-and-metrics-publishing-in-inference-frameworks-issue-16669-vllm-pro
tags:
- llmd-router-nonk8s-backends
- zmq-kv-events
- vllm-internals
- design-history
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:40.260961Z'
updated: '2026-09-12T03:37:37.580653Z'
source: https://github.com/vllm-project/vllm/issues/16669
source_domain: github.com
fetched_at: '2026-09-11T19:44:40.238419Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Closed vLLM RFC (#16669, opened Apr 15 2025 by alec-flowers/NVIDIA-Dynamo,
  closed "completed" Nov 7 2025) is the design-history record for the KVEvents publishing
  mechanism llm-d now depends on. Original proposal wanted to hook a pluggable KVBlockObserver
  directly into AsyncLLM''s constructor for in-process event injection, paralleling
  the existing StatsLoggerBase pattern for metrics. Discussion (markmc, ApostaC, ai-dynamo''s
  alec-flowers) rejected the EngineCoreEvent path as too heavyweight/critical-path,
  debated whether the existing KV Connector API (used for actual KV-tensor transfer,
  ~100MB/s with retry/ack) was the right hook point, and converged on a separate,
  decoupled mechanism: alec-flowers proposed introducing "a kv_event_config and kv_event_sink
  that the Scheduler instantiates next to the KV Connector," explicitly contrasting
  it with the Connector: "KV-Transfer handles 100MB/s tensor w/ retry and ack... KV
  Event publishing is low bandwidth publishing, pub/sub w/ fire and forget." This
  is the design that shipped as ZmqEventPublisher in vllm/distributed/kv_events.py.
  Thread also surfaces that NVIDIA Dynamo''s own kv_router (github.com/ai-dynamo/dynamo,
  lib/llm/src/kv_router/publisher.rs and indexer.rs) predates and parallels vLLM''s
  native KVEvents, and that vMaroon (IBM Research, later author of the llm-d Red Hat
  blog) explicitly frames this as complementary to Connector-API-based "external KV
  locality" awareness rather than a replacement -- i.e., KVEvents give internal-state
  visibility, Connector API gives external-storage-locality visibility, and IBM''s
  roadmap combines both for fine-grained KV-aware routing. Confirms storage/medium
  (GPU/CPU/remote like S3 or Redis) was explicitly discussed as a future BlockStored
  extension point (KuntaiDu''s question about Redis-backed KV cache), materializing
  later as the `medium` field (GPU/CPU/STORAGE) in the shipped BlockStored dataclass.'
utility_score: 11.0
---

[Skip to content](https://github.com/vllm-project/vllm/issues/16669#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/16669) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/16669) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/16669) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/issues/16669).
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
  * [ Fork 22.1k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm)
  * [ Star  91.5k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm)


#  [RFC]: KVBlocks and Metrics Publishing In Inference Frameworks #16669
Copy link
Copy link
Closed
Closed
[[RFC]: KVBlocks and Metrics Publishing In Inference Frameworks](https://github.com/vllm-project/vllm/issues/16669#top)#16669
Copy link
Labels
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
## Description
opened [on Apr 15, 2025](https://github.com/vllm-project/vllm/issues/16669#issue-2996820700)
Contributor
Issue body actions
### Motivation.
To do effective planning and routing for distributed LLM inference, we need mechanisms to publish information about the state of the inference engine to other pieces of the distributed system.
Examples include:
  * Publishing events when blocks are added and removed from the local prefix tree so that a router can create a global prefix tree with data from all the workers.
  * Publishing real-time metrics so a router can load balance between workers if a worker is starting to queue requests.


### Proposed Change.
## Metrics
In vLLM there is already a methodology to publish Metrics using classes that inherit StatsLoggerBase. For example PrometheusStatLogger. We would add a method to relevant engine classes which would accept 3rd party StatLoggers that conform to the StatsLoggerBase interface.
In this way an inference framework that wraps vLLM (such as dynamo) has a way to inject custom logic for publishing Metric events.

```
def add_stats_logger(self, name, logger: StatsLoggerBase):
	self.engine.add_logger(name, logger)
```

or

```
class AsyncLLM(EngineClient):

    def __init__(
        self,
        vllm_config: VllmConfig,
        executor_class: type[Executor],
        log_stats: bool,
        usage_context: UsageContext = UsageContext.ENGINE_CONTEXT,
        mm_registry: MultiModalRegistry = MULTIMODAL_REGISTRY,
        use_cached_outputs: bool = False,
        log_requests: bool = True,
        start_engine_loop: bool = True,
        stat_loggers: Optional[list[StatLoggerBase]] = None,
    ) -> None:
```

## KVEvents
To publish KVEvents requires a larger change as we need to introduce new KVEvent classes and add new code to the BlockPool class in order to publish these events. Because we manage the BlockPool in a separate process from the client, we will need to use zmq to send these events back to the client where a 3rd party inference framework can then inject a `KVBlockObserver` to publish events.

```
# Classes to add, need to be serializable msgspec

@dataclass
class KVCacheEvent:
    """Base class for all KV cache-related events"""
    pass

@dataclass
class BlockStored(KVCacheEvent):
    block_hashes: List[int]
    parent_block_hash: Optional[int]
    token_ids: List[int]
    num_toks_per_block: List[int]
    lora_id: int

@dataclass
class BlockRemoved(KVCacheEvent):
    block_hashes: List[int]

@dataclass
class AllBlocksCleared(KVCacheEvent):
    """Event signaling that the entire prefix cache has been reset."""
    pass

class KVBlockObserver:
def observe(self, event: KVCacheEvent)
 		pass
```

In vLLM V1 we would hook into the BlockPool class and this is where we would form events.

```
def cache_full_blocks # BlockStored
def _maybe_evict_cached_block(self, block: KVCacheBlock) # BlockRemoved 
def reset_prefix_cache(self) # AllBlocksCleared
```

In the client, we would be able to inject a KVBlockObserver that would be responsible for observing the events from the engine process, and then publishing them.

```
def add_kv_block_observer(self, name, logger: KVBlockObserver):
	
```

or

```
class AsyncLLM(EngineClient):

    def __init__(
        self,
        vllm_config: VllmConfig,
        executor_class: type[Executor],
        log_stats: bool,
        usage_context: UsageContext = UsageContext.ENGINE_CONTEXT,
        mm_registry: MultiModalRegistry = MULTIMODAL_REGISTRY,
        use_cached_outputs: bool = False,
        log_requests: bool = True,
        start_engine_loop: bool = True,
        stat_loggers: Optional[list[StatLoggerBase]] = None,
        kv_block_observer: Optional[list[KVBlockObserver]] = None,
    ) -> None:
```

### Feedback Period.
_No response_
### CC List.
[@pavanimajety](https://github.com/pavanimajety) [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat)
### Any Other Things.
_No response_
### Before submitting a new issue...
  * Make sure you already searched for relevant issues, and asked the chatbot living at the bottom right corner of the [documentation page](https://docs.vllm.ai/en/latest/), which can answer lots of frequently asked questions.
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


👍React with 👍10Reacted by Robert Shaw, rongfu.leng, Shangming Cai, Graham King, Mingxing Zhang, Wenlong Wang, Gene Der Su, Chauncey, Sylvia505 and Richard Joy
## Activity
[alec-flowers](https://github.com/alec-flowers)
added [on Apr 15, 2025](https://github.com/vllm-project/vllm/issues/16669#event-17271730530)
### KuntaiDu commented on Apr 16, 2025 
Collaborator
More actions
Hi [@alec-flowers](https://github.com/alec-flowers) , interesting design! Just wondering how does this design handle the case where the KV cache is stored in a remote storage, like Reddis?
👍React with 👍1Reacted by Jiayi Yao
### alec-flowers commented on Apr 16, 2025 
ContributorAuthor
More actions
Hey [@KuntaiDu](https://github.com/KuntaiDu), this design is currently agnostic of storage. This first pass is just communicating internal block state externally. If we were to add storage, there would be some component (like the BlockPool / BlockManger) that would need to manage the component and the transfer of KVCache. We could just treat the storage as extra KVCache and emit the same BlockStored event. This is notifying externally that hey, we have this block in cache and we are ok to use it.
The goal of this is so that a remote scheduler can know what the local state of the KVCache is and use that knowledge to schedule more effectively.
Eventually, the state of the block and where it is stored will matter in a routing calculation. For example the choice to recompute vs. route to an engine that could retrieve from an S3 bucket. In that case we could augment the BlockStored API with something like storage_type, or maybe even some metadata related to bandwidth or speed at which the block could be brought to the GPU and used for a forward pass.
We have someone working on a global block manager and the idea would be it would be this global block manager that emits events as the blocks change state (being used, offloaded to disk or x storage).
For this PR I am trying to make the first steps.
### vMaroon commented on Apr 17, 2025 
Last edited by vMaroon
Contributor
More actions
I think this is an important gap to patch in this agentic year. In the current state, through the Connector API, at best one can implement lossy indexing that targets locality in availability for the case Kuntai outlined.
Once through, IMO it will make sense to think about how KVCache engines can emit events as well.
### simon-mo commented on Apr 17, 2025 
Collaborator
More actions
cc [@comaniac](https://github.com/comaniac) [@WoosukKwon](https://github.com/WoosukKwon) [@heheda12345](https://github.com/heheda12345)
### ApostaC commented on Apr 17, 2025 
Collaborator
More actions
Since [#15960](https://github.com/vllm-project/vllm/pull/15960) is merged, I'm wondering does this PR's scope also includes the KVCache-related events in external KV cache storage. If so, should there be some standard APIs to create those KV cache events outside vLLM's BlockPool?
👍React with 👍1Reacted by Maroon Ayoub
### markmc commented on Apr 18, 2025 
Member
More actions
Hi [@alec-flowers](https://github.com/alec-flowers) !
I'd love to see the consumer side of this to better understand the use case ... is that pushed somewhere?
`EngineCoreEvent` is currently an extremely lightweight means (because it's on the critical path) of transmitting quite coarse-grained (queued, scheduled, preempted, new tokens) per-request timestamps from the engine core to the frontend, so the frontend can compute the various intervals and then record aggregated metrics.
`KVCacheEvent` seems to be more about allowing an external actor to hook into the internal state of the KV cache so it can synchronize with it, to generate routing metadata rather than metrics? It's more heavyweight, quite different in scope, unlikely to be enabled by default, probably still experimental and likely to change, etc. I'm not sure I'd follow the `EngineCoreEvent` pattern for this.
As [@ApostaC](https://github.com/ApostaC) said - the new KV connector API seems like a much closer fit conceptually? A way to hook deep into the KV cache with a variety of different plugins. Even if there is no actual KV cache transfer happening, the same hook point could probably externalize metadata about a non-distributed KV cache?
### alec-flowers commented on Apr 18, 2025 
ContributorAuthor
More actions
Hey [@markmc](https://github.com/markmc),
Here is our publisher (this has python bindings): <https://github.com/ai-dynamo/dynamo/blob/main/lib/llm/src/kv_router/publisher.rs>
Here is our consumer (also has python bindings): <https://github.com/ai-dynamo/dynamo/blob/main/lib/llm/src/kv_router/indexer.rs>
The consumer is a global prefix tree that allows you to match requests against and output how many blocks each worker matched.
I also agree with you that the EngineCoreEvent path may not be right. I had at first considered opening my own ZMQ sockets to send events but that seemed like a bigger change. One key problem is for a third party to inject a runtime object, we need to be in the same process. That is why I ended up tacking this stuff on to the EngineCoreEvent as this is the main path we get things from the engine process to the client.
Let me take a look at the Connector and and see if I can mock something up there.
Thanks for the feedback!
### vMaroon commented on Apr 18, 2025 
Last edited by vMaroon
Contributor
More actions
Hi all, adding some thoughts:
The information exposed through the Connector API (or more precisely, by the engines that implement it) reflects awareness of offloaded KVCache, which enables _external_ locality understanding - but not visibility into the internal KV state of the vLLM engine.
External KV locality is just one side of the coin. I think the proposal here covers the other side, completing the KVCache-awareness picture. An indexing API is very much in line with what we’ve been exploring at IBM Research, based on the idea that combining vLLM-internal insights with node-local indexing is key to enabling fine-grained, KV-aware request routing.
It makes sense to start with the two pieces decoupled, but streamlining the API is probably a necessary followup.
### alec-flowers commented on Apr 21, 2025 
Last edited by alec-flowers
ContributorAuthor
More actions
[@markmc](https://github.com/markmc) I agree that EngineCoreEvent is wrong because its in the critical path. I do think the KV Connector API is the right location (scheduler-side) to surface cache state, but I'm not sure its the right abstraction because the current contract forces you to choose a single connector class and these two APIs have different goals.
  * KV-Transfer handles 100MB/s tensor w/ retry and ack, and CPU -> GPU transfer
  * KV Event publishing is low bandwidth publishing, pub/sub w/ fire and forget (we can add a simple ack)


What do you think about introducing a kv_event_config and kv_event_sink that the Scheduler instantiates next to the KV Connector? The publishing would then look something like this in the schedule method:

```
if self.enable_kv_cache_events and self.kv_event_sink:
     events = self.kv_event_manager.take_events()
     self.kv_event_sink.publish(events)
```

This means no change to the Worker, the BlockPool code stays the same and gated by enable_kv_cache_events.
If you are ok with this I can add a minimal KVEventSink (abc + factory ~40 LOC) with a concrete implementation of a ZMQEventSink (pub/sub).
### github-actions commented on Jul 20, 2025 
[on Jul 20, 2025](https://github.com/vllm-project/vllm/issues/16669#issuecomment-3095038926) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue has been automatically marked as stale because it has not had any activity within 90 days. It will be automatically closed if no further activity occurs within 30 days. Leave a comment if you feel this issue should remain open. Thank you!
[github-actions](https://github.com/apps/github-actions)
added 
[Over 90 days of inactivity](https://github.com/vllm-project/vllm/issues?q=state%3Aopen%20label%3A%22stale%22)Over 90 days of inactivity
[on Jul 20, 2025](https://github.com/vllm-project/vllm/issues/16669#event-18719488787)
### github-actions commented on Aug 20, 2025 
[on Aug 20, 2025](https://github.com/vllm-project/vllm/issues/16669#issuecomment-3208778231) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue has been automatically closed due to inactivity. Please feel free to reopen if you feel it is still relevant. Thank you!
[github-actions](https://github.com/apps/github-actions)
closed this as [not planned](https://github.com/vllm-project/vllm/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Anot-planned)[on Aug 20, 2025](https://github.com/vllm-project/vllm/issues/16669#event-19256978416)
### markmc commented on Nov 7, 2025 
Member
More actions
This:
> a way to inject custom logic for publishing Metric events.
is possible with a custom `StatLogger` AIUI. But it probably didn't make sense to include in this issue anyway
This KVEvents part was resolved by [#16669](https://github.com/vllm-project/vllm/issues/16669)
[markmc](https://github.com/markmc)
closed this as [completed](https://github.com/vllm-project/vllm/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Nov 7, 2025](https://github.com/vllm-project/vllm/issues/16669#event-20781153634)
[Sign up for free](https://github.com/signup?return_to=https://github.com/vllm-project/vllm/issues/16669)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/vllm-project/vllm/issues/16669)
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
