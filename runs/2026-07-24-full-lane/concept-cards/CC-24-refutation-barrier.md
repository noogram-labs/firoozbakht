# CC-24 — The refutation-side barrier

| | |
|---|---|
| **Kind** | barrier |
| **Status** | **SOURCED (V1)** for the best known large-gap results; the comparison against $\ell_n$ is `[C]`-computable and elementary |
| **Depends on** | CC-05, CC-08, CC-21 |
| **Feeds** | OB-E5, OB-E0 (new), FT-3; CC-30 |
| **Ledger row** | `fgkmt2018long` (**V1**), abstract ¶ — $\max_{p_{n+1}\le X}(p_{n+1}-p_n) \gg \frac{\log X\,\log\log X\,\log\log\log\log X}{\log\log\log X}$. `fgkt2016large` (**V1**), abstract ¶ — the same with $(\log\log\log X)^2$ in the denominator. `granville1995harald` (**V0**) for the 1995 Erdős–Rankin form. `rankin1938difference` (**V3**, metadata only). |

## Statement

To refute Firoozbakht one needs a gap of size $\gtrsim \log^2 p$ (**CC-08**, **CC-16**).
The best **proved** lower bounds on large gaps are:

$$G(X) \;\gg\; \frac{\log X\ \log\log X\ \log\log\log\log X}{\log\log\log X}
\qquad \text{(Ford–Green–Konyagin–Maynard–Tao 2018, V1)}$$

$$G(X) \;\ge\; f(X)\,\frac{\log X\ \log\log X\ \log\log\log\log X}{(\log\log\log X)^2},\quad f(X)\to\infty
\qquad \text{(Ford–Green–Konyagin–Tao 2016, V1)}$$

Both are $o\!\big(\log^2 X\big)$ — **too small by a factor $\sim \log X/\log\log X$**,
which diverges. The constructive route (Erdős–Rankin sieving of an admissible interval,
`granville1995harald` V0 for the 1995 form) produces the same shape.

> **No construction and no proved lower bound reaches the Firoozbakht threshold, and the
> shortfall grows without bound with height.**

## The symmetry with CC-23

This is the point of pairing the two cards. The proof side fails because the best **upper**
bounds are a power of $p$ where a power of $\log p$ is needed. The refutation side fails
because the best **lower** bounds are $\log p \times$(iterated logs) where $\log^2 p$ is
needed. *The target sits in a window that current technique brackets from neither side.*

That is a cleaner statement of the polymer's strategic situation than "proof blocked,
refutation live", and it is fully ledger-backed.

## Then why is the refutation branch still "live"?

Because refutation does not require a theorem — **one witness suffices**. The live
obligation is OB-E1–E4 (**search**), not OB-E5 (**proof**). And the empirical distance is
small: the shortfall at the record locus is $\mathbf{1.05426}$ (**CC-26**), a 5.4% deficit
in $R$.

But 5.4% in $R$ is not 5.4% in cost. $R$ is a **logarithmic coordinate on height**
(`outcomes.md#A8`): the same shortfall is $\sim 6\times$ in rarity and **8–15 orders of
magnitude in search height**. Maximal-gap tables to $4\times10^{18}$ represent decades of
distributed compute. `decompose.md#0.2`'s framing — *"about 5.5%, not a factor of
$10^k$"* — is true in the $R$ coordinate and badly false in the coordinate that governs
cost. Both sentences must appear together or neither should.

**Operational conclusion, unchanged from `decompose.md#3.2`**: `notebooks` legs should
target **measurement**, not search — characterise $\max_{p\le x} R$ against height via the
parametric fit of **CC-05**, and report the implied crossover honestly.

## The missing tree node: OB-E0

`decompose.md#2`'s OB-E offers exactly two shapes: exhibit one $n$ (E1–E4), or prove
$\limsup R_n > 1$ (E5). Between them lies **the entire space of non-constructive finite
existence arguments** — averaging, pigeonhole, dichotomy — which is the standard shape in
this subfield (`outcomes.md#A9`). Add **OB-E0**. Also add **OB-E6**, conditional
refutation: OB-D has three conditional slots and OB-E has zero, with no reason given.

## A concrete deliverable nobody has computed

Worth exactly one `proof-attempt` leg (`decompose.md#3.4`): compute the shortfall factor
of the best *constructive* large-gap bound against $f_n$ at concrete heights —
$10^{18}$, $10^{100}$, $10^{1000}$. That number is the honest **"distance from provable
constructions to refutation"**; it is a different quantity from the empirical shortfall
of **CC-26**; and it does not appear anywhere in the literature in this normalisation.

## Traps

- **The iterated-log exponent.** `fgkmt2018long` has $(\log\log\log X)^{1}$ in the
  denominator; `fgkt2016large` has it **squared**. Getting this wrong is the easiest
  citation error in this family (`source-ledger.md#3.7`), and `citation-gate` checks it
  specifically.
- These are bounds on $G(X) = \max_{p_{n+1}\le X} g_n$, a max over a range — not on any
  individual gap, and not constructive in the sense of naming a locus.
- `rankin1938difference` is **V3, metadata only**. Cite `granville1995harald` (V0) for the
  Erdős–Rankin form, or the FGK(M)T papers for the modern one.
