# Syncopation Pro — iPod Technical Notes

> How Syncopation Pro talks to iPods. Published for the iPod community —
> much of this format knowledge is documented almost nowhere else.

How Syncopation talks to iPods, and the hard-won lessons behind it. Everything
here was verified on real hardware (iPod classic 7th gen, iPod nano 3rd gen)
in August 2026.

## How an iPod is detected

An iPod in disk mode is just a mounted volume containing `iPod_Control/Device/`.
Identity comes from two files there:

- **`SysInfo`** — plain `Key: value` text. Often **completely empty** on
  classics; don't rely on it.
- **`SysInfoExtended`** — XML written by iTunes/Music on first sync.
  **Warning: it is not valid XML.** The classic's firmware nests `<key>`
  elements directly inside `<array>` (in `ImageSpecifications`), which makes
  every strict plist parser fail — including Apple's own
  `PropertyListSerialization` and Python's `plistlib`. Syncopation extracts
  the keys it needs by plain text scanning.

Keys that matter:

| Key | Meaning |
|---|---|
| `DBVersion` | **Authoritative** database rule: ≤2 = plain iTunesDB, 3 = hash58 checksum, 4 = hash72, 5 = hashAB. Syncopation supports ≤3 and refuses ≥4. |
| `FireWireGUID` | 16 hex chars; keys the hash58 checksum. |
| `SerialNumber` | Last 3 characters identify the exact model (see below). |
| `FamilyID` | Device family (11 = classic, 12 = nano 3G). Fallback naming only. |
| `AudioCodecs` | The device's own declaration of what it can play. |

### Device identification — a proprietary, independently built database

Model identification uses the **serial-number suffix** (last 3 characters)
against a complete device table that is **Syncopation Pro's own proprietary
work**. It contains **no libgpod, gtkpod, or other LGPL-derived data**, and is
not published.

It was compiled independently from public reference sources: Apple's own
"Identify your iPod model" support documents and its archived serial-suffix
lists, EveryMac's per-model specifications, and the Chipmunk International
model database. Building it from scratch — rather than reusing the tables that
open-source projects ship — was a deliberate decision, so that the shipping
application carries no copyleft-licensed data and its identification layer is
wholly owned.

The iPod line is discontinued, so the table is final: every model Apple made
is covered, and no future device can be missing from it. Lookup order is
serial suffix → model number → FamilyID → generic fallback.

To be unambiguous about what is and isn't borrowed: libgpod is credited below
as the published reference for the *binary database formats* (and for the
BSD-licensed hash58 routine, whose attribution is retained in the source).
**Device identification is not among those borrowings.**

## The iTunesDB

Stock Apple firmware plays only what's registered in
`iPod_Control/iTunes/iTunesDB` — a little-endian binary database of nested
records (`mhbd` → `mhsd` sections → track/playlist records with `mhod`
children). Files themselves are scattered into hidden `Music/F00`–`F49`
folders under 4-letter names; the paths in the database (colon-separated,
e.g. `:iPod_Control:Music:F08:XQZR.m4a`) are what the firmware follows.

### Section layout — deviate and the iPod bricks itself to the restore screen

The firmware expects the exact section set and order that iTunes writes:

```
mhsd 4  albums (mhla/mhia)          ← FIRST, before tracks
mhsd 1  tracks (mhlt/mhit)
mhsd 3  playlists — duplicate of the master playlist
mhsd 2  playlists — master + user playlists
mhsd 8  artists (mhli/mhii)
mhsd 6  empty
mhsd 10 empty
mhsd 5  smart playlists: THE DEVICE'S MENU CATEGORIES
mhsd 9  genius cuid blob
```

**The single most important lesson:** `mhsd` type 5 contains six smart
playlists — *Music, Videos, Movies, TV Shows, Audiobooks, Rentals*. These are
not user playlists; they are the firmware's **main menu**. Write an empty
type 5 and the iPod reboots to **"Use iTunes to restore"** even though the
filesystem and every other structure is intact. Syncopation preserves these
playlists verbatim (raw mhod 50/51 blobs) across every rewrite, along with
the genius section.

Modern Music.app writes a leaner variant (6 sections, version 0x75, no types
8/6/10) — but type 5 with the six menu playlists is present even in a
freshly-initialized empty database. It is mandatory furniture.

### hash58

