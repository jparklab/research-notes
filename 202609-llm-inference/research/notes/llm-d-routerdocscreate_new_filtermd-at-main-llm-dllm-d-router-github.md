---
title: llm-d-router/docs/create_new_filter.md at main · llm-d/llm-d-router · GitHub
id: llm-d-routerdocscreate_new_filtermd-at-main-llm-dllm-d-router-github
tags:
- llmd-router-nonk8s-backends-54b774
- epp-plugin-registration
- router-extensibility
created: '2026-09-11T20:55:11.417833Z'
updated: '2026-09-15T04:55:23.897822Z'
source: https://github.com/llm-d/llm-d-router/blob/main/docs/create_new_filter.md
source_domain: github.com
fetched_at: '2026-09-11T20:55:11.396079Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Official llm-d-router plugin-authoring tutorial (docs/create_new_filter.md,
  last touched Aug 3 2026). Walks through writing a label-selector Filter plugin implementing
  plugin.Plugin (TypedName()) and scheduling.Filter (Filter(ctx, request, endpoints)),
  with a factory function of signature func(name string, rawParameters *json.Decoder,
  handle plugin.Handle) (plugin.Plugin, error). Critically, the ''Hooking the filter
  into the scheduling flow'' section prescribes editing the repo directly: step 1
  is ''Add an import and a plugin.Register call in runner.go'' inside registerInTreePlugins(),
  and step 2 is referencing the plugin by name/type in the EndpointPickerConfig YAML
  (plugins: list + schedulingProfiles pluginRef). The doc gives NO out-of-tree/library-import
  alternative -- the only documented path to add a plugin is to patch cmd/epp/runner/runner.go
  and rebuild, i.e. fork-and-PR or fork-and-build-your-own-binary, even though (per
  registry.go) the underlying Registry map is technically an exported global that
  a separate module could populate itself.'
