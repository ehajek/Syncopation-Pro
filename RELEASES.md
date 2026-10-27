# Syncopation — Releases

What each version repaired and what it added. Newest first. Codenames are project names; the product is
Syncopation. The running bulletin board, with an RSS feed, is
[synco.uno/dispatches](https://synco.uno/dispatches).

Numbering: `#.0.x` is a patch — repairs, no new ideas, and it keeps the
family codename. `#.#` is a version, free to everyone who owns the major it
belongs to. `#.0` is a major release, the only kind anyone is asked to pay
for again.

---

## 1.4 · Raffeimentó — Hi-Res Audio, no subscription (in development)

Not released yet; this entry is the draft of what 1.4 carries, kept in step
with the build.

- **Hi-Res Audio, named.** 24-bit files at 96 or 192 kHz play in the app,
  show their resolution in Get Info, and copy to cards and players byte for
  byte. An iPod gets the 16-bit copy it was built for, resampled and
  dithered, never more than it can play; the first two iPod generations,
  which never learned Apple Lossless, get FLAC and Apple Lossless as AAC 256
  instead of nothing.
- **Auditoré grew up:** ten bands an octave apart, a parametric mode that
  moves each band's centre and width and makes the end bands shelves, the
  Harman Curve preset, presets for Sennheiser's main models (from AutoEq,
  credited to their measurers), and leveling that plays every song at one
  loudness.
- **Biblio** shows genre, year and label beside every track, sortable;
  Get Info on any track; delete an album from the shelf and the emptied
  folder follows it to the Trash; drop files on the shelf and they are filed
  where they belong, duplicates handled by your rule.
- **Mini Player Mode** (View menu, ⇧⌘P): just the music; the sync controls
  wait out of sight. Close the main window and it is put away, not gone —
  Home, the Dock or closing the mini player brings it back.
- **The whole window from the keyboard:** every face, mode and button has a
  shortcut; Tab walks the collection, the arrows pick a song, Return plays
  it. VoiceOver reads every control. A slim orchid slider on every scrolling
  list, shown while you scroll.
- **Cair** reads Apple's catalog through MusicKit now, with ISRC lookups, and
  decides albums the way 1.3.1 does; its words and its legend are new.
- **The iPod shuffle, 3rd generation** is accepted (the same list as the 4th);
  the 1st and 2nd are turned away by name rather than handed a list they
  can't read.
- **Getting Started, rewritten.** We're an odd bird. We mean no harm.

Repairs: folder names that only change their case now change their case;
names with hidden characters are spelled plainly on the Mac and the card; a
protected file never leaves for a device that can't play it; the iPod's
database keeps its backup on every write and comes back from it if a write
is interrupted; dates in MP3 tags keep their month and day.

---

## 1.3.1 · Shufflé — one cover per album, and Cair hears the album

