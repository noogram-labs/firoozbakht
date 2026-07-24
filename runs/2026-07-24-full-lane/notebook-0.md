# Findings — notebook 0, target #0 `first-failure-maximality`

**Leg** `notebooks__0` (node 6 of the `math-attack` polymer) ·
**Molecule** node · formula `task-work-build` · crew role `coder`
**Run dir** node/notebooks__0/`
**Upstream** `decompose/decompose.md`, `frame-deliberation/outcomes.md`,
`source-ledger/source-ledger.md`, `concept-cards/` (CC-01 … CC-31)
**External attribution** Noogram · **Date** 2026-07-24

> **Firoozbakht's conjecture is open.** Nothing here proves or refutes it. Computation
> corroborates or refutes; it is **never** the proof. Every claim below is either
> proved in this note (**L0**), verified by the notebook over a stated range
> (**[C]** + the range), or explicitly tagged heuristic (**L3**).

---

## 0. The target, and why it is load-bearing

$$
\textbf{(FFM)}\quad
\text{if } n^\star \text{ is the \emph{least} index at which F fails, then }
g_{n^\star} > g_m \ \text{ for every } m < n^\star .
$$

*The first failure, if there is one, sits at a maximal (record) prime gap.*

(FFM) is not decoration. `kourbatov2015verification` verifies F to $4\times10^{18}$
and `visser2019verifying` to $2^{64}$ by walking a **first-occurrence / maximal-gap
table** — on the order of 80 rows — rather than the $\approx4.2\times10^{17}$ primes
below $2^{64}$. That substitution is licensed by (FFM) and by nothing else. If (FFM)
fails, **CC-25**'s verification frontier does not cover what it claims, and the
frontier is the single fact everything else in this polymer is measured against.

Neither `decompose.md` nor the concept-card deck states (FFM) or discharges it. It
enters the corpus only implicitly, through **CC-25**'s note that Kourbatov's
verification "is not a bare sieve: it uses a first-occurrence prime-gap table". This
leg makes the obligation explicit and attacks it.

---

## 1. Headline

| # | finding | tier |
|---|---|---|
| **F1** | (FFM) reduces **exactly** to a statement about the barrier sequence $f_n$: a violation forces $f_m > f_{n^\star}$ for some $m<n^\star$. Two certificates follow, **(C-mono)** and **(C-head)**. | **L0**, §2 |
| **F2** | **(C-mono) is REFUTED.** The barrier $f_n$ *decreases* at **57.88 %** of steps and lies below its own running maximum at **87.88 %** of indices. Any proof of (FFM) routed through monotonicity of $f_n$ — or of $\ell_n$ as a stand-in for it *along the index* — is wrong. | **[C]**, $n\le5.08\times10^{7}$ |
| **F3** | **(FFM) is CERTIFIED, not assumed,** for every $n \le 50\,847\,533$ ($p<10^{9}$) via **(C-head)**: zero exposures, zero float-guard trips, min headroom $H_n = 1.196$ at $n=2$. This is **20×** the upstream deck's range. | **[C]** |
| **F4** | **The dip is sub-unit.** $\max_n D_n = 0.548660$ at $n=214$, and $D_n<1$ at **every** index in range, decaying by a factor 2–6 per index decade above $n=10^{2}$. Gaps are integers, so a window of width $<1$ admits at most one — this is what actually saves (FFM), and it is an emptiness of density $\approx D_n$, **never a theorem**. | **[C]** |
| **F5** | **(FFM) is coextensive with F, not cheaper than it.** $\max X_n$ and $\max Y_n$ are attained at the *same* record gap one index apart, and their difference shrinks monotonically from $p\sim10^3$ onward — $1.8\times10^{-2}$, $1.5\times10^{-3}$, $1.5\times10^{-4}$, $4.1\times10^{-5}$, $7.8\times10^{-6}$, $3.5\times10^{-7}$. The maximal-gap method is valid **exactly as far as F is verified**, with a vanishing margin. | **[C]** |
| **F6** | Under Cramér, conditional on a first failure existing, $\Pr[\text{it is non-record}]$ falls from $\sim3\times10^{-10}$ at $p\sim10^{6.5}$ to $\sim10^{-13}$ at $p\sim10^{9}$ — small, non-zero, and **decreasing** with height. (FFM) gets *safer* with height; F does not. | **L3** |
| **F7** | A new, sharp, unproved obligation for downstream legs (§10.1): **given F, (FFM) follows from $\sup_{n>m}(f_m-f_n) < f_m - G_m$ for every $m$** — *the barrier never descends, at any later index, by more than the margin with which F holds at $m$*. Measured: $\sup_{n>m}(f_m-f_n) < 0$ at **all 30** records below $10^{9}$ — the barrier never returns to its value at a record — against margins of $50$–$120$ gap units. Elementary in form; unproved; and unlike **CC-23**'s barrier it is a statement about *ordinary* gaps, not extreme ones. | open |

**What this leg does *not* do.** It does not extend the verification frontier
(**CC-25**): $10^{9}$ is **eleven orders below** $2^{64}$. It does not reproduce the
record locus by search — only by 60-digit recomputation of published integers. And it
does not prove (FFM); it reduces it to a sharp statement and verifies that statement
over a stated range.

---

## 2. The reduction  [L0 — proved here, no citation]

By **CC-10** (exact criterion, no side condition, every $n\ge1$):

$$
\text{F at } n \iff g_n < f_n,
\qquad f_n := p_n^{\,1+1/n} - p_n = p_n\big(e^{L_n/n}-1\big),\quad L_n=\log p_n .
$$

Let $n^\star$ be the **least** index at which F fails, and suppose some $m<n^\star$
has $g_m \ge g_{n^\star}$. By minimality F holds at $m$, so $g_m < f_m$; F fails at
$n^\star$, so $g_{n^\star}\ge f_{n^\star}$. Chaining the three:

$$
f_m \;>\; g_m \;\ge\; g_{n^\star} \;\ge\; f_{n^\star}. \tag{$\ast$}
$$

**A violation of (FFM) forces the barrier sequence to dip.** ∎

Two certificates read off ($\ast$) directly:

| | certificate | implies |
|---|---|---|
| **(C-mono)** | $f_m \le f_n$ for all $m<n$ | (FFM) at $n$ |
| **(C-head)** | $G_{n-1} := \max_{m<n} g_m \;<\; f_n$ | (FFM) at $n$ |

**(C-head) is sharp**, and it is exactly the exposure test: a violation at $n$ needs
$g_{n^\star}\ge f_n$ *and* $g_m\ge g_{n^\star}$ for some $m<n$, hence $G_{n-1}\ge f_n$.
So $H_n := f_n - G_{n-1} > 0 \Rightarrow$ (FFM) at $n$, and $H_n\le0$ is the only way
(FFM) can be in doubt at $n$.

Both use only $g_n<f_n$ and minimality. No $\pi(x)$ bound, no explicit constant, no
asymptotic regime, no exceptional small-$n$ set. Neither is a proof of (FFM) for all
$n$: each is a **per-index certificate**, and the range over which it is checked is
the range over which (FFM) is established — the same honest boundary as **CC-25**'s.

---

## 3. F2 — the obvious proof of (FFM) is wrong  [C]

The tempting argument is: *"$f_n \approx \log^2 p_n - \log p_n - 1$ (**CC-18**) and
$\log p_n$ increases, so $f$ increases, so no earlier index can carry a larger
barrier."* Measured over $5.08\times10^{7}$ steps:

```
Delta_n = f_(n+1) - f_n
   negative :   29,430,427   (57.88%)
   positive :   21,417,105   (42.12%)
   zero     :            0
