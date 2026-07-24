# Proof attempt — target #0, `first-failure-maximality` (FFM)

**Leg**: `proof-attempt__0` of the `math-attack` polymer (node)
**Crew role**: proofsmith · **Formal backend requested**: Lean 4 / Mathlib
**Attribution**: Noogram · **Date**: 2026-07-24

> **Posture (non-negotiable).** Firoozbakht's conjecture (F) is **open**. Nothing below
> assumes it. Nothing below asserts it. A target reaches `proved` only through the
> kernel/Lean leg downstream; this document produces an *argument*, not a certificate.

---

## 0. Verdict, up front

| question | answer |
|---|---|
| Is FFM **proved** unconditionally, for all $n$? | **No.** §9 states the obstruction exactly. |
| Is FFM **refuted**? | **No — and it cannot be refuted without first refuting F** (Prop. 2). |
| Is FFM **proved** on a range? | **Yes**, on $[89,\ H)$ for every height $H$ to which the maximal-gap table is complete — up to $10^{20}$ today (Thm. 7). Tier **L1 + L2-completeness**. |
| Is the *simple* route to FFM alive? | **No.** The monotone-barrier route (C-mono) is **refuted** by an exact-integer witness at $n=7,8$ (Thm. 4). |
| Is FFM **load-bearing** for the verification literature, as the leg brief and `notebooks__0` assume? | **No.** §8: a strictly weaker, FFM-free argument certifies the same range. The premise that motivated this target is **false as stated**. |

**The one-sentence result.** FFM is *logically weaker* than F (it is implied by it,
vacuously), yet **every known route to proving FFM either passes through F itself or
through a statement strictly stronger than F** — so the apparent weakness buys nothing.
That dichotomy is Theorem 6, and it is this leg's deliverable.

---

## 1. Conventions and the statement

Indexing per **CC-01**: $p_1 = 2$, $p_2 = 3$, …; $g_n := p_{n+1} - p_n$ (**CC-02**);
$L_n := \log p_n$ (natural log).

**Firoozbakht's conjecture (F).** For every $n \ge 1$:
$$p_{n+1}^{\,1/(n+1)} \;<\; p_n^{\,1/n} \qquad\text{equivalently}\qquad p_{n+1}^{\,n} < p_n^{\,n+1}.$$
Write "**F at $n$**" for the single inequality at index $n$, and "F" for the universally
quantified statement.

**Target #0 — first-failure maximality (FFM).**

> If F fails at all, and $n^\*$ is the **least** index at which it fails, then $g_{n^\*}$
> is a **record** (maximal) gap: $g_{n^\*} > g_m$ for every $m < n^\*$.

FFM is the statement that licenses reading a *maximal-gap table* instead of enumerating
primes. It is a conditional whose antecedent is the negation of F, so it is a statement
about a hypothetical object. That shape drives everything below.

---

## 2. Proposition 1 — the exact criterion  `[L0]`

$$\textbf{For every } n \ge 1:\qquad \text{F at } n \iff g_n < f_n,\qquad
f_n \;:=\; p_n^{\,1+1/n} - p_n \;=\; p_n\big(e^{L_n/n} - 1\big).$$

**Proof.** $p_{n+1}^n < p_n^{n+1} \iff n\log p_{n+1} < (n+1)\log p_n$ ($\log$ strictly
increasing, both sides positive) $\iff \log p_{n+1} < (1 + 1/n)\log p_n \iff
p_{n+1} < p_n^{1+1/n}$ ($\exp$ strictly increasing) $\iff g_n < f_n$ (subtract $p_n$).
Every step is an equivalence. $\blacksquare$

This is **CC-10**, restated here because the whole document is written in $f_n$. No side
condition, no exceptional small-$n$ set, no external explicit bound. Note $g_n < f_n$ and
$g_n \le f_n$ define the same predicate (equality would force $p_{n+1}^n = p_n^{n+1}$,
impossible by unique factorisation), but a formalisation must pick one; we use $<$.

