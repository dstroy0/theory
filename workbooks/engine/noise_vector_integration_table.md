# The noise vector integration table

| # | noise vector | how it moves | read by | status |
|---|---|---|---|---|
| 1 | photon shot | every frame; variance grows with level | slope of Σd²/2N against level | measured |
| 2 | dark current level | fixed; grows with exposure | zero-light intercept | measured as offset |
| 3 | dark current shot | every frame; grows with exposure | intercept at two exposures | theory |
| 4 | read | every frame; level-free | intercept of E[d²]/2 | measured |
| 5 | thermal (Johnson–Nyquist) | every frame; grows with temperature | intercept, with 4 | theory |
| 6 | reset (kTC) | every reset | intercept, with 4 | theory |
| 7 | 1/f flicker | memory across frames | structure function D(k) | theory |
| 8 | row banding | shared by a row, per frame | row sums against X·Σd² | theory |
| 9 | column banding | shared by a column, per frame | column sums | theory |
| 10 | plane bias | shared by a plane, per frame | plane sums | theory |
| 11 | clock-induced / spurious charge | sparse events | tail counts above against below | theory |
| 12 | multiplicative excess (F) | per sensor scalar | slope g·F² | measured as g·F² |
| 13 | partition | binomial split | none on Poisson light | theory |
| 14 | quantum efficiency (Q_e) | scale on photons | calibrated source only | theory |
| 15 | gain (g) | per sensor scalar | transfer slope | measured |
| 16 | optical crosstalk | blur before the draw | edge spread | theory |
| 17 | electrical crosstalk | shared after the draw | Σd(v)d(v+δ) per axis | theory |
| 18 | fixed pattern (DSNU) | constant in time | anchor bits | measured |
| 19 | PRNU | gain per voxel | spread² of static means against level² | theory |
| 20 | hot pixels | fixed place | per voxel departure across samples | theory |
| 21 | dust | fixed shadow | local drop across samples | theory |
| 22 | quantization | within one step | 12σ² = 1 | built |
| 23 | clipping | at the lane's ends | counts at 0 and top | built |
| 24 | constant region | never flips | ∧ₜI = ∨ₜI | measured |
| 25 | dim z correlation | z neighbor, dim levels | Σd(v)d(v+z) | measured |
| 26 | bending transfer curve | level 40 to 200 | fit residual per level bin | measured |
| 27 | sample-wide entropy bump | one window, whole sample | bit flips per window | measured |
| 28 | mitosis / lysis entropy bump | near the event, whole sample | bit flips against divisions | theory |
| 29 | fluidic covariance around an event | falls off with distance | Σd(v)d(v+δ) near against away | theory |
| 30 | Ω, the open term | whatever the rest leave | E[d²]/2 less the sum of 1 to 29 | theory |

## Long descriptions

