# CC-30 — Proof triage: FT-7′ and FT-7″

| | |
|---|---|
| **Kind** | technique |
| **Status** | **OPEN** — FT-7″ has never been run; FT-7′ replaces a filter that this deck holds invalid |
| **Depends on** | CC-16, CC-23, CC-24, CC-31 |
| **Feeds** | debt #11; `skeptic`, `red-team-corpus`, `proof-attempt__0/1/2` |
| **Ledger row** | NONE — methodology. The entailment it rests on is `kourbatov2015upper` **Theorem 1** (V0), see **CC-16**. |

## What is being replaced

`decompose.md#4`-FT-7, mandated **Critical** and **first** on every proof attempt:

> *"any correct proof of F implies $g_n < (1-o(1))(\log p_n)^2$ for all $n$ with explicit
> constants. Therefore: a candidate proof of F that does not, somewhere, establish a
> polylogarithmic prime-gap upper bound is wrong. … it does not require reading the
> candidate proof's details."*

**The inference is invalid.** It moves from *semantic entailment* (a true statement of F
entails the gap bound — **CC-16**, V0) to *syntactic occurrence* (the proof text must
contain it). Four proof shapes evade it (`outcomes.md#A2`):

1. proof by contradiction that never exhibits the bound;
2. establishment that is ineffective or non-explicit;
3. proof via a stronger intermediate statement (e.g. Nicholson, **CC-22**);
4. induction where the constraint is only ever local.

Worse, FT-7 is **irrefutable by construction**: the only thing that could refute it is a
correct proof of Firoozbakht, which FT-7 discards before anyone reads it — and each kill
is then counted as evidence for the strategic conclusion that generated the filter. It is
the only defect in this corpus whose failure mode is a **silent false negative that
destroys a correct proof**, and it is mandated first. `outcomes.md` names it *the weakest
branch in the artifact*.

## FT-7′ — extraction obligation, non-terminal

Replace the filter with a triage. Given a candidate proof of F, attempt to **extract**
the entailed gap bound from its argument. Three outcomes, no others:

| outcome | verdict |
|---|---|
| **(a)** bound extracted | proceed to normal review |
| **(b)** not extracted, **and** the method demonstrably cannot yield it | escalate as **strong evidence of error** — with the obstruction named |
| **(c)** not extracted, no obstruction identified | **flag as high-interest. Do not terminate.** |

Amend debt #11 to read: *"FT-7′ triage; **no candidate is terminated by FT-7′ alone**."*

Outcome (c) is the whole point. Under FT-7 it was indistinguishable from (b); under FT-7′
it is the most interesting cell in the table — a proof whose mechanism nobody can locate
is either wrong in a novel way or right in a novel way, and both are worth reading.

## FT-7″ — the relativization / prime-blindness barrier

A **new** test, cheap, structural, read-free, and itself falsifiable:

1. Enumerate the arithmetic facts about the primes the candidate argument actually uses
   (density, average gap, multiplicative structure, sieve axioms, …).
2. Construct a set $A \subseteq \mathbb{N}$ satisfying **all** of them but containing a
   gap $\ge (\log p)^2$ at some point.
3. If the argument runs **verbatim** on $A$, it cannot prove F — because F is false for
   $A$.

This is the prime-gap analogue of a relativization barrier, and unlike FT-7 it is
falsifiable: exhibiting an argument that provably fails to relativize refutes the test's
applicability to that argument. It also *tells you something* when it fires — namely which
axiom the argument is missing.

Construction sketch (the obligation this card leaves open): take the primes and delete a
suitable block, or take a Cramér-random set conditioned on a long gap. Both preserve
density and most sieve-visible structure. **Nobody in this polymer has built one.** That
is the deliverable.

## How the triage relates to the barriers

**CC-23** and **CC-24** say what the target requires. FT-7′ asks whether a candidate
supplies it. The three cards must stay consistent:

- FT-7′ outcome (b) is licensed only by the **surveyed-set** form of **CC-23**, never by
  its `[L3]` universal-negative clause. "No technique in analytic number theory produces
  a polylogarithmic gap bound" is exactly the premise that would make FT-7 terminal, and
  it is unproved.
- A candidate proceeding through a *strengthening* (**CC-22**) is evasion shape 3 and
  must not be killed for it.

## Role in the proof-obligation tree

**Debt #11**, re-specified. Feeds **OB-G** (barrier/metamathematics, **CC-23**): FT-7″'s
construction, if it exists, *is* a barrier result and is publishable as one.

## Traps

- **Do not restore the "without reading the details" clause.** Extraction requires
  reading the argument's *method*, which is the cheap part; it does not require verifying
  the proof.
- A filter that can only ever confirm the prior that built it is not a test
  (**CC-31**). If FT-7′ never returns (c) across many candidates, that is evidence the
  triage has drifted back into FT-7, not evidence about F.
- The corpus contains **no test that could come out for F**. FT-8 (**CC-21**) is the only
  proposed one, and it must be **pre-registered** before `notebooks` runs or it is
  post-hoc.
