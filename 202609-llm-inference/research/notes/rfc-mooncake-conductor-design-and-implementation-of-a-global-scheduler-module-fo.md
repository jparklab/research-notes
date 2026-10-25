---
title: '[RFC]: Mooncake-Conductor: Design and Implementation of a Global Scheduler
  Module for KV-Cache-Centric Disaggregated Architecture · Issue #977 · kvcache-ai/Mooncake'
id: rfc-mooncake-conductor-design-and-implementation-of-a-global-scheduler-module-fo
tags:
- llmd-router-nonk8s-backends-54b774
- llmd-router
- mooncake-conductor
- rejected-proposal
- non-k8s-backend
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T20:49:32.070859Z'
updated: '2026-09-15T04:55:23.854486Z'
source: https://github.com/kvcache-ai/Mooncake/issues/977
source_domain: github.com
fetched_at: '2026-09-11T20:49:32.043714Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'IMPORTANT PROVENANCE FLAG: this GitHub RFC (opened Oct 28 2025, kvcache-ai/Mooncake
  issue #977) was CLOSED AS ''NOT PLANNED'' -- it is an unmerged, rejected proposal,
  not shipped Mooncake functionality. Treat every claim below as a design sketch only,
  not a description of production behavior. That said, it is architecturally the clearest
  published articulation of a backend-agnostic global scheduler for KV-cache-centric
  disaggregated serving, directly relevant to a non-k8s-pod llm-d-style router. The
  RFC proposes ''Mooncake-Conductor,'' a global scheduler realizing a three-layer
  ''sandwich'' architecture: Top (Scheduling Layer) = Conductor as an independent
  process/service acting as cluster entry point, deliberately not performing computation
  itself; Middle (Compute Layer) = inference engines (vLLM, SGLang) that are ''agnostic
  to global resource distribution... other nodes or the global location of the KVCache''
  -- i.e. compute nodes need not be Kubernetes pods, they are addressed as opaque
  ''instances''; Bottom (Storage/Data Plane) = Mooncake Store (distributed KVCache)
  + Mooncake Transfer Engine (RDMA Messenger). Defines three scheduler sub-components:
  a Cache-aware Prefill Scheduler (cost = ttft_estimate - alpha*prefix_match_length,
  explicitly inspired by ''NVIDIA Dynamo''s intelligent router'' cost function and
  SGLang Router''s hybrid cache-first/load-first strategy), a Load-balance Decoding
  Scheduler (load_score = current_batch_size + k*waiting_queue_length, with an early-rejection
  mechanism when no instance can meet TBT SLO), and a KVCache Balance Scheduler (heat-aware
  hierarchical storage: hot blocks promoted to GPU HBM, cold blocks demoted to SSD,
  hotspot replication across storage nodes). Proposes external interfaces as REST/gRPC
  (request reception, resource discovery/status collection, scheduling-decision dispatch,
  and a management API) -- a protocol-level, not k8s-API-level, integration surface.
  Flags the same event-driven-over-polling requirement seen in the vllm-ascend RFC:
  state sync should use incremental, event-driven index updates (not full polling)
  to avoid O(n^2) overhead, backed by a global Block-Radix-Tree index recording only
  KVCache block location per physical storage pool node (deliberately not recording
  k8s-specific metadata).'
---

*Suggested by [[rfc-kv-aware-routing-for-pd-disaggregated-serving-via-sourced-kv-events-issue-99]] — RFC cross-references Mooncake-Conductor as potential integration point for unified KV-cache state maintenance*

[Skip to content](https://github.com/kvcache-ai/Mooncake/issues/977#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/issues/977) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/issues/977) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/issues/977) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/issues/977).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake) You must be signed in to change notification settings
  * [ Fork 1.2k ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake)
  * [ Star  6.6k ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake)


#  [RFC]: Mooncake-Conductor: Design and Implementation of a Global Scheduler Module for KV-Cache-Centric Disaggregated Architecture
Copy link
Copy link
Closed as not planned
Closed as not planned
[[RFC]: Mooncake-Conductor: Design and Implementation of a Global Scheduler Module for KV-Cache-Centric Disaggregated Architecture](https://github.com/kvcache-ai/Mooncake/issues/977#top)#977
Copy link
Assignees
Labels
[auto-closed](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3A%22auto-closed%22)[good first issueGood for newcomers](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3A%22good%20first%20issue%22)Good for newcomers[](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3A%22stale%22)
## Description
opened [on Oct 28, 2025](https://github.com/kvcache-ai/Mooncake/issues/977#issue-3561134754)
Last edited by Asher-XunZhang
Issue body actions
### Changes proposed
## 1. Introduction
This proposal outlines the design and implementation of the "**KVCache-centric Scheduling Algorithm** " referenced in the associated research paper, and the **Mooncake-Conductor** module that realizes this algorithm – the **Global Scheduler** for the Mooncake Disaggregated Architecture. The core responsibility of the Conductor remains aligned with the paper's central thesis: to be **KVCache-centric** , thereby intelligently assigning each user request to one **Prefill Instance** and one **Decode Instance** , while managing the data layout within the distributed KVCache pool. The objective is to maximize system-level goodput while adhering to TTFT and TBT SLOs.
## 2. Motivation and Background
The Mooncake Architecture addresses resource utilization bottlenecks and SLO assurance challenges in long-context, high-concurrency scenarios prevalent in traditional coupled architectures by constructing an independent, distributed KVCache pool (Mooncake Store). However, the high performance of this disaggregated architecture is contingent upon a "brain" capable of global optimized scheduling.
  * **Current Limitations** : The current collaboration between Mooncake and inference engines like vLLM and SGLang primarily involves framework-level adaptation for the storage pool. A dedicated, strongly-stateful, predictable global scheduler deeply integrated with Mooncake's design philosophy has not yet been implemented as an independent module.
  * **Core Value** : Implementing the Mooncake-Conductor module will enable: 
    * **Unified Scheduling Policy** : Provide an official, standard scheduling implementation for the Mooncake architecture, reducing integration complexity for users.
    * **Unlocking Architectural Potential** : Fully leverage the performance and cost advantages of the disaggregated architecture through precise cache-awareness, load balancing, and predictive scheduling. Technical reports indicate this can improve throughput by up to 525% in simulated scenarios.
    * **Ecosystem Development** : A well-defined, extensible Conductor interface will facilitate the integration of more inference engines and hardware accelerators into the Mooncake ecosystem.


## 3. Detailed Design
### 3.1 Architectural Overview and Core Data Flow
#### Architectural Description
> The implementation of the Mooncake Conductor should closely collaborate with the Mooncake Store, effectively "sandwiching" inference frameworks like vLLM and SGLang in between, forming a three-layer architecture analogous to a **"sandwich" structure**. This interpretation is accurate and aligns with the design principles outlined in the Mooncake technical report .
The core concept of Mooncake is the KVCache-centric Disaggregated Architecture . The division of responsibilities among its key components is as follows:
• Top Layer (Scheduling Layer): The Mooncake Conductor acts as the global scheduler, serving as the "brain" of the system. It does not perform specific computations but is responsible for macroscopic resource orchestration and task dispatching .
• Middle Layer (Compute Layer): Inference frameworks such as vLLM and SGLang function as computational engines, "sandwiched" in the middle. They focus on efficiently executing Prefill or Decode computation tasks on individual nodes. These frameworks are agnostic to the global resource distribution, such as the state of other nodes or the global location of the KVCache .
• Bottom Layer (Storage and Data Plane Layer): The Mooncake Store (distributed KVCache storage) and the Mooncake Transfer Engine (high-performance data transfer engine, e.g., the RDMA-based Messenger) constitute the underlying storage and data movement infrastructure .
Consequently, these three layers of components form a cohesive "scheduling-compute-storage" sandwich structure .
#### Mooncake-Conductor
Mooncake-Conductor is deployed as an independent process or service, acting as the entry point for requests into the Mooncake cluster. Its core data flow is as follows:
Loading

```
flowchart TD

A[User Request] --> C(Conductor)
subgraph Conductor["Global Scheduler (Conductor)"]
    direction TB
    C --> P[Cache-aware Prefill Scheduler]
    P --> D[Load-balance Decoding Scheduler]
    K[KVCache Balance Scheduler]
end

P -- Assigns Prefill Instance to Request --> PF
D -- Assigns Decode Instance to Request --> DC

subgraph PF["Prefill Instance"]
    PF1[Load Reusable Prefix KVCache<br>from Mooncake Store]
    PF2[Perform Incremental Prefill Computation]
    PF3[Write Newly Generated Delta KVCache<br>back to Mooncake Store]
    PF1 --> PF2 --> PF3
end

subgraph MS[Mooncake Store]
    MS1[(Distributed KVCache Pool)]
end

subgraph DC["Decode Instance"]
    DC1[Asynchronously Load Full KVCache<br>from Mooncake Store to GPU]
    DC2[Perform Decoding<br>Generate Tokens]
    DC1 --> DC2
end

PF1 -- Get() Prefix KVCache --> MS1
PF3 -- Put() Delta KVCache --> MS1
MS1 -- Get() Full KVCache --> DC1
DC2 -- Stream Output --> U[Return Tokens to User]

K -- Background Async Management: <br>Hotspot Migration/Cache Eviction --> MS1

```

### 3.2 Core Scheduler Design and Algorithms
#### 3.2.1 Cache-aware Prefill Scheduler
  * **Core Objective** : To **select a Prefill Instance** for a new request, aiming to **minimize Time-To-First-Token (TTFT)**.
  * **Core Algorithm: Cost Function-driven Multi-objective Optimization**.



```
# Pseudocode Example (Python syntax): Estimated TTFT Cost Function

def estimate_ttft_cost(prefill_instance, request):
    # 1. Cache Match Evaluation
    prefix_match_length = query_global_prefix_tree(request.prompt, prefill_instance.associated_cache_blocks)
    cache_transfer_time = calculate_transfer_time(prefix_match_length, cache_location)  # Consider cache location (HBM/DRAM/SSD)
    
    # 2. Instance Load Evaluation
    queue_wait_time = estimate_queue_delay(prefill_instance.current_queue_depth)
    computation_time = estimate_prefill_compute_time(request.length - prefix_match_length)

    # 3. Aggregate Cost
    total_estimated_ttft = cache_transfer_time + queue_wait_time + computation_time
    # 4. Subtract Cache Reuse Benefit (manifested as reduced compute time)
    cache_benefit = alpha * prefix_match_length  # alpha is a weighting factor

    cost = total_estimated_ttft - cache_benefit
    return cost
```

  * **Decision Mechanism** : Involves a multi-objective optimization decision. The scheduler calculates a "cost" for each available Prefill instance, primarily balancing two factors: 
    * **Cache Locality** : Assesses the presence of reusable KVCache (prefix cache) locally or in the associated Mooncake Store. Reusing cache can skip redundant computation, significantly shortening prefill time.
    * **Instance Load (Challenge)** : Checks the instance's current queue depth and estimated computation time, avoiding sending requests to already busy instances causing queuing delays.
  * **Inspirations** : Draws from the **global cache state awareness and cost function** of NVIDIA Dynamo's intelligent router, and the **hybrid routing strategy** of SGLang Router (cache-first when system balanced, load-first when imbalanced).
  * **Challenge Analysis** : 
    * Asynchronous KVCache transfer times are difficult to predict; estimation errors might be significant in deployment, potentially leading to mis-scheduling if the prediction model lacks flexibility.
    * Avoid forcibly assigning requests to the instance with the longest prefix match; requires a reasonably accurate **cost function** to weigh the trade-offs between transfer efficiency and recomputation efficiency.
    * Requires a mechanism to record and update the "node --- KVCache stored by the node" mapping.


#### 3.2.2 Load-balance Decoding Scheduler
  * **Core Objective** : To select a **Decode Instance** for requests that have completed the prefill phase, aiming to **optimize throughput while meeting TBT SLO requirements**.
  * **Core Algorithm: Centered on Load Balancing and SLO Compliance**. The scheduler examines the current load status of all Decode instances, such as each instance's batch size and queue length. The goal is to assign new requests to the least loaded instance to prevent TBT spikes due to instance overload. 
    * **Load Scoring** : `load_score = current_batch_size + k * waiting_queue_length` (where `k` is a penalty coefficient).
    * **SLO Compliance Check (Challenge)** : Predict the TBT of each Decode instance over a future time window, only assigning requests to instances predicted to meet their TBT SLO.
    * **Early Rejection Mechanism** : If predictions indicate no instance can satisfy the request's TBT SLO upon completion, reject the request outright before the prefill phase to avoid resource waste.
  * **Inspirations** : Incorporates ideas from Ant's AI Gateway regarding **multi-objective trade-offs and SLO guarantee** , and SGLang Router's **dynamic load assessment and anti-hotspot mechanism** (random selection from a low-load group).
  * **Challenge Analysis** : 
    * Requires predicting the TBT for each Decode instance over a future time window.


#### 3.2.3 KVCache Balance Scheduler
  * **Core Objective** : Functions as a background daemon managing the data distribution of KVCache within the **Mooncake Store** , **preventing access hotspots and bottlenecks** to enhance the overall efficiency and throughput of the cache system.
  * **Core Algorithm: Heat-aware Hierarchical Storage and Automated Hotspot Migration**. 
    * **Heat Quantification (Challenge)** : Compute a heat value for KVCache blocks based on factors like access frequency, recent access time, and the SLO tier of associated requests.
    * **Data Migration** : Monitor access frequency to identify "hot" cache blocks (e.g., frequently accessed common prefixes) and potentially create replicas across multiple storage nodes for load distribution. 
      * **Hierarchical Storage** : 
        * **Hot Data Promotion** : Migrate or replicate high-frequency access KVCache blocks to faster storage tiers (e.g., GPU HBM).
        * **Cold Data Demotion** : Move low-frequency access data to cheaper storage (e.g., SSD), employing strategies like LRU/LFU.
      * **Hotspot Migration** : For globally hot data, automatically create replicas across multiple storage nodes to avoid single-point access bottlenecks.
  * **Inspirations** : References **data heat identification and hierarchical storage** strategies from traditional distributed storage systems, and the **multi-tenancy and cost-aware caching** concept from Ant's AI Gateway, applying differentiated policies based on the business priority of cached data.
  * **Challenge Analysis** : 
    * Quantifying data heat.
    * Interact with Mooncake-Store for data migration.


### 3.3 External Interface Design (Preliminary)
  * **Request Reception Interface (REST/gRPC)** : Receives inference requests, including the prompt and parameters.
  * **Resource Discovery and Status Collection Interface** : Interacts with the Prefill/Decode resource pools and Mooncake Store to obtain real-time node health status, load metrics (GPU utilization, queue length), and KVCache distribution information.
  * **Scheduling Decision Dispatch Interface** : Dispatches assignment decisions to the designated Prefill and Decode instances.
  * **Management API** : Provides administrative functions such as cluster status queries and dynamic policy adjustments.


## 4. Archi-Level: Implementation Challenges and Mitigation Strategies
  1. **Prediction Accuracy Challenge** : 
     * **Challenge** : Accurate prediction of TTFT and TBT is affected by network jitter, GPU computation variance, request characteristic uncertainty, etc. Prediction deviations may lead to scheduling errors.
     * **Mitigation Strategies** : 
       * (**Complex but Relatively Accurate**) Employ **lightweight machine learning models (e.g., Gradient Boosting Trees)** or **time series forecasting algorithms** : Combine extensive historical monitoring data for offline training and online learning.
       * (**Relatively Simple but Higher Error**) Implement **sliding window averaging** and **prediction confidence interval** assessment: Adopt more conservative scheduling strategies when prediction uncertainty is high.
  2. **State Consistency and Performance Overhead** : 
     * **Challenge** : The Conductor needs to maintain a global resource view. In large-scale clusters, frequent state synchronization can introduce significant network overhead (O(n²) pressure) and information latency, causing the scheduler to make decisions based on stale data.
     * **Mitigation Strategies** : 
       * Adopt **incremental updates** and an **event-driven** state push mechanism, rather than full polling. Index updates are triggered locally only when KVCache blocks are created, migrated, or evicted, avoiding high-frequency full synchronization.
       * Design a **global index based on a Block Radix Tree or similar structure** , partitioning the global state by resource pool. The design philosophy aligns with `ObjectMetadata` in `MasterService`, recording only the location of KVCache blocks within the physical storage pool and key attributes (the Conductor only records node location, not `Segment` details). When a new request arrives, the Conductor can quickly determine the common prefix between its prompt and all KVCache blocks in the cluster by querying this tree, thus estimating the cache match degree for each candidate Prefill node.
       * Set a **short-term validity** for state information and develop compensation mechanisms for decisions based on slightly stale states. For scheduling decisions, the index allows for brief eventual consistency rather than requiring strong consistency (allowing `ReplicaStatus` to be `Processing`), which reduces synchronization complexity and overhead.
  3. **Compatibility with Heterogeneous Inference Engines** : 
     * **Challenge** : How to provide a unified and efficient integration interface for inference engines with different architectures like vLLM and SGLang, considering the existing upstream service patterns (e.g., Proxy, P2P) implemented by these engines.
     * **Mitigation Strategies** : 
       * The Conductor should be implemented as an RPC-based service, defining a set of **standardized gRPC interfaces and data structures** describing instance status, cache block information, task instructions, etc.
       * Provide **client adapters** for different engines, maintained collaboratively by the community.


## 5. Expected Benefits and Evaluation Metrics
  * **Performance Improvement** : Aim for a significant increase in system goodput (target >30%) under typical long-context workloads compared to a baseline version without the Conductor's intelligent scheduling, while maintaining TTFT and TBT SLO attainment rates (P99) above 99.9%.
  * **Resource Utilization** : Improve the overall utilization of GPU compute resources and storage resources through intelligent KVCache layout and load balancing.
  * **System Stability** : Enhance cluster robustness during traffic peaks through predictive early rejection and overload protection.


## 6. Development Plan (Preliminary)
  1. **Community Discussion and Design Finalization** : Thoroughly discuss this RFC within the community to refine interfaces and algorithms.
  2. **Core Service Framework and Interface Design** : Implement the Conductor service framework and basic communication modules.
  3. **Scheduler Implementation** : Implement the various Schedulers (Prefill, Decoding, KVCache Balance).
  4. **Integration Testing and Performance Tuning** : Conduct integration testing with Mooncake Store and mainstream inference engines like vLLM and SGLang, followed by performance tuning under simulated and real-world loads.
  5. **Ongoing Maintenance** : Continuously add support for interfaces to different inference frameworks.


## 7. Comparison with and Lessons Learned from Other Scheduling Frameworks
### Comparison  
| **Comparison Dimension**  | **Mooncake Conductor**  | **SGLang Router**  | **(NVIDIA) Dynamo**  | **(Ant Group) AI Gateway**  | **AIBrix**  |  
| --- | --- | --- | --- | --- | --- |  
| **Core Positioning**  |  **System-level Global Scheduler** : KV Cache-centric, physically disaggregates Prefill and Decode stages. Central coordinator for the Mooncake KV-Cache-centric architecture, managing compute-storage synergy.  |  **Lightweight Routing Layer** : Focuses on request distribution and load balancing across multiple nodes, often decoupled from inference engines. Cache-aware load balancer for multi-Worker environments, optimizing cross-request reuse for RadixAttention.  |  **Cluster-level Orchestration Framework Core** : KV-Cache-aware intelligent router for distributed GPU clusters, minimizing KV Cache recomputation. Acts as the "OS for the AI factory," handling resource scheduling and routing in distributed environments.  |  **Enterprise-grade Model Gateway** : For multi-model, multi-tenant production environments, integrating traffic management, cost control, and intelligent routing.  |  **vLLM's Cloud-native Control Plane** : Handles resource orchestration at the K8s level, focusing on AI service pipeline (DAG) orchestration, scheduling request flow across models/processing units.  |  
| **Relationship with KV Cache**  |  **Direct Management & Scheduling**: Directly manages the physically disaggregated distributed KV Cache pool (Mooncake Store), making decisions on Cache prefilling, migration, replication, and eviction.  |  **Indirect Awareness & Utilization**: Estimates the match ratio of node-local KV Cache via a global approximate prefix tree, guiding requests for reuse.  |  **Global Awareness & Routing**: Tracks KV Cache distribution cluster-wide via an intelligent router, routing new requests to nodes with high cache reuse potential.  |  **Business-layer Cache Awareness** : Treats KV Cache state as a key input for intelligent routing decisions.  |  **No Direct Interaction** : Scheduling occurs at the request level, not delving into fine-grained KV Cache management.  |  
| **Relationship with Node Load**  |  **Deep Integration & Prediction**: Considers KV Cache location and dynamically monitors Prefill/Decode node load (queue depth, GPU utilization), employing prediction models to avoid overload and implement early rejection.  |  **Dynamic Load Balancing** : Uses a hybrid strategy; prioritizes cache match when load is balanced, otherwise prioritizes the least loaded node. Primary goal is cache hit, load balance is secondary.  |  **Multi-objective Cost Function** : Routing decisions balance KV cache overlap score and node real-time load (GPU utilization, queue length). Prioritizes KV Cache hit over choosing the idlest node.  |  **Multi-objective Trade-off** : Real-time balancing of inference latency, throughput, API cost, and budget control in routing decisions, achieving load balance and cost optimization.  |  **Workflow Node Load Balancing** : Schedules based on the current availability and performance of processing nodes within a pipeline.  |  
| **Scheduling Scope**  |  **Cross-Heterogeneous Resource Pools, End-to-End Co-scheduling** : Spans Prefill cluster, Decode cluster, and distributed cache pool. Selects a Prefill-Decode node _pair_ for a single request and manages KV Cache transfer between them.  |  **Multi-node Request Distribution** : Scope is a pool of homogeneous or heterogeneous Worker nodes.  |  **Cluster-level Resource Optimization** : Scope is a large GPU cluster.  |  **Gateway-level Traffic Governance** : Scope is backend model instances or clusters, distributing and governing traffic among backend service instances.  |  **Workflow/Pipeline-level Orchestration** : Scope is a DAG composed of multiple models or services.  |  
| **Target Outcome**  |  **Disaggregate Prefill/Decode** via an independent KV Cache storage layer, enabling elastic scaling of compute resources and efficient long-context support.  |  **Increase Cache Reuse Rate** , significantly reducing recomputation, especially in shared prefix scenarios.  | Accelerate inference and save compute by **avoiding KV Cache recomputation**.  |  **Guarantee Service SLOs** , control costs, and achieve multi-tenant isolation.  | Achieve **cost-efficiency and fairness** for large-scale vLLM deployment.  |  
### Lessons Learned
Based on the comparative analysis above, the algorithm implementation for Mooncake Conductor can draw the following actionable insights from the scheduling strategies of other components:  
| Target Scheduler  | Inspirational Framework  | Core Adoptable Insight  | Mooncake Implementation Consideration  |  
| --- | --- | --- | --- |  
| **Cache-aware Prefill Scheduler**  | NVIDIA Dynamo  |  **Global Cache State Awareness & Cost Function**: Maintain a global prefix tree, compute the overlap score between requests and node caches, and select the optimal node via a cost function incorporating real-time node load (GPU util, queue length).  | Must adapt to Mooncake's **disaggregated architecture**. Cost function needs a "cache transfer time" dimension. Decision becomes selecting a "Prefill-Decode node _pair_ ", not just a single node.  |  
| SGLang Router  |  **Hybrid Routing & Anti-hotspot Strategy**: Dynamically switch priority based on system load (cache-first when balanced, load-first when imbalanced), and randomly select from a low-load group to avoid new hotspots.  | Applicable for initial routing among Prefill instance groups, enabling fast, lightweight load distribution.  |  
| **KVCache Balance Scheduler**  | Traditional Distributed Storage  |  **Data Heat Identification & Tiered Storage**: Use LRU/LFU etc. to identify hot data, automatically migrate it to fast storage (GPU HBM), and evict cold data to cheap storage (SSD).  | Core challenge is **quantifying "heat"**. Requires a multi-dimensional definition based on access frequency, request SLO tier, business priority, etc.  |  
| Ant AI Gateway  |  **Multi-tenancy & Cost-aware Caching**: Implement differentiated cache policies (TTL, replica count) for users/requests of varying importance, optimizing overall business value.  | Requires implementing fine-grained **cache quota and priority management** within Mooncake Store.  |  
| **Load-balance Decoding Scheduler**  | Ant AI Gateway  |  **Multi-objective Trade-off & SLO Guarantee**: Integrate latency, throughput, cost, and SLA considerations into routing decisions, implementing early rejection for requests likely to violate SLOs.  | The core constraint for the Decoding scheduler is **TBT SLO**. Can integrate SLO compliance as a hard constraint.  |  
| SGLang Router  |  **Dynamic Load Assessment & Smooth Allocation**: Calculate load score based on real-time task count and queue length, randomly selecting from the least loaded group for smooth distribution.  | Requires finer-grained **load monitoring and prediction** within the Mooncake Decode cluster to avoid imbalances due to information lag.  |  
## This proposal details the design rationale, core algorithms, implementation challenges, and plan. I kindly request the community Maintainers and developers to review this proposal and provide valuable feedback.
### Before submitting a new issue...
  * Make sure you already searched for relevant issues and read the [documentation](https://kvcache-ai.github.io/Mooncake/)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


👍React with 👍12Reacted by Teng Ma, Ruoyu Qin, Shangming Cai, tang, ykwd, doujiang24, yejj, Xuchun Shang, Stary, Jianxin and 2 more
🚀React with 🚀2Reacted by Teng Ma and Ruoyu Qin
## Activity
### Asher-XunZhang commented on Oct 28, 2025 
Author
More actions
The five aforementioned scheduling systems exhibit distinct design philosophies and primary focuses:
• The **Mooncake Conductor** assumes the most comprehensive role. It functions as a system-level master planner, orchestrating and deeply managing the three disaggregated resource pools: Prefill, Decode, and the KVCache storage.
• Both **Ant Group's AI Gateway** and **SGLang Router** act as traffic ingress points, but with different emphases. The Ant Group's AI Gateway is oriented towards being a mature enterprise-grade product, emphasizing multi-objective optimization encompassing performance, cost, and Service Level Objectives (SLOs). In contrast, the SGLang Router is more focused on addressing the specific technical challenge of cache-aware load balancing, making it a lighter-weight and more specialized component.
• The **NVIDIA's Dynamo** Smart Router and the intelligent routing gateway of **AIBrix** both serve larger-scale, distributed inference systems. Dynamo's strength lies in its modular design and potent global cache-state awareness. AIBrix, however, emphasizes deep integration with the Kubernetes ecosystem and optimizes GPU resource allocation efficiency through mechanisms like task prioritization and GPU affinity scheduling.
[stmatengss](https://github.com/stmatengss)
assigned [on Oct 28, 2025](https://github.com/kvcache-ai/Mooncake/issues/977#event-20568284587)
### chestnut-Q commented on Oct 28, 2025 
Collaborator
More actions
Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang). Thank you for your comprehensive proposal! I understand that building the Conductor is a large-scale effort, but also a highly meaningful one. I'd like to ask a few questions regarding the roadmap:
  1. Which inference engines will Mooncake-Conductor integrate with first?
  2. What modifications will the current Mooncake-Store architecture require to support the Conductor's functionality?
  3. Since the Conductor includes many potential features, what components should be included in a minimal, runnable PoC (for example, service discovery and a 1P1D disaggregated router)?


Thanks again for this RFC.
### Asher-XunZhang commented on Oct 29, 2025 
Last edited by Asher-XunZhang
Author
More actions
> Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang). Thank you for your comprehensive proposal! I understand that building the Conductor is a large-scale effort, but also a highly meaningful one. I'd like to ask a few questions regarding the roadmap:
>   1. Which inference engines will Mooncake-Conductor integrate with first?
>   2. What modifications will the current Mooncake-Store architecture require to support the Conductor's functionality?
>   3. Since the Conductor includes many potential features, what components should be included in a minimal, runnable PoC (for example, service discovery and a 1P1D disaggregated router)?
> 

> Thanks again for this RFC.
Thank you for your recognition of the RFC proposal and the valuable questions raised! Below, I will address your three questions one by one, based on my previous RFC:
  1. I intend to prioritize end-to-end integration with vLLM to form a demonstrable closed loop before gradually expanding the ecosystem. The reasons are as follows:vLLM currently lacks its own global scheduling and routing mechanism, whereas SGLang already has the SGLang Router. Therefore, we will first integrate a version of Conductor based on the implementation of the SGLang Router and the algorithms mentioned in the paper (while also considering scheduling ideas from other schedulers). This initial version will be adapted to vLLM to evaluate its benefits.vLLM natively supports accessing APIs such as `/metrics`to obtain metrics from deployed nodes, which is highly beneficial for the Conductor layer.After this, we will adapt to other general-purpose inference engines like SGLang and TensorRT-LLM, which either have their own global schedulers or are relatively less universal. An Adapter will be separated in the middle, following the same design philosophy as the transport component, allowing inference engine developers to adapt to mature ecosystem interfaces.
  2. My goal is to avoid disrupting the existing structure and design philosophy of Mooncake-Store as much as possible, striving to only add without deleting or modifying. Currently, the necessary modifications to the Mooncake-Store architecture involve two main points: 
     * For the **Cache-aware Prefill Scheduler** : A lightweight metadata indexing service (Radix Tree) will be added to the Master Service to enable quick filtering of P-nodes with high hit rates in the Scheduler. However, the current challenge lies in the fact that, under the existing design, key calculation in the resource pool depends on intermediate steps at the inference engine layer. For example, encoding based on the deployed model’s vocabulary (consistency in model encoding—this data can be obtained in vLLM by calling the `/tokenize`API of any P-node in a cluster deploying the same model), partitioning into minimal units according to the configured `block size`(consistency in block partitioning—currently, there is no elegant solution), and hashing each `block`(consistency in hash calculation—currently, there is no elegant solution). However, the inference engine layer, within the current framework design, only becomes perceivable after being assigned by the Conductor. In contrast, calculating the cache hit rate distribution for a global view must occur before the inference engine stage. This makes it challenging to elegantly maintain consistency with the computation method of prefix caching in the Prefill Instance that have not yet been assigned.
     * For the **KVCache Balance Scheduler** : A set of monitoring APIs needs to be exposed, enabling Conductor to query the overall status of Mooncake-Store in real time, such as storage tier (HBM/DRAM/SSD) usage, network throughput, and access frequency of cache blocks. This would provide decision-making basis for Conductor’s KVCache Balance Scheduler to perform operations like hot-spot migration and cold data eviction. Alternatively, the scheduling policies of this Scheduler could be implemented within the Master Service. 
       * The advantage is a simpler design, reduced call layers, and no need to implement an efficient control plane communication protocol (e.g., gRPC-based) between Conductor and Mooncake-Store—hot-spot migration and other KV Cache scheduling behaviors could be handled directly by the Master Service.
       * The disadvantage is unclear functional module boundaries and tight coupling between scheduling and storage.
  3. I believe that, for Mooncake Conductor at this stage, a minimum viable prototype should focus on validating the **core scheduling value of Conductor** : specifically, whether intelligent "1 Prefill + 1 Decode" (1P1D) instance pairing and cache awareness can significantly improve system efficiency with minimal overhead. This PoC deliberately **does not implement** advanced features such as the complex KVCache Balance Scheduler or predictive early rejection. Upon the success of the PoC, we will gradually iterate to optimize more precise cache and load balancing algorithms, advanced cache management, and other complete functionalities.


I look forward to further suggestions and discussions!
### chestnut-Q commented on Oct 29, 2025 
Collaborator
More actions
> > Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang). Thank you for your comprehensive proposal! I understand that building the Conductor is a large-scale effort, but also a highly meaningful one. I'd like to ask a few questions regarding the roadmap:
>>   1. Which inference engines will Mooncake-Conductor integrate with first?
>>   2. What modifications will the current Mooncake-Store architecture require to support the Conductor's functionality?
>>   3. Since the Conductor includes many potential features, what components should be included in a minimal, runnable PoC (for example, service discovery and a 1P1D disaggregated router)?
>> 

>> Thanks again for this RFC.
> Thank you for your recognition of the RFC proposal and the valuable questions raised! Below, I will address your three questions one by one, based on my previous RFC:
>   1. I intend to prioritize end-to-end integration with vLLM to form a demonstrable closed loop before gradually expanding the ecosystem. The reasons are as follows:vLLM currently lacks its own global scheduling and routing mechanism, whereas SGLang already has the SGLang Router. Therefore, we will first integrate a version of Conductor based on the implementation of the SGLang Router and the algorithms mentioned in the paper (while also considering scheduling ideas from other schedulers). This initial version will be adapted to vLLM to evaluate its benefits.vLLM natively supports accessing APIs such as `/metrics`to obtain metrics from deployed nodes, which is highly beneficial for the Conductor layer.After this, we will adapt to other general-purpose inference engines like SGLang and TensorRT-LLM, which either have their own global schedulers or are relatively less universal. An Adapter will be separated in the middle, following the same design philosophy as the transport component, allowing inference engine developers to adapt to mature ecosystem interfaces.
>   2. My goal is to avoid disrupting the existing structure and design philosophy of Mooncake-Store as much as possible, striving to only add without deleting or modifying. Currently, the necessary modifications to the Mooncake-Store architecture involve two main points:
>      * For the **Cache-aware Prefill Scheduler** : A lightweight metadata indexing service (Radix Tree) will be added to the Master Service to enable quick filtering of P-nodes with high hit rates in the Scheduler. However, the current challenge lies in the fact that, under the existing design, key calculation in the resource pool depends on intermediate steps at the inference engine layer. For example, encoding based on the deployed model’s vocabulary (consistency in model encoding—this data can be obtained in vLLM by calling the `/tokenize`API of any P-node in a cluster deploying the same model), partitioning into minimal units according to the configured `block size`(consistency in block partitioning—currently, there is no elegant solution), and hashing each `block`(consistency in hash calculation—currently, there is no elegant solution). However, the inference engine layer, within the current framework design, only becomes perceivable after being assigned by the Conductor. In contrast, calculating the cache hit rate distribution for a global view must occur before the inference engine stage. This makes it challenging to elegantly maintain consistency with the computation method of prefix caching in the Prefill Instance that have not yet been assigned.
>      * For the **KVCache Balance Scheduler** : A set of monitoring APIs needs to be exposed, enabling Conductor to query the overall status of Mooncake-Store in real time, such as storage tier (HBM/DRAM/SSD) usage, network throughput, and access frequency of cache blocks. This would provide decision-making basis for Conductor’s KVCache Balance Scheduler to perform operations like hot-spot migration and cold data eviction. Alternatively, the scheduling policies of this Scheduler could be implemented within the Master Service.
>        * The advantage is a simpler design, reduced call layers, and no need to implement an efficient control plane communication protocol (e.g., gRPC-based) between Conductor and Mooncake-Store—hot-spot migration and other KV Cache scheduling behaviors could be handled directly by the Master Service.
>        * The disadvantage is unclear functional module boundaries and tight coupling between scheduling and storage.
>   3. I believe that, for Mooncake Conductor at this stage, a minimum viable prototype should focus on validating the **core scheduling value of Conductor** : specifically, whether intelligent "1 Prefill + 1 Decode" (1P1D) instance pairing and cache awareness can significantly improve system efficiency with minimal overhead. This PoC deliberately **does not implement** advanced features such as the complex KVCache Balance Scheduler or predictive early rejection. Upon the success of the PoC, we will gradually iterate to optimize more precise cache and load balancing algorithms, advanced cache management, and other complete functionalities.
> 

> I look forward to further suggestions and discussions!
Great! I think the main challenge is aligning the KVCache states between the Conductor and the inference engines, since there's currently no external interface for controlling KVCache at the engine level.
Perhaps we could start with a PoC PR in this repo first, and then open a roadmap issue to invite more contributors to join. BTW, I'd prefer to create a new repo for better project management, but not right now.
### Asher-XunZhang commented on Oct 30, 2025 
Last edited by Asher-XunZhang
Author
More actions
> > > Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang). Thank you for your comprehensive proposal! I understand that building the Conductor is a large-scale effort, but also a highly meaningful one. I'd like to ask a few questions regarding the roadmap:
>>>   1. Which inference engines will Mooncake-Conductor integrate with first?
>>>   2. What modifications will the current Mooncake-Store architecture require to support the Conductor's functionality?
>>>   3. Since the Conductor includes many potential features, what components should be included in a minimal, runnable PoC (for example, service discovery and a 1P1D disaggregated router)?
>>> 

>>> Thanks again for this RFC.
>> Thank you for your recognition of the RFC proposal and the valuable questions raised! Below, I will address your three questions one by one, based on my previous RFC:
>>   1. I intend to prioritize end-to-end integration with vLLM to form a demonstrable closed loop before gradually expanding the ecosystem. The reasons are as follows:vLLM currently lacks its own global scheduling and routing mechanism, whereas SGLang already has the SGLang Router. Therefore, we will first integrate a version of Conductor based on the implementation of the SGLang Router and the algorithms mentioned in the paper (while also considering scheduling ideas from other schedulers). This initial version will be adapted to vLLM to evaluate its benefits.vLLM natively supports accessing APIs such as `/metrics`to obtain metrics from deployed nodes, which is highly beneficial for the Conductor layer.After this, we will adapt to other general-purpose inference engines like SGLang and TensorRT-LLM, which either have their own global schedulers or are relatively less universal. An Adapter will be separated in the middle, following the same design philosophy as the transport component, allowing inference engine developers to adapt to mature ecosystem interfaces.
>>   2. My goal is to avoid disrupting the existing structure and design philosophy of Mooncake-Store as much as possible, striving to only add without deleting or modifying. Currently, the necessary modifications to the Mooncake-Store architecture involve two main points:
>>      * For the **Cache-aware Prefill Scheduler** : A lightweight metadata indexing service (Radix Tree) will be added to the Master Service to enable quick filtering of P-nodes with high hit rates in the Scheduler. However, the current challenge lies in the fact that, under the existing design, key calculation in the resource pool depends on intermediate steps at the inference engine layer. For example, encoding based on the deployed model’s vocabulary (consistency in model encoding—this data can be obtained in vLLM by calling the `/tokenize`API of any P-node in a cluster deploying the same model), partitioning into minimal units according to the configured `block size`(consistency in block partitioning—currently, there is no elegant solution), and hashing each `block`(consistency in hash calculation—currently, there is no elegant solution). However, the inference engine layer, within the current framework design, only becomes perceivable after being assigned by the Conductor. In contrast, calculating the cache hit rate distribution for a global view must occur before the inference engine stage. This makes it challenging to elegantly maintain consistency with the computation method of prefix caching in the Prefill Instance that have not yet been assigned.
>>      * For the **KVCache Balance Scheduler** : A set of monitoring APIs needs to be exposed, enabling Conductor to query the overall status of Mooncake-Store in real time, such as storage tier (HBM/DRAM/SSD) usage, network throughput, and access frequency of cache blocks. This would provide decision-making basis for Conductor’s KVCache Balance Scheduler to perform operations like hot-spot migration and cold data eviction. Alternatively, the scheduling policies of this Scheduler could be implemented within the Master Service.
>>        * The advantage is a simpler design, reduced call layers, and no need to implement an efficient control plane communication protocol (e.g., gRPC-based) between Conductor and Mooncake-Store—hot-spot migration and other KV Cache scheduling behaviors could be handled directly by the Master Service.
>>        * The disadvantage is unclear functional module boundaries and tight coupling between scheduling and storage.
>>   3. I believe that, for Mooncake Conductor at this stage, a minimum viable prototype should focus on validating the **core scheduling value of Conductor** : specifically, whether intelligent "1 Prefill + 1 Decode" (1P1D) instance pairing and cache awareness can significantly improve system efficiency with minimal overhead. This PoC deliberately **does not implement** advanced features such as the complex KVCache Balance Scheduler or predictive early rejection. Upon the success of the PoC, we will gradually iterate to optimize more precise cache and load balancing algorithms, advanced cache management, and other complete functionalities.
>> 

>> I look forward to further suggestions and discussions!
> Great! I think the main challenge is aligning the KVCache states between the Conductor and the inference engines, since there's currently no external interface for controlling KVCache at the engine level.
> Perhaps we could start with a PoC PR in this repo first, and then open a roadmap issue to invite more contributors to join. BTW, I'd prefer to create a new repo for better project management, but not right now.
I'm glad to continue discussing this topic! On the problem of adapting different inference engines, I envision that we can refer to the `Transport` in the `Mooncake-Transfer-Engine` component, so that our final implementation can only provide the adaptation for vLLM as an example, and adapt other frameworks later. Regarding my ideas, I have designed a preliminary architecture draft:
Loading

```
flowchart LR
    Input["Request Arrives<br>Contains Prompt and SLO"]
    
    subgraph Proxy["Reverse Proxy Core"]
        direction TB
        RequestParser["Request Parser & Queue"]
        SchedulerRouter["Scheduler Router"]
        DynamicWeightManager["DynamicWeightManager"]
        DecisionEngine["Decision Engine"]
        
        RequestParser --> SchedulerRouter
        SchedulerRouter --> CacheAwareScheduler
        SchedulerRouter --> LoadBalanceScheduler
        SchedulerRouter --> OtherSchedulers["Other Schedulers"]
        
        CacheAwareScheduler -->|Metrics| DynamicWeightManager
        LoadBalanceScheduler -->|Metrics| DynamicWeightManager
        OtherSchedulers -->|Metrics| DynamicWeightManager
        
        DynamicWeightManager --> DecisionEngine
    end
    
    subgraph CacheAwareScheduler["Cache-Aware Scheduler (KVEventManager)"]
        direction LR
        KVEventController["KV Event Manager"]
        ZeroMQReceiver["ZeroMQ Subscriber<br>(Listens KVEvents)"]
        Indexer["Indexer<br>(RadixTree Data Structure)"]
        
        KVEventController -->|Get| ZeroMQReceiver
        KVEventController -->|Updates| Indexer
    end
    
    subgraph ExternalComponents["External Components"]
        direction TB
        subgraph vLLMInstances["vLLM Instances"]
            vLLMZMQPublisher["zmq Publisher"]
        end
        
        subgraph MooncakeStore["Mooncake Store"]
            MooncakeStoreZMQPublisher["zmq Publisher"]
        end
        
        ZeroMQReceiver -->|Subscribe KVEvents| vLLMZMQPublisher
        ZeroMQReceiver -->|Subscribe KVEvents| MooncakeStoreZMQPublisher
    end
    
    subgraph FinalDecision["Instance"]
        direction TB
        DecisionEngine --> vLLMInstances1["vLLM Instances 1"]
        DecisionEngine --> vLLMInstances2["vLLM Instances 2"]
    end
    
    Input --> RequestParser
    
    classDef proxy fill:#e3f2fd,stroke:#1e88e5
    classDef scheduler fill:#e8f5e8,stroke:#43a047
    classDef external fill:#fff8e1,stroke:#ffa000
    
    class Proxy proxy
    class CacheAwareScheduler,LoadBalanceScheduler,OtherSchedulers scheduler
    class ExternalComponents external

```

[XucSh](https://github.com/XucSh)
added 
[good first issueGood for newcomers](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3A%22good%20first%20issue%22)Good for newcomers
[on Oct 30, 2025](https://github.com/kvcache-ai/Mooncake/issues/977#event-20623145820)
### chickeyton commented on Oct 30, 2025 
Last edited by chickeyton
More actions
There are lots of protential algorithms for the Global Scheduler, the most important thing is providing a way to lookup the cache (i.e. `query_global_prefix_tree`), I suggest Mooncake provides a FullLookup feature like the [PR](https://github.com/LMCache/LMCache/pull/1420) in LMCache
👍React with 👍2Reacted by Asher Zhang and Chenguang Zheng
### Asher-XunZhang commented on Oct 31, 2025 
Author
More actions
> There are lots of protential algorithms for the Global Scheduler, the most important thing is providing a way to lookup the cache (i.e. `query_global_prefix_tree`), I suggest Mooncake provides a FullLookup feature like the [PR](https://github.com/LMCache/LMCache/pull/1420) in LMCache
Looks cool! I will implement this idea to the `Global Cache Hit Distribution Collector` module.
### chestnut-Q commented on Oct 31, 2025 
Collaborator
More actions
> > There are lots of protential algorithms for the Global Scheduler, the most important thing is providing a way to lookup the cache (i.e. `query_global_prefix_tree`), I suggest Mooncake provides a FullLookup feature like the [PR](https://github.com/LMCache/LMCache/pull/1420) in LMCache
> Looks cool! I will implement this idea to the `Global Cache Hit Distribution Collector` module.
I completely agree. This is also part of the aforementioned Mooncake Store modifications. Contributions are welcome!
### Liziqi-77 commented on Dec 2, 2025 
Contributor
More actions
Currently, there are no histograms or other visualization functions for metrics such as kv cache hit rate. We believe it is necessary to add a module to Conductor to manage node and Mooncake metrics, while providing visualization solutions and providing formats compatible with Prometheus and general visualizations.
### lengrongfu commented on Dec 22, 2025 
More actions
vllm have a new router project <https://github.com/vllm-project/router>
### Asher-XunZhang commented on Dec 23, 2025 
Last edited by Asher-XunZhang
Author
More actions
> vllm have a new router project <https://github.com/vllm-project/router>
Yes, we've also noticed that. The current plan is to align the solution with the event reporting mechanism of vLLM, where mooncake-store proactively reports events via ZMQ [#1251](https://github.com/kvcache-ai/Mooncake/pull/1251). Additionally, we have completed the development of a Conductor written in Go, which includes integration for handling KVCache Events from both vLLM and mooncake [#1248](https://github.com/kvcache-ai/Mooncake/pull/1248). Our next step is to explore how to integrate it into the existing cache-aware algorithm in Router.
### TheManyBELL commented on Jan 7, 2026 
More actions
Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang) ,I attended the presentation about Conductor at the last Mooncake meeting, and my understanding is that it is positioned as an L1/L2/L3 multi-tier KV Cache manager that also provides external support for request cache hit rate evaluation. However, this RFC describes Conductor as being responsible for request distribution as well, which seems to overlap with the role of a gateway/router. I’d like to ask two questions:
  1. What is the **precise positioning** of Conductor?
  2. If its core positioning is indeed a KV Cache manager, what are the current plans for **its collaboration with various AI gateways?**


### Asher-XunZhang commented on Jan 7, 2026 
Last edited by Asher-XunZhang
Author
More actions
> Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang) ,I attended the presentation about Conductor at the last Mooncake meeting, and my understanding is that it is positioned as an L1/L2/L3 multi-tier KV Cache manager that also provides external support for request cache hit rate evaluation. However, this RFC describes Conductor as being responsible for request distribution as well, which seems to overlap with the role of a gateway/router. I’d like to ask two questions:
>   1. What is the **precise positioning** of Conductor?
>   2. If its core positioning is indeed a KV Cache manager, what are the current plans for **its collaboration with various AI gateways?**
> 

[@TheManyBELL](https://github.com/TheManyBELL) I apologize for the delayed update of this RFC. The context is as follows: Initially, we planned to implement a router, so the description in the RFC may appear contradictory to last meeting, which could be misleading. Later, we discovered that various implementations such as vLLM, SGLang, and AIGateway already provide mature router solutions, which include load balancing and KV-Cache-aware functionality at the instance level. As a result, we shifted our focus to developing a KV-Cache-aware data structure that integrates the L1/L2 layers of the inference engine instance with the external pooling system's L3 (Mooncake) layer ([#1251](https://github.com/kvcache-ai/Mooncake/pull/1251)) (as shown in the green box labeled `Cache-Aware Scheduler` in the latest architecture diagram). The current implementation is designed as a separate, lightweight HTTP service ([#1248](https://github.com/kvcache-ai/Mooncake/pull/1248)). The scheduler at the Router layer accesses this service to retrieve global information and makes decisions based on other algorithms.
👍React with 👍1Reacted by Flona
### TheManyBELL commented on Jan 7, 2026 
More actions
> > Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang) ,I attended the presentation about Conductor at the last Mooncake meeting, and my understanding is that it is positioned as an L1/L2/L3 multi-tier KV Cache manager that also provides external support for request cache hit rate evaluation. However, this RFC describes Conductor as being responsible for request distribution as well, which seems to overlap with the role of a gateway/router. I’d like to ask two questions:
>>   1. What is the **precise positioning** of Conductor?
>>   2. If its core positioning is indeed a KV Cache manager, what are the current plans for **its collaboration with various AI gateways?**
>> 

> [@TheManyBELL](https://github.com/TheManyBELL) I apologize for the delayed update of this RFC. The context is as follows: Initially, we planned to implement a router, so the description in the RFC may appear contradictory to last meeting, which could be misleading. Later, we discovered that various implementations such as vLLM, SGLang, and AIGateway already provide mature router solutions, which include load balancing and KV-Cache-aware functionality at the instance level. As a result, we shifted our focus to developing a KV-Cache-aware data structure that integrates the L1/L2 layers of the inference engine instance with the external pooling system's L3 (Mooncake) layer ([#1306](https://github.com/kvcache-ai/Mooncake/pull/1306)) (as shown in the green box labeled `Cache-Aware Scheduler` in the latest architecture diagram). The current implementation is designed as a separate, lightweight HTTP service ([#1248](https://github.com/kvcache-ai/Mooncake/pull/1248)). The scheduler at the Router layer accesses this service to retrieve global information and makes decisions based on other algorithms.
Thanks for the quick response! Looking forward to this feature!
😄React with 😄1Reacted by Asher Zhang
### Asher-XunZhang commented on Jan 7, 2026 
Last edited by Asher-XunZhang
Author
More actions
> > > Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang) ,I attended the presentation about Conductor at the last Mooncake meeting, and my understanding is that it is positioned as an L1/L2/L3 multi-tier KV Cache manager that also provides external support for request cache hit rate evaluation. However, this RFC describes Conductor as being responsible for request distribution as well, which seems to overlap with the role of a gateway/router. I’d like to ask two questions:
>>>   1. What is the **precise positioning** of Conductor?
>>>   2. If its core positioning is indeed a KV Cache manager, what are the current plans for **its collaboration with various AI gateways?**
>>> 

>> [@TheManyBELL](https://github.com/TheManyBELL) I apologize for the delayed update of this RFC. The context is as follows: Initially, we planned to implement a router, so the description in the RFC may appear contradictory to last meeting, which could be misleading. Later, we discovered that various implementations such as vLLM, SGLang, and AIGateway already provide mature router solutions, which include load balancing and KV-Cache-aware functionality at the instance level. As a result, we shifted our focus to developing a KV-Cache-aware data structure that integrates the L1/L2 layers of the inference engine instance with the external pooling system's L3 (Mooncake) layer ([#1306](https://github.com/kvcache-ai/Mooncake/pull/1306)) (as shown in the green box labeled `Cache-Aware Scheduler` in the latest architecture diagram). The current implementation is designed as a separate, lightweight HTTP service ([#1248](https://github.com/kvcache-ai/Mooncake/pull/1248)). The scheduler at the Router layer accesses this service to retrieve global information and makes decisions based on other algorithms.
> Thanks for the quick response! Looking forward to this feature!
Regarding the KV Event Publishing System for the Mooncake-Store module, I mistakenly provided the wrong PR yesterday (the one shared was intended for merging). If you need to review the current implementation plan, you can refer to this full PR that was previously used for functional verification: ([#1251](https://github.com/kvcache-ai/Mooncake/pull/1251)).
👍React with 👍1Reacted by Flona
### zxyyy0119 commented on Jan 20, 2026 
More actions
Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang), just wondering about the roadmap: is ZMQ considered the final choice for KV event transport, or is there an intention to move toward something like NATS in the future?
### Asher-XunZhang commented on Jan 20, 2026 
Last edited by Asher-XunZhang
Author
More actions
> Hi [@Asher-XunZhang](https://github.com/Asher-XunZhang), just wondering about the roadmap: is ZMQ considered the final choice for KV event transport, or is there an intention to move toward something like NATS in the future?
Currently, the inference engines all use ZMQ for the KV event mechanism, as detailed in [this issue](https://github.com/kvcache-ai/Mooncake/issues/1408). During our research, we did come across examples like Dynamo, which uses NATS for RAS (<https://github.com/ai-dynamo/dynamo/blob/main/docs/router/kv_cache_routing.md>). However, we currently have no plans to implement a similar approach using NATS. Our near-term consideration is more aligned with something like etcd, though we may revisit and adjust the design in future iterations based on actual deployment needs. BTW, if you have any better suggestions, we welcome you to share and discuss with us!
### ishandhanani commented on Jan 22, 2026 
Collaborator
More actions
[@zxyyy0119](https://github.com/zxyyy0119) [@Asher-XunZhang](https://github.com/Asher-XunZhang) FWIW Dynamo is planning on moving to ZMQ in the next few weeks. We've got a POC for this already since some folks did not like having to use NATS
🚀React with 🚀1Reacted by Asher Zhang
### mich45208 commented on Apr 22, 2026 
Last edited by mich45208
More actions
[@Asher-XunZhang](https://github.com/Asher-XunZhang) curious how is the current status of conductor. From what I can find, there are multiple PRs ready but not merged for some reason.
  1. [[RFC]: Mooncake Store KV Event Integration Solution #1408](https://github.com/kvcache-ai/Mooncake/issues/1408)
  2. [[RFC]: Mooncake Data Plane Model Distribution, Phase 1 #1767](https://github.com/kvcache-ai/Mooncake/issues/1767)
  3. [[Store][Feature] Update KV event publish system #1251](https://github.com/kvcache-ai/Mooncake/pull/1251)
  4. [ [Misc] add mooncake-conductor control-panel #1248](https://github.com/kvcache-ai/Mooncake/pull/1248) are these sufficient enough for kv-aware routing test? or is there any places that more frequent updates are happening?


### Asher-XunZhang commented on Apr 23, 2026 
Author
More actions
> [@Asher-XunZhang](https://github.com/Asher-XunZhang) curious how is the current status of conductor. From what I can find, there are multiple PRs ready but not merged for some reason.
>   1. [[RFC]: Mooncake Store KV Event Integration Solution #1408](https://github.com/kvcache-ai/Mooncake/issues/1408)
>   2. [[RFC]: Mooncake Data Plane Model Distribution, Phase 1 #1767](https://github.com/kvcache-ai/Mooncake/issues/1767)
>   3. [[Store][Feature] Update KV event publish system #1251](https://github.com/kvcache-ai/Mooncake/pull/1251)
>   4. [ [Misc] add mooncake-conductor control-panel #1248](https://github.com/kvcache-ai/Mooncake/pull/1248) are these sufficient enough for kv-aware routing test? or is there any places that more frequent updates are happening?
> 

Currently, the main branch is in the process of introducing some new features (such as replica migration) and modifying old features (such as the eviction mechanism). The `KVEvent` reporting mechanism within Mooncake-Store is coupled with these features and requires corresponding adaptations, which is why it has not yet been merged into the main branch. Could you share the background of your current usage? Perhaps you could consider using the branch `dev/kv-indexer`, which is still under development and specifically targets this feature.
### mich45208 commented on Apr 23, 2026 
More actions
thank you [@Asher-XunZhang](https://github.com/Asher-XunZhang)! we are trying to test kv-aware routing of XpYd on vllm-router for performance based on the conductor (set of PRs: [#1251](https://github.com/kvcache-ai/Mooncake/pull/1251) and [#1248](https://github.com/kvcache-ai/Mooncake/pull/1248)) with vllm, lmcache, and mooncake. Just to verify what's current implementation of conductor can do and performance improvement it can help. Based on KV event publish system, I assume l1/l2/l3 cache should be trackable by conductor, is `dev/kv-indexer` itself sufficient for this?
### Asher-XunZhang commented on Apr 23, 2026 
Author
More actions
> thank you [@Asher-XunZhang](https://github.com/Asher-XunZhang)! we are trying to test kv-aware routing of XpYd on vllm-router for performance based on the conductor (set of PRs: [#1251](https://github.com/kvcache-ai/Mooncake/pull/1251) and [#1248](https://github.com/kvcache-ai/Mooncake/pull/1248)) with vllm, lmcache, and mooncake. Just to verify what's current implementation of conductor can do and performance improvement it can help. Based on KV event publish system, I assume l1/l2/l3 cache should be trackable by conductor, is `dev/kv-indexer` itself sufficient for this?
Yes, because we considered that this component is still under development, the documentation hasn't been updated for too long. Let me clarify a few points now: 1. 【mooncake-store part】 In the `dev/kv-indexer` branch, the currently implemented event reporting mechanism uses a layered reporting design. You can refer to `Scheme 2` in this RFC [#1408](https://github.com/kvcache-ai/Mooncake/issues/1408) for details. The PR [#1251](https://github.com/kvcache-ai/Mooncake/pull/1251) you mentioned is an implementation of `Scheme 1`. The reason for abandoning the first scheme is that it involves interface changes and adds extra data transmission though it can work and doesn't require many modifications in vLLM. 2. 【vllm part】 The second solution implemented in the dev/kv-indexer branch requires some modifications to the connector part of vllm. During our current testing and debugging, we're based on NPU, so we modified the relevant code in the vllm-ascend repository. You can refer to [#6593](https://github.com/vllm-project/vllm-ascend/pull/6593) for details. However, it seems that some bug fixes have not been merged into the vllm-ascend repository, and the vllm-ascend repository itself has some functional issues related to layerwise that haven't been adapted yet. But progress is being made recently, and [@yejj710](https://github.com/yejj710) could provide more details. 3. 【mooncake-conductor part】 In the `dev/kv-indexer` branch, the latest API design for the indexer can be found in issue [#1403](https://github.com/kvcache-ai/Mooncake/issues/1403).
### yejj710 commented on Apr 23, 2026 
Collaborator
More actions
[@mich45208](https://github.com/mich45208) Thank you for your attention. You can get `mooncake-conductor` on this branch `/dev/kv-indexer` (commit-id: [`c67649c`](https://github.com/kvcache-ai/Mooncake/commit/c67649c83a44b2ebb465bc36df734523deb34ebd)). Considering that there is currently no comprehensive docs on how to use `mooncake-conductor`, I have started a discussion([#1967](https://github.com/kvcache-ai/Mooncake/discussions/1967)), and we will support you in completing the usage tasks~ cc [@Asher-XunZhang](https://github.com/Asher-XunZhang)
👍React with 👍1Reacted by Stary
🚀React with 🚀2Reacted by Asher Zhang and mich45208
### github-actions commented on Jul 23, 2026 
[on Jul 23, 2026](https://github.com/kvcache-ai/Mooncake/issues/977#issuecomment-5054211586) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue has had no activity for 90 days and will be closed in 7 days if there is no further activity. Please comment or react if it should stay open.
[github-actions](https://github.com/apps/github-actions)
added [on Jul 23, 2026](https://github.com/kvcache-ai/Mooncake/issues/977#event-28361806897)
### github-actions commented on Jul 30, 2026 
[on Jul 30, 2026](https://github.com/kvcache-ai/Mooncake/issues/977#issuecomment-5126344559) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
Closing due to 3 months of inactivity. If this is still relevant, please comment and we can reopen.
[github-actions](https://github.com/apps/github-actions)
added [on Jul 30, 2026](https://github.com/kvcache-ai/Mooncake/issues/977#event-28699348873)
[github-actions](https://github.com/apps/github-actions)
closed this as [not planned](https://github.com/kvcache-ai/Mooncake/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Anot-planned)[on Jul 30, 2026](https://github.com/kvcache-ai/Mooncake/issues/977#event-28699349340)
[Sign up for free](https://github.com/signup?return_to=https://github.com/kvcache-ai/Mooncake/issues/977)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kvcache-ai/Mooncake/issues/977)
## Metadata
## Metadata
### Assignees
  * [chestnut-Q](https://github.com/chestnut-Q)


### Labels
[auto-closed](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3A%22auto-closed%22)[good first issueGood for newcomers](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3A%22good%20first%20issue%22)Good for newcomers[](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3A%22stale%22)
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
