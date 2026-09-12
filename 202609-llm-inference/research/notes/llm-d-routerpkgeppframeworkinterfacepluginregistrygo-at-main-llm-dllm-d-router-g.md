---
title: llm-d-router/pkg/epp/framework/interface/plugin/registry.go at main · llm-d/llm-d-router
  · GitHub
id: llm-d-routerpkgeppframeworkinterfacepluginregistrygo-at-main-llm-dllm-d-router-g
tags:
- llmd-router-nonk8s-backends-54b774
- locus-extension-mechanism
- plugin-registration
- no-internal-api-disclaimer
created: '2026-09-11T21:24:32.427532Z'
updated: '2026-09-12T03:37:37.770696Z'
source: https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/plugin/registry.go
source_domain: github.com
fetched_at: '2026-09-11T21:24:32.398595Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Source of pkg/epp/framework/interface/plugin/registry.go (Apache-2.0, last
  touched PR #2509, Aug 25 2026). Defines the plugin registration mechanism referenced
  by docs/create_new_filter.md: package-level exported vars Registry (map[string]FactoryFunc),
  RegistryMetadata, DefaultProducerRegistry, PluginsWithPluginDependencies, and exported
  functions Register(pluginType, stability, factory), RegisterAsDefaultProducer, RegisterWithPluginDependencies,
  RegisterDeprecated, GetPluginStability, and ValidatePluginStability (which errors
  if an Alpha-stability plugin is configured without --allow-experimental-plugins).
  Critically: this file lives under pkg/ (not internal/), carries no package-level
  doc comment, and contains zero ''not a public API'' / ''internal use only'' disclaimer
  of any kind -- Register() and the Registry map are ordinary exported Go identifiers
  that any separate Go module could import and call directly (e.g., plugin.Register(...)
  from an init() in a downstream binary) without needing to fork the repo or edit
  runner.go, though nothing in the file or its comments states this is an intended/supported
  usage pattern -- the mechanism''s openness appears to be an artifact of normal Go
  visibility rules rather than a documented public API contract.'
