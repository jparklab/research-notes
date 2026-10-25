---
title: 'GitHub - Arize-ai/openinference: OpenTelemetry Instrumentation for AI Observability
  · GitHub'
id: github-arize-aiopeninference-opentelemetry-instrumentation-for-ai-observability
tags:
- llm-observability
- opentelemetry
created: '2026-10-09T23:25:56.095961Z'
updated: '2026-10-10T14:52:17.255881Z'
source: https://github.com/Arize-ai/openinference
source_domain: github.com
fetched_at: '2026-10-09T23:25:56.073738Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'OpenInference (Arize-ai/openinference) GitHub README: a set of OpenTelemetry-complementary
  conventions and instrumentation plugins for tracing AI/agent applications (LLM invocation,
  vector-store retrieval, tool/search usage), specified in transport/format-agnostic
  markdown under spec/. Natively supported by Arize Phoenix and Arize AX, but explicitly
  ''can be used with any OpenTelemetry-compatible backend as well'' — confirming it
  occupies the vendor-neutral-instrumentation slot (like OpenLLMetry) and is decoupled
  from the Phoenix storage/UI platform; a company could adopt OpenInference instrumentation
  while sending traces to Phoenix, Arize AX, or any other OTel backend. License confirmed
  Apache 2.0 via direct fetch of the repo''s LICENSE file (independent of Phoenix''s
  own ELv2 license — the instrumentation layer and the storage/UI platform carry different
  licenses). 1.3k stars, 14 watchers. Provides per-language instrumentation packages
  (Python shown: openinference-semantic-conventions, openinference-instrumentation,
  plus per-framework packages for Agno, AG2, and others).'
---

*Suggested by [[github-arize-aiphoenix-ai-observability-evaluation-github]] — Phoenix README names this as its auto-instrumentation project; need to confirm whether it is vendor-neutral OTel instrumentation separable from the Phoenix storage/UI platform*

