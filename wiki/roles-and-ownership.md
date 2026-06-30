_[← Guidelines home](home)_

# Roles & ownership

One table so "who owns this step?" has a single authoritative answer. **A** = Accountable
(one per step), **R** = Responsible (does the work), **C** = Consulted, **I** = Informed.

| Step | Business Analyst | Engineer | Tech Lead / Architect | AI Assistant | Reviewers (peers) | Risk / Compliance |
|---|---|---|---|---|---|---|
| Requirement | **A/R** | C | I | – | – | C |
| Specify (spec) | **C / approve** | **R** | A | assists | C | C (policy values) |
| Plan | I | R | **A/R** | assists | C | I |
| Tasks | I | **A/R** | C | assists | I | – |
| Implement | I | **A** | – | **R** | **R** (review) | – |
| Merge & close | I | A | C | – | **R** | I |

Key point: the **AI assistant is never Accountable**. It is Responsible for producing drafts and
code under a human who is Accountable for the result.

See [the workflow](workflow) for what each step produces.
