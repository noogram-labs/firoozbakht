# Proof attempt 1 — target `RH-conditional-bound`

**Leg**: `proof-attempt__1` (target #1 of the `math-attack` polymer)
**Upstream consumed**: `concept-cards/` (CC-07, CC-10, CC-18, CC-20, CC-23, CC-24, CC-27, CC-31), `source-ledger/source-ledger.md`, `decompose/decompose.md` §2.4-D2
**Formal backend**: Lean 4 / Mathlib (statements proposed in §9; **nothing compiled by this leg**)
**Date**: 2026-07-24

> **Posture.** Firoozbakht's conjecture
> $$p_{n+1}^{1/(n+1)} < p_n^{1/n}\quad(n\ge1)$$
> is **open**. This leg does not prove it, does not refute it, and produces no
> evidence about its truth. Everything below compares *two upper bounds on prime
> gaps with each other*; no statement about an actual gap is asserted beyond what
> a V0 ledger row already carries.
>
> Tiering follows **CC-31**: `[L0]` proved here, `[L1]` standard, `[L2]` attributed
> and locator-verified in §8, `[L3]` heuristic/recalled, `[C]` recomputed by
> `verify_rh.py` in this directory. A composite claim inherits its **weakest**
> premise's tier.

---

## 0. Verdict, stated first

The target is **`RH-conditional-bound`**: settle whether the Riemann hypothesis
closes Firoozbakht's conjecture — prove it or refute it, do not assume it.

**Verdict: the target is REFUTED as a route, and the refutation is quantitative,
not rhetorical.** Three theorems are proved here.

| | Result | Tier |
|---|---|---|
| **Thm 1** | The strongest *published* RH-conditional gap bound (Carneiro–Milinovich–Soundararajan 2019, $c=22/25$) decides Firoozbakht at **exactly three indices** $n\in\{1,2,3\}$ — and at **exactly one**, $n=3$, once the source's own range restriction $p_n>3$ is honoured. It fails at **every** $n\ge4$, forever. | `[L0]` + `[C]` |
| **Thm 2** | This is not a constant problem. For **every** $c>0$, the bound $g_n\le c\sqrt{p_n}\log p_n$ decides Firoozbakht on a **finite** initial segment only. Improving the constant moves a threshold; it never changes the verdict. | `[L0]` + `[C]` |
| **Thm 3** | The RH branch is **strictly dominated**, not merely weak. For every $c \ge 10^{-7}$ — seven orders below the published constant, and six below $c=1/2$, which the CMS authors themselves name as **the limit of their method** — the usable set ends *inside* the range already verified *unconditionally* ($p<2^{64}$, `visser2019verifying` V0). OB-D2 contributes **zero indices** that OB-B does not already own. | `[L0]` + `[C]` |

**Prop 4** explains *why* the $\sqrt{x}$ shape is not laziness: it is a property of
the prime-counting error itself. Littlewood's unconditional $\Omega_\pm$ theorem
forces any argument that bounds $|\theta(t)-t|$ pointwise and subtracts to work at
scale $\sqrt{x}\log\log\log x$ or coarser — **regardless of RH**.

**What is *not* proved** (§7, obstruction **OBS-1**): that RH does *not* imply
Firoozbakht. That is a claim about RH's deductive closure, and nothing here — or in
the literature — establishes it. The barrier proved is about the *shape of the
bounds the known RH machinery emits*, over an explicitly enumerated surveyed set.
Per **CC-23**'s discipline, the universal negative is not asserted.

**One incidental positive** (§6): RH *does* have a use in this attack — on the
**refutation** branch, certifying the index $n=\pi(p)$ (OB-E3). Quantified there:
it buys a $29\times$ more comfortable certificate than the unconditional bound
(on a **V3** row, gap **G8**), and the unconditional bound was already comfortable
by a factor 1600 on **V0** rows. RH is on the wrong branch of the tree.

**Upstream delta** (§8): the ledger's RH row is `cramer1920some` (V2, **1920**,
no explicit constant). The state of the art is **99 years newer** — `cms2019fourier`
(V0, 2019) gives an explicit constant $22/25$ and an explicit range.
`decompose.md#2.4-D2` and **CC-23** should carry it. It does not change either
document's conclusion; it makes it computable.

---

## 1. Setup: the decision predicate, and what "closing the target" would mean

### 1.1 The exact barrier and the exact criterion

From **CC-07** / **CC-10** (`visser2019verifying` eq. (2.4), **V0**), with
$g_n = p_{n+1}-p_n$ and $L_n = \log p_n$:

$$
\textbf{F holds at } n \iff g_n < f_n := p_n^{\,1+1/n}-p_n = p_n\big(e^{L_n/n}-1\big).
\tag{1.1}
$$

No side condition, no asymptotic regime, no exceptional small-$n$ set. This is the
whole content of the conjecture, index by index.

### 1.2 The usable set — the object this leg measures

Let $U=(U_n)_{n\ge1}$ be any family of upper bounds valid under a hypothesis $H$
(i.e. $H \Rightarrow g_n \le U_n$ for all $n$ in its range).

> **Definition (usable set).** $\;\mathcal{U}(U) := \{\,n \ge 1 : U_n < f_n\,\}$.

**Lemma 1.1 `[L0]`.** If $n\in\mathcal{U}(U)$ then $H$ implies F at $n$.
*Proof.* $g_n \le U_n < f_n$, and apply (1.1). $\square$

