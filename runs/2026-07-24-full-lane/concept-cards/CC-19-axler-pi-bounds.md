# CC-19 — Axler's explicit $\pi(x)$ bounds

| | |
|---|---|
| **Kind** | lemma (external, explicit) |
| **Status** | **SOURCED (V0)** — and **corrigendum-dependent** in the one place it is most used |
| **Depends on** | — (external input) |
| **Feeds** | OB-C4, OB-E3; CC-15, CC-16, CC-17, CC-18 |
| **Ledger row** | `axler2014newbounds` (**V0**), **Corollary 3.5** ¶ and **Corollary 3.6**, table row $(a,b,c,d;x_0) = (1,0,0,0;\,1\,772\,201)$ ¶. Correction: `axler2016corrigendum` (V2), via `kourbatov2015corrigendum` (V0). Ancestor: `panaitopol2000` (V2). |

## Statements

**Upper bounds (Cor. 3.5).** The member used in Kourbatov's chain:

$$\pi(x) \;<\; \frac{x}{\log x - 1 - \dfrac{1.17}{\log x}}
\qquad \text{for } x \ge \mathbf{2\,634\,800\,823}.$$

arXiv v3 states this for "$x \ge 5.43$"; **that threshold was withdrawn** by Axler's
corrigendum. Other members of the family (unaffected):

| bound | validity |
|---|---|
| $\pi(x) < x\big/\big(\log x - 1 - \frac{1}{\log x} - \frac{3.83}{\log^2 x}\big)$ | $x \ge 9.25$ |
| same shape with $(3.35, 15.43)$ | $x \ge 14.36$ |
| same shape with $(3.35, 12.65, 89.6)$ | $x \ge 21.95$ |

**Lower bound (Cor. 3.6).** Kourbatov's eq. (5), and the load-bearing input to his
Theorems 1 and 5:

$$\pi(x) \;>\; \frac{x}{\log x - 1 - \dfrac{1}{\log x} - \dfrac{1}{\log^2 x}}
\qquad \text{for } x \ge \mathbf{1\,772\,201}.$$

## Why this card, and not "Rosser–Schoenfeld / Dusart"

`decompose.md#2.3-C4` tags its $\pi(x)$/$p_n$ input **[L2, Rosser–Schoenfeld / Dusart]**.
That attribution is **wrong**: Kourbatov's chain uses **Axler**
(`source-ledger.md#1`, finding 4). Rosser–Schoenfeld is the historical ancestor of the
methodology and is in the ledger at **V3, metadata only** — cite it for lineage, never
for a constant. `dusart2018explicit` is likewise **V3**; do not quote a proposition
number from it (gap **G4**).

The correct division of labour:

| need | source | card |
|---|---|---|
| $\pi(x)$ bounds (Kourbatov's chain, and OB-E3's index bound) | **Axler**, Cor. 3.5/3.6, V0 | this card |
| $p_k$ bracket (evaluating $p_n/n$, hence $T_n$ and $c_n$) | **Dusart 2010**, Props. 6.6/6.7, V0 | **CC-20** |

## Role in the proof-obligation tree

- **OB-E3** (**CC-15**): a *lower* bound on $\pi(p)$ is all a refutation needs, and
  Cor. 3.6 supplies it in closed form. This is what collapses the "compute $\pi(x)$
  exactly" obligation — subject to the marginal-witness caveat on **CC-15**.
- **OB-C4**: the necessary and sufficient dictionary entries (**CC-16**, **CC-17**) both
  run through these corollaries. Their validity thresholds are the reason those theorems
  carry range restrictions rather than holding for all $k$.

## Traps

- **The $5.43 \to 2\,634\,800\,823$ correction is nine orders of magnitude.** A leg that
  reads arXiv v3 of Axler and does not read Kourbatov's corrigendum will state a
  sufficient condition that has been withdrawn (**CC-17**).
- These are bounds on $\pi(x)$, i.e. on the index given the height. Inverting them to
  bound $p_n$ given $n$ is a separate step with its own validity range — that is
  **CC-20**, not this card.
- Every bound here has a threshold $x_0$. Evaluating below $x_0$ silently returns a
  number that is not a bound. Any implementation must assert $x \ge x_0$, not assume it.
- `axler2016corrigendum` itself is **V2** — not obtained directly; its content is
  determined by the Kourbatov corrigendum (V0). That is adequate for the range
  correction and inadequate for anything else in it.
