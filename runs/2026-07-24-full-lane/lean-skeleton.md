# Lean skeleton — Firoozbakht's conjecture (fidelity anchor)

**Leg**: `lean-skeleton` (fidelity anchor of the `math-attack` polymer)
**Crew role**: kernel-engineer
**Backend**: `lean` (Lean 4 + Mathlib)
**Date**: 2026-07-24

---

## 0. What this leg is for, and what it deliberately does not do

The polymer's invariant is that **no target is called proved on an LLM's say-so**.
That invariant is only worth anything if the *statement* being discussed is the right
one. A downstream proof leg that closes a subtly mis-stated theorem has produced
nothing; worse, it has produced something that *looks* like everything.

So this leg has exactly one job: **transcribe Firoozbakht's conjecture into Lean 4
faithfully**, and pin every reformulation to it with a machine-checked equivalence.

It does **not** attempt to prove the conjecture. `theorem firoozbakht` ends in
`sorry`, and that `sorry` is the honest record of an open problem — not a placeholder
awaiting routine discharge.

> **Red flag for `evidence-gate`**: if any downstream leg reports that
> `Firoozbakht.firoozbakht` compiles `sorry`-free, that is a signal to **re-audit the
> statement**, not a success. It means either the statement drifted or the proof is
> unsound.

---

## 1. The statement of record

Firoozbakht conjectured that `n ↦ pₙ^(1/n)` is strictly decreasing:

$$
p_{n+1}^{1/(n+1)} < p_n^{1/n} \qquad \text{for all } n \ge 1,
$$

with `pₙ` the `n`-th prime, **1-indexed** (`p₁ = 2`).

In Lean (`lean/Firoozbakht/Statement.lean`, §2):

```lean
theorem firoozbakht (n : ℕ) (hn : 1 ≤ n) :
    (nthPrime (n + 1) : ℝ) ^ ((1 : ℝ) / ((n : ℝ) + 1)) <
      (nthPrime n : ℝ) ^ ((1 : ℝ) / (n : ℝ)) := by
  sorry
```

Three fidelity choices, each deliberate:

1. **`Real.rpow`, not `Monoid.npow`.** The conjecture as posed is about *real*
   `n`-th roots. The integer form `p_{n+1}^n < p_n^(n+1)` is easier to work with, but
   it is a *derived* form. Making it the anchor would silently substitute a different
   (albeit equivalent) statement for the one under attack. So the anchor is the
   literal form, and the integer form is obtained from it via a proved bridge.
2. **1-indexed.** Mathlib's `Nat.nth Nat.Prime` is 0-indexed. See §2.
3. **`1 ≤ n` guard.** Forced by the index shift; see §2.

---

## 2. The index-convention hazard (obligation OB-F1 from `decompose`)

`Nat.nth Nat.Prime 0 = 2`. The conjecture is stated with `p₁ = 2`. The shift is done
by

```lean
noncomputable def nthPrime (n : ℕ) : ℕ := Nat.nth Nat.Prime (n - 1)
```

Because ℕ subtraction is truncated, `nthPrime 0 = nthPrime 1 = 2` — a genuine
aliasing. It is harmless under `1 ≤ n`, but it is exactly the kind of silent aliasing
that lets a mis-stated theorem compile. Rather than assert the convention, the file
**discharges it by evaluation**:

```lean
example : nthPrime 1 = 2 := Nat.nth_prime_zero_eq_two
example : nthPrime 2 = 3 := Nat.nth_prime_one_eq_three
example : nthPrime 3 = 5 := Nat.nth_prime_two_eq_five
example : nthPrime 4 = 7 := Nat.nth_prime_three_eq_seven
example : nthPrime 5 = 11 := Nat.nth_prime_four_eq_eleven
example : nthPrime 0 = nthPrime 1 := rfl   -- the aliasing, recorded on purpose
```

and §5 of the file additionally records that a one-step index shift produces a
*different* statement (`3^0 < 2^1` is true and vacuous; `3^2 < 2^3` is false), so a
silent shift cannot pass as "the same conjecture".

**OB-F1 is closed.**

---

## 3. The reformulations, and why they are safe to use

Besides the anchor F1, five further forms circulate in the literature and in the
sibling `decompose` and `attack` documents. Each is a corollary of `firoozbakht`
*through a proved `iff`*. The `iff`
lemmas carry no `sorry`; the corollaries carry exactly the one `sorry` of the
conjecture and nothing more.

