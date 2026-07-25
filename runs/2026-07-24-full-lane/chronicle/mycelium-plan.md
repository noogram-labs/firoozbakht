# mycelium-plan.md — chronicle fold over the drained MATH-ATTACK DAG

**Molecule:** `mycelium-20260724-040a` (formula `mycelium`, crew_role `hypha`)
**Spore run:** node (spore `math-attack`, v4)
**Mission:** MATH-ATTACK Firoozbakht's conjecture — `p_{n+1}^{1/(n+1)} < p_n^{1/n}` for all n≥1.
**Target:** `docs/lore/CHRONICLES.md` — 0–3 entries, Feynman register, density over volume.

---

## 1. DAG state

23 molecules germinated in fleet `default`, project `firoozbakht-a668`.
**22 completed. 1 pending — this one (`chronicle`).** The DAG is fully drained upstream.

node is a completed molecule whose prompt carries no spore-node path
(its briefing was overwritten by `cs complete`); it maps to no `germ-.../<node>/`
output dir and produced no artifact under the run dir. Treated as out-of-band — nothing
to fold from it.

## 2. Topology (from `spores/math-attack/spore.toml`)

```
decompose → frame-deliberation → source-ledger → concept-cards
                                                      │
              ┌───────────────────────────────────────┼──────────────────┐
              │ (informal)                            │ (formal)         │
   proof-attempt ×3  ∥  notebooks ×3                lean-skeleton        │
              └──────────┬────────────┘               ├→ lean-probe      │
                      skeptic                         └→ red-team-corpus │
                         └──────────┬─────────────────────┘              │
                                 re-attack  ←── (loop, rounds=1)         │
                                     └──────→ evidence-gate ←────────────┘
                                                   ↓
                                              synthesize → write-paper
                                                   → citation-gate → editorial-verdict
                                                        → **chronicle** (here)
   trace : always-on root sidecar, ungated
```

## 3. Traversal order + artifact inventory

Read in this order — the fold is `(accumulated_chronicles, molecule_artifacts) → updated_chronicles`,
and reading downstream-last lets the verdict reframe what the early nodes were groping at.

| # | Node | Molecule | Artifacts (under `.cosmon/state/spore-runs/node/`) |
|---|------|----------|---------------------------------------------------------------------|
| 0 | trace | node | `trace/README.md`, `trace/briefs.md` |
| 1 | decompose | node | `decompose/decompose.md`, `verify_small_range.py` |
| 2 | frame-deliberation | node | `frame-deliberation/frame.md`, `synthesis.md`, `outcomes.md`, `moderator-crosscheck.md`, `responses/{wheeler,godel,feynman,popper,knuth}.md` |
| 3 | source-ledger | node | `source-ledger/source-ledger.md` |
| 4 | concept-cards | node | `concept-cards/INDEX.md` + `CC-01`…`CC-31`, `verify_cards.py`, `verify_cards.out` |
| 5 | proof-attempt ×3 | node / `de6b` / `ef6e` | `proof-attempt__{0,1,2}/proof-attempt-{0,1,2}.md` |
| 6 | notebooks ×3 | node / `0828` / `aea3` | `notebooks__{0,1,2}/findings-*.md`, `ffm.py`, `build_notebook.py`, `data/PROVENANCE.md` |
| 7 | lean-skeleton | node | `lean-skeleton/skeleton.md`, `sorry-audit.txt`, `build.log`, `lean/` (Mathlib toolchain — skip `.lake/`) |
| 8 | lean-probe | node | `lean-probe/lean-probe-report.md`, `sorry-audit.txt`, `crosscheck.txt`, `build.log` |
| 9 | skeptic | node | `skeptic/faults.md` |
| 10 | red-team-corpus | node | `red-team-corpus/coverage-report.md`, `corpus/README.md` |
| 11 | re-attack | `reattack-20260724-c80a` | `re-attack/preflight.md`, `rounds.md`, `synthesis.md` |
| 12 | evidence-gate | node | `evidence-gate/evidence-verdict.md`, `verify_gate.py`, `verify_gate.out` |
| 13 | synthesize | node | `synthesize/synthesis.md` |
| 14 | write-paper | `edit-20260724-9e06` | `write-paper/paper.md`, `log.md` |
| 15 | citation-gate | `cite-20260724-f7a3` | `citation-gate/verification-report.md`, `escalations.md`, `citations.json`, `evidence-axler-integers16-A22.txt` |
| 16 | editorial-verdict | `review-20260724-7b02` | *(node dir empty — verdict lives in the molecule's own `log.md`)* |

Plus, per molecule, the cosmon-side pair
`.cosmon/state/fleets/default/molecules/<id>/{briefing.md,log.md}`.

**Excluded from the read:** `lean-*/lean/.lake/packages/**` (vendored Mathlib, ~12 MB,
zero chronicle signal) and `__pycache__` / `.ruff_cache`.

## 4. Chronicle target

`docs/lore/CHRONICLES.md` does not yet exist in this galaxy — the fold creates it
with a header, then appends. Accumulated-chronicles input is therefore empty; the
fold is the first application.

## 5. Selection criterion (what earns an entry)

An entry must **illuminate a principle**, not report activity. Feynman register:
a smart 8-year-old must get the image. An 80 % rejection rate is healthy — routine
"node ran, node produced file" gets nothing. Candidate seams to test against the
criterion while reading:

- The attack ended **BLOCKED**, not proven — what does the machinery do when it
  does not win? Does the shape of the failure carry more information than a win would?
- `rounds = 1` cap on a v4 loop designed to re-attack: a bound hit on the first pass.
- The split gate (evidence before synthesis, citations after the paper) — a deadlock
  that was designed out before it could fire.
- Kernel-verdict invariant vs. a Lean skeleton full of `sorry`.

Selection is made **after** reading, not before. Zero entries is a valid outcome.
