---
mode: agent
description: Implement the next unchecked task from tasks.md
---
Read the `tasks.md` in the target `specs/<KEY-slug>/` folder. Implement the **first unchecked
`- [ ]` task only**.

- Follow AGENTS.md, docs/constitution.md and the relevant `.instructions.md` files.
- Write or update the test(s) named in the task and run them.
- When green, mark the task `- [x]` in tasks.md.
- Summarise what changed and stop. Do **not** start the next task — one task per run keeps
  diffs small and reviewable.

If you cannot complete the task without a decision that is not already in plan.md, stop and ask
rather than inventing a design choice.