1. **Photon shot.** From the four vectors, the draft (ξ_shot) and compression's categories. Independent every frame; its variance grows with the level. Read per level bin by the count N, Σ d and Σ d², with the neighbor difference taken first; the slope of Σ d² / 2N against the level (C3, C5, C7). Camera law, built: Binomial(4S, ½) − S, mean S and variance S (sim_camera.h:167–172). On the 44b6 set: gain 0.821 to 1.162, mean 0.986, over the 25; 44b6_0113de3b variance = 1.162 × level + 2.46; the `noise_floor` sim recovers slope 2.0046 against a planted 2. Next: none owed; it is the line every other row is read against.
2. **Dark current level.** From the draft (D, I_dark·Δt). Constant in time at a fixed exposure; grows with the exposure time Δt. Read by the zero-light level of the transfer curve, L₀ = −c/g (C7), and a dark frame's mean where the set has one. Camera law: a single offset (`offset`), with no rate and no Δt. On the 44b6 set: 44b6_0113de3b's line meets zero near level −2; the intercept runs −11.74 to 21.08 over the 25, near zero on most. Measured as an offset near zero; theory as a rate. Next: whether the set holds dark frames or a second exposure time; a sim planting D at two Δt.
3. **Dark current shot.** From Doug's list and the draft (D inside the root). Independent every frame; variance g²·D·Δt in DN², on the same line as photon shot. Read at zero light, E[d²]/2 − R² = g²·D·Δt, separated from read noise only by a second Δt or by dark frames. Camera law: not built; the shot draw would run over 4(S + D). On the 44b6 set: inside the intercept with read noise. Next: the camera law gains D; the detector reads D back from two exposure times.
4. **Read.** From the four vectors ("thermal and read"), the draft (σ²_read) and compression's categories. Independent every frame; does not depend on the level. Read by the transfer curve's intercept, E[d²]/2 at zero signal (C7). Camera law, built: Binomial(4r², ½) − 2r², variance r² (sim_camera.h:174–178). On the 44b6 set: about 1.6 lane units on 44b6_0113de3b; the `noise_floor` sim reads intercept 5.643 against a planted 6. Next: none owed.
5. **Thermal (Johnson–Nyquist).** From the four vectors and the draft's classification ("Thermal/Read"). Independent every frame; does not depend on the level; grows with temperature. Read by the same intercept as row 4; the frames alone do not separate them. Camera law: not built apart from row 4. Next: a run at two temperatures.
6. **Reset (kTC).** From Doug's list and the draft's classification. A new offset per voxel at every reset, independent of the signal; correlated double sampling cancels it on a sensor that does it. Adds 2σ²_kTC to E[d²] at every level, inside the intercept with rows 4 and 5. Camera law: not built; one more binomial half per voxel-frame. Next: the camera law gains it; the detector confirms the intercept reads R² + σ²_kTC and cannot split them.
7. **1/f flicker.** From Doug's list and the draft (the memory kernel M, ξ_flicker, σ²_1/f(k)). Carries memory across frames: its variance over a lag grows with the lag. Read by the structure function D(k) = Σ_v Σ_t (I_{t+k}(v) − I_t(v))² over static voxels at lags k = 1, 2, 4, … 64, with N(k) its count of pairs. White noise holds D(k)·N(1) = D(1)·N(k) at every k; flicker grows slowly with k; a random walk grows as k; a linear drift grows as k², and the second difference (C4) removes it. Camera law: not built; a sum of integer draws, the draw of octave o held for 2^o frames (Voss–McCartney). On the 44b6 set: not taken; bits 0 to 3 flip 499 to 500 per thousand in every window on 19 samples, which a low-amplitude flicker would not show. Next: D(k) on the dim static voxels of the 25.
8. **Row banding.** From Doug's list, the draft (σ²_row(i, k)), the numpy example (`row_banding`) and compression's spatially correlated category. An offset shared by every voxel of one row in one frame, new each frame. Read per line ℓ of X voxels along x, R_{t,ℓ} = Σ_x d_t: independent noise gives E[R²] = X·E[d²], and a shared row offset adds X(X − 1)·2σ²_row; compare Σ_ℓ R² against X·Σ d² over the same voxels. Camera law: not built; a draw keyed by (frame, plane, row). On the 44b6 set: corr x of d at level 40 is 0.761 on 44b6_267148e4, 0.354 on 668e0cc7 and 0.150 on 5740d24b, and below 0.11 on the other 21; not identified as banding. The correlation is measured. Next: the line sums along x on those three samples and on 44b6_0113de3b.
9. **Column banding.** From Doug's list, the draft (σ²_col(j, k)) and compression's spatially correlated category. An offset shared by every voxel of one column in one frame. Read as row 8, with the line along y. Camera law: not built; a draw keyed by (frame, plane, column). On the 44b6 set: as row 8, the corr y of d. Next: the line sums along y on the same samples.
10. **Plane bias.** From the draft's σ²_total, extended to the whole frame (the lane's bias moving between exposures). An offset shared by a whole plane in one frame. Read per plane, P_t = Σ over the plane of d_t, against the plane's count × Σ d². Camera law: not built; a draw keyed by (frame, plane). Next: the plane sums on the 25.
11. **Clock-induced / spurious charge.** From Doug's list and the draft's classification (CIC, under multiplicative). Sparse single-electron events at a rate p per voxel per frame, before any gain register. Read in static dark voxels by the count of values above the read-noise tail at distance k less the count below it at the same distance: read noise is symmetric and these events only add. Camera law: not built; an event where draw_below(key, counter, den) < num, for an exact rational p = num/den. Next: the tail counts in the dim static voxels of the 25.
12. **Multiplicative excess (F).** From Doug's list ("sensor dependent scalar") and the draft (F). A scalar per sensor; multiplies the shot variance by F². The transfer curve's slope is g·F² and does not separate the two; g needs a measure of its own, such as the spacing of single-photon peaks in the dimmest histogram where the camera resolves them. Camera law: not built; F = 1 today. On the 44b6 set: the slope 0.821 to 1.162 over the 25 is g·F². Next: the dimmest levels' histogram, for peaks spaced g apart.
13. **Partition.** From Doug's list. A binomial split of the carriers: quantum efficiency as thinning, or charge shared between wells. None on a Poisson source: a Poisson count thinned binomially is still a Poisson count; it shows only on a sub-Poisson source. Camera law: the shot draw is itself binomial. Next: none on this set, whose light is taken to be Poisson.
14. **Quantum efficiency (Q_e).** From the draft (Q_e). A scale on the photons, applied as the thinning of row 13. None from the frames: after thinning the transfer curve reads the same g at any Q_e; only a calibrated source measures it. Next: none on this set.
15. **Gain (g).** From the draft (G, electrons per DN) and compression's C7 (g, DN per electron). A scalar per sensor. Read by the transfer curve's slope (with F², row 12). Camera law, built: `gain`, one scalar for every voxel. On the 44b6 set: 0.821 to 1.162, mean 0.986, over the 25. Next: none owed.
16. **Optical crosstalk.** From Doug's list and the draft (the kernel K). Light landing in a neighbor before detection: the signal is blurred, then drawn. None in the noise: the draws after the blur stay independent between voxels, and corr(d, d_x) on static voxels stays 0; it shows only as the blur of an edge. Camera law: not built; an integer kernel over a power of 2 applied to S before the shot draw. Next: the edge spread of a static body.
17. **Electrical crosstalk.** From Doug's list and the draft (the kernel K). Charge or signal leaking after detection: the noise itself is shared with neighbors. Read on static voxels by Σ d(v)·d(v + δ) against Σ d² for each axis δ (C6); independent noise gives 0. Camera law: not built; the same kernel applied after the draw. On the 44b6 set: the `noise_floor` sim's null holds, Σ dd′ / Σ d² = −71,727 / 1,161,503,631 on truth-static pairs; on 44b6_0113de3b corr x of d is 0.007 at level 24. Its null is measured in the sim. Next: C6 on the static voxels of the 25, each axis.
18. **Fixed pattern (DSNU).** From the four vectors, compression's categories and the draft (η_spatial). Constant in time at a voxel. Cancels exactly in d (C3); read by the per-voxel sum Σ_t I_t and by the anchor bits ∧_t I_t and ∧_t ¬I_t (C10). Camera law, built: draw_below(key ⊕ pattern, voxel, reach + 1), added (sim_camera.h:153–160, 181). On the 44b6 set: the anchor bits are set in every frame, different in every sample. Next: none owed.
19. **PRNU.** From the draft (PRNU_{i,j}, (S·PRNU)²) and the numpy example (`prnu_field`). A gain per voxel, constant in time: g_v = g·(1 + p_v). Read among static voxels at one true level: the spread of their temporal means grows as the level squared; the square of that spread against the square of the level, per level bin. Camera law: not built; the fixed pattern is additive only, and `gain` is one scalar. Next: the camera law gains a gain pattern; the detector reads it on the static background of the 25.
20. **Hot pixels.** From compression's spatially correlated category. A voxel with a high dark current: a large offset and extra shot, fixed at a place. Read per voxel: the temporal mean and E[d²]/2 both above the line at its level by more than the null allows, at the same place across samples. Next: the per-voxel departures, counted at each place across the 25.
21. **Dust.** From compression's spatially correlated category. A shadow in the optical path: a local drop in gain, fixed at a place. Read per place: the temporal mean against the local level, persisting across samples. Next: the same count across the 25.
22. **Quantization.** From the four vectors and compression's categories. Uniform within one step of the lane. 12·σ²_q = 1 for a unit step, inside the intercept. Camera law, built: integer lanes, clamped to the u16 range (sim_camera.h:208–214). Next: none owed.
23. **Clipping.** From the camera law. A value past either end of the lane is held at the end. Read by the count of voxel-frames at 0 and at the lane's top. Camera law, built: counted as `clipped` (sim_camera.h:208–211). Not taken on the 25. Next: the counts at 0 and at the top on the 25.
24. **Constant region.** From the engine ledger (21 September). Voxels whose value never changes: no flips in any bit. Bits 0 to 3 drop together within a window (for example 350, 350, 350, 350 per thousand); ∧_t I_t = ∨_t I_t at the voxel. Six samples hold it: 44b6_0db75fae, 1574802b, 267148e4, 5740d24b, 587a1e22 and 668e0cc7; on 0db75fae, 267148e4 and 5740d24b the neighbor bound (32.9%, 32.6%, 36.9%) falls under the shot-only floor (34.5%, 35.6%, 38.4%). Its cause is not established: a voxel with read noise near 1.6 would not hold one value for 100 frames. Next: which step writes the constant (clipping, a mask, or processing); its values against row 23's counts.
25. **Dim z correlation.** From compression_table (44b6_0113de3b). The frame difference correlates with its z neighbor at the dimmest levels. Read by C6 along z. On the 44b6 set: corr z of d 0.116 at level 24 and 0.123 at 40, against corr x 0.007 and 0.031; not explained, and its cause belongs to Ω. Next: C6 along z on the 25.
26. **Bending transfer curve.** From compression_table. A transfer curve that is not a straight line from level 40 to 200. Read by the fit's residual per level bin. On the 44b6 set: 44b6_12dfb391 fits intercept −11.74; 53f95252 fits intercept 21.08 at the lowest gain, 0.821; both lines bend. Its cause belongs to Ω. Next: rows 7, 10 and 19 on those two samples.
27. **Sample-wide entropy bump.** From the entropy history (`entropy_history`, engine ledger 22 September; noise_sieve_tower §16). In window 4 (transitions 45 to 55) every bit from 5 to 10 flips more at once, bit 8 from about 122 to 153 per thousand; windows 5 to 8 then fall below where they started. Measured on 44b6_0113de3b; its cause is not named, and it sits in Ω until it is. Next: the history on the other 24 samples; row 10's plane sums at window 4.
28. **Mitosis / lysis entropy bump.** From `noise_sieve_5_cell_tracking_harmonics.md`: if a cell really lysed or divided, the entropy bumps near that time, judged over the whole sample. Read by the history's bit flips per window against the ground truth's divisions. Whether row 27's bump is one is untested. Next: row 27's window against the divisions on 44b6_0113de3b.
29. **Fluidic covariance around an event.** From `noise_sieve_5_cell_tracking_harmonics.md`: mitosis and lysis change the fluidics for a great distance around them. As noise, a covariance around an event that falls off with distance; it must be told apart from rows 8 to 10 and 17. Read by C6 at growing distance from the ground truth's divisions, against the same sums away from any division. Next: that reading on the 25.
30. **Ω, the open term.** 25 September. Whatever the named rows leave. Read per voxel as E[d²]/2 less the sum the rows above predict at its level, compared as integers, and per axis as the covariance the rows above leave; a named row moves out of Ω only when its sums account for its part. Nothing to plant: Ω is what the sims do not know. Holds rows 25, 26 and 27 today, and row 8's spatial correlation until that is identified. Irreducible until its construction is deduced. Every experiment shrinks it; what is left names the next row.

