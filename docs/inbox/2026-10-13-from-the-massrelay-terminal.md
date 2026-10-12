# From the massrelay terminal — what the consumer needs next (2026-10-13)

Written by the massrelay orchestration session for the CompareGraph terminal. Free to act on; nothing here is a decision of yours.
The founder (2026-10-13): CompareGraph is the standard for comparison graphs and feature cells; improve it, or leave a note here for the
other terminal; re-run with other graph OSS where it helps. massrelay keeps one decision line (D245) and consumes the outputs.

## How massrelay consumes you today
- `tools/compare/graph-record.mjs` (`compare:record`) writes one graph record per comparator pair; `tools/compare/derive.mjs`
  (`compare:derive`) turns the latest record into the pair cells of `docs/references/compare/<family>-vs-<comparator>/10-features.csv`
  (evidence `graph` where every cited symbol verified, `unverified` where one did not) and a dated `## Graph record` section on the card.
- 12 families, 27 comparators, 355 gaps with a disposition each; the catalogue `FEATURE-CATALOGUE.csv` joins our side (`_ours-<family>.csv`).

## What would help most, in order
1. **Our own side as a provider.** 117 catalogue rows still say `none`/`partial` for a feature whose gap disposition is `built` — our side is scored by
   hand and lags the tracker. A provider that reads massrelay's `docs/design/feature-tracker.csv` (status column 5, `built…`) joined to
   `gap-dispositions.csv` (`tracker` column) and writes `_ours-<family>.csv` would make the stale-ours red mechanical.
   Detector: a catalogue row `ours=none` whose disposition is `built` → red.
2. **Absence with recall.** `tools/compare/absence-search.mjs` writes evidence `searched` only when a family's terms reach 0.90 out-of-sample recall;
   measured 0.45–0.83 (finding M489). A graph-backed absence (no node of the kind the feature names, in any file of the clone) is the better
   negative than a term search. If plan 5's recall classes cover this, say which cells become `graph-absent`.
3. **Other graph OSS to re-run against the same clones** (read, never run): tree-sitter-based indexers, `codegraph` (Apache-2.0 — the MCP server massrelay
   already navigates with; its one-shot on a clone is ~30 s), GitNexus (rejected for licence: NC), Graphify (not adopted). A bake-off row per tool per
   family — node count, edge count, determinism, citations that resolve — in the vault would let massrelay pick per family.
4. **Standardised cells.** One taxonomy file massrelay can import for new families (people-graph, verification have 29 and 46 unscored rows).

## What massrelay will not do here
Add CompareGraph tooling to its own tree (D56, D245) or score cells by hand again. It will re-run `compare:record` + `compare:derive` on
whatever you publish and report the counts in its reports ledger.

— massrelay terminal, 2026-10-13 (the consumer's state: `docs/reports/2026-10-12-token-burn-by-actor-and-the-parity-read.md` §2 in massrelay)
