# write-paper — authoring log

**Artifacts emitted**: `paper.md`, `references.bib`, `log.md` (this file)

---

## 1. Brief points — disposition

Every point the brief enumerates, and where it lands. Nothing dropped silently.

| brief point | disposition |
|---|---|
| Write the paper from `synthesis.md` | **DONE** — `paper.md`. Synthesis read in full, plus the twelve upstream artifacts it folds (see §3). |
| LaTeX **or** markdown | **markdown + LaTeX math** chosen. Matches every other artifact in the run; renders in Obsidian; no build step between the author and the reader. |
| **Statement** | §1.1 (F1 form + strictly-decreasing form), §1.2 (five equivalent forms with proof). |
| **Method** | §2 — the eleven-leg pipeline, the four method commitments, the formal environment, and the RT-12 self-check requirement. |
| **Proof / refutation** | §4 (kernel-proved), §5 (established on paper), §6 (three routes refuted). §0.1 fixes the word "proved" to the kernel. |
| **Computational evidence** | §7 — tiered range table, independent reproduction, one open computational obligation, and the demotion of the run's own opening framing. |
| **Honest limitations** | §9 — ten numbered limitations, plus §9.1 (what must close before release) and §9.2 (the honest forecast). §10 is the separate "does not claim" list. |
| Every citation traces to a source-ledger row | **DONE with one explicit exception, flagged not hidden** — §11.1 traces 15 merged rows; §11.2 lists **six rows that were proposed by `proof-attempt__1` §8.1 and never merged into the ledger**, states which paper claims rest on them, and makes merging them a release blocker (§9.1 item 3). See §2 below. |
| Delivery posture: **staged** | **DONE** — §0.2, stated on page one with the three unpassed gates, and repeated in the colophon. |
| External attribution: **Noogram** | **DONE** — title block, §0.3, colophon. No fund affiliation appears anywhere. |
| Claim 'proved' ONLY for kernel-established targets | **DONE** — §0.1 states the rule; §4 is the only section using the word for results; §5/§6 use "established"/"argued" and carry tiers. §4.4 discharges RT-12 for all 14 named declarations. |

---

## 2. The one gap in citation tracing — stated, not papered over

The brief requires every citation to trace to a source-ledger row. **Six do not**, and
this is a real finding rather than an authoring shortcut:

- `source-ledger.md` was produced at node 3 of the polymer, before the RH-route leg ran.
- `proof-attempt__1` §8.1 **proposed** six new rows (`cms2019fourier`,
  `dudek2015riemann`, `leenosal2025sharper`, `schoenfeld1976sharper`, `littlewood1914`,
  `montgomery1973pair`) and asked for them to be merged.
- **No leg merged them.** `source-ledger.md` still carries 31 citekeys and none of these.

Options considered:

1. Drop §6.1 (the RH-route refutation) from the paper. **Rejected** — it is one of the
   run's three substantive route-refutations; dropping it would silently narrow the
   deliverable.
2. Cite the six as if ledger-backed. **Rejected** — that is precisely the fault class
   this polymer's gates exist to catch.
3. Cite them in a **separate, clearly labelled block**, state the tier the proposing leg
   claimed, name which paper claims depend on them, and make merging a release blocker.
   **Chosen.**

Implementation: `paper.md` §11.2 (table + consequence paragraph), §9 limitation 5,
§9.1 item 3, and `references.bib` BLOCK B (physically separated, every entry carrying
`note = {PROPOSED ... NOT MERGED}`). Two of the six (`schoenfeld1976sharper`,
`montgomery1973pair`) are **not used at all** in the paper and are listed only so the
citation gate sees the whole proposed set.

