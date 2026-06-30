# Spec: Refund eligibility check

**Source:** CRSU-1234 · https://gitlab.example.ubs.net/payments/refunds/-/issues/1234
**Status:** ✅ locked (BA sign-off + engineering sign-off)
**Derived from:** [`00-source-issue.md`](00-source-issue.md)

> Output of `/specify`. The vague issue has been distilled into precise, testable requirements
> using `docs/architecture.md` and `docs/domain-glossary.md`. Open questions from the issue
> (time limit, foreign currency) are now resolved and recorded below.

## Goal
Let a caller (a CS-agent-facing app) check whether a settled payment can be refunded, and by
how much, getting an instant, explainable answer with machine-readable reason codes.

## Scope
- A single read-only eligibility check. **No refund is performed** (out of scope, per issue).

## Functional requirements (EARS)
- **FR-1** The system **shall** return, for a given payment and requested amount, whether the
  refund is eligible and the maximum refundable amount.
- **FR-2** **When** the payment status is not `SETTLED`, the system **shall** return not
  eligible with reason `NOT_SETTLED`.
- **FR-3** **When** the requested currency differs from the payment's currency, the system
  **shall** return not eligible with reason `CURRENCY_MISMATCH`.
- **FR-4** **When** more than 180 days have passed since capture, the system **shall** return
  not eligible with reason `REFUND_WINDOW_EXPIRED`. *(Resolves issue open question — window
  fixed at 180 days, configurable.)*
- **FR-5** **While** the payment is `SETTLED`, the system **shall** compute
  `max_refundable = amount − already_refunded` (never below zero) and, **if** the requested
  amount exceeds it, return not eligible with reason `EXCEEDS_REFUNDABLE`.
- **FR-6** **If** the requested amount is not greater than zero, the system **shall** reject the
  request as invalid input (`INVALID_AMOUNT`).
- **FR-7** The system **shall** return *all* failing reason codes for a request, not just the
  first, so the caller sees everything wrong at once.
- **FR-8** **If** the payment does not exist, the system **shall** return `404`.

## Non-functional requirements
- **NFR-1 (latency):** p99 ≤ 100 ms server-side for a single check.
- **NFR-2 (security):** response must never include card/instrument numbers or PII; only the
  payment id, decision, amounts, currency, and reason codes.
- **NFR-3 (auditability):** every decision is explainable from its reason codes; inputs and the
  decision are logged (without PII) for audit.
- **NFR-4 (currency):** all amounts are `Decimal` with an explicit ISO-4217 currency.

## Acceptance criteria (Gherkin → these become tests)
```gherkin
Scenario: Full refund on a settled payment is eligible
  Given a SETTLED payment of 100.00 CHF captured 10 days ago with nothing refunded
  When an eligibility check is requested for 100.00 CHF
  Then the result is eligible
  And max_refundable is 100.00

Scenario: Only the remaining amount is refundable
  Given a SETTLED payment of 100.00 CHF with 40.00 already refunded
  When an eligibility check is requested for 100.00 CHF
  Then the result is not eligible with reason EXCEEDS_REFUNDABLE
  And max_refundable is 60.00

Scenario: An unsettled payment cannot be refunded
  Given an AUTHORIZED payment of 100.00 CHF
  When an eligibility check is requested for 10.00 CHF
  Then the result is not eligible with reason NOT_SETTLED

Scenario: A foreign-currency request is rejected
  Given a SETTLED payment of 100.00 CHF
  When an eligibility check is requested for 10.00 USD
  Then the result is not eligible with reason CURRENCY_MISMATCH

Scenario: A payment older than the refund window cannot be refunded
  Given a SETTLED payment of 100.00 CHF captured 200 days ago
  When an eligibility check is requested for 10.00 CHF
  Then the result is not eligible with reason REFUND_WINDOW_EXPIRED

Scenario: Unknown payment
  Given no payment with id "nope"
  When an eligibility check is requested for "nope"
  Then the response is 404
```

## Out of scope
- Executing the refund. Cross-currency refunds. Bulk/automated refunds.

## Resolved questions (were open in the issue)
- **Refund time limit:** 180 days from capture (configurable). → FR-4.
- **Different currency:** rejected, not converted. → FR-3.

## Traceability
Issue CRSU-1234 → this spec → [`plan.md`](plan.md) → [`tasks.md`](tasks.md).
