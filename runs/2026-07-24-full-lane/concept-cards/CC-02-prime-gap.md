# CC-02 — The prime gap $g_n$

| | |
|---|---|
| **Kind** | definition |
| **Status** | **PROVED** (trivial, but the integrality is load-bearing) |
| **Depends on** | CC-01 |
| **Feeds** | OB-C, OB-E; CC-03, CC-04, CC-05, CC-10, CC-14 |
| **Ledger row** | NONE — derived here |

## Statement

$$g_n := p_{n+1} - p_n .$$

Elementary facts, each used somewhere in the deck:

1. $g_n \in \mathbb{Z}_{>0}$, and $g_n$ is **even for every $n \ge 2$** (both primes odd).
   $g_1 = 1$ is the unique odd gap.
2. $0 < g_n < p_n$ for $n \ge 2$ (Bertrand's postulate: $p_{n+1} < 2p_n$).
3. $\dfrac{p_{n+1}}{p_n} = 1 + \dfrac{g_n}{p_n}$, with $0 < g_n/p_n < 1$ for $n \ge 2$ —
   the substitution that makes **CC-11** applicable.
4. Consecutivity is a *certificate obligation*: asserting "$g$ is the gap after $p$"
   requires $p$ and $p+g$ prime **and** $p+1, \dots, p+g-1$ all composite. Linear in $g$,
   routine, and routinely omitted (OB-E2).

## Why integrality is load-bearing, not pedantry

Every criterion in this deck compares the **integer** $g_n$ against a **real** barrier.
Two consequences that a purely real-analytic treatment misses:

- **The dead band can be empty.** A gap of width $W$ between two real thresholds
  contains no integer whenever the thresholds straddle no integer. This is what makes
  **CC-14** a statement with a *finite exceptional set* rather than a permanent defect.
- **Strict vs. non-strict is free.** $g_n < f_n$ and $g_n \le f_n$ define the same
  predicate whenever $f_n \notin \mathbb{Z}$, which is why the literature's $\le$ forms
  (Visser eq. (2.4)) and this deck's $<$ forms agree. See **CC-10 §Traps**.
- **Bertrand is already free.** Any bound of the shape $g_n < p_n^{\theta}$ with
  $\theta \ge 1$ is vacuous. The Firoozbakht barrier is $\asymp (\log p_n)^2$, i.e.
  *polylogarithmic* — nine orders of magnitude smaller than $p_n^{0.525}$ at
  $p \sim 10^{18}$. That size difference is the whole content of **CC-23**.

## Role in the proof-obligation tree

The variable that OB-C reduces everything to. After **CC-10**, Firoozbakht is *entirely*
a statement about $(g_n)$: no roots, no exponentials, no index-$n$ exponent.

## Traps

- $g_n$ vs. $g(p)$: the gap *after* $p$. The literature's maximal-gap tables index by the
  prime **preceding** the gap. `decompose.md#5.2` and `source-ledger.md#3.3` both follow
  that convention; a table read the other way shifts every merit by one row.
- "Maximal gap" ≠ "first occurrence" ≠ "record CSG ratio". Three different maxima over
  three different orderings. The record CSG locus ($p = 1\,693\,182\,318\,746\,371$,
  $g = 1132$) is **not** the largest known gap (**CC-26**).
