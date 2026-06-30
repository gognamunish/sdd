_[← Guidelines home](home)_

# Principles

Five ideas everything else follows from. Each has a one-line reason.

1. **The spec is the source of truth — not the ticket, not the chat.**
   *Why:* code must be built against something precise, reviewed, and version-pinned. See
   [Why the ticket is not the spec](why-ticket-is-not-spec).
2. **Small, individually-verifiable steps.**
   *Why:* small diffs are reviewable, cheaper for the assistant to produce, and less prone to
   hallucination than "build the whole thing at once".
3. **A human gate at every transition.**
   *Why:* the assistant accelerates work; it does not get to approve its own requirements, design,
   or merges. Accountability stays with people.
4. **End-to-end traceability.**
   *Why:* in a regulated environment we must be able to walk requirement → spec → code → merge in
   both directions, and prove what was built and why.
5. **Lean always-on context.**
   *Why:* anything the assistant reads on *every* request is paid for on every request, by every
   developer. Keep the always-on context small and load detail only when needed. See the
   [Context & cost model](context-and-cost).
