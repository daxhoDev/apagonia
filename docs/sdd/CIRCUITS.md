← Back to [MAIN](MAIN.md)

# Circuits

- **Module:** Circuits
- **Module code:** CIRC
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-CIRC-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

The circuit catalog and the current state of each circuit (affected by a blackout or with power, and accumulated outage hours), as derived from the channel messages, and how users see it.

## Decided so far

- The circuit catalog is **learned from messages**: a circuit exists once it appears in the channel.
- A circuit listed in a message is affected by a blackout, with the accumulated outage hours stated in that message.
- A circuit not listed in a message has power at the time of that message.
- Only the **current state** is stored and shown. No history is kept.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Message timestamp and staleness.** Whether the app shows the timestamp of the message the state comes from, and whether it warns when the data is stale.
2. **Circuit lifecycle edge cases.** The state of a newly learned circuit before its first appearance; handling of message edits and deletions in the channel.
3. **Outage hours between messages.** Whether the app shows the hours exactly as reported in the last message, or computes/increments them over time until the next message.
4. **Circuit identity and naming.** How circuit names/identifiers are normalised so the same circuit written differently is not learned twice; how circuits are displayed and searched in the app.
