---
title: README.md
id: readmemd
tags:
- llmd-router-nonk8s-backends-54b774
- file-discovery
- rank-index
- gap-5
- audited-absence
- non-k8s-discovery
created: '2026-09-11T21:55:10.798866Z'
updated: '2026-09-15T04:55:23.990906Z'
source: https://raw.githubusercontent.com/llm-d/llm-d-router/main/pkg/epp/framework/plugins/datalayer/discovery/file/README.md
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-11T21:55:10.775916Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'README for llm-d-router''s built-in ''file-discovery'' EndpointDiscovery
  plugin (type ''file-discovery'', interface EndpointDiscovery, disabled by default).
  Loads a YAML/JSON endpoints file (cap 1 MiB) at startup and optionally hot-reloads
  via fsnotify (watchFile: true), reconciling adds/deletes through DiscoveryNotifier.
  Entry schema is exactly {name (required, unique), namespace (optional, default ''default''),
  address (required, must be literal IPv4 -- hostnames/IPv6 unsupported), port (required
  string 1-65535), labels (optional free-form map)} -- CRITICAL FOR GAP-5: there is
  no rankIndex/dp-rank/''llm-d.ai/rank'' field anywhere in this schema, and cross-referencing
  plugin.go confirms the word ''rank'' does not appear in the implementation at all.
  This means every endpoint discovered by the built-in file-discovery plugin gets
  EndpointMetadata.RankIndex left at its Go zero-value (0) -- multi-DP-rank backends
  (vLLM data-parallel deployments needing multiple KV-event ZMQ ports per pod) are
  NOT supported out of the box by this plugin; a custom discovery plugin author would
  have to add their own rank field to the entry schema and set RankIndex themselves,
  since neither a label convention nor the shipped schema carries it. Explicitly documents
  what breaks running EPP without a Kubernetes controller manager: ''the InferenceModelRewrite
  and InferenceObjective reconcilers do not run, and any k8s-notification-source plugin
  in the data layer config will not bind. The runner emits a startup log naming the
  inactive features.'' Metrics are scraped from address:port only (no separate metrics
  endpoint support) in single-cluster file-discovery. A distinct ''multicluster-file-discovery''
  variant exists for cluster-scoped discovery, allowing RFC-1123 hostnames (not just
  IPv4) since it discovers whole peer clusters via gateway host, and supports per-entry
  metricsAddress/metricsPort labels for out-of-band metrics scraping.'
---

*Suggested by [[file]] — file-discovery plugin README, check config schema for rank/port fields*


```
# File Discovery Plugin

**Type:** `file-discovery`
**Interface:** `EndpointDiscovery`

Loads inference endpoints from a YAML or JSON file on the local filesystem,
optionally re-loading the file when it changes.

## What It Does

Provides an alternative to Kubernetes-based endpoint discovery for deployments
that run the EPP without a controller manager (bare metal, Slurm, Ray, local
development). The plugin reads a static endpoints file at startup, applies
each entry to the datastore via `DiscoveryNotifier`, and -- when configured to
do so -- watches the file for changes via fsnotify and reconciles the
datastore on each change.

## How It Works

- **Initial load.** On `Start`, the file is read once. Each entry is
  validated (address must be a parseable IP, port must be in `[1, 65535]`)
  and applied via `notifier.Upsert`. Per-entry validation errors are logged
  and the entry is skipped; file-level problems (open, parse, size > 1 MiB)
  abort startup.
- **Reload (optional).** When `watchFile: true`, fsnotify Write / Create /
  Remove events trigger a reload. After an atomic rename or ConfigMap-style
  symlink swap (which destroys the inode being watched), the watcher is
  re-attached so subsequent changes still fire. Reload semantics match the
  initial load: invalid entries are logged and skipped, valid entries are
  applied. Endpoints present in the previous load but absent from the new
  one are deleted via `notifier.Delete`.
- **Readiness.** The plugin closes its `Ready()` channel after the first
  successful load so callers can gate request-serving components on the
  datastore being populated.

## Inputs Consumed

A YAML or JSON file with the schema below. The path is supplied via the
plugin's `path` parameter.

```yaml
endpoints:
  - name: <string>              # required -- unique within the file
    namespace: <string>         # optional -- defaults to "default"
    address: <IPv4>             # required -- must be a valid IPv4 address
    port: <string>              # required -- integer 1-65535 as a string
    labels:                     # optional -- arbitrary key/value labels
      <key>: <value>
```

## Configuration

**Location:** `dataLayer.discovery.pluginRef` referencing a plugin entry of
type `file-discovery` in `plugins`.
**Enabled by default:** No.

### Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `path` | `string` | yes | -- | Absolute path to the endpoints file. |
| `watchFile` | `bool` | no | `false` | When true, watch the file for changes via fsnotify and reload on Write / Create / Remove events. |

### Examples

```yaml
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

```yaml
endpoints:
  - name: vllm-0
    address: "10.0.0.1"
    port: "8000"
  - name: vllm-1
    address: "10.0.0.2"
    port: "8000"
```

## Limitations

- The endpoints file is capped at 1 MiB.
- `address` must be a literal IPv4 address. Hostnames are not resolved;
  IPv6 is not supported.
- Metrics are scraped from `address:port` (same host and port that serves
  inference); separate metrics endpoints are not supported.
- File-discovery mode runs the EPP without a Kubernetes controller manager,
  so several K8s-only features are inactive: the `InferenceModelRewrite`
  and `InferenceObjective` reconcilers do not run, and any
  `k8s-notification-source` plugin in the data layer config will not bind.
  The runner emits a startup log naming the inactive features.
- A single bad entry on initial load is logged and skipped, not fatal. If
  the entire file is not readable or fails to parse, startup fails.

## Related Documentation

- [Plugins Index](../../../README.md)

## Multi-cluster support

`multicluster-file-discovery` is the cluster-scoped variant: it discovers peer clusters as endpoints, differing from the pod file discovery only in address validation. A cluster is reached by its gateway host, so an address may be an RFC-1123 hostname or an IP, not only an IPv4 pod address.

A peer may serve its pool metrics on a different host or port than its routable address. Set a `metricsAddress` label (and an optional `metricsPort` label, defaulting to the endpoint port) on the entry, and the metrics source scrapes there instead of the endpoint address.

```

