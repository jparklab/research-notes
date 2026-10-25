---
title: 'GitHub - EleutherAI/lm-evaluation-harness: A framework for few-shot evaluation
  of language models. · GitHub'
id: github-eleutherailm-evaluation-harness-a-framework-for-few-shot-evaluation-of-la
tags:
- evaluation
- llm-evaluation
- benchmark
created: '2026-10-09T23:15:37.711876Z'
updated: '2026-10-10T14:52:17.245535Z'
source: https://github.com/EleutherAI/lm-evaluation-harness
source_domain: github.com
fetched_at: '2026-10-09T23:15:37.687623Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'EleutherAI lm-evaluation-harness GitHub README: unified framework for offline
  few-shot benchmark scoring of LMs — over 60 standard academic benchmarks with hundreds
  of subtask variants. Designed for offline/batch scoring, not CI deployment gating:
  no CI/CD or PR-blocking framing anywhere in the README (contrast with promptfoo).
  Supports model backends via optional extras: HuggingFace transformers (incl. GPTQ/AutoGPTQ
  quantization, PEFT/LoRA adapters), GPT-NeoX, Megatron-DeepSpeed, and explicitly
  vLLM (''fast and memory-efficient inference''), plus commercial APIs (OpenAI, TextSynth).
  It is the backend for Hugging Face''s Open LLM Leaderboard and is cited as used
  internally by NVIDIA, Cohere, BigScience, BigCode, Nous Research, and Mosaic ML
  — named production/research adopters. 14.2k stars, 55 watchers. License confirmed
  MIT via direct fetch of LICENSE.md (Copyright 2020 EleutherAI) — the GitHub tree
  crawl itself did not render the license sidebar.'
---

