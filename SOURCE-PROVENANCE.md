# Source Provenance

This document records the exact provenance of the source snapshot in
`source/`, how it was acquired, and the packaging transformations applied
to make it self-contained.

## Bungee Basic

- **Version:** v2.4.24
- **Upstream repository:** <https://github.com/bungee-audio-stretch/bungee>
- **Exact commit:** `746833f68a574d997ec50443e7cfd2d37b026302`
  (verified to be the exact commit tagged `v2.4.24` upstream)
- **Licence:** MPL-2.0 (`source/LICENSE`, unmodified)

## Declared submodules at this commit

Recorded from `source/.gitmodules` and each submodule's checked-out `HEAD`
at the Bungee commit above. All three submodules declared at this commit
are included in this snapshot, regardless of whether each was compiled into
the Stage 1 production WASM (see "Relationship to the shipped WASM" below).

| Path | Upstream URL | Exact commit | Licence |
|---|---|---|---|
| `submodules/eigen` | <https://gitlab.com/libeigen/eigen.git> | `c29c800126982c561e8d0b9255dc65474cd98de3` | Primarily MPL-2.0 (`COPYING.MPL2`); also carries `COPYING.APACHE`, `COPYING.BSD`, `COPYING.MINPACK` for individual third-party files at this pin |
| `submodules/pffft` | <https://bitbucket.org/jpommier/pffft.git> | `02fe7715a5bf8bfd914681c53429600f94e0f536` (tag `v1.0.0`) | FFTPACK licence (BSD-style, UCAR/NCAR), reproduced in `pffft.h` |
| `submodules/cxxopts` | <https://github.com/jarro2783/cxxopts.git> | `4bf61f08697b110d9e3991864650a405b3dd515d` | MIT (`LICENSE`) |

None of these three submodules declare any further nested submodules at
these pins.

## r2 correction (2026-08-14)

The initial `bungee-v2.4.24-mpa-stage1` (r1) release had correct file
**contents** but lost Git executable-mode metadata for 19 upstream files (2
Bungee workflow scripts, 17 Eigen scripts/tools) — they were published as
`100644` instead of their upstream `100755`. Root cause: this repository was
first materialized on a Windows machine with `core.filemode=false`, which
made `git add` ignore the source files' actual permission bits. The `r2`
tag/release (this commit) corrects the Git mode for exactly those 19 paths
via a mode-only index change; no file bytes changed. `SOURCE-GIT-MODES.txt`
(added in r2) records the full mode state so this class of loss is
independently checkable going forward. The r1 tag/release remain published,
unaltered, as historical evidence, with their descriptions marked superseded.

## Source acquisition

- **Acquisition date:** 2026-08-14
- **Method:** fresh clone of the upstream Bungee repository, checked out at
  the exact commit above; `git submodule update --init --recursive` to
  populate the three declared submodules at their pinned commits; every
  resulting working tree verified clean (`git status` / `git diff` against
  `HEAD` empty) before packaging.

## Packaging transformation

A plain git checkout containing only submodule gitlink entries would still
depend on the upstream submodule hosts (GitLab, Bitbucket, GitHub) to be
usable. To make this archive self-contained, each submodule's gitlink was
**materialized into an ordinary directory** containing that submodule's
actual pinned source files, in place of the gitlink. This is the only
packaging transformation applied.

**Verification of exact content:** every tracked file in the Bungee
checkout and in all three submodules (1,966 files total) was extracted
directly from its git blob (`git archive`) and independently re-verified
by recomputing each file's git object hash from the extracted copy and
comparing it against the original blob hash recorded by `git ls-tree` at
the pinned commit. All 1,966 files matched exactly; there were no
mismatches. No upstream source file content was intentionally modified,
and none was found to differ. Line endings, whitespace, licence text, and
copyright notices were left exactly as stored upstream — nothing was
normalized.

**Verification of exact Git file mode:** separately from content, every
file's Git mode (`100644` ordinary / `100755` executable) was compared
against the upstream `git ls-tree` mode at the pinned commit for all 1,966
files. `SOURCE-GIT-MODES.txt` records the resulting mode for every packaged
file so this can be independently re-checked without needing local clones
of the upstream repositories.

Because materializing gitlinks into directories necessarily changes the
packaging tree representation, **the git tree hash of this repository does
not equal, and is not claimed to equal, upstream Bungee's git tree hash.**
The underlying source file contents are exact; only the containing
directory structure differs from a plain git checkout with unresolved
submodule gitlinks.

`.git` metadata directories from the original clones are excluded from
this snapshot; only tracked source files are included.

## Relationship to the shipped Music Practice WASM

This source snapshot corresponds to the exact upstream commit and submodule
pins used to build the Bungee engine shipped by Music Practice Stage 1 as:

- **File:** `src/audio-engine/bungee.wasm` (in the Music Practice
  application repository, not included here)
- **Size:** 67,475 bytes
- **SHA-256:** `3a0ec4b96207ad88c3b552a7b820935a11c5bd19aaaed2af543a51543ecbbc60`

**Scope of this claim:** this snapshot demonstrates **source provenance and
durable source availability** — that the exact Bungee/Eigen/pffft source
used to build that WASM is preserved here, unmodified, under project
control. It does **not** demonstrate byte-for-byte reproducible compilation
of that WASM from this source; no reproducible-build attempt has been made
as part of publishing this snapshot. `cxxopts` is included because it is a
submodule declared at the pinned Bungee commit, even though it is used only
by Bungee's upstream CLI tool and was not compiled into the shipped WASM.

## No source modifications

No file under `source/` was added to, deleted from, or modified relative to
its pristine upstream content. SPDX headers, copyright notices, and licence
files are intact exactly as they appear upstream.
