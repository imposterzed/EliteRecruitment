# Changelog

All notable changes to **Elite Recruitment** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
Versions remain in the `0.x` range (in development) until the first Steam Workshop
release, which will be tagged `1.0.0`.

## [Unreleased]

## [1.0.5] - 2026-07-14

### Changed
- Convert leading tabs to 2-space indent in the events file and the interface `.gfx`.
- Rewrite the Debug sub-mod's events file with CRLF line endings and CP1252 en-dashes.
- Drop two blank-in-block lines.

## [1.0.4] - 2026-07-14

### Added
- **Gender Gating game rule.** Adhere (default) keeps existing
  eligibility gates. Bypass opens every role to both genders
  regardless of council law or faith ordination.

### Changed
- Cost rule name now opens with a dark green "Elite Recruitment:"
  prefix.
- Cost rule descriptions cleaned up.

## [1.0.3] - 2026-07-13

### Added
- **Sub-mod thumbnails.** The Debug and Proper4KUI Patch sub-mods now
  have their own thumbnails (badged variants of the base thumbnail).

### Changed
- Small descriptor and doc housekeeping.

## [1.0.2] - 2026-06-07

### Added
- **`docs/thumbs/`** — 180-px-tall thumbnail versions of the four
  in-game screenshots (`decisions`, `gender-picker`, `role-picker`,
  `arrival`), for use in forum posts where inline `[img width=X]`
  sizing isn't supported by the forum's BBCode parser.
- **README "Community" section** linking the four discussion channels:
  Steam Workshop discussions, Paradox forum thread, GitHub Issues, and
  GitHub Discussions.

## [1.0.1] - 2026-06-07

### Added
- **README Steam Workshop links.** Installation section now leads with
  Workshop subscribe links for the base mod and the Proper4KUI Patch;
  manual install instructions kept as a fallback.
- **`picture =` field** in the Proper4KUI Patch's `.mod` descriptor so
  Steam Workshop auto-populates the Patch's thumbnail at upload time
  (CK2's Workshop doesn't allow setting a thumbnail after the initial
  upload). Reuses the base mod's `elite_recruitment.jpg` for visual
  brand consistency.

### Changed
- **Proper4KUI Patch scope** in README updated to reflect the actual
  art coverage (all 14 bespoke assets — decision icon + menu and
  arrival banners — not just the icon). The line was written during
  the Patch's early life when only the icon shipped at hi-res.

## [1.0.0] - 2026-06-07

First public release.

### Added
- **README banner.** Added at the top of the file, replacing the
  bare `# Elite Recruitment` heading.
- **Social preview image.** A 2:1 variant of the banner
  (`docs/social-preview.jpg`) for GitHub's link-thumbnail card.
- **README screenshots.** Four in-game shots (decisions panel,
  gender picker, role picker, recruit arrival) inserted after the
  lede paragraph.
- **`SECURITY.md`** community-profile file pointing security reports
  through GitHub's Private vulnerability reporting feature.
- `Council` tag added to `EliteRecruitment.mod`'s tag set.

## [0.15.5] - 2026-06-07

### Changed
- **Debug events extracted into a sibling sub-mod (Elite Recruitment -
  Debug).** The `ERD.*` event family (currently just `ERD.1` granting
  `legendary_commander_bloodline`) moved out of `EliteRecruitment/events/`
  and into a new `EliteRecruitmentDebug` sub-mod that declares
  `dependencies = { "Elite Recruitment" }`. The sub-mod is opt-in: only
  loaded when explicitly enabled in the launcher. Pre-1.0 hygiene -- the
  player-facing Workshop release no longer ships console cheat events,
  while the dev tool is preserved for future testing of hard-to-reach
  paths (bloodline grants, SCM-gap edge cases, etc.).

## [0.15.4] - 2026-06-07

### Fixed
- **Empty-menu trap on ER.11/ER.12.** Catholic + `gender = all` + only
  chaplain affordable let the decision pass and ER.10 offer both sexes,
  but picking the female route landed on an empty ER.12 (Catholic
  faith-blocks female chaplain; the other four female roles unaffordable).
  Since CK2 popups can't be dismissed with Esc and the role pickers have
  no cancel option by design, the player was trapped in a dead-end menu.
  Two new triggers
  `elite_recruitment_any_male_role_eligible_and_affordable_trigger` and
  `elite_recruitment_any_female_role_eligible_and_affordable_trigger`
  AND-combine each per-role sex-gate with its can-afford trigger,
  mirroring the per-option visibility logic inside ER.11/12. The decision
  `allow` and the orchestrator's gender-routing chain both use these
  combined triggers, so the routing into ER.10 / ER.11 / ER.12 now matches
  per-role visibility exactly -- the player can never be routed into an
  empty role picker.

