---
applyTo: "**/*.py"
---
# Python conventions

- Python 3.13, FastAPI, managed with `uv`. Full type hints; mypy strict, no `# type: ignore`.
- Pydantic v2 models at every boundary — no raw dicts in or out of the API layer.
- Async-first: `async def` for all I/O; use `httpx.AsyncClient` and async DB drivers.
- Money is `decimal.Decimal`, never `float`. Carry an ISO-4217 currency alongside every amount.
- Layering (see docs/architecture.md): `api/` (routes, Pydantic) → `domain/` (pure logic,
  no framework imports) → `adapters/` (I/O: repositories, clients). Dependencies point inward.
- Domain logic must be pure and deterministic: inject `as_of` dates / clocks, never call
  `date.today()` inside a rule. This keeps behaviour testable.
- Errors: raise typed domain exceptions; map them to RFC-7807 `application/problem+json`
  in the API layer. Never leak stack traces.
- Logging: `structlog`, JSON output. Never log PII or secrets.
- Lint and format with `ruff`; do not hand-format. No secrets in code — read from config.
