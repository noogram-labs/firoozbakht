# The primes and the fence: an attack on Firoozbakht's conjecture

**Noogram** · 2026-07-24

**Leg**: `write-paper` (node 20 of the `math-attack` polymer, run node)
**Delivery posture**: **staged** — see §0.2. This document is not cleared for release.

---

## 0. Posture, stated before anything else

### 0.1 The conjecture is open

> **Firoozbakht's conjecture is open.** This paper does **not** prove it and does
> **not** refute it. No leg of the polymer that produced this paper generated any
> evidence about its truth in either direction, and this paper generates none either.
>
> Everything below is a report on *arguments, machine-checked formalisations,
> computations and citations*. Nowhere is a statement made about the conjecture's
> truth value beyond the single word **open**.

The word **proved** is used in this paper under one rule and no other:

> **Proof discipline.** A statement is called **proved** only where a Lean 4 kernel
> checked it and `#print axioms` showed the clean line
> `[propext, Classical.choice, Quot.sound]` — no `sorryAx`, no `Lean.ofReduceBool`.
> Every such statement is listed in §4 with its verbatim Lean type. Statements
> established by human argument are called **established** or **argued**, never
> proved, and each carries its weakest premise's tier (§1.3).

### 0.2 Delivery posture: staged, and why

This paper is **staged, not cleared**. Three gates stand between it and release, and
none of them has passed:

| gate | status | consequence for this paper |
|---|---|---|
| `evidence-gate` | **BLOCKED** (§8) | two live BLOCKER faults in the corpus's self-description |
| `citation-gate` | **NOT RUN** | no citation in this paper has been independently re-fetched and re-read by an auditor |
| `editorial-verdict` | **NOT RUN** | the adversarial review of this document is a separate downstream molecule |

§9 lists exactly what must close before this document can be released, and §10 lists
what it does not claim.

### 0.3 Attribution

External attribution for this work is **Noogram**. Firoozbakht's conjecture is
attributed to Faride Firoozbakht (1982); §11.3 records that no primary document by
Firoozbakht exists, and states what may and may not be written about that attribution.

---

## 1. The problem, and the shape of it

### 1.1 Statement

Let $p_n$ denote the $n$-th prime, **1-indexed**: $p_1 = 2$, $p_2 = 3$, $p_3 = 5$, ….
Write $g_n := p_{n+1} - p_n$ for the $n$-th prime gap and $L_n := \log p_n$ (natural
logarithm throughout).

> **Firoozbakht's conjecture (F).** For every $n \ge 1$,
> $$p_{n+1}^{\,1/(n+1)} \;<\; p_n^{\,1/n},$$
> equivalently: the sequence $\big(p_n^{1/n}\big)_{n \ge 1}$ is **strictly decreasing**.

The conjecture was proposed by Faride Firoozbakht in 1982 and remains open. Its earliest
public record is Rivera's *Prime Puzzles* Conjecture 30 (2002) [`rivera2002conj30`, V1];
its earliest citable print record is Ribenboim, *The Little Book of Bigger Primes*, 2nd
ed., p. 185 [`ribenboim2004little`, V2].

### 1.2 Five forms, all pointwise equivalent

The conjecture admits five natural phrasings. They are **pointwise equivalent** — index
by index, with no asymptotics and no exceptional set. This is an elementary but
load-bearing fact: it is what licenses proving in one form and stating in another.

| form | statement | ambient |
|---|---|---|
| **F1** | $p_{n+1}^{1/(n+1)} < p_n^{1/n}$ | $\mathbb{R}$, real power |
| **F2** | $p_{n+1}^{\,n} < p_n^{\,n+1}$ | $\mathbb{N}$, pure integer |
| **F3** | $n\log p_{n+1} < (n+1)\log p_n$ | $\mathbb{R}$ |
| **F4** | $a_{n+1} < a_n$, where $a_n := (\log p_n)/n$ | $\mathbb{R}$ |
| **F5** | $S_n > 0$, where $S_n := (n+1)\log p_n - n\log p_{n+1}$ | $\mathbb{R}$ |

*Proof of equivalence.* $p_n \ge 2 > 0$, so both sides of F1 are positive and $\log$ is
strictly increasing: F1 $\iff \tfrac{1}{n+1}\log p_{n+1} < \tfrac{1}{n}\log p_n$, and
multiplying by $n(n+1) > 0$ gives F3. F3 $\iff$ F2 because $\exp$ is strictly increasing
and $m^k = \exp(k \log m)$ for $m \ge 1$. F3 $\iff$ F4 by dividing by $n(n+1) > 0$.
F3 $\iff$ F5 is the definition of $S_n$. $\square$

The bridges F1 $\iff$ F2, F1 $\iff$ F3 and F1 $\iff$ (the gap form below) are **proved**
in the machine-checked sense of §0.1 — see §4.2.

### 1.3 The exact criterion: the conjecture *is* a statement about prime gaps

> **Lemma 1 (exact criterion).** For every $n \ge 1$,
> $$\mathrm{F}\text{ holds at } n \quad\Longleftrightarrow\quad g_n \;<\; f_n,
> \qquad f_n \;:=\; p_n^{\,1+1/n} - p_n \;=\; p_n\big(e^{L_n/n} - 1\big).$$

*Proof.* By §1.2, F at $n$ $\iff$ $n \log p_{n+1} < (n+1)\log p_n \iff \log p_{n+1} <
(1+\tfrac1n)L_n \iff p_{n+1} < p_n^{1+1/n}$; subtract $p_n$. Every step is an
equivalence. $\square$

**There is no side condition, no asymptotic regime, no exceptional small-$n$ set, and no
external explicit bound.** Lemma 1 is the whole content of the conjecture, index by
index. It is also **proved** in the machine-checked sense (`firoozbakht_iff_gap`, §4.2),
in the equivalent formulation $g_n < B_n$ with $B_n := p_n(p_n^{1/n}-1) = f_n$.

Call $\sigma_n := f_n - g_n$ the **shortfall** at $n$; F at $n$ $\iff$ $\sigma_n > 0$.

Two consequences organise everything that follows.

- **$f_n$ is strictly decreasing in $n$ for fixed $p_n$.** Hence *verifying* F needs an
  **upper** bound on the index $n = \pi(p_n)$, and *refuting* it needs a **lower** bound.
  Getting this backwards converts a verification into a refutation claim; it is the most
  common way to lose the thread in this subject.
- **Asymptotically $f_n = \log^2 p_n - \log p_n - 1 + o(1)$** [`kourbatov2015upper`
  Thm 5, V0]. So the fence sits at height about $(\log p)^2$.

### 1.4 The picture

