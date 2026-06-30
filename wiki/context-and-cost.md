_[← Guidelines home](home)_

# Context & cost model

Everything the assistant reads is sent to the model and **billed per request**. The cost of a file
is not its size alone — it is **size × how often it is loaded**.

## The three tiers

| Tier | Loaded… | Examples | Cost behaviour |
|---|---|---|---|
| **Always-on** | every request | `AGENTS.md`, `copilot-instructions.md`, the open file | Paid on **every** request × **every** developer. The dominant cost. |
| **Conditional** | when a matching file is in context | `*.instructions.md` (path-scoped) | Paid only when relevant. |
| **On demand** | when explicitly invoked / fetched | `prompts/*`, a fetched ticket, files the assistant reads | Paid once, when used. |

> **The cost rule:** a bloated always-on file is the most expensive thing in the repo. Keep
> always-on context lean; push detail down to conditional / on-demand.

## Cost levers (do / don't)

| Do | Don't | Why |
|---|---|---|
| Keep `AGENTS.md` + always-on instructions together under ~2 KB | Paste the full coding standard into `AGENTS.md` | Always-on is multiplied by every request × every dev |
| Scope detailed rules with file-path globs | Make every rule global | A non-matching edit then loads nothing irrelevant |
| Split the spec into `spec` / `plan` / `tasks` | Keep one giant spec doc | Implementation loads only `tasks` + code |
| Fetch the ticket **once** in the spec step and commit the result | Re-read the live ticket every stage | The ticket payload is noisy; commit the distilled signal |
| Attach only the files in play during implementation | Attach the whole spec folder every run | Smaller context = lower cost + fewer hallucinations |
| One task per implement run | Ask for the whole plan in one shot | Bounded runs are cheaper and more reliable |

> **Headline:** detail isn't the enemy — *always-loaded* detail is. Structure the repo so the
> assistant sees the right ~500 tokens at the right moment instead of 5,000 every time.

See [the assistant config](assistant-config) for how the files map onto these tiers.
