# Putting the pet on your avatar

VRCompanion is the pet's brain: it hears your commands, keeps track of its
mood, and reacts to headpats and grabs. The pet's **body** lives on your avatar.
A Unity tool builds it for you in one click, with a default pet included: a
little cat that floats next to you, which you and others can grab.

You need your avatar's Unity project as you upload it now: **Unity 2022.3**
(VRChat's version, e.g. 2022.3.22f1) with the **VRChat Avatars SDK** (from the
VRChat Creator Companion).

## Quick start

1. Download **VRCompanionPet-x.y.z.unitypackage** from the
   [latest release](https://github.com/Waltzon/VRCompanion/releases/latest).
   In your avatar's Unity project, use **Assets → Import Package → Custom
   Package...** and pick it (or drag it into the Project window), then **Import**.
   It goes to `Assets/VoiceAssistPet`.

   **If nothing happens** (no import window, no error): download
   **VRCompanionPet-x.y.z.zip** from the same release instead, close Unity, and
   unzip it into your project's `Assets` folder, so you get
   `Assets/VoiceAssistPet` (with `Editor` and `Runtime` inside). Open the
   project again. Either way works for updating later, too. Also check that the
   project isn't in **Safe Mode** (it says so in the window title), which
   blocks importing packages.
2. Select your avatar in the Hierarchy, then **Tools → VRCompanion → Add Pet to
   Selected Avatar**. A floating cat appears next to your chest. Press Play to
   see it bob and sway.
3. To change it, select **VRCompanion Pet (settings)** under your avatar:
   color, size, where it floats, how floaty it is, or your own model. Press
   **Set up pet** again after a change.
4. Upload the avatar and make sure OSC is enabled in VRChat (**Action Menu →
   Options → OSC → Enabled**). VRCompanion asks VRChat for the new parameters
   itself. Pet doesn't react? Use **Reset Config** in the same menu once.
5. In the VRCompanion app, open the **Pet** page, turn the pet on, give it a
   name, save, and start listening. Try "Mochi, sit".

**Remove pet** (on the same component) takes everything out again.

## Turning it on and off

Set up pet adds a **Pet** toggle to your Action Menu (the radial menu). Turn it
off and the pet and its bubble disappear for everyone; turn it on
and it's back. It remembers the choice between worlds. While it's put away, the
app keeps it quiet: no greetings, no naps, and "Mochi, sit" tells you it's put
away. Costs 1 synced bit; untick **Menu Toggle** if you don't want it.

If your Action Menu is already full (8 items), the Console says so; add a
Toggle for `PetOn` to one of your submenus yourself.

## Meet-ups

Every pet carries a small "I'm a pet" contact. When your pet touches another
player's pet (bring them together, or grab one and boop the other), both pets
say hello, do a happy bounce and get happier: each owner's VRCompanion hears it
(`PetMeet`). Keep them together for a few seconds and they snuggle. The Pet
page counts how many pets yours has met, with a cheer at 1, 5, 10, 25, ...

- The other player needs the pet on their avatar (made with this tool, version
  0.14 or later) and VRCompanion running for *their* pet to react; yours reacts
  either way.
- As with hot and cold, VRChat's **Avatar Interactions** setting must allow
  the other avatar.
- VRChat doesn't say whose pet it was, so it can't keep a list of friends by name.
- Turn it off with **Meet-ups** on the component, or "Meets other pets" on the
  app's Pet page.

## Hide and seek (hot and cold)

"Mochi, hide": the pet shrinks away from you and a copy of it stays exactly
where it was, pinned in the world. Walk away and let a friend look for it.

The friend's avatar needs this tool too (with **Hide and seek** ticked; it's on
by default): it puts a proximity contact on their head that feels hidden pets
within **Hot/cold range** (12 m by default). They say "computer, play hot and
cold", and their VRCompanion tells them "warmer", "colder", "hot!" as they move,
and "You found it!" when they're right next to it. "Mochi, come here" brings
your pet back.

- The seeker needs VRChat's **Avatar Interactions** to allow your avatar
  (Safety settings; friends are allowed by default), or their avatar can't
  feel yours.
- It only knows *how close*, not the direction: walking around is the game.
- Your own hidden pet doesn't count for you.

