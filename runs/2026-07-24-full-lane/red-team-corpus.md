# Red-team corpus — coverage report

**Leg**: `red-team-corpus` (adversarial branch of the `math-attack` polymer)
**Crew role**: red-team-mathematician
**Backend**: `lean` (Lean 4 `v4.29.0` + Mathlib `v4.29.0`)
**Target**: Firoozbakht's conjecture — `p_{n+1}^(1/(n+1)) < p_n^(1/n)` for all `n ≥ 1`
**Date**: 2026-07-24

---

## 0. What this leg claims, and what it does not

This corpus says **nothing** about whether Firoozbakht's conjecture is true. Not one
entry is evidence for or against it.

What it measures is the **checker**. Nineteen false statements were placed as close
to the conjecture as the author could get them, and the Lean kernel was asked about
each one. The corpus passes when the kernel's answers match the entries' declared
verdicts — and, more usefully, it *fails informatively* when they do not.

Two entries were designed to fail informatively, and did. A third finding was not
designed at all — it surfaced when the comparator was first run and turned out to be
about the gate rather than about the corpus. All three are in §3.

---

## 1. Category coverage matrix

All fifteen required categories have at least one representative, and every
representative behaved as specified.

| # | Category | Entry | Mode | Witness / detector |
|---|---|---|---|---|
| 1 | Dropped hypothesis | `RT-01` | refute | `n = 0`: `1/0 = 0` collapses the RHS to `1` |
| 1 | Dropped hypothesis | `RT-02` | refute | `a = b = 1/2, n = 0` breaks the F1⟺F2 bridge |
| 2 | Wrong quantifier order | `RT-03` | refute | `∃n ∀C` on the barrier; instantiate `C := Bₙ` |
| 3 | Strengthened conclusion | `RT-04` | refute | `n = 1`: a factor-2 margin gives `6 < 4` |
| 3 | Strengthened conclusion | `RT-05` | refute | `n = 1`: `S₁ = log(4/3) < 1` |
| 4 | Off-by-one / boundary | `RT-06` | refute | `n = 1`: 1-indexed `p₁ = 2` vs `Nat.nth Nat.Prime 1 = 3` |
| 4 | Off-by-one / boundary | `RT-07` | refute | `n = 0`: `nthPrime 0 = nthPrime 1` aliasing |
| 5 | Wrong domain / type | `RT-08` | refute | `n = 1`: `n^(1/n)` rises on `(0, e)` |
| 6 | Universe / typing cheat | `RT-09` | reject | `Type : Type 1` — level slip |
| 7 | Ill-founded construction | `RT-10` | reject | unbounded prime search, no decreasing measure |
| 8 | Free-variable escape | `RT-11` | reject | unbound `n` under `autoImplicit false` |
| 9 | Circular assumption | `RT-12` | **kernel-blind-shape** | builds clean; caught by statement-shape audit |
| 10 | Direction reversal | `RT-13` | refute | `n = 1`: `√3 < 2` |
| 11 | Missing regularity | `RT-14` | refute | `n = 1`: `B₁ = 2` but `log²2 < 0.481` |
| 12 | Mis-cited lemma | `RT-15` | reject | `Nat.nth_prime_one_eq_three` cited for `p₁` |
| 13 | Scale-limited computation | `RT-16` | reject | `decide` on a universally quantified noncomputable goal |
| 14 | Axiom smuggling | `RT-17` | **kernel-blind-axiom** | builds 0, `sorry`-free; only `#print axioms` sees it |
| 15 | Statement drift | `RT-18` | refute | exponents elaborated in ℕ: `1/n = 0` for `n ≥ 2` |
| 15 | Statement drift | `RT-19` | refute | `StrictAnti` on all of ℕ: `root 0 = 1 < 2 = root 1` |

Nineteen entries, fifteen categories, four doubled (1, 3, 4, 15). The doubling is
not padding: categories 1, 4 and 15 are where this particular conjecture is most
fragile, for the reason given in §2.

---

## 2. Why these near-misses and not others

Firoozbakht's conjecture has an unusual property for a formalisation target: **the
index is inside the exponent.** The statement is `pₙ^(1/n)`, so an off-by-one does
not perturb the claim, it replaces it with a different claim that still looks
plausible and still has a first few true instances. Three consequences shaped the
corpus.

