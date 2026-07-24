# CC-04 — The merit $M_n$

| | |
|---|---|
| **Kind** | definition |
| **Status** | **PROVED** (definition); the PNT heuristic behind it is **SOURCED** |
| **Depends on** | CC-01, CC-02 |
| **Feeds** | CC-05; `notebooks` (the vocabulary the gap literature is written in) |
| **Ledger row** | NONE for the definition. The normalising heuristic (average gap near $x$ is $\sim\log x$) is the prime number theorem — standard, and used here only as vocabulary, never as a bound. |

## Statement

$$M_n \;:=\; \frac{g_n}{\log p_n}.$$

The gap measured in units of the *average* gap near $p_n$. Merit is the standard
currency of the prime-gap literature and of every gap table a `notebooks` leg will read;
it appears here so that this deck's criteria can be translated into that vocabulary
without a silent renormalisation.

## Translation table

Firoozbakht's criterion, restated in merit (using $T_n$ from **CC-06**):

$$\text{(SUF) } g_n \le T_n
\;\iff\;
M_n \;\le\; \frac{T_n}{\log p_n} \;=\; \frac{p_n}{n}
\;=\; \log p_n - c_n, \qquad c_n \approx 1.03 .$$

So in merit coordinates the conjecture says: **the merit of a gap must stay below
roughly $\log p_n - 1$.** At the record locus ($\log p \approx 35.07$) that is a merit
ceiling of $\approx 34.03$; the observed merit there is $1132/35.0654 = 32.28$.

Because $\log p_n$ grows, the *merit ceiling grows too* — this is why "record merit" and
"record CSG ratio" are different maxima and why the shortfall is not monotone in height
(**CC-05**, **CC-26**).

## Role in the proof-obligation tree

None structurally — merit is a change of units, not a step. It earns a card because
**every external data source this polymer will consume is indexed by merit**, and a leg
that compares a merit against a CSG threshold, or a merit ceiling against a raw gap, has
made a $\log p$-sized error that no test in `decompose.md` would catch.

## Traps

- Merit is normalised by $\log p_n$; the CSG ratio (**CC-05**) by $\log^2 p_n$. They
  differ by a factor of $\log p_n \approx 35$ at the frontier. Both are called "the
  normalised gap" in different papers.
- "Record merit" $\ne$ "record CSG ratio" $\ne$ "largest gap". Three orderings, three
  different record holders. `decompose.md#3.2` quotes all three in one table; that table
  is correct but invites exactly this confusion.
- Some sources normalise by $\log p_{n+1}$ or by $\log$ of the interval midpoint. The
  difference is $O(g/p)$ — negligible at the frontier, **not** negligible at $p < 100$
  where this deck's tightest cases live.
