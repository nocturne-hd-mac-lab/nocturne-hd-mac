# Shin Megami Tensei III Nocturne HD Remaster — Unofficial Native Mac Port (Test Build)

> **Unofficial, unfinished test build for Apple Silicon Macs.**
> This package contains **no game files**. You must use your own purchased Windows version (and your own DLC).
> Not affiliated with or endorsed by ATLUS or SEGA. ©ATLUS ©SEGA.


## What Is This?

This is an unofficial native Mac port of *Shin Megami Tensei III Nocturne HD Remaster* for Apple Silicon Macs, put together by a Japanese student looking for volunteer playtesters.

A bundled local converter builds the Mac-ready game files directly on your Mac from your own purchased Windows installation folder (and any owned DLC). No game files are included in the download.

## Current Status & What Needs Testing

- **Runs natively on Apple Silicon** at **30 fps during gameplay** (matching the original) with all **eight original languages** (Japanese, English, French, Italian, German, Spanish, Traditional Chinese, and Korean).
- Tested on my own Mac and by our first outside tester (an M6 Mac mini on macOS 27). Their reports helped fix **garbled English text**, **changing resolution in fullscreen**, and **Xbox controller input**.
- **Still unfinished — your help is welcome!** Mid-game and late-game progression, DLC screens, full playthroughs in languages other than Japanese, 8 GB / 16 GB Macs, external displays, physical controllers (USB and Bluetooth), external-drive folder permissions, and name entry across languages all need real testing.

Please read [known-issues.md](known-issues.md) before playing so you know what has and has not been checked yet.

## Requirements

- **Apple Silicon Mac** (Intel Macs are not supported)
- **Your own purchased Windows version** of *Shin Megami Tensei III Nocturne HD Remaster* (the installation folder containing `GameAssembly.dll`, plus any owned DLC under `dlc/<number>/`)
  - *No Windows PC?* You can still download your own purchased Windows files directly on a Mac (via Wine/CrossOver or the Windows client's command-line downloader). See [handbook.md](handbook.md) for details.
- **Python 3.11** — install the **macOS universal2 installer** from [python.org](https://www.python.org/downloads/macos/)
- **About 36 GB of free disk space** before conversion (or an external drive via `--output`; see [handbook.md](handbook.md))
- **Internet access** on first-time setup (to install the converter's Python packages)

## Quick Install

You do not need any Terminal or Python experience. Setup is one Terminal command:

1. Download the release package ZIP from the [latest release](https://github.com/nocturne-hd-mac-lab/nocturne-hd-mac/releases/latest) and double-click it to extract. Keep the extracted folder together.
2. Open **Terminal** (**Applications → Utilities**).
3. Type `sh ` (with a space after `sh`), drag **`setup.sh`** from the extracted folder into Terminal, and press **Return**.
4. When asked, drag your **Windows installation folder** into Terminal and press **Return**.
5. Leave Terminal open and wait for **`Setup finished.`**
   - Conversion took about **12 minutes** on my Mac (times vary by Mac).
   - If setup is interrupted, run the same command again to resume.
   - Future updates automatically **reuse unchanged files**, so you do not have to reconvert everything.
6. Close Terminal, then double-click the app (`OriginalMaterialFree.app`) inside the extracted folder to play.

### Quick Tips

- **Loading seems stuck?** A macOS folder-access prompt may be hidden behind the game window. Check behind the window and choose **Allow** (or enable access in **System Settings → Privacy & Security → Files & Folders**).
- **Language:** The app follows your Mac's first preferred supported language (defaulting to English). See [handbook.md](handbook.md) to launch in a different language with `--original-language`.
- **Entering names:** Pick characters from the on-screen character table (direct keyboard typing and OS IME input are not connected).
- **Security & manual setup:** This test build is not notarized by Apple. `setup.sh` verifies package checksums (`SHA256SUMS`) and signatures first, then removes macOS's downloaded-file quarantine mark **only** from the app, `ffmpeg`, and `ffprobe` without changing any system-wide security settings. If you prefer to review and run each step yourself, see **Advanced: manual setup** in [handbook.md](handbook.md).

## How to Report Bugs & Join Discord

Even playing for 15–30 minutes and telling me what broke (or worked!) helps a lot. You do not need to finish the game.

- **Tester Discord:** https://discord.gg/YPFDF6Q9C9
  - A small server for testers. I am a Japanese student and my English is not very strong, so there is an AI assistant bot in the server that helps answer setup questions and translates messages between us.
- **GitHub Issues:** [Issues](https://github.com/nocturne-hd-mac-lab/nocturne-hd-mac/issues)
  - Prefer not to use Discord? Please open an issue right here on GitHub.

When reporting a bug, it helps a lot to include:
- Your Mac chip/model and macOS version
- The identifiers from `version.json`
- Where the issue happened, what happened, and what you expected
- Relevant error lines from `~/Library/Logs/SMT3HDRestoration/Nocturne HD/Player.log` or `conversion-report.json`

> **Privacy & spoiler reminder before sharing logs:**
> 1. Make a copy of the log/report first and replace your home folder path, account name, and any personal details with `REDACTED`.
> 2. Never upload Windows game files, converted files, or save files.
> 3. Please add a **Spoilers** label for any mid-game, late-game, or ending details or screenshots.

## FAQ

### Was this built with AI?
Yes. This port was built with heavy AI coding assistance (**Codex** and **Claude**) and tested on my Mac. Because I am working solo with heavy AI help, there will likely be bugs and edge cases I have not run into yet — that is why volunteer playtesters are so helpful.

### Where are the game files? Can I play without owning the Windows version?
No game files are included here, and you cannot play without owning the game. You must supply your own purchased Windows version (and your own DLC). The bundled converter builds the Mac-ready files locally on your machine.

### Why is it 30 fps? Will there be a 60 fps mode?
It runs at 30 fps during gameplay just like the original HD Remaster. No 60 fps mode is planned.

### Is the source code public?
Not for now. The setup script (`setup.sh`), converter scripts, checksums (`SHA256SUMS`), and step-by-step manual setup instructions ([handbook.md](handbook.md)) are included so you can see what setup does on your Mac, while the app's source code itself is kept private for now. If you are not comfortable running a closed-source, un-notarized test build, I completely understand — please feel free to wait.

### Does this work on Intel Macs?
No, Apple Silicon Macs only.

### Is this related to Magatama (the PS2-based Nocturne project)?
No, they are completely separate projects. Magatama is based on the original PS2 version, whereas this port converts and runs your own purchased **Windows HD Remaster** (including its HD assets, eight original languages, and owned DLC) natively on Apple Silicon.

### Where are saves kept?
Normal saves are stored in `~/Library/Application Support/org.smt3hd.restoration/original/player/`. Setup does not overwrite existing saves, though since this is an unfinished test build, keeping a backup of important saves is always a good idea.

## Disclaimers

- This is an unofficial, unfinished fan project and is **not** affiliated with, sponsored by, or endorsed by ATLUS or SEGA.
- ©ATLUS ©SEGA. All original game assets, characters, and trademarks belong to their respective owners.
- Bundled `ffmpeg` and `ffprobe` tools include their LGPL licenses, corresponding source, and rebuild instructions inside `media-tools/`.
