# Proof attempt #2 — target `unconditional-verified-range`

**Leg**: `proof-attempt__2` (node 7 of the `math-attack` polymer)
**Upstream read**: `decompose/decompose.md`, `frame-deliberation/outcomes.md`,
`source-ledger/source-ledger.md`, `concept-cards/` (all 31)
**Formal backend**: Lean 4 / Mathlib (handoff in §10)
**Date**: 2026-07-24

> **Firoozbakht's conjecture is open. This document does not prove it and does not
> refute it, and nothing below is evidence about its truth.** What is proved here is a
> statement about a *finite range* — a different proposition, which is provable, and
> which this leg proves. §9 states, in the plainest terms available, why the two must
> not be confused.

---

## 0. What the target is

The brief names target #2 `unconditional-verified-range`. That string admits three
readings; they are not equivalent and the leg must commit to one.

| reading | proposition | verdict |
|---|---|---|
| **(R1)** | "F has been verified up to height $H$" — a report about the literature | Not a mathematical target. Already discharged by **CC-25**. Rejected as trivial. |
| **(R2)** | "F holds for every $n$ with $p_n < H$, for an explicit $H$, by an argument that assumes nothing unproved" | **This is the target.** Adopted. |
| **(R3)** | "Unconditional analytic methods extend the verified range beyond the reach of enumeration" | The natural companion question, and the one that decides whether (R2) is a stepping stone or a dead end. Also attacked (§8). |

**Target statement adopted (R2).** Let $p_n$ be the $n$-th prime, 1-indexed
(**CC-01**), $g_n = p_{n+1} - p_n$ (**CC-02**), and let

$$\mathrm{F}(n) \;:\equiv\; p_{n+1}^{\,n} < p_n^{\,n+1}$$

be Firoozbakht's inequality at $n$ in its integer form F2 (**CC-09**). The target is:

> **T2.** Exhibit the largest explicit $H$ for which one can *prove* — with every premise
> named, and none of them an unproved conjecture — that $\mathrm{F}(n)$ holds for every
> $n$ with $p_n < H$; and determine what binds $H$.

The second clause is the part with teeth. A verified range is worth little as a number
and a lot as a diagnosis: it tells you which of the two ingredients — the analytic
criterion or the computational census — is the binding constraint, and therefore where
effort spent on this target actually goes.

---

## 1. Verdict

**T2 is PROVED, conditionally on two named external premises, at**

$$\boxed{\;H \;=\; 10^{20}.\;}$$

The argument (§§2–6) is complete: every step is either proved here from first
principles, or is one of exactly two external inputs, both named, tiered, and isolated:

| premise | what it supplies | tier |
|---|---|---|
| **P1** — Axler, *New bounds for the prime counting function*, Cor. 3.5 (the $3.83$ member): $\pi(x) < x\big/\big(\log x - 1 - \tfrac1{\log x} - \tfrac{3.83}{\log^2 x}\big)$ for $x \ge 9.25$ | the analytic criterion | **V0** (ledger `axler2014newbounds`) · **L2** as a claim |
| **P2** — the census of first-occurrence prime gaps is complete below $10^{20}$, yielding the 85 known maximal gaps | the range | **V1** (ledger `primegaplist_faq`) · **L3** as a claim: a community computational record, not machine-checked, not refereed |

Everything else — the criterion, its monotonicity, the leverage lemma, the base case,
the assembly — is **L0**, proved below. By the propagation rule (**CC-31**) the
conclusion inherits the weakest premise: **Theorem A is L3/V1, and its weak link is
computational data, not mathematics.**

Three subsidiary results:

- **§6.3 — the range is data-bound, not method-bound.** The analytic layer is nowhere
  near binding. At the tightest of the 85 records the criterion is satisfied with
  $5.1\%$ to spare `[C]`; $H$ stops exactly where the gap census stops.
- **§7.4 — the loss from using the unconditional criterion instead of the exact barrier
  is $9.3 \times 10^{-6}$ relative** `[C]`. The relaxation from "the true criterion" to
  "a criterion made of published $\pi(x)$ bounds" costs essentially nothing. **This is
  the reason the target closes at all.**
- **§8 — the obstruction, and it is total.** No unconditional analytic result in the
  literature extends $H$ by a single prime, and none can: the best proven gap bound
  overshoots the barrier by a factor that **diverges** `[C]` — $1.5\times10^{7}$ at
  $10^{20}$, $6\times10^{47}$ at $10^{100}$. R3 fails, and fails permanently.

