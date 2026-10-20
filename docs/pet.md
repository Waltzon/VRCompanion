# The pet

A little pet that floats next to you, that you and other players can grab, pet and
talk to. VRCompanion is its brain; its body is on your avatar. To put it there,
see **[Putting the pet on your avatar](pet-avatar.md)** (a one-time Unity setup).
Then turn it on, and name it, on the app's **Pet** page.

## What it does

- **Commands:** "Mochi, sit", "jump", "spin", "dance", "do a trick", "go to
  sleep", "come here", "how is Mochi".
- **Your assistant too:** its name works like the wake word for every command
  ("Mochi, switch to fox", "Mochi, who's online?"), and it answers in its own
  name.
- **Its own speech bubble** above its head, instead of your chatbox.
- **Headpats and grabs:** it gets happier when petted and enjoys being carried
  around. Its mood slowly settles back over hours; mood and headpats are saved.
- **It notices things:** it greets friends who come online or join you, dozes off
  when you've been quiet, and wakes up when you move or talk.
- **Talking:** with the [AI brain](ai-brain.md) on, say anything to it ("Mochi,
  do you like pizza?") and it answers in character. Describe its personality on
  the Pet page.
- **Memory:** "Mochi, remember that Alex likes cats", and it does (see below).

## Memory

The pet keeps a small long-term memory in `memory.json`, next to your settings:

- **Facts you tell it:** "Mochi, remember that Alex likes cats", "Mochi, remember
  my favourite world is the Black Cat". With the [AI brain](ai-brain.md) on, it
  also keeps a clear preference you mention in passing ("Mochi, I love rainy
  worlds"), and uses what it knows in its answers ("Alex is here! You said they
  like cats").
- **Notes it keeps by itself,** small and bounded: the last 20 friends who joined
  you (and when), your last 8 worlds, and when you last played.

| Say | Does |
| --- | --- |
| **Mochi, what do you remember** / what do you know about Alex | lists it, only for you |
| **Mochi, forget about Alex** | forgets every fact about Alex |
| **Mochi, forget that** | forgets the newest fact |
| **Mochi, forget everything** | clears it all (say "undo" if that was a mistake) |

Limits: 100 facts of one short sentence each (the oldest go first), no
duplicates, and anything that looks like a password, key, code or card number is
never kept.

**Privacy:** turn **Memory** off on the Pet page (`pet.memory: false`) and nothing
new is kept and the AI brain sees none of it. What's saved stays until you clear
it on the Pet page or say "forget everything". Without the AI brain, the memory
never leaves your PC; with it, the facts that fit go to Claude (or your
VRCompanion server) with each request.

## It copies you

With OSC on, VRChat tells the app what you're doing, and the pet joins in: it
naps while you're AFK, sits when you sit (chairs too), jumps when you jump, and
cheers when you hold a thumbs-up. Turn it off with **Copies you** on the Pet page.

## Which avatars have it

When VRChat is running, the app sees which of your avatars have the pet. On one
without it, "Mochi, sit" says so instead of doing nothing, and the pet talks in
your chatbox. It also reads the avatar's letters per update, and notices if the
pet was set up with an older Unity tool (the Pet page says so: press **Set up
pet** in Unity again).

## Putting it away

Your Action Menu has a **Pet** toggle: off and it's gone (for everyone), on and
it's back. While it's away it stays quiet.

## Shy

On the **Pet** page, enter the name of an avatar toggle under **Shy when this
toggle turns on** (e.g. your SPS toggle; the names are listed on the Avatars page
under Toggles, and you can give several, separated by commas). When it turns on,
the pet gets flustered ("I-I'll give you some privacy!"), turns away and shrinks
out of sight. When it turns off, it peeks back out. Its lines stay
family-friendly, since everyone can read the bubble.

It needs the pet tool from VRCompanion 0.20 or later on your avatar (press
**Set up pet** again after updating the Unity package).

## Games

### Pat challenge

"Mochi, pat challenge": everyone has 10 seconds to pat it as much as they can. It
announces the count and keeps a record.

### Hide and seek

"**Mochi, hide**" leaves a copy of your pet exactly where it is, for a friend to
find. They say "**computer, play hot and cold**" and hear "warmer", "colder",
"hot!" until "You found it!". "Am I close?" and "I give up" work too; "Mochi,
come here" brings yours back.

Their avatar needs the pet tool too, and their VRChat Safety settings must allow
your avatar's interactions. It knows how close, not which direction.

### Meet-ups

When your pet touches another player's pet, both say hello and get happier;
keep them together a few seconds and they snuggle. It counts how many pets it has
met. (Their avatar needs the pet tool too.)

## Settings

On the **Pet** page: its name and other names, the speech bubble and its
letters per update (must match the Unity component), reactions, napping, meet-ups,
personality, memory, and the avatar parameter names (only change these if you changed
them in Unity).
