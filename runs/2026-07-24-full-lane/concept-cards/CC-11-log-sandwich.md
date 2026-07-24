# CC-11 — The logarithm sandwich

| | |
|---|---|
| **Kind** | lemma |
| **Status** | **PROVED** (proof below). Its **Mathlib names are `[L3]` — unresolved** (debt #7). |
| **Depends on** | — |
| **Feeds** | OB-C1, OB-F5; CC-12, CC-13, CC-14 |
| **Ledger row** | NONE — standard calculus, proved here. Present in Mathlib in some form; the exact lemma names have **not** been resolved by compilation and must not be quoted from memory. |

## Statement

For every real $x > 0$:

$$\frac{x}{1+x} \;<\; \log(1+x) \;<\; x .$$

Both inequalities are strict for $x > 0$, with equality only at $x = 0$.

## Proof

Let $h(x) := x - \log(1+x)$ on $[0,\infty)$. Then $h(0) = 0$ and
$h'(x) = 1 - \frac{1}{1+x} = \frac{x}{1+x} > 0$ for $x > 0$, so $h$ is strictly
increasing and $h(x) > 0$, i.e. $\log(1+x) < x$. ∎

Let $k(x) := \log(1+x) - \frac{x}{1+x}$ on $[0,\infty)$. Then $k(0) = 0$ and
$$k'(x) = \frac{1}{1+x} - \frac{(1+x) - x}{(1+x)^2} = \frac{1}{1+x} - \frac{1}{(1+x)^2}
= \frac{x}{(1+x)^2} > 0 \quad (x>0),$$
so $k$ is strictly increasing and $k(x) > 0$. ∎

## Application to the conjecture

Substitute $x = g_n/p_n$, so $1 + x = p_{n+1}/p_n$ and
$\log(1+x) = \log p_{n+1} - \log p_n$. Multiplying through by $n > 0$:

$$\frac{n\,g_n}{p_{n+1}} \;<\; n\log\!\Big(1+\frac{g_n}{p_n}\Big) \;<\; \frac{n\,g_n}{p_n}.$$

The middle term is $\log p_n - S_n$ (**CC-03**). Comparing each outer term against
$\log p_n$ gives the two screens: the **right** inequality gives (SUF) (**CC-12**), the
**left** gives (REF) (**CC-13**).

## Role in the proof-obligation tree

**OB-C1.** Note what the sandwich is *for*: it converts a transcendental comparison into
two rational ones. That is valuable when the transcendental cannot be evaluated. Here it
can (**CC-10**), so the sandwich buys **cheapness, not necessity** — and it costs a dead
band (**CC-14**). Use it for screening, never for adjudication.

**OB-F5** in Lean: pin the Mathlib names by compilation. `decompose.md#2.3-C1` guesses
the `Real.add_one_le_exp` / `Real.log_lt_sub_one_of_ne` family and explicitly tags the
guess `[L3]`. Treat any name in this deck the same way: **unresolved until a `lean-probe`
build emits it**. Named-lemma drift is the most common silent failure in an LLM-authored
Lean skeleton (`decompose.md#6.4`).

## Traps

- The two bounds are **not equally tight**. Near $x = 0$, $x - \log(1+x) \approx x^2/2$
  while $\log(1+x) - \frac{x}{1+x} \approx x^2/2$ — symmetric to leading order, so the
  band's width is $\approx x \cdot$ (the middle value), i.e. exactly the factor
  $1 + g_n/p_n$ that separates (SUF) from (REF).
- The Mathlib form is often stated as $\log x \le x - 1$ (substituting $x \mapsto 1+x$)
  or as $1 + x \le e^x$. Converting between them is routine but is *not* free in Lean —
  each conversion is a rewrite with its own side conditions on positivity.
- Strictness matters here. The non-strict versions are the ones that are usually in a
  library; the strict versions ($x \ne 0$) are separate lemmas.
