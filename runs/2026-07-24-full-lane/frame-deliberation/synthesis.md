# Synthesis — panel stress-test of the Firoozbakht attack-surface decomposition

**Artifact under review**: `decompose/decompose.md` + `decompose/verify_small_range.py`
**Panel**: wheeler · feynman · godel · popper · knuth (5, auto-selected — see `frame.md` §3)
**Moderator's independent cross-check**: `moderator-crosscheck.md` (run *before* reading any response)
**Date**: 2026-07-24

---

## 0. The one-paragraph verdict

The decomposition's **mathematics is sound and the panel confirmed it four ways
independently**: the five equivalences F1–F5, the (SUF)/(REF) criteria, the C3
sharpness *at the frontier*, the E3 antitonicity collapse, and every
script-produced `[COMPUTED]` number all survived attack. What did not survive is
the layer *above* the mathematics — the **quantifiers, the tiers, the tests, and
the routing**. Five independent lines of attack converged on one structural
disease: **a statement proved about real numbers in an asymptotic regime is used
as a statement about integers over all $n \ge 1$**, and every headline claim,
falsifiability test and budget allocation in the document inherits that slippage.
The decomposition is *repairable and worth repairing* — none of the findings
require re-deriving anything — but four of them would silently corrupt downstream
legs if compute were spent first.

---

## 1. Frame-question coverage table (mandatory)

Every Qn from `frame.md` §2, accounted for exactly once.

| Q frame | Treated | Substituted | Declined-with-rationale | Silent |
|---------|:-------:|:-----------:|:-----------------------:|:------:|
| **Q1** Completeness of the obligation tree | ✅ wheeler | | | |
| **Q2** Non-circularity / tier propagation | ✅ godel (+ knuth, wheeler secondary) | | | |
| **Q3** Teeth of FT-1…FT-4 against F | ✅ popper | | | |
| **Q4** Teeth of FT-5…FT-7; FT-7 completeness | ✅ popper (+ wheeler, independently) | | | |
| **Q5** Where the sharpness claim breaks | ✅ feynman, knuth (+ popper, godel) | | | |
| **Q6** Quiet assumptions (a–d) | ✅ wheeler, feynman, popper, knuth | | | |
| **Q7** The dead band $T_n < g_n < T_n(1+g_n/p_n)$ | ✅ feynman, knuth | | | |
| **Q8** Lean statement fidelity / OB-F risk | ✅ godel, knuth | | | |
| **Q9** Weakest branches, ranked | ✅ all five | | | |

**Silent: none. Substituted: none.** Every persona cleared the falsifier written
into its own prompt (`frame.md` §4):

| Persona | Predicted substitution | Falsifier | Outcome |
|---|---|---|---|
| wheeler | "is this the right question about the primes?" / vocabulary audit | must name a route landing in no node, or prove exhaustiveness | **Cleared** — named X₁, X₂, X₃ |
| feynman | readability / cargo-cult audit; recompute the small-$n$ table | must evaluate the band arithmetically at a concrete $(n,p,g)$ | **Cleared** — $n{=}2$, $n{=}4$, and the record locus, in full |
| godel | general incompleteness meditation (independence from ZFC) | central claim must be a named edge or named Lean defect | **Cleared** — K1/K2, C-1…C-6; ZFC explicitly parked as out-of-scope in two lines, as instructed |
| popper | praising the L0/L1/L2/L3 tier system | must deliver a verdict for all seven FTs | **Cleared** — seven verdicts, and declined to praise the tiers |
| knuth | reviewing the sieve's coding style | must address $\lvert\Delta S_n\rvert$ and `EXACT_N = 200` | **Cleared** — derived the bound from first principles, measured it against 60-digit decimal |

### 1.1 The cross-panel groupthink check — inverted, and handled honestly

`frame.md` §7 pre-registered: *if ≥4 of 5 conclude "sound, proceed", report that
as a groupthink signal.* **Zero of five did.** The opposite happened — all five
returned substantial defect lists. That is itself worth interrogating, because
the shared preamble *instructed* them to attack, and five agents told to find
problems will find problems.