Call $\sigma_n := f_n - g_n$ the **shortfall** at $n$. F at $n$ $\iff \sigma_n > 0$.

---

## 3. Proposition 2 — the asymmetry of FFM  `[L0]`

**(a) F $\Rightarrow$ FFM, vacuously.** If F holds, there is no least failure, so FFM's
antecedent is false and FFM is true.

**(b) Hence: any refutation of FFM contains a refutation of F.** To exhibit a
counterexample to FFM one must exhibit $n^\*$, the least index at which F fails — in
particular one must exhibit an index at which F fails.

**Consequences, and they are not cosmetic.**

1. The "REFUTE" branch of this leg's brief is **unreachable** except through a refutation
   of F. This leg therefore cannot deliver a counterexample to FFM without delivering
   something strictly larger than what the polymer's other legs are hunting. It did not.
2. **FFM is strictly weaker than F.** So a proof of FFM is *a priori* cheaper. §§4–6 ask
   whether that discount is real. It is not (Thm. 6).
3. FFM is **empirically unfalsifiable below the verification frontier**: on any range
   where F is verified, FFM is vacuously true there and no computation can say more.
   `notebooks__0`'s scan to $5.08\times10^{7}$ therefore *cannot* have been testing FFM
   itself; what it measured was the **certificate** (C-head) below. That distinction is
   real content, not pedantry — the certificate can fail where FFM holds.

---

## 4. Theorem 3 — the certificate (C-head), and its near-necessity  `[L0]`

Let $G_{n-1} := \max_{m<n} g_m$ (empty max $:= 0$).

> **(C-head)** If $G_{n-1} < f_n$, then FFM holds at $n$ — i.e. $n$ cannot be a
> counterexample to FFM as a first-failure index.

**Proof.** Suppose FFM fails with least failure $n^\* = n$. Then F fails at $n$, so by
Prop. 1 $g_n \ge f_n$; and failure of maximality gives some $m < n$ with $g_m \ge g_n$.
Chaining, $G_{n-1} \ge g_m \ge g_n \ge f_n$, contradicting $G_{n-1} < f_n$. $\blacksquare$

**Near-necessity.** The proof shows the converse implication at the level of the
certificate: *if* FFM fails at $n$, *then* $G_{n-1} \ge f_n$. So (C-head) is not a lossy
sufficient condition dressed up — it is the exact negation of the only way FFM can fail.
Formally:
$$\text{FFM fails at } n \;\Longrightarrow\; G_{n-1} \ge f_n \;\Longrightarrow\; \neg(\text{C-head at } n).$$
The one-way slack is that $G_{n-1} \ge f_n$ does **not** imply FFM fails: it also requires
F to actually fail at $n$.

**What (C-head) costs.** $G_{n-1}$ is the record gap below $p_n$ and $f_n = L_n^2 - L_n
- 1 + o(1)$ (**CC-18**, Kourbatov Thm. 5, V0). So "(C-head) for all $n$" *is* a
Cramér-type upper bound on maximal gaps with leading constant exactly $1$. That is the
same barrier F itself must clear (**CC-16**, **CC-23**). Held at full generality, the
certificate is not cheaper than the conjecture. Held on a *finite* range with a *complete
record table*, it is $\sim 80$ inequalities (§7).

---

## 5. Theorem 4 — the monotone-barrier route is dead  `[L0, exact integers]`

The cheapest imaginable proof of FFM: show $f$ is nondecreasing. Then $m<n^\*$ would give
$f_m \le f_{n^\*}$, while the chain in Thm. 3 forces $f_m > f_{n^\*}$ — contradiction, and
FFM would follow with no arithmetic input at all. Call this route **(C-mono)**.

> **Theorem 4.** $f$ is **not** nondecreasing. Explicitly, $f_7 > f_8$:
> $$17^{8/7} - 17 \;>\; 19^{9/8} - 19 .$$

