# Synthesis conflicts — orchestrator verdicts

**No substantive factual conflicts found across the three drafts.** All three carry the step-8 corrections
identically: v0.10.0 (2026-08-17) is the newest release and still uses the legacy `dataLayer.discovery.pluginRef`;
the nested `discovery.endpoints.pluginRef` merged 2026-08-25 and ships in no release; llm-d-router ≥ v0.9.0 is the
floor (FileDiscovery + the #1403/#1404 notification fix); PR #1007 was closed unmerged; the #1290 figures
(3,148 → 57 → 31 → 18 failures at 500 QPS; TTFT P50 165 ms → 30 ms) are quoted consistently.

What differs is framing and emphasis — by design, since the angles were assigned. Four divergences need a committed
verdict so the synthesizer does not hedge between them.

## Divergence 1: what the top-level recommendation IS

- **Draft A:** customize the `EndpointDiscovery` seam and nothing else; reach it in phases (0: file discovery with a
  generator; 1: turn on precise prefix routing; 2: the plugin, out of tree).
- **Draft B:** keep the router in Kubernetes and move the *inventory* instead; a three-gate ladder (pods via Slinky →
  endpoint picker as a Deployment with a ConfigMap-mounted endpoints file → only then a plugin, after a bake-off).
- **Draft C:** a decision procedure — availability selects topology first, then churn/addressing selects the discovery
  rung, then health gating and proxy failure mode as standing obligations.
- **Verdict: adopt C's shape, populated with A's phases and B's gates.** C is the only framing that survives both
  other drafts' evidence: A is right that the discovery seam is the only customization needed, B is right that the
  operations layer is Kubernetes-first, and those are compatible once topology and discovery are treated as *separate*
  decisions. The report commits to: (1) availability requirement selects where the router runs; (2) churn and
  addressing select the lowest discovery rung that works; (3) the out-of-tree plugin is an escalation with named
  trigger gates, not the opening move. B's "keep it in Kubernetes" becomes the recommended topology *when multi-replica
  HA is required*, not a global recommendation — its own evidence (Lease-coupled leader election) is what scopes it.

## Divergence 2: how much weight the out-of-tree precedent risk carries

- **Draft A:** documented mechanism (`docs/discovery.md` teaches `Register`-before-`Run`), no structural blocker, so
  the risk is manageable with two mitigations.
- **Draft B:** you would be the first known user — zero importers on the package index as of 2026-09-11, nothing in
  CONTRIBUTING.md, and the only written position is untriaged issue #2105 whose second option is "out-of-tree plugins
  that panic take the process down".
- **Verdict: both, in this order.** Assert the mechanism (it is documented for *discovery* plugins specifically —
  the in-tree instruction B leans on is the scheduling-*filter* tutorial), then concede first-adopter status plainly
  and price the mitigations: namespaced plugin type string, own `recover()` in `Start()`, pinned release, CI build
  against upstream `main`. Do not let either side's framing win alone; the honest position is "sanctioned mechanism,
  unprecedented in the wild".

## Divergence 3: effort numbers for the plugin

- **Draft A:** Phase 2 "1-3 weeks" for the plugin as a whole.
- **Draft C:** tiered — poll+diff ~2-4 days, long-poll +1-2 days, persistent watch ~1-2 weeks.
- **Draft B:** two weeks budgeted for a *bake-off* before building anything.
- **Verdict: use C's tiered estimate as the primary number** (it is the interim investigation's calibrated output and
  distinguishes the cheap default from the expensive case), and present A's 1-3 weeks as the end-to-end phase envelope
  including health gating, tests and CI. B's bake-off is a legitimate gate for the Gate-3 case only; keep it as a
  conditional, not a default step.

## Divergence 4: emphasis on the P/D sidecar security carve-out

- **Draft B** treats it as a headline objection; **A and C** treat it as a risk item.
- **Verdict: B is right on substance and it must be visible in the recommendation, not only in the risks section.**
  The allowlist is InferencePool-coupled, off by default, and ignores port even when on (#979); the upstream primary
  control is the gateway stripping `x-prefiller-host-port` on ingress (PR #244, after disclosure #242). State it as a
  standing requirement wherever P/D disaggregation is recommended: network isolation of the sidecar port plus
  `--enable-tls=prefill`. Date #242 (historical, partially fixed) and #979 (open) separately.

## Minor notes for the synthesizer
- Draft A adds detail the others lack and it is worth keeping: PR #1007's closing note and the merged chain that
  replaced it (#908 interface, #1080/#1081 store plumbing, #1135 the plugin, #1154 rename, #1218 runner wiring), and
  the `prefixMatchInfoProducerName` wiring requirement (without it the scorer silently falls back to approximate).
- Draft C's vendor benchmark figures (P90 TTFT ~35 s → ~120 ms, +151% throughput) must stay attributed as
  upstream-reported, never stated as independent measurement.
- All three correctly avoid citing pipeline artifacts. Keep it that way: cite source notes only.
