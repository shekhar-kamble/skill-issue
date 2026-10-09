# Spec section menu

Choose sections that help a reader build or assess this design. A small feature may need only the problem, behavior, decisions, and success criteria. Do not inspect every category as a ritual or invent requirements to fill the outline.

| Section | Include when useful |
| --- | --- |
| Summary and background | Problem, intended users, existing behavior, and why the change matters. |
| Goals and scope | Observable outcomes, affected boundaries, and confirmed exclusions. |
| Behavior and design | User flow, states, component responsibilities, decisions, and rationale. |
| Data and contracts | Types, constraints, API inputs/outputs, authorization, serialization, and persistence changes. |
| Failure handling | Relevant invalid inputs, races, retries, partial failures, and recovery. |
| Security and privacy | Concrete data sensitivity, trust boundaries, permissions, and safeguards. |
| Delivery | Migration dependencies, rollout order, compatibility, and rollback limits. |
| Verification and success | Observable acceptance criteria and proportionate checks permitted by project rules. |
| Open questions | Unresolved decisions, consequential assumptions, and what evidence would resolve them. |

For a substantial feature, this is one possible starting point:

```markdown
# Spec: <feature>

## Problem and goals
<Existing behavior, desired outcome, and confirmed scope.>

## Proposed behavior and design
<Flows, contracts, ownership, key decisions, and rationale.>

## Failure and compatibility considerations
<Relevant failure cases, existing consumers, and delivery constraints.>

## Success criteria and verification
<Observable outcomes and proposed checks.>

## Open questions
<Unresolved decisions and assumptions.>
```

Keep requirements distinct from examples and proposed implementation. Use performance targets or coverage thresholds only when provided or justified by evidence. Record important decisions where they help the design; a separate decision log is optional. Proposed verification does not imply checks were executed or authorize test runs.
