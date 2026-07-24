# CC-06 — The linearised threshold $T_n$

| | |
|---|---|
| **Kind** | definition |
| **Status** | **PROVED** (definition + its relation to the exact barrier) |
| **Depends on** | CC-01, CC-02, CC-20 (for the $p_n/n$ evaluation) |
| **Feeds** | OB-C2, OB-C3; CC-12, CC-13, CC-14, CC-28 |
| **Ledger row** | NONE for the definition. Its numerical evaluation via $p_n/n$ rests on `dusart2010estimates` Props. 6.6/6.7 (V0 + C) — see **CC-20**. |

## Statement

$$T_n \;:=\; \frac{p_n}{n}\,\log p_n .$$

$T_n$ is the **first-order linearisation** of the exact barrier $f_n$ (**CC-07**):
expanding $f_n = p_n(e^{L_n/n} - 1)$ with $L_n/n$ small,

$$f_n \;=\; p_n\Big(\tfrac{L_n}{n} + \tfrac{L_n^2}{2n^2} + \cdots\Big)
\;=\; T_n\Big(1 + \tfrac{L_n}{2n} + O(L_n^2/n^2)\Big),
\qquad L_n/n = O\!\big(L_n^2/p_n\big).$$

So $T_n$ and $f_n$ agree to relative error $O\!\big((\log p_n)^2/p_n\big)$.

## Measured agreement `[C]`

At the record locus $k = 49\,749\,629\,143\,526$, $p_k = 1\,693\,182\,318\,746\,371$
(60-digit `mpmath`, `verify_cards.py` CHECK 7):

| | |
|---|---|
| $T_k$ | $1193.41777829362\ldots$ |
| $f_k$ | $1193.41777829404\ldots$ |
| relative difference | $\mathbf{3.5\times10^{-13}}$ |
| $p_k/k$ | $34.0340691558\ldots$ |
| $c_k := \log p_k - p_k/k$ | $\mathbf{1.0313170262\ldots}$ |

Twelve significant figures of agreement — which is the true content of
`decompose.md#2.3-C3`'s "sharp to sixteen decimal places" claim, restated at the
precision it actually has.

## Why the card exists, given that CC-07 supersedes it

Three reasons, all operational:

1. **It is the form the (SUF)/(REF) screens are stated in** (**CC-12**, **CC-13**), and
   those screens are cheap: one multiplication versus an `expm1`.
2. **It is the form the integer criterion is derived from** (**CC-28**), which is what
   makes a Lean finite-verification pass tractable at all.
3. **It is the bridge to the literature's $\log^2 p - \log p$ vocabulary** (**CC-08**),
   because $p_n/n = \log p_n - c_n$ turns $T_n$ into $\ell_n - (c_n-1)\log p_n$.

None of those is a reason to use $T_n$ as the *criterion*. Use **CC-10** for that.

## The $c_n$ constant — what is and is not pinned

$c_n := \log p_n - p_n/n$. `decompose.md#2.3-C4` asserts $c_n \in (0.9, 1.2)$, tagged
`[L2]`. That interval is **not derivable from the bound C4 cites**: the two-term
bracket $n(\log n + \log\log n - 1) < p_n < n(\log n + \log\log n)$ has width exactly
$n$ and permits only $c_n \in (0.076, 1.076)$ at the record locus `[C, source-ledger §5]`.
The stated interval was an eyeball fit to a table spanning $n \le 3\times10^6$, applied
nine orders higher (`outcomes.md#A3`).

The **three-term** Dusart bracket (**CC-20**, V0) does pin it:
$$c_k \in (1.030154,\ 1.033325) \quad\text{for } k \ge 688\,383,$$
true value $1.031317$ at the record locus. Certified shortfall $1.05424 \pm 0.00005$ —
consistent with, and superseded by, the exact value $1.054256$ from **CC-07**/**CC-26**.

Note also: **$c_n - 1$ changes sign at $n = 61$** (`outcomes.md#A4`), so any use of
"$p_n/n \approx \log p_n - 1$" as a *one-sided* bound is wrong on one side of that index.

## Traps

- $T_n$'s numerical evaluation needs $p_n/n$, i.e. an explicit $p_n$ bound. That is an
  external dependency (**CC-20**) with a validity range ($k \ge 688\,383$). $f_n$ needs
  nothing external. **This is the single strongest argument for CC-07 over CC-06.**
- The chain $T_n \to \ell_n$ (**CC-08**) carries a *second* approximation whose error is
  $\approx 9\times10^{-4}$ relative — **twelve orders larger** than the $5\times10^{-16}$
  the sandwich contributes, and worth $+1.10$ **gap units** at the record locus. A
  sentence advertising $5\times10^{-16}$ while displaying the $\ell_n$ form is off by
  $10^{12}$ (`outcomes.md#A4`).
- $T_n$ appears in `decompose.md#5.2`'s table as "needed $g$". It is the **(SUF)**
  threshold, i.e. the *lower* edge of the dead band — not the refutation threshold.
