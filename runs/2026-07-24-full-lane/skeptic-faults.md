# faults.md — adversarial audit of the `math-attack` corpus

**Leg**: `skeptic` (crew role: skeptic) of the `math-attack` polymer
(node)
**Attribution**: Noogram · **Date**: 2026-07-24
**Scope audited**: `proof-attempt__0/1/2`, `notebooks__0/1/2`, `source-ledger`,
`concept-cards`, `decompose`, `lean-skeleton`, `frame-deliberation`

> **Posture.** Firoozbakht's conjecture is open. Nothing in this document bears on its
> truth. This is an audit of *arguments and citations*, not of the conjecture.

---

## 0. Verdict

**The seal is BLOCKED.** Two BLOCKER findings, seven MAJOR, nine MINOR.

The corpus is unusually strong: I attacked every proof in it line by line, re-derived
every load-bearing number from scratch in independent code, re-ran all four verifier
scripts, and fetched four primary sources the corpus cites. **I found no mathematical
error in any theorem.** Every proof in `proof-attempt__0`, `proof-attempt__1` and
`proof-attempt__2` is sound as written, and every numeric claim I tested reproduced.

The BLOCKERs are not mathematics. They are (i) a **false statement about how much of the
load-bearing external data was independently verified**, sitting in the provenance record
of the corpus's single weakest premise, and (ii) an **invalid inference presented as the
one genuinely new quantitative result** of `notebooks__2`, already being used to
reallocate the polymer's budget. Both are exactly the class of fault that a seal exists
to stop, because both are *claims about the corpus's own epistemic standing* and both are
false.

The MAJORs are dominated by citation and cross-leg-consistency faults: two legs carry
contradictory verification tiers for the same source row, one leg attributes to a sibling
a claim the sibling had already retracted, and the corpus's single external analytic
premise is cited at a locator number that does not exist in the version-of-record it is
bibliographically attributed to.

---

## 1. What I actually ran

Findings below are anchored to work, not to reading. Everything here was executed by this
leg, independently of the corpus's own scripts unless stated.

