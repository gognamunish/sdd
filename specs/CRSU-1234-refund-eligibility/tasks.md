# Tasks: Refund eligibility check

**Plan:** [`plan.md`](plan.md) · **Source:** CRSU-1234

> Output of `/tasks`. Ordered, each independently verifiable. `/implement` does one per run and
> ticks the box. (In this docs-only demo the boxes are left unchecked to show the starting state.)

- [ ] **T1 — Domain model.** Add `Payment` dataclass, `PaymentStatus` and `ReasonCode` enums,
  and `REFUND_WINDOW_DAYS = 180` in `domain/refund.py`.
  *Verify:* `uv run mypy src` passes; imports resolve.

- [ ] **T2 — Eligibility rule (happy path).** Implement `evaluate_refund_eligibility(...)` for
  the full-refund-on-settled case (FR-1, FR-5).
  *Verify:* `test_eligible_when_full_refund_on_settled_payment`.

- [ ] **T3 — Status, currency, window, amount rules.** Add FR-2, FR-3, FR-4, FR-6, and the
  all-reasons behaviour FR-7.
  *Verify:* `test_not_settled...`, `test_currency_mismatch...`, `test_window_expired...`,
  `test_collects_all_failing_reasons`.

- [ ] **T4 — Partial refund cap.** `max_refundable = amount − refunded`, never below zero;
  `EXCEEDS_REFUNDABLE` when over (FR-5).
  *Verify:* `test_only_remaining_amount_is_refundable_when_partly_refunded`.

- [ ] **T5 — Repository port + in-memory adapter.** `PaymentRepository` + seeded fixtures in
  `adapters/repository.py`.
  *Verify:* `test_repository_returns_seeded_payment`, `test_repository_raises_on_unknown`.

- [ ] **T6 — OpenAPI contract.** Author `contracts/refund-eligibility.openapi.yaml` (request,
  response, reason-code enum, 404).
  *Verify:* contract lints; matches the response model.

- [ ] **T7 — API route.** Pydantic models + router in `api/refund.py`; map to `200` / `404` /
  `422` per plan D-1/D-3 (FR-8).
  *Verify:* `test_eligible_full_refund_via_api`, `test_unknown_payment_returns_404`,
  `test_non_positive_amount_returns_422`.

- [ ] **T8 — Acceptance coverage.** Confirm every Gherkin scenario in `spec.md` has a passing
  test.
  *Verify:* `uv run pytest` green; one test per scenario, docstring names the scenario.
