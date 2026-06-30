_[← Guidelines home](home)_

# Why the ticket is not the spec

A recurring question: *if the assistant can read the ticket, why write a separate spec?* Because
the ticket is the **origin of intent**, not the **build contract**. We distil it into a committed
`spec.md` and build from that, for these reasons:

1. **Audience & altitude.** The BA writes business intent and is not versed in the code. The
   assistant needs code-aware precision. Closing that gap *is* the spec; skip it and the gap is
   closed by guessing.
2. **Immutability & reproducibility.** Tickets mutate — descriptions get edited, comments pile up.
   Code must be built against a **frozen, version-pinned** input. A commit proves exactly what the
   code was built from; a live ticket does not.
3. **A review/approval gate.** A ticket thread is discussion, not an approved contract. The spec
   gets an explicit dual sign-off (BA: "captures intent", Eng: "precise & testable").
4. **Audit & traceability.** A committed spec carrying `Source: <ticket>` gives two-way
   traceability that an auditor can walk. "The assistant read the ticket once" is not auditable.
5. **Context cost & signal.** A ticket payload is noisy (labels, metadata, dozens of comments).
   A curated spec is lean signal the assistant reads cheaply and repeatably. See the
   [Context & cost model](context-and-cost).
6. **Downstream determinism.** Plan and tasks derive from the spec. If they derive from a live,
   editable ticket, an edit mid-sprint silently desyncs the plan, tasks, and code from what was
   reviewed.
7. **Tool independence.** The spec lives with the code and survives a tracker migration, a project
   re-key, or the ticket being archived.

> **"But our assistant can fetch the ticket automatically."** That changes the *mechanism*, not the
> conclusion. Automatic fetch replaces the copy-paste into the spec step — it does **not** give you
> immutability, a review gate, audit traceability, or low-noise context. So we use the fetch to
> *feed* the spec step, and still **commit the spec** as the reviewed source of truth.

**One line:** *the ticket captures what the business wants; the spec is the engineering-grade,
reviewed, version-pinned contract the assistant and the build actually run against.*