The primes walk along a road. Every step is a gap. Firoozbakht drew a fence at height
roughly $(\log p)^2$ and said: **the primes never jump it — not once, not ever, all the
way to infinity.**

That picture makes both branches of the attack immediately legible.

- **To prove F**, one needs an upper bound on *every* prime gap, of size
  $(\log p)^2$-ish, with an explicit constant strictly below $1$. Nothing in analytic
  number theory produces a polylogarithmic gap bound at all. The best unconditional
  bound is $g_n \ll p_n^{0.525}$ [`bhp2001difference`, V2]; the Riemann hypothesis gives
  $g_n \ll \sqrt{p_n}\log p_n$. Those are **powers of $p$ against powers of $\log p$**.
  The distance is not quantitative — it is categorical.
- **To refute F**, one needs a single gap that clears the fence. At the record locus the
  gap reaches $92.06\%$ of $(\log p)^2$ while the fence sits at $97.06\%$ of the same
  quantity — a shortfall factor of $1.05426$ (§5.1). A few percent, in a quantity that
  took decades of distributed computation to measure.

The proof branch is blocked by a wall. The refutation branch is *close* but not
reachable. §7.4 records how this framing was itself demoted during the work.

### 1.5 Tiering convention

Two orthogonal ladders are used, and they are never conflated.

**Claim tier** — how well a *claim* is established: **L0** proved here from first
principles; **L1** standard/textbook; **L2** attributed with a verified locator; **L3**
heuristic, recalled, or resting on an unrefereed computational record; **[C]**
additionally recomputed by a verifier script in the run. A composite claim inherits its
**weakest** premise's tier.

**Citation tier** — how well a *citation* was checked: **V0** primary obtained and the
statement read verbatim at the named locator; **V1** primary's own metadata page read;
**V2** confirmed via a reliable secondary quoting the primary; **V3** bibliographic
metadata only, statement at locator **not** verified. Every citation in this paper is
tagged, and §11 traces each to its source-ledger row.

**A V3 locator is not usable.** Where this paper touches one, it says so and does not
lean on it.

---

## 2. Method

### 2.1 What was actually done

The attack ran as a directed polymer of specialised legs, each producing an artifact
another leg had to read. In rough order:

1. **Decomposition** — reduce the conjecture to its equivalent forms and to the gap
   criterion (§1.2, §1.3); enumerate attack routes.
2. **Frame deliberation** — a five-perspective panel adjudicating which branch is live.
3. **Source ledger** — build the bibliography as *(citekey, verification level, precise
   locator, exact statement as the source writes it)*, with independent recomputation of
   every numeric row.
4. **Concept cards** — 31 atomic definitions/lemmas, each carrying its ledger row.
5. **Lean skeleton, then Lean probe** — state the conjecture in Lean 4 / Mathlib and
   attempt real proof terms.
6. **Proof attempts** — three targets, attacked as mathematics: first-failure
   maximality, the RH-conditional route, and the unconditional verified range.
7. **Notebooks** — three computational legs, exhaustive and lever-based.
8. **Red-team corpus** — 19 adversarial artifacts designed to slip past a naive gate.
9. **Skeptic** — an adversarial audit that re-derived every load-bearing number in
   independent code and re-fetched five primary sources.
10. **Evidence gate** — fail-closed adjudication.
11. **Synthesis**, then this paper.

### 2.2 Method commitments

Four commitments constrain everything reported here.

- **Fail-closed gates.** A gate that cannot establish its predicate returns FAIL, never
  "probably fine". The evidence gate returned **BLOCKED**, and §8 reports it as such.
- **The kernel is the only source of the word "proved".** §0.1.
- **The `sorry` grep is not the audit.** A raw `grep -c sorry` over the statement file
  returns 6, of which exactly **one** is a genuine `sorry` in code — the rest are prose
  discussing what remains open. Verdicts rest on the `#print axioms` whitelist, which
  cannot be fooled by formatting.
- **Computation corroborates or refutes; it never proves.** No finite range bears on the
  conjecture. A certified range is a *fidelity instrument*, not evidence.

### 2.3 The formal environment

Lean 4 `v4.29.0` with Mathlib `v4.29.0` (revision pinned in the run's
`lake-manifest.json`). `lake build` completes with **exit status 0**, 8253 jobs. An
`Audit.lean` module is part of the library, so `#print axioms` runs on *every* build and
cannot silently rot. No `native_decide` appears anywhere in the sources, so
`Lean.ofReduceBool` never enters the trusted base: the finite verification is
kernel-checked, not compiler-trusted.

### 2.4 A named self-check on every result claim

The run's red-team corpus produced an artifact (`RT-12`) that builds with exit 0, is
`sorry`-free, is axiom-clean, **and proves a true theorem** — where the falsehood lives
in the sentence a paper would put next to it. That artifact imposes a standing
requirement on this document:

> **RT-12 requirement.** For every declaration this paper names as a result, print its
> Lean type and confirm the conclusion is not among its hypotheses.

§4.4 discharges that requirement for all fourteen declarations named in §4.

---

## 3. Results at a glance

| question | answer |
|---|---|
| Is the conjecture **proved**? | **No.** The Lean declaration `Firoozbakht.firoozbakht` still ends in `sorry`; its axiom line contains `sorryAx`. |
| Is the conjecture **refuted**? | **No.** No counterexample at any scale examined. |
| Was anything **proved** (kernel sense)? | **Yes** — 11 new theorems plus a 26-entry prime table, including the conjecture itself for all $1 \le n \le 25$ and two exact refutation criteria. §4. |
| Was anything **established on paper**? | **Yes** — §5. Two exact criteria, a verified-range theorem at $H = 10^{20}$ under two named premises, and three dead routes with quantitative obituaries. §6. |
| Evidence-gate status | **BLOCKED**. §8. |
| Citation clearance | **Not claimed, not audited.** §9. |

---

## 4. What was PROVED — machine-checked in Lean 4 / Mathlib

Everything in this section satisfies §0.1: `lake build` exit 0, and `#print axioms`
showing `[propext, Classical.choice, Quot.sound]` for each declaration.

Throughout, `nthPrime n` is the Lean 1-indexed $n$-th prime,
`noncomputable def nthPrime (n : ℕ) : ℕ := Nat.nth Nat.Prime (n - 1)`, and
`barrier n` is the exact barrier $B_n = p_n(p_n^{1/n}-1)$ of Lemma 1.

### 4.1 The headline: the conjecture, proved outright, for 25 consecutive indices

