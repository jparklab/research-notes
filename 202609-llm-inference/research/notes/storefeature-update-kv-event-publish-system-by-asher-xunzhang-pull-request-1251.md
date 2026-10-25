---
title: '[Store][Feature] Update KV event publish system by Asher-XunZhang · Pull Request
  #1251 · kvcache-ai/Mooncake · GitHub'
id: storefeature-update-kv-event-publish-system-by-asher-xunzhang-pull-request-1251
tags:
- llmd-kv-cache-routing-c665d5
- locus-offload-connector-routing-visibility
- mooncake
- kv-cache
- zmq-kv-events
- rejected-proposal
created: '2026-09-14T12:44:10.822745Z'
updated: '2026-09-15T04:55:24.131612Z'
source: https://github.com/kvcache-ai/Mooncake/pull/1251
source_domain: github.com
fetched_at: '2026-09-14T12:44:10.799334Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Mooncake PR #1251 (''[Store][Feature] Update KV event publish system,''
  opened Dec 22 2025 to align mooncake-store''s own KVEvent reporting with vLLM''s
  ZMQ mechanism, motivated by the Mooncake-Conductor RFC). Maintainer asked for it
  to be split into smaller PRs (Jan 2026); author split off PR #1306 for the interface
  declaration and deferred implementation. #1251 itself went stale and was auto-closed
  for inactivity Jul 18 2026 -- superseded, not merged. The capability it targeted
  later shipped via a different PR chain (see PR #4039/#4060).'
---

[Skip to content](https://github.com/kvcache-ai/Mooncake/pull/1251#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/pull/1251) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/pull/1251) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/pull/1251) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake) You must be signed in to change notification settings
  * [ Fork 1.2k ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake)
  * [ Star  6.6k ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake)


## Conversation
### 
commented [Dec 22, 2025](https://github.com/kvcache-ai/Mooncake/pull/1251#issue-3752339699) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Copy link  Copy Markdown 
## Description
motivation: [[RFC]: Mooncake-Conductor: Design and Implementation of a Global Scheduler Module for KV-Cache-Centric Disaggregated Architecture #977](https://github.com/kvcache-ai/Mooncake/issues/977)
Add a kv event publish system: it consists of events and an asynchronous sender, a kv event publisher based on ZMQ. Co-work with [@Liziqi-77](https://github.com/Liziqi-77) [@yejj710](https://github.com/yejj710)
### PR Overview
#### Summary of the KV Event System File Structure
Loading

```
classDiagram
    %% Event Class Hierarchy
    class KVCacheEvent {
        <<interface>>
        +pack(msgpack::packer~msgpack::sbuffer~) void
        +type_tag() string_view
    }
    
    KVCacheEvent <|-- BlockStoreEvent
    KVCacheEvent <|-- BlockUpdateEvent
    KVCacheEvent <|-- RemoveAllEvent
    
    %% Core Components
    class KVEventProducer {
        -shared_ptr~KVEventQueue~ event_queue_
        -unique_ptr~ThreadPool~ enqueue_pool_ : enqueue worker(1)
        
        +publish~Event, Args...~(args) future~bool~
        +publish_event_async(KVEventPtr) future~bool~
        +shutdown() void
    }
    
    class KVEventConsumer {
        -zmq::context_t context_
        -shared_ptr~KVEventQueue~ event_queue_
        -jthread publisher_thread_
        
        +KVEventConsumer(shared_ptr~KVEventQueue~, Config)
        +shutdown() void
        +is_running() bool
    }
    
    class ThreadSafeQueue~T~ {
        <<alias KVEventQueue>>
    }
    
    %% Facade Class
    class KVEventSystem {
        -shared_ptr~KVEventQueue~ event_queue_
        
        +KVEventSystem(const KVEventPublisherConfig&)
        +~KVEventSystem()
        +shutdown() void
        +is_running() bool
        +publish~Event, Args...~(args) future~bool~
    }
    
    %% Configuration and Event Classes
    class KVEventPublisherConfig {
    }
    class StoreEventInfo {
    }
    class EventBatch {
        +serialize() msgpack::sbuffer
    }
    
    %% Relationships
    KVEventSystem --> KVEventPublisherConfig : reads
    KVEventSystem --> KVEventProducer : contains
    KVEventSystem --> KVEventConsumer : contains
    KVEventSystem --> ThreadSafeQueue~KVEventPtr~ : contains
    
    KVEventProducer --> ThreadSafeQueue~KVEventPtr~ : enqueues
    KVEventConsumer --> ThreadSafeQueue~KVEventPtr~ : dequeues
    
    KVEventProducer --> KVCacheEvent : creates
    BlockStoreEvent --> StoreEventInfo : contains
    KVCacheEvent <.. EventBatch : aggregates
    EventBatch <.. KVEventConsumer : serialize & publish

```

  * mooncake-store/ 
    * `include/`
      * `kv_event/`
        * `kv_event.hpp`
          * **Purpose** ：Defines the base class and concrete implementations for all KV cache events
          * **Classes and Structs** ： 
            * `KVCacheEvent`：Abstract base class for events, defines serialization interface
            * `BlockStoreEvent`：Event for the first time KV cache is stored
            * `BlockUpdateEvent`：Event for any replica update operations
            * `RemoveAllEvent`：Event for clearing all cached data
            * `EventBatch`：Event batch class for improving transmission efficiency
          * **Details** ：Uses msgpack for serialization, supports event type identification, and includes `StoreEventInfo` metadata
        * `kv_event_types.h`
          * **Purpose** ：Provides unified type definitions and thread-safe queue implementation
          * **Classes and Structs** ： 
            * `QueuedItem`：Queue item struct, combines an event and a promise.
          * **Details** ：Uses concepts for type checking, defines type aliases `KVEventPtr` and `KVEventQueue`, implements thread-safe queue based on `ThreadSafeQueue`
        * `kv_event_producer.h`
          * **Purpose** ：Responsible for creating and publishing events
          * **Classes and Structs** ： 
            * `KVEventProducer`：Event producer class, manages event creation and queueing
          * **Details** ：Maintains a thread pool for asynchronous event queueing, implements a retry mechanism, tracks event types and statistics
        * `kv_event_consumer.h`
          * **Purpose** ：Consumes events from the queue and publishes them via ZeroMQ
          * **Classes and Structs** ： 
            * `KVEventConsumer`：Event consumer class, processes queued events and publishes them over the network
          * **Details** ：Uses ZeroMQ for network communication, supports event batching, maintains a replay buffer, implements automatic port selection
        * `kv_event_system.h`
          * **Purpose** ：Provides a simplified system interface, coordinating all components
          * **Classes and Structs** ： 
            * `KVEventSystem`：Event system facade class, hides internal complexity
          * **Details** ：Creates and configures all components, provides a unified publishing interface, aggregates statistics, supports graceful shutdown
        * `kv_event_publisher_config.h`
          * **Purpose** ：Defines all configuration parameters for the system
          * **Classes and Structs** ： 
            * `KVEventPublisherConfig`：System configuration struct
          * **Details** ：Contains configuration items for networking, performance, batching, thread pool, etc., and offers a configuration validation method
      * `kv_event_publisher.h`
        * **Purpose** ：Serves as the public API entry point for the KV event system, allowing external modules to include all necessary components at once
        * **Details** ：Aggregates and includes the 6 core header files from the `kv_event/` subdirectory
      * `thread_safe_queue.h`
        * **Purpose** ：Provides a thread-safe queue template class, implements the producer-consumer pattern for multi-threaded environments, supports bounded capacity and graceful shutdown mechanisms, optimized for MPSC (Multiple Producers, Single Consumer) scenarios
        * **Details** ： 
          * `ThreadSafeQueue<T>`：Core thread-safe queue template class, provides blocking/timeout push/pop operations, batch processing, queue state query, and graceful shutdown functionality, supporting flexible type for template parameter T
    * `src/`
      * `kv_event_publisher.cpp`
        * **Purpose** ：Core implementation file for the KV event publisher, responsible for the concrete implementation of configuration validation, event queue management, asynchronous event production, ZeroMQ event publishing, and replay services for the KV event system


#### System Architecture Relationships
Loading

```
sequenceDiagram
    participant App as Application
    participant ES as KVEventSystem
    participant EP as KVEventProducer
    participant TSQ as ThreadSafeQueue
    participant ZMQ as KVEventConsumer
    participant Network as ZeroMQNetwork
    
    App->>ES: 1. publish<KVEvent>(...)
    ES->>EP: 2. publish<KVEvent>(...)
    EP->>TSQ: 3. push(event)<br>(create KVCacheEvent object)
    Note over TSQ: KVEvent Buffer
    ZMQ->>TSQ: 4. peek_batch()
    ZMQ->>ZMQ: 5. batch processing
    ZMQ->>Network: 6. ZeroMQ Publish
    Network-->>ZMQ: 7. Publish confirmation
    ZMQ-->>TSQ: 8. pop_batch()
    ES-->>App: 9. return future
    
    Note over App,Network: Asynchronous processing flow

```

  * Data Flow： 
    * Events are published via `KVEventSystem::publish`
    * `KVEventSystem`delegates to `KVEventProducer` to create and enqueue events
    * Events are placed into the `KVEventQueue`
    * `KVEventConsumer` consumes events from the queue, batches them, and publishes via `ZeroMQ`
  * Component Interaction： 
    * `KVEventSystem` acts as a facade class, coordinating all components
    * Producers and consumers communicate via the `KVEventQueue`
    * All components depend on the event types defined in `kv_event.hpp`
  * Implementation Features： 
    * Uses the same `ZeroMQ` implementation as the inference engine for high-performance network communication
    * Uses the same cross-language `msgpack` as the inference engine for serializing `kv-event` information


## Type of Change
  * Types 
    * Bug fix
    * New feature 
      * Transfer Engine
      * Mooncake Store
      * Mooncake EP
      * Integration
      * P2P Store
      * Python Wheel
    * Breaking change
    * CI/CD
    * Documentation update
    * Other


## How Has This Been Tested?
I've added 2 new UTs: kv_event_publisher_test and thread_safe_queue_test
## Checklist
  * I have performed a self-review of my own code.
  * I have updated the documentation.
  * I have added tests to prove my changes are effective.


Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
[Asher-XunZhang](https://github.com/Asher-XunZhang) requested review from [XucSh](https://github.com/XucSh), [stmatengss](https://github.com/stmatengss) and [ykwd](https://github.com/ykwd) as [code owners](https://github.com/kvcache-ai/Mooncake/blob/4c2b6a95f668b0e81200b7639d043cc1fd4226b8/.github/CODEOWNERS#L13) [December 22, 2025 05:05](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21696669346)
[github-actions](https://github.com/apps/github-actions) Bot added [ run-ci ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3Arun-ci) [ Store ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3AStore) labels [Dec 22, 2025](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21696670762)
### 
**[gemini-code-assist](https://github.com/apps/gemini-code-assist) Bot ** commented [Dec 22, 2025](https://github.com/kvcache-ai/Mooncake/pull/1251#issuecomment-3680485953)
Copy link  Copy Markdown 
Contributor  
| 
## Summary of Changes
Hello [@Asher-XunZhang](https://github.com/Asher-XunZhang), I'm Gemini Code Assist! I'm currently reviewing this pull request and will post my feedback shortly. In the meantime, here's a summary to help you and other reviewers quickly get up to speed! This pull request introduces a new, asynchronous KV event publishing system to the Mooncake Store, leveraging ZeroMQ for efficient and scalable event dissemination. The system is designed to provide real-time insights into key-value cache operations, such as block storage, updates, and removals, by publishing structured event data. It includes new data models for event information, a robust ZeroMQ-based publisher with configurable operational parameters, and integrates these capabilities deeply into the core client and master services. This enhancement aims to improve observability and enable external systems to react to changes within the Mooncake Store.
### Highlights
  * **New KV Event Publishing System** : Introduced a comprehensive KV event publishing system for Mooncake Store, enabling asynchronous publication of key-value cache events via ZeroMQ.
  * **ZeroMQ and MessagePack Integration** : Integrated `cppzmq` and `msgpack-cxx` libraries to facilitate efficient and structured event communication, including their installation from source in the `dependencies.sh` script.
  * **Event Data Structures** : Defined new C++ data structures, including `KVCacheEvent` (base class), `StoreEventInfo` (detailed block metadata), `BlockStoreEvent`, `BlockUpdateEvent`, `RemoveAllEvent`, and `EventBatch` for event serialization and handling.
  * **Asynchronous Event Publisher** : Implemented `ZmqEventPublisher` for asynchronous event handling, featuring a thread-safe queue, batch processing, configurable parameters (e.g., endpoint, batch size, send interval), and a replay mechanism.
  * **Core Service Integration** : Modified `Client`, `MasterClient`, `MasterService`, `PyClient`, `RealClient`, and `WrappedMasterService` to incorporate event publishing logic, allowing events to be published during key operations like `PutEnd`, `Remove`, and `BatchEvict`.
  * **Configuration and Metrics** : Added extensive configuration options for the KV event publisher within `MasterConfig` and related builder classes, along with new metrics for monitoring publisher activity and performance.
  * **New Unit Tests** : Included new unit tests (`kv_event_publisher_test` and `thread_safe_queue_test`) to ensure the reliability and performance of the new event publishing system and its underlying thread-safe queue.

🧠 **New Feature in Public Preview:** You can now enable **Memory** to help **Gemini Code Assist** learn from your team's feedback. This makes future code reviews more consistent and personalized to your project's style. **Click[here](https://codeassist.google/code-review/login) to enable Memory in your admin console.** **Using Gemini Code Assist** The full guide for Gemini Code Assist can be found on our [documentation page](https://developers.google.com/gemini-code-assist/docs/review-github-code), here are some quick tips. **Invoking Gemini** You can request assistance from Gemini at any point by creating a comment using either `/gemini <command>` or `@gemini-code-assist <command>`. Below is a summary of the supported commands on the current page.  
 | Feature  | Command  | Description  |  
| --- | --- | --- |  
| Code Review  | `/gemini review`  | Performs a code review for the current pull request in its current state.  |  
| Pull Request Summary  | `/gemini summary`  | Provides a summary of the current pull request in its current state.  |  
| Comment  | Responds in comments when explicitly tagged, both in pull request comments and review comments.  |  
| Help  | `/gemini help`  | Displays a list of available commands.  |  
**Customization**
To customize Gemini Code Assist for GitHub experience, repository maintainers can create a configuration file and/or provide a custom code review style guide (such as PEP-8 for Python) by creating and adding files to a `.gemini/` folder in the base of the repository. Detailed instructions can be found [here](https://developers.google.com/gemini-code-assist/docs/customize-gemini-behavior-github).
**Limitations & Feedback**
Gemini Code Assist may make mistakes. Please leave feedback on any instances where its feedback is incorrect or counter productive. You can react with 👍 and 👎 on [@gemini-code-assist](https://github.com/gemini-code-assist) comments. If you're interested in giving your feedback about your experience with Gemini Code Assist for Github and other Google products, sign up [here](https://google.qualtrics.com/jfe/form/SV_2cyuGuTWsEw84yG).
**You can also get AI-powered code generation, chat, as well as code reviews directly in the IDE at no cost with the[Gemini Code Assist IDE Extension](https://cloud.google.com/products/gemini/code-assist).**
## Footnotes
  1. Review the [Privacy Notices](https://policies.google.com/privacy), [Generative AI Prohibited Use Policy](https://policies.google.com/terms/generative-ai/use-policy), [Terms of Service](https://policies.google.com/terms), and learn how to configure Gemini Code Assist in GitHub [here](https://developers.google.com/gemini-code-assist/docs/customize-gemini-behavior-github). Gemini can make mistakes, so double check it and [use code with caution](https://support.google.com/legal/answer/13505487). [↩](https://github.com/kvcache-ai/Mooncake/pull/1251#user-content-fnref-1-34e0a8c4a21ec1019e1ba241f2cbc2a8)

 |  
[Asher-XunZhang](https://github.com/Asher-XunZhang) changed the title ~~[WIP] Update KV event publisher~~ [WIP] Update KV event publish system [Dec 22, 2025](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21696691297)
**[gemini-code-assist](https://github.com/apps/gemini-code-assist) Bot ** reviewed  [ Dec 22, 2025 ](https://github.com/kvcache-ai/Mooncake/pull/1251#pullrequestreview-3602686985)
[ View reviewed changes ](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c)
### 
**[gemini-code-assist](https://github.com/apps/gemini-code-assist) Bot ** left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Code Review
This pull request introduces a comprehensive KV event publishing system using ZMQ and MessagePack. The changes are extensive, adding new dependencies, configurations, C++ classes for events and the publisher, and integrating event publishing into the master service logic. The implementation includes a thread-safe queue, a thread pool for async operations, and a robust ZMQ publisher with features like batching and a replay buffer. The code is generally well-structured. However, I've identified some areas for improvement regarding security in the dependency installation script, maintainability in serialization code, and consistency in code comments. I've also found a minor memory leak in the new tests.
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/include/kv_event/kv_event.hpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-4a3b8beaa03143bc3efb07a50215eba544967d96908217adf7370d35eca1a6ba) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/include/kv_event/kv_event_publisher_config.h](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-ab72d938a0204fbc6f02c6d0851c7deee9d419bee20c6dc8eb711bf1590907d7) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/src/kv_event_publisher.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-afa3a651520205c828b3b392ade4192e78daa680ce246dc453bab1ee518e84b7) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/tests/kv_event_publisher_test.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-ccf32ecd83f4e6ebe07b7113e29e9d8082ef5ac97c04899c10f1d24e9e34b917) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
[stmatengss](https://github.com/stmatengss) requested review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) and [doujiang24](https://github.com/doujiang24) [December 22, 2025 05:46](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21697204861)
**Copilot** [started reviewing](https://github.com/kvcache-ai/Mooncake/sessions/33159c4a-20e4-4280-950a-881839830eb8 "View session") on behalf of [stmatengss](https://github.com/stmatengss) [December 22, 2025 05:47](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21697211877) [ View session ](https://github.com/kvcache-ai/Mooncake/sessions/33159c4a-20e4-4280-950a-881839830eb8)
reviewed  [ Dec 22, 2025 ](https://github.com/kvcache-ai/Mooncake/pull/1251#pullrequestreview-3602769597)
[ View reviewed changes ](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
This PR introduces a KV event publishing system for Mooncake Store, implementing an asynchronous event publisher based on ZeroMQ with comprehensive batch processing capabilities. The system enables publishing of block store, block update, and remove events through a thread-safe queue mechanism.
**Key Changes:**
  * Implemented ZmqEventPublisher with async event publishing and batch processing
  * Added ThreadSafeQueue for MPSC (Multiple Producer Single Consumer) scenarios with timeout support
  * Integrated event publishing into master service operations (PutEnd, Remove, RemoveAll, eviction)


### Reviewed changes
Copilot reviewed 28 out of 28 changed files in this pull request and generated 14 comments.
Show a summary per file  
| File  | Description  |  
| --- | --- |  
| mooncake-store/tests/thread_safe_queue_test.cpp  | Comprehensive unit tests for ThreadSafeQueue with 1223 lines covering basic operations, timeouts, batch operations, MPSC scenarios, and stress tests  |  
| mooncake-store/tests/kv_event_publisher_test.cpp  | Unit tests for ZmqEventPublisher covering serialization, concurrent publishing, graceful shutdown, and performance  |  
| mooncake-store/tests/CMakeLists.txt  | Added test targets for thread_safe_queue_test and kv_event_publisher_test with ZMQ/msgpack dependencies  |  
| mooncake-store/src/kv_event_publisher.cpp  | Core implementation of ZmqEventPublisher with 740 lines including smart port binding, batch sending, and replay functionality  |  
| mooncake-store/src/master_service.cpp  | Integrated event publisher into master service lifecycle and key operations (PutEnd, Remove, eviction)  |  
| mooncake-store/src/rpc_service.cpp  | Updated RPC wrappers to propagate StoreEventInfo through PutEnd and BatchPutEnd calls  |  
| mooncake-store/src/real_client.cpp  | Added StoreEventInfo parameter to batch_put_from_multi_buffers methods  |  
| mooncake-store/src/master.cpp  | Added command-line flags and configuration loading for KV event publisher (lines 104-650)  |  
| mooncake-store/src/client_service.cpp  | Updated BatchPut flow to create and propagate key-event info mappings  |  
| mooncake-store/src/CMakeLists.txt  | Added cppzmq and msgpack-cxx dependencies to mooncake_store library  |  
| mooncake-store/include/thread_safe_queue.h  | Header-only implementation of ThreadSafeQueue with blocking/non-blocking operations and batch pop support  |  
| mooncake-store/include/thread_pool.h  | Added header guards to thread pool header  |  
| mooncake-store/include/kv_event_publisher.h  | Main header aggregating event, config, and publisher headers  |  
| mooncake-store/include/kv_event/kv_event_zmq_publisher.h  | ZmqEventPublisher class declaration with stats, async publishing, and resource management  |  
| mooncake-store/include/kv_event/kv_event_publisher_config.h  | Configuration structure for event publisher with network, performance, and batching parameters  |  
| mooncake-store/include/kv_event/kv_event.hpp  | Event type definitions (BlockStoreEvent, BlockUpdateEvent, RemoveAllEvent) and EventBatch with msgpack serialization  |  
| mooncake-store/include/master_service.h  | Added publisher member, GetPublisherStats method, and GetReplicasDescriptorList helper  |  
| mooncake-store/include/master_config.h  | Added enable_kv_event_publish and kv_event_publisher_config to all config classes  |  
| mooncake-store/include/master_client.h  | Updated PutEnd and BatchPutEnd signatures to accept StoreEventInfo  |  
| mooncake-store/include/rpc_service.h  | Updated RPC method signatures with StoreEventInfo parameters  |  
| mooncake-store/include/pyclient.h  | Added StoreEventInfo parameter to batch_put_from_multi_buffers interface  |  
| mooncake-store/include/real_client.h  | Updated batch_put_from_multi_buffers signatures  |  
| mooncake-store/include/dummy_client.h  | Updated batch_put_from_multi_buffers signature for consistency  |  
| mooncake-store/include/client_service.h  | Added CreateKeyEventInfosMap and updated BatchPut/FinalizeBatchPut signatures  |  
| mooncake-integration/store/store_py.cpp  | Added Python binding for StoreEventInfo class  |  
| dependencies.sh  | Added libzmq3-dev system package and installation scripts for cppzmq and msgpack-cxx  |  
💡 [Add Copilot custom instructions](https://github.com/kvcache-ai/Mooncake/new/main/.github/instructions?filename=*.instructions.md) for smarter, more guided reviews. [Learn how to get started](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).
Positive feedbackNegative feedback
Comment thread [mooncake-store/tests/thread_safe_queue_test.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-4dacac53f46c24e0695589d300de380404b03462e37cc6678a0d351345ed7480) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/tests/thread_safe_queue_test.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-4dacac53f46c24e0695589d300de380404b03462e37cc6678a0d351345ed7480) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/tests/thread_safe_queue_test.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-4dacac53f46c24e0695589d300de380404b03462e37cc6678a0d351345ed7480) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/tests/thread_safe_queue_test.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-4dacac53f46c24e0695589d300de380404b03462e37cc6678a0d351345ed7480) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/tests/kv_event_publisher_test.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-ccf32ecd83f4e6ebe07b7113e29e9d8082ef5ac97c04899c10f1d24e9e34b917) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/tests/thread_safe_queue_test.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-4dacac53f46c24e0695589d300de380404b03462e37cc6678a0d351345ed7480) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/tests/kv_event_publisher_test.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-ccf32ecd83f4e6ebe07b7113e29e9d8082ef5ac97c04899c10f1d24e9e34b917) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/src/master.cpp](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-5994a3a5ec6111b1656c5b1fea0332c5b622c72855f4bbb09123843735466cf5) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread [mooncake-store/include/kv_event/kv_event_zmq_publisher.h](https://github.com/kvcache-ai/Mooncake/pull/1251/files/b0289e06e91f33b9cd5745873159fde0a533c77c#diff-a152dc628e7ccd896caad375a22d7ec4d83dbfb39de4304e7cf140333282f07b) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
[Asher-XunZhang](https://github.com/Asher-XunZhang) added 2 commits [December 22, 2025 20:33](https://github.com/kvcache-ai/Mooncake/pull/1251#commits-pushed-0303c1a)
` Fix some CI review suggestions[](https://github.com/kvcache-ai/Mooncake/pull/1251/commits/0303c1abaf9edb74fbca940a02c6292066ae7fad "Fix some CI review suggestions") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
` Merge remote-tracking branch 'upstream/main' into zmq_event_publisher[](https://github.com/kvcache-ai/Mooncake/pull/1251/commits/101c2af83c1acc3126e30f9f03e97e04317d1aea "Merge remote-tracking branch 'upstream/main' into zmq_event_publisher") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
[Asher-XunZhang](https://github.com/Asher-XunZhang) [force-pushed](https://github.com/kvcache-ai/Mooncake/compare/23629b9687517184f2020ca853da6707a2b9052d..101c2af83c1acc3126e30f9f03e97e04317d1aea) the  zmq_event_publisher branch from [`23629b9`](https://github.com/kvcache-ai/Mooncake/commit/23629b9687517184f2020ca853da6707a2b9052d) to [`101c2af`](https://github.com/kvcache-ai/Mooncake/commit/101c2af83c1acc3126e30f9f03e97e04317d1aea) [ Compare ](https://github.com/kvcache-ai/Mooncake/compare/23629b9687517184f2020ca853da6707a2b9052d..101c2af83c1acc3126e30f9f03e97e04317d1aea) [December 22, 2025 12:55](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21704288907)
[Asher-XunZhang](https://github.com/Asher-XunZhang) changed the title ~~[WIP] Update KV event publish system~~ [WIP][Store][Feature] Update KV event publish system [Dec 23, 2025](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21719662502)
[Asher-XunZhang](https://github.com/Asher-XunZhang) mentioned this pull request [ Dec 23, 2025 ](https://github.com/kvcache-ai/Mooncake/pull/1251#ref-issue-3561134754)
[ [RFC]: Mooncake-Conductor: Design and Implementation of a Global Scheduler Module for KV-Cache-Centric Disaggregated Architecture #977 ](https://github.com/kvcache-ai/Mooncake/issues/977)
Closed 
1 task
[Asher-XunZhang](https://github.com/Asher-XunZhang) [force-pushed](https://github.com/kvcache-ai/Mooncake/compare/8fd7e1edc0582d5c00d7291f0f994db010e9a6c5..67043c1cfe4551215fa188af2d2f061176faa867) the  zmq_event_publisher branch from [`8fd7e1e`](https://github.com/kvcache-ai/Mooncake/commit/8fd7e1edc0582d5c00d7291f0f994db010e9a6c5) to [`67043c1`](https://github.com/kvcache-ai/Mooncake/commit/67043c1cfe4551215fa188af2d2f061176faa867) [ Compare ](https://github.com/kvcache-ai/Mooncake/compare/8fd7e1edc0582d5c00d7291f0f994db010e9a6c5..67043c1cfe4551215fa188af2d2f061176faa867) [December 23, 2025 07:27](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21719921161)
` Merge remote-tracking branch 'upstream/main' into zmq_event_publisher[](https://github.com/kvcache-ai/Mooncake/pull/1251/commits/281ac833f593e63d7b56a354c9cf4c85fd2e4c04 "Merge remote-tracking branch 'upstream/main' into zmq_event_publisher") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
[Asher-XunZhang](https://github.com/Asher-XunZhang) [force-pushed](https://github.com/kvcache-ai/Mooncake/compare/b5a750f1c0d657f285f3f6dd7171274d655ca41f..5b4f29cfa86770bffd9c8835cf49c0a30b2966d7) the  zmq_event_publisher branch 2 times, most recently from [`b5a750f`](https://github.com/kvcache-ai/Mooncake/commit/b5a750f1c0d657f285f3f6dd7171274d655ca41f) to [`5b4f29c`](https://github.com/kvcache-ai/Mooncake/commit/5b4f29cfa86770bffd9c8835cf49c0a30b2966d7) [ Compare ](https://github.com/kvcache-ai/Mooncake/compare/b5a750f1c0d657f285f3f6dd7171274d655ca41f..5b4f29cfa86770bffd9c8835cf49c0a30b2966d7) [December 24, 2025 09:02](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21738842546)
` Merge remote-tracking branch 'upstream/main' into zmq_event_publisher[](https://github.com/kvcache-ai/Mooncake/pull/1251/commits/600a29f7f94770f627b226e6cf2b93bd4563838a "Merge remote-tracking branch 'upstream/main' into zmq_event_publisher") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
[Asher-XunZhang](https://github.com/Asher-XunZhang) [force-pushed](https://github.com/kvcache-ai/Mooncake/compare/ef997de90118139000202a08dcf6367969c52455..f241a705c9c8fb9663f3fdcd8ee062a9b635c69f) the  zmq_event_publisher branch 2 times, most recently from [`ef997de`](https://github.com/kvcache-ai/Mooncake/commit/ef997de90118139000202a08dcf6367969c52455) to [`f241a70`](https://github.com/kvcache-ai/Mooncake/commit/f241a705c9c8fb9663f3fdcd8ee062a9b635c69f) [ Compare ](https://github.com/kvcache-ai/Mooncake/compare/ef997de90118139000202a08dcf6367969c52455..f241a705c9c8fb9663f3fdcd8ee062a9b635c69f) [December 25, 2025 12:06](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21750595736)
` Merge remote-tracking branch 'upstream/main' into zmq_event_publisher[](https://github.com/kvcache-ai/Mooncake/pull/1251/commits/cff97a9492b19b72bc3eb956b5be2dcb8b429c93 "Merge remote-tracking branch 'upstream/main' into zmq_event_publisher") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
[Asher-XunZhang](https://github.com/Asher-XunZhang) [force-pushed](https://github.com/kvcache-ai/Mooncake/compare/f241a705c9c8fb9663f3fdcd8ee062a9b635c69f..cff97a9492b19b72bc3eb956b5be2dcb8b429c93) the  zmq_event_publisher branch from [`f241a70`](https://github.com/kvcache-ai/Mooncake/commit/f241a705c9c8fb9663f3fdcd8ee062a9b635c69f) to [`cff97a9`](https://github.com/kvcache-ai/Mooncake/commit/cff97a9492b19b72bc3eb956b5be2dcb8b429c93) [ Compare ](https://github.com/kvcache-ai/Mooncake/compare/f241a705c9c8fb9663f3fdcd8ee062a9b635c69f..cff97a9492b19b72bc3eb956b5be2dcb8b429c93) [December 25, 2025 12:23](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21750779007)
[Asher-XunZhang](https://github.com/Asher-XunZhang) changed the title ~~[WIP][Store][Feature] Update KV event publish system~~ [Store][Feature] Update KV event publish system [Dec 26, 2025](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21754019621)
` Merge remote-tracking branch 'upstream/main' into zmq_event_publisher[](https://github.com/kvcache-ai/Mooncake/pull/1251/commits/dc597bd56c8d32476c5d66fa6558d91efc13d7c5 "Merge remote-tracking branch 'upstream/main' into zmq_event_publisher") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/1251).
[Asher-XunZhang](https://github.com/Asher-XunZhang) [force-pushed](https://github.com/kvcache-ai/Mooncake/compare/cd5cf89e630aed40667b8abf4edf1f9eb2a9b58b..dc597bd56c8d32476c5d66fa6558d91efc13d7c5) the  zmq_event_publisher branch from [`cd5cf89`](https://github.com/kvcache-ai/Mooncake/commit/cd5cf89e630aed40667b8abf4edf1f9eb2a9b58b) to [`dc597bd`](https://github.com/kvcache-ai/Mooncake/commit/dc597bd56c8d32476c5d66fa6558d91efc13d7c5) [ Compare ](https://github.com/kvcache-ai/Mooncake/compare/cd5cf89e630aed40667b8abf4edf1f9eb2a9b58b..dc597bd56c8d32476c5d66fa6558d91efc13d7c5) [December 27, 2025 04:56](https://github.com/kvcache-ai/Mooncake/pull/1251#event-21763677599)
### 
commented [Jan 7, 2026](https://github.com/kvcache-ai/Mooncake/pull/1251#issuecomment-3719626924)
Copy link  Copy Markdown 
Collaborator  
|  [@Asher-XunZhang](https://github.com/Asher-XunZhang), Could you split this PR into smaller PRs? I will assign reviewers for expedited review.  |  
| --- |  
### 
commented [Jan 8, 2026](https://github.com/kvcache-ai/Mooncake/pull/1251#issuecomment-3721428485)
Copy link  Copy Markdown 
Author  
| 
> [@Asher-XunZhang](https://github.com/Asher-XunZhang), Could you split this PR into smaller PRs? I will assign reviewers for expedited review.
[@stmatengss](https://github.com/stmatengss) No problem, I have split this PR. This is the first PR for the interface declaration: ([#1306](https://github.com/kvcache-ai/Mooncake/pull/1306)). The PRs for other implementations and related dependencies need to wait until this PR is merged; otherwise, it will cause CI compilation to fail.  |  
| --- |  
[Asher-XunZhang](https://github.com/Asher-XunZhang) mentioned this pull request [ Jan 20, 2026 ](https://github.com/kvcache-ai/Mooncake/pull/1251#ref-issue-3831835608)
[ [RFC]: Mooncake Store KV Event Integration Solution #1408 ](https://github.com/kvcache-ai/Mooncake/issues/1408)
Closed 
1 task
### 
commented [Jun 17, 2026](https://github.com/kvcache-ai/Mooncake/pull/1251#issuecomment-4725842243)
Copy link  Copy Markdown   
|  This PR has been automatically marked as stale because it has not had recent activity. It will be closed in 14 days if no further activity occurs. Please update the PR or comment to keep it open.  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added the [ stale ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3Astale) label [Jun 17, 2026](https://github.com/kvcache-ai/Mooncake/pull/1251#event-26841074535)
### 
commented [Jul 18, 2026](https://github.com/kvcache-ai/Mooncake/pull/1251#issuecomment-5009767641)
Copy link  Copy Markdown   
|  This PR was closed because it has been inactive for 104 days. Feel free to reopen if you'd like to continue the work.  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot closed this [Jul 18, 2026](https://github.com/kvcache-ai/Mooncake/pull/1251#event-28150482184)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fkvcache-ai%2FMooncake%2Fpull%2F1251)
###  Labels 
[ run-ci ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3Arun-ci) [ stale ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3Astale) [ Store ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3AStore)
###  3 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
