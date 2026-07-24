# Lean probe — Firoozbakht's conjecture

**Leg**: `lean-probe` (proof-term probe of the `math-attack` polymer)
**Crew role**: probe-engineer
**Backend**: `lean` (Lean 4 `v4.29.0` + Mathlib `v4.29.0`)
**Date**: 2026-07-24

---

## 0. Headline

| | |
|---|---|
| **`lake build` exit status** | **0** |
| Jobs | 8253, `Build completed successfully` |
| `sorry` warnings emitted | **exactly one** — `Statement.lean:117`, i.e. `theorem firoozbakht` |
| The conjecture itself | **UNPROVABLE_IN_BUDGET** — the `sorry` was *not* discharged |
| New `sorry`-free theorems added | **11** (plus a 26-entry prime table), all axiom-audited |

The mission was to attempt real proof terms for the `sorry`s in the `lean-skeleton`.
There was exactly one `sorry`: Firoozbakht's conjecture itself. **It was not
discharged.** That is recorded below as `UNPROVABLE_IN_BUDGET`, with the attempts
that were actually run and their verbatim compiler output — never as
"couldn't find a proof" dressed up as anything else, and never as "false".

The budget that could not close the conjecture *was* spent productively: it closed
**debt D2** of the skeleton (the "main risk item" flagged by `decompose` as OB-F8), and
added the two one-directional criteria a proof leg and a refutation leg actually need.
All of it is `sorry`-free and machine-checked.

> **LLM firewall.** Every verdict in this document is a `lake build` exit code or a
> `#print axioms` line quoted verbatim from that build. No claim rests on the model's
> assessment of its own output.

---

## 1. Per-theorem verdict

### 1.1 The target

| Theorem | Verdict | Evidence |
|---|---|---|
| `Firoozbakht.firoozbakht` | **UNPROVABLE_IN_BUDGET** | still `sorry`; `#print axioms` shows `sorryAx` |

The declaration is unchanged from the skeleton. Its axiom line in the build:

```
'Firoozbakht.firoozbakht' depends on axioms: [propext, sorryAx, Classical.choice, Quot.sound]
```

`sorryAx` present ⇒ **not proved**. This is the honest and expected outcome: the
conjecture has been open since 1982, and nothing in this run changes that. **It is not
a claim that the conjecture is false** — see §4 for what a refutation would have to
look like, which is itself now formalised.

The six corollaries (`firoozbakht_strictAnti`, `_int`, `_log`, `_antitone`, `_slack`,
`_gap`) inherit `sorryAx` exactly as they did before. **UNPROVABLE_IN_BUDGET**, by
inheritance — they were never independent targets.

### 1.2 What *was* proved (new this leg)

All `PROVED`: `lake build` exit 0, and `#print axioms` shows
`[propext, Classical.choice, Quot.sound]` — Mathlib's ordinary base, **no `sorryAx`**,
and no `native_decide` (which would have introduced `Lean.ofReduceBool`; the source is
grep-clean of it).

| Theorem | Statement | Verdict |
|---|---|---|
| `count_prime_eq_of_no_prime` | no prime in `[a,b)` ⇒ `π` constant across it | **PROVED** |
| `nthPrime_succ_eq` | next-prime step for the 1-indexed `nthPrime` | **PROVED** |
| `nthPrime_eq_1 … nthPrime_eq_26` | the table `p₁ = 2, …, p₂₆ = 101` | **PROVED** (26 decls) |
| `firoozbakht_of_le` | **the conjecture, for every `1 ≤ n ≤ 25`** | **PROVED** |
| `firoozbakht_gap_of_le` | same range, gap form `gₙ < Bₙ` | **PROVED** |
| `firoozbakht_iff_log` | literal form ⟺ log form | **PROVED** |
| `firoozbakht_of_gap_lt` | sufficient criterion `n·gₙ < pₙ·log pₙ` | **PROVED** |
| `not_firoozbakht_of_barrier_le` | refutation criterion (exact barrier) | **PROVED** |
| `not_firoozbakht_of_pow_le` | refutation criterion (ℕ-only form) | **PROVED** |
| `nthPrime_succ_le_of_prime` | minimality of `nthPrime` | **PROVED** |
| `nthPrime_succ_lt_two_mul` | Bertrand, strict: `p_{n+1} < 2pₙ` | **PROVED** |
| `firoozbakht_of_two_pow_le` | Bertrand closes `n` when `2ⁿ ≤ pₙ` | **PROVED** |

