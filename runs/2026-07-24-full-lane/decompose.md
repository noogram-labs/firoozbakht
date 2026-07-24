# Firoozbakht's conjecture — attack-surface decomposition

**Leg**: `decompose` (node 1 of the `math-attack` polymer)
**Node**: decompose (formula `task-work-reasoning`)
**Crew role**: concept-writer
**Formal backend requested**: Lean 4 / Mathlib
**Date**: 2026-07-24

> **Posture.** The conjecture is *open*. Nothing below assumes it true. Every
> statement is tagged with a confidence tier (§0.1). The single most important
> finding of this decomposition is stated up front in §0.2 so that downstream
> legs do not spend compute rediscovering it.

---

## 0. Preliminaries

### 0.1 Confidence tiers used throughout

| Tier | Meaning |
|------|---------|
| **L0** | Proved *in this document*, from first principles, with the proof written out. |
| **L1** | Standard textbook fact; proof omitted but routine and available in Mathlib or any analytic-number-theory text. |
| **L2** | Attributed to the literature. **Locator not yet verified against source** — the `source-ledger` leg must confirm author, year, venue, and the exact statement before any downstream leg leans on it. |
| **L3** | Heuristic, numerical, or recalled-from-memory. Explicitly *not* evidence. |
| **[COMPUTED]** | Produced by a computation run inside this leg; the script is reproducible (§5.4). |

**Discipline for downstream legs**: an L2 tag is a *debt*, not a citation. Do not
promote an L2 to an L1 by restating it. `source-ledger` closes these; `citation-gate`
audits that closure.

### 0.2 The headline finding

Firoozbakht's conjecture is **not an isolated curiosity about exponentials**. It is,
up to a relative error of size $O\!\left((\log p)^2/p\right)$, *exactly equivalent* to a
Cramér-type prime-gap bound with constant $1$:

$$
\text{Firoozbakht at } n \iff g_n \lesssim \frac{p_n}{n}\log p_n \approx (\log p_n)^2 - \log p_n .
$$

(Proved as L0 in §2.3, obligations C1–C4.) Two consequences dominate the whole attack surface:

1. **Any proof must clear the Cramér barrier.** It must establish
   $g_n < (1-o(1))(\log p_n)^2$ for *every* $n$, with explicit constants. The best
   unconditional bound is $g_n \ll p_n^{0.525}$ [L2, Baker–Harman–Pintz]; RH gives
   $g_n \ll \sqrt{p_n}\log p_n$ [L2]. Both are astronomically weaker. **There is no
   known route, conditional or unconditional, that reaches the required strength.**
2. **Any refutation needs only a ~5% improvement in a quantity people already track.**
   The Cramér–Shanks–Granville ratio $R_n := g_n/(\log p_n)^2$ must reach
   $\approx 1 - 1/\log p_n$. The record is $R \approx 0.9206$, at the gap $g = 1132$
   following $p = 1\,693\,182\,318\,746\,371$ [L2/L3]. There $\log p \approx 35.07$, so
   refutation would need $R \ge 0.9715$: a shortfall factor of **$1.055$** [COMPUTED
   from the quoted $(p,g)$]. The gap between "what has been observed" and "what refutes
   the conjecture" is about 5.5%, not a factor of $10^{k}$.

Therefore: **the refutation branch is the live branch**, and the proof branch should be
budgeted as *barrier-mapping* (proving that the obstruction is real and locating it
precisely), not as an attempt to close. This should be the primary input to
`frame-deliberation`.

### 0.3 Notation and index conventions

- $p_n$ = the $n$-th prime, **1-indexed**: $p_1 = 2,\ p_2 = 3,\ p_3 = 5,\dots$
- $g_n := p_{n+1} - p_n$ (the $n$-th prime gap).
- $\log$ = natural logarithm throughout.
- $M_n := g_n/\log p_n$ (the *merit* of the gap).
- $R_n := g_n/(\log p_n)^2$ (the *Cramér–Shanks–Granville ratio*).
- $\pi(x)$ = the prime-counting function.

> ⚠️ **Off-by-one hazard.** Mathlib's `Nat.nth Nat.Prime` is **0-indexed**:
> `Nat.nth Nat.Prime 0 = 2`. The conjecture as posed in the brief is 1-indexed.
> Every Lean statement in §6 fixes this explicitly. A silent index shift changes
> the conjecture (the $n$ in the exponent is not decoration — it is load-bearing;
> see §4, test FT-4). This is obligation **OB-F1**.

---

## 1. Formal restatement

### 1.1 The five equivalent forms

Let $n \ge 1$.

| Tag | Statement | Domain |
|-----|-----------|--------|
| **F1** *(as posed)* | $p_{n+1}^{1/(n+1)} < p_n^{1/n}$ | $\mathbb{R}$, `rpow` |
| **F2** *(integer)* | $p_{n+1}^{\,n} < p_n^{\,n+1}$ | $\mathbb{N}$, `Monoid.npow` |
| **F3** *(log)* | $n\log p_{n+1} < (n+1)\log p_n$ | $\mathbb{R}$ |
| **F4** *(monotone sequence)* | $a_{n+1} < a_n$ where $a_n := \dfrac{\log p_n}{n}$ | $\mathbb{R}$ |
| **F5** *(gap / slack)* | $S_n > 0$ where $S_n := \log p_n - n\log\!\left(1 + \dfrac{g_n}{p_n}\right)$ | $\mathbb{R}$ |

**Firoozbakht's conjecture** = "$\text{F}k$ holds for all $n \ge 1$", for any one (hence all) of $k = 1,\dots,5$.

### 1.2 Proof of the equivalences  [L0]

All five are equivalent *pointwise in $n$* — no quantifier juggling, no asymptotics.