**$H = 10^{20}$ is a $5.4\times$ improvement on the frontier this polymer carries.**
`decompose.md#2.2/#5.1/#5.2` state $4\times10^{18}$; **CC-25** corrects that to $2^{64}
\approx 1.845\times10^{19}$ (Visser 2019). Neither figure is wrong; both are stale. The
gain is not mathematical — the same argument that gave Visser $2^{64}$ in 2019 gives
$10^{20}$ today, because the census moved. That is precisely §6.3's point, stated as
history instead of as a theorem.

---

## 2. The exact criterion (imported, L0)

**Lemma 0** (**CC-10**, proved there; restated for self-containment). For every
$n \ge 1$, with $L_n := \log p_n$,

$$\mathrm{F}(n) \iff g_n < f_n, \qquad f_n := p_n^{\,1+1/n} - p_n = p_n\big(e^{L_n/n} - 1\big).$$

*Proof.* $\mathrm{F}(n) \iff n\log p_{n+1} < (n+1)\log p_n \iff \log p_{n+1} <
(1+\tfrac1n)L_n \iff p_{n+1} < p_n^{1+1/n}$, using that $\log$ and $\exp$ are strictly
increasing and $p_n \ge 2 > 0$; subtract $p_n$. Every step is an equivalence. $\square$

No side condition, no asymptotic regime, no exceptional small-$n$ set. This is the
object the whole leg bounds.

**Direction discipline** (**CC-15** traps, **CC-10** traps). $f_n$ is *decreasing in $n$*
for fixed $p_n$. Hence **to verify $\mathrm{F}$ one needs an upper bound on
$n = \pi(p_n)$**; a lower bound is what a refutation needs. Every use of $\pi$ below is
an upper bound. Getting this backwards turns a verification into a refutation claim, and
it is the most likely place for a reader to lose the thread.

---

## 3. Lemma 1 — the unconditional criterion

> **Lemma 1.** Let $\psi(x) := \log^2 x - \log x - 1 - \dfrac{3.83}{\log x}$. For every
> $n$ with $p_n \ge 11$,
> $$g_n < \psi(p_n) \;\Longrightarrow\; \mathrm{F}(n).$$

*Proof.* Write $L = L_n = \log p_n$ and $D(x) := \log x - 1 - \tfrac1{\log x} -
\tfrac{3.83}{\log^2 x}$, so that $\psi(x) = \log x \cdot D(x)$ identically.

1. **The barrier dominates its linearisation.** $e^t - 1 > t$ for every $t > 0$, and
   $t = L/n > 0$ since $p_n \ge 2$ and $n \ge 1$. Therefore
   $$f_n = p_n\big(e^{L/n} - 1\big) > p_n \cdot \frac{L}{n} = L \cdot \frac{p_n}{n}.$$

2. **$D$ is positive at $p_n$.** $D(x) > 0 \iff \log^3 x - \log^2 x - \log x - 3.83 > 0$.
   The cubic $u \mapsto u^3 - u^2 - u - 3.83$ is strictly increasing for $u \ge 1$ and
   its unique real root exceeds $\log 9 = 2.197$ and is below $\log(9.25) = 2.2246$;
   numerically $D(9.0) = -0.0512 < 0 < 0.0012 = D(9.25)$ `[C, C7b]`. Moreover $D$ is
   increasing in $x$ on $x > 1$: writing $u = \log x$, $dD/du = 1 + u^{-2} + 7.66u^{-3}
   > 0$. Since $p_n \ge 11 > 9.25$, therefore $D(p_n) > 0$.

3. **The index is bounded above.** $n = \pi(p_n)$, and $p_n \ge 11 > 9.25$, so **P1**
   applies: $\pi(p_n) < p_n / D(p_n)$. As $D(p_n) > 0$ by step 2, this rearranges to
   $$\frac{p_n}{n} \;>\; D(p_n).$$

4. **Assemble.** Chaining 1 and 3,
   $$f_n \;>\; L \cdot \frac{p_n}{n} \;>\; L \cdot D(p_n) \;=\; \psi(p_n).$$
   So $g_n < \psi(p_n) \Rightarrow g_n < f_n$, and Lemma 0 gives $\mathrm{F}(n)$.
   $\square$

**Tier: L0 given P1.** Steps 1, 2, 4 are elementary and complete. Step 3 *is* P1 — it is
the single external input to the criterion, and it enters once, at one place, in one
direction.

**Three remarks on the shape of this lemma.**

- **It is one-way.** $g_n < \psi(p_n)$ is sufficient for $\mathrm{F}(n)$, never
  necessary. A record that failed the test would prove nothing (§9, flag F3).
