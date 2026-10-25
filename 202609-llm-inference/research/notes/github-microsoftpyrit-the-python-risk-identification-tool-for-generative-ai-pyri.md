---
title: 'GitHub - microsoft/PyRIT: The Python Risk Identification Tool for generative
  AI (PyRIT) is an open source framework built to empower security professionals and
  engineers to proactively identify risks in generative AI systems. · GitHub'
id: github-microsoftpyrit-the-python-risk-identification-tool-for-generative-ai-pyri
tags:
- internal-llm-inference-stacks-7f0c90
- guardrails
- red-teaming
- oss-repo
- oss-governance
created: '2026-10-09T23:07:56.469138Z'
updated: '2026-10-10T14:52:17.226573Z'
source: https://github.com/microsoft/PyRIT
source_domain: github.com
fetched_at: '2026-10-09T23:07:56.443598Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub repo for Microsoft''s PyRIT (Python Risk Identification Tool for
  generative AI), the current home of the project after it was migrated from Azure/PyRIT
  (archived Mar 27 2026). Actively maintained: 4.6k stars, 50 watchers, 2,221 commits,
  35 branches, 248+ network dependents, latest release v1.1.0 (Sep 4 2026), commits
  as recent as Oct 9 2026 with multiple maintainers (including Roman Lutz, Richard
  Lundeen) plus a Copilot App bot co-authoring many PRs. Positioned to empower security
  professionals to proactively identify risks in generative AI systems; publishes
  a 2026 whitepaper, PyRIT: Democratizing AI Red Teaming Through Open-Source Tooling,
  that users are asked to cite. Governance/interop signal directly relevant to how
  open-source red-teaming tools compose: recent merged PRs explicitly add Garak-sourced
  attack scenarios into PyRIT (a Garak prompt injection scenario, a Garak exploitation
  scenario combining Jinja template injection plus SQL injection echo, and a Garak
  latent injection scenario), showing the two most-cited open-source LLM red-teaming
  tools (NVIDIA''s garak and Microsoft''s PyRIT) are being actively cross-integrated
  by their maintainers rather than treated as redundant or competing choices. Also
  shows ongoing feature work on an AIRTInitializer, an ACA rollback connection-wait
  fix, and a GUI with admin-gated reinitialize controls -- indicating PyRIT has grown
  beyond a pure CLI into a GUI-backed platform with its own deployment and rollback
  machinery (Azure Container Apps, per the ACA references).'
---

*Suggested by [[ai-guardrails-implementing-safety-for-production-llm-apps]] — current location of PyRIT after Azure/PyRIT was archived and moved*

