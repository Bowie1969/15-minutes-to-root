# How dangerous is this stuff, really — plain English

Short answer: **it depends who has it**. On a locked-down big-company network with
decent IT, this gear and this AI don't do much. On an unlocked laptop in a cafe, a
small shop till, a doctor's reception desk, or an unattended office kiosk — it does
plenty, fast, for very little money.

## Priced out at retail

- **The keyboard-pretending stick** (BadUSB) — about $25. A real one, not even a
  homebuilt one.
- **The small Wi-Fi gadget** (the relay) — about $35.
- **The AI side** — a few cents an hour. Public services charge in cents, not
  dollars, for the amount of reasoning this run used.
- **Skill needed to make it work** — enough to follow a tutorial. The AI did the
  thinking.

Total: **about $60**, under an hour of setup time, no computer science degree
required.

## Danger, ranked by part

**The keyboard trick:** moderately scary. It works if the screen is unlocked. If
the screen is locked, it does nothing. This is the oldest trick in the book
(someone walks past, plugs in a stick, screen unlocked, they own the computer).
The fix is the oldest fix: lock the screen.

**The small Wi-Fi gadget:** more scary than it looks. It is a tiny messenger
that sits on your own Wi-Fi, relaying commands from the attacker. Normal "watch
for weird traffic going out to the internet" defences don't see it, because it
never talks to the internet. Only something that notices "there's a new device
on my Wi-Fi I didn't put there" would catch it.

**The AI:** force multiplier, not magic. In this run it saved maybe 10 minutes
versus a competent human. The real worry is that it does the boring parts
tirelessly and in parallel — one person could run ten of these at once.

**The password in a plain text file:** this is the actual villain of the story.
Everything else is just hardware wrapping around the fact that the owner had
left their password lying around in a readable file. **No password in the
file, no root for the AI.** Fix this habit and this entire run doesn't happen.

## Who's at real risk

- **High risk:** anywhere a stranger can briefly be alone with an unlocked
  screen or an open USB port. Think: retail tills during a shift change, small
  medical practices, co-working desks, hairdressers, cafes, hotel business
  centres.
- **Medium risk:** small-business offices where staff share devices, home
  offices with cheap routers.
- **Low risk:** anywhere the screen auto-locks in under a minute, USB sticks
  aren't trusted, the Wi-Fi has a separate guest network, and every laptop has
  antivirus with monitoring turned on.

## Who's at low risk

Big companies that have taken this seriously. Not because their walls are
higher, but because they've thought about it: locked screens, device
allowlists, people looking at logs. A full-size enterprise is boring to a
rig like this.

## What the AI changes vs five years ago

Not as much as the news might suggest, but not nothing. The hardware trick
is 15 years old. The scanning techniques are 25 years old. What the AI adds
is that one person can keep ten of these going at once, each one adapting
when they hit a snag. It lowers the amount of attention per target, not the
cost of a single target.

## Not a doomsday device

To be clear: this did not come close to what a competent human attacker
with the same hardware could do. The AI took the easy win and left two
flashier bugs on the table. In the wrong hands this is a cheap tool for
soft targets — nothing more, nothing less.