## The speech bubble

The pet talks in its own bubble above its head, not in your chatbox (VRChat
gives each player one chatbox, so the bubble is part of your avatar). It turns
to face whoever looks at it and holds 4 lines of 18 characters.

How the text gets to everyone: VRChat only syncs avatars through parameters,
so VRCompanion sends the text **a few letters at a time** (`PetTextPointer`
says which group, `PetText0..` hold the letters). The avatar keeps each group
once it has it, only changed groups are sent, and the bubble stays hidden until
the whole message is there. It disappears after a few seconds.

You choose how many letters go per update with **Letters Per Update** on the
component. The app reads it from the avatar while VRChat is running (the setting
on the app's Pet page is only used when it can't):

| Letters per update | Synced bits | Speed | A short line | A full bubble |
|---|---|---|---|---|
| 2 | 24 | ~10 letters/s | ~2-3 s | ~7 s |
| 4 (default) | 40 | ~20 letters/s | ~1-2 s | ~4 s |
| 8 | 72 | ~40 letters/s | under 1 s | ~2 s |

(Estimates; short lines like "Mochi loves the headpats!" are the common case.)
The letters are synced as whole numbers, which VRChat doesn't smooth for other
players, so updates can go quickly without letters arriving garbled.

- Not enough room? Use fewer letters per update, or untick **Speech Bubble** on
  the component, press Set up pet, and turn the bubble off on the app's Pet
  page: the pet then writes "Mochi: ..." in your chatbox.
- The layers match your avatar's **Write Defaults** setting (mixing on and off
  breaks toggles and face tracking), except "Letters": a Direct blend tree,
  which needs it on, and only touches the bubble. The letters are kept in
  animator-only parameters (`PetTextChar0..`, free) by a parameter driver.
- The font has English letters, æøå, common accents and a few symbols (♥ ★ ♪).
  Emoji are left out.
- It's drawn by a small custom shader (`VoiceAssist/PetBubble`), so it shows on
  PC; it's hidden for anyone who blocks custom shaders, and on Quest.

## Why it's an editor tool, not a script on the avatar

VRChat doesn't run custom scripts on avatars: when you upload, it removes every
component that isn't on its allowed list. So the tool runs in Unity instead:
**Set up pet** builds the pet out of parts VRChat allows, and the VRCompanion
Pet component itself (a settings holder) is left out of the upload. VRCFury and
Modular Avatar work the same way.

## What it builds

```
<your chest bone>
  VoiceAssistPet          PhysBone: makes it float and sway, and grabbable
    PetRoot               where the pet floats (the "Offset" setting)
      Model               the pet; animated (bob, sit, jump, spin, ...)
        Eyes              the default pet's eyes (mood, sleep)
      PatContact          contact: notices hands touching its head
      Bubble              the speech bubble
      Hidden              a copy of the pet: off until "hide", then pinned in the world
```

- **Floating and grabbing:** one PhysBone from your chest bone out to the pet,
  with no gravity, so it hangs in the air and sways when you move. Grabbing is
  on: anyone can take it and pull it around (up to twice its distance). With
  **Posable** on, it stays where you let go of it until it's grabbed again.
- **Animations:** two layers added to your FX controller, "VRCompanion Pet
  Actions", "Mood", "Hide", "Toggle", "Text", "Letters" and "Bubble". Every clip writes the same properties,
  so they work with Write Defaults on or off (the tool matches what your avatar
  uses).
- **Parameters:** `PetState` (one Int: what the pet does *and* its mood) is
  synced so everyone sees the same thing: 8 of your 256 bits, plus 24, 40
  (default) or 72 for the speech bubble. So 48 bits with the default settings,
  32 with 2 letters per update, 8 without the bubble, plus 1 for the **Pet**
  toggle. `PetPatted`,
  `PetGrab_IsGrabbed`, `PetHotCold` and `PetMeet` are local
  only (free); they're listed so VRChat reports them to VRCompanion.
- **What it costs:** the Inspector shows it live as you change settings: synced
  bits, contacts (up to 5: headpats, two for hide and seek, two for
  meet-ups), constraints (the hidden pet) and the one PhysBone, plus where
  your whole avatar lands (e.g. "14 contacts, Good on PC"). Untick features
  you don't use to save contacts.
