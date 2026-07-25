# Citation gate — verification report

**Leg**: `citation-gate` · **Molecule**: `cite-20260724-f7a3` · formula `citation-audit` · crew role **editor**
**Audited artefact**: `write-paper/paper.md` (909 lines) + `write-paper/references.bib` (37 entries)
**Authority**: `source-ledger/source-ledger.md` (567 lines)
**Input**: `citations.json` (this directory) — 27 audit entries / 23 distinct citekeys

---

## 0. Verdict

> ## **BLOCKED**
>
> Zero L3. Zero fabricated citations. **Six citations do not trace to any
> source-ledger row** — the fail-closed condition of this gate. Seven entries
> are `L2_weak` and require human review.

| gate predicate | result |
|---|---|
| paper present | **YES** — `write-paper/paper.md` |
| ledger present | **YES** — `source-ledger/source-ledger.md` |
| zero unresolved **L3** | **YES** — 0 entries |
| zero **fabricated** citations | **YES** — every citekey resolves to a real, correctly-identified document; all 23 checked |
| **every paper citation traces to a source-ledger row** | **NO** — 6 citekeys have **zero** occurrences anywhere in `source-ledger.md` |
| zero **L2_weak** (advisory, not a pass condition) | **NO** — 7 entries |

**Therefore: BLOCKED.** The offending citations, named:

`cms2019fourier` · `littlewood1914` · `dudek2015riemann` · `leenosal2025sharper` ·
`schoenfeld1976sharper` · `montgomery1973pair`

Of these, **`cms2019fourier` is load-bearing**: Theorems C, D and E of the paper's
§6.1 — the entire Riemann-hypothesis-route obituary — rest on it.

**What is NOT wrong.** No citation in this paper is fabricated. No locator was
found to contradict the statement it is invoked for. Eleven citations were
verified **verbatim against the primary source re-fetched by this leg**, and one
long-standing corpus fault (MAJOR-6) is **resolved and repairable** — the repair
is given below. The paper also discloses its own citation debt honestly in §0.2,
§9 (limitation 4, 5), §11.2 and §11.3; this gate confirms that disclosure is
accurate rather than contradicting it.

---

## 1. Protocol

Tiers are the spore README §4 definitions — they grade how firmly the **locator**
was matched to the exact statement the paper uses it to support:

| tier | meaning |
|---|---|
| **L0** | Canonical/textbook; the precise locator is not load-bearing. |
| **L1** | Primary source obtained, locator **verified** — the cited number really states the claim. *L1 dominates L2: where L1 decides, lower tiers are moot and were not evaluated.* |
| **L2_strong** | Indirect match, corroborated by a second source. |
| **L2_weak** | Plausible, but the exact locator was not confirmed. **Human review.** |
| **L3** | Unresolved: source not locatable, or locator does not support the statement (fabrication risk). **Human review.** |

**Ledger-trace** is a second, orthogonal axis, and it is the one this molecule
fail-closes on: does the citekey appear in a `source-ledger.md` §3 row? A citation
can be `L1` on locator-match and still **FAIL** ledger-trace. `cms2019fourier` is
exactly that case, and conflating the two axes is how a gate gets talked out of
closing.

**Method.** Performed inline, not delegated. This leg re-fetched and re-read
primaries independently of every upstream leg: five arXiv PDFs converted to text,
the *Integers* version of record, the Kourbatov corrigendum from the Journal of
Integer Sequences, two live community/web sources, and a scanned journal PDF read
page-by-page as images because it carries no text layer. Retrieval record in §5.

---

## 2. Per-entry verdicts

### 2.1 Merged rows — trace to `source-ledger.md` §3

