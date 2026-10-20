# In VR

You can't see the app window with a headset on, so VRCompanion tells you in VR.

## Sounds

On by default: a chime when a command worked, a low buzz when it didn't, a blip
when push-to-talk starts listening. **Settings → In VR**.

## Chatbox confirmations

"Switching to fox" in your VRChat chatbox. Everyone nearby sees it; turn it off
in **Settings → In VR** if you'd rather rely on the sounds. Answers about your
friends are never sent to the chatbox.

## Overlay

*Experimental, off by default.* A small card floats in front of you for a few
seconds, showing what it heard and what happened. **Settings → In VR → SteamVR
overlay**. SteamVR must be running.

## Wrist menu

*Experimental.* A small panel on your controller, for when you can't (or don't
want to) talk. At the top it shows whether the app is listening, the avatar
you're wearing and how your pet feels. Below: chat mode and captions switches,
your avatars and worlds as pictures, your outfits (for the avatar you're
wearing), routines, pet actions, and **Undo**.

Point your other controller at the menu, like a laser pointer: a dot shows
where it points, and the button under it lights up with a tiny buzz. Pull the
trigger to press it, or hold the dot still on it for a second (its ring fills
up). It doesn't depend on your avatar's hands, and it never takes over your
controllers, so you can keep moving while it's open.

If the dot isn't where you aim, say "calibrate aim" (or press **Calibrate aim**
in **Settings → In VR**), point at the target in the middle and pull the
trigger. You can also move and tilt the menu on your wrist there.

Long lists (avatars, worlds, outfits, routines) come in pages: **<** and **>**
at the bottom, with "page 1 of 3" on **Back** and in the top corner.

Turn it on in **Settings → In VR → Wrist menu**:
- **When I say "open menu"**: "computer, open menu" shows it; it hides again after
  30 seconds without use, or with **Close** or "close menu".
- **Always on my wrist**: always there while the app is listening. Instead of
  **Close** it has **Lock** and **Hide**. Locked, nothing reacts until you hold
  the dot on **Unlock** for two seconds (or say "unlock menu"). Hidden, it leaves
  a small round button on your wrist: point at it to bring the menu back (or
  say "open menu").

Pick the hand it sits on there too (left by default). In `config.yaml` you can
also set:
- `wrist_menu_size`: `small` (14 cm wide), `medium` (17 cm, default) or `large`
  (20 cm). Bigger is easier to hit and read.
- `wrist_menu_haptics`: `true` (default) for the buzzes, `false` for none. Some
  SteamVR setups ignore them.

It needs SteamVR, and starts with listening.

## Battery warnings

On by default. When a controller, tracker or headset battery gets low, you hear
the notice chime and get told: "Your left controller is at 15%!". It warns once
at 20%, and once more at 10% ("Time to charge it"). Nothing while it's charging;
after charging (or once it's 5% above the warning level again) it warns again
next time it gets low.

- **Where you see it:** the overlay card (if it's on), the app's log and a
  notification from the tray icon. If the pet is on, it says it in its bubble;
  otherwise it goes to your chatbox, but only with chatbox confirmations on.
- **"computer, battery"** (or "how's my battery", "battery levels") tells you all
  of them: "Left controller 80%, right controller 45% (charging), waist tracker 60%".
- **Names:** left and right controller by hand; trackers by the role you gave
  them in SteamVR (**Manage Trackers**), like "waist tracker" or "left foot
  tracker", otherwise "tracker 1", "tracker 2".
- It checks every 45 seconds while the app is listening, and needs SteamVR
  (it doesn't start SteamVR, it just waits for it). Only devices that report a
  battery to SteamVR are listed; base stations aren't.
- In `config.yaml`: `battery_warnings: false` turns the warnings off (the levels
  still show), `battery_warning_percent: 30` warns at 30% and 15%.

## Live captions

*Experimental.* Writes down what the people around you say, low in your view in
VR and on the Home page. Handy in loud worlds, if you're hard of hearing, or when
someone speaks another language: "**translate for me**" turns it into English.

- Turn it on in **Settings → In VR → Live captions**, or say "**captions on**".
- It listens to the game's sound (what your PC plays), not your microphone, so
  your own voice isn't captioned. If VRChat plays through a headset that isn't
  your default output, pick it under **Listen to**.
- It uses Whisper: on this PC (wants a decent GPU while you play; start with
  `small`), or your [speech server](server-setup.md) if that's your speech engine.
  Translation on a server needs one that offers `/v1/audio/translations`
  (Speaches and Groq do; OpenAI only with `whisper-1`).
- Music and noise can come out as nonsense; it skips the usual "thank you" that
  Whisper hears in silence.
