# findings-2 — the unconditional verified range

**Leg**: `notebooks__2` (target #2, *unconditional-verified-range*) of the
`math-attack` polymer
**Artifacts**: `notebook-2.py` (source), `notebook-2.ipynb` (executed, outputs
embedded), `results-2.json` (machine-readable), `run.log`, `data/` (+ checksums)
**Date**: 2026-07-24

> **Posture.** Firoozbakht's conjecture is open. Nothing below assumes it.
> Computation corroborates or refutes; it never proves. Every number tagged
> **[COMPUTED]** is produced by `notebook-2` and reproducible from this
> directory alone; every number tagged **[INHERITED]** comes from `data/` or
> `source-ledger.md` and is *not* verified here unless stated.

---

## 0. Headline

**The phrase "verified up to $4\times10^{18}$" does not describe an
enumeration.** Nobody has evaluated the Firoozbakht criterion at $10^{17}$
consecutive primes, and nobody will. The published verified range is produced by
a **lever**:

$$\underbrace{\text{a complete table of }\sim80\text{ maximal prime gaps}}_{\text{[INHERITED], completeness unverifiable from the data}}
\;\times\;
\underbrace{\text{an explicit } \pi(x) \text{ upper bound}}_{\text{Axler Cor. 3.5, V0}}
\;\Longrightarrow\;
\sim80 \text{ inequalities}$$

`notebook-2` reconstructs that lever, runs it, and measures it:
**80 inequalities decide $\approx 2.2\times10^{18}$ primes — $2.7\times10^{16}$
primes per inequality** [COMPUTED, §11]. That ratio *is* the content of the
phrase. Any downstream leg that writes "verified to $4\times10^{18}$" without
naming the completeness hypothesis it rests on is over-claiming.

Second headline, for the polymer's budget: one of the two **pre-registered**
demotion criteria for "the refutation branch is the live branch" **fired**
(§6 below). The other did not. The honest verdict is a *partial* demotion, stated in
§8.

**No counterexample was found at any scale examined.** The conjecture is neither
proven nor refuted by this leg.

---

## 1. What the computation certifies, and on what

| range | certified by | tier | cost |
|---|---|---|---|
| $[2,\,1223]$ | exact integer comparison $p_{n+1}^n < p_n^{n+1}$ | **L0** — no arithmetic model at all | $n\le200$ |
| $[2,\,10^9)$ | exhaustive sieve + slack with a **runtime-certified** error bound | **L0** — produced here, inherits nothing | 50 847 533 pairs, 9 s |
| $[89,\,4\times10^{18})$ | lever (gap table + Axler Cor. 3.5) | **L2** — completeness from `kourbatov2015verification` (V1) | 71 inequalities |
| $[89,\,2^{64})$ | same lever, longer table | **L2** — completeness from `visser2019verifying` (V0) | 76 inequalities |
| $[89,\,10^{20})$ | same lever, full b-file | **L2** — completeness from `primegaplist_faq` (V1, community, unrefereed) | 80 inequalities |
| $[89,\,1.857\times10^{19})$ | **second, independent lever**: Visser SC2 + tabulated $\pi(P_i)$ | **L2** — needs no Axler bound | 79 inequalities |
| beyond $10^{20}$ | nothing | — | open |

All [COMPUTED, §11]. Two things are worth separating carefully:

* The exhaustive floor $[2,10^9)$ is **20× further than the upstream `decompose`
  leg reached** ($5\times10^7$) and is the only part of the range this project
  owns outright.
* Everything above it is *conditional on a completeness claim that no b-file can
  establish* — see gap **G-N1**.

### 1.1 The lever, stated

**Lemma (interval covering).** Let the maximal-gap table be complete below $H$:
records $(P_i, G_i)$ with $P_i$ the prime preceding the $i$-th maximal gap, and
$P_{k+1}:=H$. For $P_i \le p_n < P_{i+1}$ we have $g_n \le G_i$ — a larger gap
would be an unrecorded new record below $P_{i+1}$.

**Barrier lower bound.** $B_n = p(p^{1/n}-1)$, $n=\pi(p)$, is *decreasing* in
$n$, so an **upper** bound on $\pi$ gives a lower bound on $B$. From
`axler2014newbounds` Cor. 3.5 (V0):
$\pi(x) < x/(\log x - 1 - 1/\log x - 3.83/\log^2x) =: \pi^+(x)$ for $x\ge9.25$,
hence $B_n \ge \underline B(p) := p\,(p^{1/\pi^+(p)}-1)$.

**Consequence.** If $\underline B$ is non-decreasing, then $G_i < \underline B(P_i)$
for every $i$ implies Firoozbakht on $[P_1', H)$, where $P_1'$ is the first
record where the check passes.

**Result** [COMPUTED, §6]: the check passes from record **#5** ($P=89$) onward
and at **every** record thereafter, through $P_{85}=1.014\times10^{20}$. Records
1–4 are covered by the exhaustive floor. The tightest inequality in the whole
range has margin $\underline B/G = 1.0542$.

### 1.2 The second lever

`visser2019verifying` Sufficient condition 2 (V0): $g_n < (\ln(n\ln n)-1)\ln(n\ln n)$
for $n>4$ suffices for Nicholson, hence for Firoozbakht. It is **index-based** —
no $\pi(x)$ inversion, no Axler bound — so it rests on a *disjoint* auxiliary
input (the tabulated $\pi(P_i)$ of A005669). It certifies from the same record
#5 up to $P_{82} = 1.857\times10^{19}$, tightest margin $1.0506$
[COMPUTED, §7]. **Two levers on disjoint auxiliary inputs agree.** That is the
strongest structural statement this leg can make about the verified range.

---

## 2. What the computation refutes

Three claims died on contact with the arithmetic. All three are upstream claims
of this polymer, not literature claims.

### 2.1 `decompose.md#5.4`'s float-rigour argument — **refuted, and quantified**

`outcomes.md#A1` asserted the upstream error analysis was wrong. It is, and the
notebook now *measures* it rather than arguing it [COMPUTED, §2]:

| formula | error model | measured/bound, worst case |
|---|---|---|
| naive `log(q/p)` | $\approx n\,u$ — **linear in $n$** | **0.982** (the bound is tight, not slack) |
| `log1p(g/p)` | $\le 6u(\log p + 1)$ — **independent of $n$** | 0.258 |

The consequence that matters for target #2: **the naive formula stops being able
to decide the criterion at record #76, $p = 5.73\times10^{18}$** — where its own
error ($1.5\times10^{1}$) exceeds the slack it is measuring ($7.9$). That height
is **below the published $2^{64}$ frontier.** A naive double-precision
verification of the frontier is meaningless *exactly at the frontier*, and
silently so.

With `log1p`, the error at the last tabulated record is $3.0\times10^{-14}$
against a slack of $8.10$ — **14.4 orders of headroom**, sound past $10^{20}$.
The fix is one function call.

Also confirmed exactly: $\min_{n>200} S_n = \mathbf{1.7008}$ at $n=217$
($p=1327$, $g=34$) — `outcomes.md#A1`'s corrected value, against
`decompose.md#5.4`'s stated "$>3.5$".

### 2.2 "Naive Cramér is the model that makes the refutation branch live" — **weakened at the frontier**

FT-8, the two-sided test `outcomes.md#A10` demanded. Under the naive
independence model ($\lambda=1$), the expected number of Firoozbakht
counterexamples below $2^{64}$ is $\mathbf{6.72}$ [COMPUTED, §9]; the observed
number is $\mathbf{0}$. Calibrating: the model must be **cooled to
$\lambda \approx 0.834$** to predict even one. Granville's $\lambda = 2e^{-\gamma}
= 1.1229$ predicts $\mathbf{90}$ below $2^{64}$ — over-predicting by a further
factor of 13.

This does *not* refute Granville's heuristic, which is a statement about the
$\limsup$ of maximal gaps and not about counts below a finite height. It does
mean the crude independence model that supplies the refutation branch's
*quantitative* motivation is falsified at the frontier by a factor of $\ge5$ —
which is exactly demotion criterion **D1**, pre-registered before the estimator
in §10 ran.

### 2.3 A live estimator bug, produced by ledger gap G6

`source-ledger.md` gap **G6** records that $R_{\mathrm{csg}} = g/\log^2p$ exceeds
$1$ at $p = 2, 3, 7$ (values $2.0814$, $1.6571$, $1.0564$), so every "record
$R$" statement carries the restriction $p_1 > 7$. That caveat is not decorative:
the first version of §10's estimator seeded its running maximum at $p=2$, was
pinned at $R = 2.0814$ forever, and returned $\hat C = 2.80$ instead of $1.08$ —
a 2.6× error that looks entirely plausible in isolation. The fix is one guard
(`R_FLOOR_P = 7`, §10). **Recorded because it is a documented instance of a
ledger caveat silently destroying a downstream number**, and because any other
leg computing a record-$R$ statistic will hit it.

---

## 3. What the computation supports (corroboration, not proof)

* **No counterexample below $10^9$**, exhaustively, with certified arithmetic;
  minimum slack over $n>200$ is $1.70$, i.e. $5.5\times10^{13}$ times the error
  bound [COMPUTED, §3].
* **Rows 1–30 of A002386 / A005250 / A005669 reproduced exactly** from this
  notebook's own sieve — every maximal-gap record below $10^9$, including the
  indices $\pi(P_i)$ [COMPUTED, §5]. The inherited table is not wrong where we
  can check it.
* **The decompose headline is confirmed at 60 digits.** `decompose.md#0.2`
  quotes a refutation shortfall of $1.055$ at the record locus. Recomputed with
  a *rigorous lower bound* on the barrier: $\mathbf{1.0542}$; with the tabulated
  exact index $\pi(p)=49\,749\,629\,143\,526$: $\mathbf{1.0543}$ [COMPUTED, §6.2].
  The 5.5% framing survives.
* **The tightest record is the CSG record.** The argmax of $R$ and the argmin of
  $B/g$ both land on record #64, $p = 1\,693\,182\,318\,746\,371$, $g = 1132$
  [COMPUTED, §6.2]. This is a *computed coincidence on this table*, not an
  identity — the two ratios have different denominators and need not co-minimise
  at other heights.
* **The strengthening chain holds and is more sensitive.** Farhadian $\le$
  Nicholson $\le$ Firoozbakht margins at **every** maximal-gap record below
  $10^9$; minima $1.2530 / 1.2580 / 1.2665$ [COMPUTED, §8]. `source-ledger.md`
  debt #6's re-rating from [L3, unused] to V0/live is vindicated: Farhadian is a
  free early-warning line on the same sieve pass — it fails first if the family
  fails.
* **The exhaustive route provably cannot reach the frontier.** Extrapolating
  this notebook's own sieve rate: **of order $10^3$ core-years** — a live
  timing, observed between $8.6\times10^2$ and $5.3\times10^3$ across runs on the
  same machine — and $5\times10^{5}$ TB of memory to reach $4\times10^{18}$
  unsegmented [COMPUTED, §4]. The conclusion is robust across that whole spread;
  no plausible constant factor changes it. The lever is not a
  shortcut anyone took for convenience; it is the only road.

---

## 4. New quantitative result: what the verified range already excludes

`outcomes.md#A8` replaced the ill-posed "error bars on $\max R$" with a model
fit. Run [COMPUTED, §10]:

$$\max_{p\le x} R \;\approx\; C\Bigl(1-\frac{2\log\log x}{\log x}\Bigr),\qquad
\hat C = \mathbf{1.0788}\ \ (p_{\min}=10^9,\ n=55)$$

stable across cutoffs ($1.0886$ at $10^3$ → $1.1152$ at $10^{15}$), dispersion
interval $[0.999,\,1.159]$. **The interval is descriptive dispersion, not a
confidence interval** — the points are a running maximum and hence positively
dependent by construction. Implied crossover heights against the Firoozbakht
threshold $1-1/\log x$:

| $C$ | first crossing |
|---|---|
| $\hat C = 1.0788$ | $x \sim 10^{51}$ |
| upper dispersion $1.1586$ | $x \sim 10^{22}$ |
| Granville $2e^{-\gamma} = 1.1229$ | $x \sim 10^{30}$ |
| Cramér $C = 1$ | never |

And the result that is genuinely new here: **the verified range itself excludes
$C \ge 1.1736$** in this model — if $C$ were that large, the model's crossover
would fall below $10^{20}$ and a counterexample should already have been seen.
Granville's $2e^{-\gamma} = 1.1229$ sits **4.3% below that exclusion
threshold**. The community's heuristic constant is not excluded by existing
computation, but it is close to the edge — and one more decade of exhaustive
gap analysis would put it in range.

That is the actionable number for anyone budgeting a refutation search:
**the search height implied by the fitted constant is $10^{30}$–$10^{51}$, not
$10^{21}$.** `outcomes.md#A8` is right that $R$ is a logarithmic coordinate on
height: the "5.5% shortfall" of `decompose.md#0.2` is 10–30 orders of magnitude
in search height.

---

## 5. Gaps — every place this leg's claims stop

Flagged, not fixed. In descending order of how much weight they carry.

| id | gap | consequence |
|---|---|---|
| **G-N1** | **Completeness of the maximal-gap table cannot be verified from a b-file.** A b-file lists records; it cannot show none is missing. Completeness is asserted by `primegaplist_faq` (V1, community, unrefereed) at $10^{20}$, `visser2019verifying` (V0) at $2^{64}$, `kourbatov2015verification` (V1) at $4\times10^{18}$. | **This is the single load-bearing inherited input of the entire verified range.** Everything above $10^9$ is conditional on it. Rows 1–30 are recomputed here; rows 31–85 are not. |
| **G-N2** | **$\underline B$ monotonicity is checked, not proved.** The interval-covering step needs $\underline B$ non-decreasing on each $[P_i, P_{i+1})$. Verified on a 400-point logarithmic grid over $[10, 10^{21}]$. | A one-line lemma in the paper, or a `Lean` obligation. Low risk (the function is $\log^2p - \log p - 1 - O(1/\log p)$), but it is currently a numerical check standing in for a proof. |
| **G-N3** | **Axler Cor. 3.5 is used as stated in `source-ledger.md` (V0), not re-derived**, including its validity range $x \ge 9.25$ and the corrected $x \ge 2\,634\,800\,823$ for the $1.17$ variant. | Standard inheritance; flagged so the `citation-gate` can check the locator rather than the arithmetic. |
| **G-N4** | **§9's Cramér model is crude.** Exponential gaps, independence, $B \approx L^2-L-1$. The $\lambda$-calibration is a statement about the model, not about the primes. | D1's demotion is of *the model's quantitative authority*, not of the possibility of counterexamples. An $\varepsilon$ of a divergent integral is still divergent. |
| **G-N5** | **§10's interval is dispersion, not inference.** Running-max points are dependent; no valid CI is available without a model of the dependence. | Stated in the notebook output itself. Do not let it become "95% CI" downstream. |
| **G-N6** | **60-digit `mpmath` is not interval arithmetic.** §6/§7 margins exceed the arithmetic error by $>10^{40}$, so nothing is load-bearing — but no formal certificate is produced. | If the paper wants a machine-checkable claim, §6 must be re-run in interval or exact rational arithmetic. |
| **G-N7** | **The §2 error bound is derived by hand and measured, not machine-checked.** Measured tight (ratio 0.982 for the naive form) on 64 record loci with $p<2^{53}$; not measured above $2^{53}$, where the notebook uses `mpmath` instead. | The bound is used only for $p < 10^9$ verdicts, where headroom is $10^{13}$. |
| **G-N8** | **This leg did not attempt a counterexample *search*.** Consistent with `frame.md`'s allocation ("`notebooks` at measurement, not search"). §10 says where a search would have to look: $10^{30}$–$10^{51}$. | No refutation-branch evidence either way beyond §9/§10. |

---

## 6. Pre-registration and its verdicts

`outcomes.md#A10` required that the demotion criteria be fixed **before** the
estimator ran, so the test could not become post-hoc. They are written in the
notebook's §9 markdown cell, which precedes §10's code cell.

| id | criterion, as pre-registered | outcome |
|---|---|---|
| **D1** | naive ($\lambda=1$) model predicts $\ge 5$ counterexamples below the verified frontier while $0$ are observed | **TRIGGERED** — predicts $6.72$, observed $0$ |
| **D2** | the §10 estimator returns $\hat C$ with an upper bound $< 1$ | **NOT triggered** — upper dispersion $1.1586 > 1$ |

Both are honoured as written. Neither is evidence *for* the conjecture.

---

## 7. Corrections implemented, upstream

| id | source | status |
|---|---|---|
| **A1** | `outcomes.md` | **implemented and empirically confirmed** — `log1p`, per-prime runtime error bound (no hardcoded margin), $\min_{n>200}S_n$ printed, $p<2^{53}$ precondition stated and asserted, guard deliberately tripped in §3.4 to demonstrate it fires |
| **A8** | `outcomes.md` | **implemented** — the prescribed estimator, $\hat C$ with residual dispersion, crossover heights, plus the new exclusion bound $C \ge 1.1736$ |
| **A10** | `outcomes.md` | **implemented** — FT-8 computed, demotion criteria pre-registered and adjudicated |
| **debt #6** | `source-ledger.md#3.8` | **implemented** — Nicholson/Farhadian as a second trend line and Visser SC2 as an independent lever |
| **G6** | `source-ledger.md#4` | **implemented as a code guard**, after it produced a live 2.6× estimator error (§2.3) |

---

## 8. Handoff — what other legs should do with this

1. **`write-paper`** — never write "verified up to $4\times10^{18}$" bare. The
   supported sentence is: *"verified for all $p<4\times10^{18}$ by an
   $\approx70$-inequality argument, conditional on the completeness of the
   first-occurrence gap table below that height."* The compression factor
   ($2.7\times10^{16}$ primes per inequality) belongs in the paper; it is the
   most surprising true thing in this leg.
2. **`lean-skeleton` / OB-B** — **retarget the finite-verification obligation.**
   `decompose.md#2.2` costs OB-B as "verify $n \le N_0$", which even at
   $N_0=10^5$ is a `Nat.nth` nightmare and reaches nowhere near the frontier.
   The lever says the right Lean target is the **interval-covering lemma plus
   $\approx80$ numeric inequalities**, with table completeness as an explicit
   hypothesis (an axiom the statement *names*, not one it hides). That is a
   tractable formalization of the *actual* published result, and it changes
   OB-B from the tree's main risk item to one of its cheaper nodes. Visser SC2
   (index-based, no $\pi$ inversion) is the cheaper of the two levers to
   formalize — `outcomes.md#A6`'s OB-F6b should point at it.
3. **`proof-attempt__*` / `red-team-corpus`** — G-N2 ($\underline B$ monotone)
   is a genuine, small, open obligation created by this leg. Close it or flag it.
4. **`skeptic`** — §2.3 is a worked example of a ledger caveat silently
   destroying a downstream number. It is worth one line in the corpus: *a
   caveat that is not encoded as a guard is a caveat that will be violated.*
5. **Budget** — D1 fired, D2 did not. The defensible reallocation is *partial*:
   the refutation branch keeps its heuristic motivation (Granville is not
   excluded; $\limsup R$ is genuinely unknown) but loses its quantitative one
   (the model that produced "5.5% away" over-predicts observed counterexamples
   by 6.7× at the frontier, and the fitted crossover is $10^{30}$–$10^{51}$, not
   $10^{21}$). **Search is not affordable at any of those heights.** That
   argues for `frame.md`'s barrier-mapping allocation on *arithmetic* grounds
   rather than sociological ones — which is what `outcomes.md#A9` asked for.

---

## 9. Reproduction and build/test status

```bash
cd notebooks__2
python3 notebook-2.py                       # ~15 s, default N_SIEVE = 10^9
FIROOZBAKHT_NSIEVE=10000000 python3 notebook-2.py   # ~5 s, fast path
python3 build_ipynb.py                      # rebuild + execute notebook-2.ipynb
python3 verify-findings.py                  # cross-check THIS NOTE against the run
```

Honest status, as run on 2026-07-24:

| gate | command | result |
|---|---|---|
| script executes | `python3 notebook-2.py` | **exit 0**, ~15 s, all internal assertions passed |
| notebook executes | `python3 build_ipynb.py` | **37 cells (20 code), 0 error outputs** |
| note matches the run | `python3 verify-findings.py` | **exit 0** — 37 numeric + 20 literal claims cross-checked, plus 9 structural assertions |
| lint | `ruff check notebook-2.py build_ipynb.py verify-findings.py` | **All checks passed** |
| format | `ruff format --check` | **not run as a gate** — would reflow the aligned print tables; `.cosmon/config.toml` defines no format gate for this project |
| self-tests inside the run | 12 `assert`s: b-file index integrity, exact-integer F2 for $n\le200$, both error bounds valid on every measured point, the $p<2^{53}$ precondition, no counterexample below $10^9$ (with escalation to exact arithmetic), guard fires when inflated (§3.4), table rows 1–30 match OEIS exactly, $\underline B$ monotone on the grid, all 81 lever inequalities pass, $\pi^+\ge\pi$ and $\underline B\le B$ against the true index, interval covering exact on every self-computed record | **all passed** |

Dependencies: Python 3.10, `numpy`, `mpmath`, `nbformat`, `nbclient`. No
network access at run time — `data/` is committed with SHA-256 checksums.

### 9.1 What the verification pass changed

Two things were added *after* the first draft of this note, and both found
something:

* **§6.1b, lever validation against ground truth.** The lever has two
  load-bearing directions — $\pi^+ \ge \pi$ (so that $\underline B \le B$) and
  the interval-covering property — and both are silently wrong-way-round if
  mis-derived. On $[2,10^9)$ we hold the true index and every gap, so both are
  testable. Both **pass**: no violation on 385 sampled primes, and
  $\max\{g_n : P_i \le p_n < P_{i+1}\} = G_i$ **exactly** on all 30
  self-computed records. On the overlap $[89, 10^9)$ the lever's 26 inequalities
  and the exhaustive check agree.
* **`verify-findings.py`, a prose-vs-computation gate.** Prose drifts from the
  run that produced it. On first execution it **failed with 5 findings**: three
  chain minima in §3 still quoted the $10^7$ fast-path run rather than the
  $10^9$ run ($1.2611/1.2912/1.3150$ → $1.2530/1.2580/1.2665$), and the §3
  core-year figure was quoted as a point value when it is a live timing. The
  timing then failed the gate a **second** time, at a value ($8.6\times10^2$)
  outside the first band ($10^3$–$10^4$) written to accommodate it — which is
  the correct behaviour: a claim stated more precisely than the measurement
  supports will keep failing until it is stated honestly. It is now an
  order-of-magnitude claim with the observed spread quoted, and the gate checks
  the order of magnitude. Recorded rather than quietly fixed, because a note that has never failed
  its own gate is a note whose gate has not been tested.

---

## 10. One-sentence verdict

The unconditional verified range of Firoozbakht's conjecture is not a
computation but a **lever**: eighty inequalities and one unverifiable
completeness claim, standing where $10^{18}$ evaluations appear to stand — and
this leg reproduces the lever, extends the part that owes nothing to anyone from
$5\times10^{7}$ to $10^{9}$, locates the height at which the usual float
shortcut goes silently blind ($5.7\times10^{18}$, *below* the published
frontier), and finds no counterexample anywhere.
