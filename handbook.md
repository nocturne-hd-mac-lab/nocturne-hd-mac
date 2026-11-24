# Quick start

Unofficial, unfinished test version for Apple Silicon Macs. Use your own purchased Windows version and your own DLC files. Not affiliated with or endorsed by ATLUS or SEGA. ©ATLUS ©SEGA

Thank you for helping test this build. You do not need any Terminal or Python experience to get started.

**Before you begin:**
- Have your **Windows installation folder** ready (your own copy, containing `GameAssembly.dll`).
- Install **Python 3.11** from [python.org](https://www.python.org/downloads/macos/) (**macOS universal2 installer**).
- Keep at least **36 GB free** before conversion.

**Setup steps:**
1. Double-click the downloaded package ZIP to extract it. Keep the extracted folder together.
2. Open **Terminal** (**Applications → Utilities**).
3. Type `sh ` (with the space), drag **setup.sh** from the extracted folder into Terminal, and press **Return**. This is the only setup command.
   *(Dragging a file into Terminal only pastes its path; it does not move or copy the file.)*
4. When asked, drag your **Windows installation folder** into Terminal and press **Return**.
5. Setup checks the package files, extracts the app, verifies signatures, and installs any needed Python packages (internet required on first setup).
6. Wait for **“Setup finished.”** Leave Terminal open while setup converts the files. If interrupted, run the same command again. No virtual environment activation is needed.
7. Close Terminal, then double-click **OriginalMaterialFree.app** in the extracted folder to play. If macOS asks for folder access, choose **Allow**. If loading seems stuck, check behind the game window for a permission prompt.

By default, setup saves your **converted materials** (the Mac-ready game files the converter makes from your own copy) in your Library’s Application Support folder, away from Documents and Downloads. Setup keeps previous materials and backs up any existing materials setting. Please keep the converted materials after closing Terminal.

## If something goes wrong

- **Setup stops with a message:** Read the message printed after `Setup stopped:`. In many cases, you can fix the issue, leave the package folder and any partial output where they are, and run the same `sh ` + `setup.sh` command again to continue. If you are stuck, please keep the conversion report (if one was created) and ask the organizer for help.
- **The game seems stuck on the loading spinner:** A macOS permission prompt (asking for folder or external-drive access) may be hidden behind the game window. Look behind the game window and choose **Allow**. If access was denied earlier, open **System Settings → Privacy & Security → Files & Folders**, enable access for the app if it is listed, and reopen it.
- **macOS says the app cannot be opened:** Make sure `setup.sh` finished completely first. After checking the package checksums (file fingerprints) and signatures, setup removes macOS’s downloaded-file quarantine mark only from `OriginalMaterialFree.app`, `ffmpeg`, and `ffprobe`, leaving system-wide security settings unchanged. If you are setting up manually, follow the signature-check and quarantine steps in **Advanced: manual setup**. Please do not disable system-wide checks. If macOS still will not open the app, note the exact message and ask the organizer for help.
- **Not enough free space:** Keep at least **36 GB free** before conversion (leave room for **13.4 GB** of output plus **22 GB free**). If your Mac’s main drive is tight on space, you can save the converted materials on an external drive using `--output` (see **Setup options** below).
- **Python 3.11 not found:** Install **Python 3.11** using the **macOS universal2 installer** from [python.org](https://www.python.org/downloads/macos/). Close and reopen **Terminal**, then run the setup command again. If you are on an Apple Silicon Mac and see a Rosetta message instead, turn off **“Open using Rosetta”** for Terminal and reopen Terminal.

## Obtain the Windows files on a Mac

You do not need a Windows PC. Here are two ways to get your own Windows files on a Mac:

- **Command-line downloader:** Set the Windows client's command-line downloader to platform `windows`, direct it to a separate installation folder, and download application **1413480** using your own account. Please ask the organizer for the exact downloader setup if needed.
- **Wine or CrossOver:** Run the Windows client inside Wine or CrossOver, install your own Windows version, and then select that installation folder as your input.

Owned DLC is stored under `dlc/<number>/`.

**Please remember:**
- Enter passwords and verification codes **only** in the official client's prompts, never in commands, reports, or messages.
- Use only your own purchased Windows version and DLC. Never download another person's game files.

## Setup options

You can add extra options to the same setup command if needed. Put double quotes around any path that contains spaces.

- `--output`: Choose a new conversion-output folder, including on an external drive. Leave room for **13.4 GB** of output plus **22 GB free**.
- `--reuse`: Point to previous converted materials so setup can read from them without changing them. Without this option, setup looks for a previous folder in `materials.json`. The converter reuses files only when its checks pass.
- `--config`: Choose an alternate configuration file for isolated checks.

Example:

```sh
sh setup.sh "/absolute/path/to/windows-install" \
  --output "/absolute/path/to/converted-game-materials" \
  --reuse "/absolute/path/to/previous-converted-game-materials"
```

Keep these rules in mind when using options:
- Run the command from the extracted package folder, or use the dragged `setup.sh` path as in Quick start.
- Keep the output and any alternate configuration file outside the Windows files, the package folder, previous materials, and saves.
- Run only one conversion at a time.
- Repeat the same command to resume an interrupted conversion, and keep `.conversion-cache/` (setup’s saved progress folder) with its output.

## Configure materials and open the app

- **Configuration file:** After conversion succeeds, setup writes `~/Library/Application Support/org.smt3hd.restoration/materials.json` by default so the app knows where your converted materials are.
- **Usability check:** Setup reports whether the materials are usable. Any `unsupported`, `refused`, or `rejected` files prevent setup from finishing. Missing optional DLC is reported separately. If setup stops, please keep the report and ask the organizer for help.
- **Folder permissions:** First-launch permission prompts can hide behind the game window, including prompts for external drives. If access was denied, check **System Settings → Privacy & Security → Files & Folders** for the app, allow access if the app is listed, and reopen it.
- **App folder:** Keep the app’s contents together. Do not copy converted materials into it.
- **Saves:** Normal saves are in `~/Library/Application Support/org.smt3hd.restoration/original/player/`. Please keep your existing saves; setup does not replace them.

## Choose a language

The app uses your Mac’s first preferred language if it is supported, and otherwise uses English.

Available languages: Japanese, English, French, Italian, German, Spanish, Traditional Chinese, and Korean.

To play in another language for one launch, pass `--original-language` with `japanese`, `english`, `french`, `italian`, `german`, `spanish`, `tchinese`, or `koreana`:

```sh
open -n "/absolute/path/to/OriginalMaterialFree.app" --args --original-language english
```

**Entering names:** Select names from the on-screen character table. Direct typing and the OS input method are not connected to the name field.

## Advanced: manual setup

Use this section only if you prefer to run each setup step yourself instead of using `setup.sh`.

1. From the extracted package folder, check the package checksums (`SHA256SUMS`), unpack `app.zip`, create a Python virtual environment (`.venv`, a self-contained folder for the converter’s packages), install its packages, and verify signatures:

```sh
shasum -a 256 -c SHA256SUMS
unzip app.zip
cd converter/work/restoration/tools/material-converter
python3.11 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
codesign --verify --strict media-tools/macos-arm64/ffmpeg
codesign --verify --strict media-tools/macos-arm64/ffprobe
codesign --verify --deep --strict ../../../../../OriginalMaterialFree.app
```

No virtual environment activation is needed: the commands below call `.venv/bin/python` directly.

2. After all checks above pass, remove the quarantine attribute (`com.apple.quarantine`, macOS’s downloaded-file mark) **only** from the included app and media tools:

```sh
xattr -dr com.apple.quarantine ../../../../../OriginalMaterialFree.app
xattr -d com.apple.quarantine media-tools/macos-arm64/ffmpeg media-tools/macos-arm64/ffprobe
```

An absent attribute needs no action. Do not disable system-wide checks.

3. Choose a new output folder for the converted game materials, separate from the input folder and the app, and run the converter. For example:

```sh
.venv/bin/python -B convert.py "/absolute/path/to/windows-install" \
  "$HOME/Library/Application Support/org.smt3hd.restoration/converted-game-materials" \
  --kind all --keep-free-gb 22 \
  --shader-supply "../../../../../OriginalMaterialFree.app/Contents/Resources/Data/StreamingAssets/MacShaderSupply"
```

4. Check the top-level `counts` in the output folder’s `conversion-report.json`:
   - `converted`: generated files
   - `retained_in_app`: supplied tables
   - `not_owned`: absent optional DLC
   - Do not use outputs with `unsupported`, `refused`, or `rejected` files, a failed command, or any `conversion-failure.json`.
   - If an error occurs, keep the partial output and repeat the same command with `--resume` after resolving the error.
   - For an update, choose a new output folder and add `--reuse "/absolute/path/to/previous-converted-game-materials"`. The old folder is read only. An interrupted update can use both `--resume` and `--reuse`.

5. Back up any existing `materials.json`, then write it with your absolute conversion-output path:

```json
{"root":"/absolute/path/to/converted-game-materials"}
```

Place it at `~/Library/Application Support/org.smt3hd.restoration/materials.json`, then double-click **OriginalMaterialFree.app**. The converter does not write this configuration for you.

## Logs and bug reports

Please read [known-issues.md](known-issues.md) first. Measurements, inspection details, and tested limits are in [technical-notes.md](technical-notes.md).

When reporting a problem, please include:
- Your Mac chip and macOS version
- The identifiers from `version.json`
- Where the problem occurred, what happened, and what you expected
- A **spoilers** label for any late-game or ending details or screenshots

**Where to find logs:**
- **Conversion report:** in the conversion output folder (`conversion-report.json`).
- **App log:** normally `~/Library/Logs/SMT3HDRestoration/Nocturne HD/Player.log`.

**Before sharing logs or reports:**
Logs and reports may include local paths. Before sharing one:
1. Copy the file first.
2. Replace your home path, account, and full name with `REDACTED`.
3. Check the copy for any remaining names, email addresses, personal folders, private URLs, and credentials.

Send only relevant error lines. If you are unsure, describe the symptom without an attachment. Never send Windows files, converted materials, or saves.
