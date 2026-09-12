---
title: registry.go
id: registrygo
tags:
- llmd-router-nonk8s-backends-54b774
- epp-plugin-registration
- router-extensibility
created: '2026-09-11T20:57:17.296593Z'
updated: '2026-09-12T03:37:37.743514Z'
source: https://raw.githubusercontent.com/llm-d/llm-d-router/main/pkg/epp/framework/interface/plugin/registry.go
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-11T20:57:17.275154Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'pkg/epp/framework/interface/plugin/registry.go defines the actual plugin
  registry as PACKAGE-LEVEL EXPORTED globals: var Registry = map[string]FactoryFunc{}
  and var RegistryMetadata = map[string]PluginMetadata{}, mutated by the exported
  functions Register(pluginType, stability, factory), RegisterAsDefaultProducer(...),
  RegisterWithPluginDependencies(...), and RegisterDeprecated(...). Because Registry
  is an exported package-level map (not a private field on Runner, and not gated by
  any internal-only build tag), any Go package that imports github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin
  can call plugin.Register(...) itself -- typically from an init() function in a separate
  plugin package -- and that entry becomes visible to loader.InstantiateAndConfigure
  regardless of whether it was registered by llm-d-router''s own registerInTreePlugins()
  or by external code, since Go guarantees all package init() functions run before
  main() executes. This is the concrete mechanism that makes out-of-tree plugin registration
  technically possible without patching the router''s Go source: a custom main.go
  can blank-import a third-party discovery-plugin package (registering via init())
  and then call runner.NewRunner().Run(ctx), and the custom plugin type name becomes
  usable in dataLayer.discovery.endpoints.pluginRef.'
utility_score: 15.0
---

*Suggested by [[runnergo]] — runner.go references fwkplugin.Register for the plugin registry*


```
/*
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
*/

package plugin

import (
	"bytes"
	"encoding/json"
	"fmt"
	"sort"
)

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
	}
	dec := json.NewDecoder(bytes.NewReader(raw))
	dec.DisallowUnknownFields()
	return dec
}

// Register registers a factory function for the given plugin type along with its stability level.
func Register(pluginType string, stability StabilityLevel, factory FactoryFunc) {
	Registry[pluginType] = factory
	RegistryMetadata[pluginType] = PluginMetadata{
		Type:      pluginType,
		Stability: stability,
	}
}

// RegisterAsDefaultProducer registers a factory for the given plugin type with an explicit
// stability level and records it as the default producer for the given data key.
func RegisterAsDefaultProducer(pluginType string, stability StabilityLevel, factory FactoryFunc, key DataKey) {
	Register(pluginType, stability, factory)
	DefaultProducerRegistry[key.String()] = pluginType
}

// RegisterWithPluginDependencies registers a factory for the given plugin type with an explicit
// stability level and records it as dependent on other plugins referenced in the configuration struct
// returned by the plugin's configuration parser function.
func RegisterWithPluginDependencies(pluginType string, stability StabilityLevel, factory FactoryFunc, parser ConfigParserFunc) {
	Register(pluginType, stability, factory)
	PluginsWithPluginDependencies[pluginType] = parser
}

// RegisterDeprecated registers a plugin factory function with stability and deprecation metadata.
func RegisterDeprecated(pluginType string, stability StabilityLevel, factory FactoryFunc, deprecatedIn, scheduledRemovalIn, replacementType string) {
	Register(pluginType, stability, factory)
	meta := RegistryMetadata[pluginType]
	meta.Deprecated = true
	meta.DeprecatedIn = deprecatedIn
	meta.ScheduledRemovalIn = scheduledRemovalIn
	meta.ReplacementType = replacementType
	RegistryMetadata[pluginType] = meta
}

// GetPluginStability looks up the stability level of a registered plugin type.
// Returns StabilityStable by default for unregistered plugins.
func GetPluginStability(pluginType string) StabilityLevel {
	if meta, ok := RegistryMetadata[pluginType]; ok {
		return meta.Stability
	}
	return StabilityStable
}

// ValidatePluginStability inspects all plugins registered in the handle and returns an error
// if any plugin has Alpha stability when allowExperimentalPlugins is false.
func ValidatePluginStability(handle Handle, allowExperimentalPlugins bool) error {
	plugins := handle.GetAllPlugins()
	sort.Slice(plugins, func(i, j int) bool {
		return plugins[i].TypedName().Name < plugins[j].TypedName().Name
	})
	for _, p := range plugins {
		stability := GetPluginStability(p.TypedName().Type)
		if stability == StabilityAlpha && !allowExperimentalPlugins {
			return fmt.Errorf("plugin '%s' (type: '%s') has Alpha stability level, but command line flag --allow-experimental-plugins is not set", p.TypedName().Name, p.TypedName().Type)
		}
	}
	return nil
}

// Registry is a mapping from plugin type to Factory function.
var Registry = map[string]FactoryFunc{}

// RegistryMetadata maps a plugin type to its PluginMetadata.
var RegistryMetadata = map[string]PluginMetadata{}

// DefaultProducerRegistry maps a data key to the default producer plugin name (same as type).
// Populated via RegisterAsDefaultProducer.
var DefaultProducerRegistry = map[string]string{}

// PluginsWithPluginDependencies maps plugin types to their configuration parser function, used to determine plugin dependencies
var PluginsWithPluginDependencies = map[string]ConfigParserFunc{}

```

