# The spec-driven workflow

Four stages, from a vague business requirement to merged code. Each stage has a clear
**owner**, a defined **input and output**, and a **gate** that must pass before the next
stage starts. Each artifact links back to the previous one for traceability.

```
 Stage 0            Stage 1              Stage 2            Stage 3           Stage 4
 ───────            ───────              ───────            ───────           ───────
 GitLab issue  ──►  spec.md         ──►  plan.md       ──►  tasks.md     ──►  code + tests
 CRSU-1234          (what / why)         (how)              (steps)            (implementation)

 BA owns            Eng + BA own         Architect/Eng own  Eng own            AI agent + Eng own
 business intent    precise contract     technical design   work breakdown     working software

 [template]         /specify             /plan              /tasks             /implement
                    (pulls issue via                                           (one task per run)
                     GitLab MCP)
        │                 ▲
        └── Source: ──────┘   spec.md header links back to CRSU-1234 (two-way traceability)
```

## Stage 0 — Requirement (Business Analyst)
- **Input:** a business need.
- **Action:** raise a GitLab issue `CRSU-####` using `.gitlab/issue_templates/Requirement.md`.
- **Output:** an issue with goal, functional + non-functional requirements, and acceptance
  criteria — at a **business altitude**. The BA is not expected to know the code.
- **Gate:** issue triaged and labelled `~requirement`.

## Stage 1 — Specify (Engineering + BA)  → `spec.md`
- **Input:** the `CRSU-####` issue, plus `docs/constitution.md`, `docs/architecture.md`,
  `docs/domain-glossary.md`.
- **Action:** run `/specify CRSU-1234`. The agent fetches the issue (GitLab MCP), reads the
  project context, and distils a **precise, testable** `specs/CRSU-1234-<slug>/spec.md`:
  EARS requirements, concrete NFRs, Gherkin acceptance criteria, reason codes.
- **Output:** `spec.md` with header `Source: CRSU-1234`.
- **Gate (the important one):** spec review MR. **BA signs off** "captures intent";
  **engineering signs off** "precise and testable". Then the spec is **locked**.
  → See [why the issue itself is not this artifact](why-issues-are-not-specs.md).

## Stage 2 — Plan (Architect / Engineering)  → `plan.md`
- **Input:** the locked `spec.md`.
- **Action:** run `/plan`. Produces `plan.md` (components, data model, decisions as mini-ADRs,
  risks) and the API/event contract under `contracts/`.
- **Output:** `plan.md` + `contracts/`.
- **Gate:** design review. No code yet.

## Stage 3 — Tasks (Engineering)  → `tasks.md`
- **Input:** `plan.md`.
- **Action:** run `/tasks`. Produces an ordered checklist of small, individually verifiable
  tasks, each naming the files it touches and how it is verified.
- **Output:** `tasks.md`.
- **Gate:** quick sanity check that every acceptance criterion has a covering task.

## Stage 4 — Implement (AI agent + Engineer)  → code + tests
- **Input:** `tasks.md`.
- **Action:** run `/implement` repeatedly — **one task per run**, each producing a small diff
  with passing tests, ticking the box in `tasks.md`.
- **Output:** working, tested code in a feature branch.
- **Gate:** MR review + CI green. Then **close `CRSU-1234`**.

## Definition of done for the whole chain
- `CRSU-1234` ↔ `spec.md` ↔ `plan.md` ↔ `tasks.md` ↔ MR all link to each other.
- Every Gherkin acceptance criterion in `spec.md` has a passing test.
- The issue is closed with a link to the merged MR.

A fully worked example of all four artifacts lives in
[`specs/CRSU-1234-refund-eligibility/`](../specs/CRSU-1234-refund-eligibility/).
