# music-practice-bungee-source

This repository is a **project-controlled source-availability / durability
mirror** for third-party software used by [Music Practice](https://github.com/JZenithC/music-practice-app),
a guitar practice app. It is **not** the Music Practice application source
repository — the application source remains separate and proprietary, and is
not published here or anywhere else.

## What this is

An exact, self-contained snapshot of **Bungee Basic v2.4.24**
(upstream commit [`746833f68a574d997ec50443e7cfd2d37b026302`](https://github.com/bungee-audio-stretch/bungee/tree/746833f68a574d997ec50443e7cfd2d37b026302)),
the MPL-2.0-licensed audio time-stretch/pitch-shift engine that Music Practice
compiles, unmodified, into a WebAssembly module. Original upstream project:
<https://github.com/bungee-audio-stretch/bungee> (© Parabola Research Limited).

The snapshot includes the exact pinned contents of Bungee's declared
submodules at this commit — Eigen, pffft, and cxxopts — materialized as
ordinary directories instead of git submodule references, so the archive is
self-contained and does not depend on any third-party git hosting to be
usable. See [`SOURCE-PROVENANCE.md`](SOURCE-PROVENANCE.md) for exact commit
pins, upstream URLs, and acquisition details.

## Licensing

This mirror does **not** relicense any upstream software. Every included
component remains under whatever licence its own upstream repository states,
exactly as reproduced in its original licence files under `source/`
(Bungee and Eigen: MPL-2.0; pffft: the FFTPACK licence, in `pffft.h`;
cxxopts: MIT). Nothing here is a claim of ownership over, or affiliation
with or endorsement by, Parabola Research, the Eigen project, the pffft
project, or the cxxopts project.

## Why this exists

Bungee Basic's MPL-2.0 licence requires that recipients of the compiled
binary be able to obtain the corresponding Source Code Form. Working links
to the pinned upstream commits satisfy that today, but upstream
repositories can move or disappear. This repository exists to give the
project durable, independently-controlled access to the exact source used,
for reproducibility and long-term source availability — it is a
conservative durability measure, not a claim that MPL-2.0 itself mandates
self-hosting.

## Contents

```
README.md                  this file
SOURCE-PROVENANCE.md        exact commit pins, URLs, acquisition record
SHA256SUMS                  hashes of the release archive
source/                     the exact Bungee checkout (unmodified)
  submodules/
    eigen/                  pinned Eigen snapshot (unmodified)
    pffft/                  pinned pffft snapshot (unmodified)
    cxxopts/                pinned cxxopts snapshot (unmodified)
```

A tagged, hash-manifested release archive of this snapshot is published
under this repository's Releases.
