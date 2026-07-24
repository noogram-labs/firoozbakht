# Synthesis — the attack on Firoozbakht's conjecture

**Leg**: `synthesize` (fold of the `math-attack` polymer, run node)
**Target**: $p_{n+1}^{1/(n+1)} < p_n^{1/n}$ for all $n \ge 1$ — equivalently, $\big(p_n^{1/n}\big)_{n\ge1}$ is strictly decreasing
**External attribution**: Noogram · **Date**: 2026-07-24

> **Posture, non-negotiable and stated first.** Firoozbakht's conjecture is **open**.
> This run did not prove it and did not refute it. **No leg of this polymer produced
> any evidence about its truth in either direction**, and this synthesis produces
> none either. Everything below is a report on *arguments, formalisations, computations
> and citations* — never on the conjecture's truth value.

---

## 0. The verdict, in one box

| question | answer |
|---|---|
| Is the conjecture **proved**? | **No.** `Firoozbakht.firoozbakht` still ends in `sorry`; `#print axioms` shows `sorryAx`. |
| Is the conjecture **refuted**? | **No.** No counterexample at any scale examined; none is expected to be findable by any means this polymer can execute. |
| Was anything **proved**? | **Yes** — see §3. Eleven new `sorry`-free Lean theorems, the conjecture proved outright for $1\le n\le 25$, two exact refutation criteria, and three paper-level theorems that close *routes*. |
| Was anything **refuted**? | **Yes** — see §4. Three named routes are dead, two of them with quantitative, reproducible obituaries. |
| **Evidence-gate status** | **BLOCKED** — and this is the honest, load-bearing fact of the whole run. §7. |
| Rounds run | **1 of 1** (`exit_reason = "rounds-exhausted"`). §2. |
| Did the still-unproved list shrink? | **No — it is unchanged, 7 declarations in, 7 out.** Not churn either. §5.2. |
| Citation clearance | **NOT claimed.** The citation audit has not run; it gates the paper downstream at `citation-gate`. §8. |

---

## 1. What the problem actually is, once you look at it

The conjecture looks like a curiosity about exponents. It is not. The `decompose` leg
proved (L0, from first principles) that the five natural phrasings — the `rpow` form as
posed, the pure-integer form $p_{n+1}^n < p_n^{n+1}$, the log form, the "sequence
$(\log p_n)/n$ decreases" form, and a slack form — are **pointwise equivalent**, index by
index, with no asymptotics and no exceptional set. And it proved that the whole statement
collapses to a question about **prime gaps**:

$$\text{F holds at } n \iff g_n < f_n, \qquad f_n := p_n^{\,1+1/n} - p_n,
\qquad f_n \approx (\log p_n)^2 - \log p_n .$$

That is exact, not an approximation — the *exact* criterion (`CC-10`) has no side
condition at all, and the approximation to $(\log p)^2 - \log p$ is sharp at the frontier
to roughly sixteen decimal places.

Think of it as a fence. The primes walk along, and every step they take is a gap. The
fence sits at height about $(\log p)^2$. Firoozbakht says: **the primes never jump the
fence, not once, ever**. Two things follow immediately, and they organise the entire run:

- **To prove it**, you need an upper bound on *every* prime gap of size
  $(\log p)^2$-ish, with an explicit constant strictly below 1. Nothing in analytic
  number theory produces a polylogarithmic gap bound *at all*. The best unconditional
  bound is $g_n \ll p_n^{0.525}$; RH gives $g_n \ll \sqrt{p_n}\log p_n$. Those are powers
  of $p$ against powers of $\log p$. The distance is not quantitative — it is categorical.
- **To refute it**, you need one gap that clears the fence. The record ratio
  $R = g/(\log p)^2$ is about $0.9206$; the fence, at that same locus, sits at $0.9715$.
  A shortfall factor of about **1.054** — a few percent, in a quantity that took decades
  of distributed compute to measure.

So: the proof branch is blocked by a wall, and the refutation branch is *close* but not
reachable. That framing survived the whole run, with one correction (§4.4).

---

## 2. The trajectory — how many rounds, and what each one bought

