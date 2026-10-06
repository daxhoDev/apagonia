← Back to [MAIN](MAIN.md)

# Mobile app

- **Module:** Mobile app
- **Module code:** MOB
- **Version:** v0.2
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-MOB-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

Concerns that apply to the whole mobile client: supported platform, distribution and updates, UI language and display formats. Feature behaviour is specified in each feature module.

## Decided so far

- **Android only.**
- Distribution by **direct APK download** from **our own server**.
- The APK is built with **EAS Build**.
- **Updates:** the app checks the API for a newer version and offers a download link to the new APK. Updates are **optional** by default; if the API marks the installed version as **no longer compatible**, the app **blocks use until it is updated** (mandatory update). Over-the-air (OTA) updates may be added later.
- **Expo SDK:** the latest stable version at implementation start. **Minimum Android version:** the minimum supported by that SDK.
- **Getting updates while open:** the app fetches the latest data from the API when it is **opened** (or brought back to the foreground) and then about **every 1 minute while it is visible**. It does not poll while in the background; there is no SSE or WebSocket ([ARCHITECTURE.md](ARCHITECTURE.md)). When the app is closed, users are reached by push notifications ([NOTIFICATIONS.md](NOTIFICATIONS.md)).
- **Client stack:** navigation, data fetching and local storage libraries are listed in [CONVENTIONS.md](CONVENTIONS.md) §Stack.
- App UI language: **Spanish**. Docs, code and identifiers remain English (`AGENTS.md` §8).
- Times are shown in **Cuba local time** ([LISTENER.md](LISTENER.md)).
- **Formats** (Cuban conventions): dates `d/m/yyyy`; times in 12-hour format like the channel (`7:27 PM`); durations `H:MM` with hours that may exceed 24 (e.g. `27:50 h`).

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

None so far.
