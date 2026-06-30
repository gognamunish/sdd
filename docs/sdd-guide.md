# SDD guide: the harness, the files, and the cost model

How GitHub Copilot assembles context, which files do what, and how to keep the LLM cost low.
This is the conceptual background; `docs/workflow.md` is the day-to-day process.

## 1. What is actually in the prompt ("the harness")

Every Copilot request is assembled in layers. Think of three tiers by **when** they're included.

### Tier 0 — sent on *every* request (the always-on tax)
- GitHub's own system prompt (you don't control it).
- `.github/copilot-instructions.md` — repo-wide, injected into every request.
- The nearest `AGENTS.md` to the file being worked on.
- The user's message + the open file / selection / explicitly attached context.

### Tier 1 — sent *conditionally*, only when relevant files are in play
- `.github/instructions/*.instructions.md`. Each has an `applyTo:` glob; pulled in only when a
  matching file is in context. `python.instructions.md` costs nothing while editing YAML.

### Tier 2 — sent *only on demand*
- `.github/prompts/*.prompt.md` — slash commands, loaded only when invoked (e.g. `/specify`).
- Files the agent chooses to read, and (via the GitLab MCP) the issue it fetches.

> **The cost rule:** Tier 0 is multiplied by *every request × every developer × every day*.
> A bloated `copilot-instructions.md` or `AGENTS.md` is the most expensive thing in the repo.
> Keep Tier 0 lean; push detail into Tier 1/2 where it loads only when needed.

## 2. AGENTS.md vs copilot-instructions.md

| File | Who reads it | Notes |
|---|---|---|
| `AGENTS.md` | Cross-tool open standard — Copilot, the IDE, and other agents | Vendor-neutral; make this canonical |
| `.github/copilot-instructions.md` | GitHub Copilot specifically | Keep it a short pointer to AGENTS.md |

Don't duplicate content between them — you'd pay for it twice and they'd drift. In this repo
`copilot-instructions.md` is three lines pointing at `AGENTS.md` and the instruction files.

## 3. Where the spec artifacts sit (cost angle)

The feature spec is split into **three files** (`spec.md` / `plan.md` / `tasks.md`) on purpose:
during implementation you attach `tasks.md` + the code, **not** the whole history. Each phase
loads only what it needs.

The GitLab issue is **not** loaded on every request — it's pulled once, by `/specify`, then
distilled into the committed `spec.md`. That keeps the noisy issue payload (comments, metadata)
out of the recurring context. See `why-issues-are-not-specs.md`.

## 4. Cost checklist

| Lever | Do this | Why |
|---|---|---|
| Shrink Tier 0 | `AGENTS.md` + `copilot-instructions.md` together < ~2 KB | Multiplied by every request × every dev |
| Push detail to Tier 1 | Language/test/API rules → `*.instructions.md` with tight `applyTo` | A non-Python edit never loads Python rules |
| Workflows to Tier 2 | Spec/plan/task generation → `prompt.md` files | Zero cost until someone runs `/plan` |
| Split the spec | `spec.md` / `plan.md` / `tasks.md`, not one mega-doc | Implementation attaches only `tasks.md` + code |
| Issue stays upstream | Fetch once in `/specify`, commit `spec.md` | Keeps noisy issue text out of recurring context |
| Nearest-file scoping | Per-service `AGENTS.md` in a monorepo | Devs only pay for their service's context |
| No duplication | One canonical source (AGENTS.md); pointers elsewhere | Duplicated instructions = double tokens + drift |

> **Headline:** detail isn't the enemy — *always-loaded* detail is. Structure the repo so the
> model sees the right ~500 tokens at the right moment instead of 5,000 tokens every time.

## 5. Note on GitLab + Copilot

Copilot reads your **local workspace**, so `.github/copilot-instructions.md`, `AGENTS.md`, and
the instruction/prompt files all work in a **GitLab-hosted** repo — you don't need GitHub the
host, only the folder convention. Issues stay in GitLab; the GitLab MCP lets `/specify` pull
them into the spec step.

## 6. Worth evaluating: Spec Kit
GitHub's **Spec Kit** (`github/spec-kit`) ships `/specify`, `/plan`, `/tasks`, `/implement`
pre-built, plus a "constitution" concept. In a bank you'll likely **vendor and review** it
rather than pull it live, but it saves hand-rolling the prompt files.