**F1 ⟺ F3.** $p_n \ge 2 > 0$ and $p_{n+1} > 0$, so both sides of F1 are positive and
$\log$ is a strictly increasing bijection on $(0,\infty)$. Taking logs,
F1 $\iff \frac{1}{n+1}\log p_{n+1} < \frac{1}{n}\log p_n$. Multiplying by
$n(n+1) > 0$ preserves the strict inequality and yields F3. ∎

**F3 ⟺ F2.** $\exp$ is strictly increasing, and for a natural $m \ge 1$ and integer
$k \ge 0$, $m^k = \exp(k \log m)$. Apply to $m = p_{n+1}, k = n$ and
$m = p_n, k = n+1$. Both sides of F2 are positive naturals, so the real inequality
and the natural-number inequality coincide. ∎

**F3 ⟺ F4.** F3 divided by $n(n+1) > 0$ is literally $\frac{\log p_{n+1}}{n+1} < \frac{\log p_n}{n}$. ∎

**F3 ⟺ F5.** Compute the F3 defect:
$$
(n+1)\log p_n - n\log p_{n+1}
= \log p_n - n\bigl(\log p_{n+1} - \log p_n\bigr)
= \log p_n - n\log\frac{p_{n+1}}{p_n}
= \log p_n - n\log\!\left(1+\frac{g_n}{p_n}\right) = S_n .
$$
So F3 $\iff S_n > 0$. ∎

**Consequence (design note).** F2 is a statement about naturals with *no real numbers,
no logarithms, no `rpow`*. This is the form to formalize first (§6): it makes the
finite-verification leg (OB-B) a pure `decide`/`norm_num` computation and removes an
entire class of Lean friction (`Real.rpow` monotonicity side conditions, `0 < x`
hypotheses threaded everywhere). F1 is then recovered from F2 by one bridge lemma.

### 1.3 What F4 says, in words

$a_n = (\log p_n)/n$ is the *average logarithmic growth of the primes per index step*.
Since $p_n \sim n\log n$, $a_n \approx (\log n)/n \to 0$, and the natural decrement of
$(\log n)/n$ per unit step is $\approx (\log n - 1)/n^2$. A single gap $g_n$ pushes
$\log p_n$ up by $\approx g_n/p_n \approx g_n/(n\log n)$. Firoozbakht asserts that no
gap is ever large enough to overcome the natural decrement. Setting the two against each
other reproduces §0.2's threshold. **This is the intuition the whole problem reduces to:
*the primes must never stutter harder than $1/n$ decay can absorb*.**

---

## 2. The proof-obligation tree

Any complete proof *or* refutation must pass through these nodes. Obligations are
labelled `OB-x`; each carries a **status** and a **cost estimate**.

```
                          F : ∀n≥1, p_{n+1}^n < p_n^{n+1}
                                       │
        ┌──────────────────────────────┼──────────────────────────────┐
        │                              │                              │
   OB-A  equivalence            OB-B  finite                    OB-C  reduction
        layer (F1..F5)          verification                    to a gap bound
        │                       (n ≤ N₀)                              │
        │  A1 F1⟺F2                │  B1 sieve certificate            │  C1 sandwich lemma
        │  A2 F2⟺F3                │  B2 index-tracking               │  C2 explicit p_n/n
        │  A3 F3⟺F4                │  B3 trust chain                  │  C3 threshold T_n
        │  A4 F3⟺F5                                                   │  C4 two-sided sharpness
        │                                                             │
        └───────────────┬─────────────────────────────────────────────┘
                        │  (F reduced to: ∀ n>N₀,  g_n < T_n)
          ┌─────────────┴───────────────┐
          │                             │
    OB-D  PROOF branch            OB-E  REFUTATION branch
          │                             │
          │ D1 unconditional gaps       │ E1 exhibit a witness n
          │ D2 gaps under RH            │ E2 consecutivity certificate
          │ D3 gaps under GRH/DHL       │ E3 index lower bound  ← cheap! (§3.2)
          │ D4 Cramér-with-constant-≤1  │ E4 rigorous slack evaluation
          │    ⟵ THE BARRIER            │ E5 heuristic → theorem
          │                             │    (limsup R_n > 1)
          └─────────────┬───────────────┘
                        │
                 OB-F  Lean formalization
                        F1 index convention
                        F2 F2-form statement
                        F3 bridge F2→F1
                        F4 what is `sorry`-free vs. axiom-gated
```

### 2.1 OB-A — the equivalence layer

| Obligation | Statement | Status | Lean cost |
|---|---|---|---|
| **A1** | F1 ⟺ F2 | **DISCHARGED** (§1.2) [L0] | medium (`rpow` bridge) |
| **A2** | F2 ⟺ F3 | **DISCHARGED** (§1.2) [L0] | low |
| **A3** | F3 ⟺ F4 | **DISCHARGED** (§1.2) [L0] | low |
| **A4** | F3 ⟺ F5 | **DISCHARGED** (§1.2) [L0] | low |

This layer is *complete and closed*. No downstream leg needs to revisit it. It is also
the only part of the whole tree that is fully formalizable today with certainty.

### 2.2 OB-B — the finite-verification layer

**Claim.** For any fixed $N_0$, "F holds for all $n \le N_0$" is decidable and, in F2
form, is a finite conjunction of natural-number inequalities.

| Obligation | Content | Status |
|---|---|---|
| **B1** | A verified list of consecutive primes up to height $H$ | routine (sieve); the *certificate* is the work |
| **B2** | Correct index tracking: the $n$ used must be the true $\pi(p_n)$ | routine but **error-prone**; see §2.5 |
| **B3** | Trust chain: who computed, with what code, reproducible? | **OPEN** — the literature value ($p_n < 4\times10^{18}$, [L2] Kourbatov) is a *citation*, not a machine-checked certificate |

