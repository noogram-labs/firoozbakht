# CC-09 — The five equivalent forms F1–F5

| | |
|---|---|
| **Kind** | lemma |
| **Status** | **PROVED** (proof below; `[C]` cross-checked at $n \le 200$ against exact integers) |
| **Depends on** | CC-01, CC-02, CC-03 |
| **Feeds** | OB-A1–A4, OB-F2–F4; CC-10, CC-29 |
| **Ledger row** | NONE — derived here. `visser2019verifying` **Conjecture 2 eq. (2.1)** (V0) states the form $(p_{n+1}/p_n)^n \le p_n$, which is F2 rearranged. |

## Statement

Fix $n \ge 1$. The following are equivalent:

| Tag | Statement | Lives in |
|---|---|---|
| **F1** | $p_{n+1}^{1/(n+1)} < p_n^{1/n}$ | $\mathbb{R}$, `rpow` |
| **F2** | $p_{n+1}^{\,n} < p_n^{\,n+1}$ | $\mathbb{N}$, `npow` |
| **F3** | $n\log p_{n+1} < (n+1)\log p_n$ | $\mathbb{R}$ |
| **F4** | $a_{n+1} < a_n$, where $a_n := (\log p_n)/n$ | $\mathbb{R}$ |
| **F5** | $S_n > 0$ (**CC-03**) | $\mathbb{R}$ |

*Firoozbakht's conjecture* = "F$k$ holds for every $n \ge 1$", for any one (hence all) of
$k = 1,\dots,5$.

## Proof

All four equivalences are **pointwise in $n$** — no quantifier exchange, no asymptotics,
no hidden hypothesis.

**F1 ⟺ F3.** $p_n \ge 2 > 0$ and $p_{n+1} > 0$, so both sides of F1 are positive and
$\log$ is a strictly increasing bijection $(0,\infty) \to \mathbb{R}$. Hence
F1 $\iff \frac{1}{n+1}\log p_{n+1} < \frac{1}{n}\log p_n$. Multiplying by $n(n+1) > 0$
preserves strictness and gives F3. ∎

**F3 ⟺ F2.** $\exp$ is strictly increasing, and $m^k = \exp(k\log m)$ for $m \ge 1$,
$k \in \mathbb{N}$. Apply with $(m,k) = (p_{n+1}, n)$ and $(p_n, n+1)$. Both sides of F2
are positive naturals, so the real and natural orderings coincide. ∎

**F3 ⟺ F4.** Divide F3 by $n(n+1) > 0$; the result is literally F4. ∎

**F3 ⟺ F5.** Compute the F3 defect:
$$(n+1)\log p_n - n\log p_{n+1} = \log p_n - n\big(\log p_{n+1} - \log p_n\big)
= \log p_n - n\log\!\frac{p_{n+1}}{p_n} = \log p_n - n\log\!\Big(1+\frac{g_n}{p_n}\Big) = S_n.$$
So F3 $\iff S_n > 0$. ∎

## Verification `[C]`

`verify_cards.py` CHECK 1: for every $n \le 200$, the **exact integer** predicate F2 and
the **floating-point** predicate F3 (evaluated as `n*log1p(g/p) < log(p)`) agree. Zero
mismatches. This is a check on the *implementation*, not on the proof — the proof needs
no verification — and it is the calibration that licenses using the float form above
$n = 200$ where the integer form has millions of digits.

## Role in the proof-obligation tree

Discharges **OB-A** entirely (obligations A1–A4). This is the only layer of
`decompose.md#2`'s tree that is fully closed.

**But it is not "revisit-free".** `decompose.md#2.1` says OB-A "needs no revisit" and is
"fully formalizable today with certainty", while `decompose.md#6.4` says *do not trust the
Mathlib lemma names* and debt #7 is Critical. A leg obeying the first sentence skips a
Critical debt (`outcomes.md#A11`). The mathematics is closed; the **formalization** is
not, and the gap between those is exactly OB-F2–F4.

## Which form to use where

| Task | Form | Why |
|---|---|---|
| Lean statement anchor | **F2** | pure ℕ, no `rpow` side conditions, `norm_num`-friendly (**CC-29**) |
| Numerical scan | **F5** via `log1p` | one `log` per index, error independent of $n$ (**CC-27**) |
| Exact adjudication, $n \le 200$ | **F2** | exact integers, no epistemics |
| Exact adjudication, $n$ large | **CC-10** | $f_n$ in arbitrary precision; F2 has $10^6$-digit sides |
| Reduction to gap language | **F5 → CC-10** | $S_n > 0 \iff g_n < f_n$ |

## Traps

- **F2's cost is not free.** Its per-$n$ bignum comparison is $\sim O(n^{1.6})$, so
  scanning to $N_0$ costs $O(N_0^{2.6})$: $N_0 = 10^6$ is on the order of weeks
  (`outcomes.md#A6`). `decompose.md#1.2`'s design note — that F2 makes finite
  verification "a pure `decide`/`norm_num` computation" — is contradicted twice over: by
  this cost, and by `decompose.md#6.1`'s own observation that `Nat.nth` is
  noncomputable. Use **CC-28** instead.
- F1 in Lean is `Real.rpow`, which is *not* `Monoid.npow` and does not reduce to it
  definitionally. The F2 → F1 bridge (OB-F3) must be stated as an **instantiated**
  biconditional mentioning `nthPrime` on both sides; a generic bridge lemma proved
  sorry-free and never instantiated is a silent failure (`outcomes.md#A6`).
- F4 invites an asymptotic reading ("$a_n \to 0$, so eventually…"). It is a **pointwise**
  statement. Monotonicity of a sequence tending to 0 is not implied by the limit.