```lean
theorem firoozbakht_of_le (n : ℕ) (h1 : 1 ≤ n) (h2 : n ≤ 25) :
    (nthPrime (n + 1) : ℝ) ^ ((1 : ℝ) / ((n : ℝ) + 1)) <
      (nthPrime n : ℝ) ^ ((1 : ℝ) / (n : ℝ))
```

This is the **literal F1 form** — real powers, not the integer surrogate — and its
axiom line is clean:

```
'Firoozbakht.firoozbakht_of_le' depends on axioms: [propext, Classical.choice, Quot.sound]
```

so it is **not circular**: it does not depend on the `sorry`-ed conjecture. It is a
genuine unconditional fragment of an open problem, checked by the same kernel that
checks the bridges.

**Honest sizing, stated immediately and without softening.** The bound $n \le 25$ is set
by **compile time, not by mathematics** — the bridge lemma is uniform in $n$ and the
table extends mechanically. And $n \le 25$ is derisory next to the literature's
$4\times10^{18}$ [`kourbatov2015verification`] and $2^{64}$ [`visser2019verifying`].
**No finite range bears on the conjecture.** The value of this result is not its range;
it is that (a) the formal method now exists, and (b) a kernel-checked range is a
different kind of object from a script-checked one.

### 4.2 The enabling engineering: a computable bridge out of `Nat.nth`

`Nat.nth Nat.Prime` is `noncomputable` — defined by well-founded recursion over an
infinite set — so `decide` cannot evaluate it and **no finite range could be
machine-checked at all**. This was the formalisation's flagged main risk item.

The escape hatch is `Nat.nth_count : p n → Nat.nth p (Nat.count p n) = n`, which trades
`nth` for the computable `count`. The naive move works but is quadratic in the wrong
variable: a single check `Nat.count Nat.Prime 97 = 24` walks the whole range $[0,97)$ and
cost **~36 s** of kernel reduction as measured (one entry, the largest prime in the
table). That does not scale.

The leg instead proves a step lemma whose cost scales with the **gap**, not the prime:

```lean
theorem nthPrime_succ_eq {k p q : ℕ} (hk : 1 ≤ k) (hp : nthPrime k = p) (hq : Nat.Prime q)
    (hpq : p < q) (hgap : ∀ m, p < m → m < q → ¬ Nat.Prime m) :
    nthPrime (k + 1) = q
```

Each `hgap` obligation is an `interval_cases` over a gap of size $< 10$ in this range —
milliseconds. The whole file (both bridge lemmas, the 26-entry table, *and* the certified
range) compiles in **6.5–9.6 s**. **No numeral is asserted anywhere**: $p_1 = 2$ comes
from Mathlib, and every later entry is *derived* from its predecessor.

Also proved and used: `count_prime_eq_of_no_prime` (no prime in $[a,b)$ $\Rightarrow$
$\pi$ constant across it), `nthPrime_succ_le_of_prime` (minimality), and the equivalence
bridges `firoozbakht_iff_log`, `firoozbakht_iff_int`, `firoozbakht_iff_gap` — the
machine-checked form of §1.2 and Lemma 1.

### 4.3 Both directional criteria, exact

**Sufficient (verification side).**

```lean
theorem firoozbakht_of_gap_lt {n : ℕ} (hn : 1 ≤ n)
    (h : (n : ℝ) * ((nthPrime (n + 1) : ℝ) - (nthPrime n : ℝ))
        < (nthPrime n : ℝ) * Real.log (nthPrime n)) :
    (nthPrime (n + 1) : ℝ) ^ ((1 : ℝ) / ((n : ℝ) + 1)) <
      (nthPrime n : ℝ) ^ ((1 : ℝ) / (n : ℝ))
```

i.e. $n g_n < p_n \log p_n \Rightarrow \mathrm{F}$ at $n$. The proof is the single
estimate $\log(p_{n+1}/p_n) \le p_{n+1}/p_n - 1$, which is exactly where the exact
barrier degrades to its first-order surrogate. The hypothesis is therefore *strictly
stronger* than the conjecture at $n$: **sufficient, never necessary.**

**Necessary conditions for a refutation — two exact criteria.**

```lean
theorem not_firoozbakht_of_barrier_le {n : ℕ} (hn : 1 ≤ n)
    (h : barrier n ≤ ((nthPrime (n + 1) : ℝ) - (nthPrime n : ℝ))) :
    ¬ ((nthPrime (n + 1) : ℝ) ^ ((1 : ℝ) / ((n : ℝ) + 1)) <
        (nthPrime n : ℝ) ^ ((1 : ℝ) / (n : ℝ)))

theorem not_firoozbakht_of_pow_le {n : ℕ} (hn : 1 ≤ n)
    (h : (nthPrime n) ^ (n + 1) ≤ (nthPrime (n + 1)) ^ n) :
    ¬ ((nthPrime (n + 1) : ℝ) ^ ((1 : ℝ) / ((n : ℝ) + 1)) <
        (nthPrime n : ℝ) ^ ((1 : ℝ) / (n : ℝ)))
```

The second is **pure $\mathbb{N}$ arithmetic** — no real arithmetic, no `Real.log` — and
is therefore the right target for a computational search. Note what it demands: the
**index** $n = \pi(p_n)$ must be certified, not merely the gap. *A large gap alone is
not a counterexample.*

**Explicitly not admissible as a refutation**: exhibiting a gap above
$\log^2 p_n - \log p_n - 1$. That is the asymptotic surrogate, not $B_n$, and the
difference is $O(1/\log p_n)$ — which is exactly the size of the margin being contested.

### 4.4 RT-12 discharge — hypothesis/conclusion separation

Per §2.4, for every declaration this paper names as a result:

| declaration | hypotheses | conclusion | conclusion among hypotheses? |
|---|---|---|---|
| `firoozbakht_of_le` | $1\le n$, $n\le 25$ | F1 at $n$ | **no** |
| `firoozbakht_gap_of_le` | $1\le n$, $n\le 25$ | $g_n < B_n$ | **no** |
| `nthPrime_succ_eq` | $1\le k$, `nthPrime k = p`, `q` prime, $p<q$, no prime strictly between | `nthPrime (k+1) = q` | **no** |
| `nthPrime_eq_1 … _26` | none | $p_i = $ numeral | **no** |
| `count_prime_eq_of_no_prime` | $a\le b$, no prime in $[a,b)$ | $\pi$ constant | **no** |
| `nthPrime_succ_le_of_prime` | $1\le n$, `q` prime, $p_n<q$ | $p_{n+1}\le q$ | **no** |
| `firoozbakht_iff_log` | $1\le n$ | F1 $\iff$ F3 | **no** (biconditional bridge) |
| `firoozbakht_iff_int` | $1\le n$ | F1 $\iff$ F2 | **no** |
| `firoozbakht_iff_gap` | $1\le n$ | F1 $\iff$ $g_n<B_n$ | **no** |
| `firoozbakht_of_gap_lt` | $1\le n$, $n g_n < p_n L_n$ | F1 at $n$ | **no** |
| `not_firoozbakht_of_barrier_le` | $1\le n$, $B_n \le g_n$ | $\neg$F1 at $n$ | **no** |
| `not_firoozbakht_of_pow_le` | $1\le n$, $p_n^{n+1}\le p_{n+1}^n$ | $\neg$F1 at $n$ | **no** |
| `nthPrime_succ_lt_two_mul` | $1\le n$ | $p_{n+1}<2p_n$ | **no** |
| `firoozbakht_of_two_pow_le` | $1\le n$, $2^n \le p_n$ | F1 at $n$ | **no** |