```

$D_n := \max_{m<n} f_m - f_n > 0$ at **44,683,195 of 50,847,533** indices (87.88 %).

**Why the intuition inverts.** $f_n$ depends on two arguments that both move. With
$p_n/n\approx L-1$ (**CC-20**):

* one index step raises $p$ by $g_n$, raising $f$ by $\approx g_n(2L-1)/p$;
* one index step raises $n$ by 1, lowering $f$ by $\approx pL/n^2 \approx L^3/p$.

The rise beats the fall iff $g_n \gtrsim L^2/2$. Typical gaps are $\approx L$, so
**the barrier falls at typical indices and climbs only at unusually large gaps** —
precisely the record gaps. Monotonicity is not a weak version of the truth; it is the
mechanism run backwards.

> **Consequence for `proof-attempt__0` and `lean-skeleton`.** Any candidate argument
> for (FFM) that asserts, assumes, or silently uses "$f$ (or $\ell$) is nondecreasing
> in $n$" is refuted by this computation and should be terminated at that step. This
> is a *loud* filter — it names a specific false lemma — and unlike FT-7 (see
> `outcomes.md#A2`) it does not require reading the candidate's method.

---

## 4. F3/F4 — what actually saves (FFM)  [C]

```
min H_n = f_n - G_(n-1)   : 1.196152   at n=2, p=3, G=1, f=2.196152
indices with H_n <= 0     : 0
runtime float-guard trips : 0
=> (FFM) CERTIFIED for every n <= 50,847,533   (p < 1,000,000,000)
```

