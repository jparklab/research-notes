---
title: 'GitHub - traceloop/openllmetry: Open-source observability for your GenAI or
  LLM application, based on OpenTelemetry · GitHub'
id: github-traceloopopenllmetry-open-source-observability-for-your-genai-or-llm-appl
tags:
- observability
- llm-observability
- opentelemetry
- internal-llm-inference-stacks-7f0c90
- locus-observability-layer-oss-maturity
created: '2026-10-09T23:15:06.995973Z'
updated: '2026-10-10T14:52:17.242398Z'
source: https://github.com/traceloop/openllmetry
source_domain: github.com
fetched_at: '2026-10-09T23:15:06.973475Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'OpenLLMetry (Traceloop) GitHub README: a set of OpenTelemetry instrumentations
  for LLM providers (OpenAI/Azure OpenAI, Anthropic implied, Google Gemini, IBM Watsonx,
  SageMaker, Vertex AI) and vector DBs (Chroma, Pinecone, Qdrant, Weaviate), built
  and maintained by Traceloop under the Apache 2.0 license — fully permissive, no
  enterprise carve-out visible in this repo. Explicitly designed to emit into existing
  observability infrastructure the company already runs: supported exporters include
  Datadog, Honeycomb, Azure Application Insights, ServiceNow Cloud Observability,
  and a generic OpenTelemetry Collector — i.e., it occupies the vendor-neutral-instrumentation
  slot (emit OTel to a backend you operate), not the owns-storage-and-UI slot that
  Langfuse/Phoenix occupy. 7.5k stars, 21 watchers, 1.1k forks, 1,428 commits, latest
  commit Oct 6 2026. SDK telemetry (anonymous, opt-out via version bump) is only collected
  by Traceloop''s own SDK wrapper, not by the instrumentation libraries used standalone.'
---

