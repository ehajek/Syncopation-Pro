# Syncopation — Releases

What each version repaired, what it added, and how the two editions compare
at that version. Newest first. Codenames are project names; the product is
Syncopation. The running bulletin board, with an RSS feed, is
[synco.uno/dispatches](https://synco.uno/dispatches).

Numbering: `#.0.x` is a patch — repairs, no new ideas, and it keeps the
family codename. `#.#` is a version, free to everyone who owns the major it
belongs to. `#.0` is a major release, the only kind anyone is asked to pay
for again.

---

## 1.2 · Bibliotek · Thecaire · Auditoré — in review

| Edition | Build | Status |
|---|:---:|---|
| **Pro 1.2** | 5 | **Delivered to App Store Connect September 2, 2026 — in review.** A free update for everyone who owns 1.x. |
| **CE** | — | Not handed 1.2. CE stays at 1.1. |

Three names under one number because three engines arrived at once.
Everything you own, in one window: once an app can see a collection rather
than merely copy it, considerably more becomes possible.

### Added

- **Synco-biblio** *(Pro)* — the library as a shelf and as a wall of covers;
  an album slides out to its tracks. Playlists you build and reorder, sent to
  the iPod one at a time or all together. Music dropped in is copied into the
  library, loose tracks filed by their tags. Playback with a Now Playing
  card, and a chip that stays out of the way.
- **Synco-cair** *(Pro)* — album art and album names, put right. It reads
  what is already there first and leaves complete albums alone; whatever is
  missing a cover, carrying a thumbnail, or wearing the wrong name is looked
  up across the free music databases. The sure ones fix themselves; the rest
  line up for one look and one click. Audio is never re-encoded. Video files
  are never touched.
- **Synco-auditoré** *(Pro)* — a five-band equalizer and gain on playback,
  live while a track plays, remembered between launches. Playback only:
  nothing is written to files, nothing changes on the iPod.
- **One playlist or all of it** *(Pro)* — the sync face's source control
  became a Sync picker: the full library, or any one playlist, to the iPod.
  Match mirrors whichever is chosen.
- **Synco-pod, finished** *(Pro)* — podcasts file to the iPod's own Podcasts
  menu the way audiobooks already did: resuming where you stopped, kept out
  of shuffle. Tested on the nano.
- **Library Scanning** *(Pro)* — a screen that holds the door while the
  index reads, so nothing runs on a half-read shelf.
- **Prevent Syncopation from Internet Access** *(Pro)* — a switch in
  Settings that does exactly that. Synco-cair goes dark; nothing else ever
  needed the network in the first place.

### Repaired

Nothing. Additions only.

### Pro vs CE at 1.2

| | **Pro** | **CE** |
|---|:---:|:---:|
| Synco-biblio — shelf, wall of covers, playlists, playback, import | ✓ | — |
| Synco-cair — album art and album names put right | ✓ | — |
| Synco-auditoré — five-band equalizer and gain on playback | ✓ | — |
| Sync one playlist or the whole library | ✓ | — |
| Podcasts filed to the iPod's Podcasts menu | ✓ | — |
| Library Scanning screen | ✓ | — |
| Prevent Syncopation from Internet Access | ✓ | n/a — nothing in CE reaches the network |

---

## 1.1 · Podification — August 20, 2026

| Edition | Build | Status |
|---|:---:|---|
| **Pro 1.1** | 4 | **[Mac App Store](https://apps.apple.com/app/syncopation-pro/id6800440498) — released August 21, 2026.** The first Pro release to reach the public; 1.0 and the 1.0.1 repairs ship inside it. |
| **CE 1.1** | 3 | **GitHub — released August 20, 2026.** |

### Added

- **Synco-pod** *(both)* — a three-way control, Music / Podcasts / Audiobooks,
  replaces a pair of tick boxes: three states that exclude one another are a
  picker, not switches kept apart by hand. Filing follows from it — media
  type plus the resume-position, skip-when-shuffling, and unplayed flags the
  iPod has honored since 2005. `.m4b` is an audiobook whichever way the
  control is set. DRM-free files only: Apple Books and Audible purchases are
  copy-protected and cannot make the trip.
- **Appearance** *(both)* — follow the system, or pin the window light
  or dark. The dark theme is built on the synco.uno palette.
- **Liquid Glass and window transparency, each switchable off** *(Pro)* —
  translucency is lovely until it isn't, and nobody should have to read text
  through their own wallpaper.
- **A sound when a sync finishes** *(both)* — whatever alert tone you
  already chose in System Settings, at your volume. It sounds for a failure
  too.
- **One control language** *(both)* — selector tracks and action
  buttons share a height, a type size, and the same glass; the active segment
  is a real button that travels between positions rather than paint that
  jumps. Both meters are drawn by hand so they take the app's tint instead of
  the system accent.

### Repaired

- **Match Default Source removes items, not songs** *(Pro)* — the word
  stopped being true the moment episodes and audiobooks could be synced.
- **Pro also carries every 1.0.1 repair below.** They never shipped on their
  own number.

### Pro vs CE at 1.1

| | **Pro** | **CE** |
|---|:---:|:---:|
| Synco-pod — podcasts and audiobooks filed as such | ✓ | ✓ |
| Appearance — system, light, or dark | ✓ | ✓ |
| Liquid Glass and window transparency switchable off | ✓ | n/a — already flat |
| A sound when a sync finishes | ✓ | ✓ |
| One control language | ✓ | ✓ |
| Match Default Source removes items, not just songs | ✓ | — |

---

## 1.0.1 · Origen — repairs

| Edition | Build | Status |
|---|:---:|---|
| **Pro 1.0.1** | 3 | Rolled up into 1.1 — never shipped on its own number. |
| **CE 1.0.1** | 2 | **GitHub — released August 16, 2026.** |

Point releases keep the family name. Repairs cross editions when they're
ready; features make the trip on their own schedule.

### Repaired

- **Conversion runs ahead of the copy.** *Issue:* the CPU and the USB cable
  took turns — a track was converted, then copied, then the next one
  started — so a large first sync ran at the sum of both. *Repair:* a few
  converted tracks are kept ready ahead of the transfer. Roughly half the
  time on machines where conversion was the bottleneck.
- **The Mac stays awake mid-sync.** *Issue:* idle sleep cut USB power and
  took the iPod with it, politely, halfway through. *Repair:* a power
  assertion is held for as long as a sync runs. The display may sleep; the
  sync may not.
- **Space math got honest about hi-res.** *Issue:* converted tracks were
  priced at their source size, so a 24-bit album that would shrink on
  conversion was refused even though it fit. *Repair:* the free-space check
  estimates what a track will weigh on the device, capped at the source size.

### Added

Nothing. Repairs only.

### Pro vs CE at 1.0.1

| | **Pro** | **CE** |
|---|:---:|:---:|
| Conversion pipelined ahead of the copy | ✓ | ✓ |
| The Mac stays awake while a sync runs | ✓ | ✓ |
| Space check prices hi-res tracks at their converted size | ✓ | ✓ |

---

## 1.0 · Origen

| Edition | Build | Status |
|---|:---:|---|
| **Pro 1.0** | 2 | Submitted to App Review August 11, 2026; rolled up into 1.1. |
| **CE 1.0** | 1 | **GitHub — released August 8, 2026.** Codename assigned in retrospect. |

### Added

The first release. Music onto iPods and MP3 players, books onto e-readers,
anything at all onto folders, cards, and drives. FLAC converts to Apple
Lossless on the way to an iPod; the iPod's own database is written natively;
nothing ever touches a server because there is no server.

### Known issues at 1.0

All three repaired in 1.0.1: conversion and copy took turns; idle sleep
could interrupt a long sync; hi-res albums that would fit after conversion
were refused on principle.

### Not supported

- Artwork on pre-2007 iPods — older thumbnail formats, not yet verified
  against hardware; music syncs normally, artwork is skipped.
- iPod shuffle — detected and refused with a clear message. Not planned.
- hash72 / hashAB devices (nano 5G and later, iPod touch, iPhone) — refused.

### Pro vs CE at 1.0

| | **Pro** | **CE** |
|---|:---:|:---:|
| Four Synco-modes — Music, ePUB/PDF, All Files, iPod | ✓ | ✓ |
| Native iPod database engine — hash58 checksums, tracks play on eject | ✓ | ✓ |
| FLAC → Apple Lossless, 16-bit, in-process | ✓ | ✓ |
| iPod identification | Exact model — "iPod classic (7th gen)" | Family only — "iPod classic" |
| Album artwork on the iPod | ✓ | — |
| Skips tracks already on the device | By tags — even ones another app put there | By file |
| Interrupted sync | Checkpointed — carries on where it stopped | Stops cleanly — tidies up on the next run |
| Reclaims stray files other software left behind | ✓ | — |
| Two-way sync for folders and SD cards | ✓ | — |
| Match Default Source — device mirrors the folder, deletions included | ✓ | — |
| Preview, Erase, Eject, free-space check | ✓ | ✓ |
| Debug drawer — device facts and the per-file log | ✓ | ✓ |
| Interface | Liquid Glass on macOS 26, native controls on 13–15 | Flat, native |
| Distribution | Mac App Store — sandboxed, notarized, reviewed | GitHub — clone it, build it |
| Price | $19.99 once; $24.99 from January 1, 2027 | Free, GPL v3 |

---

Copyright © 2026 Eddie Hajek. iPod is a trademark of Apple Inc.; Syncopation
is not affiliated with or endorsed by Apple.