| id | citekey | locator relied on | tier | conf | evidence |
|---|---|---|---|---|---|
| C01 | `firoozbakht1982` | attribution + date only, via Visser ref. [24] | **L1** | high | `visser.txt:375` verbatim: `[24] Faride Firoozbakht, (1982), unpublished.` Supports "proposed, in 1982"; the paper correctly never writes "proved/published". |
| C02 | `rivera2002conj30` | page body | **L1** | high | Live page re-fetched: *"(pn)1/n decreases increasing n, in which pn is the nth prime number."* Also verbatim: verification against the table of *"Maximal gaps between consecutive primes <=4.444 \* 10^12"*, and Huxley's assessment *"her Conjecture is a hard one to prove but hasn't been posted before."* The **2002** date is not printed on the page, but is corroborated at V0 by `kourbatov.txt:324` ref. [7], *"Conjecture 30. The Firoozbakht Conjecture, 2002."* |
| C03 | `ribenboim2004little` | p. 185 | **L2_strong** | medium-high | Book not obtained (correctly V2). Locator corroborated verbatim at V0: `kourbatov.txt:24` *"In 1982 Firoozbakht proposed the following conjecture [6, p. 185]"*, with ref. [6] = Ribenboim, *The Little Book of Bigger Primes*, Springer, 2004. **Note**: Kourbatov's ref. does not say "2nd ed."; the paper's "2nd ed." is an unverified refinement. |
| C04 | `visser2019verifying` | Conj. 3 eq. (2.4); Abstract; §2; §4 | **L1** | high | Four locators verified verbatim in arXiv:1904.00499v2. eq. (2.4): `gn ≤ pn(pn^{1/n} − 1)` for `n ≥ 1` (`visser.txt:119–121`). Abstract: *"certainly for all primes p < 2^64"* (`:35`). Chain direction: *"the standard inequalities n ln n < pn < n ln pn show that Farhadian ⟹ Nicholson ⟹ Firoozbakht"* (`:112–113`). `p*81 > 2^64` established September 2018 (`:85`). |
| C05 | `kourbatov2015verification` | Abstract | **L1** | high | Abstract verbatim: *"We use the table of first-occurrence prime gaps in combination with known bounds for the prime-counting function to verify the Firoozbakht conjecture for primes up to four quintillion (4×10^18)."* Journal ref. and v3 (2023) match the ledger. **Minor**: the ledger renders this row as *"conjecture (1) is true for all primes p_k < 4×10^18"* — that wording is Theorem 2 of `kourbatov2015upper`, not this abstract. The paper's own use (§4.1, §7.1) is fully supported either way. |
| C06 | `kourbatov2015upper` **Thm 5** | asymptotic of the exact barrier | **L1** | high | `kourbatov.txt:254–255` verbatim: *"Theorem 5. Let pk be the k-th prime, and let fk = pk^{1+1/k} − pk, then fk = log² pk − log pk − 1 + o(1) as k → ∞."* Confirms the paper's §1.3 use **and** confirms ledger gap G1: the `−3/L − 13/L²` terms appear nowhere in this theorem. The paper correctly does not use them. |
| C07 | `kourbatov2015upper` **Table 1, last row** | record locus + published exact barrier | **L1** | high | `kourbatov.txt:152` verbatim, all five fields on one line: `49749629143526  1693182318746371  1132  1193.418  1194.516`. Every number the paper's §5.1 table attributes to this locator is present. |
| C08 | `kourbatov2015corrigendum` | full text | **L1** | high | Re-fetched from `cs.uwaterloo.ca/journals/JIS/VOL18/Kourbatov/k3c.pdf`. Verbatim: *"In inequality (11), replace 'x ≥ 5.43' with 'x ≥ 2634800823'"*; *"for pk ∈ [29, 2634800823] both (1) and (10) hold unconditionally"*; *"These changes have been incorporated in the arxiv paper arXiv:1506.03042v4."* Supports the paper's §11.3 item 1 exactly, including the instruction to cite v4. |
| C09 | `primegaplist_faq` (CSG record) | FAQ body | **L1** | high | Live page verbatim: *"For the 1132 gap, the ratio is 0.9206, the largest value observed for any p1 > 7 thus far."* The `p1 > 7` restriction is present in the source, so the paper's §5.1 caveat is correct and the "never exceeded 1" phrasing is indeed false without it. |
| C10 | `primegaplist_faq` (census, P2 of Theorem A) | FAQ body | **L2_weak** | medium | The completeness half is verbatim: *"All prime gaps in 0 < x < 10^20 have now been analyzed."* ✓ The **count is not**: the paper's P2 reads *"complete below 10^20, yielding the 85 known maximal gaps"*, and the FAQ states **no count of maximal gaps at all**. The 85-row table is sourced elsewhere in the corpus (OEIS b-files, cross-checked by the skeptic leg) — but not at this locator, on the paper's single weakest premise. **ESCALATED.** |
| C11 | `axler2014newbounds` **Cor. 3.5 (arXiv v3)** — the 3.83 member; P1 of Theorem A | analytic criterion | **L2_weak** | medium-high | The statement **is** at the named locator in the named version: `axler.txt:310–313` verbatim, *"and for every x ≥ 9.25, we have π(x) < x/(log x − 1 − 1/log x − 3.83/log²x)"*, inside Corollary 3.5 of arXiv:1409.1780v3. **But MAJOR-6 is real**: in the version of record — *Integers* **16** (2016), A22, which is what the bibliographic entry points at — the same four-member family is **Corollary 3.4, page 8**, and published Corollary 3.5 is a different (lower-bound) family. A referee checking the journal version at "Corollary 3.5" lands on the wrong result. **ESCALATED — with the repair now established, see §3.** |
| C12 | `axler2016corrigendum` | via `kourbatov2015corrigendum` ref. [A] | **L2_strong** | medium-high | Corrigendum not obtained directly (correctly V2). Its bibliographic form and its entire load-bearing content are fixed verbatim by the V0 Kourbatov corrigendum: ref. [A] = *"C. Axler, Corrigendum to 'New bounds for the prime counting function', Integers 16 (2016), A22, 15 pp."*, and the `5.43 → 2634800823` correction is quoted there. |
| C13 | `dusart2010estimates` **Props. 6.6, 6.7** | the $p_k$ bracket | **L1** | high | `dusart.txt:427–431` verbatim: *"Proposition 6.6. For k ⩾ 688 383, pk ⩽ k(ln k + ln₂k − 1 + (ln₂k − 2)/ln k)."* `:435–439`: *"Proposition 6.7. For k ⩾ 3, pk ⩾ k(ln k + ln₂k − 1 + (ln₂k − 2.1)/ln k)."* Both validity ranges match the paper's §5.1 use. |
| C14 | `granville1995harald` display after eq. (20); eq. (14) | Granville's $2e^{-\gamma}$ heuristic | **L1** | high | The PDF has **no text layer**; pages were read as images. **p. 24** verbatim, the unnumbered display immediately after eq. (20): *"max_{pn ≤ x}(p_{n+1} − p_n) ≳ 2e^{−γ} log² x, which contradicts Cramér's conjecture (14)!"* **p. 21**: eq. (14) is *"max_{pn ≤ x}(p_{n+1} − p_n) ∼ log² x."* Both locators are exactly as the ledger states. Granville presents it as a suggestion of the revised model, not a theorem — and the paper says so. |
| C15 | `cramer1936order` | block quotation in Granville, immediately preceding eq. (14) | **L2_strong** | high | 1936 primary not obtained (correctly V2), but the secondary locator is verified verbatim on **p. 21**, and it carries the hedge the paper's §11.3 item 5 insists on: *"With a probability = 1, the relation limsup (P_{n+1}−P_n)/(log P_n)² = 1 is satisfied.—Obviously we may take this as a suggestion that, for the particular sequence of ordinary prime numbers pn, some similar relation may hold."* The paper's demand that "Cramér conjectured" carry a hedge is supported by the source's own words. |
| C16 | `bhp2001difference` | main theorem | **L2_strong** | medium-high | Primary paywalled (HTTP 402; correctly V2). Metadata confirmed exact — *Proc. London Math. Soc.* (3) **83** (2001), no. 3, 532–562. Result confirmed via reliable secondaries: `[x, x + x^{0.525}]` contains primes for large `x`, hence `g_n ≪ p_n^{0.525}`. The paper's §9 limitation-6 caveat about the ineffective threshold is correct and load-bearing. |
| C17 | `fgkmt2018long` | Abstract | **L1** | high | Abstract verbatim from arXiv:1412.5029: the denominator is `log log log X` to the power **1**, not squared. Journal ref. *J. Amer. Math. Soc.* **31** (2018), no. 1, 65–105 matches. The paper's §11.3 item 4 hygiene note is correct. |

