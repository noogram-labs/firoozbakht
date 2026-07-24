# CC-22 — The Nicholson / Farhadian strengthening chain

| | |
|---|---|
| **Kind** | lemma |
| **Status** | **SOURCED (V0)** — the chain, its direction, and Visser's sufficient condition are all in one refereed paper. `[C]` on the chain direction. **Re-rated live** (deck delta **D5**). |
| **Depends on** | CC-10 |
| **Feeds** | a *new* node: strengthening; OB-F6b; `notebooks`, `lean-skeleton` |
| **Ledger row** | `visser2019verifying` (**V0**), **Conjecture 3, eqs. (2.4)–(2.6)** ¶; **§2, sentence after eq. (2.3)** ¶; **Sufficient condition 2, eq. (3.2)** ¶. Attributions: `nicholson2013` (V1), `farhadian_conj` (V1), `farhadian_jakimczuk2017` (V1). |

## Statements

Three conjectures, in one normalisation (Visser eqs. (2.4)–(2.6)):

| conjecture | barrier on $g_n$ | range |
|---|---|---|
| **Firoozbakht** | $g_n \le p_n\big(p_n^{1/n}-1\big)$ | $n \ge 1$ |
| **Nicholson** | $g_n \le p_n\big((n\ln n)^{1/n}-1\big)$ | $n > 4$ |
| **Farhadian** | $g_n \le p_n\Big(\big(p_n\frac{\ln n}{\ln p_n}\big)^{1/n}-1\Big)$ | $n > 4$ |

**The chain** (Visser §2, V0):

> *"the standard inequalities $n\ln n < p_n < n\ln p_n$ show that
> Farhadian $\Longrightarrow$ Nicholson $\Longrightarrow$ Firoozbakht."*

Mechanism: each barrier replaces $p_n$ inside the $n$-th root by something **smaller**
($n\ln n < p_n$), giving a **tighter** bound on the same $g_n$.

## Verification `[C]`

`verify_cards.py` CHECK 11, stride sample over $5 \le n \le 3\,001\,133$:

- Nicholson's barrier is $\le$ Firoozbakht's at every sampled index — **zero
  counter-samples** (the chain direction, confirmed numerically).
- Nicholson holds at every sampled index — zero failures.

## Why this was mis-rated, and what it buys

`decompose.md#5.3` excluded the chain as **[L3, recalled, unverified]** and `#7` filed
debt #6 as *"Low (unused)"*. That is **wrong on the facts** (`source-ledger.md#3.8`): the
chain is refereed, the implication direction is *proved* in a V0 source, and Visser
verifies all three conjectures to $2^{64}$.

"Unused" was an artifact of `decompose.md#2`'s tree having **no strengthening node**
(`outcomes.md#A11`). Adding one buys two things at essentially zero marginal cost:

1. **A second, more sensitive trend line on the same sieve pass.** A strengthening's
   slack fails *strictly before* Firoozbakht's. So a `notebooks` leg measuring the
   approach to threshold gets a leading indicator for free — and Nicholson's margin,
   being smaller, has better signal-to-noise for extrapolation than Firoozbakht's.

2. **Visser's Sufficient condition 2** (eq. (3.2), V0):
   $$g_n \;<\; f(n) \;=\; \big(\ln(n\ln n) - 1\big)\ln(n\ln n), \qquad n > 4,\ n \ge 2,$$
   which suffices for Nicholson and hence for Firoozbakht. Derived from (3.1) plus
   Dusart's $p_n > n(\ln(n\ln n)-1)$, $n \ge 2$.

   **This is index-based**: it needs no $\pi(x)$ inversion, no $p_n$ bound, no explicit
   Axler chain. It is a direct competitor to the integer criterion **CC-28** for the
   engine of a Lean finite-verification pass (OB-F6b), and `lean-skeleton` should
   evaluate both.

## Role in the proof-obligation tree

A **new node** — strengthening — feeding OB-E (a counterexample to Firoozbakht is a
counterexample to both strengthenings, but not conversely) and OB-F6b (a cheaper
sufficient criterion). Debt #6 is re-rated from *Low / unused* to **live**, co-owned by
`notebooks` and `lean-skeleton`.

## Traps

- **Direction.** Farhadian $\Rightarrow$ Nicholson $\Rightarrow$ Firoozbakht. So a
  counterexample to **Firoozbakht** refutes all three; a counterexample to **Farhadian**
  refutes only Farhadian. A `notebooks` leg watching Nicholson's margin is watching a
  *leading indicator*, not an equivalent statement.
- Nicholson and Farhadian carry $n > 4$; Firoozbakht is $n \ge 1$. The strengthenings say
  **nothing** at $n = 2, 4$ — the very indices where Firoozbakht is tightest (**CC-14**).
  The leading indicator is blind exactly where the phenomenon is sharpest.
- Attribution: `nicholson2013` and `farhadian_conj` are **unpublished/web** primary
  sources at V1, recorded only via Visser's reference list and OEIS A182514 (which
  returned HTTP 403 to `source-ledger`). Same status as `firoozbakht1982` itself: the
  *conjecture* is citable, the *primary document* does not exist in accessible form.
  `farhadian_jakimczuk2017` is the refereed statement of Farhadian's.
