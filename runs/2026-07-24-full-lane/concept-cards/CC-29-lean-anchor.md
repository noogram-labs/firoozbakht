# CC-29 — The Lean anchor and the fidelity anchor set

| | |
|---|---|
| **Kind** | technique |
| **Status** | **OPEN** — no Lean project exists in this galaxy yet; every Mathlib name below is **`[L3]`, recalled, unresolved** |
| **Depends on** | CC-01, CC-09, CC-15, CC-28 |
| **Feeds** | OB-F1–F8, W1–W6; `lean-skeleton`, `lean-probe`, `evidence-gate` |
| **Ledger row** | NONE — engineering, not literature. |

## The anchor

Formalize **F2** (**CC-09**), the pure natural-number form:

```lean
import Mathlib

/-- `nthPrime n` is the `n`-th prime, **1-indexed**: `nthPrime 1 = 2`.
    Mathlib's `Nat.nth Nat.Prime` is 0-indexed, hence the shift.
    NOTE: ℕ-subtraction is truncated, so `nthPrime 0 = nthPrime 1 = 2`. -/
noncomputable def nthPrime (n : ℕ) : ℕ := Nat.nth Nat.Prime (n - 1)

/-- **Firoozbakht's conjecture** (integer form F2). -/
theorem firoozbakht :
    ∀ n : ℕ, 1 ≤ n → (nthPrime (n + 1)) ^ n < (nthPrime n) ^ (n + 1) := by
  sorry
```

F2 over F1: no `Real.rpow`, no positivity side conditions, no `NNReal` coercions, and
small cases fall to `norm_num`. F1 is recovered by one bridge lemma (OB-F3).

## The fidelity anchor set — non-negotiable

An index convention cannot be tested by checking *truth*: the index-shifted statement is
also true (it is implied by the real one). It must be tested by checking a **value**.
Before the conjecture is stated, discharge:

```lean
example : nthPrime 1 = 2 := by decide          -- OB-F1
example : nthPrime 2 = 3 := by decide
example : nthPrime 2 ^ 3 - nthPrime 3 ^ 2 = 2      := by decide   -- 27 - 25
example : nthPrime 4 ^ 5 - nthPrime 5 ^ 4 = 2166   := by decide   -- 16807 - 14641
```

The shifted variant yields **76** and **132490** for the last two. These are the FT-4
tightness witnesses (**CC-14**), and this is the one place where that finding becomes a
*test* rather than a warning (`outcomes.md#A6`).

## The risk ranking, inverted

`decompose.md#6.3` ranks the OB-F obligations by **effort**. Rank them by **failure
mode** instead (`outcomes.md#A6`):

| obligation | effort | failure mode | revised rating |
|---|---|---|---|
| **OB-F1** index convention | low | **silent** — shifted statement compiles and is true | **HIGH** |
| **OB-F2** statement | low | **silent** — a strictly weaker theorem passes every truth check | **HIGH** |
| **OB-F3** F2 ⟺ F1 bridge | medium | **silent** — a *generic* `rpow` bridge proves sorry-free and is never instantiated at `nthPrime`; a one-directional bridge into a degenerate ℕ-elaborated RHS closes by `le_refl` | **HIGH** |
| **OB-F4** F3/F5 forms | low | loud | low |
| **OB-F5** sandwich names | low | loud (build fails) | low — but see below |
| **OB-F6** (SUF)/(REF) | medium | loud | medium |
| **OB-F6b** integer criterion (**CC-28**) | low | loud | medium |
| **OB-F7** antitonicity (**CC-15**) | low | loud | low |
| **OB-F8** finite verification | high | **loud** (build does not close) | high effort, low risk |

The pattern: **the three obligations rated lowest by effort are the three that fail
silently.** OB-F8 is the loud one and was rated the main risk.

Concrete traps to guard:
- OB-F1's proposed `nthPrime n < nthPrime (n+1)` is **false at $n = 0$** (truncated
  subtraction gives $2 < 2$). Guard it with `1 ≤ n`.
- OB-F3's target must be the **instantiated** biconditional mentioning `nthPrime` on both
  sides, not a generic lemma about reals.

## OB-F8: why finite verification is the hard part

`Nat.nth` is `noncomputable`, so **`decide` cannot evaluate `nthPrime`**. This is a
decidability problem, not a performance problem. A finite verification needs a bridge:
`Nat.Prime` decidability + an explicit sorted prime list + a `Nat.count` certificate tying
the list's indices to `Nat.nth Nat.Prime`.

`decompose.md#1.2`'s design note — that F2 makes finite verification "a pure
`decide`/`norm_num` computation" — is contradicted by `decompose.md#6.1`'s own caveat.
And OB-B is costed **"routine"** in `#2.2` and **"high — the main risk item"** in `#6.3`
(`outcomes.md#A11`). Single cost: **high**.

Once the bridge exists, the *arithmetic* is cheap — use **CC-28**, not F2 exponentiation
(**CC-28 §Why this is the engine**). Revised target $N_0 = 10^4$–$10^5$ in **index**
(**CC-25**), with the ~13-order coverage gap against $2^{64}$ stated explicitly.

## The firewall: `#print axioms`, not "no `sorry`"

Two rules for `evidence-gate`:

1. **Check `#print axioms`, not merely the absence of `sorry`.** `native_decide` is
   unmentioned in `decompose.md`, is the only realistic route to a large $N_0$, and is
   sorry-free **while adding an axiom** (`outcomes.md#A6`(v)).
2. **"The main theorem compiles sorry-free" is a red flag, not a success.** Firoozbakht
   will not be closed here. A sorry-free `firoozbakht` means the statement changed —
   almost certainly an index shift (**CC-01**) — or a leg is hallucinating. Treat it as a
   mandatory statement re-audit (`decompose.md#6.5`).

## What is actually shippable

**W1** (all five equivalences, **CC-09**), **W2**/**W3** ((SUF)/(REF), **CC-12**/**CC-13**),
**W6** (antitonicity, **CC-15**), plus **OB-F6b** (**CC-28**) and **W5** to a defensible
$N_0$. That is a real, honest, sorry-free artifact. It is **not** a proof of Firoozbakht,
and `write-paper` and `editorial-verdict` must be told so in those words.

## Traps

- **Every Mathlib name in this deck is `[L3]`.** `lean-probe`'s first action is a probe
  file that resolves every name against the pinned toolchain and reports corrections.
  Named-lemma drift is the most common silent failure in LLM-authored Lean.
- Toolchain: `elan` present; `leanprover/lean4:v4.29.0` and `v4.29.1` installed; a Mathlib
  `.ltar` cache at `~/.cache/mathlib`. **No Lean project exists in this galaxy** —
  `lean-skeleton` must `lake new` and pin a Mathlib revision compatible with v4.29.x.
- `decompose.md#2.1` says OB-A "needs no revisit" and is "fully formalizable today with
  certainty" while debt #7 (resolve Mathlib names) is **Critical**. A leg obeying the
  first sentence skips the Critical debt. Strike the sentence (`outcomes.md#A11`).
