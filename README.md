# Le Mows downloads

Public Windows game and Ubuntu/Debian dedicated-server downloads for the current driving/racing prototype. **No GitHub account, GitHub CLI, Unity or .NET installation is required.** Install once; the launcher and server supervisor download future published updates automatically. Source code remains private.

This is a development preview. Garage fabrication, walking, village gameplay and Steam integration are still in development. Download the named ZIP assets; GitHub's **Source code** archives contain only this download page and channel files.

## Downloads and checksums

| Download | SHA-256 |
|---|---|
| [Windows launcher/bootstrap ZIP](https://github.com/taavipa/lemows-downloads/releases/download/updater-bootstrap-v1/lemows-update-win-x64.zip) | `83d06a2378cb9cc78783de12982a3dda0c507e96a1c19c2be48a2113323f2ac9` |
| [Linux server/bootstrap ZIP](https://github.com/taavipa/lemows-downloads/releases/download/updater-bootstrap-v1/lemows-update-linux-x64.zip) | `f0700f1965049d9032f28826b164a54fe41af83bee13bf0d7a049a62e1df754c` |

[Published game/server builds](https://github.com/taavipa/lemows-downloads/releases) use matching signed release identities. The bootstrap installs the updater first; the first managed launch downloads the game/server. Later releases keep the same bootstrap pins. Preview feed: [signed preview channel](https://raw.githubusercontent.com/taavipa/lemows-downloads/main/channels/preview.json).

## Windows: install and play

Run this block in PowerShell as your normal Windows user:

```powershell
& {
    $ErrorActionPreference = 'Stop'
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
    $lemowsTemporary = Join-Path $env:TEMP ('lemows-bootstrap-' + [guid]::NewGuid().ToString('N'))
    New-Item -ItemType Directory -Path $lemowsTemporary | Out-Null
    $lemowsArchive = Join-Path $lemowsTemporary 'lemows-update-win-x64.zip'
    $lemowsPin = '83d06a2378cb9cc78783de12982a3dda0c507e96a1c19c2be48a2113323f2ac9'
    Invoke-WebRequest -UseBasicParsing -Uri 'https://github.com/taavipa/lemows-downloads/releases/download/updater-bootstrap-v1/lemows-update-win-x64.zip' -OutFile $lemowsArchive
    if ((Get-FileHash -LiteralPath $lemowsArchive -Algorithm SHA256).Hash -ne $lemowsPin) { throw 'Bootstrap checksum mismatch' }
    Expand-Archive -LiteralPath $lemowsArchive -DestinationPath (Join-Path $lemowsTemporary 'package')
    powershell -NoProfile -ExecutionPolicy Bypass -File (Join-Path $lemowsTemporary 'package/install-windows.ps1') -Archive $lemowsArchive -Sha256 $lemowsPin
    if ($LASTEXITCODE -ne 0) { throw 'Le Mows installation failed' }
}
```

Open the **Le Mows** desktop shortcut. Choose English or Estonian, then **Host a race**, **Join server** (enter its reachable IP), or **Solo driving**, and **Update and play**. Default install: `%LOCALAPPDATA%\LeMows`. A custom location can be passed to the installer with `-InstallRoot 'D:\Games\LeMows'`; `-NoShortcut` omits the shortcut. Use the shortcut/stable wrapper for future launches.

## Ubuntu/Debian x86-64: install the server

Run in Bash as the normal Linux user who will own the server. Only dependency installation needs sudo:

```bash
(
    set -eu
    sudo apt-get update
    sudo apt-get install -y curl unzip ca-certificates libstdc++6 libgcc-s1 zlib1g libgomp1
    lemows_temporary=$(mktemp -d)
    lemows_archive="$lemows_temporary/lemows-update-linux-x64.zip"
    lemows_pin=f0700f1965049d9032f28826b164a54fe41af83bee13bf0d7a049a62e1df754c
    curl -q --fail --show-error --location --proto '=https' --proto-redir '=https' \
        'https://github.com/taavipa/lemows-downloads/releases/download/updater-bootstrap-v1/lemows-update-linux-x64.zip' \
        --output "$lemows_archive"
    printf '%s  %s\n' "$lemows_pin" "$lemows_archive" | sha256sum --check -
    unzip -q "$lemows_archive" -d "$lemows_temporary/package"
    bash "$lemows_temporary/package/install-linux.sh" --archive "$lemows_archive" --sha256 "$lemows_pin" --enable-service
    systemctl --user status lemows-server.service
)
```

Default install: `~/.local/share/lemows`. Add `--root "$HOME/servers/lemows"` to the installer for a custom location and use that location in subsequent commands. To survive logout, run `sudo loginctl enable-linger "$USER"` once. The installer uses one fixed `lemows-server.service` per Linux user; multiple instances need separate users or custom units/ports.

Without systemd, omit `--enable-service`, then run:

```bash
~/.local/share/lemows/run-lemows.sh --json supervise --drain-updates
```

The supervisor checks every five minutes. It downloads/verifies while the server runs, closes admission for an update, waits for all players to leave, then stops cleanly and starts the new version. A continuously occupied server can defer updates or graceful shutdown indefinitely.

## Live terminal dashboard

In a second terminal/SSH session, as the server owner:

```bash
~/.local/share/lemows/run-lemows.sh dashboard
```

The display refreshes every second. It shows CPU (100% = one core), RAM, process uptime, connected players, connection/race capacities, race phase and clock, configured tick rate, measured mean tick rate since startup, release/source identity, draining and updater progress. Unsupported or stale metrics show unavailable. **Ctrl+C closes the dashboard; the server keeps running.** The monitor works without network access or credentials and can attach while the supervisor owns the installation.

```bash
~/.local/share/lemows/run-lemows.sh dashboard --interval 2
~/.local/share/lemows/run-lemows.sh --json dashboard --once
~/.local/share/lemows/run-lemows.sh --json dashboard --interval 5 >> server-stats.jsonl
```

`--interval` accepts 1–60 whole seconds. Redirected output and `--once` use JSON lines without terminal escape codes. Set `"language": "et"` in the local config for the Estonian terminal display. Measured tick rate is an average since startup, not a prediction performance benchmark or instantaneous frame time.

## Connect, operate and configure

Find the Linux PC's address with `hostname -I`. Enter its reachable IP in the Windows launcher's **Join server** mode. The preview uses **UDP 7770**; allow it through the server firewall and configure router forwarding if needed outside your LAN. `127.0.0.1` refers to the same PC. Automatic Steam relay is not configured. Client/server release, source and protocol must match; wait for any drain/update to finish and relaunch if you see `update_required`.

```bash
systemctl --user status lemows-server.service
systemctl --user start lemows-server.service
systemctl --user stop lemows-server.service
systemctl --user restart lemows-server.service
journalctl --user -u lemows-server.service -f
~/.local/share/lemows/run-lemows.sh --json status
~/.local/share/lemows/run-lemows.sh --json check
~/.local/share/lemows/run-lemows.sh --json update
```

Stop/restart waits safely for players. Ctrl+C on the foreground **supervisor** requests the same graceful stop. Choose either service or foreground supervision per installation. After a crash/start-limit failure, correct the cause, then `systemctl --user reset-failed lemows-server.service` and start again.

Local settings: `INSTALL_ROOT/config.json`. Stop the server before editing its `arguments`. For a different port, use `["-batchmode", "-nographics", "-mr-net", "server", "-mr-link", "off", "-mr-port", "7771"]`; put `["-mr-port", "7771"]` in the Windows config and allow that UDP port. Preserve the product/platform/trusted keys and signed `feed_url`. Keep credentials out of config. Managed release/control flags are added automatically.

Windows command-line maintenance (PowerShell 5.1 needs an explicit argument array):

```powershell
$lemowsLauncher = Join-Path $env:LOCALAPPDATA 'LeMows/run-lemows.ps1'
& $lemowsLauncher -Cli -UpdaterArguments @('--json', 'status')
& $lemowsLauncher -Cli -UpdaterArguments @('--json', 'check')
& $lemowsLauncher -Cli -UpdaterArguments @('--json', 'update')
```

## Existing private preview installations

An existing default private install with working access receives one signed transition automatically on its next update check. The verified helper changes only the feed endpoint, preserving local arguments, language, timing, keys and data. After transition, downloads no longer use GitHub credentials.

To move without signing in again, gracefully stop the old server/close the game, use the pinned public install block above, and add **`--public-feed`** to the Linux installer or **`-PublicFeed`** to the Windows installer. Reuse the same install root. The explicit switch migrates only the known old default private endpoint with matching platform and trust keys. Custom feeds are preserved; it does not replace arbitrary configuration. Start through the existing wrapper afterward. Repeated installation otherwise preserves configuration and bootstrap files.

## Driving controls

| Action | Keyboard/mouse | Xbox-style controller |
|---|---|---|
| Throttle / brake | W / S, or Up / Down | RT / LT |
| Steering | A / D, or Left / Right | Left stick |
| Pull-start engine | Hold E | Hold X |
| Cycle chase/seat/tractor orbit/driver orbit | C | View/Select |
| Look / orbit | Mouse; hold right mouse in orbit views | Right stick |
| Orbit distance / look back | Wheel / hold Left Alt or middle mouse | — / hold right-stick press |
| Recovery request | Hold R | Hold D-pad Up |
| Manual gear up/down, if enabled | Left Shift / Left Ctrl | RB / LB |

Automatic shifting is the default. Online recovery is controlled by the server.

## Recovery and data

The updater verifies signed manifests and archive/file hashes, stages immutable versions, switches atomically and checks real game startup. Failed startup restores a retained confirmed healthy version when available. A cached healthy game can start if an online check fails; a first installation needs internet.

With the game/server stopped, restore a retained previous version using `run-lemows.sh --json rollback`, the launcher's **Restore previous version**, or the Windows CLI wrapper with `@('--json', 'rollback')`. Normal launches check the feed again; rollback is not a permanent update hold. Old versions are retained. Updates preserve external data but do not reverse future save-format migrations.

Back up the install root's `config.json`, updater state and external game saves. Logs/control records are in `data/`, native logs in `data/logs/`, Windows launcher events in `logs/launcher.jsonl`. Unity saves use its normal per-user persistent data directory outside versioned executable folders. Keep user files out of `game/versions/` and `updater/versions/`. The prototype does not yet resume abandoned races after an empty-server restart.

If download verification fails, retry the matching pinned archive; do not bypass checks. If status says waiting for players, have players leave. For `launch_recovery_required`, inspect the ownership records and confirm the recorded process has ended before operator recovery; do not delete a live process's guards. If a user service stops after logout, enable lingering. If the dashboard lacks fresh status, inspect the newest native log and the supervisor journal. Share error codes/release identities, never credentials or per-launch control tokens.

Standalone stable releases reuse this signed updater engine with a separate stable channel. Steam clients will use Steam-managed distribution once the partner/AppID/depot setup is ready.
