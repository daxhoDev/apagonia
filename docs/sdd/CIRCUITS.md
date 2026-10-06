← Back to [MAIN](MAIN.md)

# Circuits

- **Module:** Circuits
- **Module code:** CIRC
- **Version:** v0.2
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-CIRC-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

The circuit catalog and the current state of each circuit (affected by a blackout or with power, and accumulated outage hours), as derived from the channel messages, and how users see it. The stored history, statistics and prediction are in [HISTORY.md](HISTORY.md). Also the provincial situation and the daily situation reported in the channel.

## Decided so far

### Catalog and state

- The circuit catalog is **learned from messages**: a circuit exists once it appears in the channel.
- A circuit listed in a message is affected by a blackout, with the accumulated outage hours stated in that message.
- A circuit not listed in a message has power at the time of that message.
- Circuits and lines (e.g. `Banes-Antilla`) are treated identically.
- The timestamp of the state is the date and time in the message **header**; the Telegram publish time is the fallback when the header date/time cannot be read ([LISTENER.md](LISTENER.md)). All times are **Cuba local time**.
- A message the listener discards (any line not understood, or no circuit lines) leaves the current state untouched ([LISTENER.md](LISTENER.md)).
- A status message whose timestamp is older than the current state is ignored ([LISTENER.md](LISTENER.md)).
- If the channel **edits** the current status message, it is re-parsed and only the resulting changes are notified; editing an older one only corrects the history. If the channel **deletes** the current status message, the state reverts to that of the previous applied status message, with notifications for the resulting changes ([LISTENER.md](LISTENER.md)).

### State details

- A circuit's state also holds the **cause** when one is reported (any text after the duration, e.g. `Avería`, `Emergencia`). The cause is **shown in the app** and in notifications ([NOTIFICATIONS.md](NOTIFICATIONS.md)).
- A circuit listed through a parenthesised part (e.g. `Cto Banes 2 (Ramal Tanganica)`, `Cto 6 de Moa(Rolo)`) is **affected**; the **ramal / parenthesised text is shown as a detail** of the circuit's state. When a circuit appears several times in one message, it takes the longest duration and all details are kept ([LISTENER.md](LISTENER.md)).
- An entry that names several circuits (e.g. `Zarzal 1 y 2`) affects each of them (`Zarzal 1`, `Zarzal 2`) with the same hours and cause.

### Outage hours and freshness in the app

- While the app is open, the circuit states, the provincial situation and the daily situation are refreshed by the app's periodic polling ([MOBILE.md](MOBILE.md)).

- The app shows an affected circuit's outage hours as the **reported hours plus the time elapsed since the header time** of the last status message, together with the **time of the last status message**.
- After **2 hours** without an **applied** status message (discarded or ignored messages do not count), the app marks the state as **possibly outdated**.

### Circuit identity and naming

- Circuits are identified by a **normalised name**: the `Cto` prefix, spacing, separators/punctuation, letter case and accents are ignored. For example, `Cto Baguanos 3`, `Baguanos 3` and `baguanos-3` are the same circuit.
- A circuit is **displayed with its name as first published** in the channel (e.g. `Cto Uñas 1`). A circuit whose name is only a number is displayed with the `Cto` prefix (e.g. `Cto 17`).
- **Name variants that normalisation cannot unify** (e.g. `A. Pino` / `Alcides Pino`, `Baguanos` / `Báguanos` / `Báguano`, `Aereopuerto` / `Aeropuerto`): the system **suggests merges** of similar names; the **administrator** (the user) **confirms** a merge through a server command/script. Until confirmed, the new name is a separate circuit.
- **Merges:** a server command lists the pending merge suggestions. Confirming a merge makes the new name an **alias** of the existing circuit, merges their history ([HISTORY.md](HISTORY.md)) and subscriptions ([SUBSCRIPTIONS.md](SUBSCRIPTIONS.md)), and keeps the **original display name**.
- Circuit search in the app uses the same normalisation as identity.
- **Listing order:** affected circuits first, by outage hours (longest first); then circuits with power, alphabetically by display name.

### Provincial situation

- The app **shows the provincial situation** from the latest status message: served demand (MW), maximum outage time and cut-off time, plus the MW served to "el Níquel" as an **extra line** when the message reports it ([LISTENER.md](LISTENER.md)).

### Daily situation

- A **"Situación del día"** section in the app shows the latest daily situation message ("Nota informativa" about the SEN, or the provincial daily situation message, [LISTENER.md](LISTENER.md)) as **raw text with its time**. Each new one replaces the previous one. It triggers no notification.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

None so far.