Devices with `DBVersion 3` (classic 6G/7G, nano 3G/4G) reject any iTunesDB
whose checksum at mhbd offset 0x58 doesn't validate. The algorithm (ported
from libgpod's `itdb_hash58.c`, BSD license): a key derived from the
FireWire GUID via LCM + substitution tables + SHA-1, then an HMAC-SHA1-style
double hash over the database with the dbid (0x18), unknown-0x32, and hash
fields zeroed and the hashing-scheme field (0x30) set to 1. Syncopation's
Swift port is verified bit-identical against an independent implementation
and accepted by real hardware.

### Add to Library vs Match Default Source

By default an iPod sync only ever **adds**: new music is copied on, and
nothing is removed. Deleting an album from the source folder leaves it on the
iPod forever.

Ticking **Match Default Source** makes the iPod match the source folder instead. After
the add pass, any track in the library whose title/artist/album is no longer
found in the source is removed — its file deleted, its record dropped from the
database and from every playlist, and its manifest entry cleared. This includes
music other software put on the device, since "matches the source folder" can't
mean anything else.

Two gates protect it, because it is the only path that deletes music the iPod
can currently play: the checkbox explains itself when ticked, and the sync stops
to show the exact list, count and size before removing anything. **Preview never
removes** — it lists what would go.

Erasing is separate: an **Erase…** button beside Refresh and Eject wipes the
destination on its own, without syncing afterwards. On an iPod it deletes every
audio file, empties the library and clears artwork, while leaving the device's
own menus, settings and firmware untouched; in the folder modes it empties the
destination folder. It asks once, with the real item count, and does nothing
else — no copy follows.

### Album artwork

Covers are **embedded in the audio files themselves** — a `PICTURE` block in
FLAC, an `APIC` frame in MP3, a `covr` atom in M4A — and Syncopation reads
them straight from the source file. On the iPod they live somewhere else
entirely: a separate `ArtworkDB` plus `.ithmb` files holding raw pre-rendered
thumbnails, in the exact pixel formats that device accepts. Each track links
to its cover through three fields in its `mhit` record (`has_artwork`,
`artwork_count`, and `mhii_link` → an `mhii` id in the ArtworkDB).

The classic 6G/7G and nano 3G share four cover formats, all RGB565
little-endian: **1061** (55×55, padded to a 56-pixel row stride), **1060**
(320×320), and **1068** / **1055** (both 128×128). Thumbnails are appended to
`F<format>_1.ithmb` and referenced by byte offset, so existing artwork is
never rewritten — only extended.

**Artwork is filled in automatically as part of every sync**, not as a
separate command. After the copy pass, any track in the library without a
cover — including music another app put there — is matched to a source file
by tags, and its embedded art is encoded and linked. One cover serves an
entire album, and art already on the device is reused rather than duplicated,
so a library of thousands of tracks costs only as many thumbnails as it has
distinct albums. Tracks whose source file can't be found, or whose file has
no embedded cover, are counted and reported; nothing fails.

### Stray files, and cleaning them up

Removing a track from an iPod's library does not delete its audio file. Most
software (including iTunes and Swinsian) leaves the file behind, so drives
accumulate **orphans**: audio in `iPod_Control/Music` that no database record
points at. The iPod can't play them and won't show them, but they still
occupy the disk. A real example: a 160 GB classic under test held 7,118 audio
files while its library listed only 4,542 — **2,576 orphans wasting 33.8 GB**,
a fifth of the drive.

**Cleanup runs automatically at the end of every sync**, once the database is
safely written. Every orphan is identified and its tags read, then:

- an orphan whose song **is** listed in the library is a redundant copy and is
  deleted (this is the common case, and it is lossless);
- an orphan whose song is **not** in the library anywhere is music the device
  has lost. It is **never deleted** — it's reported in the log so it can be
  recovered deliberately.

The result: space is reclaimed without a prompt, because only provably
redundant data is removed, while anything irreplaceable survives untouched.

### Safety model

- The existing database is parsed and **everything is preserved**: tracks,
  playlists, smart playlists, genius data, library IDs.
- Before writing, the original is copied to `iTunesDB.syncopation.bak` on
  the iPod. Restoring it (rename back, eject) undoes any sync.
- A manifest (`iPod_Control/Syncopation/manifest.json`) maps source files to
  database entries for skip-if-synced; tracks loaded by other apps are
  recognized by tag matching (title/artist/album) and never duplicated.
- Sync only **adds**. Nothing is ever deleted from the device.

