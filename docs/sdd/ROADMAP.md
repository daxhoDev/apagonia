← Back to [MAIN](MAIN.md)

# Roadmap

- **Module:** Roadmap
- **Module code:** ROAD
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-ROAD-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

The project plan: the order in which the system is built, from the MVP (which includes the push validation, Phase 0) to the full implementation. Descriptive module: it orders the phases of the other modules.

## Decided so far

- **MVP:** before the full implementation, an MVP with the most elemental features is built. Scope:
  - Listener, parser and circuit state ([LISTENER.md](LISTENER.md), [CIRCUITS.md](CIRCUITS.md)).
  - App: browse and search circuits and see their state ([MOBILE.md](MOBILE.md), [CIRCUITS.md](CIRCUITS.md)).
  - Accounts: basic registration and login only; email verification and password recovery are **not** in the MVP ([ACCOUNTS.md](ACCOUNTS.md)).
  - Subscribing to circuits ([SUBSCRIPTIONS.md](SUBSCRIPTIONS.md)).
  - Push notifications "power goes out" and "power returns" ([NOTIFICATIONS.md](NOTIFICATIONS.md)).
- Anything not listed above comes after the MVP.
- The MVP does **not** enforce the free-plan limit of 1 circuit: every user can follow any number of circuits until plans exist ([PLANS.md](PLANS.md)).
- **Phase 0 (push validation) is merged into the MVP:** Expo Push is validated with the MVP on real devices in Holguín without VPN, with the app closed; pass if most notifications arrive, no timing thresholds. If it fails, plan B: a self-hosted persistent connection ([NOTIFICATIONS.md](NOTIFICATIONS.md)).

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

None.

## Open points

1. **Phase order.** The order of the other modules' implementation phases after the MVP.
