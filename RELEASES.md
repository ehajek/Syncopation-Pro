# Syncopation — Releases

What each version repaired and what it added. Newest first. Codenames are project names; the product is
Syncopation. The running bulletin board, with an RSS feed, is
[synco.uno/dispatches](https://synco.uno/dispatches).

Numbering: `#.0.x` is a patch — repairs, no new ideas, and it keeps the
family codename. `#.#` is a version, free to everyone who owns the major it
belongs to. `#.0` is a major release, the only kind anyone is asked to pay
for again.

---

## 1.2.2 · Bibliotek — repairs

| Edition | Build | Status |
|---|:---:|---|
| **Pro 1.2.2** | 8 | Submitted to the Mac App Store, September 3, 2026 — replaces the 1.2 (5) and 1.2.1 (6) submissions. Verified on three cards and a restored iPod. |
| **CE 1.2.2** | 4 | **GitHub — September 3, 2026.** Rebuilt on this same engine with the Pro-only features removed, so the editions stop drifting apart. |

Point releases keep the family name. A run of fixes that all trace back to the
same afternoon on real hardware: an exFAT card loaded before 1.2, and a
classic that had been restored down to bare metal.

### Repaired

- **One name, two spellings.** *Issue:* FAT-family cards can't hold `?`, a
  trailing period, or a trailing space. The macOS exFAT driver that came
  before the current one stored those as invisible private-use characters,
  and a library that once crossed such a card carries them. The current
  driver lists the real character and finds the file by either spelling.
  Compared literally, one file looked like a stranger to remove and a track
  to copy — every sync — and with Match Default Source on, the card lost a
  little more each run. *Repair (1.2.1):* names are compared by what they
  mean, not how they're spelled, so the card converges.
- **A changed track couldn't be replaced.** *Issue:* the same driver deletes
  a file only by the spelling it met first in that session — Finder's if the
  card was browsed, ours if we wrote it. A track with such a name that had
  changed in the library (retagged, new art) failed with "couldn't be
  removed" and stayed old. *Repair (1.2.2):* every delete against a card —
  the replace step of a copy, Match removals, Erase — offers each spelling of
  the name and takes whichever the driver honours. Nothing on the card needs
  erasing; the track is replaced on the next run.
- **The same song, twice, cleans itself up.** *Issue:* a track that another
  program (or a past mishap) left on a device a second time was kept forever —
  Match only removes songs that aren't in your library, and a duplicate still
  matches one. The only cure was a full erase and re-sync, slow and hard on an
  ageing drive. *Repair (1.2.2):* at the very start of a sync, before anything
  is copied or matched, duplicates already on the device are folded to a single
  copy — the one that matches your library, or the one whose file is actually
  present. A duplicate is judged by title, artist, album **and** byte size, so
  a distinct edit that happens to share tags is never mistaken for a twin. On
  the iPod this covers music, podcasts and audiobooks; on an SD card or MP3
  player it keeps the copy that sits at its proper library path. No erase.
- **A restored iPod stops borrowing another's name.** *Issue:* an iPod erased
  and reinstalled loses the file that names its model, and Syncopation fell
  back to the last iPod macOS had seen — so a restored classic could show up
  wearing a nano's name. *Repair (1.2.2):* the model is read from that exact
  device's own record by its hardware ID, never "the last one seen." When
  nothing on the device names it, it shows its family — "iPod classic" — and
  syncs normally; see the [known issue](https://github.com/ehajek/Syncopation-Pro/issues/2)
  for how to restore the exact model.
- **An iPod isn't a folder.** *Issue:* the MP3 Player destination list offered
  the iPod's own volume, which would fill it with loose files the iPod never
  shows in its menus. *Repair (1.2.2):* iPod volumes are kept out of that list
  and refused as a plain folder — an iPod belongs in the iPod tab, where it
  gets a real database.
- **The picker says so after an eject.** *Issue:* eject a card and the
  destination field kept quoting a path that no longer existed; an empty iPod
  slot read like a selection, so a stray click on Sync could aim at nothing.
  *Repair (1.2.2):* the field becomes a prompt — "Choose a card or folder…",
  "Choose an iPod…" — in the attention colour, the panel below says what
  happened, and no iPod is pre-selected when the app opens. The remembered
  path is kept: reinsert the card and it is the destination again.
- **Erase tells the truth.** *Issue:* an erase that couldn't remove a folder
  still reported "Done — erased". *Repair (1.2.1):* it reports how many items
  went and how many stayed, and the log says why.

### Added

Nothing. Repairs only.

---

## 1.2 · Bibliotek · Thecaire · Auditoré

| Edition | Build | Status |
|---|:---:|---|
| **Pro 1.2** | 5 | Delivered to App Store Connect September 2, 2026; superseded by 1.2.2. |
| **CE** | — | 1.2 is Pro-only; CE went from 1.1 to 1.2.2. |

Three names under one number because three engines arrived at once.
Everything you own, in one window: once an app can see a collection rather
than merely copy it, considerably more becomes possible.

### Added

- **Synco-biblio** *(Pro)* — the library as a shelf and as a wall of covers;
  an album slides out to its tracks. Playlists you build and reorder, sent to
  the iPod one at a time or all together. Music dropped in is copied into the
  library, loose tracks filed by their tags. Playback with a Now Playing
  card, and a chip that stays out of the way.
- **macOS Now Playing** *(Pro)* — the system's Now Playing follows the
  player: track, cover and position in Control Center and the menu bar. The
  keyboard's media keys and the buttons on your headphones play, pause, skip
  and scrub it.
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

- Artwork on pre-2007 iPods — older thumbnail formats, unverified against
  hardware; music syncs normally, artwork is skipped.
- iPod shuffle — detected and refused with a clear message.
- hash72 / hashAB devices (nano 5G and later, iPod touch, iPhone) — refused.

---

Copyright © 2026 Eddie Hajek. iPod is a trademark of Apple Inc.; Syncopation
is not affiliated with or endorsed by Apple.
