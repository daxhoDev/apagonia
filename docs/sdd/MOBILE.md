← Back to [MAIN](MAIN.md)

# Mobile app

- **Module:** Mobile app
- **Module code:** MOB
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-MOB-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

Concerns that apply to the whole mobile client: supported platform, distribution and updates, UI language and display formats. Feature behaviour is specified in each feature module.

## Decided so far

- **Android only.**
- Distribution by **direct APK download**.
- App UI language: **Spanish**. Docs, code and identifiers remain English (`AGENTS.md` §8).

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **APK build and updates.** How the APK is built (e.g. cloud versus local Expo builds), where it is downloaded from, and how users learn about and get new versions.
2. **Android support range.** The minimum Android version supported.
3. **Time zone and formats.** Time zone used for display (presumably Cuba's) and date/time and duration formats in the Spanish UI.