## What was asked for, verbatim

25 September, spelling corrected ("q4 verbatim text, corrected for spelling only"):

<!-- docs-check: quoting -->
> "to our noise detector we want to add dark current shot noise, reset noise, 1/f noise flicker, row and col noise (banding patterns), clock induced charge/spurious charge noise. multiplicative excess noise (sensor dependent scalar), partition noise, crosstalk noise (optical or electrical), this will work for compression and filtering"

> "we should also include an open term for terms we cannot or have not described, they are irreducible until we can deduce their construction"

> "example do not use npy"
<!-- docs-check: end quoting -->

## The notation

A voxel v sits at (z, y, x) and holds I_t(v) in frame t, in lane units (DN). Its level L is its value's mean over the frames it is read in. The frame difference is d_t(v) = I_{t+1}(v) − I_t(v). Following compression_table C7, the gain g is DN per electron, and the draft's G is its inverse (electrons per DN). O is the lane's value at zero light. Every reading below is a sum of integer products over named voxels and frames. Two ratios are compared by multiplying across (a·d against b·c), as the period's 128-bit test does. A spread is compared as its square, with no root, and a code length is a bit length, with no logarithm.

The composite each voxel's frame difference integrates, with structure taken off by the neighbor (C5):

  E[d²] / 2 = F²·g·(L − O) + R² + σ²_thermal + σ²_kTC + σ²_row + σ²_col + σ²_plane + σ²_1/f(1) + σ²_CIC + 1/12 + Ω(v)