| Form | Lean name | Statement | Bridge (proved) |
|------|-----------|-----------|-----------------|
| F1 (anchor) | `firoozbakht` | `p_{n+1}^(1/(n+1)) < pₙ^(1/n)` (`rpow`) | — |
| F1′ strict-anti | `firoozbakht_strictAnti` | `StrictAnti (k ↦ p_{k+1}^(1/(k+1)))` | `strictAnti_nat_of_succ_lt` |
| F2 integer | `firoozbakht_int` | `p_{n+1}^n < pₙ^(n+1)` (ℕ only) | `firoozbakht_iff_int`, via `rpow_inv_lt_rpow_inv_iff_pow_lt_pow` |
| F3 log | `firoozbakht_log` | `n·log p_{n+1} < (n+1)·log pₙ` | `pow_lt_pow_iff_log` |
| F4 sequence | `firoozbakht_antitone` | `a_{n+1} < aₙ`, `aₙ := (log pₙ)/n` | inline (`div_lt_div_iff₀`) |
| F5 slack | `firoozbakht_slack` | `Sₙ > 0` | `slack_eq` |
| Gap | `firoozbakht_gap` | `gₙ < Bₙ := pₙ(pₙ^(1/n) − 1)` | `firoozbakht_iff_gap` |

Two notes on the bridges:

* **`rpow_inv_lt_rpow_inv_iff_pow_lt_pow` is stated for arbitrary positive reals**,
  not for primes. This is on purpose: the proof then cannot accidentally lean on a
  property of primes, so the equivalence is visibly about exponentiation alone.
  The move is: raise both sides to the power `n(n+1) > 0`.
* **`firoozbakht_iff_gap` does not route through the integer form.** It is the direct
  rearrangement `p_{n+1} < pₙ^((n+1)/n) = pₙ + Bₙ`. This matters because the gap form
  is what connects the conjecture to Cramér-type prime-gap analysis, and a bridge that
  detoured through ℕ would obscure that the barrier `Bₙ` is *exact*, not asymptotic.

### Why the gap form is the load-bearing one

`firoozbakht_iff_gap` is the Lean counterpart of equation (1) of the sibling `attack`
write-up, and of §0.2 of `decompose`:

$$
\text{Firoozbakht at } n \iff g_n < B_n := p_n\left(p_n^{1/n}-1\right).
$$

The equivalence is **exact at every `n`** — no asymptotics, no error term. That is
what turns the conjecture from a curiosity about exponentials into a Cramér-type
prime-gap problem with leading constant 1. Having it machine-checked means downstream
legs may move between "root form" and "gap form" without re-arguing the reduction.

Note carefully what is *and is not* formalised here: the **exact** equivalence
`F1 ⟺ gₙ < Bₙ` is proved. The **asymptotic expansion**
`Bₙ = (log pₙ)² − log pₙ − 1 − 3/log pₙ − … ` (equation (3) of `attack`, §2 of
`decompose`) is **not** formalised — it needs the prime number theorem with
lower-order terms, which is not the business of a fidelity anchor. Any downstream leg
citing that expansion is citing the natural-language documents, not the kernel.

---

## 4. Sanity checks (the statement is testable, and it is tested)

The file closes with the first four instances of the conjecture, in the integer form
where the arithmetic is decidable:

```
n = 1 :  3^1 < 2^2       (3 < 4)     ✓
n = 2 :  5^2 < 3^3       (25 < 27)   ✓
n = 3 :  7^3 < 5^4       (343 < 625) ✓
n = 4 : 11^4 < 7^5       (14641 < 16807) ✓
```

These are **not** evidence for the conjecture. They are evidence that the Lean
statement is the intended statement: the margins are narrow (`3 < 4`, `25 < 27`), so
most plausible mis-statements — an index shift, a swapped inequality, an off-by-one in
an exponent — would fail here rather than compile silently.

---

## 5. Build / verification status — reported honestly

**`lake build` exit status: 0.** 8251 jobs, `Build completed successfully`. Full
transcript in `build.log` (sibling of this file).

The compiler emits **exactly one** `sorry` warning:

```
warning: Firoozbakht/Statement.lean:117:8: declaration uses `sorry`
```

Line 117 is `theorem firoozbakht` — the conjecture. Nothing else in the file uses
`sorry`.

### 5.1 Axiom audit (`Firoozbakht/Audit.lean`)