No declaration in this table takes Firoozbakht's conjecture — in any of its five forms —
as a hypothesis. The two refutation criteria are genuine contrapositives; this was
independently confirmed inside the kernel by typechecking

```lean
example (n : ℕ) (h1 : 1 ≤ n) (h2 : n ≤ 25)
    (hbad : (nthPrime n) ^ (n + 1) ≤ (nthPrime (n + 1)) ^ n) : False :=
  not_firoozbakht_of_pow_le h1 hbad (firoozbakht_of_le n h1 h2)
```

which wires the refutation criterion and the certified range together and shows the
kernel agrees they are incompatible.

### 4.5 An external cross-check on the numerals

Lean checks that the proofs follow. It does not check that the *numerals* are the primes
anyone means. Against an independent `sympy` sieve:

```
prime table p_1..p_26 matches sympy sieve: True
n in 1..25 failing p_{n+1}^n < p_n^(n+1): []
n with 2^n <= p_n (n < 40): [1]
n < 72849 where linearised criterion n*g_n < p_n*log p_n FAILS: count=2 first=[2, 4]
```

The third line is a genuinely useful fact: over the first ~73 000 indices the linearised
sufficient criterion fails at only $n = 2$ and $n = 4$, **both inside the range already
proved outright** by `firoozbakht_of_le`. So `firoozbakht_of_gap_lt` is not a lossy
detour — and equally, it is **no easier than the target**.

---

## 5. What was ESTABLISHED on paper

These are arguments, not certificates. Each carries its tier per §1.5.

### 5.1 The record locus, and the size of the margin

At the largest known maximal-gap locus [`kourbatov2015upper` Table 1, last row, **V0**]:

$$k = 49\,749\,629\,143\,526, \qquad p_k = 1\,693\,182\,318\,746\,371, \qquad g_k = 1132.$$

Recomputed here in 60-digit arithmetic from exact integer inputs (**[C]**):

| quantity | recomputed | published | source |
|---|---|---|---|
| $\log p_k$ | $35.0653861820133348\ldots$ | — | — |
| $f_k = p_k^{1+1/k}-p_k$ | $1193.41777829404\ldots$ | $1193.418$ | `kourbatov2015upper` Table 1 ✓ |
| $\ell_k = \log^2 p_k-\log p_k$ | $1194.51592191172\ldots$ | $1194.516$ | `kourbatov2015upper` Table 1 ✓ |
| $R_{\mathrm{csg}} = g_k/\log^2 p_k$ | $0.9206385885574205\ldots$ | $0.9206$ | `primegaplist_faq` ✓ |
| $f_k/\log^2 p_k$ (the fence, same normalisation) | $0.9705887446713\ldots$ | — | **[C]** |
| **shortfall $f_k/g_k$** | $\mathbf{1.05425598789\ldots}$ | — | **[C]** |

**Use $1.05426$**, sourced directly to the *published exact barrier* $f_k = 1193.418$.
Not $1.055$ (an earlier figure in this run, superseded), and not via any $c_n$ interval —
the exact barrier makes the explicit-bounds detour unnecessary. If an explicit-bounds
route is wanted anyway it certifies $1.05424 \pm 0.00005$ (Dusart 2010 Props. 6.6/6.7,
**V0**, validity $k \ge 688\,383$) — consistent, with the exact value inside it.

Tier: **L0/L1 + V0 + [C]**.

**A caveat that is real, not decorative.** The statement "the CSG ratio
$R = g/\log^2 p$ has never exceeded 1" is **false as written**. It requires the
restriction $p > 7$. Recomputed here: $R > 1$ at exactly three points — $p=2$
($R = 2.081369$), $p=3$ ($1.657071$), $p=7$ ($1.056366$) — and $R \le 1$ for every other
$p \le 113$. The community source states the record *with* the restriction
("the largest value observed for any $p_1 > 7$"); any restatement must carry it.

### 5.2 Theorem A — an unconditional verified range at $H = 10^{20}$

> **Theorem A (established, not proved).** F holds for every $n$ with $p_n < 10^{20}$.

The argument is complete and every step is either first-principles or one of exactly two
external premises, both named and isolated:

| premise | supplies | tier |
|---|---|---|
| **P1** — Axler, *New bounds for the prime counting function*, the $3.83$ member: $\pi(x) < x\big/\big(\log x - 1 - \tfrac1{\log x} - \tfrac{3.83}{\log^2 x}\big)$ for $x \ge 9.25$ | the analytic criterion | **V0** · **L2** — see the locator fault below |
| **P2** — the census of first-occurrence prime gaps is complete below $10^{20}$, yielding the 85 known maximal gaps | the range | **V1** [`primegaplist_faq`] · **L3**: a community computational record, unrefereed, not machine-checked |

Everything else — the criterion, its monotonicity, the covering lemma, the base case,
the assembly — is L0. By the propagation rule the conclusion inherits its weakest
premise: **Theorem A is L3/V1, and its weak link is computational data, not
mathematics.**

The structural device is a **lever**. If the maximal-gap table is complete below $H$,
with records $(P_i, G_i)$, then for $P_i \le p_n < P_{i+1}$ we have $g_n \le G_i$ — a
larger gap would be an unrecorded new record. Combined with a lower bound on $B_n$ (from
an *upper* bound on $\pi$, per §1.3), this turns ~80 inequalities into a verified range
of $\approx 2.2\times10^{18}$ primes: **$2.7\times10^{16}$ primes per inequality**
[COMPUTED]. That ratio *is* the content of the phrase "verified up to $4\times10^{18}$".
Nobody has evaluated the criterion at $10^{17}$ consecutive primes, and nobody will.

Two subsidiary findings:

