# Evidence gate — Firoozbakht's conjecture

**Leg**: `evidence-gate` (pre-synthesis seal of the `math-attack` polymer)
**Crew role**: editor
**Target**: `p_{n+1}^(1/(n+1)) < p_n^(1/n)` for all `n ≥ 1` — to be PROVEN or REFUTED, never assumed
**Date**: 2026-07-24
**Gate protocol**: v4 (LOOP leg first), fail-closed

---

## 0. Verdict

> # **BLOCKED**
>
> **Failing legs: KERNEL (leg 1) and SKEPTIC (leg 2). CORPUS (leg 3) passes.**

Two of the three applicable legs fail. Under the fail-closed rule a single failing
leg is sufficient to block; two fail independently, for unrelated reasons.

This verdict is about **the evidence**, not about the conjecture. Firoozbakht's
conjecture is **open**. No leg of this polymer claims it is true, false, or likely,
and this gate makes no such claim either.

`synthesize`, `editorial-verdict` and `write-paper` are **not cleared to run** on
this evidence. `citation-gate` is out of scope here by construction — no paper
exists yet, so no citation audit is performed at this gate (see §5).

---

## 1. LOOP leg (v4) — which round is live

Read **first**, per protocol: `re-attack/reattack-verdict.json`.

| Field | Value (verbatim from the JSON) |
|---|---|
| `verdict` | `BLOCKED` |
| `rounds_run` / `rounds_target` | `1` / `1` |
| `exit_reason` | `rounds-exhausted` |
| `formal_backend` | `lean` |
| `executor_driver_capable` | `true` |
| `final_round.round` | `1` |
| `escalation.required` | `true` |

**The file is present and well-formed** (parses as JSON, carries `final_round`,
`final_round.artifacts`, `history`). So the LOOP leg does not itself block — but it
names the live round, and the live round is **round 1**.

`rounds_run = 1`, i.e. the loop nucleated no round 2. Per the v4 rule for
`rounds=1`, the live round **is** round 1 and the kernel/skeptic legs below are read
directly from `lean-probe/lean-probe-report.md` and `skeptic/faults.md`, exactly as
v3.x did. `final_round.artifacts` confirms these are the same paths the loop points at:

```
faults            → skeptic/faults.md
unproved          → lean-probe/sorry-audit.txt
lean_probe_report → lean-probe/lean-probe-report.md
```

No round-2 artifact directory exists in the run tree. There is nothing newer to read.

---

## 2. KERNEL leg — **FAIL**

Source of record: `lean-probe/lean-probe-report.md`, `lean-probe/build.log`,
and `re-attack/reattack-verdict.json → final_round.kernel_evidence`.

The gate is a **conjunction**: `lake build` exit 0 **AND** grep-clean of
`sorry`/`axiom`. The first clause passes; the second fails.

| Clause | Result | Evidence |
|---|---|---|
| `lake build` exit status | **0** ✅ | `build.log` last line: `lake build exit status: 0`; `Build completed successfully (8253 jobs).` |
| grep-clean of `sorry` | **NO** ❌ | `kernel_evidence.grep_clean = false`; `sorry_count = 1` at `lean/Firoozbakht/Statement.lean:117`; the tactic `sorry` sits at line 120 of that file |
| grep-clean of `axiom` | pass ✅ | `grep -n "^axiom "` over `lean/Firoozbakht/*.lean` → **no hits**, re-run by this gate |
| `native_decide` used | `false` ✅ | `grep -n native_decide` over the same four files → **no hits**, re-run by this gate; so no `Lean.ofReduceBool` in the trusted base |
| Backend is `none`? | **no** — `formal_backend = "lean"` | so the DEGRADED escape hatch does **not** apply |

**Two independent checks this gate ran itself**, on the artifacts rather than on any
leg's prose.

