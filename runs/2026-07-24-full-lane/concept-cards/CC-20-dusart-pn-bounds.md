# CC-20 — Dusart's explicit $p_k$ bracket

| | |
|---|---|
| **Kind** | lemma (external, explicit) |
| **Status** | **SOURCED (V0 + C)** — recomputed at the record locus in `source-ledger.md#5` |
| **Depends on** | — (external input) |
| **Feeds** | OB-C4, W4; CC-06 (the $c_n$ constant), CC-26 |
| **Ledger row** | `dusart2010estimates` (**V0 + C**), **Proposition 6.6** ¶ and **Proposition 6.7** ¶. Companions in the same source: **Prop. 6.8** (prime in $(x, x(1+1/(25\ln^2 x))]$, $x \ge 396\,738$), **Prop. 6.3** (Chebyshev $\vartheta$). |

## Statements

$$p_k \;\le\; k\left(\ln k + \ln\ln k - 1 + \frac{\ln\ln k - 2}{\ln k}\right)
\qquad (k \ge 688\,383) \tag{Prop. 6.6}$$

$$p_k \;\ge\; k\left(\ln k + \ln\ln k - 1 + \frac{\ln\ln k - 2.1}{\ln k}\right)
\qquad (k \ge 3) \tag{Prop. 6.7}$$

Together: a **three-term bracket** on $p_k/k$, hence on
$c_k := \log p_k - p_k/k$ (**CC-06**), valid for $k \ge 688\,383$.

## What it certifies `[C, source-ledger §5]`

At the record locus $k = 49\,749\,629\,143\,526$ (validity satisfied):

| | |
|---|---|
| bracket on $p_k$ | $1\,693\,082\,432\,978\,151.87 \le p_k \le 1\,693\,240\,177\,892\,500.92$ ✓ |
| $\Rightarrow c_k \in$ | $(\mathbf{1.030154},\ \mathbf{1.033325})$ |
| true value | $1.031317$ |
| $\Rightarrow$ certified shortfall $T_k/g_k \in$ | $(1.0541938,\ 1.0542920)$, i.e. $\mathbf{1.05424 \pm 0.00005}$ |

This **confirms** `outcomes.md#A3`'s claim that the three-term bound pins
$c_n \in (1.030, 1.034)$, and tightens A3's stated $\pm 0.001$ tenfold.

It also confirms A3's companion charge: the **two-term** bracket that
`decompose.md#2.3-C4` actually cites has width exactly $k$ and permits only
$c_k \in (0.076168,\ 1.076168)$ — so C4's stated $c_n \in (0.9, 1.2)$ is **not derivable
from its own citation**.

## The card's real conclusion: you do not need it

The certified shortfall $1.05424 \pm 0.00005$ is *superseded* by the exact value
$f_k/g_k = 1.054256$ (**CC-26**), which comes from **CC-07** and needs **no explicit
$p_n$ bound at all** — because Kourbatov publishes $f_k$ directly in Table 1 (V0) and
because $f_k$ is computable in closed form from $(p_k, k)$.

So this card's role is **audit trail, not arithmetic** (`outcomes.md`, sequencing note:
debt #4 is critical for the audit trail, not for the number). Keep it because:

- $T_n$ (**CC-06**) and the merit translation (**CC-04**) do need $p_n/n$;
- W4 and the $\ell_n$ dictionary need $c_n \to 1$ with an effective rate;
- a paper claiming an explicit route must be able to exhibit one.

## Traps

- **Validity range.** Prop. 6.6 requires $k \ge 688\,383$; Prop. 6.7 only $k \ge 3$. The
  bracket is only as valid as its weaker half. Below $688\,383$ there is no three-term
  upper bound from this source.
- **$c_n - 1$ changes sign at $n = 61$** (`outcomes.md#A4`). Any use of
  "$p_n/n \approx \log p_n - 1$" as a *one-sided* bound is wrong on one side of that
  index, and $n = 61$ is far below the validity range anyway.
- Prop. 6.8 (a prime in $(x,\ x(1+1/(25\ln^2 x))]$) is **not** a Firoozbakht tool: the
  interval length is $\sim x/\ln^2 x$, astronomically larger than the required
  $\sim \ln^2 x$. It belongs in the barrier catalogue (**CC-23**), as an illustration of
  how far the explicit machinery is from the target.
- `dusart2018explicit` has sharper constants and is **V3, metadata only** (PDF host had
  an expired certificate; publisher record paywalled). Use the 2010 paper.
