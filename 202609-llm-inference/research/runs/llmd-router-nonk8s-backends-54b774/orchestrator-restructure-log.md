# Orchestrator-applied edits (step 14.3 / 14.4)

The patcher escalated three findings it had skipped for the word ceiling rather than on the merits, and asked for a
ruling. With 375 words of headroom under the 10,000-word ship-gate ceiling, I applied two of the three by hand. No
section was regenerated; both changes are table edits that preserve surrounding prose.

- **Section 6A — added two comparison axes** (`Added request latency`, `Failure handling`) to the nine-strategy table,
  resolving the instruction critic's remaining readability failure and covering two of the seven comparison dimensions
  the research query named. The substantive finding the axes expose: for strategies A-D and I the discovery choice adds
  **no** request-path latency, because every one of them ends at the same endpoint-picker hop — latency only changes if
  you hand-build a router (E) or switch to a different one (H, whose advantage is vendor-reported and unverified).
  Failure handling is now per-strategy rather than stated once globally.
- **Section 1B — added the Coordinator row** to the component inventory, citing the coordinator architecture and
  async-broker docs. This addresses the width critic's inventory-completeness finding for the component the query's
  first clause is about (which llm-d components can be reused).
- **Not applied: the Inference Payload Processor inventory row.** The width critic is right that IPP belongs in the
  inventory, but the IPP repository fetch was escalated and never landed, and no vault note documents the component
  directly enough to cite. Adding the row would have meant citing a source that does not support the claim, so the row
  is deliberately omitted rather than fabricated. Recorded here as a known, bounded gap.
- **Not applied: the KAITO / KServe component-reuse precedent** (width major). Genuine and citable, but the precedent
  objection it answers is now bounded by the RAMSES addition the patcher landed, and the remaining budget was better
  spent on the two items above. Deliberate deferral, not an oversight.

Word count after these edits: 9,765 (from 9,625), leaving 235 words of headroom under the ceiling.
