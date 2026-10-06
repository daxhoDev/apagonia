# MAIN — SDD Documentation Index

This is the entry point of the project documentation. Every document below links back to this file.

Process rules (workflow, agents, git, documentation rules): [AGENTS.md](../../AGENTS.md)

## Project overview

A system that listens in real time to the Telegram channel of the Empresa Eléctrica de Holguín and lets users subscribe to circuits, see their outage time and get notified when power returns.

## Process docs

| Document | Purpose | Version |
|---|---|---|
| [CONVENTIONS.md](CONVENTIONS.md) | Project-wide conventions (stack, style, structure, testing, dependencies) | v0.2 |
| [CHANGELOG.md](CHANGELOG.md) | Change history, references only | — |
| [DEVIATIONS.md](DEVIATIONS.md) | Record of user decisions that contradict approved specs | — |
| [_TEMPLATE.md](_TEMPLATE.md) | Spec module template (template, not a module) | — |

## Modules

| Document | Code | Purpose | Version | Status |
|---|---|---|---|---|
| [ARCHITECTURE.md](ARCHITECTURE.md) | ARCH | System components, data flow and hosting | v0.1 | Draft |
| [LISTENER.md](LISTENER.md) | LSN | Telegram channel ingestion and message parsing | v0.1 | Draft |
| [CIRCUITS.md](CIRCUITS.md) | CIRC | Circuit catalog and current circuit state | v0.1 | Draft |
| [ACCOUNTS.md](ACCOUNTS.md) | ACCT | User registration, login and authentication | v0.1 | Draft |
| [SUBSCRIPTIONS.md](SUBSCRIPTIONS.md) | SUBS | Link between users and the circuits they follow | v0.1 | Draft |
| [NOTIFICATIONS.md](NOTIFICATIONS.md) | NOTIF | Notification types, user preferences and push delivery | v0.1 | Draft |
| [MOBILE.md](MOBILE.md) | MOB | Client-wide concerns: platform, distribution and locale | v0.1 | Draft |

<!-- Modules are added as rows: | [<NAME>.md](<NAME>.md) | <CODE> | <approved purpose> | vX.Y | Draft/Approved | -->
