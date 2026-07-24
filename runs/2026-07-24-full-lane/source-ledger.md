# Source ledger — Firoozbakht's conjecture

**Leg**: `source-ledger` (node 3 of the `math-attack` polymer)
**Date**: 2026-07-24
**Seed anchors**: none — built from scratch.

> **What this document is.** The authoritative bibliography for the attack on
>
> $$p_{n+1}^{1/(n+1)} < p_n^{1/n}\quad\text{for all } n\ge 1
> \qquad\Longleftrightarrow\qquad \big(p_n^{1/n}\big)_{n\ge1}\ \text{strictly decreasing.}$$
>
> **The conjecture is open.** Nothing here proves or refutes it, and no row below
> is evidence about its truth. Every claim the final paper makes must trace to a
> row in §3; `citation-gate` verifies that tracing against this file.
>
> **What a row is.** A row is *(citekey, verification level, precise locator,
> exact statement as the source writes it)*. A row is **not** an endorsement of
> the statement's truth — only of the fact that the named source, at the named
> locator, says that thing.

---

## 0. Verification protocol

Tiering here governs **how well the citation was checked**, not how true the
statement is. It is deliberately *orthogonal* to `decompose.md#0.1`'s L0/L1/L2/L3,
which governs how well a *claim* is established. A V0 row can carry a false
conjecture; a V3 row can carry a theorem.

| Level | Meaning |
|---|---|
| **V0** | The primary source was obtained by this leg and the statement was read **verbatim** at the named locator. The retrieved file is archived (§6). Locator is exact (theorem/proposition/equation/table/page). |
| **V1** | Primary source's own metadata page (arXiv abstract, publisher record) was read; the statement appears there verbatim. Locator resolves to the paper, not to an internal number. |
| **V2** | Statement confirmed from a reliable secondary that quotes the primary (e.g. Kourbatov quoting Axler); primary not obtained by this leg. |
| **V3** | Bibliographic metadata (author, title, venue, year, pages) confirmed; **the specific statement at the specific locator was NOT verified.** Do not lean on a V3 locator. |
| **C** | Additionally **recomputed independently** inside this leg (`verify_ledger.py`, output in `verify_ledger.out`). |

**Discipline.** Any downstream leg quoting a **V3** row must either (a) restate it
as *"the literature is reported to contain …"*, or (b) promote it to V0/V1 first.
`citation-gate` should fail any paper claim resting on a bare V3 locator.

**A retraction rule this leg had to use twice.** Where a source has a published
**corrigendum**, the ledger cites the corrected statement and records the
correction explicitly (rows `axler2016corrigendum`, `kourbatov2015corrigendum`).
An uncorrected locator is a wrong locator even when the paper exists.

---

## 1. Executive summary — what changed upstream

Seven findings that alter claims made in `decompose/decompose.md` or in
`frame-deliberation/outcomes.md`. Each is sourced in §3 and, where numeric,
recomputed in §5.

1. **The record locus and the exact barrier are both published in one table.**
   `decompose.md#7` debt #1 asked for the record CSG locus; Kourbatov's Table 1
   (`kourbatov2015upper`, V0) gives the full row — $k = 49\,749\,629\,143\,526$,
   $p_k = 1\,693\,182\,318\,746\,371$, $g_k = 1132$, **$f_k = 1193.418$**,
   $\ell_k = 1194.516$. Both reproduce to 6 significant figures in §5.
   Debt #1 **closed**, and closed better than asked: the *exact* barrier
   $f_k = p_k^{1+1/k}-p_k$ is a published number at the record locus, so the
   headline shortfall needs **no explicit $p_n$ bound at all**.

2. **The headline shortfall is $1.05426$, not $1.055$.** Directly from
   $f_k/g_k = 1193.4177783/1132$. This confirms `outcomes.md#A3`'s corrected
   value $1.0543$ and refutes `decompose.md#3.2`'s $1.055$. It is a **V0 + C**
   number, so it should be tiered **L0/L1**, not L2.

3. **`outcomes.md#A3`'s replacement constant checks out, and its interval can be
   tightened tenfold.** A3 claims the three-term Dusart bound "pins
   $c_n \in (1.030, 1.034)$ at the record locus (validity $n \ge 688383$)".
   Recomputation from `dusart2010estimates` Props. 6.6/6.7 — now sourced at V0 —
   gives $c_n \in (\mathbf{1.03015},\ \mathbf{1.03332})$, true value $1.031317$:
   **A3 is confirmed.** A3's companion claim also checks out: the two-term
   bracket `decompose.md#2.3-C4` actually cites permits only
   $c_n \in (0.0762,\ 1.0762)$, so C4's stated $c_n \in (0.9, 1.2)$ is indeed not
   derivable from it. The certified shortfall interval is
   $\mathbf{1.05424 \pm 0.00005}$ — ten times tighter than A3's $\pm 0.001$.

4. **The explicit-bounds attribution in `decompose.md#2.3-C4` is misdirected.**
   That section tags its $p_n$/$\pi(x)$ input **[L2, Rosser–Schoenfeld / Dusart]**.
   Kourbatov's actual chain uses **Axler**, `axler2014newbounds` Corollaries 3.5
   and 3.6 (V0) — not Rosser–Schoenfeld, not Dusart. Both families are in the
   ledger, but a paper that writes "Rosser–Schoenfeld / Dusart" while reproducing
   Kourbatov's derivation is mis-citing. Debt #4 **closed** with the corrected
   attribution *and* the Dusart three-term bracket A3 asked for.

5. **The $-1.17$ sufficient condition has a corrected validity range, and the
   correction is load-bearing.** `kourbatov2015upper` Theorem 3 originally rested
   on Axler's bound at "$x \ge 5.43$"; Axler's own corrigendum
   (`axler2016corrigendum`) moved that to **$x \ge 2\,634\,800\,823$**, and
   Kourbatov published a corrigendum (`kourbatov2015corrigendum`, V0) rewriting
   the proof. Any citation of "$b = 1.17 \Rightarrow$ Firoozbakht" that does not
   carry $p_k \ge 2\,634\,800\,823$ plus the unconditional check on
   $[29,\,2\,634\,800\,823]$ is citing a withdrawn proof.

6. **The verification frontier is not $4\times10^{18}$.** That is Kourbatov's
   2015 range. Visser 2019 (`visser2019verifying`, V0) verifies Firoozbakht —
   *and* the two strictly stronger Nicholson and Farhadian conjectures — for all
   $p < 2^{64} \approx 1.844\times10^{19}$. The Prime Gap List Project
   (`primegaplist_faq`, V1) reports first-occurrence analysis complete to
   $10^{20}$. `decompose.md#2.2-B3` and `#5.2` should carry $2^{64}$, with
   $4\times10^{18}$ retained as the exhaustive-sieve range.

