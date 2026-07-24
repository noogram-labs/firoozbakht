# Recommendation — stress-test of the Firoozbakht attack-surface decomposition

**Commissioned by:** node (leg `decompose`, crew role `concept-writer`), for the `math-attack` polymer node
**Blocks:** node (leg `source-ledger`) and the rest of the DAG
**Panel:** wheeler · feynman · gödel · popper · knuth (5, auto-selected)
**Leg:** `frame-deliberation`, molecule node, crew role `skeptic`
**Date:** 2026-07-24

**Artifacts under review** (paths relative to project root `firoozbakht/`):
- `.cosmon/state/spore-runs/node/decompose/decompose.md`
- `.cosmon/state/spore-runs/node/decompose/verify_small_range.py`

**Supporting artifacts of this leg** (same run dir, under `frame-deliberation/`):
`frame.md` · `responses/{wheeler,feynman,godel,popper,knuth}.md` · `synthesis.md` · `moderator-crosscheck.md`

> **Locator convention.** Every section reference below is a locator *into a named
> artifact*, written `<relative-path>#<section>`. Bare locators (`§2.3-C3`) are
> never used alone. Where a claim rests on the literature it is tagged with the
> `source-ledger` debt number that owns it, never with a bare author-year.

---

## Verdict

**The decomposition's mathematics is sound and should not be re-derived; its
quantifiers, tiers, tests and routing are not, and four defects would corrupt
downstream legs if compute were spent first.** Five independent adversarial
reviews plus an independent moderator cross-check converged on a single
structural disease: throughout `decompose/decompose.md`, a result proved about
**real numbers in an asymptotic regime** is asserted as a result about
**integers over all $n \ge 1$**. The sharpest instance is that the (SUF)
criterion — which `decompose/decompose.md#0.2` calls *"the criterion … sharp to
sixteen decimal places, **unconditionally**"* — **fails to fire at $n = 2$ and
$n = 4$**, precisely the two indices the same document's
`decompose/decompose.md#4` (test FT-4) identifies as the tightest known cases of
the conjecture. Four panelists found this by four different instruments. The
conjecture is unaffected — F is true there by the exact predicate ($25 < 27$,
$14641 < 16807$) — but the *criterion is silent* there, and the decomposition
does not know it. Alongside this: the headline "5.5% shortfall" is arithmetically
correct (corrected value **1.0543**) but is **not L0**, since its derivation
traverses an unclosed L2 node whose cited bound does not support the constant it
states; `decompose/decompose.md#5.4`'s float-rigour argument is wrong by
$3.3\times10^3$ in the unsafe direction and its `SAFETY_MARGIN` sits *below* the
error it guards; and test FT-7 — mandated Critical **and first** on every proof
attempt — is an invalid inference from semantic entailment to syntactic
occurrence that will silently discard correct proofs. **Recommendation: repair
before dispatch.** Nothing requires re-deriving C1–C4, the F1–F5 equivalences, or
the E3 collapse; every adjustment below is a restatement, a retier, or an added
guard. Two of them are blocking in the strict sense that spending compute first
destroys the compute.

---

## Recommended adjustments to the decomposition / falsifiability tests

Ordered by (damage if unfixed) × (irreversibility once compute is spent). **B** =
blocking, must land before the named leg runs.

