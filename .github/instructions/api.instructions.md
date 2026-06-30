---
applyTo: "**/contracts/**,**/openapi/**"
---
# API & contract conventions

- The OpenAPI file is the source of truth. Code is generated from, or validated against, it —
  never the other way round.
- Versioned paths: `/v1/...`. Breaking changes require a new major version, not an edit in place.
- Errors use RFC-7807 `application/problem+json`, with a stable machine-readable `type`.
- Business outcomes that are not errors (e.g. "not eligible") return `200` with a structured
  body and a list of reason codes — do not overload HTTP error codes for business rules.
- Reason/error codes are a closed enum, documented in the schema and kept in sync with the
  domain layer.
- Every field documented; every monetary field has an ISO-4217 `currency` sibling.
- After changing a contract: regenerate clients/stubs and update the contract tests.