### 2.2 Unmerged citekeys — **ledger-trace FAIL** (the gate-blocking set)

Verified by grep: each of these six citekeys has **0 occurrences** in
`source-ledger.md`. Not one is fabricated — all six resolve to real, correctly
identified documents — but none is in the authoritative ledger, and `references.bib`
marks each with an explicit `PROPOSED ledger row … NOT MERGED` note.

| id | citekey | tier (locator-match) | ledger-trace | conf | evidence |
|---|---|---|---|---|---|
| C18 | `cms2019fourier` | **L1** | **FAIL** | high | Full text re-fetched (arXiv:1708.04122). `cms.txt:253–257` verbatim: *"assuming the Riemann hypothesis, prove that p_{n+1} − p_n ≤ (22/25)√p_n log p_n for all primes p_n > 3."* And `:239–240` verbatim: *"We note from (1.12) and Theorem 2 (b) that the limit of this method would yield a constant ½ on the right-hand side of (1.14)."* Metadata confirmed: *Comment. Math. Helv.* **94** (2019), no. 3, 533–568, DOI 10.4171/CMH/467. **The citation is impeccable on locator-match and still blocks the gate: it is not in the ledger, and §6.1's Theorems C, D and E rest on it.** |
| C19 | `littlewood1914` | **L0** | **FAIL** | medium | The $\Omega_\pm$ theorem is canonical textbook material, so under the tier definition the precise locator is not load-bearing, and the paper uses it only for an explanatory remark it flags as V3 and excludes from Theorems C–E. Metadata (*C. R. Acad. Sci. Paris* **158** (1914), 1869–1872) is the standard citation. No ledger row exists. |
| C20 | `dudek2015riemann` | **L2_strong** | **FAIL** | high | The claim made of it — "earlier RH-conditional constant" — is corroborated verbatim by a V0 secondary: `cms.txt:201–202`, *"the current best form of this bound is due to Dudek [19, Theorem 1.3], who obtained (1.10) with constant c = 1"*, plus `:257–258` for the effective `c = 3`. No ledger row. Declared lineage-only in the paper. |
| C21 | `leenosal2025sharper` | **L2_weak** | **FAIL** | medium | The paper exists (arXiv:2312.05628; *J. Number Theory* **283**, 2026), so this is not a fabrication. But **the bib entry has a wrong author forename** — `Nosal, Patrick` where the author is **Paweł Nosal** — the year `2025` is the arXiv-v3 year rather than the publication year, and no locator is given at all. Not load-bearing. **ESCALATED.** |
| C22 | `schoenfeld1976sharper` | **L2_weak** | **FAIL** | medium | Metadata correct and standard (*Math. Comp.* **30** (1976), 337–360). No locator; tier disputed upstream (MAJOR-7); declared **not used** in the paper. **ESCALATED.** |
| C23 | `montgomery1973pair` | **L2_weak** | **FAIL** | medium | Metadata correct and standard (*Proc. Sympos. Pure Math.* **24**, 1973). The pair-correlation consequence is corroborated in passing by `cms.txt:241–243` (*"under … the Riemann hypothesis and Montgomery's pair correlation conjecture, it is known that the limit supremum … is actually zero"*), but with CMS citing [26, 27, 35], **not** Montgomery 1973 directly. No locator; declared **not used**. **ESCALATED.** |

