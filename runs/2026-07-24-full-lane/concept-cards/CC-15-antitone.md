# CC-15 — $S_n$ is strictly decreasing in $n$ (the E3 collapse)

| | |
|---|---|
| **Kind** | lemma |
| **Status** | **PROVED** (structural half). Its *operational* corollary carries a caveat that upstream states too strongly — see below. |
| **Depends on** | CC-03 |
| **Feeds** | OB-E3, OB-F7, W6; CC-29 |
| **Ledger row** | NONE for the lemma. The explicit $\pi(x)$ lower bounds that make the corollary cheap are `axler2014newbounds` **Cor. 3.6** (V0) — see **CC-19**. |

## Statement

Fix a prime $p$ and a gap $g > 0$, and regard

$$\sigma(m) \;:=\; \log p - m\log\!\Big(1+\frac{g}{p}\Big), \qquad m \in \mathbb{R}_{>0},$$

as a function of the index alone. Then $\sigma$ is **strictly decreasing**, affine in $m$
with slope $-\log(1+g/p) < 0$. In particular $S_n = \sigma(n)$ for the true index
$n = \pi(p)$, and

$$m \ge n_0 \ \text{ and }\ \sigma(n_0) \le 0 \quad\Longrightarrow\quad \sigma(m) \le 0 .$$

## Proof

$\sigma$ is affine in $m$ with slope $-\log(1+g/p)$. Since $g > 0$ and $p > 0$,
$1 + g/p > 1$, so $\log(1+g/p) > 0$ and the slope is strictly negative. ∎

## The corollary that collapses OB-E3

> **To refute Firoozbakht at $(p, g)$ one needs only a *lower* bound $n \ge n_0$ with
> $n_0\log(1+g/p) \ge \log p$. To verify it at a given index one needs only an *upper*
> bound on $n$.**

This is a genuine structural saving. The naive obligation is "compute $n = \pi(p)$
exactly", which at $p \sim 10^{18}$ means Meissel–Lehmer or Lagarias–Miller–Odlyzko — a
serious computation. The lemma replaces it with a closed-form evaluation of an explicit
Chebyshev-type $\pi(x)$ bound (**CC-19**), which is essentially free.

`decompose.md#2.5-E3` is right about this, and it is one of the three L0 results the
panel explicitly declined to break.

## The caveat upstream understates

`decompose.md` says the cost is "essentially zero". That holds only for a witness that
clears the threshold by **more than the $\pi(x)$ bound's relative deficit**
($\sim 10^{-4}$ for Axler's bounds at the frontier). And (`outcomes.md`, E3 caveat):

> the **first** counterexample, by definition of a first crossing, is the one with the
> *smallest* excess — so E3 is weakest exactly where it will be used.

Concretely: the refutation condition is $n \ge \log p / \log(1+g/p)$. Near threshold that
required index is within a relative $\varepsilon$ of the true $\pi(p)$, and an explicit
lower bound with relative deficit $\delta$ settles it only if $\varepsilon > \delta$. A
marginal witness needs exact prime counting after all. **State the collapse with this
qualifier attached, every time.**

## Role in the proof-obligation tree

- **OB-E3**, **W6**, **OB-F7** (Lean, low difficulty — affine monotonicity).
- Also the reason a refutation search need not track indices at all during the scan: it
  can scan $(p, g)$ pairs and resolve $n$ only for the nominations.

## Traps

- The lemma is about $\sigma$ as a function of a *free* index $m$, with $(p,g)$ held
  fixed. It says **nothing** about how the true $S_n$ behaves as $n$ ranges over actual
  primes — there $p_n$ and $g_n$ both change, and $S_n$ is wildly non-monotone (its
  minimum over the sieved range is at $n = 2$, **CC-03**). Conflating the two readings is
  the obvious misuse.
- Direction discipline: **lower** bound on $n$ to refute, **upper** bound to verify.
  Getting it backwards produces a refutation claim from a verification bound.
- In Lean this is antitonicity of an affine map, not of `S` as a sequence. State the
  free-variable form (`OB-F7`) or the lemma is unusable.
