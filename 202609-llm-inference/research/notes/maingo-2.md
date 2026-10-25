---
title: main.go
id: maingo-2
tags:
- llmd-router-nonk8s-backends-54b774
- pd-sidecar
created: '2026-09-11T21:54:22.926348Z'
updated: '2026-09-15T04:55:23.989537Z'
source: https://raw.githubusercontent.com/llm-d/llm-d-router/main/cmd/pd-sidecar/main.go
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-11T21:54:22.903603Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Go source (cmd/pd-sidecar/main.go, llm-d-router@main), the sidecar''s entrypoint:
  parses flags via proxy.NewOptions()/AddFlags, calls opts.Complete()/Validate(),
  then proxy.NewProxy(opts.Config).Start(ctx). Confirms the binary is a plain Go process
  with no Kubernetes-specific bootstrap beyond the SSRF-protection allowlist watcher
  created inside Start(); the service is named ''llm-d-disagg-sidecar'' for tracing.
  No network policy, admission, or other cluster-level control is generated or required
  by this binary itself.'
---

```
/*
Copyright 2025 The llm-d Authors.

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
package main

import (
	"context"

	"github.com/spf13/pflag"
	ctrl "sigs.k8s.io/controller-runtime"
	"sigs.k8s.io/controller-runtime/pkg/log"

	"github.com/llm-d/llm-d-router/pkg/common/observability/tracing"
	"github.com/llm-d/llm-d-router/pkg/sidecar/proxy"
	"github.com/llm-d/llm-d-router/pkg/sidecar/version"
)

func main() {
	// Initialize options with defaults
	opts := proxy.NewOptions()

	// Add options flags (including logging flags)
	opts.AddFlags(pflag.CommandLine)
	pflag.Parse()

	logger := opts.NewLogger()
	log.SetLogger(logger)

	ctx := ctrl.SetupSignalHandler()
	log.IntoContext(ctx, logger)

	// Complete options (handles migration from deprecated flags, populates Config)
	if err := opts.Complete(); err != nil {
		logger.Error(err, "Failed to complete configuration")
		return
	}

	// Validate options
	if err := opts.Validate(); err != nil {
		logger.Error(err, "Invalid configuration")
		return
	}

	// Initialize tracing conditionally using config
	if opts.Tracing {
		shutdown, err := tracing.InitTracing(ctx, logger, "llm-d-disagg-sidecar")
		if err != nil {
			// Log error but don't fail - tracing is optional
			logger.Error(err, "Failed to initialize tracing")
		} else if shutdown != nil {
			defer func() {
				if err := shutdown(context.Background()); err != nil {
					logger.Error(err, "Failed to shutdown tracing")
				}
			}()
		}
	}

	logger.Info("Proxy starting", "Built on", version.BuildRef, "From Git SHA", version.CommitSHA)
	logger.Info("Proxy configuration", "config", opts.Config)

	proxyServer := proxy.NewProxy(opts.Config)
	if err := proxyServer.Start(ctx); err != nil {
		logger.Error(err, "failed to start proxy server")
	}
}

```

