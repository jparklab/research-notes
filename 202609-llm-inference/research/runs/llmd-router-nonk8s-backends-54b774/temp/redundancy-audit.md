# Redundancy audit — 118 claims files, 542 claims

Method: 6-word shingles over each source's quoted_support passages; overlap = |A∩B| / min(|A|,|B|). Threshold for "derivative" = 0.60; 0.25-0.60 listed as partial overlap.


## Pairs ≥0.60 (derivative candidates)

- 0.64 — github-llm-dllm-d-router-llm-d-router-the-intelligent-entry-point-for-inferenc-2 ↔ github-llm-dllm-d-router-llm-d-router-the-intelligent-entry-point-for-inference

## Pairs 0.25–0.60 (partial overlap, usually same upstream doc quoted by two fetchers)

- none

## Conclusion
- Single derivative cluster: two byte-identical captures (12,842 words each) of the llm-d/llm-d-router GitHub landing
  page — one fetched via the old llm-d-inference-scheduler URL (redirects), one directly. Canonical = `github-llm-dllm-d-router-llm-d-router-the-intelligent-entry-point-for-inferenc-2` (it has
  the source-analysis note `source-analysis-llm-d-router-github-repo-readme-commit-history`); `github-llm-dllm-d-router-llm-d-router-the-intelligent-entry-point-for-inference` tagged
  `derivative-of` and discounted in coverage counts.
- No other pair exceeds 0.25 shingle overlap: fetchers quoted distinct passages, and batches were non-overlapping.
- Adjusted independent coverage: every atomic item still has ≥4 independent sources (see coverage-gaps.md). No item
  drops below 2 → no wave 3 needed.
