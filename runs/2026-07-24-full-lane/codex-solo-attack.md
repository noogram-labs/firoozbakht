# A solo attack on Firoozbakht's conjecture

**Author:** Noogram  
**Date:** 2026-07-24

## Verdict

I do **not** prove or refute Firoozbakht's conjecture. I obtain an exact
reformulation, derive the asymptotic size of the required prime-gap bound, and
identify precisely why standard unconditional estimates, the Riemann
hypothesis, and the usual form of Cramer's conjecture do not close the problem.
The reformulation and asymptotic analysis are standard (and substantially
overlap Kourbatov's work), not a claimed novel result.

The conjecture remains plausible from all verified data cited below, but there
is a serious heuristic reason to expect eventual counterexamples: its leading
constant for extreme gaps is effectively 1, while refined random models allow
or predict gaps above that scale. That heuristic is not a refutation.

## 1. Exact reduction to one prime-gap inequality

Write

\[
  g_n=p_{n+1}-p_n,\qquad L_n=\log p_n.
\]

Taking logarithms (all quantities are positive), Firoozbakht's inequality is
equivalent to

\[
 \frac{\log p_{n+1}}{n+1}<\frac{\log p_n}{n}
 \iff n\log\!\left(1+\frac{g_n}{p_n}\right)<L_n.
\]

Exponentiating gives the exact criterion

\[
 \boxed{\quad g_n<B_n:=p_n\left(\exp(L_n/n)-1\right)
       =p_n\left(p_n^{1/n}-1\right).\quad}                 \tag{1}
\]

Thus there is no useful slack hidden in the root formulation: the whole
problem is to rule out every prime gap exceeding the explicit, index-dependent
barrier \(B_n\).

For orientation, the first cases really do work. For example,
\(3^{1/2}<2\), \(5^{1/3}<3^{1/2}\), and so on. More importantly, the cited
computational work verifies enormously more than these toy cases; see Section
5.

## 2. What size of gap must be controlled?

The prime number theorem has a full asymptotic expansion

\[
 \pi(x)=\frac{x}{\log x}
 \left(1+\frac1{\log x}+\frac2{\log^2x}
       +\frac6{\log^3x}+\frac{24}{\log^4x}
       +O(\log^{-5}x)\right).
\]

At \(x=p_n\), inversion of the series gives

\[
 \frac{p_n}{n}
 =L_n-1-\frac1{L_n}-\frac3{L_n^2}
     -\frac{13}{L_n^3}+O(L_n^{-4}).                       \tag{2}
\]

Since \(L_n/n=O(L_n^2/p_n)\), expanding the exponential in (1) gives

\[
 B_n=\frac{p_nL_n}{n}+O\!\left(\frac{L_n^4}{p_n}\right).
\]

Combining this with (2),

\[
 \boxed{\quad
 B_n=L_n^2-L_n-1-\frac3{L_n}-\frac{13}{L_n^2}
          +O(L_n^{-3}).\quad}                             \tag{3}
\]

The error from the exponential is far smaller than the displayed inverse-log
terms. Consequently, an eventual proof needs essentially the sharp local bound

\[
 g_n<\log^2p_n-\log p_n-1-o(1),                           \tag{4}
\]

with the inequality holding for **every** sufficiently large consecutive-prime
pair, followed by a finite verification. Conversely, any gap with
\(g_n\ge B_n\) is an exact counterexample; in particular, Kourbatov proves that
for \(n>9\), a gap at least
\(\log^2p_n-\log p_n-1\) would refute the conjecture.

This is the useful partial result of the attack: it locates the target not just
at the Cramer scale \(\log^2p\), but at its first several lower-order terms.
It is a derivation of known mathematics, not a new theorem.

## 3. Why familiar routes stop

### Unconditional prime-gap bounds

Known unconditional bounds on the next-prime gap are of power size
\(g_n\ll p_n^\theta\) for some \(\theta<1\). Any positive power of \(p_n\) is
asymptotically much larger than \(\log^2p_n\), so such results cannot establish
(1). Results on bounded gaps go in the opposite direction: they prove that
some gaps are small infinitely often, whereas (1) requires that every gap be
small enough.

The prime number theorem itself controls averages and the global counting
function. It permits fluctuations far larger than (4), so substituting
\(p_n\sim n\log n\) into the conjecture is only heuristic. A proof cannot
differentiate or subtract a coarse asymptotic for \(p_n\) at consecutive
indices and expect the error to disappear; the error is much larger than the
quantity being bounded.

### The Riemann hypothesis

The classical consequence of RH for primes in short intervals yields gaps on
roughly the \(\sqrt p\,\log p\) scale (up to the precise version and constants).
That is still enormously larger than \(\log^2p\). Therefore the standard RH
error term does not prove Firoozbakht. This does **not** prove that RH could not
imply it through some deeper argument; it only shows that the familiar direct
route is inadequate.

### Cramer's conjecture

The statement \(g_n=O(\log^2p_n)\) is also insufficient: its unspecified
constant may exceed 1, while (3) demands leading constant 1 and then a negative
\(\log p_n\) correction. Even an eventual estimate
\(g_n\le\log^2p_n\) does not by itself supply that correction. Kourbatov gives
explicit comparisons: Firoozbakht implies
\(g_n<\log^2p_n-\log p_n-1\) for \(n>9\), while the uniformly stronger
assumption

\[
 g_n<\log^2p_n-\log p_n-1.17\qquad(n>9)
\]

is sufficient for Firoozbakht.

### Trying to use monotonicity or products

One might hope that global information such as estimates for \(p_n\), products
of primes, or Chebyshev functions forces \(\log p_n/n\) to decrease. But the
difference between consecutive terms is exactly

\[
 \frac{\log p_n}{n}-\frac{\log p_{n+1}}{n+1}
 =\frac{L_n-n\log(1+g_n/p_n)}{n(n+1)},
\]

whose sign is once again precisely (1). Averaging or summing only hides the
single exceptional gap that could reverse the sign. I see no valid convexity
or telescoping argument that restores this lost local control.

## 4. Heuristic stress test: the conjecture may be false

Here is a deliberately non-rigorous calculation. Model a gap near \(x\) as an
exponential random variable of mean \(L=\log x\). The Firoozbakht threshold is
approximately \(L^2-L\), so for one modeled gap

\[
 \Pr(g>L^2-L)\approx \exp(-(L^2-L)/L)=e/x.
\]

There are about \(dx/L\) prime gaps with left endpoint in \([x,x+dx]\).
The expected number of violations up to \(X\) is therefore of order

\[
 e\int^X\frac{dx}{x\log x}=e\log\log X+O(1),
\]

which diverges, although painfully slowly. This naive independence model thus
suggests infinitely many eventual failures rather than universal truth.

Refined Cramer/Granville-style heuristics are at least as hostile to a universal
leading constant 1: arithmetic correlations suggest unusually large maximal
gaps with a leading constant above 1 (the often quoted Granville factor is
\(2e^{-\gamma}>1\)). If that limsup prediction is correct, (3) must fail
infinitely often.

None of this constructs a prime gap, proves independence, or controls a
limsup. It is evidence about where to search, not a refutation. It also explains
why extensive finite verification can coexist with eventual failure: the
predicted events are extremely sparse.

## 5. What the literature establishes

My literature check found the following relevant results. I have not conducted
an exhaustive citation review, so this is a scoped account rather than a claim
to completeness.

- Alexei Kourbatov, [*Verification of the Firoozbakht conjecture for primes up
  to four quintillion*](https://arxiv.org/abs/1503.01744) (2015), verified the
  conjecture for all \(p_n<4\times10^{18}\), using first-occurrence prime-gap
  data together with explicit bounds for \(\pi(x)\).

- Kourbatov, [*Upper bounds for prime gaps related to Firoozbakht's
  conjecture*](https://arxiv.org/abs/1506.03042) (2015; later revisions), proved
  the \(-1\) necessary consequence and the \(-1.17\) sufficient condition
  quoted above, and derived asymptotics for the exact barrier. My Sections 1--3
  essentially re-derive this viewpoint.

- Matt Visser, [*Verifying the Firoozbakht, Nicholson, and Farhadian
  conjectures up to the 81st maximal prime
  gap*](https://arxiv.org/abs/1904.00499) (2019), states an unconditional
  verification below the unknown location of the 81st maximal gap, certainly
  for all \(p<2^{64}\). The paper treats Nicholson's and Farhadian's proposed
  inequalities as progressively stronger conjectures; it does not provide a
  general proof of any of them.

- Zhi-Wei Sun's work on inequalities involving primes is cited by Kourbatov as
  giving the related, weaker candidate bound
  \(g_n<\log^2p_n-\log p_n+1\); one cited source is [*On a sequence involving
  sums of primes*](https://arxiv.org/abs/1207.7059). I do not use a Sun
  conjecture as an unproved premise here.

- Reza Farhadian's name is associated with the stronger gap conjecture
  displayed and tested in Visser's paper. I am aware of claims and informal
  manuscripts around Firoozbakht on the web, but I found no generally accepted
  proof or counterexample and do not treat any proof claim as established.

## 6. Honest endpoint

The attack reduces the problem to the exact barrier (1), sharpens its scale to
(3), and shows why the most tempting standard hypotheses do not reach it. It
does not control all future maximal prime gaps, which is the essential missing
step. Nor does it exhibit a single violating gap. Accordingly:

> **Firoozbakht's conjecture is neither proven nor refuted here.**

A credible route to a proof would need a breakthrough giving every-gap control
at leading constant 1 with lower-order precision beyond ordinary Cramer. A
credible route to a refutation would be either a rigorously computed prime gap
exceeding the exact \(B_n\), with \(n=\pi(p_n)\) certified, or a theorem forcing
maximal gaps above \((1+\varepsilon)\log^2x\) infinitely often. Neither is
currently in hand in this standalone attempt.
