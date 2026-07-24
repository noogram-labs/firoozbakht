# preflight.md — re-attack loop, step 1 (refuse-fast gate)

**Molecule:** `reattack-20260724-c80a` · formula `converge-math-attack` · node `re-attack`
**Run:** node (spore `math-attack`)
**Date:** 2026-07-24

---

## 1. Driver-capability check

The re-attack loop nucleates children mid-run and waits on them. That requires a
DRIVER-CAPABLE executor, not the frozen `cosmon-remote` pilot surface. Both checks
were run before anything else.

| check | command | result |
|---|---|---|
| `cs wait` exists | `cs wait --help` | **exit 0 — PRESENT** |
| `cs run --resident` exists | `cs run --help \| grep -- '--resident'` | **match — PRESENT** |

**Verdict: driver-capable.** The loop is not refused; no collapse.

## 2. Bound check (defence in depth)

`cs spore validate` already fail-closes on `rounds > max_instances`. This is the
second lock, re-derived here from the sealed spore rather than trusted.

| quantity | source | value |
|---|---|---|
| `${rounds}` (runtime target) | `cs --json observe` → `variables.rounds` | **1** |
| `[spore.node.bounds].max_instances` (structural ceiling) | `spores/math-attack/spore.toml` §`re-attack` | **5** |

`1` is a positive integer and `1 ≤ 5`. **Bound check passes.** No collapse.

## 3. Early exit at `rounds = 1`

`rounds = 1` means the loop nucleates **nothing**. Round 1 — the spore's pinned
`skeptic` and `lean-probe` nodes, both upstream of this molecule and both already
`completed` — *is* the whole attack. This molecule reads their verdicts; it never
re-runs them.

Concretely, this run is exactly the v3.x single shot:

- no `attack-round-2/` directory is created,
- no `proof-attempt-*` / `lean-probe` / `skeptic` children are nucleated,
- the loop body is skipped and control goes straight to `emit-verdict`,
- round 1's own `skeptic` + `lean-probe` verdicts are folded as the final round.

See `rounds.md` for the ledger and the round-1 row.

---

## Exit criteria — met

- [x] Executor proven driver-capable (`cs wait` present, `cs run --resident` present); no collapse needed.
- [x] `${rounds}` parsed = 1, positive integer, `≤` the sealed `max_instances = 5`.
- [x] At `rounds = 1` nothing was nucleated, and `rounds.md` says so.
