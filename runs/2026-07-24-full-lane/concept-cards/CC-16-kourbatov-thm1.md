# CC-16 — Kourbatov Theorem 1: F $\Rightarrow g_k < \ell_k - 1$ for $k > 9$

| | |
|---|---|
| **Kind** | lemma (necessary condition) |
| **Status** | **SOURCED (V0)** + `[C]` — the consequent is verified directly over $10 \le k \le 3\,001\,133$, and the range restriction is verified to be load-bearing |
| **Depends on** | CC-08, CC-19 |
| **Feeds** | OB-C4, OB-D, FT-6, FT-7′; CC-23 |
| **Ledger row** | `kourbatov2015upper` (**V0**), **Theorem 1 (§2)** ¶ — *"If conjecture (1) is true, then $p_{k+1}-p_k < \log^2 p_k - \log p_k - 1$ for all $k > 9$."* Proof runs through `axler2014newbounds` **Cor. 3.6** at $x \ge 1\,772\,201$ ($k \ge 133\,115$); the range $9 < k < 133\,115$ is covered by direct computation. |

## Statement

$$\text{Firoozbakht's conjecture} \quad\Longrightarrow\quad
g_k \;<\; \log^2 p_k - \log p_k - 1 \;=\; \ell_k - 1
\qquad \text{for all } k > 9 .$$

This is the **necessary** direction of the Firoozbakht ⟷ prime-gap dictionary. Its
partner, the sufficient direction, is **CC-17**.

## Why it is the load-bearing consequence

The conjecture, in its own coordinates, is a statement about $n$-th roots. Theorem 1
converts it into a statement in the *standard vocabulary of the prime-gap literature*:
a Cramér-type upper bound with leading constant exactly 1. That conversion is what makes
**CC-23** — the barrier argument — sayable at all, and it is what licenses the structural
filter of **CC-30**:

> Any correct proof of Firoozbakht entails a polylogarithmic prime-gap upper bound with
> constant 1, for every $k > 9$, with no exceptional set.

(Entails. Not "must textually contain" — that stronger reading is the FT-7 error;
see **CC-30**.)

## Verification `[C]`

`verify_cards.py` CHECK 10, over the sieved range:

| claim | result |
|---|---|
| $g_k < \ell_k - 1$ for every $k \in [10,\ 3\,001\,133]$ | **holds, zero violations** |
| the same for some $k \le 9$ | **fails at $k = 1, 2, 3, 4, 6, 9$** |

The second row is the point. Kourbatov's *"for all $k > 9$"* is **load-bearing, not
cosmetic** — the inequality is genuinely false at six of the first nine indices. Any
downstream restatement dropping the range restriction is false. (This is deck delta
**D4**; `decompose.md#2.3-C4`'s "matches to the constant" framing does not carry it.)

Note what CHECK 10 does and does not establish. It verifies the **consequent** over the
sieved range — which must hold there anyway, since Firoozbakht is verified far past it
(**CC-25**). It does **not** verify the implication, whose proof needs Axler's bound and
is not reproduced here.

## Relation to the exact barrier

$f_k \le \ell_k - 1$ eventually (**CC-18**), but not always: $f_k < \ell_k$ first holds
permanently at $k = 1412$, $p_k = 11783$ `[C]`. So Theorem 1's consequent is *not*
simply "$g_k < f_k$ plus $f_k < \ell_k - 1$" at small $k$ — Kourbatov's $k > 9$ range and
his explicit-computation range $9 < k < 133\,115$ exist precisely to bridge the region
where the asymptotic ordering has not yet settled.

## Traps

- **Necessary $\ne$ sufficient.** $\ell_k - 1$ (this card) and $\ell_k - 1.17$
  (**CC-17**) are different constants doing opposite jobs. Substituting one for the other
  swaps a consequence for a criterion.
- Sun's variant with $+1$ instead of $-1$ (`sun2013sequence`, V1) is a **weaker**
  consequence, and Sun's own theorem is about $\sqrt[n]{S_n/n}$ with $S_n = \sum_{i\le n}p_i$,
  **not** about Firoozbakht (`source-ledger.md#3.6`). Do not cite Sun as a source for a
  Firoozbakht consequence without that distinction.
- Cite arXiv **1506.03042v4**, which incorporates the corrigendum. Earlier versions carry
  the withdrawn range in Theorem 3 (**CC-17**).
