← Back to [MAIN](MAIN.md)

# Notifications

- **Module:** Notifications
- **Module code:** NOTIF
- **Version:** v0.2
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-NOTIF-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

Notifications sent to users about their subscribed circuits ([SUBSCRIPTIONS.md](SUBSCRIPTIONS.md)): the notification types, the user's notification preferences, and how notifications are delivered to the mobile app.

## Decided so far

### Notification types

All three notification types exist:

1. **Power returns:** a circuit that was affected stops appearing in the messages.
2. **Power goes out:** a circuit that was not affected appears in a message.
3. **Every status message:** **one grouped notification per user per status message**, listing the state of **all** the user's subscribed circuits (affected, with outage hours, or with power), plus **one extra line** with the provincial situation (served MW and maximum outage time).

When one status message triggers several of these for the same user (e.g. "every status message" ON and a subscribed circuit changes), the user gets **one combined notification** that **highlights the change** and **summarises the rest**.

### Notification content

- Wherever a notification states a circuit's status, it also shows the **cause** (e.g. `Avería`, `Emergencia`) and the **ramal** detail (e.g. `Ramal Tanganica`) when the message reports them ([CIRCUITS.md](CIRCUITS.md)).
- A circuit listed only through one of its ramals counts as affected for the "power goes out" and "power returns" notifications.
- A message the listener discards (any line not understood, [LISTENER.md](LISTENER.md)) triggers no notification. Daily situation messages trigger no notification ([CIRCUITS.md](CIRCUITS.md)).
- Edits and deletions of the current status message by the channel are notified only through the resulting changes ([LISTENER.md](LISTENER.md)).
- Messages imported on first start trigger no notification. After listener downtime, only the changes of the **most recent** missed status message generate notifications ([LISTENER.md](LISTENER.md)).

### Preferences

- Notification preferences are configurable per user: a **global setting with per-circuit exceptions**.
- Global preferences and quiet hours are **free**; **per-circuit exceptions** are **paid** ([PLANS.md](PLANS.md)). Free users (and guests) use only the global setting. When a paid plan expires, existing exceptions are kept but inactive until renewal ([PLANS.md](PLANS.md)).
- All three notification types are **free** ([PLANS.md](PLANS.md)).
- Defaults for a new user: **power goes out ON**, **power returns ON**, **every status message OFF**.
- A newly subscribed circuit has no exception, so it follows the user's global setting.
- **Quiet hours:** the user can set a time range during which no notifications are sent. Notifications that fall in it are dropped, not delayed.
- Only users with an account get push notifications; guests get none ([ACCOUNTS.md](ACCOUNTS.md)). Paused subscriptions ([PLANS.md](PLANS.md)) get no notifications. Paid-plan expiry warnings are also sent as notifications ([PLANS.md](PLANS.md)).

### Delivery

- Channel: **Expo Push** (through FCM), with an **in-app fallback**: devices without Google services get no push, but see the up-to-date state when they open the app, plus a notice that their phone does not support notifications.
- The delivery channel is **interchangeable**: the server-side notifier and the app talk to a pluggable delivery channel, so that a self-hosted persistent connection can replace FCM if needed.
- **Delivery failures:**
  - A **temporary** failure is retried up to **5 times** with increasing backoff (**10 s, 30 s, 1 min, 5 min, 15 min** after each failed attempt). If the last retry also fails, the notification is **abandoned** and **logged**.
  - A **permanent** failure reported by the push service (device unregistered, invalid push token) is **not retried**: that push token is **removed** from the user's devices.
- **Push validation (Phase 0, merged into the MVP):** FCM availability in Cuba is unconfirmed. Expo Push is validated with the MVP on real devices in Holguín, **without VPN**, with the **app closed** ([ROADMAP.md](ROADMAP.md)). Pass criterion (lax): **most** notifications arrive; no timing thresholds. If it fails, plan B: the delivery channel is replaced by a self-hosted persistent connection.
- **Plan B is a contingency:** it is designed **only if** the push validation fails. It blocks nothing else in this spec.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

None so far.
