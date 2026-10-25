---
title: gateway-api-inference-extension/docs/proposals/0683-epp-architecture-proposal
  at main · kubernetes-sigs/gateway-api-inference-extension · GitHub
id: gateway-api-inference-extensiondocsproposals0683-epp-architecture-proposal-at-ma
tags:
- llmd-router-nonk8s
- epp-architecture
- data-layer
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:43:09.674039Z'
updated: '2026-09-15T04:55:23.671348Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/0683-epp-architecture-proposal
source_domain: github.com
fetched_at: '2026-09-11T19:43:09.652130Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Proposal 0683 (author @kfswain, status: Implemented) that restructures the
  EPP into modular layers: a non-extensible Ext-Proc Server (deviation ''ill-advised''
  since it must track the well-defined ext_proc protocol precisely) plus four extensible
  layers — Data Layer, Routing Layer, Flow Controller, Scheduling Layer. The Data
  Layer is called out as the one layer ''accessed by all the others'': it ''consumes
  and stores the InferencePool/InferenceModel config and the pre-defined Model Server
  Protocol,'' processes model-server-fed data into resource-scarcity-regime hints
  and autoscaling recommendations, and is explicitly built to be extended — but ''extenders
  accept that the Model Server Protocol will no longer provide guarantees on portability
  of a model server out of the box'' once extended. This is the architectural seam
  a router customizer would target to swap in a non-k8s-pod data source. Routing Layer
  handles per-InferenceModel routing rules (system-prompt injection, RAG callout,
  request validation) with a deliberately tight API scope to avoid becoming a dumping
  ground for custom rules.'
utility_score: 15.0
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/0683-epp-architecture-proposal#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/0683-epp-architecture-proposal) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/0683-epp-architecture-proposal) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/0683-epp-architecture-proposal) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/0683-epp-architecture-proposal).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# 0683-epp-architecture-proposal
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
success
Aug 26, 2025
[47ead4c](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/47ead4c70e28475b70eaaa91847a93fc03fb1602) · Aug 26, 2025
## History
[History](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commits/main/docs/proposals/0683-epp-architecture-proposal)
Open commit details
History
/
# 0683-epp-architecture-proposal
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
|   |   |  [Updating proposal directories to match their PR number (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/1a871729f3af64840aac85b4cf7f861880f35a8a "Updating proposal directories to match their PR number \(#741\)")[#741](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/741)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/1a871729f3af64840aac85b4cf7f861880f35a8a "Updating proposal directories to match their PR number \(#741\)")  | Apr 25, 2025  |  
|   |   |  [Updating proposal statuses (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/47ead4c70e28475b70eaaa91847a93fc03fb1602 "Updating proposal statuses \(#1472\)")[#1472](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1472)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/47ead4c70e28475b70eaaa91847a93fc03fb1602 "Updating proposal statuses \(#1472\)")  | Aug 26, 2025  |  
| View all files |  
## [README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/0683-epp-architecture-proposal#readme)
Outline
# EPP Architecture Proposal
Author(s): @kfswain
## Proposal Status
_**Implemented**_
## Summary
This proposal seeks to standardize the implementation of an EPP (End-point Picker) for the Inference Gateway extension (also known as Gateway API Inference Extension). Additionally, this proposes to restructure the current implementation of the EPP to be more modular, and approachable.
## Goals
  * Set a standard on how the EPP & APIs interact
  * Settle on common nomenclature for clearer communication
  * Allow for modularization of the EPP, to be extended to a user's specific needs


## Non-Goals
  * Reshaping the current API
  * A change in scope of the current project


## Proposal
This proposal is not proposing any net new features, instead, we are refactoring our current implementation to better handle more devs, more features, etc. At the time of writing, GIE is currently at v0.3, and that stronger experimental context (along with external feedback) made clear the need this restructure. The image below give a high level view of how our components work together.
## Overview
At a quick glance, the EPP is being broken into specific layers. The `Data Layer` is of note, as it is a vertical that will be accessed by all the others. The data layer manages the k8s, data, metric & usage data, as well as processing of the above data to determine resource scarcity regimes.
The other layers are handled in sequential process. Starting with the **Ext-Proc** call. The request is buffered and then sent to the **Routing Layer** , which processes any User defined per-InferenceModel routing rules & request enrichment happening first (at the time of writing that is currently just translating the InferenceModel name to a weight-split actual model). Then _all_ requests pass through the to-be-implemented [**Flow Controller**](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/674) to ensure that any request entry to the pool adhereing to the guidelines set by the Priority, Fairness, & Queueing configuration. And finally, the **Scheduling Layer** is the load balancing algorithm that intelligently routes requests based on the current state of the InferencePool.
## Components
To further expand upon these component layers. We will first break them into `extensible` and `non-extensible` layers. `Non-extensible` layers are intended to be static, and handled on behalf of the user, typically implementing low-opinion infrastructure.
The `Extensible` layers are:
  * Data Layer
  * Routing Layer
  * Flow Controller
  * Scheduling Layer


The `Non-Extensible` layer(s) are:
  * The Ext-Proc Server


### `Extensible`
#### Data Layer
The data layer will consume and store: the InferencePool/InferenceModel config and the pre-defined [Model Server Protocol](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/003-model-server-protocol/README.md). Additionally, the data fed from the model servers will be processed and digested to provide resource scarcity regime hints, and autoscaling reccomendations.
Many extensions to scheduling will require changes to ingested metrics, as such, the data layer will be built to be extended, but extenders accept that the Model Server Protocol will no longer provide guarantees on portability of a model server out of the box.
#### Routing Layer
The routing layer is likely to be the most opinion heavy section, as the scope of what constitutes a 'Route Rule' is somewhat broad. The current examples we expect would be:
  * System Prompt injection
  * RAG callout
  * Per-InferenceModel request validation (such as saftey/on-topic, etc)


Due to the possibility of this becoming a bit of a dumping ground. The API will keep a _very_ tight scope on which of these route rules are included in the spec. A standard method of extension will be provided if the need to define a custom rule arises.
#### Flow Controller (WIP - implementation tracked in [#674](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/674))
The flow controller will consume resource regime data, and enforce proper resource sharing between workloads. This will primarily be done through a queuing mechanism [as described here](https://docs.google.com/document/d/1VZL7opFWuwgWquvgiOzLlXAJ633qZ9U-A0ZixGjBgaI/edit?usp=sharing).
### `Non-extensible`
#### Ext-Proc Server
The Ext-Proc Server protocol is very well defined & specific, deviation could cause the EPP to become unusable or unstable. Extension is ill-advised.
You can’t perform that action at this time. 
