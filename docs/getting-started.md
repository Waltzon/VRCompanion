# Getting started

## 1. Install

1. Download **`VRCompanion-Setup-<version>.exe`** from the
   [Releases page](https://github.com/Waltzon/VRCompanion/releases).
2. Run it. It installs just for you (no admin prompt) and adds VRCompanion to the
   Start menu (and the desktop, if you tick the box). Python isn't needed and the
   speech model is included.

Windows SmartScreen may warn that the app is unrecognized, because the installer
isn't code-signed. Click **More info → Run anyway**.

## 2. The first-run setup

The first time it opens, VRCompanion walks you through:

1. **Logging in to VRChat** (with your 2FA code), or **Use without a VRChat
   account**. Your password goes only to VRChat; the app keeps the login,
   encrypted. Without a login, toggles, outfits, the chatbox, the pet, captions
   and the wrist menu all work, and avatars switch over OSC (only ones in your
   VRChat favorites: VRChat allows that). Worlds and instances, friends and
   invites, your status, importing favorites and pictures need the login. You
   can change your mind on the **VRChat account** page.
2. **Your microphone and wake word.** Pick the headset mic, then press **Save and
   start listening** and say "computer, hello". It shows what it heard.
3. **Your favorites.** Tick the VRChat favorite avatars and worlds you want to use
   by voice.
4. **The pet** (optional): a name for it. The pet also needs a one-time setup in
   Unity: see [Putting the pet on your avatar](pet-avatar.md).

You can run it again any time: **Settings → App → Run setup again**.

## 3. Turn on OSC in VRChat

In VRChat: **Action Menu → Options → OSC → Enabled**. That's how the app writes in
your chatbox, reads your avatar's toggles, and talks to the pet. The app finds
VRChat by itself (OSCQuery); if Windows asks whether VRCompanion may use private
networks, allow it.

## 4. Try it

Say: "**computer, surprise me**" (a random avatar), "**computer, open black cat**"
(any world you added), "**computer, who's online**". Or open **What can I say?**
in the app and press **Try** next to anything.

## A tour of the app

| Page | What's there |
|---|---|
| **Home** | Start/stop listening, the mic meter, a box to **type** a command, **Undo** for the last change, "Didn't catch that" (teach it what you meant), live updates, timers, captions, the chatbox |
| **Avatars** | Your avatars as pictures (click to switch) or **Edit** to rename them, give them other names, and import favorites. Toggles on the avatar you're wearing, and your outfits |
| **Worlds** | The same for worlds, plus default instance type and region, and "When I arrive" (switch avatar/outfit automatically) |
| **Friends** | Nicknames for friends with hard-to-say names, and friend groups |
| **Pet** | Its mood, actions, games and settings |
| **Routines** | One phrase for several commands |
| **What can I say?** | Every command, with your names, searchable, with **Try** buttons |
| **Settings** | In tabs (Listening, In VR, VRChat, AI brain, App), with a search box |
| **VRChat account** | Log in and out |

**Closing the window keeps VRCompanion running in the tray** (the microphone icon
near the clock). Its dot shows the state: grey stopped, green listening, blue chat
mode, red error. Right-click it for listening, chat mode, captions, your avatars,
worlds and routines, and **Undo**.

## Updates

The app checks GitHub when it starts and shows a banner with **Update now**. That
downloads the new installer, closes the app, installs and reopens it. Turn the
check off, or check by hand, in **Settings → App → Updates**.

## Uninstall

**Windows Settings → Apps → VRCompanion → Uninstall.** Your settings stay in
`%APPDATA%\VRCompanion`; delete that folder too to remove everything.
