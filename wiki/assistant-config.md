_[← Guidelines home](home)_

# The assistant config (`.github`) structure and why

AI assistants that read the repository pick up configuration files from a conventional folder. The
**concept** generalizes to any such assistant: some context is loaded *always*, some
*conditionally*, some *on demand*. Structure the files to match, or you pay for everything all the
time. See the [Context & cost model](context-and-cost) for the cost rationale.

```
.github/
├── copilot-instructions.md     # ALWAYS loaded — keep tiny; point to the agent guide
├── instructions/               # CONDITIONALLY loaded — scoped by file-path globs
│   ├── <language>.instructions.md
│   ├── tests.instructions.md
│   └── api.instructions.md
└── prompts/                    # ON DEMAND — reusable commands the developer invokes
    ├── specify.prompt.md
    ├── plan.prompt.md
    ├── tasks.prompt.md
    └── implement.prompt.md
AGENTS.md                       # ALWAYS loaded — the canonical, lean agent guide (repo root)
```

| File | When loaded | Holds | Mandatory? |
|---|---|---|---|
| `AGENTS.md` | Always | What the service is, how to build/test it, the non-negotiable rules, the workflow | Yes |
| `.github/copilot-instructions.md` | Always | A 3–5 line pointer to `AGENTS.md` and the instruction files | Yes |
| `.github/instructions/*.md` | When a file matching its glob is in context | Detailed coding / test / API standards | Recommended |
| `.github/prompts/*.md` | When the developer runs it | The `specify` / `plan` / `tasks` / `implement` commands | Recommended |

Why this shape:
- **`AGENTS.md` is canonical and tool-neutral**; `copilot-instructions.md` is a thin pointer to it.
  Don't duplicate content between them — you'd pay for it twice and they'd drift.
- **Detailed standards are scoped** so a change in one language never loads another's rules.
- **Workflow commands are on demand**, costing nothing until invoked.

> **In our setup:** these filenames are the GitHub Copilot convention. Copilot reads the local
> workspace, so this works in a **GitLab-hosted** repo too — you don't need GitHub the host, only
> the folder layout.
