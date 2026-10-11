# ZERO PARADES DIRECTOR'S CUT Voice Override

A compatibility update so **[Disco Parade (ZERO PARADES Voice Override)](https://www.nexusmods.com/zeroparadesfordeadspies/mods/7)** by **zeroparade** works on the **Director's Cut** update of the game.

The original mod crashes the game on the Director's Cut. This update fixes that. Voices, hotkeys and settings are unchanged. All credit for the mod and its voice packs goes to zeroparade.

> **To the original author:** this was made because the mod stopped working after the game update. If for any reason you want it taken down, just ask and it will be removed.

## Install

1. Install the original mod from [Nexus](https://www.nexusmods.com/zeroparadesfordeadspies/mods/7) as usual (run `Install.bat`, pick your voices).
2. Download **[ZeroParades-DirectorsCut-VoiceOverride-v0.3.22.zip](https://github.com/drhouse/disco_parade_directors_cut/releases/latest)**.
3. Extract it into the game folder (the one with `ZeroParades.exe`) and **overwrite all files**.
4. Play. The first launch takes a little longer.

If you run the original `Install.bat` again later (e.g. to add voice packs), repeat steps 2–3 afterwards.

## Controls

Same as the original: **F1** presets · **F2** male/female/off · **F3** extra voices · **F4** narrator voices · **F9** update voices · **F10** dialogue report · **F11** update popups · **F12** debug popups.

Keys can be changed in `BepInEx\config\spore.zeroparades.voiceoverride.cfg`.

## Using a trainer or other mods

Works alongside the OperantDossier trainer (Director's Cut update), which uses the same BepInEx setup. That trainer opens with **F3**, so change `ToggleExtraVoicesKey = F3` to another key (e.g. `Home`) in the config file above.

---

<details>
<summary>Technical details</summary>

**Why it broke:** the Director's Cut moved the game to Unity 6000.3.15f1.
- The BepInEx build the original installer uses (6.0.0-be.755) can't start on it.
- Unity 6000.3 crashes when hosting the mod's injected `MonoBehaviour`.

**What changed in v0.3.22:**
- The audio runner is a plain class ticked by a Harmony hook on `EventSystem.Update` instead of an injected `MonoBehaviour`.
- Toast popups were rebuilt with built-in uGUI components instead of `OnGUI`.
- Names listed in `BepInEx\voice-override-skip.txt` are skipped (debug aid; normally absent).

**What's in the zip:**
- Official [BepInEx 6.0.0-be.788](https://builds.bepinex.dev/projects/bepinex_be/788/BepInEx-Unity.IL2CPP-win-x64-6.0.0-be.788%2B5b766a3.zip) (LGPL-2.1).
- `BepInEx\core\Il2CppInterop.Runtime.dll`: [Il2CppInterop](https://github.com/BepInEx/Il2CppInterop) v1.5.3 (LGPL-3.0) with two hook-resolution fixes for this Unity build. These are the OperantDossier trainer's published Director's Cut compatibility patch, reproduced from the official be.788 file. SHA-256 `750E3C8E35CA237B1E53C32FADB686F4ADC2E52BAFBDB9CF33DA7A295D81BAAB`.
- `BepInEx\plugins\ZeroParadesVoiceOverride.dll`: built from `src/` in this repo.
- License texts in `licenses\`.

**Build:** `dotnet build src/ZeroParadesVoiceOverride.csproj -c Release -p:GameDir="C:\path\to\game"` (needs BepInEx installed there and launched once).

**Troubleshooting:** logs are in `BepInEx\LogOutput.log` and `BepInEx\voice-override-logs\voice-override.log`. If the game still closes on the loading screen, check that step 3 overwrote `BepInEx\core`.

</details>