with F the excess noise factor, R² the read variance and 1/12 the quantization of a unit step (held as 12·σ² to stay integer). The fixed pattern and the dark level cancel in d (C3). The dark current's shot rides the same line as the photon shot, since the dark electrons are counted in L. PRNU scales g per voxel instead of adding a term. Optical crosstalk blurs S before the draw and adds no noise term. Electrical crosstalk adds covariance between neighbors, not variance at one voxel. Ω is the open term.

## Doug's draft, term by term

Doug's draft (25 September) states the noise as a stochastic wave functional. It is kept as written in the thought experiments. Each of its objects has an exact form here:

| the draft's object | what it is in the draft | its exact form here | status |
|---|---|---|---|
| Φ(x, t) | the real-valued noise field | the residual per voxel-frame, I_t(v) less the fixed pattern and the structure, in integers | **theory** |
| K(x) ∗ | the crosstalk kernel, Gaussian or Laplacian | rows 16 and 17: an integer kernel over a power of 2, before the draw (optical) or after it (electrical) | **theory** |
| S(x, t) | the photon signal field | the structure; in the sims, `sim_signal`'s electrons | **built** in the sims |
| D(x, t) | the dark current rate field | rows 2 and 3 | **theory** |
| ξ_shot, √(F·(S + D)) | a white Gaussian field scaled by the shot spread | row 1's binomial draw; the Gaussian is the draw's large-count limit and is not adopted; the spread is compared as its square | **built** (the binomial) |
| F | the multiplicative excess noise scalar; the draft gives 1 for CMOS and about √2 for EMCCDs | row 12; the draft's numbers are its own, not measured here | **theory** |
| η_spatial(x) | the time-invariant field: PRNU and banding | rows 18 and 19; banding moves to rows 8 to 10, since the draft's own σ²_row(i, k) and σ²_col(j, k) carry the frame index k | **theory** |
| ζ_temporal = ζ_white + ∫ M(t − τ) ξ_flicker(τ) dτ | white noise plus flicker through a memory kernel | rows 4 to 6, and row 7; the integral becomes a sum of integer draws held for 2^o frames, octave by octave | **theory** |
| M, a kernel giving 1/f^α | the flicker's memory | the octaves' weights set α; D(k) reads it (row 7) | **theory** |
| a Hilbert space and a functional probability density | the state space of Φ | not adopted: the detector reads counts and sums, never a density | **theory** |
| Z = ∫ DΦ exp(−S[Φ]) | the partition function as a path integral | not adopted | **theory** |
| S[Φ] quadratic in Σ⁻¹ | the action under the central limit's Gaussian | not adopted; its data are row 17's covariance sums and row 7's structure function, taken exactly | **theory** |
| Σ(x, x′, t, t′) | the global spatiotemporal covariance kernel | the table of exact lag sums: Σ d(v)·d(v + δ) at each spatial lag δ and D(k) at each temporal lag k; the correlation operator (the books' C_{t,t′}, Doug's L_{t,t′}) is this object | **theory** |
| σ²_total(i, j, k) = G²F²(Q_e·S + I_dark·Δt) + (S·PRNU)² + σ²_row + σ²_col + σ²_read + σ²_1/f | the discretized composite | the composite under "The notation", with rows 5, 6, 10, 11, 22 and Ω added and G written as g = 1/G | **theory** |

