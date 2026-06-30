_[← Guidelines home](home)_

# FAQ

**Do we always need `tasks.md`?** No. It is the assistant's control surface and is high-value for
substantial features, but for a trivial change you can fold the task list into `plan.md`. Never drop
the *small-steps* principle. See [the workflow](workflow), Stage 3.

**What if the assistant can't read the ticket (no integration)?** Paste the ticket body into the
spec step manually. The committed spec is what matters; the fetch is only convenience.

**Can the BA write the spec directly?** They author the *ticket*. The *spec* needs engineering
knowledge (architecture, error model), so engineering writes it with the BA approving intent.

**Isn't writing specs slower?** It front-loads the thinking that you would otherwise pay for in
review churn, defects, and assistant rework. For non-trivial change it is faster end-to-end.

**One repo per service or a monorepo?** Either. In a monorepo, put a per-service `AGENTS.md` inside
each service folder so a developer only loads their service's always-on context.

**Can the assistant approve its own work?** No. Every gate is a human gate — see
[Roles & ownership](roles-and-ownership) and [Quality gates & DoD](quality-gates).
