_[← Guidelines home](home)_

# Artifacts & repository layout

Specs live **with the code**, one folder per requirement:

```
specs/
└── <TICKET-KEY>-<short-slug>/
    ├── spec.md          # WHAT & WHY — requirements, NFRs, acceptance criteria. No code.
    ├── plan.md          # HOW — design, decisions, risks.
    ├── tasks.md         # STEPS — ordered, verifiable checklist (optional for trivial changes).
    └── contracts/       # machine-readable interfaces (API schemas, message contracts).
```

Conventions:
- **Folder name = `<TICKET-KEY>-<slug>`** so the requirement and the spec are linked by name.
- **Every spec starts with `Source: <ticket-id>`** for two-way traceability.
- **Three files, not one** — so a given assistant interaction loads only the phase it needs
  (implementation attaches `tasks.md` + code, not the whole history). See the
  [Context & cost model](context-and-cost).
- **Mandatory:** `spec.md`. **Strongly recommended:** `plan.md`. **Scalable/optional:** `tasks.md`,
  `contracts/`.
