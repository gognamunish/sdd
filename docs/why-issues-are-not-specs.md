# Why a GitLab/Jira issue is not the final spec

In this workflow a Business Analyst raises a requirement as a GitLab issue
(`CRSU-####`). That issue is the **origin** of a requirement — it is *not* the
artifact the AI agent builds from. We deliberately transform `CRSU-####` into a
committed `specs/<KEY-slug>/spec.md` and treat **that** as the source of truth.

This is a deliberate design decision, not bureaucracy. Here is the full reasoning.

## A useful analogy

> The issue is the **source requirement / ticket**. `spec.md` is the **compiled,
> reviewed design the build runs against**. You don't ship source comments; you
> ship the reviewed, version-pinned artifact. Same idea.

## The reasons

### 1. Audience and altitude mismatch
The BA writes intent at a **business altitude** and is, by design, not versed in
the code, internal services, or APIs. The agent needs a **precise, testable,
code-aware** specification. The gap between those two has to be filled by
engineering knowledge (architecture, domain rules, error model). *That filling-in
is the spec step.* If you point the agent straight at the issue, it fills the gap
by **guessing** — and guesses are where defects and hallucinations come from.

### 2. Immutability and reproducibility
Issues mutate constantly — descriptions get edited, comments pile up, scope drifts
in a thread. Code must be built against a **frozen, versioned** input. A git commit
is a provable record of *exactly what the code was built against*; an issue's live
state is not. Fetch `CRSU-1234` today versus next month and you may get different
text. `spec.md` committed to the branch is diffable, pinned, and reproducible.

### 3. There must be a review/approval gate
A raw issue is discussion; it has not been *approved as a contract*. `spec.md` goes
through a merge-request review where the **BA signs off** ("this captures my
intent") and **engineering signs off** ("this is precise and testable"). That dual
sign-off is the moment the requirement becomes buildable. The issue thread never has
a single, agreed, point-in-time "this is it".

### 4. Traceability and audit (this matters more in a bank)
A committed `spec.md` carrying `Source: CRSU-1234` gives **two-way traceability**:
issue → spec → plan → tasks → commit → MR, and back. An auditor or regulator can
walk that chain. "The agent read the issue at some point" is not auditable; "this
commit was built from this reviewed spec, derived from this issue" is.

### 5. Context cost and signal-to-noise
An issue payload is **noisy** — labels, weight, time-tracking, forty comments, half
of them obsolete. Feeding that to the model on every `/plan` and `/implement` is
tokens spent on noise (and tokens are billed per request, per developer). `spec.md`
is **curated signal**: the agent reads a stable, lean artifact cheaply and
repeatably. (See `docs/sdd-guide.md` for the cost model.)

### 6. Determinism of the downstream stages
`plan.md` and `tasks.md` are derived from the spec. If they derive from a **live,
editable issue**, then someone editing the issue mid-sprint silently desyncs the
plan, the tasks, and the code from what was actually reviewed. Pinning the spec
makes the whole chain deterministic.

### 7. Tool independence and longevity
The spec lives **with the code**, in the repo. It survives an issue-tracker
migration (Jira → GitLab → whatever comes next), a project re-key, or an issue being
closed and archived. Requirements that live only in a tracker are hostage to that
tracker.

### 8. Separation of duties and ownership
The BA owns the **what and why** (the issue). Engineering owns the **how** (the
spec, plan, and code). Keeping these in separate artifacts keeps accountability
clear. Collapsing them into one editable issue blurs who approved what.

## "But we have a GitLab MCP — the agent *can* read the issue"

Yes, and that is genuinely useful — but it changes the **mechanism, not the
conclusion**. The MCP is the *ingestion pipe*: it lets `/specify` pull `CRSU-1234`
in automatically instead of someone copy-pasting it. It does **not** give you
immutability, a review gate, audit traceability, low-noise context, or downstream
determinism. So we use the MCP to *feed* the spec step, and we still **commit
`spec.md`** as the reviewed source of truth.

> Live-fetching the issue replaces the copy-paste. It does not replace the
> committed, reviewed artifact.

## One-line summary
**The issue captures what the business wants; `spec.md` is the engineering-grade,
reviewed, version-pinned contract the AI agent and the build actually run against.**