- **The range is data-bound, not method-bound.** At the tightest of the 85 records the
  criterion is satisfied with $5.1\%$ to spare [C]; $H$ stops exactly where the census
  stops. The gain from $2^{64}$ (2019) to $10^{20}$ (today) is **not mathematical** —
  the same argument gives both; the census moved.
- **The relaxation costs nothing.** Using the unconditional criterion instead of the
  exact barrier loses $9.3\times10^{-6}$ relative [C]. This is why the target closes at
  all.

**A live citation fault on P1 — MAJOR-6, stated here rather than buried.** The Axler
result is cited throughout this corpus as **"Corollary 3.5"**, which is the **arXiv v3**
numbering, against a bibliographic entry whose journal-of-record fields are *Integers*
**16** (2016), A22. The published corrigendum numbers the affected result **Corollary
3.4, page 8**. A referee checking the version of record will **not** find the $3.83$
bound at "Corollary 3.5". The premise is *believed* — the skeptic leg retrieved arXiv
v3 and read the four-member family verbatim, confirming P1 at V0 — but the locator does
not resolve in the version it is attributed to, and the corpus never establishes where
the $3.83$ member sits in the published numbering. **Theorem A must not be published
until this locator is repaired.**

### 5.3 The exhaustive floor the project owns outright

Independently of any external premise: F verified exhaustively for all $p < 10^9$ —
**50 847 533 consecutive pairs** — by sieve plus a *runtime-certified* floating-point
error bound, in 9 s. Tier **L0 + [C]**. This is the only part of the range this work owns
without inheriting anyone's computational claim. Below $p = 1223$ the check is exact
integer comparison of $p_{n+1}^n$ against $p_n^{n+1}$, with no arithmetic model at all.

Within that same floor, **FFM** — *if F first fails at $n^\star$, then $g_{n^\star}$ is a
record gap* — is **certified, not assumed**, for all $n \le 50\,847\,533$.

**A vacuity note, applied here rather than inherited.** On any range where F is
*verified*, FFM is **vacuously true** and carries zero information: its antecedent is the
negation of F. This paper therefore does **not** advertise "FFM proved on
$[89, 10^{20})$" as a deliverable, because on exactly that range F is verified from
exactly those premises. What is non-vacuous is the **certificate** $G_{n-1} < f_n$ on
$[89, H)$ for every $H$ to which the census is complete — strictly stronger than
FFM-at-$n$ and not implied by F's verification. The general FFM statement, for unbounded
$n$, remains **open**.

### 5.4 The FFM dichotomy

The real deliverable of the FFM target is a structural theorem, not a range:

> **Theorem B (established).** Every known route to proving FFM passes either through F
> itself or through a statement strictly stronger than F.

Two elementary facts drive it. (a) F $\Rightarrow$ FFM vacuously. (b) Hence any
*refutation* of FFM contains a refutation of F — to exhibit a counterexample to FFM one
must exhibit the least index at which F fails, and in particular an index at which F
fails. So FFM is *logically weaker* than F, **and the weakness buys nothing**.

Tier **L0**.

---

## 6. What was REFUTED

Nothing about the conjecture. **Three routes** died, and the obituaries are quantitative.

### 6.1 The Riemann hypothesis route — dead, with numbers

> **Theorem C (established).** The strongest *published* RH-conditional prime-gap bound
> (Carneiro–Milinovich–Soundararajan 2019, constant $22/25$) decides Firoozbakht at
> **exactly three indices**, $n \in \{1,2,3\}$ — and at **exactly one**, $n = 3$
> ($p = 5$), once the source's own validity restriction $p_n > 3$ is honoured. It is
> silent at every $n \ge 4$, forever.

> **Theorem D (established).** This is not a constant problem. For **every** $c > 0$, a
> bound $g_n \le c\sqrt{p_n}\log p_n$ decides F on a finite initial segment only.
> Improving the constant moves a threshold; it never changes the verdict.

> **Theorem E (established).** The RH branch is **strictly dominated**. For every
> $c \ge 10^{-7}$ — seven orders of magnitude below the published constant, and six below
> $c = 1/2$, which the CMS authors name as the limit of their method — the usable set
> ends *inside* the range already verified **unconditionally**. RH contributes **zero
> indices** that unconditional verification does not already own.

Tier **L0 + [C]** for all three, resting on a **V0** reading of the CMS paper (but see
the ledger caveat in §11.2).

The $\sqrt{x}$ shape is not laziness. Littlewood's unconditional $\Omega_\pm$ theorem
forces any argument that bounds the prime-counting error pointwise and subtracts to work
at that scale **regardless of RH**. (This explanatory proposition rests on a **V3** row
for Littlewood 1914 and is stated as such; Theorems C–E do not use it.)

**What was deliberately *not* proved**: that RH does not *imply* Firoozbakht. That is a
claim about RH's deductive closure and nothing here establishes it. The barrier proved is
about the **shape of the bounds the known machinery emits**, over an explicitly
enumerated surveyed set. That restraint is deliberate and is preserved in this paper.

**One incidental positive.** RH *does* have a use in this attack — on the **refutation**
branch, certifying the index $n = \pi(p)$. It is on the wrong branch of the tree, and the
unconditional bound was already comfortable there by a factor of ~1600.

### 6.2 The monotone-barrier route to FFM — dead, by an exact witness

The tempting proof of FFM routes through monotonicity of the exact barrier $f_n$. **$f_n$
is not monotone**: it decreases at $57.88\%$ of steps and sits below its own running
maximum at $87.88\%$ of indices, with an **exact-integer witness at $n = 7, 8$**
($f_7 - f_8 = 0.0281326864429$, reproduced independently in 40-digit arithmetic).

The error is subtle and worth recording precisely: the **proxy** barrier
$\ell_n = \log^2 p_n - \log p_n$ **is** monotone in the index; $f_n$ is not. Substituting
one for the other is the mistake, and it is what makes FFM "the price of exactness".

Tier **L0 + [C]**.

### 6.3 Unconditional analytic extension of the verified range — dead permanently

> **Theorem F (established).** No unconditional analytic result in the literature extends
> the verified range by a single prime, and none can: the best proven gap bound
> overshoots the barrier by a factor that **diverges** — $1.5\times10^{7}$ at $10^{20}$,
> $6\times10^{47}$ at $10^{100}$.

The range is **data-bound, not method-bound**. Tier **L0 + [C]**.

---

## 7. Computational evidence

Every number here is reproducible from the run directory; no RNG, no network.

### 7.1 What was computed, and on what tier