The minimum sits at $n=2$ — the regime **CC-14** and `outcomes.md#A4` identify as
tightest for *every* relaxed criterion in this corpus ((SUF) and the integer criterion
of **CC-28** both go silent at $n=2,4$). (C-head) is not one of them: it fires at
$n=2$ with $H_2=1.196$, and $n\le200$ is covered independently by the exact integer
predicate (200/200 checked, zero float/exact disagreements).

The structural reason (FFM) survives is F4:

```
max D_n = 0.548660 at n=214, p=1,307
indices with D_n >= 1 : 0
max D_n by index decade:
   10^0 <= n < 10^1 : 0.028133   10^4 <= n < 10^5 : 0.102459
   10^1 <= n < 10^2 : 0.545663   10^5 <= n < 10^6 : 0.018055
   10^2 <= n < 10^3 : 0.548660   10^6 <= n < 10^7 : 0.004564
   10^3 <= n < 10^4 : 0.220716   10^7 <= n < 10^8 : 0.000929
```

By ($\ast$), a (FFM)-violating first failure needs an **integer** $g$ with
$f_{n^\star} \le g < f_m$. That window has width $D_{n^\star}$. **It is narrower than
one gap unit at every index in range, and above $n=10^{2}$ it narrows by a factor
between 2 and 6 per index decade** (0.5487, 0.2207, 0.1025, 0.0181, 0.00456,
0.000929 — the decay is steady but not a clean power of ten, and no rate is claimed). An integer does land inside the window at 3,948 indices — the window is a
real object, not an empty formality — but the additional requirement $g = g_{n^\star}$
and $g \le G_{n^\star-1}$ is never met.

This is the same shape as **CC-14**'s dead band in a different coordinate: *emptiness
of density $\approx D_n$, never a theorem*. Stating it as a theorem is the error
`outcomes.md#A4` caught upstream, and it would be the same error here.

---

## 5. F5 — (FFM) is F's shadow, not a shortcut  [C]

Two ratios, each of which must reach 1 for the corresponding statement to die:

$$
X_n := \frac{G_{n-1}}{f_n} \ \text{((C-head) dies at 1)}, \qquad
Y_n := \frac{g_n}{f_n} \ \text{(F dies at 1)}.
$$

```
              max X          max Y        Y - X
 10^3     0.7422920      0.7604709     1.818e-02
 10^4     0.7470543      0.7485330     1.479e-03
 10^5     0.7438938      0.7440429     1.491e-04
 10^6     0.7590413      0.7590821     4.078e-05
 10^7     0.7895880      0.7895959     7.834e-06
 10^8     0.7526161      0.7526164     3.470e-07

max X (n>10) = 0.789588040   at n=1,319,946  p=20,831,533
max Y (n>10) = 0.789595874   at n=1,319,945  p=20,831,323
```

Both global maxima sit at the **same record gap** ($g=210$ at $p=20\,831\,323$), one
index apart: $Y$ at the record itself, $X$ at the index immediately after. The lead $Y-X$
is **monotonically decreasing from $p\sim10^{3}$ onward** (it is *not* monotone below
that — it rises from $3.5\times10^{-2}$ at $p\sim10^1$ to $7.4\times10^{-2}$ at
$p\sim10^2$, the small-$n$ regime again), losing between one and two orders of
magnitude per decade over the six decades where it is monotone.

