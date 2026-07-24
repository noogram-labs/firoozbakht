# rounds.md — re-attack round ledger

**Molecule:** `reattack-20260724-c80a` · formula `converge-math-attack` · node `re-attack`
**Run dir:** `<repo>/.cosmon/state/spore-runs/node
**Date:** 2026-07-24

## Loop parameters

| parameter | value |
|---|---|
| `rounds` (runtime target) | **1** |
| `max_instances` (sealed structural ceiling) | 5 |
| `formal_backend` | `lean` |
| `subquestions` | `first-failure-maximality`, `RH-conditional-bound`, `unconditional-verified-range` |
| executor | driver-capable (`cs wait` ✓, `cs run --resident` ✓) |

**`rounds=1`: no re-attack round nucleated (v3.x single shot).** Round 1 is the spore's
pinned upstream `skeptic` + `lean-probe` nodes. They were READ, never re-run. No
`attack-round-K/` directory exists and none was created.

## Pinned artifacts every round re-uses (never re-opened)

| artifact | path | status |
|---|---|---|
| concept-cards | `germ-…/concept-cards/` | pinned |
| source-ledger | `germ-…/source-ledger/` | pinned |
| lean-skeleton | `germ-…/lean-skeleton/` | **FROZEN** — theorem statement is the fidelity anchor |
| red-team-corpus | `germ-…/red-team-corpus/` | not re-opened (tests the STATEMENT, not proof progress) |

## Round ledger

| round | attempt ids | probe id | skeptic id | kernel | skeptic | converged? |
|---|---|---|---|---|---|---|
| 1 | `proof-attempt__0/1/2` (pinned upstream, v3.x names) | node | node | **UNPROVABLE_IN_BUDGET** | **blockers (2)** | **NO** |

### Round 1 — verdicts, as read from the upstream artifacts

**Kernel** — `lean-probe/lean-probe-report.md` (present, well-formed).
`lake build` exit **0**, 8253 jobs, build completed successfully — but the grep is
**not** clean: exactly one `sorry` remains, `Statement.lean:117`, which is
`theorem firoozbakht` itself. `#print axioms Firoozbakht.firoozbakht` shows `sorryAx`.
Rule: `PROVED` iff exit 0 **AND** grep-clean of sorry/axiom. Exit 0 holds, grep-clean
does **not**. ⇒ kernel = **UNPROVABLE_IN_BUDGET**, never conflated with "false".

*What the leg did close* (all `sorry`-free, axiom-audited, no `native_decide`):
11 new theorems + a 26-entry certified prime table, closing skeleton debt **D2**
(`firoozbakht_of_le`: the conjecture proved outright for `1 ≤ n ≤ 25` in the literal
`rpow` form, non-circular), plus a sufficient criterion (`firoozbakht_of_gap_lt`) and two
refutation criteria (`not_firoozbakht_of_barrier_le`, `not_firoozbakht_of_pow_le`).
Real progress — but the target theorem is untouched.

**Skeptic** — `skeptic/faults.md` (present, well-formed, 37814 bytes).
§0 Verdict: *"The seal is BLOCKED. Two BLOCKER findings, seven MAJOR, nine MINOR."*
⇒ skeptic = **blockers**, NOT clean.

- **BLOCKER-1** — `notebooks__2/data/PROVENANCE.md` overstates by 47% how much of the
  load-bearing table was independently recomputed ("rows 1–44" should read "rows 1–30").
- **BLOCKER-2** — `notebooks__2/findings-2.md` §4's *"the verified range itself excludes
  C ≥ 1.1736"* is an invalid inference, and it is the leg's advertised novelty.

Notably the skeptic found **no mathematical error in any theorem**: every proof in
`proof-attempt__0/1/2` is sound as written and every numeric claim reproduced. The
BLOCKERs are claims about the corpus's own epistemic standing, and both are false.

### `unproved-1` — theorems still `sorry`'d after round 1

