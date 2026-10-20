# AI brain

Optional. When something you say doesn't match a command, the app asks Claude
what you meant, together with your lists of avatars, worlds, friends, outfits,
toggles and routines. So "computer, put me in something comfy and open a cozy room
for me and my besties" can switch your avatar *and* create an instance. It can
also answer quick questions, and it gives your pet a voice.

## Turn it on

**Settings → AI brain**, switch on **Understand any wording**, then either:

- **My own Anthropic API key:** paste a key from
  [console.anthropic.com](https://console.anthropic.com) and press **Test**. You
  pay Anthropic per use, usually a fraction of a cent per command.
- **My VRCompanion server account:** no key needed; it uses the server address and
  key from **Settings → Listening** (hosting a server).

## Memory

The brain shares the pet's [memory](pet.md#memory). With each request it gets
what fits: the facts about what you said and who's online (all of them while
there are fewer than 25, otherwise those plus the newest), and the notes the app
keeps (friends who joined you, recent worlds, your last session). So it can say
"Alex is here! You said they like cats".

It has two tools for it: **remember** (when you ask, or clearly share a lasting
preference: "my favourite world is the Black Cat") and **forget** (when you ask).
What it keeps shows in the log ("Remembered: ...") and on the Pet page, where you
can delete it. It never keeps passwords, keys or codes, and with memory off it
gets none of it. Through a VRCompanion server, the memory travels in the request's
lists, so the server needs nothing new.

## Good to know

- Only sentences that start with the wake word (or are said to your pet) and don't
  match a normal command are sent to Anthropic.
- It works best with Whisper as the chat engine (on this PC or a server): the app
  then re-transcribes the command with Whisper, so Claude gets your exact words.
  With Vosk only, it listens without the restricted command vocabulary while the
  brain is on.
- Keys are stored encrypted, never in the settings file. See
  [Settings and security](configuration.md#security).
