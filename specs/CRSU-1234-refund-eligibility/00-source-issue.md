# CRSU-1234 — source GitLab issue (as the BA wrote it)

> This is a snapshot of the raw GitLab issue, kept for traceability. It is intentionally
> **vague** — that is normal and fine. Compare it with `spec.md` to see what the `/specify`
> step adds. The issue is the *origin of intent*; `spec.md` is the build contract.
> See `../../docs/why-issues-are-not-specs.md`.

---

**Issue:** CRSU-1234: Let agents check if a payment can be refunded
**Author:** A. Analyst (Payments BA) · **Labels:** ~requirement
**URL:** https://gitlab.example.ubs.net/payments/refunds/-/issues/1234

## Goal
Customer-service agents keep asking the back office whether a payment can be refunded.
We want a way to check this directly so they get an instant answer.

## Functional requirements
- Should tell us if a payment can be refunded.
- Should handle partial refunds — sometimes only part of the money is given back.
- Should not allow refunding more than what was paid.
- Old payments probably shouldn't be refundable but I'm not sure of the exact limit.

## Non-functional requirements (NFRs)
- Performance / latency: should feel instant.
- Security / data sensitivity: payment data is sensitive, don't expose card details.
- Audit / compliance: refunds are regulated, we need to be able to explain decisions.

## Acceptance criteria
- If a payment went through fully, an agent can see it's refundable.
- If half was already refunded, only the rest can be refunded.
- You can't refund a payment that didn't complete.

## Out of scope
- Actually performing the refund — this is just the check.

## Open questions / assumptions
- What's the time limit for refunds?
- What about refunds in a different currency?