- **Its hypothesis is $p_n \ge 11$, not $p_n > 4\times10^{18}$.** Kourbatov's Theorem 4
  states the same $3.83$-family condition for $p_k > 4\times10^{18}$ (ledger row
  `kourbatov2015upper` Thm 4), covering the rest by his own verification. That is a
  *weaker* claim than Lemma 1, so there is no conflict — but it means Lemma 1 must be
  read as **derived here (L0 given P1)**, not as a citation of Kourbatov. It is stated
  and proved above for exactly that reason.
- **The corrigendum risk is real and is isolated.** `axler2016corrigendum` moved the
  validity threshold of a *different* member of Cor. 3.5 (the $1.17$ member) from
  $x \ge 5.43$ to $x \ge 2\,634\,800\,823$ — nine orders of magnitude (**CC-19**). The
  ledger and **CC-19** both record the $3.83$ member at $x \ge 9.25$ as unaffected.
  Lemma 1 leans entirely on that. §9 flag F1 states the fallback and §7.3 shows the
  fallback has been computed, so the result survives even if the $9.25$ threshold were
  someday withdrawn.

---

## 4. Lemma 2 — the barrier is monotone

> **Lemma 2.** $\psi$ is strictly increasing on $x \ge e^{1/2}$.

*Proof.* Substitute $u = \log x$, strictly increasing on $x > 0$. Then
$\psi = u^2 - u - 1 - 3.83/u$ and
$$\frac{d\psi}{du} = 2u - 1 + \frac{3.83}{u^2}.$$
For $u \ge 1/2$ every term is $\ge 0$ and the last is $> 0$, so $d\psi/du > 0$; composing
with $u = \log x$ preserves strict monotonicity. $\square$ `[C, C7]`

Trivial, and load-bearing: it is the only reason a *record* gap at the *left endpoint* of
an interval controls the whole interval.

---

## 5. Lemma 3 — maximal-gap leverage

**Definition.** A gap $g_n$ is *maximal* (a *record*) if $g_m < g_n$ for all $m < n$.
List the records as pairs $(P_j, G_j)_{j \ge 1}$: $G_j$ is the $j$-th record value and
$P_j$ the prime it follows. So $G_1 = 1, P_1 = 2$; $G_2 = 2, P_2 = 3$; …

**Observation R** (immediate from the definition). Suppose the record list is known to
be **complete below a height $H$** — no record was missed there. Then for every $n$ with
$p_n < \min(H, P_{j+1})$,
$$g_n \le G_j.$$
*Proof.* If some $g_n > G_j$ with $p_n < P_{j+1}$, then the least such $n$ is a record
strictly between $G_j$ and $G_{j+1}$ in value and strictly before $P_{j+1}$ in position,
contradicting that $(P_{j+1}, G_{j+1})$ is the *next* record — and completeness below
$H$ is what licenses "$(P_{j+1},G_{j+1})$ is the next record" as a fact about the primes
rather than about the table. $\square$

**Completeness is a premise, not a formality.** Every use of Observation R below is
capped at the census height $H$ of **P2**; the leverage lemma is never applied above it.
This is the one place where a merely *long* record list would be worthless and a
*complete* one is essential.

> **Lemma 3 (leverage).** Fix $j_0 \ge 1$ with $P_{j_0} \ge 11$ and $J \ge j_0$, and
> suppose
> 1. $\mathrm{F}(n)$ holds for every $n$ with $p_n < P_{j_0}$  *(base case)*;
> 2. $G_j < \psi(P_j)$ for every $j$ with $j_0 \le j \le J$  *(the record checks)*;
> 3. the record list is complete below a height $H \le P_{J+1}$  *(the data premise)*.
>
> Then $\mathrm{F}(n)$ holds for **every** $n$ with $p_n < H$.

*Proof.* Let $n$ satisfy $p_n < H$. If $p_n < P_{j_0}$, hypothesis 1 gives
$\mathrm{F}(n)$. Otherwise $P_{j_0} \le p_n < H \le P_{J+1}$, so there is a unique
$j \in \{j_0,\dots,J\}$ with $P_j \le p_n < P_{j+1}$. Then

$$g_n \;\overset{\text{Obs. R}}{\le}\; G_j \;\overset{\text{hyp. 2}}{<}\; \psi(P_j)
    \;\overset{\text{Lem. 2}}{\le}\; \psi(p_n) \;\overset{\text{Lem. 1}}{<}\; f_n,$$

the last step legitimate because $p_n \ge P_j \ge P_{j_0} \ge 11$. Lemma 0 gives
$\mathrm{F}(n)$. $\square$

