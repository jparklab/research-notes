---
title: llm-d-router/pkg/epp/framework/plugins/datalayer/discovery/file at main · llm-d/llm-d-router
  · GitHub
id: llm-d-routerpkgeppframeworkpluginsdatalayerdiscoveryfile-at-main-llm-dllm-d-rout
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- file-discovery
- go-source
- multi-cluster
created: '2026-09-11T20:48:43.978261Z'
updated: '2026-09-15T04:55:23.835824Z'
source: https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/datalayer/discovery/file
source_domain: github.com
fetched_at: '2026-09-11T20:48:43.955138Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE: pkg/epp/framework/plugins/datalayer/discovery/file/README.md
  on llm-d/llm-d-router main branch (fetched Sep 11 2026, unpinned to a commit --
  treat as current-as-of-fetch). This is the authoritative, most detailed spec of
  the FileDiscovery plugin (Type: file-discovery, Interface: EndpointDiscovery), superseding
  the looser prose in the llm-d.ai blog. Key mechanics: on Start, the file is read
  once, each entry validated (address must be a parseable IP, port in [1,65535]) and
  applied via notifier.Upsert; a bad single entry is logged and skipped (non-fatal),
  but file-level problems (cannot open, cannot parse, size > 1 MiB) ABORT STARTUP.
  With watchFile: true, fsnotify Write/Create/Remove events trigger reload, explicitly
  handling atomic-rename and ConfigMap-style symlink swaps (which destroy the watched
  inode) by re-attaching the watcher; endpoints present in the old load but absent
  from the new one are removed via notifier.Delete. Ready() closes only after the
  first successful load. GIVES THE EXACT FILE SCHEMA: endpoints: - name (required,
  unique in file), namespace (optional, defaults "default"), address (required, must
  be a valid IPv4 address -- hostnames NOT resolved, IPv6 NOT supported), port (required,
  string, integer 1-65535), labels (optional key/value map). CONFIG PATH DISCREPANCY
  (three-way, flag for the report): this README places the selector at dataLayer.discovery.endpoints.pluginRef
  (shown in a worked example alongside a plugins: - type: file-discovery entry) --
  differing from BOTH the blog''s dataLayer.discovery.pluginRef AND the discovery.go
  source comment''s EndpointPickerConfig.discovery.pluginRef; the discovery/endpoints
  nesting in this README is the most concrete and should be treated as most authoritative
  since it comes with a full worked example, but all three should be verified against
  a live default-plugins.yaml before being presented as certain. STATED LIMITATIONS
  (verbatim, critical for a customization plan): endpoints file capped at 1 MiB; address
  must be literal IPv4 only; metrics are scraped from the same address:port that serves
  inference (no separate metrics endpoint supported); and -- new information not in
  the blog -- ''File-discovery mode runs the EPP without a Kubernetes controller manager,
  so several K8s-only features are inactive: the InferenceModelRewrite and InferenceObjective
  reconcilers do not run, and any k8s-notification-source plugin in the data layer
  config will not bind. The runner emits a startup log naming the inactive features.''
  Also documents a previously-undocumented MULTI-CLUSTER VARIANT: multicluster-file-discovery,
  a cluster-scoped plugin that discovers peer CLUSTERS (not pods) as endpoints reached
  via gateway host; unlike the pod variant, address may be an RFC-1123 hostname OR
  an IP (not IPv4-only), and a peer''s pool metrics can be scraped from a different
  host/port via metricsAddress/metricsPort labels.'
utility_score: 16.0
---

*Suggested by [[llm-d-router-module-githubcomllm-dllm-d-router-go-packages]] — FileDiscovery plugin implementation package identified via pkg.go.dev index*

