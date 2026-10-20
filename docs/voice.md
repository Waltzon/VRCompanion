# Voice and speech

## How it listens

Commands are recognized on your PC by [Vosk](https://alphacephei.com/vosk/), a
small offline speech model: instant, free, nothing leaves your PC. To stay
accurate, it only listens for command words and your own names (your avatars,
worlds, friends, outfits). Pick names that are real English words: "black cat"
works, "xX_Kitsune_Xx" won't (give it a nickname instead).

The **mic meter** on Home shows what it hears. The bar turns green when it counts
sound as speech; the yellow line is where that starts. By itself it follows your
room's background noise; **drag the line** to set your own (it applies at once
and is remembered), and press **Auto** to let it follow the room again.

## Wake word

Every command starts with it: "**computer**, switch to fox". Change it in the
first-run setup or **Settings → Listening**. Short, unusual words work best. If it
keeps mishearing it as something else ("commander"), it offers to count that as
the wake word too (see [When it mishears you](#when-it-mishears-you)).

- **A blip as soon as it hears it.** You hear the short "listening" sound while
  you're still talking, as soon as "computer" is recognized, not only once the
  sentence is over. (Turn sounds off in Settings if you don't want it.)
- **Wake word, pause, command.** Say "computer", wait for the blip, then "switch
  to fox": the command counts if it comes within 6 seconds
  (`wake_window_seconds` in config.yaml; `0` turns this off).
- **Fewer false starts.** The speech model says how sure it is of each word. A
  "computer" it wasn't at least 50% sure of (noise, someone else, game audio
  that sounded a bit like it) is ignored. If it misses you when you did say it,
  lower `wake_min_confidence` in config.yaml (e.g. `0.3`; `0` accepts any); if
  noise still sets it off, raise it (e.g. `0.7`).
- **Common mishearings count.** "Computers", "computer's" and "compute her"
  count as "computer" too.
- **Light on the CPU.** The speech model only runs while the mic meter says
  someone's talking (green), plus a moment from just before, so it uses next to
  nothing while you're quiet. With push-to-talk it hears everything while the key
  is down.

## Push-to-talk

Instead of the wake word, push a button. **Settings → Listening → Push-to-talk**:

- **Hold a key** while talking, or **press a key** to start and again to stop.
  Any keyboard key or a mouse side button.
- **Hold a VR controller button (SteamVR):** Quest (through SteamVR: Steam Link,
  Link, Air Link or Virtual Desktop), Index, Vive and others. By default: click
  and hold the left thumbstick. To change it, press **Open SteamVR bindings**
  (while SteamVR runs and the app is listening) and bind **Push to talk**.
- **While my VRChat mic is muted:** mute yourself in VRChat, say the command,
  unmute. Uses the button you already have; needs OSC on.

When you let go, the command runs right away, without waiting for a pause.

## Chat mode

"**Computer, activate chat mode**": everything you say is typed into your VRChat
chatbox for everyone to read, with a typing bubble while you talk. "**Computer,
stop chat mode**" ends it. Commands still work with the wake word; a sentence that
only happens to contain it ("my computer is lagging") is sent as chat.
Messages longer than the chatbox's 144 characters are cut at a word.

## Better transcription: Whisper

For chat mode (and for the [AI brain](ai-brain.md) and [captions](in-vr.md#live-captions)),
choose how speech becomes text in **Settings → Listening → Chat mode engine**:

| Engine | Good | Not so good |
| --- | --- | --- |
| **Vosk** (default) | light, instant | no punctuation, mishears more |
| **Whisper** on this PC | accurate, punctuation and capitals | ~1 s per sentence, uses CPU/GPU next to VRChat |
| **Whisper on another computer / cloud** | accurate, and your PC stays free | needs a [speech server](server-setup.md), or an OpenAI/Groq key |

Commands always use Vosk, so they stay instant. With Whisper, `small` is a good
start; go down to `base` or `tiny` if VRChat lags, up to `medium` with GPU room.
Whisper on an NVIDIA GPU also needs NVIDIA's cuBLAS and cuDNN for CUDA 12 (see
[faster-whisper](https://github.com/SYSTRAN/faster-whisper#gpu)); without them it
uses the CPU, which is fine for `small` and below.

Your avatar, world, friend and pet names are given to Whisper as hints, so it
spells them right.

**On another computer:** press one of the buttons under Speech server:
- **My own server**: a spare laptop running Whisper. Free, and your voice stays at
  home. Guide: [Speech server on your own computer](server-setup.md).
- **OpenAI** or **Groq**: paste an API key; a fraction of a cent per minute
  (Groq has a free tier). Your voice goes to that company.
- **VRCompanion server**: an address and key someone gave you
  (hosting one). **Account** shows your plan and usage.

**Test** shows how fast the server answers. If it can't be reached, chat falls
back to Vosk.

## When it mishears you

The Home page has a **Didn't catch that** list:

- **A name it didn't know** ("switch to wolfy"): pick what you meant and **Add**;
  "wolfy" becomes another name for it. Avatars, worlds, outfits and friends.
- **Something like the wake word** ("commander, switch to fox"): **Count
  "commander" as the wake word**.
- **A whole sentence** ("swish two fox"): type what you meant ("switch to fox") and
  **Learn**. From then on, saying it that way, or close to it, runs that command.

Quicker, by voice: say the command again so it works, then "**computer, remember
that**". Since the speech model tends to mishear you the same way every time, this
teaches it your voice and accent. Learned phrases are listed (and removable) in
**Settings → Listening**.

## Speech models

The small English model (40 MB) comes with the app. The bigger
`vosk-model-en-us-0.22-lgraph` (128 MB) is more accurate for chat mode on Vosk:
**Settings → Listening → Speech models → Download**. A model your settings use but
that's missing is downloaded by itself when listening starts.
