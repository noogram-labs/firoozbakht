# CC-14 — The dead band $\mathcal{B}_n$

| | |
|---|---|
| **Kind** | lemma |
| **Status** | **PROVED** (proof below; `[C]` on every numeric claim). One upstream statement about it is **corrected** here. |
| **Depends on** | CC-06, CC-10, CC-12, CC-13 |
| **Feeds** | OB-C3, OB-E1, FT-5, FT-4; CC-28, CC-29 |
| **Ledger row** | NONE — derived here. Consistent with `kourbatov2015upper` **§3** ¶: *"given a pair of primes $p_k, p_{k+1}$, the validity of (2) alone is not enough for the verification of (1)"*, with witness $p_k = 2\,010\,733$, $q = 2\,010\,929$ (V0). |

## Statement

Between the (SUF) and (REF) thresholds lies an interval where **neither** screen fires:

$$\mathcal{B}_n \;:=\; \Big(\,T_n,\ \frac{T_n}{1 - T_n/p_n}\,\Big),
\qquad
W_n := \big|\mathcal{B}_n\big| = \frac{T_n^{\,2}}{p_n - T_n}
\qquad (\text{defined when } T_n < p_n).$$

If $g_n \in \mathcal{B}_n$, the screens are silent and only the exact criterion
(**CC-10**) decides.

Since $g_n$ is an **integer** (**CC-02**), the band is harmless at index $n$ exactly when
$\mathcal{B}_n \cap \mathbb{Z} = \emptyset$. A sufficient condition for that is
$W_n < 1$, i.e.

$$T_n^{\,2} + T_n \;<\; p_n .$$

## Proof

$\mathcal{B}_n$ is by construction the set of $g$ with $g > T_n$ (so **CC-12** does not
fire) and $g < T_n/(1 - T_n/p_n)$ (so **CC-13** does not fire — see **CC-13**'s solved
form). Its width is
$$\frac{T_n}{1 - T_n/p_n} - T_n = T_n\cdot\frac{T_n/p_n}{1 - T_n/p_n} = \frac{T_n^2}{p_n - T_n}.$$
$W_n < 1 \iff T_n^2 < p_n - T_n \iff T_n^2 + T_n < p_n$. An open interval of length
$< 1$ may still contain an integer, so this is sufficient for a *width* guarantee, not for
emptiness at a given $n$ — emptiness is checked directly. ∎

## Measured facts `[C]`

`verify_cards.py` CHECK 4, sieve to $5\times10^7$:

| claim | result |
|---|---|
| $T_n^2 + T_n < p_n$ — last failure | $n = \mathbf{483}$; holds for every $n \ge 484$ in range |
| number of failures below 484 | $\mathbf{479}$, not 483 |
| indices $\le 483$ where it **already holds** | $\mathbf{476, 478, 479, 480}$ |
| $g_n \in \mathcal{B}_n$ (band actually hit) | **exactly $n = 2$ and $n = 4$** |
| band contains *some* integer, $n \ge 484$ | $20079 / 3000650 = \mathbf{0.6692\%}$ of indices |
| at the record locus ($p \sim 1.7\times10^{15}$) | $W = 8.41\times10^{-10}$ — no integer can lie in it |

**Correction to `outcomes.md#A4`.** A4 states the condition *"holds for $n \ge 484$ and
**fails for every $n \le 483$**"*. The second half is too strong: it already holds at
$n = 476, 478, 479, 480$. The condition is **not monotone** below 484; 483 is the last
failure, not the start of an unbroken run. Everything else in A4 — the band, the width
formula, the $n \ge 484$ threshold, the $n = 2, 4$ hits, the 0.67% density — reproduces
exactly.

The two exceptional bands, in the **solved** form `[C]`:

| $n$ | $p_n$ | $\mathcal{B}_n$ | $W_n$ | $g_n$ |
|---|---|---|---|---|
| 2 | 3 | $(1.6479,\ \mathbf{3.6564})$ | $2.0085$ | $2$ — inside |
| 4 | 7 | $(3.4053,\ \mathbf{6.6313})$ | $3.2260$ | $4$ — inside |

**Second correction to `outcomes.md#A4`.** A4 quotes these bands as $(1.648, 2.747)$ and
$(3.405, 5.351)$. Those upper values are $T_n(1+g_n/p_n)$ — the (REF) threshold
*evaluated at the observed gap*, which is not a band edge at all, since it moves with
$g_n$. The genuine upper edge is the solved form $T_n/(1 - T_n/p_n)$ (**CC-13**), giving
$3.6564$ and $6.6313$. A4's conclusion is unaffected — $g_n$ is inside either way — but
its bands are understated by 33% and 24%, and its "67% wide" remark describes the wrong
interval (the true $W_2 = 2.0085$ is 122% of the lower edge).

## The point of the card

Two readings, both correct, and the second is the one that matters:

1. **Diagnostic.** The (SUF)/(REF) pair is not a decision procedure. Its blind spot is
   non-empty at exactly the two indices where Firoozbakht is tightest, and above $n=484$
   its emptiness is a *probabilistic fact of density $\approx W_n$*, never a theorem.
   Any pipeline treating the screens as a criterion is wrong 0.67% of the time in
   principle and 100% of the time at $n = 2, 4$.

2. **Prescriptive.** *Stop using the screens to adjudicate.* The band exists only because
   **CC-11** was applied where **CC-10** was available. The exact criterion has no band at
   any $n$ `[C, CHECK 3]`. Keep the screens for their cheapness — one multiplication,
   and in integer form (**CC-28**) `decide`-able in microseconds — and route every screen
   hit, and every index in the band, to **CC-10**.

## Role in the proof-obligation tree

- **OB-C3**: replaces "sharp … unconditionally" with the regime-qualified statement.
- **FT-5** (sandwich-sharpness experiment, `notebooks`): the well-posed version is
  *compare the exact predicate against each screen and count band residency by height*.
- **FT-4 / CC-01**: the $n = 2, 4$ band hits are the same two indices that supply the
  Lean fidelity anchors — $p_2^3 - p_3^2 = 2$, $p_4^5 - p_5^4 = 2166$. The tightest
  cases, the screen's blind spot, and the index-convention test are all the same two
  numbers.

## Traps

- **Name collision.** `attack.md#2` writes $B_n$ for the *exact barrier* (**CC-07**);
  `outcomes.md#A4` writes $B_n$ for *this band*. Same letter, same run directory, two
  objects differing by a factor of $\sim 10^{12}$ in size. This deck writes $f_n$ and
  $\mathcal{B}_n$.
- $W_n < 1$ is **sufficient, not necessary**, for an integer-free band, and $W_n \ge 1$
  does not imply the band is hit — witness the 479 indices below 484 where the width
  condition fails but only 2 actually contain $g_n$.
- The band width $8.4\times10^{-10}$ at the record locus is the *sandwich's* error. It is
  not the operationally relevant discard window — that is the $\ell_n$ **proxy's** 1.10
  gap units (**CC-08**), $1.3\times10^9$ times wider (`outcomes.md#A5`).