[Skip to content](https://github.com/microsoft/PyRIT#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/microsoft/PyRIT) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/microsoft/PyRIT) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/microsoft/PyRIT) to refresh your session. Dismiss alert
[**35** Branches](https://github.com/microsoft/PyRIT/branches)[](https://github.com/microsoft/PyRIT/tags)
Go to file
Open more actions menu
## Latest commit
3 people
[FIX Stop silently ignoring technique_converters (](https://github.com/microsoft/PyRIT/commit/4beda9c08292be50e42cd62539fa1812971bde09)[#3021](https://github.com/microsoft/PyRIT/pull/3021)[)](https://github.com/microsoft/PyRIT/commit/4beda9c08292be50e42cd62539fa1812971bde09)
Open commit detailssuccess
Oct 9, 2026
[4beda9c](https://github.com/microsoft/PyRIT/commit/4beda9c08292be50e42cd62539fa1812971bde09) · Oct 9, 2026
## History
[2,221 Commits](https://github.com/microsoft/PyRIT/commits/main/)
Open commit details
2,221 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [FEAT: Improve adversarial benchmark dataset and scoring (](https://github.com/microsoft/PyRIT/commit/9f4b0c2c8c5681e0ba88ac61c615af13f4359b01 "FEAT: Improve adversarial benchmark dataset and scoring \(#2551\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Co-authored-by: PyRIT Benchmark Bot <pyrit-benchmark-bot@users.noreply.github.com>
Copilot-Session: a4b6deb5-a62a-403d-9ed9-2eb369b5174d")[#2551](https://github.com/microsoft/PyRIT/pull/2551)[)](https://github.com/microsoft/PyRIT/commit/9f4b0c2c8c5681e0ba88ac61c615af13f4359b01 "FEAT: Improve adversarial benchmark dataset and scoring \(#2551\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Co-authored-by: PyRIT Benchmark Bot <pyrit-benchmark-bot@users.noreply.github.com>
Copilot-Session: a4b6deb5-a62a-403d-9ed9-2eb369b5174d")  | Sep 28, 2026  |  
|   |   | [MAINT: Fold GCG into pyrit/executor/promptgen, remove auxiliary_attac…](https://github.com/microsoft/PyRIT/commit/07703764d7fcd66bb774c7d8cce5aff00295e29b "MAINT: Fold GCG into pyrit/executor/promptgen, remove auxiliary_attacks \(#2197\)

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")  | Jul 16, 2026  |  
|   |   |  [TEST: Scorer contract coverage (](https://github.com/microsoft/PyRIT/commit/92e10b2ef6123a7cf7a79f2119e69d6fedda6266 "TEST: Scorer contract coverage \(#3055\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")[#3055](https://github.com/microsoft/PyRIT/pull/3055)[)](https://github.com/microsoft/PyRIT/commit/92e10b2ef6123a7cf7a79f2119e69d6fedda6266 "TEST: Scorer contract coverage \(#3055\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [FEAT: Adding Garak prompt injection scenario (](https://github.com/microsoft/PyRIT/commit/1fe77251faab9b62210f9df4f64b99cfe972c8bc "FEAT: Adding Garak prompt injection scenario \(#2509\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 4dcdc508-9524-4451-8291-45176340e841")[#2509](https://github.com/microsoft/PyRIT/pull/2509)[)](https://github.com/microsoft/PyRIT/commit/1fe77251faab9b62210f9df4f64b99cfe972c8bc "FEAT: Adding Garak prompt injection scenario \(#2509\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 4dcdc508-9524-4451-8291-45176340e841")  | Sep 22, 2026  |  
|   |   |  [DOC: Asset cleanup and notebook-local scorer YAML relocation (](https://github.com/microsoft/PyRIT/commit/f3ce6015028550cd9ca023f458c4741bdb38ee60 "DOC: Asset cleanup and notebook-local scorer YAML relocation \(#2176\)

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")[#2176](https://github.com/microsoft/PyRIT/pull/2176)[)](https://github.com/microsoft/PyRIT/commit/f3ce6015028550cd9ca023f458c4741bdb38ee60 "DOC: Asset cleanup and notebook-local scorer YAML relocation \(#2176\)

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")  | Jul 16, 2026  |  
|   |   |   | Oct 8, 2026  |  
|  [FIX: Accept only supported external inputs when building components (](https://github.com/microsoft/PyRIT/commit/3cd6dc96015a37c01abccafd7fc917f3369e3ff4 "FIX: Accept only supported external inputs when building components \(#2959\)")[#…](https://github.com/microsoft/PyRIT/pull/2959)  | Oct 9, 2026  |  
|   |   |  [[FIX]: Bind GUI Compose publication to localhost (](https://github.com/microsoft/PyRIT/commit/60c3386a0ebcf78a25d62ae76fab8ef52a32090e "\[FIX\]: Bind GUI Compose publication to localhost \(#2963\)

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")[#2963](https://github.com/microsoft/PyRIT/pull/2963)[)](https://github.com/microsoft/PyRIT/commit/60c3386a0ebcf78a25d62ae76fab8ef52a32090e "\[FIX\]: Bind GUI Compose publication to localhost \(#2963\)

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")  | Oct 7, 2026  |  
|   |   |  [FIX: Accept only supported external inputs when building components (](https://github.com/microsoft/PyRIT/commit/3cd6dc96015a37c01abccafd7fc917f3369e3ff4 "FIX: Accept only supported external inputs when building components \(#2959\)")[#…](https://github.com/microsoft/PyRIT/pull/2959)  | Oct 9, 2026  |  
|   |   | [FIX: bound ACA rollback connection wait by deadline instead of attemp…](https://github.com/microsoft/PyRIT/commit/8934a3c495332fd132ec743b9f93669a8b19ec37 "FIX: bound ACA rollback connection wait by deadline instead of attempt count \(#2913\)

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")  | Sep 30, 2026  |  
|   |   |  [FIX Stop silently ignoring technique_converters (](https://github.com/microsoft/PyRIT/commit/4beda9c08292be50e42cd62539fa1812971bde09 "FIX Stop silently ignoring technique_converters \(#3021\)

Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")[#3021](https://github.com/microsoft/PyRIT/pull/3021)[)](https://github.com/microsoft/PyRIT/commit/4beda9c08292be50e42cd62539fa1812971bde09 "FIX Stop silently ignoring technique_converters \(#3021\)

Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [FIX Stop silently ignoring technique_converters (](https://github.com/microsoft/PyRIT/commit/4beda9c08292be50e42cd62539fa1812971bde09 "FIX Stop silently ignoring technique_converters \(#3021\)

Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")[#3021](https://github.com/microsoft/PyRIT/pull/3021)[)](https://github.com/microsoft/PyRIT/commit/4beda9c08292be50e42cd62539fa1812971bde09 "FIX Stop silently ignoring technique_converters \(#3021\)

Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   | [FEAT: Add Garak exploitation scenario (Jinja template injection + SQL…](https://github.com/microsoft/PyRIT/commit/06fd43dbac3c8f40ab47660e52c14098bc5ce32a "FEAT: Add Garak exploitation scenario \(Jinja template injection + SQL injection echo\) \(#2576\)

Co-authored-by: ManoharPaturi <186662190+ManoharPaturi@users.noreply.github.com>
Co-authored-by: Richard Lundeen <rlundeen@microsoft.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Co-authored-by: Richard Lundeen <137218279+richlundeen@users.noreply.github.com>")  | Sep 24, 2026  |  
|   |   |  [MAINT Migration from mypy to ty (](https://github.com/microsoft/PyRIT/commit/4f4d2e08a2c7ec1e1d3147afab0b9a6b35fe6238 "MAINT Migration from mypy to ty \(#1319\)

Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")[#1319](https://github.com/microsoft/PyRIT/pull/1319)[)](https://github.com/microsoft/PyRIT/commit/4f4d2e08a2c7ec1e1d3147afab0b9a6b35fe6238 "MAINT Migration from mypy to ty \(#1319\)

Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")  | Apr 30, 2026  |  
|   |   |  [FEAT: Add a HackAPrompt target (](https://github.com/microsoft/PyRIT/commit/e39d37fd9d8243e3e040af5960bf15d2fb43b1ab "FEAT: Add a HackAPrompt target \(#2801\)

Signed-off-by: Dmitry Voropaev <dy.voropaev@gmail.com>
Co-authored-by: Roman Lutz <romanlutz13@gmail.com>")[#2801](https://github.com/microsoft/PyRIT/pull/2801)[)](https://github.com/microsoft/PyRIT/commit/e39d37fd9d8243e3e040af5960bf15d2fb43b1ab "FEAT: Add a HackAPrompt target \(#2801\)

Signed-off-by: Dmitry Voropaev <dy.voropaev@gmail.com>
Co-authored-by: Roman Lutz <romanlutz13@gmail.com>")  | Sep 25, 2026  |  
|   |   |  [FEAT: Standardizing AIRTInitializer (](https://github.com/microsoft/PyRIT/commit/30dc0b0ca5554d164733b4e26e29cfaf2cd3eec3 "FEAT: Standardizing AIRTInitializer \(#1578\)

Co-authored-by: Victor Valbuena <vvalbuena@microsoft.com>")[#1578](https://github.com/microsoft/PyRIT/pull/1578)[)](https://github.com/microsoft/PyRIT/commit/30dc0b0ca5554d164733b4e26e29cfaf2cd3eec3 "FEAT: Standardizing AIRTInitializer \(#1578\)

Co-authored-by: Victor Valbuena <vvalbuena@microsoft.com>")  | Apr 10, 2026  |  
|   |   |  [[DevContainer] Provide a uniform development environment (](https://github.com/microsoft/PyRIT/commit/71734842d0e742cf37c234878febee0fabee2525 "\[DevContainer\] Provide a uniform development environment \(#787\)

Co-authored-by: Bashir Partovi <bpartovi@microsoft.com>")[#787](https://github.com/microsoft/PyRIT/pull/787)[)](https://github.com/microsoft/PyRIT/commit/71734842d0e742cf37c234878febee0fabee2525 "\[DevContainer\] Provide a uniform development environment \(#787\)

Co-authored-by: Bashir Partovi <bpartovi@microsoft.com>")  | Mar 14, 2025  |  
|   |   |  [[BREAKING] FEAT: Enforce API client compatibility (](https://github.com/microsoft/PyRIT/commit/83e2b79ad359425fd3624ddbc63bc0c9edabb9a5 "\[BREAKING\] FEAT: Enforce API client compatibility \(#2829\)")[#2829](https://github.com/microsoft/PyRIT/pull/2829)[)](https://github.com/microsoft/PyRIT/commit/83e2b79ad359425fd3624ddbc63bc0c9edabb9a5 "\[BREAKING\] FEAT: Enforce API client compatibility \(#2829\)")  | Sep 30, 2026  |  
| [.pre-commit-config.yaml](https://github.com/microsoft/PyRIT/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/microsoft/PyRIT/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |  [MAINT: Bump](https://github.com/microsoft/PyRIT/commit/35ec8879c93513c8fb6eeb273f776e753f0ff791 "MAINT: Bump https://github.com/astral-sh/ruff-pre-commit from v0.16.9 to 0.16.10 \(#3023\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>") <https://github.com/astral-sh/ruff-pre-commit> [from v0.16.9…](https://github.com/microsoft/PyRIT/commit/35ec8879c93513c8fb6eeb273f776e753f0ff791 "MAINT: Bump https://github.com/astral-sh/ruff-pre-commit from v0.16.9 to 0.16.10 \(#3023\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Oct 8, 2026  |  
|   |   |  [FEAT: Allowing Reinitialize in GUI for admins (](https://github.com/microsoft/PyRIT/commit/c32546a1e3d069ea9794df31ded86e17487dcf2a "FEAT: Allowing Reinitialize in GUI for admins \(#2833\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")[#2833](https://github.com/microsoft/PyRIT/pull/2833)[)](https://github.com/microsoft/PyRIT/commit/c32546a1e3d069ea9794df31ded86e17487dcf2a "FEAT: Allowing Reinitialize in GUI for admins \(#2833\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Sep 26, 2026  |  
|   |   |  [DOC: Cite the 2026 PyRIT paper (](https://github.com/microsoft/PyRIT/commit/1fedbc17965456877ce2e949858d7c657c690616 "DOC: Cite the 2026 PyRIT paper \(#2635\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")[#2635](https://github.com/microsoft/PyRIT/pull/2635)[)](https://github.com/microsoft/PyRIT/commit/1fedbc17965456877ce2e949858d7c657c690616 "DOC: Cite the 2026 PyRIT paper \(#2635\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Sep 14, 2026  |  
|   |   | [CODE_OF_CONDUCT.md committed](https://github.com/microsoft/PyRIT/commit/9a17b9e3cf843b8c5b010c51430abcc4bc3ceb25 "CODE_OF_CONDUCT.md committed")  | Dec 12, 2023  |  
|   |   | [Migrate PyRIT to open source repository](https://github.com/microsoft/PyRIT/commit/dc78c6fe26236b3d31058acb34b8a712abd98dbe "Migrate PyRIT to open source repository")  | Jan 26, 2024  |  
|   |   |  [[BREAKING] FEAT: Enforce API client compatibility (](https://github.com/microsoft/PyRIT/commit/83e2b79ad359425fd3624ddbc63bc0c9edabb9a5 "\[BREAKING\] FEAT: Enforce API client compatibility \(#2829\)")[#2829](https://github.com/microsoft/PyRIT/pull/2829)[)](https://github.com/microsoft/PyRIT/commit/83e2b79ad359425fd3624ddbc63bc0c9edabb9a5 "\[BREAKING\] FEAT: Enforce API client compatibility \(#2829\)")  | Sep 30, 2026  |  
|   |   |  [FIX Publish Azure partner integration test results (](https://github.com/microsoft/PyRIT/commit/b605e76f10ed36b66382ed4cd5973b760f25e355 "FIX Publish Azure partner integration test results \(#2901\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 21bf73f6-4b26-4f5f-bb22-01fa72e49186")[#2901](https://github.com/microsoft/PyRIT/pull/2901)[)](https://github.com/microsoft/PyRIT/commit/b605e76f10ed36b66382ed4cd5973b760f25e355 "FIX Publish Azure partner integration test results \(#2901\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 21bf73f6-4b26-4f5f-bb22-01fa72e49186")  | Oct 2, 2026  |  
|   |   | [FEAT: Add Garak exploitation scenario (Jinja template injection + SQL…](https://github.com/microsoft/PyRIT/commit/06fd43dbac3c8f40ab47660e52c14098bc5ce32a "FEAT: Add Garak exploitation scenario \(Jinja template injection + SQL injection echo\) \(#2576\)

Co-authored-by: ManoharPaturi <186662190+ManoharPaturi@users.noreply.github.com>
Co-authored-by: Richard Lundeen <rlundeen@microsoft.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Co-authored-by: Richard Lundeen <137218279+richlundeen@users.noreply.github.com>")  | Sep 24, 2026  |  
|   |   |  [DOC: Cite the 2026 PyRIT paper (](https://github.com/microsoft/PyRIT/commit/1fedbc17965456877ce2e949858d7c657c690616 "DOC: Cite the 2026 PyRIT paper \(#2635\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")[#2635](https://github.com/microsoft/PyRIT/pull/2635)[)](https://github.com/microsoft/PyRIT/commit/1fedbc17965456877ce2e949858d7c657c690616 "DOC: Cite the 2026 PyRIT paper \(#2635\)

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Sep 14, 2026  |  
|   |   |   | Jan 29, 2024  |  
|   |   |   | Aug 8, 2024  |  
| [THIRD_PARTY_NOTICES.txt](https://github.com/microsoft/PyRIT/blob/main/THIRD_PARTY_NOTICES.txt "THIRD_PARTY_NOTICES.txt")  | [THIRD_PARTY_NOTICES.txt](https://github.com/microsoft/PyRIT/blob/main/THIRD_PARTY_NOTICES.txt "THIRD_PARTY_NOTICES.txt")  |  [FEAT: Add Garak latent injection scenario (](https://github.com/microsoft/PyRIT/commit/f98336b7afc69be5405860cb5e6f56c52a4f1499 "FEAT: Add Garak latent injection scenario \(#2573\)

Co-authored-by: Richard Lundeen <rlundeen@microsoft.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")[#2573](https://github.com/microsoft/PyRIT/pull/2573)[)](https://github.com/microsoft/PyRIT/commit/f98336b7afc69be5405860cb5e6f56c52a4f1499 "FEAT: Add Garak latent injection scenario \(#2573\)

Co-authored-by: Richard Lundeen <rlundeen@microsoft.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Sep 23, 2026  |  
|   |   |  [[BREAKING] FEAT: Enforce API client compatibility (](https://github.com/microsoft/PyRIT/commit/83e2b79ad359425fd3624ddbc63bc0c9edabb9a5 "\[BREAKING\] FEAT: Enforce API client compatibility \(#2829\)")[#2829](https://github.com/microsoft/PyRIT/pull/2829)[)](https://github.com/microsoft/PyRIT/commit/83e2b79ad359425fd3624ddbc63bc0c9edabb9a5 "\[BREAKING\] FEAT: Enforce API client compatibility \(#2829\)")  | Sep 30, 2026  |  
|   |   |  [FIX: Fixing policheck bug (](https://github.com/microsoft/PyRIT/commit/d16daa71b05b85dd5ee3379b2352c8d25cac29d6 "FIX: Fixing policheck bug \(#261\)")[#261](https://github.com/microsoft/PyRIT/pull/261)[)](https://github.com/microsoft/PyRIT/commit/d16daa71b05b85dd5ee3379b2352c8d25cac29d6 "FIX: Fixing policheck bug \(#261\)")  | Jun 28, 2024  |  
|   |   |  [FIX Share scenario success statistics across SDK, backend and reports (](https://github.com/microsoft/PyRIT/commit/001a883c9d852e921c31b1ac64078e6beaba649a "FIX Share scenario success statistics across SDK, backend and reports \(#2820\)

Co-authored-by: hannahwestra25 <hannahwestra@microsoft.com>
Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
Co-authored-by: Roman Lutz <romanlutz13@gmail.com>")[…](https://github.com/microsoft/PyRIT/pull/2820)  | Oct 8, 2026  |  
|   |   |  [MAINT Migration from mypy to ty (](https://github.com/microsoft/PyRIT/commit/4f4d2e08a2c7ec1e1d3147afab0b9a6b35fe6238 "MAINT Migration from mypy to ty \(#1319\)

Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")[#1319](https://github.com/microsoft/PyRIT/pull/1319)[)](https://github.com/microsoft/PyRIT/commit/4f4d2e08a2c7ec1e1d3147afab0b9a6b35fe6238 "MAINT Migration from mypy to ty \(#1319\)

Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>")  | Apr 30, 2026  |  
|   |   |  [MAINT: Bump multidict from 6.7.0 to 6.9.1 (](https://github.com/microsoft/PyRIT/commit/c09ecea51849b9ac7e186421c437db2393ceeb12 "MAINT: Bump multidict from 6.7.0 to 6.9.1 \(#3035\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")[#3035](https://github.com/microsoft/PyRIT/pull/3035)[)](https://github.com/microsoft/PyRIT/commit/c09ecea51849b9ac7e186421c437db2393ceeb12 "MAINT: Bump multidict from 6.7.0 to 6.9.1 \(#3035\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Roman Lutz <romanlutz13@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Oct 8, 2026  |  
| View all files |  
## Repository files navigation
# Python Risk Identification Tool for generative AI (PyRIT)
The Python Risk Identification Tool for generative AI (PyRIT) is an open source framework built to empower security professionals and engineers to proactively identify risks in generative AI systems.
  * Check out our [website](https://microsoft.github.io/PyRIT/) for more information about how to use, install, or contribute to PyRIT.
  * Review our [security policy](https://github.com/microsoft/PyRIT/security/policy) to report vulnerabilities privately through the Microsoft Security Response Center.
  * Visit our [Discord server](https://discord.gg/9fMpq3tc8u) to chat with the team and community.


## Trademarks and Citations
This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft trademarks or logos is subject to and must follow [Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general). Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship. Any use of third-party trademarks or logos are subject to those third-party's policies.
If you use PyRIT in your research, please cite our paper, [PyRIT: Democratizing AI Red Teaming Through Open-Source Tooling](https://commandline.microsoft.com/wp-content/uploads/2026/08/PyRIT_Whitepaper_2026.pdf), using the citation in the [`CITATION.cff`](https://github.com/microsoft/PyRIT/blob/main/CITATION.cff) file in the root of this repository.
## About
The Python Risk Identification Tool for generative AI (PyRIT) is an open source framework built to empower security professionals and engineers to proactively identify risks in generative AI systems.
[microsoft.github.io/PyRIT/](https://microsoft.github.io/PyRIT/)
### Topics
[ai-red-team](https://github.com/topics/ai-red-team)[generative-ai](https://github.com/topics/generative-ai)[red-team-tools](https://github.com/topics/red-team-tools)[responsible-ai](https://github.com/topics/responsible-ai)
### Resources
### Code of conduct
### Security policy
Cite this repository
### Stars
**4.6k** stars
### Watchers
**50** watching
### Forks
[ forks](https://github.com/microsoft/PyRIT/forks)
## 
[ v1.1.0Latest Sep 4, 2026 ](https://github.com/microsoft/PyRIT/releases/tag/v1.1.0)
## 
[+ 248](https://github.com/microsoft/PyRIT/network/dependents)
## 
## Languages
  * [Python86%](https://github.com/microsoft/PyRIT/search?l=python)
  * [TypeScript13.6%](https://github.com/microsoft/PyRIT/search?l=typescript)
  * [Shell0.2%](https://github.com/microsoft/PyRIT/search?l=shell)
  * [Bicep0.1%](https://github.com/microsoft/PyRIT/search?l=bicep)
  * [JavaScript0.1%](https://github.com/microsoft/PyRIT/search?l=javascript)
  * [Dockerfile0%](https://github.com/microsoft/PyRIT/search?l=dockerfile)


You can’t perform that action at this time. 
