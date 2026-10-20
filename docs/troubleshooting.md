# Troubleshooting

**It doesn't react when I talk.**
Watch the mic meter on Home while you talk: the bar should turn green and pass the
yellow line. If it doesn't, drag the yellow line to the left (lower), pick the
right mic in **Settings → Listening**, or raise the mic volume in Windows sound
settings. Say "computer, hello": it answers if it
heard you.

**It reacts when nobody's talking, or cuts me off.**
It learns your room's background noise and only counts sound clearly above it.
If it still picks up the room (a fan, music), drag the yellow line on the mic
meter to the right, just above where the bar sits when you're quiet. If it cuts you off mid-sentence, set **Settings → Listening → Pause that ends a
sentence** longer. Noisy rooms work best with [push-to-talk](voice.md#push-to-talk).

**The wake word doesn't always work, or goes off by itself.**
Watch for the blip: it plays as soon as it hears the wake word. No blip although
the mic meter went green, or a blip and then nothing? It may not have been sure
it was the wake word (the log says "not sure it was the wake word"): lower
**Wake word certainty** in **Settings → App → Advanced** (default 0.3, e.g. to 0.15), or use
"Didn't catch that" on Home to count what it heard instead as the wake word.
Blips when nobody said it (noise, other players, game audio)? Raise
**Wake word certainty** (e.g. to 0.6), drag the yellow line on the mic meter higher,
or pick a longer, more unusual wake word. You can also say just the wake word,
wait for the blip, then the command.

**It mishears a name or a command.**
Use "Didn't catch that" on Home, or say it again and then "remember that". See
[When it mishears you](voice.md#when-it-mishears-you). Names should be real
English words; give hard ones a nickname or another name.

**Chat mode mishears a lot.**
Switch the chat engine to Whisper, or [Whisper on another computer](server-setup.md).

**Nothing appears in my chatbox / toggles and the pet don't work.**
Turn OSC on in VRChat: Action Menu → Options → OSC → Enabled. The app then asks
VRChat for your avatar's toggles itself (OSCQuery), also right after you upload
an avatar with new ones (like the pet). Only if a toggle you just added is still
missing: **Reset Config** in the same menu, once.

**"Port 9001 is taken by another OSC app".**
Another OSC app is listening where VRChat sends OSC. The app then listens on a
free port and tells VRChat about it over OSCQuery, so both apps get everything.
If toggles and mute info still don't arrive, close the other app and start
listening again.

**It can't find VRChat (OSCQuery).**
VRChat must be running with OSC on, on this PC. The app finds it on your local
network (mDNS): a firewall that blocks the app on private networks stops that.
Without it, everything still works the old way: toggles come from the files
VRChat writes for OSC, and a saved look counts toggles VRChat hasn't reported as off.

**"That needs your VRChat login."**
Log in on the **VRChat account** page (if VRChat logged you out, log in again).
Using the app without an account? Avatar switching then goes over OSC, which
VRChat only allows for avatars in your VRChat favorites: favorite it in VRChat
first.

**No live updates.**
They need you logged in; Home shows whether they're connected. They reconnect by
themselves after a drop.

**The speech server can't be reached.**
See the [speech server troubleshooting](server-setup.md#troubleshooting).

**The Unity package won't import.**
Use **Assets → Import Package → Custom Package...** in Unity 2022.3 (the
VRChat version) with the VRChat Avatars SDK installed, or drag the file into the
Project window. If Unity shows an error, the Console (Window → General → Console)
has the details. If nothing happens at all, use the **.zip** from the same
release instead ([how](pet-avatar.md#quick-start)); Unity's own log,
`%LOCALAPPDATA%\Unity\Editor\Editor.log`, says why the import was skipped.

**Something else.**
Look at the log: **Settings → App → Diagnostics and backup → Show log** shows it (you can show only warnings
and errors), and **Open folder** takes you to the file, `vrcompanion.log` in
`%APPDATA%\VRCompanion`. Then press **Make a bug report**, write what you did,
and open an [issue](https://github.com/Waltzon/VRCompanion/issues): the report
is already copied, just paste it. It has the app version, your Windows version,
your settings, what the app was doing and the end of the log. Your VRChat login,
API keys, passwords, emails and user IDs are taken out first (it's also saved as
`bug-report.txt` in the same folder, so you can check before you post).