A `#print axioms` on every declaration, so the separation is machine-checked rather
than asserted. Verbatim from the build:

**Depend on `sorryAx` — correct, these are the open conjecture and its corollaries:**

```
firoozbakht           [propext, sorryAx, Classical.choice, Quot.sound]
firoozbakht_strictAnti[propext, sorryAx, Classical.choice, Quot.sound]
firoozbakht_int       [propext, sorryAx, Classical.choice, Quot.sound]
firoozbakht_log       [propext, sorryAx, Classical.choice, Quot.sound]
firoozbakht_antitone  [propext, sorryAx, Classical.choice, Quot.sound]
firoozbakht_slack     [propext, sorryAx, Classical.choice, Quot.sound]
firoozbakht_gap       [propext, sorryAx, Classical.choice, Quot.sound]
```

**Free of `sorryAx` — the bridges and the index convention, i.e. everything this leg
actually claims to have established:**

```
rpow_inv_lt_rpow_inv_iff_pow_lt_pow   [propext, Classical.choice, Quot.sound]
firoozbakht_iff_int                   [propext, Classical.choice, Quot.sound]
pow_lt_pow_iff_log                    [propext, Classical.choice, Quot.sound]
slack_eq                              [propext, Classical.choice, Quot.sound]
firoozbakht_iff_gap                   [propext, Classical.choice, Quot.sound]
prime_nthPrime, two_le_nthPrime, nthPrime_pos, one_lt_nthPrime,
nthPrime_pos_real, one_lt_nthPrime_real, nthPrime_lt_succ
                                      [propext, Classical.choice, Quot.sound]
```

`[propext, Classical.choice, Quot.sound]` is Mathlib's ordinary axiom base — the
absence of `sorryAx` is the claim being made.

The index-convention `example`s and the four numeric sanity checks of §4 are
anonymous (`example`), so they cannot be `#print axioms`'d; they are covered by the
whole-file guarantee that the only `sorry` is at line 117.

### 5.2 What the audit does *not* establish

It establishes that `firoozbakht_int` etc. follow from `firoozbakht` in Lean's kernel.
It says nothing about whether `firoozbakht` is true. `sorryAx` is inconsistent, so
every declaration in the first block is, formally, worthless as evidence — which is
precisely the intended reading.

### 5.3 Independent cross-check of the bridges (outside Lean)

A proved `iff` in Lean is only as good as the statement on each side of it. To catch a
bridge that is internally valid but wired to the wrong formula, the four forms F1, F2,
F3 and the gap form were evaluated **numerically and independently** (Python /
`mpmath` at 60 digits, `sympy` for primes) for every `n` from 1 to 2000:

```
disagreements among F1/F2/F3/gap over n = 1..2000:  none
```

All four also hold throughout that range, consistent with the conjecture being open
rather than falsified at small `n`. This is corroboration of the *transcription*, not
evidence for the conjecture.

The barrier's documented asymptotic `Bₙ ≈ (log pₙ)² − log pₙ − 1` was spot-checked the
same way — `B/(L²−L−1)` = 1.67, 1.09, 1.0006, 0.9994 at `n` = 10, 10², 10⁴, 10⁶ —
converging to 1 as claimed. **This expansion remains unformalised** (debt D1); the
numeric agreement is a sanity check on the write-up, not a proof.

### 5.4 Statement-fidelity checks inside Lean

Two claims the prose makes about `firoozbakht` are checked by the kernel rather than
asserted (`Statement.lean` §2.1):

* **The `^` is real exponentiation.** `example : (pₙ : ℝ) ^ ((1:ℝ)/(n:ℝ)) =
  Real.rpow (pₙ : ℝ) ((1:ℝ)/(n:ℝ)) := rfl`. This closes the trap where an exponent
  elaborates as a natural and `^` silently becomes `Monoid.npow` — which would turn
  `p^(1/n)` into `p^0 = 1` for every `n ≥ 2` and make the statement nonsense.
* **"Strictly decreasing" in the standard sense.** `firoozbakht_strictAnti :
  StrictAnti (fun k : ℕ => root (k + 1))` upgrades the one-step form to genuine
  `StrictAnti` over all pairs of indices, confirming that the one-step statement
  really is the whole content of "the sequence `pₙ^(1/n)` is strictly decreasing".
  Derived from `firoozbakht` via `strictAnti_nat_of_succ_lt`; it therefore carries
  `sorryAx`, as the audit shows.

