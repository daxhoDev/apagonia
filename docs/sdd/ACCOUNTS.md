← Back to [MAIN](MAIN.md)

# Accounts

- **Module:** Accounts
- **Module code:** ACCT
- **Version:** v0.2
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-ACCT-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

User accounts: registration, login and authentication, and guest use without an account. Free and paid plans are in [PLANS.md](PLANS.md).

## Decided so far

### Accounts

- Users can register and log in with **email + password**.
- Account features: **email verification**, **password recovery**, **password change**, **account deletion**.
- **Authentication:** a short-lived **access token** (~15 min) and a long-lived **refresh token** (~30 days), stored securely on the device.
- **Passwords:** hashed with a strong algorithm (argon2 or bcrypt); minimum **8 characters**.
- The provider used to send emails is still to be decided (open point 1).

### Guests (no account)

- Without an account, a user can do **everything except receive push notifications** ([NOTIFICATIONS.md](NOTIFICATIONS.md)). Guests are on the free plan ([PLANS.md](PLANS.md)).
- Guest data (subscriptions, preferences) is stored **only locally** on the device, in a **SQLite** database (expo-sqlite, [CONVENTIONS.md](CONVENTIONS.md) §Stack). The app warns guests to register to avoid losing their data.
- On registration, the local data is **preserved**: it is moved to the new account.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Email sending provider.** Which provider sends verification and password-recovery emails.
