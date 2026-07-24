# Findings — computational notebook 1, target #1 `RH-conditional-bound`

**Leg**: `notebooks__1` (node 6 of the `math-attack` polymer)
**Artifact**: `notebook-1-rh-conditional-bound.py` (executable), `notebook-1.out` (captured run)
**Date**: 2026-07-24

---

## 0. Posture

Firoozbakht's conjecture is **open**. Nothing in this leg proves or refutes it.
The object under attack here is not the conjecture but a **route** to it:

> RH ⟹ [an explicit prime-gap upper bound] ⟹ Firoozbakht.

`CC-23` already states, qualitatively, that this route does not reach — "a power
of $p$ against a power of $\log p$". `CC-23`'s own acceptance criterion for the
barrier node **OB-G** demands more than that:

> *"each surveyed route's shortfall is a **computed number** at a **named
> height**"*

For the RH row that number does not exist anywhere in this corpus, and does not
appear in the literature in this normalisation. **This leg computes it.**

---

## 1. The headline, in one line

> **The strongest explicit RH-conditional prime-gap bound in the literature
> proves Firoozbakht's conjecture for the primes 2, 3 and 5 — and for no other
> prime below $5\times10^7$.**
>
> Inside that theorem's own stated validity range ($x \ge 4$) it proves the
> conjecture at the single prime $p = 5$.

The bound is Carneiro–Milinovich–Soundararajan 2019, **Theorem 5** (V0, read
verbatim by this leg — see §6):

$$\text{RH} \implies \text{there is a prime in } \Big[x,\; x + \tfrac{22}{25}\sqrt{x}\log x\Big] \quad (x \ge 4).$$

Against the **exact** Firoozbakht barrier $f_n = p_n\!\left(p_n^{1/n}-1\right)$
(**CC-07** — no approximation, no external $\pi(x)$ input), define the

$$\textbf{RH deficit} \qquad D(n) \;:=\; \frac{\tfrac{22}{25}\sqrt{p_n}\,\log p_n}{f_n}.$$

$D(n) < 1$ means "RH alone settles Firoozbakht at $n$".

| $n$ | $p_n$ | $D(n)$ | verdict |
|---|---|---|---|
| 1 | 2 | **0.431314** | clears (outside CMS validity $x\ge4$) |
| 2 | 3 | **0.762474** | clears (outside CMS validity $x\ge4$) |
| 3 | 5 | **0.892130** | **clears, inside validity** |
| 4 | 7 | 1.032957 | silent — and the minimum over all $n\ge4$ |
| … | … | … | silent, throughout the scan (§1 table below) |

$D$ is not monotone at small $n$ (it dips at $n=9\to10$, $30\to31$, …), so the
notebook does not assert monotonicity. It reports **decade-window minima**, which
are strictly increasing across all eight decades scanned —

| window | $\min D$ | at $p_n$ | | window | $\min D$ | at $p_n$ |
|---|---|---|---|---|---|---|
| $[10^0,10^1)$ | 0.4313 | 2 | | $[10^4,10^5)$ | 10.7797 | 10 007 |
| $[10^1,10^2)$ | 1.0339 | 11 | | $[10^5,10^6)$ | 26.6790 | 100 003 |
| $[10^2,10^3)$ | 2.0806 | 101 | | $[10^6,10^7)$ | 69.0728 | 1 000 033 |
| $[10^3,10^4)$ | 4.5868 | 1 009 | | $[10^7,10^8)$ | 184.9369 | 10 000 079 |

— and pairs that with the asymptotic
$D(x) \sim \frac{22}{25}\sqrt{x}/(\log x - 1) \to \infty$, which is flagged as an
asymptotic argument, not as scan evidence.

---

## 2. The deficit at named heights — OB-G criterion (ii), discharged for the RH row

Barrier evaluated with **exact** $\pi(x)$ inside the sieve, and above it with the
RH-conditional Schoenfeld bracket taking the **larger** end of the resulting
$f$-interval — RH is given every advantage available to it.

| height $x$ | $\log x$ | barrier $f$ | CMS bound | **deficit $D$** | note |
|---|---|---|---|---|---|
| $10^{12}$ | 27.63 | 734.73 | $2.43\times10^{7}$ | $3.31\times10^{4}$ | |
| $1.693\times10^{15}$ | 35.07 | 1193.42 | $1.270\times10^{9}$ | $\mathbf{1.064\times10^{6}}$ | **record locus** |
| $10^{18}$ | 41.45 | 1675.29 | $3.647\times10^{10}$ | $2.18\times10^{7}$ | ~Kourbatov frontier |
| $10^{20}$ | 46.05 | 2073.64 | $4.053\times10^{11}$ | $1.95\times10^{8}$ | PrimeGapList frontier |
| $10^{26}$ | 59.87 | 3523.16 | $5.268\times10^{14}$ | $1.50\times10^{11}$ | Johnston uncond. ceiling |
| $10^{100}$ | 230.26 | 52787.7 | $2.03\times10^{52}$ | $3.84\times10^{47}$ | |

