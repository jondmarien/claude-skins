---
name: term-skin
description: Switch claude-skins terminal skins (terminal colors, ASCII banner, status line theme, personality, tool sounds). List skins, activate one, reset the terminal, or set a default. USE WHEN the user types '/term-skin' or asks to change the terminal skin or terminal theme. Not for Claude Code UI skins from other mods.
---

# Skin — Claude Code Visual Themes

Switch between visual skins that change terminal colors, ASCII banners, status line theme, Claude's personality/voice, and tool feedback sounds.

## Usage

The user invokes this skill with `/term-skin` followed by an optional argument.

## Which script to run

The engine is installed twice, once per shell. Pick the one that matches the user's machine.

| Platform | How to run a script |
| --- | --- |
| macOS, Linux, WSL, Git Bash | `~/.claude/skins/engine/<name>.sh` |
| Windows PowerShell (no WSL / Git Bash required) | `pwsh` if it is on `PATH`, otherwise `powershell`: `pwsh -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.claude\skins\engine\<name>.ps1"` |

Do not run the `.sh` files on native Windows. Do not run the `.ps1` files from bash on macOS or Linux.

Windows examples below use `pwsh`. If `Get-Command pwsh` fails, substitute `powershell`.

## Behavior

### `/term-skin` (no arguments) — List available skins

macOS / Linux:

```bash
~/.claude/skins/engine/skin-command-hook.sh <<< '{"prompt": "/skin"}'
```

Windows:

```powershell
'{"prompt": "/skin"}' | pwsh -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.claude\skins\engine\skin-command-hook.ps1"
```

Display the script's stdout. The script exits 2 (its hook-mode "handled" code); that is expected, not an error. Still show the list to the user.

### `/term-skin <name>` — Activate a skin

macOS / Linux:

```bash
~/.claude/skins/engine/activate.sh <name>
```

Windows:

```powershell
pwsh -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.claude\skins\engine\activate.ps1" <name>
```

After activation, tell the user the skin is active and mention they can set it as default with `/term-skin default`.

If the personality file exists, read it and adopt that voice for the rest of the session.

- macOS / Linux: `~/.claude/skins/personalities/<name>.md`
- Windows: `$env:USERPROFILE\.claude\skins\personalities\<name>.md`

### `/term-skin reset` — Deactivate and restore

macOS / Linux:

```bash
~/.claude/skins/engine/deactivate.sh
```

Windows:

```powershell
pwsh -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.claude\skins\engine\deactivate.ps1"
```

Tell the user the terminal has been restored.

### `/term-skin default` — Set current as permanent default

Read the current skin, then set `default_skin` in the engine config to that value.

- macOS / Linux: `~/.claude/skins/engine/current` and `~/.claude/skins/engine/config.yaml`
- Windows: `$env:USERPROFILE\.claude\skins\engine\current` and `$env:USERPROFILE\.claude\skins\engine\config.yaml`

`config.yaml` is a single `default_skin:` line unless the user has added more. Replace that line; do not rewrite the rest of the file.

Tell the user which skin was set as default.
