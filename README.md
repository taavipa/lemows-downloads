# Le Mows

Lawn-tractor racing with a rural Estonian summer setting. This repository contains the **Windows game downloads, signed game updates and changelog**.

## Download and play

### [Download LeMowsSetup.exe](https://github.com/taavipa/lemows-downloads/releases/download/build-preview-20261115-6/LeMowsSetup.exe)

1. Download and double-click **LeMowsSetup.exe**.
2. Choose your language and install folder, then click **Install Le Mows**.
3. Click **Open Le Mows**, or use the **Le Mows** desktop/Start menu shortcut.
4. Choose your play mode and click **Play**.

The launcher installs the game, checks for updates and shows download progress. When a new build is available, click **Update**, then **Play**. No command prompt, PowerShell, ZIP extraction, GitHub account, Unity or .NET installation is needed.

Default install folder: `%LOCALAPPDATA%\LeMows`. Re-running setup can upgrade a recognized existing installation while retaining its settings and data. English and Estonian are available in setup and the launcher.

Current player build: **0.1.0-preview.20261115.6**. [Changelog](CHANGELOG.md) · [All player releases](https://github.com/taavipa/lemows-downloads/releases).

<details>
<summary>Download verification</summary>

`LeMowsSetup.exe` SHA-256: `05f044220f826e83b5b7c161a0e859e3ec9db656c4b273684d41f8dbcefeba2c`.

The installer verifies the signed game feed and every downloaded archive/file before use. The downloaded setup is also byte-for-byte identical to the launcher file named in the signed update package.

</details>

## Development state

**Early driving and multiplayer prototype — Phase 1 playtesting.** Updated 2026-11-15.

| Area | Current state |
|---|---|
| Tractor driving | Playable prototype with custom vehicle physics; handling review continues |
| Track | Driveable prototype race track |
| Input and cameras | Keyboard/gamepad, chase, seat and orbit views |
| Multiplayer | Hosting/joining implemented; online feel and performance remain under review |
| Race control | Lap timing, race clock, standings and recovery prototype |
| Installation and updates | Graphical Windows setup, native launcher and signed update checks |
| HUD, art and animation | Prototype presentation; further work planned |
| Garage/fabrication, walking, village and career | Planned gameplay; not yet playable |
| Persistent progression and Steam integration | Planned |

The preview is for playtesting. Structured handling feedback, multiplayer smoothness/performance work and broader load testing remain open. Development screenshots and plans may include features that are not in the downloadable game yet.

## Play modes and controls

Choose **Solo driving**, **Host a race**, or **Join game**. To join, enter the address provided by your host. Everyone in the same session needs a matching player build; update and reopen the game if it asks for a newer version.

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

Automatic shifting is the default. Camera cycling includes chase, seat, tractor orbit and driver orbit.

## Updates and recovery

Use the installed **Le Mows** shortcut for normal play. The launcher remembers your language, play mode and join address. Installing or updating does not start a game until you choose **Play**.

If a download is interrupted, reopen the launcher and retry. Previously verified versions are kept for recovery. **Restore previous version** is available when a healthy previous game version exists; normal launches still check for newer updates.

Keep user files outside version folders. Game data stays in its normal per-user data location; updates preserve external data but do not reverse future save-format migrations. Use **Open logs** when reporting a problem, and include the player version and error code.