**The number to quote**: at the record locus the best RH-conditional bound
permits gaps **$1.06\times10^{6}$ times** the barrier that Firoozbakht requires.

The whole family, at the record locus ($f_k = 1193.418$, $g_k = 1132$):

| RH-conditional bound | value at $p_k$ | deficit vs $f_k$ |
|---|---|---|
| Dudek–Grenié–Molteni 2016 Thm 1.1, $c = 1+4/\log x$ | $1.607\times10^{9}$ | $1.347\times10^{6}$ |
| Dudek 2015 Thm 1.3, $c = 1$ | $1.443\times10^{9}$ | $1.209\times10^{6}$ |
| **CMS 2019 Thm 5, $c = 22/25$** | $1.270\times10^{9}$ | $\mathbf{1.064\times10^{6}}$ |
| CMS 2019 Cor 4, $c < 21/25$ | $1.212\times10^{9}$ | $1.016\times10^{6}$ |
| *limit of the CMS method*, $c = 1/2$ | $7.214\times10^{8}$ | $6.045\times10^{5}$ |
| *(unconditional BHP, $x^{0.525}$, for scale)* | $9.887\times10^{7}$ | $8.285\times10^{4}$ |

---

## 3. Why improving the constant cannot help — the exponent gap

The five rows above differ by a factor of 2.2. The deficit is $10^6$. Constant-chasing
is not the missing ingredient; **the exponent is**. Invert the question: what
exponent would suffice?

$$x^{\theta} \le (\log x)^2 \iff \theta \le \theta_{\text{req}}(x) := \frac{2\log\log x}{\log x}.$$

$\theta_{\text{req}}$ is **not a constant — it decays to zero**, while every
available exponent is pinned at $1/2$ (RH) or $0.525$ (BHP).

| height $x$ | $\theta_{\text{req}}$ | RH gives | BHP gives | RH excess |
|---|---|---|---|---|
| $10^{3}$ | 0.5596 | 0.500 | 0.525 | 0.89 |
| $10^{4}$ | 0.4821 | 0.500 | 0.525 | 1.04 |
| $10^{18}$ | 0.1797 | 0.500 | 0.525 | 2.78 |
| $10^{100}$ | 0.0472 | 0.500 | 0.525 | 10.58 |
| $10^{1000}$ | 0.0067 | 0.500 | 0.525 | 74.36 |

> **$\theta_{\text{req}}$ falls below RH's $1/2$ at $x = 5503.66$** ($\log x = 8.613$).
> Above a *four-digit* height, an RH-strength **exponent** is already too weak,
> independently of any constant anyone might ever prove.

**Corollary that closes the constant-chasing branch.** CMS record the limit of
their own method as $c = 1/2$ (V0), and record that under **RH + Montgomery's
pair correlation conjecture** the relevant $\limsup$ is $0$ (V0) — i.e. the
strongest conditional statement in circulation in this family gives
$g_n = o(\sqrt{p_n}\log p_n)$. That hypothesis does not *entail* $g_n = O((\log p_n)^2)$ — it is strictly weaker
than what the barrier needs. **No constant in this family, the limiting value
$1/2$ and the hypothetical $0$ included, yields a bound strong enough to reach
the barrier.**

---

## 4. What RH *does* buy — and the two limits on it

This is the honest positive side, and it is smaller than it first looks.

The corpus's one external dependency is $c_n := \log p_n - p_n/n$ (**CC-06**,
**CC-20**, debt #4), which converts the exact barrier into the literature's
$\ell_n = (\log p)^2 - \log p$ vocabulary. Under RH, Schoenfeld's Corollary 1
brackets $\pi(x)$, which inverts to a bracket on $c_n$.

At the record locus:

| route | bracket on $c_k$ | width |
|---|---|---|
| Dusart three-term (unconditional, **CC-20**) | $(1.030154,\ 1.033325)$ | $3.171\times10^{-3}$ |
| **Schoenfeld / RH** | $(1.03127838,\ 1.03135693)$ | $7.855\times10^{-5}$ |
| true value | $1.03131702621$ | — |

