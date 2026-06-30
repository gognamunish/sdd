_[← Guidelines home](home)_

# Adoption checklist

For a team starting SDD on a service:

- [ ] Copy the assistant config: `AGENTS.md`, `.github/instructions/`, `.github/prompts/`.
- [ ] Tune `AGENTS.md` to the service (build / test commands, the non-negotiable rules). Keep it
      lean — see the [Context & cost model](context-and-cost).
- [ ] Add the requirement **ticket template** to the issue tracker.
- [ ] Add project context the spec step needs: principles, an architecture note, a domain glossary.
- [ ] Create `specs/` and run one requirement end-to-end as a reference
      (`specify → plan → tasks → implement`).
- [ ] Agree the **gates** with the team: who signs off the spec, who reviews the plan, who merges.
      See [Quality gates & DoD](quality-gates).
- [ ] Review the always-on context size before rolling out to everyone.
