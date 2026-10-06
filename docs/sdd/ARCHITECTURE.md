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

### Components

The system is made of these logical components. Each one is specified in the module named next to it.

1. **Listener** (Telethon): reads the Telegram channel in real time ([LISTENER.md](LISTENER.md)).
2. **Parser:** turns status messages into circuit states and the provincial situation ([LISTENER.md](LISTENER.md)).
3. **State engine:** applies parsed messages to the current circuit state and the history, and detects the resulting changes ([CIRCUITS.md](CIRCUITS.md), [HISTORY.md](HISTORY.md)).
4. **Outbox:** a PostgreSQL table where the state engine records the events the notifier must handle (see *Communication*).
5. **Notifier:** reads outbox events, decides who gets which notification ([NOTIFICATIONS.md](NOTIFICATIONS.md)) and hands them to the delivery channel.
6. **Delivery channel:** interchangeable; Expo Push today, replaceable by a self-hosted persistent connection ([NOTIFICATIONS.md](NOTIFICATIONS.md)).
7. **API** (FastAPI): serves the mobile app (circuits, accounts, subscriptions, preferences, app version checks).
   - **Email sender:** interchangeable component used for account emails; Resend today ([ACCOUNTS.md](ACCOUNTS.md)).
8. **Database:** PostgreSQL, shared by the API and the worker.
9. **Mobile app** (React Native + Expo): Android client ([MOBILE.md](MOBILE.md)).

Data flow: Telegram channel → Listener → Parser → State engine → (PostgreSQL: state, history, outbox) → Notifier → Delivery channel → app. The app reads data through the API.

### Processes

- **Default deployment: 2 processes.**
  - **API process:** the FastAPI application.
  - **Worker process:** the listener (with parser and state engine) and the notifier, together.
- The code also allows running **3 processes** (API, listener, notifier) **by deployment configuration only**, with no code change.
- The process that runs the listener is deployed with a **single replica**, protected by a database lock ([LISTENER.md](LISTENER.md)).

### Communication

- **State engine → notifier:** through the **outbox** table. Every outbox entry is written **in the same database transaction** as the state change it describes, so a change is never stored without its event, nor an event without its change. No Redis or message broker is used.
- **Outbox processing:** the notifier reads the outbox table **every 3 seconds**, handles each pending entry, **marks it as sent** once delivered, and **retries failed entries** on later reads, with the limits, backoff and permanent-failure handling defined in [NOTIFICATIONS.md](NOTIFICATIONS.md) §Delivery. There is a **single notifier** instance, consistent with the single worker.
- **App → backend:** HTTP requests to the API only. While the app is open it gets updates by **periodic polling** ([MOBILE.md](MOBILE.md)); there is no SSE or WebSocket.
- **Backend → app (closed):** push notifications through the delivery channel ([NOTIFICATIONS.md](NOTIFICATIONS.md)).

### Repository and deployment

- **Monorepo:** this repository holds the specs and both code bases (`backend/`, `mobile/`). Layout: [CONVENTIONS.md](CONVENTIONS.md) §Project structure.
- **Docker:** **one backend image**, used by both the API and the worker processes (the process is chosen at start-up by configuration).
- **Local development:** Docker Compose runs the backend processes with a PostgreSQL database.
- **Database access:** SQLAlchemy 2 (async, asyncpg); schema changes through Alembic migrations ([CONVENTIONS.md](CONVENTIONS.md) §Stack).

### Other decided items

- Stack and tooling: see [CONVENTIONS.md](CONVENTIONS.md) §Stack.
- Push delivery is a pluggable channel that can be replaced ([NOTIFICATIONS.md](NOTIFICATIONS.md)).
- Administrator operations (confirming circuit merges, activating paid plans) are **server commands/scripts** ([CIRCUITS.md](CIRCUITS.md), [PLANS.md](PLANS.md)).

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

1. **Hosting.** Not decided; it is decided **last**, after every other open point. The backend needs an always-on Telegram listener, 3 months of history in the database ([HISTORY.md](HISTORY.md)) and APK file serving ([MOBILE.md](MOBILE.md)). Options under consideration (research dated 2026-10-06):
   - **Render:** free web services sleep after 15 min idle; no free workers; free PostgreSQL expires after 30 days; its terms explicitly prohibit users in Cuba.
   - **Oracle Cloud Always Free:** always-on VMs, but no managed PostgreSQL; its terms explicitly prohibit Cuban nationals.
   - **Railway:** $1/month credit, likely insufficient for an always-on service.
   - **Fly.io:** no free plan (from about $2.19/month per small always-on machine).
   - **Koyeb:** the free instance cannot be a worker and sleeps.
   - **Northflank Developer Sandbox:** 2 always-on services + 1 database add-on, no card required; CPU/RAM unspecified; no Cuba clause found (IP blocking unknown).
   - **Neon:** free PostgreSQL, 1 GB, permanent; auto-suspends after 5 min idle; no Cuba clause found.
   - **Supabase:** 500 MB database; pauses after 1 week idle; 50 MB upload cap (too small for the APK).
   - **Cloudflare R2:** 10 GB free storage with free egress; fits the APK.
   - **Candidate free combination:** Northflank (API + one worker) + Neon or the Northflank database + R2 for the APK. It fits the default 2-process deployment (see *Processes*).
   - **Low-cost paid hosting:** covered by the app's own revenue ([PLANS.md](PLANS.md)), leaving some profit, without charging Cuban users much.
