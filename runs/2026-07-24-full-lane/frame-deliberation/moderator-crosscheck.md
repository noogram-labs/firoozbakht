# Moderator's independent cross-check (crew role: skeptic)

Run by the `frame-deliberation` moderator *before* reading any panel response, so
that the synthesis has an anchor independent of the panel. Reproduces or corrects
the load-bearing arithmetic of `decompose/decompose.md`.

## 1. The headline shortfall (§0.2 item 2, §3.2) — CONFIRMED

At the record locus quoted by the document, $p = 1\,693\,182\,318\,746\,371$, $g = 1132$:

| Quantity | Recomputed | Document |
|---|---|---|
| $\log p$ | 35.065386 | 35.07 |
| $R = g/(\log p)^2$ | 0.920639 | 0.9206 |
| required $1 - 1/\log p$ | 0.971482 | 0.9715 |
| **shortfall** | **1.055226** | **1.055** |

The arithmetic is right. (Whether the *locus* is right is `source-ledger` debt #1
— unchanged by this check.)

## 2. The dead band (frame Q7) — NON-EMPTY, AND EXACTLY AT THE TIGHTEST CASES

(SUF) fires when $g_n \le T_n$; (REF) fires when $g_n \ge T_n(1+g_n/p_n)$. The band
between them, where **neither** criterion decides, has width $T_n\,g_n/p_n$ in the
integer variable $g_n$. It is empty whenever that width is $< 1$.

**At the record locus**: band width $= 8.0\times10^{-10}$ (relative width
$6.69\times10^{-13} = g/p$). Contains no integer. Empty — the document's dismissal
is correct *there*.

**Exhaustive search over $p_n < 2\times10^6$**: the band is non-empty at exactly
**two** indices —

| $n$ | $p_n$ | $g_n$ | $T_n$ | $T_n(1+g_n/p_n)$ | verdict |
|---|---|---|---|---|---|
| **2** | 3 | 2 | 1.6479 | 2.7465 | $g$ inside the band |
| **4** | 7 | 4 | 3.4053 | 5.3513 | $g$ inside the band |

These are the *only* indices below $2\times10^6$ where $g_n > T_n$, i.e. where
**(SUF) does not fire at all**.

**They are the same two indices the document's own FT-4 (§4) identifies as the
tightest cases in the entire computed range** (normalized slack $\approx 0.070$).

So: the criterion §0.2 calls "*the* criterion, sharp to sixteen decimal places,
unconditionally" is **undefined at precisely the two hardest known points of the
conjecture**. F is true at $n = 2$ and $n = 4$ — but not because (SUF) says so.
It is true there only by the exact predicate FT-1 ($5^2 = 25 < 27 = 3^3$;
$11^4 = 14641 < 16807 = 7^5$).

This does not break the conjecture and does not break the *asymptotic* reduction.
It breaks the **quantifier** in §0.2's headline ("Firoozbakht at $n$ $\iff$
$g_n \lesssim T_n$", stated for all $n$) and in §2.3-C3's word
"unconditionally". The honest statement carries a hypothesis: the equivalence
holds for $n \ge 5$ (verified to $p < 2\times10^6$; the width argument
$T_n g_n/p_n < 1$ extends it far beyond).

**Consequence for the DAG.** Any Lean formalization of W2/(SUF) (§3.5, OB-F6)
that states it for all $n \ge 1$ is stating something **weaker than intended or
vacuous at the interesting points**, and any pipeline that routes small $n$ to
the proxy rather than the exact predicate is wrong there. The document flags the
small-$n$ trap for *tightness* (FT-4) but does not connect it to the *criterion's
domain of definition*. That connection is this leg's finding.

## 3. $p_n/n$ against $\log p_n - 1$ (§2.3-C4) — CONFIRMED in direction

Recomputed $T_n$ under $p_n/n = \log p_n - c$ for $c \in \{0.9, 1.0, 1.2\}$ at the
record locus gives shortfall $1.049$–$1.058$. The headline $1.055$ sits inside
that spread, so the claim is **not sensitive** to the unclosed Dusart/
Rosser–Schoenfeld constants (debt #4) at three significant figures. That is a
point *in the document's favour* and should be recorded as such: the L2 debt on
$c_n$ does not propagate into the headline number.

---

*Independent of the panel. Used in step 3 to detect substitution and to arbitrate
where panelists disagree.*