## [0.15.3] - 2026-06-07

### Changed
- **En-dash unification.** Replaced ASCII `--` with the W1252 en-dash (`–`) in
  the decision description and the gender-picker description across all four
  languages. Aligns with Paradox's house style -- vanilla CK2 uses en-dash
  500+ times in its loc files and never uses ` -- ` as in-sentence punctuation.
  Also unifies Spanish, which previously used `:` as a stylistic alternative.
- **Decision-description role order** reordered to council order (Chancellor →
  Marshal → Treasurer → Spymaster → Lord Spiritual), matching the order the
  role pickers display. Previously listed Marshal first.
- **Dynamic titles extended to FR / DE / ES** in male-or-neutral strings: the
  decision description, the male menu options (ER.11/13), the arrival
  tooltips (`recruit_*_TT`), and the ER.1 appoint and replace options. 22
  rows in total now resolve to localized cultural titles instead of
  hardcoded male nouns (e.g., a French Bon ruler now sees
  `un Grand médium` instead of `un aumônier` for the chaplain). The female
  menu rows (`ER_12_*`, `ER_14_*`) are deliberately unchanged -- they keep
  their explicit feminine declensions (`maréchale`, `Marschallin`,
  `mariscala`) since the getters return masculine cultural titles only.
- **German ER.1 appoint rows restructured** from the genitive
  `Ich gebe X das Amt des Marschalls` to the accusative
  `Ich ernenne X zum §Y[Root.GetMarshalName]§!`. The nominative getter
  output is grammatically correct with the accusative `zum`, where the
  earlier genitive case would have dropped the `-s` suffix.
- **Added §Y§! highlighting** around the new getter references in FR / DE /
  ES rows that previously rendered the role noun without highlighting, so
  the cultural title visually pops the same way it does in EN.

## [0.15.2] - 2026-06-07

### Fixed
- **Vanilla marshal ER.1 abort on holy-war trait roll.** On the vanilla stack
  (no CleanSlate), the 15% chance to roll a holy-war trait via
  `add_crusade_trait_effect` silently killed the marshal recruit's ER.1
  arrival pop-up -- the recruit joined court, but the appoint/replace prompt
  never fired. Vanilla's `add_crusade_trait_effect` is a sequence of bare
  `if` blocks each ending with `break = yes`, and that break propagates up
  through the marshal trait orchestrator, out of the `new_character` scope,
  and into the parent option block, aborting the
  `character_event = { id = ER.1 }` call. CleanSlate's version uses a clean
  `if`/`else_if` chain without break, so the bug only surfaced on the
  vanilla stack. Inlined the religion -> holy-war trait dispatch inside
  `assign_marshal_crusade_trait` so vanilla's broken effect is never called.

### Added
- **Five new `assign_elite_*_crusade_trait` compat hooks** in
  `elite_recruitment_trait_compat.txt` for the five trait IDs CleanSlate
  renamed: `tengri_warrior`/`skylord`, `ukkos_shield`/`ukkos_hammer`,
  `romuvas_own`/`hound_of_dievas`, `eagle_warrior`/`eagle_knight`,
  `shaddai`/`kanai`. Used by the inlined crusade-trait dispatch above so
  the right ID lands on the recruit on both stacks.

## [0.15.1] - 2026-06-06

### Fixed
- **War Elephant Leader culture gate.** Marshal recruits in Indian-cultured realms
  never rolled the `war_elephant_leader` trait because the gate used
  `culture_group = indian_group`, which doesn't exist in CK2. Replaced with
  `OR = { culture_group = indo_aryan_group culture_group = dravidian_group }`
  (Bengali / Punjabi / Rajput / Tamil / Telugu / Kannada all qualify). Verified
  live on the 22nd Bengali Kshatriya marshal post-fix.

### Added
- **Four missing loc keys** for the ER.11/ER.12 single-page layouts:
  `ER_11_spymaster`, `ER_11_chaplain`, `ER_12_spymaster`, `ER_12_chaplain`. These
  surface when affordability hides enough roles that the spymaster or chaplain
  collapses from page 2 onto page 1 -- previously the player saw the raw loc
  keys. Text cloned from the existing ER_13/ER_14 page-2 counterparts.

## [0.15.0] - 2026-06-06

### Changed
- **Dynamic council titles extended to all EN strings.** Completes the
  dynamic-titles work started in v0.13.0: previously only the menu role-picker
  labels (ER.11/12) used `[Root.Get*Name]` getters; now the decision description,
  per-role tooltips, and the ER.1 appoint/replace option labels all resolve to
  the player's actual council title too. A Tibetan-Buddhist ruler now sees "I
  shall make Jifu my Banchenpo" instead of "...my court chaplain"; a Chinese
  ruler sees "...my Xingjun Sima" for marshal; etc. EN only — FR/DE/ES keep
  their existing gendered translations.

