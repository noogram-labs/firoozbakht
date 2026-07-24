# CC-25 — The verification frontier: $p < 2^{64}$

| | |
|---|---|
| **Kind** | fact |
| **Status** | **SOURCED (V0)**. Note: this is a *citation*, not a machine-checked certificate — OB-B3 remains **OPEN**. |
| **Depends on** | — |
| **Feeds** | OB-B1–B3, W5; CC-26, CC-29 |
| **Ledger row** | `visser2019verifying` (**V0**), **Abstract** ¶ and **§4** ¶ — *"all three of these conjectures are unconditionally and explicitly verified for all primes below the location of the 81st maximal prime gap, certainly for all primes $p < 2^{64}$."* Predecessor: `kourbatov2015verification` (**V1**), abstract ¶ — *"true for all primes $p_k < 4\times10^{18}$"*. Computational record: `primegaplist_faq` (**V1**) — *"All prime gaps in $0 < x < 10^{20}$ have now been analyzed."* |

## Statement

Firoozbakht's conjecture — **and** the two strictly stronger Nicholson and Farhadian
conjectures (**CC-22**) — are verified for all primes

$$p \;<\; 2^{64} \;\approx\; 1.844\times10^{19}.$$

Three distinct quantities, routinely conflated:

| quantity | value | source |
|---|---|---|
| **verification extent** for Firoozbakht | $2^{64}$ | `visser2019verifying`, V0 |
| **exhaustive-sieve extent** | $4\times10^{18}$ | `kourbatov2015verification`, V1 |
| **gap-table analysis extent** | $10^{20}$ | `primegaplist_faq`, V1 (community, not refereed) |

`decompose.md#2.2`, `#5.1` and `#5.2` all carry $4\times10^{18}$ as *the* frontier. That
is the 2015 figure, superseded. Meanwhile `decompose.md#3.2` quotes a maximal gap at
$p \approx 1.836\times10^{19}$ — **above** the frontier it asserts elsewhere
(`outcomes.md#A11`). Table extent and verification extent are two different numbers and
must be stated as such.

## What verification does and does not establish

**Does**: no counterexample exists below $2^{64}$. Everything known about the conjecture
is of this form.

**Does not**: anything about $p \ge 2^{64}$. Finite verification cannot prove a universal
statement; it shifts the problem. And note the *method* — Kourbatov's verification is
**not a bare sieve**: it uses a first-occurrence prime-gap table **plus known bounds for
$\pi(x)$**, so it inherits the explicit-bounds chain (**CC-19**) and its validity
thresholds.

**OB-B3 remains OPEN.** The frontier is a **citation**, not a machine-checked
certificate. Nothing in this polymer has verified it, and this deck's own independent
computation reaches only $5\times10^7$ — eleven orders below. That independent range is
not a weak version of the frontier; it is a different thing (a check that *the statement
being attacked is the right one*), and it is what surfaced the small-$n$ tightness finding
the literature framing obscures.

## The $N_0$ question for Lean (W5)

`decompose.md#3.5`-W5 proposes machine-checking Firoozbakht to $N_0 = 10^6$–$10^8$.
Per `outcomes.md#A6` that is **fantasy by 4–10 orders** if F2 bignum comparison is the
engine ($O(N_0^{2.6})$ total). Revised target: $N_0 = 10^4$–$10^5$ **in index**, with
**CC-28**'s integer criterion as the engine.

**Disambiguate index vs. height.** `decompose.md#2.2`, `#3.5` and `#5.1` use $N_0$
ambiguously for both. $N_0 = 10^5$ in index is $p \approx 1.3\times10^6$ in height — a
**~13-order coverage gap** against $2^{64}$. `write-paper` must state that gap; a
Lean-checked $N_0 = 10^5$ is a genuine new artifact (nobody has a kernel-checked
certificate at any $N_0$) and is *not* progress toward the frontier.

## Traps

- Visser's §1 notes that $p^*_{81} > 2^{64}$ was established in September 2018, citing
  Nicely — whose pages are **all dead** as of 2026-07-24 (ledger gap **G5**, HTTP 404, and
  archive.org unreachable from the `source-ledger` leg). **Do not cite a live Nicely
  URL.** Use `primegaplist_faq` for the computational record.
- `primegaplist_faq` is a community resource, not peer-reviewed. Cite it as a
  computational record, never as a theorem.
- $2^{64}$ covers *Firoozbakht, Nicholson and Farhadian* — all three. That is a stronger
  statement than Kourbatov's and is one more reason debt #6 is live (**CC-22**).
