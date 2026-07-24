# CC-07 — The exact barrier $f_n = p_n^{1+1/n} - p_n$

| | |
|---|---|
| **Kind** | definition |
| **Status** | **SOURCED (V0)** — the object and its value at the record locus are published |
| **Depends on** | CC-01, CC-02 |
| **Feeds** | OB-C (all of it), OB-E1, OB-E4; CC-08, CC-10, CC-14, CC-26 |
| **Ledger row** | `kourbatov2015upper` (**V0 + C**), **Table 1** ¶ — defines $f_k = p_k^{1+1/k} - p_k$ and tabulates it; last row gives $f_k = 1193.418$. Also `visser2019verifying` (**V0**), **Conjecture 3, eq. (2.4)** ¶ — $g_n \le p_n\big(p_n^{1/n}-1\big)$, $n \ge 1$. |

## Statement

$$\boxed{\;f_n \;:=\; p_n^{\,1+1/n} - p_n \;=\; p_n\Big(p_n^{1/n} - 1\Big) \;=\; p_n\big(e^{L_n/n} - 1\big),\qquad L_n = \log p_n.\;}$$

This is **the** barrier. Firoozbakht at $n$ is *exactly* $g_n < f_n$ — see **CC-10** for
the proof. No approximation, no side condition, no exceptional small-$n$ set, no
external explicit bound, no asymptotic regime.

## Why this card is the most important definition in the deck

The upstream decomposition built its entire attack surface on the *linearised* threshold
$T_n$ (**CC-06**) reached through a logarithm sandwich (**CC-11**), and then spent
considerable effort characterising the resulting two-sided imprecision — the dead band,
its width, its non-empty exceptions at $n = 2, 4$, the $n \ge 484$ threshold
(`outcomes.md#A4`, the panel's strongest finding).

**All of that imprecision is self-inflicted.** The sandwich is a tool for bounding
$\log(1+x)$ when you cannot evaluate it. Here you can: exponentiating the log form of the
conjecture gives a closed-form barrier directly. The exact criterion is one `expm1` call,
is monotone, is valid at every $n \ge 1$, and has no band.

The dead band is therefore not a fact about Firoozbakht's conjecture. It is a fact about
a lemma that was applied where it was not needed. **CC-14** records it anyway, because
the (SUF)/(REF) screens remain useful as *cheap filters* and their blind spot must be
documented — but no downstream leg should adjudicate with them.

## Values `[C]`

`verify_cards.py` CHECK 7, 60-digit `mpmath`, at the record locus:

$$f_k \;=\; 1193.41777829404\ldots \qquad\text{(Kourbatov Table 1: } 1193.418\text{)} \;\checkmark$$

Relation to the two neighbouring quantities:

| | value at record locus | relation to $f_k$ |
|---|---|---|
| $g_k$ (observed gap) | $1132$ | $f_k/g_k = \mathbf{1.054256}$ — the shortfall (**CC-26**) |
| $T_k$ (linearisation) | $1193.41777829362$ | agrees to $3.5\times10^{-13}$ relative |
| $\ell_k = L^2 - L$ (proxy) | $1194.51592191172$ | overshoots by $\mathbf{1.098}$ **gap units** |

## Asymptotics

$$f_n \;=\; \log^2 p_n - \log p_n - 1 + o(1) \qquad (n \to \infty)$$

— `kourbatov2015upper` **Theorem 5** (V0), see **CC-18**. The finer expansion
$f_n = L^2 - L - 1 - 3/L - 13/L^2 + O(L^{-3})$ asserted in `attack.md#2` is **unsourced**
(ledger gap **G1**) and must be derived in-project with proof or deleted.

## Role in the proof-obligation tree

**CC-07 replaces the OB-C reduction chain C1–C4 with a single identity.** Concretely:

- C1 (sandwich) → not needed for the criterion; retained only for the screens.
- C2 (threshold $T_n$) → a corollary, not a definition.
- C3 (sharpness) → vacuous: there is nothing to be sharp about.
- C4 (translation to $\log^2 p - \log p$) → **still needed**, and still the deck's only
  external dependency (**CC-08**, **CC-19**, **CC-20**) — because the *literature* speaks
  in $\ell_n$, and any statement of the form "Firoozbakht requires a Cramér-type bound
  with constant 1" is a statement about $\ell_n$, not about $f_n$.

## Traps

- **Notation collision, active in this polymer.** `attack.md#2` writes $B_n$ for this
  object; `outcomes.md#A4` writes $B_n$ for the *dead band*. Two different things, one
  letter, same run directory. This deck uses **$f_n$** (Kourbatov's own, V0) for the
  barrier and $\mathcal{B}_n$ for the band. Any leg reading both documents must
  disambiguate before quoting.
- $f_n$ is **not** computable in `Nat`. Formalising the exact criterion in Lean means
  `Real.rpow`, which is precisely the friction `decompose.md#6.2` avoided by choosing the
  F2 integer form. **This is a genuine trade-off, not an oversight**: F2 is the right
  *Lean* anchor (**CC-29**) and $f_n$ is the right *numerical* anchor (**CC-27**). They
  are provably the same statement (**CC-10**); use each where it is cheap.
- Evaluating $f_n$ in double precision requires care: $p_n^{1/n} - 1$ is a
  cancellation. Use `p * expm1(log(p)/n)`, never `p * (p**(1/n) - 1)`. Above
  $p \sim 10^{16}$ use exact or arbitrary-precision arithmetic (**CC-27**).