| range | certified by | tier |
|---|---|---|
| $[2,\,1223]$ | exact integer comparison $p_{n+1}^n < p_n^{n+1}$ | **L0** — no arithmetic model at all |
| $[2,\,10^9)$ | exhaustive sieve + runtime-certified error bound; 50 847 533 pairs, 9 s | **L0** — owned outright |
| $[89,\,4\times10^{18})$ | lever (gap table + Axler) | **L2**, 71 inequalities |
| $[89,\,2^{64})$ | same lever, longer table | **L2**, 76 inequalities |
| $[89,\,10^{20})$ | same lever, full b-file | **L2/L3** (community census), 80 inequalities |
| $[89,\,1.857\times10^{19})$ | second, independent lever needing no Axler bound | **L2**, 79 inequalities |
| beyond $10^{20}$ | nothing | open |

**No counterexample was found at any scale examined.** No leg expected to find one, and
none of these ranges is evidence about the conjecture.

### 7.2 Independent reproduction

An adversarial audit leg re-derived the load-bearing numbers in code written
independently of the producing legs. Selected results:

- Re-sieved to $2\times10^{7}$ in independent NumPy: $\min H_n = 1.196152422706632$ at
  $n=2$; $\max D_n = 0.5486599111467569$ at $n=214$ ($p=1307$) — **exact matches**.
- Recomputed the record-locus bracket: $1193.345367 \le 1193.417778 \le 1193.459723$ —
  **reproduces to every quoted digit**.
- Re-ran the 85-row lever: record #4 fails ($\underline{B}/G = 0.823$), #5 onward passes,
  tightest ratio $1.054246$ at #64 — **matches**.
- Cross-checked two independently sourced maximal-gap tables (Wikipedia-sourced against
  OEIS b-files): **85/85 rows identical** in $G$, $P$ and $\pi(P)$. *No producing leg had
  performed this cross-check.*
- Tested the unconditional criterion empirically at every index to $2\times10^{7}$:
  **0 violations**.
- Re-ran all seven verifier scripts: **all exit 0**, four producing byte-identical output.

### 7.3 A genuinely open computational obligation

Monotonicity of the lower barrier bound $\underline{B}$ is used and **not proved**. It
was tested on a grid ~500× denser than any producing leg used — 300 001 points over
$[10, 10^{22}]$ — with **0 non-monotone steps**. The gap remains a gap. Recorded as an
open obligation, not as a result.

### 7.4 A correction to this work's own opening framing

The attack opened with "the refutation branch is the live branch; the shortfall is
$5.5\%$". That framing was **partially demoted** during the run, and this paper carries
the demotion rather than the headline:

- the shortfall figure is arithmetically right (corrected: $1.05426$, §5.1) but is
  **not L0** as originally derived — that derivation traversed an unclosed literature
  node; it is L0 *now*, via the published exact barrier;
- the sufficient criterion the headline leans on is **silent at $n = 2$ and $n = 4$** —
  the *tightest known cases* of the conjecture. F is true there by the exact predicate
  ($25 < 27$; $14641 < 16807$), but the criterion does not know it;
- of two **pre-registered** demotion criteria, one fired and one did not. The naive
  probabilistic model predicts $6.72$ counterexamples below the verified frontier where
  $0$ are observed — over-predicting by $6.7\times$; the fitted crossover height is
  $10^{30}$–$10^{51}$, not $10^{21}$.

**The honest reading**: the refutation branch keeps its heuristic motivation — Granville's
$\limsup_{x} \max_{p_n \le x}(p_{n+1}-p_n)/\log^2 x \gtrsim 2e^{-\gamma} \approx 1.12292$
is not excluded, and this $\limsup$ is genuinely unknown — and **loses its quantitative
one**. Search is not affordable at any of the fitted heights.

The crossover-height figure itself carries a live caveat: it is a **30-decade
extrapolation from a model fit**, and the model tag did not survive into the decision
that used it. It is reported here as a model output, never as a property of the primes.

---

## 8. Evidence gate: BLOCKED

The gate is fail-closed and it closed. This section reports the verdict as the gate
returned it.

| leg | requirement | verdict |
|---|---|---|
| **LOOP** | verdict file present, well-formed, names the live round | **PASS** — live round = 1 |
| **KERNEL** | `lake build` exit 0 **and** grep-clean of `sorry`/`axiom` | **FAIL** — exit 0 ✅ but one live `sorry`; 7 declarations carry `sorryAx` |
| **SKEPTIC** | fault report exists **and** zero residual BLOCKERs | **FAIL** — 2 live BLOCKERs |
| **CORPUS** | red-team corpus present **and** coverage report non-empty | **PASS** — 19 entries / 15 categories |

**FAIL, not DEGRADED.** DEGRADED is available only when no formal backend was attempted.
Lean *was* run, *did* build, and *did not* discharge the `sorry`. Calling that DEGRADED
would convert "we tried and the conjecture is still open" into "we could not try" — a
different and false statement.

### 8.1 The two live BLOCKERs

The audit found **no mathematical error in any theorem**. Both BLOCKERs are claims the
corpus makes *about its own trustworthiness*, and both are false. That is exactly the
class of fault a gate exists to stop.

| id | fault |
|---|---|
| **BLOCKER-1** | A provenance record states that 44 rows of the load-bearing maximal-gap table were independently recomputed. **Thirty were.** A 47% overstatement, sitting in the provenance record of the corpus's single weakest premise (P2 of Theorem A). |
| **BLOCKER-2** | The claim *"the verified range itself excludes $C \ge 1.1736$"* is an **invalid inference**: a finite range cannot exclude a value of a $\limsup$. It is a property of a model fit, read as a statement about the tail. |

Neither needs new mathematics. One is a wrong row count; one is a sentence that must be
deleted or restated. **This paper does not rely on either claim**: §7.2 reports only
counts the audit itself verified, and §7.4 states the $\limsup$ as unknown.

### 8.2 Seven MAJOR faults, carried forward

Dominated by citation and cross-leg consistency: a locator absent from the version of
record (**MAJOR-6**, disclosed in §5.2); two mutually incompatible verification tiers for
the same source (**MAJOR-7**); one leg crediting a sibling with a premise the sibling had
retracted (**MAJOR-4**); a vacuous conclusion sold as a headline verdict (**MAJOR-3**,
corrected in §5.3); a docstring carrying a formally retracted claim (**MAJOR-5**); an
undated superlative survey claim inside a V0 row (**MAJOR-8**, see §11.2); and the
30-decade extrapolation feeding a budget decision (**MAJOR-9**, caveated in §7.4).

Of these, **MAJOR-3 and MAJOR-9 are corrected in this paper**; **MAJOR-6 and MAJOR-8 are
disclosed at the point of use**; **MAJOR-4, MAJOR-5 and MAJOR-7 concern upstream
artifacts this paper does not rely on.**