---

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/plugin/registry.go#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/plugin/registry.go) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/plugin/registry.go) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/plugin/registry.go) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/interface/plugin/registry.go).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# registry.go
Copy path
More file actions
More file actions
## Latest commit
[epp: remove code deprecated since v0.7.0 (](https://github.com/llm-d/llm-d-router/commit/61350aa3f6ff7d05e69b4e50fa52b1a25417a043)[#2509](https://github.com/llm-d/llm-d-router/pull/2509)[)](https://github.com/llm-d/llm-d-router/commit/61350aa3f6ff7d05e69b4e50fa52b1a25417a043)
Open commit detailssuccess
Aug 25, 2026
[61350aa](https://github.com/llm-d/llm-d-router/commit/61350aa3f6ff7d05e69b4e50fa52b1a25417a043) · Aug 25, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/interface/plugin/registry.go)
Open commit details
History
121 lines (103 loc) · 5.15 KB
Code owner: @llm-d/router-maintainers
/
# registry.go
Copy path
## File metadata and controls
  * 

121 lines (103 loc) · 5.15 KB
Code owner: @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Open symbols panel
Edit and raw actions
/* Copyright 2025 The Kubernetes Authors. Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0 Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License. */ package plugin import ( "bytes" "encoding/json" "fmt" "sort" ) // FactoryFunc is the definition of the factory functions that are used to instantiate plugins // specified in a configuration. The framework provides a strict decoder // (DisallowUnknownFields) over the plugin's raw parameters, or nil when the plugin was // instantiated without parameters (e.g., as a default producer). Factories that ignore // parameters can take the decoder as `_ *json.Decoder`. type FactoryFunc func(name string, parameters *json.Decoder, handle Handle) (Plugin, error) // ConfigParserFunc is the definition of the factory functions that are used to parse the // parameters of plugins that have been registered as having dependencies on other plugins type ConfigParserFunc func(parameters *json.Decoder, handle Handle) (any, error) // StrictDecoder returns a *json.Decoder configured with DisallowUnknownFields over the // given raw plugin parameters, or nil when raw is empty. The framework uses this when // invoking factories so each plugin gets uniform strict parsing; tests use it to // construct factory arguments without duplicating the decoder boilerplate. func StrictDecoder(raw json.RawMessage) *json.Decoder { if len(raw) == 0 { return nil } dec := json.NewDecoder(bytes.NewReader(raw)) dec.DisallowUnknownFields() return dec } // Register registers a factory function for the given plugin type along with its stability level. func Register(pluginType string, stability StabilityLevel, factory FactoryFunc) { Registry[pluginType] = factory RegistryMetadata[pluginType] = PluginMetadata{ Type: pluginType, Stability: stability, } } // RegisterAsDefaultProducer registers a factory for the given plugin type with an explicit // stability level and records it as the default producer for the given data key. func RegisterAsDefaultProducer(pluginType string, stability StabilityLevel, factory FactoryFunc, key DataKey) { Register(pluginType, stability, factory) DefaultProducerRegistry[key.String()] = pluginType } // RegisterWithPluginDependencies registers a factory for the given plugin type with an explicit // stability level and records it as dependent on other plugins referenced in the configuration struct // returned by the plugin's configuration parser function. func RegisterWithPluginDependencies(pluginType string, stability StabilityLevel, factory FactoryFunc, parser ConfigParserFunc) { Register(pluginType, stability, factory) PluginsWithPluginDependencies[pluginType] = parser } // RegisterDeprecated registers a plugin factory function with stability and deprecation metadata. func RegisterDeprecated(pluginType string, stability StabilityLevel, factory FactoryFunc, deprecatedIn, scheduledRemovalIn, replacementType string) { Register(pluginType, stability, factory) meta := RegistryMetadata[pluginType] meta.Deprecated = true meta.DeprecatedIn = deprecatedIn meta.ScheduledRemovalIn = scheduledRemovalIn meta.ReplacementType = replacementType RegistryMetadata[pluginType] = meta } // GetPluginStability looks up the stability level of a registered plugin type. // Returns StabilityStable by default for unregistered plugins. func GetPluginStability(pluginType string) StabilityLevel { if meta, ok := RegistryMetadata[pluginType]; ok { return meta.Stability } return StabilityStable } // ValidatePluginStability inspects all plugins registered in the handle and returns an error // if any plugin has Alpha stability when allowExperimentalPlugins is false. func ValidatePluginStability(handle Handle, allowExperimentalPlugins bool) error { plugins := handle.GetAllPlugins() sort.Slice(plugins, func(i, j int) bool { return plugins[i].TypedName().Name < plugins[j].TypedName().Name }) for _, p := range plugins { stability := GetPluginStability(p.TypedName().Type) if stability == StabilityAlpha && !allowExperimentalPlugins { return fmt.Errorf("plugin '%s' (type: '%s') has Alpha stability level, but command line flag --allow-experimental-plugins is not set", p.TypedName().Name, p.TypedName().Type) } } return nil } // Registry is a mapping from plugin type to Factory function. var Registry = map[string]FactoryFunc{} // RegistryMetadata maps a plugin type to its PluginMetadata. var RegistryMetadata = map[string]PluginMetadata{} // DefaultProducerRegistry maps a data key to the default producer plugin name (same as type). // Populated via RegisterAsDefaultProducer. var DefaultProducerRegistry = map[string]string{} // PluginsWithPluginDependencies maps plugin types to their configuration parser function, used to determine plugin dependencies var PluginsWithPluginDependencies = map[string]ConfigParserFunc{}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
Copyright 2025 The Kubernetes Authors.
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
package plugin
import (
"bytes"
"encoding/json"
"fmt"
"sort"
// FactoryFunc is the definition of the factory functions that are used to instantiate plugins
// specified in a configuration. The framework provides a strict decoder
// (DisallowUnknownFields) over the plugin's raw parameters, or nil when the plugin was
// instantiated without parameters (e.g., as a default producer). Factories that ignore
// parameters can take the decoder as `_ *json.Decoder`.
type FactoryFunc func(name string, parameters *json.Decoder, handle Handle) (Plugin, error)
// ConfigParserFunc is the definition of the factory functions that are used to parse the
// parameters of plugins that have been registered as having dependencies on other plugins
type ConfigParserFunc func(parameters *json.Decoder, handle Handle) (any, error)
// StrictDecoder returns a *json.Decoder configured with DisallowUnknownFields over the
// given raw plugin parameters, or nil when raw is empty. The framework uses this when
// invoking factories so each plugin gets uniform strict parsing; tests use it to
// construct factory arguments without duplicating the decoder boilerplate.
func StrictDecoder(raw json.RawMessage) *json.Decoder {
if len(raw) == 0 {
return nil
dec := json.NewDecoder(bytes.NewReader(raw))
dec.DisallowUnknownFields()
return dec
// Register registers a factory function for the given plugin type along with its stability level.
func Register(pluginType string, stability StabilityLevel, factory FactoryFunc) {
Registry[pluginType] = factory
RegistryMetadata[pluginType] = PluginMetadata{
Type: pluginType,
Stability: stability,
// RegisterAsDefaultProducer registers a factory for the given plugin type with an explicit
// stability level and records it as the default producer for the given data key.
func RegisterAsDefaultProducer(pluginType string, stability StabilityLevel, factory FactoryFunc, key DataKey) {
Register(pluginType, stability, factory)
DefaultProducerRegistry[key.String()] = pluginType
// RegisterWithPluginDependencies registers a factory for the given plugin type with an explicit
// stability level and records it as dependent on other plugins referenced in the configuration struct
// returned by the plugin's configuration parser function.
func RegisterWithPluginDependencies(pluginType string, stability StabilityLevel, factory FactoryFunc, parser ConfigParserFunc) {
Register(pluginType, stability, factory)
PluginsWithPluginDependencies[pluginType] = parser
// RegisterDeprecated registers a plugin factory function with stability and deprecation metadata.
func RegisterDeprecated(pluginType string, stability StabilityLevel, factory FactoryFunc, deprecatedIn, scheduledRemovalIn, replacementType string) {
Register(pluginType, stability, factory)
meta := RegistryMetadata[pluginType]
meta.Deprecated = true
meta.DeprecatedIn = deprecatedIn
meta.ScheduledRemovalIn = scheduledRemovalIn
meta.ReplacementType = replacementType
RegistryMetadata[pluginType] = meta
// GetPluginStability looks up the stability level of a registered plugin type.
// Returns StabilityStable by default for unregistered plugins.
func GetPluginStability(pluginType string) StabilityLevel {
if meta, ok := RegistryMetadata[pluginType]; ok {
return meta.Stability
return StabilityStable
// ValidatePluginStability inspects all plugins registered in the handle and returns an error
// if any plugin has Alpha stability when allowExperimentalPlugins is false.
func ValidatePluginStability(handle Handle, allowExperimentalPlugins bool) error {
plugins := handle.GetAllPlugins()
sort.Slice(plugins, func(i, j int) bool {
return plugins[i].TypedName().Name plugins[j].TypedName().Name
for _, p := range plugins {
stability := GetPluginStability(p.TypedName().Type)
if stability == StabilityAlpha !allowExperimentalPlugins {
return fmt.Errorf("plugin '%s' (type: '%s') has Alpha stability level, but command line flag --allow-experimental-plugins is not set", p.TypedName().Name, p.TypedName().Type)
return nil
// Registry is a mapping from plugin type to Factory function.
var Registry = map[string]FactoryFunc{}
// RegistryMetadata maps a plugin type to its PluginMetadata.
var RegistryMetadata = map[string]PluginMetadata{}
// DefaultProducerRegistry maps a data key to the default producer plugin name (same as type).
// Populated via RegisterAsDefaultProducer.
var DefaultProducerRegistry = map[string]string{}
// PluginsWithPluginDependencies maps plugin types to their configuration parser function, used to determine plugin dependencies
var PluginsWithPluginDependencies = map[string]ConfigParserFunc{}
You can’t perform that action at this time. 
While the code is focused, press Alt+F1 for a menu of operations.
