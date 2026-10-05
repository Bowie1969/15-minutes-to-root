# How to stop this from happening to you — plain English

Doing even one of these would have stopped this run. Doing three or four of them
would stop almost every realistic version of it.

## The one habit that defeats this run

**Do not type a password where your computer records it.** On most computers,
every command you type into the black terminal window gets saved into a file.
If you ever type your own password into that window — even "to test
something" — it goes into that file in plain text. Anyone who reads the file,
reads the password.

In this run that is the only reason the AI got in. If I hadn't done that, it
would have been stuck.

**How to break the habit:**
- Never type your password at the terminal prompt. Let the system ask for it
  normally.
- If you must, put a space at the start of the line (on Linux and macOS, many
  shells skip space-prefixed lines in history by default — but only if that
  option is on).
- Use a password manager. The password should live in the manager and nowhere
  else.
- Once in a while, open the history file in a text editor and look through it.
  Erase anything that shouldn't be there.

## The five things to do on the computer itself

1. **Set the screen to lock quickly** — one minute of idle. This defeats the
   USB-stick trick dead in its tracks. The stick needs a logged-in session to
   type into; a locked screen has nothing to accept the keystrokes.

2. **Put a password on the computer that stops at boot** — a BIOS/UEFI
   password. Stops someone changing boot order to boot their own OS off a USB.

3. **Keep updates on.** The lab target had a known, patchable flaw sitting
   there. In this run the AI didn't bother using it, but a human attacker would
   have.

4. **Install antivirus with monitoring** — the free one that ships with
   Windows is fine; on Mac, use the one your work gives you; on Linux, use
   ClamAV or at least `auditd`. The AI's "download this and run it" one-liner
   would have screamed through any monitoring.

5. **Don't trust USB.** If you find a stick, don't plug it in. If someone
   hands you one, don't plug it in. If you need to receive a file from
   someone, use email or cloud storage.

## The five things to do on the network

1. **Separate your Wi-Fi.** Have a guest network for visitors and anything
   that doesn't belong to you — a smart TV, a baby monitor, a visitor's
   phone. Keep it off your work network. This defeats the Wi-Fi gadget: it
   lands on the guest network, and the guest network can't reach your real
   stuff.

2. **Change the Wi-Fi password every six months**, more often if people have
   left your household or team.

3. **Look at the list of connected devices**, say once a month. If you see
   something you don't recognise, boot it off and change the password.

4. **Turn on alerts for new devices** if your router has that feature.
   Most Mesh/ASUS/Netgear/Google routers do.

5. **Lock your router.** Change the admin password from the default. Turn
   off remote management. Keep its firmware up to date.

## The one habit about people

**If a stranger is near your computer or your Wi-Fi gear, assume the worst
and check.** Most of this run depended on physical access for 10 seconds.
Locked screens and USB discipline fix that. The rest is just hygiene.

## In case this already happened to you

- Change passwords from a different device (your phone, not the laptop).
- Pull the laptop off the network.
- If this is a work device, call IT first.
- Keep the laptop in that state until someone qualified has looked at it.

## Why "antivirus" alone isn't enough

Modern attacks don't carry traditional viruses. They borrow the computer's
own tools. In this run every attacker action was done through things that
Linux ships with — `curl`, `bash`, `sudo`, `espeak-ng`, `aplay`. Antivirus
pattern-matching for "known bad files" sees nothing. You need something
that watches *behaviour*, which is what the paid tools, and modern free
ones, do.
