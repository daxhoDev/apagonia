← Back to [MAIN](MAIN.md)

# Architecture

- **Module:** Architecture
- **Module code:** ARCH
- **Version:** v0.1
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-ARCH-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

The system's components and how they fit together: a Telegram listener that reads the Empresa Eléctrica de Holguín channel, a backend API, a database, and a mobile client. Also covers where the backend is hosted and how it is deployed. The technology stack is defined in [CONVENTIONS.md](CONVENTIONS.md) §Stack.

## Decided so far

- Components: a listener reading the Telegram channel ([LISTENER.md](LISTENER.md)), a backend API, a database, and a mobile app ([MOBILE.md](MOBILE.md)).
- Stack: see [CONVENTIONS.md](CONVENTIONS.md) §Stack.

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Hosting and deployment** of the backend.
