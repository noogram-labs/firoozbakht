# Chronicles — firoozbakht galaxy

Feynman-register log. Each entry is a short story for a future reader, and for
a reviewer who did not walk the path. Only principles get an entry. Most of what
happens here does not, and that is the point.

---

## 2026-07-24 — Four people looked at the bridge. One drove a truck across it.

A paragraph in the decomposition explained why a floating-point error could not
grow. Five adversarial reviewers were pointed at it. Two read the argument and
pronounced it sound. Two ran the number and found the paragraph wrong by a factor
of three thousand — in the unsafe direction, the one where you believe a result
that is not there.

Here is why the paragraph was so convincing. It said, in effect: *this quantity
never gets bigger than 18, so nothing here can blow up.* That is a true sentence
about the **value**. It says nothing at all about the **error**, and the error was
the thing in question. The prose reasoned *about* a computation instead of *from*
it. Read at reading-speed, the two are indistinguishable — which is exactly why
two independent skeptics reproduced the mistake verbatim, each with a skeptic's
badge on.

The alarm that was supposed to catch this had been set to `1e-11`. The real error
was `3.3e-10`. The smoke detector was tuned thirty times below the smoke. The one
mechanism in the whole apparatus advertised as *failing loudly* was, by
construction, silent.

**The lesson is not "get better reviewers."** It is that five reviewers holding the
same instrument are one reviewer with a louder voice. Reading is an instrument.
Measuring is a different instrument. If everyone reads, you have multiplied
confidence without adding a single grain of evidence — and confidence is the thing
that kills you. Diversity of *instrument*, not diversity of *opinion*.

So: any sentence that asserts a number *about* a computation is unverified until
the computation prints that number. No exceptions, no matter how many people
nodded at it.

---

## 2026-07-24 — The kitchen was spotless. The label on the jar was wrong.

The skeptic went through the whole corpus with a knife. Every proof, line by line.
Every load-bearing number re-derived in independent code. Seven verifier scripts
re-run. Five primary sources fetched by hand.

It found **zero mathematical errors**. Every theorem sound as written. Every
number reproduced to the digit.

And it stopped the run cold, on two findings. Both were sentences the corpus had
written **about itself**:

- *"We independently recomputed rows 1–44."* Thirty rows were recomputed.
- *"The verified range excludes C ≥ 1.1736."* A finite range cannot say anything
  about the limit of an infinite sequence. It was a fact about a curve fit,
  wearing the clothes of a fact about infinity.

Nobody eats from the kitchen. Everyone eats from the label. Three legs downstream
had already reallocated the run's budget on the strength of that second sentence —
so a claim the mathematics never made was steering where the compute went.

In a pipeline where each stage reads the previous stage's *summary* rather than
its *work*, the summary is not documentation. It is load-bearing structure, and it
deserves the same knife as the proofs.

**Then the sting in the tail.** The final reviewer scored the paper by counting
faults — and eleven of its fourteen failing rows were faults *the paper had
confessed itself*, in a table it built for the purpose. A paper that had quietly
buried the same six loose citations in prose, with no citekeys to count, would have
scored **better**. The rule cannot hand out credit for confession; that road ends
in self-congratulation. But it must never make hiding cheaper than telling. *A
fault exists* and *a fault was concealed* are two different measurements, and the
protocol was only taking the first one.

---

## 2026-07-24 — You cannot tell the truth about your own future.

The paper said: *"citation-gate NOT RUN — no citation has been independently
re-fetched by an auditor."*

That was true. Provably, checkably true at 18:55, when the sentence was written.

The citation gate ran at 19:10. By the time the reviewer reached the sentence it
was false, and the reviewer — correctly, by the letter of the rule — marked it as
an overclaim. The writer was penalised for accurately describing the world as it
stood in front of them.

It is like writing *"nobody has checked my homework yet"* on the homework. True
when your pen leaves the page. False the instant it lands on the teacher's desk.
And then you lose a mark for lying.

This is not a careless author. It is a **shape**. Any stage that reports on the
status of a gate downstream of itself is writing a sentence that is *guaranteed* to
rot — not likely to, guaranteed, because making that sentence false is the entire
job of the stages that come after. No amount of care upstream can fix it.

The repair is not a better writer. It is moving the sentence. A status line belongs
to whoever owns the status; where the pipeline makes a claim's truth depend on when
you read it, the claim belongs to the later node and the earlier node must leave the
line blank for the gate to stamp.

*(For the record of the run itself: Firoozbakht's conjecture is open. Nothing in
this attack proved it, refuted it, or produced evidence in either direction. The
build compiled cleanly with a hole exactly where the theorem should have been, and
every leg said so out loud. That part went right.)*