The re-attack loop (`converge-math-attack`, v4) was configured with **`rounds = 1`**
against a sealed structural ceiling of 5. At `rounds = 1` the loop body never executes:
the guard is `round < rounds`, i.e. `1 < 1`, false. **One round ran, and it is round 1** —
the spore's own pinned `lean-probe` and `skeptic` nodes, read but never re-run. No
`attack-round-K/` directory exists on disk, and none was created. The verdict below rests
on **round 1's** artifacts because round 1 *is* the final round.

The executor was verified driver-capable (`cs wait` present, `cs run --resident` present),
so the loop stopped on the **cap**, not on broken machinery. Exhaustion is `BLOCKED`;
there is no path in this formula from "we ran out of rounds" to a pass.

| round | kernel | skeptic | converged? |
|---|---|---|---|
| **1** (final) | `UNPROVABLE_IN_BUDGET` | `blockers` (2 BLOCKER, 7 MAJOR, 9 MINOR) | **no** |

**What round 1 fixed.** Two things, and they are worth naming because a `BLOCKED` verdict
is easy to misread as a wasted run:

1. It closed the Lean skeleton's **debt D2** — the one the `decompose` leg had flagged as
   the tree's *main risk item*. `Nat.nth` is noncomputable, so no finite range could be
   machine-checked at all; there is now a bridge lemma whose cost scales with the *gap*
   between consecutive primes rather than with the prime itself, and on top of it a
   certified 26-entry prime table. That is a real engineering result.
2. It produced the corpus's own audit: a skeptic that attacked every proof line by line,
   re-derived every load-bearing number in independent code, re-ran all seven verifier
   scripts, and fetched five primary sources — and found **no mathematical error in any
   theorem** while finding two false statements the corpus was making *about itself*.

**Because only one round ran, there is no shrink-versus-churn signal to read.** §5.2 says
plainly what the unproved list did (nothing) and why that is expected rather than
disappointing.

---

## 3. What was PROVED

Two registers: things a machine checked, and things a human argument closed. They are
listed separately on purpose.

### 3.1 Machine-checked in Lean 4 / Mathlib (`lake build` exit 0, `#print axioms` audited)

Eleven new theorems plus a 26-entry prime table, every one showing the clean axiom line
`[propext, Classical.choice, Quot.sound]` — **no `sorryAx`, and no `native_decide`**, so
nothing enters the trusted base beyond Mathlib's ordinary foundation.

| result | what it says | confidence |
|---|---|---|
| `firoozbakht_of_le` | **the conjecture itself, proved outright, for every $1 \le n \le 25$**, in the literal `rpow` form — not a surrogate | **certain** (kernel-checked, non-circular: its axiom line is clean) |
| `nthPrime_succ_eq` + table | the `Nat.nth` → numeral bridge and $p_1=2,\dots,p_{26}=101$, each entry *derived*, no numeral asserted | **certain** |
| `firoozbakht_of_gap_lt` | sufficient criterion: $n\,g_n < p_n \log p_n \Rightarrow$ F at $n$ | **certain** |
| `not_firoozbakht_of_barrier_le`, `not_firoozbakht_of_pow_le` | two **exact** refutation criteria — the second is pure ℕ arithmetic, the right target for a computational search | **certain** |
| `nthPrime_succ_lt_two_mul`, `firoozbakht_of_two_pow_le` | Bertrand's postulate in strict consecutive-prime form, and the conditional it yields | **certain** |
| `firoozbakht_iff_log` and the skeleton's bridges | the F1–F5 equivalences, machine-checked | **certain** |

An independent `sympy` cross-check (outside Lean, so it checks that the *numerals are the
primes anyone means*) confirms the table and the certified range.

**Honest sizing, in the leg's own words:** `n ≤ 25` is set by compile time, not by
mathematics, and it is derisory next to Kourbatov's $4\cdot10^{18}$ or Visser's $2^{64}$.
**No finite range bears on the conjecture. A certified range is a fidelity instrument,
not evidence.** Its value is that the formal method now exists and that a kernel — not a
script — checked it.

### 3.2 Proved on paper (arguments, not certificates)

