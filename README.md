# Syncopation Pro

**Load music onto classic iPods — no iTunes, no Swinsian, no cloud.**

Syncopation Pro is a native macOS app that syncs music straight onto iPods in
disk mode. Plug in an iPod classic or nano, pick a folder, hit Sync — tracks
appear on the device, with album art, ready to play the moment you eject.

> This repository is the **support and documentation home** for Syncopation
> Pro. The application itself is proprietary and its source code is not
> published here. For the free, open-source v2.0 file-sync app, see the
> [Syncopation Community Edition](https://github.com/ehajek/Syncopation).

## What it does

- **Synco-Mode** — one app, four modes: Music, ePUB/PDF, All Files, and iPod.
- **iPod mode** — detects any iPod in disk mode, identifies the exact model
  from a built-in table of every iPod ever made, and writes the device's
  native database directly (including the checksummed databases that
  classics and nanos require). Tracks play immediately after eject.
- **FLAC → ALAC conversion** — automatic, using macOS's built-in encoder.
  Output is 16-bit / 44.1–48 kHz, exactly what the devices were built to
  play; hi-res sources are downsampled correctly.
- **Album artwork** — covers are extracted from your files, rendered into
  the device's native thumbnail formats, and shown on the iPod.
- **Safe by design** — existing music and playlists on the iPod are always
  preserved, every database write keeps a backup on the device, syncing only
  adds and never deletes, and nothing is duplicated (tracks already on the
  iPod are recognized, even ones loaded by other apps).
- **Everything the free version does** — one-way and two-way folder/SD-card
  sync with conflict handling, previews, and per-mode file filtering.

## Requirements

- macOS 14 (Sonoma) or later, Apple Silicon. On macOS 26 and later the
  app uses the Liquid Glass appearance; on earlier systems it falls back
  to standard native controls automatically.
- An iPod in disk mode: iPod classic (all generations), iPod nano
  (through 4th gen), iPod video, mini, or photo. (iPod touch and iPhone use
  a different sync system and are not supported; shuffle support is planned.)

## Support

- **Questions and bug reports:** [open an issue](../../issues) in this
  repository.
- **How it works under the hood:**
  [docs/TECHNICAL.md](docs/TECHNICAL.md) — the full technical reference for
  the iPod support: device detection, the iTunesDB format, checksums, and
  what these devices really play.

## Status

In active development. Availability and pricing to be announced.

---

Copyright © 2026 Eddie Hajek. All rights reserved.
Syncopation Pro is proprietary software. iPod is a trademark of Apple Inc.;
this product is not affiliated with or endorsed by Apple.
