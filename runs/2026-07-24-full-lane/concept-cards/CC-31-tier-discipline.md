# CC-31 — Tier discipline: two systems, one propagation rule

| | |
|---|---|
| **Kind** | technique |
| **Status** | **PROVED** (the propagation rule is a triviality; its *absence* upstream is the defect) |
| **Depends on** | — |
| **Feeds** | every card; `citation-gate`, `evidence-gate`, `write-paper` |
| **Ledger row** | NONE — methodology. Defines how every other card's **Ledger row** field is read. |

## The two systems

They grade **different things** and must never be mixed.

| | grades | scale | owner |
|---|---|---|---|
| **L0–L3** (`decompose.md#0.1`) | how well a **claim** is established | L0 proved here · L1 standard · L2 attributed, locator unverified · L3 heuristic/recalled | the claim's author |
| **V0–V3** (`source-ledger.md#0`) | how well a **citation** was checked | V0 primary read verbatim at the locator · V1 metadata page · V2 reliable secondary · V3 bibliographic only | `source-ledger` |

A **V0 row can carry a false conjecture** (Cramér's, **CC-21** — verbatim, verified
locator, not a theorem). A **V3 row can carry a theorem** (Rosser–Schoenfeld's results are
true; the ledger simply never obtained the paper). Orthogonal axes.

In this deck: a card's **Status** is the L-axis; its **Ledger row** carries the V-axis.

## The propagation rule

The missing rule, and the deck's operating discipline:

$$\boxed{\;\text{tier(conclusion)} \;=\; \text{the \textbf{weakest} tier among its premises.}\;}$$

A composite may be tagged **L0 only if every premise is L0 or L1**. `decompose.md#0.1`
defines tiers for *statements* and states **no propagation rule for inference**, so
composites silently inherit their *strongest* premise's tier — which is how the headline
shortfall came to be tagged L0 while routing through an unclosed L2 node
(`outcomes.md#A3`).

**How the deck applies it**, on the headline itself:

| version | premises | tier |
|---|---|---|
| $g_n \lessgtr T_n$, sharp to $1+O((\log p)^2/p)$ | CC-06, CC-11, CC-12, CC-13 — all L0 | **L0** ✓ |
| $T_n \approx \log^2 p_n - \log p_n$ | CC-20 (V0 external) | **L1**, not L0 |
| shortfall $= 1.05426$ | **CC-07** (V0 published $f_k$) ÷ published $g_k$ | **L0/L1** — the exact barrier removes the L2 node entirely (**CC-26**) |

The third row is the deck's answer to A3: don't repair the tier, remove the premise.

## The two missing tiers

`outcomes.md#A10` identifies two states the L-scale cannot express, both of which
occurred:

- **`[CORROBORATED]`** — survived $N$ severe tests without refutation. Without it, the
  strongest empirical output of the `decompose` leg (no counterexample over three million
  indices, *independent of the literature*) was filed as a "sanity floor".
- **`[ASSERTED-ABOUT]`** — a claim *about* a computation that the computation does not
  produce. All three of `outcomes.md#A1`'s errors live here: an error bound never
  measured, a `SAFETY_MARGIN` never compared against it, a minimum never printed.

`[ASSERTED-ABOUT]` is the operationally important one. Its rule, from the D1 event:

> **A passage that reasons *about* a computation rather than *from* it is unverified until
> the computation prints the number the prose asserts.**

That defect passed **two of five adversarial reviewers** who reproduced its reasoning
verbatim. It is not caught by more review; it is caught by making the script print the
number. This deck's `verify_cards.py` prints every extremal statistic its cards quote —
that is the discipline, not a courtesy.

## Rules for the gates

**`citation-gate`.** Every paper claim names a citekey **and** a §3 locator. Fail any
claim resting on a bare **V3** row (`dusart2018explicit`, `rossersch1962`,
`rankin1938difference`, `nicely_gaplist`, all OEIS rows) or on gaps **G1–G6**. Check
specifically: the $b = 1.17$ range split (**CC-17**); $2^{64}$ vs $4\times10^{18}$
(**CC-25**); the $(\log\log\log X)^1$ exponent (**CC-24**); the $p_1 > 7$ caveat
(**CC-05**); the unsourced $-3/L - 13/L^2$ expansion (**CC-18**).

**`evidence-gate`.** Check `#print axioms`, not the absence of `sorry` (**CC-29**). Treat
a sorry-free `firoozbakht` as a red flag requiring statement re-audit.

**`write-paper` / `editorial-verdict`.** The deliverable is W1–W3, W6 and W5 to a modest
$N_0$. **It is not a proof of Firoozbakht and must not drift into sounding like one.**
Debt #12, Critical.

## Traps

- "L2" is a **debt, not a citation** (`decompose.md#0.1`). Restating an L2 claim does not
  promote it; only `source-ledger` obtaining the source does.
- A V3 row quoted downstream must be restated as *"the literature is reported to
  contain…"*, or promoted first.
- **Corrigenda.** An uncorrected locator is a **wrong** locator even when the paper
  exists. Two rows in this corpus needed it (`axler2016corrigendum`,
  `kourbatov2015corrigendum`); both changed a validity range by nine orders of magnitude.
- **Sociology is not a tier.** *"The community's heuristic expectation is that F is
  false"* (`decompose.md#8`-6) is untiered and followed by *"therefore"*. Tag **[L3]**,
  and note that `#3.6`'s budget allocation is invariant under flipping it while `#2.4`'s
  prohibition and debt #11's mandate are **not** (**CC-21**, **CC-30**).