Two places where the draft disagrees with itself:
- **F's power.** The field equation scales the shot by √(F·(S + D)), which puts F once in the variance. The discretization writes G²·F², which puts F squared there. Row 12 follows the discretization: the variance carries F².
- **Banding in time.** The draft sets banding in η_spatial, which is constant in time. Its discretization writes σ²_row(i, k) and σ²_col(j, k), with the frame index k. Rows 8 to 10 follow the discretization: a new offset every frame.

The numpy example, term by term:

| the example's line | here |
|---|---|
| `expected_charge = signal_field + dark_rate` | S + D, rows 1 to 3 |
| `np.random.normal(0, np.sqrt(expected_charge))` | row 1's Binomial(4S, ½) − S, with no float and no root |
| `prnu_field = np.random.normal(1.0, prnu_sigma)` | row 19 |
| `row_banding = np.random.normal(0, 0.5, size=(shape[0], 1))` | row 8, a draw per frame and row |
| `white_alloc = np.random.normal(0, read_noise)` | row 4 |
| the optional FFT filter for 1/f | row 7's octave draws, with no FFT |
| `total_field = (expected_charge * prnu_field) + shot_noise + row_banding + white_alloc` | the composite under "The notation" |

Per "example do not use npy", nothing here uses numpy or a float.

