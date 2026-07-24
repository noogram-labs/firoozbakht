# CC-12 — (SUF): $g_n \le T_n \Rightarrow$ F at $n$

| | |
|---|---|
| **Kind** | lemma |
| **Status** | **PROVED** (proof below). Valid for **every** $n \ge 1$ with no side condition — but **not a criterion**: it is one-directional and goes silent at $n = 2, 4$ `[C]`. |
| **Depends on** | CC-06, CC-11 |
| **Feeds** | OB-C2, OB-F6, W2; CC-14, CC-28 |
| **Ledger row** | NONE — derived here. Cf. `kourbatov2015upper` **Theorem 3** (V0) for the literature's own sufficient condition in $\ell_n$ coordinates (**CC-17**), and `visser2019verifying` **Sufficient condition 2, eq. (3.2)** (V0) for an index-based competitor (**CC-22**). |

## Statement

For every $n \ge 1$:

$$g_n \;\le\; T_n \;=\; \frac{p_n}{n}\log p_n
\qquad\Longrightarrow\qquad
S_n > 0, \text{ i.e. Firoozbakht holds at } n .$$

## Proof

By the right half of **CC-11** with $x = g_n/p_n > 0$:

$$n\log\!\Big(1 + \frac{g_n}{p_n}\Big) \;<\; \frac{n\,g_n}{p_n} \;\le\; \frac{n}{p_n}\cdot\frac{p_n\log p_n}{n} \;=\; \log p_n ,$$

using the hypothesis in the second step. Hence
$S_n = \log p_n - n\log(1+g_n/p_n) > 0$. ∎

The first inequality is strict, so the conclusion is strict even when the hypothesis is
an equality. No positivity side condition beyond $p_n > 0$, $n \ge 1$, $g_n > 0$.

## Where it goes silent `[C]`

`verify_cards.py` CHECK 5, over $n \le 3\,001\,133$:

> $g_n > T_n$ at **exactly** $n = 2$ and $n = 4$.

Those are $p = 3$ ($g = 2$, $T_2 = 1.648$) and $p = 7$ ($g = 4$, $T_4 = 3.405$). At both
indices Firoozbakht is **true** — $25 < 27$ and $14641 < 16807$ — and (SUF) simply
cannot see it. They are also, by `decompose.md#4`-FT-4, the **tightest known indices**
(normalised slack $\approx 0.070$, versus $0.210$ at the tightest large-$n$ point).

This is the panel's strongest finding restated as a property of this lemma:
**the sufficient condition is silent exactly where the conjecture is tightest.** It is
independently reproduced by `source-ledger.md#5`'s exact small-$n$ audit and by
Kourbatov's own $f_k < \ell_k$ threshold at $p_k \ge 11783$ (**CC-08**).

## Role in the proof-obligation tree

- **W2** in `decompose.md#3.5` — one of the four statements the polymer can honestly
  formalize sorry-free.
- **OB-F6** in Lean, medium difficulty (needs **CC-11** with resolved names).
- Parent of **CC-28**, the integer relaxation that makes finite verification cheap.
- Parent, with **CC-13**, of the dead band **CC-14**.

## What (SUF) is and is not

It is a **screen**: a cheap one-multiplication test that certifies the conjecture at an
index. It is **not** a criterion — failing it says nothing. `decompose.md#5.2`'s table
column "$T_n$ (needed $g$)" is this threshold, i.e. the *lower* edge of the undetermined
region, and reading it as "the gap that would refute" is a systematic error of one
band-width (**CC-14**).

Per `outcomes.md#A5`: E1 must be restated as a screen with an explicit tolerance, and
**E4 — the exact criterion (CC-10) — is the sole adjudicator**, mandatory on every
screen hit.

## Traps

- $T_n$ requires $p_n/n$, hence an explicit $p_n$ bound with a validity range
  (**CC-20**). (SUF) is only as unconditional as the arithmetic you feed it.
- The hypothesis is $\le$, the conclusion strict. Do not weaken to $<$ "for safety" — you
  lose the boundary case for nothing.
- (SUF) is **not** the contrapositive of (REF) (**CC-13**). They are two different
  one-sided implications with a gap between them. Treating one as the negation of the
  other is precisely what produces the dead band error.