**Proof (no floating point on the decision path).** The claim is equivalent to
$17^{8/7} + 2 > 19^{9/8}$. Put $S = 10^{30}$ and let
$$A := \frac{\lfloor S\,17^{8/7} \rfloor}{S}, \qquad B := \frac{\lfloor S\,19^{9/8}\rfloor + 1}{S},$$
computed by integer bisection: $A$ is certified by the integer inequality
$A_{\mathrm{num}}^7 \le 17^8 \cdot S^7$ (hence $A \le 17^{8/7}$) and $B$ by
$B_{\mathrm{num}}^8 \ge 19^9 \cdot S^8$ (hence $B \ge 19^{9/8}$). `verify_pa0.py` CHECK C1
returns
$$A = 25.481637825216555626\ldots,\qquad B = 27.453505138773696359\ldots,$$
both certificates `True`, and $A + 2 = 27.4816\ldots > B$. Therefore
$17^{8/7} + 2 \ge A + 2 > B \ge 19^{9/8}$. $\blacksquare$

For orientation (not part of the proof): $f_7 - f_8 = 0.0281326864\ldots$, matching
`notebooks__0` M2, which reports $D_8 = 0.028133$ as the running-max defect at $n=8$.

**How dead is (C-mono)?** Not marginally: `notebooks__0` M1 finds $\Delta_n = f_{n+1}-f_n
< 0$ at **57.88 %** of the $5.08\times 10^{7}$ steps it scanned. Descent is the *majority*
behaviour, not an exception. The mechanism is transparent — $f_n \approx (p_n/n)L_n$, and
$p_n/n$ falls whenever $g_n$ is below the local average gap, which is most of the time.

This is the leg's first firmly negative result: **the free route to FFM does not exist.**

---

## 6. Theorem 5 (dip bound) and Theorem 6 (the dichotomy)

(C-mono) is dead, but the *quantitative* residue survives: how far below its own past
maximum can $f$ fall?

### 6.1 Theorem 5 — an explicit dip bound  `[L1, on explicit external bounds]`

Write $f_k = (p_k/k)\cdot L_k \cdot \varphi(L_k/k)$ with $\varphi(x) = (e^x-1)/x$, which
is increasing and $\ge 1$. From **CC-20** (Dusart 2010, V0):

$$a(k) := \log k + \log\log k - 1 + \frac{\log\log k - 2.1}{\log k} \;\le\; \frac{p_k}{k}
\quad (k \ge 3) \qquad \text{[Prop. 6.7]}$$
$$b(k) := \log k + \log\log k - 1 + \frac{\log\log k - 2}{\log k} \;\ge\; \frac{p_k}{k}
\quad (k \ge 688\,383) \qquad \text{[Prop. 6.6]}$$

Define, for $k \ge K_0 := 688\,383$,
$$\Lambda(k) := a(k)\log\big(k\,a(k)\big), \qquad
\Upsilon(k) := b(k)\log\big(k\,b(k)\big)\,\varphi\!\Big(\tfrac{\log (k\,b(k))}{k}\Big).$$

Then $\Lambda(k) \le f_k \le \Upsilon(k)$ for $k \ge K_0$ (the lower bound uses
$\varphi \ge 1$ and $L_k \ge \log(k\,a(k))$; the upper uses $\varphi$ increasing).