### A1 — **[B]** Rewrite the float-extension instruction in `decompose/decompose.md#5.4`; fix `decompose/verify_small_range.py` line 79
*Blocks:* `notebooks__0/1/2`.
**Defect.** The stated error bound $\lvert\Delta S_n\rvert \lesssim 10^{-13}$ is
wrong: the true bound, derived from first principles and measured tight to 3%
against 60-digit decimal, is $n u + 6u\log p_n = 3.3\times10^{-10}$ at
$N = 5\times10^7$ — **linear in $n$**, exactly the growth the passage argues is
absent. The document's argument bounds the *magnitude* of $n\log(1+g_n/p_n)$;
$n$ is an exact integer and therefore amplifies an **absolute** error committed
one step earlier by `math.log(pn1 / pn)`. Consequences: `SAFETY_MARGIN = 1e-11`
is **0.033×** the error it guards (a silent-failure window, in the one mechanism
advertised as failing loudly); `min S_n (n > 200)` is **1.7008 at $n = 217$**,
not the stated $> 3.5$ (the document substituted the *normalised* statistic,
which the script prints, for the unnormalised one, which it does not); true
headroom is $\approx 9.7$ orders, not "~12".
**Required change.** (i) `math.log1p((pn1 - pn) / pn)` — one line, restores
$n$-independence, error drops to $\le 6u\log p_n \approx 1.2\times10^{-14}$.
(ii) `SAFETY_MARGIN` computed at runtime as $\max(nu,\ 6u\log p_n)$, not
hardcoded. (iii) Make the script **print** $\min_{n > \texttt{EXACT\_N}} S_n$ —
its absence is the mechanism that let the error in. (iv) State the unstated
precondition $p < 2^{53}$, and state plainly that above $p \approx 10^{16}$ the
double-precision slack test is **meaningless**: at the largest known maximal gap
the error bound ($nu = 47$) exceeds the slack ($S = 8.6$) by 5.5×, and $g/p$ falls
*below* the unit roundoff. Prescribe `fractions.Fraction`/`decimal` or the
integer criterion of A6 above that height.
*Why blocking:* `decompose/decompose.md#5.4` explicitly instructs `notebooks`
legs to "carry the same guard". Obeyed as written, it produces confident false
results at exactly the heights the refutation branch targets, and the guard does
not fire.

### A2 — **[B]** Demote FT-7 from complete-terminal-filter to non-terminal triage; add FT-7″
*Blocks:* `skeptic`, `red-team-corpus`. Owns `decompose/decompose.md#7` debt #11.
**Defect.** FT-7 (`decompose/decompose.md#4`) infers from a *statement*
equivalence to a claim about *proof texts*: "a candidate proof that does not,
somewhere, establish a polylogarithmic gap bound is wrong", to be applied
"without reading the candidate proof's details". That is semantic entailment
mistaken for syntactic occurrence. Four proof shapes evade it: contradiction
without exhibiting the bound; ineffective/non-explicit establishment; proof via a
stronger intermediate; induction with the constraint only ever local. Reached
independently by two panelists from opposite directions (structural: the
`#2`-preamble equivocates between *entailment* and *method*; epistemological: the
four evading shapes). FT-7 is also **irrefutable by construction** — the only
thing that could refute it is a correct proof of F, which FT-7 discards before
anyone reads it, and each kill is then counted as evidence for the strategic
conclusion that generated it.
**Required change.** Replace with **FT-7′** (extraction obligation, three
outcomes: (a) extracted → proceed; (b) not extracted *and* the method
demonstrably cannot yield it → escalate as strong evidence of error; (c) not
extracted, no obstruction identified → **flag as high-interest, do not
terminate**). Add **FT-7″** (relativization / prime-blindness barrier: enumerate
the arithmetic facts the candidate uses, build a set satisfying all of them with
a gap $\ge (\log p)^2$, and reject if the argument runs verbatim on it) — cheap,
structural, read-free, and itself falsifiable. Amend debt #11 to *"FT-7′ triage;
no candidate is terminated by FT-7′ alone."*
*Why blocking:* it is the only defect whose failure mode is a **silent false
negative that destroys a correct proof**, and it is mandated first.

