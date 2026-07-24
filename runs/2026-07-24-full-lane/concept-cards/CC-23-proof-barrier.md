# CC-23 — The proof-side barrier

| | |
|---|---|
| **Kind** | barrier |
| **Status** | **PROVED** (the *requirement*, from CC-16/CC-17) + **SOURCED** (the best known bounds) + **OPEN / `[L3]`** (the universal negative) |
| **Depends on** | CC-08, CC-16, CC-17, CC-18, CC-21 |
| **Feeds** | OB-D1–D4, OB-G (barrier/metamathematics), FT-7′; CC-30 |
| **Ledger row** | `bhp2001difference` (**V2**), main theorem ¶ — $[x - x^{0.525}, x]$ contains primes for large $x$. `cramer1920some` (**V2** statement / V1 bibliographic) — on RH, $p_{n+1}-p_n = O(\sqrt{p_n}\log p_n)$. `granville1995harald` (**V0**) for Cramér's conjecture (**CC-21**). `dusart2010estimates` **Prop. 6.8** (V0) for the best *explicit* short-interval result. |

## The requirement, stated exactly

By **CC-16** (Kourbatov Thm 1, V0), Firoozbakht entails

$$g_k \;<\; \log^2 p_k - \log p_k - 1 \qquad \text{for every } k > 9,$$

and by **CC-17** the converse holds with constant $1.17$, the two closing to each other
as $k \to \infty$ (**CC-18**). So:

> **A proof of Firoozbakht's conjecture requires a prime-gap upper bound that is
> *polylogarithmic*, has leading constant $\le 1$, holds for *every* $k > 9$ with **no
> exceptional set**, and is *explicit* (effective constants).**

Each of the four qualifiers is separately fatal to the known toolkit.

## What is actually available

| route | best known | shortfall |
|---|---|---|
| **unconditional** (Baker–Harman–Pintz) | $g_n \ll p_n^{0.525}$ | $p^{0.525}$ vs $(\log p)^2$: at $p = 10^{18}$, $\sim 10^{9.5}$ vs $\sim 1.7\times10^3$ — **six orders**, and the ratio diverges |
| **under RH** (Cramér 1920) | $g_n \ll \sqrt{p_n}\log p_n$ | still a power of $p$ against a power of $\log p$ |
| **explicit short intervals** (Dusart Prop. 6.8, V0) | a prime in $(x,\ x(1+\frac{1}{25\ln^2 x})]$ | interval length $\sim x/\ln^2 x$; required $\sim \ln^2 x$ — a factor $x/\ln^4 x$ |
| **granting Cramér** | $\limsup R_n = 1$ | **still insufficient**: a $\limsup$ of 1 does not give $R_k < 1 - 1/\log p_k$ for *every* $k$ (**CC-21**) |
| **granting Granville's revision** | $\limsup R_n \ge 2e^{-\gamma} > 1$ | would **refute** Firoozbakht |
| **GRH / Elliott–Halberstam / DHL sieve** | bounded and small gaps (Zhang, Maynard, Polymath) | **wrong direction** — these are lower-density/small-gap tools; Firoozbakht is an upper-gap statement |

Two structural readings, both worth stating:

- **The distance is categorical, not quantitative.** Every unconditional or
  RH-conditional route produces a bound that is a *power of $p$*. The target is a *power
  of $\log p$*. No amount of constant-chasing crosses that.
- **Even the conjectural route fails.** Granting Cramér — the strongest thing anyone
  believes about extreme gaps — still does not close, because Firoozbakht needs a
  per-index bound and Cramér gives a $\limsup$. Granting Granville's *refinement* refutes
  it instead. **There is no assumption in current circulation whose adoption proves
  Firoozbakht.**

## The `[L3]` clause — flagged, not asserted

`decompose.md#2.4` states:

> *"No technique in analytic number theory — zero-density estimates, sieve methods,
> zero-free regions, RH, GRH — produces a polylogarithmic gap bound at all."*

That is a **universal negative over an open-ended class**, the strongest logical form
anywhere in this corpus, and nothing in this polymer establishes it. Per
`outcomes.md#A9` it is tagged **[L3]** here, and it is the sole support for the phrase
*"categorically blocked"*.

The defensible version, which the deck does assert:

> *For every route surveyed in the table above — the strongest unconditional result, the
> strongest RH-conditional result, the strongest explicit result, and the two standard
> conjectural models — the shortfall is a power of $p$ against a power of $\log p$, or a
> $\limsup$ against a per-index requirement. No surveyed route reaches the target.*

That is a claim about a finite surveyed set, it is falsifiable by exhibiting a
polylogarithmic gap bound, and it does not require knowing what analytic number theory
cannot do.

## Role in the proof-obligation tree

- **OB-D1–D4**: all four fail, for the reasons tabulated.
- **OB-G** (new node, `outcomes.md#A9`): barrier / metamathematics. This is where the
  deliverable that `decompose.md#2.4` *itself commissions* — "make the barrier statement
  rigorous and quotable" — actually lives, and `#3.6` rank 5 currently has no acceptance
  criterion for it. **Acceptance criterion proposed here**: the barrier statement is
  complete when (i) the requirement is stated with all four qualifiers and traced to a V0
  locator, (ii) each surveyed route's shortfall is a computed number at a named height,
  and (iii) the universal-negative clause is either proved, restricted to the surveyed
  set, or deleted.
- **CC-30** (FT-7′): the triage filter derives from this card and inherits its limits.

## Design instruction for `proof-attempt` legs

Do **not** budget attempts at closing D1–D4. Budget: (i) making this barrier statement
rigorous and quotable — with the surveyed-set restriction, not the universal negative;
(ii) formalizing the conditional implications (**CC-12**, **CC-13**, **CC-10**) in Lean,
which *is* achievable and *is* a real artifact; (iii) computing the shortfall of the best
**constructive** large-gap bound against $f_n$ at concrete heights ($10^{18}$, $10^{100}$,
$10^{1000}$) — a number that does not appear in the literature in this normalisation
(**CC-24**).

## Traps

- Bertrand-type and short-interval results bound gaps by *fractions of $p$*. Any bound of
  that shape is vacuous here (**CC-02**).
- $x^{0.525}$'s threshold $x_0$ is **ineffective as published** (ledger gap **G3**) — "could
  be determined with enough effort". A paper claiming an explicit unconditional bound must
  say so.
- "Categorically blocked" is a strategic conclusion, not a theorem. It rests on the `[L3]`
  clause. Downstream legs may act on it; `write-paper` may not assert it as established.
