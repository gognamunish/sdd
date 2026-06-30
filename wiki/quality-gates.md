_[← Guidelines home](home)_

# Quality gates & Definition of Done

Each transition has a gate that a human must pass. See [Roles & ownership](roles-and-ownership)
for who signs off.

| Gate | What must be true |
|---|---|
| Requirement accepted | Ticket has a goal and acceptance criteria; triaged. |
| **Spec locked** | BA sign-off ("captures intent") **and** Eng sign-off ("precise & testable"). |
| Plan accepted | Design reviewed; decisions recorded; contracts defined. |
| Tasks ready | Every acceptance criterion has a covering task. |
| Code merged | Code review approved; CI green; every acceptance criterion has a passing test. |

**Definition of Done for the whole change:**
- Ticket ↔ spec ↔ plan ↔ tasks ↔ merge request all link to each other.
- Every acceptance criterion has a passing test.
- The ticket is closed with a link to the merged change.
