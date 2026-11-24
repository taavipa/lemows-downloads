# Le Mows

Lawn-tractor racing with a rural Estonian summer setting. This repository contains the **Windows game downloads, signed game updates and changelog**.

## Download and play

### [Download LeMowsSetup.exe](https://github.com/taavipa/lemows-downloads/releases/download/build-preview-20261124-1/LeMowsSetup.exe)

1. Download and double-click **LeMowsSetup.exe**.
2. Choose your language and install folder, then click **Install Le Mows**.
3. The launcher opens when setup finishes. Use the **Le Mows** desktop or Start menu shortcut on later launches.
4. Wait for the launcher to finish checking for updates, then click **Play**.

The launcher downloads available updates automatically and shows progress. **Play** opens the game's main menu, where you can choose how to play.

Default install folder: `%LOCALAPPDATA%\LeMows`. Re-running setup can upgrade a recognized existing installation while retaining its settings and data. English and Estonian are available in setup and the launcher.

Current player build: **0.1.0-preview.20261124.1**. [Changelog](CHANGELOG.md) · [All player releases](https://github.com/taavipa/lemows-downloads/releases).

<details>
<summary>Download verification</summary>

`LeMowsSetup.exe` SHA-256: `ff2d1e397c39eb851e91698274becde8528466778fe1a071a22de04a484bba04`.

The installer verifies the signed game feed and every downloaded archive/file before use. The downloaded setup is also byte-for-byte identical to the launcher file named in the signed update package.

</details>

## Development state

**Driving and multiplayer preview — Phase 1 playtesting.** Updated 2026-11-24.

| Area | Current state |
|---|---|
| Tractor driving | Starter 1846 and Class S are selectable in Quick Race. Handling review continues. |
| Tracks | Rappavere, Kardiranna and Tolmuste with timing, AI opponents and selectable weather |
| Input and cameras | Keyboard/gamepad, chase, seat and orbit views |
| Multiplayer | Hosting/joining implemented; online feel and performance remain under review |
| Race control | Lap timing, race clock, standings, recovery, pit service and an optional AI teammate with Class S at Rappavere |
| Installation and updates | Graphical Windows setup, native launcher and signed update checks |
| HUD, art and animation | Live menu scenes, race HUD, Tab roster, rider animations and outfit customisation |
| Workshop and walking | Walkable workshop, tool interactions and a fabrication workbench available for testing |
| Village and career progression | Planned |
| Persistent progression | Planned |
| Steam integration | Scheduled for Phase 2 |

The preview is for playtesting. Structured handling feedback, multiplayer smoothness/performance work and broader load testing remain open. Development screenshots and plans may include features that are not in the downloadable game yet.

## Play modes and controls

Choose **Quick race** in the game menu for an offline race. Choose **Race online** to host or join using a network address. Everyone in the same session needs a matching player build.

| Action | Keyboard/mouse | Xbox-style controller |
|---|---|---|
| Throttle / brake | W / S or Up / Down | RT / LT |
| Steering | A / D or Left / Right | Left stick |
| Pull-start engine | Hold E | Hold X |
| Change camera | C | View/Select |
| Look / orbit | Mouse; hold right mouse in orbit views | Right stick |
| Orbit distance | Mouse wheel | — |
| Look back | Hold Left Alt or middle mouse | Hold right-stick press |
| Recovery request | Hold R | Hold D-pad Up |
| Race standings | Tab | — |

Camera cycling includes chase, hood, seat, tractor orbit and driver orbit.

## Updates and recovery

Use the installed **Le Mows** shortcut for normal play. The launcher remembers the language chosen during setup.

If a download is interrupted, select **Try again** in the launcher. Previously verified versions are kept for recovery.

Game data stays in its normal per-user location. When reporting an update problem, include the player version and the error shown by the launcher.
