# Syncopation Pro

**Load music onto classic iPods — no iTunes, no Swinsian, no cloud.**

Syncopation Pro is a native macOS app that syncs music straight onto iPods in
disk mode. Plug in an iPod classic or nano, pick a folder, hit Sync — tracks
appear on the device, with album art, ready to play the moment you eject.

> This repository is the **support and documentation home** for Syncopation
> Pro. The application itself is proprietary and its source code is not
> published here. For the free, open-source edition, see the
> [Syncopation Community Edition](https://github.com/ehajek/Syncopation-Community-Edition).

![Syncopation Pro in iPod mode with an iPod classic verified and ready to sync](docs/images/ipod-mode.png)

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
- **Interrupted syncs don't start over** — the iPod's library is saved as the
  transfer runs, not once at the end. Knock the cable out an hour into a big
  sync and you keep everything transferred so far as a working, playable
  library; sync again and it carries on where it stopped.
- **Reclaims wasted space** — removing a track from an iPod normally leaves
  its audio file behind forever, invisible and unplayable. Syncopation finds
  these strays and clears them automatically. On a real 160 GB classic that
  recovered **33.8 GB** — a fifth of the drive.
- **Keeps your library tidy** — tracks already on the iPod are recognized and
  never duplicated, even ones another app put there, and missing album art is
  filled in as you sync.
- **Safe by design** — existing music and playlists are always preserved,
  every database write keeps a backup on the device, and syncing only adds
  unless you explicitly ask it to match your source folder.
- **Everything the free version does** — one-way and two-way folder/SD-card
  sync with conflict handling, previews, and per-mode file filtering.

![A live sync converting FLAC to Apple Lossless, with the Debug panel showing device details and the per-file log](docs/images/syncing-debug.png)

## Requirements

- macOS 13 (Ventura) or later — Intel or Apple Silicon. On macOS 26 and later the
  app uses the Liquid Glass appearance; on earlier systems it falls back
  to standard native controls automatically.
- An iPod in disk mode: iPod classic (all generations), iPod nano
  (through 4th gen), iPod video, mini, or photo. (iPod touch and iPhone use a
  different sync system and are not supported, nor is the iPod shuffle.)

## Getting your iPod ready

Syncopation talks to the iPod directly, so the one-time setup is about telling
iTunes — or Finder, which handles iPods on modern macOS — to stand down.
Connect the iPod, open its device page (Finder's sidebar on macOS Catalina and
later, iTunes everywhere else), and set it up like this:

1. **Turn OFF "Automatically sync when this iPod is connected."**
2. **Turn ON "Enable disk use" and "Manually manage music and videos."**
   Disk mode is how Syncopation sees the device at all.
3. **Uncheck "Convert higher bit rate songs to AAC."** Syncopation handles
   conversion itself, matched to what the device actually plays.
4. **Uncheck every sync category — Music, Photos, Podcasts, all of them.**
   Nothing should be left for iTunes or Finder to manage.
5. Click Apply, then eject. From here on, Syncopation does the syncing.

One habit worth keeping: **step away from the iPod in Finder before you
eject** — close any windows browsing its files, and click off its device
configuration page. Either one can hold the device busy, and the eject —
from Syncopation or anywhere else — fails with "disk in use" until Finder
lets go.

> [!WARNING]
> **Pick one librarian.** Moving an iPod back and forth between
> iTunes/Apple Music/Finder syncing and Syncopation can scramble the music
> library on the device — each program rewrites the iPod's database in its
> own image. Nothing physical is ever at risk: the worst case is erasing the
> device's music and syncing it again. But save yourself the round trip —
> once an iPod lives with Syncopation, let it.

## Support

- **Questions and bug reports:** [open an issue](../../issues) in this
  repository.
- **How it works under the hood:**
  [docs/TECHNICAL.md](docs/TECHNICAL.md) — the full technical reference for
  the iPod support: device detection, the iTunesDB format, checksums, and
  what these devices really play.

## Status

In active development, heading for the **Mac App Store**.

Working today on real hardware — an iPod classic (7th gen) and an iPod nano
(3rd gen): device identification, FLAC → ALAC conversion, writing the iPod's
native checksummed database, album artwork, checkpointed transfers that
survive a disconnect, and automatic cleanup of files other software left
stranded on the device.

Remaining before submission: moving audio conversion in-process (required by
the App Store sandbox), a compatibility guard for the earliest iPods, and the
usual store paperwork.

Pricing and availability to be announced.

---

Copyright © 2026 Eddie Hajek. All rights reserved.
Syncopation Pro is proprietary software. iPod is a trademark of Apple Inc.;
this product is not affiliated with or endorsed by Apple.