### 2.3 Prose attributions — claims carrying no citekey at the point of use

| id | claim | tier | conf | evidence |
|---|---|---|---|---|
| C24 | §1.4: *"the Riemann hypothesis gives $g_n \ll \sqrt{p_n}\log p_n$"* | **L2_weak** | medium | Load-bearing framing for §1.4's "categorical distance" argument, and it carries **no citekey at the point of use**. The ledger does hold a row for it (`cramer1920some`, §3.6, V2 on the statement) — but `cramer1920some` appears **nowhere** in the paper, neither in the §11.1 merged trace table nor in §11.2. So the paper cannot be said to trace this claim to a ledger row. Corroborated at V0 by `cms.txt:196–200` (*"a classical result of Cramér [14] yields the bound limsup (p_{n+1}−p_n)/√(p_n log p_n) ≤ c"*). **ESCALATED — a one-citekey fix.** |
| C25 | §10: *"The community's heuristic expectation (from Granville's revised Cramér model) is that it is false"* | **L2_weak** | medium | The Granville half is L1-verified (C14). The **"community's expectation"** half is ledger gap **G2**: no survey of community expectation is sourced anywhere. Severity is low — the paper states in the same sentence that it *"is recorded as a heuristic and is used as a premise nowhere"*, and §7.4 states the $\limsup$ is unknown. **ESCALATED at low severity.** |
| C26 | §9 lim. 6 / §11.3: the canonical Nicely gap tables are dead, *"all return HTTP 404 as of 2026-07-24"* | **L0** (no citation made) | high | The paper correctly cites **no** live Nicely URL and substitutes `primegaplist_faq`, which is the right call. **But the HTTP code is wrong**: re-probed today, `faculty.lynchburg.edu/~nicely/…` returns **404** ✓ while `trnicely.net` returns **406 Not Acceptable** — the host is alive and refusing the request, not gone. Operative conclusion (no usable live citation) is unaffected. Recorded as a MINOR correction, not escalated. |
| C27 | §9 lim. 6 / §10: the four-term $B_n$ expansion is unsourced and **not used** | **L0** (no citation made) | high | Confirmed from both directions: ledger gap **G1**, and independently `kourbatov.txt:254–255`, where Theorem 5 proves only the $+o(1)$ form. The paper's declared abstention is accurate. No escalation. |

