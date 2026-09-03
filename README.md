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

## Built with VVE Builder

This mod was created using **[VVE Builder](https://github.com/BelegCufea/VVE_Builder)**, an open-source automation pipeline for building AI voiceover mods for Infinity Engine games with WeiDU and [VoiceBox](https://voicebox.sh/).

VVE Builder handles the whole toolchain end-to-end:

1. **Dialog processing** — extracts and structures game dialog into a working report.
2. **Profile preparation** — pulls reusable voice samples from NPCs already voiced in the base game.
3. **Profile management** — audits samples, builds new voice profiles, assigns them to NPCs.
4. **Audio generation** — batch text-to-speech generation via VoiceBox, with caching to avoid redundant work.
5. **Quality verification** — speech-to-text transcription and scoring against the source dialog to catch bad lines.
6. **Mod compilation** — packages everything into a ready-to-install WeiDU mod.

**Found a broken or off-sounding line in Voices Voices Extravaganza: Baldur's Gate 2?** VVE Builder isn't just for building new mods from scratch — it's the same tool you'd use to fix this one. Point it at the relevant profile or NPC, regenerate that line (or a whole character's voice lines) through VoiceBox, run it back through the quality check, replace the audio files in override directly or even rebuild the whole mod package. No need to wait on anyone else — if you can hear a bad line, you can fix it yourself.

And of course, if you want to build a similar voiceover mod for another Infinity Engine game entirely, **VVE Builder is public and free to use** — head over to the repo, it's the whole toolchain, config and all.

Everything about VVE Builder runs **locally, on your own machine** — the Python pipeline, the WeiDU packaging, and the VoiceBox TTS/STT engine it drives can all be self-hosted. There's no subscription, no per-line pricing, and no cloud API bill to generate or fix voice lines — just your own hardware and time.

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
