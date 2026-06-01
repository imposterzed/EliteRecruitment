# Elite Recruitment

Elite Recruitment adds court decisions that let you spend gold — and Prestige, for
commanders, diplomats, and spymasters — to bring an elite, ready-made specialist into your court:

- **Recruit Elite Male Commander** / **Recruit Elite Female Commander**
- **Recruit Elite Steward** / **Recruit Elite Stewardess**
- **Recruit Elite Male Diplomat** / **Recruit Elite Female Diplomat**
- **Recruit Elite Male Spymaster** / **Recruit Elite Female Spymaster**

## What it does

Crusader Kings II's vanilla **Promote Commander** decision can already turn up a strong —
even legendary — commander. Elite Recruitment delivers that same high-end commander, and
extends the idea to **stewards**, **diplomats**, and **spymasters**: vanilla's "Invite Noble
to Court" only brings in an ordinary courtier, so a *guaranteed elite* steward, diplomat, or
spymaster has no real vanilla counterpart. Each comes with a clean, deliberate **male vs.
female** choice instead of leaving the recruit's gender up to the game:

- The **male** decision is available to most realms.
- The **female** decision appears for realms whose laws or faith support women holding that
  post. A brought-in courtier is unrelated and unlanded, and for most roles vanilla only
  seats such a woman on the council (or in command) under full gender equality (or an
  equal/matriarchal faith) — so the Female Commander, Stewardess, and Diplomat share that
  gate. The **Female Spymaster** is available more widely: pagan-group realms and the
  Cathar/Messalian faiths let women run the spy network without those laws, matching
  vanilla's own rules for that post.

So you choose the recruit that fits your realm, rather than the game choosing for you.

## The recruits

**Commanders** are elite tacticians — guaranteed a large **Martial** boost (plus a smaller
lift to their other skills), the **Brilliant Strategist** education, and a **battlefield
leadership** trait (defensive leader, flanker, siege leader, etc.).

**Stewards** are elite administrators — guaranteed a large **Stewardship** boost (plus a
smaller lift to their other skills) and **Midas Touched**, the top stewardship education.

**Diplomats** are elite envoys — guaranteed a large **Diplomacy** boost (plus a smaller lift
to their other skills) and **Grey Eminence**, the top diplomacy education.

**Spymasters** are elite intriguers — guaranteed a large **Intrigue** boost (plus a smaller
lift to their other skills) and **Elusive Shadow**, the top intrigue education.

On top of that, every recruit gets a **weighted "flavor" roll** that adds a little
personality — tuned so it *adds* character without changing what they fundamentally are:

- **Most of the time** you get something small and fitting — a touch more of their core
  skill, or a relevant trait (Duelist, Hunter, or Strong for commanders; Administrator,
  Architect, Gardener, or Temperate for stewards; Gregarious, Kind, or Honest for diplomats;
  Deceitful, Schemer, or Cynical for spymasters).
- **Occasionally** something bigger or quirkier appears. These are intentionally rare, so
  your recruit almost always reads as "an excellent specialist," not a lottery prize.
- **Rarely**, if you carry a bloodline that *inspires commanders*, a commander recruit can
  be a **legendary, named hero** — a unique nickname and a stronger set of traits.
- Recruits also pick up **faith-appropriate touches** where they fit (a caste trait for
  Indian religions, a zodiac sign for astrological faiths, and so on), and a commander may
  turn up a **holy-war veteran** trait matching their faith (Crusader, Mujahid, and the like).

The baseline is always "a reliably excellent specialist"; the random flavor is the
seasoning, never the main course.

## Game rules

Elite Recruitment adds **game rules** so you can tailor it to your campaign. Set them on the
**Game Rules** screen when you start a new game (they live under the "Various" group).

- **Elite Recruitment** — *On* (default) / *Off*. Enables or disables all of the mod's
  recruitment decisions at once. It defaults to **On**, so the mod works out of the box
  even if you never open the rules screen.