[Skip to content](https://github.com/Arize-ai/openinference#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/Arize-ai/openinference) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/Arize-ai/openinference) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/Arize-ai/openinference) to refresh your session. Dismiss alert
[**237** Branches](https://github.com/Arize-ai/openinference/branches)[**1531** Tags](https://github.com/Arize-ai/openinference/tags)
Go to file
Open more actions menu
## Latest commit
and
Open commit detailssuccess
Oct 9, 2026
[b49afb3](https://github.com/Arize-ai/openinference/commit/b49afb343aa8cd252eb290852f9b27a850365263) · Oct 9, 2026
## History
[2,373 Commits](https://github.com/Arize-ai/openinference/commits/main/)
Open commit details
2,373 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| [.agents/skills](https://github.com/Arize-ai/openinference/tree/main/.agents/skills "This path skips through empty directories")  | [.agents/skills](https://github.com/Arize-ai/openinference/tree/main/.agents/skills "This path skips through empty directories")  |  [feat(skills): add issue-triage skill and scheduled triage workflow (](https://github.com/Arize-ai/openinference/commit/3d396cac845ac4e2aaac0d69885f8454eb9eaa93 "feat\(skills\): add issue-triage skill and scheduled triage workflow \(#3778\)")[#…](https://github.com/Arize-ai/openinference/pull/3778)  | Sep 17, 2026  |  
| [.claude/skills](https://github.com/Arize-ai/openinference/tree/main/.claude/skills "This path skips through empty directories")  | [.claude/skills](https://github.com/Arize-ai/openinference/tree/main/.claude/skills "This path skips through empty directories")  |  [feat(skills): add issue-triage skill and scheduled triage workflow (](https://github.com/Arize-ai/openinference/commit/3d396cac845ac4e2aaac0d69885f8454eb9eaa93 "feat\(skills\): add issue-triage skill and scheduled triage workflow \(#3778\)")[#…](https://github.com/Arize-ai/openinference/pull/3778)  | Sep 17, 2026  |  
| [.codex/skills](https://github.com/Arize-ai/openinference/tree/main/.codex/skills "This path skips through empty directories")  | [.codex/skills](https://github.com/Arize-ai/openinference/tree/main/.codex/skills "This path skips through empty directories")  |  [feat(skills): add issue-triage skill and scheduled triage workflow (](https://github.com/Arize-ai/openinference/commit/3d396cac845ac4e2aaac0d69885f8454eb9eaa93 "feat\(skills\): add issue-triage skill and scheduled triage workflow \(#3778\)")[#…](https://github.com/Arize-ai/openinference/pull/3778)  | Sep 17, 2026  |  
| [.cursor/skills](https://github.com/Arize-ai/openinference/tree/main/.cursor/skills "This path skips through empty directories")  | [.cursor/skills](https://github.com/Arize-ai/openinference/tree/main/.cursor/skills "This path skips through empty directories")  | [chore(js): migrate CLAUDE.md to AGENTS.md with editor-agnostic skills…](https://github.com/Arize-ai/openinference/commit/b91a2aa8d1277f828d38cac3c58b5c51109b838e "chore\(js\): migrate CLAUDE.md to AGENTS.md with editor-agnostic skills structure \(#2773\)

Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>")  | Feb 22, 2026  |  
|   |   |  [feat(openai-go): trace the Decisions API as DECISION spans (](https://github.com/Arize-ai/openinference/commit/bd50feab7928cce239e894c93392bb4b92cb853a "feat\(openai-go\): trace the Decisions API as DECISION spans \(#3994\)

Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>")[#3994](https://github.com/Arize-ai/openinference/pull/3994)[)](https://github.com/Arize-ai/openinference/commit/bd50feab7928cce239e894c93392bb4b92cb853a "feat\(openai-go\): trace the Decisions API as DECISION spans \(#3994\)

Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>")  | Oct 9, 2026  |  
| [docs/i18n](https://github.com/Arize-ai/openinference/tree/main/docs/i18n "This path skips through empty directories")  | [docs/i18n](https://github.com/Arize-ai/openinference/tree/main/docs/i18n "This path skips through empty directories")  |  [docs: add README translations in 32 languages (](https://github.com/Arize-ai/openinference/commit/8f920cc363f089b4920b099ca4ac4435382eca94 "docs: add README translations in 32 languages \(#3980\)

Co-authored-by: Claude <noreply@anthropic.com>")[#3980](https://github.com/Arize-ai/openinference/pull/3980)[)](https://github.com/Arize-ai/openinference/commit/8f920cc363f089b4920b099ca4ac4435382eca94 "docs: add README translations in 32 languages \(#3980\)

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 8, 2026  |  
|   | Oct 9, 2026  |  
| [internal_docs/specs](https://github.com/Arize-ai/openinference/tree/main/internal_docs/specs "This path skips through empty directories")  | [internal_docs/specs](https://github.com/Arize-ai/openinference/tree/main/internal_docs/specs "This path skips through empty directories")  |  [feat(semconv): add audio and video semantic conventions (](https://github.com/Arize-ai/openinference/commit/6d9f81360eaf2ec90e03c43d2f0c617909ee2f4d "feat\(semconv\): add audio and video semantic conventions \(#3700\)

Co-authored-by: Mikyo King <mikeldking@gmail.com>")[#3700](https://github.com/Arize-ai/openinference/pull/3700)[)](https://github.com/Arize-ai/openinference/commit/6d9f81360eaf2ec90e03c43d2f0c617909ee2f4d "feat\(semconv\): add audio and video semantic conventions \(#3700\)

Co-authored-by: Mikyo King <mikeldking@gmail.com>")  | Sep 10, 2026  |  
|   | Oct 7, 2026  |  
|   | Oct 9, 2026  |  
|   |   |  [fix(bedrock): include cache tokens in Converse prompt token count (](https://github.com/Arize-ai/openinference/commit/279c2385f46f29f7e823d77a4c0a2ecb34344d94 "fix\(bedrock\): include cache tokens in Converse prompt token count \(#3855\)")[#3855](https://github.com/Arize-ai/openinference/pull/3855)  | Oct 9, 2026  |  
|   |   |  [docs: add README translations in 32 languages (](https://github.com/Arize-ai/openinference/commit/8f920cc363f089b4920b099ca4ac4435382eca94 "docs: add README translations in 32 languages \(#3980\)

Co-authored-by: Claude <noreply@anthropic.com>")[#3980](https://github.com/Arize-ai/openinference/pull/3980)[)](https://github.com/Arize-ai/openinference/commit/8f920cc363f089b4920b099ca4ac4435382eca94 "docs: add README translations in 32 languages \(#3980\)

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 8, 2026  |  
| [feat(instrumentation): add hide_retrieval_documents TraceConfig option (](https://github.com/Arize-ai/openinference/commit/19363a0a6d978b8200ed5f73a00609f3f6cdf772 "feat\(instrumentation\): add hide_retrieval_documents TraceConfig option \(#3829\)")  | Oct 2, 2026  |  
|   |   |  [feat(ollama): add Ollama instrumentor (](https://github.com/Arize-ai/openinference/commit/237ce2b413e89782ad93431d39581a1ee44cad95 "feat\(ollama\): add Ollama instrumentor \(#3348\)

Co-authored-by: Mikyo King <mikeldking@gmail.com>")[#3348](https://github.com/Arize-ai/openinference/pull/3348)[)](https://github.com/Arize-ai/openinference/commit/237ce2b413e89782ad93431d39581a1ee44cad95 "feat\(ollama\): add Ollama instrumentor \(#3348\)

Co-authored-by: Mikyo King <mikeldking@gmail.com>")  | Aug 7, 2026  |  
|   |   |  [feat(claude-agent-sdk): Add support for Claude Agent SDK (](https://github.com/Arize-ai/openinference/commit/6f627e74a0e06e823aa593922d8d13b8d3d9aa22 "feat\(claude-agent-sdk\): Add support for Claude Agent SDK \(#2796\)

Co-authored-by: Roger Hu <rogerhu1989@gmail.com>
Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>")[#2796](https://github.com/Arize-ai/openinference/pull/2796)[)](https://github.com/Arize-ai/openinference/commit/6f627e74a0e06e823aa593922d8d13b8d3d9aa22 "feat\(claude-agent-sdk\): Add support for Claude Agent SDK \(#2796\)

Co-authored-by: Roger Hu <rogerhu1989@gmail.com>
Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>")  | Mar 4, 2026  |  
| [.release-please-manifest.json](https://github.com/Arize-ai/openinference/blob/main/.release-please-manifest.json ".release-please-manifest.json")  | [.release-please-manifest.json](https://github.com/Arize-ai/openinference/blob/main/.release-please-manifest.json ".release-please-manifest.json")  |   | Oct 9, 2026  |  
|   |   | [chore(js): migrate CLAUDE.md to AGENTS.md with editor-agnostic skills…](https://github.com/Arize-ai/openinference/commit/b91a2aa8d1277f828d38cac3c58b5c51109b838e "chore\(js\): migrate CLAUDE.md to AGENTS.md with editor-agnostic skills structure \(#2773\)

Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>")  | Feb 22, 2026  |  
|   |   |   | Dec 28, 2023  |  
|   |   | [chore(js): migrate CLAUDE.md to AGENTS.md with editor-agnostic skills…](https://github.com/Arize-ai/openinference/commit/b91a2aa8d1277f828d38cac3c58b5c51109b838e "chore\(js\): migrate CLAUDE.md to AGENTS.md with editor-agnostic skills structure \(#2773\)

Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>")  | Feb 22, 2026  |  
|   |   | [docs: Create CODE_OF_CONDUCT.md](https://github.com/Arize-ai/openinference/commit/6ce67d8693715d214dea24bc3765d6b04a9e6e09 "docs: Create CODE_OF_CONDUCT.md")  | Dec 28, 2023  |  
|   |   |  [chore(docs): update contribution guideline (](https://github.com/Arize-ai/openinference/commit/437b4e45420a1002635e0d720c16b158afa4b731 "chore\(docs\): update contribution guideline \(#2924\)")[#2924](https://github.com/Arize-ai/openinference/pull/2924)[)](https://github.com/Arize-ai/openinference/commit/437b4e45420a1002635e0d720c16b158afa4b731 "chore\(docs\): update contribution guideline \(#2924\)")  | Mar 27, 2026  |  
|   |   |   | Jan 2, 2024  |  
|   |   |  [chore: fix ruff config and auto-fix in Makefile and format agentspec (](https://github.com/Arize-ai/openinference/commit/0d752f29819d542cfd302be114121206e4369b85 "chore: fix ruff config and auto-fix in Makefile and format agentspec \(#2841\)

Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>")[#…](https://github.com/Arize-ai/openinference/pull/2841)  | Mar 10, 2026  |  
|   |   |  [docs: add README translations in 32 languages (](https://github.com/Arize-ai/openinference/commit/8f920cc363f089b4920b099ca4ac4435382eca94 "docs: add README translations in 32 languages \(#3980\)

Co-authored-by: Claude <noreply@anthropic.com>")[#3980](https://github.com/Arize-ai/openinference/pull/3980)[)](https://github.com/Arize-ai/openinference/commit/8f920cc363f089b4920b099ca4ac4435382eca94 "docs: add README translations in 32 languages \(#3980\)

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 8, 2026  |  
|   |   |   | Dec 29, 2023  |  
|   |   |  [chore(js): remove js examples (](https://github.com/Arize-ai/openinference/commit/d563e428983808b9151d2b7c868d3778c33441a8 "chore\(js\): remove js examples \(#2970\)")[#2970](https://github.com/Arize-ai/openinference/pull/2970)[)](https://github.com/Arize-ai/openinference/commit/d563e428983808b9151d2b7c868d3778c33441a8 "chore\(js\): remove js examples \(#2970\)")  | Apr 9, 2026  |  
|   |   |  [feat: mastra session support (](https://github.com/Arize-ai/openinference/commit/4f1dda22693b0f322fc1e8b0a96b808fe6076f06 "feat: mastra session support \(#2092\)

Co-authored-by: Mikyo King <mikyo@arize.com>")[#2092](https://github.com/Arize-ai/openinference/pull/2092)[)](https://github.com/Arize-ai/openinference/commit/4f1dda22693b0f322fc1e8b0a96b808fe6076f06 "feat: mastra session support \(#2092\)

Co-authored-by: Mikyo King <mikyo@arize.com>")  | Aug 23, 2025  |  
| [release-please-config.json](https://github.com/Arize-ai/openinference/blob/main/release-please-config.json "release-please-config.json")  | [release-please-config.json](https://github.com/Arize-ai/openinference/blob/main/release-please-config.json "release-please-config.json")  |  [feat(python): add openinference-instrumentation-typesafe (](https://github.com/Arize-ai/openinference/commit/13bd908a5ddfe65ba22fa519e3f842fe56157639 "feat\(python\): add openinference-instrumentation-typesafe \(#3773\)

Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: Mikyo King <mikeldking@gmail.com>")[#3773](https://github.com/Arize-ai/openinference/pull/3773)[)](https://github.com/Arize-ai/openinference/commit/13bd908a5ddfe65ba22fa519e3f842fe56157639 "feat\(python\): add openinference-instrumentation-typesafe \(#3773\)

Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: Mikyo King <mikeldking@gmail.com>")  | Sep 18, 2026  |  
|   |   |  [chore: add agent-browser skill and refresh phoenix-cli (](https://github.com/Arize-ai/openinference/commit/31e8a971d2ed801b682696f2d8abe5016c819ad3 "chore: add agent-browser skill and refresh phoenix-cli \(#3108\)")[#3108](https://github.com/Arize-ai/openinference/pull/3108)[)](https://github.com/Arize-ai/openinference/commit/31e8a971d2ed801b682696f2d8abe5016c819ad3 "chore: add agent-browser skill and refresh phoenix-cli \(#3108\)")  | May 14, 2026  |  
| View all files |  
## Repository files navigation
**🇺🇸 English** • [🇨🇳 中文](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.zh.md) • [🇹🇼 繁體中文](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.zh-TW.md) • [🇯🇵 日本語](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.ja.md) • [🇵🇹 Português](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.pt-PT.md) • [🇧🇷 Português](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.pt-BR.md) • [🇰🇷 한국어](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.ko.md) • [🇪🇸 Español](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.es.md) • [🇩🇪 Deutsch](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.de.md) • [🇫🇷 Français](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.fr.md) • [🇮🇱 עברית](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.he.md) • [🇸🇦 العربية](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.ar.md) • [🇷🇺 Русский](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.ru.md) • [🇵🇱 Polski](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.pl.md) • [🇨🇿 Čeština](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.cs.md) • [🇳🇱 Nederlands](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.nl.md) • [🇹🇷 Türkçe](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.tr.md) • [🇺🇦 Українська](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.uk.md) • [🇻🇳 Tiếng Việt](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.vi.md) • [🇵🇭 Tagalog](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.tl.md) • [🇮🇩 Indonesia](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.id.md) • [🇹🇭 ไทย](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.th.md) • [🇮🇳 हिन्दी](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.hi.md) • [🇧🇩 বাংলা](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.bn.md) • [🇵🇰 اردو](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.ur.md) • [🇷🇴 Română](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.ro.md) • [🇸🇪 Svenska](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.sv.md) • [🇮🇹 Italiano](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.it.md) • [🇬🇷 Ελληνικά](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.el.md) • [🇭🇺 Magyar](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.hu.md) • [🇫🇮 Suomi](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.fi.md) • [🇩🇰 Dansk](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.da.md) • [🇳🇴 Norsk](https://github.com/Arize-ai/openinference/blob/main/docs/i18n/README.no.md)
OpenInference is a set of conventions and plugins that is complementary to [OpenTelemetry](https://opentelemetry.io/) to enable tracing of AI applications. OpenInference is natively supported by [Arize Phoenix](https://github.com/Arize-ai/phoenix) and [Arize AX](https://arize.com/products/ax?utm_source=docs&utm_medium=web&utm_content=openinference), but can be used with any OpenTelemetry-compatible backend as well.
## Specification
The OpenInference specification is edited in markdown files found in the [spec directory](https://github.com/Arize-ai/openinference/blob/main/spec). It's designed to provide insight into the invocation of LLMs and the surrounding application context such as retrieval from vector stores and the usage of external tools such as search engines or APIs. The specification is transport and file-format agnostic, and is intended to be used in conjunction with other specifications such as JSON, ProtoBuf, and DataFrames.
## Instrumentation
OpenInference provides a set of instrumentations for popular machine learning SDKs and frameworks in a variety of languages.
## Python
### Libraries  
| Package  | Description  | Version  |  
| --- | --- | --- |  
| [`openinference-semantic-conventions`](https://github.com/Arize-ai/openinference/blob/main/python/openinference-semantic-conventions)  | Semantic conventions for tracing of LLM Apps.  |  
| [`openinference-instrumentation`](https://github.com/Arize-ai/openinference/blob/main/python/openinference-instrumentation)  | Reusable utilities, decorators, configurations, and helpers for instrumentation.  |  
| [`openinference-instrumentation-agno`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-agno)  | OpenInference Instrumentation for Agno Agents.  |  
| [`openinference-instrumentation-ag2`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-ag2)  | OpenInference Instrumentation for AG2.  |  
| [`openinference-instrumentation-openai`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-openai)  | OpenInference Instrumentation for OpenAI SDK.  |  
| [`openinference-instrumentation-openai-agents`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-openai-agents)  | OpenInference Instrumentation for OpenAI Agents SDK.  |  
| [`openinference-instrumentation-claude-agent-sdk`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-claude-agent-sdk)  | OpenInference Instrumentation for Claude Agent SDK.  |  
| [`openinference-instrumentation-llama-index`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-llama-index)  | OpenInference Instrumentation for LlamaIndex.  |  
| [`openinference-instrumentation-dspy`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-dspy)  | OpenInference Instrumentation for DSPy.  |  
| [`openinference-instrumentation-bedrock`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-bedrock)  | OpenInference Instrumentation for AWS Bedrock.  |  
| [`openinference-instrumentation-langchain`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-langchain)  | OpenInference Instrumentation for LangChain.  |  
| [`openinference-instrumentation-mcp`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-mcp)  | OpenInference Instrumentation for MCP.  |  
| [`openinference-instrumentation-mistralai`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-mistralai)  | OpenInference Instrumentation for MistralAI.  |  
| [`openinference-instrumentation-portkey`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-portkey)  | OpenInference Instrumentation for Portkey.  |  
| [`openinference-instrumentation-guardrails`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-guardrails)  | OpenInference Instrumentation for Guardrails.  |  
| [`openinference-instrumentation-vertexai`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-vertexai)  | OpenInference Instrumentation for VertexAI.  |  
| [`openinference-instrumentation-crewai`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-crewai)  | OpenInference Instrumentation for CrewAI.  |  
| [`openinference-instrumentation-haystack`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-haystack)  | OpenInference Instrumentation for Haystack.  |  
| [`openinference-instrumentation-litellm`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-litellm)  | OpenInference Instrumentation for liteLLM.  |  
| [`openinference-instrumentation-groq`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-groq)  | OpenInference Instrumentation for Groq.  |  
| [`openinference-instrumentation-instructor`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-instructor)  | OpenInference Instrumentation for Instructor.  |  
| [`openinference-instrumentation-anthropic`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-anthropic)  | OpenInference Instrumentation for Anthropic.  |  
| [`openinference-instrumentation-beeai`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-beeai)  | OpenInference Instrumentation for BeeAI.  |  
| [`openinference-instrumentation-google-genai`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-google-genai)  | OpenInference Instrumentation for Google GenAI.  |  
| [`openinference-instrumentation-google-adk`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-google-adk)  | OpenInference Instrumentation for Google ADK.  |  
| [`openinference-instrumentation-autogen-agentchat`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-autogen-agentchat)  | OpenInference Instrumentation for Microsoft Autogen AgentChat.  |  
| [`openinference-instrumentation-pydantic-ai`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-pydantic-ai)  | OpenInference Instrumentation for PydanticAI.  |  
| [`openinference-instrumentation-smolagents`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-smolagents)  | OpenInference Instrumentation for smolagents.  |  
| [`openinference-instrumentation-pipecat`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-pipecat)  | OpenInference Instrumentation for Pipecat.  |  
| [`openinference-instrumentation-agentspec`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-agentspec)  | OpenInference Instrumentation for Open Agent Specification.  |  
| [`openinference-instrumentation-strands-agents`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-strands-agents)  | OpenInference Instrumentation for Strands Agents.  |  
| [`openinference-instrumentation-together`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-together)  | OpenInference Instrumentation for Together AI.  |  
| [`openinference-instrumentation-typesafe`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-typesafe)  | OpenInference Instrumentation for TypeSafe AI.  |  
| [`openinference-instrumentation-ollama`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-ollama)  | OpenInference Instrumentation for Ollama.  |  
| [`openinference-instrumentation-cohere`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-cohere)  | OpenInference Instrumentation for Cohere.  |  
## Span Processors
Normalize and convert data across other instrumentation libraries by adding span processors that unify data.  
| Package  | Description  | Version  |  
| --- | --- | --- |  
| [`openinference-instrumentation-openlit`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-openlit)  | OpenInference Span Processor for OpenLIT traces.  |  
| [`openinference-instrumentation-openllmetry`](https://github.com/Arize-ai/openinference/blob/main/python/instrumentation/openinference-instrumentation-openllmetry)  | OpenInference Span Processor for OpenLLMetry (Traceloop) traces.  |  
### Examples  
| Name  | Description  | Complexity Level  |  
| --- | --- | --- |  
| Agno agent examples  | Beginner  |  
| OpenAI Python SDK, including chat completions and embeddings  | Beginner  |  
| Claude Agent SDK  | Beginner  |  
| MistralAI Python SDK  | Beginner  |  
| VertexAI Python SDK  | Beginner  |  
| LlamaIndex query engines  | Beginner  |  
| DSPy primitives and custom RAG modules  | Beginner  |  
| Boto3 Bedrock client  | Beginner  |  
| LangChain primitives and simple chains  | Beginner  |  
| A lightweight LiteLLM framework  | Beginner  |  
| LiteLLM Proxy to log OpenAI, Azure, Vertex, Bedrock  | Beginner  |  
| Groq and AsyncGroq chat completions  | Beginner  |  
| Together and AsyncTogether chat completions, streaming, and tool calls  | Beginner  |  
| TypeSafeClient and AsyncTypeSafeClient System One calls (Noul, Choice, Score)  | Beginner  |  
| Anthropic Messages client  | Beginner  |  
| Agentic instrumentation in the BeeAI framework  | Beginner  |  
| A Haystack QA RAG application  | Intermediate  |  
| OpenAI Agents with handoffs  | Intermediate  |  
| Microsoft Autogen Assistant Agent and Team Chat  | Intermediate  |  
| AG2 tool calling, group chats, and sequential chats  | Intermediate  |  
| PydanticAI agent examples  | Intermediate  |  
| Pipecat application examples  | Intermediate  |  
| Ollama chat, streaming, and tool calls  | Beginner  |  
| Cohere ClientV2 chat completions  | Beginner  |  
## JavaScript
### Libraries  
| Package  | Description  | Version  |  
| --- | --- | --- |  
| [`@arizeai/openinference-semantic-conventions`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-semantic-conventions)  | Semantic conventions for tracing of LLM Apps.  |  
| [`@arizeai/openinference-core`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-core)  | Reusable utilities, configuration, and helpers for instrumentation.  |  
| [`@arizeai/openinference-instrumentation-bedrock`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-bedrock)  | OpenInference Instrumentation for AWS Bedrock.  |  
| [`@arizeai/openinference-instrumentation-bedrock-agent-runtime`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-bedrock-agent-runtime)  | OpenInference Instrumentation for AWS Bedrock Agent Runtime.  |  
| [`@arizeai/openinference-instrumentation-beeai`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-beeai)  | OpenInference Instrumentation for BeeAI.  |  
| [`@arizeai/openinference-instrumentation-langchain`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-langchain)  | OpenInference Instrumentation for LangChain.js.  |  
| [`@arizeai/openinference-instrumentation-mcp`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-mcp)  | OpenInference Instrumentation for MCP.  |  
| [`@arizeai/openinference-instrumentation-openai`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-openai)  | OpenInference Instrumentation for OpenAI SDK.  |  
| [`@arizeai/openinference-instrumentation-anthropic`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-anthropic)  | OpenInference Instrumentation for the Anthropic SDK.  |  
| [`@arizeai/openinference-instrumentation-claude-agent-sdk`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-claude-agent-sdk)  | OpenInference Instrumentation for Claude Agent SDK.  |  
| [`@arizeai/openinference-vercel`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-vercel)  | OpenInference Support for Vercel AI SDK.  |  
| [`@arizeai/openinference-tanstack-ai`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-tanstack-ai)  | OpenInference middleware for TanStack AI.  |  
| [`@arizeai/openinference-genai`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-genai)  | OpenInference Support for GenAI conventions  |  
| [`@arizeai/openinference-instrumentation-typesafe`](https://github.com/Arize-ai/openinference/blob/main/js/packages/openinference-instrumentation-typesafe)  | OpenInference Instrumentation for the TypeSafe AI SDK.  |  
### Examples  
| Name  | Description  | Complexity Level  |  
| --- | --- | --- |  
| Classification, structured questions, and confidence-gated routing to an LLM  | Beginner  |  
### Libraries  
| Package  | Description  | Version  |  
| --- | --- | --- |  
| [`openinference-semantic-conventions`](https://github.com/Arize-ai/openinference/blob/main/java/openinference-semantic-conventions)  | Semantic conventions for tracing of LLM Apps.  |  
| [`openinference-instrumentation`](https://github.com/Arize-ai/openinference/blob/main/java/openinference-instrumentation)  | Base instrumentation utilities.  |  
| [`openinference-instrumentation-langchain4j`](https://github.com/Arize-ai/openinference/blob/main/java/instrumentation/openinference-instrumentation-langchain4j)  | OpenInference Instrumentation for LangChain4j.  |  
| [`openinference-instrumentation-springAI`](https://github.com/Arize-ai/openinference/blob/main/java/instrumentation/openinference-instrumentation-springAI)  | OpenInference Instrumentation for Spring AI.  |  
| [`openinference-instrumentation-annotation`](https://github.com/Arize-ai/openinference/blob/main/java/instrumentation/openinference-instrumentation-annotation)  | Annotation-based manual tracing with ByteBuddy.  |  
| [`openinference-instrumentation-adk-java`](https://github.com/Arize-ai/openinference/blob/main/java/instrumentation/openinference-instrumentation-adk-java)  | OpenInference Instrumentation for Google ADK Java (`-javaagent`).  |  
### Examples  
| Name  | Description  | Complexity Level  |  
| --- | --- | --- |  
| Simple example using LangChain4j with OpenAI  | Beginner  |  
| Spring AI example with OpenAI and tool calling  | Beginner  |  
| Annotation-based tracing with `@Chain`, `@LLM`, `@Tool`, `@Agent`  | Beginner  |  
| [Programmatic Example](https://github.com/Arize-ai/openinference/blob/main/java/examples/programmatic-example)  | Manual tracing with typed span classes (`LLMSpan`, `AgentSpan`, etc.)  | Beginner  |  
| Google ADK for Java agent with a function tool, traced by the ADK `-javaagent`  | Beginner  |  
### Libraries  
| Package  | Description  | Version  |  
| --- | --- | --- |  
| [`openinference-semantic-conventions`](https://github.com/Arize-ai/openinference/blob/main/go/openinference-semantic-conventions)  | Semantic conventions for tracing of LLM Apps.  | `go get github.com/Arize-ai/openinference/go/openinference-semantic-conventions`  |  
| [`openinference-instrumentation`](https://github.com/Arize-ai/openinference/blob/main/go/openinference-instrumentation)  | Suppression, context attribute propagation, and `TraceConfig` masking. Honors `OPENINFERENCE_HIDE_*` env vars.  | `go get github.com/Arize-ai/openinference/go/openinference-instrumentation`  |  
| [`openinference-instrumentation-anthropic-sdk-go`](https://github.com/Arize-ai/openinference/blob/main/go/openinference-instrumentation-anthropic-sdk-go)  | OpenInference Instrumentation for the Anthropic Go SDK (`anthropics/anthropic-sdk-go`).  | `go get github.com/Arize-ai/openinference/go/openinference-instrumentation-anthropic-sdk-go`  |  
| [`openinference-instrumentation-openai-go`](https://github.com/Arize-ai/openinference/blob/main/go/openinference-instrumentation-openai-go)  | OpenInference Instrumentation for the official OpenAI Go SDK ([`openai/openai-go`](https://github.com/openai/openai-go)).  | `go get github.com/Arize-ai/openinference/go/openinference-instrumentation-openai-go`  |  
Requires Go 1.25+. Pair with [`arize-otel-go`](https://github.com/Arize-ai/arize-otel-go) for the one-line OTLP/HTTP setup to [Arize AX](https://arize.com/products/ax?utm_source=docs&utm_medium=web&utm_content=openinference), or wire up any OTel exporter (e.g. [Phoenix](https://github.com/Arize-ai/phoenix) at `http://localhost:6006/v1/traces`).
## Supported Destinations
OpenInference supports the following destinations as span collectors.
  * ✅ Any OTEL-compatible collector


## Community
Join our community to connect with thousands of machine learning practitioners and LLM observability enthusiasts!
  * 🌍 Join our [Slack community](https://join.slack.com/t/arize-ai/shared_invite/zt-3lqwr2oc3-7rhdyYEh82zJL_UhPKrb0A).
  * 💡 Ask questions and provide feedback in the _#phoenix-support_ channel.
  * 🌟 Leave a star on our [GitHub](https://github.com/Arize-ai/openinference).
  * 🐞 Report bugs with [GitHub Issues](https://github.com/Arize-ai/openinference/issues).
  * 𝕏 Follow us on [X](https://twitter.com/ArizePhoenix).
  * 🗺️ Check out our [roadmap](https://github.com/orgs/Arize-ai/projects/45) to see where we're heading next.


## About
OpenTelemetry Instrumentation for AI Observability
[arize-ai.github.io/openinference/](https://arize-ai.github.io/openinference/)
### Topics
[aiops](https://github.com/topics/aiops)[gemini](https://github.com/topics/gemini)[hacktoberfest](https://github.com/topics/hacktoberfest)[haystack](https://github.com/topics/haystack)[langchain](https://github.com/topics/langchain)[langraph](https://github.com/topics/langraph)[llamaindex](https://github.com/topics/llamaindex)[llmops](https://github.com/topics/llmops)[llms](https://github.com/topics/llms)[mcp](https://github.com/topics/mcp)[openai](https://github.com/topics/openai)[openai-agents](https://github.com/topics/openai-agents)[opentelemetry](https://github.com/topics/opentelemetry)[pydantic-ai](https://github.com/topics/pydantic-ai)[smolagents](https://github.com/topics/smolagents)[telemetry](https://github.com/topics/telemetry)[tracing](https://github.com/topics/tracing)[vercel](https://github.com/topics/vercel)[vertex](https://github.com/topics/vertex)
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**1.3k** stars
### Watchers
**14** watching
### Forks
[ forks](https://github.com/Arize-ai/openinference/forks)
## [Releases](https://github.com/Arize-ai/openinference/releases)1,531 (1,531)
[ @arizeai/openinference-vercel@3.3.0Latest Oct 9, 2026 ](https://github.com/Arize-ai/openinference/releases/tag/%40arizeai%2Fopeninference-vercel%403.3.0)
## 
[+ 447](https://github.com/Arize-ai/openinference/network/dependents)
## 
## Languages
  * [Python67.9%](https://github.com/Arize-ai/openinference/search?l=python)
  * [TypeScript22.9%](https://github.com/Arize-ai/openinference/search?l=typescript)
  * [Java5%](https://github.com/Arize-ai/openinference/search?l=java)
  * [Jupyter Notebook0.9%](https://github.com/Arize-ai/openinference/search?l=jupyter-notebook)
  * [Shell0.2%](https://github.com/Arize-ai/openinference/search?l=shell)
  * [Other0.1%](https://github.com/Arize-ai/openinference/search?l=Other)


You can’t perform that action at this time. 