| result | leg | confidence |
|---|---|---|
| The five forms F1–F5 are pointwise equivalent; the exact criterion $g_n<f_n$ has no side condition | `decompose`, `concept-cards` | **high** (L0, independently re-derived by `skeptic`) |
| $S_n$ is strictly decreasing in $n$ for fixed $(p,g)$ — so a **refutation needs only a lower bound on $\pi(p)$**, never exact prime counting | `decompose` | **high** (L0) |
The **certificate (C-head)**, $G_{n-1} < f_n$, holds on $[89,H)$ for every $H$ to which the maximal-gap census is complete, and **FFM** (if F first fails at $n^\star$, then $g_{n^\star}$ is a record gap) is **certified, not assumed**, for all $n \le 50\,847\,533$ | `proof-attempt__0`, `notebooks__0` | **high** for (C-head); see the vacuity note below |
| **Theorem A**: F holds for every $n$ with $p_n < 10^{20}$ — a $5.4\times$ improvement on the frontier this polymer started with | `proof-attempt__2` | **medium** — see the tier note below |
| The "lever" that produces published verified ranges: ~80 inequalities decide $\approx 2.2\times10^{18}$ primes, i.e. $2.7\times10^{16}$ primes per inequality | `notebooks__2` | **high** [COMPUTED], conditional on census completeness |
| Exhaustive verification of F for all $p < 10^9$ (50 847 533 pairs) with a runtime-certified float error bound | `notebooks__2` | **high** — this is the only part of the range the project owns outright |

**Vacuity note on FFM, applied here rather than inherited (MAJOR-3).** `proof-attempt__0`'s
verdict table advertises "FFM proved on $[89,10^{20})$" as a headline deliverable. The
skeptic showed — using PA-0's own Proposition 2 — that on any range where F is *verified*,
FFM is **vacuously true** and carries zero information; and `proof-attempt__2`'s Theorem A
verifies F on exactly that range from exactly those premises. **This synthesis does not
carry the vacuous claim.** What is non-vacuous, and what the row above states, is the
*certificate* (C-head) — strictly stronger than FFM-at-$n$ and not implied by F's
verification. The general FFM statement, for unbounded $n$, remains **open**, and PA-0's
real deliverable is its Theorem 6 dichotomy: *every known route to proving FFM passes
either through F itself or through a statement strictly stronger than F, so the apparent
weakness buys nothing.*

**Tier note on Theorem A.** It rests on exactly two external premises, both named and
isolated: an explicit $\pi(x)$ upper bound (Axler), and the completeness of the
first-occurrence prime-gap census below $10^{20}$ (a community computational record,
unrefereed, not machine-checked). By the propagation rule the conclusion inherits its
weakest premise: **its weak link is computational data, not mathematics.** And the Axler
citation carries a live fault — MAJOR-6, §6 — so the premise is *believed* but its
locator does not resolve in the version-of-record it is attributed to.

---

## 4. What was REFUTED

Nothing about the conjecture. Three **routes** died, and the obituaries are quantitative.

### 4.1 The RH route — refuted as a route, with numbers

The strongest *published* RH-conditional prime-gap bound (Carneiro–Milinovich–Soundararajan
2019, constant $22/25$) decides Firoozbakht at **exactly three indices**, $n\in\{1,2,3\}$ —
and at **exactly one**, $n=3$ ($p=5$), once the source's own validity restriction is
honoured. It is silent at every $n\ge4$, forever.

This is not a constant problem. For **every** $c>0$, a bound $g_n \le c\sqrt{p_n}\log p_n$
decides the conjecture on a finite initial segment only; improving the constant moves a
threshold and never changes the verdict. Stronger still: for every $c \ge 10^{-7}$ — seven
orders below the published constant — the usable set ends *inside* the range already
verified unconditionally. **RH contributes zero indices that unconditional verification
does not already own.** Confidence: **high** (L0 + recomputed).

The $\sqrt{x}$ shape is not laziness either: Littlewood's unconditional $\Omega_\pm$
theorem forces any argument that bounds the prime-counting error pointwise and subtracts
to work at that scale, *regardless of RH*.

**What was deliberately not proved**: that RH does not *imply* Firoozbakht. That is a
claim about RH's deductive closure and nothing establishes it. The barrier proved is
about the *shape of the bounds the known machinery emits*, over an explicitly enumerated
surveyed set. That restraint is correct and should be preserved in the paper.

### 4.2 The monotone-barrier route to FFM — refuted by an exact witness

