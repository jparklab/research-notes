---
title: main
id: main
tags:
- llmd-router-nonk8s-backends-54b774
- out-of-tree-plugin-support
- no-internal-path-blocker
created: '2026-09-11T21:47:01.419177Z'
updated: '2026-09-15T04:55:23.955913Z'
source: https://api.github.com/repos/llm-d/llm-d-router/git/trees/main?recursive=1
source_domain: api.github.com
fetched_at: '2026-09-11T21:47:01.396605Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: GitHub Trees API recursive listing of llm-d/llm-d-router@main (1819 paths,
  fetched Sep 11 2026). Confirms structurally that (1) cmd/epp/runner/{runner.go,rungroup.go,health.go,...}
  exists as a normal (non-internal) importable package -- it is a sibling of cmd/coordinator
  and cmd/pd-sidecar, none gated behind internal/; (2) pkg/epp/framework/interface/plugin/{registry.go,handle.go,plugins.go,typedname.go,stability.go,...}
  and pkg/epp/framework/interface/datalayer/{discovery.go,plugin.go} are likewise
  NOT under any internal/ path -- so nothing in the module structurally BLOCKS an
  out-of-tree Go module from importing either package; the only internal/ paths found
  near this code are the unrelated github.com/llm-d/llm-d-router/internal/runnable
  (used internally by runner.go itself, not a blocker for external importers of runner.go)
  and pkg/epp/flowcontrol/controller/internal. Also shows pkg/epp/framework/plugins/datalayer/discovery/file/{plugin.go,multicluster.go,README.md}
  as the sole first-party discovery-backend implementation checked into the tree,
  and cmd/epp/runner/{resolve_discovery_test.go,run_with_file_discovery_test.go} as
  the test coverage for the discovery-plugin resolution path.
---

```
{
  "sha": "6493998a3735ccf1b07450fe6308c9ec5dd929e1",
  "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6493998a3735ccf1b07450fe6308c9ec5dd929e1",
  "tree": [
    {
      "path": ".dockerignore",
      "mode": "100644",
      "type": "blob",
      "sha": "73ae7ffbe1370c6fbbc7cad6c42ccba02bd6b573",
      "size": 192,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/73ae7ffbe1370c6fbbc7cad6c42ccba02bd6b573"
    },
    {
      "path": ".gemini",
      "mode": "040000",
      "type": "tree",
      "sha": "8f5db9a1a54396d8883fd0d14d6baeda37386629",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8f5db9a1a54396d8883fd0d14d6baeda37386629"
    },
    {
      "path": ".gemini/settings.json",
      "mode": "100644",
      "type": "blob",
      "sha": "c3b1526595f166a98f91f27a7877975c7baec442",
      "size": 53,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c3b1526595f166a98f91f27a7877975c7baec442"
    },
    {
      "path": ".github",
      "mode": "040000",
      "type": "tree",
      "sha": "abbfd5c9ecd46e89b876c05606cb5631cf9902ad",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/abbfd5c9ecd46e89b876c05606cb5631cf9902ad"
    },
    {
      "path": ".github/ISSUE_TEMPLATE",
      "mode": "040000",
      "type": "tree",
      "sha": "7eda72e9f55733a8dc45b781ebfb3d2073f42de1",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7eda72e9f55733a8dc45b781ebfb3d2073f42de1"
    },
    {
      "path": ".github/ISSUE_TEMPLATE/blank_issue.md",
      "mode": "100644",
      "type": "blob",
      "sha": "dd6ebabf8f71b6a977c61da6e57a969ea1a48756",
      "size": 110,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dd6ebabf8f71b6a977c61da6e57a969ea1a48756"
    },
    {
      "path": ".github/ISSUE_TEMPLATE/bug_report.md",
      "mode": "100644",
      "type": "blob",
      "sha": "6b73d7536af7af392ad8710a45f5dd0a244282b8",
      "size": 742,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6b73d7536af7af392ad8710a45f5dd0a244282b8"
    },
    {
      "path": ".github/ISSUE_TEMPLATE/config.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "3ba13e0cec6cbbfd462e9ebf529dd2093148cd69",
      "size": 28,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3ba13e0cec6cbbfd462e9ebf529dd2093148cd69"
    },
    {
      "path": ".github/ISSUE_TEMPLATE/feature_request.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1eee5871bc8303dd19a065b361116e8fb85343bf",
      "size": 255,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1eee5871bc8303dd19a065b361116e8fb85343bf"
    },
    {
      "path": ".github/ISSUE_TEMPLATE/new-release.md",
      "mode": "100644",
      "type": "blob",
      "sha": "a55fbb21f27a05a3b4bca8ad021b089fe7558272",
      "size": 6066,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a55fbb21f27a05a3b4bca8ad021b089fe7558272"
    },
    {
      "path": ".github/PULL_REQUEST_TEMPLATE.md",
      "mode": "100644",
      "type": "blob",
      "sha": "d4ea3f529119eba1fbd5890c7769bf571bec14e1",
      "size": 522,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d4ea3f529119eba1fbd5890c7769bf571bec14e1"
    },
    {
      "path": ".github/actions",
      "mode": "040000",
      "type": "tree",
      "sha": "45a46469855f62bbdbe8ff6127e7e25b10ca2372",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/45a46469855f62bbdbe8ff6127e7e25b10ca2372"
    },
    {
      "path": ".github/actions/docker-build-and-push",
      "mode": "040000",
      "type": "tree",
      "sha": "6deb9d24f30bc7bc1da675572521bf29eea0a6ad",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6deb9d24f30bc7bc1da675572521bf29eea0a6ad"
    },
    {
      "path": ".github/actions/docker-build-and-push/action.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "ac88e88a7bde50318dc42b754ea3aeaadd688f82",
      "size": 3016,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ac88e88a7bde50318dc42b754ea3aeaadd688f82"
    },
    {
      "path": ".github/actions/e2e-runner-setup",
      "mode": "040000",
      "type": "tree",
      "sha": "3d4075d9773263c10b5e657aac905441a3b79759",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/3d4075d9773263c10b5e657aac905441a3b79759"
    },
    {
      "path": ".github/actions/e2e-runner-setup/action.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "2fd56638fc1b063526f6c70118160d02a28d1cbc",
      "size": 1675,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2fd56638fc1b063526f6c70118160d02a28d1cbc"
    },
    {
      "path": ".github/actions/helm-build-and-push",
      "mode": "040000",
      "type": "tree",
      "sha": "8c47c9780d87eabf27cf3eb9875857c20923e2e5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8c47c9780d87eabf27cf3eb9875857c20923e2e5"
    },
    {
      "path": ".github/actions/helm-build-and-push/action.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "6a7c03dbf88f9e86008ea7a3ea4b4d3812d39454",
      "size": 1022,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6a7c03dbf88f9e86008ea7a3ea4b4d3812d39454"
    },
    {
      "path": ".github/actions/trivy-scan",
      "mode": "040000",
      "type": "tree",
      "sha": "c9795a8461433c3cb88cb1334c168df5f6c67209",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c9795a8461433c3cb88cb1334c168df5f6c67209"
    },
    {
      "path": ".github/actions/trivy-scan/action.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "df202e151a4a2693ca61bf1a9c27e7552910837e",
      "size": 1510,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/df202e151a4a2693ca61bf1a9c27e7552910837e"
    },
    {
      "path": ".github/copilot-instructions.md",
      "mode": "120000",
      "type": "blob",
      "sha": "be77ac83a1895cfa9e271961ba1e565c273c717c",
      "size": 12,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/be77ac83a1895cfa9e271961ba1e565c273c717c"
    },
    {
      "path": ".github/dependabot.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "9449d8eb7709738905ee5b48b546fe7eeed66410",
      "size": 1661,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9449d8eb7709738905ee5b48b546fe7eeed66410"
    },
    {
      "path": ".github/labeler.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "0ff808b15b5c7f62492b5c6b0994cb78b95c73cd",
      "size": 2202,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0ff808b15b5c7f62492b5c6b0994cb78b95c73cd"
    },
    {
      "path": ".github/workflows",
      "mode": "040000",
      "type": "tree",
      "sha": "ed92aeac2d4237f50eee6676d0b22a3916bb7733",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ed92aeac2d4237f50eee6676d0b22a3916bb7733"
    },
    {
      "path": ".github/workflows/check-typos.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1b87b1e819084de637ea75792901f785fb68df33",
      "size": 432,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1b87b1e819084de637ea75792901f785fb68df33"
    },
    {
      "path": ".github/workflows/ci-build-images.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "ec4dea7d617d1994231e3147be38d54e5cde1e74",
      "size": 6753,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ec4dea7d617d1994231e3147be38d54e5cde1e74"
    },
    {
      "path": ".github/workflows/ci-coordinator.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "eef413a7982b1a7ef28f079335c91a46d0717795",
      "size": 7594,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eef413a7982b1a7ef28f079335c91a46d0717795"
    },
    {
      "path": ".github/workflows/ci-dependency-review.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "274cd2a8a503b3aa27eb29da0b70644edd7a2c4b",
      "size": 490,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/274cd2a8a503b3aa27eb29da0b70644edd7a2c4b"
    },
    {
      "path": ".github/workflows/ci-dev.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "38caf6e899ce4d84d2ab18e47388a47e23a83f38",
      "size": 2342,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/38caf6e899ce4d84d2ab18e47388a47e23a83f38"
    },
    {
      "path": ".github/workflows/ci-lint.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "556d8a9b530e3ba940243544d0dda5d1367fa4ad",
      "size": 4743,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/556d8a9b530e3ba940243544d0dda5d1367fa4ad"
    },
    {
      "path": ".github/workflows/ci-pr-checks.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "7136e9b7877a785ab1b8c12766acd0e361296bee",
      "size": 14293,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7136e9b7877a785ab1b8c12766acd0e361296bee"
    },
    {
      "path": ".github/workflows/ci-release.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c9c1e44c0c82caad3d482951b98a03ff62bfb528",
      "size": 3423,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c9c1e44c0c82caad3d482951b98a03ff62bfb528"
    },
    {
      "path": ".github/workflows/ci-signed-commits.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "2cfb4990083f2ceba02e071bcd19065e4203f99a",
      "size": 3400,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2cfb4990083f2ceba02e071bcd19065e4203f99a"
    },
    {
      "path": ".github/workflows/issue-kind-label.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "47731c1320a38c3e231c49a1c0a85573bde01778",
      "size": 5482,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/47731c1320a38c3e231c49a1c0a85573bde01778"
    },
    {
      "path": ".github/workflows/md-link-check.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "5c6be8a42c7b737f4811b7c80be4da0a66309435",
      "size": 1649,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5c6be8a42c7b737f4811b7c80be4da0a66309435"
    },
    {
      "path": ".github/workflows/nightly-router-perf-test-optimized-baseline-10k-1k.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "15c7d0418288737ce10946f391e15b5e764b1b2f",
      "size": 4624,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/15c7d0418288737ce10946f391e15b5e764b1b2f"
    },
    {
      "path": ".github/workflows/non-main-gatekeeper.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "060645764ece84392ef3053495d9f27c0311940a",
      "size": 616,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/060645764ece84392ef3053495d9f27c0311940a"
    },
    {
      "path": ".github/workflows/pr-area-labeler.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "7cc4175b9019fbf1ba9a226b485398d94508442f",
      "size": 584,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7cc4175b9019fbf1ba9a226b485398d94508442f"
    },
    {
      "path": ".github/workflows/pr-hold-gate.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "6f9bdc3f37ca3bca5a05ba5b07f44c540a48eb23",
      "size": 1013,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6f9bdc3f37ca3bca5a05ba5b07f44c540a48eb23"
    },
    {
      "path": ".github/workflows/pr-kind-label.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "525eff71265cfe896302a7e36a12d38a1e7a6b38",
      "size": 5507,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/525eff71265cfe896302a7e36a12d38a1e7a6b38"
    },
    {
      "path": ".github/workflows/pr-rebase.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "45482af576cc0a5f0059b5e2e3c6783f61fe1354",
      "size": 6136,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/45482af576cc0a5f0059b5e2e3c6783f61fe1354"
    },
    {
      "path": ".github/workflows/pr-size-labeler.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "ae9cf78deba6eff794e07758d303fd772eb0bcd0",
      "size": 660,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ae9cf78deba6eff794e07758d303fd772eb0bcd0"
    },
    {
      "path": ".github/workflows/re-run-action.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "8a60da5bdeea502df70644f25acd0cfa9a32474e",
      "size": 615,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8a60da5bdeea502df70644f25acd0cfa9a32474e"
    },
    {
      "path": ".github/workflows/release-notes-assemble.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "6f6d5a67c06c99440560eec7d7e6002924f97184",
      "size": 5707,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6f6d5a67c06c99440560eec7d7e6002924f97184"
    },
    {
      "path": ".github/workflows/release-notes-validate.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "22e8ddc630b19294c2dfeb9d0587a2338f1cab95",
      "size": 1706,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/22e8ddc630b19294c2dfeb9d0587a2338f1cab95"
    },
    {
      "path": ".github/workflows/stale.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "16671644207dff2723ef989f447521b52c5c2480",
      "size": 1938,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/16671644207dff2723ef989f447521b52c5c2480"
    },
    {
      "path": ".github/workflows/unstale.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "4550fbe6ac064597615518fff020b171c40148d4",
      "size": 1330,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4550fbe6ac064597615518fff020b171c40148d4"
    },
    {
      "path": ".gitignore",
      "mode": "100644",
      "type": "blob",
      "sha": "0125b373dbb0d047f21a5ddab4cdd62c8d207fff",
      "size": 1122,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0125b373dbb0d047f21a5ddab4cdd62c8d207fff"
    },
    {
      "path": ".golangci-security.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "08dc3dae0dbcec66f4d3e2bb81aba9e98b10752f",
      "size": 392,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/08dc3dae0dbcec66f4d3e2bb81aba9e98b10752f"
    },
    {
      "path": ".golangci.yml",
      "mode": "100644",
      "type": "blob",
      "sha": "835f682690b5b8b75261ead2f53c68362b369b6e",
      "size": 5116,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/835f682690b5b8b75261ead2f53c68362b369b6e"
    },
    {
      "path": ".lychee.toml",
      "mode": "100644",
      "type": "blob",
      "sha": "30cfe7649479015a3ab28742d834c83a94940d4e",
      "size": 1611,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/30cfe7649479015a3ab28742d834c83a94940d4e"
    },
    {
      "path": ".typos.toml",
      "mode": "100644",
      "type": "blob",
      "sha": "60e99ca5ab19f4863174e45d188d4fcf046f2423",
      "size": 459,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/60e99ca5ab19f4863174e45d188d4fcf046f2423"
    },
    {
      "path": "AGENTS.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1b71c284f4510f43755ca779412162a58dfc8c55",
      "size": 7436,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1b71c284f4510f43755ca779412162a58dfc8c55"
    },
    {
      "path": "CLAUDE.md",
      "mode": "120000",
      "type": "blob",
      "sha": "47dc3e3d863cfb5727b87d785d09abf9743c0a72",
      "size": 9,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/47dc3e3d863cfb5727b87d785d09abf9743c0a72"
    },
    {
      "path": "CODEOWNERS",
      "mode": "100644",
      "type": "blob",
      "sha": "8e83e08bf9569a3717beda09f5788021965c364b",
      "size": 4647,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8e83e08bf9569a3717beda09f5788021965c364b"
    },
    {
      "path": "CONTRIBUTING.md",
      "mode": "100644",
      "type": "blob",
      "sha": "55241c1a18a6f53337dda599fa3df3407bf128cd",
      "size": 719,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/55241c1a18a6f53337dda599fa3df3407bf128cd"
    },
    {
      "path": "DEVELOPMENT.md",
      "mode": "100644",
      "type": "blob",
      "sha": "d02aab981413dcb651f7e835a467bd7ef177a97a",
      "size": 36058,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d02aab981413dcb651f7e835a467bd7ef177a97a"
    },
    {
      "path": "Dockerfile.builder",
      "mode": "100644",
      "type": "blob",
      "sha": "9911bcfff9b3c6c09d30f12ff71d33add9fe5981",
      "size": 3963,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9911bcfff9b3c6c09d30f12ff71d33add9fe5981"
    },
    {
      "path": "Dockerfile.coordinator",
      "mode": "100644",
      "type": "blob",
      "sha": "71ab97f560aa35546d3bd332ee14cb53f00ea3de",
      "size": 1461,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/71ab97f560aa35546d3bd332ee14cb53f00ea3de"
    },
    {
      "path": "Dockerfile.epp",
      "mode": "100644",
      "type": "blob",
      "sha": "7b45dfab87a5e49495546ed6d88a4b23d6d65f6c",
      "size": 1709,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7b45dfab87a5e49495546ed6d88a4b23d6d65f6c"
    },
    {
      "path": "Dockerfile.sidecar",
      "mode": "100644",
      "type": "blob",
      "sha": "915e92fe3bf7c8f027ab7aad6a09e78c1cca4f0a",
      "size": 2019,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/915e92fe3bf7c8f027ab7aad6a09e78c1cca4f0a"
    },
    {
      "path": "LEADS.md",
      "mode": "100644",
      "type": "blob",
      "sha": "cfa467834a39bc98648cf67a7d8e5ba15c2a3516",
      "size": 1125,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cfa467834a39bc98648cf67a7d8e5ba15c2a3516"
    },
    {
      "path": "LICENSE",
      "mode": "100644",
      "type": "blob",
      "sha": "261eeb9e9f8b2b4b0d119366dda99c6fd7d35c64",
      "size": 11357,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/261eeb9e9f8b2b4b0d119366dda99c6fd7d35c64"
    },
    {
      "path": "Makefile",
      "mode": "100644",
      "type": "blob",
      "sha": "4d77ab92e3de562251a95a78e9dc16163b6f0de7",
      "size": 23522,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4d77ab92e3de562251a95a78e9dc16163b6f0de7"
    },
    {
      "path": "Makefile.cluster.mk",
      "mode": "100644",
      "type": "blob",
      "sha": "ef16037f2fb37cf2b6194b246c0ed2178601375b",
      "size": 4376,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ef16037f2fb37cf2b6194b246c0ed2178601375b"
    },
    {
      "path": "Makefile.coord.mk",
      "mode": "100644",
      "type": "blob",
      "sha": "40574c4fdefa4c2c676403dd871faa9e0677af52",
      "size": 11058,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/40574c4fdefa4c2c676403dd871faa9e0677af52"
    },
    {
      "path": "Makefile.gen.mk",
      "mode": "100644",
      "type": "blob",
      "sha": "cd60d5d0a8101129e9fe8b761216aee49b3b164b",
      "size": 4274,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cd60d5d0a8101129e9fe8b761216aee49b3b164b"
    },
    {
      "path": "Makefile.kind.mk",
      "mode": "100644",
      "type": "blob",
      "sha": "6ac92a5338556a6d7ad7b5dc4898b1f7faec7281",
      "size": 1718,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6ac92a5338556a6d7ad7b5dc4898b1f7faec7281"
    },
    {
      "path": "Makefile.tools.mk",
      "mode": "100644",
      "type": "blob",
      "sha": "81001cf2815db2f15c75dad542f459bc3ceb4379",
      "size": 2153,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/81001cf2815db2f15c75dad542f459bc3ceb4379"
    },
    {
      "path": "README.coord.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5c64f1974ce51bbd2f4c278adedc7ccb9cbc90d6",
      "size": 6102,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5c64f1974ce51bbd2f4c278adedc7ccb9cbc90d6"
    },
    {
      "path": "README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "dcca915fac6d1f322fda8dbca2c63bbe6edb8caa",
      "size": 8188,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dcca915fac6d1f322fda8dbca2c63bbe6edb8caa"
    },
    {
      "path": "RELEASE-NOTES.md",
      "mode": "100644",
      "type": "blob",
      "sha": "c885fbca1ab2992c412066570018de1fd361f655",
      "size": 39204,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c885fbca1ab2992c412066570018de1fd361f655"
    },
    {
      "path": "SECURITY.md",
      "mode": "100644",
      "type": "blob",
      "sha": "3932c271f0cf371fc08330bdc8f09d7aaa2ca759",
      "size": 183,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3932c271f0cf371fc08330bdc8f09d7aaa2ca759"
    },
    {
      "path": "apix",
      "mode": "040000",
      "type": "tree",
      "sha": "42ab018dea4e7e9deed834b3427f63191af03bbf",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/42ab018dea4e7e9deed834b3427f63191af03bbf"
    },
    {
      "path": "apix/config",
      "mode": "040000",
      "type": "tree",
      "sha": "b617f0ece93aa6b20e2925ae3727fab9e3f169ac",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b617f0ece93aa6b20e2925ae3727fab9e3f169ac"
    },
    {
      "path": "apix/config/v1alpha1",
      "mode": "040000",
      "type": "tree",
      "sha": "dd042b1f8ecc36befdb16d7b18b62cb1684592a3",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/dd042b1f8ecc36befdb16d7b18b62cb1684592a3"
    },
    {
      "path": "apix/config/v1alpha1/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3054c04d4dacfc64ef6f5949380ab5227c8c7dee",
      "size": 734,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3054c04d4dacfc64ef6f5949380ab5227c8c7dee"
    },
    {
      "path": "apix/config/v1alpha1/endpointpickerconfig_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7c243e032001305367ab097c7203c7ccd9be32dc",
      "size": 24435,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7c243e032001305367ab097c7203c7ccd9be32dc"
    },
    {
      "path": "apix/config/v1alpha1/endpointpickerconfig_types_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f29fa452e9065838cf5ece05983f564a10117dc3",
      "size": 3139,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f29fa452e9065838cf5ece05983f564a10117dc3"
    },
    {
      "path": "apix/config/v1alpha1/zz_generated.deepcopy.go",
      "mode": "100644",
      "type": "blob",
      "sha": "acf5563ea57c19772f206ee250520d7370335e27",
      "size": 12381,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/acf5563ea57c19772f206ee250520d7370335e27"
    },
    {
      "path": "apix/config/v1alpha1/zz_generated.register.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5408575804fbc1d53102a2fffc6c9cb4b2588a0a",
      "size": 1825,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5408575804fbc1d53102a2fffc6c9cb4b2588a0a"
    },
    {
      "path": "apix/v1alpha2",
      "mode": "040000",
      "type": "tree",
      "sha": "8759adfe8069a2dc2e70c66f207e85d93a5ec856",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8759adfe8069a2dc2e70c66f207e85d93a5ec856"
    },
    {
      "path": "apix/v1alpha2/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0f6ca75bb47dacd2aec96d67ae7db25e9ae55f39",
      "size": 786,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0f6ca75bb47dacd2aec96d67ae7db25e9ae55f39"
    },
    {
      "path": "apix/v1alpha2/inferencemodelrewrite_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "eee90e34a6febe46005e9359a720fd790adf2e84",
      "size": 7422,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eee90e34a6febe46005e9359a720fd790adf2e84"
    },
    {
      "path": "apix/v1alpha2/inferenceobjective_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9b7b11037c4b9b2b32489fa8de1fcf3c7e8991a2",
      "size": 5131,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9b7b11037c4b9b2b32489fa8de1fcf3c7e8991a2"
    },
    {
      "path": "apix/v1alpha2/shared_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4fc2c7782846d4577c0632e88e5af99f97344dec",
      "size": 4977,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4fc2c7782846d4577c0632e88e5af99f97344dec"
    },
    {
      "path": "apix/v1alpha2/zz_generated.deepcopy.go",
      "mode": "100644",
      "type": "blob",
      "sha": "494e9d31e7981cff8bf50cd92feda6986b6881c4",
      "size": 9614,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/494e9d31e7981cff8bf50cd92feda6986b6881c4"
    },
    {
      "path": "apix/v1alpha2/zz_generated.register.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3847977a92738d779f115de51b30dae0ca84721a",
      "size": 1912,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3847977a92738d779f115de51b30dae0ca84721a"
    },
    {
      "path": "client-go",
      "mode": "040000",
      "type": "tree",
      "sha": "0294e7549945fb68f85a3458d2679c52a3129f76",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0294e7549945fb68f85a3458d2679c52a3129f76"
    },
    {
      "path": "client-go/applyconfiguration",
      "mode": "040000",
      "type": "tree",
      "sha": "0b64eb696ba320ebb2f8f701eeb7988ed2985f10",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0b64eb696ba320ebb2f8f701eeb7988ed2985f10"
    },
    {
      "path": "client-go/applyconfiguration/apix",
      "mode": "040000",
      "type": "tree",
      "sha": "32b7952c7607a221f29cf3636350deef2cb7de28",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/32b7952c7607a221f29cf3636350deef2cb7de28"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2",
      "mode": "040000",
      "type": "tree",
      "sha": "2920b71c2da0858e461a6fcae4069f457b2dd4bb",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2920b71c2da0858e461a6fcae4069f457b2dd4bb"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/inferencemodelrewrite.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fcbe69dae4a1c7631ed80b5ca216603bc7b1407a",
      "size": 11996,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fcbe69dae4a1c7631ed80b5ca216603bc7b1407a"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/inferencemodelrewriterule.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9cd6fa2aeb667d9b5e102eda2a8c9a721577734c",
      "size": 2350,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9cd6fa2aeb667d9b5e102eda2a8c9a721577734c"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/inferencemodelrewritespec.go",
      "mode": "100644",
      "type": "blob",
      "sha": "50e63a5a7da9201972a064f170406835a3feb7ed",
      "size": 2670,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/50e63a5a7da9201972a064f170406835a3feb7ed"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/inferencemodelrewritestatus.go",
      "mode": "100644",
      "type": "blob",
      "sha": "57be59cde533bdef166b81c78f4fd39eef939b00",
      "size": 1551,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/57be59cde533bdef166b81c78f4fd39eef939b00"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/inferenceobjective.go",
      "mode": "100644",
      "type": "blob",
      "sha": "371f075cb5f7b6345851c79905fecaa4739c71bd",
      "size": 11832,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/371f075cb5f7b6345851c79905fecaa4739c71bd"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/inferenceobjectivespec.go",
      "mode": "100644",
      "type": "blob",
      "sha": "614c4fb4b7a1fea3b4f38524d0cae0508e129a14",
      "size": 3365,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/614c4fb4b7a1fea3b4f38524d0cae0508e129a14"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/inferenceobjectivestatus.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ad38e22399f151cc2834b4a1e602627812572810",
      "size": 1511,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ad38e22399f151cc2834b4a1e602627812572810"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/match.go",
      "mode": "100644",
      "type": "blob",
      "sha": "021cd78b2daeb18e507d5d6e57ea8aa4d33ec07f",
      "size": 1039,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/021cd78b2daeb18e507d5d6e57ea8aa4d33ec07f"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/modelmatch.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8f06ebbab5f1600e9bf9458e4cb343235872a332",
      "size": 1677,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8f06ebbab5f1600e9bf9458e4cb343235872a332"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/poolobjectreference.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4f7b06866f8db50130fbb7da7268ff68ce393325",
      "size": 2248,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4f7b06866f8db50130fbb7da7268ff68ce393325"
    },
    {
      "path": "client-go/applyconfiguration/apix/v1alpha2/targetmodel.go",
      "mode": "100644",
      "type": "blob",
      "sha": "83d3e24740a3bbe4d69ff7bb77d149cd62124277",
      "size": 2334,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/83d3e24740a3bbe4d69ff7bb77d149cd62124277"
    },
    {
      "path": "client-go/applyconfiguration/internal",
      "mode": "040000",
      "type": "tree",
      "sha": "21cfee81fec92bc19b6db6ab8942a76a201a2145",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/21cfee81fec92bc19b6db6ab8942a76a201a2145"
    },
    {
      "path": "client-go/applyconfiguration/internal/internal.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e9ad9b4739b24f283764108c857337ba5f6efcb0",
      "size": 935,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e9ad9b4739b24f283764108c857337ba5f6efcb0"
    },
    {
      "path": "client-go/applyconfiguration/utils.go",
      "mode": "100644",
      "type": "blob",
      "sha": "56d725622e2175bc8e5d8586d951c4593ffb638b",
      "size": 2339,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/56d725622e2175bc8e5d8586d951c4593ffb638b"
    },
    {
      "path": "client-go/clientset",
      "mode": "040000",
      "type": "tree",
      "sha": "c73b9022726312b7e3e53f1aae31086cafc4eb98",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c73b9022726312b7e3e53f1aae31086cafc4eb98"
    },
    {
      "path": "client-go/clientset/versioned",
      "mode": "040000",
      "type": "tree",
      "sha": "26d62dfc7776e113e2ef5c4cf9882e98d4638bbb",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/26d62dfc7776e113e2ef5c4cf9882e98d4638bbb"
    },
    {
      "path": "client-go/clientset/versioned/clientset.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6af18d76f19351fc959c5d6ab0bfb7a1d9eb5bba",
      "size": 3337,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6af18d76f19351fc959c5d6ab0bfb7a1d9eb5bba"
    },
    {
      "path": "client-go/clientset/versioned/fake",
      "mode": "040000",
      "type": "tree",
      "sha": "f94cc2893f921632b10a10af3bb5b387d33f96e9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f94cc2893f921632b10a10af3bb5b387d33f96e9"
    },
    {
      "path": "client-go/clientset/versioned/fake/clientset_generated.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ac11936b96d16f33bd995df68dc66cda92b47d6e",
      "size": 4426,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ac11936b96d16f33bd995df68dc66cda92b47d6e"
    },
    {
      "path": "client-go/clientset/versioned/fake/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3630ed1cd17dbf0e719414643eb66f9beb3097cc",
      "size": 124,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3630ed1cd17dbf0e719414643eb66f9beb3097cc"
    },
    {
      "path": "client-go/clientset/versioned/fake/register.go",
      "mode": "100644",
      "type": "blob",
      "sha": "07c430172729af7f66bd5b19f31fc6b7d0bc26c8",
      "size": 1281,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/07c430172729af7f66bd5b19f31fc6b7d0bc26c8"
    },
    {
      "path": "client-go/clientset/versioned/scheme",
      "mode": "040000",
      "type": "tree",
      "sha": "a6ced3ae9c4597ee9ce7babcbc8eb4932a7c280c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a6ced3ae9c4597ee9ce7babcbc8eb4932a7c280c"
    },
    {
      "path": "client-go/clientset/versioned/scheme/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "14db57a58f8d2535aae9393d9668432f4db2346c",
      "size": 140,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/14db57a58f8d2535aae9393d9668432f4db2346c"
    },
    {
      "path": "client-go/clientset/versioned/scheme/register.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7ed2de090c819366023a01fee7aeb35d0c82a3bc",
      "size": 1337,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7ed2de090c819366023a01fee7aeb35d0c82a3bc"
    },
    {
      "path": "client-go/clientset/versioned/typed",
      "mode": "040000",
      "type": "tree",
      "sha": "fc2225f559654c5c16cc35389f9b5cca84fb8c59",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/fc2225f559654c5c16cc35389f9b5cca84fb8c59"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix",
      "mode": "040000",
      "type": "tree",
      "sha": "bd7b8e1afc8e3e1c20075fe863d8c3848858d36a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/bd7b8e1afc8e3e1c20075fe863d8c3848858d36a"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2",
      "mode": "040000",
      "type": "tree",
      "sha": "6eb10e51b27a0bccd5c7fc334075426eeda9eff2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6eb10e51b27a0bccd5c7fc334075426eeda9eff2"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/apix_client.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0bc6afa587655404d6f2d2836a0f25ea8ff3b4b1",
      "size": 2837,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0bc6afa587655404d6f2d2836a0f25ea8ff3b4b1"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c11da26828d9e142fe648df3819077acbc510d9b",
      "size": 127,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c11da26828d9e142fe648df3819077acbc510d9b"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/fake",
      "mode": "040000",
      "type": "tree",
      "sha": "c1d70b52954c22bacc4f1bf847a1efe31b79389b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c1d70b52954c22bacc4f1bf847a1efe31b79389b"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/fake/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2b5ba4c8e44227355de686b02a1702a03bc86bc2",
      "size": 117,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2b5ba4c8e44227355de686b02a1702a03bc86bc2"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/fake/fake_apix_client.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fd2a3e975cccf2112c1bb89e4b3facbd481ae570",
      "size": 835,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fd2a3e975cccf2112c1bb89e4b3facbd481ae570"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/fake/fake_inferencemodelrewrite.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f8a9567a00503e94aec89ae1b14de7e96269a556",
      "size": 1740,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f8a9567a00503e94aec89ae1b14de7e96269a556"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/fake/fake_inferenceobjective.go",
      "mode": "100644",
      "type": "blob",
      "sha": "34b2a7c675148111f2b43aa43f1ace1997db894a",
      "size": 1671,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/34b2a7c675148111f2b43aa43f1ace1997db894a"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/generated_expansion.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2dad2d49128169846e61a9ecce59104d1f0c61b1",
      "size": 159,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2dad2d49128169846e61a9ecce59104d1f0c61b1"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/inferencemodelrewrite.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1a983dc1d9c0ef765dfec2d38d27f7ade2d5d4da",
      "size": 3588,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1a983dc1d9c0ef765dfec2d38d27f7ade2d5d4da"
    },
    {
      "path": "client-go/clientset/versioned/typed/apix/v1alpha2/inferenceobjective.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4dcd111d2f7e25f02d95076ffbaf935e7d8e0301",
      "size": 3450,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4dcd111d2f7e25f02d95076ffbaf935e7d8e0301"
    },
    {
      "path": "client-go/informers",
      "mode": "040000",
      "type": "tree",
      "sha": "043daaf377b34068154f2c2a37862db749022ab0",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/043daaf377b34068154f2c2a37862db749022ab0"
    },
    {
      "path": "client-go/informers/externalversions",
      "mode": "040000",
      "type": "tree",
      "sha": "35c0788cc7aa1b6809a72826a9100f6d23e36da5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/35c0788cc7aa1b6809a72826a9100f6d23e36da5"
    },
    {
      "path": "client-go/informers/externalversions/apix",
      "mode": "040000",
      "type": "tree",
      "sha": "7b77ffbea901cba1132d4937ec7f07e868f11658",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7b77ffbea901cba1132d4937ec7f07e868f11658"
    },
    {
      "path": "client-go/informers/externalversions/apix/interface.go",
      "mode": "100644",
      "type": "blob",
      "sha": "75bc9fa478c3843769e68c7ad26edf349681c88d",
      "size": 1063,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/75bc9fa478c3843769e68c7ad26edf349681c88d"
    },
    {
      "path": "client-go/informers/externalversions/apix/v1alpha2",
      "mode": "040000",
      "type": "tree",
      "sha": "10672734d010419ad808d472422bb945db8c2bef",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/10672734d010419ad808d472422bb945db8c2bef"
    },
    {
      "path": "client-go/informers/externalversions/apix/v1alpha2/inferencemodelrewrite.go",
      "mode": "100644",
      "type": "blob",
      "sha": "529111d44e4b45c793db1dd9902541b9550f4cf7",
      "size": 4017,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/529111d44e4b45c793db1dd9902541b9550f4cf7"
    },
    {
      "path": "client-go/informers/externalversions/apix/v1alpha2/inferenceobjective.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7a57071916a28b4f28dd748bb51df667dabcac62",
      "size": 3945,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a57071916a28b4f28dd748bb51df667dabcac62"
    },
    {
      "path": "client-go/informers/externalversions/apix/v1alpha2/interface.go",
      "mode": "100644",
      "type": "blob",
      "sha": "09faa655f12fc43be6167069e18a5596268ba8b3",
      "size": 1472,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/09faa655f12fc43be6167069e18a5596268ba8b3"
    },
    {
      "path": "client-go/informers/externalversions/factory.go",
      "mode": "100644",
      "type": "blob",
      "sha": "334d10e188244a53e94ae18463198d2d4e38c558",
      "size": 8688,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/334d10e188244a53e94ae18463198d2d4e38c558"
    },
    {
      "path": "client-go/informers/externalversions/generic.go",
      "mode": "100644",
      "type": "blob",
      "sha": "40c724f8a5f1ba15e15252693c09e87af64f2cd5",
      "size": 1662,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/40c724f8a5f1ba15e15252693c09e87af64f2cd5"
    },
    {
      "path": "client-go/informers/externalversions/internalinterfaces",
      "mode": "040000",
      "type": "tree",
      "sha": "7c467f9f05311db89cfbe8634656712e2600a930",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7c467f9f05311db89cfbe8634656712e2600a930"
    },
    {
      "path": "client-go/informers/externalversions/internalinterfaces/factory_interfaces.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a0e98040a5d67047cb9afad7f2e7054fb09a1d72",
      "size": 861,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a0e98040a5d67047cb9afad7f2e7054fb09a1d72"
    },
    {
      "path": "client-go/listers",
      "mode": "040000",
      "type": "tree",
      "sha": "118c849c03ed9bb44fbd04dbe7ac8961bb915d53",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/118c849c03ed9bb44fbd04dbe7ac8961bb915d53"
    },
    {
      "path": "client-go/listers/apix",
      "mode": "040000",
      "type": "tree",
      "sha": "59ffae8db8c8f0ccb997c866ce095762a7d03f30",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/59ffae8db8c8f0ccb997c866ce095762a7d03f30"
    },
    {
      "path": "client-go/listers/apix/v1alpha2",
      "mode": "040000",
      "type": "tree",
      "sha": "88ee9225d0ac985665599107f1c71285d9ae420e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/88ee9225d0ac985665599107f1c71285d9ae420e"
    },
    {
      "path": "client-go/listers/apix/v1alpha2/expansion_generated.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6d9ec6580d57c42b6d321ea964473ad8dabb7b2e",
      "size": 756,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6d9ec6580d57c42b6d321ea964473ad8dabb7b2e"
    },
    {
      "path": "client-go/listers/apix/v1alpha2/inferencemodelrewrite.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c6f78c27f4ab233d049784986cb9808d9c0d9ef6",
      "size": 2587,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c6f78c27f4ab233d049784986cb9808d9c0d9ef6"
    },
    {
      "path": "client-go/listers/apix/v1alpha2/inferenceobjective.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e0ae88d7397eacc0a31b2b5df14b26eea4525803",
      "size": 2467,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e0ae88d7397eacc0a31b2b5df14b26eea4525803"
    },
    {
      "path": "cmd",
      "mode": "040000",
      "type": "tree",
      "sha": "4edbc00e31e5944626601539f3b539910a56f6b4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4edbc00e31e5944626601539f3b539910a56f6b4"
    },
    {
      "path": "cmd/coordinator",
      "mode": "040000",
      "type": "tree",
      "sha": "5b9d6a5f0c18d6abb7a41e2a4c4089537e5be37d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5b9d6a5f0c18d6abb7a41e2a4c4089537e5be37d"
    },
    {
      "path": "cmd/coordinator/main.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1f6f8963f9f10bde64882b85cd553fc4f16e3362",
      "size": 7107,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1f6f8963f9f10bde64882b85cd553fc4f16e3362"
    },
    {
      "path": "cmd/coordinator/main_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2f0511049eb717e945ea15986a302b5192ccefa0",
      "size": 4233,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2f0511049eb717e945ea15986a302b5192ccefa0"
    },
    {
      "path": "cmd/epp",
      "mode": "040000",
      "type": "tree",
      "sha": "bf08c2a70dca6e3932150b04c665259f0aac773e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/bf08c2a70dca6e3932150b04c665259f0aac773e"
    },
    {
      "path": "cmd/epp/main.go",
      "mode": "100644",
      "type": "blob",
      "sha": "94c1b5a7815cded0abe867907d912dc452e2d289",
      "size": 1340,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/94c1b5a7815cded0abe867907d912dc452e2d289"
    },
    {
      "path": "cmd/epp/runner",
      "mode": "040000",
      "type": "tree",
      "sha": "1c221403297471aa8ffac37cef8c73a7f1f605f4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1c221403297471aa8ffac37cef8c73a7f1f605f4"
    },
    {
      "path": "cmd/epp/runner/feature_gate_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0f4aea667b31ce5d75c40e37ab946ea8c60a7e50",
      "size": 6900,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0f4aea667b31ce5d75c40e37ab946ea8c60a7e50"
    },
    {
      "path": "cmd/epp/runner/health.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1ea2cdc7180e464fc0317177d0646321045ef53e",
      "size": 7550,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1ea2cdc7180e464fc0317177d0646321045ef53e"
    },
    {
      "path": "cmd/epp/runner/health_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d56f7bd3c3de4cee0437bb12647260550c3d7d89",
      "size": 14706,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d56f7bd3c3de4cee0437bb12647260550c3d7d89"
    },
    {
      "path": "cmd/epp/runner/multicluster_config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e0117a1d7fa4857b0acb7fc6853929ef99c40b97",
      "size": 4314,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e0117a1d7fa4857b0acb7fc6853929ef99c40b97"
    },
    {
      "path": "cmd/epp/runner/resolve_discovery_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fc1fe755c5467c81adde788ca294b0a384bf250f",
      "size": 2955,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fc1fe755c5467c81adde788ca294b0a384bf250f"
    },
    {
      "path": "cmd/epp/runner/run_with_file_discovery_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6354df8265d85f8a117fe0ed7e0a691f926736f5",
      "size": 11054,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6354df8265d85f8a117fe0ed7e0a691f926736f5"
    },
    {
      "path": "cmd/epp/runner/rungroup.go",
      "mode": "100644",
      "type": "blob",
      "sha": "de9b27ddeafd6a76d74ed0971e8a0837d13588a3",
      "size": 1559,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/de9b27ddeafd6a76d74ed0971e8a0837d13588a3"
    },
    {
      "path": "cmd/epp/runner/runner.go",
      "mode": "100644",
      "type": "blob",
      "sha": "41cc976ab510eef29743de8538e59855c4fdbba7",
      "size": 63325,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/41cc976ab510eef29743de8538e59855c4fdbba7"
    },
    {
      "path": "cmd/epp/runner/test_runner.go",
      "mode": "100644",
      "type": "blob",
      "sha": "781baa85810c144ed10b1f58ef95c8b1d6d32dd3",
      "size": 3387,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/781baa85810c144ed10b1f58ef95c8b1d6d32dd3"
    },
    {
      "path": "cmd/pd-sidecar",
      "mode": "040000",
      "type": "tree",
      "sha": "8acbb02e9df4bf78867ed30969a919751bd232d9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8acbb02e9df4bf78867ed30969a919751bd232d9"
    },
    {
      "path": "cmd/pd-sidecar/main.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c0ef60bade53d366e35e439a964bbdba29f1d21a",
      "size": 2239,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c0ef60bade53d366e35e439a964bbdba29f1d21a"
    },
    {
      "path": "config",
      "mode": "040000",
      "type": "tree",
      "sha": "4bfe76c69971e921ff1f3d0a18aadff6a45a0a40",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4bfe76c69971e921ff1f3d0a18aadff6a45a0a40"
    },
    {
      "path": "config/charts",
      "mode": "040000",
      "type": "tree",
      "sha": "ef8d46ede8cdbfca55e616047a3f656bc246ddd4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ef8d46ede8cdbfca55e616047a3f656bc246ddd4"
    },
    {
      "path": "config/charts/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "fce96ad772c0d4e7f8ec658536cfa47487d9707e",
      "size": 28753,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fce96ad772c0d4e7f8ec658536cfa47487d9707e"
    },
    {
      "path": "config/charts/llm-d-router-gateway",
      "mode": "040000",
      "type": "tree",
      "sha": "82375261921bcb376e170eb7772f60ab81e96830",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/82375261921bcb376e170eb7772f60ab81e96830"
    },
    {
      "path": "config/charts/llm-d-router-gateway/.helmignore",
      "mode": "100644",
      "type": "blob",
      "sha": "0e8a0eb36f4ca2c939201c0d54b5d82a1ea34778",
      "size": 349,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0e8a0eb36f4ca2c939201c0d54b5d82a1ea34778"
    },
    {
      "path": "config/charts/llm-d-router-gateway/Chart.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "35023c7649769186ddbf52524f5e2a3c64c4cdab",
      "size": 355,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/35023c7649769186ddbf52524f5e2a3c64c4cdab"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates",
      "mode": "040000",
      "type": "tree",
      "sha": "b07220ea3d09e0793033f43004ffb2998f6e62f5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b07220ea3d09e0793033f43004ffb2998f6e62f5"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates/NOTES.txt",
      "mode": "100644",
      "type": "blob",
      "sha": "3e5b18a517b4e44c8ddf4440cc0781b15026e60d",
      "size": 51,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3e5b18a517b4e44c8ddf4440cc0781b15026e60d"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates/_helpers.tpl",
      "mode": "100644",
      "type": "blob",
      "sha": "7e638719481484894cda3a4de6e94bb6b2d7af57",
      "size": 376,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7e638719481484894cda3a4de6e94bb6b2d7af57"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates/_validations.tpl",
      "mode": "100644",
      "type": "blob",
      "sha": "c5fcdb04aff9d4f5753936c10a61e48b65e1bf55",
      "size": 272,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c5fcdb04aff9d4f5753936c10a61e48b65e1bf55"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates/epp.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "95483ef637ac3d98de87f9efb4c35c546dff6010",
      "size": 348,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/95483ef637ac3d98de87f9efb4c35c546dff6010"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates/gke.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "db528e3d3c5c670a69d0bd569bac83e485fd6dd5",
      "size": 5493,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/db528e3d3c5c670a69d0bd569bac83e485fd6dd5"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates/httproute.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "534b7fb779b1329da44cfc54d4529dd0953b85d7",
      "size": 1300,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/534b7fb779b1329da44cfc54d4529dd0953b85d7"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates/istio.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "fde74676c81fee21a319274604e9cd328399ae87",
      "size": 657,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fde74676c81fee21a319274604e9cd328399ae87"
    },
    {
      "path": "config/charts/llm-d-router-gateway/templates/rbac.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "d232e92625ceb300864353a6860cd58924d2564f",
      "size": 1073,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d232e92625ceb300864353a6860cd58924d2564f"
    },
    {
      "path": "config/charts/llm-d-router-gateway/values.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "6309a2ef064c0032b315c1c741d7334eb013d94f",
      "size": 1901,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6309a2ef064c0032b315c1c741d7334eb013d94f"
    },
    {
      "path": "config/charts/llm-d-router-standalone",
      "mode": "040000",
      "type": "tree",
      "sha": "30af0a18e9cbb4fea0d1bac6f08fa9a247666794",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/30af0a18e9cbb4fea0d1bac6f08fa9a247666794"
    },
    {
      "path": "config/charts/llm-d-router-standalone/.helmignore",
      "mode": "100644",
      "type": "blob",
      "sha": "0e8a0eb36f4ca2c939201c0d54b5d82a1ea34778",
      "size": 349,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0e8a0eb36f4ca2c939201c0d54b5d82a1ea34778"
    },
    {
      "path": "config/charts/llm-d-router-standalone/Chart.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "029a5167884e9211e47b23cc7a7340617d37dfee",
      "size": 350,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/029a5167884e9211e47b23cc7a7340617d37dfee"
    },
    {
      "path": "config/charts/llm-d-router-standalone/templates",
      "mode": "040000",
      "type": "tree",
      "sha": "490b065ef46994a27fbcb6052280aa8c0cd5ef25",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/490b065ef46994a27fbcb6052280aa8c0cd5ef25"
    },
    {
      "path": "config/charts/llm-d-router-standalone/templates/_validations.tpl",
      "mode": "100644",
      "type": "blob",
      "sha": "01faee471dd4b6e206854af2d44905e8cdc57c5d",
      "size": 3591,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/01faee471dd4b6e206854af2d44905e8cdc57c5d"
    },
    {
      "path": "config/charts/llm-d-router-standalone/templates/epp.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "118eae497be5f1bdd31f3bdc2c16f2d3b727d4b9",
      "size": 560,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/118eae497be5f1bdd31f3bdc2c16f2d3b727d4b9"
    },
    {
      "path": "config/charts/llm-d-router-standalone/templates/rbac.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "376c179936e65abc03e8bfacc50e8f0bf08c4e72",
      "size": 1121,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/376c179936e65abc03e8bfacc50e8f0bf08c4e72"
    },
    {
      "path": "config/charts/llm-d-router-standalone/values.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "12832cad5a76436b017e79baa725d225773c288f",
      "size": 19949,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/12832cad5a76436b017e79baa725d225773c288f"
    },
    {
      "path": "config/charts/routerlib",
      "mode": "040000",
      "type": "tree",
      "sha": "c3a15655181030efdae583e6d5bae22a652012c0",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c3a15655181030efdae583e6d5bae22a652012c0"
    },
    {
      "path": "config/charts/routerlib/.helmignore",
      "mode": "100644",
      "type": "blob",
      "sha": "0e8a0eb36f4ca2c939201c0d54b5d82a1ea34778",
      "size": 349,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0e8a0eb36f4ca2c939201c0d54b5d82a1ea34778"
    },
    {
      "path": "config/charts/routerlib/Chart.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "38dcca25de0223b5a713a4430140e87e206b9b3e",
      "size": 135,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/38dcca25de0223b5a713a4430140e87e206b9b3e"
    },
    {
      "path": "config/charts/routerlib/templates",
      "mode": "040000",
      "type": "tree",
      "sha": "2f43066e2ee52b8f28bf244e92bb7338346eeaa7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2f43066e2ee52b8f28bf244e92bb7338346eeaa7"
    },
    {
      "path": "config/charts/routerlib/templates/_config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "8d585b065a6c390f3855e9b7ede821980a12be76",
      "size": 6166,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8d585b065a6c390f3855e9b7ede821980a12be76"
    },
    {
      "path": "config/charts/routerlib/templates/_deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "0a1d73d7c4b7bb4668e2d9bd2350de6971889c6a",
      "size": 16012,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0a1d73d7c4b7bb4668e2d9bd2350de6971889c6a"
    },
    {
      "path": "config/charts/routerlib/templates/_gke.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "cb0f5dd2db5d997700688fa26d0ac0cc8f76e6de",
      "size": 3323,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cb0f5dd2db5d997700688fa26d0ac0cc8f76e6de"
    },
    {
      "path": "config/charts/routerlib/templates/_helpers.tpl",
      "mode": "100644",
      "type": "blob",
      "sha": "97d7e0f044d16a6075ca9fbeae53ac97b7141760",
      "size": 22169,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/97d7e0f044d16a6075ca9fbeae53ac97b7141760"
    },
    {
      "path": "config/charts/routerlib/templates/_inferenceobjective.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "85c381b29f7713ce4d54fa732fe26cd9496001d2",
      "size": 438,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85c381b29f7713ce4d54fa732fe26cd9496001d2"
    },
    {
      "path": "config/charts/routerlib/templates/_inferencepool.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "572e3673f834b325903e2aba7083245f056c1431",
      "size": 1595,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/572e3673f834b325903e2aba7083245f056c1431"
    },
    {
      "path": "config/charts/routerlib/templates/_latency-predictor.tpl",
      "mode": "100644",
      "type": "blob",
      "sha": "e90159a46c831d2ea62b1a74e3c03ef64225c207",
      "size": 5060,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e90159a46c831d2ea62b1a74e3c03ef64225c207"
    },
    {
      "path": "config/charts/routerlib/templates/_leader-election-rbac.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "ff2af400cb0c2d266457ac71af6a9cc54ad3fadb",
      "size": 1187,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ff2af400cb0c2d266457ac71af6a9cc54ad3fadb"
    },
    {
      "path": "config/charts/routerlib/templates/_proxy-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "f3eeef55b20ef371a1c44bf91b55c096bb3c8337",
      "size": 3499,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f3eeef55b20ef371a1c44bf91b55c096bb3c8337"
    },
    {
      "path": "config/charts/routerlib/templates/_proxy-service.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "f3b8ed20d8aa748ccda5c509e00741dcaf9a1f05",
      "size": 696,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f3b8ed20d8aa748ccda5c509e00741dcaf9a1f05"
    },
    {
      "path": "config/charts/routerlib/templates/_rbac.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "0c50d11708220afd6875e072d7c9b5ec9714f319",
      "size": 1916,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0c50d11708220afd6875e072d7c9b5ec9714f319"
    },
    {
      "path": "config/charts/routerlib/templates/_sa-token-secret.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b0ee0176fe603b8eb5feafbfb13221dc1c88978f",
      "size": 662,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b0ee0176fe603b8eb5feafbfb13221dc1c88978f"
    },
    {
      "path": "config/charts/routerlib/templates/_service.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "087e3e0394247cc0c3b43e8ab03e0b7fe23c1233",
      "size": 3353,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/087e3e0394247cc0c3b43e8ab03e0b7fe23c1233"
    },
    {
      "path": "config/charts/routerlib/templates/_servicemonitor.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "4f3485f9edc87e2ecbe4bc74882afea17495a857",
      "size": 1143,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4f3485f9edc87e2ecbe4bc74882afea17495a857"
    },
    {
      "path": "config/charts/routerlib/values.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c68cc74ae4b104d8962e8931ed110f4e24d8af55",
      "size": 8674,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c68cc74ae4b104d8962e8931ed110f4e24d8af55"
    },
    {
      "path": "config/coordinator",
      "mode": "040000",
      "type": "tree",
      "sha": "2d7b3f8d0cff60c07de96c5eda5a117addf789c5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2d7b3f8d0cff60c07de96c5eda5a117addf789c5"
    },
    {
      "path": "config/coordinator/coordinator.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "57571973bb977c9193a9ae7cab094f14123b6fb7",
      "size": 13504,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/57571973bb977c9193a9ae7cab094f14123b6fb7"
    },
    {
      "path": "config/crd",
      "mode": "040000",
      "type": "tree",
      "sha": "c3afea4d02581f9a2e32714fae47bc7b2df0bf12",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c3afea4d02581f9a2e32714fae47bc7b2df0bf12"
    },
    {
      "path": "config/crd/bases",
      "mode": "040000",
      "type": "tree",
      "sha": "91c02d0a9724d6a68f858068d9755db01fb5f915",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/91c02d0a9724d6a68f858068d9755db01fb5f915"
    },
    {
      "path": "config/crd/bases/llm-d.ai_inferencemodelrewrites.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b150992530276e51a86987a85fbac89cdabeebfc",
      "size": 10577,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b150992530276e51a86987a85fbac89cdabeebfc"
    },
    {
      "path": "config/crd/bases/llm-d.ai_inferenceobjectives.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "e072d4783e4ea629b9f1c4e1e56406dbdccf5d9a",
      "size": 8913,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e072d4783e4ea629b9f1c4e1e56406dbdccf5d9a"
    },
    {
      "path": "config/crd/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "e12a71b0bee7980fecba467a37632ca15a60a89e",
      "size": 198,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e12a71b0bee7980fecba467a37632ca15a60a89e"
    },
    {
      "path": "config/manifests",
      "mode": "040000",
      "type": "tree",
      "sha": "a881069329ac8840ef9cf545727efbb3015cc6f3",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a881069329ac8840ef9cf545727efbb3015cc6f3"
    },
    {
      "path": "config/manifests/benchmark",
      "mode": "040000",
      "type": "tree",
      "sha": "36e10ef43d889d1a7bccd920bb036949773e5136",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/36e10ef43d889d1a7bccd920bb036949773e5136"
    },
    {
      "path": "config/manifests/benchmark/benchmark.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "5cc44ec7fa1c48d6219189a21f5b2dc7a824490e",
      "size": 1654,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5cc44ec7fa1c48d6219189a21f5b2dc7a824490e"
    },
    {
      "path": "config/manifests/benchmark/model-server-service.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "014054cf87cb5303120fd6a1651ed4d45c416533",
      "size": 182,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/014054cf87cb5303120fd6a1651ed4d45c416533"
    },
    {
      "path": "config/manifests/gateway",
      "mode": "040000",
      "type": "tree",
      "sha": "0586bc0f949be3c4d958f84c1bc95f266f343f0e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0586bc0f949be3c4d958f84c1bc95f266f343f0e"
    },
    {
      "path": "config/manifests/gateway/agentgateway",
      "mode": "040000",
      "type": "tree",
      "sha": "a7e80ff44f1144f809822aa8b328779a3b5b7115",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a7e80ff44f1144f809822aa8b328779a3b5b7115"
    },
    {
      "path": "config/manifests/gateway/agentgateway/gateway.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "9407cc7333651f2ba738f492d4b26efa2572e182",
      "size": 190,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9407cc7333651f2ba738f492d4b26efa2572e182"
    },
    {
      "path": "config/manifests/gateway/envoyaigateway",
      "mode": "040000",
      "type": "tree",
      "sha": "2c055b231a14ca541f81fc3bac53a90ab08f4cc5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2c055b231a14ca541f81fc3bac53a90ab08f4cc5"
    },
    {
      "path": "config/manifests/gateway/envoyaigateway/gateway.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1a536411be1d7e85426637318f9dc8097f0a3127",
      "size": 369,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1a536411be1d7e85426637318f9dc8097f0a3127"
    },
    {
      "path": "config/manifests/gateway/gke",
      "mode": "040000",
      "type": "tree",
      "sha": "f6caec6617f39547fdf8f78750c2f4c67adfce5b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f6caec6617f39547fdf8f78750c2f4c67adfce5b"
    },
    {
      "path": "config/manifests/gateway/gke/gateway.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "942cde5c9e55ea0e0211ca96f414917cf8ae5dc5",
      "size": 204,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/942cde5c9e55ea0e0211ca96f414917cf8ae5dc5"
    },
    {
      "path": "config/manifests/gateway/istio",
      "mode": "040000",
      "type": "tree",
      "sha": "81f069f13c493958f78b7b0a47f58e095ad535d6",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/81f069f13c493958f78b7b0a47f58e095ad535d6"
    },
    {
      "path": "config/manifests/gateway/istio/gateway.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "dd762678e8721e9d917d954db0b08463026c9f8b",
      "size": 183,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dd762678e8721e9d917d954db0b08463026c9f8b"
    },
    {
      "path": "config/manifests/gateway/nginxgatewayfabric",
      "mode": "040000",
      "type": "tree",
      "sha": "a701a1c076ef913fd9b5764bf7b5dc7742cae39b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a701a1c076ef913fd9b5764bf7b5dc7742cae39b"
    },
    {
      "path": "config/manifests/gateway/nginxgatewayfabric/gateway.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "cdc1308ea52077af82cc374f81c6d9ffaf43bb43",
      "size": 183,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cdc1308ea52077af82cc374f81c6d9ffaf43bb43"
    },
    {
      "path": "config/manifests/inferenceobjective.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "7a8b6910bb2cc998a6f6fbf9437e1252b9342507",
      "size": 557,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a8b6910bb2cc998a6f6fbf9437e1252b9342507"
    },
    {
      "path": "config/manifests/regression-testing",
      "mode": "040000",
      "type": "tree",
      "sha": "d05dad8eaf0216d171847c215035c501bdea18d6",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d05dad8eaf0216d171847c215035c501bdea18d6"
    },
    {
      "path": "config/manifests/regression-testing/inferenceobjective.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "6971f64644b99f39985b79205da8b3d98c22e727",
      "size": 2947,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6971f64644b99f39985b79205da8b3d98c22e727"
    },
    {
      "path": "config/manifests/regression-testing/multi-lora-regression.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c57a44807193f28f5001a0e0ebdff6eb89e548fe",
      "size": 1798,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c57a44807193f28f5001a0e0ebdff6eb89e548fe"
    },
    {
      "path": "config/manifests/regression-testing/single-workload-regression.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "510773a36e2c189002e3379e6b2ca20286df927c",
      "size": 1512,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/510773a36e2c189002e3379e6b2ca20286df927c"
    },
    {
      "path": "config/manifests/regression-testing/vllm",
      "mode": "040000",
      "type": "tree",
      "sha": "ffee081f0f0737525a8d106cd97e8e8e68a6e1b9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ffee081f0f0737525a8d106cd97e8e8e68a6e1b9"
    },
    {
      "path": "config/manifests/regression-testing/vllm/multi-lora-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b7d5c28813e65c544c2aa9acaac525e85f8b70b0",
      "size": 12141,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b7d5c28813e65c544c2aa9acaac525e85f8b70b0"
    },
    {
      "path": "config/manifests/sglang",
      "mode": "040000",
      "type": "tree",
      "sha": "f1420f5d9bd849de8a648d2711984b2c5d4abdb9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f1420f5d9bd849de8a648d2711984b2c5d4abdb9"
    },
    {
      "path": "config/manifests/sglang/gpu-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "bbed1cbb55916cb425c39c00b0f422540f617216",
      "size": 2016,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bbed1cbb55916cb425c39c00b0f422540f617216"
    },
    {
      "path": "config/manifests/vllm",
      "mode": "040000",
      "type": "tree",
      "sha": "ee018eee21b4b05a871c1a1b0f14aca3612108c4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ee018eee21b4b05a871c1a1b0f14aca3612108c4"
    },
    {
      "path": "config/manifests/vllm/cpu-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "f14c8c2971b915fa42c5b4a34692548350824a2d",
      "size": 3275,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f14c8c2971b915fa42c5b4a34692548350824a2d"
    },
    {
      "path": "config/manifests/vllm/gpu-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a780dd7a730172d27b247b21eeac8c3e0cd00891",
      "size": 12365,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a780dd7a730172d27b247b21eeac8c3e0cd00891"
    },
    {
      "path": "config/manifests/vllm/gpu-grpc-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1336e9439cf23f3ee37adb2216901aee84d1f903",
      "size": 2047,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1336e9439cf23f3ee37adb2216901aee84d1f903"
    },
    {
      "path": "config/manifests/vllm/gpu-multilora-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "6605985d40951b220229d0cb718d2cc517e0968e",
      "size": 3233,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6605985d40951b220229d0cb718d2cc517e0968e"
    },
    {
      "path": "config/manifests/vllm/gpu-prefix-cache-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "3fbfacfa4af4e495d94b1dc739d4d65cf2996f6c",
      "size": 10823,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3fbfacfa4af4e495d94b1dc739d4d65cf2996f6c"
    },
    {
      "path": "config/manifests/vllm/sim-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "31221e17f26f3187d3b4941df031ccaef4540583",
      "size": 961,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/31221e17f26f3187d3b4941df031ccaef4540583"
    },
    {
      "path": "config/manifests/vllm/sim-grpc-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "2c899aab96eae10096ae712b93b72922e1bedc0c",
      "size": 877,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2c899aab96eae10096ae712b93b72922e1bedc0c"
    },
    {
      "path": "config/observability",
      "mode": "040000",
      "type": "tree",
      "sha": "5fbe41ad5eba7e23a060eebeed6f1cf5efc72517",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5fbe41ad5eba7e23a060eebeed6f1cf5efc72517"
    },
    {
      "path": "config/observability/prometheus",
      "mode": "040000",
      "type": "tree",
      "sha": "de0a00472f0d4b030a2bb0976c5da8ef55cb26f5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/de0a00472f0d4b030a2bb0976c5da8ef55cb26f5"
    },
    {
      "path": "config/observability/prometheus/rbac.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "9fad21783db1c59adb72fb08bbd39b5f7ae2d439",
      "size": 114,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9fad21783db1c59adb72fb08bbd39b5f7ae2d439"
    },
    {
      "path": "config/observability/prometheus/values.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1fb81f8b8ae70d4a3e4cd4248c336e796553c9d3",
      "size": 782,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1fb81f8b8ae70d4a3e4cd4248c336e796553c9d3"
    },
    {
      "path": "deploy",
      "mode": "040000",
      "type": "tree",
      "sha": "ce65228003a22d755d933902dadef12e63da925e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ce65228003a22d755d933902dadef12e63da925e"
    },
    {
      "path": "deploy/components",
      "mode": "040000",
      "type": "tree",
      "sha": "1dcdb734fd04404fd9e84c04f397c865312f93ed",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1dcdb734fd04404fd9e84c04f397c865312f93ed"
    },
    {
      "path": "deploy/components/crds-gateway-api",
      "mode": "040000",
      "type": "tree",
      "sha": "b179fbecc8e97f648510e0678a65d24c8eb6e246",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b179fbecc8e97f648510e0678a65d24c8eb6e246"
    },
    {
      "path": "deploy/components/crds-gateway-api/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "ef6568c04138c3e75a821b5077c72e7c22e94f78",
      "size": 507,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ef6568c04138c3e75a821b5077c72e7c22e94f78"
    },
    {
      "path": "deploy/components/crds-gie",
      "mode": "040000",
      "type": "tree",
      "sha": "973d230c1e8444b8882c528ae175fe6b7c62d44e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/973d230c1e8444b8882c528ae175fe6b7c62d44e"
    },
    {
      "path": "deploy/components/crds-gie/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "d4a17f3302f23dbf8c01c6b8958723bd17db5fc3",
      "size": 648,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d4a17f3302f23dbf8c01c6b8958723bd17db5fc3"
    },
    {
      "path": "deploy/components/crds-istio",
      "mode": "040000",
      "type": "tree",
      "sha": "023ae1b92a5b4b22d67b678f9a964a001a01a98e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/023ae1b92a5b4b22d67b678f9a964a001a01a98e"
    },
    {
      "path": "deploy/components/crds-istio/istio.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "4cb4921c9396ad57664dc887911d45d7e91ca62d",
      "size": 856556,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4cb4921c9396ad57664dc887911d45d7e91ca62d"
    },
    {
      "path": "deploy/components/crds-istio/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "5df27efd341a656e38c505aadd65557e61f9cb8f",
      "size": 443,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5df27efd341a656e38c505aadd65557e61f9cb8f"
    },
    {
      "path": "deploy/components/inference-gateway",
      "mode": "040000",
      "type": "tree",
      "sha": "7e04f2e685f634a35ae0d6f088a3a91661f09fc1",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7e04f2e685f634a35ae0d6f088a3a91661f09fc1"
    },
    {
      "path": "deploy/components/inference-gateway/deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1fd47d02b4f0278fcb6c2bfb60de9a241e6805c8",
      "size": 1732,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1fd47d02b4f0278fcb6c2bfb60de9a241e6805c8"
    },
    {
      "path": "deploy/components/inference-gateway/gateways.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "283d10fe7d832dd7b863f9fda59f88f03e60ef17",
      "size": 160,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/283d10fe7d832dd7b863f9fda59f88f03e60ef17"
    },
    {
      "path": "deploy/components/inference-gateway/httproutes.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "886171e394b8ca491de497c71a20d2543632f3d5",
      "size": 429,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/886171e394b8ca491de497c71a20d2543632f3d5"
    },
    {
      "path": "deploy/components/inference-gateway/inference-pools.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "854ea83981e1fbb640d396e2a525171b2db04faa",
      "size": 671,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/854ea83981e1fbb640d396e2a525171b2db04faa"
    },
    {
      "path": "deploy/components/inference-gateway/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "71cd752c742af3352827e97249b91ec6d7e7cb4e",
      "size": 663,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/71cd752c742af3352827e97249b91ec6d7e7cb4e"
    },
    {
      "path": "deploy/components/inference-gateway/rbac.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "7a7990fb9e0347459ae2c5a3cdd39d74c94833f6",
      "size": 1707,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a7990fb9e0347459ae2c5a3cdd39d74c94833f6"
    },
    {
      "path": "deploy/components/inference-gateway/service-accounts.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a92013a0a787eb1cce5e3f0825599799fdd4982a",
      "size": 66,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a92013a0a787eb1cce5e3f0825599799fdd4982a"
    },
    {
      "path": "deploy/components/inference-gateway/services.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "77e950e551405e7f5b2c02a2fbca040c941ea0ca",
      "size": 412,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/77e950e551405e7f5b2c02a2fbca040c941ea0ca"
    },
    {
      "path": "deploy/components/istio-control-plane",
      "mode": "040000",
      "type": "tree",
      "sha": "01844e40449cbe8ef0a71119898fa0b4da3f2bc4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/01844e40449cbe8ef0a71119898fa0b4da3f2bc4"
    },
    {
      "path": "deploy/components/istio-control-plane/configmaps.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "00d35f3b266de0a24b9c9207ecf9253daee8dbdd",
      "size": 112996,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/00d35f3b266de0a24b9c9207ecf9253daee8dbdd"
    },
    {
      "path": "deploy/components/istio-control-plane/deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "5414c976e6903e55a5425e0bc2cc1ffd6e4cf247",
      "size": 6176,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5414c976e6903e55a5425e0bc2cc1ffd6e4cf247"
    },
    {
      "path": "deploy/components/istio-control-plane/hpa.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "011f3b133f9354b8bce2dd4dc857168210703521",
      "size": 798,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/011f3b133f9354b8bce2dd4dc857168210703521"
    },
    {
      "path": "deploy/components/istio-control-plane/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "2726edf358e774f2a23777d61c7d87e1280d34cd",
      "size": 224,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2726edf358e774f2a23777d61c7d87e1280d34cd"
    },
    {
      "path": "deploy/components/istio-control-plane/namespaces.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a236b9589dc3775721b87e64cc067fefd3d5ba99",
      "size": 68,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a236b9589dc3775721b87e64cc067fefd3d5ba99"
    },
    {
      "path": "deploy/components/istio-control-plane/policies.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b79896e4a61dd1489418c036f0343c290e3a7119",
      "size": 633,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b79896e4a61dd1489418c036f0343c290e3a7119"
    },
    {
      "path": "deploy/components/istio-control-plane/rbac.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "f42854786e573aafa998247cf157d9d56f8ad070",
      "size": 11182,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f42854786e573aafa998247cf157d9d56f8ad070"
    },
    {
      "path": "deploy/components/istio-control-plane/service-accounts.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "037f7242e89b448e10c7f5ab2b0cad606b99e561",
      "size": 793,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/037f7242e89b448e10c7f5ab2b0cad606b99e561"
    },
    {
      "path": "deploy/components/istio-control-plane/services.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "7ddf259949a04eb587e3ab3a2d9dcc61a36a998a",
      "size": 922,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7ddf259949a04eb587e3ab3a2d9dcc61a36a998a"
    },
    {
      "path": "deploy/components/istio-control-plane/telemetry.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a6d1c9f21f238d8f088b2b66a9200cf0e7a9dfe3",
      "size": 201,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a6d1c9f21f238d8f088b2b66a9200cf0e7a9dfe3"
    },
    {
      "path": "deploy/components/istio-control-plane/webhooks.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "e776fe84e97ea39da17f4091ac717614c6a005b7",
      "size": 4910,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e776fe84e97ea39da17f4091ac717614c6a005b7"
    },
    {
      "path": "deploy/components/monitoring",
      "mode": "040000",
      "type": "tree",
      "sha": "7e796173e568eee0f363991b7f25607fb907c6d8",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7e796173e568eee0f363991b7f25607fb907c6d8"
    },
    {
      "path": "deploy/components/monitoring/coordinator-service-monitor.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "2d87f724e987bef9a43f34265d2d14ffbc2f629b",
      "size": 267,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2d87f724e987bef9a43f34265d2d14ffbc2f629b"
    },
    {
      "path": "deploy/components/monitoring/epp-service-monitor.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "81bca4a3ac508e9cfe2116675d9a397af60de364",
      "size": 245,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/81bca4a3ac508e9cfe2116675d9a397af60de364"
    },
    {
      "path": "deploy/components/monitoring/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b7bb3f00a7f9c9a109a17175ea0e165389c63d49",
      "size": 166,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b7bb3f00a7f9c9a109a17175ea0e165389c63d49"
    },
    {
      "path": "deploy/components/monitoring/vllm-sim-pod-monitor.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1d210da305e5d75f454f2b1c841cd8965c4269df",
      "size": 267,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1d210da305e5d75f454f2b1c841cd8965c4269df"
    },
    {
      "path": "deploy/components/overlays",
      "mode": "040000",
      "type": "tree",
      "sha": "ea0dffa7bb80c346ca53f14d1ca55cd33cf65b9c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ea0dffa7bb80c346ca53f14d1ca55cd33cf65b9c"
    },
    {
      "path": "deploy/components/overlays/simulator",
      "mode": "040000",
      "type": "tree",
      "sha": "897372c819312111a58a52ac4190ac8d04271e47",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/897372c819312111a58a52ac4190ac8d04271e47"
    },
    {
      "path": "deploy/components/overlays/simulator/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b2cce3cb9899a1adb567d8fc7035bbaa252ae2c1",
      "size": 1703,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b2cce3cb9899a1adb567d8fc7035bbaa252ae2c1"
    },
    {
      "path": "deploy/components/vllm-decode",
      "mode": "040000",
      "type": "tree",
      "sha": "6fb77795c5f4bedf61cd85d91b75112f36cf403b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6fb77795c5f4bedf61cd85d91b75112f36cf403b"
    },
    {
      "path": "deploy/components/vllm-decode/deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "dd6715fcf90f3d27b64459b802fdb3fd44189f20",
      "size": 4184,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dd6715fcf90f3d27b64459b802fdb3fd44189f20"
    },
    {
      "path": "deploy/components/vllm-decode/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "dba9606daca20ae0064ba849ad1d01190f370a7a",
      "size": 625,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dba9606daca20ae0064ba849ad1d01190f370a7a"
    },
    {
      "path": "deploy/components/vllm-encode",
      "mode": "040000",
      "type": "tree",
      "sha": "30989205a80150fe3f1cce0bae7060da273344ed",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/30989205a80150fe3f1cce0bae7060da273344ed"
    },
    {
      "path": "deploy/components/vllm-encode/deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "6c287a684ab19a35f11f34b1e242e518bea901a9",
      "size": 3001,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6c287a684ab19a35f11f34b1e242e518bea901a9"
    },
    {
      "path": "deploy/components/vllm-encode/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "35f8fc66f2731a9e226c91eb934e56e6e1ebbde3",
      "size": 694,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/35f8fc66f2731a9e226c91eb934e56e6e1ebbde3"
    },
    {
      "path": "deploy/components/vllm-encode/pvc.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "450f13917a51b2b68759ed2607f56cc54f15b04d",
      "size": 159,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/450f13917a51b2b68759ed2607f56cc54f15b04d"
    },
    {
      "path": "deploy/components/vllm-prefill",
      "mode": "040000",
      "type": "tree",
      "sha": "d202f766872bff1f0263ed2c0eb8f2b8518d5395",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d202f766872bff1f0263ed2c0eb8f2b8518d5395"
    },
    {
      "path": "deploy/components/vllm-prefill/deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a97a7b6464d50ef933358c19fecc30d8e7886ae7",
      "size": 2943,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a97a7b6464d50ef933358c19fecc30d8e7886ae7"
    },
    {
      "path": "deploy/components/vllm-prefill/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "251ad4e3df747067ef3948646cee29b671d27076",
      "size": 421,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/251ad4e3df747067ef3948646cee29b671d27076"
    },
    {
      "path": "deploy/config",
      "mode": "040000",
      "type": "tree",
      "sha": "511ce370448ec283941c72fc4c746b3c81e94d9d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/511ce370448ec283941c72fc4c746b3c81e94d9d"
    },
    {
      "path": "deploy/config/dp-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "36c23aa8be5eb91a9262e03e1990878909ec26a0",
      "size": 432,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/36c23aa8be5eb91a9262e03e1990878909ec26a0"
    },
    {
      "path": "deploy/config/epp-burst-prefix-cache-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "68dbdcddca5364663437109c81b0b0033f77c8c4",
      "size": 1362,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/68dbdcddca5364663437109c81b0b0033f77c8c4"
    },
    {
      "path": "deploy/config/epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "fb3bc1b2b5d088cdca98f8af8d24103c67391962",
      "size": 382,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fb3bc1b2b5d088cdca98f8af8d24103c67391962"
    },
    {
      "path": "deploy/config/epp-estimate-prefix-cache-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "7b86dbf886af612badee724086348942e46494da",
      "size": 505,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7b86dbf886af612badee724086348942e46494da"
    },
    {
      "path": "deploy/config/epp-mm-embeddings-cache-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "97fd9a24b522e6b0a2ab8ff207a69c7a9f598291",
      "size": 1524,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/97fd9a24b522e6b0a2ab8ff207a69c7a9f598291"
    },
    {
      "path": "deploy/config/epp-precise-prefix-cache-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "64139d3eac03f75bbec1f398440957279cb3afe9",
      "size": 1513,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/64139d3eac03f75bbec1f398440957279cb3afe9"
    },
    {
      "path": "deploy/config/pd-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a9e4c7a9be1e670cb80fbd9b6201a366e3cd67e3",
      "size": 896,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a9e4c7a9be1e670cb80fbd9b6201a366e3cd67e3"
    },
    {
      "path": "deploy/config/pd-topology-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "74e5625cb69dfb97b2a3f59bdb178b3c5c69ec73",
      "size": 1329,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/74e5625cb69dfb97b2a3f59bdb178b3c5c69ec73"
    },
    {
      "path": "deploy/config/probabilistic-admitter-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "16726db18a93ac942e4c9896aa4cad8ff89aad20",
      "size": 1076,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/16726db18a93ac942e4c9896aa4cad8ff89aad20"
    },
    {
      "path": "deploy/config/sim-e-p-d-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "dba26f24795ef03f784aee52c19a2aefb797e97a",
      "size": 731,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dba26f24795ef03f784aee52c19a2aefb797e97a"
    },
    {
      "path": "deploy/config/sim-e-pd-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c0d5f967480e8511878bc202bf435fe374f62fac",
      "size": 597,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c0d5f967480e8511878bc202bf435fe374f62fac"
    },
    {
      "path": "deploy/config/sim-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "68e718cfb254cdebdf56e1cbc80e639e9864dced",
      "size": 550,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/68e718cfb254cdebdf56e1cbc80e639e9864dced"
    },
    {
      "path": "deploy/config/sim-epp-gpu-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1da399c1bd2d91b06c01ed346758773230f448c0",
      "size": 1401,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1da399c1bd2d91b06c01ed346758773230f448c0"
    },
    {
      "path": "deploy/config/sim-epp-kvcache-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c6842e738d49fea995e65cd43696c1478d33c023",
      "size": 1126,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c6842e738d49fea995e65cd43696c1478d33c023"
    },
    {
      "path": "deploy/config/sim-epp-no-hit-lru.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a69040d951e1b0a62aa336e4c930151910d9ac8e",
      "size": 718,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a69040d951e1b0a62aa336e4c930151910d9ac8e"
    },
    {
      "path": "deploy/config/sim-epp-tokenizer-vllm-http-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "7bcfdc26bd312b2047343b73b0e3735c67617eb5",
      "size": 1062,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7bcfdc26bd312b2047343b73b0e3735c67617eb5"
    },
    {
      "path": "deploy/config/sim-pd-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "95262c303ec9161c9f30034741f7d1503f27dd1a",
      "size": 997,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/95262c303ec9161c9f30034741f7d1503f27dd1a"
    },
    {
      "path": "deploy/config/sim-program-aware-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "4d5920153979a8d4d398bd6610d2242b8e20c786",
      "size": 415,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4d5920153979a8d4d398bd6610d2242b8e20c786"
    },
    {
      "path": "deploy/config/soft-reflective-ceiling-epp-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "282365311b181347b3c443aada177b2ef004ed76",
      "size": 844,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/282365311b181347b3c443aada177b2ef004ed76"
    },
    {
      "path": "deploy/coordinator",
      "mode": "040000",
      "type": "tree",
      "sha": "ffa9a90596099bf7cc30f52e3d2e2524f745f8b9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ffa9a90596099bf7cc30f52e3d2e2524f745f8b9"
    },
    {
      "path": "deploy/coordinator/deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "57d9bba114139f5ba64c8b07e1e796def29da542",
      "size": 1472,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/57d9bba114139f5ba64c8b07e1e796def29da542"
    },
    {
      "path": "deploy/coordinator/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b9aff9f6ec55751021dbc1ebf203744f2f207d0d",
      "size": 117,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b9aff9f6ec55751021dbc1ebf203744f2f207d0d"
    },
    {
      "path": "deploy/coordinator/service-account.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1fd336091caf67e1aa7d97d654fdc62edc617398",
      "size": 72,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1fd336091caf67e1aa7d97d654fdc62edc617398"
    },
    {
      "path": "deploy/environments",
      "mode": "040000",
      "type": "tree",
      "sha": "0bc4779932b9583a0f08e97fda505b53551dc03c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0bc4779932b9583a0f08e97fda505b53551dc03c"
    },
    {
      "path": "deploy/environments/dev",
      "mode": "040000",
      "type": "tree",
      "sha": "b8d7c42cdf5bcbde81bcc62fa268d19296da6e05",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b8d7c42cdf5bcbde81bcc62fa268d19296da6e05"
    },
    {
      "path": "deploy/environments/dev/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "0f77ea988fff4b577a8466b191cc2f870f387234",
      "size": 5162,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0f77ea988fff4b577a8466b191cc2f870f387234"
    },
    {
      "path": "deploy/environments/dev/base-kind-istio",
      "mode": "040000",
      "type": "tree",
      "sha": "9324967fe95ec0802f7b07b9730c455dddc7203a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/9324967fe95ec0802f7b07b9730c455dddc7203a"
    },
    {
      "path": "deploy/environments/dev/base-kind-istio/destination-rules.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "735a9f27db4c5865edafb7b1daa0ca9c0db19465",
      "size": 252,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/735a9f27db4c5865edafb7b1daa0ca9c0db19465"
    },
    {
      "path": "deploy/environments/dev/base-kind-istio/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "9cffaff0bb61929b05e36f04e86ebd04e534186e",
      "size": 781,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9cffaff0bb61929b05e36f04e86ebd04e534186e"
    },
    {
      "path": "deploy/environments/dev/base-kind-istio/patch-deployments.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a60bd69c3276e09ff717fcb1176afd8c3a3450d0",
      "size": 635,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a60bd69c3276e09ff717fcb1176afd8c3a3450d0"
    },
    {
      "path": "deploy/environments/dev/base-kind-istio/patch-gateways.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "f73a7a3adf03692bea51f679691fa2c112990bc4",
      "size": 274,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f73a7a3adf03692bea51f679691fa2c112990bc4"
    },
    {
      "path": "deploy/environments/dev/base-kind-istio/services.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "2a92f580cf0d02042af730d65bd028b15a734fda",
      "size": 653,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2a92f580cf0d02042af730d65bd028b15a734fda"
    },
    {
      "path": "deploy/environments/dev/base-kind-istio/vllm-render.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b09d71b204bb24f00f786a1b98db99760d4a84a0",
      "size": 1804,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b09d71b204bb24f00f786a1b98db99760d4a84a0"
    },
    {
      "path": "deploy/environments/dev/coordinator-e2e-infra",
      "mode": "040000",
      "type": "tree",
      "sha": "a44c639b619c2b7b614cda469c81c4e2b2e1a8fc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a44c639b619c2b7b614cda469c81c4e2b2e1a8fc"
    },
    {
      "path": "deploy/environments/dev/coordinator-e2e-infra/envoy-3-epp.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "987c6ce842763cf0de640e387df6b13d64f933ff",
      "size": 20110,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/987c6ce842763cf0de640e387df6b13d64f933ff"
    },
    {
      "path": "deploy/environments/dev/coordinator-e2e-infra/envoy.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "5eb0f2aad9705cadb25c2eed58bcc85526f180be",
      "size": 13765,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5eb0f2aad9705cadb25c2eed58bcc85526f180be"
    },
    {
      "path": "deploy/environments/dev/coordinator-e2e-infra/inference-pools-3-epp.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "5f471d2d4efc45497535bcf2538e47b8cdb30ce9",
      "size": 1545,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5f471d2d4efc45497535bcf2538e47b8cdb30ce9"
    },
    {
      "path": "deploy/environments/dev/coordinator-e2e-infra/shared-envoy-resources.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "98966f6bb60d11b2d368b23a70e9299e974c28ea",
      "size": 2704,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/98966f6bb60d11b2d368b23a70e9299e974c28ea"
    },
    {
      "path": "deploy/environments/dev/coordinator-epd",
      "mode": "040000",
      "type": "tree",
      "sha": "b74d0da5c60f4b366b92e188fa8bb9bc3c008b0d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b74d0da5c60f4b366b92e188fa8bb9bc3c008b0d"
    },
    {
      "path": "deploy/environments/dev/coordinator-epd/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c1bd7fcf55f977ddf597f7795896906ac3f0adff",
      "size": 781,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c1bd7fcf55f977ddf597f7795896906ac3f0adff"
    },
    {
      "path": "deploy/environments/dev/coordinator-epd/patch-decode.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "3203985d1fb82e2a0845bb73849a13c76afa04aa",
      "size": 1622,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3203985d1fb82e2a0845bb73849a13c76afa04aa"
    },
    {
      "path": "deploy/environments/dev/e-p-d",
      "mode": "040000",
      "type": "tree",
      "sha": "613346509d997b183a43109c3c8d2ee0b18192b7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/613346509d997b183a43109c3c8d2ee0b18192b7"
    },
    {
      "path": "deploy/environments/dev/e-p-d/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "9051447d4067ce9a73b2e4fdd48caa28b011bd03",
      "size": 568,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9051447d4067ce9a73b2e4fdd48caa28b011bd03"
    },
    {
      "path": "deploy/environments/dev/e-p-d/patch-decode.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "5b1e6d43adfc287ce9642f469627c3b0ee88c6d2",
      "size": 427,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5b1e6d43adfc287ce9642f469627c3b0ee88c6d2"
    },
    {
      "path": "deploy/environments/dev/e-pd",
      "mode": "040000",
      "type": "tree",
      "sha": "d3afee48a1095a56c002a6bdb46b41d2e3fa122d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d3afee48a1095a56c002a6bdb46b41d2e3fa122d"
    },
    {
      "path": "deploy/environments/dev/e-pd/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "bb7b0e07dfab58e8a2b461d9f030734fbb312c08",
      "size": 514,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bb7b0e07dfab58e8a2b461d9f030734fbb312c08"
    },
    {
      "path": "deploy/environments/dev/e-pd/patch-decode.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "177d03d4161eaa931d1478fa2aac6880a14c8e7b",
      "size": 387,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/177d03d4161eaa931d1478fa2aac6880a14c8e7b"
    },
    {
      "path": "deploy/environments/dev/e2e-infra",
      "mode": "040000",
      "type": "tree",
      "sha": "2c2285343945b789c134230454aacd4b0d401d46",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2c2285343945b789c134230454aacd4b0d401d46"
    },
    {
      "path": "deploy/environments/dev/e2e-infra/envoy.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "84b31777e8fac339b6da77f9e7fd1ff2179c260c",
      "size": 11330,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/84b31777e8fac339b6da77f9e7fd1ff2179c260c"
    },
    {
      "path": "deploy/environments/dev/e2e-infra/services.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "9b19d88d8dfed0c9542ff708ceb7c7f81b11025b",
      "size": 547,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9b19d88d8dfed0c9542ff708ceb7c7f81b11025b"
    },
    {
      "path": "deploy/environments/dev/e2e-infra/vllm-render.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "f3b425f562e771356ae2bf9cbf1f320ac02f9431",
      "size": 1459,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f3b425f562e771356ae2bf9cbf1f320ac02f9431"
    },
    {
      "path": "deploy/environments/dev/epd",
      "mode": "040000",
      "type": "tree",
      "sha": "961ec9799614087b1d45e346bebbbb80bc3619da",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/961ec9799614087b1d45e346bebbbb80bc3619da"
    },
    {
      "path": "deploy/environments/dev/epd/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "89dec1e824210a12ac20aaa26b28a7d8a72870f7",
      "size": 518,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/89dec1e824210a12ac20aaa26b28a7d8a72870f7"
    },
    {
      "path": "deploy/environments/dev/epd/patch-decode.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a48914ae15c800bf3431135644e37bbcf7f7c34c",
      "size": 1437,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a48914ae15c800bf3431135644e37bbcf7f7c34c"
    },
    {
      "path": "deploy/environments/dev/kubernetes-kgateway",
      "mode": "040000",
      "type": "tree",
      "sha": "34cee6e7db9083c46ab5e979f1918515992854ff",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/34cee6e7db9083c46ab5e979f1918515992854ff"
    },
    {
      "path": "deploy/environments/dev/kubernetes-kgateway/gateway-parameters.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "da2d91d29d67412c3bcaf1e0a78ff71d8d2bf007",
      "size": 526,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/da2d91d29d67412c3bcaf1e0a78ff71d8d2bf007"
    },
    {
      "path": "deploy/environments/dev/kubernetes-kgateway/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "01187f3be623cd7420ebf94d388ad630f83af31d",
      "size": 251,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/01187f3be623cd7420ebf94d388ad630f83af31d"
    },
    {
      "path": "deploy/environments/dev/kubernetes-kgateway/patch-deployments.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "4bddd5b79da7c8846bb55b740e5a5b44f9aa3645",
      "size": 741,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4bddd5b79da7c8846bb55b740e5a5b44f9aa3645"
    },
    {
      "path": "deploy/environments/dev/kubernetes-kgateway/patch-gateways.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "d923204403e2ba420dcf0af7f151c5b56732baee",
      "size": 256,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d923204403e2ba420dcf0af7f151c5b56732baee"
    },
    {
      "path": "deploy/environments/dev/kubernetes-kgateway/secret.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "693cb12b332d33bf037cf58bb973d5598f11df6e",
      "size": 184,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/693cb12b332d33bf037cf58bb973d5598f11df6e"
    },
    {
      "path": "deploy/environments/dev/p-d",
      "mode": "040000",
      "type": "tree",
      "sha": "9523bc146e823f4ea02205f361d6f8ecb5c8c468",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/9523bc146e823f4ea02205f361d6f8ecb5c8c468"
    },
    {
      "path": "deploy/environments/dev/p-d/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "8a89f103db777a0bd82808846a43eeb8ca975659",
      "size": 524,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8a89f103db777a0bd82808846a43eeb8ca975659"
    },
    {
      "path": "deploy/environments/dev/p-d/patch-decode.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "8c9948b978391e85b4f4eb15078d56a2dde06145",
      "size": 419,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8c9948b978391e85b4f4eb15078d56a2dde06145"
    },
    {
      "path": "deploy/environments/kubernetes-base",
      "mode": "040000",
      "type": "tree",
      "sha": "55c9f12c8671d557b1aa9808160171188a4e8538",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/55c9f12c8671d557b1aa9808160171188a4e8538"
    },
    {
      "path": "deploy/environments/kubernetes-base/common",
      "mode": "040000",
      "type": "tree",
      "sha": "43de4d9781de8918500930cb8fc5e5c2bb177811",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/43de4d9781de8918500930cb8fc5e5c2bb177811"
    },
    {
      "path": "deploy/environments/kubernetes-base/common/patch-service.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "d15ea23f196cd82c925a183c1b6079f70772f236",
      "size": 194,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d15ea23f196cd82c925a183c1b6079f70772f236"
    },
    {
      "path": "deploy/environments/kubernetes-base/common/patch-statefulset.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c3a8a39cda511a6a9df0a3ec549ab6d984cae7ba",
      "size": 432,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c3a8a39cda511a6a9df0a3ec549ab6d984cae7ba"
    },
    {
      "path": "deploy/environments/kubernetes-base/common/service.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "8abb7e7e8c112c7fbf4a5c03e2141c6cbbef71f5",
      "size": 181,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8abb7e7e8c112c7fbf4a5c03e2141c6cbbef71f5"
    },
    {
      "path": "deploy/environments/kubernetes-base/common/statefulset.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "8f648815f7777940ceab673af25684406c9e5d0a",
      "size": 415,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8f648815f7777940ceab673af25684406c9e5d0a"
    },
    {
      "path": "deploy/environments/kubernetes-base/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "ab3306c647c0e4831a2531ae2efa1d4ab657f9e9",
      "size": 968,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ab3306c647c0e4831a2531ae2efa1d4ab657f9e9"
    },
    {
      "path": "deploy/environments/kubernetes-base/openshift",
      "mode": "040000",
      "type": "tree",
      "sha": "b0942b966a12520941df8626a7313e3442b56cb7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b0942b966a12520941df8626a7313e3442b56cb7"
    },
    {
      "path": "deploy/environments/kubernetes-base/openshift/patch-route.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "0b7d63ff0a6ef7728c404881571087bc37aa0eb5",
      "size": 118,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0b7d63ff0a6ef7728c404881571087bc37aa0eb5"
    },
    {
      "path": "deploy/environments/kubernetes-base/openshift/route.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "d140451618226bc2abb1c7a43c0334756f9e142e",
      "size": 180,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d140451618226bc2abb1c7a43c0334756f9e142e"
    },
    {
      "path": "deploy/environments/kubernetes-base/rbac",
      "mode": "040000",
      "type": "tree",
      "sha": "4adb4316e536f39eec4e05ebba321968c7a35f47",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4adb4316e536f39eec4e05ebba321968c7a35f47"
    },
    {
      "path": "deploy/environments/kubernetes-base/rbac/exec-rbac-role.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "fb6ac88b0fd15ffb7288c50904d12ce7ad70121b",
      "size": 197,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fb6ac88b0fd15ffb7288c50904d12ce7ad70121b"
    },
    {
      "path": "deploy/environments/kubernetes-base/rbac/exec-rbac-rolebinding.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "4c6ccf97288a2690e201e6619fb82cf76938829a",
      "size": 270,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4c6ccf97288a2690e201e6619fb82cf76938829a"
    },
    {
      "path": "deploy/environments/kubernetes-base/rbac/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "e05a34652b2dc361403b4a34294fd6c815bd9e55",
      "size": 257,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e05a34652b2dc361403b4a34294fd6c815bd9e55"
    },
    {
      "path": "deploy/environments/kubernetes-base/rbac/patch-rbac-role.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "2ac6fcd3d35b3f5f1b8906e02d80705dbdb4f795",
      "size": 207,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2ac6fcd3d35b3f5f1b8906e02d80705dbdb4f795"
    },
    {
      "path": "deploy/environments/kubernetes-base/rbac/patch-rbac-rolebinding.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "57e8df1d029ad30efdf62239b066fd1bde3cd454",
      "size": 285,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/57e8df1d029ad30efdf62239b066fd1bde3cd454"
    },
    {
      "path": "docs",
      "mode": "040000",
      "type": "tree",
      "sha": "97a18e93a093d0646d4a67dba0027f917a3ed4ea",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/97a18e93a093d0646d4a67dba0027f917a3ed4ea"
    },
    {
      "path": "docs/architecture.md",
      "mode": "100644",
      "type": "blob",
      "sha": "a7cfd45171fce55a3ebca9454f727c85de4a40d6",
      "size": 15509,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a7cfd45171fce55a3ebca9454f727c85de4a40d6"
    },
    {
      "path": "docs/area_taxonomy.md",
      "mode": "100644",
      "type": "blob",
      "sha": "12ff08d2e17084a9370a6a876c2bbf7c07d0dfa5",
      "size": 900,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/12ff08d2e17084a9370a6a876c2bbf7c07d0dfa5"
    },
    {
      "path": "docs/communication.md",
      "mode": "100644",
      "type": "blob",
      "sha": "85598b03312e1f4101f80f405be681b0c6bda61f",
      "size": 42033,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85598b03312e1f4101f80f405be681b0c6bda61f"
    },
    {
      "path": "docs/coordinator_architecture.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1cd4c9c8084e94b294c44fb77822e826afb51a7e",
      "size": 41824,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1cd4c9c8084e94b294c44fb77822e826afb51a7e"
    },
    {
      "path": "docs/coordinator_async_broker.md",
      "mode": "100644",
      "type": "blob",
      "sha": "cad248acd32f83b23dac3338e5038aebdbaec96e",
      "size": 12328,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cad248acd32f83b23dac3338e5038aebdbaec96e"
    },
    {
      "path": "docs/create_new_filter.md",
      "mode": "100644",
      "type": "blob",
      "sha": "c937f77a6eda77d541ec53dad714a9f107172e47",
      "size": 7490,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c937f77a6eda77d541ec53dad714a9f107172e47"
    },
    {
      "path": "docs/disaggregation.md",
      "mode": "100644",
      "type": "blob",
      "sha": "987b03f192252d27463ebed3d498dd248cde5b3d",
      "size": 32720,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/987b03f192252d27463ebed3d498dd248cde5b3d"
    },
    {
      "path": "docs/discovery.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5f970c00203388e1b58dbfa77a5699a03fd8976e",
      "size": 19793,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5f970c00203388e1b58dbfa77a5699a03fd8976e"
    },
    {
      "path": "docs/images",
      "mode": "040000",
      "type": "tree",
      "sha": "67eb56654cddcbbfc903daf95023be47f6054e9b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/67eb56654cddcbbfc903daf95023be47f6054e9b"
    },
    {
      "path": "docs/images/architecture.png",
      "mode": "100644",
      "type": "blob",
      "sha": "59e38574ca8f806cc82f4ff2dd6436a76583fdc3",
      "size": 270465,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/59e38574ca8f806cc82f4ff2dd6436a76583fdc3"
    },
    {
      "path": "docs/images/epd_architecture.png",
      "mode": "100644",
      "type": "blob",
      "sha": "c66c11b850bdcc0b7e41300880a4c69d2a553001",
      "size": 628241,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c66c11b850bdcc0b7e41300880a4c69d2a553001"
    },
    {
      "path": "docs/images/llm-d-router.svg",
      "mode": "100644",
      "type": "blob",
      "sha": "6ed772dd31ab7b3f5bb96bbbdc8e9251a7f853c5",
      "size": 254228,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6ed772dd31ab7b3f5bb96bbbdc8e9251a7f853c5"
    },
    {
      "path": "docs/images/plugability.png",
      "mode": "100644",
      "type": "blob",
      "sha": "48aab05aa7e90ffdaaeaf7cdd3f2dd87444b06e3",
      "size": 169462,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/48aab05aa7e90ffdaaeaf7cdd3f2dd87444b06e3"
    },
    {
      "path": "docs/metrics.coord.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ccf66027f2f79a16d0c0b1579be8b0bbf93c1043",
      "size": 14393,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ccf66027f2f79a16d0c0b1579be8b0bbf93c1043"
    },
    {
      "path": "docs/metrics.md",
      "mode": "100644",
      "type": "blob",
      "sha": "cb064eee46a59f379e189bc8a7d05f2f70135a22",
      "size": 38254,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cb064eee46a59f379e189bc8a7d05f2f70135a22"
    },
    {
      "path": "docs/operations.md",
      "mode": "100644",
      "type": "blob",
      "sha": "d1d5b5af17a331a8aa47ba9637cb96eebaf2dea5",
      "size": 14966,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d1d5b5af17a331a8aa47ba9637cb96eebaf2dea5"
    },
    {
      "path": "docs/peer-discovery.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1d26cb6f93f7d2477ca00ca0fb7b4166c613f93d",
      "size": 6415,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1d26cb6f93f7d2477ca00ca0fb7b4166c613f93d"
    },
    {
      "path": "docs/plugin-metric-protocol.md",
      "mode": "100644",
      "type": "blob",
      "sha": "67ee0d3873801d75e892ec6a42f188961f4d6398",
      "size": 5553,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/67ee0d3873801d75e892ec6a42f188961f4d6398"
    },
    {
      "path": "docs/plugin_debug.md",
      "mode": "100644",
      "type": "blob",
      "sha": "53b4b8f0745bc63eabd01b6263fd4983d669f490",
      "size": 2459,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/53b4b8f0745bc63eabd01b6263fd4983d669f490"
    },
    {
      "path": "docs/plugins-vs-builtins.md",
      "mode": "100644",
      "type": "blob",
      "sha": "81bfd58a84e1621f8403b9187b98e7370719cc05",
      "size": 4102,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/81bfd58a84e1621f8403b9187b98e7370719cc05"
    },
    {
      "path": "go.mod",
      "mode": "100644",
      "type": "blob",
      "sha": "1e0396ca3b4e1279cd8895eba9192112ae981cc5",
      "size": 7133,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1e0396ca3b4e1279cd8895eba9192112ae981cc5"
    },
    {
      "path": "go.sum",
      "mode": "100644",
      "type": "blob",
      "sha": "067fdeba05d6d72fd52df7fc02631d1abc3a28db",
      "size": 34654,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/067fdeba05d6d72fd52df7fc02631d1abc3a28db"
    },
    {
      "path": "hack",
      "mode": "040000",
      "type": "tree",
      "sha": "4f36f6dd70e8cc36786a4475f6f3981a7d637f29",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4f36f6dd70e8cc36786a4475f6f3981a7d637f29"
    },
    {
      "path": "hack/push-chart.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "6abbadb0efefe66fe72d6b4b3ad422ee8ec45026",
      "size": 3763,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6abbadb0efefe66fe72d6b4b3ad422ee8ec45026"
    },
    {
      "path": "hack/referencer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "31fb3df07b8759a901e740fd314d5dde1a2844a9",
      "size": 712,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/31fb3df07b8759a901e740fd314d5dde1a2844a9"
    },
    {
      "path": "hack/testdata",
      "mode": "040000",
      "type": "tree",
      "sha": "938b9d24f56ea029a914b9ca4af69df24a35c38b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/938b9d24f56ea029a914b9ca4af69df24a35c38b"
    },
    {
      "path": "hack/testdata/plugins-config-base.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "f6d06b396d8707cdc4c002798cc019e10aa59faf",
      "size": 584,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f6d06b396d8707cdc4c002798cc019e10aa59faf"
    },
    {
      "path": "hack/testdata/plugins-config-expected.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "8fb805bc8b4db76b405b3b662a8792abb0930e74",
      "size": 226,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8fb805bc8b4db76b405b3b662a8792abb0930e74"
    },
    {
      "path": "hack/testdata/plugins-config-overlay.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a80300564c114cdd96bd0e739690de9d28607e1b",
      "size": 76,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a80300564c114cdd96bd0e739690de9d28607e1b"
    },
    {
      "path": "hack/update-codegen.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "0c4dfb1db0fa39b86b1da67095ae7ffbcc339ee8",
      "size": 1464,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0c4dfb1db0fa39b86b1da67095ae7ffbcc339ee8"
    },
    {
      "path": "hack/verify-helm.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "e47f642d3ae4ab429285ab075b544bc6c19c4f0d",
      "size": 25942,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e47f642d3ae4ab429285ab075b544bc6c19c4f0d"
    },
    {
      "path": "hack/verify-manifests.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "1f37a76b907a8f23ce8636f7ea213d1c4e0caff6",
      "size": 3058,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1f37a76b907a8f23ce8636f7ea213d1c4e0caff6"
    },
    {
      "path": "hack/verify-plugins-config.sh",
      "mode": "100644",
      "type": "blob",
      "sha": "cc9e45036ac918866842e8a178e7046e75f7577a",
      "size": 5277,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cc9e45036ac918866842e8a178e7046e75f7577a"
    },
    {
      "path": "hooks",
      "mode": "040000",
      "type": "tree",
      "sha": "79181082ddd1970beeefd209245601800d49b341",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/79181082ddd1970beeefd209245601800d49b341"
    },
    {
      "path": "hooks/pre-commit",
      "mode": "100755",
      "type": "blob",
      "sha": "aa065a73d89ffb54daf314f50aeb8a90323d35de",
      "size": 147,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/aa065a73d89ffb54daf314f50aeb8a90323d35de"
    },
    {
      "path": "internal",
      "mode": "040000",
      "type": "tree",
      "sha": "ca359e2158663d30f7b030d1a3b12616d26b8c4d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ca359e2158663d30f7b030d1a3b12616d26b8c4d"
    },
    {
      "path": "internal/runnable",
      "mode": "040000",
      "type": "tree",
      "sha": "4cc7c87fd80effd49ef6d5fc1393a8b694d77c3b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4cc7c87fd80effd49ef6d5fc1393a8b694d77c3b"
    },
    {
      "path": "internal/runnable/grpc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8524299467c5372678c0740994344bb1882e1ad1",
      "size": 2776,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8524299467c5372678c0740994344bb1882e1ad1"
    },
    {
      "path": "internal/runnable/leader_election.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8d8ec9c91a21e87bbc2cf8f5857f7e366919f866",
      "size": 798,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8d8ec9c91a21e87bbc2cf8f5857f7e366919f866"
    },
    {
      "path": "internal/tls",
      "mode": "040000",
      "type": "tree",
      "sha": "f3bcbd39f64371bc8e8cb72c0314928972fa5d52",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f3bcbd39f64371bc8e8cb72c0314928972fa5d52"
    },
    {
      "path": "internal/tls/tls.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fb8092c648d7e04b33a39b63f3673173318244ca",
      "size": 2338,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fb8092c648d7e04b33a39b63f3673173318244ca"
    },
    {
      "path": "pkg",
      "mode": "040000",
      "type": "tree",
      "sha": "5bf7dc9ef124396da275b2c4511c99520d6fc194",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5bf7dc9ef124396da275b2c4511c99520d6fc194"
    },
    {
      "path": "pkg/common",
      "mode": "040000",
      "type": "tree",
      "sha": "9dfa1247ca7e000e0ce32bc5a3047f242abd8840",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/9dfa1247ca7e000e0ce32bc5a3047f242abd8840"
    },
    {
      "path": "pkg/common/certs.go",
      "mode": "100644",
      "type": "blob",
      "sha": "49730f5dd09cead2521feb3831ac8705e535975c",
      "size": 2547,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/49730f5dd09cead2521feb3831ac8705e535975c"
    },
    {
      "path": "pkg/common/certs_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "685171cea39066ae3892d0024afe6794730e58d4",
      "size": 11195,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/685171cea39066ae3892d0024afe6794730e58d4"
    },
    {
      "path": "pkg/common/collections",
      "mode": "040000",
      "type": "tree",
      "sha": "2dbebc9ccf65ab51dfb130695e0f146519c6bff3",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2dbebc9ccf65ab51dfb130695e0f146519c6bff3"
    },
    {
      "path": "pkg/common/collections/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "b532d2f37273411e0a4d48305156ec2475dde9ca",
      "size": 289,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b532d2f37273411e0a4d48305156ec2475dde9ca"
    },
    {
      "path": "pkg/common/collections/slices.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e38b0c3977cdea928cf440a70361b20629a8c522",
      "size": 912,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e38b0c3977cdea928cf440a70361b20629a8c522"
    },
    {
      "path": "pkg/common/collections/slices_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "29a25b2a16c700423fe0a5d4e64bd9f823f08d34",
      "size": 1281,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/29a25b2a16c700423fe0a5d4e64bd9f823f08d34"
    },
    {
      "path": "pkg/common/envoy",
      "mode": "040000",
      "type": "tree",
      "sha": "5e7127a09bbf84469b0c89689f762dcf07ca8659",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5e7127a09bbf84469b0c89689f762dcf07ca8659"
    },
    {
      "path": "pkg/common/envoy/chunking.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f538e2505c0c599e24ca8f870afb01e34d7e4720",
      "size": 2179,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f538e2505c0c599e24ca8f870afb01e34d7e4720"
    },
    {
      "path": "pkg/common/envoy/chunking_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e4663759dfc032a21409f19964d8f5fa56eaee00",
      "size": 3221,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e4663759dfc032a21409f19964d8f5fa56eaee00"
    },
    {
      "path": "pkg/common/envoy/headers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e592cd1543056da04a13b6971c71a28e2d6d1579",
      "size": 2017,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e592cd1543056da04a13b6971c71a28e2d6d1579"
    },
    {
      "path": "pkg/common/envoy/headers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "587d306a5166554083e54f299398da2121676eed",
      "size": 5205,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/587d306a5166554083e54f299398da2121676eed"
    },
    {
      "path": "pkg/common/envoy/metadata.go",
      "mode": "100644",
      "type": "blob",
      "sha": "675c92a90525b961e600a28d1bc1b515d3fed7cd",
      "size": 1052,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/675c92a90525b961e600a28d1bc1b515d3fed7cd"
    },
    {
      "path": "pkg/common/envoy/metadata_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c3500194b1d11c32b5fe41c86de1549e547a7188",
      "size": 2224,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c3500194b1d11c32b5fe41c86de1549e547a7188"
    },
    {
      "path": "pkg/common/envoy/request.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2c2c19d44e54db9eb12acdcb068a467378b55066",
      "size": 1366,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2c2c19d44e54db9eb12acdcb068a467378b55066"
    },
    {
      "path": "pkg/common/envoy/response.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6bfb4f60208f623242e5ed691bd9fdf7d05b2212",
      "size": 1327,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6bfb4f60208f623242e5ed691bd9fdf7d05b2212"
    },
    {
      "path": "pkg/common/error",
      "mode": "040000",
      "type": "tree",
      "sha": "002519002fcf9e0c3b11328d4225bc6b995ee9d2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/002519002fcf9e0c3b11328d4225bc6b995ee9d2"
    },
    {
      "path": "pkg/common/error/error.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7454e92e9797ab2eb5032d6054524933cfafeda5",
      "size": 4873,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7454e92e9797ab2eb5032d6054524933cfafeda5"
    },
    {
      "path": "pkg/common/error/error_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "567ffc7b16c186135d6338ffc389facf5ee1a935",
      "size": 8893,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/567ffc7b16c186135d6338ffc389facf5ee1a935"
    },
    {
      "path": "pkg/common/kubemeta.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6197247addb0d2f861608102a318939480bc9cbb",
      "size": 1021,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6197247addb0d2f861608102a318939480bc9cbb"
    },
    {
      "path": "pkg/common/observability",
      "mode": "040000",
      "type": "tree",
      "sha": "2962cf072d4cfecdab3484867657b58941871978",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2962cf072d4cfecdab3484867657b58941871978"
    },
    {
      "path": "pkg/common/observability/logging",
      "mode": "040000",
      "type": "tree",
      "sha": "37adbab178278f4e4408a464ee82a11cb83a467a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/37adbab178278f4e4408a464ee82a11cb83a467a"
    },
    {
      "path": "pkg/common/observability/logging/const.go",
      "mode": "100644",
      "type": "blob",
      "sha": "823ab28b11331dc8994917b041b29df5cdd0277b",
      "size": 650,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/823ab28b11331dc8994917b041b29df5cdd0277b"
    },
    {
      "path": "pkg/common/observability/logging/logger.go",
      "mode": "100644",
      "type": "blob",
      "sha": "34b6a7f7dc5e56d6d3b4a080a3c1d331125d9399",
      "size": 2746,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/34b6a7f7dc5e56d6d3b4a080a3c1d331125d9399"
    },
    {
      "path": "pkg/common/observability/logging/logger_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f6f3a43e602953d3a816938b9066d23d2cd22bb1",
      "size": 2430,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f6f3a43e602953d3a816938b9066d23d2cd22bb1"
    },
    {
      "path": "pkg/common/observability/logging/options.go",
      "mode": "100644",
      "type": "blob",
      "sha": "58bd84c63e44eb88356b50c2ef11806739662dd7",
      "size": 2661,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/58bd84c63e44eb88356b50c2ef11806739662dd7"
    },
    {
      "path": "pkg/common/observability/logging/options_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6069a86952d6152001ec5986a8ef96cfe01d2a68",
      "size": 4225,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6069a86952d6152001ec5986a8ef96cfe01d2a68"
    },
    {
      "path": "pkg/common/observability/logging/otel.go",
      "mode": "100644",
      "type": "blob",
      "sha": "21d545a68937e9b77313c0e655bf3f479a48707e",
      "size": 4969,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/21d545a68937e9b77313c0e655bf3f479a48707e"
    },
    {
      "path": "pkg/common/observability/logging/otel_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5d43298ef0d509b69c213cf89581dbee39ccafd2",
      "size": 3778,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5d43298ef0d509b69c213cf89581dbee39ccafd2"
    },
    {
      "path": "pkg/common/observability/metrics",
      "mode": "040000",
      "type": "tree",
      "sha": "fa12452b69ef86d5d1a1d567037429acf05fd997",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/fa12452b69ef86d5d1a1d567037429acf05fd997"
    },
    {
      "path": "pkg/common/observability/metrics/cardinality.go",
      "mode": "100644",
      "type": "blob",
      "sha": "29ebd3b97cf28dd24c1f0100ab3a80efbd42d119",
      "size": 4208,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/29ebd3b97cf28dd24c1f0100ab3a80efbd42d119"
    },
    {
      "path": "pkg/common/observability/metrics/cardinality_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "aef13b2a31cc64e5e57dee5b2ea349054b2ead94",
      "size": 2411,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/aef13b2a31cc64e5e57dee5b2ea349054b2ead94"
    },
    {
      "path": "pkg/common/observability/metrics/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "28377ac2670cf31e5e0619fd1f86a408892c1224",
      "size": 2307,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/28377ac2670cf31e5e0619fd1f86a408892c1224"
    },
    {
      "path": "pkg/common/observability/profiling",
      "mode": "040000",
      "type": "tree",
      "sha": "158638c05e5a37d449ba9e86f8659765786ee1bc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/158638c05e5a37d449ba9e86f8659765786ee1bc"
    },
    {
      "path": "pkg/common/observability/profiling/pprof.go",
      "mode": "100644",
      "type": "blob",
      "sha": "57f6fdc603c9b9cfd87153dbab61671701d7fab7",
      "size": 2049,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/57f6fdc603c9b9cfd87153dbab61671701d7fab7"
    },
    {
      "path": "pkg/common/observability/semconv",
      "mode": "040000",
      "type": "tree",
      "sha": "0eca63b6fe12fe63448538add5d7821ad0f7f9dd",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0eca63b6fe12fe63448538add5d7821ad0f7f9dd"
    },
    {
      "path": "pkg/common/observability/semconv/genai.go",
      "mode": "100644",
      "type": "blob",
      "sha": "06f43353e68e58b16cafaeb5f16dd84730f0bd00",
      "size": 1593,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/06f43353e68e58b16cafaeb5f16dd84730f0bd00"
    },
    {
      "path": "pkg/common/observability/semconv/genai_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4a83e897bbbcbb84e88b1ceda06b852d10e6becb",
      "size": 1438,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4a83e897bbbcbb84e88b1ceda06b852d10e6becb"
    },
    {
      "path": "pkg/common/observability/semconv/llm_d.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6016bf8903adaee6042e0bea99973f843b19547d",
      "size": 34802,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6016bf8903adaee6042e0bea99973f843b19547d"
    },
    {
      "path": "pkg/common/observability/semconv/llm_d_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f75095fbf329b41fd929951a094f9801b0664916",
      "size": 21397,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f75095fbf329b41fd929951a094f9801b0664916"
    },
    {
      "path": "pkg/common/observability/tracing",
      "mode": "040000",
      "type": "tree",
      "sha": "175504d388bc497e82026445e9de4f64c470bd0e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/175504d388bc497e82026445e9de4f64c470bd0e"
    },
    {
      "path": "pkg/common/observability/tracing/logging.go",
      "mode": "100644",
      "type": "blob",
      "sha": "db0bc2ae9fce074387b423de91fc6a420546834d",
      "size": 2586,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/db0bc2ae9fce074387b423de91fc6a420546834d"
    },
    {
      "path": "pkg/common/observability/tracing/logging_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "11174d916554cc498780ffe8fbf0c820eaacfa03",
      "size": 5542,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/11174d916554cc498780ffe8fbf0c820eaacfa03"
    },
    {
      "path": "pkg/common/observability/tracing/telemetry.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6c8064206179465ca608f540ab4ab5cf6653507d",
      "size": 13814,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6c8064206179465ca608f540ab4ab5cf6653507d"
    },
    {
      "path": "pkg/common/observability/tracing/telemetry_otlp_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c6c7db497208450fc2affee99614b1e4aac19ceb",
      "size": 20482,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c6c7db497208450fc2affee99614b1e4aac19ceb"
    },
    {
      "path": "pkg/common/observability/tracing/telemetry_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0970a792466be38a233c9c7d9ea128140d4017c9",
      "size": 17380,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0970a792466be38a233c9c7d9ea128140d4017c9"
    },
    {
      "path": "pkg/common/request",
      "mode": "040000",
      "type": "tree",
      "sha": "83bebdce449a45ff3a1ea1f1a06539955ab9bfc5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/83bebdce449a45ff3a1ea1f1a06539955ab9bfc5"
    },
    {
      "path": "pkg/common/request/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b294b087930406977c4f6c5cd971cde9697e49fb",
      "size": 1951,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b294b087930406977c4f6c5cd971cde9697e49fb"
    },
    {
      "path": "pkg/common/request/tokens.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1506fb997b5582c701ab1f853c4ef3388c74b58e",
      "size": 1630,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1506fb997b5582c701ab1f853c4ef3388c74b58e"
    },
    {
      "path": "pkg/common/request/tokens_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "405baa3f82edec4c0e903bbbe785a8e417293ab5",
      "size": 3487,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/405baa3f82edec4c0e903bbbe785a8e417293ab5"
    },
    {
      "path": "pkg/common/routing",
      "mode": "040000",
      "type": "tree",
      "sha": "ccf897a2397320a208fc52086a2ac30bb07522c2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ccf897a2397320a208fc52086a2ac30bb07522c2"
    },
    {
      "path": "pkg/common/routing/common.go",
      "mode": "100644",
      "type": "blob",
      "sha": "49dbe2d5419b4494c3b9b39d2707db6ac680bae9",
      "size": 2602,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/49dbe2d5419b4494c3b9b39d2707db6ac680bae9"
    },
    {
      "path": "pkg/common/routing/common_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ded2ada53b0f49c651c0c9414fc52da77351c458",
      "size": 3271,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ded2ada53b0f49c651c0c9414fc52da77351c458"
    },
    {
      "path": "pkg/coordinator",
      "mode": "040000",
      "type": "tree",
      "sha": "1e4ffa656f8112935142734826533b83696dbe6a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1e4ffa656f8112935142734826533b83696dbe6a"
    },
    {
      "path": "pkg/coordinator/common",
      "mode": "040000",
      "type": "tree",
      "sha": "4db7524e7439cb604602d148eda5746773791bcc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4db7524e7439cb604602d148eda5746773791bcc"
    },
    {
      "path": "pkg/coordinator/common/httplog",
      "mode": "040000",
      "type": "tree",
      "sha": "1d22ff31c1968d95c8947c5133e832dfdfaaa51d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1d22ff31c1968d95c8947c5133e832dfdfaaa51d"
    },
    {
      "path": "pkg/coordinator/common/httplog/redact.go",
      "mode": "100644",
      "type": "blob",
      "sha": "79f0de97a7341de823a697704da9dee6c0cb91b9",
      "size": 2409,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/79f0de97a7341de823a697704da9dee6c0cb91b9"
    },
    {
      "path": "pkg/coordinator/common/httplog/redact_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2875d444773694bd8ab1a511a0139a8dfd361612",
      "size": 3748,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2875d444773694bd8ab1a511a0139a8dfd361612"
    },
    {
      "path": "pkg/coordinator/config",
      "mode": "040000",
      "type": "tree",
      "sha": "be25fac39ec321531812f4388f9985c989ef1d4d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/be25fac39ec321531812f4388f9985c989ef1d4d"
    },
    {
      "path": "pkg/coordinator/config/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1861abab7c99887dadec7f904bf1bc6963b099c5",
      "size": 3393,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1861abab7c99887dadec7f904bf1bc6963b099c5"
    },
    {
      "path": "pkg/coordinator/config/config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "47f74f6c27205e968b03e5963104169010333b20",
      "size": 5153,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/47f74f6c27205e968b03e5963104169010333b20"
    },
    {
      "path": "pkg/coordinator/connectors",
      "mode": "040000",
      "type": "tree",
      "sha": "db5b87a3fcb5ba9a8d1b802c5a3ab957726fca34",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/db5b87a3fcb5ba9a8d1b802c5a3ab957726fca34"
    },
    {
      "path": "pkg/coordinator/connectors/ec",
      "mode": "040000",
      "type": "tree",
      "sha": "720a018ec0e6196bc280e1bd094b50549ef0d24c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/720a018ec0e6196bc280e1bd094b50549ef0d24c"
    },
    {
      "path": "pkg/coordinator/connectors/ec/ec.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fa8789d7aefe44281eeb9c61bf7bac90d0a00eaf",
      "size": 3145,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fa8789d7aefe44281eeb9c61bf7bac90d0a00eaf"
    },
    {
      "path": "pkg/coordinator/connectors/ec/ec_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "287b82845af77bf85f9dec1d4978a73f3dcb0a86",
      "size": 11684,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/287b82845af77bf85f9dec1d4978a73f3dcb0a86"
    },
    {
      "path": "pkg/coordinator/connectors/ec/names.go",
      "mode": "100644",
      "type": "blob",
      "sha": "310e0b695f5783391caef2c8bdb830cbd2800f4e",
      "size": 652,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/310e0b695f5783391caef2c8bdb830cbd2800f4e"
    },
    {
      "path": "pkg/coordinator/connectors/ec/nixl.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d0f907cbc9f5399d4215dce84bbe24d235757a08",
      "size": 3598,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d0f907cbc9f5399d4215dce84bbe24d235757a08"
    },
    {
      "path": "pkg/coordinator/connectors/ec/shared_storage.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1aaf1629c2a88142c07d0244c1dfa9a83ba1cc9b",
      "size": 1248,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1aaf1629c2a88142c07d0244c1dfa9a83ba1cc9b"
    },
    {
      "path": "pkg/coordinator/connectors/kv",
      "mode": "040000",
      "type": "tree",
      "sha": "15d368be5b17ec3162111efd9fbb513578e2b9be",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/15d368be5b17ec3162111efd9fbb513578e2b9be"
    },
    {
      "path": "pkg/coordinator/connectors/kv/kv.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fd591c8c58e5186e853ab89479ddbb6b06091e07",
      "size": 2438,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fd591c8c58e5186e853ab89479ddbb6b06091e07"
    },
    {
      "path": "pkg/coordinator/connectors/kv/kv_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b2b482bdfa77f24c36fc3d51cae7ed695be1bd43",
      "size": 4486,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b2b482bdfa77f24c36fc3d51cae7ed695be1bd43"
    },
    {
      "path": "pkg/coordinator/connectors/kv/names.go",
      "mode": "100644",
      "type": "blob",
      "sha": "55f53dddf699be5908d99b5b00ad7efc2d6c17c5",
      "size": 681,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/55f53dddf699be5908d99b5b00ad7efc2d6c17c5"
    },
    {
      "path": "pkg/coordinator/connectors/kv/nixl.go",
      "mode": "100644",
      "type": "blob",
      "sha": "342b54c0f9ab5d82caa880904a0016a8c760714b",
      "size": 2140,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/342b54c0f9ab5d82caa880904a0016a8c760714b"
    },
    {
      "path": "pkg/coordinator/connectors/kv/sglang.go",
      "mode": "100644",
      "type": "blob",
      "sha": "21e7009d2d0b9964c207ad25c74da88a7a1f7e49",
      "size": 3904,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/21e7009d2d0b9964c207ad25c74da88a7a1f7e49"
    },
    {
      "path": "pkg/coordinator/connectors/kv/sglang_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bf96b985532d025f3745e2395722bc86aa0d6ff8",
      "size": 1571,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf96b985532d025f3745e2395722bc86aa0d6ff8"
    },
    {
      "path": "pkg/coordinator/connectors/kv/shared_storage.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a09c2718cb96ba37dac27003a58de6e081826ddc",
      "size": 1792,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a09c2718cb96ba37dac27003a58de6e081826ddc"
    },
    {
      "path": "pkg/coordinator/gateway",
      "mode": "040000",
      "type": "tree",
      "sha": "85ca2e30cd5fa8bb7be942501b8a46e69834fe66",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/85ca2e30cd5fa8bb7be942501b8a46e69834fe66"
    },
    {
      "path": "pkg/coordinator/gateway/client.go",
      "mode": "100644",
      "type": "blob",
      "sha": "01c7253c73fbe99c660563f9f0e698006d05ba50",
      "size": 6823,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/01c7253c73fbe99c660563f9f0e698006d05ba50"
    },
    {
      "path": "pkg/coordinator/gateway/client_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1bf557d9299306d2bdb5ec0df442c6ff8bd8a852",
      "size": 9075,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1bf557d9299306d2bdb5ec0df442c6ff8bd8a852"
    },
    {
      "path": "pkg/coordinator/gateway/paths.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d5d1a25a7a6e247e09ae355e1d849d56b273426f",
      "size": 2125,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d5d1a25a7a6e247e09ae355e1d849d56b273426f"
    },
    {
      "path": "pkg/coordinator/gateway/paths_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b31160aeeaf994531051d64d617b6f325f94291d",
      "size": 1007,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b31160aeeaf994531051d64d617b6f325f94291d"
    },
    {
      "path": "pkg/coordinator/metrics",
      "mode": "040000",
      "type": "tree",
      "sha": "91fc77e1da924dcb9f22bc4f7238036ae3a3e434",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/91fc77e1da924dcb9f22bc4f7238036ae3a3e434"
    },
    {
      "path": "pkg/coordinator/metrics/cardinality.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0528cd5eb748a72cb474a6ed38d92d915d6a1ba2",
      "size": 1551,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0528cd5eb748a72cb474a6ed38d92d915d6a1ba2"
    },
    {
      "path": "pkg/coordinator/metrics/cardinality_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9603e48ecbbd74a2b9a690711b70d5bf9fa1f66e",
      "size": 884,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9603e48ecbbd74a2b9a690711b70d5bf9fa1f66e"
    },
    {
      "path": "pkg/coordinator/metrics/classify.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f1c8dad107a05ea766c6c4781b099fc92d1b62d8",
      "size": 2247,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f1c8dad107a05ea766c6c4781b099fc92d1b62d8"
    },
    {
      "path": "pkg/coordinator/metrics/classify_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3e8f3fb05466ff68e762e8e1db88ffeaf14c34cb",
      "size": 3345,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3e8f3fb05466ff68e762e8e1db88ffeaf14c34cb"
    },
    {
      "path": "pkg/coordinator/metrics/llm_d_coordinator_metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1cfe8a9e1723611174b53da0a9ef732b2ecd1f13",
      "size": 6583,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1cfe8a9e1723611174b53da0a9ef732b2ecd1f13"
    },
    {
      "path": "pkg/coordinator/metrics/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dcb1a184ef9ba4cf93e2f9a98bdaa778884f8d9d",
      "size": 5740,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dcb1a184ef9ba4cf93e2f9a98bdaa778884f8d9d"
    },
    {
      "path": "pkg/coordinator/metrics/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ee32d4eaf074d04c1644b160401956e884c9f804",
      "size": 7917,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ee32d4eaf074d04c1644b160401956e884c9f804"
    },
    {
      "path": "pkg/coordinator/metrics/record.go",
      "mode": "100644",
      "type": "blob",
      "sha": "01cd9f5b1178474c1a53bcf08e1c6a23ac53a0b9",
      "size": 3716,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/01cd9f5b1178474c1a53bcf08e1c6a23ac53a0b9"
    },
    {
      "path": "pkg/coordinator/metrics/upstream_call.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5a8407bd4f63843e45b13522ce19ea17b26995eb",
      "size": 1880,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5a8407bd4f63843e45b13522ce19ea17b26995eb"
    },
    {
      "path": "pkg/coordinator/metrics/upstream_call_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "caea3fe8c79dfb7351960e7ddc6577ee2fcb32dc",
      "size": 2019,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/caea3fe8c79dfb7351960e7ddc6577ee2fcb32dc"
    },
    {
      "path": "pkg/coordinator/pipeline",
      "mode": "040000",
      "type": "tree",
      "sha": "2c0a02ff60337432c4acbf4b955a98d83c4c6147",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2c0a02ff60337432c4acbf4b955a98d83c4c6147"
    },
    {
      "path": "pkg/coordinator/pipeline/builder",
      "mode": "040000",
      "type": "tree",
      "sha": "c70431ee30b63fde4ec54747867945afb05f046a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c70431ee30b63fde4ec54747867945afb05f046a"
    },
    {
      "path": "pkg/coordinator/pipeline/builder/builder.go",
      "mode": "100644",
      "type": "blob",
      "sha": "19441cdc9fcbd3bd8e8cb12b9d115e80be47b7e2",
      "size": 3267,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/19441cdc9fcbd3bd8e8cb12b9d115e80be47b7e2"
    },
    {
      "path": "pkg/coordinator/pipeline/builder/builder_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c9d1dc15f338db9f2c4e1a94cf1110b229b57ca0",
      "size": 2191,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c9d1dc15f338db9f2c4e1a94cf1110b229b57ca0"
    },
    {
      "path": "pkg/coordinator/pipeline/context.go",
      "mode": "100644",
      "type": "blob",
      "sha": "808254375b84d0ac59f4b53693544cdde089b872",
      "size": 3897,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/808254375b84d0ac59f4b53693544cdde089b872"
    },
    {
      "path": "pkg/coordinator/pipeline/context_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c551771d7ab9b189140d3dff8b224ab5ebefa5b0",
      "size": 3354,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c551771d7ab9b189140d3dff8b224ab5ebefa5b0"
    },
    {
      "path": "pkg/coordinator/pipeline/helpers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5bf7bfe50e9107711226e7143f17cd7b907929b2",
      "size": 772,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5bf7bfe50e9107711226e7143f17cd7b907929b2"
    },
    {
      "path": "pkg/coordinator/pipeline/integration_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8af7c63664f8106ec4f71256f9571813fdef66ae",
      "size": 10896,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8af7c63664f8106ec4f71256f9571813fdef66ae"
    },
    {
      "path": "pkg/coordinator/pipeline/pipeline.go",
      "mode": "100644",
      "type": "blob",
      "sha": "caea7319eed1f1ace6f81f49b468ce88a20d126b",
      "size": 7687,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/caea7319eed1f1ace6f81f49b468ce88a20d126b"
    },
    {
      "path": "pkg/coordinator/pipeline/pipeline_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "07cce96a014b6ec5fc149aa47ef651cfd5cee3ea",
      "size": 15928,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/07cce96a014b6ec5fc149aa47ef651cfd5cee3ea"
    },
    {
      "path": "pkg/coordinator/pipeline/registry.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5f46ac1ed54d6db4acd3d2d7d031dbd72a351e41",
      "size": 1173,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5f46ac1ed54d6db4acd3d2d7d031dbd72a351e41"
    },
    {
      "path": "pkg/coordinator/pipeline/step.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0969ffbf47b10a06201ee38de6c10430db66b484",
      "size": 1130,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0969ffbf47b10a06201ee38de6c10430db66b484"
    },
    {
      "path": "pkg/coordinator/pipeline/text_only_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4cec383e7b93e0d1e886e60a1c1d69b381ee62b5",
      "size": 4397,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4cec383e7b93e0d1e886e60a1c1d69b381ee62b5"
    },
    {
      "path": "pkg/coordinator/server",
      "mode": "040000",
      "type": "tree",
      "sha": "8d2390238044d018001de934f58e6ece59f73cdf",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8d2390238044d018001de934f58e6ece59f73cdf"
    },
    {
      "path": "pkg/coordinator/server/handlers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "89a17e87d36efca30c7b21c95370287fb64b785c",
      "size": 7709,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/89a17e87d36efca30c7b21c95370287fb64b785c"
    },
    {
      "path": "pkg/coordinator/server/handlers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ce9b5252193943be8c3d866c53b77fa801fea442",
      "size": 28598,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ce9b5252193943be8c3d866c53b77fa801fea442"
    },
    {
      "path": "pkg/coordinator/server/passthrough.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c5504f3de2b8c50a7dd31d30e9c5e58db20184ac",
      "size": 6746,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c5504f3de2b8c50a7dd31d30e9c5e58db20184ac"
    },
    {
      "path": "pkg/coordinator/server/passthrough_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f56d1b9b2cb2e5a9a65394038834e4cf6bb0d420",
      "size": 13454,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f56d1b9b2cb2e5a9a65394038834e4cf6bb0d420"
    },
    {
      "path": "pkg/coordinator/server/server.go",
      "mode": "100644",
      "type": "blob",
      "sha": "eba76f7695b88d328c0cb6bf5a843e39b1b4bb6a",
      "size": 5142,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eba76f7695b88d328c0cb6bf5a843e39b1b4bb6a"
    },
    {
      "path": "pkg/coordinator/server/server_routes_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a0e1fd5cd25e5cbd4a4d9b160b296cd1c3c64ae9",
      "size": 2221,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a0e1fd5cd25e5cbd4a4d9b160b296cd1c3c64ae9"
    },
    {
      "path": "pkg/coordinator/steps",
      "mode": "040000",
      "type": "tree",
      "sha": "25d133db4ace3f9c7da9800662755a02dcadae1e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/25d133db4ace3f9c7da9800662755a02dcadae1e"
    },
    {
      "path": "pkg/coordinator/steps/asyncbroker",
      "mode": "040000",
      "type": "tree",
      "sha": "e5f67689e80ecbc1667a37b830b3e529da96b378",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/e5f67689e80ecbc1667a37b830b3e529da96b378"
    },
    {
      "path": "pkg/coordinator/steps/asyncbroker/broker.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b5c3fd40ee3bfe3365eada4b9df145285727b3d2",
      "size": 17123,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b5c3fd40ee3bfe3365eada4b9df145285727b3d2"
    },
    {
      "path": "pkg/coordinator/steps/asyncbroker/broker_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "22909ef1dad93151ca2bb9f1adc6994c8f405b41",
      "size": 26288,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/22909ef1dad93151ca2bb9f1adc6994c8f405b41"
    },
    {
      "path": "pkg/coordinator/steps/asyncbroker/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a828d46e66b02c083eb18731ffdc3363a6048587",
      "size": 12312,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a828d46e66b02c083eb18731ffdc3363a6048587"
    },
    {
      "path": "pkg/coordinator/steps/asyncbroker/quota.go",
      "mode": "100644",
      "type": "blob",
      "sha": "27503f4e6417c00027bc37407d8a337f2a6e6074",
      "size": 3502,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/27503f4e6417c00027bc37407d8a337f2a6e6074"
    },
    {
      "path": "pkg/coordinator/steps/asyncbroker/routes.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7a63c20221b986ff551f3ef38b8e2daee540717c",
      "size": 10879,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a63c20221b986ff551f3ef38b8e2daee540717c"
    },
    {
      "path": "pkg/coordinator/steps/asyncbroker/wait.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d7013753c071b6cee877f56b830fee73e31d524e",
      "size": 4644,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d7013753c071b6cee877f56b830fee73e31d524e"
    },
    {
      "path": "pkg/coordinator/steps/asyncbroker/wait_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2a69d9d8d1b3999f5d53a0a20d7d053fc63ef078",
      "size": 3688,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2a69d9d8d1b3999f5d53a0a20d7d053fc63ef078"
    },
    {
      "path": "pkg/coordinator/steps/conditional_decode.go",
      "mode": "100644",
      "type": "blob",
      "sha": "38775b955528c4bd81f3785d8c8a2d9b756d9ac5",
      "size": 4094,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/38775b955528c4bd81f3785d8c8a2d9b756d9ac5"
    },
    {
      "path": "pkg/coordinator/steps/conditional_decode_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "afd42ee955112fa7bb8ed48c13c8ac832a7d9e54",
      "size": 11893,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/afd42ee955112fa7bb8ed48c13c8ac832a7d9e54"
    },
    {
      "path": "pkg/coordinator/steps/connector_wiring_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0f3c448c970718eaa42b6a6453954e68681cae04",
      "size": 7421,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0f3c448c970718eaa42b6a6453954e68681cae04"
    },
    {
      "path": "pkg/coordinator/steps/decode.go",
      "mode": "100644",
      "type": "blob",
      "sha": "29c1875f5aedb813ebdfce77fcedba934de5563c",
      "size": 5342,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/29c1875f5aedb813ebdfce77fcedba934de5563c"
    },
    {
      "path": "pkg/coordinator/steps/decode_proxy.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8d2c0dfe9ec9512db6a90901bb7f8b4586bfdd4a",
      "size": 8307,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8d2c0dfe9ec9512db6a90901bb7f8b4586bfdd4a"
    },
    {
      "path": "pkg/coordinator/steps/decode_proxy_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a614ea0aa53c941b3fb6e9d21877bc3d667c85c7",
      "size": 8602,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a614ea0aa53c941b3fb6e9d21877bc3d667c85c7"
    },
    {
      "path": "pkg/coordinator/steps/decode_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "42b0ee64a8802dd639ffc6f3a0ed12219f1519f0",
      "size": 14955,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/42b0ee64a8802dd639ffc6f3a0ed12219f1519f0"
    },
    {
      "path": "pkg/coordinator/steps/encode.go",
      "mode": "100644",
      "type": "blob",
      "sha": "288d22a78a880dda37279600f15109cf0b6e079a",
      "size": 8774,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/288d22a78a880dda37279600f15109cf0b6e079a"
    },
    {
      "path": "pkg/coordinator/steps/encode_prefill_flow_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3edb974ad4d27882eba51914c9f60ba18618b435",
      "size": 9364,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3edb974ad4d27882eba51914c9f60ba18618b435"
    },
    {
      "path": "pkg/coordinator/steps/encode_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8378aafe30b975e43eac4860672cb598f545d8a6",
      "size": 21766,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8378aafe30b975e43eac4860672cb598f545d8a6"
    },
    {
      "path": "pkg/coordinator/steps/gateway_paths_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "51dbe60271f896da477f57a23c7fd6d6e4c8abc6",
      "size": 8087,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/51dbe60271f896da477f57a23c7fd6d6e4c8abc6"
    },
    {
      "path": "pkg/coordinator/steps/params.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d30af499df46f909c93d2a994a2824b4717acc3c",
      "size": 3879,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d30af499df46f909c93d2a994a2824b4717acc3c"
    },
    {
      "path": "pkg/coordinator/steps/params_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b35c5e953745cf51b8bc4fb1676c14e0feb6ce19",
      "size": 6515,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b35c5e953745cf51b8bc4fb1676c14e0feb6ce19"
    },
    {
      "path": "pkg/coordinator/steps/prefill.go",
      "mode": "100644",
      "type": "blob",
      "sha": "74fbab4124e0506a46e6d1a67e6dc9810305290a",
      "size": 6542,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/74fbab4124e0506a46e6d1a67e6dc9810305290a"
    },
    {
      "path": "pkg/coordinator/steps/prefill_decode_flow_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2301e0272ee6c906092365e6eafa33d98777fcac",
      "size": 5876,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2301e0272ee6c906092365e6eafa33d98777fcac"
    },
    {
      "path": "pkg/coordinator/steps/prefill_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9ab2e46d96037278b1627708760a4e58c62b294b",
      "size": 25578,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9ab2e46d96037278b1627708760a4e58c62b294b"
    },
    {
      "path": "pkg/coordinator/steps/render.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2f5ef6623a3f1a3be0e66312493e46345f86d02c",
      "size": 13271,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2f5ef6623a3f1a3be0e66312493e46345f86d02c"
    },
    {
      "path": "pkg/coordinator/steps/render_encode_flow_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "78e3a4f8a2790eaff3fe12553edced47f9415d01",
      "size": 4851,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/78e3a4f8a2790eaff3fe12553edced47f9415d01"
    },
    {
      "path": "pkg/coordinator/steps/render_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "306578975ea214d6689a9cbb03ef8dc325ffe285",
      "size": 22697,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/306578975ea214d6689a9cbb03ef8dc325ffe285"
    },
    {
      "path": "pkg/coordinator/steps/replace_media_urls.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d9fafa6d5dc8f2bdfda6de5c27631fa3d493e6fa",
      "size": 14276,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d9fafa6d5dc8f2bdfda6de5c27631fa3d493e6fa"
    },
    {
      "path": "pkg/coordinator/steps/replace_media_urls_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9316e9033d720f6faad8d739a6c2fa802944b111",
      "size": 35763,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9316e9033d720f6faad8d739a6c2fa802944b111"
    },
    {
      "path": "pkg/coordinator/steps/utils.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5b31064789058a9737ff6000c53d6cab335dfa9c",
      "size": 16307,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5b31064789058a9737ff6000c53d6cab335dfa9c"
    },
    {
      "path": "pkg/coordinator/steps/utils_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6195ff478e3a6c5abff2b1a3999865abb8b92ab0",
      "size": 21012,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6195ff478e3a6c5abff2b1a3999865abb8b92ab0"
    },
    {
      "path": "pkg/epp",
      "mode": "040000",
      "type": "tree",
      "sha": "bcd294f97441e5ae750e53ce03972647128ce7fc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/bcd294f97441e5ae750e53ce03972647128ce7fc"
    },
    {
      "path": "pkg/epp/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "08740c78b7d65d7901048588706c3d943c147e68",
      "size": 2238,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/08740c78b7d65d7901048588706c3d943c147e68"
    },
    {
      "path": "pkg/epp/config",
      "mode": "040000",
      "type": "tree",
      "sha": "1d9ac9669fbbb2d1344580e632be27d699324a7f",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1d9ac9669fbbb2d1344580e632be27d699324a7f"
    },
    {
      "path": "pkg/epp/config/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1db6c0f7dfb2577f4c7b22c2ccb1e7771088efcc",
      "size": 1591,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1db6c0f7dfb2577f4c7b22c2ccb1e7771088efcc"
    },
    {
      "path": "pkg/epp/config/loader",
      "mode": "040000",
      "type": "tree",
      "sha": "92a554452dffaf451b70a6e80f48947acc0e8d96",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/92a554452dffaf451b70a6e80f48947acc0e8d96"
    },
    {
      "path": "pkg/epp/config/loader/configloader.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6faad35e9e5c61257fbf657e0512f7a4351d7603",
      "size": 19909,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6faad35e9e5c61257fbf657e0512f7a4351d7603"
    },
    {
      "path": "pkg/epp/config/loader/configloader_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "39babff5c65ae12004840280facee2ec3830599c",
      "size": 60395,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/39babff5c65ae12004840280facee2ec3830599c"
    },
    {
      "path": "pkg/epp/config/loader/defaults.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0a69ec1860442f25d3466d880c059e71b0c4bff7",
      "size": 13509,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0a69ec1860442f25d3466d880c059e71b0c4bff7"
    },
    {
      "path": "pkg/epp/config/loader/defaults_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4b6211930466dc35022a9bdef086ac2714ac5566",
      "size": 9886,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4b6211930466dc35022a9bdef086ac2714ac5566"
    },
    {
      "path": "pkg/epp/config/loader/flowcontrol.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a0100671eb7662a23c0a95c70e0915d6ee4b6017",
      "size": 6881,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a0100671eb7662a23c0a95c70e0915d6ee4b6017"
    },
    {
      "path": "pkg/epp/config/loader/flowcontrol_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a2b3ca5feec74581dd09ae7d89e75dd1c004e120",
      "size": 24695,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a2b3ca5feec74581dd09ae7d89e75dd1c004e120"
    },
    {
      "path": "pkg/epp/config/loader/testdata_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "14812cef926f87a22435c72c6631a7b92d806861",
      "size": 23837,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/14812cef926f87a22435c72c6631a7b92d806861"
    },
    {
      "path": "pkg/epp/config/loader/validation.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ce4d09c780160f966ac417cccb7314003916caeb",
      "size": 4773,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ce4d09c780160f966ac417cccb7314003916caeb"
    },
    {
      "path": "pkg/epp/controller",
      "mode": "040000",
      "type": "tree",
      "sha": "181a756c9b185ee688804dc7fd5bfeb686c6dc29",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/181a756c9b185ee688804dc7fd5bfeb686c6dc29"
    },
    {
      "path": "pkg/epp/controller/inferencemodelrewrite_reconciler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c644b0f02b3f38ba140309da6fcc022159481899",
      "size": 4190,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c644b0f02b3f38ba140309da6fcc022159481899"
    },
    {
      "path": "pkg/epp/controller/inferencemodelrewrite_reconciler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0ca458eda4449fd28732ab9af8346ccf76aa97c8",
      "size": 8643,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0ca458eda4449fd28732ab9af8346ccf76aa97c8"
    },
    {
      "path": "pkg/epp/controller/inferenceobjective_reconciler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "27c3ae58a52532454686d523500ecb92585c0de5",
      "size": 4529,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/27c3ae58a52532454686d523500ecb92585c0de5"
    },
    {
      "path": "pkg/epp/controller/inferenceobjective_reconciler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "32e4c07cb32ea5f5a7d9b465c5e2fe3bae6b67da",
      "size": 8093,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/32e4c07cb32ea5f5a7d9b465c5e2fe3bae6b67da"
    },
    {
      "path": "pkg/epp/controller/inferencepool_reconciler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9d8f79deaf528a3d6575c94ed80fb369ffa213fb",
      "size": 2877,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9d8f79deaf528a3d6575c94ed80fb369ffa213fb"
    },
    {
      "path": "pkg/epp/controller/inferencepool_reconciler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b38df4fd55c543c3d1c9d313fd13a5adcffd9f0e",
      "size": 8053,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b38df4fd55c543c3d1c9d313fd13a5adcffd9f0e"
    },
    {
      "path": "pkg/epp/controller/pod_reconciler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ea2065fbfbf301308d445edd8b0dbce39686c741",
      "size": 3558,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ea2065fbfbf301308d445edd8b0dbce39686c741"
    },
    {
      "path": "pkg/epp/controller/pod_reconciler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2f559756bc3e8fc4e3030cdb49eaf9682e26c446",
      "size": 9517,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2f559756bc3e8fc4e3030cdb49eaf9682e26c446"
    },
    {
      "path": "pkg/epp/controller/setup_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "03e3ae548d1b7fed26250b9602b61cd284c8c067",
      "size": 4592,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/03e3ae548d1b7fed26250b9602b61cd284c8c067"
    },
    {
      "path": "pkg/epp/datalayer",
      "mode": "040000",
      "type": "tree",
      "sha": "6d34f2b774a782ea510ba6a372d779d19ecfc88e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6d34f2b774a782ea510ba6a372d779d19ecfc88e"
    },
    {
      "path": "pkg/epp/datalayer/collector.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2c6201374278834ac1a7ac633a084ddf00337307",
      "size": 3810,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2c6201374278834ac1a7ac633a084ddf00337307"
    },
    {
      "path": "pkg/epp/datalayer/collector_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b4bdd5de8be7bd4d56b4e229747d9c03473129c6",
      "size": 10277,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b4bdd5de8be7bd4d56b4e229747d9c03473129c6"
    },
    {
      "path": "pkg/epp/datalayer/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fe625e55db99159e19fa961ba20583e829014a0e",
      "size": 2879,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fe625e55db99159e19fa961ba20583e829014a0e"
    },
    {
      "path": "pkg/epp/datalayer/cross_replica_publisher.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9902c3cbfff080a4c8e9b8bc968eec77f70db15d",
      "size": 6048,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9902c3cbfff080a4c8e9b8bc968eec77f70db15d"
    },
    {
      "path": "pkg/epp/datalayer/cross_replica_publisher_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "deee7b9db7965449676b48dc40f48c367970685b",
      "size": 16090,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/deee7b9db7965449676b48dc40f48c367970685b"
    },
    {
      "path": "pkg/epp/datalayer/data_graph.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5d2d295ffeafd7eba67803f387e809f6db8dd53d",
      "size": 9287,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5d2d295ffeafd7eba67803f387e809f6db8dd53d"
    },
    {
      "path": "pkg/epp/datalayer/data_graph_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2ed7566488c784025edb306eb4bae467437fc302",
      "size": 27427,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2ed7566488c784025edb306eb4bae467437fc302"
    },
    {
      "path": "pkg/epp/datalayer/endpoint_pool.go",
      "mode": "100644",
      "type": "blob",
      "sha": "94afa3f01c39136e0f1cc89ec30b03655cffc2ba",
      "size": 1119,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/94afa3f01c39136e0f1cc89ec30b03655cffc2ba"
    },
    {
      "path": "pkg/epp/datalayer/endpoint_scope.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f7a0025a296bdf3c2579425934b03c19556da097",
      "size": 12785,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f7a0025a296bdf3c2579425934b03c19556da097"
    },
    {
      "path": "pkg/epp/datalayer/endpoint_scope_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a4cd0f8b362a8c64068b747e93cca396ea8afe8d",
      "size": 2641,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a4cd0f8b362a8c64068b747e93cca396ea8afe8d"
    },
    {
      "path": "pkg/epp/datalayer/endpoint_scope_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "def9214c47088aef0859160311eb75fe79f6036a",
      "size": 11397,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/def9214c47088aef0859160311eb75fe79f6036a"
    },
    {
      "path": "pkg/epp/datalayer/factory.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6475594b34a4b8a7204cd9486b52feda5979a95c",
      "size": 1352,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6475594b34a4b8a7204cd9486b52feda5979a95c"
    },
    {
      "path": "pkg/epp/datalayer/factory_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3ed1c39bbbf667be7f7f5358f8a22d7db3890d94",
      "size": 1761,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3ed1c39bbbf667be7f7f5358f8a22d7db3890d94"
    },
    {
      "path": "pkg/epp/datalayer/fake_factory.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dde55d538d42359caad46a699b741dd902b1a53c",
      "size": 1738,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dde55d538d42359caad46a699b741dd902b1a53c"
    },
    {
      "path": "pkg/epp/datalayer/interval_dispatcher.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1cd23321d66f01cd962ec77c5cddf2487fee8e6b",
      "size": 2262,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1cd23321d66f01cd962ec77c5cddf2487fee8e6b"
    },
    {
      "path": "pkg/epp/datalayer/interval_dispatcher_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "50f23d923053e0a8d246a80b329401457a265bf5",
      "size": 3756,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/50f23d923053e0a8d246a80b329401457a265bf5"
    },
    {
      "path": "pkg/epp/datalayer/k8s_bind.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3ebfb87bf766537b2bf1cfe1cf38aaa5352131e2",
      "size": 7278,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3ebfb87bf766537b2bf1cfe1cf38aaa5352131e2"
    },
    {
      "path": "pkg/epp/datalayer/k8s_bind_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b02c105c28bc39b8f4e8d1daca27c5073b863127",
      "size": 4739,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b02c105c28bc39b8f4e8d1daca27c5073b863127"
    },
    {
      "path": "pkg/epp/datalayer/logger",
      "mode": "040000",
      "type": "tree",
      "sha": "052cb8f1b6af4f605a64b1cc6405f1a2f71e48f9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/052cb8f1b6af4f605a64b1cc6405f1a2f71e48f9"
    },
    {
      "path": "pkg/epp/datalayer/logger/logger.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fa5f8c348b922c2e68808eb895240007ee8e6a66",
      "size": 6146,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fa5f8c348b922c2e68808eb895240007ee8e6a66"
    },
    {
      "path": "pkg/epp/datalayer/logger/logger_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "63b3d83859929664b9a55bc71e427c45f9c525a1",
      "size": 7671,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/63b3d83859929664b9a55bc71e427c45f9c525a1"
    },
    {
      "path": "pkg/epp/datalayer/manager.go",
      "mode": "100644",
      "type": "blob",
      "sha": "860e3d425d7bbfe0e9dfb0551cc6e128161b1c10",
      "size": 7960,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/860e3d425d7bbfe0e9dfb0551cc6e128161b1c10"
    },
    {
      "path": "pkg/epp/datalayer/manager_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a1caac02a9860cb9d60aaa304096abbb6ce1d3c0",
      "size": 1449,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a1caac02a9860cb9d60aaa304096abbb6ce1d3c0"
    },
    {
      "path": "pkg/epp/datalayer/mocks",
      "mode": "040000",
      "type": "tree",
      "sha": "2dd8cb117b8f47c0a4e6fae5c18a75bb3b2e2ef9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2dd8cb117b8f47c0a4e6fae5c18a75bb3b2e2ef9"
    },
    {
      "path": "pkg/epp/datalayer/mocks/ticker.go",
      "mode": "100644",
      "type": "blob",
      "sha": "751704dfc50dd98ef734e9fec59dd9dd333686fb",
      "size": 976,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/751704dfc50dd98ef734e9fec59dd9dd333686fb"
    },
    {
      "path": "pkg/epp/datalayer/runtime.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a68a87e02c76a619e2198231ade436aff70f45a6",
      "size": 21863,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a68a87e02c76a619e2198231ade436aff70f45a6"
    },
    {
      "path": "pkg/epp/datalayer/runtime_endpoint_dispatch_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "aed2d0592ec79a4423a44617c67d5df2c303bf73",
      "size": 4560,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/aed2d0592ec79a4423a44617c67d5df2c303bf73"
    },
    {
      "path": "pkg/epp/datalayer/runtime_registrar_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "67cd216383fcf48c9db4026e25a8313fbf6c406e",
      "size": 12008,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/67cd216383fcf48c9db4026e25a8313fbf6c406e"
    },
    {
      "path": "pkg/epp/datalayer/runtime_validate_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ba61b56107d1cfe63935b3bd64d1de7d0e423bca",
      "size": 7918,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ba61b56107d1cfe63935b3bd64d1de7d0e423bca"
    },
    {
      "path": "pkg/epp/datalayer/test_runtime.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7d3370fa1b8804f149b57abc00da51dc06fd8d59",
      "size": 1810,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7d3370fa1b8804f149b57abc00da51dc06fd8d59"
    },
    {
      "path": "pkg/epp/datastore",
      "mode": "040000",
      "type": "tree",
      "sha": "411bf0ef4da8c8c7614af9b4d3599905e1892e4d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/411bf0ef4da8c8c7614af9b4d3599905e1892e4d"
    },
    {
      "path": "pkg/epp/datastore/datastore.go",
      "mode": "100644",
      "type": "blob",
      "sha": "59cc50766c36d5bd600556d43b1088ceed6e9668",
      "size": 18840,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/59cc50766c36d5bd600556d43b1088ceed6e9668"
    },
    {
      "path": "pkg/epp/datastore/datastore_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fe5c4e82fcf4a98dd9a096d1f3db9d61dbe2d412",
      "size": 51105,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fe5c4e82fcf4a98dd9a096d1f3db9d61dbe2d412"
    },
    {
      "path": "pkg/epp/datastore/modelrewritestore.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e4caf196a7bb2bd9eb87bc581adda7531b9502ab",
      "size": 6710,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e4caf196a7bb2bd9eb87bc581adda7531b9502ab"
    },
    {
      "path": "pkg/epp/datastore/modelrewritestore_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ccb5f75caec4b59ee450ed20a2c95c329c96c470",
      "size": 8697,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ccb5f75caec4b59ee450ed20a2c95c329c96c470"
    },
    {
      "path": "pkg/epp/flowcontrol",
      "mode": "040000",
      "type": "tree",
      "sha": "c0c43735038f00221249739192a6e558f62d09d7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c0c43735038f00221249739192a6e558f62d09d7"
    },
    {
      "path": "pkg/epp/flowcontrol/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "bcb61c30d8ccc2fce5c5466508f6c8ad2b96ab92",
      "size": 9264,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bcb61c30d8ccc2fce5c5466508f6c8ad2b96ab92"
    },
    {
      "path": "pkg/epp/flowcontrol/benchmark",
      "mode": "040000",
      "type": "tree",
      "sha": "594b9b37ac645d589f7003980cd0e2f35955e94d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/594b9b37ac645d589f7003980cd0e2f35955e94d"
    },
    {
      "path": "pkg/epp/flowcontrol/benchmark/benchmark_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a9bf4cac1e7b3b2937b94c3589aa829b95f70777",
      "size": 12731,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a9bf4cac1e7b3b2937b94c3589aa829b95f70777"
    },
    {
      "path": "pkg/epp/flowcontrol/benchmark/detector_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d379ece9e2d07e981cab8eeeab2b3da4c7c8f253",
      "size": 4506,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d379ece9e2d07e981cab8eeeab2b3da4c7c8f253"
    },
    {
      "path": "pkg/epp/flowcontrol/benchmark/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "04c723854e2a3a9f2edee7f50135e4f441c3678b",
      "size": 3520,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/04c723854e2a3a9f2edee7f50135e4f441c3678b"
    },
    {
      "path": "pkg/epp/flowcontrol/benchmark/evictiondynamics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bf536e5bba0b4710f1a8de946bcd7e6439fa0976",
      "size": 12905,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf536e5bba0b4710f1a8de946bcd7e6439fa0976"
    },
    {
      "path": "pkg/epp/flowcontrol/benchmark/evictionsim_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e6620c9ec8e8c9bcacd4d9c83361443bbc05bfb3",
      "size": 14802,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e6620c9ec8e8c9bcacd4d9c83361443bbc05bfb3"
    },
    {
      "path": "pkg/epp/flowcontrol/benchmark/helpers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d10a01ae20de9dea131cf23972aed3dced55fee6",
      "size": 16751,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d10a01ae20de9dea131cf23972aed3dced55fee6"
    },
    {
      "path": "pkg/epp/flowcontrol/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3e7d5fa7ad43b365c07d4b6d242b2542b89f4e7a",
      "size": 1954,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3e7d5fa7ad43b365c07d4b6d242b2542b89f4e7a"
    },
    {
      "path": "pkg/epp/flowcontrol/config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c0e0589e418d6a72471f1810847712d44d9f3a66",
      "size": 1989,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c0e0589e418d6a72471f1810847712d44d9f3a66"
    },
    {
      "path": "pkg/epp/flowcontrol/contracts",
      "mode": "040000",
      "type": "tree",
      "sha": "a603c128e772e2ecfc85e3787bfa4c4e219da886",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a603c128e772e2ecfc85e3787bfa4c4e219da886"
    },
    {
      "path": "pkg/epp/flowcontrol/contracts/dependencies.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b102d0b6c49d2b12e96b98f159c2458832da44ef",
      "size": 1324,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b102d0b6c49d2b12e96b98f159c2458832da44ef"
    },
    {
      "path": "pkg/epp/flowcontrol/contracts/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "554f54edc09fa089f00f51aa6ebc6c4bc4b3fa36",
      "size": 1231,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/554f54edc09fa089f00f51aa6ebc6c4bc4b3fa36"
    },
    {
      "path": "pkg/epp/flowcontrol/contracts/errors.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ee5dc0ee433acbd282922e48d32a9c5e74f200dd",
      "size": 1716,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ee5dc0ee433acbd282922e48d32a9c5e74f200dd"
    },
    {
      "path": "pkg/epp/flowcontrol/contracts/mocks",
      "mode": "040000",
      "type": "tree",
      "sha": "070c05fbf02c43772f7c98bc6d2ad6f6c0f0bf15",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/070c05fbf02c43772f7c98bc6d2ad6f6c0f0bf15"
    },
    {
      "path": "pkg/epp/flowcontrol/contracts/mocks/mocks.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e6f75e36721e0a4601ff1b8ea93ec86320162e92",
      "size": 13420,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e6f75e36721e0a4601ff1b8ea93ec86320162e92"
    },
    {
      "path": "pkg/epp/flowcontrol/contracts/queue.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a4454bac597bbba8ebecb62c978574c67311ccb5",
      "size": 2473,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a4454bac597bbba8ebecb62c978574c67311ccb5"
    },
    {
      "path": "pkg/epp/flowcontrol/contracts/registry.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c2470c12a6589e02adadb17129c7c30b7a367bbd",
      "size": 9523,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c2470c12a6589e02adadb17129c7c30b7a367bbd"
    },
    {
      "path": "pkg/epp/flowcontrol/controller",
      "mode": "040000",
      "type": "tree",
      "sha": "f9b6ca54110aa336872dd9b27374ab36f791b123",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f9b6ca54110aa336872dd9b27374ab36f791b123"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9df61642ef271de9d6fb625e2d22b8a5876db96c",
      "size": 11021,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9df61642ef271de9d6fb625e2d22b8a5876db96c"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "88b13b67c098f7bb67cc05e12052fc00b811339a",
      "size": 12020,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/88b13b67c098f7bb67cc05e12052fc00b811339a"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/controller.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8e490bf7da46f89c7e84e8adc0c3cae84c4cabc9",
      "size": 22403,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8e490bf7da46f89c7e84e8adc0c3cae84c4cabc9"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/controller_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6721e3dda7529cacc1ad7f464bd6d321f4755db9",
      "size": 48529,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6721e3dda7529cacc1ad7f464bd6d321f4755db9"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bc1cbfedfa7c8ebb16256d582314183ed6e6a087",
      "size": 3224,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bc1cbfedfa7c8ebb16256d582314183ed6e6a087"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/internal",
      "mode": "040000",
      "type": "tree",
      "sha": "6be9b2cae17c23a06962eb1bb1f4326b277c3ee2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6be9b2cae17c23a06962eb1bb1f4326b277c3ee2"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/internal/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7494b143d99d5ef71e1c361c314f53e35cbe25f5",
      "size": 1827,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7494b143d99d5ef71e1c361c314f53e35cbe25f5"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/internal/item.go",
      "mode": "100644",
      "type": "blob",
      "sha": "047c7c8ddb866bd9aabd1c53b43cd7e2bb6d7970",
      "size": 7732,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/047c7c8ddb866bd9aabd1c53b43cd7e2bb6d7970"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/internal/item_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0ff8c2063b24c6d94d1dbc8741b96469b8ec68aa",
      "size": 7403,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0ff8c2063b24c6d94d1dbc8741b96469b8ec68aa"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/internal/processor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6b77876a53e6e70182c7222c997da377fa23f971",
      "size": 36075,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6b77876a53e6e70182c7222c997da377fa23f971"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/internal/processor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "14f10af485f100a662ef4a4431301b208a4e410c",
      "size": 74194,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/14f10af485f100a662ef4a4431301b208a4e410c"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/internal/reclamation.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f2bb7ac0f2d0fba4fccb8bad1da66f3481337ee8",
      "size": 13531,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f2bb7ac0f2d0fba4fccb8bad1da66f3481337ee8"
    },
    {
      "path": "pkg/epp/flowcontrol/controller/internal/reclamation_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "cca8f94248e8e24b2e3966008babe9cf5e822f95",
      "size": 13470,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cca8f94248e8e24b2e3966008babe9cf5e822f95"
    },
    {
      "path": "pkg/epp/flowcontrol/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c3201bc90d072a1791bd12ebbb9f7247cc89e829",
      "size": 1515,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c3201bc90d072a1791bd12ebbb9f7247cc89e829"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction",
      "mode": "040000",
      "type": "tree",
      "sha": "9323ed1c44a1670d2007734f7202be37d9d5cedf",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/9323ed1c44a1670d2007734f7202be37d9d5cedf"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction/eviction_registry.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8523cf6bbd2ad38d7826281c359e1988adf52ba6",
      "size": 2957,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8523cf6bbd2ad38d7826281c359e1988adf52ba6"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction/eviction_registry_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f9e63eb40384410a57e4157181b21a94340a7f3f",
      "size": 2787,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f9e63eb40384410a57e4157181b21a94340a7f3f"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction/evictor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a88b83a5cc6abee313332b3d40476a7f44aa0ee8",
      "size": 3477,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a88b83a5cc6abee313332b3d40476a7f44aa0ee8"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction/evictor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "594a3ca5525083d28a63fd1db07138d9173e1bf9",
      "size": 3822,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/594a3ca5525083d28a63fd1db07138d9173e1bf9"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction/queue.go",
      "mode": "100644",
      "type": "blob",
      "sha": "701b038ade38b5055620e9490464f710f9b73f2b",
      "size": 4853,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/701b038ade38b5055620e9490464f710f9b73f2b"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction/queue_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b4df844be8008293b8697d2987608e9231472f65",
      "size": 10372,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b4df844be8008293b8697d2987608e9231472f65"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction/request_evictor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5771e404c311d232bca335f080f0075f5d53e998",
      "size": 10271,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5771e404c311d232bca335f080f0075f5d53e998"
    },
    {
      "path": "pkg/epp/flowcontrol/eviction/request_evictor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "85c5587261e652b853bf54ed9e3190f4d023f00a",
      "size": 12364,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85c5587261e652b853bf54ed9e3190f4d023f00a"
    },
    {
      "path": "pkg/epp/flowcontrol/integration_helpers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "581b5341d4c4ac915901243adbeca09e15bc9ec9",
      "size": 11421,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/581b5341d4c4ac915901243adbeca09e15bc9ec9"
    },
    {
      "path": "pkg/epp/flowcontrol/integration_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "119ad2e60f44e8ca42238ef7fe94410d57ff81de",
      "size": 65146,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/119ad2e60f44e8ca42238ef7fe94410d57ff81de"
    },
    {
      "path": "pkg/epp/flowcontrol/queue",
      "mode": "040000",
      "type": "tree",
      "sha": "89e386d448003cfe41557ea5054e20e8df74dadf",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/89e386d448003cfe41557ea5054e20e8df74dadf"
    },
    {
      "path": "pkg/epp/flowcontrol/queue/benchmark_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e443e04a0b48e121608b2eb948bfb803c527a012",
      "size": 4770,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e443e04a0b48e121608b2eb948bfb803c527a012"
    },
    {
      "path": "pkg/epp/flowcontrol/queue/priorityqueue.go",
      "mode": "100644",
      "type": "blob",
      "sha": "acdbd468d4e0562b55be268c9ef2412965a96925",
      "size": 8222,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/acdbd468d4e0562b55be268c9ef2412965a96925"
    },
    {
      "path": "pkg/epp/flowcontrol/queue/priorityqueue_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0e8cc2419cffe87b577fac3de8ac2dde05ea3ad3",
      "size": 16686,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0e8cc2419cffe87b577fac3de8ac2dde05ea3ad3"
    },
    {
      "path": "pkg/epp/flowcontrol/registry",
      "mode": "040000",
      "type": "tree",
      "sha": "fc1889181ff316fc474f35dbd75f867f437dad92",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/fc1889181ff316fc474f35dbd75f867f437dad92"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f411f2b8d9a60426b35715917197bdc687ae5dca",
      "size": 19514,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f411f2b8d9a60426b35715917197bdc687ae5dca"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0e92d087bfced2a559b2a3c0937f9d399c732759",
      "size": 15987,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0e92d087bfced2a559b2a3c0937f9d399c732759"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/connection.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9c628beb89f2a284c1ccc8df92e091b41ed3ddd6",
      "size": 1668,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9c628beb89f2a284c1ccc8df92e091b41ed3ddd6"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "514204ed52eead1b8f4cc43c0f5386ead48318ef",
      "size": 1540,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/514204ed52eead1b8f4cc43c0f5386ead48318ef"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/leasing.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ad702f5e5659ceddb35dab9b96ae8131bdb3145d",
      "size": 4083,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ad702f5e5659ceddb35dab9b96ae8131bdb3145d"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/leasing_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7fc5224abda4bb0e4a34f3413575604f0b1e38d4",
      "size": 7226,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7fc5224abda4bb0e4a34f3413575604f0b1e38d4"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/managedqueue.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e9eb1f622b66dbf348223eed9f6b2ad138991767",
      "size": 9405,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e9eb1f622b66dbf348223eed9f6b2ad138991767"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/managedqueue_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3ea7ad9de8902e0fa61e97dce5448643920e74a4",
      "size": 15283,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3ea7ad9de8902e0fa61e97dce5448643920e74a4"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/registry.go",
      "mode": "100644",
      "type": "blob",
      "sha": "45574fd840213fdbb58e15cf5f5d907676972163",
      "size": 21350,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/45574fd840213fdbb58e15cf5f5d907676972163"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/registry_helpers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "feaea014bac87c157c7111ab7909e9a5c7f14f1b",
      "size": 12654,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/feaea014bac87c157c7111ab7909e9a5c7f14f1b"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/registry_helpers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1612423e2da0465b354030c46cb07065402599ea",
      "size": 27067,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1612423e2da0465b354030c46cb07065402599ea"
    },
    {
      "path": "pkg/epp/flowcontrol/registry/registry_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "271ce125f95ff001d0b926038125f780b6a934cf",
      "size": 49858,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/271ce125f95ff001d0b926038125f780b6a934cf"
    },
    {
      "path": "pkg/epp/flowcontrol/types",
      "mode": "040000",
      "type": "tree",
      "sha": "bf12e6b611c1cc9e07628b34d63662f4d04466a8",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/bf12e6b611c1cc9e07628b34d63662f4d04466a8"
    },
    {
      "path": "pkg/epp/flowcontrol/types/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "452cd7a58a9326994789cb76d5e361d0e14fcf97",
      "size": 2807,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/452cd7a58a9326994789cb76d5e361d0e14fcf97"
    },
    {
      "path": "pkg/epp/flowcontrol/types/errors.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f2661944f975e56e4f29cadc85016e7d0ad7c426",
      "size": 3646,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f2661944f975e56e4f29cadc85016e7d0ad7c426"
    },
    {
      "path": "pkg/epp/flowcontrol/types/outcomes.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ac840fa60a7d4227cc4eb46bedfaff58d3c04a3e",
      "size": 7645,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ac840fa60a7d4227cc4eb46bedfaff58d3c04a3e"
    },
    {
      "path": "pkg/epp/flowcontrol/types/outcomes_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "76bcd30f8e1beaf2e0cfb5d4103a288519d752ab",
      "size": 5761,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/76bcd30f8e1beaf2e0cfb5d4103a288519d752ab"
    },
    {
      "path": "pkg/epp/framework",
      "mode": "040000",
      "type": "tree",
      "sha": "5cb9ae048c63c4e25f6337ac0f68a65caf50078b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5cb9ae048c63c4e25f6337ac0f68a65caf50078b"
    },
    {
      "path": "pkg/epp/framework/common",
      "mode": "040000",
      "type": "tree",
      "sha": "6a3f46b8460642c958ef3414afa5c3d6258c26c3",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6a3f46b8460642c958ef3414afa5c3d6258c26c3"
    },
    {
      "path": "pkg/epp/framework/common/request",
      "mode": "040000",
      "type": "tree",
      "sha": "e6f760c545c9bb6d403679e7cc28bf78a8e46e92",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/e6f760c545c9bb6d403679e7cc28bf78a8e46e92"
    },
    {
      "path": "pkg/epp/framework/common/request/headers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9dc7853db73e7d7f846a5e53386440767fabdd4a",
      "size": 1549,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9dc7853db73e7d7f846a5e53386440767fabdd4a"
    },
    {
      "path": "pkg/epp/framework/common/request/headers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "23a3810aa6e974d54317bc94c1371c3eb2567fde",
      "size": 3577,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/23a3810aa6e974d54317bc94c1371c3eb2567fde"
    },
    {
      "path": "pkg/epp/framework/interface",
      "mode": "040000",
      "type": "tree",
      "sha": "a25ffd41d05126876d90fdf90aa15c55452fb890",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a25ffd41d05126876d90fdf90aa15c55452fb890"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer",
      "mode": "040000",
      "type": "tree",
      "sha": "d186e49303e1a52545f406764c064120218386da",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d186e49303e1a52545f406764c064120218386da"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/attributemap.go",
      "mode": "100644",
      "type": "blob",
      "sha": "334c2db1de13349c5447adb6097d7621b7b5074d",
      "size": 3923,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/334c2db1de13349c5447adb6097d7621b7b5074d"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/attributemap_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d7f734391d0637a8617bff658132e1e84f09a02b",
      "size": 4115,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d7f734391d0637a8617bff658132e1e84f09a02b"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/cross_replica_syncer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "82020a974a8e1a54137014e5d3285a4ab33fd1f8",
      "size": 3861,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/82020a974a8e1a54137014e5d3285a4ab33fd1f8"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/discovery.go",
      "mode": "100644",
      "type": "blob",
      "sha": "85f5a57706755ecb9ba0fdf29fae5a59cd91b36c",
      "size": 4163,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85f5a57706755ecb9ba0fdf29fae5a59cd91b36c"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/discovery_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "155d0aed9fd3317e34a66727bd46528dfb3770ee",
      "size": 2099,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/155d0aed9fd3317e34a66727bd46528dfb3770ee"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/endpoint.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6b78e318c3f6088fb85319a707115b9c95adcc80",
      "size": 2704,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6b78e318c3f6088fb85319a707115b9c95adcc80"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/endpoint_metadata.go",
      "mode": "100644",
      "type": "blob",
      "sha": "76a243331946081aa7970eba525762451e9070d5",
      "size": 3880,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/76a243331946081aa7970eba525762451e9070d5"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/endpoint_metadata_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1096719a627d6994bc238f927ab557434afe606a",
      "size": 3794,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1096719a627d6994bc238f927ab557434afe606a"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "62d0de659cff94e4779bd30b1f1ad14fffb13fab",
      "size": 2808,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/62d0de659cff94e4779bd30b1f1ad14fffb13fab"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "be7439d115f5b356036099009f5a5d303f8fcb70",
      "size": 1728,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/be7439d115f5b356036099009f5a5d303f8fcb70"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/peer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ff2b00f2e4a7b24618cf51ee399d60fd87840dbd",
      "size": 3390,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ff2b00f2e4a7b24618cf51ee399d60fd87840dbd"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/peer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "241de03054fef1dc333d7141faeef5af4a892949",
      "size": 3806,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/241de03054fef1dc333d7141faeef5af4a892949"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7543bce82fc3e26ce93d5365d74c7729fb1c8eee",
      "size": 4636,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7543bce82fc3e26ce93d5365d74c7729fb1c8eee"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/registrar.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1a64805eaaf47ba1a24d05b30fa6d78401547dbe",
      "size": 2449,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1a64805eaaf47ba1a24d05b30fa6d78401547dbe"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/registry.go",
      "mode": "100644",
      "type": "blob",
      "sha": "720c66f023d0a38442ec9af6fc8c95d78ad15832",
      "size": 6420,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/720c66f023d0a38442ec9af6fc8c95d78ad15832"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/registry_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "53e840d6717c2c6a6c38a06f126395f65d0a87b3",
      "size": 7514,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/53e840d6717c2c6a6c38a06f126395f65d0a87b3"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/slot.go",
      "mode": "100644",
      "type": "blob",
      "sha": "cd12a1126adb40428a40540a307518c6857dd623",
      "size": 4024,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cd12a1126adb40428a40540a307518c6857dd623"
    },
    {
      "path": "pkg/epp/framework/interface/datalayer/slot_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9bde7b6fa517cedc363dc7419b296cb9d099b20f",
      "size": 3094,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9bde7b6fa517cedc363dc7419b296cb9d099b20f"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol",
      "mode": "040000",
      "type": "tree",
      "sha": "56942d5c62093751da70c92d7bcf08b4074e03d0",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/56942d5c62093751da70c92d7bcf08b4074e03d0"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/accessors.go",
      "mode": "100644",
      "type": "blob",
      "sha": "24df5e2c8583e229cd53cc5e9640bf70e2b04e4f",
      "size": 4136,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/24df5e2c8583e229cd53cc5e9640bf70e2b04e4f"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ca272150aa024cd8e9ad357d655d930e6585d736",
      "size": 1592,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ca272150aa024cd8e9ad357d655d930e6585d736"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/eviction.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6c243f407af5db43d60c56a48b1535699f35bbc8",
      "size": 2617,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6c243f407af5db43d60c56a48b1535699f35bbc8"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/flow.go",
      "mode": "100644",
      "type": "blob",
      "sha": "efb80dc6011b1c10a125657cb5655d2121d28a6c",
      "size": 2766,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/efb80dc6011b1c10a125657cb5655d2121d28a6c"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/mocks",
      "mode": "040000",
      "type": "tree",
      "sha": "84d8a880440fd9be6b75cdfff1eb7c1d3f9563c4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/84d8a880440fd9be6b75cdfff1eb7c1d3f9563c4"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/mocks/mocks.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ef1b47b1ae581af95001f6abac35c05762986aae",
      "size": 9745,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ef1b47b1ae581af95001f6abac35c05762986aae"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/plugins.go",
      "mode": "100644",
      "type": "blob",
      "sha": "46420373815a694bce449ffb1c3f849498edf2a4",
      "size": 9650,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/46420373815a694bce449ffb1c3f849498edf2a4"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/queue.go",
      "mode": "100644",
      "type": "blob",
      "sha": "45ecb52e247ec99adc864add201e457e3e5db52e",
      "size": 1079,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/45ecb52e247ec99adc864add201e457e3e5db52e"
    },
    {
      "path": "pkg/epp/framework/interface/flowcontrol/request.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dadc43ff5590179497f9a0391bf612b60eef4ef0",
      "size": 7238,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dadc43ff5590179497f9a0391bf612b60eef4ef0"
    },
    {
      "path": "pkg/epp/framework/interface/plugin",
      "mode": "040000",
      "type": "tree",
      "sha": "0d76c4492cc22b9f7e92ec2730b8f29ba25d95e6",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0d76c4492cc22b9f7e92ec2730b8f29ba25d95e6"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/datakey.go",
      "mode": "100644",
      "type": "blob",
      "sha": "aa33c86a66952f6ff38c0e82b797618734e07759",
      "size": 1421,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/aa33c86a66952f6ff38c0e82b797618734e07759"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/datakey_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0e4fd9270e9aa16786c99e3104dcf645eb07dc85",
      "size": 1371,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0e4fd9270e9aa16786c99e3104dcf645eb07dc85"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/handle.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f87def6ffd1478a70a60aa54949c85cf0fbbea32",
      "size": 4956,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f87def6ffd1478a70a60aa54949c85cf0fbbea32"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/handle_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c413328e288f87c8e1062b942810d761d5ea4e66",
      "size": 4445,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c413328e288f87c8e1062b942810d761d5ea4e66"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/metrics_recorder.go",
      "mode": "100644",
      "type": "blob",
      "sha": "17551eb4e2e0c9dbc752387be09e2830fc5b2384",
      "size": 834,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/17551eb4e2e0c9dbc752387be09e2830fc5b2384"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/plugin_state.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b5ee2900abe30be2f8156d592fa0c2f9a5f5fa2a",
      "size": 10432,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b5ee2900abe30be2f8156d592fa0c2f9a5f5fa2a"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/plugin_state_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c2c2c501b95c835e90b98921219e07cb1aec8e66",
      "size": 14696,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c2c2c501b95c835e90b98921219e07cb1aec8e66"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/plugins.go",
      "mode": "100644",
      "type": "blob",
      "sha": "12557fdcc8ca28fd414aedcc1a0207b6cbcb66e8",
      "size": 3869,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/12557fdcc8ca28fd414aedcc1a0207b6cbcb66e8"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/plugins_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0ded734c2d25a1bc69f9e96cce816da9c6690458",
      "size": 2587,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0ded734c2d25a1bc69f9e96cce816da9c6690458"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/registry.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8a8bd240d74e6d6f1e828f6540f47130cb263555",
      "size": 5271,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8a8bd240d74e6d6f1e828f6540f47130cb263555"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/registry_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e6edc596e7e34c1e8a352fbe084af2c60dbf2dcd",
      "size": 5508,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e6edc596e7e34c1e8a352fbe084af2c60dbf2dcd"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/shared_state.go",
      "mode": "100644",
      "type": "blob",
      "sha": "144c15177bc959d480c37809834bfa9e237f9b1e",
      "size": 1545,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/144c15177bc959d480c37809834bfa9e237f9b1e"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/shared_state_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1faf159e7ad3c8ec126863c76c7df4a186e1d7a0",
      "size": 1606,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1faf159e7ad3c8ec126863c76c7df4a186e1d7a0"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/stability.go",
      "mode": "100644",
      "type": "blob",
      "sha": "32d620adcdbe35d965717fe708b5b5cba58abb75",
      "size": 1415,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/32d620adcdbe35d965717fe708b5b5cba58abb75"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/typedname.go",
      "mode": "100644",
      "type": "blob",
      "sha": "490ebb5c6205856e0b7543a9b716396f375e7a82",
      "size": 971,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/490ebb5c6205856e0b7543a9b716396f375e7a82"
    },
    {
      "path": "pkg/epp/framework/interface/plugin/typedname_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "59a749a25e73870e47a9e0a9209587ceec142379",
      "size": 1347,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/59a749a25e73870e47a9e0a9209587ceec142379"
    },
    {
      "path": "pkg/epp/framework/interface/requestcontrol",
      "mode": "040000",
      "type": "tree",
      "sha": "65644f5c664829bf37a86c6c589a3328989e71de",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/65644f5c664829bf37a86c6c589a3328989e71de"
    },
    {
      "path": "pkg/epp/framework/interface/requestcontrol/plugins.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9d98c863de0847d0c22dcf76d32b5d8a9f02ef2c",
      "size": 6278,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9d98c863de0847d0c22dcf76d32b5d8a9f02ef2c"
    },
    {
      "path": "pkg/epp/framework/interface/requestcontrol/types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b2b987d4870c1722bc52d5f604951657845cf7c4",
      "size": 3869,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b2b987d4870c1722bc52d5f604951657845cf7c4"
    },
    {
      "path": "pkg/epp/framework/interface/requesthandling",
      "mode": "040000",
      "type": "tree",
      "sha": "5bb02fcf67d2db43446a3116664555f6c390f681",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5bb02fcf67d2db43446a3116664555f6c390f681"
    },
    {
      "path": "pkg/epp/framework/interface/requesthandling/plugins.go",
      "mode": "100644",
      "type": "blob",
      "sha": "99b09b9885b682230df11f59ebb7435b9583f8df",
      "size": 4390,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/99b09b9885b682230df11f59ebb7435b9583f8df"
    },
    {
      "path": "pkg/epp/framework/interface/requesthandling/token_ids.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4e60b144b8d60e29b3a90a88f169596d3d0e96f6",
      "size": 3484,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4e60b144b8d60e29b3a90a88f169596d3d0e96f6"
    },
    {
      "path": "pkg/epp/framework/interface/requesthandling/token_ids_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ad36b0c069b2b28cf3ad7fecb5cb92eaed69a9d2",
      "size": 8681,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ad36b0c069b2b28cf3ad7fecb5cb92eaed69a9d2"
    },
    {
      "path": "pkg/epp/framework/interface/requesthandling/types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "31cda251b5295b9cc765c2be785358d970c002f6",
      "size": 30132,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/31cda251b5295b9cc765c2be785358d970c002f6"
    },
    {
      "path": "pkg/epp/framework/interface/requesthandling/types_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c96da9532082a55f0a8bc4b2512dcfc3b0332edd",
      "size": 12676,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c96da9532082a55f0a8bc4b2512dcfc3b0332edd"
    },
    {
      "path": "pkg/epp/framework/interface/scheduling",
      "mode": "040000",
      "type": "tree",
      "sha": "b6bf8d4c8c9fe37ef57f2766cee1036f29a9d9f2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b6bf8d4c8c9fe37ef57f2766cee1036f29a9d9f2"
    },
    {
      "path": "pkg/epp/framework/interface/scheduling/attributes.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b4e36ea2612196f374884bdb6c55ff9c66a77eb9",
      "size": 2474,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b4e36ea2612196f374884bdb6c55ff9c66a77eb9"
    },
    {
      "path": "pkg/epp/framework/interface/scheduling/attributes_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "42f9b4c527e29e364a03757036792243d0c0160c",
      "size": 2959,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/42f9b4c527e29e364a03757036792243d0c0160c"
    },
    {
      "path": "pkg/epp/framework/interface/scheduling/plugins.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6758011e4268d0225e81f04902b53cf28254833a",
      "size": 3382,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6758011e4268d0225e81f04902b53cf28254833a"
    },
    {
      "path": "pkg/epp/framework/interface/scheduling/plugins_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "36fd12748de79ff69d04f8230f0b37cdabf6bd22",
      "size": 4621,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/36fd12748de79ff69d04f8230f0b37cdabf6bd22"
    },
    {
      "path": "pkg/epp/framework/interface/scheduling/types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e1c156784802f6fb3782ffb2cb5a70b28d16e4d4",
      "size": 5170,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e1c156784802f6fb3782ffb2cb5a70b28d16e4d4"
    },
    {
      "path": "pkg/epp/framework/interface/scheduling/types_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7fd452a6521692c7105c9665f3680b8b743eacb7",
      "size": 6369,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7fd452a6521692c7105c9665f3680b8b743eacb7"
    },
    {
      "path": "pkg/epp/framework/observability",
      "mode": "040000",
      "type": "tree",
      "sha": "1030a4f004c8855f8c22a8810af5652fc4101254",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1030a4f004c8855f8c22a8810af5652fc4101254"
    },
    {
      "path": "pkg/epp/framework/observability/multimodal",
      "mode": "040000",
      "type": "tree",
      "sha": "3e406008801ea9fc1c992f825c439681022bd9ad",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/3e406008801ea9fc1c992f825c439681022bd9ad"
    },
    {
      "path": "pkg/epp/framework/observability/multimodal/multimodal.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2d60af0b6f05098ec972dbb1abb86404c9ed270b",
      "size": 2464,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2d60af0b6f05098ec972dbb1abb86404c9ed270b"
    },
    {
      "path": "pkg/epp/framework/observability/multimodal/multimodal_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "048363d2de6d938f30894209fdf4a3c4195e22ca",
      "size": 3316,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/048363d2de6d938f30894209fdf4a3c4195e22ca"
    },
    {
      "path": "pkg/epp/framework/plugins",
      "mode": "040000",
      "type": "tree",
      "sha": "6f658e6c05b42405d906307daec1465d69b529f1",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6f658e6c05b42405d906307daec1465d69b529f1"
    },
    {
      "path": "pkg/epp/framework/plugins/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "e25d7bd9c21b8c2c9c7b30ecad73c23cfb5b674d",
      "size": 4437,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e25d7bd9c21b8c2c9c7b30ecad73c23cfb5b674d"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer",
      "mode": "040000",
      "type": "tree",
      "sha": "4b4b281f079cf4f4a42b944bf025f7acd3b49016",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4b4b281f079cf4f4a42b944bf025f7acd3b49016"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute",
      "mode": "040000",
      "type": "tree",
      "sha": "593e9b0b75cf086aee4867fd08d79e3cfe1d1a43",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/593e9b0b75cf086aee4867fd08d79e3cfe1d1a43"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/concurrency",
      "mode": "040000",
      "type": "tree",
      "sha": "81d5a056b9024fe0bca7e75f5f534f8dad5738af",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/81d5a056b9024fe0bca7e75f5f534f8dad5738af"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/concurrency/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "50da2e2394ca0abc5e163bb65f32754ca6b6892d",
      "size": 580,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/50da2e2394ca0abc5e163bb65f32754ca6b6892d"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/concurrency/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ec2a7b5acd442d35e63e44632bbc3dccc02ae0a2",
      "size": 3567,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ec2a7b5acd442d35e63e44632bbc3dccc02ae0a2"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/gpu",
      "mode": "040000",
      "type": "tree",
      "sha": "ff02479ccb376d12316db934a0b36475210f8770",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ff02479ccb376d12316db934a0b36475210f8770"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/gpu/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5c02ac4968c689be0b5a0fe15e94c159489c03a4",
      "size": 413,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5c02ac4968c689be0b5a0fe15e94c159489c03a4"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/gpu/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "df0c4842a3806d1ec04b9bbbb5d05cb01c7980cb",
      "size": 1033,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/df0c4842a3806d1ec04b9bbbb5d05cb01c7980cb"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/latency",
      "mode": "040000",
      "type": "tree",
      "sha": "f5fa40db707d61f02961150fc953db05d917fc8e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f5fa40db707d61f02961150fc953db05d917fc8e"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/latency/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "bf1afc8134317ba272fbfcf9d0cfa946e1870fc0",
      "size": 998,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf1afc8134317ba272fbfcf9d0cfa946e1870fc0"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/latency/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a8cda7505f5ccbdf3327fd898ed9c4c6ba657f26",
      "size": 3808,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a8cda7505f5ccbdf3327fd898ed9c4c6ba657f26"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/latency/ttft_percentiles.go",
      "mode": "100644",
      "type": "blob",
      "sha": "71e81a8a01fbdcf1d89b0156935fe72b116a2fcb",
      "size": 3171,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/71e81a8a01fbdcf1d89b0156935fe72b116a2fcb"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/metrics",
      "mode": "040000",
      "type": "tree",
      "sha": "3020ad69426c665d270824997a71c2b3581d637a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/3020ad69426c665d270824997a71c2b3581d637a"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/metrics/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4cfc3ad5eec9bb29f1502631e778a0a831835a2d",
      "size": 3186,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4cfc3ad5eec9bb29f1502631e778a0a831835a2d"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/metrics/data_types_multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9bf3e61105cc9c4216769b4ba9fd43e0d32f870a",
      "size": 1532,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9bf3e61105cc9c4216769b4ba9fd43e0d32f870a"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/metrics/data_types_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ea905bb5395d7263bf2d5d713007fa337d14d8bb",
      "size": 2442,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ea905bb5395d7263bf2d5d713007fa337d14d8bb"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/models",
      "mode": "040000",
      "type": "tree",
      "sha": "cfef7541c37be628239678336e5b3cade24cca78",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/cfef7541c37be628239678336e5b3cade24cca78"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/models/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "0033a9b7026960c4a7caa1c93117204b380dcb35",
      "size": 944,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0033a9b7026960c4a7caa1c93117204b380dcb35"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/models/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8102991d21434c60ecdf29e6c64719c2f71d4fb1",
      "size": 1849,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8102991d21434c60ecdf29e6c64719c2f71d4fb1"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/multimodal",
      "mode": "040000",
      "type": "tree",
      "sha": "94d60ffc9b6240023c9467bbc3d2a4eb8ad34f05",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/94d60ffc9b6240023c9467bbc3d2a4eb8ad34f05"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/multimodal/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c218a6110719c3772a88e0ec0c24054cf1b465da",
      "size": 2708,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c218a6110719c3772a88e0ec0c24054cf1b465da"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/prefix",
      "mode": "040000",
      "type": "tree",
      "sha": "679574cd30711406c1cd16f5f2e5b20023a231d4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/679574cd30711406c1cd16f5f2e5b20023a231d4"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/prefix/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "c1026551fef9319cb4275380656ba2835c3b8dc5",
      "size": 1196,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c1026551fef9319cb4275380656ba2835c3b8dc5"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/prefix/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "54f5a78bab5340a7dcb4ef5d6c56f4073b7d409a",
      "size": 4887,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/54f5a78bab5340a7dcb4ef5d6c56f4073b7d409a"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/prefix/data_types_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f7c804b4917e75767ce74df82b024cd582027d72",
      "size": 5004,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f7c804b4917e75767ce74df82b024cd582027d72"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/session",
      "mode": "040000",
      "type": "tree",
      "sha": "7c26145401c454903a2f80e5eba03e3b340a4555",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7c26145401c454903a2f80e5eba03e3b340a4555"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/session/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "0b3c8a9d052fb634079aa804e55577c2401aa1c5",
      "size": 724,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0b3c8a9d052fb634079aa804e55577c2401aa1c5"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/session/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3010caa38c77a350f8a1a84a2cb47113414a63fc",
      "size": 2036,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3010caa38c77a350f8a1a84a2cb47113414a63fc"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/topology",
      "mode": "040000",
      "type": "tree",
      "sha": "ca8a6dc93d98294e4ab0df4dcfdab500d6a45c70",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ca8a6dc93d98294e4ab0df4dcfdab500d6a45c70"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/topology/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "57ef9bfd37fc2e62bd489385259bd197a7c17ded",
      "size": 1290,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/57ef9bfd37fc2e62bd489385259bd197a7c17ded"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/attribute/topology/data_types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3b07d6ea3c222a3c1ac169320fc1bdbf0b8ddf6d",
      "size": 1865,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3b07d6ea3c222a3c1ac169320fc1bdbf0b8ddf6d"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/cross_plugin",
      "mode": "040000",
      "type": "tree",
      "sha": "fc11f9041a23609fd078b772d9f498da031c8839",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/fc11f9041a23609fd078b772d9f498da031c8839"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/cross_plugin/local",
      "mode": "040000",
      "type": "tree",
      "sha": "9567636252d1f42ca987a5ad9a3e018e3610b8dd",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/9567636252d1f42ca987a5ad9a3e018e3610b8dd"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/cross_plugin/local/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "fecaaa796d998d090efd440145ec38249b25b28b",
      "size": 1049,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fecaaa796d998d090efd440145ec38249b25b28b"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/cross_plugin/local/local_syncer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "aba89609d5ff70ba5003c503ed8ec4b957dd259c",
      "size": 2500,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/aba89609d5ff70ba5003c503ed8ec4b957dd259c"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/cross_plugin/local/local_syncer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4b25f397c3d71f787871faec0a6849c972408878",
      "size": 1746,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4b25f397c3d71f787871faec0a6849c972408878"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/discovery",
      "mode": "040000",
      "type": "tree",
      "sha": "88a3afd59623ad43f45653f0c3c1b61f4ad89f9c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/88a3afd59623ad43f45653f0c3c1b61f4ad89f9c"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/discovery/file",
      "mode": "040000",
      "type": "tree",
      "sha": "5fcdf428c1bebe2d8d0780e4cafee20872bb558c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5fcdf428c1bebe2d8d0780e4cafee20872bb558c"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/discovery/file/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "473ffc995865a95cba2b9ddc55cf4321df66c246",
      "size": 4500,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/473ffc995865a95cba2b9ddc55cf4321df66c246"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/discovery/file/multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e87a8dbbc692128ef2dbd8e22b8355d01e25d246",
      "size": 3396,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e87a8dbbc692128ef2dbd8e22b8355d01e25d246"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/discovery/file/multicluster_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4627aac89cd33cf1a34c5e67ce7eccb4abeb6d80",
      "size": 6106,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4627aac89cd33cf1a34c5e67ce7eccb4abeb6d80"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "85572e90f9d5d3c05ced53e49b0e2086325fb5fb",
      "size": 9669,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85572e90f9d5d3c05ced53e49b0e2086325fb5fb"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/discovery/file/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "527fd2458c692e3e1cbe975a79a007fd2c320026",
      "size": 10631,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/527fd2458c692e3e1cbe975a79a007fd2c320026"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor",
      "mode": "040000",
      "type": "tree",
      "sha": "3aa82789ae257502fdc834e5b2f35c5478d12a02",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/3aa82789ae257502fdc834e5b2f35c5478d12a02"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/dcgm",
      "mode": "040000",
      "type": "tree",
      "sha": "e5283e4038036254920024cd0c807d2008e0de07",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/e5283e4038036254920024cd0c807d2008e0de07"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/dcgm/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1bf9e5ec3b2980d053fb7cd5576dd411aa17c9a2",
      "size": 2935,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1bf9e5ec3b2980d053fb7cd5576dd411aa17c9a2"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/dcgm/extractor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "afa9ebdc102773e7e415d38691e1839896e1639f",
      "size": 4422,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/afa9ebdc102773e7e415d38691e1839896e1639f"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/dcgm/extractor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "76c87228179bd550180697843d0e9f8075cfc6fc",
      "size": 7567,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/76c87228179bd550180697843d0e9f8075cfc6fc"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics",
      "mode": "040000",
      "type": "tree",
      "sha": "540c81180a6bec1de47b11376c3dd76a96aed08d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/540c81180a6bec1de47b11376c3dd76a96aed08d"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "9a3c84f780a44b9d45a7fa5046a2541c86a241c0",
      "size": 3718,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9a3c84f780a44b9d45a7fa5046a2541c86a241c0"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/extractor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "07a319c9328f6fb3c8ea5a839431fc517df51ad8",
      "size": 10391,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/07a319c9328f6fb3c8ea5a839431fc517df51ad8"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/extractor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d37b7b45b54947ce732743088733ebed988d0fb6",
      "size": 23588,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d37b7b45b54947ce732743088733ebed988d0fb6"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/factories.go",
      "mode": "100644",
      "type": "blob",
      "sha": "26a3aba1f83a8acf577468716876fa7ea38969da",
      "size": 11535,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/26a3aba1f83a8acf577468716876fa7ea38969da"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/loraspec.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b87a8cc773ee2dd15ac28d973cf9a1e1b444d112",
      "size": 2585,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b87a8cc773ee2dd15ac28d973cf9a1e1b444d112"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/mapping.go",
      "mode": "100644",
      "type": "blob",
      "sha": "704902dcfb7e1209c075a71db0bb4997805675df",
      "size": 6015,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/704902dcfb7e1209c075a71db0bb4997805675df"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/mapping_registry.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c32fa468a70699ec8c15511fb6c5f8317cc948ea",
      "size": 3190,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c32fa468a70699ec8c15511fb6c5f8317cc948ea"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/mapping_registry_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "63e2fd372dd62dc0445c87744b69fc80c584a393",
      "size": 1908,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/63e2fd372dd62dc0445c87744b69fc80c584a393"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/metrics_extraction_from_config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9c08812155c4c46cc4cbdd302063211ac570e31d",
      "size": 22358,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9c08812155c4c46cc4cbdd302063211ac570e31d"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/mock_server_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "779cae08a636fa213f5bfe93db3490aaf157dcd2",
      "size": 2365,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/779cae08a636fa213f5bfe93db3490aaf157dcd2"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c455de0704566cddb12cb9ba076540f90cf1fd01",
      "size": 4632,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c455de0704566cddb12cb9ba076540f90cf1fd01"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/multicluster_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "15e9e87f7d15e78e3e4e904e038a799b6a718008",
      "size": 5497,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/15e9e87f7d15e78e3e4e904e038a799b6a718008"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/spec.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d1f1b225238bf50cd6e8d1488b6354a753cdd0da",
      "size": 5142,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d1f1b225238bf50cd6e8d1488b6354a753cdd0da"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/metrics/spec_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a0bd075c6d8411e3abef39e81e758d9b8ecd5b3a",
      "size": 13443,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a0bd075c6d8411e3abef39e81e758d9b8ecd5b3a"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/mocks",
      "mode": "040000",
      "type": "tree",
      "sha": "64efb1c8b11613acbbe2258f913a19546b4b2368",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/64efb1c8b11613acbbe2258f913a19546b4b2368"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/mocks/endpoint_extractor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0f4889455934fff3f7d57ce5a300fabf3b8477d1",
      "size": 2545,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0f4889455934fff3f7d57ce5a300fabf3b8477d1"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/mocks/k8s_notification_extractor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "24425386c642dd156de9dba81b7dfacdb3814295",
      "size": 4945,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/24425386c642dd156de9dba81b7dfacdb3814295"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/models",
      "mode": "040000",
      "type": "tree",
      "sha": "657e0b64a922630ec52a1628fe0f85402607a619",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/657e0b64a922630ec52a1628fe0f85402607a619"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/models/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "69358a070e13663f9d61d6c46016139632b2e2bd",
      "size": 950,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/69358a070e13663f9d61d6c46016139632b2e2bd"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/models/extractor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6391e0dadffc63d1c3e1025333edafb8228bc5b0",
      "size": 2240,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6391e0dadffc63d1c3e1025333edafb8228bc5b0"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/models/extractor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "46f02360f6e911d9f838db2f1b16b5ad085e686e",
      "size": 2553,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/46f02360f6e911d9f838db2f1b16b5ad085e686e"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/topology",
      "mode": "040000",
      "type": "tree",
      "sha": "6f42c5770caf4adbec710ab4dba103b51e81c8b2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6f42c5770caf4adbec710ab4dba103b51e81c8b2"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/topology/extractor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8c7168abd76fbb981cc438b1352c3f9edef38577",
      "size": 11169,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8c7168abd76fbb981cc438b1352c3f9edef38577"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/extractor/topology/extractor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3a1e38cf87aabafdd8efe38978121b19203e34f5",
      "size": 19037,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3a1e38cf87aabafdd8efe38978121b19203e34f5"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source",
      "mode": "040000",
      "type": "tree",
      "sha": "19793f20163a073eea598758dbbc4ad959435f13",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/19793f20163a073eea598758dbbc4ad959435f13"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/dcgm",
      "mode": "040000",
      "type": "tree",
      "sha": "25be108d48ff5d3bd48c1bf76e3bdb7a30a0f396",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/25be108d48ff5d3bd48c1bf76e3bdb7a30a0f396"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/dcgm/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "66d53a1c3206ed363fa997d3639d09ced74969d3",
      "size": 2458,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/66d53a1c3206ed363fa997d3639d09ced74969d3"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/dcgm/datasource.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a3fbba5caec58c64caae6f14503d9c623dd08c91",
      "size": 3651,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a3fbba5caec58c64caae6f14503d9c623dd08c91"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/dcgm/datasource_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "965d3e5b8d5fbcb3cfefb23e64c9a93b3c2ee87b",
      "size": 3137,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/965d3e5b8d5fbcb3cfefb23e64c9a93b3c2ee87b"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http",
      "mode": "040000",
      "type": "tree",
      "sha": "d65106bc1e374d92ab515bca9482abf37f65137b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d65106bc1e374d92ab515bca9482abf37f65137b"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "2bcda31f816bae21865608ea30a9e007c8747e1c",
      "size": 631,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2bcda31f816bae21865608ea30a9e007c8747e1c"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http/client.go",
      "mode": "100644",
      "type": "blob",
      "sha": "92d71feeb731d940112b73a30b07fc3286d62759",
      "size": 2847,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/92d71feeb731d940112b73a30b07fc3286d62759"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http/datasource.go",
      "mode": "100644",
      "type": "blob",
      "sha": "97e669307be8eea1c62c71363e5791de95baefef",
      "size": 10686,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/97e669307be8eea1c62c71363e5791de95baefef"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http/datasource_catls_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "de43188fb830f0b989eb4f6529faaf690817ab2a",
      "size": 5811,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/de43188fb830f0b989eb4f6529faaf690817ab2a"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http/datasource_isolation_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "12b2467a64f9f702c0354bf76bd83444b379e97c",
      "size": 1833,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/12b2467a64f9f702c0354bf76bd83444b379e97c"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http/datasource_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bb84d5270983a24d957e375d2c23db28b3b3b5d9",
      "size": 11176,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bb84d5270983a24d957e375d2c23db28b3b3b5d9"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http/httptest",
      "mode": "040000",
      "type": "tree",
      "sha": "54a71473b5f71458b189adfcfdb94f3eb1421955",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/54a71473b5f71458b189adfcfdb94f3eb1421955"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/http/httptest/parsercontract.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a508011d4b36d9cd5ec8fd493e4e6c9058b3a84d",
      "size": 1036,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a508011d4b36d9cd5ec8fd493e4e6c9058b3a84d"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics",
      "mode": "040000",
      "type": "tree",
      "sha": "84521ac190c68a4d41900d3aec902753e16b5b82",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/84521ac190c68a4d41900d3aec902753e16b5b82"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "448fac913239c9579a852af30511456e14c6db49",
      "size": 2290,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/448fac913239c9579a852af30511456e14c6db49"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/datasource.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3686515940bfadcb67d657ede934d66ee47f3ee0",
      "size": 4794,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3686515940bfadcb67d657ede934d66ee47f3ee0"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/datasource_config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "76141ac293e5a5fb9e1df370e38acb966f66e47b",
      "size": 1760,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/76141ac293e5a5fb9e1df370e38acb966f66e47b"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/datasource_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e00e5eeadb767a4c1309c3f8f65f1799faf341c5",
      "size": 3146,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e00e5eeadb767a4c1309c3f8f65f1799faf341c5"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9323fcebaa5e857299c6dc1aad6f16f6a857ab28",
      "size": 3160,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9323fcebaa5e857299c6dc1aad6f16f6a857ab28"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/multicluster_security_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "577bf7c995c85564de23044ea9f07a173337fc90",
      "size": 4180,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/577bf7c995c85564de23044ea9f07a173337fc90"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/multicluster_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "08f18e758794519e7fe892e99847f85a27253fe8",
      "size": 1969,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/08f18e758794519e7fe892e99847f85a27253fe8"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/parsercontract_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3a52c8bb84235e7fffaa89d000625e004987fb63",
      "size": 450,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3a52c8bb84235e7fffaa89d000625e004987fb63"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/metrics/types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d154e6d99c746536336ac6976344dc6dc9203040",
      "size": 784,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d154e6d99c746536336ac6976344dc6dc9203040"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/mocks",
      "mode": "040000",
      "type": "tree",
      "sha": "0aa62aacd2939b457ed6e60c8e867ea40934ad50",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0aa62aacd2939b457ed6e60c8e867ea40934ad50"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/mocks/data_source_mock.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0db587a485d457259c05079707af45eab3fb1fda",
      "size": 3901,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0db587a485d457259c05079707af45eab3fb1fda"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/models",
      "mode": "040000",
      "type": "tree",
      "sha": "f4b95dec7d5d4711a142a3bafc76e1b98f41abbe",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f4b95dec7d5d4711a142a3bafc76e1b98f41abbe"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/models/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "747359b388ccfc4a7bd9f31b5937ad370accdcec",
      "size": 2172,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/747359b388ccfc4a7bd9f31b5937ad370accdcec"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/models/datasource.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7e76d6ca277c6c22b830ea8ccf76aa4c43a43bdc",
      "size": 3827,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7e76d6ca277c6c22b830ea8ccf76aa4c43a43bdc"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/models/datasource_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e4af8591902e0292385f56357038bc41040d60f2",
      "size": 2587,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e4af8591902e0292385f56357038bc41040d60f2"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/models/parsercontract_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bc89e91c8dffd87841a27e3d658563fa8d1cdcf2",
      "size": 398,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bc89e91c8dffd87841a27e3d658563fa8d1cdcf2"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/notifications",
      "mode": "040000",
      "type": "tree",
      "sha": "70d592f2415b0a9c63e6040b6fc3d5f0270bad4f",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/70d592f2415b0a9c63e6040b6fc3d5f0270bad4f"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/notifications/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1f91ddc770e27ca6c4690ccf46be97442704b9d7",
      "size": 4655,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1f91ddc770e27ca6c4690ccf46be97442704b9d7"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/notifications/endpoint_datasource.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ea18d66901170b2c4f01335b80f7717657337e66",
      "size": 1925,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ea18d66901170b2c4f01335b80f7717657337e66"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/notifications/endpoint_datasource_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9fe0c827c235318d1e6b2311b3a8c5d8dab631de",
      "size": 2843,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9fe0c827c235318d1e6b2311b3a8c5d8dab631de"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/notifications/factory.go",
      "mode": "100644",
      "type": "blob",
      "sha": "619eaacddb766bc87cf5dc40da72711ea3bdc330",
      "size": 2605,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/619eaacddb766bc87cf5dc40da72711ea3bdc330"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/notifications/k8s_datasource.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b6a6c1f9f6597330c10251c91c567be0de6fef85",
      "size": 2020,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b6a6c1f9f6597330c10251c91c567be0de6fef85"
    },
    {
      "path": "pkg/epp/framework/plugins/datalayer/source/notifications/k8s_datasource_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8878c31840f0e44a0a4c2597ba4b4ba83fc9a7b8",
      "size": 6298,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8878c31840f0e44a0a4c2597ba4b4ba83fc9a7b8"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol",
      "mode": "040000",
      "type": "tree",
      "sha": "47b89c110984291c34dcf9392f961e2a73bd857f",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/47b89c110984291c34dcf9392f961e2a73bd857f"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction",
      "mode": "040000",
      "type": "tree",
      "sha": "42143f934b265d30f4c65df8a3286a191e9abc7f",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/42143f934b265d30f4c65df8a3286a191e9abc7f"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction/filtering",
      "mode": "040000",
      "type": "tree",
      "sha": "619ad9313189aeefaa7b3b7478bb5f84628375f7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/619ad9313189aeefaa7b3b7478bb5f84628375f7"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction/filtering/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "df36f646db158be3f59ab21a11689ac6d825fdf2",
      "size": 624,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/df36f646db158be3f59ab21a11689ac6d825fdf2"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction/filtering/sheddable.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9d6b1a1c9bd7c995f1f0d5caf0a23f8ff31e77ce",
      "size": 1840,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9d6b1a1c9bd7c995f1f0d5caf0a23f8ff31e77ce"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction/filtering/sheddable_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "cb18605b9e1185013d91565a9773bdad00ad867e",
      "size": 1770,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cb18605b9e1185013d91565a9773bdad00ad867e"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction/ordering",
      "mode": "040000",
      "type": "tree",
      "sha": "c27932291112c96cd71645c381e2d8e6b9a99eb4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c27932291112c96cd71645c381e2d8e6b9a99eb4"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction/ordering/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "3273e6eb5f1d6f1e5b81a6ef440a135f05bc4139",
      "size": 822,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3273e6eb5f1d6f1e5b81a6ef440a135f05bc4139"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction/ordering/priority_time.go",
      "mode": "100644",
      "type": "blob",
      "sha": "265cda2a2b4bfb17099d98f2ac3c301228b1822b",
      "size": 1948,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/265cda2a2b4bfb17099d98f2ac3c301228b1822b"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/eviction/ordering/priority_time_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8f43a6d677e7d4787513f244a6f521e0c72909b3",
      "size": 2161,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8f43a6d677e7d4787513f244a6f521e0c72909b3"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness",
      "mode": "040000",
      "type": "tree",
      "sha": "f43c6c5c5f3c1bca0c8913b3caf74de48e80ec3a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f43c6c5c5f3c1bca0c8913b3caf74de48e80ec3a"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "c746cfd3d85675cbcbed7b7e61373dc2e003e5b0",
      "size": 1992,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c746cfd3d85675cbcbed7b7e61373dc2e003e5b0"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4788b52bc21255f71744c33c22e3c703c6ab078b",
      "size": 804,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4788b52bc21255f71744c33c22e3c703c6ab078b"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/functional_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "00caa6adc314da8c325f7166f15e5560bbbdc83f",
      "size": 5346,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/00caa6adc314da8c325f7166f15e5560bbbdc83f"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/globalstrict",
      "mode": "040000",
      "type": "tree",
      "sha": "b77a878e2e93137b4213065ceeb2fe984f01415c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b77a878e2e93137b4213065ceeb2fe984f01415c"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/globalstrict/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "9a6fe41f8fbdf57792cfcec0037e9a4baebbde0d",
      "size": 2722,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9a6fe41f8fbdf57792cfcec0037e9a4baebbde0d"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/globalstrict/global_strict.go",
      "mode": "100644",
      "type": "blob",
      "sha": "568f2e338af2f748ce3eb59c9edbb3a067d977e1",
      "size": 3690,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/568f2e338af2f748ce3eb59c9edbb3a067d977e1"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/globalstrict/global_strict_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fb8591b467573d67009d67c378e36083cedcc909",
      "size": 6176,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fb8591b467573d67009d67c378e36083cedcc909"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware",
      "mode": "040000",
      "type": "tree",
      "sha": "6a495171090f0c5271d24a0bf13403408f39cf29",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6a495171090f0c5271d24a0bf13403408f39cf29"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "9282f87e2567a2d9620a010a742c4b1effa97412",
      "size": 5886,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9282f87e2567a2d9620a010a742c4b1effa97412"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4fb0dff4a657ff6d69fff058da52cb58cac17931",
      "size": 10236,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4fb0dff4a657ff6d69fff058da52cb58cac17931"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "74bb1b52d305f44ad26331a1f5c62ee06e046237",
      "size": 11855,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/74bb1b52d305f44ad26331a1f5c62ee06e046237"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/programmetrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "089a0ce1d61b4b3ebfd3e2c9e73b3b8536b0da60",
      "size": 1370,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/089a0ce1d61b4b3ebfd3e2c9e73b3b8536b0da60"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/programmetrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f248e7a9eca6e318888226142efc6bf3e01edc96",
      "size": 1306,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f248e7a9eca6e318888226142efc6bf3e01edc96"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/prometheus.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0c75700d729c7b7f84f9d41a5b52735ef9772309",
      "size": 1161,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0c75700d729c7b7f84f9d41a5b52735ef9772309"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/strategy.go",
      "mode": "100644",
      "type": "blob",
      "sha": "93707e2fca91b2a9813b6b1994af5e08c2536791",
      "size": 1373,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/93707e2fca91b2a9813b6b1994af5e08c2536791"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/strategy_las.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c8871aa1ddbfad5a5c6e9191cb13698f1415200a",
      "size": 5761,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c8871aa1ddbfad5a5c6e9191cb13698f1415200a"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/program-aware/strategy_las_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4ec2d73552b5a21b28a23e6faeff67320ddee4a6",
      "size": 6036,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4ec2d73552b5a21b28a23e6faeff67320ddee4a6"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/roundrobin",
      "mode": "040000",
      "type": "tree",
      "sha": "19d979f4a9ab2655dbd61c7f2ded0d649d4927a9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/19d979f4a9ab2655dbd61c7f2ded0d649d4927a9"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/roundrobin/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "68cede1ea66ebdf57cb493e80dd87386284c9d01",
      "size": 2040,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/68cede1ea66ebdf57cb493e80dd87386284c9d01"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/roundrobin/roundrobin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9aa44998fe479b1806731d59f1fece3115b2a813",
      "size": 3932,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9aa44998fe479b1806731d59f1fece3115b2a813"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/fairness/roundrobin/roundrobin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d2247a6e44c6e0b72633eab5f28893a4ccfcfb4f",
      "size": 9995,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d2247a6e44c6e0b72633eab5f28893a4ccfcfb4f"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering",
      "mode": "040000",
      "type": "tree",
      "sha": "8888fa00f0a46f5ce835f19d3f061e4043aff7cf",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8888fa00f0a46f5ce835f19d3f061e4043aff7cf"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "6b9e0d7da0ada4c3b278b2e16d9911271e2006cb",
      "size": 1774,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6b9e0d7da0ada4c3b278b2e16d9911271e2006cb"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "afcd903a46493dcd5f9dc982714deca334a7d9dc",
      "size": 2790,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/afcd903a46493dcd5f9dc982714deca334a7d9dc"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/edf",
      "mode": "040000",
      "type": "tree",
      "sha": "f087b51c243d7eb8ad011911b2dffd9c244edd48",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f087b51c243d7eb8ad011911b2dffd9c244edd48"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/edf/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "4368e3b0a32f0eab83389241d5c673b735bfe58f",
      "size": 2479,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4368e3b0a32f0eab83389241d5c673b735bfe58f"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/edf/edf.go",
      "mode": "100644",
      "type": "blob",
      "sha": "88431b1751a268c03bfc7e77bf5027657ff1c8cc",
      "size": 3417,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/88431b1751a268c03bfc7e77bf5027657ff1c8cc"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/edf/edf_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "85656bd5201f2ce45e8c54703cd041fe231c7904",
      "size": 4145,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85656bd5201f2ce45e8c54703cd041fe231c7904"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/fcfs",
      "mode": "040000",
      "type": "tree",
      "sha": "ee5021fba123a0ab7e32a5e05ae5048deb844a0a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ee5021fba123a0ab7e32a5e05ae5048deb844a0a"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/fcfs/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ababeb0809c2c7527cba4044fd04951ebb79c4bb",
      "size": 2709,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ababeb0809c2c7527cba4044fd04951ebb79c4bb"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/fcfs/fcfs.go",
      "mode": "100644",
      "type": "blob",
      "sha": "43fc47f463bf5354c342ceb454299c2f4cb0f7cd",
      "size": 2485,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/43fc47f463bf5354c342ceb454299c2f4cb0f7cd"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/fcfs/fcfs_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fec9e238a7594d4c0f4ec3f38c9ff37c9039027a",
      "size": 2044,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fec9e238a7594d4c0f4ec3f38c9ff37c9039027a"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/functional_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "debf05d044b744f23ac7e4ea81afd7203e6d1a66",
      "size": 2766,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/debf05d044b744f23ac7e4ea81afd7203e6d1a66"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/slodeadline",
      "mode": "040000",
      "type": "tree",
      "sha": "f6a2f6719fad157b30a95cdfd455e69b9bdaafb4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f6a2f6719fad157b30a95cdfd455e69b9bdaafb4"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/slodeadline/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "6878404cdfaeb132dbb437d95f6f0b9d8fe8d045",
      "size": 2969,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6878404cdfaeb132dbb437d95f6f0b9d8fe8d045"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/slodeadline/slo_deadline.go",
      "mode": "100644",
      "type": "blob",
      "sha": "229962fd9e8f527fdcffa79602fee7c3fd15c3cb",
      "size": 4232,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/229962fd9e8f527fdcffa79602fee7c3fd15c3cb"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/ordering/slodeadline/slo_deadline_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "71ce0f993b6be7cfdff06b5ada7d177493a3120c",
      "size": 7347,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/71ce0f993b6be7cfdff06b5ada7d177493a3120c"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector",
      "mode": "040000",
      "type": "tree",
      "sha": "faa6a7eeceadbd3f0132e7142b538737a83e643a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/faa6a7eeceadbd3f0132e7142b538737a83e643a"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/concurrency",
      "mode": "040000",
      "type": "tree",
      "sha": "8add43cc63534419cc103135bd6dbebb354a8b09",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8add43cc63534419cc103135bd6dbebb354a8b09"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/concurrency/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5513590831af96af60c7b69d9418acd7592ece2f",
      "size": 4509,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5513590831af96af60c7b69d9418acd7592ece2f"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/concurrency/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "26a19943f4a440cbddabdff84e3205cf5ea93428",
      "size": 6969,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/26a19943f4a440cbddabdff84e3205cf5ea93428"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/concurrency/detector.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9217a9a3f54742be48854703f416717b9188c447",
      "size": 7013,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9217a9a3f54742be48854703f416717b9188c447"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/concurrency/detector_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "89aaf28c83a9ef97bdec43d7f631dfdc0b1c5d56",
      "size": 30272,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/89aaf28c83a9ef97bdec43d7f631dfdc0b1c5d56"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/utilization",
      "mode": "040000",
      "type": "tree",
      "sha": "e4ebf7ef08cd77e6060a39a3ae96f5822c6f1fb4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/e4ebf7ef08cd77e6060a39a3ae96f5822c6f1fb4"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/utilization/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "3de87a0330080782da57b501ba41d44519a03e44",
      "size": 6678,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3de87a0330080782da57b501ba41d44519a03e44"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/utilization/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2792cbd6238e912070f1a30b980754a5704c98a9",
      "size": 8408,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2792cbd6238e912070f1a30b980754a5704c98a9"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/utilization/detector.go",
      "mode": "100644",
      "type": "blob",
      "sha": "043e47724adcd7a7c594e44d06555f0981e0ce48",
      "size": 8345,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/043e47724adcd7a7c594e44d06555f0981e0ce48"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/saturationdetector/utilization/detector_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bbb6b1248e83892ae68d1b630b9846939a30ba1e",
      "size": 16859,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bbb6b1248e83892ae68d1b630b9846939a30ba1e"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits",
      "mode": "040000",
      "type": "tree",
      "sha": "8296da5290ab8906580f6b44d033341d6e666c3c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8296da5290ab8906580f6b44d033341d6e666c3c"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "416ef7cf907172f4b44648751c8f560025b1c692",
      "size": 1142,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/416ef7cf907172f4b44648751c8f560025b1c692"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/priorityholdback",
      "mode": "040000",
      "type": "tree",
      "sha": "414dc2fea1e149f22502cfd041d718de13a53803",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/414dc2fea1e149f22502cfd041d718de13a53803"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/priorityholdback/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "8e2a323e05a349724c99c504ec4a3ff16060a42c",
      "size": 3100,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8e2a323e05a349724c99c504ec4a3ff16060a42c"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/priorityholdback/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "45ab3816bc24b23341206391c44aa93b648d0030",
      "size": 5346,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/45ab3816bc24b23341206391c44aa93b648d0030"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/priorityholdback/priority_holdback.go",
      "mode": "100644",
      "type": "blob",
      "sha": "74fa246550c5544546a906702e3bbe5f6a81c08e",
      "size": 4846,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/74fa246550c5544546a906702e3bbe5f6a81c08e"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/priorityholdback/priority_holdback_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "241a14655c70b2d679b8135becc09fe4b7312cd0",
      "size": 17189,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/241a14655c70b2d679b8135becc09fe4b7312cd0"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/softreflectiveceiling",
      "mode": "040000",
      "type": "tree",
      "sha": "c8ea746b63f0b31c46847ab6952a2708beacf68d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c8ea746b63f0b31c46847ab6952a2708beacf68d"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/softreflectiveceiling/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "b0727df91ba6e69ec65c7e3185d707274e8dea38",
      "size": 5655,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b0727df91ba6e69ec65c7e3185d707274e8dea38"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/softreflectiveceiling/policy.go",
      "mode": "100644",
      "type": "blob",
      "sha": "435851bdf4b518b97fd302d00823058c1f4df1fc",
      "size": 5609,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/435851bdf4b518b97fd302d00823058c1f4df1fc"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/softreflectiveceiling/policy_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0d44791dffd86113288c68b2e7bf6636af19cd0e",
      "size": 8698,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0d44791dffd86113288c68b2e7bf6636af19cd0e"
    },
    {
      "path": "pkg/epp/framework/plugins/flowcontrol/usagelimits/usagelimitpolicy.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ea7da6bdd1e793558b49f887e34d3a14f40d628d",
      "size": 3087,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ea7da6bdd1e793558b49f887e34d3a14f40d628d"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol",
      "mode": "040000",
      "type": "tree",
      "sha": "2879d70645fbbfcf8e481624723d3336838ea329",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2879d70645fbbfcf8e481624723d3336838ea329"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter",
      "mode": "040000",
      "type": "tree",
      "sha": "70fe5bfeeaa6fd0b9b25d72c2b61e653e7d707f8",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/70fe5bfeeaa6fd0b9b25d72c2b61e653e7d707f8"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter/latencyslo",
      "mode": "040000",
      "type": "tree",
      "sha": "fe2e187e423c395a034ba0969c2e116e3279feaf",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/fe2e187e423c395a034ba0969c2e116e3279feaf"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter/latencyslo/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "536c8b8708c916c483e78882a103edcdaf5c0097",
      "size": 1478,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/536c8b8708c916c483e78882a103edcdaf5c0097"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter/latencyslo/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3d851a53875a766549080ccc351768b452ef6985",
      "size": 5767,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3d851a53875a766549080ccc351768b452ef6985"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter/latencyslo/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "44d7fb0b18b4d26b18ade4482be021cb0594b04e",
      "size": 7255,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/44d7fb0b18b4d26b18ade4482be021cb0594b04e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter/probabilisticadmitter",
      "mode": "040000",
      "type": "tree",
      "sha": "854ccd97d23ed8f073235d5ed2a1b844e50e98b3",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/854ccd97d23ed8f073235d5ed2a1b844e50e98b3"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter/probabilisticadmitter/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "d5d9c4ba7ec2a19f225df93f712923c7da3750fa",
      "size": 11242,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d5d9c4ba7ec2a19f225df93f712923c7da3750fa"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter/probabilisticadmitter/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0237c3381b3fe4912423779b3a8d591d832ae12c",
      "size": 5524,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0237c3381b3fe4912423779b3a8d591d832ae12c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/admitter/probabilisticadmitter/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "73e650d7653b7cf22f048088a28cd107a4e92584",
      "size": 9084,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/73e650d7653b7cf22f048088a28cd107a4e92584"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fc7caa591ca6ef28dd94ce4905bbee57e705e538",
      "size": 878,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fc7caa591ca6ef28dd94ce4905bbee57e705e538"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer",
      "mode": "040000",
      "type": "tree",
      "sha": "bb4fb15b35ba0ee8ca0747422b8a67fb2599c4bc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/bb4fb15b35ba0ee8ca0747422b8a67fb2599c4bc"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "f7ac2d0ed04a5ea8f5518400a9a438d3e8307ec1",
      "size": 5394,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f7ac2d0ed04a5ea8f5518400a9a438d3e8307ec1"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix",
      "mode": "040000",
      "type": "tree",
      "sha": "2d755ae48a5db1262fa227d7f308f9bc8e665346",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2d755ae48a5db1262fa227d7f308f9bc8e665346"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "6c64820fde418dedf512c0a89881432351ec9929",
      "size": 4086,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6c64820fde418dedf512c0a89881432351ec9929"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/constants",
      "mode": "040000",
      "type": "tree",
      "sha": "10c1164900c4df227cde5c1235b7735c36f48711",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/10c1164900c4df227cde5c1235b7735c36f48711"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/constants/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d34e0fc9236cbb8796344b64f932db638c3b9643",
      "size": 768,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d34e0fc9236cbb8796344b64f932db638c3b9643"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/indexer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bf739d096e1ca68e882853359ae5a23a2727c17b",
      "size": 5666,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf739d096e1ca68e882853359ae5a23a2727c17b"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/indexer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1969d2abe6431a1507786479ac71f5d6115b6ed0",
      "size": 6466,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1969d2abe6431a1507786479ac71f5d6115b6ed0"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ae59eb2edef51ac6dd6a8c5186b203007961fd47",
      "size": 5094,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ae59eb2edef51ac6dd6a8c5186b203007961fd47"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "107f836294529242ad45490f0415f2dec101b0ae",
      "size": 2924,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/107f836294529242ad45490f0415f2dec101b0ae"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e077c872abf2b3047d7c60e37c8c26fac97df13e",
      "size": 1887,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e077c872abf2b3047d7c60e37c8c26fac97df13e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/multicluster_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ecf9c3acc2aa91fd0782d8264634d5bdc7b72e03",
      "size": 1403,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ecf9c3acc2aa91fd0782d8264634d5bdc7b72e03"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "63da4e26340d9a510be442c100f83b4006ecf84c",
      "size": 15490,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/63da4e26340d9a510be442c100f83b4006ecf84c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2228a58af733f8cde92f7c575e662491ae6a5738",
      "size": 37213,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2228a58af733f8cde92f7c575e662491ae6a5738"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f688a3dd51a5af939dfb9646c7bfdf6d4e9ec885",
      "size": 7469,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f688a3dd51a5af939dfb9646c7bfdf6d4e9ec885"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix",
      "mode": "040000",
      "type": "tree",
      "sha": "6b16f2ccfcead198a1a1642f0d1ee116551dadfd",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6b16f2ccfcead198a1a1642f0d1ee116551dadfd"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ed46927ed91a240948b0916d78d81b53e7451012",
      "size": 4260,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ed46927ed91a240948b0916d78d81b53e7451012"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix/assign.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3bc714c5d4bdfaa6097460d7dda6a7b739ce92a3",
      "size": 12253,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3bc714c5d4bdfaa6097460d7dda6a7b739ce92a3"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix/assign_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1a4cf0ab9885bd1962650173724a5c608faa7196",
      "size": 14008,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1a4cf0ab9885bd1962650173724a5c608faa7196"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "aed91a7e757794e3f07d5966bda2d8b607750a05",
      "size": 8774,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/aed91a7e757794e3f07d5966bda2d8b607750a05"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "22dcd2db1ce4e0b7463f12d60a0439fbab3ac901",
      "size": 6687,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/22dcd2db1ce4e0b7463f12d60a0439fbab3ac901"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix/types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "701582fb86e710a858808e40a238b6e820e61ef8",
      "size": 5455,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/701582fb86e710a858808e40a238b6e820e61ef8"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/image.png",
      "mode": "100644",
      "type": "blob",
      "sha": "ceb615000a57316fb2048232558ffb5c130eea30",
      "size": 124711,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ceb615000a57316fb2048232558ffb5c130eea30"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload",
      "mode": "040000",
      "type": "tree",
      "sha": "4a361ae414d77e011ab7d80a63d1a337f7a7b23e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4a361ae414d77e011ab7d80a63d1a337f7a7b23e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "422212801b92a1d4c90ef4f5db50314e39d5b60d",
      "size": 4150,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/422212801b92a1d4c90ef4f5db50314e39d5b60d"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/constants",
      "mode": "040000",
      "type": "tree",
      "sha": "18c67cfbbbb58ffa09ee10c30d3ed4eef0ea948e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/18c67cfbbbb58ffa09ee10c30d3ed4eef0ea948e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/constants/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "410339c7e5a1974c623e3bd1ceab2b7d2139aeb5",
      "size": 748,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/410339c7e5a1974c623e3bd1ceab2b7d2139aeb5"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "490366eb62f2aba7588f3f667abd6c317393fabf",
      "size": 3001,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/490366eb62f2aba7588f3f667abd6c317393fabf"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "95d2f60e9f5601d2bdf812179a19c2c441769963",
      "size": 16017,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/95d2f60e9f5601d2bdf812179a19c2c441769963"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/producer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fcdfbf327f571be57e560a853c0c809d005a8b43",
      "size": 34184,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fcdfbf327f571be57e560a853c0c809d005a8b43"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/producer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7cdb3d45ab1331e764b0ca09b8ffedd4d6b8abc1",
      "size": 63669,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7cdb3d45ab1331e764b0ca09b8ffedd4d6b8abc1"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/token_estimator.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0c916fdb5d14aae54f42cb7f9221ec655fd6be9a",
      "size": 5756,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0c916fdb5d14aae54f42cb7f9221ec655fd6be9a"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/token_estimator_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "44ef837b755cca285138c114358702bd0fc79d4d",
      "size": 5841,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/44ef837b755cca285138c114358702bd0fc79d4d"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver",
      "mode": "040000",
      "type": "tree",
      "sha": "64bf40a3e19ce65459cce2dd71d3801fceceefa8",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/64bf40a3e19ce65459cce2dd71d3801fceceefa8"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "13191bff45b4513cfb7af0227664a825b44c9ca4",
      "size": 9684,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/13191bff45b4513cfb7af0227664a825b44c9ca4"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/constants",
      "mode": "040000",
      "type": "tree",
      "sha": "a156bca10504e7cfd1a471c308f1ba065f397098",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a156bca10504e7cfd1a471c308f1ba065f397098"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/constants/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "83fad41da4d9a741aee589f4848b1706435ddf6d",
      "size": 851,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/83fad41da4d9a741aee589f4848b1706435ddf6d"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/flush.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4268c4f7df06f9766e8423608e8818860692d9c1",
      "size": 4748,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4268c4f7df06f9766e8423608e8818860692d9c1"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/flush_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bb5dc70a741a7960f48656c8e5261447d493075c",
      "size": 5221,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bb5dc70a741a7960f48656c8e5261447d493075c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/hooks.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f0d0651f0a301ee2cc27a9c38ae7ddfb6fb8682e",
      "size": 7428,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f0d0651f0a301ee2cc27a9c38ae7ddfb6fb8682e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/hooks_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0e3090aeb5e08a47cd25bc4744c19e4f2ac3770f",
      "size": 7105,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0e3090aeb5e08a47cd25bc4744c19e4f2ac3770f"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b3c8b7abecb847e5990e5cee111590cd58a84631",
      "size": 15302,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b3c8b7abecb847e5990e5cee111590cd58a84631"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c747af81a7e3494c656ad8881a18794ba7d95eba",
      "size": 8825,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c747af81a7e3494c656ad8881a18794ba7d95eba"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/tracker.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2d74b1e396c3a57b5c61ad3f879e89e4b4177147",
      "size": 3804,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2d74b1e396c3a57b5c61ad3f879e89e4b4177147"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/tracker_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f1d8bfdca3bb203db38692c166776e729c66bcee",
      "size": 3676,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f1d8bfdca3bb203db38692c166776e729c66bcee"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal",
      "mode": "040000",
      "type": "tree",
      "sha": "2b73a41c5a934373f09de02f6dbc1cdcfb9ab50f",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2b73a41c5a934373f09de02f6dbc1cdcfb9ab50f"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "bf8da1299486c7cc178746b9a61ea3f36f541687",
      "size": 4882,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf8da1299486c7cc178746b9a61ea3f36f541687"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal/export_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "92cc5e842fab87ee98d97b0ce0f07f2864b9aed6",
      "size": 1321,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/92cc5e842fab87ee98d97b0ce0f07f2864b9aed6"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "98f9a8eea2fcfc08cc3f0c4279459d19a478e94f",
      "size": 3093,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/98f9a8eea2fcfc08cc3f0c4279459d19a478e94f"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a1c74c98c9729e2b76ea21f2da3f312defa42e97",
      "size": 4236,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a1c74c98c9729e2b76ea21f2da3f312defa42e97"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal/prerequest.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6b99338ab675f14eb378259f7769a61473e04b49",
      "size": 2472,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6b99338ab675f14eb378259f7769a61473e04b49"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal/producer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9d03f7e3b2fc816ccf19b8f75c025d45a383c163",
      "size": 13208,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9d03f7e3b2fc816ccf19b8f75c025d45a383c163"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal/producer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6f2dd35ff6b90241e3b08362caa62da57b03f67c",
      "size": 14091,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6f2dd35ff6b90241e3b08362caa62da57b03f67c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/p2psource",
      "mode": "040000",
      "type": "tree",
      "sha": "a10f477da9e40aaf3e79089d873acb5e6ac63c70",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a10f477da9e40aaf3e79089d873acb5e6ac63c70"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/p2psource/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "b12729e9665e0646ab258dfaddf4ce970090c0a8",
      "size": 3179,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b12729e9665e0646ab258dfaddf4ce970090c0a8"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/p2psource/producer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8c258ee24a2c883169bfbd90751d89e75dd5ca72",
      "size": 12996,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8c258ee24a2c883169bfbd90751d89e75dd5ca72"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/p2psource/producer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d889938f1f0c446fb5b3ba99855d51e1c5b1be61",
      "size": 22807,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d889938f1f0c446fb5b3ba99855d51e1c5b1be61"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache",
      "mode": "040000",
      "type": "tree",
      "sha": "d83f1b749cf1fee495f1e5ffef07847be129e820",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d83f1b749cf1fee495f1e5ffef07847be129e820"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "85f356d52440d9ed8b664eb75003930313eeb758",
      "size": 3518,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85f356d52440d9ed8b664eb75003930313eeb758"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/blockkeys.go",
      "mode": "100644",
      "type": "blob",
      "sha": "18f7cf145c3ce9351ba16f7d34e10b4025065041",
      "size": 5246,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/18f7cf145c3ce9351ba16f7d34e10b4025065041"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/blockkeys_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b8229dcde018d1443f2690e52eb99f53cd22bc81",
      "size": 3134,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b8229dcde018d1443f2690e52eb99f53cd22bc81"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6df6a09acc21628363f7a5f72ae6c4e81c4e36e6",
      "size": 744,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6df6a09acc21628363f7a5f72ae6c4e81c4e36e6"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "72acc10279b393c809a1a1d657fb50c9da7c90dd",
      "size": 3718,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/72acc10279b393c809a1a1d657fb50c9da7c90dd"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/extractor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bb2f44091411a150d5835c2b7527a6c7cee60f91",
      "size": 11099,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bb2f44091411a150d5835c2b7527a6c7cee60f91"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/prerequest.go",
      "mode": "100644",
      "type": "blob",
      "sha": "347fa520f6c5e8ac548f4a3d7bd8c31d9bb689d6",
      "size": 6500,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/347fa520f6c5e8ac548f4a3d7bd8c31d9bb689d6"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/prerequest_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "20ac6aa6bae7dec94fb3ae51d70d9dede30fa162",
      "size": 6439,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/20ac6aa6bae7dec94fb3ae51d70d9dede30fa162"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/producer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "98d1dc86b2442d5b2b1cbb19a0355efb4cf2ba65",
      "size": 15179,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/98d1dc86b2442d5b2b1cbb19a0355efb4cf2ba65"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/producer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "641a20ac1f429cf003281ff2d239aa5a7354132a",
      "size": 32098,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/641a20ac1f429cf003281ff2d239aa5a7354132a"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/utils.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a3c50ab39778673100ab2bf60dd23197bd933518",
      "size": 1168,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a3c50ab39778673100ab2bf60dd23197bd933518"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency",
      "mode": "040000",
      "type": "tree",
      "sha": "c3402351eac99877aa565c6607fe0807dcbe4698",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c3402351eac99877aa565c6607fe0807dcbe4698"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "a530425a8c76d461a5761e0ac344ae87259dad06",
      "size": 3582,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a530425a8c76d461a5761e0ac344ae87259dad06"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/constants",
      "mode": "040000",
      "type": "tree",
      "sha": "4f118d0865bb7818893ca35e7a1bcd1b0139ebd9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4f118d0865bb7818893ca35e7a1bcd1b0139ebd9"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/constants/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "742b7449ddb418fa57f69c51b69e4a633b1fc329",
      "size": 774,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/742b7449ddb418fa57f69c51b69e4a633b1fc329"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/dataproducer_hooks.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e3ae45c04eefcd89c9e1e00c44224dfa46c43d7a",
      "size": 7621,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e3ae45c04eefcd89c9e1e00c44224dfa46c43d7a"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/dataproducer_hooks_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dfc5422fc755ea3584d1751611a7b4756ab704ac",
      "size": 7032,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dfc5422fc755ea3584d1751611a7b4756ab704ac"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient",
      "mode": "040000",
      "type": "tree",
      "sha": "c0af51e42ad469d90bdfa1eadd070bc2deb19f97",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c0af51e42ad469d90bdfa1eadd070bc2deb19f97"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "c80afe526c925c305fb51e3b29848beacce998ba",
      "size": 2249,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c80afe526c925c305fb51e3b29848beacce998ba"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/coalescer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "765b1e3ce5cfc1916749485b33d53162d3b45e4b",
      "size": 7193,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/765b1e3ce5cfc1916749485b33d53162d3b45e4b"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/latencypredictor_async.go",
      "mode": "100644",
      "type": "blob",
      "sha": "732ef3614f98b12c753702d75aa5b0884ac026c5",
      "size": 7814,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/732ef3614f98b12c753702d75aa5b0884ac026c5"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/latencypredictor_async_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bef108690737f07c8c190ef48c6528ef5426ecb3",
      "size": 70283,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bef108690737f07c8c190ef48c6528ef5426ecb3"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "abbdc176cadbdb7d09b61900ae970ee0ee431d02",
      "size": 5918,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/abbdc176cadbdb7d09b61900ae970ee0ee431d02"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/prediction.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6f41c13a2eca1b22c179ad11abe771f562a450c5",
      "size": 12153,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6f41c13a2eca1b22c179ad11abe771f562a450c5"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/prediction_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4dbae6402b75146d5f17e908e36be1d53855645a",
      "size": 3660,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4dbae6402b75146d5f17e908e36be1d53855645a"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests",
      "mode": "040000",
      "type": "tree",
      "sha": "cfa10c626b9e2582a31de4f18cda31ce89ee05df",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/cfa10c626b9e2582a31de4f18cda31ce89ee05df"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests/Dockerfile",
      "mode": "100644",
      "type": "blob",
      "sha": "39774f8cf21fc3c3a911259d84661d96b98ab858",
      "size": 592,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/39774f8cf21fc3c3a911259d84661d96b98ab858"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "e8dbe21869a0bc7dc5fa63b884b6f175817751d9",
      "size": 4277,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e8dbe21869a0bc7dc5fa63b884b6f175817751d9"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests/main.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ba00d49ef61f856732ad903641053210ef89700d",
      "size": 17149,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ba00d49ef61f856732ad903641053210ef89700d"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests/manifests",
      "mode": "040000",
      "type": "tree",
      "sha": "ee1eb66266facdb4158ec5279070eea41a0274e1",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ee1eb66266facdb4158ec5279070eea41a0274e1"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests/manifests/predictor_warmup.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "06f15935b84a91782e71c5257686af2663fb6d70",
      "size": 1396,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/06f15935b84a91782e71c5257686af2663fb6d70"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests/run_qps_sweep.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "ea707b31422cdfc1ffb02654290659556d063718",
      "size": 5296,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ea707b31422cdfc1ffb02654290659556d063718"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/training.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8379f7756bf07de9919eeb5ab94f8a41a37fe46c",
      "size": 13402,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8379f7756bf07de9919eeb5ab94f8a41a37fe46c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f12d1b09d6c244aa11a3ac0de52aa145c4685b7c",
      "size": 11384,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f12d1b09d6c244aa11a3ac0de52aa145c4685b7c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "004301c655148f4e5ae5d6236ccff6b4b8afcc5a",
      "size": 16316,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/004301c655148f4e5ae5d6236ccff6b4b8afcc5a"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1b5d7de9c8594b48d81ebf642d7695d19a181103",
      "size": 6805,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1b5d7de9c8594b48d81ebf642d7695d19a181103"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2e055512f1e098b44ed414515f5aa89b4cc60189",
      "size": 21493,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2e055512f1e098b44ed414515f5aa89b4cc60189"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f335e6937ddcc6a9c03a0f3920bdb6e1a9a09164",
      "size": 19137,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f335e6937ddcc6a9c03a0f3920bdb6e1a9a09164"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/prediction.go",
      "mode": "100644",
      "type": "blob",
      "sha": "aa3a8c30d20bc3ddebdfd7290f7533714045db35",
      "size": 7922,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/aa3a8c30d20bc3ddebdfd7290f7533714045db35"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/prediction_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8b429780cb46f7df62dc34ade2cb03e625823970",
      "size": 8167,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8b429780cb46f7df62dc34ade2cb03e625823970"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/requestcontrol_hooks.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2fee4dd3497b77921e0863429575e241ff24bb7a",
      "size": 15232,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2fee4dd3497b77921e0863429575e241ff24bb7a"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/requestcontrol_hooks_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6e6871542c57538fc26bb8cf1c898c1f738f2a9e",
      "size": 41562,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6e6871542c57538fc26bb8cf1c898c1f738f2a9e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/running_request_tpot_slo_queue.go",
      "mode": "100644",
      "type": "blob",
      "sha": "26db43ff78175a3012351e7ed9580d10298d35de",
      "size": 6672,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/26db43ff78175a3012351e7ed9580d10298d35de"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/running_request_tpot_slo_queue_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8cae5bbbb3c85a9dcdf0c8ae4e3a7434dd55ff6c",
      "size": 8899,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8cae5bbbb3c85a9dcdf0c8ae4e3a7434dd55ff6c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/training.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1d22327fab98ef02c80345419accfd4d3209b5c1",
      "size": 10682,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1d22327fab98ef02c80345419accfd4d3209b5c1"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/training_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2a48062153d83e5b8934dce93a7d6a01abe2a837",
      "size": 11047,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2a48062153d83e5b8934dce93a7d6a01abe2a837"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/prefixhash",
      "mode": "040000",
      "type": "tree",
      "sha": "b9281c5723214d56b019f99280be315a981aaf0a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b9281c5723214d56b019f99280be315a981aaf0a"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/prefixhash/hashing.go",
      "mode": "100644",
      "type": "blob",
      "sha": "62c1335ba8251a72823120bdb439d68632cdca4c",
      "size": 4550,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/62c1335ba8251a72823120bdb439d68632cdca4c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/prefixhash/hashing_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "eabde7e5dfa3fb4a75e4a2ac9a45521615f11079",
      "size": 8152,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eabde7e5dfa3fb4a75e4a2ac9a45521615f11079"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/sessionid",
      "mode": "040000",
      "type": "tree",
      "sha": "a243614b90d7b7d997a8e5b2be966b71a8fb991c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a243614b90d7b7d997a8e5b2be966b71a8fb991c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/sessionid/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ab39071c5695bfb8b594b4fc8978d7bf46640057",
      "size": 1175,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ab39071c5695bfb8b594b4fc8978d7bf46640057"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/sessionid/constants",
      "mode": "040000",
      "type": "tree",
      "sha": "332efd602f92cae12e00ab3ea60f0f826d39d6f9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/332efd602f92cae12e00ab3ea60f0f826d39d6f9"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/sessionid/constants/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "504a5f9092206e05d3334236ab2e017c7f1354a8",
      "size": 733,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/504a5f9092206e05d3334236ab2e017c7f1354a8"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/sessionid/producer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "85e6ab04898f2bdb2cfd4ebbbcef4c1e40039dc2",
      "size": 5210,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85e6ab04898f2bdb2cfd4ebbbcef4c1e40039dc2"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/sessionid/producer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1cdebaef215e8bf0ed143310a168d2183ae523f7",
      "size": 6240,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1cdebaef215e8bf0ed143310a168d2183ae523f7"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer",
      "mode": "040000",
      "type": "tree",
      "sha": "f7ffb5b5e8bbf01906a8d8dc9c25c75e8bc71e9b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f7ffb5b5e8bbf01906a8d8dc9c25c75e8bc71e9b"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "69fd615c806627dd137c56f3c14bcbd9be88758d",
      "size": 12698,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/69fd615c806627dd137c56f3c14bcbd9be88758d"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/backend.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0c05ba2bb1ee0cc654c28202c2bd31b803306afe",
      "size": 8938,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0c05ba2bb1ee0cc654c28202c2bd31b803306afe"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/estimate.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9a3036022c3f0a999b31dfb062b9d5687419ae71",
      "size": 12453,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9a3036022c3f0a999b31dfb062b9d5687419ae71"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/estimate_live_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d66e45f9e96280fce0839a664dccdf8882448f7e",
      "size": 9295,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d66e45f9e96280fce0839a664dccdf8882448f7e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/estimate_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8b8f885e8f7ae59dd2c1c3ca0453028f74a5d76b",
      "size": 39770,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8b8f885e8f7ae59dd2c1c3ca0453028f74a5d76b"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/mm_estimate.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f2910347793a6f04b1f28a9ad2698f8652ba66ce",
      "size": 10617,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f2910347793a6f04b1f28a9ad2698f8652ba66ce"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/pythonjson.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7785adc0985159db6d84f69abf9b4d9fb8c240d8",
      "size": 4060,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7785adc0985159db6d84f69abf9b4d9fb8c240d8"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/tokenizer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4c0b7bbf3665fab49af165cb6c32ca6409cc1f04",
      "size": 28109,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4c0b7bbf3665fab49af165cb6c32ca6409cc1f04"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/tokenizer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a11f2f44f492fdc6136464711e29325b13b75013",
      "size": 41290,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a11f2f44f492fdc6136464711e29325b13b75013"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/tokenizer_tracing_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8078ecff7b1fdab4c81c982777f59f0ae0e06a7e",
      "size": 6533,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8078ecff7b1fdab4c81c982777f59f0ae0e06a7e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/vllm_http.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dfbe923e75358f2c9cfcf834a67adae8f33cdd1c",
      "size": 17265,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dfbe923e75358f2c9cfcf834a67adae8f33cdd1c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer/vllm_http_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4b4818c6e6698739c2bbd2c85278db3dac3c9f95",
      "size": 26990,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4b4818c6e6698739c2bbd2c85278db3dac3c9f95"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestattributereporter",
      "mode": "040000",
      "type": "tree",
      "sha": "20d1e4c8f16244956d7bb4e23b2c34c867558d76",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/20d1e4c8f16244956d7bb4e23b2c34c867558d76"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestattributereporter/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5c7fa9bf5e6a66024b7aad4b02c5afba37b7359a",
      "size": 3420,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5c7fa9bf5e6a66024b7aad4b02c5afba37b7359a"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestattributereporter/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0a3555996392237b76f00c41f1f145b2238ad0a1",
      "size": 8672,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0a3555996392237b76f00c41f1f145b2238ad0a1"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestattributereporter/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7660d773c69cacfde6962752a838de6cf5e4cfbc",
      "size": 9476,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7660d773c69cacfde6962752a838de6cf5e4cfbc"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader",
      "mode": "040000",
      "type": "tree",
      "sha": "40c1ac91dee14ccca51814edf2d3ba08eb244664",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/40c1ac91dee14ccca51814edf2d3ba08eb244664"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader/agentidentity",
      "mode": "040000",
      "type": "tree",
      "sha": "46b89a9079d8b3ca58f359ea04ce8aa68e10a026",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/46b89a9079d8b3ca58f359ea04ce8aa68e10a026"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader/agentidentity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1db6effe19a6eb51d62f75d7af149fef794ae45f",
      "size": 7871,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1db6effe19a6eb51d62f75d7af149fef794ae45f"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader/agentidentity/agent_identity.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4dc28ab8a7a0f2d65116bb71318ce046ad2c7948",
      "size": 4656,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4dc28ab8a7a0f2d65116bb71318ce046ad2c7948"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader/agentidentity/agent_identity_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1831dc22bea92a638da8f370a2ac440ee054da5f",
      "size": 7997,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1831dc22bea92a638da8f370a2ac440ee054da5f"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader/outlenbucket",
      "mode": "040000",
      "type": "tree",
      "sha": "cd6ee8721801322b603fc1f091469454a890c3b2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/cd6ee8721801322b603fc1f091469454a890c3b2"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader/outlenbucket/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "7fab397b535732629c938097ac36cae171722c1c",
      "size": 5667,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7fab397b535732629c938097ac36cae171722c1c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader/outlenbucket/outlenbucket.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dfd865cf765827cc53834dcb947a1e2ab9ca2c38",
      "size": 7572,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dfd865cf765827cc53834dcb947a1e2ab9ca2c38"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/requestheader/outlenbucket/outlenbucket_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a5004a08f36bc02a0207d700140c7dfc0d7ed75b",
      "size": 6865,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a5004a08f36bc02a0207d700140c7dfc0d7ed75b"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener",
      "mode": "040000",
      "type": "tree",
      "sha": "075505fb17903888c4edaf864d495e9d6e00f672",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/075505fb17903888c4edaf864d495e9d6e00f672"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout",
      "mode": "040000",
      "type": "tree",
      "sha": "4840910f168030346aa953d8771f8161cd9d2c5d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4840910f168030346aa953d8771f8161cd9d2c5d"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "69170da8975a321a5ec047aeaa47135e93d08d8b",
      "size": 16920,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/69170da8975a321a5ec047aeaa47135e93d08d8b"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout/config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2a43c365c70aa976b66483642e2dba710032b299",
      "size": 6394,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2a43c365c70aa976b66483642e2dba710032b299"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout/config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6f9f673f3835aa098fd89d9f3861ea14ea008a49",
      "size": 7370,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6f9f673f3835aa098fd89d9f3861ea14ea008a49"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "33bfc46dca9648073d6e44dd010dbc99b63fe1f4",
      "size": 2640,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/33bfc46dca9648073d6e44dd010dbc99b63fe1f4"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "27f52f2d17fce839da9c50755fd3305873962324",
      "size": 3132,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/27f52f2d17fce839da9c50755fd3305873962324"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2d6f4ed112eca5c9b136c4f9bdc67fb3da24ae81",
      "size": 9888,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2d6f4ed112eca5c9b136c4f9bdc67fb3da24ae81"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a1a5f4ebbf5a546e3c5bd3bb6e9556744cb19901",
      "size": 25893,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a1a5f4ebbf5a546e3c5bd3bb6e9556744cb19901"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout/screener.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2fb62faaac875b20842c1319cdbd3187e523a081",
      "size": 6777,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2fb62faaac875b20842c1319cdbd3187e523a081"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/test",
      "mode": "040000",
      "type": "tree",
      "sha": "051d26b542498e6d208b227faf73f9e81495a81e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/051d26b542498e6d208b227faf73f9e81495a81e"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/test/consts.go",
      "mode": "100644",
      "type": "blob",
      "sha": "194ef070e0bf5b05b051a476409d73cfa4d2434c",
      "size": 746,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/194ef070e0bf5b05b051a476409d73cfa4d2434c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/test/responsereceived",
      "mode": "040000",
      "type": "tree",
      "sha": "4ccbe76bcf83e14e789f939136acb8d2d58d589c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4ccbe76bcf83e14e789f939136acb8d2d58d589c"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/test/responsereceived/destination_endpoint_served_verifier.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6c33fe9ece52c6acced947b53fb90395bad09f00",
      "size": 4047,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6c33fe9ece52c6acced947b53fb90395bad09f00"
    },
    {
      "path": "pkg/epp/framework/plugins/requestcontrol/test/responsereceived/destination_endpoint_served_verifier_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5412339855378013dffc1a6b0307f62b96539ec3",
      "size": 2711,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5412339855378013dffc1a6b0307f62b96539ec3"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling",
      "mode": "040000",
      "type": "tree",
      "sha": "1b1ad398e778acfd1832e0dbb60c599d7b3767ac",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1b1ad398e778acfd1832e0dbb60c599d7b3767ac"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers",
      "mode": "040000",
      "type": "tree",
      "sha": "0cc985773b3c116ed07966243694a41b6357b164",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0cc985773b3c116ed07966243694a41b6357b164"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "eec6191fb0d0a2d2c7dc1a1d975bbccebf3d2652",
      "size": 5234,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eec6191fb0d0a2d2c7dc1a1d975bbccebf3d2652"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/anthropic",
      "mode": "040000",
      "type": "tree",
      "sha": "6fb8fe7239f75741bad5290a2389ebfd539aee34",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6fb8fe7239f75741bad5290a2389ebfd539aee34"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/anthropic/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "f5da51e790dfe3edc7c4bf76d81d93865a3fd6c7",
      "size": 667,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f5da51e790dfe3edc7c4bf76d81d93865a3fd6c7"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/anthropic/anthropic.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fe2a1ebbed6843068100a4c94055a9bf5a5ffdbc",
      "size": 7282,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fe2a1ebbed6843068100a4c94055a9bf5a5ffdbc"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/anthropic/anthropic_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9db6686911ff6d476d5f82d4b3138550bdeec8e9",
      "size": 23383,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9db6686911ff6d476d5f82d4b3138550bdeec8e9"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/common.go",
      "mode": "100644",
      "type": "blob",
      "sha": "07bd9e9ef45da0b9adbfd60cf21f048d255a3589",
      "size": 681,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/07bd9e9ef45da0b9adbfd60cf21f048d255a3589"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/openai",
      "mode": "040000",
      "type": "tree",
      "sha": "779f5b32201c87aaa9d7671d85448e1c5b64e56c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/779f5b32201c87aaa9d7671d85448e1c5b64e56c"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/openai/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "cc4a9224ede07dc3fd94497818cccd1e863c30bd",
      "size": 754,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cc4a9224ede07dc3fd94497818cccd1e863c30bd"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/openai/openai.go",
      "mode": "100644",
      "type": "blob",
      "sha": "504237697b665400cfb6ecb5f9067febd9b82728",
      "size": 21479,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/504237697b665400cfb6ecb5f9067febd9b82728"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/openai/openai_extractusage_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7fd2f573ed752de81dae536ddd817718cda08772",
      "size": 1987,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7fd2f573ed752de81dae536ddd817718cda08772"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/openai/openai_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "997c5b8cdbf75979ad19201cdb335099bde9ac09",
      "size": 61240,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/997c5b8cdbf75979ad19201cdb335099bde9ac09"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/passthrough",
      "mode": "040000",
      "type": "tree",
      "sha": "5405939b31abaf160b28a01d1444116a9f3419ca",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5405939b31abaf160b28a01d1444116a9f3419ca"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/passthrough/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ba11ae430974231869da4cdf64c4809e18e40f1c",
      "size": 594,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ba11ae430974231869da4cdf64c4809e18e40f1c"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/passthrough/passthrough.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2b9f8558b5febb9a546d4e44e116f27f14b20fc7",
      "size": 2497,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2b9f8558b5febb9a546d4e44e116f27f14b20fc7"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/passthrough/passthrough_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5f5e3e3c01a92489dd1d3a7ef6fcccf78cc7b21b",
      "size": 2531,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5f5e3e3c01a92489dd1d3a7ef6fcccf78cc7b21b"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/sglanghttp",
      "mode": "040000",
      "type": "tree",
      "sha": "98bac3a336bdae4d804de5da9727a412c869072b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/98bac3a336bdae4d804de5da9727a412c869072b"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/sglanghttp/sglanghttp.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f0ee9751d09ff7bb3c51928b2a0da1df6eb725d6",
      "size": 8578,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f0ee9751d09ff7bb3c51928b2a0da1df6eb725d6"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/sglanghttp/sglanghttp_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "19c058b7a884537b079b9fcccf84228425963fa9",
      "size": 12038,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/19c058b7a884537b079b9fcccf84228425963fa9"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/util",
      "mode": "040000",
      "type": "tree",
      "sha": "0319cff20414ab76f52490ef2a074124b6d3ad93",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0319cff20414ab76f52490ef2a074124b6d3ad93"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/util/grpc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a5457e95caca98df81922abc6bc7c8d187ed5e35",
      "size": 1860,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a5457e95caca98df81922abc6bc7c8d187ed5e35"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/util/grpc_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b7596673ef753ce9ae8f23ba1a6cbd0c1f5a2dd7",
      "size": 2600,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b7596673ef753ce9ae8f23ba1a6cbd0c1f5a2dd7"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/util/json.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c86a06860c4dcf6d9f73dc4df8fffc96f385abf9",
      "size": 1980,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c86a06860c4dcf6d9f73dc4df8fffc96f385abf9"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/util/json_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4abd9ede67f91fabbd96a3a4a8124dc245dbd8d0",
      "size": 3654,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4abd9ede67f91fabbd96a3a4a8124dc245dbd8d0"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vertexai",
      "mode": "040000",
      "type": "tree",
      "sha": "3a6f3d80592c78ba912e6064165099d01c732978",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/3a6f3d80592c78ba912e6064165099d01c732978"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vertexai/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5f09c6349f3c1c0eb96a97623e3708edae55e3b2",
      "size": 2334,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5f09c6349f3c1c0eb96a97623e3708edae55e3b2"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vertexai/vertexai.go",
      "mode": "100644",
      "type": "blob",
      "sha": "81ae0df0e34956cc1f0391977aac8669e600110f",
      "size": 7604,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/81ae0df0e34956cc1f0391977aac8669e600110f"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vertexai/vertexai_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "00591b642355d3335e65c48d5d6141df342aa2e4",
      "size": 9236,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/00591b642355d3335e65c48d5d6141df342aa2e4"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc",
      "mode": "040000",
      "type": "tree",
      "sha": "44753fe079a3e192eaf3b18a54ade37aa7ba22b7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/44753fe079a3e192eaf3b18a54ade37aa7ba22b7"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "b8d210d89b0347272bc4c17859e78feb085f9de6",
      "size": 456,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b8d210d89b0347272bc4c17859e78feb085f9de6"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/api",
      "mode": "040000",
      "type": "tree",
      "sha": "74daa4108cc477a007718f622f2a1ae3faf38208",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/74daa4108cc477a007718f622f2a1ae3faf38208"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/api/gen",
      "mode": "040000",
      "type": "tree",
      "sha": "05acc52e89686c66a9555fc084c211a8246e79cb",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/05acc52e89686c66a9555fc084c211a8246e79cb"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/api/gen/vllm_engine.pb.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fb5a524cc9293732c3af8121d57f8dcc147284c3",
      "size": 83230,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fb5a524cc9293732c3af8121d57f8dcc147284c3"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/api/gen/vllm_engine_grpc.pb.go",
      "mode": "100644",
      "type": "blob",
      "sha": "540f0271091b2d41c768bde45938cfafe1dc6b30",
      "size": 13359,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/540f0271091b2d41c768bde45938cfafe1dc6b30"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/api/proto",
      "mode": "040000",
      "type": "tree",
      "sha": "deb1b00abcea9cb2db364295d296dfe7476fca3c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/deb1b00abcea9cb2db364295d296dfe7476fca3c"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/api/proto/vllm_engine.proto",
      "mode": "100644",
      "type": "blob",
      "sha": "55b8662ef8e29256e51342bfa7693a9c88386e1b",
      "size": 6453,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/55b8662ef8e29256e51342bfa7693a9c88386e1b"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/vllmgrpc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "585e5a2e05af213b47eb077d3a7a3b04ecdbd1cc",
      "size": 9951,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/585e5a2e05af213b47eb077d3a7a3b04ecdbd1cc"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/vllmgrpc_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9d754a3bfe3f16d96f08536c0046492b755d49a3",
      "size": 16234,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9d754a3bfe3f16d96f08536c0046492b755d49a3"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmhttp",
      "mode": "040000",
      "type": "tree",
      "sha": "017feba1f9fb65e1e46a5826a4288f53478c829d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/017feba1f9fb65e1e46a5826a4288f53478c829d"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmhttp/vllmhttp.go",
      "mode": "100644",
      "type": "blob",
      "sha": "668ce5f2b33fb3b9a98f658c262d313a12816d41",
      "size": 5218,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/668ce5f2b33fb3b9a98f658c262d313a12816d41"
    },
    {
      "path": "pkg/epp/framework/plugins/requesthandling/parsers/vllmhttp/vllmhttp_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3ab761b7f04b672c97b7dd42029976a5cc5a966c",
      "size": 11843,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3ab761b7f04b672c97b7dd42029976a5cc5a966c"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling",
      "mode": "040000",
      "type": "tree",
      "sha": "9e8a302d4982ccfcc1c02705df234017a48b8571",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/9e8a302d4982ccfcc1c02705df234017a48b8571"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "044023138afcc73c0a4686e040461fbe30cacfa1",
      "size": 858,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/044023138afcc73c0a4686e040461fbe30cacfa1"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter",
      "mode": "040000",
      "type": "tree",
      "sha": "1f66cd6d33aeaf95279787467842d1c793db6370",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1f66cd6d33aeaf95279787467842d1c793db6370"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel",
      "mode": "040000",
      "type": "tree",
      "sha": "12c9a3b0c88c895f9b4b211e080fef96e1639a9f",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/12c9a3b0c88c895f9b4b211e080fef96e1639a9f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "e9c4b599e4b84c1945eb9485025714603e2060b7",
      "size": 4681,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e9c4b599e4b84c1945eb9485025714603e2060b7"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ebd92d73ce13831a650413c052be8e74e0f76e90",
      "size": 72,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ebd92d73ce13831a650413c052be8e74e0f76e90"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel/filter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ea201346e2a79a4e6eca482fd14502c291635a71",
      "size": 4256,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ea201346e2a79a4e6eca482fd14502c291635a71"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel/filter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c2a91ac17689995cc3604c153f3c0a33e62c4a2f",
      "size": 6951,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c2a91ac17689995cc3604c153f3c0a33e62c4a2f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel/roles.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fcff03fa889a4dc0f436e2ffc6ae225b22be24d3",
      "size": 2598,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fcff03fa889a4dc0f436e2ffc6ae225b22be24d3"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel/roles_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7a876ae21350eaa831a3aab0d5179e45a55d40e0",
      "size": 5982,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a876ae21350eaa831a3aab0d5179e45a55d40e0"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel/selector.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fb9602fed24be5090b991a3ad8538c93625b80fe",
      "size": 3302,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fb9602fed24be5090b991a3ad8538c93625b80fe"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/bylabel/selector_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "314f7464400ae401c4892bdb5aa3c95f49de56fd",
      "size": 13605,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/314f7464400ae401c4892bdb5aa3c95f49de56fd"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/endpointattribute",
      "mode": "040000",
      "type": "tree",
      "sha": "10c9649b31831cff2a63b179ce3f340760fced79",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/10c9649b31831cff2a63b179ce3f340760fced79"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/endpointattribute/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "674025c44553f0c3aae644554024dde9d9c14b1d",
      "size": 3601,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/674025c44553f0c3aae644554024dde9d9c14b1d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/endpointattribute/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "607a882fd9fabcebc92fb3cf2c3057d39d8e00a2",
      "size": 746,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/607a882fd9fabcebc92fb3cf2c3057d39d8e00a2"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/endpointattribute/filter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "db5b927730005082d97278cb0619cf9fbaff572b",
      "size": 8252,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/db5b927730005082d97278cb0619cf9fbaff572b"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/endpointattribute/filter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f3f2bd9110aeade21c17aeb6a3860d020987c051",
      "size": 10252,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f3f2bd9110aeade21c17aeb6a3860d020987c051"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "8c6ffeedd78b8c91116f822d106c43e5e7dfb77a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8c6ffeedd78b8c91116f822d106c43e5e7dfb77a"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "d17990cb9db2603474e5306a7e9dfb84a075e55f",
      "size": 8653,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d17990cb9db2603474e5306a7e9dfb84a075e55f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2bf880ac7fdb70898999a9bc114355d1c98ad6e1",
      "size": 3004,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2bf880ac7fdb70898999a9bc114355d1c98ad6e1"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "58085b5c543f893c1d51d9973134b30c0c66300a",
      "size": 3682,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/58085b5c543f893c1d51d9973134b30c0c66300a"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "06f3f64e96e487910d7bf37d06e845d918b48b0f",
      "size": 12429,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/06f3f64e96e487910d7bf37d06e845d918b48b0f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e43bf0d7a952b1f72675e81012186f9227f5e7dc",
      "size": 13752,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e43bf0d7a952b1f72675e81012186f9227f5e7dc"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "375b7aed7e7f2275c89d94deeab0e4758cc09e91",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/375b7aed7e7f2275c89d94deeab0e4758cc09e91"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "f3e4852dde6dd460b42adbdb7a70335d07c502dc",
      "size": 5834,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f3e4852dde6dd460b42adbdb7a70335d07c502dc"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "11c78c2777268e01c872c72a7c956b857b753c16",
      "size": 677,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/11c78c2777268e01c872c72a7c956b857b753c16"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/encoded_endpoint_header.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b94316cb853da45817c6a8dd00743018497ea865",
      "size": 2508,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b94316cb853da45817c6a8dd00743018497ea865"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "635ee7c370d319b92827e27885eb90b7ea720054",
      "size": 1839,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/635ee7c370d319b92827e27885eb90b7ea720054"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/multicluster_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "241ce04b6dfe8f1633b6786813fd08ed5079b294",
      "size": 1313,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/241ce04b6dfe8f1633b6786813fd08ed5079b294"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/session_affinity.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c6c34333057c09f0cb9a2c4b0f819c494b41502e",
      "size": 6544,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c6c34333057c09f0cb9a2c4b0f819c494b41502e"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/session_affinity_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9be6171f69636cf2b3c2ff06eabc4688b56452f7",
      "size": 12120,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9be6171f69636cf2b3c2ff06eabc4688b56452f7"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/session_id_header.go",
      "mode": "100644",
      "type": "blob",
      "sha": "168508f041c93026de9fb4c105bf941d8b61c3c8",
      "size": 2825,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/168508f041c93026de9fb4c105bf941d8b61c3c8"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sessionaffinity/session_id_header_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "280fbf73457a5c12f2dfc4a3a0a6604897242788",
      "size": 6822,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/280fbf73457a5c12f2dfc4a3a0a6604897242788"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sloheadroomtier",
      "mode": "040000",
      "type": "tree",
      "sha": "14f284fca9c3e497cac78aa1b44a95bf7691c6cc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/14f284fca9c3e497cac78aa1b44a95bf7691c6cc"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sloheadroomtier/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "37919c9a2bdc416e1081503be3a851120a339e4f",
      "size": 1542,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/37919c9a2bdc416e1081503be3a851120a339e4f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sloheadroomtier/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "113f44c1925ed21c2596b283aeb89ccdab8ed664",
      "size": 5328,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/113f44c1925ed21c2596b283aeb89ccdab8ed664"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/sloheadroomtier/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "604bc383cbf82847a3a7f7585f3a6fb5b86aadf8",
      "size": 4917,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/604bc383cbf82847a3a7f7585f3a6fb5b86aadf8"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/topologyaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "17a74a59d6698d2f4622b541589034f142496e62",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/17a74a59d6698d2f4622b541589034f142496e62"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/topologyaffinity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "f5d2b3aa0b9dd7af77de0b3cd38dff37e6e6b6c9",
      "size": 3449,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f5d2b3aa0b9dd7af77de0b3cd38dff37e6e6b6c9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/topologyaffinity/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7fc0c7c9450dc241004bb0fff13b323bcbff8f84",
      "size": 771,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7fc0c7c9450dc241004bb0fff13b323bcbff8f84"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/topologyaffinity/filter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "24c053ab703ed01791477115d6dfdad98e08b244",
      "size": 4358,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/24c053ab703ed01791477115d6dfdad98e08b244"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/topologyaffinity/filter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "18f2a041043d366224d21e4bac21307af887f74a",
      "size": 5925,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/18f2a041043d366224d21e4bac21307af887f74a"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/utilization",
      "mode": "040000",
      "type": "tree",
      "sha": "8fe11cfcfcc5c9afcc1df7c85def85107d2717d0",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8fe11cfcfcc5c9afcc1df7c85def85107d2717d0"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/utilization/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "2988016a814983af3e4a94c4056903f9fe4eb221",
      "size": 3671,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2988016a814983af3e4a94c4056903f9fe4eb221"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/utilization/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3ac2cbd1fadc4661934f6e0da6ce06a3440b0f24",
      "size": 140,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3ac2cbd1fadc4661934f6e0da6ce06a3440b0f24"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/utilization/filter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8cd30887ec4596185feedbf180b2e71ed7c81894",
      "size": 8706,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8cd30887ec4596185feedbf180b2e71ed7c81894"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/filter/utilization/filter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d43981f378df501ee7b5c1f1943b081fbcd99705",
      "size": 10687,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d43981f378df501ee7b5c1f1943b081fbcd99705"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker",
      "mode": "040000",
      "type": "tree",
      "sha": "7de8f806dee1fbf25e0a74c0259af9b91dfa3ba2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7de8f806dee1fbf25e0a74c0259af9b91dfa3ba2"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5053ebf2fcfc6c9b34b5ce02a9e6ee86a6de505c",
      "size": 972,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5053ebf2fcfc6c9b34b5ce02a9e6ee86a6de505c"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/common.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0af1ab2c8c58161e96e15003816ebccffacf40ff",
      "size": 3432,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0af1ab2c8c58161e96e15003816ebccffacf40ff"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/maxscore",
      "mode": "040000",
      "type": "tree",
      "sha": "c1635bc2a8b087723a08b2e0234aa254ff1b607c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c1635bc2a8b087723a08b2e0234aa254ff1b607c"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/maxscore/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "873f51c64e2400323dc83c261e79cd95a0703f7f",
      "size": 1362,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/873f51c64e2400323dc83c261e79cd95a0703f7f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/maxscore/picker.go",
      "mode": "100644",
      "type": "blob",
      "sha": "581c66ac488a9f74b0ea0349a7f51af49bd365e9",
      "size": 4725,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/581c66ac488a9f74b0ea0349a7f51af49bd365e9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/maxscore/picker_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5e0568e5d7507410b9f771ead8de89abbb97ff4c",
      "size": 12271,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5e0568e5d7507410b9f771ead8de89abbb97ff4c"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/random",
      "mode": "040000",
      "type": "tree",
      "sha": "03eb134fcfed49b67cc625c7577a4ab51caf2691",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/03eb134fcfed49b67cc625c7577a4ab51caf2691"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/random/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "9645e1c44f59ac615780e17efe9e3654404109ef",
      "size": 1087,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9645e1c44f59ac615780e17efe9e3654404109ef"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/random/picker.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5677c65a070d104af7949237a1e7f7cf2c26e9fb",
      "size": 3836,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5677c65a070d104af7949237a1e7f7cf2c26e9fb"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/random/picker_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "250b7010f7318eeb0c4b835017a060b3b4e75686",
      "size": 3766,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/250b7010f7318eeb0c4b835017a060b3b4e75686"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/weightedrandom",
      "mode": "040000",
      "type": "tree",
      "sha": "98759f45ad49754ac7cc7ac3810c6a30ed181e50",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/98759f45ad49754ac7cc7ac3810c6a30ed181e50"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/weightedrandom/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "6350a14beabbd0556511e48a8cf68317f1380ae3",
      "size": 1546,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6350a14beabbd0556511e48a8cf68317f1380ae3"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/weightedrandom/picker.go",
      "mode": "100644",
      "type": "blob",
      "sha": "59d98173531711144785a8d074819e16c6815847",
      "size": 6407,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/59d98173531711144785a8d074819e16c6815847"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/picker/weightedrandom/picker_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f1dc5f105fd85febf7e2d15cec921b01b3b7966d",
      "size": 5024,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f1dc5f105fd85febf7e2d15cec921b01b3b7966d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler",
      "mode": "040000",
      "type": "tree",
      "sha": "7510354eff091387d62353a4a683f4a2d2821633",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7510354eff091387d62353a4a683f4a2d2821633"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/dataparallel",
      "mode": "040000",
      "type": "tree",
      "sha": "1fd11f7d5d340d62d7f17f085914e72750396ba9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1fd11f7d5d340d62d7f17f085914e72750396ba9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/dataparallel/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "9eeeed9a49a2c06a24b8f8ac88c34cddd2d2e81c",
      "size": 1149,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9eeeed9a49a2c06a24b8f8ac88c34cddd2d2e81c"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/dataparallel/dp_profile_handler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "22e39c550e0eb1b758e912c04081df63d02dc12b",
      "size": 5177,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/22e39c550e0eb1b758e912c04081df63d02dc12b"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/dataparallel/dp_profile_handler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0499fa135b01a39272e481818e98704a6dfd9987",
      "size": 10794,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0499fa135b01a39272e481818e98704a6dfd9987"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg",
      "mode": "040000",
      "type": "tree",
      "sha": "8f75d873d24b0f235708165a7c9f0cd50d8d2645",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8f75d873d24b0f235708165a7c9f0cd50d8d2645"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "cd5352566c4a7f078e5c788e9d9db7a4a14aa695",
      "size": 10731,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cd5352566c4a7f078e5c788e9d9db7a4a14aa695"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/always_disagg_mm_decider.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1b10626c99ded2cacd19465bcfddd7b3862fc531",
      "size": 1708,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1b10626c99ded2cacd19465bcfddd7b3862fc531"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/always_disagg_pd_decider.go",
      "mode": "100644",
      "type": "blob",
      "sha": "29c27587ac681c4824d00416c9242db9fab97a0d",
      "size": 1508,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/29c27587ac681c4824d00416c9242db9fab97a0d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/decider_plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a93670e627c9196f066d1c4a92f99ac59e223111",
      "size": 812,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a93670e627c9196f066d1c4a92f99ac59e223111"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/disagg_headers_handler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "68cba1327081916eb8141dd0448d623d00a7b141",
      "size": 6608,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/68cba1327081916eb8141dd0448d623d00a7b141"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/disagg_headers_handler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a820c181cbc23bfd76ad31e54173442944f62db9",
      "size": 16895,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a820c181cbc23bfd76ad31e54173442944f62db9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/disagg_profile_handler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "841068acbef474aa3667d7c1168693be900d79ec",
      "size": 23582,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/841068acbef474aa3667d7c1168693be900d79ec"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/disagg_profile_handler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "220e9fb572aaeb817da33bf84010acc01f49e070",
      "size": 64825,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/220e9fb572aaeb817da33bf84010acc01f49e070"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8190fbe51ec0978c5d3869f944e671d1390b93fd",
      "size": 4210,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8190fbe51ec0978c5d3869f944e671d1390b93fd"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ae701f7a4aa8748d67d2feafbaa9c5596c4be157",
      "size": 5226,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ae701f7a4aa8748d67d2feafbaa9c5596c4be157"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/multimodal_helpers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bdeadd4c1cbe58507fe98884e64c5a3574d14f98",
      "size": 628,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bdeadd4c1cbe58507fe98884e64c5a3574d14f98"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/prefix_based_pd_decider.go",
      "mode": "100644",
      "type": "blob",
      "sha": "795d638346083e3869803aae07b9b46c13492d50",
      "size": 12278,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/795d638346083e3869803aae07b9b46c13492d50"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/prefix_based_pd_decider_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0961f61e3f5d237b3401e77d603bf1b21da0e682",
      "size": 29177,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0961f61e3f5d237b3401e77d603bf1b21da0e682"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/disagg/scheduler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ecaff23abd6daa6dd65a1f8533e983d3f33ccbdb",
      "size": 13482,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ecaff23abd6daa6dd65a1f8533e983d3f33ccbdb"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/headerprofile",
      "mode": "040000",
      "type": "tree",
      "sha": "ce12b4114601cc122819cdf3d18e42ab75580e1d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ce12b4114601cc122819cdf3d18e42ab75580e1d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/headerprofile/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "e2ee2e2b00567a216a43dd1cc69115855f4e08a9",
      "size": 5057,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e2ee2e2b00567a216a43dd1cc69115855f4e08a9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/headerprofile/header_profile_handler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e3cfea65ef0eaf93dd44df2bb9f2c5e5fbaaf3c6",
      "size": 8568,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e3cfea65ef0eaf93dd44df2bb9f2c5e5fbaaf3c6"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/headerprofile/header_profile_handler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ada5128fea583f2d1aa29fec6d02c74852fef39b",
      "size": 20501,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ada5128fea583f2d1aa29fec6d02c74852fef39b"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/single",
      "mode": "040000",
      "type": "tree",
      "sha": "36d80340ad2ba850ed2a10358a550b0949c2b484",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/36d80340ad2ba850ed2a10358a550b0949c2b484"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/single/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ca3c15dd497df88beb5a6a5d4e443d28041ef469",
      "size": 507,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ca3c15dd497df88beb5a6a5d4e443d28041ef469"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/single/single_profile_handler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1eea1f8332fe8f20f4c00ed95a0bfe4b79bef576",
      "size": 3750,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1eea1f8332fe8f20f4c00ed95a0bfe4b79bef576"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/profilehandler/single/single_profile_handler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e0e9a82eda2aa26e3a9d92c0395fd70c3b3f13be",
      "size": 5271,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e0e9a82eda2aa26e3a9d92c0395fd70c3b3f13be"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer",
      "mode": "040000",
      "type": "tree",
      "sha": "24cfa96f32fc338b338c23add0536ecc700b154e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/24cfa96f32fc338b338c23add0536ecc700b154e"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/activerequest",
      "mode": "040000",
      "type": "tree",
      "sha": "6557aa8cf1c4c6b72d0a55a7018d7856e190bcb7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6557aa8cf1c4c6b72d0a55a7018d7856e190bcb7"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/activerequest/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5023b2435244bdb351e43c5ab0940053e3569ecb",
      "size": 1080,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5023b2435244bdb351e43c5ab0940053e3569ecb"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/activerequest/active_request.go",
      "mode": "100644",
      "type": "blob",
      "sha": "271c7cd66c93314bef721bdd3734fe499002eddf",
      "size": 7315,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/271c7cd66c93314bef721bdd3734fe499002eddf"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/activerequest/active_request_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4f73fb57c908faa2ce11477e8557da90b1f9a4a1",
      "size": 9405,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4f73fb57c908faa2ce11477e8557da90b1f9a4a1"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/activerequest/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d03110db3fbbb534714301fc9f90b1ef1a825ccf",
      "size": 101,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d03110db3fbbb534714301fc9f90b1ef1a825ccf"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/contextlengthaware",
      "mode": "040000",
      "type": "tree",
      "sha": "d3331d44f54fd02082a172ebdecd231117bfbe83",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d3331d44f54fd02082a172ebdecd231117bfbe83"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/contextlengthaware/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "beb4aa805389efdafd1ab272bf0c87f966392629",
      "size": 3424,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/beb4aa805389efdafd1ab272bf0c87f966392629"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/contextlengthaware/context_length_aware.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6b9bd3f329c6d7b5c973b42dc7f0b17fadda763b",
      "size": 10559,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6b9bd3f329c6d7b5c973b42dc7f0b17fadda763b"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/contextlengthaware/context_length_aware_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b6a1fe23c2abbd27dff76afb39bdb82988ad0df2",
      "size": 10057,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b6a1fe23c2abbd27dff76afb39bdb82988ad0df2"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/contextlengthaware/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a8483638b7770db0f1969bb52b61530a3c39d638",
      "size": 161,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a8483638b7770db0f1969bb52b61530a3c39d638"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/endpointattribute",
      "mode": "040000",
      "type": "tree",
      "sha": "b220480f7d744f4b332733e1cc17ea86bb2a63fc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b220480f7d744f4b332733e1cc17ea86bb2a63fc"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/endpointattribute/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ee56c838f3efc63f18195c0288c030e23c445744",
      "size": 3764,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ee56c838f3efc63f18195c0288c030e23c445744"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/endpointattribute/endpointattribute.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9e9bb21fb9272cfa698ef6f5b936f00cde802051",
      "size": 8692,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9e9bb21fb9272cfa698ef6f5b936f00cde802051"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/endpointattribute/endpointattribute_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "03da09bb4078c01bce6804b7b95b9e9ee6437a2e",
      "size": 9626,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/03da09bb4078c01bce6804b7b95b9e9ee6437a2e"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/endpointattribute/legacy_parity_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4fbe951b803719e61510d6ebfd1fb4e6c8cc0c94",
      "size": 5874,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4fbe951b803719e61510d6ebfd1fb4e6c8cc0c94"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/headerlabelaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "4b67a706f0de69b70be692e9ef72f92643a6ffe4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4b67a706f0de69b70be692e9ef72f92643a6ffe4"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/headerlabelaffinity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "9648741db8390e86af6255863833751dc19acb82",
      "size": 3824,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9648741db8390e86af6255863833751dc19acb82"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/headerlabelaffinity/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "cbc9a69b04789e824b984bc89759fd419e936898",
      "size": 4191,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cbc9a69b04789e824b984bc89759fd419e936898"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/headerlabelaffinity/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a80e1b1bd4a80330dc22278e4064ffc59cb00e60",
      "size": 5639,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a80e1b1bd4a80330dc22278e4064ffc59cb00e60"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization",
      "mode": "040000",
      "type": "tree",
      "sha": "b65c8750a490bcf8264fabdc4474b66612643a8c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b65c8750a490bcf8264fabdc4474b66612643a8c"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "25bdc9b0c43ade81031ebab6a06dfaedc542fc58",
      "size": 1511,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/25bdc9b0c43ade81031ebab6a06dfaedc542fc58"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/kvcache_utilization.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0a8ce207eaab7a807f33249958136c2e7ad988ab",
      "size": 3340,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0a8ce207eaab7a807f33249958136c2e7ad988ab"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/kvcache_utilization_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b7e371787744cfca387709bf4cea0432a9b0b502",
      "size": 3409,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b7e371787744cfca387709bf4cea0432a9b0b502"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2b16cc1570431796ac17a6a0c32b93540f3bb7b6",
      "size": 2984,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2b16cc1570431796ac17a6a0c32b93540f3bb7b6"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/multicluster_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "22491cc5bcea3dd7cd7cb12f608d06e72bb5ba5f",
      "size": 2895,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/22491cc5bcea3dd7cd7cb12f608d06e72bb5ba5f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/latency",
      "mode": "040000",
      "type": "tree",
      "sha": "836817a175926c8d478a16fe4d078b24ad65fb84",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/836817a175926c8d478a16fe4d078b24ad65fb84"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/latency/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "2eb5ba60bf614d5c8593cc832c25dafec2f18ec0",
      "size": 4003,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2eb5ba60bf614d5c8593cc832c25dafec2f18ec0"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/latency/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6b3217e2ef8489f47e344e289df2f38acbbb7e29",
      "size": 13609,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6b3217e2ef8489f47e344e289df2f38acbbb7e29"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/latency/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c5f11e7f61e6bfb69e67a8e826ccd08a2d684890",
      "size": 7192,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c5f11e7f61e6bfb69e67a8e826ccd08a2d684890"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/latencyobservation",
      "mode": "040000",
      "type": "tree",
      "sha": "8428fd583cf7cc16953fa29a777d3ea428ddfb9d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/8428fd583cf7cc16953fa29a777d3ea428ddfb9d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/latencyobservation/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "bd2cd2e4aaa4b054c02b5d661839d52dca484bef",
      "size": 9386,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bd2cd2e4aaa4b054c02b5d661839d52dca484bef"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/latencyobservation/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9bef792728c5db2eb35bf8597d2bc66ac997c210",
      "size": 11964,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9bef792728c5db2eb35bf8597d2bc66ac997c210"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/latencyobservation/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "557dd3e3adb4248b850f0e13cf879ff44abe17a0",
      "size": 11229,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/557dd3e3adb4248b850f0e13cf879ff44abe17a0"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loadaware",
      "mode": "040000",
      "type": "tree",
      "sha": "b5480a09278d50862a7358fbfa144f72156cb7e0",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b5480a09278d50862a7358fbfa144f72156cb7e0"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loadaware/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1806f9f7189a19c260e5dc50c6d5c57e9119a7bd",
      "size": 2695,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1806f9f7189a19c260e5dc50c6d5c57e9119a7bd"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loadaware/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "37eeb7470a9ab0dba08a9242a8456ec59451a0af",
      "size": 88,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/37eeb7470a9ab0dba08a9242a8456ec59451a0af"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loadaware/load_aware.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dc1edf8d48bc1905acc8305d1a0cb6b3e65677c9",
      "size": 3360,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dc1edf8d48bc1905acc8305d1a0cb6b3e65677c9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loadaware/load_aware_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a753319b85fb5c8ed4bb9fafaa958ecc04ba1c8e",
      "size": 1824,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a753319b85fb5c8ed4bb9fafaa958ecc04ba1c8e"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loraaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "c5bc36e7bf326c32994ec22100446f16d9a36bc2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c5bc36e7bf326c32994ec22100446f16d9a36bc2"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loraaffinity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "9572cb274555fafd5f511eae59bfab7cc7cf70b1",
      "size": 1355,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9572cb274555fafd5f511eae59bfab7cc7cf70b1"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loraaffinity/lora_affinity.go",
      "mode": "100644",
      "type": "blob",
      "sha": "af2184d40050438de32372d70b4591af7af4fb6f",
      "size": 3818,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/af2184d40050438de32372d70b4591af7af4fb6f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/loraaffinity/lora_affinity_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ce6b281b9dcd1531c7a48f63438b1b3483a2a5c4",
      "size": 7126,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ce6b281b9dcd1531c7a48f63438b1b3483a2a5c4"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/mmcacheaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "21ba9bfbee002148bbc96cf17377801225a7fcb3",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/21ba9bfbee002148bbc96cf17377801225a7fcb3"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/mmcacheaffinity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "f8b7561de1abe829709003adc044873d4c050c04",
      "size": 1761,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f8b7561de1abe829709003adc044873d4c050c04"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/mmcacheaffinity/scorer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c46a7a615e72fa0c3ad5266ff87f8b9ac2c5f795",
      "size": 4775,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c46a7a615e72fa0c3ad5266ff87f8b9ac2c5f795"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/mmcacheaffinity/scorer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "92736e9e5555929b9b4a16614f279c38068a5aa9",
      "size": 3115,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/92736e9e5555929b9b4a16614f279c38068a5aa9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/nohitlru",
      "mode": "040000",
      "type": "tree",
      "sha": "559ff4f2d2003524e0ffd3a0b548009c8713df28",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/559ff4f2d2003524e0ffd3a0b548009c8713df28"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/nohitlru/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "5ad30e8ddf5ff2bf014b16b79a09b6e00926b240",
      "size": 2117,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5ad30e8ddf5ff2bf014b16b79a09b6e00926b240"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/nohitlru/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "cedc312c9ce069071babe2456bbcac884f005aef",
      "size": 86,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cedc312c9ce069071babe2456bbcac884f005aef"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/nohitlru/no_hit_lru.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e3815e292f863133dc9825e26d44ef104ddfc8fd",
      "size": 13605,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e3815e292f863133dc9825e26d44ef104ddfc8fd"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/nohitlru/no_hit_lru_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c4ee76ccf0ee2079316781cc76ed6a1981c703a9",
      "size": 21325,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c4ee76ccf0ee2079316781cc76ed6a1981c703a9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/preciseprefixcache",
      "mode": "040000",
      "type": "tree",
      "sha": "a72405aed8d0257ec5635be0c0652a99e69019d7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a72405aed8d0257ec5635be0c0652a99e69019d7"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/preciseprefixcache/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "dd94d6facc1577c6708f903507f7108c636a570d",
      "size": 1907,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dd94d6facc1577c6708f903507f7108c636a570d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/preciseprefixcache/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c76c9413cf138f22a5ba38f852d57b84a829f5f8",
      "size": 116,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c76c9413cf138f22a5ba38f852d57b84a829f5f8"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/preciseprefixcache/precise_prefix_cache.go",
      "mode": "100644",
      "type": "blob",
      "sha": "57ce7195cb31efa7814de76ceb15ab2be8fc6952",
      "size": 9776,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/57ce7195cb31efa7814de76ceb15ab2be8fc6952"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/preciseprefixcache/precise_prefix_cache_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "36cefec5f346c72aafd8742c11bbe581e2d964de",
      "size": 7037,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/36cefec5f346c72aafd8742c11bbe581e2d964de"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/prefix",
      "mode": "040000",
      "type": "tree",
      "sha": "fc82d73908383f4022eb84e3cda8e7c6e3d5daa5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/fc82d73908383f4022eb84e3cda8e7c6e3d5daa5"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/prefix/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ccf74b440b3dfaf31d587e6dfcfb01291ca8ab22",
      "size": 2930,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ccf74b440b3dfaf31d587e6dfcfb01291ca8ab22"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/prefix/multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "54f2d4e5299d44dc7bbb8cc20c19a5097138bc6f",
      "size": 1889,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/54f2d4e5299d44dc7bbb8cc20c19a5097138bc6f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/prefix/multicluster_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5946943a6767f7eab3874d6b2e54fa05b087ebae",
      "size": 1291,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5946943a6767f7eab3874d6b2e54fa05b087ebae"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/prefix/plugin.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6bd81026b37272cbd4544de8706d2a6dcf9ca0c9",
      "size": 6222,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6bd81026b37272cbd4544de8706d2a6dcf9ca0c9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/prefix/plugin_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7657fc39abd62922cbcb4e3185c360a9a46e86a8",
      "size": 9237,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7657fc39abd62922cbcb4e3185c360a9a46e86a8"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/queuedepth",
      "mode": "040000",
      "type": "tree",
      "sha": "374bf7602c698872e8cacd7c167fff5263531164",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/374bf7602c698872e8cacd7c167fff5263531164"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/queuedepth/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "fab93495b102512847ee44dfab452f163678abd0",
      "size": 1599,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fab93495b102512847ee44dfab452f163678abd0"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/queuedepth/multicluster.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2288a74ab33996d90e9d79c0288c01b520a8e139",
      "size": 3254,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2288a74ab33996d90e9d79c0288c01b520a8e139"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/queuedepth/multicluster_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "39b4945723aea7fe42a0ea7dfede3c067867147d",
      "size": 3559,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/39b4945723aea7fe42a0ea7dfede3c067867147d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/queuedepth/queue.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b112b053066f8e3d8639a073a6a58158e59825b4",
      "size": 3824,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b112b053066f8e3d8639a073a6a58158e59825b4"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/queuedepth/queue_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f263012cba47add698b3c5b76df2da529ba76136",
      "size": 2802,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f263012cba47add698b3c5b76df2da529ba76136"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/runningrequests",
      "mode": "040000",
      "type": "tree",
      "sha": "bb0f80f267f2125f882563185d202ec8ce191a19",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/bb0f80f267f2125f882563185d202ec8ce191a19"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/runningrequests/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "abb081b5e10e0a1c2b9118bc853aec5b5a9ae88f",
      "size": 1235,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/abb081b5e10e0a1c2b9118bc853aec5b5a9ae88f"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/runningrequests/running_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1bca8f9db82e175f7327f54d95ec8169bd03e9c8",
      "size": 2845,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1bca8f9db82e175f7327f54d95ec8169bd03e9c8"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/runningrequests/runningrequest.go",
      "mode": "100644",
      "type": "blob",
      "sha": "145acc23c05b00148f23919a062da8dae54e20c2",
      "size": 4169,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/145acc23c05b00148f23919a062da8dae54e20c2"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "3a79e7926efdc35883164703a3eeb1eab842a117",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/3a79e7926efdc35883164703a3eeb1eab842a117"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "88deeecb385407073f3434d093cb58751c8949b9",
      "size": 4990,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/88deeecb385407073f3434d093cb58751c8949b9"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6a37ed4695daf45db19667fb3a29563d78b67b32",
      "size": 677,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6a37ed4695daf45db19667fb3a29563d78b67b32"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity/encoded_endpoint_header.go",
      "mode": "100644",
      "type": "blob",
      "sha": "df648ab139faee228db0caf6d421e2ea51e26738",
      "size": 2488,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/df648ab139faee228db0caf6d421e2ea51e26738"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity/session_affinity.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1754d9e195dc13c9829a3e5d6d5c37b26de5c09c",
      "size": 6136,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1754d9e195dc13c9829a3e5d6d5c37b26de5c09c"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity/session_affinity_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "80d533754b02a7aaad73d5732e25b4fc17ec48b8",
      "size": 11731,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/80d533754b02a7aaad73d5732e25b4fc17ec48b8"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity/session_id_header.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7bc0a7d603abccc0861cc05545b535f2f9ff5dd7",
      "size": 2853,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7bc0a7d603abccc0861cc05545b535f2f9ff5dd7"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity/session_id_header_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "293e9cad76dee0a21d5bfa8f34fc26ca65708acc",
      "size": 4563,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/293e9cad76dee0a21d5bfa8f34fc26ca65708acc"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/tokenload",
      "mode": "040000",
      "type": "tree",
      "sha": "2293aa31b625a4c084a3822d95a782231d761b76",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2293aa31b625a4c084a3822d95a782231d761b76"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/tokenload/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "b1772198f7a5fde083628b1e9fff33b45918b440",
      "size": 1736,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b1772198f7a5fde083628b1e9fff33b45918b440"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/tokenload/token_load.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7b908ffc397ab76a243eca5a57a96c7818fed819",
      "size": 4460,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7b908ffc397ab76a243eca5a57a96c7818fed819"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/tokenload/token_load_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ddc8a4dc27906a7e170e8b6e18736dae331b1767",
      "size": 5940,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ddc8a4dc27906a7e170e8b6e18736dae331b1767"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/topologyaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "c4dd51bf4cb596b6ada01fa82e87ede7fc75f467",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/c4dd51bf4cb596b6ada01fa82e87ede7fc75f467"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/topologyaffinity/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "bb4ccbfff36cb488175245705aba12070316c66a",
      "size": 4622,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bb4ccbfff36cb488175245705aba12070316c66a"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/topologyaffinity/scorer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f1bd3743d24866908ea35dac6be32eaf58b95d40",
      "size": 4522,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f1bd3743d24866908ea35dac6be32eaf58b95d40"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/scorer/topologyaffinity/scorer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d6bccbb2af1fc8d4f13ba6e03fb2b44ac4545cf1",
      "size": 5028,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d6bccbb2af1fc8d4f13ba6e03fb2b44ac4545cf1"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/test",
      "mode": "040000",
      "type": "tree",
      "sha": "6a6a8c14e92fa5bb6dd29bb24cff603b8ba153e7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/6a6a8c14e92fa5bb6dd29bb24cff603b8ba153e7"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/test/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "00c05dbd75305372251ce0860d643627c59253ac",
      "size": 68,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/00c05dbd75305372251ce0860d643627c59253ac"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/test/consts.go",
      "mode": "100644",
      "type": "blob",
      "sha": "166dba2e6fae4c609c9ab2e212226a551a7cf9eb",
      "size": 1546,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/166dba2e6fae4c609c9ab2e212226a551a7cf9eb"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/test/filter",
      "mode": "040000",
      "type": "tree",
      "sha": "00b705c8ef7827f82c294cedaf4d861aa2fddacd",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/00b705c8ef7827f82c294cedaf4d861aa2fddacd"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/test/filter/filter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fca48c576d30d3fcfdce3b72ecd29314928a94f3",
      "size": 5295,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fca48c576d30d3fcfdce3b72ecd29314928a94f3"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/test/filter/request_header_based_filter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fb5da7da76af25aeb31beaed63572c2873628def",
      "size": 4402,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fb5da7da76af25aeb31beaed63572c2873628def"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util",
      "mode": "040000",
      "type": "tree",
      "sha": "2e5941e4e9a7da397481f359156a5638f5614a60",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2e5941e4e9a7da397481f359156a5638f5614a60"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/sessionaffinity",
      "mode": "040000",
      "type": "tree",
      "sha": "417b66e44e84b03e42945186fd520f252c2ca597",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/417b66e44e84b03e42945186fd520f252c2ca597"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/sessionaffinity/header.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e0a45ad0da58688d60ef82cedb69bd61f033fc2d",
      "size": 4302,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e0a45ad0da58688d60ef82cedb69bd61f033fc2d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/sessionaffinity/session_id.go",
      "mode": "100644",
      "type": "blob",
      "sha": "408b018a2561858fdad81230a23d1dbe21aa8e62",
      "size": 13423,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/408b018a2561858fdad81230a23d1dbe21aa8e62"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/sessionaffinity/session_id_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "76dcfe59bace538619e380d08477a9f45b08584e",
      "size": 19342,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/76dcfe59bace538619e380d08477a9f45b08584e"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/topology",
      "mode": "040000",
      "type": "tree",
      "sha": "f5244b50a0de99d743a139863347427e4ab93632",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f5244b50a0de99d743a139863347427e4ab93632"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/topology/level.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5671a7bfc911047b811c78750d121633f4daac7d",
      "size": 2708,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5671a7bfc911047b811c78750d121633f4daac7d"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/topology/level_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fa3663839eccdb25fc5774e95464a14652f3da32",
      "size": 2568,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fa3663839eccdb25fc5774e95464a14652f3da32"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/topology/peer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3c1ca026c06d02ac4b19185810102ff69e6ef5d7",
      "size": 1907,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3c1ca026c06d02ac4b19185810102ff69e6ef5d7"
    },
    {
      "path": "pkg/epp/framework/plugins/scheduling/util/topology/peer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "182bec31551aa0b6100ece92d0cf7690035fe22c",
      "size": 2366,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/182bec31551aa0b6100ece92d0cf7690035fe22c"
    },
    {
      "path": "pkg/epp/handlers",
      "mode": "040000",
      "type": "tree",
      "sha": "caae718f009ca8d496c2c98f612ca7b02135d3d5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/caae718f009ca8d496c2c98f612ca7b02135d3d5"
    },
    {
      "path": "pkg/epp/handlers/parsers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b7848d401e9083e7ecc252431506715c04c10e94",
      "size": 4116,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b7848d401e9083e7ecc252431506715c04c10e94"
    },
    {
      "path": "pkg/epp/handlers/parsers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4e7d0f72cc84ad7648d9adc4e354fda076b33c7d",
      "size": 7216,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4e7d0f72cc84ad7648d9adc4e354fda076b33c7d"
    },
    {
      "path": "pkg/epp/handlers/request.go",
      "mode": "100644",
      "type": "blob",
      "sha": "da1713036444688b24af6c3f6d0e8615bb87200b",
      "size": 6431,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/da1713036444688b24af6c3f6d0e8615bb87200b"
    },
    {
      "path": "pkg/epp/handlers/request_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8e4f5de34364444473dbb1d12c68bbdd97af95b4",
      "size": 12029,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8e4f5de34364444473dbb1d12c68bbdd97af95b4"
    },
    {
      "path": "pkg/epp/handlers/response.go",
      "mode": "100644",
      "type": "blob",
      "sha": "47f9dde2331d76ae16fe95326e4cee8a6d2fc297",
      "size": 9086,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/47f9dde2331d76ae16fe95326e4cee8a6d2fc297"
    },
    {
      "path": "pkg/epp/handlers/response_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "024f3669dd93dc1d802fd365c1f3c6b1526711e1",
      "size": 26912,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/024f3669dd93dc1d802fd365c1f3c6b1526711e1"
    },
    {
      "path": "pkg/epp/handlers/server.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f9aa99229199185941996a15fdb7d967e269de06",
      "size": 33486,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f9aa99229199185941996a15fdb7d967e269de06"
    },
    {
      "path": "pkg/epp/handlers/server_abort_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "03d1071e819c80bea240eea4b02c5eb3bb03f3cf",
      "size": 6425,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/03d1071e819c80bea240eea4b02c5eb3bb03f3cf"
    },
    {
      "path": "pkg/epp/handlers/server_trace_correlation_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4b51ae15c4d6c773b1633b84db50cef12546949a",
      "size": 4987,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4b51ae15c4d6c773b1633b84db50cef12546949a"
    },
    {
      "path": "pkg/epp/metadata",
      "mode": "040000",
      "type": "tree",
      "sha": "3b93ce70ffc1ccf6df9475472a0ec3e9c8eb2bd3",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/3b93ce70ffc1ccf6df9475472a0ec3e9c8eb2bd3"
    },
    {
      "path": "pkg/epp/metadata/consts.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e29e5d9176f432f22ff1e4adc91dac558935918e",
      "size": 5615,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e29e5d9176f432f22ff1e4adc91dac558935918e"
    },
    {
      "path": "pkg/epp/metadata/consts_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b9874f0a19e3b5c9ec75239984ef416528f357a2",
      "size": 1833,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b9874f0a19e3b5c9ec75239984ef416528f357a2"
    },
    {
      "path": "pkg/epp/metrics",
      "mode": "040000",
      "type": "tree",
      "sha": "d11415c1bb884123619eb36eefd63fabab4f4d9b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d11415c1bb884123619eb36eefd63fabab4f4d9b"
    },
    {
      "path": "pkg/epp/metrics/cardinality.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e38857baa2374a1420080b4468e2afb8c1ee1285",
      "size": 2281,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e38857baa2374a1420080b4468e2afb8c1ee1285"
    },
    {
      "path": "pkg/epp/metrics/cardinality_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "513571c600c8ed9ddd254e35eecf37b678904efa",
      "size": 9627,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/513571c600c8ed9ddd254e35eecf37b678904efa"
    },
    {
      "path": "pkg/epp/metrics/collectors",
      "mode": "040000",
      "type": "tree",
      "sha": "a972ef3cf786c28236bc4133d92d24453bba8be5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a972ef3cf786c28236bc4133d92d24453bba8be5"
    },
    {
      "path": "pkg/epp/metrics/collectors/inference_pool.go",
      "mode": "100644",
      "type": "blob",
      "sha": "711d0f67ecce8b9fa80f2680be08ed169dbd983e",
      "size": 2007,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/711d0f67ecce8b9fa80f2680be08ed169dbd983e"
    },
    {
      "path": "pkg/epp/metrics/collectors/inference_pool_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6163f382f6262a7213d1d520c066bb99c9b95b94",
      "size": 3592,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6163f382f6262a7213d1d520c066bb99c9b95b94"
    },
    {
      "path": "pkg/epp/metrics/grpc_stream_metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b570772bae8d75b3ea62b0833b3170357b6aedbf",
      "size": 2716,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b570772bae8d75b3ea62b0833b3170357b6aedbf"
    },
    {
      "path": "pkg/epp/metrics/grpc_stream_metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7a720f4e94ca326b98a52c4e0d5b0a456da49666",
      "size": 1568,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a720f4e94ca326b98a52c4e0d5b0a456da49666"
    },
    {
      "path": "pkg/epp/metrics/llm_d_router_metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5605acfa5c01152960eaa30478be64cf24e5d3a3",
      "size": 24371,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5605acfa5c01152960eaa30478be64cf24e5d3a3"
    },
    {
      "path": "pkg/epp/metrics/metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "66bcd5f387c11a555b12574cdd469aeffeda1f06",
      "size": 40327,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/66bcd5f387c11a555b12574cdd469aeffeda1f06"
    },
    {
      "path": "pkg/epp/metrics/metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dca18e72571e006c2d4c4b346161f6a5cf7373a9",
      "size": 45742,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dca18e72571e006c2d4c4b346161f6a5cf7373a9"
    },
    {
      "path": "pkg/epp/metrics/testdata",
      "mode": "040000",
      "type": "tree",
      "sha": "b87efc921323c5653d12d17259beec2d7d88cf61",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b87efc921323c5653d12d17259beec2d7d88cf61"
    },
    {
      "path": "pkg/epp/metrics/testdata/flow_control_dispatch_cycle_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "18669664f23fd5036b0b09357f2baa089f3c9afd",
      "size": 1417,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/18669664f23fd5036b0b09357f2baa089f3c9afd"
    },
    {
      "path": "pkg/epp/metrics/testdata/flow_control_enqueue_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "db34fde8989bfad0c61ea45f640e2c9f1f603981",
      "size": 28377,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/db34fde8989bfad0c61ea45f640e2c9f1f603981"
    },
    {
      "path": "pkg/epp/metrics/testdata/input_tokens_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "a11d79c114cea1a5847cbc58e3e67937d8b4aa84",
      "size": 6367,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a11d79c114cea1a5847cbc58e3e67937d8b4aa84"
    },
    {
      "path": "pkg/epp/metrics/testdata/kv_cache_avg_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "1b1b57d295db767c60e5595804b49300defb71a2",
      "size": 295,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1b1b57d295db767c60e5595804b49300defb71a2"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_flow_control_dispatch_cycle_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "7e96e3e80f86751a25cc8787a2375566f88f06a8",
      "size": 1194,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7e96e3e80f86751a25cc8787a2375566f88f06a8"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_flow_control_enqueue_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "ee887dfb7d9de99ecce3ece118df7417e831e154",
      "size": 26463,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ee887dfb7d9de99ecce3ece118df7417e831e154"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_flow_control_pending_reclaim_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "ed87a92ca6697967492e3ff3911e9ff5253e5241",
      "size": 261,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ed87a92ca6697967492e3ff3911e9ff5253e5241"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_flow_control_reclaim_target_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "92f75401f20bcce211e42577759d6b2873095948",
      "size": 237,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/92f75401f20bcce211e42577759d6b2873095948"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_flow_control_revocations_issued_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "1c32b2bcbf7bf07d74a9f74d2d3d139ac499b1bd",
      "size": 313,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1c32b2bcbf7bf07d74a9f74d2d3d139ac499b1bd"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_flow_control_revocations_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "04ee53dc8542d71ffa837c8507a078378706943c",
      "size": 454,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/04ee53dc8542d71ffa837c8507a078378706943c"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_input_tokens_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "f31dbc0800679ceb07ccb745795c27d0aefeb622",
      "size": 7995,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f31dbc0800679ceb07ccb745795c27d0aefeb622"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_kv_cache_avg_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "046a1db686f48cc9845ff8c4e813cd770e3eabd0",
      "size": 223,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/046a1db686f48cc9845ff8c4e813cd770e3eabd0"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_kv_cache_std_dev_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "58654670fbdff973b579f3a5c6c0cd4eef0e811a",
      "size": 234,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/58654670fbdff973b579f3a5c6c0cd4eef0e811a"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_normalized_time_per_output_token_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "ec9218413721e3b8526396acdd5a9b64c0915c28",
      "size": 6313,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ec9218413721e3b8526396acdd5a9b64c0915c28"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_output_tokens_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "d97aef929bc91c0dddd7c67ccf294a9b11e5bbbd",
      "size": 5475,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d97aef929bc91c0dddd7c67ccf294a9b11e5bbbd"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_plugin_data_scope_violations_total_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "82878264051806667323ff34a2dd6b29d8f8e7a9",
      "size": 876,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/82878264051806667323ff34a2dd6b29d8f8e7a9"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_plugin_processing_latencies_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "cbc22fcf3859d24a08e2b09c19f0988bdc3bb21f",
      "size": 13795,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cbc22fcf3859d24a08e2b09c19f0988bdc3bb21f"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_prefix_indexer_hit_bytes_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "f77f3778c81633530ddd69f77fdf5b2cb385d3c8",
      "size": 1070,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f77f3778c81633530ddd69f77fdf5b2cb385d3c8"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_prefix_indexer_hit_ratio_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "64dbf6741bf557a7af3ec8d3107daab744914fb5",
      "size": 908,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/64dbf6741bf557a7af3ec8d3107daab744914fb5"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_prefix_indexer_size_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "bf3f96d8130ff652ef06f9d6f24967d15b604ab8",
      "size": 151,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf3f96d8130ff652ef06f9d6f24967d15b604ab8"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_queue_avg_size_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "dc027dba4d6c7b534a7a14e6a6df6f0db4ee7d88",
      "size": 196,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dc027dba4d6c7b534a7a14e6a6df6f0db4ee7d88"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_queue_std_dev_size_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "63fde3be91e13f579e0d7b58ab3ee143da0a4622",
      "size": 207,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/63fde3be91e13f579e0d7b58ab3ee143da0a4622"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_request_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "79e7824aec543a537e480e4e3721986fa28970e3",
      "size": 14093,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/79e7824aec543a537e480e4e3721986fa28970e3"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_request_error_total_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "8777710eaaf9fa80a91bb908d1097adecb92409b",
      "size": 511,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8777710eaaf9fa80a91bb908d1097adecb92409b"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_request_processing_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "1053aa0d579fcef5e35892575e3c83263f17218b",
      "size": 1500,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1053aa0d579fcef5e35892575e3c83263f17218b"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_request_size_bytes_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "5707592b318deacfcfd6ddc7a83af43c0155af2b",
      "size": 10971,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5707592b318deacfcfd6ddc7a83af43c0155af2b"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_request_total_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "c8191ec0d916106f718fc0d32e4a3b2ec3149e23",
      "size": 426,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c8191ec0d916106f718fc0d32e4a3b2ec3149e23"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_request_tpot_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "beee5027178fa741760e90d3b7d1e597b5acec7d",
      "size": 7682,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/beee5027178fa741760e90d3b7d1e597b5acec7d"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_request_ttft_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "31549072740e966a2e364a9c99b2951db8bdbab8",
      "size": 11245,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/31549072740e966a2e364a9c99b2951db8bdbab8"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_response_processing_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "e5bfbdeecf50d38abcf31335df9e448846ec5e0f",
      "size": 1512,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e5bfbdeecf50d38abcf31335df9e448846ec5e0f"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_response_size_bytes_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "b1ee1ebf19c59d31532e70477dbb77bc7a140a0f",
      "size": 6468,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b1ee1ebf19c59d31532e70477dbb77bc7a140a0f"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_running_requests_avg_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "e027878d53263323a91fd6415e74996403fcd52e",
      "size": 221,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e027878d53263323a91fd6415e74996403fcd52e"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_running_requests_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "6d47a991322744aa0ea1111bea686a02caa830b6",
      "size": 333,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6d47a991322744aa0ea1111bea686a02caa830b6"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_running_requests_std_dev_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "61752190be40cbc95b5bf655b0677e08ef5e6daf",
      "size": 232,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/61752190be40cbc95b5bf655b0677e08ef5e6daf"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_scheduler_attempts_different_models_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "49784dcb1bd2261ec3c1db11b48741afacb4beb7",
      "size": 403,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/49784dcb1bd2261ec3c1db11b48741afacb4beb7"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_scheduler_attempts_multiple_endpoints_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "ef24d1af3eab1b04b8b28ca1a90ba186009bc64d",
      "size": 403,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ef24d1af3eab1b04b8b28ca1a90ba186009bc64d"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_scheduler_attempts_total_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "276b49b11f9f92e01f40599565063668bac7901a",
      "size": 378,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/276b49b11f9f92e01f40599565063668bac7901a"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_scheduler_attempts_with_result_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "cc2a0d4ced6f97a2eec14e61f22cd2c9a8190a2a",
      "size": 270,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cc2a0d4ced6f97a2eec14e61f22cd2c9a8190a2a"
    },
    {
      "path": "pkg/epp/metrics/testdata/llm_d_scheduler_e2e_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "3994fdb9dfffccfcab276f4d7562f469c90c0922",
      "size": 949,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3994fdb9dfffccfcab276f4d7562f469c90c0922"
    },
    {
      "path": "pkg/epp/metrics/testdata/normalized_time_per_output_token_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "2a90e5a0587d29a7b72f33cac2393405fc952229",
      "size": 6186,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2a90e5a0587d29a7b72f33cac2393405fc952229"
    },
    {
      "path": "pkg/epp/metrics/testdata/output_tokens_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "1fdcfa3f570c52f696a3a9c24421e6523bb3f3b7",
      "size": 4394,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1fdcfa3f570c52f696a3a9c24421e6523bb3f3b7"
    },
    {
      "path": "pkg/epp/metrics/testdata/plugin_processing_latencies_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "f06de573d58958d28b951598e3e83539d587f220",
      "size": 14907,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f06de573d58958d28b951598e3e83539d587f220"
    },
    {
      "path": "pkg/epp/metrics/testdata/prefix_indexer_hit_bytes_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "2a012374c8ec17914b22aeec200d724c14ccc7c9",
      "size": 1313,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2a012374c8ec17914b22aeec200d724c14ccc7c9"
    },
    {
      "path": "pkg/epp/metrics/testdata/prefix_indexer_hit_ratio_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "33c6c15ab60d2e3e50738dbd07cf54293a533399",
      "size": 1121,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/33c6c15ab60d2e3e50738dbd07cf54293a533399"
    },
    {
      "path": "pkg/epp/metrics/testdata/prefix_indexer_size_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "55523a2bf556865e3d1411369cf56313456c7113",
      "size": 231,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/55523a2bf556865e3d1411369cf56313456c7113"
    },
    {
      "path": "pkg/epp/metrics/testdata/queue_avg_size_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "52b6abdc20589d17ed05ac58dc6ea7acb488cf18",
      "size": 258,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/52b6abdc20589d17ed05ac58dc6ea7acb488cf18"
    },
    {
      "path": "pkg/epp/metrics/testdata/request_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "a8a368ab2a7474f9f18bb830bce6c03a9cf73b9e",
      "size": 12230,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a8a368ab2a7474f9f18bb830bce6c03a9cf73b9e"
    },
    {
      "path": "pkg/epp/metrics/testdata/request_error_total_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "e152d8dd12eb915dcea9594f5189d915f2ca0458",
      "size": 587,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e152d8dd12eb915dcea9594f5189d915f2ca0458"
    },
    {
      "path": "pkg/epp/metrics/testdata/request_sizes_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "dfdaaac4fb4f002216f635b117ee3bd16d3c7d40",
      "size": 8463,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dfdaaac4fb4f002216f635b117ee3bd16d3c7d40"
    },
    {
      "path": "pkg/epp/metrics/testdata/request_total_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "61b86b7ad2692526ebf1234b78d1207e549a82d2",
      "size": 497,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/61b86b7ad2692526ebf1234b78d1207e549a82d2"
    },
    {
      "path": "pkg/epp/metrics/testdata/request_tpot_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "beee5027178fa741760e90d3b7d1e597b5acec7d",
      "size": 7682,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/beee5027178fa741760e90d3b7d1e597b5acec7d"
    },
    {
      "path": "pkg/epp/metrics/testdata/request_ttft_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "31549072740e966a2e364a9c99b2951db8bdbab8",
      "size": 11245,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/31549072740e966a2e364a9c99b2951db8bdbab8"
    },
    {
      "path": "pkg/epp/metrics/testdata/response_sizes_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "8bc6453ba65ecafc8bfd0183791b6d8308270c8f",
      "size": 5323,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8bc6453ba65ecafc8bfd0183791b6d8308270c8f"
    },
    {
      "path": "pkg/epp/metrics/testdata/running_requests_avg_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "9bf2f598714b957e27c8a97579cd4913357ab1cb",
      "size": 289,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9bf2f598714b957e27c8a97579cd4913357ab1cb"
    },
    {
      "path": "pkg/epp/metrics/testdata/running_requests_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "330b5e1d668b799cceedc6615f65a5c6905498e1",
      "size": 320,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/330b5e1d668b799cceedc6615f65a5c6905498e1"
    },
    {
      "path": "pkg/epp/metrics/testdata/scheduler_attempts_different_models_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "4476407a3ca3e0bad4cebbde1f2a06b0fd618bba",
      "size": 486,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4476407a3ca3e0bad4cebbde1f2a06b0fd618bba"
    },
    {
      "path": "pkg/epp/metrics/testdata/scheduler_attempts_multiple_endpoints_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "85db91f846b54543b4c69066722185cde76c41fb",
      "size": 486,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85db91f846b54543b4c69066722185cde76c41fb"
    },
    {
      "path": "pkg/epp/metrics/testdata/scheduler_attempts_total_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "0181fc47ee92891f7ef9fde6cb370f56c3753c0d",
      "size": 461,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0181fc47ee92891f7ef9fde6cb370f56c3753c0d"
    },
    {
      "path": "pkg/epp/metrics/testdata/scheduler_attempts_with_result_metrics",
      "mode": "100644",
      "type": "blob",
      "sha": "8fb19bd7a36c2929139c434d53b958d8f08028a3",
      "size": 348,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8fb19bd7a36c2929139c434d53b958d8f08028a3"
    },
    {
      "path": "pkg/epp/metrics/testdata/scheduler_e2e_duration_seconds_metric",
      "mode": "100644",
      "type": "blob",
      "sha": "4bfb2731074ffb550bf0e694c89a872cef6d8ce8",
      "size": 1162,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4bfb2731074ffb550bf0e694c89a872cef6d8ce8"
    },
    {
      "path": "pkg/epp/requestcontrol",
      "mode": "040000",
      "type": "tree",
      "sha": "e6d6cb02ae7bb7b0bb52b02ffeb7f819901994cc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/e6d6cb02ae7bb7b0bb52b02ffeb7f819901994cc"
    },
    {
      "path": "pkg/epp/requestcontrol/admission.go",
      "mode": "100644",
      "type": "blob",
      "sha": "86a963d69e64a5ff384eba83ee24fadd8e9f862d",
      "size": 13202,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/86a963d69e64a5ff384eba83ee24fadd8e9f862d"
    },
    {
      "path": "pkg/epp/requestcontrol/admission_integration_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0350ba4a02c16066c1cc238c18d60fe36a5090fa",
      "size": 12598,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0350ba4a02c16066c1cc238c18d60fe36a5090fa"
    },
    {
      "path": "pkg/epp/requestcontrol/admission_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bb145f416c115978ba643250edc41320eac0348c",
      "size": 21581,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bb145f416c115978ba643250edc41320eac0348c"
    },
    {
      "path": "pkg/epp/requestcontrol/candidates.go",
      "mode": "100644",
      "type": "blob",
      "sha": "08a19cc48cc14cf8c4f57c8c10bf8ce4b87b6bb2",
      "size": 11159,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/08a19cc48cc14cf8c4f57c8c10bf8ce4b87b6bb2"
    },
    {
      "path": "pkg/epp/requestcontrol/candidates_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e4f7925dafdf5bd21fe46981772bd0a6ac64a6fa",
      "size": 9682,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e4f7925dafdf5bd21fe46981772bd0a6ac64a6fa"
    },
    {
      "path": "pkg/epp/requestcontrol/director.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f1fb9e54f63613b98061d1971a36b6fa9be7ec83",
      "size": 31781,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f1fb9e54f63613b98061d1971a36b6fa9be7ec83"
    },
    {
      "path": "pkg/epp/requestcontrol/director_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f380a742b370eec96bdc111393a1584732991cc1",
      "size": 4168,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f380a742b370eec96bdc111393a1584732991cc1"
    },
    {
      "path": "pkg/epp/requestcontrol/director_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c04d4b2a76fe5aef8c7b3cc4bc427de407a90a29",
      "size": 74694,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c04d4b2a76fe5aef8c7b3cc4bc427de407a90a29"
    },
    {
      "path": "pkg/epp/requestcontrol/plugin_executor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "000daed5996858fc00cf4659c95ed852d16eefed",
      "size": 3212,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/000daed5996858fc00cf4659c95ed852d16eefed"
    },
    {
      "path": "pkg/epp/requestcontrol/plugin_executor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1b4020910dd3e5bcc65110145e925bc6edd3ee8a",
      "size": 15231,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1b4020910dd3e5bcc65110145e925bc6edd3ee8a"
    },
    {
      "path": "pkg/epp/requestcontrol/request_control_config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "22281bead0f1e724d521d83606f5910a9d9e33f4",
      "size": 6666,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/22281bead0f1e724d521d83606f5910a9d9e33f4"
    },
    {
      "path": "pkg/epp/requestcontrol/request_control_config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bafe2f61b2499893c0c6b9f29162f14cd2b597a9",
      "size": 10983,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bafe2f61b2499893c0c6b9f29162f14cd2b597a9"
    },
    {
      "path": "pkg/epp/requestcontrol/screener_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "075d6f5a0b6513e9b40b559c8c9a2141452f4315",
      "size": 4471,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/075d6f5a0b6513e9b40b559c8c9a2141452f4315"
    },
    {
      "path": "pkg/epp/scheduling",
      "mode": "040000",
      "type": "tree",
      "sha": "a639e144abd70450734be0545eb15a49020b717e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a639e144abd70450734be0545eb15a49020b717e"
    },
    {
      "path": "pkg/epp/scheduling/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6b602154eac57b387b362746cc8e751221a494dc",
      "size": 1014,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6b602154eac57b387b362746cc8e751221a494dc"
    },
    {
      "path": "pkg/epp/scheduling/scheduler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "14289cde4fd164bcc3c0feb3902fdada2c67337d",
      "size": 6179,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/14289cde4fd164bcc3c0feb3902fdada2c67337d"
    },
    {
      "path": "pkg/epp/scheduling/scheduler_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "36ec47edd2786dce648d3d5f7537b57088294544",
      "size": 5764,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/36ec47edd2786dce648d3d5f7537b57088294544"
    },
    {
      "path": "pkg/epp/scheduling/scheduler_config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "be0f6557cc30a2492ab3a6a4075145a62c27ef8e",
      "size": 1379,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/be0f6557cc30a2492ab3a6a4075145a62c27ef8e"
    },
    {
      "path": "pkg/epp/scheduling/scheduler_profile.go",
      "mode": "100644",
      "type": "blob",
      "sha": "33b2513d83ccbdb0bf01bfe1b3a83a30236e1d4c",
      "size": 17385,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/33b2513d83ccbdb0bf01bfe1b3a83a30236e1d4c"
    },
    {
      "path": "pkg/epp/scheduling/scheduler_profile_picker_tracing_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "268e16fd54fe9072742bdd9c86facbad7c204e2e",
      "size": 9323,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/268e16fd54fe9072742bdd9c86facbad7c204e2e"
    },
    {
      "path": "pkg/epp/scheduling/scheduler_profile_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d8ed8c14b5572c67c46039f69f50450d68773a81",
      "size": 34332,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d8ed8c14b5572c67c46039f69f50450d68773a81"
    },
    {
      "path": "pkg/epp/scheduling/scheduler_profile_tracing_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1813efc4237e4a03fc0ead7309d0f29d98f6db61",
      "size": 13856,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1813efc4237e4a03fc0ead7309d0f29d98f6db61"
    },
    {
      "path": "pkg/epp/scheduling/scheduler_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fe2f357f9644a925d7ec0ff4c9d9e43d52b61915",
      "size": 7057,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fe2f357f9644a925d7ec0ff4c9d9e43d52b61915"
    },
    {
      "path": "pkg/epp/scheduling/weighted_scorer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d174fdeafd92e7d6f8052835b817944177f06d09",
      "size": 1157,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d174fdeafd92e7d6f8052835b817944177f06d09"
    },
    {
      "path": "pkg/epp/server",
      "mode": "040000",
      "type": "tree",
      "sha": "7179bbb6574650f36401dacc2729f237dc4e59e9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7179bbb6574650f36401dacc2729f237dc4e59e9"
    },
    {
      "path": "pkg/epp/server/controller_config.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b2004afa0f0543f30262dccb9d54970ebf305f6e",
      "size": 3951,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b2004afa0f0543f30262dccb9d54970ebf305f6e"
    },
    {
      "path": "pkg/epp/server/controller_config_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "80407f91ee83109dc45ef8f74eb866fa532658f0",
      "size": 5696,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/80407f91ee83109dc45ef8f74eb866fa532658f0"
    },
    {
      "path": "pkg/epp/server/controller_manager.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bbc0248437f9078525a1df833a360afa68f3823f",
      "size": 4491,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bbc0248437f9078525a1df833a360afa68f3823f"
    },
    {
      "path": "pkg/epp/server/grpc_stream_metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "eaf3516ef2e04dc7f8fab01ef981f0a21049f9b5",
      "size": 1650,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eaf3516ef2e04dc7f8fab01ef981f0a21049f9b5"
    },
    {
      "path": "pkg/epp/server/grpc_stream_metrics_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d37872348783d1ee0f2afdb78651fb4f936d7015",
      "size": 4357,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d37872348783d1ee0f2afdb78651fb4f936d7015"
    },
    {
      "path": "pkg/epp/server/metrics_tls_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f71b5285db6522b3f53dac5aca5f964fc8334fcc",
      "size": 2636,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f71b5285db6522b3f53dac5aca5f964fc8334fcc"
    },
    {
      "path": "pkg/epp/server/options.go",
      "mode": "100644",
      "type": "blob",
      "sha": "cd43e7b11fc9dffcf40649dd62f26af99bbbf834",
      "size": 24981,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cd43e7b11fc9dffcf40649dd62f26af99bbbf834"
    },
    {
      "path": "pkg/epp/server/options_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "40d76aad05691f11647c1f01ac9c5982c3f4e39c",
      "size": 15126,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/40d76aad05691f11647c1f01ac9c5982c3f4e39c"
    },
    {
      "path": "pkg/epp/server/plugin_state_debug.go",
      "mode": "100644",
      "type": "blob",
      "sha": "935992b41fb81e12a04c12a5ab7dfd25b903381b",
      "size": 3949,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/935992b41fb81e12a04c12a5ab7dfd25b903381b"
    },
    {
      "path": "pkg/epp/server/plugin_state_debug_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "fd317ee2d4254332d1e133fdd86784dc52e9fa37",
      "size": 4694,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fd317ee2d4254332d1e133fdd86784dc52e9fa37"
    },
    {
      "path": "pkg/epp/server/runserver.go",
      "mode": "100644",
      "type": "blob",
      "sha": "09826392b36289d8e2e480b23c227cefefe4a669",
      "size": 9923,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/09826392b36289d8e2e480b23c227cefefe4a669"
    },
    {
      "path": "pkg/epp/server/runserver_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0386b5c8a988c492748a97c4aca0b73cbd361d9c",
      "size": 1180,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0386b5c8a988c492748a97c4aca0b73cbd361d9c"
    },
    {
      "path": "pkg/epp/server/server_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0a4d42d3521968808d981213a242b4823ffa1580",
      "size": 23007,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0a4d42d3521968808d981213a242b4823ffa1580"
    },
    {
      "path": "pkg/epp/server/tls_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7a0c95987f66fb2008d28f8e38cc2a5a75a78219",
      "size": 2351,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a0c95987f66fb2008d28f8e38cc2a5a75a78219"
    },
    {
      "path": "pkg/epp/statesync",
      "mode": "040000",
      "type": "tree",
      "sha": "f614ae15045af554126f5618297f8d55945e6310",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/f614ae15045af554126f5618297f8d55945e6310"
    },
    {
      "path": "pkg/epp/statesync/peerstore.go",
      "mode": "100644",
      "type": "blob",
      "sha": "feb5106dc95dd8f47198d3667c7a5f62224b962d",
      "size": 2238,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/feb5106dc95dd8f47198d3667c7a5f62224b962d"
    },
    {
      "path": "pkg/epp/statesync/peerstore_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8eb35ead42a2d6d22c1cb83dd374dbbc3b16e99a",
      "size": 4291,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8eb35ead42a2d6d22c1cb83dd374dbbc3b16e99a"
    },
    {
      "path": "pkg/epp/util",
      "mode": "040000",
      "type": "tree",
      "sha": "eb6b560374822fb0996acd942cca051ea2eb5733",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/eb6b560374822fb0996acd942cca051ea2eb5733"
    },
    {
      "path": "pkg/epp/util/env",
      "mode": "040000",
      "type": "tree",
      "sha": "4013a81b84a4988308ff992124964cd5e930a818",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4013a81b84a4988308ff992124964cd5e930a818"
    },
    {
      "path": "pkg/epp/util/env/env.go",
      "mode": "100644",
      "type": "blob",
      "sha": "37cb0738d3a3aa2e28555de9d4bcffa3a7b1cbdd",
      "size": 2835,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/37cb0738d3a3aa2e28555de9d4bcffa3a7b1cbdd"
    },
    {
      "path": "pkg/epp/util/env/env_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1b634b6212f9cbd72f2957d1a84b601cdef65235",
      "size": 7198,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1b634b6212f9cbd72f2957d1a84b601cdef65235"
    },
    {
      "path": "pkg/epp/util/pod",
      "mode": "040000",
      "type": "tree",
      "sha": "3cd7ae8912e59ba140efd9e8cd821e438d143c8d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/3cd7ae8912e59ba140efd9e8cd821e438d143c8d"
    },
    {
      "path": "pkg/epp/util/pod/pod.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4fcb948fcbfa1fcb67d94972a2ea98ba66dd398c",
      "size": 915,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4fcb948fcbfa1fcb67d94972a2ea98ba66dd398c"
    },
    {
      "path": "pkg/epp/util/pod/pod_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b9d0f26d0e6ab03afb53da4f29b8815e2cfe5a68",
      "size": 3775,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b9d0f26d0e6ab03afb53da4f29b8815e2cfe5a68"
    },
    {
      "path": "pkg/epp/util/pool",
      "mode": "040000",
      "type": "tree",
      "sha": "bfc686c6014966ff8e16a856de217080cc85e8e5",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/bfc686c6014966ff8e16a856de217080cc85e8e5"
    },
    {
      "path": "pkg/epp/util/pool/pool.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3f20cfa7328b0495361267bd7b13bbfd6644d6d6",
      "size": 1519,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3f20cfa7328b0495361267bd7b13bbfd6644d6d6"
    },
    {
      "path": "pkg/epp/util/pool/pool_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "97941808663ddaee2e787e0032dbd0f73cf2aa4f",
      "size": 1883,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/97941808663ddaee2e787e0032dbd0f73cf2aa4f"
    },
    {
      "path": "pkg/epp/util/request",
      "mode": "040000",
      "type": "tree",
      "sha": "5939949f74787c829d48e9b1fa2d714048a5b4ab",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5939949f74787c829d48e9b1fa2d714048a5b4ab"
    },
    {
      "path": "pkg/epp/util/request/headers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1998e87914b4932f64077a975e75d1959747abfe",
      "size": 2438,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1998e87914b4932f64077a975e75d1959747abfe"
    },
    {
      "path": "pkg/epp/util/request/headers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0145be93f9604b0f96986d119ca47162769b8266",
      "size": 1806,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0145be93f9604b0f96986d119ca47162769b8266"
    },
    {
      "path": "pkg/epp/util/request/sheddable.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c2f32c1f278b68600ad033251de1850557912728",
      "size": 735,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c2f32c1f278b68600ad033251de1850557912728"
    },
    {
      "path": "pkg/epp/util/testing",
      "mode": "040000",
      "type": "tree",
      "sha": "eb2323e016c4db0c4c89149ee47a70695e1f3860",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/eb2323e016c4db0c4c89149ee47a70695e1f3860"
    },
    {
      "path": "pkg/epp/util/testing/wrappers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e39004b56f9fbe2b8ca767066fa40cce60736f75",
      "size": 5664,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e39004b56f9fbe2b8ca767066fa40cce60736f75"
    },
    {
      "path": "pkg/epp/util/utils.go",
      "mode": "100644",
      "type": "blob",
      "sha": "418783644ff98b70b3742f129ba8ddb76b01f15c",
      "size": 2442,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/418783644ff98b70b3742f129ba8ddb76b01f15c"
    },
    {
      "path": "pkg/epp/util/utils_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "96664653a4cfe4ed64fd581d7bf6e95bd93d2e4d",
      "size": 3046,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/96664653a4cfe4ed64fd581d7bf6e95bd93d2e4d"
    },
    {
      "path": "pkg/generator",
      "mode": "040000",
      "type": "tree",
      "sha": "14a0075f1134a944ca298931b5b0f5b97dff8ca2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/14a0075f1134a944ca298931b5b0f5b97dff8ca2"
    },
    {
      "path": "pkg/generator/main.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f17fb34144cb14cc6581d694d93a7cf92928a068",
      "size": 2966,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f17fb34144cb14cc6581d694d93a7cf92928a068"
    },
    {
      "path": "pkg/kvcache",
      "mode": "040000",
      "type": "tree",
      "sha": "ba5b1e296d76e54d448b9b4cbd78d7ae45e758ea",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ba5b1e296d76e54d448b9b4cbd78d7ae45e758ea"
    },
    {
      "path": "pkg/kvcache/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "0a7396e68b13b2f9c0a785f2e2aa9ff54d87f24a",
      "size": 3207,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0a7396e68b13b2f9c0a785f2e2aa9ff54d87f24a"
    },
    {
      "path": "pkg/kvcache/backend.go",
      "mode": "100644",
      "type": "blob",
      "sha": "76074a1d5a5da46ddcc9c5f5bdaf04b3acdb2a54",
      "size": 1170,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/76074a1d5a5da46ddcc9c5f5bdaf04b3acdb2a54"
    },
    {
      "path": "pkg/kvcache/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "eb40b51b62ce3ed464b25750da554bd3401ea6c3",
      "size": 718,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eb40b51b62ce3ed464b25750da554bd3401ea6c3"
    },
    {
      "path": "pkg/kvcache/export_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9f9d225d042971d00725e836ec84c41bdd206dfc",
      "size": 1270,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9f9d225d042971d00725e836ec84c41bdd206dfc"
    },
    {
      "path": "pkg/kvcache/indexer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2ffd937ceaeec7cb2c582c122f33153608204a93",
      "size": 7821,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2ffd937ceaeec7cb2c582c122f33153608204a93"
    },
    {
      "path": "pkg/kvcache/indexer_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ed8cf0e73b975e3304e3b4c4b811ba1013257836",
      "size": 2486,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ed8cf0e73b975e3304e3b4c4b811ba1013257836"
    },
    {
      "path": "pkg/kvcache/indexer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "65ed4487b4fa97824d9141e31a90d3681662d36c",
      "size": 7812,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/65ed4487b4fa97824d9141e31a90d3681662d36c"
    },
    {
      "path": "pkg/kvcache/indexer_trace_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7751955fb07cbb631632681a6fcff830fb63d907",
      "size": 5557,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7751955fb07cbb631632681a6fcff830fb63d907"
    },
    {
      "path": "pkg/kvcache/kvblock",
      "mode": "040000",
      "type": "tree",
      "sha": "610b7597441b2d5b51e615d97329ced12c296e65",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/610b7597441b2d5b51e615d97329ced12c296e65"
    },
    {
      "path": "pkg/kvcache/kvblock/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "a7869350bf4afdfdc54bd4a2665460dae994fa9d",
      "size": 2920,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a7869350bf4afdfdc54bd4a2665460dae994fa9d"
    },
    {
      "path": "pkg/kvcache/kvblock/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4ae746ac1f56e893bc0b246facff899b1f12d442",
      "size": 726,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4ae746ac1f56e893bc0b246facff899b1f12d442"
    },
    {
      "path": "pkg/kvcache/kvblock/cost_aware_memory.go",
      "mode": "100644",
      "type": "blob",
      "sha": "958bbe0ad435df1fb3595fd46f6166dee6fcda2d",
      "size": 16986,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/958bbe0ad435df1fb3595fd46f6166dee6fcda2d"
    },
    {
      "path": "pkg/kvcache/kvblock/cost_aware_memory_internal_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "247fdd74da32a3d93bab9d28ba1e010ec78b41cf",
      "size": 2253,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/247fdd74da32a3d93bab9d28ba1e010ec78b41cf"
    },
    {
      "path": "pkg/kvcache/kvblock/cost_aware_memory_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ed10fffecef7130bc1d66df23a90729d867b4951",
      "size": 3582,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ed10fffecef7130bc1d66df23a90729d867b4951"
    },
    {
      "path": "pkg/kvcache/kvblock/extra_keys.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7a38c498a38a262b4dda1a9cbbfd46c777b50729",
      "size": 4639,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a38c498a38a262b4dda1a9cbbfd46c777b50729"
    },
    {
      "path": "pkg/kvcache/kvblock/extra_keys_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ff6914478365fe7bf642fc8cafb689bb6bba018d",
      "size": 11093,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ff6914478365fe7bf642fc8cafb689bb6bba018d"
    },
    {
      "path": "pkg/kvcache/kvblock/hma.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b245dacfeaf309c7dfe2cd97195e1f556db06dee",
      "size": 1750,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b245dacfeaf309c7dfe2cd97195e1f556db06dee"
    },
    {
      "path": "pkg/kvcache/kvblock/in_memory.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dae897eba0e283c9a0efe3238cce41b2bfc34e74",
      "size": 17119,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dae897eba0e283c9a0efe3238cce41b2bfc34e74"
    },
    {
      "path": "pkg/kvcache/kvblock/in_memory_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "060981df9e5a825cc6299c81c2c2764e9f3b98ef",
      "size": 5237,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/060981df9e5a825cc6299c81c2c2764e9f3b98ef"
    },
    {
      "path": "pkg/kvcache/kvblock/in_memory_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0be17019e07542e98f321813d51e35a6d780e23c",
      "size": 10197,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0be17019e07542e98f321813d51e35a6d780e23c"
    },
    {
      "path": "pkg/kvcache/kvblock/index.go",
      "mode": "100644",
      "type": "blob",
      "sha": "03ec82683794c2c58091a532fe060075eae1d668",
      "size": 8474,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/03ec82683794c2c58091a532fe060075eae1d668"
    },
    {
      "path": "pkg/kvcache/kvblock/index_internal_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a9281321b2666f7d5b78030ef6745473f747911e",
      "size": 2670,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a9281321b2666f7d5b78030ef6745473f747911e"
    },
    {
      "path": "pkg/kvcache/kvblock/index_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "79194889cc64e6c441f9dcb15db61881967bdf74",
      "size": 33399,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/79194889cc64e6c441f9dcb15db61881967bdf74"
    },
    {
      "path": "pkg/kvcache/kvblock/instrumented_index.go",
      "mode": "100644",
      "type": "blob",
      "sha": "688a61ba2718bfae13b0217aebc540ddb646d7fe",
      "size": 2917,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/688a61ba2718bfae13b0217aebc540ddb646d7fe"
    },
    {
      "path": "pkg/kvcache/kvblock/instrumented_index_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "82566d7390ffd5e64825ca07c7eea68a60cdab3e",
      "size": 1413,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/82566d7390ffd5e64825ca07c7eea68a60cdab3e"
    },
    {
      "path": "pkg/kvcache/kvblock/interner_internal_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3ff320b6568f308d8271e3ce11cba5c41b1d77c2",
      "size": 2371,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3ff320b6568f308d8271e3ce11cba5c41b1d77c2"
    },
    {
      "path": "pkg/kvcache/kvblock/lru_store.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0c5b5d22028c77d6a8c516d2335ed8c68d3aba3a",
      "size": 2497,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0c5b5d22028c77d6a8c516d2335ed8c68d3aba3a"
    },
    {
      "path": "pkg/kvcache/kvblock/redis.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7b58ed17ee495d95dd53352852fbf9979d3be2c8",
      "size": 12472,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7b58ed17ee495d95dd53352852fbf9979d3be2c8"
    },
    {
      "path": "pkg/kvcache/kvblock/redis_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4f5ecb0ed8497e584f74105238241ac63884a1c0",
      "size": 1380,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4f5ecb0ed8497e584f74105238241ac63884a1c0"
    },
    {
      "path": "pkg/kvcache/kvblock/token_processor.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7eed21121cbf3023c797b376faddc36b0133c84b",
      "size": 10691,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7eed21121cbf3023c797b376faddc36b0133c84b"
    },
    {
      "path": "pkg/kvcache/kvblock/token_processor_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0da547eae87435c0eb257475e23d33ab5fa40e02",
      "size": 33986,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0da547eae87435c0eb257475e23d33ab5fa40e02"
    },
    {
      "path": "pkg/kvcache/kvblock/traced_index.go",
      "mode": "100644",
      "type": "blob",
      "sha": "257d1f5cc2f2d45568190f728f4d66b352b8008d",
      "size": 5105,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/257d1f5cc2f2d45568190f728f4d66b352b8008d"
    },
    {
      "path": "pkg/kvcache/kvblock/traced_index_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "becc198e3bad931c0cff56b9dddf0e123ccdf8ed",
      "size": 10473,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/becc198e3bad931c0cff56b9dddf0e123ccdf8ed"
    },
    {
      "path": "pkg/kvcache/kvblock/walk.go",
      "mode": "100644",
      "type": "blob",
      "sha": "89f8e7f623c8a772f07a9476bcdb81aaaaf19557",
      "size": 5659,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/89f8e7f623c8a772f07a9476bcdb81aaaaf19557"
    },
    {
      "path": "pkg/kvcache/kvblock/walk_internal_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c4ebda8510d95b6abb2f7a2848f88d7d9af09362",
      "size": 1857,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c4ebda8510d95b6abb2f7a2848f88d7d9af09362"
    },
    {
      "path": "pkg/kvcache/kvblock/walk_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "70d63aa924b766c7ee871f3ffbf1aa025e6ad730",
      "size": 14359,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/70d63aa924b766c7ee871f3ffbf1aa025e6ad730"
    },
    {
      "path": "pkg/kvcache/kvblock_scorer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "422ead484506787c08f84a395ae39d68934348a3",
      "size": 3715,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/422ead484506787c08f84a395ae39d68934348a3"
    },
    {
      "path": "pkg/kvcache/kvblock_scorer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "759fae1cd4590ee8b816781f3352877a4051ac4f",
      "size": 2989,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/759fae1cd4590ee8b816781f3352877a4051ac4f"
    },
    {
      "path": "pkg/kvcache/metrics",
      "mode": "040000",
      "type": "tree",
      "sha": "4049563a8a36b216d3b7f9f67f88c39b83794ef3",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4049563a8a36b216d3b7f9f67f88c39b83794ef3"
    },
    {
      "path": "pkg/kvcache/metrics/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "f521a4dc4bf89310eebbaa9efcca41269b9d9f0c",
      "size": 1164,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f521a4dc4bf89310eebbaa9efcca41269b9d9f0c"
    },
    {
      "path": "pkg/kvcache/metrics/collector.go",
      "mode": "100644",
      "type": "blob",
      "sha": "eaa75680672fd02ae5a3e50b1dc9fce08b8c8b30",
      "size": 12776,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eaa75680672fd02ae5a3e50b1dc9fce08b8c8b30"
    },
    {
      "path": "pkg/kvcache/metrics/collector_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c41b19633abf3703160d5a44dff763d1e54744f3",
      "size": 7099,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c41b19633abf3703160d5a44dff763d1e54744f3"
    },
    {
      "path": "pkg/kvcache/multi_rank_prefix_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "085975f8c4edfcbfca918d8befd26999165af2cb",
      "size": 4443,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/085975f8c4edfcbfca918d8befd26999165af2cb"
    },
    {
      "path": "pkg/kvcache/multi_rank_scoretokens_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1171985835b9beea11a96ea976e672c85e2602f7",
      "size": 2663,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1171985835b9beea11a96ea976e672c85e2602f7"
    },
    {
      "path": "pkg/kvcache/prefix_match.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8808dc5eb51a158b2f0dfacc8ee3c3a7268e8b5b",
      "size": 14352,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8808dc5eb51a158b2f0dfacc8ee3c3a7268e8b5b"
    },
    {
      "path": "pkg/kvcache/prefix_match_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b260e6487b1baaa44e18c81e94c2917cc822dd5e",
      "size": 6072,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b260e6487b1baaa44e18c81e94c2917cc822dd5e"
    },
    {
      "path": "pkg/kvcache/prefix_match_internal_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a458b0eaf7ced2e26e2f23e0279a94c7e3747b3e",
      "size": 4064,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a458b0eaf7ced2e26e2f23e0279a94c7e3747b3e"
    },
    {
      "path": "pkg/kvcache/prefix_match_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0581bf94cf9a4ebd9d398a91e8ae669c0edf2688",
      "size": 20519,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0581bf94cf9a4ebd9d398a91e8ae669c0edf2688"
    },
    {
      "path": "pkg/kvcache/tokenization",
      "mode": "040000",
      "type": "tree",
      "sha": "978c0a9ada0027995920c8504c243f0324451ac9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/978c0a9ada0027995920c8504c243f0324451ac9"
    },
    {
      "path": "pkg/kvcache/tokenization/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "b18cc19046c03ae2067f66ebc73194456194b686",
      "size": 1136,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b18cc19046c03ae2067f66ebc73194456194b686"
    },
    {
      "path": "pkg/kvcache/tokenization/features.go",
      "mode": "100644",
      "type": "blob",
      "sha": "13973940a2ff80e77ae16dd55a6844ac0ffa54bf",
      "size": 1056,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/13973940a2ff80e77ae16dd55a6844ac0ffa54bf"
    },
    {
      "path": "pkg/kvcache/tokenization/types",
      "mode": "040000",
      "type": "tree",
      "sha": "05e424b8cf3ad8330f5a62a97ea8112d9f9f514c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/05e424b8cf3ad8330f5a62a97ea8112d9f9f514c"
    },
    {
      "path": "pkg/kvcache/tokenization/types/types.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9ee7741d24f8884fd4ebbaf072385e4ea927a02a",
      "size": 3833,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9ee7741d24f8884fd4ebbaf072385e4ea927a02a"
    },
    {
      "path": "pkg/kvcache/traced_scorer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "dc7d0a06cc2f6913f7936bdccb6caba6f4708415",
      "size": 2393,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dc7d0a06cc2f6913f7936bdccb6caba6f4708415"
    },
    {
      "path": "pkg/kvcache/traced_scorer_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f22299fc1363470729e54b7123cf0c983c8a06cf",
      "size": 3567,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f22299fc1363470729e54b7123cf0c983c8a06cf"
    },
    {
      "path": "pkg/kvevents",
      "mode": "040000",
      "type": "tree",
      "sha": "13adb4970b2d0c0584591cdabab590b34b86d470",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/13adb4970b2d0c0584591cdabab590b34b86d470"
    },
    {
      "path": "pkg/kvevents/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1420df87c9d1501b2ca7f0fad87a87020c18bc7e",
      "size": 2299,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1420df87c9d1501b2ca7f0fad87a87020c18bc7e"
    },
    {
      "path": "pkg/kvevents/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b76033808bb1128ef0ecc9fe41a0df924a66b0b5",
      "size": 764,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b76033808bb1128ef0ecc9fe41a0df924a66b0b5"
    },
    {
      "path": "pkg/kvevents/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a676cc87637474e50ab873a1c5c7a714f94d5887",
      "size": 1063,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a676cc87637474e50ab873a1c5c7a714f94d5887"
    },
    {
      "path": "pkg/kvevents/engineadapter",
      "mode": "040000",
      "type": "tree",
      "sha": "d0bb98da50b32b1d0fc8f5bfb647eebef07f577d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d0bb98da50b32b1d0fc8f5bfb647eebef07f577d"
    },
    {
      "path": "pkg/kvevents/engineadapter/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "b394b3d454f5777cebf7ae2d42c75bbee613b822",
      "size": 1083,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b394b3d454f5777cebf7ae2d42c75bbee613b822"
    },
    {
      "path": "pkg/kvevents/engineadapter/adapter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3f72544286545d33b8d14d288571e85e9fe4dc7d",
      "size": 1514,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3f72544286545d33b8d14d288571e85e9fe4dc7d"
    },
    {
      "path": "pkg/kvevents/engineadapter/common.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c68a3893ca2623f3601a5ff84b0a3ea26a166b23",
      "size": 4140,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c68a3893ca2623f3601a5ff84b0a3ea26a166b23"
    },
    {
      "path": "pkg/kvevents/engineadapter/common_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f212452839086163a6e125625c54adccc1efd18e",
      "size": 2314,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f212452839086163a6e125625c54adccc1efd18e"
    },
    {
      "path": "pkg/kvevents/engineadapter/sglang_adapter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bf1a45340accbc50af491ade58da3048aff9ecce",
      "size": 7741,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf1a45340accbc50af491ade58da3048aff9ecce"
    },
    {
      "path": "pkg/kvevents/engineadapter/sglang_adapter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2b2b6ab662808a4eaff3189954122027945867ce",
      "size": 8451,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2b2b6ab662808a4eaff3189954122027945867ce"
    },
    {
      "path": "pkg/kvevents/engineadapter/vllm_adapter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a1993de833a8108b7299fa9adea60a642bb0ca62",
      "size": 12702,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a1993de833a8108b7299fa9adea60a642bb0ca62"
    },
    {
      "path": "pkg/kvevents/engineadapter/vllm_adapter_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "02eb7ef4bcd84a6f9b2ce0ea06ecaddee746c836",
      "size": 4257,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/02eb7ef4bcd84a6f9b2ce0ea06ecaddee746c836"
    },
    {
      "path": "pkg/kvevents/engineadapter/vllm_adapter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "111e1244a103fa5488f20576ebbb16d7dfbe3ef7",
      "size": 19545,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/111e1244a103fa5488f20576ebbb16d7dfbe3ef7"
    },
    {
      "path": "pkg/kvevents/event_dedup_filter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ce0acaba388a6fde48d3631903c1af2268b0e05c",
      "size": 7512,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ce0acaba388a6fde48d3631903c1af2268b0e05c"
    },
    {
      "path": "pkg/kvevents/event_dedup_filter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "eb4d1b0b0bd65ed82eeee4f08b77be1428b7d700",
      "size": 7200,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/eb4d1b0b0bd65ed82eeee4f08b77be1428b7d700"
    },
    {
      "path": "pkg/kvevents/events.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b15f3bb67e750b111e1afa27f64d37edd4ad374c",
      "size": 5278,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b15f3bb67e750b111e1afa27f64d37edd4ad374c"
    },
    {
      "path": "pkg/kvevents/pool.go",
      "mode": "100644",
      "type": "blob",
      "sha": "db027d2caba1f5c26add043f1daa5666273c09c3",
      "size": 28366,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/db027d2caba1f5c26add043f1daa5666273c09c3"
    },
    {
      "path": "pkg/kvevents/pool_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "07828055b8f1ff63c134f13c2d5c84b2ffaf4c65",
      "size": 46937,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/07828055b8f1ff63c134f13c2d5c84b2ffaf4c65"
    },
    {
      "path": "pkg/kvevents/pool_tracing_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "54fe96811d26ef4d5856a1be9b43eb7b0b769d38",
      "size": 9956,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/54fe96811d26ef4d5856a1be9b43eb7b0b769d38"
    },
    {
      "path": "pkg/kvevents/subscriber_manager.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b80b313a7569bac53a7324fc877cbbe546bf7a11",
      "size": 6370,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b80b313a7569bac53a7324fc877cbbe546bf7a11"
    },
    {
      "path": "pkg/kvevents/subscriber_manager_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4ebb2e33b9bce11b90739e99f9df7591fce3cfb4",
      "size": 8718,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4ebb2e33b9bce11b90739e99f9df7591fce3cfb4"
    },
    {
      "path": "pkg/kvevents/zmq_subscriber.go",
      "mode": "100644",
      "type": "blob",
      "sha": "acd585ab1d1d796cdea5158bd0a88f3e4682389a",
      "size": 14719,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/acd585ab1d1d796cdea5158bd0a88f3e4682389a"
    },
    {
      "path": "pkg/kvevents/zmq_subscriber_bench_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ea61cb4edd7066b2c1a24dd9cfdb61cb7613488d",
      "size": 2476,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ea61cb4edd7066b2c1a24dd9cfdb61cb7613488d"
    },
    {
      "path": "pkg/kvevents/zmq_subscriber_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "b2990110ffbba1a00bb54a3d3b60c0d26f9b3186",
      "size": 23358,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b2990110ffbba1a00bb54a3d3b60c0d26f9b3186"
    },
    {
      "path": "pkg/kvevents/zmq_subscriber_tracing_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5c8bceec9a993a009b36100e45b7e03aed87376b",
      "size": 3244,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5c8bceec9a993a009b36100e45b7e03aed87376b"
    },
    {
      "path": "pkg/sidecar",
      "mode": "040000",
      "type": "tree",
      "sha": "5ff2d14b6a9b5c01161edc2e0f2c1e9fbac67cc6",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/5ff2d14b6a9b5c01161edc2e0f2c1e9fbac67cc6"
    },
    {
      "path": "pkg/sidecar/constants",
      "mode": "040000",
      "type": "tree",
      "sha": "1848adeda2a7f98210b9efb4879e6e238c348191",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1848adeda2a7f98210b9efb4879e6e238c348191"
    },
    {
      "path": "pkg/sidecar/constants/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "381cfde1721493f810b49d0c0931169bf1b5a181",
      "size": 1373,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/381cfde1721493f810b49d0c0931169bf1b5a181"
    },
    {
      "path": "pkg/sidecar/proxy",
      "mode": "040000",
      "type": "tree",
      "sha": "782f78c8f658cf07c7ccdc77ede3ee18fe216519",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/782f78c8f658cf07c7ccdc77ede3ee18fe216519"
    },
    {
      "path": "pkg/sidecar/proxy/MORIIO_README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "8dd431c0afdf0a2eab54c636bcc8cb3e31dd1027",
      "size": 10580,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8dd431c0afdf0a2eab54c636bcc8cb3e31dd1027"
    },
    {
      "path": "pkg/sidecar/proxy/allowlist.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ea54b594e5aed52322b28fb1a67ead28b64cff81",
      "size": 13289,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ea54b594e5aed52322b28fb1a67ead28b64cff81"
    },
    {
      "path": "pkg/sidecar/proxy/allowlist_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "170a25cd1677c8ebc74fc8bdf5bdd3e68dc06a08",
      "size": 7038,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/170a25cd1677c8ebc74fc8bdf5bdd3e68dc06a08"
    },
    {
      "path": "pkg/sidecar/proxy/cached_tokens_usage_rewriter.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d134aa09332c3f564b7862091d613f7392cf6165",
      "size": 9130,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d134aa09332c3f564b7862091d613f7392cf6165"
    },
    {
      "path": "pkg/sidecar/proxy/cached_tokens_usage_rewriter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8e6341e3830686e72d819b6a357f2da7351f87d2",
      "size": 10240,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8e6341e3830686e72d819b6a357f2da7351f87d2"
    },
    {
      "path": "pkg/sidecar/proxy/chat_completions.go",
      "mode": "100644",
      "type": "blob",
      "sha": "64931eea349acf57da67023824f2ef3a15a56221",
      "size": 8111,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/64931eea349acf57da67023824f2ef3a15a56221"
    },
    {
      "path": "pkg/sidecar/proxy/chat_completions_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "399838914852d7d47a595726785f7abdcc17d2b8",
      "size": 12887,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/399838914852d7d47a595726785f7abdcc17d2b8"
    },
    {
      "path": "pkg/sidecar/proxy/chat_completions_trace_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "773514b1cd69c07584d23282348bf7169a552e9b",
      "size": 4082,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/773514b1cd69c07584d23282348bf7169a552e9b"
    },
    {
      "path": "pkg/sidecar/proxy/connector_ec_common.go",
      "mode": "100644",
      "type": "blob",
      "sha": "06c033982adf7d1c304efc4ba228fd366a323473",
      "size": 9432,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/06c033982adf7d1c304efc4ba228fd366a323473"
    },
    {
      "path": "pkg/sidecar/proxy/connector_ec_common_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2755224f83787aca4ec00cd2899374af7fcaa1fc",
      "size": 5323,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2755224f83787aca4ec00cd2899374af7fcaa1fc"
    },
    {
      "path": "pkg/sidecar/proxy/connector_ec_multimedia_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "31969552ab15911a2d74f1485aeeb4649b706085",
      "size": 5683,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/31969552ab15911a2d74f1485aeeb4649b706085"
    },
    {
      "path": "pkg/sidecar/proxy/connector_ec_nixl.go",
      "mode": "100644",
      "type": "blob",
      "sha": "402a3066aef0363b8b6ca2ea183baac89f1219a5",
      "size": 4372,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/402a3066aef0363b8b6ca2ea183baac89f1219a5"
    },
    {
      "path": "pkg/sidecar/proxy/connector_ec_nixl_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ecbafe5f098112559f92a396835be5427d29550f",
      "size": 26001,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ecbafe5f098112559f92a396835be5427d29550f"
    },
    {
      "path": "pkg/sidecar/proxy/connector_ec_shared_storage.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a6aab33840e829fffbdd180d40af7e09a1244769",
      "size": 2533,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a6aab33840e829fffbdd180d40af7e09a1244769"
    },
    {
      "path": "pkg/sidecar/proxy/connector_ec_shared_storage_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e73abffbe30be63051673148d561662508e2e127",
      "size": 11188,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e73abffbe30be63051673148d561662508e2e127"
    },
    {
      "path": "pkg/sidecar/proxy/connector_mooncake.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a9246ee31c7d11977f1262570f60c65fbd160e88",
      "size": 6116,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a9246ee31c7d11977f1262570f60c65fbd160e88"
    },
    {
      "path": "pkg/sidecar/proxy/connector_mooncake_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8050c7eaa3f808f9ce9454ecee123b97f2f5a314",
      "size": 10127,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8050c7eaa3f808f9ce9454ecee123b97f2f5a314"
    },
    {
      "path": "pkg/sidecar/proxy/connector_nixlv2.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9cf94935254851ea2ff19150ce5df33eec7f4ee7",
      "size": 31057,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9cf94935254851ea2ff19150ce5df33eec7f4ee7"
    },
    {
      "path": "pkg/sidecar/proxy/connector_nixlv2_p2p_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "83815171210eafd32fa5b94796f5d0a49e5dc6ac",
      "size": 6075,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/83815171210eafd32fa5b94796f5d0a49e5dc6ac"
    },
    {
      "path": "pkg/sidecar/proxy/connector_nixlv2_parallel_commit_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a055c6c29cd2b539b06f45575d1b0514da708d13",
      "size": 7219,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a055c6c29cd2b539b06f45575d1b0514da708d13"
    },
    {
      "path": "pkg/sidecar/proxy/connector_nixlv2_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "09bfd219207ba664d71240038868f661347daf22",
      "size": 52234,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/09bfd219207ba664d71240038868f661347daf22"
    },
    {
      "path": "pkg/sidecar/proxy/connector_p2p.go",
      "mode": "100644",
      "type": "blob",
      "sha": "06050b07ed66e835cc9678794d6e10207eaa712f",
      "size": 14776,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/06050b07ed66e835cc9678794d6e10207eaa712f"
    },
    {
      "path": "pkg/sidecar/proxy/connector_p2p_source_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5c3332ffac033198fc4458a97ba37cc72779884d",
      "size": 10390,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5c3332ffac033198fc4458a97ba37cc72779884d"
    },
    {
      "path": "pkg/sidecar/proxy/connector_p2p_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9b7d9b41ce71312ff3324a84820327cc4bb109e5",
      "size": 14983,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9b7d9b41ce71312ff3324a84820327cc4bb109e5"
    },
    {
      "path": "pkg/sidecar/proxy/connector_pd_common.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2b9b3221d927a66ca26f548a022c66355e2a5c5d",
      "size": 4977,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2b9b3221d927a66ca26f548a022c66355e2a5c5d"
    },
    {
      "path": "pkg/sidecar/proxy/connector_sglang.go",
      "mode": "100644",
      "type": "blob",
      "sha": "98eb42b415f6eaf4a086bcb6f1fbe97685549acf",
      "size": 3175,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/98eb42b415f6eaf4a086bcb6f1fbe97685549acf"
    },
    {
      "path": "pkg/sidecar/proxy/connector_sglang_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "32917c97f44d11a69d020f624c63f11b9a4fb4b5",
      "size": 6043,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/32917c97f44d11a69d020f624c63f11b9a4fb4b5"
    },
    {
      "path": "pkg/sidecar/proxy/connector_shared_storage.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4f273f6ce1ee814ed4cfa06f5729895587ddc733",
      "size": 8904,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4f273f6ce1ee814ed4cfa06f5729895587ddc733"
    },
    {
      "path": "pkg/sidecar/proxy/connector_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "41a1462bb24e63528f01afdaf5f36edba643f983",
      "size": 11376,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/41a1462bb24e63528f01afdaf5f36edba643f983"
    },
    {
      "path": "pkg/sidecar/proxy/constants.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c96ffe95545e5160531fc363ed28dbc3d6b235a2",
      "size": 706,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c96ffe95545e5160531fc363ed28dbc3d6b235a2"
    },
    {
      "path": "pkg/sidecar/proxy/data_parallel.go",
      "mode": "100644",
      "type": "blob",
      "sha": "85b53c5c8218e7f92254446dfb2f255308a0b6fd",
      "size": 2796,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/85b53c5c8218e7f92254446dfb2f255308a0b6fd"
    },
    {
      "path": "pkg/sidecar/proxy/data_parallel_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "076dcda7b4d9f9a05685318bd02e8efd73c5c012",
      "size": 3350,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/076dcda7b4d9f9a05685318bd02e8efd73c5c012"
    },
    {
      "path": "pkg/sidecar/proxy/decode.go",
      "mode": "100644",
      "type": "blob",
      "sha": "383822dc3ece701725e4b410056d76f87565c007",
      "size": 14595,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/383822dc3ece701725e4b410056d76f87565c007"
    },
    {
      "path": "pkg/sidecar/proxy/decode_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8c383b19be4091a566ec26db8a5d430e07af1cb6",
      "size": 11213,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8c383b19be4091a566ec26db8a5d430e07af1cb6"
    },
    {
      "path": "pkg/sidecar/proxy/dns_metrics.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c6787a2d0ea1744d7ff0b513da874e68034de44d",
      "size": 5430,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c6787a2d0ea1744d7ff0b513da874e68034de44d"
    },
    {
      "path": "pkg/sidecar/proxy/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3b25f28a5c43ccca34888bc867de8d9959bc70bb",
      "size": 98,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3b25f28a5c43ccca34888bc867de8d9959bc70bb"
    },
    {
      "path": "pkg/sidecar/proxy/dp_rank.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6d4836734160e931374c63f20620ee0d0ddbf649",
      "size": 2761,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6d4836734160e931374c63f20620ee0d0ddbf649"
    },
    {
      "path": "pkg/sidecar/proxy/dp_rank_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "17e91319608ca24b4f78c125f3df83e0bce956e3",
      "size": 8017,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/17e91319608ca24b4f78c125f3df83e0bce956e3"
    },
    {
      "path": "pkg/sidecar/proxy/errors.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f182af0b6ee840fa2251e3ac28d3b6503cf18554",
      "size": 2334,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f182af0b6ee840fa2251e3ac28d3b6503cf18554"
    },
    {
      "path": "pkg/sidecar/proxy/options.go",
      "mode": "100644",
      "type": "blob",
      "sha": "84b4b2edd7925043309fc8de177348d0d4234e2a",
      "size": 40027,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/84b4b2edd7925043309fc8de177348d0d4234e2a"
    },
    {
      "path": "pkg/sidecar/proxy/options_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3347a885985604302101a01e2e1bc37322218a0f",
      "size": 52838,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3347a885985604302101a01e2e1bc37322218a0f"
    },
    {
      "path": "pkg/sidecar/proxy/proxy.go",
      "mode": "100644",
      "type": "blob",
      "sha": "589ebbb35b713da69e8a003f434b7b3d21f720a8",
      "size": 27437,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/589ebbb35b713da69e8a003f434b7b3d21f720a8"
    },
    {
      "path": "pkg/sidecar/proxy/proxy_helpers.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0076381a141ef30bdb84364b6e6e1bbd67e49afc",
      "size": 9652,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0076381a141ef30bdb84364b6e6e1bbd67e49afc"
    },
    {
      "path": "pkg/sidecar/proxy/proxy_helpers_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5688b2749b3586ddb587c07650c37a895d90643d",
      "size": 2553,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5688b2749b3586ddb587c07650c37a895d90643d"
    },
    {
      "path": "pkg/sidecar/proxy/proxy_suite_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "ee7165c468e141c53d3a3f28ff1ea26dc3201792",
      "size": 788,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ee7165c468e141c53d3a3f28ff1ea26dc3201792"
    },
    {
      "path": "pkg/sidecar/proxy/proxy_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5d1d4b85bfdc6d2bdc6cc66b3ca682e93493c7f5",
      "size": 10221,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5d1d4b85bfdc6d2bdc6cc66b3ca682e93493c7f5"
    },
    {
      "path": "pkg/sidecar/proxy/resolver.go",
      "mode": "100644",
      "type": "blob",
      "sha": "82f9e8213068e2fc1205c593568e1a2949e12dc9",
      "size": 11520,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/82f9e8213068e2fc1205c593568e1a2949e12dc9"
    },
    {
      "path": "pkg/sidecar/proxy/resolver_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d5b96d88e44f7b9228b3a45fe26cdebd9f0da79f",
      "size": 11106,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d5b96d88e44f7b9228b3a45fe26cdebd9f0da79f"
    },
    {
      "path": "pkg/sidecar/proxy/status_response_writer.go",
      "mode": "100644",
      "type": "blob",
      "sha": "01dbbdbb1bf1774e04b37ebc9a7ca4e8783edb51",
      "size": 11307,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/01dbbdbb1bf1774e04b37ebc9a7ca4e8783edb51"
    },
    {
      "path": "pkg/sidecar/proxy/tls.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d5aa28933824bc125a52be7759fc4be3bae2552d",
      "size": 2448,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d5aa28933824bc125a52be7759fc4be3bae2552d"
    },
    {
      "path": "pkg/sidecar/proxy/transport_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d55d5b9588919490bc50b37795f4d7380fe168ca",
      "size": 2395,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d55d5b9588919490bc50b37795f4d7380fe168ca"
    },
    {
      "path": "pkg/sidecar/version",
      "mode": "040000",
      "type": "tree",
      "sha": "d2cc20607059c051961add9649ead44a33194144",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d2cc20607059c051961add9649ead44a33194144"
    },
    {
      "path": "pkg/sidecar/version/version.go",
      "mode": "100644",
      "type": "blob",
      "sha": "71ecdcf4a3d5eebebfcffd696cd83e9f6ff86403",
      "size": 260,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/71ecdcf4a3d5eebebfcffd696cd83e9f6ff86403"
    },
    {
      "path": "release-notes.d",
      "mode": "040000",
      "type": "tree",
      "sha": "4a9f6ccd6997cdccb38e987846f7600454ee30e6",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4a9f6ccd6997cdccb38e987846f7600454ee30e6"
    },
    {
      "path": "release-notes.d/unreleased",
      "mode": "040000",
      "type": "tree",
      "sha": "9a70bf7c9feeeac0736df11f4d123e6fbdd83c47",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/9a70bf7c9feeeac0736df11f4d123e6fbdd83c47"
    },
    {
      "path": "release-notes.d/unreleased/2325.md",
      "mode": "100644",
      "type": "blob",
      "sha": "048b3f62150d95a91bb45224cd434a90549282be",
      "size": 307,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/048b3f62150d95a91bb45224cd434a90549282be"
    },
    {
      "path": "release-notes.d/unreleased/2371.md",
      "mode": "100644",
      "type": "blob",
      "sha": "fc173b5bf0ff562ccd16d48788e3e07228d88cf4",
      "size": 352,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/fc173b5bf0ff562ccd16d48788e3e07228d88cf4"
    },
    {
      "path": "release-notes.d/unreleased/2396.md",
      "mode": "100644",
      "type": "blob",
      "sha": "6283926c5923f771cbe0f891490f39f50fcac0bc",
      "size": 228,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6283926c5923f771cbe0f891490f39f50fcac0bc"
    },
    {
      "path": "release-notes.d/unreleased/2460.md",
      "mode": "100644",
      "type": "blob",
      "sha": "98dce2bad5c22985091cf83d1e38b1a07129f786",
      "size": 322,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/98dce2bad5c22985091cf83d1e38b1a07129f786"
    },
    {
      "path": "release-notes.d/unreleased/2492.md",
      "mode": "100644",
      "type": "blob",
      "sha": "c26f5f89a7ae41668f9cb759ded206c9739d2199",
      "size": 216,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c26f5f89a7ae41668f9cb759ded206c9739d2199"
    },
    {
      "path": "release-notes.d/unreleased/2571.md",
      "mode": "100644",
      "type": "blob",
      "sha": "972f63638bd4360571a3cd437d6fdee64bff5833",
      "size": 170,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/972f63638bd4360571a3cd437d6fdee64bff5833"
    },
    {
      "path": "release-notes.d/unreleased/2591.md",
      "mode": "100644",
      "type": "blob",
      "sha": "c1f077f31c7296e9c61409a0b98c25afe0f3b5d6",
      "size": 305,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c1f077f31c7296e9c61409a0b98c25afe0f3b5d6"
    },
    {
      "path": "release-notes.d/unreleased/2631.md",
      "mode": "100644",
      "type": "blob",
      "sha": "16836a063a0680a3417160d7e804bdc5691c7cde",
      "size": 351,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/16836a063a0680a3417160d7e804bdc5691c7cde"
    },
    {
      "path": "release-notes.d/unreleased/2643.md",
      "mode": "100644",
      "type": "blob",
      "sha": "86abb139e72908b74a16c58321a984784fa91b80",
      "size": 306,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/86abb139e72908b74a16c58321a984784fa91b80"
    },
    {
      "path": "release-notes.d/unreleased/2649.md",
      "mode": "100644",
      "type": "blob",
      "sha": "19f35bd4e4ecdd5265348b0a542bf6b72bd35157",
      "size": 333,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/19f35bd4e4ecdd5265348b0a542bf6b72bd35157"
    },
    {
      "path": "release-notes.d/unreleased/2675.md",
      "mode": "100644",
      "type": "blob",
      "sha": "02f22873eee4c6099550230fe4805c3e2d36923c",
      "size": 227,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/02f22873eee4c6099550230fe4805c3e2d36923c"
    },
    {
      "path": "release-notes.d/unreleased/2676.md",
      "mode": "100644",
      "type": "blob",
      "sha": "154520d5c4909a9f1dd1932529432baf6e6274f9",
      "size": 193,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/154520d5c4909a9f1dd1932529432baf6e6274f9"
    },
    {
      "path": "release-notes.d/unreleased/2721.md",
      "mode": "100644",
      "type": "blob",
      "sha": "6ba371bb87994a0a2fb52ef36edf06abc32275c9",
      "size": 348,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6ba371bb87994a0a2fb52ef36edf06abc32275c9"
    },
    {
      "path": "release-notes.d/unreleased/2732.md",
      "mode": "100644",
      "type": "blob",
      "sha": "aa2bf01f068da8a52a5ef02d041ee52e19c53883",
      "size": 261,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/aa2bf01f068da8a52a5ef02d041ee52e19c53883"
    },
    {
      "path": "release-notes.d/unreleased/2748.md",
      "mode": "100644",
      "type": "blob",
      "sha": "94ff06cc61db2e3d074a38610ef04e226112b1ce",
      "size": 188,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/94ff06cc61db2e3d074a38610ef04e226112b1ce"
    },
    {
      "path": "release-notes.d/unreleased/2766.md",
      "mode": "100644",
      "type": "blob",
      "sha": "ee7b43f31db2c4ce1a2d62874e26622dd7d33692",
      "size": 175,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ee7b43f31db2c4ce1a2d62874e26622dd7d33692"
    },
    {
      "path": "release-notes.d/unreleased/2809.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1ae93ecef11aee57f18641d3c09229c775edf91f",
      "size": 371,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1ae93ecef11aee57f18641d3c09229c775edf91f"
    },
    {
      "path": "scripts",
      "mode": "040000",
      "type": "tree",
      "sha": "fc0344b2e89cf409fb2e6b4b4f577472741c2dbf",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/fc0344b2e89cf409fb2e6b4b4f577472741c2dbf"
    },
    {
      "path": "scripts/check-commits.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "a24fef11c7fc9dac866d285ed7cd0efcc2811cb5",
      "size": 1324,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a24fef11c7fc9dac866d285ed7cd0efcc2811cb5"
    },
    {
      "path": "scripts/check-latest-tags.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "dcaf173a6599d5c3a6ef8ad54056c49f579e1222",
      "size": 3186,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/dcaf173a6599d5c3a6ef8ad54056c49f579e1222"
    },
    {
      "path": "scripts/compare-coverage.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "7a9828ee24d1b96d292145c5822792342b7d23de",
      "size": 4382,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7a9828ee24d1b96d292145c5822792342b7d23de"
    },
    {
      "path": "scripts/istio",
      "mode": "040000",
      "type": "tree",
      "sha": "a50675dee5e40271a85d51b4b7b23d2ee260b8ac",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/a50675dee5e40271a85d51b4b7b23d2ee260b8ac"
    },
    {
      "path": "scripts/istio/generate-cp.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "8ddc81185b464b4f2d09d7a75986154efd30a738",
      "size": 533,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8ddc81185b464b4f2d09d7a75986154efd30a738"
    },
    {
      "path": "scripts/istio/istio-cp.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "bcb9a4feb3c556c5b4215cc4c1a3739a16b4ebb3",
      "size": 290,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bcb9a4feb3c556c5b4215cc4c1a3739a16b4ebb3"
    },
    {
      "path": "scripts/istio/manifest-splitter.py",
      "mode": "100755",
      "type": "blob",
      "sha": "6da4414f862a66248aefa5f32c2d109d1c552d58",
      "size": 6586,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6da4414f862a66248aefa5f32c2d109d1c552d58"
    },
    {
      "path": "scripts/kind-dev-env.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "beddf5a842ca70ddc60a77a403e8936d89cf2d90",
      "size": 21497,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/beddf5a842ca70ddc60a77a403e8936d89cf2d90"
    },
    {
      "path": "scripts/kubernetes-dev-env.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "600b943e39fb41dff0948465ed5b375b83ce2b1d",
      "size": 9488,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/600b943e39fb41dff0948465ed5b375b83ce2b1d"
    },
    {
      "path": "scripts/load_image.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "5346cdd9eac874fa76e0c40db645b0471892e7d2",
      "size": 1059,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5346cdd9eac874fa76e0c40db645b0471892e7d2"
    },
    {
      "path": "scripts/migrate-gaie-paths.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "c2348438bd876526381257323f18d9987e07dccc",
      "size": 11487,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c2348438bd876526381257323f18d9987e07dccc"
    },
    {
      "path": "scripts/pull_images.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "53edc39ead950ad87c096fd08718e2d6fc6faaee",
      "size": 2709,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/53edc39ead950ad87c096fd08718e2d6fc6faaee"
    },
    {
      "path": "test",
      "mode": "040000",
      "type": "tree",
      "sha": "4e8361f71fdbc135c4bb5e87dc8cc10e9e93426e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4e8361f71fdbc135c4bb5e87dc8cc10e9e93426e"
    },
    {
      "path": "test/coordinator",
      "mode": "040000",
      "type": "tree",
      "sha": "55e2e1c9df42aa5c74a25445cf8c4dd3633443b7",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/55e2e1c9df42aa5c74a25445cf8c4dd3633443b7"
    },
    {
      "path": "test/coordinator/e2e",
      "mode": "040000",
      "type": "tree",
      "sha": "1eca1c4143df49a253f4ccb582476a17a88ef51d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1eca1c4143df49a253f4ccb582476a17a88ef51d"
    },
    {
      "path": "test/coordinator/e2e/coordinator",
      "mode": "040000",
      "type": "tree",
      "sha": "711fc9d00b3480b1f8231fa65f9b7a438b8a97e1",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/711fc9d00b3480b1f8231fa65f9b7a438b8a97e1"
    },
    {
      "path": "test/coordinator/e2e/coordinator/configs_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f63e942c8f88a3be1a66cf7f8f347f5d4f981ab9",
      "size": 6272,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f63e942c8f88a3be1a66cf7f8f347f5d4f981ab9"
    },
    {
      "path": "test/coordinator/e2e/coordinator/coordinator_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "53eccfb3f39dda117a5afd1788e1c7c761a04679",
      "size": 20053,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/53eccfb3f39dda117a5afd1788e1c7c761a04679"
    },
    {
      "path": "test/coordinator/e2e/coordinator/e2e_suite_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f3bf7c7250bf545aec33b326a4ccf17f0e374336",
      "size": 14179,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f3bf7c7250bf545aec33b326a4ccf17f0e374336"
    },
    {
      "path": "test/coordinator/e2e/coordinator/generate_endpoint_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9689a90721271def995af3391664e2748dc393f2",
      "size": 4273,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9689a90721271def995af3391664e2748dc393f2"
    },
    {
      "path": "test/coordinator/e2e/coordinator/pools_check_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d146a6079b5b287763e2c3a79b0551380699b170",
      "size": 1483,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d146a6079b5b287763e2c3a79b0551380699b170"
    },
    {
      "path": "test/coordinator/e2e/coordinator/setup_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "135cafe992fae82ee4e1603f3c1edd0df1cc4561",
      "size": 16840,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/135cafe992fae82ee4e1603f3c1edd0df1cc4561"
    },
    {
      "path": "test/coordinator/e2e/coordinator/utils_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f6bfa192b9909fca7c4d35a06d3f5188c8069030",
      "size": 3567,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f6bfa192b9909fca7c4d35a06d3f5188c8069030"
    },
    {
      "path": "test/coordinator/e2e/internal",
      "mode": "040000",
      "type": "tree",
      "sha": "b6cbcb47c343a72590b675918fab9e887c1b6c0c",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b6cbcb47c343a72590b675918fab9e887c1b6c0c"
    },
    {
      "path": "test/coordinator/e2e/internal/e2eutil",
      "mode": "040000",
      "type": "tree",
      "sha": "d67ecc6541e5fd49b30e02a5ad135a18dbd75660",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d67ecc6541e5fd49b30e02a5ad135a18dbd75660"
    },
    {
      "path": "test/coordinator/e2e/internal/e2eutil/util.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3b15c08e92adae974a4781761bab752e11418ef7",
      "size": 4351,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3b15c08e92adae974a4781761bab752e11418ef7"
    },
    {
      "path": "test/coordinator/scripts",
      "mode": "040000",
      "type": "tree",
      "sha": "0d104f4c7e39fc62f9be0ccfaf4d17ae68a6840d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/0d104f4c7e39fc62f9be0ccfaf4d17ae68a6840d"
    },
    {
      "path": "test/coordinator/scripts/run_e2e_coordinator.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "b714b0cdceb2845d7c768481be0f29bf46eb1349",
      "size": 637,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b714b0cdceb2845d7c768481be0f29bf46eb1349"
    },
    {
      "path": "test/e2e",
      "mode": "040000",
      "type": "tree",
      "sha": "38ad66f5519b70629f0f100b05738bc332fd0066",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/38ad66f5519b70629f0f100b05738bc332fd0066"
    },
    {
      "path": "test/e2e/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "47e8175081d5e335798ab1c02598e476676577a7",
      "size": 4401,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/47e8175081d5e335798ab1c02598e476676577a7"
    },
    {
      "path": "test/e2e/configs_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bf2d900f61d02c25cd0d1ea67da32fb882cf80a2",
      "size": 7907,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf2d900f61d02c25cd0d1ea67da32fb882cf80a2"
    },
    {
      "path": "test/e2e/disruption_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "be910d2c02b46ba58b96d5fc21c4ba4d68931db4",
      "size": 14968,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/be910d2c02b46ba58b96d5fc21c4ba4d68931db4"
    },
    {
      "path": "test/e2e/e2e_suite_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a827991b6f41df2a88408893fadb42d0011b0e39",
      "size": 16059,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a827991b6f41df2a88408893fadb42d0011b0e39"
    },
    {
      "path": "test/e2e/e2e_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a2f719e60f0fd0e7148fee63ab20fd853d06379b",
      "size": 48435,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a2f719e60f0fd0e7148fee63ab20fd853d06379b"
    },
    {
      "path": "test/e2e/generate_endpoint_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1b28b2d26fd355831695a076efd3bd7626bd6290",
      "size": 12446,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1b28b2d26fd355831695a076efd3bd7626bd6290"
    },
    {
      "path": "test/e2e/requests_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9f69cdecc24c40c63606413866eda159943e7353",
      "size": 18508,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9f69cdecc24c40c63606413866eda159943e7353"
    },
    {
      "path": "test/e2e/setup_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d3efc76de6adbfe01af1596e204ce770b263f8f9",
      "size": 10732,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d3efc76de6adbfe01af1596e204ce770b263f8f9"
    },
    {
      "path": "test/e2e/utils_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "75cd1179150f6efd482772541a47bdb92cfded63",
      "size": 13841,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/75cd1179150f6efd482772541a47bdb92cfded63"
    },
    {
      "path": "test/framework",
      "mode": "040000",
      "type": "tree",
      "sha": "37f2161399e50771981e382da60cc988baead6be",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/37f2161399e50771981e382da60cc988baead6be"
    },
    {
      "path": "test/framework/doc.go",
      "mode": "100644",
      "type": "blob",
      "sha": "033b89150aaba834629f8ae625101a913458694b",
      "size": 1415,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/033b89150aaba834629f8ae625101a913458694b"
    },
    {
      "path": "test/framework/framework_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4fcaceabf4ac9f94aa6ee6974c0326af40e9b8ec",
      "size": 806,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4fcaceabf4ac9f94aa6ee6974c0326af40e9b8ec"
    },
    {
      "path": "test/framework/net",
      "mode": "040000",
      "type": "tree",
      "sha": "1f472021c818192e22541ae175b3f856107a2e54",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1f472021c818192e22541ae175b3f856107a2e54"
    },
    {
      "path": "test/framework/net/net.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d00f3ba337f6a80a54c3328ec42e066baffb61f3",
      "size": 2164,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d00f3ba337f6a80a54c3328ec42e066baffb61f3"
    },
    {
      "path": "test/framework/net/net_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "98835a46c61eecd6f0147ba017e6595655a503d2",
      "size": 1982,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/98835a46c61eecd6f0147ba017e6595655a503d2"
    },
    {
      "path": "test/integration",
      "mode": "040000",
      "type": "tree",
      "sha": "add3349b92239e091fb66ec6d60ed9dfdb592010",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/add3349b92239e091fb66ec6d60ed9dfdb592010"
    },
    {
      "path": "test/integration/epp",
      "mode": "040000",
      "type": "tree",
      "sha": "603510a7d4b7fdbefca5dd46daa9697fb57e3d5b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/603510a7d4b7fdbefca5dd46daa9697fb57e3d5b"
    },
    {
      "path": "test/integration/epp/common_tests.go",
      "mode": "100644",
      "type": "blob",
      "sha": "7d667b878e7917ac646847b7eca04674d37699ec",
      "size": 15062,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7d667b878e7917ac646847b7eca04674d37699ec"
    },
    {
      "path": "test/integration/epp/datalayer_integration_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3b4c8145eed2ddaf936783b9b69cc1c2b6272ba1",
      "size": 2130,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3b4c8145eed2ddaf936783b9b69cc1c2b6272ba1"
    },
    {
      "path": "test/integration/epp/dynamic_attributes_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "3e1b4f6cdf594522893e08f280bef4d6cadee466",
      "size": 6245,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3e1b4f6cdf594522893e08f280bef4d6cadee466"
    },
    {
      "path": "test/integration/epp/e2e_config_smoke_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "559c3033ccec15f256cf5ab2f2d472797cac723c",
      "size": 6217,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/559c3033ccec15f256cf5ab2f2d472797cac723c"
    },
    {
      "path": "test/integration/epp/endpoint_scores_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "e49f12fa4ebf23380832e0c83fdc5c36e5389de4",
      "size": 4514,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e49f12fa4ebf23380832e0c83fdc5c36e5389de4"
    },
    {
      "path": "test/integration/epp/grpc_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "139dcff2d8fd32b1b16ae0bde9ff1f5ea37ff076",
      "size": 14773,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/139dcff2d8fd32b1b16ae0bde9ff1f5ea37ff076"
    },
    {
      "path": "test/integration/epp/harness.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f935ef4739181f7e59e9cbb14068fea220df9aa4",
      "size": 16410,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f935ef4739181f7e59e9cbb14068fea220df9aa4"
    },
    {
      "path": "test/integration/epp/hermetic_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "bc6674c3327478e8976f8a2427f7f63b1b2fb108",
      "size": 20100,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bc6674c3327478e8976f8a2427f7f63b1b2fb108"
    },
    {
      "path": "test/integration/epp/k8s_datasource_integration_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "cdf2374793196f15210c1c20c0a564f89b244bb0",
      "size": 10850,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/cdf2374793196f15210c1c20c0a564f89b244bb0"
    },
    {
      "path": "test/integration/epp/request_attribute_reporter_streaming_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "8d3fe894a2ee640ef3480692d926d57283fef77b",
      "size": 2312,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8d3fe894a2ee640ef3480692d926d57283fef77b"
    },
    {
      "path": "test/integration/epp/request_attribute_reporter_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "a063c2594dfc2ca16fc45d000f63cceb682bd216",
      "size": 3395,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a063c2594dfc2ca16fc45d000f63cceb682bd216"
    },
    {
      "path": "test/integration/epp/runtime_notification_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "1265fc54cb9fd68bdfec105e95164fc9d6e42718",
      "size": 14661,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1265fc54cb9fd68bdfec105e95164fc9d6e42718"
    },
    {
      "path": "test/integration/epp/runtime_polling_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c0dca690e2aa5bc77a0b5fdfef9c30ed5d297570",
      "size": 10193,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c0dca690e2aa5bc77a0b5fdfef9c30ed5d297570"
    },
    {
      "path": "test/integration/epp/session_affinity_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "133b57b23464cb05010fd747e6efecb1ce2ad1f6",
      "size": 20128,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/133b57b23464cb05010fd747e6efecb1ce2ad1f6"
    },
    {
      "path": "test/integration/epp/testdata",
      "mode": "040000",
      "type": "tree",
      "sha": "87ba00e522c0932f6f355c941a8dcc0cadf9ff5e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/87ba00e522c0932f6f355c941a8dcc0cadf9ff5e"
    },
    {
      "path": "test/integration/epp/testdata/datalayer-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "439d4d65675e92544d4147dc645cd65620593867",
      "size": 585,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/439d4d65675e92544d4147dc645cd65620593867"
    },
    {
      "path": "test/integration/epp/testdata/default-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "073c94ecc1429481ce1ea8c639757bd7446731db",
      "size": 498,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/073c94ecc1429481ce1ea8c639757bd7446731db"
    },
    {
      "path": "test/integration/epp/testdata/request-attribute-reporter-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "6c0d231fdd1677f98dbac0ec93c422ea0a559059",
      "size": 702,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6c0d231fdd1677f98dbac0ec93c422ea0a559059"
    },
    {
      "path": "test/integration/epp/wellknown_configs_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6d04a3374338ecd17e2311498ed20c8621f18dc9",
      "size": 14366,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6d04a3374338ecd17e2311498ed20c8621f18dc9"
    },
    {
      "path": "test/integration/epp_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f7f7e0421b8d9614fa87a54f87d76ed6c3818d11",
      "size": 1483,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f7f7e0421b8d9614fa87a54f87d76ed6c3818d11"
    },
    {
      "path": "test/integration/suite_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6c42efde752ef9b11ce21aa2360f83c2e1de6e89",
      "size": 1968,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6c42efde752ef9b11ce21aa2360f83c2e1de6e89"
    },
    {
      "path": "test/integration/util.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d851dfa48f3de1d9e10b1cb8de6889ff3f856739",
      "size": 24123,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d851dfa48f3de1d9e10b1cb8de6889ff3f856739"
    },
    {
      "path": "test/integration/util_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "f1c3a584b64698ed64c5dbbc2acd52b0668a33dd",
      "size": 5784,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/f1c3a584b64698ed64c5dbbc2acd52b0668a33dd"
    },
    {
      "path": "test/perf",
      "mode": "040000",
      "type": "tree",
      "sha": "80869103bf232f0e89e4c972e1201b3c9faaf0d8",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/80869103bf232f0e89e4c972e1201b3c9faaf0d8"
    },
    {
      "path": "test/perf/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "07c88dd56d6bb679c5973ded07107bc405aea017",
      "size": 4884,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/07c88dd56d6bb679c5973ded07107bc405aea017"
    },
    {
      "path": "test/perf/config",
      "mode": "040000",
      "type": "tree",
      "sha": "7887023cc33c2d8782c7d3a23172fa0cbb070fc4",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/7887023cc33c2d8782c7d3a23172fa0cbb070fc4"
    },
    {
      "path": "test/perf/config/llm-d-sim-deployment-1m-context.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a5cb0429184bd325df3d9db4ed8fb00c0641de14",
      "size": 2369,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a5cb0429184bd325df3d9db4ed8fb00c0641de14"
    },
    {
      "path": "test/perf/config/llm-d-sim-deployment-zmq.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "7dc3d1e9e57b1db92a98fb6c60fc259889d6f27b",
      "size": 4238,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7dc3d1e9e57b1db92a98fb6c60fc259889d6f27b"
    },
    {
      "path": "test/perf/config/llm-d-sim-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "4c34ad4b90e81edbfca752c185dd508c63b01acb",
      "size": 3287,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4c34ad4b90e81edbfca752c185dd508c63b01acb"
    },
    {
      "path": "test/perf/config/llm-d-sim-service.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "595c88cfd1b4ff9f0a35a6938e2207abaed9177e",
      "size": 340,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/595c88cfd1b4ff9f0a35a6938e2207abaed9177e"
    },
    {
      "path": "test/perf/config/router-configs",
      "mode": "040000",
      "type": "tree",
      "sha": "54023e2d16992710e2cb02ffaa01bd4eedafd2f6",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/54023e2d16992710e2cb02ffaa01bd4eedafd2f6"
    },
    {
      "path": "test/perf/config/router-configs/endpoint-attribute-parity.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "50186e8a7278d6720db13be53e1c7faa38fd8b41",
      "size": 3602,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/50186e8a7278d6720db13be53e1c7faa38fd8b41"
    },
    {
      "path": "test/perf/config/router-configs/load-aware-session-affinity-2rep.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "ae7cf08c2fc57f0fabe40ef42900bb19f0737303",
      "size": 1922,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ae7cf08c2fc57f0fabe40ef42900bb19f0737303"
    },
    {
      "path": "test/perf/config/router-configs/load-aware-session-affinity-3rep.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "83a599facd4ee4dbb1046ab8e279159d00312106",
      "size": 1922,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/83a599facd4ee4dbb1046ab8e279159d00312106"
    },
    {
      "path": "test/perf/config/router-configs/load-aware-session-affinity.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b5ba7dd4bb89b824f2f67f5dfe3d903407ed6bf3",
      "size": 1800,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b5ba7dd4bb89b824f2f67f5dfe3d903407ed6bf3"
    },
    {
      "path": "test/perf/config/router-configs/optimized-baseline.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "bf80aaf3f1a67c4e17be78f8153b40205be04d12",
      "size": 1791,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bf80aaf3f1a67c4e17be78f8153b40205be04d12"
    },
    {
      "path": "test/perf/config/router-configs/pd-session-affinity.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c5dd6ace4502d9632d0e45fbcdb72954d95f7f0a",
      "size": 2233,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c5dd6ace4502d9632d0e45fbcdb72954d95f7f0a"
    },
    {
      "path": "test/perf/config/router-configs/precise-prefix.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "5ae937d653949fc490a610eba421150d13862725",
      "size": 3820,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5ae937d653949fc490a610eba421150d13862725"
    },
    {
      "path": "test/perf/config/router-configs/random-only-default-parsers.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "3a7b66d25766a96534715e237f80682992ede7e8",
      "size": 1399,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3a7b66d25766a96534715e237f80682992ede7e8"
    },
    {
      "path": "test/perf/config/router-configs/random-passthrough-parser.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "442d28262219ba0a1b8eee942db31a6f62d02aae",
      "size": 1519,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/442d28262219ba0a1b8eee942db31a6f62d02aae"
    },
    {
      "path": "test/perf/config/router-configs/session-affinity-filter-2rep.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "054856bca436e92db0bd5fed63e0a1d03d0490a7",
      "size": 1859,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/054856bca436e92db0bd5fed63e0a1d03d0490a7"
    },
    {
      "path": "test/perf/config/router-configs/session-affinity-filter-3rep.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "acb6f6cd9fc74c11830425358227e9cd415cf4e0",
      "size": 1859,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/acb6f6cd9fc74c11830425358227e9cd415cf4e0"
    },
    {
      "path": "test/perf/config/router-configs/session-affinity-filter.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b9bde49caba2d1f930546ca464e94af500db1d54",
      "size": 1737,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b9bde49caba2d1f930546ca464e94af500db1d54"
    },
    {
      "path": "test/perf/config/shared_prefix_100k-1k-10qps.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "6550a82ef7a6fcbcb5182eb05bece5f699cb36d9",
      "size": 1603,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6550a82ef7a6fcbcb5182eb05bece5f699cb36d9"
    },
    {
      "path": "test/perf/config/shared_prefix_100k-1k-10qps_session-affinity.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "1464e8f280e7e4033776b2a567fce7628a2bddfa",
      "size": 1657,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1464e8f280e7e4033776b2a567fce7628a2bddfa"
    },
    {
      "path": "test/perf/config/shared_prefix_agentic-1m-1k.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "e869348614b5cd754dacd05a6ab4f2c79b7a226b",
      "size": 1676,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e869348614b5cd754dacd05a6ab4f2c79b7a226b"
    },
    {
      "path": "test/perf/config/shared_prefix_job1.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "749e466608fd0681321e09abbb107be618a456e8",
      "size": 1602,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/749e466608fd0681321e09abbb107be618a456e8"
    },
    {
      "path": "test/perf/config/studyanalysis",
      "mode": "040000",
      "type": "tree",
      "sha": "d8d9e489d8fec1b83dc742fcb9c1e74041141f24",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d8d9e489d8fec1b83dc742fcb9c1e74041141f24"
    },
    {
      "path": "test/perf/config/studyanalysis/high_itl_streaming_duration_analysis.md",
      "mode": "100644",
      "type": "blob",
      "sha": "10d97570a1b68d3e3d29ce3431ecdf6d47128194",
      "size": 5094,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/10d97570a1b68d3e3d29ce3431ecdf6d47128194"
    },
    {
      "path": "test/perf/config/studyanalysis/high_qps_small_token_comparison_analysis.md",
      "mode": "100644",
      "type": "blob",
      "sha": "0f734a099c63c9b0e6b1fdf1b9a953fd2dfdf405",
      "size": 4373,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0f734a099c63c9b0e6b1fdf1b9a953fd2dfdf405"
    },
    {
      "path": "test/perf/config/studyanalysis/input_token_size_impact_analysis.md",
      "mode": "100644",
      "type": "blob",
      "sha": "4629fd85b04d83c41389dd3e4516b0f3c5814faf",
      "size": 7385,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4629fd85b04d83c41389dd3e4516b0f3c5814faf"
    },
    {
      "path": "test/perf/config/studyanalysis/output_token_length_impact_analysis.md",
      "mode": "100644",
      "type": "blob",
      "sha": "466cfac64b7e836cbc6101c14173d3337be23151",
      "size": 4789,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/466cfac64b7e836cbc6101c14173d3337be23151"
    },
    {
      "path": "test/perf/config/studyanalysis/pprof_analysis_baseline_vs_precise.md",
      "mode": "100644",
      "type": "blob",
      "sha": "bbe26605855e592a0129c714367375e6bafd6e62",
      "size": 5624,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/bbe26605855e592a0129c714367375e6bafd6e62"
    },
    {
      "path": "test/perf/config/studyanalysis/precise_prefix_scaling_analysis.md",
      "mode": "100644",
      "type": "blob",
      "sha": "7be91512010012771dc7c926c7a84fafc718ea72",
      "size": 6839,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/7be91512010012771dc7c926c7a84fafc718ea72"
    },
    {
      "path": "test/perf/config/studyanalysis/session_affinity_horizontal_scaling.md",
      "mode": "100644",
      "type": "blob",
      "sha": "1df2cd591d101d3d5c76673ccd055e3061986efc",
      "size": 4697,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/1df2cd591d101d3d5c76673ccd055e3061986efc"
    },
    {
      "path": "test/perf/config/studyanalysis/session_affinity_multiturn_comparison.md",
      "mode": "100644",
      "type": "blob",
      "sha": "20ce8b283cd6c3f6ad001ca7eb13c231d8eab4bd",
      "size": 7809,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/20ce8b283cd6c3f6ad001ca7eb13c231d8eab4bd"
    },
    {
      "path": "test/perf/run_nightly_perf.py",
      "mode": "100755",
      "type": "blob",
      "sha": "306c067bd2e53c5de0d44ca3c9485fee368a1a65",
      "size": 37869,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/306c067bd2e53c5de0d44ca3c9485fee368a1a65"
    },
    {
      "path": "test/perf/test_run_nightly_perf.py",
      "mode": "100644",
      "type": "blob",
      "sha": "d3e42d6b7d33ec3380dd251acf184640fae30e6d",
      "size": 19007,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d3e42d6b7d33ec3380dd251acf184640fae30e6d"
    },
    {
      "path": "test/profiling",
      "mode": "040000",
      "type": "tree",
      "sha": "ebcde7a1edf17645bbdae47e58dff1acf7e9ff07",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/ebcde7a1edf17645bbdae47e58dff1acf7e9ff07"
    },
    {
      "path": "test/profiling/tokenizerbench",
      "mode": "040000",
      "type": "tree",
      "sha": "1b87ade4612b1801e0923160d6044d2021b6310e",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/1b87ade4612b1801e0923160d6044d2021b6310e"
    },
    {
      "path": "test/profiling/tokenizerbench/benchmark_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "11c1b943bf80b24a8392556343023192df526d09",
      "size": 7779,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/11c1b943bf80b24a8392556343023192df526d09"
    },
    {
      "path": "test/scripts",
      "mode": "040000",
      "type": "tree",
      "sha": "4738e98f8b46babe918628723e1348c3974e4b4b",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/4738e98f8b46babe918628723e1348c3974e4b4b"
    },
    {
      "path": "test/scripts/e2e-common.sh",
      "mode": "100644",
      "type": "blob",
      "sha": "6d0e170b9162e4a0e5d4c7200b86c9b73208baab",
      "size": 1501,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6d0e170b9162e4a0e5d4c7200b86c9b73208baab"
    },
    {
      "path": "test/scripts/test-e2e-router.sh",
      "mode": "100755",
      "type": "blob",
      "sha": "623f9a1adec75e6f7de1b4d0b9e123aeae251325",
      "size": 1282,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/623f9a1adec75e6f7de1b4d0b9e123aeae251325"
    },
    {
      "path": "test/sidecar",
      "mode": "040000",
      "type": "tree",
      "sha": "330f2698c6c7b761a1be3d8f9ed5c860f7996377",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/330f2698c6c7b761a1be3d8f9ed5c860f7996377"
    },
    {
      "path": "test/sidecar/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "c31fe5cfcf486711dbdb14754a6d1e6870d0c36f",
      "size": 3088,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c31fe5cfcf486711dbdb14754a6d1e6870d0c36f"
    },
    {
      "path": "test/sidecar/config",
      "mode": "040000",
      "type": "tree",
      "sha": "934eb2316566851d9d9ccb85dd2d7231a182d5dc",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/934eb2316566851d9d9ccb85dd2d7231a182d5dc"
    },
    {
      "path": "test/sidecar/config/base",
      "mode": "040000",
      "type": "tree",
      "sha": "b0be7157960ca76c4330afb01452266a3ce892a2",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/b0be7157960ca76c4330afb01452266a3ce892a2"
    },
    {
      "path": "test/sidecar/config/base/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "e13dded5b1c739d535402287ac22c4770c0d5f89",
      "size": 245,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e13dded5b1c739d535402287ac22c4770c0d5f89"
    },
    {
      "path": "test/sidecar/config/base/namespace.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "df788aac53bbfeefe9043559dc9de830be67c542",
      "size": 64,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/df788aac53bbfeefe9043559dc9de830be67c542"
    },
    {
      "path": "test/sidecar/config/base/pod-qwen.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "86d3f6bc80bccd2b00c8481df94f3f6aea46c2a6",
      "size": 1256,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/86d3f6bc80bccd2b00c8481df94f3f6aea46c2a6"
    },
    {
      "path": "test/sidecar/config/gateway",
      "mode": "040000",
      "type": "tree",
      "sha": "247a96d9cd4b38ae1fd4716c6cfdb8530a3267da",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/247a96d9cd4b38ae1fd4716c6cfdb8530a3267da"
    },
    {
      "path": "test/sidecar/config/gateway/gateway.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c4d2c109625c4d656d948d8e6571040d88a6412f",
      "size": 330,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c4d2c109625c4d656d948d8e6571040d88a6412f"
    },
    {
      "path": "test/sidecar/config/gateway/gatewayparams.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "ce61d61721c8652a342c89e1c16e0f919001b847",
      "size": 158,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ce61d61721c8652a342c89e1c16e0f919001b847"
    },
    {
      "path": "test/sidecar/config/gateway/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "78634b149ec60a6f87519ca14e5a3edcefe7d188",
      "size": 153,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/78634b149ec60a6f87519ca14e5a3edcefe7d188"
    },
    {
      "path": "test/sidecar/config/gateway/llmroute.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "6c26d080842a5096eb49ccbcf17342cf2ed2478a",
      "size": 486,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6c26d080842a5096eb49ccbcf17342cf2ed2478a"
    },
    {
      "path": "test/sidecar/config/nixl",
      "mode": "040000",
      "type": "tree",
      "sha": "64118569dbde4209a27af21446d3e5a9b537e8db",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/64118569dbde4209a27af21446d3e5a9b537e8db"
    },
    {
      "path": "test/sidecar/config/nixl/inferencepool.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "e1322ffd4ab5310238a03fbde57f26f5904e0c84",
      "size": 327,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e1322ffd4ab5310238a03fbde57f26f5904e0c84"
    },
    {
      "path": "test/sidecar/config/nixl/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "227dcfb7f71d2b10f17e683dbddcdf337b13a6ab",
      "size": 246,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/227dcfb7f71d2b10f17e683dbddcdf337b13a6ab"
    },
    {
      "path": "test/sidecar/config/nixl/pod-read-role.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "ecaeb44c78f072071c8e6fa8eac7bcc0d1c5023e",
      "size": 694,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/ecaeb44c78f072071c8e6fa8eac7bcc0d1c5023e"
    },
    {
      "path": "test/sidecar/config/nixl/pod-read-rolebinding.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "0c09913f5cc1b7f6e0ec9c984adbb79b92298fdb",
      "size": 246,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0c09913f5cc1b7f6e0ec9c984adbb79b92298fdb"
    },
    {
      "path": "test/sidecar/config/nixl/qwen-decoder-pod.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "e416eeb7d419ddb43cb92218cf14626c9b139aed",
      "size": 2437,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e416eeb7d419ddb43cb92218cf14626c9b139aed"
    },
    {
      "path": "test/sidecar/config/nixl/qwen-epp-svc.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "5371d49b34b84c6239a21717cfafcdc8357f0d9f",
      "size": 216,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5371d49b34b84c6239a21717cfafcdc8357f0d9f"
    },
    {
      "path": "test/sidecar/config/nixl/qwen-epp.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "01d0f0499938139fdfc3f4f371b2f68a14593768",
      "size": 1469,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/01d0f0499938139fdfc3f4f371b2f68a14593768"
    },
    {
      "path": "test/sidecar/config/nixl/qwen-prefiller-pod.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "8cf564b45c0b3ef4f37c0659b14cbd8e3ceb69e0",
      "size": 2126,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/8cf564b45c0b3ef4f37c0659b14cbd8e3ceb69e0"
    },
    {
      "path": "test/sidecar/config/overlays",
      "mode": "040000",
      "type": "tree",
      "sha": "56e3cb60fa5f7ceb8dbcb8a7991f6f27b20b1a01",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/56e3cb60fa5f7ceb8dbcb8a7991f6f27b20b1a01"
    },
    {
      "path": "test/sidecar/config/overlays/kind",
      "mode": "040000",
      "type": "tree",
      "sha": "cb3b9f3acd5dbce38fc158747b6a516a18def9f1",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/cb3b9f3acd5dbce38fc158747b6a516a18def9f1"
    },
    {
      "path": "test/sidecar/config/overlays/kind/.secrets.env.template",
      "mode": "100644",
      "type": "blob",
      "sha": "e65443a54e77502f8de0d4d33cbdb692f63a516c",
      "size": 34,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/e65443a54e77502f8de0d4d33cbdb692f63a516c"
    },
    {
      "path": "test/sidecar/config/overlays/kind/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "b2a44ba40f11c0066b0d861283c582753dce23f8",
      "size": 358,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/b2a44ba40f11c0066b0d861283c582753dce23f8"
    },
    {
      "path": "test/sidecar/config/overlays/llmd",
      "mode": "040000",
      "type": "tree",
      "sha": "69af6b0e1eb01053ca8e77bb2ff1afa7b8bc3fac",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/69af6b0e1eb01053ca8e77bb2ff1afa7b8bc3fac"
    },
    {
      "path": "test/sidecar/config/overlays/llmd/nixl",
      "mode": "040000",
      "type": "tree",
      "sha": "2e491b3bd8e1e78f82898a310eb2be1fdb04cb53",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/2e491b3bd8e1e78f82898a310eb2be1fdb04cb53"
    },
    {
      "path": "test/sidecar/config/overlays/llmd/nixl/README.md",
      "mode": "100644",
      "type": "blob",
      "sha": "034c18122bdcccac822a9f3d704140e87e279bf9",
      "size": 119,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/034c18122bdcccac822a9f3d704140e87e279bf9"
    },
    {
      "path": "test/sidecar/config/overlays/llmd/nixl/kustomization.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c04d3a1d498a6540703d8ea6790bee098f6c0d39",
      "size": 306,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c04d3a1d498a6540703d8ea6790bee098f6c0d39"
    },
    {
      "path": "test/sidecar/e2e",
      "mode": "040000",
      "type": "tree",
      "sha": "dc0391794f866dc16f16df0c4d552631a136164a",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/dc0391794f866dc16f16df0c4d552631a136164a"
    },
    {
      "path": "test/sidecar/e2e/e2e_suite_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c4d8c42056bacca38491ce5275f9cf2ba1164e1b",
      "size": 866,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c4d8c42056bacca38491ce5275f9cf2ba1164e1b"
    },
    {
      "path": "test/sidecar/e2e/e2e_test.go",
      "mode": "100644",
      "type": "blob",
      "sha": "6fee3416ba935bbb65e8b4e560f01ee09d42774a",
      "size": 2732,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/6fee3416ba935bbb65e8b4e560f01ee09d42774a"
    },
    {
      "path": "test/sidecar/e2e/kind-config.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "2b056f08be1866b0b7dd502a6719825b14ed848d",
      "size": 176,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2b056f08be1866b0b7dd502a6719825b14ed848d"
    },
    {
      "path": "test/sidecar/mock",
      "mode": "040000",
      "type": "tree",
      "sha": "faf454b2d539623df281e85ae8632c4fbf497b8d",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/faf454b2d539623df281e85ae8632c4fbf497b8d"
    },
    {
      "path": "test/sidecar/mock/chat_completions_handler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "9c50d8def061dbfa168d9fcfe86957e9fd7fc99e",
      "size": 11225,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9c50d8def061dbfa168d9fcfe86957e9fd7fc99e"
    },
    {
      "path": "test/sidecar/mock/generic_handler.go",
      "mode": "100644",
      "type": "blob",
      "sha": "2aa143eefbfdd7fed584bcdce9773b34e64a6a7b",
      "size": 1125,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/2aa143eefbfdd7fed584bcdce9773b34e64a6a7b"
    },
    {
      "path": "test/sidecar/req-book.txt",
      "mode": "100755",
      "type": "blob",
      "sha": "3492b3f6e7359587cfbc904cd95ac5eedcabe093",
      "size": 4699,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/3492b3f6e7359587cfbc904cd95ac5eedcabe093"
    },
    {
      "path": "test/sidecar/utils",
      "mode": "040000",
      "type": "tree",
      "sha": "d3d608e2f0e539127e14ed1bf60ef38a3b8845a9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/d3d608e2f0e539127e14ed1bf60ef38a3b8845a9"
    },
    {
      "path": "test/sidecar/utils/utils.go",
      "mode": "100644",
      "type": "blob",
      "sha": "50090ad8a9cbbb9e3cd90bc71f1109fd3f242cc0",
      "size": 1568,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/50090ad8a9cbbb9e3cd90bc71f1109fd3f242cc0"
    },
    {
      "path": "test/testdata",
      "mode": "040000",
      "type": "tree",
      "sha": "caa8f42ae502660bf6cdc48effb124c3a1cd9355",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/caa8f42ae502660bf6cdc48effb124c3a1cd9355"
    },
    {
      "path": "test/testdata/configloader_1_test.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "a894b96184b9fec0cba5aba4cc5a2ceff07b745d",
      "size": 669,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/a894b96184b9fec0cba5aba4cc5a2ceff07b745d"
    },
    {
      "path": "test/testdata/inferencepool-with-model-hermetic.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "9c6852ff08eb9f39f8deba71d7c3e549f4d7922b",
      "size": 1876,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/9c6852ff08eb9f39f8deba71d7c3e549f4d7922b"
    },
    {
      "path": "test/testdata/sim-deployment.yaml",
      "mode": "100644",
      "type": "blob",
      "sha": "c75cb9853a0ba7c20c8b9ed474cac909fca19998",
      "size": 2052,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c75cb9853a0ba7c20c8b9ed474cac909fca19998"
    },
    {
      "path": "test/utils",
      "mode": "040000",
      "type": "tree",
      "sha": "e915b155b5a14eef6cd3e3de6d996556303cd9e9",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/e915b155b5a14eef6cd3e3de6d996556303cd9e9"
    },
    {
      "path": "test/utils/context.go",
      "mode": "100644",
      "type": "blob",
      "sha": "c821db9e0ccc515fec29ca31025f2a7dc9000a3d",
      "size": 537,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/c821db9e0ccc515fec29ca31025f2a7dc9000a3d"
    },
    {
      "path": "test/utils/dump.go",
      "mode": "100644",
      "type": "blob",
      "sha": "5116e923ed654aa25aad6db14828d8db0d956736",
      "size": 6155,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/5116e923ed654aa25aad6db14828d8db0d956736"
    },
    {
      "path": "test/utils/handle.go",
      "mode": "100644",
      "type": "blob",
      "sha": "55186c5e92e1635f3f9a00b1acaefac2cfc03c57",
      "size": 2272,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/55186c5e92e1635f3f9a00b1acaefac2cfc03c57"
    },
    {
      "path": "test/utils/parallel.go",
      "mode": "100644",
      "type": "blob",
      "sha": "0e3e919cc82abe47373fe21f2c7b3a392d36cc55",
      "size": 3835,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/0e3e919cc82abe47373fe21f2c7b3a392d36cc55"
    },
    {
      "path": "test/utils/server.go",
      "mode": "100644",
      "type": "blob",
      "sha": "710277daa035968199052a4fb0d77bc43f4fcd94",
      "size": 5764,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/710277daa035968199052a4fb0d77bc43f4fcd94"
    },
    {
      "path": "test/utils/utils.go",
      "mode": "100644",
      "type": "blob",
      "sha": "4f41d4faceb37f1adbe10581f80e5389201002e5",
      "size": 15083,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/4f41d4faceb37f1adbe10581f80e5389201002e5"
    },
    {
      "path": "version",
      "mode": "040000",
      "type": "tree",
      "sha": "31d8a562325153d3da9f7d0bc03b96dc9d25d8ff",
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/trees/31d8a562325153d3da9f7d0bc03b96dc9d25d8ff"
    },
    {
      "path": "version/version.go",
      "mode": "100644",
      "type": "blob",
      "sha": "d26aefe10a36cf10a4dcfe4426960ca5d16e9a23",
      "size": 1137,
      "url": "https://api.github.com/repos/llm-d/llm-d-router/git/blobs/d26aefe10a36cf10a4dcfe4426960ca5d16e9a23"
    }
  ],
  "truncated": false
}

```