**The guard `1 ≤ n` is mathematics, not bookkeeping.** Mathlib's `Nat.nth Nat.Prime`
is 0-indexed; the conjecture is 1-indexed; the anchor bridges them with truncated ℕ
subtraction, which aliases `nthPrime 0 = nthPrime 1`. Lean's division is also total,
so `1/0 = 0` and `x^(1/0) = x^0 = 1`. Those two totality conventions conspire:
removing the guard does not produce an error, it produces a *false statement that
compiles*. `RT-01`, `RT-07`, `RT-19` are the three faces of this.

**Real exponentiation is invisible in surface syntax.** `RT-18` is the entry to read
first. It is character-for-character the conjecture; the only difference is that the
exponent literal `1/n` elaborates in ℕ, where integer division makes it `0` for every
`n ≥ 2`, degenerating the statement to `1 < 1`. Nothing in the prose gives this away.
The anchor's `rfl`-check pinning `Real.rpow` is the only defence, and `RT-18` is the
proof that the defence is needed.

**The inequality is strict but arbitrarily thin.** `Sₙ = log pₙ − n·(log p_{n+1} −
log pₙ)` is conjecturally positive and has no known uniform positive lower bound.
So any proof technique yielding a constant additive or multiplicative margin is
proving something stronger than the truth. `RT-04` and `RT-05` fail at `n = 1` —
immediately, not eventually — which is a cheap standing check on any future proof
attempt that reports "slack".

`RT-14` covers the mirror-image error on the analytic side. `attack.md` §2 states

  `Bₙ = L² − L − 1 − 3/L − 13/L² + O(L⁻³)`,  `L = log pₙ`,

and whatever its lower-order terms, it is a statement about a *tail*. Promoting it to
a statement about every index without the finite verification is how a proof sketch
reaches "therefore, for all `n`" while being unsound. `RT-14` dies at `n = 1`.

> **Gap flagged (ledger G1).** The `−3/L` and `−13/L²` terms of that expansion are
> **not** in the cited literature: `source-ledger.md` item 7 records that Kourbatov's
> Theorem 5 proves only `fₖ = log²pₖ − log pₖ − 1 + o(1)`, and that the two
> lower-order terms are either this project's own unproved derivation or an artefact.
> `RT-14` does **not** depend on them — its falsity is established by direct
> computation at `n = 1` (`B₁ = 2`, `log²2 < 0.481`), kernel-checked, with no
> asymptotics involved. The expansion appears here only as motivation, and is quoted
> as `attack.md`'s claim rather than as a sourced result.

---

## 3. Finding: `lake build` exit-0 is not a verdict

Two categories are **not detectable by the build gate**, and the corpus contains a
working demonstration of each. This is the leg's substantive result.

### 3.1 `RT-17` — axiom smuggling (category 14)

`corpus/lean/RT_17.lean` posts the open content of the conjecture as

```lean
axiom gap_below_barrier :
    ∀ n : ℕ, 1 ≤ n → ((nthPrime (n + 1) : ℝ) - (nthPrime n : ℝ)) < barrier n
```

and then derives Firoozbakht from it through `firoozbakht_iff_gap` — the anchor's
own, genuinely `sorry`-free bridge. The result:

- `lake env lean` exits **0**;
- there is no `sorry` in the file's **code** (the docstrings discuss the word, which
  is itself the subject of §3.3);
- `#print axioms` reports `[propext, Classical.choice, Quot.sound, gap_below_barrier]`.

The fleet constitution's gate reads "`lake build` exit 0 **(grep-clean of
sorry/axiom)**". `RT-17` is the evidence that the parenthesis is load-bearing and
that the two clauses are independent: the first passes while the second fails.

**Recommendation to `evidence-gate`**: the axiom audit must be a separate,
non-optional check, and it must whitelist exactly `{propext, Classical.choice,
Quot.sound}` — not merely grep the source for the token `axiom`, which a
`variable`-style or imported smuggle could evade.

### 3.2 `RT-12` — circular assumption (category 9)

