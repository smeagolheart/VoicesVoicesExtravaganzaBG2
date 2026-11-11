# Voices Voices Extravaganza: Baldur's Gate 2

**A full AI-generated NPC voiceover mod for Baldur's Gate II: Enhanced Edition.**

> 🗣️ Every line. Every companion. Every random goblin who really shouldn't be this articulate. Now with a voice.

---

## 🙏 Heavily inspired by the original

This mod would not exist without **[Voices Voices Extravaganza](https://github.com/ColossusChang/VoicesVoicesExtravaganza)** by ColossusChang — the original AI voiceover mod for Baldur's Gate 1. This mod is a spiritual successor and a direct continuation of that project's philosophy, brought to Baldur's Gate II. If you enjoy this mod, go check out the original — it did this first, and it's the reason this exists at all.

Same idea, same spirit, same "let's give everyone a voice" energy — just aimed at the sequel.

---

## What is this?

It adds AI-generated voiceovers to NPC dialog in **Baldur's Gate II: Enhanced Edition** that currently has none — main characters, companions, and the sprawling cast of Athkatla and beyond. It's built as a standard [WeiDU](https://weidu.org/) mod: install it and it patches english voiceover files straight onto the relevant dialog string references.

The mod itself is the *output* of a pipeline — the actual generation, review, and packaging work was done using **VVE Builder** (see below), not by hand-recording or hand-assigning thousands of lines one at a time.

## Picking what to install

When you run the installer, WeiDU asks which parts of the voiceover pack you want. There are three components — here's what each one actually means for what you'll hear in-game:

1. **Add Core/Essential NPC voiceovers (Mandatory!)** — main characters, companions, and every other NPC with a substantial amount of dialogue. This one is not optional!
2. **Add Common NPC voiceovers** — the shopkeepers, bartenders, innkeepers, temple priests, and generic guards/soldiers/townsfolk you'll walk past and talk to again and again over a full playthrough. If you'd rather not hear the same handful of AI-voiced lines repeated every time you sell some loot or ask a guard for directions, leave this one unchecked — those NPCs will just keep using the game's original silent (text-only) dialog.
3. **Add Minor NPC voiceovers** — everyone left over: NPCs with only a handful of generated lines that didn't fit neatly into either group above. Fully optional, and safe to skip if you'd rather keep the install lighter or the voice cast a bit more curated.

**Not sure what to pick?** Check all three for the fullest, most consistently-voiced experience. But don't think of the mandatory Core/Essential component as just "your party and a few big names" — it already covers the large majority of every voiced line in the mod. Common and Minor just add back the repetitive vendor/guard/tavern chatter and the quietest, few-line background characters on top of that, so skipping them still leaves almost everyone else talking.


## Built with VVE Builder

This mod was created using **[VVE Builder](https://github.com/BelegCufea/VVE_Builder)**, an open-source automation pipeline for building AI voiceover mods for Infinity Engine games with WeiDU and [VoiceBox](https://voicebox.sh/).

VVE Builder handles the whole toolchain end-to-end:

1. **Dialog processing** — extracts and structures game dialog into a working report.
2. **Profile preparation** — pulls reusable voice samples from NPCs already voiced in the base game.
3. **Profile management** — audits samples, builds new voice profiles, assigns them to NPCs.
4. **Audio generation** — batch text-to-speech generation via VoiceBox, with caching to avoid redundant work.
5. **Quality verification** — speech-to-text transcription and scoring against the source dialog to catch bad lines.
6. **Mod compilation** — packages everything into a ready-to-install WeiDU mod.

**Found a broken or off-sounding line in Voices Voices Extravaganza: Baldur's Gate 2?** VVE Builder isn't just for building new mods from scratch — it's the same tool you'd use to fix this one. And you don't need to touch anything but that one NPC: the [VVE Builder](https://github.com/BelegCufea/VVE_Builder) repository already ships the BG2:EE dialog report and the extracted voice-sample library used to build this mod, so the dialog-extraction and sample-preparation steps are already done — you don't even need a copy of the game's audio files for this, only the game itself to install the result into.

No need to wait on anyone else — if you can hear a bad line, you can fix it yourself. You don't need to know how to program. The walkthrough below assumes you've never used a command line before and spells out every click.

---

### Regenerating one NPC's voice yourself — full walkthrough

#### What you'll need before you start
- Baldur's Gate II: Enhanced Edition (or EET) already installed somewhere on your PC, and you know which folder it's installed in.
- About 20–30 minutes, most of which is waiting for installers/downloads.
- Windows is assumed below; the same tools exist for Mac/Linux, just installed differently.

#### Step 1 — Install Python
1. Go to [python.org/downloads](https://www.python.org/downloads/) and download the latest version.
2. Run the installer. On the **very first screen**, at the bottom, tick the checkbox **"Add python.exe to PATH"** (this is the single most common thing people forget — if you skip it, nothing later will work from the command line).
3. Click **"Install Now"** and let it finish.

#### Step 2 — Install ffmpeg (used to convert/check the generated audio)
1. Click the Windows **Start** button, type `PowerShell`, and open it.
2. Type the following and press Enter:
   ```powershell
   winget install Gyan.FFmpeg
   ```
3. Let it finish, then **restart your PC** once (this makes sure Windows picks up the new PATH entry). If `winget` isn't available on your system, search "ffmpeg windows install" for a manual install guide — you just need `ffmpeg` reachable from the command line.

#### Step 3 — Install and start VoiceBox (locally, on your own PC)
1. Go to [voicebox.sh](https://voicebox.sh/) and download/install VoiceBox for your system.
2. **VoiceBox must run on your own machine** — it is not a website you log into. After installing it, start it up and leave it running in the background (its window or tray icon staying open/running) the entire time you're generating or checking audio in the next steps. Note the local address it's serving on (it'll normally be `http://localhost:17493`, which is also what VVE Builder expects by default).
3. **Before doing anything else, download the models VoiceBox needs.** In VoiceBox's own interface, click the models icon (the little cube). From there, select the text-to-speech engine you intend to use (e.g. **Qwen 1.7B**, the one this guide assumes) and click its **Download** button, then do the same for **Whisper Base** (used for transcription/quality-checking). Wait for both downloads to finish before moving on — generation and transcription will fail if the corresponding model isn't downloaded yet.

#### Step 4 — Download VVE Builder itself (no Git needed)
1. In your web browser, go to [github.com/BelegCufea/VVE_Builder](https://github.com/BelegCufea/VVE_Builder).
2. Click the green **"<> Code"** button near the top of the file list, then click **"Download ZIP"** in the dropdown.
3. Once it's downloaded (normally into your **Downloads** folder as `VVE_Builder-main.zip`), right-click it and choose **"Extract All..."**.
4. Extract it somewhere simple and short, e.g. `C:\VVEBuilder` (avoid Desktop/OneDrive folders — long or cloud-synced paths can cause odd errors later). After extracting you'll have a folder like `C:\VVEBuilder\VVE_Builder-main` containing files like `config_gui.py`, `generate_gui.py`, etc. That's the folder you'll use for every step below — this guide calls it **the VVE Builder folder**.

#### Step 5 — Open a command prompt inside the VVE Builder folder
You'll reuse this same window for every remaining command-line step.
1. Open the VVE Builder folder in File Explorer (the one containing `config_gui.py`).
2. Click once in the empty area of the address bar at the top of the File Explorer window (where the folder path is shown) to select it all.
3. Type `cmd` and press Enter. A black Command Prompt window opens, already "inside" that folder.

#### Step 6 — Install the required Python packages
In that Command Prompt window, type the following and press Enter:
```bash
pip install -r requirements.txt
```
Wait for it to finish (it downloads several packages, so it can take a few minutes). If it ends with lines mentioning "Successfully installed", you're good.

#### Step 7 — Configure VVE Builder (`config_gui.py`)
In the same Command Prompt window, type:
```bash
python config_gui.py
```
and press Enter. A window titled **"VVE Builder - Configuration"** opens, with two tabs: **"Game data"** and **"API defaults"**.

On the **"Game data"** tab (the one already showing):
1. Next to the **"Game directory"** field, click **"Browse..."**, navigate to your BG2:EE (or EET) install folder — the one that contains an `override` folder and a `lang` folder inside it — select it, then click **"Select Folder"**.
2. The **"Language"** dropdown fills in automatically from that folder; pick your game's language (English installs will show `en_US`).
3. Leave **"Text encoding"** as `utf-8`.
4. The **"Player character"** box (player name/race/gender) only affects a handful of lines that reference your protagonist by name — you can leave the defaults here.

Switch to the **"API defaults"** tab:
1. **"Voicebox URL"** — leave it as shown; it already defaults to your local VoiceBox address. Click the **"Check now"** button next to it: the small circle should turn **green**, meaning VoiceBox was found and is running. If it turns **red**, go back and make sure VoiceBox (Step 3) is actually running, then click "Check now" again.
2. Leave **"Engine"** as `qwen` and **"Model size"** as `1.7B` — these already match VoiceBox's defaults for a local install.
3. **"Transcription language"** auto-fills once you picked your language above.

Once everything looks right, the message above the buttons should read **"Settings are valid and ready to save."** in green. Click **"Save"**. (If **"Save"** stays greyed out, read the red message just above it — it tells you exactly what's still wrong, e.g. an invalid game folder.)

#### Step 8 — Pick a voice for the NPC (`profiles-manage_gui.py`)
Back in the Command Prompt window, type:
```bash
python profiles-manage_gui.py
```
and press Enter. A window titled **"🎯 Voice Profile Manager"** opens, split into a left side and a right side.

1. On the left side, under the **"📋 NPC List"** heading, click into the box that says **"🔍 Type to filter…"** and type the NPC's name (e.g. `Anomen`).
2. The list right below it narrows down as you type. Click on that NPC's row (it shows the name and how many lines of dialog it has).
3. The right side of the window now fills in with that NPC's details. Look for the box titled **"📌 NPC Level Assignment (N lines)"** (N being however many lines that NPC has).
4. Inside that box is a row labeled **"Voice:"** with a dropdown list next to it. Click the dropdown — it lists every voice already available in this project.
   - **To use one of the existing bundled voices:** just click the voice name you want in the list. The moment you pick it, it's saved automatically — there is no separate "Save" button for this, and a small confirmation like **"✅ Updated {NPC name} → {voice name}"** briefly appears at the very bottom of the window (the status bar).
   - **To replace the sample audio behind a voice with your own recording instead of using it as-is:** every NPC already ships with a voice profile and its own samples, so the button next to the dropdown will normally show **"✏️"** (edit). If no profile is found for that NPC, you'll see an **"➕"** (add) icon instead. Either way, clicking it opens a **"🎵 Voice Profile Editor"** window for that profile, listing its existing sample(s) in the **"Samples:"** list. First remove any old sample(s) you don't want anymore: click a sample in the list, then click **"🗑️ Delete Sample"** and confirm the two prompts that follow (repeat for each existing sample). Then add your own: click **"➕ Add Sample (File)"** to pick a `.wav`/`.mp3`/etc. audio file from your PC (or **"🌐 Add Sample (URL)"** to import one from a web link) — it gets transcribed automatically, and the result appears in the **"Transcribed Text (from Voicebox):"** box; if that looks correct (it should), just click **"📋 Use This Text"** to copy it into the **"Text:"** box above it, rather than typing it out yourself. (If that box stays empty for some reason, click **"🧪 Check Similarity"** to trigger the transcription manually.) You can add more than one sample the same way. When you're done, click **"Close"**.
5. If a voice is already selected in the **"🚻 Gender Overrides"** or **"📋 System Name Overrides"** boxes further down, it will override the voice you just assigned above for the lines it covers. Feel free to edit those too if you want, but the safe bet is to set them to the empty/unassigned profile so your NPC-level choice is the one that actually applies. You can also ignore the **"🧪 Check All Samples"** button on the top-left — that's an unrelated bulk quality-audit tool for the whole voice library.
6. Once you've assigned the voice you want, just close this window (the ✕ in the corner) — everything is already saved.

#### Step 9 — Regenerate that NPC's lines (`generate_gui.py`)
Back in the Command Prompt window, type:
```bash
python generate_gui.py
```
and press Enter. A window titled **"🎙️ TTS Voice Generator"** opens.

1. Click the **"🛠️ Settings…"** button near the top. A **"Configuration"** dialog opens with three tabs.
2. If you replaced that NPC's voice sample(s) in Step 8, click the **"🗣️ Voices"** tab first and tick the checkbox **"Force delete + rebuild before generating"** (under "Profile renewal:"). Without this, VVE Builder may assume the voice profile it already sent to VoiceBox is still up to date and skip re-uploading your new sample(s).
3. Click the **"⚙️ Generation"** tab.
4. Click the button **"🎯 Manage NPC Targets…"**. This opens a separate **"NPC Targets"** window.
5. At the very top of that window is a checkbox labeled **"Skip already generated"**. If it's ticked, **untick it** — unticking it tells VVE Builder to redo lines even if it thinks it already made them before, which is exactly what you want when fixing an existing NPC's voice. This takes effect immediately, no extra button needed.
6. Click the **"Select None"** button (just under the table) to make sure nothing else is accidentally selected.
7. Click into the **"🔍 Search:"** box and type the NPC's name to narrow the table down to just their row.
8. Tick the checkbox in the leftmost column (**"☑"**) of that one row.
9. Click **"💾 Save & Close"** at the bottom of the window.
10. Back in the **"Configuration"** dialog, click **"💾 Save Config"** to close it.
11. On the main window, click **"▶️ Start Generation"**. You'll see two progress bars (one for the current line, one for the whole batch) and a scrolling log underneath. Wait until the top status reads **"Finished N job(s)."** — that means it's done. This can take anywhere from seconds to several minutes depending on how many lines that NPC has.

#### Step 10 — (Optional) Double-check the new audio (`check_gui.py`)
In the Command Prompt window, type:
```bash
python check_gui.py
```
and press Enter. A window titled **"Transcription Check"** opens.
1. Click **"Start Check"** (top-left). This checks *every* NPC's generated audio, not just the one you just redid — that's fine, it just takes a little longer.
2. When it finishes, the top status reads **"Done! N NPCs, M samples."**. In the **"NPC Results"** table, find your NPC's row and look at the **"Worst %"** / **"Avg %"** columns and the **"Status"** column: green **"Excellent"**/yellow **"Good"** are fine, orange **"Poor"**/red **"Bad"** mean a line probably needs another look (go back to Step 9 and regenerate again, or try a different voice sample).
3. Click that NPC's row to see each individual line underneath, with a **"🔊 Play Sample"** button so you can actually listen to what was generated versus the expected text.

#### Step 11 — Install the new lines into the game (no mod rebuild needed)
1. Open File Explorer and go into the VVE Builder folder, then into its `output` folder, then into the folder named after your NPC (e.g. `output\Anomen\`). You'll see a handful of `.wav` files in there — these are the freshly generated lines, already named exactly the way the game expects.
2. Select all of them (Ctrl+A), and copy them (Ctrl+C).
3. Open a second File Explorer window and navigate to your game's install folder, then into its `override` folder (the same folder you pointed "Game directory" at in Step 7, plus `\override`).
4. Paste (Ctrl+V). Windows will ask if you want to replace the existing files — confirm **"Replace the files in the destination"**. That's it: those NPC lines in-game will now use the new audio the next time they play. You don't need to reinstall or recompile the mod at all.

---

And of course, if you want to build a similar voiceover mod for another Infinity Engine game entirely, **VVE Builder is public and free to use** — head over to the repo, it's the whole toolchain, config and all.

Everything about VVE Builder runs **locally, on your own machine** — the Python pipeline, the WeiDU packaging, and the VoiceBox TTS/STT engine it drives all run self-hosted, on your own hardware. There's no subscription, no per-line pricing, and no cloud API bill to generate or fix voice lines — just your own machine and time.

## Compatibility

- **Game:** Baldur's Gate II: Enhanced Edition, and the **Enhanced Edition Trilogy (EET)**
- **Language:** English
- **Installer:** WeiDU (`setup-VVEBG2.exe` on Windows, or the platform WeiDU binary)
- Install like any other WeiDU mod — run the setup executable from the mod's parent folder and follow the prompts.

As with any large voiceover mod, expect the occasional rough edge: a line here or there may be missing, oddly paced, or not quite match the character as well as we'd like. If it bothers you, use VVE Builder to tweak them to your liking.

## Disclaimer

This mod is a fan-made, non-commercial project created purely out of love for Baldur's Gate II and for the fun/challenge of building an automated voiceover pipeline. It uses AI text-to-speech to generate voice lines for existing in-game dialog — it does not include, modify, or redistribute any of the original game's text, code, or assets beyond what WeiDU patches at install time against a copy of the game you already own.

All rights to Baldur's Gate II and its content remain with their respective owners (Bioware / Beamdog / Interplay / Wizards of the Coast). This project claims no ownership over the base game, its story, or its characters.

**Please don't sue me.** 🙂 If you're a rights holder with a concern, please just reach out — I'll listen.

## License

The original *Voices Voices Extravaganza* is released under **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 (CC BY-NC-SA 4.0)**, and in keeping with the "heavily influenced by" spirit of this project, **VVEBG2 uses the same license**:

- ✅ You're free to use, remix, and redistribute this mod and its voice files.
- ✅ Feel free to swap out voices you don't like, retrain profiles, or fork it into your own variant using VVE Builder.
- ⚠️ **Attribution required** — credit this project and the original VVE.
- ⚠️ **Non-commercial only** — no selling this, no paywalling it, no monetized bundles.
- ⚠️ **Share-alike** — if you distribute a modified version, it must carry the same license.

Full license text: [Creative Commons BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

(Note: this applies to the *mod* — the voiceover audio, mapping data, and packaging. The **VVE Builder** toolchain itself is separately licensed under the MIT License in its own repository, since it's software rather than game content.)

## Credits

- **ColossusChang** — creator of the original [Voices Voices Extravaganza](https://github.com/ColossusChang/VoicesVoicesExtravaganza), whose work directly inspired this mod.
- **[VVE Builder](https://github.com/BelegCufea/VVE_Builder)** — the automation pipeline used to generate, verify, and package this mod.
- **[VoiceBox](https://voicebox.sh/)** — the text-to-speech and speech-to-text engine used for generation and quality verification.
- **WeiDU** — for making Infinity Engine modding possible in the first place.
- Everyone in the BG modding community who's kept these 25+ year old games alive.

## Issues & Feedback

Found a bad line, a missing voice, or a bug in the installer? Open an issue on the mod's GitHub repository. If you want to help improve or regenerate specific voices yourself, grab [VVE Builder](https://github.com/BelegCufea/VVE_Builder) and dig in — it's built to be forked and customized.

## Proposed: safe-attach installer (`VVEBG2/setup-VVEBG2.tp2`, v1.02)

The shipped installer writes every mapped sound unconditionally, so it replaces any sound already on a line
(official audio, the vve mods, another voiceover pack) and copies every WAV into `override` whether or not
the line was patched. This variant:

- attaches a voiceover **only if the line currently has no sound** (empty, or a `C0BLANK`-style placeholder);
- lines that already have a sound are left alone and listed in the WeiDU debug log, with a count at the end;
- copies a WAV to `override` **only for lines it actually patched**, so no orphan files are left behind;
- keeps the line's current text (`ACTION_GET_STRREF` -> `STRING_SET_EVALUATE`), so text fixes from other mods survive.

It reuses the existing `mapping_*.txt` files and `WAV` folder unchanged. `TRA/english/setup.tra` gains one message (`@12`).
It does not check that the line's text still matches the text the audio was generated from; that needs the generator
to ship the source text per line (as VVE Builder's text-matching installer does).
