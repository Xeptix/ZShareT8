# ZShare T8

**Weapon sharing for Black Ops 4 Zombies**

by Xep

[**Download the latest release**](https://github.com/Xeptix/ZShareT8/releases/latest)

Trade guns with a teammate, hand them points, give away a box hit you don't want, and pay
for a teammate's perk or spin. All of it from the use button.

- Look at a teammate and press **use** to trade weapons. Ammo, attachments, camo and the
  Alternate Ammo Type go with the gun.
- **Crouch** first and the same press gives them 1000 points instead.
- Crouch and press use at the **box** or the **Pack-a-Punch** while the weapon you paid for
  is up, and anyone can take it.
- Crouch and press use at a **perk altar**, or at the box or Pack-a-Punch while nobody is
  using it, and the next teammate to use it pays nothing.

This is the Black Ops 4 port of [ZShare](https://github.com/Xeptix/ZShare). Every port has
the same settings, with the same names, defaults and meanings.

---

## Requirements

Black Ops 4 zombies with [Shield](https://github.com/ellabrella/shield) installed. No other
mods or dependencies.

**Only the host needs this.** Every part of ZShare runs on the host and reaches everyone
else as ordinary server-to-client traffic — the prompts, the swap, the box, the altars, the
points, the sounds. Players joining your game install nothing.

---

## Install

Copy the **`zshare`** folder from the download into your Black Ops 4 mods folder:

```
project-bo4\mods\zshare\
```

It holds two files, `metadata.json` and `zshare.gscc`. Shield reads the manifest and loads
the script; there is nothing to enable and no load order to worry about.

### Or run the installer

The download has an **`installer`** folder, one for each system:

```
installer\windows\install.bat
installer/linux/install.sh
```

Each one finds your Black Ops 4 folder and puts the `zshare` folder in
`project-bo4\mods`, then shows you what it is about to copy and asks once.
`install.bat -Yes` and `install.sh --yes` copy without asking, `-Uninstall` / `--uninstall`
removes what an install put there, and `-Find` / `--find` only shows what it detects. When it
can't find your game, `-To <folder>` / `--to <folder>` points it there.

---

## Usage

Every action is a prompt on screen, and what it offers is what a press of **use** will do.

| Action | Input |
|---|---|
| Offer your weapon to a teammate | Look at them, press **use** |
| Accept a weapon they offered | Look at them, press **use** |
| Withdraw an offer | Look at them, press **use** again |
| Give a teammate 1000 points | **Crouch**, look at them, press **use** |
| Thank whoever just did you a good turn | **Crouch**, look at them, press **use** |
| Share the box weapon you paid for | **Crouch**, press **use** at the box |
| Share the weapon waiting at the Pack-a-Punch | **Crouch**, press **use** at the machine |
| Take a shared weapon | Press **use** at the box, or at the sharer's prompt at the Pack-a-Punch |
| Pay for a teammate's perk | **Crouch**, press **use** at a perk altar |
| Pay for a teammate's box spin | **Crouch**, press **use** at the box while nobody is using it |
| Pay for a teammate's Pack-a-Punch | **Crouch**, press **use** at the machine while nobody is using it |
| Take your payment back | **Crouch**, press **use** at the machine you paid for |

Crouching is the whole modifier. Stand up and everything goes back to what it always was;
crouch and it turns into giving. Nothing else changes about how you play.

---

## Trading

What you offer is **the weapon in your hands** when you press. Your teammate can see it
there; nothing needs naming. What you get back is **whatever they're holding** when they
accept — so they switch to the gun they want to give before pressing.

An offer stays open for 10 seconds. It lapses on its own if you switch to your other
weapon, if the two of you move more than twice the prompt range apart, or if either of you
goes down. Pressing use on them again withdraws it.

**What travels with a gun:** the ammo, the attachments, the camo, the Alternate Ammo Type
and the Pack-a-Punch tier. It arrives as the same weapon that left, rather than one rebuilt
from the receiver's own build kit.

**What can't be traded:** grenades, the knife, shields, mines, equipment, hero weapons and
gadgets.

**Same gun twice.** A trade that would leave someone holding a gun they already carry — or
the other half of it, plain against Pack-a-Punched — is refused.

---

## Sharing a box hit

You pay, the weapon rises, and it's yours to take. **Crouch and press use** at the box and
it's everyone's: the prompt changes for the whole room, and whoever presses use takes it.

Black Ops 4 can already do this on its own — the player the weapon belongs to can press
**melee** at it — and ZShare's crouched press sets exactly the same thing. Either works,
and the melee press keeps working whether ZShare is installed or not.

## Giving points

Crouch, look at a teammate, press use: 1000 points move from you to them. Each press is one
gift, a second apart, so three presses is 3000. You need the points to give them.

---

## Paying for a teammate

Crouch at a perk altar, or at the box while nobody is using it, and press use to pay the
price yourself. From then on the next teammate to use it pays nothing — a drink from that
altar, or a spin from the box. Everybody hears about it, and whoever paid is told who used
it.

One payment waits at a machine at a time. While yours is waiting, crouch at it again and
the prompt offers your points back.

| When | What happens |
|---|---|
| A paid spin turns up the teddy bear | The spin cost nothing, so the game's own refund is nothing — whoever paid gets their points back instead |
| The box moves | The paid spin moves with it |
| A fire sale is on | Nobody can pay at the box while it's cheap |
| You're playing solo | There's nobody to pay for, so the prompt never appears |

**A perk altar pours whatever that player's own loadout has in that slot**, which is how
Black Ops 4 works: the perk your teammate drinks is theirs, not the one you were looking
at, and its price can differ from the one you paid. A payment here is for *a drink at that
altar* rather than for a particular perk — what you paid is what your teammate gets towards
it, so a dearer perk costs them the difference and a cheaper one leaves them the change.

Stand up and every machine works exactly as it always has.

## The perk limit

`zs_perk_limit` sets how many perks a player can hold. `0`, the default, is the game's own
limit. Any other number replaces it, and `-1` removes the limit.

---

## Configuration

Every setting is at the top of the script under `zs_load_config()`, and each one is also a
dvar of the same name. The script creates each dvar with its default on load, so you can
set them straight from the console:

```bash
zs_points_amount 500
```

The config is re-read every five seconds while the game runs, and again on every press,
so a change takes effect **almost straight away** — no map restart needed.

| Dvar | Default | What it does |
|---|---|---|
| `zs_debug` | `0` | Print what the script decides and why, to the Shield log. |
| `zs_trade` | `1` | Trade weapons with a teammate. |
| `zs_trade_offer_time` | `10` | Seconds an offer stays open. |
| `zs_trade_upgraded` | `1` | Pack-a-Punched weapons can be traded. Off, and the prompt says so when you try. |
| `zs_range` | `64` | How close you have to be for the prompt to appear, in units. An offer lapses at twice this. |
| `zs_points` | `1` | Crouch and press use on a teammate to give them points. |
| `zs_points_amount` | `1000` | How many points one press gives. |
| `zs_points_cooldown` | `1` | Seconds between gifts from one player. |
| `zs_thank` | `1` | Thank whoever did you a good turn. |
| `zs_thank_amount` | `100` | What one thank sends. Comes out of your own points. |
| `zs_thank_time` | `30` | How long a good turn stays thankable, in seconds. |
| `zs_box_share` | `1` | Crouch and press use at the box to share the weapon you paid for. |
| `zs_pap_share` | `1` | The same at the Pack-a-Punch. |
| `zs_perk_pay` | `1` | Crouch and press use at a perk altar to pay for a teammate's drink. |
| `zs_box_pay` | `1` | The same at the box, for the next spin. |
| `zs_pap_pay` | `1` | The same at the Pack-a-Punch, for the next pack. |
| `zs_perk_limit` | `0` | How many perks a player can hold. `0` is the game's own limit, a number replaces it, `-1` is no limit. |
| `zs_show_hint` | `1` | Tell players what the prompts do, once, shortly after they spawn. |
| `zs_messages` | `1` | The one-line messages — who traded with whom, who shared or paid for what, who gave points. Off leaves the prompts and the sounds. |
| `zs_offer_sound` | `zmb_trap_ready` | Played to the player an offer is made to. `none` = silent. |
| `zs_trade_sound` | `zmb_powerup_grabbed` | Played to both players when a trade goes through. `none` = silent. |
| `zs_share_sound` | `zmb_spawn_powerup` | Played to everyone else when a weapon is shared or a machine is paid for. `none` = silent. |
| `zs_points_sound` | `zmb_cha_ching` | Played when points are given, paid or handed back. `none` = silent. |
| `zs_deny_sound` | `evt_perk_deny` | Played when a press can't do what the prompt said. `none` = silent. |

### Sounds

All five are stock aliases, so the mod stays a script and a manifest. Swap one in from the
console, or silence one with `none`:

```bash
zs_trade_sound none
```

---

## How this port differs

The same features with the same settings as every other port, allowing for what Black Ops 4
gives a script:

- **The prompts say the game's own words.** On this engine a prompt can only show one of
  the game's own lines — a mod cannot write its own, and cannot add one either. So ZShare's
  prompts borrow the closest thing Black Ops 4 already says, and everything ZShare needs to
  tell you in its own words arrives as a message instead. `docs/porting-t8.md` in the
  project tree has the detail.
- **There are no chat commands.** Every other port takes `!share`, `!thank` and `!tip` from
  chat; Black Ops 4 hands chat to no script at all. Sharing and thanking are on the prompt
  here, and **there is no way to tip an arbitrary amount** — a thank sends `zs_thank_amount`
  and the 1000-point gift is the other press.
- **Thanking and giving points read the same on the prompt.** No stock line says "thank" —
  all 336 of the game's hint strings were searched — so a thank borrows the same cost line
  a gift uses, and you tell them apart by the number on it: 100 against 1000. The message
  afterwards says which it was.
- **A shared Pack-a-Punch is taken from the sharer's prompt**, not your own. The part of the
  machine that hands a finished weapon over runs on one player's copy of the prompt, so
  sharing shows you theirs and takes yours out of the way while it lasts. Nothing to do
  differently — walk up and press use.
- **It installs as a mod folder.** Black Ops 4 has no loose-script path, so what ships is a
  compiled script and a manifest for Shield to read.
- **`none` silences a sound.** An empty dvar can't be set from in game on this engine.

---

## How it works

**The prompts on players are the game's own use triggers,** one per player, linked to them
the way the revive prompt links to a downed player. This engine can put different words on
one trigger for each viewer and hide it from each viewer separately, so there is one per
player rather than one per pair as on the other ports.

**A weapon changes hands through the game's own weapon data** — the same structure the
weapon locker moves — with the gun handed over as the giver's own rather than rebuilt from
the receiver's loadout, which would quietly change the attachments and the camo.

**The box already knows how to be shared, and how to be free.** Sharing sets the two fields
the game's own melee share sets; a paid spin sets the two the game reads for a spin that
opens without charging.

**A paid drink goes through the game's own validation hook,** so the drink, the animation
and the perk are the altar's own work from start to finish.

**Points go straight onto your score,** so a gift doesn't count as points you earned.

---

## Notes

- **Prompts and other prompts.** A teammate standing in front of a door or a machine shares
  the space with it; the engine shows whichever prompt you're looking at most directly.
- **Crouching over a downed teammate** revives them. The pay prompt steps aside for a
  revive, the same way the machine's own prompt does.
- **Downed players** have no prompt and can't accept one. Going down lapses an open offer
  in either direction.
- **Custom maps** get all of it: the prompts, the box and the altars read nothing but the
  stock script structures every Black Ops 4 zombies map is built on.

---

## Testing

ZShare T8 is compiled on every build, so a mistake in the script is a build that does not
finish rather than something you find in a game. It is checked against the stock script
dump on top of that: every function it calls is a real call a zombies script can reach, and
every entity field, notify, flag, sound alias and localized string it borrows exists there.

Black Ops 4 stores every name as a hash, so the localized strings are checked by hashing
the name and finding the hash — the same way the project reads the game's hashed names back
into words.

---

## Ports

| Game | Repo |
|---|---|
| Black Ops 4 (T8) | ZShareT8 — you are here |
| Black Ops III (T7) | [ZShareT7](https://github.com/Xeptix/ZShareT7) |
| Black Ops II (T6) | [ZShare](https://github.com/Xeptix/ZShare) |
| Black Ops (T5) | [ZShareT5](https://github.com/Xeptix/ZShareT5) |
| World at War (T4) | [ZShareT4](https://github.com/Xeptix/ZShareT4) |

Versions are kept in step: the same version number means the same feature set, allowing
for what each engine can actually do.

**All five in one download.** The [Treyarch
Bundle](https://github.com/Xeptix/ZShare/releases/latest) carries every game ZShare runs
on, laid out as each drops in — the `Plutonium` tree for three of them, Black Ops III's
folders, this game's mod folder — with one installer that asks which of them to install.

---

## Changelog

### v1.2

- **Maps without perk altars are left untouched.** ZShare now leaves the global perk
  validation callback alone when there is no altar to extend.

### v1.1

- **A trade of two identical guns is refused** — "You already have that weapon" — the way
  Black Ops II has always refused it. Only the same gun both ways; a plain gun for its
  Pack-a-Punched version still trades.

### v1.0

- Initial release.
- **Trade weapons** with a teammate from the use prompt. Ammo, attachments, camo, the
  Alternate Ammo Type and the Pack-a-Punch tier travel with the gun.
- **Give points** with a crouched press of the same prompt. `zs_points_amount` sets how
  many.
- **Share a box hit** with a crouched press at the box, the same thing the game's own melee
  press does, and **share a Pack-a-Punch** the same way.
- **Pay for a teammate** at a perk altar, the box or the Pack-a-Punch with a crouched
  press, and take the payment back the same way. `zs_perk_pay`, `zs_box_pay` and
  `zs_pap_pay` switch each one.
- **Thank a teammate** who paid for you, shared a hit or gave you points: for a while the
  crouched prompt on them offers a small thank. The points come out of your own.
  `zs_thank_amount` and `zs_thank_time` set how much and how long.
- **`zs_perk_limit`** — how many perks a player can hold, or `-1` for no limit.
- Every setting is a dvar, re-read while the game runs.