The tempting proof of FFM routes through monotonicity of the exact barrier $f_n$. $f_n$ is
**not monotone**: it decreases at 57.88 % of steps and sits below its own running maximum
at 87.88 % of indices, and there is an exact-integer witness at $n = 7, 8$. The error is
subtle and worth recording: the *proxy* barrier $\ell_n = \log^2 p_n - \log p_n$ **is**
monotone in the index; $f_n$ is not. Substituting one for the other is the mistake, and
it is what makes FFM "the price of exactness". Confidence: **high** ([C], independently
reproduced by `skeptic` to the digit).

### 4.3 Unconditional analytic extension of the verified range — refuted permanently

No unconditional analytic result in the literature extends the verified range by a single
prime, and none can: the best proven gap bound overshoots the barrier by a factor that
**diverges** — $1.5\times10^{7}$ at $10^{20}$, $6\times10^{47}$ at $10^{100}$. The range
is **data-bound, not method-bound**: at the tightest of the 85 records the analytic
criterion is satisfied with 5.1 % to spare, and $H$ stops exactly where the gap census
stops. Confidence: **high** [C].

### 4.4 One correction to the run's own opening framing

The `decompose` leg opened with "the refutation branch is the live branch, the shortfall
is 5.5 %". The `frame-deliberation` panel and `notebooks__2` between them **partially
demoted** that:

- the "5.5 %" is arithmetically right (corrected value $1.0543$) but is **not L0** — its
  derivation traverses an unclosed literature node;
- the (SUF) criterion the headline leans on is **silent at $n = 2$ and $n = 4$**, which are
  the *tightest known cases of the conjecture*. F is true there by the exact predicate
  ($25 < 27$, $14641 < 16807$) — but the criterion does not know it;
- of two **pre-registered** demotion criteria, one fired and one did not. The naive model
  predicts 6.72 counterexamples below the verified frontier where 0 are observed
  (over-predicting by $6.7\times$); the fitted crossover height is $10^{30}$–$10^{51}$,
  not $10^{21}$.

**The honest reading**: the refutation branch keeps its heuristic motivation (Granville's
$\limsup R \ge 2e^{-\gamma} \approx 1.1229$ is not excluded; $\limsup R$ is genuinely
unknown) and loses its quantitative one. Search is not affordable at any of those
heights. Note the crossover-height figure itself carries a live fault (MAJOR-9: it is a
30-decade extrapolation whose model tag did not survive into the budget decision).

---

## 5. What remains OPEN

### 5.1 The conjecture

Open, exactly as it has been since 1982. `Firoozbakht.firoozbakht`:
**UNPROVABLE_IN_BUDGET — not false, not proved, open.** That phrase is a statement about
the *problem* being open, not about the budget being small: closing it in Lean would
require first formalising a theorem nobody has proved on paper.

`grep -ri firoozbakht` over the entire Mathlib tree returns **zero files**. An `exact?`
search with the project in scope "closed" the goal by citing the `sorry`-ed declaration
itself — recorded deliberately, because that is precisely the failure mode an automated
proof leg produces and a gate must catch. *An `exact?` success is not a proof.*

Bertrand's postulate is in Mathlib and *does* apply — and it is now a **theorem of this
development** that Bertrand closes **exactly one index** ($n=1$; it fails at $n=2$ because
$p_2 = 3 < 4$, and that failure is machine-checked too).

### 5.2 The still-`sorry`'d list — unchanged, and honestly so

Seven declarations entered round 1 `sorry`'d and seven left `sorry`'d:

1. `Firoozbakht.firoozbakht` — the conjecture, literal `rpow` form (`Statement.lean:117`)
2. `firoozbakht_strictAnti` · 3. `firoozbakht_int` · 4. `firoozbakht_log` ·
5. `firoozbakht_antitone` · 6. `firoozbakht_slack` · 7. `firoozbakht_gap` — all six
inherit `sorryAx` from the head.

**The list did not shrink and it did not churn: it is identical.** The six corollaries were
never independent targets — they inherit and always would until the head falls. The
progress happened *beside* the list rather than inside it: the surrounding scaffolding went
from nothing to eleven machine-checked theorems, a closed debt, and both directional
criteria. **The tooling advanced; the target did not move.**

**The honest forecast, stated plainly because it is the useful signal**: more rounds of
the same shape would very likely keep producing scaffolding without moving the head. The
obstruction is not a missing lemma that a re-read of the faults would surface — it is the
categorical gap between polylogarithmic and power-of-$p$ gap bounds. If a round 2 is run,
its value is in closing the corpus faults and hardening the criteria, **not** in expecting
the `sorry` to fall.

