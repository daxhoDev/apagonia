← Back to [MAIN](MAIN.md)

# Accounts

- **Module:** Accounts
- **Module code:** ACCT
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-ACCT-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

User accounts: registration, login and authentication.

## Decided so far

- Users have accounts: registration and login with **email + password**.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Account features.** Email verification, password reset, password rules, account deletion, and the authentication mechanism (e.g. tokens/sessions and their lifetime).
2. **Access without an account.** Whether any part of the app (e.g. browsing circuits and their state) is usable without logging in.