---

## 9. Limitations — stated in full, not summarised

1. **The conjecture is open.** It is not proved and not refuted here. Nothing in this
   paper is evidence about its truth.
2. **No finite range is evidence.** Neither the kernel-checked $n \le 25$, nor the
   exhaustive $p < 10^9$, nor Theorem A's $10^{20}$. The whole difficulty is in the tail.
3. **Theorem A inherits an unrefereed computational premise (P2, L3/V1)** and a
   **faulted citation locator (P1, MAJOR-6)**. It should be read as *medium* confidence,
   with the weak link on the data side.
4. **The citation audit has not run.** No citation here has been independently re-fetched
   and re-read by an auditor. §11 traces each to a ledger row; that is provenance, not
   clearance.
5. **Six citations used in §6.1 sit on ledger rows that were proposed but never merged**
   into the authoritative ledger. §11.2 lists them explicitly.
6. **Three named gaps remain unsourced or unusable**: the four-term expansion
   $B_n = L^2-L-1-3/L-13/L^2+O(L^{-3})$ is **unsourced** — the published theorem gives
   only the $+o(1)$ form, and this paper therefore does **not** use the lower-order terms;
   the Baker–Harman–Pintz threshold $x_0$ is **ineffective as published**, so $0.525$
   must never be quoted as an explicit unconditional bound without that caveat; and no
   live URL exists for the canonical Nicely gap tables (all return HTTP 404 as of
   2026-07-24) — cite the community project instead.
7. **The monotonicity of $\underline{B}$ (§7.3) is used and unproved.**
8. **Only one attack round ran** (a configured cap, not a failure — the executor was
   verified capable). There is therefore no shrink-versus-churn signal: the list of
   `sorry`-carrying declarations entered at 7 and left at 7, **identical, not churned**.
   The six corollaries were never independent targets — they inherit from the head.
9. **`lake build` was not re-executed by this leg.** The build record (exit 0, 8253 jobs)
   is read as the kernel leg's own transcript.
10. **The delivery posture is staged.** §0.2.

### 9.1 What must close before release

In the order the evidence imposes:

1. Close **BLOCKER-1** (correct the row count) and **BLOCKER-2** (delete or restate the
   invalid inference), and unwind any decision resting on the latter.
2. Repair the **MAJOR-6** locator: establish where the $3.83$ member sits in the
   *published* Axler numbering, or re-attribute P1 to arXiv v3 explicitly in every place
   it appears.
3. Merge or drop the six **proposed-but-unmerged ledger rows** of §11.2. Theorems C–E
   cannot ship on unmerged rows.
4. Run **`citation-gate`**, then **`editorial-verdict`**.

Steps 1–4 are corpus-honesty and citation work, not mathematics. **None of them will
close the kernel leg**: that closes only if the conjecture is proved, which is the open
problem itself.

### 9.2 The honest forecast

More rounds of the same shape would very likely keep producing scaffolding without moving
the head. The obstruction is not a missing lemma that a re-read would surface — it is the
**categorical gap between polylogarithmic and power-of-$p$ gap bounds** (§1.4). If
further rounds are run, their value is in closing the corpus faults and hardening the
criteria, **not** in expecting the `sorry` to fall.

---

## 10. What this paper does not claim