### 5.3 Named open obligations handed forward

| id | obligation | owner |
|---|---|---|
| **D1** | the asymptotic expansion of $B_n$ (needs PNT with lower-order terms) is still not formalised | a future Lean leg |
| **D2′** | the certified range stops at $n\le25$ for compile-time reasons only; extending it is mechanical and of low value until automated to $10^6$+ | a future Lean leg |
| **D3** | Kourbatov's $-1$ necessary / $-1.17$ sufficient conditions are not formalised; `firoozbakht_of_gap_lt` is a *different, weaker, elementary* criterion and must not be cited as Kourbatov's | `citation-gate` |
| **G-N1** | census completeness below $10^{20}$ cannot be established by any b-file | open, structural |
| **G-N2** | monotonicity of $\underline B$ — a small, genuine open obligation created by `notebooks__2`; 0 non-monotone steps found on a grid ~500× denser than either leg used, but still unproved | open |
| **FFM** | the general statement (unbounded $n$) | open |

---

## 6. The corpus's own faults — two live BLOCKERs

The skeptic found **no mathematical error in any theorem**. Both BLOCKERs are claims the
corpus makes *about its own trustworthiness*, and both are false. That is exactly the
class of fault a seal exists to stop.

| id | fault | why it blocks |
|---|---|---|
| **BLOCKER-1** | `notebooks__2/data/PROVENANCE.md` says 44 rows of the load-bearing table were independently recomputed. **Thirty were.** A 47 % overstatement, sitting in the provenance record of the corpus's single weakest premise. | the two files contradict each other; the row range is load-bearing |
| **BLOCKER-2** | `notebooks__2/findings-2.md` §4: *"the verified range itself excludes $C \ge 1.1736$"* is an **invalid inference** — a finite range cannot exclude a value of a $\limsup$. It is a property of a model fit read as a statement about the tail. | it is the leg's advertised novelty and it is already being used to reallocate the polymer's budget |

Neither needs new mathematics. One is a wrong row count; one is a sentence that must be
deleted or restated.

Seven MAJORs must be closed before anything reaches the paper — dominated by citation and
cross-leg-consistency faults: a locator (`Cor. 3.5`) absent from the version-of-record it
is attributed to (**MAJOR-6**), two mutually incompatible verification tiers for the same
source (**MAJOR-7**), a leg crediting a sibling with a premise the sibling had already
retracted (**MAJOR-4**), a vacuous conclusion sold as a headline verdict (**MAJOR-3**), a
docstring still carrying a formally retracted claim (**MAJOR-5**), an undated superlative
survey claim inside a V0 row (**MAJOR-8**), and the 30-decade extrapolation feeding a
budget call (**MAJOR-9**). Nine MINORs are annotations.

---

## 7. Evidence-gate status — **BLOCKED**, stated honestly

The gate is fail-closed and it closed. Three legs applicable, one passing.

| leg | requirement | verdict |
|---|---|---|
| **LOOP** | `reattack-verdict.json` present, well-formed, names the live round | present — live round = **1** |
| **KERNEL** | `lake build` exit 0 **AND** grep-clean of `sorry`/`axiom` | **FAIL** — exit 0 ✅ but one live `sorry` at `Statement.lean:117`; 7 declarations carry `sorryAx` |
| **SKEPTIC** | `faults.md` exists **AND** zero residual BLOCKERs | **FAIL** — 2 live BLOCKERs, independently re-confirmed present in the audited files |
| **CORPUS** | red-team corpus present **AND** coverage report non-empty | **PASS** — 19 entries / 15 categories / 339-line report |

Two points of discipline the gate got right and this synthesis preserves:

- **FAIL, not DEGRADED.** DEGRADED is available only when no formal backend was attempted.
  Here Lean *was* run, *did* build, and *did not* discharge the `sorry`. Calling that
  DEGRADED would convert "we tried and the conjecture is still open" into "we could not
  try" — a different and false statement.
- **The `sorry` grep has a false-positive mode.** A raw `grep -c sorry Statement.lean`
  returns 6, of which exactly one is a genuine `sorry` in code; the rest are prose
  discussing what remains open. The gate rested its verdict on the `#print axioms`
  whitelist, which cannot be fooled by formatting. The red-team corpus had demonstrated
  both this and axiom-smuggling (`RT-17`: builds with exit 0, `sorry`-free in code, and
  still smuggles an axiom) with working examples.