## [0.14.0] - 2026-06-06

### Added
- **Community files for public-release prep:** `.github/ISSUE_TEMPLATE/` with bug,
  compat, and feature templates (plus a `config.yml` that disables blank issues
  and routes users to Discussions and the Paradox CK2 forums); `AGENTS.md` for
  AI coding agents and humans (technical onboarding — encoding rules,
  scripted-effect syntax, `replace_path` quirks, file layout, graphics
  conventions, commit style, vanilla resource pointers); `CONTRIBUTING.md` with
  in-game testing notes and a console-commands cheat sheet; `SUPPORT.md` routing
  bug vs compat vs feature vs "how do I" questions; `CODE_OF_CONDUCT.md`
  adopting the Contributor Covenant v2.1.
- GitHub Discussions enabled on the repo (settings change, no committed file).

## [0.13.4] - 2026-06-06

### Added
- **Bespoke arrival banners** for the **ER.1 recruit arrival event**: 10 painted
  banners covering all role + sex combinations (Marshal/Steward/Chancellor/
  Spymaster/Chaplain × M/F). Each is an in-character action shot with the recruit's
  face naturally obscured by the pose (helmet, hood, downcast head over work) --
  the recruit reads as a real person, not an abstract candidate. Wired via ER.1's
  `picture =` conditional blocks keyed off the recruit's `elite_recruit_<role>` flag
  and `is_female`. Replaces the placeholder `GFX_evt_emissary` used for ER.1 since
  the council-seating feature shipped in 0.11.0. 450x150 base, 810x270 in the
  Proper4KUI Patch.

## [0.13.3] - 2026-06-06

### Added
- **Bespoke banners** for the **ER.11/12 role-picker menus**: five male court figures
  for the male picker (ER.11/13), five female court figures for the female picker
  (ER.12/14), each holding a tool of their council office (chancellor scroll,
  marshal sword, steward ledger, spymaster dagger-cloak, chaplain holy book).
  Same painterly parchment-scroll composition as the ER.10 gender-picker banner.
  Page 2 (ER.13/14) reuses page 1's banner -- the picture conveys "your candidate
  pool," consistent across both pages. 450x150 base, 810x270 in the Proper4KUI
  Patch.

## [0.13.2] - 2026-06-06

### Added
- **Bespoke banner** for **ER.10's gender-picker menu**: two court figures painted on
  a parchment scroll, with a gloved hand reaching in between them. Replaces the
  placeholder `GFX_evt_emissary` borrowed from vanilla since 0.13.0. 450x150 base,
  810x270 in the Proper4KUI Patch (same file-overlay pattern as the decision icon).

## [0.13.1] - 2026-06-05

### Added
- **Bespoke decision icon** for **Recruit an Elite Specialist** — a painted red wax seal
  impressed with a laurel wreath on aged parchment, in the warm-sepia painterly style of
  vanilla CK2 decision icons. Replaces the borrowed `icon_diplomacy.dds` attribute icon
  used since 0.13.0.
- **New optional companion mod, Elite Recruitment - Proper4KUI Patch**, which overrides
  the base mod's decision icon with a 50×50 hi-res version pixel-flush with Proper4KUI's
  icon standard. Declares `dependencies = { "Elite Recruitment" "Proper4KUI" }` so it
  silent-no-loads on installs lacking either, and its later load order ensures its DDS
  wins via the engine's mod-stack file resolution when both are loaded. Same file-overlay
  pattern Proper4KUI itself uses to override vanilla icons. The patch will hold further
  hi-res asset overrides as the `0.13.x` line adds them (e.g. the planned bespoke menu
  event banners).

### Changed
- The base mod's `GFX_recruit_elite_specialist` sprite now points at
  `gfx/interface/elite_recruitment_specialist_decision.dds` — a 28×28 uncompressed BGRA
  DDS shipped in the mod, matching vanilla's native decision-icon size. CK2 renders
  decision icons at the texture's native pixel size; pure-vanilla players see pixel-flush
  parity. (Proper4KUI players: install the optional patch sub-mod for hi-res parity.)

### Fixed
- **CleanSlate load-order interaction.** This mod now declares
  `dependencies = { "CleanSlate" }` in its `.mod` file so the launcher loads CleanSlate
  first when both are present. Without that, CleanSlate's `replace_path` claims on shared
  folders (`decisions/`, `events/`, `localisation/`, …) silently blocked this mod's
  content unless another mod with a CleanSlate dependency (e.g. Proper4KUI) was also
  loaded. The dependency is not a hard requirement; without CleanSlate installed it's
  silently ignored.