**Tier: L0 given P1.** The lemma is a statement about *any* record list: it consumes
data only through its own hypotheses 2 and 3, and asserts nothing about the primes
beyond them.

**What this buys.** The universally quantified statement over $\pi(10^{20}) \approx
2.22\times10^{18}$ indices collapses to **81 numeric inequalities** plus a base case
`[C, C9]` — a compression of $\approx 2.7\times10^{16}$. And the checks need
**no index data at all**: $\psi$ is a function of $p$ alone. The $n$-column of a
maximal-gap table is not a premise of this argument. That matters, because the $n$-column
is the most expensive and least independently verifiable column in the table
(it requires exact prime counting) — and the argument does not touch it.

---

## 6. Theorem A

### 6.1 Statement

> **Theorem A.** Assume **P1** (Axler Cor. 3.5, the $3.83$ member) and **P2** (the
> first-occurrence gap census is complete below $10^{20}$, with the 85 known records as
> tabulated). Then $\mathrm{F}(n)$ holds for every $n$ with
> $$p_n < 10^{20}.$$

### 6.2 Proof

Take $j_0 = 5$, so $P_5 = 89$ and $G_5 = 8$; $P_5 = 89 \ge 11$. ✓

**Hypothesis 1 (base case).** $\mathrm{F}(n)$ for every $n$ with $p_n < 89$ — that is
$n \le 23$. Verified by exact integer comparison $p_{n+1}^n < p_n^{n+1}$ in Python
bignums, for $n = 1..30$ (i.e. through $p_{30} = 113$, more than needed) `[C, C1]`. No
floating point, no $\pi(x)$ bound, no external input. This is the entire base case:
**23 integer comparisons** ($n = 1..23$, since $p_{23} = 83$ and $p_{24} = 89$).

**Hypothesis 2 (record checks).** $G_j < \psi(P_j)$ for $j = 5..85$, all 81 verified in
60-digit arithmetic `[C, C6b]`; the largest ratio $G_j/\psi(P_j)$ over the range is

$$\max_{5 \le j \le 85} \frac{G_j}{\psi(P_j)} \;=\; \mathbf{0.948545}
  \quad\text{at } j = 64:\; (P_{64}, G_{64}) = (1\,693\,182\,318\,746\,371,\; 1132).$$

**Completeness.** By **P2** the record list is complete below $H = 10^{20}$, and
$P_{84} = 6.807\times10^{19} < 10^{20} < P_{85} = 1.014\times10^{20}$ `[C, C6f]`, so
$H \le P_{85} = P_{J+1}$ with $J = 84$. Lemma 3 applies at $(j_0, J, H) = (5, 84,
10^{20})$. $\square$

