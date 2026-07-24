# CC-18 — Kourbatov Theorem 5: $f_k = \ell_k - 1 + o(1)$ — and gap G1

| | |
|---|---|
| **Kind** | lemma (asymptotic) + **OPEN** obligation |
| **Status** | **SOURCED (V0)** for the $o(1)$ form. The finer expansion asserted in `attack.md` is **OPEN / UNSOURCED** (ledger gap **G1**). |
| **Depends on** | CC-07, CC-08, CC-19 |
| **Feeds** | OB-C4, OB-D4; CC-08, CC-23; `citation-gate` |
| **Ledger row** | `kourbatov2015upper` (**V0**), **Theorem 5 (§5, Appendix)** ¶ — *"$f_k = \log^2 p_k - \log p_k - 1 + o(1)$ as $k \to \infty$"*, where $f_k = p_k^{1+1/k} - p_k$. The proof brackets $f_k$ between $\log^2 p_k - \log p_k - 1 - 3.83/\log p_k$ and $\log^2 p_k - \log p_k - 1$. |

## Statement (sourced)

$$f_k \;=\; \log^2 p_k - \log p_k - 1 + o(1) \qquad (k \to \infty),$$

with the published two-sided bracket

$$\ell_k - 1 - \frac{3.83}{\log p_k} \;\le\; f_k \;\le\; \ell_k - 1 .$$

This is **the whole published asymptotic** for the exact barrier. It is what connects
**CC-07** (the object the conjecture is actually about) to **CC-08** (the object the
literature is written in), and therefore what makes **CC-23** quotable.

## Sanity check `[C]`

At the record locus (`verify_cards.py` CHECK 7): $\ell_k - f_k = 1.09814$, and
$\log p_k = 35.0654$, so $3.83/\log p_k = 0.1092$. The bracket predicts
$\ell_k - f_k \in [1,\ 1.1092]$; observed $1.09814$. ✓ — inside, and near the lower edge,
as the $o(1)$ term's sign requires.

## Gap G1 — the unsourced refinement

`attack.md#2` states, and attributes the viewpoint to Kourbatov:

$$B_n = L^2 - L - 1 - \frac{3}{L} - \frac{13}{L^2} + O(L^{-3}),
\qquad
\frac{p_n}{n} = L - 1 - \frac{1}{L} - \frac{3}{L^2} - \frac{13}{L^3} + O(L^{-4}).$$

**The $-3/L$ and $-13/L^2$ terms appear nowhere in Theorem 5**, which proves only the
$+o(1)$ form with a $3.83/L$ bracket. The coefficients $1, 3, 13$ are the Cipolla /
OEIS A233824 family for the inversion of the PNT series and are *plausible* — but no
row in the ledger states them, and `source-ledger.md#2` records a search that came back
empty.

**Status: OPEN.** Two admissible resolutions, no third:

1. **Derive them in-project**, with proof, from the PNT asymptotic expansion
   $\pi(x) = \frac{x}{\log x}\big(1 + \frac{1}{\log x} + \frac{2}{\log^2 x} + \frac{6}{\log^3 x} + \cdots\big)$
   by series inversion. Then they are the project's own result at tier **L0** and carry
   **no citation** — in particular not Kourbatov's.
2. **Delete the terms** and use the sourced $+o(1)$ form.

What is **not** admissible is keeping them with Kourbatov attached. That attribution is
not supported at this precision, and `citation-gate` should fail it.

Note also the notation collision: `attack.md`'s $B_n$ is this card's $f_n$
(**CC-07 §Traps**).

## Role in the proof-obligation tree

- **OB-C4** — the translation step, and the deck's only external mathematical dependency.
- **OB-D4** — Cramér-with-constant-$\le 1$. Theorem 5 is what makes the comparison
  precise: Firoozbakht's barrier is $\ell_k - 1 + o(1)$, so the required gap bound has
  leading constant exactly 1 and a *negative* second-order correction. Even granting
  Cramér's $\limsup R_k = 1$, that does not deliver $R_k < 1 - 1/\log p_k$ for **every**
  $k$ (**CC-21**).

## Traps

- The $o(1)$ is as $k \to \infty$ with **no effective rate stated in the theorem itself**;
  the effective content is the $3.83/\log p_k$ bracket, which is where the constant
  actually lives. Quote the bracket, not the $o(1)$, whenever a number is needed.
- Theorem 5's bracket is *one-sided tight at the top*: $f_k \le \ell_k - 1$ exactly. That
  upper bound is what **CC-16** (Theorem 1) turns into a statement about $g_k$.
- Do not confuse this expansion (of the **barrier**) with Theorem 4's coefficient family
  (of the **sufficient condition**, **CC-17**). Different series, both with small integer
  and decimal coefficients, both around $\ell_k - 1$.
