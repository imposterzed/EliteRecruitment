# Elite Recruitment — CleanSlate Patch

A small optional add-on for [Elite Recruitment](../README.md) that adapts its traits to the
**CleanSlate** overhaul.

## Why it exists

CleanSlate replaces Crusader Kings II's trait set and renames several of the traits Elite
Recruitment uses:

| Concept | Vanilla | CleanSlate |
|---------|---------|------------|
| Physical strength | `robust` | `brawny` |
| Beauty | `fair` | `attractive` |
| Battlefield Terrain Master | `narrow_flank_leader` | `battlefield_terrain_master` |
| Direct Leader | `experimenter` | `direct_leader` |
| Terrain experts | `*_terrain_leader` | `*_expert` |

The base mod is built for vanilla and uses the vanilla names. This patch flips it to
CleanSlate's equivalents.

## How it works

The patch adds a single `on_startup` action that sets a global flag,
`elite_recruitment_cleanslate`. The base mod checks that flag and, when it's set, uses
CleanSlate's trait names instead of the vanilla ones.

`on_startup` is **additive** across mods, so this doesn't override or conflict with anything.
(An earlier file-override approach didn't work: CleanSlate's `replace_path` on
`common/scripted_effects` makes a second mod's same-named file unreliable. The flag avoids
that entirely.) If CleanSlate ever sets its own startup flag, the base mod could read that
directly and this patch would no longer be needed.

## Requirements & install

- **Elite Recruitment** (base mod)
- **CleanSlate**

Copy the `EliteRecruitmentCleanSlatePatch` folder and `EliteRecruitmentCleanSlatePatch.mod`
into `Documents/Paradox Interactive/Crusader Kings II/mod/`, then enable CleanSlate, Elite
Recruitment, and this patch in the launcher. The launcher orders them correctly on its own —
CleanSlate before Elite Recruitment (so CleanSlate's trait overhaul doesn't suppress the base
mod), and this patch after both via its `dependencies` — so you don't need to reorder anything.

## License

Released under the **MIT License**, same as Elite Recruitment.
