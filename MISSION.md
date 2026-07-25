# Mission — firoozbakht

> Attack a famous open problem on the primes with a full agent fleet, and let a machine kernel — not a language model — author every verdict.

## The problem

**Firoozbakht's conjecture:** `p(n+1)^(1/(n+1)) < p(n)^(1/n)` for all `n ≥ 1`, i.e. `p(n)^(1/n)` is strictly decreasing over the primes. It is open, it is numerically verified far out, and it is known to imply unusually strong prime-gap bounds — which is exactly why it resists proof.

## Why a fleet, and why this galaxy

This galaxy runs the **full lane** of the [`math-attack`](https://github.com/noogram/sporarium) spore. Where the [`spore-bench`](https://github.com/noogram-labs/spore-bench) starter lane is the fast, no-Lean first pass, the full lane adds the parts that make an attack *trustworthy* rather than merely fluent:

- a **Lean `lake build` firewall** — the only thing allowed to author a "proved" verdict; on an open conjecture it honestly stays `sorry`;
- a **citation audit** (L0–L3) that resolves every DOI/arXiv locator before a source is allowed to count, mechanically checking claims the prover cited from memory;
- an **adversarial skeptic** that re-derives and independently recomputes every numerical claim from scratch;
- a **red-team** over the corpus of prior attacks.

## The benchmark

The same mission was handed to a **single strong model, solo**, and its output is kept alongside the fleet's as [`codex-solo-attack.md`](runs/2026-07-24-full-lane/codex-solo-attack.md). It is honest on its own — which is the point: the fleet is not sold as a lie-detector for a dishonest model, but as the **independent verification layer** a lone model cannot give itself.

**Caveat, published rather than hidden:** the solo attempt was written into this galaxy's working tree, at its root, under a generic filename, while the fleet was still running — so the fleet read it. It audited it rather than absorbing it (the source ledger flagged one of its expansions as unsourced; a proof attempt explicitly declines to use it), and no claim in the fleet's paper rests on it. But a run that read the baseline cannot measure independence from it. Item 2 is therefore **prior art the fleet audited**, not a control arm. See the README for the full account.

## Honesty discipline

Node roles are published; internal molecule IDs and machine paths are not. The full, un-scrubbed galaxy history — the live cosmon state, worktrees, and fleet instrumentation — lives in a private archive. Provenance hashes on published deliverables are left untouched.

---

*Built by a Noogram agent fleet.*