**Honest boundary.** Finite verification can never prove F. It only shifts the problem
to $n > N_0$ and — importantly — **it is the only branch that has actually produced a
result to date**. Everything known about F is: "no counterexample below $4\times10^{18}$".

### 2.3 OB-C — the reduction to a gap bound (the load-bearing reduction)

This is where the decomposition earns its keep. Everything reduces to a two-sided
sandwich on $S_n$.

#### C1 — Sandwich lemma [L1]

**Lemma.** For all real $x > 0$: $\dfrac{x}{1+x} < \log(1+x) < x$.
*(Standard [L1]; present in Mathlib, but the lemma names are **[L3] recalled, not
resolved** — candidates are of the `Real.add_one_le_exp` / `Real.log_lt_sub_one_of_ne`
family. Pinning them by compilation is obligation OB-F5.)*

Apply with $x = g_n/p_n$, so $1+x = p_{n+1}/p_n$:
$$
\frac{n\,g_n}{p_{n+1}} \;<\; n\log\!\left(1+\frac{g_n}{p_n}\right) \;<\; \frac{n\,g_n}{p_n}.
$$

#### C2 — The threshold

Define
$$
\boxed{\;T_n := \frac{p_n}{n}\,\log p_n\;}
$$

**(SUF) Sufficient condition.** If $g_n \le T_n$ then $S_n > 0$ (F holds at $n$).
*Proof.* $n\log(1+g_n/p_n) < n g_n/p_n \le \log p_n$. ∎ [L0]

**(REF) Refutation condition.** If $g_n \ge \dfrac{p_{n+1}}{n}\log p_n = T_n\!\left(1+\dfrac{g_n}{p_n}\right)$
then $S_n < 0$ (F **fails** at $n$).
*Proof.* $n\log(1+g_n/p_n) > n g_n/p_{n+1} \ge \log p_n$. ∎ [L0]

#### C3 — Sharpness of the sandwich [L0]

The two thresholds differ by exactly the factor $1 + g_n/p_n$.