| Edition | Status |
|---|---|
| **Pro 1.3.1** | **[Mac App Store](https://apps.apple.com/app/syncopation-pro/id6800440498) — released October 22, 2026.** A point release to 1.3. |
| **CE** | Pro-only; CE stays at 1.2.2. |

Two changes, both about the album.

### Added

- **One cover per album on the iPod classic.** A click-wheel iPod draws a
  new Cover Flow cover whenever the Artist or Album text changes between
  neighbouring tracks, letter for letter, and it never reads Album Artist —
  so a guest credit or a stray capital gave one album two covers. Syncopation
  now writes the Sort Artist and Sort Album fields and the compilation flag
  the firmware groups by, worked out from the album itself; the display tags
  are untouched. An iPod synced before 1.3.1 is rewritten once at its next
  sync, with no files copied. Measured on a 4,636-track classic: 994 covers
  became 816, for 824 albums.
- **Synco-cair decides the album, not the song.** Cair used to name songs one
  at a time and file each where it stood. Now it hears every track in a
  folder, then decides the folder once: filed as that album, offered as one
  review row when it only looks like that album, or left as it is. A disc set
  comes together as one album. A track by another act is moved out; a track
  that belongs stays. An album an older organizer scattered across artist
  folders is rejoined. Act and album names are spelled the way the store
  spells them today.
- **Fewer questions, the same answer every run.** On a 5,700-track test
  library the held rows fell from 2,687 to 130, and the 130 are real
  questions: sound-alike mixes, true duplicates, other editions. Runs are
  deterministic — the same library gives the same result every time.

### Repaired

- **A partial album keeps its own name**, not a deluxe edition's.
- **One-letter album names are albums.** They are no longer treated as
  missing.
- **Apply on a duplicate keeps both**, numbered.

---

## 1.3 · Shufflé — the iPod shuffle and Rockbox

| Edition | Status |
|---|---|
| **Pro 1.3** | **[Mac App Store](https://apps.apple.com/app/syncopation-pro/id6800440498) — released September 21, 2026.** |
| **CE** | 1.3 is Pro-only; CE stays at 1.2.2. |

The one iPod Syncopation always turned away, let in.

### Added

- **iPod shuffle, 4th generation.** A shuffle plays from a list of its own,
  `iTunesSD`, rather than the database every other click-wheel iPod uses, so
  it has a writer of its own — checked byte for byte against the list Finder
  writes. It has no screen, so it speaks: track names and its own status
  messages, recorded on the Mac, offline, in a voice chosen in Settings →
  Shuffle VoiceOver from the voices installed for the system language. The
  whole library goes on, or one Biblio playlist becomes the shuffle's list,
  in playlist order. No artwork. The 1st–3rd generations are refused.
- **Rockbox players** are synced as MP3 players. Music files are managed as
  expected.
- **One converter for every iPod.** FLAC, WAV and AIFF all become 16-bit
  Apple Lossless through one Core Audio pipeline. WAV and AIFF used to be
  copied as-is, which only works for a narrow set of them — see the WAV rule
  in [`docs/TECHNICAL.md`](docs/TECHNICAL.md).
- **Synco-cair** listens further into a song, where the music is, and tries a
  second window before giving up. Held rows say which source to believe
  first.
- **Settings by mode** — General, Biblio, Cair — and a **Getting Started**
  sheet under Help.

### Repaired

- **The library index reads fast on any Mac**, Intel included.
- **Covers finish what they start.** A match confirmed after the run gets its
  cover too, and the covers bar closes when the last one lands.
- **Match asks once.** It no longer asks about removals on every sync.
- **Erase leaves a player's hidden files alone**, so a Rockbox firmware
  folder survives.
- **An empty destination says so.**

---

## 1.2.2 · Bibliotek — repairs and organize

| Edition | Status |
|---|---|
| **Pro 1.2.2** | **[Mac App Store](https://apps.apple.com/app/syncopation-pro/id6800440498) — released September 14, 2026.** Replaces the 1.2 and 1.2.1 submissions; adds Cair's per-track organize. Verified on three cards and a restored iPod. |
| **CE 1.2.2** | **GitHub — September 3, 2026.** Rebuilt on this same engine with the Pro-only features removed, so the editions stop drifting apart. |

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

| Edition | Status |
|---|---|
| **Pro 1.2** | Delivered to App Store Connect September 2, 2026; superseded by 1.2.2. |
| **CE** | 1.2 is Pro-only; CE went from 1.1 to 1.2.2. |

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

| Edition | Status |
|---|---|
| **Pro 1.1** | **[Mac App Store](https://apps.apple.com/app/syncopation-pro/id6800440498) — released August 21, 2026.** The first Pro release to reach the public; 1.0 and the 1.0.1 repairs ship inside it. |
| **CE 1.1** | **GitHub — released August 20, 2026.** |

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

| Edition | Status |
|---|---|
| **Pro 1.0.1** | Rolled up into 1.1 — never shipped on its own number. |
| **CE 1.0.1** | **GitHub — released August 16, 2026.** |

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

| Edition | Status |
|---|---|
| **Pro 1.0** | Submitted to App Review August 11, 2026; rolled up into 1.1. |
| **CE 1.0** | **GitHub — released August 8, 2026.** Codename assigned in retrospect. |

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
