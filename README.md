# firoozbakht — a full-lane cosmon-fleet attack on Firoozbakht's conjecture

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
   solo**, given the same mission as a benchmark. It is honest on its own — it
   states plainly that it neither proves nor refutes the conjecture, and flags its
   substantial overlap with known work. That honesty is the baseline the fleet is
   measured against: the fleet's added value is not "stops it lying" (a good model
   left alone is already fairly honest) — it is the **independent recount**: the
   skeptic re-derives, the citation audit mechanically checks the locators the
   prover cited from memory, and Lean refuses the hand-wave.

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