[Skip to content](https://github.com/EleutherAI/lm-evaluation-harness#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/EleutherAI/lm-evaluation-harness) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/EleutherAI/lm-evaluation-harness) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/EleutherAI/lm-evaluation-harness) to refresh your session. Dismiss alert
[**125** Branches](https://github.com/EleutherAI/lm-evaluation-harness/branches)[](https://github.com/EleutherAI/lm-evaluation-harness/tags)
Go to file
Open more actions menu
## Latest commit
[fix(basqueglue): load the f1 metric with evaluate (](https://github.com/EleutherAI/lm-evaluation-harness/commit/d6de81643928d653435c431bae19945d41d32520)[#4076](https://github.com/EleutherAI/lm-evaluation-harness/pull/4076)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/d6de81643928d653435c431bae19945d41d32520)
Open commit detailsfailure
Sep 14, 2026
[d6de816](https://github.com/EleutherAI/lm-evaluation-harness/commit/d6de81643928d653435c431bae19945d41d32520) · Sep 14, 2026
## History
[4,122 Commits](https://github.com/EleutherAI/lm-evaluation-harness/commits/main/)
Open commit details
4,122 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| [.github/workflows](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/.github/workflows "This path skips through empty directories")  | [.github/workflows](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/.github/workflows "This path skips through empty directories")  | [feat(models): add cross-platform onnxruntime-genai backend + refactor…](https://github.com/EleutherAI/lm-evaluation-harness/commit/b0195a94f4fe98c10acc043255081506b5d453f8 "feat\(models\): add cross-platform onnxruntime-genai backend + refactor winml \(#3960\)

* feat\(models\): add cross-platform onnxruntime-genai backend + refactor winml

Add a cross-platform `onnxruntime-genai` backend that runs ONNX LLMs exported
by the ONNX Runtime GenAI Model Builder on CPU/CUDA/DirectML/WebGPU/AMD-NPU via
the `og.Config` provider API. All lm-eval logic \(single-pass log-likelihood,
multiple-choice, rolling perplexity, generation\) lives in
`ONNXRuntimeGenAILM\(TemplateLM\)`, following the `OptimumLM\(HFLM\)` single-file
subclass precedent.

Refactor `winml` into a thin `WindowsML\(ONNXRuntimeGenAILM\)` subclass that
overrides only execution-provider selection, inheriting the shared scoring
core. This removes the previous winml quality debt: the per-token
teacher-forcing loop \(now a single `get_output\("logits"\)` pass\), the direct
`loglikelihood` override that bypassed `TemplateLM` token handling, and the
silent `\(0.0, False\)`-on-exception path that corrupted scores.

Add a shared parametrized test suite \(unit tests for the scoring math with a
fake `_forward_logits`, plus CPU smoke tests over hellaswag/wikitext/gsm8k
using a tiny Model Builder fixture\), packaging extra, and docs. The test is
added to the CI `--ignore` list since the ONNX runtimes are not installed in CI.

Part of QUARK-606 Phase 1. The raw `onnxruntime` backend, cross-backend parity
CI, OpenAI-compatible server, and Quark migration are follow-up stages.

Co-Authored-By: Claude <noreply@anthropic.com>

* docs: keep model_guide subclassing tip generic

Trim the subclassing note to the pre-existing OptimumLM example instead of
documenting the onnxruntime-genai backend internals, which belong in the
backend's own docstrings/README rather than the general model-authoring guide.

Co-Authored-By: Claude <noreply@anthropic.com>

* test: split winml tests into their own module

Move winml-specific coverage \(registration, subclass relationship, non-Windows
CPU fallback\) into tests/models/test_winml.py, leaving test_onnxruntime_genai.py
focused on the genai backend and its scoring math. Keeps each backend's tests
self-contained and adds the new file to the CI ignore list.

Co-Authored-By: Claude <noreply@anthropic.com>

* fix\(onnxruntime-genai\): honor genai_config.json providers by default

execution_provider now defaults to None, meaning "use the providers the export
declares in genai_config.json" — og.Config\(model_dir\) seeds its provider list
from that file, and _select_ep no longer calls clear_providers\(\) unless the
user explicitly asks for a provider. Previously the "cpu" default cleared the
list and appended nothing, silently downgrading a CUDA/DML/NPU export to CPU.
Pass execution_provider=cpu to force CPU, or a provider name to override.

winml pins its historical max_length/max_gen_toks \(4096\) so existing runs are
numerically unchanged, while the cross-platform backend keeps model-aware
defaults \(context_length from the config; max_gen_toks=256\).

Add regression tests for the three _select_ep branches and winml's defaults.

Co-Authored-By: Claude <noreply@anthropic.com>

* pacify pre-commmit

---------

Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: Baber Abbasi <baber@hey.com>")  | Aug 21, 2026  |  
|  [Feat/plugin filter metric registration (](https://github.com/EleutherAI/lm-evaluation-harness/commit/b954108c9baaaa934b4ad842033b31a97ee30816 "Feat/plugin filter metric registration \(#4014\)

* feat\(registry\): runtime plugin registration for model backends

Add a discovery layer so external packages can contribute model backends to
lm-eval without editing its source, and have them behave exactly like upstream
backends. The Registry class already supported EntryPoint placeholders, but
nothing ever scanned installed distributions for them.

Two mechanisms:
- Entry points: packages declaring an `lm_eval.models` entry point are
  discovered automatically \(lazy, zero-config\) when a model name is resolved.
- Explicit import: `--plugins mod` / `LM_EVAL_PLUGINS` env / `simple_evaluate\(plugins=...\)`
  import named modules so their `@register_*` decorators run, for local or
  unpublished components.

Also fixes Registry collision handling to upgrade an EntryPoint placeholder to
its decorated class \(the natural case where a plugin declares both an entry
point and `@register_model`\), guarding EntryPoint.__eq__ which raises on
mismatched types.

Co-Authored-By: Claude <noreply@anthropic.com>

* feat\(registry\): plugin discovery for filters, metrics, and aggregations

Extend the entry-point plugin mechanism \(added for models\) to the other
Registry-backed components. External packages can now contribute:
- filters via the `lm_eval.filters` entry-point group
- metrics via `lm_eval.metrics`
- aggregations via `lm_eval.aggregations`

get_filter/get_metric/get_aggregation discover installed entry points lazily.
Because @register_metric populates the higher_is_better and metric-aggregation
registries as a decorator side effect, is_higher_better and get_metric_aggregation
now force discovery + materialization of the named metric so plugin metrics carry
their metadata even when those getters are called standalone.

Generalize the Registry placeholder→concrete upgrade to any object with
module/name \(functions, not just classes\), so metric/aggregation plugins that
declare both an entry point and a decorator upgrade cleanly instead of colliding.

Co-Authored-By: Claude <noreply@anthropic.com>

* remove plugins from `simple_evaluate`

* fix tests

* fix docs

* add to news

---------

Co-authored-by: Baber Abbasi <baber@hey.com>")[#4014](https://github.com/EleutherAI/lm-evaluation-harness/pull/4014)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/b954108c9baaaa934b4ad842033b31a97ee30816 "Feat/plugin filter metric registration \(#4014\)

* feat\(registry\): runtime plugin registration for model backends

Add a discovery layer so external packages can contribute model backends to
lm-eval without editing its source, and have them behave exactly like upstream
backends. The Registry class already supported EntryPoint placeholders, but
nothing ever scanned installed distributions for them.

Two mechanisms:
- Entry points: packages declaring an `lm_eval.models` entry point are
  discovered automatically \(lazy, zero-config\) when a model name is resolved.
- Explicit import: `--plugins mod` / `LM_EVAL_PLUGINS` env / `simple_evaluate\(plugins=...\)`
  import named modules so their `@register_*` decorators run, for local or
  unpublished components.

Also fixes Registry collision handling to upgrade an EntryPoint placeholder to
its decorated class \(the natural case where a plugin declares both an entry
point and `@register_model`\), guarding EntryPoint.__eq__ which raises on
mismatched types.

Co-Authored-By: Claude <noreply@anthropic.com>

* feat\(registry\): plugin discovery for filters, metrics, and aggregations

Extend the entry-point plugin mechanism \(added for models\) to the other
Registry-backed components. External packages can now contribute:
- filters via the `lm_eval.filters` entry-point group
- metrics via `lm_eval.metrics`
- aggregations via `lm_eval.aggregations`

get_filter/get_metric/get_aggregation discover installed entry points lazily.
Because @register_metric populates the higher_is_better and metric-aggregation
registries as a decorator side effect, is_higher_better and get_metric_aggregation
now force discovery + materialization of the named metric so plugin metrics carry
their metadata even when those getters are called standalone.

Generalize the Registry placeholder→concrete upgrade to any object with
module/name \(functions, not just classes\), so metric/aggregation plugins that
declare both an entry point and a decorator upgrade cleanly instead of colliding.

Co-Authored-By: Claude <noreply@anthropic.com>

* remove plugins from `simple_evaluate`

* fix tests

* fix docs

* add to news

---------

Co-authored-by: Baber Abbasi <baber@hey.com>")  | Sep 1, 2026  |  
|   |   |  [fix: super_glue is moved to aps/super_glue (](https://github.com/EleutherAI/lm-evaluation-harness/commit/868f8a2f2b774f73869264c050415e4d83dc7b9b "fix: super_glue is moved to aps/super_glue \(#3523\)")[#3523](https://github.com/EleutherAI/lm-evaluation-harness/pull/3523)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/868f8a2f2b774f73869264c050415e4d83dc7b9b "fix: super_glue is moved to aps/super_glue \(#3523\)")  | Jan 27, 2026  |  
|   |   |  [fix(basqueglue): load the f1 metric with evaluate (](https://github.com/EleutherAI/lm-evaluation-harness/commit/d6de81643928d653435c431bae19945d41d32520 "fix\(basqueglue\): load the f1 metric with evaluate \(#4076\)

datasets.load_metric was removed in datasets 3.0 and the unit-test job
resolves datasets==5.0.1, so the module-level

    from datasets import load_metric

makes lm_eval/tasks/basqueglue/utils.py unimportable. Five of the six
BasqueGLUE tasks resolve a !function from that module, so they now fail to
load at all, not just at scoring time:

    bec.yaml    bec2016eu        utils.micro_f1_score
    bhtc.yaml   bhtc_v2          utils.micro_f1_score
    coref.yaml  epec_koref_bin   utils.coref_doc_to_text
    vaxx.yaml   vaxx_stance      utils.vaxx_f1_score
    wic.yaml    wiceu            utils.process_wic_docs

Load the metric with evaluate instead, the same move #2351 made for
squadv2, with the import inside the function as that fix and
kobest/utils.py do. The metric and the compute\(\) arguments are unchanged.")[#4076](https://github.com/EleutherAI/lm-evaluation-harness/pull/4076)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/d6de81643928d653435c431bae19945d41d32520 "fix\(basqueglue\): load the f1 metric with evaluate \(#4076\)

datasets.load_metric was removed in datasets 3.0 and the unit-test job
resolves datasets==5.0.1, so the module-level

    from datasets import load_metric

makes lm_eval/tasks/basqueglue/utils.py unimportable. Five of the six
BasqueGLUE tasks resolve a !function from that module, so they now fail to
load at all, not just at scoring time:

    bec.yaml    bec2016eu        utils.micro_f1_score
    bhtc.yaml   bhtc_v2          utils.micro_f1_score
    coref.yaml  epec_koref_bin   utils.coref_doc_to_text
    vaxx.yaml   vaxx_stance      utils.vaxx_f1_score
    wic.yaml    wiceu            utils.process_wic_docs

Load the metric with evaluate instead, the same move #2351 made for
squadv2, with the import inside the function as that fix and
kobest/utils.py do. The metric and the compute\(\) arguments are unchanged.")  | Sep 14, 2026  |  
|   |   | [fix(scripts): repair requests_caching.py entrypoint (bad kwargs + hel…](https://github.com/EleutherAI/lm-evaluation-harness/commit/d1193e39a2de5fe37a46b0f0459b555031f4507a "fix\(scripts\): repair requests_caching.py entrypoint \(bad kwargs + helper defined after use\) \(#4059\)

The `__main__` block passed `model=` and `device=` to
`run_model_for_task_caching\(\)`, whose signature is
`\(tasks, cache_requests\)` -- both are read from module constants inside
the function, so the call raised
`TypeError: run_model_for_task_caching\(\) got an unexpected keyword
argument 'model'` before anything ran.

With that fixed the script then failed with
`NameError: name 'request_caching_arg_to_dict' is not defined`,
because that helper was defined *below* the `__main__` block and so was
not yet bound when the block executed.

Drop the two extra keyword arguments and move the helper above the
`__main__` block, next to its only caller.")  | Aug 28, 2026  |  
| [templates/new_yaml_task](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/templates/new_yaml_task "This path skips through empty directories")  | [templates/new_yaml_task](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/templates/new_yaml_task "This path skips through empty directories")  |   | Mar 20, 2025  |  
|   |   |  [fix(bbh): guard find_match against an all-empty capture tuple (](https://github.com/EleutherAI/lm-evaluation-harness/commit/e54687eac08501af43f09cc84af40ea2fc3cad60 "fix\(bbh\): guard find_match against an all-empty capture tuple \(#4125\)

* fix\(bbh\): guard find_match against an all-empty capture tuple

`ExtendedRegexFilter.find_match` narrowed a capture tuple with
`\[m for m in match if m\]\[0\]`, which raises IndexError when every group in
the tuple is empty. A regex whose groups are all optional and whose matched
text lies outside them -- e.g. `x\(foo\)?\(bar\)?` against "x" -- makes
`findall` return `\[\("", ""\)\]`, and the run aborts instead of falling
through to the filter's fallback.

`lm_eval/filters/extraction.py` already guards the same narrowing in
`MultiChoiceRegexFilter.find_match`; these two files are copies that
predate that fix. Adopt the upstream shape verbatim so an all-empty tuple
is treated as a non-match.

No shipped bbh task can currently reach this: every configured
`regex_pattern` has a single capture group, and the two-group regex
`MapRegexFilter` builds by joining its `regex_pattern_to_value` keys always
fills exactly one group when it matches. The crash is reachable from a
user-supplied filter config, so this is a latent fix -- no task's score
changes, and no `metadata.version` bump is warranted.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* chore\(bbh\): fix pre-existing ruff violations blocking Linters CI

The Linters job runs pre-commit with `--from-ref`/`--to-ref`, which scopes
to changed *files* rather than changed lines. Touching these two files
therefore surfaces every existing violation in them, so the preceding fix
cannot go green without clearing these first. Same situation, and same
remedy, as #3999.

Twelve violations, six mirrored in each file:

- `mutable-argument-default` \(x4\) -- `convert_dict={}` on `find_match` and
  `regex_pattern_to_value={}` on `MapRegexFilter.__init__`. These are not
  unrelated cleanup: `find_match` is the function the preceding commit
  fixes, and `lm_eval/filters/extraction.py` already carries exactly this
  change \(`convert_dict: dict\[str, str\] | None = None` plus an in-body
  None check\). Adopting it closes the rest of the same divergence.

- `empty-docstring` \(x2\) -- a vestigial `""" """` on `WordSortFilter`.
  Removed rather than filled; `D10` \(undocumented-public-*\) is ignored
  repo-wide, so no docstring is the expected state.

- `zip-without-explicit-strict` \(x6\) -- made explicit with `strict=False`,
  which is the pre-3.10 default and so preserves behaviour exactly.
  `extraction.py` uses `strict=True` for the corresponding
  `zip\(resps, docs\)`, but adopting that here would change behaviour on a
  length mismatch \(silent truncation becomes a raised error\). That is a
  real question and deserves its own change, not a smuggled one inside a
  lint commit.

No behaviour changes. Both files remain byte-identical to each other.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* nits

---------

Co-authored-by: Claude Opus 5 <noreply@anthropic.com>
Co-authored-by: Baber Abbasi <baber@hey.com>")[#4125](https://github.com/EleutherAI/lm-evaluation-harness/pull/4125)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/e54687eac08501af43f09cc84af40ea2fc3cad60 "fix\(bbh\): guard find_match against an all-empty capture tuple \(#4125\)

* fix\(bbh\): guard find_match against an all-empty capture tuple

`ExtendedRegexFilter.find_match` narrowed a capture tuple with
`\[m for m in match if m\]\[0\]`, which raises IndexError when every group in
the tuple is empty. A regex whose groups are all optional and whose matched
text lies outside them -- e.g. `x\(foo\)?\(bar\)?` against "x" -- makes
`findall` return `\[\("", ""\)\]`, and the run aborts instead of falling
through to the filter's fallback.

`lm_eval/filters/extraction.py` already guards the same narrowing in
`MultiChoiceRegexFilter.find_match`; these two files are copies that
predate that fix. Adopt the upstream shape verbatim so an all-empty tuple
is treated as a non-match.

No shipped bbh task can currently reach this: every configured
`regex_pattern` has a single capture group, and the two-group regex
`MapRegexFilter` builds by joining its `regex_pattern_to_value` keys always
fills exactly one group when it matches. The crash is reachable from a
user-supplied filter config, so this is a latent fix -- no task's score
changes, and no `metadata.version` bump is warranted.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* chore\(bbh\): fix pre-existing ruff violations blocking Linters CI

The Linters job runs pre-commit with `--from-ref`/`--to-ref`, which scopes
to changed *files* rather than changed lines. Touching these two files
therefore surfaces every existing violation in them, so the preceding fix
cannot go green without clearing these first. Same situation, and same
remedy, as #3999.

Twelve violations, six mirrored in each file:

- `mutable-argument-default` \(x4\) -- `convert_dict={}` on `find_match` and
  `regex_pattern_to_value={}` on `MapRegexFilter.__init__`. These are not
  unrelated cleanup: `find_match` is the function the preceding commit
  fixes, and `lm_eval/filters/extraction.py` already carries exactly this
  change \(`convert_dict: dict\[str, str\] | None = None` plus an in-body
  None check\). Adopting it closes the rest of the same divergence.

- `empty-docstring` \(x2\) -- a vestigial `""" """` on `WordSortFilter`.
  Removed rather than filled; `D10` \(undocumented-public-*\) is ignored
  repo-wide, so no docstring is the expected state.

- `zip-without-explicit-strict` \(x6\) -- made explicit with `strict=False`,
  which is the pre-3.10 default and so preserves behaviour exactly.
  `extraction.py` uses `strict=True` for the corresponding
  `zip\(resps, docs\)`, but adopting that here would change behaviour on a
  length mismatch \(silent truncation becomes a raised error\). That is a
  real question and deserves its own change, not a smuggled one inside a
  lint commit.

No behaviour changes. Both files remain byte-identical to each other.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* nits

---------

Co-authored-by: Claude Opus 5 <noreply@anthropic.com>
Co-authored-by: Baber Abbasi <baber@hey.com>")  | Sep 14, 2026  |  
|   |   |  [feat(cli): allow key=value to be passed to](https://github.com/EleutherAI/lm-evaluation-harness/commit/02a1125e537f2357d93ae84d18972c9c547346bd "feat\(cli\): allow key=value to be passed to `--metadata`; linting \(#4054\)

* feat\(cli\): allow key=value to be passed to `--metadata`; linting

* add to .gitignore") [; linting (](https://github.com/EleutherAI/lm-evaluation-harness/commit/02a1125e537f2357d93ae84d18972c9c547346bd "feat\(cli\): allow key=value to be passed to `--metadata`; linting \(#4054\)

* feat\(cli\): allow key=value to be passed to `--metadata`; linting

* add to .gitignore")[#4054](https://github.com/EleutherAI/lm-evaluation-harness/pull/4054)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/02a1125e537f2357d93ae84d18972c9c547346bd "feat\(cli\): allow key=value to be passed to `--metadata`; linting \(#4054\)

* feat\(cli\): allow key=value to be passed to `--metadata`; linting

* add to .gitignore")  | Aug 27, 2026  |  
|   |   |  [chore: update pre-commit hooks and workflow packages (](https://github.com/EleutherAI/lm-evaluation-harness/commit/cc9b6c2a2a5c22da435aaa276fb6c3a0cab82b0e "chore: update pre-commit hooks and workflow packages \(#4023\)")[#4023](https://github.com/EleutherAI/lm-evaluation-harness/pull/4023)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/cc9b6c2a2a5c22da435aaa276fb6c3a0cab82b0e "chore: update pre-commit hooks and workflow packages \(#4023\)")  | Aug 20, 2026  |  
|   |   |   | Jan 15, 2024  |  
|   |   |   | Apr 30, 2026  |  
|   |   |   | Dec 24, 2020  |  
|   |   |  [Include all test files in sdist (](https://github.com/EleutherAI/lm-evaluation-harness/commit/cff3da29a7a9eaecb3d75309d924444710717fba "Include all test files in sdist \(#2634\)

This is useful to run unit tests during distro builds.")[#2634](https://github.com/EleutherAI/lm-evaluation-harness/pull/2634)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/cff3da29a7a9eaecb3d75309d924444710717fba "Include all test files in sdist \(#2634\)

This is useful to run unit tests during distro builds.")  | May 6, 2025  |  
|   |   |  [gguf: support modern llama.cpp logprobs format (](https://github.com/EleutherAI/lm-evaluation-harness/commit/ad8737ae7fad24cf64e50fc7fc31397bff586b9e "gguf: support modern llama.cpp logprobs format \(fixes #1437\) \(#4012\)

* gguf: fix loglikelihood scoring with exact teacher forcing

This commit combines the following four commits by @xms991, originally
from https://github.com/xms991/lm-evaluation-harness, squashed into one.
All implementation credit goes to @xms991.

gguf: support modern llama.cpp logprobs format via grammar-forced loglikelihood

llama.cpp's /v1/completions endpoint returns logprobs in the modern
OpenAI format \(logprobs.content, see llama.cpp PR #10783\) and silently
ignores 'echo', so the legacy token_logprobs/echo-based scoring no
longer works. Score continuations by forcing them with a GBNF grammar
instead: llama.cpp reports pre-sampling probabilities for
grammar-constrained tokens, which are exactly the teacher-forced
loglikelihoods needed.

Also:
- add 'model' model-arg for llama-server router mode \(fixes 400s\)
- accept base_url with or without /v1 or /v1/completions suffix
- handle empty continuations and llama.cpp's trailing empty-token entry
- honor max_gen_toks in generate_until
- add request timeout; return \(-inf, False\) instead of asserting on
  invalid responses
- document llama.cpp server usage and requirements in README

Fixes EleutherAI/lm-evaluation-harness#1437

gguf: issue requests concurrently with a thread pool

Add a 'parallel' model arg controlling how many requests are in flight
at once. By default the degree of parallelism is auto-detected from the
server's /props endpoint \(total_slots, i.e. llama-server's --parallel
setting\), falling back to serial requests when /props is unavailable.
Result order is preserved regardless of completion order.

Also derive and keep the server root URL from base_url so /props can be
queried.

gguf: pin loglikelihood requests to server slots by context group

llama.cpp keeps the KV cache per slot, so parallel requests sharing a
prompt prefix were each recomputing it on different slots. Use the
documented id_slot request parameter to pin consecutive same-context
loglikelihood requests \(e.g. the candidate continuations of one
multiple-choice question\) to the same slot, round-robing context groups
across slots. This restores prefix-cache reuse in parallel mode and
makes results match serial runs.

generate_until is deliberately left unpinned: generation lengths vary
widely and the server's dynamic idle-slot assignment load-balances
measurably better \(~30% faster on gsm8k without pinning\).

Benchmarks \(Qwen3.6-35B-A3B-Q8_0, 4 server slots\):
- hellaswag limit=40: 67.7s serial -> 59.4s, metrics identical to serial
- gsm8k limit=16: 51.1s serial -> 28.4s

gguf: fix loglikelihood scoring with exact teacher forcing

The grammar-forcing approach was subtly but seriously wrong: llama.cpp's
grammar-constrained decoding can select a fragmented tokenization of the
continuation \(e.g. forcing "'t" as "'" + "t"\), and the resulting
logprobs are for that token path, not the natural tokenization. This
systematically deflated loglikelihoods \(mean -32 nats vs the HF backend
on winogrande/Qwen3-0.6B\) and cost ~9 points of accuracy \(0.525 vs
0.615\).

New method: tokenize context and continuation separately via the
server's /tokenize endpoint \(same convention as the HF backend\), then
score each continuation token individually with its natural prefix as a
token-id prompt and a logit_bias that forces the server to sample it.
llama.cpp reports the forced token's pre-sampling logprob and an
unbiased top_logprobs list, giving exact teacher-forced loglikelihoods.

Validated against the HF backend on identical weights \(Qwen3-0.6B,
winogrande limit=200\): mean per-token loglikelihood diff -0.011 nats
\(stdev 0.26, i.e. quantization noise\), 189/200 prediction agreement,
acc 0.610 vs 0.615. Per-position requests to the same slot also chain
off llama.cpp's prompt cache, so scoring stays fast \(15s for 200 docs
on the 0.6B\).

* fix tokenization: match HF backend's encode-together-then-split and whitespace migration

Baber identified that the HF backend \(model.py:391-400\) does two things
the gguf model was missing:

1. Migrates trailing whitespace from context into continuation before
   tokenizing \(n_spaces logic\).
2. Encodes context+continuation as one string, then splits at
   len\(tok\(context\)\) — BPE-correct: avoids boundary mismatches when
   the join would merge tokens.

Applied both, and pinned the convention in tests/models/test_gguf.py
\(encode-together assertions + a dedicated whitespace-migration test\).

Verified against live llama-server \(Qwen3.5 2B\):
  gsm8k: 60% exact match \(generation\)
  mmlu_abstract_algebra: 20% acc \(loglikelihood\)
Validated vs HF backend on gemma-4-E2B \(bf16\): tinyMMLU and
tinyHellaswag loglikelihood decisions match 100%.

Tested-by: dotVansh

* gguf: default generate_until to max_gen_toks=256

Match the harness contract: every other backend caps generation at 256
tokens when the task doesn't set one. The gguf backend sent
max_tokens=None, letting llama.cpp generate unbounded, so docs that
never emit a stop string \(e.g. repetition loops\) produced different
text — and different filtered answers — than the HF backend.

Resolution now goes through the shared normalize_gen_kwargs helper, so
max_gen_toks, max_new_tokens, max_tokens and max_completion_tokens are
all honored \(with the usual priority order\) and CLI string values are
int\(\)-converted; the default is configurable via a max_gen_toks
model-arg. This also fixes the local-completions-style case where
'--gen_kwargs max_tokens=...' was silently ignored \(cf. #2081\).

Validated on tinyGSM8k \(gemma-4-E2B bf16, 9070 XT\): scores now match
the HF backend to within cross-backend numerics \(8/100 doc flips,
~0.5 IRT points\) instead of diverging on uncapped generations.

* lint

---------

Co-authored-by: xms991 <xms991@users.noreply.github.com>
Co-authored-by: dotVansh <dotVansh@users.noreply.github.com>
Co-authored-by: Baber Abbasi <baber@hey.com>") [#1437](https://github.com/EleutherAI/lm-evaluation-harness/issues/1437)[) (](https://github.com/EleutherAI/lm-evaluation-harness/commit/ad8737ae7fad24cf64e50fc7fc31397bff586b9e "gguf: support modern llama.cpp logprobs format \(fixes #1437\) \(#4012\)

* gguf: fix loglikelihood scoring with exact teacher forcing

This commit combines the following four commits by @xms991, originally
from https://github.com/xms991/lm-evaluation-harness, squashed into one.
All implementation credit goes to @xms991.

gguf: support modern llama.cpp logprobs format via grammar-forced loglikelihood

llama.cpp's /v1/completions endpoint returns logprobs in the modern
OpenAI format \(logprobs.content, see llama.cpp PR #10783\) and silently
ignores 'echo', so the legacy token_logprobs/echo-based scoring no
longer works. Score continuations by forcing them with a GBNF grammar
instead: llama.cpp reports pre-sampling probabilities for
grammar-constrained tokens, which are exactly the teacher-forced
loglikelihoods needed.

Also:
- add 'model' model-arg for llama-server router mode \(fixes 400s\)
- accept base_url with or without /v1 or /v1/completions suffix
- handle empty continuations and llama.cpp's trailing empty-token entry
- honor max_gen_toks in generate_until
- add request timeout; return \(-inf, False\) instead of asserting on
  invalid responses
- document llama.cpp server usage and requirements in README

Fixes EleutherAI/lm-evaluation-harness#1437

gguf: issue requests concurrently with a thread pool

Add a 'parallel' model arg controlling how many requests are in flight
at once. By default the degree of parallelism is auto-detected from the
server's /props endpoint \(total_slots, i.e. llama-server's --parallel
setting\), falling back to serial requests when /props is unavailable.
Result order is preserved regardless of completion order.

Also derive and keep the server root URL from base_url so /props can be
queried.

gguf: pin loglikelihood requests to server slots by context group

llama.cpp keeps the KV cache per slot, so parallel requests sharing a
prompt prefix were each recomputing it on different slots. Use the
documented id_slot request parameter to pin consecutive same-context
loglikelihood requests \(e.g. the candidate continuations of one
multiple-choice question\) to the same slot, round-robing context groups
across slots. This restores prefix-cache reuse in parallel mode and
makes results match serial runs.

generate_until is deliberately left unpinned: generation lengths vary
widely and the server's dynamic idle-slot assignment load-balances
measurably better \(~30% faster on gsm8k without pinning\).

Benchmarks \(Qwen3.6-35B-A3B-Q8_0, 4 server slots\):
- hellaswag limit=40: 67.7s serial -> 59.4s, metrics identical to serial
- gsm8k limit=16: 51.1s serial -> 28.4s

gguf: fix loglikelihood scoring with exact teacher forcing

The grammar-forcing approach was subtly but seriously wrong: llama.cpp's
grammar-constrained decoding can select a fragmented tokenization of the
continuation \(e.g. forcing "'t" as "'" + "t"\), and the resulting
logprobs are for that token path, not the natural tokenization. This
systematically deflated loglikelihoods \(mean -32 nats vs the HF backend
on winogrande/Qwen3-0.6B\) and cost ~9 points of accuracy \(0.525 vs
0.615\).

New method: tokenize context and continuation separately via the
server's /tokenize endpoint \(same convention as the HF backend\), then
score each continuation token individually with its natural prefix as a
token-id prompt and a logit_bias that forces the server to sample it.
llama.cpp reports the forced token's pre-sampling logprob and an
unbiased top_logprobs list, giving exact teacher-forced loglikelihoods.

Validated against the HF backend on identical weights \(Qwen3-0.6B,
winogrande limit=200\): mean per-token loglikelihood diff -0.011 nats
\(stdev 0.26, i.e. quantization noise\), 189/200 prediction agreement,
acc 0.610 vs 0.615. Per-position requests to the same slot also chain
off llama.cpp's prompt cache, so scoring stays fast \(15s for 200 docs
on the 0.6B\).

* fix tokenization: match HF backend's encode-together-then-split and whitespace migration

Baber identified that the HF backend \(model.py:391-400\) does two things
the gguf model was missing:

1. Migrates trailing whitespace from context into continuation before
   tokenizing \(n_spaces logic\).
2. Encodes context+continuation as one string, then splits at
   len\(tok\(context\)\) — BPE-correct: avoids boundary mismatches when
   the join would merge tokens.

Applied both, and pinned the convention in tests/models/test_gguf.py
\(encode-together assertions + a dedicated whitespace-migration test\).

Verified against live llama-server \(Qwen3.5 2B\):
  gsm8k: 60% exact match \(generation\)
  mmlu_abstract_algebra: 20% acc \(loglikelihood\)
Validated vs HF backend on gemma-4-E2B \(bf16\): tinyMMLU and
tinyHellaswag loglikelihood decisions match 100%.

Tested-by: dotVansh

* gguf: default generate_until to max_gen_toks=256

Match the harness contract: every other backend caps generation at 256
tokens when the task doesn't set one. The gguf backend sent
max_tokens=None, letting llama.cpp generate unbounded, so docs that
never emit a stop string \(e.g. repetition loops\) produced different
text — and different filtered answers — than the HF backend.

Resolution now goes through the shared normalize_gen_kwargs helper, so
max_gen_toks, max_new_tokens, max_tokens and max_completion_tokens are
all honored \(with the usual priority order\) and CLI string values are
int\(\)-converted; the default is configurable via a max_gen_toks
model-arg. This also fixes the local-completions-style case where
'--gen_kwargs max_tokens=...' was silently ignored \(cf. #2081\).

Validated on tinyGSM8k \(gemma-4-E2B bf16, 9070 XT\): scores now match
the HF backend to within cross-backend numerics \(8/100 doc flips,
~0.5 IRT points\) instead of diverging on uncapped generations.

* lint

---------

Co-authored-by: xms991 <xms991@users.noreply.github.com>
Co-authored-by: dotVansh <dotVansh@users.noreply.github.com>
Co-authored-by: Baber Abbasi <baber@hey.com>")[#4012](https://github.com/EleutherAI/lm-evaluation-harness/pull/4012)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/ad8737ae7fad24cf64e50fc7fc31397bff586b9e "gguf: support modern llama.cpp logprobs format \(fixes #1437\) \(#4012\)

* gguf: fix loglikelihood scoring with exact teacher forcing

This commit combines the following four commits by @xms991, originally
from https://github.com/xms991/lm-evaluation-harness, squashed into one.
All implementation credit goes to @xms991.

gguf: support modern llama.cpp logprobs format via grammar-forced loglikelihood

llama.cpp's /v1/completions endpoint returns logprobs in the modern
OpenAI format \(logprobs.content, see llama.cpp PR #10783\) and silently
ignores 'echo', so the legacy token_logprobs/echo-based scoring no
longer works. Score continuations by forcing them with a GBNF grammar
instead: llama.cpp reports pre-sampling probabilities for
grammar-constrained tokens, which are exactly the teacher-forced
loglikelihoods needed.

Also:
- add 'model' model-arg for llama-server router mode \(fixes 400s\)
- accept base_url with or without /v1 or /v1/completions suffix
- handle empty continuations and llama.cpp's trailing empty-token entry
- honor max_gen_toks in generate_until
- add request timeout; return \(-inf, False\) instead of asserting on
  invalid responses
- document llama.cpp server usage and requirements in README

Fixes EleutherAI/lm-evaluation-harness#1437

gguf: issue requests concurrently with a thread pool

Add a 'parallel' model arg controlling how many requests are in flight
at once. By default the degree of parallelism is auto-detected from the
server's /props endpoint \(total_slots, i.e. llama-server's --parallel
setting\), falling back to serial requests when /props is unavailable.
Result order is preserved regardless of completion order.

Also derive and keep the server root URL from base_url so /props can be
queried.

gguf: pin loglikelihood requests to server slots by context group

llama.cpp keeps the KV cache per slot, so parallel requests sharing a
prompt prefix were each recomputing it on different slots. Use the
documented id_slot request parameter to pin consecutive same-context
loglikelihood requests \(e.g. the candidate continuations of one
multiple-choice question\) to the same slot, round-robing context groups
across slots. This restores prefix-cache reuse in parallel mode and
makes results match serial runs.

generate_until is deliberately left unpinned: generation lengths vary
widely and the server's dynamic idle-slot assignment load-balances
measurably better \(~30% faster on gsm8k without pinning\).

Benchmarks \(Qwen3.6-35B-A3B-Q8_0, 4 server slots\):
- hellaswag limit=40: 67.7s serial -> 59.4s, metrics identical to serial
- gsm8k limit=16: 51.1s serial -> 28.4s

gguf: fix loglikelihood scoring with exact teacher forcing

The grammar-forcing approach was subtly but seriously wrong: llama.cpp's
grammar-constrained decoding can select a fragmented tokenization of the
continuation \(e.g. forcing "'t" as "'" + "t"\), and the resulting
logprobs are for that token path, not the natural tokenization. This
systematically deflated loglikelihoods \(mean -32 nats vs the HF backend
on winogrande/Qwen3-0.6B\) and cost ~9 points of accuracy \(0.525 vs
0.615\).

New method: tokenize context and continuation separately via the
server's /tokenize endpoint \(same convention as the HF backend\), then
score each continuation token individually with its natural prefix as a
token-id prompt and a logit_bias that forces the server to sample it.
llama.cpp reports the forced token's pre-sampling logprob and an
unbiased top_logprobs list, giving exact teacher-forced loglikelihoods.

Validated against the HF backend on identical weights \(Qwen3-0.6B,
winogrande limit=200\): mean per-token loglikelihood diff -0.011 nats
\(stdev 0.26, i.e. quantization noise\), 189/200 prediction agreement,
acc 0.610 vs 0.615. Per-position requests to the same slot also chain
off llama.cpp's prompt cache, so scoring stays fast \(15s for 200 docs
on the 0.6B\).

* fix tokenization: match HF backend's encode-together-then-split and whitespace migration

Baber identified that the HF backend \(model.py:391-400\) does two things
the gguf model was missing:

1. Migrates trailing whitespace from context into continuation before
   tokenizing \(n_spaces logic\).
2. Encodes context+continuation as one string, then splits at
   len\(tok\(context\)\) — BPE-correct: avoids boundary mismatches when
   the join would merge tokens.

Applied both, and pinned the convention in tests/models/test_gguf.py
\(encode-together assertions + a dedicated whitespace-migration test\).

Verified against live llama-server \(Qwen3.5 2B\):
  gsm8k: 60% exact match \(generation\)
  mmlu_abstract_algebra: 20% acc \(loglikelihood\)
Validated vs HF backend on gemma-4-E2B \(bf16\): tinyMMLU and
tinyHellaswag loglikelihood decisions match 100%.

Tested-by: dotVansh

* gguf: default generate_until to max_gen_toks=256

Match the harness contract: every other backend caps generation at 256
tokens when the task doesn't set one. The gguf backend sent
max_tokens=None, letting llama.cpp generate unbounded, so docs that
never emit a stop string \(e.g. repetition loops\) produced different
text — and different filtered answers — than the HF backend.

Resolution now goes through the shared normalize_gen_kwargs helper, so
max_gen_toks, max_new_tokens, max_tokens and max_completion_tokens are
all honored \(with the usual priority order\) and CLI string values are
int\(\)-converted; the default is configurable via a max_gen_toks
model-arg. This also fixes the local-completions-style case where
'--gen_kwargs max_tokens=...' was silently ignored \(cf. #2081\).

Validated on tinyGSM8k \(gemma-4-E2B bf16, 9070 XT\): scores now match
the HF backend to within cross-backend numerics \(8/100 doc flips,
~0.5 IRT points\) instead of diverging on uncapped generations.

* lint

---------

Co-authored-by: xms991 <xms991@users.noreply.github.com>
Co-authored-by: dotVansh <dotVansh@users.noreply.github.com>
Co-authored-by: Baber Abbasi <baber@hey.com>")  | Sep 10, 2026  |  
|   |   |   | Nov 3, 2023  |  
|   |   |   | May 3, 2022  |  
|   |   |  [chore(version): 0.4.14.dev0 (](https://github.com/EleutherAI/lm-evaluation-harness/commit/889c656bddc5b1d5b53381d68a1ac0b5bedfec78 "chore\(version\): 0.4.14.dev0 \(#4072\)")[#4072](https://github.com/EleutherAI/lm-evaluation-harness/pull/4072)[)](https://github.com/EleutherAI/lm-evaluation-harness/commit/889c656bddc5b1d5b53381d68a1ac0b5bedfec78 "chore\(version\): 0.4.14.dev0 \(#4072\)")  | Aug 31, 2026  |  
| View all files |  
## Repository files navigation
# Language Model Evaluation Harness
## Latest News 📣
  * [2026/09] **Plugins** : model backends, filters, metrics and aggregations can now be registered from your own package instead of forking — declare an `lm_eval.*` entry point for zero-config discovery, or point `--plugins` at a local module. See the [Plugin Guide](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/plugins.md).
  * [2025/12] **CLI refactored** with subcommands (`run`, `ls`, `validate`) and YAML config file support via `--config`. See the [CLI Reference](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/interface.md) and [Configuration Guide](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/config_files.md).
  * [2025/12] **Lighter install** : Base package no longer includes `transformers`/`torch`. Install model backends separately: `pip install lm_eval[hf]`, `lm_eval[vllm]`, etc.
  * [2025/07] Added `think_end_token` arg to `hf` (token/str), `vllm` and `sglang` (str) for stripping CoT reasoning traces from models that support it.
  * [2025/03] Added support for steering HF models!
  * [2025/02] Added [SGLang](https://docs.sglang.ai/) support!
  * [2024/09] We are prototyping allowing users of LM Evaluation Harness to create and evaluate on text+image multimodal input, text output tasks, and have just added the `hf-multimodal` and `vllm-vlm` model types and `mmmu` task as a prototype feature. We welcome users to try out this in-progress feature and stress-test it for themselves, and suggest they check out [`lmms-eval`](https://github.com/EvolvingLMMs-Lab/lmms-eval), a wonderful project originally forking off of the lm-evaluation-harness, for a broader range of multimodal tasks, models, and features.
  * [2024/07] [API model](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/API_guide.md) support has been updated and refactored, introducing support for batched and async requests, and making it significantly easier to customize and use for your own purposes. **To run Llama 405B, we recommend using VLLM's OpenAI-compliant API to host the model, and use the`local-completions` model type to evaluate the model.**
  * [2024/07] New Open LLM Leaderboard tasks have been added ! You can find them under the [leaderboard](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/lm_eval/tasks/leaderboard/README.md) task group.


## Announcement
**A new v0.4.0 release of lm-evaluation-harness is available** !
New updates and features include:
  * **New Open LLM Leaderboard tasks have been added ! You can find them under the[leaderboard](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/lm_eval/tasks/leaderboard/README.md) task group.**
  * Internal refactoring
  * Config-based task creation and configuration
  * Easier import and sharing of externally-defined task config YAMLs
  * Support for Jinja2 prompt design, easy modification of prompts + prompt imports from Promptsource
  * More advanced configuration options, including output post-processing, answer extraction, and multiple LM generations per document, configurable fewshot settings, and more
  * Speedups and new modeling libraries supported, including: faster data-parallel HF model usage, vLLM support, MPS support with HuggingFace, and more
  * Logging and usability changes
  * New tasks including CoT BIG-Bench-Hard, Belebele, user-defined task groupings, and more


Please see our updated documentation pages in `docs/` for more details.
Development will be continuing on the `main` branch, and we encourage you to give us feedback on what features are desired and how to improve the library further, or ask questions, either in issues or PRs on GitHub, or in the [EleutherAI discord](https://discord.gg/eleutherai)!
## Overview
This project provides a unified framework to test generative language models on a large number of different evaluation tasks.
**Features:**
  * Over 60 standard academic benchmarks for LLMs, with hundreds of subtasks and variants implemented.
  * Support for models loaded via [transformers](https://github.com/huggingface/transformers/) (including quantization via [GPTQModel](https://github.com/ModelCloud/GPTQModel) and [AutoGPTQ](https://github.com/PanQiWei/AutoGPTQ)), [GPT-NeoX](https://github.com/EleutherAI/gpt-neox), and [Megatron-DeepSpeed](https://github.com/microsoft/Megatron-DeepSpeed/), with a flexible tokenization-agnostic interface.
  * Support for fast and memory-efficient inference with [vLLM](https://github.com/vllm-project/vllm).
  * Support for commercial APIs including [OpenAI](https://openai.com), and [TextSynth](https://textsynth.com/).
  * Support for evaluation on adapters (e.g. LoRA) supported in [HuggingFace's PEFT library](https://github.com/huggingface/peft).
  * Support for local models and benchmarks.
  * Evaluation with publicly available prompts ensures reproducibility and comparability between papers.
  * Easy support for custom prompts and evaluation metrics.


The Language Model Evaluation Harness is the backend for 🤗 Hugging Face's popular [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard), has been used in [hundreds of papers](https://scholar.google.com/scholar?oi=bibs&hl=en&authuser=2&cites=15052937328817631261,4097184744846514103,1520777361382155671,17476825572045927382,18443729326628441434,14801318227356878622,7890865700763267262,12854182577605049984,15641002901115500560,5104500764547628290), and is used internally by dozens of organizations including NVIDIA, Cohere, BigScience, BigCode, Nous Research, and Mosaic ML.
## Install
To install the `lm-eval` package from the github repository, run:

```
git clone --depth 1 https://github.com/EleutherAI/lm-evaluation-harness
cd lm-evaluation-harness
pip install -e .
```

### Installing Model Backends
The base installation provides the core evaluation framework. **Model backends must be installed separately** using optional extras:
For HuggingFace transformers models:

```
pip install "lm_eval[hf]"
```

For vLLM inference:

```
pip install "lm_eval[vllm]"
```

For API-based models (OpenAI, Anthropic, etc.):

```
pip install "lm_eval[api]"
```

Multiple backends can be installed together:

```
pip install "lm_eval[hf,vllm,api]"
```

A detailed table of all optional extras is available at the end of this document.
## Basic Usage
### Documentation  
| Guide  | Description  |  
| --- | --- |  
| Command-line arguments and subcommands  |  
| YAML config file format and examples  |  
| Programmatic usage with `simple_evaluate()`  |  
| Available tasks and task configuration  |  
Use `lm-eval -h` to see available options, or `lm-eval run -h` for evaluation options.
List available tasks with:

```
lm-eval ls tasks
```

### Hugging Face `transformers`
Important
To use the HuggingFace backend, first install: `pip install "lm_eval[hf]"`
To evaluate a model hosted on the [HuggingFace Hub](https://huggingface.co/models) (e.g. GPT-J-6B) on `hellaswag` you can use the following command (this assumes you are using a CUDA-compatible GPU):

```
lm_eval --model hf \
    --model_args pretrained=EleutherAI/gpt-j-6B \
    --tasks hellaswag \
    --device cuda:0 \
    --batch_size 8
```

Additional arguments can be provided to the model constructor using the `--model_args` flag. Most notably, this supports the common practice of using the `revisions` feature on the Hub to store partially trained checkpoints, or to specify the datatype for running a model:

```
lm_eval --model hf \
    --model_args pretrained=EleutherAI/pythia-160m,revision=step100000,dtype="float" \
    --tasks lambada_openai,hellaswag \
    --device cuda:0 \
    --batch_size 8
```

Models that are loaded via both `transformers.AutoModelForCausalLM` (autoregressive, decoder-only GPT style models) and `transformers.AutoModelForSeq2SeqLM` (such as encoder-decoder models like T5) in Huggingface are supported.
Batch size selection can be automated by setting the `--batch_size` flag to `auto`. This will perform automatic detection of the largest batch size that will fit on your device. On tasks where there is a large difference between the longest and shortest example, it can be helpful to periodically recompute the largest batch size, to gain a further speedup. To do this, append `:N` to above flag to automatically recompute the largest batch size `N` times. For example, to recompute the batch size 4 times, the command would be:

```
lm_eval --model hf \
    --model_args pretrained=EleutherAI/pythia-160m,revision=step100000,dtype="float" \
    --tasks lambada_openai,hellaswag \
    --device cuda:0 \
    --batch_size auto:4
```

Note
Just like you can provide a local path to `transformers.AutoModel`, you can also provide a local path to `lm_eval` via `--model_args pretrained=/path/to/model`
#### Evaluating GGUF Models
`lm-eval` supports evaluating models in GGUF format using the Hugging Face (`hf`) backend. This allows you to use quantized models compatible with `transformers`, `AutoModel`, and llama.cpp conversions.
To evaluate a GGUF model, pass the path to the directory containing the model weights, the `gguf_file`, and optionally a separate `tokenizer` path using the `--model_args` flag.
**🚨 Important Note:** If no separate tokenizer is provided, Hugging Face will attempt to reconstruct the tokenizer from the GGUF file — this can take **hours** or even hang indefinitely. Passing a separate tokenizer avoids this issue and can reduce tokenizer loading time from hours to seconds.
**✅ Recommended usage:**

```
lm_eval --model hf \
    --model_args pretrained=/path/to/gguf_folder,gguf_file=model-name.gguf,tokenizer=/path/to/tokenizer \
    --tasks hellaswag \
    --device cuda:0 \
    --batch_size 8
```

Tip
Ensure the tokenizer path points to a valid Hugging Face tokenizer directory (e.g., containing tokenizer_config.json, vocab.json, etc.).
#### Evaluating GGUF Models Served by llama.cpp
The `gguf` model type evaluates models hosted by a [llama.cpp](https://github.com/ggml-org/llama.cpp) server (`llama-server`) through its OpenAI-compatible `/v1/completions` endpoint:

```
lm_eval --model gguf \
    --model_args base_url=http://127.0.0.1:8080 \
    --tasks hellaswag
```

  * Requires a llama.cpp release from December 2024 or newer, which returns logprobs in the modern OpenAI format (`logprobs.content`, see [llama.cpp#10783](https://github.com/ggml-org/llama.cpp/pull/10783)). The deprecated legacy format (`token_logprobs`) is not supported.
  * If the server runs in router mode (multiple models), pass the model name or alias: `--model_args base_url=http://127.0.0.1:8080,model=my-model-alias`.
  * Requests are issued concurrently. The default degree of parallelism is auto-detected from the server's slot count (`/props` → `total_slots`, i.e. llama-server's `--parallel` setting); override it with `parallel=<N>`. `loglikelihood` requests are additionally pinned to slots via llama.cpp's `id_slot` parameter, with consecutive requests sharing a context (e.g. the candidate continuations of one multiple-choice question) assigned to the same slot so they reuse its cached prompt prefix. `generate_until` requests are left unpinned, since the server's dynamic idle-slot assignment load-balances variable-length generations better.
  * `loglikelihood` is implemented via exact teacher forcing: llama.cpp ignores `echo` and never returns prompt logprobs, so the continuation is tokenized with the server's `/tokenize` endpoint (context and continuation encoded separately, matching the HF backend), and each continuation token is scored with its natural prefix as a token-id prompt plus a `logit_bias` that forces the server to sample it. llama.cpp reports the forced token's _pre-sampling_ logprob and an unbiased `top_logprobs`, yielding loglikelihoods that match the HF backend to within quantization noise. (A naive alternative — forcing the continuation with a GBNF grammar — does _not_ work: grammar-constrained decoding can pick a fragmented tokenization whose logprobs are not the natural-tokenization loglikelihoods, which systematically deflates scores.)
  * `loglikelihood_rolling` (perplexity tasks such as wikitext) is not implemented for this model type.


#### Multi-GPU Evaluation with Hugging Face `accelerate`
We support three main ways of using Hugging Face's [accelerate 🚀](https://github.com/huggingface/accelerate) library for multi-GPU evaluation.
To perform _data-parallel evaluation_ (where each GPU loads a **separate full copy** of the model), we leverage the `accelerate` launcher as follows:

```
accelerate launch -m lm_eval --model hf \
    --tasks lambada_openai,arc_easy \
    --batch_size 16
```

(or via `accelerate launch --no-python lm_eval`).
For cases where your model can fit on a single GPU, this allows you to evaluate on K GPUs K times faster than on one.
**WARNING** : This setup does not work with FSDP model sharding, so in `accelerate config` FSDP must be disabled, or the NO_SHARD FSDP option must be used.
The second way of using `accelerate` for multi-GPU evaluation is when your model is _too large to fit on a single GPU._
In this setting, run the library _outside the`accelerate` launcher_, but passing `parallelize=True` to `--model_args` as follows:

```
lm_eval --model hf \
    --tasks lambada_openai,arc_easy \
    --model_args parallelize=True \
    --batch_size 16
```

This means that your model's weights will be split across all available GPUs.
For more advanced users or even larger models, we allow for the following arguments when `parallelize=True` as well:
  * `device_map_option`: How to split model weights across available GPUs. defaults to "auto".
  * `max_memory_per_gpu`: the max GPU memory to use per GPU in loading the model.
  * `max_cpu_memory`: the max amount of CPU memory to use when offloading the model weights to RAM.
  * `offload_folder`: a folder where model weights will be offloaded to disk if needed.


The third option is to use both at the same time. This will allow you to take advantage of both data parallelism and model sharding, and is especially useful for models that are too large to fit on a single GPU.

```
accelerate launch --multi_gpu --num_processes {nb_of_copies_of_your_model} \
    -m lm_eval --model hf \
    --tasks lambada_openai,arc_easy \
    --model_args parallelize=True \
    --batch_size 16
```

To learn more about model parallelism and how to use it with the `accelerate` library, see the [accelerate documentation](https://huggingface.co/docs/transformers/v4.15.0/en/parallelism)
**Warning: We do not natively support multi-node evaluation using the`hf` model type! Please reference [our GPT-NeoX library integration](https://github.com/EleutherAI/gpt-neox/blob/main/eval.py) for an example of code in which a custom multi-machine evaluation script is written.**
**Note: we do not currently support multi-node evaluations natively, and advise using either an externally hosted server to run inference requests against, or creating a custom integration with your distributed framework[as is done for the GPT-NeoX library](https://github.com/EleutherAI/gpt-neox/blob/main/eval_tasks/eval_adapter.py).**
#### Tensor Parallelism (native PyTorch)
For models that support PyTorch's native Tensor Parallelism (via DTensor), you can shard model weights across GPUs without `accelerate`'s device-map by passing `tp_plan=auto` in `--model_args`. Launch with `torchrun` or `accelerate launch`:

```
torchrun --nproc-per-node=4 -m lm_eval \
    --model hf \
    --model_args pretrained=google/gemma-4-31B-it,tp_plan=auto \
    --tasks lambada_openai,arc_easy \
    --batch_size 16
```

**Constraints:**
  * `tp_plan` and `parallelize=True` are mutually exclusive — use one or the other.
  * The number of key-value heads in the model must be divisible by `--nproc-per-node` (the TP degree).
  * Requires PyTorch >= 2.4 and a `transformers` version that exposes a TP plan for the model (v4.47+).


### Steered Hugging Face `transformers` models
To evaluate a Hugging Face `transformers` model with steering vectors applied, specify the model type as `steered` and provide the path to either a PyTorch file containing pre-defined steering vectors, or a CSV file that specifies how to derive steering vectors from pretrained `sparsify` or `sae_lens` models (you will need to install the corresponding optional dependency for this method).
Specify pre-defined steering vectors:

```
import torch

steer_config = {
    "layers.3": {
        "steering_vector": torch.randn(1, 768),
        "bias": torch.randn(1, 768),
        "steering_coefficient": 1,
        "action": "add"
    },
}
torch.save(steer_config, "steer_config.pt")
```

Specify derived steering vectors:

```
import pandas as pd

pd.DataFrame({
    "loader": ["sparsify"],
    "action": ["add"],
    "sparse_model": ["EleutherAI/sae-pythia-70m-32k"],
    "hookpoint": ["layers.3"],
    "feature_index": [30],
    "steering_coefficient": [10.0],
}).to_csv("steer_config.csv", index=False)
```

Run the evaluation harness with steering vectors applied:

```
lm_eval --model steered \
    --model_args pretrained=EleutherAI/pythia-160m,steer_path=steer_config.pt \
    --tasks lambada_openai,hellaswag \
    --device cuda:0 \
    --batch_size 8
```

### NVIDIA `nemo` models
[NVIDIA NeMo Framework](https://github.com/NVIDIA/NeMo) is a generative AI framework built for researchers and pytorch developers working on language models.
To evaluate a `nemo` model, start by installing NeMo following [the documentation](https://github.com/NVIDIA/NeMo?tab=readme-ov-file#installation). We highly recommended to use the NVIDIA PyTorch or NeMo container, especially if having issues installing Apex or any other dependencies (see [latest released containers](https://github.com/NVIDIA/NeMo/releases)). Please also install the lm evaluation harness library following the instructions in [the Install section](https://github.com/EleutherAI/lm-evaluation-harness/tree/main?tab=readme-ov-file#install).
NeMo models can be obtained through [NVIDIA NGC Catalog](https://catalog.ngc.nvidia.com/models) or in [NVIDIA's Hugging Face page](https://huggingface.co/nvidia). In [NVIDIA NeMo Framework](https://github.com/NVIDIA/NeMo/tree/main/scripts/nlp_language_modeling) there are conversion scripts to convert the `hf` checkpoints of popular models like llama, falcon, mixtral or mpt to `nemo`.
Run a `nemo` model on one GPU:

```
lm_eval --model nemo_lm \
    --model_args path=path_to_nemo_model \
    --tasks hellaswag \
    --batch_size 32
```

It is recommended to unpack the `nemo` model to avoid the unpacking inside the docker container - it may overflow disk space. For that you can run:

```
mkdir MY_MODEL
tar -xvf MY_MODEL.nemo -c MY_MODEL
```

#### Multi-GPU evaluation with NVIDIA `nemo` models
By default, only one GPU is used. But we do support either data replication or tensor/pipeline parallelism during evaluation, on one node.
  1. To enable data replication, set the `model_args` of `devices` to the number of data replicas to run. For example, the command to run 8 data replicas over 8 GPUs is:



```
torchrun --nproc-per-node=8 --no-python lm_eval \
    --model nemo_lm \
    --model_args path=path_to_nemo_model,devices=8 \
    --tasks hellaswag \
    --batch_size 32
```

  1. To enable tensor and/or pipeline parallelism, set the `model_args` of `tensor_model_parallel_size` and/or `pipeline_model_parallel_size`. In addition, you also have to set up `devices` to be equal to the product of `tensor_model_parallel_size` and/or `pipeline_model_parallel_size`. For example, the command to use one node of 4 GPUs with tensor parallelism of 2 and pipeline parallelism of 2 is:



```
torchrun --nproc-per-node=4 --no-python lm_eval \
    --model nemo_lm \
    --model_args path=path_to_nemo_model,devices=4,tensor_model_parallel_size=2,pipeline_model_parallel_size=2 \
    --tasks hellaswag \
    --batch_size 32
```

Note that it is recommended to substitute the `python` command by `torchrun --nproc-per-node=<number of devices> --no-python` to facilitate loading the model into the GPUs. This is especially important for large checkpoints loaded into multiple GPUs.
Not supported yet: multi-node evaluation and combinations of data replication with tensor or pipeline parallelism.
### Megatron-LM models
[Megatron-LM](https://github.com/NVIDIA/Megatron-LM) is NVIDIA's large-scale transformer training framework. This backend allows direct evaluation of Megatron-LM checkpoints without conversion.
**Requirements:**
  * Megatron-LM must be installed or accessible via `MEGATRON_PATH` environment variable
  * PyTorch with CUDA support


**Setup:**

```
# Set environment variable pointing to Megatron-LM installation
export MEGATRON_PATH=/path/to/Megatron-LM
```

**Basic usage (single GPU):**

```
lm_eval --model megatron_lm \
    --model_args load=/path/to/checkpoint,tokenizer_type=HuggingFaceTokenizer,tokenizer_model=/path/to/tokenizer \
    --tasks hellaswag \
    --batch_size 1
```

**Supported checkpoint formats:**
  * Standard Megatron checkpoints (`model_optim_rng.pt`)
  * Distributed checkpoints (`.distcp` format, auto-detected)


#### Parallelism Modes
The Megatron-LM backend supports the following parallelism modes:  
| Mode  | Configuration  | Description  |  
| --- | --- | --- |  
| Single GPU  |  `devices=1` (default)  | Standard single GPU evaluation  |  
| Data Parallelism  | `devices>1, TP=1`  | Each GPU has a full model replica, data is distributed  |  
| Tensor Parallelism  | `TP == devices`  | Model layers are split across GPUs  |  
| Expert Parallelism  | `EP == devices, TP=1`  | For MoE models, experts are distributed across GPUs  |  
Note
  * Pipeline Parallelism (PP > 1) is not currently supported.
  * Expert Parallelism (EP) cannot be combined with Tensor Parallelism (TP).


**Data Parallelism (4 GPUs, each with full model replica):**

```
torchrun --nproc-per-node=4 -m lm_eval --model megatron_lm \
    --model_args load=/path/to/checkpoint,tokenizer_model=/path/to/tokenizer,devices=4 \
    --tasks hellaswag
```

**Tensor Parallelism (TP=2):**

```
torchrun --nproc-per-node=2 -m lm_eval --model megatron_lm \
    --model_args load=/path/to/checkpoint,tokenizer_model=/path/to/tokenizer,devices=2,tensor_model_parallel_size=2 \
    --tasks hellaswag
```

**Expert Parallelism for MoE models (EP=4):**

```
torchrun --nproc-per-node=4 -m lm_eval --model megatron_lm \
    --model_args load=/path/to/moe_checkpoint,tokenizer_model=/path/to/tokenizer,devices=4,expert_model_parallel_size=4 \
    --tasks hellaswag
```

**Using extra_args for additional Megatron options:**

```
lm_eval --model megatron_lm \
    --model_args load=/path/to/checkpoint,tokenizer_model=/path/to/tokenizer,extra_args="--no-rope-fusion --trust-remote-code" \
    --tasks hellaswag
```

Note
The `--use-checkpoint-args` flag is enabled by default, which loads model architecture parameters from the checkpoint. For checkpoints converted via Megatron-Bridge, this typically includes all necessary model configuration.
#### Multi-GPU evaluation with OpenVINO models
Pipeline parallelism during evaluation is supported with OpenVINO models
To enable pipeline parallelism, set the `model_args` of `pipeline_parallel`. In addition, you also have to set up `device` to value `HETERO:<GPU index1>,<GPU index2>` for example `HETERO:GPU.1,GPU.0` For example, the command to use pipeline parallelism of 2 is:

```
lm_eval --model openvino \
    --tasks wikitext \
    --model_args pretrained=path_to_ov_model,pipeline_parallel=True \
    --device HETERO:GPU.1,GPU.0
```

### Tensor + Data Parallel and Optimized Inference with `vLLM`
We also support vLLM for faster inference on [supported model types](https://docs.vllm.ai/en/latest/models/supported_models.html), especially faster when splitting a model across multiple GPUs. For single-GPU or multi-GPU — tensor parallel, data parallel, or a combination of both — inference, for example:

```
lm_eval --model vllm \
    --model_args pretrained={model_name},tensor_parallel_size={GPUs_per_model},dtype=auto,gpu_memory_utilization=0.8,data_parallel_size={model_replicas} \
    --tasks lambada_openai \
    --batch_size auto
```

To use vllm, do `pip install "lm_eval[vllm]"`. For a full list of supported vLLM configurations, please reference our [vLLM integration](https://github.com/EleutherAI/lm-evaluation-harness/blob/e74ec966556253fbe3d8ecba9de675c77c075bce/lm_eval/models/vllm_causallms.py) and the vLLM documentation.
Note
`data_parallel_size>1` dispatches each replica as a separate [ray](https://github.com/ray-project/ray) actor and requires `pip install ray`. Each actor reserves `tensor_parallel_size` GPUs (default 1).
vLLM occasionally differs in output from Huggingface. We treat Huggingface as the reference implementation and provide a [script](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/scripts/model_comparator.py) for checking the validity of vllm results against HF.
Tip
For fastest performance, we recommend using `--batch_size auto` for vLLM whenever possible, to leverage its continuous batching functionality!
Tip
Passing `max_model_len=4096` or some other reasonable default to vLLM through model args may cause speedups or prevent out-of-memory errors when trying to use auto batch size, such as for Mistral-7B-v0.1 which defaults to a maximum length of 32k.
### Tensor + Data Parallel and Fast Offline Batching Inference with `SGLang`
We support SGLang for efficient offline batch inference. Its **[Fast Backend Runtime](https://docs.sglang.ai/index.html)** delivers high performance through optimized memory management and parallel processing techniques. Key features include tensor parallelism, continuous batching, and support for various quantization methods (FP8/INT4/AWQ/GPTQ).
To use SGLang as the evaluation backend, please **install it in advance** via SGLang documents [here](https://docs.sglang.io/get_started/install.html#install-sglang).
Tip
Due to the installing method of [`Flashinfer`](https://docs.flashinfer.ai/)-- a fast attention kernel library, we don't include the dependencies of `SGLang` within [pyproject.toml](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/pyproject.toml). Note that the `Flashinfer` also has some requirements on `torch` version.
SGLang's server arguments are slightly different from other backends, see [here](https://docs.sglang.io/advanced_features/server_arguments.html) for more information. We provide an example of the usage here:

```
lm_eval --model sglang \
    --model_args pretrained={model_name},dp_size={data_parallel_size},tp_size={tensor_parallel_size},dtype=auto \
    --tasks gsm8k_cot \
    --batch_size auto
```

Tip
When encountering out-of-memory (OOM) errors (especially for multiple-choice tasks), try these solutions:
  1. Use a manual `batch_size`, rather than `auto`.
  2. Lower KV cache pool memory usage by adjusting `mem_fraction_static` - Add to your model arguments for example `--model_args pretrained=...,mem_fraction_static=0.7`.
  3. Increase tensor parallel size `tp_size` (if using multiple GPUs).


### ONNX Runtime GenAI
We support **ONNX Runtime GenAI** for cross-platform evaluation of ONNX LLMs produced by the [ONNX Runtime GenAI _Model Builder_](https://onnxruntime.ai/docs/genai/howto/build-model.html). Unlike the [Windows ML](https://github.com/EleutherAI/lm-evaluation-harness#windows-ml) backend, this backend runs on Linux, macOS, and Windows and selects execution providers with the cross-platform `og.Config` API (CPU, CUDA, DirectML, WebGPU, and AMD NPU via VitisAI/RyzenAI).
Install the backend along with the execution-provider wheel that matches your hardware:

```

pip install "lm_eval[onnxruntime-genai]"
# CUDA / DirectML instead of the CPU wheel (mutually exclusive):
#   pip install onnxruntime-genai-cuda
#   pip install onnxruntime-genai-directml
```

Evaluate a Model Builder ONNX model on a chosen execution provider:

```
lm_eval --model onnxruntime-genai \
    --model_args pretrained=/path/to/model_builder_output,execution_provider=cuda \
    --tasks hellaswag \
    --batch_size 1
```

The `pretrained` path is a Model Builder output directory (containing `genai_config.json`, the ONNX graph(s), and an HF tokenizer) or a `.onnx` file inside one. `execution_provider` defaults to `cpu` (which runs with no provider appended); pass `cuda`, `dml`, `VitisAI`, etc. for accelerators. Extra provider settings can be passed via `provider_options`.
Note
Supported architectures are whatever the Model Builder supports (Llama, Phi, Qwen, Gemma, Mistral, Granite, ChatGLM, …). Inference is batch-size 1 and runs one execution provider per run.
### ONNX Runtime
We also support running the _same_ Model Builder export through a raw `onnxruntime.InferenceSession` instead of the GenAI loop. Use this backend when you want scores to come from the exact runtime a deployment uses, or when you need an execution provider that `onnxruntime-genai` does not build — notably **ROCm** and **MIGraphX** for AMD GPUs.

```

pip install "lm_eval[onnxruntime]"
# CUDA / ROCm instead of the CPU wheel (mutually exclusive):
#   pip install onnxruntime-gpu
#   pip install onnxruntime-rocm
```


```
lm_eval --model onnxruntime \
    --model_args pretrained=/path/to/model_builder_output,execution_provider=rocm \
    --tasks hellaswag,arc_easy,wikitext \
    --batch_size 1
```

`execution_provider` accepts short aliases (`cpu`, `cuda`, `rocm`, `migraphx`, `dml`, `openvino`, `tensorrt`, `vitisai`, `webgpu`, `qnn`) as well as full ONNX Runtime provider names. As with the `onnxruntime-genai` backend, leaving it unset honors the providers declared in the export's `genai_config.json`, so an export built for a specific device runs there without extra flags. Non-CPU providers keep `CPUExecutionProvider` as a per-node fallback, since Model Builder graphs use contrib ops that not every provider implements.
Because both ONNX backends share one scoring implementation and end up in the same ORT kernels, they agree on the same model and provider; `tests/models/test_onnxruntime_parity.py` enforces that as a standing check.
Note
This backend currently covers `loglikelihood`, `multiple_choice`, and `loglikelihood_rolling` (hellaswag, arc, mmlu, wikitext, …). For generative tasks such as gsm8k, use `--model onnxruntime-genai` with the same model directory.
The installed `onnxruntime` must be new enough to load the contrib-op schemas your Model Builder version emitted. An older runtime can reject the graph outright — for example a 12-input `GroupQueryAttention` fails to load on ONNX Runtime 1.22.
### Windows ML
We support **Windows ML** for hardware-accelerated inference on Windows platforms. This enables evaluation on CPU, GPU, and **NPU (Neural Processing Unit)** devices.
Windows ML? <https://learn.microsoft.com/en-us/windows/ai/new-windows-ml/overview>
To use Windows ML, install the required dependencies:

```
pip install wasdk-Microsoft.Windows.AI.MachineLearning[all] wasdk-Microsoft.Windows.ApplicationModel.DynamicDependency.Bootstrap onnxruntime-windowsml onnxruntime-genai-winml
```

Evaluate an ONNX Runtime GenAI LLM on NPU/GPU/CPU on Windows:

```
lm_eval --model winml \
    --model_args pretrained=/path/to/onnx/model \
    --tasks mmlu \
    --batch_size 1
```

Note
The Windows ML backend is ONLY for ONNX Runtime GenAI model format. Models targeting `transformers.js` won't work. You can verify this by finding the `genai_config.json` file in the model folder.
Note
To run an ONNX Runtime GenAI model on the target device, you MUST convert the original model to that vendor and device type. Converted models won't work / work well on other vendor or device types. To learn more on model conversion, please visit [Microsoft AI Tool Kit](https://code.visualstudio.com/docs/intelligentapps/modelconversion)
### Model APIs and Inference Servers
Important
To use API-based models, first install: `pip install "lm_eval[api]"`
Our library also supports the evaluation of models served via several commercial APIs, and we hope to implement support for the most commonly used performant local/self-hosted inference servers.
To call a hosted model, use:

```
export OPENAI_API_KEY=YOUR_KEY_HERE
lm_eval --model openai-completions \
    --model_args model=davinci-002 \
    --tasks lambada_openai,hellaswag
```

We also support using your own local inference server with servers that mirror the OpenAI Completions and ChatCompletions APIs.

```
lm_eval --model local-completions --tasks gsm8k --model_args model=facebook/opt-125m,base_url=http://{yourip}:8000/v1/completions,num_concurrent=1,max_retries=3,tokenized_requests=False,batch_size=16
```

Note that for externally hosted models, configs such as `--device` which relate to where to place a local model should not be used and do not function. Just like you can use `--model_args` to pass arbitrary arguments to the model constructor for local models, you can use it to pass arbitrary arguments to the model API for hosted models. See the documentation of the hosting service for information on what arguments they support.  
| API or Inference Server  | Implemented?  |  `--model <xxx>` name  | Models supported:  | Request Types:  |  
| --- | --- | --- | --- | --- |  
| OpenAI Completions  | ✔️  |  `openai-completions`, `local-completions`  | All OpenAI Completions API models  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| OpenAI ChatCompletions  | ✔️  |  `openai-chat-completions`, `local-chat-completions`  | [All ChatCompletions API models](https://platform.openai.com/docs/guides/gpt)  |  `generate_until` (no logprobs)  |  
| Anthropic  | ✔️  | `anthropic`  | [Supported Anthropic Engines](https://docs.anthropic.com/claude/reference/selecting-a-model)  |  `generate_until` (no logprobs)  |  
| Anthropic Chat  | ✔️  |  `anthropic-chat`, `anthropic-chat-completions`  | [Supported Anthropic Engines](https://docs.anthropic.com/claude/docs/models-overview)  |  `generate_until` (no logprobs)  |  
|  [LiteLLM](https://github.com/BerriAI/litellm) (gateway to 100+ providers)  | ✔️  |  `litellm`, `litellm-chat`, `litellm-chat-completions`  | [All LiteLLM-supported providers](https://docs.litellm.ai/docs/providers)  |  `generate_until` (no logprobs)  |  
| Textsynth  | ✔️  | `textsynth`  | [All supported engines](https://textsynth.com/documentation.html#engines)  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| Cohere  | [⌛ - blocked on Cohere API bug](https://github.com/EleutherAI/lm-evaluation-harness/pull/395)  | N/A  | [All `cohere.generate()` engines](https://docs.cohere.com/docs/models)  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
|  [Llama.cpp](https://github.com/ggerganov/llama.cpp) (via [llama-cpp-python](https://github.com/abetlen/llama-cpp-python))  | ✔️  |  `gguf`, `ggml`  | [All models supported by llama.cpp](https://github.com/ggerganov/llama.cpp)  |  `generate_until`, `loglikelihood`, (perplexity evaluation not yet implemented)  |  
| vLLM  | ✔️  | `vllm`  | [Most HF Causal Language Models](https://docs.vllm.ai/en/latest/models/supported_models.html)  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| Mamba  | ✔️  | `mamba_ssm`  | [Mamba architecture Language Models via the `mamba_ssm` package](https://huggingface.co/state-spaces)  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| Huggingface Optimum (Causal LMs)  | ✔️  | `openvino`  | Any decoder-only AutoModelForCausalLM converted with Huggingface Optimum into OpenVINO™ Intermediate Representation (IR) format  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| Huggingface Optimum-intel IPEX (Causal LMs)  | ✔️  | `ipex`  | Any decoder-only AutoModelForCausalLM  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| Huggingface Optimum-habana (Causal LMs)  | ✔️  | `habana`  | Any decoder-only AutoModelForCausalLM  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| Neuron via AWS Inf2 (Causal LMs)  | ✔️  | `neuronx`  | Any decoder-only AutoModelForCausalLM supported to run on [huggingface-ami image for inferentia2](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2)  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| NVIDIA NeMo  | ✔️  | `nemo_lm`  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| NVIDIA Megatron-LM  | ✔️  | `megatron_lm`  |  [Megatron-LM GPT models](https://github.com/NVIDIA/Megatron-LM) (standard and distributed checkpoints)  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| Watsonx.ai  | ✔️  | `watsonx_llm`  | [Supported Watsonx.ai Engines](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/fm-models.html?context=wx)  |  `generate_until` `loglikelihood`  |  
| ONNX Runtime GenAI  | ✔️  | `onnxruntime-genai`  |  [ONNX models in GenAI format](https://onnxruntime.ai/docs/genai/howto/build-model.html) (cross-platform: CPU/CUDA/DirectML/NPU)  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| ONNX Runtime  | ✔️  | `onnxruntime`  |  [ONNX models in GenAI format](https://onnxruntime.ai/docs/genai/howto/build-model.html) via a raw `InferenceSession` (adds ROCm/MIGraphX)  |  `loglikelihood`, `loglikelihood_rolling`  |  
| Windows ML  | ✔️  | `winml`  | [ONNX models in GenAI format](https://code.visualstudio.com/docs/intelligentapps/modelconversion)  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
| [Your local inference server!](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/API_guide.md)  | ✔️  |  `local-completions` or `local-chat-completions`  | Support for OpenAI API-compatible servers, with easy customization for other APIs.  |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`  |  
Models which do not supply logits or logprobs can be used with tasks of type `generate_until` only, while local models, or APIs that supply logprobs/logits of their prompts, can be run on all task types: `generate_until`, `loglikelihood`, `loglikelihood_rolling`, and `multiple_choice`.
For more information on the different task `output_types` and model request types, see [our documentation](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/model_guide.md#interface).
Note
For best performance with closed chat model APIs such as Anthropic Claude 3 and GPT-4, we recommend carefully looking at a few sample outputs using `--limit 10` first to confirm answer extraction and scoring on generative tasks is performing as expected. providing `system="<some system prompt here>"` within `--model_args` for anthropic-chat-completions, to instruct the model what format to respond in, may be useful.
### Other Frameworks
A number of other libraries contain scripts for calling the eval harness through their library. These include [GPT-NeoX](https://github.com/EleutherAI/gpt-neox/blob/main/eval_tasks/eval_adapter.py), [Megatron-DeepSpeed](https://github.com/microsoft/Megatron-DeepSpeed/blob/main/examples/MoE/readme_evalharness.md), and [mesh-transformer-jax](https://github.com/kingoflolz/mesh-transformer-jax/blob/master/eval_harness.py).
To create your own custom integration you can follow instructions from [this tutorial](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/interface.md#external-library-usage).
### Additional Features
Note
For tasks unsuitable for direct evaluation — either due risks associated with executing untrusted code or complexities in the evaluation process — the `--predict_only` flag is available to obtain decoded generations for post-hoc evaluation.
If you have a Metal compatible Mac, you can run the eval harness using the MPS back-end by replacing `--device cuda:0` with `--device mps` (requires PyTorch version 2.1 or higher). **Note that the PyTorch MPS backend is still in early stages of development, so correctness issues or unsupported operations may exist. If you observe oddities in model performance on the MPS back-end, we recommend first checking that a forward pass of your model on`--device cpu` and `--device mps` match.**
Note
You can inspect what the LM inputs look like by running the following command:

```
python write_out.py \
    --tasks task1,task2,... \
    --num_fewshot 5 \
    --num_examples 10 \
    --output_base_path /path/to/output/folder
```

This will write out one text file for each task.
To verify the data integrity of the tasks you're performing in addition to running the tasks themselves, you can use the `--check_integrity` flag:

```
lm_eval --model openai \
    --model_args engine=davinci-002 \
    --tasks lambada_openai,hellaswag \
    --check_integrity
```

## Advanced Usage Tips
For models loaded with the HuggingFace `transformers` library, any arguments provided via `--model_args` get passed to the relevant constructor directly. This means that anything you can do with `AutoModel` can be done with our library. For example, you can pass a local path via `pretrained=` or use models finetuned with [PEFT](https://github.com/huggingface/peft) by taking the call you would run to evaluate the base model and add `,peft=PATH` to the `model_args` argument:

```
lm_eval --model hf \
    --model_args pretrained=EleutherAI/gpt-j-6b,parallelize=True,load_in_4bit=True,peft=nomic-ai/gpt4all-j-lora \
    --tasks openbookqa,arc_easy,winogrande,hellaswag,arc_challenge,piqa,boolq \
    --device cuda:0
```

Models provided as delta weights can be easily loaded using the Hugging Face transformers library. Within --model_args, set the delta argument to specify the delta weights, and use the pretrained argument to designate the relative base model to which they will be applied:

```
lm_eval --model hf \
    --model_args pretrained=Ejafa/llama_7B,delta=lmsys/vicuna-7b-delta-v1.1 \
    --tasks hellaswag
```

GPTQ quantized models can be loaded using [GPTQModel](https://github.com/ModelCloud/GPTQModel) (faster) or [AutoGPTQ](https://github.com/PanQiWei/AutoGPTQ)
GPTQModel: add `,gptqmodel=True` to `model_args`

```
lm_eval --model hf \
    --model_args pretrained=model-name-or-path,gptqmodel=True \
    --tasks hellaswag
```

AutoGPTQ: add `,autogptq=True` to `model_args`:

```
lm_eval --model hf \
    --model_args pretrained=model-name-or-path,autogptq=model.safetensors,gptq_use_triton=True \
    --tasks hellaswag
```

We support wildcards in task names, for example you can run all of the machine-translated lambada tasks via `--task lambada_openai_mt_*`.
## Saving & Caching Results
To save evaluation results provide an `--output_path`. We also support logging model responses with the `--log_samples` flag for post-hoc analysis.
Tip
Use `--use_cache <DIR>` to cache evaluation results and skip previously evaluated samples when resuming runs of the same (model, task) pairs. Note that caching is rank-dependent, so restart with the same GPU count if interrupted. You can also use --cache_requests to save dataset preprocessing steps for faster evaluation resumption.
To push results and samples to the Hugging Face Hub, first ensure an access token with write access is set in the `HF_TOKEN` environment variable. Then, use the `--hf_hub_log_args` flag to specify the organization, repository name, repository visibility, and whether to push results and samples to the Hub - [example dataset on the HF Hub](https://huggingface.co/datasets/KonradSzafer/lm-eval-results-demo). For instance:

```
lm_eval --model hf \
    --model_args pretrained=model-name-or-path,autogptq=model.safetensors,gptq_use_triton=True \
    --tasks hellaswag \
    --log_samples \
    --output_path results \
    --hf_hub_log_args hub_results_org=EleutherAI,hub_repo_name=lm-eval-results,push_results_to_hub=True,push_samples_to_hub=True,public_repo=False \
```

This allows you to easily download the results and samples from the Hub, using:

```
from datasets import load_dataset

load_dataset("EleutherAI/lm-eval-results-private", "hellaswag", "latest")
```

For a full list of supported arguments, check out the [interface](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/interface.md) guide in our documentation!
## Visualizing Results
You can seamlessly visualize and analyze the results of your evaluation harness runs using both Weights & Biases (W&B) and Zeno.
You can use [Zeno](https://zenoml.com) to visualize the results of your eval harness runs.
First, head to [hub.zenoml.com](https://hub.zenoml.com) to create an account and get an API key [on your account page](https://hub.zenoml.com/account). Add this key as an environment variable:

```
export ZENO_API_KEY=[your api key]
```

You'll also need to install the `lm_eval[zeno]` package extra.
To visualize the results, run the eval harness with the `log_samples` and `output_path` flags. We expect `output_path` to contain multiple folders that represent individual model names. You can thus run your evaluation on any number of tasks and models and upload all of the results as projects on Zeno.

```
lm_eval \
    --model hf \
    --model_args pretrained=EleutherAI/gpt-j-6B \
    --tasks hellaswag \
    --device cuda:0 \
    --batch_size 8 \
    --log_samples \
    --output_path output/gpt-j-6B
```

Then, you can upload the resulting data using the `zeno_visualize` script:

```
python scripts/zeno_visualize.py \
    --data_path output \
    --project_name "Eleuther Project"
```

This will use all subfolders in `data_path` as different models and upload all tasks within these model folders to Zeno. If you run the eval harness on multiple tasks, the `project_name` will be used as a prefix and one project will be created per task.
You can find an example of this workflow in [examples/visualize-zeno.ipynb](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/examples/visualize-zeno.ipynb).
### Weights and Biases
With the [Weights and Biases](https://wandb.ai/site) integration, you can now spend more time extracting deeper insights into your evaluation results. The integration is designed to streamline the process of logging and visualizing experiment results using the Weights & Biases (W&B) platform.
The integration provide functionalities
  * to automatically log the evaluation results,
  * log the samples as W&B Tables for easy visualization,
  * log the `results.json` file as an artifact for version control,
  * log the `<task_name>_eval_samples.json` file if the samples are logged,
  * generate a comprehensive report for analysis and visualization with all the important metric,
  * log task and cli specific configs,
  * and more out of the box like the command used to run the evaluation, GPU/CPU counts, timestamp, etc.


First you'll need to install the lm_eval[wandb] package extra. Do `pip install lm_eval[wandb]`.
Authenticate your machine with an your unique W&B token. Visit <https://wandb.ai/authorize> to get one. Do `wandb login` in your command line terminal.
Run eval harness as usual with a `wandb_args` flag. Use this flag to provide arguments for initializing a wandb run ([wandb.init](https://docs.wandb.ai/ref/python/init)) as comma separated string arguments.

```
lm_eval \
    --model hf \
    --model_args pretrained=microsoft/phi-2,trust_remote_code=True \
    --tasks hellaswag,mmlu_abstract_algebra \
    --device cuda:0 \
    --batch_size 8 \
    --output_path output/phi-2 \
    --limit 10 \
    --wandb_args project=lm-eval-harness-integration \
    --log_samples
```

In the stdout, you will find the link to the W&B run page as well as link to the generated report. You can find an example of this workflow in [examples/visualize-wandb.ipynb](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/examples/visualize-wandb.ipynb), and an example of how to integrate it beyond the CLI.
## Contributing
Check out our [open issues](https://github.com/EleutherAI/lm-evaluation-harness/issues) and feel free to submit pull requests!
For more information on the library and how everything fits together, see our [documentation pages](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/docs).
To get started with development, first clone the repository and install the dev dependencies:

```
git clone https://github.com/EleutherAI/lm-evaluation-harness
cd lm-evaluation-harness
pip install -e ".[dev,hf]"
```

### Implementing new tasks
To implement a new task in the eval harness, see [this guide](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/new_task_guide.md).
In general, we follow this priority list for addressing concerns about prompting and other eval details:
  1. If there is widespread agreement among people who train LLMs, use the agreed upon procedure.
  2. If there is a clear and unambiguous official implementation, use that procedure.
  3. If there is widespread agreement among people who evaluate LLMs, use the agreed upon procedure.
  4. If there are multiple common implementations but not universal or widespread agreement, use our preferred option among the common implementations. As before, prioritize choosing from among the implementations found in LLM training papers.


These are guidelines and not rules, and can be overruled in special circumstances.
We try to prioritize agreement with the procedures used by other groups to decrease the harm when people inevitably compare runs across different papers despite our discouragement of the practice. Historically, we also prioritized the implementation from [Language Models are Few Shot Learners](https://arxiv.org/abs/2005.14165) as our original goal was specifically to compare results with that paper.
### Support
The best way to get support is to open an issue on this repo or join the [EleutherAI Discord server](https://discord.gg/eleutherai). The `#lm-thunderdome` channel is dedicated to developing this project and the `#release-discussion` channel is for receiving support for our releases. If you've used the library and have had a positive (or negative) experience, we'd love to hear from you!
## Optional Extras
Extras dependencies can be installed via `pip install -e ".[NAME]"`
### Model Backends
These extras install dependencies required to run specific model backends:  
| NAME  | Description  |  
| --- | --- |  
| hf  | HuggingFace Transformers (torch, transformers, accelerate, peft)  |  
| vllm  | vLLM fast inference  |  
| api  | API models (OpenAI, Anthropic, local servers)  |  
| gptq  | AutoGPTQ quantized models  |  
| gptqmodel  | GPTQModel quantized models  |  
| ibm_watsonx_ai  | IBM watsonx.ai models  |  
| ipex  | Intel IPEX backend  |  
| habana  | Intel Gaudi backend  |  
| optimum  | Intel OpenVINO models  |  
| neuronx  | AWS Inferentia2 instances  |  
| onnxruntime-genai  | ONNX Runtime GenAI (cross-platform) - CPU/CUDA/DirectML/NPU  |  
| onnxruntime  | ONNX Runtime raw session - adds ROCm/MIGraphX  |  
| winml  | Windows ML (ONNX Runtime GenAI) - CPU/GPU/NPU  |  
| sparsify  | Sparsify model steering  |  
| sae_lens  | SAELens model steering  |  
### Task Dependencies
These extras install dependencies required for specific evaluation tasks:  
| NAME  | Description  |  
| --- | --- |  
| tasks  | All task-specific dependencies  |  
| acpbench  | ACP Bench tasks  |  
| audiolm_qwen  | Qwen2 audio models  |  
| ifeval  | IFEval task  |  
| japanese_leaderboard  | Japanese LLM tasks  |  
| longbench  | LongBench tasks  |  
| math  | Math answer checking  |  
| multilingual  | Multilingual tokenizers  |  
| ruler  | RULER tasks  |  
### Development & Utilities  
| NAME  | Description  |  
| --- | --- |  
| dev  | Linting & contributions  |  
| hf_transfer  | Speed up HF downloads  |  
| sentencepiece  | Sentencepiece tokenizer  |  
| unitxt  | Unitxt tasks  |  
| wandb  | Weights & Biases logging  |  
| zeno  | Zeno result visualization  |  
## Cite as

```
@misc{eval-harness,
  author       = {Gao, Leo and Tow, Jonathan and Abbasi, Baber and Biderman, Stella and Black, Sid and DiPofi, Anthony and Foster, Charles and Golding, Laurence and Hsu, Jeffrey and Le Noac'h, Alain and Li, Haonan and McDonell, Kyle and Muennighoff, Niklas and Ociepa, Chris and Phang, Jason and Reynolds, Laria and Schoelkopf, Hailey and Skowron, Aviya and Sutawika, Lintang and Tang, Eric and Thite, Anish and Wang, Ben and Wang, Kevin and Zou, Andy},
  title        = {The Language Model Evaluation Harness},
  month        = 07,
  year         = 2024,
  publisher    = {Zenodo},
  version      = {v0.4.3},
  doi          = {10.5281/zenodo.12608602},
  url          = {https://zenodo.org/records/12608602}
}

```

## About
A framework for few-shot evaluation of language models.
### Topics
[evaluation-framework](https://github.com/topics/evaluation-framework)[language-model](https://github.com/topics/language-model)[transformer](https://github.com/topics/transformer)
### Resources
### Contributing
Cite this repository
### Stars
**14.2k** stars
### Watchers
**55** watching
### Forks
[**3.7k** forks](https://github.com/EleutherAI/lm-evaluation-harness/forks)
## 
[ v0.4.13Latest Aug 31, 2026 ](https://github.com/EleutherAI/lm-evaluation-harness/releases/tag/v0.4.13)
## 
## Languages
  * [Python99.5%](https://github.com/EleutherAI/lm-evaluation-harness/search?l=python)
  * [Other0.5%](https://github.com/EleutherAI/lm-evaluation-harness/search?l=Other)


You can’t perform that action at this time. 