- It does not claim the conjecture is true, false, likely, or unlikely. The community's
  heuristic expectation (from Granville's revised Cramér model) is that it is **false**;
  that is recorded as a heuristic and is used as a premise nowhere.
- It does not claim citation clearance (§9, limitation 4; §11).
- It does not claim that RH fails to imply Firoozbakht (§6.1).
- It does not claim any verified range is evidence.
- It does not claim FFM on any range as a substantive result (§5.3).
- It does not use the unsourced lower-order terms of the $B_n$ expansion.
- It does not claim "proved" for anything outside §4.

---

## 11. Citation trace

Every citation in this paper resolves to a row in the run's authoritative source ledger,
`source-ledger/source-ledger.md`. Section numbers below are that document's.

### 11.1 Merged ledger rows used in this paper

| citekey | ledger § | V | locator relied on | used in |
|---|---|---|---|---|
| `firoozbakht1982` | §3.1 | V1 | attribution + date only | §1.1, §0.3 |
| `rivera2002conj30` | §3.1 | V1 | page body | §1.1 |
| `ribenboim2004little` | §3.1 | V2 | p. 185 | §1.1 |
| `visser2019verifying` | §3.1, §3.2, §3.8 | **V0** | Conj. 3 eq. (2.4); Abstract; §4 | §1.3, §4.1, §7.1 |
| `kourbatov2015verification` | §3.2 | V1 | Abstract | §4.1, §7.1 |
| `kourbatov2015upper` | §3.3, §3.4 | **V0 + C** | Table 1 last row; Thm 5; §3 | §1.3, §5.1 |
| `kourbatov2015corrigendum` | §3.4 | **V0** | full text | §11.3 |
| `primegaplist_faq` | §3.2, §3.3 | V1 + C | FAQ body | §5.1, §5.2, §7.1 |
| `axler2014newbounds` | §3.5 | **V0** | Cor. 3.5 (arXiv v3) — **see MAJOR-6** | §5.2 |
| `axler2016corrigendum` | §3.5 | V2 | via Kourbatov corrigendum | §5.2, §11.3 |
| `dusart2010estimates` | §3.5 | **V0 + C** | Props. 6.6, 6.7 | §5.1 |
| `granville1995harald` | §3.6 | **V0** | display following eq. (20); eq. (14) | §7.4 |
| `cramer1936order` | §3.6 | V2 | block quotation in Granville | §11.3 |
| `bhp2001difference` | §3.6 | V2 | main theorem | §1.4; §9, limitation 6 |
| `fgkmt2018long` | §3.7 | V1 | Abstract | §11.3 |

### 11.2 Proposed but **unmerged** rows — a live obligation

The RH-route results of §6.1 rest on citations that a downstream leg **proposed** for the
ledger and that were **never merged** into it. They are recorded here with the
verification level the proposing leg claimed, and they are **not cleared**:

| citekey | claimed V | statement relied on | status |
|---|---|---|---|
| `cms2019fourier` — Carneiro, Milinovich, Soundararajan, *Fourier optimization and prime gaps*, Comment. Math. Helv. **94** (2019), 533–568; arXiv:1708.04122 | **V0** (claimed; quotations independently re-fetched and confirmed exact by the audit leg) | on RH, $g_n \le \tfrac{22}{25}\sqrt{p_n}\log p_n$ for $p_n > 3$; the method's limiting constant is $\tfrac12$ | **UNMERGED** |
| `littlewood1914` | **V3** | $\psi(x)-x = \Omega_\pm(\sqrt x \log\log\log x)$ | **UNMERGED + V3** — used only for the explanatory remark in §6.1, which is flagged as such |
| `dudek2015riemann` | V1 | earlier RH-conditional constant | **UNMERGED**, lineage only |
| `leenosal2025sharper` | **V0** | sharper RH error bounds | **UNMERGED**, not load-bearing here |
| `schoenfeld1976sharper` | V3 → V2 (tier disputed, **MAJOR-7**) | classical RH bound | **UNMERGED**, **not used in this paper** |
| `montgomery1973pair` | V3 | pair-correlation consequence | **UNMERGED**, **not used in this paper** |

**Consequence, stated plainly.** Theorems C, D and E of §6.1 are **established**
mathematics resting on a citation that is not yet in the authoritative ledger. They must
not be released until `cms2019fourier` is merged at V0 and `littlewood1914` is either
promoted or the explanatory remark is restated as conditional on it. This is item 3 of
§9.1.

A further caveat on the same family (**MAJOR-8**): the phrase *"the strongest published
RH-conditional gap bound"* is a **superlative survey claim**, and no leg documents a
literature sweep after 2019 — one was attempted and blocked by rate limiting. §6.1
therefore says "the strongest *published* bound" as the producing leg's assessment,
not as a verified fact about the literature as of 2026.

### 11.3 Citation hygiene notes that must survive into any restatement

Five errors are easy to make in this literature, and the ledger pins each:

1. **The $b = 1.17$ sufficient condition has a corrected validity range.** Kourbatov's
   Theorem 3 originally rested on Axler's bound at "$x \ge 5.43$"; Axler's own corrigendum
   moved that to $x \ge 2\,634\,800\,823$, and Kourbatov published a corrigendum
   rewriting the proof. Any citation of "$b = 1.17 \Rightarrow$ Firoozbakht" that does not
   carry $p_k \ge 2\,634\,800\,823$ **plus** the unconditional check on
   $[29,\, 2\,634\,800\,823]$ is citing a withdrawn proof. Cite arXiv:1506.03042**v4**.
2. **The verification frontier is $2^{64}$, not $4\times10^{18}$.** $4\times10^{18}$ is
   Kourbatov's 2015 exhaustive-sieve range; Visser 2019 covers $p < 2^{64} \approx
   1.844\times10^{19}$, for Firoozbakht *and* the two strictly stronger Nicholson and
   Farhadian conjectures. The community census reaches $10^{20}$ (§5.2, P2).
3. **Explicit bounds here are Axler's, not Rosser–Schoenfeld's or Dusart's.** Kourbatov's
   chain uses Axler. Writing "Rosser–Schoenfeld / Dusart" while reproducing that
   derivation is mis-citing. (Dusart 2010 *is* used, at V0, but for the $p_k$ bracket of
   §5.1 — a different role.)
4. **The long-gaps exponent is $(\log\log\log X)^{1}$, not squared.** The 2018 record
   improves the 2016 one by exactly one iterated log in that denominator. This is the
   easiest citation error in the family. Either way the bound is $\ll \log^2 X$ — too
   small by a factor $\sim \log X/\log\log X$ to touch Firoozbakht.
5. **Cramér stated the $\limsup = 1$ relation about his random model, and only
   *suggested* the transfer to the primes.** Any sentence beginning "Cramér conjectured"
   must carry that hedge. And "Cramér 1920/21" is a mis-citation: the RH gap bound is
   Cramér 1920, with a distinct companion paper the same year.

Also recorded as **absences**, which is part of the provenance: there is **no primary
document by Firoozbakht** (the standard reference is literally "*(1982), unpublished*"),
so "Firoozbakht proposed, in 1982" is supportable and "Firoozbakht proved/published" is
not; the canonical Nicely gap tables are **dead links**; and **no proof, disproof, or
accepted counterexample of the conjecture was found anywhere in the literature.**

The bibliography accompanying this paper is `references.bib` (31 merged citekeys, plus a
clearly separated addendum block for §11.2's unmerged rows).

---

## 12. Reproduction

All artifacts of the run live under the run directory. The load-bearing checks:

```
# formal layer
cd <run>/lean-probe/lean && lake build          # exit 0; one `sorry` warning, at Statement.lean
                                                # Audit.lean prints axioms for 31 declarations

# recomputation layer (each deterministic, no RNG, no network)
python3 <run>/source-ledger/verify_ledger.py    # ledger rows
python3 <run>/proof-attempt__0/verify_pa0.py    # FFM numerics
python3 <run>/proof-attempt__1/verify_rh.py     # RH usable sets
python3 <run>/proof-attempt__2/verify_pa2.py    # the 85-row lever
python3 <run>/notebooks__2/verify-findings.py   # the verified-range lever
python3 <run>/concept-cards/verify_cards.py     # concept-card invariants
python3 <run>/skeptic/verify_faults.py          # the audit's own re-derivations
python3 <run>/synthesize/verify_synthesis.py    # 56/56 checks, exit 0
```

The synthesis verifier is deliberately a **tripwire**: it asserts that the two BLOCKERs
are still live. If a future session closes them, those checks **fail** — which is the
intended behaviour, not a regression.

---

## 13. The one-paragraph version

The primes walk along a road, and every step is a gap. Firoozbakht drew a fence at height
roughly $(\log p)^2$ and said the primes never jump it — not once, not ever, all the way
to infinity. This work did not decide that. What it did was survey the ground carefully.
It showed the fence is *exactly* the right description of the conjecture, not an
approximation. It proved — inside a kernel, in the literal form — that the primes stay
under it for the first 25 steps, and established by argument that they do so for every
prime below $10^{20}$, resting on two named external premises. It showed that the Riemann
hypothesis, the biggest hammer anyone would reach for, settles the question for the prime
5 and for no other. It showed that no unconditional method in the literature can extend
the verified range by a single prime, and that the shortfall **diverges** rather than
closing. And it built the two exact criteria that a future proof and a future refutation
would each need. Then its own auditor found two places where the corpus was overstating
how carefully it had checked itself, and the gate closed red on that. **The mathematics
in this corpus is sound. Its self-description was not, in two places. The conjecture is
still open.**

---

*Emitted by the `write-paper` leg (`edit-20260724-9e06`) of the `math-attack` polymer,
run node. External attribution: **Noogram**. Firoozbakht's conjecture
is **open** — not proved, not refuted. Evidence-gate status: **BLOCKED**. Citation
clearance: **not claimed, not audited**. Delivery posture: **staged**.*
