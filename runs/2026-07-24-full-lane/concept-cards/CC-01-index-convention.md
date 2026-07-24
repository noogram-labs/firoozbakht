# CC-01 — The index convention

| | |
|---|---|
| **Kind** | definition |
| **Status** | **PROVED** — the mathematical content is a convention; the *hazard* is a theorem about Mathlib and is verified by compilation, not by this card |
| **Depends on** | — (root) |
| **Feeds** | OB-F1, OB-B2; every card in the deck; `lean-skeleton`, `lean-probe`, `notebooks` |
| **Ledger row** | NONE — derived here |

## Statement

$p_n$ denotes the $n$-th prime under the **1-indexed** convention:

$$p_1 = 2,\quad p_2 = 3,\quad p_3 = 5,\quad p_4 = 7,\quad p_5 = 11,\ \dots$$

Equivalently $p_n = \min\{q \text{ prime} : \pi(q) = n\}$, and $\pi(p_n) = n$ for all
$n \ge 1$. Firoozbakht's conjecture is the assertion that
$p_{n+1}^{1/(n+1)} < p_n^{1/n}$ for all $n \ge 1$ **under this convention**.

$\log$ is the natural logarithm throughout. $L_n := \log p_n$.

## Why this is a card and not a footnote

The exponent $n$ is not decoration — it is the object the conjecture is about. Shifting
the index by one produces a **different, strictly weaker statement** that is true if
Firoozbakht is true, that passes every small-$n$ spot check anyone writes by reflex, and
that compiles.

Three independent traps, all real:

**(a) Mathlib is 0-indexed.** `Nat.nth Nat.Prime 0 = 2`. The natural transcription

```lean
∀ n : ℕ, 1 ≤ n → Nat.nth Nat.Prime (n + 1) ^ n < Nat.nth Nat.Prime n ^ (n + 1)
```

is **not** Firoozbakht: at `n = 1` it asserts $p_3^1 < p_2^2$, i.e. $5 < 9$, where the
conjecture asserts $p_2^1 < p_1^2$, i.e. $3 < 4$. Both true; different statements. The
shifted form is implied by the true one (`outcomes.md#A6`), so no test that only looks
for *falsity* can distinguish them.

**(b) Truncated subtraction aliases $n=0$ onto $n=1$.** The obvious repair

```lean
noncomputable def nthPrime (n : ℕ) : ℕ := Nat.nth Nat.Prime (n - 1)
```

gives `nthPrime 0 = nthPrime 1 = 2` because ℕ-subtraction saturates. Harmless under
`1 ≤ n`; fatal in any lemma stated without that guard. In particular
`nthPrime n < nthPrime (n+1)` — which `decompose.md#6.3`-OB-F1 proposes as an
`example` — is **false at $n = 0$** ($2 < 2$).

**(c) `Nat.nth` is `noncomputable`.** `decide` cannot evaluate it. This is not a
performance problem, it is a *decidability* problem, and it is the single reason
finite verification in Lean (OB-F8) is hard rather than routine. See **CC-29**.

## The fidelity anchor set

An index convention cannot be tested by checking a statement's *truth* — both the true
and the shifted statement are true. It must be tested by checking a *value*. The
discriminating witnesses are the two tightest known indices (**CC-14**):

| anchor | correct value | shifted-variant value |
|---|---|---|
| $p_2^{\,3} - p_3^{\,2}$ | $27 - 25 = \mathbf{2}$ | $125 - 49 = 76$ |
| $p_4^{\,5} - p_5^{\,4}$ | $16807 - 14641 = \mathbf{2166}$ | $161051 - 28561 = 132490$ |

Any formalization must discharge **both equalities as `example`s** before it states the
conjecture. This is the one place where the small-$n$ tightness finding becomes a *test*
rather than a warning (`outcomes.md#A6`).

## Role in the proof-obligation tree

Root of **OB-F1**. Not a step in any proof of the conjecture — a precondition on every
step. A polymer that gets this wrong produces artifacts that are internally consistent,
machine-checked, and about the wrong theorem.

## Traps

- $\pi(x)$ is 0 for $x < 2$; $\pi(2) = 1$. Off-by-one here propagates into every
  explicit-bound evaluation (**CC-19**).
- The literature is not uniform. Kourbatov indexes maximal gaps by $k$ with
  $p_k$ the prime *preceding* the gap — consistent with this card. Visser writes $n$
  for the same thing. No conflict, but check before transcribing a table row.
