# CC-28 — The integer sufficient criterion

| | |
|---|---|
| **Kind** | technique |
| **Status** | **PROVED** (derivation below; soundness and coverage both `[C]`-verified) |
| **Depends on** | CC-12 |
| **Feeds** | OB-B, OB-F6b, W5; CC-29 |
| **Ledger row** | NONE — derived here. Proposed in `outcomes.md#A6`(iii); this card supplies the derivation and the verification. Competitor: `visser2019verifying` **Sufficient condition 2** (V0), see **CC-22**. |

## Statement

For $n \ge 1$, write $b_n := \lfloor \log_2 p_n \rfloor$ (in Lean/Python:
`p.bit_length() - 1`). Then

$$\boxed{\;1000\,n\,g_n \;\le\; 693\,b_n\,p_n \quad\Longrightarrow\quad \text{Firoozbakht holds at } n.\;}$$

**Pure natural-number arithmetic.** One multiplication and one comparison per index. No
`Real`, no `log`, no `rpow`, no floating point, no explicit $p_n$ bound.

## Derivation

$\log 2 > 0.693$ — in Mathlib, `Real.log_two_gt_d9` gives
$\log 2 > 0.6931471803$. Since $2^{b_n} \le p_n$,

$$\log p_n \;\ge\; b_n \log 2 \;>\; 0.693\,b_n \;=\; \frac{693\,b_n}{1000}.$$

Assume the hypothesis $1000\,n\,g_n \le 693\,b_n\,p_n$. Then

$$n\,g_n \;\le\; \frac{693\,b_n}{1000}\,p_n \;<\; p_n\log p_n
\qquad\Longleftrightarrow\qquad
g_n \;<\; \frac{p_n}{n}\log p_n \;=\; T_n,$$

which is the hypothesis of **(SUF)** (**CC-12**), whence $S_n > 0$. ∎

Note the inequality is *strict* at the $\log 2$ step, so the non-strict integer hypothesis
yields the strict $T_n$ hypothesis — no boundary case is lost.

## Verification `[C]`

`verify_cards.py` CHECK 6, over $n \le 3\,001\,133$:

| claim | result |
|---|---|
| **soundness** — the criterion never fires where (SUF) fails | ✓ zero violations (checked $n \le 200\,000$) |
| **coverage** — indices where the criterion does *not* fire | **exactly $n = 2$ and $n = 4$** |

So the integer relaxation loses **nothing**: it covers precisely the same index set as
(SUF) itself over three million indices. The $\log 2$ rounding, which one would expect to
cost coverage, costs zero — because the (SUF) margin is $\gg$ the rounding everywhere
except at the two indices where (SUF) already fails (**CC-12**, **CC-14**).

## Why this is the engine for W5

`decompose.md#3.5`-W5 proposes machine-checking Firoozbakht to $N_0$ using F2
($p_{n+1}^n < p_n^{n+1}$). That comparison is a bignum exponentiation: $\sim O(n^{1.6})$
per index, $O(N_0^{2.6})$ total, putting $N_0 = 10^6$ on the order of weeks *in Python*
before a single primality certificate (`outcomes.md#A6`).

This criterion is **one multiplication per index** — roughly $10^6\times$ cheaper at
$n = 10^5$ — and `decide` discharges it in microseconds. It is what makes a Lean finite
verification tractable at all.

The two exceptions are handled by hand: $n = 2$ and $n = 4$ are discharged by the exact
integer facts $5^2 = 25 < 27 = 3^3$ and $11^4 = 14641 < 16807 = 7^5$ — which are **the
same two witnesses as the fidelity anchor set** (**CC-01**, **CC-29**). The criterion's
blind spot and the index-convention test coincide, so no extra work is created.

## Role in the proof-obligation tree

- **OB-F6b** (new, `outcomes.md#A6`): the criterion, `decide`-discharged.
- **W5**: revised target $N_0 = 10^4$–$10^5$ **in index** (**CC-25**), with this as the
  engine.
- Evaluate against **CC-22**'s competitor (Visser Sufficient condition 2, V0): also
  index-based, also needs no $\pi(x)$ inversion, but involves $\ln(n\ln n)$ — i.e. it is
  a *real* criterion, not an integer one, so it needs `Real` machinery that this card
  avoids. `lean-skeleton` should benchmark both.

## Traps

- **`bit_length` is 1-indexed against $\log_2$.** $\lfloor\log_2 p\rfloor = $
  `p.bit_length() - 1`. Off by one here silently doubles or halves the threshold.
- The criterion is **sufficient only**. Failure means "no verdict at this index", never
  "counterexample". Route failures to **CC-10**.
- $693/1000$ is a *lower* bound on $\log 2$ — using $694/1000$ makes the criterion
  **unsound**. The direction of the rounding is the whole proof.
- Still needs $n = \pi(p_n)$ to be correct (**CC-01**, OB-B2). The criterion is cheap in
  arithmetic, not in the index-tracking obligation.
