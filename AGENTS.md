# Agent Guide

Quick orientation for AI coding agents (and humans) ramping up on this
codebase. Read this before editing files.

## What this mod is

Crusader Kings II mod: a single "Recruit an Elite Specialist" decision
that opens a sex/role menu and produces a high-end recruit for any of the
five council seats — marshal, steward, chancellor, spymaster, or court
chaplain.

The user-facing summary lives in `README.md`. Version history is in
`CHANGELOG.md`.

## Critical gotchas — read these first

### 1. File encoding: Windows-1252 + CRLF for `.txt` and `.csv`

CK2 reads `.txt` (script) and `.csv` (localization) files as
**Windows-1252 with CRLF line endings**. The repo's `.gitattributes`
checks these files out as Win-1252+CRLF on disk while storing the blobs
as UTF-8 in git for readable diffs.

**Practical rule:** if you edit a file with accented characters and save
it with a UTF-8 editor, the accents corrupt once git reinterprets the
bytes. Use a Win-1252-aware tool (a small Python script with `cp1252`
encoding, or a text editor configured for Windows-1252 + CRLF). After
editing, run `git add --renormalize .` and confirm `git status` is clean.

Plain-ASCII edits (no accented chars) are unaffected. `.md` files are
plain UTF-8 and need none of this.

### 2. Scripted-effect invocation: `= yes`, never `= {}`

When calling a scripted effect, use:

    some_scripted_effect = yes

NOT:

    some_scripted_effect = { }

The `= { }` form silently doesn't execute and causes confusing debugging.

### 3. `replace_path` conflicts with overhauls

Large overhaul mods (CleanSlate, HIP, AGOT, ...) use `replace_path` in
their `.mod` file to claim shared folders. Without a `dependencies` hint
in this mod's `.mod` file, the overhaul silently suppresses our content.
See README's *Compatibility → Overhauls* section for the dep-declaration
pattern.

## Repo layout

```
EliteRecruitment.mod                            top-level mod descriptor
EliteRecruitment/                               the actual mod content
  descriptor.mod                                inner descriptor (mirror)
  decisions/elite_recruitment.txt               the one decision
  events/elite_recruitment_events.txt           the ER.* event chain
  common/scripted_effects/*.txt                 per-role effect files + shared
  common/scripted_triggers/*.txt                gates + helpers
  common/game_rules/elite_recruitment_rules.txt the cost-tier rule
  common/on_actions/elite_recruitment_on_actions.txt   startup flag
  interface/elite_recruitment.gfx               sprite registrations
  gfx/interface/*.dds                           decision-icon textures
  gfx/event_pictures/*.dds                      event-banner textures
  localisation/*.csv                            Win-1252 strings
EliteRecruitmentProper4KUIPatch/                hi-res asset overrides
EliteRecruitmentProper4KUIPatch.mod             patch top-level descriptor
CHANGELOG.md                                    version history (Keep a Changelog)
README.md                                       user-facing docs
```

## Testing

**There are no automated tests** — verification happens in-game only.

As an agent, you can:

- Verify syntactic correctness (brace balance, proper file encoding,
  reference consistency).
- Cross-check references (e.g., a `picture = GFX_X` line in an event must
  match a `spriteType { name = "GFX_X" ... }` declaration in the `.gfx`
  file; a `text = some_key` in script must match a row in a `.csv`).
- Confirm trait IDs you add or remove exist in both vanilla and
  CleanSlate (some vanilla trait IDs don't exist under CleanSlate — see
  `common/scripted_effects/elite_recruitment_trait_compat.txt`).

You **cannot** verify in-game behavior. After making changes, surface
them to the maintainer for in-game testing, and capture any uncertain
edge cases as explicit questions in your handoff.

## Adding graphics

Graphics ship in two places, matching the dual-mod pattern:

- **Base mod:** `EliteRecruitment/gfx/interface/*.dds` (decision icons)
  or `EliteRecruitment/gfx/event_pictures/*.dds` (event banners). Use
  vanilla CK2 native resolutions: **28×28** for decision icons,
  **450×150** for event pictures.
- **Proper4KUI Patch (optional):**
  `EliteRecruitmentProper4KUIPatch/gfx/...` with the same filename at the
  hi-res equivalent. **50×50** for decision icons, **810×270** for event
  pictures.

**Always ship the base-resolution version.** A graphic that only exists
in the P4KUI Patch is invisible to players without P4KUI installed — the
base mod must work standalone.

**Same filename in both folders.** CK2's mod-stack file resolution picks
the patch's file when both mods are loaded; matching filenames is the
mechanism. No registration of the hi-res version is needed; only the
base-mod sprite is declared in `interface/elite_recruitment.gfx`.

**File format:** uncompressed BGRA DDS (32 bpp) works for all UI
textures. No DXT compression needed.

**Event banners are full-bleed** — no transparent margin, no border
ring. The event window's UI provides the gilt frame around the picture.

Register new sprites in `interface/elite_recruitment.gfx`. Events
reference the sprite via `picture = GFX_<sprite_name>` in the event
body.

## Conventions

- **Commit messages:** imperative subject (~70 chars, no period),
  optional bullet body explaining the "why," closing bullet referencing
  the `CHANGELOG x.y.z.` entry.
- **Versioning:** semver in the `0.x` range until first Steam Workshop
  release, then `1.x`+. Every commit tagged with `vX.Y.Z` (lightweight
  tag).
- **CHANGELOG:** [Keep a Changelog](https://keepachangelog.com/) format.
  Add entries under `[X.Y.Z] - YYYY-MM-DD` with sub-sections **Added**,
  **Changed**, **Fixed**, **Removed**, **Deprecated**, **Security**.
- **Comments in script files:** preserve the maintainer's existing
  comment voice; don't reformat or inject new commentary. Don't narrate
  removals in comments — rationale goes in the commit message and
  CHANGELOG.

## Things to avoid

- Editing accented `.csv` / `.txt` files with a UTF-8 editor (see Gotcha
  #1).
- Calling scripted effects with `= { }` (see Gotcha #2).
- Adding new traits without first checking the CleanSlate trait-rename
  list ([trait-compat file](EliteRecruitment/common/scripted_effects/elite_recruitment_trait_compat.txt))
  or the [vanilla traits reference](https://ck2.paradoxwikis.com/Traits)
  — some vanilla trait IDs don't exist under CleanSlate.
- **Shipping graphics only in the Proper4KUI Patch** — the base mod must
  work standalone.

## Resources

- **CK2 modding wiki:** https://ck2.paradoxwikis.com/Modding — the
  authoritative reference for CK2 modding syntax (events, decisions,
  triggers, effects, game rules, etc.).
- **Traits reference:** https://ck2.paradoxwikis.com/Traits — every
  trait ID with its modifiers and `leader = yes` flag. Critical for any
  flavor-roll or strip work.
- **Vanilla CK2 game files** — if you have access to a CK2 installation,
  the vanilla script is an invaluable reference (same syntax as the mod).
  Common locations:
  - Steam: `C:\Program Files (x86)\Steam\steamapps\common\Crusader Kings II\`
  - GOG: `C:\Games\GOG Galaxy\Crusader Kings II\`

  Key folders:
  - `events/` — vanilla events; the canonical syntax reference
  - `decisions/` — vanilla decision patterns (including conditional `picture` blocks)
  - `common/scripted_effects/`, `common/scripted_triggers/` — every vanilla scripted effect/trigger
  - `common/traits/` — every vanilla trait definition
  - `gfx/event_pictures/` — vanilla event banner art (style references)