**Why the conclusion stops at $10^{20}$ and not at $P_{85} = 1.014\times10^{20}$.**
Lemma 3 would give the larger range if the list were known complete below $P_{85}$. **P2**
as sourced (`primegaplist_faq`: *"All prime gaps in $0 < x < 10^{20}$ have now been
analyzed"*) certifies completeness to $10^{20}$ and no further. The extra $1.4\%$ is
available for the asking and is not claimed here.

### 6.3 The binding constraint

The proof has two hypotheses. Only one of them is anywhere near failing.

| hypothesis | margin |
|---|---|
| the criterion (hyp. 2) | worst case $0.9485$ — **$5.1\%$ of headroom**, at $j = 64$ `[C]` |
| the census (hyp. 2's completeness, P2) | **zero headroom** — the argument stops at exactly the height the census stops |

**The verified range is bound by data, not by method.** Extend the census to $10^{25}$
and — provided every new record still satisfies $G_j < \psi(P_j)$ — Theorem A extends to
$10^{25}$ with no new mathematics, at the cost of the 5-or-so extra inequalities the new
records supply.

*Careful here.* A new record with $G_j/\psi(P_j) \ge 1$ would **not** refute
Firoozbakht; it would only put that record outside the reach of Lemma 1, forcing
adjudication by the exact criterion with the true index (flag **F3**). The relaxation
costs $9\times10^{-6}$ (§7.4), so the two events are numerically almost the same event
— but they are not the same statement, and this document does not conflate them.

This is the finding of the leg, and it is worth stating without hedging: **for target #2,
analytic number theory is not the bottleneck and never was. The sieve is.**

### 6.4 The ten tightest record checks `[C]`

(All 81 checks $j = 5..85$ were run; Theorem A consumes only $j \le 84$, since $H =
10^{20} < P_{85}$. Row 85 is shown because it is the second-tightest in the table and
because it is what a census extension past $10^{20}$ would immediately need.)

| $j$ | $G_j$ | $P_j$ | $\psi(P_j)$ | $G_j/\psi(P_j)$ |
|---|---|---|---|---|
| 64 | 1132 | 1 693 182 318 746 371 | 1193.407 | **0.94855** |
| 85 | 1854 | 101 412 319 996 363 309 069 | 2074.902 | 0.89354 |
| 6 | 14 | 113 | 15.811 | 0.88548 |
| 74 | 1442 | 804 212 830 686 677 669 | 1657.479 | 0.87000 |
| 83 | 1676 | 20 733 746 510 561 442 863 | 1932.754 | 0.86716 |
| 75 | 1476 | 1 425 172 824 437 699 411 | 1704.417 | 0.86599 |
| 61 | 906 | 218 209 405 436 543 | 1055.955 | 0.85799 |
| 73 | 1370 | 418 032 645 936 712 127 | 1604.608 | 0.85379 |
| 72 | 1356 | 401 429 925 999 153 707 | 1601.361 | 0.84678 |
| 57 | 766 | 19 581 334 192 423 | 904.972 | 0.84644 |

The minimum over $j \ge 5$ is $0.4904$ at $j = 11$ `[C]`. Two things are visible. First,
the tightest check in the whole table is the record locus of **CC-26** — the same
$1132$-after-$1.693\times10^{15}$ that carries the polymer's headline shortfall, and it
is tightest by a clear margin over the runner-up. Second, **row 6 — the gap $14$ after
the prime $113$ — sits third**, above 79 records spanning eighteen orders of magnitude.
The small-$n$ tightness that **CC-25** says the literature framing obscures is visible in
this table too, and it is not an artefact of the relaxation: at $j=6$ the exact criterion
gives $g/f = 0.725909$ `[C, C6g]`, so the relaxation costs real margin only where $p$ is small, exactly
where it does not matter.

---

## 7. What was computed here `[C]`

`verify_pa2.py` (this directory), **21 named checks, all pass**, exit 0, ~130 s wall
clock (the sieve dominates). Output in `verify_pa2.out`. Every `[C]` tag in this document points at one check.

### 7.1 Independent verification below $3\times10^9$

A segmented sieve over $[2, 3\times10^9)$ — **144 449 537 primes**, hence 144 449 536
gaps, last prime $2\,999\,999\,929$. For every index $n \le 144\,449\,536$:

- **C3**: the exact criterion $g_n < f_n$ holds. Zero violations. Minimum relative slack
  $(f_n - g_n)/f_n = \mathbf{0.088015}$, attained at $n = 4$ ($p=7$, $g=4$) `[C, C3]`.
  Not at $n=2$ — $n=2$ gives $0.089316$, second-tightest. The two tightest indices in
  $144$ million are $n = 4$ and $n = 2$, which are exactly the two indices
  `outcomes.md#A4` isolates as the dead band's non-empty cases, and they beat every index
  up to $3\times10^9$. **CC-03** reports the minimum of $S_n$ at $n = 2$; $S_n$ and the
  *relative* slack $(f_n-g_n)/f_n$ are different functionals and rank these two indices
  differently. Both statements are correct; a downstream leg quoting "the tightest index"
  must say which functional it means.
- **C3b**: no index came within $10^{-3}$ of the threshold, so no case required exact
  re-adjudication; the double-precision path is safe throughout this range (**CC-27**'s
  caution binds above $p \sim 10^{16}$, far above this sieve).
- **C4**: the $\psi$-criterion $g_n < \psi(p_n)$ holds for **every** index with
  $p_n \ge 89$. Zero violations. This is Lemma 1's hypothesis verified at 144 million
  points rather than at 33 record points — an independent confirmation that the
  criterion is not merely satisfied at the records but everywhere.
- **C5**: the 33 maximal gaps below $3\times10^9$ recomputed from the sieve match rows
  1–33 of the table **exactly**, value and position.

### 7.2 Cross-checks on the data source

The maximal-gap table is the weak premise (P2), and it was taken from a **tertiary**
source: the Wikipedia article *Prime gap* (`Special:Export`, retrieved 2026-07-24),
which cites Andersen's record-gap pages and OEIS A005250 / A002386 / A005669. Three
independent corroborations were run rather than trusting it:

1. **C5** — rows 1–33 reproduced from this leg's own sieve. Exact match.
2. **C6d** — row 64's exact barrier recomputed from $(P_{64}, n_{64})$ in 60 digits
   gives $f = 1193.417778$, matching **Kourbatov 2015, Table 1, last row: $1193.418$**
   (V0). The table's *index* column is thereby corroborated against a published,
   verbatim-read primary at the one row where it matters most.
3. **C6e** — Visser 2019's V0 statement is "verified below the location of the 81st
   maximal prime gap, certainly for all $p < 2^{64}$". That statement is *only* correct
   if $P_{80} < 2^{64} < P_{81}$. The table gives
   $$P_{80} = 18\,361\,375\,334\,787\,046\,697 \;<\; 2^{64} = 18\,446\,744\,073\,709\,551\,616 \;<\; P_{81} = 18\,470\,057\,946\,260\,698\,231. \;\checkmark$$
   A tertiary table reproducing a 2019 refereed claim about a value that was *unknown*
   in 2019 is a genuine consistency check, not a restatement.

Rows 34–85 remain a data dependency this leg has not discharged (§9, flag F2).

### 7.3 The fallback route is live

If the $x \ge 9.25$ threshold of P1 were ever withdrawn the way the $1.17$ member's
$x \ge 5.43$ was, Lemma 1 would have to be rerun with the corrected member,
$\pi(x) < x/(\log x - 1 - 1.17/\log x)$ for $x \ge 2\,634\,800\,823$, giving the barrier
$\log^2 x - \log x - 1.17$ and requiring the base case to reach $2\,634\,800\,823$
instead of $89$. **That contingency has been pre-paid**: the sieve deliberately runs to
$3\times10^9 > 2\,634\,800\,823$, and

- **C3** covers the enlarged base case exactly, and
- **C4b** confirms $g_n < \log^2 p_n - \log p_n - 1.17$ for every index with
  $2\,634\,800\,823 \le p_n < 3\times10^9$, zero violations,

so the fallback assembles with no new computation. The ratio column of §6.4 shifts by
less than $10^{-4}$ under this substitution `[C, C6b vs the `kour` column]`. **Theorem A
does not depend on which member of Axler's family survives.**

### 7.4 The cost of the relaxation

At the record locus:

$$f_{64} = 1193.417778, \qquad \psi(P_{64}) = 1193.406697, \qquad
  \frac{f - \psi}{f} = \mathbf{9.285\times10^{-6}} \;\;`[C, C6d]`.$$

So replacing the exact, uncomputable-without-$\pi$ barrier $f_n$ by the fully explicit
$\psi$ costs **nine parts per million** at the tightest point of the whole argument. The
$g/f$ and $g/\psi$ ratios there are $0.948536$ and $0.948545$ — they agree to five
decimal places.

This is why the target closes. Had the explicit machinery cost even $1\%$, the $j=64$
check ($0.9485$) would still have passed; had it cost $6\%$, Theorem A would have failed
at exactly one row out of 81 and $H$ would have collapsed from $10^{20}$ to
$1.7\times10^{15}$. **The margin of the whole result is $5.1\%$ and the machinery
consumes $0.0009\%$ of it.** The explicit-bounds layer is not the fragile part of this
argument; the census is.

---

## 8. Theorem B — the obstruction (reading R3)

Reading R3 asked whether unconditional analysis can carry the range past the census. It
cannot, and the failure is not quantitative.

> **Theorem B.** Let $\Theta$ be any of the unconditional gap bounds available in the
> literature — the explicit short-interval results (Dusart 2010, Prop. 6.8: a prime lies
> in $(x,\, x(1 + 1/(25\log^2 x))]$ for $x \ge 396\,738$) or the asymptotic
> Baker–Harman–Pintz bound $g_n \ll p_n^{0.525}$. Then for every $x \ge 10^4$,
> $$\Theta(x) \;>\; \psi(x),$$
> and $\Theta(x)/\psi(x) \to \infty$. Consequently no such bound implies
> $\mathrm{F}(n)$ for even a single $n$ above the census.

*Proof sketch and quantification.* $\psi(x) = \log^2x - \log x - 1 - 3.83/\log x \sim
\log^2 x$. Dusart's interval has length $x/(25\log^2x)$, so
$\Theta_{\mathrm{D}}/\psi \sim x/(25\log^4 x) \to \infty$. BHP gives
$\Theta_{\mathrm{BHP}}/\psi \sim x^{0.525}/\log^2x \to \infty$. Both ratios computed
exactly `[C, C8]`:

| $x$ | $\psi(x)$ | Dusart 6.8 $/\,\psi$ | $x^{0.525}/\psi$ |
|---|---|---|---|
| $10^{9}$ | 407.5 | $2.29\times10^{2}$ | $1.30\times10^{2}$ |
| $10^{15}$ | 1157.3 | $2.90\times10^{7}$ | $6.48\times10^{4}$ |
| $2^{64}$ | 1922.5 | $1.95\times10^{11}$ | $6.77\times10^{6}$ |
| $10^{20}$ | 2073.6 | $9.10\times10^{11}$ | $1.53\times10^{7}$ |
| $10^{40}$ | 8389.9 | $5.62\times10^{30}$ | $1.19\times10^{17}$ |
| $10^{100}$ | 52787.7 | $1.43\times10^{89}$ | $5.99\times10^{47}$ |

$\square$

**Three honesty notes on Theorem B.**

- **The BHP column is generous to the opposition twice over.** It treats $p^{0.525}$ as
  if the implied constant were $1$ and the bound effective; it is neither (ledger tier
  L2, and the result is asymptotic with an unspecified threshold). The real distance is
  larger than the column shows.
- **There is a middle window where $x^{0.525} < \psi(x)$** — it ends at $x = 1230$
  `[C, C8b]`. It is a curiosity, not a route: it lies six orders below the range where
  $\mathrm{F}$ is already settled by exact integer arithmetic `[C, C8c]`, and BHP is not
  valid there anyway.
- **Theorem B is about the bounds in the literature, not about all possible
  arguments.** It refutes R3 for every route this leg could find. It does **not** prove
  that no unconditional argument can extend the range — that stronger universal negative
  is precisely the claim **CC-23** carries at tier **L3**, and this leg does not upgrade
  it. §9, flag F5.

**Combining A and B**: the verified range is exactly the census extent, the analytic
layer contributes the *criterion* and never the *range*, and this is a permanent
structural feature rather than a temporary state of the art.

---

## 9. Gaps, flags, and what is not established

Stated as obligations, in decreasing order of how much they would cost if wrong.

| # | flag | severity |
|---|---|---|
| **F1** | **P1 is a citation, not a proof.** Axler Cor. 3.5 is read at V0 through the ledger; this leg did not obtain the paper. Its $3.83$ member's threshold $x \ge 9.25$ is asserted unaffected by `axler2016corrigendum` on the authority of **CC-19** + the ledger. *Mitigation*: §7.3 — the fallback via the corrected $1.17$ member is fully computed and assembles without new work. **Not fatal under any outcome.** |
| **F2** | **P2 is a tertiary data source.** Rows 34–85 of the maximal-gap table come from Wikipedia and have **not** been reproduced by this leg. Rows 1–33 were `[C, C5]`; rows 64, 80, 81 were corroborated against V0/V1 primaries `[C, C6d, C6e]`. Rows 34–63, 65–79, 82–85 rest on the tertiary source alone. This is **the weak link of Theorem A** and it is a *completeness* claim (no record was missed), which is strictly harder to corroborate than the individual rows. **OB-B3 remains OPEN** (**CC-25**), and this leg does not close it. |
| **F3** | **Lemma 1 is one-way.** A record failing $G_j < \psi(P_j)$ would prove nothing — neither $\mathrm{F}$ nor $\lnot\mathrm{F}$ at that index; adjudication would fall to the exact criterion (Lemma 0) with the true index, i.e. **OB-E4**. No record fails, so this is latent, but any downstream leg reusing §6.4's table must not read a ratio near $1$ as "nearly a counterexample". |
| **F4** | **Theorem A says nothing about $p \ge 10^{20}$.** Not "probably true above", not "evidence for". Nothing. See §10. |
| **F5** | **Theorem B is not a universal negative.** It quantifies the distance for the bounds this leg found. The claim "no technique in analytic number theory produces a polylogarithmic gap bound at all" stays at **L3** per **CC-23** / `outcomes.md#A9`, and this leg neither uses nor strengthens it. |
| **F6** | **The improvement $2^{64} \to 10^{20}$ is not mathematical progress.** Same argument, newer census. Stating it as a result of the *method* would be a misattribution. §6.3 exists to prevent that. |
| **F7** | **Nothing here is new mathematics.** Lemma 1 is Kourbatov's Theorem 4 family re-derived with a wider validity range; Lemma 3 is the standard maximal-gap leverage every verification paper uses implicitly. What is new is that both are written out with every step justified, the ratio table is published, the fallback is pre-computed, and the binding constraint is named. That is an *artefact*, not a *theorem*. |

---

## 10. What this does and does not establish

**Does.** For every $n$ with $p_n < 10^{20}$ — roughly $2.2\times10^{18}$ indices —
Firoozbakht's inequality holds, by an argument with two named external premises, 81
checkable inequalities, a 23-comparison base case, and no unproved conjecture anywhere.

**Does not.** Anything whatsoever about $p_n \ge 10^{20}$. Finite verification cannot
prove a universal statement; it relocates it (**CC-25**). Every heuristic in the corpus
that bears on the tail — Granville's $\limsup R_n \ge 2e^{-\gamma} \approx 1.1229$
(**CC-21**), the divergence of $e\log\log X$ in `attack.md#4` — points *against* the
conjecture, and none of them is touched by Theorem A. A reader who takes $H = 10^{20}$
as encouragement has misread the document; the correct reading is that the region where
the conjecture is *interesting* begins exactly where this theorem ends.

**The one number worth carrying downstream.** $0.9485$ — the worst record check in the
argument, at the $1132$-gap. Not because it is close to failing, but because it is the
*only* row that is close at all, and it is at the same locus that carries the polymer's
refutation-side headline (**CC-26**, shortfall $1.05426$). The verification frontier and
the refutation frontier are the same point in the primes, approached from opposite sides:

$$\frac{g_{64}}{f_{64}} = 0.948536, \qquad \frac{1}{0.948536} = \frac{f_{64}}{g_{64}}
  = \mathbf{1.054256} = \text{CC-26's shortfall} \quad `[C, C10]`.$$

(The relaxed ratio $g/\psi = 0.948545$ differs in the fifth decimal — §7.4.) **They are
the same number.** Everything this leg proves and everything the refutation branch hopes
for meet at one prime, $1\,693\,182\,318\,746\,371$, with $5.4\%$ between them.

### Lean handoff (backend: Lean 4 / Mathlib)

The argument decomposes for formalization about as well as anything in this polymer.

| obligation | Lean shape | difficulty |
|---|---|---|
| Lemma 0 (exact criterion) | `Real.rpow`, `Real.exp_lt_exp`, `Real.log_lt_log_iff` | medium — **CC-10** trap: needs `rpow`, unlike the F2 route |
| Lemma 1 step 1 ($e^t-1>t$) | `Real.add_one_lt_exp` | low |
| Lemma 1 step 2 ($D>0$) | `norm_num` on the cubic + monotonicity | low |
| Lemma 1 step 3 (P1) | **must be an `axiom` or a hypothesis** — Axler's bound is not in Mathlib and is not provable there | **blocked**; state as an explicit hypothesis, never a `sorry` (**CC-29**) |
| Lemma 2 (monotone $\psi$) | `StrictMonoOn`, derivative or direct | low |
| Lemma 3 (leverage) | pure order reasoning over a finite list; no analysis | low — **the highest value-per-unit-effort item in this leg** |
| Base case ($n \le 23$) | `decide` / `norm_num` on `Nat` in F2 form (**CC-28**) | low |
| §6.2 assembly | 81 `norm_num` inequalities over a hard-coded record list | mechanical, but the list is data (F2) |

**Recommendation to `lean-skeleton` / `lean-probe`**: formalize **Lemma 3 with P1 and the
record list as explicit hypotheses**. That yields a `sorry`-free theorem of the form
"*given an $\pi(x)$ upper bound of Axler's shape and a complete record list with these
81 inequalities, F holds below the list's extent*" — which is honest, kernel-checked,
and exactly delineates what is mathematics and what is data. It is also strictly more
useful than a Lean-checked $N_0 = 10^5$ (**CC-25**'s revised W5 target), because it makes
the *data dependency itself* a formal object.

---

## 11. Reproduction

```
cd proof-attempt__2
python3 verify_pa2.py          # 21 checks, exit 0, ~130 s (numpy + mpmath)
```

Files:

| file | role |
|---|---|
| `proof-attempt-2.md` | this document |
| `verify_pa2.py` | every `[C]` claim, one named CHECK each |
| `verify_pa2.out` | its output, as run 2026-07-24 |
| `maxgaps.json` | the 85 maximal gaps `(j, G_j, P_j, n_j)` — **P2**, the tertiary data premise (§9 F2) |

Check-to-claim map: C1 → §6.2 base case · C2 → P1 transcription · C3/C3b → §7.1 ·
C4 → §7.1 · C4b → §7.3 · C5 → §7.2(1) · C6b → §6.2, §6.4 · C6c → §6.2 choice of $j_0$ ·
C6d → §7.2(2), §7.4 · C6e → §7.2(3) · C6f → §6.2 · C6g → §6.4 · C7/C7b → Lemma 1
step 2, Lemma 2 · C8/C8b/C8c → §8 · C9 → §5 · C10 → §10.

---

*Emitted by the `proof-attempt__2` leg (node), target
`unconditional-verified-range`. The conjecture is **open**. This leg proves a statement
about a finite range and produces no evidence about the conjecture's truth in either
direction. `proved` is the kernel's word, and it has not been said here.*