- **Elite Recruitment: Gold Cost** — *1x* / *2x* / **4x** (default) / *6x* / *8x*. Scales the
  gold price of every recruit (commanders, stewards, diplomats, *and* spymasters). The base *1x* is the original
  cost — about a quarter of a year's income — so the default *4x* is roughly a full year's.
  Each tier also carries a small minimum equal to its multiplier (1/2/4/6/8 gold) so the
  price stays meaningful even for the poorest realms.
- **Elite Recruitment: Prestige Cost** — *1x* / *2x* / **4x** (default) / *6x* / *8x*. Scales
  the Prestige price of **commander, diplomat, and spymaster** recruits (stewards never cost
  Prestige). The base *1x* is 25 Prestige, so the default *4x* is 100.

## Requirements

- **Crusader Kings II `3.3.x`.** The base game is all you need — nothing below is required.

Several optional flourishes appear only if you own the relevant DLC. Without it, that content
is simply skipped — the mod still works:

- **Holy Fury** — legendary, named commanders (when the ruler carries an *inspire commanders*
  bloodline), warrior-lodge sponsorship and the lodge's command trait, and faith flavor
  (zodiac signs, reformed-pagan traits).
- **Jade Dragon** — Chinese "Way of the…" commander traits for Chinese-culture realms.
- **Rajas of India** — Indian flavor: a caste trait and the *War Elephant Leader* trait.
- **The Reaper's Due** — the rare *One-Eyed* commander flourish.

## Installation

Copy the `EliteRecruitment` folder and `EliteRecruitment.mod` into:

```
Documents/Paradox Interactive/Crusader Kings II/mod/
```

…then enable **Elite Recruitment** in the launcher.

## Compatibility, fixes & recommendations

Elite Recruitment works on plain vanilla and adds only new content (it doesn't overwrite
base-game files), so it's friendly with most mods.

**Built-in fixes.** Where vanilla's courtier generation has rough edges or out-of-date
bits, this mod uses corrected, current conditions and trait handling — several of them
borrowed from the excellent **CleanSlate** overhaul — so it behaves properly on a modern,
fully-patched game.

**Built for vanilla; auto-adapts to CleanSlate.** Every trait this mod uses exists in
vanilla CK2, so it's vanilla-safe out of the box. CleanSlate renames several of those traits
(physique, beauty, and some leadership traits like *Battlefield Terrain Master* and the
terrain experts). The mod detects CleanSlate automatically: CleanSlate sets a
`cleanslate_active` flag at startup, and when that flag is present the mod uses CleanSlate's
trait names instead. **CleanSlate users — make sure you're on the latest version from
[GitHub](https://github.com/ck2plus/CleanSlate)**, as that startup flag is a recent addition.
On an older CleanSlate that lacks it the mod still works; recruits just won't roll those
renamed trait variants.

**Overhauls & CleanSlate.** CK2 generally loads small add-ons like this *after* a larger
overhaul on its own, and the CK2 launcher has no manual "reorder" — so you normally don't
have to do anything. This mod was built and tested alongside the **CleanSlate** overhaul,
which also fixes a large number of vanilla bugs by itself. CleanSlate is **not required**,
but running it (it's free) is recommended for the cleanest overall experience. If you run a
*different* overhaul and the decisions don't appear, that's a load-order conflict — the
overhaul is replacing the decisions folder and loading after this mod.

## Notes

- The decisions reuse vanilla icons as placeholders — the "Employ Soldier" icon for
  commanders, the "Employ Steward"/"Employ Stewardess" icons for stewards, the **Diplomacy**
  attribute icon (a sealed scroll) for diplomats, and the **Intrigue** attribute icon (a
  dagger) for spymasters. Custom icons are planned. They'll automatically use a 4K-UI mod's
  higher-res icons (e.g. Proper4KUI) if one is installed.

## License

Elite Recruitment © 2026 imposterzed — released under the **MIT License** (see [`LICENSE`](LICENSE)).
You're free to use, modify, and redistribute it; just keep the copyright and license notice.
