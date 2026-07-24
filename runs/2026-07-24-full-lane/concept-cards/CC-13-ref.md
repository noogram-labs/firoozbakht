# CC-13 — (REF): $g_n \ge T_n(1 + g_n/p_n) \Rightarrow \lnot$F at $n$

| | |
|---|---|
| **Kind** | lemma |
| **Status** | **PROVED** (proof below). Valid for every $n \ge 1$; one-directional. |
| **Depends on** | CC-06, CC-11 |
| **Feeds** | OB-C2, OB-E1, OB-F6, W3, FT-2; CC-14 |
| **Ledger row** | NONE — derived here. |

## Statement

For every $n \ge 1$:

$$g_n \;\ge\; \frac{p_{n+1}}{n}\log p_n \;=\; T_n\Big(1 + \frac{g_n}{p_n}\Big)
\qquad\Longrightarrow\qquad
S_n < 0, \text{ i.e. Firoozbakht FAILS at } n .$$

## Proof

By the left half of **CC-11** with $x = g_n/p_n$, noting $1 + x = p_{n+1}/p_n$:

$$n\log\!\Big(1+\frac{g_n}{p_n}\Big) \;>\; n\cdot\frac{g_n/p_n}{p_{n+1}/p_n}
= \frac{n\,g_n}{p_{n+1}} \;\ge\; \frac{n}{p_{n+1}}\cdot\frac{p_{n+1}\log p_n}{n} = \log p_n ,$$

using the hypothesis in the last step. Hence $S_n = \log p_n - n\log(1+g_n/p_n) < 0$. ∎

## Why this card matters more than (SUF) does

(REF) is the **only** implication in the deck that can refute the conjecture from a
single index. One firing ends the problem. That makes its exact statement operationally
critical, and it is stated wrongly in two places upstream:

- **OB-E1** (`decompose.md#2.5`) is written in (REF) form as a *decision*, so a candidate
  inside the dead band fails the screen and **never reaches E4** — the only obligation
  carrying the exact predicate (`outcomes.md#A5`).
- **FT-2** (`decompose.md#4`) triggers on the **(SUF)** boundary — the *lower* edge — and
  has a demonstrated 4/4 false-positive rate on $n = 1,2,3,4$. Its teeth are recorded as
  "maximal"; they are screening only.

Per `outcomes.md#A5`, the repairs are: restate E1 as a screen with an explicit tolerance
$\delta_n$ covering the proxy error, the float resolution and the band width; restate
FT-2's trigger in (REF) form; make **CC-10** the sole adjudicator; and add a
falsifiability test for the **silent false negative** (a genuine counterexample rejected
by a screen), which the upstream test suite does not contain.

## The self-referential form

The hypothesis $g_n \ge T_n(1 + g_n/p_n)$ has $g_n$ on both sides. Solving:

$$g_n\Big(1 - \frac{T_n}{p_n}\Big) \ge T_n
\quad\Longleftrightarrow\quad
g_n \;\ge\; \frac{T_n}{1 - T_n/p_n} \qquad (\text{valid when } T_n < p_n).$$

That right-hand side is the **upper** edge of the dead band (**CC-14**). The condition
$T_n < p_n$ is equivalent to $\log p_n < n$, which holds for $n \ge 4$ and fails at
$n = 1, 2, 3$ — one more reason the small-$n$ regime resists every relaxed treatment.

## Role in the proof-obligation tree

- **W3** in `decompose.md#3.5`; **OB-F6** in Lean.
- Screens **OB-E1** and **FT-2** (as screens, per above).
- With **CC-12**, defines the band **CC-14**.

## Traps

- The threshold is $p_{n+1}$-based, not $p_n$-based. Writing $T_n$ where
  $T_n(1+g_n/p_n)$ belongs understates the refutation threshold by one band width and
  turns a nomination into a false claim of refutation.
- A firing of (REF) is a refutation **of the conjecture**, not of a lemma — so it must be
  adjudicated at maximum rigour before it is reported: exact or interval arithmetic on
  **CC-10**, a consecutivity certificate (OB-E2), and an index lower bound (**CC-15**).
- `verify_small_range.py` emits the string `COUNTEREXAMPLE` from its non-exact branch.
  Per `outcomes.md#A5` that must read `CANDIDATE (float) — requires exact re-check`.