> **Theorem 5.** For every $n$ with $K_0 < n \le 10^{20}$ (the range on which
> $\Upsilon,\Lambda$ are grid-checked monotone — gap **G-PA0-1**; beyond it the same
> statement holds under G-PA0-1's general form),
> $$\max_{m<n} f_m \;-\; f_n \;\le\; D^\*(n) := \max\big(\Upsilon(n),\,\mathcal H\big) - \Lambda(n),
> \qquad \mathcal H := \max_{m < K_0} f_m .$$
> `verify_pa0.py` CHECK C3 computes $\mathcal{H} = 243.714090$ at $m = 688\,377$ (sieved
> directly — Dusart gives no upper bound below $K_0$, so the head is measured, not
> estimated), and reports
> $$D^\*(n) < 0.1314 \ \ (n \ge 688\,383), \qquad D^\*(n) < 0.1300 \ \ (n \ge 794\,328),$$
> decreasing thereafter: $0.1199$ at $n = 10^9$, $0.11436$ at the record locus
> $n = 49\,749\,629\,143\,526$, $0.1111$ at $n = 10^{19}$.

**Proof.** For $m$ with $K_0 \le m < n$: $f_m \le \Upsilon(m) \le \Upsilon(n)$, using
monotonicity of $\Upsilon$ on $[K_0, n]$. For $m < K_0$: $f_m \le \mathcal H$ by
definition. And $f_n \ge \Lambda(n)$. Take the max of the two upper bounds and subtract.
$\blacksquare$

**Gap G-PA0-1 (inherited shape).** Monotonicity of $\Upsilon$ (and of $\Lambda$) is
verified on a dense logarithmic grid over $[K_0,\,10^{20}]$, **not proved**. This is the
same debt `notebooks__2` carries for $\underline B$ (its gap G-N2). Both are routine
calculus on an explicit elementary function and both are *unproved here*. Flagged, not
hidden. A Lean formalisation must discharge it (§11).

**Falsification test (CHECK C5).** A transcribed inequality can be wrong in a way no
consistency check notices, so the sandwich was tested against the *true* $f_k$ at every
Dusart-valid index of a real sieve: $582\,225$ indices, $k = 688\,383 \ldots 1\,270\,607$
($p < 2\times10^{7}$). **Zero violations on either side.** Tightest lower slack
$f - \Lambda = 0.039233$ at $k = 841\,519$; tightest upper slack
$\Upsilon - f = 6.0\times10^{-5}$ at $k = 688\,383$ — i.e. Dusart's Prop. 6.6 is nearly
sharp exactly at its own validity threshold, which is the signature one wants to see and
not what a mis-transcription would produce. The record locus is inside the bracket too:
$1193.345367 \le 1193.417778 \le 1193.459723$.

**Cross-check.** `notebooks__0` M2 measures the true running-max defect: max $D_n =
0.5487$ at $n = 214$ — *below* the Dusart validity range, so no contradiction — and
$4.564\times10^{-3}$ in the decade $10^6 \le n < 10^7$, $9.29\times10^{-4}$ in
$10^7 \le n < 10^8$. The certified bound $0.13$ is conservative by two orders, as an
explicit bound built from a $0.1/\log k$-wide bracket must be.

### 6.2 Theorem 6 — the dichotomy  `[L0 given Thm. 5]`

> **Theorem 6.** Suppose F first fails at $n^\*$ with $K_0 < n^\* \le 10^{20}$ (Thm. 5's
> certified range) and FFM fails there. Then there exists $m < n^\*$ with
> $$0 \;<\; \sigma_m \;=\; f_m - g_m \;\le\; D^\*(n^\*) \;<\; 0.132 .$$

**Proof.** Failure of maximality gives $m < n^\*$ with $g_m \ge g_{n^\*}$. Minimality of
$n^\*$ gives F at $m$, so $\sigma_m > 0$. Failure at $n^\*$ gives $g_{n^\*} \ge f_{n^\*}$
(Prop. 1). Chaining, $g_m \ge g_{n^\*} \ge f_{n^\*}$, so
$$\sigma_m = f_m - g_m \;\le\; f_m - f_{n^\*} \;\le\; \max_{m<n^\*} f_m - f_{n^\*} \;\le\; D^\*(n^\*). \qquad \blacksquare$$

**Read what this says.** An FFM violation requires *two* extreme events, not one: an
actual Cramér-scale failure at $n^\*$, **and** an earlier index $m$ that missed failing by
less than $0.132$ — on a barrier of size $\approx 1200$ at the tightest known locus, a
relative miss of $10^{-4}$. At that locus (**CC-26**) the actual shortfall is
$$\sigma = f_k - g_k = 61.4177782940\ldots \qquad\text{versus}\qquad D^\*(k) = 0.114355,$$
a factor **537** (`verify_pa0.py` CHECK C4). The closest approach on record is 537 times
too far away to permit the configuration Theorem 6 requires.

**The head closes by computation.** Theorem 6's witness $m$ may lie below $K_0$, so the
hypothesis to be excluded is *a priori* about all $m$. But the head is finite and was
swept: CHECK C3 reports
$$\min_{m < K_0} \sigma_m \;=\; 0.196152 \quad\text{at } m = 2 \ (p=3,\ g=2),$$
and $0.196152 > 0.1314 \ge D^\*$, so no $m < K_0$ can serve as Theorem 6's witness. Note
how *thin* that is: a factor $1.49$, and the binding index is $m = 2$ — **CC-14**'s
small-$n$ dead band very nearly reaches up and touches this argument. A dip bound weaker
than $0.196$ would reopen the head and force it back into the hypothesis.

**And now the sting.** With the head closed, the hypothesis Theorem 6 asks us to exclude —
$$(\text{NM}) \qquad \sigma_m > 0.132 \ \text{ for every } m > K_0$$
— is **strictly stronger than F**, which says only $\sigma_m > 0$. So:

> **Corollary (the dichotomy).** Proving FFM via the dip route requires proving (NM),
> which implies F. Proving FFM via (C-head) requires a Cramér-type bound with constant
> $1$ on record gaps, which is the same barrier F must clear (**CC-23**). Proving FFM
> vacuously requires proving F. **There is no known route to FFM that does not prove, or
> assume, at least as much as F itself.**

This is why FFM's logical weakness (Prop. 2) is not a discount: the weakness is entirely
concentrated in the vacuous case, and every non-vacuous route re-imports the full cost.

---

## 7. Theorem 7 — FFM on the tabulated range  `[L1 + L2-completeness]`

The one place where (C-head) is cheap is a finite range with a *complete* maximal-gap
table.

**Input (external, tiered).** $(P_i, G_i)_{i \le 85}$, the primes preceding maximal gaps
and the maximal gaps, from OEIS A002386 / A005250, retrieved and checksummed by
`notebooks__2` (`data/PROVENANCE.md`; **L2/V2**, community computational record). The
load-bearing property is **completeness** — that no maximal gap is missing below the
stated height — which is *not* checkable from a b-file. It is asserted by
`visser2019verifying` (V0) below $2^{64}$, `kourbatov2015verification` (V1) below
$4\times10^{18}$, `primegaplist_faq` (V1, unrefereed) below $10^{20}$.

**Barrier lower bound depending on $p$ alone.** $f_n = p(p^{1/n}-1)$ is *decreasing* in
$n$ for fixed $p$ (**CC-10** Traps), so an **upper** bound on $\pi$ gives a **lower** bound
on $f$. From `axler2014newbounds` Cor. 3.5 (V0):
$$\pi(x) < \pi^+(x) := \frac{x}{\log x - 1 - 1/\log x - 3.83/\log^2 x} \quad (x \ge 9.25)
\;\Longrightarrow\; f_{\pi(p)} \;\ge\; \underline B(p) := p\big(e^{\log p / \pi^+(p)} - 1\big).$$

> **Theorem 7.** Let the record table be complete on $[2, H)$ and suppose
> $\underline B$ is nondecreasing there. If $G_i < \underline B(P_i)$ for every record
> index $i$ with $P_i \ge 89$, then **(C-head) holds at every $n$ with
> $89 \le p_n < H$** — and therefore, by Theorem 3, **FFM holds at every such $n$**.

**Proof.** Fix $n$ with $89 \le p_n < H$ and let $i$ be the largest index with
$P_i \le p_n$. Any $m < n$ has $p_m < p_n < P_{i+1}$; if $g_m > G_i$ then $m$ would be a
record index with gap exceeding $G_i$ occurring strictly below $P_{i+1}$, contradicting
completeness of the table (the next record after $G_i$ is $G_{i+1}$, first attained at
$P_{i+1}$). Hence $G_{n-1} \le G_i$. Then
$$G_{n-1} \;\le\; G_i \;<\; \underline B(P_i) \;\le\; \underline B(p_n) \;\le\; f_n,$$
which is (C-head) at $n$. Apply Theorem 3. $\blacksquare$

**Machine check** (`verify_pa0.py` CHECK C2, 50 digits, recomputed independently of
`notebooks__2`):

| | |
|---|---|
| records loaded | 85, $P_{85} = 101\,412\,319\,996\,363\,309\,069$ |
| $\underline B$ nondecreasing on a 440-point log grid over $[10, 10^{22}]$ | **True** (grid check — gap **G-PA0-1**) |
| first record from which the test passes and never fails again | **#5**, $P_5 = 89$ |
| records #1–3 | below Axler validity $x \ge 9.25$ |
| record #4 ($P=23$, $G=6$) | **fails** ($\underline B = 4.939$) — the head genuinely needs the exhaustive floor |
| every record $\ge$ #5 | **PASS** |
| tightest margin | $\underline B/G = 1.054246$ at record **#64**, $P = 1\,693\,182\,318\,746\,371$, $G = 1132$ |
| absolute margin there | $\underline B - G = 61.4067$ |

So FFM is established on $[89, H)$ for each completeness height $H$, with the certified sub-ranges inheriting the tier
of their completeness claim: $[89,\,4\times10^{18})$ at **V1**, $[89,\,2^{64})$ at **V0**,
$[89,\,10^{20})$ at **V1-unrefereed**. Indices with $p_n < 89$ are covered by the exact
integer check for $n \le 200$ (`notebooks__0` M6, `notebooks__2` §1, zero disagreements)
— which settles F there outright, hence FFM vacuously.

The tightest margin $1.054246$ is the certified (Axler-lower-bound) form of **CC-26**'s
exact $f_k/g_k = 1.054256$; conservative on the correct side, as it must be.

---

## 8. The premise correction: FFM is *not* load-bearing

`notebooks__0`'s header states:

> "(FFM) is the statement that licenses the entire verification literature. […] Without
> (FFM) those verifications do not cover what they claim. (FFM) is therefore
> load-bearing."

**That is false as stated, and Theorem 7's proof shows why.** Run the same argument
without ever mentioning first failures:

> Fix $n$ with $89 \le p_n < H$ and let $i$ be the largest index with $P_i \le p_n$. By
> completeness, $g_n \le G_i$. By the record check and monotonicity,
> $G_i < \underline B(P_i) \le \underline B(p_n) \le f_n$. Hence $g_n < f_n$, i.e.
> **F at $n$** (Prop. 1). $\square$

No least failure, no maximality, no FFM — the same $\sim 80$ inequalities certify F
directly on the whole range. `notebooks__2` runs exactly this lever and reaches exactly
these ranges. So:

* FFM is **sufficient** to license a record-table verification, but **not necessary**.
* The genuinely load-bearing ingredients are (i) *completeness* of the gap table, and
  (ii) a *monotone explicit lower bound* on the barrier. Neither is FFM.
* FFM's own content is therefore located **entirely above the frontier**, where the table
  ends — and there, by §4 and §6, it costs as much as F.

This is a cosmon-ward-style correction *within* the polymer: a downstream leg's stated
motivation for target #0 does not survive contact with the proof. Recorded here rather
than silently patched. `synthesize` and `write-paper` should carry it; the target itself
remains worth stating, because it is what the *literature's prose* appeals to even though
its *mathematics* does not need it.

---

## 9. The obstruction, stated precisely

For the general (all-$n$) case, FFM is **stuck**, and here is exactly where.

**O1 — the certificate is Cramér-hard.** By Thm. 3, FFM at $n$ follows from
$G_{n-1} < f_n$, and (near-)only from it. With $f_n = L_n^2 - L_n - 1 + o(1)$
(**CC-18**, V0), "(C-head) for all $n$" asserts
$$G(x) \;<\; \log^2 x - \log x - 1 + o(1) \quad\text{for the maximal gap } G(x) \text{ below } x,$$
a Cramér-type bound with **leading constant exactly 1 and a negative second-order
correction**. The best unconditional bound is $g \ll x^{0.525}$ (Baker–Harman–Pintz);
RH gives $O(\sqrt x \log x)$. Both are astronomically far from polylogarithmic. Nothing
in analytic number theory delivers a polylogarithmic gap bound with *any* constant
(**CC-23**).

**O2 — the dip route needs more than F.** By Thm. 6 the alternative is to exclude a
near-miss $\sigma_m \le 0.132$, i.e. hypothesis (NM), which implies F. Strengthening the
Dusart bracket does not help: as $D^\*(n) \downarrow$, (NM) only becomes *weaker*, but it
never falls below "$\sigma_m > 0$" $=$ F. The limit of the route as the arithmetic becomes
perfect is exactly F, never less.

**O3 — the vacuous route is F.** Prop. 2(a).

**O4 — no fourth route is visible.** FFM's antecedent is the negation of a
$\Pi_1$ statement; the only structural handles on the hypothetical $n^\*$ are minimality
(used in Thms. 3 and 6) and the criterion of Prop. 1. Both are consumed. We found no
argument that exploits minimality further — e.g. no way to derive a contradiction from
"$g_m \ge g_{n^\*}$ and $\sigma_m$ tiny" using the *local* structure of primes around
$p_m$, because a large gap at $p_m$ constrains nothing about $p_{n^\*}$.

**Therefore the honest classification of target #0**: a **trap target**. It reads as a
cheap lemma; it is a restatement of the main difficulty with an extra quantifier. The
polymer should not spend further compute on an unconditional proof of FFM.

---

## 10. What would move it

Ranked by leverage, not by feasibility:

1. **Extend the record table.** Every new complete decade of the maximal-gap table
   extends Thm. 7 verbatim, at $\sim 1$ inequality per record. This is the only lever
   with a known cost curve — and it is `notebooks__2`'s finding that the frontier is a
   lever, not an enumeration.
2. **Prove monotonicity of $\underline B$ and of $\Upsilon,\Lambda$** (gap G-PA0-1).
   Elementary, and it converts Thms. 5 and 7 from grid-checked to proved. This is the
   single highest-value *tractable* item in this document.
3. **Sharpen $D^\*$ toward the truth.** Measured dips are $10^{-3}$ where the certified
   bound is $10^{-1}$. A two-order improvement (better $\pi(x)$ brackets, or a direct
   treatment of $\Delta_n = f_{n+1} - f_n$ in terms of $g_n$ rather than through $p_n/n$
   brackets) tightens Thm. 6's near-miss threshold. It does **not** break O2 — see above —
   but it makes the "537×" margin into "$10^{5}$×", which is what a paper wants.
4. **A conditional FFM under RH.** Not attempted here; it is `proof-attempt__1`'s target
   (#1, `RH-conditional-bound`). Note that RH's $O(\sqrt x \log x)$ does not reach
   $\log^2 x$, so an RH-conditional (C-head) is *not* available either. That should be
   stated in the synthesis rather than discovered twice.
5. **Abandon FFM as an independent target** and fold it into target #2
   (`unconditional-verified-range`), whose lever proves F on the same range directly (§8).
   Recommended.

---

## 11. Lean anchor (backend: `lean`)

Per **CC-29**, what is formalisable *now* versus what is a research programme:

| statement | Lean feasibility | notes |
|---|---|---|
| Prop. 1 (exact criterion) | **now** | `Real.rpow`, monotonicity of `exp`/`log`. Or via **CC-28**'s integer form `p_{n+1}^n < p_n^{n+1}` to avoid `rpow` entirely. |
| Prop. 2 (F ⟹ FFM) | **now, trivial** | pure logic once FFM is stated; `by tauto` after unfolding the least-failure witness. |
| Thm. 3 ((C-head)) | **now** | ~20 lines: `Nat.lt_irrefl` on the chain, plus Prop. 1. **This is the right first Lean target of the whole polymer** — it is short, it is load-bearing for §7, and it needs no analysis. |
| Thm. 4 ($f_7 > f_8$) | **now** | as an `rpow` inequality it needs `norm_num`-with-`rpow` support; the integer-certificate form (§5) reduces to two `Nat` power comparisons on 30-digit numerals, which `norm_num` closes (kernel `decide` would not — the numerals are far too large for unary/binary reduction). Prefer the integer form. |
| Thm. 5 (dip bound) | **hard** | needs Dusart Props. 6.6/6.7 in Mathlib (absent), plus G-PA0-1. Estimated: not this polymer. |
| Thm. 7 (tabulated range) | **partly** | the *implication* is formalisable; the 85-row table and Axler Cor. 3.5 are external inputs that must enter as axioms or as a `native_decide` data blob. State the dependency explicitly — a Lean file that `sorry`s the completeness claim is honest; one that hides it is not. |

The existing `lean-skeleton/lean/Firoozbakht.lean` should acquire Thm. 3 and Prop. 2 as
its first non-trivial content. Neither needs a single analytic estimate.

---

## 12. Gap ledger — what this document does not establish

| id | statement | status |
|---|---|---|
| **G-PA0-1** | Monotonicity of $\underline B$, $\Upsilon$, $\Lambda$ on their stated ranges | **grid-checked, not proved.** Used in Thms. 5 and 7. Inherited shape from `notebooks__2` G-N2. |
| **G-PA0-2** | Completeness of the maximal-gap table below $H$ | **external, L2/V1–V0.** Not checkable from a b-file; the tier of Thm. 7 is the tier of this claim. |
| **G-PA0-3** | Axler Cor. 3.5 and Dusart Props. 6.6/6.7 | **cited, not re-derived** (V0 via `source-ledger.md#3.5`, `#5`). Carry Axler's corrigendum thresholds (**CC-17**) if any *sufficient-condition* statement is quoted; not needed for the bounds used here. |
| **G-PA0-4** | $\mathcal H = \max_{m<688383} f_m$ and $\min_{m<688383}\sigma_m$ | **computed in float64** by this file's sieve. The margins to the decisions ($0.13$ and $0.196$ vs. a $10^{-13}$ error) are $12$ orders; but they are float computations and are labelled as such. |
| **G-PA0-5** | O4 ("no fourth route") | **a claim about the search, not a theorem.** It records that this leg found none; it is not a proof that none exists. |
| **G-PA0-6** | Firoozbakht's conjecture | **open.** Nothing here bears on it. |

---

## 13. Reproduction

```
cd <run>/proof-attempt__0
python3 verify_pa0.py            # 50-digit mpmath + two sieves; ~7 s; writes verify_pa0.json
```
Inputs: `../notebooks__2/data/{A002386,A005250}.txt` (checksummed in that leg's
`SHA256SUMS`), and `../notebooks__0/scan-1e9.json` for the cross-check only. Output of
record: `verify_pa0.out`; the run is byte-reproducible.

| check | what it decides | verdict |
|---|---|---|
| **C1** | $f_7 > f_8$ in exact integers — Theorem 4 | **PASS** |
| **C2** | the 85-row (C-head) lever — Theorem 7 | **PASS** |
| **C3** | the explicit dip bound $D^\*$ — Theorem 5 | **PASS** |
| **C4** | dip bound vs. shortfall at the record locus — Theorem 6's margin | **PASS** |
| **C5** | *falsification* of the Dusart sandwich against $582\,225$ true $f_k$ | **PASS** |

CHECK C3 additionally sweeps the head: $\min_{m<K_0}\sigma_m = 0.196152$ at $m=2$, which
is what closes Theorem 6's head case (§6.2).

All five checks **PASS**. C5 is the only one that could have falsified a theorem rather
than merely confirmed a number; it did not.

**Final restatement of the verdict.** FFM: **not proved** in general, **not refuted**,
**proved on $[89, H)$** for each height $H$ of table completeness, **shown to be a trap target** —
weaker than F by Prop. 2, yet unreachable except through F or something stronger by
Theorem 6. Firoozbakht's conjecture remains open, and nothing in this document changes
that.
