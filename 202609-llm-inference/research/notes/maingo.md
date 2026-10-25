---
title: main.go
id: maingo
tags:
- llmd-router-nonk8s-backends-54b774
- epp-entrypoint
- plugin-registration
created: '2026-09-11T20:51:56.909021Z'
updated: '2026-09-15T04:55:23.877302Z'
source: https://raw.githubusercontent.com/llm-d/llm-d-router/main/cmd/epp/main.go
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-11T20:51:56.886718Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'llm-d-router cmd/epp/main.go at main (fetched 2026-09-11; adapted from GIE
  cmd/epp/main.go): the EPP entrypoint is only ctrl.SetupSignalHandler() + runner.NewRunner().Run(ctx),
  with a comment that built-in plugins are auto-registered by runner.parsePluginsConfiguration().
  Shows the EPP binary is a thin wrapper over the importable package github.com/llm-d/llm-d-router/cmd/epp/runner,
  so a downstream main() importing the runner is the natural way to add custom plugins;
  still imports controller-runtime even in no-k8s mode.'
utility_score: 15.0
---

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

/**
 * This file is adapted from Gateway API Inference Extension
 * Original source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/cmd/epp/main.go
 * Licensed under the Apache License, Version 2.0
 */

// Package main contains the "Endpoint Picker (EPP)" program for scheduling
// inference requests.
package main

import (
	"os"

	ctrl "sigs.k8s.io/controller-runtime"

	"github.com/llm-d/llm-d-router/cmd/epp/runner"
)

func main() {
	os.Exit(run())
}

func run() int {
	ctx := ctrl.SetupSignalHandler()

	// Note: GIE built-in plugins are automatically registered by the runner
	// when it processes configuration in runner.parsePluginsConfiguration()

	if err := runner.NewRunner().
		Run(ctx); err != nil {
		return 1
	}
	return 0
}

```