The control against that is the **convergent list of things each panelist
attacked and explicitly declared sound**, unprompted and overlapping:

| Claim | Confirmed sound by |
|---|---|
| F1⟺F2⟺F3⟺F4⟺F5, pointwise, no hidden asymptotics | wheeler, feynman, godel |
| (SUF) and (REF) proofs, valid for every $n\ge1$ | wheeler, feynman, godel, popper |
| C1 sandwich $x/(1+x) < \log(1+x) < x$ and the substitution $x = g_n/p_n$ | feynman, godel |
| C3's band dismissal **at $p \gtrsim 10^{12}$** | feynman (band $8.4\times10^{-10}$), moderator (same) |
| E3 antitonicity of $S_n$ in $n$ | wheeler, feynman, godel |
| $1.055$, $1.241$, the §5.2 shortfall column, §5.1's counts, FT-4's ratios | knuth (exact decimal), feynman, godel, moderator |
| Script indexing ($n = \text{idx}+1$, $p_1 = 2$, no off-by-one) | knuth |
| FT-2's restriction to *maximal* gaps loses nothing | wheeler (proved it) |

Five adversarial agents converging on the same list of *sound* results, by
different routes, is the strongest available evidence that the attack was
calibrated rather than reflexive. **The mathematics is not in question. The
scaffolding is.**

---

## 2. Per-persona summaries

