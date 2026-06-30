---
mode: agent
description: Generate plan.md (technical design) from a locked spec.md
---
Read the `spec.md` in the target `specs/<KEY-slug>/` folder. It must be reviewed and locked
before you plan against it — if it still has open questions, stop and surface them.

Produce `plan.md` in the same folder, covering:
- **Components touched** — which modules/layers (api / domain / adapters) change, and how.
- **Data model** — entities, fields, and any migration.
- **API & events** — write the contract into `contracts/` (OpenAPI / schema). Versioned paths.
- **Key decisions** — each as a short "Decision / Options / Choice / Why" block (lightweight ADR).
  Capture genuinely contestable choices (e.g. 200-with-reasons vs 4xx, where a rule lives).
- **Risks & dependencies** — external services, rollout/rollback note.

Stay consistent with AGENTS.md, docs/constitution.md, docs/architecture.md and the instruction
files. Do not write implementation code. Flag anything in the spec that is untestable or unclear.