## What the integrated floor is for

**Compression.** Each named row's predicted spread at a voxel's level gives that voxel's counts, and the coder spends Σ −log₂ p against them (compression_table F5). The bits go to departure and to Ω. When a set needs a category the rows lack, the gap to F5 names it, and it enters Ω first.

**Filtering.** A voxel-frame whose d departs from the integrated floor by more than the null allows is departure: a body, a split, a drive (noise_sieve_tower §17). The null draws read the rate against the integrated floor, not against one sample-wide reading.

## The order of the experiments

Each run below is a tessera job and cites the rows it moves. None has run.

1. The camera law gains the unbuilt terms one at a time: D at two exposure times (rows 2 and 3), kTC (row 6), the flicker octaves (row 7), the row, column and plane draws (rows 8 to 10), spurious charge at a rational rate (row 11), a gain pattern (row 19), and the two crosstalk kernels (rows 16 and 17). Each gets a sim graded as `noise_floor` is: the detector reads the planted value back.
2. On the 25: the structure function (row 7); the line and plane sums (rows 8 to 10); C6 on static voxels along each axis (rows 17 and 25); the tail counts (row 11); the clip counts (row 23); the dimmest histogram (row 12).
3. The constant region against the clip counts (rows 23 and 24).
4. Ω per voxel, after every row above that has a reading.