### 5.5 Toolchain, pinned

- `leanprover/lean4:v4.29.0` (`lean-toolchain`)
- Mathlib `v4.29.0` (`lakefile.toml`, `[[require]] rev = "v4.29.0"`), resolved to the
  revision recorded in `lean/lake-manifest.json`
- Mathlib `.ltar` cache at `~/.cache/mathlib` (pre-existing on this host)

Every Mathlib lemma name used in the file was **resolved against a real Mathlib
checkout before being written**, per the `decompose` warning that recalled names are
L3 and must not be trusted. The names used, and where they live:

| Name | Source file in Mathlib |
|------|------------------------|
| `Nat.nth_prime_zero_eq_two` … `nth_prime_four_eq_eleven` | `Mathlib/Data/Nat/Prime/Nth.lean` |
| `Nat.prime_nth_prime` | `Mathlib/NumberTheory/PrimeCounting.lean` |
| `Nat.nth_strictMono` | `Mathlib/Data/Nat/Nth.lean` |
| `Nat.infinite_setOf_prime` | `Mathlib/Data/Nat/Prime/Infinite.lean` |
| `Real.rpow_lt_rpow_iff`, `Real.rpow_mul`, `Real.rpow_natCast`, `Real.rpow_nonneg`, `Real.rpow_add`, `Real.rpow_one` | `Mathlib/Analysis/SpecialFunctions/Pow/Real.lean` |
| `Real.log_lt_log_iff`, `Real.log_pow` | `Mathlib/Analysis/SpecialFunctions/Log/Basic.lean` |

---

## 6. Reproduce

```sh
cd lean
lake update          # resolves Mathlib v4.29.0
lake exe cache get   # fetches prebuilt Mathlib oleans
lake build           # exit 0; one `sorry` warning at Statement.lean:117
```

The `#print axioms` results of §5.1 are emitted by `lake build` itself — `Audit.lean`
is part of the library, so the separation is re-checked on every build and cannot
silently rot.

First build on this host took roughly 12 minutes (Mathlib partly rebuilt from source
despite the cache); subsequent builds replay in seconds.

---

## 7. Debts handed downstream

| # | Debt | Owner |
|---|------|-------|
| D1 | The asymptotic expansion of `Bₙ` (equation (3) of `attack`) is **not** formalised. It rests on PNT with lower-order terms. Do not cite the kernel for it. | `proof-attempt`, `skeptic` |
| D2 | **Finite verification is not formalised.** `Nat.nth` is `noncomputable`, so `decide` cannot evaluate `nthPrime`. Machine-checking the conjecture up to any `N₀` needs a bridge from `Nat.nth Nat.Prime` to an explicit sorted prime list plus a `Nat.count` certificate. This was flagged as OB-F8 / "the main risk item" by `decompose`, and it remains open — it is a substantial engineering task, not a skeleton concern. | `notebooks`, a future Lean leg |
| D3 | The sufficient condition `gₙ < (log pₙ)² − log pₙ − 1.17` (Kourbatov) and the necessary condition with `−1` are **not** formalised. Both are L2 in `decompose` and must be closed by `source-ledger` before any leg leans on them. | `source-ledger`, `citation-gate` |
| D4 | Attribution of the conjecture (Firoozbakht 1982, unpublished; first in print via Ribenboim) is **not** asserted in the Lean file — a formal file is the wrong place for a citation claim that has not passed `citation-gate`. | `source-ledger` |

---

## 8. Honest endpoint

What this leg delivers is a Lean file in which the conjecture is stated once,
faithfully, and five further equivalent readings of it are welded to that statement by
proofs the kernel has checked. What it does not deliver — and was never meant to — is
any progress on the conjecture itself.

The single `sorry` is the point.

### Files emitted

| Path | What it is |
|------|------------|
| `skeleton.md` | this document |
| `lean/Firoozbakht/Statement.lean` | the statement of record and the bridges |
| `lean/Firoozbakht/Audit.lean` | `#print axioms` separation, re-checked on every build |
| `lean/lakefile.toml`, `lean/lean-toolchain`, `lean/lake-manifest.json` | pinned toolchain and Mathlib revision |
| `build.log` | verbatim `lake build` transcript, exit status 0 |
| `sorry-audit.txt` | `grep -n sorry` over the statement file |

The same sources are committed to the galaxy repo under `lean/` (with `README.md` a
copy of this document), so they survive teardown of the run directory.