```lean
theorem rt12_circular
    (hFiroozbakht : ∀ m : ℕ, 1 ≤ m → root (m + 1) < root m)
    (n : ℕ) (hn : 1 ≤ n) : root (n + 1) < root n :=
  hFiroozbakht n hn
```

Builds. `sorry`-free. Axiom-clean. And correct — it *is* a true theorem. The
falsehood is not in the Lean, it is in the sentence a paper would put next to it.

No kernel check can catch this, because there is nothing wrong for a kernel to
catch. The only mechanical detector is a **statement-shape audit**: the conclusion
appears verbatim among the binders. `run_corpus.py` implements exactly that, against
the `#check`ed type.

**Recommendation to `evidence-gate`**: for any declaration a paper names as a
result, print its type and confirm the conclusion is not among its hypotheses. A
theorem with the goal as a premise is vacuous no matter how clean its axioms are.

### 3.3 The `sorry` grep has a false-positive mode (found by running the corpus)

The first execution of `run_corpus.py` reported thirteen failures. Every one of the
kernel's verdicts was correct; the comparator was wrong. Its `sorry` check was a
naive `grep`, and it fired on the word `sorry` appearing in the corpus files'
**docstrings** — files that necessarily discuss `sorry`, because explaining why an
entry is false is half of what they are for.

That is not a bug local to this script. The fleet constitution specifies the gate as
"`lake build` exit 0 (grep-clean of `sorry`/`axiom`)", and a literal `grep -rn sorry`
over a Lean project fails the same way on:

- this entire corpus — nineteen files with zero `sorry` in code and dozens in prose;
- `skeleton.md`, `unproved.md`, and any honest prose record of what remains open;
- the anchor's `Statement.lean`, where the count is misleading rather than wrong:
  it holds **one** genuine `sorry` (in `theorem firoozbakht`) and six occurrences of
  the token, so a reviewer counting grep hits sees six problems where there is one.

Verified, 2026-07-24: `grep -c sorry lean/Firoozbakht/Statement.lean` → `6`;
`grep -c sorry corpus/lean/RT_17.lean` → `3`, all in docstrings.

A gate that cries wolf on documentation gets disabled, and then the real smuggle
walks through. `run_corpus.py` now strips Lean comments before grepping — but the
durable fix is different: **`#print axioms` showing no `sorryAx` is strictly stronger
than any grep and cannot be fooled by formatting.** The grep should be a convenience
check; the axiom audit should be the gate.

**Recommendation to `evidence-gate`**: replace the `sorry` grep with an
`#print axioms` whitelist. Keep a grep only as a comment-stripped secondary signal.

### 3.4 `RT-11` — a fourth, weaker case: the verdict depends on a project option

`RT-11` is rejected (`unknown identifier`) only because the file sets
`autoImplicit false`. Under Lean's default `autoImplicit true` the same text
elaborates silently, auto-binding `n` as an implicit natural and producing a
different statement — one which is *false* (it is `RT-07`).

So here the kernel's verdict is a function of build configuration, not of the
mathematics. The anchor project inherits Mathlib-style options via its
`lakefile.toml`; that inheritance is a soundness-relevant setting and should be
treated as such, not as formatting.

---

## 4. Weakly covered categories (Gödel disposition: this corpus is not complete)

No finite corpus covers all failure modes. Flagging the thin spots is more useful
than claiming coverage.

| Category | Coverage | Why it is thin |
|---|---|---|
| 6 — universe cheat | **weak** | `RT-09` is a blunt level slip that any Lean user would catch. A realistic universe attack on a number-theory statement would have to come through a large-elimination or `Type`-valued-choice route; the author found no natural such construction adjacent to a statement about ℕ and ℝ. Probably a genuine feature of the domain rather than a gap — but it is asserted, not proved. |
| 7 — ill-founded construction | **weak** | `RT-10` is a plain missing `termination_by`. The dangerous version is a construction that *does* pass the termination checker via a bogus measure, and no such construction is in the corpus. |
| 12 — mis-cited lemma | **medium** | `RT-15` is caught because the index appears in the type. Mis-citations that are *type-correct but mathematically wrong* — a lemma applied under a hypothesis that happens to typecheck — are the real hazard and are not represented. |
| 13 — scale-limited computation | **medium** | `RT-16` fails at `decide`. The corpus contains no `native_decide` entry, which is the trust hole that matters (it moves the verification outside the kernel). Adding one requires deciding whether `native_decide` is admissible in this polymer at all — that is a policy question for `evidence-gate`, not a corpus question. |
| 14 — axiom smuggling | **medium** | `RT-17` smuggles one obvious axiom in the same file. Smuggling via an imported module, or via a `Fact` instance registered elsewhere, would be harder to see and is not covered. |
| 2 — quantifier order | **medium** | `RT-03` is refutable because the flipped form is absurd. The dangerous flips near this conjecture (e.g. Cramér-type `∃C ∀n` versus `∀n ∃C` statements about `gₙ/log²pₙ`) are **open**, not false, so they cannot be corpus entries — a corpus entry must be *provably* false. This is a structural limit, not an oversight. |

