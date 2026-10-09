---
name: create-spec
description: Write or revise a software feature specification from requirements, notes, or an existing design.
---

# Create spec

Read the user's input and the existing spec and project context needed to establish the design. Consult [references/spec-template.md](references/spec-template.md) only when a section outline would help; it is a menu, not a mandatory form.

Capture the problem, goals, scope, behavior, design decisions and rationale, failure cases, verification criteria, and unresolved questions. Preserve precise facts and distinguish confirmed requirements from assumptions. Surface contradictions rather than silently choosing one.

Write at the requested path, or `SPEC.md` in the project root. When updating, make a focused edit and preserve unrelated content. If the target exists but the user has not asked to update it, ask before replacing it.

Do not invent requirements, performance targets, or coverage thresholds to fill a template. Ask only when a missing decision blocks a useful spec; otherwise record it as an open question. Producing a spec does not authorize implementation; continue to a requested plan or implementation only within the user's existing scope.
