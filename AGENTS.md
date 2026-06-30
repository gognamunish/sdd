# Refund Eligibility Service — Agent Guide

> This is an **example** AGENTS.md for the demo service in this repo. It is Tier-0 context
> (sent on every Copilot request), so keep it lean — target ~1–2 KB. Detailed standards live
> in `.github/instructions/` and `docs/`, loaded on demand.

## What this is
A FastAPI microservice that decides whether a settled payment can be refunded, and by how
much. It reads payment state from the Ledger service and returns an eligibility decision with
machine-readable reason codes. Domain owner: Payments squad.

## Build, test, run
- Setup:       `uv sync`
- Tests:       `uv run pytest`
- Lint/format: `uv run ruff check --fix && uv run ruff format`
- Type check:  `uv run mypy src`
- Run:         `uv run uvicorn refund_eligibility.main:app --reload`

## Non-negotiable conventions
- Python 3.13, FastAPI. Full type hints; mypy strict, no ignores.
- Pydantic v2 at the boundary; pure dataclasses in the domain layer.
- Money is `Decimal` + ISO-4217 currency. Never `float`.
- Domain rules are pure and deterministic — inject `as_of`, never call `date.today()` in logic.
- No secrets in code; no PII in logs. See docs/constitution.md for the full list.

## Spec-driven workflow (see docs/workflow.md)
- Requirements start as GitLab `CRSU-####` issues. A raw issue is NOT the spec —
  the committed `specs/<KEY-slug>/spec.md` is. See docs/why-issues-are-not-specs.md.
- Order: `spec.md` (what) → `plan.md` (how) → `tasks.md` (steps) → implement.
- Commands: `/specify`, `/plan`, `/tasks`, `/implement` (in `.github/prompts/`).
- Worked example: `specs/CRSU-1234-refund-eligibility/`.

## Detailed standards (loaded on demand — do not inline here)
- Python rules: .github/instructions/python.instructions.md
- Test rules:   .github/instructions/tests.instructions.md
- API/contract: .github/instructions/api.instructions.md
- Architecture: docs/architecture.md   Glossary: docs/domain-glossary.md

## Do not touch
- `src/generated/**` (generated from the OpenAPI contract; regenerate, don't hand-edit).
- `.venv/**`.
