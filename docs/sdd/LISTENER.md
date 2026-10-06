← Back to [MAIN](MAIN.md)

# Listener

- **Module:** Listener
- **Module code:** LSN
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-LSN-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

Real-time ingestion of the Telegram channel of the Empresa Eléctrica de Holguín, which constantly publishes which circuits are affected by blackouts and how many hours of outage each has accumulated, and parsing of its messages into circuit states for [CIRCUITS.md](CIRCUITS.md).

## Decided so far

- Telethon connects to Telegram with the user's **personal** Telegram account.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Message format and parsing.** Real sample messages from the channel are needed to define parsing. The user will provide them.
2. **Channel identity.** The channel's name and link.
3. **Non-status messages.** The channel may publish messages that are not circuit status lists (announcements, schedules, partial updates). The rule "circuits not listed have power" ([CIRCUITS.md](CIRCUITS.md)) is only safe for full status messages. How such messages are recognised and handled.
4. **Listener downtime and catch-up.** What happens if the listener or Telegram connection is down: whether missed messages are fetched on reconnect (and whether notifications are sent for them), and how the current state is initialised on first start.
5. **Personal account operation.** Using a personal Telegram account carries account-restriction and security considerations: how the Telegram session and API credentials are stored and protected, and how the initial login (code / 2FA) is performed on the server.