- **Backups:** the first time, your FX controller and expression parameters are
  copied to "... (before Voice Assist Pet)" (the tool's old name). Generated files go to
  `Assets/VoiceAssistPet/Generated/<avatar name>/`.
- **The default pet** is built from Unity's basic shapes, combined into two
  meshes with two materials, so it costs little performance.

## Settings

| Setting | What it does |
|---|---|
| Custom Model | Your own pet model instead of the cat. It's placed in `Model` and gets the same animations (moving the whole model). |
| Color | The default cat's body color. |
| Size | 1 is about head-sized. |
| Attach To | Which bone it floats around (Chest by default; Head works too). |
| Offset | Where it floats, in meters from that bone: x = right, y = up, z = forward. |
| Grabbable | Let people grab it. |
| Posable | It stays where it's let go. |
| Follow Strength | Low: lazy and swaying. High: sticks close to you. |
| Menu Toggle | A **Pet** on/off toggle in your Action Menu (on by default, 1 bit). |
| Speech Bubble / colors | The pet's own speech bubble (on by default), its color and the text color. |
| Letters Per Update | 2, 4 or 8: speed against parameter bits (see the speech bubble). |
| Parameter names | Only change these if they clash; then change them in the app's Pet settings too. |

## The parameters

| Name | Type | Synced | What it does |
|---|---|---|---|
| `PetState` | Int | yes | action × 16 + mood: which animation plays (table below) and the mood, 0 = sad (sleepy eyes) to 15 = overjoyed (wide eyes) |
| `PetOn` | Bool | yes (saved) | The Action Menu's **Pet** toggle: off hides the pet |
| `PetPatted` | Bool | no | True while a hand touches its head |
| `PetGrab_IsGrabbed` | Bool | no | True while someone holds it (set by the PhysBone) |
| `PetMeet` | Bool | no | True while another player's pet touches yours |
| `PetHotCold` | Float | no | How close another player's hidden pet is: 0 = out of range, 1 = right there |
| `PetTextPointer` | Int | yes | Speech bubble: 0 = hidden, 1 = shown, 2+ = which letters are being sent |
| `PetText0`.. | Int | yes | Speech bubble: the letters (2, 4 or 8 of them) |
| `PetTool1` | Bool | no | Not used in game: its number tells the app which version of the Unity tool set the pet up |

Actions (`PetState` ÷ 16):

| Value | Action | You say | Ends by itself |
|---|---|---|---|
| 0 | idle (floating) | "come here", "stand up", "wake up" | |
| 1 | sit | "sit", "stay" | |
| 2 | jump | "jump", "hop" | after 2 s |
| 3 | spin | "spin", "roll over" | after 3 s |
| 4 | sleep | "sleep", "take a nap", "lie down" | |
| 5 | dance | "dance" | after 10 s |
| 6 | trick (a backflip) | "do a trick", "show off" | after 4 s |
| 7 | happy | (plays after a headpat) | after 3 s |
| 8 | hide (stays behind for others to find) | "hide", "go hide" | |
| 9 | shy (turns away, shrinks out of sight) | (when a shy toggle turns on: see the Pet page) | |

## What happens in game

- **"Mochi, dance":** VRCompanion sets `PetState`, everyone sees it dance.
- **Someone pets it:** its mood goes up, it does a happy bounce, and its bubble
  says "Mochi loves the headpats!".
- **Someone grabs it:** it wakes up if it was asleep, and says "wheee!".
- **"Mochi, switch to fox":** it does it, and says so in its bubble.
- **With the AI brain on:** "Mochi, do you like pizza?" gets an answer in its
  bubble, in character.

## Good to know

- **OSC only works in PC VRChat.** Quest players see the pet if your avatar's
  Quest version has it too, but you can only control it from PC. For a Quest
  upload, switch the two materials to `VRChat/Mobile/Toon Lit`.
- VRChat doesn't say *who* touched or grabbed it, so reactions can't name them.
- Mood and headpat count are kept by VRCompanion (`pet.json` next to your
  settings), so they carry over between avatars and sessions.
- Pats and grabs only count while VRCompanion is listening.
- The pet itself floats relative to you; only its hidden copy (hide and seek)
  stays in place in the world.
