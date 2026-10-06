← Back to [MAIN](MAIN.md)

# Plans

- **Module:** Plans
- **Module code:** PLAN
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-PLAN-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

Free and paid plans: what each includes, subscription limits, manual payments, and activation and expiry of the paid plan.

## Decided so far

- **Freemium:** a free plan with many features; specialised features and extended limits are paid. Which features are paid is decided once the full system is defined.
- **Free plan:** follows **1 circuit**. **Paid plan:** **unlimited** circuits ([SUBSCRIPTIONS.md](SUBSCRIPTIONS.md)). Guests are on the free plan ([ACCOUNTS.md](ACCOUNTS.md)).
- The paid plan requires a **registered account**.
- **Monthly plan:** the paid plan expires **30 days** after activation and must be renewed.
- **Expiry warning:** the user is warned by a **notification** and an **in-app notice** **3 days before** expiry and **on the expiry day**.
- **Price:** to be decided; it is configurable.
- **Payments are manual:** the user pays by Transfermóvil or EnZona transfer and sends the proof of payment **outside the app** (e.g. Telegram/WhatsApp to the administrator), stating the account email. The **administrator** (the user) activates the paid plan through a **server command**.
- **Expiry with several circuits followed:** when the paid plan expires and the user follows more circuits than the free plan allows, **all** their subscriptions are **paused** until the user picks one circuit (free limit) or renews.
- **Paid features so far:** unlimited circuits, the home-screen widget ([WIDGET.md](WIDGET.md)) and the prediction ([HISTORY.md](HISTORY.md)).
- **Later phase:** redeemable single-use codes.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Other paid features and price.** Which other features and limits are paid (e.g. statistics), and the price (decided after the full system is defined).