---

## 2. Debt D2 closed: certified finite verification

The skeleton handed down D2 as its main risk item:

> `Nat.nth` is `noncomputable`, so `decide` cannot evaluate `nthPrime`. Machine-checking
> the conjecture up to any `N₀` needs a bridge from `Nat.nth Nat.Prime` to an explicit
> sorted prime list plus a `Nat.count` certificate. **It remains open — a substantial
> engineering task.**

**It is now closed**, in `Firoozbakht/FiniteRange.lean`.

### 2.1 The bridge

`Nat.nth p` is defined by well-founded recursion over an infinite set and does not
reduce. The escape hatch is

```lean
Nat.nth_count : p n → Nat.nth p (Nat.count p n) = n
```

which trades `nth` for `count` — and `count` *is* computable. The naive move (`decide`
on `Nat.count Nat.Prime 97 = 24`) works, but it walks the whole range `[0, 97)`: a file
containing that single check compiled in 57 s against 21 s for the same file with the
check removed, i.e. **~36 s of kernel reduction for one entry**, and the cost grows with
the prime. That does not scale to a table.

The leg instead proves a step lemma whose cost is linear in the **gap**, not in the
prime:

```lean
theorem nthPrime_succ_eq {k p q : ℕ} (hk : 1 ≤ k) (hp : nthPrime k = p) (hq : Nat.Prime q)
    (hpq : p < q) (hgap : ∀ m, p < m → m < q → ¬ Nat.Prime m) :
    nthPrime (k + 1) = q
```

Each `hgap` obligation is an `interval_cases` over a gap of size `< 10` in this range —
milliseconds. `FiniteRange.lean` — the two bridge lemmas, the whole 26-entry table, *and*
the certified range — compiles in **6.5 s**. The `decide` route was not run to
completion; the single measured entry above (~36 s, for the largest prime) is the
evidence that it was the wrong approach, not a completed comparison.

No numeral is asserted anywhere: `p₁ = 2` comes from Mathlib, and every later entry is
derived from its predecessor by `nthPrime_succ_eq`.

### 2.2 The result

```lean
theorem firoozbakht_of_le (n : ℕ) (h1 : 1 ≤ n) (h2 : n ≤ 25) :
    (nthPrime (n + 1) : ℝ) ^ ((1 : ℝ) / ((n : ℝ) + 1)) <
      (nthPrime n : ℝ) ^ ((1 : ℝ) / (n : ℝ))
```

`sorry`-free, and **it does not depend on `Firoozbakht.firoozbakht`** — the audit line
proves it is not circular:

```
'Firoozbakht.firoozbakht_of_le' depends on axioms: [propext, Classical.choice, Quot.sound]
```

This is a genuine unconditional fragment of an open problem: the conjecture, proved
outright, for 25 consecutive indices, in the literal `rpow` form (not a surrogate).

### 2.3 Honest sizing of that result

The bound `n ≤ 25` is set by compile time, not by mathematics — the bridge is uniform
in `n` and the table extends mechanically. And the range is *derisory* next to the
literature: Kourbatov (2015) verified `pₙ < 4·10¹⁸`; Visser (2019) covers `p < 2⁶⁴`.
The value here is **not** the range. It is that (a) the formal method now exists, and
(b) a formal check is a different kind of object from a computational one — it is the
same kernel that checks the bridges.

No finite range bears on the conjecture. The whole difficulty is in the tail, where
`attack.md` §4's heuristic count of expected violations diverges like `e·log log X`.
**A certified range is a fidelity instrument, not evidence.**

---

## 3. The attempts on the conjecture, recorded

Kept in `probe-attempts/` and **deliberately excluded from the library target** — they
are a record, not a result. Two of the three end in `sorry` or an error by design.

### Attempt 1 — `exact?` with the project in scope

```
Try this:
  [apply] exact firoozbakht n hn
```

Mathlib's search closed the goal **by citing the `sorry`-ed declaration itself**.
Circular; worthless. Recorded because it is exactly the failure mode
`evidence-gate` should expect from an automated proof leg: a search that "succeeds"
against its own open hypothesis. *An `exact?` success is not a proof.*