### 2.4 Tier / trace consistency of the paper's own §11.1 table

Checked all fifteen rows of the paper's §11.1 trace table against the ledger's
§3 verification levels. **All fifteen agree** — V0/V1/V2/`+C` tags match row for
row, with no case of the paper claiming a stronger tier than the ledger grants.
This is the property MAJOR-7 (two incompatible tiers for one source) breaks
upstream; it is **not** broken inside `paper.md`.

One trivial imprecision: §11.1 lists `granville1995harald`'s "used in" as §7.4
only, while eq. (14) is actually used in §10 and §11.3 item 5. No tier effect.

---

## 3. MAJOR-6 — resolved, and the repair

MAJOR-6 has stood open across the corpus as *"the corpus never establishes where
the 3.83 member sits in the published numbering."* This leg fetched the version of
record and established it.

| version | document | the four-member $\pi(x)$ upper-bound family | the 3.83 member, $x \ge 9.25$ |
|---|---|---|---|
| arXiv:1409.1780**v3** | preprint | **Corollary 3.5** | present, verbatim |
| *Integers* **16** (2016), **A22** | version of record | **Corollary 3.4**, **p. 8** | present, verbatim |

In the published paper, **Corollary 3.5 is a different family** — the lower-bound
table indexed by $(a,b,c,d;x_0)$, which is arXiv v3's Corollary 3.6. The numbering
shifted down by one between preprint and journal, so an arXiv-v3 locator quoted
against journal-of-record bibliographic fields lands on the wrong corollary. The
fault was real.

**The repair, either branch, both now checkable:**