7. **`attack.md`'s four-term expansion of $B_n$ is not in the cited literature.**
   `attack.md#2` states $B_n = L^2 - L - 1 - 3/L - 13/L^2 + O(L^{-3})$ and
   attributes the viewpoint to Kourbatov. Kourbatov's Theorem 5 (V0) proves only
   $f_k = \log^2 p_k - \log p_k - 1 + o(1)$; the $-3/L$ and $-13/L^2$ terms appear
   **nowhere** in it. They are either a new derivation of this project (then they
   are the project's own obligation to prove, tier L0, not a citation) or an
   artefact. Currently **unsourced** — flagged in §4 as gap **G1**.

---

## 2. Sources not found — recorded as absences

Recording a search that came back empty is part of the ledger.

| Sought | Why | Outcome |
|---|---|---|
| A published statement of Firoozbakht's conjecture **by Firoozbakht** | The attribution anchor | **None exists.** Visser's ref. [24] is literally *"Faride Firoozbakht, (1982), unpublished."* The earliest citable public record is Rivera's Prime Puzzles Conjecture 30 (2002); the earliest citable **print** record is Ribenboim 2004, p. 185. Cite `ribenboim2004little` for print, `rivera2002conj30` for priority-and-date. |
| Thomas R. Nicely's gap tables (`trnicely.net`, `faculty.lynchburg.edu/~nicely/`) | Canonical source of the CSG record | **Dead.** Every URL cited by Visser [29] and by the wider literature returns HTTP 404 as of 2026-07-24. Archive.org is not reachable from this leg. The CSG value is therefore carried at **V2 + C**: quoted by the Prime Gap List Project FAQ *and* independently recomputed here. Do **not** cite a live Nicely URL in the paper. |
| A proof, disproof, or accepted counterexample of Firoozbakht | The whole point | **None found.** Consistent with `attack.md#5`. The conjecture is open. |
| A source for `attack.md`'s $-3/L - 13/L^2$ expansion | Gap G1 | **None found.** See §4. |

---

## 3. The ledger

Ordered by role in the attack. `¶` marks the exact locator.

### 3.1 The conjecture itself — origin and statement

| citekey | Source | V | Locator | Exact statement supplied |
|---|---|---|---|---|
| `firoozbakht1982` | Faride Firoozbakht, 1982, **unpublished**. Univ. of Isfahan, Iran. | V1 | Visser 2019, ref. [24] ¶ *"Faride Firoozbakht, (1982), unpublished."* | **Priority and date only.** There is no primary document. Any sentence of the form "Firoozbakht proved/wrote/published" is unsupportable; "Firoozbakht proposed, in 1982" is supported. |
| `rivera2002conj30` | C. Rivera (ed.), *Conjecture 30. The Firoozbakht Conjecture*, primepuzzles.net, 2002. | V1 | Page body ¶ | "$(p_n)^{1/n}$ decreases as $n$ increases, where $p_n$ denotes the $n$th prime number." Records that Firoozbakht verified it against maximal gaps $\le 4.444\times10^{12}$, and Huxley's assessment that it is hard and previously unposted. **Earliest public record.** |
| `ribenboim2004little` | P. Ribenboim, *The Little Book of Bigger Primes*, 2nd ed., Springer, New York, 2004. | V2 | **p. 185** ¶ | The conjecture in print. Locator is Kourbatov's own: `kourbatov2015upper` §1 cites "[6, p. 185]" for the 1982 proposal. Book not obtained by this leg — hence V2. **Earliest citable print record.** |
| `visser2019verifying` | M. Visser, *Verifying the Firoozbakht, Nicholson, and Farhadian conjectures up to the 81st maximal prime gap*, **Mathematics 7 (2019), no. 8, 691**. DOI `10.3390/math7080691`. arXiv:1904.00499v2. | V0 | **Conjecture 1**, **Conjecture 2 eq. (2.1)**, **Conjecture 3 eq. (2.4)** ¶ | Three equivalent forms: $(p_{n+1}/p_n)^n \le p_n$ (2.1); $g_n \le p_n\big(p_n^{1/n}-1\big)$ (2.4), $n \ge 1$. **Use (2.4) as the canonical gap form** — it is the exact barrier, no approximation. |

### 3.2 The verification frontier

| citekey | Source | V | Locator | Exact statement supplied |
|---|---|---|---|---|
| `kourbatov2015verification` | A. Kourbatov, *Verification of the Firoozbakht conjecture for primes up to four quintillion*, **Int. Math. Forum 10 (2015), no. 6, 283–288**. arXiv:1503.01744 (v3, 2023). | V1 | Abstract ¶; restated as **Theorem 2** in `kourbatov2015upper` | "Firoozbakht's conjecture (1) is true for all primes $p_k < 4\times10^{18}$." Method: first-occurrence prime-gap table **plus known bounds for $\pi(x)$** — i.e. it is *not* a bare sieve; it inherits the explicit-bounds chain. |
| `visser2019verifying` | *(as above)* | V0 | **Abstract** ¶ and **§4** ¶ | "all three of these conjectures are unconditionally and explicitly verified for all primes below the location of the 81st maximal prime gap, certainly for all primes $p < 2^{64}$." §1 ¶ notes $p^*_{81} > 2^{64}$ was established September 2018, citing Nicely. **This is the current frontier for Firoozbakht.** |
| `primegaplist_faq` | Prime Gap List Project (Mersenne Forum community), FAQ page, © "the prime gap list community", 2026. | V1 | FAQ body ¶ | "All prime gaps in $0 < x < 10^{20}$ have now been analyzed." Also: an exhaustive search of primes to $4\cdot10^{18}$ has been carried out. Credits Nicely and Oliveira e Silva. **Community resource, not peer-reviewed — cite as computational record, never as a theorem.** |
| `nicely_gaplist` | T. R. Nicely, *First occurrence prime gaps* / *New maximal prime gaps and first occurrences*. | V3 | — | **DEAD LINK** (§2). Metadata only. Cited here because Visser [29] and the whole gap literature depend on it. Use `primegaplist_faq` instead for any live citation. |

### 3.3 The record locus and the CSG ratio — debt #1

| citekey | Source | V | Locator | Exact statement supplied |
|---|---|---|---|---|
| `kourbatov2015upper` | A. Kourbatov, *Upper bounds for prime gaps related to Firoozbakht's conjecture*, **J. Integer Seq. 18 (2015), Article 15.11.2**. arXiv:1506.03042**v4** (12 Mar 2019). | **V0 + C** | **Table 1, last row** (p. 4) ¶ | $k = 49\,749\,629\,143\,526$; $p_k = 1\,693\,182\,318\,746\,371$; $p_{k+1}-p_k = 1132$; $f_k = p_k^{1+1/k}-p_k = \mathbf{1193.418}$; $\ell_k = \log^2p_k-\log p_k = \mathbf{1194.516}$. Table caption: "$p_k \in$ A111943". Recomputed §5: $f_k = 1193.41777829\ldots$, $\ell_k = 1194.51592191\ldots$ ✓ |
| `primegaplist_faq` | *(as above)* | **V1 + C** | FAQ ¶ | "For the 1132 gap, the ratio is 0.9206, the largest value observed for any $p_1 > 7$ thus far." Recomputed §5: $R_{\mathrm{csg}} = g/\log^2 p = 0.9206385885574205$. ✓ **Note the caveat $p_1 > 7$** — it is real, not decorative. Recomputed §5: $R > 1$ at exactly three points, $p = 2$ ($R = 2.0814$), $p = 3$ ($R = 1.6571$) and $p = 7$ ($R = 1.0564$). Any statement "the CSG ratio has never exceeded 1" is **false as written** and must carry the restriction. |
| `oeis_A111943` | OEIS A111943 (Sloane, ed.). | V2 | Sequence entry ¶ | The maximal-gap primes indexed by Kourbatov's Table 1. Cited *by* Kourbatov's caption (V0 on the citation, V2 on the content — OEIS returned 403 to this leg). |
| `oeis_A002386`, `oeis_A005250` | OEIS A002386 (primes preceding maximal gaps), A005250 (maximal gaps). | V3 | — | Standard indices for the maximal-gap table. Metadata only (403). |

**Debt #1 verdict: CLOSED.** Both the ratio $0.9206386$ and the locus
$(p, g) = (1\,693\,182\,318\,746\,371,\ 1132)$ are confirmed by two mutually
independent routes — a refereed table (`kourbatov2015upper`) and a community
record (`primegaplist_faq`) — and recomputed here. Carry the $p_1 > 7$ caveat.

### 3.4 The Firoozbakht ⟷ prime-gap dictionary — debt #2

All four rows are **V0**, read from arXiv:1506.03042**v4** (the corrected text).

| citekey | Locator | Exact statement supplied |
|---|---|---|
| `kourbatov2015upper` | **Theorem 1** (§2) ¶ | *"If conjecture (1) is true, then $p_{k+1}-p_k < \log^2 p_k - \log p_k - 1$ for all $k > 9$."* **Necessary consequence.** Proof runs through Axler Cor. 3.6 at $x \ge 1\,772\,201$ ($k \ge 133\,115$); $9 < k < 133\,115$ by direct computation. |
| `kourbatov2015upper` | **Theorem 3** (§4) ¶ *(as corrected)* | *"If $p_{k+1}-p_k < \log^2 p_k - \log p_k - 1.17$ for all $k>9$ ($p_k \ge 29$), then Firoozbakht's conjecture (1) is true."* **Sufficient condition.** Proof valid for $p_k \ge 2\,634\,800\,823$ via Axler Cor. 3.5; on $p_k \in [29,\,2\,634\,800\,823]$ both (1) and the hypothesis hold unconditionally. **Never cite without that range split** — see `kourbatov2015corrigendum`. |
| `kourbatov2015upper` | **Theorem 4** (§4) ¶ | Three further sufficient conditions, each of the form $g_k < \log^2p_k - \log p_k - 1 - c_1/\log p_k - c_2/\log^2 p_k - \dots$ with $(c_i)$ from Axler's family: $(3.83)$; $(3.35, 15.43)$; $(3.35, 12.65, 89.6)$ — each **for all $p_k > 4\times10^{18}$**, the sub-frontier part being covered by `kourbatov2015verification`. This is the *"$b \to 1$ as $k\to\infty$"* family. |
| `kourbatov2015upper` | **Theorem 5** (§5, Appendix) ¶ | *"$f_k = \log^2 p_k - \log p_k - 1 + o(1)$ as $k \to \infty$"*, where $f_k = p_k^{1+1/k}-p_k$. Proof brackets $f_k$ between $\log^2p_k-\log p_k-1-3.83/\log p_k$ and $\log^2p_k-\log p_k-1$. **This is the whole published asymptotic** — see §4/G1. |
| `kourbatov2015upper` | **§3** ¶ | The direction warning `decompose.md#2.3-C4` and `outcomes.md#A5` both need: *"given a pair of primes $p_k, p_{k+1}$, the validity of (2) alone is not enough for the verification of (1)"*, with the explicit witness $p_k = 2\,010\,733$, $q = 2\,010\,929$ — a prime sitting inside $[p_k+f_k,\ p_k+\ell_k]$. Also: $f_k < \ell_k$ holds for $p_k \ge 11783$ ($k \ge 1412$), **not for smaller $k$**. |
| `kourbatov2015corrigendum` | A. Kourbatov, *Corrigendum: Upper Bounds for Prime Gaps Related to Firoozbakht's Conjecture*, J. Integer Seq. 18. | **V0** | Full text ¶ | Corrects the proof of Theorem 3: replace "$x \ge 5.43$" with "$x \ge 2\,634\,800\,823$" in (11); remove "Let $k>9$"; remove "for $p_k \ge 29$" from (12),(13); rewrite the closing two sentences. "These changes have been incorporated in arXiv:1506.03042v4." Cause: Axler's own corrigendum. |

**Debt #2 verdict: CLOSED, with a correction.** The verification range
($4\times10^{18}$, superseded by $2^{64}$) and both directions of the
$\log^2p-\log p-b$ dictionary are pinned to numbered theorems. The
$b=1.17$ range restriction was **not** in `decompose.md` and must be added.

### 3.5 Explicit $\pi(x)$ and $p_n$ bounds — debt #4

| citekey | Source | V | Locator | Exact statement supplied |
|---|---|---|---|---|
| `axler2014newbounds` | C. Axler, *New bounds for the prime counting function $\pi(x)$*, arXiv:1409.1780**v3** (17 Mar 2015); published as *New bounds for the prime counting function*, **Integers 16 (2016), A22**. | **V0** | **Corollary 3.5** ¶ | Four upper bounds. In Kourbatov's chain: $\pi(x) < x/(\log x - 1 - 1.17/\log x)$ — v3 states "for $x \ge 5.43$", **corrected to $x \ge 2\,634\,800\,823$**. Also $\pi(x) < x/(\log x-1-1/\log x-3.83/\log^2x)$ for $x\ge9.25$; and the 3- and 4-term variants with $(3.35,15.43)$ for $x\ge14.36$ and $(3.35,12.65,89.6)$ for $x\ge21.95$. |
| `axler2014newbounds` | *(as above)* | **V0** | **Corollary 3.6**, table row $(a,b,c,d;x_0) = (1,0,0,0;\,1\,772\,201)$ ¶ | $\pi(x) > x/\big(\log x - 1 - 1/\log x - 1/\log^2 x\big)$ for $x \ge 1\,772\,201$. **This is Kourbatov's eq. (5)** and the load-bearing lower bound in Theorems 1 and 5. |
| `axler2016corrigendum` | C. Axler, *Corrigendum to "New bounds for the prime counting function"*, **Integers 16 (2016), A22**, 15 pp. | V2 | Cited by `kourbatov2015corrigendum` ref. [A] ¶ | Source of the $5.43 \to 2\,634\,800\,823$ range correction. V2: not obtained directly; its content is fully determined by the Kourbatov corrigendum (V0). |
| `dusart2010estimates` | P. Dusart, *Estimates of some functions over primes without R.H.*, arXiv:1002.0442v1 (2 Feb 2010). | **V0 + C** | **Proposition 6.6** ¶ | $p_k \le k\left(\ln k + \ln\ln k - 1 + \dfrac{\ln\ln k - 2}{\ln k}\right)$ for $k \ge 688\,383$. |
| `dusart2010estimates` | *(as above)* | **V0 + C** | **Proposition 6.7** ¶ | $p_k \ge k\left(\ln k + \ln\ln k - 1 + \dfrac{\ln\ln k - 2.1}{\ln k}\right)$ for $k \ge 3$. |
| `dusart2010estimates` | *(as above)* | V0 | **Proposition 6.8** ¶ | For all $x \ge 396\,738$ there is a prime in $\big(x,\ x(1+1/(25\ln^2x))\big]$. Improves Schoenfeld's $x/16597$ for $x \ge e^{25.77}$. **Not sufficient for Firoozbakht** ($x/\ln^2x \ggg \ln^2 x$) — keep for `decompose.md#2.4`'s barrier catalogue. |
| `dusart2010estimates` | *(as above)* | V0 | **Proposition 6.3** ¶ | $\vartheta(p_k) \le k\big(\ln k + \ln\ln k - 1 + (\ln\ln k - 2)/\ln k - 0.782/\ln^2k\big)$, $k \ge 781$. The Chebyshev-side companion. |
| `dusart2018explicit` | P. Dusart, *Explicit estimates of some functions over primes*, **Ramanujan J. 45 (2018), 227–251**. DOI `10.1007/s11139-016-9839-4`. | V3 | — | Successor to `dusart2010estimates` with sharper constants. **Metadata only** — the PDF host presented an expired certificate and the publisher record is paywalled. Do **not** quote a proposition number from this paper without obtaining it. Use `dusart2010estimates` (V0) instead. |
| `rossersch1962` | J. B. Rosser & L. Schoenfeld, *Approximate formulas for some functions of prime numbers*, **Illinois J. Math. 6 (1962), 64–94**. DOI `10.1215/ijm/1255631807`. | V3 | — | **Metadata only.** The historical origin of explicit-bounds methodology and the ancestor of every row above. **It is not in Kourbatov's chain** — see §1 finding 4. Cite for lineage, never for a constant, unless promoted to V0. |
| `panaitopol2000` | L. Panaitopol, *A formula for $\pi(x)$ applied to a result of Koninck–Ivić*, **Nieuw Arch. Wiskd. (5) 1 (2000), 55–56**. | V2 | Cited as `kourbatov2015upper` ref. [5] ¶ | The $\pi(x)$ formula underlying Axler's Corollary 3.5 family; its coefficients approximate OEIS A233824. Cite only as the ancestor of Axler's bounds. |

**Debt #4 verdict: CLOSED, with the attribution corrected.** The chain actually
used is **Axler**; the three-term $p_n$ bracket A3 asked for is **Dusart 2010
Props. 6.6/6.7**, validity $k \ge 688\,383$. See §5 for what it certifies —
and §1 finding 3 for what it does *not*.

### 3.6 Cramér, the barrier, and the heuristics — debts #3 and #5

| citekey | Source | V | Locator | Exact statement supplied |
|---|---|---|---|---|
| `cramer1936order` | H. Cramér, *On the order of magnitude of the difference between consecutive prime numbers*, **Acta Arith. 2 (1936), 23–46**. | V2 | Quoted at length in `granville1995harald`, block quotation immediately preceding eq. (14) ¶ | Cramér's own words: *"With a probability $=1$, the relation $\limsup_{n\to\infty}\dfrac{P_{n+1}-P_n}{(\log P_n)^2}=1$ is satisfied"* — followed by *"we may take this as a suggestion that, for the particular sequence of ordinary prime numbers $p_n$, some similar relation may hold."* **Cramér stated it about his random model and only *suggested* the transfer.** Any paper writing "Cramér conjectured" should carry that hedge. |
| `granville1995harald` | A. Granville, *Harald Cramér and the distribution of prime numbers*, **Scand. Actuar. J. 1995, no. 1, 12–28**. DOI `10.1080/03461238.1995.10413946`. | **V0** | **eq. (14)** ¶ | Cramér's conjecture as normally stated: $\max_{p_n\le x}(p_{n+1}-p_n) \sim \log^2 x$. |
| `granville1995harald` | *(as above)* | **V0** | unnumbered display immediately following eq. (20), in the *“Cramér’s model revisited”* discussion ¶ | $\displaystyle\max_{p_n\le x}(p_{n+1}-p_n) \gtrsim 2e^{-\gamma}\log^2 x$, *"which contradicts Cramér's conjecture (14)!"* The constant is given earlier in the paper ¶, in the Mertens/sieve discussion, as $2e^{-\gamma} \approx 1.12292\ldots$. **This is debt #3's statement.** It is a **heuristic consequence of Granville's revised Cramér model**, derived from Maier's theorem — it is *not* a theorem, and Granville does not assert it as one. |
| `maier1985primes` | H. Maier, *Primes in short intervals*, **Michigan Math. J. 32 (1985), 221–225**. | V2 | Cited as `granville1995harald` ref. [11] ¶ | The theorem that breaks Cramér's model and licenses the $2e^{-\gamma}$ revision. Content not read by this leg. |
| `cramer1920some` | H. Cramér, *Some theorems concerning prime numbers*, **Arkiv för Mat. Astr. o. Fys. 15, no. 5 (1920), 1–32**. | V2 | Bibliographic form from `granville1995harald` ref. [5] ¶; the RH gap bound from secondary sources | On RH, $p_{n+1}-p_n = O(\sqrt{p_n}\log p_n)$. **The statement is V2, the bibliographic form is V1.** Note `decompose.md#3` writes "Cramér 1920/21" — the correct single citation is 1920, and there is a companion, *On the distribution of primes*, Proc. Camb. Phil. Soc. **20** (1920), 272–280 (`granville1995harald` ref. [4]). Disambiguate before the paper ships. |
| `bhp2001difference` | R. C. Baker, G. Harman, J. Pintz, *The difference between consecutive primes, II*, **Proc. London Math. Soc. (3) 83 (2001), no. 3, 532–562**. | V2 | Main theorem ¶ | For all sufficiently large $x$, the interval $[x - x^{0.525},\, x]$ contains primes; hence $p_{n+1}-p_n \ll p_n^{0.525}$. **Debt #5's exponent confirmed at V2.** Note $x_0$ is *ineffective as stated* — the literature says it "could be determined with enough effort". A paper claiming an explicit unconditional bound must not lean on 0.525 without that caveat. |
| `sun2013sequence` | Z.-W. Sun, *On a sequence involving sums of primes*, **Bull. Aust. Math. Soc. 88 (2013), 197–205**. arXiv:1207.7059. | V1 | Abstract/intro ¶; the Firoozbakht link via `kourbatov2015upper` §1 ¶ (V0) | Firoozbakht implies $p_{n+1}-p_n < \log^2p_n - \log p_n + 1$ for large $n$ — the **weaker** variant with $+1$. Kourbatov §1: *"(Sun [10, 11] gives a variant of (2) with a larger right-hand side, $\log^2p_k-\log p_k+1$.)"* Sun's own theorem is about $\sqrt[n]{S_n/n}$, $S_n = \sum_{i\le n}p_i$ — **not** about Firoozbakht. Do not cite Sun as a source for a Firoozbakht consequence without that distinction. |

### 3.7 Large gaps — the refutation-side barrier, debt #5b

| citekey | Source | V | Locator | Exact statement supplied |
|---|---|---|---|---|
| `rankin1938difference` | R. A. Rankin, *The difference between consecutive prime numbers*, **J. London Math. Soc. s1-13 (1938), 242–247**. DOI `10.1112/jlms/s1-13.4.242`. | V3 | — | Metadata only. The Erdős–Rankin construction. |
| `granville1995harald` | *(as above)* | **V0** | unnumbered display following the Westzynthius remark ($\limsup (p_{n+1}-p_n)/\log p_n = \infty$, 1931) ¶ | The Erdős–Rankin form as of 1995: there are infinitely many $p_n$ with $p_{n+1}-p_n > \dfrac{c\,(\log p_n)(\log\log p_n)(\log\log\log\log p_n)}{(\log\log\log p_n)^2}$ for a fixed $c>0$. |
| `fgkt2016large` | K. Ford, B. Green, S. Konyagin, T. Tao, *Large gaps between consecutive prime numbers*, **Ann. of Math. (2) 183 (2016), no. 3, 935–974**. arXiv:1408.4505. | V1 | Abstract ¶ | $G(X) \ge f(X)\dfrac{\log X\,\log\log X\,\log\log\log\log X}{(\log\log\log X)^2}$ with $f(X)\to\infty$; answers a question of Erdős. |
| `fgkmt2018long` | K. Ford, B. Green, S. Konyagin, J. Maynard, T. Tao, *Long gaps between primes*, **J. Amer. Math. Soc. 31 (2018), no. 1, 65–105**. arXiv:1412.5029v3. | V1 | Abstract ¶ | $\displaystyle\max_{p_{n+1}\le X}(p_{n+1}-p_n) \gg \frac{\log X\,\log\log X\,\log\log\log\log X}{\log\log\log X}$. **Note the exponent: $(\log\log\log X)^{1}$, not squared** — one iterated-log better than `fgkt2016large`. Getting this exponent wrong is the easiest citation error in this family. |

**Debt #5b verdict: CLOSED at V1.** The best known lower bound is
`fgkmt2018long`. It is $\ll \log^2 X$ — **too small by a factor $\sim\log X/\log\log X$
to touch Firoozbakht**, which is the point `decompose.md#2.5-E5` needs.

### 3.8 The Nicholson / Farhadian chain — debt #6

| citekey | Source | V | Locator | Exact statement supplied |
|---|---|---|---|---|
| `visser2019verifying` | *(as above)* | **V0** | **Conjecture 3, eqs. (2.4)–(2.6)** ¶ | Firoozbakht $g_n \le p_n(p_n^{1/n}-1)$, $n\ge1$; **Nicholson** $g_n \le p_n\big((n\ln n)^{1/n}-1\big)$, $n>4$; **Farhadian** $g_n \le p_n\big(\big(p_n\frac{\ln n}{\ln p_n}\big)^{1/n}-1\big)$, $n>4$. |
| `visser2019verifying` | *(as above)* | **V0** | **§2**, sentence after eq. (2.3) ¶ | *"the standard inequalities $n\ln n < p_n < n\ln p_n$ show that Farhadian $\Longrightarrow$ Nicholson $\Longrightarrow$ Firoozbakht."* **The chain direction, sourced.** |
| `visser2019verifying` | *(as above)* | **V0** | **Sufficient condition 2**, eq. (3.2) ¶ | $g_n < f(n) = \big(\ln(n\ln n)-1\big)\ln(n\ln n)$ suffices for Nicholson (hence Firoozbakht), $n>4$, $n\ge2$; derived from (3.1) plus Dusart's $p_n > n(\ln(n\ln n)-1)$, $n\ge2$. **A second, independent sufficient criterion** — cheaper than Kourbatov's and *index-based*, so it needs no $\pi(x)$ inversion. Recommend `lean-skeleton` evaluate it against `outcomes.md#A6`'s OB-F6b. |
| `nicholson2013` | J. Nicholson, 2013, **unpublished**. | V1 | `visser2019verifying` ref. [33] ¶ | Attribution only; recorded via OEIS A182514. No primary document — same status as `firoozbakht1982`. |
| `farhadian_conj` | R. Farhadian, *A new conjecture on the primes*, primepuzzles.net (PDF). | V1 | `visser2019verifying` ref. [34] ¶ | Attribution only. |
| `farhadian_jakimczuk2017` | R. Farhadian, R. Jakimczuk, *On a new conjecture of prime numbers*, **Int. Math. Forum 12 (2017), no. 12, 559–564**. | V1 | `visser2019verifying` ref. [35] ¶ | The refereed statement of the Farhadian conjecture. |
| `oeis_A182514` | OEIS A182514. | V3 | — | The Nicholson-conjecture sequence. Metadata only (403). |

**Debt #6 verdict: CLOSED as "confirm", not "kill".** `decompose.md#7` rated it
Low/unused and `#5.3` excluded the chain as [L3]. That exclusion is **wrong on the
facts**: the chain is refereed (`visser2019verifying`, V0), the implication
direction is proved there, and Visser's Sufficient condition 2 is directly usable.
Recommend re-rating debt #6 from *unused* to *live*, and moving the chain out of
[L3] into the ledger-backed tier. `notebooks` and `lean-skeleton` should both see it.

---

## 4. Gaps — claims with no source

Flagged, not fixed. Each is either an obligation this project must discharge
itself, or a claim to withdraw.

| id | Claim | Where | Status |
|---|---|---|---|
| **G1** | $B_n = L^2 - L - 1 - 3/L - 13/L^2 + O(L^{-3})$, and the $\pi(x)$ series inversion $p_n/n = L - 1 - 1/L - 3/L^2 - 13/L^3 + O(L^{-4})$ | `attack.md#2` eqs. (2),(3) | **UNSOURCED.** Kourbatov Thm 5 (V0) proves only the $+o(1)$ form. The coefficients $1,3,13$ are the Cipolla / A233824 family and are *plausible*, but no source in this ledger states them. **Either derive them in-project (tier L0, with proof) or delete the terms.** `attack.md` currently attributes the viewpoint to Kourbatov — that attribution is not supported at this precision. |
| **G2** | "The community's heuristic expectation is that F is false" | `decompose.md#8`-6 | **PARTIALLY SOURCED.** What is sourced: Granville's $2e^{-\gamma}$ heuristic contradicts Cramér's constant-1 conjecture (V0, §3.6), and Wikipedia reports the conjecture "is inconsistent with the heuristics of Granville and Pintz and of Maier". What is **not** sourced: any survey of what the community expects. Restate as the specific heuristic, or drop. This is `outcomes.md#A9`'s sociology-vs-mathematics point, and the ledger cannot close it. |
| **G3** | Explicit $x_0$ for Baker–Harman–Pintz | `decompose.md#3.4`, `#2.4` | **NOT AVAILABLE.** The 0.525 result's threshold is ineffective as published. Any explicit unconditional statement must say so. |
| **G4** | Rosser–Schoenfeld and Dusart 2018 locators | `decompose.md#2.3-C4` | **V3 — do not use.** Neither source was obtained. Substitute `axler2014newbounds` (V0) and `dusart2010estimates` (V0). |
| **G5** | A live URL for the Nicely gap tables | everywhere the CSG record is cited | **DEAD** (§2). Substitute `primegaplist_faq` + the recomputation in §5. |
| **G6** | "$R_{\mathrm{csg}}$ has never exceeded 1" | implied by `decompose.md#3.2`/`#5.2` framing | **FALSE without the restriction $p_1 > 7$** — see §3.3. $R > 1$ at $p = 2, 3, 7$ (values $2.0814$, $1.6571$, $1.0564$); recomputed §5. |

---

## 5. Independent recomputation

`verify_ledger.py` (this directory), 60-digit `mpmath`, output in
`verify_ledger.out`. Exact-integer inputs only; no floating-point slack argument
is relied on. This is a **[C]** cross-check of ledger rows, not evidence about
the conjecture.

At the record locus $k = 49\,749\,629\,143\,526$, $p_k = 1\,693\,182\,318\,746\,371$, $g_k = 1132$:

| quantity | recomputed | source value | source |
|---|---|---|---|
| $\log p_k$ | $35.0653861820133348\ldots$ | — | — |
| $f_k = p_k^{1+1/k}-p_k$ | $\mathbf{1193.41777829404}\ldots$ | $1193.418$ | `kourbatov2015upper` Table 1 ✓ |
| $\ell_k = \log^2p_k-\log p_k$ | $\mathbf{1194.51592191172}\ldots$ | $1194.516$ | `kourbatov2015upper` Table 1 ✓ |
| $R_{\mathrm{csg}} = g_k/\log^2p_k$ | $\mathbf{0.9206385885574205}\ldots$ | $0.9206$ / $0.92063858855742$ | `primegaplist_faq` ✓ |
| **shortfall $f_k/g_k$** | $\mathbf{1.054255987892}\ldots$ | — | **replaces `decompose.md#3.2`'s $1.055$** |
| $T_k = (p_k/k)\log p_k$ | $1193.41777829362\ldots$ | — | agrees with $f_k$ to **12 significant figures** |
| $p_k/k$ | $34.0340691558041\ldots$ | — | — |
| $c_k$ (`decompose.md#2.3-C4` def.: $p_k/k = \log p_k - c_k$) | $\mathbf{1.0313170262}\ldots$ | — | — |

Dusart 2010 Props. 6.6/6.7 evaluated at $k$ (validity $k \ge 688\,383$ ✓):

- bracket holds: $1\,693\,082\,432\,978\,151.87 \le p_k \le 1\,693\,240\,177\,892\,500.92$ ✓
- $\Rightarrow c_k \in (\mathbf{1.030154},\ \mathbf{1.033325})$ — **confirms `outcomes.md#A3`'s $(1.030, 1.034)$**
- the **two-term** bracket `decompose.md#2.3-C4` cites gives only $c_k \in (0.076168,\ 1.076168)$ — confirming A3's charge that C4's stated $(0.9, 1.2)$ is not derivable from its own citation
- $\Rightarrow T_k/g_k \in (1.0541938,\ 1.0542920)$, i.e. **shortfall $= 1.05424 \pm 0.00005$**

> **Correction made during this leg's own verify step.** An earlier draft of §1
> reported $c_k \in (1.0574, 1.1685)$ and declared A3 wrong. That used the
> normalization $p_n/n = \log p_n - 1 - c_n/\log p_n$, which is **not** C4's.
> Under C4's definition A3 is correct. The shortfall interval was unaffected.

Dead band $B_n = \big(T_n,\ T_n/(1-T_n/p_n)\big)$ at the record locus: width
$W = T^2/(p-T) = 8.41\times10^{-10}$ — **no integer can lie in it**, confirming
`outcomes.md#A4` at this locus.

Exact small-$n$ audit ($n = 1..11$, exact integers): **Firoozbakht holds at every
$n$** (as expected — it is verified to $2^{64}$), while the (SUF) criterion
$g_n \le T_n$ **fails to fire at $n = 2$ and $n = 4$**. This independently
reproduces the panel's strongest finding (`outcomes.md#A4`) and is consistent with
Kourbatov's own $f_k < \ell_k$ threshold at $p_k \ge 11783$ (§3.4). **The criterion
is silent exactly where the conjecture is tightest.**

CSG ratio at small $n$ (source of caveat G6): $R_{\mathrm{csg}} > 1$ at exactly
three points, $p = 2$ ($2.081369$), $p = 3$ ($1.657071$), $p = 7$ ($1.056366$);
$R \le 1$ for every other $p \le 113$.

**Reproducibility.** `python3 verify_ledger.py` — exit status **0**, output byte-identical
across reruns (no RNG, no floating-point path taken: `mpmath` at 60 decimal digits
from exact integer inputs).

---

## 6. Retrieval record

Primary sources obtained by this leg and read verbatim (V0). Retrieved
2026-07-24; the fetched bytes are in this session's tool-results cache.

| citekey | Retrieved from | Form |
|---|---|---|
| `kourbatov2015upper` | `arxiv.org/pdf/1506.03042` (v4, 12 Mar 2019) | PDF → text, full |
| `kourbatov2015corrigendum` | `cs.uwaterloo.ca/journals/JIS/VOL18/Kourbatov/k3c.pdf` | PDF → text, full |
| `axler2014newbounds` | `arxiv.org/pdf/1409.1780v3` | PDF → text, §3.1–3.2 |
| `dusart2010estimates` | `arxiv.org/pdf/1002.0442` (v1) | PDF → text, §6 |
| `granville1995harald` | `chance.dartmouth.edu/.../Riemann/cramer.pdf` | PDF → text, full |
| `visser2019verifying` | `arxiv.org/pdf/1904.00499` (v2) | PDF → text, full |
| `kourbatov2015verification` | `arxiv.org/abs/1503.01744` | abstract page (V1) |
| `fgkmt2018long`, `fgkt2016large`, `sun2013sequence` | arXiv abstract pages | V1 |
| `primegaplist_faq` | `primegap-list-project.github.io/faq/` | HTML |

**Not retrieved** (V2/V3 rows): Ribenboim 2004; Rosser–Schoenfeld 1962;
Dusart 2018; Cramér 1920, 1936; Baker–Harman–Pintz 2001; Maier 1985; Rankin 1938;
Axler 2016 corrigendum; Panaitopol 2000; all OEIS entries (HTTP 403);
all Nicely pages (HTTP 404).

---

## 7. BibTeX

```bibtex
@article{kourbatov2015verification,
  author  = {Kourbatov, Alexei},
  title   = {Verification of the {Firoozbakht} conjecture for primes up to four quintillion},
  journal = {International Mathematical Forum},
  volume  = {10}, number = {6}, pages = {283--288}, year = {2015},
  eprint  = {1503.01744}, archivePrefix = {arXiv}, primaryClass = {math.NT}
}
@article{kourbatov2015upper,
  author  = {Kourbatov, Alexei},
  title   = {Upper Bounds for Prime Gaps Related to {Firoozbakht}'s Conjecture},
  journal = {Journal of Integer Sequences},
  volume  = {18}, pages = {Article 15.11.2}, year = {2015},
  eprint  = {1506.03042}, archivePrefix = {arXiv}, primaryClass = {math.NT},
  note    = {Cite arXiv v4 (12 Mar 2019), which incorporates the corrigendum}
}
@misc{kourbatov2015corrigendum,
  author  = {Kourbatov, Alexei},
  title   = {Corrigendum: Upper Bounds for Prime Gaps Related to {Firoozbakht}'s Conjecture},
  howpublished = {Journal of Integer Sequences, vol. 18},
  url     = {https://cs.uwaterloo.ca/journals/JIS/VOL18/Kourbatov/k3c.pdf}
}
@article{visser2019verifying,
  author  = {Visser, Matt},
  title   = {Verifying the {Firoozbakht}, {Nicholson}, and {Farhadian} Conjectures
             up to the 81st Maximal Prime Gap},
  journal = {Mathematics}, volume = {7}, number = {8}, pages = {691}, year = {2019},
  doi     = {10.3390/math7080691}, eprint = {1904.00499}, archivePrefix = {arXiv}
}
@article{axler2014newbounds,
  author  = {Axler, Christian},
  title   = {New bounds for the prime counting function},
  journal = {Integers}, volume = {16}, pages = {A22}, year = {2016},
  eprint  = {1409.1780}, archivePrefix = {arXiv}, primaryClass = {math.NT},
  note    = {Corollaries 3.5, 3.6 read from arXiv v3; see corrigendum}
}
@article{axler2016corrigendum,
  author  = {Axler, Christian},
  title   = {Corrigendum to ``New bounds for the prime counting function''},
  journal = {Integers}, volume = {16}, pages = {A22}, year = {2016}, pagetotal = {15}
}
@misc{dusart2010estimates,
  author = {Dusart, Pierre},
  title  = {Estimates of Some Functions Over Primes without {R.H.}},
  year   = {2010}, eprint = {1002.0442}, archivePrefix = {arXiv}, primaryClass = {math.NT}
}
@article{dusart2018explicit,
  author  = {Dusart, Pierre},
  title   = {Explicit estimates of some functions over primes},
  journal = {The Ramanujan Journal}, volume = {45}, pages = {227--251}, year = {2018},
  doi     = {10.1007/s11139-016-9839-4}, note = {NOT VERIFIED -- metadata only}
}
@article{rossersch1962,
  author  = {Rosser, J. Barkley and Schoenfeld, Lowell},
  title   = {Approximate formulas for some functions of prime numbers},
  journal = {Illinois Journal of Mathematics}, volume = {6}, pages = {64--94}, year = {1962},
  doi     = {10.1215/ijm/1255631807}, note = {NOT VERIFIED -- metadata only}
}
@article{granville1995harald,
  author  = {Granville, Andrew},
  title   = {Harald {Cram\'er} and the distribution of prime numbers},
  journal = {Scandinavian Actuarial Journal}, volume = {1995}, number = {1},
  pages   = {12--28}, year = {1995}, doi = {10.1080/03461238.1995.10413946}
}
@article{cramer1936order,
  author  = {Cram\'er, Harald},
  title   = {On the order of magnitude of the difference between consecutive prime numbers},
  journal = {Acta Arithmetica}, volume = {2}, pages = {23--46}, year = {1936}
}
@article{cramer1920some,
  author  = {Cram\'er, Harald},
  title   = {Some theorems concerning prime numbers},
  journal = {Arkiv f\"or Matematik, Astronomi och Fysik},
  volume  = {15}, number = {5}, pages = {1--32}, year = {1920}
}
@article{bhp2001difference,
  author  = {Baker, R. C. and Harman, G. and Pintz, J.},
  title   = {The difference between consecutive primes, {II}},
  journal = {Proceedings of the London Mathematical Society (3)},
  volume  = {83}, number = {3}, pages = {532--562}, year = {2001}
}
@article{fgkt2016large,
  author  = {Ford, Kevin and Green, Ben and Konyagin, Sergei and Tao, Terence},
  title   = {Large gaps between consecutive prime numbers},
  journal = {Annals of Mathematics (2)}, volume = {183}, number = {3},
  pages   = {935--974}, year = {2016}, eprint = {1408.4505}, archivePrefix = {arXiv}
}
@article{fgkmt2018long,
  author  = {Ford, Kevin and Green, Ben and Konyagin, Sergei and Maynard, James
             and Tao, Terence},
  title   = {Long gaps between primes},
  journal = {Journal of the American Mathematical Society},
  volume  = {31}, number = {1}, pages = {65--105}, year = {2018},
  eprint  = {1412.5029}, archivePrefix = {arXiv}
}
@article{rankin1938difference,
  author  = {Rankin, R. A.},
  title   = {The difference between consecutive prime numbers},
  journal = {Journal of the London Mathematical Society}, volume = {s1-13},
  number  = {4}, pages = {242--247}, year = {1938}, doi = {10.1112/jlms/s1-13.4.242},
  note    = {NOT VERIFIED -- metadata only}
}
@article{maier1985primes,
  author = {Maier, Helmut}, title = {Primes in short intervals},
  journal = {Michigan Mathematical Journal}, volume = {32}, pages = {221--225}, year = {1985}
}
@article{sun2013sequence,
  author  = {Sun, Zhi-Wei},
  title   = {On a sequence involving sums of primes},
  journal = {Bulletin of the Australian Mathematical Society},
  volume  = {88}, pages = {197--205}, year = {2013},
  eprint  = {1207.7059}, archivePrefix = {arXiv}
}
@book{ribenboim2004little,
  author = {Ribenboim, Paulo}, title = {The Little Book of Bigger Primes},
  edition = {2nd}, publisher = {Springer}, address = {New York}, year = {2004},
  note = {Firoozbakht's conjecture at p. 185}
}
@misc{firoozbakht1982,
  author = {Firoozbakht, Faride}, title = {Conjecture on the sequence $p_n^{1/n}$},
  year = {1982}, note = {Unpublished; no primary document exists}
}
@misc{rivera2002conj30,
  author = {Rivera, Carlos}, title = {Conjecture 30. The {Firoozbakht} Conjecture},
  year = {2002}, howpublished = {The Prime Puzzles and Problems Connection},
  url = {https://www.primepuzzles.net/conjectures/conj_030.htm}
}
@misc{nicholson2013,
  author = {Nicholson, John}, year = {2013},
  note = {Unpublished; recorded as OEIS A182514}
}
@misc{farhadian_conj,
  author = {Farhadian, Reza}, title = {A new conjecture on the primes},
  howpublished = {The Prime Puzzles and Problems Connection}
}
@article{farhadian_jakimczuk2017,
  author  = {Farhadian, Reza and Jakimczuk, Rafael},
  title   = {On a new conjecture of prime numbers},
  journal = {International Mathematical Forum}, volume = {12}, number = {12},
  pages   = {559--564}, year = {2017}
}
@article{panaitopol2000,
  author  = {Panaitopol, Laurentiu},
  title   = {A formula for $\pi(x)$ applied to a result of {Koninck}--{Ivi\'c}},
  journal = {Nieuw Archief voor Wiskunde (5)}, volume = {1}, pages = {55--56}, year = {2000}
}
@misc{primegaplist_faq,
  author = {{The prime gap list community}}, title = {{FAQ} -- Prime gap list project},
  url = {https://primegap-list-project.github.io/faq/}, note = {Accessed 2026-07-24}
}
@misc{nicely_gaplist,
  author = {Nicely, Thomas R.}, title = {First occurrence prime gaps},
  note = {DEAD LINK as of 2026-07-24; formerly trnicely.net/gaps/gaplist.html}
}
@misc{oeis_A002386,
  author = {{OEIS Foundation Inc.}}, title = {Sequence {A002386}: primes preceding maximal prime gaps},
  howpublished = {The On-Line Encyclopedia of Integer Sequences},
  url = {https://oeis.org/A002386}, note = {NOT VERIFIED -- HTTP 403 on 2026-07-24}
}
@misc{oeis_A005250,
  author = {{OEIS Foundation Inc.}}, title = {Sequence {A005250}: maximal prime gaps},
  howpublished = {The On-Line Encyclopedia of Integer Sequences},
  url = {https://oeis.org/A005250}, note = {NOT VERIFIED -- HTTP 403 on 2026-07-24}
}
@misc{oeis_A111943,
  author = {{OEIS Foundation Inc.}}, title = {Sequence {A111943}},
  howpublished = {The On-Line Encyclopedia of Integer Sequences},
  url = {https://oeis.org/A111943},
  note = {Cited by Kourbatov 2015b Table 1 caption; content NOT VERIFIED -- HTTP 403 on 2026-07-24}
}
@misc{oeis_A182514,
  author = {{OEIS Foundation Inc.}}, title = {Sequence {A182514}},
  howpublished = {The On-Line Encyclopedia of Integer Sequences},
  url = {https://oeis.org/A182514},
  note = {Record of the Nicholson conjecture; NOT VERIFIED -- HTTP 403 on 2026-07-24}
}
```

**Citekey count: 31.** Every citekey appearing in §3 resolves to an entry above.

---

## 8. Handoff

**To `citation-gate`.** Every paper claim must name a citekey **and** a §3
locator. Fail any claim resting on a **V3** row (`dusart2018explicit`,
`rossersch1962`, `rankin1938difference`, `nicely_gaplist`, all OEIS rows) or on
gaps **G1–G6**. Check specifically: the $b=1.17$ range split; the $2^{64}$ vs
$4\times10^{18}$ distinction; the $(\log\log\log X)^1$ exponent in `fgkmt2018long`.

**To `write-paper`.** Use shortfall $\mathbf{1.05426}$ (§5), sourced directly to
`kourbatov2015upper` Table 1's published $f_k = 1193.418$ — **not** $1.055$, and
**not** via any $c_n$ interval, which the exact barrier makes unnecessary. If an
explicit-bounds route is wanted anyway, it certifies $1.05424 \pm 0.00005$
(Dusart 2010 Props. 6.6/6.7, validity $k \ge 688\,383$) — consistent, and the
exact value lies inside it.
Attribute explicit bounds to Axler, not Rosser–Schoenfeld. State the verification
frontier as $2^{64}$ (Visser), citing $4\times10^{18}$ (Kourbatov) as the
exhaustive-sieve range. Carry the $p_1 > 7$ caveat on the CSG record.

**To `decompose` (amendment) / `notebooks` / `lean-skeleton`.** Debt #6 should be
re-rated **live, not unused** (§3.8): Visser's Sufficient condition 2 is a
refereed, index-based sufficient criterion needing no $\pi(x)$ inversion, and is a
direct competitor to `outcomes.md#A6`'s proposed OB-F6b.

**Debt status after this leg:**

| debt | status |
|---|---|
| #1 record CSG ratio + locus | **CLOSED** (V0 + V1 + C, two independent routes) |
| #2 Kourbatov range + statements | **CLOSED**, with a corrigendum-driven range correction |
| #3 Granville $2e^{-\gamma}$ | **CLOSED** (V0, p. 13) — recorded as heuristic, not theorem |
| #4 explicit $p_n$ / $\pi(x)$ bounds | **CLOSED**, attribution corrected (Axler, not R–S/Dusart) |
| #5 BHP 0.525; Cramér RH bound | **CLOSED at V2**; BHP $x_0$ ineffective (G3); Cramér 1920 needs disambiguation |
| #5b Erdős–Rankin / FGKMT | **CLOSED at V1** |
| #6 Nicholson/Farhadian | **CLOSED — and re-rated live, not killed** |

---

*Emitted by the `source-ledger` leg (node). The conjecture is
**open**; this leg found no evidence bearing on its truth in either direction, and
none of the rows above should be read as such evidence.*
