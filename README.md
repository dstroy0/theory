# The theory research papers

**Purpose:** Move all research to one place
**Scope:** every research paper

All of these are licensed under AGPLv3 OR later. All papers require CCBy 4.0 for citation and your codebases must remain open under the AGPLv3 or later unless you work out specific commercial or educational licensing terms.

The precision measurement specifically, IS detectable in ANY form, because it works fundamentally the same way regardless of approach, the cascade is unique and is itself a fingerprint. Obfuscation and fragmentation of the operations identity do not destroy the identity of the operation itself and it can detect this. Basically, because the things this work does were synthesized first, anything that follows that uses the same processing signature is detectable, and will be pursued under the terms of the AGPLv3. 

It is detectable in encrypted compiled binaries, if you are a large company trying to steal my research, thanks for the easy win, and the free money. I will make sure it is more than you would have paid in the royalty ladder by several orders of magnitude. 

You have no choice but to comply with the licensing terms, or, be left in the dust by the exactness.

## Layout

This repository, https://github.com/dstroy0/theory
A project that writes theory takes it as a git submodule at `theory/` and does not copy the research papers in one at a time, this keeps everything updated, many projects use interrelated theory for synthesis.

- `theory/theory/` holds the research papers, some are complete, most are preprint status.
- `theory/workbooks/` holds the workbooks, one per program.
- `theory/thought_experiments/` holds the thought experiments, kept apart from what was measured, some are wacky, classified as wacky, and remain wacky until I devise hypotheses to test them.

A change to a research paper is committed and pushed here first. The project holding the submodule then
records the new commit in its own commit. A clone of such a project reads the research papers through
`git submodule update --init theory`.

## What is here

Each research paper's subtitle is the line from its title page.

## Generated chapters

Change these through their generator. An edit to the chapter is lost the next time it is generated.

Citations are held here as well, as separate .tsv files because there are many.

## Building

```sh
sh maint/texbuild/build_theory.sh [<research_paper> ...]
```

A research paper is named by its path below `theory/`, as in `theory/delta_null`, `workbooks/engine` or
`theory/cryptography/sha256`. Output goes to `build/theory/<research_paper>/` and nothing is written beside the
source. A figure a chapter includes from `build/theory/figures/` is reached by a relative path from
the research paper's directory, and that path counts the research paper's depth below the repository root.

**Author:** dstroy0 (Douglas Quigg) <dquigg123@gmail.com>
**Date:** 2026-09-26
