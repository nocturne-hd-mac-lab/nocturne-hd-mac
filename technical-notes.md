# Technical notes and tested scope

These records describe particular builds and material roots on one Mac. They do not establish gameplay, Finder launch, download-host behavior or installation on another Mac. See [handbook.md](handbook.md) for setup.

## Package checks

`version.json` identifies the app, converter and package maker. `app-files.json` lists archived app files; `inventory.json` lists package files except itself and `SHA256SUMS`. `SHA256SUMS` covers every shipped file except itself. Setup checks these files, extracts the app with its executable permissions and checks the app and media-tool signatures before removing only their quarantine marks. Checksums detect damage; confirm the download source with the organizer.

The setup entry uses Python 3.11 standard-library modules. It creates `.setup-venv` and installs the pinned converter requirements, then skips installation on later runs with the same Python and requirements. It chooses the shader supply inside this app. Materials go under `~/Library/Application Support/org.smt3hd.restoration/converted-materials/`, in a folder identified by the app archive and converter export. An existing configuration supplies a read-only reuse candidate. An existing output with converter checkpoints is resumed. The converter rechecks reused files and rebuilds incompatible routes. Setup updates only `materials.json` after a successful report; every previous configuration is backed up. Player saves are untouched.

`sh setup.sh` was selected because Finder-opened command tools can receive a separate Gatekeeper document assessment ([Apple Developer explanation](https://developer.apple.com/forums/thread/706379)). Explicit shell invocation avoids depending on that Finder path or executable permission on the wrapper. Current macOS can also warn about pasted Terminal commands ([Apple Support](https://support.apple.com/en-us/127377)); typing `sh ` and dragging the included script avoids copying a command from a website. This is a design choice, not a clean-Mac or every-version test. The app and tools are ad-hoc signed, not notarized.

## Requirements and measurements

Minimum macOS version, RAM, performance on other chips and other Macs: **Unverified**. A supported Windows version is required; changed inputs are rejected by pinned hashes. Included scene templates need no Unity Editor or external scene-tool installation.

The Windows files occupy about **13GB**. On the maker's Apple M5 Pro Mac (48GB RAM, macOS 26.6.2), the current converter produced 3,163 files (12.044GB) in **909.5 seconds (15 minutes 9.5 seconds)** of active conversion: 662.8 seconds before interruption plus 246.7 seconds for a successful resume. The resume reused 2,771 files and generated 392. This is not an uninterrupted run; the current version's uninterrupted complete-conversion time has not been measured. A separate capacity-refused resume took 3.2 seconds and is excluded, as are waiting, setup and final checks. Before shader parallelism and resume support, an uninterrupted complete conversion took **883.6 seconds (14 minutes 43.6 seconds)** on the same Mac.

The current run had no unsupported, refused or rejected entries and no manifest/hash mismatches. All 38 clips (75,368 decoded frames) passed structure/timing and quality checks: the lowest clip averages were SSIM 0.993636 and PSNR 49.81dB; no quality retry was needed. With the 22GB floor, it started at 35.621GB free and reached a sampled minimum of 24.768GB across the interrupted run and resume attempts. These are observations on one Mac.

On the same Mac, a separate 591-bundle shader-route measurement took **317.3 seconds serially and 169.8 seconds with three automatically selected workers**. Output was 7.921GB in both runs. Sampled aggregate process-tree RSS peaked at 3.55GB and 8.20GB, respectively; these are observations, not RAM requirements. All 590 nonvideo bundles matched by size/SHA-256, and the video-bearing bundle passed preservation, all-frame timing and quality checks. The interrupted complete run above automatically selected two shader workers; its shader time is not the separate three-worker measurement.

Shader conversion automatically limits parallel workers using CPU count and available memory (at most four). The conservative memory estimate selects serial processing on 8/16GiB Macs; those machines have not been tested. Add `--shader-workers 1` to the conversion command to keep shader processing serial. The free-space floor and output reservation still apply.

After the Windows files, app and converter are installed, allow **18GB free before conversion**. The computed reservation is **13.388GB**, plus the default **4GB free-space floor**. This is a calculation from the pinned output-size profile, not a low-space trial or a guarantee for another Mac. `--keep-free-gb 22` retains the maker's higher floor; allow 36GB for that setting. Other Mac activity affects free space. Starting close to the reservation plus floor can leave too little space for `--resume`: a resume was refused before conversion because its remaining reservation plus the 22GB floor exceeded available space. Free extra space, then retry with the same output and options; keep the partial output intact. Leave extra room for download and unpacking; the total setup peak and a 4GB-floor run remain **Unverified（未確認）**. Run one converter at a time.

The converter uses the included ffmpeg and ffprobe by default. Videos use Apple's VideoToolbox hardware encoder at quality 80, H.264/yuv420p and AAC audio. Encoded bytes may differ between Macs. Every clip must retain its stream structure, dimensions, decoded frame count and timing within the converter's tolerances. SSIM and PSNR filters compare all decoded frames in frame order; whole-clip SSIM must be at least 0.990 and PSNR at least 47dB. If quality is too low, the converter tries quality 90 once within the existing space reservation. An unsuccessful check stops conversion without a completed root manifest. Your Mac's generated hashes are recorded locally to detect later damage.

Developers can select another pair using `--ffmpeg` and `--ffprobe`. Required decoders, encoder and filters are checked; a different version number alone is neither accepted nor rejected. The same media, quality and space checks always apply. Other Mac generations and in-game video appearance/audio remain **Unverified（未確認）**.

The independent command-line tools are FFmpeg 9.0.2 under LGPL 2.1 or later, built without GPL, nonfree or version3 components. `media-tools/NOTICE.txt`, license texts, source tar and SHA-256, `recipe.json`, `BUILD.md` and `rebuild.py` accompany them. Testers do not rebuild these tools. They can be modified, rebuilt or replaced separately from the app.

A Windows PC is not required. On one Mac, the Windows client's command-line downloader was set to platform `windows`, directed to a separate installation folder and used with the owner's account to download application **1413480**, then validate it. All **3,140 files**, including the three inputs pinned by the converter, matched the previously installed Windows version by SHA-256. Owned DLC appeared under `dlc/<number>/`.

Another route is the Windows client inside Wine or CrossOver: install your own Windows version, then select its installation folder as input. The command-line download route above was confirmed on one Mac; the Wine/CrossOver steps and a fresh tester installation remain **Unverified（未確認）**. Ask the organizer for the exact downloader setup if needed. Enter passwords and account verification codes only in the official client's own prompts; do not put them into commands, reports or messages. Never download another person's game files.

The newly generated root passed Japanese and English silent, input-free background starts with the tested app: title-processing markers were reached with no exceptions or material-loading errors. This checks startup, not gameplay, video appearance or sound. Human gameplay with the newly generated root remains **Unverified（未確認）**. Earlier Japanese first-battle observations apply to that earlier app and material root only.

On the first launch of this newly signed app, macOS may ask for access to the folder containing your materials, such as Documents. Choose Allow. If access was denied, open System Settings > Privacy & Security > Files & Folders and enable access for this app if it is listed. Then reopen the app. A background start stalled while reading materials in Documents and passed with a copy outside that folder. The cause of that stall is unconfirmed.

The configuration-file route, without `--original-material-root`, passed silent, input-free background launches in Japanese and English with the previous eight-language app on the organizer's Mac. Language selection, fonts and title processing passed with no exceptions or material-loading errors. Finder launch, this route in the supplied app and setup on another Mac remain **Unverified（未確認）**.

If no material folder is configured, the folder is missing, or the app cannot read it, normal startup reports the problem and shows a Quit button. Check the `root` value in `materials.json` or the path given with `--original-material-root`, and check folder access as described above. Unattended batch runs and dedicated verification builds record the error in `Player.log` and exit without a dialog. The error handling passed focused code tests. The dialog in the packaged app and actual macOS permission prompts remain **Unverified（未確認）**.

The app is ad-hoc signed and is not notarized. Confirm the source and checksums with the organizer first. Setup checks the package and removes quarantine only from this app and the two included media tools.

Do not disable system-wide checks. The exact approval flow for each macOS version and download host is **Unverified（未確認）**.

Save compatibility and migration are **Unverified（未確認）**. Keep existing saves. Do not replace them during setup. `--original-new-session` is for isolated diagnostics, not the normal player setup.

The tested current app passed silent, input-free background starts in Japanese and English through title processing, including language selection and font loading. The previous eight-language app passed those checks in all eight languages. These checked title processing, not the appearance of the title screen. Human gameplay has only been checked in Japanese with an earlier Japanese/English app. Gameplay in the supplied app and long-text layout remain **Unverified（未確認）**.

Choose name characters from the on-screen character table; direct typing and the OS input method (IME) are not connected to the name field. The Traditional Chinese name-entry stoppage has been fixed. Focused code tests matched expected results from the original version in 31 scenarios / 91 steps, including conversion and confirmation; no person has yet checked Traditional Chinese name entry in the app. Static inspection found no equivalent stopping stub in the other seven languages, but does not establish that their name-entry screens work during play. Traditional Chinese and Korean fonts retain a few missing characters also present in the original fonts.

Expected default Unity log path, inferred from this build's company/product settings: `~/Library/Logs/SMT3HDRestoration/Nocturne HD/Player.log`. Final archive default logging is **Unverified（未確認）**.

## Language and startup coverage

The app selects the first supported preferred Mac language, otherwise English. It does not read a persistent language field from materials.json. Use the existing `--original-language` launch argument for an override. The app is unchanged by setup. See [known-issues.md](known-issues.md) for display, controller and gameplay limits.