utility_score: 14.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/docs/create_new_filter.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/create_new_filter.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/create_new_filter.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/create_new_filter.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/docs/create_new_filter.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# create_new_filter.md
Copy path
More file actions
More file actions
## Latest commit
[feat: implement EPP plugin stability lifecycle and --allow-experiment…](https://github.com/llm-d/llm-d-router/commit/8791c008bb7ae6fc7bdee0bba69e7de96eb4c13c)
Open commit detailssuccess
Aug 3, 2026
[8791c00](https://github.com/llm-d/llm-d-router/commit/8791c008bb7ae6fc7bdee0bba69e7de96eb4c13c) · Aug 3, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/docs/create_new_filter.md)
Open commit details
History
201 lines (153 loc) · 7.31 KB
Code owner: @llm-d/router-maintainers
/
# create_new_filter.md
Copy path
## File metadata and controls
  * Preview
  * 

201 lines (153 loc) · 7.31 KB
Code owner: @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Extending llm-d-router with a custom filter
This tutorial outlines the steps needed for creating and hooking a new filter for the llm-d-router.
The tutorial demonstrates the coding of a new filter, which selects inference serving endpoints based on their labels. All relevant code is contained in the [`bylabel`](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/scheduling/filter/bylabel) package (registered as the `label-selector-filter` plugin type).
## Introduction to filtering
Plugins are used to modify llm-d-router's default behavior. Filter plugins are provided with a list of candidate inference serving endpoints and filter out the endpoints which do not match the filtering criteria. Several filtering plugins can run in succession to produce the final candidate list which is then evaluated, through the process of _scoring_ , to select the most appropriate target endpoints.
The base [`plugin.Plugin`](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/plugin/plugins.go) interface requires a single method:

```
type Plugin interface {
    TypedName() TypedName
}
```

Filters implement the [`scheduling.Filter`](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/scheduling/plugins.go) interface:

```
type Filter interface {
    plugin.Plugin
    Filter(ctx context.Context, request *InferenceRequest, pods []Endpoint) []Endpoint
}
```

Key types used in the filter signature:
  * `scheduling.InferenceRequest` — parsed request with model, body, headers, and objectives
  * `scheduling.Endpoint` — candidate endpoint interface exposing metadata (including labels) and metrics


Plugins that need to share per-request data with downstream extension points use `plugin.PluginState` (keyed by request ID). Per-endpoint attributes that flow through the data layer's Produces/Consumes graph are written via `Endpoint.Put` / read via `Endpoint.Get`.
The `Filter` function accepts the request and a slice of candidate endpoints. Each endpoint exposes relevant inference attributes, such as model server metrics, which can be used to make scheduling decisions. The function returns a (possibly smaller) slice of endpoints which satisfy the filtering criteria.
## Code walkthrough
The following walkthrough references [`selector.go`](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/filter/bylabel/selector.go).
The top of the file has the expected Go package and import statements:

```
package bylabel

import (
    "context"
    "encoding/json"
    "errors"
    "fmt"

    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
    "k8s.io/apimachinery/pkg/labels"

    "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin"
    "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/scheduling"
)
```

Specifically, we import:
  * Kubernetes `meta/v1` and `labels` — for label selector types
  * framework's `plugin` — base plugin interfaces
  * framework's `scheduling` — filter interface and scheduling-related types


Next we define the `Selector` struct type, a plugin type constant, and a compile-time interface check:

```
const (
    LabelSelectorFilterType = "label-selector-filter"
)

var _ scheduling.Filter = Selector{}

// Selector filters out endpoints that do not match its label selector criteria.
type Selector struct {
    typedName plugin.TypedName
    selector  labels.Selector
}
```

> Note the compile-time interface check `var _ scheduling.Filter = &Selector{}`. This asserts at compile time that `Selector` implements the `scheduling.Filter` interface and is useful for catching errors early, especially when refactoring (e.g., interface methods or signatures change).
### Factory function
Plugins are instantiated via factory functions. The factory receives the instance name, a `*json.Decoder` over the plugin's raw configuration parameters (or `nil` when the plugin was instantiated without parameters), and a `plugin.Handle`:

```
func SelectorFactory(name string, rawParameters *json.Decoder, _ plugin.Handle) (plugin.Plugin, error) {
    parameters := metav1.LabelSelector{}
    if rawParameters != nil {
        if err := rawParameters.Decode(parameters); err != nil {
            return nil, fmt.Errorf("failed to parse the parameters of the '%s' filter - %w", LabelSelectorFilterType, err)
        }
    }
    return NewSelector(name, parameters)
}

func NewSelector(name string, selector *metav1.LabelSelector) (*Selector, error) {
    if name == "" {
        return nil, errors.New("Selector: missing filter name")
    }
    labelSelector, err := metav1.LabelSelectorAsSelector(selector)
    if err != nil {
        return nil, err
    }

    return Selector{
        typedName: plugin.TypedName{Type: LabelSelectorFilterType, Name: name},
        selector:  labelSelector,
    }, nil
}
```

### Interface methods
Next, we define the required interface methods:
  * `TypedName()` from `plugin.Plugin`
  * `Filter()` from `scheduling.Filter`



```
func (blf *Selector) TypedName() plugin.TypedName {
    return blf.typedName
}

func (blf *Selector) Filter(_ context.Context, _ *scheduling.InferenceRequest, endpoints []scheduling.Endpoint) []scheduling.Endpoint {
    filtered := []scheduling.Endpoint{}

    for _, endpoint := range endpoints {
        labels := labels.Set(endpoint.GetMetadata().Labels)
        if blf.selector.Matches(labels) {
            filtered = append(filtered, endpoint)
        }
    }
    return filtered
}
```

Since the filter is only matching on candidate endpoint labels, we leave the `context.Context` and `InferenceRequest` parameters unnamed. Filters that need access to LLM request information (e.g., filtering based on prompt length) may use them.
## Hooking the filter into the scheduling flow
Once a filter is defined, two steps are needed to make it available:
### 1. Register the factory
Add an import and a `plugin.Register` call in [`runner.go`](https://github.com/llm-d/llm-d-router/blob/main/cmd/epp/runner/runner.go). New plugins should generally be registered with `plugin.StabilityAlpha` unless intended to be enabled by default:

```
import (
    // ...existing imports...
    "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin"
    "github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/bylabel"
    // ...
)

func registerInTreePlugins() {
    // ...existing registrations...
    plugin.Register(bylabel.LabelSelectorFilterType, plugin.StabilityAlpha, bylabel.SelectorFactory)
}
```

### 2. Reference the plugin in the EndpointPickerConfig
The EPP is configured via an `EndpointPickerConfig`. First declare the plugin instance in the `plugins` section (with optional `parameters`), then reference it by name in a `schedulingProfiles` entry:

```
apiVersion: llm-d.ai/v1alpha1
kind: EndpointPickerConfig
plugins:
- type: label-selector-filter
  name: my-label-filter
  parameters:
    matchLabels:
      role: decode
schedulingProfiles:
- name: default
  plugins:
  - pluginRef: my-label-filter
```

> Note: a real filter would require unit tests, etc. These are left out to keep the tutorial short and focused.
## Next steps
If you have an idea for a new `Filter` (or other) plugin - we'd love to hear from you! Please open an [issue](https://github.com/llm-d/llm-d-router/issues/new/choose), describing your use case and requirements, and we'll reach out to refine and collaborate.
You can’t perform that action at this time. 
