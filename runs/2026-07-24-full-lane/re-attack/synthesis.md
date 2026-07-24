# synthesis.md — re-attack loop on Firoozbakht's conjecture

**Molecule:** `reattack-20260724-c80a` · node `re-attack` · spore `math-attack` v4
**Verdict:** **BLOCKED** · `exit_reason = "rounds-exhausted"` · rounds run 1 / target 1
**Date:** 2026-07-24

---

## The short version

The loop was set to run **one** round. One round is what already happened, upstream,
before this molecule existed. So the loop's job here was not to attack anything — it was
to *look at what round 1 came back with and say honestly whether it was good enough to
stop*. It was not. So the loop stops, and it stops **red**, with a named escalation.

Think of it as a single knock on a door that has been shut since 1982. Round 1 knocked.
The door did not open. There was no budget for a second knock, and the rule of this
loop is that a door that stayed shut is reported as *shut* — never as "we'll assume it
opened".

## What the two legs of round 1 actually came back with

**The Lean kernel leg — the machine.** `lake build` finished cleanly, exit 0, 8253 jobs.
But cleanly-built is not the same as proved. Exactly one `sorry` survives the build, and
it is *the* one: `theorem firoozbakht` at `Statement.lean:117`. The axiom print confirms
it — `sorryAx` is still in the dependency list. Verdict: **UNPROVABLE_IN_BUDGET**. That
phrase carries its full weight: it means *we did not find a proof*, and emphatically not
*the conjecture is false*. Nothing in this run touches the truth of the conjecture.

What the leg *did* close is real, and it is worth naming so the BLOCKED verdict is not
misread as a wasted run. Eleven new theorems, all `sorry`-free, all axiom-audited, none
leaning on `native_decide`. Among them:

- **Debt D2 closed.** The skeleton had flagged as its main risk that `Nat.nth` is
  noncomputable, so no finite range could be machine-checked at all. There is now a
  bridge (`nthPrime_succ_eq`) whose cost scales with the *gap* between primes rather than
  with the prime itself, and on top of it a certified 26-entry prime table.
- **`firoozbakht_of_le`** — the conjecture, proved outright, for every `1 ≤ n ≤ 25`, in
  the literal `rpow` form, and demonstrably not circular (its axiom line is clean).
- **One sufficient criterion** (`firoozbakht_of_gap_lt`) and **two refutation criteria**
  (`not_firoozbakht_of_barrier_le`, `not_firoozbakht_of_pow_le`) — the two one-directional
  tools a future proof leg and a future refutation leg would each need.

The report is itself honest about the size of that: `n ≤ 25` is set by compile time, and
next to Kourbatov's `4·10¹⁸` and Visser's `2⁶⁴` it is derisory. A certified range is a
*fidelity instrument*, not evidence. The whole difficulty lives in the tail.

**The skeptic leg — the adversary.** Two BLOCKERs, seven MAJORs, nine MINORs. Not clean.

The striking thing is *where* the faults are. The skeptic attacked every proof line by
line, re-derived every load-bearing number in independent code, re-ran all seven verifier
scripts, and fetched five primary sources — and found **no mathematical error in any
theorem**. Every proof is sound as written; every numeric claim reproduced.

Both BLOCKERs are instead claims the corpus makes *about its own trustworthiness*, and
both are false:

1. A provenance file says 44 rows of the load-bearing table were independently
   recomputed. Thirty were. A 47% overstatement, sitting in the record of the corpus's
   single weakest premise.
2. A notebook's headline new result — "the verified range itself excludes `C ≥ 1.1736`" —
   is an invalid inference. It says something about a curve fit and nothing about a
   `limsup`. And it was already being used to reallocate the polymer's budget.

That is exactly the class of fault a seal exists to stop: the mathematics is fine, the
self-description is not.

## Did the unproved list shrink, or did it churn?

**Neither — it is unchanged, and honestly so.** Seven declarations entered round 1
`sorry`'d and seven left `sorry`'d: the conjecture plus its six inheriting corollaries.
The corollaries were never independent targets; they inherit `sorryAx` and always would
until the head falls.

But "unchanged" here does not mean "no progress". The progress happened *beside* the
list rather than inside it: the surrounding scaffolding went from nothing to eleven
machine-checked theorems, a closed debt, and both directional criteria. That is the
signal a reader should take away — **the tooling advanced, the target did not move.**

The honest forecast: more rounds of the same shape would very likely keep producing
scaffolding without moving the head. The conjecture has been open since 1982, and the
obstruction is not a missing lemma that a re-read of the faults would surface. If a
round 2 is ever run, its value is in closing the two BLOCKERs and hardening the
criteria — not in expecting the `sorry` to fall.

## Why BLOCKED and not something softer

The stop condition is a conjunction: kernel PROVED **and** skeptic clean, in the same
round. Round 1 satisfied neither half. The `while` guard was `round < rounds`, i.e.
`1 < 1`, so the body never ran and no round 2 was nucleated — that is the designed
`rounds=1` behaviour (exactly the v3.x single shot), not a failure of the driver. The
executor *was* verified driver-capable (`cs wait` and `cs run --resident` both present);
the cap, not the machinery, is what stopped this.

The loop therefore terminated by **exhaustion**. Exhaustion is `BLOCKED`. There is no
path in this formula from "we ran out of rounds" to a pass.

## Escalation — what a human needs to decide

The full payload is in `reattack-verdict.json` under `escalation`. In brief:

- **Two BLOCKERs must be closed before any seal.** Neither needs new mathematics. One is
  a wrong row count; one is a sentence that must be deleted or restated. Both are
  currently load-bearing for downstream budget decisions, which is why they block.
- **Seven MAJORs must be closed before anything reaches the paper** — dominated by
  citation and cross-leg-consistency faults (a locator that does not exist in the
  version-of-record, two incompatible verification tiers for the same source, a leg
  crediting a sibling with a premise the sibling had retracted).
- **Seven theorems stay `sorry`'d**, headed by `Firoozbakht.firoozbakht` itself.
- **If a further formal attack is wanted**, re-germinate with `rounds ≥ 2` on a
  driver-capable executor — but go in expecting hardened criteria, not a closed
  conjecture.

The downstream `evidence-gate` reads `reattack-verdict.json`; `final_round.artifacts`
tells it which `faults.md` and which `lean-probe-report.md` are live. Under this
verdict it should fold round 1's kernel leg (unproved) and round 1's skeptic leg
(blockers) exactly as the v3.x single shot would.
