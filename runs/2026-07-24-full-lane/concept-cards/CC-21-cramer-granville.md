# CC-21 — Cramér's model and Granville's $2e^{-\gamma}$ revision

| | |
|---|---|
| **Kind** | lemma (heuristic — **not** a theorem, in either direction) |
| **Status** | **SOURCED (V0)** for both statements *as statements*. Neither is proved; the deck asserts nothing about their truth. |
| **Depends on** | CC-05 |
| **Feeds** | OB-D4 / OB-E5 (**one node, not two**), FT-3, FT-8; CC-23, CC-24 |
| **Ledger row** | `granville1995harald` (**V0**), **eq. (14)** ¶ — $\max_{p_n\le x}(p_{n+1}-p_n) \sim \log^2 x$. Same source, **unnumbered display following eq. (20)** ¶ — $\max_{p_n\le x}(p_{n+1}-p_n) \gtrsim 2e^{-\gamma}\log^2 x$, *"which contradicts Cramér's conjecture (14)!"*, with $2e^{-\gamma} \approx 1.12292$. Cramér's own words: `cramer1936order` (V2) quoted at length in `granville1995harald` ¶. Underlying theorem: `maier1985primes` (V2). |

## The two statements

**Cramér's conjecture** (as normally stated):
$$\max_{p_n \le x}\,(p_{n+1}-p_n) \;\sim\; \log^2 x,
\qquad\text{i.e.}\qquad \limsup_n R_n = 1 .$$

**Granville's revision** (heuristic consequence of the *corrected* Cramér model, licensed
by Maier's theorem):
$$\max_{p_n \le x}\,(p_{n+1}-p_n) \;\gtrsim\; 2e^{-\gamma}\log^2 x,
\qquad 2e^{-\gamma} \approx 1.12292 > 1 .$$

## The attribution hedge, which is load-bearing

Cramér stated his result **about his random model**, not about the primes:

> *"With a probability $=1$, the relation $\limsup_{n\to\infty}\frac{P_{n+1}-P_n}{(\log P_n)^2}=1$ is satisfied"* — followed by — *"we may take this as a suggestion that, for the particular sequence of ordinary prime numbers $p_n$, some similar relation may hold."*

(`cramer1936order`, quoted verbatim in `granville1995harald`, V0.) **Cramér suggested the
transfer; he did not conjecture it.** Any paper writing "Cramér conjectured" must carry
that hedge — `citation-gate`'s check.

Symmetrically, Granville's $2e^{-\gamma}$ bound is a **heuristic consequence of a
heuristic model**. Granville does not assert it as a theorem, and the deck does not
either.

## Why this is one node, not two

`decompose.md#2` places **OB-D4** ("Cramér-with-constant-$\le 1$", proof branch) and
**OB-E5** ("$\limsup R_n > 1$", refutation branch) in different subtrees. They are
**the same question** — the value of $\limsup R_n$ relative to 1 — appearing twice under
two names, and therefore budgeted nowhere as a single node (`outcomes.md#A9`). Merge
them. The merged node has three outcomes:

| $\limsup R_n$ | consequence for Firoozbakht |
|---|---|
| $> 1$ | **refuted** (via **CC-16**: F forces $g_k < \ell_k - 1$, so $R_k < 1 - 1/\log p_k$ eventually) |
| $= 1$ (Cramér) | **undecided** — a $\limsup$ of 1 does not give $R_k < 1 - 1/\log p_k$ for *every* $k$ |
| $< 1$ | consistent with F, still not a proof (needs every $k$, with explicit constants) |

So even *granting* Cramér's conjecture outright, the proof branch does not close. That is
the sharpest single sentence available about OB-D4, and it is why **CC-23** is a
categorical barrier rather than a quantitative gap.

## What the heuristics do and do not license

**Licensed** (ledger-backed): Granville's revised model predicts extreme gaps above
Cramér's constant-1 scale, which is inconsistent with Firoozbakht.

**Not licensed** (ledger gap **G2**): *"the community's heuristic expectation is that F is
false"*. `source-ledger.md#4` records this as **partially sourced** — the specific
heuristic is V0, but no survey of community expectation exists. `decompose.md#8`-6 states
it untiered and follows it with *"therefore"*. Per `outcomes.md#A9`: tag it **[L3]**,
restate as the specific heuristic, or drop it — and note that `decompose.md#3.6`'s budget
allocation is **invariant** under flipping the expectation, while `#2.4`'s prohibition and
debt #11's mandate are **not**. A prior that routes *instructions* rather than budget
rows is the worse of the two, because a table row is visible and re-rankable while an
obeyed prohibition leaves no trace.

## FT-8, the two-sided test

Neither `decompose.md` nor its tests can come out **for** Firoozbakht. `outcomes.md#A10`
proposes the missing calibration: compute the Cramér/Granville-predicted count of
Firoozbakht counterexamples below the verified frontier and compare with the observed
zero (naive Cramér predicts $\sim 11$; there are none). Not decisive — naive independence
is a poor guide in the extreme-gap regime — but it is the only proposed test that can
come out against the strategic conclusion. **Pre-register the numerical outcome that
would demote "the refutation branch is the live branch" before `notebooks` runs**, or the
test is post-hoc.

## Traps

- $2e^{-\gamma}$ is $\approx 1.12292$, not $1.229$ or $0.5615$. It appears in Granville's
  Mertens/sieve discussion; the display asserting the $\log^2$ lower bound is
  **unnumbered**, immediately after eq. (20).
- Cramér 1920 (the RH gap bound, **CC-23**) and Cramér 1936 (the model) are different
  papers. `decompose.md#3` writes "Cramér 1920/21"; the correct single citation is 1920,
  and there is a companion in Proc. Camb. Phil. Soc. **20** (1920), 272–280. Disambiguate
  before the paper ships (`source-ledger.md#3.6`).
- A heuristic that *predicts* counterexamples is not evidence that any exist. FT-3
  requires a **proof** that $\limsup R_n > 1$, and **CC-24** says why that is unreachable.
