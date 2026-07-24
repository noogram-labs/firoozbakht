# Frame — stress-test of the Firoozbakht attack-surface decomposition

**Leg**: `frame-deliberation` (node 2 of the `math-attack` polymer)
**Commissioned by**: node (leg `decompose`, crew role `concept-writer`)
**Artifact under review**: `decompose/decompose.md` + `decompose/verify_small_range.py`
(paths relative to the spore run dir node/`)
**Date**: 2026-07-24

---

## 1. The commissioning decision

The parent leg produced a decomposition of Firoozbakht's conjecture

$$p_{n+1}^{1/(n+1)} < p_n^{1/n}\quad\text{for all } n\ge 1
\qquad\Longleftrightarrow\qquad
(p_n)^{1/n}\ \text{strictly decreasing}$$

into a proof-obligation tree (OB-A … OB-F), a strategy ranking (§3.6), and seven
falsifiability tests (FT-1 … FT-7). It draws a **strategic conclusion that
allocates all downstream compute in the polymer**:

> proof branch = categorically blocked, budget it as *barrier-mapping only*;
> refutation branch = live but out of reach, budget `notebooks` at *measurement*
> not *search*; the shippable deliverable is W1/W2/W3/W6 in Lean + W5 to a
> defensible $N_0$.

**The decision the parent is trying to make** is therefore not "is Firoozbakht
true?" — it is **"is this decomposition load-bearing enough that seven downstream
legs (`source-ledger`, `lean-skeleton`, `lean-probe`, 3× `proof-attempt`,
3× `notebooks`, `red-team-corpus`) should be routed by it?"**

The conjecture must be **PROVEN or REFUTED, not assumed**, and this leg runs
*before* any compute is spent. So the panel's job is adversarial: find where the
decomposition would send the polymer down a corridor that cannot produce the
artifact it promises — or, worse, where it would let a wrong result look right.

---

## 2. Strate-count — the sub-questions (Q1 … Q9)

The commissioning prompt bundles three orthogonal demands ("complete and
non-circular?", "do the tests have teeth?", "what is it quietly assuming?"). Each
splits further. **Strate-count: 9.**

| # | Strate | What a real answer looks like |
|---|--------|-------------------------------|
| **Q1** | **Completeness of the obligation tree.** Is `F ⟶ {OB-A…OB-F}` an exhaustive cover? Is there a proof or refutation route that lands in *no* node of the tree? | A named missing branch, or a proof that the cover is exhaustive by construction. |
| **Q2** | **Non-circularity.** Does any obligation's discharge depend, directly or through the L0/L1/L2 tier tags, on something the tree is meant to establish? Specifically: does the C1–C4 sandwich lean on §7's unclosed L2 debts? | A dependency edge that closes a cycle, with the two nodes named — or a clean topological order. |
| **Q3** | **Teeth of FT-1…FT-4 against the conjecture.** For each: would its failure *refute F*, or merely *embarrass the decomposition*? Are they falsifiers or confirmations dressed as falsifiers? | Per-test verdict: refutes-F / refutes-decomposition / refutes-nothing, with the reason. |
| **Q4** | **Teeth of FT-5…FT-7 against the decomposition.** FT-7 (the "bogus-proof smell test") is asserted as a *complete* structural filter. Is it? Could a correct proof of F exist that never establishes a polylog gap bound? | Either a counter-model for FT-7, or an argument that FT-7's completeness follows from C3's sharpness. |
| **Q5** | **The sharpness claim.** §0.2/§2.3-C3 says the criterion "$g_n$ vs $T_n$" is not an approximation but *the* criterion, sharp to ~16 digits, **unconditionally**. Sharpness is asserted at the frontier $p\sim4\times10^{18}$ — but F is a statement about **all $n\ge1$**, including $n=2$ where $p_n/n$ is $O(1)$ and $(\log p)^2/p$ is not small. Where exactly does the "sharp" reading break, and does anything downstream rely on it there? | The range of validity, stated as an inequality, and a list of which claims survive outside it. |
| **Q6** | **Quiet assumptions.** What is the decomposition assuming without tagging it? Candidates to probe, not to accept: (a) that the strategy ranking §3.6 is an inference from the tree rather than a smuggled prior; (b) that "the community expects F false" is evidential rather than sociological; (c) that the L0 tags are genuinely self-contained; (d) that a float-slack computation with a `SAFETY_MARGIN` is *evidence* rather than a *sanity check*. | Named assumptions with the section they hide in and what breaks if false. |
| **Q7** | **Directional integrity of the reduction.** §2.3-C4 asserts the (SUF) and (REF) criteria and claims Kourbatov's converse "agrees, which is exactly what C3 asserts". Do the two directions actually compose into an equivalence, or is there a gap band $T_n < g_n < T_n(1+g_n/p_n)$ where **neither** criterion fires? If that band is non-empty, is §0.2's "exactly equivalent" overstated, and does the refutation branch E1–E4 land inside it? | A verdict on whether the band is empty, negligible, or load-bearing — with the arithmetic. |
| **Q8** | **Formal-limits audit of the Lean branch (OB-F).** Is the F2 anchor a *faithful* formalization, or does the truncated-ℕ `n - 1` aliasing plus `noncomputable Nat.nth` create a statement that can compile while being the wrong theorem? Is OB-F8 correctly identified as the single high-risk item, or is the risk elsewhere (e.g. OB-F3's rpow bridge silently weakening `<` to `≤`)? | The specific failure mode that would let a *wrong* Lean statement pass `evidence-gate`. |
| **Q9** | **Actionable output: the weakest branches.** Which branches / tests must change *before* `source-ledger` and the rest of the DAG spend compute — ranked, with the concrete change. | An ordered list of (branch, defect, required change). Not a wish list. |

### 2.1 Anti-substitution constraints (per strate)

- **Q1** must NOT be answered as "the tree looks well-organized". Organization is
  not coverage. The answer must either name a route outside the tree or prove
  exhaustiveness.
- **Q2** must NOT be answered as "the L2 tags are honest". Honest labelling of a
  debt is not the absence of a cycle. The answer must trace edges.
- **Q3** must NOT be answered by restating each test's stated status. The status
  is in the document; the question is whether the test *could have failed*.
- **Q4** must NOT be answered by praising FT-7 as clever. Either exhibit a proof
  shape that evades it, or prove it cannot be evaded.
- **Q5** must NOT be answered with asymptotics. "As $p\to\infty$ the error
  vanishes" is the substitution; F quantifies over **all** $n\ge1$, and the
  document's own FT-4 says the tightest known cases are $n=2,4$.
- **Q6** must NOT produce a generic list of epistemic virtues. Each assumption
  must be tied to a section number and a consequence.
- **Q7** must NOT be answered by "the factor is $1+O((\log p)^2/p)$, so it's
  fine". That is the claim under audit, not the answer.
- **Q8** must NOT be a Lean style review. The question is *statement fidelity*,
  not tactic ergonomics.
- **Q9** must NOT be "everything is critical". A ranking that does not
  discriminate is not a ranking.

---

## 3. Panel selection

`--var panel=auto`. Selected from the worker's own available Claude Code
subagents, matched to the dispositions named in the commissioning prompt plus two
the artifact demands.

| Persona | Subagent | Disposition requested | Why this artifact needs it |
|---|---|---|---|
| **wheeler** | `wheeler` | question-framing (named in brief) | Q1, Q6 — the tree's *shape* and its vocabulary. Whether "obligation" is the right unit at all. |
| **feynman** | `feynman` | first-principles (named in brief) | Q5, Q7 — re-derive the sandwich and the threshold from scratch and see if the numbers survive contact. |
| **godel** | `godel` | formal-limits (named in brief) | Q2, Q8 — circularity, self-reference, and whether the Lean statement says what it claims. |
| **popper** | `popper` | falsifiability (demanded by Q3/Q4) | Q3, Q4 — the brief explicitly asks whether the tests would *refute*; that is Popper's exact instrument. |
| **knuth** | `knuth` | algorithmic/numerical rigor | Q5, Q6(d) — the `[COMPUTED]` evidence rests on a float slack with a hand-derived error bound and a `SAFETY_MARGIN`; someone must audit the arithmetic and the script. |

Five personas — the formula's ceiling. Every strate has at least two owners, so a
silence on any Qn is a genuine signal rather than a staffing gap.

**Coverage map** (used by step 3's table):

| | Q1 | Q2 | Q3 | Q4 | Q5 | Q6 | Q7 | Q8 | Q9 |
|---|---|---|---|---|---|---|---|---|---|
| wheeler | ● | ○ | | ○ | | ● | | | ● |
| feynman | ○ | | ○ | | ● | ● | ● | | ● |
| godel | ○ | ● | | ● | | ○ | ○ | ● | ● |
| popper | | ○ | ● | ● | | ● | | ○ | ● |
| knuth | | | ○ | | ● | ● | ● | ○ | ● |

● primary · ○ secondary

---

## 4. Per-persona substitution hypotheses

For each panelist: the **easier question they are gravitationally pulled toward**,
which step 3 will check against. If a response matches the substitution rather
than the strate, it is marked `Substituted (*)` in the coverage table — not
silently absorbed.

### wheeler — *substitution risk: HIGH*
**Hard question (Q1, Q6):** is the obligation tree an exhaustive cover, and what
does it quietly assume?
**Likely substitution:** *"is this the right question to be asking about the
primes?"* — a beautiful reframing of Firoozbakht as an information-theoretic
statement about prime density, which is interesting and answers nothing about
whether the tree covers its own target. Second-order risk: an audit of the
*vocabulary* (are "obligation", "barrier", "slack" the right names?) substituted
for an audit of the *structure*.
**Falsifier:** if the response contains no statement of the form "route X lands
in no node" or "the cover is exhaustive because Y", it substituted.

### feynman — *substitution risk: MEDIUM*
**Hard question (Q5, Q7):** where does the sharpness claim break, and is the
band $T_n < g_n < T_n(1+g_n/p_n)$ empty?
**Likely substitution:** *"is the explanation clear and free of cargo cult?"* —
a readability/simplicity audit that concludes the document is admirably honest
about its tiers. Second risk: recomputing the small-$n$ table (easy, already
done, and will confirm) instead of attacking the asymptotic-vs-all-$n$ seam.
**Falsifier:** if the response does not contain an explicit arithmetic evaluation
of the neither-criterion-fires band at at least one concrete $(n, p_n, g_n)$, it
substituted.

### godel — *substitution risk: MEDIUM-HIGH*
**Hard question (Q2, Q8):** is there a real circular dependency edge, and can the
Lean statement compile while being the wrong theorem?
**Likely substitution:** *"can any finite specification capture an infinite
statement?"* — a general incompleteness meditation (Firoozbakht may be
independent of PA/ZFC!) which is true, unfalsifiable here, and costs the polymer
nothing to know. Second risk: treating the `sorry` in §6.1 as the finding —
the document already says the main theorem will not close.
**Falsifier:** if the response's central claim is about undecidability-in-principle
rather than a named dependency edge or a named Lean fidelity defect, it substituted.

### popper — *substitution risk: LOW-MEDIUM*
**Hard question (Q3, Q4):** do FT-1…FT-7 have teeth; is FT-7 complete?
**Likely substitution:** *"is the document appropriately falsifiable in
spirit?"* — praising the L0/L1/L2/L3 tier system (which *is* good) as evidence
that the tests are sound. The tier discipline governs **citations**; the FT tests
govern **claims**. Conflating them is the substitution.
**Falsifier:** if the response does not deliver a per-test verdict for all seven
FTs, it substituted.

### knuth — *substitution risk: MEDIUM*
**Hard question (Q5, Q6d):** is the float-slack error analysis correct, and is
`SAFETY_MARGIN` doing the work claimed?
**Likely substitution:** *"is `verify_small_range.py` good code?"* — a review of
the sieve's memory behaviour, the `bytearray` slicing, and the loop's complexity.
All true, all irrelevant to whether the `[COMPUTED]` numbers are *evidence*.
Second risk: verifying the arithmetic of the small-$n$ table (which is exact and
will check out) instead of the 1-ulp error propagation claim for $n>200$.
**Falsifier:** if the response does not address the error bound
$|\Delta S_n| \lesssim 10^{-13}$ and the choice `EXACT_N = 200`, it substituted.

### Cross-panel substitution risk

All five share one gravitational pull: **agreeing with the document's own
verdict**. `decompose.md` is unusually self-critical (§5.3 "what this leg did not
establish", §7 debt table, §6.5 "LLM firewall"). A panel can mistake *displayed
humility* for *audited correctness* and return five variants of "commendably
rigorous, proceed". Every per-persona prompt therefore carries the instruction
below.

---

## 5. Shared prompt preamble (issued to all five)

> You are auditing `decompose.md`, the attack-surface decomposition for
> Firoozbakht's conjecture, produced by an upstream node of an automated
> math-attack pipeline. Seven downstream compute legs will be routed by its
> conclusions. Your review runs **before** that compute is spent.
>
> **The conjecture is OPEN. It is to be PROVEN or REFUTED, never assumed** —
> in either direction. A finding that the document assumes F false is as
> serious as one that assumes it true.
>
> **Warning — the document is self-critical by design.** It tags its own
> confidence tiers, lists its own debts, and states its own limits. Displayed
> humility is not audited correctness. Do **not** return a verdict whose
> substance is "commendably rigorous, proceed". If after genuine attack you
> find the branch sound, say so in one line and spend your budget on the
> branches that are not.
>
> Answer the questions assigned to you **as posed**. If you believe a question
> is malformed, say so explicitly and say why — do not silently answer an
> adjacent, easier one.
>
> Locators: pair any reference to the document with its section number
> (`§2.3-C3`, `FT-7`, `OB-F8`). Paths relative to the run dir. Return markdown.

---

## 6. Per-persona prompts

### 6.1 wheeler

Read `decompose/decompose.md` (§2 obligation tree, §3 strategies, §8 verdict).

**Q1 — Completeness.** The tree claims `Any complete proof *or* refutation must
pass through these nodes` (§2 preamble). Test that claim. Name a proof route or
a refutation route that lands in **no** node of OB-A…OB-F — or establish that
the cover is exhaustive and say by what argument. Candidate probes: a proof by
transfer from a different sequence; a probabilistic/measure-theoretic
non-constructive refutation; an independence result; a proof of F as a corollary
of a stronger conjecture (the §5.3 Nicholson/Farhadian chain is explicitly
excluded as [L3] — is that exclusion a gap?).

**Q6 — Quiet assumptions.** The §3.6 ranking places "Contradiction" at value
**None — do not spend** on the strength of §3.3's one-paragraph argument that the
contrapositive "adds no leverage". Is that an inference from the tree, or a
prior? Likewise: §8-6 states "the community's heuristic expectation is that F is
false" and immediately routes all budget accordingly. Distinguish which
allocations follow from the mathematics and which follow from the sociology.

**Q9.** Which branch would you change first, and to what.

**What your perspective should return:** a structural verdict on the *shape* of
the decomposition — whether the unit of decomposition ("obligation") is
load-bearing or decorative, and whether the tree's boundaries were drawn where
the mathematics puts them or where the document's narrative wanted them. Not a
vocabulary review.

---

### 6.2 feynman

Read `decompose/decompose.md` §1.2, §2.3 (C1–C4), §0.2, and §5.4.

**Q5 — Where does "sharp" break?** §2.3-C3 claims the criterion "$g_n$ vs $T_n$"
is *the* criterion, "sharp to sixteen decimal places at the frontier,
unconditionally". The relative error is $1+g_n/p_n$, controlled by
$O((\log p)^2/p)$ — which is tiny at $p\sim10^{18}$ and **not** tiny at $p=3$.
F quantifies over all $n\ge1$, and the document's own FT-4 finds the tightest
cases at $n=2,4$. Derive from scratch the range of $n$ where the sharpness
reading holds, state it as an inequality, and list which downstream claims
(§0.2's headline, FT-2, W4, the E3 collapse) survive outside it.

**Q7 — The dead band.** (SUF) fires when $g_n \le T_n$; (REF) fires when
$g_n \ge T_n(1+g_n/p_n)$. Between them lies a band where **neither** fires.
§2.3-C3 dismisses it by saying the two criteria only need to agree "near the
threshold". Evaluate the band's width at a concrete locus — use the document's
own record: $p = 1\,693\,182\,318\,746\,371$, $g = 1132$. Is the band empty,
negligible, or is it precisely where a refutation search (E1–E4) would land?
If a candidate counterexample fell in the band, what would the pipeline
conclude — and is that failure mode anywhere in the document?

**Q6 (share with knuth).** Is `[COMPUTED]` evidence or sanity check? §5.1 sieves
to $5\times10^7$ — eleven orders below the published frontier. State plainly
what that computation can and cannot support.

**What your perspective should return:** the re-derivation, done independently,
with the arithmetic shown. If your numbers match the document's, say so in a
line; spend the rest on where they diverge or where the document's framing
outruns its own algebra.

---

### 6.3 godel

Read `decompose/decompose.md` §0.1 (tier system), §2 (tree), §2.3-C4, §6 (Lean),
§7 (debts).

**Q2 — Circularity.** Build the dependency graph of the obligations and check it
is acyclic. Specific edges to test: §2.3-C4 derives the threshold using explicit
$p_n$ bounds tagged **[L2, Rosser–Schoenfeld / Dusart]** whose constants are
listed as *unclosed debt #4* — yet C2–C4 are used to justify §0.2's headline,
which is tagged **L0 (proved in this document)**. Can a claim be L0 if a step in
its derivation is L2? Second edge: §2.3-C4 calls the Kourbatov agreement "a
genuine independent confirmation of C2–C4, *not* a citation" — while Kourbatov is
itself unclosed debt #2. Is that independence real, or does confirmation flow
backwards through an unverified node?

**Q8 — Lean statement fidelity.** §6.1 defines
`nthPrime n := Nat.nth Nat.Prime (n - 1)` with truncated ℕ subtraction, and §6.3
lists OB-F8 (finite verification through a noncomputable `Nat.nth`) as the single
high-risk item. Audit that risk assignment. Is the higher risk actually
**OB-F3**, the F2→F1 `rpow` bridge — where a coercion or a `≤`/`<` slip yields a
theorem that compiles, is true, and is *not* Firoozbakht? Name the precise
failure mode that would pass `evidence-gate` while being the wrong theorem.
§6.5 tells `evidence-gate` to treat a `sorry`-free main theorem as a red flag —
is that instruction sufficient, or does it merely relocate the trust?

**Q9.** Rank the defects you found.

**What your perspective should return:** named dependency edges and named
fidelity defects. An observation that Firoozbakht might be independent of ZFC is
true, is not actionable for this polymer, and is not what is being asked — if you
raise it, mark it explicitly as out-of-scope and keep it to two lines.

---

### 6.4 popper

Read `decompose/decompose.md` §4 (all seven falsifiability tests), plus §0.1 and
§5.

**Q3 — Teeth against the conjecture (FT-1…FT-4).** For each of the four, deliver
a verdict from exactly one of:
`refutes-F` / `refutes-the-decomposition-only` / `refutes-nothing`,
with the reason. Probes: FT-2 is a "cheap proxy" that fires on published gap
tables — but by §2.3 the proxy and the exact predicate differ by $1+g/p$; does
FT-2 *refute*, or does it merely *nominate a candidate* that FT-1 must then
confirm? If the latter, the document calls its teeth "maximal" and that is
overstated. FT-3 requires proving $\limsup R_n > 1$, which §2.5-E5 says is
unreachable — is an unreachable test a falsifiability test at all, or is it an
unfalsifiable claim with a falsifiable costume? FT-4 checks small $n$ where the
conjecture is *known* to hold — what could it possibly refute?

**Q4 — FT-7's completeness.** FT-7 asserts: *any* correct proof of F must
somewhere establish a polylogarithmic prime-gap upper bound; therefore a candidate
proof that doesn't is wrong. This is used as the **first filter on every proof
attempt** (debt #11) — a false-negative here silently kills a correct proof.
Either exhibit a proof shape that evades FT-7 (e.g. one establishing the gap
bound non-explicitly, or as an unextractable consequence, or by contradiction
without ever exhibiting the bound), or show FT-7's completeness follows from the
C3 equivalence — and state which.

**Q6 — asymmetry.** §5.1's computation and §4's tests are all structured to
detect a counterexample. What test in this document could detect that F is
**true** — or, failing that, that the *decomposition's strategic conclusion*
(§8-6, proof categorically blocked) is wrong? If the answer is "none", the
document's falsifiability is one-sided; say so and say what it costs.

**What your perspective should return:** the seven verdicts as a table, then the
FT-7 ruling. Do not praise the L0/L1/L2/L3 tier system — it governs citations,
not claims, and is not under review here.

---

### 6.5 knuth

Read `decompose/verify_small_range.py` in full, plus `decompose.md` §5 (all of
it, especially §5.4) and §4 FT-4.

**Q5 — The float verdict.** §5.4 and the script's docstring argue that the
double-precision slack $S_n = \log p_n - n\log(1+g_n/p_n)$ is rigorous for
$n > 200$ because (a) `math.log` is ~1 ulp accurate, (b) neither term grows with
$n$, giving $|\Delta S_n| \lesssim 10^{-13}$, and (c) the observed minimum over
$n>200$ exceeds $3.5$. Audit each step. Note the script computes
`math.log(pn1 / pn)` — a division *then* a log, not `log(pn1) - log(pn)` and not
`log1p(g/pn)`. Does that change the error analysis? Is $n\cdot\log(p_{n+1}/p_n)$
genuinely non-growing in $n$, or does the multiplication by $n$ amplify a
relative error into an absolute one? Is `SAFETY_MARGIN = 1e-11` derived or
chosen?

**Q6(d) — evidence vs sanity check.** §5.1 calls the $5\times10^7$ sieve "a
sanity floor … deliberately independent of the literature". §7 debt #10 asks
`notebooks` to "measure growth of $\max R$ vs height … with error bars" and calls
it the real deliverable. Is a max-over-a-range statistic with no model of its
tail extrapolable *at all*? What would honest error bars on $\max_{p\le x} R$
even be? If the answer is "this deliverable is not well-posed", say so — it is
currently ranked **High** priority.

**Q7 (share with feynman).** The §5.2 shortfall column $T_n/g_n$ and §3.2's
"shortfall $1.055\times$" are load-bearing for the headline. Recompute the
$1.055$ from the quoted $(p,g) = (1\,693\,182\,318\,746\,371,\ 1132)$ and confirm
or correct it.

**Q8 (secondary).** §6.3-OB-F8 proposes a verified prime-list bridge for the Lean
finite verification. Is that the right algorithm, and what is the realistic
$N_0$? The document proposes $10^6$–$10^8$ (§3.5-W5) — is that achievable inside
a Lean kernel, and at what cost?

**What your perspective should return:** the corrected error analysis with the
bounds derived, not recalled; a verdict on whether `[COMPUTED]` numbers are
evidence; and the $1.055$ recomputation. Do not review the sieve's coding style.

---

## 7. What step 3 will check

1. Every Qn appears exactly once in the coverage table as
   Treated / Substituted / Declined-with-rationale / **Silent**.
2. Each response is checked against its persona's substitution hypothesis (§4)
   using the stated falsifier. A match marks `Substituted (*)` with the persona
   named.
3. The cross-panel risk (§4, "agreeing with the document's own verdict") is
   checked globally: if ≥4 of 5 responses conclude "sound, proceed", that
   convergence is reported as a **groupthink signal**, not as corroboration.

---

*Emitted by the `frame-deliberation` leg. Next: dispatch (step 2) → synthesis
(step 3) → recommendation to `source-ledger` and the downstream DAG (step 4).
This leg recommends; it does not nucleate.*
