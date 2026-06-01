# Changelog

All notable changes to **Elite Recruitment** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
Versions remain in the `0.x` range (in development) until the first Steam Workshop
release, which will be tagged `1.0.0`.

## [Unreleased]

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
