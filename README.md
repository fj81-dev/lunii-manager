# Lunii Manager

[Download Lunii Manager 1.4.0 for macOS](https://github.com/fj81-dev/lunii-manager/releases/download/1.4.0/Lunii-Manager-1.4.0.dmg)

A native macOS app to manage stories on your **Lunii v1 (upgraded to FW2), v2, v3 or FLAM** storyteller, without going through Luniistore.

Local library, drag-and-drop, built-in player, official Lunii account sign-in, firmware updates — all packaged in one self-contained app that runs offline whenever it can, and adapts to the way you actually organize your stories.

> 🪟 **A Windows version is currently in development.** macOS is the primary platform today, but a Windows port is on the way; no ETA yet — follow this repo for updates.

<p align="center"><img src="screen.webp" alt="Overview" /></p>

---

## What you can do

### A library you own

Keep every story you have (bought, shared, custom-made with Lunii Studio) in **one folder** — on your Mac or even on a NAS share. Lunii Manager scans the folder (and all its subfolders), shows each pack with its cover, title, age range, size, and lets you filter / sort however suits you.

- Drop in `.zip` or `.7z` archives — either Lunii Studio editor exports or the FS-backup format Lunii Manager produces itself.
- Live search on title, subtitle, description, file name or UUID.
- Filters: age range ("from 5 years", "up to 7 years"…), official vs. custom, already on the Lunii or not, night-mode availability.
- Sort by title, age, weight, date added.
- Stories built with Lunii Studio (raw editor zips) get **automatically converted** to the Lunii format (BMP images, MP3 mono audio) at install time — no third-party tools needed.

### Effortless device management

Plug your Lunii in over USB and it shows up in the window with its nickname, colour, serial number, firmware version and free space.

- **Rename** your Lunii ("Léa's Lunii", "Lucas's Lunii"…) and pick its real shell colour.
- **Reorder stories** by drag-and-drop, push one to the top, to the bottom, or remove it with a single click.
- **Back up** the Lunii — three flavours behind the toolbar button: copy all stories to your library, copy all stories to **any folder** (USB stick, NAS…), or take a **bit-for-bit `.img` dump** of the whole SD card. The dump asks for your macOS password (because reading the raw disk needs root) and shows a live progress bar.
- **Open an `.img`** dump back as if it were a Lunii (File → Ouvrir une image SD…). Mount it in **read-only** (⇧⌘O) to preserve the original byte-for-byte, or read-write (⌘O) to test the app against the dump.
- **Wipe** the Lunii (clears all stories, keeps the device settings).

<p align="center"><img src="rename.webp" alt="Rename" /></p>

### Drag and drop everywhere

- Drag a story from your **library** onto your **Lunii** to copy it over.
- Drag a story from your **Lunii** back into your **library** to save it.
- Drag a story **from one Lunii to another** when two are plugged in.

<p align="center"><img src="copy-from-library.webp" alt="Copy from the library" /></p>

### Several Lunii at once

Plug in two (or more) and each one gets its own pane, with independent selection and its own drop target.

<p align="center"><img src="two-lunii-connected.webp" alt="Two Lunii connected" /></p>

### Official Lunii account

Sign in with your real Lunii account (the one on `lunii.com`) to get back every story you've bought or have through your subscription. **Multi-select** several stories (cmd-click / shift-click), then right-click → "Télécharger N histoires sur <Lunii>": they queue up and write to the device one after the other while you keep using the app. Stories already on the device are caught up front (one sheet listing them with their covers, then the download skips straight to the rest).

<p align="center"><img src="account.webp" alt="Account" /></p>

### Built-in player

Want to listen to a story without putting it on the Lunii? Click **"Listen"** from any story (library or device) and the player opens up: it **looks like a real Lunii**, painted in your Lunii's colour, with the wheel, OK / Home / Pause / ⏯ buttons and the same navigation as the real device.

<p align="center"><img src="player.webp" alt="Player" /></p>

It plays MP3, WAV, OGG Vorbis and FLAC. Keyboard shortcuts: left / right arrows for the wheel, Return for OK, H for Home, Space for pause.

<p align="center"><img src="screen-wtih-player.webp" alt="Player in the main window" /></p>
<p align="center"><img src="player-blue.webp" alt="Player — different colour" /></p>

### Firmware updates

When the Lunii servers offer a new firmware for your device, Lunii Manager picks it up, downloads it, verifies its integrity and stages it onto the SD card. The actual flash happens on the Lunii's next boot — exactly like Luniistore. **You can still roll back**: as long as you haven't unplugged + replugged the Lunii, open the mounted volume in Finder and delete the `FU.BIN` / `FA.BIN` files at its root — the next boot will then ignore the staged firmware.

### Multilingual

UI translated into **French, English, German, Spanish, Italian, Portuguese, Dutch, Polish, Swedish and Danish**. Follows your macOS system language by default, or pick it manually from the **Lunii Manager → Language** menu.

---

## Requirements

- **macOS 14 (Sonoma) or later.**
- **Apple Silicon or Intel Mac.** Lunii Manager ships as a universal binary.
- **Lunii v1 (upgraded to firmware 2), v2, v3 or FLAM**, mounted as a USB volume.
  - **Lunii v1** must be upgraded to firmware 2 first (free, one-click from Luniistore). Once upgraded it takes the same USB-mass-storage code path as a v2. Raw v1 firmware (pre-upgrade) is not supported.

### v3 / FLAM — what to expect

> ⚠️ **Support for v3 and FLAM is experimental for now** — the code paths are fully implemented against the reverse-engineering notes and cross-validated against the existing Java / Python reference clients, but I personally only own v1 and v2 hardware, so the v3 and FLAM paths haven't been exercised on my own devices. It should work, but **back your card up** before letting Lunii Manager write to it, and please report anything that misbehaves — feedback from v3 / FLAM owners is how we get this out of "experimental".

Lunii's third-generation devices (v3 colour-screen and FLAM) protect their stories with per-device crypto keys baked into the microcontroller — those keys never leave the device. Most of what you'd want to do still works, but a few corners are blocked by design:

| Operation                         | Custom story (Lunii Studio, community pack)           | Official story (bought / subscription)                                                 |
|-----------------------------------|-------------------------------------------------------|----------------------------------------------------------------------------------------|
| List, reorder, delete             | ✅                                                     | ✅                                                                                      |
| Add from your library             | ✅                                                     | ✅ — only on the Lunii of origin (we tag the backup with its SNU and refuse mismatches) |
| Add from your account             | n/a                                                   | ✅                                                                                      |
| **Listen in the built-in player** | ✅                                                     | ❌ — listen on the device itself                                                        |
| **Back up to library**            | ✅ — re-importable on any Lunii (XXTEA "pivot" format) | ✅ — verbatim bytes + sidecar SNU, only re-installable on the source Lunii              |
| **Copy to another Lunii**         | ✅                                                     | ❌ — re-download it on the target from your account instead                             |

Official v3 packs show a **🔒 "Encrypted"** badge and pill in the side panel. The Play button is replaced with a disabled "Playback unavailable (encrypted)" label so you know up front the player can't open them.

---

## Installation

1. Download the `.dmg` from the releases page.
2. Open the `.dmg` and drag **Lunii Manager** into **Applications**.
3. The first launch will be blocked by macOS Gatekeeper because the app isn't signed by a paid Apple Developer ID: **right-click Lunii Manager → Open**, then confirm. macOS will remember from then on.
4. The first time you plug in a Lunii, macOS will ask you to grant access to **removable volumes**: allow it. If your library is on a NAS, also allow access to **network volumes**.

> Clicked "Don't Allow" by mistake? **Lunii Manager → Reset permissions…** replays the system prompt.

---

## Notes

- **Free software, personal non-commercial use only.** Not affiliated with Lunii SAS — this is an independent project. *Lunii* is a trademark of Lunii SAS.
- **If you paid for this app, you got scammed**: it's not sold anywhere.
- No external dependencies: no Homebrew, no Python, nothing else to install. Just drag the `.app`.

---

## License

Software provided "as is", without any warranty. All rights reserved — no redistribution, modification, sublicensing or commercial use without prior written permission.

© FJ81
