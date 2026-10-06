← Back to [MAIN](MAIN.md)

# Architecture

- **Module:** Architecture
- **Module code:** ARCH
- **Version:** v0.2
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-ARCH-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

The system's components and how they fit together: a Telegram listener that reads the Empresa Eléctrica de Holguín channel, a backend API, a database, and a mobile client. Also covers where the backend is hosted and how it is deployed. The project plan is in [ROADMAP.md](ROADMAP.md). The technology stack is defined in [CONVENTIONS.md](CONVENTIONS.md) §Stack.

## Decided so far

- Components: a listener reading the Telegram channel ([LISTENER.md](LISTENER.md)), a backend API, a database, and a mobile app ([MOBILE.md](MOBILE.md)).
- Stack: see [CONVENTIONS.md](CONVENTIONS.md) §Stack.
- Push delivery is a pluggable channel that can be replaced ([NOTIFICATIONS.md](NOTIFICATIONS.md)).
- Administrator operations (confirming circuit merges, activating paid plans) are **server commands/scripts** ([CIRCUITS.md](CIRCUITS.md), [PLANS.md](PLANS.md)).

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Architecture.** How the components are split into processes and communicate, and the repository layout. Not approved. A first proposal (separate listener/notifier/API processes, PostgreSQL outbox, monorepo `backend/` + `mobile/`, Docker Compose on a VPS) was not approved. A revised proposal (an API plus **one worker** combining listener and notifier, PostgreSQL outbox, monorepo, interchangeable delivery channel) is **deferred** by the user until hosting is decided.
2. **Hosting.** Not decided. The backend needs an always-on Telegram listener, 3 months of history in the database ([HISTORY.md](HISTORY.md)) and APK file serving ([MOBILE.md](MOBILE.md)). Options under consideration (research dated 2026-10-06):
   - **Render:** free web services sleep after 15 min idle; no free workers; free PostgreSQL expires after 30 days; its terms explicitly prohibit users in Cuba.
   - **Oracle Cloud Always Free:** always-on VMs, but no managed PostgreSQL; its terms explicitly prohibit Cuban nationals.
   - **Railway:** $1/month credit, likely insufficient for an always-on service.
   - **Fly.io:** no free plan (from about $2.19/month per small always-on machine).
   - **Koyeb:** the free instance cannot be a worker and sleeps.
   - **Northflank Developer Sandbox:** 2 always-on services + 1 database add-on, no card required; CPU/RAM unspecified; no Cuba clause found (IP blocking unknown).
   - **Neon:** free PostgreSQL, 1 GB, permanent; auto-suspends after 5 min idle; no Cuba clause found.
   - **Supabase:** 500 MB database; pauses after 1 week idle; 50 MB upload cap (too small for the APK).
   - **Cloudflare R2:** 10 GB free storage with free egress; fits the APK.
   - **Candidate free combination:** Northflank (API + one worker) + Neon or the Northflank database + R2 for the APK. It implies merging listener and notifier into one worker.
   - **Low-cost paid hosting:** covered by the app's own revenue ([PLANS.md](PLANS.md)), leaving some profit, without charging Cuban users much.