1. `Firoozbakht.firoozbakht` — the conjecture, literal `rpow` form (`Statement.lean:117`).
2. `Firoozbakht.firoozbakht_strictAnti` — inherits.
3. `Firoozbakht.firoozbakht_int` — inherits.
4. `Firoozbakht.firoozbakht_log` — inherits.
5. `Firoozbakht.firoozbakht_antitone` — inherits.
6. `Firoozbakht.firoozbakht_slack` — inherits.
7. `Firoozbakht.firoozbakht_gap` — inherits.

(Count: 1 independent target + 6 corollaries inheriting `sorryAx`.)

### `faults-1` — BLOCKER + MAJOR findings carried forward

| id | severity | artifact |
|---|---|---|
| 1 | **BLOCKER** | `notebooks__2/data/PROVENANCE.md` — 47% overstatement of independent recomputation |
| 2 | **BLOCKER** | `notebooks__2/findings-2.md` §4 — invalid `C ≥ 1.1736` exclusion sold as the novelty |
| 3 | MAJOR | `proof-attempt-0.md` §0 — vacuous conclusion sold as a headline verdict |
| 4 | MAJOR | `proof-attempt-0.md` §8 — attributes to `notebooks__0` a premise it had retracted |
| 5 | MAJOR | `notebooks__0/ffm.py` — docstring carries a formally retracted claim |
| 6 | MAJOR | ledger §3.5 / CC-19 / PA-0 / PA-2 — "Cor. 3.5" locator absent from the version-of-record |
| 7 | MAJOR | `proof-attempt-1.md` §8.1 — two incompatible verification tiers for `schoenfeld1976sharper` |
| 8 | MAJOR | `proof-attempt-1.md` §2 — undated superlative survey claim inside a V0 row |
| 9 | MAJOR | `notebooks__2/findings-2.md` §8-5 — budget call rests on a 30-decade extrapolation |

These are `faults-1`. Under `rounds=1` they are **not** fed to a round 2 — there is no
round 2. They are the escalation payload of the BLOCKED verdict instead.

## Stop-condition evaluation

Stop condition: `kernel == PROVED AND skeptic == clean`.
Round 1: `UNPROVABLE_IN_BUDGET` ∧ `blockers` ⇒ **condition FALSE**.

The `while` guard is `round < rounds` ⇒ `1 < 1` ⇒ **false**, so the loop body never
executes. The loop therefore terminates by **cap exhaustion**, not by fixpoint.

`exit_reason = "rounds-exhausted"` · `verdict = BLOCKED`. **Never a silent pass.**

---

## Loop execution trace (step 3, `while` discipline)

```
round = 1
while round < 1:          # 1 < 1  =>  FALSE, body never entered
    ...                   # not executed
# loop exits by CAP, not by fixpoint
if not (kernel[1] == PROVED and skeptic[1] == clean):   # TRUE (UNPROVABLE_IN_BUDGET, blockers)
    exit_reason = "rounds-exhausted"                    # verdict = BLOCKED
```

Invariants honoured, each checked explicitly:

| # | mandatory invariant | status |
|---|---|---|
| 1 | every nucleation carries `--decayed-from` + `--tag reattack-round:K` | vacuous — **zero nucleations** |
| 2 | never advance a round while the previous runs (`cs wait` first) | vacuous — no round advanced |
| 3 | never skip the `rounds` check | **enforced** — `1 < 1` is the guard that stopped the loop |
| 4 | a missing `faults.md` / `lean-probe-report.md` is NOT converged | both present and well-formed; read, not re-run |
| 5 | never re-open `lean-skeleton` or `red-team-corpus` | **not re-opened** — statement stayed FROZEN |
| 6 | round K writes only under `attack-round-K/` | vacuous — no `attack-round-*/` created; nothing written to round-1 paths |

Round 1's artifacts were read strictly read-only. No file under `skeptic/`,
`lean-probe/`, `proof-attempt__*/`, `notebooks__*/`, `lean-skeleton/` or
`red-team-corpus/` was modified by this molecule.

**Result: exhaustion at the cap. `exit_reason = "rounds-exhausted"`, verdict `BLOCKED`.**