**Why no assumption on $g_n$ is needed here** (a skeptic's first question). The
sharpness claim only has to hold *near the threshold*: if $g_n \gg T_n$ then (REF)
already fires and F is refuted outright, and if $g_n \ll T_n$ then (SUF) already fires
and F holds at $n$. The only regime where the two criteria could disagree is
$g_n \asymp T_n \asymp (\log p_n)^2$, and there the discrepancy factor is
$1 + O\!\left((\log p)^2/p\right)$ — **relative error $\approx 5\times10^{-16}$ at
$p \sim 4\times10^{18}$** [COMPUTED: $(\log 4\!\cdot\!10^{18})^2/(4\!\cdot\!10^{18}) \approx 4.6\times10^{-16}$].

So the criterion "$g_n$ versus $T_n$" is not a heuristic approximation: it is *the*
criterion, sharp to sixteen decimal places at the frontier, unconditionally.

This is the precise sense in which §0.2's headline holds.

#### C4 — Translating $T_n$ into observable quantities [L2 + COMPUTED]

We need $p_n/n$. Standard explicit bounds [L2, Rosser–Schoenfeld / Dusart; exact
constants and validity ranges are **OB-C-debt** for `source-ledger`]:
$$
n(\log n + \log\log n - 1) < p_n < n(\log n + \log\log n) \quad (n \ge 6).
$$
Hence $p_n/n = \log p_n - c_n$ with $c_n \in (\approx 0.9, \approx 1.2)$ across the
computed range. Numerically [COMPUTED, §5.4]:

| $n$ | $p_n$ | $p_n/n$ | $\log p_n - 1$ |
|---|---|---|---|
| $10^2$ | 541 | 5.410 | 5.293 |
| $10^3$ | 7919 | 7.919 | 7.977 |
| $10^5$ | 1 299 709 | 12.997 | 13.078 |
| $10^6$ | 15 485 863 | 15.486 | 15.555 |
| $3\,001\,133$ | 49 999 921 | 16.660 | 16.728 |

So
$$
T_n \;\approx\; (\log p_n)^2 - \log p_n
$$
and the criterion reads, in the standard prime-gap vocabulary:

$$
\textbf{F holds at } n \iff M_n \lesssim \log p_n - 1
\iff R_n \lesssim 1 - \frac{1}{\log p_n}.
$$

**Cross-check against the literature.** [L2] Kourbatov's stated consequence —
Firoozbakht $\Rightarrow g_n < (\log p_n)^2 - \log p_n - 1$ for $n \ge 10$ — matches this
derivation to the constant. The agreement is a genuine independent confirmation of C2–C4,
*not* a citation: we derived it here (L0) and the literature value agrees. **Note that
Kourbatov's is the converse direction** (F ⟹ gap bound); our (SUF) is the forward
direction (gap bound ⟹ F). Both directions hold, which is exactly what C3 asserts.

### 2.4 OB-D — the proof branch, and where it dies

| Obligation | Needed strength | Best known | Verdict |
|---|---|---|---|
| **D1** unconditional | $g_n < (1-o(1))(\log p_n)^2$, all $n$ | $g_n \ll p_n^{0.525}$ [L2, Baker–Harman–Pintz 2001] | **Not remotely sufficient.** $p^{0.525}$ vs. $(\log p)^2$ is a super-exponential shortfall. |
| **D2** under RH | same | $g_n \ll \sqrt{p_n}\,\log p_n$ [L2, Cramér 1920/21] | **Not sufficient.** Still power-of-$p$ vs. poly-log. |
| **D3** under GRH / Elliott–Halberstam / DHL sieve | same | these bound *small* gaps (bounded gaps, Zhang/Maynard/Polymath) | **Wrong direction.** These are lower-density / small-gap tools; F is an upper-gap statement. |
| **D4** under Cramér's conjecture | $\limsup R_n \le 1$ | Cramér's own model predicts $\limsup R_n = 1$ [L2] | **Insufficient even if granted**: $\limsup R_n = 1$ does not give $R_n < 1 - 1/\log p_n$ for every $n$; and Granville's refinement predicts $\limsup R_n \ge 2e^{-\gamma} \approx 1.1229 > 1$ [L2/L3], which would *refute* F. |

**The barrier, stated precisely.** Proving F requires an upper bound on prime gaps of
*polylogarithmic* strength with an *explicit constant strictly below 1*, valid for
**every** $n$ with no exceptional set. No technique in analytic number theory —
zero-density estimates, sieve methods, zero-free regions, RH, GRH — produces a
polylogarithmic gap bound at all. The distance is not quantitative; it is categorical.

> **Design instruction for the `proof-attempt` legs.** Do **not** budget attempts at
> closing D1–D4. Budget them at: (i) making the barrier statement itself rigorous and
> quotable; (ii) proving the *conditional* implications ("if $g_n \le T_n$ for $n>N_0$
> then F") in Lean, which *is* achievable and *is* a real artifact; (iii) mapping
> exactly which weakening of F *would* fall to current technology (§3.5).

### 2.5 OB-E — the refutation branch (the live branch)

To refute F it suffices to exhibit **one** $n$ with $S_n \le 0$.

| Obligation | Content | Cost |
|---|---|---|
| **E1** | A gap $g$ following a prime $p$ with $g \ge \frac{p+g}{n}\log p$ | the search |
| **E2** | Certificate that $p$ and $p+g$ are consecutive primes (all of $p+1..p+g-1$ composite, and $p, p+g$ prime) | routine, linear in $g$ |
| **E3** | A **lower bound** on $n = \pi(p)$ | **cheap — see below** |
| **E4** | Rigorous (interval-arithmetic or exact-rational) evaluation of $S_n < 0$ | routine |

#### E3 is much cheaper than it looks — a genuine structural observation [L0]

$S_n = \log p_n - n\log(1+g_n/p_n)$ is **strictly decreasing in $n$** for fixed $(p,g)$,
because $\log(1+g/p) > 0$. Therefore:

> To **refute**, one needs only a *lower* bound $n \ge n_0$ with
> $n_0\log(1+g/p) \ge \log p$. To **verify** F at a given $n$, one needs only an
> *upper* bound on $n$.

Both are supplied by **explicit Chebyshev-type bounds on $\pi(x)$** (Rosser–Schoenfeld,
Dusart) [L2] — which are cheap closed-form evaluations. **No exact prime counting
(Meissel–Lehmer / Lagarias–Miller–Odlyzko) is required.** A refutation therefore does
not need to know $n$; it needs only that $n$ is *large enough*, and explicit $\pi(x)$
lower bounds settle that at essentially zero cost.

This collapses what naive treatments list as the expensive obligation. **It should be
carried into the `lean-skeleton` and `notebooks` legs as a design constraint.**

#### E5 — the theoretical refutation route

If one proves $\limsup_n R_n > 1$, F falls. The Granville refinement of the Cramér model
predicts $\limsup R_n \ge 2e^{-\gamma} \approx 1.1229$ [L2/L3, heuristic]. This is
*believed* by a substantial part of the community, and it is the reason F is widely
expected to be **false**. But proving *any* nontrivial lower bound on $\limsup R_n$ is
itself far beyond current technology (the best unconditional large-gap results,
Ford–Green–Konyagin–Maynard–Tao and Ford–Green–Konyagin–Tao [L2], give gaps of order
$\frac{\log p \log\log p \log\log\log\log p}{\log\log\log p}$ — *smaller* than
$(\log p)^2$, hence useless here).

**Net.** E5 is not closeable either. The live obligation is **E1–E4: search**.

---

## 3. Candidate strategies

Rated by *expected artifact value*, not by chance of closing the conjecture (which is
~0 for all of them).

### 3.1 Direct proof — via gap bounds  ✗ blocked

Route: (SUF) + an unconditional bound $g_n \le T_n$. **Blocked at D1** (§2.4). Value of
attempting: **low**, except to produce the barrier statement.

### 3.2 Refutation by counterexample search  ◐ live, but out of reach

Route: E1–E4. **What the numbers actually say** [COMPUTED + L2]:

| Quantity | Value | Source |
|---|---|---|
| Largest verified range | $p_n < 4\times 10^{18}$ | [L2] Kourbatov 2015 |
| Record CSG ratio $R = g/(\log p)^2$ | $0.9206$, at $g = 1132$ after $p = 1\,693\,182\,318\,746\,371$ | [L2/L3] Nyman 1999, maximal-gap tables |
| $R$ needed to refute *there* ($\log p \approx 35.07$) | $1 - 1/35.07 \approx 0.9715$ | [L0] §2.3-C4 |
| **Multiplicative shortfall at the record locus** | **$1.055\times$** | [COMPUTED] |
| Largest known maximal gap | $g = 1550$ after $p \approx 1.836\times10^{19}$ | [L2/L3] |
| Its $R$ / shortfall | $0.7878$ / $1.241\times$ | [COMPUTED] |
| Max $R$ in this leg's own sieve ($p < 5\times10^7$) | $0.7395$ ($g=210$ after 20 831 323) | [COMPUTED] |
| Shortfall in that range | $1.27$–$1.56\times$ | [COMPUTED] |

**Important correction to the naive reading.** The shortfall is **not monotone in
height**. The closest approach on record sits at $p \approx 1.7\times10^{15}$
(shortfall $1.055$); the *largest* known gap, four orders of magnitude higher, is
markedly *further* from refuting (shortfall $1.241$), because $R$'s required threshold
$1 - 1/\log p$ creeps up while $R$ itself fluctuates. So one cannot say "just search
higher and the shortfall shrinks". Whether $\max_{p\le x} R$ actually trends toward
$1$ is precisely the open empirical question — and it is why §7 debt #10
(*measure $\max R$ against height, with error bars*) is a real deliverable rather than
a formality. The heuristic expectation that $\limsup R_n \ge 2e^{-\gamma} > 1$ [L2/L3]
is what makes refutation plausible; the data alone does not yet show a trend.

**Honest cost estimate.** Maximal-gap tables to $4\times10^{18}$ represent decades of
distributed compute [L3]. Extending far enough to catch an $R$ above its local threshold
$1 - 1/\log p$ is not a project this polymer can execute. **The `notebooks` legs should therefore target *measurement*, not
*search*: characterize the growth of $\max R$ with height and extrapolate honestly, with
error bars.**

### 3.3 Contradiction / contrapositive  ✗ degenerate

Assume $\exists n: S_n \le 0$ and derive a contradiction. By C3 this is *identical* to
proving $g_n < T_n$ for all $n$ — the contrapositive adds no leverage because the
statement is already a universally quantified inequality with no auxiliary structure to
exploit. **Value: none.** Record this so no `proof-attempt` leg burns compute here.

### 3.4 Construction (build a counterexample rather than find one)  ◐ interesting, unproven

Route: exhibit, by CRT / Erdős–Rankin sieving, an admissible interval of length
$> (\log p)^2$ guaranteed free of primes. **Obstruction**: this is the *same* obstruction
as §2.5-E5, from the other side. The strongest constructive large-gap results
(Erdős–Rankin, as improved by Ford–Green–Konyagin–Maynard–Tao and Ford–Green–Konyagin–Tao
[L2]) produce gaps of order
$$\frac{\log p\,\log\log p\,\log\log\log\log p}{\log\log\log p},$$
which is $o\!\left((\log p)^2\right)$ — it falls *below* the Firoozbakht threshold, and by a
factor that grows without bound. **No construction can reach the threshold with current
technique.**

**Value: medium**, and specifically *not* as a route to refutation. Worth exactly one
`proof-attempt` leg to compute the shortfall factor of the best constructive bound
against $T_n$ at concrete heights ($10^{18}$, $10^{100}$, $10^{1000}$). That number is
the honest "distance from *provable* constructions to refutation", it is a different
quantity from the empirical shortfall of §3.2, and it does not appear anywhere in the
literature in this normalization. [The bound's exact form is **[L2]** and is
`source-ledger` debt #5b.]

### 3.5 Weakening — prove what *is* provable  ✓ **highest expected value**

Nothing in the tree closes F. But several genuinely provable statements sit inside it and
are worth formalizing:

| W-tag | Statement | Provable? |
|---|---|---|
| **W1** | F1 ⟺ F2 ⟺ F3 ⟺ F4 ⟺ F5, all five, in Lean, `sorry`-free | **Yes**, today |
| **W2** | (SUF): $\forall n,\ g_n \le T_n \Rightarrow$ F at $n$ | **Yes**, today (L0 above) |
| **W3** | (REF): $g_n \ge \frac{p_{n+1}}{n}\log p_n \Rightarrow \lnot$F at $n$ | **Yes**, today |
| **W4** | F $\Rightarrow g_n < (\log p_n)^2 - \log p_n - 1$ for $n \ge 10$ | Yes, modulo explicit $p_n/n$ bounds (OB-C-debt) |
| **W5** | F for all $n \le N_0$, for a modest $N_0$ ($10^6$–$10^8$), machine-checked | **Yes**, with effort — and this is a *new* artifact if the certificate is Lean-checked rather than cited |
| **W6** | Monotone reduction (E3): $S_n$ strictly decreasing in $n$ ⟹ refutation needs only $\pi(x)$ *lower* bounds | **Yes**, today |

**W1 + W2 + W3 + W6 formalized in Lean, plus W5 to a defensible $N_0$, is a real,
honest, shippable deliverable.** It is what this polymer should aim to produce. It is
*not* a proof of Firoozbakht, and the `write-paper` and `editorial-verdict` legs must
be told, in these words, not to let it drift into sounding like one.

### 3.6 Strategy ranking (input to `frame-deliberation`)

| Rank | Strategy | Closes F? | Artifact value | Budget |
|---|---|---|---|---|
| 1 | §3.5 Weakening (W1,W2,W3,W6 in Lean) | No | **High** | Most of the formal branch |
| 2 | §3.5 W5 finite verification, Lean-checked | No | **High** | `lean-probe` |
| 3 | §3.2 Measurement of $\max R$ vs. height | No | Medium-high | `notebooks` |
| 4 | §3.4 Construction distance-to-threshold | No | Medium | 1 `proof-attempt` |
| 5 | §3.1 / §2.4 Barrier statement, made rigorous | No | Medium | 1 `proof-attempt` |
| 6 | §3.3 Contradiction | No | **None** | **Zero — do not spend** |

---

## 4. Falsifiability tests

These are the tests whose failure **refutes** the conjecture (FT-1..FT-4), plus tests
that refute *the decomposition itself* (FT-5..FT-7) — the latter matter because a wrong
decomposition silently corrupts every downstream leg.

### Tests that would refute the conjecture

**FT-1 — Direct slack test.** For each $n$: compute $S_n$ (or, exactly,
compare $p_{n+1}^n$ with $p_n^{n+1}$ in exact integer arithmetic). **Any $n$ with
$S_n \le 0$ refutes F.**
*Status*: executed here for $n \le 3\,001\,133$ ($p < 5\times 10^7$) — **no counterexample**
[COMPUTED]. Literature: none below $4\times10^{18}$ [L2].
*Teeth*: maximal. This is a decision procedure, not a heuristic.

**FT-2 — Threshold watch (cheap proxy).** Flag any prime gap with
$R_n = g_n/(\log p_n)^2 \ge 1 - 1/\log p_n$. By (REF) this is (up to the $1+g/p$ factor)
a refutation. Running this against published maximal-gap tables is $O(\text{table size})$
and is the single cheapest possible refutation attempt.
*Status*: max observed $R = 0.9206$ (at $p = 1\,693\,182\,318\,746\,371$) [L2/L3] vs.
required $0.9715$ there [COMPUTED]. **Passes — by 5.5%.**
*Teeth*: maximal, and this is where a refutation would actually first appear.

**FT-3 — Structural refutation.** A proof that $\limsup_n R_n > 1$ refutes F.
*Status*: open; heuristically predicted to be true ($2e^{-\gamma} \approx 1.1229$)
[L2/L3]. **This is the reason to expect F is false.**
*Teeth*: maximal if achieved; unreachable with current technology.

**FT-4 — Small-$n$ edge cases.** The exponent $n$ is load-bearing at small $n$, where
the inequality is *tightest*. [COMPUTED]:

| $n$ | $p_n$ | $p_{n+1}$ | $p_{n+1}^n$ | $p_n^{n+1}$ | ratio | $S_n/\log p_n$ |
|---|---|---|---|---|---|---|
| 1 | 2 | 3 | 3 | 4 | 0.750 | 0.415 |
| **2** | **3** | **5** | **25** | **27** | **0.926** | **0.0701** |
| 3 | 5 | 7 | 343 | 625 | 0.549 | 0.373 |
| **4** | **7** | **11** | **14 641** | **16 807** | **0.871** | **0.0709** |
| 5 | 11 | 13 | 371 293 | 1 771 561 | 0.210 | 0.652 |
| 8 | 19 | 23 | — | — | 0.243 | 0.481 |

**Finding [COMPUTED].** The conjecture's *tightest* points in the whole range
$n \le 3\times10^6$ are $n = 2$ and $n = 4$, with normalized slack $\approx 0.070$. The
tightest point for $n > 10$ is $n = 1\,319\,945$ ($p = 20\,831\,323$, $g = 210$), with
normalized slack $0.2104$ — **three times looser than $n=2$.** Any Lean formalization or
numeric pipeline that "handles small $n$ separately" must not silently assume the small
cases are slack; **they are the tightest cases known.** This is a real trap and is
recorded as obligation **OB-F1/OB-B2**.

### Tests that would refute *this decomposition*

**FT-5 — Sandwich sharpness.** If C3's claimed relative error $1 + g_n/p_n$ is wrong,
§0.2's headline collapses. *Test*: for a sample of $n$, compare the exact predicate
$p_{n+1}^n < p_n^{n+1}$ against the proxy $g_n \lessgtr T_n$ and confirm they agree except
within the predicted $O((\log p)^2/p)$ band. **Assigned to `notebooks`.**

**FT-6 — Literature cross-check.** Our derived threshold $T_n \approx (\log p_n)^2 - \log p_n$
must reproduce Kourbatov's stated $(\log p_n)^2 - \log p_n - 1$ for $n \ge 10$. It does
(§2.3-C4). If `source-ledger` finds the literature statement differs materially, C2–C4
are wrong and must be re-derived. **Assigned to `source-ledger`.**

**FT-7 — Bogus-proof smell test** (for the `skeptic` and `red-team-corpus` legs).
By §0.2, **any correct proof of F implies $g_n < (1-o(1))(\log p_n)^2$ for all $n$ with
explicit constants.** Therefore: *a candidate proof of F that does not, somewhere,
establish a polylogarithmic prime-gap upper bound is wrong.* This is a complete,
cheap, structural filter — it does not require reading the candidate proof's details.
**Every proof attempt in this polymer must be run through FT-7 before any other review.**

---

## 5. Empirical ground truth produced by this leg

### 5.1 Verification performed here  [COMPUTED]

- Sieved all primes to $5\times10^7$ (3 001 134 primes).
- Checked F5 ($S_n > 0$) for all $n \le 3\,001\,133$. **No counterexample.**
- This is a *sanity floor*, ~11 orders of magnitude below the published frontier
  [L2, $4\times10^{18}$]. It is deliberately independent of the literature: it confirms
  the *statement being attacked is the right one*, and it caught the small-$n$ tightness
  finding (FT-4) which the literature framing obscures.

### 5.2 Record-gap / shortfall table  [COMPUTED]

Maximal gaps below $5\times10^7$ with the refutation shortfall factor $T_n/g_n$:

| $n$ | $p_n$ | $g_n$ | merit $M_n$ | $R_n$ | $T_n$ (needed $g$) | shortfall $T_n/g_n$ |
|---|---|---|---|---|---|---|
| 40 933 | 492 113 | 114 | 8.70 | 0.664 | 157.6 | 1.38× |
| 103 520 | 1 349 533 | 118 | 8.36 | 0.592 | 184.0 | **1.56×** |
| 104 071 | 1 357 201 | 132 | 9.35 | 0.662 | 184.2 | 1.40× |
| 149 689 | 2 010 733 | 148 | 10.20 | 0.703 | 195.0 | 1.32× |
| 325 852 | 4 652 353 | 154 | 10.03 | 0.653 | 219.2 | 1.42× |
| 1 094 421 | 17 051 707 | 180 | 10.81 | 0.649 | 259.4 | 1.44× |
| **1 319 945** | **20 831 323** | **210** | **12.46** | **0.740** | **266.0** | **1.27×** |
| 2 850 174 | 47 326 693 | 220 | 12.45 | 0.704 | 293.5 | 1.33× |

### 5.3 What this leg did *not* establish

- No independent verification of the $4\times10^{18}$ range (out of scope, out of budget).
- No independent verification of the record CSG ratio 0.9206 — **[L2/L3], recalled, not
  checked.** This number, *and its locus* $p = 1\,693\,182\,318\,746\,371$, are load-bearing for §0.2's
  "5.5%" claim (the required threshold $1-1/\log p$ depends on the locus, not just on $R$)
  and are the
  **single highest-priority item for `source-ledger`.** If it is wrong, the headline's
  quantitative punch changes (its qualitative direction does not).
- No verification of the attributions: Firoozbakht (1982), Kourbatov (2015),
  Baker–Harman–Pintz (2001), Granville ($2e^{-\gamma}$), Cramér (1936), Ford–Green–
  Konyagin–Maynard–Tao (2016). All **[L2]**. All are `source-ledger` debts.
- The chain "Firoozbakht ⟸ Nicholson ⟸ Farhadian" (successively stronger conjectures)
  is **[L3, recalled, unverified]** and is deliberately *not* used anywhere above.

### 5.4 Reproducibility and numerical rigour

The computation is a plain sieve plus a loop over $S_n = \log p_n - n\log(p_{n+1}/p_n)$.
A self-contained, dependency-free script is emitted alongside this document as
`verify_small_range.py`; it runs in ~5 s for $N = 5\times10^7$ and reproduces every
`[COMPUTED]` number above.

**Why the float verdict is rigorous here.** The exact integer predicate
$p_{n+1}^n < p_n^{n+1}$ is evaluated only for $n \le 200$ (beyond that both sides have
millions of digits, at no epistemic gain). For $n > 200$ the verdict rests on $S_n$ in
IEEE-754 double. That is sound by a wide, *checked* margin:

- `math.log` is accurate to ~1 ulp, and neither term in $S_n$ grows with $n$
  ($n\log(1+g_n/p_n) = \log p_n - S_n \le \log p_n \le 18$ over this range), so the
  absolute error in $S_n$ is bounded by $\approx 10^{-13}$.
- The observed minimum of $S_n$ is $0.0770$ (at $n = 2$, itself inside the exact-check
  region); restricted to $n > 200$ it exceeds $3.5$.

That is ~12 orders of magnitude of headroom. The script encodes this as an explicit
`SAFETY_MARGIN` and exits non-zero if any $S_n$ ever comes within it, so the guarantee
degrades loudly rather than silently. **`notebooks` legs extending this range must carry
the same guard** — a float slack check without a margin assertion is not evidence.

**Incidental consistency check (partial FT-5).** The minimizer of the normalized slack
over $n > 10$ and the maximizer of $R_n$ over $p > 1000$ are the *same* index
($n = 1\,319\,945$). That is what §2.3-C3 predicts: over a range where $p_n/n$ varies
slowly, "smallest slack" and "largest CSG ratio" are the same event.

---

## 6. Lean 4 / Mathlib backend plan

### 6.1 Statement (F2 form — the recommended anchor)

```lean
import Mathlib

/-- `nthPrime n` is the `n`-th prime, **1-indexed**: `nthPrime 1 = 2`.
    Mathlib's `Nat.nth Nat.Prime` is 0-indexed, hence the shift. -/
noncomputable def nthPrime (n : ℕ) : ℕ := Nat.nth Nat.Prime (n - 1)

/-- **Firoozbakht's conjecture** (integer form).
    Equivalent to `p (n+1) ^ (1/(n+1)) < p n ^ (1/n)`; see `firoozbakht_iff_rpow`. -/
theorem firoozbakht :
    ∀ n : ℕ, 1 ≤ n → (nthPrime (n + 1)) ^ n < (nthPrime n) ^ (n + 1) := by
  sorry
```

**Two caveats on this snippet, both real traps:**

1. `n - 1` is *truncated* ℕ subtraction, so `nthPrime 0 = nthPrime 1 = 2`. This is
   harmless under the `1 ≤ n` hypothesis but is exactly the kind of silent aliasing that
   makes a mis-stated theorem compile. **OB-F1 must prove `nthPrime 1 = 2` and
   `nthPrime 2 = 3` as explicit `example`s**, not assume them.
2. `Nat.nth` is `noncomputable`, hence the `noncomputable def` — and hence `decide`
   cannot evaluate `nthPrime`. This is precisely why OB-F8 (finite verification) is the
   hard part: it needs a bridge from `Nat.nth Nat.Prime` to an explicit, computable
   sorted prime list plus a `Nat.count` certificate.

### 6.2 Why F2 and not F1

| | F1 (`rpow`) | F2 (`npow` on ℕ) |
|---|---|---|
| Side conditions | `0 < x`, `0 ≤ x`, `Real.rpow` monotonicity, `NNReal` coercions | none |
| Small-$n$ discharge | needs numeric `rpow` reasoning | `decide` / `norm_num` |
| Statement fidelity | direct | needs bridge lemma (OB-F3, §6.3) |

F2 is strictly easier and loses nothing, since the bridge is a one-off lemma.

### 6.3 The obligations, as Lean targets

| Lean obligation | Target | Difficulty |
|---|---|---|
| **OB-F1** | Index convention: `nthPrime 1 = 2`, `nthPrime 2 = 3`, and `nthPrime n < nthPrime (n+1)` | low — `Nat.nth_count`, `Nat.nth_lt_nth` (names to be pinned) |
| **OB-F2** | `firoozbakht` statement compiles, `sorry`-free modulo the main `sorry` | low |
| **OB-F3** | `firoozbakht_iff_rpow` : F2 ⟺ F1 (bridge; A1 of §2.1) | medium |
| **OB-F4** | `firoozbakht_iff_log`, `firoozbakht_iff_slack` (A2–A4) | low |
| **OB-F5** | Sandwich lemma C1: pin the Mathlib names for `x/(1+x) < log(1+x) < x` | low; **names unverified — must be checked against the pinned toolchain, not recalled** |
| **OB-F6** | (SUF) W2 and (REF) W3 as theorems | medium |
| **OB-F7** | (E3) W6: `S` strictly antitone in `n` | low |
| **OB-F8** | W5: finite verification to $N_0$, machine-checked | **high** — the real engineering: `Nat.nth` is noncomputable, so a verified prime-list bridge (`Nat.Prime` decidability + an explicit sorted list + a `Nat.count` certificate) is needed. This is the main risk item for `lean-skeleton`. |

### 6.4 Toolchain

`elan` present; toolchains `leanprover/lean4:v4.29.0` and `v4.29.1` installed; a Mathlib
`.ltar` cache exists at `~/.cache/mathlib`. **No Lean project exists in this galaxy yet** —
`lean-skeleton` must `lake new` and pin a Mathlib revision compatible with v4.29.x.

> ⚠️ **Do not trust the Mathlib lemma names quoted above.** They are recalled, not
> checked (**[L3]**). `lean-skeleton`'s first action is to compile a probe file that
> resolves every name, and to report the corrections. Named-lemma drift is the most
> common silent failure in an LLM-authored Lean skeleton.

### 6.5 The LLM firewall, restated for this conjecture

The polymer's invariant is that no target is called *proved* on an LLM's say-so. For this
conjecture the invariant has a sharp corollary: **`firoozbakht` itself will not be
closed.** The kernel's verdict will be `sorry`-free only on W1–W3, W6, and (with effort)
W5. Any downstream leg reporting otherwise has either changed the statement (index shift
— see OB-F1) or is hallucinating. **`evidence-gate` should treat "the main theorem
compiles `sorry`-free" as a *red flag requiring statement re-audit*, not as success.**

---

## 7. Open debts handed downstream

| # | Debt | Owner leg | Priority |
|---|---|---|---|
| 1 | Verify record CSG ratio $0.9206$ **and its locus** $p = 1\,693\,182\,318\,746\,371$, $g=1132$ | `source-ledger` | **Critical** (both are load-bearing for §0.2) |
| 2 | Verify Kourbatov 2015: verification range + the $(\log p)^2-\log p-1$ statement | `source-ledger` | **Critical** |
| 3 | Verify Granville's $2e^{-\gamma}$ heuristic (statement, not truth) | `source-ledger` | High |
| 4 | Pin explicit Dusart/Rosser–Schoenfeld $p_n$ and $\pi(x)$ bounds with validity ranges | `source-ledger` | High (needed for C4, W4, E3) |
| 5 | Verify Baker–Harman–Pintz exponent 0.525; and Cramér 1920/21 for the RH gap bound | `source-ledger` | Medium |
| 5b | Verify the Erdős–Rankin / FGKMT large-gap lower bound's exact form (used in §2.5-E5 and §3.4) | `source-ledger` | Medium |
| 6 | Confirm/kill the Nicholson/Farhadian chain [L3] | `source-ledger` | Low (unused) |
| 7 | Resolve all Mathlib lemma names by compilation | `lean-skeleton` | **Critical** |
| 8 | Decide $N_0$ for W5 and design the verified-prime-list bridge | `lean-skeleton` | **Critical** (OB-F8 is the main risk) |
| 9 | FT-5 sandwich-sharpness experiment | `notebooks` | Medium |
| 10 | Measure growth of $\max R$ vs. height; honest extrapolation with error bars | `notebooks` | High (this is §3.2's real deliverable) |
| 11 | Enforce FT-7 as the first filter on every proof attempt | `skeptic`, `red-team-corpus` | **Critical** |
| 12 | Guard against "proved!" drift in the paper | `editorial-verdict` | **Critical** |

---

## 8. Verdict of this leg

**The conjecture is open, and is neither proved nor refuted here.**

The decomposition establishes three things at **L0** (proved in this document):

1. **The five formulations F1–F5 are pointwise equivalent**, and F2 (pure natural-number
   exponentiation) is the right formal anchor.
2. **F is equivalent, to relative precision $1+O((\log p)^2/p)$, to the prime-gap bound
   $g_n < \frac{p_n}{n}\log p_n \approx (\log p_n)^2 - \log p_n$.** This is not an
   approximation — it is sharp at the frontier to sixteen digits.
3. **Refutation requires only an explicit *lower* bound on $\pi(p_n)$, not exact prime
   counting**, because $S_n$ is strictly decreasing in $n$.

It establishes two things at **[COMPUTED]**:

4. No counterexample for $n \le 3\,001\,133$ (independent of the literature).
5. Over the computed range $n \le 3\,001\,133$, the conjecture's tightest points are
   $n = 2$ and $n = 4$ — *not* the large-gap region (normalized slack $0.070$ there,
   versus $0.210$ at the tightest large-$n$ point). Whether some $n$ beyond this range
   is tighter is exactly the open question; what is established is that any pipeline
   special-casing small $n$ as "obviously slack" is wrong.

And it draws one **strategic** conclusion:

6. **Proof is categorically blocked** (needs a polylogarithmic gap bound with constant
   $< 1$; nothing in analytic number theory produces polylogarithmic gap bounds at all).
   **Refutation is quantitatively close but computationally out of reach** (a factor
   $1.055$ in a ratio whose record is a decades-long distributed-compute artifact, and
   whose approach to $1$ is *not* observed to be monotone in height — see §3.2).
   The community's heuristic expectation is that **F is false**. This polymer should
   therefore aim at the honest deliverable of §3.5 — W1, W2, W3, W6 formalized
   `sorry`-free in Lean, W5 to a defensible $N_0$, plus a measured extrapolation of
   $\max R$ — and must **not** present that as progress toward a proof.

---

*Emitted by the `decompose` leg. Downstream: `frame-deliberation` (stress-test §2, §3, §4),
then `source-ledger` (close the §7 debts), then `concept-cards`.*