1. Re-attribute to the version of record: **Axler, *Integers* 16 (2016), A22,
   Corollary 3.4, p. 8** — and correspondingly, every corpus citation of arXiv v3
   "Corollary 3.6" (Kourbatov's eq. (5), the load-bearing lower bound) becomes
   published **Corollary 3.5**. Both shift by one; fixing only the first would
   create a second fault.
2. Or keep arXiv v3 numbering and say so **explicitly at every point of use**,
   with the bibliographic entry naming arXiv:1409.1780v3 as the cited artefact
   rather than the *Integers* article.

One further note the paper should absorb: §5.2 currently says *"The published
corrigendum numbers the affected result Corollary 3.4, page 8."* It is the
**published article itself** that numbers it 3.4, not the corrigendum. The
corrigendum's role is the separate `5.43 → 2 634 800 823` validity-range fix.

This does **not** unblock the gate. It closes item 2 of the paper's own §9.1.

---

## 4. Summary

| tier | count | ids |
|---|---|---|
| **L0** | 3 | C19, C26, C27 |
| **L1** | 11 | C01, C02, C04, C05, C06, C07, C08, C09, C13, C14, C17 |
| **L2_strong** | 5 | C03, C12, C15, C16, C20 |
| **L2_weak** | 8 | C10, C11, C21, C22, C23, C24, C25 — *and see note* |
| **L3** | **0** | — |
| **fabricated** | **0** | — |

*Note on the count: seven entries are `L2_weak` on locator-match (C10, C11, C21,
C22, C23, C24, C25). Escalations in §5 of `escalations.md` total nine rows,
because the two `FAIL`-trace entries that are not themselves `L2_weak` — C18
(`L1`) and C19 (`L0`) — escalate on the ledger-trace axis instead.*

| ledger-trace | count |
|---|---|
| traces to a §3 row | 17 |
| **FAIL — no row exists** | **6** (C18–C23) |
| no citation made (declared absence) | 2 (C26, C27) |
| claim with no citekey at point of use | 2 (C24, C25) |

**Primaries re-fetched and read verbatim by this leg: 11.** That is the substance
behind the paper's §9 limitation 4 (*"No citation here has been independently
re-fetched and re-read by an auditor"*) — which was true when written and is no
longer true. Limitation 4 should be restated once this gate's findings are folded
in; **limitation 5 (six unmerged rows) stands exactly as written.**

---

## 5. Retrieval record

Obtained by this leg, 2026-07-24, and read directly:

| source | retrieved from | form |
|---|---|---|
| `kourbatov2015upper` | `arxiv.org/pdf/1506.03042v4` | PDF → text, full |
| `kourbatov2015corrigendum` | `cs.uwaterloo.ca/journals/JIS/VOL18/Kourbatov/k3c.pdf` | PDF → text, full |
| `axler2014newbounds` (preprint) | `arxiv.org/pdf/1409.1780v3` | PDF → text, full |
| `axler2014newbounds` (**version of record**) | *Integers* 16 (2016) A22 via EMIS `emis.de/ft/19586` | PDF → text, full — **new to this run** |
| `visser2019verifying` | `arxiv.org/pdf/1904.00499v2` | PDF → text, full |
| `dusart2010estimates` | `arxiv.org/pdf/1002.0442v1` | PDF → text, full |
| `cms2019fourier` | `arxiv.org/pdf/1708.04122` | PDF → text, full — **new to this run** |
| `granville1995harald` | `dms.umontreal.ca/~andrew/PDF/cramer.pdf` | scanned PDF, **no text layer** — pp. 21–24 read as images |
| `rivera2002conj30` | `primepuzzles.net/conjectures/conj_030.htm` | HTML, live |
| `primegaplist_faq` | `primegap-list-project.github.io/faq/` | HTML, live |
| `kourbatov2015verification` | `arxiv.org/abs/1503.01744` | abstract page (V1) |
| `fgkmt2018long` | `arxiv.org/abs/1412.5029` | abstract page (V1) |
| `nicely_gaplist` | `trnicely.net`, `faculty.lynchburg.edu/~nicely/` | HTTP probe: **406** / **404** |

Not obtained: `ribenboim2004little` (book); `bhp2001difference` (HTTP 402
paywall); `axler2016corrigendum` (content fixed by the V0 Kourbatov corrigendum);
`cramer1936order` (read through Granville's block quotation). Each is carried at
`L2_strong` and none is load-bearing without corroboration.

---

## 6. What must close for this gate to turn

1. **Merge or drop the six unmerged rows** into `source-ledger.md` §3 — the
   blocking condition. `cms2019fourier` must be merged at **V0**: this leg
   re-read both of its load-bearing statements verbatim, so the evidence for the
   merge is in §2.2 above and no further retrieval is needed. `littlewood1914`
   either promotes or the §6.1 explanatory remark restates as conditional.
   `schoenfeld1976sharper` and `montgomery1973pair` are unused — dropping them
   from `references.bib` is the cheaper close.
2. **Repair the MAJOR-6 locator** per §3 — both directions of the off-by-one, not
   just the upper bound.
3. **Give C24 a citekey.** §1.4's RH-conditional bound needs `cramer1920some`
   (already a ledger row, §3.6) at the point of use.
4. **Restate P2 of Theorem A** so the "85 maximal gaps" count cites whatever
   actually establishes it, not the FAQ, which states no count.
5. **Fix the `leenosal2025sharper` author forename** (Paweł, not Patrick) if the
   entry survives at all.
6. **Correct the Nicely HTTP code** in §9 limitation 6 / §11.3 — 406 for
   `trnicely.net`, 404 for the Lynchburg host.
7. **Restate limitation 4** — an auditor has now re-fetched and re-read eleven
   primaries.

Items 1 and 2 are the paper's own §9.1 items 3 and 2. **None of this is
mathematics, and none of it touches the conjecture, which remains open.**

---

*Emitted by the `citation-gate` leg (`cite-20260724-f7a3`) of the `math-attack`
polymer, run node. External attribution: **Noogram**.
Verdict: **BLOCKED** — 0 L3, 0 fabricated, 6 citations untraceable to the
authoritative ledger.*
