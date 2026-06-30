---
mode: agent
description: Turn a GitLab CRSU issue into a committed, detailed spec.md
---
The user gives you a GitLab issue key such as `CRSU-1234`.

1. **Fetch the issue** via the GitLab MCP — title, description, functional/NFR notes,
   acceptance criteria, and comments. If the MCP is unavailable, ask the user to paste
   the issue body. Treat the issue as *input intent*, not as the final spec.

2. **Read the project context before writing** (this is what the Business Analyst does
   not know and what turns a vague issue into a precise spec):
   - `docs/constitution.md` — non-negotiable engineering principles.
   - `docs/architecture.md` — internal structure, layering, error model.
   - `docs/domain-glossary.md` — map business terms to code/domain terms.
   - Any existing specs under `specs/` that this touches.

3. **Write** `specs/<KEY-slug>/spec.md` (KEY = issue key, slug = short kebab title).
   Distil the vague business requirement into a precise, testable specification:
   - Header line `Source: CRSU-1234` plus the issue URL (two-way traceability).
   - Functional requirements in **EARS** notation (When/While/If … the system shall …).
   - Non-functional requirements made concrete (latency budget, security, observability).
   - Acceptance criteria as **Gherkin** (Given/When/Then) — these become the tests.
   - Reason/error codes, explicit out-of-scope, and assumptions.

4. **Do not design implementation or write code.** Where the issue is ambiguous, list the
   gap under "Open questions" rather than guessing, and stop for the BA/engineer to resolve.

The issue is the origin of intent; this committed `spec.md` is the reviewed source of truth.
