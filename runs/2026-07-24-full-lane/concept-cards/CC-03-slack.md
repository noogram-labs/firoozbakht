# CC-03 — The slack $S_n$

| | |
|---|---|
| **Kind** | definition |
| **Status** | **PROVED** |
| **Depends on** | CC-01, CC-02 |
| **Feeds** | OB-C, OB-E4, OB-F7; CC-09, CC-15, CC-27 |
| **Ledger row** | NONE — derived here |

## Statement

$$S_n \;:=\; \log p_n \;-\; n\log\!\left(1 + \frac{g_n}{p_n}\right)
\;=\; (n+1)\log p_n - n\log p_{n+1}.$$

**Firoozbakht holds at $n$ $\iff$ $S_n > 0$** (form F5 of **CC-09**).

$S_n$ is the conjecture's *defect function*: it measures, in units of $\log$, how much
room is left before the inequality fails at index $n$. It is the quantity every numeric
pipeline in this polymer actually evaluates.

### Normalised slack

$$\hat S_n := \frac{S_n}{\log p_n} \in (0, 1] \quad\text{when F holds at } n,$$

which is scale-free and therefore the right statistic for comparing tightness across
indices. The two are **not interchangeable**: `decompose.md#5.4` quoted
$\min_{n>200} \hat S_n$ where it meant $\min_{n>200} S_n$, and the substitution is what
let a wrong error bound survive (`outcomes.md#A1`). This deck states which one it means,
every time.

## Measured values `[C]`

`verify_cards.py` CHECK 2, sieve to $5\times10^7$ ($n \le 3\,001\,133$):

| statistic | value | at |
|---|---|---|
| $\min_n S_n$ | $\mathbf{0.076961}$ | $n = 2$ ($p = 3$) |
| $\min_{n>200} S_n$ | $\mathbf{1.700800}$ | $n = 217$ ($p = 1327$) |
| runtime float guard at $p_{\max}$ | $1.181\times10^{-14}$ | — |

Two things to read off. First, the **global minimum sits at $n = 2$** — the conjecture
is tightest at the very smallest indices, not in the large-gap region. Second,
`decompose.md#5.4`'s claim that $\min_{n>200} S_n$ "exceeds 3.5" is wrong: it is
$1.7008$. The verdict (no counterexample) is unaffected — $1.7$ is still $10^{14}$ times
the guard — but the number was asserted *about* the computation rather than *printed by*
it, which is exactly the failure class `outcomes.md`'s D1 note names.

## Role in the proof-obligation tree

- **OB-E4** (rigorous slack evaluation) is the sole adjudicator of any refutation
  candidate — the screens (**CC-12**, **CC-13**) only nominate.
- **OB-F7** is $S$'s antitonicity in $n$ (**CC-15**).
- $S_n$ is what **CC-27** teaches you to compute without lying.

## Traps

- **$S_n$ is not the gap's slack.** $S_n$ is measured in log units; the *gap* slack is
  $f_n - g_n$ (**CC-07**). They are related by $S_n \approx n(f_n-g_n)/p_n$ near the
  threshold, a factor of $\sim n/p_n$ apart. Quoting one as the other is a
  $10^{13}$-fold error at the record locus.
- The two-term form $(n+1)\log p_n - n\log p_{n+1}$ is a **catastrophic-cancellation
  trap** in floating point: it subtracts two quantities of size $n\log p_n \sim 10^7$ to
  get a result of size $1$. Always use the $\log$1p form. See **CC-27**.
- $S_n > 0$ is *strict*. Equality would require $p_{n+1}^n = p_n^{n+1}$, impossible for
  distinct primes by unique factorization — so the boundary case is vacuous, and a
  formalization may use $\le$ or $<$ freely provided it says which.
