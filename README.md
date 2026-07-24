# firoozbakht — a full-lane cosmon-fleet attack on Firoozbakht's conjecture

> ### ⏳ Run in progress
> This repository is being written **by an agent fleet that is still running.**
> The understanding phase and the **attack** are in: decomposition, source ledger,
> concept cards, framing, **three proof-attempts** (one per subquestion), the
> **Lean skeleton** (the firewall's formal statement), and computational notebooks
> — alongside a single-model benchmark. Each proof-attempt opens with an explicit
> verdict and does **not** fake a proof: attempt #0 states its obstruction exactly
> and finds its own motivating premise false as stated; #1 and #2 report "open —
> neither proved nor refuted." Still landing as the DAG drains: the adversarial
> **skeptic** that recomputes every claim, the Lean **`lake build` probe** (the
> verdict itself), the **citation audit**, and the **red-team**. Watch the commit
> log, or see
> [`runs/2026-07-24-full-lane/STATUS.md`](runs/2026-07-24-full-lane/STATUS.md)
> for exactly which nodes have completed.

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
