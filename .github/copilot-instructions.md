Follow the conventions in /AGENTS.md.

Language- and area-specific rules load automatically when matching files are open:
- Python: .github/instructions/python.instructions.md
- Tests:  .github/instructions/tests.instructions.md
- API:    .github/instructions/api.instructions.md

This repo uses spec-driven development — see docs/workflow.md.
Requirements originate as GitLab CRSU-#### issues, but a raw issue is NOT the
build spec. The committed specs/<KEY-slug>/spec.md is the source of truth.
See docs/why-issues-are-not-specs.md.

Order of work: spec.md (what) -> plan.md (how) -> tasks.md (steps) -> implement.
