← Back to [MAIN](MAIN.md)

# History

- **Module:** History
- **Module code:** HIST
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-HIST-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

The stored history of status messages and circuit state changes, its retention, and what is built from it: per-circuit history and statistics, and the prediction of the next state change. The current state is in [CIRCUITS.md](CIRCUITS.md).

## Decided so far

### Storage and retention

- **Supersedes "current state only":** every processed status message and every circuit state change is stored, retained for the last **3 months**. Older entries are removed.
- When the channel edits or deletes an already processed status message, the history is corrected or the entry removed ([LISTENER.md](LISTENER.md)).
- On first start the history is filled by importing the channel's past messages ([LISTENER.md](LISTENER.md)).

### Statistics

- **History and statistics per circuit**, built from the stored history (the 3-month retention window): **average** and **longest** outage duration, compared with the **provincial average**.
- The **per-circuit history** and the **per-circuit statistics** are **paid** features ([PLANS.md](PLANS.md)).

### Prediction

- **Prediction, level 2:** rotation cycles are detected from the history and the next change of a circuit is projected from them. No machine-learning model.
- Shown as an **estimate with a confidence level**, e.g. "probable return around 14:30, medium confidence", **only when a cycle is detected**; otherwise the app shows "no clear pattern".
- Prediction is a **paid** feature ([PLANS.md](PLANS.md)).

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Cycle detection and confidence (deferred).** How a rotation cycle is detected and how the confidence level (e.g. low/medium/high) is computed. Decided later **with real data**: once weeks of history are stored, candidate approaches (typical durations only, vs durations plus time-of-day slots) are compared on that data.
