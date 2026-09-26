# Directions the engine's own proofs already license

**Purpose:** Collect the capabilities that follow from what the engine has already proved, and the Laplacian construction that replaces its planning cost with a determinant.
**Scope:** `src/engine/nbody/anchor_sift/anchor_sift.{h,c}`, `docs/steering.md`
**Note, 26 September:** this scope first named `src/engine/c/portable/anchor_steer.{h,c}` and `src/engine/c/portable/anchor_sift.h`. Commit `510577b` (16 September) folded the steer files into `anchor_sift.{h,c}`, `0954259` (16 September) renamed `src/engine/c/portable/` to `src/engine/c/engine/`, and `bdaed61` (24 September) moved those to `src/engine/nbody/anchor_sift/`, where they are at anchor_sift `1948ae1`.
**Note, 26 September:** the README citations below first read `README.md:97`, `README.md:99` and `README.md:105`. At anchor_sift `d09b489` those lines are `README.md:138`, `README.md:140` and `README.md:150`, and the citations now give those.

## Contents

1. [What is already proved](#what-is-already-proved)
2. [It can search a corpus it cannot read](#it-can-search-a-corpus-it-cannot-read)
3. [It can search for a pattern nobody wrote down](#it-can-search-for-a-pattern-nobody-wrote-down)
4. [It is correct on hardware that computes wrong](#it-is-correct-on-hardware-that-computes-wrong)
5. [Its own cost is the arrangement measurement](#its-own-cost-is-the-arrangement-measurement)
6. [The census matrix is a Gram matrix](#the-census-matrix-is-a-gram-matrix)
7. [Probe selection is a determinantal point process](#probe-selection-is-a-determinantal-point-process)
8. [The destroy rule is a rank test](#the-destroy-rule-is-a-rank-test)
9. [Planning without touching the corpus](#planning-without-touching-the-corpus)
10. [What each of these would cost to be wrong](#what-each-of-these-would-cost-to-be-wrong)
11. [Sources](#sources)

## What is already proved

Three results carry everything below. All three are established, and none is proposed.

Every probe is a necessary condition of an occurrence, any conjunction of probes loses no true
occurrence and the full compare removes the false survivors. The count is exact for any probe set.

The sift is therefore a sound filter and its errors are one directional. A discrepancy is always an
over-count and is detectable without knowing the answer (`README.md:138`).

A probe's agreement at shift `d` from a true occurrence is exactly a lag `d` self-agreement event in
the corpus. Writing `A(d)` for the fraction of positions where the corpus agrees with itself at lag
`d`, the probability a probe fails to refute a wrong alignment at shift `d` is `A(d)`.

## It can search a corpus it cannot read

A probe compares `corpus[at + offset]` against `needle[offset]` and uses the result of an equality.
It never reads a value for any other purpose, never orders two symbols, and never indexes a table by
one.

So let `f` be any injective function on the alphabet. Applying `f` to every byte of both the corpus
and the needle preserves every equality and reverses none. Every probe returns what it returned
before, every survivor set is identical, and the count is unchanged. Take `f` to be a keyed
pseudorandom permutation and the engine counts occurrences in a corpus nobody running it can read,
for a needle nobody running it can read.

This is a capability the alphabet claim already bought. `README.md:140` states that nothing is
indexed and no table is built over the alphabet, and that a real-valued or unenumerable alphabet
costs nothing. An engine that never enumerates symbols cannot notice that the symbols were replaced.

**What it leaks is known exactly, and that is the unusual part.** Two positions carrying the same
byte still carry the same byte after `f`. The encoding hides values and preserves the equality
pattern, an observer learns the partition of positions into equal classes and nothing finer. That
partition is the quantity `A(d)` measures. The leak of this construction is the statistic section 5
uses as a signal, and the engine can compute and report its own exposure.

**Derived, 26 September. What the leak allows.** A keyed permutation applied byte by byte is a
monoalphabetic substitution. The equality partition it leaves visible carries every symbol's
frequency, and on natural data frequency analysis recovers the text from that alone, as al-Kindi
described in the ninth century. Deterministic encryption and the attacks on it are the modern form:
Mihir Bellare, Alexandra Boldyreva and Adam O'Neill, *Deterministic and Efficiently Searchable
Encryption*, CRYPTO 2007, and Muhammad Naveed, Seny Kamara and Charles V. Wright, *Inference Attacks
on Property-Preserving Encrypted Databases*, CCS 2015. The engine counts without being given the
plaintext. Whether an observer can read the corpus depends on the data, and "cannot read" holds only
where the equality pattern does not give the values away. Cited from knowledge.

## It can search for a pattern nobody wrote down

Nothing in the necessary-condition argument requires one side of a probe to be the needle. It
requires that an occurrence satisfy the probe.

Put both ends in the corpus. A probe testing `corpus[at + o1] == corpus[at + o2]` asks whether the
window at `at` carries the same symbol at two of its own positions. A set of such probes specifies a
pattern by its internal equalities alone: which positions must agree, with no statement about what
they must agree on. Any window matching that partition satisfies every probe. The filter stays
sound and the count stays exact under the same argument.

The search target becomes a shape. `abcabc` and `xyzxyz` satisfy the same equality pattern, and so
does any string of that form over any alphabet. This is the parameterized matching problem, and the
engine reaches it without new machinery because its primitive was equality all along.

What that finds: repeated structure with substitution, source code duplicated with the identifiers
renamed, tandem repeats in a sequence, the internal structure of a cipher's output. The verifier
changes, since a full compare against a needle is the wrong final test when there is no needle, and
the replacement is a direct check of the partition on the surviving windows.

**Derived, 26 September. The verifier must check inequalities.** Equality probes test only which
positions agree. A window whose partition is coarser passes them too: `aaaaaa` satisfies every probe
that `abcabc` implies. Baker's parameterized match requires a bijective renaming, and the partition
check on the survivors has to confirm that positions in different classes differ, as well as that
positions in one class agree. With that check the count is exact for the parameterized match. Prior
art: Brenda S. Baker, *A Theory of Parameterized Pattern Matching: Algorithms and Applications*, STOC
1993, cited from knowledge.

## It is correct on hardware that computes wrong

Any probe may be replaced by a weaker necessary condition without endangering the count. The weakest
available is the probe that always agrees, and a plan of those is the empty plan, which sends every
alignment to the full compare and returns the exact answer at maximum cost.

A probe implementation whose errors run only toward agreement is therefore sound. A comparator that returns
agreement when it is uncertain, a memory that occasionally reads a stale byte in the direction of a
match, a voltage-starved ALU biased that way: each costs full compares and none costs an occurrence.
The requirement is a biased failure mode. Reliability is never needed, and a comparator can be built
to fail in the safe direction deliberately.

The converse is the thing to guard. An error toward disagreement drops a true occurrence silently,
the single failure this engine does not otherwise have. The design rule follows: every component of a
probe must be auditable for the DIRECTION of its errors, and the sound filter argument survives
unreliability in that one direction alone.

**Derived, 26 September. The argument covers the probes.** A probe error toward agreement costs a
full compare. The full compare itself has to be exact: a verifier that errs toward agreement admits
false survivors, and the count runs high. A filter with one-sided error and an exact check behind it
is the arrangement of Burton H. Bloom, *Space/Time Trade-offs in Hash Coding with Allowable Errors*,
Communications of the ACM 13(7), 1970, cited from knowledge.

## Its own cost is the arrangement measurement

`README.md:150` records an open problem. Collision entropy is permutation invariant and cannot see an
arrangement, and a period-16 counter therefore reads 4.0 bits while the dispatcher calls a perfectly structured
corpus memoryless. The note states that fixing it needs a quantity that reads arrangement and that no
threshold on collision entropy reaches it.

The engine already measures that quantity and reports it in another column.

A short-circuiting conjunction of `k` probes, each agreeing on a wrong alignment with probability
`q`, reads `(1 - q^k) / (1 - q)` bytes per alignment in expectation. The bench prints reads per
alignment. Inverting the expression recovers `q` from a number already on the page.

Now take the prediction. Under a memoryless source the agreement probability is the collision
probability, `q = 2^-H2`, computed from the histogram the dispatcher already builds. So there are two
estimates of one quantity: one predicted from the histogram, one measured from the clock. The
histogram is permutation invariant and the measurement is not, because the measurement is an average
of `A(d)` over the shifts the search actually visits.

**The gap between predicted `q` and measured `q` is arrangement information, and it is the quantity
`README.md:150` says is needed.** A permuted corpus has an identical histogram and a different
measured `q`. The period-16 counter reads `2^-4` predicted and close to 1 measured, the largest gap
the statistic can show.

**Derived, 26 September. The period-16 counter under both readings.** A period-16 counter agrees
with itself at lag `d` exactly when 16 divides `d`. For a needle taken from the corpus, an alignment at a
multiple of 16 from an occurrence is itself an occurrence, and every other alignment is refuted by
its first probe.

*Reading one, the inverted formula.* Take `k = 4` and count probe reads only. Fifteen alignments in
sixteen read 1 byte and one in sixteen reads 4, for `19/16` reads per alignment. Solving
`1 + q + q^2 + q^3 = 19/16` gives `q ≈ 0.158`. Over wrong alignments alone the measured `q` is 0.

*Reading two, the conditional rate.* Once one probe agrees, every later probe agrees, and the rate
after one agreement is 1.

The section defines measured `q` as the value the inverted formula returns, and the sentence can
mean only reading one. "Close to 1" matches reading two alone, and the reads column does not return
reading two. "The largest gap the statistic can show" fails under either. Under reading one the
measured value is 0.158, or 0 on wrong alignments, against a range that reaches 1. Under reading two
a period-256 counter shows 1 against `2^-8`, a wider gap than 1 against `2^-4`. The section's
broader claim holds in the other direction: this arrangement refutes wrong alignments faster than
the histogram predicts, and predicted and measured `q` differ.

Two conditions on reading one. The descent as built would place one probe on this corpus: after the
first probe the survivors are exactly the occurrences, no further probe prunes, and the destroy rule
fires. With `k = 1` the formula gives 1 read per alignment for every `q` and returns no value. And if
the reads column also counts the full compare, `m` bytes on each occurrence, reads per alignment are
`(19 + m)/16`, and the inverted value reaches 1 at `m = 45`. A value near 1 then measures
verification, the failure section 10 names.

It costs nothing. Both numbers are already collected, and the second one is the reads column of a
table that is currently read as a performance result.

## The census matrix is a Gram matrix

This section changes the planner, and it starts with a fact that has to be checked instead of
assumed.

Write each corpus position as a one-hot vector over the alphabet: `x_i` has a single 1 in the
coordinate for `corpus[i]`. Then `corpus[i] == corpus[j]` exactly when the inner product of `x_i` and
`x_j` is 1, and it is 0 otherwise. So

```
A(d) = (1/n) * sum over i of <x_i, x_{i+d}>
```

which makes `A` an autocorrelation of a vector-valued sequence. A function of that form is positive
semidefinite. For any offsets `o_1 ... o_k` the matrix `M` with `M_ij = A(o_i - o_j)` is positive
semidefinite. It is a Gram matrix, and its entries are counts over a shared denominator.

Write `C` for the same matrix before dividing. `C_ij` counts the positions where the corpus agrees
with itself at lag `o_i - o_j`. `C` is an integer matrix.

**Derived, 26 September. The convention decides whether `C` can be singular.** `C_ij` as defined
counts over the overlap of the corpus with its own shift, with no wraparound. Write `v_o` for the
one-hot sequence shifted by `o` and padded with zeros. `C_ij` is the inner product of `v_{o_i}` and
`v_{o_j}`. Take distinct offsets in rising order. At position `o_j`, the vector `v_{o_j}` carries
`x_0`, which is nonzero, and every vector with a larger offset is zero there. Each coefficient of a
vanishing combination is zero in turn, the vectors are independent, and `C_S` is positive definite.
An integer positive definite matrix has determinant at least 1. Under this convention `det(C_S)` is
never 0 for distinct offsets. Under a cyclic convention it can be: in a period-16 counter whose
length is a multiple of 16, the shifts by 0 and by 16 are the same vector, and a needle longer than
16 admits both offsets.

The discrete form of Bochner's theorem is Herglotz's (Gustav Herglotz, 1911): a sequence is positive
semidefinite exactly when it is the sequence of Fourier coefficients of a positive measure on the
circle. For the sample autocovariance with the shared denominator `n`, positive semidefiniteness is
a standard property in Peter J. Brockwell and Richard A. Davis, *Time Series: Theory and Methods*,
1991. Cited from knowledge.

## Probe selection is a determinantal point process

A determinantal point process assigns a subset `S` probability proportional to `det(M_S)`, and it is
the standard construction for selecting a set whose elements are not redundant with each other.
Determinantal processes discourage redundant elements and favor spread-out, repulsive selections,
which is a restatement of what a probe set is supposed to be.

Under the Gram matrix above, `det(M_S)` is the squared volume spanned by the probe positions' corpus
embeddings. It is zero when they are linearly dependent, the fully redundant case, and
largest when they are as independent as the field allows. So:

**Choosing the probe set that maximizes `det(C_S)` is choosing the least redundant set, and it is the
mode of a determinantal point process whose kernel the census already contains.**

Two classical facts make this more than an analogy. The marginal gain from adding a probe `p` to a
set `S` is `det(C_{S+p}) / det(C_S)`, the Schur complement of `p` against `S`, known in statistics as
the leverage score and in the graph Laplacian setting as the effective resistance. Sampling by effective
resistance is exactly how spectral sparsification builds a small subgraph preserving every cut of the
original within a factor, and every undirected graph admits such a sparsifier with a near-linear
number of edges. A greedy descent that scores candidates by determinant is running the sparsification
rule, and the approximation guarantees of that literature attach to it.

**Derived, 26 September. What the marginal is, and what does not attach.** The ratio
`det(C_{S+p}) / det(C_S)` is the Schur complement `C_pp - C_pS C_S^-1 C_Sp`. In the Gram picture it
is the squared distance from `v_p` to the span of the vectors in `S`, and for a Gaussian with
covariance `C` it is the variance of `p` conditioned on `S`. It is not the statistics score the
paragraph above names. That score is a diagonal entry of the hat matrix `X (X^T X)^-1 X^T`, lies
between 0 and 1, and measures a row against the whole design, where the Schur complement measures a
residual against a chosen subset.

`C` is not a Laplacian. A Laplacian has nonpositive entries off the diagonal, rows summing to zero,
and a zero eigenvalue. `C` has nonnegative counts off the diagonal, and under the convention of
section 6 every `C_S` is positive definite. Effective resistance belongs to a Laplacian: the Schur
complement of a graph Laplacian onto two nodes `u` and `v` is the Laplacian of a single edge, and the
effective resistance between `u` and `v` is the reciprocal of that edge's weight. The word
"Laplacian" in the purpose line and in section 10 takes the same correction.

Sparsification by effective resistance samples edges at random with probability proportional to
weight times resistance, reweights the edges it keeps, and guarantees with high probability that the
quadratic form of the sparse graph stays within a factor of the original. The construction of Batson,
Spielman and Srivastava is deterministic and chooses and reweights edges by a barrier potential.
Neither selects `k` items to maximize a determinant, and their guarantees bound the spectrum of a
reweighted sum, not the volume of a chosen subset. A descent that scores by determinant is greedy
volume maximization, and its guarantee is its own: greedy selection reaches at least `1/k!` of the
largest volume, volume being the square root of `det(C_S)` (Ali Çivril and Malik Magdon-Ismail,
Theoretical Computer Science 410, 2009). Choosing `k` indices to maximize `det(C_S)` is the maximum
entropy sampling problem (Michael C. Shewry and Henry P. Wynn, Journal of Applied Statistics 14,
1987), and it is NP-hard (Chun-Wa Ko, Jon Lee and Maurice Queyranne, Operations Research 43, 1995).
The mode reading holds and has its own references: Alex Kulesza and Ben Taskar, *Determinantal Point
Processes for Machine Learning*, Foundations and Trends in Machine Learning 5, 2012, and Jennifer
Gillenwater, Alex Kulesza and Ben Taskar, *Near-Optimal MAP Inference for Determinantal Point
Processes*, NIPS 2012. Cited from knowledge.

**And the arithmetic is integer.** `C` is a matrix of counts. Its determinant is an integer, computed
exactly by fraction-free Gaussian elimination in the tree's own limb arithmetic, with no float
anywhere. The tree's ban on computing libraries does not bite here, because the construction was
never floating point to begin with.

This also subsumes the Sidon set proposal. A Sidon set maximizes the distinctness of pairwise
differences, which is a combinatorial proxy for spreading the probes across lags. The determinant is
the same objective measured against the actual field instead of assumed, and it answers with the
field's own counts.

## The destroy rule is a rank test

The destroy rule fires when the best candidate leaves the truthy population exactly as it found it.
In the determinant picture, a probe that prunes nothing contributes no volume. The determinant
does not grow and the marginal `det(C_{S+p}) / det(C_S)` is at its floor. The rule is a rank
deficiency test, and the determinant is its quantitative form: the rule reports a Boolean, and the
determinant reports how far from redundant each candidate is.

**The two are not identical and the difference must be stated.** Pruning nothing is a logical
implication, that every alignment surviving `S` also survives `p`. Contributing no volume is a linear
dependence. Linear dependence relaxes the Boolean condition instead of matching it, and a
determinant can therefore be positive where the destroy rule still fires. The determinant is a
surrogate, it is computable in closed form, and the destroy rule remains the exact test.

That is the right division of labor. The determinant predicts redundancy before paying for it, the
destroy rule catches what the prediction missed, and the gap between them is measurable.

**Derived, 26 September. The heading and the section disagree.** The second paragraph is correct,
and the heading does not follow from it. Under the convention of section 6, `det(C_S)` is at least 1
for distinct offsets, every marginal is positive, and the rank never drops. A rank test on `C` never
fires. The period-16 counter of section 5 shows the distance between the two. Take offsets 0 and 1.
The corpus agrees with itself at lag 1 nowhere, `C_S` is diagonal, and the marginal of the second
probe is `n`, the largest it can be. After the first probe the survivors are the occurrences, the
second probe prunes nothing, and the destroy rule fires.

The two tests also read different inputs. `C` is built from the corpus and the offsets and does not
read the needle. The destroy rule reads the survivors, and the survivors depend on the needle bytes at
the probes. On the same counter, a needle of all zeros survives offset 0 only where 16 divides the
alignment and offset 1 only where it leaves remainder 15, the second probe prunes every survivor, and
the rule keeps it. `C` is the same for both needles.

## Planning without touching the corpus

The guide records that planning cost is stated and unmeasured, and that `anchor_steer_sweep_probes`
performs about `wanted * needle_len^2 * max_length^2 * alignments / sample_stride` byte comparisons
at worst, which exceeds the scan it plans for on any but a short needle. That is the engine's
sharpest open cost problem, and the determinant removes its dominant factor.

Scoring a candidate today walks the alignments. Scoring a candidate by determinant walks a `k` by `k`
integer matrix, where `k` is at most `ANCHOR_STEER_ANCHORS`, which is 4. The corpus is touched once,
by the census that collects `C`, and never again during planning.

| | corpus touched during planning | per-candidate cost |
|---|---|---|
| survivor scoring, today | once per candidate | alignments / sample_stride |
| determinant scoring | never | a 4 by 4 integer determinant |

The census pass that collects `A(d)` for the needed lags is one pass over the corpus, the same order
as a single scan. Everything after it is arithmetic on counts.

**The falsifiable claim, stated so it can fail.** A probe set chosen by determinant reads no more
bytes per alignment than the set chosen by survivor scoring, on the same field, while costing a
planning pass that does not scale with the alignment count. If the determinant set reads measurably
more, then the linear relaxation is losing what the Boolean test keeps, and the result is that
survivor scoring carries information the Gram matrix does not. That outcome is worth as much as the
other one and costs one bench.

**Derived, 26 September. The census is not one scan.** The candidates are every needle position
(`anchor_sift.c:1089` at `d09b489`), and the lags between candidates cover every value in `(-m, m)`.
`A` is symmetric, and the census needs `A(d)` for `d` in `[0, m)`. Collected position by position,
that is one pass making about `n * m` byte comparisons, `m` times the comparisons of a single scan.
The per-candidate column of the table holds. The sentence placing the census at the order of a
single scan does not.

Because `C` does not read the needle (the note under section 8), determinant scoring picks the same
offsets for every needle of a given length. *A needle-aware Gram, a reading.* A Gram matrix of agreement
indicators, one vector per candidate offset `o` with entry 1 at alignment `s` when
`needle[o] == corpus[s + o]`, is positive semidefinite and reads the needle. Its entries count the
alignments where two probes agree together, the joint event the destroy rule tests. Building it costs
`m` comparisons per alignment for each needle, the order of one round of survivor scoring over every
candidate.

## What each of these would cost to be wrong

No section above changes the count, because no section alters what a probe is. The
encoding section applies a bijection to both sides, the pattern section replaces the needle with a
partition and replaces the verifier to match, the hardware section weakens probes in the sound
direction, and the three Laplacian sections change only which probes get chosen. Every one of them
sits inside the necessary-condition family, and the worst outcome available to any of them is a slower
search.

Two of them could be wrong as CLAIMS instead of as code, section 5 and section 9. Section 5
asserts that the gap between predicted and measured agreement reads arrangement, which fails if the
measured reads column is dominated by something other than agreement probability. Section 9 asserts
the determinant is a useful surrogate, which fails if the relaxation discards what the planner needs.
Both are one bench each, and both were written to be checkable instead of agreeable.

**Derived, 26 September. Three more fail as claims.** Section 5's sentence on the period-16 counter,
section 7's statement that the sparsification guarantees attach to a determinant-scored descent, and
section 8's heading. The notes under those sections give each. None touches the count: every probe
set is still a set of necessary conditions, and the full compare still removes the false survivors.

## Sources

Nothing in sections 6 through 9 originates here. Read state: every entry is cited from knowledge and
from the search results listed, with no paper read in full for this document. Bibliographic fields
were checked against those results, and theorem numbers deliberately are not given.

**Gustav Kirchhoff**, 1847, for the matrix-tree theorem: the number of spanning trees of a graph is a
cofactor of its Laplacian. A determinant counts the structures, and that is the oldest instance of
the move section 7 makes.
<https://people.orie.cornell.edu/dpw/orie6334/Fall2016/lecture13.pdf>

**Odile Macchi**, *The Coincidence Approach to Stochastic Point Processes*, 1975. Determinantal point
processes, introduced to model fermions in optical beams. The determinantal process is the repulsive
one and the permanental process is the attractive one, and repulsion is the property section 7 wants:
a determinantal process discourages redundant elements by construction instead of by a rule added
afterwards.

**Daniel A. Spielman and Nikhil Srivastava**, on sampling edges by effective resistance, and
**Joshua Batson, Daniel A. Spielman and Nikhil Srivastava**, on sparse sums of positive semidefinite
matrices. Every undirected graph admits a sparsifier preserving every cut within a multiplicative
factor, with a near-linear number of edges. The greedy determinant scoring of section 9 is that
sampling rule, and their guarantees are what a scored descent would inherit.
<https://arxiv.org/pdf/0803.0929>, <https://arxiv.org/pdf/0808.0163>

**Correction, 26 September.** The links above first read `arXiv:0808.4134` and `arXiv:1107.0088`.
Checked on arXiv on 26 September: `0808.4134` is Daniel A. Spielman and Shang-Hua Teng, *Spectral
Sparsification of Graphs*, and `1107.0088` is Marcel K. de Carli Silva, Nicholas J. A. Harvey and
Cristiane M. Sato, *Sparse Sums of Positive Semidefinite Matrices*. The papers the entry names are
`0803.0929`, Spielman and Srivastava, *Graph Sparsification by Effective Resistances*, and
`0808.0163`, Batson, Spielman and Srivastava, *Twice-Ramanujan Sparsifiers*, both checked on arXiv on
26 September, and the links now give those. The entry's last two sentences do not hold: by the note
under section 7, determinant scoring is not that sampling rule, and those guarantees do not attach.

**Salomon Bochner**, for the theorem that a positive definite function is the transform of a positive
measure. That is the general form of the fact section 6 establishes directly: an autocorrelation is
positive semidefinite, and the census matrix is therefore a Gram matrix.
**Note, 26 September:** for sequences on the integers the form is Herglotz's theorem, 1911 (the note
under section 6).

**Simon Sidon**, 1932, and **Paul Erdős and Pál Turán**, 1941, for `B2` sets, whose pairwise
differences are all distinct. **James Singer**, 1938, for perfect difference sets. These are the
combinatorial ancestors of the probe-spreading argument, and section 7 replaces them with a
measurement against the field.

**Edwin T. Jaynes**, *Information Theory and Statistical Mechanics*, Physical Review 106(4), 1957,
pages 620 to 630, and the sequel at 108(2), 1957. The maximum entropy principle this whole tree is
built on, and the reason a determinantal process is the right object: it is the maximum entropy
distribution carrying prescribed marginals with negative correlation.

**Correction, 26 September.** The entry gives no source for its last claim, that a determinantal
process is the maximum entropy distribution carrying prescribed marginals with negative correlation,
and the claim stands unsupported. The link between determinants and entropy that holds is Gaussian:
a Gaussian with covariance `Σ` has differential entropy `(1/2) log det(2πeΣ)`, and choosing the
subset `S` that maximizes `det(C_S)` is choosing the most entropic Gaussian marginal, the maximum
entropy sampling problem of Shewry and Wynn (the note under section 7).

**Robert S. Boyer and J Strother Moore**, 1977, and **R. Nigel Horspool**, 1980, for the shift rules
that skip alignments without reading them. That is the prior art the companion document measures the
engine against.

Further reading used for the determinantal material:
<https://arxiv.org/pdf/2204.02570>, <https://arxiv.org/html/math/0204325v1>

**Added 26 September.** The dated notes above cite these, each from knowledge, with no paper read in
full.

- al-Kindi, ninth century, on frequency analysis of substitution ciphers.
- Mihir Bellare, Alexandra Boldyreva and Adam O'Neill, *Deterministic and Efficiently Searchable
  Encryption*, CRYPTO 2007.
- Muhammad Naveed, Seny Kamara and Charles V. Wright, *Inference Attacks on Property-Preserving
  Encrypted Databases*, CCS 2015.
- Brenda S. Baker, *A Theory of Parameterized Pattern Matching: Algorithms and Applications*, STOC
  1993.
- Burton H. Bloom, *Space/Time Trade-offs in Hash Coding with Allowable Errors*, Communications of the
  ACM 13(7), 1970.
- Gustav Herglotz, 1911, on positive definite sequences and positive measures on the circle.
- Peter J. Brockwell and Richard A. Davis, *Time Series: Theory and Methods*, 1991.
- Ali Çivril and Malik Magdon-Ismail, on greedy selection of a maximum volume submatrix, Theoretical
  Computer Science 410, 2009.
- Michael C. Shewry and Henry P. Wynn, *Maximum Entropy Sampling*, Journal of Applied Statistics 14,
  1987.
- Chun-Wa Ko, Jon Lee and Maurice Queyranne, *An Exact Algorithm for Maximum Entropy Sampling*,
  Operations Research 43, 1995.
- Alex Kulesza and Ben Taskar, *Determinantal Point Processes for Machine Learning*, Foundations and
  Trends in Machine Learning 5, 2012.
- Jennifer Gillenwater, Alex Kulesza and Ben Taskar, *Near-Optimal MAP Inference for Determinantal
  Point Processes*, NIPS 2012.

**Author:** dstroy0 (Douglas Quigg) <dquigg123@gmail.com>
**Date:** 2026-09-16