### Attempt 2 — `exact?` with the goal restated from Mathlib alone

Same goal, phrased directly in `Nat.nth Nat.Prime` so the `sorry`-ed name is out of
scope:

```
error: `exact?` could not close the goal. Try `apply?` to see partial suggestions.
```

Mathlib contains nothing that implies Firoozbakht's conjecture. A `grep -ri firoozbakht`
over the whole Mathlib tree returns **zero files**.

### Attempt 3 — reduction through the proved sufficient criterion

`apply firoozbakht_of_gap_lt hn` succeeds and leaves, verbatim from `trace_state`:

```
n : ℕ
hn : 1 ≤ n
⊢ ↑n * (↑(nthPrime (n + 1)) - ↑(nthPrime n)) < ↑(nthPrime n) * Real.log ↑(nthPrime n)
```

i.e. `n·gₙ < pₙ·log pₙ`. This is the open analytic problem, unchanged in difficulty:
it is the linearised barrier of `attack.md` §2, and proving it for all `n` requires
the every-gap control at leading constant 1 that §3 of that document shows is beyond
unconditional bounds, beyond the standard RH route, and beyond Cramér as usually
stated. The reduction is a change of shape, not of difficulty.

### Attempt 4 — Bertrand's postulate, and its exact reach

Mathlib *does* have Bertrand. The leg proved the strict consecutive-prime form and the
conditional it yields:

```lean
theorem nthPrime_succ_lt_two_mul {n : ℕ} (hn : 1 ≤ n) : nthPrime (n + 1) < 2 * nthPrime n
theorem firoozbakht_of_two_pow_le {n : ℕ} (hn : 1 ≤ n) (hbig : 2 ^ n ≤ nthPrime n) : …
```

Both `PROVED`. The hypothesis `2ⁿ ≤ pₙ` holds at `n = 1` and fails at `n = 2`
(`p₂ = 3 < 4`) — and that failure is itself machine-checked in `Criteria.lean`. Since
`pₙ ~ n log n` is far below `2ⁿ`, Bertrand closes **exactly one index**. The
inadequacy of the Bertrand route is thus a theorem of this development, not a remark.

### Why the attempt stops here

The conjecture reduces (exactly, `firoozbakht_iff_gap`, proved in the skeleton) to
`gₙ < Bₙ` with `Bₙ = (log pₙ)² − log pₙ − 1 − o(1)`. Every route in Mathlib's reach
gives gap bounds of power size or existence-of-small-gaps results, both of which are
the wrong shape. Closing it in Lean would require first formalising a theorem nobody
has proved on paper. **UNPROVABLE_IN_BUDGET** is not a statement about the budget being
small; it is a statement about the problem being open.

---

## 4. What a refutation would have to be

The polymer's red-team legs get a formal target. Both are `PROVED` and both are
**exact** — no asymptotic surrogate is admissible:

```lean
theorem not_firoozbakht_of_barrier_le {n : ℕ} (hn : 1 ≤ n)
    (h : barrier n ≤ (nthPrime (n + 1) : ℝ) - (nthPrime n : ℝ)) : ¬ (F1 at n)

theorem not_firoozbakht_of_pow_le {n : ℕ} (hn : 1 ≤ n)
    (h : nthPrime n ^ (n + 1) ≤ nthPrime (n + 1) ^ n) : ¬ (F1 at n)
```

The second is pure ℕ arithmetic — the right target for a computational search, since it
needs no real arithmetic and no `Real.log` at all. Note what it demands: the *index*
`n = π(pₙ)` must be certified, not just the gap. A large gap alone is not a
counterexample.

Explicitly **not** admissible as a refutation: exhibiting a gap above
`(log pₙ)² − log pₙ − 1`. That is the asymptotic surrogate (`attack.md` eq. 3), not
`Bₙ`, and the difference is `O(1/log pₙ)` — which is exactly the size of the margin
being contested.

---

## 5. Independent cross-check (outside Lean)

Lean checks that the proofs follow. It does not check that the *numerals* are the
primes anyone means. Cross-checked against `sympy`'s sieve, independent of Mathlib:

```
prime table p_1..p_26 matches sympy sieve: True
n in 1..25 failing p_{n+1}^n < p_n^(n+1): []
n with 2^n <= p_n (n < 40): [1]
n < 72849 where linearised criterion n*g_n < p_n*log p_n FAILS: count=2 first=[2, 4]
```

Three readings:

1. The Lean prime table is the real one, and the certified range really does hold.
2. Bertrand's reach is exactly `{1}`, matching the machine-checked `n = 2` failure.
3. **The linearised sufficient criterion is very nearly the conjecture.** Over the
   first ~73 000 indices it fails at only `n = 2` and `n = 4`, both inside the range
   already proved outright by `firoozbakht_of_le`. So `firoozbakht_of_gap_lt` is not a
   lossy detour: for `n ≥ 5` (up to the tested range) proving `n·gₙ < pₙ·log pₙ` would
   *be* proving the conjecture. That is a useful thing for a proof leg to know — and
   equally a warning that the criterion is no easier than the target.

Raw output: `crosscheck.txt`.

---

## 6. Build, verbatim

Full transcript: `build.log` (38 lines, from a clean rebuild of the `Firoozbakht`
library against the cached Mathlib).

```
⚠ [8248/8253] Built Firoozbakht.Statement (11s)
warning: Firoozbakht/Statement.lean:117:8: declaration uses `sorry`
✔ [8249/8253] Built Firoozbakht.FiniteRange (6.5s)
✔ [8250/8253] Built Firoozbakht.Criteria (9.6s)
ℹ [8251/8253] Built Firoozbakht.Audit (17s)
…
Build completed successfully (8253 jobs).
lake build exit status: 0
```

### 6.1 Axiom audit — the separation, machine-checked

`Audit.lean` is part of the library, so `#print axioms` runs on **every** build and
cannot silently rot. Grouped verbatim from `build.log`:

**Must depend on `sorryAx` (open conjecture + corollaries) — 7 declarations:**
`firoozbakht`, `firoozbakht_strictAnti`, `firoozbakht_int`, `firoozbakht_log`,
`firoozbakht_antitone`, `firoozbakht_slack`, `firoozbakht_gap`. All show
`[propext, sorryAx, Classical.choice, Quot.sound]`. ✓ as expected.

**Must NOT depend on `sorryAx` — 24 declarations**, every one showing
`[propext, Classical.choice, Quot.sound]`: the 12 skeleton bridges and index facts,
plus 12 new entries — the 11 new theorems of §1.2, and `nthPrime_eq_26`, which
spot-audits the prime table at its last entry (by construction it depends on all 25
predecessors). ✓

**Grep-clean.** `grep -n "sorry\|native_decide\|axiom "` over the four library sources
returns exactly one non-prose hit: `Statement.lean:120`, the tactic `sorry` inside
`theorem firoozbakht`. No `native_decide` anywhere, so no `Lean.ofReduceBool` enters
the trusted base — the finite verification is kernel-checked, not compiler-trusted.

---

## 7. Toolchain

- `leanprover/lean4:v4.29.0`
- Mathlib `v4.29.0`, revision pinned in `lean/lake-manifest.json`
- `lake exe cache get` against `~/.cache/mathlib`
- Clean rebuild of the `Firoozbakht` library (Mathlib oleans cached): **94 s** wall clock, measured

Every Mathlib name used was resolved against the actual checkout in
`.lake/packages/mathlib` before being written — never recalled. The names introduced
this leg: `Nat.nth_count`, `Nat.count_nth_of_infinite`, `Nat.count_succ`,
`Nat.count_strict_mono`, `Nat.nth_le_nth`, `Nat.infinite_setOf_prime`,
`Nat.exists_prime_lt_and_le_two_mul`, `Real.log_le_sub_one_of_pos`, `Real.log_div`.

---

## 8. Files emitted

