# Domain glossary

Maps the business language a BA uses to the precise terms used in specs and code. The
`/specify` step uses this so a vague issue is rewritten in exact domain terms.

| Business term (BA) | Domain / code term | Meaning |
|---|---|---|
| Payment | `Payment` | A single customer payment held in the Ledger. Has an amount, currency, status, capture date. |
| "Money has gone through" | status `SETTLED` | Funds captured and settled. Only settled payments are refundable. |
| "Not gone through yet" | status `AUTHORIZED` | Authorised but not settled — cannot be refunded. |
| "Payment failed" | status `FAILED` | Terminal failure — not refundable. |
| Refund | reversal of value | Returning some or all of a settled payment to the customer. |
| "How much can be given back" | `max_refundable` | `amount − already_refunded`, never below zero. |
| "Already refunded some" | `refunded_amount` | Sum of prior refunds against the payment. |
| "Refund deadline" / "too old" | refund window | Max days after capture a refund is allowed (config; demo = 180 days). |
| "Why it can't be refunded" | reason code | Closed enum returned when a payment is not eligible. |
| Eligibility check | `evaluate_refund_eligibility` | The pure decision: eligible? max refundable? which reasons failed? |

## Reason codes (closed set)
| Code | Meaning |
|---|---|
| `NOT_SETTLED` | Payment is not in `SETTLED` state. |
| `CURRENCY_MISMATCH` | Requested currency differs from the payment's currency. |
| `REFUND_WINDOW_EXPIRED` | More than the allowed number of days since capture. |
| `EXCEEDS_REFUNDABLE` | Requested amount is greater than `max_refundable`. |
| `INVALID_AMOUNT` | Requested amount is not greater than zero. |