**(a) The source grep, re-run on `lean/Firoozbakht/`.** The naive count is
`Audit.lean:8, Criteria.lean:1, FiniteRange.lean:4, Statement.lean:6` — **19 hits**.
Exactly **one** is a `sorry` in code: `Statement.lean:120`, the tactic body of
`theorem firoozbakht` declared at line 117. The other eighteen are prose in docstrings
that discuss what remains open. This gate therefore did **not** rest its verdict on the
grep count — that is precisely the false-positive mode the corpus leg documented in its
§3.3, reproduced here first-hand. The verdict rests on (b).

**(b) The axiom whitelist.** `grep "axioms:" lean-probe/build.log` filtered to lines
*outside* `{propext, Classical.choice, Quot.sound}` returns exactly **7 lines** out of
31, all showing `sorryAx`:

```
'Firoozbakht.firoozbakht'            depends on axioms: [propext, sorryAx, Classical.choice, Quot.sound]
'Firoozbakht.firoozbakht_strictAnti' … sorryAx …
'Firoozbakht.firoozbakht_int'        … sorryAx …
'Firoozbakht.firoozbakht_log'        … sorryAx …
'Firoozbakht.firoozbakht_antitone'   … sorryAx …
'Firoozbakht.firoozbakht_slack'      … sorryAx …
'Firoozbakht.firoozbakht_gap'        … sorryAx …
```

This is the **stronger** form of the check that the corpus leg recommended in its
§3.1 (`#print axioms` whitelist, not a source grep) — and it agrees with the grep:
the conjecture is not proved. `final_round.kernel = "UNPROVABLE_IN_BUDGET"`.

### 2.1 What the kernel leg *did* deliver (recorded, not credited to the gate)

The failure is honest and the budget was not wasted. `lean-probe` added **11 new
`sorry`-free theorems** plus a 26-entry prime table, all showing the clean axiom
line `[propext, Classical.choice, Quot.sound]` — including `firoozbakht_of_le`, the
conjecture proved outright in its literal `rpow` form for `1 ≤ n ≤ 25`, and two exact
refutation criteria. Debt D2 of the skeleton is closed.

**None of that changes the verdict**, and it must not be allowed to. The leg's own
words: *"No finite range bears on the conjecture. The whole difficulty is in the tail.
A certified range is a fidelity instrument, not evidence."* This gate agrees, and
records the 25-index range as **instrumentation**, never as evidence for the
conjecture.

### 2.2 Why this is FAIL and not DEGRADED

DEGRADED is available only when `formal_backend = "none"` — i.e. when no formal
backend was ever attempted and the leg is honestly reporting its absence. Here a
Lean backend **was** run, **did** build, and **did not** discharge the `sorry`.
That is a failed kernel leg, not a degraded one. Marking it DEGRADED would convert
"we tried and the conjecture is still open" into "we could not try", which is a
different and false statement.

---

## 3. SKEPTIC leg — **FAIL**

Source of record: `skeptic/faults.md` (594 lines) and `skeptic/verify_faults.out`.

| Requirement | Result |
|---|---|
| `faults.md` exists | **yes** ✅ |
| zero residual BLOCKERs | **no — 2 live BLOCKERs** ❌ |

`faults.md` §0: *"The seal is BLOCKED. Two BLOCKER findings, seven MAJOR, nine MINOR."*
Its §6 disposition table names both, and its explicit gate sentence is addressed to
this leg by name:

> *"Gate for `synthesize`, `evidence-gate`, `editorial-verdict` and `write-paper`:
> findings 1 and 2 must be closed before the seal."*

They are not closed.

### 3.1 The two BLOCKERs, and this gate's independent confirmation that they are live

**BLOCKER-1 — `notebooks__2/data/PROVENANCE.md` overstates independent recomputation by 47%.**
The provenance record claims *"independently recomputes rows 1–44"*; the true figure
is rows 1–30. (The "47%" is the skeptic's and the loop's own figure, quoted; recomputed
here it is `(44−30)/30 = 46.7%`, which is what they rounded.) Confirmed live by direct
read: `PROVENANCE.md:28` still reads
`independently recomputes rows 1–44`, while `findings-2.md` says 1–30 — the two
files contradict each other. This sits in the provenance record of the corpus's
single weakest premise.

