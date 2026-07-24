# CC-10 — The exact criterion: F at $n$ $\iff$ $g_n < f_n$

| | |
|---|---|
| **Kind** | lemma |
| **Status** | **PROVED** (proof below; `[C]` verified against exact integers, no exceptions) |
| **Depends on** | CC-02, CC-07, CC-09 |
| **Feeds** | OB-C (replaces C1–C3), OB-E1, OB-E4, FT-1; CC-14, CC-22, CC-27 |
| **Ledger row** | `visser2019verifying` (**V0**), **Conjecture 3, eq. (2.4)** ¶ — *"$g_n \le p_n\big(p_n^{1/n}-1\big)$, $n \ge 1$"*, given there as an equivalent statement of Firoozbakht's conjecture. |

## Statement

For every $n \ge 1$:

$$\boxed{\;p_{n+1}^{\,n} < p_n^{\,n+1}
\quad\Longleftrightarrow\quad
g_n \;<\; f_n \;=\; p_n^{\,1+1/n} - p_n \;=\; p_n\big(e^{L_n/n} - 1\big).\;}$$

**No side condition. No asymptotic regime. No exceptional small-$n$ set. No external
explicit bound.** This is the criterion; everything else in the deck's OB-C layer is a
relaxation of it.

## Proof

By **CC-09**, F2 $\iff$ F3 $\iff$ $n\log p_{n+1} < (n+1)\log p_n$, i.e.

$$\log p_{n+1} \;<\; \frac{n+1}{n}\log p_n \;=\; \Big(1+\tfrac1n\Big)\log p_n .$$

$\exp$ is strictly increasing, so this is equivalent to

$$p_{n+1} \;<\; \exp\!\Big(\big(1+\tfrac1n\big)\log p_n\Big) \;=\; p_n^{\,1+1/n}.$$

Subtracting $p_n$ from both sides (an order isomorphism of $\mathbb{R}$) gives
$g_n = p_{n+1} - p_n < p_n^{1+1/n} - p_n = f_n$. ∎

Each step is an equivalence, so the chain reverses. The only facts used are $p_n > 0$,
strict monotonicity of $\log$ and $\exp$, and $n \ge 1$.

## Verification `[C]`

`verify_cards.py` CHECK 3: for every $n \le 200$, the predicate $g_n < f_n$ (computed as
`g < p*expm1(log(p)/n)`) and the exact integer predicate $p_{n+1}^n < p_n^{n+1}$ agree.
**Zero disagreements**, including at $n = 2$ and $n = 4$ where every relaxed criterion in
this corpus goes silent (**CC-12**, **CC-14**).

## What this supersedes

`decompose.md`'s OB-C chain runs: sandwich (C1) → threshold $T_n$ (C2) → sharpness
argument (C3) → $\ell_n$ translation (C4). `outcomes.md#A4` then found — four independent
ways — that C2/C3 leave a **dead band** in which neither direction fires, non-empty at
exactly $n = 2, 4$, the two tightest known indices.

That band is an artifact of C1, not of the problem. The sandwich exists to bound
$\log(1+x)$ when it cannot be evaluated; here it can be evaluated in closed form after
exponentiating. **CC-07** and this card do that.

| upstream object | status after CC-10 |
|---|---|
| C1 sandwich (**CC-11**) | retained — but only as the derivation of the cheap screens |
| C2 threshold $T_n$ (**CC-06**) | demoted to a screen and to the Lean cost optimisation (**CC-28**) |
| C3 "sharp to sixteen decimals, unconditionally" | **superseded** — nothing to be sharp about |
| C4 translation to $\ell_n$ (**CC-08**) | **retained and still load-bearing** — the literature and the barrier argument live in $\ell_n$ |

## Role in the proof-obligation tree

- **FT-1** (direct slack test) should be *stated* in this form: it is exact, needs no
  bignum exponentiation, and is one `expm1` per index.
- **OB-E4** (rigorous adjudication of a refutation candidate) becomes: evaluate $f_n$ in
  interval or rational arithmetic and compare against the integer $g_n$. Per
  `outcomes.md#A5`, E4 is the **sole adjudicator**; E1 and FT-2 are screens.
- **OB-E3** still applies: $f_n$ is increasing in $n$ for fixed $p_n$... **no** — see
  Traps. The antitonicity result is about $S_n$ (**CC-15**), and it is $S_n$, not $f_n$,
  that gives the cheap $\pi(x)$-lower-bound route.

## Traps

- **$<$ vs. $\le$.** Visser's eq. (2.4) states $g_n \le f_n$; this card states $g_n < f_n$.
  They define the same predicate: equality would force $p_{n+1}^n = p_n^{n+1}$, impossible
  for distinct primes by unique factorization. Say which you mean anyway — a
  formalization must pick one.
- **$f_n$ depends on $n$ *and* on $p_n$**, unlike $\ell_n$ which depends on $p_n$ alone.
  So a refutation search that knows $(p, g)$ but not $n$ cannot evaluate $f_n$ directly.
  It needs a *lower* bound on $n$ — which is cheap (**CC-15**, **CC-19**) — because $f_n$
  is **decreasing** in $n$ for fixed $p_n$.
- **Do not compute `p*(p**(1.0/n) - 1)`.** For large $n$ this is $p \times$ (a
  cancellation of two numbers near 1) and loses most of its significant digits. Use
  `p*expm1(log(p)/n)`, and above $p \sim 10^{16}$ use exact arithmetic (**CC-27**).
- The exact criterion is **not** cheaper in Lean. It needs `Real.rpow`. The Lean-cheap
  route is **CC-28**; the two are provably equivalent statements about the same $n$, and
  each should be used where it is cheap.
