← Back to [MAIN](MAIN.md)

# Subscriptions

- **Module:** Subscriptions
- **Module code:** SUBS
- **Version:** v0.2
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-SUBS-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

The link between users ([ACCOUNTS.md](ACCOUNTS.md)) and the circuits they follow ([CIRCUITS.md](CIRCUITS.md)). A user sees the outage time of each subscribed circuit.

## Decided so far

- A user subscribes to circuits and sees the outage time of each subscribed circuit according to the Telegram channel.
- **Limits** (supersedes "no subscription limit"): the **free plan follows 1 circuit**; the **paid plan** is **unlimited** ([PLANS.md](PLANS.md)). The MVP does not enforce the limit ([ROADMAP.md](ROADMAP.md)).
- When a paid plan expires and the user follows more circuits than the free plan allows, **all** subscriptions are **paused** until the user picks one circuit or renews ([PLANS.md](PLANS.md)).
- Guest subscriptions are stored only locally on the device and moved to the account on registration ([ACCOUNTS.md](ACCOUNTS.md)).

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

None so far.
