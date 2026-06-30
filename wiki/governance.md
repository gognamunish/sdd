_[← Guidelines home](home)_

# Governance, audit & compliance

For regulated work the discipline in these guidelines is also the audit trail:

- **Traceability:** `Source: <ticket>` in the spec + the spec link in the merge request give a
  two-way, walkable chain: requirement → spec → plan → tasks → commit → merge.
- **Immutability:** the spec the code was built against is a specific commit, not a mutable ticket.
  See [Why the ticket is not the spec](why-ticket-is-not-spec).
- **Separation of duties:** the BA owns *what*, engineering owns *how*, a separate reviewer
  approves the merge; the assistant is never the approver. See
  [Roles & ownership](roles-and-ownership).
- **Policy values** (limits, retention windows, thresholds) are owned by Risk / Compliance and are
  recorded as **configuration referenced by the spec**, not as numbers buried in code.
- **Explainability:** business outcomes are returned as documented reason codes, so a decision can
  be explained from the spec and the contract.