**Uncovered by construction**: any failure mode whose false statement is not
*provably* false is inadmissible here. That excludes the most interesting near-misses
of all — the ones adjacent to Cramér's conjecture and to the `log²p` scale — because
their truth value is unknown. A corpus of open near-misses would be a different
artefact with a different (weaker) contract, and is not attempted.

---

## 5. Sources

The closed-set citation rule applies here as everywhere in this polymer: this leg
cites only rows of the sourcer's ledger,
`spore-runs/node/source-ledger/source-ledger.md`. Exactly two
entries make a claim that needs a source, and both were checked against it during
verification — one of them was wrong on the first pass.

| Where | Claim | Ledger row | Tier | Status |
|---|---|---|---|---|
| `RT-16` | Firoozbakht verified unconditionally for all `p < 2^64` | `visser2019verifying` | V0 | **corrected during verification.** The first draft attributed the `2^64` frontier to Kourbatov. Ledger item 6 exists precisely to correct that mis-attribution: Kourbatov's 2015 range is `4·10^18` (`kourbatov2015verification`, V1); the `2^64` frontier is Visser 2019. |
| `RT-14` | `Bₙ = L² − L − 1 − 3/L − 13/L² + O(L⁻³)` | `attack.md` §2 | — | **flagged as gap G1.** Ledger item 7 records that the two lower-order terms appear nowhere in Kourbatov's Theorem 5 (which gives only `log²p − log p − 1 + o(1)`) and are unsourced. Quoted as `attack.md`'s own claim, not as literature. The entry's refutation is independent of it. |

Every other entry's truth value is established by the Lean kernel and needs no
citation — which is the design intent, not an accident: a corpus whose falsity claims
rested on references would be a corpus of assertions.

---

## 6. Blocking checks

The role's five sign-off checks, answered.

| # | Check | Status |
|---|---|---|
| 1 | `ls corpus/*.json \| wc -l` ≥ `adversarial_corpus_min` (15) | **19** — pass. The comparator's own output lives at `corpus/results/latest.json`, deliberately outside this glob, so the count cannot be inflated by generated files. |
| 2 | Each entry has `statement`, `expected_verdict=false`, `category`, `provenance` | pass — schema enforced by `run_corpus.py`, which refuses any entry not declaring `expected_verdict: false` |
| 3 | `lake build` rejects every corpus entry | pass **with a correction to the wording**: 17/19 are rejected or refuted by the kernel; `RT-12` and `RT-17` are *accepted* by design, and that acceptance is the finding of §3. Reporting them as "rejected" would be false. |
| 4 | All 15 categories have a representative | pass — see §1; thin spots flagged in §4 |
| 5 | No corpus entry authored by the session that evaluates it | **partially satisfied — flagged.** This session authored the entries *and* ran `run_corpus.py`. The comparator is mechanical (it reads `expected_verdict` from the JSON and asks Lean; it has no opinion), so the kernel's verdicts are not self-graded. But the *adequacy* judgement — "are these the right nineteen statements?" — has not been made by an independent reader. Per the constitution's author–scorer separation, `evidence-gate` is that reader. This leg does not sign its own adequacy. |

Check 3 is the one worth reading twice. The role brief presumes every false
statement is kernel-rejectable; two of the fifteen categories are not, and saying so
is the point of the corpus rather than a shortfall of it.

---

## 7. Run record

`python3 corpus/run_corpus.py` — final run, 2026-07-24, after the verification-pass
edits of §5. Full machine-readable output in `corpus/results/latest.json`.