The corpus leg also flagged `RT-12`: a build that is exit-0, `sorry`-free, axiom-clean, and
a *true* theorem — where the falsehood would live in the sentence a paper puts next to it.
That is now a **standing requirement on `write-paper`**: for any declaration the paper
names as a result, print its type and confirm the conclusion is not among its hypotheses.

**What has to happen before the gate can be re-run**, in the order the evidence imposes:
close BLOCKER-1, close BLOCKER-2 and unwind the budget reallocation resting on it, then —
if further formal attack is wanted — re-germinate with `rounds ≥ 2`. Steps 1 and 2 are
corpus-honesty faults, not mathematics; closing them **will not** close the kernel leg.
The kernel leg closes only if the conjecture is proved, which is the open problem itself.

---

## 8. Scope — what this synthesis does NOT claim

- **No citation clearance.** The citation audit has **not** run. It gates the paper
  downstream at `citation-gate`. Three citation-shaped faults are handed forward
  explicitly (MAJOR-6, MAJOR-7, and gap G1 — $B_n$'s $-3/L$ and $-13/L^2$ lower-order
  terms appear nowhere in the cited theorem). Four of the skeptic's checks were external
  retrievals it quoted verbatim with URLs precisely so `citation-gate` can re-fetch and
  compare rather than take its word. Nothing here pre-empts that.
- **No claim that any verified range is evidence.** It is not, and every leg that produced
  one said so first.
- **No claim about the conjecture's likelihood.** The community's heuristic expectation
  (Granville) is that it is **false**; that expectation is recorded as a heuristic and is
  never used as a premise anywhere in this corpus.
- **No re-execution of `lake build`** by this leg; the shipped `build.log` (exit 0, 8253
  jobs) is read as the record of the kernel leg's own run, exactly as `evidence-gate` did.

---

## 9. The one-paragraph version

The primes walk along a road, and every step is a gap. Firoozbakht drew a fence at height
roughly $(\log p)^2$ and said the primes never jump it — not once, not ever, all the way
to infinity. This run did not decide that. What it did do is survey the ground carefully:
it proved the fence is *exactly* the right description of the conjecture, not an
approximation; it proved the primes stay under it for the first 25 steps, inside a
machine-checked kernel, and for every prime below $10^{20}$ by an argument resting on two
named external premises; it proved that the Riemann hypothesis — the biggest hammer anyone
would reach for — settles the question for the prime 5 and for no other; it proved that no
unconditional method in the literature can extend the verified range by a single prime, and
that the shortfall diverges rather than closing; and it built the two exact criteria a
future proof and a future refutation would each need. Then its own auditor found two places
where the corpus was overstating how carefully it had checked itself, and the gate closed
red on that. **The mathematics in this corpus is sound. Its self-description was not, in two
places. The conjecture is still open.**

---

## 10. Confidence ledger

| claim | confidence | basis |
|---|---|---|
| The conjecture is **open** | **certain** | `sorryAx` present in the kernel audit; nothing anywhere claims otherwise |
| F1–F5 equivalence; exact criterion $g_n<f_n$ | **certain** | L0 proof + machine-checked bridges + independent re-derivation |
| F proved for $1\le n\le25$, literal `rpow` form, non-circular | **certain** | Lean kernel, clean axiom line, `sympy` cross-check |
| Two exact refutation criteria are correct contrapositives | **certain** | typechecked wiring: a counterexample inside the certified range yields `False` |
| RH route decides exactly one index inside validity | **high** | L0 + recomputed |
| No unconditional analytic extension of the range; shortfall diverges | **high** | L0 + computed at named heights |
| FFM certified for $n\le5.08\times10^{7}$ | **high** | [C], reproduced independently by the skeptic to the digit |
| Theorem A: F for all $p_n<10^{20}$ | **medium** | inherits an unrefereed completeness claim (V1/L3) **and** a faulted citation locator (MAJOR-6) |
| "Refutation branch is the live branch" | **downgraded to heuristic-only** | one pre-registered demotion criterion fired; the quantitative motivation did not survive |
| Citation correctness across the corpus | **unknown** | audit not run — `citation-gate`'s job, §8 |

