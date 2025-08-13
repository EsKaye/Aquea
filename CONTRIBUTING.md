
# Contributing to Project Aquea

Thanks for helping bring free water to everyone.

## Dev Setup
- Docker + Compose installed.
- `cp .env.example .env`
- `docker compose up --build`

## Commit & Branching
- Conventional Commits (`feat:`, `fix:`, `docs:`…).
- PRs require: lint clean, unit tests passing.
- Add/maintain `SPDX-License-Identifier` in new files.

## NessHash
If you maintain **NessHash**, see `libs/nesshash-adapter`. We use a single interface so your library can drop-in for record hashing and ledgering.

## Security
- Keys and secrets never in code.
- Report security issues via security@ (TBD). Do not open public issues.

## Community
- Be kind. Water is life.
