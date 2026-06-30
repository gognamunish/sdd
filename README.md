# Spec-Driven Development with GitHub Copilot — reference repo

A ready-to-copy template showing how to run **spec-driven development** with GitHub Copilot,
where requirements start life as GitLab `CRSU-####` issues and are turned into committed,
reviewed specs that an AI agent implements.

It is documentation-and-scaffolding only — there is **no application code**. The point is the
*structure*: the `.github/` harness, the templates, the workflow, and a fully worked example.

---

## The flow in one picture

```
 GitLab issue  ──►  spec.md      ──►  plan.md     ──►  tasks.md    ──►  code + tests
 CRSU-1234          (what / why)      (how)            (steps)          (implementation)

 BA owns            Eng + BA own      Architect own    Eng own          AI agent + Eng own
 [issue template]   /specify          /plan            /tasks           /implement
                    pulls issue
                    via GitLab MCP
```

Each stage has an owner, a gate, and an artifact that links back to the previous one.
Full description: **[docs/workflow.md](docs/workflow.md)**.

## Why the GitLab issue is *not* the spec

The BA writes intent at a business altitude and doesn't know the code. The agent needs a
precise, testable, version-pinned, reviewed contract. We therefore distil `CRSU-####` into a
committed `spec.md` and build from **that** — for reasons of reproducibility, an approval gate,
audit traceability, context cost, and downstream determinism. Even with a GitLab MCP that can
fetch the issue, the MCP is only the *ingestion pipe*; we still commit the spec.

Full reasoning: **[docs/why-issues-are-not-specs.md](docs/why-issues-are-not-specs.md)**.

## How Copilot assembles context, and the cost model

Copilot builds every request in tiers: **Tier 0** always-on (`copilot-instructions.md`,
`AGENTS.md`), **Tier 1** conditional (`*.instructions.md` via `applyTo` globs), **Tier 2**
on-demand (`prompts/*.prompt.md`, MCP fetches). Tier 0 is multiplied by every request × every
developer, so it must stay lean. Full breakdown and the cost checklist:
**[docs/sdd-guide.md](docs/sdd-guide.md)**.

> Copilot reads the local workspace, so this `.github/` convention works in a **GitLab-hosted**
> repo — you don't need GitHub the host, only the folder layout.

## Repository layout

```
.
├── README.md                         ← you are here
├── AGENTS.md                         ← Tier-0 agent guide (lean; example for the demo service)
├── .github/
│   ├── copilot-instructions.md       ← Tier-0 — short pointer to AGENTS.md
│   ├── instructions/                 ← Tier-1 — load only when an applyTo glob matches
│   │   ├── python.instructions.md
│   │   ├── tests.instructions.md
│   │   └── api.instructions.md
│   └── prompts/                      ← Tier-2 — slash commands, on demand
│       ├── specify.prompt.md         ← CRSU issue → spec.md
│       ├── plan.prompt.md            ← spec.md → plan.md
│       ├── tasks.prompt.md           ← plan.md → tasks.md
│       └── implement.prompt.md       ← tasks.md → code, one task per run
├── .gitlab/
│   └── issue_templates/
│       └── Requirement.md            ← the BA's template (CRSU key, EARS, Gherkin)
├── docs/
│   ├── workflow.md                   ← the 4-stage process, owners, gates
│   ├── why-issues-are-not-specs.md   ← the detailed reasoning
│   ├── sdd-guide.md                  ← the harness tiers + LLM cost model
│   ├── constitution.md               ← durable engineering principles (always respected)
│   ├── architecture.md               ← internal structure the BA doesn't know
│   └── domain-glossary.md            ← business terms → code terms
└── specs/
    └── CRSU-1234-refund-eligibility/ ← a fully worked example
        ├── 00-source-issue.md        ← the vague BA issue (input)
        ├── spec.md                   ← the detailed, locked spec (output of /specify)
        ├── plan.md                   ← the technical design (output of /plan)
        ├── tasks.md                  ← the work breakdown (output of /tasks)
        └── contracts/
            └── refund-eligibility.openapi.yaml
```

## How to use it

1. **Copy** `.github/`, `AGENTS.md`, `.gitlab/issue_templates/`, and the `docs/` you want into
   your service repo.
2. **Tune** `AGENTS.md` and the instruction files to your service's stack and rules. Keep
   `AGENTS.md` + `copilot-instructions.md` together under ~2 KB.
3. **BA raises** a requirement using the GitLab issue template → `CRSU-####`.
4. **Run the flow:** `/specify CRSU-####` → review & lock `spec.md` → `/plan` → `/tasks` →
   `/implement` (one task per run) → MR → close the issue.

Read the worked example end-to-end:
[`specs/CRSU-1234-refund-eligibility/`](specs/CRSU-1234-refund-eligibility/).

## Adapting the CRSU key
`CRSU` is used here as the example GitLab project key prefix (`CRSU-1234`). Swap it for your
team's actual key throughout the issue template and spec folder names.

## Note on Spec Kit
GitHub's [Spec Kit](https://github.com/github/spec-kit) ships `/specify`, `/plan`, `/tasks`,
`/implement` pre-built. In a regulated environment you'll likely vendor and review it rather
than pull it live, but it's worth evaluating instead of hand-rolling the prompt files.