## [0.13.0] - 2026-06-05

### Changed
- **Ten recruitment decisions collapsed into one.** The Decisions tab now shows a single
  **Recruit an Elite Specialist** decision; clicking it opens a short menu where you choose
  the recruit's sex (where your laws and faith permit a choice) and then their council
  role. The male/female gating, per-role costs, and post-arrival appointment pop-up
  (ER.1) are unchanged -- this is a UX refactor of the entry point, not a balance change.
- **Auto-skip the gender picker** when only one sex has any eligible role, so a
  patriarchal-faith realm (the majority case) goes straight from the decision to the role
  menu -- same click count as the previous ten-decision flow.
- **Roles list in council order** on the role picker: Chancellor, Marshal, Steward,
  Spymaster, Chaplain -- matching the order the player sees in the council UI.
- **Dynamic council titles in EN role-option labels.** Each option uses the player's
  actual council title via `[Root.GetChancellorName]`, `[Root.GetMarshalName]`,
  `[Root.GetTreasurerName]`, `[Root.GetSpymasterName]`, `[Root.GetLordSpiritualName]` --
  so a Tibetan-Buddhist ruler sees "Chief Minister"/"Treasurer"/"Chief Diviner" exactly
  as their council UI does, while a Chinese-culture ruler sees "Xingjun Sima" for the
  marshal seat. FR/DE/ES keep their existing static gendered translations.
- **`Recruit an Elite Specialist` icon** is the **Diplomacy** attribute's sealed-scroll
  icon, fitting for the menu's "letters out, candidates come to court" framing. (Bespoke
  art planned.)

### Added
- **Adaptive role-picker pagination.** When all five recruit roles would be visible at once
  (every sex-gate and affordability check passes), the picker paginates: page 1 shows
  Chancellor/Marshal/Steward plus an "Or perhaps another..." button leading to page 2,
  which holds Spymaster and Chaplain. Otherwise (the common case) all visible roles show
  on a single page with no nav button. CK2's event window hard-caps at four visible
  options, so this shape is required to fit a five-role menu without clipping.
- **Five new menu events:** **ER.10** (gender picker), **ER.11**/**ER.12** (role picker
  page 1, per sex), **ER.13**/**ER.14** (role picker page 2, only fired when all five
  roles are simultaneously visible). Each role option pays its per-role cost, creates the
  recruit, and fires the existing ER.1 arrival pop-up.
- **Decision hover-tooltip flavor** (`recruit_elite_specialist_TT`) renders the player's
  titled name in yellow: *"The court of §Y[Root.GetTitledName]§! sends word in search of
  an elite specialist."*
- **Five new shared scripted triggers** powering the menu branching:
  `elite_recruitment_enabled_trigger` (rule not Disabled),
  `elite_recruitment_any_<male|female>_role_eligible_trigger` (gender-step auto-skip),
  `elite_recruitment_all_5_<male|female>_visible_trigger` (pagination control).
- **26 new localisation rows in EN/FR/DE/ES**: decision name + desc + hover tooltip; the
  three event descs (gender picker + two role-picker pages); per-sex per-role option
  names; gender-picker cancel; pagination "Or perhaps another..." (per sex) and "Let me
  reconsider the others." back buttons (per sex).

### Removed
- The ten old per-sex-per-role decisions
  (`recruit_<male|female>_elite_<marshal|steward|chancellor|spymaster|chaplain>`). Their
  loc rows (20 keys) are deleted; the role-specific tooltip strings (`recruit_<role>_TT`)
  remain because the new menu options still call them.
- Five per-role `*s_enabled_trigger` scripted triggers, replaced by the single shared
  `elite_recruitment_enabled_trigger`.
- The ten `GFX_recruit_<male|female>_elite_<role>` sprite registrations from
  `interface/elite_recruitment.gfx`, replaced by one `GFX_recruit_elite_specialist`.

## [0.12.9] - 2026-06-03

### Changed
- **New preview/poster art** (`elite_recruitment.jpg`) — a bolder recruitment-poster design
  (large pointing king, "ELITE RECRUITMENT" + "YOUR REALM NEEDS YOU!") that stays legible at
  Steam's 268×268 thumbnail size.

## [0.12.8] - 2026-06-03

### Added
- **README "Development" section** documenting the encoding/EOL workflow — why the script and
  localisation files are UTF-8 in git but Windows-1252 + CRLF on disk, and how to edit them
  without corrupting accents.

