← Back to [MAIN](MAIN.md)

# Conventions

**Version:** v0.4

Project-wide conventions, filled in as the user decides them.

## Process

Language and git rules (branches, commits, merges) are defined in [AGENTS.md](../../AGENTS.md) (§5 Git, §8 Language).

## Stack

How these components fit together: [ARCHITECTURE.md](ARCHITECTURE.md).

### Backend

- **Language and environment:** Python, with dependencies and virtual environment managed by **uv**.
- **API:** FastAPI.
- **Telegram listener:** Telethon.
- **Database:** PostgreSQL.
- **Database access:** SQLAlchemy 2 in async mode with the asyncpg driver.
- **Migrations:** Alembic.
- **Transactional email:** Resend, behind an interchangeable email-sending component ([ACCOUNTS.md](ACCOUNTS.md)).
- **Containers:** one Docker image for the backend, used by the API and the worker processes; Docker Compose for local development with PostgreSQL.

### Mobile app

- **Language:** TypeScript, in strict mode.
- **Package manager:** pnpm. pnpm with Expo/Metro may need its documented `node-linker` configuration; the Implementer follows Expo's official pnpm guidance.
- **Framework:** React Native + Expo.
- **Navigation:** Expo Router.
- **API data fetching:** TanStack Query.
- **Local storage of guest data:** SQLite through expo-sqlite ([ACCOUNTS.md](ACCOUNTS.md)).

## Naming

- **Product name:** `Apagonía` (with accent), used in user-facing text and documents.
- **Identifiers:** the repository, the Python package (`backend/src/apagonia/`) and all other code identifiers use `apagonia`, without the accent.

## Code style

- **Backend:** Ruff for linting and formatting; mypy in strict mode for type checking.
- **Mobile app:** ESLint for linting; Prettier for formatting; TypeScript in strict mode for type checking.

## Project structure

Monorepo ([ARCHITECTURE.md](ARCHITECTURE.md)):

```
/
├── AGENTS.md, CLAUDE.md   # Process rules
├── .claude/agents/        # Agent definitions
├── docs/sdd/              # Specs (SDD documentation)
├── compose.yaml           # Docker Compose for local development (backend processes + PostgreSQL)
├── backend/               # Python backend (API and worker)
│   ├── pyproject.toml     # Project and dependencies (uv)
│   ├── uv.lock            # Locked dependency versions (uv)
│   ├── Dockerfile         # The single backend image
│   ├── alembic/           # Alembic migrations
│   ├── src/apagonia/      # Backend Python package
│   └── tests/             # Backend tests
└── mobile/                # Expo app
    ├── package.json       # Dependencies (pnpm)
    ├── pnpm-lock.yaml     # Locked dependency versions (pnpm)
    ├── app/               # Expo Router routes (screens only)
    ├── src/               # Non-route app code (components, API client, storage, etc.)
    └── __tests__/         # App tests
```

The internal layout of `backend/src/apagonia/` and `mobile/src/` is set by the implementation phases.

## Testing

### Backend

- **Framework:** pytest.
- **Location:** `backend/tests/`, mirroring the package structure of `backend/src/apagonia/` (e.g. `backend/src/apagonia/listener/parser.py` → `backend/tests/listener/test_parser.py`).
- **Naming:** files `test_<module>.py`; test functions `test_<behaviour>`. Tests that cover an acceptance criterion name its ID in the function name or docstring (e.g. `AC-LSN-001-1`).
- **Database:** tests that need a database run against PostgreSQL (the Docker Compose service, in a separate test database), never against a different database engine.

### Mobile app

- **Framework:** Jest + React Native Testing Library.
- **Location:** `mobile/__tests__/`, mirroring the structure of `mobile/src/` and `mobile/app/` (tests are kept out of `mobile/app/`, because Expo Router treats every file there as a route).
- **Naming:** files `<name>.test.ts`, or `<name>.test.tsx` for components. Tests that cover an acceptance criterion name its ID in the test name.

## Dependencies

- **Backend:** declared in `backend/pyproject.toml` and managed with uv; `backend/uv.lock` is committed.
- **Mobile app:** declared in `mobile/package.json`; managed with pnpm; `mobile/pnpm-lock.yaml` is committed.
- New dependencies beyond the stack above follow the normal workflow: they are proposed and decided by the user ([AGENTS.md](../../AGENTS.md) §1, golden rule 2).

## Runtime versions

- **Python:** the latest stable version compatible with FastAPI and Telethon at implementation start.
- **Node.js:** the latest LTS version compatible with Expo at implementation start.
- The exact versions are pinned here when the project is scaffolded.

## Other

TBD — decided by the user.
