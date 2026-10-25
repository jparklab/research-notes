---
title: 'GitHub - ollama/ollama: Get up and running with Kimi, GLM, MiniMax, DeepSeek,
  gpt-oss, Qwen, Gemma and other models. · GitHub'
id: github-ollamaollama-get-up-and-running-with-kimi-glm-minimax-deepseek-gpt-oss-qw
tags:
- internal-llm-inference-stacks-7f0c90
- ollama
- llamacpp
- ground-truth
- architecture-correction
created: '2026-10-09T23:02:33.202668Z'
updated: '2026-10-10T14:52:17.171581Z'
source: https://github.com/ollama/ollama
source_domain: github.com
fetched_at: '2026-10-09T23:02:33.180410Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Primary source: Ollama''s own GitHub repository (README + commit log), fetched
  to check the HN-thread claim (from Ollama co-founder ''mchiang'') that Ollama had
  ''moved off of llama.cpp as a wrapper.'' The repo''s own commit history CONTRADICTS
  or at least substantially complicates that framing with a later architectural reversal.
  A commit titled ''runner: Remove CGO engines, use llama-server exclusively for GGML
  models (#16031)'' (squashed from a branch dated 2026-04-29, i.e. chronologically
  AFTER the period mchiang''s multimodal-engine comments describe) states verbatim:
  ''Remove the vendored GGML and llama.cpp backend, CGO runner, Go model implementations,
  and sample. llama-server (built from upstream llama.cpp via FetchContent) is now
  the sole inference engine for GGUF-based models... This allows us to more rapidly
  pick up new capabilities and fixes from llama.cpp as they come out.'' In other words:
  Ollama''s custom Go/CGO model implementations (the basis for mchiang''s ''we implement
  our own models'' claim) were themselves later REMOVED, and Ollama returned to depending
  directly on upstream llama-server/llama.cpp as the sole GGUF inference path -- with
  a separate, newer MLX runner handling safetensors-based models. The current README''s
  ''Supported backends'' section lists only: ''llama.cpp project founded by Georgi
  Gerganov.'' NET ASSESSMENT: the wrapper-vs-independent-engine framing in the HN
  thread was a snapshot of a specific, apparently short-lived architecture phase;
  as of this repo''s current state (commit activity visible through Oct 6, 2026, e.g.
  ''llama.cpp: version update #18761''), Ollama is again built directly on upstream
  llama.cpp (via llama-server) for GGUF models, with MLX as a parallel path for safetensors
  models. This is a genuine first-party architecture correction that should supersede
  both the Picovoice (2024) and the HN-thread (2026, pre-reversal) framings.'
---

*Suggested by [[i-see-a-lot-of-hate-for-ollama-doing-this-kind-of-thing-but-also-they-remain-one]] — primary repo evidence for Ollama's architecture / whether it still wraps llama.cpp*

[Skip to content](https://github.com/ollama/ollama#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/ollama/ollama) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/ollama/ollama) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/ollama/ollama) to refresh your session. Dismiss alert
[**473** Branches](https://github.com/ollama/ollama/branches)[**689** Tags](https://github.com/ollama/ollama/tags)
Go to file
Open more actions menu
## Latest commit
[server: skip local model compatibility migration (](https://github.com/ollama/ollama/commit/eab97e9f92b9a25c2d52d2cc6c1b1c99bd9fae21)[#18908](https://github.com/ollama/ollama/pull/18908)[)](https://github.com/ollama/ollama/commit/eab97e9f92b9a25c2d52d2cc6c1b1c99bd9fae21)
success
Oct 9, 2026
[eab97e9](https://github.com/ollama/ollama/commit/eab97e9f92b9a25c2d52d2cc6c1b1c99bd9fae21) · Oct 9, 2026
## History
[5,850 Commits](https://github.com/ollama/ollama/commits/main/)
Open commit details
5,850 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [Merge remote-tracking branch 'upstream/main' into release_v0.40.0](https://github.com/ollama/ollama/commit/0fde06c4d2a578e3eae0c94bde4923ec988dce9e "Merge remote-tracking branch 'upstream/main' into release_v0.40.0")  | Oct 4, 2026  |  
|   |   |  [api: expose model thinking levels and defaults (](https://github.com/ollama/ollama/commit/d0c8cdb795d4640890d87c146d674cc311df4138 "api: expose model thinking levels and defaults \(#18473\)")[#18473](https://github.com/ollama/ollama/pull/18473)[)](https://github.com/ollama/ollama/commit/d0c8cdb795d4640890d87c146d674cc311df4138 "api: expose model thinking levels and defaults \(#18473\)")  | Sep 18, 2026  |  
|  [model: add multimodal embeddings (](https://github.com/ollama/ollama/commit/efe43c5561047d8baf8c862730b2151249d51fd5 "model: add multimodal embeddings \(#18820\)

Implements the EmbeddingGemma2Model architecture on the MLX runner: 24-layer
bidirectional text encoder with PLE, shared gemma4 vision/audio towers,
mean-pool + L2 output. /api/embed accepts per-item media via input dicts.")[#18820](https://github.com/ollama/ollama/pull/18820)[)](https://github.com/ollama/ollama/commit/efe43c5561047d8baf8c862730b2151249d51fd5 "model: add multimodal embeddings \(#18820\)

Implements the EmbeddingGemma2Model architecture on the MLX runner: 24-layer
bidirectional text encoder with PLE, shared gemma4 vision/audio towers,
mean-pool + L2 output. /api/embed accepts per-item media via input dicts.")  | Oct 6, 2026  |  
|  [Merge pull request](https://github.com/ollama/ollama/commit/f9f4af6ce09223fde24949a5b3ccff061ca0abf4 "Merge pull request #18233 from chenlichao/fix/app-readme-download-links

docs: fix broken download links in app README") [#18233](https://github.com/ollama/ollama/pull/18233) [from chenlichao/fix/app-readme-download-links](https://github.com/ollama/ollama/commit/f9f4af6ce09223fde24949a5b3ccff061ca0abf4 "Merge pull request #18233 from chenlichao/fix/app-readme-download-links

docs: fix broken download links in app README")  | Oct 7, 2026  |  
|  [auth: fix problems with the ollama keypairs (](https://github.com/ollama/ollama/commit/64883e3c4c0238dc70fddcc456af569d1489415d "auth: fix problems with the ollama keypairs \(#12373\)

* auth: fix problems with the ollama keypairs

This change adds several fixes including:
  - reading in the pubkey files correctly
  - fixing the push unit test to create a keypair file in a temp directory
  - not return 500 errors for normal status error")[#12373](https://github.com/ollama/ollama/pull/12373)[)](https://github.com/ollama/ollama/commit/64883e3c4c0238dc70fddcc456af569d1489415d "auth: fix problems with the ollama keypairs \(#12373\)

* auth: fix problems with the ollama keypairs

This change adds several fixes including:
  - reading in the pubkey files correctly
  - fixing the push unit test to create a keypair file in a temp directory
  - not return 500 errors for normal status error")  | Sep 23, 2025  |  
|   |   |  [mlx: mitigate high latency after GPU idle (](https://github.com/ollama/ollama/commit/cf6c9de620caac8c3a1167ed476ec4773617ce40 "mlx: mitigate high latency after GPU idle \(#18807\)

Carry the MLX residency-refresh patch and enable it every second in the
runner, preserving explicit environment overrides.

Fixes #18744.")[#18807](https://github.com/ollama/ollama/pull/18807)[)](https://github.com/ollama/ollama/commit/cf6c9de620caac8c3a1167ed476ec4773617ce40 "mlx: mitigate high latency after GPU idle \(#18807\)

Carry the MLX residency-refresh patch and enable it every second in the
runner, preserving explicit environment overrides.

Fixes #18744.")  | Oct 5, 2026  |  
|  [launch: suppress Claude Code connector warning for Ollama (](https://github.com/ollama/ollama/commit/021cda52280d52dbb26cfa98e4ab4a97ab57bb6a "launch: suppress Claude Code connector warning for Ollama \(#18878\)

Co-authored-by: Parth Sareen <parth.sareen@ollama.com>")[#18878](https://github.com/ollama/ollama/pull/18878)[)](https://github.com/ollama/ollama/commit/021cda52280d52dbb26cfa98e4ab4a97ab57bb6a "launch: suppress Claude Code connector warning for Ollama \(#18878\)

Co-authored-by: Parth Sareen <parth.sareen@ollama.com>")  | Oct 9, 2026  |  
|   |   | [llama-server: prepare to remove compatibility patch](https://github.com/ollama/ollama/commit/75b952780f90807f651eb2f1f817e5a40126e81d "llama-server: prepare to remove compatibility patch

Add manifest-list storage so runner-specific manifests can coexist under one tag while preserving existing v1 tags as best-effort downgrade anchors. Show/list/copy/remove/pull/push now understand runner and digest selection and transfer referenced child manifests and layers.

Add lazy local compatibility migration for legacy Ollama GGUFs into llama.cpp-compatible children, covering the patched model families and preserving parser/renderer, templates, projectors, media metadata, split GGUFs, and MTP/draft tensors where applicable.

Extend create/import/conversion paths for the same compatibility rules, harden manifest-list combine validation, and add focused manifest, migration, transfer, and integration coverage for first-load conversion plus chat/tools/vision/audio/embedding behavior.")  | Sep 25, 2026  |  
|   |   |  [model: add multimodal embeddings (](https://github.com/ollama/ollama/commit/efe43c5561047d8baf8c862730b2151249d51fd5 "model: add multimodal embeddings \(#18820\)

Implements the EmbeddingGemma2Model architecture on the MLX runner: 24-layer
bidirectional text encoder with PLE, shared gemma4 vision/audio towers,
mean-pool + L2 output. /api/embed accepts per-item media via input dicts.")[#18820](https://github.com/ollama/ollama/pull/18820)[)](https://github.com/ollama/ollama/commit/efe43c5561047d8baf8c862730b2151249d51fd5 "model: add multimodal embeddings \(#18820\)

Implements the EmbeddingGemma2Model architecture on the MLX runner: 24-layer
bidirectional text encoder with PLE, shared gemma4 vision/audio towers,
mean-pool + L2 output. /api/embed accepts per-item media via input dicts.")  | Oct 6, 2026  |  
|   |   |   | Oct 3, 2026  |  
|   |   |   | Aug 27, 2026  |  
|  [server: proxy cloud usage and balance APIs (](https://github.com/ollama/ollama/commit/f864601538bd283dbcd011244db99e2760b125c1 "server: proxy cloud usage and balance APIs \(#18829\)

Forward GET /api/usage and GET /api/balance through the local server
using the signed-in account.

Add API documentation and tests for proxying, signing, and
disabled cloud access.")[#18829](https://github.com/ollama/ollama/pull/18829)[)](https://github.com/ollama/ollama/commit/f864601538bd283dbcd011244db99e2760b125c1 "server: proxy cloud usage and balance APIs \(#18829\)

Forward GET /api/usage and GET /api/balance through the local server
using the signed-in account.

Add API documentation and tests for proxying, signing, and
disabled cloud access.")  | Oct 7, 2026  |  
|   |   |  [create: add server-side MLX imports and drop GGUF conversion (](https://github.com/ollama/ollama/commit/98acec40ae2b3ed361fc5117e5b2ae81a4bf5c18 "create: add server-side MLX imports and drop GGUF conversion \(#14969\)

* create: add server-side MLX imports and drop GGUF conversion

Support safetensors imports through the MLX create pipeline both locally and on the server, including remote upload/staging, draft layer handling, cancellation propagation, transfer limits, and shared manifest/blob writing.

Limit GGUF create to wrapping existing GGUF inputs into Ollama manifests. Remove the in-tree safetensors-to-GGUF converter, server quantization path, and converter-only dependencies so GGUF conversion and quantization stay in llama.cpp tooling.

Keep the MLX path focused on supported safetensors model creation with validation before MLX work, and expose that flow without the --experimental CLI gate.

* address comments

* add client side gguf create fast path

* address comments

* rebase adjustments")[#14969](https://github.com/ollama/ollama/pull/14969)[)](https://github.com/ollama/ollama/commit/98acec40ae2b3ed361fc5117e5b2ae81a4bf5c18 "create: add server-side MLX imports and drop GGUF conversion \(#14969\)

* create: add server-side MLX imports and drop GGUF conversion

Support safetensors imports through the MLX create pipeline both locally and on the server, including remote upload/staging, draft layer handling, cancellation propagation, transfer limits, and shared manifest/blob writing.

Limit GGUF create to wrapping existing GGUF inputs into Ollama manifests. Remove the in-tree safetensors-to-GGUF converter, server quantization path, and converter-only dependencies so GGUF conversion and quantization stay in llama.cpp tooling.

Keep the MLX path focused on supported safetensors model creation with validation before MLX work, and expose that flow without the --experimental CLI gate.

* address comments

* add client side gguf create fast path

* address comments

* rebase adjustments")  | Sep 15, 2026  |  
|   |   |  [chore(all): replace instances of interface with any (](https://github.com/ollama/ollama/commit/9876c9faa41c7dd7143fa47727520d353559f81b "chore\(all\): replace instances of interface with any \(#10067\)

Both interface{} and any \(which is just an alias for interface{} introduced in Go 1.18\) represent the empty interface that all types satisfy.")[#10067](https://github.com/ollama/ollama/pull/10067)[)](https://github.com/ollama/ollama/commit/9876c9faa41c7dd7143fa47727520d353559f81b "chore\(all\): replace instances of interface with any \(#10067\)

Both interface{} and any \(which is just an alias for interface{} introduced in Go 1.18\) represent the empty interface that all types satisfy.")  | Apr 2, 2025  |  
| [llama-server: prepare to remove compatibility patch](https://github.com/ollama/ollama/commit/75b952780f90807f651eb2f1f817e5a40126e81d "llama-server: prepare to remove compatibility patch

Add manifest-list storage so runner-specific manifests can coexist under one tag while preserving existing v1 tags as best-effort downgrade anchors. Show/list/copy/remove/pull/push now understand runner and digest selection and transfer referenced child manifests and layers.

Add lazy local compatibility migration for legacy Ollama GGUFs into llama.cpp-compatible children, covering the patched model families and preserving parser/renderer, templates, projectors, media metadata, split GGUFs, and MTP/draft tensors where applicable.

Extend create/import/conversion paths for the same compatibility rules, harden manifest-list combine validation, and add focused manifest, migration, transfer, and integration coverage for first-load conversion plus chat/tools/vision/audio/embedding behavior.")  | Sep 25, 2026  |  
|   |   | [parsers: report the strings that end a response's thinking](https://github.com/ollama/ollama/commit/a9d8953ab0edce75eeaf6e5f5ce84c9b40d92c37 "parsers: report the strings that end a response's thinking

A format on a thinking model has to leave the thinking free and constrain
only the content after it, so whatever enforces the format needs to know
where the thinking ends. Today the server guesses whether a parser's
response starts inside thinking from the think value alone, which is wrong
for parsers whose default differs, and it has no way to learn the closing
string at all.

Each parser now answers ThinkingClose after Init: the strings any of which
ends the thinking its response begins with, or none when the response
starts in content because thinking is off, an assistant prefill continues
content, or the parser suppresses thinking for tools. Parsers whose models
open a new message before content end the thinking at that message's
header. Nothing consumes the answer yet.")  | Sep 22, 2026  |  
|   |   |  [server: skip local model compatibility migration (](https://github.com/ollama/ollama/commit/eab97e9f92b9a25c2d52d2cc6c1b1c99bd9fae21 "server: skip local model compatibility migration \(#18908\)")[#18908](https://github.com/ollama/ollama/pull/18908)[)](https://github.com/ollama/ollama/commit/eab97e9f92b9a25c2d52d2cc6c1b1c99bd9fae21 "server: skip local model compatibility migration \(#18908\)")  | Oct 9, 2026  |  
|   |   |  [api: expose model thinking levels and defaults (](https://github.com/ollama/ollama/commit/d0c8cdb795d4640890d87c146d674cc311df4138 "api: expose model thinking levels and defaults \(#18473\)")[#18473](https://github.com/ollama/ollama/pull/18473)[)](https://github.com/ollama/ollama/commit/d0c8cdb795d4640890d87c146d674cc311df4138 "api: expose model thinking levels and defaults \(#18473\)")  | Sep 18, 2026  |  
|   |   |  [llama: fix clef head reads past 2GiB on windows (](https://github.com/ollama/ollama/commit/d3c846f8b0279e79b0e3a960a7575fde6920e9df "llama: fix clef head reads past 2GiB on windows \(#18777\)

The Clef head read its weights with std::ifstream::seekg. The Windows
llama-server ships with libc++ 18.1.8, where that seek truncates the
offset to 32 bits. clef-flash stores the clef.* tensors at about 9.5GB,
so the head loaded backbone bytes as its weights and every
/v1/systemone request failed with "Clef: non-finite logit". clef:27b
keeps the head and output.weight below 2GB and was unaffected.

Read with ggml_fopen and an explicit 64-bit seek instead, as read_at
in llama/compat already does.

Fixes #18769")[#18777](https://github.com/ollama/ollama/pull/18777)[)](https://github.com/ollama/ollama/commit/d3c846f8b0279e79b0e3a960a7575fde6920e9df "llama: fix clef head reads past 2GiB on windows \(#18777\)

The Clef head read its weights with std::ifstream::seekg. The Windows
llama-server ships with libc++ 18.1.8, where that seek truncates the
offset to 32 bits. clef-flash stores the clef.* tensors at about 9.5GB,
so the head loaded backbone bytes as its weights and every
/v1/systemone request failed with "Clef: non-finite logit". clef:27b
keeps the head and output.weight below 2GB and was unaffected.

Read with ggml_fopen and an explicit 64-bit seek instead, as read_at
in llama/compat already does.

Fixes #18769")  | Oct 7, 2026  |  
|  [model: add multimodal embeddings (](https://github.com/ollama/ollama/commit/efe43c5561047d8baf8c862730b2151249d51fd5 "model: add multimodal embeddings \(#18820\)

Implements the EmbeddingGemma2Model architecture on the MLX runner: 24-layer
bidirectional text encoder with PLE, shared gemma4 vision/audio towers,
mean-pool + L2 output. /api/embed accepts per-item media via input dicts.")[#18820](https://github.com/ollama/ollama/pull/18820)[)](https://github.com/ollama/ollama/commit/efe43c5561047d8baf8c862730b2151249d51fd5 "model: add multimodal embeddings \(#18820\)

Implements the EmbeddingGemma2Model architecture on the MLX runner: 24-layer
bidirectional text encoder with PLE, shared gemma4 vision/audio towers,
mean-pool + L2 output. /api/embed accepts per-item media via input dicts.")  | Oct 6, 2026  |  
|   |   | [runner: Remove CGO engines, use llama-server exclusively for GGML mod…](https://github.com/ollama/ollama/commit/9db4bdbad6a4981ad761aa2b603e69e8fb83212c "runner: Remove CGO engines, use llama-server exclusively for GGML models \(#16031\)

* broad lint fixes to sidestep CI scope glitch

* runner: Remove CGO engines, use llama-server exclusively for GGML models

Remove the vendored GGML and llama.cpp backend, CGO runner, Go model
implementations, and sample.  llama-server \(built from upstream llama.cpp via
FetchContent\) is now the sole inference engine for GGUF-based models.
\(Safetensor based models continue to run on the new MLX engine.\)  This allows
us to more rapidly pick up new capabilities and fixes from llama.cpp as they
come out.

On windows this now requires recent AMD driver versions to support ROCm v7 as
llama.cpp currently does not support building against v6.

* llama/compat: load Ollama-format GGUFs in llama-server

Squashed from upstream/jmorganca/llama-compat on 2026-04-29.
Source tip: 0c33775d378511a9b3c7f2e3b80eda355511d9f3.

Original source commits:
- 25223160d llama/compat: add in-memory shim so llama-server can load Ollama-format GGUFs
- 7449b539a llm,server: route Ollama-format gemma3 blobs through llama/compat
- 436f2e2b1 llama/compat: make patch-apply idempotent
- 8c2c9d4c8 llama/compat: extend gemma3 handler to cover 1B and 270M blobs
- 021389f7b llama/compat: shrink clip.cpp injection from 18 lines to 1
- 61b367ec2 llama/compat: shrink patch to pure call-site hooks \(34 -> 20 lines\)
- 36049361c llama/compat: simplify shim \(gemma3-tested\)
- 8fa664865 llama/compat: add qwen35moe text handler
- db0c74530 llama/compat: add qwen35moe vision \(clip\) support
- 2a388da77 llama/compat: split shared infra into a util TU
- 9a69a17dc llama/compat: document non-public API dependencies
- d0f38a915 llama/compat: add gpt-oss and lfm2 handlers
- 086071822 llama/compat: add mistral3 text handler \(vision TODO\)
- 63bde9ff7 llama/compat: add mistral3 vision \(clip\) support
- 3a57b89d5 llama/compat: apply LLaMA RoPE permute to mistral3 vision Q/K
- 99cb87439 llama/compat: add qwen35, gemma4, deepseek-ocr handlers
- 2c7850dba llama/compat: add nemotron_h_moe handler \(latent FFN + MTP skip\)
- 9e3b54225 llama/compat: add llama4 text + clip handlers
- 034fee349 llama/compat: add gemma4 clip handler \(gemma4v projector\)
- 9945c5a93 server: remove dhiltgen/* compat redirect table
- 5d4539101 llama/compat: rewrite gemma4 tokenizer model to BPE
- 7e0765327 llama/compat: add glm-ocr text handler + text-loader load-op hook
- f1bd1a25a llama/compat: add glm-ocr clip handler \(glm4v projector\)
- 4b5cf3420 llama/compat: collapse text-loader hook back to one new patch line
- eb4ecf4fc llama/compat: extend gemma4 clip handler to gemma4a \(audio\)
- a23a5e76f llama/compat: fix gemma4a per-block norm tensor mapping
- cd2dcaff4 llama/compat: add embeddinggemma handler
- 1ce8a6b26 llama/compat: add qwen3-vl + qwen2.5-vl handlers
- fd98ffa1e llama/compat: add gemma3n + glm4moelite handlers
- cc7bdf0bc llama/compat: handle null buft in maybe_load_tensor
- 0c33775d3 llama/compat: disable mmap when load_op transforms text-side tensors

* refine implementation

* ci: fix windows MLX build

* ci: fix windows llama-server build

* ci: fix windows rocm build

* ci: windows mlx tuning

Shorten long-tail on build, and get OllamaSetup.exe back under 2g limit

* ci: fix windows dependencies

* win: fix dependency gathering

* disable openmp

* win: arm64 cross-compile build

also DRY out CI steps

* scheduler improvements

* ci: improvements from #15982

* win: favor ninja for faster developer builds

* win: fix build

* win: fix arm64 cross-compile

* win: avoid spaces in compiler path

* misc discovery fixes, and bos handling

* lint fixes

* win: fix arm cross-compile build/CI bugs

* llama.cpp update

* win: handle multiple CRT dirs

* vulkan: add windows iGPU detection

* fix creation bugs for patched models, other refactoring work

* tune batch size for better performance

* ci and lint fixes

* fix repeat_last_n bug

* build: revamp build for better developer UX

* amd, sampler, qwen3next fixes

* version bump

* fix mlx build

* revamp GPU discovery

Scanning the output of llama-server is turning out to be too error prone across
llama.cpp updates, so this switches to a thin dynamic library load against the
bundled GGML libraries so more details can be gathered from the API.

* version bump

* missing file

* ci: fix cache miss on rocm build

* refine vulkan dep handling

* fix ps reporting bug on full GPU load

* improve cmake wiring for customized local builds

* version bump

* docker build arg cleanup

* improve windows exit error logs

* fix community gemma4 support and ci flakes

* fix mlx unit test

* tighten up ps logic to avoid double counting fit log lines

* version bump

* fix ps view for full gpu layer offload

* add MTP wiring for llama-server and create with GGUFs

* pick best template by capabilities

* version bump

* ci: harden apt repos

* remove unused cpu core discovery

* adjust batch default logic to reduce OOMs

* support larger tool calls

* fix audio support, template show

* qwen35 mtp patch support

* flesh out dtypes

* rocm deps

* version bump

* lint fix

* block broken gfx1150 on windows

* fix qwen3.5 moe mtp tensors in patch

* mmproj oom fallback and vulkan on by default

* qwen MTP compat fix

* version bump

* ci: fix WoA cross-compile

* ci: workaround ui tool in cross-compile

* version bump

* win: enable OpenMP for CPU builds

* build: improve developer UX

* ci: windows path workaround for CPU build

* win: fix WoA dependencies

* win: fix large offset reads for mmproj patched loads

* version bump

* fix vulkan dup detection

* add OLLAMA_IGPU_ENABLE and largely disable iGPUs by default

* opt-in MTP, win large offset, integraton fixes

* fix unit test scheduler interaction hang

* fix multi-gpu filtering

* version bump

* review comments

* fix thinking level

* fix linux rocm ordering and granite 3.3 template

* version bump

* ci fix - non-shallow MLX checkout

* bypass linux sysfs unit test on windows

---------

Co-authored-by: jmorganca <jmorganca@gmail.com>")  | May 29, 2026  |  
|   |   |  [manifest: avoid symlinks on Windows (](https://github.com/ollama/ollama/commit/a896fdf01c64b7629a81f52052ce5ca221928762 "manifest: avoid symlinks on Windows \(#18852\)

Store named manifests as regular files on Windows, where symlinks can
be created successfully but fail when opened.

Repair existing symlinks lazily on read using the verified blob and
existing copy path. Keep locking consistent across platforms and avoid
reacquiring the store lock during pruning.

Fixes #18847")[#18852](https://github.com/ollama/ollama/pull/18852)[)](https://github.com/ollama/ollama/commit/a896fdf01c64b7629a81f52052ce5ca221928762 "manifest: avoid symlinks on Windows \(#18852\)

Store named manifests as regular files on Windows, where symlinks can
be created successfully but fail when opened.

Repair existing symlinks lazily on read using the verified blob and
existing copy path. Keep locking consistent across platforms and avoid
reacquiring the store lock during pruning.

Fixes #18847")  | Oct 7, 2026  |  
|   |   |  [feat: allow ten web searches per response (](https://github.com/ollama/ollama/commit/05a7a91dae22d7eebd490fca9cf57a074518706b "feat: allow ten web searches per response \(#18602\)")[#18602](https://github.com/ollama/ollama/pull/18602)[)](https://github.com/ollama/ollama/commit/05a7a91dae22d7eebd490fca9cf57a074518706b "feat: allow ten web searches per response \(#18602\)")  | Sep 28, 2026  |  
|   | Sep 16, 2026  |  
|  [mlx: drop carried metal residency patch now that it is upstream (](https://github.com/ollama/ollama/commit/cf2a313a298066d572c36812e5ad30a21c0db13b "mlx: drop carried metal residency patch now that it is upstream \(#18854\)")[#18854](https://github.com/ollama/ollama/pull/18854)[)](https://github.com/ollama/ollama/commit/cf2a313a298066d572c36812e5ad30a21c0db13b "mlx: drop carried metal residency patch now that it is upstream \(#18854\)")  | Oct 7, 2026  |  
|   |   |  [model: add multimodal embeddings (](https://github.com/ollama/ollama/commit/efe43c5561047d8baf8c862730b2151249d51fd5 "model: add multimodal embeddings \(#18820\)

Implements the EmbeddingGemma2Model architecture on the MLX runner: 24-layer
bidirectional text encoder with PLE, shared gemma4 vision/audio towers,
mean-pool + L2 output. /api/embed accepts per-item media via input dicts.")[#18820](https://github.com/ollama/ollama/pull/18820)[)](https://github.com/ollama/ollama/commit/efe43c5561047d8baf8c862730b2151249d51fd5 "model: add multimodal embeddings \(#18820\)

Implements the EmbeddingGemma2Model architecture on the MLX runner: 24-layer
bidirectional text encoder with PLE, shared gemma4 vision/audio towers,
mean-pool + L2 output. /api/embed accepts per-item media via input dicts.")  | Oct 6, 2026  |  
|   |   |   | Oct 3, 2026  |  
|   |   | [openai: split tool results with images instead of adding [img] markers](https://github.com/ollama/ollama/commit/31547a6cd76ed5eeffbb1f6469a5585e6334f674 "openai: split tool results with images instead of adding \[img\] markers

The \[img\] placeholders mixed internal markers into tool text: the native
llama-server path sends them as literal text, so an image-only result
gained a text part, and tool output containing \[img\] or \[img-N\] moved or
dropped image tags. api.Message cannot carry image positions without
in-band markers, so stop merging tool results that include images.

Text-only tool results, the only array form the Chat Completions spec
allows for tool messages, are still merged into one message \(now with a
strings.Builder\). Results with images keep the per-part split in order,
which preserves image positions, with the tool call id and name set on
every part.")  | Oct 3, 2026  |  
|   |   |  [create: support explicit model capabilities (](https://github.com/ollama/ollama/commit/b3f78b77328bccc4f5af336d16efee69407dbcb1 "create: support explicit model capabilities \(#18708\)

Add CAPABILITY declarations to Modelfiles and an additive capabilities
field to create requests. Preserve declarations across GGUF and safetensors
creation, inheritance, and Modelfile export.

Require decision capability before scheduling System One requests instead
of matching Qwen architecture/renderer metadata. Retain main's GGUF-only
scoring restriction until the separate MLX runtime work lands.

Extracted from the capability foundation in 36d46a0c3 on system_one_mlx;
MLX scoring and manifest-list changes are intentionally separate.")[#18708](https://github.com/ollama/ollama/pull/18708)[)](https://github.com/ollama/ollama/commit/b3f78b77328bccc4f5af336d16efee69407dbcb1 "create: support explicit model capabilities \(#18708\)

Add CAPABILITY declarations to Modelfiles and an additive capabilities
field to create requests. Preserve declarations across GGUF and safetensors
creation, inheritance, and Modelfile export.

Require decision capability before scheduling System One requests instead
of matching Qwen architecture/renderer metadata. Retain main's GGUF-only
scoring restriction until the separate MLX runtime work lands.

Extracted from the capability foundation in 36d46a0c3 on system_one_mlx;
MLX scoring and manifest-list changes are intentionally separate.")  | Sep 29, 2026  |  
|   |   |   | Sep 16, 2026  |  
|   |   |   | Aug 20, 2026  |  
|   |   |   | Sep 1, 2026  |  
|   |   |  [server: skip local model compatibility migration (](https://github.com/ollama/ollama/commit/eab97e9f92b9a25c2d52d2cc6c1b1c99bd9fae21 "server: skip local model compatibility migration \(#18908\)")[#18908](https://github.com/ollama/ollama/pull/18908)[)](https://github.com/ollama/ollama/commit/eab97e9f92b9a25c2d52d2cc6c1b1c99bd9fae21 "server: skip local model compatibility migration \(#18908\)")  | Oct 9, 2026  |  
|   |   |  [create: add server-side MLX imports and drop GGUF conversion (](https://github.com/ollama/ollama/commit/98acec40ae2b3ed361fc5117e5b2ae81a4bf5c18 "create: add server-side MLX imports and drop GGUF conversion \(#14969\)

* create: add server-side MLX imports and drop GGUF conversion

Support safetensors imports through the MLX create pipeline both locally and on the server, including remote upload/staging, draft layer handling, cancellation propagation, transfer limits, and shared manifest/blob writing.

Limit GGUF create to wrapping existing GGUF inputs into Ollama manifests. Remove the in-tree safetensors-to-GGUF converter, server quantization path, and converter-only dependencies so GGUF conversion and quantization stay in llama.cpp tooling.

Keep the MLX path focused on supported safetensors model creation with validation before MLX work, and expose that flow without the --experimental CLI gate.

* address comments

* add client side gguf create fast path

* address comments

* rebase adjustments")[#14969](https://github.com/ollama/ollama/pull/14969)[)](https://github.com/ollama/ollama/commit/98acec40ae2b3ed361fc5117e5b2ae81a4bf5c18 "create: add server-side MLX imports and drop GGUF conversion \(#14969\)

* create: add server-side MLX imports and drop GGUF conversion

Support safetensors imports through the MLX create pipeline both locally and on the server, including remote upload/staging, draft layer handling, cancellation propagation, transfer limits, and shared manifest/blob writing.

Limit GGUF create to wrapping existing GGUF inputs into Ollama manifests. Remove the in-tree safetensors-to-GGUF converter, server quantization path, and converter-only dependencies so GGUF conversion and quantization stay in llama.cpp tooling.

Keep the MLX path focused on supported safetensors model creation with validation before MLX work, and expose that flow without the --experimental CLI gate.

* address comments

* add client side gguf create fast path

* address comments

* rebase adjustments")  | Sep 15, 2026  |  
|   |   |  [create: add server-side MLX imports and drop GGUF conversion (](https://github.com/ollama/ollama/commit/98acec40ae2b3ed361fc5117e5b2ae81a4bf5c18 "create: add server-side MLX imports and drop GGUF conversion \(#14969\)

* create: add server-side MLX imports and drop GGUF conversion

Support safetensors imports through the MLX create pipeline both locally and on the server, including remote upload/staging, draft layer handling, cancellation propagation, transfer limits, and shared manifest/blob writing.

Limit GGUF create to wrapping existing GGUF inputs into Ollama manifests. Remove the in-tree safetensors-to-GGUF converter, server quantization path, and converter-only dependencies so GGUF conversion and quantization stay in llama.cpp tooling.

Keep the MLX path focused on supported safetensors model creation with validation before MLX work, and expose that flow without the --experimental CLI gate.

* address comments

* add client side gguf create fast path

* address comments

* rebase adjustments")[#14969](https://github.com/ollama/ollama/pull/14969)[)](https://github.com/ollama/ollama/commit/98acec40ae2b3ed361fc5117e5b2ae81a4bf5c18 "create: add server-side MLX imports and drop GGUF conversion \(#14969\)

* create: add server-side MLX imports and drop GGUF conversion

Support safetensors imports through the MLX create pipeline both locally and on the server, including remote upload/staging, draft layer handling, cancellation propagation, transfer limits, and shared manifest/blob writing.

Limit GGUF create to wrapping existing GGUF inputs into Ollama manifests. Remove the in-tree safetensors-to-GGUF converter, server quantization path, and converter-only dependencies so GGUF conversion and quantization stay in llama.cpp tooling.

Keep the MLX path focused on supported safetensors model creation with validation before MLX work, and expose that flow without the --experimental CLI gate.

* address comments

* add client side gguf create fast path

* address comments

* rebase adjustments")  | Sep 15, 2026  |  
|   |   |  [tools: ignore braces inside JSON strings when detecting tool call end (](https://github.com/ollama/ollama/commit/32a97b7493786e5784e4445e80c1a078c14c255f "tools: ignore braces inside JSON strings when detecting tool call end \(#16937\)

Parser.done\(\) counted the tag's open/close characters \({}, \[\]\) without
tracking JSON string context, so a streamed tool call whose string
argument value contained a closing brace or bracket \(e.g.
{"code": "if \(x\) { y }"}\) was treated as complete too early and flushed to
the user as plain text instead of being parsed as a tool call.

findArguments\(\) in the same file already tracks string context; apply the
same handling in done\(\) so open/close characters inside string values are
ignored.")[…](https://github.com/ollama/ollama/pull/16937)  | Jun 27, 2026  |  
|   |   | [Merge remote-tracking branch 'upstream/main' into release_v0.40.0](https://github.com/ollama/ollama/commit/47bbe1eebe3a7adf766045c48fd22069ab4a29d6 "Merge remote-tracking branch 'upstream/main' into release_v0.40.0

# Conflicts:
#	server/images.go
#	server/routes.go")  | Sep 28, 2026  |  
|   |   |  [create: support explicit model capabilities (](https://github.com/ollama/ollama/commit/b3f78b77328bccc4f5af336d16efee69407dbcb1 "create: support explicit model capabilities \(#18708\)

Add CAPABILITY declarations to Modelfiles and an additive capabilities
field to create requests. Preserve declarations across GGUF and safetensors
creation, inheritance, and Modelfile export.

Require decision capability before scheduling System One requests instead
of matching Qwen architecture/renderer metadata. Retain main's GGUF-only
scoring restriction until the separate MLX runtime work lands.

Extracted from the capability foundation in 36d46a0c3 on system_one_mlx;
MLX scoring and manifest-list changes are intentionally separate.")[#18708](https://github.com/ollama/ollama/pull/18708)[)](https://github.com/ollama/ollama/commit/b3f78b77328bccc4f5af336d16efee69407dbcb1 "create: support explicit model capabilities \(#18708\)

Add CAPABILITY declarations to Modelfiles and an additive capabilities
field to create requests. Preserve declarations across GGUF and safetensors
creation, inheritance, and Modelfile export.

Require decision capability before scheduling System One requests instead
of matching Qwen architecture/renderer metadata. Retain main's GGUF-only
scoring restriction until the separate MLX runtime work lands.

Extracted from the capability foundation in 36d46a0c3 on system_one_mlx;
MLX scoring and manifest-list changes are intentionally separate.")  | Sep 29, 2026  |  
|   |   |   | Aug 22, 2023  |  
|   |   |   | Jan 29, 2025  |  
|   |   |   | Sep 16, 2026  |  
|   |   | [create: Clean up experimental paths, fix create from existing safeten…](https://github.com/ollama/ollama/commit/30fdd229a434cfae409cc07456684315bf95a561 "create:  Clean up experimental paths, fix create from existing safetensor model \(#14679\)

* create:  Clean up experimental paths

This cleans up the experimental features, and adds both unit and integration test coverage to verify no regressions.

* create: preserve config and layer names when creating from safetensors models

When creating a model FROM an existing safetensors model, ModelFormat,
Capabilities, and layer Name fields were lost. ModelFormat stayed empty
because it's only set from GGML layers \(which safetensors models lack\),
and layer names weren't copied in parseFromModel. This caused derived
models to fail loading \("config.json not found in manifest"\).

* review comments")  | Apr 7, 2026  |  
|   |   | [mlx, mlxrunner: move the MLX engine out of x/](https://github.com/ollama/ollama/commit/2e036e7cdf7baccca93045a2e313b5d8d7730ae9 "mlx, mlxrunner: move the MLX engine out of x/

The MLX runner is the only Go inference runner left and is no longer
experimental, so its packages leave x/. The bindings become a top-level
mlx package beside the carried patches in mlx/compat, mirroring how
llama/ holds the llama.cpp integration, and the runner becomes mlxrunner
with the architectures nested under the package they implement.
Subpackages move with their parent unless listed.

  x/mlxrunner/mlx            mlx
  x/internal/mlxthread       mlx/mlxthread
  x/internal/mlxthreadtest   mlx/mlxthread/mlxthreadtest
  x/internal/mlxtest         mlx/mlxtest
  x/quant                    mlx/quant
  mlx/compat/*.patch         mlx/compat/mlx-c   \(MLX patches go in mlx/compat/mlx\)
  x/mlxrunner                mlxrunner
  x/models/nn                mlxrunner/nn
  x/models/<arch>            mlxrunner/model/<arch>
  x/mlxrunner/imports.go     mlxrunner/model/architectures   \(new package\)
  x/create                   create
  x/safetensors              fs/safetensors
  x/tokenizer                mlxrunner/tokenizer

Every package keeps its name, so the Go changes are the import path
rewrites the moves force, and the CMake, Dockerfile, CI cache keys, drift
check and Darwin payload script follow the new paths. Four edits are not
paths: the runner's blank architecture imports become the package
mlxrunner/model/architectures, so the list to extend for a new model sits
beside the architecture directories; a depguard rule keeps the two test
harnesses out of non-test code, as the x/internal placement used to; the
CI change filter's two entries for the long-deleted x/imagegen/mlx now
name the bindings' CMake project and the carried patches, so a change to
either builds the payload; and the tokenizer parity test reads its
fixtures from its own testdata instead of walking out of x/.

x/server and x/imagegen/manifest stay for the next two commits.")  | Sep 16, 2026  |  
|   |   |  [Add AGENTS.md and CLAUDE.md to root repository (](https://github.com/ollama/ollama/commit/96201a623a62b4434ff434a0a4a6b5b247fce481 "Add AGENTS.md and CLAUDE.md to root repository \(#16604\)")[#16604](https://github.com/ollama/ollama/pull/16604)[)](https://github.com/ollama/ollama/commit/96201a623a62b4434ff434a0a4a6b5b247fce481 "Add AGENTS.md and CLAUDE.md to root repository \(#16604\)")  | Jun 7, 2026  |  
|   |   |  [Add AGENTS.md and CLAUDE.md to root repository (](https://github.com/ollama/ollama/commit/96201a623a62b4434ff434a0a4a6b5b247fce481 "Add AGENTS.md and CLAUDE.md to root repository \(#16604\)")[#16604](https://github.com/ollama/ollama/pull/16604)[)](https://github.com/ollama/ollama/commit/96201a623a62b4434ff434a0a4a6b5b247fce481 "Add AGENTS.md and CLAUDE.md to root repository \(#16604\)")  | Jun 7, 2026  |  
|   |   |  [mlx: fix mac assumptions on linux/windows (](https://github.com/ollama/ollama/commit/4e13421378ca2ac9c879b212980149c71456604f "mlx: fix mac assumptions on linux/windows \(#17898\)

The default packaging was broken due to mac assumptions
leaking into windows")[#17898](https://github.com/ollama/ollama/pull/17898)[)](https://github.com/ollama/ollama/commit/4e13421378ca2ac9c879b212980149c71456604f "mlx: fix mac assumptions on linux/windows \(#17898\)

The default packaging was broken due to mac assumptions
leaking into windows")  | Aug 20, 2026  |  
|   |   | [runner: Remove CGO engines, use llama-server exclusively for GGML mod…](https://github.com/ollama/ollama/commit/9db4bdbad6a4981ad761aa2b603e69e8fb83212c "runner: Remove CGO engines, use llama-server exclusively for GGML models \(#16031\)

* broad lint fixes to sidestep CI scope glitch

* runner: Remove CGO engines, use llama-server exclusively for GGML models

Remove the vendored GGML and llama.cpp backend, CGO runner, Go model
implementations, and sample.  llama-server \(built from upstream llama.cpp via
FetchContent\) is now the sole inference engine for GGUF-based models.
\(Safetensor based models continue to run on the new MLX engine.\)  This allows
us to more rapidly pick up new capabilities and fixes from llama.cpp as they
come out.

On windows this now requires recent AMD driver versions to support ROCm v7 as
llama.cpp currently does not support building against v6.

* llama/compat: load Ollama-format GGUFs in llama-server

Squashed from upstream/jmorganca/llama-compat on 2026-04-29.
Source tip: 0c33775d378511a9b3c7f2e3b80eda355511d9f3.

Original source commits:
- 25223160d llama/compat: add in-memory shim so llama-server can load Ollama-format GGUFs
- 7449b539a llm,server: route Ollama-format gemma3 blobs through llama/compat
- 436f2e2b1 llama/compat: make patch-apply idempotent
- 8c2c9d4c8 llama/compat: extend gemma3 handler to cover 1B and 270M blobs
- 021389f7b llama/compat: shrink clip.cpp injection from 18 lines to 1
- 61b367ec2 llama/compat: shrink patch to pure call-site hooks \(34 -> 20 lines\)
- 36049361c llama/compat: simplify shim \(gemma3-tested\)
- 8fa664865 llama/compat: add qwen35moe text handler
- db0c74530 llama/compat: add qwen35moe vision \(clip\) support
- 2a388da77 llama/compat: split shared infra into a util TU
- 9a69a17dc llama/compat: document non-public API dependencies
- d0f38a915 llama/compat: add gpt-oss and lfm2 handlers
- 086071822 llama/compat: add mistral3 text handler \(vision TODO\)
- 63bde9ff7 llama/compat: add mistral3 vision \(clip\) support
- 3a57b89d5 llama/compat: apply LLaMA RoPE permute to mistral3 vision Q/K
- 99cb87439 llama/compat: add qwen35, gemma4, deepseek-ocr handlers
- 2c7850dba llama/compat: add nemotron_h_moe handler \(latent FFN + MTP skip\)
- 9e3b54225 llama/compat: add llama4 text + clip handlers
- 034fee349 llama/compat: add gemma4 clip handler \(gemma4v projector\)
- 9945c5a93 server: remove dhiltgen/* compat redirect table
- 5d4539101 llama/compat: rewrite gemma4 tokenizer model to BPE
- 7e0765327 llama/compat: add glm-ocr text handler + text-loader load-op hook
- f1bd1a25a llama/compat: add glm-ocr clip handler \(glm4v projector\)
- 4b5cf3420 llama/compat: collapse text-loader hook back to one new patch line
- eb4ecf4fc llama/compat: extend gemma4 clip handler to gemma4a \(audio\)
- a23a5e76f llama/compat: fix gemma4a per-block norm tensor mapping
- cd2dcaff4 llama/compat: add embeddinggemma handler
- 1ce8a6b26 llama/compat: add qwen3-vl + qwen2.5-vl handlers
- fd98ffa1e llama/compat: add gemma3n + glm4moelite handlers
- cc7bdf0bc llama/compat: handle null buft in maybe_load_tensor
- 0c33775d3 llama/compat: disable mmap when load_op transforms text-side tensors

* refine implementation

* ci: fix windows MLX build

* ci: fix windows llama-server build

* ci: fix windows rocm build

* ci: windows mlx tuning

Shorten long-tail on build, and get OllamaSetup.exe back under 2g limit

* ci: fix windows dependencies

* win: fix dependency gathering

* disable openmp

* win: arm64 cross-compile build

also DRY out CI steps

* scheduler improvements

* ci: improvements from #15982

* win: favor ninja for faster developer builds

* win: fix build

* win: fix arm64 cross-compile

* win: avoid spaces in compiler path

* misc discovery fixes, and bos handling

* lint fixes

* win: fix arm cross-compile build/CI bugs

* llama.cpp update

* win: handle multiple CRT dirs

* vulkan: add windows iGPU detection

* fix creation bugs for patched models, other refactoring work

* tune batch size for better performance

* ci and lint fixes

* fix repeat_last_n bug

* build: revamp build for better developer UX

* amd, sampler, qwen3next fixes

* version bump

* fix mlx build

* revamp GPU discovery

Scanning the output of llama-server is turning out to be too error prone across
llama.cpp updates, so this switches to a thin dynamic library load against the
bundled GGML libraries so more details can be gathered from the API.

* version bump

* missing file

* ci: fix cache miss on rocm build

* refine vulkan dep handling

* fix ps reporting bug on full GPU load

* improve cmake wiring for customized local builds

* version bump

* docker build arg cleanup

* improve windows exit error logs

* fix community gemma4 support and ci flakes

* fix mlx unit test

* tighten up ps logic to avoid double counting fit log lines

* version bump

* fix ps view for full gpu layer offload

* add MTP wiring for llama-server and create with GGUFs

* pick best template by capabilities

* version bump

* ci: harden apt repos

* remove unused cpu core discovery

* adjust batch default logic to reduce OOMs

* support larger tool calls

* fix audio support, template show

* qwen35 mtp patch support

* flesh out dtypes

* rocm deps

* version bump

* lint fix

* block broken gfx1150 on windows

* fix qwen3.5 moe mtp tensors in patch

* mmproj oom fallback and vulkan on by default

* qwen MTP compat fix

* version bump

* ci: fix WoA cross-compile

* ci: workaround ui tool in cross-compile

* version bump

* win: enable OpenMP for CPU builds

* build: improve developer UX

* ci: windows path workaround for CPU build

* win: fix WoA dependencies

* win: fix large offset reads for mmproj patched loads

* version bump

* fix vulkan dup detection

* add OLLAMA_IGPU_ENABLE and largely disable iGPUs by default

* opt-in MTP, win large offset, integraton fixes

* fix unit test scheduler interaction hang

* fix multi-gpu filtering

* version bump

* review comments

* fix thinking level

* fix linux rocm ordering and granite 3.3 template

* version bump

* ci fix - non-shallow MLX checkout

* bypass linux sysfs unit test on windows

---------

Co-authored-by: jmorganca <jmorganca@gmail.com>")  | May 29, 2026  |  
|   |   |  [docs: fix typos in repository documentation (](https://github.com/ollama/ollama/commit/dd0ed0ef172cdc270ef062ac764a58780c5c8093 "docs: fix typos in repository documentation \(#10683\)")[#10683](https://github.com/ollama/ollama/pull/10683)[)](https://github.com/ollama/ollama/commit/dd0ed0ef172cdc270ef062ac764a58780c5c8093 "docs: fix typos in repository documentation \(#10683\)")  | Nov 16, 2025  |  
|   |   |  [mlx: mitigate high latency after GPU idle (](https://github.com/ollama/ollama/commit/cf6c9de620caac8c3a1167ed476ec4773617ce40 "mlx: mitigate high latency after GPU idle \(#18807\)

Carry the MLX residency-refresh patch and enable it every second in the
runner, preserving explicit environment overrides.

Fixes #18744.")[#18807](https://github.com/ollama/ollama/pull/18807)[)](https://github.com/ollama/ollama/commit/cf6c9de620caac8c3a1167ed476ec4773617ce40 "mlx: mitigate high latency after GPU idle \(#18807\)

Carry the MLX residency-refresh patch and enable it every second in the
runner, preserving explicit environment overrides.

Fixes #18744.")  | Oct 5, 2026  |  
|   |   | [`proto` -> `ollama`](https://github.com/ollama/ollama/commit/df5fdd6647e17a546e4bc66d8730541408cdf8a5 "`proto` -> `ollama`")  | Jun 26, 2023  |  
|   |   |  [llama.cpp: version update (](https://github.com/ollama/ollama/commit/ec318f4d110cb828d1b47f7b247216f6d8608809 "llama.cpp: version update \(#18761\)")[#18761](https://github.com/ollama/ollama/pull/18761)[)](https://github.com/ollama/ollama/commit/ec318f4d110cb828d1b47f7b247216f6d8608809 "llama.cpp: version update \(#18761\)")  | Oct 6, 2026  |  
|   |   |   | Sep 15, 2026  |  
|   |   |  [mlx: drop carried metal residency patch now that it is upstream (](https://github.com/ollama/ollama/commit/cf2a313a298066d572c36812e5ad30a21c0db13b "mlx: drop carried metal residency patch now that it is upstream \(#18854\)")[#18854](https://github.com/ollama/ollama/pull/18854)[)](https://github.com/ollama/ollama/commit/cf2a313a298066d572c36812e5ad30a21c0db13b "mlx: drop carried metal residency patch now that it is upstream \(#18854\)")  | Oct 7, 2026  |  
|   |   |  [Merge pull request](https://github.com/ollama/ollama/commit/e3cddc3e897d8414a60a46e23f5ef3a99be2eb81 "Merge pull request #18739 from maziluiosif/add-oxi

README: add oxi to community integrations") [#18739](https://github.com/ollama/ollama/pull/18739) [from maziluiosif/add-oxi](https://github.com/ollama/ollama/commit/e3cddc3e897d8414a60a46e23f5ef3a99be2eb81 "Merge pull request #18739 from maziluiosif/add-oxi

README: add oxi to community integrations")  | Oct 7, 2026  |  
|   |   |  [docs: fix typos in repository documentation (](https://github.com/ollama/ollama/commit/dd0ed0ef172cdc270ef062ac764a58780c5c8093 "docs: fix typos in repository documentation \(#10683\)")[#10683](https://github.com/ollama/ollama/pull/10683)[)](https://github.com/ollama/ollama/commit/dd0ed0ef172cdc270ef062ac764a58780c5c8093 "docs: fix typos in repository documentation \(#10683\)")  | Nov 16, 2025  |  
|   |   | [llama-server: prepare to remove compatibility patch](https://github.com/ollama/ollama/commit/75b952780f90807f651eb2f1f817e5a40126e81d "llama-server: prepare to remove compatibility patch

Add manifest-list storage so runner-specific manifests can coexist under one tag while preserving existing v1 tags as best-effort downgrade anchors. Show/list/copy/remove/pull/push now understand runner and digest selection and transfer referenced child manifests and layers.

Add lazy local compatibility migration for legacy Ollama GGUFs into llama.cpp-compatible children, covering the patched model families and preserving parser/renderer, templates, projectors, media metadata, split GGUFs, and MTP/draft tensors where applicable.

Extend create/import/conversion paths for the same compatibility rules, harden manifest-list combine validation, and add focused manifest, migration, transfer, and integration coverage for first-load conversion plus chat/tools/vision/audio/embedding behavior.")  | Sep 25, 2026  |  
|   |   | [llama-server: prepare to remove compatibility patch](https://github.com/ollama/ollama/commit/75b952780f90807f651eb2f1f817e5a40126e81d "llama-server: prepare to remove compatibility patch

Add manifest-list storage so runner-specific manifests can coexist under one tag while preserving existing v1 tags as best-effort downgrade anchors. Show/list/copy/remove/pull/push now understand runner and digest selection and transfer referenced child manifests and layers.

Add lazy local compatibility migration for legacy Ollama GGUFs into llama.cpp-compatible children, covering the patched model families and preserving parser/renderer, templates, projectors, media metadata, split GGUFs, and MTP/draft tensors where applicable.

Extend create/import/conversion paths for the same compatibility rules, harden manifest-list combine validation, and add focused manifest, migration, transfer, and integration coverage for first-load conversion plus chat/tools/vision/audio/embedding behavior.")  | Sep 25, 2026  |  
|   |   | Aug 2, 2024  |  
| View all files |  
## Repository files navigation
# Ollama
Start building with open models.
## Download
### macOS

```
curl -fsSL https://ollama.com/install.sh | sh
```

or [download manually](https://ollama.com/download/Ollama.dmg)
### Windows

```
irm https://ollama.com/install.ps1 | iex
```

or [download manually](https://ollama.com/download/OllamaSetup.exe)
### Linux

```
curl -fsSL https://ollama.com/install.sh | sh
```

[Manual install instructions](https://docs.ollama.com/linux#manual-install)
### Docker
The official [Ollama Docker image](https://hub.docker.com/r/ollama/ollama) `ollama/ollama` is available on Docker Hub.
### Libraries


### Community


## Get started

```
ollama

```

You'll be prompted to run a model or connect Ollama to your existing agents or applications such as `Claude Code`, `OpenClaw`, `OpenCode` , `Codex`, `Copilot`, and more.
### Coding
To launch a specific integration:

```
ollama launch claude

```

Supported integrations include [Claude Code](https://docs.ollama.com/integrations/claude-code), [Codex](https://docs.ollama.com/integrations/codex), [Copilot CLI](https://docs.ollama.com/integrations/copilot-cli), [DeepSeek Harness](https://docs.ollama.com/integrations/deepseek-harness), [Droid](https://docs.ollama.com/integrations/droid), and [OpenCode](https://docs.ollama.com/integrations/opencode).
### AI assistant
Use [OpenClaw](https://docs.ollama.com/integrations/openclaw) to turn Ollama into a personal AI assistant across WhatsApp, Telegram, Slack, Discord, and more:

```
ollama launch openclaw

```

### Chat with a model
Run and chat with [Gemma 4](https://ollama.com/library/gemma4):

```
ollama run gemma4

```

See [ollama.com/library](https://ollama.com/library) for the full list.
See the [quickstart guide](https://docs.ollama.com/quickstart) for more details.
## REST API
Ollama has a REST API for running and managing models.

```
curl http://localhost:11434/api/chat -d '{
  "model": "gemma4",
  "messages": [{
    "role": "user",
    "content": "Why is the sky blue?"
  }],
  "stream": false
}'

```

See the [API documentation](https://docs.ollama.com/api) for all endpoints.
### Python

```
pip install ollama

```


```
from ollama import chat

response = chat(model='gemma4', messages=[
  {
    'role': 'user',
    'content': 'Why is the sky blue?',
  },
])
print(response.message.content)
```

### JavaScript

```
npm i ollama

```


```
import ollama from "ollama";

const response = await ollama.chat({
  model: "gemma4",
  messages: [{ role: "user", content: "Why is the sky blue?" }],
});
console.log(response.message.content);
```

## Supported backends
  * [llama.cpp](https://github.com/ggml-org/llama.cpp) project founded by Georgi Gerganov.


## Documentation
  * [REST API reference](https://docs.ollama.com/api)
  * [Importing models](https://docs.ollama.com/import)
  * [Modelfile reference](https://docs.ollama.com/modelfile)
  * [Building from source](https://github.com/ollama/ollama/blob/main/docs/development.md)


## Community Integrations
> Want to add your project? Open a pull request.
### Chat Interfaces
  * [Open WebUI](https://github.com/open-webui/open-webui) - Extensible, self-hosted AI interface
  * [Onyx](https://github.com/onyx-dot-app/onyx) - Connected AI workspace
  * [LibreChat](https://github.com/danny-avila/LibreChat) - Enhanced ChatGPT clone with multi-provider support
  * [Lobe Chat](https://github.com/lobehub/lobe-chat) - Modern chat framework with plugin ecosystem ([docs](https://lobehub.com/docs/self-hosting/examples/ollama))
  * [NextChat](https://github.com/ChatGPTNextWeb/ChatGPT-Next-Web) - Cross-platform ChatGPT UI ([docs](https://docs.nextchat.dev/models/ollama))
  * [Perplexica](https://github.com/ItzCrazyKns/Perplexica) - AI-powered search engine, open-source Perplexity alternative
  * [big-AGI](https://github.com/enricoros/big-AGI) - AI suite for professionals
  * [Lollms WebUI](https://github.com/ParisNeo/lollms-webui) - Multi-model web interface
  * [ChatOllama](https://github.com/sugarforever/chat-ollama) - Chatbot with knowledge bases
  * [Bionic GPT](https://github.com/bionic-gpt/bionic-gpt) - On-premise AI platform
  * [Chatbot UI](https://github.com/ivanfioravanti/chatbot-ollama) - ChatGPT-style web interface
  * [Hollama](https://github.com/fmaclen/hollama) - Minimal web interface
  * [Chatbox](https://github.com/Bin-Huang/Chatbox) - Desktop and web AI client
  * [chat](https://github.com/swuecho/chat) - Chat web app for teams
  * [Ollama RAG Chatbot](https://github.com/datvodinh/rag-chatbot.git) - Chat with multiple PDFs using RAG
  * [Tkinter-based client](https://github.com/chyok/ollama-gui) - Python desktop client


#### Desktop
  * [Dify.AI](https://github.com/langgenius/dify) - LLM app development platform
  * [AnythingLLM](https://github.com/Mintplex-Labs/anything-llm) - All-in-one AI app for Mac, Windows, and Linux
  * [Maid](https://github.com/Mobile-Artificial-Intelligence/maid) - Cross-platform mobile and desktop client
  * [Witsy](https://github.com/nbonamy/witsy) - AI desktop app for Mac, Windows, and Linux
  * [Cherry Studio](https://github.com/kangfenmao/cherry-studio) - Multi-provider desktop client
  * [Ollama App](https://github.com/JHubi1/ollama-app) - Multi-platform client for desktop and mobile
  * [PyGPT](https://github.com/szczyglis-dev/py-gpt) - AI desktop assistant for Linux, Windows, and Mac
  * [Alpaca](https://github.com/Jeffser/Alpaca) - GTK4 client for Linux and macOS
  * [SwiftChat](https://github.com/aws-samples/swift-chat) - Cross-platform including iOS, Android, and Apple Vision Pro
  * [Enchanted](https://github.com/AugustDev/enchanted) - Native macOS and iOS client
  * [RWKV-Runner](https://github.com/josStorer/RWKV-Runner) - Multi-model desktop runner
  * [Ollama Grid Search](https://github.com/dezoito/ollama-grid-search) - Evaluate and compare models
  * [macai](https://github.com/Renset/macai) - macOS client for Ollama and ChatGPT
  * [AI Studio](https://github.com/MindWorkAI/AI-Studio) - Multi-provider desktop IDE
  * [Reins](https://github.com/ibrahimcetin/reins) - Parameter tuning and reasoning model support
  * [ConfiChat](https://github.com/1runeberg/confichat) - Privacy-focused with optional encryption
  * [LLocal.in](https://github.com/kartikm7/llocal) - Electron desktop client
  * [MindMac](https://mindmac.app) - AI chat client for Mac
  * [Msty](https://msty.app) - Multi-model desktop client
  * [BoltAI for Mac](https://boltai.com) - AI chat client for Mac
  * [IntelliBar](https://intellibar.app/) - AI-powered assistant for macOS
  * [Kerlig AI](https://www.kerlig.com/) - AI writing assistant for macOS
  * [Hillnote](https://hillnote.com) - Markdown-first AI workspace
  * [Perfect Memory AI](https://www.perfectmemory.ai/) - Productivity AI personalized by screen and meeting history


#### Mobile
  * [Ollama Android Chat](https://github.com/sunshine0523/OllamaServer) - One-click Ollama on Android


> SwiftChat, Enchanted, Maid, Ollama App, Reins, and ConfiChat listed above also support mobile platforms.
### Code Editors & Development
  * [Cline](https://github.com/cline/cline) - VS Code extension for multi-file/whole-repo coding
  * [Continue](https://github.com/continuedev/continue) - Open-source AI code assistant for any IDE
  * [Void](https://github.com/voideditor/void) - Open source AI code editor, Cursor alternative
  * [Copilot for Obsidian](https://github.com/logancyang/obsidian-copilot) - AI assistant for Obsidian
  * [twinny](https://github.com/rjmacarthy/twinny) - Copilot and Copilot chat alternative
  * [gptel Emacs client](https://github.com/karthink/gptel) - LLM client for Emacs
  * [Ollama Copilot](https://github.com/bernardo-bruning/ollama-copilot) - Use Ollama as GitHub Copilot
  * [Obsidian Local GPT](https://github.com/pfrankov/obsidian-local-gpt) - Local AI for Obsidian
  * [Ellama Emacs client](https://github.com/s-kostyaev/ellama) - LLM tool for Emacs
  * [orbiton](https://github.com/xyproto/orbiton) - Config-free text editor with Ollama tab completion
  * [AI ST Completion](https://github.com/yaroslavyaroslav/OpenAI-sublime-text) - Sublime Text 4 AI assistant
  * [VT Code](https://github.com/vinhnx/vtcode) - Rust-based terminal coding agent with Tree-sitter
  * [QodeAssist](https://github.com/Palm1r/QodeAssist) - AI coding assistant for Qt Creator
  * [AI Toolkit for VS Code](https://aka.ms/ai-tooklit/ollama-docs) - Microsoft-official VS Code extension
  * [Open Interpreter](https://docs.openinterpreter.com/language-model-setup/local-models/ollama) - Natural language interface for computers
  * [oxi](https://github.com/maziluiosif/oxi) - Native Rust desktop coding agent with editor, Git panel and terminal


### Libraries & SDKs
  * [LiteLLM](https://github.com/BerriAI/litellm) - Unified API for 100+ LLM providers
  * [Semantic Kernel](https://github.com/microsoft/semantic-kernel/tree/main/python/semantic_kernel/connectors/ai/ollama) - Microsoft AI orchestration SDK
  * [LangChain4j](https://github.com/langchain4j/langchain4j) - Java LangChain ([example](https://github.com/langchain4j/langchain4j-examples/tree/main/ollama-examples/src/main/java))
  * [LangChainGo](https://github.com/tmc/langchaingo/) - Go LangChain ([example](https://github.com/tmc/langchaingo/tree/main/examples/ollama-completion-example))
  * [Spring AI](https://github.com/spring-projects/spring-ai) - Spring framework AI support ([docs](https://docs.spring.io/spring-ai/reference/api/chat/ollama-chat.html))
  * [LangChain](https://python.langchain.com/docs/integrations/chat/ollama/) and [LangChain.js](https://js.langchain.com/docs/integrations/chat/ollama/) with [example](https://js.langchain.com/docs/tutorials/local_rag/)
  * [Ollama for Ruby](https://github.com/crmne/ruby_llm) - Ruby LLM library
  * [any-llm](https://github.com/mozilla-ai/any-llm) - Unified LLM interface by Mozilla
  * [OllamaSharp for .NET](https://github.com/awaescher/OllamaSharp) - .NET SDK
  * [LangChainRust](https://github.com/Abraxas-365/langchain-rust) - Rust LangChain ([example](https://github.com/Abraxas-365/langchain-rust/blob/main/examples/llm_ollama.rs))
  * [Agents-Flex for Java](https://github.com/agents-flex/agents-flex) - Java agent framework ([example](https://github.com/agents-flex/agents-flex/tree/main/agents-flex-chat/agents-flex-chat-ollama))
  * [Elixir LangChain](https://github.com/brainlid/langchain) - Elixir LangChain
  * [Ollama-rs for Rust](https://github.com/pepperoni21/ollama-rs) - Rust SDK
  * [LangChain for .NET](https://github.com/tryAGI/LangChain) - .NET LangChain ([example](https://github.com/tryAGI/LangChain/blob/main/examples/LangChain.Samples.OpenAI/Program.cs))
  * [chromem-go](https://github.com/philippgille/chromem-go) - Go vector database with Ollama embeddings ([example](https://github.com/philippgille/chromem-go/tree/v0.5.0/examples/rag-wikipedia-ollama))
  * [LangChainDart](https://github.com/davidmigloz/langchain_dart) - Dart LangChain
  * [LlmTornado](https://github.com/lofcz/llmtornado) - Unified C# interface for multiple inference APIs
  * [Ollama4j for Java](https://github.com/ollama4j/ollama4j) - Java SDK
  * [Ollama for Laravel](https://github.com/cloudstudio/ollama-laravel) - Laravel integration
  * [Ollama for Swift](https://github.com/mattt/ollama-swift) - Swift SDK
  * [LlamaIndex](https://docs.llamaindex.ai/en/stable/examples/llm/ollama/) and [LlamaIndexTS](https://developers.llamaindex.ai/typescript/framework/modules/models/llms/ollama/) - Data framework for LLM apps
  * [Haystack](https://github.com/deepset-ai/haystack-integrations/blob/main/integrations/ollama.md) - AI pipeline framework
  * [Firebase Genkit](https://firebase.google.com/docs/genkit/plugins/ollama) - Google AI framework
  * [Ollama-hpp for C++](https://github.com/jmont-dev/ollama-hpp) - C++ SDK
  * [PromptingTools.jl](https://github.com/svilupp/PromptingTools.jl) - Julia LLM toolkit ([example](https://svilupp.github.io/PromptingTools.jl/dev/examples/working_with_ollama))
  * [Ollama for R - rollama](https://github.com/JBGruber/rollama) - R SDK
  * [Portkey](https://portkey.ai/docs/welcome/integration-guides/ollama) - AI gateway
  * [Testcontainers](https://testcontainers.com/modules/ollama/) - Container-based testing
  * [LLPhant](https://github.com/theodo-group/LLPhant?tab=readme-ov-file#ollama) - PHP AI framework


### Frameworks & Agents
  * [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT/blob/master/docs/platform/ollama.md) - Autonomous AI agent platform
  * [crewAI](https://github.com/crewAIInc/crewAI) - Multi-agent orchestration framework
  * [Strands Agents](https://github.com/strands-agents/sdk-python) - Model-driven agent building by AWS
  * [Cheshire Cat](https://github.com/cheshire-cat-ai/core) - AI assistant framework
  * [any-agent](https://github.com/mozilla-ai/any-agent) - Unified agent framework interface by Mozilla
  * [Stakpak](https://github.com/stakpak/agent) - Open source DevOps agent
  * [Hexabot](https://github.com/hexastack/hexabot) - Conversational AI builder
  * [Neuro SAN](https://github.com/cognizant-ai-lab/neuro-san-studio) - Multi-agent orchestration ([docs](https://github.com/cognizant-ai-lab/neuro-san-studio/blob/main/docs/user_guide.md#ollama))


### RAG & Knowledge Bases
  * [RAGFlow](https://github.com/infiniflow/ragflow) - RAG engine based on deep document understanding
  * [R2R](https://github.com/SciPhi-AI/R2R) - Open-source RAG engine
  * [MaxKB](https://github.com/1Panel-dev/MaxKB/) - Ready-to-use RAG chatbot
  * [Minima](https://github.com/dmayboroda/minima) - On-premises or fully local RAG
  * [Chipper](https://github.com/TilmanGriesel/chipper) - AI interface with Haystack RAG
  * [ARGO](https://github.com/xark-argo/argo) - RAG and deep research on Mac/Windows/Linux
  * [Archyve](https://github.com/nickthecook/archyve) - RAG-enabling document library
  * [Casibase](https://casibase.org) - AI knowledge base with RAG and SSO
  * [BrainSoup](https://www.nurgo-software.com/products/brainsoup) - Native client with RAG and multi-agent automation


### Bots & Messaging
  * [LangBot](https://github.com/RockChinQ/LangBot) - Multi-platform messaging bots with agents and RAG
  * [AstrBot](https://github.com/AstrBotDevs/AstrBot/) - Multi-platform chatbot with RAG and plugins
  * [Discord-Ollama Chat Bot](https://github.com/kevinthedang/discord-ollama) - TypeScript Discord bot
  * [Ollama Telegram Bot](https://github.com/ruecat/ollama-telegram) - Telegram bot
  * [LLM Telegram Bot](https://github.com/innightwolfsleep/llm_telegram_bot) - Telegram bot for roleplay


### Terminal & CLI
  * [aichat](https://github.com/sigoden/aichat) - All-in-one LLM CLI with Shell Assistant, RAG, and AI tools
  * [oterm](https://github.com/ggozad/oterm) - Terminal client for Ollama
  * [gollama](https://github.com/sammcj/gollama) - Go-based model manager for Ollama
  * [tlm](https://github.com/yusufcanb/tlm) - Local shell copilot
  * [tenere](https://github.com/pythops/tenere) - TUI for LLMs
  * [ParLlama](https://github.com/paulrobello/parllama) - TUI for Ollama
  * [llm-ollama](https://github.com/taketwo/llm-ollama) - Plugin for [Datasette's LLM CLI](https://llm.datasette.io/en/stable/)
  * [ShellOracle](https://github.com/djcopley/ShellOracle) - Shell command suggestions
  * [LLM-X](https://github.com/mrdjohnson/llm-x) - Progressive web app for LLMs
  * [cmdh](https://github.com/pgibler/cmdh) - Natural language to shell commands
  * [VT](https://github.com/vinhnx/vt.ai) - Minimal multimodal AI chat app


### Productivity & Apps
  * [AppFlowy](https://github.com/AppFlowy-IO/AppFlowy) - AI collaborative workspace, self-hostable Notion alternative
  * [Screenpipe](https://github.com/mediar-ai/screenpipe) - 24/7 screen and mic recording with AI-powered search
  * [Vibe](https://github.com/thewh1teagle/vibe) - Transcribe and analyze meetings
  * [Page Assist](https://github.com/n4ze3m/page-assist) - Chrome extension for AI-powered browsing
  * [NativeMind](https://github.com/NativeMindBrowser/NativeMindExtension) - Private, on-device browser AI assistant
  * [Ollama Fortress](https://github.com/ParisNeo/ollama_proxy_server) - Security proxy for Ollama
  * [1Panel](https://github.com/1Panel-dev/1Panel/) - Web-based Linux server management
  * [Writeopia](https://github.com/Writeopia/Writeopia) - Text editor with Ollama integration
  * [QA-Pilot](https://github.com/reid41/QA-Pilot) - GitHub code repository understanding
  * [Raycast extension](https://github.com/MassimilianoPasquini97/raycast_ollama) - Ollama in Raycast
  * [Painting Droid](https://github.com/mateuszmigas/painting-droid) - Painting app with AI integrations
  * [Serene Pub](https://github.com/doolijb/serene-pub) - AI roleplaying app
  * [Mayan EDMS](https://gitlab.com/mayan-edms/mayan-edms) - Document management with Ollama workflows
  * [TagSpaces](https://www.tagspaces.org) - File management with [AI tagging](https://docs.tagspaces.org/ai/)


### Observability & Monitoring
  * [Opik](https://www.comet.com/docs/opik/cookbook/ollama) - Debug, evaluate, and monitor LLM applications
  * [OpenLIT](https://github.com/openlit/openlit) - OpenTelemetry-native monitoring for Ollama and GPUs
  * [Lunary](https://lunary.ai/docs/integrations/ollama) - LLM observability with analytics and PII masking
  * [Langfuse](https://langfuse.com/docs/integrations/ollama) - Open source LLM observability
  * [HoneyHive](https://docs.honeyhive.ai/integrations/ollama) - AI observability and evaluation for agents
  * [MLflow Tracing](https://mlflow.org/docs/latest/llms/tracing/index.html#automatic-tracing) - Open source LLM observability


### Database & Embeddings
  * [pgai](https://github.com/timescale/pgai) - PostgreSQL as a vector database ([guide](https://github.com/timescale/pgai/blob/main/docs/vectorizer-quick-start.md))
  * [MindsDB](https://docs.mindsdb.com/integrations/ai-engines/ollama) - Connect Ollama with 200+ data platforms
  * [chromem-go](https://github.com/philippgille/chromem-go/blob/v0.5.0/embed_ollama.go) - Embeddable vector database for Go ([example](https://github.com/philippgille/chromem-go/tree/v0.5.0/examples/rag-wikipedia-ollama))
  * [Kangaroo](https://github.com/dbkangaroo/kangaroo) - AI-powered SQL client


### Infrastructure & Deployment
#### Cloud
  * [Harbor](https://github.com/av/harbor) - Containerized LLM toolkit with Ollama as default backend


#### Package Managers


## About
Get up and running with Kimi, GLM, MiniMax, DeepSeek, gpt-oss, Qwen, Gemma and other models.
### Topics
[deepseek](https://github.com/topics/deepseek)[gemma](https://github.com/topics/gemma)[gemma3](https://github.com/topics/gemma3)[glm](https://github.com/topics/glm)[go](https://github.com/topics/go)[golang](https://github.com/topics/golang)[gpt-oss](https://github.com/topics/gpt-oss)[llama](https://github.com/topics/llama)[llama3](https://github.com/topics/llama3)[llm](https://github.com/topics/llm)[llms](https://github.com/topics/llms)[minimax](https://github.com/topics/minimax)[mistral](https://github.com/topics/mistral)[ollama](https://github.com/topics/ollama)[qwen](https://github.com/topics/qwen)
### Resources
### Contributing
### Security policy
### Stars
**182.5k** stars
### Watchers
**1.0k** watching
### Forks
[**18.2k** forks](https://github.com/ollama/ollama/forks)
## 
[ v0.40.2Latest Oct 8, 2026 ](https://github.com/ollama/ollama/releases/tag/v0.40.2)
## 
## Languages
  * [Go71%](https://github.com/ollama/ollama/search?l=go)
  * [C20.2%](https://github.com/ollama/ollama/search?l=c)
  * [TypeScript5.2%](https://github.com/ollama/ollama/search?l=typescript)
  * [C++1%](https://github.com/ollama/ollama/search?l=c%2B%2B)
  * [Objective-C0.8%](https://github.com/ollama/ollama/search?l=objective-c)
  * [CMake0.7%](https://github.com/ollama/ollama/search?l=cmake)
  * [Other1.1%](https://github.com/ollama/ollama/search?l=Other)


You can’t perform that action at this time. 
