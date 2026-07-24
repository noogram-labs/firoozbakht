# CC-17 — Kourbatov Theorem 3 (corrected): $g_k < \ell_k - 1.17 \Rightarrow$ F

| | |
|---|---|
| **Kind** | lemma (sufficient condition) |
| **Status** | **SOURCED (V0)**, and **corrigendum-dependent** — the pre-corrigendum locator cites a withdrawn proof |
| **Depends on** | CC-08, CC-19 |
| **Feeds** | OB-C, OB-D, W4; CC-23, CC-28 |
| **Ledger row** | `kourbatov2015upper` (**V0**), **Theorem 3 (§4)**, *as corrected* ¶ — *"If $p_{k+1}-p_k < \log^2 p_k - \log p_k - 1.17$ for all $k>9$ ($p_k \ge 29$), then Firoozbakht's conjecture (1) is true."* Range split from `kourbatov2015corrigendum` (**V0**) and `axler2016corrigendum` (V2). Companion family: same source, **Theorem 4** ¶. |

## Statement

$$g_k \;<\; \log^2 p_k - \log p_k - 1.17 \quad \text{for all } k > 9
\qquad\Longrightarrow\qquad \text{Firoozbakht's conjecture is true.}$$

**The proof is valid for $p_k \ge 2\,634\,800\,823$** (via `axler2014newbounds`
Cor. 3.5); on $p_k \in [29,\ 2\,634\,800\,823]$ both the conjecture and the hypothesis are
verified unconditionally by computation.

## The corrigendum — why this card exists separately from CC-16

The published Theorem 3 originally rested on Axler's Corollary 3.5 at "$x \ge 5.43$".
Axler's own corrigendum moved that threshold to $x \ge 2\,634\,800\,823$ — **nine orders
of magnitude** — and Kourbatov published a corrigendum rewriting the proof accordingly.

> **Any citation of "$b = 1.17 \Rightarrow$ Firoozbakht" that does not carry
> $p_k \ge 2\,634\,800\,823$ plus the unconditional check on $[29,\ 2\,634\,800\,823]$
> is citing a withdrawn proof.**

`source-ledger.md#1`, finding 5. Cite arXiv **1506.03042v4** (12 Mar 2019), which
incorporates the corrigendum. `citation-gate` should fail any claim that drops the range
split.

## The $b \to 1$ family (Theorem 4)

Kourbatov's Theorem 4 (V0) gives three further sufficient conditions of the shape

$$g_k \;<\; \log^2 p_k - \log p_k - 1 - \frac{c_1}{\log p_k} - \frac{c_2}{\log^2 p_k} - \cdots$$

with $(c_i)$ drawn from Axler's family: $(3.83)$; $(3.35,\ 15.43)$;
$(3.35,\ 12.65,\ 89.6)$ — each **for all $p_k > 4\times10^{18}$**, the sub-frontier part
covered by `kourbatov2015verification`.

The structure to read off: the sufficient constant $b$ approaches 1 from above as more
terms are taken, while the necessary constant (**CC-16**) is exactly 1. **The gap between
what is necessary and what is sufficient closes to zero asymptotically** — the dictionary
is tight. That is the sharpest available statement of the barrier's location, and it is
the honest version of `decompose.md#0.2`'s headline.

## Relation to attack.md's unsourced expansion

Theorem 4's coefficients $(3.83)$, $(3.35, 15.43)$, $(3.35, 12.65, 89.6)$ come from
Axler's $\pi(x)$ bound family and are **V0**. They are *not* the coefficients
$(3, 13)$ that `attack.md#2` asserts for the expansion of $f_n$ itself — those are the
Cipolla / A233824 family and remain **unsourced** (gap **G1**, see **CC-18**). Two
different series; do not merge them.

## Role in the proof-obligation tree

- **W4** in `decompose.md#3.5` — "F $\Rightarrow g_k < \ell_k - 1$ for $k\ge10$" is
  **CC-16**; this card is its converse partner and together they close the dictionary.
- Feeds **CC-23**: the barrier is now quantified. A proof of Firoozbakht needs a gap
  bound with constant $\le 1.17 - o(1)$; nothing in analytic number theory delivers a
  polylogarithmic bound with *any* constant.

## Traps

- The hypothesis is *universally quantified over all $k > 9$*. It is not a per-index
  criterion — a single $k$ satisfying it proves nothing. (Compare **CC-12**, which *is*
  per-index.)
- $1.17$ is not a rounding of $1$. It is Axler's constant, and the difference between
  $\ell_k - 1$ and $\ell_k - 1.17$ is the entire width of the necessary/sufficient gap at
  finite height.
- Do not quote a proposition number from `dusart2018explicit` in this chain. That source
  is **V3, metadata only** (ledger gap G4). The chain is **Axler**, not
  Rosser–Schoenfeld and not Dusart 2018 (`source-ledger.md#1`, finding 4).
