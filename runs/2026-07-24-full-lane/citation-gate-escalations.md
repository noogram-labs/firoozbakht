# Citation-gate escalations — human review required

**Leg**: `citation-gate` · **Molecule**: `cite-20260724-f7a3` · formula `citation-audit`
**Target**: `write-paper/paper.md` · **Authority**: `source-ledger/source-ledger.md`
**Report**: `verification-report.md` (this directory) · **Input**: `citations.json`

**Gate verdict: BLOCKED.** Not an empty delta. Nine rows below.

Escalation triggers are two, and they are independent:
- **tier** `L3` or `L2_weak` — the locator was not confirmed against the exact statement (spore README §4);
- **ledger-trace FAIL** — the citekey appears in **no** `source-ledger.md` §3 row, which is this molecule's fail-closed condition.

A citation can be `L1` on locator-match and still escalate on trace. **E-01 is
exactly that**, and it is the most consequential row here.

---

## Escalation rows

| # | target | citekey | locator | tier | trace | reason | proposed action |
|---|---|---|---|---|---|---|---|
| **E-01** | `paper.md` §6.1 (Theorems C, D, E) + §11.2 | `cms2019fourier` | RH bound $g_n \le \tfrac{22}{25}\sqrt{p_n}\log p_n$ for $p_n>3$; method limit $c=\tfrac12$ | **L1** | **FAIL** | **Load-bearing and untraceable.** The whole RH-route obituary — Theorems C, D and E — rests on a citation with **zero occurrences** in `source-ledger.md`. Locator-match is impeccable: this leg re-fetched arXiv:1708.04122 and read both statements verbatim (`cms.txt:253–257`, `:239–240`), and metadata checks exact (*Comment. Math. Helv.* **94** (2019), no. 3, 533–568, DOI 10.4171/CMH/467). The fault is provenance, not truth. | **Merge into `source-ledger.md` §3.6 at V0.** The verbatim evidence and the retrieval record are already in `verification-report.md` §2.2 — no further retrieval needed. Until merged, §6.1 must not ship. |
| **E-02** | `paper.md` §5.2 (P1 of Theorem A) + §11.1 | `axler2014newbounds` | Cor. 3.5 (arXiv v3) — the $3.83$ member, $x \ge 9.25$ | **L2_weak** | ✓ | **MAJOR-6, now diagnosed exactly.** The statement is verbatim at Corollary 3.5 of arXiv:1409.1780**v3** (`axler.txt:310–313`) ✓ — but in the version of record (*Integers* **16** (2016), **A22**, which is what the bibliographic entry points at) the same family is **Corollary 3.4, p. 8**, and published Corollary 3.5 is a *different*, lower-bound family. A referee checking the journal version at "Cor. 3.5" lands on the wrong result. | **Repair both directions of the off-by-one, or neither.** Either re-attribute to *Integers* 16 (2016) A22 **Cor. 3.4, p. 8** — and simultaneously renumber every corpus citation of arXiv-v3 "Cor. 3.6" (Kourbatov's eq. (5)) to published **Cor. 3.5** — or keep v3 numbering and name arXiv:1409.1780v3 as the cited artefact at **every** point of use. Fixing only the upper bound creates a second fault. Also: §5.2 credits the "Cor. 3.4, p. 8" numbering to the *corrigendum*; it is the **published article** that numbers it so. |
| **E-03** | `paper.md` §5.2 (P2 of Theorem A), §7.1 | `primegaplist_faq` | FAQ body — census completeness | **L2_weak** | ✓ | The completeness half is verbatim ✓ (*"All prime gaps in 0 < x < 10^20 have now been analyzed"*). The **count is not at the locator**: P2 reads *"complete below $10^{20}$, yielding the 85 known maximal gaps"*, and the FAQ states **no count of maximal gaps at all**. This sits on Theorem A's single weakest premise — the same premise whose provenance record already carries BLOCKER-1 (44 rows claimed recomputed, 30 actually). | **Split the premise.** Cite `primegaplist_faq` for completeness-to-$10^{20}$ only, and cite whatever actually establishes the 85-row table (the OEIS b-files, whose 85/85 cross-check the skeptic leg performed) for the count. Do not let one citekey carry both. |
| **E-04** | `paper.md` §1.4 | *(none — no citekey at point of use)* | none | **L2_weak** | **n/a** | §1.4 asserts *"the Riemann hypothesis gives $g_n \ll \sqrt{p_n}\log p_n$"* with **no citation at all**, and it is load-bearing for §1.4's "the distance is categorical" framing. A ledger row exists (`cramer1920some`, §3.6, V2) but that citekey appears **nowhere** in the paper — neither §11.1 nor §11.2 — so the paper does not trace this claim. Corroborated at V0 by `cms.txt:196–200` (*"a classical result of Cramér [14]"*). | **One-citekey fix**: attach `cramer1920some` at the point of use, and add the row to §11.1. Carry the ledger's own disambiguation warning — the correct citation is Cramér **1920**, not "1920/21", and there is a distinct companion paper the same year. |
| **E-05** | `paper.md` §6.1 + §11.2 | `littlewood1914` | $\psi(x)-x = \Omega_\pm(\sqrt{x}\log\log\log x)$ | **L0** | **FAIL** | Untraceable — zero occurrences in `source-ledger.md`. Tier is **L0** (the $\Omega_\pm$ theorem is canonical textbook material, so the precise locator is not load-bearing) and the paper already flags it V3 and excludes it from Theorems C–E, so severity is **low**. But the gate's trace condition is unconditional. | **Merge at V1 or restate.** Either add the ledger row (metadata is the standard *C. R. Acad. Sci. Paris* **158** (1914), 1869–1872) or restate §6.1's explanatory remark as *"the literature is reported to contain…"* per the ledger's own §0 V3 discipline. |
| **E-06** | `paper.md` §11.2 | `dudek2015riemann` | earlier RH-conditional constant | **L2_strong** | **FAIL** | Untraceable — zero occurrences in `source-ledger.md`. Content is well corroborated at V0 (`cms.txt:201–202`, verbatim: *"the current best form of this bound is due to Dudek [19, Theorem 1.3], who obtained (1.10) with constant c = 1"*), and the paper declares it lineage-only. | **Merge at V2** (corroborated by the V0 CMS secondary) or drop the entry — it carries no claim in the body. |
| **E-07** | `paper.md` §11.2 | `leenosal2025sharper` | *"sharper RH error bounds"* — no locator | **L2_weak** | **FAIL** | Untraceable, **and the bib metadata is wrong**: `references.bib` gives author `Nosal, Patrick`; the author is **Paweł Nosal**. Year `2025` is the arXiv-v3 year, not the publication year (*J. Number Theory* **283**, 2026). No locator at all. Not fabricated — arXiv:2312.05628 is real — and not load-bearing. | **Drop the entry.** It is declared not load-bearing and has no locator; merging it would mean sourcing a claim the paper does not make. If it is kept for lineage, fix the forename and the year first. |
| **E-08** | `paper.md` §11.2 | `schoenfeld1976sharper` | *"classical RH bound"* — no locator | **L2_weak** | **FAIL** | Untraceable; no locator; tier **disputed upstream** (MAJOR-7: V3 vs V2 for the same source); declared **not used** in the paper. Metadata is correct and standard (*Math. Comp.* **30** (1976), 337–360). | **Drop from `references.bib`.** Cheapest close: an unused citekey with a disputed tier is pure liability. Dropping it also retires MAJOR-7's paper-side surface. |
| **E-09** | `paper.md` §11.2 | `montgomery1973pair` | *"pair-correlation consequence"* — no locator | **L2_weak** | **FAIL** | Untraceable; no locator; declared **not used**. Corroborated only obliquely — `cms.txt:241–243` notes that under RH **plus** Montgomery's pair-correlation conjecture the limsup is zero, but CMS cites [26, 27, 35] there, **not** Montgomery 1973 directly. Metadata correct (*Proc. Sympos. Pure Math.* **24**, 1973). | **Drop from `references.bib`**, same reasoning as E-08. |

---

## Recorded but NOT escalated

Two `L2_weak`-adjacent items and two declared absences were assessed and
deliberately left off the escalation list. Recording the non-escalation is part of
the audit.

| item | why not escalated |
|---|---|
| §10's *"the community's heuristic expectation … is that it is false"* (C25, `L2_weak`) | Ledger gap **G2** — no survey of community expectation is sourced. But the paper states in the **same sentence** that it *"is recorded as a heuristic and is used as a premise nowhere"*, and §7.4 states the $\limsup$ is unknown. The disclosure already carries the load an escalation would. Fold into E-04's edit pass if convenient; it blocks nothing. |
| §9 lim. 6 / §11.3 Nicely dead links (C26) | The paper cites **no** live Nicely URL and substitutes `primegaplist_faq` — the correct call. **One MINOR factual correction**: re-probed 2026-07-24, `faculty.lynchburg.edu/~nicely/…` → **404** ✓ but `trnicely.net` → **406 Not Acceptable** (host alive, refusing the request). "All return HTTP 404" is wrong; the operative conclusion is unaffected. Fix in copy-edit. |
| §9 lim. 6 / §10, the four-term $B_n$ expansion (C27) | Confirmed unsourced from both directions (ledger gap **G1**; and `kourbatov.txt:254–255` shows Theorem 5 proves only the $+o(1)$ form). The paper explicitly does **not** use the lower-order terms. A correctly declared absence is not a citation fault. |
| §9 limitation 4 — *"The citation audit has not run"* | True when written; **no longer true**. This leg re-fetched and read **eleven** primaries verbatim. Restate limitation 4 when folding in this report. **Limitation 5 (six unmerged rows) stands exactly as written** — this gate confirms it. |

---

## Disposition

**Nine escalations, zero of them mathematics.** Six are one provenance question —
*merge the row or drop the citekey* — and of those six only **E-01** touches a
result the paper actually leans on. Two are locator repairs (**E-02**, **E-04**),
and one is a premise that bundles two claims under one citekey (**E-03**).

Closing E-01 through E-04 is the whole distance between **BLOCKED** and a
citation-clean paper. E-05 through E-09 close by editing `references.bib` and the
ledger, not by retrieving anything new — this leg already read every primary that
matters.

**No citation in this paper is fabricated, and no locator was found to contradict
the statement it supports.** The conjecture remains **open**; nothing in this
escalation list bears on it.

---

*Emitted by the `citation-gate` leg (`cite-20260724-f7a3`) of the `math-attack`
polymer, run node. External attribution: **Noogram**.*
