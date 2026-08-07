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

Model identification uses the **serial-number suffix** (last 3 chars) against
a complete table generated from the libgpod project's device database
(`IPodModels.swift` — 242 suffixes, 198 model numbers). The iPod line is
discontinued, so this table is final. Order: serial suffix → model number →
FamilyID → generic.

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

- **Album artwork** — lives in a separate `ArtworkDB` + `.ithmb` thumbnail
  files in device-specific pixel formats (the accepted formats are listed in
  `SysInfoExtended` → `ImageSpecifications`). Planned.
- **iPod shuffle** — uses `iTunesSD`, a different database entirely. Refused
  with a clear message.
- **hash72/hashAB devices** (nano 5G+, iPod touch, iPhone) — refused.

## References

- libgpod (the gtkpod project) — the reverse-engineered format reference:
  https://github.com/gtkpod/libgpod
- The hash58 port retains its BSD attribution in `IPodDB.swift`
  (Copyright 2007 Christophe Fergeau, based on work by wtbw).
