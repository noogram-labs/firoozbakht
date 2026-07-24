# CC-05 — The Cramér–Shanks–Granville ratio $R_n$

| | |
|---|---|
| **Kind** | definition |
| **Status** | **SOURCED** (V1 + `[C]`) — the definition is standard; the record value and its caveat are ledger rows, recomputed here |
| **Depends on** | CC-01, CC-02 |
| **Feeds** | OB-E1, OB-E5, FT-2, FT-3; CC-21, CC-26; `notebooks` debt #10 |
| **Ledger row** | `primegaplist_faq` (V1 + C), FAQ body ¶ — *"For the 1132 gap, the ratio is 0.9206, the largest value observed for any $p_1 > 7$ thus far."* |

## Statement

$$R_n \;:=\; \frac{g_n}{(\log p_n)^2}.$$

The gap measured against Cramér's conjectural extremal scale (**CC-21**). Firoozbakht,
in these coordinates, asserts (via **CC-06**/**CC-08**)

$$R_n \;\lesssim\; 1 - \frac{1}{\log p_n} \qquad\text{for every } n,$$

so the conjecture is *exactly* the statement that $R_n$ never quite reaches 1, with a
threshold that creeps upward toward 1 as $p_n$ grows.

## Measured values `[C]`

`verify_cards.py` CHECKS 8–9:

| statistic | value |
|---|---|
| $\max R_n$ over $7 < p_n < 5\times10^7$ | $\mathbf{0.7395}$ at $n = 1\,319\,945$, $p = 20\,831\,323$, $g = 210$ |
| $R_n > 1$ occurs at | **exactly** $p = 2\ (2.0814)$, $p = 3\ (1.6571)$, $p = 7\ (1.0564)$ |
| record (literature, V1 + C) | $0.9206385885574205$ at $p = 1\,693\,182\,318\,746\,371$, $g = 1132$ |

## The $p > 7$ caveat is real, not decorative

Ledger gap **G6**. The sentence *"the CSG ratio has never exceeded 1"* is **false as
written**: it exceeds 1 at three small primes. The Prime Gap List FAQ states the
restriction $p_1 > 7$ explicitly; any downstream leg that drops it is asserting
something the ledger's own source contradicts, and `citation-gate` should fail it.

This matters beyond bookkeeping. The three exceptional primes sit exactly where
Firoozbakht is tightest (**CC-14**: the criterion's dead band is non-empty at $n=2,4$,
i.e. $p = 3, 7$). *The regime where the CSG normalisation misbehaves and the regime where
the conjecture is tightest are the same regime.* That coincidence is not evidence of
anything, but it is the reason every "small $n$ is obviously slack" shortcut in this
corpus has been wrong.

## Role in the proof-obligation tree

- **FT-2** (threshold watch) is a scan of $R_n$ against $1 - 1/\log p_n$ over published
  gap tables — the cheapest possible refutation attempt. Per `outcomes.md#A5` it is a
  **screen, not a decision**: a firing is a nomination, adjudicated by the exact
  criterion (**CC-10**). Its four known false positives are $n = 1,2,3,4$.
- **OB-E5 / FT-3**: a proof that $\limsup R_n > 1$ refutes Firoozbakht. Believed
  (**CC-21**), unreachable (**CC-24**).
- **Debt #10** (`notebooks`): $\max_{p\le x} R$ is a deterministic step function from a
  complete enumeration — it has *no* error bars, and `outcomes.md#A8` is right that the
  original debt was ill-posed. The well-posed replacement is a **parametric fit**:
  estimate $C$ in $\max_{p\le x} R \approx C(1 - 2\log\log x/\log x)$ from the maximal-gap
  table, report a confidence interval from the residuals, and report the implied
  crossover height against $1 - 1/\log x$.

## Traps

- $R$ is a **logarithmic coordinate on search height**. A 5.4% shortfall in $R$
  (**CC-26**) is 8–15 orders of magnitude in $x$. `decompose.md#0.2`'s framing
  *"about 5.5%, not a factor of $10^k$"* is true in $R$ and badly false in cost.
- $\max_{p \le x} R$ is **monotone non-decreasing by construction**. Any argument that it
  is "not monotone" is comparing a max-over-range against a single point
  (`outcomes.md#A8`). The defensible statement is that the *shortfall* is non-monotone,
  because the required threshold $1 - 1/\log p$ rises with height while $R$ fluctuates.
- Normalisation by $\log^2 p_n$ vs. $\log^2 p_{n+1}$ vs. $\log^2$(midpoint) differs by
  $O(g/p)$ — invisible at the frontier, visible at $p < 100$ where the three $R>1$ cases
  live.
