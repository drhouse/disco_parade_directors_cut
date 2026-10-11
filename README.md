# ZERO PARADES DIRECTOR'S CUT Voice Override

An unofficial update of [ZERO PARADES Voice Override](https://github.com/zeroparade/disco_parade) by **zeroparade**, fixed to run on the **Director's Cut** game update (Unity 6000.3.15f1).

All credit for the mod and the voice packs goes to the original author. This repo only contains the compatibility fixes needed for the Director's Cut build.

## What was broken

After the Director's Cut update, the original mod (v0.3.21) crashed the game on startup / the loading screen:

1. **The BepInEx build the original installer pins (6.0.0-be.755) can't start on Unity 6000.3** — Il2CppInterop can't locate its runtime hooks (`Class::Init signatures have been exhausted`) and the game aborts.
2. **Unity 6000.3 crashes when hosting the mod's injected `MonoBehaviour`**, even with all of its callbacks disabled.

## What changed (v0.3.22)

- The audio runner is now a plain class driven every frame by a Harmony hook on `UnityEngine.EventSystems.EventSystem.Update`, instead of an injected `MonoBehaviour`.
- The on-screen toasts (hotkey/state popups) were rebuilt with built-in uGUI components (`Canvas`/`Image`/`Text`) instead of `OnGUI`.
- Built against BepInEx **6.0.0-be.788**.
- Debug aid: names listed one per line in `BepInEx\voice-override-skip.txt` are skipped (patch method names such as `PlayVO`, or `Runner`). Leave the file absent for normal use.

Voice-pack format, folders, config and hotkeys are unchanged from the original.

## Requirements

- ZERO PARADES Director's Cut (Windows x64).
- **A BepInEx 6 IL2CPP build that works on the Director's Cut.** Stock be.755 does not. Tested with BepInEx **6.0.0-be.788** plus a patched `Il2CppInterop.Runtime.dll` that resolves its runtime hooks on Unity 6000.3.
- The voice packs from the original mod's installer.

## Install

1. Set up a working BepInEx 6 (see Requirements) and launch the game once so it generates its interop files.
2. Install the voice packs with the original mod's installer (`Install.bat`), choosing male / female / extras / narrator.
   - **Don't let it replace BepInEx or the plugin DLL.** Remove any loose `ZeroParadesVoiceOverride.dll` next to `Install.bat` before running it, or the old broken DLL gets copied into `plugins`.
3. Copy `BepInEx\plugins\ZeroParadesVoiceOverride.dll` from this repo into your game's `BepInEx\plugins\`, replacing the old one.
4. Launch the game. `BepInEx\voice-override-logs\voice-override.log` should show `PLUGIN_READY v0.3.22`.

## Controls

| Key | Action |
| --- | --- |
| F1 | Cycle presets: original VO, original + missing VO, male redub, female redub |
| F2 | Cycle redub profile: off / male / female |
| F3 | Toggle extra-character missing VO |
| F4 | Toggle narrator-only missing VO |
| F9 | Download/install voice-pack updates in game |
| F10 | Show/write the latest dialogue report |
| F11 | Toggle voice-pack update toasts |
| F12 | Toggle debug toasts |

Rebind any key in `BepInEx\config\spore.zeroparades.voiceoverride.cfg`. For example, if another mod or trainer uses F3, change `ToggleExtraVoicesKey = F3` to `Home`.

## Building

```
dotnet build src/ZeroParadesVoiceOverride.csproj -c Release -p:GameDir="C:\path\to\ZERO PARADES"
```

`GameDir` must contain a BepInEx install whose `core` and `interop` folders have been generated (launch the game with BepInEx once). Output: `src/bin/Release/net6.0/ZeroParadesVoiceOverride.dll`.

## Troubleshooting

- **The game closes on the loading screen:** check `BepInEx\LogOutput.log` for `Class::Init signatures have been exhausted`. If it's there and the game crashes, your BepInEx / Il2CppInterop build doesn't support this Unity version.
- **Voices still sound original:** make sure the matching pack is installed (`BepInEx\voice-overrides` for male), and check `voice-override.log` for `NO_REDUB_OVERRIDE`.
- **`PATCH_METHOD_MISSING` in the log:** a future game update renamed a dialogue method the mod hooks.