| # | check | result |
|---|---|---|
| **S1** | Re-sieved to $2\times10^{7}$ in independent NumPy and recomputed `notebooks__0`'s F3/F4/F5 statistics from scratch | `min H_n = 1.196152422706632` at $n=2$; `max D_n = 0.5486599111467569` at $n=214$ ($p=1307$); `D_n ≥ 1` count $=0$ — **all exact matches** to `scan-1e9.json` |
| **S2** | Recomputed `proof-attempt__0` Thm 4 in 40-digit `mpmath` | $f_7-f_8 = 0.0281326864429$ — **matches** |
| **S3** | Recomputed $\mathcal H=\max_{m<K_0}f_m$ and $\min_{m<K_0}\sigma_m$ from my own sieve | $\mathcal H = 243.71408980828707$ at $m=688\,377$; $\min\sigma = 0.19615242270663$ at $m=2$ — **both match** PA-0 §6.2 |
| **S4** | Recomputed $\Lambda,\Upsilon,D^\*$ at all six heights PA-0 quotes | $0.13130,\,0.12996,\,0.11992,\,0.11436,\,0.11107,\,0.11062$ — **all match**; the record-locus bracket $1193.345367 \le 1193.417778 \le 1193.459723$ reproduces to every quoted digit |
| **S5** | Re-checked gap **G-PA0-1**/**G-N2** on a grid **~500× denser** than either leg used: $\underline B$ on 300 001 points over $[10,10^{22}]$; $\Upsilon,\Lambda$ on 200 001 points over $[K_0,10^{20}]$ | **0 non-monotone steps** in all three. The gap remains a gap (still unproved), but it is not hiding a counterexample at these resolutions |
| **S6** | Recomputed PA-2 §6.4's full ratio table in 50-digit `mpmath` from the OEIS b-files | Top-10 rows and $\min = 0.490437$ at $j=11$ — **exact match**, including $0.948545$ at $j=64$ |
| **S7** | Recomputed PA-0 Thm 7's 85-row lever | record #4 fails ($\underline B/G=0.823$), #5 onward passes, tightest $1.054246$ at #64 — **matches** |
| **S8** | Tested PA-2 **Lemma 1** empirically ($f_n > \psi(p_n)$ for $p_n\ge11$) over every index to $2\times10^{7}$ | **0 violations** |
| **S9** | Cross-checked `proof-attempt__2/maxgaps.json` (Wikipedia-sourced) against `notebooks__2/data/A002386,A005250,A005669` (OEIS-sourced) | **85/85 rows identical** in $G$, $P$, and $\pi(P)$ where both exist. *No leg performed this cross-check* |
| **S10** | Re-ran `verify_pa0.py`, `verify_rh.py`, `verify_ledger.py`, `verify_pa2.py`, `ffm.py --selftest`, `verify-findings.py`, `verify_cards.py` | **all exit 0**; the first four produce output **byte-identical** to the committed `.out` files (modulo wall-clock timings). Reproducibility claims hold |
| **S11** | Fetched `arxiv.org/pdf/1708.04122` (CMS 2019) and read Theorem 5, Corollary 4, and the surrounding text verbatim | **every CMS quotation in `proof-attempt__1` and `notebooks__1` is exact** — including the unnumbered display preceding Thm 5 and the "limit of this method … constant $\frac12$" remark (see §4, positive findings) |
| **S12** | Fetched `arxiv.org/pdf/2109.02249` (Johnston 2022) | Confirms `[Sch76, Corollary 1]` verbatim → **settles the Schoenfeld tier dispute** (finding 7) |
| **S13** | Fetched `arxiv.org/pdf/1409.1780v3` (Axler) and the **published Axler corrigendum** from `math.colgate.edu/~integers` | Confirms P1 at V0 — **and surfaces a locator fault** (finding 6) |
| **S14** | Fetched the Kourbatov corrigendum PDF | Matches the ledger verbatim, word for word |
| **S15** | Attempted a post-2019 arXiv sweep for improved RH-conditional gap constants | **blocked** (HTTP 429). Recorded as *not done*, which is the point of finding 8 |

---

## 2. BLOCKER findings

A non-empty BLOCKER set blocks the seal. These two must be fixed, not annotated.

---

### **BLOCKER-1 — `notebooks__2/data/PROVENANCE.md` overstates, by 47%, how much of the load-bearing table was independently recomputed.**

**Where.** `notebooks__2/data/PROVENANCE.md`, "Tier" section:

> "`notebook-2` §5 **independently recomputes rows 1–44** (every record with $P_i < 10^9$)
> from its own sieve and reports exact agreement. Rows 45–85 are inherited."

**The fact.** There are **30** records with $P_i < 10^9$, not 44. Counted directly from
the leg's own checksummed b-file:

```
records with P < 1e9 : 30
records with P < 3e9 : 33
```

The leg's own primary artifact says so: `findings-2.md` §3 — *"Rows 1–30 of A002386 /
A005250 / A005669 reproduced exactly"* — and `findings-2.md` §5 gap **G-N1** — *"Rows
1–30 are recomputed here; rows 31–85 are not."* `notebooks__0` §8 independently confirms
30 records below $10^{9}$. `proof-attempt__2` §7.2 C5 confirms 33 below $3\times10^{9}$.
**PROVENANCE.md is the only document in the corpus that says 44, and it is wrong.**

**Why this is a BLOCKER and not a typo.**

1. It is a false claim *about the corpus's own verification coverage*, in the file whose
   entire purpose is to record verification coverage.
2. It sits on **P2 / G-N1** — the premise every leg names as the single weakest link of
   the whole verified range ("the single load-bearing inherited input of the entire
   verified range" — `findings-2.md` G-N1; "**the weak link of Theorem A**" —
   `proof-attempt-2.md` F9/F2).
3. It is load-bearing *in the wrong direction*: rows 31–44 include record #34 onward, well
   above the sieve, and the false claim converts 14 unverified rows into verified ones.
4. `proof-attempt__0` §7 cites this exact file as its provenance authority: *"retrieved
   and checksummed by `notebooks__2` (`data/PROVENANCE.md`)"*. The overstatement is
   already one hop downstream.

**Fix.** Replace "rows 1–44 (every record with $P_i<10^9$)" with "rows 1–30 (every record
with $P_i<10^9$)"; re-state "Rows 31–85 are inherited". Then re-read every downstream
sentence that leans on PROVENANCE.md.

**Falsifiable by.** Producing a record $(P_i,G_i)$ with $31 \le i \le 44$ and
$P_i < 10^{9}$. There is none: $P_{30} = 436\,273\,009 < 10^{9} < P_{31} = 1\,294\,268\,491$.

---

### **BLOCKER-2 — `notebooks__2` §4's "the verified range itself excludes $C \ge 1.1736$" is an invalid inference, and it is the leg's advertised novelty.**

**Where.** `notebooks__2/findings-2.md` §4:

> "And the result that is genuinely new here: **the verified range itself excludes
> $C \ge 1.1736$** in this model — if $C$ were that large, the model's crossover would
> fall below $10^{20}$ and a counterexample should already have been seen. Granville's
> $2e^{-\gamma} = 1.1229$ sits **4.3% below that exclusion threshold**. The community's
> heuristic constant is not excluded by existing computation, but it is close to the
> edge — and one more decade of exhaustive gap analysis would put it in range."

**Three independent reasons the inference does not hold.**

1. **Category error: a $\limsup$ constant is not a finite-height running-max level.**
   Granville's $2e^{-\gamma}$ is the conjectured constant in
   $\limsup_x \max_{p_n\le x}(p_{n+1}-p_n)/\log^2 x$. A $\limsup$ asserts that the ratio
   comes close to $C$ *infinitely often*; it places **no constraint whatever** on the
   value of $\max_{p\le x} R$ at any particular finite $x$, including $x=10^{20}$. A
   sequence with $\limsup = 10$ can sit below $0.93$ for the first $10^{20}$ terms. So
   "no counterexample below $10^{20}$" excludes **nothing** about $C$. The leg's own
   **CC-23** states this distinction correctly (*"a $\limsup$ of 1 does not give
   $R_k < 1-1/\log p_k$ for every $k$"*) — §4 then uses the inverse of the same fallacy.
2. **A deterministic curve is fitted to a stochastic running maximum, and the residual is
   then treated as bounded.** The exclusion is the statement "the fitted $C$ plus its
   residual spread cannot reach 1.1736". `findings-2.md` G-N5 already concedes the spread
   *is not an inference*: *"§10's interval is dispersion, not inference. Running-max
   points are dependent; no valid CI is available."* An exclusion threshold read off an
   interval that is explicitly **not** a confidence interval is not an exclusion.
3. **It is not even robust inside its own model.** The leg reports the upper dispersion at
   $1.1586$. That is **1.3%** below the "exclusion" threshold $1.1736$ — narrower than
   the $4.3\%$ margin the same paragraph presents as the headline. A bound whose own
   stated dispersion reaches to within 1.3% of it is not a bound one publishes as a
   result.

**Why BLOCKER.** The sentence is flagged in the corpus as *"the result that is genuinely
new here"* and *"the actionable number for anyone budgeting a refutation search"*. It is
the one novel quantitative claim `notebooks__2` offers, it is stated in the register of a
theorem, and `write-paper` will carry it. The parenthetical "in this model" does not
survive the next two sentences, which speak about "existing computation" and about what
"one more decade of exhaustive gap analysis" would achieve — both statements about the
primes, not about the model.

**Fix (either).** (a) Delete the exclusion claim and keep only the descriptive fit
($\hat C = 1.0788$, dispersion $[0.999,1.159]$, crossover heights), or (b) restate it in a
form that survives: *"within the two-parameter family $\max_{p\le x}R \approx
C(1-2\log\log x/\log x)$, and treating the running maximum as if it followed that curve
deterministically, $C \ge 1.1736$ would place the crossover below $10^{20}$. This is a
property of the fit, not a constraint on the primes, and in particular it does not bear
on Granville's $\limsup$ constant."*

**Falsifiable by.** Exhibiting a gap sequence with $\limsup R_n \ge 1.1736$ whose running
maximum stays below the Firoozbakht threshold to $10^{20}$. Such sequences are trivially
constructible, which is the whole objection.

---

## 3. MAJOR findings

---

### **MAJOR-3 — `proof-attempt__0` §0 sells a vacuous conclusion as one of five headline verdicts, and its own Prop. 2 says it is vacuous.**

**Where.** `proof-attempt-0.md` §0, verdict table row 3:

> "Is FFM **proved** on a range? **Yes**, on $[89,H)$ for every height $H$ to which the
> maximal-gap table is complete — up to $10^{20}$ today (Thm. 7). Tier
> **L1 + L2-completeness**."

**The fault.** Theorem 7's premises are exactly Axler Cor. 3.5 + completeness of the
85-row table. From those same two premises, `proof-attempt__2` Theorem A proves **F
itself** on the same range. And PA-0's own Prop. 2, consequence 3, states:

> "FFM is **empirically unfalsifiable below the verification frontier**: on any range
> where F is verified, FFM is vacuously true there and no computation can say more."

So on $[89,10^{20})$, "FFM holds" is a **logical consequence of premises the same document
already grants**, carrying zero information. PA-0 §8 proves this itself — *"No least
failure, no maximality, no FFM — the same ~80 inequalities certify F directly on the whole
range."* The verdict table nevertheless presents the range result as a deliverable, and
`synthesize`/`write-paper` will carry it as one.

**What is actually non-vacuous.** Theorem 7 proves **(C-head)** — $G_{n-1} < f_n$ — at
every such $n$, which is strictly stronger than FFM-at-$n$ and is *not* implied by F's
verification. That is a real result. Its correct headline is "the certificate (C-head) is
established on $[89,H)$", not "FFM is proved on $[89,H)$".

**Fix.** Rewrite the verdict row to name the certificate, and add the one sentence PA-0
already knows: that the FFM conclusion on that range is vacuous relative to Theorem A.

---

### **MAJOR-4 — `proof-attempt__0` §8 attributes to `notebooks__0` a premise that `notebooks__0` had already retracted, and presents the retraction as its own finding.**

**Where.** `proof-attempt-0.md` §8, "The premise correction":

> "`notebooks__0`'s header states: *'(FFM) is the statement that licenses the entire
> verification literature. […] (FFM) is therefore load-bearing.'*
> **That is false as stated** … This is a cosmon-ward-style correction *within* the
> polymer: a downstream leg's stated motivation for target #0 does not survive contact
> with the proof. **Recorded here rather than silently patched.**"

**The fact.** `notebooks__0/findings-0.md` — the leg's primary artifact — opens (§0) with
precisely the correction PA-0 claims to be making, laying out the two-route distinction
and stating in bold that *"the obvious framing ('the verification literature rests on
(FFM)') is **wrong**"*. Its §11 verification record, defect #2, is explicit:

> "**§0 overclaimed the target's role.** The first draft asserted that the published
> maximal-gap verifications are 'licensed by (FFM) and by nothing else'. That is
> **false** … §0 was rewritten around the two-route distinction."

PA-0 read the quoted sentence from `notebooks__0/ffm.py` (see MAJOR-5), not from
`findings-0.md`, and did not check whether the leg's primary artifact still held the
position. The result is a §8 that mis-states a sibling leg's position and claims
originality for a published correction.

**Consequence.** §8 is written to be carried downstream (*"`synthesize` and `write-paper`
should carry it"*). As written it would put into the paper a false statement about what
this project's own computational leg concluded.

**Fix.** Rewrite §8 to credit `notebooks__0` §0 and §11-2 with the distinction, and
restate PA-0's actual (real) contribution: the *proof* that Theorem 7's argument runs
without FFM, i.e. that FFM is sufficient but not necessary.

---

### **MAJOR-5 — `notebooks__0/ffm.py` still carries, in its module docstring, a claim the same leg formally retracted.**

**Where.** `notebooks__0/ffm.py`, lines 24–29:

> "(FFM) is the statement that licenses the entire verification literature. Kourbatov
> (2015) and Visser (2019) verify F to 4e18 and 2^64 respectively by walking a
> first-occurrence / maximal gap table … **Without (FFM) those verifications do not cover
> what they claim.** (FFM) is therefore load-bearing…"

`findings-0.md` §11 defect #2 declares this **false** and records that it was corrected —
but the correction was applied only to `findings-0.md` §0. The engine's docstring, which
is the front matter of the leg's central artifact and the text a reader opening the code
sees first, still asserts it. It is also the text `proof-attempt__0` picked up (MAJOR-4).

This is the *exact* pathology `findings-0.md` §9 item 5 warns about, applied to the leg's
own prose: a correction reasoned about in one file and never propagated to the artifact
that carries it.

**Fix.** Apply the §11-2 correction to `ffm.py`'s docstring. Then check whether any other
§11 correction was likewise applied to only one of the leg's five artifacts.

---

### **MAJOR-6 — the corpus's single external analytic premise is cited at a locator that does not exist in the version-of-record it is bibliographically attributed to.**

**Evidence obtained by this leg (V0, both).** I retrieved:

* `arxiv.org/pdf/1409.1780v3` — Axler, *New bounds for the prime counting function*.
  **Corollary 3.5** there does state, verbatim, the four-member family including
  *"for every $x \ge 9.25$, we have $\pi(x) < x/(\log x - 1 - 1/\log x - 3.83/\log^2 x)$"*
  and *"If $x\ge5.43$, then $\pi(x) < x/(\log x - 1 - 1.17/\log x)$"*.
  **PA-2's premise P1 is confirmed at V0.** ✔
* The **published corrigendum** (`math.colgate.edu/~integers`, `q22=errata`), in full:

  > "**In Corollary 3.4 on page 8**, replace 'If $x \ge 5.43$' by 'If $x \ge 2\,634\,800\,823$'."

**The fault.** The corrigendum — the authoritative statement about the *published* paper
— numbers the affected result **Corollary 3.4, page 8**. `3.5` is the **arXiv v3**
numbering. Yet the corpus cites the result as "Corollary 3.5" against a bibliographic
entry whose journal-of-record fields are `Integers 16 (2016), A22`:

| artifact | citation |
|---|---|
| `source-ledger.md` §3.5 | Locator column: "**Corollary 3.5** ¶" |
| `concept-cards/CC-19` | Axler Cor. 3.5 |
| `proof-attempt-2.md` P1, §3, F1 | "Axler, *New bounds for the prime counting function*, **Cor. 3.5**" |
| `proof-attempt-0.md` §7, G-PA0-3 | "`axler2014newbounds` Cor. 3.5" |
| `notebooks__2` §1.1, G-N3 | "`axler2014newbounds` Cor. 3.5" |

The ledger's BibTeX note (*"Corollaries 3.5, 3.6 read from arXiv v3"*) partially discloses
this, and the ledger's Source column does name v3 — but no downstream leg carries the
qualifier, and `proof-attempt-2.md`'s P1 row (the premise statement of the corpus's only
theorem) carries none. A referee checking `Integers 16 (2016), A22` will not find the
$3.83$ bound at "Corollary 3.5".

Worse, the corpus never establishes **where the $3.83$ member sits in the published
numbering**. `proof-attempt-2.md` F1 asserts the $9.25$ threshold is "unaffected by
`axler2016corrigendum` on the authority of **CC-19** + the ledger" — and both of those
rest on the *Kourbatov* corrigendum, which mentions only the $5.43$ item. **I can now
confirm from the Axler corrigendum itself that it changes exactly one threshold and does
not touch the $9.25$ member** — so the *substance* of F1 is correct and F1 can be
discharged. But the *locator* remains version-ambiguous.

**Fix.** Two edits. (i) Every citation of the $3.83$ bound must name the version:
"Axler, arXiv:1409.1780v3, Cor. 3.5" — or obtain the published PDF and give its number.
(ii) Add the Axler corrigendum as a **V0** row with its verbatim one-sentence content
(the corpus currently carries it at V2, second-hand through Kourbatov); this converts
`proof-attempt-2.md` F1 from "not fatal under any outcome" to "discharged".

---

### **MAJOR-7 — the corpus carries two mutually incompatible verification tiers for `schoenfeld1976sharper`, and the more pessimistic one is wrong.**

**The conflict.**

| artifact | tier | claim |
|---|---|---|
| `notebooks__1/findings.md` §6 | **V2**, locator **Corollary 1** | *"quoted verbatim by `johnston2022improving` eq. (1.1)"*; adds a locator correction warning that "Theorem 10" is the common mis-citation |
| `proof-attempt-1.md` §8.1 + gap **G8** | **V3**, "**locator not verified**" | *"no secondary was read verbatim by this leg at that locator — only a search summary asserted it. **V3, not V2**"* |

**Resolution (this leg, V0).** I fetched `arxiv.org/pdf/2109.02249` (Johnston 2022). §1,
first sentence:

> "In 1976, Schoenfeld [Sch76, **Corollary 1**] proved that under assumption of the
> Riemann hypothesis, $|\pi(x)-\mathrm{li}(x)| < \frac{\sqrt x}{8\pi}\log x$, for
> $x \ge 2657$."

`notebooks__1` is **correct**: a reliable secondary quotes the primary verbatim at the
named locator, which is exactly the ledger's definition of **V2**. `proof-attempt__1`'s
V3 rating is stale — its own §11 footnote records that `notebooks__1` had produced no
artifact when PA-1 was written.

**Why MAJOR.** The corpus as it stands hands `citation-gate` a row with two tiers and two
gap statuses (`G8` open vs. resolved), and the ledger's own discipline
(*"`citation-gate` should fail any paper claim resting on a bare V3 locator"*) makes the
tier operative, not cosmetic. Unresolved, it either kills a correct citation or passes an
unverified one, depending on which leg the gate reads first.

**Fix.** Promote `schoenfeld1976sharper` to **V2, Corollary 1**, on the Johnston 2022
secondary; close **G8**; delete PA-1's §8.1 V3 row and its "locator not verified" note.
PA-1's §6 conclusion is unchanged either way (it says so itself).

---

### **MAJOR-8 — "the strongest published explicit RH-conditional gap bound" is a superlative survey claim carried inside a V0 row, and no leg documents a literature sweep after 2019.**

**Where.** `proof-attempt-1.md` §2, in the *statement* cell of a row tagged **V0**:

> "`cms2019fourier` **Thm 5** … **This is the strongest published explicit RH-conditional
> gap bound.**"

and `notebooks__1/findings.md` §1: *"The strongest explicit RH-conditional prime-gap bound
**in the literature**…"*

**The fault.** I verified (S11) that every *quotation* from CMS 2019 is exact. But CMS do
**not** claim to be the strongest published bound in general — the paper says only that
Theorem 5 *"improves a result of Dudek, Grenié, and Molteni [20, Theorem 1.1]"*. The
superlative is the corpus's own survey judgement, and it is sitting in a cell whose **V0**
tag means "read verbatim at the named locator". Under the ledger's own protocol
(*"A row is … only [an endorsement] of the fact that the named source, at the named
locator, says that thing"*), this is a mis-tagged row.

Compounding it: CMS is a **2019** paper and the corpus's own dates are **2026**. Neither
`proof-attempt__1` nor `notebooks__1` records a search for post-2019 improvements; both
simply assert the superlative. My own attempt to sweep arXiv for later results was
rate-limited (S15) — so **as of this audit the superlative is unverified by anyone**.

**Mitigating, and worth stating plainly:** the claim is *decorative*, not load-bearing.
PA-1's Theorem 2 covers **every** $c>0$, and Theorem 3 covers every $c \ge 10^{-7}$ — so a
newer, better constant changes nothing in either document's conclusions. This is why the
finding is MAJOR and not BLOCKER.

**Fix.** Restate as *"the strongest explicit RH-conditional gap constant found by this
corpus's search as of 2026-07-24; the corpus did not sweep the post-2019 literature"*, and
move it out of the V0 statement cell into the surrounding prose. Add a note that Theorems
2 and 3 are insensitive to it.

---

### **MAJOR-9 — the model-fit crossover heights are used as an input to a budget decision without the model tag surviving.**

**Where.** `notebooks__2/findings-2.md` §8, handoff item 5:

> "the refutation branch … loses its quantitative one (the model that produced '5.5% away'
> over-predicts observed counterexamples by 6.7× at the frontier, and the fitted crossover
> is $10^{30}$–$10^{51}$, not $10^{21}$). **Search is not affordable at any of those
> heights.**"

**The fault.** The interval $10^{30}$–$10^{51}$ comes from the same §10 fit as BLOCKER-2:
a two-parameter deterministic curve fitted to a running maximum, with an interval its own
author declares is *not* an inference (**G-N5**), extrapolated **11 to 31 orders of
magnitude** beyond the last data point. It is then used, unqualified, as the arithmetic
ground for reallocating the polymer's budget away from the refutation branch — replacing,
in the leg's own words, "sociological grounds" with "arithmetic grounds". The arithmetic
is a 55-point extrapolation of an ad-hoc functional form across 30 decades.

This is a distinct fault from BLOCKER-2 because the consequence is different: BLOCKER-2 is
a false claim that would enter the paper; MAJOR-9 is a false claim that has already
entered an *allocation decision*, where it will be invisible.

**Fix.** State the crossover heights with their model tag every time they appear, and
restate the budget recommendation as resting on D1 (which is a real, pre-registered,
observed 6.72-vs-0 discrepancy) rather than on §10's extrapolation.

---

## 4. MINOR findings

**MINOR-10 — `proof-attempt-2.md` miscounts its own inequalities, in five places.**
Theorem A applies Lemma 3 at $J = 84$, consuming $j = 5..84$ = **80** checks. But the
document says **81** at §5 ("collapses to **81** numeric inequalities"), §7.4 ("at exactly
one row out of 81"), §10 ("81 checkable inequalities"), and twice in the Lean handoff
("81 `norm_num` inequalities"; "a complete record list with these 81 inequalities"). Only
§6.4 has it right (*"Theorem A consumes only $j \le 84$"*), and `notebooks__2` says 80
throughout. The count that would go into the Lean statement is the wrong one.

**MINOR-11 — off-by-one in a verifier's own label.** `verify_pa2.py` CHECK C3 prints
*"144,449,537 indices"*; there are 144 449 537 primes and therefore 144 449 536 gaps.
PA-2 §7.1's prose has it right. Cosmetic, but it is the label a reader trusts.

**MINOR-12 — `notebooks__0` §10.1 states a stronger hypothesis than its proof uses.** The
claim is *"(D) together with F implies (FFM)"*, but the displayed proof
($f_n > f_m - (f_m - G_m) = G_m \ge g_m$, hence $G_{n-1} < f_n$) never invokes F. (D)
alone implies (C-head) at every index, hence FFM. The remark two paragraphs later — that
(D) *cannot be proved* without F, because its RHS is the Firoozbakht margin — is correct
and is a different statement. Restate the implication with the hypothesis it actually
needs.

**MINOR-13 — the "Firoozbakht threshold" $R_n < 1 - 1/\log p_n$ is the proxy barrier,
used in two places as if exact.** From $f_n = L^2-L-1+o(1)$ the threshold is
$R_n < 1 - 1/L - 1/L^2$. `notebooks__1` §5.1 writes *"Firoozbakht needs
$R_n < 1-1/\log p_n$"* flatly, and `notebooks__2` §10 computes its crossover heights
against $1-1/\log x$. **CC-05** is careful (it writes "$\lesssim$"); the two notebooks drop
the hedge. The effect on the crossover heights is small and in the conservative direction,
but it is an approximation presented as the statement.

**MINOR-14 — index-$10^{20}$ and height-$10^{20}$ are used in adjacent sentences.**
`proof-attempt-0.md` Thm. 5 is stated for indices $K_0 < n \le 10^{20}$, while §0 and §7
use $10^{20}$ as a *height* $H$ (which corresponds to index $\approx 2.2\times10^{18}$).
Both are internally correct; a reader will conflate them, and **CC-25**'s index/height
discipline exists precisely to stop this.

**MINOR-15 — `maxgaps.json` rows 83–85 carry an index column with no second source.**
`A005669` has 82 rows; `proof-attempt__2/maxgaps.json` supplies $\pi(P_j)$ for $j=83,84,85$
from Wikipedia alone. Harmless here — PA-2 §5 correctly notes the argument *"needs no
index data at all"* — but the values are live in a committed artifact and will be reused.
Mark them single-sourced.

**MINOR-16 — two provenance chains for the same 85 rows, cross-checked by nobody.**
`notebooks__2` sources the table from OEIS b-files with SHA-256 sums; `proof-attempt__2`
independently sources it from Wikipedia's *Prime gap* article into `maxgaps.json`; and
`proof-attempt__0` §7 then cites *`notebooks__2`'s* checksums as provenance for a table
`proof-attempt__2` obtained elsewhere. No leg compared the two. **I did (S9): all 85 rows
agree exactly in $G$, $P$, and $\pi(P)$.** This is a genuine, cheap, two-source
corroboration of the corpus's weakest premise that is currently going unclaimed —
record it, and correct PA-0 §7's provenance attribution.

**MINOR-17 — `proof-attempt-2.md` §7.2 (C6e) overstates what its check establishes.** It
says Visser's "below the location of the 81st maximal prime gap, certainly for all
$p<2^{64}$" is *"only correct if $P_{80} < 2^{64} < P_{81}$"*. Only $P_{81} > 2^{64}$ is
needed for correctness; $P_{80} < 2^{64}$ makes the claim *tight*, not *true*. The check
itself is sound and the conclusion is unchanged.

**MINOR-18 — Lemma 1's own validity margin is never reported, and §7.4's "9 ppm" reads as
uniform.** PA-2 §7.4 gives $(f-\psi)/f = 9.285\times10^{-6}$ at the record locus, and §6.4
says the relaxation "costs real margin only where $p$ is small". Measured (S8), the
absolute margin $f_n - \psi(p_n)$ dips to $\approx 0.0147$ near $p \approx 1.29\times10^{7}$
(relative $\approx 6\times10^{-5}$, six times the record-locus figure) before decaying like
$\approx 0.83/\log p$. Nothing is wrong — Axler's theorem guarantees positivity everywhere
above $9.25$, and no record sits in that region — but a reader taking "9 ppm" as the
relaxation's uniform cost is taking a locus-specific number as a global one.

---

## 5. What I attacked and could not break

Stated because a red-team report that only lists faults has not finished its job, and
because the seal needs to know which parts survived a real attempt.

1. **Every theorem in the corpus is sound.** I checked each proof step by step:
   * **PA-0**: Prop. 1 (all steps are equivalences ✔), Prop. 2 (✔), Thm. 3 and its
     near-necessity claim (✔), Thm. 4 (integer certificate, ✔ S2), Thm. 5 (the
     $\Lambda \le f \le \Upsilon$ sandwich derivation is correct, including the
     $\varphi \ge 1$ / $\varphi$-increasing directions, ✔ S4), Thm. 6 (the chain
     $\sigma_m \le f_m - f_{n^\*} \le \max_{m<n^\*}f_m - f_{n^\*} \le D^\*$ ✔, and the
     head case is genuinely closed by $0.196152 > 0.1314$ ✔ S3), Thm. 7 (the
     "unrecorded record" contradiction is correctly argued and correctly gated on
     completeness ✔).
   * **PA-1**: Thm. 1 parts 1 and 2 — I re-derived every step of §3.2 (b)–(g) by hand;
     $D(n)\le2\log n$, $L \le 3\log n$, $e^x-1-x \le (e-2)x^2$ on $(0,1]$, the reduction
     to $H(n)<c$, and the envelope $\widehat H$ are all correct, and
     $\widehat H(N_0)=0.00625$ reproduces. Thm. 2 and Thm. 3 follow, including the
     $n_c \le 7.31\times10^{15} \Rightarrow p \le 5.34\times10^{17} < 2^{64}$ arithmetic.
     Prop. 4's $\Omega$-barrier argument is valid as stated and correctly scoped.
   * **PA-2**: Lemma 0 ✔, Lemma 1 (all four steps; the direction of the $\pi$ bound is
     right, and step 2's cubic argument is correct) ✔ S8, Lemma 2 ✔, Observation R and
     Lemma 3 (the `min(H, P_{j+1})` guard is what makes the completeness premise legitimate
     at $j=J$ — this is the subtlest step in the corpus and it is correct) ✔, Theorem A ✔,
     Theorem B ✔.
2. **The one derivation I expected to break, didn't.** CMS Theorem 5 gives a prime in the
   **closed** interval $[x, x+\frac{22}{25}\sqrt x\log x]$; applied at $x=p_n$ the prime
   found may be $p_n$ itself, making the gap bound vacuous. The correct route is $x =
   p_n+\delta$, $\delta\to0^+$, which recovers $g_n \le \frac{22}{25}\sqrt{p_n}\log p_n$ by
   continuity and requires $p_n \ge 4$, i.e. $p_n \ge 5$ — exactly the $p_n>3$ range CMS
   state. **And the authors state the gap form themselves**: I read, verbatim, the
   unnumbered display immediately preceding Theorem 5 (S11) —
   *"$p_{n+1} - p_n \le \frac{22}{25}\sqrt{p_n}\log p_n$ for all primes $p_n > 3$"* —
   which is precisely what `proof-attempt-1.md` §2 claims is there. The citation is exact.
3. **All four CMS quotations verify verbatim** (S11): Theorem 5; Corollary 4 eq. (1.14)
   ($\limsup \le 1/C^+(B) < 21/25$); *"the limit of this method would yield a constant
   $\frac12$"*; and the RH + pair-correlation $\limsup = 0$ remark. `proof-attempt__1` and
   `notebooks__1` cite this paper correctly in every instance I could check.
4. **The Kourbatov corrigendum verifies word for word** against `source-ledger.md` §3.4
   (S14), including the $[29, 2\,634\,800\,823]$ range split.
5. **The grid-checked monotonicity gaps (G-PA0-1, G-N2) survive a ~500× denser grid**
   (S5): 0 violations in 500 000 additional points. They remain unproved — the legs are
   right to flag them — but there is no counterexample lurking at grid resolution.
6. **Reproducibility is real** (S10). Four verifier scripts produce byte-identical output
   to their committed `.out` files. `ffm.py --selftest`, `verify-findings.py` and
   `verify_cards.py` all pass. `lean-skeleton` builds (`lake build` exit 0) with a clean
   axiom audit: exactly seven declarations depend on `sorryAx` — `firoozbakht` itself plus
   the six reformulations derived from it — and every bridge lemma
   (`rpow_inv_lt_rpow_inv_iff_pow_lt_pow`, `firoozbakht_iff_int`, `pow_lt_pow_iff_log`,
   `slack_eq`, `firoozbakht_iff_gap`) is `sorry`-free. The Lean
   index convention is pinned by evaluation, not assertion, and the "load-bearing exponent"
   example correctly distinguishes the shifted statement.
7. **No leg anywhere claims the conjecture is proved, refuted, or likely.** Every artifact
   opens with the open-status posture and closes by restating it. The one place where
   verification threatened to leak into evidence — `proof-attempt-2.md` §10 — pre-empts it
   explicitly (*"A reader who takes $H=10^{20}$ as encouragement has misread the
   document"*). This discipline held across all nine artifacts I audited, with the single
   exception recorded as BLOCKER-2.
8. **All internal cross-references resolve.** `outcomes.md#A1`–`#A11` are referenced 11
   distinct ways across the corpus and every one exists. The concept-card deck's
   dependency graph is consistent, and **CC-05**'s and **CC-23**'s own hedges are correct
   where the notebooks' are not (MINOR-13, BLOCKER-2).

---

## 6. Disposition

| id | severity | artifact | one-line fix |
|---|---|---|---|
| **1** | **BLOCKER** | `notebooks__2/data/PROVENANCE.md` | "rows 1–44" → "rows 1–30"; re-read every downstream sentence leaning on it |
| **2** | **BLOCKER** | `notebooks__2/findings-2.md` §4 | delete the $C \ge 1.1736$ exclusion, or restate it as a property of the fit that says nothing about a $\limsup$ |
| **3** | MAJOR | `proof-attempt-0.md` §0 | verdict row 3 must name **(C-head)**, not FFM, and state the vacuity |
| **4** | MAJOR | `proof-attempt-0.md` §8 | credit `notebooks__0` §0/§11-2; restate PA-0's real contribution |
| **5** | MAJOR | `notebooks__0/ffm.py` | apply the §11-2 correction to the docstring |
| **6** | MAJOR | ledger §3.5, CC-19, PA-0, PA-2, `notebooks__2` | version-qualify "Cor. 3.5"; add the Axler corrigendum as a V0 row (text supplied above) |
| **7** | MAJOR | `proof-attempt-1.md` §8.1, G8 | promote `schoenfeld1976sharper` to V2/Corollary 1; close G8 |
| **8** | MAJOR | `proof-attempt-1.md` §2, `notebooks__1` §1 | de-tag the superlative from V0; date-bound it; note Thms. 2–3 are insensitive |
| **9** | MAJOR | `notebooks__2/findings-2.md` §8-5 | rest the budget call on D1, not on §10's 30-decade extrapolation |
| **10–18** | MINOR | as listed | see §4 |

**Gate for `synthesize`, `evidence-gate`, `editorial-verdict` and `write-paper`:**
findings **1** and **2** must be closed before the seal. Findings **3–9** must be closed
before anything reaches the paper. Findings **10–18** are annotations.

---

## 7. Reproduction

```
cd <run>/skeptic
python3 verify_faults.py            # 46 named checks, exit 0, ~90 s
python3 verify_faults.py --quick    # coarser grids, ~10 s
```

`verify_faults.py` re-derives every `[S]` number in §1 independently of the corpus's own
scripts (its own NumPy sieve, its own `mpmath` barrier code), re-checks the two
monotonicity gaps on the denser grids of S5, and ends with **prose gates** that assert
each live fault is still present in the audited artifact — so a later reader can tell,
in one run, whether the findings have been fixed or merely acknowledged. Output of record:
`verify_faults.out`. **46/46 pass, exit 0.** Deterministic; no RNG.

The gates in §5 that are *not* re-run here because they are external retrievals — S11
(CMS 2019), S12 (Johnston 2022), S13 (Axler v3 + the published corrigendum), S14
(Kourbatov corrigendum) — are quoted verbatim above at the point of use, with their
retrieval URLs, so `citation-gate` can re-fetch and compare rather than take this leg's
word.

Gate status for this leg's own artifacts: `ruff check verify_faults.py` — clean;
`python3 -m py_compile` — OK. `.cosmon/config.toml` defines no `[gates]` for this
project, so these are the gates this leg imposed on itself. No check was skipped, and
none is reported as passing that was not run — including MINOR-10's prose gate, which
**failed on first execution** (my substring did not match PA-2's line-wrapped text), was
investigated rather than loosened, and turned out to understate the fault: PA-2 says "81"
in **five** places, not two. Finding 10 was rewritten to the true count.

---

*Emitted by the `skeptic` leg (node) of the `math-attack` polymer.
Firoozbakht's conjecture is **open**. This leg produced no evidence about its truth in
either direction, and found none in the corpus it audited.*
