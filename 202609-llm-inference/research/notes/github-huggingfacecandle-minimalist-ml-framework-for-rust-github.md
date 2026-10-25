---
title: 'GitHub - huggingface/candle: Minimalist ML framework for Rust · GitHub'
id: github-huggingfacecandle-minimalist-ml-framework-for-rust-github
tags:
- llm-inference
- rust
- build-vs-buy
- inference-runtime
- huggingface
- candle
created: '2026-10-09T23:28:31.565929Z'
updated: '2026-10-10T14:52:17.348229Z'
source: https://github.com/huggingface/candle
source_domain: github.com
fetched_at: '2026-10-09T23:28:31.544669Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Candle: Hugging Face''s own minimalist ML framework for Rust (dual Apache-2.0/MIT
  licensed, 21.1k stars) -- the Rust tensor backend underlying TEI and TGI. HF''s
  stated motivation mirrors Cloudflare''s Infire rationale: ''make serverless inference
  possible'' via lightweight binaries (full frameworks like PyTorch are too large/slow
  to instantiate on a cluster) and ''remove Python from production workloads'' because
  Python/GIL overhead hurts performance. HF explicitly built rather than adopted an
  existing Rust ML crate (dfdx, burn, tch-rs all considered/named) partly because
  the HF ecosystem already had Rust crates (safetensors, tokenizers) to build on.'
---

*Suggested by [[github-huggingfacetext-embeddings-inference-a-blazing-fast-inference-solution-fo]] — TEI's Rust tensor/ML backend, built in-house by HF rather than adopted — a direct build-vs-buy artifact*