**RH tightens the bracket by a factor 40.4.** Now the two limits.

**Limit 1 — RH is the *worse* route below $10^{11.19}$.** Schoenfeld's error is a
$\sqrt{x}$ object; Dusart's is a $(\log x)^{-3}$ object. They cross:

| height | $\varepsilon$ via Dusart | $\varepsilon$ via RH | RH tighter by |
|---|---|---|---|
| $10^{8}$ | 0.0584 | 0.8133 | **0.072** (RH worse) |
| $10^{11}$ | 0.0803 | 0.0952 | **0.844** (RH worse) |
| $10^{12}$ | 0.0876 | 0.0430 | 2.04 |
| $10^{18}$ | 0.1314 | $2.233\times10^{-4}$ | 588 |
| $10^{26}$ | 0.1898 | $9.878\times10^{-8}$ | $1.92\times10^{6}$ |

> **Crossover: $x^{*} = 10^{11.1947}$.** Below it, invoking RH makes the threshold
> estimate strictly *worse* than doing without it. This corrects a claim the
> notebook's own first draft made ("uniformly tighter") — the check was written
> to be able to fail, and it failed.

**Limit 2 — the advantage is operationally void.** Converting both brackets into
the quantity the corpus actually reports, the record-locus shortfall:

| route | shortfall $f_k/g_k$ | width |
|---|---|---|
| via Dusart (unconditional) | $1.05419379 \dots 1.05429201$ | $9.82\times10^{-5}$ |
| via RH (Schoenfeld) | $1.05425475 \dots 1.05425719$ | $2.43\times10^{-6}$ |
| **via the exact barrier $f_k$ (CC-07)** | $\mathbf{1.05425598789}$ | **0** |

The exact barrier needs no $\pi(x)$ bound, no Dusart, no Axler and no RH. **The
one quantity RH demonstrably improves in this problem is a quantity the corpus
already computes exactly by a route that bypasses it.**

**Limit 3 — "assuming RH" is free where computation lives.** Johnston 2022 (V0)
proves the Schoenfeld bracket holds **unconditionally** for
$2657 \le x \le 1.101\times10^{26}$, from the partial verification of RH to
height $3\times10^{12}$. So for **this ingredient**, in the entire regime any
`notebooks` leg can reach, adopting RH buys nothing that is not already
unconditional. (The scope matters: CMS Theorem 5, the *gap* bound of §§1–2, is
**not** free in that range — only the $\pi(x)$ bracket of this section is.)

---

## 5. Two structural findings for downstream legs

### 5.1 RH is neutral toward *refutation* — OB-E6 is empty for RH, and why

**CC-24** flags that OB-D has three conditional slots while OB-E has zero, "with
no reason given". This leg supplies the reason:

- every RH-conditional result in the surveyed set is an **upper** bound on gaps;
- **no known theorem derives from RH** either $\limsup R_n \le 1$ (Cramér) or
  $\limsup R_n > 1$ (Granville's revision); RH separates neither, so it cannot
  decide the question that OB-D4/OB-E5 (the same question, twice named) turns on.
  This is a statement about the surveyed literature, **not** a proof that RH is
  independent of either;
- the only lower-bound technology is unconditional and falls short.

In the corpus's own $R$ coordinate ($R_n = g_n/(\log p_n)^2$, **CC-05**),
Firoozbakht needs $R_n < 1 - 1/\log p_n$. RH's ceiling on $R$ is
$\frac{22}{25}\sqrt{x}/\log x$, which **diverges**:

| height | Firoozbakht threshold $1 - 1/\log x$ | RH ceiling on $R$ | orders of slack |
|---|---|---|---|
| $10^{12}$ | 0.963809 | $3.185\times10^{4}$ | 4.52 |
| $1.693\times10^{15}$ | 0.971482 | $1.033\times10^{6}$ | 6.03 |
| $10^{18}$ | 0.975873 | $2.123\times10^{7}$ | **7.34** |
| $10^{26}$ | 0.983296 | $1.470\times10^{11}$ | 11.17 |
| $10^{100}$ | 0.995657 | $3.822\times10^{47}$ | 47.58 |

At the Kourbatov frontier RH is **seven orders of magnitude** from constraining
the quantity the conjecture is about, and the gap widens without bound.

### 5.2 The window, with numbers on both edges

**CC-24**'s symmetry — "current technique brackets the target from neither side" —
made numeric. Lower edge: Ford–Green–Konyagin–Maynard–Tao 2018 (implied constant taken as 1 for
illustration; the true constant is unknown, so the row is an order-of-magnitude
sketch, not a bound). Upper edge: CMS Theorem 5.

| height $x$ | lower: FGKMT | **barrier $f$** | upper: RH/CMS | window spans |
|---|---|---|---|---|
| $10^{18}$ | 32.1 | **1675** | $3.65\times10^{10}$ | $1.13\times10^{9}$ |
| $10^{26}$ | 59.6 | **3523** | $5.27\times10^{14}$ | $8.84\times10^{12}$ |
| $10^{100}$ | 390 | **52788** | $2.03\times10^{52}$ | $5.20\times10^{49}$ |
| $10^{1000}$ | 6238 | **$5.30\times10^{6}$** | $2.03\times10^{503}$ | $3.25\times10^{499}$ |

The barrier sits strictly **inside** the window at every height, and the window
widens without bound.

---

## 6. Sources — and a side deliverable

This leg **fetched and text-extracted four primary PDFs**, yielding **three new
citekeys at V0** (`cms2019fourier`, `johnston2022improving`, `leenosal2025sharper`
— five V0 locator rows between them), none of which the `source-ledger` leg
carried, plus a locator correction on `schoenfeld1976sharper`. Verbatim
extractions are reproduced here so `citation-gate` can audit the locators.

| citekey | V | locator | statement (verbatim where quoted) |
|---|---|---|---|
| `cms2019fourier` | **V0** | arXiv:1708.04122v2, **Theorem 5** | *"Assume the Riemann hypothesis. Then, for $x \ge 4$, there is always a prime number in the interval $[x, x+\frac{22}{25}\sqrt{x}\log x]$."* |
| `cms2019fourier` | **V0** | same, **Corollary 4** | $\limsup (p_{n+1}-p_n)/(\sqrt{p_n}\log p_n) \le 1/C^+(B) < 21/25$ |
| `cms2019fourier` | **V0** | same, remark after Cor. 4 | *"the limit of this method would yield a constant $\frac12$"*; and under RH + pair correlation *"the limit supremum in (1.14) is actually zero"* |
| `dudek2015rh` | V2 | Int. J. Number Theory 11 (2015), Thm 1.3 | $c = 1$ — quoted verbatim by `cms2019fourier` p. 4 |
| `dgm2016short` | V2 | Int. J. Number Theory 12 (2016), Thm 1.1 | $c(x) = 1 + 4/\log x$ — quoted verbatim by `cms2019fourier` p. 5 |
| `cramer1920some` | V2 | cited as [14] by `cms2019fourier` eq. (1.10) | RH ⟹ $g_n = O(\sqrt{p_n}\log p_n)$ |
| `schoenfeld1976sharper` | V2 | Math. Comp. **30** (1976) 337–360, **Corollary 1** | $\|\pi(x)-\mathrm{li}(x)\| < \sqrt{x}\log x/(8\pi)$ for $x \ge 2657$ — quoted verbatim by `johnston2022improving` eq. (1.1) |
| `johnston2022improving` | **V0** | arXiv:2109.02249v2, abstract + eq. (1.2) | *"we get that $\|\pi(x) − \mathrm{li}(x)\| < \sqrt{x}\log x/8\pi$ for $2657 \le x \le 1.101\cdot10^{26}$"* |
| `leenosal2025sharper` | **V0** | arXiv:2312.05628v4, **Theorem 1.2** | RH ⟹ $\|\psi(x)-x\| \le \sqrt{x}\log x(\log x - \log\log x)/(8\pi)$, $x \ge 101$ |

> ⚠️ **A locator correction for `citation-gate`.** The Schoenfeld $\pi(x)$ bound is
> widely mis-cited as *"Theorem 10"*. Johnston 2022, which quotes it verbatim,
> attributes it to **`[Sch76, Corollary 1]`**. Any downstream leg writing
> "Theorem 10" is citing a locator this leg could not confirm.

---

## 7. What this leg establishes, and what it does not

**Established (by computation).**

- Firoozbakht holds for every $n \le 3\,001\,133$ ($p_n \le 49\,999\,921$), settled by
  **integer arithmetic only**: 500 indices by exact big-integer F2, 3 000 633 by
  the CC-28 integer criterion, 0 criterion-nominations needing escalation. This
  is a **sanity floor for the machinery**, nine orders of
  magnitude inside the literature's verified range (Kourbatov $4\times10^{18}$;
  Visser $2^{64}$). It is **not** evidence about the conjecture.