## Audio formats — what these iPods actually play

**The 16-bit / 44.1–48 kHz ceiling is a limitation of the iPod hardware
itself, not of Syncopation.** Every click-wheel iPod (classic, nano, mini,
video, photo, shuffle) has a 16-bit playback pipeline — the devices predate
hi-res audio entirely, and each one declares these limits in its own system
files. Files with more depth are truncated by the firmware at best, or fail
to decode at all. Syncopation converts to the best format the device can
genuinely play; your original hi-res files are never modified.

The device declares its limits in `SysInfoExtended` → `AudioCodecs`. For
both the classic and nano 3G:

| | Limit |
|---|---|
| ALAC sample rate | 48 kHz max |
| ALAC bit depth | 16-bit playback (`MaximumBitDepthUntruncated: 16`). Deeper files are truncated — **and 32-bit ALAC mostly fails to decode at all** (track blips and skips). |

**The conversion trap:** CoreAudio decodes FLAC to 32-bit integers, so a
naive `afconvert -f m4af -d alac in.flac out.m4a` produces **32-bit ALAC**
(~2,300 kbps) regardless of the source depth — files the iPod can't reliably
play. Syncopation therefore converts in two steps through an explicit 16-bit
PCM intermediate:

```
afconvert -f WAVE -d LEI16@44100  in.flac  tmp.wav
afconvert -f m4af -d alac         tmp.wav  out.m4a
```

Hi-res rates are halved to stay legal (88.2/176.4 → 44.1, 96/192 → 48).
This costs nothing audible: the iPod truncates to 16-bit anyway, and the
24-bit masters stay untouched at the source.

Formats copied as-is: MP3, M4A/AAC, M4B, WAV, AIFF. Formats the firmware
can't play are skipped and reported: OGG, Opus, WMA, APE, DSF, DFF.

## Styling and OS compatibility

The app runs on **macOS 14 (Sonoma) and later** on Apple Silicon, and adapts
its appearance to the system it finds:

| System | Appearance |
|---|---|
| macOS 26+ | Liquid Glass — frosted window, glass mode selector, glass buttons and checkboxes |
| macOS 14–15 | The same layout in standard native controls: bordered buttons, material panels |

Liquid Glass APIs (`.glass` / `.glassProminent` button styles, `glassEffect`,
`GlassEffectContainer`) exist only on macOS 26. They are never called
directly: every use goes through `syncoButton()` and `syncoPanel()` in
`Syncopation.swift`, which branch on `if #available(macOS 26.0, *)` and fall
back to `.bordered` / `.borderedProminent` buttons and material-filled
panels. Checkboxes use a custom `ToggleStyle` that renders identically on
every supported system.

Two things keep this honest:

- The build pins `-target arm64-apple-macos14.0`, so **the compiler refuses
  any macOS 26 API that isn't inside an availability check.** This is what
  makes the fallback trustworthy rather than aspirational — the deployment
  target, not discipline, enforces it.
- `LSMinimumSystemVersion` in `Info.plist` matches that target. Change one
  and you must change the other; the binary's `minos` load command should
  agree (`vtool -show-build-version`).

If a user has *Reduce Transparency* enabled in Accessibility settings,
macOS renders the glass materials as flat opaque surfaces automatically.
Nothing in the app needs to handle that case.

## Not yet supported

- **Artwork on pre-2007 iPods** — those generations use older thumbnail
  formats that haven't been verified against hardware, so artwork is skipped
  there (music syncs normally). The formats each device accepts are listed in
  its `SysInfoExtended` → `ImageSpecifications`.
- **iPod shuffle** — uses `iTunesSD`, a different database entirely. Refused
  with a clear message.
- **hash72/hashAB devices** (nano 5G+, iPod touch, iPhone) — refused.

## References

- libgpod (the gtkpod project) — the published reference for the iTunesDB and
  ArtworkDB **binary formats**: https://github.com/gtkpod/libgpod
- The hash58 checksum routine is ported from libgpod's `itdb_hash58.c`
  (BSD licence, Christophe Fergeau / wtbw); that attribution is retained in
  `IPodDB.swift`.
- **Device identification is not derived from libgpod or any other
  open-source project** — see "Device identification" above. That database is
  proprietary to Syncopation Pro and is not distributed.
- The hash58 port retains its BSD attribution in `IPodDB.swift`
  (Copyright 2007 Christophe Fergeau, based on work by wtbw).
