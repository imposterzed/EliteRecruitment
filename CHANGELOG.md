# Changelog

All notable changes to **Elite Recruitment** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
Versions remain in the `0.x` range (in development) until the first Steam Workshop
release, which will be tagged `1.0.0`.

## [Unreleased]

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