```
$ python3 corpus/run_corpus.py
corpus entries: 19  (blocking check: >= 15) OK

PASS  RT-01  [refute             ] dropped-hypothesis           kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-02  [refute             ] dropped-hypothesis           kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-03  [refute             ] wrong-quantifier-order       kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-04  [refute             ] strengthened-conclusion      kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-05  [refute             ] strengthened-conclusion      kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-06  [refute             ] off-by-one                   kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-07  [refute             ] off-by-one                   kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-08  [refute             ] wrong-domain                 kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-09  [reject             ] universe-cheat               kernel REJECTED the statement
PASS  RT-10  [reject             ] ill-founded-construction     kernel REJECTED the statement
PASS  RT-11  [reject             ] free-variable-escape         kernel REJECTED the statement
PASS  RT-12  [kernel-blind-shape ] circular-assumption          KERNEL BLIND (as predicted): file builds clean and axiom-clean; circularity caught only by the statement-shape audit
PASS  RT-13  [refute             ] direction-reversal           kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-14  [refute             ] missing-regularity           kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-15  [reject             ] mis-cited-lemma              kernel REJECTED the statement
PASS  RT-16  [reject             ] scale-limited-computation    kernel REJECTED the statement
PASS  RT-17  [kernel-blind-axiom ] axiom-smuggling              KERNEL BLIND TO EXIT CODE (as predicted): builds 0, grep-clean of sorry; only the axiom audit names gap_below_barrier
PASS  RT-18  [refute             ] statement-drift              kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)
PASS  RT-19  [refute             ] statement-drift              kernel REFUTED the statement (negation proved, sorry-free, axiom-clean)

19/19 entries behaved as specified.
categories covered by a PASSING entry: 15/15  (all 15)

wrote <worktree>/corpus/results/latest.json
```

**19/19 entries behaved exactly as their JSON specifies. All fifteen categories
have a passing representative.**

Reading the two `KERNEL BLIND` lines as passes is deliberate and is the whole point
of §3: the corpus *predicted* that the kernel would accept those two, and it did.
A run in which `RT-12` or `RT-17` were reported as "rejected" would mean the
comparator had been changed to tell a more comfortable story.

The first execution of this comparator reported 6/19 — thirteen false failures, all
from the comparator's own naive `sorry` grep hitting the corpus's docstrings. The
kernel's verdicts were correct in that run too. See §3.3; it is the reason the
comparator now strips Lean comments and leans on `#print axioms` instead.

---

## 8. What a downstream leg should take from this

- **`evidence-gate`**: treat the promotion gate as three checks, not one — build
  exit 0, axiom whitelist `{propext, Classical.choice, Quot.sound}`, and a
  statement-shape audit on every paper-named declaration (§3.1, §3.2).
- **`lean-probe` / `re-attack`**: `RT-04`, `RT-05`, `RT-14` are cheap standing
  checks. Any proof attempt reporting a constant slack, or promoting an `O(·)`
  bound to all indices, is refuted at `n = 1` before anyone reads it.
- **`write-paper`**: `RT-18` and `RT-06` are the two statement-fidelity hazards a
  reader cannot see. If the paper displays the conjecture, the displayed form must
  be checked against the anchor's elaborated term, not against its surface syntax.
- **Anyone editing the anchor**: `RT-07` and `RT-19` guard the two simplifications
  the anchor most invites — dropping `1 ≤ n` from `nthPrime_lt_succ`, and
  un-reindexing `firoozbakht_strictAnti`. Both are false.
- **`citation-gate`**: `RT-16`'s frontier was mis-attributed to Kourbatov on the
  first pass and corrected against ledger item 6 during this leg's own verification
  (§5). That is a live demonstration that the ledger's correction rows do work when
  they are actually consulted — and that a leg which does not consult them will
  reproduce exactly the mis-attribution the ledger was written to prevent.

---

*Author: Noogram. All corpus entries are hand-constructed near-misses of the
fidelity anchor `lean/Firoozbakht/Statement.lean`; no entry's truth value rests on an
LLM's assertion — falsity is established by the Lean kernel, and the two entries the
kernel cannot judge are labelled as such.*