A sharper instrument confirms it. For each record $m$, let
$\rho_m := g_m / \min\{f_n : m<n\le\text{next record}\}$. If the post-record dip ever
fell below the barrier's value *at* the record, (C-head) would become the binding
constraint there. Over all 29 records with a successor window: **0 such records**,
largest lead $\rho_m-Y_m = -2.079\times10^{-7}$.

> **Reading.** (C-head) and F are asymptotically the *same* barrier. The certificate
> outlives F, but by a margin equal to the barrier's jump at a record gap, and that
> margin is going to zero.
>
> **The verification literature should be read accordingly.** The maximal-gap-table
> method is not an independent shortcut that would survive F's refutation. It is valid
> precisely to the height at which F itself has been checked, and its validity margin
> vanishes as height grows. `write-paper` should not present the $2^{64}$ frontier as
> resting on a cheaper foundation than the conjecture it verifies.

---

## 6. F6 — the heuristic residual  [L3 — model only, not evidence]

Under Cramér, $\Pr[g_m>t]\approx e^{-t/L_m}$. Conditional on $n$ being a first
failure, an earlier $m$ violates (FFM) only if $g_m$ lands in the sub-unit window
$[f_n,f_m)$. Expected count:
$\Lambda_n = \sum_{m<n,\ f_m>f_n}\big(e^{-f_n/L_m}-e^{-f_m/L_m}\big)$,
truncated to a back-window of 200 000 indices (M2 says the running maximum is recent,
so that is where the mass is), evaluated on 1 500 sampled indices per window.

| $p$ window | mean | median | max |
|---|---|---|---|
| $2\times10^{6}\ldots1.2\times10^{7}$ | $2.74\times10^{-10}$ | $4.41\times10^{-11}$ | $1.05\times10^{-8}$ |
| $5\times10^{7}\ldots2\times10^{8}$ | $4.38\times10^{-12}$ | $5.40\times10^{-13}$ | $1.98\times10^{-10}$ |
| $7\times10^{8}\ldots10^{9}$ | $1.26\times10^{-13}$ | $1.68\times10^{-14}$ | $4.66\times10^{-12}$ |

Three windows over one and a half decades, with a stated truncation and a subsample,
do not establish a rate. What they rule out are the two answers that would have
mattered: $\Lambda$ is neither $0$ (the window is real) nor $O(1)$ (the target is not
heuristically fragile). The direction is the reassuring one — under this model **(FFM)
becomes safer with height, which is the opposite of how F behaves**.

**Two-sided calibration** (`outcomes.md#A10`, FT-8): naive Cramér predicts
$\approx e\log\log(4\times10^{18}) \approx 10$ Firoozbakht counterexamples below the
verified frontier; there are **none**. Not decisive — naive independence is a poor
guide in the extreme-gap regime — but it is the observation the upstream test suite
was structurally unable to make, every test in it having been built to detect a
counterexample and none to detect its absence.

---

## 7. Pre-registered falsification

Stated *before* any wider run, so a later result cannot be read post hoc.

| # | claim | falsified by |
|---|---|---|
| **P1** | (C-mono) is refuted | nothing — closed; $2.9\times10^{7}$ witnesses |
| **P2** | $D_n<1$ for all $n$ | any index with $D_n\ge1$ |
| **P3** | (FFM) holds for $n\le5.08\times10^{7}$ | any index with $H_n\le0$ **and** an exact re-check confirming it. A float-only hit is a `CANDIDATE (float) — requires exact re-check`, never a refutation (`outcomes.md#A5`). |
| **P4** | (C-head) is never the binding constraint | any record $m$ with $\rho_m>Y_m$. Margins are $\sim10^{-7}$: **this is the claim most likely to break first, and it breaks *without* (FFM) being false.** |
| **P5** | $X$ and $Y$ converge | the per-decade lead ceasing to shrink, or reversing sign |

**The failure mode this leg exists to name.** If P4 breaks at some height and P3 then
also breaks there, (FFM) loses its elementary certificate and the maximal-gap
verification method loses its licence at that height — *without F itself being
refuted*. That is a specific, checkable, currently-unexamined risk to **CC-25**.

