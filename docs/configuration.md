# Settings and security

## Where things are

The installed app keeps everything in **`%APPDATA%\VRCompanion`** (paste that into
Explorer's address bar, or **Settings → App → Settings file → Open folder**):

| File | What |
|---|---|
| `config.yaml` | your settings, avatars, worlds, outfits, routines... |
| `secrets.dat` | your VRChat login and API keys, encrypted |
| `pet.json` | the pet's mood, headpats and records |
| `memory.json` | what the pet remembers ([details](pet.md#memory)) |
| `avatars.json` | each avatar's toggles, as VRChat last reported them |
| `thumbnails/` | cached avatar and world pictures |
| `models/` | speech models you downloaded |
| `vrcompanion.log` | the log, for troubleshooting (also in **Settings → App → Diagnostics and backup → Show log**) |
| `bug-report.txt` | the last bug report you made, with secrets taken out |

The app edits `config.yaml` for you; you can also edit it by hand while the app is
closed. [`config.example.yaml`](config.example.yaml) explains every option.

## Backups and moving to a new PC

**Settings → App → Diagnostics and backup → Back up…** saves one file,
`VRCompanion-backup-<date>.zip` (in Documents unless you pick another place).
It has `config.yaml` (settings, avatars, worlds, outfits, routines, learned
phrases...), `pet.json`, `memory.json` and `avatars.json`; tick the option to
add the cached pictures too.

On the new PC, install VRCompanion, then **Settings → App → Diagnostics and backup → Restore…** and
pick the zip. Your current files are kept as
`config.before-import.yaml` and `pet.before-import.json`, in case you want them
back. Only VRCompanion backups are accepted.

**Your VRChat login and API keys are never in a backup**: `secrets.dat` only
works on the PC and Windows account that made it. On the new PC, log in to VRChat
again and paste your API keys (AI brain, speech server) in Settings.

## Forget me

**Settings → App → Diagnostics and backup → Forget me…** logs you out of
VRChat and deletes everything the app keeps about you: `secrets.dat` (login,
API keys), `memory.json`, `pet.json`, `avatars.json`, `thumbnails/`, the log,
crash notes, `bug-report.txt` and the `*.before-import.*` copies. From
`config.yaml` it takes out friends' nicknames, friend groups and learned
phrases; the rest of your settings stay. Tick **Also reset all settings** to
start over completely (the setup shows again). Speech models stay.

## A few options only in config.yaml

| Key | Default | |
|---|---|---|
| `restrict_vocabulary` | `true` | only listen for command words and your names (much more accurate). Turn off if a name is never recognized |
| `match_threshold` | `0.6` | how close a heard name must be to count (0–1) |
| `end_silence_ms` | `700` | how long a pause ends a sentence (also in Settings) |
| `wake_min_confidence` | `0.3` | how sure the speech model must be that it heard the wake word (0–1; `0` accepts any). Lower it if it misses you, raise it if noise sets it off |
| `wake_window_seconds` | `6` | after the wake word alone ("computer" … "switch to fox"), how long the command may take to follow; `0` = wake word and command in one sentence |
| `osc_host` / `osc_port` | `127.0.0.1` / `9000` | where VRChat listens for OSC |
| `osc_listen_port` | `9001` | where VRChat sends OSC; `0` turns it off |
| `oscquery` | `true` | read every toggle of the avatar you're wearing from VRChat (OSCQuery), and if another OSC app already has `osc_listen_port`, listen on a free port and tell VRChat about it |

## Security

- **Your VRChat password is never stored.** After you log in, VRChat gives the app
  a session cookie, like a browser gets.
- **The session and API keys are encrypted** in `secrets.dat` with Windows DPAPI:
  only your Windows account, on this PC, can read them. They're never written to
  `config.yaml`, and the settings page never shows a saved key again. Keys typed
  into `config.yaml` by hand are moved into `secrets.dat` when the app starts.
- `secrets.dat` doesn't work on another PC or Windows account: there, log in
  again and re-enter your keys. [Backups](#backups-and-moving-to-a-new-pc) leave
  it out for that reason, and bug reports and the log viewer blank out keys and
  tokens.
- **Log out** (VRChat account page) to end the session on VRChat's side too.
- **Updates** only come from this project's GitHub releases, and the installer is
  checked against the SHA-256 checksum GitHub publishes before it runs.
- **Speech servers:** use `https`, or plain `http` only on your home network or
  over Tailscale. The Test button warns otherwise.

### What leaves your PC

- **VRChat:** API requests (switching avatars, instances, invites, status, your
  favorites and friends). It's the same unofficial API the website uses, one or
  two requests per command, with a `User-Agent` that names the app, as VRChat
  asks.
- **A speech server** (only if you set one): the audio of chat sentences,
  captions and AI brain commands.
- **Anthropic** (only with the AI brain): the text of commands it handles, plus
  your avatar, world, friend and routine names.

## Notes

- The VRChat API is **unofficial**. Use it only for your own account and don't
  spam it; it's rate-limited.
- VRCompanion is a fan-made tool, not made by, endorsed by or affiliated with
  VRChat Inc.