### Removed
- **The non-standard `git-encoding=UTF-8` token** from `.gitattributes`. `working-tree-encoding`
  already stores the blob as UTF-8, so the token was redundant and ignored by git.

## [0.12.7] - 2026-06-03

### Changed
- **README intro updated to "Recruit Elite Male/Female Steward,"** matching the 0.12.6
  decision rename (it still listed the old "Stewardess" name).
- **README "The recruits" now notes the marshal battle-wound flavor** from 0.12.4, and the
  Reaper's Due line broadens the stale "One-Eyed" mention to a lost eye or disfiguring wound.

## [0.12.6] - 2026-06-03

### Changed
- **Steward decisions now read "Recruit Elite Male/Female Steward"** (dropping "Stewardess"),
  matching the explicit-gender naming the other four roles already use in every language
  (French *masculin/féminine*, German *(männlich)/(weiblich)*, Spanish *masculino/femenina*).
- **Decision descriptions are gender-neutral across all five roles** — the recruit's sex is
  stated in the decision name, so the descriptions don't repeat it (dropped "male/female" from
  the Marshal and Steward descriptions; the other languages gender the role noun naturally).

## [0.12.5] - 2026-06-03

### Fixed
- **The Chancellor recruit no longer keeps a speech impediment.** Stripped `harelip`,
  `lisp`, and `stutter` from the elite Chancellor — fitting for the realm's silver-tongued
  envoy (the Court Chaplain, also a speaker, already strips them).

## [0.12.4] - 2026-06-03

### Added
- **Marshal "battle wounds" flavor.** A marshal recruit now has a small chance to bear the
  marks of past battles — a hardened battle scar (its severity scaling with Holy Fury), or,
  with The Reaper's Due, a lost eye or a disfiguring wound. The penalty wounds come paired
  with a fitting trait (hard-won patience, or a battle-born temper) so the roll is always
  worthwhile.
- **A few new flavor traits.** Marshals can roll **Brave**; spymasters can roll **Envious**.

## [0.12.3] - 2026-06-03

### Fixed
- **Elite recruits no longer keep `inbred`.** The marshal, steward, and chancellor now strip
  `inbred` (−5 to every attribute), matching the spymaster and chaplain — no elite recruit
  should carry the game's worst congenital trait.
- **Marshals strip a generated `holy_warrior`.** It is a leadership trait (`leader = yes`),
  so it was added to the pre-roll leadership wipe; otherwise it could occupy a command slot
  and crowd out the curated leadership trait or its Chinese/lodge bonus.

## [0.12.2] - 2026-06-02

### Changed
- **Reorganized the scripted-triggers file (no gameplay change).** Grouped the two shared
  eligibility triggers together, then co-located each role's enabled gate with its
  affordability check under a per-role header, and normalized the file banner. Reordering
  and comments only — every trigger body is unchanged.

## [0.12.1] - 2026-06-02

### Changed
- **Code-comment cleanup (no gameplay change).** Standardized the scripted-effect comments
  across the five council files to a consistent role-mindset voice — dropped the
  vanilla-comparison annotations, unified the "flavor" roll headers — and corrected the
  Chancellor effect file's banner, which still read "Diplomat." Comments and a banner only;
  no behavior, balance, or localisation change.

## [0.12.0] - 2026-06-02

### Added
- **Mod-detection flag.** Elite Recruitment now sets the global flag
  `elite_recruitment_active` at game start (new games and save loads), so other mods can
  detect that it is loaded — the same courtesy CleanSlate extends with `cleanslate_active`,
  which Elite Recruitment reads for trait-name compatibility.
- **README "For modders" section** documenting the new detection flag and the per-recruit
  `elite_recruit_<role>` character flags.

## [0.11.0] - 2026-06-02