$\mathcal{U}$ is the honest measure of what a conditional bound *buys*. "$H$ closes
Firoozbakht" means $\mathcal{U}(U) \supseteq \{n : n > N_0\}$ for some $N_0$ whose
complement is finitely checkable — i.e. $\mathcal{U}$ must be **cofinite**. The
whole of §§3–5 computes $\mathcal{U}$ for the RH family and finds it **finite**;
finite and cofinite are the two possible answers, and it is the wrong one.

> **Why this framing and not "compare the asymptotics".** A leg that only compares
> $\sqrt{p}\log p$ with $\log^2 p$ and concludes "one is bigger" has proved nothing
> about *where*. $\mathcal{U}$ is a set of integers; it can be computed, it can be
> exhibited, and its emptiness at a given constant is falsifiable by naming an index.

---

## 2. The sourced input: what RH is actually known to give

All rows verified by this leg; V-levels and locators in §8.

| Source | V | Statement |
|---|---|---|
| `cramer1920some` (via `granville1995harald`) | V2 | On RH, $p_{n+1}-p_n = O(\sqrt{p_n}\log p_n)$ — the classical shape, no explicit constant. |
| `dudek2015riemann` **Thm 1.3 / abstract** | V1 | On RH, there is a prime in $\big(x-\tfrac4\pi\sqrt{x}\log x,\;x\big]$ for all $x\ge2$; the constant reduces to $1+\varepsilon$ for large $x$. This is $c=1$ in the limsup normalisation. |
| `cms2019fourier` **Cor. 4**, eq. (1.14) | **V0** | On RH, $\displaystyle\limsup_n \frac{p_{n+1}-p_n}{\sqrt{p_n}\log p_n} \le \frac1{C^+(B)} < \frac{21}{25}$. |
| `cms2019fourier` **Thm 5** + the display preceding it | **V0** | Thm 5: on RH, for $x\ge4$ there is always a prime in $\big[x,\,x+\tfrac{22}{25}\sqrt{x}\log x\big]$. The unnumbered display immediately preceding it states the gap form the authors derive: $p_{n+1}-p_n \le \tfrac{22}{25}\sqrt{p_n}\log p_n$ **for all primes $p_n>3$**. **This is the strongest published explicit RH-conditional gap bound.** |
| `cms2019fourier`, remark after (1.14) | **V0** | *"We note from (1.12) and Theorem 2 (b) that **the limit of this method would yield a constant $\tfrac12$** on the right-hand side of (1.14)."* — the authors' own ceiling for the explicit-formula + Brun–Titchmarsh + Fourier-optimisation route. |
| `cms2019fourier`, same remark | **V0** | Under **RH *and* Montgomery's pair-correlation conjecture**, the limsup in (1.14) is **zero**. |

