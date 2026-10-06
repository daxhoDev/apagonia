← Back to [MAIN](MAIN.md)

# Notifications

- **Module:** Notifications
- **Module code:** NOTIF
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-NOTIF-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

Notifications sent to users about their subscribed circuits ([SUBSCRIPTIONS.md](SUBSCRIPTIONS.md)): the notification types, the user's notification preferences, and how notifications are delivered to the mobile app.

## Decided so far

All three notification types exist:

1. **Power returns:** a circuit that was affected stops appearing in the messages.
2. **Power goes out:** a circuit that was not affected appears in a message.
3. **Every new message:** a notification for each new message, with the circuit's status and outage hours.

Notification preferences are configurable per user: a **global setting with per-circuit exceptions**.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Push notification channel.** The user chose to research options (Expo Push / FCM versus alternatives without Google services). The research has not been launched; the research question still needs the user's approval.
2. **"Every new message" notification details.** Whether it is one notification per message or per subscribed circuit; its content when a user has several subscribed circuits; whether notifications for several circuits are grouped.
3. **Notification preference model.** Exactly which settings exist (on/off per notification type? quiet hours?), their defaults for a new user and for a newly subscribed circuit.