**Handoff to `citation-gate`**: merge `cms2019fourier` at V0 (its quotations were
independently re-fetched and confirmed exact by the skeptic leg, check S11), promote or
demote `littlewood1914`, and resolve the `schoenfeld1976sharper` tier dispute (MAJOR-7,
which the skeptic's check S12 appears to settle in favour of V2).

---

## 3. Sources read to author this paper

`synthesize/synthesis.md`; `source-ledger/source-ledger.md` (full);
`lean-probe/lean-probe-report.md` (full) and the four Lean library sources verbatim
(`Statement.lean`, `Criteria.lean`, `FiniteRange.lean`, plus `probe-attempts/Verify.lean`
via the report); `skeptic/faults.md` (verdict, check table S1–S15, BLOCKER and MAJOR
heads, MAJOR-6 in full); `evidence-gate/evidence-verdict.md` (via synthesis);
`proof-attempt__0/proof-attempt-0.md` §0–3; `proof-attempt__1/proof-attempt-1.md` §0–1,
§8; `proof-attempt__2/proof-attempt-2.md` §0–3; `notebooks__2/findings-2.md` §0–1;
`concept-cards/CC-09`, `CC-10`.

Every Lean type quoted in §4 was read from the actual `.lean` source, **not** from a
report's restatement.

---

## 4. Corrections made during authoring

Recorded rather than silently applied, per the run's honesty discipline.

| # | Upstream statement | Correction in `paper.md` | How established |
|---|---|---|---|
| **C1** | `synthesis.md` §1: at the record locus "the fence sits at **0.9715**" (in units of $\log^2 p$) | **0.97059** | Recomputed here in 60-digit `mpmath` from exact integers: $f_k/\log^2 p_k = 0.970588744671\ldots$. The synthesis' companion figures ($R_{\mathrm{csg}} = 0.9206$, shortfall $1.05426$) both reproduce exactly, so this is an isolated slip in one derived figure, not a systematic error. **It does not change any conclusion** — the shortfall factor $f_k/g_k$ is the load-bearing quantity and is unaffected. |
| **C2** | `proof-attempt__0` §0 headlines "FFM proved on $[89,10^{20})$" | Not carried. §5.3 states the **vacuity** explicitly and carries the non-vacuous certificate $G_{n-1} < f_n$ instead. | MAJOR-3; PA-0's own Proposition 2 |
| **C3** | Crossover-height figure ($10^{30}$–$10^{51}$) used without its model tag | §7.4 states it as a 30-decade extrapolation from a model fit, never as a property of the primes. | MAJOR-9 |
| **C4** | Corpus-wide "shortfall 5.5%" / "$1.055$" | $1.05426$ throughout, sourced to the published exact barrier. | source-ledger §1 finding 2, §5 |
| **C5** | Corpus-wide "verified to $4\times10^{18}$" as *the* frontier | §11.3 note 2: frontier is $2^{64}$ (Visser); $4\times10^{18}$ is the exhaustive-sieve range; census reaches $10^{20}$. | source-ledger §1 finding 6 |

Two upstream figures were checked and **confirmed**, not corrected: $R_{\mathrm{csg}} =
0.9206385885574205$ and shortfall $= 1.05425598789\ldots$, both recomputed here from
exact integer inputs at 60 decimal digits.

---

## 5. Claims deliberately NOT carried into the paper

| claim | why not |
|---|---|
| "44 rows independently recomputed" (provenance) | **BLOCKER-1** — false; 30 were. §7.2 reports only counts the audit itself verified. |
| "the verified range excludes $C \ge 1.1736$" | **BLOCKER-2** — invalid inference; a finite range cannot bound a $\limsup$. §7.4 states the $\limsup$ is unknown. |
| $B_n = L^2-L-1-3/L-13/L^2+O(L^{-3})$ | Gap **G1** — unsourced. Only the published $+o(1)$ form is used (§1.3). |
| Baker–Harman–Pintz $0.525$ as an *explicit* unconditional bound | Gap **G3** — the threshold $x_0$ is ineffective as published. Used only asymptotically (§1.4), with the caveat in §9, limitation 6. |
| Any live Nicely URL | Gap **G5** — all return HTTP 404 as of 2026-07-24. |
| "the CSG ratio has never exceeded 1" | Gap **G6** — false without $p > 7$. §5.1 carries the restriction and the three exceptions. |
| Kourbatov's $-1$ / $-1.17$ conditions as formalised | Debt **D3** — they are **not** formalised; `firoozbakht_of_gap_lt` is a different, weaker, elementary criterion and §4.3 says so. |
| Any statement of the form "Firoozbakht proved/published" | No primary document exists (§11.3, absences). |
| "the community expects F to be false" as a sourced survey claim | Gap **G2** — restated as the specific Granville heuristic (§7.4, §10). |

---

## 6. Verification performed by this leg

- **Recomputation** (60-digit `mpmath`, exact integer inputs, no RNG, no network): the
  five record-locus quantities of §5.1. Four reproduce the upstream values exactly; one
  produced correction **C1**.
- **Lean type fidelity**: all 14 declarations named in §4 read verbatim from the `.lean`
  sources; **RT-12 discharged** for each (§4.4) — no declaration takes any form of the
  conjecture as a hypothesis.
- **Bibliography integrity**: `references.bib` BLOCK A is a byte-copy of the ledger's
  §7 BibTeX (31 entries, count verified programmatically); BLOCK B is authored here and
  labelled unmerged.
- **No `lake build` re-run** by this leg — the kernel leg's transcript is read as the
  record, and §9 limitation 9 says so.

---

## 7. Gates and commit

`.cosmon/config.toml` defines no `[gates]` for this project. Run artifacts live under
`.cosmon/state/`, which `.cosmon/.gitignore` excludes from version control, so this leg
has nothing to commit to the git worktree — the same condition every other leg of this
run worked under. Artifacts are written to the run's `write-paper/` directory and
mirrored to the molecule directory, per the briefing.

---

## 8. Step-2 review pass — checklist and fixes

Self-review at authoring time. The **independent adversarial review is a separate
downstream molecule** (`editorial-verdict`) and was deliberately not simulated here.

### 8.1 Coherence

- **Cross-reference integrity**: every `§x.y` in `paper.md` was extracted and matched
  against the actual heading list. **Three dangling references found and fixed**:
  - `§9.4` → `§9, limitation 4` (§9's numbered list is not subsections);
  - `§9.6` → `§9, limitation 6` (same, in the §11.1 table — and the same slip in §5 of
    this log, also fixed);
  - `§2.1` → `§11.3` in §0.3 (the Firoozbakht-attribution absences live in §11.3, not in
    the method section).
  The `§3.1`–`§3.8` references in §11.1 are **not** dangling: they are the *source
  ledger's* sections, and the table's preamble and column header both say so.
- **Count consistency**: the §4.4 RT-12 table has 14 rows; §2.4 and §4.4 said
  "thirteen"/"13". **Fixed to fourteen/14** in both the paper and this log. (The count
  differs from the kernel leg's "11 new theorems" because §4.4 also covers three
  pre-existing skeleton bridges the paper names as results, and collapses
  `nthPrime_eq_1…_26` into one row — both stated in the table.)
- **No contradictions found** between §4 (proved) and §5/§6 (established): the word
  "proved" appears for results only inside §4, and §0.1 fixes the rule.
- **No orphaned definitions**: $g_n$, $L_n$, $f_n$, $B_n$, $\sigma_n$, $a_n$, $S_n$,
  $R_{\mathrm{csg}}$, $\underline{B}$, $\psi$ each defined before first use.
- **No tone shift**: §1.4 and §13 carry the picture register deliberately and are marked
  as such by position (framing and closing); the body stays technical throughout.

### 8.2 Completeness

Every brief requirement has a section — see §1 of this log. No requirement is
unaddressed; the single partial (citation tracing) is flagged in three places in the
paper rather than dropped.

### 8.3 Compliance

| constraint | check |
|---|---|
| Delivery posture **staged** | §0.2 (page one, with the three unpassed gates), §9.1, colophon. ✓ |
| External attribution **Noogram** | title block, §0.3, colophon. **No fund affiliation, no operator name, no private path anywhere** — verified by grep. ✓ |
| 'proved' only for kernel targets | §0.1 rule; §4 only; §4.4 RT-12 table. ✓ |
| No canonical legal/licence text generated | none present. ✓ |
| Confidentiality | no internal names, credentials, or private paths; run-relative paths only (`<run>/…`). ✓ |

### 8.4 Sources — spot-check performed

The step requires spot-checking **at least one** citation against its source. Two were
checked, one by re-fetch and one by recomputation:

1. **`visser2019verifying`, Abstract (V0)** — re-fetched `arxiv.org/abs/1904.00499`
   on 2026-07-24. Abstract reads verbatim: *"all three of these conjectures are
   unconditionally and explicitly verified for all primes below the location of the 81st
   maximal prime gap, certainly for all primes $p < 2^{64}$."* **Matches the ledger row
   and the paper's use of it in §11.3 note 2.** ✓
2. **`kourbatov2015upper`, Table 1 last row (V0 + C)** — the published
   $f_k = 1193.418$ and $\ell_k = 1194.516$ reproduce here to
   $1193.41777829404\ldots$ and $1194.51592191172\ldots$ in 60-digit arithmetic from
   exact integer inputs. ✓ (This same recomputation produced correction **C1**.)

### 8.5 Unresolved — deferred with justification

| item | why deferred, and to whom |
|---|---|
| The six unmerged ledger rows (§2 above) | Merging rows into the authoritative ledger is **not this leg's authority** — the ledger is another leg's artifact, and editing it from here would be exactly the silent-patch pattern the run forbids. Flagged as a release blocker (`paper.md` §9.1 item 3) and handed to `citation-gate`. |
| **MAJOR-6** (Axler locator) | Resolving it needs the *published* Axler numbering, which no leg obtained (the corrigendum gives "Corollary 3.4, page 8"; the corpus cites arXiv v3's "3.5"). Disclosed at the point of use (`paper.md` §5.2) and made release blocker item 2. |
| **BLOCKER-1**, **BLOCKER-2** | They are faults in *upstream* artifacts (`notebooks__2` provenance and findings). This paper does not carry either claim (§5 of this log); repairing the upstream files is not this leg's scope. |
| Independent adversarial review | By design a separate molecule (`editorial-verdict`). |