---

## 11. Provenance

Every artifact folded here, with the round it belongs to. Round 1 is the final round.

| leg | artifact | role in this fold |
|---|---|---|
| `re-attack` | `reattack-verdict.json`, `rounds.md`, `synthesis.md`, `preflight.md` | loop verdict; names round 1 as final |
| `lean-probe` (round 1 kernel) | `lean-probe-report.md`, `build.log`, `sorry-audit.txt`, `crosscheck.txt` | §3.1, §5.1, §5.2, §7 |
| `skeptic` (round 1 adversary) | `faults.md`, `verify_faults.out` | §6, §7 |
| `evidence-gate` | `evidence-verdict.md`, `verify_gate.out` | §7 |
| `decompose` | `decompose.md`, `verify_small_range.py` | §1, §3.2 |
| `frame-deliberation` | `outcomes.md`, `synthesis.md`, 5 panel responses | §4.4 |
| `source-ledger` | `source-ledger.md` (V0–V3 protocol) | tiering throughout |
| `concept-cards` | 31 cards + `INDEX.md` | CC-01, CC-07, CC-09, CC-10, CC-25, CC-31 anchors |
| `lean-skeleton` | `skeleton.md`, `lean/` (FROZEN fidelity anchor) | the statement of record |
| `proof-attempt__0` | `proof-attempt-0.md` | §3.2 (FFM), §4.2 |
| `proof-attempt__1` | `proof-attempt-1.md`, `verify_rh.out` | §4.1 |
| `proof-attempt__2` | `proof-attempt-2.md`, `verify_pa2.out` | §3.2 (Theorem A), §4.3 |
| `notebooks__0` | `findings-0.md`, `scan-1e9.json` | §3.2, §4.2 |
| `notebooks__1` | `findings.md`, `notebook-1.out` | §4.1 |
| `notebooks__2` | `findings-2.md`, `results-2.json`, `data/` | §3.2, §4.4, §6 |
| `red-team-corpus` | `coverage-report.md`, `corpus/RT-01…19.json` | §7 |

**Not folded**: `citation-gate`, `editorial-verdict`, `write-paper`, `chronicle` — all
empty, all downstream, none cleared to run on this evidence.

---

## 12. Reproduction

```
cd <run>/synthesize
python3 verify_synthesis.py      # re-derives every load-bearing claim from the artifacts
```

`verify_synthesis.py` re-parses `reattack-verdict.json`, re-greps the Lean sources and
`build.log` for the `sorry`/axiom facts, re-reads the two BLOCKER artifacts to confirm the
faults are still live, re-counts the red-team corpus, and asserts that this document's
verdict fields match the upstream artifacts rather than the model's memory of them. It is
a tripwire: if a future session closes the BLOCKERs, the corresponding checks **fail**,
which is the intended behaviour.

**Build/test status, reported honestly.** `python3 verify_synthesis.py` — **56/56 checks
pass, exit 0** (output of record: `verify_synthesis.out`). `ruff check verify_synthesis.py`
— clean. `python3 -m py_compile` — OK. Deterministic; no RNG; no network. Two checks this
leg did **not** run and does not claim: it did not re-execute `lake build` (it reads the
kernel leg's shipped `build.log`), and it did not re-fetch any external citation (that is
`citation-gate`'s job, §8). `.cosmon/config.toml` defines no `[gates]` for this project, so
these are the gates this leg imposed on itself; the run artifacts live under
`.cosmon/state/`, which `.cosmon/.gitignore` excludes from version control, so there is
nothing for this leg to commit — the same condition every other leg of this run worked
under, and the working tree is clean.

**Two claims in this document that a reader should treat as corrections rather than
inheritance**, both applied here rather than passed through: the FFM vacuity note in §3.2
(MAJOR-3) and the crossover-height caveat in §4.4 (MAJOR-9). Every other MAJOR is recorded
in §6 as still live and is **not** relied on anywhere above.

---

*Emitted by the `synthesize` leg (node) of the `math-attack` polymer.
Firoozbakht's conjecture is **open** — not proved, not refuted. This leg produced no
evidence about its truth, and found none in the corpus it folded. Evidence-gate status:
**BLOCKED**. Citation clearance: **not claimed, not audited**.*
