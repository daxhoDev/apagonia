# MAIN — SDD Documentation Index

This is the entry point of the project documentation. Every document below links back to this file.

Process rules (workflow, agents, git, documentation rules): [AGENTS.md](../../AGENTS.md)

## Project overview

Product name: **Apagonía**, a wordplay between *apagón* (blackout) and *agonía* (agony). Repository, package and other code identifiers use `apagonia`, without the accent ([CONVENTIONS.md](CONVENTIONS.md)).

A system that listens in real time to the Telegram channel of the Empresa Eléctrica de Holguín and lets users subscribe to circuits, see their outage time and get notified when power returns.

## Process docs

| Document | Purpose | Version |
|---|---|---|
| [CONVENTIONS.md](CONVENTIONS.md) | Project-wide conventions (stack, style, structure, testing, dependencies) | v0.4 |
| [CHANGELOG.md](CHANGELOG.md) | Change history, references only | — |
| [DEVIATIONS.md](DEVIATIONS.md) | Record of user decisions that contradict approved specs | — |
| [_TEMPLATE.md](_TEMPLATE.md) | Spec module template (template, not a module) | — |

## Modules

| Document | Code | Purpose | Version | Status |
|---|---|---|---|---|
| [ARCHITECTURE.md](ARCHITECTURE.md) | ARCH | System components, data flow and hosting | v0.2 | Draft |
| [LISTENER.md](LISTENER.md) | LSN | Telegram channel ingestion and message parsing | v0.2 | Draft |
| [CIRCUITS.md](CIRCUITS.md) | CIRC | Circuit catalog, current circuit state, provincial and daily situation | v0.2 | Draft |
| [ACCOUNTS.md](ACCOUNTS.md) | ACCT | User accounts, authentication and guest use | v0.2 | Draft |
| [SUBSCRIPTIONS.md](SUBSCRIPTIONS.md) | SUBS | Link between users and the circuits they follow | v0.2 | Draft |
| [NOTIFICATIONS.md](NOTIFICATIONS.md) | NOTIF | Notification types, user preferences and push delivery | v0.2 | Draft |
| [MOBILE.md](MOBILE.md) | MOB | Client-wide concerns: platform, distribution and locale | v0.2 | Draft |
| [HISTORY.md](HISTORY.md) | HIST | Stored circuit history, per-circuit statistics and prediction | v0.1 | Draft |
| [PLANS.md](PLANS.md) | PLAN | Free and paid plans, limits, manual payments and plan activation | v0.1 | Draft |
| [WIDGET.md](WIDGET.md) | WIDG | Android home-screen widget | v0.1 | Draft |
| [ROADMAP.md](ROADMAP.md) | ROAD | Project plan: MVP (including push validation) and phase order | v0.1 | Draft |

<!-- Modules are added as rows: | [<NAME>.md](<NAME>.md) | <CODE> | <approved purpose> | vX.Y | Draft/Approved | -->
