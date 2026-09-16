# The reference family, and what each reference cannot read

**Purpose:** Choose a reference before pointing it at a field, by knowing what each one encodes and what it is blind to.
**Scope:** `src/engine/python/reference/`, its seven modules, and the partition choice that precedes every reading taken against them

## Contents

1. [Why the fourth column is the document](#why-the-fourth-column-is-the-document)
2. [Three kinds, not one family](#three-kinds-not-one-family)
3. [Backgrounds built by grouping](#backgrounds-built-by-grouping)
4. [The grouping family is two dimensional](#the-grouping-family-is-two-dimensional)
5. [Backgrounds built by deletion](#backgrounds-built-by-deletion)
6. [Controls, which are not backgrounds](#controls-which-are-not-backgrounds)
7. [What each reference cannot read](#what-each-reference-cannot-read)
8. [The partition is chosen before the reference and costs more](#the-partition-is-chosen-before-the-reference-and-costs-more)
9. [What a second route buys, and what it cannot](#what-a-second-route-buys-and-what-it-cannot)
10. [Sources](#sources)

## Why the fourth column is the document

A reference says what the object would look like carrying no structure of a stated kind. The reading
is the departure from it. Three of those facts are already written down for every module here: what
certainty it encodes, what background it builds, what residual it exposes.

The fourth is not written anywhere, and it decides whether a reading means anything. **A reference
blind to the structure in front of it returns a departure of zero, and a departure of zero is
indistinguishable from a field with no structure at all.** The instrument does not fail. It reports
an answer in the same form as a true one.

That is `DEVELOPMENT_RULES/MEASUREMENT.md` §16 with the projection being the reference itself. A
reference is a projection: it maps the object onto the part of it the reference can express, and
whatever lies outside that part is discarded silently. So the question to ask of a reference before
using it is the §16 question. **What does this collapse.**

The cost is measured and it is large. A domain built to carry structure at one scale reads 0.117 at
every symbol width dividing that scale and 0.450 at every width coprime to it
(`examples/proofs/posits/proof_symbol_width.py`, reported in the workbook's ledger). Those are the
same object and the same measure. The reference was chosen differently, and one of the two numbers
says there is nothing here.

## Three kinds, not one family

The directory holds seven modules and they are not seven of one thing. They are three constructions
with different epistemics, and a table treating them as one family would put a positive control in a
column meant for a background.

| kind | modules | what it does |
|---|---|---|
| grouped background | `periodic`, `self_similar`, `windowed` | partitions the positions, estimates each part from its own members |
| deleted background | `shuffles`, `ciphers` | removes a named property from the data and re-reads it |
| control | `fields`, `unselected` | supplies an object whose answer is known before the measurement |

The first two both answer *what would this look like without the structure*. They answer it by
opposite means: one builds the background up from a grouping, the other tears the structure out and
keeps what is left. The third answers a different question entirely, which is whether the instrument
is right, and it is the only kind that can make a reading WRONG instead of merely different
(`reference/fields.py`).

## Backgrounds built by grouping

The three grouped backgrounds are one operation. `reference/self_similar.py` states it: a periodic
background groups the positions congruent modulo a period and averages each group, where the group
key is a POSITION; the self-similar background groups the positions carrying the same surrounding
context, where the group key is a piece of CONTENT; and everything else is identical. The windowed
background adds the third key, which is NEARNESS.

| module | group key | a part is | the certainty it encodes |
|---|---|---|---|
| `periodic` | position modulo `P` | a phase class | a value depends on its phase and on nothing else |
| `self_similar` | the surrounding context | the centers sharing one context | a value depends on its context and on nothing else |
| `windowed` | distance from the center | the window around a position | a value depends on its neighborhood and on nothing else |

Each is the maximum entropy background under exactly one constraint, and the constraint is the
grouping. Entropy is strictly concave and a constraint fixing counts or marginals is linear, so the
constrained maximum exists at a single point and the background is solved for instead of searched
for (`reference/__init__.py`). Nothing here carries a seed, a bandwidth or a learned parameter.

**Collapsing these three into one engine taking a grouping is correct and the tree already says so.**
The note in `self_similar.py` that non-local means and a comb filter are one operation over two
groupings is that statement, written before the collapse was proposed.

## The grouping family is two dimensional

The collapse has a trap in it, and it belongs on the page before the fold happens.

The grouping is not the only thing that differs between these modules. **The central value differs
too, and it is not a style choice: it is the noise model.**

    the mean       is the first moment, and one large sample owns it
    the median     is the middle rank, and moving it takes more than half the part

`reference/windowed.py` states the consequence directly: a median rejects the replacement noise a
mean cannot, because the mean is the first moment and one impulse owns it while the median is the
middle rank and one impulse is one more vote. `reference/periodic.py` carries BOTH for that reason,
and says which is for which. Noise that ADDS to a sample leaves the phase class holding one number
plus a spread, and the least committal reconstruction fixing the class's first moment is the mean.
Noise that REPLACES a sample leaves the class holding the true value in most members and a wrong
value in a few, where fixing a first moment is the wrong background because one impulse drags the
mean off the value every clean member agrees on.

So the family is a grid, and three modules cover four of its six cells:

| | mean, for additive noise | median or consensus, for replacement noise |
|---|---|---|
| position | `periodic.mean_background` | `periodic.consensus_majority` |
| context | `self_similar.similar_background` | **empty** |
| nearness | **empty** | `windowed.window_median` |

The empty cells are real constructions with names. Nearness with a mean is the moving average, the
box filter. Context with a median is non-local medians. A repeating motif corrupted by impulses
instead of by additive noise would want exactly that, and no module here can read such an object.

**So the fold should take TWO arguments and not one.** A grouping and a central value. Done that way
the collapse does not merely remove duplication, it completes the family and the two missing cells
cost a call instead of a file. Done with the grouping alone, the noise model becomes a property of
whichever module a caller happened to pick, the same invisibility the fold is meant to
remove.

## Backgrounds built by deletion

These do not group. They take the data and remove a property, then read the same measure against
what is left.

`reference/shuffles.py` carries the reason this kind exists, and it is the strongest epistemic
statement in the directory: the results in this work that held were measured against a background
built by deleting something from the data itself, and such a background CANNOT BE WRONG about the
property it removes, because it is the same data with that property gone. The results that failed
were measured against a background that was assumed. The product rule assumed independence. The Zipf
reading assumed a memoryless process would not reproduce it.

`reference/ciphers.py` is the graded form of the same move. A shuffle deletes every arrangement at
once and gives a single floor. A cipher deletes a NAMED part of one and gives a background per part:
a substitution renames the symbols and moves nothing, so any measure reading where symbols fall must
return the same value to the last decimal; a repeating key of length `k` splits the gaps `k` ways; a
full-length pseudorandom addend is the only mapping there that erases outright.

The substitution case checks a measure instead of an object. **A measure
that changes under a pure renaming is reading the alphabet and not the arrangement**, and that is a
defect in the measure, detectable without any ground truth.

## Controls, which are not backgrounds

`reference/fields.py` and `reference/unselected.py` answer a different question and belong in a
separate column of any table.

A field built by shaping white noise in the frequency domain has its spectral exponent put in by
hand, so the answer exists before the measurement. Only that lets a reading be WRONG instead of
merely different, and the module records that every reading of the spectral exponent before these
existed was of a painting, where the plane is the only authority on what the answer should be.

`unselected.py` closes a different gap. Every corpus in this work that departs from a null
permutation was made by a person. A measure detecting arrangement and a measure detecting human
production were never separated by anything measured. The control has to be a domain with structure
and no author, and the module argues that a genome will not serve because the selection that shaped
language shaped the organism. Mathematics supplies two: the digits of a square root, and the gaps
between primes.

Neither of these is a background and neither yields a residual. A reading is taken against them to
find out whether the INSTRUMENT is right, and a family document that files them beside `periodic`
invites somebody to take a departure from a control and report it as a finding.

## What each reference cannot read

This is the column that does not exist elsewhere. Each entry is derived from the construction
instead of observed from a failure, so each is checkable by building the case named.

| reference | cannot read | why, from the construction |
|---|---|---|
| `periodic`, mean | any component constant within phase classes | such a component IS the background; the residual is zero and the target is absorbed |
| `periodic`, mean | any structure whose period divides `P` or equals it | its phase classes are unions of the true ones, so it is explained and never exposed |
| `periodic`, consensus | a phase class with no majority | the module names this its floor and breaks ties by a declared rule |
| `self_similar` | a context that never recurs exactly | a group of one carries no other evidence and is left untouched |
| `self_similar` | noise sitting on the context instead of the center | a corrupted context is a different context and matches nothing |
| `self_similar` | the edges | a center without a full context on both sides is left as it is |
| `windowed` | a signal varying inside the window | the median filter flattens it toward the local middle |
| `windowed` | corruption exceeding half the window | the middle rank moves once the impulses outvote the clean members |
| `shuffles` | anything the shuffle did not delete | it is a floor for one property and says nothing about the others |
| `ciphers`, substitution | every arrangement | it moves nothing, and a measure of arrangement must therefore be unchanged by construction |
| `fields`, `unselected` | not applicable | controls; a departure from these is a statement about the instrument |

**Two of these are the same entry from opposite sides**, and that pair is the family's clearest
demonstration. `self_similar.py` records that a periodic filter cannot read a motif recurring at
positions with no period between them, and that this one reads it because it never asked for a
period. The converse holds: a field periodic at `P` with no repeated context is invisible to
`self_similar` and exact under `periodic`. Neither is better. They are blind in different directions
and the blindness is the choice being made when a reference is picked.

**An arithmetic form of the same column, where one exists, is worth more than a description.** The
exact shift agreement detector needs two points sharing every coordinate the shift holds fixed. For
`n` points over `m` distinct slots in those held coordinates, the expected coincident pairs under
scatter is `n^2 / 2m`. A cell field of 228 points over 141 by 146 slots gives 1.26 expected and
returned zero observed, which is ordinary sampling around 1.26 and not a statement about order. The
same 228 points on a lattice of 16 columns give 1624. **Two counts and one division decide whether
the detector has anything to find, before it runs.** Where a cannot-read entry can be put in that
form it should be, because a number is checkable in advance and a sentence is checked after the
disappointment.

## The partition is chosen before the reference and costs more

Choosing the reference is the second decision. The first is the partition: the unit and the scale the
points are read at. It precedes every reference here and it is not reached by any property of them.

The measured cost is in the workbook's ledger, from
`examples/proofs/posits/proof_symbol_width.py`. A domain built so that its only arrangement is
clustering of four-byte units reads:

| symbol width | relation to the true scale | reading |
|---|---|---|
| 1, 2, 4 | divides it | 0.117, 0.119, 0.117 |
| 3, 5, 6 | coprime to it | 0.450, 0.510, 0.459 |

**The mechanism is the §16 one and it is worth stating as arithmetic.** Take a true structure of
period `Q` and a chosen width `P`. The positions fall into `P` classes by index modulo `P`. When
`P` and `Q` are coprime, every one of those classes contains every residue modulo `Q` equally often,
so each class carries the whole distribution and its mean is the global mean. The background is flat,
the residual is the original field, and the reading returns the value for no structure. Nothing broke.
The projection discarded the distinction being asked about, and returned a well-formed number.

**And the partition carries a second parameter nobody names: the offset.** At the true width of four,
the aligned slice reads 0.117 and the worst offset reads 0.309. **Misalignment at the right width
costs more than a wrong width does.** A family document that sends a reader away with the width
question answered and the phase question unasked has handed them the larger of the two errors.

**A width of one divides every scale and has no phase.** That makes the finest slice the one that
cannot hide structure, and it is the only width that is safe without knowing the answer first. Where
the scale is unknown, read at one and widen only with a reason.

## What a second route buys, and what it cannot

Every module here carries two routes to its background, sharing no code. Their agreeing is therefore
evidence: a batch sum against a Welford mean, a count against a median, a keyed dictionary against a
quadratic scan.

The rule behind that is the tree's standing one, two routes or it does not ship. **Its own
cannot-read entry is the sharpest in this document.**

Two routes that are secretly one route satisfy the rule exactly and produce agreement. The device
engine in this tree never executed: a bare `_Static_assert` does not compile under nvcc as C++,
`maint/engine/build_gpu_arm.sh` tested for the output file instead of the compiler's exit status, and
a week-old binary ran and reported the device agreeing with the portable engine at 1.01x. Every
device figure came from the portable engine measured twice.

**Agreement cannot detect a stale device, because the portable engine agrees with itself and a
working device agrees too.** The separating statistic is disagreement in the last bits: a genuine
second route reorders the arithmetic and must differ where the first rounds differently, and three
routes must differ from each other.

A second route therefore buys the detection of a defect in one of them, and buys nothing against the
case where the second route is the first one wearing a different name. Checking that the routes ARE
two is a separate act from checking that they agree, and it is the act nobody performs.

## Sources

Every claim above is read from the modules named or from the workbook's ledger, with the paths given
inline. Read state: the seven modules under `src/engine/python/reference/` were read in full. The
symbol width figures are quoted from `chapter_anchor_sift_ledger.tex`, which reports
`examples/proofs/posits/proof_symbol_width.py`; the proof itself was not run for this document.

The non-local means idea and the comb filter are prior art named in `self_similar.py` and reached
here through the tree's own construction instead of ported. The maximum entropy principle under
stated constraints is Jaynes, *Information Theory and Statistical Mechanics*, Physical Review 106(4)
and 108(2), 1957, cited from knowledge and not from a copy read for this document.

**Author:** dstroy0 (Douglas Quigg) <dquigg123@gmail.com>
**Date:** 2026-09-16
