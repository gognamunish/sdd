# Architecture (the part the BA does not know)

This document is the **internal knowledge** that turns a vague `CRSU-####` issue into a precise
spec. The `/specify` step reads it so the agent describes requirements in terms of the real
structure rather than guessing. It is illustrative for the demo service in this repo.

## Service shape
A small FastAPI service. One bounded context: *refund eligibility*. It does not own payments —
it reads them from the Ledger service and answers a single question: *can this payment be
refunded, and by how much?*

## Layering (dependencies point inward)

```
  api/        FastAPI routers, Pydantic request/response models, HTTP error mapping
   │  depends on
   ▼
  domain/     Pure business logic — dataclasses + functions. No framework imports.
   ▲  depended on by
   │
  adapters/   I/O: the Ledger client, repositories, config. Implements ports the domain needs.
```

- **api/** — translates HTTP ↔ domain. Validates input, maps domain results and exceptions to
  responses (RFC-7807 for errors; `200` + reason codes for business outcomes).
- **domain/** — the rules. Pure, deterministic, unit-testable without a server or network.
  Time is injected (`as_of`). This is where eligibility rules and reason codes live.
- **adapters/** — everything that talks to the outside world. Swappable; the demo uses an
  in-memory repository instead of the real Ledger.

## Conventions a spec should assume
- **Reason codes** are a closed enum, shared between the domain and the OpenAPI contract. A "not
  eligible" answer returns the list of codes that failed (the caller sees *everything* wrong at
  once, not just the first failure).
- **Error model:** malformed input → `422`/`400` RFC-7807 problem. Unknown payment → `404`.
  A valid request for an ineligible payment → `200` with `eligible: false` and reasons.
- **Money:** `Decimal` + currency everywhere; the contract pairs each amount with a `currency`.
- **Configurable rules** (e.g. the refund window) are config, not magic numbers in logic.

## Adding a new endpoint (so the agent knows the shape)
1. Define/extend the OpenAPI contract under the feature's `contracts/`.
2. Add or extend pure rules in `domain/`, with unit tests.
3. Add the Pydantic models + router in `api/`.
4. Wire the adapter (repository/client) it needs.
5. Add a contract test asserting the route matches the OpenAPI spec.
