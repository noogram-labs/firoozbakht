# CC-08 — The proxy barrier $\ell_n = \log^2 p_n - \log p_n$

| | |
|---|---|
| **Kind** | definition |
| **Status** | **SOURCED (V0)** — tabulated in the literature; its *relation* to $f_n$ is `[C]` here |
| **Depends on** | CC-07, CC-18, CC-19, CC-20 |
| **Feeds** | OB-C4, OB-D (the whole barrier argument), FT-2; CC-16, CC-17, CC-23 |
| **Ledger row** | `kourbatov2015upper` (**V0 + C**), **Table 1** ¶ — $\ell_k = \log^2 p_k - \log p_k$, last row $\ell_k = 1194.516$. Asymptotic link: same source, **Theorem 5** ¶. Explicit-bound chain: `axler2014newbounds` **Cor. 3.5/3.6** (V0), `dusart2010estimates` **Props. 6.6/6.7** (V0). |

## Statement

$$\ell_n \;:=\; \log^2 p_n - \log p_n \;=\; L_n^2 - L_n .$$

This is the barrier **as the prime-gap literature states it**. It is a *proxy* for the
exact barrier $f_n$ (**CC-07**), related by

$$f_n \;=\; \ell_n - 1 + o(1) \qquad (n\to\infty)$$

— `kourbatov2015upper` Theorem 5, V0 (**CC-18**).

The translation runs $f_n \approx T_n = (p_n/n)\log p_n$ and then
$p_n/n = \log p_n - c_n$ with $c_n \to 1$, giving $T_n \approx L_n^2 - c_n L_n$. It is
the second step that needs the explicit $\pi(x)$/$p_n$ machinery, and it is the deck's
**only** external mathematical dependency.

## The cost of the proxy `[C]`

`verify_cards.py` CHECK 7, at the record locus:

| | value |
|---|---|
| $f_k$ (exact) | $1193.41777829404$ |
| $\ell_k$ (proxy) | $1194.51592191172$ |
| $\ell_k - f_k$ | $\mathbf{1.09814}$ **gap units** |
| $\ell_k - 1 - f_k$ | $0.09814$ |

So the proxy overshoots by 1.10 gap units, and the corrected proxy $\ell_k - 1$ still
overshoots by 0.098. In relative terms $9.2\times10^{-4}$ — **twelve orders of magnitude
larger** than the $5\times10^{-16}$ that `decompose.md#0.2` advertises for its criterion.
That sentence conflates the sandwich's error (tiny) with the $\ell_n$ translation's error
(not tiny). Both are stated on the same line.

For screening purposes 1.1 gap units is negligible against a shortfall of 61 gap units.
For *adjudication* it is not: a candidate counterexample within 1.1 of the proxy
threshold is undetermined, and only **CC-10** settles it.

## The crossover `[C]`

$f_k < \ell_k$ is **not** universal. `verify_cards.py` CHECK 10:

> the last $k$ with $f_k \ge \ell_k$ is $k = 1411$ ($p = 11779$); it holds for every
> $k \ge 1412$, and $p_{1412} = \mathbf{11783}$.

This reproduces `kourbatov2015upper` §3's stated threshold ($f_k < \ell_k$ for
$p_k \ge 11783$, $k \ge 1412$) **exactly** — an independent confirmation of a V0 locator,
and one of the two places in this deck where recomputation and a refereed source meet on
a number neither derived from the other.

## Role in the proof-obligation tree

$\ell_n$ is what makes **CC-23** sayable. "Firoozbakht requires a prime-gap upper bound
of Cramér type with constant 1" is a statement about $\ell_n$; it cannot be phrased in
$f_n$, which is not a function of $\log p_n$ alone. Every barrier argument, every
comparison against Baker–Harman–Pintz or against RH, and every heuristic invoking
Cramér's constant, passes through this card.

## Traps

- **The deleted back-edge.** `decompose.md#2.3-C4` calls Kourbatov's agreement with its
  own derivation *"a genuine independent confirmation of C2–C4, not a citation"*, while
  `decompose.md#4`-FT-6 makes C2–C4 falsifiable **by** that same citation. Those two
  edges form a cycle through an unverified node (`outcomes.md#A7`). This deck deletes the
  confirmation edge: the derivation's own resolution on $c_n$ is too coarse — $\pm 0.3\log p
  \approx \pm 10$ at $p \sim 10^{15}$, i.e. 10–36× coarser than the additive constant
  $-1$ it claims to match — to confirm or refute anything at the level of that constant.
  What *is* confirmed, at 12 significant figures and by a computation independent of the
  paper, is $T_k \approx f_k$ (**CC-06**) and the $k = 1412$ crossover above.
- $\ell_n$ vs. $\ell_n - 1$ vs. $\ell_n - 1.17$ are three different thresholds with three
  different roles: the raw proxy, Kourbatov's **necessary** condition (**CC-16**), and
  Kourbatov's **sufficient** condition (**CC-17**). Substituting one for another swaps a
  proof for a disproof.
- FT-6 (the literature cross-check) is recorded upstream as both *passed* and
  *outstanding*. Per `outcomes.md#A7` it is **NOT YET RUN** as a falsification test, and
  a material discrepancy would be a fact about the literature or about normalisation —
  it would **not** falsify C2–C4, which stand on their own proofs.
