---
title: 'config: consolidate discovery plugins under a single discovery sectio… · llm-d/llm-d-router@950c963
  · GitHub'
id: config-consolidate-discovery-plugins-under-a-single-discovery-sectio-llm-dllm-d
tags:
- llmd-router-nonk8s-backends-54b774
- config-schema
- epp-plugin-registration
created: '2026-09-11T21:00:57.409737Z'
updated: '2026-09-12T03:37:37.748225Z'
source: https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19
source_domain: github.com
fetched_at: '2026-09-11T21:00:57.388619Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PR #2434 / commit 950c963644 (merged 2026-08-25T18:27:45Z, author Stefano
  Pascolutti), ''config: consolidate discovery plugins under a single discovery section
  (follow up #2341)''. This is the exact commit that changed the EndpointPickerConfig
  discovery key shape. BEFORE: dataLayer.discovery.pluginRef (DiscoveryConfig had
  PluginRef directly) and a separate dataLayer.peerDiscovery.pluginRef field. AFTER:
  dataLayer.discovery.endpoints.pluginRef (new nested EndpointDiscoveryConfig under
  DiscoveryConfig.Endpoints) and dataLayer.discovery.peers.pluginRef (PeerDiscoveryConfig
  moved under DiscoveryConfig.Peers). So neither of the two candidate shapes in the
  research question is exactly right post-consolidation: it is dataLayer.discovery.endpoints.pluginRef,
  not top-level discovery.pluginRef nor bare dataLayer.discovery.pluginRef. A migrateDiscoveryConfig()
  shim in pkg/epp/config/loader/configloader.go preserves backward compatibility:
  if the deprecated dataLayer.discovery.pluginRef is set and endpoints.pluginRef is
  not, the loader copies it into Endpoints.PluginRef and logs a DEPRECATION warning
  (''If both are set, the new field is used.''). The pre-release dataLayer.peerDiscovery
  field (introduced in #2341, never shipped) was deleted outright with no migration
  shim since no users depended on it. cmd/epp/runner/runner.go''s resolveDiscovery()
  and the Run() dispatch check (dlCfg.Discovery.Endpoints != nil) were updated to
  the new nested path. docs/discovery.md and docs/peer-discovery.md were updated in
  the same commit -- any llm-d.ai external docs not updated in lockstep would show
  the old dataLayer.discovery.pluginRef / dataLayer.peerDiscovery.pluginRef shapes,
  which still work via the shim but are deprecated.'
utility_score: 14.0
---

*Suggested by [[runnergo]] — identified via GitHub commit search as the Aug 25 2026 discovery-consolidation commit referenced by the orchestrator*

[Skip to content](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## File tree
Expand file treeCollapse file tree
Open diff view settings
Filter options
  * apix/config/v1alpha1
    *     *   * cmd/epp/runner
    *     * [run_with_file_discovery_test.go](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-43b71d31ea00096100f038a78de75fc070fc80b20ee574ecf3c6015297422217)
    *   *     *     *   * pkg/epp
    * config/loader
      *       *       *     * framework
      * interface/datalayer
        *       * plugins
        *         * datalayer/discovery/file
          * 

Expand file treeCollapse file tree
Open diff view settings
Collapse file
### [`‎apix/config/v1alpha1/endpointpickerconfig_types.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-b0ac55cea5177bf6d95d92857360cd09545672ee55c54014376c3f72050f6257)
Copy file name to clipboardExpand all lines: apix/config/v1alpha1/endpointpickerconfig_types.go
+36-15Lines changed: 36 additions & 15 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -234,17 +234,11 @@ type DataLayerConfig struct {` |  
| `234`  | `234`  | ` // Sources is the list of sources to define to the DataLayer `  |  
| `235`  | `235`  | ` Sources []DataLayerSource `json:"sources,omitempty"` `  |  
| `236`  | `236`  | ` // +optional `  |  
| `237`  | `- // Discovery specifies which EndpointDiscovery plugin to use for populating the `  |  
| `238`  | `- // endpoint datastore. When set, the EPP bypasses Kubernetes CRD reconcilers and `  |  
| `239`  | `- // relies entirely on the referenced plugin to enumerate and track inference `  |  
| `240`  | `- // endpoints. This enables running the EPP without a Kubernetes cluster. `  |  
| `241`  | `- // If omitted, the EPP uses the default Kubernetes-based discovery. `  |  
| `237`  | `+ // Discovery groups configuration for all discovery plugins (endpoint discovery `  |  
| `238`  | `+ // and peer discovery). If omitted, the EPP uses Kubernetes-based discovery for `  |  
| `239`  | `+ // endpoints and disables peer discovery. `  |  
| `242`  | `240`  | ` Discovery *DiscoveryConfig `json:"discovery,omitempty"` `  |  
| `243`  | `241`  | ` // +optional `  |  
| `244`  | `- // PeerDiscovery specifies which PeerDiscovery plugin to use for discovering `  |  
| `245`  | `- // peer EPP replicas. If omitted, peer discovery is disabled. `  |  
| `246`  | `- PeerDiscovery *PeerDiscoveryConfig `json:"peerDiscovery,omitempty"` `  |  
| `247`  | `- // +optional `  |  
| `248`  | `242`  | ` // CrossReplicaSyncerPluginRef names the plugin instance to use as the cross-EPP `  |  
| `249`  | `243`  | ` // cross-replica syncer. The reference is to the name of an entry in the `  |  
| `250`  | `244`  | ` // top-level Plugins section. If omitted, no cross-replica syncer is used `  |  
| `@@ -261,24 +255,51 @@ func (dlc *DataLayerConfig) String() string {` |  
| `261`  | `255`  | ` if dlc == nil {`  |  
| `262`  | `256`  | ` return nilString `  |  
| `263`  | `257`  |  
| `264`  | `- return fmt.Sprintf("{Sources: %v, Discovery: %v, PeerDiscovery: %v, CrossReplicaSyncerPluginRef: %s, CrossReplicaSyncInterval: %v}",`  |  
| `265`  | `- dlc.Sources, dlc.Discovery, dlc.PeerDiscovery, dlc.CrossReplicaSyncerPluginRef, dlc.CrossReplicaSyncInterval)`  |  
| `258`  | `+ return fmt.Sprintf("{Sources: %v, Discovery: %v, CrossReplicaSyncerPluginRef: %s, CrossReplicaSyncInterval: %v}",`  |  
| `259`  | `+ dlc.Sources, dlc.Discovery, dlc.CrossReplicaSyncerPluginRef, dlc.CrossReplicaSyncInterval)`  |  
| `266`  | `260`  |  
| `267`  | `261`  |  
| `268`  | `-// DiscoveryConfig references the EndpointDiscovery plugin to use.`  |  
| `262`  | `+// DiscoveryConfig groups endpoint and peer discovery plugin references.`  |  
| `269`  | `263`  | ` type DiscoveryConfig struct {`  |  
| `264`  | `+ // +optional `  |  
| `265`  | `+ // Endpoints specifies which EndpointDiscovery plugin to use for populating the `  |  
| `266`  | `+ // endpoint datastore. When set, the EPP bypasses Kubernetes CRD reconcilers and `  |  
| `267`  | `+ // relies entirely on the referenced plugin to enumerate and track inference `  |  
| `268`  | `+ // endpoints. This enables running the EPP without a Kubernetes cluster. `  |  
| `269`  | `+ Endpoints *EndpointDiscoveryConfig `json:"endpoints,omitempty"` `  |  
| `270`  | `+ // +optional `  |  
| `271`  | `+ // Peers specifies which PeerDiscovery plugin to use for discovering `  |  
| `272`  | `+ // peer EPP replicas. If omitted, peer discovery is disabled. `  |  
| `273`  | `+ Peers *PeerDiscoveryConfig `json:"peers,omitempty"` `  |  
| `274`  | `+ // +optional `  |  
| `275`  | `+ // PluginRef is the name of the plugin instance (from the Plugins list) that `  |  
| `276`  | `+ // implements EndpointDiscovery. `  |  
| `277`  |  
| `278`  | `+ // Deprecated: use endpoints.pluginRef instead. `  |  
| `279`  | `+ PluginRef string `json:"pluginRef,omitempty"` `  |  
| `280`  |  
| `282`  | `+ func (dc *DiscoveryConfig) String() string {`  |  
| `283`  | `+ if dc == nil {`  |  
| `284`  | `+ return nilString `  |  
| `285`  |  
| `286`  | `+ return fmt.Sprintf("{Endpoints: %v, Peers: %v, PluginRef: %s}", dc.Endpoints, dc.Peers, dc.PluginRef)`  |  
| `287`  |  
| `289`  | `+// EndpointDiscoveryConfig references the EndpointDiscovery plugin to use.`  |  
| `290`  | `+ type EndpointDiscoveryConfig struct {`  |  
| `270`  | `291`  | ` // +required `  |  
| `271`  | `292`  | ` // +kubebuilder:validation:Required `  |  
| `272`  | `293`  | ` // PluginRef is the name of the plugin instance (from the Plugins list) that `  |  
| `273`  | `294`  | ` // implements EndpointDiscovery. `  |  
| `274`  | `295`  | ` PluginRef string `json:"pluginRef"` `  |  
| `275`  | `296`  |  
| `276`  | `297`  |  
| `277`  | `- func (dc*DiscoveryConfig) String() string {`  |  
| `278`  | `- if dc == nil {`  |  
| `298`  | `+ func (edc*EndpointDiscoveryConfig) String() string {`  |  
| `299`  | `+ if edc == nil {`  |  
| `279`  | `300`  | ` return nilString `  |  
| `280`  | `301`  |  
| `281`  | `- return fmt.Sprintf("{PluginRef: %s}", dc.PluginRef)`  |  
| `302`  | `+ return fmt.Sprintf("{PluginRef: %s}", edc.PluginRef)`  |  
| `282`  | `303`  |  
| `283`  | `304`  |  
| `284`  | `305`  | `// PeerDiscoveryConfig references the PeerDiscovery plugin to use.`  |  
Collapse file
### [`‎apix/config/v1alpha1/zz_generated.deepcopy.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-1ff4948b111b98e3eb80ff39f73285dd5c03eb646f4f92ac8373b4637c30bf86)
Copy file name to clipboardExpand all lines: apix/config/v1alpha1/zz_generated.deepcopy.go
+25-5Lines changed: 25 additions & 5 deletions
Load diffSome generated files are not rendered by default. Learn more about [customizing how changed files appear on GitHub.](https://docs.github.com/github/administering-a-repository/customizing-how-changed-files-appear-on-github)
Collapse file
### [`‎cmd/epp/runner/resolve_discovery_test.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-a4adf7930a9d86da91796c7855e74d51d9329fa0ee3f1b2c86839cc35c969e24)
Copy file name to clipboardExpand all lines: cmd/epp/runner/resolve_discovery_test.go
+3-1Lines changed: 3 additions & 1 deletion  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -41,7 +41,9 @@ func newHandleWithPlugin(t *testing.T, name string, p fwkplugin.Plugin) fwkplugi` |  
| ` func discoveryConfigRef(ref string) *configapi.EndpointPickerConfig {`  |  
| ` return configapi.EndpointPickerConfig{`  |  
| ` DataLayer: configapi.DataLayerConfig{`  |  
| `- Discovery: configapi.DiscoveryConfig{PluginRef: ref},`  |  
| `+ Discovery: configapi.DiscoveryConfig{`  |  
| `+ Endpoints: configapi.EndpointDiscoveryConfig{PluginRef: ref},`  |  
 |  
 |  
 |  
 |  
Collapse file
### [`‎cmd/epp/runner/run_with_file_discovery_test.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-43b71d31ea00096100f038a78de75fc070fc80b20ee574ecf3c6015297422217)
Copy file name to clipboardExpand all lines: cmd/epp/runner/run_with_file_discovery_test.go
+1Lines changed: 1 addition & 0 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -108,6 +108,7 @@ dataLayer:` |  
| `108`  | `108`  | ` require.NoError(t, err)`  |  
| `109`  | `109`  | ` require.NotNil(t, rawConfig.DataLayer)`  |  
| `110`  | `110`  | ` require.NotNil(t, rawConfig.DataLayer.Discovery)`  |  
| `111`  | `+ require.NotNil(t, rawConfig.DataLayer.Discovery.Endpoints)`  |  
| `111`  | `112`  |  
| `112`  | `113`  | ` runErr := make(chan error, 1)`  |  
| `113`  | `114`  | ` go func() { runErr <- r.runWithFileDiscovery(ctx, opts, rawConfig) }()`  |  
Collapse file
### [`‎cmd/epp/runner/runner.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-64d12375c06d1950ba073a94fd54042d5374b840489908ccb297be4e3cc897e0)
Copy file name to clipboardExpand all lines: cmd/epp/runner/runner.go
+5-4Lines changed: 5 additions & 4 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -259,7 +259,8 @@ func (r *Runner) Run(ctx context.Context) error {` |  
| `259`  | `259`  | ` setupLog.Error(err, "Failed to parse configuration")`  |  
| `260`  | `260`  | ` return err `  |  
| `261`  | `261`  |  
| `262`  | `- if rawConfig.DataLayer != nil  rawConfig.DataLayer.Discovery != nil {`  |  
| `262`  | `+ dlCfg := rawConfig.DataLayer `  |  
| `263`  | `+ if dlCfg != nil  dlCfg.Discovery != nil  dlCfg.Discovery.Endpoints != nil {`  |  
| `263`  | `264`  | ` return r.runWithFileDiscovery(ctx, opts, rawConfig)`  |  
| `264`  | `265`  |  
| `265`  | `266`  |  
| `@@ -930,13 +931,13 @@ func resolvePoolNamespace(poolNamespace string) string {` |  
| `930`  | `931`  |  
| `931`  | `932`  |  
| `932`  | `933`  | `// resolveDiscovery returns the discovery plugin identified by`  |  
| `933`  | `-// rawConfig.DataLayer.Discovery.PluginRef. The plugin is expected to have`  |  
| `934`  | `-// already been instantiated and registered in r.PluginHandle by`  |  
| `934`  | `+// rawConfig.DataLayer.Discovery.Endpoints.PluginRef. The plugin is expected to`  |  
| `935`  | `+// have already been instantiated and registered in r.PluginHandle by`  |  
| `935`  | `936`  | `// parseConfigurationPhaseTwo; this function only looks it up and verifies its`  |  
| `936`  | `937`  | `// type, so the loader-created instance (with its real Handle wired in) is the`  |  
| `937`  | `938`  | `// one the runner drives.`  |  
| `938`  | `939`  | ` func (r *Runner) resolveDiscovery(rawConfig *configapi.EndpointPickerConfig) (fwkdl.EndpointDiscovery, error) {`  |  
| `939`  | `- ref := rawConfig.DataLayer.Discovery.PluginRef `  |  
| `940`  | `+ ref := rawConfig.DataLayer.Discovery.Endpoints.PluginRef `  |  
| `940`  | `941`  | ` p := r.PluginHandle.Plugin(ref)`  |  
| `941`  | `942`  | ` if p == nil {`  |  
| `942`  | `943`  | ` return nil, fmt.Errorf("discovery: no plugin found with name %q", ref)`  |  
Collapse file
### [`‎docs/discovery.md‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-a3de2740066d7bda9fb2744690146dda67e2ad1c3a308944b16a3e539bec8820)
Copy file name to clipboardExpand all lines: docs/discovery.md
+8-5Lines changed: 8 additions & 5 deletions
  * Display the source diff
  * Display the rich diff

  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -161,11 +161,12 @@ plugins:` |  
| `161`  | `161`  |  
| `162`  | `162`  | ` dataLayer:`  |  
| `163`  | `163`  | ` discovery:`  |  
| `164`  | `- pluginRef: my-endpoints     # must match a name in plugins above `  |  
| `164`  | `+ endpoints:`  |  
| `165`  | `+ pluginRef: my-endpoints   # must match a name in plugins above `  |  
| `165`  | `166`  |  
| `166`  | `167`  |  
| `167`  | `- When `dataLayer.discovery` is present the EPP skips the Kubernetes setup `  |  
| `168`  | `-entirely.  When it is absent the EPP uses the default Kubernetes-based`  |  
| `168`  | `+ When `dataLayer.discovery.endpoints` is present the EPP skips the Kubernetes `  |  
| `169`  | `+setup entirely.  When it is absent the EPP uses the default Kubernetes-based`  |  
| `169`  | `170`  | `discovery (backwards compatible).`  |  
| `170`  | `171`  |  
| `171`  | `172`  |  
| `@@ -384,7 +385,8 @@ schedulingProfiles:` |  
| `384`  | `385`  |  
| `385`  | `386`  | ` dataLayer:`  |  
| `386`  | `387`  | ` discovery:`  |  
| `387`  | `- pluginRef: file-disc `  |  
| `388`  | `+ endpoints:`  |  
| `389`  | `+ pluginRef: file-disc `  |  
| `388`  | `390`  |  
| `389`  | `391`  |  
| `390`  | `392`  | `### 3. Start the EPP`  |  
| `@@ -584,5 +586,6 @@ plugins:` |  
| `584`  | `586`  |  
| `585`  | `587`  | ` dataLayer:`  |  
| `586`  | `588`  | ` discovery:`  |  
| `587`  | `- pluginRef: my-disc `  |  
| `589`  | `+ endpoints:`  |  
| `590`  | `+ pluginRef: my-disc `  |  
| `588`  | `591`  |  
Collapse file
### [`‎docs/peer-discovery.md‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-7add8ee48389e52ad4cf3aee6a9912b0390742c427e50067c985cd7198a1eba1)
Copy file name to clipboardExpand all lines: docs/peer-discovery.md
+9-7Lines changed: 9 additions & 7 deletions
  * Display the source diff
  * Display the rich diff

  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -23,7 +23,7 @@ set of live EPP replicas and feeds it to consumers through a `PeerNotifier`.` |  
| `Peer discovery mirrors the [endpoint discovery](discovery.md) plugin model:`  |  
| `- - It is configured via `dataLayer.peerDiscovery.pluginRef` in the`  |  
| `+ - It is configured via `dataLayer.discovery.peers.pluginRef` in the`  |  
| ` `EndpointPickerConfig`.`  |  
| ` - Implementations are registered in the plugin registry and selected by name.`  |  
| ` - When omitted, peer discovery is disabled and no peer set is maintained.`  |  
| `@@ -107,7 +107,7 @@ returning a deterministically ordered snapshot of the current peer set.` |  
| `107`  | `107`  |  
| `108`  | `108`  | `## Selecting a peer discovery plugin in the EPP config`  |  
| `109`  | `109`  |  
| `110`  | `-Add a `peerDiscovery` section inside `dataLayer` in the`  |  
| `110`  | `+Add a `peers` entry inside `dataLayer.discovery` in the`  |  
| `111`  | `111`  | ` `EndpointPickerConfig`. The `pluginRef` field names a plugin instance defined`  |  
| `112`  | `112`  | `in the top-level `plugins` list.`  |  
| `113`  | `113`  |  
| `@@ -119,11 +119,12 @@ plugins:` |  
| `119`  | `119`  | ` # plugin-specific parameters `  |  
| `120`  | `120`  |  
| `121`  | `121`  | ` dataLayer:`  |  
| `122`  | `- peerDiscovery:`  |  
| `123`  | `- pluginRef: my-peer-disc    # must match a name in plugins above `  |  
| `122`  | `+ discovery:`  |  
| `123`  | `+ peers:`  |  
| `124`  | `+ pluginRef: my-peer-disc  # must match a name in plugins above `  |  
| `124`  | `125`  |  
| `125`  | `126`  |  
| `126`  | `- When `dataLayer.peerDiscovery` is absent, peer discovery is disabled and no `  |  
| `127`  | `+ When `dataLayer.discovery.peers` is absent, peer discovery is disabled and no `  |  
| `127`  | `128`  | `peer set is maintained (backwards compatible).`  |  
| `128`  | `129`  |  
| `129`  | `130`  |  
| `@@ -208,6 +209,7 @@ plugins:` |  
| `208`  | `209`  | `    parameters: {}`  |  
| `209`  | `210`  |  
| `210`  | `211`  | `dataLayer:`  |  
| `211`  | `-  peerDiscovery:`  |  
| `212`  | `-    pluginRef: my-peer-disc`  |  
| `212`  | `+  discovery:`  |  
| `213`  | `+    peers:`  |  
| `214`  | `+      pluginRef: my-peer-disc`  |  
| `213`  | `215`  |  
Collapse file
### [`‎pkg/epp/config/loader/configloader.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-186930e69c48cf7f3fa08f1c3e0fc3841be3eae4afd76c0d77bb13c17a4a313e)
Copy file name to clipboardExpand all lines: pkg/epp/config/loader/configloader.go
+21Lines changed: 21 additions & 0 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -121,6 +121,8 @@ func LoadRawConfig(configBytes []byte, logger logr.Logger, extraGates ...string)` |  
| `121`  | `121`  |  
| `122`  | `122`  |  
| `123`  | `123`  |  
| `124`  | `+ migrateDiscoveryConfig(logger, rawConfig)`  |  
| `124`  | `126`  | ` logger.Info("Loaded raw configuration", "config", rawConfig.String())`  |  
| `125`  | `127`  | `	} else {`  |  
| `126`  | `128`  | ` logger.Info("A configuration wasn't specified. A default one is being used.")`  |  
| `@@ -153,6 +155,25 @@ func LoadRawConfig(configBytes []byte, logger logr.Logger, extraGates ...string)` |  
| `153`  | `155`  | ` return rawConfig, featureConfig, nil `  |  
| `154`  | `156`  |  
| `155`  | `157`  |  
| `158`  | `+// migrateDiscoveryConfig lifts the deprecated bare pluginRef into the`  |  
| `159`  | `+// consolidated discovery section:`  |  
| `160`  |  
| `161`  | `+//	dataLayer.discovery.pluginRef -> dataLayer.discovery.endpoints.pluginRef`  |  
| `162`  | `+ func migrateDiscoveryConfig(logger logr.Logger, rawConfig *configapi.EndpointPickerConfig) {`  |  
| `163`  | `+ if rawConfig.DataLayer == nil {`  |  
| `164`  | `+ return `  |  
| `165`  |  
| `166`  | `+ dl := rawConfig.DataLayer `  |  
| `168`  | `+ //nolint:staticcheck // SA1019: dl.Discovery.PluginRef is deprecated: use discovery.endpoints instead. `  |  
| `169`  | `+ if dl.Discovery != nil  dl.Discovery.PluginRef != "" {`  |  
| `170`  | `+ logger.Info("DEPRECATION: dataLayer.discovery.pluginRef is deprecated, use dataLayer.discovery.endpoints.pluginRef instead. If both are set, the new field is used.")`  |  
| `171`  | `+ if dl.Discovery.Endpoints == nil {`  |  
| `172`  | `+ dl.Discovery.Endpoints = configapi.EndpointDiscoveryConfig{PluginRef: dl.Discovery.PluginRef}`  |  
| `173`  |  
| `174`  |  
| `175`  |  
| `156`  | `177`  | `// InstantiateAndConfigure performs the heavy lifting of plugin instantiation, system architecture injection, and`  |  
| `157`  | `178`  | `// scheduler construction.`  |  
| `158`  | `179`  | ` func InstantiateAndConfigure(`  |  
Collapse file
### [`‎pkg/epp/config/loader/configloader_test.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-ef696f788cc48e6916ee4ce60ec303b5cde41ff67259eef8382fa645338bdc84)
Copy file name to clipboardExpand all lines: pkg/epp/config/loader/configloader_test.go
+33Lines changed: 33 additions & 0 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -263,6 +263,39 @@ func TestLoadRawConfiguration(t *testing.T) {` |  
| `263`  | `263`  | ` wantErr:    false,`  |  
| `264`  | `264`  | ` deprecated: false,`  |  
| `265`  | `265`  |  
| `266`  |  
| `267`  | `+ name:       "Success - Deprecated discovery.pluginRef",`  |  
| `268`  | `+ configText: successDeprecatedDiscoveryPluginRefText,`  |  
| `269`  | `+ want: configapi.EndpointPickerConfig{`  |  
| `270`  | `+ TypeMeta: metav1.TypeMeta{`  |  
| `271`  | `+ Kind:       "EndpointPickerConfig",`  |  
| `272`  | `+ APIVersion: configapi.GroupVersion.String(),`  |  
| `273`  |  
| `274`  | `+ Plugins: []configapi.PluginSpec{`  |  
| `275`  | `+					{Name: "maxScore", Type: "max-score-picker"},`  |  
| `276`  | `+					{Name: "my-disc", Type: "file-discovery"},`  |  
| `277`  |  
| `278`  | `+ SchedulingProfiles: []configapi.SchedulingProfile{`  |  
| `279`  |  
| `280`  | `+ Name: "default",`  |  
| `281`  | `+ Plugins: []configapi.SchedulingPlugin{`  |  
| `282`  | `+							{PluginRef: "maxScore"},`  |  
| `283`  |  
| `284`  |  
| `285`  |  
| `286`  | `+ FeatureGates: configapi.FeatureGates{},`  |  
| `287`  | `+ DataLayer: configapi.DataLayerConfig{`  |  
| `288`  | `+ Discovery: configapi.DiscoveryConfig{`  |  
| `289`  | `+ PluginRef: "my-disc",`  |  
| `290`  | `+ Endpoints: configapi.EndpointDiscoveryConfig{`  |  
| `291`  | `+ PluginRef: "my-disc",`  |  
| `292`  |  
| `293`  |  
| `294`  |  
| `295`  |  
| `296`  | `+ wantErr:    false,`  |  
| `297`  | `+ deprecated: true,`  |  
| `298`  |  
| `266`  | `299`  |  
| `267`  | `300`  | ` name:       "Success - Deprecated Top-level SaturationDetector",`  |  
| `268`  | `301`  | ` configText: successDeprecatedTopLevelSaturationDetectorText,`  |  
Collapse file
### [`‎pkg/epp/config/loader/testdata_test.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-ef74e71408bf3981475a2cdc83197d78cee74ccc3dc2027af73ad8f0b1dc278c)
Copy file name to clipboardExpand all lines: pkg/epp/config/loader/testdata_test.go
+19Lines changed: 19 additions & 0 deletions  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -933,6 +933,25 @@ schedulingProfiles:` |  
| `933`  | `933`  | `  - pluginRef: maxScorePicker`  |  
| `934`  | `934`  |  
| `935`  | `935`  |  
| `936`  | `+// successDeprecatedDiscoveryPluginRefText tests that the deprecated bare`  |  
| `937`  | `+// discovery.pluginRef is migrated to discovery.endpoints.pluginRef.`  |  
| `938`  | `+ const successDeprecatedDiscoveryPluginRefText = ` `  |  
| `939`  | `+apiVersion: llm-d.ai/v1alpha1`  |  
| `940`  | `+kind: EndpointPickerConfig`  |  
| `941`  | `+plugins:`  |  
| `942`  | `+- name: maxScore`  |  
| `943`  | `+  type: max-score-picker`  |  
| `944`  | `+- name: my-disc`  |  
| `945`  | `+  type: file-discovery`  |  
| `946`  | `+schedulingProfiles:`  |  
| `947`  | `+- name: default`  |  
| `948`  | `+  plugins:`  |  
| `949`  | `+  - pluginRef: maxScore`  |  
| `950`  | `+dataLayer:`  |  
| `951`  | `+  discovery:`  |  
| `952`  | `+    pluginRef: my-disc`  |  
| `953`  |  
| `936`  | `955`  | `// successDeprecatedTopLevelSaturationDetectorText tests that top-level saturationDetector is correctly loaded,`  |  
| `937`  | `956`  | `// copied to nested location, and handled.`  |  
| `938`  | `957`  | ` const successDeprecatedTopLevelSaturationDetectorText = ` `  |  
Collapse file
### [`‎pkg/epp/framework/interface/datalayer/peer.go‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-cf0e6bc1a9f576b4201d4d12be6136207246c68921627fa3f25881515ee19c93)
Copy file name to clipboardExpand all lines: pkg/epp/framework/interface/datalayer/peer.go
+1-1Lines changed: 1 addition & 1 deletion  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -66,7 +66,7 @@ func (n *peerNotifier) Delete(id types.NamespacedName) {` |  
| `// PeerDiscovery discovers peer EPP replicas and drives their lifecycle through`  |  
| `// a PeerNotifier. Implementations are registered in the plugin registry and`  |  
| `-// selected via EndpointPickerConfig.dataLayer.peerDiscovery.pluginRef.`  |  
| `+// selected via EndpointPickerConfig.dataLayer.discovery.peers.pluginRef.`  |  
| ` type PeerDiscovery interface {`  |  
| `	fwkplugin.Plugin `  |  
Collapse file
### [`‎pkg/epp/framework/plugins/README.md‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-09307921f5ccd415b275b20bbbb95e0b08f0eef5dcf4cf98038ca14a35253af3)
Copy file name to clipboardExpand all lines: pkg/epp/framework/plugins/README.md
+2-1Lines changed: 2 additions & 1 deletion
  * Display the source diff
  * Display the rich diff

  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -62,7 +62,8 @@ plugins:` |  
| `  - type: single-profile-handler `  |  
| ` dataLayer:`  |  
| ` discovery:`  |  
| `- pluginRef: discovery `  |  
| `+ endpoints:`  |  
| `+ pluginRef: discovery `  |  
| ` sources:`  |  
| `    - pluginRef: metrics-source `  |  
| ` extractors:`  |  
Collapse file
### [`‎pkg/epp/framework/plugins/datalayer/discovery/file/README.md‎`](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19#diff-3390778fc2e9d2d14f4e27ca883fa69ff7b66f291b85cf8142b065febbb89aa2)
Copy file name to clipboardExpand all lines: pkg/epp/framework/plugins/datalayer/discovery/file/README.md
+2-1Lines changed: 2 additions & 1 deletion
  * Display the source diff
  * Display the rich diff

  
| Original file line number  | Diff line number  | Diff line change  |  
| --- | --- | --- |  
| `@@ -72,7 +72,8 @@ plugins:` |  
| `      watchFile: true`  |  
| `dataLayer:`  |  
| `  discovery:`  |  
| `-    pluginRef: file-discovery`  |  
| `+    endpoints:`  |  
| `+      pluginRef: file-discovery`  |  
 |  
| ` A two-endpoint file referenced by the config above:`  |  
## 0 commit comments
Comments
0 (0)
Please [sign in](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19) to comment.
You can’t perform that action at this time. 