### Added
- **Council-appointment arrival event.** When an elite recruit arrives, a mod-owned pop-up
  now offers to seat them in their council role on the spot — Marshal, Chancellor, Steward,
  Spymaster, or Court Chaplain — dismissing the current officeholder first if the seat is
  filled (the appoint button warns when it will replace a sitting officeholder). A Marshal can
  also be granted an army command, and a Court Chaplain can be made court physician (The
  Reaper's Due) or court tutor (Conclave) when suited. Replaces the borrowed vanilla `NE.1`
  notification with the mod's own `ER.1` event.

### Changed
- **The martial recruit is now the Marshal and the diplomacy recruit is now the Chancellor.**
  Both were renamed to match their council seat (joining the Steward, Spymaster, and Chaplain)
  across decisions, scripted effects, triggers, GFX sprite names, and localisation (EN/FR/DE/ES);
  the scripted-effects files are now `elite_recruitment_marshal_effects.txt` and
  `elite_recruitment_chancellor_effects.txt`. Decision descriptions keep "commander" and
  "diplomat" as trade descriptors.
- **Per-recruit marker flags standardized to `elite_recruit_<role>`** (were `invited_soldier`,
  `invited_diplomat`, `invited_steward`, `invited_spymaster`, `invited_chaplain`); the
  `elite_recruit_clergy` caste flag is folded into `elite_recruit_chaplain`. The shared
  `save_event_target_as = invited_character` target the arrival event reads is unchanged.
- **Marshal and Steward decisions now use their council-attribute icon** (Martial,
  Stewardship), matching the Chancellor, Spymaster, and Chaplain — replacing the old "Employ
  Soldier" / "Employ Steward" decision icons.

### Fixed
- **Male Marshal no longer over-gated by succession law.** Dropped the `enatic_succession` /
  `enatic_cognatic_succession` exclusions from the male decision — succession law governs
  title inheritance, not the marshal seat (vanilla `can_be_marshal_trigger` has no such clause).
  The `NOT matriarchal` gate is kept.
- **Marshal tooltip is now mod-owned** — replaced the borrowed vanilla `promoted_commander_TT`
  ("A soldier appears…") with `recruit_marshal_TT` ("A marshal appears…").

## [0.10.0] - 2026-06-01

### Changed
- **Cost rules consolidated into one sliding-scale tier.** The four old rules (the On/Off
  toggle plus the x1–x8 Gold/Prestige/Piety multipliers) are replaced by a single
  **Elite Recruitment** rule with named tiers — **Pittance · Trifle · Modicum (default) ·
  Bounty · Fortune · Disabled**. Tiers scale every cost together (×0.25 / ×0.5 / ×1 / ×2 / ×4);
  **Disabled** absorbs the old On/Off toggle. Options are ordered so Disabled sits three steps
  either way from Modicum.
- **Costs are now tuned per council role** rather than one flat multiplier: the Steward leans
  heaviest on gold with only a token Prestige, the Spymaster more gold than Prestige, the
  Commander balanced, the Diplomat more Prestige than gold, and the Chaplain mostly Piety with
  a little gold.
- **Aggressive gold floors.** Gold is still a fraction of yearly income, but the minimum floor
  is raised substantially (e.g. 50 gold for a Modicum steward vs. the old ~4), so the price
  stays meaningful for a low-income ruler sitting on a hoard — the floor, not the income
  fraction, binds for most small and mid realms.
- **Decision descriptions** brought in line with the new costs and each other: every recruit
  now leads with "brilliant" plus its specialty, and lists the currency it costs most first
  (the Steward and Spymaster now lead with wealth, and the Steward notes its Prestige cost).
  Dropped the misleading "shieldmaiden" and universal "legendary" flavor from the commanders.

### Added
- Five per-role cost effects (`pay_elite_commander_cost` … `pay_elite_chaplain_cost`) and
  matching `elite_recruitment_*_can_afford_trigger` triggers, replacing the per-currency plumbing.
- Localisation for the new tier names and descriptions (EN/FR/DE/ES).

### Removed
- The `elite_recruitment` On/Off rule (its key is repurposed as the tier selector) and the
  `elite_recruitment_gold_cost` / `_prestige_cost` / `_piety_cost` multiplier rules, along with
  their `pay_elite_gold_cost` / `_prestige_cost` / `_piety_cost` effects and
  `elite_recruitment_can_afford_gold` / `_prestige` / `_piety` triggers.

## [0.9.0] - 2026-06-01

### Added
- **Elite court chaplain recruitment:** **Recruit Elite Male Chaplain** / **Recruit Elite
  Female Chaplain** decisions — **gold + Piety** cost, a guaranteed **Mastermind Theologian**
  education and a large **Learning** boost, plus a learning/piety flavor roll (Theologian,
  Mystic, Scholar/Faqih, Erudite, Shrewd, Genius/Quick, and pious touches — Zealous, Humble,
  Diligent, or a Chaste vow that may deepen into Celibate).
  Built on `create_random_priest`; grants the priestly caste `brahmin` (not `kshatriya`) for
  dharmic faiths, satisfying `can_be_spiritual`. Icon: the Learning attribute (an open book).
- **Wonder-inspired chaplains:** if the ruler owns a wonder/upgrade flagged
  `inspires_learning` (a great library, House of Wisdom, …), the chaplain gains an extra
  learning boon (`erudite`, `physician`, `genius`, `poet`, or a learning bump) — the
  scholarly counterpart to the commander's inspire-commanders legendary bonus.
- **Piety-cost infrastructure:** new `elite_recruitment_piety_cost` game rule (x1/x2/x4/x6/x8,
  default x4), `pay_elite_piety_cost` effect, and `elite_recruitment_can_afford_piety_trigger`,
  mirroring the prestige plumbing (x1 = 25 Piety … x4 = 100). Only the chaplain costs Piety.
- Localisation for the chaplain strings and the Piety-cost rule (EN/FR/DE/ES).

### Changed
- **Chaplain trait curation:** strips incompetence (incl. `inbred`, −5 to all), the
  spawn-scholar physical set plus speech impediments (`harelip`/`lisp`/`stutter` — a chaplain
  preaches), and traits unbecoming of a holy man (`cynical`, `deceitful`, `lustful`, `proud`,
  `wroth`, `envious`); keeps `gluttonous` and `greedy` as worldly-cleric flavor (the well-fed
  friar, the coffer-filling prelate).
- **Female chaplain gate is faith-based** (`religion_allows_female_temple_holders` or a
  matriarchal faith), not law-based — it appears only for faiths that ordain women.
- The shared `assign_religion_traits` helper is now caste-aware — `brahmin` for a clergy
  recruit (flagged `elite_recruit_clergy`, i.e. the chaplain), `kshatriya` otherwise — so the
  chaplain reuses it instead of a near-duplicate effect. Unchanged for the other four recruits.

### Fixed
- Stale cost-rule comments: the gold/prestige rule and effect comments now list all paying
  roles (the spymaster's Prestige use, added in 0.8.0, was missing).

## [0.8.0] - 2026-06-01

### Added
- **Elite spymaster recruitment:** **Recruit Elite Male Spymaster** / **Recruit Elite Female
  Spymaster** decisions — gold + Prestige cost, a guaranteed **Elusive Shadow** education and
  a large **Intrigue** boost, plus an intrigue-themed flavor roll (Deceitful, Schemer,
  Cynical, Cruel, Paranoid, a charming/seductive option, Impaler, ...). Built on
  `create_random_intriguer`. The Female Spymaster uses vanilla's fuller `can_be_spymaster_trigger`
  eligibility — pagan-group realms and the Cathar/Messalian faiths allow it without
  status-of-women laws.
- Localisation for the spymaster strings (EN/FR/DE/ES).
- Two CleanSlate-compat hooks for the spymaster's lifestyle flavor traits: Schemer / Master
  Schemer, and Seducer/Seductress / Master Seducer/Master Seductress (with a sex branch).

### Changed
- **Spymaster trait curation** inverts the diplomat's: `paranoid` is **kept** (+2 intrigue),
  while conspicuous traits (giant, dwarf, hunchback, clubfooted, harelip, lisp, stutter, ugly,
  inbred) and anti-spy personality (`honest`, which is −2 intrigue, and `trusting`) are
  stripped — an elite spy should blend in.
- **All elite recruits now strip `slothful`** (−1 to all attributes); previously only the
  steward did.
- **Jain gating for Hunter / Impaler** (both carry `NOT religion = jain`): the commander's
  Hunter flavor roll and the spymaster's Impaler roll are excluded for Jain rulers (no wasted
  roll), and the legendary "the Hunter" build is withheld from Jain — while "the Giant" build
  keeps its other traits and merely drops the incidental Hunter.

## [0.7.0] - 2026-05-31

### Added
- **Elite diplomat recruitment:** **Recruit Elite Male Diplomat** / **Recruit Elite Female
  Diplomat** decisions — gold + Prestige cost, a guaranteed **Grey Eminence** education and a
  large **Diplomacy** boost, plus a diplomacy-themed flavor roll (a charismatic presence,
  Gregarious, Socializer, Kind, Honest, and the like). Modeled on vanilla's
  `spawn_fantastic_diplomat_effect`.
- **Holy-war veteran flavor for commanders:** a recruit has a small chance to gain the
  crusader-family trait matching their faith (Crusader, Mujahid, reformed-pagan
  equivalents, ...) via the cross-build `add_crusade_trait_effect`. Dharmic faiths have no
  such trait and are excluded.
- Localisation for the diplomat strings (EN/FR/DE/ES).

### Changed
- **Commander negative-trait removal** no longer strips **Giant** (a net positive for a
  commander — +combat rating and +vassal/tribal opinion) or **Paranoid** (no martial
  impact), so a naturally-rolled recruit keeps them. The legendary builds that add Giant are
  unaffected (Giant/Dwarf are opposites and auto-resolve).
- The **Prestige Cost** game rule now also applies to diplomats (stewards remain
  Prestige-free).

### Fixed
- **Female steward gate** tightened to full gender equality (`status_of_women_4`, or an
  equal/matriarchal faith). A brought-in courtier is unrelated and unlanded, and vanilla
  won't seat such a woman on the council below that threshold — so the prior
  `status_of_women_2/3` options could offer a Stewardess who couldn't actually take the
  post. The new Female Diplomat uses the same corrected gate.

## [0.6.0] - 2026-05-31

### Changed
- CleanSlate is now detected automatically via the `cleanslate_active` global flag that
  CleanSlate sets at startup (merged upstream), rather than a flag set by a separate patch
  mod. When the flag is present the mod uses CleanSlate's renamed traits; on vanilla — or an
  older CleanSlate without the flag — it uses the vanilla traits, exactly as before.

### Removed
- The **Elite Recruitment — CleanSlate Patch** companion mod, no longer needed now that
  CleanSlate sets its own startup flag. CleanSlate users should run a current version, as the
  flag is a recent addition.

## [0.5.0] - 2026-05-31

### Added
- Commander leadership roll rebuilt and grouped by the wiki's categories — General,
  Unit-specific (incl. **War Elephant** for Indian-culture realms), and **Terrain Expertise**.
- **Chinese commander traits (Jade Dragon):** a Chinese-culture recruit has a ~50% chance to
  also gain a "Way of the..." leadership trait, respecting the engine's two-trait cap.
- **Warrior-lodge sponsorship (Holy Fury):** a recruit of a warrior-lodge ruler is inducted
  into that lodge, with a ~25% chance of having earned its command trait.
- **Elite Recruitment — CleanSlate Patch** companion mod (same repo): an `on_startup` action
  sets a global flag so CleanSlate installs use CleanSlate's renamed traits. It only *adds* an
  action (no file override), so it can't conflict with CleanSlate or other mods.

### Fixed
- **Vanilla-safety:** several traits the mod used were CleanSlate renames that don't exist in
  vanilla CK2, so they silently no-opped there. Every trait the mod adds/removes now resolves
  on both builds. Build-renamed traits (physique Robust/Brawny, beauty Fair/Attractive,
  Battlefield Terrain Master, Direct Leader, terrain experts) are routed through hooks in
  `common/scripted_effects/elite_recruitment_trait_compat.txt`, switched by the patch's flag.
- **Double leadership trait:** the generated soldier could arrive with its own leadership trait
  and the mod added another, filling the two-trait cap and blocking the Chinese/lodge bonus.
  Any generated leadership trait is now cleared before the curated roll.

### Changed
- Commander and steward trait assignment refactored into clean orchestrators that call per-step
  helpers (negative-trait removal, education, leadership, flavor, compatibility).

## [0.4.0] - 2026-05-31

### Added
- Game rule **Elite Recruitment: Gold Cost** (*1x* / *2x* / *4x* / *6x* / *8x*, default
  *4x*) scaling the gold price of all recruits — commanders and stewards. Each tier carries
  a min gold floor equal to its multiplier (1–8 gold), so the cost stays meaningful even for
  low-income realms.
- Game rule **Elite Recruitment: Prestige Cost** (*1x* / *2x* / *4x* / *6x* / *8x*, default
  *4x*) scaling the Prestige price of commander recruits (stewards stay Prestige-free).

### Changed
- Recruit costs now flow through centralized `pay_elite_gold_cost` /
  `pay_elite_prestige_cost` effects and `elite_recruitment_can_afford_gold_trigger` /
  `elite_recruitment_can_afford_prestige_trigger` triggers, branched by the new rules.
  Default costs are raised from the prior baseline (gold ~4x; commander Prestige 100).

## [0.3.0] - 2026-05-31

### Added
- Game rule **Elite Recruitment** (*On* / *Off*) to enable or disable all recruitment
  decisions at once. Defaults to *On*, so the mod works without ever opening the rules
  screen.
- Mod tags **Cheats** and **QoL**.

## [0.2.0] - 2026-05-31

### Added
- Elite steward recruitment: **Recruit Elite Steward** / **Recruit Elite Stewardess**
  decisions — gold-only cost, guaranteed Midas Touched plus a large Stewardship boost.
- Localisation for the steward strings (EN/FR/DE/ES).

### Fixed
- Female commander decision required `status_of_women_3`, but women cannot command until
  `status_of_women_4`; corrected the gate.

## [0.1.0] - 2026-05-31

### Added
- Elite commander recruitment: **Recruit Elite Male Commander** /
  **Recruit Elite Female Commander** decisions — Prestige plus scaled gold cost, a
  guaranteed Brilliant Strategist education, a battlefield-leadership trait, and weighted
  flavor; rare legendary-commander result when the ruler carries an *inspire commanders*
  bloodline.
- Localisation for the commander strings (EN/FR/DE/ES).
