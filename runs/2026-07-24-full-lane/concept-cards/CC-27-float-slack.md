# CC-27 — Rigorous floating-point slack evaluation

| | |
|---|---|
| **Kind** | technique |
| **Status** | **PROVED** (the error bound is derived below and measured). Supersedes `decompose.md#5.4`, which is wrong by $3.3\times10^3$ in the unsafe direction. |
| **Depends on** | CC-03, CC-10 |
| **Feeds** | OB-B, OB-E4, FT-1; `notebooks__0/1/2` |
| **Ledger row** | NONE — derived here. |

## The rule

$$\boxed{\;S_n \;=\; \log p_n \;-\; n\cdot\texttt{log1p}\big(g_n/p_n\big),
\qquad \text{guard} \;=\; 6u\log p_n,\ \ u = 2^{-53},
\qquad \text{require } p_n < 2^{53}.\;}$$

Never `log(p_{n+1}/p_n)`. Never `(n+1)*log(p_n) - n*log(p_{n+1})`. Never a hardcoded
margin. Above $p \sim 10^{16}$, do not use doubles at all.

## Why the upstream analysis fails

`decompose.md#5.4` argues: *"neither term in $S_n$ grows with $n$ (since
$n\log(1+g_n/p_n) = \log p_n - S_n \le \log p_n \le 18$), so the absolute error in $S_n$
is $\approx 10^{-13}$."*

The argument is **true of the value and silent about the error**. If the ratio
$p_{n+1}/p_n$ is formed first and then logged, the log's argument carries a relative error
$\sim u$, so `log(pn1/pn)` carries an **absolute** error $\sim u$ — and multiplying by the
exact integer $n$ amplifies it to $nu$. Linear in $n$, exactly the growth the passage
argues is absent.

At $N = 5\times10^7$: $nu + 6u\log p_n = 3.3\times10^{-10}$, measured tight to 3% against
60-digit decimal (`outcomes.md#A1`). The script's `SAFETY_MARGIN = 1e-11` is therefore
**0.033×** the error it guards — a silent-failure window in the one mechanism advertised
as failing loudly.

This is the corpus's canonical instance of `outcomes.md`'s D1 lesson: **two of five
adversarial reviewers read the passage and declared it sound**, because it is persuasive
and reasons *about* a computation rather than *from* it.

## The fix, and its bound

`log1p(x)` computes $\log(1+x)$ with relative error $\sim u$ **in the result**, not in the
argument. Writing $x = g_n/p_n$ (one rounding, relative error $u$):

$$\text{rel. error in } n\,\texttt{log1p}(x) \;\lesssim\; 3u
\quad\Longrightarrow\quad
\text{abs. error} \;\lesssim\; 3u\cdot n\log(1+x) \;=\; 3u(\log p_n - S_n) \;\le\; 3u\log p_n .$$

Adding the error in `log(p_n)` itself ($\le 2u\log p_n$) and one subtraction rounding:

$$\big|\Delta S_n\big| \;\le\; 6u\log p_n .$$

**Independent of $n$.** At $p = 5\times10^7$ this is $1.18\times10^{-14}$ `[C]` — a
$2.8\times10^4$ improvement over the ratio-then-log form, achieved by changing one
function call.

## The four rules, and why each is separately load-bearing

1. **`log1p`, not `log` of a ratio.** Removes the $n$-amplification. One line.
2. **Runtime guard, never hardcoded.** Compute $6u\log p_n$ per index and compare. A
   constant chosen once is a constant that is wrong at a different height.
3. **Print the extremum.** The script must emit $\min_{n > \texttt{EXACT\_N}} S_n$. Its
   absence is *the mechanism* that let the wrong bound survive: `decompose.md#5.4` quoted
   "$> 3.5$" for a quantity the script never printed, having substituted the
   **normalised** slack (which it does print) for the **unnormalised** one. The true
   value is $\mathbf{1.7008}$ at $n = 217$ `[C]`.
4. **Assert $p < 2^{53}$.** Above that, integers are not exactly representable and the
   whole analysis is void.

## Where doubles die

Above $p \approx 10^{16}$ the double-precision slack test is **meaningless**:

- at the largest known maximal gap, the amplified error bound ($nu \approx 47$) exceeds
  the slack ($S \approx 8.6$) by $5.5\times$;
- $g/p$ falls **below** the unit roundoff, so `1 + g/p` rounds to 1 and `log1p` is the
  only formulation that survives at all.

**Prescription above $10^{16}$**: `fractions.Fraction`, `decimal`/`mpmath` at declared
precision, or the integer criterion of **CC-28**. This is not optional — it is the height
band the refutation branch actually targets (**CC-24**), and `decompose.md#5.4`'s
instruction *"notebooks legs extending this range must carry the same guard"*, obeyed as
written, produces **confident false results there with the guard silent**.

## The exact-adjudication ladder

| regime | tool |
|---|---|
| $n \le 200$ | exact integers, $p_{n+1}^n$ vs $p_n^{n+1}$ (**CC-09**) |
| $200 < n$, $p < 10^{16}$ | doubles with `log1p` + runtime guard (this card) |
| $p \ge 10^{16}$, screening | **CC-28** integer criterion, or `mpmath` at declared dps |
| any adjudication of a candidate | **CC-10** in exact/interval arithmetic — **mandatory** |

## Traps

- A screen hit must be reported as `CANDIDATE (float) — requires exact re-check`, never
  as `COUNTEREXAMPLE` (`outcomes.md#A5`).
- The **normalised** slack $\hat S_n = S_n/\log p_n$ and the raw $S_n$ differ by a factor
  of up to 18 over the sieved range. State which one every time (**CC-03**).
- `verify_small_range.py`'s docstring claims it "reproduces every `[COMPUTED]` number in
  decompose.md". It does not — the $\min_{n>200} S_n$ figure and four numbers at loci
  above the sieve are not reproduced. Retract the claim or make it true
  (`outcomes.md#A11`).
