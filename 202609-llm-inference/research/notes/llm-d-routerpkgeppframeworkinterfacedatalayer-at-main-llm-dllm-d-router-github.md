---
title: llm-d-router/pkg/epp/framework/interface/datalayer at main · llm-d/llm-d-router
  · GitHub
id: llm-d-routerpkgeppframeworkinterfacedatalayer-at-main-llm-dllm-d-router-github
tags:
- llmd-router-nonk8s-backends-54b774
- data-layer
- navigational
created: '2026-09-11T21:47:12.242139Z'
updated: '2026-09-12T03:37:37.796232Z'
source: https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/interface/datalayer
source_domain: github.com
fetched_at: '2026-09-11T21:47:12.218642Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub directory listing of llm-d-router pkg/epp/framework/interface/datalayer
  at main (fetched Sep 11 2026) — the package that defines the EPP''s data-layer interfaces
  (EndpointDiscovery, DiscoveryNotifier, EndpointMetadata, the DataSource/Extractor
  pairs, CrossReplicaSyncer, peer types). Crawl captured GitHub''s file-tree chrome
  rather than file contents, so its value is navigational plus one dated signal: the
  directory''s latest commit is 6592a6f, ''feat(datalayer): typed attribute slots
  and value-safety registry'' (PR #2352, Sep 11 2026, tracking issue #2338), which
  adds compile-time-typed Slot[T] attribute keys and a startup registry that validates
  every consumer''s Consumes() declaration against the producing plugin''s declared
  key and value type — the value-safety complement to PR #2190''s key-safety check,
  and evidence that the plugin/data-layer interface surface was still changing on
  the day of this run. Prefer the individually fetched Go sources (discovery.go, endpoint_metadata.go,
  cross_replica_syncer.go, peer.go) for interface detail.'
---

[Skip to content](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/interface/datalayer#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/interface/datalayer) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/interface/datalayer) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/interface/datalayer) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/interface/datalayer).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# datalayer
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[feat(datalayer): typed attribute slots and value-safety registry (](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e)[#2352](https://github.com/llm-d/llm-d-router/pull/2352)[)](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e)
Open commit detailsfailure
Sep 11, 2026
[6592a6f](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e) · Sep 11, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/interface/datalayer)
Open commit details
History
/
# datalayer
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
|   |   |  [feat(datalayer): typed attribute slots and value-safety registry (](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2352](https://github.com/llm-d/llm-d-router/pull/2352)[)](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [refactor(datalayer): key AttributeMap by DataKey instead of string (](https://github.com/llm-d/llm-d-router/commit/800ec0e453d8b962ca0af6f03d3bfbe8e685350c "refactor\(datalayer\): key AttributeMap by DataKey instead of string \(#2190\)

* refactor\(datalayer\): key AttributeMap by DataKey instead of string

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* enforce\(requestcontrol\): scope DataProducer Put/Get to declared keys

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* enforce\(framework\): confine plugin attribute access to declared keys

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* refactor\(framework\): confine plugin attribute access to declared keys

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* refactor\(datalayer\): surface plugin data-scope violations

Move runtime confinement alongside the static dependency validation it
mirrors, so both halves of the Produces\(\)/Consumes\(\) contract live in one
package.

A rejected access surfaced only as a log line, and a rejected read logged
one per endpoint per request. Count every rejection under
llm_d_epp_plugin_data_scope_violations_total and log the first offence of
each kind per invocation, so a misdeclared plugin stays visible in
production without depending on log verbosity.

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* fix\(endpointattribute\): reject the combined Attribute/Producer spelling

The attribute name and its producer are separate parameters, but they were
once a single "Attribute/Producer" string. A key built from that spelling
matches nothing: the read resolves as absent, so the filter keeps every
endpoint and the scorer returns zero, with no error anywhere.

Reject it at construction and name the split in the message. A producer
pointer tells an omitted producer from one set to the empty string, so an
attribute whose own name contains a slash stays reachable.

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

---------

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>")[#…](https://github.com/llm-d/llm-d-router/pull/2190)  | Aug 12, 2026  |  
|   |   | [fix(CrossReplicaSyncer): disentangle CrossReplicaSyncing interval fro…](https://github.com/llm-d/llm-d-router/commit/bf0cadf617523fc27f447b6d301596acf7e5ad3d "fix\(CrossReplicaSyncer\): disentangle CrossReplicaSyncing interval from main ticket in PoolingDispatcher \(#2310\)

* fix: decouple cross-replica sync from collector base tick

The crossReplicaPublisher was registered as a PollingDispatcher on the
shared collector, whose base tick is set by refresh-metrics-interval
\(typically 60s for vLLM metric scraping\). This silently clamped the
cross-replica sync interval to 60s regardless of the configured
crossReplicaSyncInterval, because periodTicks\(1ms, 60s\) rounds to 1
tick = 60s.

Give the publisher its own per-endpoint goroutine with an independent
time.Ticker at the configured sync interval. This decouples lightweight
Redis state sync \(two int64s per endpoint\) from heavy HTTP metric
scraping, so the two can run at their natural cadences.

Before: crossReplicaSyncInterval: 50ms → silently fires at 60s
After:  crossReplicaSyncInterval: 50ms → fires at 50ms
Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* improving comments

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* chore: drop unused crossReplicaPublisherType const

The publisher no longer registers as a named polling dispatcher, so
TypedName\(\) was removed and the type-name const became dead code.

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* fix\(datalayer\): sync cross-replica state from one goroutine, not one per endpoint

The PollingDispatcher is per endpoint because it establishes and polls a
connection per endpoint. Cross-replica syncing does neither: it reads local
state and hands it to the syncer, so one goroutine iterating the endpoints is
sufficient.

Doing it per endpoint was also leaking. The goroutine only returned on
ctx.Done\(\), and NewEndpoint receives the process-lifetime parentCtx, so
ReleaseEndpoint stopped the collector while the ticker kept publishing state
for an endpoint that no longer existed. Iterating the live set each tick
removes both the leak and the stale writes, with no teardown to get wrong.

The collector registry now stores each endpoint alongside its Collector, so the
live set can be enumerated without a second registry to keep in step. The entry
is immutable once stored, so the read path needs no lock.

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* refactor\(datalayer\): encapsulate cross-replica publishing

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* fix\(datalayer\): order cross-replica state updates

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* feat\(datalayer\): configure cross-replica publish timeout

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* Restore local cross-replica syncer

Keep the in-memory implementation available for single-replica deployments and tests under a new path compatible with upstream mock removal.

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* Keep cross-replica publisher methods private

The publisher is an internal runtime helper, so keep its methods package-private and remove the unused error return from endpoint event handling.

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* Continue endpoint cleanup after publisher miss

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* Package local cross-replica syncer

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

* Restore cross-replica sync comment

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>

---------

Signed-off-by: Lucas-Fernandes-Martins <lucasfmartins16@gmail.com>")  | Sep 4, 2026  |  
|   |   |  [FileDiscovery plugin (4/4) - wiring into the runner (](https://github.com/llm-d/llm-d-router/commit/bda7c04a9f8b4de93c71ed01a0990f0f7b5a3695 "FileDiscovery plugin \(4/4\) - wiring into the runner \(#1218\)

* runner: add file discovery path with RunnableGroup, resolveDiscovery, and serveMetrics

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* gate request serving on EndpointDiscovery.Ready\(\) to fix sync issues

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* serve metrics from controller-runtime registry with bounded shutdown

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* treat unset pool AppProtocol as no constraint in health check

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* log inactive Kubernetes-only features at file-discovery startup

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* document pool namespace handling and warn on default fallback in file mode

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* avoid double parse and duplicate startup logs in K8s mode

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* fix resolve_discovery_test.go imports for renamed module

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* docs\(discovery\): drop metricsHost references and tighten IPv4 constraint

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* fix\(runner\): drop stray blank line in runWithFileDiscovery

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* fix\(runner\): look up discovery plugin from handle instead of re-instantiating

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* docs\(runner\): document phaseOne idempotency at the early-return guard

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* refactor\(profiling\): share pprof handler set between K8s and file-discovery paths

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* test\(runner\): add smoke test for runWithFileDiscovery wiring

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* fix\(runner\): adapt resolve_discovery_test to upstream *json.Decoder Factory signature

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

---------

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>")[#1218](https://github.com/llm-d/llm-d-router/pull/1218)[)](https://github.com/llm-d/llm-d-router/commit/bda7c04a9f8b4de93c71ed01a0990f0f7b5a3695 "FileDiscovery plugin \(4/4\) - wiring into the runner \(#1218\)

* runner: add file discovery path with RunnableGroup, resolveDiscovery, and serveMetrics

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* gate request serving on EndpointDiscovery.Ready\(\) to fix sync issues

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* serve metrics from controller-runtime registry with bounded shutdown

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* treat unset pool AppProtocol as no constraint in health check

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* log inactive Kubernetes-only features at file-discovery startup

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* document pool namespace handling and warn on default fallback in file mode

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* avoid double parse and duplicate startup logs in K8s mode

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* fix resolve_discovery_test.go imports for renamed module

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* docs\(discovery\): drop metricsHost references and tighten IPv4 constraint

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* fix\(runner\): drop stray blank line in runWithFileDiscovery

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* fix\(runner\): look up discovery plugin from handle instead of re-instantiating

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* docs\(runner\): document phaseOne idempotency at the early-return guard

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* refactor\(profiling\): share pprof handler set between K8s and file-discovery paths

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* test\(runner\): add smoke test for runWithFileDiscovery wiring

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

* fix\(runner\): adapt resolve_discovery_test to upstream *json.Decoder Factory signature

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>

---------

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>")  | May 25, 2026  |  
|   |   |  [epp: generalize endpoint identity (](https://github.com/llm-d/llm-d-router/commit/0c3d3ff0b315c62b01d15d9f346d150c62050e7e "epp: generalize endpoint identity \(#2147\)

* epp: generalize endpoint identity

Rename EndpointMetadata.PodName to Name. The endpoint model is not Kubernetes-only:
file-discovered endpoints have no pod, and NamespacedName is the identity. Name is
always set, so an endpoint has a name regardless of discovery source.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: rename endpoint identity to ID

Rename EndpointMetadata.NamespacedName to ID, the endpoint's unique identity,
with GetID as the accessor. ID is a type alias for types.NamespacedName so the
identity can later move off the Kubernetes-specific type without churning
datastore keys. The discovery docs state that each source must set ID uniquely.

Continues the endpoint-identity generalization from the PodName rename.
EndpointMetadata is an internal framework type, so this is a plain rename with no
deprecation shim.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")[#2147](https://github.com/llm-d/llm-d-router/pull/2147)[)](https://github.com/llm-d/llm-d-router/commit/0c3d3ff0b315c62b01d15d9f346d150c62050e7e "epp: generalize endpoint identity \(#2147\)

* epp: generalize endpoint identity

Rename EndpointMetadata.PodName to Name. The endpoint model is not Kubernetes-only:
file-discovered endpoints have no pod, and NamespacedName is the identity. Name is
always set, so an endpoint has a name regardless of discovery source.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: rename endpoint identity to ID

Rename EndpointMetadata.NamespacedName to ID, the endpoint's unique identity,
with GetID as the accessor. ID is a type alias for types.NamespacedName so the
identity can later move off the Kubernetes-specific type without churning
datastore keys. The discovery docs state that each source must set ID uniquely.

Continues the endpoint-identity generalization from the PodName rename.
EndpointMetadata is an internal framework type, so this is a plain rename with no
deprecation shim.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")  | Aug 2, 2026  |  
|   |   | [Move datalayer plugin intefaces and associated types to framework/int…](https://github.com/llm-d/llm-d-router/commit/84106f8f6c8744ec4ce2cbd61e18ab67d681b626 "Move datalayer plugin intefaces and associated types to framework/interface/datalayer \(kubernetes-sigs/gateway-api-inference-extension#2228\)")  | Jan 28, 2026  |  
|   |   | [feat: implement EPP plugin stability lifecycle and --allow-experiment…](https://github.com/llm-d/llm-d-router/commit/8791c008bb7ae6fc7bdee0bba69e7de96eb4c13c "feat: implement EPP plugin stability lifecycle and --allow-experimental-plugins flag \(#1912\) \(#2073\)

* feat: implement EPP plugin stability lifecycle and experimentalPlugins feature gate \(#1912\)

Signed-off-by: Cong Liu <conliu@google.com>

* mark session affinity plugins alpha - they have not been used and are under major refactoring
Signed-off-by: Cong Liu <conliu@google.com>

* fix: restore sessionaffinity filter and scorer to StabilityBeta
Signed-off-by: Cong Liu <conliu@google.com>

* fix: set sessionaffinity filter and scorer to StabilityAlpha and enable experimentalPlugins in test
Signed-off-by: Cong Liu <conliu@google.com>

* Merge origin/main into plugin-stability-lifecycle
Signed-off-by: Cong Liu <conliu@google.com>

* test: enable experimentalPlugins featureGate for test configs using Alpha plugins
Signed-off-by: Cong Liu <conliu@google.com>

* feat: replace experimentalPlugins feature gate with --allow-experimental-plugins CLI flag
Signed-off-by: Cong Liu <conliu@google.com>

* feat: enforce experimental plugin check on auto-created system defaults and data producers
Signed-off-by: Cong Liu <conliu@google.com>

* test: remove experimentalPlugins feature gate reference from coordinator configs_test.go
Signed-off-by: Cong Liu <conliu@google.com>

* refactor: validate plugin stability once after all plugin initialization
Signed-off-by: Cong Liu <conliu@google.com>

* fix: remove obsolete activerequest filter package from branch \(replaced by utilization filter in upstream #2218\)
Signed-off-by: Cong Liu <conliu@google.com>

* refactor\(runner\): move plugin stability validation out of configuration parsing to run after all phases complete
Signed-off-by: Cong Liu <conliu@google.com>

* refactor\(runner\): remove unused opts parameter from parseConfigurationPhaseTwo
Signed-off-by: Cong Liu <conliu@google.com>

* docs & test: address PR #2073 review feedback
Signed-off-by: Cong Liu <conliu@google.com>

---------

Signed-off-by: Cong Liu <conliu@google.com>")  | Aug 3, 2026  |  
|   |   |  [epp: generalize endpoint identity (](https://github.com/llm-d/llm-d-router/commit/0c3d3ff0b315c62b01d15d9f346d150c62050e7e "epp: generalize endpoint identity \(#2147\)

* epp: generalize endpoint identity

Rename EndpointMetadata.PodName to Name. The endpoint model is not Kubernetes-only:
file-discovered endpoints have no pod, and NamespacedName is the identity. Name is
always set, so an endpoint has a name regardless of discovery source.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: rename endpoint identity to ID

Rename EndpointMetadata.NamespacedName to ID, the endpoint's unique identity,
with GetID as the accessor. ID is a type alias for types.NamespacedName so the
identity can later move off the Kubernetes-specific type without churning
datastore keys. The discovery docs state that each source must set ID uniquely.

Continues the endpoint-identity generalization from the PodName rename.
EndpointMetadata is an internal framework type, so this is a plain rename with no
deprecation shim.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")[#2147](https://github.com/llm-d/llm-d-router/pull/2147)[)](https://github.com/llm-d/llm-d-router/commit/0c3d3ff0b315c62b01d15d9f346d150c62050e7e "epp: generalize endpoint identity \(#2147\)

* epp: generalize endpoint identity

Rename EndpointMetadata.PodName to Name. The endpoint model is not Kubernetes-only:
file-discovered endpoints have no pod, and NamespacedName is the identity. Name is
always set, so an endpoint has a name regardless of discovery source.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: rename endpoint identity to ID

Rename EndpointMetadata.NamespacedName to ID, the endpoint's unique identity,
with GetID as the accessor. ID is a type alias for types.NamespacedName so the
identity can later move off the Kubernetes-specific type without churning
datastore keys. The discovery docs state that each source must set ID uniquely.

Continues the endpoint-identity generalization from the PodName rename.
EndpointMetadata is an internal framework type, so this is a plain rename with no
deprecation shim.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")  | Aug 2, 2026  |  
|   |   | [fix(lora-affinity): correct ActiveModels docs and remove wrong capaci…](https://github.com/llm-d/llm-d-router/commit/2d7e7401dc283fa2f5d386f20140fca4c4a5de6d "fix\(lora-affinity\): correct ActiveModels docs and remove wrong capacity \(#2625\)

* fix\(lora-affinity\): correct ActiveModels docs and remove wrong capacity

- correct doc comments to clarify ActiveModels tracks in-flight request
  activity, not GPU residency
- remove capacity check that compared activity counts against slot count,
  falsely reporting spare capacity when loaded adapters are idle
- update waiting-adapter score from 0.6 to 0.8 to reflect that queued
  requests are a strong signal the adapter is loaded \(close to 1.0\)

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: code review

-  correct docs comment to clarify ActiveModels and WaitingModels carry
   the same adapters from vLLM
-  keep original score by put back 0.8 and 0.6: when 0.8 means union of
   ActiveModels and WaitingModels < max_lora => still room to load it,
   this fix the double counte of active and waiting
-  add a new testcase to map vLLM output

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Sep 9, 2026  |  
|   |   | [Move datalayer plugin intefaces and associated types to framework/int…](https://github.com/llm-d/llm-d-router/commit/84106f8f6c8744ec4ce2cbd61e18ab67d681b626 "Move datalayer plugin intefaces and associated types to framework/interface/datalayer \(kubernetes-sigs/gateway-api-inference-extension#2228\)")  | Jan 28, 2026  |  
|   |   | [config: consolidate discovery plugins under a single discovery sectio…](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19 "config: consolidate discovery plugins under a single discovery section \(follow up #2341\) \(#2434\)

* config: consolidate discovery plugins under a single discovery section

Group endpoint discovery and peer discovery under dataLayer.discovery
with endpoints and peers sub-fields instead of two separate top-level
fields \(dataLayer.discovery and dataLayer.peerDiscovery\).

A migration shim in the config loader preserves backward compatibility:
- dataLayer.discovery.pluginRef migrates to discovery.endpoints.pluginRef
- dataLayer.peerDiscovery migrates to discovery.peers

Follows the existing deprecation pattern used for parser and
saturationDetector fields.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

* config: remove pre-release peerDiscovery deprecation shim

The dataLayer.peerDiscovery field was introduced in #2341 but never
shipped in a release, so no users depend on it. Remove the field, its
migration shim, and its test instead of carrying unnecessary deprecation
machinery. The endpoint discovery pluginRef migration \(which IS released\)
is preserved.

Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

* address review feedback on discovery config migration

Add "If both are set, the new field is used." to the discovery
pluginRef deprecation message, matching the saturationDetector and
parser migration patterns. Extract a local variable in runner.go
to reduce repetition of the DataLayer field path.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

---------

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>
Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>")  | Aug 25, 2026  |  
|   |   |  [feat: EPP peer discovery infrastructure (](https://github.com/llm-d/llm-d-router/commit/620045bb19a95b088dc53f046c78a2d712b90a76 "feat: EPP peer discovery infrastructure \(#2341\)

* refactor: EPP peer discovery via EndpointSlices

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* address comments

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* docstring

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* ensure rbac is gated only when peer discovery is enabled

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* ensure rbac is gated only when peer discovery is enabled \(kustomize overlay\)

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* refactor: use config instead of flags

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* restore to main to rescope + address comments

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

---------

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>")[#2341](https://github.com/llm-d/llm-d-router/pull/2341)[)](https://github.com/llm-d/llm-d-router/commit/620045bb19a95b088dc53f046c78a2d712b90a76 "feat: EPP peer discovery infrastructure \(#2341\)

* refactor: EPP peer discovery via EndpointSlices

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* address comments

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* docstring

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* ensure rbac is gated only when peer discovery is enabled

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* ensure rbac is gated only when peer discovery is enabled \(kustomize overlay\)

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* refactor: use config instead of flags

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

* restore to main to rescope + address comments

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>

---------

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>")  | Aug 16, 2026  |  
|   |   |  [feat(datalayer): configurable per-source scrape interval (](https://github.com/llm-d/llm-d-router/commit/d4782911a2b6e10e59ba5c3f964d1a2fb0dd5989 "feat\(datalayer\): configurable per-source scrape interval \(#2214\)

* feat\(datalayer\): allow per-source scrape intervals
Polling sources shared one Collector tick \(~50ms\), so slow-changing
sources \(DCGM, /v1/models\) were scraped far more often than useful.
Schedule each dataLayer.sources entry by a tick multiple of
--refresh-metrics-interval; omit keeps every-tick behavior.

Signed-off-by: noalimoy <nlimoy@redhat.com>

* feat\(datalayer\): move interval to plugin parameters

Signed-off-by: noalimoy <nlimoy@redhat.com>

* fix\(datalayer\): round intervals and enforce 50ms base-tick floor

Signed-off-by: noalimoy <nlimoy@redhat.com>

* refactor\(datalayer\): use intervalDispatcher decorator for per-source scheduling

Replace ScheduledDispatcher with a transparent intervalDispatcher
decorator. Configure pre-scans source intervals and lowers the base
tick to the smallest one \(floored at 50ms\) so no source is forced to
run slower than its configured cadence. Each endpoint gets its own
decorator clone to avoid shared state across Collector goroutines.

Signed-off-by: noalimoy <nlimoy@redhat.com>

---------

Signed-off-by: noalimoy <nlimoy@redhat.com>")[#2214](https://github.com/llm-d/llm-d-router/pull/2214)[)](https://github.com/llm-d/llm-d-router/commit/d4782911a2b6e10e59ba5c3f964d1a2fb0dd5989 "feat\(datalayer\): configurable per-source scrape interval \(#2214\)

* feat\(datalayer\): allow per-source scrape intervals
Polling sources shared one Collector tick \(~50ms\), so slow-changing
sources \(DCGM, /v1/models\) were scraped far more often than useful.
Schedule each dataLayer.sources entry by a tick multiple of
--refresh-metrics-interval; omit keeps every-tick behavior.

Signed-off-by: noalimoy <nlimoy@redhat.com>

* feat\(datalayer\): move interval to plugin parameters

Signed-off-by: noalimoy <nlimoy@redhat.com>

* fix\(datalayer\): round intervals and enforce 50ms base-tick floor

Signed-off-by: noalimoy <nlimoy@redhat.com>

* refactor\(datalayer\): use intervalDispatcher decorator for per-source scheduling

Replace ScheduledDispatcher with a transparent intervalDispatcher
decorator. Configure pre-scans source intervals and lowers the base
tick to the smallest one \(floored at 50ms\) so no source is forced to
run slower than its configured cadence. Each endpoint gets its own
decorator clone to avoid shared state across Collector goroutines.

Signed-off-by: noalimoy <nlimoy@redhat.com>

---------

Signed-off-by: noalimoy <nlimoy@redhat.com>")  | Jul 30, 2026  |  
|   |   | [refactor(datalayer): typed Extractor[T] and source-as-dispatcher poll…](https://github.com/llm-d/llm-d-router/commit/0026d0fb52932234230d07ee1803c42ce0716052 "refactor\(datalayer\): typed Extractor\[T\] and source-as-dispatcher polling \(#1073\)

* epp/datalayer: typed Extractor\[T\] + PollingDispatcher refactor

Replaces ExtractorBase / PollingDataSource / reflect-based type compat
with generic Extractor\[T\] and PollingDispatcher. HTTPDataSource becomes
HTTPDataSource\[T any\] owning its extractors and running them via
Dispatch per tick.

Sources specialize via typed parsers \(PrometheusMetricMap,
*ModelResponse\). Extractor implementations replace ExtractEndpoint /
ExtractNotification with the unified Extract method on Extractor\[T\].

PollingDispatchers store in a dedicated map that errors on duplicate
name. Notification/endpoint sources keep variantSourceMap.
extractorMap broadens to plugin.Plugin; validateSourceExtractors adds
variant-type assertions inline and rejects sources that implement
multiple variant interfaces.

Collector records DataLayerPollErrorsTotal on dispatcher failure; the
dispatcher records extract failures itself per the PollingDispatcher
contract.

Adds PollingExtractor\[T\] alias and uses slices.Clone for the
per-Dispatch extractor snapshot.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp/datalayer: tests for typed Extractor\[T\] + PollingDispatcher

Updates tests across runtime, collector, manager, sources, extractors,
callsites, and integration tests to the new typed contract.
configloader and integration tests use plugin.Plugin for the Extractors
slice.

Adds TestExtractorMap_ConcurrentAppendDedupsByType pinning the
read-then-write dedup under concurrency.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")  | May 28, 2026  |  
|   |   |  [feat(datalayer): typed attribute slots and value-safety registry (](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2352](https://github.com/llm-d/llm-d-router/pull/2352)[)](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat(datalayer): typed attribute slots and value-safety registry (](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2352](https://github.com/llm-d/llm-d-router/pull/2352)[)](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat(datalayer): typed attribute slots and value-safety registry (](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2352](https://github.com/llm-d/llm-d-router/pull/2352)[)](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat(datalayer): typed attribute slots and value-safety registry (](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2352](https://github.com/llm-d/llm-d-router/pull/2352)[)](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
| View all files |  
You can’t perform that action at this time. 