[Skip to content](https://github.com/traceloop/openllmetry#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/traceloop/openllmetry) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/traceloop/openllmetry) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/traceloop/openllmetry) to refresh your session. Dismiss alert
[**603** Branches](https://github.com/traceloop/openllmetry/branches)[**528** Tags](https://github.com/traceloop/openllmetry/tags)
Go to file
Open more actions menu
## Latest commit
[fix(google-generativeai): correct version to match released package v…](https://github.com/traceloop/openllmetry/commit/df4921a40940cdce136c4409380a55e9d4c7a96d)
Open commit detailssuccess
Oct 6, 2026
[df4921a](https://github.com/traceloop/openllmetry/commit/df4921a40940cdce136c4409380a55e9d4c7a96d) · Oct 6, 2026
## History
[1,428 Commits](https://github.com/traceloop/openllmetry/commits/main/)
Open commit details
1,428 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [fix: use GITHUB_TOKEN for release creation to resolve 403 error (](https://github.com/traceloop/openllmetry/commit/857e30028e556edfee02c68e1166965edc29f780 "fix: use GITHUB_TOKEN for release creation to resolve 403 error \(#3821\)")[#3821](https://github.com/traceloop/openllmetry/pull/3821)[)](https://github.com/traceloop/openllmetry/commit/857e30028e556edfee02c68e1166965edc29f780 "fix: use GITHUB_TOKEN for release creation to resolve 403 error \(#3821\)")  | Mar 18, 2026  |  
|   | Nov 21, 2023  |  
|   |   | [fix(google-generativeai): correct version to match released package v…](https://github.com/traceloop/openllmetry/commit/df4921a40940cdce136c4409380a55e9d4c7a96d "fix\(google-generativeai\): correct version to match released package version \(#4573\)")  | Oct 6, 2026  |  
|   |   |  [chore(ci): uv + ruff migration (](https://github.com/traceloop/openllmetry/commit/86aa6058000c376eef5b5fdca82c4b68730caa76 "chore\(ci\): uv + ruff migration \(#3538\)")[#3538](https://github.com/traceloop/openllmetry/pull/3538)[)](https://github.com/traceloop/openllmetry/commit/86aa6058000c376eef5b5fdca82c4b68730caa76 "chore\(ci\): uv + ruff migration \(#3538\)")  | Jan 15, 2026  |  
|   |   | [bump: version 0.62.3 → 0.62.4](https://github.com/traceloop/openllmetry/commit/be498301c40c55155e6d0678b63943093cda14d1 "bump: version 0.62.3 → 0.62.4")  | Sep 29, 2026  |  
|   |   |  [fix(tracing): Add association property (](https://github.com/traceloop/openllmetry/commit/9290933ece73f04a39956d5f317b3be074451400 "fix\(tracing\): Add association property \(#3524\)")[#3524](https://github.com/traceloop/openllmetry/pull/3524)[)](https://github.com/traceloop/openllmetry/commit/9290933ece73f04a39956d5f317b3be074451400 "fix\(tracing\): Add association property \(#3524\)")  | Dec 27, 2025  |  
|   |   | [bump: version 0.62.3 → 0.62.4](https://github.com/traceloop/openllmetry/commit/be498301c40c55155e6d0678b63943093cda14d1 "bump: version 0.62.3 → 0.62.4")  | Sep 29, 2026  |  
|   |   |  [chore(ci): uv + ruff migration (](https://github.com/traceloop/openllmetry/commit/86aa6058000c376eef5b5fdca82c4b68730caa76 "chore\(ci\): uv + ruff migration \(#3538\)")[#3538](https://github.com/traceloop/openllmetry/pull/3538)[)](https://github.com/traceloop/openllmetry/commit/86aa6058000c376eef5b5fdca82c4b68730caa76 "chore\(ci\): uv + ruff migration \(#3538\)")  | Jan 15, 2026  |  
|   |   |   | Sep 4, 2023  |  
|   |   |  [fix(openai-agents): apply content tracing flag to content (](https://github.com/traceloop/openllmetry/commit/f941d0c9b6e55a9cb76e4ab2cb6d2e16df0a09df "fix\(openai-agents\): apply content tracing flag to content \(#3487\)")[#3487](https://github.com/traceloop/openllmetry/pull/3487)[)](https://github.com/traceloop/openllmetry/commit/f941d0c9b6e55a9cb76e4ab2cb6d2e16df0a09df "fix\(openai-agents\): apply content tracing flag to content \(#3487\)")  | Jan 29, 2026  |  
|   |   |   | Dec 5, 2023  |  
|   |   |   | Sep 2, 2023  |  
|   |   |  [chore: update maintainers (](https://github.com/traceloop/openllmetry/commit/117bac476a734d4765148f18f52158638165f12b "chore: update maintainers \(#3054\)")[#3054](https://github.com/traceloop/openllmetry/pull/3054)[)](https://github.com/traceloop/openllmetry/commit/117bac476a734d4765148f18f52158638165f12b "chore: update maintainers \(#3054\)")  | Jul 2, 2025  |  
|   |   |  [docs(readme): update supported frameworks (](https://github.com/traceloop/openllmetry/commit/88f61bc50ddb17e7aad5fef78dc8835310824f7c "docs\(readme\): update supported frameworks \(#3468\)")[#3468](https://github.com/traceloop/openllmetry/pull/3468)[)](https://github.com/traceloop/openllmetry/commit/88f61bc50ddb17e7aad5fef78dc8835310824f7c "docs\(readme\): update supported frameworks \(#3468\)")  | Nov 25, 2025  |  
|   |   |   | Sep 4, 2023  |  
|   |   |  [chore(ci): ci improvement + lint fixes (](https://github.com/traceloop/openllmetry/commit/e4cb1c0483fadb360370555f31f96646c90c52bc "chore\(ci\): ci improvement + lint fixes \(#73\)")[#73](https://github.com/traceloop/openllmetry/pull/73)[)](https://github.com/traceloop/openllmetry/commit/e4cb1c0483fadb360370555f31f96646c90c52bc "chore\(ci\): ci improvement + lint fixes \(#73\)")  | Sep 27, 2023  |  
|   |   |  [chore(deps-dev): bump @nxlv/python from 20.15.1 to 21.3.0 (](https://github.com/traceloop/openllmetry/commit/4ee45c65973c41a8adbdd8e18278b9cc8c724505 "chore\(deps-dev\): bump @nxlv/python from 20.15.1 to 21.3.0 \(#3541\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Gal Kleinman <33281963+galkleinman@users.noreply.github.com>")[#3541](https://github.com/traceloop/openllmetry/pull/3541)[)](https://github.com/traceloop/openllmetry/commit/4ee45c65973c41a8adbdd8e18278b9cc8c724505 "chore\(deps-dev\): bump @nxlv/python from 20.15.1 to 21.3.0 \(#3541\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Gal Kleinman <33281963+galkleinman@users.noreply.github.com>")  | Feb 23, 2026  |  
|   |   |  [chore(deps-dev): bump @nxlv/python from 20.15.1 to 21.3.0 (](https://github.com/traceloop/openllmetry/commit/4ee45c65973c41a8adbdd8e18278b9cc8c724505 "chore\(deps-dev\): bump @nxlv/python from 20.15.1 to 21.3.0 \(#3541\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Gal Kleinman <33281963+galkleinman@users.noreply.github.com>")[#3541](https://github.com/traceloop/openllmetry/pull/3541)[)](https://github.com/traceloop/openllmetry/commit/4ee45c65973c41a8adbdd8e18278b9cc8c724505 "chore\(deps-dev\): bump @nxlv/python from 20.15.1 to 21.3.0 \(#3541\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Gal Kleinman <33281963+galkleinman@users.noreply.github.com>")  | Feb 23, 2026  |  
| View all files |  
## Repository files navigation
Open-source observability for your LLM application
####  [**Get started »**](https://traceloop.com/docs/openllmetry/getting-started-python) [Slack](https://traceloop.com/slack) | [Docs](https://traceloop.com/docs/openllmetry/introduction) | [Website](https://www.traceloop.com/openllmetry)
**🎉 New** : Our semantic conventions are now part of OpenTelemetry! Join the [discussion](https://github.com/open-telemetry/community/blob/1c71595874e5d125ca92ec3b0e948c4325161c8a/projects/llm-semconv.md) and help us shape the future of LLM observability.
Looking for the JS/TS version? Check out [OpenLLMetry-JS](https://github.com/traceloop/openllmetry-js).
OpenLLMetry is a set of extensions built on top of [OpenTelemetry](https://opentelemetry.io/) that gives you complete observability over your LLM application. Because it uses OpenTelemetry under the hood, [it can be connected to your existing observability solutions](https://www.traceloop.com/docs/openllmetry/integrations/introduction) - Datadog, Honeycomb, and others.
It's built and maintained by Traceloop under the Apache 2.0 license.
The repo contains standard OpenTelemetry instrumentations for LLM providers and Vector DBs, as well as a Traceloop SDK that makes it easy to get started with OpenLLMetry, while still outputting standard OpenTelemetry data that can be connected to your observability stack. If you already have OpenTelemetry instrumented, you can just add any of our instrumentations directly.
## 🚀 Getting Started
The easiest way to get started is to use our SDK. For a complete guide, go to our [docs](https://traceloop.com/docs/openllmetry/getting-started-python).
Install the SDK:

```
pip install traceloop-sdk
```

Then, to start instrumenting your code, just add this line to your code:

```
from traceloop.sdk import Traceloop

Traceloop.init()
```

That's it. You're now tracing your code with OpenLLMetry! If you're running this locally, you may want to disable batch sending, so you can see the traces immediately:

```
Traceloop.init(disable_batch=True)
```

## ⏫ Supported (and tested) destinations
  * ✅ [Azure Application Insights](https://www.traceloop.com/docs/openllmetry/integrations/azure)
  * ✅ [OpenTelemetry Collector](https://www.traceloop.com/docs/openllmetry/integrations/otel-collector)
  * ✅ [Service Now Cloud Observability](https://www.traceloop.com/docs/openllmetry/integrations/service-now)


See [our docs](https://traceloop.com/docs/openllmetry/integrations/exporting) for instructions on connecting to each one.
## 🪗 What do we instrument?
OpenLLMetry can instrument everything that [OpenTelemetry already instruments](https://github.com/open-telemetry/opentelemetry-python-contrib/tree/main/instrumentation) - so things like your DB, API calls, and more. On top of that, we built a set of custom extensions that instrument things like your calls to OpenAI or Anthropic, or your Vector DB like Chroma, Pinecone, Qdrant or Weaviate.
  * ✅ [Google Generative AI (Gemini)](https://ai.google/)
  * ✅ [IBM Watsonx AI](https://www.ibm.com/watsonx)
  * ✅ [OpenAI / Azure OpenAI](https://openai.com/)
  * ✅ [SageMaker (AWS)](https://aws.amazon.com/sagemaker/)
  * ✅ [Vertex AI (GCP)](https://cloud.google.com/vertex-ai)


### Vector DBs


### Frameworks
  * ✅ [AWS Strands](https://strandsagents.com/) (built-in OTEL support)


### Protocol


## 🔎 Telemetry
We no longer log or collect any telemetry in the SDK or in the instrumentations. Make sure to bump to v0.49.2 and above.
### Why we collect telemetry
  * The primary purpose is to detect exceptions within instrumentations. Since LLM providers frequently update their APIs, this helps us quickly identify and fix any breaking changes.
  * We only collect anonymous data, with no personally identifiable information. You can view exactly what data we collect in our [Privacy documentation](https://www.traceloop.com/docs/openllmetry/privacy/telemetry).
  * Telemetry is only collected in the SDK. If you use the instrumentations directly without the SDK, no telemetry is collected.


## 🌱 Contributing
Whether big or small, we love contributions ❤️ Check out our guide to see how to [get started](https://traceloop.com/docs/openllmetry/contributing/overview).
Not sure where to get started? You can:
  * Book a free pairing session with one of our teammates!
  * Join our [Slack](https://traceloop.com/slack), and ask us any questions there.


## 💚 Community & Support
  * [Slack](https://traceloop.com/slack) (For live discussion with the community and the Traceloop team)
  * [GitHub Discussions](https://github.com/traceloop/openllmetry/discussions) (For help with building and deeper conversations about features)
  * [GitHub Issues](https://github.com/traceloop/openllmetry/issues) (For any bugs and errors you encounter using OpenLLMetry)
  * [Twitter](https://twitter.com/traceloopdev) (Get news fast)


## 🙏 Special Thanks
To @patrickdebois, who [suggested the great name](https://x.com/patrickdebois/status/1695518950715473991?s=46&t=zn2SOuJcSVq-Pe2Ysevzkg) we're now using for this repo!
## 💫 Contributors
## About
Open-source observability for your GenAI or LLM application, based on OpenTelemetry
[www.traceloop.com/openllmetry](https://www.traceloop.com/openllmetry)
### Topics
[artifical-intelligence](https://github.com/topics/artifical-intelligence)[datascience](https://github.com/topics/datascience)[generative-ai](https://github.com/topics/generative-ai)[good-first-issue](https://github.com/topics/good-first-issue)[good-first-issues](https://github.com/topics/good-first-issues)[help-wanted](https://github.com/topics/help-wanted)[llm](https://github.com/topics/llm)[llmops](https://github.com/topics/llmops)[metrics](https://github.com/topics/metrics)[ml](https://github.com/topics/ml)[model-monitoring](https://github.com/topics/model-monitoring)[monitoring](https://github.com/topics/monitoring)[observability](https://github.com/topics/observability)[open-source](https://github.com/topics/open-source)[open-telemetry](https://github.com/topics/open-telemetry)[opentelemetry](https://github.com/topics/opentelemetry)[opentelemetry-python](https://github.com/topics/opentelemetry-python)[python](https://github.com/topics/python)
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**7.5k** stars
### Watchers
**21** watching
### Forks
[**1.1k** forks](https://github.com/traceloop/openllmetry/forks)
## 
[ 0.62.4Latest Sep 29, 2026 ](https://github.com/traceloop/openllmetry/releases/tag/0.62.4)
## [Used by](https://github.com/traceloop/openllmetry/network/dependents)1.7K (1.7K)
[+ 1,721](https://github.com/traceloop/openllmetry/network/dependents)
## 
## Languages
  * [Python100%](https://github.com/traceloop/openllmetry/search?l=python)


You can’t perform that action at this time. 
