# Implementation plan: Refund eligibility check

**Spec:** [`spec.md`](spec.md) (locked) · **Source:** CRSU-1234

> Output of `/plan`. Technical design only — no code. Consistent with `docs/architecture.md`.

## Components touched
| Layer | Change |
|---|---|
| `api/` | New router `POST /v1/payments/{payment_id}/refund-eligibility`; Pydantic request/response models; map domain result → `200`, unknown payment → `404`, bad input → `422`. |
| `domain/` | New pure function `evaluate_refund_eligibility(payment, amount, currency, *, as_of)` returning an `EligibilityDecision`; `Payment` dataclass; `ReasonCode` enum; `REFUND_WINDOW_DAYS = 180`. |
| `adapters/` | `PaymentRepository` port + in-memory implementation seeded with fixtures (stands in for the Ledger client in the demo). |

## Data model
`Payment`: `payment_id`, `amount: Decimal`, `currency: str`, `status: PaymentStatus`,
`captured_at: date | None`, `refunded_amount: Decimal`. No persistence owned by this service.

## API & contract
Defined in [`contracts/refund-eligibility.openapi.yaml`](contracts/refund-eligibility.openapi.yaml).
Request `{ amount, currency }`; response `{ payment_id, eligible, max_refundable, currency, reasons[] }`.

## Key decisions (lightweight ADRs)

### D-1 — Business outcome uses 200 + reasons, not a 4xx
- **Options:** (a) `409/422` when not eligible; (b) `200` with `eligible:false` + reason codes.
- **Choice:** (b).
- **Why:** "not eligible" is a valid answer to a valid question, not a client error. Reason codes
  are explainable and auditable (NFR-3). HTTP errors are reserved for malformed input / not found.

### D-2 — Return all failing reasons, not the first
- **Choice:** collect every failed rule (FR-7).
- **Why:** the CS agent fixes everything at once; better UX and clearer audit trail.

### D-3 — Invalid amount (≤ 0) is malformed input → 422
- **Choice:** validate `amount > 0` at the Pydantic boundary (`422`); keep `INVALID_AMOUNT` in the
  domain enum for non-HTTP callers.
- **Why:** a non-positive amount is a malformed request, not a business outcome.

### D-4 — Refund window is configuration, not a literal
- **Choice:** `REFUND_WINDOW_DAYS` constant/config = 180.
- **Why:** policy changes shouldn't require touching business logic (constitution).

### D-5 — Time is injected
- **Choice:** the domain function takes `as_of: date`; the API passes `date.today()`.
- **Why:** deterministic, testable rules (constitution / python instructions).

## Risks & dependencies
- Real implementation depends on the Ledger client; demo uses an in-memory repository.
- Refund window value is a compliance-owned policy — confirm 180 days with risk before go-live.

## Rollout
Additive, read-only endpoint. No migration. Safe to ship behind the existing service; rollback is
removing the route.