### wheeler — the tree is a work plan with a proof-shaped diagram over it
The cover is exhaustive only in the **entailment** sense (any proof of F entails
OB-A and OB-C, because those are *equivalences*) — which is automatic and carries
zero routing information — and it is **not** exhaustive in the **method** sense.
Three named routes land in no node: **X₁** non-constructive finite existence
(prove $\exists n: S_n\le0$ without a witness and without $\limsup R_n>1$ — the
standard Erdős-school shape, and precisely what "the live obligation is E1–E4:
search" forecloses); **X₂** conditional refutation (OB-D has three conditional
slots, OB-E has zero, with no justification); **X₃** barrier/metamathematics —
and X₃ is the home of a deliverable §2.4 *itself commissions* ("making the
barrier statement rigorous and quotable") with no node to attach it to. Structural
diagnosis: the label "obligation" runs three incompatible node types (logical
obligation, this polymer's engineering task, literature-survey verdict), and the
proof is that OB-B carries two irreconcilable cost estimates in one document
("routine" §2.2 vs "high — the main risk item" §6.3). The D/E split is drawn by
*outcome*, so D4 and E5 are the same question — the value of $\limsup R_n$ vs 1 —
appearing twice under two names and therefore **nowhere as a single budgeted node**.

### feynman — a real-valued error is being used as an integer-valued decision
Re-derived the sandwich from scratch and produced the number the document never
writes: the band where **neither** criterion fires is
$B_n = \big(T_n,\ T_n/(1-T_n/p_n)\big)$ with width $W_n = T_n^2/(p_n-T_n)$. Since
$g_n$ is an **integer**, sharpness means $B_n \cap \mathbb{Z} = \emptyset$, i.e.
$T_n^2 + T_n < p_n$ — which holds for $n \ge 484$ ($p_n \ge 3461$) and **fails for
every $n \le 483$**. And separately: §0.2's *displayed* formula contains a second
approximation, $p_n/n \to \log p_n - 1$, whose error is $9\times10^{-4}$ —
**twelve orders of magnitude larger** than the $5\times10^{-16}$ sharpness the
sentence advertises, and worth $+1.10$ *gap units* at the record locus. Also found
that §3.2's "important correction to the naive reading" is an invalid argument:
$\max_{p\le x}R$ is monotone non-decreasing by construction, and §3.2 compares a
max-over-range against a single point.

### godel — the tiers have no propagation rule, and the Lean risk ranking is inverted
§0.1 defines tiers for *statements* and never states a rule for *inference*, so
composite claims silently inherit the tier of their **strongest** premise. Result:
§0.2's headline, both its consequences, §3.2 row 3 and §8 items 2–3 are tagged
L0/[COMPUTED] while routing through C4, an L2 node (debt #4). Worse, **C4's L2
attribution is factually wrong**: the two-term bound it cites pins $c_n$ only to
$(0.075, 1.104)$, not the claimed $(0.9, 1.2)$ — so on the document's own
authority the headline "5.5%" is pinned only to **5.2%–8.4%**. (Constructive fix
supplied: the three-term Dusart bound pins $c_n\in(1.030,1.034)$.) Named the
**Kourbatov loop**: §2.3-C4 calls the agreement "genuine independent confirmation,
*not* a citation" while FT-6 makes C2–C4 falsifiable *by* that same citation —
and the citation is unclosed Critical debt #2. On Lean: §6.3 ranks by *effort*,
not by *failure mode*. OB-F8 fails **loudly**; OB-F1/F2/F3 fail **silently** —
and he exhibits the wrong theorem: `Nat.nth Nat.Prime (n+1) ^ n < Nat.nth
Nat.Prime n ^ (n+1)` compiles, is true if F is true, is **strictly weaker than
F**, and passes both of OB-F1's proposed `example`s.

### popper — one tooth against F, not four; and FT-7 is a kill switch that cannot itself be killed
Seven verdicts delivered. Only **FT-1** is `refutes-F`. **FT-2** is
`refutes-nothing` — it triggers on the **(SUF)** boundary, not (REF), and has a
demonstrated 4/4 false-positive rate on its first four inputs; the script's
`if pn > 1000` guard filters out exactly the regime where FT-2 lies. **FT-3** is
`refutes-nothing` — a valid entailment with no executable protocol; it imposes
zero risk on F, so F's survival of it corroborates nothing, yet its *Status* line
is what licenses §8-6's strategic prior. **FT-4** is
`refutes-the-decomposition-only` and is misfiled. **FT-7 is not complete**: it
jumps from *semantic entailment* to *syntactic occurrence*, and four proof shapes
evade it (contradiction-without-exhibition, ineffective/non-explicit
establishment, via-a-stronger-intermediate, induction/local transfer). Since debt
#11 makes it **Critical and first**, its false negatives are silent and terminal.
And FT-7 is irrefutable-by-construction: the only thing that could refute it is a
correct proof of F, which FT-7 would discard before anyone read it. The
falsifiability is **one-sided**: nothing in the document could detect that F is
true or that §8-6 is wrong.

### knuth — the most careful passage in the document has the highest error density
Derived the true float error bound from first principles:
$\lvert\Delta S_n\rvert \le n u + 6u\log p_n$ — **linear in $n$**, exactly the
growth §5.4 argues is absent. The docstring's argument bounds the *magnitude* of
$n\log(1+g/p)$ and then applies a *relative* error model; but $n$ is an exact
integer, so it acts as a pure **amplifier of an absolute error committed one step
earlier** by `math.log(pn1/pn)`. Measured against 60-digit decimal, the bound is
**tight to 3%**. Three §5.4 claims are wrong, all in the unsafe direction: the
$10^{-13}$ bound (off by $3.3\times10^3$), "$\min_{n>200}S_n > 3.5$" (truth:
**1.7008 at $n=217$**), and `SAFETY_MARGIN = 1e-11` — which is **0.033× the error
it guards against**, a silent-failure window. Root cause named precisely: the
script never *prints* $\min_{n>\texttt{EXACT\_N}} S_n$, so the prose supplied it
from a neighbouring statistic. Two constructive contributions that are worth more
than the criticism: (i) `math.log1p((pn1-pn)/pn)` restores $n$-independence and
buys back five orders of magnitude in one line; (ii) an **integer** sufficient
criterion $1000\,n\,g_n \le 693\lfloor\log_2 p_n\rfloor p_n$, verified to hold for
all $n \le 3\,001\,133$ **except $n = 2, 4$**, which replaces an $O(n^{1.6})$
bignum exponentiation with one multiplication. Consequently §3.5-W5's
"$N_0 = 10^6$–$10^8$" is **fantasy by 4–10 orders**; realistic is $10^4$–$10^5$.

---

## 3. Convergences — where the panel agrees, and how independently

### C1. The (SUF) criterion is undefined at exactly the two tightest known cases — **4 independent finders**

| Finder | Route | Result |
|---|---|---|
| feynman | solved the (REF) inequality for $g$, derived $B_n$ and $W_n$ | $g_2 = 2 \in (1.648, 2.747)$; $g_4 = 4 \in (3.405, 5.351)$; threshold $n \ge 484$ |
| popper | evaluated FT-2's own trigger on the first nine indices | FT-2 fires — falsely — at $n = 1,2,3,4$; band 67% wide |
| knuth | tested his integer criterion over the full sieve | holds everywhere **except $n = 2, 4$** |
| moderator | exhaustive band search over $p_n < 2\times10^6$ | the band is non-empty at exactly $n = 2$ and $n = 4$ |

Four different instruments, four different reasons to look, one answer. **This is
the panel's strongest result.** The document's §0.2 calls the criterion "*the*
criterion … sharp to sixteen decimal places, **unconditionally**", and its own
§4-FT-4 identifies $n=2,4$ as the tightest cases in the whole computed range.
Those two sentences sit 250 lines apart and collide. F *is* true at $n=2,4$ — by
the exact predicate ($25<27$, $14641<16807$) — but **the criterion does not say
so.** Nothing in the mathematics breaks; the *quantifier* breaks.

### C2. Two approximations of wildly different size are conflated in the headline — **4 of 5**
§0.2/§8-2's "sharp to sixteen digits" belongs to the C3 sandwich
($1+g_n/p_n \approx 1 + 4.6\times10^{-16}$). The *displayed formula* also contains
$p_n/n \to \log p_n - 1$, whose error is $\approx 9\times10^{-4}$ — **twelve to
thirteen orders larger**, and $+1.10$ gap units at the record locus. Found by
feynman (measured at three loci), knuth ("§8 conflates the two; §2.3-C4's own
table refutes §8's sentence"), godel (via the $c_n$ interval), popper (the ~0.4%
*sign-flipping* discrepancy, a tenth of the 5.5% margin and able to point either
way). Corrected shortfall at the record locus, converged three ways: **1.0543**,
not 1.0552.

### C3. FT-7 is invalid as a read-free filter, and it is mandated first — **2 of 5, from opposite ends**
wheeler reached it structurally (the §2 preamble equivocates between *entailment*
and *method*; FT-7 is where the swap happens). popper reached it
epistemologically (semantic entailment ≠ syntactic occurrence; four evading proof
shapes; FT-7 is unfalsifiable by construction). Neither saw the other's work.
**This is the single most dangerous item in the artifact**: every other defect is
recoverable, while a false negative here silently destroys a correct proof, and
debt #11 puts it first in the review pipeline with Critical priority. Both
supplied compatible repairs — popper's **FT-7′** (extraction obligation, three
outcomes, outcome (c) non-terminal) plus **FT-7″** (relativization/prime-blindness
barrier) is the constructive form; wheeler's node **OB-G** is where the barrier
theorem it needs would live.

### C4. §3.6's ranking is a smuggled prior wearing the costume of a derivation — **2 of 5**
Both noted that the "Closes F?" column reads *No* six times, carries zero
information, and orders nothing — so the entire ranking rests on "Artifact
value", a column with no definition, units, or derivation. feynman named the
tell: the prior is in the header (*"Formal backend requested: Lean 4/Mathlib"*),
and the ranking optimises for **what the pipeline can certify**, not what anyone
would learn — the one claim of novelty in the document (§3.4) is ranked 4th,
below re-formalising a two-line corollary.

### C5. Tier discipline has no propagation rule, and `[COMPUTED]` is laundering L2 inputs — **3 of 5**
godel: no inference rule in §0.1, so composites inherit the *strongest* premise's
tier; four `[COMPUTED]` tags are arithmetic on [L2/L3] recollection and are not
producible by the emitted script. knuth: independently confirmed the same four,
and added a category the tier table lacks — *asserted about the computation but
not produced by it*, which is exactly where all three §5.4 errors live. popper:
the tier table has no slot for **corroborated (survived N severe tests)**, which
is why the leg's own strongest empirical output is filed as a "sanity floor".

### C6. Debt #10 ("measure max R with error bars", High) is not well-posed — **2 of 5**
knuth: $\max_{p\le x}R$ is a deterministic step function from a complete
enumeration — known exactly, zero uncertainty; asking for its error bars asks for
the variance of a constant. A running maximum is also the least extrapolable
statistic available (non-decreasing by construction; one point out of $\pi(x)$;
Gumbel fluctuation that does not shrink). feynman: independently, §3.2's
motivating argument is invalid for the same underlying reason. **knuth supplied
the well-posed replacement**: fit $\max_{p\le x}R \approx C(1-2\log\log x/\log x)$,
report $\hat C$ with a CI from the residuals, report the implied crossover height.
Preliminary: $C = 2e^{-\gamma} \Rightarrow x\sim10^{30}$; calibrated to the Nyman
record $\Rightarrow x\sim10^{23}$; the verified range already **excludes
$C \gtrsim 1.2$**; $C = 1$ never crosses.

### C7. "5.5%, not a factor of $10^k$" is true in the wrong coordinate — **2 of 5**
popper: $R$'s density is exponential, so a 5.5% ratio improvement is a **~6×
rarity** improvement. knuth: converted through the tail model, closing that
shortfall means advancing the search from $10^{15}$ to $10^{23}$–$10^{30}$ —
**8 to 15 orders of magnitude, i.e. $10^k$ with $k \approx 8$–$15$.** $R$ is a
logarithmic coordinate on height; a small shortfall in it is not a small
distance. §0.2 exists precisely to stop downstream rediscovery — as written it
will instead propagate a flattering coordinate choice.

### C8. Lean: the risk ranking is inverted, and $N_0$ is fantasy — **2 of 5, complementary not redundant**
godel attacked **fidelity** (silent vs loud failure; the wrong-theorem index
shift; the unanchored generic bridge; the `≤`-into-degenerate-RHS slip closed by
`le_refl`). knuth attacked **cost** (F2 exponentiation is $O(n^{1.6})$ per $n$, so
$N_0 = 10^6$ is ~58 days *in Python* before a single primality certificate;
`native_decide` is the only route to a large $N_0$ and it is `sorry`-free **while
adding an axiom**, so §6.5's firewall must check `#print axioms`, not just
`sorry`). The two compose into one repair: **godel's fidelity anchor set is
discharged by knuth's integer criterion**, and both agree the anchors must land
before any W-lemma is accepted as evidence.

---

## 4. Divergences — and how they resolve

### D1. §5.4's float-rigour argument: **two panelists declared it sound without measuring it; two measured it and found it wrong**

| Panelist | Verdict on §5.4 | Basis |
|---|---|---|
| wheeler | "sound; the $n$-fold amplification is bounded, exactly as claimed" | read the argument |
| godel | non-finding (iii): "§5.4's float-rigour argument holds" | read the argument |
| feynman | **wrong** — measured error $2\times10^{-10}$ at $n=2.85\times10^6$, grows as $6\times10^{-17}n$ | 60-digit `Decimal` comparison |
| knuth | **wrong** — derived $nu + 6u\log p_n$; measured/derived ratio **0.97** | first-principles derivation + 4000-point measurement |

**Resolution: the measurements win, decisively.** wheeler and godel both
reproduced the document's own reasoning — *"$n\log(1+g_n/p_n) = \log p_n - S_n \le
18$, so it does not grow"* — which is true of the **value** and says nothing about
the **error**, because $n$ is an exact integer amplifying an absolute error
committed by the division. This is the most instructive event in the whole
deliberation: **the document's argument is persuasive enough that two independent
adversarial auditors accepted it verbatim**, and it took someone who ran the
numbers to see it fail. Recorded as a first-class finding, not a bookkeeping note:
*an error analysis that survives reading and fails measurement is exactly the
class of defect this polymer's `evidence-gate` exists to catch, and it very nearly
propagated with two adversarial endorsements attached.*

### D2. Does §8-6's sociological prior route anything? **wheeler says no, popper says it routes seven legs**
wheeler traced §3.6 and found ranks 1–2 — the budget *mass* — are
**branch-neutral** (they are OB-A + OB-C + OB-F work that is identical whether F
is true or false); the prior tilts only ranks 3–5, the three lowest-value items.
Conclusion: the headline "the refutation branch is the live branch" is
**decorative with respect to the allocation it exists to justify**. popper traced
a different channel and found the prior routes §2.4's design instruction (*"Do
**not** budget attempts at closing D1–D4"*), FT-7's Critical-and-first mandate,
and §3.6's rank-6 zero-budget line.

**Resolution: both are right about different objects, and together they are
sharper than either.** The prior routes **instructions and prohibitions**, not
budget *rows*. That is the worse of the two, because a table row is visible and
re-rankable while a prohibition is invisible once obeyed — no leg reports the
work it was told not to do. **Required change: §8-6 must carry both statements —
that the §3.6 allocation is invariant under flipping the expectation, *and* that
§2.4's and debt #11's prohibitions are not.**

### D3. §3.2's non-monotonicity claim: knuth "real and correctly computed", feynman "invalid argument"
**Not an actual conflict.** knuth verified the *arithmetic* of the two rows
($R = 0.7878$, shortfall $1.2408$ at the largest known gap — both confirmed).
feynman attacked the *inference*: $\max_{p\le x}R$ is monotone non-decreasing by
construction, so comparing a max-over-range ($0.9206$ at $p\sim1.7\times10^{15}$)
against a single point ($0.7878$ at $p\sim1.8\times10^{19}$) cannot show
non-monotonicity of the shortfall. **Both hold. The numbers are right; the
conclusion drawn from them does not follow.** The conclusion happens to be
defensible on other grounds (the required threshold $1-1/\log p$ rises with
height) — so the fix is to rebuild the argument, not to withdraw the claim.

### D4. The range of $c_n$: feynman $(0.398, 1.112)$, godel $(0.075, 1.104)$, godel-with-Dusart-3-term $(1.030, 1.034)$
**Not a conflict — three different questions.** feynman measured the *empirical*
range of $c_n = \log p_n - p_n/n$ across $n \ge 6$. godel computed what the
*cited two-term bound* permits at the record locus. godel's third figure is what
the *three-term* Dusart bound permits there. All three are correct and all three
indict §2.3-C4's stated $(0.9, 1.2)$, which matches none of them: it is an eyeball
fit to a table spanning $n \le 3\times10^6$, applied nine orders of magnitude
higher. feynman adds the operationally sharpest consequence: **$c_n - 1$ changes
sign at $n = 61$**, so any downstream leg using the proxy as a *one-sided* bound
is wrong on one side of 61.

### D5. Is the decomposition's core salvageable? **Unanimous yes — and this is a convergence worth naming as a divergence from the expected outcome**
No panelist proposed re-deriving C1–C4, the equivalences, or E3. Every ranked
list is a list of **restatements, retiers, and added guards** — not
reconstructions. godel supplies the cleanest form: split §0.2 into
**2a [L0]** (F ⟺ $g_n \lessgtr T_n = (p_n/n)\log p_n$, sharp to $1+O((\log p)^2/p)$
— survives audit intact) and **2b [L2, debt #4]** ($T_n \approx (\log p_n)^2 -
\log p_n$). The document's real result is 2a; almost every defect found by the
panel lives in the slide from 2a to 2b and in the claim that 2b inherits 2a's
tier.

---

## 5. Surprising insights the frame did not anticipate

1. **The dead band is a small-$n$ phenomenon that lands exactly on the tightest
   cases.** The frame asked whether the band was "empty, negligible, or where a
   refutation would land" (Q7). The answer is none of the three: it is empty at
   the frontier (moderator: width $8.4\times10^{-10}$), and non-empty *only* at
   $n = 2, 4$ — where the conjecture is tightest and where nobody was looking for
   a criterion failure.
2. **The pipeline's own screen would discard a genuine counterexample.**
   feynman: OB-E1 is stated in (REF) form, so a band candidate fails the screen
   and **never reaches E4**, the only obligation carrying the exact predicate.
   The operationally relevant band is not the sandwich's $8.4\times10^{-10}$ but
   the C4 proxy's **1.10 gap units** — $1.3\times10^9$ times wider, and analysed
   nowhere.
3. **The refutation heuristic's most direct prediction is already at odds with
   the data.** popper: under naive Cramér, ~11 Firoozbakht counterexamples should
   exist below $4\times10^{18}$; there are zero. He is explicit that this settles
   nothing (naive independence is a poor guide in the extreme-gap regime — which
   is why Granville's refinement exists). The finding is that the document's
   strategic prior rests on a model it never calibrates, and the calibration is
   half a page of arithmetic.
4. **`native_decide` is the hole in the LLM firewall, and it is unmentioned.**
   knuth: it is `sorry`-free *and* axiom-bearing, and it is the only route to a
   large $N_0$ — so the pressure of §3.5-W5's stated $10^6$–$10^8$ target points
   `lean-skeleton` straight at it. §6.5's acceptance criterion ("`sorry`-free")
   would pass it.
5. **The two hardest obligations are the same obligation.** wheeler: OB-D4 asks
   whether $\limsup R_n \le 1$; OB-E5 asks whether $\limsup R_n > 1$. §3.4 says so
   in the document's own words ("the **same** obstruction … from the other side").
   The crux of the entire problem appears twice under two names and therefore
   nowhere as a single node with a single budget.
6. **The most self-critical passage has the highest error density.** knuth's
   closing observation: §5.4 names its own error model, quantifies its own
   headroom, and instructs downstream legs to inherit its discipline — and three
   of its four quantitative claims are wrong, because it reasons *about* the
   computation instead of *from* it. The number it needed
   ($\min_{n>200}S_n$) is the one number the script never prints.

---

## 6. Decision-relevant tension points

**T1 — Two findings block, in the strict sense that spending compute first
destroys the compute.** (i) §5.4's extension instruction, which will make
`notebooks` produce confident false results at $p \gtrsim 10^{16}$ ($nu = 47$ vs
$S = 8.6$ at the largest known gap; and $g/p < u$ there, so the gap is invisible
to the representation). (ii) FT-7 as a Critical-and-first terminal filter, which
will silently discard correct proofs in `skeptic`/`red-team-corpus`. Everything
else can be fixed after the fact.

**T2 — The headline's tier must drop, and its number survives.** Every route
(feynman, godel, knuth, moderator) converges: the "5.5%" arithmetic is right
(corrected: 1.0543), and it is **not L0** — it traverses debt #4, and the cited
bound does not support the stated $c_n$. The fix is a retier plus an error bar,
not a re-derivation. Whether `source-ledger` should close debt #4 with the
three-term Dusart bound *before* touching debts #1/#2 is the one genuine
sequencing call this leg hands upward.

**T3 — The panel found nothing that changes the conjecture's status.** F remains
open, neither proved nor refuted here, and no panelist claimed otherwise. Two
independently strengthened the case that the *refutation-by-search* branch is
further away than §0.2 suggests (popper: ~6× in rarity; knuth: 8–15 orders in
height) — which **supports** §3.2's cost verdict while **undercutting** §0.2's
framing. Downstream must not read this as evidence about F's truth in either
direction; it is evidence about the cost of one search.

---

*Emitted by the `frame-deliberation` leg. Next: `outcomes.md` — the recommendation
to `source-ledger` and the downstream DAG. This leg recommends; it does not
nucleate.*
