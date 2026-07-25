# firoozbakht — the first full-lane fleet attack, and the baseline it was measured against

> **Which repository is which.** Two runs of the same spore on the same
> conjecture exist, and they answer different questions:
>
> | | This repo | [`firoozbakht-cleanroom`](https://github.com/noogram-labs/firoozbakht-cleanroom) |
> |---|---|---|
> | Run | First full lane, 23 nodes | Clean-room re-run, 22 nodes |
> | Working tree during the run | Contained a solo-model attempt, which the fleet read | Contained nothing but the spore's own output |
> | Comparison against the baseline | Not independent — see the caveat below | Independent |
> | Published as | A rebuilt, scrubbed repo | The galaxy exactly as generated |
> | Also hosts | **The Codex solo baseline** both runs are compared against | — |
>
> Read this one for the baseline, and for what a fleet does when a stray
> document enters its workspace. Read the other for the independent attack.
> The comparative study of both is in
> [sporarium `docs/reports/`](https://github.com/noogram/sporarium/tree/main/docs/reports).

> ### ✅ Run complete — 23/23 nodes — and the seal is **BLOCKED**, honestly
> The fleet drained the whole DAG. The headline is not a proof; it is a
> **disciplined refusal to overclaim**, which is the point:
>
> - **The conjecture is neither proved nor refuted** — it is open. The Lean
>   `lake build` returned exit 0 with **exactly one `sorry`, the conjecture itself**,
>   left undischarged (`sorryAx` in `#print axioms`). Never dressed up as anything else.
> - **But the budget produced real, machine-checked mathematics:** 11 new `sorry`-free
>   Lean theorems, the conjecture proved outright for 1≤n≤25, two exact refutation
>   criteria, and three named routes killed (two with reproducible obituaries).
> - **Every gate fails closed, on purpose.** The adversarial `skeptic` found *no
>   mathematical error* — every proof sound, every number reproduces — yet **blocked
>   the seal** on two *epistemic-standing* faults (a provenance file overstating its own
>   independent verification by 47%, an invalid inference sold as a result). The
>   `citation-gate` also blocked — not for a fabricated citation (there are none) but
>   for coverage below the tier bar.
> - **The paper is included, and it refuses to clear itself.** `runs/2026-07-24-full-lane/paper/paper.md`
>   carries a `staged — not cleared for release` posture in its own §0.2, naming the three
>   gates that block it. That self-gating *is* the discipline on display.
>
> See [`runs/2026-07-24-full-lane/STATUS.md`](runs/2026-07-24-full-lane/STATUS.md) for the
> node table, and [`runs/2026-07-24-full-lane/synthesis.md`](runs/2026-07-24-full-lane/synthesis.md)
> for the fold of the whole run. Full un-scrubbed history is in a private archive.

This is a dedicated galaxy running the **full lane** of the
[`math-attack`](https://github.com/noogram/sporarium) cosmon spore against
**Firoozbakht's conjecture** — the claim that

```
p(n+1)^(1/(n+1)) < p(n)^(1/n)      for all n ≥ 1
```

(equivalently, that `p(n)^(1/n)` is strictly decreasing over the primes). It is a
famous **open problem**, and nothing here claims to settle it. The point is to
show a fleet of agents doing the *real* work of attacking it — and being honest
about where the attack stops.

## The invariant

`math-attack`'s load-bearing rule is that **no result is ever called "proved" on
a language model's say-so.** A machine kernel authors the verdict; the model only
proposes. In the full lane that kernel is **Lean**: a claim is "proved" only when
`lake build` compiles it against Mathlib. On a genuinely open conjecture the Lean
core will honestly land on `sorry` — *that is the firewall doing its job, not the
run failing.*

## Two things to compare

1. **`runs/2026-07-24-full-lane/`** — the **fleet's** attack (full lane: Lean
   firewall + citation audit + red-team + adversarial skeptic). Filling in as the
   run drains.
2. **`runs/2026-07-24-full-lane/codex-solo-attack.md`** — a **single strong model,
   solo**, given the same mission. It is honest on its own — it states plainly
   that it neither proves nor refutes the conjecture, and flags its substantial
   overlap with known work. The fleet's added value is therefore not "stops it
   lying" (a good model left alone is already fairly honest) — it is the
   **independent recount**: the skeptic re-derives, the citation audit
   mechanically checks the locators the prover cited from memory, and Lean
   refuses the hand-wave.

> **A clean-room re-run now exists:**
> [`noogram-labs/firoozbakht-cleanroom`](https://github.com/noogram-labs/firoozbakht-cleanroom).
> Same conjecture, same spore, a working tree that contained no prior attempt for
> the entire run — and published exactly as generated, with no staging copy and no
> scrub pass. Read that one for the independent attack; read this one for the
> contamination and what the fleet did with it.

### ⚠️ This is not a clean-room comparison — and here is exactly why

Read the two side by side, but **do not read them as independent runs.** The solo
attempt was written into this galaxy's working tree, at its root, under a generic
filename, *while the fleet was still running*. The fleet found it and read it.

We are publishing that fact rather than quietly re-running, because what the fleet
did next is the more interesting result: **it treated the stray document as an
untrusted input and audited it.** The source ledger flagged its four-term
expansion of $B_n$ as **unsourced** (gap G1 — "derive it with proof, or delete
it"); one proof attempt states explicitly that it *does not use* that expansion
and routes through a cited bound instead; the concept cards caught a notation
collision between the two documents. No claim in the fleet's paper rests on the
solo document.

So the honest reading is: the verification layer held under an unplanned
contamination, but the *benchmark* claim does not — you cannot measure
independence with a run that read the baseline. Treat item 2 as **prior art the
fleet audited**, not as a control arm. A genuine clean-room comparison requires
the solo attempt to live outside the worktree for the whole run; that discipline
is now an explicit part of the spore's input perimeter.

## Lineage

This galaxy is the deeper sibling of
[`spore-bench`](https://github.com/noogram-labs/spore-bench), which recorded the
fast **starter lane** of the same spore on the same conjecture. The template
itself descends from [`flow-matching-gaussians`](https://github.com/noogram-labs/flow-matching-gaussians) —
the first cosmon fleet to ship a publishable scientific artifact and report its
own life-cycle; `math-attack` generalises that pattern into a reusable spore.

The bundled spore under [`spores/math-attack/`](spores/math-attack/) is the
published copy, included so this run is reproducible from one place.

---

*Built by a Noogram agent fleet.*