So the RH-conditional family is
$$
U^{(c)}_n := c\,\sqrt{p_n}\,\log p_n,
\qquad c = \tfrac{22}{25}=0.88 \ \text{(published)},\quad
c = \tfrac12 \ \text{(the method's stated ceiling)}.
\tag{2.1}
$$

The barrier it must clear is $f_n$, which by `kourbatov2015upper` **Thm 5** (V0,
**CC-18**) satisfies $f_n = \log^2p_n - \log p_n - 1 + o(1)$. **A power of $p$
against a power of $\log p$** — the qualitative point already in **CC-23**. §§3–5
turn it into integers.

---

## 3. Theorem 1 — the published RH bound decides three indices

> **Theorem 1 `[L0]` + `[C]`.** With $c = 22/25$,
> $$\mathcal{U}\big(U^{(c)}\big) = \{1,\,2,\,3\}, \qquad\text{i.e. } p_n \in \{2,3,5\}.$$
> Restricted to the range in which `cms2019fourier` states the bound ($p_n>3$),
> $\mathcal{U} = \{3\}$. For every $n \ge 4$, $\;f_n < U^{(c)}_n$.

Consequence, via Lemma 1.1: **the total contribution of the Riemann hypothesis to
Firoozbakht's conjecture, through the strongest published explicit route, is the
single index $n=3$ — the assertion $5^{1/3} < 3^{1/2}$, which is checkable by
cubing and squaring ($125 < 243$).**

### 3.1 Proof, part 1 — the finite range $n \le N_0 = 688\,383$

Direct evaluation of both sides of $U^{(c)}_n < f_n$ for every $n \le 688\,383$
(all primes to $1.1\times10^7$; $p_{N_0} = 10\,384\,261$). The predicate holds at
$n=1,2,3$ and fails at every $4 \le n \le N_0$. `[C]` CHECK 1a, 1b, 1c, 2e-v.

**Floating-point discipline (CC-27).** The scan runs in double precision with a
fail-safe: whenever the two sides agree to within a relative $10^{-9}$, the
decision is re-taken in 60-digit `mpmath`. **The guard fired 0 times** across all
$3.6\times10^{6}$ comparisons (726 517 indices $\times$ 5 constants) `[C]` — the two
families are never within a relative $10^{-9}$ of each other
anywhere in the scanned range, so no verdict in this document rests on a float
comparison. (This is the discipline `outcomes.md#A8` demanded and `feynman.md`
found violated upstream; here the guard is calibrated *above* the achievable error,
not below it, and its firing count is reported rather than assumed.)

$N_0 = 688\,383$ is not arbitrary: it is the validity threshold of
`dusart2010estimates` **Prop. 6.6** (V0), the only external input part 2 uses.
The finite part is sized to close exactly the range the sourced bound cannot.

### 3.2 Proof, part 2 — the tail $n \ge N_0$, analytically

Write $L = L_n = \log p_n$ and $x = L/n$.

**(a) Size of $p_n$.** `dusart2010estimates` **Prop. 6.6** (V0): for $n \ge N_0$,
$$p_n \le n\,D(n), \qquad D(n) := \log n + \log\log n - 1 + \frac{\log\log n - 2}{\log n}.$$

**(b) $D(n) \le 2\log n$ for $n \ge N_0$ `[L0]`.** Equivalent to
$\log\log n + \frac{\log\log n - 2}{\log n} \le \log n + 1$. Now $\log\log n \le \log n$
(since $\log t \le t$ for $t>0$) and $\frac{\log\log n-2}{\log n} \le \frac{\log\log n}{\log n}\le 1$.
Adding gives the claim. $\square$ Hence
$$p_n \le 2\,n\log n \qquad (n \ge N_0).\tag{3.1}$$
`[C]` CHECK 2e-ii evaluates both sides at $N_0$: $15.085 \le 26.884$.

**(c) $x \le 1$, and $x \le 3\log n/n$ `[L0]`.** By (3.1),
$L \le \log 2 + \log n + \log\log n \le 3\log n$, using $\log2 \le \log n$ and
$\log\log n \le \log n$ for $n \ge N_0$. So $x \le 3\log n/n$, which is decreasing
for $n \ge 3$ and equals $5.9\times10^{-5}$ at $N_0$; in particular $x \le 1$
throughout the tail.

**(d) Barrier upper bound `[L0]`.** For $0 \le x \le 1$,
$$e^x - 1 - x = \sum_{k\ge2}\frac{x^k}{k!} \le x^2\sum_{k\ge2}\frac1{k!} = (e-2)\,x^2 \le 0.72\,x^2 .$$
`[C]` CHECK 2e-i confirms $\sup_{(0,1]}(e^x-1-x)/x^2 = e-2 = 0.718282$. Therefore
$$f_n = p_n\big(e^{x}-1\big) \;\le\; p_n\,x\,(1+0.72\,x) \;=\; \frac{p_nL}{n}\Big(1+\frac{0.72\,L}{n}\Big).
\tag{3.2}$$

**(e) The comparison `[L0]`.** By (3.2), $f_n < c\sqrt{p_n}\,L$ is implied by
$$H(n) := \frac{\sqrt{p_n}}{n}\Big(1+\frac{0.72\,L}{n}\Big) \;<\; c .
\tag{3.3}$$
(Divide (3.2) by $\sqrt{p_n}L>0$.) So $H(n)<c$ certifies $n \notin \mathcal{U}$.

**(f) Envelope `[L0]`.** By (3.1) and (c),
$$H(n) \;\le\; \sqrt{\frac{2\log n}{n}}\,\Big(1+\frac{2.16\log n}{n}\Big) \;=:\; \widehat H(n).$$
$\widehat H$ is a product of two positive decreasing functions of $n$ for $n\ge3$
(because $\log n/n$ is decreasing there), hence decreasing. `[C]` CHECK 2e-iv
confirms the decrease across $n = 10^6 \ldots 10^{40}$; CHECK 2e-vi confirms
$H \le \widehat H$ at sampled $n$.

**(g) Conclusion.** $\widehat H(N_0) = 0.00624959 < 0.88 = c$ `[C]` CHECK 2e-iii —
a margin of $140\times$. By monotonicity, $H(n) \le \widehat H(n) \le \widehat H(N_0) < c$
for all $n \ge N_0$, so by (3.3) no $n \ge N_0$ is usable. With part 1, $\square$

### 3.3 What the proof does and does not use

- **Uses**: (1.1) `[L0]`, Dusart Prop. 6.6 `[V0]`, elementary calculus `[L0]`, and a
  finite computation `[C]`.
- **Does not use**: RH, any $\pi(x)$ inversion, `attack.md`'s unsourced $-3/L-13/L^2$
  expansion (ledger gap **G1** — deliberately avoided; nothing here depends on it),
  or Kourbatov's Thm 5 asymptotic. The asymptotic is used only for *narration* in §2
  and for the sharp crossover *estimates* in §4.2, which are labelled as estimates.
- **The theorem is unconditional.** It is a statement about two explicit functions of
  $n$. RH enters only through Lemma 1.1's hypothesis, i.e. only when one wants to
  read the (empty) conclusion.

---

## 4. Theorem 2 — the obstruction is the shape, not the constant

The natural objection to Theorem 1: *$22/25$ is just today's constant; wait for a
better one.* This section closes that door.

> **Theorem 2 `[L0]`.** For every $c>0$, $\mathcal{U}\big(U^{(c)}\big)$ is **finite**.
> Explicitly, $\mathcal{U}\big(U^{(c)}\big) \subseteq [1,\,n_c)$ where
> $n_c := \min\{n \ge N_0 : \widehat H(n) < c\}$, which is finite because
> $\widehat H(n) \le \sqrt{2\log n/n}\,(1+2.16\log n/n) \to 0$.

*Proof.* Verbatim §3.2 (b)–(g), with $c$ in place of $22/25$: steps (b)–(f) never
mention $c$, and (g) becomes "$\widehat H$ decreasing and $\to0$, hence eventually
$<c$". $\square$

### 4.1 The usable set at the constants that matter `[C]` CHECK 1

| $c$ | provenance | $|\mathcal{U}|$ | last usable index | last usable prime |
|---|---|---|---|---|
| $22/25 = 0.88$ | `cms2019fourier` Thm 5, **published** | **3** | $n=3$ | $p=5$ |
| $1/2$ | `cms2019fourier`: *"the limit of this method"* | **17** | $n=17$ | $p=59$ |
| $0.1$ | hypothetical, $8.8\times$ better than published | 769 | $n=769$ | $p=5851$ |
| $0.01$ | hypothetical, $88\times$ better | 133 145 | $n=133\,145$ | $p=1\,772\,591$ |
| $0.001$ | hypothetical, $880\times$ better | $>726\,517$ | beyond the sieve | $>1.1\times10^7$ |

In every case the usable set is an **initial segment** $\{1,\dots,n_c\}$ `[C]`
CHECK 1d — the RH bound is decisive exactly where the primes are small, and goes
silent permanently thereafter.

> **Read the second row again.** The authors of the strongest result in this
> literature state that their method's ceiling is $c=1/2$. At that ceiling, RH
> decides Firoozbakht up to $p=59$. Not $10^{59}$ — fifty-nine.

### 4.2 The crossover law `[C]` CHECK 2, tier `[L1]`-derived

Using $f_n = \log^2p_n-\log p_n-1+o(1)$ (`kourbatov2015upper` Thm 5, V0), the
crossover prime $p^*(c)$ solves $c\sqrt{p} = \log p - 1 - 1/\log p$, i.e.
$$\boxed{\;p^*(c) \;\approx\; \Big(\frac{\log p^*(c)-1}{c}\Big)^{2}\;}$$
— quadratic in $1/c$, with only a logarithmic correction. Bisected values `[C]`:

| $c$ | $10^{-1}$ | $10^{-2}$ | $10^{-3}$ | $10^{-6}$ |
|---|---|---|---|---|
| $p^*(c)$ | $5.66\times10^{3}$ | $1.77\times10^{6}$ | $3.46\times10^{8}$ | $1.13\times10^{15}$ |

These are *estimates* (they inherit Kourbatov's $o(1)$). The **rigorous** companion
is Theorem 2's envelope bound $n_c$, together with $p_{n_c} \le 2n_c\log n_c$ from
(3.1); it is an upper bound and sits a factor $3.57$ above the estimate at
$c=10^{-6}$ `[C]` CHECK 2f-v — same order, wrong side, which is the correct
direction for a bound. CHECK 2a confirms the sieved crossovers bracket the
estimates at $c = 10^{-1}, 10^{-2}$.

**The number that ends the discussion.** To make the RH-shape bound decisive out to
the range already verified unconditionally, $p<2^{64}$, one needs
$$c \;\lesssim\; \frac{\log(2^{64})-1-1/\log(2^{64})}{\sqrt{2^{64}}} \;=\; 1.00906\times10^{-8}$$

`[C]` CHECK 2c — again an *estimate*, carrying the same $o(1)$ caveat.
That is **$8.7\times10^{7}$ times smaller** than the published constant and
$5\times10^{7}$ times smaller than the method's stated ceiling. At the record CSG
locus ($p = 1.693\times10^{15}$) the requirement is already $c \lesssim 8.27\times10^{-7}$
`[C]` CHECK 2d. The rigorous version of the same statement — costing one order of
magnitude of sharpness, and still leaving six orders of margin — is Theorem 3.

---

## 5. Theorem 3 — the RH branch is strictly dominated

> **Theorem 3 `[L0]` + `[C]`.** Let $\mathcal{V}$ be the set of indices settled by
> unconditional verification, $\mathcal{V} \supseteq \{n : p_n < 2^{64}\}$
> (`visser2019verifying`, **V0**). Then for every $c \ge 10^{-7}$ —
> in particular for the published $c=22/25$ and for the method's ceiling $c=1/2$ —
> $$\mathcal{U}\big(U^{(c)}\big) \subsetneq \mathcal{V}.$$
> **OB-D2 contributes no index that OB-B does not already own.**

*Proof.* By Theorem 2, $\mathcal{U}(U^{(c)}) \subseteq [1, n_c)$ with
$n_c = \min\{n\ge N_0 : \widehat H(n)<c\}$, and by (3.1) every such index has
$p_n \le 2n_c\log n_c$. Evaluating `[C]` CHECK 2f: at $c=22/25$ and at $c=1/2$ the
envelope already closes at $n_c = N_0$, so $p_n \le 1.85\times10^{7}$; at
$c=10^{-7}$, $n_c \le 7.31\times10^{15}$ and $p_n \le 5.34\times10^{17}$. All three
are below $2^{64}=1.845\times10^{19}$. The inclusion is strict since $\mathcal{V}$
reaches $2^{64}$. $\square$

**The rigorous/sharp boundary, stated honestly.** The envelope is not tight enough
to carry the claim to $c = 1.009\times10^{-8}$: there $2n_c\log n_c = 6.81\times10^{19} > 2^{64}$
`[C]` CHECK 2f-iv. The sharp threshold of §4.2 remains an estimate; Theorem 3 is the
part that is proved. It costs one order of magnitude and still clears the published
constant by seven.

At the record locus the numbers are stark `[C]` CHECK 3:

| quantity at $p_k = 1\,693\,182\,318\,746\,371$ | value |
|---|---|
| exact barrier $f_k$ | $1193.418$ (reproduces `kourbatov2015upper` Table 1, V0) |
| observed gap $g_k$ | $1132$ |
| RH bound $U^{(22/25)}_k$ | $1\,269\,735\,204$ |
| RH bound $U^{(1/2)}_k$ | $721\,440\,457$ |
| **overshoot $U/f$** | $\mathbf{1.06\times10^{6}}$ (published) · $6.05\times10^{5}$ (ceiling) |

The RH-conditional bound is, at the single tightest known point of the conjecture,
**a million times too weak**. The observed gap is a factor $10^{6}$ *below* what RH
permits — RH is not remotely tight there, and the slack is not recoverable by
constants.

> **The strategic reading, in one line.** The proof branch does not narrowly miss.
> It misses by six orders of magnitude at the one place where the conjecture is
> within 5.4% of failing.

---

## 6. Where RH is *not* useless: the refutation branch (OB-E3)

Recorded because a leg that only reports a negative has not finished looking.

**CC-15 / decompose §2.5-E3**: refuting F at a candidate $(p,g)$ needs only a
**lower** bound on the index $n=\pi(p)$, since $S_n$ is decreasing in $n$. So RH's
sharpest classical payoff — Schoenfeld's explicit $\pi(x)$ error term — lands
*here*, not on the proof branch.

`[C]` CHECK 4, at the record locus:

| quantity | value | reading |
|---|---|---|
| index needed for (REF) to fire | $52\,448\,844\,419\,981$ | $5.4256\%$ above the true $k$ |
| **the margin available** | $\mathbf{5.4256\%}$ | this *is* the shortfall $f_k/g_k = 1.054256$ (§8 note) |
| Schoenfeld RH error $\sqrt{x}\log x/8\pi$ at $p_k$ `[L3]`, gap **G8** | $5.74\times10^{7}$ | $1.154\times10^{-6}$ of $k$ |
| unconditional Dusart 6.6/6.7 index bound | $49\,747\,979\,158\,586$ | $3.317\times10^{-5}$ of $k$ |

**Verdict.** RH's index certificate is $29\times$ tighter than the unconditional
one — a comparison resting on a **V3** row (gap **G8**) — and the unconditional one,
which rests on **V0** (Dusart Props. 6.6/6.7), is already **1600× inside** the margin
that matters. The load-bearing half of this table is the unconditional half.
RH is a comfort, never a load-bearing input. Any refutation search that claims to
*need* RH for index certification has mis-designed its certificate.

> **A structural coincidence worth recording `[C]`.** The relative index margin
> ($0.054255988$) and the gap shortfall $f_k/g_k - 1$ ($0.054255988$, `source-ledger`
> §5) agree to nine significant figures. They are the same first-order object —
> $\log p/\log(1+g/p) \approx (p/g)\log p$ and $f_k/g_k \approx T_k/g_k$ — seen from
> the index side and the gap side. A downstream leg that reports them as two
> independent confirmations would be double-counting one number.

---

## 7. The obstruction, stated precisely (OBS-1)

What Theorems 1–3 establish and what they do not, with no slack between the two.

**Proved.** For the family $U^{(c)}_n = c\sqrt{p_n}\log p_n$ — the shape that every
known RH-conditional argument emits — the usable set is finite for every $c>0$ and
is $\{1,2,3\}$ at the published constant.

**Not proved, and not provable by this method.** That RH does not imply
Firoozbakht. That is a statement about the deductive closure of RH, and it would
require either a limitation metatheorem (out of reach) or a proof of $\lnot$F under
RH (nobody has one, and by §5 the target sits above the verified range).

**Why the $\sqrt{x}$ shape is not an artefact of insufficient effort.** Two
independent barriers, one proved here, one sourced.

> **Proposition 4 (the $\Omega$-barrier) `[L0]` modulo a cited theorem.**
> Call an argument *Chebyshev-difference type* if it deduces "$(x-h,x]$ contains a
> prime" from a pointwise error bound $|\theta(t)-t| \le E(t)$ ($t \ge t_0$) via
> $\theta(x)-\theta(x-h) \ge h - E(x) - E(x-h) > 0$.
> Then necessarily $h(x) > E(x)+E(x-h)$, and by Littlewood's theorem
> $\theta(x)-x = \Omega_\pm\big(\sqrt{x}\,\log\log\log x\big)$ `[L3]`
> (`littlewood1914`, **V3**, gap **G7**), $E(x) \ge |\theta(x)-x|$ forces
> $$\limsup_{x\to\infty}\frac{h(x)}{\sqrt{x}\,\log\log\log x} \;>\; 0 .$$
> In particular **no argument of this type can produce $h(x) = O(\log^2 x)$**, and
> the shortfall against the Firoozbakht requirement grows like
> $\sqrt{x}\log\log\log x/\log^2x \to \infty$.
> *Proof.* Immediate from the two displayed inequalities. $\square$

Three things to note about Prop 4, none of them decoration:

1. **It is unconditional.** It never mentions RH. The $\sqrt{x}$ is a property of
   the *prime-counting error itself*, not of our ignorance about zeros. Assuming
   more about the zeros cannot repair it, because the obstruction is downstream of
   the zeros. This is the precise sense in which `decompose.md#2.4-D2`'s verdict
   ("still power-of-$p$ vs poly-log") is structural.
2. **It has a stated scope, and methods escape it.** Arguments exploiting
   cancellation over the interval — Goldston, Ramaré–Saouter, Dudek, and CMS's
   Fourier optimisation — do not bound the error pointwise and are outside the
   class. For *those*, the ceiling is the authors' own: **$c=1/2$**
   (`cms2019fourier`, **V0**), which §4.1 shows is decisive to $p=59$.
   The two arguments together cover the surveyed literature. **They do not cover
   all conceivable arguments, and this document does not claim they do**
   (**CC-23**'s surveyed-set discipline).
3. **The `ψ`-versus-`θ` variant is even cheaper.** Any argument routed through
   $\psi$ pays $\psi(x)-\theta(x) \ge \theta(\sqrt x) \sim \sqrt x$ `[L1]` before it
   starts — the prime-power term alone is of the forbidden size.

**And the strongest conditional statement in circulation still does not close it.**
Under RH *and* Montgomery's pair-correlation conjecture, the limsup in (1.14) is
**zero** (`cms2019fourier`, **V0**) — i.e. $g_n = o(\sqrt{p_n}\log p_n)$. Its usable
set is **empty**, for a reason worth stating plainly:

> **Remark 7.1 `[L0]`.** An ineffective $o(\cdot)$ bound has empty usable set by
> construction. $\mathcal{U}$ is defined by a pointwise inequality at a named $n$;
> a statement of the form "for every $c$ there exists an unspecified $N_c$" names no
> $n$ and therefore decides no index. To convert it one needs an effective rate, and
> by §4.2 the rate required is $c(n) \lesssim \log p_n/\sqrt{p_n}$ — which is
> algebraically identical to demanding $g_n \lesssim \log^2 p_n$, i.e. **Cramér
> strength**. Pair correlation removes the constant and leaves the shape; the
> requirement is unchanged.

---

## 8. Ledger deltas and citation obligations

### 8.1 New rows proposed for `source-ledger/source-ledger.md` §3.6

| citekey | Source | V | Locator | Statement supplied |
|---|---|---|---|---|
| `cms2019fourier` | E. Carneiro, M. B. Milinovich, K. Soundararajan, *Fourier optimization and prime gaps*, **Comment. Math. Helv. 94 (2019), no. 3, 533–568**; arXiv:1708.04122. | **V0** | **Thm 5** ¶; **Cor. 4** eq. (1.14) ¶; the remark following (1.14) ¶; the display preceding Thm 5 ¶ | Thm 5: on RH, for $x\ge4$ there is a prime in $[x, x+\tfrac{22}{25}\sqrt x\log x]$. Preceding display: $p_{n+1}-p_n \le \tfrac{22}{25}\sqrt{p_n}\log p_n$ **for all primes $p_n>3$**. Cor. 4: $\limsup \le 1/C^+(B) < 21/25$. Remark: *"the limit of this method would yield a constant $\tfrac12$"*; and under RH + Montgomery pair correlation the limsup is $0$. Retrieved from `arxiv.org/pdf/1708.04122`, text extracted, read verbatim. |
| `dudek2015riemann` | A. Dudek, *On the Riemann hypothesis and the difference between primes*, **Int. J. Number Theory 11 (2015)**; arXiv:1402.6417. | V1 | Abstract ¶ (arXiv metadata page) | On RH, a prime in $(x-\tfrac4\pi\sqrt x\log x, x]$ for all $x\ge2$, improving Ramaré–Saouter; the constant reduces to $1+\varepsilon$ for large $x$. **Superseded by `cms2019fourier` for the explicit constant** — cite for lineage. |
| `leenosal2025sharper` | E. S. Lee, P. Nosal, *Sharper bounds for the error in the prime number theorem assuming the Riemann Hypothesis*, arXiv:2312.05628v4 (2 Oct 2025). | **V0** | **eq. (1)**, second line ¶; **Thm 1.2** ¶ | Records Schoenfeld's classical RH bound $|\psi(x)-x|\le \sqrt x(\log x)^2/8\pi$ for $x\ge73.2$, and improves it to $\sqrt x\log x(\log x-\log\log x)/8\pi$ for $x\ge101$ ($\vartheta$: $x\ge2657$). Retrieved from `arxiv.org/pdf/2312.05628`, text extracted, read verbatim. |
| `schoenfeld1976sharper` | L. Schoenfeld, *Sharper bounds for the Chebyshev functions $\theta(x)$ and $\psi(x)$. II*, **Math. Comp. 30 (1976), 337–360**. | **V3** | Corollary 1 — **locator not verified** | Reported statement: on RH, $|\pi(x)-\mathrm{li}(x)| < \sqrt x\log x/(8\pi)$ for $x\ge2657$. **Primary not obtained** (AMS returned HTTP 403 to this leg) and no secondary was read *verbatim* by this leg at that locator — only a search summary asserted it. **V3, not V2**: bibliographic form is solid, the numbered locator is not. Used in §6 only, where the conclusion is unchanged if the row is dropped entirely. The $\psi$ companion ($|\psi(x)-x|\le\sqrt x(\log x)^2/8\pi$, $x\ge73.2$) **is** confirmed at V0 through `leenosal2025sharper` eq. (1). |
| `littlewood1914` | J. E. Littlewood, *Sur la distribution des nombres premiers*, **C. R. Acad. Sci. Paris 158 (1914), 1869–1872**. | **V3** | — | $\psi(x)-x = \Omega_\pm(\sqrt x\log\log\log x)$ (equivalently for $\theta$). **Metadata + statement from secondaries only.** Prop 4 rests on this row; a `citation-gate` pass must promote it or Prop 4 must be restated as conditional on it. |
| `montgomery1973pair` | H. L. Montgomery, *The pair correlation of zeros of the zeta function*, Proc. Sympos. Pure Math. 24 (1973). | V3 | via `cms2019fourier` refs [26,27,35] ¶ | Cited only for the "limsup $=0$" consequence, which is itself carried at V0 by `cms2019fourier`. |

### 8.2 Corrections owed upstream

| # | Where | Delta |
|---|---|---|
| **R1** | `decompose.md#2.4-D2`; **CC-23** row *"under RH (Cramér 1920)"* | The RH row is carried at V2 from a **1920** source with **no explicit constant**. The state of the art is `cms2019fourier` (V0, 2019): explicit constant $22/25$, explicit range $x\ge4$. Neither document's *conclusion* changes; both become computable. Replace the row. |
| **R2** | **CC-23**, column *"shortfall"*, RH row: *"still a power of $p$ against a power of $\log p$"* | Correct but unquantified. Replace with the computed content of §4.1/§5: usable set $\{1,2,3\}$; overshoot $1.06\times10^{6}$ at the record locus; $c$ needed at $2^{64}$ is $1.009\times10^{-8}$. |
| **R3** | **CC-23**, *"granting Cramér … still insufficient"* | Add the sharper sibling proved here (Remark 7.1): **granting RH + pair correlation gives $o(\sqrt p\log p)$ and still decides no index**, because an ineffective $o(\cdot)$ has empty usable set. This is a stronger and cheaper statement than the $\limsup$-versus-per-index argument, and it is V0-sourced. |
| **R4** | `decompose.md#3.6` strategy ranking | Rank 5 (*"barrier statement, made rigorous"*, budget: 1 `proof-attempt`) is **discharged for the RH sub-branch** by this document, against **CC-23**'s own acceptance criterion: (i) requirement stated with all four qualifiers ✓ §1.2; (ii) each surveyed route's shortfall a computed number at a named height ✓ §4.1, §4.2, §5; (iii) the universal negative restricted to the surveyed set, not asserted ✓ §7. |

### 8.3 Gaps this leg opens or leaves open

| id | Claim | Status |
|---|---|---|
| **G7** *(new)* | Littlewood's $\Omega_\pm$ result at V3 | Prop 4's only external input. Promote to V0/V2 or restate Prop 4 as *"conditional on the standard $\Omega$-result"*. **Not fatal**: Prop 4's role is explanatory; Theorems 1–3 do not use it. |
| **G8** *(new)* | `schoenfeld1976sharper` Cor. 1 at **V3** — locator unverified | Used only in §6, for a number that is *not* load-bearing: the **unconditional** Dusart bound already clears the margin by $1600\times$, so deleting the row changes no conclusion in this document. Promote or delete; do not let it into the paper as a V2. |
| **G1** *(inherited)* | `attack.md`'s $-3/L-13/L^2$ expansion | **Untouched and unused.** Deliberately avoided: §3's tail proof routes through Dusart Prop. 6.6 (V0) instead. No result here depends on G1. |

---

## 9. Handoff to the Lean leg

Per **CC-29**: every Mathlib name below is `[L3]` — *recalled, not resolved by
compilation*. **This leg compiled nothing.** These are statement proposals; the
`lean-probe` leg owns name resolution.

The formalisable content, in decreasing order of value-per-effort:

```lean
-- P1.  The usable-set predicate.  Pure real analysis; no prime theory.
--      `f n` is the exact barrier of CC-07 at the n-th prime.
def usable (c : ℝ) (p : ℕ → ℕ) (n : ℕ) : Prop :=
  c * Real.sqrt (p n) * Real.log (p n) < (p n : ℝ) ^ (1 + 1/(n:ℝ)) - (p n : ℝ)

-- P2.  Theorem 2, the shape barrier.  This is the whole point, and it needs
--      NO prime input beyond an upper bound p n ≤ 2 * n * Real.log n.
theorem usable_finite (c : ℝ) (hc : 0 < c) (p : ℕ → ℕ)
    (hp : ∀ n, 688383 ≤ n → (p n : ℝ) ≤ 2 * n * Real.log n) :
    {n | usable c p n}.Finite := by sorry

-- P3.  Lemma 1.1 — the bridge from a gap bound to Firoozbakht at one index.
--      Consumes CC-10, which the lean-skeleton leg already targets.
theorem firoozbakht_of_usable (c : ℝ) (n : ℕ) (hn : 1 ≤ n)
    (hgap : (nthPrime (n+1) : ℝ) - nthPrime n ≤ c * Real.sqrt (nthPrime n) * Real.log (nthPrime n))
    (hu : usable c nthPrime n) :
    (nthPrime (n+1) : ℝ) ^ (n : ℝ) < (nthPrime n : ℝ) ^ ((n:ℝ)+1) := by sorry
```

**Three warnings, all inherited from the deliberation legs and all live here.**

1. **Index convention (CC-01, OB-F1).** `nthPrime` above is a placeholder for the
   1-indexed sequence. Mathlib's `Nat.nth Nat.Prime` is **0-indexed**. §3's finite
   computation is 1-indexed; an off-by-one silently shifts $\mathcal{U}=\{1,2,3\}$.
2. **P2 is the honest deliverable, and it is prime-free.** It says: *any* bound of
   square-root shape decides only finitely many indices. It needs one crude prime
   input ($p_n \le 2n\log n$) and is otherwise real analysis — cheap in Lean, and it
   is the load-bearing theorem of this document. Formalising P2 without P3 is still
   a real artifact.
3. **Do not state P3 with `≤` on the conclusion** (`godel.md`'s silent-slip finding):
   a `≤` version degenerates to `1 ≤ 1` at $n=0$ and closes by `le_refl`, proving
   nothing. The conclusion must be strict, and the `1 ≤ n` guard must survive.

---

## 10. Self-audit

**Against FT-7′ (CC-30).** This document is not a candidate proof of F, so the
triage does not apply to it — but the mirror question does: *does it establish a
polylogarithmic gap bound anywhere?* No. It establishes that a family of
**square-root-scale** bounds cannot reach a polylogarithmic barrier. That is a
negative result about a bound family, and it is stated as such.

**Against CC-31 (tier propagation).** Every composite claim here inherits its
weakest premise:

| Claim | Premises | Tier |
|---|---|---|
| Thm 1, Thm 2, Thm 3 | (1.1) `[L0]`, Dusart 6.6 **V0**, calculus `[L0]`, computation `[C]` | **`[L0]`+`[C]`** — no RH, no $o(1)$, no V2/V3 row |
| "the published RH bound is $c=22/25$" | `cms2019fourier` **V0** | `[L2]`, locator-verified |
| "the method's ceiling is $c=1/2$" | `cms2019fourier` **V0** | `[L2]` — and it is the *authors'* claim about *their* method, not a theorem |
| §4.2 crossover estimates | Kourbatov Thm 5's $o(1)$ **V0** | `[L1]`-derived **estimate**, explicitly not `[L0]` |
| Prop 4 | Littlewood **V3** (gap G7) | `[L3]` — flagged, and no theorem depends on it |
| §6's "$29\times$" | Schoenfeld **V3** (gap G8) | `[L3]` — flagged; the section's verdict survives deleting the row |
| §7 "no route reaches the target" | surveyed set only | **restricted**, per CC-23; the universal negative is *not* asserted |

**Three ways this document could be wrong, named so a red-team leg can aim.**

1. **The finite computation is wrong.** Falsifiable by naming an $n \ge 4$ with
   $\tfrac{22}{25}\sqrt{p_n}\log p_n < f_n$. Guarded: 60-digit re-check on any
   near-tie, 0 promotions fired.
2. **The tail proof's envelope is wrong.** Falsifiable by exhibiting $n \ge N_0$ with
   $H(n) > \widehat H(n)$, or by finding that Dusart Prop. 6.6's validity range is
   not $k \ge 688\,383$. Both are one-line checks.
3. **The framing is wrong** — i.e. some RH-conditional argument produces a bound
   *not* of square-root shape. **This is the live risk, and it is the whole content
   of OBS-1.** Nothing here excludes it; §7 says so explicitly, twice.

**Reproducibility.** `python3 verify_rh.py` in this directory — **exit 0**, 34/34
checks pass, ~45 s, output in `verify_rh.out`. Exact-integer inputs; 60-digit
`mpmath` on every transcendental step or float-guarded with a reported firing count;
no RNG.

---

## 11. One-paragraph summary for `synthesize`

The Riemann hypothesis does not close Firoozbakht's conjecture, and the reason is
now a number rather than an impression. The strongest published RH-conditional
prime-gap bound — Carneiro–Milinovich–Soundararajan's $g_n \le \tfrac{22}{25}\sqrt{p_n}\log p_n$
— is strong enough to settle the conjecture at exactly three indices, $p = 2, 3, 5$,
and fails at every index after that, permanently. This is not a constant problem:
for **every** constant $c>0$ the usable range is finite, growing only like $c^{-2}$,
so that even $c = 10^{-6}$ — six orders below the ceiling the authors state for
their own method — stops at $p \approx 10^{15}$, still short of the $2^{64}$ range
verified unconditionally without any hypothesis at all. The RH branch of the proof
tree is therefore not weak but **strictly dominated**: it owns no index that plain
computation does not already own. Behind this sits an unconditional reason —
carried here on an unverified locator (**V3**, gap G7), so it explains rather than
proves: Littlewood's oscillation theorem puts the prime-counting error at scale $\sqrt{x}$,
so arguments that bound that error and subtract can never reach the $\log^2 x$ scale
the conjecture needs — assuming more about the zeros does not help, because the
obstruction lies downstream of the zeros. What this leg does **not** prove is that
RH fails to imply the conjecture by some other route; that remains open, and is
stated as the obstruction rather than smuggled in as a conclusion. The one genuine
use found for RH in this attack is on the other branch entirely — certifying the
index $\pi(p)$ when checking a candidate counterexample — where it is $29\times$
more comfortable than the unconditional bound, and where the unconditional bound was
already comfortable by a factor of 1600.

---

*Emitted by the `proof-attempt__1` leg (node), target
`RH-conditional-bound`. Firoozbakht's conjecture is **open**; this leg produced no
evidence about its truth in either direction. `notebooks__1`, the computational leg
for the same target, had produced no artifact at the time of writing — nothing here
depends on it, and §3's computation is self-contained.*
