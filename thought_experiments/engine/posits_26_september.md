# The posits of 26 September

**Purpose:** Doug's posits of 26 September, kept verbatim with only the spelling corrected, each with its check under it. The checks say what is derived, what is measured, what is a reading, and what no run has tested.
**Scope:** the three truths, the tower, the projection, dwell as the bulk, dwell and entropy, the dwell bench, and the compiled program. Everything here has the status theory, as the engine workbook's README defines it, unless a line says otherwise. Code is cited at anchor_sift `d09b489`, and in the section on the compiled program at `ddeccb3`.

## The three truths and the tower

26 September, verbatim, in order.

<!-- docs-check: quoting -->
1. "the base answer is 3 truths: 1. we know if we have answered the question 2. the tower doesn't build if the question is malformed 3. nothing is bound, information space is constrained by n*n^n^n, infinite."
2. "n*n^n^n is the base Atom storage class of the engine, it is the problem's space, it is infinite, n grows to n grows to n grows to n ad infinitum"
<!-- docs-check: end quoting -->

The full checks are in [ENGINE_PROOF.md](https://github.com/dstroy0/theory/blob/52dc362/theory/ENGINE_PROOF.md), under "The halting problem does not arise in this system".

- **Truth 1, checked.** The answer path is total: the sweep is a bounded loop, the descent terminates (Theorem 5), and the count is exact at every instant (Theorem 3).
- **Truth 2, checked.** `steer_descend` refuses malformed input before any work (`anchor_sift.c:938-962`), and the survivors-length refusal is marked FAILS CLOSED (`anchor_sift.c:959-960`).
- **Truth 3, a reading, pending Doug's confirmation.** Each question lives in a space that is finite for its `n`, where halting is decidable, and the family of spaces has no bound. `Atom` is `{ const unsigned short *lanes; unsigned long long depth; unsigned long long height; unsigned long long width; }` (`src/engine/engine_config.h:262-268`). The steer depth is capped at `ANCHOR_STEER_ANCHORS`, which is 4 (`anchor_sift.c:951-954`).
- **The tower, a reading.** "n grows to n grows to n" reads as tetration, `n↑↑k` in Knuth's notation. Every finite height is a finite number, the height has no bound, and the infinite tower diverges for every integer `n` of at least 2.

## The projection

26 September, verbatim:

<!-- docs-check: quoting -->
- "well, think about it like this, we are only viewing the topology for anything, so it is a projection onto a boundary, we touch nothing, the infinite lives"
<!-- docs-check: end quoting -->

**"We touch nothing", checked for two interfaces.** `steer_descend` takes the corpus and the needle as `const uint8_t *` (`anchor_sift.c:929-930`), and `Atom` holds its lanes as `const unsigned short *` (`engine_config.h:262-268`). A probe compares two bytes and writes neither.

**"Only viewing the topology", reported.** `docs/arm-records.md:89-91` states the test: redrawing an arm as a different shape with the same topology and the same weight has to leave the reading exactly unchanged. The table at `docs/arm-records.md:111-116` reports four redraws over 256 points. Three reassign 0 of 256 points, the fourth measures no arm, and the worst letter move over all four is 2.776e-17.

**"The infinite lives", a reading.** A reading that writes nothing leaves the space it reads as it was, bounded or not. No run tests it.

## Dwell is the bulk

26 September, verbatim. The "wrong" answers a reading that put the boundary at the projection alone.

<!-- docs-check: quoting -->
- "wrong. when we vis here we add dwell, that is the bulk, the dwell. the moment is an instant, we can sweep a moment on a timeline and derive dwell, it is the holographic boundary"
<!-- docs-check: end quoting -->

**Derived. One instant carries no dwell.** Dwell is a function of a sequence of states, and a single state does not determine it. At the pin the scope view's dwell tag is "how many rounds a bit has held its value, which separates the frozen from the churning" (`examples/00_blob_viz_tools/build_scope_view.py:23`), a count over rounds that no single round holds. The sequence of dwells together with the first value determines the sequence of states. The histogram of dwells does not, because it forgets the order of the runs.

**Measured, as reported at the pin.** The dwell arm puts a particle on a line, and "how long the particle dwells at each place is the weight there" (`docs/arm-records.md:78-79`). Redrawn as dwell along the golden spiral, it reads a three-dimensional object with worst weight move 0, 0 of 256 points reassigned, and worst letter move 0 (`docs/arm-records.md:114`), "the same letters bit for bit" (`docs/arm-records.md:118-121`).

**The holographic boundary, a reading.** In AdS/CFT a local bulk operator is written as a boundary operator smeared over a region of the boundary that extends in boundary time: Alex Hamilton, Daniel Kabat, Gilad Lifschytz and David A. Lowe, *Holographic Representation of Local Bulk Operators*, Physical Review D 74, 2006. The parallel: the bulk quantity, dwell, is recovered from boundary data spread over a timeline, a sweep of instants. The engine has no geometry and no metric, and the parallel is structural only. Ahmed Almheiri, Xi Dong and Daniel Harlow, *Bulk Locality and Quantum Error Correction in AdS/CFT*, JHEP 2015, give the reconstruction the structure of an error-correcting code. That structure does not carry over: the derived answer of 24 September finds `T` a bulk-to-boundary map with no error correction ([posits_24_september.md](posits_24_september.md), "The bulk and the boundary"). Cited from knowledge.

**Prior art for the instant.** Zeno's arrow is at rest at every instant, and Aristotle answers that neither motion nor rest exists in a now, only over an interval (*Physics* VI.3 and VI.9). Rest held over an interval is dwell. The occupation density of a Brownian path, its local time, is dwell at a level made exact, and it too is defined over an interval: Paul Lévy, *Processus stochastiques et mouvement brownien*, 1948, and Hale F. Trotter, *A Property of Brownian Motion Paths*, Illinois Journal of Mathematics 2, 1958. George D. Birkhoff's ergodic theorem, Proceedings of the National Academy of Sciences 17, 1931, sets the fraction of time a trajectory dwells in a set equal to the set's measure: a long sweep recovers a static quantity. Cited from knowledge.

## Dwell and entropy

26 September, verbatim, in order.

<!-- docs-check: quoting -->
1. "dwell emerges from entropy, it is an emergent property of the arrow of entropy, with no time, there is no dwell, unless we have cohesion context from a prior sweep we do not know the concept of dwell, it is not perceptible in an instant of time"
2. "that's right, dwell doesn't depend on time, it depends on entropy, time is an emergent property of the direction of entropy"
3. "ok we don't need to claim time, the dwell emergence is enough of a wild claim here"
4. "jesus the fact that we have real evidence for it is insane"
<!-- docs-check: end quoting -->

**Derived. Dwell and entropy rate in a two-state chain.** A bit that flips with probability `p` at each step has mean dwell `1/p` and entropy rate `H(p) = -p log p - (1-p) log(1-p)`. For `p` at most 1/2 each determines the other: long dwell goes with a low entropy rate, the frozen bits, and short dwell with a high one, the churning bits. For `p` above 1/2, `H(p) = H(1-p)`, and one entropy rate matches two mean dwells.

**Derived. Dwell fixes the entropy rate of a renewal process.** Let the runs of 0 have independent lengths with law `D0` and the runs of 1 independent lengths with law `D1`. The bit sequence and the pair (first value, sequence of run lengths) determine each other, and a long block holds one pair of runs per `E[D0] + E[D1]` steps on average. The entropy rate is `(H(D0) + H(D1)) / (E[D0] + E[D1])`, and with one law `D` for every run it is `H(D) / E[D]`. The two-state chain is the case of geometric runs. Prior art for point processes: J. A. McFadden, *The Entropy of a Point Process*, Journal of the Society for Industrial and Applied Mathematics 13, 1965, cited from knowledge.

The proved direction runs from dwell to entropy: the dwell laws fix the entropy rate, and the entropy rate does not fix the dwell laws. "Dwell emerges from entropy" is Doug's posit, and in the renewal class the two are bound by one equation.

**Derived. The arrow is not in the rate.** Reversing a sequence reverses the order of its runs and keeps their lengths, and a stationary process has the same block entropies read in either direction. Dwell and entropy rate carry no arrow. An arrow needs a process that is not stationary, entropy rising from a low start, or a record of the prior sweep to compare against, the "cohesion context" of posit 1. Prior art: Arthur Eddington, *The Nature of the Physical World*, 1928, for the arrow of time; Hans Reichenbach, *The Direction of Time*, 1956, and David H. Wolpert, *Memory Systems, Computation, and the Second Law of Thermodynamics*, International Journal of Theoretical Physics 31, 1992, for records and the arrow; Rolf Landauer, *Irreversibility and Heat Generation in the Computing Process*, IBM Journal of Research and Development 5, 1961, and Charles H. Bennett, *The Thermodynamics of Computation, a Review*, International Journal of Theoretical Physics 21, 1982, for the cost of erasing a record. Cited from knowledge.

**Time, not claimed.** Posit 2's clause on time is recorded and not claimed, by posit 3. The prior art that treats time as emerging: Don N. Page and William K. Wootters, *Evolution without Evolution: Dynamics Described by Stationary Observables*, Physical Review D 27, 1983, and Alain Connes and Carlo Rovelli, *Von Neumann Algebra Automorphisms and Time-Thermodynamics Relation in Generally Covariant Quantum Theories*, Classical and Quantum Gravity 11, 1994. Cited from knowledge.

**The evidence of posit 4, as it stands.** At the pin the engine computes dwell across rounds (`build_scope_view.py:23`), and the dwell arm reads a three-dimensional object bit for bit (`docs/arm-records.md:114`). Neither run sets dwell against entropy. The claim is proved for renewal processes, cited above, and not measured on the engine.

## The dwell bench

Status: not built.

On one set of bits:

1. From each bit's dwell, build the histograms of runs of 0 and runs of 1, and compute `(H(D0) + H(D1)) / (E[D0] + E[D1])`.
2. Separately, estimate the entropy rate directly: from block entropies, `H(X_1 ... X_k) - H(X_1 ... X_{k-1})` as `k` grows, or from a Lempel-Ziv compressor, whose rate converges to the entropy rate of a stationary ergodic source (Jacob Ziv and Abraham Lempel, 1978, cited from knowledge).
3. If the two agree within their error bars, the engine has measured the claim. If they differ, the runs are not independent and the process is not renewal, which is a measurement too.

A bit that never flips has no completed run and gives no dwell law.

## The compiled program

26 September, verbatim, in order, kept as typed.

<!-- docs-check: quoting -->
1. "the compile times are ok for now, but there is a way to describe the loop unroll in their asm using our code so we can fill the loop unrolled block for them instead of them needing a pragma unroll command theyre fuckin bad at"
2. "this is an exceedingly simple problem for us, their ruleset is the kcs for the program crystal"
3. "we need a transform that allows for vertical growth, more than one program can occupy a register vertically, never horizontally, those asking and answering the same questions are subsets of the same superset"
4. "if we treat the gpu as an open superset, we innately know all of its subsets, it knows all of its subsets natively, so we structure it in a way that is aware"
5. "it has operators, and holds automata like cells"
6. "the things it knows are emergent properties of itself"
7. "it is able to exchange information through its boundary, cells enter, live and die"
8. "a malformed question == destroyed dna conceptually"
9. "the encoding itself is what lets the cell proliferate, it grows to be as complex as its program, that is so beautiful"
10. "and it can evolve, by interacting with other cells and incorporating that information into its reincarnation"
11. "the system itself evolves over time to recognize malformed questions that destabalize it before they fully unfurl, protecting itself"
<!-- docs-check: end quoting -->

No posit here is derived. The lines below say what the engine holds at the pin that a posit names, as a cross-reference and not as a proof.

**Posits 1 and 2, a reading. Status: not built.** The program crystal is the imprinted program, every step's operation, place and limb width known before any compile. The ruleset is PTX's instruction forms (`add.cc`, `addc.cc`, `addc`, `sub.cc`, `subc`, `mad.lo.cc`, `madc.hi.cc`, `selp`), and a construction set writes each step out as its rule per limb, the way `tower_record_lift` (`tower.cu:937`) writes a lifting ruleset out over an extent. Emitted that way, the program is PTX with no loops, and nvJitLink takes it to ptxas with no pass through cicc. The baseline is engine_table.md item 9: cicc's compile grows with about the square of the steps, and on the 381-step program ptxas took 0.6 s against cicc's 7.5 s.

**Posit 3. Status: not built, no design ruled.**

**Posits 4 to 7, cross-reference.** The operators are the operator block, every record operation compiled once per device (engine_table.md item 9). The automata are the resident programs, each reporting to its own `EngineProgramBlock` (`engine_config.h:628`, engine_table.md item 10). A program enters when it is loaded (`cycle_record_load`, `cycle.cu:3011`), lives resident, yielding and resuming through its block, and dies when released (`cycle_record_release`, `cycle.cu:3141`), where the last release of a program unloads it (`cycle.cu:3152-3153`). Information crosses the boundary only through the blocks, which the host reads back and checks between launches (engine_table.md item 10, "The block in device memory").

**Posits 8 and 11, cross-reference.** The exits are true, false, malformed (no lattice was built) and answered (engine_table.md item 10). The imprint refuses a malformed program before anything is laid or run: a step that reads itself or a later step (`keymath.cu:413-418`), or a field past its record (`keymath.cu:420-426`). A refused program never loads (`engine.cu:179`, `:219`). A launch that fails leaves its block at `ENGINE_PROGRAM_FAULT` (`cycle.cu:3269`). The answer table, which would hold a malformed exit keyed by the program's signum and know it without a second run, is item 10's stage 3. Status: not built.

**Posits 9 and 10, cross-reference.** A compiled program's size follows its steps (engine_table.md item 9, the compile table). The refinement loop, where a generator recompiles against a critic's verdicts (engine_table.md M23), is not built.