---

## 8. Corroboration of upstream numbers

Everything below was recomputed here from integers, not copied.

| quantity | upstream | this leg | route |
|---|---|---|---|
| maximal gaps below $10^{9}$ | OEIS A005250 (V2/V3) | 30 records, last $g=282$ at $p=436\,273\,009$ — **exact match** | own sieve |
| $\min_{n>200} S_n$ | **CC-27**: $1.7008$ at $n=217$ | $1.700799909$ at $n=217$, $p=1327$ | `log1p` + runtime guard |
| $\min_n S_n$ | **CC-03**: minimum at $n=2$ | $0.076961041$ at $n=2$ | ditto |
| $f_k$ at the record locus | **CC-26** / Kourbatov Tab. 1: $1193.418$ | $1193.41777829403992714$ | mpmath, 60 dps |
| $\ell_k$ at the record locus | **CC-26**: $1194.516$ | $1194.51592191171838348$ | mpmath, 60 dps |
| $R_k$ at the record locus | **CC-26** / FAQ: $0.9206$ | $0.92063858855742052574$ | mpmath, 60 dps |
| shortfall $f_k/g_k$ | **CC-26**: $1.05426$ | $1.05425598789226$ | mpmath, 60 dps |

The record locus sits at $p=1.69\times10^{15} > 2^{53}$, so **CC-27** forbids the
double path there; mpmath from the exact integers is the only admissible route, and
the code refuses to run doubles above $2^{53}$ rather than warning about it.

---

## 9. Honest boundaries

1. **$10^{9}$ is eleven orders below $2^{64}$.** This leg does not extend **CC-25**'s
   frontier and is not a weak version of it. It is a different thing: a check that the
   *statement being relied upon* is the right one, and that its elementary certificate
   actually fires.
2. **(FFM) is not proved.** F3 is a range statement. F4's sub-unit dip is a measured
   density, not a theorem — exactly the error `outcomes.md#A4` caught upstream.
3. **F6 is a model, not evidence.** Tagged L3, with its truncation and subsample
   stated. It is reported because it makes the residual risk numerical, not because it
   bears on the truth of anything.
4. **F itself: neither proved nor refuted.** Zero screen hits below $10^{9}$.
5. **A real defect was found and fixed in this leg's own code**, not in review: the
   streaming scan applied `np.maximum.accumulate` per segment without seeding from the
   carried-in running maximum, so $G_{n-1}$ and $\max_{m<n}f_m$ silently restarted at
   every 16 MiB boundary. The single-segment run was correct and the multi-segment run
   was not — the configuration-invisible failure. It was caught only because the
   record table stopped being monotone, which has an independent oracle. `ffm.py
   --selftest` is the permanent version of that oracle and is run as a notebook cell.
   Recorded here per `outcomes.md`'s D1 lesson: **a passage that reasons *about* a
   computation rather than *from* it is unverified until the computation prints the
   number the prose asserts.**

---

## 10. The obligation, and the handover

### 10.1 What is actually left to prove

Be precise about what (C-head) leaves open, because the tempting shortcuts are all
wrong. $D_n<1$ does **not** imply (FFM): a window of width $<1$ still contains an
integer at 3,948 indices in range, and (FFM) survives there only because the specific
gaps $g_m$ do not land inside. The correct obligation is:

$$
\textbf{(D)}\qquad
\sup_{n>m}\ \big(f_m - f_n\big) \;<\; f_m - G_m
\qquad\text{for every } m,
$$

where $G_m = \max_{j\le m} g_j$. In words: *the barrier never descends, at any later
index, by more than the margin with which F holds at $m$.*

**(D) together with F implies (FFM).** For $n>m$, $f_n > f_m - (f_m - G_m) = G_m \ge g_m$,
so $G_{n-1} < f_n$ for every $n$, which is (C-head) at every index. ∎