| Path | What it is |
|---|---|
| `lean-probe-report.md` | this document |
| `lean/Firoozbakht/FiniteRange.lean` | the `Nat.nth`→numeral bridge, prime table, certified range (D2) |
| `lean/Firoozbakht/Criteria.lean` | sufficient criterion, refutation criteria, the Bertrand route |
| `lean/Firoozbakht/Statement.lean` | unchanged from the skeleton (the single `sorry`) |
| `lean/Firoozbakht/Audit.lean` | extended `#print axioms` separation, 31 declarations |
| `lean/probe-attempts/Attempt{1,2,3}_*.lean` | the recorded attempts, outside the library target |
| `lean/probe-attempts/Verify.lean` | step-2 verification pass (§11), outside the library target |
| `build.log` | verbatim clean-build transcript, exit status 0 |
| `crosscheck.txt` | independent `sympy` cross-check output |

---

## 8b. Step-2 verification pass

`probe-attempts/Verify.lean` re-checks, in a file the library does not depend on, the
five claims this report makes that a reader should not have to take on trust. It
compiles with **no errors and no warnings**:

1. **`firoozbakht_of_le` is in the literal `rpow` form.** `#check` output, verbatim:
   `∀ (n : ℕ), 1 ≤ n → n ≤ 25 → ↑(nthPrime (n + 1)) ^ (1 / (↑n + 1)) < ↑(nthPrime n) ^ (1 / ↑n)`.
   Not the integer surrogate.
2. **The range is instantiable at both endpoints** (`n = 1` and `n = 25`), so it is not
   vacuous or off-by-one.
3. **The table endpoints** `nthPrime 1 = 2` and `nthPrime 26 = 101` hold.
4. **Re-audit from outside**: `firoozbakht_of_le`, `nthPrime_eq_26` and
   `firoozbakht_of_gap_lt` all print `[propext, Classical.choice, Quot.sound]`.
5. **The refutation criterion really is the contrapositive.** The following typechecks,
   i.e. a counterexample inside the certified range would yield `False`:

   ```lean
   example (n : ℕ) (h1 : 1 ≤ n) (h2 : n ≤ 25)
       (hbad : (nthPrime n) ^ (n + 1) ≤ (nthPrime (n + 1)) ^ n) : False :=
     not_firoozbakht_of_pow_le h1 hbad (firoozbakht_of_le n h1 h2)
   ```

   This is the strongest single check in the leg: it wires the refutation criterion and
   the certified range together and shows the kernel agrees they are incompatible.

### Report claims corrected during this pass

Recorded rather than silently fixed, per the polymer's honesty discipline:

| Claim as first written | Corrected to | How |
|---|---|---|
| "9 new theorems" | **11** | counted against §1.2 |
| "`decide` route ≈ 15 min" (extrapolated) | one measured entry at ~36 s; extrapolation withdrawn | the full route was never run |
| "`build.log`, 39 lines" | **38** | `wc -l` |
| "clean rebuild ~45 s" | **94 s** | timed |

---

## 9. Debts handed on

| # | Debt | Owner |
|---|---|---|
| D1 | The asymptotic expansion of `Bₙ` (needs PNT with lower-order terms) is **still not formalised**. Unchanged from the skeleton. | `proof-attempt`, `skeptic` |
| **D2** | **CLOSED.** Bridge + certified range delivered. | — |
| D2′ | The certified range stops at `n ≤ 25` for compile-time reasons only. Extending it is mechanical (one `nthPrime_succ_eq` per prime) but does not become mathematically interesting until it is automated to reach `10⁶`+, at which point it is still nowhere near Kourbatov's `4·10¹⁸`. Low value; recorded so nobody re-discovers it as a gap. | a future Lean leg |
| D3 | Kourbatov's `−1` necessary and `−1.17` sufficient conditions are **still not formalised**. `firoozbakht_of_gap_lt` is a *different*, weaker, elementary criterion — it must not be cited as Kourbatov's. | `source-ledger`, `citation-gate` |
| D4 | Attribution of the conjecture is still not asserted in Lean. Unchanged. | `source-ledger` |

---

## 10. Honest endpoint

The `sorry` is still there, and it is still the point.

What this leg adds is that the space around it is now better mapped *inside the
kernel*: the conjecture is proved for 25 indices, the shape of a valid refutation is
formalised, one classical route (Bertrand) is proved to close exactly one index, and
the elementary sufficient criterion is proved — together with the numerical evidence
that it is essentially as hard as the conjecture itself.

**`Firoozbakht.firoozbakht`: UNPROVABLE_IN_BUDGET. Not false. Not proved. Open.**
