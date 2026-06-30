# Engineering constitution

Durable, project-wide principles the AI agent and every contributor must respect. These rarely
change and apply to *all* specs, plans, and code. AGENTS.md references this file; the `/specify`
and `/plan` prompts read it. Keep it short and absolute.

## Correctness & money
- Money is always `Decimal` with an explicit ISO-4217 currency. Never `float` for amounts.
- Compare and operate on amounts only within the same currency.
- Domain logic is **pure and deterministic** — inject time (`as_of`) and randomness; never read
  the wall clock inside a business rule.

## Security & data
- No secrets in code, config, or logs. Read secrets from the platform secret store.
- Never log PII or full instrument numbers. Mask to the minimum needed.
- Validate and type all input at the boundary (Pydantic). Reject, don't coerce silently.

## API & contracts
- The OpenAPI contract is the source of truth. Versioned paths (`/v1`). No breaking changes
  inside a version.
- Errors use RFC-7807. Business outcomes (e.g. "not eligible") are `200` + structured reason
  codes, not HTTP error codes.

## Testing & change size
- Every acceptance criterion in a spec has at least one test.
- Changes are small and reversible — one task per `/implement` run.
- No merge without green CI and a reviewed MR.

## Traceability
- Every spec records its `Source: CRSU-####`.
- Every MR links the spec it implements.

## When in doubt
- If a rule here conflicts with a request, the constitution wins — stop and raise it.
- If something is ambiguous, ask; do not guess a business rule.