[Skip to content](https://github.com/huggingface/candle#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/huggingface/candle) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/huggingface/candle) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/huggingface/candle) to refresh your session. Dismiss alert
[**100** Branches](https://github.com/huggingface/candle/branches)[](https://github.com/huggingface/candle/tags)
Go to file
Open more actions menu
## Latest commit
[Tile lhs rows in the generic CPU quantized matmul for prefill (](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a)[#4003](https://github.com/huggingface/candle/pull/4003)[)](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a)
Open commit detailssuccess
Oct 2, 2026
[c68b249](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a) · Oct 2, 2026
## History
[2,725 Commits](https://github.com/huggingface/candle/commits/main/)
Open commit details
2,725 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [Fix wasm build by enabling getrandom wasm_js backend (](https://github.com/huggingface/candle/commit/f1286e6e09c193500c6977f23a16be0d2205b7a1 "Fix wasm build by enabling getrandom wasm_js backend \(#3055\)")[#3055](https://github.com/huggingface/candle/pull/3055)[)](https://github.com/huggingface/candle/commit/f1286e6e09c193500c6977f23a16be0d2205b7a1 "Fix wasm build by enabling getrandom wasm_js backend \(#3055\)")  | Aug 18, 2025  |  
|   |   |  [Pin GitHub Actions to commit SHAs (](https://github.com/huggingface/candle/commit/e65eb1de3cfe41008fc3acc99ed3f1c72a8d250f "Pin GitHub Actions to commit SHAs \(#3996\)

A tag is mutable: whoever controls the action's repository can move
`v4` to different code, and the next run picks it up with no diff to
review. Pinning to the commit SHA freezes the code that runs today.

Co-authored-by: hf-security-analysis\[bot\] <265538906+hf-security-analysis\[bot\]@users.noreply.github.com>")[#3996](https://github.com/huggingface/candle/pull/3996)[)](https://github.com/huggingface/candle/commit/e65eb1de3cfe41008fc3acc99ed3f1c72a8d250f "Pin GitHub Actions to commit SHAs \(#3996\)

A tag is mutable: whoever controls the action's repository can move
`v4` to different code, and the next run picks it up with no diff to
review. Pinning to the commit SHA freezes the code that runs today.

Co-authored-by: hf-security-analysis\[bot\] <265538906+hf-security-analysis\[bot\]@users.noreply.github.com>")  | Sep 23, 2026  |  
|   |   |   | Sep 4, 2026  |  
|   |   |  [Tile lhs rows in the generic CPU quantized matmul for prefill (](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a "Tile lhs rows in the generic CPU quantized matmul for prefill \(#4003\)

* Tile lhs rows in the generic CPU quantized matmul for prefill

k_quants::matmul, the CPU quantized matmul used whenever no repacked
kernel applies, looped over the input rows and dispatched the weight
column quads to the thread pool once per row. With m prefill rows every
weight block was re-read m times and the pool dispatched m times, so
rows/s at m = 128 was the same as at m = 1 \(581 vs 587 rows/s for a
38 MB Q4K 8184x8192 weight on a 4-core Xeon\).

Keep the m == 1 decode path as is and, for m > 1, run a single chunked
dispatch over \(row tile, column quad\) work items, row-tile major, with
tiles of 16 rows \(ggml's src1 row block\). A worker walking its range
keeps the quantized lhs tile in cache and streams each weight quad once
per tile instead of once per row. Every output element is still produced
by the same vec_dot call on the same inputs, only the loop order changes,
so results are bit-identical to the previous code.

For weights larger than the LLC this gives ~1.45x rows/s at m >= 32 on
the generic path \(Q4K 38 MB: 587 -> 833 rows/s, Q8_0 71 MB: 273 -> 396\);
for LLC-resident weights the path is bound by the per-element vec_dot
kernel and the gain is 0-25%. The repacked paths are untouched.

Add a test that checks, for every GGML type and several m, that the
batched output equals the single-row output exactly, and an ignored
timing harness for the prefill shapes.

* Assert dst and lhs lengths in the CPU quantized matmul

k_quants::matmul is a safe fn whose workers write dst through a raw
pointer, so an undersized dst would be written out of bounds from the
pool threads instead of panicking on slice indexing. Check dst.len\(\)
against m * n up front and make the lhs length check a real assert.

* Format the dst length assert

* Drop std/core float module imports that trip the deprecated lint on Rust 1.99

Rust 1.99 deprecates the module-level float constants. With `use std::f64;`
\(or `std::f32`/`core::f32`\) in scope, `f64::INFINITY` and friends resolve
to those deprecated constants, so clippy with -D warnings fails. Removing
the imports makes them resolve to the associated constants instead.")[#4003](https://github.com/huggingface/candle/pull/4003)[)](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a "Tile lhs rows in the generic CPU quantized matmul for prefill \(#4003\)

* Tile lhs rows in the generic CPU quantized matmul for prefill

k_quants::matmul, the CPU quantized matmul used whenever no repacked
kernel applies, looped over the input rows and dispatched the weight
column quads to the thread pool once per row. With m prefill rows every
weight block was re-read m times and the pool dispatched m times, so
rows/s at m = 128 was the same as at m = 1 \(581 vs 587 rows/s for a
38 MB Q4K 8184x8192 weight on a 4-core Xeon\).

Keep the m == 1 decode path as is and, for m > 1, run a single chunked
dispatch over \(row tile, column quad\) work items, row-tile major, with
tiles of 16 rows \(ggml's src1 row block\). A worker walking its range
keeps the quantized lhs tile in cache and streams each weight quad once
per tile instead of once per row. Every output element is still produced
by the same vec_dot call on the same inputs, only the loop order changes,
so results are bit-identical to the previous code.

For weights larger than the LLC this gives ~1.45x rows/s at m >= 32 on
the generic path \(Q4K 38 MB: 587 -> 833 rows/s, Q8_0 71 MB: 273 -> 396\);
for LLC-resident weights the path is bound by the per-element vec_dot
kernel and the gain is 0-25%. The repacked paths are untouched.

Add a test that checks, for every GGML type and several m, that the
batched output equals the single-row output exactly, and an ignored
timing harness for the prefill shapes.

* Assert dst and lhs lengths in the CPU quantized matmul

k_quants::matmul is a safe fn whose workers write dst through a raw
pointer, so an undersized dst would be written out of bounds from the
pool threads instead of panicking on slice indexing. Check dst.len\(\)
against m * n up front and make the lhs length check a real assert.

* Format the dst length assert

* Drop std/core float module imports that trip the deprecated lint on Rust 1.99

Rust 1.99 deprecates the module-level float constants. With `use std::f64;`
\(or `std::f32`/`core::f32`\) in scope, `f64::INFINITY` and friends resolve
to those deprecated constants, so clippy with -D warnings fails. Removing
the imports makes them resolve to the associated constants instead.")  | Oct 2, 2026  |  
|   |   |  [Use fancy-regex feature in tokenizers over onig (](https://github.com/huggingface/candle/commit/5814a6fa6292567378b82de035249cbd1ceafeff "Use fancy-regex feature in tokenizers over onig \(#3952\)

Co-authored-by: fbbbkaya <fzbkaya@gmail.com>")[#3952](https://github.com/huggingface/candle/pull/3952)[)](https://github.com/huggingface/candle/commit/5814a6fa6292567378b82de035249cbd1ceafeff "Use fancy-regex feature in tokenizers over onig \(#3952\)

Co-authored-by: fbbbkaya <fzbkaya@gmail.com>")  | Sep 3, 2026  |  
|   |   |  [Use fancy-regex feature in tokenizers over onig (](https://github.com/huggingface/candle/commit/5814a6fa6292567378b82de035249cbd1ceafeff "Use fancy-regex feature in tokenizers over onig \(#3952\)

Co-authored-by: fbbbkaya <fzbkaya@gmail.com>")[#3952](https://github.com/huggingface/candle/pull/3952)[)](https://github.com/huggingface/candle/commit/5814a6fa6292567378b82de035249cbd1ceafeff "Use fancy-regex feature in tokenizers over onig \(#3952\)

Co-authored-by: fbbbkaya <fzbkaya@gmail.com>")  | Sep 3, 2026  |  
|   |   | [Fix FlashAttention v3 null tile count semaphore, stream, and LSE sizi…](https://github.com/huggingface/candle/commit/2a13b0f3ff62f7e67013597f2996f764c5735e21 "Fix FlashAttention v3 null tile count semaphore, stream, and LSE sizing \(#3892\)

* fix\(flash-attn-v3\): allocate the tile count semaphore

The causal/local/split path selects DynamicPersistentTileScheduler, which
atomicAdds params.tile_count_semaphore. run_mha_v3 memsets the params and
never set it, so the kernel dereferenced a null pointer.

* fix\(flash-attn-v3\): launch on the caller's stream

run_mha_v3 hardcoded the legacy default stream. candle allocates its
streams with cudaStreamNonBlocking, which do not synchronize with it, so
the kernel could race the surrounding ops. Matches candle-flash-attn.

* fix\(flash-attn-v3\): size softmax_lse to b*h*seqlen_q

The buffer was over-allocated 128x. get_lse_gmem_layout is \(b, h, m\) and
the epilogue bounds its stores by actual_seq_len, so the extra factor was
never written. At 16k tokens this reclaims ~3 GB per forward.

* fix\(flash-attn-v3\): stop clobbering the varlen window sizes

FlashAttnVarLen overwrote window_size_left and window_size_right with
max_seqlen_k whenever they were smaller, so the -1/0 sentinels never
survived and is_causal was always 0. Every varlen call ran bidirectional,
silently corrupting output for causal models, and sliding windows were
widened to the full sequence the same way.

* fix\(flash-attn-v3\): keep varlen off the GQA-packed launchers

run_mha_fwd_gqa_ hardcodes FixedSeqLenTraits for every head dim, with no
SEQLEN_SWITCH, so it ignores cu_seqlens and indexes a packed batch by
seqlen_q and the batch stride. With use_gqa_packing set, varlen therefore
read the wrong rows for every sequence past the first.")  | Aug 15, 2026  |  
|   |   |  [Bump candle version to 0.11.0 (](https://github.com/huggingface/candle/commit/31f35b147389700ed2a178ee66a91c3cc25cc80d "Bump candle version to 0.11.0 \(#3658\)")[#3658](https://github.com/huggingface/candle/pull/3658)[)](https://github.com/huggingface/candle/commit/31f35b147389700ed2a178ee66a91c3cc25cc80d "Bump candle version to 0.11.0 \(#3658\)")  | Jun 26, 2026  |  
|   |   |  [fix(kernels): gate the __hmax_nan/__hmin_nan shim (](https://github.com/huggingface/candle/commit/e68b659fd096ed0947855d43b2eaa55634759c56 "fix\(kernels\): gate the __hmax_nan/__hmin_nan shim \(#3909\)

Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#3909](https://github.com/huggingface/candle/pull/3909)[)](https://github.com/huggingface/candle/commit/e68b659fd096ed0947855d43b2eaa55634759c56 "fix\(kernels\): gate the __hmax_nan/__hmin_nan shim \(#3909\)

Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")  | Sep 22, 2026  |  
|   |   |  [fix(metal): surface command buffer errors that occur during execution (](https://github.com/huggingface/candle/commit/091395a1dcc61641f57f55b0a3477435448808f2 "fix\(metal\): surface command buffer errors that occur during execution \(#3862\)

* fix\(metal\): surface command buffer errors that occur during execution

Re-check the status after waiting and report it.

---------
Co-authored-by: ivarflakstad <69173633+ivarflakstad@users.noreply.github.com>")[…](https://github.com/huggingface/candle/pull/3862)  | Aug 14, 2026  |  
|   |   |  [Tile lhs rows in the generic CPU quantized matmul for prefill (](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a "Tile lhs rows in the generic CPU quantized matmul for prefill \(#4003\)

* Tile lhs rows in the generic CPU quantized matmul for prefill

k_quants::matmul, the CPU quantized matmul used whenever no repacked
kernel applies, looped over the input rows and dispatched the weight
column quads to the thread pool once per row. With m prefill rows every
weight block was re-read m times and the pool dispatched m times, so
rows/s at m = 128 was the same as at m = 1 \(581 vs 587 rows/s for a
38 MB Q4K 8184x8192 weight on a 4-core Xeon\).

Keep the m == 1 decode path as is and, for m > 1, run a single chunked
dispatch over \(row tile, column quad\) work items, row-tile major, with
tiles of 16 rows \(ggml's src1 row block\). A worker walking its range
keeps the quantized lhs tile in cache and streams each weight quad once
per tile instead of once per row. Every output element is still produced
by the same vec_dot call on the same inputs, only the loop order changes,
so results are bit-identical to the previous code.

For weights larger than the LLC this gives ~1.45x rows/s at m >= 32 on
the generic path \(Q4K 38 MB: 587 -> 833 rows/s, Q8_0 71 MB: 273 -> 396\);
for LLC-resident weights the path is bound by the per-element vec_dot
kernel and the gain is 0-25%. The repacked paths are untouched.

Add a test that checks, for every GGML type and several m, that the
batched output equals the single-row output exactly, and an ignored
timing harness for the prefill shapes.

* Assert dst and lhs lengths in the CPU quantized matmul

k_quants::matmul is a safe fn whose workers write dst through a raw
pointer, so an undersized dst would be written out of bounds from the
pool threads instead of panicking on slice indexing. Check dst.len\(\)
against m * n up front and make the lhs length check a real assert.

* Format the dst length assert

* Drop std/core float module imports that trip the deprecated lint on Rust 1.99

Rust 1.99 deprecates the module-level float constants. With `use std::f64;`
\(or `std::f32`/`core::f32`\) in scope, `f64::INFINITY` and friends resolve
to those deprecated constants, so clippy with -D warnings fails. Removing
the imports makes them resolve to the associated constants instead.")[#4003](https://github.com/huggingface/candle/pull/4003)[)](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a "Tile lhs rows in the generic CPU quantized matmul for prefill \(#4003\)

* Tile lhs rows in the generic CPU quantized matmul for prefill

k_quants::matmul, the CPU quantized matmul used whenever no repacked
kernel applies, looped over the input rows and dispatched the weight
column quads to the thread pool once per row. With m prefill rows every
weight block was re-read m times and the pool dispatched m times, so
rows/s at m = 128 was the same as at m = 1 \(581 vs 587 rows/s for a
38 MB Q4K 8184x8192 weight on a 4-core Xeon\).

Keep the m == 1 decode path as is and, for m > 1, run a single chunked
dispatch over \(row tile, column quad\) work items, row-tile major, with
tiles of 16 rows \(ggml's src1 row block\). A worker walking its range
keeps the quantized lhs tile in cache and streams each weight quad once
per tile instead of once per row. Every output element is still produced
by the same vec_dot call on the same inputs, only the loop order changes,
so results are bit-identical to the previous code.

For weights larger than the LLC this gives ~1.45x rows/s at m >= 32 on
the generic path \(Q4K 38 MB: 587 -> 833 rows/s, Q8_0 71 MB: 273 -> 396\);
for LLC-resident weights the path is bound by the per-element vec_dot
kernel and the gain is 0-25%. The repacked paths are untouched.

Add a test that checks, for every GGML type and several m, that the
batched output equals the single-row output exactly, and an ignored
timing harness for the prefill shapes.

* Assert dst and lhs lengths in the CPU quantized matmul

k_quants::matmul is a safe fn whose workers write dst through a raw
pointer, so an undersized dst would be written out of bounds from the
pool threads instead of panicking on slice indexing. Check dst.len\(\)
against m * n up front and make the lhs length check a real assert.

* Format the dst length assert

* Drop std/core float module imports that trip the deprecated lint on Rust 1.99

Rust 1.99 deprecates the module-level float constants. With `use std::f64;`
\(or `std::f32`/`core::f32`\) in scope, `f64::INFINITY` and friends resolve
to those deprecated constants, so clippy with -D warnings fails. Removing
the imports makes them resolve to the associated constants instead.")  | Oct 2, 2026  |  
|   |   |  [fix(onnx): LogSoftmax without an axis attribute returned softmax (](https://github.com/huggingface/candle/commit/73fe03819bad0d00b5aab457c57a2e65d259439f "fix\(onnx\): LogSoftmax without an axis attribute returned softmax \(#3975\)

The default-axis branch called softmax_last_dim instead of log_softmax. The
existing test asserted the softmax values, so it is updated to the
log-softmax ones.

Co-authored-by: Eric Buehler <65165915+EricLBuehler@users.noreply.github.com>")[#3975](https://github.com/huggingface/candle/pull/3975)[)](https://github.com/huggingface/candle/commit/73fe03819bad0d00b5aab457c57a2e65d259439f "fix\(onnx\): LogSoftmax without an axis attribute returned softmax \(#3975\)

The default-axis branch called softmax_last_dim instead of log_softmax. The
existing test asserted the softmax values, so it is updated to the
log-softmax ones.

Co-authored-by: Eric Buehler <65165915+EricLBuehler@users.noreply.github.com>")  | Sep 24, 2026  |  
|   |   |  [chore(deps): update pyo3 requirement from 0.27 to 0.29 (](https://github.com/huggingface/candle/commit/4761e00bfb687cc2666e30be7726077f9e2c4456 "chore\(deps\): update pyo3 requirement from 0.27 to 0.29 \(#3929\)

* chore\(deps\): update pyo3 requirement from 0.27 to 0.29

Updates the requirements on \[pyo3\]\(https://github.com/pyo3/pyo3\) to permit the latest version.
- \[Release notes\]\(https://github.com/pyo3/pyo3/releases\)
- \[Changelog\]\(https://github.com/PyO3/pyo3/blob/main/CHANGELOG.md\)
- \[Commits\]\(https://github.com/pyo3/pyo3/compare/v0.27.0...v0.29.2\)

---
updated-dependencies:
- dependency-name: pyo3
  dependency-version: 0.29.2
  dependency-type: direct:production
...

Signed-off-by: dependabot\[bot\] <support@github.com>

* fix bump errors

---------

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Ivar Flakstad <69173633+ivarflakstad@users.noreply.github.com>")[#3929](https://github.com/huggingface/candle/pull/3929)[)](https://github.com/huggingface/candle/commit/4761e00bfb687cc2666e30be7726077f9e2c4456 "chore\(deps\): update pyo3 requirement from 0.27 to 0.29 \(#3929\)

* chore\(deps\): update pyo3 requirement from 0.27 to 0.29

Updates the requirements on \[pyo3\]\(https://github.com/pyo3/pyo3\) to permit the latest version.
- \[Release notes\]\(https://github.com/pyo3/pyo3/releases\)
- \[Changelog\]\(https://github.com/PyO3/pyo3/blob/main/CHANGELOG.md\)
- \[Commits\]\(https://github.com/pyo3/pyo3/compare/v0.27.0...v0.29.2\)

---
updated-dependencies:
- dependency-name: pyo3
  dependency-version: 0.29.2
  dependency-type: direct:production
...

Signed-off-by: dependabot\[bot\] <support@github.com>

* fix bump errors

---------

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Ivar Flakstad <69173633+ivarflakstad@users.noreply.github.com>")  | Aug 31, 2026  |  
|   |   |  [Tile lhs rows in the generic CPU quantized matmul for prefill (](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a "Tile lhs rows in the generic CPU quantized matmul for prefill \(#4003\)

* Tile lhs rows in the generic CPU quantized matmul for prefill

k_quants::matmul, the CPU quantized matmul used whenever no repacked
kernel applies, looped over the input rows and dispatched the weight
column quads to the thread pool once per row. With m prefill rows every
weight block was re-read m times and the pool dispatched m times, so
rows/s at m = 128 was the same as at m = 1 \(581 vs 587 rows/s for a
38 MB Q4K 8184x8192 weight on a 4-core Xeon\).

Keep the m == 1 decode path as is and, for m > 1, run a single chunked
dispatch over \(row tile, column quad\) work items, row-tile major, with
tiles of 16 rows \(ggml's src1 row block\). A worker walking its range
keeps the quantized lhs tile in cache and streams each weight quad once
per tile instead of once per row. Every output element is still produced
by the same vec_dot call on the same inputs, only the loop order changes,
so results are bit-identical to the previous code.

For weights larger than the LLC this gives ~1.45x rows/s at m >= 32 on
the generic path \(Q4K 38 MB: 587 -> 833 rows/s, Q8_0 71 MB: 273 -> 396\);
for LLC-resident weights the path is bound by the per-element vec_dot
kernel and the gain is 0-25%. The repacked paths are untouched.

Add a test that checks, for every GGML type and several m, that the
batched output equals the single-row output exactly, and an ignored
timing harness for the prefill shapes.

* Assert dst and lhs lengths in the CPU quantized matmul

k_quants::matmul is a safe fn whose workers write dst through a raw
pointer, so an undersized dst would be written out of bounds from the
pool threads instead of panicking on slice indexing. Check dst.len\(\)
against m * n up front and make the lhs length check a real assert.

* Format the dst length assert

* Drop std/core float module imports that trip the deprecated lint on Rust 1.99

Rust 1.99 deprecates the module-level float constants. With `use std::f64;`
\(or `std::f32`/`core::f32`\) in scope, `f64::INFINITY` and friends resolve
to those deprecated constants, so clippy with -D warnings fails. Removing
the imports makes them resolve to the associated constants instead.")[#4003](https://github.com/huggingface/candle/pull/4003)[)](https://github.com/huggingface/candle/commit/c68b24997319b8d76e5a4e775dab197c7f26322a "Tile lhs rows in the generic CPU quantized matmul for prefill \(#4003\)

* Tile lhs rows in the generic CPU quantized matmul for prefill

k_quants::matmul, the CPU quantized matmul used whenever no repacked
kernel applies, looped over the input rows and dispatched the weight
column quads to the thread pool once per row. With m prefill rows every
weight block was re-read m times and the pool dispatched m times, so
rows/s at m = 128 was the same as at m = 1 \(581 vs 587 rows/s for a
38 MB Q4K 8184x8192 weight on a 4-core Xeon\).

Keep the m == 1 decode path as is and, for m > 1, run a single chunked
dispatch over \(row tile, column quad\) work items, row-tile major, with
tiles of 16 rows \(ggml's src1 row block\). A worker walking its range
keeps the quantized lhs tile in cache and streams each weight quad once
per tile instead of once per row. Every output element is still produced
by the same vec_dot call on the same inputs, only the loop order changes,
so results are bit-identical to the previous code.

For weights larger than the LLC this gives ~1.45x rows/s at m >= 32 on
the generic path \(Q4K 38 MB: 587 -> 833 rows/s, Q8_0 71 MB: 273 -> 396\);
for LLC-resident weights the path is bound by the per-element vec_dot
kernel and the gain is 0-25%. The repacked paths are untouched.

Add a test that checks, for every GGML type and several m, that the
batched output equals the single-row output exactly, and an ignored
timing harness for the prefill shapes.

* Assert dst and lhs lengths in the CPU quantized matmul

k_quants::matmul is a safe fn whose workers write dst through a raw
pointer, so an undersized dst would be written out of bounds from the
pool threads instead of panicking on slice indexing. Check dst.len\(\)
against m * n up front and make the lhs length check a real assert.

* Format the dst length assert

* Drop std/core float module imports that trip the deprecated lint on Rust 1.99

Rust 1.99 deprecates the module-level float constants. With `use std::f64;`
\(or `std::f32`/`core::f32`\) in scope, `f64::INFINITY` and friends resolve
to those deprecated constants, so clippy with -D warnings fails. Removing
the imports makes them resolve to the associated constants instead.")  | Oct 2, 2026  |  
|   |   |  [chore(deps): update yew-agent requirement from 0.2.0 to 0.5.0 (](https://github.com/huggingface/candle/commit/76ef45d26629ec8664eeb1e11aa4db09d298beca "chore\(deps\): update yew-agent requirement from 0.2.0 to 0.5.0 \(#3939\)

* chore\(deps\): update yew-agent requirement from 0.2.0 to 0.5.0

Updates the requirements on \[yew-agent\]\(https://github.com/yewstack/yew\) to permit the latest version.
- \[Release notes\]\(https://github.com/yewstack/yew/releases\)
- \[Changelog\]\(https://github.com/yewstack/yew/blob/master/CHANGELOG.md\)
- \[Commits\]\(https://github.com/yewstack/yew/compare/yew-agent-v0.2.0...yew-agent-v0.5.0\)

---
updated-dependencies:
- dependency-name: yew-agent
  dependency-version: 0.5.0
  dependency-type: direct:production
...

Signed-off-by: dependabot\[bot\] <support@github.com>

* Update yew-agent usages

---------

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: ivarflakstad <69173633+ivarflakstad@users.noreply.github.com>")[#3939](https://github.com/huggingface/candle/pull/3939)[)](https://github.com/huggingface/candle/commit/76ef45d26629ec8664eeb1e11aa4db09d298beca "chore\(deps\): update yew-agent requirement from 0.2.0 to 0.5.0 \(#3939\)

* chore\(deps\): update yew-agent requirement from 0.2.0 to 0.5.0

Updates the requirements on \[yew-agent\]\(https://github.com/yewstack/yew\) to permit the latest version.
- \[Release notes\]\(https://github.com/yewstack/yew/releases\)
- \[Changelog\]\(https://github.com/yewstack/yew/blob/master/CHANGELOG.md\)
- \[Commits\]\(https://github.com/yewstack/yew/compare/yew-agent-v0.2.0...yew-agent-v0.5.0\)

---
updated-dependencies:
- dependency-name: yew-agent
  dependency-version: 0.5.0
  dependency-type: direct:production
...

Signed-off-by: dependabot\[bot\] <support@github.com>

* Update yew-agent usages

---------

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: ivarflakstad <69173633+ivarflakstad@users.noreply.github.com>")  | Sep 2, 2026  |  
|   |   |  [Fix unused warnings in cande-wasm-tests (](https://github.com/huggingface/candle/commit/c30b26cd3bb59e5b39927eebff00884b0c0729d5 "Fix unused warnings in cande-wasm-tests \(#3946\)")[#3946](https://github.com/huggingface/candle/pull/3946)[)](https://github.com/huggingface/candle/commit/c30b26cd3bb59e5b39927eebff00884b0c0729d5 "Fix unused warnings in cande-wasm-tests \(#3946\)")  | Sep 2, 2026  |  
|   |   |  [Fix clippy warnings for Rust 1.97 (](https://github.com/huggingface/candle/commit/35e1bd7d38e0d73c6e94a768114aff6b42ccee33 "Fix clippy warnings for Rust 1.97 \(#3754\)

* Fix clippy warnings for Rust 1.97

`cargo clippy --fix` for lints promoted to errors on stable 1.97
\(useless_borrows_in_formatting, for_kv_map, manual_filter\).

* Preserve NaN handling in LogitsProcessor::new

f64 is only PartialOrd, so clippy's manual_filter suggestion drops NaN
where the original comparison kept it. Keep the original expression and
allow the lint instead.")[#3754](https://github.com/huggingface/candle/pull/3754)[)](https://github.com/huggingface/candle/commit/35e1bd7d38e0d73c6e94a768114aff6b42ccee33 "Fix clippy warnings for Rust 1.97 \(#3754\)

* Fix clippy warnings for Rust 1.97

`cargo clippy --fix` for lints promoted to errors on stable 1.97
\(useless_borrows_in_formatting, for_kv_map, manual_filter\).

* Preserve NaN handling in LogitsProcessor::new

f64 is only PartialOrd, so clippy's manual_filter suggestion drops NaN
where the original comparison kept it. Keep the original expression and
allow the lint instead.")  | Jul 23, 2026  |  
|   |   |  [Optimization for CPU Causal Flash Attention (integrated into Qwen3) (](https://github.com/huggingface/candle/commit/5447a8779871654e542e65693e7d26f1e33b215f "Optimization for CPU Causal Flash Attention \(integrated into Qwen3\) \(#3254\)

* add flash attn to qwen3

* feature flash-attn

* flash generative true

* no mask in cpu flash

* add causal loop-bound optimization to cpu_flash_attention

* attempt at hybrid

* working tiling

* working but poor performance tiled flash

* factorized attention cpu flash

* logging

* causal cpu flash

* depracation warning

* depracation warning specific

* formatting

* interleaved will live in cpu_flash

* clippy resolve

* AttnMask: remove unnecessary lifetime

* fix: flash-attn KV cache masking during decode

* single batch CPU flash + integrate CPU varlen

* fail B>1 my cpu flash

* benchmark qwen3

* clean up

* varlen force

* dispatch logic smolLM3

* interleaved cpu

* interleaved smollm3

* memory leak

* back to exact exp

* simplfy

* cargo fmt

* clippy

* errors; fmt

* cpu flash comment

* remove bench script

* merge main + resolve; default CPU flash \(note that quantized varlen CPU flash not implemented\) and remove --use-flash-attn CLI \(note there is no path to standard attention for qwen3 and quantized qwen3\)

* debug cfg lazylock only

* remove unused cli

* programatic commentary address

* proposed comments for internal use

---------

Co-authored-by: michaelfeil <me@michaelfeil.eu>
Co-authored-by: ivarflakstad <69173633+ivarflakstad@users.noreply.github.com>")[#…](https://github.com/huggingface/candle/pull/3254)  | Apr 23, 2026  |  
| [.pre-commit-config.yaml](https://github.com/huggingface/candle/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/huggingface/candle/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |   | Jun 22, 2023  |  
|   |   |   | Dec 17, 2023  |  
|   |   |   | Sep 24, 2026  |  
|   |   |   | Aug 7, 2023  |  
|   |   |   | Aug 7, 2023  |  
|   |   |   | Sep 2, 2023  |  
|   |   |  [Initial integration of cutile and fused MoE kernel (](https://github.com/huggingface/candle/commit/e3f026d2b05266921b2e526ac32ff2ef7a61a46e "Initial integration of cutile and fused MoE kernel \(#3934\)

* Initial integration of cutile and fused MoE kernel

* Improve example a bit

* Fix overlap with ci runners

---------

Co-authored-by: ivarflakstad <69173633+ivarflakstad@users.noreply.github.com>")[#3934](https://github.com/huggingface/candle/pull/3934)[)](https://github.com/huggingface/candle/commit/e3f026d2b05266921b2e526ac32ff2ef7a61a46e "Initial integration of cutile and fused MoE kernel \(#3934\)

* Initial integration of cutile and fused MoE kernel

* Improve example a bit

* Fix overlap with ci runners

---------

Co-authored-by: ivarflakstad <69173633+ivarflakstad@users.noreply.github.com>")  | Sep 2, 2026  |  
| View all files |  
## Repository files navigation
# candle
Candle is a minimalist ML framework for Rust with a focus on performance (including GPU support) and ease of use. Try our online demos: [whisper](https://huggingface.co/spaces/lmz/candle-whisper), [LLaMA2](https://huggingface.co/spaces/lmz/candle-llama2), [T5](https://huggingface.co/spaces/radames/Candle-T5-Generation-Wasm), [yolo](https://huggingface.co/spaces/lmz/candle-yolo), [Segment Anything](https://huggingface.co/spaces/radames/candle-segment-anything-wasm).
## Get started
Make sure that you have [`candle-core`](https://github.com/huggingface/candle/tree/main/candle-core) correctly installed as described in [**Installation**](https://huggingface.github.io/candle/guide/installation.html).
Let's see how to run a simple matrix multiplication. Write the following to your `myapp/src/main.rs` file:

```
use candle_core::{Device, Tensor};

fn main() -> Result(), Boxdyn std::error::Error {
    let device = Device::Cpu;

    let a = Tensor::randn(0f32, 1., (2, 3), device)?;
    let b = Tensor::randn(0f32, 1., (3, 4), device)?;

    let c = a.matmul(b)?;
    println!("{c}");
    Ok(())
}
```

`cargo run` should display a tensor of shape `Tensor[[2, 4], f32]`.
Having installed `candle` with Cuda support, simply define the `device` to be on GPU:

```
- let device = Device::Cpu;
+ let device = Device::new_cuda(0)?;
```

For more advanced examples, please have a look at the following section.
## Check out our examples
These online demos run entirely in your browser:
  * [yolo](https://huggingface.co/spaces/lmz/candle-yolo): pose estimation and object recognition.
  * [whisper](https://huggingface.co/spaces/lmz/candle-whisper): speech recognition.
  * [LLaMA2](https://huggingface.co/spaces/lmz/candle-llama2): text generation.
  * [T5](https://huggingface.co/spaces/radames/Candle-T5-Generation-Wasm): text generation.
  * [Phi-1.5, and Phi-2](https://huggingface.co/spaces/radames/Candle-Phi-1.5-Wasm): text generation.
  * [Segment Anything Model](https://huggingface.co/spaces/radames/candle-segment-anything-wasm): Image segmentation.
  * [BLIP](https://huggingface.co/spaces/radames/Candle-BLIP-Image-Captioning): image captioning.


We also provide some command line based examples using state of the art models:
  * [LLaMA v1, v2, and v3](https://github.com/huggingface/candle/blob/main/candle-examples/examples/llama): general LLM, includes the SOLAR-10.7B variant.
  * [Falcon](https://github.com/huggingface/candle/blob/main/candle-examples/examples/falcon): general LLM.
  * [Codegeex4](https://github.com/huggingface/candle/blob/main/candle-examples/examples/codegeex4-9b): Code completion, code interpreter, web search, function calling, repository-level
  * [GLM4](https://github.com/huggingface/candle/blob/main/candle-examples/examples/glm4): Open Multilingual Multimodal Chat LMs by THUDM
  * [Gemma v1 and v2](https://github.com/huggingface/candle/blob/main/candle-examples/examples/gemma): 2b and 7b+/9b general LLMs from Google Deepmind.
  * [RecurrentGemma](https://github.com/huggingface/candle/blob/main/candle-examples/examples/recurrent-gemma): 2b and 7b Griffin based models from Google that mix attention with a RNN like state.
  * [Phi-1, Phi-1.5, Phi-2, and Phi-3](https://github.com/huggingface/candle/blob/main/candle-examples/examples/phi): 1.3b, 2.7b, and 3.8b general LLMs with performance on par with 7b models.
  * [StableLM-3B-4E1T](https://github.com/huggingface/candle/blob/main/candle-examples/examples/stable-lm): a 3b general LLM pre-trained on 1T tokens of English and code datasets. Also supports StableLM-2, a 1.6b LLM trained on 2T tokens, as well as the code variants.
  * [Mamba](https://github.com/huggingface/candle/blob/main/candle-examples/examples/mamba): an inference only implementation of the Mamba state space model.
  * [Mistral7b-v0.1](https://github.com/huggingface/candle/blob/main/candle-examples/examples/mistral): a 7b general LLM with better performance than all publicly available 13b models as of 2023-09-28.
  * [Mixtral8x7b-v0.1](https://github.com/huggingface/candle/blob/main/candle-examples/examples/mixtral): a sparse mixture of experts 8x7b general LLM with better performance than a Llama 2 70B model with much faster inference.
  * [StarCoder](https://github.com/huggingface/candle/blob/main/candle-examples/examples/bigcode) and [StarCoder2](https://github.com/huggingface/candle/blob/main/candle-examples/examples/starcoder2): LLM specialized to code generation.
  * [Qwen1.5](https://github.com/huggingface/candle/blob/main/candle-examples/examples/qwen): Bilingual (English/Chinese) LLMs.
  * [RWKV v5 and v6](https://github.com/huggingface/candle/blob/main/candle-examples/examples/rwkv): An RNN with transformer level LLM performance.
  * [Replit-code-v1.5](https://github.com/huggingface/candle/blob/main/candle-examples/examples/replit-code): a 3.3b LLM specialized for code completion.
  * [Yi-6B / Yi-34B](https://github.com/huggingface/candle/blob/main/candle-examples/examples/yi): two bilingual (English/Chinese) general LLMs with 6b and 34b parameters.
  * [Quantized LLaMA](https://github.com/huggingface/candle/blob/main/candle-examples/examples/quantized): quantized version of the LLaMA model using the same quantization techniques as [llama.cpp](https://github.com/ggerganov/llama.cpp).
  * [Quantized Qwen3 MoE](https://github.com/huggingface/candle/blob/main/candle-examples/examples/quantized-qwen3-moe): support gguf quantized models of Qwen3 MoE models.


  * [Stable Diffusion](https://github.com/huggingface/candle/blob/main/candle-examples/examples/stable-diffusion): text to image generative model, support for the 1.5, 2.1, SDXL 1.0 and Turbo versions.


  * [Wuerstchen](https://github.com/huggingface/candle/blob/main/candle-examples/examples/wuerstchen): another text to image generative model.


  * [yolo-v3](https://github.com/huggingface/candle/blob/main/candle-examples/examples/yolo-v3) and [yolo-v8](https://github.com/huggingface/candle/blob/main/candle-examples/examples/yolo-v8): object detection and pose estimation models.


  * [segment-anything](https://github.com/huggingface/candle/blob/main/candle-examples/examples/segment-anything): image segmentation model with prompt.


  * [SegFormer](https://github.com/huggingface/candle/blob/main/candle-examples/examples/segformer): transformer based semantic segmentation model.
  * [Whisper](https://github.com/huggingface/candle/blob/main/candle-examples/examples/whisper): speech recognition model.
  * [EnCodec](https://github.com/huggingface/candle/blob/main/candle-examples/examples/encodec): high-quality audio compression model using residual vector quantization.
  * [MetaVoice](https://github.com/huggingface/candle/blob/main/candle-examples/examples/metavoice): foundational model for text-to-speech.
  * [Parler-TTS](https://github.com/huggingface/candle/blob/main/candle-examples/examples/parler-tts): large text-to-speech model.
  * [T5](https://github.com/huggingface/candle/blob/main/candle-examples/examples/t5), [Bert](https://github.com/huggingface/candle/blob/main/candle-examples/examples/bert), [JinaBert](https://github.com/huggingface/candle/blob/main/candle-examples/examples/jina-bert) : useful for sentence embeddings.
  * [DINOv2](https://github.com/huggingface/candle/blob/main/candle-examples/examples/dinov2): computer vision model trained using self-supervision (can be used for imagenet classification, depth evaluation, segmentation).
  * [VGG](https://github.com/huggingface/candle/blob/main/candle-examples/examples/vgg), [RepVGG](https://github.com/huggingface/candle/blob/main/candle-examples/examples/repvgg): computer vision models.
  * [BLIP](https://github.com/huggingface/candle/blob/main/candle-examples/examples/blip): image to text model, can be used to generate captions for an image.
  * [CLIP](https://github.com/huggingface/candle/blob/main/candle-examples/examples/clip): multi-model vision and language model.
  * [TrOCR](https://github.com/huggingface/candle/blob/main/candle-examples/examples/trocr): a transformer OCR model, with dedicated submodels for hand-writing and printed recognition.
  * [Marian-MT](https://github.com/huggingface/candle/blob/main/candle-examples/examples/marian-mt): neural machine translation model, generates the translated text from the input text.
  * [Moondream](https://github.com/huggingface/candle/blob/main/candle-examples/examples/moondream): tiny computer-vision model that can answer real-world questions about images.


Run them using commands like:

```
cargo run --example quantized --release

```

In order to use **CUDA** add `--features cuda` to the example command line. If you have cuDNN installed, use `--features cudnn` for even more speedups. The opt-in `cutile` feature supports writing JIT-compiled CUDA kernels in Rust; see the [cuTile guide](https://github.com/huggingface/candle/blob/main/candle-book/src/guide/cutile.md) for setup and interop details.
There are also some wasm examples for whisper and [llama2.c](https://github.com/karpathy/llama2.c). You can either build them with `trunk` or try them online: [whisper](https://huggingface.co/spaces/lmz/candle-whisper), [llama2](https://huggingface.co/spaces/lmz/candle-llama2), [T5](https://huggingface.co/spaces/radames/Candle-T5-Generation-Wasm), [Phi-1.5, and Phi-2](https://huggingface.co/spaces/radames/Candle-Phi-1.5-Wasm), [Segment Anything Model](https://huggingface.co/spaces/radames/candle-segment-anything-wasm).
For LLaMA2, run the following command to retrieve the weight files and start a test server:

```
# install target platform 'wasm32-unknown-unknown'
rustup target add wasm32-unknown-unknown

cd candle-wasm-examples/llama2-c
wget https://huggingface.co/spaces/lmz/candle-llama2/resolve/main/model.bin
wget https://huggingface.co/spaces/lmz/candle-llama2/resolve/main/tokenizer.json
trunk serve --release --port 8081
```

And then head over to <http://localhost:8081/>.
## Useful External Resources
  * [`candle-tutorial`](https://github.com/ToluClassics/candle-tutorial): A very detailed tutorial showing how to convert a PyTorch model to Candle.
  * [`candle-lora`](https://github.com/EricLBuehler/candle-lora): Efficient and ergonomic LoRA implementation for Candle. `candle-lora` has out-of-the-box LoRA support for many models from Candle, which can be found [here](https://github.com/EricLBuehler/candle-lora/tree/master/candle-lora-transformers/examples).
  * [`candle-video`](https://github.com/FerrisMind/candle-video): Rust library for text-to-video generation (LTX-Video and related models) built on Candle, focused on fast, Python-free inference.
  * [`optimisers`](https://github.com/KGrewal1/optimisers): A collection of optimisers including SGD with momentum, AdaGrad, AdaDelta, AdaMax, NAdam, RAdam, and RMSprop.
  * [`candle-vllm`](https://github.com/EricLBuehler/candle-vllm): Efficient platform for inference and serving local LLMs including an OpenAI compatible API server.
  * [`candle-ext`](https://github.com/mokeyish/candle-ext): An extension library to Candle that provides PyTorch functions not currently available in Candle.
  * [`candle-coursera-ml`](https://github.com/vishpat/candle-coursera-ml): Implementation of ML algorithms from Coursera's [Machine Learning Specialization](https://www.coursera.org/specializations/machine-learning-introduction) course.
  * [`kalosm`](https://github.com/floneum/floneum/tree/master/interfaces/kalosm): A multi-modal meta-framework in Rust for interfacing with local pre-trained models with support for controlled generation, custom samplers, in-memory vector databases, audio transcription, and more.
  * [`candle-sampling`](https://github.com/EricLBuehler/candle-sampling): Sampling techniques for Candle.
  * [`gpt-from-scratch-rs`](https://github.com/jeroenvlek/gpt-from-scratch-rs): A port of Andrej Karpathy's _Let's build GPT_ tutorial on YouTube showcasing the Candle API on a toy problem.
  * [`candle-einops`](https://github.com/tomsanbear/candle-einops): A pure rust implementation of the python [einops](https://github.com/arogozhnikov/einops) library.
  * [`atoma-infer`](https://github.com/atoma-network/atoma-infer): A Rust library for fast inference at scale, leveraging FlashAttention2 for efficient attention computation, PagedAttention for efficient KV-cache memory management, and multi-GPU support. It is OpenAI api compatible.
  * [`llms-from-scratch-rs`](https://github.com/nerdai/llms-from-scratch-rs): A comprehensive Rust translation of the code from Sebastian Raschka's Build an LLM from Scratch book.
  * [`vllm.rs`](https://github.com/guoqingbao/vllm.rs): A minimalist vLLM implementation in Rust based on Candle.


If you have an addition to this list, please submit a pull request.
## Features
  * Simple syntax, looks and feels like PyTorch. 
    * Model training.
    * Embed user-defined ops/kernels, such as [flash-attention v2](https://github.com/huggingface/candle/blob/89ba005962495f2bfbda286e185e9c3c7f5300a3/candle-flash-attn/src/lib.rs#L152).
  * Backends. 
    * Optimized CPU backend with optional MKL support for x86 and Accelerate for macs.
    * CUDA backend for efficiently running on GPUs, multiple GPU distribution via NCCL.
    * WASM support, run your models in a browser.
  * Included models. 
    * Language Models. 
      * LLaMA v1, v2, and v3 with variants such as SOLAR-10.7B.
      * Falcon.
      * StarCoder, StarCoder2.
      * Phi 1, 1.5, 2, and 3.
      * Mamba, Minimal Mamba
      * Gemma v1 2b and 7b+, v2 2b and 9b.
      * Mistral 7b v0.1.
      * Mixtral 8x7b v0.1.
      * StableLM-3B-4E1T, StableLM-2-1.6B, Stable-Code-3B.
      * Replit-code-v1.5-3B.
      * Bert.
      * Yi-6B and Yi-34B.
      * Qwen1.5, Qwen1.5 MoE, Qwen3 MoE.
      * RWKV v5 and v6.
    * Quantized LLMs. 
      * Llama 7b, 13b, 70b, as well as the chat and code variants.
      * Mistral 7b, and 7b instruct.
      * Mixtral 8x7b.
      * Zephyr 7b a and b (Mistral-7b based).
      * OpenChat 3.5 (Mistral-7b based).
      * Qwen3 MoE (16B-A3B, 32B-A3B)
    * Text to text. 
      * T5 and its variants: FlanT5, UL2, MADLAD400 (translation), CoEdit (Grammar correction).
      * Marian MT (Machine Translation).
    * Text to image. 
      * Stable Diffusion v1.5, v2.1, XL v1.0.
      * Wurstchen v2.
    * Image to text. 
      * BLIP.
      * TrOCR.
    * Audio. 
      * Whisper, multi-lingual speech-to-text.
      * EnCodec, audio compression model.
      * MetaVoice-1B, text-to-speech model.
      * Parler-TTS, text-to-speech model.
    * Computer Vision Models. 
      * DINOv2, ConvMixer, EfficientNet, ResNet, ViT, VGG, RepVGG, ConvNeXT, ConvNeXTv2, MobileOne, EfficientVit (MSRA), MobileNetv4, Hiera, FastViT.
      * yolo-v3, yolo-v8.
      * Segment-Anything Model (SAM).
      * SegFormer.
  * File formats: load models from safetensors, npz, ggml, or PyTorch files.
  * Serverless (on CPU), small and fast deployments.
  * Quantization support using the llama.cpp quantized types.


## How to use
Cheatsheet:  
| Using PyTorch  | Using Candle  |  
| --- | --- |  
| Creation  | `torch.Tensor([[1, 2], [3, 4]])`  | `Tensor::new(&[[1f32, 2.], [3., 4.]], &Device::Cpu)?`  |  
| Creation  | `torch.zeros((2, 2))`  | `Tensor::zeros((2, 2), DType::F32, &Device::Cpu)?`  |  
| Indexing  | `tensor[:, :4]`  | `tensor.i((.., ..4))?`  |  
| Operations  | `tensor.view((2, 2))`  | `tensor.reshape((2, 2))?`  |  
| Operations  | `a.matmul(b)`  | `a.matmul(&b)?`  |  
| Arithmetic  | `a + b`  | `&a + &b`  |  
| Device  | `tensor.to(device="cuda")`  | `tensor.to_device(&Device::new_cuda(0)?)?`  |  
| Dtype  | `tensor.to(dtype=torch.float16)`  | `tensor.to_dtype(&DType::F16)?`  |  
| Saving  | `torch.save({"A": A}, "model.bin")`  | `candle::safetensors::save(&HashMap::from([("A", A)]), "model.safetensors")?`  |  
| Loading  | `weights = torch.load("model.bin")`  | `candle::safetensors::load("model.safetensors", &device)`  |  
## Structure
  * [candle-core](https://github.com/huggingface/candle/blob/main/candle-core): Core ops, devices, and `Tensor` struct definition
  * [candle-nn](https://github.com/huggingface/candle/blob/main/candle-nn): Tools to build real models
  * [candle-examples](https://github.com/huggingface/candle/blob/main/candle-examples): Examples of using the library in realistic settings
  * [candle-kernels](https://github.com/huggingface/candle/blob/main/candle-kernels): CUDA custom kernels
  * [candle-datasets](https://github.com/huggingface/candle/blob/main/candle-datasets): Datasets and data loaders.
  * [candle-transformers](https://github.com/huggingface/candle/blob/main/candle-transformers): transformers-related utilities.
  * [candle-flash-attn](https://github.com/huggingface/candle/blob/main/candle-flash-attn): Flash attention v2 layer.
  * [candle-onnx](https://github.com/huggingface/candle/blob/main/candle-onnx): ONNX model evaluation.


### Why should I use Candle?
Candle's core goal is to _make serverless inference possible_. Full machine learning frameworks like PyTorch are very large, which makes creating instances on a cluster slow. Candle allows deployment of lightweight binaries.
Secondly, Candle lets you _remove Python_ from production workloads. Python overhead can seriously hurt performance, and the [GIL](https://www.backblaze.com/blog/the-python-gil-past-present-and-future/) is a notorious source of headaches.
Finally, Rust is cool! A lot of the HF ecosystem already has Rust crates, like [safetensors](https://github.com/huggingface/safetensors) and [tokenizers](https://github.com/huggingface/tokenizers).
### Other ML frameworks
  * [dfdx](https://github.com/coreylowman/dfdx) is a formidable crate, with shapes being included in types. This prevents a lot of headaches by getting the compiler to complain about shape mismatches right off the bat. However, we found that some features still require nightly, and writing code can be a bit daunting for non rust experts.
We're leveraging and contributing to other core crates for the runtime so hopefully both crates can benefit from each other.
  * [burn](https://github.com/burn-rs/burn) is a general crate that can leverage multiple backends so you can choose the best engine for your workload.
  * [tch-rs](https://github.com/LaurentMazare/tch-rs.git) Bindings to the torch library in Rust. Extremely versatile, but they bring in the entire torch library into the runtime. The main contributor of `tch-rs` is also involved in the development of `candle`.


### Common Errors
#### Missing symbols when compiling with the mkl feature.
If you get some missing symbols when compiling binaries/tests using the mkl or accelerate features, e.g. for mkl you get:

```
  = note: /usr/bin/ld: (....o): in function `blas::sgemm':
          .../blas-0.22.0/src/lib.rs:1944: undefined reference to `sgemm_' collect2: error: ld returned 1 exit status

  = note: some `extern` functions couldn't be found; some native libraries may need to be installed or have their path specified
  = note: use the `-l` flag to specify native libraries to link
  = note: use the `cargo:rustc-link-lib` directive to specify the native libraries to link with Cargo

```

or for accelerate:

```
Undefined symbols for architecture arm64:
            "_dgemm_", referenced from:
                candle_core::accelerate::dgemm::h1b71a038552bcabe in libcandle_core...
            "_sgemm_", referenced from:
                candle_core::accelerate::sgemm::h2cf21c592cba3c47 in libcandle_core...
          ld: symbol(s) not found for architecture arm64

```

This is likely due to a missing linker flag that was needed to enable the mkl library. You can try adding the following for mkl at the top of your binary:

```
extern crate intel_mkl_src;
```

or for accelerate:

```
extern crate accelerate_src;
```

#### Cannot run the LLaMA examples: access to source requires login credentials

```
Error: request error: https://huggingface.co/meta-llama/Llama-2-7b-hf/resolve/main/tokenizer.json: status code 401

```

This is likely because you're not permissioned for the LLaMA-v2 model. To fix this, you have to register on the huggingface-hub, accept the [LLaMA-v2 model conditions](https://huggingface.co/meta-llama/Llama-2-7b-hf), and set up your authentication token. See issue [#350](https://github.com/huggingface/candle/issues/350) for more details.
#### Docker build
When building CUDA kernels inside a Dockerfile, nvidia-smi cannot be used to auto-detect compute capability.
You must explicitly set CUDA_COMPUTE_CAP, for example:

```
FROM nvidia/cuda:12.9.0-devel-ubuntu22.04

# Install git and curl
RUN set -eux; \
  apt-get update; \
  apt-get install -y curl git ca-certificates;

# Install Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# Clone candle repo
RUN git clone https://github.com/huggingface/candle.git

# Set compute capability for the build
ARG CUDA_COMPUTE_CAP=90
ENV CUDA_COMPUTE_CAP=${CUDA_COMPUTE_CAP}

# Build with explicit compute cap
WORKDIR /app
COPY . .
RUN cargo build --release features cuda

```

#### Compiling with flash-attention fails

```
/usr/include/c++/11/bits/std_function.h:530:146: error: parameter packs not expanded with ‘...’:

```

This is a bug in gcc-11 triggered by the Cuda compiler. To fix this, install a different, supported gcc version - for example gcc-10, and specify the path to the compiler in the NVCC_CCBIN environment variable.

```
env NVCC_CCBIN=/usr/lib/gcc/x86_64-linux-gnu/10 cargo ...

```

#### Linking error on windows when running rustdoc or mdbook tests

```
Couldn't compile the test.
---- .\candle-book\src\inference\hub.md - Using_the_hub::Using_in_a_real_model_ (line 50) stdout ----
error: linking with `link.exe` failed: exit code: 1181
//very long chain of linking
 = note: LINK : fatal error LNK1181: cannot open input file 'windows.0.48.5.lib'

```

Make sure you link all native libraries that might be located outside a project target, e.g., to run mdbook tests, you should run:

```
mdbook test candle-book -L .\target\debug\deps\ `
-L native=$env:USERPROFILE\.cargo\registry\src\index.crates.io-6f17d22bba15001f\windows_x86_64_msvc-0.42.2\lib `
-L native=$env:USERPROFILE\.cargo\registry\src\index.crates.io-6f17d22bba15001f\windows_x86_64_msvc-0.48.5\lib

```

#### Extremely slow model load time with WSL
This may be caused by the models being loaded from `/mnt/c`, more details on [stackoverflow](https://stackoverflow.com/questions/68972448/why-is-wsl-extremely-slow-when-compared-with-native-windows-npm-yarn-processing).
#### Tracking down errors
You can set `RUST_BACKTRACE=1` to be provided with backtraces when a candle error is generated.
#### CudaRC error
If you encounter an error like this one `called `Result::unwrap()`on an`Err` value: LoadLibraryExW { source: Os { code: 126, kind: Uncategorized, message: "The specified module could not be found." } }` on windows. To fix copy and rename these 3 files (make sure they are in path). The paths depend on your cuda version. `c:\Windows\System32\nvcuda.dll` -> `cuda.dll` `c:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4\bin\cublas64_12.dll` -> `cublas.dll` `c:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4\bin\curand64_10.dll` -> `curand.dll`
## About
Minimalist ML framework for Rust
### Resources
Apache-2.0, MIT licenses found
### Stars
**21.1k** stars
### Watchers
**166** watching
### Forks
[**1.8k** forks](https://github.com/huggingface/candle/forks)
## 
[22tags](https://github.com/huggingface/candle/tags)
## [Used by](https://github.com/huggingface/candle/network/dependents)3.7K (3.7K)
[+ 3,729](https://github.com/huggingface/candle/network/dependents)
## 
## Languages
  * [Rust75.2%](https://github.com/huggingface/candle/search?l=rust)
  * [Cuda9.1%](https://github.com/huggingface/candle/search?l=cuda)
  * [Metal6.1%](https://github.com/huggingface/candle/search?l=metal)
  * [C++6%](https://github.com/huggingface/candle/search?l=c%2B%2B)
  * [HTML1.6%](https://github.com/huggingface/candle/search?l=html)
  * [Python1.5%](https://github.com/huggingface/candle/search?l=python)
  * [Other0.5%](https://github.com/huggingface/candle/search?l=Other)


You can’t perform that action at this time. 
