# Contributing to Elite Recruitment

Thanks for your interest in contributing! This document covers bug
reports, feature requests, and code contributions.

- For what the mod does, see the [README](README.md).
- For the technical ramp-up (file layout, encoding rules, CK2 modding
  gotchas, commit conventions), see [AGENTS.md](AGENTS.md).

## Reporting bugs

Open a [new issue](../../issues/new/choose) and pick the **Bug report**
template. For mod-vs-mod conflicts, use the **Compatibility issue**
template.

For "how do I..." questions, please use
[Discussions](../../discussions) instead.

## Suggesting features

Open a [new issue](../../issues/new/choose) and pick the **Feature
request** template. Please describe the use-case — what does the player
gain that current Elite Recruitment doesn't already provide?

## Contributing code

Bug fix PRs are welcome. For new features, please open a
feature-request issue first to discuss the design before spending time
on a PR.

The technical reference (file layout, encoding, commit conventions,
graphics) lives in [AGENTS.md](AGENTS.md). The notes below cover only
the in-game testing parts that an automated agent can't do for you.

### Testing your changes in-game

There's no automated test suite — verification is in-game. The fastest
loop:

1. Copy or junction the mod into your CK2 mod directory
   (`Documents/Paradox Interactive/Crusader Kings II/mod/`).
2. Launch CK2 with Elite Recruitment enabled.
3. Check `logs/error.log` for any lines mentioning Elite Recruitment
   files — clean = parse OK.
4. Trigger the **Recruit an Elite Specialist** decision and walk the
   menu chain.

### Pick a ruler / set game rules that match the condition

Most religion / culture / government / DLC testing can skip cheats
entirely by setting up the new game correctly:

- **Female eligibility** — set the **gender game rule to "all"** at
  new-game setup, OR pick a matriarchal/equal-faith ruler (Norse pagan,
  Bön, Cathar with feminist features). The female chaplain is
  faith-gated separately — see the faith table.
- **Religion / culture / government type** — use the 1066 / 867 / 769
  / 936 / 1337 bookmark filters to find a ruler who already meets the
  condition. Examples:
  - Sunni `mujahid` marshal: any Sunni ruler.
  - Hindu kshatriya/brahmin caste: any Hindu ruler with Rajas of India.
  - Norse `valhalla_bound` + pagan branch + warrior lodge: any 769/867
    unreformed Norse pagan.
  - Han "Way of the…" trait: Han-culture ruler with Jade Dragon.
- **Wonder bonus (chaplain)** — the House of Wisdom sits in Baghdad.
  Play the Abbasid Caliph in 769/867.

Bloodlines, force-applied traits, and currency are the exceptions
where you'll fall back to cheats below.

### Console commands

Open the console with `~`.

| What | Command |
|---|---|
| Show character info on hover | `charinfo` |
| +5000 gold | `cash` |
| +Prestige / +Piety | `prestige 5000` / `piety 5000` |
| Hop to another ruler | `play <charID>` (hover with `charinfo` on) |
| Change religion | `religion <charID> <religion>` ([religion IDs](https://ck2.paradoxwikis.com/Religion#List_of_religions)) |
| Change culture | `culture <charID> <culture>` ([culture IDs](https://ck2.paradoxwikis.com/Cultures#List_of_cultures)) |
| Add a law | `add_law <law>` (e.g. `add_law status_of_women_4`; [law IDs](https://ck2.paradoxwikis.com/Laws)) |
| Add a trait | `add_trait <trait> <charID>` ([trait IDs](https://ck2.paradoxwikis.com/Traits)) |
| Join a society | `join_society <society_key>` (e.g. `join_society warrior_lodge_norse`) |
| Grant a bloodline | `event <event_id>` — fire one of the [HF bloodline events](https://ck2.paradoxwikis.com/HF_bloodline_events). For Elite Recruitment's legendary marshal path, you need a `legendary_commander_male`/`female` bloodline. |

For a UI alternative to typing trait / currency / society-rank
commands, install **SketchyCheatMenu** — a Workshop mod that adds an
in-game cheat panel. *Note: it doesn't grant bloodlines or join
societies — those still need the console commands above.*

### Pull request workflow

1. Fork the repo.
2. Create a branch from `main` for your change.
3. Make and test your changes (see Testing above).
4. Commit with the [convention from AGENTS.md](AGENTS.md#conventions)
   (imperative subject, optional bullet body, CHANGELOG entry).
5. Open a PR against `main` with a brief description of what changed
   and why.

## License

By contributing, you agree that your contributions are licensed under
the same **MIT License** that covers the rest of the project. No CLA
required.