- Every number quoted in §§1–5, at the precision quoted, printed by the notebook.

**Refuted — as a route, over a finite surveyed set.**

- The route [RH ⟹ explicit gap bound ⟹ Firoozbakht] closes at exactly three
  indices and is silent everywhere above $p = 5$.
- Constant improvement cannot rescue it (§3), including the limiting constants
  $1/2$ and $0$.
- RH cannot supply a conditional *refutation* either (§5.1).

**Explicitly NOT established.**

1. **Firoozbakht's conjecture is neither proved nor refuted here.** It is open.
2. **"RH does not imply Firoozbakht" is NOT shown.** RH could imply it through a
   route that never passes through a gap bound of the surveyed shape. The object
   refuted is a **route**, over a **finite surveyed set**. This is exactly
   `CC-23`'s *defensible version*, not its `[L3]` universal negative — and this
   leg does **not** promote that `[L3]` clause.
3. **A scan is not a proof.** "No crossover in $[a,b]$" means: evaluated at every
   prime in that range. Statements beyond the scanned range (§1's
   *"silent above $p=5$"* read as unbounded, §2's *"grows without bound"*, §3's decay) rest on the flagged asymptotic arguments, not on the scan.
4. The FGKMT lower bound in §5.2 uses implied constant 1; the true constant is
   unknown. The row is an order-of-magnitude illustration, not a bound.

---

## 8. Numerical discipline

Inherited from `frame-deliberation/outcomes.md#A1` and knuth's finding on
`decompose/verify_small_range.py#5.4`:

- the exact predicate is settled by **integer** arithmetic, never by a float
  comparison;
- slack uses `math.log1p`, never `log(q/p)`, so the error does not grow with $n$;
- the float guard band is **computed at runtime** ($nu + 6u\log p$), never
  hardcoded — the run reports min slack $1.7008$ (at $n = 217$) against a band of
  $3.332\times10^{-10}$;
- the bulk $D$-scan runs in **doubles** (justified: $p_n < 2^{53}$, $D$ is $O(1)$,
  and the tightest index clears the $D=1$ boundary by $0.033$), with every index
  landing within $10^{-6}$ of the boundary **re-adjudicated in 60-digit mpmath**.
  The escalation count is printed: **0** at the full sieve limit;
- $f_n$ is evaluated as `p*expm1(log(p)/n)`, never `p*(p**(1/n)-1)` (**CC-07** trap);
- everything above $p > 2^{53}$ goes through `mpmath` at 60 digits;
- the notebook **prints every extremal statistic** the prose quotes. No number in
  this note is absent from `notebook-1.out`;
- **`p.bit_length() - 1`**, not `p.bit_length()`, in the CC-28 criterion. The `-1`
  is load-bearing: without it the criterion over-estimates $\log_2 p$ and becomes
  unsound in the direction that matters.

**External validation of the arithmetic.** The notebook reproduces two published
numbers at the record locus from the exact integers alone, without using them as
input: $f_k = 1193.41777829404$ against Kourbatov Table 1's $1193.418$, and
$\ell_k = 1194.51592191172$ against Table 1's $1194.516$. Both are V0 rows. The
overshoot $\ell_k - f_k = 1.098$ gap units matches **CC-07** exactly.

**Two checks failed on first run and both were real findings, not bugs** — the
$\{n=1,2,3\}$ clearing set (§1) and the $10^{11.19}$ Dusart/Schoenfeld crossover
(§4). Both corrections are recorded in the notebook at the site of the check.

---

## 9. Recommendations to downstream legs

1. **`proof-attempt` on target #1**: do **not** budget an attempt to close the RH
   route. §3 shows the obstruction is the exponent, and the required exponent
   decays to zero. Budget instead the **barrier statement** (OB-G): §§1–3 supply
   criterion (ii) — a computed shortfall at named heights — for the RH row.
2. **`write-paper`**: §1's one-line headline is the quotable form. Carry §7's
   clauses 2 and 3 *in the same paragraph*; the sentence is only true as a
   statement about a surveyed route.
3. **`source-ledger` / `citation-gate`**: four rows are promoted to V0 here (§6);
   the Schoenfeld locator is **Corollary 1**, not Theorem 10.
4. **`skeptic` / `red-team-corpus`**: the two self-corrections in §8 are the
   places to attack first — they are where this leg's first draft was wrong.
5. **OB-E6 (conditional refutation)** can be closed as **empty for RH**, with
   §5.1 as the reason. Whether some *other* hypothesis fills it is untouched here.
