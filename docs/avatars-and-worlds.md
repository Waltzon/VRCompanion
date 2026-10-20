# Avatars, outfits and worlds

## Avatars

On the **Avatars** page, **Import from VRChat favorites** (or **+ Add avatar**
and paste an `avtr_...` id). Avatars without a name are numbered: "switch to avi
2", "switch to two", "the second one". Give one a name you can say ("fox") in
**Edit**, plus other names (aliases) separated by commas. **Pictures** shows them
as a grid; click one to switch.

You can only switch to avatars you're allowed to use: your own uploads and public
avatars. VRChat picks up the switch within a few seconds. Without the VRChat
login, switching goes over OSC, which only works for avatars in your VRChat
favorites; if VRChat doesn't switch, the app tells you.

**Current avatar** on Home shows what you're wearing, with a button to add it to
your list if it isn't there.

## Toggles

With OSC on, the app asks VRChat which toggles the avatar you're wearing has, and
how each is set right now (OSCQuery): when the app starts and every time you
change avatars. What VRChat sends when you flip a toggle in game, and what the
app sets itself, always count over that read (VRChat may report a toggle's
starting value until it changes). If it can't reach VRChat that way, it reads the
list VRChat saves the first time you wear an avatar. It remembers each avatar's
toggles (`avatars.json`), so they work right after a switch, while VRChat is
closed, and for Build & Test avatars. A toggle called `Clothes/HatOn` can be said as "hat" or
"clothes hat": "turn on hat", "hat off", "toggle hat", "set hue to 50 percent".
To give a toggle an easier name, use **Avatars → Toggles on your current avatar**.

## Outfits

An outfit is a saved set of toggles: "computer, outfit cozy" turns the hoodie on,
the hat off and the socks to style 2 at once.

- **Save one:** dress up in VRChat (app listening, OSC on) and say "**save current
  look**", or press **Save current look** on the Avatars page. Each avatar numbers
  its own: "outfit 1", "outfit 2"... "Save look as cozy" saves it with a name.
- **A look is complete:** every on/off toggle is saved, the ones that are off
  too, so wearing the outfit turns off anything that isn't part of it. The app
  reads every toggle's real value from VRChat when you save. If it can't reach
  VRChat that way, it only knows the toggles VRChat reported when they changed,
  and saves the others as off (the app says how many). Saved an outfit before
  0.25? Press **Update** on it once while wearing it, so it gets its "off"
  toggles too.
- **"Outfit 2" is always the outfit of the avatar you're wearing**; it never
  switches avatar. To switch too: "**switch to avatar 1 outfit 2**" or "fox outfit
  2". The app switches, waits for the avatar to load, then sets its toggles.
- On the Avatars page you can rename outfits, remove single toggles, **Update**
  one with what you're wearing, or make it "Any avatar".

## Worlds and instances

On the **Worlds** page, import favorites or add a `wrld_...` id. The **name** is
what you say: "open black cat", "create new instance black cat friends plus
europe". Each world can have its own default instance type and region; otherwise
the defaults in **Settings → VRChat → Instances** are used.

After creating an instance, the app (by default) sends you an invite to it.
**Settings → VRChat → After creating an instance** can instead **launch** VRChat
straight into it (a `vrchat://` link, works even when the game isn't running), or
do nothing.

## Worlds that set you up

In **Edit** on the Worlds page, "**When I arrive**" can switch you to an avatar
and/or put on an outfit when you enter that world (your swimsuit in a pool world).
It needs live updates (logged in), and waits a few seconds after you arrive.