[Skip to content](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/datalayer/discovery/file#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/datalayer/discovery/file) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/datalayer/discovery/file) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/datalayer/discovery/file) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/datalayer/discovery/file).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# file
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[config: consolidate discovery plugins under a single discovery sectio…](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19)
Open commit detailsfailure
Aug 25, 2026
[950c963](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19) · Aug 25, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/datalayer/discovery/file)
Open commit details
History
/
# file
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
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
|   |   |  [epp: multicluster config-load validation + review cleanups (](https://github.com/llm-d/llm-d-router/commit/9691af2f68251504ab4398e95d5e51c2abc59f23 "epp: multicluster config-load validation + review cleanups \(#2287\)

* epp: use stdlib cmp.Or in the multicluster metrics extractor

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: drop the duplicate name field from the multicluster wrappers

The embedded plugin already carries the name via TypedName\(\); read it back
there instead of holding a second copy.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: fail config load when a multicluster scorer lacks its extractor

The multicluster metric scorers read pool attributes the multicluster metrics
extractor produces, but their scheduling Consumes\(\) map\[string\]any was invisible
to the datalayer produce/consume graph. A config listing the scorers without the
extractor loaded clean and then silently returned no score.

Make the extractor a ProducerPlugin and the two scorers declare their pool key as
a Required DataDependency, so CreateMissingDataProducers fails the load. Add the
ErrNoDefaultProducer sentinel so callers can match with errors.Is.

Fixes #2255

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: validate cluster hostnames with apimachinery IsDNS1123Subdomain

Replace the hand-rolled RFC-1123 hostname scanner with the shared
k8s.io/apimachinery validator, so a cluster address is checked the same way as
the rest of the ecosystem. Drops the custom scanner and its dedicated test.

Behavior narrows to DNS-1123: uppercase and trailing-dot addresses are now
rejected, and a numeric-only label is accepted. Each is fail-closed \(rejected
at config load, never a misroute\), and all are fine for a cluster gateway
address.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")[#2287](https://github.com/llm-d/llm-d-router/pull/2287)[)](https://github.com/llm-d/llm-d-router/commit/9691af2f68251504ab4398e95d5e51c2abc59f23 "epp: multicluster config-load validation + review cleanups \(#2287\)

* epp: use stdlib cmp.Or in the multicluster metrics extractor

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: drop the duplicate name field from the multicluster wrappers

The embedded plugin already carries the name via TypedName\(\); read it back
there instead of holding a second copy.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: fail config load when a multicluster scorer lacks its extractor

The multicluster metric scorers read pool attributes the multicluster metrics
extractor produces, but their scheduling Consumes\(\) map\[string\]any was invisible
to the datalayer produce/consume graph. A config listing the scorers without the
extractor loaded clean and then silently returned no score.

Make the extractor a ProducerPlugin and the two scorers declare their pool key as
a Required DataDependency, so CreateMissingDataProducers fails the load. Add the
ErrNoDefaultProducer sentinel so callers can match with errors.Is.

Fixes #2255

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: validate cluster hostnames with apimachinery IsDNS1123Subdomain

Replace the hand-rolled RFC-1123 hostname scanner with the shared
k8s.io/apimachinery validator, so a cluster address is checked the same way as
the rest of the ecosystem. Drops the custom scanner and its dedicated test.

Behavior narrows to DNS-1123: uppercase and trailing-dot addresses are now
rejected, and a numeric-only label is accepted. Each is fail-closed \(rejected
at config load, never a misroute\), and all are fine for a cluster gateway
address.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")  | Aug 6, 2026  |  
|   |   |  [epp: multicluster config-load validation + review cleanups (](https://github.com/llm-d/llm-d-router/commit/9691af2f68251504ab4398e95d5e51c2abc59f23 "epp: multicluster config-load validation + review cleanups \(#2287\)

* epp: use stdlib cmp.Or in the multicluster metrics extractor

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: drop the duplicate name field from the multicluster wrappers

The embedded plugin already carries the name via TypedName\(\); read it back
there instead of holding a second copy.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: fail config load when a multicluster scorer lacks its extractor

The multicluster metric scorers read pool attributes the multicluster metrics
extractor produces, but their scheduling Consumes\(\) map\[string\]any was invisible
to the datalayer produce/consume graph. A config listing the scorers without the
extractor loaded clean and then silently returned no score.

Make the extractor a ProducerPlugin and the two scorers declare their pool key as
a Required DataDependency, so CreateMissingDataProducers fails the load. Add the
ErrNoDefaultProducer sentinel so callers can match with errors.Is.

Fixes #2255

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: validate cluster hostnames with apimachinery IsDNS1123Subdomain

Replace the hand-rolled RFC-1123 hostname scanner with the shared
k8s.io/apimachinery validator, so a cluster address is checked the same way as
the rest of the ecosystem. Drops the custom scanner and its dedicated test.

Behavior narrows to DNS-1123: uppercase and trailing-dot addresses are now
rejected, and a numeric-only label is accepted. Each is fail-closed \(rejected
at config load, never a misroute\), and all are fine for a cluster gateway
address.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")[#2287](https://github.com/llm-d/llm-d-router/pull/2287)[)](https://github.com/llm-d/llm-d-router/commit/9691af2f68251504ab4398e95d5e51c2abc59f23 "epp: multicluster config-load validation + review cleanups \(#2287\)

* epp: use stdlib cmp.Or in the multicluster metrics extractor

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: drop the duplicate name field from the multicluster wrappers

The embedded plugin already carries the name via TypedName\(\); read it back
there instead of holding a second copy.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: fail config load when a multicluster scorer lacks its extractor

The multicluster metric scorers read pool attributes the multicluster metrics
extractor produces, but their scheduling Consumes\(\) map\[string\]any was invisible
to the datalayer produce/consume graph. A config listing the scorers without the
extractor loaded clean and then silently returned no score.

Make the extractor a ProducerPlugin and the two scorers declare their pool key as
a Required DataDependency, so CreateMissingDataProducers fails the load. Add the
ErrNoDefaultProducer sentinel so callers can match with errors.Is.

Fixes #2255

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: validate cluster hostnames with apimachinery IsDNS1123Subdomain

Replace the hand-rolled RFC-1123 hostname scanner with the shared
k8s.io/apimachinery validator, so a cluster address is checked the same way as
the rest of the ecosystem. Drops the custom scanner and its dedicated test.

Behavior narrows to DNS-1123: uppercase and trailing-dot addresses are now
rejected, and a numeric-only label is accepted. Each is fail-closed \(rejected
at config load, never a misroute\), and all are fine for a cluster gateway
address.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")  | Aug 6, 2026  |  
|   |   |  [epp: add multicluster scheduling plugin variants (](https://github.com/llm-d/llm-d-router/commit/38e3412cdcb55ed78560af1ed921bb97f51f3f21 "epp: add multicluster scheduling plugin variants \(#2232\)

* epp: add multicluster datalayer plugins

Discovery, metrics source, and metrics extractor variants for a
cluster-scoped EPP whose endpoints are peer clusters.

- multicluster-file-discovery accepts an RFC-1123 hostname or an IP for a
  cluster gateway, via an injected address validator, so the stock IPv4-only
  pod discovery is unchanged.
- multicluster-metrics-data-source scrapes a peer cluster. Because it crosses
  a cluster trust boundary it verifies the peer certificate by default and
  caps the response size.
- multicluster-metrics-extractor writes only the pool-aggregate metrics into
  the llm-d.ai/multicluster-* attributes the scorers read.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: add multicluster scheduling plugin variants

Co-located variants beside their stock plugins, for a cluster-scoped EPP.

- multicluster-kv-cache-utilization-scorer and multicluster-queue-scorer score
  on the llm-d.ai/multicluster-* pool-aggregate attributes, since the per-pod
  metric fields are empty on a cluster-endpoint.
- multicluster-session-affinity-filter, multicluster-prefix-cache-scorer, and
  multicluster-approx-prefix-cache-producer delegate to their stock plugins.
  Cluster identity comes from discovery and the approx indexer already keys on
  endpoint identity, so it tracks \(cluster, block\) unchanged.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: register and document the multicluster plugin family

Register the multicluster datalayer and scheduling variants in the runner,
and document the family in the plugins README with a dependency note and a
wiring example.

The config-load test drives parseConfigurationPhaseTwo so it instantiates each
plugin against the registry and asserts it resolves to the expected type; phase
one alone would pass even with an unregistered or misspelled factory.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")[#2232](https://github.com/llm-d/llm-d-router/pull/2232)[)](https://github.com/llm-d/llm-d-router/commit/38e3412cdcb55ed78560af1ed921bb97f51f3f21 "epp: add multicluster scheduling plugin variants \(#2232\)

* epp: add multicluster datalayer plugins

Discovery, metrics source, and metrics extractor variants for a
cluster-scoped EPP whose endpoints are peer clusters.

- multicluster-file-discovery accepts an RFC-1123 hostname or an IP for a
  cluster gateway, via an injected address validator, so the stock IPv4-only
  pod discovery is unchanged.
- multicluster-metrics-data-source scrapes a peer cluster. Because it crosses
  a cluster trust boundary it verifies the peer certificate by default and
  caps the response size.
- multicluster-metrics-extractor writes only the pool-aggregate metrics into
  the llm-d.ai/multicluster-* attributes the scorers read.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: add multicluster scheduling plugin variants

Co-located variants beside their stock plugins, for a cluster-scoped EPP.

- multicluster-kv-cache-utilization-scorer and multicluster-queue-scorer score
  on the llm-d.ai/multicluster-* pool-aggregate attributes, since the per-pod
  metric fields are empty on a cluster-endpoint.
- multicluster-session-affinity-filter, multicluster-prefix-cache-scorer, and
  multicluster-approx-prefix-cache-producer delegate to their stock plugins.
  Cluster identity comes from discovery and the approx indexer already keys on
  endpoint identity, so it tracks \(cluster, block\) unchanged.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: register and document the multicluster plugin family

Register the multicluster datalayer and scheduling variants in the runner,
and document the family in the plugins README with a dependency note and a
wiring example.

The config-load test drives parseConfigurationPhaseTwo so it instantiates each
plugin against the registry and asserts it resolves to the expected type; phase
one alone would pass even with an unregistered or misspelled factory.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")  | Aug 4, 2026  |  
|   |   |  [epp: add multicluster scheduling plugin variants (](https://github.com/llm-d/llm-d-router/commit/38e3412cdcb55ed78560af1ed921bb97f51f3f21 "epp: add multicluster scheduling plugin variants \(#2232\)

* epp: add multicluster datalayer plugins

Discovery, metrics source, and metrics extractor variants for a
cluster-scoped EPP whose endpoints are peer clusters.

- multicluster-file-discovery accepts an RFC-1123 hostname or an IP for a
  cluster gateway, via an injected address validator, so the stock IPv4-only
  pod discovery is unchanged.
- multicluster-metrics-data-source scrapes a peer cluster. Because it crosses
  a cluster trust boundary it verifies the peer certificate by default and
  caps the response size.
- multicluster-metrics-extractor writes only the pool-aggregate metrics into
  the llm-d.ai/multicluster-* attributes the scorers read.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: add multicluster scheduling plugin variants

Co-located variants beside their stock plugins, for a cluster-scoped EPP.

- multicluster-kv-cache-utilization-scorer and multicluster-queue-scorer score
  on the llm-d.ai/multicluster-* pool-aggregate attributes, since the per-pod
  metric fields are empty on a cluster-endpoint.
- multicluster-session-affinity-filter, multicluster-prefix-cache-scorer, and
  multicluster-approx-prefix-cache-producer delegate to their stock plugins.
  Cluster identity comes from discovery and the approx indexer already keys on
  endpoint identity, so it tracks \(cluster, block\) unchanged.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: register and document the multicluster plugin family

Register the multicluster datalayer and scheduling variants in the runner,
and document the family in the plugins README with a dependency note and a
wiring example.

The config-load test drives parseConfigurationPhaseTwo so it instantiates each
plugin against the registry and asserts it resolves to the expected type; phase
one alone would pass even with an unregistered or misspelled factory.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")[#2232](https://github.com/llm-d/llm-d-router/pull/2232)[)](https://github.com/llm-d/llm-d-router/commit/38e3412cdcb55ed78560af1ed921bb97f51f3f21 "epp: add multicluster scheduling plugin variants \(#2232\)

* epp: add multicluster datalayer plugins

Discovery, metrics source, and metrics extractor variants for a
cluster-scoped EPP whose endpoints are peer clusters.

- multicluster-file-discovery accepts an RFC-1123 hostname or an IP for a
  cluster gateway, via an injected address validator, so the stock IPv4-only
  pod discovery is unchanged.
- multicluster-metrics-data-source scrapes a peer cluster. Because it crosses
  a cluster trust boundary it verifies the peer certificate by default and
  caps the response size.
- multicluster-metrics-extractor writes only the pool-aggregate metrics into
  the llm-d.ai/multicluster-* attributes the scorers read.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: add multicluster scheduling plugin variants

Co-located variants beside their stock plugins, for a cluster-scoped EPP.

- multicluster-kv-cache-utilization-scorer and multicluster-queue-scorer score
  on the llm-d.ai/multicluster-* pool-aggregate attributes, since the per-pod
  metric fields are empty on a cluster-endpoint.
- multicluster-session-affinity-filter, multicluster-prefix-cache-scorer, and
  multicluster-approx-prefix-cache-producer delegate to their stock plugins.
  Cluster identity comes from discovery and the approx indexer already keys on
  endpoint identity, so it tracks \(cluster, block\) unchanged.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* epp: register and document the multicluster plugin family

Register the multicluster datalayer and scheduling variants in the runner,
and document the family in the plugins README with a dependency note and a
wiring example.

The config-load test drives parseConfigurationPhaseTwo so it instantiates each
plugin against the registry and asserts it resolves to the expected type; phase
one alone would pass even with an unregistered or misspelled factory.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")  | Aug 4, 2026  |  
| View all files |  
## [README.md](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/datalayer/discovery/file#readme)
Outline
# File Discovery Plugin
**Type:** `file-discovery` **Interface:** `EndpointDiscovery`
Loads inference endpoints from a YAML or JSON file on the local filesystem, optionally re-loading the file when it changes.
## What It Does
Provides an alternative to Kubernetes-based endpoint discovery for deployments that run the EPP without a controller manager (bare metal, Slurm, Ray, local development). The plugin reads a static endpoints file at startup, applies each entry to the datastore via `DiscoveryNotifier`, and -- when configured to do so -- watches the file for changes via fsnotify and reconciles the datastore on each change.
## How It Works
  * **Initial load.** On `Start`, the file is read once. Each entry is validated (address must be a parseable IP, port must be in `[1, 65535]`) and applied via `notifier.Upsert`. Per-entry validation errors are logged and the entry is skipped; file-level problems (open, parse, size > 1 MiB) abort startup.
  * **Reload (optional).** When `watchFile: true`, fsnotify Write / Create / Remove events trigger a reload. After an atomic rename or ConfigMap-style symlink swap (which destroys the inode being watched), the watcher is re-attached so subsequent changes still fire. Reload semantics match the initial load: invalid entries are logged and skipped, valid entries are applied. Endpoints present in the previous load but absent from the new one are deleted via `notifier.Delete`.
  * **Readiness.** The plugin closes its `Ready()` channel after the first successful load so callers can gate request-serving components on the datastore being populated.


## Inputs Consumed
A YAML or JSON file with the schema below. The path is supplied via the plugin's `path` parameter.

```
endpoints:
  - name: <string>              # required -- unique within the file
    namespace: <string>         # optional -- defaults to "default"
    address: <IPv4>             # required -- must be a valid IPv4 address
    port: <string>              # required -- integer 1-65535 as a string
    labels:                     # optional -- arbitrary key/value labels
      <key>: <value>
```

## Configuration
**Location:** `dataLayer.discovery.pluginRef` referencing a plugin entry of type `file-discovery` in `plugins`. **Enabled by default:** No.
### Parameters  
| Name  | Type  | Required  | Default  | Description  |  
| --- | --- | --- | --- | --- |  
| `path`  | `string`  | yes  | --  | Absolute path to the endpoints file.  |  
| `watchFile`  | `bool`  | no  | `false`  | When true, watch the file for changes via fsnotify and reload on Write / Create / Remove events.  |  
### Examples

```
plugins:
  - type: file-discovery
    name: file-discovery
    parameters:
      path: /etc/epp/endpoints.yaml
      watchFile: true
dataLayer:
  discovery:
    endpoints:
      pluginRef: file-discovery
```

A two-endpoint file referenced by the config above:

```
endpoints:
  - name: vllm-0
    address: "10.0.0.1"
    port: "8000"
  - name: vllm-1
    address: "10.0.0.2"
    port: "8000"
```

## Limitations
  * The endpoints file is capped at 1 MiB.
  * `address` must be a literal IPv4 address. Hostnames are not resolved; IPv6 is not supported.
  * Metrics are scraped from `address:port` (same host and port that serves inference); separate metrics endpoints are not supported.
  * File-discovery mode runs the EPP without a Kubernetes controller manager, so several K8s-only features are inactive: the `InferenceModelRewrite` and `InferenceObjective` reconcilers do not run, and any `k8s-notification-source` plugin in the data layer config will not bind. The runner emits a startup log naming the inactive features.
  * A single bad entry on initial load is logged and skipped, not fatal. If the entire file is not readable or fails to parse, startup fails.


## Related Documentation


## Multi-cluster support
`multicluster-file-discovery` is the cluster-scoped variant: it discovers peer clusters as endpoints, differing from the pod file discovery only in address validation. A cluster is reached by its gateway host, so an address may be an RFC-1123 hostname or an IP, not only an IPv4 pod address.
A peer may serve its pool metrics on a different host or port than its routable address. Set a `metricsAddress` label (and an optional `metricsPort` label, defaulting to the endpoint port) on the entry, and the metrics source scrapes there instead of the endpoint address.
You can’t perform that action at this time. 