Measured at all 30 records below $10^{9}$ (notebook §12; $\Delta_m$ assembled by a
reverse cumulative minimum over the per-record window minima — it is a *suffix*
quantity and cannot be read off the forward scan's global $\max D_n$):

| | quantity | value |
|---|---|---|
| LHS | $\Delta_m = \sup_{n>m}(f_m-f_n)$ at every record $m$ | **negative at all 30**; closest approach $-1.0\times10^{-4}$ at $n=10\,655\,462$ |
| RHS | $f_m-G_m$, the Firoozbakht margin at a record | $0.196$ ($n=2$) rising to $92$–$120$ above $n=10^{7}$ |
| RHS | $f_k-g_k$ at **CC-26**'s record locus ($g=1132$) | $61.42$ |

**violations: 0.** $\Delta_m<0$ is a stronger statement than (D) needs: *after a record
gap, the barrier never returns to the value it had there* — so the "descent" the
obligation must bound is not merely small, it is (in range) not a descent at all. The
closest approach shrinks toward $0^-$ with height ($-1.2\times10^{-2}$ at $n\sim10^5$,
$-1\times10^{-4}$ at $n\sim10^{7}$), so a sign change at greater height is not excluded
— but it would have to be followed by ~90 further gap units of descent before (D)
binds, against a total observed descent range of $0.55$ units.

This is F5 arriving from the other side: **(D) is nowhere near the tight constraint;
F is.** Note the shapes of the two sides:
the RHS is exactly the Firoozbakht margin, so (D) *cannot* be proved without F; but
the LHS is a statement about the barrier's behaviour at **ordinary** indices, where
$g_n\approx\log p_n$, and not about extreme gaps at all. That is what distinguishes it
from **CC-23**'s barrier, which needs control of the largest gaps and has no known
route, conditional or unconditional.

### 10.2 Handover

**To `proof-attempt__0`** — the target now has a precise shape, stated in §10.1
below. The obvious route through monotonicity of the barrier is closed (F2); the live
obligation is a bound on the barrier's *descent*, and it is a statement about ordinary
gaps rather than extreme ones.

**To `lean-skeleton`** — (C-head) is `decide`-shaped: `G_{n-1} < f_n` with
$G$ maintained as a running fold. Combined with **CC-28**'s integer criterion it gives
a finite verification whose *coverage claim* is honest, because (FFM) is what turns an
index-range check into a height-range claim. The index/height disambiguation of
**CC-25** applies unchanged.

**To `write-paper`** — F5 is the paragraph nobody has written: the maximal-gap
verification method is not a cheaper foundation than the conjecture it verifies, and
its margin vanishes with height. State it with the numbers of §5.

**To `skeptic` / `red-team-corpus`** — P4 is the soft joint. Attack it.

**Cosmon-ward** — none. No cosmon-level invariant was broken and no primitive was
missing; the friction in this leg was a broken `jupyter_events`/`jsonschema` pair in
the ambient Python environment, which is an environment fact, not a core pathology.
It was routed around (`build_notebook.py`) rather than patched into the operator's
global installation.

---

## 11. Artifacts and reproduction

| file | what |
|---|---|
| `ffm.py` | the engine: streaming segmented sieve, all measurements, self-test, mpmath checks. CLI, exit-coded. |
| `notebook-0.py` | the notebook in `py:percent` — diffable source of truth |
| `notebook-0.ipynb` | the executed notebook, outputs and figures embedded |
| `build_notebook.py` | percent → `.ipynb` + execute, without `jupytext`/`nbconvert` |
| `findings-0.md` | this note |
| `scan-1e9.out`, `scan-1e9.json` | the full $10^{9}$ scan, human and machine readable |
| `fig-1-dip-depth.png` | M2 — the exposure window, sub-unit and shrinking |
| `fig-2-two-barriers.png` | M4 — $X$ vs $Y$ per decade, and the shrinking lead |
| `fig-3-records.png` | M5 — the 30 maximal gaps and each one's shortfall |

```
python3 ffm.py --selftest                       # segment-invariance, ~2 s
python3 ffm.py --quick                          # p < 5e6,  ~2 s
python3 ffm.py --limit 1000000000 --json s.json # p < 1e9,  ~35 s, 0.4 GB
python3 build_notebook.py                       # rebuild + execute notebook-0.ipynb
```

Requires `numpy`, `mpmath`, `matplotlib`, `nbformat`, `nbclient`. Deterministic: no
RNG anywhere in the pipeline. Exit 0 = every check passed.
