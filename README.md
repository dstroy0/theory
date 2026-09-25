# theory_bucket

**Purpose:** Author the theory books in one place, and know which direction a change travels between
here and the release before making one.
**Scope:** the seven books at this repository's root. The anchor_sift workbook is not one of them and
is not here.

## What decides publication

This repository is private. Whether a file here may be published is decided by the class of its
content in `PQC/theory/PARTITION.tsv`. That table keys PQC's own paths and anchor_sift's, and holds
no row for any path in this repository. A directory name decides nothing, and that includes both
`PUBLIC/` and `HELD/`.

**The sha256 book is held and lives under `HELD/`.** `PARTITION.tsv:162-167` and `:172-176` class all
eleven of its chapters HELD, and `:72` gives a book the strictest class of anything it includes.
Measured on 2026-09-16, the copies here carry the held text: `chapter_spectral.tex` is 155 lines,
where `:167` records 81 lines published and 155 held. The whole book, 17 files, moved from
`PUBLIC/cryptography/sha256/` to `HELD/cryptography/sha256/` on Douglas's ruling in commit
`77d3620`. `latexmk` with `lualatex` builds it to 57 pages from either location.

**The subtree pull shown under "The flow" publishes.** `anchor_sift` is a public repository, so the
pull carries whatever it pulls onto a public remote, and held content must not travel by it. The seed
for the public `dstroy0/theory` repository is specified in `PLANS/THEORY_PUBLIC_PRIVATE_SPLIT.md:72`
at the workspace root, to be built row by row from the partition and to refuse any file without a
row.

## The flow

This repository is upstream. `anchor_sift` is downstream and carries these books in its tree as a
git subtree under `theory_bucket/`. They ship with the release and a clone gets them without a
second fetch.

```
theory_bucket  (here, authored)
      |
      |  git subtree pull --prefix=theory_bucket
      v
anchor_sift/theory_bucket/  (release, read only)
```

`anchor_sift` main is downstream only. A change typed into `anchor_sift/theory_bucket/` is lost the
next time that subtree is pulled, the same way an edit to any generated file is lost. Bring the
change here, open a pull request against `main`, and pull it down after it merges.

## What is here

| book | what it is |
| --- | --- |
| `Salishan` | the corpus work, whose tables are speakers' words written down |
| `HELD/cryptography/sha256` | the SHA-256 readings, refutations included |
| `crystallography` | the lattice work |
| `delta_null` | the null construction |
| `millennium` | the Navier-Stokes and Euler reading |
| `precision` | the relation search and what precision costs |
| `thought_experiments` | the boundary arguments |

## What is not here, and why

The anchor_sift workbook stays in `anchor_sift` at `theory/workbook/`. It is the book about that
engine, it moves when that engine moves, and separating the two would put a book in one repository
and its subject in another.

Two chapters in this tree are generated and say so in their own first lines. Edit the generator, not
the chapter:

- `Salishan/chapters/chapter_Salishan_pure_corpus_README.tex`, from
  `anchor_sift/maint/data/salishan/hand_extraction/pure_corpus_index.py`.
* `HELD/cryptography/sha256/chapters/chapter_sources.tex`, from `tools/book/build_bibliography.py`
  in the private PQC tree. The `btc` remote in anchor_sift's `.git/config` still points at that tree's old
  location and no longer resolves.

## Building

Each book is a directory holding `main.tex`, `preamble.tex`, `chapters/` and `frontmatter/`. Nothing
lands beside the source: build with `-output-directory` pointed under `anchor_sift/build/theory/`.
The one upward path in the tree, the figure guard in
`HELD/cryptography/sha256/chapters/chapter_boundary.tex`, resolves against that build directory and depends on a book sitting three levels below the repository
root, which is where these sit both here and downstream.

**Author:** dstroy0 (Douglas Quigg) <dquigg123@gmail.com>
**Date:** 2026-09-11