**BLOCKER-2 — `notebooks__2/findings-2.md` §4: "the verified range itself excludes C ≥ 1.1736" is an invalid inference.**
Confirmed live by direct read: `findings-2.md:219` still carries the exclusion, and
`findings-2.md:271` still advertises it as a delivered result (*"plus the new
exclusion bound C ≥ 1.1736"*). A finite verified range cannot exclude a value of a
`limsup` constant; the sentence is a property of a model fit being read as a
statement about the tail. It is the leg's advertised novelty and is already being
used to reallocate the polymer's budget — so it propagates.

`skeptic/verify_faults.py` ships **prose gates** designed exactly so a later reader
can tell whether a finding was fixed or merely acknowledged. This gate re-read its
output of record, `verify_faults.out` — **48/48 pass, exit 0** — including:

```
[PASS] BLOCKER-1  PROVENANCE.md still says 'rows 1–44'  (fault is live)
[PASS] BLOCKER-1  findings-2.md says rows 1–30 (contradicting PROVENANCE.md)
[PASS] BLOCKER-2  the C>=1.1736 exclusion claim is live in findings-2.md
```

A `PASS` here means *the fault is still present*. Both BLOCKERs are **residual**.

### 3.2 The seven MAJORs are not this gate's blocking criterion, but they are recorded

The gate's stated criterion is *zero residual BLOCKERs*, so MAJOR-3 … MAJOR-9 do not
independently block. They do, however, all carry the skeptic's second gate sentence:
*"Findings 3–9 must be closed before anything reaches the paper."* MAJOR-6 in
particular — the corpus's single external analytic premise cited at a locator
(`Cor. 3.5`) that does not exist in the version-of-record it is attributed to —
is a citation fault that `citation-gate` will have to adjudicate later, and it will
not disappear on its own.

---

## 4. CORPUS leg — **PASS**

Source of record: `red-team-corpus/coverage-report.md` (339 lines) and
`red-team-corpus/corpus/`.

| Requirement | Result |
|---|---|
| red-team corpus present | **yes** ✅ — `corpus/RT-01.json … RT-19.json` |
| coverage report non-empty | **yes** ✅ — 339 lines (21 931 bytes on disk; 21 663 characters decoded) |

Independently counted by this gate:

- `ls corpus/RT-*.json | wc -l` → **19** entries (threshold `adversarial_corpus_min = 15`; the comparator's own output lives at `corpus/results/latest.json`, outside the glob, so the count cannot be inflated by generated files)
- `wc -l corpus/provenance.jsonl` → **19** rows, one per entry
- coverage matrix in §1 → **15/15** categories represented, four doubled (1, 3, 4, 15)

This leg passes. It is worth being precise about *what* it certifies: the corpus
says **nothing** about whether the conjecture is true. It measures the **checker**.

### 4.1 Two findings this gate accepts and adopts

The corpus's substantive result is that `lake build` exit-0 is not a verdict, and it
ships a working demonstration of each blind spot:

- **`RT-17` (axiom smuggling)** builds with exit 0 and is `sorry`-free in code, yet
  `#print axioms` reports a smuggled `gap_below_barrier` axiom. **Adopted**: this gate
  ran the `#print axioms` whitelist check in §2, not merely a source grep.
- **`RT-12` (circular assumption)** builds, is `sorry`-free and axiom-clean, and is a
  *true* theorem — the falsehood would live in the sentence a paper puts next to it.
  Detectable only by a statement-shape audit. **Adopted as a standing requirement on
  `write-paper`**: for any declaration the paper names as a result, print its type and
  confirm the conclusion is not among its hypotheses.
- **The `sorry` grep has a false-positive mode.** `grep -c sorry Statement.lean` → 6,
  of which exactly **1** is a genuine `sorry` in code; the rest are prose. This gate
  therefore did **not** rest §2 on a raw grep count; it rested on the axiom audit,
  which cannot be fooled by formatting, and cross-read `sorry_location`.

### 4.2 The one check the corpus leg refused to sign for itself

Its §6 check 5 (*"no corpus entry authored by the session that evaluates it"*) is
recorded as **partially satisfied — flagged**: the same session authored the entries
and ran the comparator. The kernel's verdicts are not self-graded (the comparator is
mechanical), but the *adequacy* judgement — "are these the right nineteen statements?"
— was explicitly deferred to this gate.

**This gate's adequacy reading**: the nineteen entries are adequate for the corpus's
declared purpose. The three doubled categories (1, 4, 15) are doubled where this
particular conjecture is genuinely most fragile — the index sits inside the exponent,
so an off-by-one yields a *different plausible claim that compiles* rather than an
error. `RT-18` (exponent elaborating in ℕ, making `1/n = 0`) is character-for-character
the conjecture and is the strongest entry in the set. The leg's own §4 flags its thin
spots (categories 6, 7, 12, 13, 14, 2) rather than claiming completeness, and its
structural limit — *a corpus entry must be provably false, which excludes the most
interesting near-misses because they are open* — is correctly reasoned, not an
oversight. **Adequate. Not complete, and it does not claim to be.**

---

## 5. Scope — what this gate did NOT do

**No citation audit was performed.** Per the mission, the citation audit happens later,
at `citation-gate`, once `write-paper` has produced the paper. No paper exists in this
run (`write-paper/` is empty). Auditing citations here would be auditing an artefact
that does not exist.

Two citation-shaped faults are nevertheless **handed forward** to `citation-gate`,
because they are already live in the corpus and will reach the paper if unaddressed:

| id | fault | where |
|---|---|---|
| MAJOR-6 | The single external analytic premise is cited at `Cor. 3.5`, a locator that does not exist in the attributed version-of-record. Needs version-qualification + the Axler corrigendum as a V0 row. | ledger §3.5, CC-19, PA-0, PA-2, `notebooks__2` |
| MAJOR-7 | Two mutually incompatible verification tiers for `schoenfeld1976sharper` (V2 in `notebooks__1`, V3 in `proof-attempt__1`); the pessimistic one is wrong. | `proof-attempt-1.md` §8.1, gap G8 |
| G1 | `Bₙ`'s `−3/L` and `−13/L²` lower-order terms appear nowhere in Kourbatov's Thm 5 (which gives only `log²p − log p − 1 + o(1)`) — unsourced. | `attack.md` §2, ledger item 7 |

Four of the skeptic's gates are **external retrievals** it could not re-run
(S11 CMS 2019, S12 Johnston 2022, S13 Axler v3 + corrigendum, S14 Kourbatov
corrigendum). It quoted them verbatim with retrieval URLs precisely so
`citation-gate` can re-fetch and compare rather than take its word. That is the
right handoff and this gate does not pre-empt it.

---

## 6. Leg summary

| # | Leg | Requirement | Verdict |
|---|---|---|---|
| 0 | **LOOP** | `reattack-verdict.json` present and well-formed; names the live round | **present** — live round = **1** (`rounds_run = 1`, loop nucleated nothing) |
| 1 | **KERNEL** | `lake build` exit 0 **AND** grep-clean of `sorry`/`axiom` | **FAIL** — exit 0 ✅ but `grep_clean = false`, 1 live `sorry` at `Statement.lean:117`, 7 declarations carry `sorryAx`. Backend is `lean`, not `none`, so DEGRADED does not apply. |
| 2 | **SKEPTIC** | `faults.md` exists **AND** zero residual BLOCKERs | **FAIL** — exists ✅ but **2 live BLOCKERs**, both independently re-confirmed present in the audited files. |
| 3 | **CORPUS** | corpus present **AND** coverage report non-empty | **PASS** — 19 entries / 15 categories / 339-line report; adequacy signed here per the corpus leg's deferred check 5. |

**Applicable legs: 3. Passing: 1. Verdict: BLOCKED.**

---

## 7. What has to happen before this gate can be re-run

In the order the evidence imposes, not in order of effort:

1. **Close BLOCKER-1** — `notebooks__2/data/PROVENANCE.md`: `rows 1–44` → `rows 1–30`,
   then re-read every downstream sentence that leans on the wider claim. Not a typo
   fix; the row range is load-bearing for the corpus's weakest premise.
2. **Close BLOCKER-2** — `notebooks__2/findings-2.md` §4: delete the `C ≥ 1.1736`
   exclusion, or restate it as a property of the model fit that says nothing about a
   `limsup`. Then unwind the budget reallocation that currently rests on it (§8-5,
   MAJOR-9, which rests on a 30-decade extrapolation).
3. **Then re-germinate with `rounds ≥ 2`** on a driver-capable executor if further
   formal attack is wanted. The executor here *was* driver-capable
   (`executor_driver_capable = true`); the constraint was `rounds_target = 1`, which
   exhausted the cap with the kernel unproved and no automatic recourse left in the run.

Note that steps 1 and 2 are **corpus-honesty faults, not mathematics** — neither needs
new theory, and closing them will not close the kernel leg. The kernel leg closes only
if the conjecture is proved, which is the open problem itself.

---

## 8. Honest endpoint

The gate is fail-closed and it closed.

Absent evidence is BLOCKED, never PASS — and here the evidence is not absent, it is
**present and negative**: a build that succeeds while the theorem it was built for is
still `sorry`, and an auditor that found two live overstatements in the corpus's own
record of what it verified. Both are exactly the failure modes a pre-synthesis gate
exists to catch, and both were caught by the polymer's own instrumentation rather
than by assertion.

The conjecture is proved for 25 indices, the shape of a valid refutation is
formalised, and the checker has been red-teamed. **Firoozbakht's conjecture remains
open — not proved, not refuted.** Nothing in this run bears on its truth, and this
gate produced no evidence about it in either direction.

---

## 9. Reproduction

```
cd <run>/evidence-gate
python3 verify_gate.py          # 61 named checks, exit 0
```

`verify_gate.py` re-derives **every** field of §1–§5 from the source artifacts on
disk — it re-parses `reattack-verdict.json`, re-greps `build.log` for the axiom
whitelist, re-greps `lean/Firoozbakht/*.lean` first-hand for `sorry` / `axiom` /
`native_decide`, re-reads the two BLOCKER artifacts to confirm the faults are live,
re-counts the corpus, and re-checks that the four downstream leg directories are
still empty. It asserts the verdict is `BLOCKED`. If a future session fixes
the BLOCKERs, the corresponding checks **fail**, which is the intended behaviour:
this script is a tripwire, not a rubber stamp. Output of record: `verify_gate.out` —
**61/61 pass, exit 0**. Deterministic; no RNG; no network.

Gate status for this leg's own artifacts: `ruff check verify_gate.py` — clean;
`python3 -m py_compile` — OK. `.cosmon/config.toml` defines no `[gates]` for this
project, so these are the gates this leg imposed on itself. The run artifacts live
under `.cosmon/state/`, which `.cosmon/.gitignore:4` excludes from version control,
so there is nothing for this leg to commit — the same condition every other leg of
this run worked under.

No check is reported here as passing that was not run. The two checks this gate
could **not** run itself are named as such: it did not re-execute `lake build`
(it read the shipped `build.log`), and it did not re-fetch the four external
citations S11–S14 (those are `citation-gate`'s, per §5).

---

*Emitted by the `evidence-gate` leg (node) of the `math-attack`
polymer. Firoozbakht's conjecture is **open**. This leg produced no evidence about
its truth in either direction, and found none in the corpus it gated.*
