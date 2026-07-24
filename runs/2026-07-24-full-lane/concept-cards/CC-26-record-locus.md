# CC-26 — The record locus and the 1.05426 shortfall

| | |
|---|---|
| **Kind** | fact |
| **Status** | **SOURCED (V0 + V1) + `[C]`** — two mutually independent routes, plus 60-digit recomputation |
| **Depends on** | CC-05, CC-07, CC-25 |
| **Feeds** | OB-E1, FT-2; `write-paper`, `notebooks` |
| **Ledger row** | `kourbatov2015upper` (**V0 + C**), **Table 1, last row (p. 4)** ¶ — $k = 49\,749\,629\,143\,526$; $p_k = 1\,693\,182\,318\,746\,371$; $p_{k+1}-p_k = 1132$; $f_k = 1193.418$; $\ell_k = 1194.516$; caption *"$p_k \in$ A111943"*. `primegaplist_faq` (**V1 + C**) ¶ — *"For the 1132 gap, the ratio is 0.9206, the largest value observed for any $p_1 > 7$ thus far."* |

## The locus

| quantity | value |
|---|---|
| index $k$ | $49\,749\,629\,143\,526$ |
| prime $p_k$ | $1\,693\,182\,318\,746\,371$ |
| gap $g_k$ | $1132$ |
| $\log p_k$ | $35.0653861820133348\ldots$ `[C]` |
| **exact barrier** $f_k$ | $\mathbf{1193.41777829404\ldots}$ `[C]` — Table 1: $1193.418$ ✓ |
| proxy $\ell_k$ | $1194.51592191172\ldots$ `[C]` — Table 1: $1194.516$ ✓ |
| linearisation $T_k$ | $1193.41777829362\ldots$ `[C]` |
| CSG ratio $R_k$ | $0.9206385885574205\ldots$ `[C]` — FAQ: $0.9206$ ✓ |
| **shortfall $f_k/g_k$** | $\mathbf{1.054255987892\ldots}$ `[C]` |

`verify_cards.py` CHECK 7, 60-digit `mpmath` from exact integer inputs. No floating-point
path taken.

## The shortfall — the polymer's headline number

$$\frac{f_k}{g_k} = \frac{1193.41777829\ldots}{1132} = \mathbf{1.05426}.$$

**Read it as: the closest approach on record misses refuting Firoozbakht by 5.4%.** The
gap would have to have been 1194 instead of 1132.

Three things about this number that earlier legs got wrong:

1. **It is 1.05426, not 1.055.** `decompose.md#0.2`/`#3.2` state 1.055; the corrected
   value is 1.0543 (`outcomes.md#A3`) and the exact value is 1.054256.
2. **It is L0/L1, not L2.** It comes from Kourbatov's *published* $f_k$ (V0) divided by
   the *published* gap. No $c_n$ interval, no explicit $p_n$ bound, no Dusart, no Axler
   is needed — because $f_k$ is a closed-form function of $(p_k, k)$ (**CC-07**).
   `decompose.md`'s derivation routed through an L2 node (**CC-06**, **CC-20**) and
   therefore could not tier it L0 (`outcomes.md#A3`); the exact barrier removes the
   node entirely.
3. **The explicit route agrees anyway.** Dusart Props. 6.6/6.7 certify
   $T_k/g_k \in (1.0541938,\ 1.0542920)$, i.e. $1.05424 \pm 0.00005$
   (`source-ledger.md#5`). The exact value lies inside. Keep this as the **audit trail**,
   not as the arithmetic (**CC-20**).

## Two independent routes, which is why this is closed

Debt #1 asked for the record CSG locus. It is confirmed by:

- a **refereed table** (`kourbatov2015upper` Table 1, V0), giving $k$, $p_k$, $g_k$,
  $f_k$, $\ell_k$; and
- a **community record** (`primegaplist_faq`, V1), giving the ratio $0.9206$;

and recomputed here to 60 digits from the integers alone. Three routes, one answer.

## What the number does not mean

- **The record locus is not the largest known gap.** The largest known maximal gap sits
  at $p \approx 1.836\times10^{19}$ with $g = 1550$, whose $R = 0.7878$ and shortfall
  $\approx 1.24$ — *further* from refuting, four orders of magnitude higher
  (`decompose.md#3.2`). Three different maxima, three different loci (**CC-04**).
- **5.4% is not 5.4% of anything expensive.** $R$ is a logarithmic coordinate on height:
  the same shortfall is $\sim 6\times$ in rarity and **8–15 orders of magnitude in search
  height** (`outcomes.md#A8`, **CC-24**). *"About 5.5%, not a factor of $10^k$"* is true
  in $R$ and false in cost.
- **It is not a trend.** Whether $\max_{p\le x} R$ approaches 1 is exactly the open
  empirical question, and the data does not yet show it. That is **CC-05**'s parametric
  fit, and it is `notebooks`' real deliverable.

## Traps

- Carry the **$p_1 > 7$ caveat** (**CC-05**, ledger gap **G6**): $R > 1$ at $p = 2, 3, 7$.
- `write-paper`: use **1.05426**, sourced directly to `kourbatov2015upper` Table 1's
  published $f_k = 1193.418$ — not 1.055, and not via any $c_n$ interval.
- OEIS A111943, A002386, A005250 are all **V2/V3** (HTTP 403 to the `source-ledger` leg).
  Kourbatov's *citation* of A111943 is V0; the sequence *content* is not verified.
