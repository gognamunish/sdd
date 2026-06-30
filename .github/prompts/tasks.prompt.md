---
mode: agent
description: Generate tasks.md (ordered work items) from plan.md
---
Read the `plan.md` in the target `specs/<KEY-slug>/` folder. Produce `tasks.md` as an ordered
checklist of small, independently verifiable tasks. Each task:
- Starts with `- [ ]`.
- Names the file(s) it touches.
- Is doable in well under an hour.
- States how it is verified (the test name or the command to run).

Rules:
- Order by dependency; a task only depends on earlier ones.
- Put each test alongside the code it covers, not all tests at the end.
- The final task is always "all acceptance criteria in spec.md have a passing test".
- Keep it mechanical — no design decisions here; those belong in plan.md.