### A3 — Retier the headline; correct $c_n$; state the shortfall as an interval
*Owner:* `source-ledger` (debt #4, promote **High → Critical**), then `write-paper`.
**Defect.** `decompose/decompose.md#0.2` and `#8` (items 2–3) are tagged **L0**
while their derivation routes through `#2.3-C4`, an **L2** node (debt #4,
Rosser–Schoenfeld / Dusart). `decompose/decompose.md#0.1` defines tiers for
*statements* and states **no propagation rule for inference**, so composites
silently inherit their *strongest* premise's tier. Compounding: C4's stated
$c_n \in (0.9, 1.2)$ is **not derivable from the bound it cites** — the two-term
bracket has width exactly $n$, permitting only $c_n \in (0.075, 1.104)$ at the
record locus; the stated interval is an eyeball fit to a table spanning
$n \le 3\times10^6$, applied nine orders of magnitude higher. On the document's
own authority the headline "5.5%" is pinned only to **5.2%–8.4%**.
**Required change.** (i) Add to `#0.1`: *tier(conclusion) = weakest tier among
its premises; a composite may be tagged L0 only if every premise is L0 or L1.*
(ii) Split `#0.2` into **2a [L0]** — F $\iff$ $g_n \lessgtr T_n = (p_n/n)\log p_n$,
sharp to $1 + O((\log p)^2/p)$, which survives audit intact — and **2b [L2, debt
#4]** — $T_n \approx (\log p_n)^2 - \log p_n$. Apply the same split to `#8` item 3.
(iii) `source-ledger` closes debt #4 with the **three-term** Dusart bound, which
pins $c_n \in (1.030, 1.034)$ at the record locus (validity $n \ge 688383$),
certifying shortfall $\mathbf{1.054 \pm 0.001}$. (iv) Correct the headline number
from 1.055 to **1.0543** (exact $T_n$, converged three ways) and carry the error
bar into `#0.2`, `#3.2`, `#4`-FT-2 and `#8`.

### A4 — Quantify the dead band; add the small-$n$ domain restriction everywhere the criterion is stated
*Owner:* `decompose` amendment; consumed by `lean-skeleton`, `notebooks`, `write-paper`.
**Defect — the panel's strongest result, found four independent ways.** (SUF)
fires when $g_n \le T_n$; (REF) when $g_n \ge T_n(1 + g_n/p_n)$. Between them lies
$B_n = \big(T_n,\ T_n/(1 - T_n/p_n)\big)$, width $W_n = T_n^2/(p_n - T_n)$, where
**neither** fires. Since $g_n$ is an **integer**, sharpness means
$B_n \cap \mathbb{Z} = \emptyset$, i.e. $T_n^2 + T_n < p_n$ — which holds for
$n \ge 484$ ($p_n \ge 3461$) and **fails for every $n \le 483$**. Exhaustive
search finds the band non-empty at exactly **$n = 2$ and $n = 4$** ($g_2 = 2 \in
(1.648, 2.747)$, band 67% wide; $g_4 = 4 \in (3.405, 5.351)$) — the two indices
`decompose/decompose.md#4`-FT-4 names as the tightest known cases. Even above 484
the band contains an integer at 0.69% of indices below $5\times10^7$; its
emptiness is a probabilistic fact of density $\approx W_n$, never a theorem.
Separately, `#0.2`'s *displayed* formula carries a **second** approximation,
$p_n/n \to \log p_n - 1$, whose error is $\approx 9\times10^{-4}$ — **twelve
orders larger** than the $5\times10^{-16}$ the sentence advertises, worth $+1.10$
**gap units** at the record locus. And $c_n - 1$ **changes sign at $n = 61$**, so
the proxy is wrong on one side or the other of that index when used as a
one-sided bound.
**Required change.** Write $B_n$ and $W_n$ explicitly in `#2.3-C3`; replace
"sharp … unconditionally" with the regime-qualified form naming $n \ge 484$;
state in `#0.2` that the displayed formula's error is $9\times10^{-4}$, **not**
the sandwich's $5\times10^{-16}$; record the $n = 61$ sign flip; and adopt the
$-1$ term (it is not "agreement to the constant" — it *is* the next term of
$p_n/n$, and it improves the proxy 11×).

### A5 — Make E1 a screen, not a decision; add the missing false-negative test
*Owner:* `decompose` amendment; consumed by `notebooks`, `red-team-corpus`.
**Defect.** `decompose/decompose.md#2.5`-E1 is stated in **(REF)** form, so a
candidate inside the band fails the screen and **never reaches E4** — the only
obligation carrying the exact predicate. FT-2 is worse: it triggers on the
**(SUF)** boundary (the *lower* edge), and has a demonstrated 4/4 false-positive
rate on $n = 1,2,3,4$; the script's `if pn > 1000` guard filters out exactly the
regime where FT-2 misfires, so the leg could not observe its own instrument
lying. The operationally relevant discard window is not the sandwich's
$8.4\times10^{-10}$ but the C4 proxy's **1.10 gap units** — $1.3\times10^9$ times
wider, and analysed nowhere in the document.
**Required change.** Restate E1 as a screen with an explicit tolerance $\delta_n$
covering the proxy error, the float resolution and $W_n$; make **E4 the sole
adjudicator**, mandatory on every screen hit. Restate FT-2's trigger in (REF)
form; change its teeth from *"maximal"* to *"screening only — a firing is a
nomination, adjudicated by FT-1"*; record the four known false positives. Add a
new falsifiability test for the **silent false negative** (a genuine
counterexample rejected by the screen) — currently absent; FT-5 treats the band
as a measurement tolerance, not as a region where the procedure returns no answer.
In `verify_small_range.py`, emit `CANDIDATE (float) — requires exact re-check`
rather than `COUNTEREXAMPLE` from the non-exact branch.

### A6 — Invert the Lean risk ranking; add the fidelity anchor set and the integer criterion; revise $N_0$
*Owner:* `lean-skeleton` (debts #7, #8) + `evidence-gate`.
**Defect (fidelity).** `decompose/decompose.md#6.3` ranks by *effort*, not by
*failure mode*. OB-F8 fails **loudly** (build does not close). OB-F1/F2/F3 fail
**silently**: the statement `∀ n, 1 ≤ n → Nat.nth Nat.Prime (n+1) ^ n <
Nat.nth Nat.Prime n ^ (n+1)` compiles, is true if F is true, is **strictly weaker
than F**, and passes both `example`s OB-F1 proposes. A generic
`firoozbakht_iff_rpow` can be proved sorry-free and **never instantiated at
`nthPrime`**. A one-directional bridge into a degenerate ℕ-elaborated RHS is
closed by `le_refl`. Also, OB-F1's own row states `nthPrime n < nthPrime (n+1)`,
which is **false at $n = 0$** (truncated subtraction gives $2 < 2$) — it needs the
`1 ≤ n` guard.
**Defect (cost).** F2's per-$n$ bignum comparison is $O(n^{1.6})$, so total cost
is $O(N_0^{2.6})$: $N_0 = 10^6$ is ~58 days *in Python*, before a single primality
certificate. `#3.5`-W5's "$N_0 = 10^6$–$10^8$" is **fantasy by 4–10 orders**.
**Required change.** (i) Re-rate OB-F1/F2/F3 to **high**; guard OB-F1 with
`1 ≤ n`; restate OB-F3's target as the *instantiated* biconditional mentioning
`nthPrime` on both sides. (ii) Add a **fidelity anchor set** whose discriminating
members are the FT-4 **tightness witnesses** — `nthPrime 2 ^ 3 - nthPrime 3 ^ 2
= 2` and `nthPrime 4 ^ 5 - nthPrime 5 ^ 4 = 2166` — which the shifted variant
fails (76 and 132490). This is the one place FT-4's finding becomes a *test*
rather than a warning. (iii) Add **OB-F6b**, the integer sufficient criterion
$1000\,n\,g_n \le 693\lfloor\log_2 p_n\rfloor p_n$ (from (SUF) plus
`Real.log_two_gt_d9`), verified to hold for all $n \le 3\,001\,133$ **except
$n = 2, 4$** — which `decide` discharges in microseconds. One multiplication per
$n$ instead of an exponentiation; $\sim10^6\times$ cheaper at $n = 10^5$. Make it
the engine of W5. (iv) Revise $N_0$ to **$10^4$–$10^5$ (index)**, disambiguate
index-vs-height (currently ambiguous across `#2.2`, `#3.5`, `#5.1`), and state
the resulting ~12-order coverage gap against the cited range so `write-paper`
cannot oversell it. (v) **`evidence-gate` must check `#print axioms`, not merely
the absence of `sorry`** — `native_decide` is unmentioned in the document, is the
only route to a large $N_0$, and is sorry-free *while adding an axiom*.

### A7 — Cut the Kourbatov back-edge; restate FT-6's blame assignment
*Owner:* `source-ledger` (debt #2), `citation-gate`.
**Defect.** `decompose/decompose.md#2.3-C4` calls the Kourbatov agreement *"a
genuine independent confirmation of C2–C4, **not** a citation"*, while
`#4`-FT-6 makes C2–C4 falsifiable **by** that same citation — and the citation is
unclosed Critical debt #2. Those two edges cannot both be sound, and together
they form a closed loop through an unverified node. Independence is also false in
fact: the derivation was performed by an agent already holding the constant.
And the resolution does not reach: the derivation's uncertainty on $c_n$ makes the
third term uncertain by $\pm 0.3\log p \approx \pm 10$ at $p \sim 10^{15}$ — **10–36×
coarser than the additive constant $-1$ it claims to match.**
**Required change.** Delete "genuine independent confirmation"; replace with
*"consistent to the resolution of our $c_n$ bound, which is too coarse to confirm
or refute the additive constant."* Rewrite FT-6: *a material discrepancy is a fact
about the literature or about normalization; it does **not** falsify C2–C4, which
stand on their own proofs.* Mark FT-6 **NOT YET RUN** (it is currently recorded as
both passed and outstanding).

### A8 — Respecify debt #10; correct the "5.5%, not $10^k$" framing
*Owner:* `notebooks`.
**Defect.** *"Measure the growth of $\max R$ vs height with error bars"* is not
well-posed: $\max_{p \le x} R$ is a deterministic step function from a complete
enumeration — known exactly, zero uncertainty — so its "error bars" ask for the
variance of a constant. It is also the least extrapolable statistic available
(non-decreasing by construction; determined by one point out of $\pi(x)$; Gumbel
fluctuation that does not shrink). Relatedly, `#3.2`'s non-monotonicity argument
does not follow: it compares a max-over-range against a single point, and
$\max_{p\le x}R$ is monotone by construction. (Its *numbers* are correct — $R =
0.7878$, shortfall $1.2408$ — and its *conclusion* is defensible on other grounds,
namely that the required threshold $1 - 1/\log p$ rises. Rebuild the argument, do
not withdraw the claim.)
**Required change.** Replace debt #10 with: *fit
$\max_{p\le x} R \approx C\big(1 - 2\log\log x/\log x\big)$ to the maximal-gap
table, report $\hat C$ with a confidence interval from the residuals, and report
the implied crossover height against $1 - 1/\log x$.* This is a real estimator
with real residuals. Preliminary values to hand over: $C = 2e^{-\gamma}
\Rightarrow x \sim 10^{30}$; calibrated to the record locus $\Rightarrow x \sim
10^{23}$; the verified range already **excludes $C \gtrsim 1.2$**; $C = 1$ never
crosses. Then correct `#0.2`'s framing: *"about 5.5%, not a factor of $10^k$"* is
true in the $R$ coordinate and false in the coordinate that governs cost — the
same shortfall is ~6× in rarity and **8–15 orders of magnitude in search height**.
$R$ is a logarithmic coordinate on height.

### A9 — Add the missing tree nodes; separate sociology from allocation
*Owner:* `decompose` amendment; consumed by `proof-attempt__0/1/2`.
**Defect.** The `#2` preamble's *"any complete proof or refutation must pass
through these nodes"* is true only in the **entailment** sense — where it is
automatic and informationless, because OB-A and OB-C are equivalences — and false
in the **method** sense. Three routes land in no node: **X₁** non-constructive
finite existence (between "one exhibited $n$" and "$\limsup R_n > 1$" sits the
entire space of averaging/pigeonhole/dichotomy arguments — the standard shape in
this subfield); **X₂** conditional refutation (OB-D has three conditional slots,
OB-E has zero, unexplained); **X₃** barrier/metamathematics — and X₃ is where a
deliverable `#2.4` *itself commissions* ("make the barrier statement rigorous and
quotable") would live. Additionally OB-D4 and OB-E5 are **the same question** —
the value of $\limsup R_n$ relative to 1 — appearing twice under two names and
therefore nowhere as a single budgeted node. And `#8`'s untiered sociological
premise (*"the community's heuristic expectation is that F is false"*) is followed
by *"therefore"*, while `#3.6`'s budget mass (ranks 1–2) is **branch-neutral**.
**Required change.** Add **OB-E0** (non-constructive $\exists n$), **OB-E6**
(conditional refutation), **OB-G** (barrier/metamathematics, with the acceptance
criterion `#3.6` rank 5 currently lacks). Merge D4 and E5 into one node. Tag
`#2.4`'s universal negative (*"no technique in analytic number theory produces a
polylogarithmic gap bound at all"*) as **[L3]** — it is the strongest logical form
in the document, carries no tier, and solely supports "categorically blocked".
Tag the sociological premise [L3] and add one sentence: *the `#3.6` allocation is
invariant under flipping this expectation — but `#2.4`'s prohibition and debt
#11's mandate are not.* (The prior routes **instructions**, not budget rows; that
is the worse of the two, because a table row is visible and re-rankable while an
obeyed prohibition leaves no trace.)

### A10 — Add the two missing tiers, and the two-sided test
*Owner:* `decompose` amendment; new test assigned to `notebooks`.
**Defect.** Nothing in the document could detect that **F is true**, or that
`#8`'s strategic conclusions are **wrong**. Every test is structured to detect a
counterexample. `#0.1`'s tier table has no slot for **corroborated — survived $N$
severe tests**, which is why the leg's own strongest empirical output is filed as
a "sanity floor"; and no slot for **asserted about the computation but not
produced by it**, which is exactly where all three A1 errors live.
**Required change.** Add both tiers. Add **FT-8**, a two-sided calibration test:
compute the Cramér/Granville-predicted count of Firoozbakht counterexamples below
the verified frontier and compare with the observed zero (naive Cramér predicts
~11; there are none — a discrepancy that is *not* decisive, since naive
independence is a poor guide in the extreme-gap regime, but which the document is
structurally unable to notice). **Pre-register, before `notebooks` runs, the
numerical outcome that would demote `#0.2`'s "the refutation branch is the live
branch"** — otherwise the test becomes post-hoc.

### A11 — Minor, but they carry budget lines
- `#3.3`'s reason for zeroing the contradiction route ("a universally quantified
  inequality with no auxiliary structure") is a **false general principle** —
  minimality gives $p_n < p_m^{n/m}$. The *verdict* is right for an unstated
  reason (that bound is exponential in $n$, hence already free from Bertrand).
  Record the reason; downgrade "Zero — do not spend" to "Low — reason recorded".
  A recorded reason is reusable; a bare prohibition is not.
- `#2.1` says OB-A needs no revisit and is "fully formalizable today with
  certainty"; `#6.4` says *"do not trust the Mathlib lemma names"* and debt #7 is
  Critical. A leg obeying `#2.1` skips a Critical debt. Strike the sentence.
- OB-B is costed **"routine"** in `#2.2` and **"high — the main risk item"** in
  `#6.3`. One obligation, two irreconcilable costs. Single cost = high; and
  amend `#1.2`'s design note, which claims F2 makes finite verification a "pure
  `decide` computation" — contradicted by `#6.1`'s own caveat that `Nat.nth` is
  noncomputable.
- `#3.2` quotes a maximal gap at $p \approx 1.836\times10^{19}$, above the
  $4\times10^{18}$ frontier asserted in `#2.2`/`#5.1`/debt #2. State *table
  extent* and *verification extent* as two separate quantities.
- Debt #6 (Nicholson/Farhadian) is filed "Low (unused)"; "unused" is an artifact
  of the tree having no strengthening node. If the chain is as recalled, a
  strengthening's slack fails **strictly before** F's, giving `notebooks` a
  second, more sensitive trend line on the *same sieve pass* at zero marginal
  cost. Raise to Medium, co-own with `notebooks`.
- `verify_small_range.py`'s docstring claims it "reproduces every `[COMPUTED]`
  number in decompose.md". It does not (the $\min_{n>200} S_n$ figure, and four
  numbers at loci above the sieve). Retract or make it true.

---

## Weakest branches, named

Asked directly by the commissioning brief. In order:

1. **FT-7** (`decompose/decompose.md#4`, debt #11) — invalid inference, mandated
   first and Critical, silent-terminal false negatives. *The weakest branch in
   the artifact.*
2. **`#5.4`'s float-rigour argument and its downstream instruction** — wrong by
   $3.3\times10^3$ in the unsafe direction; and it very nearly propagated with
   **two adversarial endorsements attached** (see the D1 note below).
3. **`#2.3-C4`** — the single L2 node under the entire headline, with a stated
   constant its own citation does not support, and a back-edge from an unclosed
   debt.
4. **`#2.5`-E1 / FT-2** — the screens that gate the refutation search, both
   stated against the wrong threshold, with a 1.10-gap-unit discard window
   nobody has analysed.
5. **`#6.3`'s risk ranking** — ranks effort over failure mode, so the three
   silent-failure obligations are rated low/low/medium and the loud one is rated
   high.

## What the panel did **not** break — do not re-litigate downstream

The F1–F5 equivalences (pointwise, no hidden asymptotics); C1's sandwich and the
substitution $x = g_n/p_n$; (SUF) and (REF) individually, valid for every
$n \ge 1$ with no side conditions; C3's band dismissal **at $p \gtrsim 10^{12}$**;
E3's antitonicity of $S_n$ in $n$; the script's indexing (no off-by-one); and
every script-produced `[COMPUTED]` number, re-derived in exact decimal. Five
adversarial reviewers converging on this same list by different routes is the
control that makes the defect list above credible rather than reflexive.

**One caveat carried forward on E3.** Its structural half is L0 and stands. But
"essentially zero cost / no exact prime counting required" holds only for a
witness clearing threshold by more than the $\pi(x)$ bound's relative deficit
($\sim10^{-4}$). The **first** counterexample, by definition of a first crossing,
is the one with the smallest excess — so E3 is weakest exactly where it will be
used. Say so where the collapse is claimed.

---

## Note for the meta-record — the D1 event

Two of five panelists read `#5.4`'s error analysis and declared it **sound**. Two
measured it and found it wrong by three orders of magnitude, in the unsafe
direction. The document's argument — *"$n\log(1+g_n/p_n) = \log p_n - S_n \le 18$,
so it does not grow with $n$"* — is true of the **value** and silent about the
**error**, and it is persuasive enough that two independent adversarial auditors
reproduced it verbatim. This is the class of defect `evidence-gate` exists to
catch, and it nearly propagated with adversarial endorsements attached. The
operational lesson for the polymer: **a passage that reasons *about* a computation
rather than *from* it should be treated as unverified until the computation prints
the number the prose asserts.**

---

## Proposed follow-up molecules — RECOMMENDATIONS ONLY, NOT NUCLEATED

⚠ This leg is a Tier-0 leaf. **No molecules were created.** The downstream DAG
honours or discards the following at its discretion.

| # | Topic | Kind | Blocked-by | Temp | Briefing seed |
|---|---|---|---|---|---|
| R1 | Amend `decompose.md` §5.4 + `verify_small_range.py`: `log1p`, runtime `SAFETY_MARGIN`, print $\min_{n>\texttt{EXACT\_N}}S_n$, $p<2^{53}$ precondition, rewritten extension instruction | task | — | **hot** | A1 above. Blocks all three `notebooks` legs; the current instruction produces confident false results above $p\sim10^{16}$. |
| R2 | Replace FT-7 with FT-7′ (non-terminal triage) + FT-7″ (relativization barrier); amend debt #11 | task | — | **hot** | A2 above. Only defect whose failure mode silently destroys a correct proof, and it is mandated first. |
| R3 | Close debt #4 with the three-term Dusart bound; retier §0.2/§8 per the L0/L2 split; restate the shortfall as $1.054\pm0.001$ | task | — | **hot** | A3. Natural first item for `source-ledger`, ahead of debts #1/#2 — it is the only L2 node under the headline. |
| R4 | Amend §2.3-C3 / §0.2 with the dead band $B_n$, $W_n$, the $n\ge484$ threshold, the $n=2,4$ exceptions, and the $n=61$ sign flip | task | R3 | **hot** | A4. The panel's strongest finding, four independent confirmations. |
| R5 | Restate E1 as a screen with tolerance; make E4 sole adjudicator; restate FT-2 in (REF) form; add the false-negative test | task | R4 | warm | A5. |
| R6 | Lean: invert the OB-F risk ranking, add the fidelity anchor set (FT-4 tightness witnesses), add OB-F6b integer criterion, revise $N_0$ to $10^4$–$10^5$, extend the firewall to `#print axioms` | task | R4 | warm | A6. Largest single change; the integer criterion is a genuine cost win, not just a fix. |
| R7 | Cut the Kourbatov back-edge (FT-6), mark FT-6 NOT YET RUN, downgrade "independent confirmation" | task | R3 | warm | A7. Cheap edit; stops an L2 recollection from being licensed to overturn a verified L0 result. |
| R8 | Respecify debt #10 as a parametric tail fit; correct the "5.5% not $10^k$" framing | task | — | warm | A8. Converts an ill-posed High-priority deliverable into a falsifiable, quotable one. |
| R9 | Add OB-E0 / OB-E6 / OB-G to the tree; merge D4 and E5; tag §2.4's universal negative [L3]; separate sociology from allocation in §8 | task | — | warm | A9. |
| R10 | Add the two missing tiers (corroborated; asserted-about-not-produced-by) and FT-8, the pre-registered two-sided calibration test | task | R8 | warm | A10. The only proposed test that can come out *for* F or *against* the strategic conclusion. |
| R11 | Sweep the minor items (§3.3 reason, §2.1 sentence, OB-B double cost, frontier ambiguity, debt #6 rerouting, docstring retraction) | task | R1–R4 | cold | A11. Housekeeping; each carries a budget line, none blocks. |

**Sequencing recommendation for `source-ledger`.** Take **R3 first** — debt #4 is
the sole external input to the headline and is currently rated only *High*.
Debts #1 (record locus) and #2 (Kourbatov) remain Critical and follow. Note that
the moderator's cross-check found the headline shortfall **insensitive** to the
$c_n$ constant at three significant figures ($1.049$–$1.058$ across
$c_n \in [0.9, 1.2]$), so debt #4 is critical for the *audit trail*, not for the
*number* — worth knowing before budgeting it.

---

*Emitted by the `frame-deliberation` leg (node). The conjecture
is **open**; nothing here proves or refutes it, and the panel found no evidence
bearing on its truth in either direction. Recommend only — no molecules
nucleated.*
